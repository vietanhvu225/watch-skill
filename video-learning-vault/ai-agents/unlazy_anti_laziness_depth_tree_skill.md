# unlazy: Kỹ Năng Depth Tree & Stop Hook Chống "Lười Biếng" Cho AI Coding Agents

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=c47uqR7XB_c)
- **Watch Skill ID:** `6c10d9d9043f21ab`
- **Category:** #ai-agents
- **Date Processed:** 2026-09-23
- **Duration:** 12:54 (774.3s)
- **Speaker / Channel:** AI LABS (@AILABS-393)
- **Tác giả Skill:** Leon Lin (`Leonxlnx`) — Tác giả nổi tiếng của `taste-skill` (Top #1 GitHub Trending)
- **Kho lưu trữ Skill:** [GitHub: Leonxlnx/unlazy](https://github.com/Leonxlnx/unlazy)

---

## 📌 1. Vấn đề: Căn bệnh "Lười biếng" & Kết thúc non của AI (Model Laziness)

Một trong những vấn đề gây ức chế nhất khi làm việc với các Coding Agents (Claude Code, OpenAI Codex, Cursor):
- **Báo cáo hoàn thành sớm (Premature Completion):** Khi được giao một task phức tạp (ví dụ: *"Xây dựng trang landing page có pricing, checkout và auth"*), agent thường chỉ làm được khoảng 70–80% khối lượng công việc rồi tự tuyên bố *"Đã xong!"*.
- **Code nửa vời (Half-done code):** Để lại các comment `// TODO`, mockup dữ liệu tĩnh thay vì kết nối backend thật, không xử lý responsive mobile, hoặc bỏ qua việc xử lý lỗi (error handling).
- **Thất bại của Prompt ép buộc (Prompt begging):** Các câu prompt như *"Hãy làm thật kỹ", "Đừng bỏ sót", "Kiểm tra kỹ lưỡng"* gần như không có tác dụng lâu dài trên các task lớn.

---

## 💡 2. Triết lý của `unlazy` v2: Ép buộc bằng Kỹ thuật thay vì Cầu xin

`unlazy` được xây dựng dựa trên các nghiên cứu 2025–2026 về hiện tượng thiếu suy nghĩ (underthinking) và kết thúc sớm của mô hình ngôn ngữ lớn:

> **Khẩu hiệu cốt lõi của `unlazy` v2:**  
> *"v1 told the model to work harder. v2 makes half-done structurally visible: acceptance gates live in files, checks run as commands, and an optional hook blocks the agent from declaring victory while gates are unmet.*  
> ***You do not promise you are done. You prove it against a ledger.***"  
> (Bạn không được hứa là đã làm xong. Bạn phải chứng minh điều đó trước một sổ cái nghiệm thu.)

```
[QUY TRÌNH THÔNG THƯỜNG]
Prompt ──► Agent gõ code ──► Agent tự bảo "Xong rồi!" (Thực tế chỉ 75%) ──► Người dùng thất vọng

[QUY TRÌNH UNLAZY V2]
Prompt ──► Depth Tree (Chẻ nhỏ thành cây N tầng)
               │
               ▼
         [GATES.md] (Sổ cái nghiệm thu bắt buộc)
               │
               ▼
         [Run Code & Subagents]
               │
               ▼
         Agent định thoát? ──► [STOP HOOK Chặn lại]
                                     │
                               Có cổng chưa pass?
                                 /          \
                            [CHẶN THOÁT]   [CHO PHÉP THOÁT]
                            (Bắt làm tiếp) (Bằng chứng đầy đủ)
```

---

## 🌳 3. Phương pháp Cốt lõi: Depth Tree (`tree N`)

Phương pháp Depth Tree chia nhỏ một bài toán lớn thành một cây phân cấp có độ sâu $N$. Điểm đặc biệt: **Mỗi nhánh lá (leaf) được cấp toàn bộ ngân sách nỗ lực của một task độc lập**, giúp nhân bội nỗ lực giải quyết vấn đề tương ứng với độ sâu của cây.

### Các cấp độ sâu khuyến nghị:
- **`tree 2–3` (Solo Mode):** Phù hợp cho 1 tính năng cụ thể hoặc một đợt săn lỗi (bug hunt). Thời lượng thực hiện dưới 30 phút. Chỉ cần **1 file `GATES.md`** duy nhất.
- **`tree 4–5` (Orchestrated Mode):** Phù hợp cho việc xây dựng một hệ thống con (subsystem) hoặc module lớn hoàn chỉnh.
- **`tree 6–7` (Full Project Mode):** Phù hợp để kiến tạo toàn bộ dự án từ con số 0, phân rã sâu và thực thi bằng nhiều subagent độc lập.

> **Tự động điều chỉnh độ sâu:** Nếu bạn chọn số `tree` quá cao so với độ phức tạp thực tế, kỹ năng sẽ tự động hạ độ sâu xuống mức hợp lý để tránh lãng phí token.

---

## 📋 4. Cấu trúc Sổ cái Nghiệm thu (`GATES.md`)

Mọi yêu cầu nghiệm thu được cấu trúc hóa trong file `GATES.md` dưới dạng lệnh terminal có thể thực thi và kiểm tra đối chiếu:

```markdown
# Gates: Atelier Assembly v1 (Integration Checklist)

- [x] N1: Every Server Action guards itself against unauthorized calls
  CHECK: node scripts/action-check.mjs
  EXPECT: ACTION GUARDS OK
  EVIDENCE: 59/59 action-guard assertions passed. Output: ACTION GUARDS OK

- [x] N2: The checkout page loads and handles real-time total updates
  CHECK: npm test checkout -- --silent
  EXPECT: 1 passed, 0 failed
  EVIDENCE: Test Suites: 1 passed, Tests: 3 passed, 0 failed

- [x] N3: The landing page passes anti-AI-slop audit
  CHECK: npx ai-slop-detector audit components/marketing/
  EXPECT: Verdict: CLEAN - 0 high, 0 medium
  EVIDENCE: Ran over 7 files. Verdict: CLEAN - 0 high, 0 medium
```

### Cơ chế hoạt động của Stop Hook (Claude Code):
- Khi Claude Code cố gắng kết thúc phiên làm việc (hoặc gọi lệnh stop), **Stop Hook** sẽ chặn đứng tiến trình.
- Hook đọc file `GATES.md` và chạy các lệnh `CHECK`.
- Nếu có bất kỳ cổng nào chưa được đánh dấu `[x]` hoặc lệnh kiểm tra thất bại, hook sẽ gửi phản hồi từ chối kèm lý do chính xác, buộc Claude phải quay lại viết tiếp code để khắc phục.

---

## ⚡ 5. Cải tiến Đột phá của AI LABS: Kích hoạt Subagents Chạy Song song

Đây là đóng góp thực chiến giá trị nhất từ kênh AI LABS khi thử nghiệm `unlazy`:

### 1. Lỗ hổng của bản gốc `unlazy`:
- Ở chế độ Orchestrated, bản gốc phân rã công việc cho các subagent nhưng lại thực thi **tuần tự (sequential)**: Giao việc cho Agent 1 ➔ Chờ xong ➔ Giao cho Agent 2.
- **Hậu quả:** Phiên chạy thử kéo dài **3 đến 4 tiếng đồng hồ liên tục** nhưng chỉ tạo được mỗi trang Login do lãng phí tài nguyên chờ đợi.

### 2. Bản vá thực chiến (Parallel Subagents with Disjoint Files):
AI LABS đã chỉnh sửa chỉ dẫn trong `SKILL.md` để khai thác triệt để khả năng chạy đa tác nhân song song của Claude Code và Codex:

1. **Nguyên tắc Tệp tin rời rạc (Disjoint Files):**
   - Trong file `PLAN.md`, mỗi task của subagent được phân bổ một tập hợp các file hoàn toàn tách biệt.
   - Ví dụ: Subagent A chỉ sửa `components/auth/*`, Subagent B phụ trách `components/pricing/*`, Subagent C lo `lib/payments/*`.
   - **Tác dụng:** Loại bỏ hoàn toàn nguy cơ các agent ghi đè hoặc xung đột code (race conditions / merge conflicts).
2. **Kích hoạt đồng thời 10 Subagents:**
   - Thay vì chạy từng con một, hệ thống dispatch toàn bộ các nhánh lá độc lập cùng một lúc.
   - **Kết quả:** Xây dựng xong toàn bộ ứng dụng demo đầy đủ chức năng chỉ trong **dưới 2 giờ** (thay vì 4 giờ tắc nghẽn).

---

## 🧠 6. Phối hợp cùng Kỹ Năng Định Tuyến Mô Hình (Model Router)

Khi chạy hệ thống ở quy mô 10 subagents cùng lúc, chi phí token và giới hạn rate limit có thể tăng vọt. Kênh đề xuất phối hợp `unlazy` với kỹ năng **Model Router**:
- **Nhiệm vụ cơ học / lá đơn giản:** Định tuyến sang model nhẹ, chi phí thấp (ví dụ: Claude 3.5 Haiku) để làm các việc lặp lại, dựng layout tĩnh, viết test cơ bản.
- **Nhiệm vụ kiến trúc / lá phức tạp:** Định tuyến sang model mạnh (Claude 3.5 Sonnet / Opus / GPT-5) để xử lý thiết kế logic nghiệp vụ, bảo mật và tích hợp payment.

---

## 🚀 7. Hướng dẫn Cài đặt & Sử dụng

### Cài đặt qua Skills CLI:
```bash
# Cài đặt tự động cho Claude Code, Codex, Cursor
npx skills add Leonxlnx/unlazy

# Cài đặt global cho tất cả agents tìm thấy
npx skills add Leonxlnx/unlazy -g --all
```

Sau khi cài đặt, bạn sẽ thấy thư mục `.agents/skills/unlazy` (và symlink trong `.claude/skills/unlazy`).

### Cú pháp gọi lệnh:
```text
/unlazy tree 5 Xây dựng toàn bộ ứng dụng Event Management gồm landing page, authentication, dashboard quản lý vé, tích hợp cổng thanh toán Stripe và gửi email thông báo.
```

---

## 📊 8. So Sánh: Ralph Loop vs. /goal vs. unlazy

| Tiêu chí | Ralph Loop (`/ralph-loop`) | Claude `/goal` | unlazy v2 |
| :--- | :--- | :--- | :--- |
| **Bản chất** | Vòng lặp `while true` đơn giản trên shell | Session-scoped Stop Hook tích hợp sẵn của Claude | Cây phân cấp độ sâu (Depth Tree) + Sổ cái `GATES.md` |
| **Đánh giá hoàn thành** | Model tự phán đoán theo chuỗi text | Gọi model nhỏ (Haiku) hỏi Yes/No | Chạy lệnh test thực tế (`CHECK` commands) kiểm tra output |
| **Quy mô xử lý** | Task đơn, lặp đi lặp lại một prompt | Task trong 1 session hội thoại | Task lớn, subsystem, dự án từ 0 với 10+ subagents song song |
| **Khả năng song song** | Tuyến tính | Tuyến tính | Đa agent song song trên các tệp tin rời rạc |

---

## 🔍 Từ khóa tìm kiếm liên quan
`unlazy`, `Leonxlnx`, `taste-skill`, `Depth Tree`, `Model Laziness`, `AI Premature Completion`, `GATES.md`, `Stop Hook Claude Code`, `Parallel Subagents`, `Disjoint Files`, `Model Router`.
