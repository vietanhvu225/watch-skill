# 🌐 CEO Vercel Tiết Lộ Quy Trình Agentic Engineering: Nợ An Ninh, Tầng Trừu Tượng Harness & Lợi Thế Cạnh Tranh Thời Hậu AGI — Guillermo Rauch

> **Nguồn tư liệu:** [Vercel CEO reveals his Agentic Engineering Workflow | Guillermo Rauch](https://www.youtube.com/watch?v=WeiB_gLOdQE)  
> **Khách mời:** **Guillermo Rauch** (Sáng lập & CEO của Vercel, tác giả Next.js, Socket.io, Mongoose)  
> **Thời lượng video:** 01 giờ 37 phút 49 giây (5,869s) | **Toàn văn Transcript:** [vercel_ceo_agentic_engineering_workflow_guillermo_rauch_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/vercel_ceo_agentic_engineering_workflow_guillermo_rauch_transcript.txt)  
> **Chủ đề cốt lõi:** Tầm nhìn thế giới thời Hậu AGI (Post-AGI Era), Sự dịch chuyển đòn bẩy kỹ thuật lên tầng Harness Engineering & Software Factory, Nợ an ninh (Security Debt) & Đưa toàn bộ vòng đời phát triển vào Cloud Sandboxes để triệt tiêu tấn công chuỗi cung ứng, Lợi thế cạnh tranh khi "ai cũng có thể build mọi thứ" (Gu thẩm mỹ, Storytelling & Đánh giá Evals có căn cứ thay vì chạy theo Vibe ảo), Kiến trúc Backend Agent-First (Supabase connector & Serverless preview deployments).

---

## Executive Summary: Kỷ Nguyên Hậu AGI & Sự Nâng Tầng Trừu Tượng Của Kỹ Sư

Trong buổi đối thoại chuyên sâu dài gần 100 phút, Guillermo Rauch — CEO của Vercel và là một trong những tượng đài công nghệ của cộng đồng web thế giới — đã phác họa bức tranh toàn cảnh về sự chuyển dịch từ lập trình truyền thống sang **Kỹ nghệ Tác nhân (Agentic Engineering)**.

Khi các mô hình AI thế hệ mới (như Opus 5.5, GPT-6, Astra) có thể tự động viết code, tạo dựng toàn bộ thế giới ảo 3D và giao diện người dùng phức tạp chỉ sau vài câu lệnh, **giá trị cốt lõi của người kỹ sư không hề biến mất mà dịch chuyển lên một tầng trừu tượng cao hơn (One layer of abstraction higher)**:

```
+----------------------------------------------------------------------------------------------------+
|               SỰ DỊCH CHUYỂN TẦNG TRỪU TƯỢNG TRONG KỸ NGHỆ PHẦN MỀM (GUILLERMO RAUCH)             |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ TẦNG CŨ: LẬP TRÌNH THỦ CÔNG (MANUAL CODING) ]                                                   |
|    • Con người ngồi gõ từng thẻ HTML, từng câu truy vấn SQL, từng API route.                       |
|    • Tương đương với việc làm nông thủ công 200 năm trước (chiếm 90% nhân lực xã hội).              |
|                                     │                                                              |
|                                     ▼ (AI đảm nhận việc gõ code)                                   |
|                                                                                                    |
|  [ TẦNG MỚI: AGENTIC ENGINEERING & HARNESS ORCHESTRATION ]                                         |
|    • HARNESS ENGINEERING: Thiết kế môi trường, rào chắn, sandbox và công cụ cho AI Agent.          |
|    • SOFTWARE FACTORY: Điều phối hạm đội bầy đàn Agent (Fleets of Agents) chạy song song.          |
|    • CREATIVE & TASTE CONTROL: Định hình gu thẩm mỹ đỉnh cao, nghệ thuật kể chuyện (Storytelling).  |
|    • CYBERSECURITY CONTROL: Quản trị nợ an ninh, cô lập sandbox để chống rò rỉ mã độc.            |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 1. Nợ An Ninh (Security Debt) & Tại Sao Phải Đưa Vòng Đời Lập Trình Vào Sandboxes

Mối đe dọa lớn nhất hiện nay khi các tổ chức áp dụng AI Coding Agent với tốc độ chóng mặt chính là **Nợ An Ninh (Security Debt)** [00:01]:
* **Bề mặt tấn công phình to (Massive Attack Surface):** Khi Agent có thể sinh ra hàng triệu dòng code trong vài giờ, lượng lỗ hổng tiềm ẩn (Latent Vulnerabilities) ẩn sâu trong codebase trở nên không thể đo lường nổi bằng sức người.
* **Tấn công Chuỗi Cung Ứng (Supply Chain Attacks):** Khi Agent tự động cài đặt các gói phụ thuộc npm/pip bên thứ ba, nguy cơ bị chèn mã độc đánh cắp biến môi trường (`.env`) là cực lớn.
* **Giải pháp của Vercel:** **Chuyển dịch toàn bộ vòng đời phát triển phần mềm vào các Cloud Sandboxes cô lập tuyệt đối.**
  * Agent không được phép chạy code trực tiếp trên máy trạm của kỹ sư hoặc môi trường production.
  * Mọi lần build, test và preview đều diễn ra trong các container siêu nhẹ, dùng xong là hủy (Ephemeral Sandboxes), triệt tiêu hoàn toàn khả năng lây lan mã độc vào hệ thống cốt lõi.

---

## 2. Khi "Ai Cũng Có Thể Build Mọi Thứ": Đâu Là Lợi Thế Cạnh Tranh Thực Sự?

Khi rào cản kỹ thuật gõ code bị xóa sổ bởi AI, bất kỳ ai cũng có thể dựng một ứng dụng trong 10 phút. Vậy điều gì tạo nên sự khác biệt giữa một sản phẩm thất bại và một đế chế tỷ đô? [05:28]

```
+────────────────────────────────────────────────────────────────────────────────────────────────────+
|                     3 TRỤ CỘT LỢI THẾ CẠNH TRANH TRONG THỜI ĐẠI AI                                 |
+────────────────────────────────────────────────────────────────────────────────────────────────────+
|                                                                                                    |
|  [ 1. GU THẨM MỸ ĐỈNH CAO & TRẢI NGHIỆM WOW (TASTE & VIBES) ]                                      |
|    • Web tiêu dùng trong tương lai sẽ cực kỳ lộng lẫy: Đồ họa 3D, Shaders, hiệu ứng hạt hạt        |
|      (Particle effects), Animation siêu mượt 120 FPS. Người dùng mua sản phẩm vì CẢM XÚC.          |
|                                                                                                    |
|  [ 2. NGHỆ THUẬT KỂ CHUYỆN (STORYTELLING & HUMAN AMBITION) ]                                       |
|    • Con người luôn muốn mua câu chuyện từ con người khác. AI là công cụ khuếch đại tham vọng      |
|      và thông điệp thương hiệu, không phải là thứ thay thế linh hồn người sáng lập.                |
|                                                                                                    |
|  [ 3. HỆ THỐNG EVALS THỰC CHỨNG (GROUNDED EVALS VS EMPTY VIBES) ]                                  |
|    • Đa số lập trình viên hiện nay dùng AI dựa trên "Vibe rỗng tuếch" (nghe đồn trên Twitter).     |
|    • Lợi thế độc quyền (Alpha) thuộc về người thiết lập được BỘ EVALS CỦA RIÊNG MÌNH               |
|      (Company & Personal Evals) để biết chính xác model nào tối ưu cho công việc nào.              |
|                                                                                                    |
+────────────────────────────────────────────────────────────────────────────────────────────────────+
```

### Nghịch lý của sự kỳ vọng (Anticipation Alpha) [06:00]:
Guillermo Rauch chia sẻ một bài tập quan trọng: Hãy luôn duy trì một danh sách các bài toán mà các mô hình AI hiện tại **hoàn toàn bó tay**.
* Khi một model mới xuất hiện (như Opus 5.5 hay Jev), những người dẫn đầu không ngồi chờ đọc hướng dẫn trên mạng, mà ngay lập tức nạp danh sách "những điều bất khả thi" đó vào model mới để khám phá không gian tiềm ẩn (Latent space).
* Ai phát hiện ra khả năng mới sớm nhất sẽ là người dựng nên doanh nghiệp mới đón đầu làn sóng trước toàn bộ thị trường.

---

## 3. Kiến Trúc Backend Agent-First & Tự Động Hóa Vòng Đời Triển Khai (Deployments)

Một điểm nghẽn lớn nhất của các kỹ sư làm việc với AI: Code frontend thì nhanh, nhưng đến tầng Backend (Database, Authentication, Storage, Migrations) thì thường xuyên bị vấp [04:50].
* **Chuẩn hóa Backend Agent-First với Supabase / Serverless:**
  * Việc sử dụng các đầu nối MCP chính thức (Official Connectors) cho phép Agent trực tiếp tạo bảng, phân quyền Row Level Security (RLS) và chạy migrations tự động mà không cần can thiệp thủ công.
* **Preview Deployments — Nền tảng xác thực cho Agent:**
  * Tại Vercel, mỗi nhánh (branch) do Agent tạo ra lập tức được gán một URL Preview độc lập.
  * Agent tự động mở URL Preview này trên trình duyệt headless để tự chụp ảnh màn hình, tự chạy end-to-end tests kiểm chứng kết quả trước khi gõ cửa nhờ con người review.

---

## Actionable Takeaways Cho Lập Trình Viên Hiện Đại:
1. **Dịch chuyển tư duy lên tầng Orchestrator:** Dừng việc tự hào vì nhớ cú pháp code; hãy rèn luyện năng lực cấu hình Agent Harness, viết Evals và quản trị rào chắn an ninh.
2. **Xây dựng Sandbox cho AI:** Không bao giờ cho phép AI agent chạy tự do trên máy tính cá nhân nếu không có lớp cách ly workspace hoặc container an toàn.
3. **Đo lường bằng Evals, không đo bằng Vibes:** Khi nghe nói model A giỏi hơn model B, hãy tự viết một bộ benchmark nhỏ phản ánh đúng bài toán của dự án để kiểm chứng.
4. **Tập trung vào Taste và Storytelling:** Trau chuốt trải nghiệm người dùng, thiết kế giao diện có chiều sâu nghệ thuật vì đây là thứ duy nhất AI không thể tự quyết định thay con người.
