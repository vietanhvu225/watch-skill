# AI Tools for Forward Deployed Engineering | Varick Agents Case Study

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=l0FLhNqBOic)
- **Watch Skill ID:** `acbbc7f09f79f49d`
- **Category:** #ai-agents
- **Date Processed:** 2026-08-03
- **Duration:** 20:00
- **Speakers:** Vasuman Moza (CEO, Varick Agents) & JD Pruitt (Head of Engineering/Platform, Varick Agents)

---

## 1. The Real Bottleneck in Enterprise AI
While AI execution (writing code, using browsers, executing API calls) is largely solved by frontier models and tools like MCP, the core bottleneck has shifted:
* **The Problem:** **Understanding business processes and retrieving the correct context.** 
* Every company operates differently (e.g., Accounts Payable workflows vary wildly). Slapping AI onto broken, undocumented, or poorly understood human processes is why **87% to 95% of enterprise generative AI pilots fail to reach production**.
* **The Solution:** Forward Deployed Engineers (FDEs) who embed directly in companies to map, re-engineer, and deploy agents on top of existing **Systems of Record** (NetSuite, SAP, Salesforce, Dynamics) rather than forcing companies to migrate off them.

---

## 2. The FDE Agent (Codex for FDEs)
FDEs must possess top-tier technical skills (High IQ) alongside high EQ to interview non-technical business leads and handle 24/7 client comms. To scale this scarce talent, Varick built the **FDE Agent** in three progressive stages:

```
┌─────────────────────────────────────────────────────────────┐
│ 1. The Engagement Agent (Assistant)                        │
│    - Ingasts meeting notes, PowerPoint slides, and docs.    │
│    - Answers queries: "Who owns AP?", "Are Mike/Michael same?"│
└──────────────┬──────────────────────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. The Workflow Agent (Co-pilot)                            │
│    - Lives inside Varick's platform workspace.              │
│    - Scans workflows in real-time, pointing out gaps/edges. │
└──────────────┬──────────────────────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. The Autonomous Assistant (Future)                         │
│    - Reads client request emails (e.g. "update QC email").  │
│    - Autonomous updates DAG workflow without human FDE.     │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Technical Implementation Details

### A. The Single Source of Truth: Dependency Graphs (DAGs)
* Enterprise processes are highly linear but contain cycles (loops of approvals). 
* Varick represents a company's operations using a **Dependency Graph (DAG)** stored in PostgreSQL/Graph DB.
* This model maps clear lines of approval (e.g., Person C cannot act until Person A and B approve).

### B. Custom Model Post-Training
* Frontier models (like Claude 3.5 Sonnet) are too verbose for business analysis and fail to distinguish between critical detail and negligible noise.
* Varick post-trains/fine-tunes open-source models (such as **Hermes 2.6 / Llama-3-Hermes-2-Theta**) on top of their own custom enterprise dataset to strike the perfect balance of conciseness and clarity.

### C. RL Environment for Graph Traversal
* Searching a massive corporate knowledge graph is highly prone to error.
* Varick created a **Reinforcement Learning (RL)** environment where the agent learns to traverse the graph using custom tools:
  - `Entity Resolution Tool`: Resolve names (e.g., distinguishing multiple "Mikes" in the same company).
  - `Redundancy / Loop Detector`: Ensure workflow graph updates do not violate the DAG.

---

## 4. Business Impact
Varick delivers department-wide transformations rather than simple point solutions (which only yield 5-10% ROI):
* **Department-wide ROI:** **25% to 75%** improvements.
* **Three Pillars of Value:** Cost Savings, Revenue Uplift, and Risk Mitigation.

---

## 5. Useful Search Keywords
- `varick agents fde`, `forward deployed engineering ai`, `process mapping dependency graph`, `rl graph traversal agents`, `systems of record ai integration`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
