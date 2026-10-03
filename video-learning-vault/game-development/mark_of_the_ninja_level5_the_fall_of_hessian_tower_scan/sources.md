# 📋 Thông Tin Nguồn & Metadata: Level 5 ("The Fall of Hessian Tower")

> **Căn cứ tài liệu:** Scan tổng thể màn chơi Level 5 của Mark of the Ninja trong chiến dịch phân tích walkthrough 2 lượt phục vụ dự án game 2D stealth về Đặc công Việt Nam.

---

## 1. Thông Tin Định Danh Video

* **Tên video gốc trên YouTube:** [Mark of the Ninja - Gameplay Walkthrough - Part 5 [Level 5: The Fall of Hessian Tower] (Xbox 360)](https://www.youtube.com/watch?v=CVhlTVVzlsk)
* **Kênh phát hành:** [theRadBrad](https://www.youtube.com/@theRadBrad)
* **Ngày phát hành:** 14/09/2012
* **Thời lượng video:** 20 phút 03 giây (1,203 giây)
* **Nền tảng & Phiên bản:** Layout nút bấm hiển thị tay cầm Xbox (`A`, `B`, `X`, `Y`, `LT`, `RT`). Độ khó Normal.
* **Đặc tính cảnh quay:** Video chơi liên tục không cắt ghép (raw gameplay), thu lại đầy đủ các phân đoạn đối đầu với lính bắn tỉa, chết do rơi vào tầm ngắm sniper và trận đấu trùm Kelly.

---

## 2. Trang Bị & Tiến Trình Đầu Màn

* **Trang bị sẵn có:** Kiếm Katana, Phi tiêu tre (`Darts`), Thiết bị tạo tiếng ồn (`Noisemaker`), Bom khói (`Smoke Bomb`), Bẫy chông sắt (`Spike Strips`).
* **Mục tiêu chính của màn (Objective):** Đột nhập tòa tháp trung tâm Hessian đang chìm trong biển lửa do Level 4 tạo ra, truy lùng Count Karajan và tiêu diệt chỉ huy Kelly (người trực tiếp chỉ huy cuộc đột kích tàn sát tộc ninja ở màn 1).

---

## 3. Công Cụ & Phương Pháp Kiểm Chứng

* **Dữ liệu âm thanh thoại:** Bóc tách 279 phân đoạn transcript phụ đề kèm timestamp bằng `faster-whisper` và WebVTT.
* **Kiểm chứng khung hình (Visual Verification):** Trích xuất khung hình độ nét cao bằng `ffmpeg` tại các mốc:
  - `00:09:58`: Cutscene giữa màn — Karajan trốn chạy bằng trực thăng, để Kelly ở lại chặn hậu.
  - `00:14:07`: Giới thiệu lính bắn tỉa (`Watch out! The sniper can end your life with just one shot... find a way to block his scope`).
  - `00:14:33`: Người chơi bị sniper bắn trúng và chết (`Target sighted... Shots fired`).
  - `00:17:10` – `00:19:40`: Trận đấu trùm Kelly (`Kelly seems to think you'll face him like a glorious samurai...`).
* **Nhận diện văn bản HUD:** Đối chiếu chữ trên giao diện bằng OCR (`watch_skill.perceive.ocr`).
