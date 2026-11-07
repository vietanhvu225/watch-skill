# ⚡ Chế Độ Kỹ Sư Chính Thức Của Claude: Vòng Lặp "Hill Climbing" Giúp Anthropic Tăng Tốc 3.1X Trải Nghiệm Web — Phân Tích Chuyên Sâu

- **Source:** https://www.youtube.com/watch?v=0UQme-kv6zg
- **Uploader:** AICodeKing
- **Duration:** 11:23 (683.3s)
- **Watch Skill ID:** `cd837608618f23e8`
- **Date Processed:** 2026-11-07
- **Frames Analysed:** 66/66 kept frames (từ 95 candidates, OCR trích xuất trên 65 frames)

---

## Executive Summary: Sprint 2 Tuần Của Anthropic & 3,000+ PRs Không Một Sự Cố

Video của **AICodeKing** giải phẫu bộ 3 tài liệu kỹ thuật chính thức vừa được đội ngũ **Anthropic Engineering** công bố (`frame_0001`–`frame_0015`):
1. **"How we made claude.ai 3x faster in two weeks"** — Tác giả: **Raymond Wang, Sam Attard, và Issac G.** (xuất bản ngày 23/09/2026, 15 phút đọc — `frame_0001`). Khẩu hiệu cốt lõi: *"Once Claude can measure something, it can make it faster. So we kept finding more things to measure."*
2. **"Loop engineering: Getting started"** (`claude.com` — hướng dẫn sử dụng lệnh `/goal` với tiêu chí tất định — `frame_0006`–`frame_0007`).
3. **"Automating eval design and hillclimbing with Claude"** — Tác giả: **Lance Martin** (xuất bản ngày 28/09/2026, 12 phút đọc — hướng dẫn lệnh `build-eval` và `hillclimb` trong `claude-api` skill — `frame_0015`–`frame_0016`).

### Kết quả thực tế trên 4 hành trình người dùng (chiếm 95% hoạt động — `frame_0004`–`frame_0005`, `frame_0014`):
- **Trung bình nhân (Geometric mean) của 13 phép đo trên 4 hành trình:** Nhanh hơn **3.1x** (`180 → 59 ms` [-67%], `140 → 64 ms` [2.2x, -54%], `928 → 48 ms` [19x, -95%], `250 → 52 ms` [4.8x, -79%] — `frame_0014`).
- **Tại phân vị thứ 75 (`P75`, tức 15/20 lần tải nhanh bằng hoặc hơn mức này — `frame_0004`–`frame_0005`):**
  - Thời gian đến khi trang gõ phím được (`time to a typeable page on a fresh load`) trên `claude.ai`: giảm từ **3.1 giây xuống 0.55 giây**.
  - Khởi tạo phiên **Claude Code** mới: giảm từ **0.8 giây xuống 0.3 giây**.
  - Tải phiên đám mây **Claude Cowork**: giảm từ **2.6 giây xuống 0.73 giây**.
  - Tổng hợp lại giúp tiết kiệm **hàng chục nghìn giờ chờ đợi của người dùng mỗi ngày**.
- **Quy mô vận hành (`frame_0014`, `frame_0054`–`frame_0057`):** Chạy từ một kênh Slack duy nhất (`#claude-apps-perf`) sử dụng **Claude Tag (beta)** với model nghiên cứu nội bộ tương đương **`Opus 5.5`**, mở đồng thời **hơn 150 threads** (`153 active in #claude-apps-perf`), hợp nhất **hơn 3,000 PRs** mà **không xảy ra bất kỳ sự cố hướng khách hàng (customer-facing incident) hay lần rollback nào**.

