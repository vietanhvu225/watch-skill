# MCP Apps: Extending the Frontier | MCP Steering Committee

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=-jY2T2PiJBE)
- **Watch Skill ID:** `7b31de6eea9c4516`
- **Category:** #ai-agents
- **Date Processed:** 2026-08-04
- **Duration:** 18:00
- **Speakers:** Ido Salomon (Creator of MCPY, Co-creator of MCP Apps) & Liad Yosef (Co-creator of MCP Apps, Co-founder of Aura)

---

## 1. The Problem with Text-Only Agents
While LLM agents are highly functional, text/markdown outputs are an inefficient way to convey dense information (leading to "walls of text").
* **Company Blocker:** Enterprises refuse to build traditional text-only MCP servers because they do not want their services reduced to a textual database, losing their custom UX, branding, and identity.
* **The Vision:** Instead of printing markdown text, MCP servers should transmit sandboxed, interactive **user interfaces (UI)** directly into the agent's chat interface (Claude, ChatGPT, VS Code).

---

## 2. What is an MCP App?
An **MCP App** is an open protocol extension to the Model Context Protocol (derived from the original open-source project **MCPUI**). It standardizes how interactive UIs are transmitted and how these UIs communicate back and forth with the host LLM client.

```
┌──────────────┐     Get HTML      ┌──────────────┐  Render HTML  ┌──────────────┐
│  Host Chat   │ ───────────────>  │  MCP Server  │ ────────────> │  Sandboxed   │
│   (Claude)   │ <───────────────  │ (Spotify API)│               │  Iframe UI   │
└──────────────┘  Tool Calls/API   └──────────────┘               └──────────────┘
       ▲                                                                 │
       │                                                                 │ Click
       └───────────────────────── Send Event ────────────────────────────┘
                              (App Callback)
```

1. **Transmission:** The MCP server returns HTML (stored as an MCP Resource).
2. **Sandboxing:** The host chat client preloads and renders the HTML inside a sandboxed iframe.
3. **Bi-directional Comms:** When the user clicks an interactive element (e.g., "Favorite Song"), the app callback sends a structured event to the host.
4. **Host-in-Control:** Rather than the web app controlling the journey, the host LLM decides how to act on the event (e.g., running another prompt or invoking a specific backend tool).

---

## 3. The "Agentic Web" Shift
MCP Apps represent a fundamental shift in how humans consume the web:

| Traditional Web (Browser-Centric) | Agentic Web (Assistant-Centric) |
|---|---|
| Users open 20 browser tabs (e.g., planning a trip on Booking + Google Maps + Calendar) and navigate heavy dashboards. | The personal assistant understands the user's intent and composes **atomic UI chunks** dynamically. |
| The service provider controls the user's journey. | The host chat client controls the user's journey for full auditability. |
| 99% of dashboard UI is generic and irrelevant to the specific task. | Only the relevant "atoms" of the application are rendered. |

---

## 4. The Generative UI Spectrum
MCP Apps is agnostic to how UI is generated and sits on a spectrum:
* **Predefined UI (MCP Apps MVP):** A static HTML iframe returned by the server.
* **Declarative UI (A2UI / JSON Render):** The server returns JSON UI schemas, and the host client renders them natively.
* **Generative UI (Claude Artifacts):** The LLM generates the UI code on the fly based on instructions.

*Note: The working group has released guides on A2UI-to-MCP-Apps interoperability, allowing servers to write once and deploy across Gemini, ChatGPT, and Claude.*

---

## 5. Addressable Market & Ecosystem
- **Scale:** ChatGPT has 800M+ weekly users (10% of world population). Building an MCP App instantly exposes a service to a market **170x larger** than the Apple App Store launch.
- **Steering Committee:** Features members from Anthropic, OpenAI, and community partners, holding tri-weekly meetings.
- **Repository:** Spec and SDK are hosted under the official `X apps` repository.

---

## 6. Useful Search Keywords
- `mcp apps protocol`, `mcpui interactive applications`, `agentic web atoms`, `a2ui mcp apps interoperability`, `generative ui spectrum`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
