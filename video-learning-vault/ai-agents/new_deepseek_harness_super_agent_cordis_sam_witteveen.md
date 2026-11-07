# Giải Phẫu Kiến Trúc DeepSeek Harness & Cordis Framework: Tại Sao "Mọi Thứ Đều Là Plugin" Là Tương Lai Của Super Agent | Sam Witteveen

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=OrcsMi2-IcM)
- **Video ID:** `OrcsMi2-IcM`
- **Watch Skill ID:** `50662c16e15cb814`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 17:52
- **Speaker:** Sam Witteveen (Chuyên gia AI/LLM & Đồng dẫn chương trình podcast *Beyond the Pilot* — Giải phẫu kiến trúc Cordis IoC `arXiv:2608.25512`, bảng xếp hạng `FrontierHarness Eval` và nguyên mẫu `deepseek-ai/dsh-superagent-prototype`)
- **Visual Frames:** 87 khung hình trích xuất tại `C:\Users\Admin\.watch-skill\frames\50662c16e15cb814\` (`frame_0000.jpg` – `frame_0086.jpg`)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Khi DeepSeek công bố nguyên mẫu **Super Agent** (`deepseek-ai/dsh-superagent-prototype`, đi kèm bài báo *"Exploring Super-agent Design: Towards a More Autonomous Agent System"*, `frame_0008`–`frame_0009`) — một tác nhân tự động canh gác kho mã nguồn GitHub và ứng dụng Feishu để biến các Issue như `Issue #482: Checkout test fails on Safari` thành Pull Request hoàn chỉnh `PR #483 (+12 -3, Ready for review)` mà bạn **không bao giờ phải mở khung chat** (`"you never open a chat"`, `frame_0001`–`frame_0002`) — phần lớn cộng đồng chỉ chú ý đến tính năng bề nổi. Tuy nhiên, chuyên gia **Sam Witteveen** chỉ ra rằng điều thực sự chấn động nằm ở **cách thức họ xây dựng nó** (`frame_0003`, `frame_0052`, `frame_0084`):

> **Toàn bộ hệ thống Super Agent phức tạp này được lắp ghép hoàn toàn từ một file cấu hình `cordis.patch.yml` và vài plugins thả vào `deepseek-ai/deepseek-harness` (ra mắt 13/08/2026, MIT License, ~279.6k stars) mà KHÔNG HỀ CHẠM VÀO LÕI (`core untouched / No fork needed`)!**

Để làm được điều đó, DeepSeek đã xây dựng Harness của mình dựa trên **Cordis** (`arXiv:2608.25512`, `frame_0038`, `frame_0085`) — một framework lập trình của 3 nhà nghiên cứu thuộc **Đại học Bắc Kinh (Peking University)** về **Khả năng hợp thành Không-Thời gian (Spatio-Temporal Composability)**.

