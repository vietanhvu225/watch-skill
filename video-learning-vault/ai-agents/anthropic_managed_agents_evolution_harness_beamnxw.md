# Cách Anthropic Xây Dựng Multi-Agent Harnesses — Tiến Hóa Từ Messages API Đến Managed Agents

- **Video URL:** https://www.youtube.com/watch?v=xHVbj_rOubw
- **Watch Skill ID:** `6ea1264b7caa8adc`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 11:52
- **Speaker:** Gagan Bhat (Member of Technical Staff, Anthropic)
- **Channel:** hyperfixation on AI | beamnxw

---

## 1. Tóm Tắt Điều Hành

```
┌──────────────────────────────────────────────────────────────────────────┐
│   TIẾN HÓA CỦA AGENTIC SURFACES TẠI ANTHROPIC                            │
│   (Trình bày tại tokens& agentic engineering summit & Code with Claude)   │
│                                                                          │
│   Speaker: Gagan Bhat — Member of Technical Staff | ANTHROPIC            │
│                                                                          │
│   THẾ HỆ 1: Messages API                                                 │
│   → Tokens in, tokens out. Bạn tự build mọi thứ.                        │
│   → Context mgmt, agentic loop, session state, tools — TẤT CẢ của bạn   │
│                                                                          │
│   THẾ HỆ 2: Claude Agent SDK                                             │
│   → Agentic loop được cung cấp sẵn                                       │
│   → Context mgmt, sandbox, file system built-in                          │
│   → Nhưng: hosting, scaling, session state vẫn phải tự lo               │
│                                                                          │
│   THẾ HỆ 3: Claude Managed Agents (MỚI)                                 │
│   → Bạn chỉ mang: Task + Agent config + Custom tool logic                │
│   → Anthropic lo: Harness, Tool runtime, Sandbox, Session state,         │
│     Credential vaults, Hosting, Scaling, Observability                   │
│                                                                          │
│   KEY BREAKTHROUGH:                                                       │
│   "Brain tách khỏi Hands" — Agent loop server-side, Sandbox on-demand    │
│   → 60% faster startup, single point of failure eliminated              │
└──────────────────────────────────────────────────────────────────────────┘
```

Gagan Bhat (Anthropic) trình bày tại **tokens& Agentic Engineering Summit** (cũng là talk tại Code with Claude conference) về lịch sử tiến hóa 3 thế hệ của agentic surfaces và Claude Managed Agents mới nhất.

---

## 2. Ba Thế Hệ Agentic Surfaces

### 2.1 Thế Hệ 1: Messages API

```
Your Product
     │
     ▼
Agentic loop ← Bạn tự viết
     │
     ▼
   Claude  ← Messages API (tokens in/out)
     │
Production Infrastructure: Bạn tự lo TẤT CẢ
  Session mgmt  │  Observability  │  Credentials  │
  Hosting       │  Sandboxing     │               
```

**Gánh nặng developer:**
- Tự build agentic loop
- Tự manage context
- Tự execute tools
- Tự handle session state & recovery
- Tự handle errors, observability
- Tự build custom tool logic

### 2.2 Thế Hệ 2: Claude Agent SDK

```
Your Product
     │
     ▼
Claude Agent SDK ← Anthropic cung cấp
  ├── Agentic loop (run by Anthropic)
  ├── Context management
  ├── Built-in tool executions
  ├── Sandbox (filesystem capability)
  └── Still uses Messages API underneath

Production Infrastructure: VẪN phải tự lo
  Hosting  │  Scaling  │  Session state  │  Observability
```

**Claude Code là ví dụ điển hình của Agent SDK.**

**Vẫn còn gánh nặng:** Hosting, scaling, session state, observability, custom tool logic → "Still a lot of work to take to production"

### 2.3 Thế Hệ 3: Claude Managed Agents (Hiện Tại)

