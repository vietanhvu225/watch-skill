# DeepSeek Tiết Lộ Thiết Kế Super Agent: Cách Xây Dựng Tác Nhân Kiểu GrokBot Dưới Góc Nhìn Hệ Thống Phân Tán

- **Video URL:** https://www.youtube.com/watch?v=3LjF3rpo-yI
- **Watch Skill ID:** `b066c34cd4754fd4`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 16:19
- **Speaker:** lustoykov
- **Channel:** lustoykov

---

## 1. Tóm Tắt Điều Hành

```
┌────────────────────────────────────────────────────────────────────────────┐
│      "EXPLORING SUPER-AGENT DESIGN: TOWARDS A MORE AUTONOMOUS SYSTEM"      │
│      Giải phẫu 5 khối kiến trúc + Watchdog dưới góc nhìn Distributed Sys   │
├────────────────────────────────────────────────────────────────────────────┤
│  VẤN ĐỀ: Con người chỉ theo dõi được 3–5 sessions cùng lúc                 │
│  MỤC TIÊU: Tách Agent khỏi khái niệm "single manually-driven session"      │
│            → 1 thực thể liên tục (GrokBot / Claude Duet / ChatGPT Dots)    │
├───────────────────────────────┬────────────────────────────────────────────┤
│  4 ĐẶC TÍNH CỐT LÕI          │  5 KHỐI KIẾN TRÚC + WATCHDOG               │
│  1. Continuous (Liên tục)     │  [2] Two-Level Architecture (Router/Worker)│
│  2. Productive (Năng suất)    │  [3] Event-Driven Triggers (Fast/Slow Path)│
│  3. Proactive (Chủ động)      │  [4] Full-Duplex Multi-Channel Interaction │
│  4. Persistent (Bền bỉ 24/7)  │  [5] Compute-Storage-Environment Decoupling│
│                               │  [6] External Watchdog (Monitor & Recovery)│
└───────────────────────────────┴────────────────────────────────────────────┘
  TUYÊN NGÔN THIẾT KẾ: "Sessions should be an implementation detail of the
                        system, not a mental burden on its users."
```

---

## 2. Đặt Vấn Đề: Tại Sao Thiết Kế Single-Session Đã Chạm Trần?

Trong video, **lustoykov** mổ xẻ bài báo kỹ thuật *"Exploring Super-agent Design: Towards a More Autonomous Agent System"* của đội ngũ **DeepSeek Harness (`@DeepSeekHarness`)** — bản thiết kế đứng sau làn sóng các tác nhân cá nhân luôn trực tuyến (always-on personal agents) như **Grok Bot (xAI)**, **Claude Duet (Anthropic)** và **ChatGPT Dots (OpenAI)**.

### Giới hạn của kiến trúc Agent hiện tại (Frame `0:28` – `1:19`):
- **Đơn vị công việc bị khóa vào Session:** Các agent hiện nay vẫn dùng một phiên đơn lẻ (single session) làm đơn vị công việc cơ bản, và các phiên hoàn toàn cô lập với nhau.
- **Điểm nghẽn sự chú ý của con người:** Theo kinh nghiệm thực nghiệm của DeepSeek, một người chỉ có thể theo dõi **3 đến 5 active sessions cùng lúc**:
  - **1 tác vụ đơn lẻ (Single task):** Bị giới hạn bởi **năng lực mô hình (model capability)**.
  - **Đa tác vụ song song (Multi-tasking):** Bị giới hạn bởi **sự chú ý của con người (human attention)**.
- **Tại sao chỉ gắn thêm CLI/MCP/Connectors vào Local Agent là không đủ?**
  - Nó vẫn chạy trong **1 phiên đồng bộ (synchronous session)** bị chặn (block) khi gặp tác vụ dài.
  - Ngữ cảnh từ các công việc khác nhau bị **trộn lẫn (contexts get mixed together)**.
  - Mọi thứ **dừng hoạt động ngay khi thiết bị cá nhân offline**.

> **Giải pháp — Định nghĩa Super-Agent (Frame `1:19` – `2:04`):**
> Tách rời (decouple) agent khỏi bất kỳ phiên thủ công đơn lẻ nào. Thay vào đó, agent phải là **một thực thể liên tục duy nhất (one continuous entity)** giữ vững ngữ cảnh xuyên suốt các phiên và xử lý nhiều tác vụ phức tạp cùng lúc mà không gây nhiễu lẫn nhau.

