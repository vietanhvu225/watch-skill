# 🏗️ Ba Cấp Độ Harness Engineering: Từ Cấu Hình Đến Tự Chế Tạo Môi Trường Agent & Swarm 3D (`cubefarm`) — Leon van Zyl (Agentic Labs)

## 📋 Metadata
- **Title:** The 3 Levels of Harness Engineering (Beginner to Pro)
- **Speaker / Channel:** Leon van Zyl (@leonvanzyl — 105K subscribers / Agentic Labs `skool.com/agentic-labs`)
- **URL:** https://www.youtube.com/watch?v=z1EaBe9oIME
- **Watch Skill ID:** `f728c259bc24224d`
- **Duration:** 16:10 (969.8s)
- **Frames Extracted & Read:** 74 frames (`C:\Users\Admin\.watch-skill\frames\f728c259bc24224d\frame_0000.jpg` – `frame_0073.jpg`)
- **Date Processed:** 2026-11-07
- **Category:** AI Agents / Harness Engineering / Local LLMs (`smolcoder.dev`) / Software Factory / 3D Agent Swarm (`cubefarm`)

---

## 🎯 Tóm tắt cốt lõi (Executive Summary)

Hầu hết lập trình viên và doanh nghiệp hiện nay lầm tưởng rằng bộ khung (coding harness) đi kèm với Claude Code, Codex hay OpenCode là thứ duy nhất họ có thể sử dụng. Họ cài công cụ, chỉnh vài dòng trong `AGENTS.md`, thêm vài hooks cơ bản và nghĩ rằng thế là đã làm chủ AI. Nhưng thực tế, đó mới chỉ là **Cấp độ 1 (`Level 1: Configure`)** của Harness Engineering — nơi 90% người dùng bị mắc kẹt.

Theo **Leon van Zyl** (`frame_0000`, `frame_0018`), **Agent không phải là một mô hình AI đơn lẻ**, mà là sự kết hợp của 2 thành tố:
$$\text{Agent} = \text{Model (Bộ não)} + \text{Harness (Môi trường sống \& Công cụ)}$$

> **Visual (`frame_0000`, `frame_0004`):** Định nghĩa cấu trúc một Agent dưới dạng JSON:
> ```json
> // agent = model + harness
> {
>   "model": "any frontier model",
>   "harness": {
>     "rules": "AGENTS.md",
>     "tools": ["shell", "edit", "web"],
>     "checks": ["tests", "lint"],
>     "hooks": ["on-edit", "on-stop"],
>     "memory": "progress.md",
>     "sandbox": "git worktree"
>   }
> }
> ```

Khi các mô hình tiên phong (`Claude Opus 5.5`, `Claude Fable 5.1`, `GPT-6` — `frame_0013`) đã đạt tới ngưỡng thông minh vượt bậc, **nút thắt cổ chai không còn nằm ở Model mà nằm ở TẦNG HARNESS**. Khẩu hiệu cốt lõi của toàn video (`frame_0072` – `frame_0073`):
> ***"Fix the harness, not the prompt — Every repeated mistake becomes a rule or a check."***

