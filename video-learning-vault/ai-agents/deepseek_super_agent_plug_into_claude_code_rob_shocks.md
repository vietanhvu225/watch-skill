# Giải Mã DeepSeek Super-Agent & Hướng Dẫn Tích Hợp Trực Tiếp Vào Claude Code / OpenCode | Rob Shocks (Switch Dimension)

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=n2TLtU65ZtA)
- **Video ID:** `n2TLtU65ZtA`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-05
- **Duration:** 12:39
- **Speaker:** Rob Shocks (Switch Dimension — Phân tích nguyên mẫu `dsh-superagent-prototype` của DeepSeek và hướng dẫn tự dựng Super-Agent bằng TypeScript + Docker cho Claude Code / OpenCode)
- **Transcript File:** [deepseek_super_agent_plug_into_claude_code_rob_shocks_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_super_agent_plug_into_claude_code_rob_shocks_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong bối cảnh các mô hình ngôn ngữ lớn (LLM) đã tiến bộ vượt bậc trong 6 tháng qua và không còn cần con người "trông trẻ" (babysit) từng bước, nút thắt cổ chai thực sự của ngành công nghệ phần mềm đã dịch chuyển hoàn toàn sang **con người — những "Meat Proxies" (Trạm trung chuyển bằng xương bằng thịt)**. Như Lauren Tan (SpaceX/Cursor) từng ví von về *"dòng sông mã nguồn hung dữ" (violent river of code)* hay báo cáo SDLC của Anthropic chỉ ra: mỗi tác nhân AI bạn mở thêm chỉ là một tab terminal nữa bạn phải canh chừng, một bản diff nữa phải đọc, và một câu hỏi nữa đang chờ bạn bấm nút duyệt.

Theo nghiên cứu của DeepSeek, một lập trình viên chỉ có thể theo dõi tối đa **3 đến 5 phiên làm việc (sessions) đồng thời** trước khi não bộ bị quá tải ngữ cảnh. Để đập tan giới hạn này, ngày 31/10/2026, DeepSeek công bố nguyên mẫu nghiên cứu **DSH Super-Agent** (`dsh-superagent-prototype`) xoay quanh một tuyên ngôn duy nhất trong tài liệu triết lý (`000-philosophy.md`):

> **"Sessions should be an implementation detail of the system. They shouldn't be a mental burden on its users."**
> *(Các phiên làm việc phải là chi tiết kỹ thuật ẩn bên dưới hệ thống, chứ không được phép là gánh nặng tâm trí đè lên người dùng).*

Điểm giá trị nhất của video này là **Rob Shocks** không chỉ giải phẫu kiến trúc của DeepSeek mà còn **tách rời hoàn toàn bản thiết kế này khỏi hệ sinh thái DeepSeek**, đóng gói thành một bản mẫu mã nguồn mở viết bằng **TypeScript chạy trong Docker khóa chặt (Locked-down Container)** để bất kỳ ai cũng có thể cắm trực tiếp vào **Claude Code, OpenCode hoặc Cursor** chỉ với 1 dòng cấu hình.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│               SESSION-BASED BABYSITTING VS. DEEPSEEK / ROB SHOCKS SUPER-AGENT               │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  TRADITIONAL WORKFLOW (CON NGƯỜI LÀ BỘ ĐỊNH TUYẾN - "MEAT PROXY"):                          │
│                                                                                             │
│     [Tab 1: Claude Code] ◄──┐                                                               │
│     [Tab 2: Codex CLI]   ◄──┼──► [LẬP TRÌNH VIÊN (3-5 Tabs Max)] ──► Quá tải não bộ,        │
│     [Tab 3: Cursor]      ◄──┤    - Tự nhớ ngữ cảnh từng tab          Mất hết việc khi       │
│     [Tab 4: OpenCode]    ◄──┘    - Tự copy-paste kết quả qua lại     gập màn hình Laptop!   │
│                                                                                             │
│  SUPER-AGENT ARCHITECTURE (1 CONTINUOUS ENTITY + LOCKED-DOWN DOCKER SANDBOX):               │
│                                                                                             │
│   External Triggers              STAGE 1: CODE GATE          STAGE 2: ROUTER & WORKERS       │
│   ┌────────────────┐           ┌────────────────────┐      ┌─────────────────────────────┐  │
│   │- GitHub Issues │           │ Deterministic Code │      │  ROUTER (Light Model, No    │  │
│   │- PR Comments   ├──────────►│ Allowlist Check    ├─────►│  Bash) ──► Tạo Ticket +     │  │
│   │- CI Failures   │           │ (Chặn 93% nhiễu,   │      │  Git Worktree + Branch      │  │
│   │- Sentry / Jira │           │  tốn 0 Token!)     │      └──────────────┬──────────────┘  │
│   └────────────────┘           └────────────────────┘                     │                 │
│                                                                           ▼                 │
│   ┌────────────────────────────────────────────────┐      ┌─────────────────────────────┐  │
│   │ HUMAN SIGN-OFF PANEL ("Needs You")             │      │  ISOLATED WORKER (Docker)   │  │
│   │ - Không tự ý merge code                        │◄─────┤  - Claude Code / OpenCode   │  │
│   │ - Chỉ giao tiếp qua cổng CLI kiểm duyệt `sa`   │      │  - Subscribe thẳng vào PR   │  │
│   │ - Con người chỉ làm Product Decisions & Duyệt  │      │  - Silently Reproduce & Fix │  │
│   └────────────────────────────────────────────────┘      └─────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Giải Phẫu Nguyên Mẫu DeepSeek Super-Agent (`dsh-superagent-prototype`)

### 1. Bốn Đặc Tính Cốt Lõi (The 4 Core Traits)
Thay vì coi mỗi lần mở cửa sổ chat là một đơn vị công việc rời rạc, Super-Agent hoạt động như một thực thể duy nhất với 4 phẩm chất:
1. **Continuous (Liên tục):** Dù bạn nhắn tin cho nó trên GitHub Issue, PR thread hay ứng dụng chat nội bộ, bạn vẫn đang nói chuyện với cùng một tác nhân duy nhất nắm giữ mạch bối cảnh xuyên suốt.
2. **Productive (Năng suất cao):** Thực thi hàng chục tác vụ phức tạp song song ở dưới nền mà không bao giờ làm nghẽn luồng giao tiếp với người dùng.
3. **Proactive (Chủ động):** Không ngồi chờ người dùng gõ prompt. Tự động lắng nghe sự kiện môi trường và hành động trước.
4. **Persistent (Bền bỉ 24/7):** Luôn online trên server/container; không bị ngắt quãng khi lập trình viên đóng nắp MacBook đi ngủ.

### 2. Kiến Trúc Phân Tầng: Router (Không Công Cụ) & Workers (Cô Lập)
- **Tầng Router (Điều phối viên):**
  - Chạy một phiên sống dài hạn (long-lived session), chịu trách nhiệm toàn bộ việc giao tiếp với con người và phân công việc.
  - **Quy tắc sống còn:** Router **không bao giờ tự làm việc** và **hoàn toàn không được cấp quyền dùng công cụ `bash`**. Vì chỉ làm nhiệm vụ phân loại và điều phối, Router chỉ cần dùng một model nhỏ, nhanh và rẻ.
- **Tầng Workers (Công nhân thực thi):**
  - Mỗi Worker nhận một nhiệm vụ độc lập, chạy trong không gian ngữ cảnh riêng biệt với bộ công cụ đầy đủ và model mạnh nhất.
  - Worker **không bao giờ nói chuyện trực tiếp với con người** mà chỉ báo cáo kết quả về cho Router.

### 3. Cơ Chế Triggers, Lọc Kép (Double Filtering) & Đăng Ký Luồng Trực Tiếp (PR Subscription)
Tương tự như cách Anthropic sử dụng file `intent.md` để gom mọi tín hiệu từ Email, Jira, Sentry, lịch họp vào quy trình SDLC, DeepSeek gọi mọi tín hiệu đầu vào là **Triggers**.
- **Lọc Tầng 1 (Code Gate):** Kiểm tra bằng code thuần túy xem người gửi là ai, từ nguồn nào, có nằm trong danh sách cho phép (`allowlist`) hay không.
- **Lọc Tầng 2 (Prompt Policy):** Chỉ những sự kiện vượt qua Tầng 1 mới được đưa vào LLM để đối chiếu với các chính sách viết bằng ngôn ngữ tự nhiên.
- **Tối ưu hóa Cache Token bằng PR Thread Subscription:** Khi một Worker đã nhận xử lý một Pull Request, nó sẽ **đăng ký theo dõi (subscribe) trực tiếp vào luồng PR đó**. Mọi bình luận review tiếp theo trên PR sẽ đi thẳng vào phiên của Worker đó mà **bỏ qua Router**. Điều này giúp giữ nguyên bộ nhớ Warm Cache của Worker, tránh việc phải nạp lại toàn bộ ngữ cảnh (context drop) vốn cực kỳ tốn tiền.

### 4. Chính Sách "Silently Reproduce and Fix" (Âm Thầm Tái Hiện & Sửa Lỗi)
Nếu Lauren Tan có mẫu hình *"Fix and Fuzz"*, thì DeepSeek áp dụng chính sách **"Silently Reproduce and Fix"**:
- Khi thấy một báo cáo lỗi (bug report) xuất hiện trong nhóm chat hoặc Issue, một Worker sẽ âm thầm chạy ở chế độ nền để thử tái hiện lỗi và tìm bản vá.
- **Bảo vệ sự chú ý tuyệt đối:** Nếu không tái hiện được hoặc chưa tìm ra cách sửa đơn giản, Worker sẽ **im lặng hoàn toàn**. Nó chỉ lên tiếng báo cáo khi đã xác nhận được lỗi và có sẵn phương án sửa chữa rõ ràng.
- Bên cạnh đó, một phiên **Watchdog (Chó canh gác)** độc lập chạy định kỳ để kiểm tra xem các Workers có đang tiến triển thực sự hay đã đi chệch mục tiêu (goal drift).

---

## 3. Phân Tích Số Liệu Thực Nghiệm: Đánh Đổi Token Lấy Tốc Độ & Sự Ổn Định

DeepSeek đã thiết lập một bài kiểm thử thực tế lấy từ **300 tin nhắn thực** (từ group chat, GitHub và thông báo CI), trong đó chỉ có **20 tin nhắn là yêu cầu công việc thực sự** (còn lại 280 tin nhắn là nhiễu).

| Chỉ Số Đo Lường (Chạy 5 lần) | Một Phiên Đơn Lẻ + Sub-Agent Tool | DeepSeek Super-Agent (Router + Workers) | Đánh Giá Kỹ Thuật |
| :--- | :---: | :---: | :--- |
| **Thời gian hoàn thành (Trung vị)** | ~20.0 – 25.0 phút | **12.0 phút** | Nhanh hơn gần gấp đôi |
| **Thời gian chạy chậm nhất (Worst-case)**| **29.5 phút** | **15.7 phút** | Ổn định hơn vượt trội |
| **Độ trễ quyết định khi tải cao (40 triggers/phút)**| Nghẽn cổ chai tuần tự | **Giảm từ 6.4s xuống 1.6s / quyết định** | Nhờ cơ chế gom lô tự động (**Event Batching**) |
| **Lượng Cache Read Tokens tiêu thụ** | **~6 triệu tokens** | **~23 triệu tokens** | Đánh đổi ~3.8x cache tokens để mua thời gian của con người |

> [!IMPORTANT]
> **Hiện tượng nghịch lý của Router khi tải cao:** Bạn có thể nghĩ một Router duy nhất sẽ bị nghẽn khi có quá nhiều sự kiện dồn dập. Nhưng thực tế, khi tần suất tăng từ 1 trigger/phút lên **40 triggers/phút**, các sự kiện đến trong lúc Router đang suy luận sẽ được **gom lại thành một lô (Batch)** cho lượt gọi tiếp theo. Nhờ vậy, thời gian trung bình cho mỗi quyết định điều phối giảm mạnh từ **6.4 giây xuống chỉ còn 1.6 giây**!

---

## 4. Hướng Dẫn Tự Dựng Super-Agent Với Claude Code / OpenCode Trong Docker Khóa Chặt

Rob Shocks cảnh báo một lỗ hổng bảo mật lớn trong tài liệu triển khai (`006-deployment.md`) của nguyên mẫu DeepSeek: Mặc định cả Router và Workers của họ đều chạy với quyền **Full Access nguy hiểm (`--dangerously-skip-permissions`)**, rất dễ dính tấn công **Prompt Injection** khi đọc bình luận từ nguồn không tin cậy.

Do đó, Rob đã thiết kế lại một phiên bản **TypeScript tinh gọn, an toàn cấp doanh nghiệp** (`switch-dimension/deepseek-superagent-guide`) để chạy với **Claude Code** hoặc **OpenCode**:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│             ROB SHOCKS' LOCKED-DOWN DOCKER SUPER-AGENT FOR CLAUDE CODE / OPENCODE           │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│  [GitHub Repo Issues / PRs]                                                                 │
│              │                                                                              │
│              ▼ (1) Mention Bot (@my-agent)                                                  │
│  ┌───────────────────────────────────────────────────────────────────────────────────────┐  │
│  │ HOST MACHINE GATE (Plain TypeScript Code)                                             │  │
│  │ - Kiểm tra cứng người gửi có trong `allowlist` không (Comment không thể tự cấp quyền) │  │
│  │ - Tạo Ticket ID + Nhánh Git mới + Git Worktree độc lập                                │  │
│  └───────────┬───────────────────────────────────────────────────────────────────────────┘  │
│              │                                                                              │
│              ▼ (2) Khởi chạy Agent trong Container cô lập                                   │
│  ┌───────────────────────────────────────────────────────────────────────────────────────┐  │
│  │ LOCKED-DOWN DOCKER CONTAINER (Non-Root User)                                          │  │
│  │ - KHÔNG có quyền truy cập thư mục `$HOME` hay `SSH Keys` của máy chủ                  │  │
│  │ - Dùng tài khoản GitHub Bot riêng với Fine-grained Token chỉ mở cho 1 Sandbox Repo    │  │
│  │ - Engine: Đổi giữa `claude` (Claude Code) hoặc `opencode` chỉ bằng 1 dòng Config!     │  │
│  │ - Agent KHÔNG ĐƯỢC gọi trực tiếp GitHub API, bắt buộc phải đi qua lệnh CLI nhỏ: `sa`  │  │
│  └───────────┬───────────────────────────────────────────────────────────────────────────┘  │
│              │                                                                              │
│              ▼ (3) Lệnh `sa` kiểm duyệt đầu ra                                              │
│  ┌───────────────────────────────────────────────────────────────────────────────────────┐  │
│  │ OUTPUT SANITIZER & "NEEDS YOU" SIGN-OFF PANEL                                         │  │
│  │ - Host kiểm tra độ dài tin nhắn, đóng dấu `[Automated Agent]`                         │  │
│  │ - CẤM Agent tự merge PR của chính nó ──► Đưa vào bảng "Needs You" chờ người duyệt!    │  │
│  └───────────────────────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

### Các Chốt Chặn An Ninh & Vận Hành Thực Tế Của Rob Shocks:
1. **Không Cấp Quyền Qua Văn Bản:** Cổng kiểm tra quyền (`allowlist`) được viết cứng bằng code TypeScript trước khi gọi bất kỳ mô hình nào. Không một câu thần chú Prompt Injection nào trong bình luận GitHub có thể lừa hệ thống cấp quyền thực thi.
2. **Cổng Giao Tiếp Một Chiều `sa` (Super-Agent CLI):** Tiến trình Claude Code bên trong container không có token để post thẳng lên GitHub. Khi muốn phản hồi, nó phải gọi một lệnh shell nhỏ tên là `sa`. Tiến trình chủ (Host) sẽ kiểm duyệt nội dung, cắt gọn và tự động gắn nhãn xác thực trước khi đăng.
3. **Thử Nghiệm Thực Tế (End-to-End):**
   - Rob mở một Issue yêu cầu sửa lỗi chính tả trong file `README.md`.
   - Hệ thống tự động nhận diện, tạo worktree, dùng mô hình **Claude Haiku** trong Claude Code sửa lỗi, chạy toàn bộ test suite và mở Pull Request hoàn chỉnh.
   - **Tổng thời gian:** **30 giây**. **Tổng chi phí:** **~$0.10 (10 cents)**.
   - PR hoàn tất nằm gọn trong bảng điều khiển **"Needs You"** để con người vào bấm nút duyệt cuối cùng (Sign-off).

---

## 5. Bài Học Chiến Lược: Vị Trí Mới Của Kỹ Sư Phần Mềm

- **Con người không biến mất, mà dịch chuyển lên tầng cao hơn (Moving Up the Chain):** Khi hệ thống tự quản lý các phiên làm việc và ngữ cảnh, kỹ sư phần mềm thoát khỏi cảnh làm "bảo mẫu đọc diff" ở tầng thực thi để bước lên tầng **Product Engineering**: Quyết định cái gì đáng xây dựng, thiết lập quy tắc kiểm thử (rules/verifiers), và ký duyệt cuối cùng (sign-off).
- **Sở hữu toàn bộ hạ tầng (Build Your Own, Run Your Own):** Thay vì phụ thuộc vào các nền tảng đóng đắt đỏ, bất kỳ đội ngũ kỹ thuật nào cũng có thể áp dụng mẫu hình Router + Worktree Workers + Docker Sandbox này lên chính công cụ họ đang dùng (Claude Code, OpenCode, Codex).

---

## 6. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

- [Thực Chiến DeepSeek SuperAgent Harness (AICodeKing)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_superagent_harness_hands_on_aicodeking.md) — Phân tích thực nghiệm vì sao phải tước bỏ công cụ `bash` khỏi Router và bài toán kinh tế học token cache.
- [DeepSeek Mã Nguồn Mở DSH Superagent (TEKTHRILL)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_open_sources_super_agent_dsh_tekthrill.md) — Tổng quan nhanh về triết lý "Polite by Design" của nguyên mẫu DeepSeek.
- [Xác Minh Là Code Review Mới (Ray Amjad)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/verification_is_the_new_code_review_ray_amjad.md) — Cách xây dựng hệ thống Verifiers tự động để con người tự tin ký duyệt ở bước cuối.
- [Công Cụ Git Worktree Tốt Nhất: Worktrunk (Better Stack)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/the_best_git_worktree_tool_worktrunk_better_stack.md) — Hạ tầng quản lý Git Worktree siêu tốc bổ trợ hoàn hảo cho các Worker Sessions.