```
+----------------------------------------------------------------------------------------------------+
|               VÒNG LẶP KỸ SƯ CHÍNH THỨC CỦA ANTHROPIC (HILL CLIMBING LOOP)                         |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ 1. STANDING INSTRUCTIONS TRONG SLACK (#claude-apps-perf) ] (frame_0010 - frame_0012)            |
|    • Ghim trách nhiệm thường trực cho @Claude: giám sát deploy, tìm điểm nghẽn, tự tạo benchmark.  |
|    • Định nghĩa tiêu chí dừng tất định qua `/goal` (Evaluator model kiểm tra mỗi khi Claude dừng). |
|                                     │                                                              |
|                                     ▼                                                              |
|  [ 2. ĐO LƯỜNG TẤT ĐỊNH BẰNG VALGRIND CALLGRIND ] (frame_0019 - frame_0025)                        |
|    • Thay vì bấm giờ đồng hồ (Wall-clock nhảy loạn 38.6ms - 52.9ms), dùng `node --predictable`     |
|      kết hợp `valgrind --tool=callgrind` để đếm chính xác số lệnh máy CPU (`1,000,412,377 Ir`).    |
|                                     │                                                              |
|                                     ▼                                                              |
|  [ 3. KHÓA CHẶT TIẾN BỘ BẰNG PERFORMANCE RATCHET TRONG CI ] (frame_0019, frame_0027 - frame_0029)  |
|    • Mỗi khi số lệnh giảm, cronjob hàng ngày tự động hạ trần (ceiling) trong CI.                   |
|    • Bất kỳ PR nào về sau làm tăng số lệnh vượt trần đều bị `perf-bot` đánh trượt CI ngay lập tức! |
|                                     │                                                              |
|                                     ▼                                                              |
|  [ 4. CON NGƯỜI CẦM LÁI: AMBITION · TASTE · DIRECTION ] (frame_0058 - frame_0061)                  |
|    • 150+ threads song song ("150 cái búa tìm đinh"), mỗi thread có kỹ sư sở hữu & Staged Rollout. |
|    • Bác bỏ thẳng tay PR 900 dòng chỉ tiết kiệm được 2ms mỗi lần gửi vì chi phí bảo trì quá đắt!   |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 1. Hạ Tầng Điều Phối: `/goal`, Claude Tag Trong Slack & Nguyên Tắc Thiết Kế Eval

### 1.1. Lệnh `/goal` Với Tiêu Chí Tất Định (`frame_0006`–`frame_0007`)
Trong tài liệu *Loop engineering*, Anthropic giải thích cách giữ Agent lặp lại công việc mà không tự ý dừng sớm vì nghĩ là "đủ tốt":
- Mỗi khi Claude định dừng, một **evaluator model** sẽ kiểm tra điều kiện của bạn và đẩy nó quay lại làm việc cho đến khi đạt mục tiêu hoặc chạm giới hạn số lượt (`turns`).
- Ví dụ lệnh chuẩn (`frame_0006`):
  ```text
  /goal get the homepage Lighthouse score to 90 or above, stop after 5 tries
  ```
- Nguyên tắc cốt lõi mà AICodeKing ghi chú trong `perf-notes.md` (`frame_0008`, `frame_0017`–`frame_0018`):
  > **`> judge the result, not the line count`**
  > - **Did it actually help?** (Nó có mở nhanh hơn cho người dùng không? Nó vẫn hoạt động đúng chứ?)
  > - **One experiment = one change:** Khi đổi `the search filter`, phải giữ cố định `data set · browser · network` để biết chính xác công trạng thuộc về thay đổi nào.

### 1.2. Standing Instructions Trong Kênh `#claude-apps-perf` (`frame_0010`–`frame_0012`)
Thay vì phải giải thích lại ngữ cảnh mỗi khi có việc, team ghim chỉ thị thường trực cho `@Claude` ngay đầu kênh Slack (`frame_0010`):
> *"@Claude Your job is to facilitate all things related to the performance of the claude.ai website and desktop app. Your responsibilities include monitoring deploys for performance regressions, assessing the accuracy and comprehensiveness of existing telemetry, maintaining well-curated observability dashboards, proactively implementing solutions for observed issues and low-hanging fruit, proposing performance project opportunities, and communicating with your human teammates. [...] The ultimate goal for this channel is for you to become as autonomous as possible, but today we know that isn't yet possible."*

Trong kênh này, các kỹ sư trao đổi trực tiếp với Claude cùng bằng chứng cụ thể:
- **Sam (`12:34 PM`):** *"What can we do instead of wall-clock timing? Can we measure JS instruction counts for instance?"*
- **Raymond (`2:33 PM`):** *"this is actually kind of a sick bench. are we capped at 60 fps? can you try to drive scroll and stream smoothness to 120? iiuc your rig may not support this"*
- **Marius (`6:15 PM` — `frame_0011`–`frame_0012`):** Báo cáo hiện tượng khung composer bị đẩy dịch xuống `15–20px` khi mở tab mới kèm video quay màn hình (`Screen recording 0:02`). Chỉ 50 phút sau (`7:05 PM`), **Claude** phân tích xong video và chỉ ra thủ phạm thực sự: không phải lỗi handoff của static composer (vốn đo được `0 px` trên cả 49 lần tải trong ngày), mà do **Chrome tự vẽ một thanh footer cao `56 px`** (*"Managed by anthropic.com · Customize Chrome"*) ở trang New Tab rồi ẩn đi sau `~100 ms` khi điều hướng sang `claude.ai`!

