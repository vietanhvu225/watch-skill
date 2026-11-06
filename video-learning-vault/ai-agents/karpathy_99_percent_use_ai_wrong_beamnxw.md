# Andrej Karpathy: "99% Dùng AI Sai Cách" — Hướng Dẫn Thực Chiến Từ Người Tạo Ra GPT-2 | beamnxw

- **Video URL:** https://www.youtube.com/watch?v=7YEHs2SMaVQ
- **Watch Skill ID:** `5835ce1329f51f52`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 2:05:00
- **Speaker:** Andrej Karpathy (người sáng tạo GPT-2, cựu Tesla AI, đồng sáng lập OpenAI)
- **Channel:** hyperfixation on AI | beamnxw

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

```
┌──────────────────────────────────────────────────────────────────────────┐
│   ANDREJ KARPATHY: HƯỚNG DẪN THỰC CHIẾN DÙNG LLM (2:05:00)              │
│   "99% người dùng AI sai — đây là cách Karpathy dùng"                   │
│                                                                          │
│  MENTAL MODEL CỐT LÕI:                                                   │
│  LLM = "1TB lossy probabilistic zip file of the internet"               │
│  → Params = kiến thức nén từ internet (pre-training)                    │
│  → Context window = working memory (tokens = tài nguyên quý)            │
│  → Post-training = "gắn khuôn mặt vui vẻ" (SFT + RLHF)               │
│  → Knowledge cutoff: mô hình không biết gì sau ngày đó                  │
│                                                                          │
│  7 LAYER TOOLS KARPATHY DÙNG:                                            │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │ 1. Base Chat (ChatGPT/Claude/Gemini/Grok/Perplexity)              │  │
│  │ 2. Thinking Models (O1/O3/DeepSeek R1) → math/code/reasoning      │  │
│  │ 3. Internet Search → fresh info, esoteric topics                   │  │
│  │ 4. Deep Research → search + thinking kết hợp (~10 phút/query)     │  │
│  │ 5. File Upload + PDF → đọc sách/papers cùng LLM                   │  │
│  │ 6. Advanced Data Analysis → Python interpreter + figures           │  │
│  │ 7. Claude Artifacts → React app/Mermaid diagram live               │  │
│  └────────────────────────────────────────────────────────────────────┘  │
│                                                                          │
│  MULTIMODALITY:                                                           │
│  Audio input: SuperWhisper/WispFlow (F5 key) → 50% queries by voice     │
│  Advanced Voice: real audio-in-audio (natively in LLM, not pipeline)    │
│  Image input: screenshot + paste → toothpaste, blood test, meme         │
│  Video: DALL-E 3, Veo-2, Sora, Hunyuan, Pika 2.1, Runway Gen-3         │
│                                                                          │
│  QUALITY OF LIFE:                                                         │
│  Memory → database về bạn, prepend vào mọi conversation                 │
│  Custom Instructions → "đừng như HR, cứ nói thẳng"                     │
│  Custom GPTs → Korean vocab extractor, Korean cap OCR+translator        │
│  Cursor/Composer → vibe coding với Claude 3.7 Sonnet                    │
│                                                                          │
│  TIP THỰC CHIẾN:                                                          │
│  → Bắt đầu chat mới khi chuyển topic                                    │
│  → Dùng "LLM Council" → hỏi nhiều model cùng lúc                       │
│  → Deep Research = first draft, KHÔNG phải sự thật                      │
│  → Thinking model → khi standard model thất bại                         │
└──────────────────────────────────────────────────────────────────────────┘
```

Đây là bài giảng 2 giờ toàn diện nhất về cách dùng LLM trong thực tế, từ góc nhìn của Andrej Karpathy — người tạo ra GPT-2, cựu giám đốc AI Tesla, và đồng sáng lập OpenAI. Video được beamnxw tổng hợp và trình bày lại với cấu trúc rõ ràng. Karpathy trình bày từ mental model cơ bản nhất đến các tool nâng cao, với hàng chục ví dụ thực tế từ công việc hàng ngày của ông.

---

## 2. Mental Model Cốt Lõi: LLM = 1TB Zip File

### 2.1 Phép Ẩn Dụ Zip File

Karpathy mở đầu bằng metaphor trung tâm của toàn bài:

```
Internet (petabytes) → Pre-training (gradient descent) → 1TB params
                                                          ↓
                                      "1TB lossy probabilistic zip file"
                                      - Lossy: không lưu chính xác
                                      - Probabilistic: trả về probable text
                                      - Zip file: nén kiến thức thành params
```

**Ví dụ Karpathy hay dùng:** "Hi, I'm ChatGPT. I am a 1TB zip file. My knowledge comes from the internet roughly 6 months ago."

### 2.2 Pre-training vs Post-training

| Phase | Mục Đích | Kết Quả |
|-------|----------|---------|
| **Pre-training** | Nén internet vào params bằng next-token prediction | 1TB "zip file" kiến thức |
| **Post-training** | SFT + RLHF → uốn nắn behavior | "Gắn khuôn mặt vui vẻ lên zip file" |

> Karpathy dùng câu nói hài hước: Post-training chỉ là "attaching a smiley face" lên zip file — kiến thức đã có sẵn từ pre-training, post-training chỉ làm nó có personality.

### 2.3 Context Window = Working Memory

- **Context window** = tất cả tokens trong cuộc hội thoại hiện tại
- **Tokens** = tài nguyên quý — không lãng phí
- **Thực hành của Karpathy:** Bắt đầu chat mới khi chuyển topic (clean slate)
- **Knowledge cutoff:** Model không biết gì sau ngày training xong → cần internet search cho fresh info

---

## 3. Thinking Models: Khi Nào Dùng?

### 3.1 Cơ Chế

Thinking models (O1, O3, DeepSeek R1, Claude 3.7 Extended) được train bằng **Reinforcement Learning** — model tự khám phá "inner monologue" strategies:
- Tự đặt câu hỏi → tự phản bác → tự kiểm tra
- Không phải người design strategy — RL tự tìm ra
- Hiển thị raw thoughts (DeepSeek R1 trên Perplexity cho xem chain-of-thought)

### 3.2 Case Study Thực Tế: Gradient Check Bug

```
Vấn đề: Bug trong code tính gradient check (numerical vs analytical)
        → Chênh lệch nhỏ bất thường

GPT-4o (standard): ❌ Thất bại — không tìm ra nguyên nhân
O1 Pro:            ✅ Giải quyết trong 1 phút — phát hiện lỗi precision
Claude 3.5 Sonnet: ✅ Cũng giải quyết — không cần thinking mode
```

### 3.3 Khi Nào KHÔNG Cần Thinking Model?

- Travel advice → không cần
- Câu hỏi thông thường → không cần
- Chỉ cần khi: math phức tạp, code khó, reasoning logic sâu

### 3.4 Câu Hỏi Toán Trick (Frame OCR — frame_0027)

```
Toán câu hỏi trick Karpathy tìm trong paper:
(x²-1)(x+1) / (x³-x) - 1/x evaluated at x = π

→ Đây là câu hỏi TRICK — biểu thức đơn giản hóa thành 0
   nhưng nhiều model (kể cả GPT-4o) bị nhầm
→ O1 giải đúng
→ Dùng để test thinking model vs standard model
```

---

## 4. Internet Search & Deep Research

### 4.1 Khi Nào Cần Internet Search?

Karpathy đưa ra 2 tiêu chí:
1. **Esoteric**: thông tin ít xuất hiện trên internet → không chắc trong zip file
2. **Time-varying**: thông tin thay đổi theo thời gian → model có thể lỗi thời

**Ví dụ Karpathy hay dùng:**
- Bryan Johnson dùng kem đánh răng nào gần đây? (esoteric + time-varying)
- Vietnam có an toàn để du lịch không? (time-varying)
- USAID là chuyện gì? (trending topic)
- White Lotus season 3 có gì? (film thông tin mới)

### 4.2 Deep Research: Search + Thinking = Research Paper

**Deep Research** (ChatGPT Pro — $200/tháng) = internet search + thinking, chạy ~10 phút:

```
Input: "CAKG trong longevity mix của Bryan Johnson — nghiên cứu?"
       → 27 sources, 5 phút thinking
       → Output: gần như custom research paper với citations

Comparison (Karpathy):
- ChatGPT Deep Research: TỐT NHẤT — thorough, dài, có sense
- Perplexity Deep Research: Khá tốt nhưng ngắn hơn
- Grok Deep Search: Tương tự nhưng ngắn hơn

⚠️ CẢNH BÁO: Deep Research = "first draft" — KHÔNG phải sự thật
   Citations quan trọng → phải vào đọc paper gốc
   Ví dụ thất bại: Table LLM labs USA → thiếu xAI, có Hugging Face (sai)
```

