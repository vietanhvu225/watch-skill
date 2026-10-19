# 🏗️ Ba Cấp Độ Harness Engineering: Từ Cấu Hình Đến Tự Chế Tạo Môi Trường Agent & Swarm 3D — Agentic Labs

> **Nguồn tư liệu:** [The 3 Levels of Harness Engineering (Beginner to Pro) | Agentic Labs](https://www.youtube.com/watch?v=z1EaBe9oIME)  
> **Thời lượng video:** 16 phút 10 giây (970s) | **Toàn văn Transcript:** [the_3_levels_of_harness_engineering_agentic_labs_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/the_3_levels_of_harness_engineering_agentic_labs_transcript.txt)  
> **Chủ đề cốt lõi:** Định nghĩa bản chất: Agent = Model (Bộ não LLM) + Harness (Môi trường, công cụ, rào chắn, bộ nhớ), Ba cấp độ Harness Engineering (Level 1: Configuration, Level 2: Custom Harness, Level 3: Orchestration Harnesses & Software Factory), Thí nghiệm đột phá của LangChain (GPT-5.2 nhảy từ 53% lên 66%) và Can Bölük (Grok Code Fast 1 nhảy từ 6.7% lên gần 70% chỉ nhờ đổi harness), Vấn nạn 30,000 token rác của Claude Code bóp nghẹt mô hình cục bộ và giải pháp SMOL Coder (1,400 tokens), Mô hình hóa bầy Agent Swarm làm việc trong văn phòng 3D kết nối GitHub Issues.

---

## Executive Summary: "Agent = Model + Harness" & Bức Tường Ngăn Cách Giữa Người Dùng Và Chuyên Gia

Hầu hết lập trình viên và doanh nghiệp hiện nay lầm tưởng rằng bộ khung (coding harness) đi kèm với Claude Code, Cursor hay Codex là thứ duy nhất họ có thể sử dụng. Họ cài đặt công cụ, chỉnh vài dòng trong `AGENTS.md`, thêm vài hooks cơ bản và nghĩ rằng thế là đã làm chủ AI.

Nhưng trong thực tế, đó mới chỉ là **Cấp độ 1 (Level 1: Configuration)** của Harness Engineering — và đa số mọi người bị mắc kẹt vĩnh viễn ở đây.

Theo phân tích của Agentic Labs, **Agent không phải là một mô hình AI đơn lẻ**, mà là sự kết hợp chặt chẽ của 2 thành tố:
$$\text{AI Agent} = \text{Model (Bộ não)} + \text{Harness (Môi trường sống \& Công cụ)}$$

Khi mô hình AI (như Opus 5.5, GPT-6, Fable) đã đạt tới ngưỡng thông minh vượt bậc, **nút thắt cổ chai của toàn bộ ngành công nghiệp không còn nằm ở Model, mà nằm ở TẦNG HARNESS**. Việc thay đổi harness có thể biến một mô hình từ thất bại thảm hại thành một trợ thủ phi thường.

```
+----------------------------------------------------------------------------------------------------+
|               BA CẤP ĐỘ CỦA HARNESS ENGINEERING (TỪ CƠ BẢN ĐẾN CHUYÊN GIA)                         |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ CẤP ĐỘ 1: CẤU HÌNH (CONFIGURATION) ] ──────────────────────────────────────────> 90% NGƯỜI DÙNG  |
|    • Dùng harness có sẵn (Claude Code, Cursor, Codex).                                             |
|    • Tinh chỉnh file quy tắc `AGENTS.md`, phân tách tệp theo thư mục `docs/`.                      |
|    • Viết tài liệu bàn giao (Handoff documents) khi chạm trần Context Window.                     |
|    • Cài Stop Hook kiểm tra bắt buộc cập nhật CHANGELOG.md và chạy test suite.                     |
|                                     │                                                              |
|                                     ▼                                                              |
|  [ CẤP ĐỘ 2: TỰ CHẾ TẠO HARNESS (CUSTOM HARNESSES) ] ─────────────────────────────> DOANH NGHIỆP   |
|    • Tự viết môi trường riêng để chống Vendor Lock-in (chạy linh hoạt OpenAI, Anthropic, Ollama).   |
|    • Giải quyết nút thắt Context: Claude Code ngốn 30,000 tokens chỉ để nạp harness rác!           |
|    • Xây dựng SMOL Coder: Nén toàn bộ harness xuống chỉ còn 1,400 tokens cho Open-Weight Models.  |
|    • Nâng vọt điểm chuẩn: Can Bölük đẩy Grok Code Fast 1 từ 6.7% lên 70% chỉ nhờ đổi harness!     |
|                                     │                                                              |
|                                     ▼                                                              |
|  [ CẤP ĐỘ 3: ĐIỀU PHỐI ĐA HARNESS & NHÀ MÁY PHẦN MỀM (ORCHESTRATION & FACTORIES) ] > KIẾN TRÚC SƯ  |
|    • AutoForge: Harness tự hành dài hạn (Long-running), tự gắp hàng trăm task từ Kanban board.     |
|    • Software Factory Swarm: Bầy 10-20 agents (Claude, Codex, Local) tự chia luồng làm việc.      |
|    • Trực quan hóa 3D Swarm: Bầy agent làm việc trong văn phòng 3D, click vào màn hình ra PR Git.  |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 1. Cấp Độ 1: Cấu Hình Harness (Configuration Engineering) [03:57]

Ở cấp độ này, bạn tận dụng tối đa các tính năng của harness có sẵn mà không can thiệp vào tầng lõi:

### 1. Chuẩn Hóa Bộ Nhớ Với Chuẩn Mở `AGENTS.md`:
* `AGENTS.md` là chuẩn mở được mọi coding harness cao cấp hỗ trợ.
* **Nguyên tắc phân rã (Composability):** Không dồn hàng nghìn dòng vào 1 file markdown duy nhất. Hãy tạo thư mục `docs/` chứa các file chỉ dẫn chuyên biệt (UI, Database, Auth...) và dùng `AGENTS.md` làm trang mục lục trỏ tới.
* **Tài liệu Handoff (Handoff Documents) [06:10]:** Khi phiên hội thoại chạm trần Context Window, yêu cầu agent tóm tắt toàn bộ tiến độ, kiến trúc và các việc còn dang dở vào một tệp `handoff.md`. Mở phiên mới chỉ cần nạp tệp này để tiếp tục ngay lập tức.

### 2. Cưỡng Chế Hành Vi Bằng Stop Hooks [06:40]:
Thay vì van xin model trong prompt, hãy dùng **Stop Hook**:
* Mỗi khi agent hoàn thành công việc và định trả quyền điều khiển cho người dùng, Stop Hook tự động kích hoạt một script kiểm tra.
* Nếu phát hiện agent chưa cập nhật `CHANGELOG.md` hoặc chưa chạy unit test, script lập tức chặn đứng phiên làm việc và ra lệnh: *"Bạn chưa cập nhật changelog, hãy quay lại làm ngay!"*

---

## 2. Cấp Độ 2: Tự Xây Dựng Harness Độc Lập (Custom Harnesses) [07:54]

Đây là bước chuyển biến phân định giữa một lập trình viên thông thường và một **AI Engineer thực thụ**.

### Bằng chứng thực nghiệm: Đổi Harness Làm Thay Đổi Số Phận Model [08:45]:
* **Thí nghiệm của LangChain:** Cùng một model GPT-5.2 với cùng một prompt, khi chạy trên harness thông thường chỉ đạt **53%**, nhưng khi chuyển sang chạy trên harness **Deep Agents CLI** của LangChain, kết quả nhảy vọt lên **66%**.
* **Thí nghiệm của Can Bölük:** Model Grok Code Fast 1 ban đầu bị coi là phế phẩm với tỷ lệ thành công chỉ **6.7%**. Sau khi Can Bölük viết lại toàn bộ công cụ nền tảng trong harness, điểm chuẩn vọt lên gần **70%**!
* 👉 **Kết luận:** *Không phải model ngu ngốc, mà chính cái Harness là nút thắt cổ chai kìm hãm trí tuệ nhân tạo!*

### Cứu Tinh Cho Open-Weight Models: Dự Án SMOL Coder [10:30]:
* **Nghịch lý Claude Code:** Khi khởi động một phiên Claude Code, ngay trước khi bạn gõ ký tự đầu tiên, hệ thống đã nhồi nhét **hơn 30,000 tokens** vào Context Window (gồm system prompt, hướng dẫn tool, định nghĩa memory).
* **Thảm họa với Local Models:** Các model mã nguồn mở chạy trên máy cá nhân (qua Ollama, LM Studio) thường chỉ có bộ nhớ ngữ cảnh 32k hoặc 64k tokens. Việc bị harness chiếm mất 30k tokens khiến model bị nghẹt thở, mất sạch trí nhớ và sinh mã lỗi.
* **Giải pháp SMOL Coder:** Tác giả tự viết một harness siêu nhẹ:
  * Loại bỏ hoàn toàn các công cụ thừa thãi như MCP hay Sub-agents phức tạp.
  * Tối ưu hóa định nghĩa tool và system prompt xuống vỏn vẹn **1,400 tokens**!
  * Tự động nhận diện mô hình chạy trên Ollama/LM Studio mà không cần cấu hình phức tạp.

---

## 3. Cấp Độ 3: Điều Phối Đa Harness & Nhà Máy Phần Mềm (Orchestration & Software Factory) [13:00]

Ở đỉnh cao của Harness Engineering, người kỹ sư trở thành một **Kiến Trúc Sư Nhà Máy (Factory Architect)** điều hành một bầy đàn AI đa chủng loại:

```
+────────────────────────────────────────────────────────────────────────────────────────────────────+
|                     MÔ HÌNH NHÀ MÁY PHẦN MỀM (SOFTWARE FACTORY AGENT SWARM)                         |
+────────────────────────────────────────────────────────────────────────────────────────────────────+
|                                                                                                    |
|  [ KANBAN BOARD / GITHUB ISSUES ] ─── Hàng trăm tính năng & bug tickets cần xử lý                  |
|                 │                                                                                  |
|                 ▼                                                                                  |
|  [ ORCHESTRATION HARNESS (BỘ ĐIỀU PHỐI TRUNG TÂM) ]                                                |
|    • Phân tích độ phức tạp của từng ticket.                                                        |
|    • Quản trị hàng đợi: Nếu toàn bộ bầy agent đang bận, tạm giữ task chờ giải phóng tài nguyên.   |
|                 │                                                                                  |
|     ┌───────────┼───────────────────────────┬───────────────────────────┐                          |
|     ▼           ▼                           ▼                           ▼                          |
| [ WORKER 1 ]  [ WORKER 2 ]            [ WORKER 3 ]                [ WORKER N... ]                  |
| Claude Code   OpenAI Codex            Custom SMOL Coder           DeepSeek Harness                 |
| (Viết Logic)  (Viết Unit Tests)       (Sửa CSS / Docs cục bộ)     (Rà soát An ninh)                |
|     │           │                           │                           │                          |
|     └───────────┴───────────────────────────┴───────────────────────────┘                          |
|                 │                                                                                  |
|                 ▼                                                                                  |
|  [ AUTONOMOUS VERIFICATION & AUTO-PR ] ─── Tạo Pull Request thật trên GitHub kèm video/logs        |
|                                                                                                    |
+────────────────────────────────────────────────────────────────────────────────────────────────────+
```

### Dự Án 3D Office Agent Swarm [14:50]:
Tác giả đã trực quan hóa toàn bộ bầy Agent Swarm dưới dạng một **văn phòng ảo 3D mã nguồn mở**:
* Mỗi con bot trong không gian 3D đại diện cho một agent đang chạy ngầm trên máy tính (con chạy Claude Code, con chạy Codex).
* Người dùng có thể di chuyển nhân vật đến từng bàn làm việc, click vào màn hình của bot để xem mã nguồn đang được viết trong thời gian thực và liên kết trực tiếp tới Pull Request tương ứng trên GitHub.

---

## Actionable Takeaways Cho Lập Trình Viên:
1. **Làm chủ Level 1 ngay hôm nay:** Chia nhỏ `AGENTS.md`, đưa tài liệu chi tiết vào thư mục `docs/`, và thiết lập Stop Hook để tự động hóa kỷ luật kiểm thử.
2. **Học cách tự viết Custom Harness:** Đừng phụ thuộc vào một nhà cung cấp duy nhất. Tự xây dựng harness giúp bạn kiểm soát hoàn toàn bảo mật doanh nghiệp và cắt giảm hàng chục nghìn tokens lãng phí.
3. **Giải cứu Local Models:** Nếu chạy mô hình mã nguồn mở trên Ollama/LM Studio, tuyệt đối không dùng harness của Claude Code; hãy dùng các harness tối giản như SMOL Coder (1,400 tokens) để bảo tồn Context Window.
4. **Tiến tới mô hình Software Factory:** Tách rời khâu lập kế hoạch (Kanban) khỏi khâu gõ code, để bầy Agent tự động chia nhau cày cuốc hàng trăm issues chạy ngầm trong background.