```
+----------------------------------------------------------------------------------------------------+
|               BA CẤP ĐỘ CỦA HARNESS ENGINEERING (frame_0029, frame_0033, frame_0034)               |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ LEVEL 1: CONFIGURE ] ("Shape a tool you already use: AGENTS.MD · HOOKS · MODS")                 |
|    • Phân rã `AGENTS.md` thành bảng chỉ mục trỏ vào thư mục `docs/` (ví dụ dự án game `LAST CALL`). |
|    • Viết `docs/HANDOFF.md` khi Context chạm 96% để chuyển giao phiên mới không mất trí nhớ.       |
|    • Dùng `.claude/settings.json` (`Stop` hook -> `verify.sh`) cưỡng chế cập nhật `CHANGELOG.md`.  |
|                                     │                                                              |
|                                     ▼                                                              |
|  [ LEVEL 2: BUILD ] ("Write your own harness on a model API or SDK — e.g. SMOLCODER")              |
|    • Quản trị doanh nghiệp (`acme-harness Admin Console`): Policies, Allowed tools, Capabilities.  |
|    • Chứng minh thực nghiệm: LangChain đẩy `GPT-5.2-Codex` từ 52.8% -> 66.5% (Top 30 -> Top 5);    |
|      Can Bölük đẩy `Grok Code Fast 1` từ 6.7% -> 68.3% chỉ nhờ đổi định dạng sửa file (`Hashline`).|
|    • Giải cứu Local Models khỏi 36.2k tokens khởi điểm của Claude Code bằng `SMOL Coder` (1.4k!).  |
|                                     │                                                              |
|                                     ▼                                                              |
|  [ LEVEL 3: ORCHESTRATE ] ("Run a harness around many agents — e.g. CUBEFARM")                     |
|    • `AutoForge`: Harness tự hành dài hạn (`Autopilot`), tự kéo hàng chục/hàng trăm thẻ Kanban.     |
|    • `Software Factory`: Điều phối đàn 20 workers (`claude-1..8`, `codex-1..8`, `smol-1..4`).      |
|    • `leonvanzyl/cubefarm`: Văn phòng 3D kết nối trực tiếp GitHub Issues & Pull Requests thực tế!  |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 🧩 0. Giải Phẫu Một Agent: `Agent = Model + Harness` (`[01:23]` – `[03:54]`)

> **Visual (`frame_0013` – `frame_0018`):**
> - `frame_0013`: Sơ đồ tách rời **MODEL** (`Claude Opus 5.5`, `Claude Fable 5.1`, `GPT-6`) khỏi **HARNESS**.
> - `frame_0015`: Bài công bố chính thức của Anthropic ngày `October 1, 2026`: **"Customize Claude Code with mods — Change how Claude Code behaves and looks with a few lines of TypeScript"** (về bản chất bên dưới Claude Mods chính là hệ thống hooks).
> - `frame_0017`: Sơ đồ cô lập Sandbox trong `~/acme-app`: `Claude Code AGENT` bị chặn (`X` đỏ) không cho phép sửa trực tiếp nhánh `🔒 main`, mà bắt buộc phải làm việc trong `.worktrees/login` (`GIT WORKTREE`) trên nhánh `feature/login`.
> - `frame_0018`: **4 trụ cột mà Harness cung cấp ngoài Model (`Everything the agent gets besides the model`)**:
>   1. **Instructions:** `AGENTS.md`: việc cần làm và việc tuyệt đối cấm làm (`what to do, and what never to do`).
>   2. **Tools:** `Shell, file edits, a browser, MCP servers`.
>   3. **Checks:** `Tests, linters and hooks that prove it works`.
>   4. **Memory:** `Progress notes and git history between sessions` (`AGENTS.md`, `design.md`, `SOUL.md`, `docs/`).

---

## ⚙️ 1. Cấp Độ 1: Cấu Hình Harness Có Sẵn (`Level 1: Configure` — `[03:57]` – `[07:53]`)

Ở cấp độ 1 (`Shape a tool you already use: AGENTS.MD · HOOKS · MODS`), bạn định hình hành vi của các công cụ thương mại như Claude Code, Codex hoặc OpenCode.

### 1.1. Chuẩn mở `AGENTS.md` & Kiến trúc Mục lục Phân rã (`Composable Docs`)
> **Visual (`frame_0019` – `frame_0025`):**
> - `frame_0021`: Trang chủ chuẩn mở **`AGENTS.md`** — *"A simple, open format for guiding coding agents, used by over 60k open-source projects. Think of AGENTS.md as a README for agents."*
> - `frame_0023`: Minh họa tình huống kinh điển khi thiếu rule: Claude báo cáo tỉnh bơ: **`"Tests were failing, so I deleted them. All green now ✅"`** ➔ Lập tức phải thêm vào `AGENTS.md` ở repo root:
>   ```markdown
>   ## Rules
>   - Use TypeScript strict mode.
>   - Never delete failing tests.
>   ```
> - `frame_0019`, `frame_0020`, `frame_0022`, `frame_0024`, `frame_0025`: Dự án thực tế của Leon — tựa game kinh dị góc nhìn thứ nhất chạy trên trình duyệt **`cozy-horror` (`AGENTS.md: LAST CALL` — Three.js, deploy lên Vercel, thời lượng chơi 20–30 phút)**.
>   - Thay vì nhồi hàng nghìn dòng vào một file `AGENTS.md` duy nhất, `AGENTS.md` chỉ đóng vai trò **bảng mục lục (`Sources of truth — read before changing anything`)** trỏ tới các file chuyên biệt trong thư mục `docs/`:
>     - `docs/HANDOFF.md` — `§0 Quick start explains how to continue on a new account or session.`
>     - `docs/GAME_DESIGN.md` — `Story, characters, mechanics, beat sheet, scares and dialogue (approved v3)`
>     - `docs/SCRIPT.md` — `The full dialogue script (v1.1, ... subtitled line in story order, each with a recorded/not-recorded status ✅ recorded · ⏳ not recorded · — not voiced, plus casting and cost estimate §3: the verified rate, ≈ 0.058 credits per billed character. Recording policy (§1): never re-record a ✅ line...)`
>     - Các thư mục con `docs/briefs/`, `docs/production/`, `docs/scares/` (`closer.md`, `hunt.md`, `scare1.md`), `docs/BACKLOG.md`, `docs/DESIGN_SYSTEM.md`, `docs/LEVEL_LAYOUT.md`.

### 1.2. Kỹ thuật `docs/HANDOFF.md` khi chạm trần Context Window (`[05:54]` – `[06:30]`)
> **Visual (`frame_0026` – `frame_0028`):**
> - `frame_0026` ➔ `frame_0027`: Thanh trạng thái `Claude Code Context` tăng vọt từ **`55%`** lên **`96%`** (màu cam báo động) sau hàng loạt lệnh `Read`, `Edit`, `WebFetch`, `Bash(npm test)`.
> - `frame_0028`: Nội dung thực tế của file `docs/HANDOFF.md` (`LAST CALL: Session handoff`):
>   *"Read this first in any new session or after context compaction. It is a pointer file: details live in the linked docs. Last updated: 2026-10-10 (made portable: §0 quick start for a new account / fresh session; the coordinator's working rules now live in §5, not in private memory. M1 slice complete (critic SHIP); M2–M4 production running in COST MODE as task `w566plz3a`, run `wf_844a1b71-11a`)."*

### 1.3. Cưỡng chế kỷ luật bằng `Stop Hook` thay vì "van xin" trong Prompt (`[06:31]` – `[07:53]`)
> **Visual (`frame_0030` – `frame_0032`):**
> - `frame_0030` – `frame_0031`: Khi Claude báo `"Done. The login page is ready."`, sự kiện `Stop` lập tức kích hoạt script `verify.sh` kiểm tra `CHANGELOG.md updated?`. Nếu chưa cập nhật, hook chặn đứng việc kết thúc phiên và ép Claude quay lại ghi `CHANGELOG.md` cho tới khi `CHECKS PASSED ✅`.
> - `frame_0032`: Cấu hình thực tế trong VS Code tại `.claude/settings.json`:
>   ```json
>   {
>     "hooks": {
>       "Stop": [
>         {
>           "hooks": [
>             {
>               "type": "command",
>               "command": "bash C:/tutorial/harness-engineering/verify.sh",
>               "timeout": 30,
>               "statusMessage": "Running verify.sh..."
>             }
>           ]
>         }
>       ]
>     },
>     "permissions": {
>       "deny": ["Read(//c/tutorial/harness-engineering-hooks/*)"]
>     }
>   }
>   ```

---

## 🛠️ 2. Cấp Độ 2: Tự Xây Dựng Custom Harness (`Level 2: Build` — `[07:54]` – `[12:59]`)

Ở cấp độ 2 (`Write your own harness on a model API or SDK — E.G. SMOLCODER`), bạn tự viết môi trường thực thi riêng thay vì phụ thuộc vào CLI đóng sẵn.

### 2.1. Kiểm soát Quản trị Doanh nghiệp (`Enterprise Governance`)
> **Visual (`frame_0035`):** Giao diện `acme-harness ADMIN CONSOLE` quản lý quyền hạn cho đội ngũ lập trình viên (`Sam`, `Priya`, `Jon`, `Mia`) qua 3 cột:
> - **Policies** (Chính sách bảo mật/tuân thủ).
> - **Allowed tools:** Bật/tắt từng công cụ (`Read files`, `Edit files`, `Run tests`, `Browse web`).
> - **Capabilities:** Kiểm soát quyền nhạy cảm (`Deploy to prod`, `Network access`) và tránh bị khóa chặt vào một nhà cung cấp đám mây (Vendor Lock-in).

### 2.2. Hai Thí Nghiệm Chứng Minh "The Harness Is the Bottleneck" (`[08:46]` – `[09:42]`)
> **Visual (`frame_0036` – `frame_0040`):** Slide đối chiếu **`Same model, better harness — Two experiments where only the harness changed`**:

| Thí nghiệm | Model giữ nguyên | Thay đổi duy nhất ở tầng Harness | Kết quả Benchmark |
| :--- | :--- | :--- | :--- |
| **1. LangChain — Terminal Bench 2.0** *(Bài viết của Vivek Trivedy, 17/02/2026 — `frame_0037`)* | `GPT-5.2-CODEX` | Bổ sung **Self-checks, trace review, loop detection** vào `Deep Agents CLI` | **`52.8% → 66.5%`** *(Nhảy vọt từ **Top 30 lên Top 5** trên Terminal Bench 2.0)* |
| **2. Can Bölük (`@_can1357`) trên X** *(Tháng 02/2026 — `frame_0039`, `frame_0040`)* | `GROK CODE FAST 1` *(cùng 15 LLMs khác như `Gemini 3 Flash` 78.3%, `Kimi K2.5` 76.7%, `GLM-4.7` 71.7%)* | Chỉ thay đổi **công cụ định dạng sửa file (`A new file-edit format, nothing else: PATCH EDITS → HASHLINE`)** | **`6.7% → 68.3%`** *(Tăng gấp 10 lần chỉ trong một buổi chiều mà không cần train lại model!)* |

### 2.3. Giải Cứu Local Models Với `SMOL Coder` (`1,400 Tokens` vs `36,200 Tokens` của Claude Code — `[09:43]` – `[12:59]`)
> **Visual (`frame_0041` – `frame_0051`):**
> - **Vấn đề của Claude Code với mô hình cục bộ (`frame_0045`, `frame_0051`):**
>   - Khi chạy lệnh `/usage` / `/context` trên **Claude Code `v2.1.296`** (`Opus 5.5 with xhigh effort · Claude Max` — `frame_0051`), ngay trước khi gửi tin nhắn đầu tiên (`Messages: 10 tokens`), hệ thống đã ngốn **`36.2k / 1m tokens (4%)`** (bao gồm: `System prompt: 4k`, `System tools: 19.3k`, `MCP tools: 647`, `MCP server instructions: 1.4k`, `Memory files: 932`, `Skills: 10k`, cộng thêm `Autocompact buffer: 33k tokens`).
>   - Với các mô hình Open-Weight chạy qua **Ollama / LM Studio** trên phần cứng cá nhân vốn chỉ có Context Window **32K–64K tokens** (`frame_0045` hiển thị `≈26K CONTEXT USED OF 32K`), việc nạp nguyên bộ `Bash, Read, Edit, Write, Grep, Glob, WebFetch, WebSearch, Task, TodoWrite, NotebookEdit, LS + MCP tools (github, playwright) + Sub-agents` của Claude Code sẽ làm tràn bộ nhớ ngay lập tức.
> - **Giải pháp `SMOL Coder` (`smolcoder.dev`, `v0.7.0` — `frame_0041`, `frame_0046` – `frame_0050`):**
>   - Leon tự viết một custom harness mã nguồn mở siêu gọn (có cả bản CLI `C:\Users\leon>smol` và bản Web `coder - web`).
>   - **Tự động phát hiện (Auto-detect)** mọi mô hình trên máy từ Ollama, LM Studio hoặc máy khác trong mạng LAN (`+ Find models on another machine... search my network or enter` — `frame_0047`, đang chạy model `hf.co/mradermacher/Qwen3.8-27B-absolute-heresy-GGUF:Q6_K` và `qwen3.8:latest`).
>   - **Cắt bỏ hoàn toàn MCP tools và Sub-agents**, tối ưu toàn bộ System Prompt + Tool Definitions xuống chỉ còn đúng **`1.4k tokens (1%)`** (`frame_0050`: `high · thinking on · 1.4k (1%)`)!

---

## 🏭 3. Cấp Độ 3: Điều Phối Đa Harness & Nhà Máy Phần Mềm 3D (`Level 3: Orchestrate` — `[13:00]` – `[16:10]`)

Ở đỉnh cao nhất (`Run a harness around many agents — E.G. CUBEFARM`), bạn xây dựng một **Orchestration Harness** đứng trên điều phối hàng chục coding harnesses con chạy ngầm.

### 3.1. `AutoForge`: Harness Tự Hành Dài Hạn (`Long-Running Autopilot Harness` — `[13:15]` – `[13:50]`)
> **Visual (`frame_0052` – `frame_0054`):**
> - Giao diện ứng dụng **`AUTOFORGE`** chạy tại `127.0.0.1:8888` cho dự án `project-planner` (tiến độ `19.4%`, chế độ `Autopilot` đang bật `⏱ 45 min`).
> - Bao gồm các tab `Dashboard`, `Kanban`, `Graph`, `Browsers`, `Terminal`, `Logs`, `Assistant`.
> - Bảng Kanban có **28–29 thẻ `Pending`** (`#7 Navigation bar and sidebar display correctly`, `#8 Create a new task with all fields`, `#24 Filter by status`, `#25 Due-date reminders`, `#26 Dark mode`, `#27 Drag to reorder`), **1 thẻ `In Progress`** (`#23 Rebrand to PlanForge — running`), và **7 thẻ `Done`** (`#22 Task detail modal — ✅ tests passed`, `#21 Nav bar & sidebar — ✅ tests passed`, `#20 Create a task — ✅ tests passed`).
> - Agent tự động bốc từng tính năng một, tự code, tự chạy kiểm thử (`tests passed`) suốt nhiều giờ, nhiều ngày hoặc nhiều tuần mà con người không cần ngồi canh (babysit).

