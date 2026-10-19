# 🧩 9 NEW Claude Mods That Can Truly Change How You Work — Kiến Trúc Phần Mềm Tự Biến Hình (Malleable Software) Trong Claude Code

> **Nguồn tư liệu:** [9 NEW Claude Mods that can truly change how you work — Jay E (RoboNuggets)](https://www.youtube.com/watch?v=lDrAZ1wAyVs)  
> **Tác giả / Kênh:** **Jay E | RoboNuggets** (Chuyên gia AI, Thạc sĩ Data Science, Sáng lập cộng đồng RoboNuggets)  
> **Thời lượng video:** 12 phút 47 giây (767s) | **Toàn văn Transcript:** [9_new_claude_mods_robonuggets_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/9_new_claude_mods_robonuggets_transcript.txt)  
> **Chủ đề cốt lõi:** Claude Code Mods (Bản cập nhật v2.1.287+), Phần mềm Tự biến hình (Malleable Software), Phân biệt Mods vs Skills vs MCP Connectors, Kiến trúc Hướng sự kiện (Event-driven Architecture), 9 Bản Mod Đột phá, Giám sát Chi phí Prompt Cache (`/keepwarm`), Cổng an toàn Blast Radius, Vòng lặp Micro-RLHF Reflect.

---

## Executive Summary: Cú Hích "Malleable Software" Đưa Claude Code Thành Môi Trường Tùy Biến Tuyệt Đối

Vào đầu tháng 10/2026, Anthropic đã âm thầm tung ra một trong những bản cập nhật mang tính cách mạng nhất cho **Claude Code** (từ phiên bản `v2.1.287` trở lên): **Hệ thống MODS**.

Ý tưởng cốt lõi của tính năng này mang tính triết học phần mềm sâu sắc: **Nếu người dùng có thể mô tả một tính năng mà họ muốn Claude Code sở hữu, Claude Code có thể tự lập trình tính năng đó vào chính bản thân nó.** Đây là hiện thân thực tế đầu tiên của khái niệm **"Malleable Software" (Phần mềm tự biến hình)** do một phòng nghiên cứu AI hàng đầu thế giới triển khai thương mại.

```
+----------------------------------------------------------------------------------------------------+
|               PHÂN ĐỊNH BA TẦNG MỞ RỘNG TRONG HỆ SINH THÁI CLAUDE CODE                             |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ 1. SKILLS (SỔ TAY CÔNG TÁC) ]                                                                   |
|    • Bản chất: Tập tin Markdown hướng dẫn Agent *cách thức* giải quyết một nghiệp vụ cụ thể.       |
|    • Phạm vi: Nằm trong không gian giải pháp của bài toán; không thay đổi giao diện hay engine.     |
|    • Ví dụ: Hướng dẫn viết test Playwright, quy trình review code, cấu trúc commit Git.            |
|                                                                                                    |
|  [ 2. MCP CONNECTORS (CẦU NỐI NGOẠI VI) ]                                                          |
|    • Bản chất: Giao thức chuẩn kết nối Claude Code với hệ thống bên ngoài (Database, Browser, API).|
|    • Phạm vi: Mở rộng giác quan và bàn tay của Agent sang các ứng dụng khác.                       |
|    • Ví dụ: MCP Server Postgres, MCP GitHub, MCP Slack, MCP Watch-Skill.                           |
|                                                                                                    |
|  [ 3. CLAUDE MODS (BIẾN HÌNH BẢN THÂN ENGINE) ]                                                    |
|    • Bản chất: Mã thực thi can thiệp trực tiếp vào runtime và giao diện người dùng của Claude Code.|
|    • Phạm vi: Chạy BÊN TRONG Claude Code, lắng nghe Event Bus, thay đổi UI/Terminal, chặn hành vi. |
|    • Khả năng: Thêm thanh tiến độ, tạo side panel, đổi theme màu, chặn lệnh nguy hiểm, gắn mini-game.|
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 1. Kiến Trúc Hướng Sự Kiện Của Claude Mods (Event Bus Architecture)

Làm thế nào một đoạn mã mod có thể thay đổi những gì hiển thị trên màn hình terminal hay ứng dụng desktop của Claude Code? [03:03]

Khi Claude Code thực thi bất kỳ hành động nào (chạy lệnh bash, gọi sub-agent, đọc tệp tin, truy vấn model), nó đều phát ra một tín hiệu nội bộ gọi là **Event (Sự kiện)**. Một bản Mod về bản chất là một tiến trình lắng nghe các sự kiện này và có thể can thiệp theo **3 cơ chế**:

```
                              [ CLAUDE CODE RUNTIME ]
                                         |
                                (Phát tín hiệu Event)
                                         v
                         +-------------------------------+
                         |   EVENT BUS (HỆ THỐNG SỰ KIỆN) |
                         +-------------------------------+
                                         |
               +-------------------------+-------------------------+
               |                         |                         |
               v                         v                         v
     [ 1. WATCH (GIÁM SÁT) ]    [ 2. MUTATE (THAY ĐỔI) ]    [ 3. INTERCEPT (CHẶN BẮT) ]
     - Chỉ đọc dữ liệu event.   - Sửa đổi nội dung event.   - Chiếm quyền điều khiển.
     - Đo lường token, thời     - Đổi màu sắc hiển thị,     - Ngăn chặn lệnh xóa file,
       gian, gọi API credits.     vẽ thêm thanh Progress.     bắt người dùng xác nhận.
```

* **Ưu điểm về chi phí (Token Economy):** Phần lớn các bản mod được xây dựng bằng **Mã tất định (Deterministic Code - TypeScript/Node.js/Python)**, do đó chúng tiêu tốn **0 Token LLM** khi vận hành hàng ngày (trừ các mod phân tích chuyên sâu như *You Should Know*).

---

## 2. Giải Phẫu Chi Tiết 9 Bản Mod Đột Phá

### Mod 1: Savvy Progress — Giám Sát Đội Ngũ Sub-agents Theo Thời Gian Thực [02:02]
* **Vấn đề giải quyết:** Khi giao cho Claude các tác vụ nặng chạy nhiều sub-agents ngầm, terminal thường rơi vào trạng thái "hộp đen" im ắng khiến người dùng không biết agent có bị treo hay đang đốt sạch ngân sách token.
* **Cơ chế hoạt động:**
  * Vẽ một thanh tiến độ (*Progress Bar*) trực quan ngay phía trên thanh nhập lệnh prompt.
  * Khi có đội ngũ sub-agents hoạt động song song, mod tự động mở một bảng điều khiển trực tiếp (*Live Panel*):
    * Liệt kê từng helper sub-agent kèm định danh model (Opus, Sonnet, Flash).
    * Hiển thị tiến độ hoàn thành (%) và ước tính số tiền API credits đã tiêu tốn theo thời gian thực.
  * Cho phép người dùng tùy biến hình ảnh Mascot đại diện (ví dụ: chú cua pixel trang phục lập trình viên hoặc nhà thiết kế).

### Mod 2: Claude Skins — Cá Nhân Hóa Hệ Thống Nhận Diện Thị Giác [03:56]
* **Vấn đề giải quyết:** Mặc định terminal hiển thị đơn điệu, khó phân biệt giữa log công cụ, diff code và văn bản chat thông thường.
* **Cơ chế hoạt động:**
  * Cung cấp các bộ Theme hoàn chỉnh (như *Noir*, *Tokyo Night*).
  * Mỗi loại Tool Call (Bash, File Read, File Edit, Web Search) được cấp biểu tượng icon độc lập.
  * Các khối chỉnh sửa code được tô màu cú pháp rõ ràng, giúp mắt người dùng dễ dàng lướt nhanh những vùng dữ liệu quan trọng.

### Mod 3: You Should Know (Anthropic Official) — Bộ Lọc Rủi Ro Tiềm Ẩn [04:33]
* **Vấn đề giải quyết:** Hiện tượng "Bức tường chữ" (Wall of Text) — Claude thường xuất ra những đoạn giải thích dài hàng trăm dòng mà con người hầu như không đọc hết, bỏ sót các cảnh báo nguy hiểm hoặc chi phí đắt đỏ.
* **Cơ chế hoạt động:**
  * Chạy một **Sub-agent thứ hai độc lập** đọc quét ngầm câu trả lời của Claude.
  * Khi phát hiện thông tin quan trọng nhưng bị chôn vùi (ví dụ: chi phí gọi API sinh video Kling qua Kie.ai lên tới vài chục USD, hoặc cảnh báo thư viện sắp bị deprecate), mod sẽ bật một **Pop-up nổi bật** ngay trên màn hình.
  * Người dùng có thể tùy chỉnh tiêu chí quét: tập trung vào chi phí, rủi ro khách hàng hay lỗi tự động hóa.

### Mod 4: File Tree — Bảng Giám Sát Cây Thư Mục Thời Gian Thực [06:05]
* **Vấn đề giải quyết:** Không rõ Agent đang âm thầm đọc và chỉnh sửa những tệp tin nào trong dự án lớn.
* **Cơ chế hoạt động:**
  * Tích hợp một bảng cây thư mục (*File Tree Panel*) bên tay phải màn hình terminal (chi phí 0 token).
  * **Hiệu ứng lấp lánh (Shimmering):** Khi Claude bắt đầu chạm vào một tệp tin (đọc hoặc sửa), tệp đó sẽ nhấp nháy phát sáng trên cây thư mục.
  * **Chuyển xanh (Green):** Khi thay đổi được lưu thành công, tệp tin chuyển sang màu xanh lá.
  * Cực kỳ hữu ích cho cả dân lập trình và người làm tài liệu/thuế cần theo dõi tiến trình xử lý hàng chục hồ sơ cùng lúc.

### Mod 5: Cache Tax & `/keepwarm` — Radar Tiết Kiệm Chi Phí Prompt Cache [07:03]
* **Vấn đề giải quyết:** Cơ chế Prompt Cache của Claude giúp giảm 90% chi phí đọc lại ngữ cảnh. Tuy nhiên, sau khoảng 1 giờ không có tương tác, cache sẽ bị "nguội" (*Cold Cache*). Tin nhắn tiếp theo sẽ buộc Claude nạp lại toàn bộ lịch sử hội thoại khổng lồ, đốt sạch hạn mức sử dụng chỉ trong 1 turn.
* **Cơ chế hoạt động:**
  * **Cảnh báo trước khi gửi:** Mod phát hiện khi nào cache sắp hết hạn và tính toán ước lượng số token/tiền sẽ mất nếu người dùng gửi tin nhắn vào lúc đó.
  * **Lệnh `/keepwarm`:** Tự động gửi một tín hiệu ping cực nhỏ đến phiên làm việc ngay trước khi cache bị nguội, giữ cho phiên hội thoại luôn trong trạng thái "Ấm" (*Warm Cache*) ngay cả khi người dùng rời bàn làm việc.

### Mod 6: Blast Radius (Anthropic Official) — Cổng An Toàn Phanh Khẩn Cấp [08:15]
* **Vấn đề giải quyết:** Nguy cơ thảm họa khi các mô hình AI tự ý chạy lệnh phá hủy (ví dụ: `rm -rf`, xóa toàn bộ thư mục dự án do hiểu sai ngữ cảnh).
* **Cơ chế hoạt động:**
  * Lắng nghe các lệnh có bán kính sát thương lớn (*High Blast Radius*).
  * **Cưỡng chế dừng hoạt động:** Hiển thị danh sách chính xác các tệp tin sẽ bị biến mất vĩnh viễn.
  * Bắt buộc người dùng bằng xương bằng thịt phải bấm nút **Proceed** (Tiếp tục) hoặc **Cancel** (Hủy bỏ) thì lệnh mới được phép chạy.

### Mod 7: Reflect — Vòng Lặp Micro-RLHF Tạo Luật Cá Nhân Hóa [09:05]
* **Vấn đề giải quyết:** Người dùng phải nhắc đi nhắc lại một thói quen hoặc sở thích code cho Claude qua từng phiên làm việc.
* **Cơ chế hoạt động:**
  * Lắng nghe các từ khóa phản hồi tiêu cực từ người dùng: *"Không"*, *"Đừng làm thế này"*, *"Tôi muốn làm theo cách X cơ"*.
  * Ngay lập tức hiển thị một thanh banner gợi ý: *"Bạn có muốn lưu quy tắc này vĩnh viễn vào tệp CLAUDE.md hoặc Skill không?"*.
  * Chỉ cần 1 cú click, quy tắc mới được ghi thẳng vào tệp cấu hình, giúp Claude thông minh hơn theo thời gian dựa trên chính phong cách của bạn.

### Mod 8: Terminal Browser — Trình Duyệt Nhúng Ngay Trong CLI [10:02]
* **Vấn đề giải quyết:** Nút thắt chuyển đổi ngữ cảnh (Context Switching) liên tục giữa Terminal và Google Chrome để xem trước giao diện web hoặc kiểm tra PR trên GitHub.
* **Cơ chế hoạt động:**
  * Mở một khung trình duyệt thật ngay cạnh luồng chat terminal.
  * Claude vừa code xong trang web hoặc tạo PR, kết quả hiển thị tức thì bên cạnh để người dùng kiểm tra trực quan.

### Mod 9: Replay Theater (Anthropic Official) — Trình Chiếu Tua Lại Lịch Sử Sửa Mã [10:10]
* **Vấn đề giải quyết:** Sau một lượt code dài, người dùng muốn hiểu tuần tự tư duy của Agent đã can thiệp vào những dòng nào thay vì đọc một bản diff tĩnh khổng lồ.
* **Cơ chế hoạt động:**
  * Sau khi turn kết thúc, xuất hiện nút **Replay**.
  * Khi bấm vào, mod sẽ tua lại từng bước chỉnh sửa tệp tin theo dòng thời gian như một thước phim ngắn (time-lapse), giúp kỹ sư nắm bắt chính xác tiến trình biến đổi của codebase.

---

## 3. Quy Trình Tự Động Tạo Mod Bằng Kỹ Năng `plugin-authoring`

Để xây dựng một bản Mod cho riêng mình, người dùng không cần học thuộc API phức tạp [10:51]:

1. **Yêu cầu phiên bản:** Claude Code từ phiên bản `2.1.287` trở lên (gõ `claude update`).
2. **Kỹ năng có sẵn:** Claude Code đã được tích hợp sẵn skill mang tên **`plugin-authoring`**.
3. **Mô tả bằng ngôn ngữ tự nhiên:** Chỉ cần chat: *"Tôi muốn một bảng bên cạnh hiển thị số dòng code đã sửa"* $\rightarrow$ Claude sẽ tự động viết mã mod.
4. **Cơ chế Hot-Reloading:** Claude sẽ hỏi bạn có muốn bật chế độ tự động tải lại phiên làm việc không. Khi bật, bản mod mới sẽ xuất hiện ngay lập tức trên màn hình để dùng thử.
5. **Lưu vĩnh viễn:** Mặc định mod chỉ tồn tại trong phiên thử nghiệm; chỉ cần yêu cầu: *"Save this as a permanent plugin"*, mod sẽ được cài đặt cố định cho mọi phiên làm việc tương lai.

---

## 4. Tầm Nhìn Kỷ Nguyên Phần Mềm Tự Biến Hình (Malleable Software)

Video của Jay E chỉ ra một bước ngoặt lớn của ngành công nghiệp phần mềm:

* Trước đây, người dùng phải thích nghi với giao diện và tính năng cố định của nhà sản xuất (VS Code, JetBrains, Terminal chuẩn).
* Với **Claude Mods**, ranh giới giữa **Người dùng (User)** và **Nhà phát triển công cụ (Toolmaker)** đã bị xóa nhòa. Claude Code không chỉ là một trợ lý lập trình, mà trở thành một **phôi phần mềm (Software Embryo)** có khả năng tự mọc thêm cơ quan xúc giác, mắt nhìn và nút điều khiển theo chính xác thói quen tư duy của từng kỹ sư.
