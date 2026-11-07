# 🚀 Claude Code 3.0: Cuộc Cách Mạng Phiên Làm Việc Lồng Nhau (Nested Sessions) & Điều Phối Đa Nhiệm Desktop

- **Video URL:** [https://www.youtube.com/watch?v=7Vu3uYL4ZP4](https://www.youtube.com/watch?v=7Vu3uYL4ZP4)
- **Video ID:** `7Vu3uYL4ZP4`
- **Watch Skill ID:** `d5ecd25ee6ce3d8c`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 03:19 (198.6s)
- **Speaker:** **Stack Delta**

---

## 1. Tóm Tắt Điều Hành (Executive Summary): Chuyển Dịch Từ "Một Trợ Lý Đơn Lẻ" Sang "Cả Một Đội Ngũ Song Song"

Vào ngày **14/10/2026**, Anthropic phát hành bản nâng cấp lớn cho ứng dụng **Claude Code Desktop**, được cộng đồng kỹ sư AI gọi là **"Claude Code 3.0"** (`frame_0002`, `frame_0003`).

Nếu như bản cập nhật tháng 04/2026 chỉ đơn thuần xây lại ứng dụng desktop với thanh sidebar liệt kê các phiên làm việc riêng lẻ (`frame_0008`–`0010`), thì bản nâng cấp tháng 10/2026 xoay quanh một tư tưởng cốt lõi: **chạy hàng loạt phiên song song cạnh nhau (`lots of sessions, side by side`)** với 3 trụ cột (`frame_0006`, `frame_0007`):
1. **Start (`⚡`):** Khởi tạo phiên nhanh hơn nhờ màn hình `Recent folders` & `Saved environments`.
2. **Split (`⑃`):** Chia nhỏ công việc qua nhiều phiên con độc lập nhờ `Git Worktree` & `Nested Sessions`.
3. **See (`👁`):** Nắm bắt tức thì từng phiên đang làm gì qua 3 nhóm trạng thái (`Needs input`, `Working`, `Completed`), `Live to-do`, `Recap card` và `Side rail`.

```
+----------------------------------------------------------------------------------------------------+
|               SỰ TIẾN HÓA CỦA KIẾN TRÚC PHIÊN LÀM VIỆC TRONG CLAUDE CODE                           |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ KIẾN TRÚC CŨ: BỨC TƯỜNG CHỮ TRÊN 1 LUỒNG (BEFORE: A WALL OF TEXT - frame_0026) ]                |
|    • Yêu cầu làm 3 task song song -> Tất cả log, tool calls, diff dồn chung vào 1 khung chat.      |
|    • Hậu quả: Ngữ cảnh bị pha loãng khủng khiếp, trôi thông tin, người dùng không thể kiểm soát.   |
|                                                                                                    |
|  [ KIẾN TRÚC MỚI: PHIÊN LỒNG NHAU (NESTED SESSIONS / CLAUDE CODE 3.0 - frame_0024) ]               |
|                                                                                                    |
|                       +----------------------------------+                                         |
|                       |   YOUR MAIN SESSION (PHIÊN CHÍNH)|                                         |
|                       |   - Chỉ nhận kết quả sạch/tóm tắt|                                         |
|                       |   - Giữ ngữ cảnh tinh gọn tối đa |                                         |
|                       +-----------------+----------------+                                         |
|                                         |                                                          |
|                 +-----------------------+-----------------------+                                  |
|                 |                       |                       |                                  |
|                 v                       v                       v                                  |
|      [ nested · task 1 ]     [ nested · task 2 ]     [ nested · task 3 ]                           |
|      - Fix product grid      - Retry failed webhook  - Migrate cart to                             |
|        layout shift            delivery                server actions                              |
|      - Worktree riêng biệt   - Worktree riêng biệt   - Worktree riêng biệt                         |
|      - Chạy độc lập ngầm     - Chạy độc lập ngầm     - Chạy độc lập ngầm                           |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 2. Giải Phẫu 5 Tính Năng Đột Phá Trong Bản Nâng Cấp

### 2.1. Màn Hình Khởi Tạo Phiên Mới (`New session screen` — `frame_0012`, `frame_0013`)
- Đưa ngay **3 thư mục làm việc gần nhất (`Recent folders, up front`)** lên mặt tiền (ví dụ: `storefront-web`, `payments-api`, `infra`).
- Chỉ cần **1 cú click** vào thư mục là phiên làm việc bắt đầu ngay (`session started`), loại bỏ hoàn toàn thao tác phải mở hộp thoại duyệt cây thư mục mỗi lần tạo phiên.

### 2.2. Môi Trường Lưu Sẵn (`Saved environments` — `frame_0014`–`0019`)
- Trước đây, mỗi khi mở một phiên mới cho dự án đa repo, người dùng phải chọn lại từng thư mục hoặc cấu hình kết nối từ đầu.
- Hộp thoại **`New environment`** (`frame_0015`, `frame_0016`) hỗ trợ **4 chế độ thực thi**:
  1. `Local` (Máy cục bộ)
  2. `Cloud` (Đám mây)
  3. `SSH` (Máy chủ SSH từ xa)
  4. `Remote Control` (Điều khiển từ xa)
- **Ví dụ thực tế (`frame_0018`, `0019`):** Gom 2 thư mục `frontend-web` (hoặc `storefront-web`) và `backend-api` (hoặc `payments-api`) lưu thành một môi trường tên là **`storefront`**. Từ đó về sau, mỗi khi mở phiên mới trong môi trường `storefront`, Claude Code tự động nạp sẵn cả hai codebase cùng lúc (`save a setup once, start sessions in it`).

### 2.3. Cách Ly Không Gian Bằng Git Worktree (`One worktree per folder` — `frame_0020`)
- Vấn đề nan giải nhất của việc chạy 6–10 phiên song song là **xung đột tệp tin (File Collisions)**: hai phiên cùng sửa một file trên cùng thư mục sẽ ghi đè làm hỏng code của nhau.
- Claude Code Desktop cho phép cấp cho mỗi thư mục một **Git Worktree riêng biệt (`a separate copy for each session`)**:
  - `your repo` $\rightarrow$ `copy 1` (`session A`) và `copy 2` (`session B`).
  - Hai phiên làm việc hoàn toàn cô lập trên hai bản sao vật lý độc lập, không bao giờ tranh giành cùng một file.

### 2.4. Ngôi Sao Sáng Nhất: Phiên Làm Việc Lồng Nhau (`Nested sessions` & `One-click follow-ups` — `frame_0021`–`0029`)
- **Cơ chế `Nested sessions` (`one per task`):** Khi bạn giao cho Claude một danh sách gồm nhiều đầu việc song song, phiên chính (`your main session`) sẽ tự động đẻ ra các phiên con riêng biệt (`nested · task 1`, `nested · task 2`, `nested · task 3`).
- **Tại sao điều này quan trọng (`why it matters` — `frame_0025`–`0027`)?**
  - **Trước đây (`before`):** Mọi công việc song song đều đổ dồn kết quả, log chạy test và tool calls vào chung một luồng chat duy nhất, tạo thành một **bức tường chữ (`a wall of text`)** gây ngập lụt context window.
  - **Hiện tại (`now`):** Phiên chính chỉ nhận về kết quả đã làm sạch (`clean results`), trong khi bạn vẫn có thể bấm vào bất kỳ phiên con nào để kiểm tra chi tiết từng bước thực thi.
- **Đề xuất việc phụ 1-Click (`while Claude works: one-click follow-ups` — `frame_0028`, `0029`):**
  - Trong lúc đang sửa code, nếu Claude phát hiện các vấn đề bên lề, nó sẽ hiển thị bảng **`3 suggested tasks`** kèm nút **`Start all`** hoặc nút **`Do it here` / `Start`** cho từng mục:
    1. `Checkout: label the form fields` (*Six inputs rely on placeholders. Give each a visible label.*)
    2. `Images: lazy-load below the fold` (*Product images load eagerly. Add `loading="lazy"`.*)
    3. `Cart: add width and height` (*Thumbnails have no size, so the cart shifts.*)
  - Bấm **`Start`** chỉ 1 click: tác vụ phụ đó lập tức tách thành một phiên mới trong mục `Working` ở sidebar trong khi phiên chính vẫn tiếp tục chạy không hề bị gián đoạn!

### 2.5. Bảng Điều Hướng 3 Nhóm Trạng Thái & Giao Diện Giám Sát Chi Tiết (`frame_0030`–`0035`)
- **Sidebar chia thành 3 nhóm (`3 groups`):**
  1. `Needs input` (biểu tượng cảnh báo vàng `⚠️`): Các phiên đang tạm dừng chờ người dùng quyết định.
  2. `Working` (vòng xoay đỏ `◌`): Các phiên đang tích cực chạy.
  3. `Completed`: Các phiên đã hoàn tất (ví dụ: `Upgrade the Stripe SDK`, `Add dark mode color tokens`).
- **Bên trong mỗi phiên làm việc có 3 thành phần trực quan (`frame_0032`–`0035`):**
  1. **`Live to-do`:** Danh sách các bước tiếp theo theo thời gian thực. Ví dụ trong phiên `Fix product grid layout shift`:
     - `✓ Reproduce the shift, run baseline tests`
     - `✓ Add image dimensions to ProductCard`
     - `✓ Reserve space with an aspect-ratio fallback`
     - `✓ Check the product grid in the preview`
     - `✓ Add a layout shift test, final run`
  2. **`Recap card`:** Thẻ tóm tắt nổi bật khi Claude dừng lại hỏi ý kiến: *"RECAP: Images now have a width and height, and all 14 tests pass. Open a PR for this?"* kèm 2 nút hành động `[Open PR]` và `[Not now]`.
  3. **`Side rail` (Thanh thông tin bên phải):** Hiển thị trực quan:
     - **Environment:** `storefront` (`storefront-web`, `payments-api`, `design-system`)
     - **Context usage:** `12%` (thanh tiến trình màu cam)
     - **Pending changes (3 files):** `~ ProductCard.tsx`, `~ ProductGrid.module.css`, `+ layout-shift.test.ts`.

---

## 3. Cái Bẫy Hạn Mức (`The Catch: Usage Limits`) & Cơ Chế Nén Khi Nghỉ (`Idle Compaction` — `frame_0036`–`0048`)

### 3.1. Cơn Sốt Đốt Hạn Mức & Hết Hạn Prompt Cache Sau 1 Giờ (`frame_0037`–`0042`)
- **Nhân bản mức tiêu thụ (`frame_0038`, `0039`):** Chạy `1 session` tốn ít hạn mức, nhưng khi bật **10 `parallel sessions` + `nested sessions` cùng lúc**, tổng lượng token tiêu thụ sẽ nhanh chóng vượt vạch đỏ **`plan limit`**.
- **Bẫy hết hạn Prompt Cache (`frame_0040`–`0042`):**
  - Nếu bạn để một phiên lớn **nghỉ khoảng 1 giờ (`idle for ~1 hour`)** kể từ tin nhắn cuối (`last message`), **Prompt Cache sẽ hết hạn (`cache expires`)**.
  - Khi bạn quay lại gõ tiếp, hệ thống buộc phải **xử lý lại toàn bộ cuộc hội thoại từ đầu (`reprocess the whole conversation = a big chunk of your limit`)**, ngốn một mảng lớn hạn mức gói cước.

### 3.2. Giải Pháp Của Anthropic: `Idle Compaction` & Sự Đánh Đổi (`frame_0043`–`0048`)
- **Cơ chế hoạt động (`frame_0044`–`0046`):** Ngay trước thời điểm mốc `~1 hr · cache expires`, khi phiên đang còn trong cache ấm, Claude tự động chạy bước **`compact`** để tóm tắt cuộc hội thoại dài thành một bản **`summary`** gọn nhẹ. Khi bạn quay lại làm việc (`when you come back`), chi phí nạp lại ngữ cảnh **rẻ hơn rất nhiều (`way cheaper`)**.
- **Sự đánh đổi (`the trade-off` — `frame_0047`, `0048`):**
  - Vì bạn tiếp tục công việc từ một bản tóm tắt (`you resume from a summary`), **các chi tiết nhỏ có thể bị thất thoát (`small details can get lost`)**.
  - Nếu muốn giữ nguyên vẹn 100% lịch sử hội thoại và không dùng tính năng này, bạn có thể tắt trong `settings.json`:
    ```json
    // settings.json
    { "idleCompaction": false }
    ```
    (Mặc định cờ `"idleCompaction": true` đang được bật — `idle compaction on`).

---

## 4. Bảng Đối Chiếu Dữ Liệu Trực Quan Từ 53 Frames (`Watch Skill ID: d5ecd25ee6ce3d8c`)

| Frame & Timestamp | Nội dung hiển thị trên màn hình | Ý nghĩa kỹ thuật / Bài học thực chiến |
|---|---|---|
| `frame_0000`–`0003` (`00:02`–`00:15`) | Từ `one assistant` sang 6 phiên chạy đồng thời (`a whole team · at the same time`) trong bản cập nhật `Oct 14, 2026` (`Claude Code 3.0`) | Chuyển dịch mô hình làm việc từ đơn luồng sang điều phối cả một đội ngũ tác nhân song song. |
| `frame_0005`–`0007` (`00:22`–`00:32`) | Ba trụ cột của `lots of sessions, side by side`: **start** (`sessions faster`), **split** (`work across a few`), **see** (`what each one is doing`) | Khung thiết kế UX cốt lõi cho mọi công cụ Agentic Development Environment (ADE). |
| `frame_0012`–`0013` (`00:51`–`00:56`) | Màn hình `New session` hiển thị 3 `Recent folders` (`storefront-web`, `payments-api`, `infra`) | Giảm ma sát khởi tạo phiên mới xuống chỉ còn 1 cú click chuột. |
| `frame_0015`–`0019` (`01:01`–`01:11`) | Hộp thoại `New environment` (`Local`, `Cloud`, `SSH`, `Remote Control`) gom `frontend-web` + `backend-api` thành môi trường `storefront` | Lưu cấu hình đa repo và máy chủ SSH một lần duy nhất để tái sử dụng cho mọi phiên sau. |
| `frame_0020` (`01:18`) | Sơ đồ `one worktree per folder`: `your repo` tách thành `copy 1` (`session A`) và `copy 2` (`session B`) | Tự động hóa Git Worktree để triệt tiêu hoàn toàn xung đột ghi đè file giữa các phiên song song. |
| `frame_0022`–`0027` (`01:26`–`01:48`) | Sơ đồ `nested sessions` (`your main session` đẻ ra `nested · task 1..3`) đối chiếu với `before: a wall of text` | Giữ ngữ cảnh phiên chính sạch tuyệt đối, chỉ nhận báo cáo kết quả cuối cùng từ các phiên con. |
| `frame_0028`–`0029` (`01:50`–`01:58`) | Giao diện `one-click follow-ups` (`3 suggested tasks`: `Checkout`, `Images`, `Cart`) với nút `Start` / `Start all` | Biến các lỗi phụ phát hiện trong lúc code thành phiên độc lập chỉ bằng 1 click mà không làm đứt mạch phiên chính. |
| `frame_0031`–`0035` (`02:05`–`02:15`) | 3 nhóm trạng thái (`Needs input`, `Working`, `Completed`) + `Live to-do` 5 bước + `RECAP` card + `Side rail` (`Context 12%`, `Pending changes 3 files`) | Chuẩn hóa bảng điều khiển giám sát giúp con người nắm bắt trạng thái của hàng chục phiên chỉ trong vài giây. |
| `frame_0037`–`0048` (`02:20`–`02:49`) | Biểu đồ vượt trần `plan limit` khi chạy 10 phiên, hết hạn Prompt Cache sau `~1 hour`, cơ chế `idle compaction` và cấu hình `{ "idleCompaction": true }` trong `settings.json` | Hiểu rõ bài toán kinh tế Prompt Cache TTL (~1 giờ) và cách bật/tắt `idleCompaction` tùy theo yêu cầu độ chính xác chi tiết. |

---

## 5. Liên Kết Chéo Trong Vault

- [The S TIER Claude Code Mods — Ray Amjad](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/s_tier_claude_code_mods_ray_amjad.md) — Bản độ `cache-clock` đếm ngược TTL Prompt Cache (60 phút cho main session / 5 phút cho subagent) và tự động viết `handoff.md`.
- [The Best Git Worktree Tool (Worktrunk)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/the_best_git_worktree_tool_worktrunk_better_stack.md) — Công cụ Rust CLI `wt` quản lý vòng đời Git Worktree và copy-on-write dependencies cho Claude Code.
- [Bạn Đang Dùng AI Agents Sai Cách — Theo t3.gg](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/youre_using_ai_agents_wrong_theo_t3.md) — Quản lý đa luồng theo mô hình Inbox Zero (`Settle` & `Snooze`) trên máy chủ Linux từ xa.
