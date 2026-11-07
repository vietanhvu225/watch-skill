# I Built An Online Multiplayer Game With Gemini 4 ARGON (Backyard Rascals Case Study)

- **Source URL:** https://www.youtube.com/watch?v=sRxBt6uDeLQ
- **Channel:** Chong-U — AI Oriented Dev (`@chongdashu` / `vibegamedev.com`)
- **Duration:** 22:22
- **Watch Skill ID:** `d2465dd3f35670b1`
- **Date Processed:** 2026-11-07
- **Tags:** #gemini-4-argon #game-dev #multiplayer #rust-wasm #webgpu #colyseus #blender #tripo #cursor #visual-coaxing #netcode

---

## 1. Tổng Quan & Biên Lai Sản Xuất Thực Tế (Build Receipt)

Sau 8 tháng chủ yếu sử dụng các model của Anthropic (**Claude Opus 5.5**) và **Astra** vì dòng Gemini Flash trước đây thiếu chiều sâu cho phát triển game, **Chong-U** (tác giả *Pressure Wash Panic*, *Tidebreaker*, *Haiku Harbour*) đã thử nghiệm mô hình frontier mới nhất của Google DeepMind là **Gemini 4 Argon** (công bố ngày `13/10/2026` bởi **Koray Kavukcuoglu** — SVP & Chief AI Architect tại Google DeepMind, `frame_0006`) để xây dựng hoàn chỉnh tựa game 3D Capture the Flag 3v3 chơi mạng thời gian thực trên trình duyệt mang tên **`Backyard Rascals` (`v0.1.12`)**.

Chỉ chưa đầy 12 giờ sau khi đăng lên X và phát hành trên **Wavedash**, trò chơi đã thu hút hàng trăm người chơi thực tế. Toàn bộ số liệu định lượng của quá trình phát triển được trích xuất tự động trong trang **Build Receipt (`frame_0017`)**:

| Chỉ số thống kê (`frame_0017`) | Giá trị thực đo | Chi tiết kỹ thuật |
| :--- | :--- | :--- |
| **Chi phí Token quy đổi theo API List Price** | **`≈ $207`** | **Cursor: `\$193`** + **Claude Code: `\$14`** + **Metered APIs: `\$2.09`** (chạy thực tế bằng gói thuê bao/subscriptions). |
| **Tổng thời gian Agent hoạt động** | **`12h 23m`** | Tổng thời gian từ prompt đầu tiên đến bản release cuối cùng là **`24h 53m`** (Cursor chạy xuyên suốt; Claude Code chạy hỗ trợ `46 phút` từ mốc `+13h 41m` đến `+14h 27m`). |
| **Số lượng Prompts của người dùng** | **`203 prompts`** | Được lưu nguyên văn trong 38 bản ghi prompt (`prompt records`). |
| **Số lượt gọi Model & Tool** | **`3,795` Model Calls / `11,043` Tool Calls** | Bao gồm chỉnh sửa code, lệnh shell, tìm kiếm, kiểm tra trình duyệt (browser checks) và chạy script Blender. |
| **Tổng lượng Tokens xử lý** | **`1.26B tokens`** | Trong đó **`1.24B tokens`** được đọc lại từ Prompt Cache và **`2.92M tokens`** sinh mới. |
| **Số lượng Sub-Agents song song** | **`72 Sub-Agents`** | Điều phối làm việc song song, đạt đỉnh **8 agents chạy trong cùng 1 phút**. |
| **Quy mô mã nguồn & Phát hành** | **`89 Commits` / `20 Releases`** | Thêm mới **`53,000 dòng code`** (53K LOC) sau khi import template; gồm **13 bản build Wavedash** (tới `v0.1.12`) và **7 lần deploy landing page**. |

---

## 2. Kiến Trúc Engine No-Engine & Dây Chuyền Đa Tác Nhân Song Song

