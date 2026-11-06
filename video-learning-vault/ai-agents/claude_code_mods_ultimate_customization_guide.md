# Claude Code Mods: Hướng Dẫn Tùy Biến Toàn Diện

- **Video URL:** https://www.youtube.com/watch?v=8wZVnc99x_k
- **Watch Skill ID:** `a8836baaacbb0049`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 09:53
- **Speaker:** Eric Tech
- **Channel:** Eric Tech

---

## 1. Tóm Tắt Điều Hành

```
┌─────────────────────────────────────────────────────────────────────┐
│              CLAUDE CODE MODS — THAY ĐỔI CẤU TRÚC APP              │
├──────────────────────────┬──────────────────────────────────────────┤
│    Skills & Hooks        │           Mods                          │
│    = Di chuyển nội thất  │           = Phá dỡ / dịch chuyển tường │
│    Same app structure    │           Change the app structure      │
│    (giữ cấu trúc UI)    │           (thay đổi hoàn toàn UI)      │
├──────────────────────────┴──────────────────────────────────────────┤
│  CÁCH TẠO MOD (không cần code):                                     │
│  ① Mô tả điều bạn muốn → ② Claude viết code → ③ Tích hợp vào App  │
├─────────────────────────────────────────────────────────────────────┤
│  MOD CÓ THỂ LÀM GÌ:                                                │
│  • Thêm/thay đổi UI window (panel, progress bar, model router...)  │
│  • Intercept tool calls (hỏi approval trước khi thực thi)         │
│  • Nhớ dữ liệu giữa các phiên (Memory)                            │
│  • Thêm slash commands mới (/aside, /q push...)                    │
└─────────────────────────────────────────────────────────────────────┘
```

**Điểm mấu chốt:** Mods là "nhân viên toàn thời gian" bên trong app — khác với Hooks là "nhà thầu bên ngoài". Chỉ Mods mới có thể thay đổi UI, nhớ dữ liệu, và tạo tương tác đa chiều.

---

## 2. Claude Code Là Gì? (Quick Recap)

Khi dùng Claude Code, có 2 thành phần chính:

```
  Your Computer
       ↓ (request)
  Claude Code App    ← Middleman
       ↓ (redirect)
     Brain            ← AI Model (Opus 5.5, v.v.)
       ↓ (output)
  Claude Code App    ← Render UI + tool calls
```

**Trước khi có Mods**, chỉ có thể tùy biến:
- `CLAUDE.md` / `agent.md` (system prompt)
- Skills (workflows, instructions, SOPs)
- Hooks (triggers chạy scripts bên ngoài)

**Sau khi có Mods**: Thay đổi hoàn toàn UI và hành vi của Claude Code app.

---

## 3. Phép Ẩn Dụ Ngôi Nhà

```
  Claude Code = Ngôi nhà
  ┌─────────────────────────────────────────────────────┐
  │            Skills & Hooks           │      Mods      │
  │          Di chuyển nội thất         │ Phá/dựng tường │
  │         (cùng cấu trúc app)         │  (đổi cấu trúc)│
  └─────────────────────────────────────────────────────┘
```

- **Skills & Hooks**: Như thêm bàn ghế, đổi màu sơn — không thay đổi bố cục phòng
- **Mods**: Như phá tường, xây phòng mới — thay đổi hoàn toàn bố cục

---

## 4. Mods Có Thể Làm Gì?

Mods kiểm soát 2 thứ chính trong Claude Code App:

### 4.1 Window (UI)

Thêm panel UI tùy chỉnh phía trên input box:
- **Cache percentage tracker**: Hiển thị % token tiết kiệm được nhờ cache
- **Model router**: Button click để chuyển model (Haiku/Sonnet/Opus) thay vì gõ `/model`
- **Progress bar**: Theo dõi tiến độ task đang chạy + context window usage
- **Image preview**: Chụp ảnh màn hình → paste vào Claude CLI → hiển thị preview ngay trong UI
- **Task board**: Hiển thị danh sách tasks đang mở

### 4.2 Tool Calls (Intercept)

Mod có thể intercept tool calls TRƯỚC khi thực thi:
```
  Claude → Tool Call → [MOD INTERCEPT] → Hỏi approval? → Thực thi
```

Ví dụ từ video:
```
  Tool timeline:
  ✓ Read package.json          0.2s
  ✗ Bash rm -rf dist           DENIED (mod blocked dangerous rm -rf)
  ✓ Bash trash dist            0.3s  (mod redirected to safer trash)
  ✓ Bash bun test              42 tests ✓
```
Mod có thể deny `rm -rf dist` và suggest dùng `trash dist` thay thế — an toàn hơn!

---

## 5. Mod vs Hook — Sự Khác Biệt Quan Trọng

| Tiêu chí | Hook | Mod |
|----------|------|-----|
| **Vị trí** | Ngoài Claude app (`.claude/hooks/`) | Bên trong Claude app |
| **Phép ẩn dụ** | Nhà thầu bên ngoài | Nhân viên toàn thời gian |
| **Trigger** | Programmatic (script) | UI-aware (có thể render UI) |
| **Memory** | ❌ Không có | ✅ Có thể nhớ dữ liệu |
| **Thay đổi UI** | ❌ Không | ✅ Có thể render/change UI |
| **Interaction** | ❌ Chỉ chạy 1 chiều | ✅ Hỏi approval, pause, v.v. |

```
  Hook:  Trigger → Run script
         (Hiring a contractor is enough)

  Mod:   New interface + Remember data + More interaction
         (Only Mods can do this)
```

---

## 6. Cách Tạo Mod (Không Cần Tự Code)

