# Don't Ship Skills Without Evals | Google DeepMind Case Study

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=0vphxNt4wyk)
- **Watch Skill ID:** `8c9cbc216c898efe`
- **Category:** #ai-agents
- **Date Processed:** 2026-08-04
- **Duration:** 21:25
- **Speaker:** Philipp Schmid (Gemini API & Agents Team, Google DeepMind)

---

## 1. The Core Problem: Untested Skills
Agentic skills (instructions/context guides) on average improve LLM performance by **~15%** (based on Skill Bench 1.1). However, of the 50,000+ skills indexed on GitHub, almost none have evaluations (evals).
* **The Risk:** Untested AI-written skills can actually **degrade performance**, over-trigger, or waste token costs.
* **Non-Determinism:** Because LLMs are non-deterministic, without evals, you cannot tell if a failure is due to a poorly written skill or because the task itself is too difficult.

---

## 2. Anatomy of a Skill: Progressive Disclosure
To prevent context bloating and high token costs, skills should be structured in three layers:

```
┌────────────────────────────────────────────────────────┐
│ Layer 1: Metadata (Title & Description)                │ <-- Always in initial context (100-200 tokens)
├────────────────────────────────────────────────────────┤
│ Layer 2: Skill Body (Instructions & Constraints)       │ <-- Loaded ONLY when triggered (< 500 words)
├────────────────────────────────────────────────────────┤
│ Layer 3: External Reference Files                      │ <-- Explored by agent as-needed (e.g. AWS vs Azure)
└────────────────────────────────────────────────────────┘
```

---

## 3. Capability vs. Preference Skills
Philipp differentiates between two main types of skills:

| Feature | Capability Skills | Preference Skills |
|---|---|---|
| **Purpose** | Teach models tasks they cannot do consistently yet (e.g. log tracing). | Encode company-specific styles, workflows, or private domain knowledge. |
| **Durability** | **Temporary.** As foundation models improve, these skills become redundant. | **Durable.** Foundation models will never natively integrate private workflows. |
| **Retirement** | Retired once model baseline matches skill performance. | Protected by evals to prevent regression across agent upgrades. |

---

## 4. 8 Rules for Writing High-Quality Skills
1. **Write Bounded Descriptions:** Descriptions must clarify *why*, *how*, and *when* to use the skill to prevent model confusion.
2. **Write Directives, Not Essays:** Use active/imperative voice (e.g., *"Use the Interactions API for chat"*) rather than passive recommendations (*"Interactions API is recommended"*).
3. **Keep it Lean:** Keep the `skills.md` file **below 500 words**. Layer detailed context in external reference files.
4. **Define Goals, Not Workflows:** Set goals and constraints, not step-by-step workflows. If the path is static, write a script instead of wasting LLM reasoning tokens.
5. **Enforce Negative Cases:** Explicitly define when the agent should *not* trigger the skill to prevent over-triggering.
6. **Test Early (10-20 Prompts):** Write 5 happy path prompts (where it must trigger) and 5 negative path prompts (where it must not trigger).
7. **Kill No-Ops (AI Fluff):** Remove useless instructions (e.g., *"write clean code"* or *"make variable names readable"*). They change zero model behavior but bloat token costs.
8. **Ablation Testing (A/B Test):** Run evals with and without the skill. If performance matches without it, retire the skill to save maintenance overhead and token costs.

---

## 5. Google DeepMind Internal CI Pipeline
Google DeepMind treats skills as code and gates merges with automated evals:

```
┌──────────────┐      ┌───────────────┐      ┌──────────────┐      ┌──────────────┐
│  Skill Diff  │ ───> │ CI Workspace  │ ───> │ Run Evals    │ ───> │ Block / Merge│
│  Submitted   │      │ (Preload Libs)│      │(Regex / LLM) │      │  on Evals    │
└──────────────┘      └───────────────┘      └──────────────┘      └──────────────┘
```

* **Test Harness:** Uses a JSON/YAML file containing the prompt, language, `should_trigger` boolean, and expected assertions.
* **Cheap Assertions:** Uses **Regex** check-scripts to verify correct SDK usage, method calls, and prohibited patterns. It is extremely fast and cost-effective compared to LLM-as-a-judge.
* **LLM-as-a-judge:** Exclusively reserved for complex traces where the entire trajectory must be graded against a structured rubric.
* **CI Integration:** Evals run on every pull request containing skill changes. Merges are blocked if they do not improve or maintain eval scores.

---

## 6. Useful Search Keywords
- `dont ship skills without evals`, `capability vs preference skills`, `skill bench leaderboard`, `ablation test coding agent`, `llm no ops removal`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
