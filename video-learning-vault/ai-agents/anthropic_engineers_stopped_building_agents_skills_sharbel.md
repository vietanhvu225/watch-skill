# 🧠 Kỹ Sư Anthropic Đã Ngừng Chế Tạo Thêm AI Agents: Sự Lên Ngôi Của Kiến Trúc "Skills" & Nguyên Lý Progressive Disclosure — Sharbel A.

> **Nguồn tư liệu:** [Anthropic's Engineers Stopped Building AI Agents. Here's What They Build Instead. | Sharbel A.](https://www.youtube.com/watch?v=VxGIopUQVKo)  
> **Tác giả / Kênh:** **Sharbel A.**  
> **Thời lượng video:** 16 phút 09 giây (969s) | **Toàn văn Transcript:** [anthropic_engineers_stopped_building_agents_skills_sharbel_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/anthropic_engineers_stopped_building_agents_skills_sharbel_transcript.txt)  
> **Chủ đề cốt lõi:** Bước ngoặt tư duy kiến trúc tại Anthropic (Bỏ Multi-Agent Zoo, tập trung vào Skills), Phép loại suy Điện thoại Thông minh (Model = Chipset, Agent = OS, Skills = Apps), Giải phẫu tệp `SKILL.md`, Cơ chế Progressive Disclosure (Khai mở tăng dần tiết kiệm Context Window), 3 phương pháp tạo Skill (Manual, Interactive Q&A qua Claude, Screen Recording), Hệ sinh thái mở & Cảnh báo an ninh chuỗi cung ứng (Supply Chain Security Audit).

---

## Executive Summary: Cái Bẫy "Vườn Thú Đa Agent" & Mô Hình "Một Nhân Viên Nhiều Dụng Cụ"

Trong nhiều tháng, hầu hết các kỹ sư và nhà phát triển AI đều rơi vào cùng một cái bẫy: **Cứ có một tác vụ mới là lại dựng lên một Agent (hoặc Sub-agent) độc lập.** Một agent chuyên viết nội dung, một agent trả lời email, một agent rà soát mã nguồn, một agent tiếp cận khách hàng. Hậu quả là các nhóm phát triển phải "trông trẻ" một vườn thú hỗn loạn gồm hàng tá agent chưa được huấn luyện thấu đáo, tiêu tốn tài nguyên quản lý, giám sát và ngữ cảnh.

Các kỹ sư tại Anthropic đã đi đến một kết luận mang tính bước ngoặt: **Hãy dừng việc chế tạo thêm agent. Thay vào đó, hãy trang bị cho MỘT Agent duy nhất một Hộp Công Cụ (Toolbox) chứa các Kỹ Năng (Skills) có thể mở rộng vô hạn.**

```
+----------------------------------------------------------------------------------------------------+
|               PHÉP LOẠI SUY ĐIỆN THOẠI THÔNG MINH (THE SMARTPHONE ANALOGY)                         |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ AI MODEL (Opus 5.5, Sonnet, Argon) ]  ───────>  BỘ VI XỬ LÝ (PROCESSOR / CHIPSET)               |
|    • Cung cấp sức mạnh suy luận thô, tốc độ và logic cơ sở.                                        |
|                                                                                                    |
|  [ AI AGENT (Claude Code, Cursor, Codex) ] ───>  HỆ ĐIỀU HÀNH (OPERATING SYSTEM)                   |
|    • Quản lý bộ nhớ, quyền truy cập tệp tin, điều phối công việc và giao tiếp.                     |
|                                                                                                    |
|  [ AI SKILLS (Các thư mục SKILL.md) ]    ───────>  CÁC ỨNG DỤNG (APPS TRÊN ĐIỆN THOẠI)             |
|    • Bạn không mua một chiếc điện thoại mới mỗi khi cần thêm tính năng.                           |
|    • Bạn chỉ cần cài thêm một Ứng Dụng (Skill) mới, và chiếc điện thoại lập tức làm được việc đó!  |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 1. Giải Phẫu Tệp Kỹ Năng (`SKILL.md`) & Cơ Chế Khai Mở Tăng Dần (Progressive Disclosure)

Về bản chất kỹ thuật, một Skill cực kỳ tinh gọn: **Nó chỉ là một thư mục chứa một tệp văn bản duy nhất mang tên `skill.md` (hoặc `SKILL.md`).**

```
skills/
└── youtube-pockets/
    ├── SKILL.md          <── Tệp chỉ dẫn cốt lõi (Frontmatter + Hướng dẫn)
    ├── scripts/          <── (Tùy chọn) Mã nguồn Python / Bash hỗ trợ
    └── examples/         <── (Tùy chọn) Dữ liệu mẫu tham chiếu
```

### Cấu trúc 2 tầng của `SKILL.md` [01:56]:
1. **Frontmatter Header (Metadata điều phối):**
   ```markdown
   ---
   name: youtube-pockets
   description: Chuyên tìm kiếm các ngách chủ đề YouTube đang viral dựa trên outlier score > 2x. Sử dụng khi người dùng yêu cầu phân tích xu hướng hoặc tìm ý tưởng video.
   ---
   ```
   * **Nguyên tắc cốt tử:** Phần `description` **không viết cho con người đọc, mà viết riêng cho AI Agent**. Đây là chiếc "chìa khóa định tuyến" (routing key) để Agent biết chính xác khi nào thì cần mở hộp công cụ lấy chiếc cờ-lê này ra.
2. **Body Instructions (Cẩm nang thao tác):**
   * Viết bằng ngôn ngữ tự nhiên rõ ràng, quy định tuần tự từng bước cần làm, các bộ lọc cần áp dụng, công cụ MCP nào cần gọi và định dạng kết quả đầu ra.

### Bí mật tiết kiệm Token: Progressive Disclosure (Khai mở tăng dần) [02:20]:

```
┌────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                             NGUYÊN LÝ PROGRESSIVE DISCLOSURE TRONG SKILLS                          │
├────────────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                                    │
│  TÌNH HUỐNG: Bạn sở hữu 200 Skills trong hệ thống.                                                 │
│                                                                                                    │
│  [ CÁCH LÀM SAI (Nạp toàn bộ vào Prompt) ]                                                         │
│  Mỗi tin nhắn người dùng gửi đến, hệ thống nhồi toàn bộ 200 file SKILL.md vào Context Window.     │
│  ===> Đốt sạch 200,000 tokens ngay lập tức! Chi phí trên trời, Agent bị ảo giác và quá tải.        │
│                                                                                                    │
│  [ CÁCH LÀM CHUẨN CỦA ANTHROPIC (Progressive Disclosure) ]                                         │
│  1. Trạng thái nghỉ (Hộp đồ nghề đóng):                                                            │
│     Agent CHỈ đọc `name` và `description` của 200 skills (~50 tokens/skill = 10,000 tokens).      │
│                                                                                                    │
│  2. Khi có yêu cầu ăn khớp:                                                                        │
│     User: "Hãy tìm cho tôi 5 ngách AI đang hot nhất trên YouTube tuần này."                        │
│     Agent rà soát description ──> Khớp với skill `youtube-pockets`!                                │
│                                                                                                    │
│  3. Mở ngăn kéo (On-Demand Loading):                                                               │
│     Lúc này Agent mới gọi tool đọc NỘI DUNG CHI TIẾT của duy nhất file `youtube-pockets/SKILL.md`.  │
│                                                                                                    │
└────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Ba Phương Pháp Chế Tạo Skill: Từ Thủ Công Đến "Siêu Lười"

Tác giả trình diễn 3 cách xây dựng Skill đi từ thấp đến cao:

```
[ METHOD 1: VIẾT TAY THỦ CÔNG ] ────────> Dành cho người muốn hiểu sâu cơ chế gốc (Không khuyến khích)
                  │
                  ▼
[ METHOD 2: PHỎNG VẤN Q&A VỚI CLAUDE ] ─> Phương pháp vàng dùng cho 90% trường hợp thực tế
                  │
                  ▼
[ METHOD 3: SCREEN RECORDING ] ─────────> "Cách lười nhất" — Quay video thao tác màn hình để Claude tự học
```

### Phương pháp 1: Viết tay trực tiếp (Manual Creation) [04:30]
* Thích hợp cho việc nắm vững cú pháp frontmatter và cấu trúc chỉ lệnh.
* Nhược điểm: Dễ sót các trường hợp biên (edge cases), mất nhiều thời gian định dạng.

### Phương pháp 2: Đối thoại phỏng vấn có định hướng qua Claude (Recommended) [07:01]
* **Cách thực hiện:** Thay vì tự viết cú pháp, hãy nói ra quy trình bạn muốn bằng lời thoại tự nhiên (hoặc gõ prompt tổng quan).
* Claude sẽ đóng vai trò người phỏng vấn, liên tục đặt ngược lại **10 đến 15 câu hỏi làm rõ**:
  - *"Bộ lọc thời gian cụ thể là bao lâu (tuần hay tháng)?"*
  - *"Điểm Outlier Score tối thiểu là bao nhiêu (2x hay 3x)?"*
  - *"Định dạng tiêu đề gợi ý có cần theo cấu trúc 'Làm thế nào để...' không?"*
* Người dùng chỉ cần trả lời ngắn gọn qua voice hoặc gõ phím. Claude tự động tổng hợp toàn bộ tri thức thành một tệp `SKILL.md` đạt chuẩn chuyên nghiệp.
* **Thực nghiệm kỹ năng `YouTube Pockets` [08:30]:**
  - Sau khi nạp skill, tác giả chỉ cần ra lệnh: `Use YouTube pockets for the AI niche`.
  - Agent tự động kích hoạt MCP (như VidIQ), quét từ khóa, lọc video có lượt xem > 50,000 và Outlier Score $\ge 2\times$, tự động nhận diện 4 ngách đang bùng nổ: *AI Danger Explained Calmly, Contrarian AI Opportunity Takes, AI Robots 2026, Faceless AI Channels*.

### Phương pháp 3: Ghi hình màn hình (Screen Recording - Laziest Way) [10:38]
* Tính năng độc quyền trên ứng dụng Claude Desktop: Cho phép người dùng bật ghi hình màn hình và thao tác công việc bằng tay như thường lệ (ví dụ: mở hòm thư, tải tệp đính kèm, đổi tên theo quy chuẩn, nén zip và đẩy lên kho lưu trữ).
* Claude tự động phân tích thị giác từng cú click chuột, tự hiểu logic nghiệp vụ và dịch ngược toàn bộ hành động đó thành một bản hướng dẫn tự động hóa trong `SKILL.md`.

---

## 3. Hệ Sinh Thái Mở (Open Standard) & Nơi Tìm Kiếm Skills Hàng Đầu

Nhờ việc cộng đồng công nghệ chuẩn hóa cấu trúc thư mục Skills, một tệp `SKILL.md` hiện nay có tính **tương thích đa nền tảng (Cross-platform portability)** tuyệt đối: Một skill viết cho Claude có thể chạy mượt mà trên **Claude Code, Cursor, OpenAI Codex, Hermes Agent, Antigravity** mà không cần sửa đổi.

### 2 Nguồn cung cấp Skills lớn nhất [12:00]:
1. **Kho chính thức (Official Claude Catalog):** Tích hợp sẵn trong giao diện Claude Desktop với hơn 200 kỹ năng được kiểm duyệt kỹ càng.
2. **Cộng đồng Mã Nguồn Mở (GitHub & Chợ Marketplace):**
   * **GitHub Repositories:** Tìm kiếm theo từ khóa hoặc theo dõi mục Trending. Tác giả mở mã nguồn các skills nổi tiếng như *Hermes Agent Mission Control Dashboard*, *X Bookmarks Auto-actioning*.
   * **Chợ chuyên dụng:** `skillsmp.com`, `clawmarketplaces.com`, `lobehub.com`.
   * **Meta-Skill "Find Skills":** Một kỹ năng cài sẵn vào Agent, có nhiệm vụ lắng nghe khi bạn than thở: *"Làm sao để làm việc X?"* $\to$ nó tự động tìm kiếm trên các marketplace và đề xuất tải về kỹ năng tương ứng.

---

## 4. Cảnh Báo Sống Còn: Kiểm Toán An Ninh Chuỗi Cung Ứng (Supply Chain Security)

Một sai lầm chết người mà rất nhiều người dùng mắc phải khi sử dụng các chợ Skills mở: **Copy link skill của người lạ và bấm Install ngay lập tức.**

```
+────────────────────────────────────────────────────────────────────────────────────────────────────+
|                     MỐI NGUY HẠI TỪ CÁC SKILLS BÊN THỨ BA (THIRD-PARTY SKILLS)                      |
+────────────────────────────────────────────────────────────────────────────────────────────────────+
|                                                                                                    |
|  BẢN CHẤT CỦA SKILL:                                                                               |
|  Không chỉ là văn bản, Skill thường chứa các đoạn script Bash, lệnh Python hoặc gọi Webhook MCP!   |
|                                                                                                    |
|  CÁC KỊCH BẢN TẤN CÔNG ĐIỂN HÌNH:                                                                  |
|  ├── Đánh cắp Token/API Key: Chèn ngầm lệnh `curl` gửi biến môi trường (.env) về máy chủ lạ.       |
|  ├── Prompt Injection: Lệnh ẩn chỉ đạo Agent âm thầm ghi đè file nhạy cảm trong máy tính.          |
|  └── Mã độc cài cắm: Chạy các script nhị phân nguy hiểm dưới quyền hạn của Agent.                  |
|                                                                                                    |
|  QUY TẮC PHÒNG THỦ BẮT BUỘC [15:00]:                                                               |
|  1. TUYỆT ĐỐI KHÔNG cài đặt mù quáng bất kỳ Skill nào từ internet mà chưa đọc code.               |
|  2. Ra lệnh cho Agent KIỂM TOÁN AN NINH (Security Audit) toàn bộ nội dung tệp `SKILL.md`            |
|     và các script đi kèm trước khi cấp quyền kích hoạt.                                            |
|                                                                                                    |
+────────────────────────────────────────────────────────────────────────────────────────────────────+
```

---

## 5. Bảng So Sánh Chiến Lược: Multi-Agent Pipeline vs Skill-Based Single Agent

| Đặc điểm | Multi-Agent Zoo (Nhiều Agent riêng lẻ) | Skill-Based Architecture (1 Agent + Đa Skills) |
|---|---|---|
| **Cơ chế hoạt động** | Khởi tạo nhiều Agent/Sub-agent cho từng tác vụ con. | Một Agent trung tâm duy nhất, nạp kỹ năng on-demand. |
| **Chi phí Token** | Cực lớn do ngữ cảnh bị phân mảnh và nhân bản qua nhiều agent. | Tối ưu tuyệt đối nhờ cơ chế **Progressive Disclosure**. |
| **Bảo trì & Nâng cấp** | Ác mộng quản trị: Phải cấu hình và tinh chỉnh prompt cho từng con. | Cực dễ: Muốn đổi nghiệp vụ chỉ cần sửa 1 file text `SKILL.md`. |
| **Tính tái sử dụng** | Thường bị gắn chặt vào một dự án cụ thể. | Dùng chung toàn cầu: Chia sẻ qua Git hoặc dùng chung giữa Claude, Cursor, Hermes. |
| **Độ tin cậy** | Dễ đứt gãy dây chuyền (Pipeline failure cascading). | Ổn định cao: Agent mẹ giữ vững bộ nhớ và ngữ cảnh xuyên suốt. |

---

## Actionable Checklist Áp Dụng Ngay:
1. **Xóa sổ thói quen tạo Sub-agent tràn lan:** Khi gặp một công việc mới, hãy tự hỏi: *"Mình có thể biến việc này thành một tệp `SKILL.md` trong thư mục `skills/` không?"*
2. **Chuẩn hóa Description cho Agent:** Đảm bảo trường `description` trong YAML frontmatter nêu rõ **ĐIỀU KIỆN KÍCH HOẠT (Trigger Condition)** bằng những câu từ cụ thể nhất.
3. **Phỏng vấn Claude để sinh Skill:** Dành 3 phút trò chuyện và trả lời các câu hỏi làm rõ của Claude để có được bản `SKILL.md` hoàn chỉnh bao quát mọi tình huống lỗi.
4. **Kiểm tra an ninh 100%:** Luôn quét sạch mã độc, script ẩn hoặc lệnh gửi dữ liệu ra ngoài trước khi cài đặt bất kỳ Skill nào từ cộng đồng.
