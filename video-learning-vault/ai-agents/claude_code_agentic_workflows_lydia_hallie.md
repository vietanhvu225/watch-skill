# Claude Code & Agentic Workflows Masterclass — Lydia Hallie (Anthropic)

- **Video URL:** [Facebook Reel](https://www.facebook.com/reel/1172205942011624)
- **Watch Skill ID:** `ec8ec278c38667ce`
- **Diễn giả:** Lydia Hallie — Member of Technical Staff tại Anthropic (Claude Code Team)
- **Category:** #ai-agents, #claude-code, #agentic-workflows, #subagents, #engineering-practices
- **Date Processed:** 2026-09-28
- **Duration:** 01:02:30 (3750 giây)
- **Transcript File:** [claude_code_agentic_workflows_lydia_hallie_transcript.txt](file:///f:/source/watch-skill/video-learning-vault/ai-agents/claude_code_agentic_workflows_lydia_hallie_transcript.txt)

---

## 1. Bối cảnh & Giới thiệu Tác giả: Lydia Hallie (Anthropic Claude Code Team)

Lydia Hallie là gương mặt quen thuộc trong giới công nghệ thế giới với các chuỗi bài giải thích trực quan (Visual Guides) nổi tiếng về JavaScript/TypeScript internals. Trước khi gia nhập Anthropic, cô từng làm việc tại **Vercel** và đội ngũ phát triển **Bun** (JavaScript runtime hiệu năng cực cao). Sau khi Bun được tích hợp sâu vào hệ sinh thái của Anthropic, Lydia chính thức trở thành kỹ sư thuộc **Claude Code Team**.

Trong buổi Masterclass kéo dài hơn 1 giờ này, Lydia không chỉ chia sẻ các mẹo sử dụng cơ bản mà đi thẳng vào cấu trúc nội tại (**Agentic Glue**), kiến trúc lắp ráp ngữ cảnh (Prompt Assembly) và cách Anthropic thiết kế hệ thống **Loops, Graphs, Subagents, Skills và Hooks** để biến LLM thành một lập trình viên cộng tác thực thụ.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                          LỘ TRÌNH MASTERCLASS CLAUDE CODE (LYDIA HALLIE)                    │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  ⏱️ 00:00 - The Agentic Glue & Prompt Assembly: Cơ chế lắp ráp ngữ cảnh dưới hậu trường.    │
│  ⏱️ 00:27 - CLAUDE.md & Plan Mode: Quản lý tri thức dự án & phỏng vấn ngược người dùng.   │
│  ⏱️ 11:24 - Skills & Hooks: Đóng gói quy trình chuẩn & bắt sự kiện vòng đời agentic loop. │
│  ⏱️ 37:02 - Xây dựng Subagents: Phân luồng context sạch, chạy song song chống ô nhiễm bộ nhớ.│
│  ⏱️ 52:47 - Agent Teams vs. Self-Improving Loops: Đội ngũ agent, task list chung & Evals.   │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Cơ chế Lắp Ráp Prompt & "Chất Keo" Hệ Thống (Prompt Assembly & Agentic Glue)

Một trong những sai lầm phổ biến nhất của kỹ sư là coi Claude Code chỉ như một chiếc "Chatbot trong Terminal". Thực chất, mỗi lượt tương tác (turn) là kết quả của một bộ máy ghép nối ngữ cảnh phức tạp:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                         CẤU TRÚC ASSEMBLED PROMPT GỬI ĐẾN MODEL                             │
├─────────────────────────────────────────────────────────────────────────────────────────────┤
│  1. System Prompt (~21,000 tokens):                                                         │
│     └── Cố định do Anthropic quản lý (Định nghĩa tính cách, an toàn, giao thức tool call).   │
│                                                                                             │
│  2. CLAUDE.md Context:                                                                      │
│     └── Quy ước dự án, cấu trúc repo, kiến trúc luồng dữ liệu, lệnh build/test.            │
│                                                                                             │
│  3. Skills Metadata Registry:                                                               │
│     └── CHỈ GỬI Name + Description của các skills có sẵn (TIẾT KIỆM TỐI ĐA TOKEN).          │
│         Toàn bộ nội dung chi tiết của skill chỉ được nạp khi skill đó được kích hoạt!       │
│                                                                                             │
│  4. Messages Array (Lịch sử hội thoại):                                                     │
│     └── [User 1, Assistant 1, User 2, Assistant 2...] tích lũy qua từng lượt.              │
│                                                                                             │
│  5. Tool Results & Files Context:                                                           │
│     └── Kết quả trả về từ lệnh terminal, nội dung file đọc được, hình ảnh đính kèm.        │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

### Giám sát Ô nhiễm Ngữ cảnh với Lệnh `/context`:
- Lệnh `/context` hiển thị tỷ lệ phần trăm bộ nhớ đang dùng trong cửa sổ 1 triệu tokens.
- Kỹ sư cần chủ động theo dõi xem những tài liệu hoặc file rác nào đang vô tình làm phình to context window, từ đó làm suy giảm khả năng tập trung (attention drift) của model.

---

## 3. Chiến Lược Quản Lý `CLAUDE.md` & Phân Cấp Thư Mục

Lydia chia sẻ quy tắc nằm lòng khi xây dựng file cấu hình dự án:

> [!IMPORTANT]
> **Quy tắc Vàng về `CLAUDE.md`:**  
> *"Nếu bạn thấy mình phải lặp lại một chỉ dẫn nào đó nhiều lần trong các lượt prompt, đó là tín hiệu bắt buộc phải đưa chỉ dẫn đó vào file `CLAUDE.md`."*

### Khởi tạo Tự động bằng `/init`:
Khi gõ `/init` trong một dự án có sẵn, Claude Code sẽ tự động duyệt codebase và sinh ra khung `CLAUDE.md` gồm:
- **Project Overview & Tech Stack:** Các công nghệ đang dùng (React, Vite, D&D Kit, Tailwind, Bun...).
- **Build & Dev Commands:** Lệnh chạy dev, test, lint, migration.
- **Architecture & Data Flow:** Luồng dữ liệu qua state management (Redux, Zustand, React Query).
- **Lợi ích:** Giúp model định vị ngay các file cần thiết mà không phải tốn hàng chục lượt gọi công cụ (`tool calls`) vô ích để thăm dò thư mục.

### Cây Phân Cấp Ghi Đè (Hierarchy):
Claude Code hỗ trợ phân tầng cấu hình theo thứ tự từ rộng đến hẹp:
```
User Root (~/.claude/CLAUDE.md)
  └── Repo Root (my-repo/CLAUDE.md)
        └── Monorepo Sub-package (my-repo/packages/web/CLAUDE.md)
```
- Khi khởi động phiên làm việc tại thư mục con, Claude Code sẽ ưu tiên `CLAUDE.md` tại thư mục đó, sau đó kế thừa ngược lên root.

### Triết lý "Less is More" với Models Thế Hệ Mới:
- Với các model cũ (như Opus 4.5/4.6), kỹ sư thường phải viết các mệnh lệnh gay gắt, nhiều dấu chấm than (`DO NOT DO THIS!`).
- Với các model thế hệ mới (Opus 4.7+), khả năng hiểu ngữ cảnh và ý định (intent) đã vượt trội. Lydia khuyên: **Hãy định kỳ xóa bớt các dòng thừa trong `CLAUDE.md`** để kiểm tra xem model có còn phạm lỗi không. Đừng để file này trở thành một bãi rác token.

---

## 4. Plan Mode & Kỹ Thuật Phỏng Vấn Ngược (`ask_user_question`)

### Bước chuyển Vai trò: Từ Coder thành Reviewer & PM:
Khi làm việc với Claude Code, kỹ sư không còn ngồi gõ từng ký tự cú pháp, mà trở thành **Product Manager & Kiến trúc sư**. Trước khi bắt tay vào code bất kỳ tính năng nào, luôn bắt đầu bằng **Plan Mode**.

### Khóa Thực Thi trong Plan Mode:
- Bật bằng phím tắt `Shift + Tab` trong CLI hoặc prompt rõ: *"Don't code anything yet, plan first"*.
- **Cơ chế an toàn:** Trong Plan Mode, model bị khóa hoàn toàn quyền ghi file (`Write`, `Edit`) và thực thi shell; nó chỉ được phép đọc codebase, phân tích sự phụ thuộc và xuất ra bản kế hoạch (`Implementation Plan`).

### Tuyệt chiêu Phỏng Vấn Ngược với `ask_user_question`:
Một kỹ thuật đỉnh cao được chính kỹ sư Anthropic sử dụng thường xuyên là ép Claude dùng tool `ask_user_question` để "phỏng vấn" lại con người:

```markdown
Prompt mẫu của Lydia:
"I want to create a feature where issues have multiple owners and priorities. 
Use the ask_user_question tool to interview me thoroughly. 
Think about edge cases I may not have thought of yet."
```

- Khi nhận prompt này, Claude Code sẽ xuất hiện các modal trắc nghiệm tương tác trực tiếp trên CLI/UI.
- Nó sẽ hỏi kỹ sư về: Vị trí đặt UI, hành vi khi người dùng bấm Escape, cách fallback dữ liệu, phân quyền truy cập.
- Kỹ thuật này giúp giải quyết toàn bộ các lỗ hổng thiết kế **trước khi viết một dòng code nào**.

---

## 5. Kỹ Thuật Thiết Kế Skills Chuyên Sâu

Skill không đơn thuần là một đoạn text mẫu. Trong kiến trúc của Anthropic, Skill là một **thủ tục có cấu trúc và có thể cấu hình quyền hạn nghiêm ngặt**.

### Cấu trúc Thư mục Chuẩn:
```
.claude/
  └── skills/
        └── deploy/
              └── SKILL.md
```

### Các Trường Frontmatter Cao Cấp trong `SKILL.md`:

```yaml
---
name: deploy
description: Deploys the codebase to staging or production environment.
when_to_use: Use this skill specifically when the user mentions deploying, releasing, or shipping code.
model: sonnet
disable_model_invocation: true  # Biến skill thành Slash Command thuần túy, cấm model tự gọi ngầm
user_invocable: true            # Cho phép hiển thị trên CLI cho người dùng gõ /deploy
arguments: "[environment]"      # Khai báo tham số truyền vào
argument_hint: "staging | production"
allowed_tools:                  # Giới hạn sandbox chỉ cho phép một số tool nhất định
  - Bash(npm run build)
  - Bash(gh release create)
---

# Deploy Procedure
Run the build verification, check git status, then deploy to `$ARGUMENTS`.
```

### Dynamic Context Injection (Chạy lệnh Shell trước khi gửi Prompt):
Một tính năng cực mạnh ít người biết là cú pháp thực thi lệnh shell động:
```markdown
## Current Pull Requests
!`gh pr list --limit 5`

Dựa trên danh sách PRs ở trên, hãy thực hiện tác vụ sau...
```
Cú pháp này sẽ thực thi lệnh shell `gh pr list` trên máy local và chèn kết quả trực tiếp vào nội dung skill trước khi payload được gửi lên model!

### `skill-creator` & Tự Động Chạy Evals:
- Claude Code tích hợp sẵn công cụ `skill-creator`.
- Khi tạo skill mới, nó không chỉ sinh markdown mà còn tự động dựng kịch bản **Ablation Test (Đo lường hiệu quả tương đối)**:
  - Chạy thử nghiệm tác vụ khi **CÓ** skill vs. khi **KHÔNG CÓ** skill.
  - Đo lường số lượng tokens tiêu thụ, thời gian hoàn thành và tỷ lệ chính xác.
  - Xuất báo cáo trực quan dưới dạng file HTML.

---

## 6. Vòng Đời Agentic Hooks: Kiểm Soát Tuyệt Đối Hành Vi Hệ Thống

Nếu Skills đại diện cho quy trình công việc, thì **Hooks** chính là "lực lượng cảnh sát" giám sát toàn bộ chu trình sống của Agent, tương tự như Git Hooks.

```
                           ┌────────────────────────┐
                           │      Session Start     │
                           └───────────┬────────────┘
                                       │
                           ┌───────────▼────────────┐
                           │   User Prompt Submit   │
                           └───────────┬────────────┘
                                       │
                           ┌───────────▼────────────┐
                   ┌───────┤     Pre-Tool Use       │◄────── Đánh chặn lệnh nguy hiểm
                   │       └───────────┬────────────┘
                   │                   │
                   │       ┌───────────▼────────────┐
                   │       │   Tool Execution       │
                   │       └───────────┬────────────┘
                   │                   │
                   │       ┌───────────▼────────────┐
                   └──────►│     Post-Tool Use      │◄────── Chạy Type-Check / Linter tự động
                           └───────────┬────────────┘
                                       │
                           ┌───────────▼────────────┐
                           │     Subagent Stop      │
                           └────────────────────────┘
```

### Các Hooks Trọng Yếu:
1. `pre_tool_use`: Chặn trước khi một lệnh nhạy cảm được thực thi (ví dụ: cấm `rm -rf`, kiểm tra quyền hạn).
2. `post_tool_use`: Chạy ngay sau khi tool ghi nhận thay đổi (ví dụ: tự động chạy `tsc --noEmit` hoặc `biome check --apply` mỗi khi có file bị chỉnh sửa).
3. `session_start`: Nạp cấu hình môi trường hoặc in thông báo kiểm tra phiên.

### Cách cấu hình Hook trong `settings.json`:
```json
{
  "hooks": {
    "post_tool_use": [
      {
        "matcher": "Edit|Write",
        "command": "npm run type-check"
      }
    ]
  }
}
```

---

## 7. Kiến Trúc Subagents vs. Agent Teams (Teammates)

Đây là phần mang lại giá trị kiến trúc sâu sắc nhất trong bài giảng của Lydia:

```
┌──────────────────────────────────────┐     ┌──────────────────────────────────────────────┐
│        MÔ HÌNH SUBAGENTS             │     │           MÔ HÌNH AGENT TEAMS                │
├──────────────────────────────────────┤     ├──────────────────────────────────────────────┤
│  Main Agent                          │     │  Team Lead (Main Agent đóng vai trò PM)      │
│     │ (Spawn ngầm song song)         │     │     │                                        │
│     ├── Subagent 1 (Forked Context)  │     │     ├── Teammate A (UI Dev) ◄──┐             │
│     └── Subagent 2 (Forked Context)  │     │     ├── Teammate B (Backend) ──┼── Giao tiếp │
│     │                                │     │     └── Teammate C (QA)     ───┘   chéo nhau │
│     ▼ (Chỉ trả kết quả cuối cùng)    │     │     │                                        │
│  Main Messages Array SẠCH SẼ 100%    │     │  Dùng chung Shared Task List, TỐN TOKEN KHỦNG│
└──────────────────────────────────────┘     └──────────────────────────────────────────────┘
```

### Bảng So Sánh Chi Tiết:

| Đặc tính | Subagents (Tiểu ban độc lập) | Agent Teams (Đội ngũ Teammates) |
|---|---|---|
| **Cơ chế Ngữ cảnh** | **Forked Context:** Tạo môi trường sạch, có system prompt riêng, công cụ riêng. | **Dynamic Panels:** Các agent chạy song song trên nhiều panels, nhìn thấy task list chung. |
| **Giao tiếp** | Đơn hướng: Nhận lệnh từ Main Agent $\to$ làm việc $\to$ chỉ trả lại bản tóm tắt kết quả. | Đa hướng: Các teammates có thể nhắn tin, phản biện và phân công lại công việc cho nhau. |
| **Mức độ tiêu thụ Token** | **Thấp - Trung bình:** Vì context được cách ly, kết quả rác giữa chừng không bị dồn vào luồng chính. | **Cực cao:** Mỗi message trao đổi qua lại giữa các teammates làm phình to token theo cấp số nhân. |
| **Xung đột File** | Dễ gặp merge conflict nếu nhiều subagents cùng ghi vào một file. | Có sự điều phối của Team Lead để phân chia vùng file tránh ghi đè. |
| **Khuyến nghị từ Anthropic** | **Nên dùng 90% thời gian:** Cho các tác vụ như Code Review, Explore codebase, Search docs. | **Thận trọng:** Chỉ dùng cho các bài toán cực lớn; dễ bị bẫy kiểm tra vô tận (Ouroboros loop). |

---

## 8. Sơ Đồ Toàn Cảnh Quy Trình Làm Việc Chuẩn Claude Code

```
                        ┌───────────────────────────────┐
                        │   KHỞI TẠO DỰ ÁN (/init)      │
                        │   Sinh CLAUDE.md chuẩn hóa    │
                        └───────────────┬───────────────┘
                                        │
                        ┌───────────────▼───────────────┐
                        │          PLAN MODE            │
                        │ (Shift + Tab / Khóa ghi code) │
                        └───────────────┬───────────────┘
                                        │
                        ┌───────────────▼───────────────┐
                        │      PHỎNG VẤN NGƯỢC          │
                        │   Dùng `ask_user_question`    │
                        │   Làm rõ mọi edge cases & UI  │
                        └───────────────┬───────────────┘
                                        │
                        ┌───────────────▼───────────────┐
                        │      THỰC THI QUA SKILLS      │
                        │   Dynamic Context Injection   │
                        │   Chạy với Allowed Tools      │
                        └───────────────┬───────────────┘
                                        │
                        ┌───────────────▼───────────────┐
                        │      SUBAGENT ISOLATION       │
                        │   Forked context cho Review   │
                        │   Giữ main messages tinh gọn  │
                        └───────────────┬───────────────┘
                                        │
                        ┌───────────────▼───────────────┐
                        │      HOOKS ENFORCEMENT        │
                        │   Post-tool use type check    │
                        │   Tự động sửa lỗi (Self-loop) │
                        └───────────────────────────────┘
```

---

## 9. Các Lệnh Cần Biết & Mẹo Vặt Giá Trị Cao (Power Tools)

- `/init`: Tự động khảo sát codebase và tạo `CLAUDE.md`.
- `/context`: Kiểm tra dung lượng bộ nhớ token đang dùng và phân tích thành phần.
- `/insights`: Báo cáo phân tích hành vi lập trình của bạn với Claude Code (được ví như bản trắc nghiệm tính cách Myers-Briggs cho lập trình viên).
- `/powerup`: Khóa huấn luyện tương tác giúp người dùng mới nắm vững các phím tắt và mẹo dùng `@` để gắn file nhanh vào prompt.
- Gắn file bằng `@`: Thay vì bảo Claude "hãy đọc file utils.ts", gõ `@src/utils.ts` ngay trong prompt để Claude Code tự động nạp nội dung file mà không tốn thêm 1 lượt gọi tool.
- Sử dụng mô hình hợp lý:
  - **Claude Haiku:** Dành cho Subagent Explore (tìm kiếm file, grep mã nguồn) và QA Reviewer đơn giản $\to$ Siêu nhanh và tiết kiệm chi phí.
  - **Claude Sonnet:** Dành cho đa số công việc lập trình tính năng và phân tích logic.
  - **Claude Opus:** Dành cho vai trò Kiến trúc sư trưởng trong Plan Mode và giải quyết các bài toán hóc búa cần suy luận nhiều bước.

---

## 10. Tổng Kết & Triết Lý Phát Triển Thời AI

1. **Xây dựng trực giác cá nhân:** Không có một công thức duy nhất đúng cho mọi dự án. Claude Code là một cộng sự linh hoạt; hãy trò chuyện và hiệu chỉnh nó như một người đồng nghiệp thông minh.
2. **Kỹ nghệ Kỹ năng (Skill Engineering) thay thế Kỹ nghệ Lời nhắc (Prompt Engineering):** Đừng viết các đoạn prompt dài dằng dặc mỗi ngày. Hãy codify quy trình của bạn thành các `SKILL.md` tái sử dụng được và kiểm chứng bằng Evals.
3. **Giữ cho Context luôn sạch:** Lạm dụng context sẽ dẫn đến sự đãng trí và ảo giác của mô hình. Tách biệt các tác vụ phụ vào Subagents để luồng tư duy chính luôn sắc bén.
4. **Tự động hóa phòng thủ với Hooks:** Đừng tin vào lời hứa của AI rằng code không có lỗi. Hãy dùng `post_tool_use` hook để bắt compiler và test runner kiểm chứng lại từng thay đổi ngay lập tức.
