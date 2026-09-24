# Make Your Coding Agents Way Faster: Jev + Google Antigravity | TypeSafe AI

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=fsEssFc-WhU)
- **Watch Skill ID:** `f62aea0c858fb6c9`
- **Kênh phát hành:** Smitha Kolan - Machine Learning Engineer
- **Category:** #ai-agents
- **Date Processed:** 2026-09-25
- **Duration:** 08:09
- **Transcript File:** [jev_google_antigravity_faster_agents_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/jev_google_antigravity_faster_agents_transcript.txt)

---

## 1. Nút thắt độ trễ (Latency Bottleneck) trong Vòng lặp Agentic

Khi xây dựng các AI Coding Agents chạy tần suất cao (High-Frequency Loops) trên Google Antigravity, Claude Code hay Codex, vấn đề lớn nhất kìm hãm tốc độ không phải là thời gian chạy lệnh bash mà là **độ trễ suy luận (Inference Latency) của LLM**:

- **Bản chất của LLM tạo sinh (Generative LLMs):** Các mô hình như Gemini, Claude, GPT được tối ưu hóa để sinh chuỗi văn bản token-by-token.
- **Lãng phí thời gian vào JSON formatting:** Ngay cả khi bật JSON Mode hay Structured Outputs, mô hình vẫn phải sinh từng ký tự ngoặc nhọn `{`, dấu ngoặc kép `"`, tên trường và giá trị.
- **Hậu quả:** Một câu hỏi nhị phân cực kỳ đơn giản trong agent loop (ví dụ: *"Lệnh shell này có an toàn để chạy tự động không?"* hoặc *"Task này cần Gemini Flash hay Gemini Pro?"*) thường ngốn từ **1.5 đến 3 giây**. Với hàng chục quyết định trung gian mỗi phiên, vòng lặp trở nên chậm chạp và ì ạch.

---

## 2. Jev là gì? Đột phá từ TypeSafe AI (typesafe.ai)

**Jev** (phát triển bởi TypeSafe AI) là một mô hình phân loại chuyên biệt (Specialized Classification Model) được thiết kế riêng cho các **quyết định phi văn bản (Non-Text Decisions)** với tốc độ xử lý siêu tốc: **~15ms đến 50ms** (nhanh gấp 50-100 lần so với LLM truyền thống).

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                          GENERATIVE LLM vs. JEV DECISION ENGINE                             │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  Mô Hình LLM Truyền Thống (Claude / Gemini):                                                │
│  [State + Prompt] ──► Token-by-token Generation ──► {"is_safe": true}                      │
│                       (Mất 1,500ms - 2,500ms, tốn token, cần parse JSON)                   │
│                                                                                             │
│  Jev Classification Engine (TypeSafe AI):                                                   │
│  [State + Questions] ──► Single Forward Pass (15ms) ──► Typed Value (Bool / Score / Choice) │
│                          (Không sinh token, không parse string, độ trễ cực tiểu)            │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

### 3 Kiểu dữ liệu nguyên bản (Typed Primitives) của Jev:
1. **`Choice`:** Chọn một phương án tối ưu trong danh sách tùy chọn định sẵn (Predefined Options).
2. **`Score`:** Đánh giá mức độ phù hợp/chất lượng của tác vụ theo thang điểm số học liên tục kèm độ tin cậy (Confidence Score).
3. **`Bool`:** Trả về giá trị nhị phân (xác suất $0.0 \to 1.0$ cho mệnh đề Đúng/Sai).

> **Tính năng nổi bật:** Jev cho phép gửi **nhiều câu hỏi song song** cùng lúc với một trạng thái (State) trong **một API call duy nhất**. Toàn bộ các câu hỏi được đánh giá đồng thời chỉ trong ~15ms.

---

## 3. Case Study: Đặt vé máy bay trong 7.1 giây

Một minh chứng ấn tượng được cộng đồng chia sẻ (bởi Gregor Zunic):
- Xây dựng Agent tự động tương tác và điều hướng trên giao diện Google Flights để book vé từ Zurich sang London.
- Thay vì để LLM nhìn ảnh và suy nghĩ từng bước mất hàng chục giây, agent sử dụng Jev để **đánh giá đồng thời toàn bộ các tọa độ click và input khả dĩ** trên màn hình chỉ trong một pass 15ms.
- Toàn bộ quy trình tìm kiếm, chọn chuyến và hoàn tất diễn ra thành công trong **7.1 giây**.

