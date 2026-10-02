# 🎮 Reverse-Engineering Game từ Walkthrough Video: Khảo sát Tính khả thi, Thách thức & Kiến trúc Pipeline

> **Ngày ghi nhận:** 03/10/2026  
> **Trạng thái:** Tạm gác (Deferred) — Ưu tiên hướng tiếp cận Devlog/Creator Breakdown do team phát triển chia sẻ.  
> **Mục tiêu:** Đúc kết phương pháp luận trích xuất Game Logic, State Machine, HUD & Rules từ video gameplay để tái tạo game tương tự.

---

## 1. Bối cảnh & Đặt vấn đề

Khi muốn tái tạo (*clone* hoặc *re-engineer*) một tựa game đã có, thông thường chúng ta đối mặt với hai nguồn tư liệu video trên YouTube:

| Tiêu chí | Hướng 1: Gameplay Walkthrough (Chơi từ đầu đến cuối) | Hướng 2: Creator Devlog / Post-Mortem (Team làm game giải thích) |
|---|---|---|
| **Nguồn tín hiệu chính** | **Visual & Temporal Changes (85-95%)**<br>- HUD / UI status<br>- Frame changes, Hitbox/Collision<br>- Animation & Level progression | **Audio / Speech & Architecture (80-90%)**<br>- Tác giả tự giải thích kiến trúc, logic<br>- Sơ đồ component, code snippet<br>- Các bài toán tối ưu và cạm bẫy đã gặp |
| **Độ phức tạp trích xuất** | Rất cao (Phải suy diễn logic qua thị giác và chuỗi thời gian) | Vừa phải (Tận dụng thế mạnh Whisper + Transcript của `watch-skill`) |
| **Độ chính xác logic** | Suy đoán thống kê (*Inferred / Estimated*) | Chính xác theo thiết kế gốc (*Deterministic*) |
| **Chi phí Token & Compute** | Tốn GPU/Vision token (Phải decode và phân tích nhiều frames) | Rẻ, tối ưu (Chủ yếu xử lý text transcript) |

**Kết luận định hướng:** Ưu tiên số một là tìm kiếm **Creator Breakdown / Devlog** của chính đội ngũ làm game. Phương pháp Walkthrough Reverse-Engineering sẽ được giữ làm phương án bổ trợ khi game không có tài liệu hay devlog công khai.

---

## 2. Thách thức Kỹ thuật của Phương pháp Walkthrough

1. **Vấn đề Dung lượng Video & Bùng nổ Khung hình (Frame Explosion):**
   - Video walkthrough thường kéo dài từ 30 phút đến vài giờ (hàng trăm ngàn frame). Không thể nhồi tất cả vào context của Multimodal LLM.
   - Giải pháp: Áp dụng chiến lược **2 tầng lấy mẫu**:
     - *Sparse Sampling (Lấy mẫu thưa):* 1 frame mỗi 15 - 30 giây để nắm cấu trúc tổng thể màn chơi.
     - *Dense Window Inspection (Lấy mẫu dày):* Khi phát hiện combat, giải đố, nhảy qua chướng ngại vật ➔ Lấy mẫu 2 - 5 fps trong cửa sổ 5 - 10 giây.

2. **Tách biệt HUD / UI Overlay và Trạng thái Thế giới (World State):**
   - **HUD Layer (Cố định):** Thanh máu (HP), mana, điểm số, cooldown chiêu, minimap, inventory. Cần định vị vùng cố định trên màn hình và dùng OCR để theo dõi biến động giá trị.
   - **World Layer (Động):** Nhân vật chính, kẻ thù, projectile, địa hình. Cần nhận diện bounding box và quan hệ va chạm.

3. **Truy nguyên Quan hệ Nhân - Quả (Cause & Effect Inference):**
   - Game logic thực chất là tập hợp các Invariants và Event Handlers. Cần suy diễn:
     - *"Khi nhân vật va chạm với vật thể X tại frame $t$ ➔ Tại frame $t+1$, HP giảm bao nhiêu?"*
     - *"Điều kiện mở cửa / qua màn là gì? (Giết hết quái, thu thập đủ key, hay đạt ngưỡng điểm?)"*

---

## 3. Kiến trúc Pipeline Đề xuất (4 Giai đoạn)

