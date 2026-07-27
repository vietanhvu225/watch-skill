# Loop Engineering from First Principles | HumanLayer Case Study

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=xIt_mTQp6mY)
- **Watch Skill ID:** `cc024710b796fbf1`
- **Category:** #ai-agents
- **Date Processed:** 2026-07-27
- **Duration:** 18:00
- **Speaker:** Kyle Mistele (Software Engineer, HumanLayer)

---

## 1. The Core Philosophy
AI coding agents are powerful, but naive implementations (e.g., throwing a coding agent at a codebase with a simple bash loop) result in massive, unreadable 40,000-line PRs that stack up, create merge conflicts, and block development teams.
* **Real-World Engineering:** Real enterprise codebases with users, SLAs, and regulatory compliance require building **incremental, controlled loops** that minimize risk, control token budgets, and incorporate low-friction human steering.
* **Rule of Thumb:** *"Never send an agent to do a deterministic code's job."* Use static tools for sensing and filtering, and LLMs strictly for reasoning and code synthesis.

---

## 2. Control Theory Model of Codebases
HumanLayer maps software development to physical control loops (like thermostats or flight controls):

```
                   ┌────────────────────────────┐
                   │    Desired Set Point       │ (Codebase Standards)
                   └─────────────┬──────────────┘
                                 │
                                 ▼
   ┌───────────┐           ┌───────────┐           ┌───────────┐
   │  Sensor   │──────────>│Controller │──────────>│ Actuator  │
   │ (ast-grep)│  Error    │  (Jq/APM) │  Signal   │  (Agent)  │
   └─────▲─────┘           └───────────┘           └─────┬─────┘
         │                                               │
         │             ┌───────────────────┐             │ Incremental
         │             │   Disturbances    │             │ Change
         └─────────────│ (Coworker Code)   │<────────────┘
                       └───────────────────┘
```

1. **Set Point:** The target state of the codebase (e.g., all RPC routes migrated to the "Effect" library).
2. **Sensor:** Detects existing violations (unmigrated routes).
3. **Measured Error:** The list of files requiring migration.
4. **Controller:** Filters and decides which single file to migrate next (e.g. picking the smallest first, or using APM error rates to prioritize buggy files).
5. **Actuator:** The LLM agent that edits the code.
6. **Disturbances:** Concurrent code changes from other developers.

---

## 3. HumanLayer's Implementation Architecture

### Step 1: The Sensor (`ast-grep`)
- They use **`ast-grep`** (an Abstract Syntax Tree-based search tool) rather than standard regex or ESLint.
- **Why?** It is language-agnostic and runs out-of-band of ESLint. This prevents agents from bypassing rules using inline comments like `// eslint-disable-next-line`.
- The sensor outputs a list of violations, sorted deterministically.

### Step 2: The Disturbance Dampener
- To stop developers from adding new violations while the loop is running:
  - They run a full scan once on the main branch and record existing violations.
  - CI blocks any PR that introduces *new* violations, stopping the bleeding immediately.

### Step 3: Golden Patterns (Actuation)
- Agents are pattern replicators; they generate slop if relying purely on general internet knowledge.
- **Golden Patterns:** HumanLayer hand-writes idiomatic examples of the target migration and provides them to the agent as templates to emulate.

### Step 4: Execution & Flow Control
- **Cron Jobs:** The workflow runs once a day in GitHub Actions, opening a single, small, low-risk PR every morning.
- **Flow Control (PR Gate):** To prevent PR noise and conflicts when the team is busy, the workflow checks if any PR with the loop's label is already open. If so, the cron job **immediately exits**. There is at most **one open PR per loop** at any time.

---

## 4. Human-in-the-Loop (HITL) Steering
To avoid high-friction workflow changes (like editing prompt files in a codebase, committing, and redeploying), HumanLayer uses two key developer mechanisms:

1. **The Markdown Feedback File:**
   - A markdown file (`feedback.md`) tracked in Git records developer instructions. The agent loads this file at startup.
2. **Slash Commands (`/iterate`):**
   - Developers leave a comment on the PR (e.g., `/iterate make this variable private`).
   - GitHub Actions picks up the comment, loads the PR diff and discussion history, instructs the agent to fix the code, and writes the updated feedback rules back into `feedback.md` automatically.

---

## 5. Useful Search Keywords
- `humanlayer loop engineering`, `ast-grep coding loops`, `golden patterns agents`, `disturbance dampener CI`, `feedback md steering`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
