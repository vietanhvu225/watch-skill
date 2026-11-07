# Anthropic's Engineers Stopped Building AI Agents. Here's What They Build Instead. (Sharbel)

## 📋 Metadata
- **Title:** Anthropic's Engineers Stopped Building AI Agents. Here's What They Build Instead.
- **Speaker / Channel:** Sharbel A. (@sharbelxyz)
- **URL:** https://www.youtube.com/watch?v=VxGIopUQVKo
- **Watch Skill ID:** `ca2cb4a0e7693e9f`
- **Duration:** 16:09
- **Frames Extracted & Read:** 64 frames (`C:\Users\Admin\.watch-skill\frames\ca2cb4a0e7693e9f\frame_0000.jpg` – `frame_0063.jpg`)
- **Date Processed:** 2026-11-07
- **Category:** AI Agents / Claude Skills / Progressive Disclosure / MCP & Skill Architecture / Open Standard

---

## 🎯 Tóm tắt cốt lõi (Executive Summary)

Video mở đầu bằng trích đoạn bài thuyết trình của hai kỹ sư Anthropic (**Barry Zhang & Mahesh Murag**) lý giải vì sao đội ngũ kỹ sư tại Anthropic đã ngừng việc xây dựng một AI Agent mới cho từng tác vụ riêng lẻ, và chuyển hoàn toàn sang **Agent Skills**. Thay vì phải quản lý "một vườn thú" gồm hàng chục agent nửa vời (agent viết nội dung, agent trả lời email, agent nghiên cứu...), bạn chỉ cần **một General Agent duy nhất** và trang bị cho nó **một hộp công cụ (Toolbox) gồm các file `SKILL.md`**.

Tác giả Sharbel giải thích cơ chế **Progressive Disclosure** (tiết lộ lũy tiến giúp giữ sạch Context Window), trực tiếp xây dựng và chạy thử một Skill nghiên cứu YouTube ngách (`youtube-pockets`) kết hợp với **vidIQ MCP** theo **3 phương pháp** (Tự viết tay `SKILL.md`, Dùng `skill-creator` hỏi đáp tương tác, và Quay màn hình trực tiếp bằng tính năng `Record a skill` trên Claude Desktop), đồng thời chia sẻ các kho Skills chuẩn mở (dùng chung cho Claude, Codex, Cursor, Hermes) cùng cảnh báo bảo mật Supply Chain khi cài đặt Skill từ bên thứ ba.

---

## 📱 1. Ẩn dụ Chiếc Điện Thoại & Tư duy "1 Agent Beats 10" (`[00:00]` – `[01:50]`)

> **Visual (`frame_0001` – `frame_0007`, `frame_0019`, `frame_0026`):**
> - `frame_0001`: Cảnh hội thảo của **Barry Zhang & Mahesh Murag, Anthropic** giải thích lý do ngừng xây dựng từng agent riêng rẽ.
> - `frame_0003` – `frame_0006`: Sơ đồ giải phẫu chiếc iPhone (**`THE WHOLE IDEA, IN ONE OBJECT`**):
>   - **`THE PROCESSOR`** = **AI Model** (bộ não xử lý thô, ví dụ `Opus 5.5 Medium Auto`).
>   - **`THE OPERATING SYSTEM`** = **AI Agent** đang vận hành model (Claude, Hermes, Cursor, Codex).
>   - **`THE APPS`** = **Skills** (các ứng dụng cài thêm vào máy).
> - `frame_0007`: Đồ họa so sánh **`TWO WAYS TO GET A NEW FEATURE`**:
>   - Cách cũ (gạch bỏ): ~~`A WHOLE NEW PHONE (everything again, from scratch)`~~ — mỗi lần muốn tính năng mới lại đập đi xây lại một chiếc điện thoại mới từ đầu.
>   - Cách mới: Cài đặt thêm các ô App vào cùng một chiếc điện thoại (`THINGS IT CAN NOW DO: 12`).
> - `frame_0019` & `frame_0026`: Tiêu đề chương **`1 ONE AGENT BEATS TEN`** và sơ đồ **`ONE FILE` / `1 FOLDER · 1 FILE`**.