Ngoài ra, Sam Witteveen công bố số liệu thực nghiệm từ bộ quy chuẩn **`FrontierHarness Eval`** (`frontierharness.org` do Runta thực hiện, `frame_0021`–`frame_0030`, `frame_0085`): Khi giữ cố định cùng một mô hình (**Kimi K3**) và chạy qua **9 Harnesses / 12 cấu hình** trên cùng **30 tác vụ** (`21 Terminal-Bench` + `9 DeepSWE`), **DSH Creator đạt tỷ lệ đỗ ngang ngửa Claude Code (`63.3%` = 19/30 bài) nhưng chi phí trung vị mỗi bài đỗ chỉ bằng gần 1/6 (\$3.28 so với \$18.34 — chênh lệch 5.6×)**. Khoảng cách chi phí giữa cấu hình rẻ nhất (`Exo Harness` \$1.05) và đắt nhất (`Claude Code` \$18.34) lên tới **17.5 lần**, còn tỷ lệ đỗ chênh tới **16.7 điểm phần trăm** (`OpenCode` 50.0% vs `Codex` 66.7%) — chứng minh đanh thép nhận định của CTO Instacart và CTO Box rằng ở thời điểm hiện tại, **Harness quan trọng hơn cả Model** (`frame_0016`–`frame_0018`).

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│               TRADITIONAL MONOLITHIC HARNESS VS. DEEPSEEK CORDIS PLUGIN TREE                    │
├─────────────────────────────────────────────────────────────────────────────────────────────────┤
│  TRADITIONAL AGENT HARNESS (No Clean Unload — `frame_0039`–`frame_0040`):                       │
│     ┌────────────────────────────────────────────────────────┐                                  │
│     │ MONOLITHIC CORE (Agent Loop + Model Call + Session DB) │                                  │
│     └──────┬─────────────────────────────┬───────────────────┘                                  │
│            ├──► [old flag / tool-web]    └──► [model A cfg / old hook — "who cleans these up?"] │
│     ➔ Muốn đổi sang kiến trúc Super-Agent? Phải FORK và đập đi xây lại toàn bộ Core!            │
│                                                                                                 │
│  DEEPSEEK HARNESS + CORDIS (`arXiv:2608.25512` — "There is no privileged core to patch"):       │
│     [MỌI THÀNH PHẦN ĐỀU LÀ PLUGIN NGANG HÀNG — `frame_0034`, `frame_0047`, `frame_0084`]:       │
│     ├── `model adapter`   ├── `tool registry`   ├── `session log`     ├── `agent loop`          │
│     ├── `tool-fs`         ├── `tool-bash`       ├── `persona`         └── `skills`              │
│                                                                                                 │
│     4-LAYER BOOT COMPOSITION (`$ dsh --profile web --dump-config` — `frame_0041`–`frame_0043`): │
│     [1. bundles] ──► [2. profile · cordis.patch.yml] ──► [3. home patch] ──► [4. --patch CLI]   │
│                                                                                                 │
│     • Temporal Composability: Gỡ `tool-jobs` ➔ tự động xóa sạch `job_output/job_list/job_kill`. │
│     • Spatial Composability : `agent loop (active)` ──needs──► `model adapter` (Reactive DAG).  │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Cuộc Tranh Luận Lớn: Model Quan Trọng Hơn Hay Harness Quan Trọng Hơn? (`02:50 – 05:52`, `frame_0016`–`frame_0030`)

Trong các cuộc phỏng vấn trên podcast *VentureBeat: Beyond the Pilot* (do Outshift by Cisco tài trợ) với **CTO của Instacart** và **CTO của Box** (`frame_0016`), khi Sam Witteveen đặt câu hỏi *"What's more important, the model or the harness?"*, cả hai vị CTO đều nghiêng hẳn bàn cân về phía **Harness** (`frame_0018`).

### Bằng Chứng Thực Nghiệm: Bảng Xếp Hạng `FrontierHarness Eval` (`frontierharness.org`)
Thí nghiệm kiểm soát chặt chẽ trên `FrontierHarness Eval` (`frame_0022`–`frame_0030`):
- **Biến cố định:** Duy nhất 1 mô hình (**`Kimi K3`**), cùng 1 môi trường runtime, giải quyết cùng **30 bài toán** (`21 Terminal-Bench` + `9 DeepSWE`, mỗi bài tương ứng `≈ 3.3 points`).
- **Biến thay đổi:** Chạy qua **9 coding harnesses** gồm `Codex`, `Claude Code`, `Pi`, `OpenCode`, `Kimi Code`, `Exo Harness`, `Oh My Pi`, `Hermes`, và `DeepSeek Harness` (chạy cả 4 chế độ `×4 DSH modes` tạo thành tổng cộng **12 configurations**, `frame_0022`).

| Tên Harness & Cấu Hình | Mô Hình | Tỷ Lệ Đỗ (Pass Rate) | Chi Phí Trung Vị / Bài Đỗ | Điểm Nhấn Từ Đồ Thị (`frame_0025`–`frame_0030`) |
| :--- | :---: | :---: | :---: | :--- |
| **`Codex`** | Kimi K3 | **66.7%** (20/30 bài) | Trung bình | Đứng đầu bảng — chỉ hơn DSH Creator đúng **1 bài toán (`1 task ≈ 3.3 pts`)** |
| **`DSH Creator` (DeepSeek Harness)** | Kimi K3 | **63.3%** (19/30 bài) | **\$3.28** | **Tỷ lệ đỗ ngang ngửa Claude Code nhưng rẻ hơn 5.6 lần!** |
| **`Claude Code`** | Kimi K3 | **63.3%** (19/30 bài) | **\$18.34** | Đắt gấp **5.6×** mỗi bài đỗ so với DSH Creator trên cùng model Kimi K3 |
| **`OpenCode`** | Kimi K3 | **50.0%** (15/30 bài) | — | Thấp hơn Codex **16.7 điểm %** dù chạy cùng model (`"Kimi K3 is weak? No, OpenCode!"`) |
| **`Exo Harness`** | Kimi K3 | — | **\$1.05** | Cấu hình rẻ nhất toàn bảng (chênh lệch **17.5×** so với \$18.34 của Claude Code) |
| **Biên độ toàn bảng (12 configs)** | Kimi K3 | **50.0% – 66.7%** | **\$1.05 – \$18.34** | Tỷ lệ Cache hit dao động **67.8% – 88.0%**, tốc độ trung bình **5m 41s – 9m 38s** (`frame_0029`) |

