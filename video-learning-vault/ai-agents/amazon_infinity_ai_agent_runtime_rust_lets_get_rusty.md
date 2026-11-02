# Amazon Infinity: AI Agent Runtime Viết Bằng Rust Cho Cực Hạn Đồng Thời (75,000 Agents Trên Raspberry Pi) | Let's Get Rusty

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=os5eNeAVRpc)
- **Video ID:** `os5eNeAVRpc`
- **Category:** #ai-agents
- **Date Processed:** 2026-10-20
- **Duration:** 07:06
- **Speaker:** Bogdan (Let's Get Rusty) — Phân tích bài công bố chính thức của Amazon AWS về dự án Infinity (Massively Concurrent Agent Runtime in Rust)
- **Transcript File:** [amazon_infinity_ai_agent_runtime_rust_lets_get_rusty_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/amazon_infinity_ai_agent_runtime_rust_lets_get_rusty_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Đội ngũ kỹ thuật của Amazon Web Services (AWS) vừa công bố mã nguồn mở một framework mang tính cách mạng cho hệ thống tác nhân AI: **Infinity** — một runtime được viết hoàn toàn bằng **Rust**, thiết kế chuyên biệt cho bài toán **đồng thời cực hạn (Extreme Agent Concurrency)**.

Thông số benchmark ấn tượng nhất mà AWS đưa ra: **Infinity có thể vận hành đồng thời 75,000 AI agents trên bộ nhớ vỏn vẹn 8 GB của một chiếc máy tính mini Raspberry Pi** (~100 KB footprint cho mỗi agent ở trạng thái chờ).

Lý do đằng sau kỳ tích kỹ thuật này xuất phát từ một nhận thức mang tính bản chất về hành vi của AI Agent trong thế giới thực: **"Agents spend most of their lives waiting" (Các tác nhân AI dành phần lớn cuộc đời của chúng chỉ để chờ đợi)**:
- Chờ bản build CI/CD chạy mất 15–20 phút.
- Chờ Webhook từ hệ thống bên thứ ba phản hồi sau vài giờ.
- Chờ con người kiểm duyệt (Human-in-the-loop approval) mất nửa ngày hoặc nhiều ngày.

Các framework agent truyền thống (phần lớn viết bằng Python như LangChain, CrewAI, AutoGen) duy trì mô hình **1 Process / 1 Thread cho mỗi Agent** và giữ tiến trình đó chạy liên tục trong RAM để chờ kết quả. Đây là một sự lãng phí tài nguyên khủng khiếp. 

AWS Infinity giải quyết triệt để vấn đề này bằng cách mang tư duy **Async/Await State Machine của Rust** lên tầng kiến trúc phân tán: chia nhỏ vòng đời agent thành các **Execution Slices (Lát cắt thực thi)**, đưa công cụ ra ngoài qua giao thức **RAP (Reactive Agent Protocol)**, và áp dụng nguyên lý **Synthetic Tool Calls** để đưa tài nguyên agent về mức **Zero Memory / Zero CPU** khi đang chờ đợi.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                    TRADITIONAL PROCESS-PER-AGENT VS. AWS INFINITY SLICES                    │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  TRADITIONAL MODEL (LangChain / CrewAI / Python Daemons):                                    │
│  [Process A: 150MB RAM] ──(Gọi Tool)──> [CHỜ BUILD 20 PHÚT: Vẫn ngốn 150MB RAM & OS Thread]  │
│  [Process B: 150MB RAM] ──(Gọi Duyệt)─> [CHỜ USER 2 NGÀY: Vẫn ngốn 150MB RAM & OS Thread]   │
│  ➔ HẬU QUẢ: Cạn kiệt RAM/Thread khi scale lên hàng nghìn Agents, rò rỉ bộ nhớ khi chờ đợi. │
│                                                                                             │
│  AWS INFINITY MODEL (Rust Sliced Execution + RAP Server):                                    │
│                                                                                             │
│   Incoming Event       EXECUTION SLICE (Vài giây)          HIBERNATION / SCALE TO ZERO      │
│   ┌────────────┐      ┌───────────────────────────┐         ┌─────────────────────────┐     │
│   │Input Queue ├─────►│ 1. Load State từ Durable  │         │ Agent Process TẮT HẲN!  │     │
│   │(SQS/Redis) │      │ 2. Pre-process input      │         │ 0 MB RAM in Memory      │     │
│   └────────────┘      │ 3. Call LLM (Bedrock/...) │         │ 0% CPU Overhead         │     │
│                       │ 4. Lưu State vào Storage  │         └────────────▲────────────┘     │
│                       │ 5. Bắn Tool sang RAP Serv │                      │ Chờ vài giờ      │
│                       └─────────────┬─────────────┘                      │ hoặc vài ngày    │
│                                     │ Trigger Async Execution            │                  │
│                                     ▼                                    │                  │
│                       ┌───────────────────────────┐                      │                  │
│                       │ RAP Tool Server (Worker)  ├──────────────────────┘                  │
│                       │ - Chạy build/gửi webhook  │ (Sau khi hoàn tất: PUSH Event           │
│                       │ - Khi xong: gửi message   │  ngược vào Input Queue để mở            │
│                       └─────────────┬─────────────┘  Slice tiếp theo)                       │
│                                     │                                                       │
│                                     ▼                                                       │
│                              Input Queue (Event: Tool_Result_injected)                      │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Căn Bệnh "Thuế Chờ Đợi" (Idle Waiting Tax) Của Kiến Trúc Agent Hiện Nay

Trong các hệ sinh thái AI Agent hiện nay (chủ yếu được xây dựng trên nền Python), khi một tác nhân thực hiện một nhiệm vụ phức tạp, nó liên tục rơi vào trạng thái chờ I/O:
1. **Tool Execution Latency:** Đọc một codebase lớn, thực hiện web scraping, gọi database queries, chạy test suite, kích hoạt deployment pipeline.
2. **Asynchronous External Events:** Chờ webhook thanh toán Stripe, chờ GitHub Action hoàn thành test, chờ đối tác phản hồi API.
3. **Human Feedback Loop:** Chờ người quản trị xác nhận lệnh xoá database, phê duyệt PR hoặc chọn phương án thiết kế.

### Hậu Quả Kỹ Thuật Khi Dùng Mô Hình Tiến Trình Giữ Chỗ (Process-per-agent):
- **Bộ nhớ chết (Dead Memory Bloat):** Mỗi tiến trình Python agent thường tiêu tốn từ 50MB đến 200MB RAM chỉ để nạp interpreter, dependencies, prompt templates và in-memory vector cache. Nếu bạn có 10,000 agents đang phục vụ khách hàng nhưng 9,900 agents đang "ngủ" chờ khách nhập tin nhắn, hệ thống vẫn phải cấp phát gần **1.5 TB đến 2 TB RAM** chỉ để giữ các tiến trình đó sống!
- **Cạn kiệt File Descriptors & Sockets:** Các tiến trình giữ kết nối HTTP/gRPC và database pool mở liên tục trong lúc không làm gì, dẫn tới cạn kiệt tài nguyên mạng của hệ điều hành.
- **Rủi ro Crash & Mất dữ liệu:** Nếu một node máy chủ bị restart hoặc crash trong lúc agent đang `time.sleep()` hoặc `await` một webhook, toàn bộ in-process state machine của agent đó tan biến, khiến quy trình dang dở không thể phục hồi.

---

## 3. Kiến Trúc Lõi Của AWS Infinity — 4 Trụ Cột Kỹ Thuật

AWS Infinity giải quyết bài toán trên bằng cách tái cấu trúc toàn diện tầng runtime với 4 trụ cột kỹ thuật hệ thống:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                       AWS INFINITY 4 CORE ARCHITECTURAL PILLARS                             │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│  [Trụ Cột 1: Execution Slices]         [Trụ Cột 2: Async Future State Machine]              │
│  - Thực thi theo lát cắt hữu hạn       - Tương tự `async fn` trong Rust                     │
│  - Không chạy daemon vô tận            - Điểm `.await` chính là điểm gọi Tool Call          │
│  - Xong một bước là tắt runtime        - Trạng thái được tuần tự hoá ra Storage bền vững   │
│                                                                                             │
│  [Trụ Cột 3: RAP & Synthetic Messages] [Trụ Cột 4: Scale to Zero / Serverless Ready]        │
│  - Reactive Agent Protocol Server      - Không tốn một byte RAM nào khi agent nhàn rỗi      │
│  - "Everything is a Message"           - Chạy hoàn hảo trên AWS Lambda / ECS Fargate        │
│  - Sự kiện injected như tool call thật - Đạt kỷ lục 75,000 agents trên 8GB RAM              │
│                                                                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

### Trụ Cột 1: Vòng Đời Lát Cắt Thực Thi (Execution Slices)

Một Agent trong Infinity **không bao giờ là một process chạy liên tục**. Nó chỉ tồn tại trong các "Lát cắt" (Slice) có thời gian sống ngắn (vài trăm mili-giây đến vài giây).

Một Slice được khởi động theo chu trình nghiêm ngặt:
1. **Trigger từ Input Queue:** Một message mới xuất hiện trong hàng đợi (có thể là tin nhắn của user, kết quả của tool, hoặc một event từ bên ngoài).
2. **Hydration từ Durable Storage:** Runtime tải lịch sử hội thoại và context của thread từ kho lưu trữ bền vững (DynamoDB, S3, PostgreSQL).
3. **Pre-processing:** Kiểm tra schema, lọc token, nạp prompt.
4. **Invoke Model Provider:** Gửi request đến LLM (Claude trên AWS Bedrock, OpenAI,...). Model trả về: Text phản hồi, Reasoning tokens, hoặc một **Tool Call**.
5. **Persist State:** Ghi nhận trạng thái mới nhất vào Durable Storage.
6. **Dispatch & Shutdown:** Nếu model yêu cầu gọi Tool, runtime bắn một lệnh POST không đồng bộ tới **RAP Tool Server** rồi **ngay lập tức chấm dứt lát cắt (terminate slice)**. Runtime giải phóng toàn bộ RAM và CPU của agent đó.

### Trụ Cột 2: Ánh Xạ Tư Duy Rust Async Await Vào Hệ Thống Phân Tán

Trong ngôn ngữ Rust:
- Một hàm `async fn` thực chất được trình biên dịch chuyển đổi thành một **State Machine (Máy trạng thái)** hiện thực trait `Future`.
- Mỗi khi gặp từ khoá `.await`, hàm sẽ tạm dừng (yield) trả quyền điều khiển lại cho Executor (Tokio), cho phép thread đó chạy các task khác.
- Khi I/O hoàn tất, Executor đánh thức (wake) state machine dậy và tiếp tục thực thi chính xác từ điểm đã dừng.

AWS Infinity mang chính xác tư duy này áp dụng cho cấp độ hệ thống phân tán:
- **Hàm Async** = Toàn bộ quy trình giải quyết tác vụ của Agent.
- **Điểm `.await`** = Lời gọi Tool Call (chờ API, chờ build, chờ người duyệt).
- **Lưu State Machine** = Thay vì giữ trong con trỏ bộ nhớ (RAM pointer) của tiến trình, Infinity lưu cấu trúc state machine này xuống **Durable Storage**.
- **Waker** = Khi RAP Server chạy xong tool, nó gửi message vào Input Queue đóng vai trò như Waker, kích hoạt một slice mới để tiếp tục hành trình của agent.

### Trụ Cột 3: Giao Thức RAP & Nguyên Lý "Everything is a Message"

Khi thiết kế hệ thống sự kiện cho Agent, các kỹ sư thường mắc 2 sai lầm:
1. **Tạo ra Event System riêng biệt:** Dẫn đến việc mô hình LLM không hiểu thứ tự thời gian giữa các sự kiện bên ngoài và luồng chat.
2. **Đẩy sự kiện vào vai trò `user` message:** Khiến LLM hiểu lầm rằng chính người dùng gõ câu thông báo đó, gây sai lệch ngữ cảnh và ảo giác phân quyền.

Infinity giải quyết vấn đề này bằng triết lý **"Everything is a Message"** thông qua **Synthetic Tool Calls (Lời gọi công cụ tổng hợp)**:
- Mọi tương tác đều tuân thủ đúng 3 vai trò gốc mà LLM hiểu: `system`, `user`, và `tool`.
- Khi agent muốn tạo một tác vụ con (sub-thread), nó gọi công cụ chuẩn:
  ```json
  {
    "tool": "spawn_thread",
    "parameters": {
      "task": "Review auth.ts for security vulnerabilities"
    }
  }
  ```
- Khi tác vụ con đó hoàn thành (dù sau 30 phút hay 3 tiếng), RAP Server sẽ đóng gói kết quả và gửi trả lại luồng chính dưới dạng một **Synthetic Tool Call** được đánh dấu hậu tố đặc biệt:
  ```json
  {
    "tool": "spawn_thread_injected",
    "result": "Found SQL injection vulnerability on line 42..."
  }
  ```
- **Ý nghĩa sống còn:** Bằng cách biến mọi sự kiện bất đồng bộ thành cặp `tool_call` và `tool_result_injected`, Infinity **hoàn toàn không cần bất kỳ in-process state machine nào** để theo dõi trạng thái chờ! Lịch sử hội thoại chính là trạng thái duy nhất.

### Trụ Cột 4: Scale to Zero & Serverless Native

Nhờ thiết kế Slices và Durable State:
- Một Agent đang chờ người duyệt trong 5 ngày **tiêu thụ đúng 0 byte RAM và 0 chu kỳ CPU**.
- Chi phí duy nhất là dung lượng lưu trữ vài chục KB JSON trong cơ sở dữ liệu.
- Runtime có thể triển khai hoàn hảo trên các nền tảng **Serverless** như AWS Lambda:
  - Message vào SQS -> Kích hoạt Lambda chạy 1 Slice (2 giây) -> Lưu DB -> Lambda kết thúc.
  - Không cần duy trì cụm máy chủ EC2 hay Kubernetes đắt đỏ chạy 24/7 chỉ để đợi tin nhắn của người dùng.

---

## 4. Bảng So Sánh Đối Chiếu Hệ Thống: Python Frameworks vs. AWS Infinity (Rust)

| Tiêu Chí So Sánh | Frameworks Truyền Thống (LangChain, CrewAI, AutoGen) | AWS Infinity Runtime (Rust) |
| :--- | :--- | :--- |
| **Ngôn ngữ lõi** | Python (GIL, dynamic memory allocations) | Rust (Zero-cost abstractions, Memory safety) |
| **Mô hình luồng** | Long-running Process / Thread per Agent | Stateless Ephemeral Execution Slices |
| **Mức tiêu thụ RAM khi chờ** | **50MB – 200MB / Agent** (Giữ full context trong RAM) | **0 MB** (Toàn bộ tiến trình tắt hoàn toàn) |
| **Khả năng đồng thời** | Vài trăm đến tối đa vài nghìn agents trên cụm server lớn | **75,000 agents trên 8GB RAM Raspberry Pi** |
| **Xử lý tác vụ chờ dài hạn** | Block luồng hoặc dùng async loop dễ đứt gãy nếu crash | Hibernation tự nhiên; Tool chạy trên RAP Server |
| **Phục hồi sự cố (Crash Recovery)** | Khó khăn, dễ mất state nếu tiến trình chết giữa chừng | Deterministic Recovery từ Durable Storage 100% |
| **Tương thích Serverless** | Kém (Yêu cầu runtime chạy nền liên tục) | Hoàn hảo (Thiết kế đo ni đóng giày cho AWS Lambda) |
| **Cơ chế sự kiện bất đồng bộ** | Custom event bus hoặc Polling vòng lặp | Native Synthetic Tool Calls (`_injected`) |

---

## 5. Ứng Dụng Thực Tiễn & Tác Động Tới Kỹ Sư Hệ Thống

Kiến trúc của AWS Infinity mở ra kỷ nguyên ứng dụng AI Agent ở quy mô công nghiệp:

1. **Hệ Thống Chăm Sóc Khách Hàng Siêu Quy Mô (Customer Support at Massive Scale):**
   - Một sàn thương mại điện tử phục vụ hàng triệu người dùng cùng lúc. Mỗi khách hàng có thể hỏi một câu rồi biến mất 15 phút mới trả lời tiếp.
   - Với Infinity, hàng triệu agent có thể được tạo ra mà không làm sập hạ tầng, vì 99% thời gian chúng không tốn tài nguyên máy chủ.

2. **Hệ Thống Tự Động Hoá Di Chuyển Mã Nguồn Lớn (Enterprise Codebase Migration):**
   - Hàng chục ngàn sub-agents được kích hoạt để refactor từng module mã nguồn độc lập. Mỗi agent có thể đợi build test hoặc bảo mật hàng giờ mà không gây cạn kiệt tài nguyên cluster.

3. **Bài Học Cho Kỹ Sư Xây Dựng Agentic Systems:**
   - **Tách rời Thực thi suy luận (Reasoning) khỏi Thực thi công cụ (Tool Execution):** Đừng bao giờ cho agent process trực tiếp chạy những lệnh tốn thời gian. Hãy bắn lệnh sang một worker queue (như RAP Server) và cho agent đi ngủ.
   - **Tận dụng LLM-native formats:** Đừng cố phát minh ra các giao thức chat kỳ quái. Hãy tận dụng cơ chế `tool_call` chuẩn mực của LLM để thông báo sự kiện bất đồng bộ.
   - **Rust là tương lai của Agent Infrastructure:** Trong khi Python vẫn thống trị ở tầng thử nghiệm và nghiên cứu mô hình (research/prototyping), tầng hạ tầng vận hành (runtime infrastructure/orchestrator) quy mô lớn bắt buộc phải chuyển dịch sang Rust để đảm bảo hiệu năng và chi phí.

---

## 6. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

- [Báo Cáo Anthropic: Gộp Sub-Agents Vào Một Vòng Lặp Duy Nhất](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/claude_code_getting_rid_of_subagents_aicodeking.md) — Phân tích sự tối giản hóa kiến trúc agent và loại bỏ sự cồng kềnh của mô hình multi-agent truyền thống.
- [DeepSeek SuperAgent Harness Hands-On](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_superagent_harness_hands_on_aicodeking.md) — Kiến trúc Harness và tối ưu hoá chi phí vòng lặp tự động.
- [High-Throughput Agentic Engineering (Kun Chen)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/high_throughput_agentic_engineering_kun_chen.md) — Kỹ thuật kỹ nghệ luồng thông lượng cao cho AI Agents.
