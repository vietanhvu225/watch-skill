# 🧩 9 NEW Claude Mods That Can Truly Change How You Work — Kiến Trúc Phần Mềm Tự Biến Hình (Malleable Software) Trong Claude Code

- **Source:** https://www.youtube.com/watch?v=lDrAZ1wAyVs
- **Watch Skill ID:** `c8467449c524ea4a`
- **Uploader:** Jay E | RoboNuggets (Jay Enriquez — Founder RoboLabs & Cộng đồng RoboNuggets, Official Member of Claude Partner Network)
- **Duration:** 12:47 (767s)
- **Date Processed:** 2026-11-07
- **Category:** #ai-agents #claude-code #claude-mods #malleable-software #prompt-cache #agent-harness #developer-tools

---

## Executive Summary: Cú Hích "Malleable Software" Đưa Claude Code Thành Môi Trường Tự Biến Hình Tuyệt Đối

Vào ngày **01/10/2026**, Anthropic đã tung ra một trong những bản cập nhật lớn nhất lịch sử **Claude Code** (yêu cầu từ phiên bản `v2.1.287+`, hoạt động trên **Claude Code Terminal CLI** và tab **Code của Claude Desktop App**, chưa hỗ trợ trên VS Code Extension tại thời điểm quay): **Hệ thống MODS**.

Ý tưởng cốt lõi mang tính bước ngoặt của ngành phần mềm: **Nếu bạn có thể mô tả một tính năng mà bạn ước Claude Code sở hữu, Claude Code giờ đây có thể tự lập trình tính năng đó vào chính bản thân nó** nhờ kỹ năng dựng sẵn **`plugin-authoring`** và cơ chế **Hot-Reloading** ngay trong phiên. Đây là hiện thân thương mại đầu tiên của khái niệm **"Malleable Software" (Phần mềm tự biến hình)** từ một phòng lab AI hàng đầu.

```
+----------------------------------------------------------------------------------------------------+
|               PHÂN ĐỊNH BA TẦNG MỞ RỘNG TRONG HỆ SINH THÁI CLAUDE CODE                             |
+----------------------------------------------------------------------------------------------------+
|                                                                                                    |
|  [ 1. SKILLS (SỔ TAY CÔNG TÁC — HANDBOOK) ]                                                        |
|    • Bản chất: Tập tin Markdown hướng dẫn Claude *cách thức* thực hiện một công việc cụ thể.       |
|    • Phạm vi: Nằm trong không gian giải pháp của bài toán; không thay đổi giao diện hay runtime.    |
|                                                                                                    |
|  [ 2. MCP CONNECTORS (CẦU NỐI NGOẠI VI) ]                                                          |
|    • Bản chất: Cắm Claude Code vào một ứng dụng hoặc dịch vụ bên ngoài (Database, Slack, GitHub).  |
|    • Phạm vi: Mở rộng dữ liệu và công cụ sang hệ thống thứ ba.                                     |
|                                                                                                    |
|  [ 3. CLAUDE MODS (BIẾN HÌNH BẢN THÂN ENGINE) ]                                                    |
|    • Bản chất: Add-on chạy BÊN TRONG Claude Code, lắng nghe Event Bus và thay đổi trực tiếp UI/UX. |
|    • Khả năng: Thêm nút bấm, mở bảng điều khiển phụ (Side Panel), đổi giao diện, chặn lệnh nguy    |
|      hiểm, giữ ấm Prompt Cache, nhúng trình duyệt web vào Terminal, hoặc gắn cả mini-game.          |
|                                                                                                    |
+----------------------------------------------------------------------------------------------------+
```

---

## 1. Kiến Trúc Hướng Sự Kiện Của Claude Mods (Event Bus: Watch · Change · Handle)

Làm thế nào một bản Mod có thể thay đổi trực tiếp những gì xuất hiện trên màn hình của bạn? (`[03:02]` — Frame `0023`)

