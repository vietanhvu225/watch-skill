# How To Build A Harness With Jev | LangChain x TypeSafe AI

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=HHUsHkYhkcM)
- **Watch Skill ID:** `d884350cc6bbb425`
- **Kênh phát hành:** LangChain
- **Category:** #ai-agents
- **Date Processed:** 2026-09-27
- **Duration:** 48:04
- **Diễn giả:** Allie (DevRel, TypeSafe AI), Hunter (Tech Lead Open Source, LangChain), Sydney (PM Open Source, LangChain)
- **Transcript File:** [how_to_build_a_harness_with_jev_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/how_to_build_a_harness_with_jev_transcript.txt)

---

## 1. Nghịch lý của Kiến trúc Agent hiện đại: Đường vòng "Ngôn ngữ tự nhiên"

Trong buổi hội thảo kỹ thuật giữa LangChain và TypeSafe AI, các diễn giả đã chỉ ra một sự phi lý đang tồn tại trong hầu hết các hệ thống AI Agent hiện nay:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                          THE NATURAL LANGUAGE DETOUR PARADOX                                │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  Hiện Trạng (Agent dùng LLM truyền thống):                                                 │
│                                                                                             │
│  [Code / Máy tính] ──► Chuyển đổi thành Câu chữ (Text/Prompt)                               │
│                                │                                                            │
│                                ▼                                                            │
│  [Mô hình LLM khổng lồ] ──► Suy luận nặng nề, sinh từng token văn bản                       │
│                                │                                                            │
│                                ▼                                                            │
│  [Structured Output / Tool Call] ──► Ép khuôn ra JSON string `{ "safe": true }`             │
│                                │                                                            │
│                                ▼                                                            │
│  [CLI / Parser] ──► Parse string ngược lại thành Mã lệnh nhị phân                           │
│                                │                                                            │
│                                ▼                                                            │
│  [Code / Máy tính] ──► Thực thi trên hệ điều hành                                           │
│                                                                                             │
│  => Chi phí cực đắt, độ trễ 1.5s - 3s cho một quyết định nhị phân hiển nhiên!               │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

Phần lớn tự động hóa trong tương lai là **giao tiếp giữa máy với máy (Machine-to-Machine Automation)**. Việc bắt máy tính nói chuyện với máy tính thông qua một mô hình sinh văn bản của con người (Prose-generating model) là một sự lãng phí tài nguyên khủng khiếp.

**Jev (TypeSafe AI)** ra đời để thiết lập lại sự chuẩn mực: **Machine-Native AI Format** — tiếp nhận State, đánh giá các câu hỏi có Schema kiểu dữ liệu rõ ràng, và trả về giá trị số học nguyên bản trong **15ms** mà không qua bất kỳ bước sinh chữ nào.

---

## 2. Hệ tư duy Nhanh & Chậm: Bản chất Code là System 2, Jev là System 1

Dựa trên cuốn sách *Thinking, Fast and Slow* của Daniel Kahneman:
- **System 1 (Jev):** Trực giác tức thì, phản xạ nhanh dưới 15ms. Một câu hỏi tốt dành cho Jev là câu hỏi mà *một hội đồng gồm những chuyên gia thông minh có thể trả lời ngay trong vòng 5 giây* mà không cần đào sâu phân tích đa tầng.
- **System 2 (Code của lập trình viên & LLM suy luận sâu):** Code chính là tầng System 2 kết nối các mảnh ghép logic. Code sẽ tổng hợp nhiều phán đoán System 1 nhanh như chớp từ Jev để đưa ra quyết định thực thi sau cùng.

---

## 3. Ba kiểu câu hỏi nguyên bản (The 3 Typed Primitives) & Quy tắc định nghĩa

```
                ┌──────────────────────────────────────────────┐
                │          JEV INPUT: [STATE OBJECT]           │
                │        + Array of Structured Questions       │
                └──────────────────────┬───────────────────────┘
                                       │
         ┌─────────────────────────────┼─────────────────────────────┐
         ▼                             ▼                             ▼
  ┌──────────────┐              ┌──────────────┐              ┌──────────────┐
  │ 1. CHOICE    │              │ 2. SCORE     │              │ 3. NOUL      │
  │ (Phân loại)  │              │ (Thang điểm) │              │ (Xác suất)   │
  └──────────────┘              └──────────────┘              └──────────────┘
```

### 1. `Choice` (Chọn 1 trong danh sách phương án)
- Bản chất tương đương một Classifier.
- Hoạt động tốt nhất khi các lựa chọn có tính phân tách rõ ràng, không bị chồng lấn quá lớn về ngữ nghĩa.
- Sử dụng cho: Phân loại phòng ban hỗ trợ, chọn tool phù hợp, định tuyến mô hình.