---

## 4. Hướng dẫn tích hợp Jev vào Google Antigravity

### Bước 1: Cài đặt Skill `typesafe`
Cài đặt skill chính thức vào Antigravity IDE thông qua prompt hoặc đặt thư mục skill vào `.agents/skills/typesafe` (hoặc `builtin/skills/`):
```markdown
# Cấu hình cài đặt TypeSafe AI Skill vào Google Antigravity
Tải và nạp skill.md từ TypeSafe AI repository để kích hoạt công cụ ra quyết định tốc độ cao Jev.
```

### Bước 2: Cấu hình Khóa API
Đăng ký tài khoản tại console của TypeSafe AI, lấy API Key và lưu vào file `.env` của dự án:
```bash
TYPESAFE_API_KEY=ts_live_xxxxxxxxxxxxxxxxxxxxxxxx
```

### Bước 3: Ứng dụng thực tế — Xây dựng Bộ định tuyến mô hình siêu tốc (`router.py`)
Tác giả đã triển khai một kịch bản tiêu biểu: **Model Router** chạy ngầm trong Antigravity để phân luồng prompt:

```
                  ┌───────────────────────────────┐
                  │    User Prompt / Agent Task   │
                  └──────────────┬────────────────┘
                                 │
                                 ▼
                  ┌───────────────────────────────┐
                  │   Jev Classification Engine   │  (Độ trễ: ~15ms)
                  │   Đánh giá độ phức tạp task   │
                  └──────────────┬────────────────┘
                                 │
                 ┌───────────────┴───────────────┐
                 │                               │
        [Độ phức tạp thấp]              [Độ phức tạp cao]
                 │                               │
                 ▼                               ▼
       ┌──────────────────┐            ┌──────────────────┐
       │   Gemini Flash   │            │    Gemini Pro    │
       │  (Nhanh, tiết    │            │ (Suy luận sâu,   │
       │   kiệm chi phí)  │            │  kiến trúc khó)  │
       └──────────────────┘            └──────────────────┘
```

- **Kết quả:** Phân loại yêu cầu chuẩn xác trong 15ms mà không làm nghẽn tiến trình làm việc của Antigravity IDE, tối ưu hóa cả tốc độ phản hồi lẫn chi phí token hàng tháng.

---

## 5. Các Ràng buộc & Lưu ý quan trọng khi sử dụng Jev

Mặc dù có tốc độ vượt trội, Jev có phạm vi hoạt động chuyên biệt và cần lưu ý các nguyên tắc thiết kế sau:

1. **Không thay thế LLM suy luận (Evaluation Layer Only):** Jev là tầng đánh giá/phân loại (Guardrail / Router / Verifier). Nó không có khả năng sinh code, viết tài liệu hay giải thích nguyên nhân dài dòng.
2. **Nguyên tắc Đánh giá Nguyên nghĩa (Literal Evaluation):** Jev đánh giá chính xác câu chữ trong tiêu chí được cung cấp, không suy diễn ngầm ý (no implicit implications). Tiêu chí câu hỏi phải tuyệt đối rõ ràng, không mơ hồ.
3. **Lọc trạng thái đầu vào (State Filtering):** Không nên "ném" toàn bộ file mã nguồn hàng nghìn dòng vào context của Jev. Chỉ truyền phần state/dữ liệu tối thiểu cần thiết để ra quyết định.
4. **Giới hạn phạm vi quyết định (Decision Scope):** Đóng khung câu hỏi vào dạng phân loại có cấu trúc (structured classification), điểm số (scoring) hoặc nhị phân (boolean). Tránh xa các tác vụ mở mang tính sáng tạo.

---

## 6. Tổng kết

Sự xuất hiện của các mô hình chuyên biệt như **Jev (TypeSafe AI)** đánh dấu bước chuyển mình quan trọng của kỷ nguyên Agentic: **Tách rời tầng tạo sinh (Generative Layer) và tầng ra quyết định/kiểm định (Evaluation Layer)**.

Việc kết hợp **Jev làm tầng kiểm soát siêu tốc (15ms)** cùng **Google Antigravity / Gemini / Claude làm bộ não tạo mã** giúp giảm độ trễ vòng lặp xuống mức tối đa, mang lại trải nghiệm lập trình tự động mượt mà và an toàn.
