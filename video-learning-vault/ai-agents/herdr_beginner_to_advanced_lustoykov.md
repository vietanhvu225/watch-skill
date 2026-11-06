# Herdr: Từ Người Mới Đến Nâng Cao Trong 15 Phút | lustoykov

- **Video URL:** [https://www.youtube.com/watch?v=WTmCkYMelIU](https://www.youtube.com/watch?v=WTmCkYMelIU)
- **Video ID:** `WTmCkYMelIU`
- **Category:** #ai-agents
- **Date Processed:** 2026-11-06
- **Duration:** 15:44
- **Speaker:** lustoykov
- **Transcript File:** [herdr_beginner_to_advanced_lustoykov_transcript.txt](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/herdr_beginner_to_advanced_lustoykov_transcript.txt)

---

## 1. Tóm Tắt Điều Hành (Executive Summary)

```
┌─────────────────────────────────────────────────────────────────┐
│                      HERDR: TERMINAL CHO AI AGENTS              │
│                                                                 │
│  Herdr = Terminal Workspace Manager + Multiplexer               │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  LEAD AGENT (Opus 5.5)                                  │   │
│  │       │                                                 │   │
│  │   orchestrates via Herdr CLI/Messaging Bridge           │   │
│  │       │                                                 │   │
│  │  ┌────┴────────────────────────────────────┐            │   │
│  │  │  WORKER AGENTS (2x2 grid)               │            │   │
│  │  │  ┌──────────┐  ┌──────────┐             │            │   │
│  │  │  │ GPT:     │  │ GPT:     │             │            │   │
│  │  │  │ Bug Fix  │  │ Perf Fix │             │            │   │
│  │  │  └──────────┘  └──────────┘             │            │   │
│  │  │  ┌──────────┐  ┌──────────┐             │            │   │
│  │  │  │ Opus:    │  │ Opus:    │             │            │   │
│  │  │  │ UI Polish│  │ Architect│             │            │   │
│  │  │  └──────────┘  └──────────┘             │            │   │
│  │  └────────────────────────────────────────┘            │   │
│  └─────────────────────────────────────────────────────────┘   │
│                                                                 │
│  KEY FEATURES:                                                  │
│  • Mouse-first + tmux keybindings (not built on tmux)           │
│  • Status indicators: 🔴 blocked  🟡 working  🟢 done           │
│  • Persist sessions (server mode): close terminal, agents run   │
│  • Remote machine via SSH: `herdr machine @macmini`             │
│  • Agent Skill File: teaches agents Herdr's CLI capabilities    │
└─────────────────────────────────────────────────────────────────┘
```

lustoykov giới thiệu **Herdr** — terminal workspace manager được thiết kế từ đầu dành cho AI agents. Video đi từ cơ bản (cài đặt, keybinding, single agent) đến nâng cao (multi-agent orchestration, remote machine qua SSH, resource pooling). Điểm nổi bật: agents có thể tự điều khiển terminal thông qua Herdr Skill File, giao tiếp với nhau qua Herdr CLI bridge, và tất cả tiếp tục chạy ngay cả khi đóng terminal.

---

## 2. Khái Niệm Cốt Lõi

### 2.1 Herdr Là Gì?

**Herdr** = Terminal Workspace Manager + Multiplexer dành cho AI coding agents.

| Tính năng | Mô tả |
|-----------|-------|
| **Terminal Multiplexer** | Mở nhiều terminal trong 1 cửa sổ duy nhất |
| **Mouse-first** | Điều khiển bằng chuột, nhưng cũng hỗ trợ tmux keybindings |
| **Không phải tmux** | Kiến trúc khác hoàn toàn, tuy nhiên hỗ trợ tmux key conventions |
| **Agent-native** | Thiết kế từ đầu để agents điều khiển terminal |
| **Server mode** | Chạy server nền — đóng terminal nhưng agents vẫn hoạt động |

### 2.2 Herdr Skill File — Thành Phần Quan Trọng

Khi cài Herdr, **bắt buộc phải cài thêm Agent Skill File**:

```bash
# Xem skill file
herdr skill

# Skill file dạy agents:
# - Cách giao tiếp với agents khác
# - Cách spawn agents mới
# - Cách mở panes, split terminal
# - Toàn bộ Herdr CLI orchestration
```

Skill file là "instruction manual" để agents biết dùng các power mà Herdr cung cấp.

### 2.3 Hệ Thống Màu Trạng Thái (Status Indicators)

```
🔴 Đỏ  = Agent bị blocked (cần input từ user)
🟡 Vàng = Agent đang làm việc (working)
🟢 Xanh = Agent đã xong (done/idle)
```

Với nhiều agents chạy song song, nhìn vào màu để biết ngay agent nào cần chú ý — không cần nhìn vào từng pane.

---

## 3. Setup Chi Tiết: Từ Cơ Bản Đến Nâng Cao

### 3.1 Cài Đặt & Khởi Động

```bash
# Cài Herdr (copy lệnh từ website)
# Sau đó khởi động:
herdr

# Cài Agent Skill File (quan trọng!)
herdr skill
```

**Setup ban đầu:**
- Chọn theme (ví dụ: cappuccino)
- Bật/tắt Status Indicators (màu trạng thái)
- Bật/tắt Sound notifications (ồn nếu có 20+ agents)
- Chọn integrations: Pi Cloud, Codex, Open Code, ...

### 3.2 Điều Khiển Terminal

**Hai cách điều khiển:**

1. **Chuột**: Right-click → "Split right", "Close pane", ...
2. **Keyboard** (tmux-style):
   - `Ctrl+B` → prefix mode
   - `Ctrl+B` + `?` → xem tất cả keybindings
   - `Ctrl+B` + `/` → tìm kiếm keybinding
   - `Ctrl+B` + `V` → split vertically
   - `Ctrl+B` + `X` → đóng pane

**Triết lý học keybindings:**
> Thay vì cố nhớ hết, cứ nhấn `Ctrl+B` rồi `?` rồi `/` để tìm cái cần dùng.

### 3.3 Spaces & Agents

Herdr có 2 khái niệm chính trong sidebar:
- **Spaces** (Projects): Projects đang làm việc
- **Agents**: Các coding agents đang chạy trong project đó

### 3.4 Setup Lead Agent

```
# Yêu cầu agent tự đổi tên (Herdr Skill cho phép điều này!)
> "Can you change your name to Opus 5.5 lead and also the tab name?"

# Agent sẽ dùng Herdr CLI để:
# 1. Đổi tên instance của mình
# 2. Cập nhật tên tab
```

### 3.5 Setup Worker Agents (2x2 Grid)

Thay vì click thủ công, **để Lead Agent tự tạo**:

```
Prompt gửi cho Lead:
"Create a workers tab in the same workspace with a 2x2 grid 
 four fresh worker sessions one per pane:
 - 2x GPT for performance and bug fixing (specify model + effort)
 - 2x Opus for UI polish and architecture"
```

Kết quả: Lead agent sẽ:
1. Tạo tab mới tên "Workers"
2. Chia 4 panes theo 2x2 grid
3. Khởi động đúng coding agent trong từng pane
4. Assign đúng task cho từng worker

---

## 4. Orchestration: Điều Phối Agents

### 4.1 Herdr CLI Bridge — Giao Tiếp Giữa Agents

```bash
# Gửi message vào Lead agent từ terminal riêng
herdr agent prompt "Opus 5.5 lead" "Give me a quick status update on the workers"

# Agents giao tiếp với nhau thông qua Herdr Bridge:
# Lead → inject message → Worker
# Worker → inject message → Lead
```

**Tại sao dùng Herdr thay vì custom scripts?**
- Herdr là terminal multiplexer sở hữu tất cả terminal instances
- Inject messages natively, không cần hacks
- Hoạt động out-of-the-box — không cần maintain custom code

### 4.2 Multi-Agent trên 1 Branch (Tiết Kiệm Disk)

Thay vì tạo worktree riêng cho mỗi agent (tốn disk), lustoykov dùng trick:

```
Prompt cho Lead Agent:
"Tell all workers: You share the same checkout on main.
 Coordinate directly through Herdr messaging.
 Make routine decisions autonomously.
 Only message the lead with:
   - Completed work
   - Unresolved blockers  
   - Important decisions needing input
 Skip chatter and routine progress updates."
```

**Kết quả**: 
- Tất cả workers làm việc trên `main` cùng lúc
- Tự phối hợp với nhau qua Herdr messaging
- Rất ít conflict
- Nếu thực sự cần worktree riêng → agents có thể tự tạo nhưng họ ít khi thực sự cần

### 4.3 Resource Pooling (iOS Simulator Example)

```
Scenario: 4 agents, nhưng chỉ có 3 iOS simulators

Giải pháp Herdr:
"Hey, coordinate between each other who's going 
 to use the simulator."

→ Agents tự thỏa thuận ai dùng simulator nào
→ Lock/unlock chia sẻ simulator tự động
```

---

## 5. Remote Machine Integration

### 5.1 Prerequisites

```bash
# Trên remote machine:
# 1. Cài Herdr
# 2. Setup SSH connection
ssh macmini  # phải chạy được trước
```

### 5.2 Connect Remote Machine

```bash
# Single command để connect remote Herdr session
herdr machine @macmini --label "Mac mini"
```

Sau khi connect:
- Sidebar chia thành **Local** và **Mac mini**
- Có thể thấy projects và agents của cả hai machines
- Điều khiển agents remote như agents local
- Connect bao nhiêu remote machines tùy ý

### 5.3 Use Cases

| Situation | Lý do dùng Remote |
|-----------|------------------|
| Quá nhiều agents → máy local quá tải | Offload sang Mac mini/cloud box |
| iOS development trên Mac | Dev trên Mac mini từ máy khác |
| Team dev | Mỗi người connect vào shared dev box |

---

## 6. Feature Nổi Bật: Persist Sessions

```
Scenario:
1. Herdr đang chạy với 5 agents làm việc
2. User đóng terminal window
3. ??? 

Với tmux thông thường: agents chết
Với Herdr: Herdr chạy server ngầm → agents TIẾP TỤC CHẠY

4. User mở terminal mới
5. Gõ: herdr
6. Tất cả agents quay lại — họ chưa bao giờ dừng!
```

**Tác dụng thực tế:**
- Khởi động agents buổi sáng, đi làm việc khác
- Agents tự xử lý tasks
- Quay lại xem tiến độ bất cứ lúc nào
- Không bao giờ mất session

---

## 7. Workflow Thực Tế Của lustoykov

```
Setup hoàn chỉnh cho dự án iOS (Halvie):

LOCAL MACHINE:
├── Tab 1: Lead Agent (Opus 5.5) — Tab chính, theo dõi chủ yếu
└── Tab 2: Workers (2x2 grid)
    ├── Pane 1: GPT — Bug fixing
    ├── Pane 2: GPT — Performance optimization
    ├── Pane 3: Opus — UI polish
    └── Pane 4: Opus — Architecture

REMOTE MACHINE (Mac mini):
└── Attached project với Lead + Workers tương tự

SIDEBAR VIEW:
├── 🔴 = Blocked agents (cần attention)
├── Priority view = chỉ hiện agents cần reply
└── Grouped view = theo project/machine
```

**Triết lý**: Chỉ cần nhìn vào Lead agent, workers tự xử lý. Chỉ nhảy vào khi thấy 🔴 (blocked).

---

## 8. Liên Kết Chéo & Tài Liệu Liên Quan Trong Vault

### Notes cùng chủ đề Herdr:
- [Herdr Terminal Multiplexer](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/herdr_terminal_multiplexer.md) — Overview ban đầu về Herdr
- [Herdr Plugins & Multi-Agent Orchestration](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/herdr_plugins_multi_agent_orchestration.md) — Ecosystem plugins của Herdr

### Notes về Multi-Agent Orchestration:
- [AI Civilization in Herdr](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/ai_civilization_in_herdr.md) — Use case nâng cao
- [Orchestras Not Factories](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/orchestras_not_factories_charlie_holtz.md) — Triết lý orchestration đúng
- [High Throughput Agentic Engineering](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/high_throughput_agentic_engineering_kun_chen.md) — Scale agents

### Notes về T3 Code (ADE tương tự):
- [T3 Code Đã Chinh Phục Jan Marshal](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/t3_code_won_me_over_jan_marshal.md) — So sánh Herdr vs T3 Code
- [Sử Dụng AI Agents Sai Cách — Theo t3.gg](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/youre_using_ai_agents_wrong_theo_t3.md) — Workflow T3 Code của Theo

### Notes về Skill Files & Agent Coordination:
- [Anthropic Engineers Stopped Building Agents](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/anthropic_engineers_stopped_building_agents_skills_sharbel.md) — Skills-based approach
- [Claude Code Mods Ultimate Customization Guide](file:///d:/1_source/0_Learning/watch-skill/video-learning-vault/ai-agents/claude_code_mods_ultimate_customization_guide.md) — Customize harness

