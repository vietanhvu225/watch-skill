# 📋 Thông Tin Nguồn & Metadata: Level 4 ("A Change in Course")

> **Căn cứ tài liệu:** Scan tổng thể màn chơi Level 4 của Mark of the Ninja trong chiến dịch phân tích walkthrough 2 lượt phục vụ dự án game 2D stealth về Đặc công Việt Nam.

---

## 1. Thông Tin Định Danh Video

* **Tên video gốc trên YouTube:** [Mark of the Ninja - Gameplay Walkthrough - Part 4 [Level 4: A Change In Course] (Xbox 360)](https://www.youtube.com/watch?v=VUaFnEfMvy4)
* **Kênh phát hành:** [theRadBrad](https://www.youtube.com/@theRadBrad)
* **Ngày phát hành:** 13/09/2012
* **Thời lượng video:** 12 phút 22 giây (742 giây)
* **Nền tảng & Phiên bản:** Layout nút bấm hiển thị tay cầm Xbox (`A`, `B`, `X`, `Y`, `LT`, `RT`). Ghi nhận video chơi ở độ khó Normal.
* **Đặc tính cảnh quay:** Video chơi liên tục không cắt ghép, người chơi bình luận trực tiếp; có nhắc đến việc chơi lại màn này sau khi gặp lỗi kỹ thuật lưu video ở lần quay trước.

---

## 2. Trang Bị & Tiến Trình Đầu Màn

* **Trang bị sẵn có:** Kiếm Katana, Phi tiêu tre (`Darts`), Thiết bị tạo tiếng ồn (`Noisemaker`), Bom khói (`Smoke Bomb`), Bẫy chông sắt (`Caltrops / Spike Strips`).
* **Mục tiêu chính của màn (Objective):** Thâm nhập tòa tháp điều hành, tạo sự cố rò rỉ khí gas trong tầng hầm (`Gas Leak`), mở các van thông gió để lan tỏa khí gas khắp tòa nhà, sau đó phóng hỏa đốt tòa nhà làm nghi binh thu hút lính bảo vệ.

---

## 3. Công Cụ & Phương Pháp Kiểm Chứng

* **Dữ liệu âm thanh thoại:** Bóc tách 192 phân đoạn transcript phụ đề kèm timestamp bằng `faster-whisper` và WebVTT.
* **Kiểm chứng khung hình (Visual Verification):** Trích xuất khung hình độ nét cao bằng `ffmpeg` tại các mốc:
  - `00:03:09`: Hướng dẫn cảm biến chuyển động quét nhịp (`Stand Still when sensor sweeps across`).
  - `00:03:56`: Hướng dẫn kích hoạt rò rỉ khí gas tầng hầm (`Gas main leak`).
  - `00:05:04`: Mở các cửa hút gió (`Intake drawing gas upstairs`).
  - `00:07:00`: Hệ thống đếm van thông gió (`Vents open: 1 of 6`).
  - `00:11:15` – `00:12:14`: Leo lên mái nhà, kích hoạt hỏa hoạn và sàn mái nhà sụp đổ.
* **Nhận diện văn bản HUD:** Đối chiếu văn bản thông báo mục tiêu bằng OCR (`watch_skill.perceive.ocr`).
