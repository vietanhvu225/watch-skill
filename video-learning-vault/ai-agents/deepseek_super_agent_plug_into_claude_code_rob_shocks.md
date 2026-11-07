# Giải Mã DeepSeek Super-Agent & Hướng Dẫn Tích Hợp Trực Tiếp Vào Claude Code / OpenCode

- **Video URL:** https://www.youtube.com/watch?v=n2TLtU65ZtA
- **Watch Skill ID:** `e3295b2559218e5d`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 12:39
- **Speaker:** Rob Shocks
- **Channel:** Rob Shocks - Switch Dimension

---

## 1. Tóm Tắt Điều Hành

```
┌────────────────────────────────────────────────────────────────────────────┐
│          DEEPSEEK SUPER-AGENT EXPLAINED (PLUG IT INTO CLAUDE CODE)         │
│          Giải phẫu dsh-superagent-prototype & Bản dựng TypeScript/Docker   │
├────────────────────────────────────────────────────────────────────────────┤
│  ĐIỂM NGHẼN MỚI: Không còn là năng lực Model, mà là CON NGƯỜI ("Meat Proxy"│
│                  đọc từng diff, nhớ từng tab chat, làm Router thủ công)    │
│  TUYÊN NGÔN:     "Sessions should be an implementation detail of the       │
│                  system, not a mental burden on its users."                │
├───────────────────────────────┬────────────────────────────────────────────┤
│  KIẾN TRÚC DEEPSEEK PROTOTYPE │  BẢN TỰ DỰNG CỦA ROB SHOCKS (TYPESCRIPT)  │
│  • 01: DeepSeek Harness (OSS) │  • Không phụ thuộc DeepSeek Harness        │
│  • 02: Super-agent (Design)   │  • Pluggable Workers: Claude Code, OpenCode│
│  • Router (No bash, light LLM)│  • Code Gate (Allowlist) chặn trước LLM    │
│  • Workers (1 task, warm cache│  • CLI cổng kiểm duyệt `sa` (Outbox stamp) │
│    via PR Thread Pass-Through)│  • Docker Non-root (`uid=1000(node)`),     │
│  • Event Batching: 1→40/min,  │    0 access vào $HOME / SSH keys           │
│    latency 6.4s → 1.6s        │  • First real run: ~30s, \$0.10 (Haiku),   │
│  • 23M cache-read tokens/run  │    nằm chờ ở bảng "Needs You" (No auto-merge)│
└───────────────────────────────┴────────────────────────────────────────────┘
```

---

## 2. DeepSeek Thực Sự Đã Xây Dựng Cái Gì? (Frame `0:00` – `0:29`)

**Rob Shocks** mở đầu bằng việc phân biệt rõ hai lớp công nghệ mà DeepSeek vừa tung ra (Frame `0001`, `0002`, `0004`):
1. **Lớp 01 — `DeepSeek Harness` (`THE HARNESS · OPEN SOURCE`):** Bộ khung agent mã nguồn mở xây dựng trên kiến trúc *"Everything is a plugin"* của Cordis, có thể chạy như ứng dụng Desktop hoặc khởi chạy Web UI từ code.
2. **Lớp 02 — `Super-agent` (`THE DESIGN · RUNS ON 01`):** Kho lưu trữ `deepseek-ai/dsh-superagent-prototype` (phát hành ngày 31/10/2026, đạt **12.8k stars** và **1.3k forks** chỉ sau 2 ngày — Frame `0004`, `0012`). Đây là bản thiết kế kiêm nguyên mẫu nghiên cứu chạy bên trên lớp 01.
3. **Tính độc lập công cụ (Frame `0006`, `0007`):** Giá trị lớn nhất nằm ở **các nguyên lý kiến trúc**. Bạn không bắt buộc phải dùng mô hình DeepSeek hay DeepSeek Harness — Rob Shocks đã bóc tách bản thiết kế này để cắm trực tiếp vào **`Claude Code`**, **`OpenCode`** hoặc **`Cursor`** theo triết lý *"Build your own. Run your own."* cho doanh nghiệp mà không bị khóa chặt vào bất kỳ nền tảng độc quyền (proprietary setup) nào.

