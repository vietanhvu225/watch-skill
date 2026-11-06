# Thực Chiến DeepSeek SuperAgent Harness — Có Thực Sự Tốt Không?

- **Video URL:** https://www.youtube.com/watch?v=ZIcvDE0HoWE
- **Watch Skill ID:** `09c842bacf5f5ebb`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 14:16
- **Speaker:** AICodeKing
- **Channel:** AICodeKing

---

## 1. Tóm Tắt Điều Hành

```
┌─────────────────────────────────────────────────────────────────────┐
│          "EXPLORING SUPER-AGENT DESIGN" — DeepSeek Harness Team     │
│          Phân tích kiến trúc + Demo PandaCode thực tế               │
├─────────────────────────────────────────────────────────────────────┤
│  VẤN ĐỀ: Bottleneck là CON NGƯỜI (3-5 sessions max)               │
│  Ý TƯỞNG: "Stop managing sessions" → 1 agent duy nhất bạn nói chuyện│
├──────────────────────────┬──────────────────────────────────────────┤
│  SUPER AGENT = 4 TÍNH CHẤT│  2-LAYER DESIGN                       │
│  • Continuous            │  Layer 1: Router (long-lived session)   │
│  • Productive            │    - Talks to people, hands out work    │
│  • Proactive             │    - No bash tool needed                │
│  • Persistent            │  Layer 2: Workers (isolated contexts)   │
│                          │    - 1 task + 1 folder per worker       │
│                          │    - Never talk to people directly      │
└──────────────────────────┴──────────────────────────────────────────┘
  KEY INSIGHT: Không thể dựa vào prompt để model tự delegate.
               Phải làm nó thành CẤU TRÚC CỦA HỆ THỐNG.
```

---

## 2. Nguồn Gốc & Disclaimer

- **Bài viết gốc:** "Exploring Super-Agent Design" — DeepSeek Harness team (tiếng Trung, bản dịch máy)
- **Prototype:** `dsh-superagent` trên GitHub — research code, **không phải sản phẩm chính thức**
- **Disclaimer quan trọng:** Bài viết về **kiến trúc** (architectural ideas), không phải danh sách features hiện có của DSH

**AICodeKing nói:** "One of the most useful things I have read about coding agents this year. Because it isn't selling a model or a feature. It starts from a problem you have probably felt already."

---

## 3. Vấn Đề Cốt Lõi

```
  REALITY: Người có thể follow 3-5 sessions cùng lúc
  
  PAST 5 SESSIONS:
  • Quên session nào biết gì
  • 1 session đã chờ approval 20 phút
  • 2 sessions đang touch cùng 1 pull request

  Single task limited by: MODEL
  Many tasks limited by: YOUR ATTENTION
```

**Giải pháp:** Stop making the session the thing you manage. Talk to ONE agent in places where you already work (team chat, GitHub comments) → system creates and manages sessions behind the scenes.

---

## 4. 4 Tính Chất Của Super Agent

| # | Tính chất | Mô tả |
|---|-----------|-------|
| 1 | **Continuous** | Luôn cảm giác là 1 agent. Plan A trong chat → Plan B trong PR comment → hiểu được sự thay đổi |
| 2 | **Productive** | Nhận instruction tiếp theo TRƯỚC KHI task trước xong |
| 3 | **Proactive** | Tự theo dõi, tự hành động mà không cần được nhắc |
| 4 | **Persistent** | Luôn online, tự phục hồi khi có lỗi |

---

## 5. 2-Layer Design: Router + Workers

### Trade-off cơ bản:
```
  Gửi mọi event vào 1 session:
  → Context hoàn hảo, nhưng chỉ làm 1 việc 1 lúc + mix tasks

  Mỗi event = 1 session mới:
  → Nhiều tốc độ, nhưng không có kết nối + nhiều agents reply riêng lẻ

  GIẢI PHÁP: 2 layers
```

### Layer 1: Router
```
  ROUTER (long-lived session, always running)
  ├─ Talks to people (team chat, GitHub comments)
  ├─ Hands out work → Workers
  ├─ Doesn't do the work itself
  ├─ Có thể chạy KHÔNG CẦN bash tool
  └─ Tracks how tasks relate

  Model choice: LIGHT model (router job đơn giản hơn trông)
```

### Layer 2: Workers
```
  WORKER (per-task)
  ├─ 1 task + 1 clean context + 1 folder
  ├─ NEVER talks to people directly
  ├─ Only reports back to Router
  └─ Model choice: BEST model (đây là nơi code được viết)
```

**Khác với normal agent team:** Lead trong normal team phải deeply understand + split task. Router chỉ decide ai nhận việc gì + track relations.

---

## 6. Cơ Chế Event-Driven & Filter

**Events = inputs thay thế prompts:**
```
  Group chat messages  ──┐
  Emails               ──┤  FILTER 1: Code (source + allow lists)
  Review comments      ──┤  FILTER 2: Model (plain language rules)
  CI results           ──┘       ↓
                            Router processes
```

### Subscription System (Clever Part!)
Khi worker nhận task, nó **subscribe** vào thread của task (ví dụ PR):
- Events mới trên thread → đi thẳng vào worker
- Router vẫn thấy nhưng không cần forward
- 2 workers cùng PR → cả 2 nhận cùng events → tự phát hiện conflict

### Context Window As Cache
```
  Model nhận:
  1. ID + short summary (đầu tiên)
  2. Things it will probably need next
  3. Long content: truncated + pointer to rest

  Every object links to related objects by ID
  → Router follows links back to OLD tasks weeks later
  → Called: "Chain of Recall"
```

---

## 7. Tính Năng "Silently Reproduce and Fix" (Future-Like)

