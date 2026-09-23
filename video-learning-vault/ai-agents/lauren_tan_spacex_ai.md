# How I Shipped 1,000+ PRs a Month: The Trust Curve, Verification First & Dune Architecture | Lauren Tan (Cursor / SpaceX)

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=EWSUvEyFwjc)
- **Watch Skill ID:** `b30f721a93ea5958`
- **Category:** #ai-agents
- **Date Processed:** 2026-09-24
- **Duration:** 55:04
- **Speaker:** Lauren Tan (Engineer at Cursor / Anysphere, ex-SpaceX, ex-Meta; author of Pstack)
- **Transcript File:** [lauren_tan_spacex_ai_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/lauren_tan_spacex_ai_transcript.txt)

---

## 1. Bối cảnh & Tốc độ Vận hành Phi thực tế (1,000+ PRs/tháng)

Trong buổi chia sẻ trực tiếp, **Lauren Tan** trình bày hành trình thực chiến từ lúc mới gia nhập Cursor (Anysphere) 5 tháng trước cho đến khi đạt tốc độ ship mã nguồn đáng kinh ngạc:
- **Tháng trước:** Merge hơn **1,000 pull requests**.
- **Tháng hiện tại (chỉ mới đến ngày 12):** Đã merge gần **800 PRs**.
- **Auto-merging to main:** Thức dậy vào buổi sáng và thấy hơn 20 PRs do AI Agents tự động tạo, tự chạy kiểm thử và tự merge thẳng vào nhánh `main`, sau đó Lauren chỉ cần review trực tiếp trên `main`.

> [!WARNING]
> Lauren khẳng định: Con số 1,000 PRs không phải là "Vibe Coding Slop" (rác mã nguồn không kiểm soát). Để đạt được trạng thái này mà không làm sập ứng dụng production, hệ thống bắt buộc phải giải quyết bài toán cốt lõi: **Xây dựng lòng tin (The Trust Curve) dựa trên Xác thực tự động (Automated Verification) và Ràng buộc kiến trúc cực kỳ nghiêm ngặt (Hard Constraints).**

---

## 2. Đường cong Lòng tin (The Trust Curve) & Nghịch lý Quản trị Agent

### Quản trị Agent tương tự Quản lý Nhân sự Kỹ thuật
Khi một Tech Lead hay Engineering Manager quản lý một kỹ sư mới vào đội:
- Nếu **chưa tin tưởng**, người quản lý rơi vào cái bẫy **Micromanagement** (soi từng dòng code, đứng sau lưng giám sát, liên tục kiểm tra).
- Tương tự với AI Agent: Nếu kỹ sư không tin tưởng agent, họ sẽ phải dán mắt vào từng tool call, copy từng log lỗi vào chat, tranh cãi với agent. Khi đó, **chính con người trở thành điểm nghẽn (Human Bottleneck)** và không bao giờ có thể chạy song song (parallelize) hàng chục hay hàng trăm agent.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            THE TRUST CURVE (LAUREN TAN)                     │
├─────────────────────────────────────────────────────────────────────────────┤
│  Autonomy & Scale                                                           │
│     ▲                                                                       │
│     │                                                 [TỰ ĐỘNG MERGE]       │
│     │                                                 Overnight Auto-merge  │
│     │                                                 Cloud Agents (Benny)  │
│     │                                          ▲                            │
│     │                                         ╱                             │
│     │                                        ╱  Đầu tư Harness & Evals      │
│     │                                       ╱   Hard CI Constraints         │
│     │                                      ╱                                │
│     │                                     ╱                                 │
│     │                              ▲     ╱                                  │
│     │                             ╱     ╱                                   │
│     │                            ╱     ╱  Verification Skills               │
│     │                           ╱     ╱   (CDP, Memory Traces, Feature Map) │
│     │                          ╱                                            │
│     │  [MICROMANAGEMENT]      ╱                                             │
│     │  1-2 Agents in-the-loop                                               │
│     │  Human = Bottleneck                                                   │
│     └─────────────────────────────────────────────────────────────►         │
│        Tháng 1 (Mới bắt đầu)                 Tháng 5 (Tự chủ hoàn toàn)     │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Cách leo lên đỉnh đường cong lòng tin:
1. **Giai đoạn 1 (In-the-loop):** Quan sát cực kỳ tỉ mỉ các thất bại ngớ ngẩn của agent (đọc suy nghĩ thought blocks, quan sát tool calls).
2. **Giai đoạn 2 (Đóng gói tri thức thành Skill):** Mỗi khi agent thất bại vì phỏng đoán hay ảo giác, đóng gói quy trình kiểm tra thành **Skill** (Markdown hướng dẫn + CLI/Tooling).
3. **Giai đoạn 3 (Closed-Loop Verification):** Cung cấp công cụ để agent tự chạy, tự chụp trace, tự tương tác DOM thay vì bắt người làm thay.
4. **Giai đoạn 4 (Scaling Cloud Agents):** Đưa agent lên môi trường cloud chạy nền (như Benny agent), tự tái hiện bug và tự submit PR.