---

## 3. Điểm Nghẽn "Meat Proxy" & Thang Bậc Vị Trí Con Người (Frame `1:03` – `2:37`)

### Tại sao con người lại trở thành Router bất đắc dĩ? (Frame `0016`)
Khi chạy song song 5 cuộc trò chuyện (`CHAT 01: Fix auth bug`, `CHAT 02: Add tests`, `CHAT 03: Update docs`, `CHAT 04: Refactor API`, `CHAT 05: Review PR`), chính bộ não của bạn phải đóng vai **`Project manager` + `Router`**:
- Mỗi agent mở thêm là một tab phải canh chừng, một bản diff phải đọc, một câu hỏi đang chờ bạn trả lời.
- Gắn thêm MCP servers hay CLIs vào một local single-session agent không giải quyết được gốc rễ: nó vẫn chạy đồng bộ, ngữ cảnh giữa các việc bị trộn lẫn, và khi gập laptop thì mọi thứ chết đứng (Frame `0019`).

### Thang bậc "Where The Human Sits" (Frame `0015`, `0020`, `0021`, `0058`)

```
          ┌─────────────────────────┐
          │        Sign off         │  ▲
          ├─────────────────────────┤  │  MỤC TIÊU DỊCH CHUYỂN LÊN TRÊN:
   (ROB) ─►│      Set the rules      │  │  Product Engineering, đặt luật,
          ├─────────────────────────┤  │  kiểm tra kết quả & ký duyệt
          │    Check the result     │  │
          ├─────────────────────────┤  │
          │     Review the PR       │  │
          ├─────────────────────────┤ ─┼────────────────────────────────
   (ROB) ─►│    Read every diff      │ ◄── [BOTTLENECK HIỆN TẠI]
          ├─────────────────────────┤
          │     Write the code      │  (Đã giao cho Agent)
          └─────────────────────────┘
```

- Trước đây, nút thắt cổ chai nằm ở tầng **`Read every diff`** (Frame `0021`).
- Triết lý của `dsh-superagent` là: **"Take as little human attention as possible, and give as much value as possible"** (Chiếm dụng ít sự chú ý của con người nhất có thể, và mang lại nhiều giá trị nhất có thể — Frame `0060`). Con người không biến mất, mà dịch chuyển lên tầng **`Set the rules`**, **`Check the result`** và **`Sign off`** (Frame `0058`).

---

## 4. Điểm Qua Công Cụ Hỗ Trợ: Cline Desktop & ClinePass (Frame `2:43` – `3:40`)

- **Cline Desktop (`cline.bot/desktop` — Frame `0023` – `0037`):** Ứng dụng mã nguồn mở miễn phí trên Mac, Windows và Linux, hỗ trợ hơn **300 models** từ hơn **50 providers** (DeepSeek V4 Pro, GLM-5.3, Kimi K3, Qwen3.8 Max, MiniMax M3, MiMo-V2.5 Pro, Nemotron 3.5 Super VL free...).
- **Tính năng nổi bật:**
  - Không chỉ dùng để code mà còn làm Research và xử lý file (`Not only for coding · Research`).
  - Import trực tiếp phiên làm việc đang dở từ `Claude Code` hoặc `Codex` sang Cline.
  - Chạy nhiều agents song song (`AGENTS SIDE BY SIDE` — Frame `0032`: một phiên `Checkout feature [BUILD]`, một phiên `Stripe API research [RESEARCH]`).
  - Lập lịch tác vụ tự động (`New schedule` — Frame `0033`: chạy mỗi sáng 7:00 AM tìm kiếm tin tức công cụ mới).
  - Gói **`ClinePass` (\$9.99/tháng — Frame `0036`):** Cho các mô hình open-weight (DeepSeek, Kimi, GLM) với hạn mức sử dụng **gấp 5 lần (`Up to 5x`)** so với API tiêu chuẩn.

---

## 5. Giải Phẫu 6 Cơ Chế Cốt Lõi Của DeepSeek Super-Agent (Frame `3:56` – `9:17`)

