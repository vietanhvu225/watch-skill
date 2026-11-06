# Claude Code Đang Loại Bỏ Sub-Agents? — Phân Tích Bài Viết Của Anthropic

- **Video URL:** https://www.youtube.com/watch?v=fdtkh30oFA8
- **Watch Skill ID:** `759a58fea2734de2`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 11:04
- **Speaker:** AICodeKing
- **Channel:** AICodeKing

---

## 1. Tóm Tắt Điều Hành

```
┌─────────────────────────────────────────────────────────────────────┐
│          "FOLD SUBAGENTS INTO MAIN LOOP" — Anthropic / Diane Sarkis │
│                        (22/10/2026)                                  │
├─────────────────────────────────────────────────────────────────────┤
│  VẤN ĐỀ: Nhiều team sao chép sơ đồ tổ chức vào agent architecture  │
│  → billing subagent, support subagent, search subagent, ...         │
│  → Conway's Law: hệ thống phản chiếu cấu trúc tổ chức              │
├─────────────────────────────────────────────────────────────────────┤
│  GIẢI PHÁP: Một main agent loop duy nhất                            │
│  → Progressive disclosure: skills & tools tải theo nhu cầu         │
│  → Anthropic giảm Claude Code system prompt >80% bằng cách này     │
├──────────────────────────┬──────────────────────────────────────────┤
│  KHI NÀO GIỮ SUBAGENT?  │  KHI NÀO GỌP VÀO MAIN LOOP?            │
│  • Verification độc lập  │  • Mỗi bước xây trên bước trước         │
│  • Parallel work          │  • Các bước chia sẻ dữ liệu            │
│  • Modular permissioning │  • Thường gọi cùng nhau                 │
│  (đo được, có lý do đặt │  • Routing/planning có thể tự làm        │
│   tên cụ thể)            │                                          │
└──────────────────────────┴──────────────────────────────────────────┘
```

**Luận điểm chính:** Mặc định dùng one chat. Chỉ nâng lên team mode khi có lý do cụ thể đặt tên được.

---

## 2. Nguồn Gốc & Bối Cảnh

Video phân tích bài viết chính thức của Anthropic: **"Building agents: When to fold subagents into your main agent loop"** do **Diane Sarkis** (Claude Platform) viết, đăng ngày 22/10/2026.

AICodeKing trình bày bài viết theo 2 phần:
1. Phân tích nội dung bài viết bằng ngôn ngữ đơn giản
2. Demo thực tế trên **PandaCode** — công cụ tích hợp cả single-agent mode và team mode

---

## 3. Vấn Đề Cốt Lõi: Conway's Law Trong Agent Design

### 3.1 Lỗi Phổ Biến: Copy Org Chart Vào Agent

Nhiều công ty copy sơ đồ tổ chức (org chart) vào kiến trúc agent:
- Billing team → Billing subagent
- Support team → Support subagent
- Search team → Search subagent
- Orchestrator ngồi trên điều phối

**Vấn đề:** Trông có vẻ có tổ chức trên diagram, nhưng thực tế **làm giảm chất lượng, tăng chi phí và latency**.

### 3.2 Giải Pháp: Progressive Disclosure

Thay vì load tất cả subagents trước, dùng **progressive disclosure**:
- Agent bắt đầu với mô tả ngắn
- Tải skill/tool đầy đủ **chỉ khi cần**
- Kết quả: Anthropic giảm Claude Code system prompt **>80%** cho các advanced models

---

## 4. Khi Nào Subagent Vẫn Hữu Ích

Bài viết không nói subagent luôn tệ. 3 trường hợp subagent vẫn nên dùng:

### 4.1 Verification (Kiểm Chứng Độc Lập)
```
  maker (viết code) → artifact → fresh agent (không viết code này)
  "looks fine to me" ×              ✓ mistake caught
```
Fresh agent chưa tạo ra artifact nên không bị bias — có thể bắt lỗi mà maker tự thuyết phục là đúng.

### 4.2 Parallel Work (Công Việc Song Song)
Các pieces không phụ thuộc nhau → chạy song song thực sự tiết kiệm thời gian.

### 4.3 Modular Permissioning (Phân Quyền Module)
Cho subagent chỉ những access cần thiết thay vì cấp full access cho một agent lớn.