---

## 5. File Upload & Document Reading

### 5.1 Đọc Papers + Books Cùng LLM

Karpathy không còn đọc sách một mình — **luôn có LLM cùng đọc**:

```
Workflow đọc sách:
1. Tìm nội dung (Project Gutenberg cho sách cũ)
2. Copy-paste chapter vào Claude/ChatGPT
3. Hỏi summary → đọc chapter → hỏi clarifying questions
4. Claude tạo conceptual Mermaid diagram của chapter → spatial understanding

Ví dụ: The Wealth of Nations (Adam Smith, 1776)
- Chapter 3: Division of Labor limited by extent of market
- Claude tạo Mermaid diagram: Land transport vs Water transport
  → Chứng minh water transport → specialization → wealth creation
```

**Với papers khoa học:**
- Upload PDF (30MB Evo2 paper từ Ark Institute)
- Hỏi summary → đọc abstract → hỏi từng phần
- Claude 3.7 vừa ra khi quay video → "just released Claude 3.7" (có thinking mode Extended)

### 5.2 Toán Học & Code: Advanced Data Analysis

Python interpreter tool trong ChatGPT → **LLM viết code và chạy**:
- Plot figures, phân tích data
- NMR LipoProfile (blood test) → vẽ charts
- Karpathy: "Như có junior analyst — nhưng absent-minded, phải kiểm tra cẩn thận"
- Nhiều video chuyên sâu về Advanced Data Analysis → xem thêm nếu cần

---

## 6. Claude Artifacts: App Tức Thì

### 6.1 Adam Smith Flashcard App (Demo Thực Tế)

```
Step 1: "Generate 20 flash cards from this text [Adam Smith Wikipedia intro]"
         → 20 Q&A cards

Step 2: "Now use artifacts to write a flash cards app to test these"
         → Claude viết React component với Q&A hardcoded
         → Interface với "Show Answer" + "Correct/Wrong" + "Shuffle"
         → Deploy trực tiếp trong browser

Paradigm shift:
- Trước: Engineer viết app → user customize
- Giờ:  Claude viết app custom NGAY cho bạn → không cần backend/database
```

### 6.2 Conceptual Diagram (Use Case Karpathy Dùng Nhất)

```
Prompt: "Create a conceptual diagram of this chapter"
→ Claude viết Mermaid code → render diagram ngay trong browser
→ Spatial understanding của argument structure
→ Dùng cho books, source code, bất cứ thứ gì
```

### 6.3 Karpathy's Excalidraw Full LLM Map (frame_0021, frame_0022)

Karpathy tạo full architecture diagram trên excalidraw.com:

```
Pre-training           Post-training          Tool Use
┌────────────┐         ┌────────────┐         ┌───────────────────────┐
│ Internet   │ →grad→  │   SFT +    │ →tools→ │ Internet Search      │
│ (petabytes)│         │   RLHF     │         │ Python Interpreter    │
│ next-token │         │ "smiley    │         │ (ChatGPT ADA/Artifacts│
│ prediction │         │  face"     │         │ /Cursor Composer)     │
└────────────┘         └────────────┘         └───────────────────────┘
      ↓                      ↓                        ↓
   1TB params            Behavior              Context Window Extended
   
Modalities:
  Audio input: SuperWhisper, WispFlow → Speech-to-text
  Audio output: Text-to-speech (read aloud button)
  Advanced Voice: Native audio (GPT-4o omni)
  Image input: Screenshot paste → OCR+understand
  Video input: Advanced Voice mobile → camera feed
  Image output: DALL-E 3, ideogram.ai
  Video output: Veo-2, Sora, Hunyuan, Pika, Runway, Hailuo
```

---

## 7. Voice & Multimodality

### 7.1 Karpathy Dùng Voice 50% Thời Gian

```
Desktop: SuperWhisper (F5 key) → record → transcribe → paste vào ChatGPT
Mobile:  Microphone icon trong app → 80% queries by voice (phone = lazy)

Hạn chế: Tên kỹ thuật (library names) → gõ tay
         Thông thường → voice rất tốt

"Fake audio" vs "True audio":
- Fake: speech-to-text → text token → LLM → text → text-to-speech
- True: Audio tokens natively trong LLM (GPT-4o omni, Advanced Voice)
```