- **Vấn đề của việc tạo nhiều Agent:** Nhiều người tự hào khoe có 5–10 agent khác nhau (một cho content, một cho email, một cho outreach...), nhưng thực chất họ đang phải làm "bảo mẫu" cho một bầy nhân viên học việc nửa vời, không chia sẻ ngữ cảnh và tốn công bảo trì lặp lại.
- **Giải pháp của Anthropic:** Thay vì tuyển 10 nhân viên mới cho 10 đầu việc, hãy giữ **1 nhân viên giỏi nhất** và đưa cho họ **một hộp công cụ (toolbox)** ngày càng đầy đặn hơn.

---

## 📂 2. Giải phẫu `SKILL.md` & Cơ chế Progressive Disclosure (`[01:51]` – `[02:51]`)

Một **Skill** về bản chất chỉ là **1 thư mục chứa 1 file văn bản thuần (`SKILL.md`)** (có thể kèm thêm các thư mục con chứa scripts, templates hoặc tài liệu tham khảo).

### Cấu trúc 2 phần của `SKILL.md`:
1. **YAML Frontmatter (2 dòng đầu tiên — `name` và `description`):**
   - **Điểm mấu chốt:** Phần `description` **không phải viết cho con người đọc**, mà viết **cho chính AI Agent đọc** để nó tự quyết định *khi nào* cần kích hoạt Skill đó.
   - Ví dụ: Nếu chỉ ghi *"Helps with YouTube"* thì quá mơ hồ; phải ghi rõ điều kiện kích hoạt như *"Finds viral YouTube pockets and title formats. Use when the user asks to research YouTube video ideas or says use youtube-pockets."*
2. **Body (Hướng dẫn bằng ngôn ngữ tự nhiên):**
   - Ghi lại quy trình từng bước (step-by-step SOP) giống hệt cách bạn viết tài liệu bàn giao công việc cho một nhân sự mới.

### Cơ chế Progressive Disclosure (Tiết lộ lũy tiến):
- Nhiều người lo ngại: *"Nếu tôi cài 50 hay 100 Skills, chẳng phải toàn bộ hướng dẫn sẽ làm tràn Context Window và khiến Agent bị ngáo sao?"*
- **Cách hoạt động thực tế:** Hộp công cụ luôn **đóng nắp** theo mặc định. Ở mỗi lượt hội thoại, Agent **chỉ đọc danh sách `name` + `description` ngắn gọn** của các Skills. Chỉ khi yêu cầu của người dùng khớp với `description` của một Skill cụ thể, Agent mới "mở nắp" đọc toàn bộ nội dung file `SKILL.md` (và các script liên quan) của đúng Skill đó vào bộ nhớ làm việc.

---

## 🛠️ 3. Case Study Thực Chiến: 3 Cách Xây Dựng Skill `youtube-pockets` (`[02:53]` – `[11:53]`)

Sharbel minh họa bằng quy trình thực tế anh dùng hàng tuần để tìm ý tưởng video YouTube dễ nổ view (**"YouTube Pockets"** — những chủ đề ngách có nhiều kênh nhỏ đạt lượng view vượt trội so với số sub).

### Quy trình thủ công ban đầu trên YouTube & vidIQ (`[03:10]` – `[05:53]`):
> **Visual (`frame_0030` – `frame_0038`):**
> - `frame_0030`: Tìm kiếm từ khóa `AI` trên YouTube với thanh công cụ vidIQ (`Keyword Score: 73 Overall Score HIGH`, `Search Volume: HIGH`, `Competition: LOW`).
> - `frame_0033`: Bảng điều khiển vidIQ (`35,800 Subscribers | 2,566,433 Views`), gợi ý video đột phá của Greg Isenberg (`"Jev is HERE. How to use it"`) và bảng từ khóa thịnh hành (`#1 claude skills`, `#2 opus5 +1,383%`).
> - `frame_0036`: Bộ lọc tìm kiếm Outlier trong tab Research của vidIQ: `Outlier Score >= 2x`, `Views >= 50k`, `Publishing Date: This Month` cho từ khóa `AI`.

Thay vì lặp lại 30 phút lọc thủ công mỗi tuần, quy trình này được đóng gói thành Skill kết hợp với **vidIQ MCP connector** qua **3 phương pháp** (`frame_0009` – `frame_0011`):

---

