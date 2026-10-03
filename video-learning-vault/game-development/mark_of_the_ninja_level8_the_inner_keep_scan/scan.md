# 🔍 Scan Màn Chơi: Level 8 — "The Inner Keep"

> **Dự án đối chiếu:** Game 2D stealth về Đặc công Việt Nam (Single-player, 1 nhân vật điều khiển chính, Leader NPC hướng dẫn từ vòng ngoài, đồng đội xuất hiện tại các điểm chốt).  
> **Tài liệu nguồn:** [sources.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level8_the_inner_keep_scan/sources.md)  
> **Video khảo sát:** [theRadBrad — Level 8: The Inner Keep (23:32)](https://www.youtube.com/watch?v=FMfS6DYmns4)

---

## 1. Nguy Cơ & Cơ Chế Mới So Với Các Màn Trước

### ① Lính Hộ Pháp Hạng Nặng (Heavy Brute Guards) — Cơ Chế Buộc Phải Làm Choáng (Daze Mechanic)
* **Quan sát trực tiếp:** 
  - Tại phút `11:15`, xuất hiện loại kẻ thù to lớn vượt trội (Heavy Guard/Brute) mặc áo giáp dày và mang vũ khí cận chiến cồng kềnh.
  - HUD hiển thị cảnh báo: *"Even a surprise attack might not bring him down. You'll need to DAZE him somehow before you move in for the kill."*
  - Nếu người chơi thực hiện đòn tấn công bất ngờ (stealth kill) thông thường khi lính hộ pháp chưa bị choáng, đòn tấn công bị chặn đứng và người chơi bị hất ngược lại mất máu.
  - Cách hạ gục quan sát được:
    - *Cách 1 (Tương tác vật lý môi trường):* Tại phút `13:48`, người chơi bắn phi tiêu cắt dây đứt bóng đèn chùm khổng lồ treo trên trần nhà (`Chandelier Drop`). Đèn chùm rơi trúng đầu lính hộ pháp đè bẹp chết ngay tại chỗ (`+400 HAZARD KILL`).
    - *Cách 2 (Làm choáng trước):* Dùng đòn bẩy va đập hoặc ném bẫy chông/bom gây choáng váng (`Dazed`), sau đó mới kích hoạt được chuỗi QTE kết liễu.
* **Lời người chơi:** 
  - `11:15`: *"Even a surprise attack might not bring him down, you'll need to daze him somehow before you move in for the kill... this guy is a straight up boss himself, this is the biggest enemy we've had to face!"*
  - `13:48`: *"I can do the chandelier, that's the... really, I think that just killed him! Was that a stun maneuver or was that 'no one lives forever'?"*
  - `18:03`: *"Think about these guys is they got to be stunned first, which pisses me off... Drop something on his big dumb head!"*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Nâng cấp phân tầng kẻ địch: Game đưa cơ chế boss của Level 5 (Kelly) trở thành một loại kẻ thù thường trực (Regular Heavy Enemy) xuất hiện tuần tra trong màn chơi, đòi hỏi người chơi phải tận dụng tối đa môi trường thay vì chỉ rình rập đâm sau lưng.
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Lính tuần tra trang bị giáp chống đạn hạng nặng hoặc lính lê dương hộ pháp: Không thể hạ gục bằng dao găm một nhát; chiến sĩ phải giật đứt dây chằng thùng hàng, kéo sập giàn gỗ hoặc dùng báng súng nện choáng trước khi khống chế.

---

### ② Phân Nhánh Chiến Thuật Cố Định: Lựa Chọn Phương Án Vô Hiệu Hóa Trực Thăng (Branching Tactical Choice)
* **Quan sát trực tiếp:** 
  - Tại phút `19:57`, người chơi tiếp cận sân bay trực thăng (`Helipad`). Ora giao hai phương án chiến thuật rõ ràng:
    - *"The helipad, there it is! We can't let Karajan escape again. We can either TAKE OUT THE PILOT or DESTROY THE FUEL PUMP. Either way he'll be trapped like a rat!"*
  - *Phương án A:* Đột nhập buồng lái/khu chờ hạ sát viên phi công.
  - *Phương án B:* Luồn vào trạm cấp điện/nhiên liệu cắt nguồn máy bơm (`Fuel Pump / Electricity`).
  - theRadBrad lựa chọn Phương án B (phá trạm nhiên liệu và điện) ở phút `20:20`.
* **Lời người chơi:** 
  - `19:59`: *"We can either take out the pilot or destroy the fuel pump... I'll stick with Plan B, which is the alternative, taking out the electricity."*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Minh chứng rõ ràng cho việc thiết kế **lựa chọn cách tiếp cận (Player Agency)**: Cùng một mục tiêu chiến dịch (ngăn trực thăng cất cánh), người chơi được tự do chọn giải pháp bạo lực (ám sát phi công) hoặc giải pháp phá hoại kỹ thuật (cắt bơm xăng/ngắt điện) tùy theo phong cách chơi (Ghost hay Lethal).
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Khi mục tiêu là ngăn đoàn xe chỉ huy xuất kích: Người chơi có thể chọn ám sát viên tài xế dẫn đường, hoặc bí mật đổ đường vào bình xăng xe, hoặc cắt đứt dây cầu phao bắc qua sông. Cả 3 cách đều dẫn tới cùng kết quả ngăn chặn nhưng phục vụ các phong cách chơi khác nhau.

---

## 2. Cách Màn Chơi Kết Hợp Những Cơ Chế Đã Biết

* **Sniper Khóa Góc Kết Hợp Lính Hạng Nặng (08:49 & 13:00):** 
  - Hành lang rộng có sniper chốt giữ tầm xa ở tầng trên, trong khi phía dưới sàn có lính hộ pháp đi tuần tra. Người chơi không thể nhảy chạy liều mạng vì sniper sẽ bắn chết trong 1 phát; buộc phải dùng Noisemaker tách lính và luồn qua các kẽ xà nhà.
* **Tương Tác Thả Đèn Chùm (Chandelier Physics):** 
  - Đèn chùm treo trần trở thành vũ khí phục kích hàng đầu: Cắt dây thả rơi để tiêu diệt gọn cụm 2–3 kẻ địch đứng dưới mà không để lại dấu vết ám sát trực tiếp (`Hazard Kill`).
* **Chó Nghiệp Vụ:** 
  - **Thiếu dữ liệu / Không quan sát thấy:** Không thấy xuất hiện chó nghiệp vụ trong Level 8.

---

## 3. Tình Huống Mắc Lỗi, Bị Bắn Chết & Tháo Chạy Ngược Lên Thượng Lâu

* **Chết do Sniper ở phút 08:49:**
  - theRadBrad cố tình nhảy qua khoảng trống giữa hai ban công, bị tia ngắm của sniper bắt trúng và bắn hạ ngay lập tức (`One shot! Did you see that?!`).
* **Bị báo động cưỡng bức ở phút 10:50:**
  - theRadBrad không tìm được đường kín, quyết định tạo tiếng động kích hoạt báo động (`Alarm raised, bring it on!`), sau đó dụ lính chạy dồn vào bẫy chông sắt đặt sẵn ở cửa.
* **Chuỗi Rút Lui Ngược Lên Thượng Lâu (Backtracking Escape) (21:18 – 23:20):**
  - Sau khi phá hủy máy bơm xăng của trực thăng, toàn bộ căn cứ hú còi báo động khẩn cấp: *"Lost contact with the helipad... stay on alert! Reach the upper castle!"*
  - Nhiệm vụ cập nhật: Người chơi phải quay ngược lại bản đồ (backtrack) qua các căn phòng vừa đi qua, lúc này đã bị bố trí thêm nhiều toán lính tăng viện và các chốt chặn mới, tìm đường leo lên Thượng Lâu để kết thúc màn.

---

## 4. 4 Timestamp Đáng Phân Tích Sâu (Candidate Timestamps)

| Timestamp | Tên phân đoạn | Lý do đề xuất phân tích sâu cho Lượt 2 |
|---|---|---|
| **`08:45 - 09:15`** | Sniper 1-Hit Kill & Khắc phục tại Checkpoint | Bằng chứng thực nghiệm trực quan về tầm sát thương và thời gian ngắm bắn của lính bắn tỉa. |
| **`11:15 - 12:00`** | Giới thiệu Lính Hộ Pháp (Heavy Brute) | Nghiên cứu cơ chế khóa đòn stealth thông thường và điều kiện kích hoạt trạng thái choáng (`Dazed`). |
| **`13:40 - 14:10`** | Cắt dây thả đèn chùm đè bẹp kẻ thù | Phân tích cơ chế bẫy môi trường tự nhiên (Hazard Kill) không bị tính là đòn giết trực tiếp. |
| **`19:55 - 20:45`** | Phân nhánh chiến thuật tại Helipad | Xem xét cách bố trí màn chơi và ngôn ngữ hướng dẫn của NPC để người chơi lựa chọn giữa ám sát phi công vs cắt máy bơm nhiên liệu. |

---

## 5. Điều Chưa Biết & Bằng Chứng Cần Bổ Sung

1. **Kết quả nếu chọn Phương án A (Ám sát phi công):** Trong video theRadBrad chọn Phương án B (phá trạm bơm xăng); chưa có dữ liệu quan sát trực tiếp về cách phòng thủ của lính tại buồng lái phi công nếu đi theo nhánh A.
2. **Khả năng dùng bom khói làm choáng lính hộ pháp:** Liệu ném bom khói có khiến lính hộ pháp rơi vào trạng thái Dazed để áp sát ám sát được không?
3. **Phạm vi playlist:** Đây là video cuối cùng trong playlist 8 phần của theRadBrad; 4 màn cuối của game (Level 9 đến 12) không có trong danh sách phát này.
