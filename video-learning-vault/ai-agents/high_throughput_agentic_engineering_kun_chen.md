# High Throughput Agentic Engineering with Kun — Kun Chen

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=MSbacZ99E14)
- **Watch Skill ID:** `dfeda6082b840242`
- **Kênh phát hành:** Kun Chen (Cựu L8 Principal Engineer tại Meta, Microsoft, Atlassian)
- **Category:** #ai-agents, #agentic-engineering, #multi-agent, #high-throughput, #first-mate, #orchestration, #production-ai
- **Date Processed:** 2026-09-29
- **Duration:** 53:57 (3237.0 giây)
- **Transcript File:** [high_throughput_agentic_engineering_kun_chen_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/high_throughput_agentic_engineering_kun_chen_transcript.txt)

---

## 1. Triết Lý & Vấn Đề Cốt Tử Của Kỹ Nghệ Tác Nhân Đa Nhiệm (High-Throughput Multitasking)

Kun Chen mở đầu bài giảng bằng việc chỉ ra nút thắt cổ chai lớn nhất trong kỷ nguyên AI: **Băng thông chú ý và thời gian của con người (Human Attention & Bandwidth)**:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                          MÔ HÌNH QUẢN TRỊ 36+ DỰ ÁN CỦA KUN CHEN                             │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│                                    [ CAPTAIN (Human) ]                                      │
│                                (Giao tiếp bằng Giọng nói)                                   │
│                                             │                                               │
│                                             ▼                                               │
│                                  [ FIRST MATE (Root Agent) ]                                │
│                     (Session duy nhất - Nắm toàn bộ ngữ cảnh & mục tiêu)                    │
│                                             │                                               │
│                        ┌────────────────────┴────────────────────┐                          │
│                        ▼                                         ▼                          │
│             [ SECOND MATE (Ship App) ]               [ SECOND MATE (Wallet App) ]           │
│             (Sub-Orchestrator phân nhánh)            (Sub-Orchestrator phân nhánh)          │
│                        │                                         │                          │
│          ┌─────────────┴─────────────┐                           ▼                          │
│          ▼                           ▼               [ CREWMATE: iOS Feature ]              │
│  [ CREWMATE: UI Fix ]      [ CREWMATE: Bug Triage ]    (Model: Claude Fable)                │
│  (Model: Claude Fable)      (Model: Grok 4.5 / Luna)                                        │
│                                                                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

> *"Nếu bạn phải nhảy qua lại giữa 10 cửa sổ terminal, theo dõi từng lệnh tool call và lo lắng từng chút một về context window, bạn sẽ kiệt sức vì context-switching. Kỹ sư trưởng chỉ nên tương tác với một Agent duy nhất (First Mate). Toàn bộ việc điều phối, phân rã bài toán và quản trị đội tàu do First Mate đảm nhiệm."*

---

## 2. Hệ Thống Hạ Tầng & Công Cụ Cốt Lõi (The Tooling Stack)

Kun Chen xây dựng một bộ công cụ mã nguồn mở hoàn chỉnh để hiện thực hóa quy trình này:

### A. Herder — Bộ Ghép Kênh Phiên Đa Máy (Next-Gen Multi-Machine Multiplexer)
- Tiến hóa vượt bậc so với `tmux`: Herder quản lý hàng chục phiên làm việc của Agent trên **nhiều phần cứng vật lý khác nhau**.
- **Mô hình Hybrid Hardware:**
  - *MacBook cục bộ (Local):* Chạy giao diện tương tác và session của First Mate.
  - *Mac Mini Headless (Trên kệ tủ):* Chạy không màn hình/chuột, chịu tải toàn bộ các Second Mates và những tác vụ build/test nặng nhọc của Crewmates.

### B. First Mate & Second Mates (Mô Hình Phân Cấp Hàng Hải)
- **First Mate:** Agent cấp điều hành tối cao. Không bao giờ tự tay viết từng dòng code để luôn rảnh tay nhận lệnh tiếp theo từ Captain.
- **Second Mates:** Khi một dự án phình to (như app `Ship` có quá nhiều bug report và tính năng), First Mate ủy quyền cho một *Second Mate* chuyên trách quản lý riêng kho mã nguồn đó.

