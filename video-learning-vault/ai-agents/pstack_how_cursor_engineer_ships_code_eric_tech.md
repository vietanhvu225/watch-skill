# Giải Phẫu Pstack: Cách Kỹ Sư Cursor Vận Hành & Giao Mã Bằng AI | Eric Tech (Phân Tích Ngăn Xếp Của Lauren Tan)

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=4wBVuj2an8E)
- **Video ID:** `4wBVuj2an8E`
- **Watch Skill ID:** `a0f672555e224ef8`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 14:56
- **Speaker:** Eric Tech (Phân tích bộ kỹ năng mã nguồn mở `pstack` của Lauren Tan `poteto` — Principal Engineer tại SpaceXAI, cựu Member of Technical Staff tại Cursor, cựu Staff Engineer tại Meta & React Core Team)
- **Visual Frames:** 75 khung hình trích xuất tại `C:\Users\Admin\.watch-skill\frames\a0f672555e224ef8\` (`frame_0000.jpg` – `frame_0074.jpg`)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong video này, **Eric Tech** giải phẫu toàn diện bộ công cụ mã nguồn mở **`pstack` (Potato Stack)** do **Lauren Tan** (`poteto`) xây dựng và công bố tại repository chính thức `cursor/plugins` (`plugins/pstack/skills/`, phiên bản Cursor plugin `0.15.17` và bản port `pstack-claude` `0.9.82`, xuất hiện ở `frame_0004`–`frame_0005`). Lauren Tan là cựu thành viên React Core Team tại Meta, cựu kỹ sư tại Cursor và hiện là Principal Engineer tại SpaceXAI (sau thương vụ sáp nhập Cursor, trực tiếp phát triển Grok Bot & Cursor — `frame_0000`–`frame_0003`).

Triết lý cốt lõi của `pstack` được gói gọn trong câu châm ngôn trên trang chủ (`frame_0005`):
> *"if you want to go fast, go deep first. pstack helps you write less, but higher quality code. rigorous agent workflows you can parallelize with confidence."*

Eric lập bản đồ toàn bộ 30+ skills và principles của `pstack` lên bảng tương tác **Super Board Skill Map** theo **6 Giai Đoạn Vòng Đời Phát Triển Phần Mềm AI-Native (AI-SDLC)** (`Plan` ➔ `Design` ➔ `Build` ➔ `Test` ➔ `Deploy` ➔ `Maintain` + `Anytime` skills, `frame_0006`–`frame_0010`), làm nổi bật 6 điểm khác biệt lớn nhất so với các bộ skill phổ biến (như Matt Pocock hay Superpowers):

1. **Tối ưu hóa tuyệt đối cho Dự án Hiện hữu (Brownfield Mastery):** Thay vì tập trung vào greenfield cho solo builder, `pstack` mở đầu bằng quy trình *Learn the code first* (`/how` ➔ `/why` ➔ `/teach`) và lập kế hoạch cho các đợt migration quy mô lớn (`/figure-it-out` ➔ `/show-me-your-work`).
2. **Thiết kế Hình Thái Mã Nguồn Đa Mô Hình (`/architect` ➔ `/arena` ➔ `/interrogate`):** Chốt kiểu dữ liệu (Type signatures) trước khi viết bất kỳ dòng code triển khai nào; cho nhiều mô hình (Opus, GPT, Grok) đua phương án song song trong `/arena` và dùng `/interrogate` tổng hợp báo cáo thiết kế tối ưu.
3. **Kỷ Luật "Xóa Sạch Comments" (`/deslop` & `/no-comments`):** Xóa bỏ triệt để chú thích thừa vì khi comment (`// returns A`) mâu thuẫn với code thực tế (`return B;`), các mô hình AI thường tin vào comment cũ dẫn đến ảo giác dây chuyền (`frame_0025`–`frame_0027`).
4. **Kỹ Năng Xác Minh Đầu Cuối Tự Sinh (`/create-verification-skill` & `/swarm`):** Agent tự đọc codebase để sinh file `verify-<app>/SKILL.md` (gồm 6 phần: `Launch, Doctor, Drive, Evidence, Cleanup, Helpers`) và thư mục `features/` (mỗi tính năng 1 file), sau đó dùng `/swarm` phân tán nhiều worker kiểm thử song song trên ứng dụng thật (`frame_0035`–`frame_0039`).
5. **Kiến Trúc `skills-as-index` Thay Thế `system-prompt.md` Phình To:** Tách các quy tắc kỹ thuật thành các file `principle-*` nạp theo ngữ cảnh (on-demand) thay vì nhồi toàn bộ vào `system-prompt.md` gây lãng phí token ở mọi lượt hội thoại (`frame_0061`–`frame_0063`).
6. **Hai Nguyên Tắc Vàng (`attack-the-premise` & `type-system-discipline`):** Tự động phanh gấp khi 2 lần sửa lỗi liên tiếp cùng dựa trên một giả định đều thất bại (`frame_0065`–`frame_0067`), và dùng hệ thống kiểu biên dịch để cấm tuyệt đối các trạng thái bất khả thi (`frame_0069`–`frame_0071`).