### 5.1. Kiến Trúc 2 Tầng & Tuyên Ngôn Xóa Bỏ Session (Frame `0039`, `0040`, `0065`)
- **Tầng trên — `Router` (Frame `0039`):**
  - `TALKS TO HUMANS` (Giao tiếp với người dùng trong 1 khung chat nhóm duy nhất — `One team chat`).
  - `LONG-LIVED · PERSISTENT` (Sống dài hạn, bền bỉ).
  - `DISPATCHES WORK` (Phân phối việc cho các agents bên dưới, không bao giờ tự làm).
  - `✗ NO BASH TOOL` (Không có công cụ bash).
  - `LIGHTER MODEL ■■□□□` (Chỉ cần mô hình nhẹ, tốc độ cao).
- **Tầng dưới — `Workers` (Frame `0065`):**
  - Mỗi Worker nhận **1 ngữ cảnh cô lập**, bộ công cụ đầy đủ, **1 nhiệm vụ duy nhất**, và **mô hình mạnh nhất** bạn có.
  - Không bao giờ nói chuyện trực tiếp với con người mà báo cáo ngược về Router. Nhờ đó, nhóm làm việc thoát khỏi giới hạn `CAP · 3-5 SESSIONS`.
- **Tuyên ngôn của toàn bộ Repository (`THE MANIFESTO` — Frame `0040`):**
  > *"Sessions should be an implementation detail of the system, not a mental burden on its users."*

### 5.2. Thay Thế `INTENT.MD` Bằng Luồng Sự Kiện & Lọc 2 Tầng (Frame `0041` – `0043`)
- So sánh với bài nói về Anthropic SDLC dùng file `intent.md` (Frame `0011`, `0041`) để gom ý định từ Slack/Jira/Sentry: Super-agent chuyển hẳn sang cơ chế **Event-Driven Triggers** (tin nhắn group chat, email, Sentry, Jira, Linear, comment code review, kết quả CI).
- **Lọc nhiễu 2 bước (Frame `0043`):**
  1. `01 · FAST PATH (Code filter)`: Kiểm tra bằng code thuần (`SOURCE` / `GROUP` / `Allowlist`) xem ai gửi và có quyền không.
  2. `02 · SLOW PATH (Prompt policies)`: Chỉ những gì vượt qua bước 1 mới vào model để đối chiếu với quy tắc viết bằng ngôn ngữ tự nhiên.

### 5.3. Đăng Ký Luồng (`Threads and Pass-Through`) Để Giữ Ấm Cache (Frame `0044`)
- Khi một Worker nhận xử lý `PULL REQUEST #482 · OPEN (Fix retry in rate limiter)`, nó tạo kết nối `SUBSCRIBED` trực tiếp vào thread của PR đó.
- Mọi bình luận tiếp theo (`MAYA: Can you add a test?`, `TOM: Rename the flag`, `MAYA: Nit: log the retry count`) **bỏ qua Router (`--✗--> Router`)** và đi thẳng vào Worker đó.
- **Lý do kinh tế (`CONTEXT █████ KEPT WARM`):** Mỗi khi khởi tạo một agent/session mới, bạn phải nạp lại toàn bộ ngữ cảnh từ đầu (cold context drop). Giữ nguyên Worker sống xuyên suốt vòng đời PR giúp **tận dụng tối đa Prompt Cache (`cache tokens`)**, tiết kiệm chi phí lớn.

### 5.4. Hành Động Đi Trước (`Silently Reproduce and Fix`) vs `Fix and Fuzz` (Frame `0046` – `0051`)
- DeepSeek áp dụng chính sách **`"silently reproduce and fix"`** (Frame `0046`): Khi có người báo lỗi trong group chat, Worker âm thầm tái hiện lỗi và tìm bản vá dưới nền; chỉ lên tiếng khi đã xác nhận được lỗi và có bản sửa gọn.
- **Đối chiếu với `Fix and Fuzz` của Lauren Tan (`poteto` — Frame `0047`, `0048`):**
  - Lauren Tan dùng bot `sandcastle` để cắt bản phát hành (release cut), tự động DM cho mọi contributors kèm link PR, sau đó kích hoạt **`automated fuzz swarm`** (hơn 10 agents chạy `Grok 4.7 xhigh` đóng vai người dùng thật và `"chaos monkey"` click ngẫu nhiên tìm lỗi), rồi `@mention` bot kỹ sư `poteto` tạo Cursor Project để vá lỗi và cherry-pick vào nhánh release.
