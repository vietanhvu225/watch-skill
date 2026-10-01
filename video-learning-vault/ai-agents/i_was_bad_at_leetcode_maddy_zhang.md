# I was Bad at LeetCode. Then I did This — Maddy Zhang

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=Q5QoGocSnjo)
- **Watch Skill ID:** `771dbc200ae13fd2`
- **Kênh phát hành:** Maddy Zhang (Senior Software Engineer tại Google, cựu kỹ sư tại Amazon, IBM, Microsoft)
- **Category:** #dsa, #leetcode, #engineering-interviews, #algorithms, #career, #problem-solving, #faang-interviews
- **Date Processed:** 2026-10-01
- **Duration:** 09:49 (589.0 giây)
- **Transcript File:** [i_was_bad_at_leetcode_maddy_zhang_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/i_was_bad_at_leetcode_maddy_zhang_transcript.txt)

---

## 1. Bản Chất Của LeetCode Trong Kỷ Nguyên AI 2026

Maddy Zhang mở đầu bằng việc đập tan ảo tưởng về "số lượng bài làm" (Problem Count):
> *"Tôi đã giải hơn 400 bài LeetCode và trúng tuyển Google, Amazon, Microsoft, IBM. Nhưng khi ngồi ở phía bên kia bàn phỏng vấn tại Google, tôi nhận ra: Ứng viên xuất sắc nhất không phải là người cày nhiều bài nhất. Họ là người có khả năng nhìn vào một bài toán hoàn toàn xa lạ, nhận ra quy luật ngầm (pattern) và chọn đúng công cụ ngay lập tức."*

### Tại sao LeetCode vẫn tồn tại song song với phỏng vấn AI?
- **Các Big Tech và AI Labs hàng đầu (OpenAI, Anthropic, Google):** Vẫn duy trì các vòng thi thuật toán trực tiếp trên môi trường live coding (CodeSignal, Colab) hoặc editor thuần không cho phép AI.
- **Hội chứng teo cơ cú pháp (Syntax Atrophy):** Nếu cả năm bạn chỉ dùng AI để tạo code, não bộ sẽ bị thụ động. Khi bước vào phòng phỏng vấn không có Copilot/Cursor gợi ý, bạn sẽ lúng túng ngay cả với các cú pháp vòng lặp cơ bản.
- **Sự thay đổi trọng tâm:** Phỏng vấn hiện nay không kiểm tra việc bạn nhớ lời giải của 300 bài LeetCode, mà kiểm tra tư duy lập luận (reasoning), khả năng gỡ lỗi (debugging), thiết kế và xác thực quyết định kỹ thuật.

---

## 2. Bản Đồ 9 Mẫu Hình (Patterns) Giải Quyết 90% Bài Toán LeetCode

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                          BẢN ĐỒ 9 PATTERNS GIẢI THUẬT CỐT LÕI                                │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                             │
│  [ 1. HASHING ] ───────────── Tra cứu O(1): "Đã thấy bao giờ chưa?" / "Tần suất?"          │
│  [ 2. TWO POINTERS ] ──────── Mảng Sorted hoặc Bài toán Cặp: Đi 2 đầu hoặc Nhanh/Chậm      │
│  [ 3. SLIDING WINDOW ] ────── Đoạn liên tục (Contiguous): Cập nhật trạng thái vi mô        │
│  [ 4. BINARY SEARCH ] ─────── Không gian đáp án đơn điệu (Monotonic Answer Space)           │
│  [ 5. MONOTONIC STACK ] ───── Tìm phần tử lớn/nhỏ hơn gần nhất ở 2 phía                     │
│  [ 6. TREES & GRAPHS ] ────── DFS (Đường đi, Đảo liên thông) vs BFS (Đường đi ngắn nhất)    │
│  [ 7. TOP-K HEAP ] ────────── K phần tử lớn nhất dùng Min-Heap size K (O(N log K))          │
│  [ 8. BACKTRACKING ] ──────── Tìm "tất cả cấu hình" (Choose ➔ Recurse ➔ Undo)               │
│  [ 9. DYNAMIC PROGRAMMING ] ─ Đệ quy tránh tính lặp: Brute Force ➔ Memoization             │
│                                                                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Phân Tích Chi Tiết Từng Pattern & Dấu Hiệu Nhận Biết

