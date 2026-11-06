# Agent Desktop — Computer Use Không Cần Screenshot, Nhanh Hơn 40x — Prism Labs

- **Video URL:** https://www.youtube.com/watch?v=iEhCLn9utDU
- **Watch Skill ID:** `55fcaceb3822812f`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-07
- **Duration:** 07:16
- **Speaker:** Prism Labs (channel narration / analysis)
- **Channel:** Prism Labs

---

## 1. Tóm Tắt Điều Hành

```
┌────────────────────────────────────────────────────────────────────────────┐
│   AGENT DESKTOP: COMPUTER USE KHÔNG CẦN SCREENSHOT                         │
│   (by Vercel Labs — phân tích bởi Prism Labs)                              │
│                                                                            │
│  VẤN ĐỀ: Screenshot-based computer use = chậm                             │
│    → 3-8 giây MỖI LẦN agent nhìn vào màn hình                             │
│    → Model phải nhìn ảnh + suy luận tọa độ → click                        │
│    → Tọa độ bị drift, pixel-match thất bại                                 │
│                                                                            │
│  GIẢI PHÁP: Agent Desktop — hỏi OS thay vì chụp ảnh                       │
│    → Dùng Accessibility API (cùng layer screen readers dùng)               │
│    → Nhận JSON với ref cho mỗi phần tử UI                                  │
│    → Observe → Act → Observe (không pixel nào)                             │
│                                                                            │
│  HIỆU NĂNG:                                                                │
│  ┌────────────────────────────────────────────────────────────────┐        │
│  │  Tree snapshot:   120ms (thường)  │  21ms qua native binary   │        │
│  │  Vision best:     3,000ms          │  3s                       │        │
│  │  Vision worst:    8,000ms          │  8s                       │        │
│  │  → Nhanh hơn 25-66x so với screenshot                         │        │
│  └────────────────────────────────────────────────────────────────┘        │
│                                                                            │
│  KEY INSIGHT: "Give the model text because text is what models             │
│               are actually good at."                                       │
│                                                                            │
│  FREE & OPEN SOURCE: Apache 2.0, Mac + Windows + Linux                    │
│  8.8k ⭐ GitHub | 609 forks | 60+ commands | 3 platforms                  │
└────────────────────────────────────────────────────────────────────────────┘
```

Agent Desktop là tool của Vercel Labs (thực nghiệm arm của Vercel — tạo ra Next.js, 10k followers, 395 public repos trên GitHub). Chris Tate announce ngày 31/10 với một dòng: **"Agent browser for your whole computer."** Prism Labs phân tích đây là breakthrough trong lớp hạ tầng computer use.

---

## 2. Vấn Đề Cốt Lõi: Screenshot Computer Use Quá Chậm

### 2.1 Vòng Lặp Của Screenshot-Based Computer Use

```
Agent muốn click button
        │
        ▼
  [Chụp screenshot]  ← 3-8 giây
        │
        ▼
  [Gửi ảnh vào model frontier]  ← token vision đắt tiền
        │
        ▼
  [Model suy luận tọa độ x, y]  ← dễ sai 2px
        │
        ▼
  [Click vào tọa độ đó]
        │
        ▼
  [Chụp screenshot lại để verify]  ← 3-8 giây nữa
```

**Ví dụ:** Agent cần 50 observations để hoàn thành task:
- Screenshot loop: 50 × 3-8s = **2.5 — 6.6 phút** chỉ nhìn màn hình
- Agent Desktop: 50 × 120ms = **6 giây** tổng cộng

### 2.2 Điểm Yếu Chết Người

```
"If you've ever watched a vision agent click two pixels left of a 
 button, you already know why this matters."
                                              — Prism Labs
```

---

## 3. Giải Pháp: Accessibility Tree + JSON Refs

### 3.1 Kiến Trúc Cốt Lõi

```
                    Calling Agent
                    (LLM code / CLI)
                         │
                         ▼
              ┌──────────────────┐
              │   agent-desktop  │  ← CLI / C library / npm package
              └──────────────────┘
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
           macOS      Windows     Linux
         AX API    UI Automation  ATSPI
              │          │          │
              └──────────┼──────────┘
                         ▼
                    Target App
                 (bất kỳ app nào)
```

**PlatformAdapter** trừu tượng hóa 3 OS — Core không bao giờ import OS crate, platform crates inject vào Core khi compile time.

### 3.2 Vòng Lặp 4 Lệnh — "The Loop in Four Commands"

```
1. agent-desktop snapshot --app Notes -i --compact
   → Trả về JSON: snapshot ID + list elements với role/label

2. agent-desktop act --ref "gs8f3k2p9:e4" click  
   → Click vào element với ref đó; tool confirm delivery trước khi return

3. agent-desktop type --ref "gs8f3k2p9:e8" "Hello World"
   → Gõ text vào textarea

4. agent-desktop snapshot --app Notes  
   → Snapshot lại để verify thay đổi
```

