# AI CLI Tools Community Digest 2026-10-12

> Generated: 2026-10-12 08:27 UTC | Tools covered: 7

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

# Cross-Tool Comparison Report — AI CLI Ecosystem (2026-10-12)

## 1. Ecosystem Overview

The AI CLI ecosystem has entered a consolidation phase where the competitive battleground has shifted from raw model capability to **agent reliability, orchestration contracts, and entitlement correctness**. Across all seven tools, communities report remarkably similar failure modes — silent subagent failures, session-state corruption on resume, MCP authentication breakage, and billing/quota mismatches — suggesting the category's pain points are structural rather than tool-specific. Release velocity varies sharply: Qwen Code is shipping multiple artifacts (CLI, SDK, desktop), Gemini CLI is on a nightly cadence with a security-hardening wave, while Claude Code, Codex, Copilot CLI, OpenCode, and Pi shipped nothing in the last 24h. The most engaged threads are no longer about flashy features but about **trust**: agents that claim success while doing nothing, and plans that don't match what users paid for.

## 2. Activity Comparison

| Tool | Issues (updated) | PRs (updated) | Discussions | Release Status |
|---|---|---|---|---|
| **Claude Code** | 50 | 0 | N/A (none provided) | None |
| **OpenAI Codex** | Not quantified (~10 notable listed) | 10 (closed, bot-authored batch) | Active (Ideas / Q&A / Show & tell) | None |
| **Gemini CLI** | Not quantified (~10 notable listed) | 10+ (security/hardening cluster) | N/A (none provided) | v0.65.0 nightly |
| **GitHub Copilot CLI** | 22 | 0 | N/A (none provided) | None |
| **OpenCode** | 50 | 50 | N/A (none provided) | None |
| **Pi** | 50 | 10 closed + 1 open | Present (Show & tell) | None |
| **Qwen Code** | Not quantified (~10 notable listed) | 10+ (managed-agent cluster) | N/A (none provided) | v0.25.1 stable + nightly + SDK v0.1.19 + desktop v0.25.1 |

*Note: Issue/PR "updated" counts reflect digest-reported totals; several digests list only the top-ranked items rather than a total. "N/A (none provided)" indicates no discussion data was captured for the period, not that a channel is disabled.*

## 3. Shared Feature Directions

