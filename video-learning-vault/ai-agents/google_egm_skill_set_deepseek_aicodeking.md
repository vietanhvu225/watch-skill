# Google EGM (Elephant-Goldfish Model) + DeepSeek: Quy Trình Thiết Kế Với Voi & Phản Biện Với Cá Vàng (Re-analyzed)

- **Source URL:** https://www.youtube.com/watch?v=jyz4ALAahdE
- **Watch Skill ID:** `1d8d67cf063232ec`
- **Uploader:** AICodeKing
- **Duration:** 12:05 (725.0s)
- **Resolution:** 1280x720 (av1)
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Frames Analyzed:** 90/90 frames (`C:\Users\Admin\.watch-skill\frames\1d8d67cf063232ec\frame_0000.jpg` → `frame_0089.jpg`)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong video này, **AICodeKing** phân tích chuyên sâu và trình diễn thực chiến bộ plugin mã nguồn mở mới từ Google: **`google/elephant-goldfish-model` (EGM)** (giấy phép Apache 2.0, đạt ~2.600+ stars sau 6 tuần, được hàng nghìn kỹ sư Google sử dụng nội bộ).

Lấy cảm hứng từ bài tiểu luận *"Elephants, Goldfish and the New Golden Age of Software Engineering"* của **Dave Rensin** (27/04/2026), EGM giải quyết một trong những căn bệnh nguy hiểm nhất của các phiên làm việc AI kéo dài (**Long-session Blind Spots**): khi lập trình viên và Agent trò chuyện hàng giờ liền, cả hai hình thành một loạt **giả định ngầm chung (shared assumptions)** khiến bản thiết kế viết ra trông có vẻ hoàn hảo với cả hai, nhưng thực chất lại bỏ sót những lỗ hổng chí mạng khi bước vào cài đặt code.

### Ba Vai Trò Cốt Lõi Trong Mô Hình Voi – Cá Vàng (EGM Triad)

| Vai Trò (Role) | Ngữ Cảnh (Context) | Trách Nhiệm (Responsibility) |
| :--- | :--- | :--- |
| **🐘 Elephant (Voi)** | Toàn bộ ngữ cảnh dự án, yêu cầu và lịch sử các quyết định trong cuộc hội thoại (`remembers everything`). | Khám phá bài toán, đặt câu hỏi làm rõ ranh giới và soạn thảo đặc tả thiết kế (`design_draft.md`) từng mục một. |
| **🐠 Goldfish (Cá Vàng)** | **Sub-agent mới hoàn toàn (`0 memory`, `fresh context`)** — chỉ được đọc duy nhất file `design.md` và các file mã nguồn được tham chiếu trực tiếp, kèm công cụ chỉ đọc (`read-only tools`). | Đóng vai người đọc hoàn toàn mới để phát hiện các giả định bị thiếu, thuật ngữ mơ hồ và rủi ro kiến trúc; bị cấm tuyệt đối khen ngợi xã giao (`No Glazing / No Flattery`). |
| **👤 You (Con Người)** | Mục tiêu kinh doanh và các ràng buộc thực tế quan trọng. | Phân xử các đánh đổi (trade-offs), phê duyệt thiết kế và ra quyết định cuối cùng về những gì sẽ được xây dựng. |

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
│                      GOOGLE ELEPHANT-GOLDFISH MODEL (EGM) — FULL WORKFLOW                       │
├─────────────────────────────────────────────────────────────────────────────────────────────────┤
│  [0. /egm-bootstrap]  Quét codebase từ dưới lên (bottom-up) → sinh README mỗi thư mục cho Voi   │
│         │                                                                                       │
│         ▼                                                                                       │
│  [1. /egm-brainstorm] Voi hỏi 4 câu chốt biên ──► Tung 6 Goldfish song song (6 góc nhìn)        │
│         │             ──► Xuất bản định hướng `concepts_brief.md`                               │
│         ▼                                                                                       │
│  [2. /egm-draft]      Voi viết `design_draft.md` TỪNG MỤC MỘT (Overview → Tech Plan...)         │
│         │             ──► Dừng sau mỗi mục để chờ người dùng Approve / Sửa trực tiếp            │
│         ▼                                                                                       │
│  [3. /egm-review]     5 Tầng Cá Vàng độc lập (`fresh subagents`, `omitClaudeMd: true`):         │
│         │             Stage 1: Zoom Out ──► Stage 2: Comprehension ──► Stage 3: Design Critic   │
│         │             ──► Stage 4: Zoom Out 2 ──► Stage 5: Implementation Readiness             │
│         │             ──► Xuất `consolidated_review_report.md` (Blocker / Major / Minor)        │
│         ▼                                                                                       │
│  [4. /egm-resolve]    Lập bảng `PROPOSE FIX` vs `CONTEST` trong `goldfish_review_report.md`     │
│         │             ──► Người dùng chốt (`auto accept`) trước khi ra lệnh viết code           │
│         ▼                                                                                       │
│  [5. /egm-drift-report] (Hàng tuần) Đối chiếu `design.md` với code thực tế để bắt lệch chuẩn    │
└─────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Ý Nghĩa Chiến Lược: Google Đem Quy Trình Lên Mọi Harness (`One Set of Skills, No Forks`)