### 2. `Score` (Đánh giá mức độ trên một trục tiêu chí)
- Trả về giá trị liên tục từ $0 \to N$.
- **Quy tắc sinh tử:** **Bắt buộc phải mô tả ngữ nghĩa rõ ràng cho từng mức thang điểm (Level Semantic Descriptions)**.  
  *Sai lầm:* Chỉ đặt mức 0 là "Bình tĩnh" và mức 10 là "Rất tức giận". Model sẽ không thể hiểu được sự khác biệt giữa mức 6 và mức 7.  
  *Đúng đắn:* Định nghĩa rõ ràng: Mức 1 = Có chút bực bội nhưng vẫn hợp tác; Mức 2 = Bực tức rõ rệt và phàn nàn dịch vụ; Mức 3 = Gay gắt, đe dọa rời bỏ sản phẩm.
- Chỉ đo lường **duy nhất 1 chiều tiêu chí** trên mỗi Score. Tuyệt đối không gộp 2 trục thông tin vào cùng một Score (compound question).

### 3. `Noul` (Bernoulli — Xác suất $0.0 \to 1.0$)
- Đánh giá tính Đúng/Sai của một mệnh đề.
- **Cảnh báo vàng:** Không bao giờ chứa từ `OR` trong câu hỏi Noul (ví dụ: *"Tin nhắn này thể hiện sự khẩn cấp HOẶC sự tức giận"* $\to$ đây là câu hỏi ghép, model sẽ bị nhiễu).

---

## 4. Xây dựng Agent Harness với thư viện `langchain-typesafe`

Hunter (Tech Lead tại LangChain) đã trình diễn cách tích hợp Jev trực tiếp vào Middleware của LangChain Agents:

```
                      [User Prompt / Agent Goal]
                                  │
                                  ▼
                ┌───────────────────────────────────┐
                │     BEFORE_AGENT MIDDLEWARE       │
                │   (Model Router Middleware)       │ ──► [Jev: ~15ms] Phân loại độ khó
                └─────────────────┬─────────────────┘
                                  │
                  ┌───────────────┴───────────────┐
                  ▼                               ▼
            [Fast Route]                    [Deep Route]
            (Gemini Flash / Luna)           (Claude 3.7 / Sol / Pro)
                  │                               │
                  └───────────────┬───────────────┘
                                  ▼
                ┌───────────────────────────────────┐
                │          LLM REASONING            │
                │     Đề xuất gọi một Tool Call     │
                └─────────────────┬─────────────────┘
                                  │
                                  ▼
                ┌───────────────────────────────────┐
                │      AUTO MODE MIDDLEWARE         │
                │     (Tool Risk Gatekeeper)        │ ──► [Jev: ~15ms] Đánh giá độ an toàn
                └─────────────────┬─────────────────┘
                                  │
                  ┌───────────────┴───────────────┐
                  │                               │
             [SAFE TOOL]                    [RISKY TOOL]
           (Thêm CRM Note)              (Xóa Customer DB)
                  │                               │
                  ▼                               ▼
          [Thực thi ngay]             [HALT & Thao tác an toàn]
                                      Trả về Status Error /
                                      Hỏi xác nhận từ Con Người
```

### 1. `Auto Mode Middleware` (Chặn đứng Tool phá hủy / Rủi ro)
- Trước khi tool được gọi, middleware gửi tham số của tool và ngữ cảnh vào Jev.
- Nếu là tác vụ lành tính (Ghi chú nội bộ), Jev cho qua ngay lập tức.
- Nếu là tác vụ phá hủy (Xóa tài khoản khách hàng, sửa database), Jev phân loại là `Risky = True` trong 15ms $\to$ Agent chặn thực thi, bảo vệ hệ thống trước lỗi ảo giác phá hoại của LLM.

### 2. `Model Router Middleware` (Phân luồng Model tự động)
- Kiểm tra yêu cầu người dùng trước khi kích hoạt agent.
- Câu hỏi format text hoặc tác vụ đơn giản được chuyển sang mô hình siêu rẻ; câu hỏi kiến trúc khó được chuyển sang Frontier LLM.

### 3. Khả năng quan sát thời gian thực với LangSmith
- Toàn bộ các quyết định của Jev (câu hỏi, câu trả lời, phân phối xác suất, latency) được trace trực tiếp trên dashboard của LangSmith, cho phép lập trình viên debug trực quan từng bước rẽ nhánh của Agent.

---

## 5. Cuộc cách mạng Real-Time AI: Tốc độ cao + Chi phí rẻ = Mở khóa Thời gian thực

Allie nhấn mạnh một nhận thức quan trọng:
> **"Fast + Cheap unlocks Real-Time."**  
> *(Nhanh + Rẻ là chìa khóa mở ra kỷ nguyên thời gian thực.)*