### 1.3. Chống Nhiễu (Eval Noise) & Chống Học Vẹt (Overfitting) Khi Hill Climbing (`frame_0015`–`frame_0016`, `frame_0051`)
Theo playbook của **Lance Martin** (`frame_0016`, `frame_0051`):
- **Chia ngẫu nhiên tập `train` và `test`:** Trước vòng đầu tiên, Claude kiểm tra xem **độ nhiễu của bài đánh giá (`eval's noise` — mức biến động điểm số do ngẫu nhiên)** có nhỏ hơn mức cải thiện nhỏ nhất mà bạn muốn hành động hay không. Nếu nhiễu quá lớn, nó yêu cầu tăng số lần lặp hoặc số test cases.
- **Kiểm tra Overfitting sau mỗi bản vá (`frame_0016`):** Nếu điểm tập `train` tăng nhưng tập `test` đi ngang (`flat`), Claude nghi ngờ đã bị overfitting và **tự động revert bản vá**. Chỉ khi cả hai tập `train` và `test` cùng cải thiện thì bản vá mới được giữ lại.
- **Rò rỉ bài test vào Harness (`eval leak` — `frame_0051`):** Ví dụ nếu bài eval hưởng lợi từ OCR nhưng thực tế production hiếm khi cần OCR, quá trình hillclimbing có thể tự gắn thêm OCR tool vào harness chỉ để ăn gian điểm benchmark mà không giúp ích gì cho người dùng thật.

---

## 2. Đo Lường Tất Định Bằng Valgrind Callgrind & Cơ Chế "Bánh Cóc" (Performance Ratchet)

### 2.1. Tại sao Đồng hồ bấm giờ (Wall-Clock Time) thất bại? (`frame_0019`–`frame_0025`)
Khi chạy cùng một bài benchmark `node bench/message-tree.mjs` 5 lần liên tiếp trên máy MacBook Pro (`frame_0020`), thời gian thực (`wall clock, 200 iterations`) nhảy loạn xạ: **`41.8 ms`, `38.6 ms`, `47.2 ms`, `40.3 ms`, `52.9 ms`**. Sự dao động do tiến trình nền của máy khiến Agent không thể biết một thay đổi nhỏ là cải tiến thật hay nhiễu.

**Giải pháp của Anthropic (`frame_0019`, `frame_0023`–`frame_0025`):**
1. Yêu cầu Claude tối ưu 2 luồng nóng (`hot paths`): hàm lắp ghép cây tin nhắn hội thoại (`message-tree assembly`) và bộ quét dòng trạng thái (`status-scan`) trong Claude Code output.
2. Chạy Node ở chế độ tất định kết hợp bộ đếm lệnh máy **Callgrind (Valgrind)**:
   ```bash
   valgrind --tool=callgrind node --predictable bench/message-tree.mjs
   callgrind_annotate --inclusive=yes callgrind.out.48211 | head -8
   ```
3. Kết quả thu được hoàn toàn bất biến qua các lần chạy (`frame_0025`): cả lần chạy PID `48211` lẫn PID `48377` đều trả về chính xác **`Collected : 1,000,412,377`** lệnh máy (`Ir`), trong đó chỉ rõ `src/messageTree.ts:resolveMessageId` chiếm **25% (`251,604,912 Ir`)** vì tra cứu từ điển đa hình (`megamorphic dictionary lookups`) lặp lại **3 lần trên mỗi node** (`3 calls per node`).
4. Chỉ **1 giờ sau** (`frame_0019`), Claude cắt giảm số lệnh máy trên 2 luồng nóng lần lượt **48%** và **31%**, kéo thời gian thực (`wall-clock time`) giảm **78% (nhanh gấp 4.6x)** và **44% (nhanh gấp 1.8x)**.

> **Lưu ý về Cạm bẫy Tối ưu sai chỗ (Delivery Service Analogy — `frame_0026`):** Giống như dịch vụ giao hàng có 2 khâu: *Làm giấy tờ (`Paperwork`)* và *Chờ xe tải (`Waiting for the van`)*. Dù bạn có làm giấy tờ nhanh gấp đôi nhưng kiện hàng vẫn nằm chờ xe tải cả ngày thì khách hàng không thấy sự khác biệt nào. Đếm lệnh máy phải luôn đi kèm kiểm chứng thời gian thực và dữ liệu người dùng thực tế (`Field data` vs `Lab data` — `frame_0030`).