Một chi tiết khiến cộng đồng bất ngờ (`00:18` – `01:33`, `10:01` – `11:00` | `frame_0004` – `frame_0012`, `frame_0083` – `frame_0086`):
- Dù Google sở hữu hệ sinh thái **Gemini, Gemini CLI, Jules và Antigravity** cạnh tranh trực tiếp với Anthropic và OpenAI, kho lưu trữ `google/elephant-goldfish-model` lại hỗ trợ hạng nhất (**first-class support**) cho:
  1. **Antigravity:** `agy plugin install https://github.com/google/elephant-goldfish-model` (hoặc `/plugin` trong IDE).
  2. **Claude Code:** `claude plugin marketplace add google/elephant-goldfish-model` && `claude plugin install egm@egm`.
  3. **Claude Cowork:** Cài từ `claude.ai/plugins` hoặc bảng plugin Cowork qua marketplace `google/elephant-goldfish-model`.
  4. **Codex:** `codex plugin marketplace add google/elephant-goldfish-model` && `codex plugin add egm@egm`.
  5. **ChatGPT Work:** Import marketplace qua repo URL, path `.agents/plugins`.
  6. **Cline & PandaCode:** Chạy trực tiếp vì toàn bộ EGM chỉ gồm **7 file `SKILL.md` thuần Markdown** và **9 file persona trong `agents/`**.
- **Triết lý "Không Fork Skill Text" (`frame_0068`, `frame_0069`, `frame_0084`):**
  > *"EGM is one set of skills and personas. This repository serves as the plugin source for each supported harness. No harness gets a fork of the skill text."*
  - File `plugin.json` ở thư mục gốc là nguồn chuẩn (canonical source) theo schema Agent Plugins 1.0.0.
  - Mọi harness dùng chung đúng một bộ `skills/`, `agents/` và `resources/document_standards/`. Mỗi harness chỉ ánh xạ cụm từ *"spawn a subagent"* sang công cụ tạo sub-agent riêng của mình.
- **Lưu ý cài đặt quan trọng (`03:05` – `03:16` | `frame_0023`):** Phải cài đặt hoặc clone **toàn bộ plugin repo**, tuyệt đối **không chỉ copy thư mục `skills/`**, vì ở thời điểm chạy (runtime), 7 file `SKILL.md` trong `skills/` sẽ đọc trực tiếp các file đóng vai phản biện trong `agents/` và tiêu chuẩn tài liệu trong `resources/document_standards/`.

### Cấu Trúc 7 Skills & 9 Agent Personas Trong Repo (`frame_0009`, `frame_0066`)

- **7 Skills (`skills/*/SKILL.md`):**
  1. `skills/egm_bootstrap/SKILL.md`
  2. `skills/egm_brainstorm/SKILL.md`
  3. `skills/egm_draft/SKILL.md`
  4. `skills/egm_drift_report/SKILL.md`
  5. `skills/egm_resolve/SKILL.md`
  6. `skills/egm_review/SKILL.md`
  7. `skills/egm_review_loop/SKILL.md`
- **9 Agent Personas (`agents/` & `distributions/claude/agents`):**
  1. `bootstrap_orchestrator.md`
  2. `codebase_details.md`
  3. `comprehension.md`
  4. `design_critic.md`
  5. `implementation_readiness.md`
  6. `structural.md`
  7. `style.md`
  8. `synthesizer.md`
  9. `zoom_out.md`