```
Your Product
     │  (chỉ mang)
     │  → Task
     │  → Agent configuration  
     │  → Custom tool logic
     ▼
Claude Managed Agents ← Anthropic lo TẤT CẢ
  ├── Purpose-built harness
  ├── Tool runtime & sandbox (on-demand)
  ├── Session state persistence & checkpointing
  ├── Auth & credential vaults
  ├── Hosting, scaling, observability
  └── Still uses Messages API tokens in/out underneath
```

---

## 3. 3 Resources Cốt Lõi Của Managed Agents

### 3.1 Sơ Đồ Kiến Trúc

```
┌────────────────────────────────────────────────────────────────┐
│  CLAUDE MANAGED AGENTS — 3 Resources                           │
│                                                                │
│  Resource 1: AGENT CONFIGURATION (versioned, immutable)        │
│  ┌─────────────────────────────────────────────┐               │
│  │  - Persona & capabilities                   │               │
│  │  - Which model to use                       │               │
│  │  - System prompts                           │               │
│  │  - Tools available                          │               │
│  │  - MCP servers                              │               │
│  │  - Skills                                   │               │
│  │  → Versioned: iterate & rollback if needed  │               │
│  └─────────────────────────────────────────────┘               │
│                                                                │
│  Resource 2: ENVIRONMENT (infrastructure & guardrails)         │
│  ┌─────────────────────────────────────────────┐               │
│  │  - Container configuration                  │               │
│  │  - Networking boundary (allowed hosts)      │               │
│  │  - Internet access level                    │               │
│  │  - Security guardrails                      │               │
│  │  → The environment your agent lives in      │               │
│  └─────────────────────────────────────────────┘               │
│                                                                │
│  Resource 3: SESSION                                           │
│  ┌─────────────────────────────────────────────┐               │
│  │  - Combines: Agent Config + Environment     │               │
│  │  - Spin up session with agent_id + env_id   │               │
│  │  - Session runs in cloud                    │               │
│  │  - You control via events                   │               │
│  └─────────────────────────────────────────────┘               │
└────────────────────────────────────────────────────────────────┘
```

### 3.2 Code Mẫu — SRE Agent

```python
# 1. Define agent configuration
agent = anthropic.beta.agents.create(
    name="sre-incident-agent",
    model="claude-opus-4-5",
    tools=[...],           # tools available
    system_prompt="...",   # agent persona
)
# → agent.id persisted in cloud (versioned, immutable)

# 2. Define environment
env = anthropic.beta.environments.create(
    name="sre-sandbox",
    networking={"allowed_hosts": ["api.internal.company.com"]},
    container_config={...}
)
# → env.id persisted in cloud

# 3. Mount data & start session
session = anthropic.beta.sessions.create(
    agent_id=agent.id,
    environment_id=env.id,
)
session.upload_file("incident_logs.txt")  # context for agent

# → Session runs in cloud with real sandbox
# → You can close your laptop — agent still runs
```

---

## 4. Key Architectural Breakthrough: Brain Tách Khỏi Hands

### 4.1 Vấn Đề Với Architecture Cũ

```
Architecture cũ (Claude Agent SDK):

  Container:
  ┌────────────────────────────────────────┐
  │  Agentic loop (brain)                  │
  │  Tool execution (hands)                │
  │  Sandbox                               │
  └────────────────────────────────────────┘

Vấn đề:
  → Mỗi lần start agent: HIGH LATENCY (phải khởi động toàn bộ container)
  → Single point of failure: container down = agent loop down
  → Sandbox chạy ngay cả khi agent không cần
```

### 4.2 Architecture Mới — Separated Brain & Hands

```
Claude Managed Agents Architecture:

  Cloud (Anthropic manages):
  ┌──────────────────────┐    ┌──────────────────────┐
  │  Agentic Loop        │    │  Sandbox (on-demand)  │
  │  (BRAIN)             │◄──►│  (HANDS)              │
  │  - Agent loop        │    │  - Tool execution     │
  │  - Context mgmt      │    │  - Filesystem         │
  │  - Orchestration     │    │  - Shell commands     │
  └──────────────────────┘    └──────────────────────┘
            ▲                           ▲
            │                           │
            └───────── Sessions ────────┘
                     Events in/out

Your Product:
  → Chỉ gửi events → nhận events
  → Không host gì cả
  → Không manage gì cả
```