### 7.2 Advanced Voice Mode Demo (frame_0003 trở đi)

Karpathy demo Advanced Voice trên mobile:
- Camera feed → model nhìn thấy environment real-time
- Nhận ra: acoustic foam panels, Genghis Khan book, Feynman book, Aranet4 CO2 monitor (713 ppm = OK), Middle-earth map
- **Note**: Under the hood có thể vẫn là frames/second, không phải true video stream

### 7.3 Image Input Use Cases

| Use Case | Phương Pháp |
|---------|------------|
| Blood test NMR LipoProfile (20-page PDF) | Screenshot từng panel → paste → interpret |
| Toothpaste ingredients | Photo → OCR → "which is essential vs unnecessary?" |
| Meme giải thích | Screenshot → "explain" → murder of crows joke |
| Korean cap/subtitle | Screenshot → Korean cap GPT → OCR + translate + breakdown |
| Korean sentence analysis | Paste → Korean vocabulary extractor GPT → Anki format |

### 7.4 Video Generation Comparison (frame_0043)

Karpathy so sánh 7 models tạo "tiger in jungle":

| Model | Nhà cung cấp | Nhận xét Karpathy |
|-------|-------------|-------------------|
| **Veo-2** | Google | Near state of the art, really good |
| **Sora** | OpenAI | Khá tốt |
| **Hunyuan Video** | Tencent | - |
| **Pika 2.1** | Pika Labs | - |
| **Wanx 2.1** | Alibaba Cloud | - |
| **Runway Gen-3** | Runway | - |
| **Hailuo T2V-01** | MiniMax | - |

→ Veo-2 dẫn đầu tại thời điểm quay video

---

## 8. Personalization & Quality of Life

### 8.1 Memory Feature (ChatGPT-only tại thời điểm quay)

```
Cách hoạt động:
1. Nói "Can you please remember this?" hoặc tự động trigger
2. ChatGPT lưu text summary vào memory database
3. Memory database được prepend vào MỌI conversation

Ví dụ: "Believes 1990s-2000s was peak Hollywood"
Karpathy: "ChatGPT dần dần hiểu tôi hơn theo thời gian"
⚠️ Memories quá personal → không show trong video
```

### 8.2 Custom Instructions (Karpathy's Settings)

```
Traits: "Just don't be like an HR business partner. Talk to me normally.
         I love explanations, education, insights. Be educational whenever you can."

Identity: Andrej Karpathy, AI researcher background

Korean preference: Formal polite tone (합쇼체) for Korean responses
                   → Tránh quá informal hoặc quá formal
```

### 8.3 Custom GPTs (Karpathy's Favorites)

**Korean Vocabulary Extractor:**
```
Input: Korean sentence
Output: 한국어;English (semicolon-separated → import vào Anki)
Technique: Few-shot prompting với 4 concrete examples
```

**Korean Detailed Translator (tốt hơn Google Translate/Naver/Papago):**
```
Input: Korean sentence
Output: Full translation + word-by-word breakdown
        (particle by particle với context)
Technique: Few-shot prompt với XML-like tags cho examples
```

**Korean Cap OCR + Translator:**
```
Input: Screenshot từ Korean TV show (Single's Inferno)
Output: OCR → Translation → Word breakdown
        "Na Jin-young và Seulki nói gì trong cảnh này?"
```

---

## 9. Karpathy's LLM Council & Workflow

### 9.1 LLM Council

Thay vì dùng 1 model, Karpathy hỏi nhiều model cùng lúc cho câu hỏi quan trọng:

```
LLM Council:
├── ChatGPT (GPT-4o / O1 Pro)  — most feature-rich, default
├── Claude (3.5 Sonnet / 3.7)  — artifacts, diagrams, coding
├── Gemini                      — Google integration
├── Grok                        — ít censored hơn, "cagey" ít hơn
└── Perplexity                  — search-first, pioneer internet search
```

### 9.2 Karpathy's YouTube Channel (frame_0036, frame_0039)