---

## 3. Giải Phẫu Quy Trình Thực Chiến 4 Bước Chính + 2 Bước Bổ Trợ (`03:19` – `06:08`)

AICodeKing demo trực tiếp trên dự án mẫu `hookshot` trong **Claude Code v2.1.31** với bài toán: *"Add webhook delivery to this project"*.

### Bước 1: `/egm:egm-brainstorm` — Chốt Biên & Tung 6 Cá Vàng Đa Góc Nhìn (`03:25` – `03:54` | `frame_0026` – `frame_0028`)

1. **Voi đặt 4 câu hỏi chốt ranh giới trước khi gọi bất kỳ Cá Vàng nào (`frame_0026`):**
   > *"Before any Goldfish sees this I need the boundaries. Four questions:"*
   > 1. **Retries:** Khi gửi thất bại (timeout, 5xx), thử lại trong bao lâu và điều gì xảy ra sau lần thử cuối?
   > 2. **Duplicates:** Cùng một sự kiện có thể tới receiver 2 lần không, hay receiver đòi hỏi exactly-once?
   > 3. **Ordering:** Receiver có cần nhận sự kiện theo đúng thứ tự không, hay thứ tự bất kỳ đều được?
   > 4. **Signing:** Làm sao receiver biết request thực sự đến từ `hookshot`?
2. **Người dùng trả lời ngắn gọn (`frame_0027`):**
   `24h of retries, then dead-letter. At-least-once + event id. Any order. HMAC signing.`
3. **Tung đồng thời 6 Goldfish Reviewers chạy song song trong ngữ cảnh mới (`frame_0027`, `frame_0028`):**
   Mỗi Goldfish soi giả thuyết tính năng qua một lăng kính (lens) chuyên biệt:
   - **Lens 1 — `Contrarian / YAGNI lens`** (23.8k tokens): Chất vấn tại sao tính năng này cần tồn tại và tự động grep `package.json` tìm xem các thư viện có sẵn (`bull | bee-queue | pg-boss`) đã giải quyết 90% bài toán chưa.
   - **Lens 2 — `Cost, Quota & Token Economics lens`** (17.3k tokens): Soi chi phí hạ tầng, hạn ngạch và tần suất `INSERT INTO events`.
   - **Lens 3 — `UX & Developer Ergonomics lens`** (16.6k tokens): Đọc `README.md` để đánh giá trải nghiệm tích hợp của developer.
   - **Lens 4 — `Migration & Backward Compatibility lens`** (18.2k tokens): Kiểm tra `ls src/db/migrations` để đánh giá tương thích ngược.
   - **Lens 5 — `Observability & SRE Debuggability lens`** (19.3k tokens): Đọc `src/lib/logger.ts` để kiểm tra khả năng truy vết lỗi vận hành.
   - **Lens 6 — `Long-Term Tech Debt & Maintenance lens`** (16.3k tokens): Tìm kiếm `eventType` trong `src/` để đánh giá nợ kỹ thuật dài hạn.
4. **Đầu ra:** Xuất file `concepts_brief.md` ghi lại định hướng đã thống nhất.

---

### Bước 2: `/egm:egm-draft` — Soạn Thảo Thiết Kế Từng Mục & Dừng Chờ Phản Hồi (`03:55` – `04:09` | `frame_0029` – `frame_0034`)

- Thay vì đổ ồ ạt một file thiết kế dài hàng trăm dòng cùng lúc, `/egm:egm-draft` viết file `design_draft.md` **từng phần một (section by section)** rồi dừng lại hỏi ý kiến người dùng (`frame_0029`):
  - Viết xong mục `Overview & Goals` (25 dòng đầu) ──► hiện menu tương tác:
    1. `Approve Overview & Goals and proceed to next section`
    2. `Request revisions to Overview & Goals (via inline comments or write-in)`
    3. `Type something.`
    4. `Chat about this`
