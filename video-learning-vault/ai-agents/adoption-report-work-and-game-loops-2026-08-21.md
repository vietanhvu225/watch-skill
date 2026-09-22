# AI Agents Research — Adoption Report for Work and Game Loops

- **Reviewed:** 2026-08-21
- **Scope:** 21 analysis notes and their transcripts under `ai-agents/`
- **Decision lanes:** current engineering workflow; Broken Studio / browser-game execution with an unattended loop
- **Incumbents considered:** Andy Development Loops; Codex + OpenSpec planning; Broken Studio DNA handoff and architecture contracts; LoopX; Orca; manual Codex review

## Evidence boundary

This folder is primarily a **video-learning corpus**, not a collection of 21 independently verified repositories. The table below treats each note as a summarized claim source. It does not silently promote a video claim into runtime evidence.

I deep-checked only the strongest runnable candidate, **Herdr**, against its current upstream source and Windows release. I also confirmed that Cognee is a current Apache-2.0 repository, but did not trial it because mutable memory is already deferred in the current game/tooling direction.

## Independent triage reconciliation

Three independent review lanes evaluated seven notes each after the main report draft. They converged on the same high-priority mechanisms: one reasoning owner, deterministic playable verification, Target/Budget/Stall stops, bounded actions, and skill evals.

The meaningful differences were reconciled deliberately:

- The independent Herdr verdict relied on the old v0.7 video summary and rated it only `borrow`. Current v0.8.2 source plus the real Windows smoke supersedes that stale boundary and justifies `trial`.
- Some lanes rated Graph Engineering, Art of Loop Engineering, and AI FDE as `adopt` for the game loop. They remain downgraded here because their useful task-graph/eval/ownership mechanics are already covered by OpenSpec, LoopX, Herdr/Orca, and Broken Studio DNA; adopting their whole topology would add a second control plane.
- Cognee remains a product `skip`, but its compact provenance schema—owner, timestamp, authority, status, stable identity, and supersedes links—is worth borrowing into the existing file/event ledger.

Verdict legend:

- **Trial** — run one isolated proof before adoption.
- **Adopt practice** — encode the mechanism in the existing workflow; no new platform required.
- **Borrow** — take one bounded rule/artifact only.
- **Park** — useful only after a named trigger.
- **Skip** — no current fit or duplicates a stronger incumbent.

## Executive verdict

The corpus contains many useful ideas, but it does **not** justify installing a large graph, memory platform, or second planning framework.

The strongest actions are:

1. **Trial Herdr for game execution.** It is now a credible Windows-native, terminal-first alternative to Orca for persistent CLI agents, isolated Git worktrees, agent status, session resume, and script/agent control.
2. **Adopt a deterministic playable-loop verifier.** Build, primary input, objective progress, fail/restart, console errors, nonblank screenshot, and a fixed seed/state must be the machine gate before an unattended game task can finish.
3. **Use `Target + Budget + Stall` as the stop contract.** This is the missing bridge between an approved OpenSpec plan and a safe long-running LoopX/Codex execution.
4. **Keep one owner of reasoning.** Multi-agent workers may return facts, independent implementations, or reviews; they must not pass product judgment through a long chain of agents.
5. **Add evals before adding more skills.** Trigger-positive, trigger-negative, behavioral assertions, and ablation are more valuable now than another skill pack.

## Full comparison table