### Hai ví dụ thực tế từ nội bộ DeepSeek Harness (Frame `0:15`, `2:04`):
1. **Một tác vụ xuyên suốt IM và GitHub (`One task, carried forward continuously across IM and GitHub`):**
   - `User A` trên IM group chat: *"Tôi muốn font chữ cho body text, code và terminal có thể cấu hình riêng biệt."*
   - `Maintainer` tag `@Super-agent Please add support for this feature.` → `Super-agent` mở Pull Request với 3 dòng cài đặt font.
   - `User B` hỏi tiếp trên IM: *"Cỡ chữ (font sizes) có chỉnh riêng được không?"* → `Super-agent` cập nhật vào **cùng Pull Request đó**.
   - Lên GitHub PR review, `Reviewer` và `Architect` góp ý gom vào collapsible section rồi đổi lại thành mở rộng trực tiếp từ dòng Font size → `Super-agent` tự động hiểu toàn bộ mạch ngữ cảnh từ IM sang GitHub và sửa cho đến khi PR được merge.
2. **Giám sát thầm lặng group chat công khai & tự mở PR sửa lỗi (`Silently monitoring a public group chat, proactively identifying an issue and opening a fix Pull Request`):**
   - Không ai `@mention` agent. Khi người dùng báo lỗi trong group chat: *"Trên bản macOS desktop, click vào một session khi cửa sổ đang unfocused bị tính là click đúp..."*, `Super-agent` âm thầm tìm nguyên nhân gốc rễ (root cause) và tự mở Pull Request sửa lỗi.

---

## 3. Bốn Đặc Tính Cơ Bản Của Một Super-Agent (Frame `2:52`)

Không một vòng lặp ReAct đơn lẻ (single ReAct loop) nào có thể cung cấp đồng thời cả 4 đặc tính sau:

| Đặc tính | Định nghĩa từ bài báo (Frame `0007`) | Ý nghĩa thực tế |
| :--- | :--- | :--- |
| **1. Continuous (Liên tục)** | Người dùng tương tác trực tiếp tại nơi họ làm việc (IM chats, GitHub comments) và luôn cảm thấy đang nói chuyện với **cùng một agent**. Không bao giờ phải tự hỏi *"Session X đã có thông tin Y trong context chưa?"*. Đổi từ Plan A trên IM sang Plan B trên GitHub PR comment → agent tự hiểu và điều chỉnh. | Xóa bỏ ranh giới phiên trong tâm trí người dùng. |
| **2. Productive (Năng suất)** | Phản hồi nhanh và hoàn thành nhiều tác vụ phức tạp **song song (in parallel)** với chất lượng cao. Có thể nhận chỉ thị tiếp theo trước khi lệnh hoặc tác vụ trước đó kịp hoàn thành. | Không bao giờ bị khóa cứng chờ đợi. |
| **3. Proactive (Chủ động)** | Không ngồi chờ chỉ thị từng bước (step-by-step instructions). Liên tục lắng nghe sự kiện trong môi trường, hành động khi thấy việc đáng xử lý, và **giữ im lặng trong các trường hợp còn lại (`stays quiet otherwise`)**. | Chuyển từ Prompt-driven sang Event-driven. |
| **4. Persistent (Bền bỉ)** | Luôn trực tuyến (`always online`), kết nối trực tiếp với môi trường làm việc thật của người dùng, và tự động phục hồi sau các sự cố bất thường trong quá trình chạy dài hạn. | Hoạt động 24/7 xuyên thiết bị. |

---

## 4. Tổng Quan Sơ Đồ Kiến Trúc 6 Mô-đun (Frame `3:26` – `4:50`)