Karpathy show channel của mình:
- **"Deep Dive into LLMs like ChatGPT"** — 1.2M views (phổ biến nhất)
- **"Let's Reproduce GPT-2 (124M)"** — hands-on training
- **"LLM Tokenization"** — tokenization deep dive
- **"Intro to Large Language Models"** — introductory

### 9.3 Cursor/Composer: Vibe Coding

```
Karpathy's coding workflow:
1. KHÔNG dùng ChatGPT web để code → không có file context
2. Dùng Cursor (Claude 3.7 Sonnet under the hood)
3. Command I → Composer → autonomous agent trên codebase

Demo Tic-Tac-Toe → Confetti khi thắng:
- Cursor tự cài react-confetti library
- Tự download victory.mp3 từ somewhere
- Kết quả: confetti + sound khi thắng (Karpathy surprised it worked!)

Karpathy: "Vibe coding" — term ông tự đặt
           = letting composer take control, just tell it what to do
           = worst case: fall back to standard programming
```

---

## 10. Healthy Skepticism

### 10.1 Các Cảnh Báo Quan Trọng

```
⚠️ Deep Research: "First draft only" — citations quan trọng để verify
⚠️ Hallucination: Vẫn xảy ra kể cả với citations
⚠️ Advanced Data Analysis: "Junior analyst — absent-minded"
⚠️ Table accuracy: LLM labs USA table → thiếu xAI, bao gồm sai entries
⚠️ Image trong PDF: Có thể bị discard hoặc không hiểu tốt
⚠️ Under the hood: Video stream thực ra là frames/second
```

### 10.2 Summary Final Karpathy (1:24:52)

```
Checklist sử dụng LLM hiệu quả:
1. Nhớ bạn đang nói chuyện với ZIP FILE
2. Biết pricing tier → biết model nào đang dùng
3. Thinking model → math/code/reasoning (try standard trước)
4. Internet search → fresh info / esoteric topics
5. File upload → đọc paper/book cùng LLM
6. Advanced Data Analysis → Python + figures
7. Claude Artifacts → web app / diagrams
8. Cursor/Composer → professional coding (vibe coding)
9. Voice → 50% queries (SuperWhisper desktop, mic mobile)
10. Multimodal → image paste, advanced voice camera
11. Quality of life → memory, custom instructions, custom GPTs
12. Mobile vs web → features khác nhau, biết cái nào có cái nào
```

---

## 11. Liên Kết Chéo

### Khái Niệm Liên Quan Trong Vault

- **Watch Skill ID `b159c5cb00750dd9`** (`anthropic_engineers_use_claude_code_ai_labs.md`) — Felix Rieseberg dùng Claude Code theo 3 chiều Altitude/Depth/Range; Karpathy dùng Cursor/Composer = agentic coding từ góc độ người dùng
- **Watch Skill ID `ec8ec278c38667ce`** (`claude_code_agentic_workflows_lydia_hallie.md`) — Lydia Hallie Anthropic: Artifacts, CLAUDE.md hierarchy, Hooks — cùng ecosystem Claude mà Karpathy reference
- **Watch Skill ID `df84a40038087d90`** (`claude_code_low_cost_100_agents_lydia_hallie.md`) — Prompt cache prefix = cách tối ưu context window mà Karpathy mô tả là "tokens = tài nguyên quý"
- **Watch Skill ID `fa6483c46d4119a1`** (`art_of_loop_engineering.md`) — Agent Loop; Karpathy mô tả Cursor/Composer như "autonomous agent on your codebase"
- **Watch Skill ID `48de73e19c423d59`** (`10_levels_of_jev_for_agentic_engineers_indydevdan.md`) — Jev decision harness; Karpathy's "LLM Council" = multi-model voting tương tự Jev parallel sampling

### Insight Độc Đáo

> **"LLM là zip file"** — đây là mental model mạnh nhất để hiểu tại sao LLM có knowledge cutoff, tại sao cần internet search, tại sao context window là working memory. Karpathy — người xây dựng các hệ thống này từ đầu — xác nhận đây là cách chuẩn để nghĩ về LLM.

> **"Vibe coding"** — term do Karpathy đặt ra, mô tả workflow "nhường quyền cho Composer, chỉ cho lệnh, hy vọng nó chạy". Worst case: fall back to standard programming.

> **Deep Research = "first draft"** — cảnh báo quan trọng: thậm chí với citations, Deep Research vẫn có thể hallucinate. Treat như starting point, không phải truth.