```
┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
│                        PSTACK 6-STAGE AI-SDLC SKILL MAP (LAUREN TAN / POTETO)                    │
├──────────────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. PLAN (Brownfield & Unattended Jobs)                                                           │
│    • Learn the code first : /how ──► /why ──► /teach                                             │
│    • Plan big/unattended  : /figure-it-out ──► /show-me-your-work                                │
│                                                │                                                 │
│                                                ▼                                                 │
│ 2. DESIGN (Settle the Code Shape — 0 Lines of Implementation Code)                               │
│    • /architect (Sketch types first) ──► /arena (Multi-model fanout) ──► /interrogate            │
│                                                │                                                 │
│                                                ▼                                                 │
│ 3. BUILD (Test-First & Diff Hygiene)                                                             │
│    • Write it test-first  : /tdd (Quick bug fix, skips bad tests) ──► /typescript-best-practices │
│    • Clean up the diff    : /deslop (Strip AI junk vs main) ──► /no-comments                     │
│                                                │                                                 │
│                                                ▼                                                 │
│ 4. TEST (Real App Proof, Blast Radius & Thermonuclear Audit)                                     │
│    • Prove on real app    : /create-verification-skill (verify-<app>/SKILL.md) ──► /swarm        │
│    • Vet change & number  : /blast-radius ──► /benchmark-checklist                               │
│    • Attack the diff      : /interrogate ──► /thermo-nuclear-code-quality-review (>1,000 lines)  │
│                                                │                                                 │
│                                                ▼                                                 │
│ 5. DEPLOY (Get the PR Merged) & 6. MAINTAIN                                                      │
│    • /make-pr-easy-to-review ──► /babysit ──► /fix-merge-conflicts ──► /get-pr-comments ──► /fix-ci│
│                                                │                                                 │
│                                                ▼                                                 │
│ ∞. ANYTIME SKILLS & MODULAR PRINCIPLES (`principle-*` loaded via `skills-as-index`)              │
│    • Router & Setup       : /poteto-mode, /setup-pstack, /poteto-help                            │
│    • Catch up & Plain talk: /recall, /ketchup, /bro, /what-did-i-get-done                        │
│    • Core Principles      : attack-the-premise, type-system-discipline, laziness-protocol, etc.  │
└──────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Giải Phẫu Chi Tiết 6 Giai Đoạn AI-SDLC Trên Bản Đồ Pstack

### Giai Đoạn 1: Plan — Thấu Hiểu Codebase Hiện Hữu (`01:15 – 02:35`, `frame_0010`–`frame_0013`)
Khác với các bộ skill dành cho nhà sáng lập độc lập xây dựng MVP từ con số 0 (như Matt Pocock hay Superpowers), `pstack` được sinh ra từ môi trường đại công ty (Meta, Cursor, SpaceXAI) nên đặt trọng tâm vào **Brownfield projects** (dự án lớn đang vận hành):
- **Luồng 1 — *Learn the code first* (`frame_0012`):**
  - `/how` (*explain how code works*): Giải thích cơ chế vận hành thực tế của luồng mã nguồn hiện tại.
  - `/why` (*find why code was built*): Truy vết lịch sử `git blame`, PRs, ADRs để hiểu lý do tại sao kiến trúc cũ lại được viết như vậy trước khi dám đập đi xây lại.
  - `/teach` (*explain your work simply*): Tổng hợp lại cả *how* và *why* thành bản giải thích ngắn gọn cho kỹ sư.
- **Luồng 2 — *Plan a big or unattended job* (`frame_0012`–`frame_0013`):**
  - `/figure-it-out` (*design custom playbook for job*): Thiết kế kịch bản thực thi tùy chỉnh cho các đợt di chuyển hệ thống (migrations) hoặc tác vụ chạy tự động không người giám sát.
  - `/show-me-your-work` (*log every decision made*): Ghi nhật ký mọi quyết định kiến trúc của agent ra file Markdown để kỹ sư kiểm toán sau khi job hoàn tất.

---

### Giai Đoạn 2: Design — Định Hình Kiến Trúc Trước Khi Viết Code (`02:35 – 03:45`, `frame_0014`–`frame_0017`)
Mục tiêu của giai đoạn Design là **Settle the code shape** (Chốt hình thái mã nguồn và kiểu dữ liệu) khi chưa viết một dòng code logic nào:
- **`/architect` (*sketch types first, then code*, `frame_0015`):** Phác thảo toàn bộ các cấu trúc dữ liệu và chữ ký hàm (ví dụ trong `frame_0015`: `type Order`, `interface PaymentGateway`, `type Receipt`) trước khi đi vào chi tiết hàm.
- **`/arena` (*build rival versions, pick best*, `frame_0014`, `frame_0016`):** Kích hoạt nhiều mô hình đối thủ (Opus, GPT, Grok) chạy song song trong các git worktrees độc lập để cùng giải bài toán thiết kế, sau đó chọn ra kiến trúc xuất sắc nhất hoặc lai ghép các phần tốt nhất.
- **`/interrogate` (*have AI models attack diff*, `frame_0014`, `frame_0017`):** Cho các mô hình AI đóng vai trò phản biện tấn công trực diện vào bản thiết kế/diff từ nhiều góc độ và gom kết quả về một báo cáo tổng hợp duy nhất.

---

### Giai Đoạn 3: Build — TDD Linh Hoạt & Khử Rác AI (`03:45 – 06:00`, `frame_0018`–`frame_0029`)

#### Luồng 1: *Write it test-first* (`/tdd` ➔ `/typescript-best-practices`)
- **`/tdd` (*write failing test, fix bug*, `frame_0018`–`frame_0019`):** Điểm đặc biệt của `/tdd` trong `pstack` là quy tắc: *"Write a failing test that reproduces the bug before fixing it; **skip the test if it would be a bad one**."* (Nếu việc viết test đòi hỏi mock quá nhiều thành phần giả tạo không mang lại giá trị thực, agent được phép bỏ qua bài test đó).
- **Bảng so sánh 3 trường phái TDD hàng đầu (`frame_0020`–`frame_0021`):**

| Tiêu chí | `pstack` (`/tdd`) | `Matt Pocock` (`/tdd`) | `Superpowers` (`test-driven-development`) |
| :--- | :--- | :--- | :--- |
| **Phù hợp nhất (Best for)** | **Quick bug fix** (Sửa bug nhanh) | **Overall** (Toàn diện nhất) | **Strict team discipline** (Kỷ luật nhóm nghiêm ngặt) |
| **Đặc tính kiểm thử** | Soft (Mềm dẻo) / Skips bad tests / Fastest | Firm rules (Quy tắc vững) / Agreed test spots | Iron law (Luật sắt) / Deletes pre-test code (Xóa code viết trước test) |

- **`/typescript-best-practices` (`frame_0018`):** Áp đặt các quy chuẩn TypeScript nghiêm ngặt ngay trong lúc viết code.

#### Luồng 2: *Clean up the diff* (`/deslop` ➔ `/no-comments`)
- **`/deslop` (*strip AI junk from diff*, `frame_0022`–`frame_0025`):** Khi chạy trên một nhánh tính năng trước lúc mở PR, `/deslop` đối chiếu toàn bộ diff với nhánh `main`, tự động săn lùng và dọn sạch 5 loại "rác AI" (`frame_0025`):
  1. `extra comments` (Chú thích dư thừa)
  2. `try / catch` (Khối bắt lỗi phòng thủ không cần thiết)
  3. `error exceptions` (Ném ngoại lệ chung chung vô nghĩa)
  4. `any without types` (Kiểu `any` làm thủng hệ thống kiểm tra kiểu)
  5. `deep nesting` (Vòng lặp/điều kiện lồng nhau quá sâu)
  Sau đó chạy lại bộ test để xác nhận hành vi không đổi và xuất tóm tắt 1–3 câu.
- **`/no-comments` (*delete needless code comments*, `frame_0026`–`frame_0029`):**
  - Eric minh họa trực quan trong `frame_0027`: Khi trong hàm `calculateOrder()` xuất hiện dòng comment cũ `// returns A` nhưng dòng lệnh bên dưới đã được sửa thành `return B;`, các LLM đọc ngữ cảnh thường bị dòng comment đánh lừa và suy luận rằng hàm trả về `A`.
  - Do đó, `pstack` xóa sạch mọi comment mô tả *what/how* mà bản thân tên hàm và kiểu dữ liệu đã thể hiện rõ, chỉ giữ lại những ghi chú thực sự giải thích *why* (tại sao phải dùng workaround).