### Cách 1: Tự viết tay (`Built By Hand` — `[05:54]` – `[07:00]`)
> **Visual (`frame_0010`, `frame_0041` – `frame_0043`):**
> - Trên giao diện Claude Desktop (`Happy Friday, Sharbel`, model `Opus 5.5 Medium Auto`), nhấn nút `+` → `Skills` → `Browse skills` → `+ Add` → **`Create a skill`**.
> - Điền form:
>   - **Skill name:** `youtube-pockets` (`Saved as /youtube-pockets`)
>   - **Description:** `Finds viral youtube pockets. Use when I say use youtube-pockets`
>   - **Instructions:** Viết quy trình tìm các video có `Outlier >= 2x`, `Views >= 50,000` trong 30 ngày qua và gom nhóm thành các "pockets" kèm công thức đặt tiêu đề.
> - `frame_0043`: Giao diện quản lý phiên bản **`Youtube pockets v1 · current`** hiển thị cấu trúc file `/SKILL.md` và nút tải xuống `.zip`.

---

### Cách 2: Phỏng vấn tương tác với `skill-creator` (`Created With Claude` — `[07:01]` – `[10:23]`)
> **Visual (`frame_0045` – `frame_0048`):**
> - Chọn `+ Add` → **`Create with Claude`**, hệ thống tự động điền prompt: *"Let's create a skill together using your skill-creator skill. First ask me what the skill should do."*
> - `frame_0045`: Claude chia buổi phỏng vấn thành 4 trụ cột (**`The job`**, **`The trigger`**, **`Inputs and output`**, **`Your way of doing it`**) và đặt 12 câu hỏi làm rõ quy trình.
> - `frame_0046`: Sau khi người dùng trả lời, Claude hỏi tiếp 3 câu chốt hạ cực kỳ sắc bén:
>   1. Giới hạn ngân sách gọi API: **Tối đa 5 lượt tìm kiếm (~25 credits vidIQ)** mỗi lần chạy để tránh đốt sạch quota.
>   2. Định dạng đầu ra: Hiển thị các tiêu đề thực tế làm bằng chứng, kèm **3 tiêu đề gợi ý mới** bên dưới mỗi cụm.
>   3. Tiêu chí xếp hạng: Xếp hạng các "pockets" theo **tiềm năng viral** (số lượng kênh khác nhau cùng thắng lớn ở ngách đó + độ cao của điểm Outlier + đà tăng trưởng tìm kiếm).

#### Kết quả chạy thử `/youtube-pockets` trực tiếp với vidIQ MCP (`frame_0047` – `frame_0048`):
Báo cáo trả về **22 video đạt chuẩn từ 1 lượt tìm kiếm (`AI` · trong tháng này · `2x+ outlier` · `50k+ views` · video dài · tiếng Anh)**, được phân tách thành **4 Pockets xếp hạng từ cao xuống thấp**:
1. **`Pocket 1: AI danger explained calmly (highest potential)`**
   - Thắng lớn trên **5 kênh khác nhau**, nhu cầu tìm kiếm đang tăng vọt (`"ai warning" +346%`, `"ai safety" +98%`, `"ai risk" +85%`).
   - Dẫn chứng thực tế: CBC News (`39x outlier · 1.79M views`), TEDxBerlin (`62x outlier`), Computerphile (`17x outlier`).
   - Gợi ý 3 tiêu đề mới (ví dụ: *"A Reasonable Person's Guide To How AI Takes Your Job"*).
2. **`Pocket 2: Contrarian AI opportunity takes`** (thắng trên 4 kênh, có video đạt `241x outlier` và `1.4M views`).
3. **`Pocket 3: Five insane AI robots in 2026`**.
4. **`Pocket 4: AI history / Faceless AI channels`**.

---

### Cách 3: Quay màn hình làm mẫu 1 lần (`Recorded Once` — `[10:24]` – `[11:53]`)
> **Visual (`frame_0011`, `frame_0051` – `frame_0053`):**
> - Trên ứng dụng Claude Desktop, nhấn `+ Add` → **`Record a skill`**.
> - `frame_0053`: Hộp thoại **`Record a skill`** hiển thị thông báo:
>   *"Your screen, clicks, typing, and voice are recorded, then sent to Claude and turned into a repeatable skill. Don't type passwords or secrets, or display sensitive information or private conversations while recording."*
> - Bạn chỉ cần nhấn **Start recording**, vừa thao tác trên màn hình vừa nói thành tiếng giải thích lý do mình bấm vào từng bộ lọc. Khi kết thúc, Claude tự động tổng hợp toàn bộ thao tác chuột, bàn phím và lời giảng thành một file `SKILL.md` hoàn chỉnh. *(Tính năng độc quyền trên bản Claude Desktop).*