- Khi viết tiếp mục `## Technical Plan (Architecture)` (`frame_0030` – `frame_0034`), ban đầu Voi đề xuất tự viết `outbox worker` và `src/webhooks/deliver.ts`. Người dùng phản hồi ngay tại chỗ: *"Use pg-boss as the queue. We own signing + endpoints."*
- Voi lập tức cập nhật lại `design_draft.md` (`frame_0033`, `frame_0034`):
  ```text
  POST /events ──► pg-boss queue (Postgres) ──► deliver job ──► POST <endpoint>
  pg-boss owns retries (backoff, 24 h) and its dead-letter queue.
  We own: `src/webhooks/sign.ts` and the `webhook_endpoints` table.
  ```

---

### Bước 3: `/egm:egm-review` — 5 Tầng Phản Biện Cá Vàng Độc Lập (`04:10` – `04:48` | `frame_0035` – `frame_0039`)

Khi chạy `/egm:egm-review design_draft.md`, agent cho phép chọn chế độ thực thi (`frame_0036`):
1. `Interactive review with all review rubrics` (kiểm tra từng chặng).
2. `Consolidated report with all review rubrics` (chạy trọn vẹn cả 5 tầng không dừng).
3. `Feature spec only` (chỉ đánh giá thiết kế tính năng).
4. `Implementation plan only` (chỉ đánh giá mức độ sẵn sàng cài đặt).

**5 Giai Đoạn Phản Biện (5 Stages of Goldfish Review — `frame_0037`, `frame_0058`):**
1. **Stage 1 — `Zoom Out` (`agents/zoom_out.md`):** Kiến trúc sư hoài nghi (Skeptical Architect) nhìn tổng thể xem có phần nào quá phức tạp hoặc không cần thiết.
2. **Stage 2 — `Comprehension` (`agents/comprehension.md`):** Người đọc ngây thơ (Naive Reader) gắn cờ mọi thuật ngữ, từ viết tắt hoặc khái niệm dùng lẫn lộn mà chưa được định nghĩa rõ (ví dụ: dùng lẫn lộn `"delivery"` và `"attempt"` ở mục §3 và §5 — `frame_0063`).
3. **Stage 3 — `Design Critique` (`agents/design_critic.md`):** **Hostile Design Critic** — tấn công trực diện vào kiến trúc hệ thống, khả năng mở rộng, kinh tế học hạ tầng và các rủi ro chưa được giảm thiểu (`frame_0037`).
4. **Stage 4 — `Zoom Out` (Lần 2):** Nhìn tổng thể lại một lần nữa sau khi đã mổ xẻ chi tiết.
5. **Stage 5 — `Implementation Readiness` (`agents/implementation_readiness.md`):** Tech Lead thực dụng kiểm tra xem bản thiết kế có còn bỏ ngỏ quyết định thiết kế nào đẩy sang cho người viết code tự đoán hay không.

**Đầu ra (`frame_0039`):** Xuất báo cáo tổng hợp `consolidated_review_report.md` (ví dụ: **23 findings từ 5 stages**, phân loại rõ `🔴 Blocker`, `🟡 Major`...).

---

### Bước 4: `/egm:egm-resolve` — Phân Xử Đề Xuất Sửa Đổi (`04:49` – `04:58` | `frame_0039`)

- Chạy `/egm:egm-resolve consolidated_review_report.md` sẽ tổng hợp thành file `goldfish_review_report.md` có đánh nhãn khuyến nghị của AI cho từng vấn đề (`frame_0039`):
  - `### [Issue 1] ('egm-design') - Severity: High` ──► `**AI Suggestion:** PROPOSE FIX`
  - `### [Issue 2] ('egm-zoom-out') - Severity: High` ──► `**AI Suggestion:** CONTEST` (bác bỏ nếu reviewer yêu cầu thái quá).
- Người dùng có thể gõ `auto accept` hoặc duyệt từng mục để chốt các thay đổi vào bản thiết kế trước khi ra lệnh code.

---

### Bước 5 & 6: Hai Kỹ Năng Bổ Trợ — `/egm-bootstrap` & `/egm-drift-report` (`05:43` – `06:08` | `frame_0046`, `frame_0047`)

