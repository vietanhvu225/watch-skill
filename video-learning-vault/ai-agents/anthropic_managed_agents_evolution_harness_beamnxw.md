# Cách Anthropic Xây Dựng Multi-Agent Harnesses: Claude Managed Agents | beamnxw

- **Video URL:** [https://www.youtube.com/watch?v=xHVbj_rOubw](https://www.youtube.com/watch?v=xHVbj_rOubw)
- **Video ID:** `xHVbj_rOubw`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-06
- **Duration:** 11:52
- **Speaker:** Product Engineer tại Anthropic (beamnxw channel)
- **Transcript File:** [anthropic_managed_agents_evolution_harness_beamnxw_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/anthropic_managed_agents_evolution_harness_beamnxw_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

```
┌──────────────────────────────────────────────────────────────────────┐
│   TIẾN HÓA CỦA AGENTIC SURFACES TẠI ANTHROPIC                       │
│                                                                      │
│  Gen 1: Messages API                                                 │
│  ┌────────────────────────────────────┐                              │
│  │ Tokens In → [Claude] → Tokens Out  │                              │
│  │ ❌ Bạn tự build: loop, context,    │                              │
│  │    tools, state, recovery, auth    │                              │
│  └────────────────────────────────────┘                              │
│            ↓                                                         │
│  Gen 2: Claude Agent SDK (harness của Claude Code)                   │
│  ┌────────────────────────────────────┐                              │
│  │ SDK: Loop + Context + Sandbox      │                              │
│  │ ❌ Bạn vẫn tự lo: hosting, scaling,│                              │
│  │    session state, observability    │                              │
│  └────────────────────────────────────┘                              │
│            ↓                                                         │
│  Gen 3: Claude Managed Agents ← HIỆN TẠI                            │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │  Bạn mang: Task + Agent Config + Custom tool logic             │  │
│  │  Anthropic lo: Harness, Tool Runtime, Sandbox, Session State,  │  │
│  │                Checkpointing, Credential Vaults, Hosting,      │  │
│  │                Scaling, Observability                           │  │
│  │                                                                │  │
│  │  3 Resources:   Agent (persona) │ Environment (sandbox)       │  │
│  │                              ↘ ↙                              │  │
│  │                            Session                             │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                      │
│  KEY ADVANCE: Brain (model) tách khỏi Hands (sandbox) → 60% faster  │
│  Advanced: Dreaming, Outcomes, Memory, Scheduled Agents, Vaults      │
└──────────────────────────────────────────────────────────────────────┘
```

Product Engineer của Anthropic trình bày (tại Code with Claude conference) về **ba thế hệ tiến hóa** của agentic surfaces: Messages API → Claude Agent SDK → Claude Managed Agents. Trọng tâm là kiến trúc mới trong Managed Agents: tách rời brain (agent loop) khỏi hands (sandbox/tools), session state persistent trên cloud, và các tính năng nâng cao như Dreaming (tự học từ transcript), Memory, Outcomes, Vaults, Scheduled Agents.

---

## 2. Ba Thế Hệ Agentic Surfaces Của Anthropic

### 2.1 Gen 1: Messages API — "Tokens In, Tokens Out"

**Messages API** = API cơ bản nhất, chỉ cung cấp model inference.

```
Input Tokens → [Claude] → Output Tokens

Developer phải tự build:
❌ Agentic loop (vòng lặp reasoning → action → observation)
❌ Context management
❌ Tool execution
❌ Session state & recovery
❌ Error handling
❌ Observability
❌ Custom tool logic
```

Đây là giai đoạn "sunrise of LLMs" — developer phải dựng toàn bộ infrastructure từ đầu.

### 2.2 Gen 2: Claude Agent SDK — Harness Trên Messages API

**Claude Agent SDK** = harness của Claude Code, extracted thành SDK.

```
Claude Agent SDK cung cấp:
✅ Agentic loop
✅ Context management
✅ Built-in tool execution (sandbox + file system)
❌ Developer vẫn phải tự lo:
   - Hosting & scaling
   - Session state management
   - Observability
   - Custom tool logic
```

> *"Under the hood it still uses Messages API for tokens in and tokens out. But you still had to manage hosting and scaling, session state, observability, custom tool logic."*

Agent SDK là harness — giải quyết vấn đề agentic loop, nhưng production infra vẫn là responsibility của developer.

### 2.3 Gen 3: Claude Managed Agents — Fully Managed

**Claude Managed Agents** = "bring your task, we handle the rest."

```
Developer mang đến:
├── Task (gì cần làm)
├── Agent configuration (persona, model, prompts)
└── Custom tool logic (nếu có)

Anthropic lo:
├── Purpose-built harness
├── Tool runtime & sandbox
├── Session state persistence & checkpointing
├── Credential Vaults (lưu API keys an toàn)
├── Hosting & scaling
└── Observability
```

---

## 3. Kiến Trúc Đột Phá: Brain Tách Khỏi Hands

### 3.1 Vấn Đề Của Kiến Trúc Cũ

```
Kiến trúc Gen 1/2 (monolithic):
┌──────────────────────────────────────────────┐
│          Single Container                    │
│  ┌────────────┐    ┌────────────────────────┐│
│  │ Agent Loop │ ←→ │ Tool Execution/Sandbox  ││
│  │  (brain)   │    │       (hands)           ││
│  └────────────┘    └────────────────────────┘│
└──────────────────────────────────────────────┘
Vấn đề:
- Mỗi lần start agent → high latency (toàn bộ container phải spin up)
- Single point of failure (container down = agent loop down)
- Sandbox không phải lúc nào cũng cần nhưng luôn chiếm tài nguyên
```

### 3.2 Kiến Trúc Mới: Separation of Concerns

```
Claude Managed Agents (decoupled):
┌────────────────────┐    ┌──────────────────────────┐
│   Agent Loop       │    │   Sandbox (on-demand)    │
│   (server-side)    │ ←→ │   Only spin up when      │
│   Always running   │    │   tool execution needed  │
│   in cloud         │    └──────────────────────────┘
└────────────────────┘
         ↑
   Lợi ích:
   - 60% faster to first token
   - Sandbox chỉ spin up khi cần → tiết kiệm tài nguyên
   - Agent loop không bị ảnh hưởng nếu sandbox fail
   - Scale độc lập từng layer
```

**Hệ quả quan trọng:**
> *"You can close your laptop and the agent is still running in the cloud."*

Agent loop chạy server-side trên cloud. Người dùng chỉ cần **gửi events** để điều khiển hướng của agent, không cần giữ connection liên tục.

---

## 4. Ba Resources Cốt Lõi Trong Managed Agents

### 4.1 Resource 1: Agent (Persona & Capabilities)

```python
# Định nghĩa Agent
agent = {
    "name": "SRE Agent",
    "model": "claude-opus-5-5",
    "system_prompt": "...",
    "tools": [...],
    "mcp_servers": [...],
    "skills": [...]
}
# → Versioned & immutable
# → Có thể rollback
# → Persisted tự động trên cloud
```

**Nội dung Agent config:**
- Persona: agent là ai, làm gì
- Model: model nào được dùng
- Prompts: system prompt
- Tools: các tools agent có thể dùng
- MCP Servers: external services
- Skills: pre-built capabilities

### 4.2 Resource 2: Environment (Infrastructure & Guardrails)

```python
# Định nghĩa Environment
env = {
    "container_type": "...",
    "networking": {
        "allowed_hosts": ["api.github.com", "..."],
        "blocked_hosts": [...]
    },
    "mounted_data": ["logs/", "skills/"]
}
# → Giới hạn networking boundary
# → Mount data/logs cho agent access
# → Unique ID → reusable
```

**Nội dung Environment config:**
- Container type
- Networking boundaries (allowed/blocked hosts)
- Data mounting (log files, skill files)
- Resource limits

### 4.3 Resource 3: Session (Combining Agent + Environment)

```python
# Khởi động Session
session = client.sessions.create(
    agent_id="agent_xxx",
    environment_id="env_xxx"
)
# → Agent + Environment → Session ID
# → Session tự persist trên cloud
# → Có thể list sessions: client.sessions.list()
```

**Session lifecycle:**
- Tạo session = spin up agent với config đã định
- Session persist — không cần lo lưu transcript
- Multi-session persistence tự động
- Events-based communication (push events vào, nhận events ra)

---

## 5. Event-Driven Architecture

### 5.1 Communication Model

```
Cloud Agent (server-side)          Developer/Product
      │                                   │
      │◄──── Send Event (task/input) ─────│
      │                                   │
      │───── Event Stream ──────────────►│
      │   (agent thinking, tool calls,    │
      │    results, custom tool use)      │
      │                                   │
      │◄──── Control Event ───────────────│
      │   (approve, redirect, inject)     │
```

### 5.2 Custom Tool Handling

```
Scenario: Agent cần tool chỉ available locally

Cloud Agent → Event: "Need custom_tool(args)"
                    ↓
           Developer Product receives event
                    ↓  
           Executes tool locally
                    ↓
           Sends result event back
                    ↓
           Cloud Agent continues
```

Với MCP servers: agent gọi thẳng MCP server, không cần developer làm intermediary.

---

## 6. Use Case Demo: SRE Agent

Từ workshop thực tế (GitHub: `anthropic/cwc-workshops`):

```
Scenario: Checkout service error rate spike

SRE Managed Agent:
1. Define agent (SRE persona + tools: logs, metrics, Kubernetes)
2. Define environment (access to monitoring APIs)
3. Mount logs file for agent to analyze
4. Start session
5. Send event: "Checkout service error rate is high, investigate"

Agent (server-side):
→ Reads logs via sandbox
→ Calls Kubernetes API to check pod status  
→ Queries metrics database
→ Correlates root cause
→ Reports findings + suggested fixes

Result: Production-grade from day 1
```

**Key insight:** Cùng code bạn dùng để prototype → deploy lên production, chỉ cần thay environment config.

---

## 7. Tính Năng Nâng Cao

### 7.1 Dreaming — Self-Improvement Từ Transcripts

```
Dreaming = Học từ session transcripts theo thời gian

Agent hoàn thành session → Transcript được lưu
                        ↓
              "Dreaming" process phân tích transcript
                        ↓  
              Trích xuất lessons learned
                        ↓
              Cập nhật memory store + tạo skills mới
                        ↓
              Agent thức dậy "thông minh hơn"
```

> *"Dreaming...learns from transcripts over time and implement skills and improves the memory store to perform better in the future."*

### 7.2 Memory — Học Từ Sessions Trước

Agent nhớ thông tin giữa các sessions. Không cần manually inject context từ session trước.

### 7.3 Outcomes — Iterative Quality Improvement

```
Outcomes = Agent iterate nhiều lần để đạt output chất lượng cao

Task → Agent attempt 1 → Evaluate
     → Refine → Agent attempt 2 → Evaluate
     → Refine → Agent attempt N → Final high-quality output

Giống "Ralph Breaks the Internet" (Wreck-It Ralph sequel) analogy
từ speaker — iterate on the product to perfection
```

### 7.4 Vaults — Credential Management

```
Vaults = Secure storage cho API keys và credentials

Developer upload credentials → Vault (encrypted, managed by Anthropic)
                                      ↓
Agent nhận credential khi cần → Không expose trong code/prompts
```

Giải quyết vấn đề bảo mật lớn trong agentic systems.

### 7.5 Scheduled & Triggered Agents

```
Examples:
- Cron-style: "Run at 1:00 AM every day"
- Event-triggered: "Run when new GitHub PR is opened"
- Time-based: "Daily digest at 8:00 AM for all users"

→ Không cần external scheduler
→ Managed by Anthropic infrastructure
```

### 7.6 Console Agent Builder

```
Anthropic Console → Agent Builder
"Describe your agent in plain text"
      ↓
AI generates production-grade configuration
      ↓
Export configuration → Use in product immediately
```

No-code path to production-grade agent configuration.

---

## 8. So Sánh Tổng Hợp Ba Thế Hệ

| Feature | Messages API | Claude Agent SDK | Managed Agents |
|---------|-------------|-----------------|---------------|
| Agent loop | ❌ Self-build | ✅ SDK | ✅ Managed |
| Context mgmt | ❌ Self-build | ✅ SDK | ✅ Managed |
| Tool execution | ❌ Self-build | ✅ Sandbox | ✅ Managed |
| Hosting/Scaling | ❌ Self-build | ❌ Self-build | ✅ Managed |
| Session persistence | ❌ Self-build | ❌ Self-build | ✅ Managed |
| Observability | ❌ Self-build | ❌ Self-build | ✅ Managed |
| Credential Vaults | ❌ Self-build | ❌ Self-build | ✅ Managed |
| Dreaming/Memory | ❌ | ❌ | ✅ |
| Scheduled Agents | ❌ Self-build | ❌ Self-build | ✅ Managed |
| Time to production | Weeks | Days | Hours |

---

## 9. Key Takeaways

```
Triết lý đằng sau Managed Agents:

Trước:  Prototype → (Weeks) → Production
        "It takes hours to get to a prototype, 
         then weeks to actually ship"

Sau:    Prototype = Production
        Cùng agent_id + environment_id
        → ship ngay không cần refactor
```

**Dấu hiệu chín muồi của ecosystem:**
- Anthropic chuyển từ "cung cấp model" → "cung cấp platform"
- Infrastructure concerns trở thành commodity (không còn là differentiator)
- Developer chỉ cần focus vào: **business logic, agent persona, và custom tools**

---

## 10. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

### Notes về Harness Architecture:
- [Agent Harness — Claude Code & Codex 200lab](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/agent_harness_claude_code_codex_200lab.md) — Giải thích Model vs Harness, 4 thành phần
- [Ba Cấp Độ Harness Engineering — Agentic Labs](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/the_3_levels_of_harness_engineering_agentic_labs.md) — Từ Config đến Swarm 3D
- [How to Build a Harness with JEV](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/how_to_build_a_harness_with_jev.md) — Xây harness từ scratch

### Notes về T3 Code (ADE tương tự nhưng cho devs):
- [T3 Code Đã Chinh Phục Jan Marshal](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/t3_code_won_me_over_jan_marshal.md) — Multi-harness orchestration layer
- [Sử Dụng AI Agents Sai Cách — Theo t3.gg](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/youre_using_ai_agents_wrong_theo_t3.md) — Workflow thực chiến

### Notes về Dreaming & Self-Improving Agents:
- [Self-Improving AI Agents — Evolving the Harness](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/self_improving_agents.md) — Nghiên cứu về agents tự cải thiện
- [Kỹ Sư Anthropic Ngừng Chế Tạo Agents: Sự Lên Ngôi Của Skills — Sharbel](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/anthropic_engineers_stopped_building_agents_skills_sharbel.md) — Từ agents sang skills

### Notes về Multi-Agent Architecture:
- [Claude Code Khai Tử Sub-Agents?](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/claude_code_getting_rid_of_subagents_aicodeking.md) — Khi nào nên dùng sub-agents
- [Orchestras Not Factories](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/orchestras_not_factories_charlie_holtz.md) — Triết lý orchestration đúng
- [Amazon Infinity AI Agent Runtime](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/amazon_infinity_ai_agent_runtime_rust_lets_get_rusty.md) — Managed runtime khác (AWS approach)

### Notes về Security & Credentials trong Agent Systems:
- [DeepSeek Super Agent Plug Into Claude Code — Rob Shocks](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/deepseek_super_agent_plug_into_claude_code_rob_shocks.md) — Docker security, credential isolation
- [Vercel CEO Agentic Engineering Workflow](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/vercel_ceo_agentic_engineering_workflow_guillermo_rauch.md) — Security sandboxes
