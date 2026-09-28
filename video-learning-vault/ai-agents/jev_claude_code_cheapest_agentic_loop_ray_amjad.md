# Jev + Claude Code: The Cheapest Agentic Coding Loop Yet — Ray Amjad

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=ScvXFi4MUSc)
- **Watch Skill ID:** `b14c5995731f05b3`
- **Kênh phát hành:** Ray Amjad
- **Category:** #ai-agents, #jev, #claude-code, #system1-system2, #agentic-coding, #jevons-paradox, #code-review
- **Date Processed:** 2026-09-29
- **Duration:** 27:28 (1648.0 giây)
- **Transcript File:** [jev_claude_code_cheapest_agentic_loop_ray_amjad_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/jev_claude_code_cheapest_agentic_loop_ray_amjad_transcript.txt)

---

## 1. Nguồn Gốc Tên Gọi & Định Lý Nghịch Lý Jevons (Jevons Paradox)

Ray Amjad mở đầu bằng việc giải thích nguồn gốc tên gọi của **Jev**: mô hình được đặt tên theo **Nghịch lý Jevons (Jevons Paradox)** trong kinh tế học — *"Khi hiệu suất sử dụng một nguồn tài nguyên tăng lên và chi phí giảm mạnh, tổng mức tiêu thụ của nguồn tài nguyên đó trên thực tế sẽ bùng nổ theo cấp số nhân thay vì giảm đi."*

Trong kỷ nguyên Agentic, khi chi phí cho mỗi quyết định giảm xuống chỉ còn $0.00004 và độ trễ dưới 200ms, lập trình viên sẽ không giảm bớt số lần gọi AI, mà sẽ **cài cắm hàng trăm ngàn lượt kiểm tra AI vào từng dòng code, từng cú click chuột, và từng PR**.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                       SỰ KẾT HỢP HOÀN HẢO GIỮA SYSTEM 1 VÀ SYSTEM 2                         │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│     SYSTEM 2 (Tư Duy Chậm, Có Ý Thức)            SYSTEM 1 (Phản Xạ Nhanh, Tiềm Thức)        │
│        Claude Code / Opus 5 / Astra                           Jev (TypeSafe)                │
│     ┌───────────────────────────────────┐        ┌──────────────────────────────────┐       │
│     │ • Hoạch định chiến lược tổng thể  │        │ • Phản xạ cục bộ (Local tactics) │       │
│     │ • Đánh giá thất bại & viết lại luật│  ────► │ • Tương tác trực tiếp thời gian  │       │
│     │ • Tái huấn luyện tiêu chí cho Sys1│        │   thực (70ms – 200ms)            │       │
│     │ • Độ trễ cao, chi phí token đắt   │ ◄────  │ • Bắt lỗi, phân loại, lọc trước  │       │
│     └───────────────────────────────────┘        └──────────────────────────────────┘       │
│                       ▲                                    ▲                                │
│                       └───────── Cầu nối: ─────────────────┘                                │
│                                 Switch Statement / Rules                                    │
│                                                                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

> *"Khi mới tập lái xe, bạn dùng System 2 để quan sát từng gương chiếu hậu, sang số, đạp côn một cách chậm chạp. Qua hàng ngàn giờ luyện tập, mọi thao tác chuyển hóa thành phản xạ vô điều kiện của System 1. Jev chính là hệ phản xạ vô điều kiện đó cho Agent."*

---

## 2. Ba Nguyên Mẫu Quyết Định Cốt Lõi (The 3 Primitives)

1. **`null` (Đánh giá Chân trị - Boolean Truth Rating):**
   - Trả về độ xác thực kèm xác suất hiệu chuẩn (Calibrated Probability).
   - *Ví dụ kiểm tra hóa đơn gian lận (Fraud Detection):* Jev trả về `85% True`. Khi nạp thêm danh sách các tín hiệu gian lận (`fraud signals`), Jev tự động điều chỉnh lên `94% True` trong vòng chưa đầy **100ms**.
2. **`choice` (Lựa chọn Đa phương án - Multiple Choice):**
   - Hỗ trợ tối đa lên tới **255 tùy chọn enum (Cardinality cap)**.
   - Trả về nhãn được chọn kèm bảng phân phối xác suất trên toàn bộ 254 nhãn còn lại.
3. **`score` (Chấm điểm theo Thang đo Rubric):**
   - Hỗ trợ thiết lập tối đa **11 mốc Rubric neo (Anchor Rubrics)**, tạo ra thang điểm liên tục từ **0 đến 10**.
   - *Ví dụ chấm điểm mức độ nghiêm trọng log hệ thống cho On-call Engineer:*
     - Mốc 0: Log định kỳ thông thường.
     - Mốc 3: Cạn kiệt Connection Pool cơ sở dữ liệu.
     - Jev đánh giá log lỗi đạt `2.99 / 3.0` $\to$ Code kích hoạt cảnh báo PagerDuty gọi kỹ sư trực ca ngay lập tức.

---

## 3. Các Thử Nghiệm Đột Phá Được Ray Amjad Trình Diễn

### A. Live Demo: Jev và Astra Phối Hợp Chơi Game Minecraft Thời Gian Thực
- **Kiến trúc phân tầng:**
  - *GPT-5.6 Astra (System 2):* Cứ 2 phút một lần hoặc sau các cột mốc/thất bại lớn, Astra nhìn nhận toàn cảnh và giao mục tiêu trung hạn (Xây nơi trú ẩn trước khi trời tối $\to$ Chế tạo lò nung $\to$ Chế tạo cúp đá $\to$ Đào kim cương $\to$ Mở cổng Nether).
  - *Jev (System 1):* Nhận trạng thái thời gian thực (máu, độ đói, thời gian trong ngày, quái vật xung quanh) và bấm các phím điều khiển (WASD, đào gỗ, chạy trốn Creeper, đặt cửa vào nhà).
