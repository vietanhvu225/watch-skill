# How the Top 1% Use Claude Code Differently (9 Rules for Opus 5 & 5.5)

- **Source URL:** https://www.youtube.com/watch?v=J9NQTYUP6k0
- **Channel:** Simon Scrapes (`Agentic Academy` / `skool.com/scrapes`)
- **Duration:** 18:09
- **Watch Skill ID:** `4e29ebea365bb4f8`
- **Date Processed:** 2026-11-07
- **Tags:** #claude-code #opus-5-5 #plan-mode #goal-loop #claude-md #skills #plugin-evals #skill-doctor #hooks #subagents

---

## 1. Tổng Quan (Overview)

Video của **Simon Scrapes** bóc tách sự thay đổi căn bản trong cách vận hành **Claude Code** khi thế hệ model **Opus 5** và **Opus 5.5** (cùng bản thử nghiệm sớm *Fable*) ra mắt. Những lời khuyên phổ biến chỉ vài tháng trước (bật *Plan Mode* trước mọi tác vụ, gõ `"think hard"` / `"ultrathink"`, nhồi nhét hàng trăm dòng quy tắc viết HOA vào `CLAUDE.md`, hay cài hàng tá bộ *Skill Pack* từ GitHub) nay không những lỗi thời mà còn trực tiếp làm giảm chất lượng đầu ra và đốt sạch *context window*.

Dựa trên chia sẻ trực tiếp từ kỹ sư tạo ra Claude Code (**Boris Cherny — `@bcherny`**), tài liệu chính thức mới của Anthropic (`code.claude.com/docs/en/plugin-evals`), và kết quả benchmark **450 lượt chạy thử nghiệm thực tế** trên 9 bộ Skill phổ biến nhất GitHub, Simon tổng hợp **9 quy tắc thực chiến** phân tách nhóm 1% kỹ sư khai thác Claude Code hiệu quả nhất với phần còn lại:
1. **Khai tử Plan Mode truyền thống** — thay bằng **Brief 3 dòng (Task / Guardrails / Done)** kết hợp yêu cầu xuất **One-Page Visual Explainer** sau khi thực thi.
2. **Bỏ các từ khóa ép suy nghĩ (`think hard`, `ultrathink`)** — giữ mức **Effort Level** ở mặc định khuyến nghị (`Opus 5.5 Medium`).
3. **Thay câu nhắc *"Double-check your work"* bằng bài kiểm tra tất định (`PASS`/`FAIL` script)** như `check-report` để nâng chất lượng đầu ra gấp 2–3 lần.
4. **Dùng lệnh `/goal` kèm script kiểm chứng và giới hạn số vòng lặp (`stop after 15 turns`)** để khắc phục điểm mù của **Haiku Judge** (vốn chỉ đọc được cửa sổ chat mà không thể tự chạy terminal hay đọc file).
5. **Biến `CLAUDE.md` thành trang mục lục gọn dưới 200 dòng** (không lạm dụng `@import` vì file import vẫn nạp thẳng vào context ngay lúc khởi động).
6. **Tuân thủ 3 quy tắc trỏ file (Pointer Rules) và kiểm toán bằng `/doctor prompt-audit`** để tránh giới hạn preview **100 dòng đầu tiên** của Claude đối với file lồng nhau.
7. **Xây dựng Skill bằng cách dịch ngược từ lỗi thực tế (Reverse-Engineer Failures) hoặc dùng `Record a skill`** thay vì tự ngồi đoán quy trình.
8. **Dọn dẹp các Skill vô dụng bằng `/skill-doctor` và `claude plugin eval`** (benchmark 450 lượt chạy cho thấy **7/9 bộ Skill nổi tiếng trên GitHub không thắng nổi một file rỗng**).
9. **Thay thế quy tắc viết HOA (`IMPORTANT: YOU MUST...`) bằng Pre-tool Hooks tất định, giới hạn Sub-Agents, và vận hành các dự án tự hành theo sự kiện (Claude Code Projects & MCP Events).**