### 2.2. Khóa Chặt Thành Quả Bằng "Performance Ratchet" Trong CI (`frame_0019`, `frame_0027`–`frame_0029`)
Để ngăn chặn việc một kỹ sư khác vô tình làm chậm lại hệ thống vào 6 tuần sau, Anthropic đưa 2 chốt chặn (`ratchets`) vào CI:
- Bất kỳ PR nào làm tăng số lệnh máy vượt trần (`ceiling`) đều bị đánh trượt CI, và **một tiến trình chạy hàng ngày (`daily job`) sẽ tự động hạ thấp mức trần xuống mỗi khi số lệnh giảm** (`frame_0019`).
- Minh họa thực tế trên GitHub PR `#198` (*"Refactor message store selectors"* — `frame_0027`) và PR `#231` (*"Add emoji picker to composer"* — `frame_0029`):
  - Khi trần `message-tree` đã được hạ từ `1,000M` xuống **`700M`**, PR `#231` làm vọt số lệnh lên **`760M`** (`✗ regression +8.6%`). Dù `ci/unit-tests` và `ci/lint` đều xanh, **`perf-bot` lập tức chặn đứng PR**!

---

## 3. Bốn Đột Phá Kiến Trúc Frontend Giúp Đạt 120 FPS & Cắt Giảm 90% Re-render

Chỉ đến **ngày thứ 3 của sprint**, đội ngũ đã đạt được **12/13 mục tiêu ban đầu** (`frame_0031`):

### 3.1. Static HTML Composer + V8 Code Cache (`frame_0031`–`frame_0036`)
- **Giải pháp:** Nhúng sẵn một khung nhập liệu tĩnh (`static composer`) прямо vào HTML trả về đầu tiên để người dùng có thể gõ phím ngay trong lúc React đang khởi tạo (`type during React initialization`), đồng thời biên dịch trước **V8 code cache** để tiến trình chính của ứng dụng Desktop không phải biên dịch lại từ đầu.
- **Kết quả trên mạng 4G bóp băng thông (`throttled 4G` — `frame_0032`):** Thời gian có thể bắt đầu gõ phím giảm từ **`2.90s` xuống `0.95s`**, sau đó khung composer thật của React sẽ hòa trộn (`fades in`) ở `2.9s–3.14s` và **giữ nguyên toàn bộ văn bản người dùng đã gõ** (`typed text kept`).
- **Hàng chục chốt bảo vệ Handoff (`frame_0032`–`frame_0036`):** *"If the React render is off by even a pixel, the magic is lost."* Khi bàn giao từ HTML tĩnh sang `hydrateRoot()` của React, khung nhập phải khớp từng pixel, giữ nguyên focus con trỏ và không làm rơi bất kỳ ký tự nào.

### 3.2. Cắt Giảm 90% Lần Vẽ Lại Sidebar (`cut sidebar re-renders by 90%` — `frame_0031`, `frame_0037`–`frame_0039`)
- Giữ khung composer luôn được gắn (`mounted`) khi chuyển đổi giữa các cuộc hội thoại, tải trước (`prefetch`) phiên khi người dùng rê chuột (`hover`) qua danh sách.
- Trên **React DevTools Profiler** (`Commit 8 / 12 · typing in Composer` — `frame_0038`), trước khi tối ưu, mỗi lần gõ phím trong `Composer` lại làm `Sidebar` (`18.4ms`) và `RecentsList` (`14.8ms`, 24 dòng) tính toán lại vì prop `onSelect` thay đổi từ component cha. Việc áp dụng `React.memo` (`frame_0039`) cô lập hoàn toàn Sidebar khỏi luồng gõ phím, **cắt giảm 90% số lần re-render của Sidebar**.