1. **`/egm:egm-bootstrap` (`frame_0046`, `frame_0047`):**
   - Duyệt toàn bộ codebase hiện có **từ dưới lên (`bottom up`)**: viết file `README` cho từng thư mục lá (`api/`, `db/`, `ui/`, `lib/`), sau đó tổng hợp ngược lên tầng trung gian (`server/`, `web/`) và thư mục gốc (`app/`).
   - Giúp **Voi (Elephant)** có sẵn một tấm bản đồ (`a map of your project`) chuẩn xác trước khi bắt đầu thiết kế tính năng mới.
2. **`/egm:egm-drift-report` (`05:57` – `06:08`):**
   - So sánh các tài liệu thiết kế (`design.md`) với mã nguồn thực tế hiện tại để chỉ ra những điểm đã bị lệch (drift) sau khi merge code. Được khuyến nghị chạy **mỗi tuần 1 lần**.

---

## 4. Hai Nguyên Tắc Thiết Kế Cứng Làm Nên Chất Lượng Của EGM (`04:59` – `05:42`)

### 4.1. Cấm Tuyệt Đối Nịnh Hót (`No Glazing / Banned Adjectives` — `frame_0041`, `frame_0042`)

Trích xuất trực tiếp dòng 44–61 của `skills/egm_review/SKILL.md` trên terminal (`frame_0041`):
- **Rule 3 — `Default to Stress-Testing`:** Không bao giờ đồng tình mặc định. Nhiệm vụ số 1 là tìm điểm yếu nhất, edge cases và điểm mù trước khi khẳng định bất cứ điều gì.
- **Rule 4 — `No Glazing`:** Loại bỏ hoàn toàn thói nịnh bợ (sycophancy). Không gọi ý tưởng là *"great"*, *"brilliant"* hay *"smart"*. Lời khen không có nội dung là rác nhiễu.
- **Rule 5 — `Critique First`:** Kể cả khi ý tưởng rất mạnh, phải mở đầu bằng những gì sai, thiếu hoặc rủi ro trước khi nêu ưu điểm.
- **Rule 6 — `Reject Echo-Chamber Framing`:** Không nhại lại cách đặt vấn đề của người dùng.
- **Rule 7 — `Generate Counter-Arguments`:** Với mọi chiến lược được đề xuất, phải nêu lập luận phản bác mạnh nhất (*"What is the strongest counter-argument?"*).
- **Rule 8 — `Earn Your Agreement`:** Chỉ đồng ý khi ý tưởng đã sống sót qua sự săm soi khách quan khắt khe.
- **Rule 9 — `Banned Adjectives (No Flattery)`:** Cấm tuyệt đối dùng các tính từ vuốt ve như `"brilliant"`, `"great"`, `"smart"`, `"correct"`, `"perfect"`, `"excellent"`, `"spot-on"` khi nói về input hoặc quyết định của người dùng.

### 4.2. Luật Cứng Về Cách Ly Ngữ Cảnh (`No Subagent Tool → EGM Stops` — `frame_0044`, `frame_0045`)

- Trích dòng 20 của `skills/egm_review/SKILL.md` (`frame_0044`):
  > *"If your harness has no subagent tool, stop and tell the user: a review in the same context is not a Goldfish review."*
- Trong adapter của Claude Code (`.claude-plugin/agents/design_critic.md` — `frame_0044`), mỗi Goldfish reviewer bị khóa chặt:
  ```yaml
  name: egm-design
  description: Architecture and design critic.
  license: Apache-2.0
  tools: [Read, Grep, Glob]
  model: inherit
  color: yellow
  omitClaudeMd: true
  ```
  - Chỉ có 3 công cụ chỉ đọc: `[Read, Grep, Glob]` (`Remain read-only`).
  - Bật cờ **`omitClaudeMd: true`** để không bị nhiễm cả file hướng dẫn phiên làm việc chung, giữ cho Cá Vàng thực sự là một Cá Vàng với bộ nhớ bằng 0 (`the goldfish stays a goldfish`).

---

## 5. Bài Toán Đốt Token & Chiến Lược Phối Hợp Đa Model Với ClinePass / PandaCode (`06:10` – `10:00`)

