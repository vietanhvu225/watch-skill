# 🔍 Scan Màn Chơi: Level 6 — "An Ancestral Home"

> **Dự án đối chiếu:** Game 2D stealth về Đặc công Việt Nam (Single-player, 1 nhân vật điều khiển chính, Leader NPC hướng dẫn từ vòng ngoài, đồng đội xuất hiện tại các điểm chốt).  
> **Tài liệu nguồn:** [sources.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level6_an_ancestral_home_scan/sources.md)  
> **Video khảo sát:** [theRadBrad — Level 6: An Ancestral Home (21:57)](https://www.youtube.com/watch?v=lUQz6zixb80)

---

## 1. Nguy Cơ & Cơ Chế Mới So Với Các Màn Trước

### ① Môi Trường Địa Hình Hầm Mộ Cổ (Ancient Catacombs) & Khí Độc Tự Nhiên (Foul Gas)
* **Quan sát trực tiếp:** 
  - Tại phút `16:09`, khi mặt đất bị lính Hessian phong tỏa gắt gao, Ora chỉ dẫn đi xuống hầm mộ: *"There are guards all over the ground. We should head down to the catacombs, that will be our best shot at getting to the inner keep. Find a route under the castle."*
  - Tại phút `17:24`, trong các buồng hầm mộ sâu xuất hiện những túi khí độc màu xanh lơ lửng (`foul gas / poison air`). Khi nhân vật bước vào làn khí độc, thanh máu bắt đầu tụt dần.
  - Phía đáy hầm mộ xuất hiện các hố nước tù/bùn lầy nguy hiểm (`foul water`, phút `17:43`).
* **Lời người chơi:** 
  - `16:09`: *"We should head down to the catacombs, that will be our best shot... find a route under the castle."*
  - `17:24`: *"There's something wrong with the air down here, when you see that foul gas... I don't know what the hell that stuff is below, I guess it's water."*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Chuyển dịch không gian từ công trình công nghệ cao sang địa đạo ngầm cổ kính: Giới thiệu hiểm họa khí độc môi trường kết hợp với việc leo trèo qua các mỏm đá ngầm, buộc người chơi phải di chuyển nhanh qua các túi khí độc để tìm vùng không khí sạch.
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Hệ thống hầm ngầm, địa đạo (như Địa đạo Củ Chi / Vịnh Mốc) hoặc cống ngầm thoát nước: Có những đoạn ngập nước hoặc tích tụ khí độc, người chơi phải nín thở bơi qua hoặc tìm cửa thông hơi để lấy lại dưỡng khí.

---

### ② Tương Tác Tâm Lý Dẫn Truyện Của NPC Ora — Sự Thật Về Người Mang Hình Xăm
* **Quan sát trực tiếp:** 
  - Không chỉ hướng dẫn đường đi, NPC Ora bắt đầu gieo rắc sự nghi ngờ vào tâm trí nhân vật chính về truyền thống của Sensei Azai và gia tộc:
  - Phút `07:42`: *"Azai refers to you as the champion, but do you know what they used to call the ones who got the mark? ... Once you have the mark, they treat you like an outsider: strange, unpredictable, and dangerous. You're as good as banished."*
* **Lời người chơi:** 
  - `07:40`: *"Search a way into the castle... once you have the mark they treat you like an outsider... you're good as banished."*
  - `13:45`: *"Some of this stuff is really sweet the way they do it, but at the same time this is one of those games that's incredibly engrossing, you really got to be on top of it."*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Kỹ thuật kể chuyện lồng ghép qua đồng đội đồng hành: Tạo chiều sâu tâm lý và sự mâu thuẫn nội tâm cho nhân vật chính trong khi vẫn đang thực thi nhiệm vụ sinh tử.
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Leader NPC vòng ngoài qua các phiên liên lạc vô tuyến hé lộ các chi tiết về hoàn cảnh chiến dịch, số phận của các đồng đội đi trước, tạo động lực tinh thần và chiều sâu cảm xúc cho người chiến sĩ.

---

## 2. Cách Màn Chơi Kết Hợp Những Cơ Chế Đã Biết

* **Tái Xuất Hiện Chó Săn + Cơ Chế Kéo Cần Gạt (07:59 – 08:35):** 
  - Chó nghiệp vụ được bố trí nằm gác ngay cạnh đòn bẩy mở cổng sắt. theRadBrad phải sử dụng vật che chắn, tính toán chu kỳ quay đầu của lính đi kèm để ám sát lính và triệt hạ chó mà không để chó kịp sủa báo động.
* **Mê Cung Không Gian Thẳng Đứng Kết Hợp Laser Ngầm (18:37 – 20:30):** 
  - Dưới hầm mộ tối tăm, kẻ địch lắp đặt lưới tia laser an ninh kết nối với nắp hầm chìm (`Hatch Puzzle`). Người chơi phải luồn qua các kẽ đá, đẩy thùng kim loại chặn chùm laser để kích hoạt công tắc mở nắp hầm thâm nhập tường thành.
* **Bẫy Chông Đặt Sẵn Trong Hẹp:** Tiếp tục được người chơi sử dụng để dọn dẹp lính tuần tra trong các hành lang đá hẹp của lâu đài.

---

## 3. Tình Huống Mắc Lỗi, Lạc Đường & Phục Hồi

* **Lạc đường và mắc kẹt tại hầm mộ (12:05 – 12:25):**
  - theRadBrad mất phương hướng, phải bật bản đồ toàn cảnh (`Look at the map`) để tìm hướng đi xuống catacombs.
* **Lỗi phát hiện và báo động ở phút 05:51 – 06:46:**
  - Người chơi nhảy vội vào phòng có 2 lính đang đứng gần nhau, kích hoạt trạng thái cảnh báo (`Alert Mode`). Lính hô hào gọi viện binh (`Calling for backup`).
  - Phục hồi: theRadBrad lùi lại góc tối, ném Noisemaker sang phía đối diện để tách 2 tên lính ra, sau đó lần lượt hạ gục từng tên khi chúng mất tập trung.

---

## 4. 3 Timestamp Đáng Phân Tích Sâu (Candidate Timestamps)

| Timestamp | Tên phân đoạn | Lý do đề xuất phân tích sâu cho Lượt 2 |
|---|---|---|
| **`07:40 - 08:35`** | Đoạn thoại Ora & Tiêu diệt chó gác cần gạt | Phân tích cách game đan cài thoại cốt truyện vào ngay trước một encounter nguy hiểm (chó săn gác cần gạt cổng). |
| **`16:05 - 17:50`** | Chuyển trục xuống Catacombs & Cơ chế túi khí độc | Nghiên cứu cơ chế tụt máu theo thời gian khi đứng trong túi khí độc và thiết kế đường di chuyển nhảy bám đá ngầm. |
| **`18:35 - 20:30`** | Câu đố nắp hầm chìm & Lưới laser hầm mộ | Bóc tách cách phối hợp giữa bóng tối cổ kính và lưới laser công nghệ cao của lực lượng chiếm đóng. |

---

## 5. Điều Chưa Biết & Bằng Chứng Cần Bổ Sung

1. **Khả năng dìm xác lính xuống nước:** Tại phút `17:52` người chơi tự hỏi liệu có thể vứt xác lính xuống hố nước độc để phi tang không (`Can I drop them in that?`); trong video người chơi chưa thử nghiệm thao tác này.
2. **Tốc độ sát thương của khí độc hầm mộ:** Cần đo chính xác số giây nhân vật có thể trụ được trong làn khí độc trước khi mất hết thanh sinh lực.
3. **NPC Hỗ Trợ:** Tương tự các màn trước, Ora chỉ xuất hiện qua thoại vô tuyến và hình bóng cutscene, không trực tiếp hỗ trợ chiến đấu trong màn chơi.