---

## 2. Phân Tích Chi Tiết 9 Quy Tắc Của Nhóm Top 1%

### 2.1. Quy tắc 1: "Plan Mode Is Dead" — Thay bằng Brief 3 dòng + Visual Explainer (`[00:01]` – `[02:18]`)
- **Bản chất thật của Plan Mode (`frame_0004`, `frame_0008`, `frame_0009`):**
  - Trước đây, các tài liệu hướng dẫn (như *Builder Methods* hay *ClaudeLog* — bấm `Shift+Tab` hai lần hoặc gõ `/plan` ở `frame_0000`–`frame_0002`) đều khuyên mở Plan Mode trước khi làm việc.
  - Ngày `25/09/2026`, trên chủ đề Hacker News *"Plan mode is dead"*, chính **Boris Cherny (`@bcherny`)** — tác giả của Claude Code — đã xác nhận (`frame_0008`, `frame_0009`):
    > *"[I work on Claude Code] I broadly agree with the author's point: **plan mode was useful, and is no longer useful.** In Claude Code, all plan mode does is add a little reminder to every user message along the lines of 'you're in plan mode, please don't code yet'. It's something I came up with late on a Sunday night because users kept asking for a way to plan before coding... Something people might not realize is **plan mode has always been a prompt — it has never changed the toolset because doing so would break the prompt cache, and so would be expensive for users.** ... With early versions of Fable and Opus 5.5, planning happens while the model works."*
- **Điểm yếu của việc duyệt bản kế hoạch dài bằng chữ:** Khi Claude sinh ra một bản kế hoạch văn bản dài trước khi đụng vào dữ liệu thực tế, người dùng thường đọc lướt rồi bấm *Approve*, trong khi mọi ngoại lệ thực sự chỉ lộ diện khi model bắt đầu đọc file và chạy công cụ.
- **Quy trình thay thế của nhóm 1% (`frame_0010`, `frame_0011`, `frame_0013`, `frame_0017`, `frame_0023`):**
  1. **Viết Brief 3 dòng ngắn gọn (`frame_0013`):**
     - **Task:** `This week's report from the CRM export` (Tác vụ cụ thể cần hoàn thành).
     - **Guardrails:** `Don't amend the template, don't email anyone` (Rào chắn những điều tuyệt đối không được tự ý làm).
     - **Done:** `Every figure matches the export` (Tiêu chí định nghĩa trạng thái hoàn thành).
  2. **Yêu cầu xuất One-Page Visual Explainer *sau khi* hoàn tất (`frame_0011`, `frame_0017`):**
     - Luồng làm việc mới (`frame_0017`): `You` $\rightarrow$ `Brief` $\rightarrow$ `Claude` $\rightarrow$ `Output + Explainer`.
     - Tận dụng tính năng **Claude Dashboards** và **Claude Motion** (ra mắt bản beta ngày `08/10/2026` bởi `@claudeai` — `frame_0011`), yêu cầu Claude sau khi làm xong phải tạo một trang giải thích trực quan (sơ đồ, dashboard tương tác, hoặc bản tóm tắt 1 trang) nêu rõ **nó đã làm gì và đã giả định (assumed) những điều gì**. Review kết quả thật kèm visual explainer nhanh và chính xác hơn nhiều so với đọc bản kế hoạch giả định.

---

### 2.2. Quy tắc 2: Bỏ từ khóa `"think hard"` / `"ultrathink"` — Giữ Effort ở mức khuyến nghị (`[02:18]` – `[03:19]`)
- **Cơ chế cũ vs. mới (`frame_0015`, `frame_0018`):**
  - Ở các thế hệ Claude Code đầu tiên, những từ khóa như `think`, `think hard`, `think harder`, `ultrathink` được gắn cứng với ngân sách token suy nghĩ (*thinking budget*).
  - Trên **Opus 5.5**, các từ này chỉ còn được xử lý như từ ngữ thông thường; việc ép model *"suy nghĩ thật kỹ"* bằng prompt thường khiến model tự suy diễn phức tạp hóa vấn đề (*second-guessing itself*).
  - Thay vào đó, hãy để **Effort Level** ở đúng mức khuyến nghị mặc định: **`Opus 5.5 Medium`** (`frame_0018`, `frame_0031`). Mức `Medium` trên Opus 5.5 đạt hoặc vượt chất lượng của Opus 5 ở mức `High` nhưng tốn ít bước và ít token hơn hẳn.

