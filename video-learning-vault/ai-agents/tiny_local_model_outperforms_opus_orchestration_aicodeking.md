# Tự Chế Mô Hình Cục Bộ 31MB Đánh Bại Cả Opus: Bí Thuật Điều Phối Tác Nhân (PandaOrchestrator & Panda Router) | AICodeKing

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=Xa0Zhl8ljPY)
- **Video ID:** `Xa0Zhl8ljPY`
- **Category:** #ai-agents
- **Date Processed:** 2026-10-20
- **Duration:** 14:09
- **Speaker:** AICodeKing (Nhà phát triển mã nguồn mở dự án PandaCode & PandaOrchestrator)
- **Transcript File:** [tiny_local_model_outperforms_opus_orchestration_aicodeking_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/tiny_local_model_outperforms_opus_orchestration_aicodeking_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong các hệ sinh thái lập trình đa tác nhân (Multi-Agent Coding Teams), mô hình kinh điển là: **Một tác nhân Kiến trúc sư (Architect)** phân tích yêu cầu lớn thành các nhiệm vụ con, sau đó điều phối cho các **Tác nhân Thực thi (Worker Agents)** chạy song song. Tuy nhiên, kiến trúc này luôn vấp phải một tử huyệt nghiêm trọng: **Architect phân bổ mô hình cực kỳ tồi tệ**. 
- Hoặc nó gán thẳng các mô hình khổng lồ đắt đỏ như Claude Opus / GPT-4o cho mọi tác vụ (đốt cạn quota token chỉ để đổi tên biến).
- Hoặc nó chọn bừa một model giá rẻ cho tác vụ kiến trúc phức tạp, dẫn đến lỗi dây chuyền và con người phải dọn dẹp thủ công.

Trong video này, **AICodeKing** công bố giải pháp mã nguồn mở đột phá: **PandaOrchestrator** và **Panda Router** — một mô hình ra quyết định cục bộ siêu nhỏ (**32M tham số, dung lượng nén INT8 chỉ 31 MB, chạy trên CPU mất 10–25ms, chi phí huấn luyện vỏn vẹn $50**).

Kết quả thực nghiệm trên bộ 150 bài toán kiểm thử độc lập (Held-out benchmark):
- Dùng một model cố định đắt tiền (GPT-6.1 / Opus): Giải được **79/100** tác vụ, chi phí **$1.11 / 100 tác vụ**.
- Panda Router chọn 1 lần duy nhất: Giải được **77/100** tác vụ, chi phí chỉ **$1.00 / 100 tác vụ**.
- **Panda Router kết hợp Thang Leo Bậc (Escalation Ladder - tối đa 2 lần thử lại): Đạt tỷ lệ giải thành công 91/100 tác vụ!** (Tiệm cận mức trần hoàn hảo 94/100 của Oracle lý thuyết, trong khi tiết kiệm 60–80% chi phí).

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                    PANDA ROUTER: JUDGMENT IN MODEL, POLICY IN CODE                          │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│     USER TASK ("Add dark mode with settings toggle & pass tests")                           │
│                              │                                                              │
│                              ▼                                                              │
│     ┌─────────────────────────────────────────────────────────┐                             │
│     │ PANDA ROUTER (Tiny 31MB ONNX Model - 10-25ms CPU Local) │                             │
│     │ - Category: Frontend / Logic / Testing                  │                             │
│     │ - Difficulty: [1 to 10] (Lopsided Loss - Thà đoán khó)  │                             │
│     │ - Required Skills: [CSS, React State, Jest]             │                             │
│     │ - Flags: [needs_tools: true, needs_images: false]       │                             │
│     └────────────────────────┬────────────────────────────────┘                             │
│                              │ Task Profile (JSON)                                          │
│                              ▼                                                              │
│     ┌─────────────────────────────────────────────────────────┐   MODEL CARDS (Plain Data)  │
│     │ DETERMINISTIC POLICY ENGINE (Rust Code - No Neural Net) │◄──[Haiku: $0.25, Lat: 400ms]│
│     │ - Compute Expected Cost = Cost + P(fail)*RetryCost     │   [DeepSeek: $0.14, Math: 9]│
│     │ - Pessimistic Success Head (Lower Bound 95% CI)         │   [Sonnet: $3.00, Code: 9.5]│
│     │ - User Tier List Weight (70% Tier + 30% Model Rating)   │   [Opus: $15.00, Arch: 10]  │
│     └────────────────────────┬────────────────────────────────┘                             │
│                              │                                                              │
│                              ▼                                                              │
│     ┌─────────────────────────────────────────────────────────┐                             │
│     │ ESCALATION LADDER (Thang leo bậc - Không bao giờ lùi):  │                             │
│     │ 1. Rung 1: Haiku 4.5  ──(Chạy thử & Test)──► [PASS] ──► (Xong với chi phí rẻ nhất)   │
│     │                                       │                                               │
│     │                                     [FAIL]                                            │
│     │                                       ▼                                               │
│     │ 2. Rung 2: DeepSeek Flash / Kimi ──(Test)──► [PASS]                                   │
│     │                                       │                                               │
│     │                                     [FAIL]                                            │
│     │                                       ▼                                               │
│     │ 3. Rung 3: Claude Sonnet 4.5 ───────(Test)──► [PASS] (Độ chính xác nhảy vọt 91%)     │
│     └─────────────────────────────────────────────────────────┘                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Triết Lý Thiết Kế Lõi: "Phán Đoán Trong Model, Chính Sách Trong Code"

Thay vì cố gắng huấn luyện một mô hình ngôn ngữ lớn để nó trả về tên model (vốn dễ bị ảo giác và không thể test hồi quy), AICodeKing phân tách bài toán định tuyến thành 2 tầng rõ rệt:

### 1. Phán đoán định tính (Qualitative Judgment) thuộc về Neural Network
- Model chỉ làm đúng một việc: Đọc văn bản tác vụ và trích xuất một bản **Hồ sơ tác vụ (Task Profile)** chuẩn hóa gồm:
  - **Category:** Coding, Refactoring, Documentation, Architecture, DevOps.
  - **Difficulty:** Điểm từ 1 đến 10.
  - **Skills:** Bộ kỹ năng cần thiết (ví dụ: `[sql, regex, frontend, concurrency]`).
  - **Flags:** Có cần công cụ bash, có cần xử lý ảnh, có mơ hồ hay không.

### 2. Chính sách định tuyến (Routing Policy) thuộc về Code thuần chủng (Deterministic Code)
- Thuật toán bằng code (Rust/Python) nhận Task Profile và chấm điểm tất cả các model khả dụng dựa trên:
  $$\text{Score} = f(\text{P(Success)}, \text{Expected Cost}, \text{Latency})$$
- **Lợi ích kiến trúc vô giá:**
  - **Khả năng kiểm thử (Testability):** Nếu router chọn sai model cho một tác vụ, lập trình viên có thể debug từng dòng code thuật toán chấm điểm và sửa rule, thay vì phải fine-tune lại trọng số mạng nơ-ron.
  - **Mô hình chỉ là Dữ liệu (Models are just Data):** Mỗi LLM trên thị trường được mô tả bằng một file JSON nhỏ (**Model Card**) chứa giá token, cửa sổ ngữ cảnh, độ trễ và điểm kỹ năng. Khi OpenAI hay Anthropic ra model mới, chỉ cần thêm 1 file JSON mà không cần huấn luyện lại Router!

---

## 3. Quy Trình Huấn Luyện Panda Router Với Ngân Sách "Con Nhà Nghèo" ($50)

Toàn bộ quy trình huấn luyện Router được thực hiện với chi phí cực thấp nhưng áp dụng các kỹ thuật chưng cất tri thức (Knowledge Distillation) bài bản:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                    BUDGET TRAINING PIPELINE ($50 TOTAL COST)                                │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│  [1. Labeling Dataset ($2.77)]                                                              │
│  - DeepSeek-V4-1-Flash gán nhãn 8,000 tasks (Category, Skills, Difficulty ban đầu).        │
│                                                                                             │
│  [2. Attempt & Grade Ground Truth ($44)]                                                    │
│  - Chạy thực tế các models thật trên 8,000 tasks.                                           │
│  - Strict Judge chấm điểm nhị phân PASS / FAIL (Outcome-based empirical data).              │
│                                                                                             │
│  [3. Architecture & Distillation ($1.67 trên Modal)]                                        │
│  - Họ mô hình Ettin (MIT License, encoder-only, chung tokenizer).                           │
│  - Teacher Model (150M params) ──(Distill)──► Student Model (32M params).                   │
│  - Lượng tử hoá INT8 ──► Xuất ONNX File 31 MB (Chạy CPU 10-25ms).                          │
│                                                                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

### Các Kỹ Thuật Độc Đáo Trong Lúc Huấn Luyện (Training Tricks)

1. **Hàm mất mát độ khó lệch bên (Lopsided Difficulty Loss):**
   - Trong quá trình học, nếu router đoán sai độ khó của bài toán:
     - Đoán một bài dễ thành khó: Phạt nhẹ.
     - Đoán một bài khó thành dễ: Phạt cực nặng!
   - *Lý do kinh tế:* Đánh giá thấp độ khó là sai lầm đắt giá nhất, vì hệ thống sẽ giao bài khó cho model yếu, gây hỏng việc và tốn tiền retry.
2. **Fine-tuning trên 1,500 yêu cầu mơ hồ (Vague Requests):**
   - Các prompt như *"hãy làm app này xịn hơn"* hoặc *"tối ưu code giúp tôi"* không có phạm vi cụ thể.
   - Router được huấn luyện có chủ đích để xếp các task mơ hồ vào nhóm **Độ khó cao (High Difficulty)**, bởi vì sự mơ hồ đòi hỏi suy luận logic bậc cao nhất để tự suy đoán ý đồ người dùng.
3. **Pessimistic Success Head (Đầu ra thành công bi quan):**
   - Thay vì dùng xác suất thành công trung bình (optimistic average), mạng nơ-ron xuất ra **Cận dưới khoảng tin cậy (Lower Bound)**. Router chỉ tin vào cận dưới bi quan, tránh đặt cược vào những model "chỉ giỏi trên lý thuyết".
4. **Expected Cost Modeling:**
   - Chi phí kỳ vọng của một model tính cả xác suất thất bại:
     $$\text{Expected Cost} = \text{Cost}_{\text{initial}} + P(\text{fail}) \times \text{Cost}_{\text{escalated\_retry}}$$
   - Một model siêu rẻ nhưng tỉ lệ xịt 50% thực chất có chi phí kỳ vọng đắt hơn một model tầm trung chạy 1 lần là đạt!

---

## 4. Đột Phá Kiến Trúc: Thang Leo Bậc (Escalation Ladder)

Thang leo bậc là cơ chế đưa tỷ lệ giải quyết bài toán từ **77% lên 91%**:

```
                       ┌───────────────────────────────┐
                       │ Router sinh Escalation Ladder │
                       │    [Model A ➔ Model B ➔ Model C]│
                       └───────────────┬───────────────┘
                                       │
                                       ▼
                       ┌───────────────────────────────┐
                       │ Worker chạy trên Model A      │
                       │ (Model rẻ nhất có cơ hội Pass)│
                       └───────────────┬───────────────┘
                                       │
                                       ▼
                       ┌───────────────────────────────┐
                       │ Deterministic Verifier        │
                       │ (Chạy unit tests, typecheck,  │
                       │  hoặc linter review)          │
                       └───────────────┬───────────────┘
                                       │
                       ┌───────────────┴───────────────┐
                       │                               │
                 [Kết quả PASS]                  [Kết quả FAIL]
                       │                               │
                       ▼                               ▼
         ┌───────────────────────────┐   ┌───────────────────────────┐
         │ Hoàn thành tác vụ         │   │ BƯỚC LÊN BẬC TIẾP THEO    │
         │ Chi phí tối thiểu         │   │ (Model B mạnh hơn hẳn)    │
         └───────────────────────────┘   │ *NGUYÊN TẮC: KHÔNG LÙI BẬC│
                                         └─────────────┬─────────────┘
                                                       │
                                                       ▼
                                         [Thử tối đa 2 lần retry]
                                         ➔ Tránh vòng lặp vô tận đốt tiền
```

### Bảng Số Liệu Kiểm Thử Thực Tế (150 Held-Out Tasks, 9 Models Pool)

| Phương Pháp Điều Phối | Tỷ Lệ Giải Thành Công | Chi Phí / 100 Tasks | Nhận Xét |
| :--- | :---: | :---: | :--- |
| **Always Flagship (GPT-6.1 / Opus)** | 79 / 100 | $1.11 | Đắt đỏ, lãng phí tài nguyên cho bài toán dễ |
| **Panda Router (1 Single Pick - No Retry)** | 77 / 100 | **$1.00** | Tương đương model đầu bảng nhưng rẻ hơn |
| **Panda Router + Escalation Ladder (Max 2 Retries)** | **91 / 100** | **~$1.25** | **Nhảy vọt hiệu năng (+14%), vượt xa Opus đơn lẻ** |
| **Oracle Lý Thuyết (Biết trước model đúng 100%)** | 94 / 100 | $0.85 | Trần giới hạn toán học lý thuyết |

---

## 5. Cập Nhật Mới Trong PandaCode Desktop (Hệ Điều Hành Cho AI Agents)

Trong nửa sau video, AICodeKing trình diễn cách Router được tích hợp sâu vào mã nguồn PandaCode:

### 1. Đóng gói Rust Native 100% (No Python Dependency)
- Toàn bộ Router được port sang **Rust**.
- Thư viện **ONNX Runtime** được nhúng trực tiếp vào binary của PandaCode.
- Người dùng chỉ tải model 31 MB một lần duy nhất qua GitHub Release (xác thực SHA-256). Không cần cài Python, không cần dựng HTTP server ngầm.

### 2. Ba Chế Độ Đội Ngũ (Team Presets)
- **Cheap Team:** Ưu tiên model rẻ nhất đạt ngưỡng thành công tối thiểu.
- **Balanced Team:** Cân bằng giữa Chi phí, Tỉ lệ thành công và Tốc độ (Được khuyên dùng mặc định).
- **Best Team:** Bỏ qua chi phí, luôn chọn model mạnh nhất cho từng nấc thang.

### 3. Failover Thông Minh & Giám Sát Sức Khỏe Model (Model Health)
- **Phân biệt Lỗi Lập Luận vs. Lỗi Hạ Tầng:**
  - Nếu model trả lời sai test: Leo bậc thang lên model mạnh hơn.
  - Nếu harness gặp sự cố (Rate limit, văng CLI, token hết hạn): Giữ nguyên model, tự động chuyển sang harness khác (ví dụ: chuyển từ gọi qua Claude Code sang Codex hoặc OpenCode).
- **Tạm dừng lũy tiến (Exponential Model Pausing):** Khi một model bị lỗi kết nối, tạm dừng nó 10 phút -> 20 phút -> tối đa 2 giờ. Nếu 2 models trên cùng harness lỗi, dừng toàn bộ harness đó.

### 4. Bảng Xếp Hạng Người Dùng (Interactive Tier List S/A/B/C/D)
- Người dùng có thể kéo thả các model vào bảng S, A, B, C, D theo sở thích cá nhân.
- Thuật toán định tuyến sẽ kết hợp trọng số: **70% theo Tier người dùng + 30% theo đánh giá khách quan của Router**.

### 5. Đỗ Tiến Trình (Process Parking) & Bảng Ghi Chú Dùng Chung (Shared Notes Board)
- **Sub-Workers Scoped:** Worker chính có thể sinh ra tối đa 3 sub-workers chạy song song nhưng chỉ được thao tác trong file của nó.
- **Process Parking (Tiết kiệm RAM tương tự AWS Infinity):** Khi worker cha chờ sub-workers hoàn thành, tiến trình của nó **tắt hoàn toàn (exits)**, giải phóng **250MB – 390MB RAM**. Khi sub-workers xong, worker cha được đánh thức nạp lại kết quả.
- **Shared Notes Board:** Một bảng đen chia sẻ tối đa 64 sự kiện ngắn (API schema, quyết định kiến trúc). Worker Backend vừa thống nhất API là Worker Frontend nhận được ngay lập tức mà không cần prompt lòng vòng.

---

## 6. Bài Học Đắt Giá Cho Kỹ Sư Hệ Thống & Agentic Developers

1. **Đừng Dùng LLM Để Điều Phối LLM:**
   - Việc viết một prompt dài dặn dò Claude Opus *"hãy chọn model phù hợp"* là một sai lầm chết người: tốn token, độ trễ cao (2–5 giây), và thiếu tính tất định.
   - Hãy dùng một **Tiny Decision Model (30MB)** kết hợp **Mã nguồn thuần túy (Code Policy)**: Độ trễ chỉ 10ms, miễn phí 100%, bảo mật tuyệt đối trên máy local.

2. **Sức Mạnh Của Verifiers Kết Hợp Thang Leo Bậc (Escalation):**
   - Một model giá rẻ kết hợp vòng lặp kiểm thử (Unit tests/Linter) và cơ chế tự động nâng cấp model khi hỏng ăn có thể **đánh bại hoàn toàn một model siêu cấp đứng một mình**.

3. **Cơ Chế Process Parking Là Chuẩn Mực Tương Lai:**
   - Cả **AWS Infinity** (Rust Execution Slices) và **PandaCode** (Process Parking giải phóng 390MB) đều chứng minh: **Không được để tiến trình AI Agent chạy ngâm RAM khi đang chờ đợi**.

---

## 7. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

- [AWS Infinity: AI Agent Runtime Viết Bằng Rust Cho Cực Hạn Đồng Thời](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/amazon_infinity_ai_agent_runtime_rust_lets_get_rusty.md) — Phân tích chi tiết kiến trúc Execution Slices và cơ chế Sleep/Hibernate đưa bộ nhớ về Scale to Zero.
- [Thực Chiến DeepSeek SuperAgent Harness](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_superagent_harness_hands_on_aicodeking.md) — AICodeKing phân tích kiến trúc Router nhẹ + Worker mạnh và kinh tế học token cache.
- [Xác Minh Là Code Review Mới (Ray Amjad)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/verification_is_the_new_code_review_ray_amjad.md) — Thiết kế Verifiers tất định làm chốt chặn nghiệm thu cho các vòng lặp nâng bậc (Escalation Ladder).
- [Mô Hình Voi & Cá Vàng (Google EGM)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/google_egm_skill_set_deepseek_aicodeking.md) — Phương pháp chia tách bối cảnh sâu và tác nhân phản biện không ký ức.