3 bước đơn giản:
```
  ① Describe what you need
     "Tôi muốn có panel hiển thị model hiện tại và
      context window usage"

  ② Claude writes the code
     Claude tự viết mod code

  ③ Add it to the app
     Mod tự động được tích hợp
```

**Nguồn tham khảo**: Tweet của Thariq (@trq212, kỹ sư Anthropic): "Claude can make mods for you! Just prompt it" — có link đến blog post của Addy Osmani.

---

## 7. Cài Đặt Mod Từ Community

### 7.1 Cập Nhật Claude Trước

```bash
claude update  # Cập nhật lên version mới nhất hỗ trợ Mods
```

### 7.2 Cài Từ Community Marketplace

**Repository chính thức:** `github.com/anthropics/claude-plugins-community`
- Có **50+ collections of mods** từ cộng đồng
- Tìm mod phù hợp → copy link → paste vào Claude: "Please install this mod"

### 7.3 Ví Dụ: Cài Mod "next-steps"

Tác giả: Thariq (kỹ sư Anthropic)

**Chức năng:** Sau mỗi turn, hiển thị tối đa 3 gợi ý prompt phía trên input box:
```
  next:
  1: run the tests you just wrote
  2: do the same for the settings page
  3: /code-review high
  0: dismiss
```

Nhấn 1, 2, 3 → điền vào input box → Enter để thực thi. Tab key lấy gợi ý đầu tiên. Plugin never prompts on its own.

**Cơ chế:** Dùng `function-hooks plugin (hooks/register.tsx)`:
- `turn.complete`: fork session với `$model.fork` để ask next prompts
- `$.command.list`: session's skills and slash commands

---

## 8. Top 6 Mods Được Eric Recommend

Eric dùng Claude để phân tích "second brain" của mình và chọn 6 mods phù hợp:

| # | Mod | Mô tả |
|---|-----|-------|
| 1 | **flight-deck** | Xem task đang chạy, plan hiện tại, context window (Eric bỏ qua vì đã có status bar) |
| 2 | **context-meter** | Đo lường context usage chi tiết |
| 3 | **quota-watcher** | Theo dõi quota / rate limit |
| 4 | **Claude Q** | Queue messages — `/q push "message"` thêm message vào queue trong khi Claude đang làm việc |
| 5 | **secret-reductor** | Ẩn secrets/sensitive values khỏi UI (passwords, API keys, v.v.) |
| 6 | **aside** | Hỏi side question mà không làm bẩn main chat context |

**Đặc biệt về `/aside`:** Claude Code không có tính năng này như ChatGPT. Với `aside` mod, dùng `/aside <câu hỏi>` → câu hỏi không được thêm vào main conversation → có thể đóng lại và tiếp tục chat.

Demo từ video:
```bash
/aside Is the current secret-reductor already active?
# → Claude trả lời trong side window
# → Đóng lại → tiếp tục main conversation bình thường
```

---

## 9. Quản Lý Mods

### 9.1 Nơi Lưu Trữ

```
  ~/.claude/
  └── plugins/
      ├── next-steps/
      ├── token-weather/   (đã xóa trong video)
      └── blast-radius/
```

### 9.2 Các Lệnh Quản Lý

```bash
# Liệt kê tất cả mods đã cài
claude plugin list

# Xóa mod
claude plugin uninstall blast-radius

# Reload mods trong session hiện tại
/reload-plugins
```

---

## 10. Slash Commands Mới Từ Mods

Sau khi cài mods, Eric có thêm các slash commands:
```bash
/reload-plugins    # Reload tất cả plugins/mods
/q push "message"  # Queue message trong khi Claude đang làm việc
/aside "question"  # Hỏi side question không ảnh hưởng main chat
/cache             # Kiểm soát prompt cache
```

---

## 11. EricOS Custom Mod (Mod Tự Làm)

Eric đã tự tạo "EricOS" — custom mod hiển thị trong CLI với các tính năng:

```
  EricOS (Claude Code v2.1.292, Opus 5.5 1M context)
  ├─ Model switching        → Click để đổi model
  ├─ Task tracking          → Xem tasks đang chạy
  ├─ Skill recap/recalibrate → Xem lại và cải thiện skills
  └─ Retro sections         → Phân tích conversation → cải thiện skills
```

Eric đã đóng gói tất cả mods tự tạo thành PDF có thể tải trong school community.

---

## 12. Key Takeaways

1. **Mods = "di chuyển tường"** — thay đổi cấu trúc app, không chỉ nội dung
2. **Không cần code** — mô tả → Claude tạo mod cho bạn
3. **Phân biệt Hook vs Mod**: Hook là contractor (ngoài app), Mod là nhân viên (trong app) — chỉ Mod mới có UI + Memory + Interaction
4. **50+ community mods** tại `github.com/anthropics/claude-plugins-community`
5. **Top picks**: next-steps, aside, secret-reductor, Claude Q, quota-watcher
6. **Mods lưu tại** `~/.claude/plugins/` — manage qua `claude plugin list/uninstall`
7. **Reload trong session**: `/reload-plugins`

---

## Liên Kết Đến Các Note Liên Quan

- [[claude_code_getting_rid_of_subagents_aicodeking]] — Khi nào fold subagents (AICodeKing)
- [[s_tier_claude_code_mods_ray_amjad]] — S-tier Claude Code mods từ Ray Amjad
- [[anthropic_engineers_use_claude_code_ai_labs]] — Anthropic engineers dùng Claude Code thế nào
- [[9_new_claude_mods_robonuggets]] — 9 Claude mods mới từ Robonuggets