> [!TIP]
> **Quy tắc Đánh giá Mô hình (Model Evaluation Rule):** Đừng bao giờ kết luận một mô hình yếu chỉ sau khi thử trên 1 Harness (như thấy Kimi K3 đạt 50% trên OpenCode). Hãy thử mô hình trên 2–3 Harnesses khác nhau vì chính Harness quyết định chiến lược prompt caching, chi phí, tốc độ và tỷ lệ thành công!

---

## 3. Giải Phẫu Framework Cordis (`arXiv:2608.25512`): "Khả Năng Hợp Thành Không-Thời Gian" (`05:53 – 09:36`, `frame_0031`–`frame_0047`)

DeepSeek Harness (`deepseek-ai/deepseek-harness`, ra mắt ngày 13/08/2026 cùng DeepSeek V4 Pro, giấy phép MIT, lưu lại mọi lệnh gọi công cụ *"every tool call, on the record"*, hỗ trợ mọi OpenAI-compatible endpoints chứ không khóa chặt vào model DeepSeek — `frame_0005`, `frame_0031`–`frame_0032`) được xây dựng trên nền tảng **Cordis** (`arXiv:2608.25512`, Đại học Bắc Kinh).

Khác với hầu hết các agent frameworks nơi `core` nắm giữ `agent loop` cố định (`frame_0033`), trong DSH **mọi thành phần đều là plugin ngang hàng** (`model adapter`, `tool registry`, `session log`, `agent loop`, `tool-fs`, `skills` — *"There is no privileged core to patch"*, `frame_0034`, `frame_0047`).

### 3.1. Hai Trụ Cột Của Cordis (`frame_0036`–`frame_0040`)
1. **Temporal Composability (Hợp thành theo Thời gian — Gỡ Bỏ Sạch Sẽ 100%, `frame_0037`, `frame_0039`–`frame_0040`):**
   - Ví dụ trực quan trong `frame_0037`: Plugin `tool-jobs` đăng ký 3 công cụ `job_output`, `job_list`, `job_kill` vào `tool registry`. Ngay khi gỡ (unload) `tool-jobs`, toàn bộ 3 công cụ và các event hooks của nó tự động biến mất hoàn toàn.
   - Trong các harness truyền thống (`no clean unload`, `frame_0039`–`frame_0040`), khi chuyển từ `model A` sang `model B` hoặc tắt `tool-web`, hệ thống để lại hàng loạt tàn dư (`old flag`, `model A cfg`, `old hook` — *"who cleans these up?"*). Cordis giải quyết triệt để bài toán này ở tầng ngôn ngữ/runtime.
2. **Spatial Composability (Hợp thành theo Không gian — Đồ Thị Phụ Thuộc Phản Ứng, `frame_0038`):**
   - Trích dẫn bài báo `arXiv:2608.25512`: *"declare and reactively manage inter-component dependencies"*.
   - Ví dụ: `agent loop` khai báo `needs -> model adapter`. Chỉ khi `model adapter` sẵn sàng thì `agent loop` mới chuyển sang trạng thái `active`, và tự động tạm ngưng an toàn nếu dependency biến mất.

### 3.2. Bốn Tầng Lắp Ghép Khởi Động & 4 Chế Độ UI (`frame_0041`–`frame_0047`)
Theo `docs/architecture.md` (`frame_0041`–`frame_0043`), khi khởi động, DSH áp dụng tuần tự 4 lớp lên một danh sách rỗng:
1. **`bundles`** (Các gói plugin chuẩn của profile)
2. **`profile · cordis.patch.yml`** (File patch của profile)
3. **`home patch`** (Cấu hình cá nhân tại thư mục người dùng)
4. **`--patch overlays`** (Tham số truyền qua CLI)