---

### 2.3. Quy tắc 3: Thay lời dặn *"Double-Check Your Work"* bằng bài kiểm tra tất định (`PASS`/`FAIL`) (`[03:20]` – `[05:06]`)
- **Tại sao tự kiểm tra bằng mắt lại thất bại:** Yêu cầu model *"kiểm tra lại bài làm"* chỉ khiến nó đọc lại chính văn bản nó vừa sinh ra và xác nhận *"Trông ổn rồi"*.
- **Nguyên lý từ Boris Cherny & Claude Code Docs (`frame_0021`):**
  - Bài viết trên `paddo.dev` (*"09 // The Most Important Tip"*): *"Boris saved the best for last: give Claude a way to verify its work. If Claude has that feedback loop, it will 2-3x the quality of the final result."*
  - Tài liệu chính thức `Claude Code Docs`: *"Give Claude a way to verify its work — Include tests, screenshots, or expected outputs so Claude can check itself. **It's the difference between a session you watch and one you walk away from.**"*
- **Cách áp dụng cho cả tác vụ phi lập trình (`frame_0025`):**
  - Ngay cả khi làm báo cáo kinh doanh hay nội dung marketing (dựa trên `brand-voice.md` gồm `## Tone` và `## Words we avoid`), hãy bảo Claude viết trước một script Python nhỏ trả về `PASS` hoặc `FAIL` (ví dụ script `check-report` ở `frame_0025`):
    ```python
    #!/usr/bin/env python3
    # every figure matches the export
    report = load("weekly-report")
    export = load("crm-export.csv")

    for name, value in report.figures:
        assert value == export[name], name

    for section in report.sections:
        assert section.filled, section.title
    ```
  - Một bài test tất định biến tiêu chí *"Done"* mơ hồ thành điều kiện nhị phân không thể ăn gian.

---

### 2.4. Quy tắc 4: Dùng `/goal` kèm điều kiện kiểm chứng & giới hạn lượt (`stop after N turns`) (`[05:07]` – `[06:40]`)
- **Điểm mù kiến trúc của `/goal` (`frame_0029`):**
  - Khi chạy lệnh `/goal`, sau mỗi lượt của `Claude (Turn 1)`, một model nhỏ và rẻ hơn là **`Judge (haiku)`** sẽ quyết định xem đã đạt mục tiêu chưa hay phải chạy tiếp.
  - Sơ đồ kiến trúc ở `frame_0029` chỉ rõ: **`Judge (haiku)` bị đánh dấu `❌` đỏ ở cả `$ terminal` lẫn `report.md`** — nó **chỉ có đường kết nối duy nhất tới `chat`**. Nghĩa là Haiku Judge **không thể tự chạy lệnh hay tự mở file kiểm tra**, mà hoàn toàn phụ thuộc vào những gì model chính báo cáo trong khung chat!
  - Nếu giao một `/goal` mơ hồ, model chính chỉ cần nói *"Tôi đã hoàn thành báo cáo"* là Haiku Judge lập tức cho dừng vòng lặp dù số liệu trong file vẫn sai.
- **Công thức `/goal` chuẩn (`frame_0031`):**
  - Ép model phải **chạy script kiểm chứng và in kết quả ra cửa sổ chat** (để Haiku Judge nhìn thấy), kèm theo rào chắn và giới hạn số vòng lặp:
    ```text
    /goal check-report script passes and you show me the output, the template is unchanged, or stop after 15 turns
    ```

---