---

## 🌐 4. Kho Tàng Skills Chuẩn Mở & Cảnh Báo Bảo Mật (`[11:54]` – `[16:09]`)

Điểm mạnh nhất của kiến trúc Skills là nó được thiết kế như **một chuẩn mở (Open Standard)**: cùng một thư mục chứa `SKILL.md` có thể bê nguyên sang **Claude Code, Claude Desktop, OpenAI Codex, Cursor, hoặc Hermes Agent** mà không cần viết lại (`frame_0060`).

### 3 Nguồn tìm và "mượn" Skills chất lượng cao:
1. **Thư viện chính thức của Anthropic (`Browse skills` / `Plugins` — `frame_0051`, `frame_0055`):**
   - Hơn **200+ Skills & Plugins miễn phí** được phân loại theo danh mục: `Plugin Management (4.9M installs)`, `Productivity (2.9M)`, `Engineering (2M)`, `Finance (1.8M)`, `Product Management (1.6M)`, `Legal (1.4M)`, `Data`, cùng các skill nổi bật như `learn`, `doc-coauthoring`, `web-artifacts-builder`, `Render`, `Postiz (1.8M)`, `PDF Viewer (1.6M)`.
2. **GitHub & Các dự án Mã nguồn mở (`frame_0015`, `frame_0059`):**
   - Tìm kiếm cụm từ `"claude skills"` hoặc `"agent skills"` trên GitHub.
   - Các repo của chính Sharbel (`@sharbelxyz`): **`hermes-agent-mission-control`**, **`sharbelxyz/sophia`**, và hệ thống tự động hóa **X Bookmarks** (tự động đọc các bài viết đã bookmark trên X và biến chúng thành hành động/tác vụ thực tế mỗi ngày).
3. **Các Chợ Skills Cộng đồng & Meta-Skill `find-skills` (`frame_0060` – `frame_0062`):**
   - Các chợ phổ biến: **`clawmarketplaces.com`**, **`skillsmp.com` (SkillsMP)**, **`lobehub.com`**.
   - **Mẹo Pro:** Cài đặt meta-skill tên là **`find-skills`** — mỗi khi bạn hỏi Agent *"Làm sao để làm việc X?"*, Agent sẽ tự động dùng `find-skills` lục tìm trên các chợ cộng đồng để gợi ý và cài đặt Skill phù hợp nhất cho bạn.

### ⚠️ Cảnh báo Bảo mật Supply Chain (`[14:32]` – `[15:09]`):
- Vì một Skill không chỉ có văn bản hướng dẫn mà còn có thể đính kèm **executable scripts (Python/Bash)** để Agent thực thi trực tiếp trên máy của bạn, việc tải bừa một Skill trôi nổi trên mạng tiềm ẩn nguy cơ **Malware / Prompt Injection / Đánh cắp API Key**.
- **Quy tắc bắt buộc trước khi cài đặt Skill lạ:** Luôn yêu cầu AI Agent của bạn **đọc toàn bộ các file trong thư mục Skill đó và quét kiểm tra mã độc (scan for malicious code/exfiltration)** trước khi cho phép kích hoạt hoặc chạy bất kỳ script nào.

---

## 💡 Bài học rút ra & Hành động áp dụng (Key Takeaways)

1. **Dừng việc chia nhỏ hàng chục Sub-Agent tĩnh:** Hãy tập trung xây dựng một hệ thống Skills (`SKILL.md`)模組 hóa cho Agent chính để tận dụng cơ chế **Progressive Disclosure** — vừa giữ nguyên toàn bộ ngữ cảnh chung, vừa không làm phình Context Window.
2. **Tối ưu dòng `description` trong YAML Frontmatter:** Luôn viết `description` dưới dạng quy tắc điều hướng cho Agent (*"Làm gì + Khi nào cần gọi Skill này"*).
3. **Kết hợp MCP (Tay chân) + Skill (Quy trình não bộ):** Như ví dụ `vidIQ MCP` (cung cấp dữ liệu thô) kết hợp với Skill `youtube-pockets` (quy định tiêu chí lọc `Outlier >= 2x`, giới hạn 5 lần search, cách gom nhóm và đặt tiêu đề), sự kết hợp giữa **MCP + Skill** mới là công thức tạo ra quy trình tự động hóa chuyên sâu.
