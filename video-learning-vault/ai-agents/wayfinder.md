# Wayfinder | Multi-Session Agentic Planning Framework

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=F3lL98Pj90o)
- **Watch Skill ID:** `e1d70f540dee9974`
- **Category:** #ai-agents
- **Date Processed:** 2026-08-05
- **Duration:** 15:08
- **Speaker:** Matt Pocock (AI Hero)

---

## 1. The Single-Session Planning Ceiling
Standard planning primitives (like `/grill-me` or `/grill-with-docs`) work well for small tasks but fall short on large features.
* **Smart Zone Context Limits:** Complex features exceed the model's high-reasoning context window ("smart zone").
* **Fog of War:** Trying to specify every detail upfront fails because downstream decisions depend on upstream research and prototypes.
* **Token Bloat:** Managing massive planning context in a single chat session leads to high costs and cognitive degradation in model reasoning.

---

## 2. What is Wayfinder?
Wayfinder (`/wayfinder`) is a framework that maps out a large goal into a structured dependency graph of "decision tickets" managed directly inside an issue tracker (GitHub, Linear, Jira).

```
┌──────────────────────────────────────────────┐
│          WAYFINDER PARENT MAP                │
│  (Tracks overall progress & resolves fog)    │
└──────────────────────┬───────────────────────┘
                       │
         ┌─────────────┴─────────────┐
         ▼                           ▼
┌──────────────────┐       ┌──────────────────┐
│ ACTIVE FRONTIER  │       │   FOG OF WAR     │
│ (Ready to solve) │       │(Blocked by deps) │
└────────┬─────────┘       └────────┬─────────┘
         │                          │
         ├─ Research Ticket         ├─ Downstream Grilling
         ├─ Prototype Ticket        └─ Implementation Tasks
         └─ Grilling Ticket
```

### Core Mechanisms:
* **The Frontier:** A list of active tickets that can be worked on immediately because their blocking dependencies are resolved.
* **The Fog of War:** Downstream tickets that remain locked/unclear until upstream tickets on the frontier are resolved.
* **Ticket Resolution:** You run `wayfinder <ticket-url>` in a new clean session. Once resolved, the decision is summary-written back to the sub-issue (which is closed) and propagated to the parent map.

---

## 3. Four Decision Ticket Types

1. **Research:** The agent spawns a background sub-agent to search documentation or code and report findings back to the ticket.
2. **Prototype:** Generates high-fidelity mockups/sketches/code. Prototyping prevents upfront planning from turning into a rigid "waterfall" process by providing immediate, real-world feedback.
3. **Grilling:** Interactive QA sessions with the human on specific design or architecture details.
4. **Task:** Action items that need to be executed in the real world (manual setups, external APIs, etc.).

---

## 4. Non-Persistent Specifications
Traditional development keeps specs as living, persistent documents that require constant updating. 
* **The Wayfinder Spec Model:** In Wayfinder, specs are **non-persistent**.
* **The Flow:** 
  1. Map out the project via `/wayfinder`.
  2. Once the map is fully resolved, compile it into a spec using `/to-spec`.
  3. Generate implementation tickets via `/to-tickets`.
  4. Implement the tickets.
  5. **Delete/close the spec issue.** The code itself is the only persistent source of truth.
* **Primary Source Linking:** Wayfinder specs link directly to the historical closed decision tickets, allowing agents to audit *why* a decision was made.

---

## 5. Useful Search Keywords
- `wayfinder multi session planning`, `ai agent frontier map`, `non persistent spec driven development`, `issue tracker agnostic wayfinder`, `preventing waterfall with prototypes`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