---

## 3. Chốt chặn số 1: Verification First & Kỹ thuật Feature Map

### Verification: Kỹ năng quan trọng nhất trong Hộp công cụ
- Nếu agent không có công cụ tự kiểm chứng (Verification Skill), người lập trình phải: mở ứng dụng dev lên, tự click thử, chụp màn hình, copy log lỗi paste vào cửa sổ prompt.
- **Verification Skill** cho phép agent:
  - Tự khởi động ứng dụng (CLI, Web, Electron app, hoặc iOS simulator).
  - Tự tương tác qua **Chrome DevTools Protocol (CDP)** hoặc Apple Simulator Automation CLI.
  - Tự lấy **CPU Traces**, **Heap Snapshots**, **Flame Graphs**, phát hiện memory leak hoặc long tasks (>16ms).
  - Tự xác nhận code đã chạy đúng trước khi báo hoàn thành.

### Sáng kiến Feature Map (Bản đồ Tính năng UI)
Khi xây dựng skill `control-glass` (Glass là tên nội bộ của cửa sổ Agent Window trong Cursor), Lauren gặp hiện tượng:
- Agent có công cụ CDP để tương tác với giao diện Electron, nhưng nó **hoàn toàn mù tịt về mặt nghiệp vụ UI**.
- Khi người dùng gửi báo cáo lỗi: *"Tab PR bên phải bị lag"* hoặc chỉ gửi 1 ảnh chụp màn hình kèm dấu `???`, agent không biết click vào đâu, tìm selector DOM nào, hay phím tắt gì để kích hoạt tính năng. Agent bắt đầu cào cấu vô định trong source code.
- **Giải pháp:** Xây dựng file `feature_map.md`:
  - Bản đồ hóa toàn bộ chức năng người dùng: phím tắt, đường dẫn UI, selector DOM cụ thể (`data-testid`, attributes), luồng kích hoạt.
  - Khi có bug report mơ hồ, agent tra cứu Feature Map để biết chính xác cách navigate đến view đó và kích hoạt kiểm thử tự động.

---

## 4. Hệ sinh thái Pstack ("Potato Stack") & Eval Playbook

**Pstack** (`potato stack`) là bộ skill và playbook mã nguồn mở do Lauren Tan phát triển (đặt tên hài hước đối xứng với GStack của Gary Tan - CEO Y Combinator):

1. **`create-verification-skill` & `maintain-verification-skill`:**
   - Hướng dẫn agent tự quét toàn bộ codebase dự án để sinh ra `feature_map.md` và mã điều khiển tự động hóa verification (CDP/CLI) cho dự án đó.
2. **`how` skill:**
   - Ngăn chặn triệt để thói quen xấu của agent: đoán mò "smoking gun" mà không thèm đọc file liên quan. Bắt buộc agent phải dùng sub-agents tra cứu code trước khi kết luận.
3. **Eval Playbook (Unit Test cho Agent Skills):**
   - Lauren sử dụng một **Coordinator Agent** lập ra Rubric (tiêu chí chấm điểm).
   - Tạo các thư mục cô lập được đặt tên trung tính (cleverly masked paths) để các sub-agents không nhận biết mình đang bị kiểm thử (tránh việc model thay đổi hành vi khi biết bị test).
   - Chạy kiểm thử song song kỹ năng trên nhiều mô hình nền tảng khác nhau (Claude 3.7 Sonnet, GPT-4o, Grok, v.v.).
   - Áp dụng kỹ thuật **Hill Climbing với `/loop`**: Yêu cầu agent tự sửa đổi tài liệu skill và prompt lặp lại liên tục cho đến khi rubric đạt điểm tuyệt đối 10/10.

---

## 5. Kiến trúc Dune: Kỷ luật Thép cho GrokBot và Electron