### Pattern 1: Hashing (Bảng băm tra cứu $O(1)$)
- **Dấu hiệu nhận biết:** Câu hỏi dạng *"Tôi đã thấy phần tử này trước đây chưa?"* hoặc *"Phần tử này xuất hiện bao nhiêu lần?"*.
- **Cơ chế:** Khi giải pháp Brute Force quét mảng 2 lần để đối chiếu, HashMap thu gọn vòng lặp lồng nhau thành một phép tra cứu $O(1)$, đưa độ phức tạp từ $O(N^2)$ về $O(N)$.
- **Bài toán kinh điển:**
  - *Two Sum:* Thay vì duyệt cặp, duyệt 1 lần và kiểm tra `target - current` có trong Map chưa.
  - *Longest Consecutive Sequence:* Đẩy vào Set, chỉ bắt đầu đếm từ số không có láng giềng bên trái để giữ thời gian tuyến tính $O(N)$.
  - *Group Anagrams:* Dùng chuỗi ký tự đã sắp xếp làm Key, danh sách từ làm Value.

### Pattern 2: Two Pointers (Hai con trỏ)
- **Dấu hiệu nhận biết:** Dữ liệu đầu vào **đã sắp xếp (Sorted)** hoặc bài toán liên quan đến các cặp đối xứng.
- **Hai biến thể:**
  1. *Hai con trỏ đi ngược chiều:* Bắt đầu ở hai đầu xa nhất và co lại.
     - *Container With Most Water:* Mỗi bước luôn dịch con trỏ ở bức tường thấp hơn (vì dịch tường cao hơn chỉ làm giảm diện tích).
     - *3Sum:* Cố định 1 số bằng vòng lặp ngoài, dùng Two Pointers quét 2 số còn lại.
  2. *Con trỏ nhanh & chậm (Fast & Slow Pointers):* Con trỏ chậm đi 1 bước, con trỏ nhanh đi 2 bước.
     - *Linked List Cycle:* Nếu có vòng lặp, hai con trỏ chắc chắn va chạm.
     - *Middle of Linked List:* Khi con trỏ nhanh đến đích, con trỏ chậm đang ở chính giữa danh sách.

### Pattern 3: Sliding Window (Cửa sổ trượt)
- **Dấu hiệu nhận biết:** Từ khóa **Contiguous (liên tục)** như: Chuỗi con dài nhất (longest substring), mảng con ngắn nhất, tổng lớn nhất của $K$ phần tử liên tiếp.
- **Quy tắc vàng:** **Cập nhật trạng thái vi mô (Incremental Update)** — 1 phần tử vào thì 1 phần tử ra. Nếu bạn tính lại tổng cửa sổ từ đầu ở mỗi bước, bạn chỉ đang viết vòng lặp lồng nhau giả danh cửa sổ trượt!
- **Bài toán kinh điển:** *Longest Substring Without Repeating Characters*, *Minimum Window Substring*.

### Pattern 4: Binary Search Trên Không Gian Đáp Án (Answer Space)
- **Dấu hiệu nhận biết:** Không có mảng sắp xếp cụ thể, nhưng bài toán hỏi về giá trị nhỏ nhất/lớn nhất thỏa mãn điều kiện và mang tính chất **đơn điệu (Monotonic Yes/No)**: *"Nếu đáp án $X$ thỏa mãn, thì mọi giá trị lớn hơn $X$ cũng thỏa mãn"*.
- **Sức mạnh:** Biến một bài toán phải quét qua hàng triệu khả năng thành khoảng **20 bước kiểm tra** ($\log_2(1,000,000) \approx 20$).
- **Bài toán kinh điển:**
  - *Koko Eating Bananas:* Tìm tốc độ ăn chuối nhỏ nhất. Nếu tốc độ 7 nải/giờ kịp giờ thì tốc độ 8 chắc chắn kịp ➔ Binary search từ 1 đến kích thước nải chuối lớn nhất.
  - *Capacity To Ship Packages Within D Days:* Tìm tải trọng tàu tối thiểu.

### Pattern 5: Monotonic Stack (Ngăn xếp đơn điệu)
- **Dấu hiệu nhận biết:** Với mỗi vị trí trong mảng, tìm phần tử lớn hơn hoặc nhỏ hơn **gần nhất** ở phía bên phải hoặc bên trái.
- **Cơ chế:** Duy trì một Stack luôn có thứ tự (tăng dần hoặc giảm dần). Khi một phần tử mới vào phá vỡ thứ tự, liên tục pop các phần tử cũ ra và ghi nhận khoảng cách. Mỗi phần tử chỉ được push và pop đúng 1 lần ➔ Runtime luôn là $O(N)$.
- **Bài toán kinh điển:**
  - *Daily Temperatures:* Tìm số ngày chờ đến khi trời ấm hơn.
  - *Trapping Rain Water* & *Largest Rectangle in Histogram:* Những câu hỏi phỏng vấn hóc búa nhất của Big Tech trở nên cực kỳ trực quan khi áp dụng Monotonic Stack.