### 2.1. Khung Khởi Đầu `VGD WASM Game Template (3D Starter) v2.1.0` (`frame_0020`, `frame_0023`)
Thay vì dùng Unity hay Unreal nặng nề, Chong-U khởi tạo dự án từ bộ khung **`vgd-wasm-game-template` (`v2.1.0`)** chia sẻ tại `vibegamedev.com`:
- **`src/` và `crates/engine/`:** Engine trung lập viết bằng **Rust biên dịch ra WebAssembly (WASM)** với xung nhịp mô phỏng cố định **120 Hz step-clock**, giao tiếp C ABI, trình kết xuất trực tiếp **WebGPU / WebGL 2**, hệ thống UI/flow, âm thanh, lưu trữ và bộ kiểm thử tự động (**smoke harness**: `47 Rust tests · 154 game tests · smoke webgl2 60 fps · 0 errors` — `frame_0034`).
- **`game/` và `addons/starter-arena/`:** Chứa logic game hiện tại và bản demo có thể chạy ngay qua `GAME_DIR=addons/starter-arena npm run dev`.
- **Quy trình 2 cổng kiểm duyệt (Two-Review Collaborative Pipeline) & Khóa Kỹ Năng (`skills.lock.json`):**
  1. **Review 1:** Chốt bản mô tả ngắn (*Brief* tại `game/docs/brief-and-direction.md`) và sinh **4 bản mockup giao diện gameplay** để chọn hướng nghệ thuật.
  2. **Review 2:** Xây dựng lát cắt dọc chơi được hoàn chỉnh (*Vertical Slice*: `blockout` + hợp đồng tài nguyên `asset/runtime contracts` + tích hợp asset + kiểm chứng điều khiển thực tế).
  - Các kỹ năng chuyên biệt của dự án nằm trong `.agents/skills/` (symlink sang `.claude/skills`) và được khóa chặt phiên bản bằng `python3 scripts/sync_skills.py check`.

### 2.2. Khởi tạo Brief, 3 Vũ Khí Hài Hước & Chạy Song Song 3 Nhánh (`[03:28]` – `[09:34]`, `frame_0024`–`frame_0031`)
- **Opening Prompt & Kết quả Review 1 (`frame_0024`, `frame_0025`):**
  - Chế độ đấu **3v3 Capture the Flag** trong sân sau nhà (*Backyard*), mỗi hiệp **75–90 giây**, đội nào cướp được **3 cờ trước** sẽ thắng, chạm vào cờ đội nhà bị rơi để trả về ngay lập tức (*instant touch-return*). Hỗ trợ cả Desktop (`WASD`/Arrows/Gamepad) lẫn Mobile (`Touch Landscape split-thumb`).
  - **Bộ 3 vũ khí Slapstick (`frame_0024`):** (1) **Wooden Melee Stick** (gậy gỗ cận chiến), (2) **Plunger Bow & Arrow** (cung bắn cây thụt bồn cầu dính vào đầu đối thủ), và (3) **Suds Bomb / Soapy Water Balloon** (bom bóng nước xà phòng tạo vũng trơn trượt).
  - Dùng model **`fal-ai/nano-banana-2`** (`1376x768` 16:9, chi phí `\$0.08/ảnh` $\rightarrow$ `\$0.32` cho 4 mockups) để chọn góc nhìn **Direction B — Over-the-Shoulder Chase Brawl (`28° Third-Person Action Camera`)** (`frame_0025`).
- **Phân tách 3 nhánh chạy song song bằng Sub-Agents (`[05:36]` – `[08:49]`):**
  1. **Nhánh 1 — Turnaround Sheets & Character 3D Meshes (`frame_0026`–`frame_0029`):** Ra lệnh cho nhiều Sub-Agents gọi **`GPT Image 2.5 Flare`** sinh bảng xoay 4 góc (**Front, Side, Back, 3/4 Profile**) cho 6 nhân vật thú 2 chân (`Fox`, `Otter`, `Badger`, `Squirrel`, `Raccoon`, `Opossum`), đẩy sang **Tripo AI** tạo mesh 3D, rồi đưa vào **Blender** gắn chung một bộ khung xương (*Shared Skeleton/Armature*). Kiểm tra toàn bộ 6 nhân vật trong công cụ nội bộ **`Model preview`** (`assets/characters/character_test_scene.glb` — `41,009 tris · 78 primitives · 72 materials · 152 draws/frame` — `frame_0027`).
  2. **Nhánh 2 — Isolated Props & Symmetric Floor Plans (`frame_0028`–`frame_0030`):** Sinh bảng tham chiếu đạo cụ sân vườn độc lập (`Backyard props sheet`: bồn trồng cà rốt, xe kéo đỏ, cuộn vòi nước, thùng nhựa xanh, chuồng chó, xô cắm cờ) và bản đồ mặt bằng có tính đối xứng, chiều sâu và độ cao (`Backyard arena ref`).
  3. **Nhánh 3 (Main Chat) — Graybox Prototype (`frame_0030`, `frame_0031`):** Giữ luồng chat chính tập trung dựng bản **Graybox** bằng Rust WASM + WebGL2 (`1280x720`, `60 fps`) không cần đợi đồ họa để kiểm chứng cảm giác di chuyển, khoảng cách giữa 2 căn cứ và nhịp độ cướp cờ.

