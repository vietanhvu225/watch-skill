# 🚦 Fixing the PR Bottleneck: Kiểm Soát "Khẩu Đại Bác Slop" (Slop Cannon) Với 3 Tầng Phanh, Deep Modules & Compounding Retro — Matt Pocock (AIHero)

- **Source URL:** https://www.youtube.com/watch?v=LlgiOCmFG_w
- **Channel:** AI Engineer (Diễn giả: Matt Pocock — AIHero.dev, tại AI Engineer Paris organized by Mistral)
- **Duration:** 22:34
- **Watch Skill ID:** `0d17794d0232f55e`
- **Date Processed:** 2026-11-07
- **Tags:** #matt-pocock #aihero #pr-bottleneck #code-review #deep-modules #software-factory #tautological-tests #subagents #retro-skill

---

## Executive Summary: Nút Thắt Cổ Chai Mới Của Kỹ Nghệ Phần Mềm

Tại hội nghị **AI Engineer Paris** (do Mistral tổ chức — `frame_0001`, `frame_0005`), **Matt Pocock** (`aihero.dev/skills`, phát hành phiên bản `v1.3`) giải phẫu nghịch lý lớn nhất của kỷ nguyên **Software Factory**: khi các tác nhân AI và trình phân loại tự động (như **Jev** phân loại support tickets hay **PlanetScale** gửi báo cáo slow queries) tự động kích hoạt luồng sinh code không cần con người bấm nút (`Accelerators`), nếu thiếu hệ thống phanh hãm (`Brakes`), toàn bộ nhà máy phần mềm sẽ biến thành một **"Khẩu đại bác bắn Slop" (`Slop Cannon`)**.

```
+-----------------------------------------------------------------------------------------+
|                              THE SLOP CANNON DILEMMA                                    |
|                                                                                         |
|  [ Accelerators ]  ==== (Bug reports, Support tickets, Slow queries) ====>              |
|  [ AI Agents ]     ==== (Permanent Acceleration) ====> [ SLOP CANNON ] ===> [ PR Queue ]|
|                                                                                         |
|  -> HỆ QUẢ: Codebase là môi trường sống của Agent ("Code is the environment your agent  |
|     operates in"). Code tồi sinh ra nhiều code tồi hơn (Software Entropy Nightmare).   |
|  -> GIẢI PHÁP: Hệ thống 3 Tầng Phanh (Automated Checks -> Automated Review -> Human)    |
|     với nguyên lý cốt lõi: "Stop The Slop: Higher code quality = faster human review". |
+-----------------------------------------------------------------------------------------+
```

Bài thuyết trình cung cấp kiến trúc thực chiến gồm **3 Tầng Phanh (3 Brakes)**, vạch trần **3 kiểu "nói dối" của Automated Checks**, áp dụng **Deep Modules** (John Ousterhout) kèm kỹ năng kiểm toán HTML (`course-video-manager`), tách rời **Overloaded Implementer** và **Underloaded Reviewer Sub-Agent (`CODING_STANDARDS.md`)** theo quy tắc **"Commits, Not Comments"**, chuẩn hóa PR với **Merge Danger (`One-Way vs Two-Way Door`) + Pseudocode/Show-Me Diagrams**, và khép kín vòng lặp tiến hóa bằng kỹ năng **`retro` (Retrospective)**.

---

## 1. Accelerators vs. Brakes & Nguyên Lý "Stop The Slop" (`[00:00]` – `[04:09]`)

- **Accelerators — Đầu vào tự động của Software Factory (`frame_0005` – `frame_0007`):**
  - Slide `Accelerators`: *"Inputs to the software factory - bug reports, support tickets, slow queries"*.
  - Thay vì con người khởi tạo mọi tác vụ, các hệ thống định sẵn (deterministic code) tự động kích hoạt: ví dụ bộ phân loại **Jev** đọc support ticket và chuyển thành bản vá/reproduction, hoặc **PlanetScale** báo cáo các truy vấn cơ sở dữ liệu chậm (`slow queries`) và tự đẩy vào pipeline của nhà máy phần mềm.