Một trong những phần giá trị nhất của bài nói là kiến trúc **Dune** được xây dựng cho **GrokBot** (ứng dụng desktop dạng chat/orchestration đa agent mới của Cursor):

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       DUNE ARCHITECTURE & 5 ENFORCEMENT LAYERS              │
├─────────────────────────────────────────────────────────────────────────────┤
│  Layer 1: Codebase Architecture                                             │
│  - Phân tách tuyệt đối giữa `electron-main` và `electron-renderer`.         │
│  - Mỗi tính năng cô lập 100% trong 1 thư mục con (Collocated feature).      │
│  - Nguyên lý cốt lõi: "The shortest path is the best path".                │
│                                                                             │
│  Layer 2: Hard Static Analysis & CI Enforcements (Hard Failure)             │
│  - CẤM TIỆT `useEffect` trong React (loại bỏ race conditions & re-renders). │
│  - CẤM VIẾT CODE COMMENTS: Agent thường giải thích lịch sử vô nghĩa vào    │
│    comment; comment bị cấm bởi linter để giữ code sạch và ngắn.            │
│  - Dependency Graph Linter: Báo đỏ CI ngay lập tức nếu Renderer vô tình    │
│    import module nặng từ Main Process (bảo vệ chuẩn 60 FPS / khung hình     │
│    16ms).                                                                   │
│                                                                             │
│  Layer 3: Compiler Diagnostics & Strict Typing                              │
│  - Tận dụng TypeScript strict hoặc Rust borrow-checker (nếu code compile     │
│    thì 90% là đúng logic).                                                  │
│                                                                             │
│  Layer 4: BugBot & Automated Code Review (CI level)                         │
│  - Bot kiểm duyệt tự động dựa trên quy tắc chuyên sâu của dự án.            │
│                                                                             │
│  Layer 5: Rules & Skills (Soft Constraints - Không được dựa dẫm hoàn toàn) │
│  - System prompts, `AGENTS.md`, style guide. Dễ bị agent "lãng quên".      │
└─────────────────────────────────────────────────────────────────────────────┘
```

> [!IMPORTANT]
> **Bài học xương máu về Code Review:**
> *"Mỗi khi một kỹ sư con người phải vào PR để bình luận: 'Đừng viết như thế này', đó chính là một CODE SMELL của tổ chức. Thay vì comment bằng tay, hãy biến nó ngay lập tức thành một Lint Rule, một CI Failure hoặc một thay đổi cấu trúc để triệt tiêu vĩnh viễn khả năng phạm lỗi."*

---

## 6. Sức mạnh Đột phá: Trao quyền cho PM, Designer & Non-Engineers

Nhờ kiến trúc Dune và hệ thống kiểm chứng tự động:
- **Agents hấp thụ toàn bộ sự phiền toái (Agent absorbs the annoyance):** Các quy tắc CI cực kỳ khó tính và khắt khe, nếu con người tự gõ code thì sẽ vô cùng ức chế, nhưng AI Agent không hề biết mệt mỏi hay phàn nàn.
- **Product Managers & Designers tự ship feature:** Vì ranh giới an toàn đã được mã hóa vào CI, PM hoặc Designer chỉ cần chat mô tả với agent trong GrokBot. Agent tự sửa code trong feature directory, CI tự kiểm tra, và PM có thể mở PR chuẩn chỉnh đến mức Lauren chỉ cần bấm duyệt (stamp) mà không sợ hồi quy hiệu năng (performance regression).

---

## 7. Tổng hợp Bài học Hành động cho Kỹ sư & Đội ngũ AI Native

| Hạng mục | Thực trạng Sai lầm | Tiêu chuẩn Lauren Tan / Cursor |
| :--- | :--- | :--- |
| **Kiểm thử** | Chờ con người click tay test thử hoặc chỉ đọc mắt | Agent tự khởi chạy qua CDP, tự chụp flame graph & heap snapshot |
| **Prompting** | Viết prompt dài giải thích toàn bộ ứng dụng | Cung cấp file `feature_map.md` với selector và keyboard shortcuts chuẩn |
| **Quy chuẩn Code**| Nhắc nhở qua tài liệu Styleguide hoặc PR review | Biến thành quy tắc Lint đỏ CI; cấm triệt để `useEffect`, cấm code comment rác |
| **Kích thước PR** | Dồn nén 1 PR hàng nghìn dòng làm nhiều việc | Chia nhỏ thành các PR nguyên tử (atomic PRs) với git commit có ý nghĩa |
| **Chiến lược Token**| Sợ tốn token nên không cho agent chạy lặp | Đầu tư token vào khâu xây dựng Harness & Evals trước, hưởng lợi nhuận tự động hóa lâu dài |
