# Hướng Dẫn Tùy Biến Tối Thượng: Claude Code Mods | Eric Tech

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=8wZVnc99x_k)
- **Video ID:** `8wZVnc99x_k`
- **Category:** #ai-agents
- **Date Processed:** 2026-10-20
- **Duration:** 09:53
- **Speaker:** Eric Tech
- **Transcript File:** [claude_code_mods_ultimate_customization_guide_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/claude_code_mods_ultimate_customization_guide_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong cẩm nang kỹ thuật thực chiến này, **Eric Tech** giải phẫu tính năng thay đổi cuộc chơi của Claude Code: **Claude Code Mods (Plugins)** — cho phép người dùng không chỉ điều khiển hành vi logic mà còn tái thiết kế toàn bộ giao diện người dùng (UI), chặn bắt các cuộc gọi công cụ (tool call interception), và nhúng trạng thái có bộ nhớ (stateful memory) vào bên trong ứng dụng Claude Code.

Eric đưa ra hai phép ẩn dụ mang tính khai sáng để làm rõ sự khác biệt giữa các cơ chế tùy biến:
1. **Phép Ẩn Dụ Căn Nhà (The House Metaphor):**
   - **Skills & Hooks:** Là *Nội thất* của căn nhà (bạn có thể kê thêm bàn ghế, bổ sung quy trình SOPs).
   - **Mods:** Là việc *Đập tường, xây lại layout* căn nhà (thay đổi tận gốc cách thức giao diện hiển thị và luồng tương tác bên trong).
2. **Phép Ẩn Dụ Nhân Sự (Contractor vs. Full-Time Employee):**
   - **Hooks (Nhà thầu bên ngoài):** Chạy bên ngoài ứng dụng Claude Code, được kích hoạt phi trạng thái (stateless) qua script kịch bản dòng lệnh, không có quyền chạm vào giao diện đồ họa.
   - **Mods (Nhân viên chính thức):** Sống ngay bên trong runtime của Claude Code, có bộ nhớ trạng thái duy trì qua các lượt chat, có quyền tương tác trực tiếp lên giao diện (vẽ progress bar, tạo nút bấm chuyển model, hiển thị modal câu hỏi phụ) và tạm dừng tiến trình để xin phê duyệt từ người dùng.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                       CLAUDE CODE INTERNAL MOD RUNTIME ARCHITECTURE                         │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                  USER / DEVELOPER                                           │
│                                          │                                                  │
│                                          ▼                                                  │
│                        [ CLAUDE CODE DESKTOP / CLI RUNTIME ]                                │
│                                          │                                                  │
│         ┌────────────────────────────────┼────────────────────────────────┐                 │
│         ▼ (UI / Presentation Layer)      ▼ (Interception Layer)           ▼ (Memory Layer)  │
│   [ WINDOW MODIFICATIONS ]         [ TOOL CALL INTERCEPTORS ]       [ STATEFUL CACHE ]      │
│   - Header Progress Bar            - Intercept File Writes          - Context Window Meter  │
│   - Clickable Model Switcher       - Require Interactive Approval   - Prompt Cache % Gauge  │
│   - Image Previews in Prompt Box   - Sensitive Path Guardrails      - Skill Calibration Log │
│         │                                │                                │                 │
│         └────────────────────────────────┼────────────────────────────────┘                 │
│                                          │                                                  │
│                                          ▼                                                  │
│                         [ FRONTIER MODEL (THE BRAIN) ]                                      │
│                               (Claude 3.7 / Opus 5.5)                                       │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. So Sánh Bản Chất: Hooks vs. Mods

| Đặc tính kỹ thuật | Claude Code Hooks | Claude Code Mods (Plugins) |
| :--- | :--- | :--- |
| **Vị trí cư trú** | Nằm ngoài ứng dụng (thư mục `.claude/hooks/`) | Nằm bên trong runtime ứng dụng (`.claude/plugins/`) |
| **Giao diện người dùng (UI)** | Hoàn toàn không thể can thiệp vào UI | Toàn quyền kiểm soát, vẽ thêm widgets, thanh tiến trình |
| **Quản lý trạng thái (Memory)** | Vô trạng thái (Stateless); chạy xong tự hủy | Có bộ nhớ (Stateful); lưu dữ liệu qua nhiều phiên chat |
| **Cơ chế can thiệp** | Kích hoạt tự động khi lệnh thực thi xong | Chặn bắt (Intercept), tạm dừng, hiển thị hộp thoại xin phép |
| **Hình tượng tương đương** | Nhà thầu bên ngoài (External Contractor) | Nhân viên biên chế nội bộ (Full-Time Employee) |

---

## 3. Các Tính Năng Đột Phá Mà Mods Mang Lại

### 1. Tùy Biến Cửa Sổ & Giao Diện Người Dùng (UI Window Customization)
- **Thanh tiến trình & Giám sát Cache (Cache % Tracker):** Hiển thị trực quan tỷ lệ cache hit của Prompt Caching ngay trên thanh trạng thái, giúp lập trình viên biết chính xác mình đang tiết kiệm được bao nhiêu token.
- **Nút chuyển đổi mô hình (Visual Model Switcher):** Thay vì phải gõ lệnh `/model` và chọn danh sách thủ công, Mod cho phép gắn các nút bấm trực quan để chuyển tức thì giữa Sonnet, Haiku, và Opus 5.5.
- **Xem trước ảnh nội dòng (Inline Image Previews):** Khi chụp ảnh màn hình hoặc tải ảnh vào cửa sổ chat, Mod hiển thị thumbnail ảnh trực tiếp bên trên ô nhập prompt thay vì chỉ hiển thị đường dẫn tệp văn bản khô khan.

### 2. Chặn Bắt & Kiểm Soát Cuộc Gọi Công Cụ (Tool Call Interception)
- Khi mô hình chuẩn bị thực thi công cụ chỉnh sửa tệp tin nhạy cảm (như `.env`, migrations, hoặc core files), Mod chặn yêu cầu lại và hiển thị hộp thoại xác nhận tương tác, chờ lập trình viên bấm cho phép rồi mới nhả lệnh.

---

## 4. Top 6 Mods Thực Chiến Được Đánh Giá Cao Nhất

Từ kho lưu trữ cộng đồng với hơn 50 bộ sưu tập mods, Eric chỉ ra 6 mod có giá trị thực dụng cao nhất:

1. **`Next Steps` (Phát triển bởi Theoric — Kỹ sư Anthropic):**
   - Tự động gợi ý các bước tiếp theo mang tính logic sau mỗi hành động (ví dụ: sau khi git status sạch sẽ, mod đề xuất nút bấm *"Commit changes"*, *"Switch branch"* hoặc *"Bump version"* điền thẳng vào prompt).
2. **`Aside` (Khung Hội Thoại Phụ - Sub-Chat Modal):**
   - **Vấn đề giải quyết:** Khi đang chạy một session phức tạp, nếu lập trình viên hỏi một câu hỏi ngoài lề (như *"hàm này cú pháp thế nào?"*), toàn bộ câu hỏi và câu trả lời rác sẽ làm ô nhiễm cửa sổ ngữ cảnh chính (Context Pollution).
   - **Giải pháp:** `Aside` mở một pop-up chat phụ cô lập hoàn toàn, trả lời xong đóng lại mà không để lại bất kỳ token thừa nào trong luồng chính.
3. **`Secret Redactor` (Tự Động Che Giấu Bí Mật):**
   - Tự động quét và che giấu (mask/redact) các chuỗi nhạy cảm như API keys, tokens, mật khẩu trước khi gửi payload lên mô hình, ngăn ngừa rò rỉ bảo mật.
4. **`Claude Queue` (Hàng Đợi Lệnh Tự Động):**
   - Cho phép người dùng xếp hàng nhiều prompt và tác vụ để Claude Code tự động xử lý tuần tự mà không cần ngồi chờ canh từng lượt phản hồi.
5. **`Flight Deck` (Trạm Chỉ Huy Tác Vụ):**
   - Bảng điều khiển tổng hợp theo dõi các sub-agents đang chạy ngầm, trạng thái kế hoạch và hạn mức quota.
6. **`Context Meter` / `Quota Watcher`:**
   - Đồng hồ đo mức tiêu thụ context window theo thời gian thực, cảnh báo khi phiên làm việc chạm ngưỡng nguy hiểm cần nén (compact).

---

## 5. Hướng Dẫn Cài Đặt & Quản Trị Hệ Thống Mods

### 1. Cập Nhật Phiên Bản Mới Nhất
```bash
claude update
```

### 2. Cài Đặt Mod Từ Cộng Đồng Hoặc Đường Dẫn URL
```bash
# Cài đặt trực tiếp từ URL mod được chia sẻ
# Ví dụ: Dán link repository plugin của cộng đồng vào phiên Claude Code
claude plugins install <PLUGIN_GIT_URL_OR_NAME>
```

### 3. Vị Trí Lưu Trữ Cục Bộ
Toàn bộ các mod đã cài đặt được lưu trữ tại:
```text
~/.claude/plugins/
```

### 4. Quản Lý Danh Sách & Gỡ Bỏ
```bash
# Liệt kê tất cả các mods đang hoạt động
claude plugin list

# Gỡ bỏ mod không sử dụng
claude plugin uninstall <plugin_name>
```

---

## 6. Bài Học Hành Động

1. **Nâng cấp từ Prompting sang Interface Customization:** Đừng chấp nhận giao diện mặc định nếu nó làm chậm tốc độ của bạn. Hãy tận dụng Mods để đưa các thông tin quan trọng (Cache %, Token Quota, Active Plans) ra trước mắt.
2. **Sử Dụng `Aside` Để Bảo Vệ Context:** Tuyệt đối không hỏi các câu hỏi lý thuyết bên lề trong phiên làm việc chính; hãy dùng modal `Aside` để giữ cho ngữ cảnh sạch và tiết kiệm chi phí.
3. **Bảo mật là trên hết:** Cài đặt ngay `Secret Redactor` để đảm bảo các khóa API bí mật không bao giờ bị vô tình gửi vào context prompt của LLM.
