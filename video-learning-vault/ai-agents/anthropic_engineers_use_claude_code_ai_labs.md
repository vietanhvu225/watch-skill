# Cách Kỹ Sư Anthropic Thực Sự Dùng Claude Code — AI LABS

- **Video URL:** [https://www.youtube.com/watch?v=92ZDgn8bgLM](https://www.youtube.com/watch?v=92ZDgn8bgLM)
- **Watch Skill ID:** `b159c5cb00750dd9`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 11:47
- **Speaker:** Felix Rieseberg (Kỹ sư Anthropic), được AI LABS phân tích và trình bày
- **Channel:** AI LABS (@AILABS-393 · 133K subscribers)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

```
┌──────────────────────────────────────────────────────────────────────┐
│   CÁCH DÙNG CLAUDE CODE ĐỂ ĐẠT KẾT QUẢ VƯỢT TRỘI                   │
│   (theo Felix Rieseberg — Kỹ sư Anthropic)                           │
│                                                                      │
│  Vấn đề cốt lõi: Claude luôn làm đúng những gì bạn hỏi              │
│  nhưng hiếm khi đạt kết quả "thực sự tốt" nếu bạn hỏi sai cách     │
│                                                                      │
│  3 CHIỀU PROMPT MÀ KỸ SƯ ANTHROPIC DÙNG:                            │
│                                                                      │
│  ┌──────────────────────────────────────────────────────────────┐    │
│  │  1. ALTITUDE (Độ cao)                                         │    │
│  │     Thay vì: "Build me a reports page"                        │    │
│  │     Hỏi về KẾT QUẢ ĐẦU RA CAO HƠN: "Build a dashboard       │    │
│  │     with actionable decisions, not just charts"               │    │
│  │     → Claude tự thêm features không được yêu cầu             │    │
│  │     → Gauge: Effort "Low" → tự leo thang lên "High"          │    │
│  │                                                               │    │
│  │  2. DEPTH (Độ sâu)                                           │    │
│  │     Prompt mẫu của Felix (felixrieseberg.com):               │    │
│  │     "I want a deeper version of this task, not a bigger one. │    │
│  │      Before rewriting it: (1) say what a top professional    │    │
│  │      would do here; (2) name the rawest material they'd      │    │
│  │      start from; (3) name the specific methods and tools;    │    │
│  │      (4) say what, other than you or me, would show the      │    │
│  │      result is right. Then rewrite my prompt to ask for      │    │
│  │      exactly that."                                           │    │
│  │                                                               │    │
│  │  3. RANGE (Phạm vi)                                          │    │
│  │     Nhìn công việc qua nhiều lăng kính chuyên gia:           │    │
│  │     Ví dụ: 6 specialists (Security, UX, Legal, Privacy,      │    │
│  │     Engineer, Designer) chạy tuần tự trong 1h59m             │    │
│  │     → Mỗi người xây dựng trên thành quả của người trước      │    │
│  └──────────────────────────────────────────────────────────────┘    │
│                                                                      │
│  ĐIỀU KIỆN BẮT BUỘC (Felix):                                         │
│  "Mỗi loại prompt CHỈ hoạt động nếu Claude được nhìn thấy           │
│   vấn đề + có đủ context + có môi trường test để kiểm chứng"        │
│                                                                      │
│  Ví dụ crash-free app: Sentry → MCP → Crash report → Claude Code    │
│  → Tự phát hiện bug → Tự fix → Tự verify không crash                │
└──────────────────────────────────────────────────────────────────────┘
```

AI LABS phân tích phương pháp của Felix Rieseberg (kỹ sư Anthropic) — tác giả trang `felixrieseberg.com/how-to-be-more-ambitious-a-tutorial-for-humans-with-models` — về cách prompt Claude Code để đạt kết quả vượt tầm yêu cầu ban đầu. Video trình bày **3 chiều prompt** (Altitude, Depth, Range), minh họa bằng case study hiring app thực tế, và giới thiệu `be-ambitious` SKILL.md dùng được trên mọi harness.

---

## 2. Vấn Đề Nền Tảng: Claude Làm Đúng Nhưng Chưa Đủ

### 2.1 Triết Lý Cốt Lõi

```
Bạn hỏi: "Build a reports page"
Claude xây: Reports page ✓ (đúng yêu cầu)

Nhưng thực ra bạn muốn: "Dashboard ra quyết định được ngay"
Claude không xây: Actionable insights, decision engine, recommendations

→ Lý do thất bại: Bạn hỏi về CÔNG CỤ, không phải về KẾT QUẢ
```

**Nghiên cứu Goal-Setting:**

```
"Do your best"              "Specific, hard goal"
     [3 người]                   [3 người + ✓]

Kết quả thực nghiệm: Nhóm có mục tiêu cụ thể và khó
đạt kết quả cao hơn đáng kể so với "cứ làm hết sức"
```

> *"The main reason you're using Claude Code is to get a result, not just to get a thing built."*

### 2.2 Hệ Thống AI Tools Hiện Tại

```
              [Terminal/Task]
                    │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
   Claude Code   Codex     Cursor
        ✓          ✓          
        └──────────┼──────────┘
                   ▼
                 Gemini
```

Video bối cảnh hóa trong hệ sinh thái multi-harness: Claude Code (✓ featured), Codex (✓), Gemini, Cursor — tất cả đều cần cách prompt đúng để phát huy tối đa.

---

## 3. Ba Chiều Prompt Của Felix

### 3.1 Chiều 1: ALTITUDE (Độ cao mục tiêu)

```
                    ↑ Altitude
                    │
            Cấp 3: │ Quyết định chiến lược
                    │ ("Follow up 3 stalled offers")
            Cấp 2: │ Insights hành động được
                    │ ("Hire faster — BEHIND schedule")
            Cấp 1: │ Dữ liệu thuần
                    │ ("42 candidates, 31 days, 78%")
                    │
                  Thấp         Cao → Effort
                     [Low ●────────── High]
```

**Ví dụ thực tế:**

```
Prompt kém:    "Build a reports page"
               → Claude xây: Bảng số, charts cơ bản
               → Effort gauge: LOW (Claude làm ít thôi)

Prompt tốt:    "Build a reports dashboard. But what I
                actually want is for our roles to get
                filled faster. Go through our hiring data,
                find what's slowing roles down, and build
                the page to flag stuck roles early and tell
                us what to do about them. Put the three most
                urgent decisions on it, with the numbers
                behind them."

               → Claude đọc 230,282p lib/db.ts + queries
               → Tự thêm: "DO THIS NEXT: Follow up 3 stalled offers"
               → Tự thêm: "Hire faster — BEHIND"
               → Effort gauge: HIGH (Claude làm hết mình)
```

**Nguyên tắc Altitude:** Hỏi về *outcome* (kết quả kinh doanh) thay vì *output* (sản phẩm kỹ thuật).

### 3.2 Chiều 2: DEPTH (Độ sâu chuyên môn)

**Prompt mẫu của Felix** (đăng công khai trên felixrieseberg.com):

```
"I want a deeper version of this task, not a bigger one.
 Before rewriting it:
 (1) say what a top professional in this field would do
     here that I'm not asking for;
 (2) name the rawest material they would start from
     instead of what has already been digested for me;
 (3) name the specific methods and tools they would use;
 (4) say what, other than you or me, would show the
     result is right.
 Then rewrite my prompt to ask for exactly that."
```

**Cách hoạt động:**

```
Trước khi Claude thực thi task:
  → Claude tự đặt câu hỏi: "Professional thực sự làm gì?"
  → Tìm raw materials (không phải derived fields)
  → Xác định tools cụ thể
  → Thiết kế verification mechanism
  → Rồi mới viết lại prompt → thực thi

Ví dụ hiring app:
  Task ban đầu: "Add a feature to rank candidates"
  
  Sau khi áp Depth prompt:
  "Build it from the raw record, not the derived fields.
   For each role, combine the job requirements, salary band,
   workplace, seniority AND its originating job_requests.notes
   with this client's past decline reasons...
   Apply hard filters before ranking: desired salary vs band,
   workplace preference, already withdrawn. Then place candidates
   in tiers (Put forward / Strong maybe / Hold) with an order
   inside each tier..."
   
  → Claude dùng codex CLI để review ranking feature
  → Chạy: wc -l lib/shortlist.ts → 87+ lines
  → Tự verify logic + chạy tests
```

### 3.3 Chiều 3: RANGE (Phạm vi đa chuyên gia)

```
Thay vì 1 agent làm tất cả:
  You → [Claude] → Result (nhìn 1 chiều)

Range = Nhìn từ NHIỀU vai trò chuyên môn:

  Your session (idle) ──→ Background session
                              ├── [1] Head of candidate experience
                              ├── [2] Client services director
                              ├── [3] Fair hiring & pay specialist
                              ├── [4] Privacy officer
                              ├── [5] Security engineer
                              └── [6] Content designer

  Mỗi specialist:
  - Làm real work trong lĩnh vực của mình
  - Chạy sequence (người sau kế thừa người trước)
  - Mỗi người: typecheck + lint + smoke test trước khi commit
  
  Kết quả: 1h 59m → Hiring app chất lượng production
           App quality bar: ████████████░░░ (tăng dần qua 5/6 bước)
```

**Triển khai bằng Workflow:**

```
Claude Code terminal:
> i want you to use the codex cli installed and then review
  the ranking feature that was implemented and then report
  back all the findings. you then need to fix those based
  on the findings returned by codex

Bash: which codex && codex --version → codex-cli 0.162.1

Workflow(Six field specialists each do their discipline's
real work on Hirefield, in sequence)
  Running in background · /workflows → medium size · /config

The specialists are working now. Six of them are going one
after another in the main worktree, each building on the
previous one's changes:
  1. Head of candidate experience: stopping candidates from being ghosted
  2. Client services director: the hiring manager's side
  3. Fair hiring & pay transparency specialist: legal and fairness
  4. Privacy officer: consent before sharing with client
  5. Security engineer: brute force on logins, scam jobs
  6. Content designer: rewriting every user-facing string
```

---

## 4. SKILL "be-ambitious": Kết Hợp Cả 3 Chiều

### 4.1 SKILL.md Felix Dùng

```yaml
# be-ambitious/SKILL.md
name: be-ambitious
description: Run a task the ambitious way, following
  Felix Rieseberg's "How to be more ambitious" method
  (Altitude, Depth, Range) and then actually do the
  work that way, not just talk about it. Climbs from
  the task to the goal beyond it, rewrites the ask into
  the deeper version a top professional would do,
  assembles a crew of fields as parallel subagents doing
  real field work, sets an unreasonable target and treats
  it as solvable, sets up an outside judge before building,
  executes, validates, and leaves a durable workflow so the
  human isn't needed next time.
  
  Use when the user says "be ambitious", "go deeper",
  "go beyond", "what would the best in the world do",
  "think bigger", invokes /be-ambitious, or hands over
  a substantial goal (a review, an analysis, a deck, a
  product problem, a reliability or performance target).
```

```
VSCode: be-am... / SKILL.md
  ├── references/
  │     └── [Felix's writing]
  └── SKILL.md ← file duy nhất cần thiết
```

**Điểm mấu chốt:** SKILL.md này hoạt động trên mọi harness — Claude Code, Codex, Cursor, Gemini — không phụ thuộc vào platform cụ thể.

### 4.2 Điều Kiện Tiên Quyết Của Felix

> *"Every one of these types only works if the agent can see the problem, enough context about it, and a test environment."*

```
3 Điều Kiện Bắt Buộc:
┌────────────────────────────────────────────────┐
│ 1. CAN SEE THE PROBLEM                         │
│    Agent phải thấy được vấn đề thực tế         │
│    → Crash reports, logs, error traces         │
│    → Không phải mô tả abstract của bạn         │
│                                                │
│ 2. ENOUGH CONTEXT                              │
│    Đủ ngữ cảnh để hiểu bài toán               │
│    → Codebase structure, business rules        │
│    → Tại sao feature này tồn tại              │
│                                                │
│ 3. TEST ENVIRONMENT                            │
│    Môi trường để kiểm chứng kết quả           │
│    → Có thể chạy tests                        │
│    → Có thể verify "result is right"          │
└────────────────────────────────────────────────┘
```

---

## 5. Case Study: Crash-Free App Với MCP

**Yêu cầu:** Giữ app không crash

```
Setup của Felix:

  Sentry ──MCP──→ Crash report ──→ Claude Code
                                         │
                              ┌──────────┴──────────┐
                              │  Agent thấy:         │
                              │  - Crash data        │
                              │  - Stack trace       │
                              │  - Context code      │
                              └──────────────────────┘
                                         │
                                    Tự sửa bug
                                         │
                                    Tự verify
                                         │
                              ✓ App crash-free
```

**Quy trình:**

```
1. Felix kết nối Sentry qua MCP → Claude Code thấy crash reports
2. Claude nhìn thấy vấn đề (crash) + context (codebase)
3. Claude tự fix → test môi trường chạy → verify
4. Không cần Felix can thiệp trong loop

→ Đây là Altitude: Felix không hỏi "fix this bug"
   Felix hỏi về outcome: "app không crash"
→ Claude tự làm mọi thứ cần để đạt outcome đó
```

---

## 6. Workflow Thực Tế: Hiring App Pipeline

### 6.1 Vòng Đời Đầy Đủ

```
Step 1: Felix viết prompt Altitude
  "Build a reports dashboard... for our roles to get filled
   faster... flag stuck roles... three most urgent decisions..."
   
Step 2: Claude đọc raw data
  Bash: sed -n 230,282p lib/db.ts && cat app/recruiter/queries.ts
  Bash: ls-files | grep -v node_modules | tail... → 123 files

Step 3: Claude xây dashboard (Altitude applied)
  localhost:3000/reports:
  - "Result: Hire faster — BEHIND" ● (cờ đỏ)
  - Metrics trending charts
  - "DO THIS NEXT: Follow up 3 stalled offers" →

Step 4: Felix viết prompt Depth
  → Claude xây ranking module từ raw records
  → Dùng codex CLI review → Report findings → Fix
  
Step 5: Felix trigger Range (Workflow 6 specialists)
  1h59m sau → Hiring app production-grade
  
Step 6: Felix dùng /be-ambitious SKILL
  → Skill kết hợp cả 3 chiều → workflow tự chạy
  → Felix chỉ cần check kết quả cuối
```

### 6.2 Kết Quả Đo Được

| Metric | Trước | Sau (3 chiều prompt) |
|--------|-------|---------------------|
| Effort gauge | Low | High |
| Feature depth | Surface level | Professional-grade |
| Specialist review | Không có | 6 chuyên gia đa chiều |
| Thời gian build | Nhanh | 1h59m (nhưng chất lượng cao) |
| App quality | ░░░░░░░░░░░░ | ████████████░░ |
| Cần Felix can thiệp | Liên tục | Chỉ ở cuối |

---

## 7. AI Labs Pro — Tài Nguyên Tiếp Theo

```
AI Labs Pro (ailabspro.io):
  ├── Library
  │   ├── The Third Level Actually Separates Your AI Design...
  │   ├── Your Project Will Get Big... Anthropic Is Fixing That (Harness)
  │   ├── Claude Code Just Killed Every Shopify Agency Ever
  │   ├── This One Command Fixed The Biggest Problem Of A...
  │   └── 3 Ways to Build ACTUALLY Beautiful Websites Using Cursor
  │
  └── NEW: Community is open (For AI Labs viewers)
      Video code: V31 (video này)
```

---

## 8. Key Takeaways

```
Triết lý thực hành của Felix:

  "Whatever you'd usually ask for,
   ask for what's beyond it,
   and that's how you start using
   your agents differently."

Áp dụng ngay:
  ❌ Đừng:  "Add a search bar to candidates page"
  ✅ Thay:  "Make sure no qualified candidate gets missed
             because we couldn't find them fast enough.
             Build whatever search capability makes that true."

  ❌ Đừng:  "Fix this crash"
  ✅ Thay:  Connect Sentry via MCP → "Keep the app crash-free"
             → Claude thấy crashes + tự fix loop

  ❌ Đừng:  "Review this code"
  ✅ Thay:  "I want a deeper version. What would a top engineer
             actually do? What raw materials? What tools?
             What proves it's right? Then do that."
```

---

## 9. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

### Notes về Ambitious Prompting & Goal-Setting:
- [Matt Pocock: Kỹ Năng Giá Trị Nhất Thời AI](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/matt_pocock_most_valuable_skill_age_of_ai.md) — Giao tiếp sáng sủa + Domain-Driven Design cho prompts
- [How the Top 1% Use Claude Code Differently](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/how_the_top_1percent_use_claude_code_differently.md) — Brief 3 dòng + Verifiers tất định

### Notes về Workflow & Multi-Specialist Pipeline:
- [Verification is the New Code Review — Ray Amjad](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/verification_is_the_new_code_review_ray_amjad.md) — Verifier thay Code Review, Answer Key
- [Claude Code 3.0 Parallel Desktop](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/claude_code_3_parallel_desktop_stack_delta.md) — Parallel sessions, background session

### Notes về MCP & Tool Integration:
- [Anthropic Managed Agents — Brain Tách Khỏi Hands](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/anthropic_managed_agents_evolution_harness_beamnxw.md) — Tại sao MCP + Session-based architecture quan trọng
- [9 New Claude Mods — Context Meter & Blast Radius](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/9_new_claude_mods_robonuggets.md) — MCP loaded on-demand, context visibility

### Notes về Skills & SKILL.md Pattern:
- [Anthropic Engineers Stopped Building Agents — Sharbel](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/anthropic_engineers_stopped_building_agents_skills_sharbel.md) — Từ agents sang Skills, SKILL.md anatomy
- [Claude Code Mods Ultimate Guide](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/claude_code_mods_ultimate_customization_guide.md) — Customization patterns tương đồng với be-ambitious SKILL

### Notes về Senior Engineer Mindset:
- [How Senior Engineers Build With AI — Intercom Clone](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/how_senior_engineers_build_with_ai_intercom_clone.md) — Senior vs Junior wishful prompting, PRD & Vertical Slices
- [My New AI Setup — 30+ PRs/Day](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/my_new_ai_coding_setup_ship_30_prs_a_day.md) — Multi-model planning + Bot Tech Lead
