# Spotify Portal `shunt` — Context-routing finding

- **Reviewed:** 2026-09-07
- **Status:** Deferred — borrow the mechanism, do not adopt the implementation
- **Decision owner:** Andy
- **Scope:** Andy Development Loops / coding-agent harnesses; not a DXP rollout decision
- **Revisit trigger:** repeated, measured cost or context-pressure pain on large discovery work, after a local/private worker lane is available

## Decision

**Do not install or adopt Spotify Portal / AiKA `shunt` as-is.** It depends on a Portal instance with AiKA enabled and the `shunt` plugin is Claude Code-only. Instead, retain one transferable design rule:

> Put expensive I/O routing in an enforceable execution policy, not in advisory instructions; keep exact reading, reasoning, edits, and acceptance with the primary agent and human owner.

This is a **deferred hypothesis**, not an approved implementation item.

## What the source actually demonstrates

Spotify's article describes a three-layer plugin for Claude Code: PreToolUse hooks block broad reads of large files; skills redirect the agent; scripts invoke lower-cost AiKA modes for bulk reading and boilerplate generation. The claimed purpose is to keep large source corpora and generated code out of Claude's expensive context while reserving Claude for reasoning, debugging, architectural judgment, and exact edits.[1]

The source is unusually honest about its boundary: summaries are not reliable enough for editing, a worker missed a subtle thread-safety bug, and remote delegation adds latency.[1]

The open-source repository contains the claimed hooks, scripts, skills, transport tests, and benchmark fixtures. Its root Portal plugin supports Claude Code, Codex, and Cursor; `shunt` itself is currently Claude-only.[2]

## Mechanisms worth borrowing

1. **Hard policy over prompt convention.** A guard that intercepts broad reads is more reliable than “remember to use a cheap model” in a project instruction file.
2. **Discovery is distinct from change authority.** A worker may summarize a broad corpus; the primary agent must re-read exact evidence before an edit or conclusion with material consequences.
3. **Scripts are a stable tool contract.** Named inputs such as question, paths, reference, target, explicit payload limits, timeouts, and structured errors are safer than asking an agent to assemble shell commands from prose.
4. **Reference-grounded boilerplate only.** Generated tests/config/stubs should start from an existing reference pattern, never from a bare speculative prompt.
5. **Route separately from model choice.** The policy decides *when* work may be delegated; a replaceable adapter decides *how* and to which model.

## Non-negotiable changes if this is ever trialed

A local Context Router must not copy `shunt` literally:

- **No direct write to source of truth by a lower-cost worker.** Write a draft/patch/sidecar artifact only. A primary agent and human approval apply it.
- **Evidence-carrying summaries.** Return source path, symbol, exact range/quote, and explicit uncertainty; a summary alone is not edit authority.
- **Privacy boundary by default.** Product code must not be sent to an external worker unless the specific provider/data policy is approved.
- **Measure the whole trade-off.** Record total cost, latency, summary fidelity, rework, review findings, and defect escapes — not just primary-agent context tokens.
- **Keep DXP human-gated.** This could become an opt-in personal development-loop experiment first, never a silent team-wide enforcement hook.

## Local verification receipt

Checkout inspected: `D:\source\personal\portal-ai-plugins` at `3c24ca30ff63e1f5bbad1c43fe5324daff579123` (`feat: add Portal CLI feedback workflow`, 2026-08-17). Working tree was clean.

- Bash syntax checks for hooks and scripts: passed.
- Plugin JSON manifests: parsed successfully.
- Plugin creator validator: passed.
- `claude plugin validate --strict .`: passed.
- Full shipped eval runner: not run because `jq` is missing on this Windows host; `jq` is a declared runtime prerequisite.

### Windows-specific blocker

The implementation assumes a non-Linux payload limit of 400 KB while passing the request through a command-line argument. On this Windows host, a native Node process accepted a 30,000-byte argument but failed at 33,000 bytes with `WinError 206`; 400,000 bytes also failed. A future Windows implementation must use stdin/file/IPC rather than a large argv payload, or set a conservative Windows-specific ceiling.

## Why the 90% claim is not a rollout metric

The article and repository report 82–94% savings for bulk reads and a 90% mean.[1][2] The repository benchmark estimates tokens as characters divided by four, uses limited fixtures, and requires authenticated Portal/AiKA access for a real end-to-end run. Treat the number as a hypothesis about avoided primary-model context, not proof of lower total engineering cost or better delivery quality.

## Revisit criteria

Open a bounded trial only when all conditions hold:

1. Large-file/multi-file discovery is a measured recurring cost or context-pressure problem.
2. A local or approved private worker endpoint exists.
3. The experiment is isolated from DXP delivery and source changes remain human-gated.
4. The trial has a fixed corpus, baseline, budget, latency ceiling, and fidelity/rework acceptance criteria.
5. The router can pass exact evidence and use a Windows-safe transport.

## Sources

[1] https://engineering.atspotify.com/2026/9/portal-by-spotify-cut-my-claude-code-token-usage-by-90 — *Portal by Spotify cut my Claude Code token usage by 90%*

[2] https://github.com/spotify/portal-ai-plugins — *spotify/portal-ai-plugins*
