# DeepSeek Đang Tái Định Nghĩa Harness! — 7 Prompts Creator Mode & Công Thức 5 Phần

- **Video URL:** https://www.youtube.com/watch?v=XNf8bOaHVTo
- **Watch Skill ID:** `78b21d36f7e9f4ef`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 09:11
- **Speaker:** AICodeKing
- **Channel:** AICodeKing

---

## 1. Tóm Tắt Điều Hành

```
┌─────────────────────────────────────────────────────────────────────┐
│          DeepSeek Harness Creator Mode — 7 Prompts Chính Thức       │
│          "Everything is a plugin" | 269,000+ stars on GitHub (MIT) │
├─────────────────────────────────────────────────────────────────────┤
│  CÔNG THỨC PROMPT 5 PHẦN:                                           │
│  1. Nơi plugin sống (left sidebar / right sidebar / conversation)   │
│  2. Chức năng cụ thể                                                │
│  3. Số lượng & giá trị mặc định (con số, không phải "một vài")     │
│  4. Những gì không muốn (ràng buộc loại trừ)                       │
│  5. Kết nối hai chiều với chat                                       │
├─────────────────────────────────────────────────────────────────────┤
│  STACK KHÔNG ĐỐT API CREDIT:                                        │
│  DSH Creator Mode ──▶ ClinePass ($9.99/tháng, không giới hạn turns)│
│  Regular work      ──▶ Cline Desktop (cùng subscription)            │
│  Parallel all      ──▶ PandaCode (DSH như 1 harness, ACP protocol) │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 2. Bối Cảnh: DeepSeek Harness Là Gì?

- **Repo:** MIT license, 269,000+ stars trên GitHub
- **Tagline:** "Everything is a plugin"
- **Latest build:** October 30th version
- **2 chế độ chính:**
  - **Standard mode:** Agent code bình thường
  - **Creator mode:** Thêm 2 read-only runtime introspection tools — agent nhìn vào bên trong app đang chạy TRƯỚC khi viết gì

**Creator mode cơ chế:**
```
  Agent đọc runtime → list what is loaded → check methods & types
      ↓
  Viết plugin code
      ↓
  Package thành bundle → Install qua Plugin Manager
      ↓
  Plugin thuộc profile → Survive restart (bền vững qua các lần khởi động lại)
```

**Cảnh báo quan trọng từ DeepSeek:** "Some plugins were refined over several rounds of conversation — a single prompt may not get you exactly what you saw in the video." Đây là **starting points, not magic spells**.

---

## 3. Phân Tích 7 Prompts Chính Thức — 7 Bài Học

### Prompt 1: 3D Gallery of Famous Landmarks
**Chủ đề:** 3D models của Eiffel Tower, Great Wall, Forbidden City + 1 toà nhà do model chọn  
**Plugin location:** Left sidebar entry

> **Lesson 1: Luôn nói plugin sống ở đâu (say where it lives)**

Thống kê từ 7 prompts:
- 3 prompts: **left sidebar**
- 2 prompts: **opened from conversation**
- 1 prompt: **right sidebar**
- 1 prompt: không nói → model phải tự đoán (thường ra sai)

"One more famous building of the model's choice" — để model có không gian sáng tạo thay vì phải đoán tất cả.

---

### Prompt 2: Transformer 3D Visualization
**Chủ đề:** Interactive 3D game giải thích computation bên trong transformer  
**Dòng quan trọng:** "Keep the panel title and toolbar clear of the native macOS window buttons so they don't overlap."

> **Lesson 2: Khi thứ gì đó trông bị vỡ, đừng bắt đầu lại — đặt tên chính xác vấn đề trong message tiếp theo và đưa vào prompt lần sau**

Dòng "clear of macOS window buttons" rõ ràng là được thêm vào SAU KHI lần đầu làm bị overlap.

---

### Prompt 3: DSH Pixelated Interface (Yêu thích của AICodeKing)
**Chủ đề:** Plugin thay đổi TOÀN BỘ giao diện app với 5 styles khác nhau (pixelated là một trong số đó)

> **Lesson 3: Cho nó một con số — "5 styles", không phải "some styles". Con số dễ kiểm tra hơn tính từ.**

"This is not a panel inside the app. This is **the app changing itself**." — Đây chính là "Everything is a plugin" trong 1 prompt.

---

### Prompt 4: Poster Design Workbench
**Chủ đề:** Workbench thiết kế poster, mở từ conversation, dùng shader background + English font từ DSH website  
**Dòng quan trọng:** "Keep the default page minimal with no unnecessary subheadings or information. Set the default title to 'Everything is a plugin'."

> **Lesson 4: Nói những gì KHÔNG muốn, và cho giá trị mặc định. Models thích thêm filler — prompt này tắt điều đó.**

---

### Prompt 5: Video Editing Workbench
**Chủ đề:** Dark interface kiểu video editor thực thụ — multiple video uploads, editing, exporting, large preview on top, timeline below  
**Đặc điểm:** Import và edit videos bằng natural language requests trong conversation

> **Lesson 5: Plugin tốt hơn khi agent cũng có thể điều khiển nó. Mô tả layout như wireframe (top và bottom), và kết nối plugin với chat.**

---

### Prompt 6: Paper Reader (Right Sidebar)
**Chủ đề:** Upload papers, giữ nguyên layout/fonts/images, text selection → floating menu 3 options: highlight / add note / ask in conversation  
**Dòng quan trọng:** "Default question prompt phải match interface language."

> **Lesson 6: Mô tả chi tiết interaction từng bước. Dòng "match interface language" chỉ xuất hiện ở người đã thực sự dùng qua tool rồi.**

---

### Prompt 7: Conversation Statistics
**Chủ đề:** Dashboard built on ACTUAL conversation data — total token usage, conversation count, estimated costs, budget, token timeline, usage & cost per conversation, CPU activity  
**Lưu ý:** Prompt này KHÔNG nói plugin sống ở đâu → model phải tự đoán

> **Lesson 7: Nói "real data" hoặc bạn sẽ nhận được dashboard đẹp đầy số giả.**

---

## 4. Tổng Hợp Công Thức 5 Phần

```
  Prompt Creator Mode tốt gồm 5 phần:

  ① Where it lives        → Left sidebar / right sidebar / conversation
  ② What it does          → Chức năng cụ thể, rõ ràng
  ③ Numbers & defaults    → "5 styles", default title = "...", v.v.
  ④ What to leave out     → "No unnecessary subheadings", "keep minimal"
  ⑤ Connect to the chat   → Plugin có thể nhận natural language từ conversation