Mỗi khi Claude Code thực hiện một thao tác (chạy lệnh shell, khởi tạo helper sub-agent, gọi model, sửa file), hệ thống sẽ phát ra một tín hiệu nội bộ gọi là **Event (Sự kiện)**. Một bản Mod là một đoạn mã nhỏ lắng nghe các sự kiện đó và thực hiện **1 trong 3 hành động (hoặc kết hợp cả 3)**:

```
                               [ CLAUDE CODE RUNTIME ]
                                          |
                                 (Phát tín hiệu Event)
                                          v
                          +-------------------------------+
                          |   EVENT BUS (HỆ THỐNG SỰ KIỆN) |
                          +-------------------------------+
                                          |
                +-------------------------+-------------------------+
                |                         |                         |
                v                         v                         v
      [ 1. 👁 WATCH (THEO DÕI) ]  [ 2. ✏️ CHANGE (THAY ĐỔI) ]  [ 3. ✋ HANDLE (XỬ LÝ/CHẶN) ]
      - Lắng nghe khi helper      - Vẽ lại khu vực phía trên   - Tự đứng ra vẽ toàn bộ bảng
        được sinh ra & gọi LLM.     ô nhập prompt, chèn thanh    điều khiển hoặc chặn lệnh
      - Ghi nhận Model, số          Progress Bar, đổi màu        nguy hiểm chờ người dùng
        Tokens và ước tính Cost.    hiển thị của Tool Call.      bấm nút xác nhận.
```

* **Kinh tế học Token (Deterministic vs LLM-backed Mods):** Đa số các bản Mod chạy bằng **mã tất định (Deterministic Code — TypeScript)** nên tốn **0 token LLM** khi vận hành (như `File Tree`, `Claude Skins`, `Cache Tax`, `Blast Radius`, `Replay Theater`). Chỉ những Mod cần một Agent thứ hai đọc hiểu ngữ nghĩa trong nền (như `You Should Know`) mới tiêu tốn thêm hạn mức Claude của bạn.

---

## 2. Giải Phẫu Chi Tiết 9 Bản Mod Đáng Giá Nhất (Kèm Bằng Chứng Thực Chiến Trên Khung Hình)

### 🦀 Mod 1: Savvy Progress — Giám Sát Đội Ngũ Sub-Agents & Tùy Biến Linh Vật Pixel (`[02:00]` — Frames `0016`..`0023`)
* **Bài toán:** Khi chạy các tác vụ dài hạn có nhiều Sub-agents (những *"helper Claudes"* chia nhau từng phần việc), màn hình mặc định rất khó theo dõi xem từng tác nhân phụ đang dùng model gì, tiến độ tới đâu và đã đốt bao nhiêu tiền.
* **Cơ chế hoạt động (Sử dụng cả 3 quyền `Watch` + `Change` + `Handle`):**
  1. **Thanh tiến độ ngay trên khung chat:** Hiển thị trạng thái tổng thể, ví dụ: `● Launch kit | Tasks 2/4 | 50% | 🦀 ×4` (hoặc `Review, 3/4 | 75%`).
  2. **Bảng điều khiển `Agents` trực tiếp bên phải (Frame `0020`):** Trong bài test chạy lệnh `/savvy-flow:savvy-flow Build the launch kit for our new cold brew...`, bảng điều khiển hiển thị tổng chi phí `Cost: ≈\$0.25` (sau đó tăng lên `≈\$0.31`), `Tokens: 254k` (`317k`), `Time: 0:26` (`0:52`) và bóc tách chi tiết 4 Sub-agents:
     * **Running (1):** `Build the landing page` — cấu hình `light Opus 5.5 · low`, tiến độ `3/5 · Hero copy`, `ctx 4% · 38k ≈\$0.05 0:25`.
     * **Finished (2):** `Design the launch graphics` (`medium Opus 5.5 · medium`, `4/4`, `ctx 4% · 39k ≈\$0.09 0:18`) & `Research competitor launches` (`careful Opus 5.5 · high`, `4/4`, `ctx 4% · 44k ≈\$0.11 0:17`).
     * **Planned (1):** `4. Review the whole kit` (`heavy Opus · xhigh · after 1, 2, 3`).
  3. **Tùy biến ngoại hình Pixel Claude Crabs (Frame `0022`):** Bạn có thể yêu cầu Claude thiết kế riêng diện mạo cho từng chú cua mascot theo vai trò: Cua đội mũ thám tử Sherlock cầm kính lúp (Researcher), Cua đội mũ lưỡi trai xanh cầm cờ caro, Cua đội mũ nồi đỏ cầm cọ vẽ & bảng màu (Designer), và Cua đội mũ bảo hộ vàng cầm cờ lê (Developer/Builder).

