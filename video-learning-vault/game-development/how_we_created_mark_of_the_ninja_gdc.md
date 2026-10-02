# 🥷 How We Created Mark of the Ninja: Kỹ nghệ Thiết kế Game Stealth 2D, Feedback Trực quan & Kiến trúc Thử nghiệm Giả thuyết (GDC Breakdown)

> **Nguồn tư liệu:** [How We Created Mark of the Ninja Without (Totally) Losing Our Minds — GDC](https://www.youtube.com/watch?v=A7ejh3YUbac)  
> **Diễn giả:** Jamie Cheng (Founder & CEO, Klei Entertainment) & Jeff Agala (Chief Creative Officer & Director, Klei Entertainment)  
> **Thời lượng:** 26:50 | **Transcript gốc:** [how_we_created_mark_of_the_ninja_gdc_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/game-development/how_we_created_mark_of_the_ninja_gdc_transcript.txt)  
> **Danh mục:** 🎮 Game Development & Engineering | **Game tham chiếu:** *Mark of the Ninja* (Metacritic: 91/100)

---

## Executive Summary: Chuẩn mực Vàng của Game Stealth 2D

Trước khi *Mark of the Ninja* ra mắt vào năm 2012, thể loại lén lút (Stealth) gần như được coi là "lãnh địa bất khả xâm phạm" của không gian 3D (*Thief, Splinter Cell, Metal Gear Solid, Dishonored*). Hầu hết các thử nghiệm làm stealth trên nền tảng 2D đều thất bại ê chề vì người chơi bị giới hạn tầm nhìn, dễ chết bất đắc dĩ bởi những kẻ địch ngoài màn hình (*off-screen unfair detection*), và biến game thành chuỗi "thử và sai" (*trial-and-error*) ức chế.

Klei Entertainment đã giải quyết triệt để bài toán này, biến *Mark of the Ninja* thành một kiệt tác với điểm số 91/100.

Bài chia sẻ tại GDC của Jamie Cheng và Jeff Agala không nói về lý thuyết viển vông, mà giải phẫu chính xác:
1. **Lý do dự án từng suýt bị hủy bỏ** và rơi vào bẫy "Lạc lối trong khu rừng sai" (*The Wrong Forest*).
2. **Bước ngoặt cứu vãn game:** Chuyển đổi từ thiết kế dựa trên linh cảm (*vague vision*) sang **Phát triển dựa trên Giả thuyết Khoa học (*Hypothesis-Driven Game Design*)**.
3. **Bản chất của Game Stealth:** Không phải là hành động nhanh tay lẹ mắt, mà là **Sự Lập Kế Hoạch (*Planning*)** dựa trên **Quan hệ Nhân - Quả có thể dự đoán (*Predictable Cause and Effect*)**.
4. **Hệ thống cơ chế kỹ thuật cụ thể:** Ánh sáng nhị phân (*Binary Lighting*), Vòng sóng âm thanh trực quan (*Sound Rings*), và triết lý loại bỏ AI thông minh để dùng **AI Đơn giản, Dự đoán được (*Stupid Predictable AI*)**.

```
+-----------------------------------------------------------------------------------------+
|                  CORE GAMEPLAY LOOP OF MARK OF THE NINJA                                |
|                                                                                         |
|       [ OBSERVE ]  =====>  [ WORK FOR INFO ]  =====>  [ PLAN ]  =====>  [ EXECUTE ]     |
|   (Quan sát phòng)         (Nấp, nghe lén,            (Vẽ lộ trình,     (Ám sát/Lướt qua|
|                             soi tầm nhìn)             dùng đồ chơi)      chính xác 100%)|
|           ^                                                                    |        |
|           +==================== [ PREDICTABLE FEEDBACK ] ======================+        |
+-----------------------------------------------------------------------------------------+
```

---

## 1. Cuộc khủng hoảng "Khu rừng Sai" (The Wrong Forest Dilemma)

Sau khi hoàn thành *Shank 1*, đội ngũ Klei rơi vào tình trạng kiệt sức (*burnout*) trầm trọng. Jamie Cheng nhận ra rằng sự lãng phí lớn nhất trong phát triển game không phải là code chậm hay vẽ lâu, mà là **Xây dựng điều hoàn toàn sai (*Building the Wrong Thing*)**.

> **Ẩn dụ "Khu rừng sai" (The Forest Analogy):**  
> *"Một nhóm thợ đốn cây đang ra sức chặt cây hùng hục dưới mặt đất. Một người trèo lên ngọn cây cao nhất nhìn ra xa và hét lớn: 'Này các cậu, chúng ta đang chặt nhầm rừng rồi!'. Nhưng những người bên dưới trả lời: 'Kệ đi, chúng ta đang đốn được rất nhiều cây ở đây, đổi sang rừng khác là lãng phí công sức!'"*

```
[ Tháng 3 - Tháng 5/2011: Khởi đầu thất bại ]
  - Tầm nhìn ban đầu: "Một ninja bằng thủy tinh (Glass Cannon), thoắt ẩn thoắt hiện trong bóng tối, địch không hề hay biết."
  - Đã lập trình đầy đủ: Shuriken, Ám sát lén lút (Stealth kills), Hệ thống sáng/tối, Tiếng động.
  - HỆ QUẢ: Gameplay cảm giác cực kỳ nhão nhoét, lộn xộn và dở tệ ("Mushy and sucked").
  - PHẢN ỨNG SAI LẦM: Nghĩ rằng 2D Stealth không khả thi, team đổi hướng biến game thành "Action Game kết hợp Stealth".
  - KẾT CỤC: Game càng trở nên tệ hại hơn nữa!
```

Lý do thất bại: Toàn bộ tính năng được xây dựng trên **những giả định chưa qua kiểm chứng (*untested assumptions*)** thay vì những giả thuyết có thể đo lường.

---

## 2. Phương pháp Thiết kế Game Dựa trên Giả thuyết (Hypothesis-Driven)

Để thoát khỏi vũng lầy, Klei đảo ngược quy trình. Thay vì lập trình một hệ thống lớn rồi hy vọng nó vui, họ viết ra các **Giả thuyết có thể kiểm thử (Testable Hypotheses)** và dựng hơn 24 nguyên mẫu vi mô (*micro-scenarios*) độc lập để thất bại thật nhanh (*fail quickly*):

* *Giả thuyết 1:* "Nếu người chơi nấp sau một chiếc bình hoa và tên lính đi ngang qua sát bên, người chơi sẽ cảm thấy lén lút và hồi hộp." ➔ **Kiểm thử:** Dựng đúng 1 phòng thử nghiệm với 1 bình hoa và 1 lính.
* *Giả thuyết 2:* "Nếu người chơi không biết vị trí địch trong phòng kế bên nhưng có thể ghé tai nghe ngóng qua khe cửa để thấy hình bóng mờ, việc 'phải nỗ lực để có thông tin' sẽ tạo cảm giác như một ninja thực thụ." ➔ **Thành công rực rỡ!**
* *Giả thuyết 3:* "Nếu người chơi có thể đỡ đòn và phản công cận chiến kiểu Assassin's Creed / Shank, game sẽ hấp dẫn hơn." ➔ **Thất bại!** Khi nhân vật cận chiến quá mạnh, người chơi sẽ bỏ qua cơ chế lén lút và lao vào chém nhau. Mechanic này bị xóa sổ ngay lập tức.

---

## 3. Bản chất của Stealth: Lập Kế Hoạch & Quan Hệ Nhân - Quả

Từ 24 kịch bản thử nghiệm, Klei đúc kết ra chân lý nền tảng:
> **Về mặt cơ chế, bản chất của game Stealth KHÔNG PHẢI LÀ PHẢN XẠ NHANH, mà là KHẢ NĂNG LẬP KẾ HOẠCH (PLANNING).**

Người chơi muốn:
1. Quan sát môi trường một cách an toàn.
2. Nỗ lực thu thập thông tin (*Work for information*).
3. Lên phương án: *"Mình sẽ ném phi tiêu tắt bóng đèn ➔ Tên lính nghe tiếng vỡ sẽ quay lại nhìn ➔ Mình sẽ đu dây qua đầu hắn để chui vào ống thông gió."*
4. Thực thi kế hoạch đó mà không bị bất kỳ yếu tố ngẫu nhiên vô lý nào phá hỏng.

Để người chơi có thể lập kế hoạch trước 3 - 5 bước, thế giới trong game bắt buộc phải có **Quan hệ Nhân - Quả có thể dự đoán 100% (*Predictable Cause and Effect*)**. Mọi sự mập mờ, xác suất may rủi đều biến game stealth thành thảm họa.

---

## 4. Giải phẫu Cơ chế Kỹ thuật để Tái tạo Game (Game Cloning Blueprint)

Khi bạn muốn code một tựa game tương tự *Mark of the Ninja* (bằng Godot Engine hoặc bất kỳ framework nào), đây là 5 cơ chế kỹ thuật bắt buộc phải triển khai chính xác:

### 4.1. Hệ thống Ánh sáng Nhị phân (Binary Lighting System)
* **Vấn đề trong thế giới thực:** Ánh sáng có độ suy giảm mềm (*soft gradients, penumbra*). Người chơi đứng ở vùng nửa sáng nửa tối không biết mình đã bị lộ hay chưa.
* **Giải pháp của Klei:** Chuyển toàn bộ ánh sáng thành **Nhị phân (0 hoặc 1)**.
  * Nếu nhân vật bước 1 ngón chân vào quầng sáng ➔ Trạng thái = **LIT (1)**. Toàn bộ sprite hiển thị đầy đủ màu sắc, chi tiết trang phục.
  * Nếu đứng trong bóng tối ➔ Trạng thái = **HIDDEN (0)**. Sprite tự động chuyển thành bóng đen hoàn toàn (*black/blue silhouette*).
* **Giá trị logic:** Người chơi biết chắc chắn 100% ở từng khung hình mình có tàng hình trước mắt kẻ địch hay không.

```
       BÓNG TỐI (Shadow)                     ÁNH ĐÈN (Light Source)
   +-----------------------+              +---------------------------+
   | Trạng thái: HIDDEN    |              | Trạng thái: LIT           |
   | Visual: Silhouette    |  ==== Bước chân ===> | Visual: Full Color Sprite |
   | Guard: Không thấy     |    vào quầng | Guard: Phát hiện tức thì  |
   +-----------------------+      sáng    +---------------------------+
```

### 4.2. Vòng Sóng Âm thanh Trực quan (Sound Rings Propagation)
* **Vấn đề:** Trong game truyền thống, âm thanh là vô hình. Người chơi không biết tiếng bước chạy, tiếng vỡ bóng đèn lan xa bao nhiêu mét.
* **Giải pháp:** Khi bất kỳ hành động nào phát ra âm thanh, vẽ một **vòng tròn mở rộng (*expanding circular ring*)** trên màn hình đại diện cho phạm vi lan truyền sóng âm.
  * Chạy trên sàn gỗ: Vòng âm thanh bán kính $R = 8m$.
  * Rón rén đi bộ: $R = 0m$ (Hoàn toàn im lặng).
  * Phi tiêu đập vỡ bóng đèn: Vòng âm thanh $R = 12m$.
  * Chạy trên thảm cỏ/thảm vải: Vòng âm thanh $R = 3m$.
* **Đập tan định kiến "Làm game quá dễ":** Ban đầu team lo ngại rằng hiển thị vòng âm thanh sẽ làm mất độ khó. Thực tế, khi thông tin minh bạch, người chơi chuyển từ việc *"đoán mò xem lính có nghe thấy không"* sang việc **chủ động dùng âm thanh như vũ khí đánh lạc hướng** (tạo tiếng động tại điểm A để dụ lính rời khỏi điểm B).

```
   [ Lọ gốm vỡ ] 
        ( ( ( ( ( ( O ) ) ) ) ) )  <--- Vòng sóng âm bán kính R
                      \
                       +---> Chạm vào tai [ Tên lính gác ]
                             => Trạng thái chuyển: UNAWARE -> SUSPICIOUS
                             => Quay đầu lại và bước về tâm vòng tròn!
```

### 4.3. Nguyên lý "AI Ngốc nghếch" (Stupid Predictable AI)
* **Sai lầm ban đầu:** Lập trình viên của Klei tốn hàng tháng trời xây dựng hệ thống AI cực kỳ phức tạp và thông minh (tự tìm đường vòng, phán đoán vị trí ẩn nấp của ninja, bọc lót chiến thuật).
* **Kết quả:** Người chơi không thể hiểu nổi con AI đang làm cái quái gì! Game trở nên ức chế vì AI hành xử thất thường.
* **Giải pháp:** **Vứt bỏ AI thông minh, thay bằng AI đơn giản, ngốc nghếch và dễ bị thao túng (*Highly Manipulatable*)!**
  * Tiết kiệm **4 - 5 tháng công sức lập trình**.
  * Tạo ra Finite State Machine rõ ràng:

```
               [ UNAWARE (Tuần tra bình thường) ]
                                |
                 Nghe tiếng động / Thấy bóng mờ
                                v
               [ SUSPICIOUS (Điều tra nguồn phát) ]
                    /                        \
          Hết thời gian / Không thấy     Nhìn thấy trực diện
                  /                            \
                 v                              v
        [ Trở lại Tuần tra ]          [ ALERT / HOSTILE (Tấn công) ]
                                                |
                                      Mất dấu trong bóng tối
                                                v
                                      [ SEARCHING (Sục sạo) ]
                                                |
                                      Thấy xác đồng đội treo cổ
                                                v
                                      [ PANICKED (Hoảng loạn) ]
                               (Xả súng mù quáng, bắn chết cả phe mình)
```

### 4.4. Ngôn ngữ Thị giác: Biểu tượng Cảm xúc trên đầu Lính (Awareness Icons)
* Ban đầu Klei thử diễn hoạt biểu cảm khuôn mặt (*facial animation*), nhưng kích thước nhân vật trên màn hình 2D quá nhỏ khiến người chơi không nhìn thấy.
* Giải pháp tối ưu: **Đặt Icon trạng thái ngay trên đầu kẻ địch**:
  * Dấu chấm hỏi (`?`): Nghi ngờ, đang đi kiểm tra.
  * Dấu chấm than (`!`): Đã phát hiện ninja, báo động.
  * Biểu tượng mắt mở / nhắm: Thể hiện tầm nhìn đang bị che khuất hay thông thoáng.
  * Biểu tượng đầu lâu/sợ hãi: Trạng thái hoảng loạn bắn bừa.

### 4.5. Mặt phẳng 2D Thuần túy & Quỹ đạo Ngắm Bắn Tuyệt đối (Precision Trajectory)
* Không dùng chiều sâu 2.5D gây nhầm lẫn. Mọi bức tường, trần nhà, thanh xà đều phẳng và có quy tắc rõ ràng:
  * Trần nhà bằng gỗ: Bắn móc câu (*grappling hook*) đu lên được.
  * Trần nhà bằng kim loại trơn: Móc câu bị trượt.
  * Ống thông gió: Chui vào là tàng hình tuyệt đối.
* Khi ngắm phi tiêu hoặc móc câu: Game kích hoạt hiệu ứng làm chậm thời gian (*focus mode*), vẽ đường cong tia ngắm chính xác đến từng pixel, triệt tiêu mọi lỗi chết do bấm trượt nút.

---

## 5. Quy trình Sản xuất Màn chơi (Level Design Pipeline)

Klei gặp một bài toán hóc búa về quy trình cộng tác giữa Game Designer và Artist khi thiết kế level:

```
[ QUY TRÌNH CŨ (THẤT BẢI) ]:
Game Designer dựng không gian hình học trừu tượng (hộp vuông, bục nhảy)
       |
       v
Chuyển cho Artist vẽ đè lên
       |
       v
BẾ TẮC: Artist không thể biến một ma trận hộp vô nghĩa thành một thế giới sống động!
(Đây là cống ngầm, tòa nhà chọc trời hay căn cứ quân sự? Vị trí các phòng phi lý!)
```

```
[ QUY TRÌNH MỚI (THÀNH CÔNG VƯỢT TRỘI) ]:
Artist thiết kế Cấu trúc Kiến trúc trước (Biết rõ cần có phòng, cửa, ống thông gió)
       |
       v
Chuyển cho Game Designer: Đặt lính gác, bẫy laser, đồ chơi vào các căn phòng thực tế
       |
       v
Artist thực hiện bước hoàn thiện mỹ thuật cuối cùng (Polish & Lighting pass)
       |
       v
KẾT QUẢ: Màn chơi vừa có tính mỹ thuật cao, vừa mang cấu trúc stealth chặt chẽ!
```

> **Bài học cốt lõi:** *"Process Effectiveness > Individual Efficiency"*. Quy trình này thoạt nhìn tốn công hơn (Artist phải làm việc 2 lần), nhưng hiệu quả toàn cục của sản phẩm cuối cùng lại cao gấp nhiều lần.

---

## 6. Lưỡi gươm Sắc bén (Finely Sharpened Blade) vs Cây Bonsai (Bonsai Tree)

Jamie Cheng đưa ra một so sánh triết học sâu sắc giữa hai tựa game huyền thoại của Klei:

| Đặc tính | Mark of the Ninja (Lưỡi gươm sắc bén) | Don't Starve (Cây Bonsai) |
|---|---|---|
| **Bản chất thiết kế** | **Trừ bớt tính năng (Subtractive Design)** | **Bồi đắp hệ thống (Additive / Emergent Systems)** |
| **Quy tắc vận hành** | Gọt giũa từng milimet cho đến khi sắc lẹm. Nếu thêm một cơ chế thừa (như counter-attack), lưỡi gươm sẽ mẻ gãy. | Các hệ thống lồng ghép vào nhau (thời tiết, thức ăn, lửa, quái vật) tự do sinh sôi nảy nở theo thời gian. |
| **Cách kiểm soát** | Kiểm thử giả thuyết để **cắt bỏ những thứ gây nhiễu kế hoạch**. | Mở rộng hệ sinh thái mô phỏng để người chơi tự khám phá. |

---

## 7. Khung Kỹ thuật (Technical Spec) để Clone Cơ chế trên Godot / Web

Nếu bạn chuẩn bị viết mã cho game tương tự, đây là danh sách thành phần cốt lõi cần lập trình:

1. **Light2D & Visibility Raycast System:**
   - Dùng Node `PointLight2D` hoặc Shader nhị phân.
   - Script `PlayerController`: Bắn `PhysicsRayQueryParameters2D` từ nguồn sáng tới Player. Nếu va chạm không bị che khuất ➔ `is_lit = true; player_sprite.modulate = Color.WHITE`. Ngược lại ➔ `is_lit = false; player_sprite.modulate = Color(0.1, 0.2, 0.4, 1.0)`.
2. **SoundPropagationManager (Autoload / Singleton):**
   - Hàm `emit_sound(origin: Vector2, radius: float, noise_type: String)`:
     - Tạo một vòng tròn mở rộng visual bằng Shader/Node2D `draw_arc`.
     - Lấy danh sách Guard trong bán kính `radius` bằng `get_tree().get_nodes_in_group("guards")`.
     - Nếu khoảng cách `guard.global_position.distance_to(origin) <= radius`: Gửi signal `on_sound_heard(origin)`.
3. **Guard FSM (Finite State Machine):**
   - States: `Patrol`, `Investigate(target_pos)`, `Alert(player_pos)`, `Search`, `Panic`.
   - Vision Cone: Node `Area2D` hình nón (Góc 60 độ, tầm xa 200px).
   - Kiểm tra: Nếu Player ở trong Vision Cone VÀ (`player.is_lit == true` HOẶC khoảng cách `< 40px` ngay cả trong tối) ➔ Chuyển state sang `Alert`.
4. **Interactive Stealth Anchors:**
   - Đèn huỳnh quang: Có HP = 1, bắn phi tiêu trúng ➔ Phát âm thanh vỡ, tắt nguồn sáng tương ứng.
   - Ống thông gió (Vent): Khi Player ấn nút tương tác ➔ Vô hiệu hóa va chạm với Guard, ẩn Sprite, cho phép di chuyển xuyên tường ngầm.

---

## 8. Kết luận & Bài học Vận hành

1. **Không ẩn nấp sau chiêu bài "Nghệ thuật" để làm việc điên cuồng:** Làm game hay không đồng nghĩa với kiệt sức (*crunch*). Bằng cách thử nghiệm giả thuyết sớm và cắt bỏ các tính năng sai từ trong trứng nước, Klei đã tạo ra siêu phẩm mà vẫn giữ được sự cân bằng cuộc sống cho đội ngũ.
2. **Minh bạch thông tin là chìa khóa của Stealth:** Đừng giấu thông tin của người chơi. Hãy cho họ thấy rõ vòng âm thanh, ranh giới bóng tối, và tâm trạng của kẻ địch.
3. **AI Đơn giản đánh bại AI Phức tạp:** Trong game stealth, AI là những "con rối" được thiết kế để người chơi thao túng, không phải là đối thủ chơi cờ siêu cấp.