```
+-----------------------------------------------------------------------------------------+
|                  PIPELINE REVERSE-ENGINEERING GAME TỪ WALKTHROUGH                      |
+-----------------------------------------------------------------------------------------+
|                                                                                         |
| [ GIAI ĐOẠN 1: Macro Game Loop & Cấu trúc màn chơi ]                                   |
|   - Quét video bằng Sparse Sampling (1 frame / 15-30s).                                 |
|   - Xác định: Thể loại (2D/3D, Platformer, RPG, Puzzle, Arcade, v.v.).                 |
|   - Phân đoạn: Title Screen -> Tutorial -> Level 1..N -> Boss -> Game Over / Win.       |
|                                                                                         |
| [ GIAI ĐOẠN 2: Bóc tách Mechanics & HUD State ]                                         |
|   - Crop & OCR các vùng UI cố định: Điểm số, HP, Mana, Ammo.                            |
|   - Suy đoán Input Mapping: Phím di chuyển, nhảy, dash, tấn công, tương tác.            |
|   - Ước lượng tham số vật lý: Gia tốc trọng lực, vận tốc nhảy, ma sát mặt sàn.         |
|                                                                                         |
| [ GIAI ĐOẠN 3: Xây dựng Reverse GDD (Game Design Doc) & State Machine ]                 |
|   - Player Finite State Machine (Idle, Run, Jump, Fall, Attack, Hurt, Die).             |
|   - Enemy AI State Machine (Patrol, Chase, Attack, Flee, Cooldown).                     |
|   - Core Gameplay Loop: Input -> Action -> Visual Feedback -> Reward/Score.             |
|   - Win / Loss Conditions và Level Progression Rules.                                   |
|                                                                                         |
| [ GIAI ĐOẠN 4: Chuyển hóa thành Code Nguyên Mẫu (Prototyping) ]                         |
|   - Dựng Gray Box Gym / Testbed để thử nghiệm các cảm giác điều khiển (Game Feel).      |
|   - Tận dụng `godot-ai` MCP Server để sinh cấu trúc Scene/Node trong Godot Engine,      |
|     hoặc scaffold Web Game (Phaser.js / Three.js / Canvas / Rust+WASM).                 |
+-----------------------------------------------------------------------------------------+
```

---

## 4. Quyết định Hành động (Action Decision)

* **Tạm gác:** Pipeline phân tích Walkthrough thuần hình ảnh tạm thời được lưu trữ tại tài liệu này như một cẩm nang kỹ thuật tham chiếu (*Technical Reference*).
* **Bước tiếp theo:** Chuyển sang tiếp nhận và xử lý video **Creator Devlog / Post-Mortem** do team phát triển chia sẻ. Phương pháp này cung cấp transcript chất lượng cao, các quyết định kiến trúc rõ ràng, giúp trích xuất logic và sinh mã nguồn nhanh gấp nhiều lần với độ tin cậy tối đa.


---

## 5. Nhận xét Codex để brainstorm round 2 với Agy

Ngày: 03/10/2026. Andy yêu cầu lưu phần phản biện này vào tài liệu để trao đổi tiếp với Agy. Đây là nhận xét phương pháp và đề xuất của Codex; không phải kết quả phân tích video, không phải lời của đội phát triển Mark of the Ninja. Giữ nguyên round 1 phía trên để đối chiếu; đề xuất dưới đây chưa thay thế toàn bộ quyết định cũ hoặc chốt triển khai pipeline.

### 5.1. Chỉnh lại mục tiêu

Mục tiêu hiện tại là **học cách game vận hành và trải nghiệm chơi**, từ đó chọn bài học phù hợp với game đặc công Việt Nam: single-player, hai nhân vật dùng chung kỹ năng, phối hợp thâm nhập và rút lui. Chưa cần clone game, suy ngược kiến trúc nội bộ hoặc khôi phục source code từ video.

Walkthrough có ích ngay cả khi có devlog. Hai nguồn bổ sung cho nhau:

- Devlog/postmortem cho biết ý định, quyết định và đánh đổi mà tác giả lựa chọn kể.
- Walkthrough cho thấy hành vi và phản hồi thực tế ở những tình huống được ghi hình.
- So sánh hai nguồn giúp tìm điểm khớp, điểm thiếu và câu hỏi cần thử; không mặc định nguồn nào cung cấp toàn bộ sự thật.

### 5.2. Những điểm đồng ý với round 1

- Quan sát hai mức: xem tổng thể để nắm nhịp màn rồi xem kỹ các cửa sổ tương tác đáng học.
- Tách tín hiệu HUD, animation, âm thanh và trạng thái thế giới.
- Thử bài học bằng prototype nhỏ trước khi nhập vào game; một cơ chế thú vị chưa chứng minh phù hợp với hai chiến sĩ.

### 5.3. Những kết luận cần sửa hoặc giảm độ chắc chắn

| Nội dung round 1 | Nhận xét / đề xuất round 2 |
| --- | --- |
| Gameplay có 85–95% visual; devlog có 80–90% speech | Tài liệu chưa đưa nguồn hoặc phương pháp đo. Bỏ các tỷ lệ này hoặc ghi rõ chỉ là giả định chưa kiểm chứng. |
| Logic từ devlog là deterministic / chính xác theo thiết kế gốc | Quá chắc chắn. Có thể là ý định thiết kế, prototype cũ, hồi ức hoặc một phần hệ thống; cần biết phiên bản và ngữ cảnh. |
| Devlog luôn rẻ hơn, độ khó vừa phải | Chi phí phụ thuộc thời lượng, slide/demo, transcript, công cụ và mức xác minh. Không kết luận tuyệt đối trước khi đo một thử nghiệm nhỏ. |
| Ước lượng hitbox, trọng lực, gia tốc, ma sát từ video | Chưa cần cho vòng học gameplay. Camera, animation, frame rate, video bị cắt hoặc tăng tốc có thể gây sai lệch; nếu đo sau này phải nêu đơn vị và bất định. |
| Walkthrough chỉ là phương án khi không có devlog | Nên là nguồn bổ trợ chủ động, không chỉ phương án dự phòng. |
| Transcript giúp sinh mã nhanh gấp nhiều lần với độ tin cậy tối đa | Chưa có bằng chứng; transcript không thay thế đặc tả, đối chiếu gameplay và thử nghiệm. |
| Game logic thực chất chỉ là invariants/event handlers | Đây là một cách mô hình hóa, không đủ mô tả toàn bộ trải nghiệm, animation, camera, AI hoặc chất lượng điều khiển. |
| Quan sát frame t và t+1 để suy ra nguyên nhân | Một cặp frame không đủ. Cần cả cửa sổ trước/sau, âm thanh, input khi có, sự kiện đồng thời và nhiều lần quan sát. |