### 🎨 Mod 2: Claude Skins — Khoác Áo Mới Cho Giao Diện & Đồng Hồ Đo Hạn Mức (`[03:56]` — Frame `0026`)
* **Bài toán:** Bản Mod đầu tiên an toàn nhất để làm quen vì chỉ thay đổi tầng hiển thị mà không can thiệp vào luồng công việc.
* **Cơ chế hoạt động:**
  * Kho mã nguồn mở đi kèm sẵn **7 bộ giao diện (Themes)** như **`Noir`** và **`Tokyo Night`**, cấp biểu tượng riêng cho từng loại Tool Call (`>_ Rerun tests` $\rightarrow$ `✓ 3 passed`) và tô màu rõ ràng các dòng code thêm/xóa (`return c * 9 / 5 + 32; added`).
  * Hiển thị trực quan **3 vòng tròn hạn mức** ngay dưới câu trả lời acompañada nút `[Compact chat]`: **`5% context used`**, **`20% of 5-hour limit`**, và **`43% of 7-day limit`**.
  * **Tạo Skin từ bảng màu bất kỳ:** Chỉ cần dán ảnh chụp một dải 5 màu (color palette) vào ô chat kèm câu lệnh `"Build me a skin from this..."`, Claude sẽ tự động dựng thành giao diện hoàn chỉnh.

### 💡 Mod 3: You Should Know (`cc-plugin-you-should-know@builtin`) — Tác Nhân Thứ Hai Quét Cảnh Báo Ẩn (`[04:33]` — Frames `0027`..`0029`)
* **Nguồn gốc:** Bản Mod chính chủ do **Anthropic (`@ClaudeDevs`)** phát hành ngày `02/10/2026`. Bật nhanh bằng lệnh:
  ```bash
  /plugin enable cc-plugin-you-should-know@builtin
  ```
* **Bài toán:** Hội chứng **"Bức tường chữ" (Wall of Text)** — người dùng thường lướt qua câu trả lời dài của Claude và bỏ sót các chi tiết sống còn về chi phí API, thay đổi ngầm hoặc rủi ro hệ thống.
* **Bằng chứng thực chiến (Frame `0029`):** Khi Jay yêu cầu Claude tạo video quảng cáo Cold Brew bằng mô hình **Kling** thông qua nhà cung cấp **Kie AI** (`Saves to generations/cold-brew-launch.mp4`), agent thứ hai của `You Should Know` lập tức bóc tách thông tin chi phí bị chôn vùi trong văn bản và bật khung cảnh báo viền vàng nổi bật ngay trên ô nhập lệnh:
  > 💡 **YOU SHOULD KNOW**
  > **Each Kling clip `costs about \$0.45`**
  > **`Kie AI` charges 90 credits for each 5-second clip.**
  > `[ Tell me more ]` · `[ I knew this ]` · `Dismiss`
* **Khả năng tùy biến:** Bạn có thể chỉ đạo riêng cho Mod này tập trung canh gác các rủi ro cụ thể của doanh nghiệp (chi phí API, lỗi ảnh hưởng khách hàng, hoặc các điểm mù trong luồng tự động hóa).

