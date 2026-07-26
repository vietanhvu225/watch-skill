# The Art of Loop Engineering | LangChain Webinar

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=jPPiZ22DY3g)
- **Watch Skill ID:** `fa6483c46d4119a1`
- **Category:** #ai-agents
- **Date Processed:** 2026-07-26
- **Duration:** 46:00
- **Speaker:** Sydney (Product Manager on the Open Source Team, LangChain)

---

## 1. Why Loops Matter in Agentic Software
Agents are a new software paradigm characterized by endless inputs and non-deterministic outputs. Traditional static software development practices break when applied to agents. 
- **The Core Thesis:** The value of agentic systems lies not just in the agent you build to automate work, but in the **loops** you construct around it to manage, verify, and evolve its behavior.

---

## 2. The 4 Levels of Loops (Russian Doll Model)
LangChain models loops as composable, nested layers. Each outer loop manages and optimizes the inner loops:

```
┌─────────────────────────────────────────────────────────┐
│          Level 4: Hill Climbing / Learning Loop         │ <-- Evolving prompts, memory, and skills from traces
│  ┌───────────────────────────────────────────────────┐  │
│  │             Level 3: Event-Driven Loop            │  │  <-- Slack/GitHub integrations & schedule triggers
│  │  ┌─────────────────────────────────────────────┐  │  │
│  │  │         Level 2: Verification / Goal        │  │  │  │  <-- Grader/rubric middleware retry checking
│  │  │  ┌───────────────────────────────────────┐  │  │  │  │
│  │  │  │           Level 1: Core Agent         │  │  │  │  │  <-- Action-taking loop (Model <-> Tools)
│  │  │  └───────────────────────────────────────┘  │  │  │  │
│  │  └─────────────────────────────────────────────┘  │  │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

### Level 1: The Core Agent Loop
* **Mechanism:** The model receives context -> generates tool calls -> tools return observations -> the model reasons over output -> calls the next tool or exits.
* **Optimization:** Selecting the right model size based on task complexity (cost vs. capability), and optimizing tool descriptions (prompt engineering) so the agent knows when to invoke them.

### Level 2: The Verification / Goal Loop
* **Mechanism:** Evaluates the output of the Core Agent Loop against a set of rubrics or grading criteria. If the output fails the criteria, it feeds the state back to the agent loop for a retry.
* **Middleware Integration:** LangChain's open-source terminal coding agent (**Dcode**, based on Deep Agents) implements a `/goal` command with Rubric middleware, giving users visibility and control over success criteria.
* **Benefit:** Allows developers to use cheaper models for complex tasks by letting the grading loop catch and correct early errors.

### Level 3: The Event-Driven Loop
* **Mechanism:** Integrating agents into existing communications and workspaces (Slack, GitHub, email, calendars).
* **Triggers:** Scheduled (e.g., calendar daily brief agents) or event-driven (e.g., receiving an email, PR creation, or Slack ping `@agent`).
* **Example:** A Slack channel `#docs-please` triggers a docs-writer agent to draft a PR. The Level 2 loop verifies links/CI. A human merges the PR (updating the docs), closing the system loop and motivating future triggers.

### Level 4: The Hill Climbing / Self-Improvement / Learning Loop
* **Mechanism:** A background agent (e.g., LangSmith **Engine**) analyzes bulk execution traces to detect recurring failure modes (e.g. bad tool arguments, missed context, ignored user preferences).
* **Action:** The Engine automatically updates prompts, tool descriptions, or memory stores in the source code repository, and pushes the improvements back to the agent.
* **Memory vs. Code Changes:** For self-improvement, developers should first focus on updating **memory, skills, and prompts** (semantic changes). Only change **code/runtime architecture** (deterministic rules) if prompts fail to enforce compliance.

---

## 3. Human-In-The-Loop (HITL) Integration
Automation does not mean removing human judgment. Rather, it means leveraging humans where they add strategic value.

| Loop Level | HITL Placement | Example Use Case |
|---|---|---|
| **Level 1 (Core)** | Tool Call Approval | Travel agent asking approval before booking a flight or running a credit card transaction. |
| **Level 2 (Verify)** | Grader Verification | Having a human grade the output quality at the verification phase. |
| **Level 3 (Event)** | Workflow Gatekeeping | A human reviewing and merging an automatically generated Pull Request. |
| **Level 4 (Learning)**| Code Review | A developer reviewing and approving automatic prompt/skill updates pushed to Git. |

---

## 4. Evals: The Engine of Loop Engineering
You cannot safely close the Level 4 self-improvement loop without a robust evaluation suite ("an eval in a trench coat") to prevent regressions. LangChain recommends having two types of evals:
1. **Baseline Unit Evals:** Simple checks to ensure core functionality doesn't regress.
2. **Integration / Stretch Evals:** Complex, end-to-end tasks designed for hill climbing.
*Note: LangChain provides an **Eval Engineering** skill that automatically generates evals from traces by interviewing developers.*

---

## 5. Useful Search Keywords
- `langchain loop engineering`, `dcode rubric middleware`, `cmu productivity dissipation`, `agentic vs algorithmic`, `eval engineering`, `hill climbing loop`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
