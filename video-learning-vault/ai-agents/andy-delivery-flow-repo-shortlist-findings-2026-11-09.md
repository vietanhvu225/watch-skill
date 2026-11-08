# Finding & Repo/Video Shortlist: Đối Chiếu Hệ Sinh Thái 3 Tầng (`andy-delivery-flow` · `adt-yagni` · `andy-dev-board` / `lane-board`) Với Video Learning Vault

- **Date:** 2026-11-09
- **Scope:**
  - **Lượt 1:** Quét `D:\6_AI-flow\andy-delivery-flow` (`/adf`) để nắm tinh thần làm việc ("Engineering DNA") ở tầng thực thi đơn lẻ (13 phases) và lập Shortlist các repositories ưu tiên.
  - **Lượt 2:** Quét sâu `D:\6_AI-flow\andy-dev-board` (`/board` + Claude Code Mod `mods/board` v0.8.2), kết nối cùng `adt-yagni` và `lane-board` để hiểu bức tranh tổng thể **3 tầng (Flow $\rightarrow$ Team $\rightarrow$ Board)** bạn đang xây dựng, từ đó lập Shortlist các video & bài học trong Vault khớp trực tiếp với Roadmap `B-01`..`B-17` và `L-01`..`L-04` của `andy-dev-board`.
- **Status:** Active Reference & Prioritized Study Guide

---

## Phần I — Kiến Trúc 3 Tầng & Tinh Thần Làm Việc ("Engineering DNA") Của Bạn