- **Giám sát viên độc lập (`Watchdog · SEPARATE SESSION` — Frame `0051`):** Một phiên Watchdog riêng biệt theo dõi tiến độ của từng Worker (`W1`, `W2`, `W3`) hướng tới `GOAL` để phát hiện các Worker bị kẹt hoặc đi chệch hướng.

### 5.5. Thực Nghiệm Ablation Study & Hiện Tượng Gom Lô Sự Kiện (Frame `0052` – `0055`)
Trong mục **7.1 Ablation Study** (Frame `0052` – `0054`), DeepSeek thử nghiệm 4 thiết kế trên luồng **300 tin nhắn** (chỉ có **~20 yêu cầu công việc thật**, còn lại là chat phiếm, follow-up, hủy lệnh, thông báo CI), chạy lặp lại 5 lần:

| Thiết kế | Thời gian hoàn thành (Median / P95) | Ghi chú từ thực nghiệm |
| :--- | :--- | :--- |
| **Super-agent** | **12.4 phút** (Median) / **15.7 phút** (P95) | Nhanh và ổn định nhất (Frame `0054`). |
| **Single Session + Subagent tool** | **~20 – 29.5 phút** | Dùng prompt trung lập (*"Use a subagent when appropriate..."*) hoặc prompt ưu tiên ủy quyền (*"delegation-first prompt"*). |

- **Bài kiểm tra tải Router (`ROUTER UNDER LOAD · TEST 7.2 — CHOKES?` — Frame `0055`):**
  - Tưởng rằng khi bắn dồn dập sự kiện vào 1 Router duy nhất thì Router sẽ bị nghẽn cổ chai. Nhưng thực tế xảy ra điều ngược lại nhờ **Cơ chế Gom lô (`Event Batching`)**: Các sự kiện dồn ứ trong lúc một lượt gọi đang chạy sẽ được gom chung vào lượt gọi kế tiếp!
  - Khi lưu lượng tăng từ **1 trigger/phút lên 40 triggers/phút**, thời gian xử lý trung bình cho mỗi quyết định **giảm mạnh từ `6.4 giây` xuống chỉ còn `1.6 giây`**.
- **Sự đánh đổi về Token (`Trade-off`):**
  - Vì chạy nhiều phiên độc lập, một lần chạy Super-agent tiêu thụ khoảng **23 triệu cache-read tokens** so với **~1 triệu tokens** của thiết kế 1 phiên đơn lẻ. Bạn đang **đổi lượng token (đã được giảm giá nhờ cache) để lấy thời gian hoàn thành ngắn hơn và ổn định hơn**.

### 5.6. Năm Quy Tắc Cư Xử Trong `000 Philosophy` (Frame `0060`, `0061`)
Tài liệu `docs/000-philosophy.md` định nghĩa 5 kỷ luật vàng cho Agent:
1. **Chỉ hành động khi được yêu cầu trực tiếp hoặc có quy tắc thành văn (`It acts only when asked, or when a written rule allows it`):** Việc không ai yêu cầu mà tự làm thì phải có luật do nhóm viết ra, và ban đầu chỉ chạy ở **chế độ ghi nhận (`starts in a mode that only records what the agent would have done`)**.
2. **Chỉ lên tiếng khi con người cần hành động hoặc dùng được kết quả (`It speaks only when a person must act or can use a result`):** Xác nhận yêu cầu đúng 1 lần kèm tên việc sẽ làm, sau đó im lặng làm việc.
3. **Mỗi lần chỉ hỏi 1 câu hỏi cụ thể (`It asks one concrete question at a time`):** Đặt ở định dạng có cấu trúc rõ ràng thay vì chôn vùi câu hỏi trong một báo cáo dài lê thê.
4. **Dừng lại theo mức độ tác động, không phải theo công sức (`It stops by impact, not by effort`):** Thay đổi cục bộ thì làm thẳng tới kết quả review được; thay đổi cần quyết định thiết kế thì dừng lại kèm câu hỏi rõ ràng.
5. **Báo cáo trung thực (`It reports honestly`):** Bản nháp phải gắn nhãn chưa xong, thất bại báo là thất bại, không bao giờ tự ý vượt rào giới hạn để né việc đặt câu hỏi.

