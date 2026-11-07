# T3 Code Đã Hoàn Toàn Chinh Phục Tôi — Giải Phẫu ADE, Orchestrator V2 & T3 Connect | Jan Marshal

- **Video URL:** [https://www.youtube.com/watch?v=SAFrydhpgJo](https://www.youtube.com/watch?v=SAFrydhpgJo)
- **Video ID:** `SAFrydhpgJo`
- **Watch Skill ID:** `af3ab01b3545222c`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 32:18 (74 frames trích xuất & phân tích trực quan)
- **Speaker:** **Jan Marshal** (Founder của `SyntaxPath` / `SyntaxKit` & `CargoMarshal`, đóng quân tại Đức)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│     KIẾN TRÚC T3 CODE ADE — SƠ ĐỒ TLDRAW CỦA JAN MARSHAL (FRAME 0025–0027)   │
│                                                                              │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │                                T3 Code                                 │  │
│  │  ┌──────────────────────────────────────────────────────────────────┐  │  │
│  │  │                         Desktop / Mobile                         │  │  │
│  │  └────────────────────────────────┬─────────────────────────────────┘  │  │
│  │                                   ▼                                    │  │
│  │  ┌──────────────────────────────────────────────────────────────────┐  │  │
│  │  │                         Orchestrator V2                          │  │  │
│  │  │            Threads, delegation, Provider Switching               │  │  │
│  │  └────────────────────────────────┬─────────────────────────────────┘  │  │
│  └───────────────────────────────────┼────────────────────────────────────┘  │
│                ┌─────────────────────┼─────────────────────┐                 │
│                ▼                     ▼                     ▼                 │
│        ┌──────────────┐      ┌──────────────┐      ┌──────────────┐          │
│        │    Cursor    │      │    Codex     │      │ Claude Code  │  (+AGY)  │
│        └──────┬───────┘      └──────┬───────┘      └──────┬───────┘          │
│               └─────────────────────┼─────────────────────┘                  │
│                                     ▼                                        │
│  ┌────────────────────────────────────────────────────────────────────────┐  │
│  │                            Your Environment                            │  │
│  │    Project Files, Git Worktrees, Terminal, Tools (Skills, MCP, ...)    │  │
│  └────────────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────────────┘
```

Từng ghét cay đắng **T3 Code** ở lần thử đầu tiên (thời `Orchestrator V1` còn khóa chặt 1 harness cho mỗi thread), **Jan Marshal** đã hoàn toàn bị chinh phục sau 6 ngày chuyển sang bản **T3 Code Nightly (`v0.0.40+`)** để phát triển các sản phẩm thực tế (`syntaxkit-final`, `cargo-new`, `fable-landing-page`, `final-cut-pro`). Bài phân tích 32 phút của anh đính chính hiểu lầm phổ biến nhất trên Twitter/X: **T3 Code KHÔNG phải là một Harness**, mà là một **ADE (Agentic Development Environment)** kiêm **Multi-Agent Orchestrator** nằm *bên trên* các harnesses chính hãng (Claude Code, Codex, Cursor, Antigravity). Chính quyết định kiến trúc "không tự viết harness riêng" như OpenCode đã giúp T3 Code lách luật hợp pháp để tận dụng **Subsidized Usage** từ các gói thuê bao cố định (như trả **\$100/tháng** Claude Code nhưng xài tới **\$3,728.94** giá trị API, hay tổng **\$23,464.57 API estimate** kèm **\$110,791.58 Cache Savings** trong 90 ngày). Bên cạnh các vũ khí sát thủ như **Orchestrator V2** (Cross-Harness Sub-agents & Mid-Session Context Handoff), **T3 Connect** (Real-time Sync Engine đa thiết bị), **Usage Window**, **Auto PR Subscription** và **Standardized Visualizations**, Jan cũng chỉ ra thẳng thắn **5 điểm yếu chí mạng** cần sớm khắc phục.

---

## 2. Đính Chính Khái Niệm: T3 Code Là ADE, Không Phải Harness (`00:00 – 05:48`)

### 2.1 Tại Sao "T3 Code Is NOT a Harness"? (`frame_0007`, `frame_0018`–`frame_0027`)

Trên X/Twitter, nhiều người dùng (như `@alxpkhl`, `frame_0019`–`frame_0022`) ca ngợi *"t3code is the best harness out there atm"*. Jan Marshal vẽ sơ đồ trực tiếp trên `tldraw.com` (`frame_0018`, `frame_0023`, `frame_0025`–`frame_0027`) để làm rõ biên giới kỹ thuật:
- **Model = Bộ não (`The brain`):** Thực hiện suy luận, tư duy.
- **Harness = Hệ thống công cụ & Quản lý thực thi (`frame_0023`):** Cung cấp công cụ đọc/sửa file, chạy lệnh terminal, quản lý ngữ cảnh (context compaction), xin quyền (permissions). Mỗi hãng (`Claude Code`, `Codex`, `Cursor`, `Antigravity`) đều tự xây dựng và tối ưu harness riêng cho mô hình của họ.
- **T3 Code = Agentic Development Environment (ADE):** Gồm 2 khối chính là **Application (Desktop / Mobile)** và **Orchestrator V2** nằm *phía trên* các harnesses sẵn có (`frame_0025`).

### 2.2 "Cheat Code" Kinh Tế: Vì Sao Không Tự Viết Harness Lại Vượt Trội Hơn OpenCode? (`04:30 – 05:05`, `11:45 – 12:35`)

- **Bài học từ OpenCode:** OpenCode cũng có ý tưởng gom các gói thuê bao vào một nơi, nhưng vì OpenCode **tự viết harness riêng của mình**, Anthropic đã chặn không cho phép dùng gói thuê bao Claude Code (Max/Pro) bên trong OpenCode. Hơn nữa, một harness đa dụng của bên thứ ba không thể hiểu sâu về hành vi nội tại của mô hình Claude bằng chính harness `Claude Code` do kỹ sư Anthropic tinh chỉnh.
- **Lợi thế của T3 Code:** Vì T3 Code gọi trực tiếp harness chính chủ (`Claude Code CLI`, `Codex CLI`, `Cursor CLI`) ở tầng dưới:
  1. Hoàn toàn hợp lệ để dùng gói thuê bao cố định hàng tháng.
  2. Khai thác triệt để **Subsidized Usage** (lượng token được các ông lớn Anthropic, OpenAI, Cursor trợ giá).
  3. Khai thác được chất lượng harness tốt nhất của từng hãng trong cùng một giao diện.

---

## 3. Orchestrator V2: Điều Phối Xuyên Harness & Chuyển Giao Ngữ Cảnh Giữa Chừng (`03:15 – 14:18`)

### 3.1 Khuyến Nghị Số 1: Luôn Cài Bản Nightly (`07:51 – 08:35`, `frame_0045`, `frame_0052`)

- Dấu hiệu nhận biết bản **Nightly (`t3.codes/download?channel=nightly`, `v0.0.40+`)**: Góc trên bên trái của sidebar có hình nền dải ngân hà tím (**Galaxy background**, `frame_0011`, `frame_0044`).
- **Sự khác biệt giữa V1 (Stable) và V2 (Nightly):**
  - **Orchestrator V1 (Stable):** Mỗi khi mở một thread bằng model của hãng nào (ví dụ `Opus 5.5` của Claude Code), toàn bộ thread đó và mọi sub-agents bên trong bị **khóa chết** vào đúng harness đó.
  - **Orchestrator V2 (Nightly):** Mở khóa hoàn toàn khả năng phối hợp đa harness trong cùng một phiên làm việc, tốc độ mượt hơn gấp nhiều lần, tích hợp sẵn Visualization và T3 Connect ổn định hơn.

### 3.2 Thực Chiến Cross-Harness Sub-Agents (`09:20 – 11:45`, `frame_0047`)

Jan demo trực tiếp trên repo `ski043/syntaxkit-final`:
1. Tạo thread mới → T3 Code tự động chạy `Setting up worktree` dựa trên nhánh `origin` mới nhất (`t3/...`) để không làm bẩn nhánh hiện tại.
2. Chọn **Main Agent** là **`GPT-6.1-Sol` (High reasoning effort)** chạy trên **Codex harness**.
3. Ra lệnh cho GPT-6.1-Sol: *"Find out when this repository was created, how many PRs have already been created, and when the last PR was merged. Please spin up a Haiku 5.5 sub-agent to just find out..."*
4. **Kết quả (`frame_0047`):** T3 Code hiển thị trạng thái `Getting orchestration capabilities` và khởi chạy một sub-agent mang **biểu tượng hoa thị cam của Claude (`Claude Haiku 5.5`)** ngay bên dưới Main Agent của **Codex**!
5. Sub-agent `Claude Haiku 5.5` chạy lệnh `gh api repos/ski043/syntaxkit-final` và `GraphQL repository.pullRequests`, báo cáo chính xác:
   - Tổng số PR từng tạo: **476 PRs** (`open: 3`, `closed-unmerged: 84`, `merged: 389`).
   - PR mới nhất được merge: **PR #476** (`fix(qa): complete auth integration storage mock; harden verify docs`, merged `2026-10-10 14:48:16 Europe/Berlin`).
- *(Chi tiết thú vị ở `11:13`)*: Trong trang `Settings -> Providers`, T3 Code tự động làm mờ (`blur`) tên hiển thị và email tài khoản mặc định — một tính năng cực kỳ tâm lý do chính một YouTuber (Theo) thiết kế cho giới sáng tạo nội dung.

### 3.3 Vòng Lặp Bất Tử: Mid-Session Context Handoff Khi Chạm Rate Limit (`12:35 – 14:18`, `frame_0048`)

Jan trình bày một quy trình từ A đến Z trên nhánh `Audit React Upgrade Best Practices` (`PR #470`):
1. Bắt đầu với **Main Agent = `Claude Opus 5.5`**: Yêu cầu chạy `react-doctor` CLI và tung **10 research sub-agents (`Haiku 5.5`)** để kiểm toán toàn bộ dự án, sau đó gọi bộ kỹ năng **`pstack` (`poteto-mode`)** để sửa lỗi.
2. Đang sửa dở thì gặp thông báo đỏ: **`Claude usage limit reached. The turn is paused until the 5-hour limit resets in 2 hours.`**
3. Thay vì ngồi chờ 2 tiếng, ngay trong cùng khung chat đó, Jan chỉ cần đổi model và nhắn: *"Hey, I'm out of my usage limit, so please switch to GPT-6.1-Sol."*
4. T3 Code tự động tạo khối **`Context handoff: Claude Opus 5.5 → GPT-6.1-Sol`** và tiếp tục công việc ngay lập tức trên cùng git worktree!
5. Khi gói Codex chạm giới hạn, anh lại chuyển tiếp sang **Cursor (`Grok 4.7`)**, rồi quay vòng lại **Claude Opus 5.5** (`Claude Code → Codex → Cursor → Claude Code`).

---

## 4. T3 Connect: Động Cơ Đồng Bộ Thời Gian Thực Đa Thiết Bị (`19:20 – 23:32`)

### 4.1 Hơn Cả Remote SSH — Đó Là Một Real-Time Sync Engine (`frame_0062`)

Trong `Settings -> Connections`, Jan kết nối đồng thời:
- **`Mac Studio von Jan`** *(Local machine tại Đức — máy trạm đang quay video)*
- **`MacBook von Jan`** *(Remote laptop)*
- **Ứng dụng T3 Code trên iPhone** *(Cài bản Nightly Mobile từ `Settings -> General -> Mobile application`)*

**Điểm khác biệt cốt lõi so với `claude rc` (Claude Code Remote Control):**
- Với `claude rc`, mỗi lần muốn điều khiển từ xa phải mở terminal gõ lệnh, chờ khởi tạo phiên, chọn cấu hình rồi mới mở app điện thoại.
- Với **T3 Connect**, chỉ cần đăng nhập cùng một tài khoản là mọi thiết bị **luôn luôn kết nối (`Always connected`)**:
  - Đứng từ `Mac Studio`, Jan chọn tạo thread mới nhưng trỏ Environment sang `MacBook von Jan` → T3 Code tự tạo worktree `t3/3b5830bd` và chạy lệnh trực tiếp trên chiếc MacBook (`jan@MacBook-Pro-von-Jan`, `frame_0062`).
  - Mở iPhone lên, vào đúng thread đó, đổi model sang **`GPT-6 Luna`** và gửi *"Yo, what's up?"* → Ngay lập tức màn hình Desktop xuất hiện khối `Context handoff` sang `GPT-6 Luna`; đổi tiếp trên iPhone sang **`Cursor (Grok 4.7)`** → Desktop cập nhật theo thời gian thực (`Context handoff: Opus 5.5 → GPT-6 Luna → Grok 4.7`).
  - **Đồng bộ cả Settings:** Khi Jan đổi `Default model` sang `Grok 4.7` trên chiếc MacBook, màn hình cài đặt trên chiếc Mac Studio tự động nhảy sang `Grok 4.7` ngay tức khắc mà không cần bấm tải lại trang (`23:20`).

---

## 5. Các Tính Năng Đắt Giá Khác Khiến Jan Nghiện T3 Code (`23:32 – 30:18`)

### 5.1 Usage Window — Bảng Thống Kê Chi Phí & Token Chi Tiết Nhất Thị Trường (`01:00`, `26:15 – 27:35`, `frame_0016`, `frame_0068`)

Không một IDE hay Harness nào khác (kể cả Cursor, OpenCode hay Antigravity) cung cấp bảng phân tích giá trị API quy đổi chi tiết như mục **`Usage / All environments`** của T3 Code:

| Khung thời gian | Tổng giá trị API quy đổi | Chi tiết theo Provider / Model (Trích xuất từ `frame_0016` & `frame_0068`) | Thống kê Tokens & Cache Savings |
| :--- | :--- | :--- | :--- |
| **90 ngày (`frame_0016`)** | **\$23,464.57** *(3,702 sessions)* | • **Cursor** *(3,208 sessions)*: **\$18,181.83** (`77.5%`, `21.1B tokens`)<br>• **Claude Code** *(204 sessions)*: **\$3,728.94** (`15.9%`, `10.7B tokens` — chỉ từ gói thuê bao \$100/tháng!)<br>• **Codex** *(281 sessions)*: **\$1,482.72** (`6.3%`, `2.18B tokens`)<br>• **OpenCode** *(9 sessions)*: **\$71.08** (`0.3%`, `179M tokens`) | • **Processed tokens:** `34.2B`<br>• **Cached input:** `32.4B`<br>• **Uncached input:** `989M`<br>• **Output:** `144M`<br>• **Cache savings:** **\$110,791.58** |
| **24 giờ qua (`frame_0068`)** | **\$293.47** *(84 sessions)* | • **Claude Code** *(13 sessions)*: **\$185.79** (`63.2%`, `687M tokens`)<br>• **Cursor** *(71 sessions)*: **\$107.68** (`36.8%`, `108M tokens`)<br>*(Breakdown theo model: `#1 claude-opus-5-5` \$149.90 / 403M tokens; `#2 github_bugbot` \$68.90 / 44M tokens; `#3 claude-sonnet-5-5` \$34.18 / 189M tokens)* | • **Processed tokens:** `796M`<br>• **Cached input:** `757M`<br>• **Uncached input:** `14.9M`<br>• **Output:** `3.75M`<br>• **Cache savings:** **\$1,891.15** |

### 5.2 Auto PR Subscription & Pull Request Review Tool (`00:51 – 00:56`, `27:36 – 28:15`, `frame_0012`–`frame_0015`, `frame_0044`, `frame_0046`, `frame_0065`)

- Giao diện tích hợp sẵn trình duyệt PR (`Summary` / `Timeline` / `Code` diff cho 82 files `+1,389 -697` trên PR `#480 feat(billing): make AI quota cost-weighted in credits`, `frame_0012`–`frame_0014`).
- **Tự động đăng ký theo dõi PR (`Waiting on 4 monitors: Watching pull request #477, Watching pull request #478...`, `frame_0011`, `frame_0065`):** Ngay khi agent tạo PR, T3 Code tự động lắng nghe trạng thái CI checks và comment từ các bot (như `github_bugbot`) để tự động sửa lỗi và phản hồi mà không cần con người phải copy-paste lỗi vào khung chat.
- Hỗ trợ gộp hàng loạt PRs: Trong `cargomarshal / Compare PRD With Application` (`frame_0044`, `frame_0046`), chỉ với một câu *"ah go ahead and merge"*, agent tự động merge 5 PRs (`#244`, `#245`, `#243`, `#246`, `#247`) vào `main`, kiểm tra không có xung đột và báo cáo trạng thái migration `pg_trgm`.

### 5.3 Standardized Visualizations Xuyên Mọi Harness (`28:43 – 30:18`, `frame_0069`–`frame_0072`)

- Trong dự án `final-cut-pro / Build a Reusable Facecam Effect`, Jan yêu cầu agent giải thích cơ chế hoạt động của hiệu ứng tự động zoom mặt (`DetectFaceRectanglesRequest` → `Smooth` → `Write FCPXML` → `Import`).
- T3 Code kết xuất ngay trong luồng chat một bảng điều khiển đồ họa tương tác (`frame_0069`–`frame_0072`):
  - Có thanh trượt chỉnh trực tiếp `Face Point (0.38, 0.42)`, `Size`, `Top/Right Margin (48px)`, `Shape (Rounded square)`, `Auto Zoom (160%, Ease In/Out 12/12 fr)` kèm biểu đồ đường cong `Zoom over time` (`ease in` → `hold` → `ease out`) và biểu đồ khử nhiễu chuyển động đầu 60 giây (`raw detection jittery` vs `smoothed path` vs `dead zone`).
- **Giá trị cốt lõi:** Đây là một **Standardized Primitive** — bất kể bạn đang chạy Claude Code, Codex, Cursor hay Antigravity bên dưới, tất cả đều kết xuất được cùng một chuẩn Visualization tương tác này.

### 5.4 Quick Toggle Hover & Tự Động Tạo Git Worktree (`18:50 – 19:20`, `23:32 – 23:55`)

- **Quick Toggle Hover:** Rê chuột lên bất kỳ thread nào ở sidebar sẽ thấy ngay tên thread, repository, máy thực thi (`Mac Studio` hay `MacBook`), nhánh worktree, model, chuỗi PRs đang mở (4–10 PRs) và cảnh báo `Limited` (màu vàng) nếu chạm trần quota.
- **Workspace = `New worktree`:** Jan khuyên luôn bật tùy chọn tự động tạo Git Worktree mới cho mỗi thread để chạy 5–10 luồng song song trên cùng một repo mà không bao giờ đụng độ code.

*(Ghi chú nhà tài trợ `05:50 – 07:50`, `frame_0032`–`frame_0043`: **Letaido** — công cụ "live coding for marketing" do đội ngũ **Ahrefs** phát triển, tự động kết nối dữ liệu Ahrefs Site/Keywords Explorer để dựng dashboard theo dõi đối thủ, Content freshness, AI visibility 34% và gửi báo cáo SEO hàng tuần vào kênh Slack `#seo-reports`).*

---

## 6. 5 Điểm Yếu Chí Mạng Khiến Jan "Phát Điên" (`14:18 – 18:50`, `24:08 – 26:12`, `30:18 – 30:55`)

| # | Vấn đề trên T3 Code (Nightly) | Mô tả chi tiết & Minh chứng trực quan từ Frames | Đối chiếu với đối thủ |
| :--- | :--- | :--- | :--- |
| **1** | **Không xem được Live Code Diff khi Agent / Sub-agent đang chạy** (`14:18 – 16:28`, `frame_0050`–`frame_0054`) | Sub-agent `Cost-weighted AI quota and USD cost` chạy suốt **`33m 48s`** (`Ran 24 commands, changed 3 files`, viết ~2,000 dòng code bằng skill `pstack:poteto-mode`), nhưng trong màn hình sub-agent hoàn toàn **không hiển thị bất kỳ khung code diff nào** cho đến khi kết thúc. Nhiều lúc sub-agent im lặng 20 phút khiến Jan tưởng bị treo phải hỏi Main Agent. | Trong **Claude Code Desktop** (`Fake Tweet Generator route`, `frame_0053`), người dùng thấy ngay tức thì từng dòng diff (`tweet-card.tsx` `+13 -1`) ngay khi agent vừa sửa xong. |
| **2** | **Sub-agent không tự động Restart sau khi qua mốc Reset Rate Limit** (`16:28 – 16:47`, `frame_0050`, `frame_0056`) | Khi chạm giới hạn phiên (`You've hit your session limit - resets 12:40am Europe/Berlin`, `<synthetic> Failed`, `frame_0056`), qua giờ reset thì **Main Agent tự chạy lại**, nhưng **Sub-agent vẫn nằm chết**, buộc người dùng phải bấm thủ công. | Cần cơ chế tự động đánh thức toàn bộ cây sub-agents khi quota hồi phục. |
| **3** | **Sidebar phẳng, thiếu hoàn toàn Folder Grouping theo Dự Án** (`16:48 – 18:50`, `frame_0057`–`frame_0061`) | Toàn bộ các threads từ nhiều repos (`syntaxkit-final`, `cargo-new`, `syntax-path-homepage-new`, `fable-figma`) bị đổ chung vào một danh sách phẳng theo thời gian. Muốn tạo thread mới phải bấm nút `+` rồi chọn lại repo từ đầu. | Cả **Claude Code Desktop**, **Codex** (`frame_0057`–`frame_0058`) và **Cursor** (`frame_0059`) đều gom nhóm các sessions gọn gàng vào từng thư mục dự án (`fable-landing-page`, `cargo-new`, `MarshalDesk`) kèm nút `+` tạo nhanh ngay trên tên thư mục. |
| **4** | **Tính năng SnapShots & Design Mode Screenshot hay bị lỗi trên bản Nightly** (`24:08 – 26:12`, `frame_0064`, `frame_0066`–`frame_0067`) | T3 Code có tính năng **SnapShots** (`Settings -> SnapShots`, nhấn đúp `⌘⌘` để chụp cửa sổ kèm âm thanh `Whoosh`, `frame_0064`) và thanh công cụ **Design Mode** (`Select / Region / Draw / Erase` trên trình duyệt `http://localhost:3000/en`, `frame_0066`–`frame_0067`). Tuy nhiên trên bản Nightly, ảnh chụp đính kèm bị mờ nhòe (`blurry`) hoặc không tải được. Ngoài ra phím tắt `⌘+` để Zoom In đôi lúc không ăn, phải vào menu `View -> Zoom In` (`frame_0070`). | Trên **Codex Desktop App**, tính năng chụp nhanh và khoanh vùng hoạt động ổn định hơn. |
| **5** | **Không hiển thị được Native Primitives riêng của từng Harness (như Claude Artifacts)** (`30:18 – 30:55`) | Dù chạy `Claude Code` harness bên dưới, khi agent tạo một **Claude Artifact**, T3 Code báo lỗi không thể hiển thị Artifact đó trong giao diện. | Xem được đầy đủ trong ứng dụng Claude chính chủ. |

---

## 7. Tổng Kết & Lời Khuyên Thực Chiến Từ Jan Marshal (`31:00 – 32:18`)

- **Mức độ gắn bó:** Sau 6 ngày sử dụng liên tục trên cả iPhone, MacBook và Mac Studio, Jan dành **60% thời gian code qua T3 Connect** (ngồi trên MacBook điều khiển phiên chạy trên Mac Studio mà không gặp bất kỳ lỗi rớt mạng nào) và chuẩn bị mua thêm một chiếc **Mac mini** làm máy chủ chạy agent 24/7.
- **Cấu hình khuyến nghị:**
  1. Tải bản **T3 Code Nightly** (`t3.codes/download?channel=nightly`) để có **Orchestrator V2**.
  2. Giữ nguyên giao diện mặc định **T3 Code Dark Theme** (`frame_0063`).
  3. Vào `Settings -> Workspace` đặt mặc định là **`New worktree`** cho mỗi thread mới.
  4. Liên kết đủ bộ 3 gói thuê bao (**Claude Code + Codex + Cursor**) để xoay vòng `Context Handoff` bất tận mỗi khi chạm trần rate limit 5 giờ.

---

## 8. Liên Kết Chéo Trong Video Learning Vault

- [T3 Code Giải Thích Toàn Diện Cho Người Không Thích Terminal (`DzXAsObvMio`)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/t3_code_徹底解説_aimy.md) — Góc nhìn đào tạo từ Nhật Bản (Imanyu & J-kun) với demo Python/Remotion.
- [Bạn Đang Dùng AI Agents Sai Cách — Theo t3.gg (`q1D90-uGvBg`)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/youre_using_ai_agents_wrong_theo_t3.md) — Quy trình thực tế của chính tác giả T3 Code đạt 50+ PRs/tuần.
- [Pstack: Cách Kỹ Sư Cursor Ship Code Với AI (`4wBVuj2an8E`)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/pstack_how_cursor_engineer_ships_code_eric_tech.md) — Giải phẫu bộ kỹ năng `pstack` (`poteto-mode`) mà Jan Marshal sử dụng trong video này.
- [Setup AI Coding Ship 30+ PRs/Ngày (`54D1hFfaju8`)](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/my_new_ai_coding_setup_ship_30_prs_a_day.md) — Một kiến trúc kết hợp đa công cụ khác cùng sử dụng `pstack`.