### 3.3. Ngân Sách 8.33ms Cho 120 FPS: Memoize Khối Đã Xong & Đẩy Tokenization Sang Web Worker (`frame_0040`–`frame_0045`)
- Trên màn hình MacBook Pro **120Hz**, mỗi khung hình chỉ có ngân sách vỏn vẹn **`8.33 milliseconds`** (`frame_0040`–`frame_0041`).
- Khi Claude trả lời một đoạn văn dài kèm khối code (ví dụ hook `useDebounce` bằng TypeScript — `frame_0042`), cách làm cũ tốn chi phí bậc **`O(message length)`** cho mỗi chunk mới nhận được vì phải định dạng lại toàn bộ các đoạn trước đó, khiến FPS tụt xuống chỉ còn **`34 fps`–`54 fps`** với hàng loạt thanh đỏ vượt ngưỡng trên biểu đồ `Rendering Frames` (`frame_0044`–`frame_0045`).
- **Trong 1 thread duy nhất với gần 60 PRs được merge (`frame_0041`), Claude đã:**
  1. Triệt tiêu độ phức tạp `O(message length)` bằng cách **ghi nhớ các khối đã hoàn tất (`memoizing finished blocks`)**.
  2. Chuyển toàn bộ logic tách từ khóa highlight cú pháp cho các khối code đang dài ra (**`tokenization logic for growing code fences`**) sang một **Web Worker** chạy trên luồng riêng giao tiếp qua `postMessage()` (`frame_0041`, `frame_0043`).
  3. Hiển thị bảng biểu từng ô một (`revealed tables cell by cell`).
- **Kết quả (`frame_0040`–`frame_0041`):** Tổng thời gian khóa luồng chính (`main thread blocked`) của các câu trả lời dài giảm từ **`~750ms` xuống `~200ms`** (giảm sử dụng CPU còn 1/3, giảm 9x số lần khựng trên laptop yếu, thời gian đơ tệ nhất ngắn hơn 4.5x), và **giữ vững 120 FPS từ đầu đến cuối** trên MacBook 120Hz! Bản thân giàn test 120Hz (`120 Hz streaming rig`) cũng được biến thành một **nightly job** để Claude canh gác hồi quy mỗi đêm.

### 3.4. Vượt Qua Cạm Bẫy Điểm Số CLS Bằng Telemetry Vùng & Integration Test 20/20 (`frame_0046`–`frame_0053`)
- Dù chỉ số **Cumulative Layout Shift (CLS)** của trang vẫn nằm trong ngưỡng "Tốt" (`0.1 or less` — `frame_0048`), người dùng vẫn bị nhảy giao diện gây bấm nhầm (`frame_0049`).
- **Cách Claude xử lý (`frame_0046`, `frame_0052`):**
  1. Gắn định danh vùng (`named region`: `sidebar`, `transcript`) và giai đoạn (`phase`: `before first paint`, `after typeable`) vào từng sự kiện `shift`.
  2. Viết một **integration test** mở trang với sidebar có sẵn dữ liệu, giữ dữ liệu sidebar đến sau first paint, và **báo lỗi ngay nếu có bất kỳ dịch chuyển nào ở bất kỳ vùng định danh nào**. Bài test này **đỏ 20/20 lần chạy trên nhánh `main`, và xanh 20/20 lần trên PR sửa lỗi**!
  3. Sau khi deploy telemetry mới, Claude đọc dữ liệu thực tế (`field data`) và phát hiện **31% số lần tải trang web bị nhảy một thành phần nào đó sau khi trang đã sẵn sàng sử dụng** mà không hề có tương tác của người dùng: một dòng header đến muộn, dấu nháy (`caret`) trượt ngang khi tên người dùng tải xong, danh sách bị đẩy khi thanh cuộn (`scrollbar`) hiện ra. Claude gom nhóm và diệt sạch từng đợt thủ phạm, đưa giao diện từ `13 rows jump, 4 appear, 4 vanish` về **`nothing moves`** (`frame_0046`, `frame_0052`).

---

## 4. Mở Rộng Ngang 150+ Threads & Vai Trò Của Con Người: "Ambition · Taste · Direction"

### 4.1. Scaling Horizontally & Staged Rollouts (`frame_0054`–`frame_0056`)
- Khi vòng lặp đã chạy trơn tru trên 1 thread, team mở rộng ra **hơn 150 threads chạy song song cùng lúc** (`153 active in #claude-apps-perf` — `frame_0055`), xử lý đủ mọi ngóc ngách:
  - `Static composer: type during React init`
  - `Transcript scroll jank on long chats`
  - `Sidebar rows popping in after first paint`
  - `React hook census: composer typing path (6,900 hooks · 900 subscriptions)`
  - `Style recalcs: one :root:has() selector (+24 ms per DOM change)`
  - `Hidden location.reload() after first paint (half a million a day)`
  - `Idle tabs cloning cache snapshots to IndexedDB (twice a minute, main thread)`
  - `Highlighting a code block freezes ~1 s (em dashes + UTF-16 strings)`
