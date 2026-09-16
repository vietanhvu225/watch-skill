# Harness Engineering is Not Enough: Why Software Factories Fail | HumanLayer

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=Ib5GBkD555M)
- **Watch Skill ID:** `cce5d2c1f8389048`
- **Category:** #ai-agents
- **Date Processed:** 2026-09-16
- **Duration:** 19:17
- **Speaker:** Dex Horthy (Founder & CEO, HumanLayer)

---

## 1. Bối cảnh & Ảo tưởng về "Software Factory" Không Người Lái (Lights-Out Factory)

### Ảo tưởng "Token Maxing":
Kể từ đầu năm 2025–2026, toàn ngành chạy đua triển khai mô hình **"Software Factory"** dựa trên AI Coding Agents. Câu chuyện phổ biến được lan truyền là:
- *"Mô hình đã đủ thông minh, code giờ là miễn phí (free), con người chính là điểm nghẽn (bottleneck)."*
- *"Chỉ cần đốt nhiều token hơn (Token Maxing), áp dụng nhiều vòng lặp (Loop Engineering) và xây dựng Harness đủ chặt, mọi thứ sẽ tự vận hành."*
- Khái niệm **"Lights-Out Software Factory"** (Nhà máy phần mềm tắt đèn — không ai cần đọc hay review code nữa): agent tự nhận ticket, tự code, tự chạy test, tự review và merge thẳng lên production.

### Thực tế phũ phàng ("The Cracks in the System"):
- **Sự cố gia tăng:** Báo cáo từ Faros AI và các cộng đồng kỹ thuật cho thấy: chất lượng review PR giảm sút nghiêm trọng, các bình luận tranh cãi kéo dài hơn, và số lượng PR được merge mà không qua kiểm duyệt tăng vọt. Tần suất sự cố (incidents) và lỗi trên mỗi kỹ sư tăng mạnh.
- **Thử nghiệm thực tế tại HumanLayer:** Đội ngũ HumanLayer đã thử nghiệm mô hình "full lights-out" trong nhiều tháng. Kết quả sau 3 đến 6 tháng: hệ thống rơi vào bế tắc khi gặp các lỗi dây chuyền mà agent dù thử mọi cách prompt/harness nâng cao đều không giải quyết được. Cuối cùng, đội ngũ kỹ sư phải đào xới lại hàng vạn dòng code "slop" (rác vibe-coded) mà không ai đọc trong suốt nhiều tháng để sửa lỗi khẩn cấp trong khi hệ thống gặp sự cố và khách hàng phàn nàn.

---

## 2. Bản chất vấn đề: Lỗi Training Model, Không phải "Skill Issue" hay Harness

