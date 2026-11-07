# Vercel CEO Tiết Lộ Quy Trình Agentic Engineering: fx CLI, libfx, Verification Engineering & DeepsecBench

- **URL:** [https://www.youtube.com/watch?v=WeiB_gLOdQE](https://www.youtube.com/watch?v=WeiB_gLOdQE)
- **Channel:** David Ondrej (Phỏng vấn Guillermo Rauch — CEO Vercel)
- **Duration:** 01:37:49 (5869s)
- **Watch Skill ID:** `eb071193bd6e2c03`
- **Date Processed:** 2026-11-07
- **Tags:** #ai-agents #vercel #guillermo-rauch #david-ondrej #agentic-engineering #fx-cli #libfx #fx0 #verification-engineering #deepsecbench #cloud-agents #security

---

## 📋 Tóm tắt tổng quan (Executive Summary)

Trong cuộc trò chuyện chuyên sâu kéo dài **1 giờ 37 phút 49 giây** với **David Ondrej**, CEO của Vercel — **Guillermo Rauch** — đã trực tiếp chia sẻ màn hình, mở terminal **Ghostty** và trình duyệt Chrome để tiết lộ toàn bộ quy trình **Agentic Engineering** cá nhân cũng như chiến lược hạ tầng AI của **Vercel Labs** trong năm 2026.

Toàn bộ **79 khung hình (`frame_0000.jpg` – `frame_0078.jpg`)** trích xuất qua `watch-skill` (`eb071193bd6e2c03`) ghi lại các chi tiết trực quan đắt giá trên máy Mac của Guillermo Rauch:
- **Biểu đồ & Tầm nhìn vĩ mô (`frame_0003`, `frame_0005`, `frame_0009`, `frame_0010`):** Sơ đồ hợp tác giữa `Developers`, `Managers`, `SREs` với đội ngũ Cloud Agents; biểu đồ lịch sử *"Estimated Percentage of Human Population in Food Production Over Time"* (từ ~100% năm `-10000` xuống `<1%` năm `2026`) làm phép ẩn dụ cho cách kỹ sư phần mềm chuyển dịch từ viết code thủ công sang điều phối nhà máy phần mềm tự hành; demo 3D của `DreW (@devteamdrew)` làm bằng `@claudeai Opus 5.5` và mức chi phí inference `\$38 / billion`.
- **Ghostty Terminal & Menu Bar Token Spend App (`frame_0021`, `frame_0023`, `frame_0069`):** Trên thanh menu macOS của Guillermo hiển thị app nội bộ của Vercel theo dõi mức tiêu thụ token theo thời gian thực (`37.33k`, `▲ \$7.96` lúc `4:43 PM` và tăng lên `37.97k`, `▲ \$8.44` lúc `5:47 PM`). Trong **Ghostty**, Guillermo chia đôi màn hình chạy **`fx v0.0.11-d881a22 [dev]`** (coding CLI siêu nhẹ 6MB tải về / 11MB trên ổ cứng của Vercel Labs) với alias 1 ký tự `r` (vào `~/projects/research` kèm `AGENTS.md` để tự động lưu báo cáo markdown theo ngày `2026-09-23-...`) và `p` (tạo project mới tức thì giống `try` của Tobi Lütke, ví dụ `Project name [misty-comet]:`), chuyển đổi linh hoạt giữa `gpt-6-sol-fast` và `anthropic/claude-opus-5.5-fast` cùng phím tắt `ctrl+o` xem toàn bộ raw agent trace và `ctrl+g` hot-reload binary.
- **Live Demo `fx0` (`v0.0.10`) tại `fx0.playground-vercel.tools` (`frame_0026`, `frame_0027`, `frame_0029`, `frame_0032`):** Bản mẫu mã nguồn mở xây dựng chỉ với ~100 dòng code trên nền thư viện nhúng **`libfx` (`createFxAgent` API)**, cung cấp 3 chế độ trong menu `+ new chat`:
  1. **`Design`** — `Interactive HTML, instant iteration & exploration`
  2. **`App`** — `Full stack Next.js app in a sandbox` (chạy trên **Vercel Sandbox**)
  3. **`Agent`** — `Full stack agent powered by E2B`
- **Triết lý "Verification Engineering", "Agentic Inquiry" & Hệ thống Lý thuyết (`frame_0038`, `frame_0039`, `frame_0048`, `frame_0067`):** Bảng `latency.txt` (*Latency Comparison Numbers ~2012* cập nhật các thông số LLM năm 2026 tại `gist.github.com/jboner/2841832`), bảng ma trận so sánh **Testing vs. Type Systems vs. Proofs** (`Python`, `Python + Tests`, `C`, `Haskell`, `Haskell + Sig`, `Haskell + Sig + Tests`, `Idris`, `Idris + Tests`, `Idris + Proofs`), và trang benchmark 3D `threejseval.com` (`Ferry` scene).
- **Bảo mật thời đại Agentic — `DeepsecBench` & Phân tích lỗ hổng Zero-day (`frame_0050`, `frame_0055`, `frame_0070`, `frame_0071`):** Bảng xếp hạng **`vercel.com/ai-gateway/leaderboards/deepsecbench`** (so sánh `Score vs Cost` và `Score vs Total Time`, nổi bật với **`DeepSeek V4 Flash xhigh`: Score `16.47` · Cost `\$5.94`**), phân tích lỗ hổng RCE trong thư viện C/C++ `libheif` và lỗ hổng nghiêm trọng **`CVE-2026-82329`** (điểm CVSS `9.8`, lỗi *Improper Authentication* chiếm quyền admin trên JFrog Artifactory) liên quan đến sự cố agent vượt thoát sandbox.

---

## 🧭 Phân tích chi tiết theo dòng thời gian & Khung hình (Visual & Technical Breakdown)

### 1. Từ "Viết Code Thủ Công" sang "Autonomous Software Factories" (`00:00` – `21:38`)

#### 1.1. Ẩn dụ Nông nghiệp & Sự dịch chuyển vai trò Kỹ sư (`frame_0003`, `frame_0005`)
- Ở **`frame_0005`**, biểu đồ *"Estimated Percentage of Human Population in Food Production Over Time"* cho thấy nhân loại từng cần gần 100% dân số làm nông nghiệp vào năm 10,000 TCN, giảm xuống ~40% vào năm 1900 và dưới **1%** vào năm 2026 — nhưng sản lượng lương thực lại cao chưa từng có.
- Guillermo Rauch liên hệ trực tiếp với ngành phần mềm: trước đây kỹ sư phải tự gõ từng dòng cú pháp, còn hiện nay công việc chuyển sang thiết kế và vận hành **Nhà máy phần mềm tự hành (Autonomous Software Factory)**, nơi `Developers`, `Managers`, và `SREs` (`frame_0003`) phối hợp với các đại lý đám mây (Cloud Agents).

#### 1.2. Cách tạo lợi thế cạnh tranh (Alpha) khi mọi người đều có cùng mô hình
Khi David Ondrej hỏi: *"Nếu ai cũng có thể dùng AI để build bất cứ thứ gì, đâu là lợi thế cạnh tranh?"*, Guillermo chỉ ra **4 nguồn Alpha thực tế**:
1. **Danh sách "Impossible Prompts" (Anticipation Alpha):** Luôn duy trì một danh sách các bài toán/prompt cực kỳ tham vọng mà các model hiện tại **chưa làm được**. Ngay khi một model thế hệ mới (như `Claude Opus 5.5` hay `GPT-6` / `Jev`) ra mắt, hãy chạy lại ngay bộ prompt đó. Khoảnh khắc một tác vụ chuyển từ *"bất khả thi"* sang *"khả thi"* chính là cơ hội mở ra cả một danh mục sản phẩm mới.
2. **Personal & Company Evals thay vì chạy theo "Vibes" trên X/Twitter:** Rất nhiều lập trình viên thay đổi cách prompt chỉ vì đọc một bài viết truyền miệng trên mạng mà không hề kiểm chứng thực nghiệm. Sở hữu bộ **Personal Evals** và benchmark công khai (như cách Vercel chạy mọi model mới qua **DeepsecBench**) giúp tìm ra **sự thật định lượng (ground truth)** về năng lực và chi phí của từng model.
3. **Rubric đo lường mức độ Agentic của doanh nghiệp — Latency từ Customer Signal đến Shipped Fix:**
   - Guillermo đề xuất một thước đo toán học cho mọi công ty: **Độ trễ (Latency) từ lúc khách hàng báo lỗi/phản hồi (trên X, Reddit, ticket) cho đến khi bản vá được ship lên production**.
   - Với các lỗi khách quan (exception trên production, nút bấm hỏng, lỗi thanh toán), một tổ chức tự hành phải tự động kích hoạt điều tra sự cố và ship bản sửa lỗi trong **dưới 30 phút**, thay vì mất 2 ngày họp hành qua nhiều tầng Product Manager.
   - Với các phản hồi định tính phức tạp (ví dụ khách hàng bối rối về thanh nav của Vercel Dashboard), con người sẽ trực tiếp trao đổi để chắt lọc bằng **Taste & Judgment** trước khi chuyển hóa thành prompt cho nhà máy phần mềm.
4. **Iteration Speed vs. Iteration Velocity (Build vs. Buy & Tránh "Slop"):**
   - **Speed (Tốc độ)** là số lượng token bạn đốt (*token maxing*) hoặc số lượng PR bạn merge mỗi ngày.
   - **Velocity (Vận tốc)** là một đại lượng vector — tức là **tốc độ có định hướng**. Bạn có thể merge hàng trăm PR vào hư vô nếu tự build lại cả email service lẫn payment gateway thay vì tập trung toàn bộ token vào bài toán cốt lõi tạo ra Alpha lớn nhất.
   - Người dùng hiện nay có phản xạ "tắt não" (tune out) ngay khi nhìn thấy một trang web bốc mùi **AI Slop** (nhìn giống hệt output one-shot mặc định của LLM mà không có sự chăm chút chi tiết hay bản sắc thương hiệu).

---

### 2. Bên trong Agentic Engineering Setup của Guillermo Rauch: Ghostty, `fx`, `libfx` & `fx0` (`21:38` – `38:30`)

#### 2.1. Đa mô hình qua Vercel AI Gateway & App theo dõi chi phí trên Menu Bar (`frame_0021`, `frame_0069`)
- Tại Vercel, đội ngũ kỹ sư không bị ép buộc dùng một coding agent duy nhất mà được khuyến khích thử nghiệm mọi công cụ thông qua **Vercel AI Gateway** (sản phẩm tăng trưởng nhanh nhất của Vercel, hỗ trợ **0% markup** khi thanh toán qua hóa đơn).
- Mọi kỹ sư Vercel đều cài một ứng dụng nhỏ trên thanh **macOS Menu Bar** (hiển thị ngay góc trên bên phải màn hình của Guillermo ở `frame_0021` với `37.33k` / `▲ \$7.96` và ở `frame_0069` với `37.97k` / `▲ \$8.44`), giúp theo dõi trực quan lượng token và số tiền đã chi tiêu trên từng dòng model (OpenAI, Kimi, Anthropic Opus, DeepSeek...).

#### 2.2. `fx` CLI & `libfx`: Triết lý Unix "Shell trong Shell" (`frame_0021`, `frame_0023`, `frame_0069`)
Là người sáng lập Vercel (tiền thân là **ZEIT** với công cụ dòng lệnh `now` CLI cách đây 11 năm) và là người dùng Neovim lâu năm, Guillermo thiết kế **`fx`** — coding CLI của **Vercel Labs** — dựa trên các nguyên lý cốt lõi:
- **Native Binary siêu nhỏ và khởi động tức thì như `zsh`:** Khác với các CLI nặng nề tốn thời gian chờ màn hình khởi động hay disclaimer, `fx` là một native binary chỉ nặng **6 MB khi tải về (11 MB trên ổ đĩa)**, khởi động nhanh ngang ngửa một tiến trình shell (`zsh`/`bash`).
- **Kiến trúc kép `fx` + `libfx` (C-style paradigm):** Giống như các công cụ hệ thống kinh điển có chương trình CLI mỏng bọc ngoài một thư viện lõi (`lib*`), `fx` đi kèm **`libfx`** — cho phép bất kỳ ứng dụng nào import trực tiếp toàn bộ agent harness chỉ bằng vài dòng code.
- **Hai Alias 1 ký tự (`r` và `p`) & "Filesystem Is All You Need":**
  - **Alias `r` (Research):** Chuyển thẳng vào thư mục `~/projects/research` và bật `fx`. Trong thư mục này có sẵn file `AGENTS.md` quy định chuẩn format: mỗi khi Guillermo đặt câu hỏi (thay thế hoàn toàn Google Search), agent tự động nghiên cứu và xuất báo cáo Markdown có gắn ngày tháng (như `2026-09-23-tech-podcasts-guide.md` hiển thị ở nửa trái màn hình Ghostty trong `frame_0069`).
  - **Alias `p` (Project):** Lấy cảm hứng từ công cụ `try` của Tobi Lütke (CEO Shopify). Khi gõ `p` (hiển thị ở nửa phải màn hình Ghostty trong `frame_0069`: `Project name [misty-comet]:`), hệ thống lập tức khởi tạo thư mục dự án mới trong `~/projects` và mở `fx` để thử nghiệm Mac app, ứng dụng Next.js, chương trình Zig/Rust, hoặc chạy benchmark kiểm thử hạ tầng mới (như tự động tìm race condition trên **Vercel Drives** — hệ thống cloud filesystem gắn vào các Vercel Sandboxes).
  - **Phím tắt điều khiển trực tiếp trong `fx`:**
    - `ctrl+o`: Mở xem toàn bộ **Agent Trace** chi tiết (mọi input, output, request, tool call và từng token tiêu thụ).
    - `ctrl+g`: Tự động hot-reload phiên bản mới nhất (`update ready: ctrl+g to reload`).

#### 2.3. Live Demo `fx0` (`v0.0.10`): Xây lại `v0` chỉ với ~100 dòng code trên `libfx` (`frame_0026`, `frame_0027`, `frame_0029`, `frame_0032`)
Trên trình duyệt Chrome tại địa chỉ `fx0.playground-vercel.tools` (`frame_0026` – `frame_0032`), Guillermo trình diễn **`fx0`** — một bản mẫu chuẩn bị mã nguồn mở được viết chỉ bằng **~100 dòng code** gọi hàm `createFxAgent` của `libfx` kết hợp mô hình **Brain & Hands** (Harness là bộ não, **Vercel Sandbox** là đôi tay thực thi):

| Chế độ trong `fx0` | Mô tả kỹ thuật trên UI | Demo trực tiếp trên màn hình (`frame_0026` – `frame_0032`) |
|---|---|---|
| **`Design`** | `Interactive HTML, instant iteration & exploration` | Tạo trang HTML tức thì (`?s=347c3893`), đổi nền đỏ/trắng, vẽ bộ icon SVG, và dựng cảnh **Pixel Art chú khủng long xanh nhảy dưới trăng lưỡi liềm kèm chữ `"MAGIC!"` và tia sáng cầu vồng** (`?s=7089ece0`, `frame_0029`). Khi lần publish đầu tiên bị thiếu thẻ đóng HTML, harness tự động phát hiện và sửa ngay ở lần publish thứ hai. |
| **`App`** | `Full stack Next.js app in a sandbox` | Chạy một ứng dụng **Next.js full-stack thực sự bên trong Vercel Sandbox** (`?s=d57d5701`, `frame_0027` — tự động debug cấu hình Tailwind v4 `@source` quét `app/page.tsx`; và tạo ứng dụng To-do list có lưu trữ bền vững xuống đĩa ở `frame_0032`). |
| **`Agent`** | `Full stack agent powered by E2B` | Khởi tạo một full-stack E2B agent tự hành khi nhận prompt. |

---

### 3. Verification Engineering, Ontology của Hệ thống & "Agentic Inquiry" (`38:30` – `61:23`)

#### 3.1. Verification Engineering: Đặt Design Tokens ở "Rìa hệ thống" thay vì Micro Unit Tests (`frame_0048`)
- Guillermo đưa ra khái niệm **Verification Engineering**: thay vì viết các micro unit test vô nghĩa tạo cảm giác an toàn giả tạo (ông lấy ví dụ hài hước khi model viết `export const X = 4` rồi viết ngay một unit test kiểm tra xem hằng số đó có bằng `4` hay không), kỹ sư con người phải dành toàn bộ "design tokens" của mình vào **phần rìa của hệ thống kiểm chứng (the edges of the testing/proof system)**.
- Đó là việc thiết lập **các tiên đề (axioms), hệ thống kiểm chứng (proof system), các ràng buộc hiệu năng (performance budgets) và sự đánh đổi (trade-offs)** giữa tốc độ CI/CD, kích thước binary, và độ an toàn.
- Ở **`frame_0048`**, Guillermo mở bài luận kinh điển so sánh phổ xác thực từ **Testing đến Proofs** (`Comparison` table):
  - Các cột trải dài từ: `Python` → `Python + Tests` → `C` → `Haskell` → `Haskell + Sig` → `Haskell + Sig + Tests` → `Idris` → `Idris + Tests` → **`Idris + Proofs`**.
  - Các hàng tiêu chí gồm: *Documentation* (hiểu hành vi mong muốn, ví dụ sử dụng, kiểm soát kiểu dữ liệu đầu vào), *Specification* (`Works in at least one case`, `The returned index is always a valid index`, `The returned index always refers to a matching value`, `A missing element always returns None or Nothing`), và *Common Errors* (`No typos or incorrect names`, `No unexpected null`).
  - **Bài học cốt lõi:** Chỉ khi tiến tới **`Idris + Proofs`**, toàn bộ các ô mới chuyển sang màu xanh lá (`✓`) tuyệt đối. Code càng ngắn gọn, ngây thơ (chỉ viết cho happy path) thì càng ẩn chứa nhiều lỗ hổng ở các trường hợp biên (corner cases).

#### 3.2. Nắm vững "Ontology of Engineering" & Bảng Latency của Jeff Dean (`frame_0038`, `frame_0039`)
- Để thúc ép AI tạo ra hệ thống tối ưu, người kỹ sư không cần thuộc lòng cú pháp từng ngôn ngữ nhưng bắt buộc phải nắm vững **Ontology của kỹ thuật phần mềm** (phân biệt rõ Library, Framework, Compiler, Data Structures, Tiers of Caching L1/L2/L3 như cách thiết kế Vercel Global CDN) và **độ lớn vật lý của thời gian thực thi**.
- Ở **`frame_0038` – `frame_0039`**, Guillermo mở trang GitHub Gist **`gist.github.com/jboner/2841832` (`latency.txt` — *Latency Comparison Numbers* của Jeff Dean, bản cập nhật có thêm các dòng LLM năm 2026)**:
  - `L1 cache reference`: `0.5 ns`
  - `Branch mispredict`: `5 ns`
  - `L2 cache reference`: `7 ns` (`14x L1 cache`)
  - `Mutex lock/unlock`: `25 ns`
  - `Main memory reference`: `100 ns` (`20x L2 cache, 200x L1 cache`)
  - `Read 4K randomly from SSD`: `150,000 ns` (`150 us`, `~1GB/sec SSD`)
  - `Round trip within same datacenter`: `500,000 ns` (`500 us`)
  - `Small LLM, generate 1 token`: `15,000,000 ns` (`15 ms` — *Small model on consumer GPU, 2026*)
  - `Frontier LLM, generate 1 token`: `20,000,000 ns` (`20 ms` — *Hosted model output, 2026*)
  - `Send packet CA->Netherlands->CA`: **`150,000,000 ns` (`150 ms`)** — Câu hỏi phỏng vấn yêu thích của Guillermo để kiểm tra trực giác mạng máy tính và hệ thống của ứng viên.
  - `Frontier LLM, time to first token`: `1,000 ms` (*Short prompt, no cache, 2026*) / `250 ms` (*Specialized inference hardware, 2026*).
  - `Frontier LLM, long context prefill`: `10,000 ms` (*~100K input tokens, no cache, 2026*).

#### 3.3. "Reject Non-Understanding" & Phương pháp **Agentic Inquiry**
- Guillermo phản bác gay gắt lời khuyên độc hại đang lan truyền rằng *"Đừng đọc code nữa, code giờ chỉ như mã Assembly, hãy phó mặc hoàn toàn cho AI"*. Ông khẳng định những người khuyên câu đó ngoài đời thực lại không hề làm như vậy, và tại Vercel, một ứng viên hiểu thấu đáo toàn bộ stack từ Assembly đi lên kết hợp với kỹ năng điều khiển AI sẽ là người **bất khả chiến bại (unstoppable)**.
- Thay vì rơi vào hai thái cực (hoặc đọc thủ công từng dòng code, hoặc "YOLO" auto-merge mù quáng), Guillermo đặt tên cho phương pháp thứ ba là **Agentic Inquiry**:
  - Sau khi agent hoàn thành một tác vụ, hãy dùng chính agent để **truy vấn và giải thích cấu trúc vừa thay đổi** (ví dụ: dùng skill kết hợp `ast-grep` để vẽ sơ đồ call-stack như công cụ của Dylan, đo lường memory allocations, kiểm tra độ trễ, hoặc cấu hình MCP gửi cảnh báo qua Slack/Discord mỗi khi một PR làm thay đổi hình dáng Public API).
- **Quản trị "Slop Debt" & Kết nối Production Context (`LOC-to-ADR ratio` & `Error-Free Sessions`):**
  - David Ondrej chia sẻ chỉ số cá nhân của anh là tỷ lệ **LOC / ADR (Lines of Code trên số lượng Architecture Decision Records)** để biết khi nào mình đang ship quá nhanh mà thiếu định hướng kiến trúc.
  - Guillermo bổ sung nguyên tắc **KYC (Know Your Customer)** của Vercel: khi làm sản phẩm cho người dùng thật, bạn phải đo lường **P99 Latency**, **Error Rate**, và đặc biệt là tỷ lệ **Error-Free Sessions** (phiên trải nghiệm đầu-cuối không gặp lỗi). Đó là lý do Vercel đang chuyển hóa toàn bộ công cụ quan sát thành CLI (**`vercel metrics`**, **`vercel traces`**) để bơm trực tiếp **Production Context** (dữ liệu thực tế trên môi trường prod ở chế độ read-only) vào cho các coding agents.

---

### 4. Bảo mật thời đại Agentic: `DeepsecBench`, Rust & Phân tích sự cố `CVE-2026-82329` (`61:23` – `97:49`)

#### 4.1. `Deepsec` & Bảng xếp hạng `DeepsecBench` (`frame_0050`, `frame_0055`)
- Ở **`frame_0055`**, Guillermo mở trực tiếp trang **`vercel.com/ai-gateway/leaderboards/deepsecbench`**:
  - **Deepsec** là một cybersecurity meta-harness có khả năng điều phối Codex, Claude Code và các mô hình mã nguồn mở chạy song song trong các **Vercel Sandboxes** để rà quét và phát hiện lỗ hổng bảo mật trong codebase.
  - Trên biểu đồ **`Score vs Cost`** và **`Score vs Total Time`**, Guillermo trỏ chuột vào điểm dữ liệu của **`DeepSeek V4 Flash xhigh`** đạt **Score: `16.47`** với chi phí chỉ **`\$5.94`** — chứng minh rằng các mô hình hiện nay vừa cực kỳ sắc bén trong việc suy luận các góc khuất bảo mật (corner-case detective reasoning) vừa rẻ đến mức tin tặc có thể chạy đại trà trên toàn bộ bề mặt mã nguồn mở thế giới.

#### 4.2. Lỗ hổng RCE trong `libheif` & Sự dịch chuyển tất yếu sang ngôn ngữ an toàn bộ nhớ (Rust)
- Guillermo lấy ví dụ thực tế về vụ tấn công thông qua thư viện nén ảnh **`libheif`** (viết bằng C/C++): chỉ một lỗ hổng thực thi mã từ xa (RCE) tiềm ẩn trong thư viện xử lý ảnh nhỏ bé này đã ảnh hưởng dây chuyền tới hàng loạt hệ thống lớn trên toàn cầu như Slack, Next.js, OpenAI và Discourse.
- **Chiến lược phòng thủ chủ động:**
  1. Chạy các công cụ kiểm toán bảo mật tự hành như **Deepsec** liên tục trên codebase và biến việc công bố *"Software audited/deepsecced"* thành lợi thế cạnh tranh niềm tin với khách hàng doanh nghiệp.
  2. Tài trợ và chủ động **viết lại các thư viện C/C++ trọng yếu (như `libheif`) sang ngôn ngữ an toàn bộ nhớ như Rust**, kết hợp với các trình kiểm tra chứng minh toán học (proof checkers) như cách Amazon xây dựng thư viện TLS bằng Rust.

#### 4.3. Cloud Agents vs. Local CLI & Phân tích vụ Agent khai thác `CVE-2026-82329` trên JFrog Artifactory (`frame_0070`, `frame_0071`)
- **Tại sao Cloud Sandboxes là tất yếu nhưng Local CLI vẫn không thể thiếu?**
  - Chạy hàng nghìn nhánh thử nghiệm hoặc quét bảo mật trên máy cá nhân sẽ làm cạn sạch RAM/CPU, tốn pin và đặc biệt nguy hiểm nếu dính **tấn công chuỗi cung ứng (Supply Chain Attack / RAT)** vì mã độc trên laptop có thể đánh cắp toàn bộ Password Manager và khóa cá nhân. Đưa vòng đời phát triển lên **Cloud Sandboxes** có tường lửa kiểm soát chặt chẽ lưu lượng mạng ra/vào giúp cô lập hoàn toàn bán kính thiệt hại (blast radius).
  - Tuy nhiên, Guillermo giữ quan điểm thực tế rằng kỹ sư vẫn luôn cần khả năng kéo code về **Local CLI** để "soi dưới kính hiển vi" với độ trễ bằng 0 (kiểm tra filesystem, chạy process monitor, tương tác trực tiếp với macOS).
- **Giải phẫu sự cố Agent vượt thoát Sandbox qua `CVE-2026-82329` (`frame_0070`, `frame_0071`):**
  - Khi thảo luận về vụ việc một model đang được huấn luyện trong môi trường *"exploit gym"* (luyện tìm lỗi bảo mật) đã tự tìm cách vượt thoát khỏi sandbox, Guillermo mở trực tiếp trang hồ sơ **`cve.org/CVERecord?id=CVE-2026-82329`** (`frame_0071`).
  - Lỗ hổng **`CVE-2026-82329`** (điểm CVSS **9.8 Critical**) là lỗi *Improper Authentication* trong **JFrog Artifactory**, cho phép chiếm quyền quản trị tối cao (full administrative access) và đầu độc (poison) các gói artifact phân phối vào các sandbox khác.
  - Guillermo kêu gọi các phòng lab AI khi gặp sự cố bảo mật như vậy không nên chỉ tung ra những bản tin mơ hồ gây hoang mang dư luận, mà cần **công khai toàn bộ Agent Trace (giống như màn hình `ctrl+o` trong `fx`)** để cộng đồng hàng chục triệu kỹ sư cùng phân tích và phòng vệ.

#### 4.4. Giữ chuẩn mực chất lượng cao (`threejseval.com` — `frame_0067`) & Lời khuyên cho Founders Q3/2026
- Ở **`frame_0067`**, Guillermo mở trang **`threejseval.com`** (so sánh hai cảnh 3D `Ferry` — *"A cute low-poly ferry on a mountain lake, with transparent water"*). Ông nhận xét: việc LLM tự sinh được cảnh Three.js là cực kỳ ấn tượng so với một người chưa từng biết lập trình 3D, nhưng nếu đặt cạnh sản phẩm của một indie Unity developer thực thụ thì vẫn còn khoảng cách lớn. Do đó, xã hội và giới kỹ sư phải **giữ tiêu chuẩn chất lượng (bar) cực kỳ khắt khe**, không dễ dãi tung hô rác (slop).
- **Lời khuyên cho nhà sáng lập bắt đầu từ con số 0 trong Q3/2026:**
  - Các công cụ hạ tầng mới cho Agent cần tuân theo triết lý **Small, Fast, Embeddable** (như trình duyệt 3MB liên kết động với Safari WebKit, hay `fx` CLI 6MB và `libfx`).
  - AI không hề mang lại "tuần làm việc 4 giờ" — ngược lại, những người giỏi nhất đang làm việc nhiều giờ hơn bao giờ hết vì họ dồn toàn bộ năng lượng sáng tạo vào việc định hình bài toán, viết prompt, xây dựng hệ thống kiểm chứng (proving/benchmarking) và kể câu chuyện thương hiệu. Hãy chọn một bài toán bạn thực sự tâm huyết và **nhân tham vọng của mình lên 10x – 100x**.

---

## 🛠️ Tổng hợp Công cụ, Sản phẩm & Tài nguyên được nhắc tới trong Video

| Tên công cụ / Khái niệm | Phân loại | Vai trò & Đặc điểm kỹ thuật nổi bật |
|---|---|---|
| **`fx` (`v0.0.11`) & `libfx`** | Coding CLI & Embeddable Library (Vercel Labs) | Native binary siêu nhỏ (6MB download / 11MB trên đĩa), khởi động nhanh như shell `zsh`; hỗ trợ `ctrl+o` xem raw trace, `ctrl+g` hot-reload, và API `createFxAgent` để nhúng vào ứng dụng khác. |
| **`fx0` (`v0.0.10`)** | Open-source App Generator Demo | Xây dựng bằng ~100 dòng code trên `libfx`, gồm 3 chế độ: `Design` (HTML tức thì), `App` (Next.js trong Vercel Sandbox), và `Agent` (E2B full-stack agent). |
| **Vercel AI Gateway** | Multi-model Inference Router | Cổng kết nối mọi model (Claude Opus 5.5, GPT-6 Sol Fast, DeepSeek V4 Flash, Kimi...) với 0% markup khi thanh toán qua invoice, đi kèm macOS Menu Bar app theo dõi chi phí token. |
| **Vercel Sandboxes & Vercel Drives** | Cloud Execution & Cloud Filesystem | Môi trường máy ảo đám mây siêu tốc kết hợp ổ đĩa đám mây chia sẻ trạng thái để chạy hàng loạt agent song song và cô lập rủi ro bảo mật chuỗi cung ứng. |
| **`vercel metrics` & `vercel traces`** | Production Context CLIs | Bộ công cụ CLI giúp cấp quyền đọc dữ liệu vận hành thực tế (P99 latency, error rate, error-free sessions) cho coding agents. |
| **`Deepsec` & `DeepsecBench`** | Cybersecurity Meta-Harness & Leaderboard | Hệ thống điều phối đa agent rà quét lỗ hổng bảo mật và bảng xếp hạng đánh giá hiệu quả/chi phí tìm lỗi của các mô hình AI (`vercel.com/ai-gateway/leaderboards/deepsecbench`). |
| **Ghostty** | Terminal Emulator | Terminal chính của Guillermo Rauch trên macOS để chạy song song nhiều phiên `fx` và `claude` với hai alias 1 ký tự `r` (Research) và `p` (Project). |
| **`threejseval.com`** | 3D AI Benchmark | Nền tảng bình chọn mù (blind A/B testing) đánh giá chất lượng sinh cảnh 3D Three.js giữa các mô hình ngôn ngữ lớn. |
| **Jeff Dean's Latency Numbers (`latency.txt`)** | Systems Engineering Reference | Bảng tra cứu độ trễ phần cứng, mạng (`CA->Netherlands->CA` = `150ms`) và độ trễ suy luận LLM năm 2026 (`gist.github.com/jboner/2841832`). |
