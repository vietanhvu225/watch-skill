# Self-Improving AI Agents | Evolving the Harness, Not the Model

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=KoDohnhLpJM)
- **Watch Skill ID:** `15657c4a3940e9c7`
- **Category:** #ai-agents
- **Date Processed:** 2026-07-16
- **Duration:** 35:19

---

## 1. Core Thesis: Model + Harness + UI
The core architecture of an AI Agent consists of three parts: **Model (weights) + Harness (code/environment wrapper) + UI**. 
- **The Concept:** While the weights of the model are frozen (training is completed), the **harness** wrapped around the model is where self-improvement and optimization should actually live.
- **Benefits:** Harness optimization is deterministic, version-controlled (readable, testable, undoable), and runs on your own schedule rather than depending on model provider updates.

---

## 2. The 5 Levels of Self-Improvement (Lilian Weng)
Lilian Weng (OpenAI researcher) defines a ladder of self-improvement for AI systems, from safest at the bottom to riskiest at the top:

```
┌─────────────────────────────────────────────────────────┐
│               Level 5: Meta-Improving                   │  <-- Improving the system that does the improving
│  ┌───────────────────────────────────────────────────┐  │
│  │             Level 4: Harness Code                 │  │  <-- Editing the harness codebase and config values
│  │  ┌─────────────────────────────────────────────┐  │  │
│  │  │              Level 3: Workflow              │  │  │  │  <-- Tuning execution steps the agent takes
│  │  │  ┌───────────────────────────────────────┐  │  │  │  │
│  │  │  │              Level 2: Context         │  │  │  │  │  <-- Optimizing RAG, data formats, compacting
│  │  │  │  ┌─────────────────────────────────┐  │  │  │  │  │  │
│  │  │  │  │         Level 1: Prompt         │  │  │  │  │  │  │  <-- Instructions, system prompts, agents.md
│  │  │  │  └─────────────────────────────────┘  │  │  │  │  │  │
│  │  │  └───────────────────────────────────────┘  │  │  │  │
│  │  └─────────────────────────────────────────────┘  │  │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```
*This video operates at **Level 4**, altering the harness configuration values surrounding the model.*

---

## 3. Case Study: Solving the Log File Truncation Failure
The creator describes an actual experiment conducted on a local agent running a frozen **Gemma** model. A second, stronger model (**Fable**) was deployed to diagnose and solve a repeated failure.

### The Bug
- **Scenario:** The local Gemma agent was asked to find a password inside a long log file. 
- **Failure Mode:** The agent constantly failed because the file parser was truncating the content at a hardcoded character limit (4,000 characters). The password sat at character 8,690.
- **Agent Behavior:** Gemma did not lie or hallucinate; it observed: *"The log ends abruptly... I cannot determine the password."* This proved the bug was in the plumbing (harness) rather than the model.

### The Self-Improving Pipeline
The system utilized methodologies from three major papers/sources: **Ariel (Ant Group)**, **Self-Harness (Shanghai AI Lab)**, and **Lilian Weng's Blog**.

1. **Weakness Mining:** Obsolescence logs were grouped to find repeated failures with a shared cause, forming a **Cluster**.
2. **Editable Surface definition:** A single JSON configuration file (35 lines, 11 values) defined the only parameters the external model (Fable) was allowed to edit. Fable could not modify codebase files.
3. **Cheating Prevention:** The test suite lived outside the editable surface. The system tracked SHA fingerprints of seeded test files. If Fable edited the answer key instead of the bug, the test failed immediately.
4. **Split-Testing (Held-in vs. Held-out):** Tasks were split into *Held-in* (shown to the fixer) and *Held-out* (hidden from the fixer) to verify generalization and prevent gaming the test.
5. **Fractional Scoring:** Small models are inconsistent. Every candidate fix was run multiple times (49 runs across 13 tasks), yielding average pass fractions (e.g. 2/3) rather than binary pass/fail.
6. **Strict Acceptance Criteria:** A fix was accepted only if it was **at least as good** as the baseline on both piles (held-in and held-out) and **strictly better** in at least one place. No partial credit.
7. **Safe Audited Deployment:** Rather than auto-merging, the successful fix (raising the log limit to 12,000 characters) generated a Pull Request (PR) on a separate branch, waiting for human review.

---

## 4. Key Limitations & Risks

- **Capability Mismatch:** Strong models (like Fable) might propose clever, nuanced harness changes or prompts that smaller local models (like Gemma) are not capable of following.
- **Hidden Pile Leakage:** Over multiple self-improving iterations, using the held-out (hidden) score to pick the winning fix gradually leaks information about the hidden pile. The system must periodically retire and replace the held-out tasks.
- **Editable Surface Boundaries:** In this experiment, the fixer was only allowed to change numbers/values. To perform structural code modifications (like implementing "compaction" instead of raising the character limit), the editable surface needs to be expanded safely.

---

## 5. Checklist for Evaluating Self-Improving AI Systems
Use this checklist to determine if a self-improving system is legitimate engineering or marketing hype:
- [ ] **Observability:** Does it find failures by looking at real execution logs?
- [ ] **Clustering:** Are failures grouped by shared root causes?
- [ ] **Regression Testing:** Is the fix tested against tasks the fixer never saw (held-out)?
- [ ] **Constrained Edits:** Is the fixer restricted to a short, named "editable surface" list?
- [ ] **No Local Cheating:** Does it prevent the fixer from altering the test suite or answer keys?
- [ ] **Staged Deployment:** Does it require a human review path (like an open PR) rather than auto-merging?
- [ ] **Self-Explanation:** Can the system clearly explain what changed and provide performance numbers?

---

## 6. Useful Search Keywords
- `harness optimization`, `self-improving agents`, `lilian weng ladder`, `ariel paper`, `self-harness paper`, `held-out validation`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