---

## 3. Khắc Phục "The Taste Gap" Của Gemini 4 Argon Qua 9 Bước Tiến Hóa Thị Giác

### 3.1. Chẩn đoán "Khoảng trống gu thẩm mỹ" (The Taste Gap) (`[09:39]` – `[11:55]`)
Khi để **Gemini 4 Argon** tự dựng cảnh 3D đầu tiên từ bản Graybox, kết quả rất kém thẩm mỹ so với **Claude Opus 5.5**: màu xanh lá cây chói lọi (*oversaturated*), mất hoàn toàn ánh sáng và đổ bóng do cơ chế `dev-tuning` và `engine.apply()` ghi đè lên `game.style` sau khi tải cảnh (`frame_0034`), bụi cây chỉ là những khối hộp xanh phẳng lì (`Hedges as green boxes` — `frame_0015`), mặt nước là một vệt sơn phẳng trên cỏ, và phía sau hàng rào là bầu trời trống rỗng.

### 3.2. Lộ trình 9 bước nâng cấp đồ họa & Kỹ thuật "Visual Coaxing" (`frame_0007`, `frame_0032`–`frame_0063`)
Chong-U phát hiện ra rằng nhắc Argon sửa đồ họa bằng câu chữ trừu tượng (*"làm ánh sáng đẹp hơn"*) không hiệu quả; **chìa khóa mở khóa Argon là dùng ảnh chụp màn hình thực tế của game đè lên bởi các biến thể sinh từ `GPT Image 2.5` để làm đích ngắm trực quan (Visual Coaxing)**:

1. **Bước 1–3 (`Graybox` $\rightarrow$ `First 3D, unlit` $\rightarrow$ `Lighting back on` — `frame_0032`–`frame_0038`):** Khôi phục **Ground AO**, **Room AO**, và ngăn xếp **Post-FX** với bóng đổ ánh tím lạnh (*cool blue-violet shadow fill*); vẽ lại texture bãi cỏ và atlas `1024²` trong Blender (`Roughness >= 0.85`); bổ sung hiệu ứng hạt thời gian thực (**Live VFX**: phấn hoa bay, vòi phun nước, bọt xà phòng, chữ nổ kiểu truyện tranh `BONK!`).
2. **Bước 4 (`Neighbourhood skybox` — `frame_0044`):** Yêu cầu thêm dãy nhà và đồi cây bao quanh bên ngoài hàng rào để sân vườn không còn lơ lửng giữa hư không, đồng thời áp dụng **Bump Mapping** tạo độ sần cho gỗ, đá và tường nhà.
3. **Bước 5 (`Cover in the middle` — `frame_0045`–`frame_0047`):** Yêu cầu Sub-Agent dùng `GPT Image 2.5` thiết kế lại mặt bằng có vật chắn tầm nhìn ở khu vực giữa sân (bụi cây, giàn gỗ pergola, chồng thùng gỗ) để hai đội không nhìn xuyên thẳng sang căn cứ của nhau, đồng thời tăng độ dày viền đen (*ink outlines*) quanh nhân vật để dễ đọc trạng thái.
4. **Bước 6 (`First toy-diorama pass` — `frame_0048`–`frame_0050`):** Lấy ảnh chụp màn hình thực tế trong game và bảo `GPT Image 2.5` vẽ đè 4 hướng nghệ thuật (`frame_0050`):
   - `1. Storybook gouache`
   - `2. Comic halftone` (`frame_0048`)
   - **`3. Tactile toy diorama [Picked]`** (`frame_0049`)
   - `4. Cozy sunset`
   - Nạp bức ảnh số 3 và số 4 làm **Target Image** để Argon tự động thêm hiệu ứng xóa phông theo chiều sâu (*depth-of-field blur*), ánh sáng ấm và chất liệu mô hình đồ chơi.