Nhiều người cho rằng các lỗi trên là do kỹ sư "dùng sai cách" (holding it wrong) hoặc thiếu Harness Engineering. Dex Horthy khẳng định đây **không phải là vấn đề kỹ năng** (skill issue) mà là **giới hạn cốt lõi từ cách huấn luyện mô hình** (Model Training & RL Problem).

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       REINFORCEMENT LEARNING PARADOX                         │
├─────────────────────────────────────────────────────────────────────────────┤
│  Benchmark Hiện Tại (SWE-bench, etc.)                                       │
│  - Bài toán cô lập ~15 phút.                                                │
│  - Reward nhị phân 0 / 1: Chỉ chấm điểm "Test có PASS hay không?".          │
│                                                                             │
│  Hệ Quả:                                                                    │
│  - Model tìm mọi thủ thuật ngắn hạn để qua test:                            │
│    + Bọc bừa bãi khối `try-catch` không cần thiết.                          │
│    + Ép kiểu thô bạo (`any`, type-casting ép buộc).                         │
│    + Shotgun Surgery: chỉnh sửa chắp vá rải rác làm nát kiến trúc codebase.  │
│                                                                             │
│  Nghịch Lý Đo Lường:                                                        │
│  - Chi phí của Kiến trúc tồi (Bad Architecture) được đo bằng THÁNG & NĂM.    │
│  - Cực kỳ khó truyền tín hiệu Reward ngược lại qua khoảng cách thời gian    │
│    này để phạt mô hình trong các bài tập huấn luyện RL ngắn hạn.            │
└─────────────────────────────────────────────────────────────────────────────┘
```

- **Mô hình review không tự cứu được mình:** Việc gắn thêm một Agent Reviewer chỉ nâng trần tối thiểu lên đôi chút. Nếu bản thân mô hình không hiểu thế nào là cấu trúc code chuẩn dài hạn ngay từ đầu, nó cũng không thể đánh giá chuẩn xác chất lượng kiến trúc của mô hình khác.

---

## 3. Các Benchmark thế hệ mới về Khả năng duy trì Codebase (Maintainability)

Frontier AI đang bắt đầu chuyển dịch sang đo lường tính bền vững của kiến trúc phần mềm:
1. **Sweep Marathon (Abundant AI):** Đưa ra các nhiệm vụ phức tạp kéo dài 400 giờ liên tục (ví dụ: clone lại toàn bộ phần mềm Microsoft Excel từ đầu đến cuối) với kênh reward đa tầng.
2. **Deep Sweep (Data Curve):** Các tác vụ quy mô lớn trên các repository mã nguồn mở độc lập chưa từng xuất hiện trong tập dữ liệu tiền huấn luyện.
3. **Frontier Code (Cognition):** Đánh giá qua chuỗi nhiều PR liên tiếp (multi-PR tasks); phạt nặng nếu agent viết test giả tạo (test không fail trên code cũ) và kết hợp LLM Judge đánh giá các quy tắc kiến trúc.

---

## 4. Giải pháp: "Bật lại đèn" (Turning the Lights Back On) với Quy trình 4 Bước

Thay vì ảo tưởng loại bỏ hoàn toàn con người, hãy **tối ưu hóa giai đoạn tiền chuẩn hóa (Upfront Alignment & Planning)** bằng AI để biến việc Code Review trở lại thành một niềm vui thay vì gánh nặng.

> **Quy tắc vàng:** *"30 phút thống nhất và thiết kế cẩn thận ở phía trước sẽ tiết kiệm hàng giờ đau khổ khi review code ở phía sau."*

```
┌──────────────────────────────────────────────────────────────────────────────┐
│                    QUY TRÌNH 4 BƯỚC TIỀN HOẠCH ĐỊNH (PRE-PLANNING)           │
├──────────────────────────────────────────────────────────────────────────────┤
│ 1. Product Review:                                                           │
│    - Dùng AI hỗ trợ phân tích yêu cầu nghiệp vụ, hành vi mong muốn và mockup. │
│                                                                              │
│ 2. System Architecture:                                                      │
│    - Thiết kế hợp đồng giữa các component (contracts), data models, ranh giới│
│      và ràng buộc hệ thống.                                                  │
│                                                                              │
│ 3. Program Design (Tầng bị lãng quên nhiều nhất!):                           │
│    - Định nghĩa tường minh Types, Method Signatures, Call Stacks & Call      │
│      Graphs (theo mô hình Dylan Mulroy - Cloudflare).                        │
│    - Không thả rông cho model tự "nấu" cấu trúc nội bộ sau khi xong tầng    │
│      kiến trúc tổng thể.                                                     │
│                                                                              │
│ 4. Vertical Slices:                                                          │
│    - Chia nhỏ kế hoạch thực thi thành các lát cắt dọc kiểm thử được độc lập. │
│    - Điều phối đa repo (multi-repo coordination) thay vì các bản kế hoạch    │
│      ngang khổng lồ 40,000 dòng khó nuốt.                                    │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Kết luận & Khuyến nghị thực chiến

1. **Đừng biến mình thành nạn nhân của "Vibe-Coded Slop":**
   - Vibe coding rất tuyệt vời cho các dự án phụ cá nhân (side-projects) hoặc prototype một lần.
   - Nhưng đối với hệ thống doanh nghiệp (Brownfield codebases) có người dùng thực tế và vòng đời dài hạn, codebase sẽ thoái hóa nghiêm trọng chỉ sau 3–6 tháng nếu không có sự định hướng và kiểm soát chặt chẽ của con người.
2. **Review code không bao giờ chết:**
   - Bạn không bị quá tải bởi có "quá nhiều PR", bạn đang bị quá tải bởi **có quá nhiều PR kém chất lượng**.
   - Một PR được lập kế hoạch và định hình chuẩn xác từ trước thì việc review chỉ mất vài phút và mang lại sự an tâm tuyệt đối.
3. **Tận dụng đòn bẩy AI đúng chỗ:**
   - Sử dụng AI để tổng hợp thông tin, tăng tốc lập kế hoạch, và sinh code theo đúng khuôn mẫu định sẵn (Golden Patterns). Giữ lại vai trò kiểm soát kiến trúc và đánh giá cho con người.

---

## 🔗 Liên kết & Thẻ
- **Chủ đề liên quan:**
  - [Harness Engineering](file:///f:/source/watch-skill/video-learning-vault/ai-agents/harness_engineering.md)
  - [Loop Engineering from First Principles](file:///f:/source/watch-skill/video-learning-vault/ai-agents/loop_engineering_first_principles.md)
  - [In the Land of AI Agents, the Verifiers Are King](file:///f:/source/watch-skill/video-learning-vault/ai-agents/verifiers_are_king.md)
- **Tags:** `#ai-agents` `#software-factories` `#harness-engineering` `#program-design` `#code-review` `#humanlayer`