---

### Giai Đoạn 4: Test — Chứng Minh Trên Ứng Dụng Thật & Kiểm Toán Hạt Nhân (`06:00 – 09:10`, `frame_0030`–`frame_0043`)

#### Luồng 1: *Prove it on the real app* (`/create-verification-skill` ➔ `/swarm`)
- **`/create-verification-skill` (*build reusable app verification*, `frame_0032`–`frame_0036`):** Khác hoàn toàn với unit test, skill này biến quy trình chạy thử ứng dụng thực tế thành một kỹ năng tái sử dụng được qua **5 bước tự động (`frame_0035`–`frame_0036`)**:
  1. **Đọc trực tiếp từ repo (không hỏi người dùng):** Tự tìm hiểu cách khởi chạy ứng dụng, điều khiển UI/API và thu thập bằng chứng.
  2. **Ghi file `verify-<app>/SKILL.md`:** Bao gồm đầy đủ 6 mục chuẩn hóa: `Launch` (Khởi tạo DB/server trên cổng cô lập), `Doctor` (Kiểm tra sức khỏe môi trường), `Drive` (Điều khiển qua trình duyệt/CLI), `Evidence` (Chụp ảnh màn hình/video/log làm bằng chứng), `Cleanup` (Tắt tiến trình và xóa dữ liệu rác), và `Helpers`.
  3. **Tạo thư mục `features/`:** Mỗi tính năng được tách thành 1 file kịch bản kiểm tra riêng biệt (`one file per feature`).
  4. **Tự chạy kiểm nghiệm End-to-End 1 lần:** Đảm bảo chính kịch bản vừa viết hoạt động trơn tru từ đầu đến cuối.
  5. **Trỏ sang skill bảo trì (`maintain-verification`):** Để cập nhật khi UI/API thay đổi về sau.
