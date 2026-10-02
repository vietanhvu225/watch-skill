# 🥷 Mark of the Ninja — Level 1 ("Ink & Dreams"): Giải Phẫu Kỹ Thuật Thiết Kế Màn Chơi, Pacing Tutorial & Mechanics Reverse-Engineering

> **Nguồn tư liệu:** [Mark of the Ninja Walkthrough | 100% Stealth / Collectibles Part 1 | "Ink & Dreams" — Centerstrain01](https://www.youtube.com/watch?v=CpMr5EfeGfc)  
> **Lối chơi thực nghiệm:** 100% Ghost / Pure Stealth / Non-Lethal (0 Kills, 0 Alarms, 3/3 Scrolls, 3/3 Artifacts, hoàn thành Sub-objective gõ 4 chuông).  
> **Thời lượng:** 11:06 | **Transcript gốc:** [mark_of_the_ninja_level1_ink_and_dreams_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_ink_and_dreams_transcript.txt)  
> **Tài liệu lý thuyết đối chiếu:** [how_we_created_mark_of_the_ninja_gdc.md](file:///f:/source/watch-skill/video-learning-vault/game-development/how_we_created_mark_of_the_ninja_gdc.md)  
> **Danh mục:** 🎮 Game Development & Engineering | **Màn chơi:** Level 1 - *Ink & Dreams* (Mực và Mộng mị)

---

## Executive Summary: Nghệ thuật "Dạy Chơi Không Dùng Chữ" (Show, Don't Tell Tutorial)

Level 1 của *Mark of the Ninja* ("Ink & Dreams") được các chuyên gia thiết kế màn chơi (*Level Designers*) trên thế giới đánh giá là một trong những **màn hướng dẫn (Tutorial Level) mẫu mực nhất lịch sử game 2D**. 

Thay vì bắt người chơi đọc những bức tường chữ hướng dẫn tẻ nhạt, Klei Entertainment đã áp dụng một quyết định thiết kế thiên tài: **TƯỚC ĐOẠT VŨ KHÍ CỦA NGƯỜI CHƠI (DISARMING THE PLAYER)**.
* Nhân vật chính thức tỉnh trong cuộc đột kích của lính đánh thuê Hessian nhưng **hoàn toàn không có kiếm**.
* Không có kiếm đồng nghĩa với việc: **Người chơi KHÔNG THỂ chém giết hay dùng bạo lực để vượt qua thử thách**.
* Ràng buộc này ép người chơi phải tập trung 100% vào việc: **Học cách quan sát ánh sáng, di chuyển rón rén trong bóng tối, nấp sau bình phong, lắng nghe tiếng động và luồn lách qua ống thông gió**.
* Chỉ đến cuối màn chơi, sau khi đã thuần thục toàn bộ bản năng sinh tồn của một Ninja lén lút, người chơi mới được trao lại thanh kiếm Katana.

Dưới đây là bản giải phẫu kỹ thuật chi tiết từng căn phòng, từng cơ chế kích hoạt và hệ thống tính điểm để phục vụ việc lập trình tái tạo game tương tự.

---

## 1. Giải Phẫu Từng Phân Cảnh (Room-by-Room Deconstruction & Mechanics Pacing)

```
[ Room 1: Thức tỉnh ] ➔ [ Room 2: Sóng âm bước chân ] ➔ [ Room 3: Nấp & Tránh né lính đầu tiên ]
          |                                                               |
          v                                                               v
[ Room 6: Phá vỡ đèn ]  [ Room 5: Gõ chuông & Nghe lén ]  [ Room 4: Focus Mode & Phi tiêu ]
          |
          v
[ Room 7: Đánh lạc hướng kép & Cứu đồng đội ] ➔ [ Room 8: Sân đình trung tâm & Cứu Sensei Azai ]
```

---

### Phân cảnh 1: Tỉnh thức & Ẩn nấp cơ bản (00:00 - 01:50)
* **Bối cảnh:** Ngôi đền của tộc Hisomu bị tấn công bằng vũ khí hiện đại. Nữ đồng hành Ora đánh thức nhân vật chính.
* **Cơ chế được giới thiệu:**
  1. *Di chuyển cơ bản & Nấp sau vật thể:* Nhắc người chơi bấm nút tương tác (`PRESS TO USE HIDING SPOTS`) khi đứng cạnh bình phong hoặc bình hoa.
  2. *Ánh sáng nhị phân:* Khi lính cầm đèn pin bước vào phòng, người chơi đang nấp sẽ thấy tên lính bước qua mặt mình mà không hề bị phát hiện.
  3. *Thoát nấp (`UNHIDE`) & Đu bám gờ tường (`Ledge Grab & Pull Up`):* Nhân vật nhảy bám vào gờ sàn tầng trên và kéo người lên nhẹ nhàng.
* **Quy tắc thiết kế:** Không đặt kẻ địch nguy hiểm; tạo tình huống an toàn tuyệt đối để người chơi nhận diện cơ chế "Nấp = Tàng hình 100%".

---

### Phân cảnh 2: Tiếng bước chân & Vòng sóng âm trực quan (01:50 - 02:45)
* **Cơ chế được giới thiệu:**
  1. *Sự tương phản âm thanh (Running vs Sneaking):*
     - Khi chạy nhanh: Một **vòng tròn màu đỏ mở rộng (*expanding red sound ring*)** bán kính $R \approx 8m$ xuất hiện quanh bước chân.
     - Khi rón rén (giữ cần gạt nhẹ hoặc phím đi bộ): Bán kính vòng âm thanh $R = 0m$ (Hoàn toàn yên lặng).
  2. *Chỉ dẫn âm thanh không gian từ kẻ địch (*Spatial Dialogue Cues*):*
     - Lính đánh thuê trò chuyện: *"Did you see those banners? We could totally sell them to a gallery..."*
     - Lời thoại vừa xây dựng thế giới, vừa giúp người chơi ước lượng vị trí lính đang đứng ở tầng dưới.
  3. *Leo trèo trần nhà (Ceiling Pipe Traversal):* Người chơi bám tay vào xà ngang/đường ống trên trần để di chuyển qua đầu lính.
* **Vật phẩm thu thập:** Nhặt **Cuộn giấy Hisomu (Scroll 1/3)** đầu tiên kể về lịch sử dòng tộc.

---

### Phân cảnh 3: Hệ thống Điểm thưởng & Triết lý "Ghost" (02:45 - 04:10)
* **Bối cảnh:** Căn phòng đầu tiên có 1 lính đánh thuê đi tuần tra liên tục qua lại giữa 2 điểm.
* **Cơ chế tính điểm cốt lõi (Score & Reward System):**
  * `+200 UNDETECTED`: Điểm thưởng khi ninja vượt qua một khu vực tuần tra mà không làm kẻ địch nghi ngờ.
  * `+150 DISTRACTED`: Điểm thưởng khi dùng âm thanh lừa lính quay lưng lại.
  * `+500 ARTIFACT RECOVERED`: Điểm thưởng khi nhặt cổ vật ẩn.
* **Thiết kế Game Feel:** Game không trừng phạt người chơi không giết chóc; trái lại, người chơi theo phong cách **Ghost (Bóng ma phi sát thương)** nhận được số điểm tương đương hoặc cao hơn phong cách Predator (Đồ sát).

---

### Phân cảnh 4: Chế độ Focus Mode (Đóng băng thời gian) & Phi tiêu Darts (04:10 - 05:20)
* **Vũ khí nhận được:** **Phi tiêu Darts** (Vũ khí không sát thương, chỉ dùng để tương tác môi trường).
* **Cơ chế Đột phá: Focus Mode (Time Freeze Aiming):**
  * Câu thoại triết lý trên màn hình: *"Focus your thoughts and you can freeze time in your mind"*.
  * **Hành vi kỹ thuật:**
    - Khi người chơi giữ nút ngắm bắn (Right Trigger / Chuột phải), **toàn bộ thế giới trong game dừng lại 100% (Thời gian đóng băng)**.
    - Một đường cong quỹ đạo ngắm bắn hiển thị chính xác mục tiêu (đèn, chuông, nút bấm).
    - Triệt tiêu hoàn toàn áp lực thời gian thực, cho phép người chơi bình tĩnh căn chỉnh góc bắn.
* **Thử thách không gian:**
  * Nhảy qua nón tầm nhìn (*Vision Cone*) của lính gác từ trên cao để nhặt Cổ vật 1 (*Artifact 1*).
  * Chui xuống hệ thống ống thông gió sàn (`LOOK FOR VENT / GRATE`) để né hoàn toàn lính gác và nhặt Cuộn giấy 2 (*Scroll 2/3*).

```
   [ Focus Mode Activated ]
   +-------------------------------------------------------+
   |  - Engine TimeScale = 0.0 (Freeze Physics & AI)       |
   |  - Audio: Low-pass filter (Hiệu ứng ù tai dưới nước)   |
   |  - Camera: Thu phóng nhẹ vào tâm ngắm                 |
   |  - Trajectory Line: Vẽ tia parabol laser chính xác    |
   +-------------------------------------------------------+
```

---

### Phân cảnh 5: Sóng âm diện rộng & Thử thách phụ Gõ 4 quả chuông (05:20 - 06:40)
* **Thử thách phụ (Sub-objective):** Gõ vang 4 quả chuông phong thủy trong màn chơi (`Ring 4 Bells`).
* **Cơ chế kích hoạt sóng âm chủ động:**
  * Bắn phi tiêu vào quả chuông treo trên cao ➔ Chuông phát ra **vòng sóng âm khổng lồ ($R \approx 15m$)**.
  * Tên lính đang chặn cửa nghe thấy ➔ Lập tức đổi trạng thái từ `UNAWARE` sang `SUSPICIOUS`, quay lưng lại và bước về phía quả chuông để kiểm tra.
  * Người chơi ung dung đi xuyên qua cửa mà không bị phát hiện.
* **Cơ chế Nghe lén qua cửa (Door Peeking / Sensing):**
  * Hướng dẫn: *"See that? Don't open it yet. Just lean against it and try to sense what's on the other side."*
  * Khi người chơi đứng tựa người vào cánh cửa đóng kín: Camera tự động mở rộng và hiển thị không gian phòng bên kia dưới dạng **bóng mờ màu xanh (Silhouette Blue Reveal)**.
  * Người chơi thấy rõ vị trí lính và chùm đèn pin trước khi quyết định mở cửa ➔ **Triệt tiêu 100% sự bất ngờ bất công (Unfair surprises)**.

---

### Phân cảnh 6: Phá hủy nguồn sáng & Tận dụng Chiều đứng (06:40 - 07:45)
* **Cơ chế phá hủy môi trường:**
  * Bắn phi tiêu vào bóng đèn huỳnh quang / đèn lồng ➔ Đèn vỡ tan kèm tiếng nổ nhỏ.
  * Căn phòng ngay lập tức chìm vào bóng tối (`is_lit = false`).
  * Tên lính hoang mang, đèn pin rung rinh, đi về phía chiếc đèn vừa vỡ.
* **Chiều đứng (Vertical Platforming):**
  * Ninja leo lên thanh xà trần nhà, bò qua đầu tên lính khi hắn đang cúi xuống soi đèn vỡ.
  * Thu thập Cuộn giấy cuối cùng (*Scroll 3/3*).

---

### Phân cảnh 7: Đánh lạc hướng kép (Dual Distraction) & Giải cứu Ninja đồng đội (07:45 - 08:50)
* **Bài toán câu đố stealth:**
  * Một ninja đồng đội bị bắt trói treo lơ lửng giữa phòng.
  * Có 2 tên lính gác đứng chéo góc với tầm nhìn bao quát lẫn nhau. Nếu chỉ bắn cắt dây cứu đồng đội, tiếng rớt xuống sẽ báo động tên lính thứ 2.
* **Kỹ thuật Đánh lạc hướng kép (Dual Distraction):**
  1. *Hành động 1:* Bắn phi tiêu cắt dây rơi ninja đồng đội xuống.
  2. *Hành động 2:* Ngay lập tức bắn quả phi tiêu thứ hai vào vật thể ở góc xa đối diện.
  3. *Quy tắc AI:* **Âm thanh mới nhất sẽ ghi đè (override) ưu tiên điều tra của AI**. Tên lính quay ngoắt sang góc xa, người chơi tiếp cận giải cứu đồng đội an toàn.
* Gõ quả chuông thứ 3 và thứ 4 ➔ Hoàn thành trọn vẹn Sub-objective. Nhặt Cổ vật thứ 3 (*Artifact 3/3*).

---

### Phân cảnh 8: Cao trào Sân đình & Giải cứu Sensei Azai (08:50 - 10:10)
* **Bối cảnh cao trào:**
  * Tên trùm lính đánh thuê Kelly đang chĩa súng đe dọa chưởng môn Azai: *"You picked the wrong guys to rob, sensei. It's time for the old man to retire, boys."*
  * Khu vực sân đình rộng lớn có 3 lính súng trường hạng nặng canh gác nghiêm ngặt.
* **Lộ trình giải quyết theo phong cách Ghost:**
  1. Đu dây bám dọc theo tường bên phải, thả người xuống vùng tối bên dưới sân.
  2. Ném phi tiêu đánh lạc hướng lính ở góc xa bên trái.
  3. Khi cả 3 lính đồng loạt quay lưng sang trái điều tra, ninja lướt nhanh vào góc tối cạnh Sensei Azai.
* **Phần thưởng tự sự:**
  * Cutscene diễn ra: Sensei Azai giải thích nguồn gốc hình xăm ma thuật Desert Bloom (The Mark) — ban tặng giác quan siêu phàm nhưng sẽ dần tha hóa tâm trí người mang nó.
  * Sensei trao lại thanh kiếm Katana cổ truyền. Ninja chính thức trở thành kiếm sĩ bóng đêm hoàn chỉnh.

---

## 2. Bản Đồ Kiến Trúc Màn Chơi (Level Connectivity ASCII Graph)

```
[ OUTSIDE / ENTRY ]
       |
       v
+------------------+     (Vents / Pipes)
| ROOM 1: Bedchamber| ==================> [ ROOM 2: Upper Rafters ]
| - Hide Spot: Vase |                      | - Sound tutorial
| - 1 Guard Patrol  |                      | - Hisomu Scroll #1
+------------------+                      +-----------------------+
                                                      |
                                                      v
                                          +-----------------------+
                                          | ROOM 3: Guard Hall    |
                                          | - +200 Undetected     |
                                          | - Ledge Climb         |
                                          +-----------------------+
                                                      |
                                                      v
+------------------------+ (Focus Mode)  +-----------------------+
| ROOM 5: Bell Shrine    | <============ | ROOM 4: Armory/Darts  |
| - Sub-obj: 4 Bells     |               | - Freeze-time Aim     |
| - Door Peeking (Sense) |               | - Artifact #1 & #2    |
| - Hisomu Scroll #2     |               | - Floor Grate Vents   |
+------------------------+               +-----------------------+
       |
       v
+------------------------+
| ROOM 6: Dark Corridor  |
| - Breakable Lanterns   |
| - Light-to-Dark toggle |
| - Hisomu Scroll #3     |
+------------------------+
       |
       v
+------------------------+
| ROOM 7: Hostage Cell   |
| - Dual Distraction     |
| - Rescue Ally Ninja    |
| - Artifact #3          |
+------------------------+
       |
       v
+---------------------------------------------+
| ROOM 8: Central Courtyard (Grand Finale)    |
| - 3 Heavy Mercenaries (Assault Rifles)      |
| - Multi-tier Shadow Paths                   |
| - Rescue Sensei Azai -> RECEIVE KATANA      |
+---------------------------------------------+
```

---

## 3. Bảng Bóc Tách Thực Thi Kỹ Thuật (Technical Implementation Matrix)

| Cơ chế (Mechanic) | Input Trigger | Hành vi Hệ thống (System Behavior) | Phản hồi Thị giác & Âm thanh (Feedback) | Giá trị Điểm thưởng |
|---|---|---|---|---|
| **Hiding Spot** | Nút `B` / Phím `E` | Vô hiệu hóa Collider của Player; gán cờ `is_hidden = true`. Guard đi qua không kích hoạt Raycast va chạm. | Nhân vật ẩn vào sau bức tranh/bình hoa; hiển thị icon `UNHIDE`. | Giữ trạng thái ẩn mật |
| **Running Sound Ring** | Giữ `Shift` / Cần gạt đẩy hết cỡ | Gọi hàm `emit_sound(pos, 8.0)`. Quét tất cả Guard trong bán kính 8m. | Vẽ vòng tròn đỏ mở rộng quanh chân player; tiếng bước chân dồn dập. | 0 điểm (Nguy cơ bị lộ) |
| **Sneaking** | Di chuyển nhẹ / Không giữ chạy | Gọi hàm `emit_sound(pos, 0.0)`. | Không có vòng âm thanh; nhân vật hạ thấp trọng tâm, di chuyển uyển chuyển. | Tránh báo động |
| **Undetected Bonus** | Vượt qua vùng kiểm soát của Guard | Trigger Box gắn ở lối ra của mỗi phòng kiểm tra: `if guard.state == UNAWARE: award_points()`. | Popup chữ vàng `+200 UNDETECTED` nổi lên trên màn hình. | `+200` điểm |
| **Focus Mode** | Giữ `LT` / Chuột phải | `Engine.time_scale = 0.0` (Đóng băng vật lý & AI). Cho phép xoay cần điều khiển để chỉnh góc bắn. | Màn hình tối viền (*Vignette*), âm thanh nghẹt (*Muffled audio*), vẽ tia laser quỹ đạo parabol. | Miễn phí thời gian |
| **Dart Throw** | Nhả `RT` / Click chuột trái | Bắn ra đạn `Dart2D` theo vector vận tốc của tia laser. | Đèn vỡ tung tóe mảnh kính; chuông rung bần bật phát sóng âm $R=15m$. | `+150 DISTRACTED` |
| **Door Peeking** | Đứng sát cửa kín | Kiểm tra tiếp xúc Raycast với cửa; kích hoạt Shader làm trong suốt phòng kế tiếp. | Mở rộng Camera, chuyển phòng kế bên sang màu xanh dương bóng mờ silhouette. | Xóa bỏ rủi ro mở cửa |
| **Dual Distraction** | Bắn 2 phi tiêu liên tiếp trong 1.5s | AI Guard hủy lệnh điều tra điểm 1, lập tức quay đầu điều tra điểm 2. | Vòng âm thanh thứ 2 đè lên vòng thứ 1; icon `?` trên đầu lính nhấp nháy chuyển hướng. | `+150` điểm mỗi lần |

---

## 4. Hướng Dẫn Lập Trình Trên Godot Engine (`godot-ai`)

Để hiện thực hóa Level 1 này vào mã nguồn cụ thể bằng Godot Engine, cấu trúc các Node chính như sau:

### 4.1. Hệ thống Đóng Băng Thời Gian (FocusAimSystem.gd)
```gdscript
extends Node2D
class_name FocusAimSystem

@export var trajectory_line: Line2D
var is_aiming: bool = false
var aim_origin: Vector2
var aim_direction: Vector2

func _input(event: InputEvent) -> void:
    if event.is_action_pressed("aim"):
        start_focus()
    elif event.is_action_released("aim"):
        fire_dart()

func start_focus() -> void:
    is_aiming = true
    Engine.time_scale = 0.05 # Làm chậm gần như đóng băng hoàn toàn
    trajectory_line.visible = true
    AudioServer.set_bus_effect_enabled(1, 0, true) # Bật Low-Pass Filter

func _process(delta: float) -> void:
    if is_aiming:
        # Dùng get_global_mouse_position() bất chấp time_scale bị chậm
        aim_direction = (get_global_mouse_position() - global_position).normalized()
        update_trajectory_line()

func fire_dart() -> void:
    is_aiming = false
    Engine.time_scale = 1.0 # Trả lại thời gian thực
    trajectory_line.visible = false
    AudioServer.set_bus_effect_enabled(1, 0, false)
    # Instantiate Dart Projectile theo vector aim_direction
```

### 4.2. Hệ thống Nhìn Trộm Qua Cửa (DoorPeekSensor.gd)
```gdscript
extends Area2D
class_name DoorPeekSensor

@export var next_room_viewport: SubViewport
@export var door_silhouette_shader: CanvasItemMaterial

func _on_body_entered(body: Node2D) -> void:
    if body.is_in_group("player"):
        # Kích hoạt hiệu ứng nhìn thấu phòng kế bên
        reveal_next_room(true)

func _on_body_exited(body: Node2D) -> void:
    if body.is_in_group("player"):
        reveal_next_room(false)

func reveal_next_room(reveal: bool) -> void:
    var tween = create_tween()
    var target_alpha = 1.0 if reveal else 0.0
    tween.tween_property(door_silhouette_shader, "shader_parameter/visibility", target_alpha, 0.3)
```

---

## 5. Kết Luận: Công Thức Tạo Màn Chơi Stealth Mẫu Mực

1. **Tước đoạt sức mạnh trước, trao thưởng sức mạnh sau:** Buộc người chơi làm chủ cơ chế phòng thủ và lẩn trốn trước khi cho phép họ tấn công.
2. **Một phòng - Một bài học:** Mỗi căn phòng trong Level 1 chỉ giới thiệu duy nhất một tương tác mới (Phòng 1: Nấp; Phòng 2: Sóng âm bước chân; Phòng 4: Focus ném phi tiêu; Phòng 5: Nghe lén cửa; Phòng 6: Phá đèn; Phòng 7: Đánh lạc hướng kép).
3. **Ghost là lựa chọn cao quý nhất:** Điểm thưởng không phát sinh từ số lượng xác chết, mà phát sinh từ sự thuần khiết khi đi qua một căn phòng như một bóng ma vô hình (`+200 Undetected`).
