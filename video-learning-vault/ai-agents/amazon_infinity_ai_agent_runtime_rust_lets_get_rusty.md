# Amazon Infinity — Runtime Rust Cho 75,000 AI Agents Đồng Thời — Let's Get Rusty

- **Video URL:** https://www.youtube.com/watch?v=os5eNeAVRpc
- **Watch Skill ID:** `f99f79c639eac3cb`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 07:06
- **Speaker:** Lý giải bởi Let's Get Rusty (Rust education channel)
- **Channel:** Let's Get Rusty

---

## 1. Tóm Tắt Điều Hành

```
┌────────────────────────────────────────────────────────────────────────────┐
│   AMAZON INFINITY: RUNTIME CHO EXTREME AGENT CONCURRENCY                   │
│   (AWS Open Source Blog — 23 Oct 2026 | by Shadaj Laddad)                 │
│                                                                            │
│  VẤN ĐỀ HIỆN TẠI: Agents dành phần lớn thời gian để... chờ               │
│    → Build có thể mất 20 phút                                              │
│    → Webhook fire sau nhiều giờ                                            │
│    → Con người trả lời vào ngày hôm sau                                   │
│    → Runtime cũ: giữ 1 process/agent suốt thời gian chờ → lãng phí RAM   │
│                                                                            │
│  GIẢI PHÁP: Infinity — "Scale to Zero" Agent Runtime                      │
│    → Idle agent = chỉ là conversation history (pure data)                  │
│    → 103 KB/agent → 75,000 agents trên 8GB Raspberry Pi RAM               │
│    → Hibernate khi chờ, wake up khi message đến                           │
│                                                                            │
│  3 KIẾN TRÚC CỐT LÕI:                                                     │
│  ┌────────────────────────────────────────────────────────────────┐        │
│  │  1. EXECUTION SLICES     → Giống async/await trong Rust        │        │
│  │     Mỗi slice = 1 turn; giữa slices = không có state machine  │        │
│  │                                                                │        │
│  │  2. RAP TOOL SERVER      → Reactive Agent Protocol             │        │
│  │     Tách biệt tool execution khỏi runtime slice               │        │
│  │                                                                │        │
│  │  3. EVERYTHING IS A MESSAGE (InputMessage)                     │        │
│  │     User text, tool results, synthetic tool calls = cùng type │        │
│  └────────────────────────────────────────────────────────────────┘        │
│                                                                            │
│  KEY INSIGHT: Reuse LLM tool call primitive để giao tiếp nội bộ           │
│  → Runtime tắt hoàn toàn giữa slices (true scale to zero)                 │
│  → Perfect cho AWS Lambda (serverless)                                     │
└────────────────────────────────────────────────────────────────────────────┘
```

AWS ra mắt **Infinity** (một dự án của Hydro) — Rust framework mã nguồn mở để xây dựng agentic systems đồng thời cực hạn, nhẹ đến mức chứa 75,000 agents trong 8GB RAM. Let's Get Rusty phân tích các quyết định kỹ thuật đằng sau.

---

## 2. Vấn Đề Cốt Lõi: Agents Dành 90% Thời Gian Chờ

```
Timeline điển hình của 1 agent:

  t=0      t=0.1s  t=20m    t=20m+1s  t=hours  t=hours+1s
   │          │      │          │         │          │
  [WORK]  [TOOL]  [wait...]  [WORK]   [wait...]  [WORK]
           call    build       0.1s    webhook     0.1s
                              done     response   done

Với runtime cũ:
  → 1 process/agent suốt timeline → lãng phí RAM cả lúc chờ

Với Infinity:
  → Agent = pure data khi chờ
  → Chỉ chiếm RAM khi đang thực sự chạy (slice)
```

**Nguồn gốc:** AWS blog post "Introducing Infinity: a runtime for extreme agent concurrency" — by Shadaj Laddad, 23 Oct 2026.

---

## 3. Kiến Trúc: 75k Agents Trên Raspberry Pi

### 3.1 Idle Agent = Pure Data

```
Idle agent (giữa các turns):
  - Conversation history  ← stored in durable storage
  - No task
  - No stack
  - No open connection
  → Chỉ ~103 KB/agent

Tính toán:
  8 GB RAM ÷ 103 KB/agent ≈ 75,000 agents đồng thời
```

**Triết lý:** "Infinity does for agents what async did for threads."

### 3.2 Execution Slices — Giống Async/Await

```
Infinity: Time-Sliced

  time →
  ─────────────────────────────────────────────────────
  Agent 1: [slice1]      [slice2]         [slice3]
  Agent 2:        [slice1]    [slice2]
  Agent 3:                         [slice1]    [slice2]
  ─────────────────────────────────────────────────────
  → 1 chỉ có "active slices" tại bất kỳ thời điểm nào
  → Giữa slices: không có in-process state machine
```