```
┌──────────────────────┐    ┌──────────────────────┐    ┌───────────────────────────┐    ┌──────────────────────────┐
│  [3] Event Triggers  ├───►│ [3] Two-Stage Filter ├───►│     [2] Router Layer      │◄──►│ [4] Interaction Channels │
│ Chat / PR / CI / Mail│    │   Fast / Slow Path   │    │ Shared Context, Routing   │    │   IM / GitHub / Email    │
└──────────────────────┘    └──────────┬───────────┘    └──────┬─────────────┬──────┘    └──────────────────────────┘
                                       │                       │             │
                      Subscription     │                       ▼             ▼
                      Pass-Through     │            ┌────────────────┐ ┌────────────────┐    ┌──────────────────────┐
                                       └───────────►│Worker Session 1│ │Worker Session N│───►│   [5] Environments   │
                                                    │Isolated Context│ │Isolated Context│    │ Via Endpoint Daemons │
                                                    └───────┬────────┘ └───────┬────────┘    └──────────────────────┘
┌──────────────────────┐                                    │                  │
│     [6] Watchdog     │                                    ▼                  ▼
│  External Monitor &  ├───────────────────────────►┌───────────────────────────────────┐
│       Recovery       │                            │       [5] Session Database        │
└──────────────────────┘                            │  Persistent State for All Sessions│
                                                    └───────────────────────────────────┘
```

---

## 5. Giải Phẫu 5 Khối Kiến Trúc Cốt Lõi

### Khối 1 (Section 2): Kiến Trúc Hai Tầng — Two-Level Architecture (Frame `4:33` – `5:19`)

Để giải quyết sự đánh đổi cơ bản giữa **tính liên tục của ngữ cảnh (context continuity)** và **năng suất thực thi đồng thời (productive concurrency)**, hệ thống tách làm 2 tầng:

1. **Tầng Định Tuyến (`Router Layer`):**
   - Giữ ngữ cảnh chung (`Shared Context`), giao tiếp với con người qua các kênh (`IM`, `GitHub`, `Email / Calendar`) và điều phối công việc.
   - **Quy tắc sống còn:** Để không trở thành cổ chai hiệu năng (performance bottleneck), Router **tuyệt đối không tự thực thi tác vụ nặng** — nó chỉ định tuyến và quản lý trạng thái toàn cục.
2. **Tầng Công Nhân (`Worker Sessions 1..N`):**
   - Mỗi Worker chạy trong một ngữ cảnh cô lập (`Isolated Context`), nhận 1 tác vụ cụ thể và thực thi độc lập, không làm bẩn ngữ cảnh của nhau.

> **Triết lý Sản phẩm (Frame `0010`):**
> *"Việc bắt người dùng tự quản lý các sessions bằng tay thực chất là một thói quen thiết kế sót lại từ các LLM API cấp thấp. Sessions phải là một chi tiết cài đặt ẩn của hệ thống, không phải gánh nặng tâm trí đè lên người dùng."*
> Trong các thiết kế cũ, một session phải gánh 2 việc: (1) quản lý context của model, và (2) làm đơn vị quản lý dự án của người dùng. Khi tách rời hai việc này, quản lý dự án không còn bị trói buộc vào ranh giới session nữa.

---

### Khối 2 (Section 3.1 – 3.3): Bộ Kích Hoạt Hướng Sự Kiện — Event-Driven Triggers (Frame `6:04` – `9:11`)

Để biến Super-Agent thành **Proactive**, đầu vào chính của hệ thống không còn là từng câu prompt con người gõ tay vào, mà là **các sự kiện bán cấu trúc từ môi trường (`Triggers`)**: tin nhắn group chat, email, lịch hẹn, comment review code, kết quả CI.

Hành trình xử lý một sự kiện đi qua 3 cơ chế kỹ thuật sắc bén:

#### A. Chính Sách Lọc Hai Giai Đoạn (`Two-Stage Filter: Fast Path & Slow Path` — Frame `0012`)
Nếu đẩy mọi tin nhắn trong group chat thẳng vào LLM của Router, chi phí và độ trễ sẽ bùng nổ. DeepSeek đặt một phễu lọc 2 tầng:

```
Trigger 1 (Unmonitored Group) ──────┐
"Where to eat lunch?"               │   ┌─────────────────────────┐
                                    ├──►│   Programmatic Policy   │
Trigger 2 (Routine User Group Chat) │   │        Fast Path        │
"The new UI looks clean" ───────────┼──►│   Deterministic Rules   │───► Drop Trigger 1 (0 LLM call)
                                    │   │  Group scope / Allowlist│     Pass Trigger 2, 3 (Candidates)
Trigger 3 (User Group Bug Report)   │   └─────────────────────────┘              │
"Sometimes blank on session switch" ┘                                            ▼
                                                                    ┌─────────────────────────┐
                                                                    │     Semantic Policy     │
                                                                    │        Slow Path        │
                                                                    │LLM prompt-based judgment│
                                                                    │ Silence 2, Act on 3     │
                                                                    └────────────┬────────────┘
                                                                                 ▼
                                                                      Route Event (Trigger 3)
```