**Lợi ích:**
- 60% faster startup (sandbox chỉ spin up khi cần)
- Không còn single point of failure
- Scale independently (brain vs hands)
- Not every use case needs sandbox → save cost

---

## 5. Event-Driven Communication

### 5.1 Sessions Speak in Events

```
Your Product (Client)          Cloud Agent (Server)
     │                               │
     │──── Send event ──────────────►│
     │     (task, instructions)      │  Agent working...
     │                               │
     │◄─── Receive events ───────────│
     │     (progress, tool use,      │
     │      results, questions)      │
     │                               │
     │──── Send event ──────────────►│
     │     (direction, approval)     │
     │                               │
  Close laptop                       │  Agent still running
                                     │  in cloud ✓
```

### 5.2 Luồng Event Điển Hình (SRE Agent Example)

```
Incident: checkout service high baseline

Your Product → Session (agent_id, env_id)
                    │
                    ▼
         Cloud Agent starts
                    │
         Sends events about investigation:
         ← "Reading logs..."
         ← "Found anomaly at 14:32"
         ← tool_use: "query_metrics_db"  → Local tool (your code handles)
         ← "Root cause: DB connection pool exhausted"
         ← "Recommended fix: restart connection pool"
                    │
Your Product → Approve fix
                    │
                    ▼
         Agent applies fix → Done
```

**MCP servers** có thể directly reach out mà không cần client handle.

---

## 6. Session Persistence & Multi-Session

```
Conversation lives in the CLOUD:

  Session 1  → Conversation stored server-side
  [Close app]
  Session 2  → Reconnect → continue where left off
  [Next day]
  Session 3  → Full history available

CLI command to list sessions:
  $ anthropic sessions list
  → Hiện toàn bộ sessions trong cloud
  → Không cần implement storage, read/write logic
```

---

## 7. Advanced Features — Beyond Basics

| Feature | Mô Tả |
|---------|-------|
| **Subagents / Multi-agents** | Spin off agents trong hierarchy — orchestrate phức tạp |
| **Memory** | Learn từ sessions → agent gets better over time |
| **Dreaming** | Tự học từ transcripts → implement skills mới → improve memory |
| **Outcomes** | Iterate nhiều lần để đạt high-quality output (like "Ralph Breaks the Internet") |
| **Vaults** | Store credentials safely → API keys cho services khác |
| **MCP servers** | Plug in MCP tools |
| **Webhooks** | Event-driven triggers |
| **Permission policies** | Fine-grained access control |
| **Interrupts** | Pause/resume agent execution |
| **Console Agent Builder** | Describe agent in plain text → production config (no-code!) |
| **Scheduled agents** | Cron-like triggers (daily digest at 8am, etc.) |

### 7.1 Dreaming (Chú Ý Đặc Biệt)

```
Dreaming = tính năng không được mention trong slides:

  Sessions → Transcripts
      │
      ▼
  Dreaming process (async)
      │
      ▼
  Learned patterns → New Skills
      │
      ▼
  Memory store updated
      │
      ▼
  Agent performs BETTER next time

→ Agent tự cải thiện từ kinh nghiệm thực tế
```

### 7.2 Console Agent Builder

```
Anthropic Console:
  "I want an SRE agent that monitors checkout service, 
   investigates incidents, and can restart services"
   
                    ▼
           [Console processes]
                    ▼
   Production-grade configuration:
   - System prompt (optimized)
   - Tool definitions
   - Safety guardrails
   - Resource limits
   
→ Zero code → production-ready agent config
```

---

## 8. Demo: Workflow Thực Tế

