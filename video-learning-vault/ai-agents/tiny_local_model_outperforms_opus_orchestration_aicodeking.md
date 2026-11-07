# Tự Chế Mô Hình Cục Bộ 31MB Đánh Bại Cả Opus: Bí Thuật Điều Phối Tác Nhân (PandaOrchestrator & Panda Router) | AICodeKing

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=Xa0Zhl8ljPY)
- **Watch Skill ID:** `5431301fbf871733`
- **Video ID:** `Xa0Zhl8ljPY`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 14:09 (849.3s)
- **Resolution:** 1280x720 (91 frames extracted & inspected via `watch-skill`, 85 frames with OCR)
- **Speaker:** AICodeKing (Nhà phát triển mã nguồn mở dự án `aicodeking/pandacode` & `aicodeking/PandaOrchestrator`)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong các hệ sinh thái lập trình đa tác nhân (Multi-Agent Coding Teams) như **PandaCode** (chạy song song Claude Code, Codex, Kimi Code, OpenCode, GLM, Grok Build, DeepSeek, Command Code, Cline, Kilo Code, Devin CLI, Hermes Agent, ZCode, Muse Code...), mô hình kinh điển là: **Một tác nhân Kiến trúc sư (Architect)** phân tích yêu cầu lớn thành các nhiệm vụ con (Assignments), sau đó giao cho các **Tác nhân Thực thi (Worker Agents)** chạy song song. Tuy nhiên, kiến trúc này luôn vấp phải một tử huyệt nghiêm trọng: **Architect phân bổ mô hình cực kỳ tồi tệ** (`frame_0001.jpg`, t=00:03).
- Hoặc nó gán thẳng mô hình đắt đỏ nhất như Claude Opus cho mọi tác vụ (đốt cạn quota token chỉ để đổi tên biến `rename a variable`).
- Hoặc nó chọn bừa một model giá rẻ cho tác vụ kiến trúc phức tạp, dẫn đến lỗi dây chuyền và con người phải dọn dẹp thủ công.

Trong video này, **AICodeKing** công bố giải pháp mã nguồn mở đột phá: **PandaOrchestrator** và **Panda Router** — một mô hình ra quyết định cục bộ siêu nhỏ (**32M tham số chưng cất từ 150M teacher thuộc họ Ettin, dung lượng nén INT8 ONNX chỉ 31 MB, chạy trên CPU mất 10–25ms không cần GPU, tổng chi phí từ lúc tạo repo trống đến bản v4 trong 3 ngày vỏn vẹn ~\$50**).

