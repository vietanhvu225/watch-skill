# Nguồn Dữ Liệu & Giới Hạn Kỹ Thuật (Provenance & Limitations)

> **Mục đích tài liệu:** Ghi nhận nguồn gốc dữ liệu, công cụ phân tích thực nghiệm và các giới hạn quan sát trong quá trình phân tích video walkthrough *Mark of the Ninja — Level 1: "Ink & Dreams"*.  
> **Dự án ứng dụng:** Nghiên cứu thiết kế trải nghiệm, cơ chế hướng dẫn (onboarding) và vòng lặp tương tác cho game 2D stealth về Đặc công Việt Nam (Single-player, 1 nhân vật điều khiển, NPC hỗ trợ).

---

## 1. Thông Tin Nguồn Video (Video Provenance)

* **URL:** [https://www.youtube.com/watch?v=e1UnoyKqeMs](https://www.youtube.com/watch?v=e1UnoyKqeMs)
* **Tiêu đề gốc:** `Mark of the Ninja - Gameplay Walkthrough - Part 1 [Level 1: Ink & Dreams] (Xbox 360/PS3/PC)`
* **Kênh phát hành:** [theRadBrad](https://www.youtube.com/@theRadBrad) (YouTuber chuyên walkthrough gaming uy tín, nổi tiếng với phong cách chơi tự nhiên, quan sát cốt truyện và phản xạ chân thực).
* **Ngày phát hành:** 08/09/2012 (Thời điểm ra mắt chính thức của game trên hệ máy Xbox 360 qua Xbox Live Arcade).
* **Thời lượng video:** 20 phút 21 giây (1,221 giây).
* **Nền tảng vận hành trong video:** Xbox 360 (Giao diện hiển thị nút tay cầm Xbox: `A`, `B`, `X`, `Y`, `LT`, `RT`, `D-Pad`).
* **Độ khó:** Normal (Thiết lập mặc định lần đầu chơi của tựa game).

---

## 2. Công Cụ & Phương Pháp Thu Thập (Tools & Methodology)

1. **Tải và Trích xuất Dữ liệu Gốc:**
   * Công cụ tải: `yt-dlp` (phiên bản `2024.x` với cờ player client `android,web`).
   * Tệp video lưu trữ tạm thời: `C:\Users\Admin\AppData\Local\Temp\vid_e1UnoyKqeMs.mp4` (Dung lượng: 58.17 MB, Độ phân giải: 640x360 @ 29.97 fps, Audio: AAC Stereo 44.1 kHz).
2. **Chuyển ngữ Âm thanh (Audio Transcription):**
   * Sử dụng mô hình `faster-whisper` (`base` model, `int8` CPU quantization).
   * Tạo tệp transcript thô có kèm timestamp chính xác từng giây: [transcript.txt](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/transcript.txt).
   * Phân tách thủ công và đối chiếu giữa **Lời thoại nhân vật trong game (In-game Voice & Subtitles)** và **Lời bình luận của YouTuber (theRadBrad Commentary)**.
3. **Trích xuất Khung hình Bằng chứng (Visual Frame Extraction):**
   * Sử dụng `ffmpeg` cắt chính xác các khung hình tại các mốc giây xảy ra tương tác quan trọng vào thư mục [frames/](file:///f:/source/watch-skill/video-learning-vault/game-development/mark_of_the_ninja_level1_slow_walkthrough/frames/).
   * Đọc văn bản trên HUD / UI bằng mô hình OCR (`watch_skill.perceive.ocr`) kết hợp kiểm chứng trực tiếp bằng mắt người.

---

## 3. Giới Hạn Kỹ Thuật & Các Điểm Cần Lưu Ý (Known Limitations & Caveats)

1. **Khác biệt về Phong cách Chơi (Playstyle Variance):**
   * Đây là một lượt chơi mang tính **khám phá lần đầu (Blind / Casual Playthrough)**: theRadBrad không hề biết trước vị trí bẫy, vị trí cổ vật hay lịch trình tuần tra của lính.
   * Người chơi thường xuyên dừng lại 5–10 giây để đọc hướng dẫn chữ nổi trên màn hình, nghe thoại cutscene, quan sát nón ánh sáng đèn pin trước khi ra quyết định.
   * **Tuyệt đối không đồng nhất:** Hành động chậm rãi của người chơi là nhịp độ tự nhiên của một người tiếp cận game lần đầu, không đại diện cho toàn bộ các cách giải hay giới hạn tối đa của hệ thống cơ học game.
2. **Âm thanh và Tạp âm Lời bình:**
   * theRadBrad nói chuyện và phản ứng liên tục trong quá trình chơi. Một số hiệu ứng âm thanh môi trường rất nhỏ (như tiếng sột soạt nhẹ của cỏ hay tiếng bước chân rón rén) có thể bị tiếng bình luận lấn át. Tuy nhiên, các tín hiệu âm thanh quan trọng (sóng âm bước chạy, tiếng chuông rung, còi báo động, nhạc chuyển pha chiến đấu) đều được nghe rõ và có phản hồi thị giác đồng bộ trên HUD.
3. **Giới hạn Góc nhìn Tay cầm (No Input Overlay):**
   * Video không có overlay hiển thị cần gạt hay lực nhấn nút. Mức độ tác động input chỉ được xác nhận khi game hiện icon tutorial hướng dẫn ngữ cảnh (Contextual Prompts) hoặc qua gia tốc chuyển động hiển thị của nhân vật.
4. **Không có Cắt dựng Rác (Unedited Footage):**
   * Video được thu âm liền mạch từ menu khởi động đến cutscene kết thúc màn 1, không có tua nhanh hay cắt ghép giấu lỗi. Nếu người chơi chết hoặc restart checkpoint, video sẽ ghi nhận trung thực.