---

## 6. Cảnh Báo Bảo Mật Nghiêm Trọng Của Nguyên Mẫu Gốc (Frame `9:18` – `9:58`)

Rob Shocks chỉ ra 2 tử huyệt bảo mật nếu bạn bê nguyên `dsh-superagent-prototype` lên chạy thật (Frame `0066`, `0067`, `0071`):
1. **`danger-full-access` trên tài khoản OS của người vận hành (Frame `0067`):** Cả hai preset `dsh-superagent-worker` và `dsh-superagent-router` trong nguyên mẫu gốc đều chạy với quyền `danger-full-access` dưới user OS hiện tại, thay đổi `PATH` và cấu hình `git` toàn cục, đồng thời đọc được toàn bộ thư mục `$HOME` của bạn!
2. **Lỗ hổng `LLM01:2025 Prompt Injection` (Frame `0071`):** Càng mở rộng cánh cửa cho agent đọc nhiều nguồn sự kiện không đáng tin cậy (untrusted inputs từ GitHub issues công khai, group chat, plugins bên thứ ba), kẻ tấn công càng dễ cài cắm chỉ thị độc hại (invisible prompt injection) để chiếm quyền điều khiển agent.

---

## 7. Hướng Dẫn Tự Dựng Super-Agent Cho Claude Code / OpenCode Trong Docker Khóa Chặt (Frame `9:59` – `12:15`)

Thay vì cài đặt DeepSeek Harness nặng nề, Rob Shocks đã cho agent đọc tài liệu thiết kế + bộ prompts của DeepSeek để viết lại một phiên bản **TypeScript tinh gọn, độc lập và khóa chặt bảo mật** (Frame `0073` – `0076`):

### 7.1. Cấu Trúc Mã Nguồn TypeScript (`package.json` & `src/` — Frame `0073`, `0074`)
```
deepseek-harness/
├── bin/
│   └── sa                        # CLI cổng kiểm duyệt duy nhất để Agent gửi thông điệp ra ngoài
├── docker/
│   └── entrypoint.sh             # Script khởi chạy container cô lập
├── prompts/
│   └── team/
│       ├── host.md               # System prompt cho Router (Host)
│       └── worker.md             # System prompt cho Worker
├── src/
│   ├── adapters/
│   │   ├── claude-code.ts        # Adapter cắm trực tiếp Claude Code (@anthropic-ai/claude-code)
│   │   ├── opencode.ts           # Adapter cắm OpenCode (đổi qua lại chỉ bằng 1 dòng config)
│   │   ├── index.ts
│   │   └── types.ts
│   ├── config.ts                 # Cấu hình đọc từ superagent.config.json (Zod validation)
│   ├── domain.ts                 # Định nghĩa Ticket, Trigger, Thread, Pull
│   ├── engine.ts                 # Lõi điều phối: subscribe(t.id, thread, "pull"), pullsDone(), replyTarget()
│   ├── executor.ts               # Quản lý tiến trình Worker & Git Worktree riêng cho từng Ticket
│   ├── gate.ts                   # Cổng lọc bằng Code (Allowlist) chạy TRƯỚC mọi lệnh gọi LLM
│   ├── main.ts                   # CLI entry: start | doctor | status
│   ├── outbox.ts                 # Kiểm duyệt tin nhắn đầu ra, đóng dấu "[Automated Agent]"
│   ├── proc.ts                   # Quản lý child processes
│   ├── server.ts                 # Web panel phục vụ bảng "Needs You"
│   └── store.ts                  # Lưu trữ trạng thái bền vững
├── Dockerfile                    # Image node:24-bookworm-slim + claude-code@2.1.316
└── docker-compose.yml
```

