# agit: Hệ thống Version Control Cho Hội thoại AI Agent (Claude Code & Codex)

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=CHHEjuNBxoQ)
- **Watch Skill ID:** `2dfe3efe2675c615`
- **Category:** #ai-agents
- **Date Processed:** 2026-09-23
- **Duration:** 12:24 (744.6s)
- **Speaker / Channel:** AI LABS (@AILABS-393)
- **Công cụ trọng tâm:** `agit` (AgentGit) — Open Source Version Control for Agent Sessions ([GitHub: Einsia/agent-git](https://github.com/Einsia/agent-git) | [Hub: agent-git.com](https://agent-git.com))

---

## 📌 1. Bối cảnh: Tại sao Git truyền thống là chưa đủ cho AI Coding?

Khi lập trình viên làm việc với các Coding Agents (Claude Code, OpenAI Codex, Cursor, Devin), một vấn đề lớn phát sinh:
- **Git chỉ lưu mã nguồn (Code diff):** Git chỉ theo dõi các tệp tin trong repository bị thay đổi như thế nào (`git commit`, `git push`).
- **Ngữ cảnh hội thoại (Context / Talk) bị thất lạc:** Toàn bộ quá trình ra quyết định, reasoning, các câu prompt tinh chỉnh, lịch sử công cụ đã gọi (ToolUse) và các thử nghiệm sai lầm đều nằm trong terminal session của một cá nhân.
- **Handoff giữa người với người (Human-to-Human Handoff):** Khi đồng nghiệp muốn tiếp quản một task do agent làm dở, họ chỉ thấy code diff trong Git nhưng không biết agent đã được dặn dò gì, đã thử những cách nào và tại sao lại chọn kiến trúc đó.
- **Bế tắc khi Agent đi chệch hướng (Dead-end sessions):** Khi Claude Code sa lầy vào một hướng giải quyết sai sau 10 turns, người dùng thường phải xóa session làm lại từ đầu (mất hết context đúng trước đó) vì không thể "checkout" hay "revert" từng turn hội thoại.

> **Khẩu hiệu cốt lõi của AgentGit:**  
> *"Code is cheap, show me your talk. Version control for agent sessions."*  
> (Code giờ rất rẻ, hãy cho tôi xem lịch sử hội thoại của bạn.)

---

## 🏗️ 2. Kiến trúc & Khái niệm Cốt lõi của `agit`

`agit` là công cụ mã nguồn mở (viết bằng Rust với bản phân phối binary prebuilt qua npm) hoạt động song song với Git thông thường:

```
                  ┌──────────────────────────────────────────────┐
                  │          PROJECT REPOSITORY                  │
                  │                                              │
                  │   [Git] ────────► Tracks CODE Changes        │
                  │   (.git)          (commits, branches, PRs)   │
                  │                                              │
                  │   [agit] ───────► Tracks CONTEXT / TURNS     │
                  │   (.agit)         (prompts, tool calls, logs)│
                  └──────┬───────────────────────┬───────────────┘
                         │                       │
                         ▼                       ▼
                  GitHub / GitLab        AgentGit Hub
                  (Code Remote)          (agent-git.com)
```

### Các thành phần chính trong `agit`:
1. **Turn (Lượt trao đổi):** Đơn vị cơ sở của `agit`. Một turn gồm 1 câu prompt của người dùng + toàn bộ quá trình suy nghĩ (thinking), gọi công cụ (tool use events) và câu trả lời hoàn tất của agent.
2. **Session / Branch:** Một chuỗi các turns liên tiếp tương tự như một branch trong Git. Bạn có thể rẽ nhánh hội thoại (`agit branch`, `agit new -b <name>`).
3. **Milestone Commit:** Đóng băng trạng thái hội thoại ở một mốc quan trọng (`agit commit --milestone "landing + auth pages"`), có thể kèm cờ `--code` để liên kết đồng bộ với commit của Git.
4. **Hub (`agent-git.com`):** Nơi lưu trữ, chia sẻ và duyệt trực quan cây hội thoại, lượt tương tác của đồng nghiệp hoặc cộng đồng.

---

## 🛠️ 3. Cài đặt & Khởi tạo (Quickstart)

### Cài đặt qua npm / pnpm:
```bash
# Cài đặt global (tự động tải prebuilt binary tương ứng với OS/CPU)
npm install -g @einsia/agent-git

# Hoặc khởi tạo nhanh
npx -y create-agit

# Kiểm tra phiên bản
agit --version
```

### Đăng nhập & Khởi tạo Repo cho Agent:
```bash
# 1. Đăng nhập vào AgentGit Hub (hỗ trợ Browser OAuth hoặc Device Code cho SSH)
agit login

# 2. Khởi tạo kho lưu trữ cho project hiện tại
agit init
```
Khi chạy `agit init`, CLI sẽ hiển thị giao diện tương tác:
- Chọn Owner & Repository name trên `agent-git.com`.
- Liên kết thư mục hiện tại.
- Import các file chỉ dẫn (`CLAUDE.md`, `AGENTS.md`) và rules vào bộ nhớ agent.
- Bật tính năng Auto-push để tự động đồng bộ hóa phiên làm việc lên Hub.

---

## ⚡ 4. Các Tính Năng Đột Phá Của `agit`

### 1. Adopt / Import Session đang chạy (`agit import`)
Bạn không cần khởi động agent thông qua `agit`. Bạn có thể mở Claude Code trực tiếp (`claude`), bắt đầu làm việc. Khi thấy phiên làm việc có giá trị, chỉ cần gõ:
```bash
agit import
```
`agit` sẽ tự động nhận diện runtime của Claude Code, thu nạp toàn bộ lịch sử các turns vừa chạy vào một branch mới và đồng bộ lên Hub.

### 2. Xem lịch sử hội thoại chi tiết (`agit log`)
```bash
agit log <owner/repo>@<branch>
```
Lệnh này liệt kê từng turn kèm ID băm (hash), số lượng events, số lần ToolUse và câu prompt:
```text
turns (12)
# 10 fb15009ef turn  9 events  1 ToolUse  "how do i know the next step"
#  9 136c7c2f4 turn 15 events  1 ToolUse  "how to revert using agit"
#  8 f4962ed6e turn 16 events  0 ToolUse  "in simplest terms what is agit"
#  7 4d2051c88 turn 28 events  1 ToolUse  "can you cherry pick message 4"
#  1 fad5bdc2f turn 243 events 24 ToolUse "i want you to build landing page..."
```

### 3. Cherry-Pick Ngữ Cảnh giữa các Sessions
Nếu bạn đang ở một session mới và muốn lấy lại một turn giải thích kiến trúc hoặc giải pháp quan trọng từ session cũ của đồng nghiệp:
- Bạn có thể chỉ định lấy đúng nội dung prompt + response của turn đó đưa thẳng vào context hiện tại của Claude Code mà không bị rác context bởi toàn bộ lịch sử dài dòng.
- *Lưu ý:* Cherry-pick trong `agit` chỉ copy ngữ cảnh đàm thoại, không tự động áp dụng các thay đổi file/code trong quá khứ.

### 4. Cross-Agent Handoff: Bàn giao trực tiếp giữa Claude Code và OpenAI Codex
Đây là tính năng độc nhất vô nhị mà các tool khác không có:
- Thông thường, khi chuyển từ Claude Code sang Codex, bạn phải yêu cầu Claude viết file tóm tắt (handoff markdown), rồi mở Codex và bảo nó đọc file đó.
- Với `agit`, bạn dùng lệnh `fork`:
  ```bash
  agit fork @ --branch codex -as codex --resume
  ```
- `agit` sẽ:
  1. Tạo một branch mới từ hội thoại hiện tại.
  2. **Tự động chuyển đổi định dạng hội thoại (Format translation)** từ schema của Claude Code sang schema mà Codex đọc hiểu được.
  3. Mở session trực tiếp trong Codex để bạn tiếp tục làm việc mà không mất ngữ cảnh!

### 5. Revert Turn Hội Thoại (`agit revert`)
Khi agent bắt đầu ảo giác (hallucinate) hoặc đi vào ngõ cụt:
```bash
agit revert <owner/repo>@<branch>#turn_number
```
Nguyên tắc an toàn của `agit`: **Không bao giờ rebase hay force-push làm mất lịch sử hội thoại**, mà tạo bản ghi revert có kiểm soát.

---

## 🔒 5. Bảo mật & Bộ lọc Bí mật (Secret & Credential Guard)

Một rủi ro nghiêm trọng khi lưu và push lịch sử hội thoại của AI Agent lên đám mây là **rò rỉ API Keys và Passwords**:
- Khi agent đọc file `.env`, file cấu hình hoặc output terminal, các secrets (OpenAI key, AWS keys, Database password) lập tức lọt vào prompt context.
- Nếu push conversation lên remote, toàn bộ team hoặc công chúng có thể nhìn thấy secrets.

### Cơ chế bảo vệ đa tầng của `agit`:
1. **Auto-masking bằng Pattern Matching:**
   - `agit` tự động quét các định dạng API key chuẩn (ví dụ: bắt đầu bằng `sk-demo-`, độ dài cố định, token format).
   - Tự động thay thế giá trị thực bằng **Placeholders** an toàn khi lưu trữ và trước khi push.
2. **Kỹ năng bảo vệ Password (`agit-secret-guard` Skill):**
   - Các mật khẩu thông thường không có pattern cố định như API keys nên regex dễ bỏ sót.
   - Nhóm AI LABS phát triển skill riêng: Agent tự quét và đăng ký danh sách biến môi trường/secrets hiện có trong repo với `agit daemon` trước khi bắt đầu session.
   - Bất kỳ mật khẩu mới nào người dùng gõ vào prompt đều được đăng ký tức thì để bị che (masked) trước khi ghi vào log.

---

## 🤖 6. Tích hợp `CLAUDE.md` chuẩn cho `agit`

Để Claude Code tự động tuân thủ quy trình của `agit`, thêm đoạn cấu hình sau vào `CLAUDE.md`:

```markdown
<!-- agit:begin -->
## Session version control (agit)
Use these rules when the user requests an AgentGit operation or `AGIT_SESSION` or `AGIT_MERGE_TX` identifies the current managed session. Otherwise continue the user's task without agit checks, transcript discovery, or session adoption.

The working directory and this file alone do not activate AgentGit.

- Inspect `agit status --json` when the requested operation needs session or workspace state.
- Settle completed phases with `agit commit <owner/repo>@<branch> --milestone "<summary>"` (add `--code` when relevant).
- If resumed as a merge agent, follow the `AGIT_MERGE_TX` protocol in the agit skill.
- Never rebase or force-push AgentGit history; remove context with `agit revert <owner/repo>@<branch>#n..k`.
<!-- agit:end -->
```

---

## 📊 7. Bảng So Sánh Git vs. agit

| Tiêu chí | Git | agit (AgentGit) |
| :--- | :--- | :--- |
| **Đối tượng quản lý** | Files, Code diff, Commits | Turns, Prompts, Thinking logs, ToolUse |
| **Đơn vị cơ sở** | Line changes (Diff) | Conversation Turn (Prompt + Agent Response) |
| **Mục tiêu chính** | Phiên bản hóa mã nguồn | Phiên bản hóa ngữ cảnh & lịch sử quyết định |
| **Khả năng cộng tác** | Review Code qua Pull Request | Review tư duy Agent, xem Prompt hiệu quả |
| **Chuyển đổi Agent** | Không hỗ trợ | `agit fork -as codex` chuyển đổi qua lại giữa Claude & Codex |
| **Bảo vệ Secret** | Dựa vào `.gitignore`, `git-secrets` | Secret Filter tự động mask token trong log hội thoại |

---

## 💡 Đánh giá & Khuyến nghị thực tế

1. **Khi nào nên dùng `agit`?**
   - Khi làm việc nhóm với AI Agents: Cần chia sẻ prompt patterns và cách agent debug cho đồng nghiệp.
   - Khi thực hiện các task phức tạp kéo dài nhiều ngày: Cần các điểm checkpoint (milestone) để quay lại nếu agent đi sai hướng.
   - Khi sử dụng song song nhiều AI Agents (Claude Code cho Planning/Arch, Codex cho Implementation).
2. **Lưu ý triển khai:**
   - Phải cài đặt skill secret guard ngay từ đầu session, vì một khi secret đã bị ghi vào history chưa masked thì việc thêm skill sau đó sẽ không xóa được lịch sử cũ.
   - Không lạm dụng việc commit mọi tin nhắn rác, chỉ nên tạo milestone commit sau khi hoàn thành một lát cắt chức năng (functional slice).

---

## 🔍 Từ khóa tìm kiếm liên quan
`agit`, `agent-git`, `Einsia`, `Claude Code Workflow`, `Codex CLI`, `Session Version Control`, `AI Handoff`, `Secret Masking`, `CLAUDE.md agit rules`, `Context Cherry-pick`.