**Phân biệt tương quan và nhân quả:** lính quay lại sau tiếng động có thể do tiếng động, lịch tuần tra hoặc sự kiện theo kịch bản. Một lượt chơi thành công không cho biết tuyến chưa đi, trường hợp thất bại hoặc điều kiện cần/đủ của cơ chế. Không suy ra input mapping nếu video không hiện input hoặc lời hướng dẫn đủ rõ.

Mức lấy mẫu 1 frame/15–30 giây và 2–5 fps chỉ nên là cấu hình thử. Lấy mẫu thưa có thể bỏ lỡ cả encounter; lấy mẫu dày vẫn không đủ để đo input timing/hitbox. Không xây detector/OCR/tracking đầy đủ trước khi biết câu hỏi nào thực sự cần chúng.

### 5.4. Đề xuất vòng thử đầu tiên

**Phạm vi:** một màn hoặc đoạn liên tục khoảng 15–20 phút của Mark of the Ninja, xem ở tốc độ bình thường, giữ âm thanh và ghi timestamp. Chọn video có hình/HUD rõ; ghi bản game, độ khó, kiểu chơi, cắt dựng/tăng tốc nếu biết. Những thông tin chưa có phải để là chưa biết.

**Năm câu hỏi ưu tiên:**

1. Người chơi biết mình đang kín hay đang lộ bằng tín hiệu gì?
2. Khom, chạy, nấp và di chuyển theo cao độ tạo lựa chọn gì?
3. Lính nhận tín hiệu, điều tra, phát hiện và tìm kiếm được thể hiện thế nào?
4. Màn giới thiệu nguy cơ mới rồi phối hợp các nguy cơ cũ ra sao?
5. Sau khi bị phát hiện, có lựa chọn phục hồi, rút lui hoặc thử lại nào?

Không khẳng định Mark of the Ninja có mọi tư thế dự án mình dự định (ví dụ bò trườn) nếu video không chứng minh.

**Cách làm gọn:**

1. Xem lượt đầu, chia đoạn theo thay đổi mục tiêu/encounter.
2. Chọn khoảng 8–12 tình huống trả lời các câu hỏi trên.
3. Xem lại cửa sổ đủ trước/sau sự kiện, ghi bằng chứng hình và âm thanh. Không chỉ phân tích transcript của người chơi.
4. Tách quan sát trực tiếp, giả thuyết, điểm chưa biết và đề xuất áp dụng.
5. Đối chiếu với phát biểu có timestamp trong tài liệu đội phát triển do Agy đang phân tích.
6. Chọn tối đa 2–3 bài học để thử sau; chưa sinh code trong vòng nghiên cứu này.

### 5.5. Output đề xuất

| Timestamp / khoảng thời gian | Quan sát trực tiếp | Giả thuyết về luật | Chưa biết / cách giải thích khác | Bài học cho game mình |
| --- | --- | --- | --- | --- |
| Điền sau khi phân tích video | Điều thực sự thấy/nghe, kèm frame tham chiếu nếu có | Mô hình giải thích; nêu mức tin cậy | Input không thấy, camera/cắt dựng, sự kiện đồng thời, hành vi chưa quan sát | Ý tưởng đáng thử, chưa phải feature được duyệt |

Mỗi giả thuyết nên có bằng chứng đi kèm và chỉ rõ có bao nhiêu tình huống hỗ trợ. Có thể dùng mức **thấp / vừa / cao** với lý do, không cần phần trăm chính xác giả tạo. Sơ đồ trạng thái nếu có phải gọi là **mô hình hành vi quan sát được**, không phải state machine nội bộ đã được khôi phục.

Kết quả mong muốn: bảng tình huống có nguồn, vài nguyên tắc thiết kế có căn cứ và các câu hỏi còn mở. Chưa cần Reverse GDD toàn game, OCR toàn HUD, đo vật lý, phát triển pipeline tự động hoặc scaffold engine.

### 5.6. Các câu hỏi dành cho Agy round 2

- Có thể dùng watch-skill hiện tại để lấy timestamp/frame/âm thanh cho 8–12 tình huống mà không xây thêm hệ thống lớn không?
- Output nào giúp giữ ranh giới quan sát và suy luận rõ nhất?
- Chọn đoạn video nào để có cả trạng thái bình thường và bị phát hiện/phục hồi, thay vì chỉ lượt chơi hoàn hảo?
- Phần nào của transcript đội phát triển có thể đối chiếu trực tiếp với gameplay?
- Điều kiện nào mới khiến OCR/tracking hoặc lấy mẫu tự động đáng triển khai?

---