### 7.2. Năm Chốt Chặn An Ninh & Vận Hành Thực Chiến (Frame `0074` – `0076`)
1. **Cổng kiểm tra bằng Code cứng (`gate.ts`):** Khi có người nhắc tên bot trên GitHub Issue, `gate.ts` kiểm tra xem user đó có nằm trong `allowlist` hay không **trước khi gọi bất kỳ mô hình nào**. Không một dòng chữ nào viết trong comment có thể tự cấp quyền (chống Prompt Injection leo thang đặc quyền).
2. **Cách ly Ticket bằng Branch + Worktree + Session riêng:** Mỗi yêu cầu hợp lệ trở thành 1 `Ticket` có nhánh Git riêng, thư mục worktree riêng và phiên agent riêng. Mọi comment phản hồi trên cùng luồng sẽ đi thẳng về đúng phiên đó (`this.store.subscribe(t.id, thread, "pull")` trong `engine.ts` dòng 408 — Frame `0074`).
3. **Cổng kiểm duyệt đầu ra qua lệnh `sa` (`bin/sa` & `outbox.ts`):** Agent **không có quyền tự đăng bài lên GitHub**. Nó chỉ được phép gọi một shell command nhỏ tên là `sa`. Tiến trình Host sẽ kiểm tra từng tin nhắn, ép độ dài ngắn gọn, và đóng dấu rõ ràng là được viết bởi tác nhân tự động.
4. **Đóng gói Docker Non-Root tuyệt đối (`Frame 0075`):**
   - Chạy trong container `node:24-bookworm-slim` dưới quyền user thường **`uid=1000(node) gid=1000(node)`**.
   - **Không có quyền truy cập vào thư mục `$HOME` hay SSH keys** của máy chủ.
   - Sử dụng một tài khoản GitHub Bot riêng biệt với Fine-grained Token chỉ được phép chạm vào đúng 1 repository sandbox được chỉ định.
5. **Chặn quyền tự Merge & Bảng điều khiển `"Needs You"` (`Frame 0076`):**
   - Trong lần chạy thực tế đầu tiên (`FIRST REAL RUN · ISSUE #1 -> PR #2`, từ `15:04:56` đến `15:05:25`), Rob mở một issue yêu cầu sửa lỗi chính tả trong `README.md`.
   - Agent phản hồi đúng 1 lần, sửa lỗi, chạy bộ test và mở Pull Request trong **~30 giây End-to-End** với chi phí chỉ **~10 cents (\$0.10)** khi dùng mô hình **Haiku** qua `Claude Code`.
   - Agent bị cấm tự merge PR của chính nó — kết quả nằm yên trong bảng điều khiển ở mục **`"Needs You"`** để con người rảnh tay vào duyệt bất cứ lúc nào.

---

## 8. Liên Kết Chéo Trong Kho Lưu Trữ (Cross-References)

- [DeepSeek Tiết Lộ Thiết Kế Super Agent: Cách Xây Dựng Tác Nhân Kiểu GrokBot Dưới Góc Nhìn Hệ Thống Phân Tán — lustoykov](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_super_agent_design_grokbot_agents_lustoykov.md) — Phân tích chi tiết 5 khối kiến trúc lý thuyết (`Two-Level Architecture`, `Chain of Recall`, `Full-Duplex ReAct`, `Endpoint Daemons`).
- [Thực Chiến DeepSeek SuperAgent Harness — AICodeKing](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_superagent_harness_hands_on_aicodeking.md) — Phân tích bài toán chi phí 23M cached tokens (\$0.07 trên DeepSeek Flash vs \$4.64 trên Sonnet) và thiết lập qua PandaCode Bots.
- [DeepSeek Open-Source Super-Agent DSH — TEKTHRILL](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_open_sources_super_agent_dsh_tekthrill.md) — Tổng quan nhanh về nguyên mẫu `dsh-superagent`.
- [LIVE: Poteto on Shipping 1,000s of PRs a Month (Matt Pocock x Lauren Tan)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/live_poteto_pstack_shipping_thousands_prs_spacex_matt_pocock.md) — Kiến trúc `Fix and Fuzz` và `sandcastle` + `poteto` bot mà Rob Shocks trích dẫn ở phút `06:31`.