- Nếu một mô hình quá chậm, nó không thể theo kịp luồng live. Nếu nó quá đắt, việc chạy liên tục mỗi mili-giây sẽ làm phá sản doanh nghiệp.
- **Minh chứng đột phá:**
  1. **Demo chơi game Doom bằng Jev:** Jev phân tích khung hình render của Doom mỗi frame, phán đoán mục tiêu quái vật cần ngắm bắn và quyết định hướng di chuyển trong thời gian thực.
  2. **Bộ lọc chat tương tác trực tiếp (Live Stream Filter):** Xử lý hàng chục nghìn comment trên luồng Twitch/YouTube trong 100ms. Cho phép người dùng vặn một thanh trượt vật lý (dial) để tinh chỉnh mức độ lọc (chỉ hiện câu hỏi chất lượng cao hoặc cho phép cả emoji/hype).

---

## 6. Cạm bẫy toán học: Đừng tin tưởng mù quáng vào `Confidence Threshold`

Một phát hiện chuyên sâu cực kỳ đắt giá được chia sẻ trong buổi thảo luận:

- **Bản chất của chỉ số `confidence`:** Giá trị `confidence` mà API trả về thực chất là một phép tính thống kê đo độ phân tán (entropy) trên bản đồ xác suất.
- **Khi nào nên dùng Confidence:** Khi bài toán kỳ vọng **chỉ có duy nhất một đáp án đúng áp đảo** (ví dụ: một email chỉ thuộc về 1 phòng ban).
- **Khi nào Confidence phản tác dụng (The Confidence Trap):**
  - Trong các bài toán có **nhiều phương án cùng tốt** (ví dụ: trong game Doom có 3 con quái vật đều đáng bắn, hoặc giỏ hàng thương mại điện tử gợi ý 3 sản phẩm đều phù hợp).
  - Khi đó, xác suất sẽ chia đều cho các phương án hàng đầu $\to$ chỉ số `confidence` tính theo độ phân tán sẽ **rất thấp**.
  - Nếu lập trình viên ngây thơ áp dụng quy tắc: *"Nếu confidence thấp thì dừng lại hoặc không hành động"* $\to$ Hệ thống sẽ bị tê liệt hoàn toàn!
- **Công thức thay thế cho lập trình viên:**
  - Lấy trực tiếp xác suất cao nhất: $\max(P)$.
  - Tính độ chênh lệch giữa vị trí số 1 và số 2: $P_{\text{top1}} - P_{\text{top2}}$.
  - Tỉ lệ tương đối: $P_{\text{top1}} / P_{\text{top2}}$.
  - Thêm hệ số suy giảm thời gian (Temporal Decay / Stickiness) để tránh việc Agent đổi mục tiêu liên tục gây rung lắc (oscillation).

---

## 7. Kỹ thuật Context Engineering & Quản lý "Model Jaggedness"

- **Giới hạn ngữ cảnh:** 32K token cho State + câu hỏi dài nhất; 64K token cho toàn bộ request.
- **Hiện tượng suy giảm độ chính xác khi Context dài (Long Context Degradation):** Nếu dồn một đống dữ liệu thừa mứa vào State, độ nhạy của Jev sẽ giảm sút.
- **Quy tắc State Filtering:** Chỉ truyền vào State những trường thông tin tối thiểu mà chuyên gia con người cần để ra quyết định.
- **Quản lý tập trung (Centralized Criteria):** Tách toàn bộ câu chữ định nghĩa (instruction, criteria) ra một file cấu hình riêng (vd: `criteria.py` hoặc YAML). Khi cần tối ưu tỷ lệ chính xác, lập trình viên chỉ cần tinh chỉnh file này mà không phải sửa logic code.
- **Công cụ `Jevify`:** Kỹ năng prompt giúp quét toàn bộ codebase hiện tại, tự động phát hiện những vị trí LLM đang bị lạm dụng sai chỗ để thay thế bằng Jev.

---

## 8. Bảng so sánh tổng hợp kiến trúc

| Tiêu chí | Generative LLM (Claude / Gemini) | Decision Engine (Jev / TypeSafe) |
|---|---|---|
| **Vị trí trong Hệ thống** | Bộ não suy luận sâu (System 2) | Tầng phản xạ & Cổng bảo vệ (System 1) |
| **Vai trò tối ưu** | Viết code, tổng hợp tài liệu, đàm thoại | Guardrail tool calls, Model routing, Triage |
| **Độ trễ phản hồi** | 1,500ms – 10,000ms | **15ms – 50ms** |
| **Rủi ro ảo giác** | Có thể bịa đặt JSON | **Không thể bịa đặt** (Type-safe Schema) |
| **Tích hợp Harness** | Đặt ở trung tâm agent loop | Đặt ở Middleware (Before/After tool & routing) |