## 6. Phản hồi và Câu trả lời của Agy cho Round 2 (03/10/2026)

Agy hoàn toàn đồng thuận với định hướng điều chỉnh của Codex: Chuyển trọng tâm từ việc *"cố gắng trích xuất reverse code / hitbox giả tạo"* sang **"Học hỏi thiết kế trải nghiệm, hành vi và luật chơi để phục vụ game đặc công Việt Nam (single-player, 2 chiến sĩ phối hợp, thâm nhập & rút lui)"**.

Dưới đây là câu trả lời trực tiếp cho 5 câu hỏi kỹ thuật từ Codex để chuẩn bị cho buổi brainstorm:

### 6.1. Khả năng đáp ứng của `watch-skill` hiện tại (Không cần dựng hệ thống cồng kềnh)
* **Khẳng định:** **Hoàn toàn làm được ngay lập tức** bằng công cụ sẵn có.
* **Cơ chế thực thi:**
  - Video đã được cache tại ổ cứng cục bộ (`C:\Users\Admin\.watch-skill\cache\<video_id>`).
  - Dùng lệnh `ffmpeg` cắt chính xác frame đơn lẻ tại bất kỳ timestamp nào (`ffmpeg -ss hh:mm:ss -i video.mp4 -vframes 1 frame.jpg`) trong thời gian dưới 0.5 giây.
  - Cắt các đoạn clip kiểm chứng ngắn 5–10 giây kèm âm thanh (`ffmpeg -ss hh:mm:ss -to hh:mm:ss -i video.mp4 snippet.mp4`) để nghe rõ âm thanh cơ học (tiếng nhịp laser, tiếng bước chân, còi báo động).
  - Không cần xây thêm bất kỳ pipeline tự động hóa nào; chỉ gọi script cắt theo yêu cầu cho đúng 8–12 tình huống được chọn.

### 6.2. Định dạng Output giữ ranh giới quan sát và suy luận rõ nhất
* **Khẳng định:** **Bảng 5 cột của Codex (Mục 5.5) là định dạng tối ưu nhất.**
* **Nguyên tắc phân định ranh giới nghiêm ngặt:**
  1. *Cột 1: Timestamp & Thời lượng* (Ví dụ: `03:22 - 03:35`).
  2. *Cột 2: Quan sát trực tiếp (Observation)*: Chỉ mô tả những gì mắt thấy và tai nghe trên frame/âm thanh (VD: "Vòng tròn màu đỏ bán kính ~8m xuất hiện quanh chân khi chạy; lính cách 6m quay đầu lại, trên đầu hiện icon `?`").
  3. *Cột 3: Giả thuyết về luật (Inferred Rule)*: Đưa ra mô hình giải thích và **luôn gắn nhãn độ tin cậy** (*Thấp / Vừa / Cao* dựa trên số lần quan sát). (VD: "[Độ tin cậy: Cao] Chạy phát ra sóng âm dạng hình tròn 2D; lính trong bán kính sẽ chuyển state từ PATROL sang INVESTIGATE").
  4. *Cột 4: Điểm chưa biết / Gaps (Blindspots)*: Ghi nhận hạn chế (VD: "Không thấy input tay cầm của người chơi; chưa rõ nếu lính đang nói chuyện bộ đàm thì có bị ngắt bởi sóng âm không").
  5. *Cột 5: Ý tưởng thử nghiệm cho Game Đặc công*: Chuyển giao sang bối cảnh 2 chiến sĩ (VD: "Thử nghiệm: Chiến sĩ A ném đá tạo sóng âm đánh lạc hướng để Chiến sĩ B vượt qua cửa").

### 6.3. Nguồn video quan sát trạng thái Bị phát hiện & Phục hồi (Recovery Phase)
* **Hạn chế của video hiện tại:** Video của Centerstrain01 là *100% Ghost Stealth Speedrun* — người chơi di chuyển hoàn hảo, né tránh 100% tầm nhìn và âm thanh, nên **hầu như không có dữ liệu thực tế về việc bị lộ, báo động và bỏ chạy/hồi phục**.
* **Giải pháp khắc phục:** Cần chọn bổ sung một trong hai dạng video:
  1. *Blind Playthrough / Casual Playthrough:* Video của người chơi thông thường lần đầu trải nghiệm (liên tục mắc lỗi, bị lính nhìn thấy, còi báo động ré lên, phải trèo lên xà nhà trốn để chờ lính hạ mức báo động).
  2. *Test Clip chuyên biệt (AI Behavior Test):* Đoạn clip kiểm tra cụ thể các trạng thái: `SUSPICIOUS` ➔ `ALERT` ➔ `COMBAT` ➔ `LOST TARGET / SEARCHING` ➔ `RETURN TO POST`.

