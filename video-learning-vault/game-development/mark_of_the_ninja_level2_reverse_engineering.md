# 🥷 Mark of the Ninja — Level 2 ("Breaching the Perimeter"): Giải Phẫu Kỹ Thuật Đột Nhập Công Nghiệp, Laser Grids & Pacing Thử Thách

> **Nguồn tư liệu:** [Mark of the Ninja Walkthrough | 100% Stealth / Collectibles Part 2 | "Breaching the Perimeter" — Centerstrain01](https://www.youtube.com/watch?v=wF6noSysObY)  
> **Lối chơi thực nghiệm:** 100% Ghost / Pure Stealth / Non-Lethal (0 Kills, 0 Alarms, 3/3 Scrolls, 3/3 Artifacts, Đạt 3/3 Seals: Destroy 20 Lights, Sub-minute Transformer, Undetected Tower Climb).  
> **Thời lượng:** 10:58 | **Transcript gốc:** [mark_of_the_ninja_level2_breaching_perimeter_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level2_breaching_perimeter_transcript.txt)  
> **Đối chiếu phần 1:** [mark_of_the_ninja_level1_reverse_engineering.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_reverse_engineering.md)  
> **Tài liệu lý thuyết đối chiếu:** [how_we_created_mark_of_the_ninja_gdc.md](file:///f:/source/watch-skill/video-learning-vault/game-development/how_we_created_mark_of_the_ninja_gdc.md)  
> **Danh mục:** 🎮 Game Development & Engineering | **Màn chơi:** Level 2 - *Breaching the Perimeter* (Chọc Thủng Vành Đai Phòng Thủ)

---

## Executive Summary: Bước Chuyển Từ "Thủ Thế Tự Vệ" Sang "Tấn Công Công Nghiệp"

Nếu như **Level 1 ("Ink & Dreams")** là bài học vỡ lòng mang tính tôn nghiêm cổ kính trong ngôi đền Hisomu, nơi người chơi bị tước đoạt vũ khí để học cách sinh tồn cơ bản trong bóng tối, thì **Level 2 ("Breaching the Perimeter")** là cú nhảy vọt về quy mô thiết kế và độ phức tạp cơ học:

1. **Sự va chạm văn minh (Tradition vs. Modern Technology):**
   * Người chơi rời khỏi kiến trúc gỗ, bình phong và chuông đồng của ngôi đền cổ để đột nhập vào căn cứ quân sự bọc thép kiên cố của tập đoàn lính đánh thuê **Hessian Services** do Bá tước **Karajan** cầm đầu.
   * Kẻ thù không còn chỉ là lính cầm đèn pin đơn thuần, mà là một tổ hợp an ninh công nghiệp: **Lưới tia laser cảm biến (Laser Grids)**, **Trạm biến áp cao thế (High-Voltage Transformers)**, **Máy phát điện dự phòng (Diesel Generators)**, **Báo động liên lạc bộ đàm (Tactical Radio Networks)**, và **Cửa từ chống đột nhập (Magnetic Bulkheads)**.

2. **Cái giá của hình xăm (The Curse of the Ink & The Champion's Vow):**
   * Mở đầu màn chơi hé lộ nút thắt tự sự sâu sắc: Chất mực xăm ma thuật được chiết xuất từ độc tố của một loài hoa sa mạc hiếm. Nó ban cho ninja phản xạ siêu phàm và khả năng bẻ cong giác quan, nhưng đổi lại sẽ **gặm nhấm lý trí và đẩy người mang nó vào cơn điên loạn tột cùng**.
   * Mọi Ninja được chọn (Champion) đều phải thề một lời thề nghiệt ngã: **Tự sát danh dự (Ritual Suicide / Seppuku)** trước khi độc tố nuốt chửng linh hồn và biến họ thành quái vật hủy diệt chính gia tộc mình.

3. **Mở rộng kho công cụ chủ động:**
   * Giới thiệu trang bị gây xao nhãng chủ động: **Noisemaker** (thiết bị phát sóng âm nhân tạo có thể gắn lên mọi bề mặt).
   * Cơ chế câu đố vật lý: **Đẩy/kéo thùng hàng nặng (`Crate Dragging`)** để chắn tia laser cảm biến.
   * Thử thách áp lực thời gian động: **Hệ thống phục hồi điện lưới (Breaker Reset Cycles)** — khi ninja ngắt cầu dao, lính gác sẽ đàm thoại bộ đàm và đi kiểm tra đóng lại điện, tạo ra cửa sổ thời cơ có hạn.

---

## 1. Giải Phẫu Từng Phân Cảnh (Room-by-Room Deconstruction & Level Pacing)

```
[ Phân cảnh 1: Vành đai ngoài & Phá 20 đèn ] ➔ [ Phân cảnh 2: Phòng biến áp & Tốc hành 60s ]
                           |
                           v
[ Phân cảnh 4: Góc khuất cầu thang & Bụi rậm ]  [ Phân cảnh 3: Phòng thử thách & Kéo thùng chắn Laser ]
                           |
                           v
[ Phân cảnh 5: Đánh lạc hướng tách đôi lính ] ➔ [ Phân cảnh 6: Tháp chỉ huy & Phục hồi cầu dao ]
                           |
                           v
              [ Phân cảnh 7: Đỉnh tháp & Cuộn giấy Tetsuji ]
```

---

### Phân cảnh 1: Vành đai phòng hộ ngoài & Giới thiệu Thử thách 20 Bóng Đèn (00:00 - 02:00)
* **Bối cảnh:** Ranh giới bên ngoài khu phức hợp Hessian. Tường bê tông kiên cố, tháp canh và hàng rào kẽm gai.
* **Cơ chế & Thử thách:**
  1. *Thử thách con dấu (Seal 1): Phá hủy ít nhất 20 bóng đèn (`Destroy at least 20 Lights`).*  
     * Thiết kế này biến việc bắn phi tiêu vào đèn từ một thủ thuật phụ ở Level 1 thành **một thói quen định tuyến bắt buộc (Continuous Environmental Routing)**.
     * Người chơi phải luôn ngước nhìn trần nhà, tính toán quỹ đạo phi tiêu để dập tắt ánh sáng huỳnh quang trước khi tiến vào vùng tuần tra.
  2. *Bám mép tường & Nhảy vọt qua đầu lính (`Wall Vault & Leap`):*  
     * Lính gác ở khu vực này cầm súng tuần tra liên tục. Người chơi phải canh thời điểm lính vừa quay lưng bước qua bóng đèn, nhảy bám tường và đu qua đầu hắn.
  3. *Tối ưu hóa điểm số Ghost:*  
     * Game ghi nhận khoảng cách áp sát: Nếu ninja đu dây lên ngay sát lưng kẻ thù trước khi vượt qua, hệ thống sẽ kích hoạt điểm thưởng áp sát tối đa (`+200 Undetected`).

---

### Phân cảnh 2: Trạm Biến Áp & Thử thách Tốc độ 60 Giây (02:00 - 03:15)
* **Mục tiêu chính:** `DESTROY THE TRANSFORMER` (Phá hủy trạm biến áp điện để mở cổng an ninh chính).
* **Thử thách con dấu (Seal 2):** Tiếp cận và phá hủy trạm biến áp trong thời gian **dưới 1 phút (`Reach the transformer in under a minute`)**.
* **Kỹ thuật thiết kế Pacing (Speedrun Incentive):**
  * Klei khéo léo lồng ghép bài kiểm tra tốc độ: Để đạt 100% danh dự (Honor), người chơi không thể chỉ di chuyển rùa bò. Họ phải nắm vững lộ trình, kết hợp giữa phi tiêu dập đèn tầm xa, lướt ống thông gió sàn và trèo tường liên hoàn.
* **Vũ khí mới nhận được:** **Noisemaker** (Thiết bị tạo tiếng ồn cơ học).  
  * Khác với phi tiêu đập vào chuông có sẵn, Noisemaker có thể gắn vào bất kỳ vị trí nào để phát xung âm liên tục, thu hút lính rời khỏi vị trí chốt chặn.

---

### Phân cảnh 3: Phòng Thử Thách Ẩn — Cơ chế Lưới Laser & Thùng Hàng Nặng (03:15 - 04:50)
* **Khu vực đặc biệt:** **Challenge Room 1** (Phòng thử thách giải đố môi trường để lấy Cuộn giấy Hisomu #1).
* **Cơ chế Lưới Laser Đỏ (`Red Laser Grids`):**
  * Khác với ánh sáng đèn thông thường chỉ làm lộ diện ninja, **tia laser màu đỏ là cảm biến an ninh tức thời**:
    - Chạm vào tia laser ➔ Báo động toàn khu vực lập tức vang lên, cửa sắt sập xuống hoặc phóng điện gây sát thương chí mạng.
  * **Nhịp điệu xung nhịp âm thanh (Acoustic Cadence):**  
    - Tia laser bật/tắt theo chu kỳ 4 nhịp cơ học (4 cơ chế "tách - tách - tách - cạch"). Người chơi phải lắng nghe nhịp điệu âm thanh để tính thời điểm chạy nước rút.
* **Cơ chế Đẩy/Kéo Thùng Hàng Nặng (`Draggable Heavy Crate`):**
  * Người chơi giữ nút `B` để bám vào thùng hàng kim loại nặng và kéo/đẩy nó.
  * **Tương tác che chắn vật lý:** Thùng hàng có thể chặn đứng tia laser. Ninja đẩy thùng vào đường cắt của chùm laser để tạo một khoảng trống an toàn bên dưới, sau đó trèo qua nó để lấy Cuộn giấy cổ.
  * *Ràng buộc:* Người chơi phải kéo đủ 11-12 bước di chuyển. Nếu nhảy quá cao vượt khỏi tầm che của thùng hàng, đầu của ninja sẽ chạm vào chùm laser phía trên và chết ngay lập tức.

---

### Phân cảnh 4: Góc Khuất Cầu Thang & Ngụy Trang Bụi Cây (04:50 - 06:15)
* **Cơ chế Góc Khuất Dưới Cầu Thang (Under-Stairway Blindspot):**
  * Trong kiến trúc bậc thang hở (open metal stair treads), nón tầm nhìn của lính gác hướng chếch lên trên hoặc thẳng về phía trước theo hướng bước đi.
  * Khoảng tam giác tối bên dưới gầm cầu thang hoàn toàn che khuất ninja khỏi tầm mắt của lính đang đi trên bậc thang ngay trên đầu. Người chơi có thể nấp yên tĩnh ngay dưới chân kẻ thù mà không sợ bị phát hiện.
* **Cơ chế Ẩn Nấp Ngoài Trời (`Bush Hide`):**
  * Khi tiến ra khu vực sân ngoài trời, trò chơi thay thế bình phong trong nhà bằng **các bụi cây rậm rạp (`Bushes/Planters`)**.
  * Khi chui vào bụi cây, thân ảnh ninja chuyển sang dạng bóng đen viền trắng mờ; lính gác dù cầm đèn pin quét ngang bụi rậm cũng không thể phát hiện.

---

### Phân cảnh 5: Kỹ Thuật Đánh Lạc Hướng Tách Đôi Lính (Dual Guard Acoustic Split) (06:15 - 08:00)
* **Bài toán hóc búa của Level Designer:**
  * Có 2 lính gác đứng chốt chặn ở một hành lang hẹp, mắt nhìn chéo nhau. Nếu ném phi tiêu vào một điểm, cả 2 lính sẽ cùng đi về phía đó hoặc một tên đi còn một tên ở lại bọc lót.
* **Giải pháp: Kích hoạt sóng âm ngược hướng (Opposing Acoustic Triggers):**
  1. Chờ đợi lính tuần tra bên phải di chuyển về phía xa.
  2. Nấp trong bụi rậm, bắn phi tiêu vào vật thể ở cực bên trái ➔ Kéo lính gác bên trái quay lưng bước sang trái.
  3. Ngay lập tức bắn phi tiêu thứ hai vào vật thể ở cực bên phải ➔ Kéo lính gác bên phải bước tiếp sang phải.
  4. **Kết quả:** Hành lang ở giữa hoàn toàn trống trải. Ninja lướt qua trung lộ êm ru mà cả hai tên lính đều không hay biết.

```
       [ Guard A ]  (Sound 1)                (Sound 2)  [ Guard B ]
             ^                                                  ^
             |                     [ EMPTY CORRIDOR ]           |
             +------------------- [ Ninja Slips Through ] -------+
```

---

### Phân cảnh 6: Tháp An Ninh & Cơ Chế Khôi Phục Cầu Dao Tự Động (08:00 - 09:30)
* **Bối cảnh:** Bên trong Tháp An Ninh (Security Tower) nhiều tầng.
* **Thử thách con dấu (Seal 3):** Leo lên đỉnh tháp mà hoàn toàn không bị phát hiện (`Reach the top of the tower without being detected`).
* **Cơ chế Khôi phục Cầu Dao (Dynamic Breaker Reset Cycle):**
  * Ninja phá máy phát hoặc ngắt cầu dao tổng để vô hiệu hóa lưới laser và đèn trần.
  * **Hành vi AI phản ứng qua bộ đàm:**  
    - Màn hình hiển thị phụ đề bộ đàm: `Radio: Turning the power for 16 back on`.
    - Hệ thống không để phòng tối vĩnh viễn: Một tên lính gác gần đó sẽ được giao nhiệm vụ đi đến bảng điều khiển để bật lại điện, hoặc hệ thống điện tử tự khởi động lại sau một khoảng đếm lùi ẩn.
  * **Khoảng thời gian vàng (Window of Opportunity):**  
    - Người chơi chỉ có khoảng 15-20 giây trong bóng tối trước khi điện phụt sáng trở lại. Nếu không leo kịp lên tầng tiếp theo, ninja sẽ bị phơi bày ngay giữa luồng ánh sáng chói lọi.

---

### Phân cảnh 7: Đỉnh Tháp & Khám Phá Truyền Thuyết Master Tetsuji (09:30 - 10:58)
* **Thu thập trọn vẹn 3 Cuộn Giấy Hisomu (Tetsuji's Lore):**
  * Lời ngâm vịnh truyền thuyết về Master Tetsuji — vị Ninja huyền thoại đầu tiên của tộc Hisomu:
    1. *"Let me tell you stories of the birth of the mighty Hisomu clan, from the time of our first and greatest master, Tetsuji."*
    2. *"I tremble before Tetsuji, greatest ninja of clan Hisomu."*
    3. *"The master accepts any student with the wit to uncover him."*
    4. *"The glint of shuriken reflects as fear in the eyes of Tetsuji's prey."*
* **Thoát hiểm qua đường ống thông gió đỉnh tháp:** Hoàn thành nhiệm vụ chọc thủng phòng tuyến ngoại vi, mở đường thâm nhập vào tòa nhà trung tâm của Karajan.

---

## 2. Bản Đồ Kiến Trúc Kết Nối Màn Chơi (Level Connectivity ASCII Graph)

```
[ PERIMETER TRENCH ] (Destroy Lights 1-5)
         |
         v
+-----------------------------+
| OUTER COURTYARD             | ===(Speedrun Corridor)===> +------------------------+
| - First Guard Patrols       |                           | TRANSFORMER SUB-STATION|
| - Wall Vaults & Darts       |                           | - Sub-minute Challenge |
+-----------------------------+                           | - Disable Outer Grid   |
         |                                                +------------------------+
         v                                                             |
+----------------------------------------------------------------------+
|
v
+-----------------------------+
| UNDERGROUND STORAGE         |
| - CHALLENGE ROOM #1         | ===> [ Heavy Crate Dragging ]
| - 4-Tick Red Laser Grid     | ===> [ Scroll #1: Master Tetsuji ]
+-----------------------------+
         |
         v
+-----------------------------+
| STAIRWELL & SERVICE VENTS   |
| - Under-Stair Blindspots    |
| - Bush Camouflage (Garden)  |
| - Dual Distraction Setup    |
+-----------------------------+
         |
         v
+-----------------------------+
| SECURITY TOWER BASE         |
| - Generator Sabotage        |
| - Radio Breaker Reset Cycle |
| - Artifacts #2 & #3         |
+-----------------------------+
         |
         v
+-------------------------------------------------+
| TOWER APEX & EXTRUSION DUCT                     |
| - Undetected Stealth Seal Checkpoint            |
| - Scroll #3: The Glint of Shuriken              |
| - Extraction to Karajan's Executive Penthouse   |
+-------------------------------------------------+
```

---

## 3. Bảng Ma Trận Kỹ Thuật: So Sánh Tiến Trình Level 1 vs Level 2

| Thành phần thiết kế | Level 1: "Ink & Dreams" (Cổ điển) | Level 2: "Breaching the Perimeter" (Hiện đại) | Ý nghĩa Thiết kế Game (Design Implication) |
|---|---|---|---|
| **Môi trường & Bối cảnh** | Đền gỗ, sân gạch, bình phong, lồng đèn giấy | Tường bê tông, tháp thép, hàng rào laser, ống dẫn khí | Nâng cấp độ nguy hiểm thị giác, tạo cảm giác đối đầu với công nghệ tối tân |
| **Vũ khí & Trang bị** | Tay không ➔ Phi tiêu Darts cơ bản | Darts + **Noisemaker chủ động** | Người chơi có khả năng tạo nguồn phát âm thanh ở mọi tọa độ tùy ý |
| **Chướng ngại vật chính** | Tầm nhìn lính gác, ánh sáng đèn lồng | **Lưới tia laser cảm biến đỏ (Laser Grids)** | Không chỉ ẩn nấp trước mắt lính mà còn phải giải mã chướng ngại vật điện tử |
| **Tương tác vật lý** | Chui ống thông gió, đu xà gồ | **Kéo/đẩy thùng hàng kim loại (`B Button Drag`)** | Thêm chiều sâu giải đố vật lý che chắn nón chiếu laser |
| **Cơ chế Ánh sáng** | Phá đèn thụ động để tạo bóng tối | **Chỉ tiêu phá 20 đèn + Cầu dao tự khôi phục** | Đưa việc quản trị ánh sáng thành tài nguyên chiến thuật có giới hạn thời gian |
| **Trạng thái AI Kẻ địch** | Đi tuần cơ bản, nhìn thấy/nghe thấy | **Đàm thoại bộ đàm, đi sửa cầu dao điện** | AI có tính tổ chức quân sự, liên lạc hỗ trợ nhau |
| **Thử thách Con dấu (Seals)** | Gõ 4 chuông phong thủy | Phá 20 đèn, Speedrun < 60s, Leo tháp vô hình | Đo lường kỹ năng toàn diện: Định tuyến, Tốc độ, và Kỹ năng ẩn mật tuyệt đối |

---

## 4. Hệ Thống Điểm Thưởng & Danh Dự (Score & Honor Economics)

Bảng điểm tổng kết thực nghiệm của màn chơi Level 2 minh chứng cho triết lý thiết kế khuyến khích **Lối chơi Bóng ma hoàn hảo (Pure Ghost Mastery)**:

```
+---------------------------------------------------------+
|                  MISSION RESULTS: LEVEL 2               |
+---------------------------------------------------------+
|  Base Mission Score:                            10,850  |
|  Undetected Bonus (Vượt qua an toàn):          + 6,000  |
|  Distracted Bonus (17 lần đánh lạc hướng x 400):+ 6,800  |
|  No Alarms Raised (Không hề kích hoạt còi):    + 3,000  |
|  No Enemies Killed (Không tước đoạt sinh mạng):+ 5,000  |
+---------------------------------------------------------+
|  TOTAL SCORE:                                   31,650  |
|  TOTAL HONOR:                                    9 / 9  |
+---------------------------------------------------------+
```

> [!TIP]
> **Điểm mấu chốt của hệ thống tính điểm:**
> Người chơi không giết bất kỳ ai nhưng đạt tới **31,650 điểm** nhờ tối đa hóa việc **đánh lạc hướng (`Distracted Bonus = 6,800`)**. Điều này chứng minh *Mark of the Ninja* thiết kế AI không phải để người chơi tránh né càng xa càng tốt, mà khuyến khích người chơi **"khiêu vũ quanh kẻ thù"**: áp sát thật gần, gieo rắc ảo giác âm thanh, làm lính quay cuồng rồi lướt qua trong gang tấc.

---

## 5. Hướng Dẫn Hiện Thực Hóa Trên Godot Engine 4.x (`godot-ai`)

Dưới đây là 3 mã nguồn GDScript chuẩn chỉnh để lập trình lại các cơ chế đột phá của Level 2:

### 5.1. Hệ Thống Tia Laser Cảm Biến Nhịp Điệu (LaserBeam2D.gd)
Tia laser sử dụng `RayCast2D` để quét va chạm theo thời gian thực. Nếu phát hiện Thùng hàng chắn ngang, tia laser sẽ co ngắn lại và không kích hoạt báo động. Tia laser có chu kỳ nhấp nháy 4 nhịp âm thanh.

```gdscript
extends Node2D
class_name LaserBeam2D

@export var max_length: float = 800.0
@export var is_cycling: bool = false
@export var active_time: float = 3.0
@export var inactive_time: float = 1.5

@onready var raycast: RayCast2D = $RayCast2D
@onready var line: Line2D = $Line2D
@onready var alarm_audio: AudioStreamPlayer2D = $AlarmAudio
@onready var tick_audio: AudioStreamPlayer2D = $TickAudio

var is_active: bool = true
var cycle_timer: float = 0.0

func _ready() -> void:
    raycast.target_position = Vector2.DOWN * max_length

func _physics_process(delta: float) -> void:
    if is_cycling:
        _process_cadence(delta)
    
    if not is_active:
        line.visible = false
        raycast.enabled = false
        return
        
    line.visible = true
    raycast.enabled = true
    
    if raycast.is_colliding():
        var collider = raycast.get_collider()
        var hit_point = raycast.get_collision_point()
        line.points = [Vector2.ZERO, to_local(hit_point)]
        
        # Nếu trúng Player và Player không được che chắn
        if collider.is_in_group("player") and not collider.get("is_hidden"):
            trigger_security_alarm(hit_point)
    else:
        line.points = [Vector2.ZERO, Vector2.DOWN * max_length]

func _process_cadence(delta: float) -> void:
    cycle_timer += delta
    if is_active and cycle_timer >= active_time:
        is_active = false
        cycle_timer = 0.0
    elif not is_active and cycle_timer >= inactive_time:
        is_active = true
        cycle_timer = 0.0
        tick_audio.play() # Phát âm thanh nhịp báo hiệu kích hoạt

func trigger_security_alarm(pos: Vector2) -> void:
    alarm_audio.play()
    get_tree().call_group("security_managers", "trip_alarm", pos)
```

---

### 5.2. Cơ Chế Kéo Thùng Hàng Nặng Che Chắn Laser (DraggableCrate2D.gd)
Thùng hàng có thuộc tính chặn tia laser và cho phép nhân vật bám vào để kéo lê với tốc độ chậm.

```gdscript
extends CharacterBody2D
class_name DraggableCrate2D

@export var drag_speed: float = 80.0
@export var friction: float = 0.2

var is_being_dragged: bool = false
var dragging_player: Node2D = null

func interact_drag(player: Node2D) -> void:
    if not is_being_dragged:
        is_being_dragged = true
        dragging_player = player
    else:
        is_being_dragged = false
        dragging_player = null

func _physics_process(delta: float) -> void:
    if is_being_dragged and dragging_player != null:
        var input_dir = Input.get_axis("move_left", "move_right")
        velocity.x = input_dir * drag_speed
        move_and_slide()
    else:
        velocity.x = lerp(velocity.x, 0.0, friction)
        move_and_slide()
```

---

### 5.3. Thiết Bị Gây Xao Nhãng Chủ Động (Noisemaker.gd)
Thiết bị Noisemaker găm vào tường/sàn và phát ra sóng âm định kỳ, gọi hàm `hear_sound` trên toàn bộ Guard trong bán kính.

```gdscript
extends RigidBody2D
class_name NoisemakerProjectile

@export var pulse_radius: float = 250.0
@export var total_pulses: int = 3
@export var pulse_interval: float = 1.2

@onready var sound_ring_visual: Node2D = $SoundRingVisual

var pulses_left: int
var timer: Timer

func _ready() -> void:
    pulses_left = total_pulses
    body_entered.connect(_on_impact)

func _on_impact(_body: Node) -> void:
    freeze = true # Dính chặt vào bề mặt va chạm
    timer = Timer.new()
    timer.wait_time = pulse_interval
    timer.timeout.connect(_emit_sound_pulse)
    add_child(timer)
    timer.start()
    _emit_sound_pulse()

func _emit_sound_pulse() -> void:
    if pulses_left <= 0:
        queue_free()
        return
        
    pulses_left -= 1
    # Hiệu ứng vòng tròn sóng âm lan tỏa
    sound_ring_visual.expand_ring(pulse_radius)
    
    # Kích thích giác quan AI
    var space_state = get_world_2d().direct_space_state
    var guards = get_tree().get_nodes_in_group("guards")
    for guard in guards:
        var dist = global_position.distance_to(guard.global_position)
        if dist <= pulse_radius:
            guard.investigate_acoustic_source(global_position)
```

---

## 6. Bài Học Kinh Điển Cho Nhà Thiết Kế Game (Key Takeaways)

1. **Escalation (Sự leo thang hợp lý):**  
   Đừng vội đưa toàn bộ vũ khí và công nghệ cao vào màn đầu. Hãy để màn 1 thuần túy tự nhiên, và sang màn 2 mới đưa công nghệ cao vào để tạo sự choáng ngợp và tương phản thẩm mỹ.
2. **Âm thanh là đồng minh số một của Puzzle Design:**  
   Việc cho tia laser phát ra 4 nhịp cơ học giúp người chơi "nghe thấy thời gian", biến việc canh nhịp né laser trở thành một trải nghiệm có nhịp điệu (rhythmic gameplay) thay vì canh mắt mỏi mệt.
3. **Đừng để công tắc an ninh chết vĩnh viễn:**  
   Khi người chơi tắt cầu dao, việc cho lính gác nói qua bộ đàm và đi bật lại cầu dao vừa mang tính chân thực cao độ (đội bảo an chuyên nghiệp), vừa tạo áp lực thời gian tự nhiên mà không cần đặt một đồng hồ đếm ngược vô duyên trên HUD.
