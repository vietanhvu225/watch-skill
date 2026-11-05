# T3 Code Đã Hoàn Toàn Chinh Phục Tôi | Jan Marshal

- **Video URL:** [https://www.youtube.com/watch?v=SAFrydhpgJo](https://www.youtube.com/watch?v=SAFrydhpgJo)
- **Video ID:** `SAFrydhpgJo`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-06
- **Duration:** 32:18
- **Speaker:** Jan Marshal (SyntaxPath)
- **Transcript File:** [t3_code_won_me_over_jan_marshal_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/t3_code_won_me_over_jan_marshal_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

```
┌─────────────────────────────────────────────────────────────────────┐
│          T3 CODE: ADE (Agentic Development Environment)             │
│                                                                     │
│  T3 Code ≠ Harness (không phải harness!)                            │
│  T3 Code = Multi-Agent Orchestrator built ON TOP OF harnesses       │
│                                                                     │
│  ┌────────────┐    ┌─────────────────────────────────────────────┐  │
│  │  T3 Code   │    │          Harnesses (providers)              │  │
│  │ Application│    │  ┌──────────┐ ┌─────────┐ ┌───────────┐    │  │
│  │  Desktop/  │───▶│  │ Claude   │ │  Codex  │ │  Cursor   │    │  │
│  │  Mobile    │    │  │   Code   │ │ (OpenAI)│ │  (Grok)   │    │  │
│  └────────────┘    │  └──────────┘ └─────────┘ └───────────┘    │  │
│         │          └─────────────────────────────────────────────┘  │
│         ▼                           │                               │
│  ┌────────────┐                     ▼                               │
│  │Orchestrator│    ┌─────────────────────────────────────────────┐  │
│  │    V2      │    │              Environment                    │  │
│  │(cross-     │    │  Project files, Git worktrees, Terminal,    │  │
│  │harness)    │    │  Tools (Skills, MCP servers)                │  │
│  └────────────┘    └─────────────────────────────────────────────┘  │
│                                                                     │
│  KILLER FEATURES: Orchestrator V2, T3 Connect, Visualization        │
│  ANNOYANCES: No code diff in sub-agents, no sidebar grouping        │
└─────────────────────────────────────────────────────────────────────┘
```

Jan Marshal vốn ghét T3 Code khi dùng lần đầu, nhưng lần thử thứ hai khiến anh phải thay đổi hoàn toàn quan điểm. Video giải thích rõ T3 Code là ADE (Agentic Development Environment) chứ không phải harness, trình bày Orchestrator V2 (cross-harness orchestration), T3 Connect (sync engine đa thiết bị), và cả những điểm chưa tốt.

---

## 2. T3 Code Là Gì? Làm Rõ Khái Niệm

### 2.1 T3 Code ≠ Harness

**Harness** = layer cung cấp tools cho model và quản lý cách model hoạt động:
- Đọc/ghi files, chạy commands, quản lý context, hỏi permissions
- Ví dụ: Claude Code, Codex, Cursor — mỗi cái có harness riêng

**T3 Code** = ADE (Agentic Development Environment):
- Không tự mang harness của mình
- Builds ON TOP của các harnesses có sẵn (Claude Code, Codex, Cursor)
- Thêm features nằm ngoài và trên các harnesses đó

```
Misconception phổ biến:
"T3 Code là harness tốt nhất hiện tại"

Thực tế:
T3 Code không phải harness. Nó là layer 
orchestration + UX bên trên các harnesses.
```

### 2.2 Tại Sao Không Phải Harness Lại Là Lợi Thế?

**OpenCode** có concept tương tự (dùng subscriptions có sẵn), nhưng có vấn đề: OpenCode dùng harness riêng → Anthropic cấm dùng Claude Code subscription trong OpenCode.

**T3 Code** không có harness riêng → T3 Code dùng thẳng Claude Code harness → Anthropic cho phép sử dụng subscription → **Người dùng được subsidized usage**.

> *"You can exploit the subsidized usage provided by the providers, Anthropic, OpenAI, Google. That's essentially a little cheat code."* — Jan Marshal

---

## 3. Orchestrator V2: Cross-Harness Orchestration

### 3.1 V1 vs V2

| Feature | V1 (Stable) | V2 (Nightly) |
|---------|-------------|--------------|
| Multi-harness | ❌ Bị lock vào 1 harness | ✅ Dùng nhiều harnesses |
| Provider switch | ❌ Không thể | ✅ Mid-session |
| Context handoff | ❌ Mất context | ✅ Tự động tạo handoff |
| Tốc độ | Chậm hơn | Nhanh hơn, smooth hơn |

**Khuyến nghị**: Luôn dùng **Nightly version** (nhiều features hơn, nhanh hơn).

### 3.2 Cross-Harness Demo

```
Scenario: Main agent = Codex (GPT-6.1 Soul)
          Sub-agent  = Claude Code (Haiku 5.5)

Prompt:
"Find out when this repository was created, 
 how many PRs have been created, 
 and when the last PR was merged."
 → Spin up a Haiku 5.5 sub-agent via Claude Code harness

Kết quả: 
- Main agent: Codex icon
- Sub-agent: Claude icon  
- Hai agents khác harness làm việc cùng nhau!
```

### 3.3 Context Handoff Mid-Session

**Use case thực tế của Jan Marshal:**

```
1. Đang dùng Opus 5.5 (Claude Code harness)
2. Claude usage limit hit!
3. Jan gửi: "Switch to GPT-6.1-Sol"
4. T3 Code tạo Context Handoff: Claude Opus 5.5 → GPT-6.1-Sol
5. GPT tiếp tục task không mất context

6. Codex usage limit hit!
7. Jan gửi: Switch sang Cursor (Grok 4.7)
8. Loop tiếp tục...
```

Vòng lặp: Claude Code → Codex → Cursor → Claude Code → ...

> *"When my Claude usage limit runs out, I switch to Codex. Once my Codex usage limit runs out, I switch to Cursor. This just goes in a loop. That's quite powerful."*

---

## 4. T3 Connect: Sync Engine Đa Thiết Bị

### 4.1 T3 Connect Là Gì?

Không chỉ là "remote access" — T3 Connect là **sync engine**:

```
Mac Studio (desktop) ←──── T3 Connect ────→ MacBook (laptop)
                                   │
                                   └────→ iPhone (mobile)

Tính năng:
✅ Mở session trên 1 thiết bị, tiếp tục trên thiết bị khác
✅ Thay đổi settings trên 1 thiết bị → sync real-time sang tất cả
✅ Run agents trên máy A, monitor từ máy B
✅ Không cần terminal command phức tạp
```

### 4.2 So Sánh Với Claude Code Remote

| Approach | Claude Code (RC mode) | T3 Connect |
|----------|----------------------|------------|
| Setup | Mở terminal → `cloud rc` → chờ → config | Log in → automatic |
| Connection | Per-session | Always connected |
| Mobile | App riêng, cần setup | Cùng T3 Code app |
| Multi-device | Phức tạp | Native |

### 4.3 Connections & Multi-Environment

```
Settings → Connections:
- Mac Studio from Jan (local, đang dùng)
- MacBook from Jan (remote, luôn connected)

Khi tạo thread mới → chọn environment:
├── Mac Studio (local machine)
└── MacBook (remote machine — mọi thứ chạy trên đó)
```

**Use case**: Mua Mac mini, dùng làm worker machine. Connect từ MacBook/iPhone, run agents trên Mac mini. Tiết kiệm pin laptop, tận dụng Mac mini 24/7.

---

## 5. Những Điều Jan Yêu Thích

### 5.1 Usage Window — Tracking API Usage

```
T3 Code → Usage Window:
- Estimated API usage tổng
- Breakdown theo từng model
- Tokens: cached input / uncached input / output / cache savings
- 7-day/30-day/90-day history
- Model trend (which models đang dùng nhiều)

Jan Marshal: $100/tháng Claude Code → $3,000 worth of API credits
```

Không có provider nào khác cho xem breakdown chi tiết như thế này.

### 5.2 PR Review Tool

```
T3 Code → Pull Requests view:
- 11 open PRs tất cả listed
- Click vào xem summary + timeline + code review
- T3 Code tự subscribe vào PRs của bạn
- Tự động respond bot comments (không cần input thủ công)
```

### 5.3 Visualization Feature

Bất kỳ harness nào cũng có thể render visualization → Standardized primitive:

```
Prompt: "Create a high-quality visualization explaining 
         how Final Cut Pro motion templates work"

Kết quả: Interactive diagram có thể resize, 
         zoom, chỉnh margin — works với Claude Code, 
         Codex, Cursor, Antigravity đều như nhau
```

### 5.4 Quick Toggle Hover

Hover vào thread trong sidebar → instantly xem:
- Thread name
- Repository
- Machine
- Branch  
- Model đang dùng
- PRs đã tạo
- Status (limited = usage limit hit)

---

## 6. Những Điều Jan Không Thích

### 6.1 Không Thấy Code Changes Trong Sub-Agent

**Vấn đề lớn nhất**:

```
Claude Code: Instant xem từng bước agent làm
  → Đọc file X ✓
  → Ghi file Y ✓  
  → Chạy command Z ✓

T3 Code: Sub-agent chạy 33 phút, tạo 2000 dòng code
  → Bạn KHÔNG thấy gì cả
  → Chỉ thấy "done" ở cuối
  → Không có code diff review trong sub-agent view
```

> *"I often catch myself creating sub-agents and they will just sit there for 20 minutes straight and not do anything. And often I will then go into the main agent and say, 'Is the sub-agent lagging?' And the main agent will tell me, 'No, the sub-agent already created 300 lines of code.'"*

### 6.2 Sub-Agent Không Restart Sau Rate Limit

Khi rate limit hit → main agent restart ✅, nhưng sub-agents **không** restart ❌.

### 6.3 Sidebar Không Có Grouping

```
Claude Code sidebar:   T3 Code sidebar:
├── SyntaxKit/         ├── thread-1 
│   ├── session-1      ├── thread-2
│   └── session-2      ├── thread-3
├── Fable/             ├── thread-4
│   └── session-3      └── thread-5
└── ...                (không phân nhóm)
```

Jan muốn có folder grouping như Claude Code và Cursor.

### 6.4 Snapshot Feature Broken

Nightly version: snapshot/screenshot feature thỉnh thoảng bị broken, ảnh blur không load được.

### 6.5 Không Thể Dùng Primitives Của Harness Khác

Claude Code có "artifacts" → T3 Code không hiển thị được artifacts đó, dù đang dùng Claude Code harness.

---

## 7. Kết Luận & Verdict Của Jan

**Sau 6 ngày dùng liên tục:**
- Dùng T3 Connect 60% thời gian (MacBook ↔ Mac Studio)
- Zero issues với T3 Connect
- Paid players làm tệ hơn T3 Code (open source, free)

**Summary verdict:**

| Điểm mạnh | Điểm yếu |
|-----------|----------|
| Orchestrator V2 (cross-harness) | No code diff trong sub-agent |
| T3 Connect (sync engine) | Sidebar không có grouping |
| Usage tracking chi tiết | Snapshot broken trên nightly |
| Auto PR subscription + response | Sub-agent không restart sau rate limit |
| Visualization across harnesses | Artifacts của Claude không hiển thị |
| Free & Open source | — |

---

## 8. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

### Notes về T3 Code:
- [Sử Dụng AI Agents Sai Cách — Theo t3.gg](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/youre_using_ai_agents_wrong_theo_t3.md) — Workflow thực tế của Theo (tác giả T3 Code)
- [T3 Code Giải Thích Từ Góc Độ Nhật](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/t3_code_徹底解説_aimy.md) — Intro T3 Code cho người mới

### Notes về Harnesses:
- [Agent Harness — Claude Code & Codex 200lab](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/agent_harness_claude_code_codex_200lab.md) — Harness là gì
- [How to Build a Harness with JEV](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/how_to_build_a_harness_with_jev.md) — Xây harness từ đầu

### Notes về Multi-Agent Orchestration:
- [Herdr: Beginner To Advanced](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/herdr_beginner_to_advanced_lustoykov.md) — Tool orchestration alternative
- [High Throughput Agentic Engineering](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/high_throughput_agentic_engineering_kun_chen.md) — Scale agents

### Notes về Remote Machine & Multi-Device:
- [Amazon Infinity AI Agent Runtime](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/amazon_infinity_ai_agent_runtime_rust_lets_get_rusty.md) — Remote agent runtime
- [Vercel CEO Agentic Workflow](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/vercel_ceo_agentic_engineering_workflow_guillermo_rauch.md) — CEO workflow với AI agents