5. **Bước 7 (`Creek and topiary` — `frame_0051`, `frame_0056`–`frame_0063`):**
   - **Cây và bụi rậm (`Talking point 2 of 6` — `frame_0056`–`frame_0059`):** Chất vấn Argon *"Are you creating them in Blender properly?"* để thay thế mẫu cây bị kéo giãn lặp đi lặp lại bằng **3 loài cây riêng biệt (`oak`, `pine`, `blossom`)** được ghép từ các cụm lá lồi lõm (**lumpy leaf clusters**) thay vì khối cầu nhẵn.
   - **Mặt nước và hiệu ứng nổ bóng nước (`Talking point 3 of 6` — `frame_0060`–`frame_0063`):** Đào lòng suối lõm xuống có sỏi đá dưới đáy và mặt nước trong suốt có sóng chuyển động; nâng cấp vụ nổ bóng nước từ 2 hình tròn phẳng lì thành **sóng nước 3D, 108 hạt nước bắn tung tóe, nền đất ướt sẫm màu và vũng nước chảy bám theo độ dốc của cầu gỗ**.
6. **Bước 8–9 (`The overhaul` $\rightarrow$ `Final` — `frame_0051`–`frame_0054`):** Dựng phiên bản đại tu chạy song song ở cổng `:5175` cạnh bản cũ `:5173` (`BASELINE (:5173)` vs `OVERHAUL (:5175)` — `frame_0052`) để so sánh trực tiếp mặt cỏ cắt tỉa kẻ sọc (*striped manicured lawn*), tán cây xốp và bộ lông nhân vật dạng nỉ (*felt-like fur*) cùng sương mù nhẹ. Tổng thể sân đấu hoàn chỉnh đạt `261,762 tris · 130 primitives · 54 materials · 259 draws/frame` (`frame_0035`, `frame_0062`).

---

## 4. Xử Lý Lỗi Gắn Kết Xương (4×4 Bone Sockets) & Hoạt Cảnh Cường Điệu

### 4.1. Giải quyết triệt để lỗi Socket bằng Test Scene chuyên biệt (`[16:32]` – `[17:19]`, `frame_0067`–`frame_0069`)
- **Hiện tượng lỗi (`Before`):** Nhân vật không thực sự cầm vũ khí — gậy gỗ chỉ lấy vị trí tịnh tiến (*translation-only offset*) nên phớt lờ góc xoay cổ tay và bay lơ lửng ngoài nắm đấm; cán cờ bị vẽ lệch một khoảng cố định `cx + fx * 0.38m` trong không gian thế giới (**trôi nổi cách ngực `38 cm`**); bao đeo bóng nước xuyên qua đuôi; cung tên không có hoạt cảnh kéo căng dây.
- **Cách khắc phục (`After` — `frame_0067`–`frame_0069`):**
  1. Yêu cầu tạo một **Focused Test Scene** hoàn toàn trống (chỉ có nền đen và nhân vật) để kiểm tra từng khe gắn vũ khí.
  2. Chuyển sang gắn kết theo **ma trận xương đầy đủ `4x4 socket_hand_R` (Full 4×4 Bone Matrix Parenting)** kết hợp dáng bàn tay nắm chặt (*sculpted paw grip*) và tư thế vác cờ hành quân (*carry march*).
  3. Gắn bóng nước vào xương hông **`socket_hip_L`** và đeo gậy gỗ chéo qua lưng tại **`socket_back`**.

### 4.2. Hoạt cảnh cường điệu (Exaggerated Animations) (`[15:46]` – `[16:31]`, `frame_0009`, `frame_0064`)
- Bổ sung chuyển tiếp mượt (*animation blending*) giữa các động tác, yêu cầu nâng cao lá cờ lên trên đầu khi chạy và cường điệu hóa biên độ vung tay/nhấc chân để nhìn từ xa vẫn nhận diện ngay hành động của từng con thú.