| # | Note / candidate | What it mechanically contributes | Current engineering flow | Game + unattended loop | Exact piece worth keeping | Main caveat |
|---:|---|---|---|---|---|---|
| 1 | [Why We Killed Our Multi-Agent Pipeline](why_we_killed_multi_agent_pipeline.md) | Deterministic sensing, one reasoning owner, graph-bounded hypotheses, subagents returning facts | **Borrow** | **Adopt practice** | Keep calculations/checks outside the model; one executor owns end-to-end judgment | Domain KG is expensive and unnecessary for a game plan that already has clear tasks |
| 2 | [Wayfinder](wayfinder.md) | Multi-session **decision** map: frontier, blocked decisions, fog, ticket claims | **Park** | **Park / skip after plan lock** | Use frontier/fog only when a feature is too uncertain to specify in one session | Codex + OpenSpec already own planning; Wayfinder explicitly plans rather than executes and would create a second tracker |
| 3 | [Verifiers Are King](verifiers_are_king.md) | Guide → deterministic/agentic verify → maintain; verifier-first SDLC | **Borrow** (mostly existing) | **Adopt practice** | Build the playable evidence gate before increasing autonomy | Vendor metrics in the note are not independently verified; an AI review is not a substitute for executable proof |
| 4 | [Top 1% Tech Advice](top_1percent_tech_advice.md) | Career time-boxing, deliberate stretch work, teaching/review | **Skip as agent tooling** | **Borrow for personal cadence** | Reserve fixed second-job hours and share-before-perfect | Career advice, not an agent architecture or runnable tool |
| 5 | [How to Build Loops](senior_google_ai_pm_how_i_build_loops.md) | Nine-part loop contract and Target/Budget/Stall stop trio | **Borrow** | **Adopt practice — P0** | Goal, context, allowed actions/tools, evals, memory, guardrails, escalation, stop | A checklist is not a runner; LoopX/Codex must enforce it mechanically |
| 6 | [Self-Improving Agents](self_improving_agents.md) | Failure clustering, constrained editable surface, held-in/held-out tests, champion promotion | **Park** | **Park until repeated failures exist** | Improve the harness only from real failure logs; hidden tests remain outside the edit surface | Premature without a corpus; repeated use leaks the holdout and structural edits are much riskier than config tuning |
| 7 | [AI-Hero Skills v1.2](new_skills_v12.md) | Small composable skills such as `wait-what`, updated grilling and writing support | **Borrow selectively** | **Skip for execution** | `wait-what` is a useful human correction primitive; retain the existing grilling direction | Installing a whole skill pack adds overlap; it does not make an approved plan run unattended |
| 8 | [MCP Apps](mcp_apps_extending_frontier.md) | Interactive UI resources embedded in an MCP host | **Park for product UX research** | **Skip for game execution** | Remember “tools may need a visual interaction surface” for future Copilot product work | It does not coordinate coding agents, persist worktrees, or verify gameplay |
| 9 | [Loop Engineering from First Principles](loop_engineering_first_principles.md) | Deterministic sensor, golden implementation pattern, one small PR, no next run while one remains open | **Adopt practice for repetitive migrations/cleanup** | **Borrow** | One open item/PR per maintenance loop; new violations blocked deterministically | Not the right loop for a whole feature or authored gameplay judgment |
| 10 | [Loop Engineering Explained](loop_engineering_explained.md) | High-level loop anatomy and iterative site example | **Skip** | **Skip** | No unique mechanism beyond the stronger loop notes | Introductory synthesis; duplicates items 5, 9, and 18 |
| 11 | [Herdr](herdr_terminal_multiplexer.md) / [upstream](https://github.com/herdrdev/herdr) | Rust terminal runtime, Windows ConPTY, worktrees, Codex/Agy/Grok/Hermes state, prompt/wait/read, detach/reattach, native session resume | **Trial personally; do not insert into DXP yet** | **Trial — P0** | Use it as the execution substrate for one or two isolated game workers; compare with Orca | It is not LoopX: no objective/quota/eval ledger; no embedded browser/diff/playtest; screen-based blocked detection can misclassify |
| 12 | [Harness Engineering](harness_engineering.md) | Small `AGENTS.md`, tool/context design, eval-aware harness tuning | **Borrow** (already mature) | **Borrow** | Keep game guidance as a map plus on-demand contracts, not an encyclopedia | Broad conceptual guidance; most of the current flow already implements it |
| 13 | [Graph Engineering](graph_engineering_10x_claude.md) | Planner → independent workers → skeptic → merger → human gate | **Park for research graphs** | **Skip as canonical game implementation** | Parallelize independent evidence collection only; one artifact still has one owner | Context handoffs, cascading errors, cost, and majority-vote architecture conflict with the current ownership rule |
| 14 | [Google OKF](google_okf.md) | Git-backed Markdown bundle, `type` metadata, indexes, links, progressive disclosure | **Borrow only if a consumer needs the format** | **Skip** | Stable definitions belong in linked Markdown; volatile facts belong behind tools/APIs | Broken Studio DNA and repository guidance already use linked Markdown; OKF would mostly rename an existing practice |
| 15 | [Don't Ship Skills Without Evals](dont_ship_skills_without_evals.md) | Positive/negative trigger evals, cheap assertions, LLM-judge only when needed, ablation | **Adopt practice — P0** | **Adopt practice — P0** | Every new workflow/game skill needs trigger and behavioral evals; retire no-op capability skills | The numerical claims are summary-level; build local evals against Andy's real tasks rather than universal targets |
| 16 | [Dangerously Self Educated](dangerously_self_educated.md) | Avoid the “AI passenger”; explain and redraw the system from memory | **Borrow for Learning loop** | **Adopt human learning gate** | Each learner explains state, frame/turn update, input→action, success/fail, renderer boundary | It intentionally increases human involvement; it is not an unattended-execution mechanism |
| 17 | [Cognee Memory](cognee_memory.md) / [upstream](https://github.com/topoteretes/cognee) | Persistent graph/vector memory platform | **Skip product; borrow schema** | **Skip product; borrow schema** | Add owner, timestamp, authority, status, stable ID, and `supersedes` links to the existing ledger; do not add Cognee | Mutable memory was deliberately deferred; the platform adds a second truth plane and operational surface |
| 18 | [Art of Loop Engineering](art_of_loop_engineering.md) | Nested loops, HITL, eval-driven improvement | **Borrow vocabulary only** | **Borrow vocabulary only** | Evals and human escalation wrap the execution loop | Conceptual overlap; no new runner or game-specific verifier |
| 19 | [Anthropic / Graph Second Opinion](anthropic_fixed_graph_engineering_flaw.md) | Fresh independent validator; browser screenshot checks; separate review lenses | **Borrow** | **Adopt one bounded final verifier** | Freeze target, run one fresh verifier on diff + playable evidence, then stop | Do not fan out review indefinitely or let readability findings reopen a clean defect review |
| 20 | [Ambitious but Inconsistent](ambitious_but_inconsistent.md) | Consistency and frustration-tolerance advice | **Skip as tooling** | **Borrow for scheduling only** | Prefer one fixed small game commitment over collecting frameworks | Personal-development content, not an agent/runtime design |
| 21 | [AI FDE / Varick](ai_fde_varick.md) | Context/tooling for enterprise system integration and forward-deployed engineering | **Park for Engage AI product research** | **Skip** | Potential later lesson for connecting agents to systems of record | Wrong domain/object for browser-game execution; product claims were not source-tested here |

## Priority shortlist

### P0 — Do now or trial next

| Candidate | Action | Why it survives the overlap check |
|---|---|---|
| **Herdr** | One isolated Windows game-repo trial | Adds an uncovered runtime surface: persistent terminal agents + worktree fan-out + machine-addressable agent state. It may be a lighter fit than Orca when the goal is “leave Codex/Agy running and return later.” |
| **Playable-loop verifier** | Encode in the game repo before unattended execution | Neither Herdr, Orca, nor LoopX knows whether a game is playable. This is the actual completion oracle. |
| **Target / Budget / Stall** | Add to every long-running execution contract | Prevents the “treo máy” run from becoming unlimited retries or token burn. |
| **Skill evals** | Add evals before another workflow skill | Current risk is accumulated instruction overlap, not lack of available skills. |

### P1 — Borrow into the contract

- One owner of reasoning; subagents return facts or independent artifacts.
- Deterministic sensor before model work.
- One fresh final verifier on a frozen target.
- One open maintenance PR/item at a time.
- Human explain-back for the three learners.

### Park

- **Wayfinder** until there is a goal that cannot yet become an OpenSpec in one session.
- **Self-improving harness** until at least several repeated, classified failures exist.
- **MCP Apps** until a product needs interactive agent UI.
- **Cognee** until retrieval from Git-backed files demonstrably fails.
- **Graph engineering** for research-only experiments, not canonical implementation.

## Recommended game + loop topology

### Mode A — One approved plan, one unattended executor

```text
Locked feature + OpenSpec plan
  → one reasoning owner (Codex or Agy)
  → optional Herdr terminal host / LoopX durable tick state
  → deterministic playable-loop verifier
  → one fresh independent final verifier
  → Andy: 10-minute playtest and accept/reject
```

Use **LoopX** when the important problem is durable objective/todo/quota/evidence across multiple turns. Use **Herdr** when the important problem is keeping/resuming terminal agents and addressing them programmatically on Windows. Do not combine them in the first experiment; promote the pair only if a real run needs both properties.

### Mode B — Two implementations race against one locked target

```text
Same locked OpenSpec
  ├─ Codex @ worktree A
  └─ Agy/Grok @ worktree B
      ↓ same verifier and same evidence schema
  → compare two receipts
  → human selects one winner; loser is not merged wholesale
```

Use **Herdr** for terminal-first persistence and scriptable `agent prompt/wait/read`. Use **Orca** when its graphical diff, browser, computer-use, and richer desktop review surface are the deciding benefit. The agents share a target, not a checkout.

### Machine verifier required for either mode

At minimum:

1. install and production build;
2. first actionable screen;
3. primary input path;
4. objective/score/progress state change;
5. fail and immediate restart;
6. browser console/page errors;
7. visible/nonblank playfield screenshot;
8. desktop and mobile viewport sanity;
9. deterministic seed or state hook;
10. changed files, commands, exit codes, screenshot paths, known gaps.

The human review can be reduced to product judgment and a short playtest. It cannot be reduced to zero because “fun”, authored betrayal, and target feeling are not executable assertions.

## Herdr evidence receipt

- Upstream: `herdrdev/herdr`, Apache-2.0, current source HEAD inspected: `624dfd4796559042ec13ccf4d4b54374902ab81d`.
- Current package/release inspected: `0.8.2`; official Windows ZIP published 2026-08-19.
- Portable ZIP SHA-256 computed locally: `0ab3d0fe1434d55757997542b978c771d642987bb15a7130f4160f0db38821d5`. No published checksum asset was visible in the latest release asset list, so this is a local receipt, not an upstream authenticity proof.
- `herdr.exe --version` and `--help`: passed on this Windows host.
- Isolated temp-profile server: started; reported version `0.8.2`, protocol `20`, compatible socket.
- Runtime smoke: workspace create/list passed; two Git worktree workspaces (`race-codex`, `race-agy`) were created, listed, then safely removed; pane command execution wrote and verified a filesystem sentinel.
- Source confirms supported agent kinds include `codex`, `agy`, `grok`, `hermes`, `claude`, `opencode`, and others. It also exposes `agent start`, `prompt`, `wait`, `read`, and native session resume for Codex/Agy/Grok/Hermes.
- Important limits: a full source test did **not** run because the local build requires Zig for vendored `libghostty-vt`; the failure was `program not found`, not a test failure. No paid-model agent was launched, so Codex/Agy lifecycle detection and resume remain unexercised in this receipt.
- Operational nuance found during smoke: worktree checkout location uses Herdr's worktree setting/default, not the isolated config path automatically. The default placed the two disposable checkouts under `C:\Users\AnhVuViet\.herdr\worktrees`; both were removed after the test. Configure `[worktrees].directory` explicitly before a real pilot.
- `pane wait-output` can match echoed command text already in the terminal; use filesystem/test receipts or a result pattern that cannot appear in the submitted command when proving execution.

## Final recommendation

**Do not adopt the corpus as a stack.**

Adopt one operating principle and trial one product:

- **Principle:** deterministic playable evidence + Target/Budget/Stall + one reasoning owner.
- **Product trial:** Herdr on one disposable browser-game repo, against Orca using the same two-worker race and the same evidence schema.

Choose Herdr if persistence, scripting, agent state, and low overhead win. Choose Orca if graphical review, embedded browser/computer use, and diff UX materially reduce the final human check. Choose LoopX only when the job needs durable multi-turn control state rather than a better terminal runtime.