Qua việc đọc toàn bộ mã nguồn và tài liệu thiết kế trong `D:\6_AI-flow\`, hệ thống của bạn không phải là các prompt rời rạc mà là một **kiến trúc điều phối 3 tầng hoàn chỉnh** (mỗi tầng cao hơn tái sử dụng tầng thấp hơn chứ không thay thế nó):

| Tầng | Tên Hệ Thống | Phạm Vi | Trả Lời Câu Hỏi | Repo / Thư Mục |
| :---: | :--- | :--- | :--- | :--- |
| **Layer 1** | **Andy Delivery Flow** (`/adf` / `andy-dev-flow`) | **1 ADO item · 13 Phases** | *Làm sao hoàn thành 1 item đúng chuẩn kỹ thuật với bằng chứng không thể chối cãi?* | `D:\6_AI-flow\andy-delivery-flow` & `andy-dev-board\skills\andy-dev-flow` |
| **Layer 2** | **Andy Dev Team** (`ADT` / `/adt`) | **1 ADO item · 1 Leader + 5 Roles** | *Ai làm item đó: 1 phiên Solo hay một đội 5 vai trò chạy trên commit đóng băng?* | `D:\6_AI-flow\adt-yagni` |
| **Layer 3** | **Andy Dev Board** (`/board` + `mods/board`) & **`lane-board`** | **Nhiều ADO items song song trong ngày/sprint** | *Làm sao chạy 5–15 lanes song song dưới 1 Orchestrator, nhìn thấy nhau để không làm gãy nhánh `v17/dev`, và hiển thị trạng thái trực tiếp trên thanh Band/Pane của Claude Desktop?* | `D:\6_AI-flow\andy-dev-board` & `D:\6_AI-flow\lane-board` |

### 5 Nguyên Lý Cốt Lõi Định Hình Phong Cách Của Bạn

1. **"Evidence Earns Autonomy" & "Gates Pass by Mechanism, Not Intent":**
   - Một phase chỉ hoàn thành khi skill được gọi thật và để lại **receipt/artifact trên đĩa**. Mọi lời khẳng định trong code comments, XML docs, commit messages đều bị coi là **unverified claims** cần kiểm chứng.
   - Không có máy móc nào được thêm vào nếu chưa có một lỗi thực tế trong quá trình chạy thử yêu cầu nó (*Contract §1.5: "No machinery without a trial failure that needs it"*).
2. **Tách Biệt Tuyệt Đối Maker $\neq$ Checker (Phase 8 vs. Phase 9):**
   - **Phase 8:** Chấm điểm tuân thủ theo từng `check-ID` (`umbraco-cms-backoffice-skills` kèm `version-guard`), tạo `phase-8b` delta receipts vì *"Clean is perishable"*, và **xóa toàn bộ comment giải thích logic trước khi sang Phase 9** để ép Reviewer đọc code trần.
   - **Phase 9:** Chạy Reviewer độc lập, read-only (`codex-plugin` `task --fresh` hoặc `everything-claude-code/code-reviewer`) không được đọc `kickoff` hay `implementation-notes` ở Round 1, xuất biên lai đầy đủ (`Provider`, `Target SHA`, `Round`, `Verdict`).
3. **Con Người Giữ Quyền Quyết Định ("Andy Decides") & Two-Tier YAGNI:**
   - Một lane không bao giờ tự khởi chạy trên một lượt tự động (*automated turn*); nó chỉ chạy khi chính bạn gõ lệnh bằng lời của mình.
   - **Tier-1 YAGNI (Phase 0 & Resume Un-defer Tripwire):** *AC này có đáng làm ngay bây giờ không?* $\rightarrow$ **Tier-2 YAGNI (Phases 2, 5, 8, 9):** *Thiết kế đơn giản nhất thỏa mãn AC là gì?*
4. **Forward Resume Reconciliation:**
   - Khi resume một công việc dở dang, bắt buộc đối chiếu không chỉ với ADO mà còn **đọc tiến về phía trước (read forward)** qua các file `_daily/{YYYY-MM}/{DD}/` mới hơn (nơi các quyết định rút lui/retraction ở `office-end.md` được ghi nhận).
5. **Tối Ưu Hóa Sự Chú Ý Trên Một Bề Mặt Duy Nhất (`The Title is the Status` + `Board Band Mod`):**
   - Bạn không đọc transcript dài dòng của từng lane. Bạn nhìn vào **Board Band (`AbovePrompt`)** trên phiên `Engage Orchestrator` (12 dòng, tự động gập `working` $\rightarrow$ `WAIT` $\rightarrow$ `CLOUD`, giữ nguyên `CHECK` và `GATE`, phát hiện tag bị treo `CHECK?`/`GATE?` qua `B-01`, nhảy thẳng sang lane qua nút `[ › ]`) và **Lane Detail Pane (`[ ☰ ]` — `L-04`)** mở trực tiếp file đầu ra của từng phase P0–P12 trong file pane của Claude Desktop với lớp bảo vệ chống symlink/path-traversal (`$.fs.stat { resolve: true }`).

---

## Phần II — Lượt 1: Shortlist Ưu Tiên Cho Tầng 1 (`andy-delivery-flow`)

Để giải quyết các bài toán đang mở của Tầng 1 (`H-001` Receipt Validator, `H-002` Bounded `-yolo` mode, `R-301` Expert Council, `R-302` External Review Provider, và Phase 8/9/10), đây là **5 repositories cần ưu tiên đọc trước**:

### 🥇 1. `google/elephant-goldfish-model` (Google EGM Skill Set)
- **Ghi chú trong Vault:** [google_egm_skill_set_deepseek_aicodeking.md](./google_egm_skill_set_deepseek_aicodeking.md)
- **Áp dụng vào đâu:** **`R-301` (Bounded Expert Council)**, **`R-302`**, **Phase 2 Audit**, và **Phase 7/9 (`Maker != Checker`)**.
- **Điểm đắt giá nhất:**
  - Chia vai **Elephant** (giữ context dự án, viết thiết kế) và **Goldfish** (sub-agent **0 memory, read-only, `omitClaudeMd: true`**, cấm nịnh hót `No Glazing`).
  - Thay vì để nhiều agent thảo luận tạo đồng thuận giả như `BMAD-METHOD` Party Mode, `/egm-brainstorm` tung **6 Goldfish Lenses độc lập**, `/egm-review` quét **5 tầng phản biện** (có `Hostile Design Critic`), và `/egm-resolve` lập bảng **`PROPOSE FIX` vs. `CONTEST`** để con người ra quyết định cuối cùng.

### 🥈 2. `cursor/plugins` — Bộ `pstack` (`poteto` / Lauren Tan) & Kiến Trúc `Dune`
- **Ghi chú trong Vault:**
  - [pstack_how_cursor_engineer_ships_code_eric_tech.md](./pstack_how_cursor_engineer_ships_code_eric_tech.md)
  - [lauren_tan_orchestrate_40_agents_dune_pstack_viet_nguyen_ai.md](./lauren_tan_orchestrate_40_agents_dune_pstack_viet_nguyen_ai.md)
- **Áp dụng vào đâu:** **Phase 8 (`comment-hygiene.md` & Simplicity Pass)**, **Phase 10 Verification**, và **Fixtures/Evals cho `H-001`/`H-002`**.
- **Điểm đắt giá nhất:**
  - `/deslop` (quét 5 loại rác AI) + `/no-comments` (cùng triết lý với `comment-hygiene.md` của bạn).
  - Kiến trúc `skills-as-index` tách các nguyên tắc thành file nhỏ `principle-*` (`attack-the-premise`, `type-system-discipline`).
  - **`Feature Map` + `/create-verification-skill`:** Tự động hóa việc ánh xạ thay đổi code với kịch bản kiểm thử Phase 10.
  - **Kinh nghiệm viết Skill Evals:** Giấu tên thư mục test (không đặt chữ `eval`/`test` trong path) và dùng Cross-Model Judge chấm điểm trace.

### 🥉 3. `anthropics/claude-plugins-official` (`engineer-mode` của Ashwin Bhat) & `Stop Hook (Exit Code 2)`
- **Ghi chú trong Vault:**
  - [claude_official_engineer_mode_anthropic_loop.md](./claude_official_engineer_mode_anthropic_loop.md)
  - [agent_harness_claude_code_codex_200lab.md](./agent_harness_claude_code_codex_200lab.md)
  - [shopify_claude_code_workflow.md](./shopify_claude_code_workflow.md)
- **Áp dụng vào đâu:** **`H-002` (Bounded `-yolo` / `Phase-8-stop` mode)** và **Runtime Risk Breaker / Receipt Validator (`H-001`)**.
- **Điểm đắt giá nhất:**
  - Cấu trúc **3 điều kiện dừng hợp lệ duy nhất** (`Done with Proof`, `True External Blocker`, `Ambiguity Requiring User Choice`) + phân loại **Blast Radius**.
  - Dùng `Stop hook` / `PreToolUse hook` trả về **`exit code 2`** để biến các cổng kiểm tra từ instruction-level sang cưỡng chế cơ học 100%.

### 4. `max-sixty/worktrunk` (`wt` — Rust Git Worktree CLI)
- **Ghi chú trong Vault:** [the_best_git_worktree_tool_worktrunk_better_stack.md](./the_best_git_worktree_tool_worktrunk_better_stack.md)
- **Áp dụng vào đâu:** **Phase 1 Worktree Setup** và **Phase 10 Separate Verify Worktree** (như bạn mô tả trong `andy-dev-flow/SKILL.md` dòng 161: *copy local-only files `appsettings.Local.json` into the verify worktree*).
- **Điểm đắt giá nhất:** `wt step copy-ignored` dùng Copy-on-Write chép tức thì các file git-ignored (`appsettings.Local.json`, build cache) sang worktree mới và bộ lọc `hash_port` chống đụng cổng localhost.

### 5. `vercel-labs/agent-desktop` (Accessibility Tree Computer Use)
- **Ghi chú trong Vault:** [agent_desktop_computer_use_accessibility_tree_prism_labs.md](./agent_desktop_computer_use_accessibility_tree_prism_labs.md)
- **Áp dụng vào đâu:** **Phase 10 UI Verification** — đọc Accessibility Tree / DevTools CDP trong `50–200ms` kèm định danh `snapshot_id:element_id` tự động bác bỏ các tham chiếu UI cũ (*stale refs*).

---

## Phần III — Lượt 2: Phân Tích `andy-dev-board` & Shortlist Các Video Trong Vault Để Học Hỏi Tiếp

### 1. Những Gì Bạn Đã Làm Rất Xuất Sắc Trong `andy-dev-board` (`mods/board` v0.8.2)
Đọc qua `docs/decisions.md`, `ROADMAP.md`, `docs/analysis/board-trial/band-ideas-from-case-studies.md` và mã nguồn trong `mods/board/hooks/`, mình thấy bạn đã đi rất xa:
- Bạn đã nghiên cứu kỹ các case studies (`Helper Desk`, `Cache Clock`, `Savvy Progress`, `Replay Theater`, `Handoff`), lọc bỏ những thứ màu mè tốn token (như shimmer 300ms hay tiêm system prompt qua `prompt.compose`), và giữ lại kiến trúc **read-only siêu sạch** (91 unit tests pass, kiểm tra chặt chẽ `$.fs.stat { resolve: true }` chống symlink/junction escape ở `L-04`).
- Bạn đã giải quyết bài toán **`B-01` (Stale Tag Detection)** rất thông minh: kết hợp `list_sessions` (`isRunning: true` qua 2 chu kỳ poll liên tiếp) với `tagSince` để gắn cờ `CHECK?` / `GATE?` khi một lane quên đổi tag sau khi đã được bạn trả lời.
- Bạn đã thiết kế phương án **`B-02` Title-Only (YAGNI option)**: tận dụng giới hạn >200 ký tự của `set_session_title` (`GATE · 71803 gallery-test · P10 · PR 401/402 · since 31-10 14:05 · need: go for push`) để gom 2 lần ghi (`set_session_title` + `<id>.json`) về **1 nguồn sự thật duy nhất**, loại bỏ hoàn toàn lỗi lệch trạng thái giữa title và file.

### 2. Các Bài Toán Đang Mở Của `andy-dev-board` Mà Các Video Trong Vault Có Lời Giải Trực Tiếp
Đối chiếu với `andy-dev-board/ROADMAP.md` và `core/board-contract.md`:
1. **Bài toán #1 — Hai PR xanh riêng lẻ nhưng khi merge làm đỏ `v17/dev` & Tranh chấp tài nguyên cục bộ (`B-08`: 9GB DB Clone, Ports, Shared Files):**
   - Shared notes hiện tại phụ thuộc vào việc lane *nhớ* đọc và ghi trước khi sửa/merge. Làm sao khóa cơ học việc tranh chấp tài nguyên (DB clone 9GB, port `44367`, shared files) và tự động restack/re-verify khi nhánh base (`v17/dev` / `v18/dev`) tiến lên?
2. **Bài toán #2 — Quản lý bộ nhớ RAM & Vòng đời "Sleeping Lanes" (`B-04`, `B-10`):**
   - Khi chạy 5–10 lanes .NET / Umbraco cùng lúc, RAM tăng cao (`79% / cap 85%`). Làm sao đóng băng (park/archive) các lanes đang ở trạng thái `WAIT` (chờ Nathan review PR hoặc chờ CI/Cloud deploy) và tự động đánh thức chúng dậy đúng lúc mà không cần polling thủ công?
3. **Bài toán #3 — Kiến trúc Mod nâng cao, Chuyển giao giữa các Orchestrator (`L-01`), và Đưa `ADT` lên `Board` (`L-02`):**
   - Khi `Engage Orchestrator` đầy context và handover sang Orchestrator mới, làm sao giữ liên lạc và trạng thái bền vững xuyên qua nhiều thế hệ Orchestrator? Khi nào một lane nên chạy `solo` vs. `ADT` (5 roles)?

---

### 🎯 Shortlist 6 Video & Ghi Chú Trong Vault Bạn Nên Xem Nhất Cho `andy-dev-board`

Dưới đây là **6 video đã phân tích trong Vault** (ngoài 3 case studies bạn đã đọc trong `band-ideas-from-case-studies.md`) chứa các cơ chế kỹ thuật khớp trực tiếp với lộ trình của `andy-dev-board`:

#### 🥇 1. Video về `Herdr` (`lustoykov`) — Cơ Chế `Resource Lock File` Cho `B-08` (9GB DB Clone) & `No-Chatter Coordination`
- **Ghi chú trong Vault:** [herdr_beginner_to_advanced_lustoykov.md](./herdr_beginner_to_advanced_lustoykov.md)
- **Giải quyết bài toán nào của `andy-dev-board`:** **`B-08` (Lane start note claims its 9GB DB clone / port `44367`)** và **vấn đề 2 lanes cùng sửa chung file làm gãy `v17/dev`**.
- **Bài học thực chiến nên rút ra ngay:**
  - Trong video này, tác giả chạy 4–6 Worker Agents song song cần dùng chung một tập hữu hạn **3 iOS Simulators** (tương đương hệt bài toán **9GB SQL/Engage DB Clones** và **TestSite port `44367`** của bạn!).
  - Thay vì chỉ ghi vào shared notes dễ bị đọc sót do race condition khi 2 lanes khởi động gần nhau, họ dùng một cơ chế **Lease/Lock file nguyên tử trong thư mục dùng chung** (ví dụ `D:\source\work\_shared\board\locks\db-clone-1.lock` chứa `{"lane": 71803, "sessionId": "local_...", "acquiredAt": "..."}` kèm rule: nếu session đó đã `DONE` hoặc `isArchived` thì lock tự động hết hạn).
  - Trước khi chạy Phase 10 UI Verify hoặc đụng vào DB clone, lane bắt buộc acquire lock; nếu tất cả DB clones đang bận, lane tự chuyển sang `WAIT · ... · need: db-clone` mà không làm hỏng dữ liệu của lane bên cạnh!

#### 🥈 2. Video về `S-Tier Claude Code Mods` (Ray Amjad) — Patterns Từ `diagram-pane`, `cache-clock` & `dream-pass` Cho `mods/board`
- **Ghi chú trong Vault:** [s_tier_claude_code_mods_ray_amjad.md](./s_tier_claude_code_mods_ray_amjad.md)
- **Giải quyết bài toán nào của `andy-dev-board`:** **Tối ưu hóa `mods/board` (`B-09`, `L-01`, `L-04`)** và **Tự động hóa Handover khi Orchestrator đầy Context**.
- **Bài học thực chiến nên rút ra ngay:**
  - Trong `band-ideas-from-case-studies.md`, bạn đã đọc mã nguồn của `cache-clock` từ gói `benai-mods`. Video của chính tác giả Ray Amjad giải thích lý do thiết kế đằng sau 8 bản mod S-Tier trên Claude Code `v2.1.296+`:
    1. **Cơ chế Auto-Handoff trong `cache-clock`:** Không chỉ đếm ngược TTL của cache (60m main / 5m subagent), mà khi context hoặc thời gian chạm ngưỡng, nó chuẩn bị sẵn bản tóm tắt bàn giao (`handoff.md`) trước khi người dùng bấm chuyển phiên — rất hợp để kết hợp với tính năng **F14 (`Handover` button cho `Engage Orchestrator`)** và **`L-01` (Durable notes across Orchestrator handovers)** của bạn.
    2. **Cách tổ chức Side Pane (`diagram-pane` & `impello-swarm`):** Các mẹo xử lý bố cục cột, ký tự Unicode (`┌`, `╭`, `╔`, `▶`) và gom trạng thái (`atom` snapshot) khi vẽ bảng điều khiển đa tác nhân trong Pane của Claude Desktop.

#### 🥉 3. Video về `PandaOrchestrator / PandaCode` (`AICodeKing`) — Cơ Chế `Process Parking` (Giải Phóng RAM `B-04`/`B-10`) & `Architect Hibernation`
- **Ghi chú trong Vault:** [tiny_local_model_outperforms_opus_orchestration_aicodeking.md](./tiny_local_model_outperforms_opus_orchestration_aicodeking.md)
- **Giải quyết bài toán nào của `andy-dev-board`:** **`B-04` (Sleeping lanes — Archive & Wake)**, **`B-10` (Quản lý RAM trên thanh Board)**, và **Vấn đề transcript của Orchestrator bị phình to do turn-end notes**.
- **Bài học thực chiến nên rút ra ngay:**
  - Mỗi tiến trình Claude Code / .NET language server ngầm của một lane đang đứng chờ (`WAIT`) ngốn từ **250–390MB RAM** (chưa kể `.NET` build server).
  - Trong `PandaCode`, họ cài đặt cơ chế **Process Parking**: ngay khi một Worker chuyển sang trạng thái chờ bên ngoài (như `WAIT` chờ reviewer Nathan hoặc chờ Cloud deploy), hệ thống tự động lưu checkpoint trạng thái ra file và **đóng băng/archive phiên đó** để giải phóng toàn bộ RAM vật lý; đồng thời **Architect (Orchestrator) chỉ thức dậy trên 4 tín hiệu thực sự cần hành động** (`CHECK`, `GATE`, `CLOUD`, `DONE`) chứ không bị đánh thức bởi mọi bước trung gian. Điều này củng cố trực tiếp cho thiết kế `B-04` của bạn.

#### 4. Video về `Theo Browne (t3.gg) — You're Using AI Agents Wrong` — Quy Tắc `Inbox Zero (Settle / Snooze)` & `Babysit PR Loop` Cho `B-04` / `B-05`
- **Ghi chú trong Vault:** [youre_using_ai_agents_wrong_theo_t3.md](./youre_using_ai_agents_wrong_theo_t3.md)
- **Giải quyết bài toán nào của `andy-dev-board`:** **Quản lý sự chú ý trên Board khi số lượng lanes vượt quá 8–12 (`B-11`, `B-04`, `B-05`)**.
- **Bài học thực chiến nên rút ra ngay:**
  - Theo vận hành >50 PRs/tuần bằng triết lý **"Quản lý các Agent Lanes giống hệt như Inbox Zero"**:
    - **`Settle`:** Ngay khi PR merge và xong thủ tục, lane tự động biến mất khỏi tầm mắt (giống cách bạn vừa đổi `DONE` thành chỉ 1 con số đếm trên Header ở Band 1.1 `mod 0.7.0`!).
    - **`Snooze` (Tương đương `WAIT` + Archive ở `B-04`):** Các lane đang đợi CI hoặc đợi người khác review phải được ẩn/gập gọn và chỉ "nổi lên" lại (`unarchive` / chuyển sang `CHECK` hoặc `CLOUD`) khi có sự kiện mới trên PR hoặc sau khi bản build `vN/dev` deploy xong.

