# 🔍 Scan Màn Chơi: Level 7 — "Above a Bottomless Chasm"

> **Dự án đối chiếu:** Game 2D stealth về Đặc công Việt Nam (Single-player, 1 nhân vật điều khiển chính, Leader NPC hướng dẫn từ vòng ngoài, đồng đội xuất hiện tại các điểm chốt).  
> **Tài liệu nguồn:** [sources.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level7_above_a_bottomless_chasm_scan/sources.md)  
> **Video khảo sát:** [theRadBrad — Level 7: Above A Bottomless Chasm (14:44)](https://www.youtube.com/watch?v=MmrezLJqNFw)

---

## 1. Nguy Cơ & Cơ Chế Mới So Với Các Màn Trước

### ① Thùng Hàng Di Động Trên Vực Thẳm (Moving Cargo Crates on Cable System)
* **Quan sát trực tiếp:** 
  - Tại phút `03:20`, xuất hiện hệ thống cáp treo vận chuyển thùng hàng vũ khí chạy liên tục qua vực thẳm sâu không đáy.
  - HUD và thoại Ora chỉ dẫn: *"They're using those crates to haul weapons. Nobody will notice if you ride along."*
  - Người chơi có thể nhảy bám vào thành dưới hoặc nấp bên trong các thùng hàng đang di chuyển trên không để vượt qua các chốt gác và vực sâu chết người mà không phát ra tiếng động.
* **Lời người chơi:** 
  - `03:20`: *"They're using those crates to haul weapons, nobody will notice if you ride along... Come on, oh I got it, that was close!"*
  - `08:39`: *"You have to duck in this little thing right here, and you have to wait for it on the other side... look at that, beautiful!"*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Cơ chế "Nơi ẩn nấp di động" (Mobile Hiding Spot): Kết hợp giữa yếu tố giải đố platforming (nhảy bám đúng thời điểm) và tàng hình di động để vượt qua các khoảng không gian mở không có bóng tối tự nhiên.
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Thâm nhập qua hệ thống xe goòng chở than, gầm xe tải quân sự chở hàng tiếp tế hoặc thuyền buồm chở lương thực: Chiến sĩ bám dưới gầm hoặc chui vào thùng hàng để qua cổng kiểm soát gắt gao.

---

### ② Lính Đeo Mặt Nạ Phòng Độc (Gas Mask Soldiers)
* **Quan sát trực tiếp:** 
  - Tại phút `14:01`, khi còi báo động ré lên trong khu vực ngập tràn khí gas độc hại, lính chỉ huy hét lệnh: *"Put on your masks!"*
  - Toàn bộ lính trong khu vực lập tức đeo mặt nạ phòng độc (Gas Masks), cho phép chúng di chuyển tự do và tác chiến bình thường bên trong làn khí độc mà không bị mất máu như người chơi.
* **Lời người chơi:** 
  - `14:01`: *"I don't know, put on your masks... come on, open up, made it!"*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Cao]** Khí độc không còn là vũ khí có thể dùng để tiêu diệt thụ động kẻ thù: Địch có trang bị bảo hộ chuyên dụng phản ứng tức thì khi có sự cố hóa chất.
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Khi người chơi sử dụng lựu đạn cay hoặc chọc thủng bình hóa chất, lính đối phương sẽ đeo mặt nạ phòng hóa; người chơi nếu không có khăn ướt bịt mũi sẽ bị giảm tầm nhìn và tụt thể lực nhanh hơn địch.

---

### ③ Tiếng Ồn Cố Tình Khi Kích Hoạt Cửa Cơ Khí (Scripted Noise Gate Activation)
* **Quan sát trực tiếp:** 
  - Tại phút `10:44`, Ora cảnh báo về cơ chế cửa thoát hiểm: *"I found a way out, it's just past the next room, but there's a rusty old gate blocking the way. There's a way to open it from your side, but be ready when the racket starts!"*
  - Tại phút `12:55`, khi người chơi gạt cần mở cửa hàng (`Cargo Door`), tiếng kim loại rỉ sét cọ xát rít lên cực lớn tạo ra vòng sóng âm khổng lồ trên màn hình. Lính gác toàn khu vực đồng loạt quay lại hô hoán: *"Someone activated the old cargo door! Get them!"* và xua chó săn lao tới lùng sục.