```

---

## 5. Vấn Đề Chi Phí & Giải Pháp Stack

### Vấn Đề
Creator mode đốt rất nhiều tokens vì:
- Đọc runtime → viết plugin → install → iterate nhiều rounds
- Nếu trả per-token → tốn tiền rất nhanh

### Giải Pháp: Stack Không Đốt API Credit

```
  ┌────────────────────────────────────────────────────────────────┐
  │                    ClinePass ($9.99/tháng)                      │
  │  • 2-5x more usage than paying directly                        │
  │  • API key (không khóa vào Cline)                              │
  │  • OpenAI format → slot vào DSH Custom Model API               │
  ├─────────────────────────┬──────────────────────────────────────┤
  │  DSH Creator Mode       │  Cline Desktop                      │
  │  the app builds itself  │  the regular work                   │
  │  Dùng ClinePass:        │  Cũng ClinePass subscription!       │
  │  • Kimi K3.1 Pro:       │  • Schedule (name, freq, prompt,    │
  │    bigger plugins       │    model, workspace → runs for you) │
  │  • Flash models:        │  • Import từ Claude Code / Codex   │
  │    small fixes          │    → continue on ClinePass model    │
  └─────────────────────────┴──────────────────────────────────────┘
```

**Cài ClinePass vào DSH:**
1. Settings → Models → Add model provider
2. Pick: Custom model API
3. Put in ClinePass base URL
4. Protocol: OpenAI chat completions
5. Paste API key → Fetch available models

---

## 6. PandaCode: Chạy Tất Cả Song Song

```
  PandaCode runs DSH as one of its harnesses:
  
  npm install dsh  → PandaCode launches it over ACP
  → lists its models → set reasoning effort

  Ví dụ kiến trúc:
  Architect (Claude Code) ─── holds whole task
      ├─ Worker 1 → DeepSeek Harness (DSH package via NPM, ACP)
      └─ Worker 2 → Cline on ClinePass model

  Muốn build cả 7 plugins song song?
  → Hand them out in parallel → review in one place
```

---

## 7. Limitations (Những Điều Cần Biết)

| # | Limitation | Chi tiết |
|---|------------|----------|
| 1 | **Developer preview** | DSH ship release candidates rất thường xuyên → things can change between builds |
| 2 | **Single prompt ≠ video result** | Plugins được refined qua nhiều rounds — cần iterate |
| 3 | **Whole-app skins có thể break UI** | Pixel style plugin có thể làm vỡ giao diện → biết cách tắt qua Plugin Manager |
| 4 | **ClinePass không unlimited** | Kimi K3.1 Pro chạy Creator Mode cả ngày → hit limits (5-hour window, weekly, monthly) |
| 5 | **Cline Desktop vẫn beta** | Expect some rough edges |

---

## 8. Key Takeaways

1. **DeepSeek công bố 7 prompts chính thức** của Creator Mode — không phải magic spells, chỉ là starting points
2. **Công thức 5 phần**: where it lives + what it does + numbers/defaults + what to leave out + connect to chat
3. **"Everything is a plugin"**: Plugin có thể thay đổi TOÀN BỘ giao diện app (Prompt 3 — pixel art skin)
4. **Stack tiết kiệm**: ClinePass \$9.99/tháng covers cả DSH Creator Mode và Cline Desktop
5. **PandaCode**: Chạy DSH như 1 harness qua ACP → architect + workers → build 7 plugins song song
6. **Creator mode cơ chế**: Runtime introspection (read-only tools) → viết plugin → install → survive restart
7. **Lesson 7**: Luôn yêu cầu "real data" — không có thì nhận được dashboard đầy số giả

---

## Liên Kết Đến Các Note Liên Quan

- [[deepseek_open_sources_super_agent_dsh_tekthrill]] — DSH superagent overview từ TEKTHRILL
- [[deepseek_superagent_harness_hands_on_aicodeking]] — Hands-on AICodeKing với DSH superagent
- [[new_deepseek_harness_super_agent_cordis_sam_witteveen]] — Cordis framework + DSH (Sam Witteveen)
- [[claude_code_mods_ultimate_customization_guide]] — So sánh: Mods trong Claude Code vs Plugins trong DSH
- [[tiny_local_model_outperforms_opus_orchestration_aicodeking]] — PandaCode orchestration patterns