#### 5. Video của Anthropic (`When to Fold Subagents into Your Main Agent Loop`) — Giải Quyết Quyết Định **`L-02` (Khi Nào Dùng `Solo Lane` vs. `ADT 5-Role Lane`?)**
- **Ghi chú trong Vault:** [claude_code_getting_rid_of_subagents_aicodeking.md](./claude_code_getting_rid_of_subagents_aicodeking.md)
- **Giải quyết bài toán nào của `andy-dev-board` & `adt-yagni`:** **Câu hỏi mở #3 trong `docs/decisions.md` và `L-02` (`ADT lanes on the Board`)**.
- **Bài học thực chiến nên rút ra ngay:**
  - Bài viết của Diane Sarkis (Anthropic) phân tích 3 lỗi kinh điển khi tách quá nhiều vai trò (`Planner` $\rightarrow$ `Reviewer` $\rightarrow$ `Implementer` $\rightarrow$ `Auditor` như trong `ADT`): **Telephone Game** (tam sao thất bản qua các file handoff trung gian), **Subagent Sprawl**, và **Frankenprompt**.
  - Anthropic đưa ra **Bài kiểm tra 3 câu hỏi** cực kỳ sắc bén giúp bạn chốt ranh giới giữa **`solo` lane (với Phase 9 independent reviewer)** và **`ADT` lane (`L-02`)**:
    1. *Ngữ cảnh của bước trước có làm hỏng phán đoán của bước sau không?* $\rightarrow$ Ở **Phase 9 (External Code Review)** và **Phase 7 (Marketer Panel)**, câu trả lời là **CÓ** (vì Maker $\neq$ Checker), nên tách ra một fresh session là **bắt buộc và cực kỳ hiệu quả**.
    2. *Nhưng giữa `Planner` (Phase 2) và `Implementer` (Phase 5), chúng có cần chia sẻ toàn bộ hiểu biết sâu về codebase không?* $\rightarrow$ Câu trả lời là **CÓ**. Tách `Planner` và `Implementer` thành 2 phiên rời rạc trong `ADT` spesso làm mất ngữ cảnh ngầm và tốn thời gian đọc lại repo.
  - **Kết luận rất giá trị cho bạn:** Mô hình hiện tại của `andy-dev-board` (**Solo Lane chạy xuyên suốt Phases 0–8 trong cùng 1 phiên để giữ ngữ cảnh sâu + gọi Fresh Independent Session ở Phase 7 và Phase 9**) thực chất **tối ưu hơn cả việc bê nguyên 5 vai trò rời rạc của `ADT` lên Board**! Bạn có thể tự tin giữ `Solo Lane + Fresh Phase 9 Reviewer` làm kiến trúc chủ lực lâu dài.