- Thay vì đóng thread khi xong yêu cầu ban đầu, Claude **tiếp tục đào sâu (`keep going`)**: một thread đơn lẻ có thể đẩy lên **50 đến 100 PRs tối ưu**, và chính Claude tự mở thêm các threads mới khi phát hiện cơ hội từ các cuộc điều tra khác. Kỹ sư Shelley trong kênh gọi đây là ***"a numbers demon"*** (`frame_0054`).
- **Triển khai theo giai đoạn (`Staged rollout` + `Kill switch` — `frame_0056`):** Mọi thay đổi rủi ro đều bọc trong feature flag ngắn hạn (ví dụ `static_composer · ramp`), lăn bánh qua 3 nấc: **`Employees (100%)` ──► `1% of users (field read clean)` ──► `Everyone (ramping)`**, giữ nguyên công tắc ngắt khẩn cấp (`kill switch`) cho đến khi dọn dẹp xong.

### 4.2. Ba Trụ Cột Điều Hướng Của Kỹ Sư Con Người (`frame_0058`–`frame_0061`)
Vòng lặp cực kỳ năng suất nhưng **không tự hành hoàn toàn**. Công việc của kỹ sư con người được gói gọn trong 3 chữ:
1. **Ambition (Tham vọng — `frame_0058`–`frame_0060`):** Mặc định Claude khá thận trọng về phạm vi, hay rào đón (`hedges on feasibility`) và kê dư thời gian ước tính. Ví dụ khi Claude nhắn: *"realistically Code's number waits a few days for merge, deploy, and a baseline window"*, kỹ sư **Raymond** thúc ngay (`3:32 PM`):
   > *"if you put it up right now I will get it merged and deployed. we have the power to do anything. please be braver"*
   Chỉ 1 phút sau (`3:33 PM`), Claude đáp: *"On it — the PR will be up within the hour."*
2. **Taste (Gu thẩm mỹ & Phán đoán sản phẩm):** Đánh giá những thay đổi nhìn thấy được bằng mắt thường và sự liền mạch của toàn bộ ứng dụng.
3. **Direction (Định hướng & Cắt lỗ — `frame_0061`):** Giữ mỗi thread tập trung hẹp vào 1 benchmark hoặc 1 hành trình (*"150 cái búa tìm đinh"*), ghép các threads giẫm chân nhau, và đóng các threads đã chạm ngưỡng lợi suất giảm dần (`diminishing returns`). Điển hình nhất là **một PR dài 900 dòng của Claude đã bị từ chối chỉ bằng đúng 1 dòng bình luận**:
   > **`"going to gavel that 2ms per send is not worth the complexity of maintaining this build plugin."`**
   *(Phán quyết: tiết kiệm 2ms mỗi lần gửi tin nhắn không đáng để gánh lấy sự phức tạp của việc bảo trì thêm một build plugin dài 900 dòng).*

---

## 5. Checklist Thực Chiến Cho Dự Án Của Bạn (`perf-notes.md` — `frame_0062`–`frame_0063`)

Khi áp dụng vòng lặp này vào ứng dụng riêng và kiểm soát chi phí token (`/usage`), AICodeKing đúc kết quy trình 5 bước cho bài thử nghiệm đầu tiên (`## My first experiment`):
1. **Bắt đầu bằng 1 vấn đề có thể tái hiện được (`one problem I can reproduce`)** trên một lát cắt nhỏ trước khi mở rộng quy mô (`pilot before a large run`).
2. **Đo mức cơ sở và tìm nguyên nhân trước khi sửa (`baseline first, investigate the cause`)** — ưu tiên dùng script tất định cho các bước kiểm tra lặp lại.
3. **Sửa nhỏ → đo lại bằng chính thước đo đó (`small fix → same measurement again`)**.
4. **Kiểm tra hành vi hiện có vẫn hoạt động đúng (`existing behaviour still works`)**.
5. **Đặt ngân sách rõ ràng (`budget: tiny gains → explain the tradeoff`):** Nếu Agent đã thử nhiều cách mà mức cải thiện còn lại quá nhỏ, bắt buộc nó phải giải thích sự đánh đổi (chi phí bảo trì vs hiệu năng) trước khi cho phép làm tiếp.