Kết quả thực nghiệm trên bộ **150 bài toán kiểm thử độc lập (Held-out tasks)** với 9 models (`frame_0022.jpg`–`frame_0023.jpg`, t=04:33–05:01):
- Dùng cố định một model lớn (`GPT-6.1 Sol` always): Giải được **79/100** tác vụ, chi phí **\$1.11 / 100 tác vụ**.
- **Panda Router (Single Pick - chọn 1 lần):** Giải được **77/100** tác vụ, chi phí chỉ **~\$1.00 / 100 tác vụ**.
- **Panda Router + Thang Leo Bậc (Escalation Ladder - tối đa 2 lần thử):** Đạt tỷ lệ giải thành công **91/100 tác vụ!** (Tiệm cận mức trần hoàn hảo **94/100** của Oracle lý thuyết khi biết trước model đúng cho mọi bài toán).

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                    PANDA ROUTER: JUDGMENT IN MODEL, POLICY IN CODE                          │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│     task text + optional context ("Add dark mode with settings toggle & pass tests")        │
│                              │                                                              │
│                              ▼                                                              │
│     ┌─────────────────────────────────────────────────────────┐                             │
│     │ PROFILER (ONNX int8, 31 MB | Ettin-32M encoder + heads) │ ◄── [Judgment · A Model]    │
│     │ - Category, Difficulty 1-10 (Lopsided Loss)             │     10–25 ms · CPU · no GPU │
│     │ - Skill levels, Flags (tools/images), Token estimate    │                             │
│     └────────────────────────┬────────────────────────────────┘                             │
│                              │ TaskProfile                                                  │
│                              ▼                                                              │
│     ┌─────────────────────────────────────────────────────────┐   CARDS/*.JSON (66+ Cards)  │
│     │ MATCHER (Deterministic Plain Code - Rust / Python)      │◄──[Haiku.json, Sonnet.json] │
│     │ - Calibrated P(success) lower bound (Success Head)      │   [Opus.json, GPT-6.1 Sol]  │
│     │ - Expected Cost = cost + (1 - P) × next_try             │   [Kimi K3.1 Pro, +61 more] │
│     │ - User Tier List (70% User Tier + 30% Router Rating)    │                             │
│     └────────────────────────┬────────────────────────────────┘                             │
│                              │ mode: max_utility | max_quality | cheapest_above_threshold   │
│                              ▼                                                              │
│     ┌─────────────────────────────────────────────────────────┐                             │
│     │ ESCALATION LADDER (choice + ranking + trace_id):        │                             │
│     │ 1. Rung 1 (Opener): Haiku 5.5 ──(Run & Verify)──► [PASS] ──► (Xong với giá rẻ nhất)   │
│     │                                        │                                              │
│     │                                      [FAIL] (/escalate -> next model, never step down)│
│     │                                        ▼                                              │
│     │ 2. Rung 2: Sonnet / Mid-tier ───(Run & Verify)──► [PASS]                              │
│     │                                        │                                              │
│     │                                      [FAIL]                                           │
│     │                                        ▼                                              │
│     │ 3. Rung 3: Opus 5.5 (Strongest) ─(Run & Verify)─► [PASS] (Độ chính xác vọt lên 91/100)│
│     └─────────────────────────────────────────────────────────┘                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Triết Lý Thiết Kế Lõi: "Judgment in Model, Policy in Code"

Qua sơ đồ kiến trúc trong README của `aicodeking/PandaOrchestrator` (`frame_0009.jpg`, t=01:31 và `frame_0012.jpg`, t=02:32), AICodeKing tách bạch hoàn toàn phần phán đoán của mạng nơ-ron và phần chính sách điều phối:

### 1. Phán đoán định tính (`judgment · a model`) thuộc về Profiler 31MB
- Mô hình `Ettin-32M encoder + heads` không viết code và không trực tiếp trả về tên LLM (tránh bị khóa cứng vào danh sách model lúc train).
- Nó đọc `task text + optional context` và xuất ra `TaskProfile` gồm:
  - `category` (loại tác vụ)
  - `difficulty 1-10` (độ khó từ 1 đến 10)
  - `skill levels` (các kỹ năng cần thiết)
  - `flags` (cần gọi tool hay xử lý ảnh) & `token estimate`.

### 2. Chính sách chọn model (`policy · plain code`) thuộc về Matcher tất định
- Một khối code thuần túy (Deterministic Matcher, không phải mạng nơ-ron) nhận `TaskProfile` và chấm điểm từng model khả dụng theo 3 chế độ (`max_utility` mặc định, `max_quality`, hoặc `cheapest_above_threshold`).
- **Tại sao giữ Policy trong Code?** Vì **code có thể viết unit test và debug trực tiếp**. Nếu Router đưa ra một lựa chọn kỳ quặc, tác giả biết chính xác dòng rule nào gây ra và sửa ngay lập tức mà không cần train lại model.
- **Models Are Just Data (`frame_0010.jpg`, t=01:41):** Mỗi mô hình chỉ là một file `.json` trong thư mục `cards/` (`Haiku.json`, `Sonnet.json`, `Opus.json`, `GPT-6.1 Sol.json`, `Kimi K3.1 Pro.json` và hơn 61 thẻ model khác — tổng cộng ~66 model cards từ Claude, Codex/GPT, Grok, GLM, Kimi, OpenRouter, Ollama). Khi một model mới ra mắt, chỉ cần thêm 1 thẻ `.json` chứa skill levels, giá, context window và latency mà không cần đụng vào trọng số của Router.

---

## 3. Quy Trình Huấn Luyện Panda Router Với Ngân Sách ~\$50

Toàn bộ quá trình xây dựng từ repo trống đến bản v4 chỉ mất **3 ngày** và tốn khoảng **\$50** (`frame_0011.jpg`–`frame_0024.jpg`, t=02:01–05:34):

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                    BUDGET TRAINING PIPELINE (~$50 TOTAL COST IN 3 DAYS)                     │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│  [1. Labeling 8,000 Tasks ($2.77)]                                                          │
│  - Dùng DeepSeek-V4.1-Flash làm Teacher gán nhãn 8,000 tasks chỉ tốn $2.77.                 │
│                                                                                             │
│  [2. Attempt & Grade — Real Outcome Data (~$44)]                                            │
│  - Cho các LLM thực tế (Haiku, Sonnet, GPT-6.1 Sol...) giải bài tập trong tập train.        │
│  - Strict Judge chấm PASS / FAIL thực nghiệm (chạy đến khi cạn sạch $44 OpenRouter credits  │
│    và số dư DeepSeek về $0 giữa lúc chấm điểm — frame_0017.jpg).                            │
│                                                                                             │
│  [3. Ettin Distillation trên Modal ($1.67)]                                                 │
│  - Họ mô hình Ettin (MIT License, encoder-only, train trên dữ liệu mở có code, cùng         │
│    tokenizer giữa mọi kích cỡ).                                                             │
│  - Train Teacher 150M tham số trên Modal -> Chưng cất (Distill) sang Student 32M tham số.   │
│  - Quantize về INT8 -> Xuất ONNX 31 MB (chạy CPU 10–25ms, không dữ liệu nào rời máy).       │
│                                                                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

### 4 Kỹ Thuật Huấn Luyện Đắt Giá (`frame_0018.jpg`–`frame_0024.jpg`)

1. **Calibrated Success Head & Cận Dưới Bi Quan (`frame_0018.jpg`, t=03:32):**
   - Từ dữ liệu PASS/FAIL thực tế, tác giả huấn luyện một `success head` dự đoán xác suất thành công `P(success)` đã hiệu chuẩn cho từng model (ví dụ ở Difficulty 5 Implementation: `Sonnet 0.62`, `Haiku 0.50`, `GPT-6 Luna 0.31`) kèm theo một **cận dưới (lower bound)**.
   - Router ra quyết định dựa trên **lower bound** chứ không dựa trên con số lạc quan trung bình, tránh đặt cược vào những model chỉ đẹp trên lý thuyết.
2. **Chi Phí Kỳ Vọng Tính Cả Giá Của Thất Bại (`frame_0019.jpg`, t=03:46):**
   - Công thức so sánh chi phí:
     $$\text{expected cost} = \text{cost} + (1 - P) \times \text{next try}$$
   - Một model siêu rẻ như Haiku (`\$0.0009`) nhưng thất bại 50% số lần thực chất không hề rẻ khi cộng thêm chi phí phải chạy lại ở nấc thang tiếp theo.
3. **Hàm Mất Mát Độ Khó Lệch Bên — Lopsided Difficulty Loss (`frame_0020.jpg`, t=04:03):**
   - Đồ thị hàm phạt bất đối xứng: Đoán bài toán **dễ hơn thực tế (`<- under-rate`)** bị phạt cực nặng (đường dốc đứng màu đỏ), trong khi đoán **khó hơn thực tế (`over-rate ->`)** chỉ chịu một khoản phạt nhỏ (`a small penalty`).
   - Kết hợp với việc fine-tune thêm **1,500 yêu cầu mơ hồ (vague requests)** như *"make the app better"* hoặc *"a model trainer"* (được đẩy độ khó từ `1.5` lên `~5` trong `frame_0021.jpg`), giúp tỉ lệ chọn nhầm model yếu cho bài khó giảm mạnh.
4. **Kỷ Luật "Pass Rules First" (`frame_0024.jpg`, t=05:34):**
   - Bảng lịch sử chất lượng Profiler trên 359 bài toán Gold Set (`frame_0021.jpg`):
     - `v1`: Category Acc `0.624` | Difficulty MAE/rho `0.900 / 0.675` | Flags AUROC `0.885`
     - `v2`: Category Acc `0.641` | Difficulty MAE/rho `0.897 / 0.705` | Flags AUROC `0.886`
     - `v3`: Category Acc `0.688` | Difficulty MAE/rho `0.825 / 0.747` | Flags AUROC `0.901`
   - Dù đã huấn luyện thêm `v5` và `v6` với nhiều dữ liệu hơn, tác giả đã viết sẵn bộ **Pass Rules trước khi chạy eval** (`every probe passes`, `pick-rate check`, `no cell below v4.3`). Cả `v5` và `v6` đều trượt ít nhất 1 tiêu chí, nên **`v4.3` vẫn được giữ làm mặc định**. (Quá trình test `v6` cũng giúp phát hiện bug cắt cụt câu trả lời dài quá 6,000 ký tự trước khi chấm điểm khiến Sonnet bị dìm điểm oan).

---

## 4. Tích Hợp Sâu Vào PandaCode: Điều Phối Đa Tác Nhân Thế Hệ Mới

Từ phút `06:26` (`frame_0030.jpg` trở đi), AICodeKing trình diễn hàng loạt nâng cấp biến PandaCode thành một hệ điều hành điều phối Agent hoàn chỉnh:

### 1. Tích Hợp Rust Native & Tải Một Lần (`frame_0033.jpg`–`frame_0037.jpg`, t=06:45–07:12)
- Toàn bộ logic Panda Router được port sang **Rust** và nhúng sẵn **ONNX Runtime** vào ứng dụng (không cần cài Python hay chạy server riêng).
- Lần đầu tiên bật Agent Team, PandaCode tải file profiler **35 MB** một lần duy nhất từ một commit cố định trên GitHub và kiểm tra mã băm **SHA-256** (trong vài giây đang tải, hệ thống dùng `keyword fallback` nên người dùng không bao giờ phải chờ).
- Thay vì giới hạn 24 models (6 models/tool) như trước, Router quét toàn bộ danh sách model của mọi công cụ đã cài (ví dụ **45 models** trên máy tác giả).

### 2. Menu Team Tối Giản & Router-First (`frame_0032.jpg`, `frame_0038.jpg`–`frame_0040.jpg`, t=06:34–08:05)
- Nút chọn Team dưới khung chat chỉ gồm 4 lựa chọn trực quan:
  - **No team:** Chạy một agent đơn lẻ.
  - **Cheap team:** Chọn model rẻ nhất vượt qua ngưỡng `Minimum chance of success` do bạn đặt.
  - **Balanced team:** Cân bằng giữa xác suất thành công, chi phí và tốc độ.
  - **Best team:** Chọn model mạnh nhất phù hợp với tác vụ.
- **Teams Are Router First:** Mọi assignment (`frontend`, `backend`, `tests`) đều đi qua Panda Router mặc định. Architect chỉ tự chọn model khi bạn chỉ đích danh tên model trong câu lệnh (ví dụ: *"dùng Kimi K3 cho frontend"*).
- Trong `Cheap` và `Balanced team`, mỗi worker bắt đầu ở nấc thấp nhất của **Escalation Ladder** (`ladder 1/3, opener`). Nếu Architect chạy test/review thấy thất bại, nó thay thế worker và leo lên model mạnh hơn (tối đa **2 lần retry** rồi dừng báo lỗi chứ không lặp vô tận đốt quota).

### 3. Failover Giữa Các Harness & Quản Lý Sức Khỏe Model (`frame_0041.jpg`–`frame_0049.jpg`, t=08:22–09:44)
- **Same Model, Another Harness (`frame_0042.jpg`):** Nhiều khi worker thất bại không phải do model dốt mà do **harness bị gãy** (dính rate limit, crash CLI, hết hạn đăng nhập). Khi đó, PandaCode tự động chuyển worker sang **cùng model đó nhưng chạy trên một harness khác** đã cài trên máy (ví dụ: chuyển `GPT-6.1 Sol` từ `Command Code` sang `Codex`) trên một phiên mới tinh.
- **Paused = Out of the Pool (`frame_0044.jpg`, `frame_0046.jpg`):** Khi một model報 lỗi trên một harness, PandaCode tạm dừng (pause) nó 10 phút và nhân đôi thời gian mỗi lần lặp lại (tối đa 2 giờ; nếu 2 models cùng lỗi trên 1 harness thì pause cả harness đó). Model đang pause (ví dụ `GPT-5.6-Terra · Codex Paused 8 min` kèm nút `Allow now`) sẽ bị loại khỏi pool của cả Router lẫn Architect và trạng thái pause được giữ nguyên kể cả khi khởi động lại app.
- **Crash ≠ Bad Answer (`frame_0047.jpg`):** Một lượt chạy bị crash giữa chừng không bị tính là câu trả lời sai; chỉ khi kết quả chạy xong nhưng trượt bước nghiệm thu (review/test) thì Router mới nâng bậc lên model đắt hơn.
- **Kiểm Soát Danh Mục Lớn (`frame_0048.jpg`–`frame_0049.jpg`):** Nút gạt bật/tắt từng model (`Space Bunny Free: off`), và với các harness có kho model khổng lồ như OpenCode, Kilo Code hay Hermes Agent, PandaCode chỉ lấy **10 models dùng gần nhất** cộng với các model bạn bấm `Add model...` (`Showing 10 of 14 models · Add model...`).

### 4. Bảng Xếp Hạng Tier List Kéo-Thả S / A / B / C / D (`frame_0050.jpg`–`frame_0054.jpg`, t=09:52–10:20)
- Trong phần Settings, người dùng có thể kéo thả bất kỳ model nào vào các hàng **S, A, B, C, D** (ví dụ xếp `Kimi K3.1 Pro` vào hạng S, `Claude Opus 5.5` vào hạng A).
- **Công thức pha trộn trọng số (`frame_0052.jpg`):** Với model đã được người dùng xếp hạng, xác suất thành công được tính bằng:
  $$\text{Chance of Success} = 70\% \times \text{Your Tier} + 30\% \times \text{Router's Own Rating}$$
  ở mọi mức độ khó từ 1 đến 10, đồng thời quyết định luôn thứ tự trên Escalation Ladder.
- **No Card? Rank It (`frame_0054.jpg`):** Ngay cả khi một model hoàn toàn mới chưa có thẻ `.json` trong Router, chỉ cần bạn kéo nó vào một hạng (ví dụ hạng B), nó lập tức trở thành `pickable` để Router điều phối!

---

## 5. Bốn Cải Tiến Cốt Lõi Của Bộ Điều Phối (Orchestrator Upgrades)

Từ phút `10:31` (`frame_0055.jpg`–`frame_0080.jpg`), video đi sâu vào 4 nâng cấp hệ thống giúp tiết kiệm RAM và Token:

1. **Sub-Workers Cô Lập Theo File & Cơ Chế Đỗ Tiến Trình (`frame_0056.jpg`–`frame_0057.jpg`, t=10:37–10:46):**
   - Một worker có thể tự đẻ thêm các **Sub-workers** (sâu tối đa 1 cấp, tối đa 3 sub-workers chạy song song và 6 sub-workers tổng cộng).
   - **Own Files Only:** Sub-workers của Worker 1 chỉ được phép sửa đúng các file được giao cho Worker 1 (`app.js`, `theme.js`, `styles.css`, `index.html`), tuyệt đối không chạm được vào file của Worker 2 (`api.js`, `db.js`).
   - **Process Parking:** Trong lúc worker cha đợi sub-workers chạy, tiến trình của worker cha **thoát hoàn toàn (exits)**, giải phóng **250 MB – 390 MB RAM**, và tự động thức dậy nhận kết quả khi sub-workers hoàn tất mà không tốn thêm lượt gọi của Architect.
2. **Team Messaging Không Cần Polling & Shared Notes Board (`frame_0061.jpg`, t=11:07):**
   - **Unread News Rides on Every Result:** Xóa bỏ hoàn toàn cơ chế polling tốn kém. Mọi kết quả trả về từ công cụ nhóm (`team tool result: ok`) đều đính kèm một bản tóm tắt tin nhắn chưa đọc (`digest · 2 unread — backend: API is ready`). Tin nhắn khẩn cấp (urgent message) có thể chèn ngang vào giữa lượt chạy của một worker.
   - **Shared Notes Board:** Bảng ghi chú chung lưu tối đa **64 sự kiện ngắn** (như cấu trúc API hoặc quyết định thiết kế) và tự động cấp cho mọi worker mới khi khởi chạy để Frontend và Backend đồng bộ tức thì.
3. **Architect Nghỉ Ngơi & Tiết Kiệm Token Cực Đại (`frame_0062.jpg`–`frame_0063.jpg`, t=11:31–11:58):**
   - **The Architect Rests:** Thay vì giữ một tiến trình nhàn rỗi chạy ngầm liên tục, Architect hoàn toàn nghỉ ngơi và chỉ thức dậy khi có **1 trong 4 tín hiệu (`wakes on 4 signals`)**:
     1. `urgent message` (tin nhắn khẩn)
     2. `blocker` (tác vụ bị chặn)
     3. `failure` (worker lỗi)
     4. `finished results` (kết quả đã xong).
   - **Fewer Tokens (`frame_0063.jpg`):** Hướng dẫn hệ thống cho worker (`worker instructions`) ngắn hơn **80%**, văn bản công cụ nhóm (`team tool text`) giảm còn **1/3**, và các lượt nghiệm thu của Architect (`review turns`) cũng giảm kích thước xuống **1/3**.
4. **Trực Quan Hóa Luồng Chạy & Changes View Tức Thì (`frame_0064.jpg`–`frame_0080.jpg`, t=12:04–12:57):**
   - Trong bản demo thực tế thêm tính năng Dark Mode kèm nút gạt Settings và chạy test cho `sample-app` với chế độ **Balanced team**:
     - Architect chia thành 3 nhiệm vụ trong 1 batch (`frame_0064.jpg`):
       - `Theme tokens` (`src/theme.ts`): Giao cho model giá rẻ **Claude Haiku 5.5** (`ladder 1/3, opener`).
       - `Settings toggle` (`src/components/appearance...`): Giao cho model tầm trung **Codex · GPT-5.6-Terra**.
       - `Appearance tests` (`tests/appearance-toggle.t...`): Giao cho model mạnh **Claude Opus 5.5**.
     - **The Changes View (`frame_0069.jpg`–`frame_0070.jpg`):** Gắn nhãn chính xác worker nào đã sửa dòng code nào và mở các bản diff lớn ngay lập tức không bị đơ giao diện.

---

## 6. Hạn Chế Hiện Tại & Bài Học Kiến Trúc (`frame_0085.jpg`–`frame_0088.jpg`, t=13:10–13:46)

- **Các giới hạn được tác giả thẳng thắn chia sẻ:**
  - Nhiều thẻ model (`model cards`) cho các công cụ lập trình mới vẫn là con số ước lượng ban đầu.
  - Điểm xác suất thành công nhìn có vẻ thấp do được chấm bởi một **Strict Judge** rất khắt khe (nên dùng để so sánh tương đối giữa các model với nhau thay vì so với mức 100%).
  - **Intel Macs và x86 Linux (`frame_0085.jpg`)** hiện vẫn dùng `keyword fallback` do chưa có bản build ONNX Runtime tương thích đóng gói sẵn cho các kiến trúc này.
  - Cơ chế Escalation Ladder chỉ tốt tương đương với bài kiểm tra mà Architect chạy — hãy cung cấp **unit tests thực tế** cho dự án để phát huy tối đa sức mạnh.
- **Liên kết mã nguồn (MIT License):**
  - `github.com/aicodeking/PandaOrchestrator` (hỗ trợ CLI, HTTP Server, Python API và MCP Server — dùng được cho mọi Agent ngoài PandaCode).
  - `github.com/aicodeking/pandacode` (Desktop workspace tích hợp sẵn Panda Router bằng Rust).

---

## 7. Liên Kết Chéo Trong Vault

- [AWS Infinity: AI Agent Runtime Viết Bằng Rust Cho Cực Hạn Đồng Thời](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/amazon_infinity_ai_agent_runtime_rust_lets_get_rusty.md) — Cùng triết lý giải phóng RAM khi Agent chờ đợi (Execution Slices vs. Process Parking 250–390MB).
- [Thực Chiến DeepSeek SuperAgent Harness](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_superagent_harness_hands_on_aicodeking.md) — Phân tích của AICodeKing về điều phối đa model và tối ưu hóa chi phí.
- [Xác Minh Là Code Review Mới (Ray Amjad)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/verification_is_the_new_code_review_ray_amjad.md) — Tại sao bộ kiểm thử tất định (Tests / Typecheck) là điều kiện tiên quyết để Escalation Ladder hoạt động.