#### 6. Video của Lauren Tan (`40+ Agents & Kiến Trúc Dune`) & `DeepSeek Super-Agent` — Giải Quyết Tận Gốc Sự Cố "2 PRs Xanh Riêng Lẻ Nhưng Merge Vào Làm Đỏ `v17/dev`"
- **Ghi chú trong Vault:**
  - [lauren_tan_orchestrate_40_agents_dune_pstack_viet_nguyen_ai.md](./lauren_tan_orchestrate_40_agents_dune_pstack_viet_nguyen_ai.md)
  - [deepseek_super_agent_design_grokbot_agents_lustoykov.md](./deepseek_super_agent_design_grokbot_agents_lustoykov.md)
- **Giải quyết bài toán nào của `andy-dev-board`:** **Nguyên nhân gốc rễ tạo ra `andy-dev-board` (sự cố ngày 27-10-2026 khi 2 lanes cùng xanh nhưng merge vào làm gãy `v17/dev`)** và **`L-01` (Durable notes across Orchestrator handovers)**.
- **Bài học thực chiến nên rút ra ngay:**
  - Khi nhiều lanes chạy song song, lỗi tích hợp (*semantic merge conflict*) thường xảy ra ở các **shared roots** (file đăng ký DI trung tâm, file enum/constants chung, hoặc test helper dùng chung).
  - Trong `core/board-contract.md` §7, bạn đã có quy tắc rất đúng: *"Re-run CI on the current base before merging; One merge per vN/dev at a time"*.
  - Từ hệ thống của Lauren Tan (tự động restack 20 PRs mỗi đêm), bạn có thể bổ sung thêm 1 quy tắc nhỏ vào **`roles/lane.md` ("Before merge")** và **`roles/orchestrator.md`**:
    - Khi Lane A vừa merge vào `v17/dev`, `Engage Orchestrator` (hoặc shared note) phát tín hiệu **`BASE_MOVED (v17/dev @ <SHA>)`**.
    - Mọi Lane B, C đang ở `P8`–`P11` trên cùng nhánh `v17/dev` bắt buộc phải `git fetch && git rebase origin/v17/dev` + chạy lại `dotnet test` (và nếu tập file của Lane A giao cắt với file/module của Lane B thì chạy lại `phase-8b` delta) trước khi được phép gắn tag `GATE · ... · need: go for merge`.