### 2.5. Quy tắc 5: Thiết kế `CLAUDE.md` như một Trang Mục Lục dưới 200 dòng (`[07:07]` – `[08:08]`)
- **Bài học từ chính Anthropic (`frame_0032`):**
  - Khi phát hành Opus 5, đội ngũ Anthropic đã **xóa hơn 80% câu lệnh hướng dẫn hệ thống (system instructions)** bên trong chính Claude Code vì các quy tắc đỡ đần cho model đời cũ nay lại bó tay bó chân model đời mới.
  - Tài liệu chính thức (`frame_0032`) quy định rõ:
    - **`Size: target under 200 lines per CLAUDE.md file.`** File dài hơn làm tiêu tốn context và giảm mức độ tuân thủ (*adherence*).
    - **Cảnh báo về `@import`:** *"Imports don't reduce context cost because imported files also load at launch."* Việc tách file rồi dùng `@path/to/file` trong `CLAUDE.md` không hề tiết kiệm token vì toàn bộ nội dung file được import vẫn bị nạp ngay lúc khởi động phiên.
- **4 thứ duy nhất nên nằm trong `CLAUDE.md` (`frame_0033`):**
  1. `The tools and commands Claude can't guess` (Các công cụ và câu lệnh đặc thù của dự án mà Claude không thể tự đoán).
  2. `Anything you do differently to the normal way` (Những quy ước làm khác chuẩn thông thường).
  3. `The gotchas it's got wrong more than once` (Những cái bẫy thực tế mà Claude đã từng vấp phải nhiều hơn 1 lần).
  4. `Boundaries (e.g. only write the email draft)` (Ranh giới an toàn, ví dụ: chỉ được soạn nháp email chứ không được gửi).

---

### 2.6. Quy tắc 6: Ba quy tắc trỏ file (Pointer Rules) & lệnh `/doctor prompt-audit` (`[08:08]` – `[09:47]`)
- **3 quy tắc tổ chức tài liệu tham chiếu (`frame_0039`, `frame_0052`):**
  1. **Viết đường dẫn trong cặp dấu backtick kèm điều kiện kích hoạt** (ví dụ: *"Khi viết bài social, hãy đọc `docs/brand-voice.md`"*), tuyệt đối không dùng `@import` nếu không cần nạp ở mọi phiên.
  2. **Trỏ trực tiếp từ `CLAUDE.md` (không trỏ bắc cầu qua nhiều tầng file):** Nếu `CLAUDE.md` trỏ sang File A, rồi File A lại trỏ sang File B, thì ở các file lồng nhau đó Claude chỉ đọc bản preview **100 dòng đầu tiên**.
  3. **Mọi file dài quá 100 dòng bắt buộc phải có Mục lục (Table of Contents) ở ngay đầu file (`frame_0052`):** Vì *"CLAUDE ONLY READS THE FIRST 100 LINES OF LONG SKILL FILES"* trong lần quét đầu, việc đặt mục lục ở 100 dòng đầu giúp Claude biết chính xác cần đọc tiếp đoạn dòng nào.
- **Kiểm toán tự động bằng `/doctor prompt-audit` (`frame_0036`):**
  - Chạy lệnh `/doctor prompt-audit` trong Claude Code để tự động rà soát toàn bộ `CLAUDE.md`, rules và skills nhằm phát hiện: (a) các chỉ dẫn viết cho model đời cũ không còn cần thiết, (b) đường dẫn file hoặc câu lệnh đã bị hỏng, và (c) các quy tắc mâu thuẫn nhau giữa các file.

---

