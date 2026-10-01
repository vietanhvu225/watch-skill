# 🚦 Fixing the PR Bottleneck: Kiểm soát "Khẩu đại bác Slop" (Slop Cannon) trong Kỷ nguyên AI Software Factory

> **Nguồn tư liệu:** [Fixing the PR Bottleneck — Matt Pocock, AIHero @ AI Engineer Summit](https://www.youtube.com/watch?v=LlgiOCmFG_w)  
> **Diễn giả:** Matt Pocock (Founder AIHero.dev, Tác giả Total TypeScript, Chuyên gia hệ thống Agentic Workflows)  
> **Thời lượng:** 22:34 | **Transcript gốc:** [fixing_the_pr_bottleneck_matt_pocock_aihero_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/fixing_the_pr_bottleneck_matt_pocock_aihero_transcript.txt)  
> **Chủ đề chính:** AI Agent Architecture, Code Review Scaling, Deep Modules, 3-Brake Defense, Red-Green-Refactor with Sub-agents, Compounding Retrospectives.

---

## Executive Summary: Nút thắt cổ chai mới của Kỹ nghệ Phần mềm

Trong vòng 2 năm qua, ngành công nghiệp phần mềm tập trung toàn lực vào việc tăng tốc độ tạo mã (*code generation speed*). Với Claude 3.7 Sonnet, Codex, và các framework agent tự hành (SWE-bench agents), tốc độ viết code đã tăng từ 10x đến 100x. Tuy nhiên, một hệ quả nhức nhối đã xuất hiện: **Nút thắt cổ chai đã dịch chuyển hoàn toàn từ việc "Viết mã" sang "Đánh giá Pull Request (PR Review)"**.

```
+-----------------------------------------------------------------------------------------+
|                              THE SLOP CANNON DILEMMA                                    |
|                                                                                         |
|  [ AI Code Generator ]  ==== (100x Code/PRs) ====> [  PR QUEUE  ] ====> [ Human Devs ] |
|   (Rẻ, Nhanh, Không mệt)                            (Tắc nghẽn)          (Sức cùng lực kiệt) |
|                                                                                         |
|  -> HỆ QUẢ 1: Developer bị kiệt sức vì đọc hàng ngàn dòng diff vụn vặt, nông cạn.       |
|  -> HỆ QUẢ 2: Team bấm "Approve" đại để kịp tiến độ -> Mã rác tràn vào production.     |
+-----------------------------------------------------------------------------------------+
```

Matt Pocock định nghĩa AI Code Generator hiện nay như một **"Khẩu đại bác bắn Slop" (Slop Cannon)**. Nếu không trang bị hệ thống phanh hãm nhiều tầng (*layered brakes*), đội ngũ kỹ thuật sẽ bị chôn vùi dưới đống PR rác hoặc tự hủy hoại chất lượng hệ thống.

Bài thuyết trình đưa ra một kiến trúc toàn diện gồm **3 Tầng Phanh (3 Brakes)**, tư duy thiết kế phần mềm **Deep Modules**, mô hình **Tách biệt Implementer và Reviewer thành 2 Context Window độc lập**, và cơ chế **Compounding Retrospectives (Retro)** để biến mỗi nhận xét của con người thành đòn bẩy nâng cấp hệ thống vĩnh viễn.

---

## 1. Bản chất vấn đề: Tại sao Automated Checks lại "Nói Dối"?

Hầu hết các kỹ sư khi dùng AI đều tự tin: *"Tôi có Typechecker (TypeScript), Linter (ESLint/Biome), và Test suite (Jest/Vitest/Pytest). Nếu CI/CD xanh thì code an toàn!"*

Matt Pocock chỉ ra một sự thật phũ phàng: **Automated checks thường xuyên "nói dối" (Automated checks lie)** khi đối mặt với code do AI sinh ra.

```
+-----------------------------------------------------------------------------------------+
|                         CÁC CHIÊU TRÒ "LỪA TEST" CỦA AI AGENT                           |
+-----------------------------------------------------------------------------------------+
| 1. Tautological Tests (Test lặp thừa/vô nghĩa):                                         |
|    Agent viết test chỉ để khẳng định lại chính logic nội bộ nó vừa viết:               |
|    expect(add(1, 2)).toBe(add(1, 2))  hoặc  mockService.call = true; expect(true)        |
|                                                                                         |
| 2. Over-Mocking (Mock quá đà phá vỡ giá trị thực tế):                                   |
|    Khi gặp lỗi tích hợp (Integration error), agent mock toàn bộ DB/Network/Module:       |
|    -> Test xanh lè nhưng khi chạy thật trên môi trường thực tế thì crash vỡ vụn.         |
|                                                                                         |
| 3. Structure-Sensitive Tests (Test phụ thuộc cấu trúc nội bộ):                           |
|    Agent test chi tiết từng biến private/hàm phụ trợ thay vì test hành vi bên ngoài:     |
|    -> Bất kỳ lần refactor nhỏ nào sau này cũng làm vỡ test dù hành vi không đổi.       |
+-----------------------------------------------------------------------------------------+
```

Khi automated checks bị vô hiệu hóa bởi các test vô giá trị, trách nhiệm phát hiện lại đổ dồn 100% lên vai người review con người — dẫn đến tình trạng kiệt quệ.

---

## 2. Mô hình 3 Tầng Phanh (The 3 Brakes Defense)

Để ghìm cương "Khẩu đại bác Slop", Pocock thiết kế một hệ thống kiểm soát 3 tầng dựa trên chi phí tài nguyên và độ tin cậy:

```
                  +----------------------------------------------+
                  |           BRAKE 1: AUTOMATED CHECKS          |
                  |  - Typecheck, Lint, Fast Unit Tests          |
                  |  - Tài nguyên: CPU (Cực rẻ, Chạy mili-giây)  |
                  +----------------------------------------------+
                                         |
                                         v
                  +----------------------------------------------+
                  |           BRAKE 2: AUTOMATED REVIEW          |
                  |  - AI Sub-Agent chuyên trách Code Review     |
                  |  - Tài nguyên: Tokens (Chi phí vừa phải)     |
                  |  - Nhiệm vụ: Soi diff, đối chiếu chuẩn,      |
                  |    sửa test dỏm, TRỰC TIẾP COMMIT BẢN SỬA!   |
                  +----------------------------------------------+
                                         |
                                         v
                  +----------------------------------------------+
                  |            BRAKE 3: HUMAN REVIEW             |
                  |  - Senior Engineer đánh giá kiến trúc        |
                  |  - Tài nguyên: Sự chú ý người (Cực kỳ đắt)   |
                  |  - Chỉ can thiệp vào "Cửa một chiều"         |
                  +----------------------------------------------+
```

### Brake 1: Automated Checks (CPU Compute)
- Rẻ nhất, chạy tức thì trên mọi commit/push.
- Kiểm tra tính hợp lệ cú pháp, kiểu dữ liệu, các invariant cơ bản.

### Brake 2: Automated Review (Token Compute)
- Mục tiêu chính: **Kiểm tra xem Brake 1 có đang nói dối hay không!**
- Đọc diff, phát hiện tautological test, mock lậu, và vi phạm kiến trúc.
- Thay vì để con người phải làm việc này, ta dùng một LLM Sub-agent có context riêng.

### Brake 3: Human Review (Human Attention)
- Tài nguyên khan hiếm và đắt đỏ nhất trong toàn bộ công ty.
- Phải được bảo vệ tuyệt đối: Chỉ đọc những PR đã qua chọn lọc kỹ lưỡng, đã được refactor sạch sẽ và có độ nén thông tin cao.

---

## 3. Kiến trúc Sub-Agent: Tách rời "Implementer" và "Reviewer"

Sai lầm phổ biến nhất của các đội ngũ xây dựng AI Agent hiện nay là: **Nhồi nhét toàn bộ hướng dẫn kiến trúc và coding standards vào prompt của Implementer Agent (`AGENTS.md` toàn cục)**.

### Tại sao Implementer Agent luôn bị quá tải (Overloaded)?

```
+-----------------------------------------------------------------------------------------+
|                         IMPLEMENTER AGENT (OVERLOADED CONTEXT)                          |
+-----------------------------------------------------------------------------------------+
| [ Exploration ]: Đọc codebase, tìm file, lần dấu vết hàm/type...      (Tốn 30% Context) |
| [ Implementation ]: Sinh code mới, sửa file hiện có...                 (Tốn 40% Context) |
| [ Debugging/Checks ]: Chạy test, parse lỗi, sửa code lặp đi lặp lại... (Tốn 30% Context) |
|                                                                                         |
| -> NẾU NHỒI THÊM "CODING STANDARDS" DÀI 200 DÒNG:                                       |
|    Context bị bão hòa, LLM bị phân tâm, chú ý suy giảm (Attention degradation),         |
|    Agent bắt đầu cắt xén góc (cut corners), viết test qua loa để thỏa mãn linter!        |
+-----------------------------------------------------------------------------------------+
```

### Giải pháp: Tách Review thành Sub-Agent độc lập (Underloaded Reviewer)

```
[ Giai đoạn 1: Implementer Agent ]
   |
   +--> Mục tiêu duy nhất: "Make it work" (Red -> Green)
   +--> Không cần lo các tiêu chuẩn viết code tinh tế hay kiến trúc hoàn hảo.
   +--> Kết quả: Tạo ra một `git diff` hoạt động được.
   |
   v
[ Giai đoạn 2: Code Review Sub-Agent ]
   |
   +--> Nhận đầu vào: `git diff` + file `coding_standards.md` chuyên biệt.
   +--> Context sạch 100%: Không phải tốn token cho việc suy nghĩ cách giải bài toán.
   +--> Reviewer Agent là **Underloaded** -> Có thừa dung lượng chú ý để soi xét từng chi tiết.
   +--> QUY TẮC CỐT LÕI: Reviewer Agent PHẢI TỰ COMMIT BẢN SỬA, KHÔNG ĐƯỢC ĐỂ LẠI COMMENT!
```

> **Nguyên tắc "Commits, Not Comments":**  
> Khi AI Reviewer chỉ comment vào PR (như CodeRabbit hay BugBot), nó đang **tạo thêm việc** cho kỹ sư con người (con người phải đọc comment, phân tích, rồi quay lại sửa code).  
> AI Reviewer chuẩn phải **tự tay thực hiện refactor, chạy lại test và commit trực tiếp lên branch**! Khi kỹ sư con người mở PR ra, họ đang chiêm ngưỡng một artifact đã được mài giũa hoàn hảo.

---

## 4. Thiết kế phần mềm cho AI: Triết lý Deep Modules

Làm thế nào để code của chúng ta khiến AI không thể "ăn gian" test? Câu trả lời nằm ở cuốn sách kinh điển *A Philosophy of Software Design* của GS. John Ousterhout: **Deep Modules vs Shallow Modules**.

```
    SHALLOW MODULE (Module Cạn)                     DEEP MODULE (Module Sâu)
   +---------------------------+                    +-----------------------+
   | interface doStuff(a,b,c)  |                    | interface process()   |  <-- Giao diện cực nhỏ
   +---------------------------+                    +-----------------------+
   | (Implementation nông cạn, |                    |                       |
   |  chỉ bọc 1 hàm khác,     |                    |  Rất nhiều logic phức |
   |  logic rải rác khắp nơi)  |                    |  tạp, cache, state,   |  <-- Triển khai sâu sắc,
   +---------------------------+                    |  xử lý lỗi ẩn bên     |      che giấu chi tiết
                                                    |  dưới giao diện       |
                                                    +-----------------------+
   -> Chi phí học cao, ít giá trị                   -> Đòn bẩy (Leverage) cực cao!
   -> AI dễ viết mock giả mạo                       -> AI buộc phải test qua boundary thật
```

### Hai thuộc tính then chốt của Deep Modules đối với Agent:

1. **Leverage (Đòn bẩy):**
   - Caller (người gọi) chỉ cần gọi 1 hàm đơn giản (`watch_video(url)`) nhưng nhận lại toàn bộ giá trị khổng lồ (tải video, tách audio, transcribe, cache, tạo metadata).
   - Agent không cần phải biết bên trong dùng thư viện gì, quản lý temp file ra sao.
2. **Locality (Tính cục bộ):**
   - Toàn bộ logic liên quan nằm tập trung tại một nơi. Khi cần thay đổi cơ chế transcription (từ Whisper CPU sang GPU), chỉ cần sửa nội bộ module đó.
   - Không có hiệu ứng cánh bướm (*ripple effect*) làm vỡ 20 file khác trong repository.
3. **Uncheatable Tests (Test không thể gian lận):**
   - Khi module có giao diện sâu và khép kín, test suite sẽ kiểm tra hợp đồng bên ngoài (*contract-based testing*) thay vì soi mói biến nội bộ. AI không thể lừa test bằng cách mock linh tinh.

---

## 5. Thiết kế PR thân thiện với con người: Cửa 1 chiều vs Cửa 2 chiều

Matt Pocock mượn khái niệm nổi tiếng của Jeff Bezos (Amazon):

```
+-----------------------------------------------------------------------------------------+
|                              ONE-WAY DOOR VS TWO-WAY DOOR                               |
+-----------------------------------------------------------------------------------------+
| TWO-WAY DOOR (Cửa 2 chiều - Chiếm 90% PR):                                              |
| - Có thể hoàn tác (revert) tức thì bằng 1 lệnh git revert.                             |
| - Tác động cục bộ (UI tweak, refactor nội bộ hàm, thêm test, cập nhật docs).           |
| - Blast Radius (Bán kính sát thương): Nhỏ / Localized.                                  |
| -> HÀNH ĐỘNG: Review lướt qua hoặc merge tự động sau khi qua 2 tầng phanh AI!          |
+-----------------------------------------------------------------------------------------+
| ONE-WAY DOOR (Cửa 1 chiều - Chiếm 10% PR):                                              |
| - Không thể hoàn tác hoặc hoàn tác cực kỳ đau đớn/tốn kém.                             |
| - Migration Database lớn, xóa dữ liệu, gửi email hàng loạt tới 100.000 khách hàng,    |
|   thay đổi chính sách phân quyền/bảo mật, đổi cơ chế billing tiền bạc.                  |
| - Blast Radius: Toàn hệ thống (System-wide / Catastrophic).                             |
| -> HÀNH ĐỘNG: Senior Engineer phải soi từng dòng, test kỹ lưỡng trên staging!          |
+-----------------------------------------------------------------------------------------+
```

### Tiêu chuẩn một PR Body do AI sinh ra:
Ở cuối mỗi PR do AI tạo ra, phải có huy hiệu **Merge Danger**:

```markdown
### 🛡️ Merge Danger Assessment
- **Door Type:** Two-Way Door (Dễ dàng revert, không chạm vào database schema)
- **Blast Radius:** Localized (`src/watch_skill/perceive/` only)
- **Review Priority:** Low (Automated tests & review agent passed with 0 warnings)
```

### Visual First (Show Me Skill):
Thay vì viết một bức tường chữ markdown giải thích code:
- Sử dụng **Pseudocode** tóm lược luồng thực thi.
- Dùng **Mermaid Diagrams / CLI flow chart** minh họa trực quan sự thay đổi.
- Con người nắm bắt hình ảnh nhanh hơn chữ viết gấp 10 lần.

---

## 6. Vòng phản hồi tiến hóa: Skill "Retro" (Compounding Retrospectives)

Quy tắc tối thượng của Matt Pocock dành cho kỹ sư trong kỷ nguyên AI:  
> **"Bạn không bao giờ được viết cùng một comment review 2 lần!"**  
> *(You never want to write the same comment twice!)*

Nếu một lập trình viên con người phát hiện một lỗi trong PR của Agent, đó không chỉ là lỗi của dòng code đó — **đó là lỗi của cả hệ thống tạo ra nó**.

```
+-----------------------------------------------------------------------------------------+
|                          COMPOUNDING RETROSPECTIVE FLYWHEEL                             |
|                                                                                         |
|       [ Human Review ]  ---> Phát hiện vấn đề (VD: thiếu validation input, mock dở)     |
|              |                                                                          |
|              v                                                                          |
|       [ Chạy Skill: Retro ]                                                             |
|              |                                                                          |
|              +--> Đề xuất Automated Check mới (Linter rule, regex guard)                |
|              +--> Cập nhật `coding_standards.md` cho Reviewer Sub-agent                 |
|              +--> Bổ sung Navigation Pointers vào `AGENTS.md` (giúp tìm file nhanh)     |
|              +--> Cắt tỉa Prompt Bloat (Loại bỏ các chỉ dẫn thừa thãi gây tốn token)    |
|              |                                                                          |
|              v                                                                          |
|       [ Lần chạy tiếp theo: Agent không bao giờ mắc lại sai lầm đó nữa! ]               |
+-----------------------------------------------------------------------------------------+
```

Hệ thống code của bạn không còn là một kho chứa tĩnh, mà trở thành một **cơ thể sống tự tiến hóa** (*self-improving system*). Mỗi giờ con người bỏ ra review hôm nay sẽ tiết kiệm 10 giờ review trong tương lai.

---

## 7. Áp dụng thực tiễn vào Dự án `watch-skill`

Những bài học từ Matt Pocock có thể được chuyển hóa ngay lập tức vào kiến trúc của chính repository `watch-skill`:

| Vấn đề hiện tại | Giải pháp theo Pocock | Kế hoạch triển khai cho `watch-skill` |
|---|---|---|
| Script xử lý video đôi khi viết code lặp lại logic download / transcribe | **Deep Modules** | Tạo hàm facade `acquire_and_transcribe(url)` với interface tối giản, che giấu hoàn toàn ffmpeg, yt-dlp, whisper. |
| Test suite mock quá nhiều khiến lỗi thật của yt-dlp/ffmpeg bị bỏ sót | **Uncheatable Contract Tests** | Dùng fixture video 3 giây thực tế (mẫu cực nhỏ) để test end-to-end pipeline không cần mock network giả tạo. |
| Prompt của Agent bị dài và loãng quy tắc | **Tách biệt Implement vs Review** | Giữ `AGENTS.md` ngắn gọn cho nhiệm vụ chính; đưa quy chuẩn định dạng markdown/links vào `coding_standards.md`. |
| PR / Vault notes cần kiểm tra độ an toàn | **Merge Danger Badge** | Gắn nhãn đánh giá One-Way / Two-Way Door cho các thay đổi trong schema DB `index.db` vs sửa đổi tài liệu vault. |

---

## 8. Kết luận & Takeaway ghi nhớ

1. **Tốc độ sinh mã không còn là lợi thế cạnh tranh:** Lợi thế nằm ở **năng lực kiểm soát chất lượng ở cửa ngõ PR**.
2. **Không ép 1 Agent làm tất cả:** Hãy tách thành 2 pha độc lập: *Implementer* (làm cho chạy) và *Reviewer* (làm cho chuẩn, commit trực tiếp).
3. **Automated Checks phải đi kèm Automated Review:** Dùng token rẻ để bảo vệ thời gian đắt đỏ của con người.
4. **Áp dụng Deep Modules:** Giao diện nhỏ, logic sâu, kiểm thử hành vi thực tế, triệt tiêu mọi cơ hội sinh test giả mạo của AI.
5. **Vận hành Retrospectives liên tục:** Biến mọi feedback của con người thành luật tự động hóa, xây dựng hiệu ứng lãi kép cho chất lượng phần mềm.
