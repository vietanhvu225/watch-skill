# Pstack Is Agent Overkill. Use It Anyway! | Rob Shocks (Phân tích Kiến trúc Pstack của Lauren Tan)

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=lUhXa8GiXns)
- **Watch Skill ID:** `27f9503c737f09e8`
- **Category:** #ai-agents
- **Date Processed:** 2026-09-24
- **Duration:** 12:57
- **Speaker:** Rob Shocks
- **Transcript File:** [pstack_agent_overkill_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/pstack_agent_overkill_transcript.txt)

---

## 1. Tổng quan: Pstack ("Potato Stack") là gì?

**Pstack** là bộ plugin và hệ thống kỹ năng mã nguồn mở dành cho Cursor / Claude Code / Codex, được thiết kế bởi **Lauren Tan** (cựu kỹ sư core React team, cựu Principal Engineer tại SpaceX, hiện tại thuộc Cursor / xAI GrokBot team). 

Rob Shocks nhận định: Pstack không đơn thuần là một danh sách prompt, mà là **"bộ não của một Senior Engineer được trích xuất thành một kiến trúc phần mềm hoàn chỉnh"**.

> [!NOTE]
> Mặc dù Pstack tiêu tốn lượng token gấp 2 đến 3 lần so với quy trình code thông thường ("Agent Overkill"), nhưng chất lượng sản phẩm đầu ra, tính kháng lỗi và khả năng tự động hóa vượt trội hoàn toàn so với mô hình vibe coding truyền thống.

---

## 2. Kiến trúc Trung tâm: Potato Mode & 22 Playbooks

