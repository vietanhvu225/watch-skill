# How We Solved Agent Building | Andrew Qu, Vercel

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=9dYcwOkpCE8)
- **Watch Skill ID:** `cfac3ef3ae9fd926`
- **Category:** #ai-agents
- **Date Processed:** 2026-09-16
- **Duration:** 17:34
- **Speaker:** Andrew Qu (Chief of Software, Vercel)

---

## 1. Tầm nhìn: "An Agent on Every Desk"

Lấy cảm hứng từ câu nói nổi tiếng của Bill Gates năm 1980 (*"A computer on every desk and in every home"*), Andrew Qu và CTO của Vercel đặt mục tiêu đưa **AI Agent lên mọi bàn làm việc** trong doanh nghiệp, mở rộng từ lập trình kỹ thuật sang các phòng ban nghiệp vụ: Sales, Marketing, Legal, và Data Science.

### Bài toán thực tế tại Vercel (The Data Science Bottleneck):
- Vercel tăng trưởng nóng kéo theo lượng dữ liệu khách hàng khổng lồ trên Snowflake.
- Đội ngũ Data Science tinh gọn liên tục bị gián đoạn công việc cốt lõi do phải trả lời hàng trăm câu hỏi ad-hoc từ Sales/Marketing bằng cách viết query thủ công.
- Mục tiêu: Xây dựng một Agent tự động hóa toàn diện quy trình truy vấn dữ liệu từ ngôn ngữ tự nhiên (Internal Data Agent tên là **D0**).

---

## 2. Hành trình 4 bước tiến hóa kiến trúc Agent tại Vercel

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    TIẾN HÓA KIẾN TRÚC AGENT TẠI VERCEL                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. The Mega-Prompt (Prompt duy nhất)                                       │
│     - Ném toàn bộ Snowflake schema vào System Prompt.                       │
│     - Kết quả: Thất bại do ảo giác, cú pháp SQL sai, không kiểm soát được.  │
│                                │                                            │
│                                ▼                                            │
│  2. Chained Multi-Agent Pipeline (Chuỗi Agent chuyên biệt)                  │
│     - Query Agent ➔ Planning Agent ➔ SQL Agent ➔ Exec ➔ Reporting           │
│     - Kết quả: "Leaky context", đứt gãy thông tin giữa các bước chuyển giao.│
│       Nếu SQL lỗi, agent không thể tự quay lại (backtrack) để sửa sai.      │
│                                │                                            │
│                                ▼                                            │
│  3. Single Stateful Agent (Một Agent lớn tự quản lý trạng thái)            │
│     - Cho phép phản tư (reflection), retry và quay lại các bước trước.      │
│     - Kết quả: Chỉ đạt 30% benchmark; chi phí bảo trì luật cứng quá lớn.   │
│                                │                                            │
│                                ▼                                            │
│  4. File System Agent & Sandbox Paradigm (BƯỚC NGOẶT ĐỘT PHÁ)              │
│     - Học tập từ Claude Code & Opus 4.5.                                    │
│     - Đưa toàn bộ Semantic Layer vào một Sandbox File System.               │
│     - Agent dùng các công cụ tự nhiên: `bash`, `read`, `write`, `grep`.     │
│     - Kết quả: Điểm Eval NGAY LẬP TỨC TĂNG GẤP ĐÔI!                          │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Bước ngoặt đột phá: "File System is All You Need"

Khi phân tích lý do **Claude Code** hoạt động vượt trội hơn mọi agent nội bộ tự viết, Vercel nhận ra một quy luật cốt lõi:
- **Mô hình được huấn luyện tốt nhất trên hệ điều hành và file system:** LLM rất giỏi dùng `bash`, `grep`, đọc/ghi file và điều hướng thư mục.
- **Không trói buộc model bằng custom tools cứng nhắc:** Thay vì viết hàng chục function-calling phức tạp (`search_schemas`, `read_entity_yaml`), hãy dump toàn bộ tài liệu ngữ nghĩa (semantic layer), định nghĩa schema và quy tắc vào cây thư mục trong Sandbox.
- Để Agent tự do khám phá, tìm kiếm bằng grep/bash và sinh mã SQL thực thi an toàn trong môi trường cô lập.

