# Bạn Đang Dùng AI Agents Sai Cách | Theo - t3.gg

- **Video URL:** [https://www.youtube.com/watch?v=q1D90-uGvBg](https://www.youtube.com/watch?v=q1D90-uGvBg)
- **Video ID:** `q1D90-uGvBg`
- **Watch Skill ID:** `1598db16b3cbf3cf`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 49:22
- **Speaker:** Theo Browne (`t3.gg`) — Founder & CEO Ping.gg, tác giả **T3 Code** (`t3.codes`)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

```
┌──────────────────────────────────────────────────────────────────────┐
│          WORKFLOW CỦA THEO: MERGE 50+ PRs/TUẦN KHI ĐI DU LỊCH      │
│                                                                      │
│  Vấn đề: CEO part-time dev + đang bó bột tay trái ("down a hand")    │
│  Giải pháp: AI workflow tối ưu để maximize throughput & giữ động lực │
│                                                                      │
│  STEP 1: New thread + Wispr Flow voice + chạy trên REMOTE machine    │
│          (Linux box `bb-1`: Ryzen AI MAX+ 395, 32 threads, 62GB RAM) │
│                                                                      │
│  STEP 2: Ignore thread until done                                    │
│          (đừng ngồi nhìn — không productive)                        │
│                                                                      │
│  STEP 3: Multiple threads cùng lúc                                   │
│          ├── Burn tokens để giảm uncertainty                        │
│          ├── Dùng cheap models (GLM 5.3-Flash) cho triage task       │
│          └── 2 threads với models khác nhau để compare              │
│                                                                      │
│  STEP 4: Manage threads như inbox                                    │
│          ├── Settle = done (tự biến mất khi PR merge)               │
│          ├── Snooze = pending review later                          │
│          └── Link PR → thread (auto-settle on merge)                │
│                                                                      │
│  STEP 5: Test trước khi merge (không ma sát)                         │
│          ├── Web: `--share-dev` qua Tailscale + pairing token        │
│          ├── iOS: `sqim` (`milq-ai/tap/sqim`) cài từ remote Mac      │
│          └── Desktop: CI label `preview:mac` → rolling prerelease    │
│                                                                      │
│  STEP 6: Safety nets (không phải guardrails)                        │
│          └── Nightly builds mỗi 3 tiếng (`0.0.35-nightly`)          │
└──────────────────────────────────────────────────────────────────────┘
```

Theo Browne (tác giả **T3 Code**) livestream quy trình làm việc thực tế trong khi đang đeo nẹp cổ tay trái (*"down a hand"*), dùng **Wispr Flow** đọc giọng nói để điều phối hàng loạt AI agents trên máy chủ Linux từ xa (`bb-1`), kiểm toán PR của cộng đồng, báo lỗi ngược lên repo `anthropics/claude-code`, và merge code trực tiếp trên sóng. Video không phải bài hướng dẫn lý thuyết — đây là **cửa sổ nhìn thẳng vào cách cha đẻ T3 Code đạt kỷ lục 52 PRs/tuần ngay cả khi đang đi du lịch tại DEF CON**.

---

## 2. Bối Cảnh: CEO Part-Time Developer & Biểu Đồ Đóng Góp Thực Tế

### 2.1 Thống Kê Đóng Góp Trên Repo T3 Code (`frame_0006`, `frame_0007`, `frame_0053`)
Dù là CEO phải điều hành công ty, quay YouTube và đi sự kiện, biểu đồ **T3 Code contribution rate** của Theo Browne từ **29/05 đến 26/08/2026** ghi nhận:
- **223 authored commits** (tất cả đều là squash-merged PRs vào `main`)
- **39 active days** | **184 commits trong 30 ngày gần nhất (+182% vs prior 30d)**
- **Chuỗi tăng trưởng PR theo tuần:**
  - Tháng 5 – đầu tháng 7: chỉ `1 → 3` PRs/tuần (bận làm video & vận hành)
  - Giữa tháng 7 – tháng 8: vọt lên `23` → `37` → `41` → **đỉnh `52 PRs/tuần` (tuần 07/08 khi đang đi DEF CON!)** → `16` → `47 PRs` (chỉ trong 6 ngày của tuần hiện tại).

### 2.2 Mục Tiêu Video
> *"I finally found flows that allow me to be as productive as I want to coding while also having to run the rest of my life."*

Mục tiêu không phải quảng cáo T3 Code, mà là chỉ ra cách **nhận diện từng điểm ma sát (friction)** ngăn cản bạn bấm nút Merge và xây dựng công cụ/quy trình để triệt tiêu ma sát đó.

---

## 3. Nguyên Tắc Cốt Lõi: Đốt Token Để Giữ Động Lực (Burn Tokens to Prevent Motivation Loss)

### 3.1 Triết Lý Chủ Đạo
```
Thay vì tiết kiệm tokens → Burn tokens để:
1. Giảm khả năng xảy ra kết quả tệ (bad outcome)
2. Giảm uncertainty trong đầu
3. Bảo vệ nguồn tài nguyên quý nhất: ĐỘNG LỰC (Motivation)
4. Cho phép trust the model và chuyển sang việc khác ngay
```

> *"You need to do whatever you can to avoid those demotivating moments. One of the best things you can do is reduce the likelihood that the bad outcome happens. One of the easiest ways that you can prevent these failures is burning more tokens."*

### 3.2 Vòng Xoáy Mất Động Lực (ADHD / Demotivation Cycle)
Nếu cố tiết kiệm token → model làm ẩu → bạn vào test thử thấy code hỏng → chán nản → mở Twitter / ra mở tủ lạnh → **mất trắng cả buổi chiều làm việc**:

```
Bad outcome cycle:
Effort → Test → Broken → Demotivated → Twitter/fridge → Lost context
                                          ↑
                                          |
                            Phòng ngừa: Đốt thêm token ngay từ đầu
                            (dùng High/Extra High effort, chạy 2 models song song)
                            để tăng xác suất thành công ngay lần đầu!
```

---

## 4. Giải Phẫu Hạ Tầng & Quy Trình 6 Bước Thực Chiến

### 4.1 Step 1: New Thread + Giọng Nói (Wispr Flow) + Máy Chủ Linux Từ Xa (`bb-1`)

Do đang chấn thương cổ tay trái (`frame_0000`, `frame_0018`), Theo giữ phím tắt để đọc prompt dài bằng **Wispr Flow** rồi gửi thẳng vào `New worktree` trên máy chủ từ xa **`bb-1`**.

**Cấu hình thực tế của dàn máy từ xa (`frame_0002`, `frame_0027` — màn hình `cmux` + `btop`):**
- **Máy chủ chính `bb-1`:** CPU **AMD Ryzen AI MAX+ 395 w/ Radeon 8060S** (16 nhân / **32 luồng** xung nhịp ~3.0–4.3 GHz), **61.8 GiB RAM** (đang dùng ~28.3 GiB), ổ cứng **3.44 TiB NVMe**.
- **Mức tải thực tế khi gánh hàng chục Agents:** Dù đang chạy đồng thời hàng loạt tiến trình `claude --output-format stream-json`, `codex`, `cli-proxy-api`, `tailscaled`, và `cloudflared`, CPU của `bb-1` chỉ tốn **4%–5%** (`Load avg: 2.03 1.44 0.95`).
- **Dàn 6 máy (Fleet) hiển thị trên sidebar `cmux`:** `ClawMini`, `zbook`, `ComfyPC`, `ssh bb-1`, `ProMini`, `t3code-1` — tất cả đều được cài sẵn model giá rẻ `GLM 5.3 Flash` trong Codex (`Installed GLM 5.3 Flash in Codex across all 6 machines`).

**Tại sao phải dùng Linux thay vì macOS để chạy song song?**
> *"macOS has a ton of problems that make it really hard to reliably parallelize your work. Generally speaking, I treat my Macs as one or two tasks at a time, and I treat my Linux boxes as effectively unlimited tasks at a time."* (Một chiếc PC Linux 16 luồng giá ~\$600 đã dư sức gánh hàng chục agents mà không làm nóng hay giật chiếc MacBook bạn đang dùng).

---

### 4.2 Step 2: Gửi Xong Phải Quên Ngay (Ignore Thread Until Done)

Sai lầm lớn nhất của lập trình viên khi dùng AI là **ngồi nhìn chằm chằm vào dòng chữ đang chạy**:
- Ngồi nhìn không làm thread chạy nhanh hơn dù chỉ 1 mili-giây.
- 99% thời gian bạn không cần can thiệp giữa chừng.
- **Quy tắc vàng:** Bấm Enter gửi nhiệm vụ → lập tức chuyển sang thread khác hoặc mở issue tiếp theo. Chỉ quay lại khi T3 Code báo hoàn tất.

---

### 4.3 Step 3: Chạy Đa Luồng & Chiến Lược Chọn Model (`frame_0017`, `frame_0020`)

Trong bảng chọn model của T3 Code, Theo gắn phím tắt **Favorites**:
- `⌘1` **Claude Fable 5** (`High · 1M` hoặc `Extra High · 1M` context)
- `⌘2` **Claude Opus 5**
- `⌘3` **GPT-5.6 Sol** (trong Codex harness)
- `⌘4` **cx-Alpha**
- Ngoài ra còn có **GLM 5.3 Flash** và **Luna** cho các tác vụ phân loại nhanh.

| Model | Vai trò trong quy trình của Theo | Lý do lựa chọn |
|-------|----------------------------------|----------------|
| **Claude Fable 5** (`⌘1`) | Sửa bug phức tạp, kiểm toán PR, đóng vai Theo trả lời trên GitHub, chạy `$babysit` | Khả năng suy luận sâu với 1M context, văn phong viết nhận xét PR cực chuẩn |
| **Claude Opus 5** (`⌘2`) | Kiến trúc khó, quyết định thiết kế lớn | Năng lực tư duy tổng thể mạnh |
| **GPT-5.6 Sol** (`⌘3`) | Điều tra lỗi hạ tầng (`Diagnose Codex 0.150 Breakage`), viết GitHub Actions CI | Cực kỳ đáng tin cậy khi mò mẫm hệ thống, tái hiện lỗi trong môi trường cô lập |
| **GLM 5.3 Flash** | PR Triage (rà soát hàng loạt PRs xem cái nào đáng đọc) | Rẻ như cho, tốc độ cực nhanh, đủ thông minh để lọc sơ bộ |
| **Luna** | Chạy song song cùng `GLM 5.3 Flash` trên cùng 1 câu hỏi triage | Đối chiếu chéo kết quả (A/B compare) với chi phí gần như bằng 0 |

---

### 4.4 Step 4: Quản Lý Thread Như Hộp Thư Đến (Inbox Zero: Settle & Snooze)

Theo tự thiết kế cơ chế quản lý trạng thái luồng trong T3 Code để thanh bên (sidebar) không bao giờ biến thành bãi rác:
1. **Active:** Những việc đang cần xử lý.
2. **Settled (Đã giải quyết):** Biến mất khỏi sidebar. Đặc biệt, khi một Thread được **liên kết với PR (Linked PR)**, ngay khoảnh khắc PR đó được bấm **Merge** trên GitHub, Thread trong T3 Code sẽ **tự động chuyển sang Settled và biến mất khỏi sidebar** (`frame_0052`).
3. **Snoozed (Tạm ẩn):** Nếu chưa muốn xử lý ngay (chờ mai, chờ 3 tiếng, chờ tuần sau), bấm Snooze để giữ sidebar sạch tuyệt đối, tránh phân tâm.

---

### 4.5 Step 5: Vòng Lặp Tự Động Với `$babysit` Skill (`frame_0029`–`0035`, `0038`)

Khi tạo PR xong, Theo không tự ngồi đọc từng lời phàn nàn của các bot review tự động. Ông gọi kỹ năng **`$babysit`**:
- **Nội dung lệnh `$babysit`:** Yêu cầu Agent canh chừng PR, mỗi khi các bot tự động (`macroscopeapp[bot]`, `cursor[bot]`) để lại nhận xét, Agent tự đánh giá xem nhận xét đó có hợp lý không. Nếu đúng → tự sửa code → tự `git push` → tiếp tục canh cho đến khi mọi bot đều báo xanh (Approved) và CI pass hết.
- **Ví dụ thực tế trên sóng (`PR #8243`):** Bot `macroscopeapp[bot]` và `cursor[bot]` bắt các lỗi tiềm ẩn trong workflow shell script → Agent tự sửa và đẩy các commit `fix(ci): harden preview prerelease workflow` & `fix(ci): handle prerelease creation race` → Theo chỉ việc vào bấm nút **Squash and merge**!

---

## 5. Giải Phẫu Các Ca Thực Chiến Trực Tiếp Trong Video (Case Studies)

### 5.1 Case Study 1: Sửa Lỗi Gợi Ý Skill & Slash Command (`Issue #7671` — `frame_0016`..`0022`)
- **Bối cảnh:** Người dùng `Brechard` mở Issue `#7671` (*Composer offers skills and slash commands that cannot run*) chỉ ra 3 lỗi:
  1. Gõ `$` vẫn gợi ý cả các skills có cờ `disable-model-invocation: true` (vốn bị ẩn khỏi model).
  2. Gõ `$` bỏ qua cấu hình `skillOverrides` trong `settings.json`.
  3. Gõ `/` giữa câu vẫn bật menu gợi ý slash commands dù CLI chỉ thực thi `/` ở đầu tin nhắn.
- **Cách Theo viết Prompt (Giải phẫu từng câu):**
  - *"This is an issue that's been affecting a number of users as of recent..."* → Đặt mức độ nghiêm trọng cao để model không làm qua loa.
  - *"I want to make a fix that is both simple in its implementation and not confusing to users."* → Chốt ranh giới ưu tiên (No-compromise line).
  - *"In my opinion, you should be able to..."* → Cụm từ `"In my opinion"` mở đường cho phép model **phản biện (push back)** nếu giả định của Theo sai.
  - *"I'm not sure about the characteristics in Claude Code as to how these behaviors are triggered..."* → Thừa nhận vùng mù kiến thức để model tự đi kiểm chứng thay vì tin mù quáng vào người dùng.
  - *"If you see a simple fix, stop and let me know ASAP."* → Cho phép dừng khám phá sớm.
- **Tuyệt chiêu "Explain Like I'm 5" (ELI5):** Khi model trả về bản kế hoạch quá dài và rối rắm, thay vì cố đọc trong mệt mỏi rồi bỏ cuộc, Theo nhắn: *"I really like your description of the problems here. I find your solution hard to parse... Can you give me a simpler, easy to digest, ELI5 description of how you plan to fix these problems?"* (`frame_0025`).

### 5.2 Case Study 2: Kiểm Toán PR Cộng Đồng & Trả Lời Thay Mặt Theo (`PR #8327` — `frame_0023`..`0039`)
- **Bối cảnh:** Cộng tác viên `mackinleysmith` gửi PR `#8327` (`feat(usage): show subscription limits` — hiển thị hạn mức tiêu thụ kiểu `Codex 18 sessions $98.83` và `Claude Code 112 sessions $1,051.91`).
- **Quy trình kiểm toán bằng Claude Fable 5:**
  - Theo yêu cầu Agent đối chiếu PR này với 5 công cụ theo dõi hạn mức nổi tiếng trong hệ sinh thái (`CodexBar`, `ccusage`, `Claude-Code-Usage-Monitor`, `CCSeva`, và `codex TUI`).
  - **4 lỗi nghiêm trọng (Must-fix) mà Agent phát hiện (`frame_0024`):**
    1. **Nguy cơ dính bão `429 Rate Limit` từ Anthropic:** PR đang gọi `/api/oauth/usage` mỗi 60s mà không có `Retry-After` backoff (trong khi `CCSeva` phải đặt sàn tối thiểu `110s` và Theo yêu cầu sàn **`180s`**), cộng thêm `FAILURE_CACHE_TTL_MS = 5_000` khiến mỗi khi lỗi nó lại spawn lại tiến trình `claude` CLI sau mỗi 5 giây!
    2. Bỏ sót cửa sổ hạn mức `seven_day_fable` mới của Anthropic.
    3. Tự bịa ra cửa sổ 5h giả tạo (`Pro ∞`) khi API trả về `null`.
    4. Ghi sai tên gói cước trong tài liệu (`Max 5x/20x`).
  - **Pattern "Draft Before Post":** Theo bảo Agent soạn bản nháp nhận xét trước để ông duyệt → chỉnh sửa đôi chút → ra lệnh *"leave the comment on my behalf"*. Kết quả trên GitHub xuất hiện bình luận chi tiết mở đầu bằng dòng chữ: *"(Claude Fable 5 responding on behalf of Theo)"* (`frame_0037`).

### 5.3 Case Study 3: Truy Tìm Bug Thượng Nguồn Của Claude Code (`anthropics/claude-code#67113` — `frame_0040`, `0041`)
- Khi điều tra việc gọi chồng nhiều skills (`$skill1 $skill2`) chỉ nạp được skill đầu tiên trên đường dẫn SDK / `--print`, Agent của Theo tự lùng sục repo `anthropics/claude-code`, tìm ra đúng Issue `#67113` (đã được `bcherny` gắn nhãn `reproduced` và `area:core`), đồng thời đại diện Theo đăng luôn bình luận kỹ thuật xác nhận cách tái hiện lỗi lên thẳng issue của Anthropic!

---

## 6. Hạ Tầng Kiểm Thử Không Ma Sát (Zero-Friction Testing Infrastructure)

Để dám merge 50+ PRs/tuần khi code đang nằm trên máy chủ Linux từ xa `bb-1`, Theo đã tự xây dựng 3 đường ống kiểm thử tức thì:

### 6.1 Kiểm Thử Web App Từ Xa: `--share-dev` Qua Tailscale (`frame_0043`–`0047`, `0050`, `0051`, `0054`)
- **Bài toán:** Cần kiểm thử tính năng kéo-thả mọi loại file (`PR #8092` — `Generic File Upload Merge Readiness`) đang chạy trên máy `bb-1`.
- **Giải pháp:** Theo chỉ cần nhắn cho Agent trong T3 Code: *"use the `--share-dev` command with tailscale so that I can try it out remotely. Respond with a URL that has the pairing code."*
- **Kết quả trong 23 giây (`frame_0046`):** Agent tự khởi chạy server dev kết hợp cờ `--tailscale`, trả về đường dẫn đã gắn sẵn mã ghép đôi:
  `https://bb-1.porpoise-micro.ts.net:6392/pair#token=83LYU5R6NP97`
- Theo bấm vào link ngay trên MacBook của mình, kéo thả thử file `index.html` (được lưu vào `.t3/userdata/attachments/...`) và dán ảnh chụp màn hình PNG để xác nhận cả luồng tài liệu lẫn luồng ảnh đều hoạt động hoàn hảo trước khi merge!

### 6.2 Kiểm Thử iOS Từ Xa: Công Cụ `sqim` (`frame_0048`, `0049`)
- **Bài toán:** Làm sao cài bản build iOS từ một máy Mac đặt ở xa lên chiếc iPhone đang cầm trên tay khi đi du lịch mà không cần nối cáp hay cấu hình VPN/Tailscale phức tạp?
- **Giải pháp:** Theo dùng **`sqim`** (`brew install milq-ai/tap/sqim` — *"xcode to iphone on any network, works with claude code, no vpn or tailscale needed"*). Agent build app trên Mac từ xa → `sqim` tạo một đường link web → mở link trên iPhone để cài trực tiếp và test ngay trên đường đi.

### 6.3 Kiểm Thử Desktop App: Nhãn `preview:mac` & Rolling Prerelease Không Cần Đăng Nhập (`frame_0003`, `0029`–`0035`)
- **Bước 1 (`PR #8182`):** Gắn nhãn `preview:mac` vào bất kỳ PR nào → GitHub Actions tự động đóng gói file cài đặt `.dmg` cho macOS Apple Silicon và bình luận link tải ngay trong PR.
- **Bước 2 — Xóa bỏ ma sát cuối cùng (`PR #8243`):** Vì GitHub bắt phải đăng nhập tài khoản mới cho tải workflow artifact (rất phiền khi muốn tải nhanh trên một chiếc máy Mac sạch dùng để test), Theo ra lệnh cho Agent tạo tiếp `PR #8243` (`feat(ci): download macOS preview DMGs without signing in`): tự động đẩy bản DMG vừa build lên một nhánh **rolling GitHub prerelease (`desktop-preview`)** để bất kỳ máy test nào cũng có thể tải trực tiếp hoặc `curl` về mà không cần login!

---

## 7. Lưới An Toàn Thay Vì Rào Chắn (Safety Nets > Guardrails — `frame_0055`)

```
Guardrails (Rào chắn) = Chặn trước khi cho ship → gây ma sát, làm chậm toàn bộ đội ngũ.
Safety Nets (Lưới an toàn) = Cho phép ship nhanh nhưng bắt lỗi ngay lập tức trước khi ảnh hưởng diện rộng.
```

- Trên trang GitHub Releases của T3 Code (`frame_0055`), các bản **`T3 Code Nightly`** (ví dụ: `0.0.35-nightly.20260827.1200 (33b650a5b3b2)`) được tự động phát hành **mỗi 3 tiếng một lần**.
- Trong tổng số **hơn 200,000 người dùng** T3 Code, có **vài nghìn người dùng đam mê** tự nguyện cài bản Nightly.
- Bất kỳ lỗi hồi quy (regression) nào lọt qua bước review đều bị phát hiện và báo cáo ngay trong vòng vài giờ trên kênh Nightly trước khi kịp chạm tới 99% người dùng bản Stable. Nhờ tấm lưới an toàn này, Theo hoàn toàn tự tin bấm nút Merge liên tục mỗi ngày.

---

## 8. Bảng Đối Chiếu Dữ Liệu Trực Quan Từ 59 Frames (`Watch Skill ID: 1598db16b3cbf3cf`)

| Frame & Timestamp | Nội dung hiển thị trên màn hình | Ý nghĩa kỹ thuật / Bài học thực chiến |
|---|---|---|
| `frame_0000`, `0001`, `0018` (`00:00`–`06:15`) | Theo đeo nẹp cổ tay trái, dùng **Wispr Flow** đọc prompt dài vào T3 Code | Giọng nói + AI Agent giúp duy trì tốc độ 50+ PRs/tuần ngay cả khi chỉ gõ được bằng 1 tay. |
| `frame_0002`, `0027` (`00:34`, `14:09`) | Cửa sổ `cmux` + `btop` trên máy chủ `bb-1` (**AMD Ryzen AI MAX+ 395**, 32 threads, 61.8 GiB RAM, 3.44 TiB NVMe) | Chạy hàng chục tiến trình `claude` & `codex` chỉ tốn **4%–5% CPU** trên Linux; sidebar quản lý dàn 6 máy (`ClawMini`, `zbook`, `ComfyPC`, `bb-1`, `ProMini`, `t3code-1`). |
| `frame_0003` (`00:35`) | PR `#8182`: `feat(desktop): build macOS previews from a PR label` (`preview:mac`) | Tự động hóa việc đóng gói `.dmg` từ nhãn PR bằng `GPT-5.6 Sol` trên Codex harness. |
| `frame_0006`, `0007`, `0053` (`00:50`, `41:36`) | Biểu đồ **T3 Code contribution rate** (29/05 – 26/08/2026): **223 commits**, **+182%**, đỉnh **52 PRs/tuần** | Minh chứng định lượng cho sự bùng nổ năng suất khi chuyển sang điều phối đa luồng trên máy từ xa. |
| `frame_0016`–`0022` (`04:11`–`09:05`) | Issue `#7671` của `Brechard` & bảng chọn model Favorites (`⌘1 Claude Fable 5`, `⌘2 Claude Opus 5`, `⌘3 GPT-5.6 Sol`, `⌘4 cx-Alpha`) | Phân tích cấu trúc prompt thực chiến: đặt mức độ ưu tiên, cho phép model phản biện, và dùng ELI5 khi giải pháp khó hiểu. |
| `frame_0023`–`0026`, `0034`–`0039` (`10:15`–`32:10`) | Kiểm toán PR `#8327` (`feat(usage): show subscription limits`) bằng `Claude Fable 5` | So sánh với `CodexBar`, `ccusage`, `CCSeva`; bắt lỗi thiếu sàn 180s chống `429 Rate Limit` & TTL 5s làm nổ tiến trình CLI; bình luận thay mặt Theo. |
| `frame_0029`–`0033`, `0052` (`16:10`–`41:10`) | PR `#8243` (`download macOS preview DMGs without signing in`) + kỹ năng `$babysit` | Vòng lặp tự động sửa code theo nhận xét của `macroscopeapp[bot]` & `cursor[bot]`; tự động **Settle** ẩn thread ngay khi PR được merge. |
| `frame_0040`, `0041` (`34:08`–`35:43`) | Issue `anthropics/claude-code#67113` (`Stacked slash-skill invocations load only the first skill`) | Agent tự truy vết bug lên tận repo chính chủ của Anthropic và viết báo cáo kỹ thuật xác nhận lỗi. |
| `frame_0043`–`0047`, `0050`–`0054` (`38:28`–`43:00`) | Lệnh `--share-dev` qua Tailscale (`https://bb-1.porpoise-micro.ts.net:6392/pair#token=...`) test PR `#8092` | Kiểm thử trực tiếp web app chạy trên server Linux từ xa ngay từ trình duyệt laptop trong 23 giây. |
| `frame_0048`, `0049` (`39:54`–`40:24`) | Repo & trang chủ **`sqim`** (`brew install milq-ai/tap/sqim`) | Giải pháp đẩy bản build Xcode từ máy Mac ở xa sang iPhone qua mạng bất kỳ mà không cần VPN hay Tailscale. |
| `frame_0055` (`44:51`) | GitHub Releases: `T3 Code Nightly 0.0.35-nightly.20260827.1200` | Triết lý **Safety Nets > Guardrails**: phát hành bản Nightly mỗi 3 giờ cho hàng nghìn người dùng thử nghiệm trước. |

---

## 9. Tổng Kết Bài Học Cốt Lõi (Key Takeaways)

| Nguyên tắc | Hành động cụ thể cần áp dụng ngay |
|------------|-----------------------------------|
| **1. Tách rời máy chạy Agent khỏi máy làm việc** | Đẩy toàn bộ các phiên chạy Agent nặng sang một máy chủ Linux (`bb-1`, mini PC Ryzen) kết nối qua SSH/Tailscale để máy cá nhân luôn mượt và chạy được hàng chục luồng song song. |
| **2. Đốt Token để bảo vệ Động lực** | Đừng tiếc token cho các mức suy luận cao (`High`/`Extra High`) hoặc chạy 2 models song song; một lần code lỗi làm bạn chán nản mở mạng xã hội sẽ đắt hơn gấp trăm lần tiền token. |
| **3. Gửi xong là quên ngay (Fire & Forget)** | Tuyệt đối không ngồi nhìn chữ chạy. Gửi prompt xong là chuyển sang thread/issue tiếp theo. |
| **4. Quản lý Thread như Hộp thư đến (Inbox Zero)** | Dùng cơ chế **Settle** (tự động đóng khi PR merge) và **Snooze** (tạm ẩn) để thanh bên chỉ hiển thị những việc thực sự cần làm ngay lúc này. |
| **5. Tự động hóa vòng lặp Review bằng `$babysit`** | Giao việc đọc bình luận của bot review, sửa lỗi lặt vặt và chờ CI xanh cho một kỹ năng tự động hóa. |
| **6. Triệt tiêu mọi ma sát khi Test** | Tự xây dựng các đường tắt như `--share-dev` (Tailscale), `preview:mac` (rolling prerelease không cần login), và `sqim` (iOS) để việc kiểm thử chỉ tốn vài giây. |
| **7. Lưới an toàn (Safety Nets) > Rào chắn (Guardrails)** | Thay vì làm chậm quy trình bằng các thủ tục chặn trước khi merge, hãy phát hành bản Nightly mỗi 3 tiếng để bắt lỗi tức thì trong thực tế. |

---

## 10. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

### Notes về T3 Code:
- [T3 Code Đã Chinh Phục Jan Marshal](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/t3_code_won_me_over_jan_marshal.md) — Giải phẫu Orchestrator V2, T3 Connect và bài toán kinh tế Subsidized Usage (\$23,464 API value từ gói thuê bao)
- [T3 Code Giải Thích Toàn Diện — いまにゅ](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/t3_code_徹底解説_aimy.md) — Định vị ma trận 2×2 (GUI + Đa mô hình) và thực chiến Remotion

### Notes về Workflow & High-Throughput Engineering:
- [Live — Poteto, pStack, shipping thousands PRs](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/live_poteto_pstack_shipping_thousands_prs_spacex_matt_pocock.md) — Quy trình của Lauren Tan (`poteto`)
- [My New AI Coding Setup — Ship 30 PRs a Day](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/my_new_ai_coding_setup_ship_30_prs_a_day.md) — Ngăn xếp Dual-Driver \$400/tháng của Thomas Dohyun Kim
- [High Throughput Agentic Engineering](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/high_throughput_agentic_engineering_kun_chen.md) — Mở rộng quy mô điều phối tác nhân

### Notes về Kiểm Thử, Code Review & PR Bottleneck:
- [How I Review AI Code — Meta Staff Engineer](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/how_i_review_ai_code_meta_staff_engineer.md) — Phương pháp kiểm toán code do AI sinh ra
- [Fixing the PR Bottleneck](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/fixing_the_pr_bottleneck_matt_pocock_aihero.md) — Giải quyết điểm nghẽn PR của Matt Pocock

### Notes về Remote Execution & Linux Multiplexing:
- [Herdr: Beginner To Advanced](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/herdr_beginner_to_advanced_lustoykov.md) — Điều phối đa tác nhân và kết nối máy trạm từ xa qua SSH
- [The Best Git Worktree Tool (Worktrunk)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/the_best_git_worktree_tool_worktrunk_better_stack.md) — Quản lý hàng chục nhánh git worktree không tốn dung lượng ổ cứng
