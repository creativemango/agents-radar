# OpenClaw Ecosystem Digest 2026-10-16

> Issues: 500 | PRs: 500 | Projects covered: 5 | Generated: 2026-10-16 00:40 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# OpenClaw Project Digest — 2026-10-16

## 1. Today's Overview

OpenClaw is in a high-velocity release and hardening cycle. Over the last 24 hours the repo saw **500 issues updated** (393 open/active, 107 closed) and **500 PRs updated** (201 open, 299 merged/closed), plus **4 new releases** spanning the `2026.10.10-beta.1` bleeding edge down to the `2026.9.33` extended-stable (LTS-equivalent) branch. The dominant engineering theme is **Gateway performance and durability** — the beta line is explicitly moving SQLite work off the main thread, while the issue tracker shows an ongoing cluster of WAL/checkpoint, plugin-capture, and update-reliability regressions. Overall project health is strong on throughput (a ~60% PR close rate in the window) but there is meaningful unresolved P0/P1 debt concentrated in storage-layer stability and plugin lifecycle.

## 2. Releases

Four releases shipped in the window:

### 🔷 v2026.10.10-beta.1 (Latest)
- **Highlights:** Faster, steadier Gateway. Chat turns, session lists, startup, and idle Gateways do substantially less SQLite and main-thread work, with more database work moved onto worker threads so busy hosts and large agent fleets stay responsive. (#167965, #168002, #168093, …)
- **Breaking changes:** None stated in release notes.
- **Migration notes:** None stated; standard beta channel behavior applies.

### 🔷 v2026.10.5-beta.4
- **Highlights:** More reliable updates — macOS updates no longer time out on aliased temporary directories; systemd user installs reclaim abandoned runtimes; pacman and FreeBSD installs defer to the package manager; snapshot failures are surfaced before applying.
- **Breaking changes:** None stated.

### 🔷 v2026.10.4 (Stable)
- **Highlights:** Gateway survives computer sleep and system clock jumps instead of restarting itself on a stale ownership heartbeat (#170790). Gateway shutdown/restart no longer hangs on certain paths. *(Notes truncated in provided data.)*
- **Breaking changes:** None stated.

### 🟢 v2026.9.33 (extended-stable / LTS)
- Gateway-only extended-stable release covering end-of-September 2026 OpenClaw plus **critical security updates, reliability and performance fixes**, and new model support. This is the branch recommended for operators who want stability over novelty.

**Release takeaway:** The beta train is churning fast (three beta/stable cuts in the window), which is consistent with the heavy Gateway refactoring underway in PRs like #173504 and #173523. Operators on LTS are being served with security backports rather than feature churn.

## 3. Project Progress

**299 PRs merged/closed in the last 24h** — a strong merge cadence. Notable active work visible in the top-of-queue PRs:

- **[PR #173523](https://github.com/openclaw/openclaw/pull/173523)** — *refactor: simplify infra and plugin lifecycle bookkeeping* (steipete). Removes redundant admission, producer-owned fact copying, and unsupported reentrant coordination from lifecycle paths. Flagged `merge-risk: 🚨 availability`, `security-sensitive-changed`.
- **[PR #173504](https://github.com/openclaw/openclaw/pull/173504)** — *refactor(gateway): remove reload debt and startup claims*. Cuts overlapping reservation/proxy-owner/restart-debt state in Gateway startup and config reload.
- **[PR #173579](https://github.com/openclaw/openclaw/pull/173579)** — *fix: resume interrupted tasks in their original session*. WebChat "Resume" now continues a saved task in the same session, preserving identity, workspace, participants, and children.
- **[PR #173574](https://github.com/openclaw/openclaw/pull/173574)** — *perf(runtime): stop GC telemetry from waking idle workers* (already closed). Directly aligned with the v2026.10.10-beta.1 "steady idle Gateway" theme.
- **[PR #173548](https://github.com/openclaw/openclaw/pull/173548)** — *perf(subagents): release settled run text from memory* (closed). Heap analysis identified ~25 MiB of retained task text across the registry — a memory-pressure fix.
- **[PR #173266](https://github.com/openclaw/openclaw/pull/173266)** — *fix(control-ui): retain cached conversation titles on warm reload*.
- **[PR #173572](https://github.com/openclaw/openclaw/pull/173572)** — *fix(cron): ignore rate-limit cooldowns when Doctor records an automation account*.

**Advanced themes:** Gateway lifecycle simplification, memory reclamation in subagents, Control UI Solid-migration perf cleanups, and CI infrastructure investment (Crabbox runner provisioning via #173581/#173582).

**Notable closures:** Issues #164113 (FICLONE EPERM in unprivileged LXC, P0), #121617 (post-compaction "Already compacted" misclassification, P0), #136262 (openai-completions stream text duplication, P1), and #170332 (beta update continuing after live DB change) all closed in the window.

## 4. Community Hot Topics

Ranked by comment volume in the last 24h:

1. **[Issue #143524](https://github.com/openclaw/openclaw/issues/143524) — 128 comments.** *Agent SQLite WAL grows to 1.4–2.8 GB in days despite `wal_autocheckpoint=1000`; blocks gateway startup (Windows, 2026.9.2/9.3).* P0, `impact:message-loss`, `impact:crash-loop`. Despite the huge comment count, it remains open with `clawsweeper:no-new-fix-pr`, indicating no fix PR is yet attached. **Underlying need:** durable WAL checkpointing under long-lived in-process readers — a recurring storage-architecture problem visible in two other issues below.

2. **[Issue #160959](https://github.com/openclaw/openclaw/issues/160959) — 25 comments.** *Gateway blocks for minutes while capturing large external plugins (2026.9.6 regression).* P0, diamond-lobster rated. **Need:** plugin capture must be incremental/async and must not block the Gateway event loop.

3. **[Issue #148707](https://github.com/openclaw/openclaw/issues/148707) — 20 comments.** *Reply lost with "Reply operation has no active tool authority snapshot" when a second run displaces an in-flight turn.* P1, `impact:session-state`, `impact:message-loss`. **Need:** safe turn displacement / message-loss guarantees on session-state races.

4. **[Issue #157989](https://github.com/openclaw/openclaw/issues/157989) — 18 comments.** *Plugin source capture rewrites ~1.1–1.4 GB per CLI command, ~6.5 GB per Gateway start; severe SSD wear.* P1. **Need:** capture deduplication and reuse across invocations.

5. **[Issue #68596](https://github.com/openclaw/openclaw/issues/68596) — 17 comments, 8 👍 (highest reaction).** *Feature Request: Configurable streaming watchdog timeout threshold.* **Need:** users running long-reasoning models (kimi-k2.5, DeepSeek-R1) need to tune the 30s watchdog, which currently produces spurious "backend may have dropped this run" warnings.

6. **[Issue #164294](https://github.com/openclaw/openclaw/issues/164294) — 17 comments.** *Gateway state DB WAL auto-checkpoint never completes (pinned by in-process reader).* **Need:** same root-cause family as #143524 — an in-process reader is starving checkpoint progress. Directly relevant to the v2026.10.10-beta.1 "move DB work to worker threads" effort.

7. **[PR #173523](https://github.com/openclaw/openclaw/pull/173523)** and **[PR #173504](https://github.com/openclaw/openclaw/pull/173504)** — top-of-queue PRs by review activity, both XL-scope Gateway refactors from the maintainer account (steipete), flagged for availability and security-boundary review.

**Analysis:** The comment-heavy issues cluster tightly around three root causes: (a) SQLite WAL/checkpoint lifecycle under concurrent readers, (b) plugin capture cost (CPU, I/O, SSD wear), and (c) message/session-state loss under turn races. These are exactly the areas the current beta line and Gateway refactors are targeting.

## 5. Bugs & Stability

**P0 / Release-blocker severity (open):**

| Issue | Impact | Status |
|---|---|---|
| [#143524](https://github.com/openclaw/openclaw/issues/143524) — WAL grows to 2.8 GB, blocks startup | message-loss, crash-loop | OPEN, no fix PR, 128 comments |
| [#160959](https://github.com/openclaw/openclaw/issues/160959) — Gateway blocks minutes on plugin capture | crash-loop | OPEN, needs maintainer review |
| [#70903](https://github.com/openclaw/openclaw/issues/70903) — file-based provider cooldown blocks users for hours after billing recovery | auth-provider, UX release blocker | OPEN (stale), no fix PR |
| [#152275](https://github.com/openclaw/openclaw/issues/152275) — post-commit plugin activation failure leaves model catalog & reply dispatch dead until restart | message-loss, crash-loop | OPEN |
| [#171918](https://github.com/openclaw/openclaw/issues/171918) — Safari 27.0.1 intermittently rejects Control UI entry graph | UX release blocker | OPEN, has synthetic repro |
| [#161921](https://github.com/openclaw/openclaw/issues/161921) — after `state-migrated-no-rollback`, `doctor --fix` and `plugins update` deadlock each other | UX release blocker | OPEN |

**P1 severity (open):**

- [#148707](https://github.com/openclaw/openclaw/issues/148707) — reply lost on displaced in-flight turn.
- [#157989](https://github.com/openclaw/openclaw/issues/157989) — plugin capture SSD wear.
- [#159512](https://github.com/openclaw/openclaw/issues/159512) — every subagent completion invalidates requester CLI session twice (claude-cli).
- [#164799](https://github.com/openclaw/openclaw/issues/164799) — agents admit slowly after version change; `readyz` returns 503 until restart.
- [#81182](https://github.com/openclaw/openclaw/issues/81182) — overflow recovery should truncate tool results before waiting the ~900s auto-compaction timeout.
- [#101793](https://github.com/openclaw/openclaw/issues/101793) — assistant text preceding a tool call silently dropped on Signal.
- [#77717](https://github.com/openclaw/openclaw/issues/77717) — Feishu channel bot identity recovery race → permanent disconnection.
- [#170956](https://github.com/openclaw/openclaw/issues/170956) — Windows slow startup, unclean plugin shutdown, `schtasks` permission errors.

**Notable P2 stability items:** [#172091](https://github.com/openclaw/openclaw/issues/172091) (worker SQLite transaction pinned open forever, blocking shared-state checkpoints — filed 2026-10-14, directly adjacent to the beta Gateway work), [#99659](https://github.com/openclaw/openclaw/issues/99659) (OOM kill after companion app connect), [#81484](https://github.com/openclaw/openclaw/issues/81484) (Discord guild reply regression, repeated outbound loops), [#166133](https://github.com/openclaw/openclaw/issues/166133) (MCP integer args coerced to string).

**Closed in window (stability wins):** [#164113](https://github.com/openclaw/openclaw/issues/164113) (FICLONE EPERM in unprivileged LXC), [#121617](https://github.com/openclaw/openclaw/issues/121617) (post-compaction guard misclassification), [#136262](https://github.com/openclaw/openclaw/issues/136262) (openai-completions bare `text_delta` doubling message content), [#170332](https://github.com/openclaw/openclaw/issues/170332) (beta update continued after live DB change), [#77298](https://github.com/openclaw/openclaw/issues/77298) (cron `consecutiveErrors` inflated by restart interruptions).

**Fix PR status:** Most P0 issues carry `clawsweeper:no-new-fix-pr` — i.e., **no fix PR attached yet**. The exceptions are #164113 and #121617, both now closed. The active Gateway refactor PRs (#173504, #173523) plausibly subsume several of the WAL/lifecycle fixes indirectly but are not explicitly linked as fixes.

## 6. Feature Requests & Roadmap Signals

High-signal feature requests, grouped by likely roadmap proximity:

**Likely to land soon (aligned with active PR/refactor work):**
- [#79902](https://github.com/openclaw/openclaw/issues/79902) — companion-friendly SQLite transcript/session seams on top of the database-first runtime. The Control UI Solid migration and search-capability refactor ([PR #172681](https://github.com/openclaw/openclaw/pull/172681)) move in this direction.
- [#172091](https://github.com/openclaw/openclaw/issues/172091) / [#164294](https://github.com/openclaw/openclaw/issues/164294) — WAL/checkpoint correctness. The v2026.10.10-beta.1 "worker-thread DB" thrust is the natural vehicle.
- [#81182](https://github.com/openclaw/openclaw/issues/81182) — truncate tool results before the full auto-compaction timeout. Low-risk, high-UX-value; memory-core is already an active PR target ([#172681](https://github.com/openclaw/openclaw/pull/172681)).

**Plausible next-cycle candidates (clear scope, moderate effort):**
- [#68596](https://github.com/openclaw/openclaw/issues/68596) (8 👍) — configurable streaming watchdog timeout. Strong user demand from reasoning-model users.
- [#67413](https://github.com/openclaw/openclaw/issues/67413) (5 👍) — per-agent dreaming configuration to prevent OOM during cron memory sweeps.
- [#67419](https://github.com/openclaw/openclaw/issues/67419) — stop re-injecting bootstrap files every turn (20–30% token waste). Pairs with the context-budget issues.
- [#71142](https://github.com/openclaw/openclaw/issues/71142) — configurable upload size limit for Control UI (currently hardcoded 5 MB).
- [#71452](https://github.com/openclaw/openclaw/issues/71452) — pagination for `message list` / `channel-list` instead of hardcoded 25-item truncation.

**Longer-horizon / architectural:**
- [#63990](https://github.com/openclaw/openclaw/issues/63990) — multi-index embedding memory with model-aware failover (no mixed vector spaces).
- [#138245](https://github.com/openclaw/openclaw/issues/138245) — output guards to catch pseudo tool-calls emitted as plain text before they poison session history.
- [#66252](https://github.com/openclaw/openclaw/issues/66252) — per-agent TTS/STT overrides for multi-language deployments.
- [#103764](https://github.com/openclaw/openclaw/issues/103764) — Kotlin Multiplatform shared mobile logic.
- [#63930](https://github.com/openclaw/openclaw/issues/63930) — Anthropic advisor beta server-side tool support.

**Prediction for next version:** Given the v2026.10.10-beta.1 Gateway-perf direction, expect the next betas to include (a) further WAL/checkpoint fixes tied to worker-thread DB work, (b) plugin-capture reuse/dedup work, and (c) incremental Control UI Solid migration completions. The streaming-watchdog configurability request (#68596) has the clearest user-demand-to-effort ratio and is a strong candidate for inclusion.

## 7. User Feedback Summary

**Satisfaction signals:**
- Users are actively stress-testing releases and filing detailed, high-quality repros (diamond- and lobster-rated issues), which reflects an engaged, technically sophisticated community.
- Reactions skew positive on feature requests that propose configurability (8 👍 on #68596, 5 👍 on #67413), suggesting users trust the maintainers to respond to concrete, well-scoped asks.
- The extended-stable / LTS line (#v2026.9.33) is a mature operational signal that the project is serving conservative production users, not just tinkerers.

**Pain points (ranked):**
1. **Storage-layer instability dominates.** The three highest-comment issues (#143524, #164294, #172091) are all variants of the same root cause: SQLite WAL checkpoints starve behind long-lived readers. Users report multi-gigabyte WAL files, SSD wear, and Gateway startup failures — some have been open since September and are P0.
2. **Performance regressions from plugin capture** (#160959, #157989) — minutes-long Gateway stalls and ~6.5 GB of I/O per Gateway start. Users on large plugin trees are materially blocked.
3. **Update reliability** — macOS temp-dir timeouts, LXC seccomp/FICLONE failures, systemd abandonment, and `state-migrated-no-rollback` deadlocks. Multiple releases in the window are dedicated to this, indicating the maintainers hear the pain.
4. **Session/message loss races** — displaced turns, dropped Signal text, subagent completion side-effects (#148707, #101793, #159512, #96975). Users perceive non-determinism in reply delivery, which is the worst class of bug for a personal-assistant product.
5. **Token/context economy** — bootstrap re-injection (#67419), context-overflow over-counting ([#101929](https://github.com/openclaw/openclaw/issues/101929)), and Gemini `textSignature` bloat ([#48709](https://github.com/openclaw/openclaw/issues/48709)) all translate directly into user cost and latency.

**Overall sentiment:** Users are invested and precise, but the volume of P0/P1 storage and lifecycle bugs suggests production operators are being asked to absorb real risk. The rapid beta cadence and explicit LTS branch are correct mitigations, but closing the WAL cluster should be the top priority.

## 8. Backlog Watch

Issues and PRs that are important, long-standing, and/or awaiting maintainer attention:

**Storage-layer (highest priority — merge into a single workstream):**
- [#143524](https://github.com/openclaw/openclaw/issues/143524) — WAL growth to 2.8 GB. Open since **2026-09-09**, 128 comments, no fix PR. The single most-discussed issue in the tracker.
- [#164294](https://github.com/openclaw/openclaw/issues/164294) — WAL auto-checkpoint never completes. Open since **2026-10-03**, 17 comments.
- [#172091](https://github.com/openclaw/openclaw/issues/172091) — worker SQLite transaction pinned open forever. Filed **2026-10-14**; newest member of the WAL family and the most actionable for the current beta.

**Long-stale but consequential:**
- [#68596](https://github.com/openclaw/openclaw/issues/68596) — configurable streaming watchdog. Open since **2026-04-18** (~6 months), highest reaction count in the top-50. `stale` tagged but clearly still relevant.
- [#70903](https://github.com/openclaw/openclaw/issues/70903) — persistent provider cooldown blocks users after billing recovery. Open since **2026-04-24**, P0, stale, no fix PR attached.
- [#67419](https://github.com/openclaw/openclaw/issues/67419) — bootstrap-file re-injection wasting 20–30% of context every turn. Open since **2026-04-15**, P2.
- [#67413](https://github.com/openclaw/openclaw/issues/67413) — per-agent dreaming config (OOM prevention). Open since **2026-04-15**, 5 👍.
- [#69242](https://github.com/openclaw/openclaw/issues/69242) — Linux `exec` SIGKILLs broad find/grep without OOM evidence. Open since **2026-04-20**, stale, needs info.
- [#72015](https://github.com/openclaw/openclaw/issues/72015) — active-memory blocks replies on multi-agent Gateways. Open since **2026-04-26**.

**PRs needing maintainer attention:**
- [PR #173523](https://github.com/openclaw/openclaw/pull/173523) — `status: ⏳ waiting on author`, `merge-risk: 🚨 availability`, `security-sensitive-changed`, XL scope. This is a load-bearing refactor; review latency here gates downstream fixes.
- [PR #173504](https://github.com/openclaw/openclaw/pull/173504) — same flags as above; Gateway reload debt removal is a prerequisite for predictable startup behavior.
- [PR #171990](https://github.com/openclaw/openclaw/pull/171990) — *fix: silence replies ending in a standalone `NO_REPLY`*. Open since **2026-10-14**, P1, touches every major channel (Discord, Matrix, Mattermost, Teams, Slack, Telegram, Feishu). Broad blast radius across messaging surfaces.
- [PR #157806](https://github.com/openclaw/openclaw/pull/157806) — *fix(sessions): preserve requester identity at execution admission*. Open since **2026-09-25**, P1, `security-sensitive-changed`, `merge-risk: 🚨 security-boundary`. Long-running and identity/permission-critical.
- [PR #172681](https://github.com/openclaw/openclaw/pull/172681) — *refactor(search): engine-owned full-text and vector capabilities*. Open since **2026-10-15**, XL, memory-core — relevant to several search/memory feature requests above.

---

**Bottom line:** OpenClaw's release velocity and PR throughput are healthy, and the v2026.10.10-beta.1 line is correctly aimed at the Gateway's biggest structural weakness (synchronous SQLite and plugin-capture work on the main thread). The main health risk is the **unresolved WAL/checkpoint cluster** (#143524, #164294, #172091), which has P0 severity, months of open time, and no linked fix PR — it should be the explicit anchor of the next release cycle. Secondary watch items are the long-stale P0 provider-cooldown bug (#70903) and the broad-blast-radius `NO_REPLY` PR (#171990).

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — Personal AI Assistant / Agent OSS Ecosystem  
**Date:** 2026-10-16  
**Projects:** OpenClaw, Hermes Agent, IronClaw, QwenPaw, ZeroClaw

## 1. Ecosystem Overview

The personal AI assistant / agent open-source landscape is splitting into three tiers: OpenClaw at hyperscale, Hermes/QwenPaw/ZeroClaw in active productization, and IronClaw in a quiet eval-focused niche. Across the active projects, the dominant engineering pressure is the **gateway/session layer**: single-owner session state, durable storage, plugin lifecycle, multi-channel delivery, and provider routing. Security and observability are shifting from optional features to release gates, especially around plugins, MCP/approval flows, credentials, and multi-user identity. Contribution volume remains healthy in four projects, but merge/review throughput — not contributor interest — is the bottleneck outside OpenClaw.

## 2. Activity Comparison

| Project | Issues Updated | PRs Updated | Release Status | Health Score* |
|---|---:|---:|---|---:|
| **OpenClaw** | 500 (393 open, 107 closed) | 500 (201 open, 299 merged/closed) | 4 releases: `2026.10.10-beta.1`, `2026.10.5-beta.4`, `2026.10.4` stable, `2026.9.33` LTS | **8.0 / 10** |
| **Hermes Agent** | 50 (48 open, 2 closed) | 50 (46 open, 4 merged/closed) | 1 corrective release: `v0.22.2` | **6.5 / 10** |
| **IronClaw** | 1 (1 open, 0 closed) | 0 (0 open, 0 merged/closed) | None | **3.5 / 10** |
| **QwenPaw** | 29 (22 open, 7 closed) | 23 (20 open, 3 merged/closed) | 1 stable release: `v2.2.2` | **7.0 / 10** |
| **ZeroClaw** | 27 (27 open, 0 closed) | 50 (48 open, 2 closed) | None | **5.5 / 10** |

\*Health score is a qualitative composite across activity, merge velocity, release cadence, open P0/P1 severity, and backlog trend. It is not a vendor metric.

## 3. OpenClaw’s Position

OpenClaw is the ecosystem’s scale leader by a wide margin: **500 issues and 500 PRs updated in 24h**, with **299 PRs merged/closed** and **four releases** including an extended-stable/LTS branch. Its peer activity is roughly 10x lower for Hermes, QwenPaw, and ZeroClaw, and effectively dormant for IronClaw.

**Advantages vs peers:**
- Largest community and maintainer throughput.
- Mature LTS/stable/beta release structure.
- Broadest operational surface: Gateway, Control UI, plugins, channels, cron, subagents, storage, updates.
- Strong user sophistication: detailed P0/P1 repros, diamond/lobster-rated reports.

**Technical approach differences:**
- Database-first runtime with explicit movement of SQLite work off the main thread onto worker threads.
- Heavy Gateway lifecycle refactoring to remove reload debt and improve startup/idle behavior.
- Plugin-capture and lifecycle bookkeeping being simplified, but still a major risk area.
- LTS branch serves conservative operators; beta line carries aggressive Gateway hardening.

**Key risk:** The unresolved **SQLite WAL/checkpoint cluster** (`#143524`, `#164294`, `#172091`) is P0, months old, and has no linked fix PR. Plugin capture cost and update reliability are secondary but material. OpenClaw is the most mature project, but its storage-layer debt is the ecosystem’s largest single reliability risk.

## 4. Shared Technical Focus Areas

| Requirement | Projects | Specific Needs |
|---|---|---|
| **Gateway/session lifecycle correctness** | OpenClaw, Hermes, QwenPaw, ZeroClaw | Single-owner sessions, safe turn displacement, no silent cron/agent cancellation, restart resilience, process supervision. |
| **Durable storage/checkpointing** | OpenClaw, ZeroClaw, Hermes, QwenPaw | WAL checkpoint completion, persistent config writes, session-state durability, no message loss under races. |
| **Plugin/extension lifecycle** | OpenClaw, ZeroClaw, QwenPaw, Hermes | Async/dedup plugin capture, hot reload/unload, WASM isolation, egress denial, MCP grants, rollback-safe updates. |
| **Channel/messaging reliability** | OpenClaw, ZeroClaw, QwenPaw, Hermes | Idempotent delivery, attachment handling, auto-reply loops, `\Seen` state, localisation, no dropped Signal/Discord/email/WhatsApp messages. |
| **Observability/progress visibility** | ZeroClaw, QwenPaw, OpenClaw, Hermes | Tool receipts, partial output, cross-device message visibility, configurable watchdogs, trustworthy release metadata. |
| **Provider/model routing and streaming** | Hermes, QwenPaw, ZeroClaw, OpenClaw, IronClaw | Atomic provider/model overrides, streaming correctness, parser compatibility, stale `base_url` fixes, model-quality eval. |
| **Security/identity/credentials** | ZeroClaw, Hermes, OpenClaw, QwenPaw | OIDC, multi-user isolation, approval tokens, dependency advisories, credential scoping, secret redaction. |

## 5. Differentiation Analysis

| Project | Target Users | Feature Focus | Technical Architecture |
|---|---|---|---|
| **OpenClaw** | Production operators, large agent fleets, power users | Broad personal assistant platform; Gateway, Control UI, channels, cron, subagents, LTS | Database-first runtime; SQLite moving to worker threads; monolithic Gateway with LTS/beta split |
| **Hermes Agent** | Multi-surface local users, homelab, multi-provider setups | Unified gateway across CLI/TUI/Desktop/ACP/cron/bots; MCP and approval security | Local session gateway owns all surfaces; provider-routing and credential hardening |
| **IronClaw** | Benchmark/eval teams, model-quality researchers | Failure taxonomy, officeqa diagnostics, model-quality triage | Eval/benchmark harness; not an end-user assistant platform |
| **QwenPaw** | Multi-device, multi-agent desktop users; third-party harness users | Console/desktop/chat terminal, skill pool, plugin hub, multi-agent orchestration | AgentScope-based console + Hub + desktop; plugin catalog and ACP sessions |
| **ZeroClaw** | Self-hosted, security-conscious operators; Rust/WASM ecosystem | WASM plugins, channels (email/WhatsApp/Telegram/Slack), OIDC/multi-user | Rust runtime + gateway separation; WASM plugin host; pre-release v0.8.6/v0.9.0 |

## 6. Community Momentum & Maturity

**Activity tiers:**
1. **Hyper-scale / mature:** OpenClaw — very high velocity, four releases, LTS, but significant P0 storage debt.
2. **Active productization / stabilizing:** Hermes, QwenPaw, ZeroClaw.
   - **Hermes:** Post-unified-gateway fallout; corrective release; targeted fix PRs; trust issues from wrong-commit release.
   - **QwenPaw:** Stable `v2.2.2`; strong console/desktop work; but growing backlog and critical lifecycle/cancellation bugs.
   - **ZeroClaw:** Very high contribution volume, but **48 open PRs vs 2 closed**; review-starved; no release in window.
3. **Low activity / niche:** IronClaw — 1 issue, 0 PRs, 0 releases; benchmark-focused and quiet.

**Rapidly iterating:** OpenClaw, ZeroClaw, QwenPaw.  
**Stabilizing:** Hermes, OpenClaw LTS branch.  
**Maturity ranking:** OpenClaw > QwenPaw ≈ Hermes > ZeroClaw > IronClaw.

## 7. Trend Signals

1. **Gateway consolidation is the new architecture battleground.** Projects are converging on a single gateway that owns sessions across CLI, desktop, bots, cron, and subagents. The hard problems are ownership, restart resilience, and avoiding silent cancellations.
2. **Storage work is moving off the main thread.** OpenClaw’s beta explicitly moves SQLite to worker threads; peers show related durability and config-write concerns. Async storage is becoming a baseline expectation for responsive agent fleets.
3. **Plugin lifecycle is a security and reliability surface.** OpenClaw, ZeroClaw, QwenPaw, and Hermes all have plugin/MCP/approval work in flight. Needs: deduplicated capture, hot reload, rollback safety, egress control, and supply-chain patching.
4. **Multi-channel reliability is table stakes.** Email loops, WhatsApp voice-note drops, Signal text loss, Discord async result failures, and localisation fallbacks are recurring user-facing failures. Idempotency and delivery guarantees are now core product requirements.
5. **Observability is a UX feature, not just diagnostics.** Users want tool receipts, partial output, progress streams, and cross-device visibility. Silent long-running turns are the most repeated UX complaint in ZeroClaw and QwenPaw.
6. **Provider routing and streaming correctness remain fragile.** Stale `base_url`, forced non-streaming paths, OpenCode session headers, DSML parsing, and configurable watchdogs show that model/provider abstraction is still immature.
7. **Context/token economy is a direct cost and latency lever.** Bootstrap re-injection, oversized tool results, base64 bloat, and output bounding are active concerns across OpenClaw, QwenPaw, and ZeroClaw.
8. **Identity, multi-user, and credential governance are rising.** ZeroClaw’s OIDC/multi-user trackers, Hermes’ MCP/approval security, and OpenClaw’s security-sensitive PRs indicate enterprise/self-hosted deployments are pulling security forward.
9. **Platform parity matters.** Windows startup, macOS LaunchAgent identity, Linux socket paths, and non-English locales appear as recurring friction. Cross-platform reliability is a maturity signal.
10. **Eval/model-quality taxonomies are emerging.** IronClaw’s failure taxonomy shows a need for systematic benchmark diagnostics, especially where failures are model-quality rather than platform bugs.

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-10-16

## 1. Today's Overview
Hermes Agent is in a high-activity stabilization phase. In the last 24 hours, 50 issues and 50 PRs were updated: 48 issues remain open/active, 46 PRs are open, and 4 PRs were merged/closed. The dominant theme is post-merge fallout from unified gateway PR [#106742](https://github.com/NousResearch/hermes-agent/pull/106742), tracked by issue [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) with 69 comments and several fix-package issues. A corrective release, [v0.22.2](https://github.com/NousResearch/hermes-agent/releases/tag/v0.22.2), was published to replace v0.22.1, which was built from the wrong commit. Overall project health is active but elevated-risk: maintainers and contributors are responding with targeted PRs, but P1/P2 gateway, provider-routing, platform, and security issues remain.

## 2. Releases
### v0.22.2 — October 15, 2026
- **Link:** [v0.22.2](https://github.com/NousResearch/hermes-agent/releases/tag/v0.22.2)
- **Changes:** v0.22.2 is a corrective release. v0.22.1 was built from the wrong commit, shipped unreleased gateway work, and the Microsoft Store app could stick on “Connecting.”
- **Breaking changes:** None explicitly stated in the provided release note.
- **Migration notes:** Users on v0.22.1 should update to v0.22.2. The release note flags a known issue for sessions that ran under the 0.22.1 gateway, but the provided excerpt is truncated.
- **Related release metadata issue:** The CLI/banner release date may still show a stale date because of [#135217](https://github.com/NousResearch/hermes-agent/issues/135217) and the now-closed duplicate [#139514](https://github.com/NousResearch/hermes-agent/issues/139514).

## 3. Project Progress
- **Merged/closed PRs:** 4 PRs were merged or closed in the last 24h, but the provided top-20 list contains only open PRs, so the specific merged PRs cannot be identified from this data.
- **Unified gateway follow-up:** [#106742](https://github.com/NousResearch/hermes-agent/pull/106742) merged on Oct 14. Post-merge fixes are being organized through [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) and package issues:
  - [#139158](https://github.com/NousResearch/hermes-agent/issues/139158) — classic CLI, 16 findings
  - [#139160](https://github.com/NousResearch/hermes-agent/issues/139160) — session creation, model picks, credentials, 17 findings
  - [#139161](https://github.com/NousResearch/hermes-agent/issues/139161) — Ink TUI and Desktop sessions, commands, live events, 19 findings
  - [#139162](https://github.com/NousResearch/hermes-agent/issues/139162) — messaging bots, cron, Kanban workers, 10 findings
- **Closed issue:** [#139514](https://github.com/NousResearch/hermes-agent/issues/139514) was closed as a duplicate; the underlying stale release-date issue remains open in [#135217](https://github.com/NousResearch/hermes-agent/issues/135217).
- **Active fix PRs in flight:** [#139521](https://github.com/NousResearch/hermes-agent/pull/139521) cron inactivity, [#139522](https://github.com/NousResearch/hermes-agent/pull/139522) browser fallback tools, [#139524](https://github.com/NousResearch/hermes-agent/pull/139524) desktop fallback-model pickers, [#139353](https://github.com/NousResearch/hermes-agent/pull/139353) Azure CLI auth, [#139519](https://github.com/NousResearch/hermes-agent/pull/139519) Windows bot turn lock, [#139358](https://github.com/NousResearch/hermes-agent/pull/139358) Docker agent paths, [#130459](https://github.com/NousResearch/hermes-agent/pull/130459) Anthropic credential benching, [#117280](https://github.com/NousResearch/hermes-agent/pull/117280) Discord admin actions, and [#139516](https://github.com/NousResearch/hermes-agent/pull/139516) plugin-catalog context engine.

## 4. Community Hot Topics
- [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) — **69 comments.** Tracking post-merge fixes for the unified gateway. Underlying need: stabilize one gateway owning local sessions across CLI, TUI, Desktop, ACP, cron, bots, Windows, session state, and message delivery.
- [#125095](https://github.com/NousResearch/hermes-agent/issues/125095) — **8 comments, 1 👍.** `claude-subscription-directsdk` is forced onto the non-streaming path, so the 240s stale watchdog kills healthy long Opus responses. Need: provider plugin streaming correctness.
- [#28024](https://github.com/NousResearch/hermes-agent/issues/28024) — **7 comments.** Feature request for a whitelisted `config_set` tool so agents can safely self-configure non-security settings. Need: agent autonomy with config safety.
- [#138843](https://github.com/NousResearch/hermes-agent/issues/138843) — **7 comments, 1 👍.** Local `session.create` provider override keeps stale `model.base_url`, sending a new provider’s key to the old endpoint and producing a 401. Need: atomic provider/model route updates.
- [#123118](https://github.com/NousResearch/hermes-agent/issues/123118) — **5 comments.** macOS gateway LaunchAgent appears as opaque `osascript` in Privacy & Security. Need: identifiable Hermes background services.
- [#135217](https://github.com/NousResearch/hermes-agent/issues/135217) — **5 comments.** `hermes --version` and banner show a stale release date. Need: trustworthy build/release metadata.
- [#133744](https://github.com/NousResearch/hermes-agent/issues/133744) — **5 comments.** Post-reboot PID reuse within start-tick drift tolerance wedges the supervised gateway forever. Need: resilient process supervision.
- [#113094](https://github.com/NousResearch/hermes-agent/issues/113094) — **5 comments.** `browser_vault_enter_code` misses split OTP widgets without `maxlength`/`autocomplete`. Need: better browser-auth automation.

## 5. Bugs & Stability
Ranked by severity. This is not exhaustive; 50 issues were updated and 48 remain active.

### P1
- [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) — **Tracking: one gateway owns every local session — post-merge fixes.** Affects agent, CLI, gateway, TUI, ACP, cron, Desktop, Windows, sessions, profiles. Fix work is split across [#139158](https://github.com/NousResearch/hermes-agent/issues/139158), [#139160](https://github.com/NousResearch/hermes-agent/issues/139160), [#139161](https://github.com/NousResearch/hermes-agent/issues/139161), and [#139162](https://github.com/NousResearch/hermes-agent/issues/139162).

### P2
- [#138843](https://github.com/NousResearch/hermes-agent/issues/138843) — Provider override keeps stale `base_url`, causing 401s. Package/fix path: [#139160](https://github.com/NousResearch/hermes-agent/issues/139160).
- [#123118](https://github.com/NousResearch/hermes-agent/issues/123118) — macOS LaunchAgent shows as `osascript`, not identifiable as Hermes.
- [#133744](https://github.com/NousResearch/hermes-agent/issues/133744) — Post-reboot PID reuse wedges supervised gateway.
- [#132492](https://github.com/NousResearch/hermes-agent/issues/132492) — 1Password vault fill cannot determine page origin.
- [#134394](https://github.com/NousResearch/hermes-agent/issues/134394) — Dependency pins carry open security advisories: PyJWT 2.13.0, httpx2 2.7.0, urllib3 2.7.0, oauthlib 3.3.1.
- [#139091](https://github.com/NousResearch/hermes-agent/issues/139091) — Stable update 10s merge-base probe times out even when both commits are local.
- [#137843](https://github.com/NousResearch/hermes-agent/issues/137843) — Desktop workspace file panel stuck loading after switching sessions.
- [#138261](https://github.com/NousResearch/hermes-agent/issues/138261) — Model named by `model.default` is rejected by model-switch validator.
- [#139135](https://github.com/NousResearch/hermes-agent/issues/139135) — Discord async child completes but durable result stays pending; parent does not resume.
- [#139429](https://github.com/NousResearch/hermes-agent/issues/139429) — `hermes update` with backup enabled places root-owned backup files inside profile backup directories.
- [#70387](https://github.com/NousResearch/hermes-agent/issues/70387) — OpenRouter `response_cache` defaults to enabled, risking replay of stale tool calls.
- [#137645](https://github.com/NousResearch/hermes-agent/issues/137645) — Windows domain-joined gateway install fails when user is a local account due to `USERDOMAIN` poisoning.
- [#122786](https://github.com/NousResearch/hermes-agent/issues/122786) — Browser tools fail on Linux when scratch + task-id session names exceed agent-browser’s 103-byte socket cap.

### P3
- [#139138](https://github.com/NousResearch/hermes-agent/issues/139138) — Discord/bot-mailbox receipts with ambiguous `permission_denied` status rejected as unknown schema on every gateway scan.
- [#139492](https://github.com/NousResearch/hermes-agent/issues/139492) — `/context` displays invented 256K fallback as real context window.
- [#138785](https://github.com/NousResearch/hermes-agent/issues/138785) — Anthropic `AuthError` has no code, so kanban worker exits 1 instead of 75 and charges failure breaker.
- [#120263](https://github.com/NousResearch/hermes-agent/issues/120263) — Azure Foundry `gpt-6-luna` requires Responses API when function tools and reasoning are enabled.
- [#113094](https://github.com/NousResearch/hermes-agent/issues/113094) — Browser vault OTP entry misses split widgets.
- [#135217](https://github.com/NousResearch/hermes-agent/issues/135217) — Stale release date in `hermes --version` and banner.
- [#139514](https://github.com/NousResearch/hermes-agent/issues/139514) — Closed as duplicate; stale `__release_date__` remains.

### Fix PRs worth noting
Targeted open PRs exist for several stability/security areas: [#139521](https://github.com/NousResearch/hermes-agent/pull/139521) cron inactivity, [#139522](https://github.com/NousResearch/hermes-agent/pull/139522) browser fallback, [#139524](https://github.com/NousResearch/hermes-agent/pull/139524) desktop profile fallback pickers, [#139353](https://github.com/NousResearch/hermes-agent/pull/139353) Azure CLI auth, [#139519](https://github.com/NousResearch/hermes-agent/pull/139519) Windows bot turn lock, [#139358](https://github.com/NousResearch/hermes-agent/pull/139358) Docker container paths, [#130459](https://github.com/NousResearch/hermes-agent/pull/130459) Anthropic plan-window benching, [#95756](https://github.com/NousResearch/hermes-agent/pull/95756) MCP durable grants, [#112415](https://github.com/NousResearch/hermes-agent/pull/112415) approval payload tokens, [#116140](https://github.com/NousResearch/hermes-agent/pull/116140) cron monitor changes, [#136635](https://github.com/NousResearch/hermes-agent/pull/136635) MCP auth parks, [#136636](https://github.com/NousResearch/hermes-agent/pull/136636) MCP OAuth providers, [#136634](https://github.com/NousResearch/hermes-agent/pull/136634) approval deny rules, [#132395](https://github.com/NousResearch/hermes-agent/pull/132395) browser stderr tail, [#136637](https://github.com/NousResearch/hermes-agent/pull/136637) CDP URL handling, [#136638](https://github.com/NousResearch/hermes-agent/pull/136638) CDP redial logging, [#136639](https://github.com/NousResearch/hermes-agent/pull/136639) vault card expiry dates, and [#137370](https://github.com/NousResearch/hermes-agent/pull/137370) vault card secret redaction.

## 6. Feature Requests & Roadmap Signals
- [#28024](https://github.com/NousResearch/hermes-agent/issues/28024) — Whitelisted `config_set` tool for agent self-configuration. P3, 7 comments, `needs-decision`.
- [#107937](https://github.com/NousResearch/hermes-agent/issues/107937) — Per-task provider/model pinning on `delegate_task` for cross-provider subagents. P3, `needs-decision`.
- [#90435](https://github.com/NousResearch/hermes-agent/issues/90435) — Unify Desktop Preview and Browser into a human-agent Workspace Viewer. P3, innovation.
- [#80828](https://github.com/NousResearch/hermes-agent/issues/80828) — Desktop UI to view/select active Copilot credential when multiple are configured. P3.
- [#117280](https://github.com/NousResearch/hermes-agent/pull/117280) — Discord channel, role, and server management actions. Open PR.
- [#139516](https://github.com/NousResearch/hermes-agent/pull/139516) — Plugin catalog addition: `hermes-vcc-engine`. Open PR.
- [#139353](https://github.com/NousResearch/hermes-agent/pull/139353) — Profile-scoped Azure CLI authentication for Foundry. Open PR.
- [#130459](https://github.com/NousResearch/hermes-agent/pull/130459) — Bench Anthropic credential when plan window is spent. Open PR.

**Prediction:** The next patch, likely v0.22.3, will be dominated by unified-gateway post-merge fixes, release metadata correction, Windows/Docker path fixes, and MCP/approval-security hardening. Feature requests with open PRs — Discord admin actions, plugin catalog, Azure CLI auth, Anthropic credential benching — have a plausible path into an upcoming release if reviewed and merged. The `needs-decision` items (`config_set`, delegate provider pinning, Workspace Viewer) are less likely in the immediate next patch.

## 7. User Feedback Summary
- **Pain points:** Unified gateway regressions affecting sessions, message delivery, compatibility, and Windows; trust issues from the wrong-commit v0.22.1 release and stale release date; provider routing errors producing 401s; platform-specific breakages on macOS Privacy & Security, Windows domain-joined installs, and Linux browser sockets; open security advisories in pinned dependencies; browser-automation gaps for OTP and vault origin; Desktop file panel stuck loading; Discord async delegate results not resuming; root-owned backup files inside profile backup directories.
- **Satisfaction signals:** Maintainers are actively triaging, decomposing gateway fallout into package issues, and opening targeted PRs. Reactions are generally low, with a few issues receiving 1 👍.
- **Dissatisfaction signals:** Blunt user feedback on backup-file behavior, opaque `osascript` identity on macOS, stuck UI panels, 401 provider errors, gateway process wedging, and stale version output.
- **Real use cases:** Multi-provider homelab subagents, voice/STT/TTS configuration, browser-based 1Password/OTP flows, Discord bot management, Azure Foundry, Copilot credential selection, and desktop workspace previews.

## 8. Backlog Watch
Long-unanswered or high-impact items needing maintainer attention:
- [#28024](https://github.com/NousResearch/hermes-agent/issues/28024) — Created May 18; `config_set` tool still open with 7 comments.
- [#70387](https://github.com/NousResearch/hermes-agent/issues/70387) — Created Jul 23; P2 OpenRouter `response_cache` stale-tool-call risk.
- [#80828](https://github.com/NousResearch/hermes-agent/issues/80828) — Created Aug 7; Desktop Copilot credential UI, only 1 comment.
- [#90435](https://github.com/NousResearch/hermes-agent/issues/90435) — Created Aug 20; Workspace Viewer proposal, 3 comments.
- [#95756](https://github.com/NousResearch/hermes-agent/pull/95756) — Created Aug 26; P2 MCP durable grants PR, security boundary.
- [#112415](https://github.com/NousResearch/hermes-agent/pull/112415) — Created Sep 15; approval payload token PR, security-sensitive.
- [#113094](https://github.com/NousResearch/hermes-agent/issues/113094) — Created Sep 16; split OTP widgets, 5 comments.
- [#116140](https://github.com/NousResearch/hermes-agent/pull/116140) — Created Sep 19; P2 cron monitor changes across admission refusals.
- [#120263](https://github.com/NousResearch/hermes-agent/issues/120263) — Created Sep 23; Azure Foundry Responses API requirement.
- [#122786](https://github.com/NousResearch/hermes-agent/issues/122786) — Created Sep 25; P2 browser socket path length failure.
- [#123118](https://github.com/NousResearch/hermes-agent/issues/123118) — Created Sep 25; P2 macOS LaunchAgent privacy/identity.
- [#125095](https://github.com/NousResearch/hermes-agent/issues/125095) — Created Sep 27; streaming path bug for `claude-subscription-directsdk`, 8 comments.
- [#132492](https://github.com/NousResearch/hermes-agent/issues/132492) — Created Oct 3; P2 1Password vault origin determination.
- [#133744](https://github.com/NousResearch/hermes-agent/issues/133744) — Created Oct 6; P2 PID reuse wedges gateway.
- [#134394](https://github.com/NousResearch/hermes-agent/issues/134394) — Created Oct 7; P2 dependency security advisories.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-10-16

## 1. Today's Overview
IronClaw activity for 2026-10-16 was minimal: 1 issue was updated in the last 24h, while PR activity was 0 open/merged/closed and there were 0 new releases. The only active item is open issue #8138, a daily failure taxonomy for 2026-10-15, with 0 comments and 0 reactions. No code changes, release artifacts, or issue closures were recorded in the reporting window. From a code-flow perspective, project health appears quiet/stable, but engineering throughput and community engagement are very low. The single active issue concerns benchmark failure classification rather than a confirmed code defect or feature request.

## 2. Releases
No new releases in the reporting window. No changelog, breaking changes, migration notes, or release assets to report.

## 3. Project Progress
No merged or closed PRs today, so no features advanced and no fixes landed. No issues were closed. Project progress for this window: none recorded.

## 4. Community Hot Topics
- **#8138 [OPEN] Daily ironclaw failure taxonomy — 2026-10-15**  
  Author: pranavraja99 | Comments: 0 | 👍: 0  
  Link: https://github.com/nearai/ironclaw/issues/8138  
  This is the only active issue, though it is not “hot” by comments or reactions. The underlying need is systematic tracking and classification of benchmark failures, especially the linked officeqa run with 50 non-pass tasks. The issue summary suggests those failures are “overwhelmingly genuine model-quality errors” involving DeepSeek-V4-Flash, which points to eval/model-quality triage rather than immediate IronClaw code instability.  
  Benchmark link: https://nearai.github.io/benchmarks/#/runs/ironclaw/officeqa/417ae5e9-500f-43f5-8a6b-91f739687305

## 5. Bugs & Stability
No explicit bug, crash, or regression reports appeared in the provided data. Issue #8138 documents 50 non-pass officeqa tasks, but the summary characterizes them as primarily model-quality errors, not IronClaw infrastructure failures. Severity ranking: **Informational / Low** for project code stability. No fix PR exists, and 0 PRs were updated in the window.  
Link: https://github.com/nearai/ironclaw/issues/8138

## 6. Feature Requests & Roadmap Signals
No feature requests were present in the dataset. The only roadmap-adjacent signal is continued investment in automated failure taxonomy and benchmark diagnostics, which could eventually influence eval tooling, model routing, or quality reporting. With 0 PRs and 0 releases, there is not enough evidence to predict next-version contents.

## 7. User Feedback Summary
No direct user feedback, satisfaction, or dissatisfaction signals were captured. The only author-driven input is from pranavraja99’s failure-taxonomy issue, which indicates a need for clearer diagnosis of benchmark failures and model-quality limitations. With 0 comments and 0 reactions, community sentiment cannot be assessed from this data.

## 8. Backlog Watch
- **#8138 [OPEN] Daily ironclaw failure taxonomy — 2026-10-15**  
  Link: https://github.com/nearai/ironclaw/issues/8138  
  This is the only open issue in the dataset and currently has 0 comments. It is not yet long-unanswered, having been created/updated on 2026-10-15, but it may need maintainer triage or a linked eval follow-up if it remains unaddressed. No other long-unanswered issues or PRs were present in the provided data.

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw Project Digest — 2026-10-16

## 1. Today's Overview

QwenPaw activity remains high: 29 issues were touched in the last 24h (22 still open, 7 closed) and 23 PRs moved (20 open, 3 merged/closed), alongside the stable **v2.2.2** release. The issue queue is dominated by bug reports rather than feature discussion, with several multi-agent runtime, cron/hot-reload, and Windows-desktop stability problems recurring across releases 2.2.1–2.2.2b5. Maintainer throughput on fixes is visible — a batch of console, desktop, and plugin-catalog fixes landed or advanced — but the open:closed ratio (22:7) suggests the backlog is growing faster than it is being drained. The most-commented threads (#8120 page load failures, #7599 OpenCode `MissingSessionID`) were closed, indicating active triage of provider-integration regressions. Overall project health is solid on delivery cadence (stable + beta releases, active first-time contributors) but strained on Desktop/Windows reliability and long-running-task lifecycle correctness.

## 2. Releases

**v2.2.2 (Stable)** — released ~2026-10-15, with release verification tracked in [#8262](https://github.com/agentscope-ai/QwenPaw/issues/8262). Release notes were prepared via [#7928](https://github.com/agentscope-ai/QwenPaw/pull/7928) (closed).

Highlights visible in the changelog:
- **Console & Workspaces:** six theme palettes with custom colors and corner radii, live preview, and persisted preferences ([#7741](https://github.com/agentscope-ai/QwenPaw/pull/7741)); an authenticated, multi-tab terminal embedded in Chat.

Breaking changes / migration notes: none stated in the provided release excerpt. The prior beta line (v2.2.2-beta.6, [#8258](https://github.com/agentscope-ai/QwenPaw/issues/8258)) closed its installation verification successfully, so the stable path is verified across platforms.

## 3. Project Progress

**Merged/closed PRs (3):**
- [#8259](https://github.com/agentscope-ai/QwenPaw/pull/8259) — `feat(tools): add structured human-in-the-loop questions`. Ships the `ask_user_question` tool, letting an agent pause mid-turn, collect structured answers, and resume in the same turn (reuses AgentScope 2.0.10 `AskUserParams`/`AskUserAnswer`). Significant capability advance for interactive agent flows.
- [#7928](https://github.com/agentscope-ai/QwenPaw/pull/7928) — `chore: release notes for v2.2.2`.
- [#8260](https://github.com/agentscope-ai/QwenPaw/pull/8260) — `fix(desktop): correct Windows listen backlog and chunk recovery state`. Raises the reused-port listen backlog from 1 to 2048 and fixes a spurious error page during chunk recovery — directly relevant to the "page load failed" complaints.

**Fix PRs advancing (open, under review):**
- [#8263](https://github.com/agentscope-ai/QwenPaw/pull/8263) — unblock config editors (Continue editing / Discard and close workflow).
- [#8229](https://github.com/agentscope-ai/QwenPaw/pull/8229) — truncate oversized base64 tool outputs in chat history (addresses 10 MB+ responses and 30 s console timeouts).
- [#8261](https://github.com/agentscope-ai/QwenPaw/pull/8261) — align Hub plugin catalog with installed state.
- [#8257](https://github.com/agentscope-ai/QwenPaw/pull/8257) — honor menu visibility in Settings Center.
- [#8203](https://github.com/agentscope-ai/QwenPaw/pull/8203) — echo the user's own message as the first frame of a new run stream (cross-device visibility gap).
- [#8012](https://github.com/agentscope-ai/QwenPaw/pull/8012) — Telegram: render every fenced code block correctly (info strings with symbols, e.g. `c++`).

**Large features in flight:** PawApp SDK/control-plane redesign ([#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874)), community/inbox integration ([#7903](https://github.com/agentscope-ai/QwenPaw/pull/7903)), realtime voice chat ([#7785](https://github.com/agentscope-ai/QwenPaw/pull/7785)), channels-as-plugins with subprocess isolation ([#8187](https://github.com/agentscope-ai/QwenPaw/pull/8187)), plugin hot-reload/unload ([#7565](https://github.com/agentscope-ai/QwenPaw/pull/7565)), and OpenUI inline rendering ([#8248](https://github.com/agentscope-ai/QwenPaw/pull/8248)).

## 4. Community Hot Topics

| Item | Comments | Signal |
|---|---|---|
| [#8120](https://github.com/agentscope-ai/QwenPaw/issues/8120) `[CLOSED]` Frequent "page load failed" across multiple devices (2.2.2b4) | 8 | Highest-engagement thread; closed after triage. Desktop/web reliability is the top user-visible pain. |
| [#7599](https://github.com/agentscope-ai/QwenPaw/issues/7599) `[CLOSED]` OpenCode Go models fail with `MissingSessionID` (400) | 5 | Provider-integration breakage; closed alongside related [#7531](https://github.com/agentscope-ai/QwenPaw/issues/7531). |
| [#8126](https://github.com/agentscope-ai/QwenPaw/issues/8126) `[OPEN]` Make skill-pool download a cancellable background job | 4 | Follow-up to #8055; users hit the client's 30 s timeout on large skills. Strong candidate for near-term work. |
| [#8252](https://github.com/agentscope-ai/QwenPaw/issues/8252) `[OPEN]` Inter-agent calls silently cancelled (`CancelledError`) | 3 | Multi-agent fleet users losing artifacts with no initiator/reason recorded. High-severity correctness gap. |
| [#7531](https://github.com/agentscope-ai/QwenPaw/issues/7531) `[CLOSED]` OpenCode API now requires `x-opencode-session` header | 3 | Same provider cluster as #7599; vendor-side API change. |

**Underlying needs:** (a) users run genuinely multi-device and multi-agent fleets and expect deterministic lifecycle semantics (no silent cancellations, no data loss on reload); (b) third-party provider compatibility (OpenCode, Tavily, Firecrawl) is a recurring friction point that users want surfaced and maintained in the built-in catalog.

## 5. Bugs & Stability

Ranked by severity (impact × reproducibility):

1. **Critical — Silent cancellation of inter-agent sessions** [#8252](https://github.com/agentscope-ai/QwenPaw/issues/8252) (open, 2.2.1, Windows). `CancelledError` arrives in same-second pairs, artifacts go to zero, and the cancel path at `qwenpaw/hooks/error/error_hook.py:90` records neither initiator nor reason. No fix PR yet.
2. **Critical — Hot reload cancels running cron tasks** [#8225](https://github.com/agentscope-ai/QwenPaw/issues/8225) (open, 2.2.1). Saving a heartbeat setting via `PUT /api/config/heartbeat` aborts in-flight cron jobs, recorded as `cancelled` with no user notice. No fix PR.
3. **High — Hub restart drops desired-running runtimes; cron jobs silently skipped** [#8213](https://github.com/agentscope-ai/QwenPaw/issues/8213) (open). Local process provisioner does not restore runtimes after restart; Hub can look healthy while schedules are missed.
4. **High — Doom-loop gate kills legitimate polling** [#8184](https://github.com/agentscope-ai/QwenPaw/issues/8184) (open). Gate cannot distinguish "stuck repeating" from "waiting on a long-running background task," and cron subsequently records a false success.
5. **High — Frequent page load failure** [#8120](https://github.com/agentscope-ai/QwenPaw/issues/8120) `[CLOSED]`. Reproduced on several devices; likely mitigated by [#8260](https://github.com/agentscope-ai/QwenPaw/pull/8260) (listen backlog 1 → 2048) and chunk-recovery fix.
6. **Medium-High — Plugin hot-install aborts on distro-patched Python 3.11** [#8265](https://github.com/agentscope-ai/QwenPaw/issues/8265) (open, 2.2.2). `importlib.invalidate_caches()` called unconditionally at `loader.py:201/465/1094`; raises on some builds and blocks installation.
7. **Medium-High — `qwenpaw acp` never loads plugins** [#8222](https://github.com/agentscope-ai/QwenPaw/issues/8222) (open). `PluginLoader` is never instantiated in ACP sessions, so plugin tools and cross-process task callbacks are unavailable.
8. **Medium — `qwenpaw-creator` Windows failures:** project deletion blocked by open `creator.log` (WinError 5, [#8206](https://github.com/agentscope-ai/QwenPaw/issues/8206)) and startup abort on stale pid (`SystemError` not caught, [#8194](https://github.com/agentscope-ai/QwenPaw/issues/8194)).
9. **Medium — Cron job editor corrupts values** [#8249](https://github.com/agentscope-ai/QwenPaw/issues/8249) (2.2.2 beta5): default time resets to 00:00 and Request Content renders `[object Object]`.
10. **Medium — Message-navigation directory mis-positions** [#8251](https://github.com/agentscope-ai/QwenPaw/issues/8251) (2.2.2b4); follow-on to the #8186/#8214 navigation fixes.
11. **Medium — `desktop.log` never rotates on Windows** [#8185](https://github.com/agentscope-ai/QwenPaw/issues/8185): sidecar tee handle defeats `_SafeRotatingFileHandler`.
12. **Low/Resolved — Model deprecation handling** [#8218](https://github.com/agentscope-ai/QwenPaw/issues/8218) `[CLOSED]` (`mimo-v2.5-free` deprecated → `mimo-v2.6-flash-free`); provider session-header issues [#7531](https://github.com/agentscope-ai/QwenPaw/issues/7531)/[#7599](https://github.com/agentscope-ai/QwenPaw/issues/7599) `[CLOSED]`.

**Pattern:** cron/task-lifecycle cancellation bugs (#8252, #8225, #8213, #8184) form a coherent cluster and are the highest-value fix area; none currently has a linked fix PR.

## 6. Feature Requests & Roadmap Signals

- **Cancellable skill-pool downloads with progress** [#8126](https://github.com/agentscope-ai/QwenPaw/issues/8126) — directly follows merged work in #8055; likely to land soon given maintainer-initiated spin-off.
- **Firecrawl as a third `web_search` provider** [#8199](https://github.com/agentscope-ai/QwenPaw/issues/8199) — **implementation PR already open** ([#8200](https://github.com/agentscope-ai/QwenPaw/pull/8200), first-time contributor, Under Review). High merge probability.
- **Tavily API key via credential store** [#8174] → PR [#8204](https://github.com/agentscope-ai/QwenPaw/pull/8204) (Under Review).
- **Provider presets for CommandCode and Hugging Face Inference Providers** [#8233](https://github.com/agentscope-ai/QwenPaw/issues/8233) — low effort, catalog expansion; plausible for a 2.2.x patch.
- **Delete action in Model Pool** [#8195](https://github.com/agentscope-ai/QwenPaw/issues/8195) — purge deprecated models; pairs naturally with #8218.
- **Cross-device message visibility during in-flight runs** [#8202](https://github.com/agentscope-ai/QwenPaw/issues/8202) — **fix PR open** ([#8203](https://github.com/agentscope-ai/QwenPaw/pull/8203)).
- **i18n for protected execution contract prompt** [#8211](https://github.com/agentscope-ai/QwenPaw/issues/8211) — extends the existing `SCROLL_SYSTEM_PROMPT_TEMPLATES` mechanism.
- **Optional host navigation policy** [#8223](https://github.com/agentscope-ai/QwenPaw/issues/8223) — enterprise/curated deployment control, related to in-flight [#8257](https://github.com/agentscope-ai/QwenPaw/pull/8257).
- **Claude Code third-party harness** [#7396](https://github.com/agentscope-ai/QwenPaw/issues/7396) — catalog entry still a placeholder in `harnesses/registry.py`.

**Prediction for next release (2.2.3 / 2.3.0):** Firecrawl + Tavily credential support, cross-device message echo, message-navigation fixes, plugin-catalog/installed-state fixes, and desktop startup/backlog fixes. Larger items (realtime voice, channels-as-plugins, PawApp SDK redesign, OpenUI) look targeted at a minor/major release rather than a patch.

## 7. User Feedback Summary

**Pain points:**
- **Desktop/web reliability and startup latency.** #8120 (multi-device page load failures) and [#8264](https://github.com/agentscope-ai/QwenPaw/issues/8264) ("starts slowly, easily >10s before reaching the main interface; feels worse than workbuddy and Bean Bag") show the perceived experience gap versus competing assistants.
- **Task reliability under configuration changes.** Users lose running cron jobs merely by saving settings (#8225) or restarting the Hub (#8213), and the failures are silent.
- **Multi-agent orchestration fragility.** The #8252 fleet user is running an Orchestrator/Reasoner/Executor/Reviewer/Defender topology with a Feishu channel gateway — a sophisticated production use case whose sessions are being cancelled without diagnostics.
- **Windows as a second-class platform.** #8185, #8194, #8206 all cluster on Windows (log rotation, stale pid, file locks).
- **Provider drift.** OpenCode header/session requirements (#7531, #7599) and model deprecations (#8218) break working setups without user action.

**Satisfaction signals:** Positive reception of 2.2.x console investments (themes, in-Chat terminal) and enthusiasm for the third-party harness feature (Codex and Qoder supported; Claude Code eagerly awaited in #7396). First-time contributors are actively submitting PRs (#8012, #8200, #8204, #8259) — a healthy community funnel. Dissatisfaction concentrates on reliability and perceived latency rather than missing features.

## 8. Backlog Watch

Long-standing or high-importance items lacking recent resolution:

- [#7396](https://github.com/agentscope-ai/QwenPaw/issues/7396) — *Claude Code as a third-party agent harness — status and roadmap?* Open since **2026-08-28**; two comments, no maintainer roadmap answer. Users can see the placeholder in source, so silence is a visible gap.
- [#7565](https://github.com/agentscope-ai/QwenPaw/pull/7565) — *clean plugin unload and rollback-safe hot reload* (opened **2026-09-04**, size/XXXL). Directly relevant to today's plugin hot-install failure (#8265) and hot-reload cancellation (#8225); needs review bandwidth.
- [#7785](https://github.com/agentscope-ai/QwenPaw/pull/7785) — *realtime voice chat* (opened **2026-09-15**, size/XXXL). Long-running feature PR without visible merge signal.
- [#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874) — *redesign the PawApp SDK and app control plane* (opened **2026-09-19**, size/XXXL). Note #8266 explicitly ships Data App 1.0.3 *independently of* Host vNext, implying this redesign is still far from landing — worth a public status note.
- [#7903](https://github.com/agentscope-ai/QwenPaw/pull/7903) — *integrate QwenPaw community and inbox* (opened **2026-09-20**, size/XXXL, `[wip]`).
- [#8012](https://github.com/agentscope-ai/QwenPaw/pull/8012) — Telegram fenced-code rendering fix from a first-time contributor (opened **2026-09-29**); small, user-visible, and a good candidate for prompt review to retain the contributor.

**Recommendation:** Prioritize a maintainer response on #7396 (roadmap clarity) and a review pass on #7565, since plugin lifecycle correctness underpins three of today's open bugs.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-10-16

## 1. Today's Overview

ZeroClaw remains in a high-churn, pre-release development phase: 27 issues and 50 pull requests were updated in the last 24 hours, yet no new releases landed and zero issues were closed, with only two PRs closed (one a superseded Dependabot bump, one a security patch). The open PR queue has grown to 48 items, several of them `size:XL` / `risk:high` and tagged for `release:v0.8.6` or `release:v0.9.0`, indicating a large review backlog relative to merge throughput. Bug activity skews heavily toward the **email and WhatsApp channel implementations**, with a cluster of S2 (degraded behavior) reports filed by the same contributor in a single day. Strategic work continues around the WASM plugin system, runtime/gateway separation, and the OIDC/multi-user security milestones, all of which remain tracked as open milestones rather than shipped. Overall health: **very active, but merge- and release-starved** — the project is accumulating validated work faster than maintainers are closing it.

## 2. Releases

No new releases in the last 24 hours. No release notes, breaking changes, or migration guidance to report.

## 3. Project Progress

Only two PRs left the open queue today:

- **#11755 [CLOSED]** — `fix(deps): patch Wasmtime and Rustls security advisories`. Bumped the locked Wasmtime family `48.0.5 → 48.0.6` (plus Pulley/Cranelift alignment) and raised three direct plugin dependency minimums to `48.0.6`, addressing **14 Wasmtime RustSec findings**. This is the most concrete stability win of the day. ([PR #11755](https://github.com/zeroclaw-labs/zeroclaw/pull/11755))
- **#11723 [CLOSED]** — Dependabot `rust-all` group bump (30 updates), closed as superseded by **#11776** (same group with 31 updates). ([PR #11723](https://github.com/zeroclaw-labs/zeroclaw/pull/11723), [PR #11776](https://github.com/zeroclaw-labs/zeroclaw/pull/11776))

Feature work advanced in-review but not merged, notably the **plugin lifecycle stack** from IftekharUddin (#11236 recovery of incomplete installs, #11261 staged admission replacement, #11262 verified `zeroclaw plugin update`, #11309 quickstart plugin install, #11347 shipping the WASM plugin host in release artifacts, #11311 artifact smoke harness, #11304 egress-denial logging). Per the OIDC tracker (#8289), the core OIDC stack and the consolidated enrollment/gateway/private-memory slices (#11082) have previously landed, leaving that effort in close-out.

## 4. Community Hot Topics

Ranked by comment count in the last 24h window:

| Item | Comments | Theme |
|---|---|---|
| [#7952](https://github.com/zeroclaw-labs/zeroclaw/issues/7952) | 8 | Release-shape confusion — optional `channels-full` prebuilt bundle vs. lean default |
| [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) | 7 | Tracker for runtime Phase 2 (v0.8.6) and gateway separation (v0.9.0) |
| [#10531](https://github.com/zeroclaw-labs/zeroclaw/issues/10531) | 6 | Delegate sub-agent progress visibility (tool receipts, partial output) |
| [#8763](https://github.com/zeroclaw-labs/zeroclaw/issues/8763) | 4 | ZeroCode UI for subagent activity and expandable tool results |
| [#8289](https://github.com/zeroclaw-labs/zeroclaw/issues/8289) | 4 | OIDC canonical principals / inbound authentication tracker |
| [#10426](https://github.com/zeroclaw-labs/zeroclaw/issues/10426) | 3 | User-facing agent progress in Telegram |

**Underlying needs analysis:** The comment leaders cluster around two distinct anxieties. First, **operator confidence** — users cannot see what delegated sub-agents are doing (#10531, #8763, #10426), and the release packaging debate (#7952) reflects friction when a configured channel doesn't work with the shipped binary. Second, **architectural durability** — the runtime/gateway (#7432) and OIDC (#8289) trackers are long-lived sources of truth, suggesting contributors want contract stability before building further on these surfaces.

## 5. Bugs & Stability

Ranked by reported severity:

**S1 — Workflow blocked**
- [#11130](https://github.com/zeroclaw-labs/zeroclaw/issues/11130) — *DeepSeek DSML tool-call markup is not parsed; raw markup leaks to the channel and the turn ends silently.* Root cause is in the shared tool-call parser, with symptoms in `channel` and `runtime`. `status:in-progress`, so a fix is believed underway. ([related #11742](https://github.com/zeroclaw-labs/zeroclaw/issues/11742) broadens this to `custom` OpenAI-compatible providers defaulting to text-fallback.)

**S2 — Degraded behavior**
- [#10114](https://github.com/zeroclaw-labs/zeroclaw/issues/10114) (p1) — `max_tool_result_chars` is a fixed 50,000 default, unrelated to the model's actual context window.
- [#10116](https://github.com/zeroclaw-labs/zeroclaw/issues/10116) (p1, `follow-up`) — Oversized tool results are cut byte-wise middle-out; should spill to a file handle as `web_fetch` does.
- [#11745](https://github.com/zeroclaw-labs/zeroclaw/issues/11745) — Email channel does not store inbound attachments as documented.
- [#11749](https://github.com/zeroclaw-labs/zeroclaw/issues/11749) — Email channel answers auto-replies, bounces, and list mail, risking **mail loops**.
- [#11760](https://github.com/zeroclaw-labs/zeroclaw/issues/11760) — Email channel re-answers live mail after every restart (active mode never sets `\Seen`).
- [#11761](https://github.com/zeroclaw-labs/zeroclaw/issues/11761) — Email channel misses mail arriving during a fetch until the next IDLE timeout (default 29 min).
- [#11764](https://github.com/zeroclaw-labs/zeroclaw/issues/11764) — WhatsApp Web silently drops untranscribed voice notes.
- [#11773](https://github.com/zeroclaw-labs/zeroclaw/issues/11773) — Regional OS locales (`fr_FR`, `es_ES`, `ja_JP`) fall back to English.
- [#11771](https://github.com/zeroclaw-labs/zeroclaw/issues/11771) — Bootstrap file cap mismatch: 20,000 chars on gateway turns vs. 6,000 (with `compact_context`) on channel/agent-loop turns.

**S3 — Minor**
- [#11769](https://github.com/zeroclaw-labs/zeroclaw/issues/11769) — Four channel tests hard-code English notice text and fail under other locales.
- [#11758](https://github.com/zeroclaw-labs/zeroclaw/issues/11758) — WhatsApp Web keeps printing pairing QR codes for ~2.5 min after its listener stops.

**Fix PRs present?** Partially. [#11297](https://github.com/zeroclaw-labs/zeroclaw/pull/11297) restores `keep_tool_context_turns` semantics, [#10391](https://github.com/zeroclaw-labs/zeroclaw/pull/10391) tightens bounded-delegate workspace/tool/command policy, [#11672](https://github.com/zeroclaw-labs/zeroclaw/pull/11672) fixes JPEG admission projection, and the Wasmtime/Rustls advisories were patched (#11755). **No fix PR is visible for the email-cluster or WhatsApp-cluster bugs (#11745, #11749, #11760, #11761, #11764, #11758), which is the most concerning stability gap today.**

## 6. Feature Requests & Roadmap Signals

Concrete feature requests updated today, mapped to release tags where present:

- **Release/packaging:** [#7952](https://github.com/zeroclaw-labs/zeroclaw/issues/7952) — optional broad-channel prebuilts (`channels-full`).
- **Agent observability:** [#10531](https://github.com/zeroclaw-labs/zeroclaw/issues/10531), [#8763](https://github.com/zeroclaw-labs/zeroclaw/issues/8763), [#10426](https://github.com/zeroclaw-labs/zeroclaw/issues/10426), [#11743](https://github.com/zeroclaw-labs/zeroclaw/issues/11743) — progress/thinking streams for delegate, ZeroCode, Telegram, and Slack.
- **RFC:** [#11735](https://github.com/zeroclaw-labs/zeroclaw/issues/11735) — bounded tool and delegate results at the turn boundary (directly addresses the same pain as #10114/#10116).
- **Email channel hardening:** [#11747](https://github.com/zeroclaw-labs/zeroclaw/issues/11747) (SMTP STARTTLS / Office 365:587), [#11751](https://github.com/zeroclaw-labs/zeroclaw/issues/11751) (`tls_ca_cert_path` for private CAs).
- **Config/UX:** [#10665](https://github.com/zeroclaw-labs/zeroclaw/issues/10665) (configurable ZeroCode per-pane session limit), [#11753](https://github.com/zeroclaw-labs/zeroclaw/issues/11753) (configurable ack reaction emoji).

**Prediction for the next version (v0.8.6, then v0.9.0):** the strongest near-term signal is the **WASM plugin lifecycle** — a half-dozen stacked, `release:v0.8.6`-tagged PRs (#11236, #11261, #11262, #11309, #11347, #11311) plus the already-landed security patch (#11755). Also likely to ride along: the eleven-built-in-tools default (#11221) and the runtime delivery items in the #7432 tracker. OIDC (#8289) and multi-user isolation (#8290) are explicitly `release:v0.9.0` and remain close-out trackers rather than fresh work. The turn-boundary bounding RFC (#11735), if accepted, may be picked up in a v0.9.x cycle.

## 7. User Feedback Summary

**Pain points voiced by real users:**

- **Silent long-running turns.** Operators on Telegram (#10426) and delegating agents generally (#10531) describe requests that "look stalled" because no intermediate progress is surfaced. This is the single most repeated UX complaint across issues.
- **Email channel is not production-trustworthy.** A single contributor (RustLangLatam) filed five email issues in two days covering attachments, auto-reply loops, `\Seen` state after restart, IDLE-timeout mail loss, STARTTLS, and private-CA trust. This reads as an operator attempting a real mailbox deployment and hitting a wall.
- **Configuration surprises.** #7952 (channels not in the lean prebuilt) and #11742 (`custom` provider silently defaulting to text-fallback) both reflect confusion when defaults don't match documented expectations. #11771 (differing bootstrap caps by turn path) is the same class of "the config says one thing, the runtime does another."
- **Localization treated as second-class.** #11773 and #11769 show non-English locales (`fr_FR`, `es_ES`, `ja_JP`) degrading to English or breaking tests.

**Satisfaction signals:** contributors are engaged and producing detailed, well-scoped bug reports with file/line references; the security patch and active `p1` in-progress items show responsiveness on high-severity paths. **Dissatisfaction** concentrates on channel reliability and visibility — the areas with the least PR coverage in today's data.

## 8. Backlog Watch

Long-lived or high-importance items needing maintainer attention:

- [#7952](https://github.com/zeroclaw-labs/zeroclaw/issues/7952) — open since **2026-06-19**, 8 comments, tagged `status:icebox` despite `status:accepted` and `risk:high`. Highest-comment issue in the window with no visible movement.
- [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) — open since **2026-06-09**, the v0.8.6/v0.9.0 runtime/gateway tracker; effectively gates the next two releases.
- [#8289](https://github.com/zeroclaw-labs/zeroclaw/issues/8289) and [#8290](https://github.com/zeroclaw-labs/zeroclaw/issues/8290) — both open since **2026-06-24**, `topic:identity-access`, `risk:high`, marked as close-out trackers for v0.9.0. They need explicit sign-off or closure to unblock downstream planning.
- [PR #10430](https://github.com/zeroclaw-labs/zeroclaw/pull/10430) — Gemini speech-to-speech channel, open since **2026-08-28**, `needs-author-action`, `status:parking-lot`, `size:XL`.
- [PR #10391](https://github.com/zeroclaw-labs/zeroclaw/pull/10391) — bounded-delegate policy enforcement, open since **2026-08-26**, `needs-author-action`; directly relevant to the delegate-boundary RFC (#11735).
- [PR #10499](https://github.com/zeroclaw-labs/zeroclaw/pull/10499) — persistent config write validation, open since **2026-08-31**, `needs-author-action`, `risk:high`.
- [PR #11221](https://github.com/zeroclaw-labs/zeroclaw/pull/11221) — eleven-built-in-tools default, open since **2026-09-29**, `needs-author-action`, `release:v0.8.6` **and** `release:v0.9.0`; a release-gating change with no merge despite active rebasing.
- [PR #11304](https://github.com/zeroclaw-labs/zeroclaw/pull/11304) — plugin egress-denial logging, explicitly `status:blocked`.
- [#11130](https://github.com/zeroclaw-labs/zeroclaw/issues/11130) — S1 DSML parser bug, `status:in-progress` with no linked PR visible; warrants a status check given its workflow-blocking severity.

**Bottom line:** the project's bottleneck is review throughput, not contribution volume. With 48 open PRs against 2 closures and a release-gating item (#11221) aging in `needs-author-action`, the healthiest next step would be triaging the v0.8.6 plugin stack and the unaddressed email/WhatsApp bug cluster.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*