---

## 5. Ba Failure Modes Khi Split Quá Nhiều

### Failure Mode 1: Subagent Sprawl
Mỗi team ship subagent riêng, overlap nhau, không ai sở hữu toàn bộ set.

### Failure Mode 2: The Frankenprompt
```
  system prompt:
  ├─ "Be brief."           ← team A thêm
  ├─ "Explain every step." ← team B thêm
  ├─ "Never use lists."    ← team C thêm
  └─ "Always use a list."  ← team D thêm
        → THE RULES FIGHT EACH OTHER
```

### Failure Mode 3: The Telephone Game
```
  agent 1 → agent 2 → agent 3 → agent 4
  [full detail] → [a bit lost] → [more lost] → [faded copy]
  + mỗi handoff thêm latency
```

**Con số thực tế:** Trong một case study, single agent loop chạy **~2x nhanh hơn** multi-agent setup mà nó thay thế.

---

## 6. Test Của Bài Viết: Cái Gì Ở Lại Trong Main Loop?

**Giữ trong main loop khi:**
1. **Mỗi bước xây trên bước trước** (sequential dependency)
2. **Các bước chia sẻ cùng dữ liệu**
3. **Thường gọi cùng nhau** — nếu 2 pieces luôn đi cùng nhau, split chỉ thêm handoff vô nghĩa

**Pick the right container trong main loop:**
```
  Domain judgment (cách expert xử lý case) → SKILL
  Action / live data / hard limit           → TOOL
  Facts chỉ cần thỉnh thoảng              → KNOWLEDGE BASE LOOKUP
```

### 6.1 Fold Back: Nhiều Router/Planner Giờ Có Thể Xóa

Các model hiện đại tự làm routing, planning, categorizing trong normal reasoning. Ví dụ bài viết: Classifier + pipeline + branches + orchestrator → thay bằng **1 prompt rõ ràng + 4-5 thin tools + guardrails**.

**Cảnh báo quan trọng:** Tool choice degraded khi vượt **30-50 tool definitions** trong prompt. Nên: 1 loop + small set well-named tools + phần còn lại disclosed khi cần.

---

## 7. Quy Trình Migration (Làm Thế Nào Chuyển Đổi An Toàn)

```
1. Fold từng subagent một lần
   → Bắt đầu với pair hay dùng nhất hoặc tightly coupled nhất
   → Convert subagent's prompt → skill
   → Convert API calls → tools

2. Đo lường trước khi chuyển cái tiếp theo
   → Dùng ít nhất 10 golden examples (tasks với kết quả đã biết)
   → Chạy old setup và folded setup trên cùng examples

3. Run mỗi eval NHIỀU lần
   → Case study: folding silently broke flow, chỉ xuất hiện trong 1/2 runs
   → Run 1 lần pass may mắn = ship coin flip
```

---

## 8. Checklist: Khi Nào GIỮ Subagent Riêng

**Giữ separate CHỈ KHI (phải đo được):**

| # | Điều kiện | Mô tả |
|---|-----------|-------|
| 1 | **Context makes main worse** | Đo được rằng context của subagent làm main agent tệ hơn |
| 2 | **Parallel fresh contexts** | Work chạy tốt song song trong fresh contexts có thể throw away khi xong |
| 3 | **Tool/instruction conflict** | Một agent liên tục struggle với tool choice hoặc conflicting instructions |

**Quy tắc chung:** Split by **context** (how little the work needs from the rest), NOT by **problem type** (billing vs support vs search).

```
  Cho separate agent:
  ├─ work cần ít từ phần còn lại của system
  └─ trả về 1 compact result (independent research, black-box check)
```

---

## 9. Demo Thực Tế: PandaCode

AICodeKing demo bài viết trên **PandaCode** — app có cả single agent mode và team mode.

### 9.1 PandaCode Team Architecture
```
  ARCHITECT (Claude Opus 5.5 · max)
  ├─ Plans task
  ├─ Splits into assignments
  ├─ Dispatches workers in parallel (one batch)
  └─ Reviews results

  WORKERS (mỗi worker = fresh context riêng)
  ├─ Read-only workers: inspect & review (không thay đổi code)
  └─ Writing workers: declare narrow file scope trước khi bắt đầu
       worker 1 → owns: src/stats/goal.ts (data code)
       worker 2 → owns: src/stats/goal-calc.ts (calc code)
       worker 3 → owns: tests/stats/ (test code)
```