- **Kết quả:** Jev tự chế tạo cúp đá, đào thành công kim cương và đi thẳng tới cánh cổng Nether (The Nether) hoàn toàn mượt mà!

```
[Mục tiêu tối thượng: Slay the Ender Dragon]
                  │
                  ▼
[Astra System 2: Kế hoạch 2 phút / lần] ──► "Trú ẩn ban đêm, đào sắt, chế tạo cúp"
                  │
                  ▼
[Jev System 1: Vi quyết định 100ms]      ──► Thu thập gỗ, né Skeleton, đặt cửa gỗ
```

---

### B. Bộ Định Tuyến Kỹ Năng Động (Dynamic Skill Router Cho Coding Agents)
- **Vấn đề "Context Bloat" của Claude Code:** Khi người dùng cài đặt hàng trăm kỹ năng (skills), việc nhồi nhét mô tả của toàn bộ kỹ năng vào System Prompt làm tiêu tốn từ **10,000 đến 20,000 tokens** cho mỗi turn trò chuyện một cách vô ích.
- **Giải pháp với Jev (Chứng minh trên Hermes Agent):**
  - Cho Jev làm cổng chọn lọc trong số **182 skills**.
  - Nếu để LLM tự chọn: tỷ lệ load sai skill lên tới **17%**.
  - Khi có Jev làm Router đề xuất: tỷ lệ load sai giảm xuống chỉ còn **7.3%**!
  - **Lợi ích:** Cắt giảm ngay lập tức **10,000 tokens** rác khỏi Context Window của Claude Code, giúp Agent phản hồi nhanh hơn và không bị xao nhãng.

---

### C. Vòng Lặp Phản Hồi Kiểm Thử Trình Duyệt Siêu Rẻ (Massive Browser-Use Feedback Loop)
- Ray sử dụng Jev kết hợp Browser-Use: Tự động đặt vé máy bay từ Zurich đến London hoàn thành trong **7 giây** với chi phí chỉ **$0.004 (4/10 của 1 xu)**.
- **Massively Parallel Adversarial Testing:** Với chi phí rẻ tới mức khó tin, các đội ngũ kỹ thuật có thể dựng **hàng chục đến hàng trăm browser agent chạy Jev song song trên mỗi Pull Request (PR)**, click ngẫu nhiên và cố tình phá vỡ giao diện web để tìm bug trước khi merge code vào production.

---

### D. Qualitative Linters & Dọn Dẹp "Comment Rác" (Garbage Comments Cleanup)
- Ray Amjad thử nghiệm chạy Jev trên một codebase thực tế:
  - Quét **150 comments** trong mã nguồn chỉ mất **9.3 giây**, tốn **1 xu ($0.01)**.
  - Dự toán quét sạch toàn bộ codebase: tốn **57 xu ($0.57)** để lọc ra 1,700 comment vô nghĩa (ví dụ comment kiểu `x = x * 2 // nhân x với 2`).
- **AI Qualitative Linter:** Dùng Jev bắt các vi phạm chất lượng code mà linter truyền thống không bắt được:
  - Tên hàm có miêu tả đầy đủ tác dụng phụ (side-effects) không?
  - Dòng log có vô tình làm lộ Secret hay thông tin thẻ tín dụng/PII không?
- **Quét Code Smells (Theo sách Refactoring của Martin Fowler):** Phân tích toàn bộ codebase gồm **28 triệu tokens đầu vào**, Jev hoàn thành xuất sắc với tổng hóa đơn chỉ vỏn vẹn **$1.19**!

---

### E. Giảm Chi Phí Code Review của Claude Code Gấp 10 Lần
- Code review bằng Claude Opus thường tiêu tốn lượng token khổng lồ.
- Thiết lập Jev làm **Tầng gác cổng 100 câu hỏi trực giác (100 Gut Questions)** trên bản Git Diff:
  - Thay đổi này có làm yếu đi bài unit test nào không?
  - Bề mặt rủi ro an mật có tăng lên không?
  - Có hardcode chuỗi nhạy cảm nào không?
- Chỉ những điểm bị Jev đánh điểm rủi ro cao mới được đẩy lên Claude Opus để phân tích và viết phản biện.
- **Kết quả đo đạc từ Sentry:** Các kỹ sư tại **Sentry** xác nhận việc thay thế mô hình open-source GPT-OSS 120B bằng Jev trong pipeline bảo mật giúp pipeline **nhanh hơn, rẻ hơn gấp 5 lần và đạt độ chính xác cao hơn**!

---

## 4. Tài Nguyên & Tham Khảo
- [Transcript gốc chi tiết](file:///f:/source/watch-skill/video-learning-vault/ai-agents/jev_claude_code_cheapest_agentic_loop_ray_amjad_transcript.txt)
- [Bambood Multi-Agent Setup (AICodeKing)](file:///f:/source/watch-skill/video-learning-vault/ai-agents/astra_jev_ds_v4_1_flash_aicodeking.md)
- [LangChain Jev Harness](file:///f:/source/watch-skill/video-learning-vault/ai-agents/building_a_harness_with_jev_langchain.md)
