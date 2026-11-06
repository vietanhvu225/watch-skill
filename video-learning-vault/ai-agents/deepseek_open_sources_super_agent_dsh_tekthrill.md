# DeepSeek Open-Source Super-Agent DSH: Agent Code Không Cần Babysit

- **Video URL:** https://www.youtube.com/watch?v=RZ4YtHjkA88
- **Watch Skill ID:** `7979a1c80b99031a`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 02:45
- **Speaker:** TEKTHRILL
- **Channel:** TEKTHRILL

---

## 1. Tóm Tắt Điều Hành

```
┌─────────────────────────────────────────────────────────────────────┐
│          DSH SUPERAGENT — DeepSeek Open-Source (31/10/2026)         │
│          MIT License | 6,000+ stars trong 1 ngày                    │
├─────────────────────────────────────────────────────────────────────┤
│  Vấn đề: Agent thông thường cần người babysit từng session          │
│  Người có thể follow 3-5 sessions cùng lúc → bottleneck là CON NGƯỜI│
├─────────────────────────────────────────────────────────────────────┤
│  Giải pháp: 2-layer design                                           │
│  ┌─────────────────────┐    ┌────────────────────────────────────┐  │
│  │  ROUTER (always on) │    │  WORKER SESSIONS (nhiều song song) │  │
│  │  Đọc GitHub/Feishu  │───▶│  Mỗi ticket → session + folder    │  │
│  │  Tạo ticket từ req  │    │  Tasks không mix context           │  │
│  └─────────────────────┘    └────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────────┤
│  Kết quả: 15.7 phút vs 29.5 phút (~1.9x nhanh hơn)                │
│  Mục tiêu: Thay thế Claude Code (Bloomberg)                        │
└─────────────────────────────────────────────────────────────────────┘
```

**Triết lý cốt lõi:** "Take as little human attention as possible" — Agent chạy trong nền, con người chỉ nhận kết quả.

---

## 2. Bối Cảnh & Sự Kiện

- **Ngày phát hành:** 31/10/2026
- **Tổ chức:** DeepSeek Harness team
- **Tên dự án:** `dsh-superagent-prototype`
- **License:** MIT (mã nguồn mở hoàn toàn)
- **Design article:** "Exploring Superagent Design" / "Towards a More Autonomous Agent System"
- **Tốc độ viral:** 6,000+ stars trong ~1 ngày
- **Cạnh tranh:** Bloomberg đưa tin DeepSeek nhắm vào Claude Code của Anthropic

---

## 3. Vấn Đề Với Agent Thông Thường

```
  Most coding agents (Claude Code, Codex, v.v.):

  You ──▶ session 1 (babysit)
  You ──▶ session 2 (babysit)
  You ──▶ session 3 (babysit)
  You ──▶ session 4 (babysit)   ← Giới hạn con người: 3-5 sessions
  You ──▶ session 5 (babysit)

  Vấn đề: Bottleneck KHÔNG phải model — là CON NGƯỜI
  "The limit on multitasking isn't the model, it's you."
```

---

## 4. Giải Pháp: 2-Layer Design

### Layer 1: Router (Always On)

Router chạy liên tục, lắng nghe events từ:
```
  GitHub comment  ──┐
  Pull Request    ──┼──▶ ROUTER (always running) ──▶ Tạo Ticket
  Feishu message  ──┘    (ByteDance's workplace app)
```

Khi người được phép (authorized) yêu cầu → Router tạo **ticket**.

### Layer 2: Worker Sessions

```
  Ticket 1 ──▶ Worker Session 1 + Folder A  (độc lập)
  Ticket 2 ──▶ Worker Session 2 + Folder B  (độc lập)
  Ticket 3 ──▶ Worker Session 3 + Folder C  (độc lập)
  ...
  Mỗi task = session riêng + folder riêng → không bao giờ mix context
```

Khi task xong → kết quả trả về conversation gốc, thường dưới dạng **Pull Request**.

---

## 5. Triết Lý "Polite By Design" (000 Philosophy)

Agent tuân thủ 3 nguyên tắc giao tiếp:
```
  ✓ Confirms once       — Xác nhận request 1 lần
  ✓ Quiet while working — Im lặng trong khi làm việc
  ✓ Speaks only for:    — Chỉ lên tiếng khi có:
     • a result         → Kết quả
     • a question       → Cần hỏi thêm
     • a failure        → Thất bại
```

**Autonomous actions:** Việc không ai yêu cầu cần có **written team rule** và bắt đầu ở chế độ **chỉ ghi lại** (record-only mode — ghi những gì agent ĐÃ LÀM thay vì thực thi). Web panel chỉ hiển thị những gì cần human review.

---

## 6. Kết Quả Benchmark

| Metric | DSH Superagent | Single Session + Subagents |
|--------|----------------|---------------------------|
| Slowest task | **15.7 phút** | 29.5 phút |
| Tốc độ | **~1.9x nhanh hơn** | — |
| Test size | 300 messages, 20 real requests | — |

**Lưu ý quan trọng:**
- Đây là **simulated messages** từ DeepSeek
- Code quality **không được đo**
- DeepSeek tự gọi là **research prototype**, không phải sản phẩm chính thức
- "Review before you use it"

---

## 7. Yêu Cầu Cài Đặt

```bash
# Dependencies cần thiết:
□ DeepSeek Harness   (harness chính)
□ Node.js
□ Git
□ Python
□ Tmux
□ GitHub CLI (gh)

# Setup: Dùng chính coding agent để đọc setup guide và cài đặt
```

---

## 8. Ý Nghĩa Lớn Hơn

```
  Xu hướng AI coding đang dịch chuyển:

  Chatbot (babysit)     →    Background teammates
  "Mở chat, giao task,  →    "Agent tự đọc GitHub,
   theo dõi, can thiệp"       tự làm, trả PR"

  Con người từ: Người điều khiển từng bước
  Con người thành: Người review kết quả
```

TEKTHRILL đặt câu hỏi mở: "Would you let an AI agent watch your team's chats?"

---

## 9. Key Takeaways

1. **DSH Superagent = MIT open-source** — 31/10/2026, 6,000+ stars/ngày
2. **Bottleneck = con người** (3-5 sessions max) — không phải model
3. **2-layer**: Router always-on + Worker sessions song song
4. **Ticket system**: Mỗi request = ticket = session riêng + folder riêng
5. **Polite by design**: Confirm once → quiet → result/question/failure only
6. **~1.9x nhanh hơn** trong internal simulation (cần review độc lập)
7. **Bloomberg**: DeepSeek nhắm vào Claude Code
8. **Vẫn là prototype** — cần review kỹ trước khi dùng production

---

## Liên Kết Đến Các Note Liên Quan

- [[deepseek_super_agent_design_grokbot_agents_lustoykov]] — Phân tích thiết kế super agent kiểu Grokbot
- [[new_deepseek_harness_super_agent_cordis_sam_witteveen]] — DeepSeek Harness + Cordis framework
- [[deepseek_redefining_harnesses_creator_mode_prompts_aicodeking]] — Creator Mode prompts của DSH
- [[deepseek_superagent_harness_hands_on_aicodeking]] — Hands-on với DSH superagent
- [[deepseek_super_agent_plug_into_claude_code_rob_shocks]] — Tích hợp super agent vào Claude Code
