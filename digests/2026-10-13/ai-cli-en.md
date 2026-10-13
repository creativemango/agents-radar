# AI CLI Tools Community Digest 2026-10-13

> Generated: 2026-10-13 03:08 UTC | Tools covered: 7

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://github.com/openai/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [OpenCode](https://github.com/anomalyco/opencode)
- [Pi](https://github.com/earendil-works/pi)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [Claude Code Skills](https://github.com/anthropics/skills)

---

## Cross-Tool Comparison

# Cross-Tool AI CLI Comparison Report — 2026-10-13

## 1. Ecosystem Overview

The AI CLI landscape is shifting from terminal chat toward production-grade agent runtimes. Across Claude Code, Codex, Gemini CLI, Copilot CLI, OpenCode, Pi, and Qwen Code, the highest-engagement threads are now about session durability, permission correctness, cost governance, multi-agent orchestration, and cross-platform reliability rather than raw model quality alone. Release cadence is uneven: Copilot CLI, Pi, Gemini CLI, and Qwen Code shipped updates; Claude Code, Codex, and OpenCode were release-quiet but issue/PR-active. Enterprise concerns — managed config, BYOK routing, MCP auth, sandbox policy — now sit alongside individual pain points like undo, terminal rendering, and clipboard behavior. The common signal is that agentic workflows have graduated from demos to systems that need rollback, observability, and enforceable policy.

## 2. Activity Comparison

| Tool | Issues (24h) | PRs (24h) | Discussions (24h) | Release status |
|---|---:|---:|---:|---|
| Claude Code | 10 highlighted | 0 updated | N/A* | None in 24h |
| OpenAI Codex | 50 updated | 24 updated | 4 | None in 24h |
| Gemini CLI | 14 highlighted (10 hot + 4 also active) | 15 highlighted/merged | N/A* | v0.65.0-nightly.20261013.g51617c1bc |
| GitHub Copilot CLI | 35 updated (30 shown) | 0 updated | N/A* | v1.0.97-2, v1.0.97-1, v1.0.97-0, v1.0.96 |
| OpenCode | 10 highlighted | 10 highlighted | N/A* | None in 24h |
| Pi | 10 highlighted | 11 highlighted | 1 | v1.2.0 |
| Qwen Code | 10 highlighted | 10 highlighted | N/A* | v0.25.1-nightly.20261012.18f80f5666 |

\*N/A = no discussion data provided in the source digest; this is not a count of zero and does not necessarily mean discussions are disabled.

## 3. Shared Feature Directions

| Theme | Tools | Specific needs |
|---|---|---|
| Session safety, rollback, resume | Codex, Claude, OpenCode, Pi, Copilot, Gemini, Qwen | `/undo`, rollback of untracked/uncommitted work; prevent context loss after compaction; resume after overflow/retries; restore dynamically activated tools; cancellation provenance; restart-proof recovery. |
| Permission/sandbox/hook precedence | Claude, Gemini, Copilot, OpenCode, Pi, Codex | Explicit precedence: hooks > settings > guard heuristics; fix over-broad worktree/sandbox refusals; enforce per-command and webfetch deny rules; secure session directories; restore saved thread permissions. |
| Model/provider fallback and capacity | Codex, OpenCode, Claude, Copilot, Pi | Cross-model failover; capacity errors despite quota; BYOK short-name and sub-agent routing; auto-mode classifier fallback; key-aware OpenRouter catalogs. |
| Cost governance and usage observability | Claude, Codex, Copilot, Pi, Gemini | Enforce agent/concurrency limits; budget caps and spend visibility; configure usage-limit warnings; prompt caching; model-call OTel; token-frugal context. |
| Multi-agent durability/observability | Qwen, Gemini, Codex, Claude, Pi, Copilot | Agent identity/team messaging; shared memory; subagent status and trajectories; MCP process leak isolation; concurrency caps; per-request taskId/purpose; BYOK sub-agent defaults. |
| Managed/enterprise config and auth | OpenCode, Copilot, Codex, Qwen | System/managed config discovery on Windows/Linux/macOS; managed policy resilience; Entra/WAM auth; enterprise deprecation banners; authenticated-account visibility. |
| Cross-platform reliability | Codex, Claude, Copilot, OpenCode, Pi, Qwen, Gemini | Windows/WSL/macOS/Linux startup, auth, clipboard, shell path, packaging, kernel/resource leaks, Wayland, ripgrep SIGSEGV. |
| MCP/tool ecosystem scaling | Gemini, Copilot, Codex, Qwen, OpenCode, Claude | Tool-count ceilings above ~128; MCP OAuth/protocol currency; durable MCP lifecycle; per-invocation server exclusion; OAuth resource encoding; stale auth state. |
| Silent-failure and correctness | Claude, Gemini, Pi, OpenCode, Copilot, Codex | LSP gitignore filtering; Bash backslash collapse; UTF-16/32 emptiness; NAME_MAX failures; omitted directory entries; dropped session defects; image attachment loss; duplicate Host headers. |
| Terminal/TUI UX | Gemini, Pi, Copilot, OpenCode, Qwen, Codex | tmux flicker/OSC52; resize refresh; debug console layout; clipboard reliability; terminal state restore; mouse/focus escape leakage; keyboard history; inline images. |

## 4. Differentiation Analysis

| Tool | Feature focus | Target users | Technical approach |
|---|---|---|---|
| Claude Code | File-write capability, desktop/sandbox permissions, hooks, plugin API, cost guardrails | Anthropic/terminal+desktop teams | First-party model, desktop/CLI parity, policy hooks, worktree/sandbox enforcement |
| OpenAI Codex | Session safety/undo, daemon opt-in, model catalog/capacity, browser/Computer Use, Windows | OpenAI/ChatGPT power users, enterprise | CLI/app-server, ChatGPT auth, MCP process model, cross-platform fixes |
| Gemini CLI | Subagent reliability, AST-aware context, sandboxing, browser agent, TUI rendering | Google/Gemini/Code Assist, MCP-heavy | Gemini-native, agent registry, subagents, sandbox.toml, nightly releases |
| GitHub Copilot CLI | Enterprise auth, BYOK routing, agent hooks, inline images, MCP | GitHub enterprise/managed fleets | Copilot+BYOK, agent-scoped declarative hooks, rapid patch releases |
| OpenCode | V2 parity, provider gateway compatibility, config precedence, self-hosting, desktop/TUI | Multi-provider/self-hosted power users | Model-agnostic, Drizzle/DBMS, plugins/tools, V2 rewrite |
| Pi | Durable runtime, SDK extensibility, OpenRouter key-aware models, TUI/protocol | SDK embedders, production agent developers | Durable runtime API, per-key model discovery, extension hooks, Nix/Bun |
| Qwen Code | Managed agents, multi-agent durability, K8s runtime, Web Shell | Enterprise/cloud managed-agent users | Managed-agent dual-path, ACP/daemon, Java/Python SDK, Kubernetes |

The clearest split is between first-party model-tied tools (Claude Code, Codex, Gemini CLI) and model-agnostic/enterprise-routing tools (OpenCode, Pi, Copilot CLI BYOK). First-party tools optimize for deep model integration and desktop/CLI parity; model-agnostic tools compete on provider flexibility, managed config, and self-hosting. Qwen Code is the most infrastructure-oriented, pushing managed-agent architecture and Kubernetes runtime, while Pi is the most SDK/durable-runtime-oriented.

## 5. Community Momentum & Maturity

OpenAI Codex has the highest raw activity in this snapshot: 50 updated issues, 24 updated PRs, and 4 discussions, with `/undo` (#9203) at 93 comments and 479 👍 — the strongest single community demand signal. GitHub Copilot CLI shows high release velocity with four releases, but review bandwidth appears absorbed by the release train, with 0 PRs updated and 35 issues updated. Claude Code remains highly engaged despite no releases or PRs: its top issue has 78 👍 and its closed compaction bug has 30 👍, indicating a mature user base sensitive to silent context loss. Gemini CLI and Qwen Code are iterating rapidly through nightly releases and PR clusters around agent reliability and managed-agent architecture. OpenCode and Pi are active but still in higher-churn phases: OpenCode has V2 parity fatigue, while Pi is expanding its durable runtime and SDK surface. Overall maturity is highest in Claude Code, Codex, Copilot CLI, and Gemini CLI; OpenCode, Pi, and Qwen Code are high-velocity but still stabilizing core semantics.

## 6. Trend Signals

- **Agent reliability is now table stakes.** False success statuses, hangs, session poisoning, and lost context are recurring across Gemini, Codex, Pi, OpenCode, and Claude. Developers need deterministic termination, cancellation, resume, and observability before adding more autonomy.
- **Permission models need explicit precedence.** Claude’s hook-override bug, Gemini’s sandbox rules, Copilot’s hook composition, and OpenCode’s webfetch deny all point to the same requirement: documented precedence and auditable enforcement.
- **Cost and capacity resilience are product features.** Cross-model fallback, BYOK routing, quota-aware model catalogs, budget caps, and spend visibility appear in Codex, OpenCode, Claude, Copilot, and Pi. Rate limits and capacity errors are now first-class UX failures.
- **Multi-agent orchestration is entering production.** Qwen’s managed-agent architecture, Gemini’s subagent observability, Codex’s MCP process leaks, and Pi’s per-request taskId requests show demand for agent identity, team messaging, shared memory, and resource isolation.
- **Enterprise manageability is a competitive axis.** Managed config discovery, Entra/WAM auth, policy resilience, and deprecation banners are blocking managed fleets in Copilot, OpenCode, Codex, and Qwen.
- **Cross-platform parity remains a major differentiator.** Windows/WSL is the dominant bug cluster for Codex, Copilot, OpenCode, Pi, and Claude; Linux packaging and Wayland affect Qwen and Gemini; macOS Gatekeeper and crashes affect Copilot and Codex.
- **MCP is still maturing.** Tool-count ceilings, OAuth/protocol currency, durable MCP lifecycle, and per-invocation server exclusion are active across Gemini, Copilot, Codex, Qwen, OpenCode, and Claude.
- **Silent failures are increasingly unacceptable.** LSP filtering, Bash escaping, file encoding, dropped session defects, and image attachment loss all erode trust. Developers want loud, actionable errors over apparent success.
- **TUI/terminal UX still matters.** Flicker, clipboard, resize, keyboard history, and terminal-state restoration are repeated across Gemini, Pi, Copilot, OpenCode, and Qwen. For CLI agents, the terminal is the product surface.
- **Model-agnostic and BYOK demand is rising.** OpenCode’s fallback request, Pi’s per-key OpenRouter discovery, and Copilot’s BYOK routing regressions show that enterprises and power users increasingly want provider portability without losing agent quality.

**Bottom line for technical decision-makers:** prioritize durable session state, explicit permission precedence, provider fallback, cost guardrails, and cross-platform CI. These are no longer differentiators — they are the minimum requirements for production agentic CLI workflows.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills — Community Highlights Report
**Source:** github.com/anthropics/skills · **Data as of:** 2026-10-13

> **Data caveat:** The PR feed supplies `Comments: undefined` for every entry, so PR-level comment counts are not recoverable from this dataset. PR ranking below therefore follows the API's stated "sorted by comments" order, weighted by recency of updates, issue linkage, and scope of change. Issue rankings use verified comment counts.

---

## 1. Top Skills Ranking

| # | Skill / PR | What it does | Discussion highlights | Status |
|---|---|---|---|---|
| 1 | **mcp-builder: MCP ≥2.0 support** — [PR #1742](https://github.com/anthropics/skills/pull/1742) | Adapts `connections.py` to the `streamable_http_client` rename and moves custom headers to `create_mcp_http_client` / `http_client`. | Directly closes #1668; the MCP 2.0 break is the single most impactful regression in the builder toolchain. Updated 2026-10-08. | OPEN |
| 2 | **skill-creator: isolate trigger evals / Windows + runtime failures** — [PR #1298](https://github.com/anthropics/skills/pull/1298) | Fixes cross-matching workers, `select()` on subprocess pipes failing on Windows, and runtime errors being scored as non-triggers. | Overlaps with issues #1352 and #1383; the "false-negative trigger rate" cluster is the most-reported defect family in the repo. | OPEN |
| 3 | **skill-creator: harden eval viewer** — [PR #1961](https://github.com/anthropics/skills/pull/1961) | Closes script breakout, DNS rebinding, cross-site POST, and escaping gaps in `generate_review.py` + `viewer.html`. | Security hardening of a local viewer that renders untrusted model output; complements issue #1394 (attribute-safe `escapeHtml`). | OPEN |
| 4 | **proofcore-contract-auditor** — [PR #1771](https://github.com/anthropics/skills/pull/1771) | New Agent Skill for static analysis of Solidity/Rust contracts, anchoring Merkle audit proofs on TON. | Representative of the "vertical domain skill" wave; raises questions about third-party protocol dependencies inside official skills. | OPEN (draft-class) |
| 5 | **md2video-audio** — [PR #1703](https://github.com/anthropics/skills/pull/1703) | Zero-cost pipeline compiling Markdown → Marp slides → MP4 with synthetic voiceover. | Popular "content production" direction; discussion centers on runtime cost and external toolchain (Marp, TTS) reproducibility. | OPEN |
| 6 | **docx: LibreOffice timeout handling** — [PR #1792](https://github.com/anthropics/skills/pull/1792) | Returns an error on `soffice` timeout and verifies `w:ins`/`w:del` removal before claiming success. | Part of a document-skill reliability thread alongside [PR #1734](https://github.com/anthropics/skills/pull/1734) (orphaned docx comments). | OPEN |
| 7 | **notion-spec-to-implementation + quantitative-resume-auditor** — [PR #1245](https://github.com/anthropics/skills/pull/1245) | Turns specs into Notion tasks with acceptance criteria; audits resumes against quantitative criteria. | Longest-lived active PR (created 2026-06-02, updated 2026-09-30) — workflow-automation demand. | OPEN |
| 8 | **skill-creator: direct execution of `package_skill.py`** — [PR #1681](https://github.com/anthropics/skills/pull/1681) | Fixes `ModuleNotFoundError: scripts.quick_validate` when running the packager standalone; updates stale CLI docs. | Same author as #1742; signals sustained contributor investment in the packaging path. | OPEN |

**Observation:** every entry in the top 20 is **OPEN** — no merged PR appears anywhere in the visible set. The submission queue is deep and slow-moving relative to the volume of incoming skills.

---

## 2. Community Demand Trends

Distilled from the top Issues (comment counts verified):

1. **Skill-authoring & eval tooling reliability — the dominant theme.**
   - [#556](https://github.com/anthropics/skills/issues/556) (12 comments, 👍7): `run_eval.py` never triggers skills — 0% trigger rate across all queries.
   - [#1352](https://github.com/anthropics/skills/issues/1352) (4, 👍2): parallel workers cross-match skill UUIDs → systematically wrong trigger rates.
   - [#1383](https://github.com/anthropics/skills/issues/1383) (4): six reproducible `skill-creator` failures, incl. directory layout mismatch and inverted benchmark deltas.
   - [#202](https://github.com/anthropics/skills/issues/202) (8, CLOSED): `skill-creator` should be rewritten as an operational skill, not developer documentation.

2. **Security & trust boundaries.**
   - [#492](https://github.com/anthropics/skills/issues/492) (46 comments, 👍2) — the single most-discussed issue: community skills shipping under the `anthropic/` namespace, enabling impersonation and privilege escalation.
   - [#1394](https://github.com/anthropics/skills/issues/1394): display-path XSS in the eval viewer.
   - [#1175](https://github.com/anthropics/skills/issues/1175, CLOSED): whether access control should live inside `SKILL.md`.

3. **Distribution & org-level sharing.**
   - [#228](https://github.com/anthropics/skills/issues/228) (16, 👍8): org-wide skill sharing in Claude.ai instead of manual `.skill` file exchange.
   - [#189](https://github.com/anthropics/skills/issues/189) (6, 👍9): `document-skills` and `example-skills` install identical content → duplicate context load.

4. **Context-window economics.**
   - [#1487](https://github.com/anthropics/skills/issues/1487): `claude-api` eagerly injects ~156k tokens in a single tool call.
   - [#1329](https://github.com/anthropics/skills/issues/1329): proposed `compact-memory` skill using symbolic notation for agent state.

5. **Governance / quality-gate skills.**
   - [#412](https://github.com/anthropics/skills/issues/412, CLOSED) and [#1385](https://github.com/anthropics/skills/issues/1385): policy enforcement, adversarial review, delivery verification pipelines.

**New-Skill directions most in demand:** skill QA/security auditing, org distribution tooling, context-efficient memory representation, and workflow conversion (spec → implementation, document → media).

---

## 3. High-Potential Pending Skills (active, unmerged)

Ranked by recency of activity + issue linkage — the likeliest to land next:

| PR | Skill | Why it's likely to land |
|---|---|---|
| [#1742](https://github.com/anthropics/skills/pull/1742) | mcp-builder MCP ≥2 support | Updated 2026-10-08; closes a concrete, reproducible break against a released dependency. |
| [#1961](https://github.com/anthropics/skills/pull/1961) | skill-creator eval-viewer hardening | Updated 2026-10-11 (most recent top-20 activity); security fixes tend to be fast-tracked. |
| [#1298](https://github.com/anthropics/skills/pull/1298) | skill-creator trigger-eval isolation | Updated 2026-09-16; converges with independently filed issues #1352/#1383. |
| [#1681](https://github.com/anthropics/skills/pull/1681) | skill-creator standalone packager | Updated 2026-10-08; narrow, low-risk, single-module fix. |
| [#1792](https://github.com/anthropics/skills/pull/1792) | docx timeout/verification | Updated 2026-09-25; corrects an incorrect-success report — clear correctness win. |
| [#1730](https://github.com/anthropics/skills/pull/1730) | claude-api dead-URL repair | Updated 2026-10-04; URLs verified HTTP 200, trivially mergeable. |
| [#1977](https://github.com/anthropics/skills/pull/1977) | algorithmic-art `wrapAround()` | Updated 2026-10-07; fixes #1897, minimal diff. |
| [#1976](https://github.com/anthropics/skills/pull/1976) | webapp-testing element discovery | Updated 2026-10-07; fixes #1891, minimal diff. |

*New-skill proposals with real momentum but heavier review needs:* [#1771](https://github.com/anthropics/skills/pull/1771) (Web3 auditing), [#1703](https://github.com/anthropics/skills/pull/1703) (md2video-audio), [#1615](https://github.com/anthropics/skills/pull/1615) (scnet-hpc), [#1245](https://github.com/anthropics/skills/pull/1245) (Notion/resume).

---

## 4. Skills Ecosystem Insight

**The community's demand is concentrated not on inventing new Skills, but on making the Skill lifecycle trustworthy — repair the skill-creator evaluation harness, harden Skill security and namespace provenance, and cut context cost — with new domain Skills arriving faster than the maintenance backlog can absorb them.**

---

# Claude Code Community Digest — 2026-10-13

## 1. Today's Highlights

No new releases or pull requests landed in the last 24 hours, so community activity is entirely issue-driven. The top of the queue is dominated by long-running, high-engagement threads: a 40-comment feature request to let Claude write/update project files (#16550, 78 👍) and a now-closed bug report about idle compaction silently discarding working context (#98747, 30 👍). A second cluster of pain points is forming around **worktree/sandbox permission enforcement** — Bash guards overriding explicit `PreToolUse: allow` hooks and refusing git-free commands — alongside platform-specific correctness bugs in Bash escaping, LSP result filtering, and Desktop/VS Code session lifecycle.

## 2. Releases

None in the last 24 hours.

## 3. Hot Issues

1. **[#16550](https://github.com/anthropics/claude-code/issues/16550) — Allow Claude to Write/Update Project Files** `OPEN` `enhancement, area:core, area:desktop` · 40 comments · 78 👍
   The single most-discussed open item by a wide margin, dating back to January and still updated today. A very high 👍-to-comment ratio suggests broad latent demand rather than a contentious debate — users want first-class file-write capability framed as a core/desktop capability rather than a workaround.

2. **[#98747](https://github.com/anthropics/claude-code/issues/98747) — Idle compaction silently discards working context (2.1.286)** `CLOSED` `bug, platform:macos, area:core` · 22 comments · 30 👍
   Since 2.1.286, idle sessions are compacted before the prompt cache expires with no opt-out and no warning, and the event is logged as "manual." For long-running sessions this discards the grounding the session was built on. Closed now, but the engagement level shows how sensitive users are to silent context loss.

3. **[#91495](https://github.com/anthropics/claude-code/issues/91495) — Built-in browser ignores "Allow all websites" site permissions** `OPEN` `bug, platform:macos, area:permissions, area:browser-extension, area:desktop` · 19 comments · 23 👍
   A permissions-policy violation in Claude Code Desktop: the app-level allowlist is not honored by the embedded browser. High comment count plus solid 👍 indicates this is blocking real browser-automation workflows, not a cosmetic quirk.

4. **[#88561](https://github.com/anthropics/claude-code/issues/88561) — Bash tool collapses `\\` to `\` before shell parsing** `OPEN` `bug, has repro, platform:windows, area:bash` · 7 comments · 4 👍
   Backslash collapse happens inside single quotes, double quotes, and quoted heredocs, breaking the POSIX guarantee that single quotes preserve backslashes. Corrupts regexes and Windows paths silently — a correctness bug that eats hours of debugging before the user suspects the harness. Has a repro.

5. **[#68083](https://github.com/anthropics/claude-code/issues/68083) — Desktop "Auto-fix CI and address comments" toggle not applied to `gh`-created PRs and not persisted** `OPEN` `bug, platform:macos, area:desktop` · 7 comments · 8 👍
   The global toggle neither applies to PRs opened via `gh` from local sessions nor persists in `claude_desktop_config.json`. The author explicitly distinguishes this from the closed #51765, framing it as broken existing config rather than a new request — a sign desktop config semantics need clarification.

6. **[#78263](https://github.com/anthropics/claude-code/issues/78263) — Add API fallback for the auto-mode classifier** `OPEN` `enhancement, area:model, area:permissions` · 5 comments · 4 👍
   A Sonnet 5 1M outage stopped a run because the auto-mode classifier had no fallback. Users want automatic degradation to another model rather than a hard stop — a resilience gap in an otherwise invisible subsystem that directly halts agentic work.

7. **[#95122](https://github.com/anthropics/claude-code/issues/95122) — Worktree isolation refuses git-free Bash commands (75 refusals in 3 days)** `OPEN` `bug, has repro, area:bash, area:sandbox, platform:wsl` · 3 comments
   In worktree-isolated sessions on 2.1.272/2.1.273, Bash calls containing no `git` and referencing only worktree-internal paths are refused, contradicting the 2.1.257 changelog. The quantified refusal count makes a strong case that the sandbox guard is over-broad.

8. **[#100385](https://github.com/anthropics/claude-code/issues/100385) — Worktree Bash guard overrides explicit `PreToolUse` `permissionDecision: allow`** `OPEN` `bug, has repro, area:bash, area:hooks, area:sandbox` · 1 comment · 1 👍
   The hook's allow verdict is recorded in the transcript and *then* the refusal follows, with no setting or rule able to override it. This breaks the contract that hooks are the last word on permissions — a serious issue for teams relying on policy hooks.

9. **[#101476](https://github.com/anthropics/claude-code/issues/101476) — LSP tool drops `findReferences`/`goToDefinition` results in gitignored paths** `OPEN` `bug, has repro, area:tools, area:lsp` · 1 comment
   `incomingCalls` and `hover` return the same locations, proving gopls has indexed them — the filtering is happening in Claude Code, and `respectGitignore: false` has no effect. Silently lossy code intelligence in monorepos and vendored trees.

10. **[#101650](https://github.com/anthropics/claude-code/issues/101650) — Orchestrating session ignored written agent limits, burned ~12% of a weekly Max allowance, merged nothing** `OPEN` `bug, area:cost, area:model, area:agents` · 1 comment
    An Opus coordinator ran ~1.5–2× the project's documented 6-agent concurrency cap, then retried its own load-induced timeouts for two hours. A worst-case cost/guardrail story that ties together agent limits, timeout handling, and budget visibility.

## 4. Key PR Progress

None — no pull requests were updated in the last 24 hours.

## 5. Hot Discussions

No discussion data was provided for this period; this section is omitted.

## 6. Feature Request Trends

- **Opt-out and fallback controls for automatic systems.** Idle compaction (#98747) and the auto-mode classifier (#78263) both triggered requests for graceful degradation or an explicit disable switch. Users accept automation, not automation without an escape hatch.
- **Agent orchestration guardrails and cost governance.** #101650 requests enforcement of project-declared concurrency/agent limits and better handling of self-inflicted timeouts. Expect more asks for budget caps and per-session spend visibility.
- **Desktop parity with the terminal.** #100913 asks for headless session handoff (prompt + model + effort, no click) to escape re-sent-context costs; #68083 asks for desktop config to actually persist and apply. Desktop is converging on "the terminal, but configurable."
- **Mods/plugin API expansion.** #101377 (per-call sandbox deny rules from a `tool.call` hook) and #99425 (a mascot slot for plugin companions) show the plugin surface is still young and being probed at both the security and UX ends.
- **Finer-grained sandbox and hook authority.** #100385 and #95122 both point to the same direction: users want a clear, documented precedence order where hooks > settings > guard heuristics, with no silent overrides.
- **Correct tooling semantics over new tooling.** LSP gitignore handling (#101476), Bash backslash handling (#88561), and MCP OAuth resource encoding (#101205) are all requests for existing tools to behave as documented.

## 7. Developer Pain Points

- **Silent, destructive state changes.** Idle compaction discarding context (#98747), `\\` → `\` corruption before shell parsing (#88561), and LSP results vanishing from gitignored paths (#101476) all fail without a clear, actionable warning.
- **Permission model inconsistencies.** Desktop site permissions ignored (#91495), a hook's explicit `allow` overridden by the worktree guard (#100385), and the Chrome extension's approval popup re-firing within an already-approved session (#101654) all erode trust in the permission system.
- **Session lifecycle fragility.** Switching organization on a single account drops running Code sessions (#94507), a VS Code extension-host restart leaves panels permanently frozen (#94743), and the footer PR indicator stays pinned to a long-merged PR for weeks (#100360).
- **Over-broad sandbox refusals.** Worktree isolation rejecting git-free Bash (#95122) and the `PreToolUse` override bug (#100385) force users into workarounds or abandoning isolation entirely.
- **Cost blowouts from runaway agents.** #101650 quantifies the risk: a session that violates its own written limits can consume a meaningful fraction of a weekly allowance with zero merged output. Users lack real-time circuit breakers.
- **Platform-specific config corruption.** Windows `~/.claude.json` accumulating case-differing project keys that make the file unparseable (#90438), plus SIGILL during "Compacting conversation…" on Linux/Bun (#101648), show config and runtime stability still varies sharply by platform.

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-10-13

## Today's Highlights
No new releases were published in the last 24 hours. Activity was dominated by 50 updated issues, 24 updated PRs, and 4 discussions, with the strongest community signals around restoring `/undo` and making the CLI background daemon opt-in. PR activity was largely closed bot-authored fixes covering proxy security, terminal/TUI correctness, thread permission restoration, WSL clipboard reliability, and Windows build/runtime isolation.

## Hot Issues
1. **[#9203 — Please make "/undo" back](https://github.com/openai/codex/issues/9203)** — 93 comments, 479 👍. Highest-demand session-safety request: users report Codex deleting untracked files or modifying uncommitted work and want a reliable rollback path.
2. **[#48195 — CLI: make `daemon_auto_start` opt-in](https://github.com/openai/codex/issues/48195)** — 16 comments, 129 👍. Strong consent/resource concern: a persistent, self-updating background daemon should not be enabled by default.
3. **[#52839 — Sol Ultrafast omitted from first-party model catalog with ChatGPT authentication](https://github.com/openai/codex/issues/52839)** — 16 comments. Windows CLI users on ChatGPT auth cannot see Sol Ultrafast despite fast/ultrafast mode being enabled.
4. **[#25015 — Codex app-server leaks MCP process stacks for subagents](https://github.com/openai/codex/issues/25015)** — 15 comments, 4 👍. Long-lived app-server memory grows on Linux as MCP child process trees survive subagent completion.
5. **[#49453 — Browser access blocked by saved chat-level denial after explicit allow](https://github.com/openai/codex/issues/49453)** — 14 comments, 1 👍. Windows Browser Use can remain blocked after the user allows a site, indicating stale or conflicting permission state.
6. **[#52567 — CLI goal hits context overflow after WebSocket retries and fails on resume](https://github.com/openai/codex/issues/52567)** — 9 comments, 2 👍. Long-running CLI sessions can become unrecoverable after connectivity retries.
7. **[#47740 — Add setting to disable CLI usage-limit warnings](https://github.com/openai/codex/issues/47740)** — 7 comments, 17 👍. Configurability request for suppressing proactive rate-limit notices in `config.toml`.
8. **[#32665 — Allow custom presets in the model picker power slider](https://github.com/openai/codex/issues/32665)** — 7 comments, 20 👍. App users want persisted custom model/power presets rather than fixed advanced-picker values.
9. **[#50252 — "Selected model is at capacity" on GPT-6.1-Sol despite quota remaining](https://github.com/openai/codex/issues/50252)** — 5 comments, 1 👍. Capacity errors continue despite 97% 5h and 99% weekly quota, interrupting normal repository analysis.
10. **[#44995 — Windows Desktop `ntfs.sys` `NtFC` nonpaged pool grows ~0.5 GB/min](https://github.com/openai/codex/issues/44995)** — 4 comments. Severe Windows kernel-level performance issue while Codex Desktop is open; growth stops when Codex closes.

## Key PR Progress
All listed PRs were closed in the last 24h; many were authored by `copyberry[bot]`.

1. **[#53506 — Reject duplicate Host headers in the network proxy](https://github.com/openai/codex/pull/53506)** — Security hardening: returns `400 Bad Request` for multiple `Host` headers before policy evaluation/upstream parsing, preventing parser disagreement.
2. **[#53451 — Treat incomplete responses as terminal errors](https://github.com/openai/codex/pull/53451)** — Introduces `ResponseIncomplete` errors so `max_output_tokens` responses do not trigger retries or compaction fallback after hitting the output limit.
3. **[#53452 — Close input admission while a turn finishes recording](https://github.com/openai/codex/pull/53452)** — Fixes a turn-finalization race where inputs could be queued after the final batch drained or recorded ahead of it.
4. **[#53419 — Restore saved thread permissions during configuration loading](https://github.com/openai/codex/pull/53419)** — Ensures saved read-only thread permissions are honored during resume even when configured defaults change; affects network proxy and permission validation.
5. **[#53424 — Add regression coverage for network grants on cold thread resume](https://github.com/openai/codex/pull/53424)** — Linux app-server integration test verifies restored grants and approval behavior when default permissions change to `:danger-full-access`.
6. **[#53436 — Route TUI request replies to originating connection and thread](https://github.com/openai/codex/pull/53436)** — Prevents pending request replies and resolution notifications from crossing session switches or reused request IDs.
7. **[#53429 — Apply configuration toggles live and improve settings navigation](https://github.com/openai/codex/pull/53429)** — Reduces restart requirements for several toggles, fixes settings search scope, and improves narrow-terminal rendering.
8. **[#53453 — Prefer Windows clipboard and bound copy attempts on WSL](https://github.com/openai/codex/pull/53453)** — Addresses WSLg clipboard hangs and PowerShell clipboard stalls that could block TUI copy requests.
9. **[#53400 — Add explicit local board connections with fixed runtime access](https://github.com/openai/codex/pull/53400)** — Adds `LocalBoard`/`LocalBoardConnection` so independently created Codex runtimes can share a board with different access permissions.
10. **[#53389 — Use cached login credentials for Guardian decisions](https://github.com/openai/codex/pull/53389)** — Stops bypassing `AuthManager` cached auth by reading `OPENAI_API_KEY` directly for Guardian decisions on the OpenAI provider.

Additional Windows/CI fixes closed: [#53443](https://github.com/openai/codex/pull/53443) local MSVC toolchains for native Windows Bazel tests, [#53412](https://github.com/openai/codex/pull/53412) OS-isolated arg0 helper directories, and [#53426](https://github.com/openai/codex/pull/53426) Windows runtime registration diagnostics.

## Hot Discussions

### Ideas
- **[#53294 — Suppress `config.toml` deprecation warnings from Cloud-Managed/Enterprise layers](https://github.com/openai/codex/discussions/53294)** — 1 comment, 1 👍. Proposal tied to issue #53293 to remove un-dismissible deprecation banners in Enterprise/Team orgs.

### Q&A
- **[#53366 — Sites publishing from Codex blocked by credential handoff](https://github.com/openai/codex/discussions/53366)** — 0 comments, 1 👍. macOS bundled Sites workflow requires a short-lived write credential handoff that managed Auto-review rejects; asks for a supported route.

### Show and tell
- **[#53200 — Text alerts service for Codex reset announcements](https://github.com/openai/codex/discussions/53200)** — 1 comment, 2 👍. SMS in US/Canada, Telegram otherwise, plus dashboards around reset on-time percentage.
- **[#53492 — Secure Browser Use for Zen: Firefox/Zen bridge for Codex browser control](https://github.com/openai/codex/discussions/53492)** — 0 comments, 1 👍. Independent Firefox/Zen compatibility layer submitted to Mozilla Add-ons; aims to bring signed-in Codex sidebar/browser control.

## Feature Request Trends
- **Session safety and rollback** — `/undo` restoration (#9203), saved thread permission restoration (#53419), and reliable resume after context overflow/WebSocket retries (#52567).
- **User control over CLI/app behavior** — opt-in `daemon_auto_start` (#48195), disable usage-limit warnings (#47740), custom model-power presets (#32665), and suppress enterprise deprecation banners (#53294).
- **Model availability and catalog correctness** — Sol Ultrafast missing under ChatGPT auth (#52839), repeated at-capacity errors despite quota (#50252, #53407, #53524), and GPT-6 Pro failures (#49434).
- **Browser/Computer Use permission reliability** — persistent saved denials after allow (#49453, #53031), elevated-app input blocking (#45388), dot cloud browser handback issues (#51475), and dot MCP tool access (#53136).
- **Cross-device / Dots workflows** — multiple personal computers per dot (#49824), Windows Dots offline/disconnect (#53519), and remote-created task persistence (#34254).
- **Performance and process isolation** — MCP subagent process leaks (#25015), Windows `ntfs.sys` pool growth (#44995), headless `codex exec` inheriting desktop computer-use runtime (#52645), and external-agent YAML detection stalls (#53374).
- **Extensibility and ecosystem bridges** — Zen/Firefox browser control (#53492), Codex reset alerts (#53200), and Sites publishing credential handoff (#53366).

## Developer Pain Points
- **Model capacity errors are a major interruption source**, especially on GPT-6.1-Sol and GPT-6 Pro, even when quota appears healthy.
- **Sessions remain fragile**: context overflow after retries, stream disconnects, resume failures, missing `/undo`, and permission state not being restored after config changes.
- **Windows is the dominant platform-bug cluster**: browser denial persistence, generated-file links, elevated-app Computer Use, `ntfs.sys` nonpaged-pool growth, headless-exec runtime inheritance, and WSL clipboard stalls.
- **Background/process lifecycle is a recurring resource concern**: default auto-start daemon, leaked MCP child process stacks, idle helper respawns, and helper-directory cleanup across Windows/WSL.
- **Configuration/auth UX needs polish**: usage-limit warnings, enterprise deprecation banners, absence of authenticated-account visibility in CLI, and skills disappearing during auth 503s.
- **Performance stalls can be severe and non-obvious**: deeply nested YAML in external-agent frontmatter, MCP process leakage, and Windows kernel pool growth.
- **Cross-platform consistency remains uneven**: macOS app crashes and stream issues, Linux browser fallback behavior, and WSL clipboard/runtime edge cases.

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-10-13

## 1. Today's Highlights

A quiet release day on the nightly channel: `v0.65.0-nightly.20261013.g51617c1bc` ships a single robustness fix so a failed extension allowlist check no longer blocks other extensions from loading. Issue activity is dominated by agent reliability — a P1 report that subagent `MAX_TURNS` interruptions are mislabeled as `GOAL` success, a long-standing generalist agent hang (8 👍), and a cluster of browser-agent bugs. On the PR side, the community is pushing hard on terminal rendering stability (tmux flicker, resize refresh, debug console layout) and edge-case correctness in core file handling.

## 2. Releases

**v0.65.0-nightly.20261013.g51617c1bc** — [Release](https://github.com/google-gemini/gemini-cli/releases)
- `fix(cli)`: keep loading other extensions when one fails the allowlist check ([#29720](https://github.com/google-gemini/gemini-cli/pull/29720)) — prevents a single bad/misconfigured extension from taking down the whole extension load path.
- [Full changelog](https://github.com/google-gemini/gemini-cli/compare/v0.65.0-nightly.20261012.g9b6e0265d...v0.65.0-nightly.20261013.g51617c1bc)

## 3. Hot Issues

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — Subagent recovery after MAX_TURNS reported as GOAL success (P1, 13 comments)** — The highest-traffic issue today. `codebase_investigator` returns `status: "success"` / `Termination Reason: "GOAL"` despite hitting the turn limit before doing any analysis. This is a correctness-of-trust bug: callers that branch on status get silent false positives, which is especially damaging for automated/eval pipelines.
2. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — Generalist agent hangs forever (P1, 8 👍)** — The most-reacted issue in the set. Even trivial tasks (folder creation) hang indefinitely once the CLI defers to the generalist agent; instructing the model not to use subagents is the current workaround. High reaction count signals broad reproducibility.
3. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — Zero-dependency OS sandboxing & post-execution intent routing (P2, 9 comments)** — Proposal to lean into Gemini 3's native bash affinity by sandboxing POSIX-shell workflows rather than restricting them. A design-level discussion about how to get model capability and user safety without trading one for the other.
4. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini does not use skills and sub-agents enough (P2, 7 comments)** — Anecdotal but persistent: with well-described `gradle`/`git` skills available, the model rarely invokes them unless explicitly told. Directly limits the value of the custom skills feature.
5. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — Assess AST-aware file reads, search, and mapping (P2 epic, 7 comments)** — Tracks whether AST-aware tooling can cut turns, reduce misaligned reads, and lower token noise in `codebase_investigator`. Has spawned child issues ([#22746](https://github.com/google-gemini/gemini-cli/issues/22746), [#22747](https://github.com/google-gemini/gemini-cli/issues/22747)) evaluating tools like tilth, glyph, and ast-grep.
6. **[#22267](https://github.com/google-gemini/gemini-cli/issues/22267) — Browser Agent ignores `settings.json` overrides (P2)** — `AgentRegistry` merges the config correctly, but the Browser Agent discards it (e.g. `maxTurns`). A clean config-plumbing bug with clear reproduction.
7. **[#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — Browser subagent fails on Wayland (P1)** — Reports `Termination Reason: GOAL` on Linux/Wayland despite failure, compounding the misleading-status problem in #22323. Linux desktop users are effectively blocked from browser automation.
8. **[#24246](https://github.com/google-gemini/gemini-cli/issues/24246) — 400 error with >128 tools available (P2)** — The agent does not scope down the enabled tool set, so large tool inventories break requests outright. Relevant for MCP-heavy users with many servers connected.
9. **[#22672](https://github.com/google-gemini/gemini-cli/issues/22672) — Agent should stop/discourage destructive behavior (P2)** — The model reaches for `git reset` / `--force` when safer alternatives exist, and needs stronger guardrails around stateful resources like databases. Safety-adjacent and maintainer-authored.
10. **[#21763](https://github.com/google-gemini/gemini-cli/issues/21763) — `/bug` report doesn't include subagent context (P1)** — Bug reports capture only the main session, making subagent failures effectively unreproducible for maintainers. Cheap fix, high leverage for triage quality.

*Also active:* [#20079](https://github.com/google-gemini/gemini-cli/issues/20079) (symlinked agent `.md` files ignored), [#23571](https://github.com/google-gemini/gemini-cli/issues/23571) (tmp edit scripts scattered across the workspace), [#22186](https://github.com/google-gemini/gemini-cli/issues/22186) (output-hook crash), [#22232](https://github.com/google-gemini/gemini-cli/issues/22232) (browser profile lock recovery).

## 4. Key PR Progress

1. **[#29728](https://github.com/google-gemini/gemini-cli/pull/29728) — fix(cli): prevent session poisoning on rapid Ctrl-C (P1)** — Double `Ctrl+C` during tool execution/streaming raced an unawaited `cancelOngoingRequest()` against `/quit`. A high-severity UX fix for a very common user action.
2. **[#29725](https://github.com/google-gemini/gemini-cli/pull/29725) — fix(ui): synchronized output + isolated spinner rendering** — Wraps frame writes in terminal synchronized-output sequences to kill flicker under `tmux` and other multiplexers. Directly addresses the long-running TUI rendering complaints.
3. **[#29695](https://github.com/google-gemini/gemini-cli/pull/29695) — fix(ui): debug console height calculation and `terminalBuffer` flickering** — Fixes `F12` debug console layout plus incremental rendering and static history memoization with safe cache invalidation.
4. **[#29644](https://github.com/google-gemini/gemini-cli/pull/29644) — fix(cli): restore debounced static UI refresh on terminal width changes** — Reinstates the 100 ms debounced `refreshStatic()` on resize, restoring correct inline rendering when terminals are widened/narrowed.
5. **[#29703](https://github.com/google-gemini/gemini-cli/pull/29703) — fix(core): keep atomic-write temp file name within NAME_MAX** — Filenames of 215–255 bytes failed with `ENAMETOOLONG` because the `.<uuid>.tmp` suffix blew past the 255-byte limit. Clean, well-diagnosed filesystem correctness fix.
6. **[#29726](https://github.com/google-gemini/gemini-cli/pull/29726) — fix(sandbox): ignore trap wrapper when resolving command name** — On POSIX, wrapped commands were misidentified as `trap`, silently disabling per-command `sandbox.toml` rules and session approvals. A meaningful security-adjacent fix.
7. **[#29722](https://github.com/google-gemini/gemini-cli/pull/29722) — fix(core): treat whitespace-only UTF-16/UTF-32 files as empty** — `isEmpty()` always decoded sampled bytes as UTF-8, leaving NULs that `trim()` wouldn't strip. Small but representative of encoding edge cases in the file toolchain.
8. **[#29727](https://github.com/google-gemini/gemini-cli/pull/29727) — fix(core): report omitted `list_directory` entries** — Previously the tool silently returned zero entries when path validation rejected files; now rejections are surfaced to both model and user without loosening restrictions.
9. **[#29536](https://github.com/google-gemini/gemini-cli/pull/29536) — fix(grep): prevent command-line option injection (Closed)** — Hardens `packages/core/src/tools/grep.ts` (CWE-88) with explicit `-e` pattern delimiters for both `git grep` and system `grep`. Notable security hardening landing in the tool layer.
10. **[#29568](https://github.com/google-gemini/gemini-cli/pull/29568) — fix(core): append-only delta patching in ChatRecordingService (Closed, P1)** — Replaces full-history `{ $set: { messages } }` rewrites and unbounded in-memory retention with incremental deltas and bounded windowing. A scalability win for long sessions and `/chat share`.

*Also merged/closed today:* [#29532](https://github.com/google-gemini/gemini-cli/pull/29532) (honor `RetryInfo` delay of zero instead of classifying as terminal quota error), [#29535](https://github.com/google-gemini/gemini-cli/pull/29535) (respect allowed onboarding tier for Code Assist auth), [#29638](https://github.com/google-gemini/gemini-cli/pull/29638) (drop VS Code marketplace update check on activation), [#29635](https://github.com/google-gemini/gemini-cli/pull/29635) (mock `isHeadlessMode` for non-TTY test runs).

## 5. Hot Discussions

No discussion data was provided for this period, so this section is omitted.

## 6. Feature Request Trends

- **Agent orchestration and reliability** — Recurring themes across [#21409](https://github.com/google-gemini/gemini-cli/issues/21409), [#22323](https://github.com/google-gemini/gemini-cli/issues/22323), [#21968](https://github.com/google-gemini/gemini-cli/issues/21968), [#20195](https://github.com/google-gemini/gemini-cli/issues/20195), [#18287](https://github.com/google-gemini/gemini-cli/issues/18287): correct termination semantics, reliable skill/subagent invocation, and eventually parallel subagent collaboration with shared memory.
- **Subagent observability** — [#22598](https://github.com/google-gemini/gemini-cli/issues/22598) (`/chat share` for subagent trajectories) and [#21763](https://github.com/google-gemini/gemini-cli/issues/21763) (subagent context in `/bug`) both ask for the same thing: make subagent internals inspectable, not opaque.
- **Token-frugal, structure-aware context** — The AST epic ([#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746), [#22747](https://github.com/google-gemini/gemini-cli/issues/22747)) plus [#19561](https://github.com/google-gemini/gemini-cli/issues/19561) "Tactful Extraction" target the ~36.6k-token baseline per turn with surgical reads.
- **Persistent, file-based task state** — [#18836](https://github.com/google-gemini/gemini-cli/issues/18836) and [#21000](https://github.com/google-gemini/gemini-cli/issues/21000) propose replacing in-context `WriteToDo` with CRUD task tracking on disk to survive context rot and session boundaries.
- **Sandboxing and safety guardrails** — [#19873](https://github.com/google-gemini/gemini-cli/issues/19873), [#22672](https://github.com/google-gemini/gemini-cli/issues/22672), and [#18397](https://github.com/google-gemini/gemini-cli/issues/18397) converge on per-workspace policy, post-execution intent routing, and discouraging destructive git/database operations.
- **Terminal UX and configuration fidelity** — Flicker-free resize ([#21924](https://github.com/google-gemini/gemini-cli/issues/21924)), browser-agent config overrides ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)), and agent self-awareness of its own flags/hotkeys ([#21432](https://github.com/google-gemini/gemini-cli/issues/21432)).

## 7. Developer Pain Points

- **Hangs with no exit path.** The generalist agent blocking forever ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)) and the CLI stalling on interactive prompts ([#22465](https://github.com/google-gemini/gemini-cli/issues/22465), e.g. `create vite`) force users to kill sessions and lose state.
- **Failures masquerading as success.** `MAX_TURNS` reported as `Termination Reason: GOAL` ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323), [#21983](https://github.com/google-gemini/gemini-cli/issues/21983)) breaks any automation or evals that branch on agent status.
- **Agent ignores configuration.** `settings.json` overrides dropped by the Browser Agent ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)) and sandbox rules silently skipped on POSIX ([#29726](https://github.com/google-gemini/gemini-cli/pull/29726)) undermine the config model users rely on.
- **Tool-count ceilings.** Requests fail with 400 errors once more than ~128 tools are enabled ([#24246](https://github.com/google-gemini/gemini-cli/issues/24246)) — a real blocker for MCP-heavy setups.
- **Terminal rendering noise.** Repeated reports of flicker, resize artifacts, and layout bugs ([#21924](https://github.com/google-gemini/gemini-cli/issues/21924)), currently being attacked by a cluster of open PRs ([#29725](https://github.com/google-gemini/gemini-cli/pull/29725), [#29695](https://github.com/google-gemini/gemini-cli/pull/29695), [#29644](https://github.com/google-gemini/gemini-cli/pull/29644)).
- **Workspace hygiene and destructive commands.** Stray tmp edit scripts across directories ([#23571](https://github.com/google-gemini/gemini-cli/issues/23571)) and instinctive `git reset --force` usage ([#22672](https://github.com/google-gemini/gemini-cli/issues/22672)) create cleanup and safety burden.
- **Edge-case correctness in core tools.** Symlinked agent definitions ([#20079](https://github.com/google-gemini/gemini-cli/issues/20079)), incorrect `\n` escaping ([#22466](https://github.com/google-gemini/gemini-cli/issues/22466)), UTF-16/32 emptiness checks ([#29722](https://github.com/google-gemini/gemini-cli/pull/29722)), and `NAME_MAX` failures ([#29703](https://github.com/google-gemini/gemini-cli/pull/29703)) all point to a filesystem layer that still has sharp edges.
- **Quota/retry misclassification.** A zero `RetryInfo` delay being treated as terminal ([#29532](https://github.com/google-gemini/gemini-cli/pull/29532)) shows how small error-handling details produce disproportionate user-facing failure.

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI — Community Digest
**Date: 2026-10-13** | Source: [github.com/github/copilot-cli](https://github.com/github/copilot-cli)

---

## 1. Today's Highlights

Copilot CLI shipped a rapid-fire patch series (v1.0.96 → v1.0.97-2) headlined by **agent-scoped declarative hooks**, **inline image rendering enabled by default**, and a fix for custom-agent discovery when repositories define more than 100 agents. Meanwhile, the issue tracker is dominated by a cluster of **BYOK (Bring Your Own Key) model-routing regressions** (Issues #5126, #5127, #5103) and long-standing **Windows/enterprise authentication failures** that continue to block managed fleets. Notably, no pull requests were updated in the last 24 hours.

---

## 2. Releases

Four releases landed in the window, with v1.0.97 as the feature-bearing version and two follow-up patches.

### [v1.0.97-2](https://github.com/github/copilot-cli/releases/tag/v1.0.97-2)
- **Fixed:** Custom agent discovery now works even when repositories define more than 100 agents or nest agent files deeply.

### [v1.0.97-1](https://github.com/github/copilot-cli/releases/tag/v1.0.97-1)
- Minor fixes and changes (no detailed changelog provided).

### [v1.0.97-0](https://github.com/github/copilot-cli/releases/tag/v1.0.97-0)
- **Added:** Agent-scoped declarative hooks for custom agents.
- **Improved:** Inline images now render by default on supported terminals without requiring experimental mode. Disable via `/settings inlineImages false`.
- **Fixed:** Autopilot now correctly handles a `preToolUse` hook's `"ask"` decision instead of denying it.

### [v1.0.96](https://github.com/github/copilot-cli/releases/tag/v1.0.96) — 2026-10-12
- Interactive sandbox settings now suggest possible environment secrets and let you add masking hosts before saving.
- Model IDs in `/model` and `/config model` are now case-insensitive, and canonical IDs are saved.
- `/allow-all` remains available during startup while enterprise policy is still resolving.
- Additional changes truncated in source data.

---

## 3. Hot Issues

*(10 noteworthy items selected by comment count, recency, and impact)*

1. **[#970 — macOS Gatekeeper blocks Copilot app under corporate security policy](https://github.com/github/copilot-cli/issues/970)** · OPEN · 8 comments · 👍 22
   Every Homebrew upgrade triggers "Apple could not verify 'copilot' is free of malware." Highest-reaction issue in the tracker; requires manual Privacy & Security approval each time. A persistent install-friction tax for macOS enterprise users.

2. **[#5068 — Windows: MCP Entra sign-in fails with "scopes could not be safely validated"](https://github.com/github/copilot-cli/issues/5068)** · OPEN · 3 comments · 👍 12
   Blocks authentication to Entra-protected MCP servers (e.g., Azure DevOps MCP) on every fresh interactive sign-in. High community agreement given the enterprise Azure dependency.

3. **[#3808 — Enhance prompt caching for Claude Sonnet to reduce latency and token costs](https://github.com/github/copilot-cli/issues/3808)** · OPEN · 3 comments · 👍 8
   Long-standing feature request noting Anthropic's prompt caching isn't leveraged for repeated context or long system prompts. Direct cost/latency impact for heavy users.

4. **[#5114 — Session doesn't continue after hitting limit and waiting 5 hours](https://github.com/github/copilot-cli/issues/5114)** · OPEN · 3 comments
   Sessions remain blocked by a stale 429 runtime cap even after the stated cooldown, stranding partially-completed work.

5. **[#5100 — Session event delivery permanently fails after one 120s host-ack timeout](https://github.com/github/copilot-cli/issues/5100)** · OPEN · 1 comment
   A single `tool.execution_partial_result` ack timeout renders the entire long-running session unusable until resume. Serious reliability concern for agentic workflows.

6. **[#5103 — BYOK: sub-agents always use the session's wire API, causing 400s](https://github.com/github/copilot-cli/issues/5103)** · OPEN · 1 comment
   `COPILOT_PROVIDER_WIRE_API` leaks into sub-agents running models from a different family. Core BYOK routing defect.

7. **[#5126 — BYOK: built-in agents (explore, task, research) silently run on the session model](https://github.com/github/copilot-cli/issues/5126)** · OPEN (triage) · 0 comments
   Built-in agent default models (e.g., `gpt-5.6-luna`) are ignored, silently degrading agent behavior in BYOK setups.

8. **[#5127 — BYOK: short model names aren't matched to provider models; misleading "No GitHub OAuth token" error](https://github.com/github/copilot-cli/issues/5127)** · OPEN (triage) · 1 comment
   Provider-qualified IDs (`copilot/gpt-5.4-mini`) work, but short names fail with a confusing auth error. Newly filed — likely to gain traction.

9. **[#5109 — copilot-cli is unusable: managed account policy cannot be refreshed](https://github.com/github/copilot-cli/issues/5109)** · OPEN · 1 comment
   Enterprise users land in a retry loop at the trust prompt when managed policy resolution fails. A hard blocker for the entire CLI in managed environments.

10. **[#5105 — macOS sandbox blocks Gradle daemon connection despite local networking allowed](https://github.com/github/copilot-cli/issues/5105)** · OPEN · 2 comments
    Gradle's daemon/client localhost handshake is blocked even with outbound + local networking permitted, breaking JVM builds inside the sandbox.

**Also worth noting (recently closed):** [#4960](https://github.com/github/copilot-cli/issues/4960) (enterprise custom model selectable), [#5101](https://github.com/github/copilot-cli/issues/5101) (`--add-github-mcp-tool issue_write` yielding no tools), [#5079](https://github.com/github/copilot-cli/issues/5079) (MCP ping on 2026-07-28 protocol), [#5082](https://github.com/github/copilot-cli/issues/5082) (Dataverse MCP RFC 8414 issuer mismatch), [#5118](https://github.com/github/copilot-cli/issues/5118) (Atlassian Rovo MCP auth probe 400), and [#5030](https://github.com/github/copilot-cli/issues/5030) (ACP mode unable to launch custom agents).

---

## 4. Key PR Progress

**No pull requests were updated in the last 24 hours (Total: 0 items).** There is no PR activity to summarize for this digest period — review bandwidth appears to be absorbed by the v1.0.97 release train.

---

## 5. Hot Discussions

*No GitHub Discussions data was provided in the source dataset. This section is omitted.*

---

## 6. Feature Request Trends

Distilled from open issues and closed requests over the window:

- **BYOK / custom provider parity** — The largest emerging theme. Users want short model names resolved, per-agent model defaults honored, per-sub-agent wire API selection, and consistent behavior between provider-qualified and short model IDs ([#5127](https://github.com/github/copilot-cli/issues/5127), [#5126](https://github.com/github/copilot-cli/issues/5126), [#5103](https://github.com/github/copilot-cli/issues/5103)).
- **Cost and latency optimization** — Explicit demand for Anthropic-style prompt caching on Claude Sonnet ([#3808](https://github.com/github/copilot-cli/issues/3808)).
- **Observability of agent execution** — Requests for prompt/response timing and execution-duration tracking ([#5125](https://github.com/github/copilot-cli/issues/5125)).
- **MCP protocol currency** — Support for MCP `2026-07-28` Multi Round-Trip Requests / URL elicitation, now partially addressed ([#4834](https://github.com/github/copilot-cli/issues/4834), [#5079](https://github.com/github/copilot-cli/issues/5079)).
- **Enterprise policy resilience** — Making the CLI usable and `/allow-all` available while managed policy is still resolving ([#5109](https://github.com/github/copilot-cli/issues/5109), partially addressed in v1.0.96).

---

## 7. Developer Pain Points

Recurring frustrations, ranked by frequency and severity across the tracker:

1. **Enterprise & identity authentication is fragile.** Entra/WAM broker sign-ins crash or fail on Windows ([#5088](https://github.com/github/copilot-cli/issues/5088) — `0xc0000005` in `msalruntime.dll`), MCP OAuth scope validation fails ([#5068](https://github.com/github/copilot-cli/issues/5068)), and managed policy resolution loops indefinitely ([#5109](https://github.com/github/copilot-cli/issues/5109)). This is the single most damaging category for managed deployments.
2. **BYOK model routing is inconsistent.** Built-in agents, sub-agents, and short model names all behave incorrectly, often failing silently or with misleading error messages ([#5126](https://github.com/github/copilot-cli/issues/5126), [#5127](https://github.com/github/copilot-cli/issues/5127), [#5103](https://github.com/github/copilot-cli/issues/5103)).
3. **Session reliability degrades permanently.** One ack timeout ([#5100](https://github.com/github/copilot-cli/issues/5100)) or one stale rate-limit cap ([#5114](https://github.com/github/copilot-cli/issues/5114)) bricks a session, forcing a resume and losing context.
4. **The sandbox is too restrictive for real build tooling.** Gradle daemon localhost connectivity is blocked even with local networking enabled ([#5105](https://github.com/github/copilot-cli/issues/5105)).
5. **Terminal state is not restored on abnormal exit.** Mouse-tracking and focus-reporting escape sequences leak into the parent shell, making tabs unusable until reset ([#5120](https://github.com/github/copilot-cli/issues/5120)) — a recurring complaint across CLI tools.
6. **Startup race conditions silently drop configuration.** `--agent <name>` is deselected ~1s after launch during sign-in / MCP reload ([#5124](https://github.com/github/copilot-cli/issues/5124)).
7. **Plugin lifecycle on Windows is unreliable.** Uninstalling plugins fails with `Access is denied (os error 5)` ([#5119](https://github.com/github/copilot-cli/issues/5119)).
8. **macOS install friction.** Gatekeeper verification warnings on every Homebrew upgrade force manual approval ([#970](https://github.com/github/copilot-cli/issues/970), 👍 22).
9. **Hook composition drops data silently.** With multiple plugins returning `additionalContext` on `userPromptSubmitted`, only the last plugin's text reaches the model ([#5122](https://github.com/github/copilot-cli/issues/5122)).
10. **ACP/non-interactive mode lags behind interactive.** `/rename` reports success but is a no-op, and custom agents previously failed to launch via the task tool ([#5115](https://github.com/github/copilot-cli/issues/5115), [#5030](https://github.com/github/copilot-cli/issues/5030)).

---

*Digest generated from GitHub issue/release metadata for the 24-hour window ending 2026-10-13. Issue counts: 35 updated (30 shown), 0 PRs updated.*

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

**Today's Highlights**  
No new releases landed in the last 24h, but activity remains high around V2 parity, managed config discovery, provider reliability, and desktop/Windows UX. The long-running native model fallback request ([#7602](https://github.com/anomalyco/opencode/issues/7602)) closed with 120 👍 and 31 comments, signaling strong demand for cross-model failover. PR work is concentrated on config precedence, compaction-loop fixes, MCP auth state, OTLP header decoding, and permission gating for Code Mode.

**Releases**  
No new releases were published in the last 24h.

**Hot Issues**
- [#7602](https://github.com/anomalyco/opencode/issues/7602) — **[CLOSED] Native Model Fallback / Failover Support**. 31 comments, 120 👍. Allows fallback between different models on errors/rate limits; top community resilience request.
- [#14212](https://github.com/anomalyco/opencode/issues/14212) — **[CLOSED] Support more DBMS for OpenCode state storage**. 15 comments, 24 👍. Drizzle migration opens the door to Postgres and other DBs for self-hosted/enterprise use.
- [#50168](https://github.com/anomalyco/opencode/issues/50168) — **[OPEN] Desktop zoom resets to 100% after restarting**. 7 comments, 3 👍. Basic desktop UX persistence bug; duplicated by [#54990](https://github.com/anomalyco/opencode/issues/54990).
- [#52281](https://github.com/anomalyco/opencode/issues/52281) — **[OPEN] Windows child `cmd.exe` flashes console and steals focus**. 6 comments. Affects git probes and MCP spawns despite `windowsHide`; high Windows usability friction.
- [#42135](https://github.com/anomalyco/opencode/issues/42135) — **[CLOSED] `deepseek-v4-pro` on opencode-go `/responses` fails on multi-turn requests**. 7 comments. Gateway/provider compatibility issue: first turn works, later turns fail.
- [#42091](https://github.com/anomalyco/opencode/issues/42091) — **[CLOSED] `deepseek-v4-pro` on zen/go `/v1/messages` returns 400 `Empty input messages`**. 6 comments. Anthropic-compatible endpoint rejects string content while `deepseek-v4-flash` works.
- [#51107](https://github.com/anomalyco/opencode/issues/51107) — **[OPEN] V2 does not load managed config from `%ProgramData%\opencode`**. 3 comments, 6 👍. Enterprise/admin-managed settings regression vs V1.
- [#53074](https://github.com/anomalyco/opencode/issues/53074) — **[OPEN] `/etc/opencode` not respected in v2**. 3 comments, 1 👍. Linux counterpart to the managed-config parity issue.
- [#42250](https://github.com/anomalyco/opencode/issues/42250) — **[CLOSED] Tool files that fail to load crash the whole session/registry**. 4 comments. A single bad `.opencode/tool/*.ts` file can make projects unusable.
- [#42259](https://github.com/anomalyco/opencode/issues/42259) — **[CLOSED] Session run defects are silently dropped**. 3 comments. UI shows idle instead of surfacing tool/resolver/provider failures; poor observability.

**Key PR Progress**
- [#54921](https://github.com/anomalyco/opencode/pull/54921) — **[OPEN] fix(core): discover system configuration directory at lowest precedence**. Fixes [#51107](https://github.com/anomalyco/opencode/issues/51107) and [#53074](https://github.com/anomalyco/opencode/issues/53074); restores V1 managed/system config parity.
- [#54994](https://github.com/anomalyco/opencode/pull/54994) — **[OPEN] fix(opencode): stop compaction loops and stale compaction retries**. Closes [#54993](https://github.com/anomalyco/opencode/issues/54993); stops repeated compaction after context overflow.
- [#54976](https://github.com/anomalyco/opencode/pull/54976) — **[OPEN] fix(core): gate Code Mode fetch on the webfetch permission**. Closes [#54595](https://github.com/anomalyco/opencode/issues/54595); ensures `"webfetch": "deny"` applies to Code Mode.
- [#54226](https://github.com/anomalyco/opencode/pull/54226) — **[CLOSED] fix(mcp): mark a server `needs_auth` when a tool call is rejected with 401**. Closes [#53942](https://github.com/anomalyco/opencode/issues/53942); stops stale “Connected” MCP status after failed OAuth refresh.
- [#54234](https://github.com/anomalyco/opencode/pull/54234) — **[CLOSED] fix(cli): restore `models --refresh` flag on v2**. Restores documented V1 CLI flags dropped in the V2 rewrite.
- [#55004](https://github.com/anomalyco/opencode/pull/55004) — **[OPEN] fix(util): decode OTLP header values**. Closes [#55002](https://github.com/anomalyco/opencode/issues/55002); fixes `Bearer%20test-token` reaching OTLP collectors instead of `Bearer test-token`.
- [#54728](https://github.com/anomalyco/opencode/pull/54728) — **[OPEN] feat(schema): identify branded IDs for client generation**. Refs [#43886](https://github.com/anomalyco/opencode/issues/43886); prerequisite for client brand activation and typed IDs.
- [#52980](https://github.com/anomalyco/opencode/pull/52980) — **[OPEN] fix(core): retain interrupted tool checkpoints**. Closes [#39565](https://github.com/anomalyco/opencode/issues/39565); keeps checkpoints until async cleanup and publication finish.
- [#52978](https://github.com/anomalyco/opencode/pull/52978) — **[OPEN] fix(core): read workspace files through environment**. Closes [#44568](https://github.com/anomalyco/opencode/issues/44568); routes workspace FS reads through `Environment.files`.
- [#54949](https://github.com/anomalyco/opencode/pull/54949) — **[OPEN] feat(tui): prompt start height, percentage sizes and home screen layout settings**. Closes [#54947](https://github.com/anomalyco/opencode/issues/54947); adds TUI layout customization, with a v2 port planned.

**Feature Request Trends**
- **Provider/model resilience**: cross-model fallback and failover remain the standout ask ([#7602](https://github.com/anomalyco/opencode/issues/7602)).
- **Config and storage extensibility**: more DBMS options ([#14212](https://github.com/anomalyco/opencode/issues/14212)) and system/managed config precedence ([#51107](https://github.com/anomalyco/opencode/issues/51107), [#53074](https://github.com/anomalyco/opencode/issues/53074)).
- **V2 migration parity**: managed config, plugins, CLI flags, MCP flat entries, and default agents still need parity work ([#48201](https://github.com/anomalyco/opencode/issues/48201), [#37225](https://github.com/anomalyco/opencode/issues/37225)).
- **Desktop/TUI polish**: zoom persistence ([#50168](https://github.com/anomalyco/opencode/issues/50168)), image thumbnails for text-only models ([#54991](https://github.com/anomalyco/opencode/issues/54991)), quick-jump sidebar ([#32165](https://github.com/anomalyco/opencode/issues/32165)), and TUI layout settings ([#54949](https://github.com/anomalyco/opencode/pull/54949)).
- **Plugin/tool ecosystem robustness**: local plugin paths ([#49608](https://github.com/anomalyco/opencode/issues/49608), [#46551](https://github.com/anomalyco/opencode/issues/46551)) and bad tool files crashing registries ([#42250](https://github.com/anomalyco/opencode/issues/42250)).
- **Privacy and permissions**: protect `.env` in grep/glob results ([#17073](https://github.com/anomalyco/opencode/issues/17073)) and gate Code Mode fetch ([#54976](https://github.com/anomalyco/opencode/pull/54976)).
- **Provider gateway compatibility**: deepseek-v4-pro issues ([#42135](https://github.com/anomalyco/opencode/issues/42135), [#42091](https://github.com/anomalyco/opencode/issues/42091)) and Anthropic-compatible tool use ([#34331](https://github.com/anomalyco/opencode/issues/34331)).
- **Reliability/performance**: idle CPU/RSS ([#42299](https://github.com/anomalyco/opencode/issues/42299), [#42271](https://github.com/anomalyco/opencode/issues/42271)), silent session defects ([#42259](https://github.com/anomalyco/opencode/issues/42259)), and server 400s ([#54230](https://github.com/anomalyco/opencode/issues/54230)).
- **Internationalization**: zh/zht dictionaries are missing 93 English keys, causing mixed-language UI ([#53858](https://github.com/anomalyco/opencode/issues/53858)).

**Developer Pain Points**
- **V2 regression fatigue**: config precedence, plugin paths, CLI flags, default agents, and MCP flat config keep breaking V1 expectations.
- **Windows friction**: visible `cmd.exe` flashes and focus stealing ([#52281](https://github.com/anomalyco/opencode/issues/52281)), zoom resets ([#50168](https://github.com/anomalyco/opencode/issues/50168)), and managed config path gaps ([#51107](https://github.com/anomalyco/opencode/issues/51107)).
- **Silent failures**: dropped session defects ([#42259](https://github.com/anomalyco/opencode/issues/42259)), silently dropped plugin paths ([#46551](https://github.com/anomalyco/opencode/issues/46551)), stale MCP auth status ([#54226](https://github.com/anomalyco/opencode/pull/54226)), and empty GLM streams ([#42325](https://github.com/anomalyco/opencode/issues/42325)).
- **Provider/gateway incompatibility**: deepseek-v4-pro multi-turn and `/v1/messages` failures ([#42135](https://github.com/anomalyco/opencode/issues/42135), [#42091](https://github.com/anomalyco/opencode/issues/42091)).
- **Resource leaks/performance**: idle serve CPU bursts ([#42299](https://github.com/anomalyco/opencode/issues/42299)), high RSS ([#42271](https://github.com/anomalyco/opencode/issues/42271)), and Bun segfaults ([#54228](https://github.com/anomalyco/opencode/issues/54228)).
- **Plugin/tool loading brittleness**: one bad tool file can crash the whole registry ([#42250](https://github.com/anomalyco/opencode/issues/42250)); local plugin file paths are rejected or dropped ([#49608](https://github.com/anomalyco/opencode/issues/49608), [#46551](https://github.com/anomalyco/opencode/issues/46551)).
- **Model fallback limitations**: no cross-model failover means long-running agents still stall on rate limits or provider errors ([#7602](https://github.com/anomalyco/opencode/issues/7602)).
- **Configuration discovery inconsistency**: `/etc/opencode`, `%ProgramData%\opencode`, and macOS managed paths are not consistently honored in V2 ([#53074](https://github.com/anomalyco/opencode/issues/53074), [#51107](https://github.com/anomalyco/opencode/issues/51107)).
- **Permissions/security gaps**: `.env` exposure via grep/glob ([#17073](https://github.com/anomalyco/opencode/issues/17073)) and Code Mode bypassing webfetch deny ([#54976](https://github.com/anomalyco/opencode/pull/54976)).

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi Community Digest — 2026-10-13

## Today's Highlights
Pi **v1.2.0** shipped with per-key OpenRouter model discovery: model lists, context sizes, max output, and pricing now reflect the actual capabilities of your OpenRouter key, including regional endpoints configured in `models.json`. Meanwhile, the issue tracker is dominated by Windows/WSL terminal edge cases (#6187, #9361), session-resume fidelity problems (#10785, #10864), and a cluster of durable-runtime API requests (#10845, #10846) that signal growing production use of the SDK.

## Releases

**[v1.2.0](https://github.com/earendil-works/pi/releases/tag/v1.2.0)**
- **Per-key OpenRouter model lists** — with an OpenRouter key configured, Pi now lists only models that key is permitted to use, pulling context size, max output tokens, and prices directly from the key's entitlements (including regional endpoints defined in `models.json`). This closes a long-standing gap where users could select models their key's guardrails would reject. Related work landed via [PR #10569](https://github.com/earendil-works/pi/pull/10569) and [#10672](https://github.com/earendil-works/pi/pull/10672).

## Hot Issues

1. **[#6187 [CLOSED] Pi login hangs in WSL after browser-based GitHub Copilot device authorization](https://github.com/earendil-works/pi/issues/6187)** — 28 comments. The single most-discussed issue in the window: the WSL client never observes the completed Copilot device authorization and hangs indefinitely. Closed, likely addressed by the OAuth polling-margin fix in [PR #10694](https://github.com/earendil-works/pi/pull/10694), which targets WSL clock drift causing persistent `slow_down` failures.

2. **[#9361 [OPEN] Windows: settings `shellPath` is non-deterministically ignored when extensions are loaded](https://github.com/earendil-works/pi/issues/9361)** — 14 comments. A valid `shellPath` in `~/.pi/agent/settings.json` is silently bypassed once any extension loads, falling through to Git Bash detection and then to WSL's `System32\bash.exe` on `PATH`. Non-determinism makes this particularly painful to diagnose; it's the top open Windows reliability issue.

3. **[#9688 [CLOSED] Regression: clipboard copy doesn't work anymore](https://github.com/earendil-works/pi/issues/9688)** — 10 comments, 2 👍. A fix for #9618 narrowed OSC 52 emission to SSH sessions only, breaking container/interactive use cases. Good example of a regression introduced by a targeted fix — the community's 2 upvotes underscore the impact on remote dev workflows.

4. **[#9391 [CLOSED] Stale signed thinking blocks replayed after compaction (Anthropic `prefix_binding_mismatch`)](https://github.com/earendil-works/pi/issues/9391)** — 7 comments. After a manual compaction, 15 stale thinking blocks were resent on every turn, with Anthropic dropping them each time. Silent context loss plus per-request overhead in long sessions — a correctness issue for anyone doing extended Anthropic work.

5. **[#10645 [OPEN][inprogress] `resizeImage` resolves null in compiled (Bun) executables](https://github.com/earendil-works/pi/issues/10645)** — 7 comments. All image attachments have been silently omitted since 0.87.x in standalone binaries, reproduced by capturing outbound provider requests. Marked in progress; a fallback fix is proposed in [PR #10834](https://github.com/earendil-works/pi/pull/10834).

6. **[#10705 [OPEN] `abort()` from `agent_settled` resolves but a deferred continuation still runs](https://github.com/earendil-works/pi/issues/10705)** — 4 comments. In SDK 1.1.0, a host that aborts a session can still see a new provider HTTP request fire afterward. Significant for SDK embedders who need hard stop guarantees (billing, safety, rate limiting).

7. **[#10785 [OPEN][prio:p0] Resuming a session drops dynamically activated tools](https://github.com/earendil-works/pi/issues/10785)** — 3 comments. Tools registered with `defaultActive: false` or `exposure: "deferred"` deactivate on resume and are then recorded as removed from the tool list. Any long-running agent workflow relying on deferred tool exposure breaks across restarts.

8. **[#10628 [OPEN] OSC 52 copy in fullscreen TUI is swallowed by tmux (no passthrough wrap)](https://github.com/earendil-works/pi/issues/10628)** — 3 comments, 3 👍. Pi writes raw OSC 52; tmux absorbs it into its own paste buffer rather than relaying. The highest-👍 open issue in this window, reflecting a broad headless/SSH-over-tmux user base.

9. **[#10770 [OPEN] Default permissions of `.pi/agent/sessions` should be `0700`](https://github.com/earendil-works/pi/issues/10770)** — 2 comments. Session transcripts are created world-readable by default. A small, high-leverage privacy/security hardening request with straightforward implementation.

10. **[#10798 [OPEN] Concurrent pi processes race managed npm install on shared `.pi/npm` prefix (no install lock)](https://github.com/earendil-works/pi/issues/10798)** — 2 comments. Parallel subagent child sessions invoking `npm install --prefix <project>/.pi/npm` concurrently corrupt the managed package store. A structural gap as multi-agent fan-out becomes a common pattern.

## Key PR Progress

1. **[#10694 [OPEN] fix(ai): adapt OAuth device polling margin after `slow_down`](https://github.com/earendil-works/pi/pull/10694)** — Responds to the WSL clock-drift root cause behind #6187; increases the polling interval margin so Copilot OAuth can converge instead of looping on rate-limit errors.

2. **[#10528 [OPEN] refactor Nix packaging](https://github.com/earendil-works/pi/pull/10528)** — Cleanup of `package.nix`, drops source maps/type decls, adopts `makeBinaryWrapper`, skips version checks. Improves reproducibility for Nix-based deployments.

3. **[#10855 [CLOSED] fix(ai): map minimal thinking to low on Xiaomi MiMo models](https://github.com/earendil-works/pi/pull/10855)** — MiMo's API rejects `reasoning_effort: "minimal"` with a 400; the catalog now sets `thinkingLevelMap: { minimal: "low" }`. Fixes #10818.

4. **[#10569](https://github.com/earendil-works/pi/pull/10569) / [#10672 [CLOSED] feat(ai,coding-agent): filter OpenRouter models by key availability](https://github.com/earendil-works/pi/pull/10672)** — The implementation behind the v1.2.0 headline feature: models are filtered through authenticated `GET /api/v1/models/user`, with per-model context/output/pricing taken from the key's view and regional guardrails respected.

5. **[#10847 [OPEN] fix: preserve legacy model catalog route on publish](https://github.com/earendil-works/pi/pull/10847)** — Publisher was replacing the legacy catalog entry on every publish, breaking Pi 0.80.8–0.84.4 clients (which fetch from pi.dev but still use `openai-completions`). Fixes #9099.

6. **[#10718 [OPEN] fix(coding-agent): include system prompt in `--export` HTML](https://github.com/earendil-works/pi/pull/10718)** — Brings non-interactive export to parity with interactive `/export`, restoring the system prompt from the current branch for auditability.

7. **[#10533 [CLOSED] fix(durable): reject waits that close a cycle](https://github.com/earendil-works/pi/pull/10533)** — Cyclic waits now fail at the offending wait rather than hanging, matching existing self/owner-wait behavior. Fixes #10411; important robustness win for the durable runtime.

8. **[#10834 [CLOSED] fix(coding-agent): fall back after Bun image worker null](https://github.com/earendil-works/pi/pull/10834)** — Treats a `null` from the Bun-compiled resize worker as a miss and falls through to URL/http workers, restoring image attachments in standalone binaries (#10645).

9. **[#10830 [CLOSED] fix(coding-agent): bound retries across successful tool turns](https://github.com/earendil-works/pi/pull/10830)** — A successful `toolUse` response previously reset `retry.maxRetries`, so interleaved error/toolUse sequences could retry forever. Now retries are bounded across the turn.

10. **[#10820 [CLOSED] fix(ai): send `x-opencode-session` by endpoint, not provider id](https://github.com/earendil-works/pi/pull/10820)** — Generalizes the #9326 fix: the per-conversation routing header is now added based on endpoint rather than a hardcoded provider-id list, fixing other providers that proxy OpenCode zen endpoints.

*Also notable:* [#10739](https://github.com/earendil-works/pi/pull/10739) fixes `before_agent_start` being skipped for runs started by `pi.sendMessage(..., { triggerTurn: true })`, which previously corrupted the system prompt mid-run and broke provider prompt caching.

## Hot Discussions

**Ideas / Q&A**

- **[#10841 [General] Where should a package put a one-shot pre-turn routing decision?](https://github.com/earendil-works/pi/discussions/10841)** — The MatPilot team is building a per-prompt classification layer backed by a Python `semantic-router` sidecar (<100 ms budget) that decides which research workflow to run, and is asking where in the package lifecycle an async, one-shot pre-turn decision belongs. No replies yet, but it's a concrete signal that extension authors need an officially-sanctioned "pre-turn hook with side-effectful async I/O" — related in spirit to [#10832](https://github.com/earendil-works/pi/issues/10832), which requests wrapping/observing session model calls.

## Feature Request Trends

- **Key-aware, opt-in model discovery.** v1.2.0 addresses per-key OpenRouter filtering; [#10866](https://github.com/earendil-works/pi/issues/10866) asks for a `modelDiscovery` setting so `/model` and `/scoped-models` can exclude providers enabled only via environment variables. Users want tighter, less noisy catalogs. [#10147](https://github.com/earendil-works/pi/issues/10147) pushes to fold `/scoped-models` into `/model`.
- **Extension API surface expansion.** [#10832](https://github.com/earendil-works/pi/issues/10832) (wrap/observe model calls for OpenTelemetry), [#10845](https://github.com/earendil-works/pi/issues/10845) (identify `taskId`/`purpose` per model request), [#10846](https://github.com/earendil-works/pi/issues/10846) (injectable daemon process for Worker environments), and [#10833](https://github.com/earendil-works/pi/issues/10833) (session entry ids on message events) all request deeper, structured observability hooks.
- **Privacy and multi-process safety.** [#10770](https://github.com/earendil-works/pi/issues/10770) (0700 session dirs) and [#10798](https://github.com/earendil-works/pi/issues/10798) (install lock for shared `.pi/npm`) are both about making the local footprint safe under real-world use — including parallel subagents.
- **Protocol/terminal compatibility.** [#10628](https://github.com/earendil-works/pi/issues/10628) (tmux OSC 52 passthrough), [#10863](https://github.com/earendil-works/pi/issues/10863) (overlays drawn under inline images), [#10862](https://github.com/earendil-works/pi/issues/10862) (hardware cursor column under mouse selection), and [#10836](https://github.com/earendil-works/pi/issues/10836) (preserve case-distinct tool names in Anthropic OAuth) form a steady stream of TUI and wire-protocol polish requests.
- **Tool routing guidance.** [#10860](https://github.com/earendil-works/pi/issues/10860) requests that builtin `grep`/`bash` descriptions state when text search is the wrong tool — a light prompt-engineering lever on tool selection.

## Developer Pain Points

- **WSL and Windows remain the highest-friction platforms.** #6187 (28 comments) and #9361 (14 comments) top the window, spanning OAuth timing, shell resolution, and PATH fallback behavior. The underlying WSL clock-drift problem is being patched at the OAuth layer rather than fixed at the source.
- **Session persistence is lossy at the edges.** Resuming a session silently drops dynamically activated tools (#10785), and a session outliving its release loses not-yet-loaded providers after `pi update` prunes old builds (#10864: `Cannot find module`). Both are "everything looks fine until it doesn't" failure modes.
- **Silent degradation rather than loud failure.** Image attachments vanish in compiled binaries (#10645), MCP direct-exposure tools are silently dropped by `--tools` (#10838), and clipboard "Copied!" flashes even when tmux swallowed the sequence (#10628). Users repeatedly report being told the operation succeeded when it did not.
- **Cancellation and retry semantics aren't airtight.** `abort()` can still be followed by a live provider request (#10705), and retry budgets can be reset indefinitely by interleaved successful tool turns (#10830). For SDK embedders, these translate directly to unbounded cost and control-flow violations.
- **Anthropic-specific wire-format fragility.** Stale thinking blocks replayed after compaction (#9391) and tool-name casing collisions under the OAuth adapter (#10836) both require users to understand provider-internal behavior to work around.
- **Dependency-range and catalog drift.** [#10865](https://github.com/earendil-works/pi/issues/10865) shows `^1.0.4` resolving `pi-codemode@1.2.0` and failing to load under Node 22.23.3, while [#10847](https://github.com/earendil-works/pi/pull/10847) shows catalog publishing clobbering legacy client routes — version compatibility management across the monorepo remains a recurring tax.

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-10-13

## Today's Highlights
The 2026-10-13 digest is dominated by Managed Agent architecture and multi-agent durability work: a high-traffic dual-path proposal (#12380), Kubernetes runtime tracking (#13395), and new PRs for member-to-member messaging, agent identity over ACP/daemon, and L4 Shell/MCP lifecycle. The latest nightly release includes managed-agent connector/broker robustness and additional managed-agent feature work. Meanwhile, developers are actively flagging CI checkout instability, Linux Desktop packaging crashes, and UI/rendering issues as near-term reliability concerns.

## Releases
- **v0.25.1-nightly.20261012.18f80f5666** — [release link](https://github.com/QwenLM/qwen-code/releases/tag/v0.25.1-nightly.20261012.18f80f5666)  
  Visible changes include `fix(managed-agent): connector and broker robustness from the #12692 R2 review` by @wenshao in [#13330](https://github.com/QwenLM/qwen-code/pull/13330), plus at least one additional managed-agent feature entry that is truncated in the release notes. This is a nightly, not a stable release.

## Hot Issues
- **#12380 — proposal(serve): Define Managed Agent dual-path architecture and staged delivery** ([link](https://github.com/QwenLM/qwen-code/issues/12380)) — 56 comments. The central architecture thread for durable Sessions, Workspace bindings, recoverable tool executions, and a stable Web Shell path while preserving the TypeScript agent loop. Community reaction is heavy and still need-discussion.
- **#13395 — tracking(runtime): Kubernetes tool runtime progress and cross-platform delivery gates** ([link](https://github.com/QwenLM/qwen-code/issues/13395)) — 19 comments. Tracks K8s tool runtime delivery and platform-distribution gates against the current main baseline. High-signal for infrastructure and release readiness.
- **#13492 — XML tool-call recovery drops outer calls containing quoted tool markup** ([link](https://github.com/QwenLM/qwen-code/issues/13492)) — 11 comments. Core tool-call parsing bug: quoted tool markup is partially fixed, but outer-call recovery remains open pending PR #13579. Important for agent reliability.
- **#13785 — Multi-Agent API: agent-identity dimension on the public contract** ([link](https://github.com/QwenLM/qwen-code/issues/13785)) — 7 comments. Requests that multi-agent execution be attributable, tree-shaped, and interruptible on the existing public contract. Marked ready-for-human and need-discussion.
- **#13502 — Cancellation provenance: five decisions deferred out of PR 13436** ([link](https://github.com/QwenLM/qwen-code/issues/13502)) — 6 comments. Deals with cancellation semantics, legacy timestamp compatibility, and terminal records. Currently blocked; important for session-management correctness.
- **#14011 — Harness SWE: feature-type benchmark tasks that exercise Qwen Code’s own features** ([link](https://github.com/QwenLM/qwen-code/issues/14011)) — 5 comments. Proposes realistic feature-implementation benchmarks for todo tracking, plan mode, skills/workflows, and subagent delegation. New and timely for measuring feature quality.
- **#13898 — Web Shell: show live status of background subagents** ([link](https://github.com/QwenLM/qwen-code/issues/13898)) — 5 comments. Requests always-visible status for background Agent runs: running/done, elapsed time, remaining count. Directly addresses multi-agent observability.
- **#13837 — Chat view breaks mixed RTL/LTR text: code blocks forced LTR, no bidi isolation** ([link](https://github.com/QwenLM/qwen-code/issues/13837)) — 5 comments. Affects Hebrew/Arabic mixed with English technical strings. Has `feature/need-help` and `welcome-pr`, indicating maintainer openness.
- **#14004 — Separate the agentic harness from the LLM subscription to remove the conflict of interest** ([link](https://github.com/QwenLM/qwen-code/issues/14004)) — 4 comments. Argues the same vendor controls token pricing/quota and the harness spending them. A policy/product discussion with need-discussion status.
- **#13680 — Linux Desktop vendored ripgrep SIGSEGVs on every startup probe** ([link](https://github.com/QwenLM/qwen-code/issues/13680)) — 4 comments, closed. P1 platform bug: vendored x64-linux ripgrep was rewritten post-link, causing startup crashes. Closed but representative of packaging fragility.

## Key PR Progress
- **#13996 — feat(sdk-java): add private CSI Session and native SQL authority** ([link](https://github.com/QwenLM/qwen-code/pull/13996)) — Adds explicit Session creation, durable request pinning, native journal/resource proof, immutable SQL execution grants, and authenticated worker bind/history/execute. A major SDK Java authority block.
- **#13955 — feat(managed-agent): member-to-member messages ride the session_command channel (H4e-b3c)** ([link](https://github.com/QwenLM/qwen-code/pull/13955)) — Enables open-team members to message each other end-to-end without spinning up a lead model turn; `send_message` now supports `to`.
- **#13902 — feat: project agent identity and Agent Team delivery onto the ACP/daemon wire (refs #13785)** ([link](https://github.com/QwenLM/qwen-code/pull/13902)) — Projects agent identity and team coordination onto ACP/daemon frames with `_meta.agent` metadata and distinct subagent/team collaboration columns.
- **#13931 — feat(managed-agent): add L4 Shell and MCP session lifecycle** ([link](https://github.com/QwenLM/qwen-code/pull/13931)) — Adds durable close/delete for `hosted-workspace-shell/1` and `hosted-workspace-mcp/1`, settling effects and SessionEnd/SessionDelete hooks before detaching.
- **#13973 — feat(web-shell): Reuse standard UI for Managed Agents** ([link](https://github.com/QwenLM/qwen-code/pull/13973)) — Managed Agent creation and conversations reuse the standard Web Shell welcome page, composer, message layout, and native Workspace/model controls.
- **#14003 — feat(cli): add repeatable --disable-mcp-server to exclude servers per invocation** ([link](https://github.com/QwenLM/qwen-code/pull/14003)) — Adds a repeatable CLI flag to exclude named MCP servers for a single invocation without mutating `settings.json`.
- **#14016 — fix(cli): give bare ↑/↓ back to input history in Virtualized History** ([link](https://github.com/QwenLM/qwen-code/pull/14016)) — Restores input-history cycling for bare arrow keys; transcript scrolling moves to `Shift+↑/↓`, `PgUp/PgDn`, and `Ctrl+Home/End`.
- **#13731 — fix(channels): preserve Markdown structure when splitting messages** ([link](https://github.com/QwenLM/qwen-code/pull/13731)) — DingTalk and WeCom Markdown splitting now breaks near a 3,800-unit target at Markdown boundaries, preserving links, tables, and code blocks.
- **#13862 — fix(sdk-python): resolve Windows command shims on the effective PATH** ([link](https://github.com/QwenLM/qwen-code/pull/13862)) — Fixes Python SDK startup on Windows when npm exposes the CLI through `qwen.cmd`; resolution returns an absolute path against the effective child PATH.
- **#13642 — feat(runtime-broker): add bounded retention for terminal JDBC history** ([link](https://github.com/QwenLM/qwen-code/pull/13642)) — Adds opt-in bounded cleanup for old terminal Runtime Broker history under retired bindings, running outside request handling on a dedicated scheduler.

## Feature Request Trends
- **Managed-agent and multi-agent durability**: #12380, #13785, #13902, #13955, #13931, #13898, #13937, #13838, #13984. The strongest direction is durable session ownership, agent identity, team coordination, and observability.
- **Cross-platform runtime and packaging**: #13395, #13680, #13862, #13967, #13642. Requests cluster around Kubernetes runtime, Windows shims, Linux packaging, and IDE localization.
- **UI/UX, i18n, and accessibility**: #13898, #13837, #6149, #13870, #13967, #13998, #14016. Users want better Web Shell/Desktop/TUI polish, bidi support, locale-aware UI, and predictable keyboard behavior.
- **Session lifecycle, cancellation, and recovery**: #12380, #13502, #13838, #13984, #13992, #13937. Multiple issues target restart-proof recovery, cancellation provenance, and held-runtime cleanup.
- **MCP and tooling/search integration**: #13987, #14003, #14009, #13931. Requests include free web-search MCP providers, per-invocation MCP exclusion, and durable MCP session lifecycle.
- **Memory, context, and benchmark quality**: #13612, #14011, #13926. Interest is growing in controllable memory extraction cadence and benchmarks that exercise Qwen Code’s native features.
- **CI/test reliability**: #13951, #12714, #14012, #13935. Persistent requests to stabilize integration tests and managed-agent fixtures.

## Developer Pain Points
- **Managed-agent recovery edge cases**: #13838, #13937, #13984, #13992, #13816, #13986. Developers are repeatedly hitting restart, reattachment, proof-gate, and mount-release corner cases after control-plane or harness interruptions.
- **CI instability and infrastructure flakiness**: #13951, #12714, #14012, #13935. Failures such as `fatal: pack has 1 unresolved delta` and main-branch test failures slow down PR velocity.
- **Platform-specific startup/configuration failures**: #13680, #13943, #13130, #13862. Linux ripgrep SIGSEGVs, `.lsp.json` BOM parsing, Windows command shims, and trusted-workspace lockouts remain high-friction.
- **CLI/settings recompute bugs**: #14009, #14003. Ephemeral CLI overrides like `--disable-mcp-server` can be lost when settings are recomputed, causing disabled servers to reconnect.
- **UI/rendering regressions and ergonomics**: #13837, #6149, #13998, #14016, #13870. Mixed RTL/LTR text, TUI scrolling/link interaction, turn collapse hiding final answers, and keyboard history behavior are recurring complaints.
- **Memory extraction control**: #13612. Follow-ups after #13571 show demand for finer cadence, boundary flushes, window bounds, and telemetry.
- **Release process quality**: #13925. Duplicate PRs in release notes were reported and closed, but the issue highlights ongoing release-note hygiene concerns.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*