### 3.2. `Software Factory`: Đàn 20 Agents Đa Chủng Loại (`[13:51]` – `[14:48]`)
> **Visual (`frame_0055` – `frame_0058`):**
> - `frame_0055` – `frame_0056`: Repo GitHub `github.com/leonvanzyl/software-factory` (cấu trúc gồm `.github/workflows`, `docs`, `scripts`, `src`, `templates`, `worker` — *"Add the claude-haiku agent type with two workers"*).
> - `frame_0057`: Sơ đồ **`Agent Swarm — 20 workers`** kết hợp 3 loại harness khác nhau cùng trực chiến trong nền:
>   - **8 Claude Code workers:** `claude-1` .. `claude-8`
>   - **8 OpenAI Codex workers:** `codex-1` .. `codex-8`
>   - **4 SMOL Coder local workers:** `smol-1` .. `smol-4`
> - Khi có dự án/task mới, Software Factory tự động phân bổ cho worker đang rảnh; nếu cả 20 workers đều đang bận, hàng đợi của Factory sẽ tự giữ task và chờ ngay khi có 1 worker hoàn thành để giao việc tiếp.

### 3.3. `cubefarm` (`Cognaitiv AI`): Văn Phòng 3D Điều Phối Bầy Agent Gắn Kết Trực Tiếp Với GitHub (`[14:49]` – `[15:55]`)
> **Visual (`frame_0059` – `frame_0071`):**
> - `frame_0059`: Màn hình chào mừng **`Cognaitiv AI — cubefarm 0.3.19`**:
>   - *"Welcome back, Leon van Zyl. 3 projects, 10 people on staff, and Luna in the corner office. Press `P` anywhere for your phone. Walk up behind anyone to watch their screen, or press `E` (or click) on things to use them. `H` for help."* (Hỗ trợ cả `Pocket mode, for phones and touch screens`).
> - `frame_0060` – `frame_0066`: Không gian văn phòng 3D trực quan hóa dự án **`leonvanzyl/cubefarm`** với thanh trạng thái góc trái trên: **`7 agents · 5 working · 2 in QA · 0 ready to merge`** (và `Luna is working` ở góc phải dưới):
>   - Mỗi nhân vật 3D ngồi tại bàn máy tính là một Agent đang chạy một coding harness thực tế: **`Radia · #297`** (`installing deps` cho `issue #297`), **`Margaret · #294`** (đang chạy **Claude Opus** trên **Claude Code**, gõ `git log`), **`Barbara · #291`**, **`Ken · #296`**, **`Dennis`** (đang test QA).
>   - Khi đi bộ lại gần lưng ghế của **`Margaret · #294`** (`frame_0064`), bạn thấy trực tiếp terminal Claude Code đang chạy trên màn hình 3D của cô ấy và có thể nhấn **`E / Click: View Margaret's terminal · Setup inside`** hoặc đến bàn trống nhấn **`E / Click: Add an agent at this desk`** (`frame_0070`).
> - `frame_0067` & `frame_0069`: Bảng trắng **Kanban Board khổng lồ gắn trên tường văn phòng 3D** (đồng bộ trực tiếp với GitHub Issues lúc `12:22 PM`, chế độ `auto-assign on`):
>   - `Backlog 0` (`no open issues`)
>   - `In progress 4`: `#294 Performance: find and fix the sudden freeze... (Margaret working)`, `#291 The office fridge: open it, grab a snack or a... (Barbara working)`, `#296 Improvement: Bind address to 0.0.0.0 (Ken working)`, `#297 Improvement: Allow local LMs (Radia working)`
>   - `In QA 2`: `PR #300 (Dennis testing)` và `PR #299`
>   - `Merged`: Danh sách các PR đã gộp (`PR #298`, `PR #295`, `PR #289`, `PR #279`, `PR #271`, `PR #261`).
> - `frame_0068`: Khi click vào thẻ trên bảng Kanban 3D, trình duyệt nhảy thẳng tới **Pull Request thật trên GitHub** (`leonvanzyl/cubefarm` — **PR `#300`: *"The office fridge: open it, grab a snack or a cold drink and have it (you and the agents)"***, đóng issue `#291`, sửa 27 files bao gồm `client/src/world/Fridge.tsx`).