### 2.7. Quy tắc 7: Xây dựng Skill từ lỗi thực tế (Reverse-Engineer Failures) hoặc dùng `Record a skill` (`[09:48]` – `[12:00]`)
- **Sai lầm phổ biến:** Ngồi viết một file `SKILL.md` dài hàng trăm dòng mô tả từng bước lý thuyết trước khi cho model chạy thử.
- **Quy trình ngược của nhóm 1% (`frame_0038`):**
  1. Giao thẳng tác vụ cho Claude chạy **không có Skill** (*zero-shot*).
  2. Quan sát những chỗ model làm sai, thiếu ngữ cảnh hoặc gọi nhầm công cụ.
  3. Chỉ đưa đúng những điểm gãy đó (*gotchas*) vào một Skill nhỏ gọn, hoặc sử dụng nút **`Record a skill`** trên ứng dụng Claude Desktop để làm mẫu một lần cùng Claude rồi đóng gói lại.
  - **Ví dụ thực tế (`frame_0038`):** Simon dùng `Record a skill` để ghi lại quy trình **`ManyChat DM automation for the latest video`** kết hợp **Notion MCP** cho video *"Anthropic Just Solved Claude Cowork's Biggest Limitation (Goodbye MCP)"* trên kênh **Simon Scrapes** (`102k subscribers`).

---

### 2.8. Quy tắc 8: Kiểm chứng Skill bằng `claude plugin eval` & dọn rác với `/skill-doctor` (`[12:01]` – `[13:37]`)
- **Kết quả thực nghiệm chấn động (450 runs):**
  - Đội ngũ của Simon đã lấy **9 bộ Skill nhiều sao nhất trên GitHub**, thiết kế các bài toán đúng chuyên môn của từng bộ, và chạy benchmark tổng cộng **450 lượt** trên Opus 5.5 để so sánh với một **file dummy rỗng**.
  - Kết quả: **7 trên 9 bộ Skill nổi tiếng hoàn toàn không cải thiện kết quả so với file rỗng** (thậm chí một số bộ còn làm kết quả tệ đi và tốn thêm hàng chục nghìn token). Hai bộ duy nhất tạo ra khác biệt thực sự chỉ chứa các công cụ/lệnh nội bộ mà model không thể tự biết.
- **Hai công cụ chính thức để kiểm định & dọn dẹp (`frame_0040`, `frame_0041`):**
  1. **`claude plugin eval` (`code.claude.com/docs/en/plugin-evals` — `frame_0040`):** Chạy bộ test *The no-plugin baseline* (`cases * runs` có và không có plugin, mặc định `threshold: 1.0`). Độ chênh lệch điểm số ($\Delta$) cho biết chính xác Skill đó có thực sự đóng góp giá trị hay không.
  2. **`/skill-doctor` (`frame_0041`):** Tài liệu Anthropic nêu rõ: *"Every skill in the skill listing adds to your context on every turn, whether or not Claude ever uses it. Run `/skill-doctor` to see what each of your skills costs and how often it gets used, then disable or remove the ones that aren't earning their keep."* (Mở tab `Stats` trong trình quản lý `/plugin` hoặc chạy cờ `-p` ở chế độ non-interactive).

---

### 2.9. Quy tắc 9: Thay quy tắc viết HOA bằng Pre-tool Hooks, giới hạn Sub-Agents & chạy Event-Driven Projects (`[13:38]` – `[18:09]`)
- **Xóa quy tắc ALL-CAPS, thay bằng Deterministic Hooks (`frame_0043`, `frame_0046`):**
  - Những dòng kiểu `- IMPORTANT: YOU MUST run tests before committing` (trong các bản hướng dẫn `CLAUDE.md` cũ hồi tháng 4/2026 — `frame_0043`) cần được loại bỏ:
    - Nếu Opus 5.5 vốn đã tự làm điều đó $\rightarrow$ **xóa hẳn** cho nhẹ context.
    - Nếu đó là hành động rủi ro tuyệt đối không được phép xảy ra (ví dụ tự ý gửi email cho khách hàng ở `frame_0046`) $\rightarrow$ chuyển thành **Pre-tool Hook** tất định (tốn `0` token AI và chặn 100% ở tầng hệ thống).