* **Lời người chơi:** 
  - `10:44`: *"Be ready when the racket starts..."*
  - `12:55`: *"Someone activated the old cargo door, get them! ... The great thing is I can use my ability... dogs, wow, hey someone there, check the bed, he has to be in here somewhere!"*
  - `13:48`: *"Out dog there really, can I take down the dog in stealth?"*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Thiết kế kịch bản bắt buộc gây ồn (Forced Noise Interaction): Đặt người chơi vào thế buộc phải chấp nhận gây tiếng động để mở đường, sau đó kiểm tra kỹ năng ẩn nấp ứng biến nhanh trước sự lùng sục của chó săn và lính tuần tra.
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Khi đặc công phải cắt xích sắt cổng kho hoặc nổ mìn phá khóa bộc phá: Hành động này chắc chắn tạo tiếng nổ/tiếng kim loại vang xa; ngay sau thao tác, người chơi có 5–10 giây di tản vào hầm ngầm trước khi đội phản ứng nhanh ập tới.

---

## 2. Cách Màn Chơi Kết Hợp Những Cơ Chế Đã Biết

* **Chó Săn Lùng Sục Tận Nơi Ẩn Nấp (13:31):** 
  - Sau khi cửa rỉ sét phát ra tiếng động, chó săn dẫn đầu toán lính chạy thẳng vào phòng điều tra. Chó đánh hơi tới tận gầm giường/tủ đồ (`check the bed`), cho thấy nơi ẩn nấp thông thường không đảm bảo an toàn nếu chó tiếp cận quá gần.
* **Khí Độc Hành Lang + Chạy Đua Thời Gian (02:42):** 
  - Hành lang hẹp bị bao phủ bởi khí độc dày đặc, không có lỗ thông hơi ở giữa. Người chơi phải chạy hết tốc lực (`move fast`) từ đầu này sang đầu kia trước khi cạn thanh máu.
* **Lưới Laser Trên Vực Thẳm:** Chạm vào laser không chỉ báo động mà còn khiến nhân vật trượt chân ngã thẳng xuống vực sâu chết ngay.

---

## 3. Tình Huống Mắc Lỗi, Bị Lùng Sục & Phục Hồi

* **Mắc kẹt tại câu đố laser treo trần (09:00 – 10:25):**
  - theRadBrad bối rối vì không gian quá tối và các chùm laser đan chéo dày đặc trên đầu (`I could not see cuz it was so damn dark`).
  - Phục hồi: theRadBrad dùng kỹ năng viễn thị (`Far Sight`) soi sáng toàn bộ sơ đồ phòng, phát hiện điểm bám trên trần nhà và đu qua an toàn.
* **Bị chó săn và lính áp sát sau tiếng động mở cửa (13:10 – 14:15):**
  - Người chơi kịp thời leo lên xà nhà cao ngoài tầm ngửi của chó, thả bẫy chông sắt xuống sàn để chặn bước tiến của toán lính đeo mặt nạ phòng độc, sau đó nhanh chân lao qua cánh cửa vừa mở hé.

---

## 4. 3 Timestamp Đáng Phân Tích Sâu (Candidate Timestamps)

| Timestamp | Tên phân đoạn | Lý do đề xuất phân tích sâu cho Lượt 2 |
|---|---|---|
| **`03:15 - 04:15`** | Đu bám thùng hàng di động trên vực thẳm | Nghiên cứu cơ chế hitbox và góc ẩn nấp của nhân vật khi bám trên vật thể cơ học chuyển động liên tục. |
| **`10:40 - 11:20`** | Lời dặn của Ora về cửa rỉ sét gây tiếng vang | Phân tích cách game báo trước (telegraphing) rủi ro âm thanh không thể tránh khỏi để người chơi chủ động chuẩn bị phương án thoát ly. |
| **`12:55 - 14:20`** | Báo động mở cửa, chó săn sục sạo & Lính đeo mặt nạ | Bóc tách chuỗi phản ứng liên hoàn của AI: tiếng ồn ➔ chó dẫn đường ngửi mùi ➔ lính trang bị mặt nạ phòng độc ➔ người chơi rút lui qua cửa hẹp. |

---

## 5. Điều Chưa Biết & Bằng Chứng Cần Bổ Sung

1. **Khả năng triệt hạ chó săn trong lúc lùng sục:** Ở phút `13:48` người chơi đặt câu hỏi *"Can I take down the dog in stealth?"* nhưng không dám bấm nút thử vì sợ lộ. Cần bằng chứng bổ sung xem liệu có thể ám sát chó săn từ trên trần nhà nhảy xuống không.
2. **Mức độ ảnh hưởng của chó săn tới hiding spot:** Nếu nhân vật trốn trong tủ đồ/thùng rác khi chó săn đi qua, chó có tự động cào cửa phát hiện người chơi không?
3. **NPC Hỗ Trợ:** Ora đóng vai trò người do thám đi trước (scout), tìm ra vị trí cánh cửa thoát hiểm và liên lạc qua bộ đàm.