- **Tầng 1 — `Programmatic Policy (Fast Path)`:** Code tất định (deterministic rules) kiểm tra phạm vi group / allowlist. Loại bỏ ngay `Trigger 1` (*"Trưa nay ăn gì?"*) mà **không tốn bất kỳ lệnh gọi LLM nào (`no LLM call`)**.
- **Tầng 2 — `Semantic Policy (Slow Path)`:** Dùng một LLM nhẹ đánh giá bằng prompt đối với các ứng viên lọt qua (`Trigger 2, 3`). Giữ im lặng trước `Trigger 2` (*"UI mới nhìn sạch đấy"* — non-actionable chat) và chỉ kích hoạt hành động cho `Trigger 3` (báo cáo lỗi màn hình trắng khi chuyển session).

#### B. Cơ Chế Đăng Ký & Chuyển Tiếp Thẳng (`Subscription and Pass-Through` — Frame `0013`)
- **Vấn đề:** Nếu mọi tin nhắn tiếp theo trong cùng một luồng thảo luận (Thread / PR) đều phải đi qua Router, Router sẽ trở thành điểm nghẽn cổ chai.
- **Giải pháp:**
  - **Sự kiện đầu tiên (`Unsubscribed / Slow Path`):** Đi qua Router Layer để tạo Worker và thiết lập một **Subscription** giữa Worker đó với Thread.
  - **Các sự kiện tiếp theo (`Follow-up Pass-Through / Fast Path`):** Đi **thẳng trực tiếp từ Thread vào Worker Session đã đăng ký**, bỏ qua việc đánh giá lại trên Router!
  - Quan hệ giữa `Threads` và `Workers` là **nhiều-nhiều (`many-to-many`)**: Một Worker có thể đăng ký theo dõi nhiều Thread liên quan (ví dụ: vừa theo dõi IM thread vừa theo dõi GitHub PR thread của cùng một tính năng), và một Thread có thể có nhiều Worker cùng lắng nghe.

#### C. Cơ Chế `Inline & Spill` và Đồ Thị Truy Hồi Đa Chặng (`Chain of Recall` — Frame `0014`)
- **Vấn đề:** Khi các sự kiện đi thẳng xuống Worker qua Fast Path, cộng thêm thời gian chạy dài hạn, Router sẽ bị mất ngữ cảnh chi tiết hoặc tràn bộ nhớ nếu nhồi tất cả vào prompt.
- **Giải pháp (`Inline & Spill` + `Chain of Recall`):**
  - Mọi thực thể trong hệ thống (`Triggers`, `Threads`, `Worker Sessions`) đều được cấp một **ID duy nhất** (ví dụ: `Trigger #42`, `Thread #8`, `Worker #3`).
  - Khi tiêm một sự kiện vào model, hệ thống chỉ **nhúng trực tiếp (`Inline`)** phần đầu ngắn gọn (ví dụ: 2K ký tự đầu của `Trigger #42` + danh sách ID của các thực thể liên quan: `thread_id: #8`, `worker_id: #3`) và **tràn (`Spill`)** phần nội dung dài còn lại ra lưu trữ ngoài.
  - Mỗi đối tượng khi được gọi qua tool (`read_thread(#8)`, `read_trigger(#39)`, `read_worker(#3)`) lại tiếp tục trả về bản tóm tắt kèm ID của các đối tượng chặng kế tiếp (`next-hop objects`). Nhờ đó, model có thể **lần theo đồ thị tham chiếu từng bước một (`hop by hop along references`)** để khôi phục ngữ cảnh sâu tùy ý mà không làm phình context window!

---

### Khối 3 (Section 3.4): Hành Động Đi Trước — Speculative Execution / Acting Ahead (Frame `9:25` – `9:47`)