### 🌳 Mod 4: File Tree — Cây Thư Mục Tương Tác Thời Gian Thực Cho Cả Dev Lẫn Non-Dev (`[06:04]` — Frames `0033`..`0035`)
* **Bài toán:** Khi làm việc trên codebase sản phẩm thực tế hoặc thư mục nhiều tài liệu, bạn cần biết chính xác Claude đang đọc, sửa hay lưu tệp tin nào mà không tốn một token nào.
* **Cơ chế hiển thị:**
  * Mở bảng cây thư mục bên phải màn hình: tệp tin đang được Claude chạm vào sẽ **nhấp nháy lấp lánh (shimmer)** kèm nhãn trạng thái (`reading` màu tím, `editing` màu cam), và chuyển sang **màu xanh lá (`+5 -2`)** ngay khi thay đổi được lưu/commit.
  * **Ứng dụng ngoài lập trình (Frame `0035` — Hồ sơ thuế `tax-return-2026`):** Khi chạy lệnh *"Add my work expenses from the receipts to deductions.csv"*, bảng bên phải hiển thị khu vực **`CLAUDE TOUCHED (4 FILES)`** gồm 3 hóa đơn PDF (`desk-chair.pdf`, `laptop.pdf`, `phone-bill.pdf` gắn nhãn `Opened`/`reading`) và tệp `deductions.csv` gắn nhãn `Edited`/`editing` (tự động trích xuất 3 dòng: `Laptop 1899.00`, `Desk chair 429.00`, `Work phone 64.00`).

### 💸 Mod 5: Cache Tax (`karanb192/cache-tax`) & `/keepwarm` — Lá Chắn Chống Đốt Tiền Khi Nguội Prompt Cache (`[07:03]` — Frame `0037`)
* **Nguồn gốc:** Phát triển bởi **Karan Bansal (`github.com/karanb192/cache-tax`)**.
* **Bài toán kinh tế:** **Prompt Cache** là bộ nhớ ngắn hạn giúp Claude không phải đọc lại toàn bộ cuộc hội thoại sau mỗi tin nhắn. Tuy nhiên, sau khoảng **1 giờ** ngừng tương tác trên gói thông thường, cache sẽ bị **nguội (Cold Cache)**. Tin nhắn tiếp theo bạn gửi sẽ bắt Claude nạp lại toàn bộ lịch sử từ đầu, ngốn một lượng lớn token và hạn mức.
* **Cơ chế chặn & so sánh trực quan (Frame `0037`):**
  * Khi Jay quay lại sau `6h38m` và gõ *"Where were we? Give me a 2 line recap."*, hook của `cache-tax` lập tức **giữ tin nhắn lại (`Your message is on hold`)** và hiện thẻ so sánh chi phí chấn động:
    * **Send it now (Cold Cache):** **`\$2.32`** *(ghi lại tới `231,608 tokens` ở mức giá `\$10/MTok`)*.
    * **A warm turn:** **`\$0.12`** *(chi phí thực tế nếu cache còn ấm — rẻ hơn gần **20 lần**!)*.
  * Cung cấp 3 lựa chọn: **`[ Pay and send ]`**, **`[ Start fresh ]`**, hoặc **`Show cache status`**.
  * **Vũ khí `/keepwarm`:** Trước khi rời máy, bạn có thể gõ `/keepwarm` (hoặc bật mặc định) — Mod sẽ tự động ping phiên làm việc bằng một tin nhắn siêu nhỏ ngay trước khi cache kịp nguội (giữ ấm liên tục trong `3h00m`).

### 🛡️ Mod 6: Blast Radius (Anthropic Official) — Cổng Phanh Khẩn Cấp Trước Lệnh Hủy Diệt (`[08:15]` — Frames `0039`..`0041`)
* **Nguồn gốc:** Bản Mod chính chủ do **Anthropic** xây dựng.
* **Bài toán:** Ngay cả các mô hình biên giới thông minh nhất như **Claude Opus** hay **Claude Fable** đôi khi vẫn gặp edge case và chạy các lệnh xóa sổ toàn bộ thư mục dự án (như `rm -rf build` hay xóa nhầm thư mục gốc).
* **Cơ chế bảo vệ:** Khi phát hiện bất kỳ lệnh nào có bán kính sát thương lớn, `Blast Radius` tự động phanh gấp, liệt kê chính xác danh sách từng tệp tin sẽ bị xóa vĩnh viễn và bắt buộc **con người (Human-in-the-loop)** phải tự tay bấm nút **`Proceed`** hoặc **`Cancel`**.

