# Why We Killed Our Multi-Agent Pipeline | ZS Associates Case Study

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=u6jJcIFDLE4)
- **Watch Skill ID:** `143af38e605641db`
- **Category:** #ai-agents
- **Date Processed:** 2026-07-26
- **Duration:** 15:00
- **Speakers:** Subbiah Sethuraman (Head of AI Engineering, ZS) & Abhilash Asokan (Director of AI Engineering, ZS)

---

## 1. Context: Pharma Commercial Analytics
In the pharmaceutical industry, commercial analysts monitor drug market performance by performing four sequential steps:
1. **Signal Detection:** Finding drop/rise anomalies in doctor prescriptions (TRx) or sales.
2. **Root Cause Analysis:** Investigating *why* the signal failed (competitor entry, insurance/payer coverage drop, field reps underperforming).
3. **Action Recommendation:** Recommending business actions based on the root cause.
4. **Outlook Forecasting:** Estimating the future sales recovery trajectory if the action is taken.

---

## 2. The Failed Experiment: The Multi-Agent Pipeline
ZS Associates initially built a multi-agent system designed to mimic human analyst workflows:

```
                  ┌──────────────────────┐
                  │  Orchestrator Agent  │
                  └──────────┬───────────┘
         ┌───────────────────┼───────────────────┐
         ▼                   ▼                   ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│  Signal Agent   │ │ Root Cause Agent│ │ Synthesis Agent │
└─────────────────┘ └─────────────────┘ └─────────────────┘
```
*   **Result:** The pipeline failed because it lacked coherence.
*   **Example Failure:** The system correctly identified that sales dropped because a **payer (insurance) moved a drug to a lower coverage tier** (making it expensive for patients). However, the Synthesis Agent recommended **sending more sales reps to talk to doctors** (which doesn't solve the financial/payer problem), rendering the outlook forecast invalid.

### Why the Multi-Agent Pipeline Failed
1. **LLMs Running Deterministic Work:** The Signal Detection step (calculating trends/anomalies in data tables) was assigned to an LLM. LLMs are unreliable for statistical math, leading to noisy and hallucinated signals.
2. **Context Loss at Handoffs:** With multiple agents, context was passed sequentially. Reasoning weightage and crucial context got lost at each handoff (e.g. the Synthesis Agent lost the *why* behind the insurance tier drop).
3. **Lack of Shared Domain Knowledge:** Agents didn't understand pharma terminology (TRx, payer tiers) and relationships. They were trying to infer relationships by querying raw databases on the fly, leading to hallucinations.

---

## 3. The Solution: Bounded Single Agent + KG Control Plane
To fix these failures, ZS Associates redesigned the architecture based on three core principles:

```
┌────────────────────────────────────────────────────────┐
│               Deterministic Analytics                  │ <-- Stats & SQL (anomaly check)
└──────────────────────────┬─────────────────────────────┘
                           │ (Queues Signal)
                           ▼
┌────────────────────────────────────────────────────────┐
│                  Single Reasoning Agent                │ <-- Controls judgment end-to-end
│  (Navigates graph, spawns local tasks, queries DB)     │
└──────────────────────────▲─────────────────────────────┘
                           │
┌──────────────────────────▼─────────────────────────────┐
│             Knowledge Graph (Control Plane)            │ <-- Dictates valid paths & hypotheses
└────────────────────────────────────────────────────────┘
```

### I. Offloading Deterministic Work
- Moved **Signal Detection** completely out of the agentic pipeline. 
- A traditional, deterministic database query/script runs statistical anomaly detection and queues signals. The AI Agent only wakes up when a signal is queued, focusing entirely on *investigation* instead of *identification*.

### II. Consolidating Reasoning
- **Killed the Multi-Agent Setup:** Consolidated all judgment, reasoning, and final decision-making into a **single primary agent** to prevent context loss at handoffs.
- **Dynamic Sub-agents:** The primary agent still spins up focused sub-agents dynamically for parallel investigations (e.g., analyzing regional field rep activity), but sub-agents only return raw query results—**never** judgment or reasoning.

### III. Knowledge Graph as a Control Plane
- Built a **Knowledge Graph** representing the domain ontology (payer entities, geographies, brands, KPIs, and their relationships).
- The Knowledge Graph is used as a **Control Plane**, not just a static lookup:
  - It dictates which paths and investigation hypotheses the agent is physically allowed to evaluate.
  - Every edge in the graph represents a hypothesis (e.g. Sales Drop -> Region -> Payer -> Rep Activity). The agent traverses these edges, runs SQL/queries to verify/refute each hypothesis, and continues until it narrows down the root cause.
- **Outcome:** Bounding the agent's search space using the graph reduced hallucinations, lowered token usage, and completed investigations (taking 3–4 weeks for a human analyst) in 20–30 minutes across ~50 turns.

---

## 4. Key Takeaways for Agent Architects
1. **Don't Mimic Human Structures blindly:** Avoid introducing human organizational charts or design constraints into AI architectures (e.g. split agents just because humans have split roles).
2. **Deterministic vs. Agentic separation:** Never let LLMs handle calculations, thresholds, or statistical monitoring that can be solved deterministically with SQL or code.
3. **Single Owner of Reasoning:** Keep reasoning centralized in one agent. If you must use sub-agents, treat them as tools returning raw facts, not co-reasoners.
4. **Graphs as Bounded Surfaces:** Use a Knowledge Graph as a control plane/map to restrict the agent's exploration paths, ensuring deterministic and safe navigation.

---

## 5. Useful Search Keywords
- `multi-agent pipeline failure`, `knowledge graph control plane`, `zs associates case study`, `deterministic vs agentic`, `reasoning consolidation`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