---

## 5. Sức Mạnh Tuyệt Đối Của Argon: Netcode Multiplayer & Các Con Số Tối Ưu Hóa Mạng

Nếu phần mỹ thuật đòi hỏi phải dẫn dắt bằng hình ảnh, thì ở mảng **Lập trình Mạng (Multiplayer Netcode) với Colyseus + Rust WASM**, **Gemini 4 Argon** giải quyết gần như **1-shot** toàn bộ bài toán khó nhất (`[17:22]` – `[20:50]`, `frame_0071`–`frame_0079`):

### 5.1. Hệ thống Phòng chờ (6-Seat Lobby), Bot Backfill & Vòng khởi động (`frame_0071`–`frame_0076`)
- Thiết kế phòng chờ 6 chỗ ngồi trên **Colyseus**: nếu người chơi đợi quá thời gian quy định mà chưa đủ 6 người, **Bots tự động lấp vào các ghế trống (`BOT (REPLACEABLE)`)** để trận đấu bắt đầu ngay; khi có người chơi thật mới vào giữa trận, họ sẽ **tiếp quản thẳng vị trí của một con Bot**.
- Trong lúc chờ ghép trận, người chơi được chạy nhảy khởi động tự do trên phần sân nhà của mình (ngăn cách bởi một hàng rào lực *force field* màu xanh — `frame_0073`), và khi đủ người sẽ phát đoạn cắt cảnh camera bay qua căn cứ hai đội đếm ngược `3... 2... 1... Capture the Flag!`.

### 5.2. Năm kỹ thuật tối ưu hóa Netcode định lượng (`IN NUMBERS` — `frame_0077`–`frame_0079`)
Khi chỉ cần giao một prompt yêu cầu kiểm toán và tối ưu hóa netcode chống giật cục (*rubber banding*), Argon đã triển khai trọn gói 5 cơ chế với thông số cụ thể:

| Vấn đề mạng (`Before`) | Giải pháp kỹ thuật của Argon (`After`) | Thông số định lượng (`frame_0077`–`frame_0079`) |
| :--- | :--- | :--- |
| **1. Giật lùi nhân vật (Rubber Banding):** Mỗi bản tin cập nhật từ server (trễ `~50 ms`) lại kéo giật người chơi lùi về phía sau 1/4 quãng đường. | **Client-Side Prediction & Input Replay:** Trình duyệt cho nhân vật di chuyển ngay lập tức; khi gói tin server tới, engine tua lại điểm đó và phát lại toàn bộ phím đã bấm. | Lưu bộ đệm **`96 frames`** lịch sử phím bấm; làm mượt sai số trong **`~38 ms`** thay vì giật cục (*snap*). |
| **2. Đối thủ di chuyển khựng cục (`stop, start, stop`):** Giữa các gói tin, người chơi khác phanh gấp rồi nhảy cóc tới vị trí mới. | **Entity Extrapolation / Glide:** Giữa các lần cập nhật, đối thủ, cú nhảy và cả bóng nước đang bay tiếp tục trượt theo vận tốc hiện tại. | Chuyển động nội suy/ngoại suy liên tục trên xung nhịp `120 Hz`. |
| **3. Lệch tâm ngắm bắn (Aim Drift):** Gửi độ lệch chuột tương đối (*"I moved the mouse a bit"*) khiến góc nhìn trên màn hình (xanh) và server (cam đứt nét) lệch nhau. | **Absolute Aim Synchronization:** Đổi sang gửi hướng ngắm tuyệt đối (*"I'm pointing here"*) để client và server luôn trùng khớp 100%. | Độ lệch góc ngắm về **`0°`**. |
| **4. Lặp âm thanh & hiệu ứng (`JUMP! JUMP!` Echo):** Trình duyệt phát tiếng nhảy ngay khi bấm, rồi 50ms sau lại phát lần nữa khi bản sao từ server dội về. | **Self-Echo Suppression:** Tự động bỏ qua tiếng vọng sự kiện của chính người chơi từ server gửi về. | Triệt tiêu hoàn toàn lỗi nhân đôi SFX/VFX sau `50 ms`. |
| **5. Lãng phí băng thông (Bandwidth Bloat):** Gửi **12 số thập phân dài (floats) ở mọi khung hình (`120+ gói tin/giây`)**, kể cả khi đứng yên. | **Data Quantization & Idle Suppression:** Lượng tử hóa thành số nguyên nhỏ, giới hạn tốc độ gửi và ngắt gửi khi đứng yên. | Chỉ còn **`7 số nguyên nhỏ / bản tin`**, tối đa **`≤ 60 bản tin/giây`**, và **`0 bản tin`** khi đứng yên. |