### 5.1. Khi Nào KHÔNG Nên Dùng EGM (`frame_0049` – `frame_0052`)
- FAQ của repo rất trung thực (`frame_0050`): Với các bản vá lỗi nhỏ, rõ ràng (small, well-understood bug fixes), hãy dùng luồng code bình thường; chạy EGM sẽ làm chậm tốc độ vô ích.
- **Chi phí Token (`frame_0051`, `frame_0052`):** Một vòng review đơn lẻ kích hoạt **5–6 sub-agents mới tinh**, mỗi sub-agent đều đọc lại từ đầu bản thiết kế và toàn bộ các file code tham chiếu. Nếu chạy 2–3 vòng review trên gói **\$20/tháng** của Claude hoặc Codex, bạn sẽ chạm trần giới hạn (usage limit) rất nhanh.

### 5.2. Giải Pháp Kinh Tế & Đa Dạng Hóa Góc Nhìn: Mix Models Qua ClinePass (`\$9.99/tháng`) & PandaCode (`frame_0053` – `frame_0082`)

AICodeKing chỉ ra một lợi thế kiến trúc cực hay khi kết hợp EGM với **ClinePass** (\$9.99/tháng — cung cấp hạn mức gấp 2–5 lần giá API chuẩn cho các model mã nguồn mở mạnh nhất như `Kimi K3.1 Pro`, `DeepSeek V4.1 Flash`, `GLM 5.3`, `GLM 5.3 Flash`, `MiniMax M3.1 Flash`, `Qwen3.8 Max`):

> **Insight cốt lõi (`07:48` – `08:25` | `frame_0059`, `frame_0060`):**
> Mục đích của Cá Vàng là không chia sẻ giả định với Voi. Nếu **Cá Vàng còn chạy trên một Model khác hẳn với Voi**, nó thậm chí **không chia sẻ cùng thói quen suy luận hay điểm mù huấn luyện (model blind spots)** của model viết thiết kế!

**Công thức phối hợp Model tối ưu (Model Mixing Combo — `frame_0061` – `frame_0063`, `frame_0078`):**

| Vai Trò Trong EGM | Model Khuyên Dùng (ClinePass) | Lý Do Kỹ Thuật & Lệnh Thực Thi Mẫu |
| :--- | :--- | :--- |
| **🐘 Elephant (Voi — Brainstorm & Draft)** | **`Kimi K3.1 Pro`** (`cline-pass/kimi-k3.1-pro`) | Rất mạnh về hội thoại dài hơi, cẩn trọng và viết tài liệu thiết kế mạch lạc. |
| **🐠 Goldfish Nặng (`Design Critic` & `Implementation Readiness`)** | **`GLM 5.3`** (`cline-pass/glm-5.3`) | Cực kỳ sắc bén khi đọc hiểu mã nguồn thực tế (`src/db/schema.sql`, `src/queue/poller.ts`).<br>`cline -m cline-pass/glm-5.3 "Use ~/egm/skills/egm_review/SKILL.md: critic + readiness only"` |
| **🐠 Goldfish Nhẹ (`Comprehension` / `Zoom Out`)** | **`DeepSeek V4.1 Flash`** hoặc **`MiniMax M3.1 Flash`** | Tốc độ phản hồi cực nhanh, bắt lỗi thuật ngữ chuẩn xác.<br>`cline -m cline-pass/deepseek-v4.1-flash "Use ~/egm/skills/egm_review/SKILL.md: comprehension only"` |

### 5.3. Thiết Lập Đội Hình Tự Động Trong PandaCode (`09:26` – `10:00` | `frame_0078` – `frame_0082`)

Trong **PandaCode**, chế độ **Team (`Architect & workers`)** khớp hoàn hảo với mô hình Voi – Cá Vàng:
- **Architect (= The Elephant):** Đặt là `Kimi K3` — giữ toàn bộ ngữ cảnh cuộc trò chuyện.
- **4 Workers chạy song song (= Fresh Goldfish Reviewers — `frame_0078`, `frame_0082`):**
  1. Worker 1 (`Zoom out`): `GLM-5.3`
  2. Worker 2 (`Comprehension`): `DeepSeek V4.1 Flash`
  3. Worker 3 (`Design critic`): `GLM-5.3`
  4. Worker 4 (`Implementation readiness`): `MiniMax M3`
- Khi bấm chạy, cả 4 Goldfish workers khởi chạy đồng thời trong các phiên chỉ đọc tách biệt và trả kết quả về cùng một giao diện tổng hợp trong ~15 giây.