Trọng tâm của Pstack là lệnh điều phối `/potato`:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                          PSTACK ARCHITECTURE OVERVIEW                       │
├─────────────────────────────────────────────────────────────────────────────┤
│                                  USER GOAL                                  │
│                                      │                                      │
│                                      ▼                                      │
│                            [ /potato (ROUTER) ]                             │
│                                      │                                      │
│         ┌────────────────────────────┼────────────────────────────┐         │
│         ▼                            ▼                            ▼         │
│   [Playbook Router]           [Execution Mesh]           [Validation Gates] │
│   - figure-it-out             - Arena (Multi-model)      - Verification     │
│   - tdd-loop                  - Swarm (Sub-agents)       - Interrogate      │
│   - first-principles          - Probe / Scaffold         - Feature Map      │
│   - laziness-protocol         - Worktree Isolation       - Evals Rubric     │
│                                                                             │
│                    SUPPORTING COGNITIVE SKILLS                              │
│   - /why (Tra cứu ADRs, Sentry logs, Slack threads, PostHog qua MCP)        │
│   - /recall (Phục hồi ngữ cảnh dự án sau nhiều ngày gián đoạn)              │
│   - /onslaught (Thanh trừng ngôn từ AI sáo rỗng: "delve", "pivotal", "—")  │
│   - /bro (Dịch tóm tắt kỹ thuật sang ngôn ngữ đời thường khi kiệt sức)      │
└─────────────────────────────────────────────────────────────────────────────┘
```

- **Router Thông minh:** Khi người dùng đưa ra mục tiêu, `potato mode` tự động phân tích và kích hoạt 1 trong 22 Playbooks phù hợp nhất (ví dụ: `figure-it-out` để giải quyết vấn đề chưa rõ ràng, hoặc `tdd` để viết test trước).
- **Triết lý "Không tin vào Planning":** Lauren Tan có quan điểm gây bất ngờ: *"The best spec is the code. I don't believe in planning."* Pstack không áp dụng các bước lập kế hoạch dài dòng (như BMAD hay OpenSpec) vì trong thực tế, các giả định ban đầu luôn bị phá vỡ khi chạm vào codebase thực tế (**Design changes forced by reality**). Thay vào đó, Pstack dùng kỹ thuật **Probing** (viết script nhỏ thăm dò tính khả thi) và nhảy thẳng vào thực thi có kiểm chứng.

---

## 3. Arena Mode vs. Swarm Mode: Nghệ thuật Đua mô hình & Song song hóa

Rob Shocks làm rõ sự khác biệt giữa hai chế độ chạy song song mạnh mẽ nhất trong Pstack:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         ARENA MODE vs SWARM MODE                            │
├─────────────────────────────────────────────────────────────────────────────┤
│  ARENA MODE (Đua mô hình trên CÙNG một bài toán):                           │
│  - Bố trí 3-4 frontier models khác nhau cùng giải quyết 1 task:             │
│       Agent A (Claude 3.7)   Agent B (GPT-4o)   Agent C (Grok 4.6)          │
│                 │                   │                   │                   │
│                 └───────────────────┼───────────────────┘                   │
│                                     ▼                                       │
│                            [ARENA EVALUATOR]                                │
│                     So sánh, chắt lọc phần tốt nhất                         │
│                    (Graft best parts, reject the rest)                      │
│                                     ▼                                       │
│                               Final Commit                                  │
│                                                                             │
│  SWARM MODE (Chia để trị trên các nhánh độc lập):                           │
│  - Chia bài toán lớn thành các lát cắt trực giao (orthogonal slices).        │
│  - Mỗi sub-agent làm việc trên một Git Worktree / Feature branch riêng biệt.│
│  - Tổng hợp về một báo cáo duy nhất mà không gây xung đột mã nguồn.        │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Các Skill Độc đáo Đáng chú ý nhất

### 1. Skill `/why` (Kỹ năng tìm hiểu căn nguyên kiến trúc)
Thông thường, agent chỉ đọc lướt git commit hoặc transcript hiện tại. Skill `/why` đưa agent kết nối trực tiếp với các **MCP Servers** của doanh nghiệp:
- Tra cứu bảng phân tích người dùng trong **PostHog**.
- Tra cứu các cuộc thảo luận kiến trúc trên các kênh **Slack**.
- Truy vết log ngoại lệ và lỗi hệ thống trong **Sentry**.
- Đọc các tài liệu **ADR** (Architecture Decision Records) để hiểu lý do sâu xa đằng sau một đoạn code trước khi sửa.

### 2. Skill `/bro` (Giải cứu tải nhận thức cho kỹ sư)
Sau một buổi sáng quản lý 10 agent chạy song song, não bộ của kỹ sư thường bị kiệt sức (cognitive exhaustion). Khi agent trả về một báo cáo dài dòng ngập tràn thuật ngữ phức tạp, gõ `/bro` sẽ yêu cầu agent giải thích lại bằng ngôn ngữ bình dân, súc tích và dễ hiểu nhất.

### 3. Skill `onslaught` (Khử "mùi AI")
Tự động rà soát và loại bỏ các từ ngữ sáo rỗng đặc trưng của LLM: *"pivotal moment"*, *"crucial"*, *"delve"*, *"enduring"* và cả dấu gạch ngang dài (em dash `—`).

---

## 5. Các Nguyên lý Kỹ thuật Cốt lõi của Lauren Tan trong Pstack

1. **Laziness Protocol (Giao thức lười biếng):**
   - Khi tái cấu trúc (refactoring), ưu tiên hàng đầu là **XÓA BỚT CODE**, làm cho hệ thống đơn giản hơn thay vì đắp thêm code mới.
   - Luôn hướng tới sự thay đổi nhỏ nhất (smallest diff) để hoàn thành nhiệm vụ.
2. **Redesign from First Principles (Thiết kế lại từ nguyên lý ban đầu):**
   - Khi thêm tính năng mới, hãy tự hỏi: *"Nếu tính năng này được thiết kế ngay từ ngày đầu tiên (Day 1), kiến trúc và cấu trúc cơ sở dữ liệu sẽ như thế nào?"* Tránh việc chắp vá các cấu trúc tạm bợ làm nát codebase.
3. **Minimizing Reader Load (Giảm tải cho người đọc):**
   - Hạn chế tối đa các tầng trừu tượng (abstractions) không cần thiết. Code phải dễ đọc và dễ hiểu cho cả con người lẫn các agent trong tương lai.
4. **Build a Lever (Xây dựng đòn bẩy):**
   - Nếu bạn thấy mình hoặc agent phải thực hiện một thao tác thủ công từ 2 lần trở lên, hãy lập tức viết một CLI tool hoặc script tự động hóa.
5. **Guard the Context Window (Bảo vệ cửa sổ ngữ cảnh):**
   - Giữ ngữ cảnh của Agent điều phối (Coordinator) luôn tinh gọn. Đẩy các tác vụ nặng sang sub-agents với context window độc lập, sau đó chỉ thu nhận kết quả tóm tắt.

---

## 6. Đánh giá ROI: Khi nào nên dùng Pstack?

| Tiêu chí | Tiếp cận Tiêu chuẩn (Vibe Coding) | Áp dụng Pstack |
| :--- | :--- | :--- |
| **Thời gian hoàn thành** | ~30 phút | ~60 phút (gấp đôi) |
| **Lượng token tiêu thụ** | Thấp / Trung bình | Rất cao (chạy Arena & Evals) |
| **Tỷ lệ ảo giác (Hallucination)** | Thường xuyên sót lỗi ngầm | Bị chặn và sửa đổi nhờ Verification Loop |
| **Độ bền kiến trúc** | Giảm sút nhanh sau vài tuần | Đạt chuẩn sản phẩm bền vững dài hạn |
| **Trường hợp khuyên dùng** | Thử nghiệm UI nhanh, prototype | Codebase production, tính năng quan trọng |