- **`/swarm` (*run many workers at once*, `frame_0037`–`frame_0039`):** Khi ứng dụng có hàng chục trang hoặc tính năng trong thư mục `features/`, `/swarm` chia nhỏ công việc cho nhiều sub-agents (`worker 1`, `worker 2`, `worker 3`...) chạy kiểm thử song song rồi gom kết quả về một báo cáo tổng hợp (`report`).

#### Luồng 2: *Vet the change and the number* (`/blast-radius` ➔ `/benchmark-checklist`)
- **`/blast-radius` (*find what change could break*, `frame_0030`):** Quét toàn bộ đồ thị phụ thuộc để tìm các module có nguy cơ bị ảnh hưởng dây chuyền bởi thay đổi hiện tại.
- **`/benchmark-checklist` (*sanity-check speed numbers*, `frame_0030`):** Kiểm tra chéo các con số đo đạc hiệu năng để tránh báo cáo ảo.

#### Luồng 3: *Attack the diff* (`/interrogate` ➔ `/thermo-nuclear-code-quality-review`)
- **`/thermo-nuclear-code-quality-review` (*extra-harsh maintainability audit*, `frame_0040`–`frame_0043`):** Khi một file phình to vượt quá **1,000 dòng (`1000+ lines`)** hoặc các khối điều kiện chồng chéo như mì Ý (`frame_0042`), kỹ năng này kích hoạt chế độ kiểm toán khắc nghiệt nhất: yêu cầu tái cấu trúc toàn diện, gọt bỏ mọi lớp trừu tượng thừa thay vì chỉ góp ý bề mặt.