Mượn khái niệm **Thực thi suy đoán (`Speculative Execution`)** từ kiến trúc vi xử lý máy tính:
- Dựa trên các tín hiệu môi trường, agent **bắt đầu làm việc ngầm trong nền (in the background) trước cả khi biết chắc kết quả đó có được dùng hay không**.
- Chấp nhận tốn thêm compute chạy nền để con người phải chờ ít hơn và can thiệp ít hơn. Kết quả nào hữu ích thì giao ra, phần còn lại hủy bỏ (`discarded`).
- **Ví dụ 1 (Personal Agent):** Khi nhận email mời họp hoặc tin nhắn to-do, agent tự kiểm tra lịch trùng, thu thập tài liệu nền và soạn sẵn bản nháp trả lời. Người dùng chỉ việc nhìn lướt qua và bấm gửi.
- **Ví dụ 2 (R&D Collaboration — Chính sách `"Silently Reproduce and Fix"` — Frame `0016`):**
  - Trong group người dùng DeepSeek Harness, User A phàn nàn: *"Dấu ngã (`~`) trong đầu ra của model bị render thành chữ gạch ngang (strikethrough), rất khó đọc."*
  - Trong lúc cả nhóm còn đang tranh luận xem nên sửa ở bước training hay sửa luật render Markdown, `Super-agent` ở dưới nền đã âm thầm tái hiện lỗi, mở sẵn Pull Request sửa luật chỉ gạch ngang khi có hai dấu `~~text~~` (giữ nguyên một dấu `~` cho khoảng số như `18~20`), và trích dẫn chuẩn `Pandoc` vs `GFM` để giải thích khi User B hỏi! Nếu không tái hiện được lỗi hoặc không tìm ra bản vá gọn, nó sẽ giữ im lặng tuyệt đối.

---

### Khối 4 (Section 4): Tương Tác Đa Kênh Song Công Toàn Phần — Full-Duplex Multi-Channel Interaction (Frame `10:27` – `10:48`)

```
CLASSIC REACT LOOP (Half-Duplex / Foreground Blocking):
[User Msg 1] ──► [LLM Call: call run()] ──► (BỊ CHẶN ĐỒNG BỘ CHỜ TOOL CHẠY XONG...) ──► [Tool Output] ──► [LLM Reply]

ASYNCHRONOUS REACT LOOP (Full-Duplex / Background Execution):
[User Msg 1] ──► [LLM Call: call start()] ──► [Immediate "Started" Notice] ──► [User Msg 2] ──► [LLM Instant Reply]
                          │                                                                            ▲
                          └───────► (Asynchronous Tool Execution in Background) ──► [New Msg: Final] ──┘
```

- **Phá bỏ vòng lặp ReAct chặn (`Asynchronous ReAct Loop` — Frame `0018`):**
  - Trong ReAct cổ điển (Half-Duplex), khi model gọi lệnh `run()` đồng bộ, toàn bộ phiên bị khóa cứng cho đến khi tool chạy xong; người dùng nhắn gì thêm cũng không phản hồi được.
  - Chuyển sang **Full-Duplex**: Lệnh gọi tool bất đồng bộ trả về thông báo `"Started"` ngay lập tức và đẩy tác vụ xuống chạy nền. Phiên trò chuyện ở tiền cảnh vẫn hoàn toàn rảnh để trả lời tin nhắn mới (`User Msg 2`) hoặc nhận lệnh điều chỉnh ngay trong lúc tool đang chạy!
- **Thích ứng đa kênh (`Multi-Channel Tailoring`):** Điều chỉnh mức độ chi tiết và định dạng câu trả lời phù hợp với văn hóa giao tiếp của từng kênh (tin nhắn IM ngắn gọn khác với nhận xét chi tiết trên GitHub PR hoặc Email).

---

### Khối 5 (Section 5 & 6): Tách Biệt Compute – Storage – Environment & External Watchdog (Frame `13:33` – `16:19`)

Đây là điểm mà **lustoykov** đánh giá cao nhất về mặt kỹ thuật thực chiến:

```
        ┌──────────────────────────────────────┐          ┌──────────────────────────────────────┐
        │              Agent Loop              │◄────────►│           Session Database           │
        │   Stateless Elastic Cloud Containers │          │       Persistent State Storage       │
        └──────────────────┬───────────────────┘          └──────────────────────────────────────┘
                           │
      ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─┼─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─
                           │ (Kết nối theo yêu cầu qua Endpoint Daemons)
        ┌──────────────────┴───────────────────┬─────────────────────────────────────────┐
        ▼                                      ▼                                         ▼
┌───────────────────────────────┐    ┌───────────────────────────────┐    ┌────────────────────────────────────────┐
│     Personal Dev Machine      │    │    Cloud / On-Demand Env      │    │ Intermittently Online Personal Devices │
│        endpoint daemon        │    │        endpoint daemon        │    │             endpoint daemon            │
└───────────────────────────────┘    └───────────────────────────────┘    └────────────────────────────────────────┘
```