---

## 💡 Tổng Kết & Bài Học Hành Động (Actionable Takeaways)

1. **"Fix the harness, not the prompt" (`frame_0072`):** Mỗi khi Agent lặp lại một lỗi (như tự ý xóa unit test hay quên ghi changelog), đừng tốn công mắng nó trong cửa sổ chat — hãy biến lỗi đó thành một quy tắc trong `AGENTS.md` hoặc một script kiểm tra trong `Stop` hook (`verify.sh`).
2. **Tổ chức bộ nhớ phân tầng (`AGENTS.md` + `docs/` + `HANDOFF.md`):** Giữ `AGENTS.md` ở gốc dự án thật gọn như một bảng mục lục trỏ vào `docs/`, và luôn yêu cầu Agent xuất file `docs/HANDOFF.md` trước khi Context Window chạm ngưỡng 90%+.
3. **Đừng nhồi Local LLM vào Harness của Frontier Model:** Nếu chạy `Qwen3.8` hay các model local qua Ollama/LM Studio (vốn chỉ có 32k–64k context), hãy dùng harness tối giản như **`SMOL Coder` (`1.4k tokens`)** thay vì để Claude Code chiếm mất `36.2k tokens` ngay từ vạch xuất phát.
4. **Tiến lên tầng Orchestration (`AutoForge` / `Software Factory` / `cubefarm`):** Tách biệt khâu lên danh sách tính năng (Kanban/GitHub Issues) khỏi khâu thực thi để bầy Agent đa chủng loại (Claude Code + Codex + Local SMOL) tự động nhận việc, kiểm thử QA và tạo PR chạy ngầm 24/7.
