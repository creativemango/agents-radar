# AI CLI Tools Community Digest 2026-10-14

> Generated: 2026-10-14 01:21 UTC | Tools covered: 7

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

# Cross-Tool AI CLI Ecosystem Report — 2026-10-14

> **Data caveat:** The digests provide “hot/selected” items, not full repository totals. Counts below are digest-visible counts. **N/A** means no discussion data was provided or the channel is not used; it does not necessarily mean zero activity.

---

## 1. Ecosystem Overview

The AI CLI ecosystem is consolidating around three hard problems: **agent cost/limit enforcement**, **long-session durability**, and **cross-surface reliability**. Release cadence remains high in some tools—Codex, Gemini CLI, Copilot CLI, and Qwen Code all shipped builds—while Claude Code, OpenCode, and Pi had no releases in the window. The issue trackers show that users increasingly treat these tools as production automation surfaces, not chat toys: they demand spend caps, restart-safe sessions, subagent observability, sandbox clarity, and predictable MCP/plugin behavior. Meanwhile, provider/model flexibility—BYOK, OpenAI-compatible backends, Bedrock/Vertex routing, and rapid model API drift—has become a major differentiator and pain point. Windows, IDE parity, and terminal rendering remain persistent friction points across otherwise mature products.

---

## 2. Activity Comparison

| Tool | Issues surfaced | PRs surfaced | Discussions surfaced | Release status |
|---|---:|---:|---:|---|
| **Claude Code** | 10 hot; heavy issue traffic | 2 | N/A | None in last 24h |
| **OpenAI Codex** | 10 hot | 10 selected | 7 | 3 Rust alphas: `0.163.0-alpha.8/.9/.10` |
| **Gemini CLI** | 10 hot + also active | 10 key + 7 watchlist | N/A | `v0.64.0`; `v0.65.0-preview.0`; nightly |
| **GitHub Copilot CLI** | 10 hot | 0 updated | N/A | `v1.0.98-1`; `v1.0.98-0`; `v1.0.97`; `v1.0.97-3` |
| **OpenCode** | 10 hot | 10 key + 2 notable | N/A | None in last 24h |
| **Pi** | 10 hot | 10 key | N/A | None in last 24h |
| **Qwen Code** | 10 hot + honorable mentions | 10 key + 6 notable | N/A | `v0.25.2-preview.0`; `v0.25.1-nightly` |

**Notes:**
- Claude Code remains issue-driven; external PR volume is minimal.
- Codex has the richest discussion surface among the set, with Show-and-tell and Q&A activity.
- Copilot CLI shipped multiple patch releases but had no PR updates in the window.
- Gemini CLI, OpenCode, Pi, and Qwen Code all show active PR-based development.

---

## 3. Shared Feature Directions

### Cost and token governance
- **Claude Code:** per-session spend caps, background subagent token/turn/time caps, orchestrator concurrency limits.
- **OpenCode:** configurable prompt-cache TTL, retry/backoff policy, cost/latency governance.
- **Codex:** token receipt tooling, reset alerts, cost-estimation community projects.
- **Pi:** cache-preserving reasoning-effort changes, compaction token-cap reliability.
- **Gemini CLI / Qwen Code:** token-frugal context access, reactive compaction correctness, tool-loading cost evaluation.

**Shared need:** move from reactive cost surprises to enforceable budgets, cache control, and visible spend attribution.

### Session durability, resume, and compaction
- **Claude Code:** scheduled-task silent failures, daily re-login, stale cloud branches.
- **Codex:** stream disconnects, remote compaction failures, cross-device state sync, queue management.
- **OpenCode:** V2 migration hiding sessions, reconnect failures, post-compact tool amnesia.
- **Pi:** resumed sessions dropping dynamically activated tools, compaction summarisation caps.
- **Qwen Code:** hosted session takeover loss, wake inputs not executed after harness death.
- **Copilot CLI:** session rename/exit inconsistency, CLI/Desktop sync gaps.

**Shared need:** sessions must survive restarts, network interruptions, compaction, upgrades, and multi-surface handoffs without silent state loss.

### Subagent/orchestration reliability and observability
- **Claude Code:** uncapped background subagents, orchestrator ignoring project limits, VS Code OOM with parallel agents.
- **Gemini CLI:** subagent hangs, MAX_TURNS misreported as success, browser-agent settings ignored.
- **Pi:** tool-state restoration and malformed tool-call handling.
- **Qwen Code:** multi-agent durability, child-result recovery, broker takeover.
- **Codex:** Guardian review handling, async compaction, daemon restart behavior.

**Shared need:** enforceable concurrency, accurate termination reporting, approval gates, and auditable subagent actions.

### IDE, OS, and cross-surface parity
- **Claude Code:** Zed integration, VS Code chat syntax highlighting, Windows installer, Linux desktop auth.
- **Codex:** Windows Desktop, VS Code sandbox, iOS remote projects.
- **Copilot CLI:** Windows MCP OAuth loops, enterprise policy friction, CLI/Desktop sync.
- **OpenCode:** Windows desktop terminal, V2 non-git session visibility.
- **Gemini CLI:** Wayland browser subagent failures, tmux rendering.
- **Pi:** TUI Home/End, hyperlinks, tmux images, rxvt-unicode wheel behavior.

**Shared need:** users expect the same core experience across terminal, IDE, desktop, web, and mobile.

### Sandboxing, permissions, and security
- **Claude Code:** safety-filter false positives on legitimate security work.
- **Gemini CLI:** read-only untrusted workspaces, OS sandboxing, least-privilege defaults.
- **Codex:** sandbox/approval transparency, per-project network access, stale locks.
- **Copilot CLI:** file-scoped write permissions, overbearing prompts.
- **Qwen Code:** provider error diagnostics, managed-agent isolation.
- **Pi:** OAuth identity persistence, side-effect-free SDK registration.

**Shared need:** security controls must be explainable, scoped, and not block legitimate reverse-engineering, security research, or local development.

### Provider/model compatibility and BYOK
- **Copilot CLI:** BYOK model discovery, short-name matching, per-agent default models, sampling defaults.
- **OpenCode:** GitHub Copilot provider, Bedrock region catalogs, OpenAI-compatible Vertex.
- **Pi:** provider routing, model aliases, Gemini 4 thinking controls, GPT-6 reasoning-effort caching.
- **Qwen Code:** vLLM/SGLang tool serialization and context-overflow detection.
- **Codex:** model catalog/auth parity for ChatGPT users.
- **Gemini CLI / Copilot CLI:** Gemini 4 Argon support in flight/shipped.

**Shared need:** a stable abstraction over rapidly changing model APIs, with clear auth, catalog discovery, and backend-specific behavior.

### MCP/plugin extensibility
- **Claude Code:** Mods/hooks/plugins megathread, WebView/GUI marketplace requests.
- **Codex:** MCP/connectors, browser/computer-use robustness, plugin/skill metadata.
- **Copilot CLI:** plugin hooks, MCP status sync, OAuth initialization.
- **OpenCode:** plugin permission assertions, remote Figma MCP, web-search plugins.
- **Qwen Code:** ACP deferred MCP tools, MCP server lifecycle resilience.
- **Pi:** provider extensions, MCP client identification.

**Shared need:** MCP and plugins are becoming first-class integration paths, but auth, discovery, status, and hook reliability remain immature.

---

## 4. Differentiation Analysis

| Tool | Feature focus | Target users | Technical approach |
|---|---|---|---|
| **Claude Code** | Extensibility via Mods, cost governance, IDE parity, safety-filter behavior | Anthropic-centric pro developers, security engineers, teams | TUI + IDE extensions, hooks/plugins, background subagents; issue-driven roadmap |
| **OpenAI Codex** | Windows Desktop, Computer Use/browser/MCP, queue management, cross-device sync | OpenAI/ChatGPT users, Windows desktop and VS Code users | Rust CLI/runtime, app-server daemon, Guardian review, async compaction |
| **Gemini CLI** | Google model integration, subagent reliability, terminal rendering, sandboxing | Google Cloud/Vertex users, open-source contributors, Linux/tmux users | Node/TypeScript CLI, ACP/A2A server, browser agent, OS sandboxing, PR-driven |
| **GitHub Copilot CLI** | Enterprise GitHub integration, BYOK, MCP/plugin hooks, Windows auth | GitHub enterprise, Copilot subscribers, BYOK/custom-provider users | CLI + ACP, plugin hooks, MCP, frequent patch releases |
| **OpenCode** | V2 stabilization, provider breadth, plugin permissions, streaming performance | Open-source/self-hosted users, multi-provider developers | Server/client architecture, TUI/desktop, managed config, V2 API migration |
| **Pi** | Provider/model routing, session/tool-state durability, TUI polish, Nix packaging | Terminal power users, multi-provider/Nix users, embedders | Provider abstraction, SDK, TUI, export/Web UI experiments |
| **Qwen Code** | Hosted managed-agent durability, Java SDK, multi-agent coordination | Enterprise/managed-agent users, Java/self-hosted backends | Runtime Broker, MySQL persistence, ACP, managed-agent control plane |

**Key differentiators:**
- **Claude Code** is the most extensibility- and governance-oriented, but its external PR culture is weak.
- **Codex** is pushing frontier desktop/browser/Computer Use features, with Windows as its biggest liability.
- **Gemini CLI** is strongest on open-source PR throughput and Google-model integration, but subagent hangs and TUI rendering are recurring.
- **Copilot CLI** is enterprise/GitHub-first and BYOK-heavy; its Windows/MCP auth issues are its main drag.
- **OpenCode** is in a V2 transition, with provider breadth and plugin permissioning as its main bets.
- **Pi** is a power-user/multi-provider tool with strong provider-routing and TUI concerns.
- **Qwen Code** is the most enterprise-managed-agent-focused, with durability and provider-compatibility as top issues.

---

## 5. Community Momentum & Maturity

### Highest-velocity communities
- **Claude Code:** Highest-engagement issue in the set (`#91870`, 256 comments; `#18435`, 895 upvotes). Strong signal, but sparse external PR contribution.
- **OpenAI Codex:** High issue engagement (`#49458`, 79 comments), active PR batch, and the only rich Discussions surface. Fast alpha cadence.
- **Gemini CLI:** High PR volume, steady releases, and active issue churn around subagents and rendering. Strong open-source contribution model.
- **OpenCode:** Very active issue/PR flow, especially V2 stabilization; high-comment free-model quota thread (`#15585`, 56 comments).
- **Qwen Code:** Focused but active; managed-agent durability and provider compatibility drive P1/P2 issues and PRs.

### More mature but lower PR cadence
- **GitHub Copilot CLI:** Frequent releases and active issue tracker, but no PR updates in the window. Maturity is visible in release engineering; community contribution appears more issue-driven.
- **Pi:** Smaller community, but active across issues and PRs. Strong technical depth in provider/model routing and TUI edge cases.

### Momentum signals
- **Rapid iteration:** Codex (three alphas), Gemini CLI (preview + nightly + stable), Copilot CLI (four patch releases), Qwen Code (preview + nightly).
- **Stabilization mode:** OpenCode is clearly in V2 stabilization; Pi is hardening session/tool state.
- **Trust-sensitive phase:** Quota ambiguity, silent failures, and cost overruns are increasingly treated as trust issues, not just bugs.

---

## 6. Trend Signals

### 1. Cost control is becoming a core product feature
Across Claude Code, Codex, OpenCode, Pi, and Qwen Code, users want enforceable budgets, cache TTL control, token receipts, and approval gates. Tools that ship first-class spend governance will win production trust.

### 2. Long-running sessions must be restart-safe and compaction-safe
Session loss, tool-state amnesia, stream disconnects, and compaction failures appear in every major tool. The next competitive baseline is **durable, resumable, observable sessions** across CLI, IDE, desktop, and mobile.

### 3. Subagent orchestration needs runtime enforcement
Concurrency caps, token/turn/time limits, accurate termination reasons, and subagent traces are repeated asks. Orchestrators that can ignore written limits are a production risk.

