# Nguồn Dữ Liệu & Giới Hạn Kỹ Thuật (Provenance & Limitations) — Level 2

> **Mục đích tài liệu:** Ghi nhận nguồn gốc dữ liệu, công cụ phân tích thực nghiệm và các giới hạn quan sát trong quá trình phân tích video walkthrough *Mark of the Ninja — Level 2: "Breaching the Perimeter"*.  
> **Dự án ứng dụng:** Nghiên cứu thiết kế trải nghiệm, cơ chế giới thiệu nguy cơ mới (lưới laser, chó săn, bẫy), lựa chọn đường đi, và cơ chế hồi phục sau sai lầm cho game 2D stealth về Đặc công Việt Nam (Single-player, 1 nhân vật điều khiển, NPC hỗ trợ).

---

## 1. Thông Tin Nguồn Video (Video Provenance)

* **URL:** [https://www.youtube.com/watch?v=MSwyZ45L1r8](https://www.youtube.com/watch?v=MSwyZ45L1r8)
* **Tiêu đề gốc:** `Mark of the Ninja - Gameplay Walkthrough - Part 2 [Level 2: Breaching the Perimeter] (Xbox 360)`
* **Kênh phát hành:** [theRadBrad](https://www.youtube.com/@theRadBrad)
* **Ngày phát hành video:** 10/09/2012.
* **Thời lượng video:** 22 phút 36 giây (1,356 giây). Bao gồm thời lượng lời chào mở đầu, bình luận của YouTuber và cutscene chuyển tiếp cốt truyện.
* **Giao diện điều khiển & Nền tảng:** Hiển thị layout nút bấm Xbox (`A`, `B`, `X`, `Y`, `LT`, `RT`, `D-Pad`). Có thể chơi trên console Xbox 360 hoặc PC cắm tay cầm Xbox; chưa có dữ liệu phần cứng cụ thể để khẳng định 100%.
* **Độ khó:** Thiết lập mặc định (Normal) trong menu game.
* **Tài liệu đối chiếu:** 
  - Lượt chơi nhịp nhanh của Centerstrain01: [mark_of_the_ninja_level2_reverse_engineering.md](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level2_reverse_engineering.md)
  - Video gốc của Centerstrain01 (Level 2): [https://www.youtube.com/watch?v=wF6noSysObY](https://www.youtube.com/watch?v=wF6noSysObY) (11:06)
  - Khảo sát phương pháp luận: [game_walkthrough_reverse_engineering_findings.md](file:///f:/source/watch-skill/video-learning-vault/game-development/game_walkthrough_reverse_engineering_findings.md)

---

## 2. Công Cụ & Phương Pháp Thu Thập (Tools & Methodology)

1. **Tải và Lưu trữ Dữ liệu Gốc:**
   * Công cụ tải: `yt-dlp`.
   * Tệp video lưu trữ: `C:\Users\Admin\AppData\Local\Temp\vid_MSwyZ45L1r8.mp4` (Resolution: 640x360 @ 30 fps, Audio: AAC Stereo).
2. **Chuyển ngữ Âm thanh (Audio Transcription):**
   * Sử dụng `faster-whisper` (`base` model, `int8` quantization).
   * Tạo tệp transcript thô có kèm timestamp chính xác từng giây: [transcript.txt](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level2_slow_walkthrough/transcript.txt).
   * Phân tách thủ công giữa **Lời thoại/hướng dẫn trong game (In-game Voice & Subtitles)** và **Lời bình luận cảm xúc cá nhân của theRadBrad**.
3. **Trích xuất Khung hình Bằng chứng (Visual Frame Extraction):**
   * Sử dụng `ffmpeg` cắt chính xác các khung hình tại các mốc giây xảy ra tương tác quan trọng vào thư mục [frames/](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level2_slow_walkthrough/frames/).
   * Đọc văn bản trên HUD / UI bằng OCR kết hợp kiểm chứng mắt người trên chuỗi khung hình liên tiếp (chuỗi trước - trong - sau sự kiện).

---

## 3. Giới Hạn Kỹ Thuật & Các Điểm Cần Lưu Ý (Known Limitations & Caveats)

1. **Tính chất Lượt chơi (Exploratory / Unoptimized Play):**
   * theRadBrad tiếp tục phong cách chơi khám phá: thường dừng lại dò đường, đọc tutorial khi có cơ chế mới (laser, chó săn, cầu dao), mắc một số lỗi phán đoán dẫn đến báo động và phải ứng biến xử lý.
   * Lối chơi này không đại diện cho cách giải tối ưu hay toàn bộ tiềm năng cơ học của game, nhưng cung cấp dữ liệu thực nghiệm chân thực về phản ứng của hệ thống game khi người chơi mắc sai lầm.
2. **Không Suy Diễn Mã Nguồn hay Thuật Toán Nội Bộ:**
   * Mọi cơ chế được mô tả thuần túy theo hiện tượng quan sát được trên màn hình và âm thanh nghe được.
   * Các giá trị kích thước, khoảng cách hoặc thời gian ước lượng không được coi là thông số chuẩn của game gốc mà chỉ là giả định thiết kế phục vụ thử nghiệm prototype sau này.
3. **Phân Tách Dữ Liệu Thiếu (Gaps):**
   * Nếu trong video người chơi không gặp một tương tác cụ thể (ví dụ không để chó săn cắn, hoặc không thử kéo xác qua lưới laser), tình huống đó được ghi nhận rõ ràng là "Thiếu dữ liệu đối chứng", không tự suy diễn kết quả.
