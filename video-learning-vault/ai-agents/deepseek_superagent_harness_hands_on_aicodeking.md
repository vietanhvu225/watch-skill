# Thử Nghiệm Thực Chiến DeepSeek SuperAgent Harness: Bản Thiết Kế Đột Phá Hay Chỉ Là Lý Thuyết? | AICodeKing

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=ZIcvDE0HoWE)
- **Video ID:** `ZIcvDE0HoWE`
- **Category:** #ai-agents
- **Date Processed:** 2026-10-20
- **Duration:** 14:16
- **Speaker:** AICodeKing
- **Transcript File:** [deepseek_superagent_harness_hands_on_aicodeking_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_superagent_harness_hands_on_aicodeking_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Sau khi nhóm nghiên cứu DeepSeek Harness công bố tài liệu kiến trúc chuyên sâu *"Exploring Super-agent Design"* và mã nguồn mở nguyên mẫu `dsh-superagent-prototype`, **AICodeKing** đã tiến hành mổ xẻ thực nghiệm toàn diện hệ thống này. Thay vì các chiêu bài quảng cáo mô hình thông thường, công trình của DeepSeek giải quyết trực diện một "nỗi đau" nhức nhối mà mọi kỹ sư vận hành AI đều thấm thía: **Mô hình AI đã đủ giỏi để làm việc tự chủ, nhưng thứ đang làm chậm toàn bộ quy trình lại chính là... sự chú ý của con người**.

Não bộ con người chỉ có thể theo sát tối đa **3 đến 5 phiên chat đồng thời**. Vượt quá ngưỡng này, kỹ sư bắt đầu quên mất phiên nào đang nắm ngữ cảnh gì, các agent bị treo 20 phút để chờ bấm nút duyệt, hoặc hai agent giẫm chân lên nhau sửa cùng một Pull Request. Triết lý cốt lõi của DeepSeek là: **Ngừng biến "phiên làm việc" (session) thành thứ mà con người phải quản lý**. Kỹ sư chỉ giao tiếp với **duy nhất một tác nhân đại diện** trên các kênh quen thuộc (Telegram, Slack, Feishu, GitHub), trong khi hệ thống ngầm tự động tạo, điều phối và hủy bỏ hàng chục phiên làm việc biệt lập bên dưới.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                    DEEPSEEK SUPERAGENT: ARCHITECTURAL DECOUPLING                            │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  TRADITIONAL COLLAPSE (CON NGƯỜI LÀ ĐIỂM NGHẼN):                                            │
│  [Human Engineer] ──── Quản lý 5 cửa sổ chat ────> [Context Switching Fatigue / Lỗi chồng lấn]│
│                                                                                             │
│  DEEPSEEK 2-LAYER SUPERAGENT ARCHITECTURE:                                                  │
│                                                                                             │
│  [Human Engineer] (Telegram / Slack / GitHub / Feishu)                                      │
│         │                                                                                   │
│         ▼ < 100 từ, trả lời nhanh                                                           │
│  ┌───────────────────────────────────────────────────────────────────────────────────────┐  │
│  │ TẦNG 1: THE ROUTER (DeepSeek V4.1 Flash - Siêu Rẻ)                                    │  │
│  │ - KHÔNG CÓ BASH / EDIT TOOLS (Cấm cứng bằng Deny Rules để ép buộc ủy quyền)           │  │
│  │ - Tiếp nhận triggers (Chat, PR comments, CI events qua bộ lọc Code Filter)            │  │
│  │ - Quản lý ngữ cảnh như bộ đệm cache (Context as Cache & Chain of Recall)              │  │
│  └───────────────────────────────────┬───────────────────────────────────────────────────┘  │
│                                      │                                                      │
│                                      │ Điều phối tác vụ (bot_message_bot / Ticket)          │
│                                      ▼                                                      │
│  ┌───────────────────────────────────────────────────────────────────────────────────────┐  │
│  │ TẦNG 2: ISOLATED WORKERS (Mô hình Lập trình Cấp cao: Kimi K3.1 Pro / GLM 5.3)         │  │
│  │ - Worker A (Builder): Checkout Worktree riêng, thực thi TDD, im lặng lập trình        │  │
│  │ - Worker B (Reviewer): Phản biện đối kháng độc lập, phát hiện lỗi góc khuất           │  │
│  │ - Không bao giờ nói chuyện trực tiếp với con người (chỉ báo cáo về Router)            │  │
│  └───────────────────────────────────┬───────────────────────────────────────────────────┘  │
│                                      │                                                      │
│                                      ▼                                                      │
│  [KẾT QUẢ ĐẦU RA: Tự động mở Pull Request / Báo cáo ngắn gọn kèm link nghiệm thu]          │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

Điểm nhấn thực nghiệm nổi bật:
- **Phát hiện chấn động về ủy quyền:** Một mô hình với prompt thông thường *"hãy ủy quyền cho subagent nếu thấy phù hợp"* đã **0 lần gọi subagent trong cả 5 lần thử nghiệm**! Muốn AI ủy quyền, bắt buộc phải dùng **ràng buộc kiến trúc (Architectural Enforcement - tước bỏ công cụ shell của Router)** chứ không thể trông chờ vào prompt.
- **Bài toán kinh tế học Token Cache:** Một lần chạy Superagent ngốn tới **23.2 triệu cached tokens**. Trên Claude Sonnet 3.5, chi phí là **$4.64/run**, nhưng trên DeepSeek V4.1 Flash, chi phí chỉ vỏn vẹn **$0.07/run (rẻ hơn 66 lần)**. Kiến trúc Superagent chỉ khả thi về mặt thương mại trên nền tảng open-weight giá rẻ.
- **Công thức triển khai thực tế của AICodeKing:** Kết hợp **Pandode** (Desktop Multi-Agent) + **Cline Pass ($9.99/tháng)** + **DeepSeek Harness** để dựng hệ thống Superagent cá nhân chạy 24/7 ngay trong tuần.

---

## 2. Bốn Trụ Cột Cốt Lõi Của Một "Super-Agent" Chuẩn Mực

Theo định nghĩa trong tài liệu nghiên cứu của DeepSeek, một Superagent thực thụ phải thỏa mãn đồng thời 4 thuộc tính:

| Trụ Cột | Khái Niệm | Biểu Hiện Thực Tế Trong Quy Trình Kỹ Thuật |
| :--- | :--- | :--- |
| **1. Continuous (Tính Liền Mạch)** | Luôn tạo cảm giác là một thực thể duy nhất | Kỹ sư yêu cầu Kế hoạch A trên Telegram, sau đó chuyển sang Kế hoạch B trong bình luận GitHub PR; Agent hiểu rõ toàn bộ bối cảnh chuyển dịch mà không bị đứt đoạn. |
| **2. Productive (Tính Năng Suất)** | Tiếp nhận việc mới trước khi việc cũ hoàn thành | Nhận lệnh tiếp theo của kỹ sư khi tác vụ trước vẫn đang build/test ngầm, không bắt con người phải chờ đợi phiên làm việc nhàn rỗi. |
| **3. Proactive (Tính Chủ Động)** | Tự quan sát và hành động không cần nhắc | Tự động nhân bản lỗi người dùng vừa báo trong chat (`Silently Reproduce and Fix`), tạo PR sửa lỗi trước khi cuộc thảo luận kết thúc; nếu không tái hiện được thì im lặng không làm phiền. |
| **4. Persistent (Tính Bền Vững)** | Sống 24/7 và tự phục hồi khi có sự cố | Vòng lặp agent nằm trên cloud/server; tiến trình giám sát độc lập (Watchdog) định kỳ kiểm tra để giải cứu các agent bị kẹt vòng lặp logic (semantic loop). |

---

## 3. Giải Phẫu Kiến Trúc 2 Tầng: Router & Workers

### 3.1. Nghịch Lý Lựa Chọn Mô Hình: Đảo Ngược Vai Trò
Trong các nhóm multi-agent truyền thống, người ta thường đặt mô hình mạnh nhất (Opus/GPT-4o) làm "Trưởng nhóm" (Lead) để phân rã nhiệm vụ và mô hình rẻ làm công nhân. **DeepSeek đảo ngược hoàn toàn tư duy này:**
- **Router (Tầng 1):** Nhiệm vụ của Router thực chất đơn giản hơn vẻ bề ngoài — nó chỉ đọc tin nhắn, tra cứu bảng người dùng hợp lệ, gán việc và gửi phản hồi dưới 100 từ. Vì vậy, một mô hình siêu nhẹ, siêu rẻ và có độ trễ cực thấp như **DeepSeek V4.1 Flash** là hoàn hảo.
- **Workers (Tầng 2):** Đây mới là nơi diễn ra công việc lập trình thực tế, đòi hỏi suy luận sâu, bám sát context dài và xử lý cú pháp phức tạp. Do đó, toàn bộ ngân sách và mô hình mạnh nhất (**Kimi K3.1 Pro, GLM 5.3, Claude Sonnet**) phải được dồn cho Tầng 2.

### 3.2. Cấm Cứng Bằng Ràng Buộc Kiến Trúc (Deny Rules Over Prompting)
Thực nghiệm của DeepSeek chỉ ra một sự thật cay đắng: Khi prompt mô hình: *"Hãy dùng subagent khi thích hợp"*, mô hình **tự làm 100% trong cả 5 lần chạy** vì nó luôn tự tin đánh giá tác vụ là "đơn giản".
- **Giải pháp của DeepSeek:** Tước bỏ hoàn toàn công cụ `bash` và `edit_file` khỏi Router.
- **Triển khai của AICodeKing:** Thiết lập **Deny Rules** tuyệt đối trên Router Bot trong Pandode. Khi không có công cụ gõ lệnh hay sửa file, Router **buộc phải gọi worker** thông qua cơ chế gửi thông điệp liên bot (`bot_message_bot`).

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                          DEEPSEEK DELEGATION BENCHMARK RESULTS                              │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. Neutral Prompt ("Use subagent when appropriate"):                                        │
│    ➔ Gọi Subagent: 0 / 5 lần (0%)  | Thời gian hoàn thành: 29.5 phút (Chậm nhất)            │
│                                                                                             │
│ 2. Aggressive Prompt ("Push hard for delegation"):                                          │
│    ➔ Gọi Subagent: 4 / 5 lần (80%) | Vẫn có 1 lần tự ý ôm việc vì nghĩ task nhỏ             │
│                                                                                             │
│ 3. Structural Enforcement (Gỡ bỏ Bash tool / Deny Rules):                                   │
│    ➔ Bắt buộc phân luồng 100%      | Thời gian hoàn thành: 15.7 phút (Nhanh hơn 1.9 lần)    │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Kỹ Nghệ Quản Trị Ngữ Cảnh & Kinh Tế Học Token

### 4.1. Ngữ Cảnh Như Một Bộ Đệm (Context as Cache & Chain of Recall)
Thay vì nhồi nhét toàn bộ lịch sử trò chuyện vào prompt của Router:
- Hệ thống chỉ nạp **ID và bản tóm tắt cực ngắn (short summary)** của các đối tượng liên quan.
- Các nội dung dài (diffs, full logs) được cắt tỉa và thay bằng **con trỏ (pointer)**.
- **Chain of Recall:** Mọi đối tượng đều liên kết với nhau qua ID. Khi người dùng nhắc lại một lỗi từ nhiều tuần trước, Router lần theo chuỗi ID này để truy xuất đúng ngữ cảnh lịch sử mà không làm phình kích thước cửa sổ hoạt động.

### 4.2. Cơ Chế Lắng Nghe Trực Tiếp (Thread Subscriptions)
Khi Worker nhận bàn giao một Pull Request (ví dụ PR #102):
- Worker **tự động đăng ký (subscribe)** vào luồng sự kiện của PR đó.
- Các bình luận review mới từ GitHub sẽ chảy thẳng vào Worker mà Router không cần tốn token trung chuyển.
- Nếu 2 Worker cùng thao tác trên 1 PR, cả hai cùng nhận được luồng sự kiện để **tự động phát hiện xung đột mã nguồn**.

### 4.3. Bài Toán Kinh Tế: Vì Sao Mô Hình Giá Rẻ Quyết Định Tương Lai Superagent
Trong một phiên mô phỏng xử lý 20 yêu cầu lập trình thực tế:
- Hệ thống tiêu tốn tới **23.2 triệu Cached Read Tokens** (nếu không có bộ lọc pass-through, con số này lên tới **31.8 triệu tokens**).
- **So sánh chi phí:**
  - Trên **Claude Sonnet 3.5/5.5**: 23.2 triệu tokens tốn khoảng **$4.64**.
  - Trên **DeepSeek V4.1 Flash (Off-peak)**: 23.2 triệu tokens chỉ tốn **~$0.07**.
- **Kết luận:** Mô hình Superagent chạy nền liên tục chỉ có thể trở thành hiện thực trong doanh nghiệp nếu chi phí token đệm giảm về mức tiệm cận 0, biến DeepSeek và các dòng mô hình mã nguồn mở giá rẻ thành nền tảng tối ưu.

---

## 5. Hướng Dẫn Thực Hành: Xây Dựng Superagent Cá Nhân Bằng Pandode & Cline Pass

AICodeKing hướng dẫn chi tiết cách hiện thực hóa các ý tưởng của DeepSeek ngay trên máy tính cá nhân bằng bộ công cụ có sẵn:

```
                  ┌───────────────────────────────────────────────────────────┐
                  │                 PANDODE PERSONAL SUPERAGENT               │
                  └─────────────────────────────┬─────────────────────────────┘
                                                │
                     ┌──────────────────────────┴──────────────────────────┐
                     ▼                                                     ▼
        ┌─────────────────────────┐                           ┌─────────────────────────┐
        │ ROUTER BOT (Telegram)   │                           │ CLINE DESKTOP           │
        │ - Model: DeepSeek V4.1  │                           │ - Tác vụ định kỳ (Cron) │
        │ - Deny: Bash / Edit     │                           │ - Review PR ban đêm     │
        │ - Pairing Code (RBAC)   │                           │ - Security Scans        │
        └────────────┬────────────┘                           └─────────────────────────┘
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
┌──────────────────┐  ┌──────────────────┐
│ BUILDER BOT      │  │ REVIEWER BOT     │
│ - Model: Kimi    │  │ - Model: GLM 5.3 │
│   K3.1 Pro       │  │ - Bắt lỗi góc    │
│ - Giữ context sâu│  │   khuất đối lập  │
└──────────────────┘  └──────────────────┘
```

### Bước 1: Cài Đặt Công Cụ & Gói Thuê Bao Mô Hình Siêu Rẻ
1. Cài đặt **Pandode** (Ứng dụng desktop mã nguồn mở quản lý đa agent).
2. Đăng ký gói **Cline Pass** ($9.99/tháng): Cung cấp quyền truy cập trọn gói không giới hạn cho các mô hình mã nguồn mở hàng đầu: Kimi K3.1 Pro, GLM 5.3, Qwen 38 Max, DeepSeek V4.1 Flash.
3. Cài đặt CLI:
   ```bash
   npm install -g cline
   cline auth  # Đăng nhập và kích hoạt Cline Pass
   npm install -g @deepseek-ai/dsh
   ```

### Bước 2: Thiết Lập Router Bot
- Tạo bot tên `Router`, gán não bộ là **DeepSeek V4.1 Flash**.
- Kết nối kênh liên lạc qua **Telegram Bot** (yêu cầu mã Pairing Code duyệt con người trong lần đầu để chống tin nhắn rác).
- **Cấu hình Deny Rules:** Tắt quyền truy cập tool `shell` và `edit_file`.
- Thiết lập System Prompt: Giới hạn câu trả lời chat dưới 100 từ, nhiệm vụ duy nhất là tiếp nhận và phân phối việc qua lệnh `bot_message_bot`.

### Bước 3: Thiết Lập Đội Ngũ Worker Chuyên Biệt
- **Builder Bot:** Sử dụng **Kimi K3.1 Pro** nhờ khả năng ghi nhớ ngữ cảnh dài cực tốt cho các phiên viết mã phức tạp.
- **Reviewer Bot:** Sử dụng **GLM 5.3** (nguyên lý: *hai mô hình khác nhau có các điểm mù khác nhau*, giúp phát hiện bug khách quan).
- **Cơ chế phê duyệt:** Khi Worker cần chạy lệnh nguy hiểm (xóa DB, deploy production), thông báo phê duyệt sẽ gửi về Telegram của kỹ sư để bấm Yes/No.

### Bước 4: Thiết Lập Watchdog Tự Động Định Kỳ
- Dùng **Cline Desktop** thiết lập lịch chạy mỗi 30 phút.
- Watchdog sử dụng GitHub CLI (`gh pr list`, `gh run list`) kiểm tra tình trạng CI và PRs; chỉ nhắn tin cảnh báo kỹ sư khi phát hiện tiến trình bị treo hoặc lỗi.

---

## 6. Cảnh Báo An Ninh Sống Còn (Security & Prompt Injection)

AICodeKing nhấn mạnh lời cảnh báo nghiêm khắc từ DeepSeek:
1. **Rủi Ro Prompt Injection Từ Kênh Công Khai:**
   - Nếu bạn kết nối Superagent vào một nhóm chat công khai hoặc mở quyền cho bất kỳ ai comment trên GitHub issue, kẻ xấu có thể chèn prompt độc hại (Prompt Injection).
   - Nếu Router bị chiếm quyền, nó có thể điều khiển các worker chạy mã độc phá hoại toàn bộ các máy chủ được kết nối.
2. **Quy Tắc Vàng:**
   - Chỉ cho phép người dùng đã xác thực (Allowlist / Pairing code) tương tác với Agent.
   - Luôn khởi chạy các tác vụ tự động ở chế độ **Dry-Run (Chỉ ghi log, không ghi đè mã nguồn)** cho đến khi độ tin cậy được kiểm chứng hoàn toàn.

---

## 7. Bài Học Hành Động Dành Cho Kỹ Sư (Actionable Takeaways)

1. **Nhận Diện Điểm Nghẽn Bản Thân:** Đừng cố mở 10 tab chat Claude/Cursor để rồi kiệt sức chuyển đổi ngữ cảnh. Hãy chuyển sang tư duy xây dựng Router trung gian.
2. **Ép Buộc Phân Luồng Bằng Ràng Buộc Cứng (Hard Constraints):** Không tin vào prompt khi muốn agent phân quyền. Hãy tước công cụ shell khỏi tác nhân điều phối.
3. **Tận Dụng Mô Hình Đa Dạng (Ensemble Diversity):** Dùng mô hình siêu rẻ (DeepSeek Flash) cho việc định tuyến, mô hình giữ context (Kimi K3.1 Pro) để code, và mô hình độc lập (GLM 5.3) để review.
4. **Tách Biệt Nhiệm Vụ Cố Định:** Đưa các việc lặp lại (review PR đêm, quét bảo mật, dọn dẹp log) vào các job định kỳ độc lập (Cline Desktop Cron) thay vì giao cho agent tương tác.
