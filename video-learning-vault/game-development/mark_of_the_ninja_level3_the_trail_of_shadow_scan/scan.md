# 🔍 Scan Màn Chơi: Level 3 — "The Trail of Shadow"

> **Dự án đối chiếu:** Game 2D stealth về Đặc công Việt Nam (Single-player, 1 nhân vật điều khiển chính, Leader NPC hướng dẫn từ vòng ngoài, đồng đội xuất hiện tại các điểm chốt).  
> **Tài liệu nguồn:** [sources.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level3_the_trail_of_shadow_scan/sources.md)  
> **Video khảo sát:** [theRadBrad — Level 3: The Trail of Shadow (23:56)](https://www.youtube.com/watch?v=fpTq1-gdKE8)

---

## 1. Nguy Cơ & Cơ Chế Mới So Với Level 1 và Level 2

### ① Chó Nghiệp Vụ (Guard Dogs) — Cơ chế Đánh Hơi Xuyên Bóng Tối
* **Quan sát trực tiếp:** 
  - Tại phút `01:30`, HUD hiển thị bảng hướng dẫn chính thức: *"They can sniff you out, even in the darkness."*
  - Khác với lính người chỉ phụ thuộc vào tầm nhìn ánh sáng/nón đèn pin, chó nghiệp vụ có cơ chế phát hiện dựa trên mùi/khứu giác. Khi người chơi đứng trong bóng tối hoàn toàn, chó vẫn có thể lần theo mùi và sủa báo động nếu ở cự ly gần.
  - Tại phút `17:30`, quan sát thấy trạng thái ngủ của chó (`Sleeping State`): Khi lính chưa bị đánh động, chó nằm ngủ cạnh chân lính; chỉ khi có tiếng ồn hoặc báo động thì chó mới tỉnh dậy và lùng sục.
  - Tại phút `02:36`, người chơi thực hiện thao tác tiêu diệt được chó bằng đòn cận chiến/ám sát (`DROP BODY`).
* **Lời người chơi:** 
  - `01:30`: *"Steer clear of the dogs, they can sniff you out even in the darkness... I'm not even going to do that, there's no point, you get more of a reward to not do that."*
  - `02:36`: *"Killed a f***ing dog, unreal, absolutely unreal, I can't believe I just did that!"*
  - `17:30`: *"All right, so the dog is sleeping, that's how he is by default. So if you don't alert the guards, then there's no reason for that problem."*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Chó săn là khắc tinh trực tiếp của cơ chế ẩn nấp bóng tối ("Binary Lighting"): Nó tước bỏ tính an toàn tuyệt đối của bóng tối, buộc người chơi phải giữ khoảng cách vật lý thay vì chỉ dựa vào bóng râm.
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Bổ sung chó béc-giê tuần tra tại các kho xăng, bốt canh vòng ngoài căn cứ địch. Khi gặp chó, đặc công không thể chỉ núp lùm cây trong bóng đêm mà phải đi trên cành cây cao, lội qua mương nước để cắt mùi, hoặc dùng mồi tẩm thuốc/hương liệu đánh lạc hướng khứu giác.

---

### ② Bom Khói (Smoke Bomb) — Vô Hiệu Hóa Tầm Nhìn & Làm Nhiễu Tia Laser
* **Quan sát trực tiếp:** 
  - Tại phút `15:04`, người chơi nhặt trang bị mới tại Nhà số 6. HUD hiển thị: *"The smokebomb gives you cover from enemies. It can also disrupt laser beams, letting you sneak right past them!"*
  - Tại phút `17:15`, người chơi ném bom khói vào một hành lang có lưới laser đỏ gắn súng máy tự động: Khói bốc lên làm mờ/phân tán chùm tia laser, cho phép nhân vật chạy xuyên qua mà súng không khai hỏa.
* **Lời người chơi:** 
  - `15:04`: *"Smoke bomb, nice! It's left behind some equipment. The smoke bomb gives you cover from enemies, it can also disrupt their laser beams... letting you sneak right through traps."*
  - `17:15`: *"So these lasers got the ones with the guns on them, so we pretty much have to throw a smoke bomb in here to get through the door on the right."*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Bom khói đóng vai trò công cụ đa năng 2-trong-1: Vừa tạo màn che thị giác trước mắt lính tuần tra, vừa làm gián đoạn hệ thống cảm biến quang học công nghệ cao.
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Quả nổ khói ngụy trang của đặc công: Dùng để vượt qua khoảng sân trống có đèn pha quét qua hoặc che mắt ụ súng máy tự động khi phải băng qua cửa hẹp.

---

### ③ Lính Mang Khiên Chống Bạo Động (Riot Shield Guard)
* **Quan sát trực tiếp:** 
  - Tại phút `07:49`, xuất hiện loại lính mới cầm khiên chắn lớn phía trước. HUD hiển thị lời nhắc: *"As long as he has his shield to hide behind, I wouldn't take him head on. Try to get behind him."*
  - Khi đối mặt trực diện, người chơi không thể thực hiện đòn tấn công xuyên qua khiên.
* **Lời người chơi:** 
  - `07:49`: *"Nice, as long as he has a shield to hide behind, I wouldn't take him head on, try to get behind him... I heard something, maybe I'm just hearing things, I got to get behind this guy, so obviously I need a noisemaker."*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Lính mang khiên định hướng góc phòng thủ 180 độ: Buộc người chơi phải chuyển hướng tấn công ra phía sau lưng (bằng Noisemaker đánh lạc hướng hoặc luồn qua ống gió).
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Lính tuần tra trang bị giáp ngực chống đạn hoặc khiên chống bạo động: Không thể tiêu diệt trực diện từ đằng trước, buộc phải tiếp cận đánh tỉa từ sau lưng hoặc dùng bẫy chông dưới đất.

---

## 2. Cách Màn Chơi Kết Hợp Những Cơ Chế Đã Biết

* **Chó Săn + Lính Tuần Tra Đi Kèm:** 
  - Chó đi cùng lính gác (phút `02:30`, `14:00`, `17:30`). Nếu đánh động lính, chó sẽ thức dậy; nếu chó ngửi thấy mùi và sủa, lính sẽ lập tức rút súng chuyển sang trạng thái sẵn sàng chiến đấu.
* **Lưới Laser Đỏ Gắn Tháp Pháo Tự Động (Turret Lasers):** 
  - Khác với Level 2 (laser chỉ báo động hoặc di chuyển trong phòng giải đố), tại Level 3 laser được liên kết trực tiếp với súng tự động treo trần: Chạm vào tia đỏ là súng xả đạn tiêu diệt ngay lập tức. Người chơi phải kết hợp bom khói hoặc ngắt công tắc điện để vượt qua.
* **Noisemaker + Bọc Sườn Lính Khiên:** 
  - Sử dụng Noisemaker ném ra sau lưng lính khiên để lính quay mặt lại điều tra, mở ra khoảng trống phía sau để người chơi tiếp cận ám sát hoặc luồn qua cửa.

---

## 3. Tình Huống Mắc Lỗi, Bị Phát Hiện & Chuỗi Rút Lui (Extraction / Escape)

* **Sai lầm & Báo động ở phút 20:17 – 20:36:**
  - *Diễn biến:* Sau khi đặt thiết bị theo dõi (`Tracking Device`), hệ thống báo động toàn căn cứ bị kích hoạt: *"They're coming, RUN!"*
  - *Tín hiệu:* Còi báo động rú liên hồi, đèn đỏ xoay quanh, lính đổ ra hành lang truy đuổi. Điểm số bị phạt báo động.
* **Pha Rút Lui Khẩn Cấp (Phút 20:36 – 23:26):**
  - *Quan sát trực tiếp:* Game chuyển sang trạng thái tháo chạy (Chase/Escape phase). Lối vào ban đầu đã bị phong tỏa bằng cửa thép đóng sập. Người chơi buộc phải tìm đường thoát hiểm theo trục thẳng đứng: Đu dây leo lên các đường ống thông gió trên cao (`ventilation shaft`), liên tục chuyền cành qua các mỏm xà trong khi phía dưới lính xả súng đuổi theo.
  - Tại phút `23:22`, người chơi nhảy ra khỏi lỗ thông hơi nóc nhà an toàn, hoàn thành màn chơi.
* **Lời người chơi:** 
  - `20:19`: *"Open the doors quick! The tracking device is in there. They're coming, run! Someone up there, down, I got to get the f*** out of here in a hurry!"*
  - `21:23`: *"All right, so I think I found an exodus at the very top of this ventilation shaft... Come on, come on... what are we trying to get to?"*
  - `23:22`: *"My escape was not pretty, but we managed! Good work, now he can't get away from us."*
* **Bài học cho Game Đặc công:** 
  - **Phân kỳ cấu trúc màn chơi thành 2 pha rõ rệt:** 
    - *Pha Thâm nhập (Infiltration):* Chậm rãi, kín đáo, tránh chó và lính khiên để đặt bộc phá/gắn thiết bị mật.
    - *Pha Rút lui (Exfiltration/Escape):* Sau khi hoàn thành mục tiêu, căn cứ báo động đỏ; cửa chính bị chặn, mở ra tuyến đường thoát hiểm thứ hai (đường hầm bí mật hoặc mái nhà).

---

## 4. 4 Timestamp Đáng Phân Tích Sâu (Candidate Timestamps)

| Timestamp | Tên phân đoạn | Lý do đề xuất phân tích sâu cho Lượt 2 |
|---|---|---|
| **`01:30 - 02:40`** | Giới thiệu chó nghiệp vụ & Cơ chế khứu giác | Phân tích chính xác bán kính ngửi mùi của chó khi người chơi ở trong bóng tối; cơ chế chuyển trạng thái từ ngủ sang tỉnh dậy khi có tiếng bước chân. |
| **`07:49 - 08:30`** | Cơ chế Lính mang khiên & Điều hướng đánh lạc hướng | Xem xét góc quét 180 độ của khiên; khoảng cách ném Noisemaker cần thiết để ép lính xoay lưng lại. |
| **`15:04 - 17:35`** | Bom khói & Vô hiệu hóa bẫy laser súng máy | Đo đạc thời gian tồn tại của đám khói (smoke duration) và ngưỡng an toàn khi băng qua chùm tia laser. |
| **`20:17 - 23:26`** | Chuỗi rút lui khẩn cấp (Alarm & Vertical Escape) | Nghiên cứu cách game hướng dẫn người chơi tìm lối thoát hiểm khi toàn bộ căn cứ bị phong tỏa và báo động đỏ. |

---

## 5. Điều Chưa Biết & Bằng Chứng Cần Bổ Sung

1. **Khả năng triệt hạ chó săn trong im lặng:** Ở phút `02:36` người chơi cận chiến giết chó nhưng gây tiếng ồn; chưa rõ có đòn stealth kill im lặng hoàn toàn cho chó săn như với lính người không.
2. **Tác động của bom khói lên chó săn:** Liệu bom khói có làm tê liệt khứu giác của chó săn không, hay chó vẫn đánh hơi được người chơi bên trong màn khói?
3. **NPC Hỗ Trợ (Ora / Đồng Đội):** 
   - *Ghi nhận:* Ora xuất hiện trong các đoạn đối thoại cutscene và nhắc nhở mục tiêu qua bộ đàm.
   - *Hạn chế:* Trong suốt màn chơi thực tế, **không có đồng đội AI nào chiến đấu cùng**. Mọi hành động chiến đấu hoàn toàn là độc hành (single character).
