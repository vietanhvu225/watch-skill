# 🚀 Cách Top 1% Sử Dụng Claude Code Khác Biệt: 10 Bước Chuyển Dịch Tư Duy & Loại Bỏ Lời Khuyên Lỗi Thời — Agentic Academy

> **Nguồn tư liệu:** [How the Top 1% Use Claude Code Differently | Agentic Academy](https://www.youtube.com/watch?v=J9NQTYUP6k0)  
> **Tác giả / Kênh:** **Agentic Academy** (Matt)  
> **Thời lượng video:** 18 phút 09 giây (1,089s) | **Toàn văn Transcript:** [how_the_top_1percent_use_claude_code_differently_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/how_the_top_1percent_use_claude_code_differently_transcript.txt)  
> **Chủ đề cốt lõi:** Bước ngoặt tư duy của Boris Cherny (Nhà sáng lập Claude Code tuyên bố Plan Mode đã hết thời), 10 quy tắc tối thượng của Top 1% kỹ sư AI: Bộ 3 Brief (Task + Guardrails + DoD), Khai tử prompt "Think Hard", Verifiers tất định nhị phân PASS/FAIL, Cạm bẫy vòng lặp `/goal` với judge Haiku, Tái định nghĩa `CLAUDE.md` như trang mục lục (<200 dòng), Cú sốc 450 runs (7/9 bộ skill GitHub tệ hơn file rỗng), Thay thế chữ IN HOA bằng Hooks cưỡng chế, Quản trị Sub-agents & MCP Event-Driven Projects chạy ngầm.

---

## Executive Summary: Sự Đảo Chiều Của Boris Cherny & Cái Chết Của "Plan Mode"

Suốt cả năm qua, mọi cẩm nang hướng dẫn Claude Code đều mở đầu bằng câu thần chú: *"Luôn bắt đầu ở Plan Mode"*. Chính Boris Cherny (kỹ sư trưởng sáng tạo ra Claude Code tại Anthropic) hồi tháng 1/2026 từng khẳng định hầu hết mọi phiên làm việc của ông đều mở đầu bằng Plan Mode.

Thế nhưng, trong một bài đăng gây chấn động trên Hacker News vào tháng 10/2026, **Boris thừa nhận: "Plan Mode hiện tại không còn hữu ích nữa"**. 

Bản chất của Plan Mode ra đời vào một đêm Chủ nhật muộn khi Boris cảm thấy mệt mỏi vì phải nhắc các model cũ (như Sonnet 4.6 hoặc Opus thời kỳ đầu) đừng vội vàng nhảy vào gõ code bừa bãi. Plan Mode thực chất chỉ là một dòng nhắc nhở hệ thống: *"Bạn đang ở chế độ lập kế hoạch, đừng code"*. Với các model tư duy mới như **Claude Opus 5 và Opus 5.5**, khả năng lập kế hoạch diễn ra **đồng thời trong lúc model suy luận (In-flight Planning)**. Ép model lập kế hoạch văn bản dài lê thê trước khi làm việc chỉ làm tốn tokens và làm chậm tốc độ.

```
+----------------------------------------------------------------------------------------------------+
|               BƯỚC CHUYỂN DỊCH TƯ DUY TỪ CŨ SANG MỚI TRONG CLAUDE CODE                             |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ QUY TRÌNH CŨ (LỖI THỜI - 2025/ĐẦU 2026) ]                                                       |
|  1. Bật Plan Mode, viết prompt dài dòng.                                                           |
|  2. Ngồi đọc bản kế hoạch văn bản 2,000 từ của Claude và sửa từng chữ.                             |
|  3. Thêm các từ gào thét: "THINK HARD", "MUST DOUBLE CHECK", "IMPORTANT DO NOT...".                |
|  4. Tự tay gõ "continue" liên tục mỗi khi agent dừng.                                              |
|                                                                                                    |
|                                     │                                                              |
|                                     ▼ (Chuyển dịch sang Top 1%)                                    |
|                                                                                                    |
|  [ QUY TRÌNH MỚI CỦA TOP 1% (CUỐI 2026) ]                                                          |
|  1. Bỏ Plan Mode. Đưa ra BRIEF 3 DÒNG: Task + Guardrails + Definition of Done (DoD).               |
|  2. Để Opus 5.5 tự chạy. Yêu cầu sản phẩm trả về kèm BẢN GIẢI TRÌNH TRỰC QUAN 1 TRANG              |
|     (One-page Visual: Sơ đồ Mermaid, Live Dashboard hoặc Explainer động).                          |
|  3. Để Effort Level mặc định (Medium). Ép chất lượng bằng CHECK SCRIPT TẤT ĐỊNH (Pass/Fail).       |
|  4. Tự động hóa vòng lặp bằng `/goal` kèm điều kiện dừng rõ ràng và Turn Limit.                    |
|  5. Cắt 80% `CLAUDE.md`, biến thành trang Mục lục (TOC) trỏ file on-demand.                        |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 1. Mười Bước Chuyển Dịch Tư Duy Của Top 1% Kỹ Sư Claude Code

### Bước 1: Khai Tử Plan Mode — Thay Thế Bằng "Brief 3 Dòng" & "Visual Explainer" [00:25]
Thay vì yêu cầu Claude lập kế hoạch trước khi làm, Boris Cherny tại YC Startup School chia sẻ công thức tối giản:
* **Task (Công việc):** Nhiệm vụ cụ thể cần làm (ví dụ: Tạo báo cáo tuần từ tệp CRM export).
* **Guardrails (Rào chắn):** Những gì cấm đụng tới (ví dụ: Không sửa mẫu template có sẵn, không gửi email cho bất kỳ ai).
* **Definition of Done - DoD (Định nghĩa Hoàn thành):** Tiêu chuẩn nghiệm thu định lượng (ví dụ: Mọi số liệu trong báo cáo phải khớp 100% với tệp export).
* **Kiểm tra đầu ra:** Sau khi Claude làm xong, yêu cầu model xuất ra một bản tóm tắt trực quan 1 trang (Onepage Visual Dashboard / Mermaid Diagram) giải trình những gì nó đã làm và những giả định nó đã đưa ra.

---

### Bước 2: Bỏ Qua Prompting Cấp Độ Suy Nghĩ ("Think Hard", "Ultra Think") [02:18]
* Các từ khóa như `think hard`, `think harder`, `ultra think` giờ đây chỉ được Claude Code xử lý như những từ ngữ văn xuôi thông thường, không còn kích hoạt cơ chế cấp thêm token budget bí mật nào nữa.
* Tính năng này đã được chuyển thành thiết lập cấu hình **Effort Level**.
* **Lời khuyên của Anthropic:** Giữ nguyên mức **Recommended Effort (Medium trên Opus 5.5)**. Mức Medium trên Opus 5.5 đã vượt trội hơn Opus 5 ở mức High nhưng tốn ít bước suy luận và ít token hơn rất nhiều.

---

### Bước 3: Cung Cấp Trình Xác Thực Tất Định (Deterministic Verifier), Đừng Bắt "Double Check" [03:20]
* Viết câu *"Hãy kiểm tra lại bài làm thật kỹ (double check your work)"* vào prompt chỉ làm tốn tiền vô ích vì Opus 5.5 vốn đã tự phản tư (self-reflection) trong luồng suy luận.
* Thay vào đó, hãy viết hoặc yêu cầu Claude viết một đoạn script nhỏ (ví dụ `check_report.py` hoặc test suite) kiểm tra dữ liệu đầu ra và trả về kết quả nhị phân: **PASS hoặc FAIL**.
* Nếu bài test trả về FAIL, Claude tự đọc lỗi, tự sửa code, chạy lại test cho tới khi PASS mới nộp bài cho con người. Bạn chỉ việc đọc bằng chứng nghiệm thu (Evidence) thay vì tự tay đi test lại từng dòng.

---

### Bước 4: Vận Hành Vòng Lặp `/goal` Đúng Cách & Cảnh Báo "Thoát Non" Của Judge Haiku [05:06]
Lệnh `/goal` giúp tự động hóa thao tác gõ `continue`. Tuy nhiên, cơ chế ngầm của `/goal` chứa một cạm bẫy chí mạng:
* Sau mỗi turn, một model nhỏ (như **Haiku**) sẽ đóng vai trò **Thẩm phán (Judge)** đọc cuộc hội thoại để quyết định mục tiêu đã hoàn thành hay chưa.
* **Cạm bẫy:** Judge Haiku **KHÔNG tự chạy lệnh bash hay đọc tệp tin ngầm**. Nó chỉ đọc văn bản trong hội thoại. Nếu DoD của bạn mơ hồ (như *"báo cáo đã xong"*), Claude chỉ cần nói *"Tôi đã xong báo cáo"* là Haiku sẽ tin ngay và ngắt vòng lặp sớm (**Premature Completion**)!
* **Công thức tạo `/goal` chuẩn mực:**
  ```bash
  /goal Chạy script check_report.py đạt PASS và hiển thị kết quả. Tệp template gốc không bị thay đổi. Tự động dừng sau 15 turns nếu chưa xong.
  ```

---

### Bước 5: Tái Định Nghĩa `CLAUDE.md` — Trang Mục Lục Dưới 200 Dòng [07:06]
Khi phát hành Opus 5, Anthropic đã âm thầm **xóa sạch hơn 80% chỉ dẫn nội bộ** của Claude Code vì model mới không còn mắc những lỗi ngớ ngẩn của quá khứ.
`CLAUDE.md` không phải là nơi xả rác văn bản. Nó chỉ được đóng vai trò là **Trang Mục Lục (Contents Page)** của toàn bộ Workspace, chỉ chứa 4 nội dung bất biến:
1. Các công cụ và lệnh CLI đặc thù mà Claude không thể tự đoán.
2. Những quy ước dự án khác biệt với tiêu chuẩn thông thường.
3. Các lỗi lặp lại (gotchas) mà Claude từng mắc phải trên 2 lần.
4. Ranh giới tuyệt đối (Boundaries).

Mọi thông tin chi tiết khác phải được đẩy sang các file phụ riêng biệt.

---

### Bước 6: Bộ Ba Quy Tắc Dẫn Đường Tệp Tham Chiếu Trong `CLAUDE.md` [08:08]
1. **Dẫn đường dẫn dạng Path kèm điều kiện đọc, CẤM dùng `@import`:**
   * Dùng cú pháp `@file` sẽ ép buộc Claude nạp toàn bộ file đó vào Context Window trong **MỌI PHIÊN HỘI THOẠI**.
   * Hãy viết đường dẫn trong dấu backtick kèm điều kiện kích hoạt: ``Đọc `docs/client_brief.md` trước khi thao tác liên quan tới khách hàng``. Claude sẽ chỉ mở khi thực sự cần.
2. **Trỏ trực tiếp từ `CLAUDE.md`, cấm lồng nhánh nhiều tầng (Nested References):**
   * Tài liệu của Anthropic cảnh báo: Nếu một file chỉ được trỏ tới gián tiếp qua một file trung gian khác, Claude Code sẽ **chỉ đọc trước 100 dòng đầu tiên (preview)** của file đó, dẫn đến mất mát thông tin quan trọng phía sau!
3. **Quy tắc 100 dòng:** Bất kỳ file tài liệu tham chiếu nào dài hơn 100 dòng đều phải có mục lục tóm tắt ở đầu file.
4. **Dọn dẹp hệ thống:** Chạy lệnh `/doctor prompt audit` để tự động phát hiện đường dẫn chết, luật xung đột hoặc chỉ dẫn thừa cho model cũ.

---

### Bước 7: Xây Dựng Skill Từ Thất Bại Thực Tế, Không Phải Quy Trình Tưởng Tượng [09:50]
Hầu hết mọi người thất bại khi làm Skill vì họ tự ngồi nghĩ ra quy trình trong đầu rồi yêu cầu AI viết `SKILL.md` trước khi chạy thử.
* **Quy trình chuẩn của Anthropic:**
  1. Cho Claude chạy tác vụ thực tế trên model mục tiêu ở trạng thái **trần trụi (Không có bất kỳ Skill nào)**.
  2. Quan sát xem Claude tự làm được đến đâu và **thực sự vấp ngã ở những điểm nào**.
  3. Skill được sinh ra bằng cách **dịch ngược (Reverse Engineering)** các lỗi sai thực tế đó thành cẩm nang khắc phục (Gotcha fixers).
  *👉 Một Skill xuất sắc nhất của Anthropic thường chỉ dài vài dòng, giải quyết đúng điểm nghẽn mà model không tự vượt qua được.*

---

### Bước 8: Cú Sốc 450 Lần Chạy Thử — 7/9 Bộ Skill Nổi Tiếng Trên GitHub Không Bằng Tệp Rỗng! [12:00]
Một nhà phát triển đã tiến hành benchmark 450 lần chạy trên Opus 5.5 với 9 bộ Skill pack nổi tiếng nhất có hàng nghìn ngôi sao trên GitHub:
* **Kết quả kinh hoàng:** **7/9 bộ skill cho kết quả không hề tốt hơn (hoặc thậm chí tệ hơn) một tệp văn bản rỗng!** Chúng chỉ đóng vai trò "đồ trang trí", làm rác ngữ cảnh và tăng tiền token.
* **Cách thanh lọc hệ sinh thái:**
  * Chạy lệnh `/skill doctor` để thống kê chi phí token và tần suất sử dụng của từng skill $\to$ xóa sạch những skill không dùng.
  * Chạy lệnh `claude plugin eval` (chạy song song tác vụ khi có skill vs không có skill) để so sánh điểm số thực chất trước khi quyết định giữ lại.

---

### Bước 9: Thay Thế Chữ IN HOA Gào Thét Bằng "Deterministic Hooks" [14:35]
Việc viết chữ in hoa: *"IMPORTANT: DO NOT EMAIL THE CLIENT"* trong file quy tắc hoàn toàn vô nghĩa khi context window bị đầy (Prompt Fatigue).
* Nếu muốn đảm bảo 100% một hành động không bao giờ xảy ra: **Hãy biến nó thành Pre-tool Hook**.
* Hook là một script shell/python chạy ngắt quãng ngay trước khi tool được gọi. Nếu phát hiện lệnh gửi mail, Hook lập tức trả về mã lỗi (`exit 2`) và chặn đứng lệnh gọi công cụ mà không tốn 1 token AI nào.

---

### Bước 10: Giới Hạn Sub-Agents & Tự Động Hóa Qua Projects & Sự Kiện MCP [15:10]
* Opus 5.5 có thiên hướng rất ham chia việc cho Sub-agents. Với các tác vụ nhỏ, việc sinh quá nhiều Sub-agents sẽ nhân đôi thời gian và chi phí. Cần thiết lập giới hạn trần (cap) số lượng sub-agents trong cấu hình.
* **Chỉ định Sub-agents có chủ đích:** Chỉ ép dùng Sub-agents khi thực hiện các tác vụ nặng về tìm kiếm hoặc kết nối MCP ồn ào nhằm **giữ sạch Context Window của phiên chính (Main Chat)**.
* **Dự án chạy ngầm (Autonomous Projects):** Kết hợp tính năng Claude Code Projects chạy liên tục ngay cả khi tắt máy với các sự kiện kích hoạt (MCP Events / Webhooks / Cron timers) để biến AI thành hệ thống tự hành (ví dụ: tự động duyệt lead CRM, tự dựng carousel social media mà con người chỉ cần bấm Duyệt).

---

## Bảng Đối Chiếu 10 Lời Khuyên Cũ vs Thực Hành Mới Của Top 1%

| Yếu tố | Lời khuyên cũ (Lỗi thời) | Thực hành của Top 1% (Chuẩn mới) |
|---|---|---|
| **Lập kế hoạch** | Bật Plan Mode, đọc kế hoạch text dài trước khi code. | Brief 3 dòng (Task, Guardrails, DoD) + Bản visual 1 trang sau khi làm xong. |
| **Effort Prompting** | Gõ `think hard`, `ultra think` vào prompt. | Bỏ qua prompt, để Effort Level mặc định (Medium Opus 5.5). |
| **Xác thực chất lượng** | Nhắc model *"hãy kiểm tra lại bài thật kỹ"*. | Viết script test nhị phân PASS/FAIL, model tự sửa khi có lỗi. |
| **Vòng lặp tự động** | Gõ `continue` bằng tay liên tục. | Dùng `/goal` kèm DoD rõ ràng và giới hạn số turns. |
| **Tệp `CLAUDE.md`** | Nhồi nhét hàng nghìn dòng quy tắc và context. | Dưới 200 dòng, đóng vai trò Mục lục (TOC) trỏ path on-demand. |
| **Dẫn nguồn tài liệu** | Dùng `@file` import bừa bãi vào ngữ cảnh. | Dùng backtick path kèm điều kiện đọc, cấm trỏ gián tiếp nhiều cấp. |
| **Tạo Skill** | Ngồi tưởng tượng quy trình nghiệp vụ rồi prompt tạo skill. | Chạy thử không skill $\to$ chỉ viết skill cho những chỗ model thất bại. |
| **Dùng Skill Marketplace** | Tải hàng tá bộ skill pack nghìn sao trên GitHub. | Benchmark qua `plugin eval` & `/skill doctor`, xóa bỏ 80% skill thừa. |
| **Cấm đoán hành vi** | Viết chữ IN HOA gào thét: *"NEVER DO THIS"*. | Cài đặt Pre-tool / Stop Hooks chặn đứng bằng code tất định (exit 2). |
| **Điều phối Agent** | Dựng team agent thủ công, mở sub-agent vô tội vạ. | Giới hạn trần sub-agents, cô lập tác vụ ồn ào, chạy ngầm qua MCP events. |

---

## Actionable Checklist Áp Dụng Ngay Hôm Nay:
1. **Kiểm tra `CLAUDE.md`:** Xóa sạch các câu triết lý chung chung, rút gọn dưới 200 dòng, chuyển toàn bộ `@import` thành đường dẫn on-demand.
2. **Thanh lọc thư mục Skills:** Chạy `/skill doctor` và xóa ngay các skill chưa từng kích hoạt hoặc không vượt qua được bài test `plugin eval`.
3. **Chuyển luật cấm thành Hook:** Rà soát các dòng chữ IN HOA cấm đoán và chuyển đổi thành Pre-tool Hook với Exit Code 2.
4. **Chuẩn hóa công thức Brief:** Áp dụng cấu trúc 3 dòng (Nhiệm vụ - Rào chắn - Chuẩn nghiệm thu DoD) cho mọi yêu cầu lớn.
