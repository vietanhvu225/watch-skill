# Building a Harness with Jev — LangChain

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=VE5dsWll06M)
- **Watch Skill ID:** `28e972df4be755e2`
- **Kênh phát hành:** LangChain (Sydney - Open Source Product Manager)
- **Category:** #ai-agents, #jev, #langchain, #system1-ai, #evals, #agent-harness, #guardrails
- **Date Processed:** 2026-09-29
- **Duration:** 09:14 (554.0 giây)
- **Transcript File:** [building_a_harness_with_jev_langchain_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/building_a_harness_with_jev_langchain_transcript.txt)

---

## 1. Bối Cảnh Lịch Sử: Nhu Cầu Định Hình Cấu Trúc Cho Vòng Lặp Agent (The Agent Loop)

Sydney (PM mảng mã nguồn mở tại LangChain) mở đầu bằng việc tái hiện lại vòng lặp cốt lõi của Agent (Core Agent Loop):

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                           TIẾN HÓA CỦA VÒNG LẶP AGENT TẠI LANGCHAIN                         │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│  Giai đoạn 1: Text-in / Text-out thuần túy (2022 - 2023)                                    │
│  [User Prompt] ──► [LLM sinh chuỗi văn bản tự do] ──► Rất khó tương tác với mã nguồn        │
│                                                                                             │
│  Giai đoạn 2: Tool Calling & Structured Outputs (2023 - 2025)                               │
│  [User Prompt] ──► [LLM + JSON Schema] ──► [Tool Call: get_order(id=123)] ──► Trả kết quả    │
│  * Hạn chế: LLM vẫn sinh token tuần tự (Autoregressive), độ trễ cao (3s - 8s) & chi phí lớn│
│                                                                                             │
│  Giai đoạn 3: Tách tầng System 1 (Jev) & System 2 (Frontier LLM) (2026+)                    │
│  [Input State] ──► [Jev System 1: Typed Decision trong <500ms] ──► Quyết định nhánh / Chặn  │
│                     │                                                                       │
│                     └── Chỉ khi cần tư duy sâu ──► Kích hoạt Frontier LLM (System 2)        │
│                                                                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

Theo triết học nhận thức của Daniel Kahneman trong cuốn *"Thinking, Fast and Slow"*:
- **System 1 (Hệ thống 1 - Jev):** Nhanh, rẻ, phản xạ tức thì, trả về quyết định định kiểu trực tiếp cho phần mềm thực thi.
- **System 2 (Hệ thống 2 - Frontier LLM):** Chậm, đắt, tư duy từng bước trên các bài toán mở.

---

## 2. Thử Nghiệm Đối Đầu: Nhận Diện PII (Personally Identifiable Information)

LangChain làm một bản demo trực tiếp: *"Trong đoạn văn bản này có chứa thông tin định danh cá nhân (PII) không?"*
- **Frontier LLM:** Mất **~5 giây** để sinh văn bản giải thích và trả về JSON có cấu trúc.
- **Jev (TypeSafe):** Phản hồi **gần như tức thì (<200ms)** với kết quả định kiểu: `has_pii: true` kèm điểm xác suất chính xác `0.98` (98%).

---

## 3. Ba Nguyên Mẫu Câu Hỏi của Jev (The 3 Question Types)

LangChain minh họa qua một tình huống hỗ trợ khách hàng:  
*State:* `"Hi, I've been trying to connect my Stripe account for 3 days and it keeps failing. I'm losing sales. Please help ASAP."*

1. **Choice (Phân loại chọn một mục):**
   - Câu hỏi: Đội ngũ nào nên xử lý ticket này?
   - Kết quả Jev: `billing` (score: `0.84`, confidence: `0.596`).
2. **Score (Đánh giá trên thang đo liên tục):**
   - Câu hỏi: Mức độ ức chế của khách hàng từ 0 (Bình tĩnh) đến 2 (Rất giận dữ)?
   - Kết quả Jev: `1.035` (Khách hàng đang ức chế nhưng chưa tới mức mất kiểm soát).
3. **Dual (Boolean Yes/No với xác suất):**
   - Câu hỏi: Tin nhắn có tính khẩn cấp cao không?
   - Kết quả Jev: `0.999` (Xác suất khẩn cấp cực cao).

> [!IMPORTANT]
> **Đánh giá song song nhiều câu hỏi (Single State, Many Questions):**  
> Điểm độc đáo của Jev so với LLM là nhà phát triển có thể gửi **1 State kèm hàng chục câu hỏi đồng thời**, Jev sẽ giải quyết tất cả trong **một lượt suy luận song song duy nhất (Single Parallel Pass)** mà không bị tăng thời gian tuyến tính như LLM.

---

## 4. Tích Hợp Chính Thức: `langchain-typesafe`

LangChain phát hành gói tích hợp chính thức `langchain-typesafe`:

```bash
uv pip install langchain-typesafe
```

### Cách thức hoạt động trong mã nguồn:
```python
from langchain_typesafe import TypeSafeClassifier