**Nếu 2 writing scopes overlap:** PandaCode chạy tuần tự thay vì song song.

### 9.2 "Fold" trong PandaCode

Bài viết của Anthropic dùng từ "fold". Trong PandaCode:
```
  Article: "fold subagents into main loop"
       ↕ (tương đương)
  PandaCode: "Team off = one chat / Team on = architect + workers"
```

AICodeKing lưu ý: "fold" là từ của article, không phải PandaCode. Đây là interpretation.

### 9.3 Khi Nào Team Wins (PandaCode)

| Scenario | Mode |
|----------|------|
| Read-only research song song nhiều phần codebase | TEAM |
| Feature lớn: data side + interface + tests + review | TEAM |
| Compare model outputs cùng prompt | TEAM |
| Bulk edits với cheap worker, strong model review | TEAM |
| Task quá lớn cho 1 context window | TEAM |
| One-file typo / small config fix | SINGLE CHAT |
| Interactive tweaks (cần xem từng step) | SINGLE CHAT |
| Debug 1 failing test (state shared) | SINGLE CHAT |
| Edits cùng file liên tiếp | SINGLE CHAT |

### 9.4 Chi Phí Của Team Mode

```
  Team mode ≠ free speed

  Bạn nhận được:          Bạn trả bằng:
  ├─ Parallel time    ←→  ├─ Token nhiều hơn (mỗi worker = 1 model run)
  └─ Fresh eyes       ←→  └─ Lost detail ở mỗi handoff (telephone game)
```

---

## 10. Quy Tắc Cá Nhân Của AICodeKing

```
  ┌─────────────────────────────────────────────────────────┐
  │  DEFAULT: one chat                                       │
  │                                                          │
  │  EARN the team with a reason you can NAME:              │
  │  ✓ Parallel research                                     │
  │  ✓ Clean split (ít overlap)                             │
  │  ✓ Separate check / verification                        │
  │  ✓ Job too long for one context                         │
  │                                                          │
  │  Can't name the reason? → STAY IN MAIN LOOP             │
  └─────────────────────────────────────────────────────────┘
```

**Kết luận:** PandaCode giữ cả 2 options chỉ 1 decision apart — "fold back to single agent" = chỉ cần chọn Team off lúc bắt đầu task, không cần rewrite.

---

## 11. Key Takeaways

1. **Mặc định one chat** — team mode phải có lý do đặt tên được
2. **Progressive disclosure** > loading everything upfront
3. **Split by context** (how isolated the work is), NOT by problem type/org structure
4. **3 failure modes** khi split quá nhiều: sprawl, Frankenprompt, telephone game
5. **Migrate từng bước**: 1 subagent, đo, rồi mới chuyển cái tiếp theo
6. **Run eval nhiều lần** — 1 run pass có thể là may mắn
7. **Tool limit ~30-50** definitions — vượt qua thì tool choice degraded
8. **Verification subagent** vẫn hữu ích (fresh eyes, no bias)

---

## 12. Thông Tin Video

- **Bài viết gốc:** "Building agents: When to fold subagents into your main agent loop" — Diane Sarkis, Claude Platform, 22/10/2026
- **Công cụ demo:** PandaCode (architect + workers architecture)
- **Điểm đặc biệt:** AICodeKing dùng custom diagram slides (trắng-đen) giải thích rõ từng concept, không chỉ screen record

---

## Liên Kết Đến Các Note Liên Quan

- [[anthropic_managed_agents_evolution_harness_beamnxw]] — Multi-agent harness evolution từ Anthropic
- [[claude_code_mods_ultimate_customization_guide]] — Claude Code customization deep dive
- [[deepseek_super_agent_design_grokbot_agents_lustoykov]] — So sánh thiết kế super agent
- [[herdr_plugins_multi_agent_orchestration]] — Multi-agent orchestration patterns
- [[tiny_local_model_outperforms_opus_orchestration_aicodeking]] — Orchestration với local models
