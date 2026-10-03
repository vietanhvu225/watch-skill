# 🗺️ Tổng Hợp Khảo Sát Walkthrough Toàn Diện: Mark of the Ninja (Lượt 1)

> **Mục tiêu tài liệu:** Báo cáo kết quả Lượt 1 — Rà soát toàn bộ playlist walkthrough của theRadBrad, xác định ranh giới phạm vi dữ liệu, tổng hợp ma trận bằng chứng các cơ chế stealth, chỉ rõ các khoảng trống dữ liệu và đề xuất các phân đoạn sáng giá nhất cho Lượt 2.  
> **Dự án áp dụng:** Game 2D stealth về Đặc công Việt Nam (Single-player, 1 nhân vật điều khiển chính, Leader NPC hướng dẫn từ vòng ngoài, đồng đội hỗ trợ ở các điểm chốt).  
> **Nguyên tắc phương pháp luận:** Tuân thủ triệt để tính thực nghiệm (Empirical rigor) do Andy và Codex thống nhất — tách bạch Quan sát, Lời người chơi, Suy luận và Đề xuất áp dụng; không suy đoán luật tuyệt đối; không sinh code/prototype.

---

## 1. Kết Quả Kiểm Tra Phạm Vi Playlist: "8 Level" Có Tương Ứng Toàn Bộ Game Không?

* **Kết luận kiểm chứng thực tế:** **KHÔNG.** "8 level" trong playlist **KHÔNG** tương ứng với toàn bộ trò chơi *Mark of the Ninja*.
  1. **Quy mô game gốc:** Phiên bản tiêu chuẩn của *Mark of the Ninja* có tổng cộng **12 màn chơi (Chapters)** chính thức, cộng thêm 1 màn chơi đặc biệt trong bản Special Edition DLC ("Dosan's Tale").
  2. **Quy mô của playlist:** Playlist YouTube của `theRadBrad` (`PLs1-UdHIwbo5RrqHUbq4zvjquAYJ_Bghf`) chỉ có đúng **8 video**. theRadBrad đã dừng (hoặc kết thúc sớm) loạt video walkthrough của mình sau Part 8 [Level 8: The Inner Keep]. Bốn màn chơi cuối cùng của game (Level 9 đến Level 12) **hoàn toàn không có trong playlist này**.
  3. **Tương quan giữa Video và Màn chơi trong playlist:** Trong phạm vi 8 video hiện có, **mỗi video tương ứng đúng 1 màn chơi** (tỷ lệ 1:1, từ Level 1 đến Level 8). Không có màn nào bị chia làm 2 video và không có video nào gộp 2 màn.

### Bảng Ánh Xạ Chi Tiết 8 Video Trong Playlist theRadBrad