1. **Tại sao đẩy toàn bộ môi trường dev lên Cloud VM lại đau đớn? (`12:20` – `13:15`)**
   - Ai từng thử bê nguyên môi trường phát triển từ máy cá nhân lên đám mây đều gặp cơn ác mộng: phải dựng lại môi trường, đồng bộ dotfiles, cấp lại toàn bộ đăng nhập (logins), khóa SSH, quyền truy cập dữ liệu cục bộ.
2. **Giải pháp Tách Biệt 2 Tầng (`Two-Level Decoupling` — Frame `0019`, `0020`):**
   - **Tầng 1 — Tách Compute khỏi State trên Cloud:** `Agent Loop` chạy trong các container đám mây co giãn phi trạng thái (`Stateless Elastic Cloud Containers`), còn toàn bộ trạng thái bền vững nằm trong `Session Database` (như S3 / DB phân tán).
   - **Tầng 2 — Tách Agent Loop khỏi Môi trường thực thi (`Decoupled Execution Environments`):** Thay vì bắt người dùng chuyển nhà lên Cloud, hãy cài một **`endpoint daemon` cực nhẹ** ngay trên:
     - Máy dev cá nhân (`Personal Dev Machine`)
     - Môi trường Cloud theo yêu cầu (`Cloud / On-Demand Env`)
     - Các thiết bị cá nhân lúc online lúc offline (`Intermittently Online Personal Devices`)
   - **Kết quả:** Agent không còn bị trói vào một cỗ máy duy nhất. Nó sống trên Cloud 24/7, nhưng khi cần chạy lệnh hay đọc code trên laptop cá nhân của bạn (nơi đã có sẵn mọi login và dữ liệu), nó gọi qua `endpoint daemon`. Ngay cả khi laptop của bạn gập máy offline rồi mở lại, Cloud Agent sẽ tự động bắt nhịp và hoàn thành các tác vụ cần chạy trên thiết bị đó!
3. **Mô-đun [6] `External Watchdog`:** Một trình giám sát độc lập bên ngoài liên tục theo dõi sức khỏe hệ thống và tự động khôi phục các phiên bị treo hoặc lỗi bất thường trong quá trình vận hành dài ngày.

---

## 6. Liên Kết Chéo Trong Kho Lưu Trữ (Cross-References)

- **Chuỗi bài phân tích hệ sinh thái DeepSeek Super-Agent & Harness:**
  - [Thực Chiến DeepSeek SuperAgent Harness — AICodeKing](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_superagent_harness_hands_on_aicodeking.md) — Thử nghiệm thực tế nguyên mẫu `dsh-superagent`, bài toán kinh tế học token cache ($0.07 trên DeepSeek Flash vs $4.64 trên Sonnet) và lý do phải cấm `bash` tool trên Router bằng Deny Rules.
  - [DeepSeek Open-Source Super-Agent DSH — TEKTHRILL](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_open_sources_super_agent_dsh_tekthrill.md) — Tóm tắt nhanh triết lý *"Polite by Design"* và benchmark tăng tốc 1.9x (15.7 phút vs 29.5 phút).
  - [DeepSeek Đang Tái Định Nghĩa Harness! — 7 Prompts Creator Mode (AICodeKing)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_redefining_harnesses_creator_mode_prompts_aicodeking.md) — Giải phẫu Creator Mode tự sinh plugin bằng 2 công cụ Read-only Runtime Introspection.
- **Đối chiếu kiến trúc tương đồng từ các Frontier Labs khác:**
  - [Cách Anthropic Xây Dựng Multi-Agent Harnesses: Claude Managed Agents](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/anthropic_managed_agents_evolution_harness_beamnxw.md) — Kiến trúc *"Brain tách khỏi Hands"* của Anthropic có cùng tư tưởng phân tách `Agent Loop` (stateless container), `Session Database` (event log) và `Environment` (sandboxes/endpoints).
  - [Amazon Infinity: AI Agent Runtime Viết Bằng Rust](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/amazon_infinity_ai_agent_runtime_rust_lets_get_rusty.md) — Cách AWS giải bài toán thực thi bất đồng bộ (`RAP Tool Server`) và biến agent đang chờ thành dữ liệu thuần túy (`Scale to Zero`).

