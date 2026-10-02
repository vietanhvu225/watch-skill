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