- **Ba Tầng Phanh (`3 Brakes` — `frame_0001`, `frame_0015`):**
  1. **Brake 1 — Automated Checks:** Các kiểm tra tất định (deterministic checks) đã tồn tại từ thập niên 1950: Linting, Tests, Type Checking, Code Quality Metrics. Chỉ tốn **CPU cycles**, không tốn token hay sức người.
  2. **Brake 2 — Automated Review:** Tác nhân AI kiểm tra những gì test bỏ sót và cấu trúc tổng thể của codebase.
  3. **Brake 3 — Human Review:** Con người duyệt PR ở tầng cuối cùng.
- **Nguyên lý đòn bẩy (`frame_0015` — `Stop The Slop`):**
  - Khi chất lượng code đầu ra từ 2 tầng đầu được nâng cao, con người phải can thiệp ít hơn hẳn, giúp tốc độ duyệt PR tổng thể tăng vọt (*counterintuitively go faster*).

---

## 2. Tại Sao Automated Checks Lại "Nói Dối"? — Hệ Thống Phát Hiện Nói Dối (`Lie Detectors`, `[04:10]` – `[08:25]`)

- **Visual (`frame_0019` – `frame_0020`):** Slide **`Automated Checks: Lie Detectors`** — *"Does a green CI mean that the code is ready for merge? No. Automated/human review is for finding lies in the automated checks."*
- Matt Pocock trình chiếu **3 bằng chứng code thực tế** từ các tác nhân AI (đặc biệt là `Opus 5`) viết các bài test vô giá trị làm xanh CI giả tạo:

```
+-----------------------------------------------------------------------------------------+
|                    3 KIỂU "NÓI DỐI" CỦA AUTOMATED CHECKS DO AI SINH RA                  |
+-----------------------------------------------------------------------------------------+
| 1. Tautological Tests (Test lặp thừa định nghĩa — [05:24], frame_0021):                 |
|    - Code thực thi: `X_POST_CHARACTER_LIMIT = 280`                                      |
|    - Test của Opus 5: `expect(X_POST_CHARACTER_LIMIT).toBe(280)`                        |
|    -> Chỉ khẳng định lại đúng hằng số vừa khai báo, không kiểm tra hành vi thực.        |
|                                                                                         |
| 2. Structure-Sensitive Tests (Test nhạy cảm cấu trúc — [06:04], frame_0022–0026):       |
|    - Định nghĩa (frame_0022): "A test that knows too much about the internal workings   |
|      of the system".                                                                    |
|    - Ví dụ thực tế cực đoan (frame_0024–0025): Để kiểm tra mục `Videos` hiển thị sau    |
|      mục `Content Plan` trên trang `_app.pitches.$pitchId.tsx`, thay vì render UI,      |
|      Agent dùng `fs.readFileSync(PITCH_DETAIL_PATH, "utf-8")` đọc thẳng mã nguồn        |
|      vào bộ nhớ rồi so sánh vị trí chuỗi:                                               |
|      `const contentPlanIndex = content.indexOf(">Content Plan<");`                      |
|      `const videosIndex = content.indexOf('title="Videos"');`                           |
|      `expect(videosIndex).toBeGreaterThan(contentPlanIndex);`                           |
|    -> Chỉ cần đổi cách trình bày mã nguồn là test vỡ ngay lập tức!                      |
|                                                                                         |
| 3. Tests That Literally Cannot Fail (Test không thể đỏ do lạm dụng Mock — [07:15]):     |
|    - Hàm `useAudioBoost()` gọi `AudioContext` API của DOM vốn có nhiều chế độ lỗi       |
|      phức tạp trong thực tế, nhưng Agent stub/mock toàn bộ bằng các hàm giả.            |
|    -> Test luôn xanh nhưng ứng dụng crash ngay trên production.                         |
+-----------------------------------------------------------------------------------------+
```

---

## 3. Codebase Design: Chống Gian Lận Test Bằng "Deep Modules" (`[08:25]` – `[11:12]`)