**Ref format:** `snapshotID:elementID` — stale ref từ snapshot cũ bị **reject tự động**, không silent click nhầm.

### 3.3 JSON Snapshot Format

```json
{
  "snapshot": "s8f3k2p9",
  "app": "Notes",
  "elements": [
    {
      "ref": "gs8f3k2p9:e4",
      "role": "button",
      "label": "New Note"
    },
    {
      "ref": "0s8f3k2p9:e8",
      "role": "textfield",
      "label": "Body",
      "value": ""
    }
  ]
}
```

**Điểm mấu chốt:** Model đọc text JSON ở vài trăm tokens — không phải hình ảnh toàn màn hình. Rẻ hơn nhiều.

---

## 4. Hiệu Năng: Số Liệu Thực Tế

```
Time per observation (milliseconds):

Tree snapshot    █ 120ms
Vision (best)    ████████████████████████████ 3,000ms  
Vision (worst)   █████████████████████████████████████████████████████████████████ 8,000ms

Qua native binary (direct C call): ~21ms/observation
```

**So sánh chi tiết:**
| Phương pháp | Thời gian/observation | Lý do |
|-------------|----------------------|-------|
| Tree snapshot | 50-200ms (avg ~120ms) | Hỏi OS trực tiếp, không model |
| Vision (best case) | ~3,000ms | Model frontier xem ảnh |
| Vision (worst case) | ~8,000ms | Mạng chậm + phức tạp |
| Native binary call | ~21ms | Không overhead JS/CLI |

**Verification độc lập:** Developer Flavio Copes chạy end-to-end trên Linux:
> *"The agent observes, acts, and observes again. There is no pixel match in that loop."*
Real text landed in real editor, edits verified, auto-wait handled timing không cần `sleep`.

---

## 5. Safety by Refusal

```
┌──────────────────────────────────────────────────────┐
│  SAFETY BY REFUSAL — Mô hình an toàn cực kỳ thận trọng │
│                                                      │
│  ✓ Headless by default                               │
│    → Không bao giờ di chuyển pointer hoặc steal focus  │
│    → Phải explicit pass --headed flag                │
│                                                      │
│  ✓ Snapshot-scoped refs                              │
│    → Stale refs bị reject thay vì click nhầm         │
│                                                      │
│  ✓ key-down / mouse-down refuse until proven safe    │
│    → Raw primitives bị vô hiệu cho đến khi team      │
│      tin tưởng release mechanics                     │
│                                                      │
│  ✓ Protected OS processes can't be closed            │
│                                                      │
│  ✓ Elevated Windows apps → PERM_DENIED               │
│    → Không cho phép agent leo thang privilege        │
│                                                      │
│  Triết lý: "When in doubt, refuse."                  │
│  "Make the agent's world predictable, not powerful." │
└──────────────────────────────────────────────────────┘
```

---

## 6. Installation & Usage

### 6.1 Cài Đặt

```bash
# Qua npm (Node wrapper)
npm install -g agent-desktop
# → agent-desktop@1.0.2

# Qua AGY skills
agy skills add https://github.com/vercel-labs/agent-desktop --skill agent-desktop
agent-desktop skills get-desktop
agent-desktop skills verify-rig

# Dùng như C library (in-process, Python bindings)
# Xem docs trên GitHub
```

### 6.2 Workflow Thực Tế

```bash
# 1. Snapshot app để xem elements
$ agent-desktop snapshot --app Notes -i --compact
{"snapshot":"s8f3k2p9","elements":[
  {"ref":"gs8f3k2p9:e4","role":"button","label":"New Note"},
  {"ref":"0s8f3k2p9:e8","role":"textfield","label":"Body"}]}

# 2. Act on element
$ agent-desktop set --ref "0s8f3k2p9:e8" "My new note content"

# 3. Verify
$ agent-desktop snapshot --app Notes
```

### 6.3 Platform Support

| Feature | macOS | Windows | Linux |
|---------|-------|---------|-------|
| Accessibility API | AX API | UI Automation | ATSPI |
| Screenshot | Yes | Yes | Yes (Wayland: portal) |
| Mouse input | Yes | Yes | X11 / libei |
| Click/Type/Keyboard | Yes | Yes | AT-SPI, X11 GNOME |
| App & window mgmt | Yes | Yes | LaunchSVC X11 window control |

---

## 7. So Sánh Proprietary vs Open Source

```
PROPRIETARY                        OPEN SOURCE
Screenshot computer use     VS     Agent Desktop
─────────────────────────          ─────────────────────────
• Model reasons over pixels        • OS reports real UI structure
• 3-8s per observation             • 50-200ms per read
• Coordinates can drift            • Stable refs, safe retries
• Works on ANYTHING visible        • Works on anything LABELED
• Vision tokens for every step     • Text tokens (cheap)
```

