# AI CLI Tools Community Digest 2026-10-16

> Generated: 2026-10-16 00:40 UTC | Tools covered: 7

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

# Cross-Tool Comparison Report — AI CLI Ecosystem
**Snapshot date: 2026-10-16 · Sources: community digests for Claude Code, OpenAI Codex, Gemini CLI, GitHub Copilot CLI, OpenCode, Pi, Qwen Code**

---

## 1. Ecosystem Overview

The AI CLI landscape is consolidating around a common core — agent loops, MCP tooling, sandboxed shell execution, and session persistence — while differentiation has shifted from "can it code" to **operational trust**: whether agents terminate honestly, whether sandboxes permit real workflows, and whether upgrades preserve state. Today's snapshot shows all seven tools shipping reliability hardening rather than headline features, with the loudest community signals being sandbox/permission friction, context and token economics, and platform parity (especially Windows). Release cadence remains high (Claude Code, Copilot CLI, OpenCode, Pi, Qwen Code all shipped; Codex shipped a minor plus alpha), but the maintenance load is visible: stealth-update regressions, V2→V1 downgrade corruption, OAuth latches, and CI flakiness. A second-order theme is **channel maturity** — some projects absorb feedback through heavy issue traffic, others through discussion boards or extension APIs — which makes raw volume a poor proxy for health.

---

## 2. Activity Comparison

> **Methodology note:** The digests use inconsistent aggregation. Where a digest states an exact repo-wide count (e.g. "39 issues updated", "50 updated issues, 16 updated PRs"), that figure is reported. Where only curated lists are provided, the table says so explicitly. **"N/A" means the digest contained no data for that channel or the channel does not exist — not that the tool is inactive.**

| Tool | Issues (last 24h) | PRs (last 24h) | Discussions | Release status |
|---|---|---|---|---|
| **Claude Code** | Not aggregated; 10 hot + ~9 secondary threads | **1** updated (throughput-constrained) | N/A (no discussion data) | **v2.1.301** patch — retry-watchdog cap, OTel guidance |
| **OpenAI Codex** | Not aggregated; 10 top issues | ~16 (10 listed + 6 related preset PRs) | **10** (3 Ideas, 1 Q&A, 6 Show & tell) | **rust-v0.163.0** + alpha.10.2 — searchable `/config`, live steering |
| **Gemini CLI** | Not aggregated; 10 hot issues | **10** updated | N/A (no discussion data) | **None** |
| **GitHub Copilot CLI** | **39** updated (top 30 shown; 10 summarized) | **0** updated | N/A (no discussion data) | **v1.0.99-1** and **v1.0.99-0** — `--trust-folder`, verified plugin installs, `/share`, session sandboxing |
| **OpenCode** | Not aggregated; 10 hot + ~7 tracking | ~15 (10 listed + ~5 merged) | N/A (no discussion data) | **v1.19.1** — bugfixes only; **session sharing discontinued** |
| **Pi** | **50** updated | **16** updated | **1** (Show & tell) | **v1.3.0** — configurable `/share` destinations |
| **Qwen Code** | Not aggregated; 10 hot + ~5 secondary | ~16 (10 listed + ~6 also moving) | N/A (no discussion data) | **6 releases**: v0.25.2, desktop-v0.25.2, sdk-typescript-v0.1.20, preview.1, 2 nightlies |

**Read-out:** Pi and Copilot CLI are the only repos reporting exact aggregate counts (50/16 and 39/0 respectively). Claude Code's single-PR day and Copilot CLI's zero-PR day stand out as review/merge bottlenecks rather than quiet communities — both had strong issue engagement.

---

## 3. Shared Feature Directions

Requirements appearing in **three or more** tool communities:

**Sandbox, permissions and safe shell execution** — the single most universal cluster.
- *Claude Code:* `apply-seccomp` failing on Linux `/proc/self/setgroups` (#43454, top-liked open bug, 6 months old); auto-mode classifier denying after explicit user approval with no remote override (#100370, #100374).
- *Codex:* Windows sandbox ACL failures on `node_repl.exe` (#51590), Linux protected-path cleanup (#54445), per-project network access (#53199), permission presets (#54383/#54385).
- *Gemini CLI:* zero-dependency OS sandboxing for bash-native workflows (#19873); guardrails against destructive git/temp-script behavior.
- *Copilot CLI:* macOS sandbox blocking local Gradle daemon (#5105), Linux bubblewrap preflight (#5052), sandbox-specific git credentials (#5102).
- *OpenCode:* permission check skipped for unparseable commands — bare redirects, PowerShell `--` (#49954, #55883; security hardening).

**Context, token and storage economics** — now a first-class product concern.
- *Claude Code:* unused connectors/MCP servers billed on every session (#102333, 56.8k tokens on "hi").
- *Codex:* `~/.codex/sessions` reaching ~165 GiB, ~95% base64 screenshots (#35458); prompt-cache misses (#54091).
- *Gemini CLI:* AST-aware reads/search/mapping EPIC to cut turns and token noise (#22745–#22747).
- *Pi:* auto-compaction not firing in CLI mode (#10330), image-heavy sessions stalling tasks (#10162), Bedrock Opus summarization blocked as policy violation (#10045).
- *Qwen Code:* large tool output hard-errors the turn instead of degrading gracefully (#14331).

**Honest agent termination and verification signals.**
- *Gemini CLI:* subagent reports `GOAL` success after hitting `MAX_TURNS` (#22323, p1) — directly undermines eval signals.
- *Claude Code:* #102599 (status doc claiming CI gates passed alongside a contradictory plan); unverified-claim theme.
- *Qwen Code:* `child_run` stuck at `dispatch_started` with no process ever existing (#13801); misleading timeout text that duplicates messages on retry (#14063).
- *Pi:* context recovery on declared model metadata vs. empty responses (PR #10964).

**Auth and credential robustness.**
- *Claude Code:* `session_stale_relogin` latching and force-logging-out live sessions (#93014); macOS Keychain 2000 ms timeout cached as logged-out (#101877).
- *Gemini CLI:* atomic, serialized credential-store writes (#29765); `/auth signin` no longer overwriting unrelated on-disk edits (#29762).
- *Pi:* ChatGPT OAuth ID token never persisted (#10300); Anthropic OAuth `Invalid effort level` for Opus 5/5.5 (#10063).
- *Copilot CLI:* Entra cached-account selection; MCP OAuth discovery shadowing configured Bearer headers (#3100).
- *Codex:* rotating stdin credentials for remote exec-server (#54434), Zsh credential masking (#54432).

**MCP and tool-scale correctness.**
- *Gemini CLI:* bounded `tools/list` discovery timeout, defaulting to 60 s (#29767); large tool-set errors (#24246).
- *Copilot CLI:* duplicate `content` + `structuredContent` injected into context (#4515); Cloudflare OAuth succeeding then reporting subscription-limit (#4991).
- *OpenCode:* global stdio MCP servers spawning once per loaded directory → OOM (#51003).
- *Pi:* `$ref` schema inlining for NVIDIA NIM (PR #10521); JSON-encoded nested tool arguments (PR #9569).
- *Claude Code:* making deferred MCP schemas genuinely lazy; plugin main-thread escalation (#101325).

**TUI/input ergonomics and terminal correctness.**
- Copilot CLI (vi/vim mode closed at 76 👍, cancel enqueued messages), Codex (searchable `/config`, key-bind presets, title/statusline dials), OpenCode (paged transcript, ⌘V paste fix), Pi (mintty/ConPTY color-query leakage, NumLock bindings, CJK bold), Qwen Code (Ctrl+V clipboard text on Linux), Claude Code (trackpad scroll history, missing redo binding).

**Update/upgrade and data-integrity hygiene.**
- *Claude Code:* stealth auto-update killing Remote Control sessions (#95364); macOS TCC permissions reset per version bump (#74068).
- *OpenCode:* V2→V1 downgrade leaving a hybrid DB where every session 500s (#55555); `opencode upgrade` leaving a 229-byte `.exe` stub (#51553, #55888).
- *Gemini CLI:* auto-update reporting success when the active binary is PATH-shadowed (#29757); atomic write overwriting read-only files and splitting hard links (#29749).
- *Pi:* interactive mode silently ignoring settings-save failures (#10168).

---

## 4. Differentiation Analysis

| Dimension | Claude Code | OpenAI Codex | Gemini CLI | Copilot CLI | OpenCode | Pi | Qwen Code |
|---|---|---|---|---|---|---|---|
| **Primary focus** | Observability-first agent deployment; unattended CI/automation | Windows Computer Use + cloud/remote daemon | Agent-orchestration correctness; AST tooling | GitHub-native workflow + MCP/plugin ecosystem | V2 core rewrite; desktop extensibility | Extension API + multi-provider correctness | Hosted `serve` control plane; multi-agent |
| **Target user** | Enterprise teams, automation/CI operators | Enterprise desktop + Windows power users | Agent builders, eval/benchmark authors | GitHub-centric teams, enterprise IdP users | Plugin/app developers, TUI power users | Multi-provider tinkerers, extension authors | Platform builders, hosted-service operators |
| **Technical approach** | Bundled skills/plugins, OTel tracing, watchdog config flags | Rust core, rotating remote credentials, `taskpolicy` daemon priority, SQLite | JS/TS; fault-graded agent registry; native bash affinity | Sandbox-scoped sessions, verified `ghapp://air/install` plugin channel | Core/runtime rewrite; GenUI render protocol; GUI plugin exports | Provider-agnostic catalog with metadata gating; O(n²) stream parse fix | Dual-path TS loop, Kubernetes resident workspaces, journal-only crash recovery, multi-language SDKs |
| **Notable distinction** | Only tool explicitly pushing OTel tracing guidance into a bundled skill | Deepest Computer Use/browser-control surface, and the deepest Windows pain | Strongest signal on *truthful* termination semantics | Community steering via sustained 👍 (vi mode, localization) | Only tool that **removed** a collaboration feature (session sharing) this cycle | Highest issue+PR throughput relative to footprint | Only tool shipping Java/Python/TS SDKs in lockstep with the CLI |

**Key divergences:**
- **Safety posture splits two ways.** Claude Code and Codex invest in *preventive* enforcement (permission classifiers, sandbox watches) and are now absorbing backlash for over-blocking and opacity. Gemini CLI and OpenCode are investing in *correctness of evaluation* (true termination signals, running permission checks even when parsing fails).
- **Extensibility models diverge.** Claude Code (skills/plugins), Copilot CLI (verified install links), OpenCode (GenUI + GUI plugin exports), and Pi (host extension lifecycle, `queue_update` observability) all treat the extension surface as a product, but OpenCode is the only one letting agents *render UI* into the host app.
- **Hosting model is a real fork.** Qwen Code is the only digest dominated by server-side concerns (Workspace bindings, TTL sub-session cleanup, Java SDK review criticals); Codex occupies a middle ground with remote exec-server credentials; Claude Code's remote story is currently a pain point (Remote Control sessions dropped by updates, no denial-override UI).

---

## 5. Community Momentum & Maturity

**Highest raw engagement (by thread depth):**
- **OpenAI Codex** — top threads at 94, 82, 61, 49 comments, plus a healthy 10-discussion board spanning Ideas/Q&A/Show-and-tell. Engagement is broad but heavily concentrated in a Windows Computer Use failure cluster.
- **Qwen Code** — one architectural thread at 68 comments (#12380) functioning as a de facto roadmap RFC; six releases in 24h shows aggressive cadence.
- **Claude Code** — highest single-issue heat (51 comments, 37 👍) and the strongest *reaction* signal in the dataset (#43454 at 49 👍), but **1 PR in 24h** indicates merge/review capacity, not contribution volume, is the binding constraint.

**Fastest iteration:**
- **Pi** — 50 issues + 16 PRs updated in one day, with a rapid open→closed PR flow (7 of 10 listed PRs already closed) and release v1.3.0. Small-footprint, high-velocity.
- **OpenCode** — ~15 PRs, several community-authored fixes closing reproduced bugs within the window (#55894, #55893, #55900, #55871). Fast loop, but the V2 migration is generating a steady parity-bug tail.
- **Codex** — parallel stable + alpha channels with infrastructure-grade PRs (credential rotation, SQLite journal modes, macOS daemon priority).

**Most mature operational posture:**
- **Gemini CLI** — disciplined labeling (`p1/need-retesting`, `p2`, `p3`), a clear EPIC structure for AST work, and PRs that pre-empt known failure modes (16 KB-sized discovery timeouts, atomic credential writes). Notably shipped zero releases today, and the tracker still looks healthy.
- **Copilot CLI** — closing long-standing community requests (vi mode at 76 👍) and improving supply-chain posture (verified Skills/plugin installs) — signs of a project past its initial feedback triage phase.

**Fragility indicators to watch:**
- **Claude Code:** the 2.37337.1 environment-picker refactor produced a multi-front Desktop regression wave (sidebar grouping, Routines hidden, silent worktree sessions, thinking display) — users are explicitly asking for toggles and rollback.
- **OpenCode:** bidirectional V2 migration loss (LSP/formatter dropped forward, hybrid-DB 500s backward) is the highest-severity item in the set.
- **Qwen Code:** a persistent `qwen-code-dev-bot` main-branch failure stream plus deflake PRs widening timeouts 30 s → 120 s suggests load-related test fragility.
- **Claude Code / Copilot CLI / Qwen Code:** effectively no usable discussion channel in this dataset — feedback is funneled entirely through issue trackers, which biases toward bug reporting over design debate.

---

## 6. Trend Signals

**1. "Did the agent actually succeed?" is becoming a first-class API concern.**
Gemini CLI's `MAX_TURNS`→`GOAL` misreport (#22323), Qwen Code's phantom `dispatch_started` runs (#13801), and Claude Code's unverified CI-pass claims (#102599) are the same failure in three architectures. Expect demand for machine-checkable termination contracts and third-party-verifiable run journals. This is the highest-leverage gap for tool builders targeting autonomous/CI use.

**2. Context economics has moved from optimization to product.**
Every tool now has a concrete cost story: 56.8k tokens for "hi" (Claude Code), 165 GiB of session screenshots (Codex), AST-aware reads as a token-reduction strategy (Gemini CLI), compaction that doesn't fire (Pi), large-output hard failure (Qwen Code). For developers: treat context budget management as an architectural requirement, not a tuning pass — deferred tool schemas, artifact references over inline payloads, and graceful degradation are becoming table stakes.

**3. Sandbox UX is the top unsolved friction across every platform.**
Six of seven tools report active sandbox blockers, spanning Linux seccomp, macOS Gradle localhost, Windows ACLs, and shell-scanner bypasses. Two distinct complaints are emerging: sandboxes that **block legitimate work** (Gradle daemon, local networking, air-gapped builds) and permission classifiers that **override explicit user approval** with no escape hatch (Claude Code #100370/#100374). Vendors that ship auditable, per-project, quickly-overridable policy will win developer trust faster than those tightening defaults.

**4. Windows parity is now a competitive differentiator, not a footnote.**
Codex (Computer Use, MXC launcher, sandbox ACL, model catalog) and Pi (ConPTY color queries, BEL hijacking the editor) carry the largest platform-specific clusters; OpenCode has a broken npm upgrade path and raw-mode leakage on Windows ConPTY. For teams standardizing on Windows dev fleets, this is currently the single best proxy for tool readiness.

**5. Update mechanics are a trust surface.**
Stealth updates that kill live sessions (Claude Code #95364), TCC permissions resetting per version bump (#74068), PATH-shadowed auto-update false success (Gemini CLI #29757), and 229-byte broken binaries after upgrade (OpenCode) all damage more trust than the features they ship. Release engineering, atomic migrations, and downgrade safety are becoming legitimate evaluation criteria.

**6. MCP has standardized the protocol but not the semantics.**
Duplicate `content`/`structuredContent`, OAuth-vs-Bearer fallback ambiguity, per-directory process explosion, 10-minute discovery stalls, and `$ref` schema rejection are all *implementation* gaps. The ecosystem needs a de facto conformance profile for MCP clients — result-format preference, discovery timeouts, auth precedence, and schema normalization.

**7. Remote and multi-agent workloads need lifecycle governance.**
Claude Code's dropped Remote Control sessions and missing remote approval UI, Codex's remote credential rotation and event-delivery-into-open-chat request, Qwen Code's zombie processes surviving 48 days and unbounded sub-session accumulation, and OpenCode's `pair --remote` 503s with no reset path all point the same direction: TTLs, resource accounting, and recoverable session ownership are the next infrastructure layer.

**8. Observability is graduating from third-party add-on to shipped guidance.**
Claude Code adding OpenTelemetry tracing guidance to its bundled `claude-api` skill, Gemini CLI collecting subagent trajectories for bug reports, and Codex recording local image inputs with SHA-256 metadata all indicate that agent runs are being treated as auditable production workloads. Developers building on these tools should assume trace export, run provenance, and artifact lineage will be expected — and should instrument for them now.

---

**Bottom line for decision-makers:** if your priority is autonomous CI/verified outcomes, weight Gemini CLI's termination-semantics work and Claude Code's observability push, while discounting Claude Code's review-velocity and Desktop churn risk. For Windows-heavy fleets, no tool is currently clean — Codex has the deepest capability and the deepest backlog. For plugin/platform builders, OpenCode and Pi offer the most active extension surfaces; for multi-language SDK integration, Qwen Code is uniquely shipping Java/Python/TS in lockstep. Across all seven, assume sandbox policy, context budgeting, and upgrade safety will determine real-world adoption more than model quality.

---

## Per-Tool Reports

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills Highlights

> Source: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills — Community Highlights Report
**Source:** github.com/anthropics/skills · **Data as of:** 2026-10-16

> ⚠️ **Data note:** The PR dataset reports `Comments: undefined` for all 50 pull requests. PR ranking below is therefore based on a composite of *activity recency, linked-issue discussion volume, and breadth of impact* rather than raw comment counts. Issue rankings use actual comment counts.

---

## 1. Top Skills Ranking

| # | Skill / PR | Functionality | Discussion highlights | Status |
|---|---|---|---|---|
| 1 | **skill-creator — eval viewer hardening** ([PR #1961](https://github.com/anthropics/skills/pull/1961)) | Hardens the local eval viewer (`generate_review.py` + `viewer.html`) against script breakout, DNS rebinding, cross-site POST, and escaping gaps | Directly answers [Issue #1394](https://github.com/anthropics/skills/issues/1394) (attribute-unsafe `escapeHtml`, display-path XSS). Renders untrusted model output locally — high blast radius | OPEN (updated 10-11) |
| 2 | **skill-creator — trigger eval isolation + Windows/runtime failures** ([PR #1298](https://github.com/anthropics/skills/pull/1298)) | Isolates per-worker command probes, fixes `select()` on Windows pipes, stops unrelated tools from aborting the scan; runtime failures no longer scored as "non-triggers" | Consolidates the largest cluster of skill-creator bugs: [#1352](https://github.com/anthropics/skills/issues/1352) (parallel workers cross-match UUIDs → false-negative trigger rates), [#1383](https://github.com/anthropics/skills/issues/1383), [#556](https://github.com/anthropics/skills/issues/556) | OPEN (updated 09-16) |
| 3 | **mcp-builder — mcp>=2 `streamable_http_client` + custom headers** ([PR #1742](https://github.com/anthropics/skills/pull/1742)) | Migrates connection scripts to the renamed `streamable_http_client` API; routes custom headers through `create_mcp_http_client` / `http_client` | Fixes [#1668](https://github.com/anthropics/skills/issues/1668). Sits alongside [Issue #1390](https://github.com/anthropics/skills/issues/1390) (evaluation.py scores 0/N against every real MCP server) — mcp-builder is the weakest link in the toolchain | OPEN (updated 10-08) |
| 4 | **skill-creator — YAML frontmatter scalar parsing** ([PR #1997](https://github.com/anthropics/skills/pull/1997)) | Replaces hand-rolled quote-stripping/continuation-line joining in `parse_skill_md` with correct YAML scalar parsing | Silent metadata drift between `SKILL.md` and what optimization commands actually see — corrupts eval inputs | OPEN (updated 10-13) |
| 5 | **webapp-testing — remove `shell=True`** ([PR #1980](https://github.com/anthropics/skills/pull/1980)) | Rewrites `with_server.py` to avoid shell command injection (CWE-78) while still supporting `cd <dir> && <command>` chains | Security fix in an agent-driven execution path; part of the widening security-focused PR stream | OPEN (updated 10-06) |
| 6 | **slack-gif-creator — text-bounds-aware centering** ([PR #2004](https://github.com/anthropics/skills/pull/2004)) | Uses Pillow ink bounding-box offsets instead of naive width/height halving when `centered=True` | Reproducible misalignment measured on Pillow 12.3.0 (caption requested at y=64 renders ink center at y=68) | OPEN (updated 10-14) |
| 7 | **document-typography** ([PR #514](https://github.com/anthropics/skills/pull/514)) | New skill for typographic QC: orphan word wrap, widow paragraphs, numbering misalignment in AI-generated documents | Broadest "affects every generated document" claim in the set; long-lived (created 03-04) | OPEN |
| 8 | **notion-spec-to-implementation + quantitative-resume-auditor** ([PR #1245](https://github.com/anthropics/skills/pull/1245)) | Two-skill bundle: turns product/tech specs into actionable Notion tasks with acceptance criteria; plus a resume quantification auditor | Representative of the "workflow automation" submission wave; still unmerged after 4+ months | OPEN (updated 10-15) |

**Also notable:** [PR #1977](https://github.com/anthropics/skills/pull/1977) (algorithmic-art `wrapAround()` double-ended bug, fixes #1897), [PR #1980](https://github.com/anthropics/skills/pull/1980), [PR #1792](https://github.com/anthropics/skills/pull/1792) (docx LibreOffice timeout falsely reported as success), [PR #1730](https://github.com/anthropics/skills/pull/1730) (dead docs URLs), [PR #486](https://github.com/anthropics/skills/pull/486) (ODT skill), [PR #83](https://github.com/anthropics/skills/pull/83) (skill-quality-analyzer + skill-security-analyzer).

---

## 2. Community Demand Trends

Distilled from the top-15 Issues:

1. **Trust & namespace integrity (loudest signal).** [Issue #492](https://github.com/anthropics/skills/issues/492) — *46 comments, by far the most-discussed item in the entire dataset* — warns that community skills shipped under the `anthropic/` namespace impersonate official skills and enable trust-boundary abuse. This is the community's #1 concern.
2. **Evaluation infrastructure that actually works.** A dense cluster of bug reports targets the measurement layer rather than any single skill: [Issue #556](https://github.com/anthropics/skills/issues/556) (0% trigger rate across all queries), [#1352](https://github.com/anthropics/skills/issues/1352) (parallel workers produce false negatives), [#1383](https://github.com/anthropics/skills/issues/1383) (six reproducible skill-creator failures), [#1390](https://github.com/anthropics/skills/issues/1390) (mcp-builder eval scores 0/N).
3. **Distribution & org-level sharing.** [Issue #228](https://github.com/anthropics/skills/issues/228) (16 comments, 8 👍) asks for an in-product shared skill library instead of manual `.skill` file passing; [Issue #189](https://github.com/anthropics/skills/issues/189) (9 👍) reports `document-skills` and `example-skills` installing identical content and polluting the context window.
4. **Context/token efficiency.** [Issue #1487](https://github.com/anthropics/skills/issues/1487) reports `claude-api` eagerly injecting ~156k tokens in a single tool call, exhausting the window.
5. **Memory & state compression.** [Issue #1329](https://github.com/anthropics/skills/issues/1329) proposes `compact-memory` — symbolic notation for compact agent state — a genuinely new capability direction rather than a bug fix.
6. **Governance / safety patterns.** [Issue #412](https://github.com/anthropics/skills/issues/412) proposes `agent-governance` (policy enforcement, threat detection, trust scoring, audit trails); [Issue #1385](https://github.com/anthropics/skills/issues/1385) proposes a three-gate reasoning quality pipeline.
7. **Document-script correctness.** [Issue #2018](https://github.com/anthropics/skills/issues/2018) — `merge_runs.py` drops run-local namespace declarations and writes invalid XML while reporting success; the direct `.docx` mode overwrites the input archive.

**Net read:** demand is skewed toward **hardening and meta-tooling** (skill-creator, mcp-builder, packaging, evals, security) far more than toward brand-new domain skills. New-skill proposals (Notion, ODT, typography, HPC, governance, memory) exist but attract far less discussion than the tooling/security issues.

---

## 3. High-Potential Pending Skills

Active, unmerged PRs with evidence of engaged maintainer/author iteration — most likely to land:

| PR | Skill | Why it may land soon |
|---|---|---|
| [#1742](https://github.com/anthropics/skills/pull/1742) | mcp-builder API migration | Fixes a tracked issue (#1668); author active from 09-08 through 10-08 |
| [#1298](https://github.com/anthropics/skills/pull/1298) | skill-creator trigger evals | Addresses three separately-filed issues at once; iterative updates through 09-16 |
| [#1961](https://github.com/anthropics/skills/pull/1961) | skill-creator viewer hardening | Security-scoped, tied to #1394; refreshed 10-11 |
| [#1997](https://github.com/anthropics/skills/pull/1997) | skill-creator YAML parsing | Correctness fix in the eval pipeline; refreshed 10-13 |
| [#2004](https://github.com/anthropics/skills/pull/2004) | slack-gif-creator centering | Small, well-scoped, reproducible; refreshed 10-14 |
| [#1977](https://github.com/anthropics/skills/pull/1977) | algorithmic-art `wrapAround()` | Closes #1897; clean one-function fix |
| [#1980](https://github.com/anthropics/skills/pull/1980) | webapp-testing `shell=True` | CWE-78 removal with a documented migration path |

*Stalled but still open:* [#514](https://github.com/anthropics/skills/pull/514) (document-typography, last touched 03-13), [#1245](https://github.com/anthropics/skills/pull/1245) (touched 10-15 but open since 06-02), [#83](https://github.com/anthropics/skills/pull/83) (last touched 01-07).

---

## 4. Skills Ecosystem Insight

**The community's most concentrated demand is not for new Skills but for a trustworthy Skill supply chain — namespace/security integrity, correct evaluation and trigger measurement, and reliable packaging — because the meta-tooling (skill-creator, mcp-builder, distribution) is where nearly all discussion, bugs, and security findings are concentrated.**

---

# Claude Code Community Digest — 2026-10-16

## 1. Today's Highlights
A single patch release landed (v2.1.301), extending the unattended retry watchdog with a configurable usage-limit wait cap and adding OpenTelemetry tracing guidance to the `claude-api` skill — a clear nudge toward observability-first agent deployments. Community attention remains dominated by **Desktop app regressions introduced by the 2.37337.1 environment-picker update** (sidebar grouping and Routines sections removed), plus a growing cluster of complaints that the **auto-mode permission classifier is over-blocking legitimate, user-approved work**. Sandbox, auth, and context-cost issues continue to accumulate on the CLI side.

## 2. Releases
**v2.1.301** (last 24h):
- Added `CLAUDE_CODE_RETRY_WATCHDOG_MAX_USAGE_LIMIT_WAIT_MS` — lets unattended retry mode (`CLAUDE_CODE_RETRY_WATCHDOG`) abort a request when the usage limit resets too far in the future, instead of blocking indefinitely. Directly targets long-running CI/automation use cases.
- Added OpenTelemetry tracing guidance to the bundled `claude-api` skill (release notes truncated in the source data).

No other releases in the window.

## 3. Hot Issues

1. **[#102088](https://github.com/anthropics/claude-code/issues/102088)** — *Desktop 2.37337.1: "Group by" removed from Code tab sidebar* (51 comments, 👍37). The highest-engagement item of the day: sessions can now only be grouped by environment. This is the flagship of a wave of Desktop UX regressions shipped in the same build — high community heat and broad reach.

2. **[#43454](https://github.com/anthropics/claude-code/issues/43454)** — *`apply-seccomp` fails on Linux: cannot write `/proc/self/setgroups`* (29 comments, 👍49). A six-month-old sandbox bug with the strongest 👍 signal in the dataset. It blocks Linux sandboxing entirely on affected kernels and remains unfixed, making it the clearest long-tail reliability debt.

3. **[#95364](https://github.com/anthropics/claude-code/issues/95364)** — *Desktop auto-update silently quits and relaunches, dropping every Remote Control session* (11 comments). Still-open report that idle-window "stealth updates" kill remote sessions across multiple machines. Undermines the core promise of remote/unattended use of Claude Code.

4. **[#102158](https://github.com/anthropics/claude-code/issues/102158)** — *Desktop 2.37337.1 (macOS): Routines sidebar section hidden with no toggle* (9 comments). Companion regression to #102088 — scheduled task results vanish from the sidebar. Signals the environment-picker refactor wasn't fully regression-tested.

5. **[#93014](https://github.com/anthropics/claude-code/issues/93014)** — *`session_stale_relogin` latches and force-logs-out active sessions* (6 comments, 👍6). OAuth refresh fails with `invalid_grant`, and the latch keeps forcing re-login. Auth instability that interrupts work mid-session.

6. **[#100370](https://github.com/anthropics/claude-code/issues/100370)** — *Auto-mode classifier blocks merging a PR into the user's own repo* (3 comments). A standing workflow (commit → PR → merge → deploy) is blocked with a "Merge Without Review" rationale. Points at a mismatch between classifier heuristics and user-declared skill pipelines.

7. **[#100374](https://github.com/anthropics/claude-code/issues/100374)** — *Auto mode keeps denying after explicit user approval, with no approval path for remote users* (3 comments). Directly related to #100370: when driving remotely via Remote Control, there is no UI affordance to override the denial at all.

8. **[#102333](https://github.com/anthropics/claude-code/issues/102333)** — *Unused connectors and MCP servers still cost tokens in every session* (2 comments). A session with a single "hi" already sits at 56.8k tokens of context. Connects context bloat to cost and latency; strong candidate for near-term product work.

9. **[#102496](https://github.com/anthropics/claude-code/issues/102496)** — *New Desktop sessions silently start in a worktree, with no setting to disable it* (1 comment). Behavior changed silently after the environment-picker update; linked to the previously closed-for-inactivity #53545.

10. **[#102245](https://github.com/anthropics/claude-code/issues/102245)** — *Windows: idle Claude Code session processes pin one thread at 100% CPU for hours* (1 comment, has repro). Desktop-hosted sessions burning a core while idle — a battery/thermal problem for laptop users.

**Also active today:** a cluster of **cyber-safety false positives** filed by the same reporter (#[88808](https://github.com/anthropics/claude-code/issues/88808), #[88718](https://github.com/anthropics/claude-code/issues/88718), #[88683](https://github.com/anthropics/claude-code/issues/88683), #[85393](https://github.com/anthropics/claude-code/issues/85393), #[88926](https://github.com/anthropics/claude-code/issues/88926)) — all closed as duplicates but resurging, each describing `session-halted` blocks on authorized work. Also notable: **[#102599](https://github.com/anthropics/claude-code/issues/102599)** (Claude Code self-filing a process violation), **[#102604](https://github.com/anthropics/claude-code/issues/102604)** (thinking blocks collapsed to one line / omitted in some sessions), **[#101877](https://github.com/anthropics/claude-code/issues/101877)** (macOS Keychain 2000 ms timeout cached as logged out for 30 s), **[#74068](https://github.com/anthropics/claude-code/issues/74068)** (macOS TCC prompts titled with bare version numbers, permissions reset per update), and **[#102537](https://github.com/anthropics/claude-code/issues/102537)** (trackpad scroll not navigating command history).

## 4. Key PR Progress

Only **one** PR was updated in the last 24h — unusually quiet, worth noting in itself:

1. **[#95698](https://github.com/anthropics/claude-code/pull/95698)** — `fix(plugins): run ralph-wiggum and output-style .sh hooks through bash with a quoted path` (open, by `claude[bot]`). Three bundled plugins registered hook scripts as bare, unquoted paths (e.g. `${CLAUDE_PLUGIN_ROOT}/hooks/stop-hook.sh`), which breaks when the plugin root contains spaces or when the script lacks a shebang/exec bit. The fix routes execution through `bash` with the path quoted. Refs #95673 and resolves the plugin half of #78490.

> Note: PR throughput in this window is too low to compile a top-10 list; treat this as a signal that review/merge activity — not contribution volume — is the current constraint.

## 5. Hot Discussions
*Omitted — no discussion data was provided in this dataset.*

## 6. Feature Request Trends
Distilled from the open enhancement-labeled issues:

- **Context and token economy** (#102333): make deferred MCP tool schemas actually lazy, so idle connectors don't cost tokens on every session start. The clearest cost-focused ask.
- **Desktop Code tab customization** (#102088, #102158, #102219): restore/expand grouping controls, expose Routines in the sidebar, and let plugins contribute rows to the Session Rail. Users want the Desktop Code tab to be configurable rather than opinionated.
- **Editor/keybinding completeness** (#76684): add redo (`Ctrl+R` in vim NORMAL mode, plus a bindable `chat:redo`) — undo exists without redo, which surprises vim users daily.
- **Permission-model escape hatches** (#100370, #100374, #101463): a reliable way to override the auto-mode classifier after explicit user approval, including for remote/Cowork sessions where no UI exists.
- **Plugin ecosystem plumbing** (#101325): direct main-thread connection for `cc-plugin-you-should-know` so plugin findings can be escalated selectively rather than copy-pasted.

## 7. Developer Pain Points

- **Desktop regressions shipping in point updates.** The 2.37337.1 environment-picker release removed/changed sidebar grouping, hid the Routines section, silently enabled worktree sessions, and altered thinking display. Multiple reports frame this as an untested refactor; users are asking for toggles and rollback.
- **Update mechanics that destroy state.** `#95364` (stealth update quits app mid-session, dropping Remote Control) and `#74068` (macOS TCC permissions reset on every version bump) make frequent releases actively costly for daily users.
- **Auto-mode classifier vs. real workflows.** Repeated reports of denials *after* explicit approval, opaque reasons, and no override path — especially painful for remote/headless sessions (#100370, #100374, #101463).
- **Auth and credential fragility.** OAuth `session_stale_relogin` latching (#93014) and Keychain read timeouts cached as "not logged in" (#101877) produce spurious logouts under load.
- **Safety-filter false positives halting sessions.** The recurring `cyber`-tagged cluster (Opus 4.8, `session-halted`) keeps being closed as duplicate without a visible systemic fix, and reporters keep returning.
- **Sandbox friction on Linux.** `apply-seccomp` failing on `/proc/self/setgroups` (#43454) remains the top-liked open bug, with no resolution in six months.
- **Resource waste in the background.** Idle Desktop sessions pinning a CPU thread on Windows (#102245) plus per-session context bloat from unused connectors (#102333) — performance and cost issues that scale with always-on usage.
- **Model self-discipline / verification.** #102599 (a status doc claiming CI gates passed alongside a plan saying otherwise) and the general theme of unverified claims from agents reflect demand for stronger process guarantees in long-running autonomous runs.

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex Community Digest — 2026-10-16

## Today's Highlights
Codex shipped **rust-v0.163.0**, adding a searchable `/config` experience, immediate application of supported preferences, restart-required indicators, and the ability to steer an active response with a follow-up message. The issue tracker is still dominated by **Windows Computer Use/sandbox failures** and **model-availability problems**, while PR activity focused on remote credential security, macOS daemon performance, SQLite reliability, and TUI customization.

## Releases
- **[rust-v0.163.0](https://github.com/openai/codex/releases/tag/rust-v0.163.0)** — New `/config` lets users search preferences across categories, change supported settings immediately, and see which changes require a restart. Users can also send a follow-up while Codex is responding to steer it immediately; interrupting a turn now stops active Code Mode cells.  
- **[rust-v0.163.0-alpha.10.2](https://github.com/openai/codex/releases/tag/rust-v0.163.0-alpha.10.2)** — Alpha release published.

## Hot Issues
1. **[#52407](https://github.com/openai/codex/issues/52407)** — Windows Dots CUA MXC launcher fails with `HRESULT 0x80070003` after direct shell recovery. High-impact Windows Computer Use blocker; 94 comments, 10 👍.  
2. **[#49458](https://github.com/openai/codex/issues/49458)** — Windows dot-started local tasks lack Computer Use tools while ordinary local Codex sessions work. Strong community signal: 82 comments, 26 👍.  
3. **[#25271](https://github.com/openai/codex/issues/25271)** — Computer Use cannot determine Chrome URL on Windows, even on `chrome://newtab/`. Long-running browser-control failure; 61 comments, 12 👍.  
4. **[#54083](https://github.com/openai/codex/issues/54083)** — Windows Chat reports “Model not found” for 5.6 and 6 at all thinking levels, while web succeeds. Model routing/availability regression; 49 comments.  
5. **[#51590](https://github.com/openai/codex/issues/51590)** — Windows sandbox fails opening running `node_repl.exe` for ACL update (error 32), blocking Computer Use and shell. Sandbox initialization pain; 41 comments, 11 👍.  
6. **[#53188](https://github.com/openai/codex/issues/53188)** — Windows browser tools fail to initialize with MXC launcher `HRESULT 0x80070003`. Same launcher family as #52407; 39 comments.  
7. **[#52747](https://github.com/openai/codex/issues/52747)** — Windows Desktop unknown injected `code_mode_tool_description_first` and opaque forced-cleanup denial. Config/sandbox opacity issue; 22 comments, 3 👍.  
8. **[#52839](https://github.com/openai/codex/issues/52839)** — Sol Ultrafast omitted from first-party model catalog with ChatGPT authentication. Affects model discovery and onboarding; 20 comments.  
9. **[#37754](https://github.com/openai/codex/issues/37754)** — TUI resume fails because `list_turns` is not supported yet. Breaks CLI session continuity; 18 comments, 4 👍.  
10. **[#35458](https://github.com/openai/codex/issues/35458)** — Desktop screenshots re-persisted in full on every compaction and inherited by subagent forks; `~/.codex/sessions` reached ~165 GiB, ~95% base64 images. Severe storage/context bloat; 14 comments.

## Key PR Progress
- **[#54445](https://github.com/openai/codex/pull/54445)** — Remove obsolete protected-path monitoring from the Linux sandbox; missing metadata paths already receive read-only synthetic mounts.  
- **[#54434](https://github.com/openai/codex/pull/54434)** — Add rotating stdin credentials for remote exec-server, including initial credential timeout and rotation support.  
- **[#54432](https://github.com/openai/codex/pull/54432)** — Support remote credential masking with Zsh shell snapshots, extending masking beyond Bash.  
- **[#54421](https://github.com/openai/codex/pull/54421)** — Treat explicit connections to the verified default daemon as local, preventing TUI connections from being misclassified as remote.  
- **[#54420](https://github.com/openai/codex/pull/54420)** — Record local image inputs on completed image-generation items with path and SHA-256 metadata.  
- **[#54414](https://github.com/openai/codex/pull/54414)** — Allow root turn suspension with live descendants, improving session recovery and shutdown behavior.  
- **[#54401](https://github.com/openai/codex/pull/54401)** — Run managed daemon processes at background priority on macOS via `taskpolicy -b`, reducing foreground impact.  
- **[#54398](https://github.com/openai/codex/pull/54398)** — Store generated image items as attachment references with `fileId`, falling back to inline results only when needed.  
- **[#54389](https://github.com/openai/codex/pull/54389)** — Preserve existing SQLite journal modes on startup to avoid failures when another connection holds a write lock.  
- **[#54388](https://github.com/openai/codex/pull/54388)** — Add quick dial presets for terminal title and status line layouts, preserving item order and aliases.  
- Related TUI preset work also landed in **[#54387](https://github.com/openai/codex/pull/54387)**, **[#54385](https://github.com/openai/codex/pull/54385)**, **[#54383](https://github.com/openai/codex/pull/54383)**, **[#54381](https://github.com/openai/codex/pull/54381)**, **[#54380](https://github.com/openai/codex/pull/54380)**, and **[#54379](https://github.com/openai/codex/pull/54379)**.

## Hot Discussions
### Ideas
- **[#54390](https://github.com/openai/codex/discussions/54390)** — “Latest CLI version feels like a step backwards.” Users argue sessions/threads add CLI bloat and GUI features should stay in the GUI; 1 comment, 1 👍.  
- **[#50754](https://github.com/openai/codex/discussions/50754)** — Feature request: event delivery into an existing local Codex Desktop chat without polling or launching a second app-server session.  
- **[#54124](https://github.com/openai/codex/discussions/54124)** — Custom key binds in the model selector for faster model switching.  

### Q&A
- **[#53199](https://github.com/openai/codex/discussions/53199)** — Answered: how to enable network access for only one local Codex Desktop project when custom permission profiles are missing from the picker.  

### Show and tell
- **[#54333](https://github.com/openai/codex/discussions/54333)** — Agent Standup: replay Codex sessions as a live “team call” from local rollout files, read-only.  
- **[#54166](https://github.com/openai/codex/discussions/54166)** — Built with Codex: CSV Check Studio / CSV Cleanup Kit, a local CSV preflight checker preserving conflicting contact IDs.  
- **[#53955](https://github.com/openai/codex/discussions/53955)** — Unmix: remove a recorded development decision and adapt retained changes through Codex MCP.  
- **[#53200](https://github.com/openai/codex/discussions/53200)** — Reset Alerts: SMS in US/Canada and Telegram elsewhere for Codex reset announcements.  
- **[#53492](https://github.com/openai/codex/discussions/53492)** — Secure Browser Use for Zen: Firefox/Zen bridge for Codex browser control.  
- **[#51825](https://github.com/openai/codex/discussions/51825)** — Project Architect: open Codex skill for long-running AI coding projects across chats and checkpoints.

## Feature Request Trends
- **TUI/CLI customization and restraint:** undo/redo typing (#2379), custom model-selector key binds (#54124), presets for prompts/permissions/title/statusline (#54379–#54388), and complaints that the CLI is growing GUI-like bloat (#54390).  
- **Session continuity and integration:** reliable TUI resume (#37754, #54073), event delivery into an already-open Desktop chat (#50754), remote dot session reads (#50157), and better turn suspension/recovery (#54414).  
- **Permissions, security, and network control:** per-project network access (#53199), inline permission profile selection (#54383), saving current permission settings as presets (#54385), and remote credential masking/rotation (#54432, #54434).  
- **Storage and resource governance:** controls for screenshot persistence (#35458), hidden memory-generation quota use (#51177), macOS memory/swap exhaustion (#44996), subagent memory leaks (#23260), and prompt-cache misses (#54091).  
- **Windows parity:** Computer Use, browser control, sandbox setup, model catalog, and timezone-sensitive model availability remain the largest cluster of requested fixes.  
- **Remote/cloud workflow reliability:** Codex Cloud library transfers (#51737), secure credential transport for Sites publication (#52801), and remote session compatibility (#50157).

## Developer Pain Points
- **Windows Computer Use and sandbox instability:** repeated MXC launcher `HRESULT 0x80070003` failures (#52407, #53188), missing Computer Use tools for dot-started tasks (#49458), Chrome URL detection failures (#25271), ACL error 32 on `node_repl.exe` (#51590), and opaque code-mode/sandbox denials (#52747).  
- **Model availability confusion on Windows:** “Model not found” for GPT-5.6/GPT-6 (#54083, #54209), China Standard Time triggering the issue (#54340), and Sol Ultrafast omitted from the first-party catalog (#52839).  
- **Session/resume reliability:** TUI resume failing on `list_turns` (#37754), `codex resume` failing with `thread/read failed` (#54073), project membership reverting (#53761), disappearing chat responses (#52750), and missing shared projects/chats (#47366).  
- **Resource bloat and quota surprises:** screenshot persistence reaching ~165 GiB (#35458), 13.6 GB macOS memory reports and swap exhaustion (#44996), subagent memory leaks (#23260), hidden memory-generation quota consumption (#51177), and cached-input loss (#54091).  
- **Configuration and permission opacity:** Guardian silently omitting `auto_review.extra_policy` (#50309), per-tool plugin approval modes being ignored (#41167), custom permission profiles missing from the picker (#53199), and secure OpenAI Platform tools unavailable on Windows (#54154).  
- **Remote/cloud connectivity blockers:** Codex Cloud library download failures (#51737), Sites publication blocked by missing secure credential transport (#52801), remote dot session format incompatibilities (#50157), and Cloudflare 403s in Windows desktop (#53515).

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI Community Digest — 2026-10-16

## Today's Highlights
No new releases were published in the last 24h. Activity was dominated by agent reliability and safety: the top issue is a p1 report that subagents can report `GOAL` success after hitting `MAX_TURNS` ([#22323](https://github.com/google-gemini/gemini-cli/issues/22323)), while high-engagement threads cover generalist-agent hangs ([#21409](https://github.com/google-gemini/gemini-cli/issues/21409)), bash sandboxing ([#19873](https://github.com/google-gemini/gemini-cli/issues/19873)), AST-aware codebase tooling ([#22745](https://github.com/google-gemini/gemini-cli/issues/22745)), and browser-agent failures/configuration ([#22267](https://github.com/google-gemini/gemini-cli/issues/22267), [#21983](https://github.com/google-gemini/gemini-cli/issues/21983)). PR traffic is focused on config/auth correctness, credential-store safety, MCP discovery timeouts, and terminal/IDE edge cases.

## Hot Issues
- [#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — **Subagent `MAX_TURNS` reported as `GOAL` success.** A `codebase_investigator` subagent reports `status: success` and `Termination Reason: GOAL` even though it hit the maximum turn limit before doing analysis. This undermines trust in agent outcomes and eval signals. **Reaction:** 13 comments, 2 👍; p1/need-retesting.
- [#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — **Zero-dependency OS sandboxing for bash-native workflows.** Proposal to exploit Gemini 3 models’ native bash affinity with sandboxing and post-execution intent routing, allowing POSIX tool chaining without compromising security. **Reaction:** 9 comments, 1 👍; large p2 enhancement.
- [#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — **Generalist agent hangs indefinitely.** Users report that deferral to the generalist agent can hang forever, even for simple folder creation; avoiding subagents is a workaround. **Reaction:** 8 comments, 8 👍; p1 bug.
- [#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — **AST-aware file reads, search, and mapping EPIC.** Tracks whether AST-aware tools can reduce turns, token noise, and misaligned reads. Related to [#22746](https://github.com/google-gemini/gemini-cli/issues/22746) and [#22747](https://github.com/google-gemini/gemini-cli/issues/22747). **Reaction:** 7 comments, 1 👍.
- [#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — **Gemini underuses skills and subagents.** Anecdotal reports say custom skills/subagents are rarely invoked unless explicitly instructed. This impacts extensibility and agent autonomy. **Reaction:** 7 comments; p2/need-retesting.
- [#22267](https://github.com/google-gemini/gemini-cli/issues/22267) — **Browser Agent ignores `settings.json` overrides.** `maxTurns` and other settings are merged by `AgentRegistry` but not honored by Browser Agent, breaking user configuration expectations. **Reaction:** 4 comments; p2 bug.
- [#22232](https://github.com/google-gemini/gemini-cli/issues/22232) — **Browser session lock recovery.** Requests automatic session takeover/recovery instead of fail-fast behavior when a persistent browser profile is locked by an orphaned instance. **Reaction:** 4 comments; p3 feature.
- [#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — **Browser subagent fails on Wayland.** Browser subagent reports `GOAL` termination but fails under Wayland. This is a platform-specific reliability blocker. **Reaction:** 4 comments, 1 👍; p1/agent-browser.
- [#21000](https://github.com/google-gemini/gemini-cli/issues/21000) — **Native file tools for task tracker.** Maintainers are experimenting with using native file tools to create and maintain the task tracker instead of relying on in-context tracking. **Reaction:** 4 comments; p3/customer issue.
- [#20079](https://github.com/google-gemini/gemini-cli/issues/20079) — **Symlinked agent files not recognized.** `~/.gemini/agents/filename.md` symlinks are not discovered as agents, complicating dotfile management and agent setup. **Reaction:** 4 comments; p2/need-info.

## Key PR Progress
- [#29762](https://github.com/google-gemini/gemini-cli/pull/29762) — **Preserve disk edits when changing authentication.** Prevents `/auth signin` from overwriting unrelated settings edited after Gemini CLI started. p1/core.
- [#29767](https://github.com/google-gemini/gemini-cli/pull/29767) — **Bound MCP `tools/list` discovery with a short timeout.** Avoids silent 10-minute stalls when an MCP server never answers `tools/list`; defaults discovery to 60s.
- [#29765](https://github.com/google-gemini/gemini-cli/pull/29765) — **Atomic and serialized file credential store writes.** Prevents silent credential loss or user lockout when multiple processes write concurrently or a write is interrupted.
- [#29756](https://github.com/google-gemini/gemini-cli/pull/29756) — **Configurable API request header/body timeouts.** Adds settings to raise model API timeouts under all auth types, addressing `UND_ERR_BODY_TIMEOUT` on long-thinking turns.
- [#29749](https://github.com/google-gemini/gemini-cli/pull/29749) — **Respect read-only files and hard links in atomic `writeTextFile`.** Fixes silent overwrites of `0444` files and hard-link splitting introduced by atomic rename.
- [#29750](https://github.com/google-gemini/gemini-cli/pull/29750) — **Resolve settings placeholders after loading `.env` files.** Fixes a load-order defect where placeholders defined only in `.env` failed settings validation. Closed.
- [#29769](https://github.com/google-gemini/gemini-cli/pull/29769) — **Harden `SKILL.md` frontmatter parsing.** Handles single-line descriptions, BOMs, trailing spaces after fence markers, and platform-specific formatting. Closed.
- [#29771](https://github.com/google-gemini/gemini-cli/pull/29771) — **Only auto-nudge genuinely empty continuations and await the nudge.** Fixes fire-and-forget nudges after tool loops that returned no visible text.
- [#29757](https://github.com/google-gemini/gemini-cli/pull/29757) — **Verify active binary update to prevent PATH-shadowing false success.** Ensures auto-update does not report success when the active binary is shadowed or managed by another package manager.
- [#29760](https://github.com/google-gemini/gemini-cli/pull/29760) — **Disable interactive credential/SSH prompts in `getSafeGitEnv`.** Prevents background Git extension checks from hijacking stdin/TUI and freezing the terminal. Closed.

## Feature Request Trends
- **Agent orchestration reliability and observability.** Strong demand for correct subagent termination semantics, shareable subagent trajectories, subagent context in bug reports, and better skill/subagent invocation. Examples: [#22323](https://github.com/google-gemini/gemini-cli/issues/22323), [#22598](https://github.com/google-gemini/gemini-cli/issues/22598), [#21763](https://github.com/google-gemini/gemini-cli/issues/21763), [#21968](https://github.com/google-gemini/gemini-cli/issues/21968).
- **Safe shell execution and sandboxing.** Users want Gemini’s bash affinity to be usable safely, with OS-level sandboxing and guardrails against destructive commands. Examples: [#19873](https://github.com/google-gemini/gemini-cli/issues/19873), [#22672](https://github.com/google-gemini/gemini-cli/issues/22672), [#23571](https://github.com/google-gemini/gemini-cli/issues/23571).
- **AST-aware, token-frugal codebase intelligence.** Repeated interest in AST-aware reads/search/mapping and surgical extraction to reduce turns and context bloat. Examples: [#22745](https://github.com/google-gemini/gemini-cli/issues/22745), [#22746](https://github.com/google-gemini/gemini-cli/issues/22746), [#22747](https://github.com/google-gemini/gemini-cli/issues/22747), [#19561](https://github.com/google-gemini/gemini-cli/issues/19561).
- **Persistent, file-based task tracking.** Proposal to replace in-context `WriteToDo` with CRUD file tracking and experiment with native file tools. Examples: [#18836](https://github.com/google-gemini/gemini-cli/issues/18836), [#21000](https://github.com/google-gemini/gemini-cli/issues/21000).
- **Browser-agent resilience and configuration.** Requests for settings overrides, session takeover/lock recovery, and Wayland support. Examples: [#22267](https://github.com/google-gemini/gemini-cli/issues/22267), [#22232](https://github.com/google-gemini/gemini-cli/issues/22232), [#21983](https://github.com/google-gemini/gemini-cli/issues/21983).
- **Extensibility/config correctness and tool scaling.** Subagent discovery via `settings.json`, symlink support, keybinding arrays, skill parsing, and MCP/tool-count limits. Examples: [#18285](https://github.com/google-gemini/gemini-cli/issues/18285), [#20079](https://github.com/google-gemini/gemini-cli/issues/20079), [#24246](https://github.com/google-gemini/gemini-cli/issues/24246).

## Developer Pain Points
- **Hanging agents and silent timeouts.** Generalist-agent hangs, interactive-prompt deadlocks, and MCP discovery stalls are recurring reliability blockers. See [#21409](https://github.com/google-gemini/gemini-cli/issues/21409), [#22465](https://github.com/google-gemini/gemini-cli/issues/22465), [#29767](https://github.com/google-gemini/gemini-cli/pull/29767).
- **Misleading success/termination signals.** Agents can report `GOAL` success while actually hitting turn limits or failing, masking incomplete work. See [#22323](https://github.com/google-gemini/gemini-cli/issues/22323), [#21983](https://github.com/google-gemini/gemini-cli/issues/21983), [#21763](https://github.com/google-gemini/gemini-cli/issues/21763).
- **Configuration ignored or overwritten.** Browser Agent ignores `settings.json`, symlinked agents are missed, and auth/settings flows can overwrite edits or resolve placeholders too early. See [#22267](https://github.com/google-gemini/gemini-cli/issues/22267), [#20079](https://github.com/google-gemini/gemini-cli/issues/20079), [#29762](https://github.com/google-gemini/gemini-cli/pull/29762), [#29750](https://github.com/google-gemini/gemini-cli/pull/29750).
- **Unsafe file/git behavior.** Atomic writes can overwrite read-only files or split hard links, and models can create scattered temp scripts or use destructive git commands. See [#29749](https://github.com/google-gemini/gemini-cli/pull/29749), [#22672](https://github.com/google-gemini/gemini-cli/issues/22672), [#23571](https://github.com/google-gemini/gemini-cli/issues/23571).
- **Terminal/TUI/IDE integration instability.** Flicker on resize, credential/SSH prompts hijacking stdin, diff-tab focus stealing, and IDE reconnection issues remain painful. See [#21924](https://github.com/google-gemini/gemini-cli/issues/21924), [#29760](https://github.com/google-gemini/gemini-cli/pull/29760), [#28183](https://github.com/google-gemini/gemini-cli/pull/28183), [#29744](https://github.com/google-gemini/gemini-cli/pull/29744).
- **Tool-count and MCP scaling limits.** CLIs encounter errors with large tool sets, and MCP servers can stall discovery. See [#24246](https://github.com/google-gemini/gemini-cli/issues/24246), [#29767](https://github.com/google-gemini/gemini-cli/pull/29767).

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI Community Digest — 2026-10-16

## Today's Highlights
Two releases landed in the last 24h: [v1.0.99-1](https://github.com/github/copilot-cli/releases/tag/v1.0.99-1) adds `--trust-folder` for one-off prompt/piped sessions and verified Skills/plugin installs via `ghapp://air/install?...`, while [v1.0.99-0](https://github.com/github/copilot-cli/releases/tag/v1.0.99-0) adds a `/share` action menu and session-scoped sandboxing. Community attention remains focused on input/session UX and platform reliability, with the long-running vi/vim input request closing at 76 👍 and several sandbox/MCP issues still driving discussion. No pull requests were updated in the last 24h.

## Releases

### [v1.0.99-1](https://github.com/github/copilot-cli/releases/tag/v1.0.99-1)
- **Added:** `--trust-folder` for one-off prompt-mode sessions started with `-p` or piped stdin.
- **Added:** Install verified Skills and plugins from GitHub install links via `copilot plugin install "ghapp://air/install?..."`.
- **Improved:** `/login` for Microsoft Entra now lets you choose a cached account.

### [v1.0.99-0](https://github.com/github/copilot-cli/releases/tag/v1.0.99-0)
- **Added:** `/share` without arguments opens a full-screen action menu for sharing the current session.
- **Improved:** `/sandbox` can keep sandboxing enabled for the current session without enabling it globally.
- **Improved:** Trusted repository plugins install and update automatically before chat runs.
- Additional release-note text is truncated in the source.

## Hot Issues
There were 39 issues updated in the last 24h; the source shows the top 30 by comment count. Below are 10 notable items, selected for engagement, impact, and recency.

- **[#13 — CLI input should have a vi/vim input mode](https://github.com/github/copilot-cli/issues/13)** — `CLOSED` | 14 comments | 👍76. A long-requested modal-editing mode for keyboard-driven users. Its closure with strong support confirms input ergonomics as a top community concern.

- **[#1857 — Allow users to cancel or remove enqueued messages before execution](https://github.com/github/copilot-cli/issues/1857)** — `OPEN` | 12 comments | 👍30. Messages queued via `Ctrl+Q` / `Ctrl+Enter` cannot be removed while the agent is busy or during `/compact`. This creates accidental prompts during long-running tasks.

- **[#4998 — Copilot CLI unusable after macOS update/reboot because `.mcp-writer.binding` persists stale filesystem device ID](https://github.com/github/copilot-cli/issues/4998)** — `CLOSED` | 11 comments | 👍11. A platform-specific MCP state issue that broke new and resumed sessions after macOS updates. Closed, but highlights fragility in persistent MCP bindings.

- **[#5105 — macOS sandbox blocks Gradle daemon connection despite local networking being allowed](https://github.com/github/copilot-cli/issues/5105)** — `OPEN` | 5 comments. The Gradle daemon starts on localhost but the client cannot connect inside the macOS sandbox. This materially affects Java/Gradle build workflows.

- **[#4991 — MCP: Cloudflare connection fails with “Subscription limit reached” after successful OAuth](https://github.com/github/copilot-cli/issues/4991)** — `CLOSED` | 5 comments. OAuth succeeds, but the runtime reports a subscription-limit error and then misleadingly says authentication is required. Shows ongoing MCP auth/error-handling complexity.

- **[#1575 — Allow Copilot to see the output from user shell commands](https://github.com/github/copilot-cli/issues/1575)** — `CLOSED` | 3 comments | 👍4. Users want manually run shell commands and their output included in Copilot’s context. Closed feature request pointing to deeper context-capture demand.

- **[#4515 — Copilot CLI exposes both MCP `content` and `structuredContent`](https://github.com/github/copilot-cli/issues/4515)** — `OPEN` | 3 comments. When MCP tool results include both fields, Copilot appears to add both to context. Expected behavior is to prefer `structuredContent`.

- **[#3100 — HTTP MCP server with Bearer token fails OAuth discovery instead of falling back to headers auth](https://github.com/github/copilot-cli/issues/3100)** — `OPEN` | 1 comment | 👍10. A common `.mcp.json` configuration pattern breaks because the CLI attempts OAuth discovery instead of using configured Authorization headers. High community signal despite low comment count.

- **[#5139 — Claude models (and Gemini Argon) fail with HTTP 400 “request body is not valid JSON”](https://github.com/github/copilot-cli/issues/5139)** — `OPEN` | `triage` | 1 comment. Claude Sonnet 5.5, Claude Opus 5.5, and Gemini Argon reportedly fail while other models work. Cross-model compatibility regressions are a high-impact concern.

- **[#4407 — Add Chinese (zh-CN) UI localization for the GitHub Copilot desktop app](https://github.com/github/copilot-cli/issues/4407)** — `OPEN` | 1 comment | 👍10. Requests localization for menus, settings, and terminal/CLI integration. Strong reaction signals demand for non-English UX.

## Key PR Progress
No pull requests were updated in the last 24h (`Total: 0`). There are no PRs to summarize today.

## Feature Request Trends
- **Input and keyboard control:** vi/vim mode ([#13](https://github.com/github/copilot-cli/issues/13)), cancel enqueued messages ([#1857](https://github.com/github/copilot-cli/issues/1857)), higher prompt history via `commandHistoryMaxSize` ([#5159](https://github.com/github/copilot-cli/issues/5159)), and repurposable top tabs ([#5153](https://github.com/github/copilot-cli/issues/5153)).
- **Sandbox and permissions flexibility:** localhost/Gradle access on macOS ([#5105](https://github.com/github/copilot-cli/issues/5105)), Linux bubblewrap preflight fixes ([#5052](https://github.com/github/copilot-cli/issues/5052)), `commandIdentifiers` with spaces ([#4150](https://github.com/github/copilot-cli/issues/4150)), and distinct sandbox git credentials ([#5102](https://github.com/github/copilot-cli/issues/5102)).
- **MCP and auth robustness:** prefer `structuredContent` ([#4515](https://github.com/github/copilot-cli/issues/4515)), Bearer-token fallback instead of OAuth discovery ([#3100](https://github.com/github/copilot-cli/issues/3100)), tenant-specific MCP auth for guest accounts ([#5157](https://github.com/github/copilot-cli/issues/5157)), and registry-only cloud MCP policy handling ([#5155](https://github.com/github/copilot-cli/issues/5155)).
- **Model compatibility and routing:** Claude/Gemini invalid-JSON failures ([#5139](https://github.com/github/copilot-cli/issues/5139)), unsupported sub-agent reasoning effort ([#4473](https://github.com/github/copilot-cli/issues/4473)), Sol context-budget rejections ([#5151](https://github.com/github/copilot-cli/issues/5151)), and Gemini cache telemetry anomalies ([#5161](https://github.com/github/copilot-cli/issues/5161)).
- **Context, memory, and tool output:** Copilot seeing manually run shell command output ([#1575](https://github.com/github/copilot-cli/issues/1575)) and preserving plan-mode steering responses ([#5160](https://github.com/github/copilot-cli/issues/5160)).
- **Localization and accessibility:** Chinese UI localization ([#4407](https://github.com/github/copilot-cli/issues/4407)).
- **Browser automation discoverability:** `/browser` or `/playwright` alongside `/computer` ([#5152](https://github.com/github/copilot-cli/issues/5152)).
- **Session lifecycle:** `/restart` with `--new-session-id` ([#5147](https://github.com/github/copilot-cli/issues/5147)) and session names reverting to linked PR titles ([#5158](https://github.com/github/copilot-cli/issues/5158)).

## Developer Pain Points
- **Sandbox and platform friction remains the biggest cluster:** macOS sandbox blocking local Gradle daemon connections ([#5105](https://github.com/github/copilot-cli/issues/5105)), Linux bubblewrap namespace preflight failures ([#5052](https://github.com/github/copilot-cli/issues/5052)), stale macOS MCP binding state ([#4998](https://github.com/github/copilot-cli/issues/4998)), and sandbox git credential limitations ([#5102](https://github.com/github/copilot-cli/issues/5102)).
- **MCP auth/configuration is still error-prone:** Bearer tokens falling through to OAuth discovery ([#3100](https://github.com/github/copilot-cli/issues/3100)), guest-account tenant selection ([#5157](https://github.com/github/copilot-cli/issues/5157)), Cloudflare OAuth/subscription errors ([#4991](https://github.com/github/copilot-cli/issues/4991)), and duplicate `content`/`structuredContent` context ([#4515](https://github.com/github/copilot-cli/issues/4515)).
- **Model-specific regressions disrupt workflows:** HTTP 400 invalid-JSON failures on Claude/Gemini ([#5139](https://github.com/github/copilot-cli/issues/5139)), unsupported reasoning effort for sub-agents ([#4473](https://github.com/github/copilot-cli/issues/4473)), and context-budget rejections before the first prompt ([#5151](https://github.com/github/copilot-cli/issues/5151)).
- **Input and session control gaps:** no way to cancel enqueued messages ([#1857](https://github.com/github/copilot-cli/issues/1857)), `/restart` conflicts with `--new-session-id` ([#5147](https://github.com/github/copilot-cli/issues/5147)), prompt-history ceilings ([#5159](https://github.com/github/copilot-cli/issues/5159)), and disappearing plan-mode steering responses ([#5160](https://github.com/github/copilot-cli/issues/5160)).
- **Platform-specific skill/plugin discovery issues persist:** Windows `skill` tool failing to find valid skills under `~/.agents/skills` ([#4401](https://github.com/github/copilot-cli/issues/4401)), even as release notes improve verified plugin/skill installation.

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode Community Digest — 2026-10-16

## Today's Highlights
The V2 migration continues to dominate community attention: LSP/formatter parity, agent-model resolution, and downgrade safety are all active threads, with a steady stream of reproduced bug reports landing in the last 24h. On the platform side, the team discontinued session sharing in v1.19.1 and is pushing Generative UI and GUI-plugin extensibility in the desktop app. Windows/upgrade-path and permission-check defects round out the highest-engagement issues.

## Releases
**v1.19.1** — Core bugfixes only:
- **Session sharing discontinued.** Existing share links and session-sharing commands are no longer available, and importing sessions from share URLs is unsupported. Expect follow-up discussion from users relying on share links for collaboration.

## Hot Issues

1. **[#50672 — Hardcoded default port 49374 locks out a second client](https://github.com/anomalyco/opencode/issues/50672)** (10 comments, OPEN)
   The managed background service binds a machine-global port while registration is per-user, so any second client on shared loopback (multi-user hosts, WSL mirrored networking) hits a generic timeout. High-impact for shared dev machines; the top-commented issue today.

2. **[#53756 — Bedrock inference profile ARNs get Nova-format effort variants for Claude](https://github.com/anomalyco/opencode/issues/53756)** (9 comments, OPEN)
   Claude-backed application inference profiles receive Nova-style `reasoningConfig`, producing 400s. V2 counterpart of an earlier V1 report — a real blocker for Bedrock enterprise users.

3. **[#55629 — Big Pickle "THINKING" bleeds into the answer](https://github.com/anomalyco/opencode/issues/55629)** (9 comments, OPEN, pending close/triaging)
   `/think` tags appearing raw in output, either mis-emitted by the model or mis-parsed client-side. Users report the last four releases all exhibit the problem.

4. **[#50916 — LSP support gutted from v2?](https://github.com/anomalyco/opencode/issues/50916)** (7 comments, 10 👍, OPEN)
   Community asking for clarification after migration docs state V2 does not run language servers, expose LSP tools, or produce diagnostics. Highest-reaction issue in the set — a core capability regression.

5. **[#38528 — V2: port LSP and formatter services](https://github.com/anomalyco/opencode/issues/38528)** (6 comments, 11 👍, OPEN)
   The tracking issue for restoring LSP/formatter behavior under the V2 Core/runtime architecture. The 11 👍 signal this as the single most-wanted V2 feature.

6. **[#55163 — V2 TUI leaves terminal in raw mode after /exit on Windows ConPTY](https://github.com/anomalyco/opencode/issues/55163)** (8 comments, OPEN)
   Exiting under Alacritty + zellij-windows can corrupt the shell prompt or take down the multiplexer. V2 counterpart to a V1 report; significant for Windows TUI users.

7. **[#51003 — Global stdio MCP servers spawn once per loaded directory](https://github.com/anomalyco/opencode/issues/51003)** (6 comments, OPEN)
   Clients with many open directories (e.g. one per chat) multiply MCP processes until the host OOMs and health checks time out. A scaling landmine for multi-directory workflows.

8. **[#55727 — Agent `model` config ignored for new sessions](https://github.com/anomalyco/opencode/issues/55727)** (3 comments, reproduced, OPEN)
   In 2.0.29, `agents.<id>.model` and agent frontmatter are ignored; `opencode run` always uses the default model. Reproduced and already has a community PR ([#55894](https://github.com/anomalyco/opencode/pull/55894)).

9. **[#55886 — Server restart mid-tool-call breaks the next Anthropic request](https://github.com/anomalyco/opencode/issues/55886)** (2 comments, OPEN)
   Interrupted `shell` tool calls replay a malformed `tool_use.input`, and Anthropic rejects with `400 Input should be an object`. One restart broke all three sessions that had a live tool call — fixed by PR [#55900](https://github.com/anomalyco/opencode/pull/55900).

10. **[#55525 — Piped stdout truncated at exactly 64 KiB](https://github.com/anomalyco/opencode/issues/55525)** (3 comments, reproduced, OPEN)
    `opencode --completions fish` truncates at 65,536 bytes, breaking Homebrew-generated fish completions. Fixed by PR [#55893](https://github.com/anomalyco/opencode/pull/55893).

*Also worth tracking:* [#54428](https://github.com/anomalyco/opencode/issues/54428) and [#54305](https://github.com/anomalyco/opencode/issues/54305) (streaming-markdown render perf), [#55555](https://github.com/anomalyco/opencode/issues/55555) (V2→v1 downgrade leaves a hybrid DB, all sessions 500), [#51553](https://github.com/anomalyco/opencode/issues/51553)/[#55888](https://github.com/anomalyco/opencode/issues/55888) (npm 12 upgrade skips postinstall, leaves a 229-byte `opencode.exe`), [#55726](https://github.com/anomalyco/opencode/issues/55726) (`max_tokens` sent alongside `max_completion_tokens`, strict endpoints reject).

## Key PR Progress

1. **[#54747 — feat(app): generative UI in the GUI](https://github.com/anomalyco/opencode/pull/54747)** (OPEN)
   Agents emit a small GenUI program via `genui_render`; the app renders it with OpenCode's own components as the call streams, with render validation feedback. On by default, gated under Settings → Experimental.

2. **[#55901 — fix(infra): deploy beta web app with Workers Static Assets](https://github.com/anomalyco/opencode/pull/55901)** (OPEN)
   Fixes `beta.opencode.ai` deploys failing with code 10024 after Office previews landed (a 27 MB wasm asset exceeding the 25 MiB KV limit).

3. **[#55900 — fix(core): keep interrupted tool calls replayable for Anthropic](https://github.com/anomalyco/opencode/pull/55900)** (OPEN, closes #55886)
   Ensures aborted `streaming`/`running` tool calls emit a valid `tool_use` payload on replay instead of `input: null`.

4. **[#55893 — fix(cli): deliver large stdout writes past one pipe buffer](https://github.com/anomalyco/opencode/pull/55893)** (OPEN, closes #55525)
   Root cause traced to stdout/stderr left non-blocking; restores ~68 KiB completion output through pipes.

5. **[#55246 — feat(desktop): ship GUI plugins through `./gui` exports](https://github.com/anomalyco/opencode/pull/55246)** (OPEN)
   Plugins in `opencode.json` gain a GUI part beside `server` and `./tui` entries. Built-in desktop features become built-in plugins, listed in Settings → Plugins.

6. **[#51195 — feat(tui): paged transcript output experiment](https://github.com/anomalyco/opencode/pull/51195)** (OPEN, closes #51203)
   Adds paged transcript output for readability — a direct response to user reports of eye strain reading long streaming transcripts.

7. **[#55894 — fix(core): honor the selected agent's configured model](https://github.com/anomalyco/opencode/pull/55894)** (OPEN, closes #55727)
   Community contributor fix for agent-level `model` being ignored on new sessions.

8. **[#49954](https://github.com/anomalyco/opencode/pull/49954) / [#55883 — fix(core): run shell permission check even when the command can't be parsed](https://github.com/anomalyco/opencode/pull/55883)** (OPEN / CLOSED)
   Commands the shell scanner decomposes to zero nodes (bare redirects, PowerShell with `--`) previously skipped `permission.assert` and ran unchecked. A meaningful security hardening.

9. **[#55871 — fix(tui): roll the compact form over to M at the rounding boundary](https://github.com/anomalyco/opencode/pull/55871)** (CLOSED, closes #55256)
   `Locale.number` picked its unit before rounding, printing 999,950–999,999 as `1000.0K`. Contributor fix, merged.

10. **[#55020](https://github.com/anomalyco/opencode/pull/55020) / [#54482 — fix: paste into unfocused composer; include attached images in `/btw` side questions](https://github.com/anomalyco/opencode/pull/54482)** (OPEN)
    Two input-path papercuts: `⌘V` inserting a literal `v` in the session composer/embedded terminal, and `/btw` dropping freshly attached images.

*Also merged:* [#55248](https://github.com/anomalyco/opencode/pull/55248) (Commentary timeline category), [#55889](https://github.com/anomalyco/opencode/pull/55889) (fade long file names in file tree), [#55879](https://github.com/anomalyco/opencode/pull/55879) (steady Review pane on side-terminal toggle), [#55899](https://github.com/anomalyco/opencode/pull/55899) (file view stays in place on link reread), [#55756](https://github.com/anomalyco/opencode/pull/55756) (publish prompt-prep status before cold activation).

## Feature Request Trends
- **V2 parity with V1 is the dominant ask.** LSP diagnostics and formatter services ([#38528](https://github.com/anomalyco/opencode/issues/38528), [#50916](https://github.com/anomalyco/opencode/issues/50916)) have the strongest 👍 signal, and migration friction is generating a long tail of parity bugs.
- **Extensibility & UI surface.** Generative UI ([#54747](https://github.com/anomalyco/opencode/pull/54747)) and GUI plugin exports ([#55246](https://github.com/anomalyco/opencode/pull/55246)) point to a push to make the desktop app programmable by agents and third-party plugins.
- **Structured, paged, readable output.** The paged transcript experiment ([#51195](https://github.com/anomalyco/opencode/pull/51195)), commentary timeline category, and thinking-text color fixes ([#55686](https://github.com/anomalyco/opencode/issues/55686)) form a coherent TUI readability theme.
- **Remote/portable session access.** OpenTunnel reliability ([#54624](https://github.com/anomalyco/opencode/issues/54624), [#55873](https://github.com/anomalyco/opencode/issues/55873), [#55864](https://github.com/anomalyco/opencode/issues/55864)) is an unmet need, especially now that share links are gone.
- **Safer shell execution.** Permission evaluation for unparseable commands ([#49954](https://github.com/anomalyco/opencode/pull/49954), [#55883](https://github.com/anomalyco/opencode/pull/55883)) reflects demand for defense-in-depth.

## Developer Pain Points
- **V2 migration is lossy in both directions.** V1→V2 loses LSP/formatter behavior and agent `model` resolution; V2→V1 downgrades leave a hybrid database where every existing session returns HTTP 500 ([#55555](https://github.com/anomalyco/opencode/issues/55555)).
- **Upgrade path is fragile on Windows/npm.** `opencode upgrade` reports success while leaving a 229-byte text stub as `bin/opencode.exe`, breaking the next invocation ([#51553](https://github.com/anomalyco/opencode/issues/51553), [#55888](https://github.com/anomalyco/opencode/issues/55888)) — postinstall is skipped.
- **TUI/terminal state management.** Windows ConPTY raw-mode leakage ([#55163](https://github.com/anomalyco/opencode/issues/55163)), markdown cursor-offset corruption ([#48714](https://github.com/anomalyco/opencode/issues/48714)), and muted-color regressions ([#55686](https://github.com/anomalyco/opencode/issues/55686)) show renderer/finalization regressions between V1 and V2.
- **Resource scaling.** One MCP process per loaded directory ([#51003](https://github.com/anomalyco/opencode/issues/51003)) and duplicate fff engines on location invalidation ([#54631](https://github.com/anomalyco/opencode/issues/54631)) exhaust memory in multi-session workflows.
- **Streaming render cost.** Two profiled reports quantify per-delta main-thread cost in the session-ui markdown store ([#54305](https://github.com/anomalyco/opencode/issues/54305), [#54428](https://github.com/anomalyco/opencode/issues/54428)) — block re-parse, not store walk, dominates.
- **Provider-integration regressions.** Strict OpenAI-compatible endpoints rejecting `max_tokens` + `max_completion_tokens` ([#55726](https://github.com/anomalyco/opencode/issues/55726)), Bedrock ARN handling ([#53756](https://github.com/anomalyco/opencode/issues/53756)), and interrupted-call replay ([#55886](https://github.com/anomalyco/opencode/issues/55886)) all break previously working setups.
- **Remote pairing flakiness.** `pair --remote` 503s and stuck pending tunnels with no reset path ([#55873](https://github.com/anomalyco/opencode/issues/55873), [#54624](https://github.com/anomalyco/opencode/issues/54624)) leave users with no recovery mechanism.

*No Discussion data was provided for this period, so section 5 is omitted.*

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi Community Digest — 2026-10-16

Source: [earendil-works/pi](https://github.com/earendil-works/pi). Last 24h snapshot: 1 release, 50 updated issues, 16 updated PRs, 1 discussion.

## 1. Today's Highlights

- **v1.3.0** adds configurable `/share` upload destinations via `sessionShareDestination` (`auto`, `gist`, or `radius`).
- The highest-engagement issues are around **OAuth/identity persistence**, **terminal compatibility regressions**, **fullscreen rendering performance**, and **context/compaction reliability**.
- The PR queue is focused on robustness: context recovery, streaming JSON parse performance, small-context compaction tuning, model compatibility, and extension UI lifecycle fixes.

## 2. Releases

- **[v1.3.0](https://github.com/earendil-works/pi/releases/tag/v1.3.0)** — New feature: choose where `/share` uploads with `sessionShareDestination`, supporting `auto`, `gist`, or `radius`. See the linked release and docs for session sharing/export behavior.

## 3. Hot Issues

1. **[#10300 — ChatGPT OAuth ID token is not persisted](https://github.com/earendil-works/pi/issues/10300)**  
   `credentialFromTokenResponse` omits the ID token, so extensions cannot access account identity; token refresh uses the same path. **Reaction:** 16 comments, open — high-impact auth/extension issue.

2. **[#10256 — Terminal color query replies leak into prompt and BEL opens external editor](https://github.com/earendil-works/pi/issues/10256)**  
   Affects mintty/ConPTY on Windows in 0.99.x; terminal replies appear in the prompt and BEL triggers the editor. **Reaction:** 12 comments, 4 👍 — notable Windows terminal regression.

3. **[#9549 — Large transcripts re-render every frame; resize re-emits whole transcript](https://github.com/earendil-works/pi/issues/9549)**  
   Fullscreen mode saturates one core on large transcripts, especially on lower-core Windows Terminal setups. **Reaction:** 11 comments, 1 👍 — persistent performance pain.

4. **[#9335 — openai-responses: support `configuration_update` for cache-preserving reasoning changes](https://github.com/earendil-works/pi/issues/9335)**  
   Would let users change reasoning effort without busting prompt cache for GPT-6. **Reaction:** 11 comments, 8 👍, closed as no-action — strong demand despite closure.

5. **[#10840 — GPT-6.1 Sol still uses 272K after Codex default increased to 500K](https://github.com/earendil-works/pi/issues/10840)**  
   Stale catalog metadata keeps Pi’s context window and compaction budget behind upstream Codex. **Reaction:** 10 comments, closed — model-catalog drift concern.

6. **[#10063 — Anthropic OAuth returns `Invalid effort level` for Opus 5/5.5 and Fable 5](https://github.com/earendil-works/pi/issues/10063)**  
   Direct Anthropic OAuth requests fail at default/low/high thinking modes. **Reaction:** 8 comments, open — blocks Anthropic OAuth users.

7. **[#10074 — Anthropic tool calls corrupt non-ASCII edit arguments](https://github.com/earendil-works/pi/issues/10074)**  
   Dropped `u` in `\uXXXX` causes control characters and occasional file corruption, especially with Korean text. **Reaction:** 8 comments, open — data-integrity risk.

8. **[#10162 — Too many input images stop the agent task](https://github.com/earendil-works/pi/issues/10162)**  
   Breaks long-running autonomous sessions that rely on auto-compaction. **Reaction:** 7 comments, open — affects agent durability.

9. **[#10330 — Auto-compaction does not start in CLI mode](https://github.com/earendil-works/pi/issues/10330)**  
   `pi --mode json` never auto-compacts, while TUI mode works. **Reaction:** 7 comments, open — CLI reliability gap.

10. **[#10045 — Auto-compaction of Opus 5.5 on Bedrock blocked by Anthropic policy](https://github.com/earendil-works/pi/issues/10045)**  
    Long sessions fail near ~98% context because summarization is blocked as a policy violation. **Reaction:** 6 comments, updated 2026-10-16 — critical for Bedrock/Opus users.

## 4. Key PR Progress

1. **[PR #11016 — Fix/copilot request size overflow](https://github.com/earendil-works/pi/pull/11016)** *(OPEN)*  
   Targets large sessions with images; `/compact` helps manually, but the PR aims for automatic recovery.

2. **[PR #11008 — Throttle streaming partial-json re-parse](https://github.com/earendil-works/pi/pull/11008)** *(CLOSED)*  
   Fixes O(n²) Anthropic stream parsing that could wedge the event loop for minutes on multi-MB tool arguments.

3. **[PR #10964 — Base context recovery on declared model metadata](https://github.com/earendil-works/pi/pull/10964)** *(CLOSED)*  
   When a turn exhausts context, Pi recovers with one compaction and continuation instead of returning an empty response.

4. **[PR #11009 — Use 90% threshold for small-context compaction](https://github.com/earendil-works/pi/pull/11009)** *(CLOSED)*  
   Avoids premature compaction for 200k–400k context models by changing the trigger threshold.

5. **[PR #11007 — Check model catalog against latest release before publishing](https://github.com/earendil-works/pi/pull/11007)** *(CLOSED)*  
   Prevents catalog updates from requiring code older Pi releases do not have, a failure seen with kimi-coding.

6. **[PR #11005 — Avoid `thinkingBudget` and MINIMAL on unmapped Gemini 4 models](https://github.com/earendil-works/pi/pull/11005)** *(CLOSED)*  
   P0 fix for Gemini 4 custom `models.json` definitions and `pi-ai` callers hitting HTTP 400.

7. **[PR #11010 — Include allowed values in enum validation errors](https://github.com/earendil-works/pi/pull/11010)** *(CLOSED)*  
   Gives models the valid enum set, reducing retry round-trips and manual schema inspection.

8. **[PR #9569 — Coerce JSON-encoded object and array tool arguments](https://github.com/earendil-works/pi/pull/9569)** *(OPEN)*  
   Recovers nested tool arguments that models incorrectly double-encode as JSON strings.

9. **[PR #10521 — Inline `$ref` tool schemas for NVIDIA NIM models](https://github.com/earendil-works/pi/pull/10521)** *(OPEN)*  
   Fixes NVIDIA NIM models returning local `$ref` objects as JSON strings, which `validateToolArguments` currently rejects.

10. **[PR #11000 — Drop thinking from other models instead of sending as text](https://github.com/earendil-works/pi/pull/11000)** *(CLOSED)*  
    Prevents old model reasoning blocks from being replayed as visible text after switching models.

## 5. Hot Discussions

**Show and tell**

- **[#10974 — swe-mux: run pi sessions side by side and from your phone](https://github.com/earendil-works/pi/discussions/10974)**  
  Native Pi status detection, tabs/panes, dev-server previews, and mobile access over live terminal sessions. **Reaction:** 1 👍.

No Ideas or Q&A discussions appeared in the provided 24h data.

## 6. Feature Request Trends

- **Extension API maturity:** More observability and lifecycle control — suppress bug hints for explained errors ([#10354](https://github.com/earendil-works/pi/issues/10354)), emit `queue_update` for injected inputs ([#10317](https://github.com/earendil-works/pi/issues/10317)), durable host extensions ([#10386](https://github.com/earendil-works/pi/issues/10386)), prompt lifecycle from `agent_settled` ([#10388](https://github.com/earendil-works/pi/issues/10388)), and atomic hosted-session idle close ([#11018](https://github.com/earendil-works/pi/issues/11018)).
- **Provider/model metadata correctness:** Cache-preserving reasoning controls ([#9335](https://github.com/earendil-works/pi/issues/9335)), context-window updates ([#10840](https://github.com/earendil-works/pi/issues/10840)), Gemini thought signatures ([#10157](https://github.com/earendil-works/pi/issues/10157)), and OAuth identity persistence ([#10300](https://github.com/earendil-works/pi/issues/10300)).
- **Context and compaction control:** CLI auto-compaction ([#10330](https://github.com/earendil-works/pi/issues/10330)), Bedrock Opus policy workarounds ([#10045](https://github.com/earendil-works/pi/issues/10045)), image-heavy session limits ([#10162](https://github.com/earendil-works/pi/issues/10162)), and less aggressive small-context compaction ([PR #11009](https://github.com/earendil-works/pi/pull/11009)).
- **Performance and cost awareness:** Fullscreen transcript rendering ([#9549](https://github.com/earendil-works/pi/issues/9549)), codemode highlighting reuse ([#10971](https://github.com/earendil-works/pi/issues/10971)), and avoiding re-billing when the system prompt head moves mid-run ([#10024](https://github.com/earendil-works/pi/issues/10024)).
- **Terminal/UX robustness:** CJK bold rendering ([#10154](https://github.com/earendil-works/pi/issues/10154)), NumLock/kitty keybindings ([#10352](https://github.com/earendil-works/pi/issues/10352)), paste/editor edge cases ([#10351](https://github.com/earendil-works/pi/issues/10351)), and OSC conflicts ([#11017](https://github.com/earendil-works/pi/issues/11017)).

## 7. Developer Pain Points

- **OAuth and provider auth fragmentation:** ChatGPT OAuth ID token persistence ([#10300](https://github.com/earendil-works/pi/issues/10300)) and Anthropic OAuth effort-level failures ([#10063](https://github.com/earendil-works/pi/issues/10063)) remain high-friction.
- **Terminal/platform regressions:** mintty/ConPTY color queries, BEL opening external editors ([#10256](https://github.com/earendil-works/pi/issues/10256)), NumLock keybinding mismatches ([#10352](https://github.com/earendil-works/pi/issues/10352)), stdin `read EIO` crashes ([#10272](https://github.com/earendil-works/pi/issues/10272)), and OSC marker conflicts ([#11017](https://github.com/earendil-works/pi/issues/11017)).
- **Context/compaction reliability:** CLI auto-compaction not firing ([#10330](https://github.com/earendil-works/pi/issues/10330)), Bedrock Opus policy blocks ([#10045](https://github.com/earendil-works/pi/issues/10045)), wrong resumed context levels ([#10082](https://github.com/earendil-works/pi/issues/10082)), and image-heavy sessions stopping tasks ([#10162](https://github.com/earendil-works/pi/issues/10162)).
- **Performance bottlenecks:** Fullscreen transcript re-rendering ([#9549](https://github.com/earendil-works/pi/issues/9549)), O(n²) streaming partial JSON parsing ([PR #11008](https://github.com/earendil-works/pi/pull/11008)), and repeated codemode highlighting ([#10971](https://github.com/earendil-works/pi/issues/10971)).
- **Tool-call/data integrity:** Corrupted non-ASCII edit arguments ([#10074](https://github.com/earendil-works/pi/issues/10074)), JSON-encoded nested arguments ([PR #9569](https://github.com/earendil-works/pi/pull/9569)), `$ref` schema rejection ([PR #10521](https://github.com/earendil-works/pi/pull/10521)), and dropped Gemini thought signatures ([#10157](https://github.com/earendil-works/pi/issues/10157)).
- **Extension ergonomics:** No receipt for injected inputs ([#10317](https://github.com/earendil-works/pi/issues/10317)), silent deferred `prompt()` behavior ([#10388](https://github.com/earendil-works/pi/issues/10388)), and custom UI overlay closure bugs ([PR #10999](https://github.com/earendil-works/pi/pull/10999)).
- **Configuration persistence:** Interactive mode silently ignores settings-save failures, e.g. read-only `settings.json` ([#10168](https://github.com/earendil-works/pi/issues/10168)).

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code Community Digest — 2026-10-16

## 1. Today's Highlights

The v0.25.2 line shipped across CLI, Desktop, and the TypeScript SDK (which now bundles CLI 0.25.2), carrying another heavy batch of `managed-agent` hardening — notably the H4e-b2 lead mailbox over Session messages and connector/broker robustness fixes out of the #12692 R2 review. Activity is dominated by the Hosted/serve control plane: fault-gate test coverage (FG8/FG9), session lifecycle cleanup, and journal-only crash recovery. Meanwhile, user-facing bug reports cluster around graceful degradation (large tool output blowing the context window), resource leaks in long-lived sessions, and Web Shell feature parity with the terminal.

## 2. Releases

| Release | Notes |
|---|---|
| [v0.25.2](https://github.com/QwenLM/qwen-code/releases) | Stable CLI release. Highlights: `feat(managed-agent): H4e-b2` — the Hosted team lead's mailbox delivered over Session messages ([#13892](https://github.com/QwenLM/qwen-code/pull/13892)); connector and broker robustness from the #12692 R2 review ([#13330](https://github.com/QwenLM/qwen-code/pull/13330)). No known breaking changes. |
| [desktop-v0.25.2](https://github.com/QwenLM/qwen-code/releases) | Desktop build on the same change set, including the broker/connector robustness fixes. |
| [sdk-typescript-v0.1.20](https://github.com/QwenLM/qwen-code/releases) | Bundles CLI 0.25.2 (built from the same branch/ref as the SDK). |
| [v0.25.2-preview.1](https://github.com/QwenLM/qwen-code/releases) | Preview cut of the 0.25.2 line. |
| [v0.25.2-nightly.20261015.c24e99e9cb](https://github.com/QwenLM/qwen-code/releases) | Includes `fix(managed-agent): admit team_plan_approval into the journal-only interrupted-turn settle` ([#13847](https://github.com/QwenLM/qwen-code/pull/13847)). |
| [v0.25.1-nightly.20261015.415925cbda](https://github.com/QwenLM/qwen-code/releases) | Connector/broker robustness slice; its release workflow failed on the `quality` job ([#14243](https://github.com/QwenLM/qwen-code/issues/14243), closed). |

**Theme:** no breaking changes; essentially all release content is Hosted/managed-agent reliability.

## 3. Hot Issues

1. **[#12380](https://github.com/QwenLM/qwen-code/issues/12380) — proposal(serve): Managed Agent dual-path architecture and staged delivery** *(68 comments, in-progress, P2)*
   The single largest conversation in the tracker. Proposes keeping the TypeScript agent loop while decoupling model inference from tool-environment provisioning, and giving Sessions durable ownership, Workspace bindings, and recoverable tool executions. Effectively the architectural roadmap issue for everything else in `qwen serve`; the comment volume reflects sustained design debate rather than bug triage.

2. **[#14282](https://github.com/QwenLM/qwen-code/issues/14282) — feat(artifacts): record and update session artifacts when agents modify existing files** *(8 comments)*
   Today artifacts are only registered when `write_file` creates a file with an allowlisted extension; updates to existing files depend on the model calling `record_artifact`. Asks for a consistent create-and-update lifecycle so deliverables are tracked reliably. High leverage for anyone building on session artifacts.

3. **[#14234](https://github.com/QwenLM/qwen-code/issues/14234) — Deferred review findings from PR #14021: settle never-started background dispatches in the live turn** *(7 comments)*
   Explicitly deferred items from a merged fix, confirmed against the reviewed head, that change failure-path concurrency semantics. Illustrates a recurring pattern in this repo: review findings deferred into tracked follow-ups rather than blocking merges.

4. **[#13801](https://github.com/QwenLM/qwen-code/issues/13801) — `child_run` stuck at `dispatch_started` never settles when the process provably never started** *(6 comments, closed)*
   A `child_run` commits `run.state: "running"` with `execution: "dispatch_started"` while there is no `:process` ledger row and no cgroup unit — so the public tasks API reports a task as running that never existed. A correctness bug in the observable task state machine; now closed.

5. **[#14063](https://github.com/QwenLM/qwen-code/issues/14063) — Hosted `session_command` timeout text misleads senders; repeating the send duplicates the message** *(6 comments)*
   The timeout evidence text tells a `member_message` sender to "read the board," and retrying duplicates the message. Both a model-facing prompt-quality problem and a real duplication hazard in multi-agent sessions.

6. **[#13063](https://github.com/QwenLM/qwen-code/issues/13063) — Event-driven memory recall during autonomous tool runs** *(5 comments, P3)*
   Memory recall currently fires only at turn entry points carrying query text, so a long autonomous tool run never re-selects memory after the original prompt. Requests an Advisor-triggered mid-run recall. A core context/performance direction.

7. **[#14331](https://github.com/QwenLM/qwen-code/issues/14331) — Large tool output can fill the context window and fail the run instead of degrading gracefully** *(4 comments, P2)*
   When a tool returns a large result near the context limit, the run hard-errors ("model stream ended after a tool result without visible progress") and the user loses the turn. One of the most user-visible reliability complaints this cycle.

8. **[#14288](https://github.com/QwenLM/qwen-code/issues/14288) — Zombie (defunct) session child processes accumulate under long-running/suspended sessions** *(4 comments, P3)*
   A 48-day-old zombie observed on Linux across 0.22.2–0.25.1 with 10–20 parallel sessions per day. Classic long-session resource-leak territory.

9. **[#14289](https://github.com/QwenLM/qwen-code/issues/14289) — feat(serve): auto-remove completed sub-sessions after a configurable TTL** *(4 comments)*
   Sub-sessions created via `create_sub_session` pile up in the Web Shell sidebar with no UI or API removal path. Directly paired with PR [#14335](https://github.com/QwenLM/qwen-code/pull/14335).

10. **[#14314](https://github.com/QwenLM/qwen-code/issues/14314) — Grep rejects valid ripgrep inline flags before running the search** *(3 comments, in-review)*
    Validation parses patterns with JavaScript's `RegExp` although ripgrep executes them with Rust's regex engine, so valid patterns like `(?i)hello` and `(?-i)TODO` are rejected. A clean, high-signal tool-correctness bug with a fix already in review.

*Also active:* [#14278](https://github.com/QwenLM/qwen-code/issues/14278) `/branch` blocked by invisible background requests (closed), [#14310](https://github.com/QwenLM/qwen-code/issues/14310) Python SDK drops buffered messages on CLI failure, [#14312](https://github.com/QwenLM/qwen-code/issues/14312) web terminal stays input-blocked after a large paste, [#14325](https://github.com/QwenLM/qwen-code/issues/14325) false "interrupted request" banner after `/compress`.

## 4. Key PR Progress

1. **[#13805](https://github.com/QwenLM/qwen-code/pull/13805) — feat(core): gate AutoSkill reviews on experience signals**
   AutoSkill reviews become eligible based on *accepted* experience rather than raw tool volume (e.g. ≥5 accepted completed calls with same-tool failure/recovery, or ≥20 with substantive work). Meaningful change to how self-review is triggered.

2. **[#13526](https://github.com/QwenLM/qwen-code/pull/13526) — feat(runtime): add resident Kubernetes Workspace file runtime**
   Opt-in K8s runtime where each tenant/Workspace owns a resident Pod and fixed CSI PVC; closing a Session settles work and revokes tool context while preserving the runtime binding. The infrastructure-heavy corner of the managed-agent roadmap.

3. **[#14319](https://github.com/QwenLM/qwen-code/pull/14319) — feat(managed-agent): wire static Skills and extension configuration**
   Connects static Skills plus extension configuration into managed sessions — the capability layer on top of the Session/mailbox plumbing.

4. **[#14335](https://github.com/QwenLM/qwen-code/pull/14335) — feat(serve): auto-remove completed sub-sessions after a configurable TTL**
   Adds `serve.subSessionCompletionTtlHours` (default 24; `0` preserves today's forever-retain behavior). Directly answers [#14289](https://github.com/QwenLM/qwen-code/issues/14289).

5. **[#14333](https://github.com/QwenLM/qwen-code/pull/14333) — test(managed-agent): FG9a/FG9f `session_message` and `session_command` relay fault gates**
   Multi-process fault-injected gates proving replay-stability for the two cross-Session relay domains on a real control plane. Slice 1 of Stage F FG9 ([#14164](https://github.com/QwenLM/qwen-code/issues/14164)).

6. **[#14334](https://github.com/QwenLM/qwen-code/pull/14334) — test(managed-agent): FG8a/FG8b/FG8d monitor_run and background-shell fault gates**
   Adds lost-reply, store-failure, and cancellation/drain-release-split gates, extending the FG8 work from [#14120](https://github.com/QwenLM/qwen-code/pull/14120).

7. **[#13314](https://github.com/QwenLM/qwen-code/pull/13314) — fix(sdk-java): close Hosted Harness review criticals from #12654**
   Lands fixes for 11 Critical and 2 Minor findings from the post-merge review of the private Java client, plus closures for a second review round filed against the PR itself. Notable example of the repo's long-tail review-debt workflow.

8. **[#14302](https://github.com/QwenLM/qwen-code/pull/14302) — fix(providers): validate daemon credentials and close QwenCloud follow-ups**
   The daemon now validates a provider API key *before* persisting it, rejecting wrong-billing-family keys with `invalid_api_key` instead of failing later. Closes a batch of [#14257](https://github.com/QwenLM/qwen-code/issues/14257) follow-ups on top of [#14073](https://github.com/QwenLM/qwen-code/pull/14073).

9. **[#13997](https://github.com/QwenLM/qwen-code/pull/13997) — fix(cli): hold pre-runtime `/acp` WebSocket upgrades instead of falling through to a 406**
   Fast-path `qwen serve` boots accept HTTP connections before the runtime app exists; upgrade requests now wait rather than returning 406. Fixes the E2E failure tracked in [#13995](https://github.com/QwenLM/qwen-code/issues/13995).

10. **[#14172](https://github.com/QwenLM/qwen-code/pull/14172) — feat(cli): paste clipboard text with Ctrl+V on Linux**
    `Ctrl+V` now reads clipboard *text* when no image is present; previously the only clipboard reader in the TUI was the image path. Small but immediately felt quality-of-life fix.

*Also moving:* [#14277](https://github.com/QwenLM/qwen-code/pull/14277) consolidate server/broker sources (closed), [#14306](https://github.com/QwenLM/qwen-code/pull/14306) align hosted prompts with declared tools (closed), [#14056](https://github.com/QwenLM/qwen-code/pull/14056) recover Hosted journal-only tool batches after crashes, [#14033](https://github.com/QwenLM/qwen-code/pull/14033) widen workspace-lock retry budget to ~5s worst case, [#14202](https://github.com/QwenLM/qwen-code/pull/14202) deflake workflow-completion E2E budgets (30s → 120s), [#13568](https://github.com/QwenLM/qwen-code/pull/13568) route LSP file queries to applicable servers, [#14220](https://github.com/QwenLM/qwen-code/pull/14220) remove mobile composer history arrow buttons.

## 5. Hot Discussions

No Discussion data was provided for this period. *(Section omitted per format.)*

## 6. Feature Request Trends

- **Managed Agent / Hosted serve maturity.** The dominant direction: dual-path architecture ([#12380](https://github.com/QwenLM/qwen-code/issues/12380)), durable Session ownership, mailbox-over-Session-messages, Kubernetes workspace runtimes ([#13526](https://github.com/QwenLM/qwen-code/pull/13526)), and static Skills wiring ([#14319](https://github.com/QwenLM/qwen-code/pull/14319)).
- **Session lifecycle hygiene.** Artifact create-and-update lifecycle ([#14282](https://github.com/QwenLM/qwen-code/issues/14282)), TTL auto-removal of completed sub-sessions ([#14289](https://github.com/QwenLM/qwen-code/issues/14289)), and correct recovery banners ([#14325](https://github.com/QwenLM/qwen-code/issues/14325)).
- **Context and token management with graceful degradation.** Large tool output should degrade rather than hard-fail ([#14331](https://github.com/QwenLM/qwen-code/issues/14331)); event-driven memory recall mid-autonomous-run ([#13063](https://github.com/QwenLM/qwen-code/issues/13063)).
- **Web Shell parity with the terminal.** `ui.customWittyPhrases` / `ui.accessibility.enableLoadingPhrases` ignored in the browser ([#14324](https://github.com/QwenLM/qwen-code/issues/14324)); no sub-session management UI ([#14289](https://github.com/QwenLM/qwen-code/issues/14289)).
- **SDK robustness across languages.** Python SDK message-loss and `ProcessExitError` masking ([#14310](https://github.com/QwenLM/qwen-code/issues/14310)); Java Hosted Harness review criticals ([#13314](https://github.com/QwenLM/qwen-code/pull/13314)); TypeScript SDK now versioned alongside CLI.
- **Tool correctness.** ripgrep-native pattern validation ([#14314](https://github.com/QwenLM/qwen-code/issues/14314)), LSP query routing ([#13568](https://github.com/QwenLM/qwen-code/pull/13568)), and Hosted prompts aligned to declared tools ([#14306](https://github.com/QwenLM/qwen-code/pull/14306)).
- **Platform polish.** Linux clipboard text paste ([#14172](https://github.com/QwenLM/qwen-code/pull/14172)), VS Code companion bundle hygiene ([#14291](https://github.com/QwenLM/qwen-code/issues/14291)).

## 7. Developer Pain Points

1. **Hard failure where graceful degradation is expected.** [#14331](https://github.com/QwenLM/qwen-code/issues/14331): a large tool result near the context limit kills the whole turn rather than summarizing/truncating. Users lose work and must restart.
2. **Opaque or misleading error states.** [#14278](https://github.com/QwenLM/qwen-code/issues/14278) `/branch` claims "a response or tool call is in progress" without naming the invisible blocker (e.g. auto-compact); [#14063](https://github.com/QwenLM/qwen-code/issues/14063) timeout text sends senders to "read the board" and duplicates messages on retry; [#14325](https://github.com/QwenLM/qwen-code/issues/14325) a false "interrupted request" banner after a successful `/compress`.
3. **Resource leaks in long-lived, multi-session workloads.** Zombie child processes surviving up to 48 days ([#14288](https://github.com/QwenLM/qwen-code/issues/14288)) and unbounded sub-session accumulation in the sidebar until daemon restart ([#14289](https://github.com/QwenLM/qwen-code/issues/14289)) — both reported by users running 10–20 parallel sessions daily.
4. **Silent data loss in SDK/terminal paths.** Buffered messages dropped and `ProcessExitError` hidden when the CLI dies between async iterations ([#14310](https://github.com/QwenLM/qwen-code/issues/14310)); the web terminal can stay input-blocked because PTY output is treated as input acknowledgement ([#14312](https://github.com/QwenLM/qwen-code/issues/14312)).
5. **CI and release flakiness.** A steady stream of `qwen-code-dev-bot` main-branch failures ([#12714](https://github.com/QwenLM/qwen-code/issues/12714), [#13995](https://github.com/QwenLM/qwen-code/issues/13995), [#14028](https://github.com/QwenLM/qwen-code/issues/14028), [#14183](https://github.com/QwenLM/qwen-code/issues/14183), [#14189](https://github.com/QwenLM/qwen-code/issues/14189), [#14191](https://github.com/QwenLM/qwen-code/issues/14191)) plus a failed nightly release ([#14243](https://github.com/QwenLM/qwen-code/issues/14243)). Several fixes are deflakes (e.g. [#14202](https://github.com/QwenLM/qwen-code/pull/14202) widening reply budgets 30s → 120s), suggesting shard-load-related timing fragility.
6. **Review debt tracking overhead.** Large consolidated deferral records ([#14234](https://github.com/QwenLM/qwen-code/issues/14234), [#14257](https://github.com/QwenLM/qwen-code/issues/14257), [#13901](https://github.com/QwenLM/qwen-code/issues/13901)) show PRs merging ahead of their review rounds, leaving correctness work to be chased in follow-ups — efficient for velocity, but a growing maintenance tax on the managed-agent area.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*