### Cải tiến tiếp theo: Thư mục `skills/` & nền tảng `skillsh`:
- Vercel nhận thấy hàng nghìn câu hỏi mỗi ngày đều quy về các mẫu hình truy vấn tương tự nhau (tổng hợp doanh thu, tra cứu thông tin khách hàng, số lượt tải npm).
- Họ thiết lập một Cron Job tự động chắt lọc các câu hỏi phổ biến thành các **Skills** lưu trong thư mục `skills/` (~100 skills). Khi agent khởi động, nó nạp sẵn các tri thức này thay vì phải dò dẫm từ con số 0.
- Ra mắt công cụ **`skillsh`** (trung tâm chia sẻ và cài đặt Agent Skills cho cộng đồng).

---

## 4. Eve (`eve.dev`): "Next.js for Agents"

Từ kinh nghiệm xây dựng Next.js (quy ước cấu trúc thư mục định nghĩa hạ tầng - Framework Defined Infrastructure), Vercel chính thức phát hành **Eve** – khung làm việc nguồn mở cho Agent:

```
my-agent/
├── instructions/    # Các chỉ dẫn hệ thống & ngữ cảnh doanh nghiệp
├── skills/          # Các kỹ năng nghiệp vụ chuyên biệt (.md / scripts)
├── tools/           # Công cụ mở rộng tùy biến
└── channels/        # Cổng kết nối (Slack, Web, API)
```

### Điểm nổi bật của Eve:
1. **Kiến trúc phân tầng chuẩn hóa:** Tách biệt rõ ràng giữa **Runtime** (độ bền vững, môi trường cô lập Sandbox, đa mô hình) và **Channels** (giao tiếp người dùng).
2. **Hỗ trợ cởi mở (Open Source) & Triển khai tối ưu trên Vercel:**
   - Hỗ trợ adapter cắm ngoài: Postgres, Docker, OpenAI Responses API.
   - Khi deploy trên Vercel: Tự động tích hợp *Vercel Workflows* (độ bền vững/resumability), *Vercel Sandbox* (thực thi an toàn), và *Vercel Connect* (sinh OIDC tokens bảo mật).
3. **Observability Out-of-the-Box:** Theo dõi trực quan toàn bộ lượt chạy (agent runs), các lượt gọi tool, từng bước suy luận và ước tính chi phí token.

---

## 5. Kết quả thực tiễn & Bài học doanh nghiệp

1. **Hiệu quả thực tế tại Vercel:**
   - Hiện có hơn 20 Agent vận hành thực tế (PMF nội bộ): Tự động đánh giá Marketing retros, hỗ trợ Legal rà soát và gạch đỏ hợp đồng (redline contracts), và trợ lý Data Science tự động.
   - Đội ngũ Data Science giải phóng hoàn toàn thời gian gõ query thủ công, tập trung tối ưu hóa hiệu năng Snowflake và tích hợp nguồn dữ liệu mới.
2. **Sức mạnh của Tri thức doanh nghiệp (Company-Specific Knowledge):**
   - Các giải pháp Agent đóng gói sẵn trên thị trường (Off-the-shelf) thường thất bại vì thiếu ngữ cảnh sâu về nghiệp vụ riêng của từng công ty.
   - Việc tự xây dựng Agent trên nền tảng như Eve và nạp đầy đủ tri thức doanh nghiệp mang lại giá trị cao gấp nhiều lần so với mua giải pháp đóng hộp.

---

## 🔗 Liên kết & Thẻ
- **Chủ đề liên quan:**
  - [Harness Engineering is Not Enough: Why Software Factories Fail](file:///f:/source/watch-skill/video-learning-vault/ai-agents/why_software_factories_fail.md)
  - [Google OKF: Enterprise Context Folder](file:///f:/source/watch-skill/video-learning-vault/ai-agents/google_okf.md)
  - [Why We Killed Our Multi-Agent Pipeline](file:///f:/source/watch-skill/video-learning-vault/ai-agents/why_we_killed_multi_agent_pipeline.md)
- **Tags:** `#ai-agents` `#vercel` `#eve` `#file-system-agents` `#skills` `#data-science` `#nextjs`