---

## Phần IV — Tổng Hợp Gợi Ý Cụ Thể Cho Roadmap Của `andy-dev-board`

Dựa trên sự giao thoa giữa những gì bạn đã viết trong `andy-dev-board/ROADMAP.md` và các bài học từ Vault, đây là 5 đề xuất thực thi ngắn gọn nhất theo đúng tinh thần **YAGNI** của bạn:

| Mã Roadmap trong `andy-dev-board` | Đề xuất chốt thiết kế (Dựa trên bằng chứng của bạn + Vault) |
| :--- | :--- |
| **`B-02` & `B-03` (Status Truth)** | **Ủng hộ 100% phương án `Title-Only` của bạn (01-11-2026):** Bỏ file `<sessionId>.json` của từng lane để triệt tiêu hoàn toàn lỗi lệch trạng thái (drift) giữa title và file, đồng thời xóa luôn sự phụ thuộc vào `statusDir` ở phía các child lanes. Chỉ cần sửa `hooks/parse.ts` không bao giờ drop dòng `RAW` và đọc phần đuôi `· since ... · need: ...` từ title 200 ký tự. |
| **`B-08` (DB Clone & Shared Resource Claim)** | Thêm trường `db:<clone-name>` (hoặc file lock nhẹ trong `_shared/board/`) ngay ở bước khởi tạo lane (`roles/lane.md` "At start") để 2 lanes không bao giờ vô tình chạy migration/test đè lên cùng một DB clone 9GB. |
| **`L-01` (Durable Notes Across Handovers)** | Thay vì chỉ dựa vào thư mục `side-session-notes\<parent-id>` (vốn bị chia cắt khi đổi từ Orchestrator cũ sang Orchestrator mới), hãy dùng một file bền vững duy nhất tại `D:\source\work\_shared\board\shared-lanes-log.md` (hoặc trong `.claude-scratch/_daily/`) theo kiểu append-only để cả các lanes thuộc Orchestrator cũ lẫn mới đều nhìn thấy nhau. |
| **`L-02` (ADT Lanes on the Board)** | **Giữ nguyên ưu tiên cho `Solo Lane + Fresh Phase 9 Reviewer`:** Như bài phân tích của Anthropic (`claude_code_getting_rid_of_subagents_aicodeking.md`) chỉ ra, một Solo Lane giữ trọn ngữ cảnh từ Phase 0 $\rightarrow$ Phase 8 kết hợp với Fresh Independent Reviewer ở Phase 9 (`codex-plugin` `task --fresh`) tránh được hiệu ứng *Telephone Game* tốt hơn nhiều so với việc tách 5 vai trò rời rạc như `ADT`. |
| **`B-13` & `B-14` (Share Readiness)** | Tách `board-settings.yaml` + bảng ánh xạ ngữ nghĩa cố định (`needs_you`, `waiting`, `working`, `done`) tách rời khỏi tên nhãn (`CHECK`, `GATE`, `CLOUD`, `WAIT`) và đọc tổng số phase từ định dạng `P10/13` trên title — thiết kế này trong `ROADMAP.md` dòng 231–235 của bạn cực kỳ thanh thoát và sẽ cho phép dùng chung một bản Mod `board` cho cả `andy-dev-board` (13 phases) lẫn `lane-board` (Matt Pocock flow). |