---

### Giai Đoạn 5 & 6: Deploy (Đưa PR Tới Đích Merge) & Maintain (`09:10 – 10:00`, `frame_0044`–`frame_0046`)
- **Chuỗi 5 kỹ năng *Get the PR merged* (`frame_0045`):**
  1. `/make-pr-easy-to-review`: Sắp xếp lại mô tả và cấu trúc PR để đồng nghiệp duyệt nhanh nhất.
  2. `/babysit`: Canh gác PR liên tục cho tới khi được merge hoàn tất.
  3. `/fix-merge-conflicts`: Tự động gỡ xung đột code với nhánh đích và chạy lại lệnh build kiểm tra.
  4. `/get-pr-comments`: Gom toàn bộ bình luận review trên GitHub thành danh sách đầu việc hành động (action items).
  5. `/fix-ci`: Tự đọc log CI bị đỏ và đẩy commit sửa lỗi.
- **Lưu ý trên bản đồ (`frame_0044`–`frame_0045`):** `pstack` dừng lại ở bước merge PR (không có skill cho hạ tầng deploy, rollout, rollback hay giám sát sau phát hành), và ở cột **Stage 6: Maintain** chỉ có các skill bảo trì tài liệu/kiểm thử nội bộ nên Eric bổ sung bằng công cụ `Super Collect` của mình.

*(Ghi chú phân đoạn tài trợ `10:00 – 11:51`, `frame_0047`–`frame_0057`: Eric giới thiệu **Blotato** tại `blotato.com` — nền tảng Social Media API + MCP Server cho phép AI Agents trong ChatGPT và Claude tự động cắt video dài như video `github/reverse-engineer` 70k stars và `FDE Roadmap Google $500k Offer` thành Reels, lên lịch đăng lên 9+ mạng xã hội X, LinkedIn, Facebook, Instagram, TikTok, YouTube, Threads, Bluesky, Pinterest và tự động gửi DM thu thập email khi người xem bình luận từ khóa `REAVIDEO`.)*

---

## 3. Nhóm Kỹ Năng Vận Hành Bất Kỳ Lúc Nào (`Anytime`) & Triết Lý `principle-*` (`11:52 – 14:56`, `frame_0058`–`frame_0074`)

### 3.1. Các Kỹ Năng Điều Phối & Phục Hồi Ngữ Cảnh (`frame_0058`–`frame_0060`)
- **Router:** `/poteto-mode` (*pick and run right playbook*) — Bộ định tuyến tự động chọn đúng chuỗi skill trong `pstack` cho bài toán hiện tại.
- **Setup & Help:** `/setup-pstack` (*choose models for each job*), `/poteto-help` (*answer pstack how-to questions*).
- **Catch up & Plain talk (`frame_0060`):**
  - `/recall` (*rebuild what you were doing*): Khôi phục lại mạch công việc khi chuyển đổi giữa nhiều nhánh/dự án.
  - `/ketchup` (*recap chat since last message*): Tóm tắt nhanh những gì đã diễn ra trong phiên chat.
  - `/bro` (*repeat last reply simply*): Yêu cầu AI giải thích lại câu trả lời vừa rồi bằng ngôn ngữ đời thường, ngắn gọn.
  - `/what-did-i-get-done` (*summarize recent commits*): Tóm tắt các commit vừa hoàn thành để báo cáo tiến độ.