**Key trade-off:** 
- Screenshot: works kể cả canvas apps, game rendered UI
- Agent Desktop: chỉ hoạt động nếu app có accessibility labels

---

## 8. Command Groups (60+ Commands)

| Group | Commands |
|-------|---------|
| Observe | snapshot, find, get, ls, screenshot, list-surfaces |
| Act on a ref | click, double-click, triple-click, right-click, type, set-value, clear, focus, select, toggle, check, uncheck, expand, collapse, scroll, activate |
| Keyboard/mouse | key, reset, insert-text, mouse-move, mouse-up, mouse-down |
| Apps & windows | launch, close-app, list-apps, list-windows, focus-window, minimize-window, review-window, new-window, maximize, escalate, restore, open-surface |
| Notifications | list-notifications, dismiss-notification, dismiss-all-notifications, notification-action |
| Clipboard | clipboard-get, clipboard-set, clipboard-clear |
| Wait | wait |
| Sessions | session, trace, cursor-overlay |
| System | status, permissions, version, batch, skills |

---

## 9. Phản Ứng Cộng Đồng

```
COMMUNITY (ủng hộ):
  "Computer use across Claude, ChatGPT and Grok is fragmented —
   one tool for all of them is welcome."

SKEPTIC (phản biện):
  "The accessibility tree is great until you hit a canvas app or
   an Electron app with half the labels missing."

SKEPTIC:
  "How does it compare to the other drivers? We need a real
   computer-use benchmark."

PRISM LABS (đáp):
  "Fair — but 120ms reads + the agent-browser DevTools bridge 
   cover most real workflows today."
```

**Bridge thông minh cho Electron apps:** Agent Desktop có thể mở DevTools port và chuyển web content sang Agent Browser — hai tool designed to "meet in the middle".

---

## 10. Timeline: 8 Tháng Đến 1.0

```
Timeline:
  2026-02-19  ● Repo created
              │  Quiet start under vercel-labs
              │
  2026-10-28  ● v1.0.0 ships
              │  Patched to v1.0.1 same night
              │
  2026-10-30  ● v1.0.2
              │  Stability fixes before going public
              │
  2026-10-31  ● Public announcement (Halloween)
                 "agent-browser, for your whole computer"
                 — After stable release, NOT before it
```

**Nhận xét của Prism Labs:**
> *"Vercel shipped the software first and the hype second, which tells you who they built it for."*

---

## 11. Điểm Đặc Biệt: Agent-Agnostic

Không lock vào bất kỳ agent vendor nào:
- **Claude Code** ✓
- **Codex** ✓  
- **Cursor** ✓
- **Bất kỳ script nào có thể chạy shell command** ✓

Ships as:
- Plain CLI
- Installable skill qua skills ecosystem
- C library cho in-process calls với Python bindings

---

## 12. Key Takeaways

```
Triết lý cốt lõi của Agent Desktop:

  "Give the model text because text is what models are actually good at."

  Thay vì: Screenshot → Vision model → Coordinates → Click
  Dùng:    OS Accessibility API → JSON text → Ref → Act

Áp dụng ngay:
  ✅ npm install -g agent-desktop
  ✅ agent-desktop snapshot --app [AppName]
  ✅ Dùng ref từ snapshot để act
  ✅ Snapshot lại để verify
  ✅ Không cần screenshot, không cần vision tokens

Giới hạn cần biết:
  ⚠️ Canvas-rendered apps (games, Figma) → không thấy elements
  ⚠️ Electron apps với bad accessibility labels → partial visibility
  ⚠️ → Workaround: DevTools bridge sang Agent Browser
```

---

## 13. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

### Notes về Computer Use & Desktop Automation:
- [Vercel CEO Agentic Engineering Workflow — Guillermo Rauch](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/vercel_ceo_agentic_engineering_workflow_guillermo_rauch.md) — Context về Vercel's agentic vision

### Notes về Agent Infrastructure:
- [Excalidraw CLI — Diagrams cho AI Agents](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/excalidraw_cli_diagrams_for_ai_agents.md) — CLI tools cho agents
- [Amazon Infinity AI Agent Runtime — Rust](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/amazon_infinity_ai_agent_runtime_rust_lets_get_rusty.md) — Pure Rust agent runtime (cùng ngôn ngữ với Agent Desktop)

### Notes về Multi-Platform Agent Tools:
- [Claude Code Mods Ultimate Guide](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/claude_code_mods_ultimate_customization_guide.md) — Skills ecosystem agent-desktop dùng để install
- [9 New Claude Mods — Robonuggets](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/9_new_claude_mods_robonuggets.md) — MCP tools + computer use integrations