Lệnh kiểm tra toàn bộ cây cấu hình thực tế trên máy (`frame_0043`):
```bash
$ dsh --profile web --dump-config
# "Any row it prints can be replaced by a patch of your own."
```
- **4 Chế độ trên giao diện (`standard`, `PTC`, `minimal`, `Creator` — `frame_0044`–`frame_0045`):** Thực chất chỉ là 4 tập hợp plugin khác nhau! Chẳng hạn chế độ **`minimal`** tắt hết `tool-fs`, `tool-jobs`, `tool-web`, `tool-subagent`, `compaction`, chỉ giữ lại duy nhất `tool-bash` (*"one persistent shell + read_image"*).
- **Continual Learning ở tầng Harness với `Creator` mode (`frame_0045`):** Khi người dùng gõ `"Write a Pomodoro timer plugin for me."`, agent tự viết plugin (`✓ writes the plugin`), tự cài vào cây Cordis (`✓ installs it`) và tự kiểm tra nó đã chạy sống (`> checks it's live`).
- **Tại sao `CONTRIBUTING.md` từ chối nhận PR bên ngoài? (`frame_0046`–`frame_0047`):** *"We are sorry that we cannot accept external pull requests at the moment."* — Bởi vì không có "Core đặc quyền" nào cần sửa cả; mọi tính năng mới đều được đóng gói và phân phối như một plugin độc lập từ chính repo của bạn (*"ship it from your repo"*).

---

## 4. Giải Phẫu `dsh-superagent-prototype`: Biến Harness Thành Super Agent Không Cần Fork (`09:37 – 15:29`, `frame_0048`–`frame_0076`)

### 4.1. Cấu Trúc File `cordis.patch.yml` & Worker Preset Bị Cắt Gọt (`frame_0048`–`frame_0052`)
Mở repo `dsh-superagent-prototype`, toàn bộ phép màu nằm trong file `cordis.patch.yml`:
1. **Chèn 3 plugins mới (Lines 4–12, `frame_0048`, `frame_0052`):**
   - `id: dsh-superagent` (`name: dsh-superagent`)
   - `id: dsh-superagent-github` (`name: dsh-superagent/github-watch`)
   - `id: dsh-superagent-feishu` (`name: dsh-superagent/feishu-watch`, mặc định `disabled: true`)
2. **Định nghĩa `dsh-superagent-worker-preset` (Lines 13–23, `frame_0049`–`frame_0050`):**
   - Lấy preset `Standard` của DSH nhưng **gạch bỏ 9 tính năng không phù hợp với worker chạy tự động ngầm** (`frame_0050`):
     - ~~`questions to a person`~~ (Cấm hỏi người dùng)
     - ~~`presenting`~~
     - ~~`plan mode`~~ (Tắt chế độ lập kế hoạch tương tác)
     - ~~`plugin manager + market`~~
     - ~~`scheduling`~~
     - ~~`goals`~~
     - ~~`workflows`~~
     - ~~`web fetch`~~
     - ~~`forked subagents`~~ (Cấm đẻ thêm sub-agents con)
3. **Cơ chế Shell Không Chặn (`timeoutMs: 5000`, `frame_0051`):**
   - Mọi lệnh shell chạy quá **5,000 ms (5 giây)** đều tự động bị đẩy xuống `background`.
   - Minh họa trong `frame_0051`: Khi `worker · work/482` chạy lệnh `$ pnpm build` mất `12.5 s`, sau 5 giây lệnh build được đẩy xuống nền (*"the build keeps running"*), giải phóng `foreground` để worker lập tức đọc được bình luận mới của `jlee`: `"also check Firefox? ✓"` thay vì bị khóa cứng như các shell truyền thống (`blocking shell`).