### 5.3. Ba lỗi thực tế phát hiện qua Playtesting trực tuyến & Cân bằng bằng Jev (`frame_0079`)
1. **Lỗi `"Play again" froze you`:** Rời trận cũ kết thúc trễ hơn một tích tắc sau khi vòng khởi động trận mới đã bắt đầu nên làm tắt điều khiển $\rightarrow$ sửa lại thứ tự dọn dẹp trận cũ trước khi khởi tạo trận mới.
2. **Lỗi `Two tabs couldn't find each other`:** Mở 2 tab trên cùng máy tự tạo 2 phòng riêng do thiếu địa chỉ server $\rightarrow$ kết nối qua liên kết player-to-player của Wavedash.
3. **Lỗi `Hidden from Trending` & Thời gian chờ ghép trận (Jev analysis):** Phân tích thống kê playtest bằng **Jev** cho thấy game mất trung bình **`12 giây`** để tải xong và vào tới phòng chờ (thời점 người chơi thật vào phòng lần lượt ở `10.7s, 11.5s, 12s, 15.6s, 19s, 24.9s`), nên mức chờ `10 giây` ban đầu là quá ngắn khiến máy chủ vội nhét đầy Bot trước khi người thật kịp tải xong! Đồng thời chỉ khởi tạo phòng khi người chơi bấm *Quickplay* và đếm số người chơi thực trên các máy khác nhau để hiện đúng lên mục *Trending*.

---

## 6. Bảng Đánh Giá Tổng Kết: Gemini 4 Argon vs. Claude Opus 5.5

| Tiêu chí đối chiếu | Gemini 4 Argon (Google DeepMind) | Claude Opus 5.5 (Anthropic) |
| :--- | :--- | :--- |
| **Gu thẩm mỹ tự thân (Inherent Taste)** | **Cần dẫn dắt nhiều:** Bản đầu tiên thường chói, phẳng, thiếu chiều sâu; bắt buộc phải dùng kỹ thuật **Visual Coaxing** (sinh ảnh mẫu bằng `GPT Image 2.5` để làm đích ngắm). | **Xuất sắc nhất hiện tại:** Tự động phối màu, ánh sáng, đổ bóng và không khí (*ambiance*) có hồn ngay từ những lượt đầu (như tựa game platformer 2.5D *Tidebreaker* ở `frame_0036`, `frame_0090`–`frame_0091`). |
| **Giải quyết bài toán kỹ thuật nặng & Netcode** | **Đỉnh cao (1-Shot):** Tự thiết kế và tối ưu hóa trọn gói phòng chờ Colyseus, Bot backfill, Client-Side Prediction (`96 frames`), nội suy `~38 ms` và nén lượng tử hóa (`12 floats @ 120Hz` $\rightarrow$ `7 ints @ ≤60Hz`). | Tốt, nhưng với các hệ thống mạng phức tạp đôi khi cần nhiều vòng tinh chỉnh hơn. |
| **Làm việc với Blender & 3D Rigging** | Hiểu sâu các kỹ thuật nâng cao trong Blender (cụm lá *leaf clusters*, lòng suối lõm, ma trận xương `4x4 socket_hand_R`) một khi được chỉ rõ lỗi thông qua **Test Scene**. | Rất tốt và ổn định. |
| **Vai trò khuyến nghị trong Pipeline** | Giao phụ trách **Core Engine (Rust/WASM/WebGPU), Multiplayer Netcode, Tối ưu hóa hiệu năng/băng thông** kết hợp quy trình **Visual Coaxing**. | Giao phụ trách **Art Direction, Look-Dev, Shaders thẩm mỹ và thiết kế cảm xúc thị giác**. |