| Phần (Part) | Tên Màn Chơi (Level Name) | URL Video | Thời Lượng | Nền Tảng Quan Sát | Độ Khó | Tình Trạng Khảo Sát |
|---|---|---|---|---|---|---|
| **Part 1** | **Level 1: Ink & Dreams** | [e1UnoyKqeMs](https://www.youtube.com/watch?v=e1UnoyKqeMs) | 20:21 | Layout Xbox 360 | Normal | Đã phân tích chuyên sâu ([findings.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/findings.md)) |
| **Part 2** | **Level 2: Breaching the Perimeter** | [MSwyZ45L1r8](https://www.youtube.com/watch?v=MSwyZ45L1r8) | 22:36 | Layout Xbox 360 | Normal | Đã phân tích chuyên sâu ([findings.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level2_slow_walkthrough/findings.md)) |
| **Part 3** | **Level 3: The Trail of Shadow** | [fpTq1-gdKE8](https://www.youtube.com/watch?v=fpTq1-gdKE8) | 23:56 | Layout Xbox 360 | Normal | Đã scan Lượt 1 ([scan.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level3_the_trail_of_shadow_scan/scan.md)) |
| **Part 4** | **Level 4: A Change in Course** | [VUaFnEfMvy4](https://www.youtube.com/watch?v=VUaFnEfMvy4) | 12:22 | Layout Xbox 360 | Normal | Đã scan Lượt 1 ([scan.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level4_a_change_in_course_scan/scan.md)) |
| **Part 5** | **Level 5: The Fall of Hessian Tower** | [CVhlTVVzlsk](https://www.youtube.com/watch?v=CVhlTVVzlsk) | 20:03 | Layout Xbox 360 | Normal | Đã scan Lượt 1 ([scan.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level5_the_fall_of_hessian_tower_scan/scan.md)) |
| **Part 6** | **Level 6: An Ancestral Home** | [lUQz6zixb80](https://www.youtube.com/watch?v=lUQz6zixb80) | 21:57 | Layout Xbox 360 | Normal | Đã scan Lượt 1 ([scan.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level6_an_ancestral_home_scan/scan.md)) |
| **Part 7** | **Level 7: Above A Bottomless Chasm** | [MmrezLJqNFw](https://www.youtube.com/watch?v=MmrezLJqNFw) | 14:44 | Layout Xbox 360 | Normal | Đã scan Lượt 1 ([scan.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level7_above_a_bottomless_chasm_scan/scan.md)) |
| **Part 8** | **Level 8: The Inner Keep** | [FMfS6DYmns4](https://www.youtube.com/watch?v=FMfS6DYmns4) | 23:32 | Layout Xbox 360 | Normal | Đã scan Lượt 1 ([scan.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level8_the_inner_keep_scan/scan.md)) |

### Các Màn Chơi Của Game Gốc Chưa Có Dữ Liệu Trong Playlist Này
- **Level 9:** *A Blade at His Neck* (Ám sát Karajan)
- **Level 10:** *A Shattered Stronghold* (Tấn công cứ điểm tàn phá)
- **Level 11:** *Set to Flight* (Tháo chạy trong bão cát)
- **Level 12:** *The Return* (Hồi hương và kết cục bi kịch của Mark)
- *(DLC)*: *Dosan's Tale* (Câu chuyện tiền truyện của thương nhân Dosan)

---

## 2. Ma Trận Bằng Chứng Thực Nghiệm Theo 6 Chủ Đề Ưu Tiên

Dưới đây là ma trận tổng hợp bằng chứng quan sát được qua 8 màn chơi đã nghiên cứu:

| Chủ Đề Ưu Tiên | Có Bằng Chứng Thực Nghiệm Ở Màn Nào? | Tóm Tắt Hiện Tượng Quan Sát Được | Khoảng Trống Chưa Xác Minh (Gaps) |
|---|---|---|---|
| **1. Chó Nghiệp Vụ (Guard Dogs)** | **Level 3, Level 6, Level 7** <br>*(Vắng mặt ở Level 1, 2, 4, 5, 8)* | • Đánh hơi xuyên bóng tối (`sniff you out in the darkness`), vô hiệu hóa tính an toàn của bóng đêm. <br>• Có trạng thái ngủ cạnh lính (`Sleeping State`), chỉ tỉnh dậy khi có tiếng bước chân/báo động. <br>• Lùng sục tới tận các điểm ẩn nấp (gầm giường, tủ đồ) khi có còi báo động. <br>• Có thể bị tiêu diệt bằng đòn cận chiến. | • Chưa có bằng chứng liệu có đòn ám sát im lặng hoàn toàn (Silent Kill) đối với chó không. <br>• Chưa rõ bom khói hoặc mồi nhử có làm mất dấu mùi của chó hay không. |
| **2. Đèn Pha & Cảm Biến Quang Học (Searchlights & Sensors)** | **Level 1, Level 2, Level 3, Level 4, Level 7, Level 8** | • Đèn pha di động quét chu kỳ hình nón (L1, L2, L3); trúng đèn bị bắn hạ rất nhanh. <br>• Cảm biến sinh trắc nhận diện thẻ lính (L2, L5): Có thể kéo lê xác lính qua để ngắt báo động. <br>• Cảm biến chuyển động (L4): Đứng yên tuyệt đối (`Stand Still`) khi tia quét qua sẽ không bị lộ. <br>• Lưới laser đỏ (L2, L3, L6, L7): Chặn được bằng khối kim loại (L2) hoặc làm nhiễu bằng bom khói (L3). | • Chưa đo đạc cụ thể tốc độ quét (vận tốc góc) của đèn pha. <br>• Chưa kiểm chứng nếu ném vật thể khi đứng trong cảm biến chuyển động thì có bị bắt chuyển động tay không. |
| **3. Phối Hợp Nhiều Lính & Phân Tầng Kẻ Địch (Multi-Guard & Enemy Tiers)** | **Toàn bộ Level 1 đến Level 8** | • **Lính thường:** Súng trường + đèn pin, tầm nhìn nón hẹp, phản xạ theo tiếng động (L1–L8). <br>• **Lính mang khiên:** Miễn nhiễm đòn trực diện 180°, buộc phải bọc sườn/đánh lạc hướng (L3). <br>• **Lính bắn tỉa (Sniper):** Tia laser đỏ cực dài, 1 phát chết ngay (`1-hit kill`), buộc phải che scope hoặc đi đường vòng (L5, L8). <br>• **Lính phòng hóa:** Đeo mặt nạ phòng độc khi có còi báo động trong vùng khí gas (L7). <br>• **Lính hộ pháp (Heavy Brute):** Miễn nhiễm đòn ám sát bất ngờ; bắt buộc phải làm choáng (`Daze`) bằng môi trường (thả đèn chùm, bẫy chông) trước khi kết liễu (L8, boss Kelly L5). | • Chưa rõ nếu 2 lính đi sát nhau, việc ám sát 1 tên có làm tên kia quay lại tức thì hay có khoảng trễ phản xạ 0.5s. |
| **4. Báo Động, Tìm Kiếm & Hạ Nhiệt (Alarm, Search & Cooldown)** | **Level 1, Level 2, Level 3, Level 6, Level 7, Level 8** | • Phát hiện xác chết (`Body Found`) kích hoạt báo động sục sạo cục bộ, không tự động biến thành Game Over. <br>• Hiding spot (thùng rác, hốc cửa, bình phong) cách ly tầm nhìn an toàn: Lính lục soát một lúc không thấy sẽ buông thoại bỏ cuộc và hạ mức cảnh giác (`Suspicion Cooldown`). <br>• Có tình huống báo động bắt buộc theo kịch bản (mở cửa cargo rỉ sét L7, đặt máy theo dõi L3, phá trạm bơm xăng L8). | • Chưa có đồng hồ bấm giờ chính xác cho thời gian hạ nhiệt nghi ngờ giữa các độ khó khác nhau. |
| **5. Đường Rút Lui & Thay Đổi Nhịp Độ (Extraction & Pacing)** | **Level 1, Level 2, Level 3, Level 4, Level 5, Level 6, Level 7, Level 8** | • Màn chơi thường chia làm 2 pha: Pha thâm nhập kín đáo ➔ Pha rút lui khẩn cấp. <br>• Khi hoàn thành mục tiêu nổ/phá hoại, lối vào chính bị phong tỏa; game mở ra trục thoát hiểm thay thế: <br>  - Trục thẳng đứng trên cao qua ống gió (L3). <br>  - Sập mái nhà chuyển cảnh (L4). <br>  - Leo ngược lại bản đồ (Backtracking) qua các chốt chặn mới (L8). <br>  - Bám thùng hàng trên cáp treo vượt vực (L7). | • Chưa có thử nghiệm xem nếu người chơi ở lại chiến đấu tiêu diệt hết lính ở pha rút lui thì có được không hay bắt buộc phải chạy. |
| **6. NPC Hỗ Trợ (Ora, Sensei Azai, Đồng đội)** | **Level 1, Level 2, Level 3, Level 4, Level 5, Level 6, Level 7, Level 8** | • **Hoàn toàn là NPC dẫn dắt vòng ngoài / vô tuyến:** Ora chỉ xuất hiện ở cutscene, đứng ở các điểm chốt ranh giới hoặc giao tiếp qua đàm thoại radio/gợi ý mục tiêu. <br>• **Không có AI đồng hành trực tiếp:** Trong gameplay, người chơi 100% điều khiển độc lập một nhân vật chính; không có đồng đội đi kè kè hỗ trợ bắn yểm trợ. | • Chưa thấy cơ chế phối hợp tác chiến 2 người thời gian thực trong game gốc. |

---

## 3. Tổng Hợp Các Khoảng Trống Dữ Liệu (Data Gaps & Missing Evidence)

1. **Thiếu dữ liệu 4 màn cuối (Levels 9, 10, 11, 12):** Playlist của theRadBrad kết thúc ở Level 8; nếu muốn nghiên cứu các màn sau (đặc biệt là màn bão cát Level 11 và màn đối đầu cuối Level 12), cần tìm nguồn walkthrough bổ sung từ kênh khác.
2. **Thiếu thông số đo đạc vi mô (Pixel & Millisecond metrics):** Do mục tiêu Lượt 1 là scan diện rộng để nắm cơ chế, chúng ta chưa đo đạc thời gian trễ của AI tính bằng mili-giây hoặc bán kính vòng tròn sóng âm pixel của từng loại vũ khí.
3. **Chưa có dữ liệu nhánh rẽ đối lập tại Helipad (Level 8):** theRadBrad chọn Phương án B (phá trạm nhiên liệu); hành vi của lính và cách bố phòng nếu chọn Phương án A (ám sát phi công trực thăng) chưa có dữ liệu quan sát trực tiếp.
4. **Giới hạn của một lượt chơi đơn lẻ:** theRadBrad là người chơi phong cách khám phá, thường dùng Noisemaker và giết lính; chưa phản ánh hết các phương án vượt màn theo phong cách thuần ẩn nhẫn không đụng độ (Ghost/No-Kill).

---

## 4. Đề Xuất 4 Cụm Phân Đoạn Cho Lượt 2 (Candidates for Deep-Dive)

Dựa trên nhu cầu thiết kế cốt lõi của game Đặc công Việt Nam, đề xuất Andy và Codex cân nhắc 4 cụm phân đoạn có giá trị học hỏi cao nhất:

### 🎯 Ứng viên 1: Cơ Chế Chó Nghiệp Vụ & Khắc Chế Bóng Tối
* **Phân đoạn chọn lọc:** **Level 3 (01:30 – 02:40 & 17:30 – 18:00)** hoặc **Level 7 (13:10 – 14:15)**.
* **Giá trị cho game Đặc công:** Giải quyết bài toán tạo áp lực cho người chơi ở các vùng bóng tối an toàn; học cách thiết kế trạng thái ngủ/thức của chó và tương tác khứu giác trong không gian 2D.

### 🎯 Ứng viên 2: Lính Bắn Tỉa (Sniper) & Khóa Vùng Tầm Xa
* **Phân đoạn chọn lọc:** **Level 5 (14:04 – 15:15)** hoặc **Level 8 (08:45 – 09:15)**.
* **Giá trị cho game Đặc công:** Áp dụng cho các tháp canh, chòi gác có đèn pha và súng bắn tỉa của địch; nghiên cứu cơ chế "1 phát chết ngay", cách hiển thị tia laser cảnh báo và các giải pháp che chắn tầm ngắm (`block scope`).

### 🎯 Ứng viên 3: Phân Nhánh Chiến Thuật Cố Định (Player Agency)
* **Phân đoạn chọn lọc:** **Level 8 (19:55 – 21:00)** tại khu vực Helipad.
* **Giá trị cho game Đặc công:** Cung cấp mẫu hình thiết kế cho phép đặc công lựa chọn phương án hoàn thành mục tiêu: Ám sát cá nhân chỉ huy địch (phương án bạo lực) vs. Phá hoại khí tài/cắt điện/đốt kho xăng (phương án kỹ thuật ngầm).

### 🎯 Ứng viên 4: Pha Rút Lui Khẩn Cấp Sau Báo Động Đỏ (Exfiltration / Chase Phase)
* **Phân đoạn chọn lọc:** **Level 3 (20:17 – 23:26)** hoặc **Level 8 (21:18 – 23:20)**.
* **Giá trị cho game Đặc công:** Cực kỳ phù hợp với đề xuất phân pha đã thống nhất ở Mục 8.5: Sau khi bộc phá nổ, căn cứ báo động vĩnh viễn; nghiên cứu cách game dẫn dắt người chơi tìm đường thoát hiểm theo trục khác (ống thông gió trên cao hoặc luồn lách ngược bản đồ) dưới làn đạn truy đuổi.

---

## 5. Điểm Dừng & Chờ Phản Hồi

Toàn bộ tài liệu scan chi tiết và sources của 6 màn còn lại đã hoàn tất:
- [Level 3 Scan](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level3_the_trail_of_shadow_scan/scan.md) & [Sources](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level3_the_trail_of_shadow_scan/sources.md)
- [Level 4 Scan](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level4_a_change_in_course_scan/scan.md) & [Sources](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level4_a_change_in_course_scan/sources.md)
- [Level 5 Scan](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level5_the_fall_of_hessian_tower_scan/scan.md) & [Sources](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level5_the_fall_of_hessian_tower_scan/sources.md)
- [Level 6 Scan](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level6_an_ancestral_home_scan/scan.md) & [Sources](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level6_an_ancestral_home_scan/sources.md)
- [Level 7 Scan](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level7_above_a_bottomless_chasm_scan/scan.md) & [Sources](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level7_above_a_bottomless_chasm_scan/sources.md)
- [Level 8 Scan](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level8_the_inner_keep_scan/scan.md) & [Sources](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level8_the_inner_keep_scan/sources.md)

**Agy tạm dừng tại đây** theo đúng chỉ đạo để Andy và Codex đánh giá kết quả Lượt 1 và lựa chọn các phân đoạn ưu tiên để tiến hành phân tích sâu cho Lượt 2.