Khi ai đó report issue:
```
  Agent (background) → Reproduce it
       ↓ Confirmed + simple fix?
  YES  → Open pull request WHILE people are still discussing
  NO   → Silent (you never hear about it)
```

AICodeKing: "That is pretty amazing, to be honest."

---

## 8. Persistence Architecture

```
  CLOUD (agent loop + saved state)
        ↕
  YOUR MACHINE (small daemon — just executes commands)
        ↕
  WATCHDOG (separate session, runs on schedule)
  → Phát hiện stuck agent (process healthy nhưng model going in circles)
```

**Channel-aware replies:** Hard limit 100 words per chat reply; detailed writing cho pull requests.

---

## 9. Kết Quả Benchmark (Chỉ Indicative!)

| Scenario | Typical | Slow end |
|----------|---------|----------|
| **Super agent** | 12.4 phút | 15.7 phút |
| Single session (neutral prompt) | — | 29.5 phút |

**Kết quả quan trọng nhất (theo AICodeKing):**
- Neutral prompt "use subagent when appropriate" → **0/5 runs** gọi subagent
- Aggressive delegation prompt → **4/5 runs** delegated (run thứ 5 tự làm vì task nhỏ)

> **"You can't rely on a prompt to make a model delegate. You have to make it the STRUCTURE OF THE SYSTEM."**

### Chi phí cache tokens (1 synthetic run):
| Model | Cost (23.2M cache tokens) |
|-------|--------------------------|
| DeepSeek v4.1 Flash (off-peak) | **~$0.07** |
| Sonnet 5.5 | **~$4.64** |

→ Design này chỉ hợp lý với **cheap models**.

---

## 10. Vấn Đề Chưa Giải Quyết

- **Long-term memory**: Vẫn là giới hạn chính sau nhiều tháng chạy
- **Security**: Agent đọc outside events → exposed to **prompt injection**. Cloud loop bị compromise → mọi machine kết nối đều có thể bị takeover
- **⚠️ CẢNH BÁO:** Đừng connect tới public group chat cho đến khi security được giải quyết

---

## 11. Cách AICodeKing Build Phiên Bản Thực Tế Với PandaCode

### Bots feature trong PandaCode:

```
  PandaCode Bot = Persistent agent
  ├─ Message từ Telegram / Slack / Discord / WhatsApp / local web chat
  ├─ Own memory + schedules + rules
  └─ Every conversation = normal PandaCode task (readable on desktop)
```

### "The Trick": More Than One Bot

```
  ROUTER BOT          BUILDER BOT         REVIEWER BOT
  "Only talk to me,   "Build what          "Review pull
   hand out work,      Router assigns"      requests"
   reply < 100 words"
  
  Model: DeepSeek V4.1 Flash   Model: Best available
```

**Cài qua:** `npm install -g @deepseek-ai/dsh` → PandaCode Router needs DSH

### Tính năng "Productive" trong PandaCode:
```
  User: /stop  (trong chat để hủy task)
  
  Watchdog: Separate "Radar" bot, runs every 30 minutes on schedule
            → Phát hiện stuck agent
```

### Persistence:
```
  Cloud task keeps running even when laptop is asleep
  → Back to worktree when back online
  
  Recurring: Set up once
  ├─ NIGHTLY: PR review
  └─ OR: Security scan (Cline Desktop schedule feature)
```

---

## 12. Gaps — PandaCode Khác Với DSH Thực Sự

| GAP | PandaCode | DSH Superagent |
|-----|-----------|----------------|
| **GAP 1** | Paired senders only (safer default) | Có thể sit in public group chat |
| **GAP 2** | GitHub comment không phải channel | Có thể follow Plan B từ PR comment |
| **GAP 3** | Mỗi chat thread = conversation riêng | One global context với shared memory |
| **GAP 4** | Router phụ thuộc instructions & rules | Proper subscription + pass-through system |

**AICodeKing trung thực:** "This is not a super agent. I'll show you where it falls short."

---

## 13. Nhận Xét & Key Takeaways

**Nhận xét cuối:**
"DeepSeek is right about the main point. Models are not the only thing limiting us anymore, and adding more sessions to a screen doesn't fix that."

**The real idea (đơn giản):**
```
  One agent talks to you
  + Isolated workers do the work  
  + Cheap code filters the noise
  + Independent watchdog checks progress
```

**Key takeaways:**
1. **Bottleneck = con người**, không phải model — 3-5 sessions là giới hạn thực
2. **"Stop managing sessions"** — 1 agent trong nơi bạn đã làm việc (chat, GitHub)
3. **Router = light model**, Workers = best model (ngược với normal lead-heavy teams)
4. **Prompt không đủ để delegate** — phải làm thành cấu trúc hệ thống
5. **Cost: DeepSeek Flash $0.07/run vs Sonnet $4.64/run** — cheap model là bắt buộc
6. **Security warning**: Prompt injection risk khi connect tới public channels
7. **PandaCode Bots**: Thực tế, không hoàn hảo, nhưng workable version ngay hôm nay

---

## Liên Kết Đến Các Note Liên Quan

- [[deepseek_open_sources_super_agent_dsh_tekthrill]] — TEKTHRILL overview DSH superagent
- [[deepseek_redefining_harnesses_creator_mode_prompts_aicodeking]] — Creator Mode của DSH
- [[deepseek_super_agent_design_grokbot_agents_lustoykov]] — Phân tích thiết kế super agent kiểu Grokbot
- [[new_deepseek_harness_super_agent_cordis_sam_witteveen]] — Sam Witteveen: Cordis + DSH
- [[claude_code_getting_rid_of_subagents_aicodeking]] — So sánh: fold subagents (Anthropic)
- [[tiny_local_model_outperforms_opus_orchestration_aicodeking]] — PandaCode Panda Router