### 🧠 Mod 7: Reflect — Vòng Lặp Micro-RLHF Tự Động Ghi Nhớ Phản Hồi Vào `CLAUDE.md` (`[09:04]`)
* **Bài toán:** Người dùng thường xuyên phải sửa lưng Claude về cùng một lỗi hoặc sở thích cá nhân qua nhiều phiên làm việc.
* **Cơ chế hoạt động:**
  * Lắng nghe các từ khóa phản hồi mang tính hiệu chỉnh từ người dùng như: *"No"*, *"This doesn't look good"*, hoặc *"I wish we'd done it this way"*.
  * Ngay lập tức bật một thanh nhỏ (banner) phía trên ô nhập prompt hỏi xem bạn có muốn lưu bài học đó thành quy tắc vĩnh viễn trong tệp **`CLAUDE.md`** hoặc tệp **Skill** hay không. Chỉ cần bấm **`Save`**, Claude tự động biên tập và ghi quy tắc vào file.

### 🌐 Mod 8: Terminal Browser (`zenbu-labs/terminal-browser`) — Nhúng Trình Duyệt Web Thực Vào Cạnh Khung Chat CLI (`[09:38]` — Frames `0043`..`0046`)
* **Nguồn gốc:** Kho mã nguồn mở **`github.com/zenbu-labs/terminal-browser`** (3k+ stars, minh họa qua PR `#113` của tác giả `RobPruzan`).
* **Bài toán:** Dành riêng cho các kỹ sư làm việc thuần túy trên **Claude Code Terminal** muốn xóa bỏ hoàn toàn việc chuyển đổi cửa sổ (context switching) giữa Terminal và trình duyệt ngoài.
* **Cơ chế hoạt động (Frame `0043`, `0044`):**
  * Chỉ cần gõ lệnh `/browser`, một trình duyệt web thực sự (`terminal-browser://start`) mở ra ngay nửa bên phải của cửa sổ Terminal.
  * Tự động phát hiện danh sách **`Running servers`** đang chạy trên máy (`localhost:3306` docker, `localhost:4747/5173/5174/5177` node, `localhost:5432` postgres, `localhost:6379` redis), các tài liệu gần đây (`README.md`, `/tmp/plan.html`) và cho phép duyệt trực tiếp giao diện web vừa dựng hoặc kiểm tra **GitHub Pull Request** ngay trong Terminal.

### 🎬 Mod 9: Replay Theater (Anthropic Official) — Tua Lại Từng Bước Sửa File Cho Cả Code Lẫn Bài Viết (`[10:08]` — Frames `0047`..`0052`)
* **Nguồn gốc:** Bản Mod mẫu chính chủ của **Anthropic (`@ClaudeDevs`)** ra mắt ngày `02/10/2026`.
* **Cơ chế hoạt động:**
  * Ghi lại toàn bộ lịch sử chỉnh sửa tệp tin của Claude theo đúng trình tự thời gian trong một lượt (turn). Khi hoàn tất, người dùng nhấn nút **Replay** hoặc gõ lệnh **`/replay`** để mở khung trình chiếu bên phải với hai nút điều hướng **`[ Previous edit ]`** và **`[ Next edit ]`**.
  * **Ứng dụng 1 — Review Code (Frame `0049`):** Đổi tên hàm `greet` thành `welcome` qua `4 edits in 4 files (6 lines added, 6 removed)` theo đúng thứ tự: `1. README.md` $\rightarrow$ `2. greet.js` $\rightarrow$ `3. cli.js` $\rightarrow$ `4. index.js`, tô màu chi tiết tới từng từ thay đổi (`Changed word`).
  * **Ứng dụng 2 — Biên tập Nội dung/Bài viết (Frame `0052`):** Yêu cầu *"Tighten my cold brew article. Make the intro punchier and the how-to simpler."* $\rightarrow$ `Replay Theater` hiển thị `3 edits in 1 file (draft.md)` để người viết đối chiếu từng câu văn Claude vừa gọt giũa (`- Cold brew is what a lot of people like.` $\rightarrow$ `+ Cold brew is smoother than iced coffee.`).

---

## 3. Bảng Tổng Hợp 9 Bản Mod Claude Code