Làm sao để thiết kế codebase khiến AI không thể viết các bài test nhạy cảm cấu trúc (`Structure-Sensitive Tests`)? Câu trả lời của Matt Pocock là **Deep Modules** (từ cuốn *A Philosophy of Software Design* của GS. John Ousterhout):

- **So sánh Module A vs Module B (`frame_0030` – `frame_0032`):**
  - **Module A (Deep Module — Hình chữ nhật hẹp và sâu bên trái ở `frame_0031`):** Giao diện (interface) ở đỉnh cực nhỏ nhưng ẩn giấu phần cài đặt (implementation) lớn và phức tạp bên dưới. Khi ép Agent chỉ được kiểm thử qua giao diện nhỏ ở đỉnh, Agent buộc phải viết test hành vi thực chất thay vì chọc sâu vào chi tiết cài đặt.
  - **Module B (Shallow Module — Hình chữ nhật rộng và nông bên phải ở `frame_0031`):** Giao diện phơi bày hàng tá hàm nhỏ lẻ, mỗi hàm làm rất ít việc, tạo điều kiện cho AI viết các bài test phụ thuộc chặt vào cấu trúc nội bộ.
- **Skill Kiểm Toán Deep Modules & Từ Vựng Thiết Kế Chung (`frame_0033` – `frame_0036`):**
  - Khi chạy skill thiết kế codebase trên một repo thực tế (`course-video-manager` trong `frame_0033`), skill xuất ra báo cáo HTML trực quan so sánh sơ đồ **Before vs After**: *"Give Course View Display Settings a predicate module instead of three inline copies"*.
  - **Phân tích cụ thể trên slide (`frame_0035` – `frame_0036`):**
    - **Problem:** Biểu thức kiểm tra hiển thị phức hợp (`compound visibility expression`) bị viết tay lặp lại ở 3 nơi gọi khác nhau mà không có tên chung và không có unit test nào (`section-card.tsx` hoàn toàn không có file test).
    - **Solution:** Gom từng quy tắc có tên (*"can show the dependency spine"*, *"can show the lesson-type icon"*) vào module thuần `visibility-predicates.ts` xử lý trên `ResolvedVisibility`, để cả 3 component (`section-card.tsx`, `lesson-item.tsx`, v.v.) cùng gọi.
    - **3 Thuộc tính ngôn ngữ thiết kế chuẩn hóa cho cả Team & Agent:**
      1. **Locality (Tính cục bộ):** Quy tắc kiểm soát nằm gọn trong một module duy nhất, có thể unit test độc lập mà không cần mount cả 3 UI components.
      2. **Leverage (Đòn bẩy):** Một giao diện đơn giản phục vụ ba nơi gọi (*"one interface, three call sites"*).
      3. **Seam & Deletion Test:** Xóa module đó sẽ làm lộ rõ 3 components mất nguồn chân lý duy nhất (*source of truth*).

---

## 4. Automated Review: Tách Rời Implementer (Overloaded) & Reviewer (Underloaded) (`[11:13]` – `[16:28]`)

Làm thế nào để bắt buộc Agent tuân thủ các tiêu chuẩn **Deep Modules** và viết test chuẩn?

### 4.1. Sai lầm kinh điển: Nhồi `CODING_STANDARDS` vào Implementer Agent (`frame_0037` – `frame_0039`)
- Trong một cửa sổ ngữ cảnh (context window) duy nhất, **Implementer Agent** đã phải gánh 3 phần việc nặng nề (`frame_0039`):
  1. **Exploration** (tìm kiếm file và vị trí cần sửa).
  2. **Implementation** (viết và cập nhật code).
  3. **Debugging** (chạy automated checks và sửa lỗi).
- **Mental Model cốt lõi:** **Implementer Agent luôn bị quá tải (`Implementation is overloaded`)**. Nếu nhồi thêm hàng trăm dòng quy chuẩn code vào `AGENTS.md` toàn cục, nó sẽ bị loãng sự chú ý và làm việc kém đi.

