# AI-Hero Skills v1.2.0 | Release & Features Guide

- **Video URL:** [YouTube Link](https://www.youtube.com/watch?v=gaDdrDdczO4)
- **Watch Skill ID:** `93bc609ba570beb4`
- **Category:** #ai-agents
- **Date Processed:** 2026-08-05
- **Duration:** 11:37
- **Speaker:** Matt Pocock (AI Hero)

---

## 1. Documentation & Distribution Upgrades
The `ai-hero` skills repository has reached over 24K stars on GitHub (ranking 24th most starred repository of all time).
* **Official Docs:** Accessible at `aihero.dev/skills`, featuring structured groupings (Grill with Docs ➔ Spec ➔ Tickets ➔ Implement ➔ Code Review), FAQ wikis, and integrations with an AI coding dictionary.
* **Claude Code Integration:** Officially available in the Claude Code marketplace. Install via `plugin add mattpocock/skills` for read-only bundles and automatic updates.
* **Codeex Integration:** Added `openai.yaml` sidecar files to every skill. This allows Codeex UI to recognize user-invoked vs. model-invoked hiding rules, setting `allowImplicitInvocation: false` to prevent context-window bloating.

---

## 2. New & Updated Skills in v1.2.0

### `/wait-what` (Verbosity Cure) [NEW]
* **Problem:** Advanced LLMs (like Opus 5) frequently produce verbose, wordy explanations or use weird LLM boilerplate jargon that is hard for developers to scan.
* **Solution:** `/wait-what` forces the model to compress its output using two rules:
  1. Enforce **ASD-STE100 Simplified Technical English** guidelines (short, clear, declarative sentences).
  2. Ground its vocabulary in the project-specific ubiquitous language defined in `context.md`.

### `/grill-me` (Graph-Based Round Questioning) [UPDATED]
* **Old Pattern:** Asked one question per user turn. This was incredibly slow and frustrating when only simple confirmation questions remained.
* **New Pattern:** Questions are mapped as a **directed graph**.
* **Flow:** The agent asks critical parent questions in Round 1. Once satisfied, it opens up the child questions and groups easy ones together in batches. Emojis are used for visual navigation.

### `/writing-for-agents` [UPDATED]
* **Rename:** Formally renamed from `writing-great-skills` because it applies to *all* files consumed by LLM agents (e.g. `agents.md`, `context.md`, `skills.md`).
* **Objective:** Restructures agent-facing files to be concise, predictable, and prevent context-window frontloading.

### `/wizard` (Deterministic Local Scripts) [NEW]
* **Problem:** Allowing AI agents to provision cloud infrastructure (AWS/Azure) directly using tools like "computer use" is high-risk and slow.
* **Solution:** Generates a **deterministic local bash script wizard** that guides the human step-by-step (e.g. opens browser to AWS login, pauses to let human paste API keys, updates `.env` or GitHub Secrets locally).
* **Safety:** It runs locally and deterministically, ensuring private API keys/secrets are never transmitted back to Anthropic/OpenAI servers.

### `/questionnaire` (Stakeholder Collaboration) [NEW]
* **Objective:** Extracts the agent's technical grilling questions into a clean markdown document or Google Doc.
* **Flow:** Share the Google Doc with non-technical stakeholders (e.g., product managers or clients) to comment on, then paste their replies back to the agent in bulk. Prevents the need for stakeholders to interact with CLI/Slack agents directly.

---

## 3. Useful Search Keywords
- `ai hero skills documentation`, `claudecode plugin mattpocock`, `wait-what simplified technical english`, `grill-me graph dependency`, `wizard local bash execution`

*Run: `watch-skill search "<keyword>"` to query this video and others in your vault.*