### 3.2. Kiến Trúc `skills-as-index`: Tại Sao Phải Tách `principle-*` Khỏi `system-prompt.md`? (`frame_0061`–`frame_0064`)
- Nếu nhồi toàn bộ hàng chục nguyên tắc thiết kế vào một file `system-prompt.md` (`CLAUDE.md` / `AGENTS.md`) khổng lồ, thanh tiêu thụ `tokens` sẽ luôn bị chiếm dụng ở mức cao trong mọi lượt chat (`frame_0061`) và làm loãng sự chú ý của mô hình.
- Giải pháp của Lauren Tan (`frame_0062`–`frame_0063`): Giữ file chỉ mục cực nhẹ (`skills-as-index`), tách từng quy tắc thành một skill độc lập có tiền tố `principle-*` được chia thành 4 cụm trên bản đồ (`frame_0063`, `frame_0068`, `frame_0072`):
  - **Principles: Keep it small:** `laziness-protocol` (*delete first, change least*), `subtract-before-you-add` (*remove dead weight first*), `minimize-reader-load` (*cut what readers must trace*), `outcome-oriented-execution`, `migrate-callers-then-delete-legacy-apis`.
  - **Principles: Design the shape:** `foundational-thinking` (*get data shapes right first*), `redesign-from-first-principles`, `attack-the-premise`, `experience-first`, `exhaust-the-design-space` (*compare 2-3 real alternatives*).
  - **Principles: Code structure & safety:** `model-the-domain` (*put rules in data structures*), `boundary-discipline` (*validate at edges only*), `type-system-discipline` (*types forbid impossible states*), `make-operations-idempotent`, `separate-before-serializing-shared-state`.
  - **Principles: Prove it:** `prove-it-works` (*test the real thing*), `fix-root-causes` (*fix cause, not symptom*), `sequence-verifiable-units`, `test-behavior-not-implementation`, `explain-the-number`.

### 3.3. Hai Nguyên Tắc Đỉnh Cao Được Eric Đưa Vào Super Board (`frame_0065`–`frame_0073`)
1. **`attack-the-premise` (*Question the shared assumption*, `frame_0065`–`frame_0067`):**
   - **Khi nào kích hoạt (`WHEN TO USE IT`):** *"Two fixes that share one assumption have both failed."* (Khi 2 lần thử sửa lỗi liên tiếp cùng dựa trên một giả định đều thất bại: `fix` ➔ `still broken` ➔ `fix` ➔ `still broken` trong `frame_0066`).
   - **Hành động (`WHAT IT IS`):** *"Stop fixing. Question the shared assumption."* — Buộc agent ngừng ngay việc viết bản vá thứ 3 và phải đặt câu hỏi xét lại giả định gốc rễ.
2. **`type-system-discipline` (*Make the compiler reject impossible states*, `frame_0069`–`frame_0071`):**
   - **Khi nào kích hoạt:** Khi thiết kế kiểu dữ liệu (`types`) hoặc chữ ký hàm (`signature`) trong ngôn ngữ có kiểu tĩnh.
   - **Hành động:** Thiết kế Discriminated Unions sao cho trình biên dịch báo lỗi ngay lập tức nếu xuất hiện trạng thái vô lý (ví dụ: một đơn hàng không thể vừa ở trạng thái `paid` vừa ở trạng thái `pending`).

---

## 4. Bài Học Hành Động (Actionable Takeaways)

1. **Áp dụng ngay bộ đôi `/deslop` + `/no-comments` trước khi mở PR:** Quét toàn bộ diff so với `main` để xóa các khối `try/catch` thừa, kiểu `any` và đặc biệt là các dòng comment cũ có nguy cơ đánhừa LLM ở các phiên làm việc tiếp theo.
2. **Chuyển từ `system-prompt.md` nguyên khối sang `skills-as-index` (`principle-*`):** Chỉ giữ danh mục chỉ dẫn ngắn gọn trong `CLAUDE.md` và nạp chi tiết từng nguyên tắc (`attack-the-premise`, `type-system-discipline`, `boundary-discipline`) khi tác vụ thực sự chạm tới.
3. **Đầu tư xây dựng `verify-<app>/SKILL.md` cho dự án:** Để agent tự đọc repo và đóng gói quy trình `Launch -> Doctor -> Drive -> Evidence -> Cleanup`, giúp mọi PR về sau đều được kiểm chứng tự động trên ứng dụng thật.
