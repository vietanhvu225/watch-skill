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