---

## Phần V — Danh Sách Đọc Nhanh Khi Bạn Mở Trên PC Khác

Khi bạn `git pull` repo này trên PC khác, hãy mở đọc theo đúng thứ tự ưu tiên sau:

1. **Cho `andy-dev-board` (Layer 3 — Multi-Lane & Claude Code Mods):**
   - [herdr_beginner_to_advanced_lustoykov.md](./herdr_beginner_to_advanced_lustoykov.md) *(Xem cơ chế Resource Pool Lock cho DB Clone `B-08` & phối hợp đa tác nhân)*
   - [s_tier_claude_code_mods_ray_amjad.md](./s_tier_claude_code_mods_ray_amjad.md) & [9_new_claude_mods_robonuggets.md](./9_new_claude_mods_robonuggets.md) *(Xem kỹ thuật thiết kế Claude Code Mods nâng cao, `cache-clock` auto-handoff & side pane)*
   - [youre_using_ai_agents_wrong_theo_t3.md](./youre_using_ai_agents_wrong_theo_t3.md) *(Xem quy tắc Inbox Zero `Settle`/`Snooze` & hạ tầng test không đụng độ)*
   - [tiny_local_model_outperforms_opus_orchestration_aicodeking.md](./tiny_local_model_outperforms_opus_orchestration_aicodeking.md) *(Xem cơ chế Process Parking giải phóng RAM cho Sleeping Lanes `B-04`)*
   - [claude_code_getting_rid_of_subagents_aicodeking.md](./claude_code_getting_rid_of_subagents_aicodeking.md) *(Xem tiêu chuẩn của Anthropic để quyết định giữa `Solo Lane` vs. `ADT Role Team` ở `L-02`)*