# Khởi tạo classifier với TypeSafe API key
classifier = TypeSafeClassifier(api_key="typesafe_api_key_here")

# Truyền vào State và danh sách các câu hỏi định kiểu (Choice, Score, Dual)
response = classifier.invoke(
    state="User complaint: Stripe webhook failed to trigger after successful charge...",
    questions=[
        {"id": "routing", "type": "choice", "options": ["billing", "infra", "auth"]},
        {"id": "is_urgent", "type": "dual"},
        {"id": "sentiment", "type": "score", "min": 0, "max": 10}
    ]
)
```

---

## 5. Ba Kiến Trúc Harness Thực Chiến Được LangChain Khuyên Dùng

### Kiến Trúc 1: Dynamic Model Routing (Định Tuyến Động)
- Trong các coding agent nội bộ tại LangChain, việc gửi mọi prompt vụn vặt lên mô hình lớn gây tốn kém khủng khiếp và làm chậm tốc độ gõ code của kỹ sư.
- Sử dụng Jev làm tầng Router: phân tích prompt đầu vào dựa trên bộ tiêu chí xác định để quyết định chuyển sang **Fast/Cheap Model** hay **Frontier Model** chỉ trong chưa đầy **0.5s**.

### Kiến Trúc 2: Auto Mode Middleware (Bộ Giám Sát Tool Call Thời Gian Thực)
- **Vấn đề thực tế:** Sydney chia sẻ từng phải tắt tính năng *Auto Mode* trong coding agent vì bước kiểm tra an toàn bằng LLM quá chậm (mất 3-5 giây trước mỗi tool call khiến lập trình viên phát bực).
- **Giải pháp với Jev:** Bật lại *Auto Mode* dưới dạng Middleware chặn các Tool Call nguy hiểm (`DROP DATABASE`, `rm -rf`, ghi đè file nhạy cảm). Jev phân loại mức độ rủi ro trong **<100ms**, bảo vệ an toàn tuyệt đối mà không làm đứt đoạn mạch suy nghĩ của lập trình viên.

```
[Agent sinh Tool Call] ──► [Jev Auto Mode Middleware (<100ms)]
                                    │
                  ┌─────────────────┴─────────────────┐
                  ▼                                   ▼
          [Risk Level: LOW]                   [Risk Level: CRITICAL]
           Thực thi ngay                       Chặn đứng & Yêu cầu xác nhận
```

### Kiến Trúc 3: Jev-as-a-Judge cho Online Evals (Đánh Giá Trực Tuyến Tự Động)
- Đánh giá chất lượng của Agent trên quy mô hàng triệu trace không thể phụ thuộc vào con người, trong khi phương pháp truyền thống **LLM-as-a-Judge** quá đắt đỏ và thiếu tính nhất quán.
- **Jev-as-a-Judge:** Cung cấp bộ Rubric đánh giá (Tính chính xác, Mức độ bám sát tài liệu `groundedness`, Trích dẫn nguồn hợp lệ `cited_sources`).
- **Ưu thế được chứng minh:** Không chỉ rẻ hơn 40x-400x và nhanh hơn 20x-200x, Jev còn cho ra kết quả **đáng tin cậy và có độ nhất quán cao hơn hẳn so với LLM thông thường** qua các bài kiểm thử benchmark nội bộ của LangChain.

---

## 6. Tài Nguyên Liên Quan
- [Transcript gốc chi tiết](file:///f:/source/watch-skill/video-learning-vault/ai-agents/building_a_harness_with_jev_langchain_transcript.txt)
- Thư viện mã nguồn mở: `langchain-typesafe`
- Bài viết kỹ thuật của LangChain: *How to use Jev as a Judge for Agentic Evals*
