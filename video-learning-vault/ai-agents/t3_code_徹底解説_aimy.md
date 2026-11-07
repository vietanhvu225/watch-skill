# T3 Code Giải Thích Toàn Diện — Môi Trường AI Agent Tốt Nhất Cho Mọi Người (Không Cần Terminal) | いまにゅのAIプログラミング塾

- **Video URL:** [https://www.youtube.com/watch?v=DzXAsObvMio](https://www.youtube.com/watch?v=DzXAsObvMio)
- **Video ID:** `DzXAsObvMio`
- **Watch Skill ID:** `edb481699633f512`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 23:34 (61 frames trích xuất & phân tích trực quan)
- **Speakers:** **Imanyu (いまにゅ)** — Giảng viên AI Programming, Founder của `code4biz` (株式会社ミチガエル) & **J-kun (J君)** — Co-host trở lại ("J君復活祭")

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│      T3 CODE — MA TRẬN ĐỊNH VỊ 2×2 CỦA IMANYU (FRAME 0008)                   │
│                                                                              │
│                       いろんなAIを使える (Dùng được nhiều AI)                 │
│                                      ▲                                       │
│                   ┌──────────────────┼──────────────────┐                    │
│                   │                  │                  │                    │
│                   │  ★ T3 Code       │  Superset · Orca │                    │
│                   │  (GUI + Đa AI)   │  (>_ Terminal)   │                    │
│                   │                  │                  │                    │
│  アプリ (GUI App) ◄──────────────────┼──────────────────► ターミナル (むずい) │
│                   │                  │                    (Terminal: Khó!)   │
│                   │  Claude Code App │                  │                    │
│                   │  Codex App       │                  │                    │
│                   │                  │                  │                    │
│                   └──────────────────┼──────────────────┘                    │
│                                      ▼                                       │
│                         使えるAIは1つ (Chỉ dùng 1 AI)                         │
└──────────────────────────────────────────────────────────────────────────────┘
```

Video từ kênh giáo dục AI Programming hàng đầu Nhật Bản **いまにゅのAIプログラミング塾** giải thích toàn diện về **T3 Code (`t3.codes`)** — *"The open-source control plane for coding agents"* (24k+ GitHub stars, *"Tolerated by over 400,000 devs"*). Cùng với sự trở lại của trợ lý **J-kun** (đại diện cho nhóm người dùng non-engineer sợ màn hình đen Terminal), Imanyu chỉ ra vì sao các công cụ multi-harness như **Superset** hay **Orca** quá nặng về Terminal đối với người mới, trong khi **Claude Code Desktop** và **Codex Desktop** lại bị khóa chặt vào 1 hệ sinh thái mô hình duy nhất. **T3 Code** lấp đầy khoảng trống góc phần tư **App (GUI) + Đa mô hình**, hỗ trợ đa nền tảng (macOS, Windows 10/11, Linux, iPhone/iPad, Android), liên kết trực tiếp gói thuê bao sẵn có trong 10 giây, điều khiển đa thiết bị (MacBook Pro + Mac mini ở nhà), và đi kèm bộ công cụ Right Panel (Browser, Terminal, Files, Diff, PR, Sub-agents, Device).

---

## 2. Định Vị T3 Code Trên Bản Đồ AI Agent Harnesses (`00:00 – 04:05`)

### 2.1 Bối Cảnh & Ma Trận 2×2 (`frame_0008`)

*(Lưu ý kỹ thuật ở `00:17`, `frame_0004`: Micro của Imanyu gặp sự cố nhiễu nhẹ trong buổi ghi hình mừng J-kun trở lại nên ekip đã bổ sung phụ đề telop tiếng Nhật chi tiết xuyên suốt video).*

Imanyu chia sẻ thực tế từ trường đào tạo `code4biz` (nơi dạy AI cho dân văn phòng & non-engineers):
- Trước đây, anh từng giới thiệu **Superset** và **Orca** — các công cụ rất mạnh cho phép chạy song song nhiều AI agents nhưng lại đặt trọng tâm vào **Terminal (`>_`)**. Nhiều học viên (và cả J-kun) thấy màn hình dòng lệnh quá đáng sợ (`むずい` — khó nhằn), dẫn đến việc bỏ cuộc và quay về dùng **Claude Code Desktop App** hoặc **Codex Desktop App**.
- Tuy nhiên, dùng app chính hãng của từng bên lại gặp hạn chế lớn: chỉ gọi được mô hình của đúng hãng đó (Claude Code chỉ chạy Claude; Codex chỉ chạy GPT).
- **T3 Code (`t3.codes`, `frame_0013`–`frame_0014`, `frame_0059`)** giải quyết triệt để bài toán này:
  - **Slogan chính thức:** *"The open-source control plane for coding agents. Orchestrate Claude Code, Codex, Antigravity, OpenCode, Cursor, and Grok from one surface. Bring your own subscription. Fork the whole thing."*
  - Giao diện mang cảm giác gần như **90% giống Codex Desktop App / Claude Code Desktop App**, nhưng bên dưới có thể điều phối mọi harness lớn.

### 2.2 Hỗ Trợ Đa Nền Tảng — Phá Bỏ Tư Duy "Mac-First" (`03:38 – 03:58`, `frame_0013`, `frame_0050`)

Trên trang chủ `t3.codes`, T3 Code cung cấp bộ cài đặt đầy đủ cho:
- **macOS:** Apple Silicon (`arm64`) & Intel (`x64` build).
- **Windows:** `Windows 10, 11 (.exe)` — Imanyu nhấn mạnh đây là điểm cộng cực lớn tại Nhật Bản, nơi đa số học viên và doanh nghiệp dùng Windows và thường bị bỏ rơi bởi các công cụ AI "Mac-first".
- **Linux:** `Debian, Ubuntu (.deb)` và `x86_64 (AppImage)` (cùng bản `arm64`).
- **Mobile (`frame_0050`):** Có ứng dụng trên **iPhone & iPad (App Store)** và **Android (Google Play)** để kết nối từ xa vào máy tính ở nhà.
- **Bản Nightly vs Stable:** Imanyu khuyên người dùng có thể cài song song hoặc ưu tiên bản **T3 Code (Alpha / Nightly)** vì tính năng mới được cập nhật liên tục mỗi ngày.

---

## 3. Giao Diện, Themes & Cấu Hình Providers/Environments (`04:05 – 07:25`)

### 3.1 Phím Tắt Model Favorites (`frame_0000`, `frame_0016`–`frame_0017`)

Trong menu chọn model của T3 Code, người dùng có thể gắn sao (**Favorites**) để chuyển đổi tức thì bằng phím tắt `⌘1`, `⌘2`, `⌘3`:
- Bộ Favorites thực tế của Imanyu (`frame_0017`):
  - `⌘1` — **GPT-6.1-Sol** *(Codex)*
  - `⌘2` — **Claude Opus 5.5** `[NEW]` *(Claude)*
  - `⌘3` — **Claude Sonnet 5.5** `[NEW]` *(Claude)*
  - Ngoài ra trong danh sách còn có **GPT-6.1 Luna**, v.v.

### 3.2 Cá Nhân Hóa Giao Diện & Themes (`frame_0018`–`frame_0019`)

Trong `Settings -> Appearance`:
- **Color scheme:** `System` / `Light` / `Dark`.
- **Light theme / Dark theme:**
  - `T3 Code` *(Default — Clean neutral palette built for focus)*
  - `T3 Chat` *(T3 Chat palette — tông màu hồng đặc trưng mà J-kun hài hước gọi là "エロいピンク" / hồng quyến rũ, được Imanyu chọn dùng xuyên suốt buổi demo)*
  - `Grove` *(Warm earthy greens with natural contrast)*
  - `Ocean` *(Cool coastal blues with crisp depth)*
  - `Ember` *(Warm amber tones with a soft glow)*
  - `Iris` *(Soft violet accents with balanced contrast)*
  - Nút **`+ Create theme`** / **`+ Add theme`** cho phép tự tạo theme riêng.
- **Contrast slider:** Điều chỉnh độ tương phản (`100%` mặc định) và **Diff colors** (`Red & green`).

### 3.3 Liên Kết Subscriptions Trong 10 Giây (`05:20 – 06:30`, `frame_0001`)

Trong `Settings -> Providers`, T3 Code tự động phát hiện và liên kết các gói thuê bao CLI/Desktop sẵn có trên máy mà **không tốn thêm phí API riêng**:
- **Codex:** `v0.160.0` — `Authenticated · ChatGPT Pro 20x Subscription`
- **Claude:** `v2.1.209` — `Authenticated`
- **Cursor:** `v2026.09.10-f2a0623` — `Authenticated · Cursor Ultra Subscription`
- **Grok:** Hỗ trợ bật/tắt trực tiếp bằng nút gạt.

### 3.4 Quản Lý Đa Môi Trường — `Connections / Environments` (`06:30 – 07:25`, `frame_0020`)

Tại mục `Settings -> Connections`:
- Imanyu kết nối đồng thời 2 máy:
  1. **`MacBook Pro`** *(This device — máy đang dùng để quay video)*
  2. **`ImanyuのMac mini`** *(Máy đặt cố định ở nhà)*
- Khi chuyển tab môi trường trên đầu trang `Providers` (`MacBook Pro` ↔ `ImanyuのMac mini`), mỗi máy quản lý trạng thái đăng nhập Provider độc lập. Người dùng có thể giao việc nặng cho chiếc Mac mini ở nhà chạy nền và theo dõi từ MacBook Pro hoặc điện thoại iPhone khi ra ngoài.

---

## 4. Khởi Tạo Dự Án, Mức Effort & Thực Chiến Tạo Video Bằng Code (`07:25 – 16:08`)

### 4.1 Thêm Project & Chọn Nguồn (`07:25 – 08:30`, `frame_0021`–`frame_0023`)

Khi bấm **`Add project`**:
1. **Chọn Environment:** `MacBook Pro (This device)` hoặc `ImanyuのMac mini`.
2. **Chọn Sources (`frame_0022`):**
   - `New project`
   - `Local folder` (Imanyu chọn thư mục `/Users/imanyu/projects/YouTube/20261005/T3 Code 実演用`)
   - `Git URL`
   - `GitHub repository`
   - `Azure DevOps repository`
   - `Bitbucket repository`
3. Ngoài ra còn có chế độ **`Without project`** (`frame_0024`) để mở nhanh một luồng chat/tác vụ độc lập mà không cần gắn vào thư mục dự án cố định.

### 4.2 Tinh Chỉnh Effort, Context Window & Permission (`08:30 – 09:50`, `frame_0025`)

Ở thanh công cụ bên dưới khung chat, khi chọn **Claude Opus 5.5**, T3 Code hiển thị bảng điều khiển chi tiết (`frame_0025`):
- **Effort Levels:**
  - `Medium`
  - `High`
  - `Extra High`
  - `Max`
  - `Ultracode` *(xhigh effort plus multi-agent workflow orchestration)*
  - `Ultrathink`
- **Context Window:** `200k` hoặc `1M Default`.
- **Fast Mode:** Bật/tắt (`On` / `Off`) để tăng tốc độ phản hồi.
- **Permission Mode:** `Supervised` / `Auto` / `Full access`.

### 4.3 Live Demo 1: Tạo Video Animation 10 Giây Mừng J-kun Trở Lại (`09:55 – 14:55`)

- **Prompt (nhập bằng giọng nói qua Aqua Voice):** Tạo một video animation khoảng 10 giây theo chủ đề *"J-kun đã trở lại như một người hùng"* (`J君帰還のヒーローアニメーション`) — nhân vật anh hùng màu đỏ, đeo găng tay đấm bốc (boxing gloves), giơ nắm đấm lên trời.
- **Xử lý lỗi OAuth thực tế (`frame_0027`):** Ngay lần chạy đầu, T3 Code báo đỏ: `Failed to authenticate: OAuth session expired and could not be refreshed — Claude could not authenticate. For subscription login, run "claude auth login"`. Imanyu chỉ cần mở terminal gõ `claude auth login` là phiên chạy tiếp tục trơn tru (`frame_0028`).
- **Chạy đa luồng song song (`11:08 – 11:45`):** Trong lúc luồng 1 đang chạy Claude Opus 5.5 (`Working...`), Imanyu mở ngay một thread mới bên cạnh và chọn **GPT-6.1** để làm việc khác mà không hề bị xung đột.
- **Kết quả Lần 1 — Python (`PIL + numpy + ffmpeg`, `frame_0037`–`frame_0039`):**
  - Sau `5m 29s`, Claude Opus 5.5 tự viết script `j_returns.py` dùng `PIL + numpy` vẽ từng frame (`1920x1080 / 30fps / 10.0秒`), tự tạo hiệu ứng âm thanh fanfare bằng bộ lọc thông thấp (`one-pole lowpass filter` trên `numpy` vì máy chưa cài `scipy`), tự kiểm tra preview từng mốc thời gian (`0~2秒: 夜の街に星が光り、「あの男が、ついに……」の字幕が出る`), dịch chuyển vị trí đám mây khỏi đè lên phụ đề rồi xuất ra file `J君が帰ってきた.mp4`.
  - Khi mở xem trực tiếp trong Right Panel của T3 Code (`frame_0038`), J-kun bật cười thích thú vì có cả nhạc fanfare và hình nhân vật chữ `J` đeo găng đỏ, nhưng Imanyu đánh giá bản vẽ bằng Python còn hơi sơ sài ("クソしょぼい").

### 4.4 Live Demo 2: Nâng Cấp Lên Remotion & Triết Lý Deterministic vs Probabilistic (`14:45 – 23:10`)

- Imanyu yêu cầu agent làm lại bằng **JavaScript / Remotion** (hoặc Hyperframe) để đạt chất lượng đồ họa cao hơn (`frame_0042`).
- **Góc nhìn chiến lược của Imanyu — Tạo video bằng Code (`15:00 – 15:52`, `frame_0040`–`frame_0041`):**
  - **Tiếp cận Xác suất (`確率論的なアプローチ` — Generative Video AI như Sora/Runway):** Linh hoạt, tự do nhưng mỗi lần chạy lại ra một sắc thái/gu thẩm mỹ khác nhau, rất khó chỉnh sửa chính xác một chi tiết nhỏ.
  - **Tiếp cận Tất định (`決定論的なアプローチ` — Programmatic Video bằng Code như Remotion/Python):** Viết code để dựng video giúp đảm bảo tính nhất quán tuyệt đối (chạy 100 lần ra kết quả giống hệt 100 lần), dễ dàng chỉnh sửa đúng 1 dòng chữ hay màu sắc và tái sử dụng làm template hàng loạt. Imanyu khẳng định dựng và biên tập video bằng chương trình sẽ trở thành xu hướng chủ đạo.
- **Kết quả Lần 2 — Remotion (`frame_0047`–`frame_0049`, `frame_0051`–`frame_0058`):**
  - Agent tự động tạo thư mục `j-returns-remotion/`, dựng khung xương SVG (`SVGの骨格リグ`), kiểm tra ảnh tĩnh (`stills/sheet1.png`, `sheet2.png`, `sheet3.png`, `jzoom.png`), phát hiện chữ `"J"` ở tiêu đề hơi bị lẹm phần đầu nên tự căn chỉnh lại rồi mới render file `J君が帰ってきた_Remotion.mp4`.
  - Bản Remotion có hiệu ứng tia chớp đánh xuống bầu trời đêm thành phố (`frame_0056`), hiệu ứng rung chấn camera, tia sáng mặt trời xoay tròn rực rỡ và nhân vật cử động mượt mà hơn hẳn (`frame_0057`–`frame_0058`).

---

## 5. Khám Phá Right Panel, Skills & Cài Đặt Mặc Định (`16:08 – 19:50`)

### 5.1 Menu Công Cụ Right Panel (`16:12 – 17:35`, `frame_0043`–`frame_0044`)

Khi bấm dấu `+` ở thanh điều hướng bên phải (`frame_0044`), T3 Code cung cấp 8 chế độ xem chuyên biệt kèm phím tắt:
1. **`Browser` (`B`)** — Trình duyệt tích hợp ngay trong app để xem live preview hoặc duyệt web; hỗ trợ cả tính năng **Annotate** (khoanh vùng/vẽ trực tiếp lên giao diện để chỉ định chỗ cần sửa giống Codex Desktop App, `17:15`).
2. **`Terminal` (`T`)** — Mở cửa sổ dòng lệnh khi cần chạy lệnh thủ công.
3. **`Files` (`F`)** — Cây thư mục dự án (`Search files`), xem code trực tiếp và phát file media (`.mp4`, `.png`) ngay trong T3 Code.
4. **`Diff` (`D`)** — Xem chi tiết các dòng code thay đổi.
5. **`Pull request` (`P`)** & **`Linked pull requests` (`L`)** — Quản lý PR trực tiếp cho kỹ sư phần mềm.
6. **`Agents` (`A`)** — Quan sát các **Sub-agents** (`Spawn sub-agents`) đang chạy song song với giao diện trực quan tương tự Codex Desktop (`16:28 – 16:45`).
7. **`Device` (`M`)** — Quản lý thiết bị/môi trường kết nối.

### 5.2 Gọi Skills (`/`) & Mentions (`@`) — Cùng Ứng Dụng Thực Tế Tạo Tài Liệu 50 Trang (`12:30 – 13:40`, `17:40 – 18:10`)

- Trong ô nhập lệnh (`Ask anything, @tag files/folders, $use skills, or / for commands`):
  - Dùng `/` hoặc `$` để gọi các **Skills** đã cài trong Claude Code và Codex.
  - Dùng `@` để đính kèm file/thư mục (Imanyu lưu ý nhỏ rằng trên bản Nightly hiện tại phím `@` đôi lúc phản hồi hơi trễ một nhịp, chỉ cần bấm chọn lại là được).
- **Minh họa sức mạnh của Skill tạo tài liệu (`12:30 – 13:38`, `frame_0030`–`frame_0036`):**
  - Trong lúc chờ agent render video, Imanyu mở bộ tài liệu giới thiệu trường học **code4biz スクールご案内** dài **50 trang** (`TOP10%のAI人材へ — 「AIといえば、あの人」になる`, `frame_0030`–`frame_0036`) được tạo hoàn toàn bằng **Skill tạo tài liệu (`資料作成スキル`)** chạy trên Claude Code.

### 5.3 Thiết Lập Mặc Định & Lưu Ý Ngôn Ngữ (`18:35 – 19:45`, `frame_0046`–`frame_0047`)

- Người dùng có thể cấu hình mặc định cho mỗi khi mở thread mới:
  - **Default Permission:** Chọn sẵn `Auto mode` hoặc `Full access` để không phải bấm chỉnh lại mỗi lần.
  - **Default Model:** Imanyu đặt mặc định cho New Thread là **`GPT-6.1-Sol`** (hoặc `GPT-6 Astra`).
- **Điểm hạn chế duy nhất hiện tại (`19:30 – 19:44`):** Giao diện T3 Code hiện chưa có tùy chọn ngôn ngữ tiếng Nhật (chỉ có tiếng Anh), nhưng bố cục rất trực quan nên chỉ cần nắm khu vực chỉnh Model/Effort ở dưới khung chat và Right Panel bên phải là có thể sử dụng thành thạo.

---

## 6. Bảng Đối Chiếu Các Công Cụ AI Coding Theo Góc Nhìn Đào Tạo

| Tiêu chí | Claude Code / Codex Desktop | Superset / Orca | **T3 Code (`t3.codes`)** |
| :--- | :--- | :--- | :--- |
| **Giao diện chính** | GUI Desktop thân thiện | Nặng về Terminal (`>_`) | **GUI Desktop hiện đại (6 themes + Custom)** |
| **Số lượng AI / Harnesses** | Khóa vào 1 hãng duy nhất | Đa mô hình / Đa agent | **Đa nền tảng (Claude, Codex, Cursor, Grok, Antigravity, OpenCode)** |
| **Hệ điều hành hỗ trợ** | Chủ yếu ưu tiên macOS trước | Desktop (Mac/Linux/Win) | **macOS, Windows 10/11, Linux + iOS & Android** |
| **Kết nối đa máy từ xa** | Hạn chế | Tùy cấu hình SSH | **Tích hợp sẵn Connections (MacBook ↔ Mac mini ↔ Mobile)** |
| **Độ thân thiện với Non-Engineer** | ⭐⭐⭐⭐⭐ | ⭐⭐ (Dễ nản vì CLI) | **⭐⭐⭐⭐⭐ (Quen thuộc ngay lập tức)** |

---

## 7. Liên Kết Chéo Trong Video Learning Vault

- [T3 Code Đã Chinh Phục Jan Marshal (`SAFrydhpgJo`)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/t3_code_won_me_over_jan_marshal.md) — Phân tích chuyên sâu T3 Code từ góc nhìn kỹ sư phần mềm.
- [Bạn Đang Dùng AI Agents Sai Cách — Theo t3.gg (`q1D90-uGvBg`)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/youre_using_ai_agents_wrong_theo_t3.md) — Triết lý thiết kế của tác giả T3 Code.
- [Setup AI Coding Ship 30 PRs/Ngày (`54D1hFfaju8`)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/my_new_ai_coding_setup_ship_30_prs_a_day.md) — Quy trình phối hợp Claude Opus 4.6 và Codex 5.4 trong thực tế.
- [Herdr: Beginner To Advanced (`WTmCkYMelIU`)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/herdr_beginner_to_advanced_lustoykov.md) — Công cụ điều phối đa agent hướng terminal.