2. **Cho `andy-delivery-flow` (Layer 1 — 13-Phase Gate & Independent Review):**
   - [google_egm_skill_set_deepseek_aicodeking.md](./google_egm_skill_set_deepseek_aicodeking.md) *(Xem mô hình Elephant-Goldfish cho Phase 8/9 & `R-301` Expert Council)*
   - [pstack_how_cursor_engineer_ships_code_eric_tech.md](./pstack_how_cursor_engineer_ships_code_eric_tech.md) & [lauren_tan_orchestrate_40_agents_dune_pstack_viet_nguyen_ai.md](./lauren_tan_orchestrate_40_agents_dune_pstack_viet_nguyen_ai.md) *(Xem `/deslop`, `/no-comments`, `Feature Map`, Skill Evals & kiến trúc `Dune`)*
   - [claude_official_engineer_mode_anthropic_loop.md](./claude_official_engineer_mode_anthropic_loop.md) & [agent_harness_claude_code_codex_200lab.md](./agent_harness_claude_code_codex_200lab.md) *(Xem `hooks.json` & `Stop hook exit code 2` cho `H-001` và `H-002`)*
   - [the_best_git_worktree_tool_worktrunk_better_stack.md](./the_best_git_worktree_tool_worktrunk_better_stack.md) *(Xem `wt step copy-ignored` & `hash_port` cho Phase 1 / Phase 10 Verify Worktree)*