### 4. BYOK and provider abstraction are table stakes
Copilot CLI, OpenCode, Pi, and Qwen Code all show significant BYOK/custom-provider pain. Developers want model discovery, aliases, per-agent defaults, correct sampling, and backend-specific error handling.

### 5. MCP/plugin ecosystems are maturing but fragile
Plugin hooks, MCP auth, tool discovery, and context merging are active across Claude Code, Codex, Copilot CLI, OpenCode, and Qwen Code. The ecosystem needs clearer status, retries, and permission models.

### 6. Windows and non-VS Code surfaces remain underserved
Windows Desktop, Zed, Wayland, tmux, and Linux auth issues recur. Cross-platform parity is now a baseline expectation, not a differentiator.

### 7. Security controls must not block legitimate work
Safety-filter false positives on security research, sandbox confusion, and opaque policy blocks are recurring. The winning posture is **scoped, explainable, bypassable-with-audit** security—not blanket denial.

### 8. Terminal UX still matters
Flicker, border bleed, Home/End behavior, hyperlinks, and resize handling remain high-frequency complaints. Even advanced agent users spend most of their time in the terminal.

### Reference value for developers
- **If building agent tooling:** prioritize cost caps, session durability, and subagent observability.
- **If integrating providers:** assume model API drift and design for BYOK/custom backends from day one.
- **If shipping plugins/MCP:** invest in auth, discovery, status, and hook reliability early.
- **If targeting enterprises:** Windows, policy errors, managed config, and auditability are non-negotiable.
- **If targeting power users:** TUI polish, provider routing, and cross-surface continuity drive retention.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills Community Highlights — as of 2026-10-14

> Data note: PR comment counts are `undefined` in the supplied dataset. The PR ranking below preserves the repository’s “sorted by comments” top-20 order; all listed PRs are currently **OPEN**.

## 1. Top Skills Ranking — Most-Watched PRs

