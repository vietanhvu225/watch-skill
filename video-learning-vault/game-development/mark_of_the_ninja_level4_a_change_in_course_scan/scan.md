# 🔍 Scan Màn Chơi: Level 4 — "A Change in Course"

> **Dự án đối chiếu:** Game 2D stealth về Đặc công Việt Nam (Single-player, 1 nhân vật điều khiển chính, Leader NPC hướng dẫn từ vòng ngoài, đồng đội xuất hiện tại các điểm chốt).  
> **Tài liệu nguồn:** [sources.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level4_a_change_in_course_scan/sources.md)  
> **Video khảo sát:** [theRadBrad — Level 4: A Change In Course (12:22)](https://www.youtube.com/watch?v=VUaFnEfMvy4)

---

## 1. Nguy Cơ & Cơ Chế Mới So Với Level 1, 2 và 3

### ① Cảm Biến Chuyển Động Quét — Cơ Chế "Đứng Yên Tuyệt Đối" (Stand Still Mechanic)
* **Quan sát trực tiếp:** 
  - Tại phút `03:09`, xuất hiện chùm tia cảm biến quét qua lại theo chu kỳ hình nón. HUD hiển thị bảng hướng dẫn: *"Watch out for those sensors, they can catch your slightest twitch. So when they sweep across you, STAND STILL."*
  - Nếu nhân vật di chuyển (bước đi, nhảy, lăn) khi chùm tia quét qua, còi báo động kích hoạt ngay lập tức. Nếu người chơi dừng mọi cử động và đứng im, chùm tia quét qua người mà không phát hiện báo động.
* **Lời người chơi:** 
  - `03:09`: *"Watch out for those sensors, they can catch your slightest twitch, so when they sweep across you, stand still... It doesn't hunt by sound on those, so basically I got to run and hide each time this happens."*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Đây là cơ chế đối trọng với ánh sáng/bóng tối thông thường: Ngay cả khi đứng ở chỗ sáng, nếu người chơi kịp dừng chuyển động đúng nhịp tia quét, họ vẫn an toàn trước loại cảm biến này.
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Mô phỏng cảm biến địa chấn / cảm biến rung rải rác quanh hàng rào điện tử Mc-Namara: Khi máy quét xung kích hoạt, đặc công phải dừng đứng yên bất động, nếu bước chân sẽ kích hoạt tín hiệu báo về trung tâm điều khiển.

---

### ② Phá Hoại Môi Trường Hệ Thống Khí Gas (Environmental Sabotage — Gas Mains & Vents)
* **Quan sát trực tiếp:** 
  - Tại phút `03:56`, người chơi tiếp cận đường ống gas chính ở tầng hầm để chọc thủng (`start a leak`).
  - Từ phút `05:04` đến `07:00`, nhiệm vụ cập nhật: Tìm các van hút gió kết nối với tầng hầm và mở toang ra (`Open vents upstairs: 1 of 6`). Cần mở tối thiểu 3 van để khí gas lan tỏa đủ nồng độ, mở càng nhiều van thì vụ hỏa hoạn càng lớn.
  - Sau khi đủ khí gas, người chơi di chuyển lên mái nhà ở phút `11:40` và ném phi tiêu đánh lửa tạo đám cháy khổng lồ.
* **Lời người chơi:** 
  - `03:56`: *"Buildings usually have a gas main in the basement, start a leak and we can spread the gas to the rest of the complex."*
  - `05:04`: *"That intake is drawing in the gas, open a few of the vents upstairs, we'll be ready to turn this place into an inferno."*
  - `05:46`: *"Three should do it, but opening more will create a bigger distraction."*
* **Suy luận & Mức tin cậy:** 
  - **[Mức tin cậy: Rất cao]** Cơ chế mục tiêu phi tuyến tính (Non-linear Environmental Sabotage): Cho phép người chơi tự chọn mức độ rủi ro (làm tối thiểu 3 van để đi tiếp an toàn hoặc mạo hiểm mở cả 6 van để nhận thưởng điểm tối đa).
* **Đề xuất áp dụng cho Game Đặc công:** 
  - Nhiệm vụ phá hoại kho xăng / kho đạn: Đặc công có thể đặt kíp nổ tại 1 bể xăng chính để mở đường tháo chạy, hoặc luồn sâu đặt kíp nổ liên hoàn cả 4 bồn chứa để phá hủy hoàn toàn mục tiêu chiến lược.

---

## 2. Cách Màn Chơi Kết Hợp Những Cơ Chế Đã Biết

* **Bẫy Chông Sắt (Caltrops / Spike Strips) + Điều Hướng Tiếng Động:** 
  - Tại phút `04:18` và `10:32`, người chơi rải bẫy chông sắt xuống sàn hành lang hẹp, sau đó tạo tiếng động dụ lính bước ngang qua. Lính giẫm trúng chông bị hạ gục ngay tại chỗ mà người chơi không cần áp sát.
* **Áp Lực Thời Gian Tự Nhiên Khi Địch Bắt Đầu Nghi Ngờ:** 
  - Tại phút `11:13`, khi các van gas đã mở, thoại lính và loa thông báo bắt đầu hoảng hốt về mùi gas: *"They're starting to get suspicious, we need to get to the roof!"* Trò chơi thúc đẩy người chơi di chuyển nhanh dần lên tầng thượng.
* **Chó Nghiệp Vụ:** 
  - **Thiếu dữ liệu / Không quan sát thấy:** Trong suốt 12 phút 22 giây của Level 4, không ghi nhận sự xuất hiện của chó nghiệp vụ. Kẻ địch hoàn toàn là lính Hessian có súng và các chốt cảm biến cơ học.

---

## 3. Tình Huống Mắc Lỗi, Phát Hiện & Đoạn Kết Sập Mái Nhà

* **Phát hiện & Bị bắn tại phút 08:53 – 09:00:**
  - *Diễn biến:* Người chơi di chuyển thiếu quan sát khi chạy qua hành lang tầng 2, rơi vào tầm nhìn của lính. Lính hô lớn và nổ súng liên tiếp (`Shots fired`).
  - *Phục hồi:* theRadBrad nhanh chóng tung người nhảy bám xà nhà phía trên và lẩn vào ống thông gió trần. Lính mất dấu tại chỗ và quay lại tuần tra.
  - *Lời người chơi (08:56):* *"Got away, got away, shots fired, that's good, we're good... I know I heard something, it's clear."*
* **Kết màn bất ngờ — Sập mái nhà (Roof Collapse) ở phút 11:58 – 12:14:**
  - Sau khi bắn phi tiêu kích nổ khí gas, ngọn lửa bốc lên ngùn ngụt làm kết cấu mái nhà bị nứt toác. Sàn mái đổ sập, nhân vật chính rơi thẳng xuống dưới và màn chơi kết thúc đột ngột chuyển sang Level 5.
  - Không có đường rút lui yên bình: Đây là chuyển tiếp kịch tính bằng sự cố môi trường.

---

## 4. 3 Timestamp Đáng Phân Tích Sâu (Candidate Timestamps)

| Timestamp | Tên phân đoạn | Lý do đề xuất phân tích sâu cho Lượt 2 |
|---|---|---|
| **`03:05 - 03:35`** | Cảm biến chuyển động quét nhịp & Đứng yên | Đo chu kỳ quét của chùm tia cảm biến; xác định ngưỡng dung sai (tolerance) của thao tác "đứng yên" (có được phép xoay người hay phải bất động 100%). |
| **`05:00 - 07:15`** | Phân nhánh mở van gas tầng hầm | Phân tích cấu trúc phân nhánh: Người chơi lựa chọn thứ tự mở van gas (tối thiểu 3/6 van) và phản ứng tăng dần cấp độ cảnh giác của lính gác. |
| **`11:45 - 12:15`** | Kích nổ hỏa hoạn & Sập sàn chuyển màn | Nghiên cứu thủ pháp chuyển giao cao trào (environmental cliffhanger) kết nối trực tiếp sang màn đột nhập tháp ở Level 5. |

---

## 5. Điều Chưa Biết & Bằng Chế Cần Bổ Sung

1. **Quy tắc phát hiện của cảm biến quét:** Nếu người chơi ném phi tiêu hoặc ném bom khói khi đang đứng trong chùm tia quét, cảm biến có bắt được chuyển động tay ném không?
2. **Hậu quả nếu mở đủ 6/6 van gas:** Điểm thưởng danh dự (Honor) hoặc con dấu (Seal) cộng thêm có tạo ra sự khác biệt lớn so với mở 3 van không?
3. **NPC Hỗ Trợ (Ora):** Ora chỉ xuất hiện ở cutscene đầu màn bàn kế hoạch nghi binh ("set that building on fire..."), sau đó đóng vai trò thoại hướng dẫn qua vô tuyến; không trực tiếp tham gia trên màn hình gameplay.
