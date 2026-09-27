# How to Run 100+ Claude Agents at Minimum Cost — Lydia Hallie (Anthropic)

- **Video URL:** [Facebook Reel](https://www.facebook.com/reel/1764176427829852)
- **Watch Skill ID:** `df84a40038087d90`
- **Diễn giả:** Lydia Hallie — Member of Technical Staff tại Anthropic (Claude Code Team)
- **Category:** #ai-agents, #token-economics, #claude-code, #cost-optimization, #engineering-practices
- **Date Processed:** 2026-09-28
- **Duration:** 26:35 (1595 giây)
- **Transcript File:** [claude_code_low_cost_100_agents_lydia_hallie_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/claude_code_low_cost_100_agents_lydia_hallie_transcript.txt)

---

## 1. Bối cảnh: Sự Chuyển dịch Mô hình Kinh tế Phần mềm thời AI Agents

Mở đầu buổi hội thảo chuyên sâu dành riêng cho bài toán chi phí, Lydia Hallie nhấn mạnh một bước ngoặt căn bản trong ngành kỹ nghệ phần mềm:
- **Kỷ nguyên phần mềm cũ (SaaS truyền thống):** Trả phí cố định theo số lượng người dùng (Flat fee per seat / subscription). Chi phí phần mềm được cố định hàng tháng dù bạn gõ 10 dòng hay 10,000 dòng code.
- **Kỷ nguyên Agentic Coding:** **Trả phí trực tiếp theo khối lượng công việc thực tế (Pay-per-work, task-by-task, prompt-by-prompt)**.
- **Kỹ năng sống còn mới của Software Engineer:** Quản trị token và tối ưu hóa chi phí vận hành Agentic Loops giờ đây là một **kỹ năng kỹ thuật cốt lõi (Core Engineering Competency)**, tương tự như việc tối ưu truy vấn Database hay thuật toán trước đây.

> *"99% người dùng chỉ dùng Claude Code như một phiên bản Google nâng cao... nhưng chỉ có 1% là những người vận hành cả một đội ngũ Agent tự học và tự phát triển liên tục với hơn 100 Agent chạy trong cùng một vòng lặp."*

---

## 2. Vật lý của Token: 3 Loại Token & Cơ Chế Định Giá

Để tối ưu chi phí, kỹ sư bắt buộc phải hiểu cơ chế phần cứng (GPU) xử lý từng loại token:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                            VẬT LÝ TOKEN & BẢN CHẤT CHI PHÍ PHẦN CỨNG                        │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  1. Input Tokens (RẺ):                                                                      │
│     ├── GPU xử lý song song toàn bộ batch prompt trong đúng MỘT lượt (Single Forward Pass). │
│     └── Rất tối ưu cho kiến trúc Tensor Core -> Đơn giá rẻ.                                 │
│                                                                                             │
│  2. Output Tokens (ĐẮT NHẤT - GẤP 3 ĐẾN 5 LẦN INPUT):                                       │
│     ├── Mô hình sinh từng token một tuần tự (Autoregressive Generation).                    │
│     └── Để sinh được 1 từ, GPU phải duyệt qua TOÀN BỘ ngữ cảnh trước đó.                    │
│         -> Tốn tài nguyên tính toán GPU khủng khiếp -> Đơn giá cao nhất.                   │
│                                                                                             │
│  3. Cached Tokens (RẺ NHẤT - CHỈ BẰNG 10% - 20% INPUT THƯỜNG):                              │
│     ├── Giữ nguyên trạng thái KV-Cache của phần tiền tố (Prefix) giống hệt từ request trước.│
│     └── GPU chỉ việc đọc lại từ bộ nhớ đệm, KHÔNG CẦN TÍNH TOÁN LẠI!                        │
│                                                                                             │
│  ==> MỤC TIÊU CỐT LÕI: Giữ cho Prompt Cache chiếm > 80% - 90% tổng lượng Input Tokens!      │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

Trong một phiên làm việc hiệu quả (Healthy Session), lệnh `/usage` phải cho thấy phần lớn input token nằm trong thùng chứa `Prompt Cache`.

---

## 3. "Prompt Cache Prefix Physics": Những Lỗi Phá Vỡ Cache Phổ Biến

Prompt Cache hoạt động dựa trên nguyên lý **Tiền tố Khớp Tuyệt đối (Identical Prefix Matching)** tính từ token đầu tiên (`Token 0`). Chỉ cần một token ở phần đầu bị sai lệch, toàn bộ phần ngữ cảnh phía sau sẽ bị coi là mới và GPU sẽ tính tiền toàn bộ theo đơn giá Input đầy đủ!

```
Request 1: [System Prompt] [CLAUDE.md] [Tools] [History Turn 1] [New Message]
           └────────────────── Cached Prefix ──────────────────┘ └─ New Input ─┘

Request 2: [System Prompt] [CLAUDE.md] [Tools] [History Turn 1] [History Turn 2] [New Message]
           └────────────────────────── Cached Prefix ──────────────────────────┘ └─ New Input ─┘
```

### 4 "Tội đồ" Phá Vỡ Prompt Cache Thường Gặp:

1. **Đổi Model giữa phiên làm việc (Model Switching Mid-Session):**
   - Đổi từ Sonnet sang Opus hoặc Haiku ngay giữa cuộc hội thoại.
   - Do mỗi model có kiến trúc tensor và KV-Cache nội bộ riêng biệt, model mới không thể tái sử dụng cache của model cũ. Toàn bộ lịch sử hội thoại sẽ bị đọc lại từ đầu với giá 100% input token!
2. **Đổi Effort Level giữa chừng (Effort Level Switching):**
   - Trừ model Fable 5.1, với các model khác, Effort Level được nhúng trực tiếp vào header/prefix của request. Việc đổi effort giữa chừng làm thay đổi prefix và thổi bay cache.
3. **Bật/Tắt MCP Servers giữa chừng:**
   - Khiến toàn bộ định nghĩa công cụ (Tool Definitions) ở phần đầu request bị thay đổi.
4. **Hết hạn Cache TTL (5 Phút vs. 1 Giờ):**
   - Trên **API Billing / Cloud Providers**, thời gian sống của Prompt Cache mặc định chỉ là **5 PHÚT**.
   - Nếu bạn tạm dừng công việc 6 phút để đi lấy cà phê hoặc họp nhanh rồi quay lại gõ tiếp, cache đã "nguội" (expired). Lượt prompt tiếp theo sẽ tính tiền lại toàn bộ lịch sử!
   - Trên tài khoản gói Pro/Team/Enterprise, cửa sổ này là **1 giờ**.

---

## 4. Kỹ Thuật Nén & Bảo Trì Ngữ Cảnh: `/compact`, `/clear`, và Subagents

Nguyên lý bất biến của LLM: **Mô hình chỉ biết cộng thêm vào ngữ cảnh, không bao giờ tự động xóa bớt.**

```
Phiên làm việc Không Clear (Chi phí tăng lũy tiến):
Task 1: [Context Task 1] ───────────────────────────────────────────► (1x Cost)
Task 2: [Context Task 1] + [Context Task 2] ────────────────────────► (2x Cost)
Task 3: [Context Task 1] + [Context Task 2] + [Context Task 3] ─────► (3x Cost)

Phiên làm việc Chuẩn (Có Quản trị):
Task 1: [Context Task 1] ──► /rename Task1 ──► /clear
Task 2: [Context Task 2] ──► /clear
Task 3: [Context Task 3] ──► Giữ chi phí luôn ở mức tối thiểu!
```

### Bí quyết Vàng khi dùng `/compact`:
- Lệnh `/compact` yêu cầu model đọc lại toàn bộ lịch sử để viết bản tóm tắt ngắn gọn.
- **Quy tắc:** Luôn chạy `/compact` **TRƯỚC KHI RỜI BÀN LÀM VIỆC**, khi cache vẫn còn "ấm" (Warm Cache).
- Nếu bạn đi ăn trưa về (sau 1 tiếng, cache đã chết) rồi mới gõ `/compact`, lệnh compact đó sẽ tốn toàn bộ chi phí đọc lại bằng đơn giá input đắt đỏ!

### Tắt tiếng Đầu ra Terminal (Quiet Test Runners):
- Mọi ký tự in ra màn hình terminal đều bị ghi vào Context Window vĩnh viễn.
- Một bộ test chạy in 400 dòng `PASS test/unit/...` sẽ khiến mỗi lượt prompt tiếp theo phải cõng 400 dòng vô nghĩa đó.
- **Giải pháp:** Cấu hình trong `CLAUDE.md` các lệnh test có cờ `--quiet` hoặc `reporter=dot` (chỉ in lỗi, không in danh sách test thành công).

---

## 5. Subagents: Cơ Chế Giảm Chi Phí & Cách Ly Nhiễu

Nhiều người nghĩ Subagent tốn kém hơn vì nó chạy thêm một agent nữa. Nhưng Lydia giải thích bài toán kinh tế ngược lại:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                       TẠI SAO SUBAGENTS LẠI TIẾT KIỆM TIỀN CHO DỰ ÁN?                       │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  Nhiệm vụ đào bới (Research & Digging):                                                     │
│  - Phải đọc 30 file mã nguồn, duyệt qua log build 1,000 dòng, grep toàn bộ repo.            │
│                                                                                             │
│  Nếu chạy trên Luồng Chính (Main Thread):                                                  │
│  └── 30 files + 1,000 dòng log sẽ nằm LÌ TRONG CONTEXT WINDOW của bạn cho đến hết phiên!    │
│      Mọi lượt prompt sau đó đều phải trả tiền mang vác đống rác này.                        │
│                                                                                             │
│  Nếu chuyển cho Subagent (Isolated Subagent):                                              │
│  ├── Subagent mở context riêng, đọc 30 file và lọc log.                                     │
│  └── Sau khi xong việc, nó CHỈ TRẢ VỀ 1 ĐOẠN TÓM TẮT 5 DÒNG cho Luồng Chính!                │
│  └── Toàn bộ 30 files và log rác BỊ HỦY BỎ hoàn toàn!                                      │
│                                                                                             │
│  ==> Luồng chính giữ được kích thước siêu nhỏ, các lượt tương tác sau đó rẻ hơn gấp nhiều lần! │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

- **Mẹo chọn model:** Có thể giao việc đào bới cho Subagent chạy **Claude Haiku** (cực nhanh, cực rẻ) thay vì dùng Opus!

---

## 6. Bộ Điều Khiển Doanh Nghiệp Tập Trung: `managed-settings.json`

Dành cho Tech Lead và Engineering Manager quản lý chi phí cho cả team, Anthropic cung cấp file cấu hình tập trung `managed-settings.json` đẩy từ Admin Console xuống máy toàn bộ kỹ sư (kỹ sư không thể ghi đè):

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                          BỘ CÔNG CỤ QUẢN TRỊ CHI PHÍ DOANH NGHIỆP                           │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  1. Khóa Model Mặc định (Default Model):                                                    │
│     └── Chặn việc dev vô tình để mặc định Claude Opus cho mọi tác vụ gõ code cơ bản.         │
│                                                                                             │
│  2. Khóa Effort Level tối đa (Max Effort Cap):                                              │
│     └── Giới hạn không cho phép kéo lên max effort bừa bãi.                                 │
│                                                                                             │
│  3. Cấu hình Prompt Cache TTL tập trung:                                                    │
│     └── Nếu team hay họp ngắt quãng, chỉnh TTL lên 1 giờ để tránh mất cache.                │
│                                                                                             │
│  4. Giám sát bằng OpenTelemetry (2 Biến môi trường):                                       │
│     ├── Tỷ lệ Cache Read Share: Phát hiện kỹ sư nào hay đổi model/effort làm vỡ cache.     │
│     └── Đồ thị Input Tokens over Time: Nhận diện ai đang duy trì "phiên làm việc bất tận"    │
│         mà không bao giờ chịu gõ /clear.                                                    │
│                                                                                             │
│  5. Auto-continue at usage limit:                                                           │
│     └── Tự động kích hoạt tiếp tác vụ chạy nền khi cửa sổ giới hạn 5 giờ được làm mới.     │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Sơ Đồ Quy Trình Tối Ưu Chi Phí Chuẩn Anthropic

```
                  ┌───────────────────────────────────────────────┐
                  │          BẮT ĐẦU PHIÊN LÀM VIỆC MỚI           │
                  │   Chọn cố định: Model + Effort + MCP servers  │
                  │        (KHÔNG THAY ĐỔI GIỮA CHỪNG)            │
                  └───────────────────────┬───────────────────────┘
                                          │
                  ┌───────────────────────▼───────────────────────┐
                  │             ĐƯA ĐÚNG NGỮ CẢNH                 │
                  │  - Gắn file đích bằng cú pháp `@file`         │
                  │  - Chạy lệnh test với cờ `--quiet`            │
                  │  - Chuyển tác vụ đặc thù từ CLAUDE.md sang    │
                  │    SKILL.md (chỉ tải on-demand)               │
                  └───────────────────────┬───────────────────────┘
                                          │
                  ┌───────────────────────▼───────────────────────┐
                  │    CẦN NGHIÊN CỨU SÂU / ĐỌC NHIỀU FILE?       │
                  └───────────────┬───────────────┬───────────────┘
                                  │               │
                                 YES              NO
                                  │               │
                  ┌───────────────▼─────────┐     │
                  │ BẮN SANG SUBAGENT       │     │
                  │ (Dùng Haiku / Sonnet)   │     │
                  │ Chỉ lấy kết luận về main│     │
                  └───────────────┬─────────┘     │
                                  │               │
                                  └───────┬───────┘
                                          │
                  ┌───────────────────────▼───────────────────────┐
                  │          KẾT THÚC TASK HOẶC TẠM DỪNG           │
                  │  - Đang xong task: /rename -> /clear          │
                  │  - Đi họp / nghỉ trưa: /compact TRƯỚC KHI ĐI  │
                  └───────────────────────────────────────────────┘
```

---

## 8. Bảng Đối Chiếu Quyết Định: Mô Hình Nào Cho Tác Vụ Nào?

| Mô hình | Định vị trong Team | Khi nào nên dùng? | Mức độ ngốn Token |
|---|---|---|---|
| **Claude Haiku** | Trợ lý / Thực tập sinh nhanh nhẹn | Đào bới thư mục, grep log, review format cú pháp, chạy subagent tìm kiếm | Siêu rẻ |
| **Claude Sonnet** | Kỹ sư tổng quát (Generalist) | 80% công việc phát triển tính năng, viết unit tests, sửa lỗi có mô tả rõ | Trung bình - Rất kinh tế |
| **Claude Opus** | Chuyên gia / Kiến trúc sư trưởng | Thiết kế hệ thống trong Plan Mode, giải quyết lỗi ngầm hóc búa, tái cấu trúc lớn | Đắt - Cần cân nhắc kỹ |
| **Fable 5.1** | Chuyên gia đặc nhiệm | Xử lý bài toán độc dị chưa từng gặp; Model duy nhất **không vỡ prompt cache khi đổi effort** | Chuyên biệt |

---

## 9. Ba Lời Khuyên Cốt Tử Tóm Gọn (The 3 Golden Rules)

1. **Cố định cấu hình từ đầu:** Chọn Model, Effort Level và MCP Servers ngay lúc khởi tạo phiên và để nguyên. Không đổi giữa chừng để bảo vệ Prompt Cache Prefix.
2. **Kỷ luật với những gì nạp vào Context:** Dùng `@file` chỉ điểm chính xác, tắt stdout ồn ào của test runner, và ném toàn bộ việc đọc nhiều file cho Subagent.
3. **Luôn giữ phiên làm việc tinh gọn:** Xóa (`/clear`) khi chuyển sang task mới. Nếu cần nghỉ ngơi, hãy compact (`/compact`) **ngay lúc cache còn ấm** thay vì đợi quay lại mới compact.