1. **[mcp-builder: support mcp>=2 streamable_http_client import and custom headers](https://github.com/anthropics/skills/pull/1742)** — OPEN  
   Fixes MCP v2 compatibility in `mcp-builder`: `streamablehttp_client` rename and custom headers via `create_mcp_http_client` / `http_client`. Links to issue #1668. High-impact core Skill maintenance.

2. **[skill-creator: isolate trigger evals and handle Windows and runtime failures](https://github.com/anthropics/skills/pull/1298)** — OPEN  
   Addresses false misses/invalid scores in trigger evaluation, Windows subprocess pipe issues, and runtime failures incorrectly passing negative examples. Directly strengthens the Skill authoring loop.

3. **[proofcore-contract-auditor for smart contract notarization](https://github.com/anthropics/skills/pull/1771)** — OPEN  
   New Web3 Skill for static analysis of Solidity/Rust contracts and cryptographic audit-proof anchoring on TON via a Merkle protocol.

4. **[Detect orphaned docx comments](https://github.com/anthropics/skills/pull/1734)** — OPEN  
   Improves the `docx` Skill by detecting orphaned comments. Minimal summary, but part of the active document-processing cluster.

5. **[Add md2video-audio skill](https://github.com/anthropics/skills/pull/1703)** — OPEN  
   New zero-cost Skill that compiles Markdown into MP4 videos with voiceovers using Marp and related tooling.

6. **[Add notion-spec-to-implementation and quantitative-resume-auditor skills](https://github.com/anthropics/skills/pull/1245)** — OPEN  
   New productivity Skills: one turns Notion specs into implementation tasks with acceptance criteria; the other audits resumes quantitatively.

7. **[skill-creator: harden eval viewer — script breakout, DNS rebinding, cross-site POST, escaping](https://github.com/anthropics/skills/pull/1961)** — OPEN  
   Security hardening for the local eval viewer used by `skill-creator`. Important because the viewer renders untrusted model-written eval outputs.

8. **[docx: report LibreOffice timeout as an error and verify the output](https://github.com/anthropics/skills/pull/1792)** — OPEN  
   Prevents false success when `soffice` times out and verifies that revision marks (`w:ins` / `w:del` / `w:moveFrom` / `w:moveTo`) are actually removed.

## 2. Community Demand Trends — From Issues

The most-commented Issues show demand clustering around **platform trust, Skill reliability, and enterprise readiness**, plus several domain-specific automation directions.

- **Security, trust boundaries, and governance**  
  [#492 — Community skills under `anthropic/` namespace enable trust boundary abuse](https://github.com/anthropics/skills/issues/492) (46 comments) is by far the most-discussed issue. Related: [#1394 eval-viewer XSS](https://github.com/anthropics/skills/issues/1394), [#1175 SharePoint access-control concerns](https://github.com/anthropics/skills/issues/1175), [#412 agent-governance proposal](https://github.com/anthropics/skills/issues/412).

- **Skill lifecycle reliability: evals, triggers, benchmarks, packaging**  
  [#556 run_eval.py never triggers skills/commands](https://github.com/anthropics/skills/issues/556) (12 comments), [#1383 silent benchmark failures](https://github.com/anthropics/skills/issues/1383), [#1352 parallel workers cross-match skill UUIDs](https://github.com/anthropics/skills/issues/1352), [#1390 mcp-builder evaluation scores 0/N](https://github.com/anthropics/skills/issues/1390), [#202 skill-creator best-practice update](https://github.com/anthropics/skills/issues/202).

- **Enterprise sharing and administration**  
  [#228 Enable org-wide skill sharing in Claude.ai](https://github.com/anthropics/skills/issues/228) (16 comments, 8 👍) asks for shared Skill libraries and direct sharing. [#189 duplicate skills from `document-skills` and `example-skills`](https://github.com/anthropics/skills/issues/189) (6 comments) highlights packaging/overlap problems.

- **Context efficiency and memory compression**  
  [#1487 `claude-api` injects ~156k tokens in one tool call](https://github.com/anthropics/skills/issues/1487) and [#1329 compact-memory symbolic notation](https://github.com/anthropics/skills/issues/1329) (9 comments) point to demand for token-efficient Skill design and compact agent state.

- **MCP/API integration correctness**  
  MCP v2 import/header fixes and evaluation failures show the community wants Skills to stay compatible with fast-moving MCP and Claude API surfaces.

- **Domain-specific automation**  
  Recurring directions include document quality/typography, ODT/ODS, DOCX comment handling, Markdown-to-video, HPC/Slurm workflows, E2E browser testing, smart-contract auditing, Notion spec-to-task automation, and resume auditing.

## 3. High-Potential Pending Skills

PR comment counts are unavailable, so this selection uses recent update activity, linked issues, and likely ecosystem impact. All are OPEN and unmerged as of the data snapshot.

| PR | Skill / Area | Why it may land soon |
|---|---|---|
| [#1742](https://github.com/anthropics/skills/pull/1742) | `mcp-builder` MCP v2 compatibility | Updated 2026-10-08; fixes issue #1668; critical for MCP Skills. |
| [#1298](https://github.com/anthropics/skills/pull/1298) | `skill-creator` trigger evals | Updated 2026-09-16; fixes Windows/runtime eval failures. |
| [#1961](https://github.com/anthropics/skills/pull/1961) | `skill-creator` eval-viewer security | Updated 2026-10-11; security hardening likely prioritized. |
| [#1997](https://github.com/anthropics/skills/pull/1997) | `skill-creator` YAML frontmatter parsing | Updated 2026-10-13; recent correctness fix for eval/description optimization. |
| [#1980](https://github.com/anthropics/skills/pull/1980) | `webapp-testing` shell injection | Updated 2026-10-06; removes `shell=True` command-injection risk. |
| [#1977](https://github.com/anthropics/skills/pull/1977) | `algorithmic-art` wrapAround | Updated 2026-10-07; fixes negative-value wrapping bug (#1897). |
| [#1681](https://github.com/anthropics/skills/pull/1681) | `package_skill.py` direct execution | Updated 2026-10-08; improves Skill packaging/developer UX. |
| [#1730](https://github.com/anthropics/skills/pull/1730) | `claude-api` dead URLs | Updated 2026-10-04; docs correctness, low-risk merge. |
| [#1771](https://github.com/anthropics/skills/pull/1771) | `proofcore-contract-auditor` | New Web3 smart-contract auditing Skill; updated 2026-09-16. |
| [#1703](https://github.com/anthropics/skills/pull/1703) | `md2video-audio` | New media-generation Skill; updated 2026-09-15. |
| [#1245](https://github.com/anthropics/skills/pull/1245) | Notion spec + resume auditor | New productivity Skills; updated 2026-09-30. |
| [#1615](https://github.com/anthropics/skills/pull/1615) | `scnet-hpc` | New HPC/Slurm Skill; updated 2026-08-24. |
| [#822](https://github.com/anthropics/skills/pull/822) | AWT E2E testing | New browser-testing Skill; updated 2026-09-19. |
| [#514](https://github.com/anthropics/skills/pull/514) | `document-typography` | New document-quality Skill; updated 2026-03-13. |
| [#486](https://github.com/anthropics/skills/pull/486) | ODT/ODS Skill | New OpenDocument Skill; updated 2026-04-14. |
| [#83](https://github.com/anthropics/skills/pull/83) | skill-quality + skill-security analyzers | New meta Skills; updated 2026-01-07. |

## 4. Skills Ecosystem Insight

The community’s most concentrated demand is not a single domain Skill but a hardened Skills platform: secure trust boundaries, reliable trigger/eval infrastructure, and enterprise-ready sharing, with domain Skills building on top.

---

# Claude Code Community Digest — 2026-10-14

## 1. Today's Highlights

No new releases shipped in the last 24h, but issue traffic remains heavy: the **Mods** extensibility megathread (#91870) is now at 256 comments as the team works through feedback on the new plugin/hook affordance. The dominant community signals today are **runaway agent cost** (uncapped background subagents, orchestrators ignoring project limits) and **IDE/platform parity** requests (Zed integration, VS Code chat rendering). A large batch of `[Bug][cyber]` safety-filter false-positive reports from a single reporter was closed en masse, mostly as duplicates or stale.

## 2. Releases

None in the last 24h.

## 3. Hot Issues

1. **[#91870](https://github.com/anthropics/claude-code/issues/91870) — Mods: make Claude 10x more extensible** · 256 comments · 👍131
   The central thread for the new extensibility surface (hooks + plugins). Maintainers posted a community micro-update on Oct 1 confirming active iteration on feedback. Highest-engagement issue in the repo and the best place to track the Mods API direction.

2. **[#18435](https://github.com/anthropics/claude-code/issues/18435) — Multi-account management in Claude Desktop** · 200 comments · 👍895
   The single most-upvoted open request in this dataset. Users want profile switching for work/personal accounts without re-auth. Persisting since January with no shipped solution — a clear signal that account ergonomics is a top-tier gap.

3. **[#26457](https://github.com/anthropics/claude-code/issues/26457) — Windows 11 reinstall fails on signature verification** · 31 comments · 👍20
   Long-running installer/signature bug blocking Windows users from upgrading or reinstalling. High severity because it strands users on old builds with no workaround.

4. **[#32362](https://github.com/anthropics/claude-code/issues/32362) — Zed IDE integration support** · 20 comments · 👍54
   `/ide` returns "No available IDEs detected" under Zed. Second-most-upvoted item here; the community increasingly expects parity beyond VS Code and JetBrains.

5. **[#64968](https://github.com/anthropics/claude-code/issues/64968) — Syntax highlighting in VS Code chat panel** · 15 comments · 👍37
   Fenced code blocks render as plain monochrome text regardless of language tag or theme. A reopened request — notable that it was closed once before, and users are still pushing.

6. **[#89812](https://github.com/anthropics/claude-code/issues/89812) — Linux Desktop forces daily re-login (`session_stale_relogin`)** · 15 comments · 👍5
   Elevated-auth sessions expire roughly daily, and Cowork folder grants demand sign-in twice. Reproducible, multi-platform, and disruptive to headless/desktop workflows.

7. **[#95966](https://github.com/anthropics/claude-code/issues/95966) — Scheduled tasks silently skip their fire window** · 10 comments
   Over a documented 5-day window, tasks failed to fire with no error and no run-history entry while both host and scheduler were demonstrably alive. Silent failure in an automation primitive is a trust problem.

8. **[#94013](https://github.com/anthropics/claude-code/issues/94013) — Background subagents have no token/turn/time cap** · 5 comments
   Three background research agents consumed **1.7M tokens** with no approval gate and no visible cost. Directly tied to the growing cost-control cluster below.

9. **[#101901](https://github.com/anthropics/claude-code/issues/101901) — VS Code extension host OOM-crash with 2+ parallel subagents** · 2 comments (filed 10/13)
   Extension host terminates within seconds-to-a-minute of launching two parallel background `Agent` subagents on Windows. Three mitigation attempts failed — a hard blocker for parallel agent workflows in the IDE.

10. **[#101650](https://github.com/anthropics/claude-code/issues/101650) — Orchestrator ignored written agent/batch limits, burned ~12% of weekly Max allowance** · 2 comments
    An Opus orchestration session exceeded a project-declared 6-agent concurrency cap, then retried its own load-induced timeouts for two hours with nothing merged. The clearest evidence yet that agent-level guardrails aren't enforced at runtime.

## 4. Key PR Progress

PR volume was minimal today — only two items, both closed and neither introducing product functionality:

- **[#44393](https://github.com/anthropics/claude-code/pull/44393) — fix: respect `DAYS_BACK` env var in backfill-duplicate-comments script** (closed)
  The workflow passed `DAYS_BACK` but the script ignored it, always falling back to `MIN/MAX_ISSUE_NUMBER`. The fix computes a cutoff date and filters by creation date, with early pagination termination. Tooling-only impact, but it touches the duplicate-comment triage path that many of today's issue closures flow through.

- **[#1](https://github.com/anthropics/claude-code/pull/1) — Create SECURITY.md** (closed)
  Historical housekeeping PR from Feb 2025, closed this cycle. No functional change.

> Note: the sparse PR list is itself worth flagging — this repo appears to absorb community contributions primarily through issues rather than external PRs, so the issue tracker is the real product roadmap signal.

## 5. Hot Discussions

No discussion data was provided for this period.

## 6. Feature Request Trends

Distilled from the open enhancement issues:

- **Extensibility / Mods platform** — The largest active theme. Requests cluster around making Mods more capable as a genuine plugin surface: [#91870](https://github.com/anthropics/claude-code/issues/91870) (core megathread), [#99213](https://github.com/anthropics/claude-code/issues/99213) (WebView pane for local pages, no server), and [#101910](https://github.com/anthropics/claude-code/issues/101910) (replace the TUI Plugins/Marketplace/MCP editors with a Web GUI).
- **Cost & token governance** — [#95964](https://github.com/anthropics/claude-code/issues/95964) asks for a per-session spend cap that works in *interactive* mode (`--max-budget-usd` is print-mode only); [#94013](https://github.com/anthropics/claude-code/issues/94013) wants caps and approval gates on background subagents; [#101650](https://github.com/anthropics/claude-code/issues/101650) shows the failure mode when limits aren't enforced.
- **IDE parity** — [#32362](https://github.com/anthropics/claude-code/issues/32362) (Zed) and [#64968](https://github.com/anthropics/claude-code/issues/64968) (VS Code chat syntax highlighting) represent demand for consistent experience across editors rather than a VS Code/JetBrains-only story.
- **Multi-account / auth ergonomics** — [#18435](https://github.com/anthropics/claude-code/issues/18435) (profile switching) plus [#89812](https://github.com/anthropics/claude-code/issues/89812) (session persistence) point at auth being the top friction area on Desktop.
- **Desktop reliability & automation correctness** — [#26457](https://github.com/anthropics/claude-code/issues/26457) (Windows installer), [#95966](https://github.com/anthropics/claude-code/issues/95966) (scheduled tasks), [#93585](https://github.com/anthropics/claude-code/issues/93585) (cloud sessions checking out stale branches).

## 7. Developer Pain Points

- **Unbounded autonomous spend.** The most acute and fastest-growing frustration. Background subagents run without token/turn/time caps ([#94013](https://github.com/anthropics/claude-code/issues/94013)), `--max-budget-usd` doesn't apply to interactive sessions ([#95964](https://github.com/anthropics/claude-code/issues/95964)), and orchestrators have been observed ignoring project-written concurrency limits outright ([#101650](https://github.com/anthropics/claude-code/issues/101650)). Users report spending a meaningful fraction of a weekly plan with nothing merged.
- **Safety-filter false positives on legitimate security work.** A single reporter's `[Bug][cyber]` cluster ([#75210](https://github.com/anthropics/claude-code/issues/75210), [#75104](https://github.com/anthropics/claude-code/issues/75104), [#88612](https://github.com/anthropics/claude-code/issues/88612), [#88613](https://github.com/anthropics/claude-code/issues/88613), [#75187](https://github.com/anthropics/claude-code/issues/75187), and others) was bulk-closed on 10/14 as duplicate/stale. The pattern — *session-halted* blocks on the user's own hardware, packet captures, memory dumps, and firmware inspection — is a recurring, reproducible workflow-killer for security engineers and reverse engineers, and the duplicate-closure strategy risks burying the signal.
- **Auth and session churn on Desktop.** Roughly daily forced re-logins on Linux ([#89812](https://github.com/anthropics/claude-code/issues/89812)) and no multi-account support ([#18435](https://github.com/anthropics/claude-code/issues/18435)) — 895 upvotes and counting.
- **Silent failures over loud ones.** Scheduled tasks that never fire and log nothing ([#95966](https://github.com/anthropics/claude-code/issues/95966)), and usage-credit gating that withholds prompts without a clear error ([#93886](https://github.com/anthropics/claude-code/issues/93886)), erode trust in automation more than crashes do.
- **Agent orchestration isn't load-safe in the IDE.** The VS Code extension host OOM-crashes with just two parallel subagents ([#101901](https://github.com/anthropics/claude-code/issues/101901)), while sub-agent permission prompts lack the main agent's auto-deny timeout, stalling unattended bypass-mode runs indefinitely ([#99899](https://github.com/anthropics/claude-code/issues/99899)).
- **Platform gaps remain.** Windows installer signature verification ([#26457](https://github.com/anthropics/claude-code/issues/26457)), stale branches on web/cloud sessions ([#93585](https://github.com/anthropics/claude-code/issues/93585)), and credit-balance discrepancies after top-up ([#31537](https://github.com/anthropics/claude-code/issues/31537)).

---
*Digest generated from github.com/anthropics/claude-code activity on 2026-10-14. No new releases or discussion data were available for this window.*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-10-14

## 1. Today’s Highlights
Codex published three Rust alpha releases (`0.163.0-alpha.8` through `.10`) with no substantive release notes beyond version names, indicating continued pre-release iteration. Community attention remains heavily concentrated on Windows Desktop reliability: Computer Use/browser/MCP failures, stream disconnects, compaction failures, and cross-device state sync dominate the issue tracker. PR activity closed a broad batch of internal improvements around background compaction, Guardian review handling, Windows WinGet daemon installation, TUI keybindings, and app-server daemon restarts.

## 2. Releases
- [rust-v0.163.0-alpha.10](https://github.com/openai/codex/releases/tag/rust-v0.163.0-alpha.10) — release notes only state “Release 0.163.0-alpha.10”; no detailed changes provided.
- [rust-v0.163.0-alpha.9](https://github.com/openai/codex/releases/tag/rust-v0.163.0-alpha.9) — same: no substantive changelog.
- [rust-v0.163.0-alpha.8](https://github.com/openai/codex/releases/tag/rust-v0.163.0-alpha.8) — same: no substantive changelog.

**Analyst note:** The rapid alpha cadence suggests active internal iteration on the Rust CLI/runtime, but without changelogs users should treat these as unstable pre-release builds.

## 3. Hot Issues
1. **[#49458](https://github.com/openai/codex/issues/49458) — Windows dot-started local tasks lack Computer Use tools while ordinary local Codex sessions work.**  
   High-impact Windows regression: dot-started tasks lose Computer Use capability. **79 comments / 26 👍** makes this the most engaged issue in the window.

2. **[#53188](https://github.com/openai/codex/issues/53188) — Windows Browser tools fail to initialize: MXC launcher HRESULT `0x80070003`.**  
   Browser tool initialization failure blocks Windows users from browser-based workflows. **19 comments**, though no upvotes yet.

3. **[#52839](https://github.com/openai/codex/issues/52839) — Sol Ultrafast omitted from first-party model catalog with ChatGPT authentication.**  
   Model catalog/auth parity issue: paying ChatGPT users cannot see or select Sol Ultrafast. **19 comments** indicate meaningful confusion and reproducibility interest.

4. **[#52919](https://github.com/openai/codex/issues/52919) — Windows / Codex Desktop + VS Code intermittent stream disconnects and reconnect failures since Oct 10.**  
   Connectivity instability affects both desktop and IDE extension workflows. **14 comments / 2 👍**.

5. **[#28864](https://github.com/openai/codex/issues/28864) — Add shortcuts to edit or remove specific queued follow-up inputs in the TUI.**  
   Long-running enhancement with strong community support: users need finer control over queued prompts. **13 comments / 24 👍**.

6. **[#47559](https://github.com/openai/codex/issues/47559) — VS Code agent sandbox fails before commands run: mountinfo path is not absolute.**  
   Marked **CLOSED**, but still notable because sandbox failures block command execution entirely. **12 comments / 6 👍**.

7. **[#48504](https://github.com/openai/codex/issues/48504) — Windows MCP HTTP responses fail with “error decoding response body”; app connectors unavailable.**  
   MCP/connector breakage on Windows affects extensibility and tool integrations. **10 comments**.

8. **[#36454](https://github.com/openai/codex/issues/36454) — iOS Remote Project List is not synchronized with Codex Desktop.**  
   Cross-device state mismatch: Desktop is not treated as source of truth for Projects. **10 comments**.

9. **[#35485](https://github.com/openai/codex/issues/35485) — Windows Desktop bundled `node_repl` MCP processes leak one-per-thread.**  
   Resource leak in bundled MCP runtime can degrade long-running Windows Desktop sessions. **9 comments**.

10. **[#50789](https://github.com/openai/codex/issues/50789) — macOS browser access remains blocked by conversation-level denied origin despite global/site Allow settings.**  
    Browser permission state appears inconsistent across global, site, and conversation scopes. **8 comments**.

## 4. Key PR Progress
*Selected PRs are marked CLOSED in this window; comment counts were not exposed.*

1. **[#53800](https://github.com/openai/codex/pull/53800) — Wire background compaction into the turn lifecycle.**  
   Launches background compaction at eligible pre-turn, mid-turn, and post-turn checkpoints when `AsyncCompaction` is enabled. Important for long-running session stability.

2. **[#53811](https://github.com/openai/codex/pull/53811) — Support daemon installation from Windows WinGet packages.**  
   Fixes layout mismatch where WinGet places CLI/code-mode host at package root, preventing daemon installation.

3. **[#53816](https://github.com/openai/codex/pull/53816) — Reject Guardian review threads during interactive CLI resume.**  
   Prevents resuming internal Guardian review threads, with guidance to resume the parent session instead.

4. **[#53805](https://github.com/openai/codex/pull/53805) — Add safe diagnostics to workspace routing configuration errors.**  
   Improves troubleshooting without exposing secrets in raw configuration errors.

5. **[#53794](https://github.com/openai/codex/pull/53794) — Retain rollout migration locks across worker cancellation.**  
   Prevents other writers/migrations from touching files still being compressed or decompressed by a cancelled worker.

6. **[#53790](https://github.com/openai/codex/pull/53790) — Fix cancellation of macOS user-verification prompts.**  
   Addresses Keychain signing contexts where biometric prompts could not be reliably cancelled.

7. **[#53763](https://github.com/openai/codex/pull/53763) — Enrich V2 capability discovery with resolved plugin and skill metadata.**  
   Returns qualified skill names, parsed dependencies, and policy metadata; shares `agents/openai.yaml` parsing.

8. **[#53754](https://github.com/openai/codex/pull/53754) — Support Command/Super shortcuts in the TUI keymap.**  
   Normalizes `cmd`/`command` aliases to `super` and supports capturing/displaying/using these bindings.

9. **[#53751](https://github.com/openai/codex/pull/53751) — Apply feature overrides when restarting the app-server daemon.**  
   Ensures `--enable`, `--disable`, and boolean `-c features.*` overrides are honored during daemon restart.

10. **[#53733](https://github.com/openai/codex/pull/53733) — Default Guardian v2 async classification to conversation mode.**  
    Makes `conversation` the default when no override/model default exists, keeping `snapshot` as explicit override.

## 5. Hot Discussions

### Show and tell
- **[#53200](https://github.com/openai/codex/discussions/53200) — Reset Alerts: SMS/Telegram notifications for Codex resets.**  
  Community-built service for tracking Codex resets without refreshing social media. **1 comment / 2 👍**.
- **[#53721](https://github.com/openai/codex/discussions/53721) — tokenreceipt: what Codex CLI sessions would cost at API list prices.**  
  Reads existing session logs and prints a receipt-style cost estimate. **1 comment / 1 👍**.
- **[#53492](https://github.com/openai/codex/discussions/53492) — Secure Browser Use for Zen: Firefox/Zen bridge for Codex browser control.**  
  Independent compatibility layer bringing signed-in Codex sidebar/browser control to Zen. **1 comment / 1 👍**.
- **[#52198](https://github.com/openai/codex/discussions/52198) — cloud-alter-ego: persistent memory for Codex and Claude Code.**  
  Learns user preferences, projects, and past sessions across agent workflows. **1 comment / 1 👍**.
- **[#53552](https://github.com/openai/codex/discussions/53552) — NOS: paste an HTML page, fix wording on screen, share as link.**  
  In-browser HTML editing workflow with rendered preview and shareable links. **1 comment / 1 👍**.
- **[#53803](https://github.com/openai/codex/discussions/53803) — swe-mux: run several Codex sessions with live status, and the same sessions on your phone.**  
  Runs real `codex` CLI sessions in real terminals and shows waiting/working/done status. **1 👍**.

### Q&A
- **[#53199](https://github.com/openai/codex/discussions/53199) — How to enable network access for only one local Codex Desktop project when custom permission profiles are missing from the picker?**  
  User wants persistent network access for one project while keeping workspace filesystem sandboxing and network restrictions elsewhere. **1 comment / 1 👍**.

**Ideas:** No Ideas-tagged discussions appeared in this window.

## 6. Feature Request Trends
- **Queue management in TUI/CLI/Desktop:** edit, remove, and clear queued follow-up inputs; make consumed prompts visible and avoid blocked queues. See [#28864](https://github.com/openai/codex/issues/28864), [#51150](https://github.com/openai/codex/issues/51150).
- **Context-aware composer suggestions:** suggest logical next prompts after a turn completes. See [#42587](https://github.com/openai/codex/issues/42587).
- **Cross-surface synchronization:** iOS remote projects, Windows Chat/Work toggle, worktree menus, and Dots computer naming should reflect a consistent source of truth. See [#36454](https://github.com/openai/codex/issues/36454), [#52811](https://github.com/openai/codex/issues/52811), [#49638](https://github.com/openai/codex/issues/49638), [#53016](https://github.com/openai/codex/issues/53016).
- **Sandbox, permission, and approval transparency:** per-project network access, clearer policy-block explanations, consistent approval behavior, and fewer stale locks. See [#53134](https://github.com/openai/codex/issues/53134), [#53821](https://github.com/openai/codex/issues/53821), [#41305](https://github.com/openai/codex/issues/41305), [#53199](https://github.com/openai/codex/discussions/53199).
- **Context/compaction reliability and observability:** fix stream disconnects, HTTP 400s, remote compaction failures, and mismatched token counters. See [#53203](https://github.com/openai/codex/issues/53203), [#52406](https://github.com/openai/codex/issues/52406), [#43648](https://github.com/openai/codex/issues/43648).
- **Browser/Computer Use/MCP robustness:** init failures, permission scope bugs, process leaks, connector unavailability, and stalled tool calls. See [#49458](https://github.com/openai/codex/issues/49458), [#53188](https://github.com/openai/codex/issues/53188), [#50789](https://github.com/openai/codex/issues/50789), [#48504](https://github.com/openai/codex/issues/48504), [#35485](https://github.com/openai/codex/issues/35485).
- **CLI/TUI polish:** Command/Super shortcuts, copy-on-select, TUI bootstrap resilience, and daemon packaging. See [#53754](https://github.com/openai/codex/pull/53754), [#53753](https://github.com/openai/codex/pull/53753), [#52775](https://github.com/openai/codex/issues/52775), [#53811](https://github.com/openai/codex/pull/53811).
- **Agent memory and session observability:** community projects around persistent memory, cost receipts, reset alerts, and multi-session dashboards. See discussions [#52198](https://github.com/openai/codex/discussions/52198), [#53721](https://github.com/openai/codex/discussions/53721), [#53200](https://github.com/openai/codex/discussions/53200), [#53803](https://github.com/openai/codex/discussions/53803).

## 7. Developer Pain Points
- **Windows Desktop remains the highest-friction surface.** Computer Use tools missing for dot-started tasks, browser/MCP initialization failures, process leaks, stream disconnects, stuck Work conversations, and embedded-browser crashes all cluster on Windows. See [#49458](https://github.com/openai/codex/issues/49458), [#53188](https://github.com/openai/codex/issues/53188), [#48504](https://github.com/openai/codex/issues/48504), [#35485](https://github.com/openai/codex/issues/35485), [#53822](https://github.com/openai/codex/issues/53822), [#53820](https://github.com/openai/codex/issues/53820).
- **Long-running sessions are fragile.** Stream disconnects, remote compaction failures, HTTP 400s, and inconsistent context counters block sustained work. See [#52919](https://github.com/openai/codex/issues/52919), [#53203](https://github.com/openai/codex/issues/53203), [#52406](https://github.com/openai/codex/issues/52406), [#43648](https://github.com/openai/codex/issues/43648).
- **Sandbox and approval semantics are confusing.** Users report “Ask for approval” being ignored, policy-blocked commands without explanation, stale `.git/index.lock` files, and missing per-project network permission profiles. See [#53134](https://github.com/openai/codex/issues/53134), [#53821](https://github.com/openai/codex/issues/53821), [#41305](https://github.com/openai/codex/issues/41305), [#53199](https://github.com/openai/codex/discussions/53199).
- **Queue management is immature.** Consumed prompts can remain invisible, queued prompts can become blocked, and there is no clear way to edit/remove/clear the queue. See [#51150](https://github.com/openai/codex/issues/51150), [#28864](https://github.com/openai/codex/issues/28864).
- **Cross-device and remote state is not authoritative.** Project lists, Chat/Work UI, worktree actions, and Dots computer names do not stay synchronized across surfaces. See [#36454](https://github.com/openai/codex/issues/36454), [#52811](https://github.com/openai/codex/issues/52811), [#49638](https://github.com/openai/codex/issues/49638), [#53016](https://github.com/openai/codex/issues/53016).
- **MCP/connectors can silently degrade.** Decode failures, unavailable app connectors, leaked `node_repl` processes, and tool calls stalling for hours make extensibility unreliable. See [#48504](https://github.com/openai/codex/issues/48504), [#35485](https://github.com/openai/codex/issues/35485), [#47910](https://github.com/openai/codex/issues/47910), [#50329](https://github.com/openai/codex/issues/50329).
- **Auth/connectivity can be environment-sensitive.** Some users see region/ISP-specific auth failures while mobile hotspots work, and Windows users report intermittent reconnect failures. See [#52738](https://github.com/openai/codex/issues/52738), [#52919](https://github.com/openai/codex/issues/52919).
- **CLI upgrades still produce edge-case regressions.** TUI bootstrap failures after upgrade, managed app-server socket issues, and model catalog/auth mismatches remain active concerns. See [#52775](https://github.com/openai/codex/issues/52775), [#50932](https://github.com/openai/codex/issues/50932), [#52839](https://github.com/openai/codex/issues/52839).

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-10-14

## 1. Today's Highlights

Release activity is steady: **v0.64.0** shipped, **v0.65.0-preview.0** landed with a security-relevant hardening for untrusted workspaces, and a new nightly rolled out an extension-loading resilience fix. Meanwhile, the issue tracker is dominated by **subagent reliability** (hangs, MAX_TURNS misreporting, browser agent settings being ignored) and **terminal rendering stability** (flicker, resize, border bleed), with a large share of today's PRs targeting UI/rendering correctness. Notably, PR #29737 proposes first-class support for a **Gemini 4 Argon** model.

## 2. Releases

- **v0.65.0-preview.0** — [repo](https://github.com/google-gemini/gemini-cli)
  - `fix(cli)`: enforce read-only workspace settings in untrusted folders ([#29583](https://github.com/google-gemini/gemini-cli/pull/29583)) — tightens the security posture when opening untrusted projects.
  - `fix(core)`: avoid duplicate tool response turns when resuming sessions — addresses a source of corrupted/replayed session history.
- **v0.65.0-nightly.20261013.g51617c1bc**
  - `fix(cli)`: keep loading other extensions when one fails the allowlist check ([#29720](https://github.com/google-gemini/gemini-cli/pull/29720)) — one bad extension no longer blocks the rest.
  - [Full changelog comparison](https://github.com/google-gemini/gemini-cli/compare/v0.65.0-nightly.20261012.g9b6e0265d...v0.65.0-nightly.20261013.g51617c1bc)
- **v0.64.0**
  - `refactor(a2a-server)`: V1 → V2 settings migration logic ([#29450](https://github.com/google-gemini/gemini-cli/pull/29450)).
  - `fix(acp)`: bridge `PromptResponse.usage` and emit `usage_update` notifications ([#29389](https://github.com/google-gemini/gemini-cli/pull/29389)).

## 3. Hot Issues

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — Subagent recovery after MAX_TURNS reported as GOAL success** (13 comments, 2 👍, p1)
   `codebase_investigator` reports `status: "success"` / `Termination Reason: "GOAL"` despite hitting the turn limit before doing any analysis. Silent truncation masquerading as success undermines trust in any agent workflow built on subagent output — hence the sustained, high-volume discussion.

2. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — Zero-dependency OS sandboxing & post-execution intent routing** (9 comments, 1 👍, p2)
   Proposes letting Gemini 3 models lean on their native bash affinity (`grep`, `cat`, `sed`, `awk`) without sacrificing safety, via OS-level sandboxing plus intent routing after execution. A central design question for how much shell freedom the agent gets.

3. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — Generalist agent hangs** (8 comments, **8 👍**, p1)
   Delegating to the generalist agent hangs indefinitely, even for trivial tasks like folder creation; disabling subagent deferral works around it. The highest upvote count in the top-30 list signals broad reproduction, not an edge case.

4. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — Assess the impact of AST-aware file reads, search, and mapping** (7 comments, p2, EPIC)
   Tracks whether AST-aware tooling can cut misaligned reads, turns, and token noise. A foundational efficiency investment that several other issues (#22746, #22747) hang off.

5. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini does not use skills and sub-agents enough** (7 comments, p2)
   Anecdotal but widely echoed: the model only invokes custom skills/subagents when explicitly instructed, even when clearly relevant. This is the flip side of the delegation-hang problem — routing reliability in both directions.

6. **[#22267](https://github.com/google-gemini/gemini-cli/issues/22267) — Browser Agent ignores `settings.json` overrides (e.g. `maxTurns`)** (4 comments, p2)
   `AgentRegistry` merges settings correctly, but the Browser Agent bypasses them. Configuration that silently doesn't apply is a classic debugging trap for teams.

7. **[#22232](https://github.com/google-gemini/gemini-cli/issues/22232) — Browser agent resilience: automatic session takeover and lock recovery** (4 comments, p3)
   Requests replacing the current fail-fast behavior on locked browser profiles (persistent session mode with an orphaned process) with recovery logic.

8. **[#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — Browser subagent fails on Wayland** (4 comments, 1 👍, p1, `agent/browser`)
   Reports a "Browser Agent Finished" with `Termination Reason: GOAL` while failing on Wayland — Linux desktop users remain a second-class path for the browser agent.

9. **[#21000](https://github.com/google-gemini/gemini-cli/issues/21000) — Experiment with native file tools for the task tracker** (4 comments, p3)
   Explores moving the todo/task tracker onto native file tools, related to the persistent file-based tracking proposal in #18836.

10. **[#20079](https://github.com/google-gemini/gemini-cli/issues/20079) — `~/.gemini/agents/filename.md` symlinks not recognized as agents** (4 comments, p2, need-info)
    Blocks dotfile-manager and stow-style workflows where agent definitions are symlinked rather than real files.

*Also active:* [#24246](https://github.com/google-gemini/gemini-cli/issues/24246) (400 error with >128 tools), [#23571](https://github.com/google-gemini/gemini-cli/issues/23571) (model scatters temp scripts), [#22672](https://github.com/google-gemini/gemini-cli/issues/22672) (agent should avoid destructive `git reset --force`), [#22465](https://github.com/google-gemini/gemini-cli/issues/22465) (stuck at interactive `vite` prompt), [#21763](https://github.com/google-gemini/gemini-cli/issues/21763) (bug reports omit subagent context).

## 4. Key PR Progress

1. **[#29725](https://github.com/google-gemini/gemini-cli/pull/29725) — `fix(ui)`: synchronized output + isolated spinner rendering** (p2, size/xl)
   Wraps interactive UI frame writes in terminal synchronized-output sequences to eliminate flicker in multiplexers like `tmux`. Likely the single most user-visible rendering fix in flight.

2. **[#29728](https://github.com/google-gemini/gemini-cli/pull/29728) — `fix(cli)`: prevent session poisoning on rapid Ctrl-C interruption** (CLOSED, p1)
   Awaits `cancelOngoingRequest` instead of firing it detached, closing a race where a double `Ctrl+C` poisoned the session before `/quit` ran. High-severity UX fix.

3. **[#29737](https://github.com/google-gemini/gemini-cli/pull/29737) — `feat(models)`: add Gemini 4 Argon model support**
   Registers `gemini-4-argon` in `VALID_GEMINI_MODELS` with a chat config extending `chat-base-3`, so it is selectable without a custom override. If accepted, this is the headline model-availability change of the day.

4. **[#29736](https://github.com/google-gemini/gemini-cli/pull/29736) — `refactor(core,cli)`: emit tool call groups from the scheduler, drop UI lookahead**
   Moves tool-call grouping out of `useGeminiStream` and into the scheduler, removing the fragile lookahead that held back finished subagent calls. Directly relevant to the subagent-turn reporting bugs.

5. **[#29742](https://github.com/google-gemini/gemini-cli/pull/29742) — `fix(cli)`: prevent right-hand border bleed in scrollable tool containers** (p2)
   Fixes intermittent broken borders in expanded `WriteFile`/`Edit` views and alternate-buffer tool results when a scrollbar is present.

6. **[#29695](https://github.com/google-gemini/gemini-cli/pull/29695) — `fix(ui)`: debug console height + `terminalBuffer` flickering** (p2)
   Corrects `F12` layout math and enables incremental rendering with static history memoization and safe cache invalidation.

7. **[#29718](https://github.com/google-gemini/gemini-cli/pull/29718) — `fix(cli)`: `--userns=keep-id` for rootless Podman sandboxes** (p2)
   Resolves `EACCES` failures for rootless Podman users by correctly mapping the caller's UID into the container namespace.

8. **[#29536](https://github.com/google-gemini/gemini-cli/pull/29536) — `fix(grep)`: prevent command-line option injection via explicit `-e`** (CLOSED, p2, security)
   Hardens `packages/core/src/tools/grep.ts` against CWE-88 by enforcing strict argument separation for both `git grep` and system `grep`.

9. **[#29678](https://github.com/google-gemini/gemini-cli/pull/29678) — `fix(cli)`: load env vars before resolving settings placeholders** (CLOSED, p2)
   Fixes a load-order race in `_doLoadSettings`/`loadEnvironment` where `${VAR}` placeholders were expanded before `.env` files populated `process.env`.

10. **[#29703](https://github.com/google-gemini/gemini-cli/pull/29703) — `fix(core)`: keep atomic-write temp file name within `NAME_MAX`**
    Long destination filenames (215–255 bytes) failed with `ENAMETOOLONG` because the `.<uuid>.tmp` suffix pushed past the 255-byte limit.

*Also worth watching:* [#29722](https://github.com/google-gemini/gemini-cli/pull/29722) (whitespace-only UTF-16/32 files treated as empty), [#29735](https://github.com/google-gemini/gemini-cli/pull/29735) (extension `downloadFile` stream error handling), [#29563](https://github.com/google-gemini/gemini-cli/pull/29563) (preserve line terminators when truncating), [#29573](https://github.com/google-gemini/gemini-cli/pull/29573) (registry ports in sandbox image names), [#29644](https://github.com/google-gemini/gemini-cli/pull/29644) (restore debounced static refresh on terminal width change), [#29740](https://github.com/google-gemini/gemini-cli/pull/29740) (bump to `0.66.0-nightly`), [#29741](https://github.com/google-gemini/gemini-cli/pull/29741) (v0.64.0 changelog).

## 5. Hot Discussions

No discussion data was provided for this window; this section is omitted.

## 6. Feature Request Trends

- **Subagent observability and correctness.** A distinct cluster of issues asks for accurate termination reporting (#22323), visible subagent trajectories via `/chat share` (#22598), and subagent context included in `/bug` reports (#21763). Users want to audit what subagents actually did, not just what they claimed.
- **Smarter delegation and self-knowledge.** Complementing the reliability bugs, users want the model to *choose* skills/subagents proactively (#21968) and to accurately describe its own CLI flags, hotkeys, and invocation patterns (#21432).
- **Token-frugal, structurally-aware context access.** AST-aware reads/search/mapping (#22745, #22746, #22747) plus "Tactful Extraction" surgical reads (#19561) point to a clear demand for reducing the ~36.6k-token-per-turn baseline and avoiding large-file firehosing.
- **Persistent, file-backed task tracking.** #18836 and #21000 both push away from in-context `WriteToDo` toward CRUD-style persistent task state to combat context rot and cross-session memory loss.
- **Sandboxing and least-privilege defaults.** Zero-dependency OS sandboxing (#19873), workspace-scoped policies (#18397), read-only untrusted folders, and destructive-command avoidance (#22672) form a coherent security-direction theme.
- **Parallel/shared-memory multi-agent collaboration.** #18287 (blocked on parallel subagent support) and #18285 (subagent discovery via `settings.json`) mark the next frontier after single-subagent stability.
- **Terminal rendering robustness.** Resize without flicker (#21924), tmux synchronization, and border integrity are recurring quality-of-life asks.

## 7. Developer Pain Points

- **Hangs and stalls with no useful diagnostics.** The generalist agent hanging indefinitely (#21409) and getting stuck at interactive prompts like `vite` (#22465) both surface as "the tool just stopped," with no timeout or explanatory error.
- **Silent misconfiguration.** Settings that load but never apply — Browser Agent ignoring `maxTurns` (#22267), `settings.json` values, `.env` placeholders resolved too early (#29678) — force developers into source-level debugging of their own config.
- **Terminal rendering instability.** Flicker in `tmux`, layout breakage on resize (#21924, #29695), and border bleed in scrollable containers (#29742) degrade the core TUI experience, especially for multiplexer users.
- **Session state corruption.** Duplicate tool response turns on resume (v0.65.0-preview.0), rapid `Ctrl+C` poisoning sessions (#29728), and escaping bugs around `\n` (#22466) erode confidence in long-running sessions.
- **Sandbox and permission friction on Linux/containers.** Rootless Podman `EACCES` (#29718) and image-name parsing with registry ports (#29573) show the container path still needs polish.
- **Tool-count and workspace hygiene limits.** A 400 error above ~128 tools (#24246) and the model scattering temp scripts across directories (#23571) create cleanup overhead and hard ceilings on large setups.
- **Agent filesystem conventions are brittle.** Symlinked agent definitions not being recognized (#20079) breaks rational dotfile management for users with many agents.

---
*Data source: [github.com/google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli) · Compiled 2026-10-14*

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-10-14

## 1. Today's Highlights
Copilot CLI shipped **v1.0.98-1** with **Gemini 4 Argon** support and fixes for MCP/session synchronization, resumed permission handling, and terminal input preservation. The latest release wave also includes v1.0.97-3/v1.0.97 fixes for plugin-auth retries, TLS-intercepting proxies, and GitHub SCP remote parsing. Community attention remains concentrated on **BYOK/custom provider model handling**, **session lifecycle consistency**, and **Windows/MCP authentication reliability**.

## 2. Releases
- **v1.0.98-1** — Added Gemini 4 Argon support. Fixed MCP status and `/mcp` staying synced to the active session after split-view switches; resuming a session now respects abandoned pending-permission work and continues only active permission requests; ordinary typed characters are preserved.  
  https://github.com/github/copilot-cli/releases/tag/v1.0.98-1
- **v1.0.98-0** — Fixes and changes; no detailed notes in provided data.  
  https://github.com/github/copilot-cli/releases/tag/v1.0.98-0
- **v1.0.97** — Added agent-scoped declarative hooks for custom agents; remote plugin marketplace/plugin fetches retry once with signed-in GitHub token after HTTPS auth failure; streamed responses behind TLS-intercepting proxies complete when `close_notify` is omitted; fixed GitHub SCP parsing.  
  https://github.com/github/copilot-cli/releases/tag/v1.0.97
- **v1.0.97-3** — Fixed remote plugin marketplace auth retry, TLS proxy streaming completion, and SCP remotes with absolute paths so repository owner/name parse correctly.  
  https://github.com/github/copilot-cli/releases/tag/v1.0.97-3

## 3. Hot Issues
- **#4082 [OPEN] Cross-app session sync between Copilot CLI and Copilot Desktop App** — 3 comments / 13 👍. High-demand workflow gap: sessions started in CLI are invisible in desktop and vice versa.  
  https://github.com/github/copilot-cli/issues/4082
- **#1724 [OPEN] Show the current TODO status** — 2 comments / 12 👍. Users want agent TODO state surfaced in a sidebar, similar to other coding agents.  
  https://github.com/github/copilot-cli/issues/1724
- **#4358 [OPEN] BYOK: populate the `/model` picker from the provider's `/models` endpoint** — 2 comments / 9 👍. Current BYOK flow exposes only one configured model, forcing restarts to switch models.  
  https://github.com/github/copilot-cli/issues/4358
- **#5115 [OPEN] ACP: `/rename` reports success but doesn't rename the session** — 5 comments. Breaks ACP/non-interactive automation expectations despite advertising the command.  
  https://github.com/github/copilot-cli/issues/5115
- **#4950 [OPEN] BYOK custom providers: CLI forces greedy sampling (`temperature=0`), causing reasoning-model degeneration and silent hangs** — 3 comments. Serious correctness issue for vLLM/OpenAI-compatible custom providers since 1.0.81.  
  https://github.com/github/copilot-cli/issues/4950
- **#5126 [OPEN] BYOK: built-in agents never use their default models and silently run on the session model** — 1 comment. Affects `explore`, `task`, and `research` agent behavior under BYOK.  
  https://github.com/github/copilot-cli/issues/5126
- **#5127 [OPEN] SDK with BYOK providers: short model names aren't matched to provider models** — 1 comment. Custom agents fail, and short session models can produce a misleading “No GitHub OAuth token” error.  
  https://github.com/github/copilot-cli/issues/5127
- **#5134 [OPEN] Windows: runaway MCP OAuth initialization opens repeated browser sessions; Maximum update depth exceeded** — 0 comments. New but critical: prevents normal startup/authentication on Windows.  
  https://github.com/github/copilot-cli/issues/5134
- **#5122 [OPEN] `userPromptSubmitted`: only the last plugin's `additionalContext` reaches the model** — 0 comments. Silent data loss in multi-plugin hook workflows; other hooks run but their context is dropped.  
  https://github.com/github/copilot-cli/issues/5122
- **#5130 [OPEN] Constant error message around “Managed account policy”** — 0 comments / 1 👍. Enterprise/policy friction reported since at least 1.0.94.  
  https://github.com/github/copilot-cli/issues/5130

## 4. Key PR Progress
No pull requests were updated in the last 24 hours in the provided dataset. No PR progress to summarize.  
https://github.com/github/copilot-cli/pulls

## 5. Feature Request Trends
- **BYOK/custom provider maturity**: `/model` picker from provider `/models`, short-name matching, per-agent default models, credential commands in `providers.json`, and control over sampling parameters.  
  https://github.com/github/copilot-cli/issues/4358 · https://github.com/github/copilot-cli/issues/5127 · https://github.com/github/copilot-cli/issues/5126 · https://github.com/github/copilot-cli/issues/4936 · https://github.com/github/copilot-cli/issues/4950
- **Session lifecycle and naming**: cross-app session sync, rename from Sessions view/sidebar, reliable ACP rename, persistent session names, and predictable `/exit` behavior after nested session switches.  
  https://github.com/github/copilot-cli/issues/4082 · https://github.com/github/copilot-cli/issues/5128 · https://github.com/github/copilot-cli/issues/5115 · https://github.com/github/copilot-cli/issues/5132 · https://github.com/github/copilot-cli/issues/5135
- **Agent observability and control**: surfacing TODO status, prompt/response timing, and preventing `--agent` selection from being dropped after startup.  
  https://github.com/github/copilot-cli/issues/1724 · https://github.com/github/copilot-cli/issues/5125 · https://github.com/github/copilot-cli/issues/5124
- **Permissions and interruption UX**: file-scoped write permissions, permission-system epic, waiting while the user types, and avoiding terminal-safety dead ends during recovery.  
  https://github.com/github/copilot-cli/issues/316 · https://github.com/github/copilot-cli/issues/285 · https://github.com/github/copilot-cli/issues/5131 · https://github.com/github/copilot-cli/issues/5123
- **Enterprise/platform readiness**: system-wide Windows installer, managed account policy error reduction, and stable MCP OAuth initialization.  
  https://github.com/github/copilot-cli/issues/5129 · https://github.com/github/copilot-cli/issues/5130 · https://github.com/github/copilot-cli/issues/5134
- **Plugin/MCP robustness**: merging plugin `additionalContext`, MCP status sync, and cloud-agent Playwright MCP recovery.  
  https://github.com/github/copilot-cli/issues/5122 · https://github.com/github/copilot-cli/issues/4931

## 6. Developer Pain Points
- **BYOK providers remain fragile**: model discovery, short-name resolution, default agent models, sampling defaults, and authentication errors are recurring blockers.
- **Session state is inconsistent across surfaces**: ACP rename, sidebar naming, split-view MCP status, nested `/exit`, and CLI/Desktop sync create confusion.
- **Windows and enterprise auth/policy friction**: runaway MCP OAuth browser loops, persistent notification badges, user-profile installs, and “Managed account policy” errors are high-friction.
- **Permission prompts are overbearing**: users report interruptions while typing, overly broad write access, and terminal-safety denials that block session recovery.
- **Plugin/MCP hook reliability needs work**: only the last plugin’s context is honored in one hook path, and MCP tool-selection loops still appear in older reports.
- **Edge-case networking/remote parsing has been actively fixed**: TLS proxy `close_notify`, SCP-style GitHub remotes, and plugin-auth retries indicate ongoing hardening.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-10-14

## 1. Today's Highlights

No releases shipped in the last 24 hours; the day's activity is dominated by V2 stabilization work — session environment inheritance, reconnect resilience, and managed-config precedence all landed as PRs against freshly filed reproductions. Meanwhile, the highest-traffic thread of the period remains the free-model quota question (#15585, 56 comments), and V2 feature-parity gaps (todo tools, timestamp toggle, non-git session visibility) continue to drive the backlog.

## 2. Releases

None in the last 24 hours.

## 3. Hot Issues

1. **[#15585 [CLOSED] "free usage exceed" on all free models](https://github.com/anomalyco/opencode/issues/15585)** — 56 comments, 17 👍. The single most-discussed item in the window: users on 1.2.15 hit identical quota errors across all three free models, raising the question of whether "free" tiers carry undisclosed limits. The volume of comments (and the high 👍 count) suggests the messaging around free-model quotas is a trust issue, not just a bug.

2. **[#42421 [OPEN] [2.0] Add todo support for v2](https://github.com/anomalyco/opencode/issues/42421)** — 27 comments, 5 👍. In V1 the model read/wrote its native TODO list via `todowrite`/`todoread`; in V2 the tool catalog no longer exposes them. High-comment count reflects how central the TODO surface is to agent workflow steering.

3. **[#34644 [OPEN] GitHub Copilot provider not registered (Student plan, auto-only)](https://github.com/anomalyco/opencode/issues/34644)** — 9 comments, **22 👍**. OAuth completes but `github-copilot` never appears in the model selector. The highest-reaction open issue here, indicating a sizable blocked cohort of student subscribers.

4. **[#30158 [CLOSED] Terminal button in web UI disappears since v1.15.12](https://github.com/anomalyco/opencode/issues/30158)** — 17 comments, 14 👍. Regression tracked with clear bisection (1.15.11 works, 1.15.12 doesn't), now closed. Good example of a small UI regression causing disproportionate disruption.

5. **[#51949 [OPEN] After auto-compact, agent stops using top-level tools](https://github.com/anomalyco/opencode/issues/51949)** — 8 comments. Post-compaction, the agent routes everything through `execute → tools.shell`, reports its tools as unregistered, and fails. Potentially severe for long sessions where compaction is inevitable.

6. **[#49827 [OPEN] migrate-v1: non-git sessions hidden after V1→V2 upgrade](https://github.com/anomalyco/opencode/issues/49827)** — 7 comments. Sessions in V1's `global` project vanish from the TUI picker because V2 creates per-directory projects and filters to the current one. Data is intact, which makes the migration optics worse.

7. **[#51109 [OPEN] Configurable prompt-cache TTL per agent and provider/model](https://github.com/anomalyco/opencode/issues/51109)** — 7 comments. With caching on by default but TTL pinned to Anthropic's 5-minute ephemeral window, orchestrators waiting on subagents pay repeated cache misses. A concrete, cost-driven request for `opencode.jsonc` configuration.

8. **[#55189 [OPEN] Desktop terminal fails on Windows: `opencode-pty` not in $PATH](https://github.com/anomalyco/opencode/issues/55189)** — 5 comments. Toggling the terminal yields "Desktop IPC handler failed" with a server-side `503 Executable not found in $PATH`. Filed and reproduced same-day; Windows desktop remains the roughest platform.

9. **[#55228 [OPEN] [reproduced] `session.environment` not applied to subagent sessions](https://github.com/anomalyco/opencode/issues/55228)** — 4 comments. `PUT /api/session/{id}/environment` works for the parent shell but child sessions inherit the service process environment instead. Environment leakage across the agent tree is both a correctness and a security concern.

10. **[#50672 [OPEN] Hardcoded default port 49374 breaks second client](https://github.com/anomalyco/opencode/issues/50672)** — 6 comments. The managed service binds a machine-global port while registration is per-user, so a second client on shared loopback (multi-user machines, WSL mirrored networking) fails with only a generic timeout.

## 4. Key PR Progress

1. **[#55257 fix(core): inherit the parent's session environment in child sessions](https://github.com/anomalyco/opencode/pull/55257)** — Closes #55228; `Session.create` already inherits location, metadata, and permissions, so environment was an oversight rather than a design choice. Fast turnaround from reproduction to patch.

2. **[#55258 fix(tui): tolerate transient failures in the post-reconnect location sync](https://github.com/anomalyco/opencode/pull/55258)** — Closes #55250, the false "Session location unavailable" prompt after network reconnect. Treats the sync as best-effort while the server/proxy settles.

3. **[#55149 fix(tui): keep long-pending requests past Bun's fetch deadline](https://github.com/anomalyco/opencode/pull/55149)** — Closes #54974. The TUI's plain `OpenCode.make(...)` client inherits Bun's 5-minute fetch deadline, killing legitimately long requests.

4. **[#54930 feat(plugin): let plugins assert permissions](https://github.com/anomalyco/opencode/pull/54930)** — Adds an in-process permission assertion path for plugins, filling a real gap: previously only `list`/`get`/`reply` plus the `evaluate` hook existed, and the HTTP route doesn't await a reply.

5. **[#54023 fix(core): coordinate credential refreshes across locations](https://github.com/anomalyco/opencode/pull/54023)** — Serializes refreshes through `credentials.lock(id, operation)` so separate Locations share one refresh and observe the same outcome instead of racing.

6. **[#55261 feat(tui): add prompt history scope setting](https://github.com/anomalyco/opencode/pull/55261)** — Addresses #52154 with an opt-in `prompt.history_scope`; `global` preserves today's cross-session recall, `session` scopes Up/Down to the current conversation.

7. **[#55267 fix(core): give managed configuration highest precedence](https://github.com/anomalyco/opencode/pull/55267)** — Loads the system config directory and macOS managed preferences last so admin-enforced settings override global and project config. Complements the closed discovery fix in #54921 (#51107 / #53074).

8. **[#55262 feat(core): show only Bedrock models the region serves](https://github.com/anomalyco/opencode/pull/55262)** — Region-scoped model listing via a parallel `foundation-models` probe with a 10s timeout, reducing noise and failed selections for Bedrock users.

9. **[#48431](https://github.com/anomalyco/opencode/pull/48431) + [#48432](https://github.com/anomalyco/opencode/pull/48432) fix(tui): coalesce delta writes / grow markdown live tail in place** — Both closed; jointly remove the O(n²) cost from the client-side streaming path (closes #36043). Independent layers, mergeable in either order — the most consequential performance work in this window.

10. **[#55274 add SerpApi websearch to opencode](https://github.com/anomalyco/opencode/pull/55274)** — V2 port of the V1 web-search plugin (supersedes #52480 lineage). Carries `needs:title`/`needs:compliance` labels, so expect review churn before merge.

*Also worth noting:* [#55270 fix(ci): push daily stats with the OpenCode app token](https://github.com/anomalyco/opencode/pull/55270) (STATS.md stale since 2026-08-27) and [#55269 (closed) fix(plugin): resolve the figma plugin's peer from the workspace](https://github.com/anomalyco/opencode/pull/55269), which unblocked `publish` on `dev` after repeated failures.

## 5. Hot Discussions

No discussion data was provided for this window.

## 6. Feature Request Trends

- **V2 feature parity with V1 is the dominant theme.** Native TODO tools (#42421), per-message timestamps (#49742), prompt-history scoping (#52154 → #55261), managed config directories (#51107 → #55267), and non-git/global session visibility (#49827) are all "V1 had it, V2 lost it" requests.
- **Cost and latency governance.** Configurable prompt-cache TTL per agent and provider/model (#51109) and configurable retry/backoff policy instead of fixed exponential backoff (#41709).
- **Deeper plugin surface area.** In-process permission assertions (#54930), intercepting outbound requests on the AI-SDK path (#42846), and per-call error fidelity in parallel hook batches (#50331).
- **Provider breadth and correctness.** GitHub Copilot Student auto-only support (#34644), OpenAI-compatible models via google-vertex (#49741), region-accurate Bedrock catalogs (#55262), and third-party web search (#55274).
- **MCP as a first-class integration path.** Remote Figma MCP support (#54805) and robust loopback OAuth callback handling (#55201).
- **Long-lived session management.** A session-scoped process registry for long-running work (#53614), environment propagation to subagents (#55228), and per-session message history isolation (#52154).

## 7. Developer Pain Points

- **Quota and entitlement ambiguity.** The 56-comment free-model thread (#15585) and the unregistered Copilot provider (#34644) both point to unclear or silently unsupported subscription states.
- **V2 migration friction.** Missing tools, hidden history, dropped managed config, and absent UI affordances make upgrades feel like data loss even when nothing is actually lost.
- **Streaming and rendering performance.** The O(n²) stream path (#36043, #48431/#48432) and per-delta markdown re-parsing (#54428) remain the top performance complaints for long sessions.
- **File-watcher and reload instability.** Any write inside the global config directory triggers an un-debounced compatibility reload — with `~/.claude/skills` resubscribing in a loop (#53393).
- **Windows is disproportionately broken.** `shell: pwsh` ignored in favor of PowerShell 5.1 (#41426) and the desktop terminal's missing `opencode-pty` (#55189).
- **Networking fragility.** Sessions that fail to resume after websocket interruption (#35029), false "location unavailable" prompts on reconnect (#55250), and the hardcoded service port colliding across users (#50672).
- **Context compaction degrading agent behavior.** Post-compact tool amnesia (#51949), sessions wedged by empty assistant messages (#53571), and queued messages re-injected once per agent step (#55204) all indicate the turn/prompt assembly layer needs hardening.

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi Community Digest — 2026-10-14

## Today's Highlights
No new releases were published in the last 24h. Across 50 updated issues and 14 updated PRs, activity is concentrated on provider/model routing correctness, OAuth identity persistence, session/tool-state restoration, and model API drift such as Gemini 4 thinking controls and GPT-6 reasoning-effort caching. PR work is landing fixes for those reliability paths, alongside Web UI, Nix packaging, and export improvements.

## Hot Issues (10)
- [Issue #10314](https://github.com/earendil-works/pi/issues/10314) — **Reconsider Home/End defaults in fullscreen mode?** `[CLOSED]`  
  A high-engagement UX debate: should Home/End keep classic line-editing behavior or use new fullscreen TUI scrolling? 25 comments and 5 👍 indicate strong community interest in terminal interaction defaults.

- [Issue #4180](https://github.com/earendil-works/pi/issues/4180) — **Links not clickable anymore** `[CLOSED]`  
  A regression after alternate-term-mode changes broke clickable hyperlinks. 19 comments show this was a persistent annoyance for users following web sources from the agent.

- [Issue #10300](https://github.com/earendil-works/pi/issues/10300) — **ChatGPT OAuth ID token is not persisted, preventing extensions from accessing account identity** `[OPEN]`  
  The OAuth credential conversion drops the ID token, so extensions cannot reliably access ChatGPT account identity. 15 comments and security/identity implications make this a priority issue.

- [Issue #5916](https://github.com/earendil-works/pi/issues/5916) — **Support provider extensions with model aliases and improve search** `[OPEN]` `[bug, inprogress]`  
  Users configuring OpenRouter/custom providers via `models.json` need model aliases and better discovery. 14 comments reflect ongoing demand for more flexible provider configuration.

- [Issue #9075](https://github.com/earendil-works/pi/issues/9075) — **Compaction summarisation inherits session thinking level on adaptive models and hits output cap** `[OPEN]`  
  Adaptive-thinking Anthropic models can deterministically hit the token cap during compaction summarisation. 10 comments and 4 👍 highlight a serious reliability issue for long sessions.

- [Issue #10853](https://github.com/earendil-works/pi/issues/10853) — **Cache-preserving reasoning effort changes for GPT-6** `[OPEN]` `[prio:p0]`  
  Mid-session thinking-level changes rewrite the request prefix and miss cache; on Codex they also drop WebSocket continuation. 5 comments, p0 priority.

- [Issue #10299](https://github.com/earendil-works/pi/issues/10299) — **openai-completions: empty tool call from OpenRouter/GLM permanently wedges the session** `[OPEN]` `[bug]`  
  A malformed empty `tool_calls` entry gets saved and replayed, permanently breaking later turns. The reporter includes repro and a fix for #4854, making this highly actionable.

- [Issue #10785](https://github.com/earendil-works/pi/issues/10785) — **Resuming a session drops dynamically activated tools** `[CLOSED]` `[prio:p0]`  
  Optional/deferred tools activated during a session are lost after resume, and the next turn records their removal. A critical session-state bug with 3 comments.

- [Issue #10797](https://github.com/earendil-works/pi/issues/10797) — **macOS: bash timeout leaves Playwright Chrome running in a detached process group** `[OPEN]`  
  Interrupted foreground tasks can leak Chrome processes on macOS. 3 comments; important for users running browser automation through Pi.

- [Issue #10868](https://github.com/earendil-works/pi/issues/10868) — **pi-ai: Invalid `\u` escapes cause tool-call arguments to lose fields** `[CLOSED]` `[bug, prio:p0]`  
  Invalid JSON escapes such as `\url` can cause silent field loss in streaming tool-call parsing. 2 comments, but p0 severity because it corrupts tool arguments.

## Key PR Progress (10)
- [PR #10913](https://github.com/earendil-works/pi/pull/10913) — **Resolve model references through one provider selection policy** `[OPEN]`  
  Fixes #10160: `--model provider/id` should respect credentials instead of selecting the first gateway in catalog order.

- [PR #10910](https://github.com/earendil-works/pi/pull/10910) — **Default NVIDIA to Nemotron 3.5 Super VL** `[OPEN]`  
  Switches the NVIDIA default model to `nvidia/nemotron-3.5-super-vl-120b-a12b`. Draft until the model is public; supersedes #10906.

- [PR #10528](https://github.com/earendil-works/pi/pull/10528) — **Refactor Nix part** `[OPEN]`  
  Reworks Nix packaging: cleans `package.nix`, drops maps/type files, uses `makeBinaryWrapper`, and skips version checks.

- [PR #10902](https://github.com/earendil-works/pi/pull/10902) — **Use thinking levels for Gemini 4 models** `[CLOSED]`  
  Fixes Gemini 4 requests that incorrectly send `thinkingBudget`; Vertex rejects budget-only controls with a 400. Related duplicate: [PR #10899](https://github.com/earendil-works/pi/pull/10899).

- [PR #10896](https://github.com/earendil-works/pi/pull/10896) — **Added Web UI** `[CLOSED]`  
  A notable feature PR introducing a Web UI, though the provided summary is empty. Worth tracking for future Pi usability work.

- [PR #10521](https://github.com/earendil-works/pi/pull/10521) — **Inline `$ref` tool schemas for NVIDIA NIM models** `[OPEN]`  
  Fixes #10270: NIM models can return object fields as JSON strings when schemas use local `$ref`, causing `validateToolArguments` failures.

- [PR #10795](https://github.com/earendil-works/pi/pull/10795) — **Drop images from Anthropic error tool results** `[CLOSED]`  
  Fixes #10794 by preventing image content from being sent in Anthropic tool error results, avoiding API failures.

- [PR #10887](https://github.com/earendil-works/pi/pull/10887) — **Restore active tools on session resume** `[CLOSED]`  
  Fixes #10785: resumed sessions now restore the recorded tool loadout, with `--tools +name/-name` applied on top.

- [PR #10886](https://github.com/earendil-works/pi/pull/10886) — **Send GPT-6 effort changes as `configuration_update` items** `[CLOSED]`  
  Closes #10853: keeps the first effort at request level and sends later changes as configuration updates, preserving cache and Codex WebSocket continuation.

- [PR #10718](https://github.com/earendil-works/pi/pull/10718) — **Include system prompt in `--export` HTML** `[OPEN]`  
  Makes CLI HTML export match interactive `/export` by including the recorded system prompt from the current branch.

## Feature Request Trends
No Discussion data was provided, so trends below are distilled from Issues and PRs.

- **Provider/model routing and configuration**  
  Users want clearer provider selection, model aliases, credential-aware routing, and system-level config precedence. Examples: [#10913](https://github.com/earendil-works/pi/pull/10913), [#5916](https://github.com/earendil-works/pi/issues/5916), [#10895](https://github.com/earendil-works/pi/issues/10895), [#10889](https://github.com/earendil-works/pi/issues/10889).

- **Session and tool-state durability**  
  Restoring active tools, preserving compaction baselines, handling summarisation token caps, and fixing prompt-template argument shifts are recurring themes. Examples: [#10785](https://github.com/earendil-works/pi/issues/10785), [#10904](https://github.com/earendil-works/pi/issues/10904), [#9075](https://github.com/earendil-works/pi/issues/9075), [#10867](https://github.com/earendil-works/pi/issues/10867).

- **Terminal/TUI interaction polish**  
  Home/End behavior, clickable links, hardware cursor positioning, tmux inline images, keyboard input recovery, and rxvt-unicode wheel handling remain active UX concerns. Examples: [#10314](https://github.com/earendil-works/pi/issues/10314), [#4180](https://github.com/earendil-works/pi/issues/4180), [#10878](https://github.com/earendil-works/pi/issues/10878), [#10872](https://github.com/earendil-works/pi/pull/10872).

- **Security, identity, and trust**  
  OAuth token persistence, MCP client identification, system-level configuration precedence, and side-effect-free SDK runtime registration are important for embedders and enterprise trust. Examples: [#10300](https://github.com/earendil-works/pi/issues/10300), [#10883](https://github.com/earendil-works/pi/issues/10883), [#10458](https://github.com/earendil-works/pi/issues/10458).

- **Packaging and ecosystem compatibility**  
  Nix builds, pinned provider SDK refreshes, Bun/Node compatibility, proper-lockfile maintenance, and Web UI/export tooling are recurring ecosystem requests. Examples: [#10881](https://github.com/earendil-works/pi/issues/10881), [#10909](https://github.com/earendil-works/pi/issues/10909), [#10912](https://github.com/earendil-works/pi/issues/10912), [#10896](https://github.com/earendil-works/pi/pull/10896).

## Developer Pain Points
- **Silent state corruption after resume/compaction** — Dynamically activated tools disappear, compaction baselines drift, and summarisation can hit token caps: [#10785](https://github.com/earendil-works/pi/issues/10785), [#10904](https://github.com/earendil-works/pi/issues/10904), [#9075](https://github.com/earendil-works/pi/issues/9075), [#10908](https://github.com/earendil-works/pi/issues/10908).

- **Model API drift** — Gemini 4 rejects `thinkingBudget`, GPT-6 reasoning-effort changes break cache continuity, and malformed tool calls from OpenRouter/GLM wedge sessions: [#10898](https://github.com/earendil-works/pi/issues/10898), [#10853](https://github.com/earendil-works/pi/issues/10853), [#10299](https://github.com/earendil-works/pi/issues/10299), [#10868](https://github.com/earendil-works/pi/issues/10868).

- **OAuth and auth friction** — ChatGPT ID tokens are not persisted, and Copilot device polling can fail indefinitely under WSL clock skew: [#10300](https://github.com/earendil-works/pi/issues/10300), [#10694](https://github.com/earendil-works/pi/pull/10694).

- **Provider/config surprises** — Missing providers, aliases/search gaps, and credential-unaware model routing create confusion: [#10889](https://github.com/earendil-works/pi/issues/10889), [#5916](https://github.com/earendil-works/pi/issues/5916), [#10913](https://github.com/earendil-works/pi/pull/10913).

- **Terminal/TUI edge cases** — Home/End defaults, hyperlinks, cursor jumps, tmux images, stdin buffering, and rxvt-unicode wheel behavior continue to generate reports: [#10314](https://github.com/earendil-works/pi/issues/10314), [#4180](https://github.com/earendil-works/pi/issues/4180), [#10862](https://github.com/earendil-works/pi/issues/10862), [#10884](https://github.com/earendil-works/pi/issues/10884).

- **Build and compatibility regressions** — Nix v1.2.0 build failures, SDK/TypeBox pinning, Pi Codemode compatibility, and extension cache clearing affect downstream hosts: [#10881](https://github.com/earendil-works/pi/issues/10881), [#10909](https://github.com/earendil-works/pi/issues/10909), [#10865](https://github.com/earendil-works/pi/issues/10865), [#10911](https://github.com/earendil-works/pi/issues/10911).

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-10-14

## 1. Today's Highlights

Release engineering continues on the `managed-agent` line: both a preview (`v0.25.2-preview.0`) and a nightly shipped today, headlined by connector/broker robustness fixes from the #12692 R2 review plus the start of the H4e-b2 work. Issue traffic is dominated by **Hosted Managed Agent durability and recovery gaps** — P1 bugs where wake inputs, child results, and session takeovers are lost or 409-refused across harness restarts. A second cluster of reports focuses on **provider compatibility**: strict OpenAI-compatible backends (vLLM/SGLang) fail wholesale on tool serialization or context-overflow detection, and the resulting CLI errors are undiagnosable.

## 2. Releases

Two builds landed in the last 24 hours:

- **v0.25.2-preview.0** — https://github.com/QwenLM/qwen-code/releases
  - `fix(managed-agent)`: connector and broker robustness follow-ups from the #12692 R2 review (#13330, @wenshao)
  - `feat(managed-agent)`: H4e-b2 (release notes truncated in the source data)
- **v0.25.1-nightly.20261013.6212189f1f** — same connector/broker fix plus the H4e-b2 feature slice

Both are effectively the same content line; the preview is the stabilization cut, the nightly is the CI-verified snapshot of commit `6212189f1f`.

## 3. Hot Issues

1. **#14116 — P1: wake input committed before harness death is never run until something unrelated loads the Session** (https://github.com/QwenLM/qwen-code/issues/14116)
   Monitor notifications, channel messages, child results, automation fires, and session messages are all committed durably but only executed while a Session is loaded on a Hosted Harness. This is the root-cause issue behind several crash-recovery bugs below.

2. **#14114 — P1: ACP sessions never announce MCP tools discovered after session start** (https://github.com/QwenLM/qwen-code/issues/14114)
   `qwen --acp` registers MCP servers from `session/new` but never drains the deferred-tool reminder, so the model can't see or use the tools. Headless mode works — a sharp, reproducible integration regression.

3. **#14112 — vLLM-backed OpenAI-compatible providers 400 on every request** (https://github.com/QwenLM/qwen-code/issues/14112)
   Parameterless tools are serialized without a `parameters` field and `tool_choice: "required"` is sent to backends that reject it. Reproduced against TheHive AI with DeepSeek/GLM models — effectively a hard blocker for that provider class.

4. **#14115 — Bodyless provider 400s are undiagnosable** (https://github.com/QwenLM/qwen-code/issues/14115)
   The CLI surfaces only `[API Error: 400 status code (no body)]`. Request to surface `errorResponse`, headers, and `x-request-id` in both the CLI error and the `OPENAI_ERROR` log. Directly amplifies #14112.

5. **#14096 / #14097 — vLLM and SGLang context-overflow errors are not recognized** (https://github.com/QwenLM/qwen-code/issues/14096, https://github.com/QwenLM/qwen-code/issues/14097)
   `getContextLengthExceededInfo` doesn't match either server's wording, so reactive compaction never triggers and long sessions become permanently stuck. Two independent reporters (both @Shizoqua) filed the same root cause for two backends.

6. **#14062 — Hosted `send_message` inside its size bound can still fail the entire sender Turn** (https://github.com/QwenLM/qwen-code/issues/14062)
   `encodeToolInput` overflow is thrown rather than returned as a refusal, so the model never gets a "shorten and retry" signal. Local byte checks and the actual encoder disagree.

7. **#14063 — `session_command` timeout text tells a `member_message` sender to "read the board"; retrying duplicates the message** (https://github.com/QwenLM/qwen-code/issues/14063)
   Misleading evidence text plus non-idempotent retry — a correctness issue for multi-agent coordination, tagged `status/ready-for-agent`.

8. **#14110 — Build fails: `packages/cli` uses ink APIs not exported by pinned `ink@7.0.3`** (https://github.com/QwenLM/qwen-code/issues/14110)
   51 TypeScript errors on a clean `pnpm install --frozen-lockfile && pnpm build` at v0.25.1. A contributor-blocking onboarding failure (e.g. missing `ReadonlyFrame` export).

9. **#13787 — XML recovery repeats prefix scans on large multi-call responses** (https://github.com/QwenLM/qwen-code/issues/13787)
   Latency regression on responses with many valid tool calls; reproduction tables compare `main` against PR #13579. Performance-sensitive for agentic loops.

10. **#12333 — eval: verify tool-loading cost and task quality with local CLI A/B** (https://github.com/QwenLM/qwen-code/issues/12333)
    The most-commented issue in the window (16 comments) and now **CLOSED**. Scoping was narrowed to existing local CLI A/B evidence rather than requiring a standard-suite runner; its follow-up **#14065** documents three arm-B failure sessions as concrete counterexamples.

Honorable mentions: **#11272** (cancelling a stdio MCP call kills the server permanently in Channel mode), **#13283** (LSP pull capability never read → 15s timeout vetoes workspace reports), **#14075** (Static mode orphans 1–2 characters at terminal width), **#14122** (Stage D7 `java_durable` admission profile — the last unfinished slice of #12867).

## 4. Key PR Progress

1. **#14123 — fix(managed-agent): restart-proof the held-Runtime drain across Harness takeover** (https://github.com/QwenLM/qwen-code/pull/14123)
   The held-Runtime drain obligation was process-local memory only, so a restart left the Session's close answering incorrectly. This is the durable fix; #13986 is the autofix-takeover variant.

2. **#14069 — feat(managed-agent): consolidate persistence into a 32-table baseline** (https://github.com/QwenLM/qwen-code/pull/14069)
   Collapses 63 business tables into a shared 32-table MySQL 8.4 baseline; server and Runtime Broker now consume one versioned DDL source and the retired migration sequence is archived. (Closed.)

3. **#14073 — feat(providers): add QwenCloud as the first `/auth` entry** (https://github.com/QwenLM/qwen-code/pull/14073)
   Adds QwenCloud ahead of ModelStudio per the #14025 product ruling; the two entries coexist and existing ModelStudio credentials/endpoints are untouched.

4. **#13996 — feat(sdk-java): add private CSI Session and native SQL authority** (https://github.com/QwenLM/qwen-code/pull/13996)
   Extracted from #13526: durable request pinning, writer/first-activation semantics, native journal/resource proof, immutable SQL grants, and authenticated worker bind/history/execute.

5. **#13642 — feat(runtime-broker): bounded retention for terminal JDBC history** (https://github.com/QwenLM/qwen-code/pull/13642)
   Opt-in cleanup of old terminal history under retired bindings, run off the request path on a dedicated scheduler, deleting executions before sessions and bindings.

6. **#13325 — fix(managed-agent): close the critical R2 review findings on #12692** (https://github.com/QwenLM/qwen-code/pull/13325)
   Fixes eight Criticals including an InnoDB lock-order inversion and keyset pagination walking a mutable `updated_at`. Pairs with the release-note fix in today's builds.

7. **#13737 — fix(rewind): resolve legacy turns by source identity** (https://github.com/QwenLM/qwen-code/pull/13737)
   Rewind now links visible messages, model entries, and recorded boundaries through source identity — a recovered legacy key can no longer authorize file restore.

8. **#13816 — fix(managed-agent): hand back the Workspace mount when a tool turn goes recovery blocked** (https://github.com/QwenLM/qwen-code/pull/13816)
   Adds a narrow broker-internal `POST /tool-sessions/{runtimeSessionId}:release-mount` to free only the Workspace execution lease without disturbing other state.

9. **#14120 — test(managed-agent): FG8e/FG8f monitor_run and background-shell fault gates** (https://github.com/QwenLM/qwen-code/pull/14120)
   Multi-process integration tests driving a real packaged Harness, an in-JVM Spring control plane with embedded Runtime Broker, and real MySQL with JDBC ground truth.

10. **#14021 — fix(managed agent): settle never-started background dispatches in the live turn** (https://github.com/QwenLM/qwen-code/pull/14021)
    A failed background Shell dispatch now settles its child task when the original Broker proves no process was admitted, while present/unknown/rejected results remain unsettled.

Also notable: **#13994** (OSC 7501 terminal status reporting with `qwen-code` identifier), **#14035** (RTL/bidi isolation for Web Shell Markdown), **#13957** (Markdown preserved in height-bounded CLI tool results), **#14107/#14108** (hosted recovery test-window widening and shell lifecycle journal-head assertion fixes for CI flakiness), **#9305** (bottom-align short VP content — long-standing UX fix).

## 5. Hot Discussions

No Discussions data was provided for this window; this section is omitted.

## 6. Feature Request Trends

- **Managed Agent durability & multi-agent session management** — the dominant theme. Requests cluster around restart-proof Runtime drain obligations, wake-input execution independent of Session load, actor roles/tenant isolation acceptance (#13535), the `java_durable` admission profile (#14122), and subagent tool behavior (#14065).
- **Broader provider compatibility** — explicit asks for correct tool schema serialization for vLLM-style backends (#14112), provider-specific context-overflow detection for vLLM and SGLang (#14096/#14097), and richer diagnostics for bodyless 4xx responses (#14115).
- **Context & token management** — reactive compaction correctness, tool-loading cost vs. task quality evaluation (#12333), XML tool-call recovery performance (#13787), and deduplicating auto-recall hook context against local memory (#14089).
- **Auth/provider surface expansion** — QwenCloud as a first-class `/auth` entry (#14073) alongside ModelStudio.
- **MCP ecosystem breadth** — ACP deferred-tool announcements (#14114), MCP server lifecycle resilience (#11272), and documentation for third-party MCP web-search services such as SerpApi (#13092).
- **Core tooling & environment integration** — LSP pull-capability negotiation (#13283), slash-command/CLI ergonomics, and desktop/web-shell polish (logo refresh #14038, native drag-and-drop #14049).

## 7. Developer Pain Points

- **Hosted session state is lost across restarts and takeovers.** Concurrent takeover loads race and one is refused with `409 hosted_session_already_attached`, costing a parked Turn a full backoff step (#14070); re-attachment after a control-plane kill fails similarly (#13838); recovered child answers sit unread until the next unrelated input (#14047).
- **Errors that can't be diagnosed.** Bodyless provider 400s (#14115), context-overflow wording mismatches (#14096/#14097), and misleading timeout evidence text (#14063) all leave users with no actionable signal — and in the retry case, duplicate side effects.
- **Self-hosted OpenAI-compatible backends are second-class.** vLLM-backed providers fail on every request (#14112), and neither vLLM nor SGLang overflows trigger compaction.
- **Build and CI friction.** `pnpm build` fails out of the box on the pinned `ink@7.0.3` (#14110), and main-branch CI failures are filed repeatedly by the automation bot (#12714, #14072), with follow-up PRs widening timing windows and fixing recovery assertions (#14107, #14108).
- **MCP server lifecycle fragility.** Cancelling a long-running stdio tool call kills the MCP server with no recovery path in Channel mode (#11272).
- **Terminal rendering polish.** Static-mode word wrap drops 1–2 characters at full width (#14075), and short-viewport content alignment has been an open complaint since August (#9305).
- **Reported size limits don't match enforced limits.** Hosted `send_message` passes local checks but throws in the encoder (#14062), and `qwen batch` accepts lexically distinct paths resolving to the same file, producing duplicate requests (#14041).

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*