| # | Tên Mod | Nguồn / Tác Giả | Tốn Token LLM? | Chức Năng Cốt Lõi & Lệnh Kích Hoạt |
| :--- | :--- | :--- | :---: | :--- |
| **1** | **Savvy Progress** | Community / Custom | Không (Chỉ đo lường) | Thanh tiến độ trên prompt + Bảng `Agents` theo dõi Model/Token/Cost từng Sub-agent + Cua Pixel tùy biến. |
| **2** | **Claude Skins** | Open-Source Repo | Không | 7 themes (`Noir`, `Tokyo Night`...), icon riêng cho Tool Calls, 3 vòng tròn hạn mức, tự tạo skin từ ảnh bảng màu. |
| **3** | **You Should Know** | **Anthropic Official** | **Có** (Chạy Agent thứ 2) | `/plugin enable cc-plugin-you-should-know@builtin` — Quét output của Claude để bật cảnh báo chi phí/rủi ro bị chôn vùi. |
| **4** | **File Tree** | Community / Custom | Không | Cây thư mục bên phải: nhấp nháy khi Claude đang `reading`/`editing`, hóa xanh khi lưu; hữu ích cho cả hồ sơ thuế/tài liệu. |
| **5** | **Cache Tax** | **Karan Bansal** (`karanb192/cache-tax`) | Không | Chặn tin nhắn khi Prompt Cache bị nguội (so sánh `\$2.32` cold vs `\$0.12` warm) + lệnh `/keepwarm` tự ping giữ ấm 3 giờ. |
| **6** | **Blast Radius** | **Anthropic Official** | Không | Phanh khẩn cấp trước các lệnh xóa file nguy hiểm (`rm -rf`), liệt kê danh sách file sẽ mất và bắt bấm `Proceed`/`Cancel`. |
| **7** | **Reflect** | Community / Custom | Rất ít (Khi bấm Save) | Bắt từ khóa phàn nàn (*"no"*, *"doesn't look good"*) $\rightarrow$ hiện banner 1-click lưu thành rule vĩnh viễn trong `CLAUDE.md`. |
| **8** | **Terminal Browser** | **Zenbu Labs** (`zenbu-labs/terminal-browser`) | Không | Lệnh `/browser` mở trình duyệt web thực ngay cạnh khung chat Terminal, tự bắt các cổng `localhost` và xem GitHub PR. |
| **9** | **Replay Theater** | **Anthropic Official** | Không | Lệnh `/replay` tua lại từng bước chỉnh sửa file (`Previous edit` / `Next edit`) cho cả codebase lẫn bản thảo bài viết. |

---

## 4. Quy Trình 4 Bước Tự Tạo & Tinh Chỉnh Mod Với Kỹ Năng `plugin-authoring` (`[10:50]`)

1. **Cập nhật phiên bản:** Đảm bảo Claude Code đạt từ bản **`v2.1.287`** trở lên bằng cách gõ `claude update` trong Terminal (hoặc bấm thông báo cập nhật trên Desktop App).
2. **Mô tả bằng ngôn ngữ tự nhiên:** Mở Claude Code và mô tả tính năng hoặc giao diện bạn muốn thay đổi. Claude Code đã tích hợp sẵn skill **`plugin-authoring`** nên hiểu rõ cấu trúc lắp ghép một bản Mod.
3. **Bật Hot-Reloading cho phiên hiện tại:** Khi Claude hỏi có muốn bật **Hot Reloading** không, hãy xác nhận để phiên làm việc tự khởi động lại nhẹ và hiển thị bản Mod ngay lập tức khi vừa code xong.
4. **Đóng gói thành Plugin vĩnh viễn:** Mặc định các bản Mod do Claude vừa dựng chỉ mang tính **tạm thời (temporary)** trong phiên đó. Khi đã ưng ý sau vài lượt tinh chỉnh bằng lời nói, hãy yêu cầu:
   > *"Save this mod as a plugin and install it permanently."*
   Từ đó về sau, bản Mod sẽ luôn xuất hiện mỗi khi bạn mở Claude Code.