**1. Subagent / multi-agent orchestration reliability (near-universal).**
- *Claude Code*: parallel subagents sharing a cwd with no warning (#101461); background subagent dying on API error returns truncated output with no error signal (#101460); silent write/MCP failures (#13890).
- *Gemini CLI*: subagent reports `GOAL` success after hitting `MAX_TURNS` (#22323); generalist agent hangs forever (#21409); browser agent ignores settings (#22267).
- *Qwen Code*: parent close waits for rather than stops child turns (#13875); writer fencing/takeover (#12952).
- *Copilot CLI*: subagents ignoring model-inheritance settings (#4287).
- *Pi*: `before_agent_start` lifecycle guarantees for extension-driven runs (#5581, #10267, #10639).

**2. Silent / misleading failure as the top trust concern.**
Every community independently flagged agents reporting success while doing nothing — Claude Code (#13890, #101460), Gemini (#22323, #29713), Copilot (#5115, #5101), OpenCode (#42950, #54733), Pi (#10606, #10812), Codex (#38162, #52898).

**3. Plan, billing, and quota-state mismatches.**
- *Claude Code*: Team Max-20x tier request (#47509, 170 👍), Max 20x detected as Pro (#98134).
- *Codex*: "model at capacity" across families (#43375), resets not credited (#37396), catalog omission (#52839).
- *Pi*: ChatGPT Pro resets ignored (#10480), OAuth 403 (#10605).
- *OpenCode*: DeepSeek V4 Flash missing privacy setting (#50155), free-tier permission inconsistency (#50627, #54744).

**4. MCP integration tax — auth, spec drift, and tool exposure.**
- *Copilot CLI*: RFC 8414 issuer mismatch blocks Dataverse (#5082), Atlassian Rovo auth probe 400 (#5118), a single flag wiping all GitHub MCP tools (#5101).
- *Codex*: MCP tools discovered but never reaching Chat (#38162).
- *Claude Code*: 2026-07-28 stateless spec breaking header-bound context (#81965), elicitation URL-mode gaps.
- *Gemini CLI*: MCP SDK bump 1.23.0 → 1.32.0 bundled in a 74-dependency PR (#29664); >128 tools triggers hard 400s (#24246).
- *OpenCode*: MCP discovery starving chat request slots (#53050), orphaned discovery connections (#53046).

**5. Session state fragility on resume.**
Codex (orphaned in-progress turns #41591), Copilot CLI (stale permission replays #4469, rate-limit resume failure #5114), OpenCode (V1→V2 migration abort #54736), Pi (terminal resize freeze #10821).

**6. Terminal/TUI rendering fidelity and configurability.**
Claude Code (resize scrollback corruption #51828, mouse-click opt-out #75599), Codex (Windows paste shortcuts, copyable turn tips), Gemini (streaming flicker #29629), OpenCode (heredoc coloring, plugin input readability), Pi (synchronous full-transcript relayout #10821).

## 4. Differentiation Analysis

| Tool | Feature Focus | Target Users | Technical Approach |
|---|---|---|---|
| **Claude Code** | Plan tiering, TUI control, subagent isolation | Power users on agentic CLI workflows; teams | Proprietary, MCP-centric; issue tracker as primary channel |
| **OpenAI Codex** | Remote Control (mobile/desktop), git UI, Windows parity | IDE-first + mobile-adjacent developers | Multi-surface (TUI, desktop, VS Code, iOS); bot-driven PR landing (`copyberry[bot]`) |
| **Gemini CLI** | Agent/orchestration, security hardening, token-frugal exploration | Open-source contributors, CI/multi-agent users | Native bash affinity, AST-aware exploration EPIC, nightly cadence |
| **GitHub Copilot CLI** | MCP auth, ACP/headless parity, plugin lifecycle | GitHub-ecosystem and automation/CI users | Deep GitHub integration; ACP protocol for programmatic control |
| **OpenCode** | Generative UI, provider flexibility, free-tier permissions | Multi-provider users, desktop/TUI enthusiasts | Provider-agnostic; generative UI (`genui_render`) as differentiator |
| **Pi** | OpenAI/ChatGPT auth reliability, compaction control, extensions | Extension developers, local-model (llama.cpp) users | Extension lifecycle APIs; RPC mode for third-party native apps |
| **Qwen Code** | Managed Agent architecture (durable sessions), Web Shell, team messaging | Enterprise/multi-agent platform builders | Full-stack (CLI + SDK + Java runtime + Desktop); fastest iteration |

**Key differentiators:** Claude Code competes on agentic depth; Codex on multi-surface reach; Gemini CLI on open-source extensibility and security; Copilot CLI on GitHub-native workflows; OpenCode on provider neutrality and generative UI; Pi on extension/hackability; and Qwen Code is the only tool clearly pursuing an **enterprise multi-agent platform** narrative.

## 5. Community Momentum & Maturity

**Rapidly iterating (high release + merge throughput):**
- **Qwen Code** — Multiple simultaneous releases (CLI, SDK, desktop), a 56-comment architecture umbrella (#12380), and active managed-agent PR stream. Highest structural velocity; maturation risk is CI reliability (Flyway ordering #13922, CVE audit failures #13078).
- **OpenCode** — 50 issues + 50 PRs, broad feature surface (generative UI, Linux window controls, session tabs). High throughput but signals migration/upgrade fragility (#54736).
- **Gemini CLI** — Nightly cadence; a distinct security-hardening wave (path traversal #29522, checkpoint containment #29521, A2A trust #29525, env isolation #29523). Strong maintainer responsiveness.

**High-engagement, lower-ship-tempo (issue-dominated):**
- **Claude Code** — 50 issues, zero PRs/releases; engagement is deep (170 👍 on plan tiering) but delivery is stalled on the issues that matter most.
- **Pi** — 50 issues, mostly closure-oriented PRs; strong extension ecosystem and clear regression triage (1.1.0 bug cluster).
- **Codex** — Bot-driven PR batches (all 10 authored by `copyberry[bot]`) suggest automated landing rather than organic contribution; Windows remains the clear weak platform.
- **Copilot CLI** — Steady 22 issues, no PRs/releases; MCP auth and ACP parity are the emerging bottleneck.

**Maturity signal:** The most mature communities (Claude Code, Codex, Copilot) show *concentrated, sustained* threads on billing and platform gaps — classic signs of an installed base hitting production limits. The youngest/rising (Qwen, OpenCode) show *diffuse* feature breadth and architectural churn.

## 6. Trend Signals

**1. Silent failure is the defining UX problem of 2026.** Across all seven tools, the highest-anxiety bugs are agents that report success while doing nothing. The industry is converging on the need for **verifiable execution contracts** — explicit error propagation, end-of-turn verification summaries (Claude Code #101247), and trustworthy status signals. For developers, this means designing automation that assumes tool output can lie and needs external validation.

**2. Concurrency contracts are the next frontier.** Shared working directories (#101461), writer fencing (#12952), and stale notifications (#101462) indicate that parallel agent execution lacks a formalized isolation model. Expect per-subagent sandboxing and explicit lifecycle ownership to become table stakes.

**3. Billing/entitlement is a competitive liability.** Claude Code (#47509), Codex (#43375), Pi (#10480), and OpenCode (#50155) all show entitlement layers failing to match purchased reality. This is unblockable from the user side and drives outsized frustration — a reliability dimension vendors systematically under-invest in.

**4. MCP spec drift imposes a recurring integration tax.** Every tool with an MCP surface reports version-related breakage. As MCP becomes the de-facto standardization layer, **capability negotiation and backward compatibility** are the integration authors' cost center — a signal to budget for continuous MCP maintenance.

**5. Windows is the industry-wide weak platform.** Codex (#51981 ACL failures, #25271 browser breakage), Copilot CLI (#5119 plugin uninstall), and Pi (#10801 auth lock) all cluster on Windows-specific packaging, sandbox, and permission issues. Cross-platform parity remains under-served.

**6. Session durability is a hard requirement, not a nice-to-have.** Resume-time corruption appears in Codex, Copilot, OpenCode, and Pi. As sessions grow longer and agents run unattended, **durable, reconcilable session state** (Qwen's Managed Agent work being the most explicit effort) is becoming the architectural differentiator.

**Reference value for developers:** The converged pain points define a shortlist of non-negotiables for anyone building on or integrating these tools today: (a) never trust agent-reported success without external verification; (b) isolate concurrent agents at the filesystem and session level; (c) treat entitlement/quota state as a first-class integration risk; (d) budget for MCP version churn; and (e) favor tools with active release cadence (Qwen, Gemini, OpenCode) when stability-under-change matters more than a frozen feature set.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

**Note:** PR comment counts are shown as `undefined` in the provided snapshot, so the ranking below follows the repository’s provided “popular PR” ordering and recent update/impact signals.

## 1. Top Skills Ranking

| Rank | Skill / PR | What it does | Discussion / attention highlights | Status |
|---|---|---|---|---|
| 1 | **mcp-builder: MCP v2 compatibility fix** — [#1742](https://github.com/anthropics/skills/pull/1742) | Fixes `mcp>=2` support by updating `streamablehttp_client` → `streamable_http_client` and moving custom headers to `create_mcp_http_client` / `http_client`. | Addresses issue #1668; high-impact because MCP builder is core infrastructure for tool/server integrations. Updated 2026-10-08. | OPEN |
| 2 | **skill-creator: trigger eval isolation + Windows/runtime handling** — [#1298](https://github.com/anthropics/skills/pull/1298) | Fixes false misses/invalid scores in trigger evaluation: worker probe competition, Windows subprocess pipe failures, unrelated tool interference, and runtime failures being treated as non-triggers. | Directly targets reliability of skill evaluation and optimization. Updated 2026-09-16. | OPEN |
| 3 | **proofcore-contract-auditor** — [#1771](https://github.com/anthropics/skills/pull/1771) | New Web3 Agent Skill for static analysis of Solidity/Rust smart contracts, anchoring audit proofs on TON via ProofCore’s zero-storage Merkle protocol. | New domain expansion into smart-contract notarization/audit; active new-skill proposal. Updated 2026-09-16. | OPEN |
| 4 | **docx: detect orphaned comments** — [#1734](https://github.com/anthropics/skills/pull/1734) | Targets orphaned comments in `.docx` documents. | Summary is empty, so attention is inferred from title and document-skill relevance. Updated 2026-09-25. | OPEN |
| 5 | **md2video-audio** — [#1703](https://github.com/anthropics/skills/pull/1703) | New zero-cost skill that compiles Markdown into MP4 videos with realistic voiceovers, using Marp and related tooling. | Expands Skills into media generation/document-to-video workflows. Updated 2026-09-15. | OPEN |
| 6 | **notion-spec-to-implementation + quantitative-resume-auditor** — [#1245](https://github.com/anthropics/skills/pull/1245) | Adds two skills: one turns product/tech specs into Notion implementation tasks; the other audits resumes quantitatively. | Combines workflow automation with document/recruiting use cases. Updated 2026-09-30. | OPEN |
| 7 | **skill-creator: harden eval viewer** — [#1961](https://github.com/anthropics/skills/pull/1961) | Security-hardens `generate_review.py` and `viewer.html`: script breakout, DNS rebinding, cross-site POST, and escaping gaps. | Security-focused PR on a core meta-skill; updated very recently on 2026-10-11. | OPEN |
| 8 | **docx: LibreOffice timeout as error + output verification** — [#1792](https://github.com/anthropics/skills/pull/1792) | Makes `accept_changes.py` return an error on LibreOffice timeout and verify output DOCX no longer contains revision marks. | Reliability fix for document workflows; updated 2026-09-25. | OPEN |

## 2. Community Demand Trends

From the Issue list, the strongest demand is not simply “more Skills,” but better **trust, reliability, and context efficiency** around Skills.

- **Security and trust boundaries** — Community skills under the `anthropic/` namespace can impersonate official skills: [#492](https://github.com/anthropics/skills/issues/492). Related concerns include eval-viewer XSS ([#1394](https://github.com/anthropics/skills/issues/1394)), SharePoint permission security ([#1175](https://github.com/anthropics/skills/issues/1175)), and agent-governance proposals ([#412](https://github.com/anthropics/skills/issues/412)).
- **Skill evaluation and trigger reliability** — Users report `run_eval.py` never triggering skills/commands ([#556](https://github.com/anthropics/skills/issues/556)), parallel-worker UUID cross-matching ([#1352](https://github.com/anthropics/skills/issues/1352)), and silent benchmark failures ([#1383](https://github.com/anthropics/skills/issues/1383)).
- **Context-window and token efficiency** — The `claude-api` skill eagerly injecting ~156k tokens ([#1487](https://github.com/anthropics/skills/issues/1487)) and duplicate skills from overlapping plugins ([#189](https://github.com/anthropics/skills/issues/189)) highlight demand for leaner, lazier skill loading.
- **Memory and compact agent state** — Proposal for `compact-memory`, a symbolic notation for compact persistent agent state: [#1329](https://github.com/anthropics/skills/issues/1329).
- **Team/org distribution** — Demand for org-wide skill sharing instead of manual `.skill` file downloads/uploads: [#228](https://github.com/anthropics/skills/issues/228).
- **Quality gates and verification workflows** — Proposal for pre-task calibration, adversarial review, and delivery verification: [#1385](https://github.com/anthropics/skills/issues/1385).
- **Skill-authoring best practices** — Requests to make `skill-creator` more operational and token-efficient rather than human-facing documentation: [#202](https://github.com/anthropics/skills/issues/202).

## 3. High-Potential Pending Skills

These are open PRs with recent activity, core-skill impact, or direct issue links — likely candidates to land or shape the ecosystem soon.

- **mcp-builder MCP v2 fix** — [#1742](https://github.com/anthropics/skills/pull/1742): Core compatibility fix for `mcp>=2`; updated 2026-10-08.
- **skill-creator eval-viewer hardening** — [#1961](https://github.com/anthropics/skills/pull/1961): Security fix for local eval viewer; updated 2026-10-11.
- **webapp-testing: remove `shell=True`** — [#1980](https://github.com/anthropics/skills/pull/1980): Mitigates command-injection risk (CWE-78); created 2026-10-06.
- **docx: LibreOffice timeout/error verification** — [#1792](https://github.com/anthropics/skills/pull/1792): Makes document conversion failures visible and verified; updated 2026-09-25.
- **algorithmic-art: fix `wrapAround()`** — [#1977](https://github.com/anthropics/skills/pull/1977): Corrects wrapping behavior; fixes #1897; updated 2026-10-07.
- **webapp-testing: correct textarea/select reporting** — [#1976](https://github.com/anthropics/skills/pull/1976): Improves element discovery; fixes #1891; updated 2026-10-07.
- **skill-creator: direct `package_skill.py` execution** — [#1681](https://github.com/anthropics/skills/pull/1681): Fixes `ModuleNotFoundError` and outdated paths; updated 2026-10-08.
- **proofcore-contract-auditor** — [#1771](https://github.com/anthropics/skills/pull/1771): New Web3 smart-contract audit skill; updated 2026-09-16.

## 4. Skills Ecosystem Insight

The community’s most concentrated demand is for **trustworthy, secure, reliable, and token-efficient Skill infrastructure** — especially namespace integrity, trigger/eval correctness, and context-window discipline — rather than only more domain-specific Skills.

---

# Claude Code Community Digest — 2026-10-12

## 1. Today's Highlights

No new releases or pull requests landed in the last 24 hours; all activity was concentrated in the issue tracker, where 50 issues were updated. The dominant thread is **plan economics**: the long-running Team-plan tiering request (#47509, 60 comments / 170 👍) and a pair of billing/auth regressions around the Max 20x subscription continue to draw sustained engagement. A secondary cluster of freshly filed issues (all from today) flags **parallel and background subagent correctness problems** — shared working directories, missing error signals, and stale monitor notifications — suggesting agent orchestration reliability is the emerging frontier concern.

## 2. Releases

None in the last 24 hours.

## 3. Key PR Progress

No pull requests were updated in the last 24 hours.

## 4. Hot Issues

1. **[#47509 — Team plan needs a Max 20x equivalent tier](https://github.com/anthropics/claude-code/issues/47509)** (OPEN, 60 comments, 170 👍)
   The highest-engagement issue in the cohort. Team seats top out at Premium (6.25x Pro), which power users on agentic CLI workflows say is insufficient; they want a Max-20x-class tier inside Team billing. The volume of 👍 relative to comments signals broad latent demand rather than a niche complaint.

2. **[#51828 — Scrollback duplication on terminal resize in 2.1.116](https://github.com/anthropics/claude-code/issues/51828)** (OPEN, has repro, 29 comments, 36 👍)
   Reproducible TUI corruption in VS Code's integrated terminal on macOS after resize. High 👍 on a rendering bug means it disrupts daily workflows; the "persists in version X" framing implies it has survived multiple fix attempts.

3. **[#98134 — Active Apple Max 20x subscription recognized as Pro](https://github.com/anthropics/claude-code/issues/98134)** (OPEN, area:auth, 11 comments)
   Entitlement misclassification on macOS: subscribers paying for Max 20x are capped at Pro limits. Pairs with #47509 and #61828 into a coherent "the billing layer doesn't match what users bought" theme.

4. **[#64479 — Edit tool fails on mixed literal/escape Unicode](https://github.com/anthropics/claude-code/issues/64479)** (OPEN, has repro, platform:linux, 18 comments)
   A multi-line `old_string` containing the same character as both a literal codepoint and a `\uXXXX` escape defeats the fallback swap logic. Low emoji count but persistent comments indicate a stubborn correctness bug in a core tool.

5. **[#75599 — Granular control over mouse click behavior in interactive menus](https://github.com/anthropics/claude-code/issues/75599)** (OPEN, enhancement, 11 comments, 23 👍)
   Since v2.1.181, single-click immediately confirms selections in permission prompts, `/model`, `/config`, and `AskUserQuestion` dialogs. Users want an opt-out — a small configurability gap with disproportionate ergonomic impact.

6. **[#45775 — Gmail MCP regression: `create_draft` missing `threadId`](https://github.com/anthropics/claude-code/issues/45775)** (CLOSED, invalid, area:mcp, 31 comments, 25 👍)
   The highest-comment closed issue of the day. Closed as *invalid* after 31 comments, which likely means the underlying signal lives upstream in the connector rather than the CLI — worth watching for recurrence.

7. **[#13890 — Subagents silently fail to write files or call MCP tools](https://github.com/anthropics/claude-code/issues/13890)** (CLOSED, has repro, 14 comments, 12 👍)
   Task-tool subagents believe operations succeeded while nothing reaches disk. Silent failure mode makes it a trust issue, not just a bug.

8. **[#101461 — Parallel subagents share a working directory with no warning](https://github.com/anthropics/claude-code/issues/101461)** (OPEN, filed 2026-10-12)
   Two concurrent Agent-tool invocations inherit the same cwd; concurrent builds/tests writing to shared output can silently corrupt artifacts. Day-one issue with clear architectural implications for parallel agent execution.

9. **[#101460 — Background subagent dying on API error returns truncated result with no error signal](https://github.com/anthropics/claude-code/issues/101460)** (OPEN, filed 2026-10-12)
   After retry exhaustion, a backgrounded subagent returns partial output with no failure marker. Same trust-erosion pattern as #13890, now in the background-execution path.

10. **[#100723 — GitHub integration: cloud routines cannot `add_repo` to push](https://github.com/anthropics/claude-code/issues/100723)** (OPEN, platform:web, area:routines)
    Unattended scheduled routines are expected to push HTML files to a private repo but report no `add_repo` capability. Blocks the "set it and forget it" automation story that routines are sold on.

*Also notable:* [#94373](https://github.com/anthropics/claude-code/issues/94373) (desktop notifications should distinguish permission-wait from task-complete), [#87978](https://github.com/anthropics/claude-code/issues/87978) (VoiceOver/screen-reader turn-completion signaling), [#101356](https://github.com/anthropics/claude-code/issues/101356) (Opus 5.5 misclassifying hardening of user-owned source as malicious), and [#93246](https://github.com/anthropics/claude-code/issues/93246) (desktop browser pane loops on HTTP Basic Auth).

## 5. Hot Discussions

No Discussion data was provided for this period.

## 6. Feature Request Trends

- **Plan tiering and cost transparency.** #47509 (Team Max-20x tier), #98134 (Max 20x detected as Pro), and the CLOSED #61828 (premature "usage limit reached") collectively form the single loudest request: make entitlement and metering match what users paid for, and give power users a headroom tier.
- **TUI configurability and rendering fidelity.** #75599 (mouse-click opt-out), #76988 (re-run `statusLine` on SIGWINCH), #89824 (emoji reactions on assistant messages), and #51828 (resize scrollback corruption) point to demand for a terminal UI that is both more controllable and more stable.
- **Composable terminal affordances.** #101247 asks the harness — not the model — to emit a verification summary at end-of-turn, a notable shift toward trusting tooling over self-reported model claims.
- **Desktop notification semantics.** #94373 and #67992 both want richer session state in the OS/desktop layer: differentiate permission-blocked from finished, and reliably surface scheduled/routine sessions with badges.
- **Subagent isolation and observability.** #101461 and #101460 request per-subagent working directories and explicit error propagation — the beginnings of a real concurrency contract for the Agent tool.
- **MCP spec currency.** #48164, #88075, and #69087 (all CLOSED) cluster around URL-mode elicitation support, while #81965 flags header-bound context loss under the 2026-07-28 stateless spec — MCP version drift is a recurring integration tax.
- **Accessibility.** #87978 is the clearest signal that screen-reader users need explicit turn-completion affordances, not a wall of streaming tokens.

## 7. Developer Pain Points

- **Billing and entitlements drift from reality.** Usage counters, plan tiers, and subscription detection are all producing mismatches (#47509, #98134, #61828). This drives the most engagement and the most frustration because it is unblockable from the user side.
- **Silent failure is the worst failure.** Across #13890, #101460, and #77325, agents report success while doing nothing, or return truncated output with no error marker. Developers cannot build reliable automation on tools that lie about outcomes.
- **Concurrency hazards in multi-agent runs.** Shared working directories (#101461) plus stale, superseded notifications (#101462) mean parallel agent workloads can corrupt state without anyone noticing — a trust problem that will grow with adoption.
- **MCP ecosystem churn.** New spec revisions (2026-07-28 stateless, elicitation URL-mode) repeatedly break existing servers and leave capability negotiation gaps (#81965, #88075, #48164, #69087, #47733). Integration authors bear the maintenance cost.
- **Terminal/TUI fragility across host environments.** Resize handling, VS Code integration, fullscreen dialog clipping, and statusline staleness (#51828, #69087, #76988) disproportionately affect users in the environments where Claude Code is most heavily used.
- **Blocked unattended automation.** Routine and GitHub-integration issues (#100723, #67992) undercut the cloud/scheduled-task value proposition precisely where hands-off reliability matters most.

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-10-12

## 1. Today's Highlights
No new releases shipped in the last 24 hours; activity is dominated by long-running bug threads and a large batch of closed TUI/app-server PRs from an automated contributor (`copyberry[bot]`). The highest-engagement issues remain **iOS/macOS Remote Control regressions** (#36040, 79 comments) and **Windows platform gaps** across sandboxing, browser automation, and local project persistence. On the enhancement side, community pressure is mounting to restore **visible Commit/Push controls** in the desktop sidebar (#47511, 46 👍).

## 2. Releases
No releases published in the last 24 hours.

## 3. Hot Issues

1. **#36040 — iOS Remote only lists projects with recent chats** (OPEN, 79 comments, 👍5) — Highest-traffic thread. Remote Control paired with a macOS desktop host silently hides projects without recent chat activity, making the mobile surface unreliable for real project switching. [Link](https://github.com/openai/codex/issues/36040)
2. **#25271 — Computer Use can't determine Chrome URL on Windows** (OPEN, 56 comments, 👍12) — Even `chrome://newtab/` fails URL resolution, blocking browser-driven workflows on Windows. Long-lived (since May) with no confirmed fix. [Link](https://github.com/openai/codex/issues/25271)
3. **#43375 — Multiple GPT-5/GPT-6 models return "Selected model is at capacity"** (OPEN, 30 comments, 👍19) — Capacity errors span multiple models, so switching models does not recover the session. Directly impacts paid-tier reliability. [Link](https://github.com/openai/codex/issues/43375)
4. **#47511 — Missing Commit and Push buttons** (OPEN, 19 comments, 👍46) — Regression in desktop build 26.917.51856 that buries core git actions in an unlabeled overflow menu. Highest reaction count in the current issue set; duplicated by enhancement #47897. [Link](https://github.com/openai/codex/issues/47511)
5. **#41591 — Orphaned inProgress turn hides later completed turns (macOS)** (OPEN, 15 comments, 👍3) — A stuck turn in long threads hides subsequent output and survives restart, implying persisted bad state in the session store. [Link](https://github.com/openai/codex/issues/41591)
6. **#38162 — Local MCP tools discovered but not exposed to Chat sessions** (OPEN, 12 comments, 👍2) — MCP servers start successfully on Windows but tools never reach Chat, a silent capability gap rather than a hard error. [Link](https://github.com/openai/codex/issues/38162)
7. **#51981 — Windows sandbox ACL validation fails with os error 32 on `node_repl.exe`** (OPEN, 11 comments) — Blocks Chrome control *and* shell commands, indicating the sandbox token/ACL layer is the shared root cause behind several Windows browser-automation bugs. [Link](https://github.com/openai/codex/issues/51981)
8. **#52839 — Sol Ultrafast omitted from first-party model catalog under ChatGPT auth** (OPEN, 10 comments) — Users with `fast_mode`/`ultrafast_mode` enabled and no catalog override still can't select the model; an entitlement/catalog plumbing mismatch. [Link](https://github.com/openai/codex/issues/52839)
9. **#49735 — Token-budget reminders encourage premature closure in long-running agents** (OPEN, 5 comments) — A substantive model-behavior report requesting runtime and training ablations; notable because it targets agentic reliability rather than UI. [Link](https://github.com/openai/codex/issues/49735)
10. **#50084 — VS Code Codex extension stuck in streaming state after turn completes** (OPEN, 3 comments, 👍6) — Only an Extension Host restart clears it; reloading the webview is insufficient. High impact for IDE-first users. [Link](https://github.com/openai/codex/issues/50084)

**Also worth tracking:** #52917 / #53305 (Windows desktop loses local project catalog; #53305 since closed), #52836 (state-DB backfill stuck in `running` → "Organization settings could not be loaded"), #50986 (Codex Cloud runtime has no outbound network), #32241 (macOS Remote Control "stream became unknown" resets), #44334 (project sessions cluttering global Recents).

## 4. Key PR Progress
All 10 PRs below were **closed on 2026-10-11 and authored by `copyberry[bot]`**, suggesting a single automated/agent-driven landing batch rather than independent community contributions.

1. **#53224 — Support Windows clipboard text paste shortcuts in the TUI** — `Ctrl+V` previously attempted image-paste only (erroring on text clipboards); `Shift+Insert` now routes through the paste path. [Link](https://github.com/openai/codex/pull/53224)
2. **#53201 — Keep agents navigation available after a thread is archived** — Fixes the dead Left-arrow agents shortcut when a thread is archived externally while the composer is empty. [Link](https://github.com/openai/codex/pull/53201)
3. **#53173 — Allow restarting managed background servers from the TUI** — Lets users refresh stale startup-config warnings without leaving the TUI. [Link](https://github.com/openai/codex/pull/53173)
4. **#53156 — Simplify the custom terminal drawing API** — Removes `Terminal::autoresize` and fallible `try_draw` helpers; rendering consolidates in `draw_with_size`. Internal cleanup reducing TUI rendering complexity. [Link](https://github.com/openai/codex/pull/53156)
5. **#53155 — Keep remote app-server events deliverable during shutdown** — Prevents an in-flight notification from killing the worker before the WebSocket close frame is sent. Directly relevant to Remote Control stability issues. [Link](https://github.com/openai/codex/pull/53155)
6. **#53153 — Show subagent navigation shortcuts in TUI help** — Adds `Alt+Left` / `Alt+Right` ("Subagents (empty prompt)") to the shortcut overlay, with updated snapshots. [Link](https://github.com/openai/codex/pull/53153)
7. **#53152 — Allow selecting and copying TUI turn tips** — Registers working/completion tip rows for mouse selection and copy. [Link](https://github.com/openai/codex/pull/53152)
8. **#53150 — Restore thread-owned permissions on cold resume** — Fixes cold resume replacing saved unnamed permissions with current defaults, including first-turn context that predates a settings update. Security-relevant. [Link](https://github.com/openai/codex/pull/53150)
9. **#53148 — Fix startup draft recovery for pending Vim Replace keys** — Corrects recovery of unsent text (e.g. `abcd` → `Xbcd`, not `Xabcd`) after an interrupted Vim Replace. [Link](https://github.com/openai/codex/pull/53148)
10. **#53109 — Handle broken pipes gracefully in plugin list output** — `codex plugin list | head` no longer panics; table and JSON output use a fallible writer. [Link](https://github.com/openai/codex/pull/53109)

## 5. Hot Discussions

**Ideas**
- **#14595 — "Remote control wen?"** (7 comments, 👍18) — Sustained demand for first-class remote control, with users reporting third-party workarounds (farfield) are not good enough. Highest-signal roadmap request in the set. [Link](https://github.com/openai/codex/discussions/14595)
- **#53061 — Resume an existing local Codex chat when GitHub Actions CI finishes** — Requests an event trigger so Codex can continue a PR workflow after CI completes instead of manual "continue" prompts or polling. [Link](https://github.com/openai/codex/discussions/53061)
- **#53294 — Suppress `config.toml` deprecation warnings for Cloud-Managed/Enterprise layers** — Proposes suppressing un-dismissible banners when legacy keys originate from managed config rather than the user. [Link](https://github.com/openai/codex/discussions/53294)
- **#53216 — Adaptive Auto Reasoning Mode** — Wants automatic reasoning-level selection to cut usage waste on simple tasks. [Link](https://github.com/openai/codex/discussions/53216)

**Q&A**
- **#53079 — [ANSWERED] Building a reliable full-stack workflow with Codex** (3 comments) — Feature branches, structured agent workflows, professional practices; answered. [Link](https://github.com/openai/codex/discussions/53079)
- **#53199 — Enable network access for a single project when permission profiles are missing from the picker** — Wants per-project network access while keeping the filesystem sandbox and other projects restricted. [Link](https://github.com/openai/codex/discussions/53199)

**Show and tell**
- **#42049 — Bundled-marketplace refresh leaks staging directories (≈31 GiB)** — 323 abandoned `openai-bundled.staging-*` dirs caused severe disk exhaustion on macOS. [Link](https://github.com/openai/codex/discussions/42049)
- **#51406 — "No Comment" PostToolUse hook** — Hook that strips chat-narrating comments Codex writes; addresses instruction fade-out over long sessions. [Link](https://github.com/openai/codex/discussions/51406)
- **#53043 — plan-ledger** — Hooks + CLI that record what `/plan` agreed and flag changed files outside the plan. [Link](https://github.com/openai/codex/discussions/53043)
- **#53200 — Reset Alerts** — SMS (US/CA) / Telegram notifications for Codex rate-limit resets, plus reset dashboards. [Link](https://github.com/openai/codex/discussions/53200)

## 6. Feature Request Trends
- **Remote Control maturity** — Mobile/desktop remote control is the single most requested direction: project listing parity (#36040), stream reliability (#32241, #35727), and a general roadmap ask (#14595).
- **Per-project network and permission scoping** — #53199 and related sandbox issues point to demand for granular, persistent network grants that don't weaken filesystem isolation.
- **Restore/retain direct git actions** — #47511 and #47897 both ask for visible Commit/Push controls, pushing back on UI simplification.
- **Session & project catalog hygiene** — Keep project sessions out of global Recents (#44334), stop catalog loss on crash/restart (#52917, #53305), and restore state after cold resume (#53150).
- **Adaptive reasoning / usage efficiency** — #53216 plus rate-limit threads (#37396, #51026) show appetite for automatic reasoning-level selection.
- **CI/CD and event-driven agent continuation** — #53061 asks for Codex to resume work on external events (GitHub Actions) rather than manual prompts.
- **IDE/extension reliability** — #50084 (VS Code stuck streaming) reflects a growing IDE-first cohort with distinct failure modes.

## 7. Developer Pain Points
- **Windows is the clear weak platform.** Sandbox ACL failures (#51981), browser/Computer Use breakage (#25271, #45340, #52898), MCP tools not reaching Chat (#38162), and project catalog loss (#52917, #53305) cluster around the same builds — suggesting a shared Windows sandbox/state-layer root cause.
- **Remote Control loses trust on state.** Projects disappear from mobile (#36040), active tasks are blocked by writer locks (#51053), transcripts truncate after sequence gaps (#35727), and healthy streams reset (#32241). PR #53155 addresses only the shutdown-delivery slice.
- **Rate-limit and entitlement surprises.** "Model is at capacity" across model families (#43375), resets advertised but not credited (#37396), unavailable usage resets (#51026), and subagents failing instantly with `usage_limit_exceeded` (#32432).
- **Silent capability gaps beat loud errors.** MCP tools discovered but not exposed (#38162), browser backends detected but unusable (#52898), and model catalog omissions under ChatGPT auth (#52839) all fail without actionable diagnostics.
- **Session state is fragile.** Orphaned in-progress turns (#41591), chats vanishing after restart (#38927), and VS Code extension stuck streaming (#50084) force restarts and manual recovery.
- **Disk hygiene in the desktop app.** Abandoned marketplace staging directories consuming ~31 GiB (#42049) is an unresolved operational hazard for heavy users.
- **UI regressions in the name of simplification.** Burying Commit/Push drew the strongest single reaction (👍46) of any open issue.

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-10-12

## 1. Today's Highlights

Activity today centered on the **agent/subagent workstream**: a large batch of long-running `area/agent` issues re-entered `status/need-retesting` (#22323, #21409, #21968, #22267, #21983), suggesting maintainers are validating recent fixes around subagent termination semantics, hangs, and config propagation. On the PR side, a cluster of security-hardening patches landed or closed — env isolation for external checkers, glob path containment, checkpoint path traversal, and A2A workspace trust — alongside core fixes for JSON serialization, Gemini 3 multimodal tool responses, and streaming flicker.

## 2. Releases

- **v0.65.0-nightly.20261012.g9b6e0265d** — nightly release published for today, accompanied by the automated version-bump PR [#29714](https://github.com/google-gemini/gemini-cli/pull/29714). No user-facing changelog beyond the comparison diff: [compare v0.65.0-nightly.20261010...v0.65.0-nightly.20261012](https://github.com/google-gemini/gemini-cli/compare/v0.65.0-nightly.20261010.g9b6e0265d...v0.65.0-nightly.20261012.g9b6e0265d)

## 3. Hot Issues

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — Subagent recovery after MAX_TURNS reported as GOAL success (P1, 13 comments, 👍2)** — The `codebase_investigator` subagent claims `status: "success"` / `Termination Reason: GOAL` despite having hit the turn limit before doing any analysis. This is the top-discussed issue of the day because it silently corrupts agent reliability signals — the core failure mode behind "the agent said it was done, but nothing happened."
2. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — Zero-dependency OS sandboxing & post-execution intent routing (P2, 9 comments, 👍1)** — Proposes leaning into Gemini 3's native bash affinity by chaining POSIX tools inside a sandbox rather than masking them behind bespoke tools. A large-effort design thread that would reshape how the CLI handles file exploration and edits.
3. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — Generalist agent hangs forever (P1, 8 comments, 👍8)** — Highest community reaction count today: deferring to the generalist agent makes even trivial operations (folder creation) hang indefinitely; disabling subagent deferral is the only workaround. Now in retesting.
4. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — Assess AST-aware file reads, search, and mapping (EPIC, 7 comments)** — Tracks whether AST-aware tools reduce misaligned reads, token noise, and turn count. Signals a strategic shift toward structural code understanding instead of raw text firehosing.
5. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini does not use skills and sub-agents enough (7 comments)** — Anecdotal but widely echoed: the model ignores custom skills and subagents unless explicitly instructed, undercutting the whole extensibility story.
6. **[#22267](https://github.com/google-gemini/gemini-cli/issues/22267) — Browser Agent ignores `settings.json` overrides e.g. `maxTurns` (4 comments)** — `AgentRegistry` merges settings correctly at init, but the Browser Agent bypasses them at runtime — a configuration-contract bug affecting any tuned deployment.
7. **[#22232](https://github.com/google-gemini/gemini-cli/issues/22232) — Browser agent session takeover / lock recovery (P3, 4 comments)** — Requests replacing the "fail-fast" behavior on locked persistent browser profiles with automatic takeover of orphaned sessions. High-value for CI and multi-agent use.
8. **[#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — Browser subagent fails on Wayland (P1, 4 comments, 👍1)** — Reports a GOAL termination without actual completion on Wayland sessions; Linux desktop users remain a recurring blind spot.
9. **[#20079](https://github.com/google-gemini/gemini-cli/issues/20079) — Symlinked agent files in `~/.gemini/agents/` not recognized (4 comments)** — Blocks dotfile-manager and monorepo workflows; needs-info but a simple, concrete gap in agent discovery.
10. **[#24246](https://github.com/google-gemini/gemini-cli/issues/24246) — 400 error with >128 tools (3 comments)** — Tool-count explosion from MCP servers and subagents triggers hard API failures; the agent needs smarter tool scoping.

*Also worth watching:* **[#29713](https://github.com/google-gemini/gemini-cli/issues/29713)** (new, awaiting triage) — `list_directory` silently drops entries that fail path validation, reporting a successful but incomplete listing.

## 4. Key PR Progress

1. **[#29626](https://github.com/google-gemini/gemini-cli/pull/29626) — fix(core): preserve shared references in JSON serialization (P2)** — `safeJsonStringify` used a single global `WeakSet`, replacing legitimately shared (non-cyclic) objects with `[Circular]`; affects OTel metrics and any re-serialized telemetry payload.
2. **[#29611](https://github.com/google-gemini/gemini-cli/pull/29611) — fix(core): multimodal function response for dotted Gemini 3 models and aliases (P2)** — Resolves aliases and dotted versions in `supportsMultimodalFunctionResponse()`, preventing invalid sibling parts on models like `gemini-3.8-flash`.
3. **[#29621](https://github.com/google-gemini/gemini-cli/pull/29621) — fix(core): preserve subagent multimodal tool response parts (P2)** — Image data emitted alongside a function response was being discarded when a local subagent fed results back to the model.
4. **[#29527](https://github.com/google-gemini/gemini-cli/pull/29527) — fix(core): ensure request contents do not end with a model turn (P1, closed)** — Fixes the `400 Bad Request` after `/rewind`, stream interruption, or stripped trailing user turns. Closes a long-standing P1 reliability gap.
5. **[#29523](https://github.com/google-gemini/gemini-cli/pull/29523) — fix(core): minimal env and capped output for external safety checkers (P2, closed)** — Stops leaking `GEMINI_API_KEY` and other secrets into third-party checker processes and caps unbounded stdout accumulation.
6. **[#29522](https://github.com/google-gemini/gemini-cli/pull/29522) — fix(core): keep glob tool matches inside validated search directory (closed)** — Absolute glob patterns (e.g. `/etc/*.conf`) previously escaped the validated `dir_path`; a real path-traversal fix for the glob tool.
7. **[#29521](https://github.com/google-gemini/gemini-cli/pull/29521) — fix(core): contain legacy checkpoint paths (P1, closed)** — Tags like `x/../../secret` resolved outside the checkpoint directory; hardens checkpoint read/delete paths.
8. **[#29525](https://github.com/google-gemini/gemini-cli/pull/29525) — fix(a2a-server): never derive workspace trust from request `agentSettings` (closed)** — Aligns `createTask()` with the other entry points that normalize settings, closing a trust-escalation vector in the A2A server.
9. **[#29629](https://github.com/google-gemini/gemini-cli/pull/29629) — fix(cli): cap pending plain text height to reduce streaming flicker (P2)** — Caps streaming markdown height to avoid full-screen clear-and-redraw on every update; directly addresses the flicker pain from [#21924](https://github.com/google-gemini/gemini-cli/issues/21924).
10. **[#29709](https://github.com/google-gemini/gemini-cli/pull/29709) — fix(vscode-ide-companion): track all `activate()` disposables (P2)** — Comma expressions in `context.subscriptions.push(...)` meant registrations leaked and weren't disposed. Fixes #27790.

*Additional:* **[#29664](https://github.com/google-gemini/gemini-cli/pull/29664)** (P1, XL) bundles 74 npm dependency bumps including `@modelcontextprotocol/sdk` 1.23.0 → 1.32.0 — worth review attention given the MCP surface area. **[#29705](https://github.com/google-gemini/gemini-cli/pull/29705)** fixes `formatDuration` unit-boundary rounding (`1000ms` → `1.0s`).

## 5. Hot Discussions

No discussion data was provided for this period.

## 6. Feature Request Trends

- **Smarter, more autonomous subagent orchestration** — Multiple threads converge on the agent *choosing* to use skills/subagents ([#21968](https://github.com/google-gemini/gemini-cli/issues/21968)), running them in parallel with shared memory ([#18287](https://github.com/google-gemini/gemini-cli/issues/18287)), and exposing their trajectories for review ([#22598](https://github.com/google-gemini/gemini-cli/issues/22598), [#21763](https://github.com/google-gemini/gemini-cli/issues/21763)).
- **Token-frugal, structure-aware code exploration** — The AST-aware EPIC ([#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746), [#22747](https://github.com/google-gemini/gemini-cli/issues/22747)) plus "Tactful Extraction" ([#19561](https://github.com/google-gemini/gemini-cli/issues/19561)) target the ~36.6k-token/turn baseline and large-file context bloat.
- **Persistent, file-based task tracking** — [#18836](https://github.com/google-gemini/gemini-cli/issues/18836) and [#21000](https://github.com/google-gemini/gemini-cli/issues/21000) propose replacing in-context `WriteToDo` with CRUD on a real file to survive context rot and session boundaries.
- **Safer-by-default execution** — [#22672](https://github.com/google-gemini/gemini-cli/issues/22672) (discourage `git reset --force`-style destructiveness) and [#19873](https://github.com/google-gemini/gemini-cli/issues/19873) (OS sandboxing) both argue for guardrails without sacrificing native bash capability.
- **Per-workspace policy and self-awareness** — [#18397](https://github.com/google-gemini/gemini-cli/issues/18397) (workspace-scoped policies instead of global) and [#21432](https://github.com/google-gemini/gemini-cli/issues/21432) (accurate self-description of flags/hotkeys).

## 7. Developer Pain Points

- **Silent or misleading agent outcomes.** Subagents report `GOAL`/`success` when they actually hit `MAX_TURNS` ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323)) or fail outright on Wayland ([#21983](https://github.com/google-gemini/gemini-cli/issues/21983)); `list_directory` reports success while omitting files ([#29713](https://github.com/google-gemini/gemini-cli/issues/29713)). Trust in agent status output is the top recurring complaint.
- **Hangs and interactive-prompt deadlocks.** The generalist agent stalls indefinitely ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)), and scaffolding flows like `create vite app` get stuck at interactive prompts ([#22465](https://github.com/google-gemini/gemini-cli/issues/22465)).
- **Configuration that doesn't take effect.** `settings.json` overrides ignored by the Browser Agent ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267)) and symlinked agent definitions not discovered ([#20079](https://github.com/google-gemini/gemini-cli/issues/20079)).
- **Tool-surface scaling limits.** Hard 400 errors beyond 128/400 tools ([#24246](https://github.com/google-gemini/gemini-cli/issues/24246)) as MCP and subagent tool counts grow.
- **Workspace hygiene and terminal UX.** Models scattering ad-hoc temp scripts across the tree ([#23571](https://github.com/google-gemini/gemini-cli/issues/23571)), inconsistent `\n` escaping ([#22466](https://github.com/google-gemini/gemini-cli/issues/22466)), and resize/streaming flicker ([#21924](https://github.com/google-gemini/gemini-cli/issues/21924), being addressed by [#29629](https://github.com/google-gemini/gemini-cli/pull/29629)).

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI — Community Digest
**Date:** 2026-10-12 · **Source:** [github.com/github/copilot-cli](https://github.com/github/copilot-cli)

---

## 1. Today's Highlights

No new releases, pull requests, or discussion threads landed in the last 24 hours, but issue activity was heavy: 22 issues were updated, with a clear focus on **MCP authentication failures**, **ACP/non-interactive parity gaps**, and **plugin lifecycle problems on Windows**. Several long-running rendering and session-resume bugs were closed out, while a fresh batch of open issues highlights friction in remote sessions (mobile attachments) and agent-loop reliability.

---

## 2. Releases

*No new releases in the last 24 hours.*

---

## 3. Hot Issues

1. **[#5119 [OPEN] Windows: uninstalling an installed plugin fails with "Access is denied" (os error 5)](https://github.com/github/copilot-cli/issues/5119)**
   Freshly filed and already triaged. Uninstalling the `workiq` plugin fails with an OS-level permission error. Matters because it leaves Windows users with no clean way to remove plugins — a lifecycle gap that will block adoption of the plugin ecosystem. No comments yet, so the maintainers haven't responded.

2. **[#5118 [CLOSED] Atlassian Rovo MCP Server connector fails auth probe with HTTP 400, blocking all Atlassian tool calls](https://github.com/github/copilot-cli/issues/5118)**
   The built-in Atlassian Rovo connector fails its authorization probe, and every Jira/Confluence tool call returns `401 Unauthorized`. High impact: it hard-blocks an in-progress agent session. Closed same-day, suggesting a fast triage/fix path or a documentation resolution.

3. **[#5101 [OPEN] `--add-github-mcp-tool issue_write` causes no MCP tools to be available](https://github.com/github/copilot-cli/issues/5101)**
   A single CLI flag appears to silently disable the entire GitHub MCP server toolset instead of narrowing it to `issue_write`. This is a sharp footgun for automation and CI use cases where the flag is scripted. Low comment volume so far, but high severity.

4. **[#5049 [OPEN] Computer Use plugin unavailable in ACP mode despite being enabled in CLI (Windows, 1.0.91)](https://github.com/github/copilot-cli/issues/5049)**
   ACP advertises `/computer` but the bundled plugin and its MCP server never materialize in the session. Representative of a broader pattern: interactive CLI features that don't carry over to ACP/headless mode.

5. **[#5115 [OPEN] ACP: `/rename` reports success but doesn't rename the session](https://github.com/github/copilot-cli/issues/5115)**
   The command is advertised in `available_commands_update` and returns `end_turn` in ~1 ms with a success message, but the rename never happens. Silent no-ops in the ACP protocol are particularly damaging for tooling built on top of it.

6. **[#5116 [OPEN] Intermittent 400 internal server error on response continuation after tool output](https://github.com/github/copilot-cli/issues/5116)**
   Multi-step sessions intermittently die with `Execution failed: 400 internal server error` while submitting a continuation via `previous_response_id`. Flaky mid-session termination is one of the most disruptive failure modes for long agent runs.

7. **[#5114 [OPEN] Copilot CLI doesn't continue when prompted to "continue" after hitting the session limit](https://github.com/github/copilot-cli/issues/5114)**
   After a 429 cap and a five-hour wait, the session still refuses to resume, reporting the earlier cap is "still blocking" work. Users are burning time waiting for limits that don't actually reset for the session in question.

8. **[#4469 [CLOSED] Orphaned `permission.requested` event replays on every session resume](https://github.com/github/copilot-cli/issues/4469)**
   A repeatedly-resumed session replays a directory-access prompt for a command completed 10 days earlier, and the prompt can't be dismissed. A strong example of session state (event log) not being reconciled with completed work. Closed after an extended lifespan (filed August, closed October).

9. **[#3755 [CLOSED] Reasoning/thinking display garbles streamed text with duplicated overlapping chunks](https://github.com/github/copilot-cli/issues/3755)**
   With `showReasoning: true`, streamed reasoning re-emits word boundaries ("from" → "fromply from"). The most-engaged item in this batch (6 comments, 2 👍) and a long-standing terminal-rendering complaint; now closed.

10. **[#5082 [CLOSED] MCP OAuth: Dataverse MCP rejected by RFC 8414 issuer check (`login.windows.net` vs `login.microsoftonline.com`)](https://github.com/github/copilot-cli/issues/5082)**
    A strict issuer-vs-metadata-URL comparison blocks Dataverse/Dynamics 365 MCP auth, both with and without a static `oauthClientId`. Relevant to any enterprise IdP that uses aliased issuer hosts — a compliance-vs-interop tension worth watching.

---

## 4. Key PR Progress

*No pull requests were updated in the last 24 hours.*

---

## 5. Hot Discussions

*No discussion data was provided for this period.*

---

## 6. Feature Request Trends

Distilled from the 22 updated issues:

- **Plugin lifecycle management** — install/uninstall reliability on Windows ([#5119](https://github.com/github/copilot-cli/issues/5119)) and automatic pruning of stale installed versions (keep only the latest three, [#5077](https://github.com/github/copilot-cli/issues/5077)).
- **ACP / non-interactive parity** — bundled plugins and session commands must work the same in `--acp --stdio` as they do interactively ([#5049](https://github.com/github/copilot-cli/issues/5049), [#5115](https://github.com/github/copilot-cli/issues/5115)).
- **Remote & mobile session capability** — attachments (files/images) are supported on desktop/web but have no entry point in the GitHub Mobile composer ([#5117](https://github.com/github/copilot-cli/issues/5117)).
- **Ignore-file semantics** — explicit `.copilotignore` behavior, especially for nested ignore files ([#3785](https://github.com/github/copilot-cli/issues/3785)).
- **Terminal UX polish** — clearer notification/tab headers distinguishing "working" from "waiting for input" ([#5078](https://github.com/github/copilot-cli/issues/5078)), and continued rendering fixes for reasoning streams and shortcuts.
- **Session continuity** — reliable resume after rate limits, and no replay of stale permission events ([#4469](https://github.com/github/copilot-cli/issues/4469), [#5114](https://github.com/github/copilot-cli/issues/5114)).

---

## 7. Developer Pain Points

- **MCP auth and OAuth is the top reliability risk.** RFC 8414 issuer mismatches ([#5082](https://github.com/github/copilot-cli/issues/5082)), Atlassian Rovo probe failures ([#5118](https://github.com/github/copilot-cli/issues/5118)), and a single flag wiping all GitHub MCP tools ([#5101](https://github.com/github/copilot-cli/issues/5101)) all point to a fragile integration layer that silently degrades agent capability.
- **Transient server errors interrupt long agent runs.** Repeated "server error, retrying" spam during subagent/fleet execution ([#4226](https://github.com/github/copilot-cli/issues/4226)) and intermittent 400s on `previous_response_id` continuations ([#5116](https://github.com/github/copilot-cli/issues/5116)) make multi-step work unreliable.
- **Session state is not reconciled on resume.** Stale permission events replay for completed commands ([#4469](https://github.com/github/copilot-cli/issues/4469)) and rate-limited sessions fail to continue after the cap window ([#5114](https://github.com/github/copilot-cli/issues/5114)).
- **Configuration is not always honored.** Subagents ignoring model-inheritance settings ([#4287](https://github.com/github/copilot-cli/issues/4287)), hooks silently dropped after `extensions_reload` ([#3167](https://github.com/github/copilot-cli/issues/3167)), and `"bell": true` having no effect ([#3573](https://github.com/github/copilot-cli/issues/3573)) all erode trust in the settings surface.
- **Terminal input/rendering edge cases persist.** Right-click copy also pasting into the input ([#4126](https://github.com/github/copilot-cli/issues/4126)), Home/End in `ask_user` scrolling the timeline ([#5113](https://github.com/github/copilot-cli/issues/5113)), and garbled reasoning streams ([#3755](https://github.com/github/copilot-cli/issues/3755)) show a steady drip of keyboard/render bugs.
- **Windows-specific plugin friction.** Access-denied uninstall errors ([#5119](https://github.com/github/copilot-cli/issues/5119)) and ACP plugin unavailability on Windows ([#5049](https://github.com/github/copilot-cli/issues/5049)) suggest platform-specific packaging/permission handling needs attention.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-10-12

## Today's Highlights
No new releases shipped in the last 24h, while 50 issues and 50 PRs were updated. The community’s attention is on free-tier/permission inconsistencies, provider-settings mismatches, and reliability gaps around service restarts, migrations, and silent failures. PR activity shows momentum in generative UI, session/tab management, MCP connection hygiene, and TUI readability.

## Hot Issues
1. **#50155 — DeepSeek V4 Flash requires Global regions but Privacy setting is missing** [OPEN, 13 comments, 10 👍]  
   Paid OpenCode Go users hit an upstream error telling them to select Global regions, but the setting is reportedly unavailable. High reaction count makes this a visible subscription/provider-config blocker.  
   [anomalyco/opencode#50155](https://github.com/anomalyco/opencode/issues/50155)

2. **#42950 — Intermittent socket disconnects cause silent drop (no UI error)** [CLOSED, 11 comments, 1 👍]  
   `big-pickle` streams can drop with `AI_APICallError` and repeated “Aborted” logs while the UI does not surface the failure. Closed, but it highlights expectations around stream reliability.  
   [anomalyco/opencode#42950](https://github.com/anomalyco/opencode/issues/42950)

3. **#50627 — Deny `shell *` on custom agent breaks free tier** [OPEN, 9 comments]  
   Adding a shell-deny permission on a custom primary agent makes free-tier models fail with “can only be used from within OpenCode,” even inside the TUI. This points to confusing permission-policy interactions.  
   [anomalyco/opencode#50627](https://github.com/anomalyco/opencode/issues/50627)

4. **#43596 — Configurable retry policy: expose maxRetries / initialDelay / backoffFactor / maxDelay** [CLOSED, 8 comments, 13 👍]  
   Users want configurable retry behavior because hardcoded retries abort turns while provider errors are still transient. Strong 👍 count shows this is a widely desired control.  
   [anomalyco/opencode#43596](https://github.com/anomalyco/opencode/issues/43596)

5. **#54267 — Service relaunch fails with “Managed service port already in use”** [OPEN, 7 comments]  
   Reproduced issue where a previous background service has not exited, causing `opencode` relaunch failures on the fixed managed-service port. Service lifecycle robustness remains a practical pain point.  
   [anomalyco/opencode#54267](https://github.com/anomalyco/opencode/issues/54267)

6. **#47742 — Support a Shared `.agents/` Directory and Manifest-Based Discovery** [OPEN, 5 comments, 2 👍]  
   Proposal to reduce repo sprawl from tool-specific agent directories and improve interoperability. Signals growing demand for cross-agent configuration standards.  
   [anomalyco/opencode#47742](https://github.com/anomalyco/opencode/issues/47742)

7. **#54477 — GitHub action: permission resolving to “ask” hangs forever** [CLOSED, 4 comments]  
   The `anomalyco/opencode/github` action blocks indefinitely because the handler subscribes to no permission event. Important for CI/automation users, though now closed.  
   [anomalyco/opencode#54477](https://github.com/anomalyco/opencode/issues/54477)

8. **#54744 — Free-tier 403 rejects restricted subagents even with bash allowed** [OPEN, 3 comments]  
   A new report that custom subagents with restricted tools fail on free-tier models while built-in agents succeed. Reinforces the free-tier/permission confusion theme.  
   [anomalyco/opencode#54744](https://github.com/anomalyco/opencode/issues/54744)

9. **#54733 — TUI hangs indefinitely with spinner on deprecated model** [OPEN, 2 comments]  
   When a model is deprecated server-side, the TUI spins forever and `esc` does not interrupt, while `opencode run` reports the problem correctly. Error surfacing in the TUI needs improvement.  
   [anomalyco/opencode#54733](https://github.com/anomalyco/opencode/issues/54733)

10. **#54736 — V1 migration aborts on legacy message ID failing `Session.Message.ID` schema** [OPEN, 2 comments]  
    Upgrading from V1 to v2.0.27 can fail because legacy message IDs do not match the new `msg_` schema. A serious upgrade blocker for existing users.  
    [anomalyco/opencode#54736](https://github.com/anomalyco/opencode/issues/54736)

## Key PR Progress
1. **#54747 — feat(app): add generative UI to the GUI**  
   Agents can emit OpenUI Lang via `genui_render`, rendered interactively while the model is still streaming; button clicks and form submissions return as user messages. A significant GUI capability expansion.  
   [anomalyco/opencode#54747](https://github.com/anomalyco/opencode/pull/54747)

2. **#54749 — feat(app,tui): open sessions created elsewhere as background tabs**  
   Opt-in setting to surface sessions created by `opencode run`, the SDK, codemode, or other clients instead of requiring manual search. Duplicate/closed companion: #54746.  
   [anomalyco/opencode#54749](https://github.com/anomalyco/opencode/pull/54749)

3. **#54748 — refactor(app): derive unread state from server viewed watermarks**  
   Aligns desktop unread state with server-side session tracking already used by the TUI, removing desktop’s local notification-list behavior.  
   [anomalyco/opencode#54748](https://github.com/anomalyco/opencode/pull/54748)

4. **#52338 — feat(desktop): integrate native window controls on Linux**  
   Adds native window-controls overlay on Linux using Electron 43, closing #36225. Brings Linux desktop parity with earlier Windows overlay work.  
   [anomalyco/opencode#52338](https://github.com/anomalyco/opencode/pull/52338)

5. **#53925 — fix(server): avoid booting cold locations for pending prompt reads**  
   Avoids acquiring a session’s full Location graph when no prompts exist, using a scoped cached check instead. Closes #53918.  
   [anomalyco/opencode#53925](https://github.com/anomalyco/opencode/pull/53925)

6. **#53050 — fix(app): reserve chat request slots during MCP discovery**  
   Includes `/api/command`, `/api/mcp`, and nested routes in the slow-request quota so MCP discovery cannot starve chat requests.  
   [anomalyco/opencode#53050](https://github.com/anomalyco/opencode/pull/53050)

7. **#53048 — fix(app): retry failed session metadata without reloading**  
   Passes the session error boundary reset callback to the error page so session load failures can recover without a full reload.  
   [anomalyco/opencode#53048](https://github.com/anomalyco/opencode/pull/53048)

8. **#53046 — fix(mcp): reclaim discovery-only connections**  
   Starts MCP lazily and releases connections opened only for command/tool/resource discovery. Fixes #51003 for discovery-only MCP connections.  
   [anomalyco/opencode#53046](https://github.com/anomalyco/opencode/pull/53046)

9. **#54707 — feat(tui): show plugin and MCP tool input as a readable block**  
   Improves TUI inspectability for plugin/MCP tool calls. Notes that v1 is now the maintenance branch; a v2 version is planned after migration.  
   [anomalyco/opencode#54707](https://github.com/anomalyco/opencode/pull/54707)

10. **#54705 — feat(tui): color heredocs in bash commands in their own language**  
    Builds on #54704’s bash-command coloring, adding language-aware heredoc highlighting. Part of a broader TUI readability push.  
    [anomalyco/opencode#54705](https://github.com/anomalyco/opencode/pull/54705)

## Feature Request Trends
- **Permission/policy clarity:** free-tier eligibility, shell-deny behavior, and subagent tool restrictions are recurring themes (#50627, #54744).
- **Provider/settings UX:** missing Global privacy setting for DeepSeek V4 Flash (#50155), interactive custom provider registration in `/connect` (#49091), and empty model picker issues (#54092).
- **Agent-config interoperability:** shared `.agents/` directory and manifest-based discovery (#47742).
- **TUI/CLI ergonomics:** dedicated file-attachment button (#54743), close-button placement (#54438), readable plugin/MCP tool input (#54703), and bash/heredoc syntax coloring (#54700, #54701).
- **Reliability/operability:** configurable retry policy (#43596), service relaunch/port handling (#54267), migration/upgrade robustness (#54736), and clearer errors for deprecated models (#54733).
- **Extensibility/integration:** Reverse Engineer Anything (#54587), generative UI (#54747), and native Linux window controls (#52338).

## Developer Pain Points
- **Free-tier permission gate is inconsistent and opaque:** valid in-app requests can fail with “can only be used from within OpenCode,” and custom subagents may 403 despite bash being allowed (#50627, #54744).
- **Silent failures and hangs:** socket disconnects without UI errors (#42950), deprecated-model spinner with no error (#54733), and GitHub Action permission `ask` hanging indefinitely (#54477).
- **Upgrade/migration fragility:** V1→V2 migration schema validation can abort on legacy message IDs (#54736), and update-to-2.0 failures are reported (#54469).
- **Managed service lifecycle problems:** relaunch can fail when a previous background service has not exited (#54267).
- **Provider/model configuration mismatches:** missing Global privacy setting for DeepSeek V4 Flash (#50155), empty model picker (#54092), and incorrect Copilot URL (#45302).
- **Retry/backoff is not configurable:** hardcoded retry limits abort transient provider errors too early (#43596).
- **TUI/desktop polish gaps:** Thai combining-mark width handling (#49348), close-button placement (#54438), file attachment flow (#54743), plugin/MCP input readability (#54703), and bash coloring (#54700/#54701).
- **MCP/request resource pressure:** MCP discovery can consume slow-request slots and leave discovery-only connections open (#53050, #53046).

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi Community Digest — 2026-10-12

## Today's Highlights
No new releases landed in the last 24 hours, but issue traffic was heavy: 50 issues were updated, dominated by OpenAI/ChatGPT authentication and quota-state bugs plus a cluster of 1.1.0 regressions in codemode and the TUI. On the PR side, the maintainers closed out several long-standing extension-lifecycle fixes — notably `before_agent_start` now fires for runs started by custom messages ([#10739](https://github.com/earendil-works/pi/pull/10739)) — and shipped the OpenRouter per-key model filtering requested in [#10353](https://github.com/earendil-works/pi/issues/10353). The only open PR is [#10795](https://github.com/earendil-works/pi/pull/10795), an Anthropic tool-result image fix.

## Hot Issues

1. **[#4945](https://github.com/earendil-works/pi/issues/4945) — openai-codex Connection Reliability Issues** (OPEN, inprogress, 82 comments, 35 👍)
   The community's hottest thread by a wide margin. `gpt-5.5` over `openai-codex` intermittently hangs the interactive TUI on `Working...` with no streamed text, no tool call, and no error; the only escape is `Esc`, which logs an aborted turn. High like count signals this is widely reproduced, not an edge case.

2. **[#10480](https://github.com/earendil-works/pi/issues/10480) — Direct openai connection not recognising manual usage limit reset** (OPEN, 18 comments)
   Banked ChatGPT Pro resets are ignored on the direct OpenAI connection, leaving users locked out until they `/logout` and re-login via `openai-codex`. A painful billing/entitlement correctness bug with a known workaround.

3. **[#10605](https://github.com/earendil-works/pi/issues/10605) — ChatGPT/OpenAI OAuth 403 issue** (OPEN, 10 comments, 1 👍)
   Plus-tier users hit `subscription_sharing_user_not_eligible` after logout/login. Same family as #10480 and likely part of a broader credential/entitlement state problem.

4. **[#7553](https://github.com/earendil-works/pi/issues/7553) — Configurable thinking level/model for compaction** (OPEN, inprogress/to-discuss, 10 comments, 3 👍)
   Compaction unconditionally inherits the session thinking level, so summarization burns the same reasoning budget as normal turns. A well-scoped, repeatedly requested knob for users running auto-compaction on reasoning models.

5. **[#10267](https://github.com/earendil-works/pi/issues/10267) — Prompt text from `before_agent_start` dropped on non-user-prompt runs** (CLOSED, p0, 9 comments, 4 👍)
   Marked P0 because the drop forces re-billing of the whole prompt on background-task notifications, plan-mode continues, retries, and resumes. Fixed by [#10739](https://github.com/earendil-works/pi/pull/10739).

6. **[#5581](https://github.com/earendil-works/pi/issues/5581) — `pi.sendMessage()` with `triggerTurn: true` bypasses `before_agent_start`** (CLOSED, 9 comments, 4 👍)
   The root cause behind a family of extension bugs: the agent loop was kicked via `_runAgentPrompt` directly, skipping `emitBeforeAgentStart` and silently changing the system prompt mid-run.

7. **[#9602](https://github.com/earendil-works/pi/issues/9602) — Compaction can overflow by including thinking messages omitted from earlier requests** (OPEN, 9 comments)
   With local Qwen3.8 via llama.cpp, thinking content excluded from prior requests still counts toward compaction, causing overflow. Another entry in the growing compaction-correctness backlog.

8. **[#10645](https://github.com/earendil-works/pi/issues/10645) — `resizeImage` resolves null in compiled Bun executables** (OPEN, inprogress, 6 comments)
   Standalone binaries from `v0.87.1`/`v1.0.4` silently drop all image attachments because `resizeImage` fails under Bun compilation. A functional regression for RPC integrators, with clean reproduction via a fake gateway.

9. **[#10821](https://github.com/earendil-works/pi/issues/10821) — Terminal resize freezes the UI with long sessions** (CLOSED, 2 comments)
   Every resize frame synchronously re-lays out the entire transcript; severely degrades on Windows Terminal/VS Code terminals. A 1.1.0-era TUI performance regression.

10. **[#10678](https://github.com/earendil-works/pi/issues/10678) — codemode `joinAdjacentText` is quadratic** (CLOSED, 3 comments, 1 👍)
    Introduced in 1.1.0: scripts calling `text()` tens of thousands of times block the host thread because joining runs synchronously with quadratic cost. Fast turnaround on the fix, but a notable release-quality miss.

## Key PR Progress

1. **[#10739](https://github.com/earendil-works/pi/pull/10739) — emit `before_agent_start` for runs started by custom messages** (CLOSED)
   Closes out [#5581](https://github.com/earendil-works/pi/issues/5581) and [#10267](https://github.com/earendil-works/pi/issues/10267): next-turn refresh no longer patches away handler-added prompt sections, preserving system-prompt stability and provider prompt caching.

2. **[#10672](https://github.com/earendil-works/pi/pull/10672) — list only the OpenRouter models a key may use** (CLOSED)
   Combines the catalog with `GET /models/user`, keeping context size, max output, and pricing from the authenticated response. Implements [#10353](https://github.com/earendil-works/pi/issues/10353).

3. **[#10820](https://github.com/earendil-works/pi/pull/10820) — send `x-opencode-session` by endpoint, not provider id** (CLOSED)
   Generalizes [#9326](https://github.com/earendil-works/pi/pull/9326): any provider routed to OpenCode zen endpoints now gets the per-conversation routing header, avoiding `400 MissingSessionID`.

4. **[#10819](https://github.com/earendil-works/pi/pull/10819) — use `registerToolRenderer` in the minimal-mode example** (CLOSED)
   Docs fix with real consequences: re-registering built-ins with `pi.registerTool({...})` replaced `execute` entirely, silently breaking extensions that reroute built-in tool execution.

5. **[#10813](https://github.com/earendil-works/pi/pull/10813) — preserve client-loaded Anthropic tool references** (CLOSED)
   Keeps tool references supplied by the client intact through the Anthropic provider path.

6. **[#10774](https://github.com/earendil-works/pi/pull/10774) — enable kitty images in herdr** (CLOSED)
   Reverts an unintended 1.1.0 regression where `#10573` returned `images: null` for herdr panes despite herdr supporting the kitty graphics protocol.

7. **[#10810](https://github.com/earendil-works/pi/pull/10810) — support override `colorFn` options in `CompactionStatusIndicator`** (CLOSED)
   Fixes [#10809](https://github.com/earendil-works/pi/issues/10809); aligns the compaction indicator with `WorkingIndicator` for themes that don't set `borderColor`.

8. **[#10805](https://github.com/earendil-works/pi/pull/10805) — support extension session cwd** (CLOSED)
   Adds `cwd` to `ctx.newSession()` and `cwdOverride` to `ctx.switchSession()`, completing the git-worktree workflow extensions could already script via `pi.exec`.

9. **[#10751](https://github.com/earendil-works/pi/pull/10751) — use pi.dev configuration schemas** (CLOSED)
   Makes pi.dev schema endpoints the canonical `$id` values for generated settings, models, keybindings, and theme schemas, and adopts the published theme schema in built-ins/examples.

10. **[#10795](https://github.com/earendil-works/pi/pull/10795) — drop images from Anthropic error tool results** (OPEN)
    The only PR still open; fixes [#10794](https://github.com/earendil-works/pi/issues/10794) by stripping images from Anthropic error tool results, with before/after session traces attached.

## Hot Discussions

**Show and tell**
- **[#10808](https://github.com/earendil-works/pi/discussions/10808) — Pippa, a native Mac interface for everyday Pi tasks** (1 👍)
  A free, MIT-licensed Swift macOS app that talks to Pi over `pi --mode rpc`, letting non-terminal users drop a letter or folder onto a floating pill and pick a task. Local models run through llama.cpp, with Mac integrations layered on top; the author is recruiting testers.

## Feature Request Trends

- **Compaction control and correctness**: the single strongest theme across the window. Requests span a dedicated thinking level/model for compaction ([#7553](https://github.com/earendil-works/pi/issues/7553)), overflow from thinking messages ([#9602](https://github.com/earendil-works/pi/issues/9602)), abort semantics during pre-prompt compaction ([#10289](https://github.com/earendil-works/pi/issues/10289)), and transport settings not applying to summary requests ([#10781](https://github.com/earendil-works/pi/issues/10781)).
- **Provider entitlement and model discovery**: users want Pi's UI to reflect real account state rather than the static catalog — per-key OpenRouter model filtering ([#10353](https://github.com/earendil-works/pi/issues/10353)), honoring ChatGPT usage resets ([#10480](https://github.com/earendil-works/pi/issues/10480)), and warning instead of silently falling back when a saved default model disappears ([#10812](https://github.com/earendil-works/pi/issues/10812)).
- **Extension lifecycle guarantees**: consistent `before_agent_start` semantics ([#5581](https://github.com/earendil-works/pi/issues/5581), [#10267](https://github.com/earendil-works/pi/issues/10267), [#10639](https://github.com/earendil-works/pi/issues/10639)), safe renderer overrides ([#10819](https://github.com/earendil-works/pi/pull/10819)), and session cwd control for worktree flows ([#10805](https://github.com/earendil-works/pi/pull/10805)).
- **Tool/MCP declaration fidelity**: prompt sections must match the tools actually exposed — MCP servers removed by `--no-tools`/`--exclude-tools` are still advertised ([#10635](https://github.com/earendil-works/pi/issues/10635)), codemode drops input constraints like `minimum`/`maximum`/`default` ([#10707](https://github.com/earendil-works/pi/issues/10707)), and Anthropic `thinking.display: "updates"` is requested ([#10804](https://github.com/earendil-works/pi/issues/10804)).
- **Headless/unattended agent robustness**: explicit continuation paths for max-token stops ([#10799](https://github.com/earendil-works/pi/issues/10799)), generic classifier model support on custom providers ([#10791](https://github.com/earendil-works/pi/issues/10791)), and cheaper image handling via ChatGPT `file_id` references ([#8617](https://github.com/earendil-works/pi/issues/8617)).

## Developer Pain Points

- **Provider connectivity and account state are the top recurring frustration.** Silent TUI hangs on `openai-codex` ([#4945](https://github.com/earendil-works/pi/issues/4945)) and 403/quota-state mismatches ([#10480](https://github.com/earendil-works/pi/issues/10480), [#10605](https://github.com/earendil-works/pi/issues/10605)) force logins, workarounds, and aborted turns.
- **Silent failures over loud errors.** Dropped RPC prompts acknowledged with `success: true` ([#10606](https://github.com/earendil-works/pi/issues/10606)), skipped `before_agent_start` calls ([#10639](https://github.com/earendil-works/pi/issues/10639)), ignored default models ([#10812](https://github.com/earendil-works/pi/issues/10812)), and output-limit stops that settle without notice ([#10799](https://github.com/earendil-works/pi/issues/10799)) all cost debugging time because nothing surfaces.
- **1.1.0 regression cluster.** Quadratic codemode text joining ([#10678](https://github.com/earendil-works/pi/issues/10678)), synchronous full-transcript relayout on resize ([#10821](https://github.com/earendil-works/pi/issues/10821)), and broken herdr inline images ([#10774](https://github.com/earendil-works/pi/pull/10774)) suggest release-testing gaps around TUI and codemode paths.
- **Prompt-cache and cost regressions from lifecycle bugs.** Re-billing entire prompts when contributed prompt text is dropped ([#10267](https://github.com/earendil-works/pi/issues/10267)) and mid-run system-prompt changes that break provider caches ([#10739](https://github.com/earendil-works/pi/pull/10739)) directly hit users' wallets.
- **Platform-specific sharp edges.** Windows auth lock acquisition failing on transient `EPERM` ([#10801](https://github.com/earendil-works/pi/issues/10801)), orphaned Playwright Chrome processes after bash timeouts on macOS ([#10797](https://github.com/earendil-works/pi/issues/10797)), mouse-wheel target confusion under Windows + Zellij ([#9656](https://github.com/earendil-works/pi/issues/9656)), and broken image attachments in compiled Bun binaries ([#10645](https://github.com/earendil-works/pi/issues/10645)) keep platform coverage on the critical path.

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-10-12

Data source: [github.com/QwenLM/qwen-code](https://github.com/QwenLM/qwen-code)

---

## 1. Today's Highlights

Release activity centers on the **v0.25.1** line, with matching SDK (TypeScript v0.1.19) and Desktop (v0.25.1) artifacts. The dominant engineering theme remains the **Managed Agent architecture** (durable Sessions, writer fencing/takeover, team messaging, L4 Shell/MCP lifecycle), which continues to drive the majority of high-traffic issues and PRs. Meanwhile, a cluster of new reports around **UTF-8 BOM config files silently failing** and **CI/release-tooling reliability** (Flyway ordering, CVE audit, duplicated release notes) highlights growing friction in configuration robustness and automation.

---

## 2. Releases

| Release | Notes |
|---|---|
| [v0.25.1](https://github.com/QwenLM/qwen-code/releases) | Stable CLI release. Auto-generated notes; the only substantive feature line references restoring a text-only connection and validated session/workspace route after Android Activity recreation. Full change list is not populated in the notes. |
| [v0.25.1-nightly.20261011.01d3e6d81a](https://github.com/QwenLM/qwen-code/releases) | Nightly carrying `fix(managed-agent): connector and broker robustness` (#13330) plus additional managed-agent work. |
| [sdk-typescript-v0.1.19](https://github.com/QwenLM/qwen-code/releases) | Bundles CLI version 0.25.1 (built from the same source branch/ref as the SDK). Release body also references bundled CLI 0.25.0, suggesting stale/duplicated note generation. |
| [desktop-v0.25.1](https://github.com/QwenLM/qwen-code/releases) | Desktop release includes `fix(serve): preserve session creation failure diagnostics` (#12331) and `feat(sdk-java): Add managed runtime` work; ships `.deb` (amd64/arm64) and AppImage artifacts. |

> ⚠️ Note: Release notes for this window appear partially duplicative — directly related to open issue [#13925](https://github.com/QwenLM/qwen-code/issues/13925).

---

## 3. Hot Issues

1. **[#12380 — Proposal(serve): Define Managed Agent dual-path architecture and staged delivery](https://github.com/QwenLM/qwen-code/issues/12380)** *(56 comments, OPEN)*
   The umbrella design for keeping the TypeScript agent loop while decoupling inference from tool-environment provisioning, with durable Session ownership, Workspace bindings, recoverable tool executions, and a stable Web Shell surface. It is the single most-discussed issue in the repo and the parent of most managed-agent workstreams (Stages G, H3, H4).

2. **[#13078 — Daily dependency CVE audit failed](https://github.com/QwenLM/qwen-code/issues/13078)** *(18 comments, OPEN)*
   A scheduled security audit job is failing repeatedly, with maintainers investigating whether it is a genuine high-severity finding or npm audit endpoint instability. High community attention because it gates supply-chain hygiene.

3. **[#12952 — Stage G: authoritative Session history, writer fencing and takeover](https://github.com/QwenLM/qwen-code/issues/12952)** *(8 comments, OPEN)*
   Defines the slices and exit checks for externalizing Session history/checkpoints and proving writer fencing before removing owner affinity. This is the architectural hinge for multi-writer/hosted sessions.

4. **[#13680 — [P1] Desktop Linux vendored ripgrep rewritten post-link, SIGSEGVs on startup probe](https://github.com/QwenLM/qwen-code/issues/13680)** *(4 comments, OPEN)*
   The `x64-linux` `rg` binary has an injected `DT_RUNPATH $ORIGIN` and moved ELF program headers, crashing every startup probe. The only P1 in this window — a hard blocker for Linux desktop users.

5. **[#13807 — "ERROR 500 An unsupported generation guide was used" with macOS Foundation Models as Fast Model](https://github.com/QwenLM/qwen-code/issues/13807)** *(6 comments, OPEN)*
   Works for tool summaries but fails on the fast-model path when using `fm serve` via an `openai:system…` provider string. Points at provider/routing edge cases for local OpenAI-compatible endpoints.

6. **[#13924 — Publish Qwen Code Desktop on Flathub with a working update channel](https://github.com/QwenLM/qwen-code/issues/13924)** *(5 comments, OPEN)*
   Requests Flathub distribution so Linux app stores (e.g., Mint Software Manager) can list the app and provide updates. Builds on the already-shipping `.deb`/AppImage artifacts.

7. **[#13875 — [H4f] Parent close waits for running child Turn instead of stopping it](https://github.com/QwenLM/qwen-code/issues/13875)** *(4 comments, OPEN)*
   Cancel-then-close leaves child Sessions executing. A correctness gap in multi-agent lifecycle semantics that directly affects cancellation guarantees.

8. **[#13922 — Flyway migrations numbered below main's newest pass both version checks](https://github.com/QwenLM/qwen-code/issues/13922)** *(4 comments, OPEN)*
   Both uniqueness gates only check for duplicates, not ordering, so upgraded databases can refuse to start. Notable because it allows a broken migration to merge green.

9. **[#13944 / #13943 — UTF-8 BOM config files silently skipped](https://github.com/QwenLM/qwen-code/issues/13944)** *(3 comments each, OPEN)*
   A `.toml` custom command file with a BOM is dropped (`Unknown character "65279"`), and a BOM-prefixed `.lsp.json` causes the entire file to be discarded with only a debug log — so no LSP servers start and users see no error. Related: [#13710](https://github.com/QwenLM/qwen-code/issues/13710) closed for Claude MCP import.

10. **[#13925 — Release notes duplicate PRs released in previous releases](https://github.com/QwenLM/qwen-code/issues/13925)** *(3 comments, OPEN)*
    Desktop release notes are accreting entries instead of scoping to the current release — a visible trust issue for downstream users tracking what actually shipped.

*Also worth tracking:* [#13951](https://github.com/QwenLM/qwen-code/issues/13951) (CI checkout `pack has 1 unresolved delta`), [#13920](https://github.com/QwenLM/qwen-code/issues/13920) (CJK teammate names collapse in `sanitizeName`), [#13942](https://github.com/QwenLM/qwen-code/issues/13942) (multi-provider, same model ID switching friction), [#13930](https://github.com/QwenLM/qwen-code/issues/13930) (H3 held-Runtime drain follow-ups).

---

## 4. Key PR Progress

1. **[#13940 — feat(managed-agent): Java mirror of managed-event-envelope contract (MQ2a)](https://github.com/QwenLM/qwen-code/pull/13940)**
   First sub-slice of the EventTransport MQ2 phase: a Java mirror of the frozen `managed-event-envelope/1` contract plus a fixture consumer proving cross-language parity.

2. **[#13950 — feat(mods): Add tool interception and permission checks (PR3a)](https://github.com/QwenLM/qwen-code/pull/13950)**
   Adds `tool.call` / `tool.check` hooks to opt-in Mods: rewrite arguments, refuse calls, return synthetic JSON, and inspect/replace native tool results — with every `next` re-entering native validation, hooks, and permissions.

3. **[#13952 — feat(web-shell): Bridge trajectory history and live lifecycle state](https://github.com/QwenLM/qwen-code/pull/13952)**
   Connects historical trajectory windows to bounded request/tool lifecycle state via the session subscription; reconnects preserve page, selection, filters, and scroll position.

4. **[#13878 — feat(web-shell): Add workspace file browser and previews](https://github.com/QwenLM/qwen-code/pull/13878)**
   A Files action in the right panel with full-width workspace tree, Markdown/HTML/image rendering, read-only source inspection, and resizable/removable tree.

5. **[#13957 — fix(cli): Preserve Markdown in height-bounded tool results](https://github.com/QwenLM/qwen-code/pull/13957)**
   Restores Markdown formatting in completed CLI tool results while keeping terminal-height limits, truncation cues, and expansion — with height accounting for wrapping, list markers, code gutters, and tables.

6. **[#13777 — feat(managed-agent): Recover Hosted G3 parked turns](https://github.com/QwenLM/qwen-code/pull/13777)**
   Implements recovery for G3 parked turns on the Hosted stack, a key durability step toward Stage G exit criteria.

7. **[#13931 — feat(managed-agent): L4 Shell and MCP session lifecycle](https://github.com/QwenLM/qwen-code/pull/13931)**
   Adds durable close/delete for `hosted-workspace-shell/1` and `hosted-workspace-mcp/1`, settling business effects and SessionEnd/SessionDelete hooks exactly once before detaching the writer.

8. **[#13955 — feat(managed-agent): Member-to-member messages ride the session_command channel (H4e-b3c)](https://github.com/QwenLM/qwen-code/pull/13955)**
   Team members can message each other end-to-end over a durable command channel with no model turn spinning up on the lead. Companion: [#13928](https://github.com/QwenLM/qwen-code/pull/13928) (member board tools).

9. **[#13815 — fix(managed-agent): Numeric parity and family-list hoisting with end-to-end admission and wedge witnesses](https://github.com/QwenLM/qwen-code/pull/13815)**
   Fixes the capture-publication reserve refusing background Shell/Monitor payloads, and adds end-to-end admission tests plus wedge witnesses after review churn. ([#13918](https://github.com/QwenLM/qwen-code/pull/13918) addresses the related tenant-wide InnoDB lock blast radius.)

10. **[#13954 — fix(ci): Enforce Flyway migration ordering](https://github.com/QwenLM/qwen-code/pull/13954)**
    Both Flyway gates now reject newly added versions that are not strictly greater than the highest published version across scanned modules and SQL/Java locations — directly closing the gap from [#13922](https://github.com/QwenLM/qwen-code/issues/13922).

*Also notable:* [#13654](https://github.com/QwenLM/qwen-code/pull/13654) (async tool-publication verification), [#13642](https://github.com/QwenLM/qwen-code/pull/13642) (bounded JDBC history retention), [#13929](https://github.com/QwenLM/qwen-code/pull/13929) (Monitor auto-stop exit evidence), [#13938](https://github.com/QwenLM/qwen-code/pull/13938) (closed — Web Shell voice feedback localization), [#13335](https://github.com/QwenLM/qwen-code/pull/13335) (closed — config/API surface hygiene).

---

## 5. Hot Discussions

No discussion data was provided for this window. Section omitted.

---

## 6. Feature Request Trends

- **Managed Agent / multi-agent platform (dominant):** durable Session history and ownership, writer fencing and takeover, workspace bindings, recoverable/verified tool executions, parent-child lifecycle correctness, team member-to-member messaging and shared boards, and durable Shell/MCP session lifecycle. See [#12380](https://github.com/QwenLM/qwen-code/issues/12380), [#12952](https://github.com/QwenLM/qwen-code/issues/12952), [#13875](https://github.com/QwenLM/qwen-code/issues/13875), [#13271](https://github.com/QwenLM/qwen-code/issues/13271).
- **Linux / platform distribution:** Flathub listing with a working update channel, alongside existing `.deb` and AppImage targets. See [#13924](https://github.com/QwenLM/qwen-code/issues/13924).
- **Web Shell ergonomics:** workspace/project pinning in the sidebar ([#12444](https://github.com/QwenLM/qwen-code/issues/12444), with duplicate [#13888](https://github.com/QwenLM/qwen-code/issues/13888) already closed), file browsing and previews, localized and dismissible voice feedback.
- **Model provider flexibility:** multiple providers serving the same model ID with distinguishable names and quick switching ([#13942](https://github.com/QwenLM/qwen-code/issues/13942)); documented local/OpenAI-compatible providers such as Rapid-MLX ([#13934](https://github.com/QwenLM/qwen-code/issues/13934)); macOS Foundation Models as a fast model ([#13807](https://github.com/QwenLM/qwen-code/issues/13807)).
- **Configuration robustness:** graceful handling of UTF-8 BOM and other encoding quirks across TOML, JSON, LSP, and imported Claude MCP configs ([#13944](https://github.com/QwenLM/qwen-code/issues/13944), [#13943](https://github.com/QwenLM/qwen-code/issues/13943), [#13710](https://github.com/QwenLM/qwen-code/issues/13710)).
- **CI/CD and release tooling reliability:** CVE audit stability, migration ordering enforcement, release-note deduplication, and more accurate review-automation parsing ([#13078](https://github.com/QwenLM/qwen-code/issues/13078), [#13922](https://github.com/QwenLM/qwen-code/issues/13922), [#13925](https://github.com/QwenLM/qwen-code/issues/13925), [#13735](https://github.com/QwenLM/qwen-code/issues/13735)).

---

## 7. Developer Pain Points

- **Silent failures on malformed config.** BOM-prefixed `.toml`, `.lsp.json`, and imported Claude configs are discarded with only debug-level logging, producing confusing symptoms (missing slash commands, "No LSP servers configured," no diagnostics). Multiple independent reporters in one window suggest an encoding-hardening sweep is warranted.
- **CI gates that pass when they shouldn't.** The Flyway checks permitted out-of-order migrations, the dependency CVE audit job is chronically failing, and integration lanes hit Git-level errors (`pack has 1 unresolved delta`) — eroding confidence in green builds.
- **Release-notes accuracy.** Duplicated PR entries across Desktop releases make it hard for users to know what actually shipped; the v0.25.1 notes are similarly under-populated.
- **Lifecycle correctness in multi-agent flows.** Parent close that waits rather than stops child turns, and recovery-blocked sessions wedging unrelated sessions via tenant-wide locks, are high-impact reliability concerns.
- **Internationalization edge cases.** CJK teammate names collapsing to empty in `sanitizeName` forces model-invented pinyin fallbacks — a correctness issue for non-English users adopting Agent Teams.
- **Token/context accounting gaps.** Compaction sizing against an inferred window instead of the server-reported ceiling ([#13432](https://github.com/QwenLM/qwen-code/issues/13432), now closed) reflects recurring sensitivity around context-limit behavior.
- **Multi-provider model switching friction.** Users must change providers in the CLI to reach the same model served by different vendors; the absence of per-provider aliasing is a repeated usability complaint.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*