**Mapping sang Rust async:**
```
Rust async:                     Infinity:
─────────────────────────       ─────────────────────────
async fn process()          =   agent receiving message
  ...
  let result = api.await    =   model hits tool call
  ...                       =   RAP tool server executes
  use result                =   new slice starts, reconstructs
                                from durable storage
```

### 3.3 Anatomy of a Slice — Vòng Đời Đầy Đủ

```
InputMessage arrives (tool result / user text / event)
       │
       ▼
  Runtime Slice Starts
       │
       ├─→ Load history + processed IDs from Durable Storage
       ├─→ Prepare inputs (dedup, routing)
       │
       ▼
  Model Provider (Anthropic, OpenAI, etc.)
       │
       ├─→ Returns: text, reasoning tokens, OR tool call
       │
       ▼
  Persist new turn to Durable Storage
       │
       ├─→ If tool call: POST to RAP Tool Server (Fire and Forget!)
       │   └─→ 200 OK (acknowledgment only)
       │   └─→ Slice ENDS, process exits
       │
       └─→ RAP Tool Server executes (minutes/hours later)
              │
              ▼
         POST result to callback URL → new message on Input Queue
              │
              ▼
         New Slice Starts (reconstructs from Durable Storage)
```

**Fire and Forget:** Runtime nhận 200 OK từ RAP và kết thúc ngay. Không giữ connection mở.

---

## 4. RAP — Reactive Agent Protocol Tool Server

```
RAP (Reactive Agent Protocol) = Tách rời tool execution:

  Runtime Slice                  RAP Tool Server
  ─────────────                  ───────────────
  Nhận tool call request  ──→    Nhận POST
  Nhận 200 OK             ←──    Xử lý tool (file I/O, web, etc.)
  SLICE ENDS ✓                   ...chờ...
                                 Xong! POST result to callback URL
                           ──→   New InputMessage arrives
  New Slice Starts               → New Slice processes result
```

**Lợi ích:**
- Tool execution có thể mất minutes/hours/days
- Runtime không cần biết — nó đã kết thúc
- Khi tool xong → message mới → slice mới → xử lý tiếp

---

## 5. Everything Is A Message — InputMessage

### 5.1 Thiết Kế Thống Nhất

```
Mọi input đều là InputMessage:
  ┌───────────────────────────────────────────────────────┐
  │  InputMessage                                          │
  │  ├── user text          ← người dùng gõ               │
  │  ├── tool result        ← RAP trả về                  │
  │  ├── subscription event ← streaming events            │
  │  ├── OAuth challenge    ← auth flow                   │
  │  ├── timer wake-up      ← scheduled triggers          │
  │  └── synthetic tool call ← (đặc biệt!)               │
  │                                                        │
  │  Mỗi message có: group_id (conversation thread)       │
  └───────────────────────────────────────────────────────┘
```

### 5.2 Synthetic Tool Calls — Trick Thông Minh

**Vấn đề:** Làm sao notify runtime khi child thread xong mà không cần in-process state machine?

**Giải pháp:** Dùng lại primitive tool calls của LLM provider, nhưng đánh dấu `_injected`:

```json
// Regular tool call (spawn child thread):
{
  "tool": "spawn_thread",
  "args": {
    "instructions": "Review auth.ts for security issues"
  }
}

// Synthetic tool call (child thread completed):
{
  "tool": "spawn_thread_injected",  ← "_injected" suffix
  "args": {
    "result": "Found 3 security issues: ..."
  }
}
```

**Tại sao dùng tool call syntax thay vì user message?**
- User message → model tưởng user nói → confused
- Tool call syntax → model biết đây là system event
- `_injected` suffix → phân biệt synthetic vs real

**Kết quả:** Runtime có thể tắt hoàn toàn vì không có state machine nào cần giữ. Khi child xong → synthetic tool call message → new slice → reconstruct từ durable storage → tiếp tục.

---

## 6. Scale to Zero — Serverless Ready

```
Vòng đời memory của agent:

  Agent nhận message → Slice starts → load từ storage
                                         │
                             [RAM: conversation state]
                                         │
                       Slice ends (tool call dispatched)
                                         │
                             [RAM: 0 bytes ✓]
                                         │
  Tool completes → New message → New Slice → load lại
```

**Scale to Zero by default:**
- Idle agents: 0 RAM, 0 CPU
- Perfect cho AWS Lambda (pay per invocation)
- Không có "sleeping process" tiêu tốn tài nguyên

---

## 7. So Sánh Với Traditional Agentic Systems

