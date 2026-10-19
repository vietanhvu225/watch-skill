# Giải Phẫu Pstack: Cách Kỹ Sư Cursor Vận Hành & Giao Mã Bằng AI | Eric Tech (Phân Tích Ngăn Xếp Của Lauren Tan)

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=4wBVuj2an8E)
- **Video ID:** `4wBVuj2an8E`
- **Category:** #ai-agents
- **Date Processed:** 2026-10-20
- **Duration:** 14:56
- **Speaker:** Eric Tech (Phân tích kiến trúc Pstack của Lauren Tan — Principal Engineer tại SpaceX, Technical Staff tại Cursor, cựu React Core Team)
- **Transcript File:** [pstack_how_cursor_engineer_ships_code_eric_tech_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/pstack_how_cursor_engineer_ships_code_eric_tech_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

Trong video chuyên sâu này, **Eric Tech** phân tích toàn diện kho tri thức mã nguồn mở **Pstack** ("Potato Stack") do **Lauren Tan** (`poteto`) xây dựng. Lauren Tan là một trong những bộ óc kỹ thuật có tầm ảnh hưởng lớn nhất trong phong trào Agentic Engineering: Principal Engineer tại SpaceX, thành viên nhóm kỹ thuật tại Cursor (xAI / GrokBot), cựu kỹ sư Meta và cựu thành viên nhóm nòng cốt React (React Core Team).

Eric cấu trúc lại toàn bộ hệ thống kỹ năng và quy tắc của Pstack theo **6 Giai Đoạn Vòng Đời Phát Triển Phần Mềm AI (AI-SDLC)**, chỉ rõ lý do tại sao quy trình của Lauren Tan có thể giúp một kỹ sư vận hành hàng ngàn PRs mỗi tháng mà không gây suy thoái mã nguồn:
1. **Thiên hướng Dự án Hiện hữu (Brownfield Mastery):** Khác với các bộ kỹ năng chú trọng làm dự án mới từ số 0 (greenfield), Pstack tối ưu hóa tuyệt đối cho các codebase lớn đã tồn tại qua bộ 3 kỹ năng đọc hiểu: `how`, `why`, và `teach`.
2. **Thiết kế Đa Mô Hình (Multi-Model Arena & Interrogate):** Không bao giờ tin tưởng một mô hình duy nhất khi hoạch định kiến trúc. Kích hoạt `arena` để Opus, GPT và Grok cùng đua phương án, sau đó dùng `interrogate` làm thẩm phán chắt lọc phần tinh túy nhất.
3. **Kỷ Luật "Xóa Sạch Comments" (`no-comments`):** Một triết lý độc đáo: Xóa bỏ các dòng chú thích thừa thãi trong code vì AI có xu hướng đọc bình luận sai lệch thay vì phân tích mã nguồn thực tế.
4. **Kỹ Năng Xác Minh Đầu Cuối (`create-verification-skill` & `swarm`):** Phân biệt rõ rệt giữa unit test (TDD) và verification skill (khởi tạo database tạm, chạy máy chủ, thao tác UI thực tế, thu thập bằng chứng screenshots/video).
5. **Nguyên Tắc "Tấn Công Giả Định" (`Attack the Premise`):** Chấm dứt vòng lặp sửa lỗi vô tận (Fix ➔ Fail ➔ Fix ➔ Fail) bằng cách ép AI dừng lại và hoài nghi tiền đề ban đầu.
6. **Loại Trừ Trạng Thái Bất Khả Thi (Reject Impossible States):** Ràng buộc kiểu dữ liệu TypeScript nghiêm ngặt, biến trạng thái sai thành lỗi biên dịch (Compile-time error).

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                          PSTACK 6-STAGE AI-SDLC ARCHITECTURE                                │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. PLANNING (Brownfield Focus)                                                              │
│    - Learn the code first: `how`, `why`, `teach` skills                                     │
│    - Plan big job: `figure-it-out`, `show-me-your-work`                                     │
│                                           │                                                 │
│                                           ▼                                                 │
│ 2. DESIGNING (Settle the Code Shape)                                                        │
│    - `architect` triggers `arena`: Multi-model fanout (Opus, GPT, Grok)                     │
│    - `interrogate`: Judge model extracts best parts into unified blueprint                  │
│                                           │                                                 │
│                                           ▼                                                 │
│ 3. BUILDING & CODING                                                                        │
│    - `tdd` loop + `typescript-best-practice`                                                │
│    - `cleanup-diff` / `d-slop`: Remove extra comments (`no-comments` rule), nested catches  │
│                                           │                                                 │
│                                           ▼                                                 │
│ 4. TESTING & VERIFICATION                                                                   │
│    - `prove application works`: `create-verification-skill` (E2E env bootstrap + evidence)  │
│    - `swarm`: Multi-agent parallel verification partitioned by feature folder               │
│    - `attack-the-difference` & `thermonuclear-code-quality-review`                          │
│                                           │                                                 │
│                                           ▼                                                 │
│ 5. DEPLOYMENT & PR MANAGEMENT                                                               │
│    - `pr-easy-review`, `babysit-pr`, `fix-merge-conflicts`, `get-pr-comments`, `fix-ci`     │
│                                           │                                                 │
│                                           ▼                                                 │
│ 6. SECONDARY COGNITIVE SKILLS & PRINCIPLES                                                  │
│    - Router: `/potato` mode                                                                 │
│    - Context Recall: `/recall`                                                              │
│    - Principles: `Attack the Premise` & `Reject Impossible States`                          │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Giải Phẫu 6 Giai Đoạn Phát Triển Phần Mềm (AI-SDLC) Trong Pstack

### Giai Đoạn 1: Lập Kế Hoạch Cho Dự Án Hiện Hữu (Brownfield Planning)
- **Quy trình "Hiểu mã trước khi sửa" (Learn the Code First):**
  - Kỹ năng `/how`: Phân tích luồng thực thi cơ học của mã nguồn.
  - Kỹ năng `/why`: Đào sâu lý do thiết kế, kết nối lịch sử commit, ADRs (Architectural Decision Records) và ngữ cảnh nghiệp vụ.
  - Kỹ năng `/teach`: Kết hợp cả *how* và *why* để tóm tắt cho kỹ sư trước khi động vào code.
- **Quy trình phân rã việc lớn:** Dùng `figure-it-out` và `show-me-your-work` để chia nhỏ task thành các đầu việc vi mô, ghi vết vào tài liệu Markdown.

---

### Giai Đoạn 2: Thiết Kế Hình Thái Mã Nguồn (Design - Settle Code Shape)
- **Đua mô hình (Arena Mode):** Thay vì chỉ dùng Claude hoặc GPT, Pstack kích hoạt nhiều mô hình độc lập cùng giải một bài toán thiết kế.
- **Thẩm vấn & Tổng hợp (Interrogate):** Một mô hình đóng vai trò thẩm phán độc lập (Judge) sẽ chất vấn từng phương án, chỉ ra điểm yếu và kết hợp những giải pháp tinh túy nhất của từng mô hình vào một bản thiết kế chung.

---

### Giai Đoạn 3: Lập Trình & Kỷ Luật Dọn Dẹp Mã Nguồn (Building & D-Slop)
- **Kỹ nghệ TDD:** Viết bài kiểm thử trước, ép mô hình viết code chỉ để vượt qua bài test.
- **Kỹ năng `no-comments` (Xóa bỏ chú thích thừa):**
  - **Lý do kỹ thuật sâu sắc:** Khi mã nguồn có chú thích (comment), AI thường có xu hướng tin vào lời bình luận thay vì đọc mã nguồn logic. Nếu comment bị cũ hoặc mô tả sai so với code thực tế, AI sẽ sinh ra ảo giác trầm trọng.
  - Việc loại bỏ triệt để comments rác buộc mô hình phải đọc hiểu 100% dòng lệnh thực thi.
- **Kỹ năng `cleanup-diff` / `d-slop`:** Tự động rà soát nhánh tính năng với nhánh `main`, loại bỏ các khối `try/catch` bừa bãi, các biến không định kiểu và cấu trúc lồng nhau (deep nesting).

---

### Giai Đoạn 4: Thử Nghiệm & Xác Minh Đầu Cuối (Testing & Verification)
- **Phân biệt Unit Test và Verification Skill:**
  - `tdd`: Kiểm tra tính đúng đắn của hàm (Functional/Unit level).
  - `create-verification-skill`: Thiết lập môi trường hoàn chỉnh (dựng database tạm thời, khởi động server, giả lập hành vi người dùng bằng browser, thu thập ảnh chụp màn hình và video bằng chứng, sau đó dọn dẹp hạ tầng).
- **Quy tắc Một tính năng Một thư mục (One feature per folder):** Mỗi module được quản lý trong thư mục cô lập, đi kèm bộ hướng dẫn kiểm thử riêng biệt.
- **Review Hạt Nhân (`thermonuclear-code-quality-review`):** Lớp review code cực kỳ khắt khe nhằm gọt giũa mã nguồn về trạng thái tối giản nhất, loại bỏ mọi đoạn code phình to không cần thiết.

---

### Giai Đoạn 5 & 6: Vận Hành PR & Các Nguyên Tắc Bất Biến

#### Bộ Công Cụ Tự Động Hóa PR:
- `babysit-pr`: Giám sát PR từ lúc mở cho đến khi merge thành công.
- `fix-merge-conflicts`: Tự động giải quyết xung đột nhánh khi có kỹ sư khác merge trước.
- `fix-ci`: Tự phân tích log kiểm thử CI thất bại và đẩy commit sửa lỗi.

#### Hai Nguyên Tắc Kiến Trúc Đỉnh Cao:
1. **Tấn Công Giả Định (`Attack the Premise`):**
   - Khi gặp tình trạng AI sửa lỗi lặp đi lặp lại nhiều lần mà vẫn hỏng (Fix ➔ Fail ➔ Fix ➔ Fail), kỹ năng này ép AI dừng lại, không được phép tiếp tục vá víu mà phải quay lại đặt câu hỏi: *"Liệu giả định ban đầu về kiến trúc hay luồng dữ liệu của chúng ta có hoàn toàn sai từ gốc không?"*.
2. **Loại Trừ Trạng Thái Bất Khả Thi (Reject Impossible States):**
   - Tận dụng hệ thống kiểu dữ liệu TypeScript (Union types, Discriminated unions) để triệt tiêu các trạng thái phi lý (ví dụ: Một giao dịch không thể vừa có trạng thái `paid` vừa `pending` cùng một lúc).

---

## 3. Bảng Đối Chiếu: Pstack vs. Các Bộ Kỹ Năng Thông Thường

| Tiêu chí so sánh | Bộ Prompt / Skills Thông Thường | Ngăn Xếp Pstack (Lauren Tan) |
| :--- | :--- | :--- |
| **Trọng tâm dự án** | Greenfield (Dự án mới từ đầu) | Brownfield (Hệ thống lớn, code có sẵn) |
| **Chiến lược mô hình** | Dùng 1 mô hình duy nhất từ đầu đến cuối | Arena đa mô hình + Thẩm phán Interrogate |
| **Quan điểm về Comment** | Thường yêu cầu AI viết comment giải thích | `no-comments`: Xóa sạch comment để AI đọc code thật |
| **Tiêu chuẩn nghiệm thu** | Dừng lại khi code không báo lỗi syntax | Bắt buộc có kịch bản E2E + Bằng chứng ảnh/video |
| **Giải quyết bế tắc bug** | Tiếp tục thử sửa code theo vòng lặp | Kích hoạt `Attack the Premise` kiểm tra lại giả định |
| **Quản trị quy tắc** | Nhồi nhét vào System Prompt khổng lồ | Chia nhỏ thành các Skills nạp theo ngữ cảnh |

---

## 4. Bài Học Hành Động

1. **Ứng Dụng Nguyên Lý `no-comments`:** Thử nghiệm dọn dẹp các đoạn comment rác trong codebase để quan sát khả năng hiểu code chính xác hơn của AI agent.
2. **Xây Dựng Verification Skills Thay Vì Chỉ Viết Test:** Đầu tư thời gian hướng dẫn agent cách khởi chạy app, kiểm tra màn hình và xuất trình bằng chứng thực tế trước khi bấm merge.
3. **Phá Bỏ Vòng Lặp Bế Tắc Bằng Cách Đổi Góc Nhìn:** Khi agent bị mắc kẹt quá 3 lượt sửa lỗi thất bại, hãy yêu cầu agent "Attack the Premise" để tìm kiếm nguyên nhân gốc rễ.