```
Bước 1: Define agent config (10 dòng code)
  → agent_id = persisted in cloud

Bước 2: Define environment
  → env_id = networking + container config persisted

Bước 3: Mount data + start session
  → Upload log files, skills
  → session = agent_id + env_id

Bước 4: Send task via event
  → "Investigate checkout service incident"

Bước 5: Receive progress events
  → Agent đang điều tra trong cloud sandbox

Bước 6: (Optional) Approve/redirect via events
  → Steer agent nếu cần

Bước 7: Done → move same config to production
  → Same agent_id, same env_id
  → Scalability comes built-in
```

---

## 9. Lợi Ích Thực Tế Cho Developer

```
Timeline truyền thống:
  Hours to prototype → Weeks to production
  (vì phải: hosting, session state, auth, observability, scaling...)

Với Claude Managed Agents:
  Minutes to prototype → Hours to production
  (vì Anthropic lo tất cả infrastructure)
```

**Key value proposition:**
> *"The fastest way to build and ship transformative agentic products."*

---

## 10. So Sánh 3 Thế Hệ

| Tiêu chí | Messages API | Claude Agent SDK | Claude Managed Agents |
|----------|-------------|-----------------|----------------------|
| Agentic loop | Bạn tự build | Anthropic cung cấp | Anthropic (server-side) |
| Context mgmt | Bạn tự lo | SDK handle | Cloud handle |
| Sandbox | Bạn tự lo | SDK có sẵn | On-demand (tách biệt) |
| Session state | Bạn tự lo | Bạn tự lo | Cloud persist |
| Hosting | Bạn tự lo | Bạn tự lo | Anthropic |
| Scaling | Bạn tự lo | Bạn tự lo | Built-in |
| Credential mgmt | Bạn tự lo | Bạn tự lo | Vaults |
| Time to prod | Weeks | Days | Hours |
| Customization | Maximum | High | Medium (custom tools) |

---

## 11. Key Takeaways

```
3 insights quan trọng nhất:

  1. BRAIN TÁCH KHỎI HANDS
     → Agent loop (brain) không cần chạy cùng sandbox (hands)
     → Sandbox spin up on-demand → 60% faster, no SPOF

  2. EVENTS = COMMUNICATION PROTOCOL
     → Sessions speak in events (send/receive)
     → Bạn steer agent, không execute nó
     → Close laptop, agent vẫn chạy

  3. CONVERSATION LIVES IN CLOUD
     → Multi-session persistence tự động
     → No storage code, no read/write logic
     → List all sessions với 1 command

Áp dụng ngay:
  → Dự án mới: bắt đầu với Managed Agents
  → Cần custom infra: dùng Claude Agent SDK
  → Cần control tối đa: Messages API
  → Workshop: github.com/anthropics/cwc-workshops
```

---

## 12. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

### Notes về Managed Agents & Architecture:
- [Anthropic Engineers Stopped Building Agents — Sharbel](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/anthropic_engineers_stopped_building_agents_skills_sharbel.md) — Skills thay thế agents, SKILL.md anatomy
- [Claude Code Getting Rid of Sub-Agents — AICodeKing](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/claude_code_getting_rid_of_subagents_aicodeking.md) — Single loop vs multi-agent

### Notes về Harness Engineering:
- [The 3 Levels of Harness Engineering — Agentic Labs](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/the_3_levels_of_harness_engineering_agentic_labs.md) — Harness levels
- [Agent Harness Claude Code Codex — 200lab](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/agent_harness_claude_code_codex_200lab.md) — Harness components

### Notes về Multi-Agent Systems:
- [Amazon Infinity — Runtime Rust 75k Agents](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/amazon_infinity_ai_agent_runtime_rust_lets_get_rusty.md) — Extreme concurrency runtime
- [Herdr Beginner to Advanced — lustoykov](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/herdr_beginner_to_advanced_lustoykov.md) — Multi-agent orchestration patterns

### Notes về Claude Code Evolution:
- [Claude Code 3.0 Parallel Desktop — Stack Delta](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/claude_code_3_parallel_desktop_stack_delta.md) — Claude Code 3.0 features
- [How Senior Engineers Build With AI — Intercom Clone](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/how_senior_engineers_build_with_ai_intercom_clone.md) — Senior engineering patterns