| Tiêu chí | Traditional | Infinity |
|----------|-------------|---------|
| Memory/agent khi idle | Toàn bộ process (~100MB+) | ~103 KB (pure data) |
| Agents trên 8GB | ~80 agents | 75,000 agents |
| Tool execution | Blocking (giữ process) | Async (fire & forget) |
| State persistence | In-process state machine | Durable storage |
| Serverless friendly | ❌ (long-lived process) | ✅ (scale to zero) |
| Concurrency model | 1 process/agent | Async runtime (Tokio) |
| Failure recovery | Process crash = state lost | Reconstruct từ storage |

---

## 8. Rust Async Analogy — Tại Sao Rust?

```
Rust async/await:
  async fn foo() {
    let x = make_http_call().await;  ← pause point
    // ...
  }

Compile thành state machine:
  State 0: before await
  State 1: waiting for HTTP
  State 2: after await, have x

Infinity slices:
  Slice 0: nhận message, call model
  [PAUSE] tool call → RAP (fire & forget)
  Slice 1: nhận tool result, tiếp tục
```

**Key mapping:**
- `await` point = model hits tool call → slice ends
- State machine serialized to memory = Infinity persists to durable storage
- async runtime resumes correct state = new slice reconstructs from storage

**Tại sao Rust đặc biệt phù hợp:**
- Zero-cost abstractions → lightweight per-agent overhead
- Memory safety → không leak state giữa agents
- Tokio async runtime → thousands of agents on few threads
- Pure Rust → không GC pause, deterministic memory

---

## 9. Use Cases Thực Tế

```
Infinity phù hợp nhất cho:

  ✅ Customer service at scale
     → Hàng trăm nghìn requests → 1 agent/request
     → Agents chờ human reply → không tốn RAM

  ✅ CI/CD agents
     → Build 20 phút → agent hibernate → wake up khi done

  ✅ Monitoring agents
     → Chờ webhook hours → không tốn tài nguyên

  ✅ Scheduled agents
     → Timer wake-up → xử lý → hibernate lại

  ❌ Không phù hợp cho:
     → Agents cần low-latency streaming response
     → Single agent, high-performance inference loop
```

---

## 10. Triết Lý Cốt Lõi

```
"Infinity does for agents what async did for threads."

Trước async:
  1 thread = 1 connection → blocking I/O → scale kém

Sau async:
  1 thread = N connections → non-blocking → massive scale

Trước Infinity:
  1 process = 1 agent → sleeping processes → scale kém

Sau Infinity:
  1 runtime = N agents → hibernate/wake → extreme scale
```

---

## 11. Key Takeaways

```
3 insights quan trọng nhất:

  1. IDLE = PURE DATA
     → Đừng giữ process cho sleeping agents
     → Chỉ cần conversation history trong durable storage

  2. TOOL CALL AS COMMUNICATION PRIMITIVE
     → Reuse LLM tool call format cho synthetic events
     → Runtime không cần biết về event types → đơn giản hóa

  3. FIRE AND FORGET TOOL SERVER
     → Dispatch tool call → 200 OK → slice ends
     → Không cần long-lived connection
     → Tool results → messages → new slices

Áp dụng ngay:
  → Nếu có system cần >100 concurrent agents: xem xét Infinity
  → Nếu dùng AWS Lambda: Infinity scale to zero = cost efficient
  → Nếu agents của bạn chờ nhiều: đây là kiến trúc đúng
```

---

## 12. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

### Notes về Agent Runtime & Infrastructure:
- [Agent Desktop — Computer Use Không Cần Screenshot](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/agent_desktop_computer_use_accessibility_tree_prism_labs.md) — Pure Rust agent tooling (cùng ngôn ngữ)
- [The 3 Levels of Harness Engineering — Agentic Labs](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/the_3_levels_of_harness_engineering_agentic_labs.md) — Kiến trúc harness levels

### Notes về Multi-Agent Orchestration:
- [DeepSeek Super Agent Design — Grokbot Agents](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_super_agent_design_grokbot_agents_lustoykov.md) — Kiến trúc distributed agent (State/Compute/Environment separation)
- [Anthropic Managed Agents — Brain tách khỏi Hands](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/anthropic_managed_agents_evolution_harness_beamnxw.md) — Session-based architecture

### Notes về Scalable Agent Systems:
- [Claude Code Getting Rid of Sub-Agents](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/claude_code_getting_rid_of_subagents_aicodeking.md) — Single loop vs multi-agent trade-offs
- [My New AI Coding Setup — 30+ PRs/Day](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/my_new_ai_coding_setup_ship_30_prs_a_day.md) — Multi-agent practical setup
