# Những Claude Code Mods Hạng S (S-Tier): Mẫu Hình Kiến Trúc & Tối Ưu Bầy Agent | Ray Amjad

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=Od9706XIXcc)
- **Video ID:** `Od9706XIXcc`
- **Category:** #ai-agents
- **Date Processed:** 2026-10-20
- **Duration:** 19:00
- **Speaker:** Ray Amjad (Kỹ sư AI, Tác giả khóa học Agentic Engineering)
- **Transcript File:** [s_tier_claude_code_mods_ray_amjad_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/s_tier_claude_code_mods_ray_amjad_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong bài phân tích kỹ thuật đẳng cấp này, **Ray Amjad** làm sáng tỏ nguyên lý thiết kế đứng sau những **Claude Code Mods hạng S (S-Tier)** — bộ công cụ mở rộng giúp biến Claude Code từ một CLI chat đơn thuần thành một hệ thống điều hành phát triển phần mềm tự chủ mạnh mẽ.

Ray chỉ ra mẫu hình kiến trúc then chốt tạo nên sức mạnh của các S-Tier Mods:
> **"Mỗi khi Claude hoàn thành một lượt xử lý (turn), một bộ đánh giá siêu nhẹ (Haiku hoặc Decision Classifier như Jev) sẽ kiểm tra xem có tác vụ nền nào đáng làm hay không. Nếu có, phiên hội thoại sẽ được phân nhánh (Fork) sang một agent con chạy ngầm trên cùng tiền tố Prompt Cache để xử lý, trong khi luồng chính của Claude tiếp tục tập trung vào mục tiêu của người dùng."**

Hệ thống mod khai thác 2 khối xây dựng nền tảng (Primitives):
1. **`model_complete`:** Gọi nhanh các mô hình kinh tế (Haiku để lấy câu trả lời nhị phân Yes/No, hoặc Jev để lấy phân phối xác suất độ tự tin).
2. **`conversation_fork`:** Nhân bản toàn bộ ngữ cảnh hội thoại sang một luồng phụ độc lập (Side-car worker), tận dụng lại 100% prompt cache mà không làm ô nhiễm context của luồng chính.

Ray giới thiệu danh mục **9 Mod Hạng S** giải quyết triệt để các vấn đề nhức nhối nhất trong kỹ nghệ tác nhân: Quản lý bộ nhớ tự động, trực quan hóa sơ đồ phiên làm việc dài, theo dõi bầy 100 agents trên cloud sandboxes, chống cạn kiệt Prompt Cache, định tuyến kỹ năng thông minh, và chặn bắt prompt mơ hồ.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                       S-TIER CLAUDE CODE MODS: FORK & CLASSIFY PATTERN                      │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                     MAIN SESSION TURN                                       │
│                               (Claude 3.7 / Opus 5.5 Focus)                                 │
│                                             │                                               │
│                                             ▼                                               │
│                           [ TURN COMPLETION TRIGGER / HOOK ]                                │
│                                             │                                               │
│                                             ▼                                               │
│                         [ FAST CLASSIFIER (Haiku / Jev Router) ]                            │
│                         - Takes <50ms, Cost: $0.0001                                        │
│                         - Question: "Did user state a memory/decision/doc need?"            │
│                                             │                                               │
│                      ┌──────────────────────┴──────────────────────┐                        │
│                      ▼ (NO)                                        ▼ (YES)                  │
│               [ CONTINUE WORK ]                        [ CONVERSATION FORK ]                │
│               Main agent moves on                      - Clones state with same Warm Cache  │
│                                                        - Appends specialized side prompt    │
│                                                                    │                        │
│                                       ┌────────────────────────────┼────────────────────────┐
│                                       ▼                            ▼                        ▼
│                              [ MEMORY UPDATER ]            [ DIAGRAM PANE ]         [ SWARM MONITOR ]
│                              Writes `memory.md`            Generates Mermaid        100 Cloud VM
│                              without polluting main        in side panel            Agents on Impello
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Giải Phẫu Danh Mục 9 Claude Code Mods Hạng S (S-Tier)

### 1. `memory-fork` (Hệ Thống Quản Lý Bộ Nhớ Tự Động)
- **Vấn đề:** Bắt agent chính vừa code vừa nhớ các quy tắc hệ thống sẽ làm loãng mục tiêu và lãng phí token suy nghĩ.
- **Giải pháp:** Khi người dùng nói *"Hãy nhớ rằng tôi muốn độ trễ P99 của ClickHouse dưới 50ms"*, Haiku lập tức phân loại đây là yêu cầu ghi nhớ. Một phiên fork được tạo ra chạy ngầm, tự động mở tệp `memory.md`, ghi lại quy tắc mới và đồng bộ xuống đĩa. Cửa sổ bên phải hiển thị số dòng diff đã cập nhật.

### 2. `diagram-pane` (Sơ Đồ Hóa Phiên Làm Việc Kéo Dài)
- **Vấn đề:** Khi Claude làm việc liên tục trong 30-60 phút giải quyết một tác vụ lớn, người dùng bị choáng ngợp bởi hàng nghìn dòng chữ log và không biết hệ thống đã ra những quyết định kiến trúc nào.
- **Giải pháp:** Sau các lượt xử lý lớn, mod tự động tạo sơ đồ trực quan hiển thị trên thanh sidebar bên phải. Sử dụng quy ước hình khối rõ ràng:
  - Hình chữ nhật vuông: Tệp tin đã chỉnh sửa.
  - Hình chữ nhật bo tròn góc: Thành phần/component mới được tạo.
  - Hình chữ nhật viền đôi: Các bước kiểm thử và lệnh bash đã chạy.
  - **Tích hợp Mermaid:** Tự động bắt các khối Mermaid trong văn bản và ghim (pin) sang bảng điều khiển bên cạnh để người dùng tương tác phóng to/thu nhỏ.

### 3. `dream-pass` (Bảo Trì Bộ Nhớ Định Kỳ Theo Phong Cách Cognition)
- Lấy cảm hứng từ tính năng "Dreaming" của Cognition (nhóm phát triển Devin).
- Tự động kích hoạt sau mỗi 20 lượt chat hoặc ngay trước thời điểm nén ngữ cảnh (`compaction`).
- Quét toàn bộ kho `memory.md` và các phiên cũ để: xóa bỏ các ghi chú đã lỗi thời, gộp các ghi chú trùng lặp, và viết lại tri thức tinh gọn.

### 4. `swarm-pane` (Trạm Giám Sát Bầy Điểu Khiển Đám Mây)
- **Khả năng:** Quản lý và theo dõi trực tiếp từ 50 đến 100 cloud sandboxes chạy song song trên nền tảng **Impello** (chi phí cực rẻ: chỉ ~$1.80/giờ cho 100 máy ảo).
- **Tính năng:**
  - Bảng tin nhắn tập trung (Message Board): Giám sát các trao đổi giữa các agent con trong bầy khi chúng chia nhau nghiên cứu, tìm lỗi và red teaming.
  - Phân tích cụm (Cluster Analysis): Nhận diện các cặp agent nào đang phối hợp thường xuyên nhất để phát hiện điểm nghẽn giao tiếp.

### 5. `decision-tree` (Khám Phá Quyết Định Phân Nhánh Tương Tác)
- **Vấn đề:** Thay vì bắt người dùng trả lời từng câu hỏi cứng nhắc theo một đường thẳng, mod cung cấp lệnh `/explain` mở ra một Cây Quyết Định (Decision Tree) trên panel bên phải.
- **Cách hoạt động:** Cho phép người dùng thử nghiệm nhánh SQLite, rồi rẽ sang nhánh PostgreSQL, so sánh ưu/nhược điểm độc lập trên các phiên forked trong nền, sau đó nhấn "Map" để nhìn lại toàn cảnh cây quyết định và chọn phương án tối ưu nạp về phiên chính.

### 6. `cache-clock` (Đồng Hồ Đếm Ngược Cache & Tự Động Handoff)
- **Cơ chế Prompt Cache của Claude:** Cache tồn tại trong 1 giờ cho phiên chính và 5 phút cho sub-agents.
- Khi thời gian sống của cache còn dưới 5 phút, mod tự động viết một bản tóm tắt bàn giao (`handoff.md`). Khi người dùng quay lại sau nhiều giờ, chỉ cần mở một phiên mới toanh và nạp `handoff.md` mà không phải trả phí "đọc nguội" (cold read) đắt đỏ.

### 7. `jev-skills` (Bộ Định Tuyến Kỹ Năng Tiết Kiệm Token)
- **Vấn đề "Thuế Kỹ Năng" (Skill Tax):** Khi cài đặt 48 skills, phần mô tả và tên gọi của chúng ngốn hơn **10,000 tokens** trong context ở MỌI LƯỢT TRÒ CHUYỆN!
- **Giải pháp:** Chặn Claude tải toàn bộ danh mục kỹ năng vào prompt. Khi người dùng bắt đầu gõ (ví dụ: *"Tôi muốn chỉnh sửa video"*), mô hình Jev trong nền tính toán xác suất và chỉ nạp đúng 1-3 kỹ năng liên quan nhất vào ngữ cảnh.

### 8. `secret-box` / Agent Proxy (Quản Lý Bí Mật An Toàn)
- Cung cấp công cụ `request_secret`. Mọi token/API key được lưu vào tệp `.env` hoặc hệ thống Infracloud mà Claude **bị cấm tuyệt đối không được đọc**.
- Claude chỉ nhìn thấy các định danh giả lập đã được che giấu (redacted IDs như `secret_stripe_live`). Khi agent thực hiện gọi API ra ngoài, mod Agent Proxy sẽ tự động tráo đổi định danh bằng khóa thực tế mà không bao giờ để lộ khóa trong bản ghi lịch sử chat (transcripts).

### 9. `prompt-bouncer` (Người Gác Cổng Prompt Mơ Hồ)
- Khi người dùng gửi một prompt mơ hồ (ví dụ: *"hãy sửa cái thứ trên trang cài đặt"*), Haiku lập tức chặn lại và từ chối gửi lên Claude chính.
- Bắt buộc người dùng đối chiếu với Bảng Thuật Ngữ Dự Án (Glossary) — ví dụ nhắc nhở: *"Dự án này gọi đó là Billing Panel, bạn có muốn sửa trang đó không?"*. Giúp tiết kiệm hàng chục ngàn token cho các lượt sinh mã sai hướng.

---

## 3. So Sánh Hiệu Quả: Trước & Sau Khi Ứng Dụng S-Tier Mods

| Khía cạnh vận hành | Claude Code Mặc Định | Khi Tích Hợp S-Tier Mods |
| :--- | :--- | :--- |
| **Chi phí ngữ cảnh cho Skills** | Tốn 10,000+ tokens cho 48 skills mỗi turn | Giảm xuống còn vài trăm tokens nhờ `jev-skills` router |
| **Bảo mật khóa API** | Nguy cơ rò rỉ vào log và context prompt | Cô lập 100% qua `secret-box` và Agent Proxy |
| **Độ rõ của phiên làm việc dài** | Đọc tường chữ hàng ngàn dòng trong terminal | Xem trực tiếp sơ đồ kiến trúc động trên `diagram-pane` |
| **Tận dụng Prompt Cache** | Quá hạn cache dẫn đến tốn kém chi phí | Tự động sinh `handoff.md` trước 5 phút với `cache-clock` |
| **Điều phối bầy lớn (50-100 agents)** | Gần như bất khả thi trong CLI đơn lẻ | Quản trị trực quan qua `swarm-pane` trên sandbox Impello |

---

## 4. Bài Học Đúc Kết Cho Kỹ Sư Phần Mềm AI

1. **Tận Dụng Sức Mạnh Của Forking:** Đừng để luồng chính gánh vác các tác vụ bên lề như viết tài liệu, cập nhật bộ nhớ hay vẽ sơ đồ. Hãy tách nhỏ sang các nhánh forked chạy ngầm trên cùng prompt cache prefix.
2. **Khai Tử Lãng Phí Token Bằng Decision Classifiers:** Dùng Haiku hoặc Jev làm các chốt chặn System 1 (nhanh, rẻ, chính xác) để phân loại ý định trước khi đánh thức "bộ não khổng lồ" đắt đỏ.
3. **Kỷ Luật Thuật Ngữ (Ubiquitous Language):** Việc sử dụng `prompt-bouncer` ép buộc kỹ sư phải tư duy chuẩn xác về mặt ngôn ngữ trước khi yêu cầu AI viết mã, tạo ra tỷ suất hoàn vốn cao nhất trong dài hạn.