### 4.2. Giải pháp Red-Green-Refactor với 2 Context Windows: `code-review` Sub-Agent (`frame_0040` – `frame_0043`)
- Tách quy trình viết code tốt thành **2 pha trên 2 cửa sổ ngữ cảnh độc lập** (tương tự **Red-Green-Refactor** trong TDD):
  - **Pha 1 — Implementer Agent (`Red-Green`):** Chỉ tập trung làm cho code chạy được (*"make it work"*), không bị áp đặt `CODING_STANDARDS.md`.
  - **Pha 2 — Code Review Sub-Agent (`Refactor` — `frame_0041` – `frame_0043`):** Chạy trong một **Sub-agent có context window riêng**. Vì nó nhận sẵn bản `git diff`, nó chỉ cần làm một chút `Exploration` để hiểu ngữ cảnh xung quanh và hoàn toàn không phải làm `Implementation` hay `Debugging` từ đầu. Do **Reviewer đang rảnh tải (`Review is underloaded`)**, ta có thể nạp toàn bộ file **`CODING_STANDARDS.md`** của dự án vào cho nó kiểm tra cực kỳ sắc bén.
- **Đừng thuê ngoài (Don't Outsource Automated Review — `[14:32]` – `[15:20]`):**
  - Các dịch vụ bên thứ ba như Cursor BugBot hay CodeRabbit thường quá chung chung (gây nhiều cảnh báo giả — false positives) hoặc không khớp với đặc thù ngôn ngữ/kiến trúc riêng của đội ngũ. Hãy tự xây dựng file `CODING_STANDARDS.md` nội bộ và chia sẻ trong team.

### 4.3. Nguyên Tắc Vàng: "Commits, Not Comments" (`[15:22]` – `[16:19]`, `frame_0046` – `frame_0048`)
- **Visual (`frame_0047`):** Slide **`Automated Review: Commits, Not Comments`** — *"Automated review should NOT burden humans with comments to review and fix."*
- Nếu Agent kiểm duyệt chỉ để lại hàng loạt bình luận dài dòng trên PR, nó đang **đẩy thêm việc cho con người** đọc và quyết định.
- **Quy tắc bắt buộc:** Mặc định của Reviewer Agent là **trực tiếp sửa code và tạo Commit mới lên nhánh PR**. Chỉ khi thực sự có thắc mắc không thể tự quyết định mới để lại comment. Hãy ngừng cố gắng "one-shot" code hoàn hảo ngay ở bước Implementer!

---

## 5. Human Review: Tối Ưu Hóa PR Cho Con Người & Vòng Lặp Lãi Kép `retro` (`[16:29]` – `[22:34]`)

### 5.1. Kỹ năng `pr` mới: Phân loại `One-Way vs Two-Way Door` & Thẻ `Merge Danger` (`frame_0051` – `frame_0052`)
- **Visual (`frame_0051`):** Slide **`Human Review: One-Way or Two-Way Door?`** — *"How easily can we revert this change?"*
  - **Two-Way Door (Cửa 2 chiều — đa số PR phần mềm):** Có thể hoàn tác dễ dàng. Khác với kỹ sư cầu đường (xây cầu sập là cửa 1 chiều), kỹ sư phần mềm có thể `git revert` hầu hết thay đổi. Bạn **không cần** soi từng dòng ở mọi cửa 2 chiều.
  - **One-Way Door (Cửa 1 chiều):** Thay đổi vô tình bắn email cho 60,000 khách hàng, chạy database migration đắt đỏ hoặc gây mất dữ liệu (`data loss`). Những PR này bắt buộc con người phải kiểm tra cực kỳ kỹ lưỡng.
- **Thẻ `Merge Danger` ở cuối mỗi PR (`frame_0052`):**
  - Hình chụp thực tế từ PR của Matt Pocock:
    > **Merge Danger**
    > **Door:** Two-way — trivial to revert; changes only how an error message is rendered, no behavior change to the turn loop itself.
    > **Blast Radius:** Localized — only affects the text of error messages surfaced from stream `"error"` parts (auth/bridge/compact failures) across all commands, not just `/compact`. No API, schema, or control-flow changes.

### 5.2. Hiểu "Why" Nhanh Nhất Bằng Pseudocode & Sơ Đồ Trực Quan (`show-me` Skill — `frame_0053` – `frame_0055`)
- Lấy cảm hứng từ kỹ năng **`show-me`** trong repo `humanlayer` của **Dex Horthy**, phần mô tả PR loại bỏ các bức tường chữ (wall of text) và thay bằng:
  1. **Sequence / Mermaid Diagrams (`frame_0054`):** Minh họa luồng tương tác giữa `Author` → `lesson-item-planner` (`section flips empty -> real, renumbers every real section's path`, `updateSectionTitle(id, "02-my-section")`) → `section-title` (`tries to save the clicked, outdated path — the exact "01-domain-language" bug`).
  2. **Tóm tắt CLI / Pseudocode ngắn gọn (`frame_0055`):**
     ```bash
     ip move ...
     ip delete <id>      # soft delete
     ip restore <id>     # NEW: undelete
     ip words <id>
     ```
     Giúp người duyệt nắm bắt ngay lệnh mới và cờ (flags) chỉ trong vài giây.

### 5.3. Nguyên Lý Thứ 3: Duyệt Quy Trình Tạo Code Qua Skill `retro` (`[19:43]` – `[22:34]`, `frame_0058` – `frame_0060`)
- **Tư duy hệ thống:** *"When you do a human review, you're not just reviewing the code, you're reviewing the system that creates it. You never want to write the same comment twice."*
- **Cơ chế hoạt động của kỹ năng `retro` (Retrospective):**
  - Sau một phiên làm việc của Agent, một PR, hoặc toàn bộ các PR và nhận xét review trong tuần qua, chạy skill `retro` để tạo **hiệu ứng lãi kép (compounding effect — `frame_0058`)** thông qua 5 tác động tự động:
    1. **Đề xuất Automated Checks mới** để bắt lỗi bằng CPU ở lần sau.
    2. **Đề xuất cập nhật vào `CODING_STANDARDS.md`** cho Reviewer Sub-Agent.
    3. **Bổ sung `Navigation Pointers` vào `AGENTS.md`:** Phân tích xem Agent tìm kiếm thông tin trong repo có vất vả không và thêm chỉ dẫn đường đi để lần sau tìm thấy ngay.
    4. **Kiểm toán `Tool Economy`:** Phát hiện các lệnh gọi công cụ tốn nhiều token không cần thiết trong phiên làm việc để tối ưu hóa hiệu suất token.
    5. **Cắt tỉa `Bloat` (`frame_0060`):** Rà soát các file điều hướng (`steering files`) hoặc `skills` bị phình to làm giảm chất lượng đầu ra để tổ chức lại gọn gàng hơn.

---

## 6. Tổng Kết Chiến Lược Giải Quyết PR Bottleneck

| Tầng kiểm soát | Tài nguyên tiêu thụ | Công cụ & Kỹ năng chủ lực (`aihero.dev/skills v1.3`) | Nguyên tắc cốt lõi |
| :--- | :--- | :--- | :--- |
| **1. Automated Checks** | CPU Cycles (Rẻ nhất) | Linting, Typecheck, Unit Tests + **Deep Modules (`codebase-design`)** | Thiết kế giao diện hẹp & sâu (`Locality` + `Leverage`) để chống test giả (`Tautological` / `Structure-Sensitive`). |
| **2. Automated Review** | Tokens (Sub-agent riêng) | **`code-review` skill** + **`CODING_STANDARDS.md`** | Không nhồi chuẩn code vào Implementer; tách sang Sub-agent rảnh tải và tuân thủ **"Commits, Not Comments"**. |
| **3. Human Review** | Sự chú ý của con người (Đắt nhất) | **`pr` skill** (`Merge Danger`, `show-me` diagrams) + **`retro` skill** | Chỉ soi kỹ **One-Way Doors**, lướt nhanh **Two-Way Doors**, và dùng `retro` để không bao giờ viết cùng một nhận xét 2 lần. |
