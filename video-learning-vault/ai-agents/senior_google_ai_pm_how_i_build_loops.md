# How to Build Loops | Senior Google AI PM Guide to Loop Engineering

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=ew6gBJNzC5w)
- **Watch Skill ID:** `9acffc384b62f6a9`
- **Category:** #ai-agents
- **Date Processed:** 2026-08-05
- **Duration:** 34:23
- **Speaker:** Senior Google AI Product Manager

---

## 1. What is Loop Engineering?
Running the same prompt 10 times is not a loop; it is a slot machine where you re-roll and hope for a better result. 
**Loop Engineering** is the discipline of automating the manual chatbot workflow (write -> review -> correct -> retry) by encoding quality checking, memory, boundaries, and stop triggers into a systematic AI agent pipeline.

### The Mental Model: The Junior Employee
Think of a loop like a junior intern:
* **Infinite Patience:** They will happily run a task 400 times without getting bored, tired, or sloppy.
* **Zero Judgment:** They have no innate understanding of what "good" looks like. You must explicitly encode quality criteria and rubrics.

---

## 2. The 9 Anatomy Parts of a Reliable Loop
Every robust AI loop needs these nine components specified before execution:

```
┌──────────────────────────────────────────────────────────────────────────┐
│                                1. GOAL                                   │
├───────────────┬────────────────┬────────────────────────┬────────────────┤
│  2. CONTEXT   │   3. ACTIONS   │        4. TOOLS        │    5. EVALS    │
├───────────────┼────────────────┴────────┬───────────────┴────────────────┤
│   6. MEMORY   │      7. GUARDRAILS     │         8. ESCALATION          │
├───────────────┴─────────────────────────┴────────────────────────────────┤
│                          9. STOP CONDITION                               │
└──────────────────────────────────────────────────────────────────────────┘
```

1. **Goal:** A measurable, concrete finish line (e.g., *“average rubric score of 4.5/5”*, not *“make replies sound good”*).
2. **Context:** Onboarding documents, style guides, past examples, schemas, and constraints.
3. **Actions:** Bounded, micro-moves allowed per round (e.g., *“edit a single file”*, *“propose one change”*).
4. **Tools:** Interfaces the agent can touch (e.g., browser, spreadsheet, database, API client).
5. **Evals:** Concrete judges of the output (binary tests, strict rubrics). Vague grades (e.g. *“rate 1 to 10”*) are easily gamed.
6. **Memory:** Lessons carried between iterations to prevent repeating the same failures.
7. **Guardrails:** Lines the agent cannot cross (e.g., *“never email a customer directly”*, *“never modify the pricing page”*).
8. **Escalation (Human-in-the-Loop):** Knowing when to raise a hand and wait for human approval (e.g., financial or irreversible decisions). Escalation is a design feature, not a failure.
9. **Stop Condition:** A trio of stopping triggers:
   * **Target:** Goal achieved.
   * **Budget:** Financial or token cap reached.
   * **Stall Rule:** Diminishing returns detected (e.g., 3 rounds with no uptick).

---

## 3. Five Loop Case Studies

### Loop 1: Self-Improving Champion Loop (Prompt Optimizer)
* **Objective:** Iterate and improve a support reply prompt.
* **Train/Test Split:** 40 customer cases split into **25 improvement cases** (where the agent iterates) and **15 holdout cases** (hidden from the agent, used for final evaluation to prevent overfitting).
* **The Rule:** Challenger proposed prompts are promoted to "Champion" *only* if they beat the champion's holdout average score. Ties go to the incumbent.
* **Results:** Rejects overfitting changes (like adding massive examples that score high on training but drop holdout quality) and locks in real, generalizable prompt improvements.

### Loop 2: Empathy Simulation Loop (Qualitative Researcher)
* **Objective:** Cluster interview transcripts/tickets into pain points.
* **Eval Check (Saturation):** Did the latest batch of 20 transcripts create new clusters, or just add weight to existing ones?
* **Stop:** 2 consecutive batches with zero new clusters (saturation reached) or a cap of 10 batches.

### Loop 3: Devil's Advocate Loop (Concept Hardener)
* **Objective:** Harden a PRD or design doc before sharing.
* **Role Separation:** Uses two distinct models:
  * **Builder:** Generates/revises the document.
  * **Critique:** Exclusively pokes holes and attacks assumptions.
  * *Pro Tip:* Use different models (e.g., Claude for building, Gemini for critiquing) because they have different personalities. Never let a model grade its own work.
* **Stop:** No high-impact objections remain, or the Critique starts repeating itself.

### Loop 4: Product Walking Loop (UX Auditor)
* **Objective:** Walk user flow in a clean session, capture screens, and evaluate visual/UI hierarchy.
* **Stop:** Target UX score met, or 2 passes with no gain.
* **Guardrail:** Pricing or legal copy changes halt the loop immediately and trigger human escalation.

### Loop 5: Refund Follow-up Loop (Back Office Admin)
* **Objective:** Automatically email/chase service providers for refunds.
* **Process:** Files claims, tracks vendor deadline dates, follows up on day 16 (if wait time was 15 days), cites policy documents.
* **Stop:** Refund arrived OR situation is blocked (requires legal/human step), causing escalation.

---

## 4. Five Predictable Failure Modes & Fixes

1. **Drift (Goal Wandering):**
   * *Problem:* The agent slightly wanders off course to satisfy a specific check (e.g., shortening emails to pass a length constraint, eventually writing empty emails).
   * *Fix:* Restate the core goal in every round's context, and monitor log trends rather than just scores.
2. **Weak Evals (Gaming):**
   * *Problem:* Asking the model to grade itself on a loose scale (1-10) leads to inflated scores.
   * *Fix:* Implement strict binary checks (Yes/No) + human spot checks.
3. **Cost Multiplication:**
   * *Problem:* Running multiple cases over several rounds multiplies token spend.
   * *Fix:* Pre-define a hard dollar/token cap in the stopping condition.
4. **Latency:**
   * *Problem:* Complex loops take 30-60 minutes to complete.
   * *Fix:* Run them overnight, and read the logs in the morning.
5. **Endless Retries:**
   * *Problem:* Setting unreachable target thresholds causes the loop to run forever.
   * *Fix:* Always enforce the **Target-Budget-Stall** trio. Stop if no uptick is observed after 3 rounds.

---

## 5. Useful Search Keywords
- `loop engineering checklist`, `champion challenger loop prompt`, `eval saturation qualitative research`, `different models builder critique`, `target budget stall stop condition`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
