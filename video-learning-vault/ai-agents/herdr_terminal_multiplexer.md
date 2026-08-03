# Herdr: The Modern Terminal Multiplexer | Tool Case Study

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=27B50lXinWM)
- **Watch Skill ID:** `3decb06030d1bf87`
- **Category:** #ai-agents
- **Date Processed:** 2026-08-04
- **Duration:** 19:25
- **Subject:** Herdr Multiplexer (v0.7)

---

## 1. What is Herdr?
**Herdr** is a modern terminal multiplexer heavily inspired by `tmux`, but rebuilt to include modern CLI defaults out-of-the-box. It bridges the gap between terminal multiplexing and AI-assisted development by featuring native **AI Agent Awareness**.
* **Platforms:** Supported on macOS (`brew`), Linux/NixOS (`curl`), and Windows (`PowerShell`).
* **Defaults:** Default prefix key map is `Ctrl + B` (matching tmux), with visual hotkey helper hints displayed at the bottom of the screen upon hitting the prefix.

---

## 2. Core Feature Comparison: Tmux vs. Herdr

| Feature | Tmux | Herdr |
|---|---|---|
| **Mouse Support** | Disabled by default; basic features when enabled in config. | Out-of-the-box; switch tabs, right-click menu to split/swap panes, drag-to-resize. |
| **Vim Keymaps** | Requires plugins/extensive configuration. | Built-in Vim-style navigation for panes, resizing, and copy mode (`prefix [`). |
| **State Persistence** | Server crash kills layouts/tabs (unless using resurrection plugins). | Separates session states from processes. Layouts, tabs, and panes are saved in persistent snapshots. |
| **Project Workspace** | Handled via sessions; manual tmuxinator setups. | Built-in **Workspaces** designed around projects. |
| **AI Agent Awareness** | None. | Native tracking of terminal AI agents. |

---

## 3. Native AI Agent Awareness
Herdr is designed for modern developer workflows where multiple terminal-based AI agents run concurrently:
- **Detection:** Automatically scans shell processes in active panes to identify supported agents (e.g. Codex, Hermes, Claude Code).
- **Sidebar Monitoring:** Expanding the sidebar shows agent status: `Running`, `Idle`, `Blocked (Awaiting user permission)`, or `Done`.
- **Detached Execution:** Agents keep running in the background even if you detach from the Herdr session.
- **Session Restoring:** The config option `resume_agents_on_restore = true` automatically restores native agent conversation sessions upon server restart or reattach.

---

## 4. HerdrPlus: Projects & Quick Actions
The official **HerdrPlus** plugin (requires v7.0+) expands Herdr into a complete local developer dashboard:

### A. Quick Actions (Fuzzy command runner)
* Instead of memorizing key bindings for build scripts, you hit `prefix Y` to open a fuzzy-search list of commands.
* Supports nested configurations (e.g., selecting "Edit Configs" opens a sub-list of target configuration files).
* Actions are defined as simple executables inside `.toml` files.

### B. Projects TOML (Auto-Workspaces)
* Triggered by `prefix Shift O`, it lets you jump into predefined projects.
* Opens a dedicated workspace, sets up multiple tabs and panes, and boots target commands (e.g., opening Neovim in pane 1, and starting an AI coding agent in pane 2).
* **Sample `projects.toml` Workspace configuration:**
  ```toml
  [[projects]]
  name = "zshrc-config"
  directory = "~"
  tabs = [
      { name = "editor", command = "nvim .zshrc" },
      { name = "shell", command = "zsh" }
  ]
  ```

---

## 5. Keyboard Navigation & Configuration Cheatsheet
* Remaps are configured in `config.toml` (typically located in the user config directory).

| Default Bind | Action | Custom Remap (Recommended) |
|---|---|---|
| `Ctrl + B` | Prefix | - |
| `prefix q` | Detach | - |
| `prefix c` | Create Tab | - |
| `prefix Shift T`| Rename Tab | `prefix ,` |
| `prefix v` | Vertical Split | - |
| `prefix -` | Horizontal Split | - |
| `prefix z` | Toggle Zoom (Maximize Pane) | `prefix m` |
| `prefix Shift R`| Reload Config | `prefix r` |
| `prefix r` | Resize Mode (use Vim keys to adjust) | `prefix Shift R` |

---

## 6. Useful Search Keywords
- `herdr terminal multiplexer`, `herdr agent awareness`, `herdrplus projects toml`, `modern tmux alternative`, `thin client terminal remote`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