### C. Tiện Ích Mở Rộng Calm Mode (`/calm`)
- Mặc định ẩn sạch toàn bộ các dòng log công cụ (tool calls: đọc file, quét thư mục, suy nghĩ lan man).
- Chỉ hiển thị một biểu tượng chiếc thuyền nhỏ đang trôi nổi nhẹ nhàng trên màn hình. Kỹ sư giữ được sự tĩnh tâm tuyệt đối để tư duy chiến lược thay vì bị ngợp bởi ma trận log. Khi cần debug, gõ `/calm` để bật/tắt chi tiết.

### D. QuotaRX — Điều Phối Thông Minh Theo Ngân Sách Quota (Quota-Aware Dispatch)
- Một công cụ quản lý hạn ngạch (quota) từ Claude (Anthropic), Codex (OpenAI), Cursor và Grok.
- First Mate đọc API từ QuotaRX: Khi cần phân phối việc cho Crewmate, nó tự động chọn nhà cung cấp nào đang còn nhiều quota nhất trong chu kỳ tuần, triệt tiêu 100% tình trạng nghẽn do chạm Rate Limit.

---

## 3. Quy Tắc Phân Bổ Mô Hình Cực Kỳ Chuẩn Xác (`crew_dispatch.json`)

Kun Chen cấu hình tự động bộ quy tắc phân phối tác vụ cho từng mô hình:

```json
{
  "rules": [
    {
      "condition": "Giao diện người dùng & App iOS trả phí",
      "model": "Claude Fable",
      "reason": "Khả năng cảm thụ thẩm mỹ UI và xử lý tương tác hoàn hảo nhất"
    },
    {
      "condition": "Tác vụ cần sinh tài nguyên hình ảnh",
      "model": "Codex + GPT-5.6",
      "reason": "Codex tích hợp sẵn công cụ gọi model sinh ảnh native của OpenAI"
    },
    {
      "condition": "Thiết kế kiến trúc, quy hoạch hệ thống phức tạp",
      "model": ["Claude Fable", "Kimi K3", "GPT-6 Astra"],
      "reason": "Khả năng lập luận sâu đa chiều trên các bài toán mở"
    },
    {
      "condition": "Sửa lỗi bug đơn giản, đã rõ nguyên nhân",
      "model": ["GPT-5.6 Luna", "Claude Sonnet", "Cursor Grok 4.6"],
      "reason": "Tốc độ phản hồi cực nhanh, tiết kiệm chi phí tối đa"
    }
  ]
}
```

---

## 4. Kiểm Thử Trực Quan & Vòng Lặp Quyết Định: Lavish, Bearings & Ahoy

### A. Lavish — Bản Trình Diễn Tương Tác HTML (Visual Validation Artifacts)
- Thay vì để Agent mô tả bằng văn bản hay đưa mã code thô, Kun Chen yêu cầu Crewmates xuất ra **Lavish Artifact** (trang HTML tương tác).
- *Ví dụ thực tế trong video:*
  1. *Fly With Me (Game bay lượn sinh thế giới ngẫu nhiên):* Agent đề xuất 5 loại chim (Cú đêm, Chim ưng, Cò cổ dài...) kèm màu sắc và 3 mẫu núi tuyết. Kun mở Lavish xem trực tiếp, so sánh với ảnh thực tế đỉnh Everest/K2 và bấm chọn phiên bản ưng ý.
  2. Phản hồi của Captain gửi thẳng ngược lại cho Crewmate thực thi mà không làm phiền First Mate.