- **Kiểm soát Sub-Agents (`frame_0046`):**
  - Opus 5 và 5.5 có xu hướng tự sinh Sub-Agents rất mạnh tay (đặc biệt ở mức `Ultracode` có thể đẻ ra nhiều nhánh sub-agent chồng chéo làm đội chi phí và thời gian cho các việc nhỏ).
  - Hãy **đặt giới hạn (cap) số lượng sub-agent** trong cấu hình; chỉ ép buộc gọi sub-agent bên trong một Skill khi tác vụ đòi hỏi tra cứu nặng hoặc gọi các MCP trả về dữ liệu quá lớn/nhiễu (`frame_0046`) nhằm giữ cho luồng chat chính luôn sạch.
- **Tự động hóa cấp độ cao nhất — Claude Code Projects & MCP Events (`frame_0050`, `frame_0052`, `frame_0053`):**
  - **Claude Code Projects (`frame_0050`):** Kiến trúc `You` $\rightarrow$ `Coordinator (Claude is working)` điều phối **8 luồng tác vụ chạy song song trên đám mây** ngay cả khi bạn đã tắt máy (`Client-side navigation`, `Optimize images`, `Cache the page content`, `Lazy-load scripts`, `Preload web fonts`, `Split the JS bundle`, `Reserve banner space`, `Compress static assets`).
  - **Custom Approval UI (`frame_0052`):** Thay vì ngồi gõ prompt mỗi ngày, Simon thiết kế luồng tự động dựng bài轮播 (carousel) Instagram cho `@simonscrapes` (*"7 new rules for Claude Skills"*) và đẩy lên một giao diện duyệt bài tối giản chỉ có 2 nút **`Reject` / `Approve`**.
  - **MCP Events (`frame_0053`):** Kết hợp thông báo từ **OpenAI DevDay 2026** (`openai.com/index/devday-2026-recap/` — **"MCP events for plugin automations"**), các hệ thống agent hiện đại chuyển hẳn sang mô hình *event-driven*: sự kiện từ MCP tự động kích hoạt workflow ngầm, chạy qua script kiểm chứng tất định, và chỉ thông báo cho con người ở bước phê duyệt cuối cùng.

---

## 3. Bảng Đối Chiếu Tư Duy Cũ vs. Top 1% Trên Opus 5.5

| Khía cạnh | Thói quen cũ (Thời kỳ tiền Opus 5) | Chuẩn Top 1% hiện tại (Opus 5 & 5.5) |
| :--- | :--- | :--- |
| **Lập kế hoạch** | Bật `Plan Mode` (`/plan`), đọc duyệt bản kế hoạch chữ dài lê thê trước khi chạy | Giao **Brief 3 dòng (`Task` / `Guardrails` / `Done`)** + yêu cầu xuất **1-Page Visual Explainer** sau khi làm xong |
| **Điều khiển suy luận** | Gõ từ khóa `"think hard"`, `"ultrathink"` vào prompt | Bỏ toàn bộ từ khóa ép suy nghĩ; giữ **Effort Level** ở `Opus 5.5 Medium` |
| **Kiểm tra chất lượng** | Nhắc `"Please double-check your work"` | Yêu cầu chạy script kiểm chứng tất định trả về `PASS`/`FAIL` (VD: `check-report`) |
| **Vòng lặp `/goal`** | Giao mục tiêu chung chung (bị **Haiku Judge** mù file/terminal cho dừng sớm) | `/goal <script> passes and you show me the output, <guardrail>, or stop after 15 turns` |
| **Tệp `CLAUDE.md`** | Dài hàng trăm dòng, dùng `@import` tràn lan, viết HOA `IMPORTANT: YOU MUST` | **Dưới 200 dòng**, đóng vai trò trang mục lục 4 mục; dùng backtick path có điều kiện + **Pre-tool Hooks** |
| **Quản lý Skill** | Tải các bộ Skill Pack nhiều sao trên GitHub; viết file Skill dài >100 dòng không có mục lục | Dịch ngược từ lỗi thực tế / dùng `Record a skill`; đặt mục lục ở **100 dòng đầu**; kiểm định bằng `claude plugin eval` & `/skill-doctor` |
