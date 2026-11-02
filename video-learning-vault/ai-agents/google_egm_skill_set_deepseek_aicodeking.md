# Mô Hình Voi & Cá Vàng (Google EGM): Nghệ Thuật Phản Biện Đối Kháng Thiết Kế Phần Mềm Với AI | AICodeKing

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=jyz4ALAahdE)
- **Video ID:** `jyz4ALAahdE`
- **Category:** #ai-agents
- **Date Processed:** 2026-10-20
- **Duration:** 12:05
- **Speaker:** AICodeKing
- **Transcript File:** [google_egm_skill_set_deepseek_aicodeking_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/google_egm_skill_set_deepseek_aicodeking_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Google đã chính thức phát hành mã nguồn mở bộ kỹ năng và plugin mang tên **Elephant-Goldfish Model (EGM)** (`google/elephant-goldfish-model`) theo giấy phép Apache 2.0. Được hàng ngàn kỹ sư Google sử dụng nội bộ, EGM giải quyết một trong những nguyên nhân hàng đầu khiến các dự án AI coding thất bại thảm hại: **Lập trình vội vã khi thiết kế chưa được thẩm định độc lập**.

Lấy cảm hứng từ bài luận kinh điển *"Elephants, Goldfish, and the new golden age of software engineering"* của Dave Rensin, EGM chia tách vòng đời tiền phát triển (pre-implementation) thành hai vai trò đối lập triệt để:
1. **The Elephant (Chú Voi - Trí nhớ siêu việt):** Tác nhân đồng hành cùng con người trong suốt nhiều giờ, ghi nhớ toàn bộ yêu cầu, bối cảnh dự án, các ràng buộc kỹ thuật và tiến hành soạn thảo tài liệu thiết kế (`design document`).
2. **The Goldfish (Cá Vàng - Não 3 giây / Không ký ức):** Các sub-agent hoàn toàn mới (**Fresh Sub-agents**) được khởi tạo với **ngữ cảnh bằng 0**. Chúng chỉ nhận duy nhất bản tài liệu thiết kế và các file mã nguồn liên quan để soi xét với tư cách một độc giả khách quan, hoài nghi và phản biện gay gắt.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                       GOOGLE ELEPHANT-GOLDFISH MODEL (EGM) ARCHITECTURE                     │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  THE ELEPHANT (VOI - TRÍ NHỚ DÀI HẠN)                                                       │
│  - Mô hình: Kimi K3.1 Pro / Claude Opus (Context dài, hiểu sâu bối cảnh)                    │
│  - Nhiệm vụ: Hội thoại với kỹ sư, nắm bắt requirements, sinh concepts_brief.md & draft.md   │
│         │                                                                                   │
│         ▼ Xuất bản Bản Thảo Thiết Kế (Design Doc Draft)                                     │
│  ┌───────────────────────────────────────────────────────────────────────────────────────┐  │
│  │ BỨC TƯỜNG CÁCH LY NGỮ CẢNH TUYỆT ĐỐI (CONTEXT FIREWALL)                               │  │
│  │ *Cấm mang theo lịch sử hội thoại! Không có ký ức ngầm! Không có giả định chung!       │  │
│  └───────────────────────────────────┬───────────────────────────────────────────────────┘  │
│                                      │                                                      │
│                                      ▼ Kích hoạt bầy Goldfish song song                     │
│  THE GOLDFISH SWARM (CÁ VÀNG - PHẢN BIỆN ĐỐI KHÁNG KHÔNG KÝ ỨC)                             │
│  - Mô hình đa dạng: GLM 5.3 (Kiến trúc & Code), DeepSeek V4.1 Flash (Thuật ngữ & Chi phí)   │
│                                                                                             │
│  ┌────────────────────┐  ┌────────────────────┐  ┌────────────────────┐  ┌────────────────┐ │
│  │ 1. Zoom Out Pass 1 │  │ 2. Comprehension   │  │ 3. Hostile Critic  │  │ 4. Readiness   │ │
│  │ Kiến trúc sư hoài  │  │ Bắt bẻ thuật ngữ,  │  │ Tấn công mở rộng,  │  │ Tech Lead thực │ │
│  │ nghi, cắt gọt      │  │ viết tắt không rõ  │  │ bảo mật, failure   │  │ tế, cấm mơ hồ  │ │
│  │ over-engineering   │  │ ràng (Naive reader)│  │ modes đối kháng    │  │ implementation │ │
│  └─────────┬──────────┘  └─────────┬──────────┘  └─────────┬──────────┘  └───────┬────────┘ │
│            │                       │                       │                     │          │
│            └───────────────────────┼───────────────────────┴─────────────────────┘          │
│                                    ▼                                                        │
│  ┌───────────────────────────────────────────────────────────────────────────────────────┐  │
│  │ HUMAN-IN-THE-LOOP RESOLUTION (/egm:egm resolve)                                       │  │
│  │ - Đánh giá phản biện (Tiếp thu điểm chí mạng, bỏ qua điểm thừa)                       │  │
│  │ - Phê duyệt thiết kế hoàn chỉnh TRƯỚC KHI cho Agent viết dòng code đầu tiên           │  │
│  └───────────────────────────────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

Điểm chấn động trong ngành: Dù là kho lưu trữ chính thức của Google, EGM hỗ trợ hạng nhất (first-class) không chỉ cho **Google Antigravity** (`ag plugin install`) mà còn cho **Claude Code** (`claude plugin install egm@egm`), **OpenAI Codex**, và **Cline/Pandode**. Điều này chứng minh rằng **chuẩn hóa quy trình (Harness / Skills) đang vượt lên trên sự độc quyền của mô hình và nền tảng**.

---

## 2. Vì Sao Cần EGM? Căn Bệnh "Điểm Mù Đồng Thuận" (Shared Blind Spots)

Khi bạn ngồi chat với một AI Agent trong 1 đến 2 tiếng về một tính năng phức tạp:
1. **Sự hình thành giả định ngầm:** Cả bạn và AI đều vô thức tích lũy một lượng lớn các giả định chung không được viết thành văn bản.
2. **Cảm giác an tâm giả tạo:** Khi Agent viết ra tài liệu thiết kế, bạn đọc thấy rất mượt mà và Agent cũng thấy hoàn hảo — bởi vì cả hai bên đều đã "ngầm hiểu" những phần bị thiếu trong đầu.
3. **Thảm họa khi triển khai:** Khi bắt tay vào viết code, các lỗ hổng bắt đầu phát nổ: *"Webhooks này retry theo cơ chế nào? Kiểu dữ liệu này lưu ở bảng nào? Khi mạng rớt thì fallback ra sao?"*. Những câu hỏi này bị bỏ quên vì không có ai phản biện tài liệu.

**Giải pháp của EGM:** Đưa tài liệu thiết kế cho một **chú Cá Vàng (Goldfish)** — một agent mới toanh, không hề tham gia cuộc trò chuyện trước đó, không biết các thỏa thuận ngầm, chỉ đọc đúng những gì được viết ra giấy. Nếu tài liệu thiếu sót, Goldfish sẽ lập tức chất vấn và chỉ ra ngay lỗ hổng kiến trúc.

---

## 3. Quy Trình 5 Bước Của EGM & Bộ 5 Tầng Phản Biện Đối Kháng

Quy trình làm việc chuẩn hóa của EGM bao gồm các lệnh điều phối:

### 3.1. Bước 1: Động Não Đa Chiều (`/egm:egm brainstorm`)
- Kỹ sư đưa ra ý tưởng sơ khởi (ví dụ: *"Thêm tính năng gửi webhook vào dự án"*).
- Agent Elephant đặt câu hỏi làm rõ phạm vi.
- Hệ thống điều phối **6 Goldfish độc lập** phân tích ý tưởng dưới nhiều góc nhìn:
  - *Kẻ Phản Biện (Contrarian):* Hỏi thẳng tại sao tính năng này cần tồn tại? Có thư viện mã nguồn mở nào đã giải quyết 90% việc này chưa?
  - *Chuyên gia Kinh Tế Học Token & Hạ Tầng:* Đo lường chi phí vận hành, quota API, tải trọng hệ thống.
- Đầu ra: Bản tóm tắt thống nhất hướng đi (`concepts_brief.md`).

### 3.2. Bước 2: Soạn Thảo Từng Phân Đoạn (`/egm:egm draft`)
- Biến bản tóm tắt thành tài liệu thiết kế kỹ thuật hoàn chỉnh.
- **Quy tắc vàng:** Không ném ra một tài liệu dài lê thê trong một lần sinh. Agent viết **từng phần một (section-by-section)**, dừng lại sau mỗi phần để kỹ sư đọc, chỉnh sửa và phản hồi trực tiếp.

### 3.3. Bước 3: Đại Phẫu Thẩm Định 5 Tầng (`/egm:egm review`)
Đây là trái tim của EGM, kích hoạt 5 tầng phản biện chuyên sâu:
1. **Zoom Out Review 1 (Kiến Trúc Sư Hoài Nghi):** Rà soát các chi tiết thừa thãi, phát hiện bẫy over-engineering và đơn giản hóa thiết kế.
2. **Comprehension Review (Độc Giả Ngây Thơ):** Đóng vai một người mới vào nghề, gắn cờ (flag) mọi thuật ngữ chuyên môn, từ viết tắt hoặc khái niệm trừu tượng chưa được định nghĩa rõ ràng.
3. **Hostile Design Critic (Kẻ Chỉ Trích Hiếu Chiến):** Tấn công trực diện vào khả năng mở rộng (scalability), điểm nghẽn chịu tải, bảo mật, và mọi kịch bản lỗi có thể xảy ra (`failure modes`).
4. **Zoom Out Review 2:** Đánh giá tổng thể tính mạch lạc của toàn bộ hệ thống sau các đợt công kích.
5. **Implementation Readiness Check (Tech Lead Thực Tế):** Đảm bảo tài liệu chi tiết đến mức lập trình viên (hoặc AI coding agent) khi nhận việc **không còn bất kỳ quyết định kiến trúc mơ hồ nào phải tự suy đoán**.

### 3.4. Bước 4 & 5: Hòa Giải (`/egm:egm resolve`) & Bắt Đầu Viết Mã
- Kỹ sư cùng Agent duyệt qua danh sách các phát hiện của Goldfish. Kỹ sư có toàn quyền: tiếp thu điểm chí mạng, bỏ qua điểm không phù hợp.
- Cập nhật tài liệu thiết kế lần cuối trước khi giao mã nguồn cho Builder Agent thực thi.

---

## 4. Kỷ Luật Phản Biện Thép: Hai Nguyên Tắc Bất Khả Xâm Phạm

### 4.1. Cấm Tuyệt Đối Thói Nịnh Bợ (Ban Flattery Outright)
Trong hệ thống prompt của EGM, các chuyên gia đánh giá bị cấm hoàn toàn việc khen ngợi người dùng:
- Tuyệt đối không được dùng các từ như *"Great idea"*, *"Brilliant design"*, hay *"Excellent approach"*.
- Bắt buộc phải **mở đầu bằng những điểm sai, điểm yếu hoặc rủi ro tiềm ẩn**.
- Bắt buộc phải đưa ra **luận điểm phản bác mạnh mẽ nhất (strongest counter-argument)** đối với bất kỳ giải pháp nào được đề xuất.

### 4.2. Nguyên Tắc Cách Ly Ngữ Cảnh Bắt Buộc (Hard Sub-agent Rule)
Nếu môi trường lập trình của bạn (harness) không thể sinh ra các sub-agent hoàn toàn cô lập, **EGM sẽ chủ động dừng quy trình review**:
- EGM kiên quyết **không bao giờ giả lập review trong cùng một phiên chat**, vì review trong cùng context sẽ biến Goldfish thành Elephant (bị nhiễm toàn bộ giả định ngầm).
- Mỗi Goldfish nhận quyền truy cập công cụ chỉ đọc (**read-only tools**) và không gian nhớ độc lập tuyệt đối.

---

## 5. Kinh Tế Học Token & Công Thức Phối Hợp Đa Mô Hình (Cline Pass $9.99/tháng)

### 5.1. Nỗi Lo "Cháy Ví" Khi Vận Hành Bầy Goldfish
Một quy trình review của EGM khởi chạy từ 5 đến 6 sub-agents đồng thời. Mỗi sub-agent phải đọc lại toàn bộ tài liệu thiết kế và các file mã nguồn liên quan từ đầu.
- Nếu chạy trên gói trả phí thông thường của Claude hoặc Codex ($20/tháng), bạn sẽ nhanh chóng chạm ngưỡng giới hạn tốc độ (rate limits) hoặc tốn hàng chục USD tiền API.

### 5.2. Giải Pháp Cline Pass ($9.99/tháng) & Chiến Lược Phối Hợp Đa Mô Hình
AICodeKing đề xuất công thức tối ưu hóa chi phí và triệt tiêu định kiến mô hình bằng gói **Cline Pass**:

| Vai Trò Trong EGM | Mô Hình Khuyên Dùng | Lý Do Lựa Chọn & Ưu Điểm Kỹ Thuật |
| :--- | :--- | :--- |
| **The Elephant (Voi Soạn Thảo)** | **Kimi K3.1 Pro** | Cực mạnh trong việc duy trì ngữ cảnh dài, đàm thoại chi tiết và soạn thảo tài liệu mạch lạc. |
| **Goldfish Hiếu Chiến & Readiness** | **GLM 5.3** | Năng lực đọc mã nguồn thực tế và tư duy phản biện đối kháng vượt trội. Khác họ mô hình với Kimi giúp **triệt tiêu hoàn toàn điểm mù chung**. |
| **Goldfish Rà Soát Thuật Ngữ & Chi Phí** | **DeepSeek V4.1 Flash** hoặc **MiniMax M3.1 Flash** | Tốc độ phản hồi cực nhanh, chi phí tiệm cận 0, hoàn hảo cho các tác vụ kiểm tra từ ngữ và logic bề mặt. |

> [!TIP]
> Việc sử dụng **hai họ mô hình khác nhau** cho Elephant (Kimi) và Goldfish (GLM / DeepSeek) mang lại lợi thế vượt bậc: Chúng không chỉ khác nhau về ký ức mà còn khác nhau về tập dữ liệu huấn luyện và thói quen suy luận, giúp phát hiện ra những sai sót mà các mô hình cùng họ thường bỏ qua cho nhau.

---

## 6. Hướng Dẫn Cài Đặt Trên Các Nền Tảng

### 6.1. Cài đặt trên Google Antigravity
Chạy lệnh trực tiếp trong Antigravity CLI:
```bash
ag plugin install https://github.com/google/elephant-goldfish-model
```

### 6.2. Cài đặt trên Claude Code
```bash
# Bước 1: Thêm marketplace của Google
claude plugin marketplace add google/elephantgoldfishmodel

# Bước 2: Cài đặt plugin EGM
claude plugin install egm@egm
```

### 6.3. Cài đặt trên OpenAI Codex
```bash
codex plugin marketplace add google/elephantgoldfishmodel
codex plugin add egm@egm
```

### 6.4. Cài đặt trên Cline / Pandode
1. Clone toàn bộ repo `https://github.com/google/elephant-goldfish-model` (Lưu ý: Không chỉ copy thư mục `skills/` vì hệ thống nạp động các file persona và tiêu chuẩn tài liệu khi chạy).
2. Trỏ Cline vào file kỹ năng mong muốn và cấu hình cho phép khởi chạy fresh sub-agents.
3. Kích hoạt Cline Pass để sử dụng không giới hạn các mô hình Kimi K3.1 Pro, GLM 5.3 và DeepSeek Flash.

---

## 7. Hai Kỹ Năng Bổ Trợ Giá Trị Cao Trong EGM

- `/egm:egm bootstrap`: Quét toàn bộ codebase hiện có từ dưới lên trên (bottom-up), tự động tạo các file `README.md` tóm tắt cho từng thư mục con rồi tổng hợp lên thư mục gốc. Đây là cách nhanh nhất để cung cấp một "bản đồ kiến trúc hoàn chỉnh" cho chú Voi khi tiếp nhận dự án cũ (brownfield).
- `/egm:egm drift-report`: So sánh tài liệu thiết kế đã duyệt với mã nguồn thực tế đang chạy và cảnh báo những điểm bị sai lệch (drift). Khuyến nghị chạy định kỳ mỗi tuần một lần để ngăn chặn tình trạng tài liệu bị "chết" ngay sau khi merge code.

---

## 8. Bài Học Hành Động Dành Cho Kỹ Sư (Actionable Takeaways)

1. **Đầu Tư Vào Giai Đoạn Tiền Lập Trình (Pre-Code Phase):** Đối với bất kỳ tính năng phức tạp nào, đừng bao giờ để AI gõ mã ngay. Hãy buộc nó đi qua chu trình Brainstorm ➔ Draft ➔ Goldfish Review.
2. **Loại Bỏ Thói Quen Nhận Lời Khen Ảo Từ AI:** Cài đặt prompt nghiêm ngặt cấm AI khen ngợi kế hoạch của bạn. Buộc nó phải đóng vai kẻ chỉ trích cay nghiệt nhất để tìm ra lỗ hổng trước khi người dùng thực tế tìm thấy.
3. **Phản Biện Bằng Đa Mô Hình (Cross-Model Review):** Luôn dùng một mô hình khác (ví dụ GLM hoặc DeepSeek) để review tài liệu do mô hình chính (Kimi hoặc Claude) soạn thảo.
4. **Không Dùng EGM Cho Việc Sửa Bug Nhỏ:** Đối với các lỗi hiển nhiên hoặc thay đổi giao diện 1-2 dòng, hãy dùng quy trình thông thường để tiết kiệm token và thời gian. EGM sinh ra để bảo vệ các quyết định kiến trúc lớn.
