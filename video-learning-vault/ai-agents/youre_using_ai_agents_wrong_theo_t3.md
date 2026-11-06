# Bạn Đang Dùng AI Agents Sai Cách | Theo - t3.gg

- **Video URL:** [https://www.youtube.com/watch?v=q1D90-uGvBg](https://www.youtube.com/watch?v=q1D90-uGvBg)
- **Video ID:** `q1D90-uGvBg`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-06
- **Duration:** 49:22
- **Speaker:** Theo (t3.gg) — Founder & CEO, tác giả T3 Code
- **Transcript File:** [youre_using_ai_agents_wrong_theo_t3_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/youre_using_ai_agents_wrong_theo_t3_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

```
┌──────────────────────────────────────────────────────────────────────┐
│          WORKFLOW CỦA THEO: MERGE 50+ PRs/TUẦN KHI ĐI DU LỊCH      │
│                                                                      │
│  Vấn đề: CEO part-time dev, nhiều thứ cần làm ngoài code             │
│  Giải pháp: AI workflow được tối ưu để maximize throughput           │
│                                                                      │
│  STEP 1: New thread + hotkey paste + chạy trên REMOTE machine       │
│          (không block laptop đang làm việc)                         │
│                                                                      │
│  STEP 2: Ignore thread until done                                    │
│          (đừng ngồi nhìn — không productive)                        │
│                                                                      │
│  STEP 3: Multiple threads cùng lúc                                   │
│          ├── Burn tokens để giảm uncertainty                        │
│          ├── Dùng cheap models (53-Flash) cho triage task           │
│          └── 2 threads với models khác nhau để compare              │
│                                                                      │
│  STEP 4: Manage threads như inbox                                    │
│          ├── Settle = done (tự biến mất khi PR merge)               │
│          ├── Snooze = pending review later                          │
│          └── Link PR → thread (auto-settle on merge)                │
│                                                                      │
│  STEP 5: Test trước khi merge                                        │
│          ├── Web: share --dev qua tailscale                         │
│          ├── iOS: Swim app cho remote build                         │
│          └── Desktop: CI label preview:mac → download DMG           │
│                                                                      │
│  STEP 6: Safety nets (không phải guardrails)                        │
│          └── Nightly builds mỗi 3 tiếng → community test           │
└──────────────────────────────────────────────────────────────────────┘
```

Theo (tác giả T3 Code) livestream workflow thực tế trong khi merge code, giải thích từng quyết định về prompt engineering, model selection, và cách ông build infrastructure để ship code với tốc độ cao và confidence. Video không phải tutorial — đây là **window into how T3 Code's creator actually works**.

---

## 2. Bối Cảnh: CEO Part-Time Developer

### 2.1 Vấn Đề Ban Đầu

Theo là founder/CEO của Ping.gg và T3 Code — rất ít thời gian để code. Nhưng contribution graph của ông trong T3 Code repo:
- Từ 1-3 PRs/tuần → 23 → 37 → 41 → **52 PRs/tuần**
- Tuần đạt 52 PRs: ông **đang ở Def Con, đi du lịch!**

### 2.2 Mục Tiêu Video

> *"I finally found flows that allow me to be as productive as I want to coding while also having to run the rest of my life."*

Video này là showcase workflow thực tế, không phải tutorial. Mục tiêu: người xem học được patterns, không nhất thiết phải dùng T3 Code.

---

## 3. Nguyên Tắc Cốt Lõi: Burn Tokens Để Giảm Stress

### 3.1 Triết Lý Chủ Đạo

```
Thay vì tiết kiệm tokens → Burn tokens để:
1. Giảm khả năng xảy ra kết quả tệ
2. Giảm uncertainty trong đầu
3. Giúp maintain motivation
4. Cho phép trust the model và move on
```

> *"You need to do whatever you can to avoid those demotivating moments. One of the best things you can do is reduce the likelihood that the bad outcome happens. One of the easiest ways that you can prevent these failures is burning more tokens."*

### 3.2 Tại Sao Motivation Quan Trọng?

Theo mô tả: nếu put effort → test → code broken → không vui → mất tập trung → mở Twitter → mất cả buổi làm việc.

```
Bad outcome cycle:
Effort → Test → Broken → Demotivated → Twitter/fridge → Lost context
                                          ↑
                                          |
                            Phòng bằng: burn tokens trước
                            để tăng probability of success
```

---

## 4. Workflow Chi Tiết: 6 Steps

### 4.1 Step 1: New Thread + Remote Machine

```
1. New thread (hotkey)
2. Paste task description  
3. QUAN TRỌNG: Select remote machine
   - Không phải laptop đang làm việc
   - Linux box (BB1) ở phòng khác
   - Lý do: agents multi-threaded nặng trên macOS
             Linux: 5% CPU cho 40+ agents
             macOS: struggle với parallelize

Setup của Theo:
- BB1 = Linux box, 32 cores, \$600 16-thread machine cũng ổn
- T3 Code server running trên BB1
- Connect qua T3 Connect từ laptop/phone
```

**Key insight về Linux vs macOS:**
> *"macOS has a ton of problems that make it really hard to reliably parallelize your work. Generally speaking, I treat my Macs as one or two tasks at a time, and I treat my Linux boxes as effectively unlimited tasks at a time."*

### 4.2 Step 2: Ignore Thread Until Done

**Sai lầm phổ biến**: Ngồi nhìn thread chạy để "monitor".

```
❌ Sai: Watch thread → không productive
        Thread không hoàn thành nhanh hơn
        99% cases không cần can thiệp

✅ Đúng: Send thread → làm việc khác
         Thread notify khi done
```

> *"The most important step of landing a lot of code: Ignoring the thread until it's done."*

### 4.3 Step 3: Multiple Threads Concurrently

Thay vì chờ từng thread xong:

```
Scenario trong video:
- Thread 1: Fix skill bug (Claude Code, Extra High)
- Thread 2: Audit PR từ contributor (Claude Code)
- Thread 3: PR triage — 53-Flash (cheap!) 
- Thread 4: PR triage — Luna (compare với Thread 3)
- Thread 5: Investigate disconnect bug (Codex Soul)
```

**Model selection strategy:**
| Model | Dùng cho | Lý do |
|-------|----------|-------|
| Opus 5.5 | Complex tasks, architecture | Thinking ability |
| GPT-6.1 Soul | Exploration, investigation | Reliable for research |
| 53-Flash (GLM) | Triage, cheap tasks | Hilariously cheap, surprisingly capable |
| Luna | Light tasks | Cheap, fast |
| Fable | PR comments, babysitting | Good at ongoing monitoring |

### 4.4 Step 4: Thread Management = Inbox Management

**Settled flow** — Theo tự build feature này vào T3 Code:

```
Thread states:
├── Active    = work cần làm
├── Settled   = xong, biến khỏi sidebar
└── Snoozed   = pending, quay lại sau X giờ

Auto-settle: 
  Link PR → Thread
  PR merged → Thread auto-settled
  → Sidebar luôn chỉ chứa việc CẦN LÀM
```

**Snooze feature:**
```
Nếu không muốn deal với issue ngay:
  → Snooze 3 giờ / đến ngày mai / tuần sau
  → Sidebar sạch, không bị distract
  → Nó sẽ tự quay lại khi cần
```

> *"Generally speaking, the only reason you should go back to an old thread is because you need the context to fix something else, and even then, I don't need it that often."*

### 4.5 Babysit Skill — Vòng Lặp Tự Động Với PR

```
Babysit Skill (custom skill Theo viết):
"Hey, this PR probably has comments from automated reviewers. 
 Keep an eye on it. 
 Take a look every time automated review comments come in.
 Decide if they're worth addressing.
 If they are: make changes, push, keep monitoring.
 Keep going until all review bots have nothing else to say."

Kết quả loop:
Agent: Make changes → PR created
AI Review Bot: 12 comments
Agent: Address all 12 → Push
AI Review Bot: 4 more comments
Agent: Address 4 → Push
AI Review Bot: 1 more comment
Agent: Address 1 → Push
AI Review Bot: Approved ✅
Agent: Mark as done
User: Hit merge (agent đã làm sạch rồi)
```

---

## 5. Prompt Engineering Insights

### 5.1 Prompt Anatomy Của Theo

Phân tích từng câu trong một prompt thực tế:

```
Prompt: "This is an issue that's been affecting a number of users 
         as of recent..."

Lý do:
- "Number of users" → cho model biết đây không phải side project
- Tăng urgency và reliability requirement
- Model sẽ tiếp cận một cách serious hơn
```

```
"I want to make a fix that is both SIMPLE in its implementation 
 and NOT CONFUSING to users..."

Lý do:
- Đây là "no compromise line"
- Nêu rõ priorities trước khi đi vào chi tiết
```

```
"In my opinion, you should be able to..."

Lý do:
- "In my opinion" = signal cho model có thể push back
- Nếu infeasible → model có thể nói không
- Nếu Theo sai → model sẽ correct Theo
```

```
"I'm not sure about the characteristics in Claude code as to 
 how these behaviors are triggered..."

Lý do:
- Nói rõ kiến thức của mình ở đâu
- Model biết cần giải thích gì khi trả lời
- Model không blindly trust assumptions
```

```
"If you see a simple fix, stop and let me know ASAP."

Lý do:
- Cho model permission to stop exploration early
- Không cần explore hết 17 options nếu option 1 đủ tốt
```

### 5.2 "Explain Like I'm 5" Trick

```
Khi model trả lời quá phức tạp / Theo không muốn đọc:

"I really like your description of the problems here. 
 I find your SOLUTION hard to parse, and I can't tell 
 which parts of the problem you're actually solving. 
 Can you give me a simpler, easy to digest, ELI5 description 
 of how you plan to fix these problems?"

→ Không mất progress
→ Không phải tự đọc khó
→ Fight ADHD temptation to tab out
→ Model explains again, cleaner
```

### 5.3 Draft Before Post Pattern

```
Với PR comments / GitHub interactions:

"Help me draft a good comment... 
 Share your simple draft with me here 
 so that I can APPROVE it before the comment is left."

→ Review trước khi post
→ Modify nếu cần
→ Nếu OK: "After you make these changes, leave the comment 
           on my behalf"
```

---

## 6. Testing Infrastructure Theo Tự Build

### 6.1 Web App Testing — Share Dev Via Tailscale

```
Problem: Thread chạy trên BB1 (remote), 
         muốn test web app từ laptop

Giải pháp Theo build vào T3 Code:
Prompt: "I want to test these changes. Can you use the 
         --share-dev command with tailscale so that I can 
         try it out remotely? Respond with a URL that has 
         the pairing code."

→ Agent spin up dev server
→ Agent share via tailscale
→ Trả URL cho Theo
→ Theo mở trên browser/phone → test ngay
```

### 6.2 iOS Testing — Swim App

```
Vấn đề: Không có VPN/Tailscale cho iOS build
Giải pháp: Swim app
- Build iOS app trên remote Mac
- Tạo downloadable link (không cần signed GitHub)
- Click link trên phone → install → test

Limitation:
- Không có granular permissions đầy đủ (AirDrop, Share, v.v.)
- Nhưng đủ dùng cho "quick test on the go"
```

### 6.3 Desktop Testing — CI Preview Label

```
Label PR với: preview:mac
→ CI workflow tự trigger
→ Sau vài phút: macOS DMG download link xuất hiện trong thread
→ Download trên test Mac (clean machine)
→ Install → test

Thêm: Không cần sign in GitHub để download
(Theo tự build feature này vì cần test trên clean machine)
```

---

## 7. Nightly Builds Như Safety Net

**Philosophy**: Safety nets > Guardrails

```
Guardrails = chặn trước khi ship (gây friction, slow down)
Safety nets = catch khi có vấn đề xảy ra

T3 Code approach:
- Nightly builds mỗi 3 tiếng (không phải 24h)
- Vài nghìn nightly users trong 200,000+ user base
- Nếu có vấn đề → ngay lập tức nhận reports
- Nightly users biết trước về rủi ro

Kết quả:
- Stable builds gần như không có regressions
- Vì nightly đã catch hết
```

> *"Safety nets, not guardrails. The fact that a change that breaks things will be called out and noticed before it hits even a thousandth of our users makes it so much easier to hit merge and worry later."*

---

## 8. Key Lessons & Takeaways

### 8.1 Principles Chính

| Principle | Mô tả |
|-----------|-------|
| **Remote execution** | Luôn chạy agents trên dedicated machine, không phải laptop đang dùng |
| **Burn tokens fearlessly** | Tokens rẻ hơn thời gian của bạn và motivation của bạn |
| **Ignore until done** | Đừng baby-sit threads — thời gian đó dành để làm việc khác |
| **Multiple parallel threads** | 5-10 threads song song thay vì 1 thread tuần tự |
| **Treat threads as inbox** | Settle/Snooze/Archive — sidebar = work in progress |
| **Linux for parallelize** | macOS không tốt cho nhiều agents; Linux là "effectively unlimited" |
| **Safety nets over guardrails** | Nightly builds, slow rollouts > blocking reviews |

### 8.2 Friction Identification

> *"If you find where there is friction in your flows — if you pay close attention to what keeps you from merging things, what keeps you from being excited to hit that button — these are all opportunities to improve your project, improve your workflow, improve the tools you use to build."*

**Mỗi lần thấy friction** → build/fix tool để remove friction → throughput exponential growth.

### 8.3 Điều Quan Trọng Nhất

> *"I don't really care if you're using T3 code or not, because the people who are have already seen the difference and are very happy with it. What I really wanted to be was a showcase of how I improve the tools I use by running into problems and also the workflows I have built around T3 code."*

---

## 9. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

### Notes về T3 Code:
- [T3 Code Đã Chinh Phục Jan Marshal](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/t3_code_won_me_over_jan_marshal.md) — Review T3 Code từ góc độ người dùng
- [T3 Code Giải Thích Từ Góc Độ Nhật](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/t3_code_徹底解説_aimy.md) — Intro cho người mới

### Notes về Workflow & Throughput:
- [Live — Poteto, pStack, shipping thousands PRs](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/live_poteto_pstack_shipping_thousands_prs_spacex_matt_pocock.md) — Người ship nhất tháng
- [My New AI Coding Setup — Ship 30 PRs a Day](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/my_new_ai_coding_setup_ship_30_prs_a_day.md) — Setup tương tự
- [High Throughput Agentic Engineering](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/high_throughput_agentic_engineering_kun_chen.md) — Scale agents

### Notes về Safety & Testing:
- [How I Review AI Code — Meta Staff Engineer](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/how_i_review_ai_code_meta_staff_engineer.md) — Code review với AI
- [Fixing the PR Bottleneck](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/fixing_the_pr_bottleneck_matt_pocock_aihero.md) — Giải quyết bottleneck PR

### Notes về Prompt Engineering:
- [How Senior Engineers Build with AI](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/how_senior_engineers_build_with_ai_intercom_clone.md) — Prompts của senior engineers
- [How the Top 1% Use Claude Code Differently](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/how_the_top_1percent_use_claude_code_differently.md) — Patterns của top users

### Notes về Remote & Linux:
- [Herdr: Beginner To Advanced](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/herdr_beginner_to_advanced_lustoykov.md) — Remote terminal management
- [Amazon Infinity AI Agent Runtime](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/amazon_infinity_ai_agent_runtime_rust_lets_get_rusty.md) — Remote agent runtime