### B. Bearings & "Captain's Call" — Bảng Điều Khiển Ra Quyết Định Tập Trung
- Khi quản lý 36 dự án, có hàng chục quyết định treo chưa được chốt.
- Kỹ năng `/bearings lavish` quét toàn bộ hạm đội và dựng một bảng tương tác:
  - *What's charted next:* Việc chuẩn bị làm.
  - *Currently underway:* Việc đang chạy.
  - *Recently landed:* Việc vừa hoàn thành.
  - *Captain's Call:* Danh sách các lựa chọn cần Captain phê duyệt (Duyệt làm ngay, Tạm hoãn, hoặc Bỏ qua).

### C. Kỹ Năng `/ahoy` — Bắt Kịp Nhịp Độ Trong 3 Giây
- Khi Captain quay lại màn hình sau giờ giải lao, gõ `/ahoy`: First Mate tóm tắt toàn bộ diễn biến vừa xảy ra, chỉ ra việc gì đã merge, việc gì gặp lỗi GitHub và việc gì đang chờ quyết định.

---

## 5. Phễu Tiếp Nhận Đa Kênh (Omnichannel Ingress) & Chính Sách PR

### A. Nhúng Agent Vào Discord & Mạng Xã Hội X
- Kun Chen kết nối bot First Mate vào kênh `#bug-reports` trên Discord và X (Twitter):
  - Người dùng báo lỗi trên Discord $\to$ Kun chỉ cần tag `@FirstMate investigate this`.
  - Bot tự động relay về phiên làm việc cục bộ của First Mate, First Mate cử Crewmate điều tra, tìm ra nguyên nhân (ví dụ: cờ hook bị drop do chính sách bảo mật), sau đó bot tự động đăng câu trả lời giải thích lại lên Discord!

### B. Chính Sách PR: Phân Cấp An Toàn (`projects.md`)
Kun Chen thiết lập chính sách kiểm duyệt riêng cho từng dự án dựa trên mức độ rủi ro:

1. **Chính Sách `Direct PR + YOLO Merge`:**
   - Áp dụng cho các dự án phụ, game thử nghiệm cá nhân (`fly-with-me`).
   - Agent tự kiểm tra, tự tạo PR và tự động Merge thẳng vào `main` nếu thấy ổn thỏa, không cần con người phê duyệt.
2. **Chính Sách `No Mistakes Pipeline`:**
   - Áp dụng cho dự án trọng yếu (Production/Paid apps).
   - Chạy quy trình kiểm thử đối kháng (Adversarial Review), thực thi 9/10 kịch bản kiểm chứng thực tế và lập báo cáo rủi ro (Risk Assessment: Low / Medium / High).
   - Quy tắc của Kun: *"Nếu là thay đổi nhỏ có đánh giá rủi ro Low và đã pass bài test, tôi thậm chí không buồn đọc code mà merge luôn. Chỉ mở code khi Risk Assessment từ Medium trở lên!"*

---

## 6. Bài Học Đắt Giá Về Quản Trị Context Window

Kun Chen phản đối việc lập trình viên tốn quá nhiều nơ-ron não vào việc canh cánh tối ưu hóa context window thủ công:
- **Tài nguyên quý giá nhất là Sự tập trung của con người.** Nếu não bạn lúc nào cũng lo lắng khi nào nên compact context, bạn sẽ không còn năng lượng để nghĩ xem *Nên xây dựng cái gì tiếp theo*.
- **Thiết lập ngưỡng tự động (Auto-Compaction Threshold):**  
  Cài đặt biến môi trường tự động nén context khi chạm ngưỡng:
  ```bash
  export CLAUDE_CODE_AUTO_COMPACT_WINDOW=500000
  ```
  Để hệ thống tự động compact ở mức 500k tokens và quên nó đi. Hãy dồn toàn lực vào bài toán nghiệp vụ và sản phẩm.

---

## 7. Tài Nguyên & Tham Khảo
- [Transcript gốc chi tiết](file:///f:/source/watch-skill/video-learning-vault/ai-agents/high_throughput_agentic_engineering_kun_chen_transcript.txt)
- Các dự án mã nguồn mở của Kun Chen: `first-mate`, `herder`, `quotarx`, `lavish`
- Cộng đồng Discord: *Built with Kun*