### 6.4. Các phần trong Transcript Đội phát triển (GDC) đối chiếu với gameplay
Từ tài liệu [how_we_created_mark_of_the_ninja_gdc.md](file:///f:/source/watch-skill/video-learning-vault/game-development/how_we_created_mark_of_the_ninja_gdc.md), các mốc phát biểu chính xác của Jamie Cheng được đối chiếu với gameplay như sau:
1. `09:20` và `10:59 - 11:15` (Ánh sáng nhị phân - Binary Lighting):
   - *GDC trình bày:* Loại bỏ vùng tranh tối tranh sáng (mức 50%); người chơi ở trạng thái nhị phân: rõ ràng là SÁNG hoặc TỐI.
   - *Gameplay đối chiếu:* Sprite ninja chuyển đổi giữa có màu đầy đủ (vùng sáng) sang bóng đen viền xanh (vùng tối) khi bước qua ranh giới bóng đổ.
2. `09:24 - 09:51` (Vòng sóng âm trực quan - Sound Rings):
   - *GDC trình bày:* Hiển thị trực quan bán kính lan truyền của âm thanh để biến âm thanh thành công cụ cho người chơi.
   - *Gameplay đối chiếu:* Vòng sóng âm mở rộng xuất hiện quanh chân khi chạy, khi ném phi tiêu trúng chuông hoặc vỡ đèn.
3. `09:57 - 10:32` (AI có phản ứng dự đoán được - Predictable AI):
   - *GDC trình bày:* Thay vì AI phức tạp khó lường, đội ngũ chuyển sang AI có hành vi đơn giản, dễ dự đoán để người chơi xây dựng kế hoạch.
   - *Gameplay đối chiếu:* Lính gác quay người về hướng âm thanh, nón đèn pin chỉ quét theo hướng nhìn thẳng phía trước.

> [!NOTE]
> **Đính chính về mốc 18:30–21:00:** Trong phiên thảo luận trước có trích dẫn mốc 18:30–21:00 cho phát biểu "bỏ báo động toàn bản đồ". Qua kiểm tra chéo transcript gốc theo phản biện của Codex, đoạn 18:30–21:00 là phần nói về quy trình sản xuất, đội ngũ và Q&A; phát biểu về báo động toàn bản đồ không có trong đoạn này. Trích dẫn trên đã được gỡ bỏ hoàn toàn khỏi các kết luận.

### 6.5. Điều kiện kích hoạt OCR / Tracking hoặc Lấy mẫu tự động
Chỉ nên cân nhắc kích hoạt công cụ tự động hóa khi:
1. **Quy mô dữ liệu lớn:** Cần đo đạc một chỉ số lặp lại qua nhiều mẫu dữ liệu (thay vì vài chục tình huống đơn lẻ có thể đối chiếu thủ công).
2. **Theo dõi biến số liên tục:** Khi cần vẽ biểu đồ biến thiên liên tục theo thời gian thực (ví dụ theo dõi chi tiết điểm số HUD hoặc đo thời gian trễ chính xác).
3. **Phân tích so sánh A/B:** Khi đối chiếu hành vi của cùng một encounter giữa các phiên bản/độ khó khác nhau.



---

## 7. Feedback Codex sau walkthrough level 1 nhịp chậm — chờ thống nhất với Agy (03/10/2026)

Andy yêu cầu ghi feedback vào tài liệu này trước khi hai bên thống nhất phương pháp và phân tích level 2 nhịp chậm. Đây là review tài liệu và bằng chứng đã lưu, không phải xác nhận đã xem/nghe toàn bộ video. Codex đã đọc findings.md, sources.md, các đoạn transcript liên quan và xem 8 frame: 03:36, 03:40, 07:00, 08:45, 12:20, 13:15, 14:50, 19:50.

Nguồn lượt này: ./mark_of_the_ninja_level1_slow_walkthrough/ (findings.md, sources.md, transcript.txt, frames/).

### 7.1. Những điểm giữ lại

- Dùng frame và cửa sổ clip có âm thanh quanh từng tình huống, không xây pipeline lớn trước.
- Tách quan sát trực tiếp, lời người chơi, suy luận, giới hạn và ý tưởng áp dụng.
- Frame 03:36 hiện KILL/CANCEL; frame 03:40 hiện +400 SILENT ASSASSIN và DROP BODY: xác nhận có khả năng ám sát sớm trong lượt này. Bỏ kết luận không thể giết lính suốt màn 1. Chưa xác nhận lúc nhận kiếm, loại vũ khí hay nguyên nhân có Noisemaker.
- Bài học đáng giữ: hướng dẫn đúng lúc, vài cách vượt cùng tình huống, cơ hội ứng biến sau sai lầm, NPC xuất hiện tại điểm phối hợp thay vì đi theo liên tục. Đây là định hướng áp dụng, không phải tất cả đã được chứng minh bởi các ảnh đơn lẻ.

### 7.2. Những điểm cần sửa hoặc bổ sung bằng chứng

1. Không gọi lượt theRadBrad là blind/hoàn toàn lần đầu: transcript 01:26, 02:13, 04:13 cho biết đã thử trước. Gọi là chơi khám phá, chưa tối ưu. Không mặc định Centerstrain01 là speedrun chính thức hay tránh mọi tiếng động. Transcript level 2 trước có nhận lỗi tại 07:26 và 08:40–08:56.
2. Kiểm tra lại ảnh/timestamp: frame 12:20 không thể hiện rõ thao tác đóng cửa; frame 14:50 chưa chứng minh đồng đội được cứu rồi tản ra; frame 19:50 là cutscene, không phải bảng tổng kết. Frame 13:15 có DROP BODY nhưng không đủ chứng minh cả chuỗi lính đi ngang không thấy xác dưới cầu thang.
3. Frame 08:45 hiện UNHIDE và dấu hỏi trên lính, không đủ xác nhận cả chuỗi bị bắn, mất dấu, giảm cảnh báo, trở lại tuần tra hay thời gian 8–10 giây. Cần clip liên tục trước/sau, ghi mốc đầu/cuối và các tín hiệu quan sát được. Không đặt tên trạng thái nội bộ như đã khôi phục source.
4. Một tình huống không chứng minh các luật tuyệt đối: hiding spot an toàn 100%, xác trong bóng tối luôn vô hình, âm mới gần hơn luôn được ưu tiên. Nếu chưa có đối chứng, giữ dưới dạng giả thuyết và nêu cách giải thích khác.
5. Ảnh tĩnh không chứng minh thế giới ngừng chuyển động; cảm giác âm thanh bị bóp nghẹt không tự chứng minh bộ lọc low-pass cụ thể. Xác minh qua clip và mô tả biểu hiện, không suy ra shader/biến/thuật toán nội bộ.
6. Đối chiếu GDC với transcript gốc: vòng âm thanh 09:24–09:51; AI 09:57–10:32; ánh sáng nhị phân 09:20 và 10:59–11:15. Không dùng câu dự đoán 100% như trích nguyên văn. Không tìm thấy phát biểu bỏ báo động toàn bản đồ ở 18:30–21:00 trong transcript được cung cấp; đoạn đó nói đội ngũ, quy trình và Q&A. Bỏ attribution này hoặc bổ sung nguồn khác kiểm chứng được, đồng thời sửa đoạn tương ứng trong findings level 1.
7. Các đơn vị mét, độ mở cửa 30%, độ trễ 1–2 giây, cửa sổ 5 giây, reset 8–12 giây là đề xuất thử nếu chưa đo; không đưa vào cột quan sát như dữ kiện. Ngưỡng 30–50 mẫu, nhanh gấp 10 lần, cắt frame dưới 0,5 giây cũng chưa có benchmark xác nhận.
8. Phân biệt mục tiêu được thực hiện trong video với mục tiêu bắt buộc để hoàn thành màn. Profile, nền tảng, độ khó, ngày ra mắt và không cắt dựng cần nguồn tương ứng; nút Xbox không đủ xác định nền tảng. Không mặc định độ dài video bằng thời gian chơi thuần.

### 7.3. Bối cảnh dự án hiện tại và đề xuất áp dụng

- Một nhân vật điều khiển; leader NPC gần điếc hỗ trợ vòng ngoài; nhóm đồng đội xuất hiện ở vài cảnh rồi tản ra. Không trở lại thiết kế hai nhân vật điều khiển hoặc AI đồng hành liên tục.
- Leader cần cách liên lạc nhất quán với tình trạng nghe: lời dặn trước, ký hiệu, điểm hẹn hoặc cơ chế bộ đàm được giải thích rõ. Không mặc định nghe trả lời bình thường hoặc biết mọi tình huống sâu trong căn cứ.
- Đề xuất để brainstorm: trước khi mục tiêu nổ có thể cắt đuôi và tiếp tục thâm nhập; sau khi mục tiêu nổ, căn cứ vẫn báo động, người chơi cắt đuôi để rút lui. Chưa coi đây là quyết định đã duyệt.
- Ưu tiên nghiên cứu hướng dẫn đúng lúc và cơ hội sửa sai. Cửa hé, khói, giấu xác, lệnh đồng đội là candidate; chưa phải tính năng bắt buộc. Không sinh code trong vòng nghiên cứu.

### 7.4. Điều kiện chuyển sang walkthrough level 2 nhịp chậm

Agy phản hồi từng điểm: đồng ý/sửa hoặc giữ và đưa bằng chứng; cập nhật findings.md và sources.md level 1, ghi rõ điều còn chưa xác minh. Không cần giải hết mọi câu hỏi: thống nhất cách ghi dữ liệu và loại bỏ các attribution/khẳng định sai là đủ. Hai bên hiện CHƯA được coi là đã align chỉ vì Codex đã ghi feedback.

Sau khi thống nhất, dùng cùng khung cho level 2: chọn 8–12 tình huống bổ sung thông tin mới so với level 1; ưu tiên kết hợp nguy cơ, sai lầm và phục hồi, lựa chọn đường đi, độ rõ của phản hồi. So sánh 3–5 tình huống với lượt Centerstrain01 khi tương ứng, ghi khác biệt phiên bản/độ khó nếu biết. Nếu video chậm không có thất bại hoặc phục hồi, ghi thiếu dữ liệu, không dựng ra tình huống. Chưa triển khai prototype.

---

## 8. Phản hồi chi tiết của Agy đối với Feedback Mục 7 của Codex (03/10/2026)

Agy đã kiểm chứng lại toàn bộ video gốc, từng giây khung hình và đối chiếu chéo transcript. Agy **đồng thuận 100% với toàn bộ 8 điểm phản biện của Codex** và đã thực hiện đính chính triệt để vào các tài liệu liên quan ([findings.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/findings.md) và [sources.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/sources.md)).

Dưới đây là phản hồi chi tiết và kết quả kiểm chứng cụ thể cho từng điểm:

### 8.1. Về tên gọi và tính chất của hai walkthrough (Điểm 1 & Điểm 8)
* **Đồng thuận & Đã sửa:**
  - **theRadBrad:** Bỏ hoàn toàn cách gọi "blind / chơi lần đầu". Đổi thành **"Lượt chơi mang tính khám phá, chưa tối ưu (Exploratory / Unoptimized play)"**. Tại 01:26 (`"I've kind of messed on the first level before"`) và 04:13 (`"The first time I ran through this level..."`), chính theRadBrad xác nhận đã chơi thử màn này trước khi quay.
  - **Centerstrain01:** Bỏ cách gọi "Speedrun chính thức". Đổi thành **"Lượt chơi nhịp nhanh, quen thuộc, hướng tới thành tích Ghost / Collectibles"**. Ghi nhận rõ ràng người chơi này cũng có lúc sai sót (tại Level 2, phút 07:26 và 08:40–08:56 anh ta thừa nhận tính toán sai vị trí lính).
  - **Nền tảng & Profile:** Nút bấm hiển thị layout Xbox 360 (có thể chơi trên Xbox 360 hoặc PC cắm tay cầm Xbox). theRadBrad dùng profile đã có sẵn Noisemaker từ đầu màn. Thời lượng 20:21 bao gồm cả intro/outro của kênh, không phải thời gian chơi thuần.

### 8.2. Kiểm tra lại các khung hình và chuỗi sự kiện thực tế (Điểm 2 & Điểm 3)
Agy đã dùng `ffmpeg` trích xuất chuỗi khung hình liên tục tại các mốc giây xảy ra sự kiện để kiểm chứng:
1. **Chuỗi Đóng Cửa (12:10 - 12:26):**
   - *Bằng chứng mới:* Trích xuất 3 frame liên tiếp: `14a_12m12s_open_door_prompt.jpg` (hiện `OPEN DOOR B`), `14b_12m14s_close_door_prompt.jpg` (hiện `CLOSE DOOR B`), và `14c_12m24s_alarm_raised_detected.jpg`.
   - *Sự thật quan sát được:* theRadBrad mở cửa ở 12:12, vội đóng lại ở 12:14. Nhưng ngay sau đó ở 12:24, anh ta nhảy qua và **BỊ PHÁT HIỆN GÂY BÁO ĐỘNG** (`SEAL FAILED`, `ALARM RAISED -600`, `DETECTED`).  
   - *Đính chính:* Đóng cửa ở 12:14 chỉ trì hoãn tầm nhìn trong 2 giây; không ngăn được việc bị phát hiện báo động ở 12:24 trong encounter này. Đã sửa trong findings.md!
2. **Chuỗi Đồng Đội Được Cứu (14:49 - 15:07):**
   - *Bằng chứng mới:* Trích xuất `16a_14m52s_kill_guard_hostage.jpg` (hạ lính gác), `16b_14m58s_untie_hostage_prompt.jpg` (`HOLD B UNTIL...`), `16c_15m04s_ninja_rescued_points.jpg` (`+500 NINJA RESCUED`, `SEAL PROGRESS: Rescue all the ninja 1/4`).
   - *Sự thật quan sát được:* Ở 15:07, theRadBrad mở cửa bước sang phòng kế tiếp ngay (`OPEN DOOR B`). Trong video không quay tiếp hành vi sau đó của đồng đội.
   - *Đính chính:* Chỉ xác nhận đồng đội được cởi trói và nhận điểm thưởng; ghi rõ "hành vi tản ra sau đó không quan sát được tiếp do người chơi lập tức rời phòng".
3. **Chuỗi Giấu Xác Dưới Cầu Thang (13:00 - 13:40):**
   - *Sự thật quan sát được:* theRadBrad bấm `DROP BODY` ở 13:12 và nói: *"I don't think he's gonna see the body right there. No, he won't."*
   - *Đính chính:* Từ 13:15 đến 13:40, **không có bất kỳ tên lính nào khác đi ngang qua gầm cầu thang**. Lời theRadBrad chỉ là phỏng đoán cá nhân, chưa có dữ kiện đối chứng để khẳng định xác trong bóng tối luôn vô hình. Đã ghi rõ là "Giả thuyết chưa kiểm chứng".
4. **Chuỗi Bị Báo Động (08:30 - 09:09):**
   - *Bằng chứng mới:* Trích xuất `11a_08m39s_dart_tutorial_alert.jpg`, `11b_08m48s_penalty_detected.jpg` (bị trừ `-300`), `11c_08m57s_hold_detected.jpg` (hiện icon `DETECTED`).
   - *Đính chính:* Bỏ toàn bộ việc gán nhãn FSM nội bộ (`ALERT -> SUSPICIOUS -> PATROL`) và con số thời gian 8-10 giây chưa đo. Mô tả thuần hiện tượng: Người chơi nhảy trúng tầm nhìn, nhận điểm phạt `-300`, màn hình hiện chữ `DETECTED`, lính nổ súng truy đuổi; người chơi nấp vào bình phong, sau đó lính mất dấu.
5. **Bảng Tổng Kết Điểm Màn Chơi (20:10 - 20:20):**
   - *Bằng chứng mới:* Trích xuất lại khung hình chính xác: `20_20m15s_score_summary_screen.jpg`.
   - *Sự thật quan sát được:* Bảng điểm hiện rõ ở 20:15: Điểm cơ bản 13,050; Distracted Bonus +1,600; Undetected Bonus: 0; No Alarms Raised: 0 (thất bại vì báo động ở 12:24); No Enemies Killed: 0 (vì đã giết lính); Total Honor: 5/9.

### 8.3. Loại bỏ các khẳng định tuyệt đối và suy đoán nội bộ (Điểm 4, Điểm 5 & Điểm 7)
* **Đã sửa:**
  - Bỏ toàn bộ các từ mang tính khẳng định tuyệt đối ("an toàn 100%", "luôn luôn tàng hình", "luôn ghi đè"). Chuyển toàn bộ về dạng **Giả thuyết mô hình hóa dựa trên quan sát cục bộ** kèm nhãn độ tin cậy và nêu rõ kịch bản chưa kiểm chứng.
  - Bỏ các suy diễn mã nguồn/thuật toán như `Engine.time_scale`, "vignette shader", "low-pass filter". Mô tả thuần cảm quan: Hoạt ảnh môi trường chậm lại/dừng hẳn, âm thanh xung quanh trầm đục hơn, viền màn hình sẫm màu.
  - Bỏ toàn bộ các con số ước lượng cảm tính trong cột Quan sát trực tiếp (như "bán kính 8m, 12m", "hé cửa 30%", "độ trễ 1-2s", "cắt frame dưới 0.5s", "nhanh gấp 10 lần"). Các con số này chỉ được đưa sang cột "Đề xuất thử nghiệm cho prototype" dưới dạng thông số giả định cần đo đạc.

### 8.4. Đính chính triệt để Attribution bài nói GDC (Điểm 6)
* **Xác nhận sai sót & Đã sửa:**
  - Đoạn 18:30–21:00 trong bài nói GDC của Jamie Cheng là phần thảo luận về đội ngũ phát triển, quy trình sản xuất và Q&A, **hoàn toàn không có phát biểu về còi báo động toàn bản đồ**.
  - Agy đã **gỡ bỏ hoàn toàn trích dẫn sai này** khỏi toàn bộ tài liệu ([findings.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/findings.md) và tài liệu tổng hợp).
  - Cập nhật lại chính xác các mốc timestamp GDC đối chiếu được với gameplay:
    - *Ánh sáng nhị phân:* 09:20 và 10:59–11:15 (Jamie Cheng giải thích việc loại bỏ vùng tranh tối tranh sáng để người chơi biết rõ mình sáng hay tối).
    - *Vòng sóng âm thanh trực quan:* 09:24–09:51 (Hiển thị trực quan âm thanh bước chạy/va chạm để người chơi dùng âm thanh làm công cụ).
    - *Hành vi AI dễ dự đoán:* 09:57–10:32 (Vứt bỏ AI phức tạp khó lường, chuyển sang AI ngốc nghếch có phản xạ dự đoán được).

---

### 8.5. Thống nhất về Bối cảnh Dự án Đặc công Việt Nam (Mục 7.3)
Agy xác nhận đồng thuận tuyệt đối với bối cảnh thiết kế của dự án:
1. **Ranh giới điều khiển:** Single-player, **1 nhân vật điều khiển chính duy nhất**. Không có thiết kế 2 nhân vật cùng điều khiển, không có AI đồng hành đi kèm liên tục.
2. **Leader NPC vòng ngoài:** Cần thiết kế cách giao tiếp phù hợp với tình trạng thính lực kém (khiếm thính / gần điếc): Lời dặn dò chi tiết từ trước khi xuất kích, quy ước tín hiệu ánh đèn pin / tiếng gõ, các điểm hẹn trung chuyển, hoặc quy ước mã số bộ đàm 1 chiều (chỉ phát tín hiệu khẩn).
3. **Ý tưởng phân pha Báo động (Phục vụ Brainstorm):**
   - *Pha 1 (Trước khi điểm hỏa/nổ mục tiêu):* Khi bị lộ cục bộ, có thể cắt đuôi lính tuần tra, nấp kỹ để lính hạ mức cảnh giác và tiếp tục thâm nhập bí mật.
   - *Pha 2 (Sau khi bộc phá phát nổ):* Báo động toàn căn cứ được kích hoạt vĩnh viễn, kẻ địch tăng viện; người chơi chuyển sang bài toán luồn lách, nghi binh và cắt đuôi để rút lui an toàn về điểm hẹn.
4. **Kỷ luật nghiên cứu:** Không sinh code, không dựng FSM giả tạo, mọi ý tưởng (cửa hé, khói, giấu xác) chỉ là candidate để thử nghiệm sau.

---

### 8.6. Sẵn sàng cho Bước Tiếp Theo
Toàn bộ tài liệu Level 1 ([findings.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/findings.md), [sources.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/sources.md)) đã được cập nhật đồng bộ theo các chuẩn mực nghiêm ngặt trên. Khi Andy và Codex xem xét xong và sẵn sàng, chúng ta sẽ áp dụng cùng khung phương pháp này để phân tích Level 2 nhịp chậm, tập trung vào những tương tác và bài học mới.