### Pattern 6: Trees & Graphs (DFS vs. BFS)
- **Depth-First Search (DFS):** Đi sâu nhất có thể rồi quay lui (thường dùng đệ quy). Dùng khi cần duyệt hết đường đi hoàn chỉnh, tính toán bottom-up, hoặc đếm vùng liên thông (*Number of Islands*).
- **Breadth-First Search (BFS):** Đi theo từng tầng (level-by-level) bằng Queue. **Dùng ngay khi xuất hiện từ khóa "Shortest Path" (đường đi ngắn nhất) trong đồ thị không có trọng số.**
  - *Binary Tree Level Order Traversal:* BFS theo định nghĩa.
  - *Rotting Oranges:* BFS đa điểm xuất phát (Multi-source BFS).

### Pattern 7: Top-K With Heap (Hàng đợi ưu tiên)
- **Dấu hiệu nhận biết:** $K$ phần tử lớn nhất, nhỏ nhất, hoặc xuất hiện nhiều nhất.
- **Tối ưu hóa:** Thay vì sắp xếp mảng tốn $O(N \log N)$, dùng Heap kích thước $K$ chỉ tốn $O(N \log K)$. Khi $N$ rất lớn và $K$ nhỏ, đây là mức nhảy vọt về hiệu năng.
- **Nghịch lý Heap:** **Muốn tìm $K$ phần tử lớn nhất, bạn phải dùng MIN-HEAP!** Heap giữ đúng $K$ phần tử, phần tử nhỏ nhất trong nhóm $K$ này sẽ nằm trên đỉnh, sẵn sàng bị đẩy ra khi có một phần tử lớn hơn xuất hiện.

### Pattern 8: Backtracking (Quay lui)
- **Dấu hiệu nhận biết:** Bài toán yêu cầu **"tất cả cấu hình" (All Subsets, Permutations, Combinations, Valid Board Configurations)** thay vì tìm một giá trị tối ưu duy nhất. Nếu kết quả trả về là một mảng của các mảng (`List<List<T>>`), 99% đó là Backtracking.
- **Cấu trúc bất biến:**
  ```python
  def backtrack(path, options):
      if is_solution(path):
          result.append(list(path))
          return
      for choice in options:
          make_choice(choice)      # 1. Chọn
          backtrack(path, options) # 2. Đệ quy
          undo_choice(choice)      # 3. Hoàn tác (Undo)
  ```
- **Lưu ý:** Độ phức tạp thời gian luôn ở mức hàm mũ (Exponential), đây là điều hoàn toàn bình thường đối với bài toán duyệt tổ hợp.

### Pattern 9: Dynamic Programming (Quy hoạch động)
- **Bản chất:** Đệ quy nhưng ngừng việc tính toán lại những thứ đã tính rồi (Recursion without recomputing).
- **Dấu hiệu nhận biết:** Đếm số cách thực hiện (number of ways) hoặc tìm min/max qua một chuỗi các quyết định phụ thuộc nhau.
- **Phương pháp tiếp cận:**
  1. Viết giải pháp đệ quy Brute Force trước.
  2. Xác định các tham số bị lặp lại qua các lần gọi hàm.
  3. Áp dụng bảng nhớ (Memoization Cache) để lưu lại kết quả.
- **Bài toán kinh điển:** *Climbing Stairs*, *Coin Change*, *House Robber*.

---

## 4. Lời Khuyên Hành Động: Chiến Lược Học Tập Thực Chiến

1. **Đừng học lan man:** Thay vì giải 400 bài ngẫu nhiên, hãy tập trung vào 9 patterns này.
2. **Quy tắc 5-10:** Mỗi pattern chỉ cần giải nhuần nhuyễn từ 5 đến 10 bài điển hình.
3. **Luyện viết code chay:** Dù bạn dùng AI code hàng ngày, hãy dành 30 phút mỗi tuần tự gõ code thuật toán trên giấy hoặc editor trắng để giữ cho phản xạ cú pháp không bị mai một.