### 4.2. Phân Biệt `Agent Teams` vs. `Super Agent` & Kiến Trúc 2 Tầng (`frame_0053`–`frame_0076`)

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│             AGENT TEAMS (ONE JOB) VS. SUPER AGENT (MANY JOBS OVER DAYS/WEEKS)                   │
├─────────────────────────────────────────────────────────────────────────────────────────────────┤
│  • AGENT TEAMS (`frame_0053`): Điều phối nhiều agents BÊN TRONG 1 JOB (`roster`, `task board`,  │
│    `mailbox`) diễn ra trong vài phút.                                                           │
│  • SUPER AGENT (`frame_0054`): Xử lý HÀNG LOẠT JOBS ĐỘC LẬP từ nhiều người qua nhiều ngày       │
│    (`Mon: Issue #512 jlee` ➔ `Tue: Feishu kai` ➔ `Thu: PR #490 mia` ➔ `Sat: Issue #533 omar`).  │
├─────────────────────────────────────────────────────────────────────────────────────────────────┤
│  LUỒNG XỬ LÝ TRIGGER 3 BƯỚC & CƠ CHẾ DIRECT PR SUBSCRIPTION (`frame_0058`–`frame_0068`):        │
│                                                                                                 │
│  [1. DETERMINISTIC GATE (`docs/001-domain.md` — 0 LLM Token, `frame_0058`–`frame_0059`)]        │
│      Kiểm tra cứng bằng code: `sender on allowlist` + `mentions @agent`                         │
│      • `Issue #512 · jlee (@agent take this?)` ──► ✓✓ (Qua cổng)                                │
│      • `PR #490 · mia (nit: rename this)`      ──► ✓✗ (Loại vì thiếu `@agent`)                  │
│      • `Feishu chat · kai (@agent ship it)`    ──► ✗  (Loại vì `kai` ngoài allowlist)           │
│                                     │                                                           │
│                                     ▼                                                           │
│  [2. ROUTER MAIN SESSION (`fast, cheap model`, không cần Bash — `frame_0060`, `frame_0066`)]    │
│      • Gom lô dưới tải cao (`3 triggers share one read`: 1➔40 triggers/phút, batch 1.0➔2.8,    │
│        giảm thời gian ra quyết định từ `6.4s ➔ 1.6s`!).                                         │
│      • Cấp `Ticket #512 Mandate` (`policy version`, `scope`, `strength` — "can't be widened     │
│        later", `matched rules`, `sender jlee · GitHub`, `received via`).                        │
│                                     │                                                           │
│                                     ▼                                                           │
│  [3. ISOLATED WORKER (`strong model` / `open model`, `work/512` — `frame_0061`–`frame_0062`)]   │
│      • Worker tự động `subscribed` vào `PR #513 · Fix login redirect` mà nó mở.                 │
│      • Khi `mia` bình luận `"can you add a test?"`, tin nhắn ĐI THẲNG VÀO WORKER (`bypasses     │
│        the gate and grants no new authority` - `docs/002-authorization.md`), BỎ QUA ROUTER!     │
│      • Tắt tính năng Subscription này làm số quyết định của Router tăng vọt `+30%` (`frame_0080`).│
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

### 4.3. Ba Phát Hiện Ngược Trực Giác Trong Thiết Kế Super Agent
1. **Càng Tải Cao, Router Ra Quyết Định Càng Nhanh (`frame_0065`–`frame_0068`, `frame_0080`):**
   - Do cấu trúc prompt của Router có **đầu vào dài (long input) nhưng đầu ra rất ngắn (short output)**, khi nhiều sự kiện (`#540`, `#541`) đổ tới lúc Router đang suy luận, chúng được gom lại xử lý chung trong 1 lần đọc cache (`3 triggers share one read`, `frame_0066`).
   - Kết quả stress test khi tăng tải từ **1 lên 40 triggers/phút** (`frame_0068`): Kích thước lô trung bình tăng từ `1.0 → 2.8`, thời gian mỗi lệnh gọi tăng nhẹ từ `2.5s → 3.9s`, nhưng **thời gian trung bình cho mỗi quyết định giảm mạnh 4 lần từ `6.4s → 1.6s`** (*"load helps here"*).
2. **Đảo Ngược Phân Bổ Mô Hình (Router Nhẹ & Rẻ — Workers Mạnh, `frame_0069`–`frame_0072`):**
   - Trái với suy nghĩ phổ biến rằng Orchestrator phải dùng model đắt nhất (Opus) còn Sub-agents dùng model nhỏ, DeepSeek phát hiện công việc của Router (nhờ đã có Gate lọc code và cơ chế PR Subscription) đơn giản hơn nhiều và **thậm chí không cần công cụ Bash**.
   - Do đó, bạn có thể dùng một **mô hình nhanh, rẻ (`fast, cheap model`) cho Router** và dồn **mô hình mạnh (`strong model` hoặc các `open model` khác nhau) cho từng Worker** (`"same router, swap the workers"`, `frame_0071`–`frame_0072`).
3. **Triết Lý Lịch Sự & Chế Độ `Recorded, not run` (`00-philosophy.md`, `frame_0073`–`frame_0076`):**
   - Nguyên tắc tối thượng trong `00-philosophy.md` (`frame_0073`): *"Take as little human attention as possible, and give as much value as possible."*
   - Super Agent chỉ hành động trong 2 trường hợp (`frame_0074`–`frame_0076`):
     1. **`Someone allowed asks`:** Người nằm trong allowlist trực tiếp gọi `@agent`.
     2. **`A rule the team wrote` (Tự khởi xướng theo quy tắc nhóm):** Khi bắt đầu công việc không ai trực tiếp yêu cầu, agent khởi chạy ở chế độ **`Recorded, not run`** (`would have reproduce #517`, `would have patch the test`, `would have open a PR`), tự chạy bước kiểm nghiệm (`✓ it would have worked`), và chỉ lên tiếng mở PR (`Fix ready for review: #518`) khi đã chắc chắn thành công (*"quiet until it matters"*).

---

## 5. Bốn Hạn Chế & Lưu Ý Thực Tế (`15:30 – 17:52`, `frame_0077`–`frame_0086`)

1. **Nguyên mẫu nghiên cứu (`"not identical to our internal version"`, `frame_0077`):** Một số thành phần nêu trong bài báo (như phiên `Watchdog` giám sát độc lập) là định hướng tương lai và chưa có mặt đầy đủ trong repo `dsh-superagent-prototype`.
2. **Phạm vi tích hợp mặc định (`One repo, for now`, `frame_0078`–`frame_0079`):** Hiện mới hỗ trợ 1 GitHub repo và tùy chọn Feishu (chưa có sẵn Slack hay Email), nhưng repo đã kèm sẵn ví dụ `webhook` và Cookbook để cộng đồng tự viết plugin nguồn sự kiện mới.
3. **Benchmark chạy trên tải giả lập (`synthetic workload`, `frame_0080`–`frame_0081`):** Các con số `6.4s → 1.6s` và `+30% decisions` được đo trên môi trường giả lập đơn giản hóa, còn `FrontierHarness Eval` mới dừng ở mẫu 30 bài toán (`1 task ≈ 3.3 pts`).
4. **Rủi ro Bảo mật mở (`Security`, `frame_0082`):** Cả hai preset hiện chạy với quyền `dangerously-skip-permissions` dưới tài khoản OS của người vận hành; bài toán **Prompt injection**, **Credential leaks** và **Device permissions** vẫn chưa được giải quyết triệt để trong bản nguyên mẫu, do đó nên chạy trong môi trường cô lập (container/VM) hoặc với mô hình cục bộ.

---

## 6. Liên Kết Chéo & Tài Liệu Tham Khảo (`frame_0085`)

- **Repositories & Papers:**
  - `deepseek-ai/deepseek-harness` (DeepSeek Harness)
  - `deepseek-ai/dsh-superagent-prototype` (Super Agent)
  - `arxiv.org/abs/2608.25512` (Bài báo Cordis — Đại học Bắc Kinh)
  - `frontierharness.org` (FrontierHarness Eval · Runta)
- **Các bài phân tích liên quan trong Vault:**
  - [DeepSeek Tái Định Nghĩa Agent Harness: 7 Prompts Của Creator Mode (AICodeKing)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_redefining_harnesses_creator_mode_prompts_aicodeking.md)
  - [Giải Mã DeepSeek Super-Agent & Tích Hợp Vào Claude Code (Rob Shocks)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_super_agent_plug_into_claude_code_rob_shocks.md)
  - [Bản Thiết Kế Hệ Thống Phân Tán Của DeepSeek Super Agent (lustoykov)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_super_agent_design_grokbot_agents_lustoykov.md)
