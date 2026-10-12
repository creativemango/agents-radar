# OpenClaw Ecosystem Digest 2026-10-12

> Issues: 500 | PRs: 500 | Projects covered: 5 | Generated: 2026-10-12 08:27 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# OpenClaw Project Digest — 2026-10-12

## 1. Today’s Overview

OpenClaw remains highly active: 500 issues were updated in the last 24 hours (357 open/active, 143 closed), and 500 PRs were updated (363 open, 137 merged/closed), alongside two new releases. The latest releases focus almost entirely on update reliability, with v2026.10.2 fixing clean in-place upgrades from stable 2026.10.1 and v2026.10.5-beta.1 hardening updates across devices, filesystems, Windows elevation, and interrupted activations. However, the issue tracker shows persistent high-severity stability problems, especially memory leaks, zombie processes, update failures, session-state loss, and plugin-capture performance regressions. Project health is therefore mixed: maintenance throughput is high, but unresolved P0/P1 issues continue to affect production deployments, particularly on Windows, npm-global installs, and complex container/filesystem setups.

## 2. Releases

### v2026.10.2
Link: https://github.com/openclaw/openclaw/releases/tag/v2026.10.2

Highlights from the release notes:
- The first upgrade from stable 2026.10.1 now completes cleanly.
- Successful in-place updates no longer end in errors.
- Windows, cloned-filesystem, and private-Node update paths finish instead of stalling or rolling back.
- Referenced fixes: #168849, #169031.

### v2026.10.5-beta.1
Link: https://github.com/openclaw/openclaw/releases/tag/v2026.10.5-beta.1

Highlights from the release notes:
- Safer updates across devices, filesystems, Windows elevation, and interrupted activations.
- Recovery from stale leases or changed evidence instead of stranding the install or blocking future updates.
- Referenced fixes: #164636, #164769, #165257, #165863, plus one truncated reference.

**Breaking changes / migration notes:** The provided release notes do not mention explicit breaking changes. The primary migration signal is improved resilience in update paths, which should reduce manual recovery for failed upgrades. That said, many open issues still report update failures across npm-global, Windows, LXC, and Windows multi-agent installs, so operators should still back up state before major upgrades.

## 3. Project Progress

- **PRs merged/closed today:** 137.
- **Issues closed today:** 143.

Closed issues indicate fixes landed in several areas:
- **Update reliability:** #164113 — update failed at `updater-runtime-retention` with FICLONE EPERM inside unprivileged LXC; closed with a queueable fix. Link: https://github.com/openclaw/openclaw/issues/164113
- **Package recovery:** #167506 — deleted temporary handoff database blocked package recovery and future updates; closed. Link: https://github.com/openclaw/openclaw/issues/167506
- **Session state:** #69118 — Claude CLI sessions reset on every turn in group channels due to `groupIntro` drift; closed as already fixed. Link: https://github.com/openclaw/openclaw/issues/69118
- **Model request serialization:** #123265 — `role:"custom"` runtime-context carrier was serialized as a trailing `user` message on every request; closed. Link: https://github.com/openclaw/openclaw/issues/123265
- **Memory/CPU saturation:** #136175 — full local memory reindex saturated CPU and blocked diagnostics; closed. Link: https://github.com/openclaw/openclaw/issues/136175
- **Sub-agent idempotency:** #37446 — sub-agent timeout recovery created duplicate API posts; closed. Link: https://github.com/openclaw/openclaw/issues/37446

A large share of open PR activity is driven by maintainer `steipete`, focused on architectural simplification and moving synchronous SQLite work off the Gateway thread:
- #169648 — simplify state and process worker lifecycles. https://github.com/openclaw/openclaw/pull/169648
- #169584 — prepare execution policy through workers. https://github.com/openclaw/openclaw/pull/169584
- #169580 — move Gateway state consumers through existing workers. https://github.com/openclaw/openclaw/pull/169580
- #169566 — route session forks, reads, and cleanup through workers. https://github.com/openclaw/openclaw/pull/169566
- #169735 — avoid redundant reminder and session snapshot SQL. https://github.com/openclaw/openclaw/pull/169735
- #169707 — reduce Control UI cold-load chat rendering. https://github.com/openclaw/openclaw/pull/169707
- #169709 — reduce idle Gateway event-loop sampling work. https://github.com/openclaw/openclaw/pull/169709

There is also a significant open P1 PR aimed at safer updates:
- #144005 — “back up state before migrations and restore it on rollback.” Still open, marked `needs proof`, large scope. https://github.com/openclaw/openclaw/pull/144005

## 4. Community Hot Topics

Most active issues by comment count:

1. **#91588 — P0 Gateway memory leak (38 comments)**  
   RSS grows from ~350 MB to 15.5 GB over days, causing repeated OOM crashes. No new fix PR.  
   https://github.com/openclaw/openclaw/issues/91588

2. **#159356 — llama.cpp manager reports ready while embedding child exits (26 comments)**  
   Embedding requests return HTTP 500 “Failed to read connection”; P2 but impacts session state and crash loops.  
   https://github.com/openclaw/openclaw/issues/159356

3. **#97616 — P1 unreaped hook/tool child processes (18 comments)**  
   Zombie accumulation under the main `openclaw` process causes runtime degradation.  
   https://github.com/openclaw/openclaw/issues/97616

4. **#148707 — P1 reply lost with “no active tool authority snapshot” (18 comments)**  
   2026.9.4 regression: a second run displaces an in-flight turn and the reply is lost.  
   https://github.com/openclaw/openclaw/issues/148707

5. **#160959 — P0 Gateway blocks for minutes while capturing large external plugins (15 comments)**  
   2026.9.6 regression; event loop blocked at startup and model runtime republish.  
   https://github.com/openclaw/openclaw/issues/160959

6. **#86996 — Active Memory + Codex app-server path causes latency and stalls (15 comments, closed)**  
   Long response latency, hook timeouts, startup aborts, and Gateway event-loop stalls.  
   https://github.com/openclaw/openclaw/issues/86996

7. **#157989 — P1 plugin source capture rewrites ~1.1–1.4 GB per CLI command (15 comments)**  
   ~6.5 GB per Gateway start with no reuse; severe SSD wear.  
   https://github.com/openclaw/openclaw/issues/157989

**Underlying needs:** Users are consistently asking for resource stability, update reliability, plugin performance, and session/message durability. The highest-comment issues are not feature requests but production-impacting reliability gaps, many tagged `clawsweeper:no-new-fix-pr` or `needs-maintainer-review`, suggesting maintainer attention is the bottleneck rather than lack of reports.

## 5. Bugs & Stability

Ranked by severity from the provided data:

### P0 / Critical
- **#91588 — Gateway memory leak, RSS 350 MB → 15.5 GB, repeated OOM crashes.** Open, 38 comments, no new fix PR.  
  https://github.com/openclaw/openclaw/issues/91588
- **#160959 — Gateway event loop blocks for minutes while capturing large external plugins (2026.9.6 regression).** Open, 15 comments, no new fix PR.  
  https://github.com/openclaw/openclaw/issues/160959
- **#156112 — `openclaw update` fails at global install swap while direct `npm install -g` succeeds.** Open, 14 comments, marked `manual-only`.  
  https://github.com/openclaw/openclaw/issues/156112
- **#167652 — Windows Gateway hangs after 2026.9.9 upgrade despite Doctor reporting verified restart.** Open, 8 comments, P0, release blocker.  
  https://github.com/openclaw/openclaw/issues/167652
- **#126821 — SQLite corruption recurs on pristine rebuilt DBs within 15–24h (WSL2), including paralyzed gateway mode.** Open, 9 comments.  
  https://github.com/openclaw/openclaw/issues/126821
- **#112475 — Device pairing recovery fails after removal.** Open, 8 comments, P0.  
  https://github.com/openclaw/openclaw/issues/112475
- **#155191 — Native memory leak in 2026.9.5: RSS grows ~1 GiB per 30s while V8 heap stays stable.** Open, 8 comments.  
  https://github.com/openclaw/openclaw/issues/155191

### P1 / High
- **#97616 — Leaked/unreaped hook and tool child processes cause zombie accumulation and runtime degradation.** Open, 18 comments.  
  https://github.com/openclaw/openclaw/issues/97616
- **#148707 — Reply lost with “Reply operation has no active tool authority snapshot” on displaced in-flight turn.** Open, 18 comments.  
  https://github.com/openclaw/openclaw/issues/148707
- **#145309 — `claude-cli` backend ignores `CLAUDE_CONFIG_DIR`, causing missing-transcript resets and failover.** Open, 12 comments.  
  https://github.com/openclaw/openclaw/issues/145309
- **#104719 — memory-wiki supplement exhaustive fallback ignores tool deadline.** Open, 12 comments.  
  https://github.com/openclaw/openclaw/issues/104719
- **#168955 — Startup regression in 2026.10.1 on Windows: ready ~1030s vs ~410s on 2026.9.9.** Open, 10 comments.  
  https://github.com/openclaw/openclaw/issues/168955
- **#158788 — Cron agent turns fail at setup with `DataCloneError` on Windows (2026.9.6).** Open, 8 comments.  
  https://github.com/openclaw/openclaw/issues/158788
- **#159512 — `claude-cli`: every subagent completion invalidates requester CLI session twice.** Open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/159512
- **#107244 — WhatsApp group messages never reach inbound handling; DMs work.** Open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/107244
- **#126950 — Telegram opaque callback payloads (`tgcb1:`) silently dropped for non-plugin callers.** Open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/126950
- **#54488 — Session lane starvation: followup drain monopolizes lane, blocks inbound dispatch for 20–30min.** Open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/54488

### P2 / Moderate
- **#159356 — llama.cpp manager reports ready while embedding child exits; embedding requests return HTTP 500.** Open, 26 comments.  
  https://github.com/openclaw/openclaw/issues/159356
- **#105528 — `exec`/`read` tools silently return empty output on Windows (2026.6.x regression).** Open, 10 comments.  
  https://github.com/openclaw/openclaw/issues/105528
- **#120385 — Code Mode tool catalog incomplete for scheduled/cron and memory-flush turns; MCP tools missing.** Open, 10 comments.  
  https://github.com/openclaw/openclaw/issues/120385
- **#164394 — Control UI WebChat transcript jitters continuously when scrolling into history.** Open, 9 comments.  
  https://github.com/openclaw/openclaw/issues/164394
- **#162914 — Intermittent “Aborted | sandbox_provisioning” failures with rootless Podman sandbox.** Open, 9 comments.  
  https://github.com/openclaw/openclaw/issues/162914
- **#48709 — Gemini 2.5 Pro: `textSignature` bloat, think tags, mixed text/tool causing session failures.** Open, 8 comments.  
  https://github.com/openclaw/openclaw/issues/48709
- **#99659 — OpenClaw OOM-killed after companion app connected.** Open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/99659
- **#166894 — Gateway restart sometimes hangs 30+ minutes to 2 hours on Windows multi-agent installs (2026.9.8).** Open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/166894
- **#124759 — iOS app lags badly when “show reasoning and tool activity” is enabled.** Open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/124759

**Fix PR status:** Many high-severity issues carry `clawsweeper:no-new-fix-pr`, meaning no linked fix PR is visible in the provided data. Some closed issues — #164113, #167506, #69118, #123265, #136175, #37446 — indicate fixes have recently landed for update recovery, session hashing, request serialization, and sub-agent idempotency.

## 6. Feature Requests & Roadmap Signals

- **#50093 — WhatsApp: backfill missed messages after reconnection.** P1, open, 14 comments. Users lose messages during outage windows.  
  https://github.com/openclaw/openclaw/issues/50093
- **#16670 — Onboarding wizard should include Memory/Embedding setup as a mandatory step.** P2, open, 10 comments. Memory persistence is considered a core feature but setup does not mention embedding configuration.  
  https://github.com/openclaw/openclaw/issues/16670
- **#14785 — Reduce tool schema token overhead (~3,500 tokens/session).** P2, open, 11 comments. Fixed context tax on every session.  
  https://github.com/openclaw/openclaw/issues/14785
- **#38568 — Inject context window % into system prompt runtime section.** P3, open, 8 comments. Useful for agents that manage context usage.  
  https://github.com/openclaw/openclaw/issues/38568
- **#111143 — Allow pinning plugin tabs in the Control UI sidebar.** P3, open, 6 comments.  
  https://github.com/openclaw/openclaw/issues/111143
- **#74481 — Dynamic catalog refresh from configured provider `/v1/models`.** P2, open, 7 comments. Addresses hardcoded model catalog and proxy environments.  
  https://github.com/openclaw/openclaw/issues/74481
- **#42591 — Modularize `install.sh` to improve maintainability.** P3, open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/42591
- **#76247 — Native dispatch landing ACK / receiver-entry telemetry across cron, sessions_spawn, channel_inbound, gateway_agent, cli_agent, acp_downstream.** P2, open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/76247
- **#124911 — Compaction `reserveTokensFloor` ignores model context window; adaptive helper exists but is unused.** P2, open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/124911

**Roadmap prediction:** Given the latest releases are update-focused, the next stabilization window may target session/message durability and plugin capture performance. Feature requests with the strongest signals are memory/embedding setup (#16670), token overhead reduction (#14785), and WhatsApp backfill (#50093). The large open PR #144005 for update backup/rollback is a likely candidate for the next stable release once proof requirements are met. Plugin UI customization and dynamic model catalog refreshes are lower-priority but repeatedly requested.

## 7. User Feedback Summary

- **Severe dissatisfaction with stability:** Users report OOM crashes, memory leaks, zombie processes, SQLite corruption, and gateway hangs that make deployments unusable (#91588, #155191, #97616, #126821, #167652).
- **Update pain is widespread:** `openclaw update` fails on npm-global installs, Windows, LXC containers, and cloned filesystems, often while direct npm installs succeed (#156112, #168955, #167652, #164113, #167506).
- **Performance and hardware concerns:** Plugin source capture rewrites gigabytes per command and per Gateway start, causing SSD wear and multi-minute event-loop blocks (#160959, #157989).
- **Message loss and session continuity are critical:** Users lose replies, WhatsApp group messages, Telegram callbacks, and session state under load or reconnection (#148707, #107244, #126950, #54488).
- **Platform-specific friction:** Windows users face empty tool output, slow startup, hangs, and `DataCloneError`; iOS users see lag with reasoning/tool activity enabled (#105528, #166894, #158788, #124759).
- **Maintainer responsiveness is a concern:** Many high-impact issues are tagged `needs-maintainer-review` or `needs-product-decision` and remain open for weeks or months.
- **One user abandoned a deployment:** #88087 documents poor UX for long-running background tasks and silent cron wake failures on a DigitalOcean droplet, leading the user to tear it down. https://github.com/openclaw/openclaw/issues/88087
- **Positive signals:** The project is very active, releases are frequent, and several long-standing issues were closed today, including update and session-state fixes. Users continue to provide detailed reports with environment specifics.

## 8. Backlog Watch

Important issues/PRs that are old, high-impact, and still awaiting maintainer attention or product decisions:

- **#91588 — P0 Gateway memory leak, 38 comments, created 2026-06-09.** No new fix PR. Highest-priority backlog item.  
  https://github.com/openclaw/openclaw/issues/91588
- **#97616 — P1 zombie process accumulation, created 2026-06-29.** Open, 18 comments.  
  https://github.com/openclaw/openclaw/issues/97616
- **#104719 — P1 memory-wiki supplement ignores deadline, created 2026-07-11.** Open, 12 comments.  
  https://github.com/openclaw/openclaw/issues/104719
- **#126821 — P0 recurring SQLite corruption, created 2026-08-20.** Open, 9 comments.  
  https://github.com/openclaw/openclaw/issues/126821
- **#155191 — P0 native memory leak, created 2026-09-21.** Open, 8 comments.  
  https://github.com/openclaw/openclaw/issues/155191
- **#167652 — P0 Windows Gateway hang after upgrade, created 2026-10-09.** Open, 8 comments.  
  https://github.com/openclaw/openclaw/issues/167652
- **#50093 — P1 WhatsApp backfill, created 2026-03-19.** Needs product decision and live repro.  
  https://github.com/openclaw/openclaw/issues/50093
- **#54488 — P1 session lane starvation, created 2026-03-25.** Open, 7 comments.  
  https://github.com/openclaw/openclaw/issues/54488
- **#14785 — P2 tool schema token overhead, created 2026-02-12.** Open, 11 comments.  
  https://github.com/openclaw/openclaw/issues/14785
- **#16670 — P2 memory/embedding setup in onboarding, created 2026-02-15.** Open, 10 comments.  
  https://github.com/openclaw/openclaw/issues/16670
- **#38568 — P3 context window % in system prompt, created 2026-03-07.** Open, 8 comments.  
  https://github.com/openclaw/openclaw/issues/38568
- **#144005 — P1 PR: backup state before migrations and restore on rollback.** Open since 2026-09-10, large scope, marked `needs proof`.  
  https://github.com/openclaw/openclaw/pull/144005
- **#158387 — P2 PR: reject channel names without message destinations.** Open since 2026-09-25, marked ready for maintainer look.  
  https://github.com/openclaw/openclaw/pull/158387

**Overall backlog concern:** The most critical reliability issues are not new; they have accumulated over months while the project shipped update-focused releases. Maintainer review and product decision bandwidth appear to be central constraints for closing the highest-impact P0/P1 gaps.

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — Personal AI Assistant / Agent OSS Ecosystem
**Window:** 2026-10-11 → 2026-10-12 | **Projects:** OpenClaw, Hermes Agent, IronClaw, QwenPaw, ZeroClaw

> **Method note:** Activity figures are taken directly from each project's 24-hour digest. Health scores are analyst-assigned composites (1–10) weighing activity throughput, closure ratio, release cadence, critical-backlog severity, and community engagement — they are not vendor- or GitHub-provided metrics. OpenClaw's issue/PR counts appear capped at the 500-item API ceiling and should be read as "≥500."

---

## 1. Ecosystem Overview

The personal AI assistant / agent OSS landscape is **functionally converging while structurally diverging**. Nearly every active project is independently rebuilding the same core stack — a gateway, a plugin/extension runtime, MCP tool plumbing, a memory/compaction layer, multi-channel ingress, and provider-agnostic model routing — but with sharply different language, packaging, and target-user bets. The dominant unsolved problem across all five projects is not capability but **operational reliability**: install/update fragility, gateway resource leaks, session durability, and plugin sandboxing cost appear in every backlog, and the highest-engagement community threads are production-incident reports rather than feature requests. A second cross-cutting constraint is **maintainer review bandwidth**: contributor supply is healthy in QwenPaw and ZeroClaw, yet merge latency — not idea scarcity — is the binding constraint.

---

## 2. Activity Comparison

| Project | Issues (24h) | PRs (24h) | Release Status | Health Score |
|---|---|---|---|---|
| **OpenClaw** | ~500 updated (357 open/active, 143 closed · 28.6%) | ~500 updated (363 open, 137 merged/closed · 27.4%) | **2 releases:** v2026.10.2 (stable, update fixes), v2026.10.5-beta.1 | **6.5 / 10 — Mixed** |
| **Hermes Agent** | 50 updated (10 closed · 20%) | 50 updated (18 merged/closed · 36%) | **v0.21.7** patch (~204 PRs rolled up) | **8.0 / 10 — Strong** |
| **IronClaw** | 0 | 7 updated (3 merged, 4 open) — **all dependabot** | None | **6.0 / 10 — Quiet/Stable** |
| **QwenPaw** | 13 touched (≥1 closed, incl. #7311) | 23 updated (13 merged/closed · 57%, 10 open) | None | **7.0 / 10 — Good flow, strained release** |
| **ZeroClaw** | 16 touched (**0 closed**) | 45 updated (**2 closed** · 4.4%, 43 open) | None | **5.0 / 10 — Review-bound** |

**Reading the table:** OpenClaw operates at roughly **10× the raw throughput** of its nearest peer and two orders of magnitude above IronClaw, but its closure ratios (≈27–29%) lag Hermes and QwenPaw because inflow is enormous. ZeroClaw shows the inverse pathology — high contributor inflow with a **2:43 merge-to-open ratio** — indicating a review bottleneck rather than a demand problem. QwenPaw's 57% PR closure rate is the healthiest in the set, aided by a deliberate multi-month backlog cleanup, yet it shipped **zero releases** despite a queue of merged fixes.

---

## 3. OpenClaw's Position

**Scale advantage is decisive but double-edged.** OpenClaw's ~1,000 combined issue+PR touches in 24 hours dwarfs Hermes (~100), QwenPaw (~36), ZeroClaw (~61), and IronClaw (~7). It holds the de facto "core reference" position with the broadest platform surface in the ecosystem: Windows, macOS, iOS, WSL2, LXC, Docker, npm-global, cloned-filesystem, and multi-agent Windows installs all appear in its tracker.

**Technical approach vs. peers:**

| Dimension | OpenClaw | Peer contrast |
|---|---|---|
| Runtime | Node/TS monolithic Gateway + synchronous SQLite state | ZeroClaw/IronClaw are Rust; Hermes is Python + desktop shell |
| In-flight refactor | Moving SQLite/state work **off the Gateway thread** via workers (steipete PR chain #169648/#169580/#169566) | ZeroClaw pursues equivalent goals via **runtime/gateway separation** (v0.9.0 RFC #7432) |
| Plugin model | Source-capture of external plugins (rewrites **1.1–1.4 GB per CLI command**, ~6.5 GB per Gateway start — #157989) | IronClaw/ZeroClaw use **WASM sandboxing**; QwenPaw moving to subprocess-isolated channels (#8187) |
| Session model | Session lanes + tool-authority snapshots (source of #148707 reply-loss regression) | Hermes uses a desktop backend pool with idle reaper; ZeroClaw pursues durable peer-agent turns (#9597) |

**Community size comparison:** OpenClaw is the only project where the tracker is saturated against API limits. Hermes is a credible second tier. QwenPaw and ZeroClaw have engaged, technically sophisticated but substantially smaller bases. IronClaw shows **zero community activity** in-window.

**Where peers beat it:** OpenClaw's most critical defects are old — the P0 Gateway memory leak (#91588) has been open since 2026-06-09 with 38 comments and no fix PR, and P0 SQLite corruption (#126821) since 2026-08-20. Hermes and QwenPaw closed comparatively severe issues *today*. OpenClaw's problem is not responsiveness in the aggregate but **triage prioritization of long-lived P0s against update-focused release work**.

---

## 4. Shared Technical Focus Areas

These requirements appear independently in **three or more** projects — strong evidence of genuine ecosystem-level demand rather than single-project preference.

| Cross-cutting need | OpenClaw | Hermes | QwenPaw | ZeroClaw | IronClaw |
|---|---|---|---|---|---|
| **Install/update reliability & permissions** | npm-global, Windows, LXC failures (#156112, #167652); rollback PR #144005 | Windows elevated-install ownership blocks later updates (#126860) | Docker install corruption (#8179); quickstart PRs | Stacked quickstart/install PRs (#11347) | CI/dependency bumps |
| **Gateway resource hygiene (FD/process/memory)** | RSS 350 MB→15.5 GB (#91588); zombie children (#97616) | SQLite FD leak → 256-fd launchd limit (#96027) | Desktop backend pool leak (#105239, closed) | Gateway body-limit/timeout wiring (#7692) | wasmtime risk PR #7834 |
| **Memory correctness & transparency** | memory-wiki deadline (#104719); embedding setup onboarding (#16670) | `/memory approve all` destructive overwrites (#136738) | `none` memory backend (#5732); consolidation category (#11657) | Default-config Core memory soft-hide (#11655) | — |
| **Plugin lifecycle & sandboxing** | Capture cost/SSD wear (#157989, #160959) | Plugin auto-install/backfill (#137033, #134537) | Plugin-market version routing (#5661, #5612) | WASM plugin hosting chain (8+ PRs) | wasmtime group bumps |
| **Session/state durability** | Lane starvation (#54488); reply loss (#148707) | Telegram topic misrouting (#136567); `/resume` numbering (#60136) | Streaming tool-call 404 (#7016, 59d); zombie `running` state (#8173) | Durable peer-agent turns (#9597) | — |
| **Provider quirk handling** | claude-cli `CLAUDE_CONFIG_DIR` (#145309) | DeepSeek `max_tokens` truncation (#126305); Z.AI `reasoning_effort` (#133595) | Codex backend; Tavily key surface (#8174) | Routing RFC (#11074); overflow codes (#11659) | — |
| **Approval/security transparency** | Tool-authority snapshots | Obfuscated-command approval gate (#25793); MCP approval in ACP (#135522) | Approval race (#8188); session-scoped approval (#8182) | Command-policy inheritance (#11667); SSRF propagation (#10290) | — |
| **Multi-agent orchestration** | Sub-agent idempotency (#37446) | Kanban worker workspaces (#91568) | Coding-CLI worker containers (#8156) | Swarms (#10025); Dark Factory (#11664); A2A (#11254) | — |

**MCP** appears explicitly in OpenClaw, Hermes, and QwenPaw (hardening, approval bridging, import validation). **A2A** is currently a ZeroClaw differentiator. **Windows as a first-class platform** is a shared pain point across OpenClaw, Hermes, and QwenPaw.

---

## 5. Differentiation Analysis

| | **OpenClaw** | **Hermes Agent** | **IronClaw** | **QwenPaw** | **ZeroClaw** |
|---|---|---|---|---|---|
| **Primary focus** | Breadth: every channel, OS, deployment mode | Desktop-first assistant + team workflows (Kanban, MS Store) | Secure sandboxed execution infra | Self-hosted Console + plugin market + multi-agent Docker | Protocol-forward multi-agent runtime |
| **Target users** | Power users, self-hosters, integrators | Consumer desktop users, small teams | Rust/infra developers | Self-hosted operators, CJK/HarmonyOS community | Advanced multi-agent builders |
| **Language/stack** | TS/Node, SQLite | Python + desktop shell, ACP bridge | Rust, tokio, wasmtime | Server + web Console, Codex backend | Rust |
| **Plugin model** | Source capture (high cost) | Plugin catalogue + auto-install | WASM sandbox | Version-routed plugin market | WASM + capability boundaries |
| **Distinctive bet** | Reference implementation & ubiquity | Release discipline + desktop polish | Minimal human surface; CI automation via `claude-code-action` | Console UX + accessibility tier (#8137) | A2A interoperability + RFC-driven design |

**Architectural fault line:** Rust-based projects (IronClaw, ZeroClaw) are converging on **WASM sandboxing + capability boundaries** as the security primitive, while JS/Python projects (OpenClaw, Hermes) are still managing **process-level isolation and source-capture cost**. OpenClaw's plugin model is the ecosystem's largest known efficiency liability; the Rust projects are effectively a generation ahead on this axis.

---

## 6. Community Momentum & Maturity

**Tier 1 — Hyper-scale, capacity-constrained:** **OpenClaw.** Highest velocity in the ecosystem, but operating in reactive mode against a P0 backlog. Fastest releases (2 in 24h), but releases have been narrowly update-focused while memory/process leaks persist for months.

**Tier 2 — Actively stabilizing and shipping:** **Hermes Agent.** The most balanced profile: high throughput, a release actually shipped, meaningful security and lifecycle issues closed (#25793, #105239, #127450), and a coherent in-flight fix wave (compression retry, Windows status probe, TUI locking). **Best current health-to-scale ratio.**

**Tier 3 — Improving but release-starved:** **QwenPaw.** Strong contribution flow (57% PR closure), aggressive backlog cleanup of 6-month-old PRs, and high-quality community reports — but **no release** while v2.2.x regressions accumulate (#8173, #8179, #8175). The single highest-leverage action is cutting a release.

**Tier 3b — Design-heavy, review-bottlenecked:** **ZeroClaw.** Six simultaneous high-risk RFCs, architecturally ambitious, deeply engaged power users — but a **4.4% PR closure rate** and a 50-day-stale wasm-adjacent PR (#7834-adjacent chain). Iterating on *design* more than on *code*.

**Tier 4 — Quiet maintenance:** **IronClaw.** Zero issues, zero human contributions, dependabot-only activity. Either a mature/frozen codebase or a dormancy signal — undeterminable from this snapshot.

**Iterating rapidly:** OpenClaw, Hermes, QwenPaw. **Stabilizing:** Hermes, QwenPaw. **Designing:** ZeroClaw. **Quiescent:** IronClaw.

---

## 7. Trend Signals

**1. Reliability is the product, not a feature.** Across all five projects, the highest-comment items are incident reports: memory leaks, FD exhaustion, zombie processes, silent data corruption. *For developers:* resource hygiene (RSS, FDs, child processes), atomic state, and crash recovery are now table stakes for adoption — not roadmap items.

**2. Updates/installs are becoming a first-class subsystem.** Cross-platform elevation semantics, rollback-on-failure, and backup-before-migration appear independently in OpenClaw (PR #144005), Hermes (#126860), and QwenPaw (#8179). *For developers:* design the update path with atomicity and privilege-boundary awareness from day one; it is a top-three source of user churn.

**3. Plugin sandboxing is shifting to WASM and process isolation.** IronClaw (wasmtime), ZeroClaw (WASM hosting chain), and QwenPaw (#8187 subprocess isolation) are all moving away from in-process plugin execution. OpenClaw's GB-per-command source capture is a cautionary counter-example. *For developers:* choose an isolation boundary before the plugin API stabilizes — retrofitting is expensive.

**4. Memory/context management is the new frontier of correctness.** Compaction, context overflow recovery, token budgets, and RAG corpora appear in all five. Critically, the failure mode users complain about is **silent and destructive** — QwenPaw's binary corruption (#8179), ZeroClaw's default-config memory hiding (#11655), Hermes's opaque `/memory approve all` (#136738).

**5. Silent failures are the single most-cited dissatisfaction pattern.** QwenPaw (#8173, #8175, #8179), OpenClaw (#159356 embedding child exits while "ready"), and Hermes (#135522 silently denied approval) all surface identical language: "no log, no error." *For developers:* debuggability and explicit failure surfaces are a competitive feature, especially for self-hosted operators.

**6. Protocol interoperability is consolidating around MCP and A2A.** MCP is now assumed in OpenClaw, Hermes, and QwenPaw; A2A is ZeroClaw's strategic bet (#11254). *For developers:* build tool and agent interfaces against these protocols rather than bespoke wire formats — expected lifetime is longer.

**7. Multi-agent orchestration is moving from experiment to product.** Kanban workers (Hermes), sub-agent idempotency (OpenClaw), worker containers (QwenPaw), and swarms/Dark Factory (ZeroClaw) indicate users are outgrowing single-agent topologies and need lifecycle, isolation, and policy-inheritance primitives.

**8. Provider quirks remain an unabstracted tax.** DeepSeek, Z.AI, Gemini, claude-cli, Codex, and custom-prefix providers each generate project-specific bugs. *For developers:* assume provider heterogeneity is permanent; treat model routing, token-limit defaults, and credential paths as configurable surfaces, not constants.

**9. Review bandwidth, not contributor supply, is the ecosystem's binding constraint.** OpenClaw (maintainer-tagged `needs-maintainer-review`), QwenPaw (~6-month PR latency, now clearing), and ZeroClaw (2:43 merge ratio) all exhibit the same pathology.

**10. Cost and hardware externality are becoming UX concerns.** OpenClaw's ~6.5 GB/start plugin capture causing SSD wear (#157989) is an early signal that **resource efficiency is entering the user-facing value proposition** for long-running agent deployments.

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-10-12

Scope: based on the provided 24-hour GitHub snapshot for `NousResearch/hermes-agent` and the top-comment issues/PRs shown.

## 1. Today's Overview

Hermes Agent shows high maintenance activity: 50 issues updated, 50 PRs updated, 18 PRs merged/closed, 10 issues closed, and a new patch release `v0.21.7`. The project is in an active stabilization phase, with many fixes landing across gateway, desktop, plugins, sessions, and platform-specific behavior. However, several P1/P2 reliability concerns remain open, especially around gateway file-descriptor leaks, memory-approval safety, Windows permissions, Telegram routing, and provider-specific compression failures. Overall health is strong on throughput, but the open backlog still contains high-impact stability and platform compatibility issues.

## 2. Releases

### v0.21.7
- Release date: October 11, 2026
- Link: [Hermes Agent v0.21.7](https://github.com/NousResearch/hermes-agent/releases/tag/v0.21.7)
- Provided notes: patch release rolling up approximately 204 PRs merged since `v0.21.6` into a stable release for the desktop app (macOS Apple silicon, Windows), Microsoft Store and MSIX, the installer, Docker, and Hermes Cloud.
- Breaking changes/migration notes: none stated in the provided release excerpt. Treat this as a patch/stable roll-up unless the full curated notes say otherwise.
- Note: the release note text in the source is truncated after “Full curated notes for this…”, so full change detail is unavailable from this data.

## 3. Project Progress

Today’s window closed 10 issues and merged/closed 18 PRs. Visible progress includes:

- [#137033](https://github.com/NousResearch/hermes-agent/pull/137033) — Home Assistant is no longer auto-installed into profiles that never used it; failed plugin auto-installs back off instead of retrying hourly forever. This also resolves [#132814](https://github.com/NousResearch/hermes-agent/issues/132814).
- [#127666](https://github.com/NousResearch/hermes-agent/pull/127666) — Desktop transcript duplicate-row fix: prevents one assistant reply from rendering twice when the committed page already carries a pending live row.
- Closed issue [#105239](https://github.com/NousResearch/hermes-agent/issues/105239) — desktop backend pool resource leak; opened-chat backends were exempt from `maxBackends` and idle reaper, holding resident processes.
- Closed issue [#127450](https://github.com/NousResearch/hermes-agent/issues/127450) — `hermes update` no longer stalls around 11 minutes on hosts with many checkpoint projects.
- Closed issue [#60136](https://github.com/NousResearch/hermes-agent/issues/60136) — gateway `/resume <number>` now respects `/sessions search` result numbering.
- Closed issue [#110799](https://github.com/NousResearch/hermes-agent/issues/110799) — unattended memory review no longer treats Hermes-generated compaction handoffs as user evidence.
- Closed issue [#34046](https://github.com/NousResearch/hermes-agent/issues/34046) — `baoyu-infographic` skill no longer silently bypasses `image_generate`.
- Closed security issue [#25793](https://github.com/NousResearch/hermes-agent/issues/25793) — approval-gate gap for shell-idiom-obfuscated dangerous commands.
- Closed issue [#16979](https://github.com/NousResearch/hermes-agent/issues/16979) — QQ Bot file attachments no longer silently dropped on download failure.
- Closed feature/docs issues [#94793](https://github.com/NousResearch/hermes-agent/issues/94793) and [#22699](https://github.com/NousResearch/hermes-agent/issues/22699) — auxiliary review reasoning effort and `gpt-realtime-2` documentation.

Open PRs in flight also target stability across gateway compression, Windows status probing, tool-argument repair, TUI event locking, Mattermost loop containment, Discord typing loops, UV link mode, dashboard profile query params, Kanban dependencies, Gemini TTS, and gateway guard performance.

## 4. Community Hot Topics

Most-active issues by comment count:

1. [#136567](https://github.com/NousResearch/hermes-agent/issues/136567) — **13 comments** — Telegram DM topics: opening message of a newly created topic is delivered thread-less, causing lobby recovery to misroute it to the previous topic. Open, P2. Underlying need: correct Telegram forum/thread session mapping and message delivery guarantees.
2. [#135522](https://github.com/NousResearch/hermes-agent/issues/135522) — **4 comments** — MCP write-capable tool approval is silently denied in ACP sessions. Open, P2. Underlying need: approval prompts must bridge correctly between Hermes and ACP clients.
3. [#132814](https://github.com/NousResearch/hermes-agent/issues/132814) — **4 comments** — Home Assistant migration installed the plugin into profiles that never used HA. Closed; fixed via [#137033](https://github.com/NousResearch/hermes-agent/pull/137033).
4. [#105239](https://github.com/NousResearch/hermes-agent/issues/105239) — **4 comments** — Desktop backend pool kept opened-chat backends resident, holding ~120 MB per process. Closed.
5. [#96027](https://github.com/NousResearch/hermes-agent/issues/96027) — **4 comments** — Gateway leaks SQLite file descriptors on `state.db`/WAL; can exhaust launchd’s 256-fd limit. Open, P1. Need: long-running gateway resource hygiene.
6. [#126305](https://github.com/NousResearch/hermes-agent/issues/126305) — **4 comments** — DeepSeek compression summary always truncates because provider silently defaults `max_tokens` to 8192, below the ~14K target. Open, P2.
7. [#136738](https://github.com/NousResearch/hermes-agent/issues/136738) — **4 comments** — `/memory approve all` can apply destructive whole-entry overwrites without showing what was overwritten. Open, P1, needs-decision.
8. [#127450](https://github.com/NousResearch/hermes-agent/issues/127450) — **4 comments** — `hermes update` stalls on hosts with many checkpoint projects. Closed.

Reaction activity is low overall: [#126860](https://github.com/NousResearch/hermes-agent/issues/126860) has 2 👍, while [#22699](https://github.com/NousResearch/hermes-agent/issues/22699), [#71575](https://github.com/NousResearch/hermes-agent/issues/71575), and [#94793](https://github.com/NousResearch/hermes-agent/issues/94793) each have 1 👍.

## 5. Bugs & Stability

Ranked by severity based on available labels.

### P1 — highest impact
- [#96027](https://github.com/NousResearch/hermes-agent/issues/96027) — Gateway leaks SQLite connections on `state.db`; 48 db FDs and 46 WAL FDs after 22 days. Open. No direct fix PR visible in this snapshot.
- [#136738](https://github.com/NousResearch/hermes-agent/issues/136738) — Memory write-approval: `/memory approve all` applies destructive proposals without showing overwritten content. Open, needs-decision.
- [#126860](https://github.com/NousResearch/hermes-agent/issues/126860) — Windows: elevated install/update leaves `%LOCALAPPDATA%\hermes\tools` entries admin-owned; later non-elevated `hermes update` fails with WinError 5. Open, duplicate, 2 👍.

### P2 — significant
- [#136567](https://github.com/NousResearch/hermes-agent/issues/136567) — Telegram DM topic opening message misrouted thread-less. Open.
- [#135522](https://github.com/NousResearch/hermes-agent/issues/135522) — MCP write-capable tool approval silently denied in ACP sessions. Open.
- [#126305](https://github.com/NousResearch/hermes-agent/issues/126305) — DeepSeek compression always truncates due to `max_tokens` default. Open.
- [#131409](https://github.com/NousResearch/hermes-agent/issues/131409) — Docker/remote terminal backend runs prompt-backend-probe per process, creating a container per chat turn. Open.
- [#132558](https://github.com/NousResearch/hermes-agent/issues/132558) — `exclude-newer-package` omits exact pins, breaking resolution on indexes without PEP 691 upload-time. Open.
- [#102425](https://github.com/NousResearch/hermes-agent/issues/102425) — `model_overrides` never match for custom providers because `custom:` prefix is not stripped. Open.
- [#91568](https://github.com/NousResearch/hermes-agent/issues/91568) — Kanban Docker worker workspaces need to be task-scoped and host-backed. Open.
- [#135344](https://github.com/NousResearch/hermes-agent/issues/135344) — Kanban dispatcher `_pid_recycled` exact-equality check misidentifies live macOS workers and duplicates them. Open.
- Closed in this window: [#105239](https://github.com/NousResearch/hermes-agent/issues/105239) desktop backend pool leak; [#127450](https://github.com/NousResearch/hermes-agent/issues/127450) update stalls.

### P3 — notable
- [#132259](https://github.com/NousResearch/hermes-agent/issues/132259) — ~5 random platform plugins fail to load on every boot with “dictionary changed size during iteration”. Open.
- [#134537](https://github.com/NousResearch/hermes-agent/issues/134537) — Unauthorized Home Assistant upgrade installs root-owned files and git submodules, blocking future updates. Open.
- [#104865](https://github.com/NousResearch/hermes-agent/issues/104865) — Concurrent hook callbacks are dropped instead of serialized. Open.
- [#105123](https://github.com/NousResearch/hermes-agent/issues/105123) — `disk-cleanup` hook crashes with ENAMETOOLONG on quoted terminal args containing `\n` escapes. Open.
- [#137017](https://github.com/NousResearch/hermes-agent/issues/137017) — Memory provider `holographic` not installed; automatic install failed. Open, new today.
- [#137010](https://github.com/NousResearch/hermes-agent/issues/137010) — Multiplexed gateway session reset plugin check runs only in root scope, but `pre_gateway_dispatch` is per message profile. Open, new today.
- [#133595](https://github.com/NousResearch/hermes-agent/issues/133595) — Z.AI `glm-5.3-flash` rejects `reasoning_effort=medium` with HTTP 400 / code 1210. Open.
- Closed: [#25793](https://github.com/NousResearch/hermes-agent/issues/25793) approval-gate obfuscation; [#16979](https://github.com/NousResearch/hermes-agent/issues/16979) QQ Bot attachment drops.

Fix PRs in flight include [#137051](https://github.com/NousResearch/hermes-agent/pull/137051) gateway compression retry, [#137050](https://github.com/NousResearch/hermes-agent/pull/137050) Windows status probe, [#137032](https://github.com/NousResearch/hermes-agent/pull/137032) tool-argument repair, [#137038](https://github.com/NousResearch/hermes-agent/pull/137038) TUI event locking, [#137037](https://github.com/NousResearch/hermes-agent/pull/137037) Mattermost bot loops, [#137042](https://github.com/NousResearch/hermes-agent/pull/137042) Discord typing loop, [#137040](https://github.com/NousResearch/hermes-agent/pull/137040) UV link mode, and [#137039](https://github.com/NousResearch/hermes-agent/pull/137039) Kanban dependency editing.

## 6. Feature Requests & Roadmap Signals

Open feature requests:
- [#71575](https://github.com/NousResearch/hermes-agent/issues/71575) — Surface Anthropic consumer-terms constraint at OAuth selection and provide an opt-out for Claude Code credential fallthrough. P3, `needs-decision`, 1 👍.
- [#18081](https://github.com/NousResearch/hermes-agent/issues/18081) — Allow interval-based cron jobs to specify a first-run anchor, decoupling cadence from registration time. P3, longstanding.

Feature-oriented PRs:
- [#137049](https://github.com/NousResearch/hermes-agent/pull/137049) — Enhance tool search with semantic synonyms, CJK support, and compact schemas.
- [#119684](https://github.com/NousResearch/hermes-agent/pull/119684) — Support OpenRouter Decisions approvals for smart command approvals.
- [#137039](https://github.com/NousResearch/hermes-agent/pull/137039) — Desktop Kanban dependency editing.
- [#137043](https://github.com/NousResearch/hermes-agent/pull/137043) — Plugin catalog re-pin of `agora` to v2.0.12.
- [#137044](https://github.com/NousResearch/hermes-agent/pull/137044) — Update Vornik companion catalogue to 0.19.0.

Closed feature/docs signals:
- [#94793](https://github.com/NousResearch/hermes-agent/issues/94793) — `auxiliary.review.reasoning_effort` support.
- [#22699](https://github.com/NousResearch/hermes-agent/issues/22699) — Document `gpt-realtime-2` as supported OpenAI Realtime model.

Prediction: the next minor release after `v0.21.7` is likely to pick up tool-search/CJK improvements, OpenRouter Decisions approvals, plugin catalog refreshes, and Kanban dependency UI. Cron first-run anchoring and Anthropic OAuth opt-out may remain blocked on maintainer decisions.

## 7. User Feedback Summary

Real user pain points are concentrated in cross-platform reliability and plugin lifecycle:
- Windows install/update ownership: [#126860](https://github.com/NousResearch/hermes-agent/issues/126860) shows elevated installs breaking later non-elevated updates.
- Plugin auto-install/migration: [#132814](https://github.com/NousResearch/hermes-agent/issues/132814), [#134537](https://github.com/NousResearch/hermes-agent/issues/134537), and [#137017](https://github.com/NousResearch/hermes-agent/issues/137017) reflect frustration with unwanted Home Assistant installs, root-owned files, and failed automatic installs.
- Approval safety/transparency: [#135522](https://github.com/NousResearch/hermes-agent/issues/135522) and [#136738](https://github.com/NousResearch/hermes-agent/issues/136738) show users need visible, trustworthy approval flows.
- Platform/session routing: [#136567](https://github.com/NousResearch/hermes-agent/issues/136567) highlights Telegram threading edge cases.
- Provider quirks: [#126305](https://github.com/NousResearch/hermes-agent/issues/126305) and [#133595](https://github.com/NousResearch/hermes-agent/issues/133595) show provider-specific defaults still cause session failures.
- Resource/stability: [#96027](https://github.com/NousResearch/hermes-agent/issues/96027) and closed [#105239](https://github.com/NousResearch/hermes-agent/issues/105239) reflect long-running gateway/desktop resource concerns.

Use cases active in feedback include Telegram DMs/topics, MCP tools with `trust: untrusted`, ACP sessions, DeepSeek auxiliary compression, Docker remote terminals, Kanban workers, Home Assistant, memory providers, cron automation, and Windows/macOS desktop installs. Satisfaction is visible in rapid closure of several important issues, but dissatisfaction remains around install ownership, plugin migration behavior, approval clarity, and platform-specific breakage. Multilingual engagement is also present, e.g. Chinese-language reporting in [#137017](https://github.com/NousResearch/hermes-agent/issues/137017).

## 8. Backlog Watch

Important open items needing maintainer attention:

- [#96027](https://github.com/NousResearch/hermes-agent/issues/96027) — P1 gateway SQLite FD leak. Open since 2026-08-27, 4 comments. High-risk for long-running gateways.
- [#126860](https://github.com/NousResearch/hermes-agent/issues/126860) — P1 Windows elevated install/update permissions. Open since 2026-09-28, 3 comments, 2 👍. Duplicate but user-impacting.
- [#126305](https://github.com/NousResearch/hermes-agent/issues/126305) — P2 DeepSeek compression truncation. Open since 2026-09-28, 4 comments. Blocks reliable long sessions.
- [#91568](https://github.com/NousResearch/hermes-agent/issues/91568) — P2 Kanban Docker worker workspaces. Open since 2026-08-21, 3 comments. Affects Docker/Kanban workflows.
- [#102425](https://github.com/NousResearch/hermes-agent/issues/102425) — P2 `model_overrides` custom provider prefix mismatch. Open since 2026-09-03, 2 comments.
- [#132259](https://github.com/NousResearch/hermes-agent/issues/132259) — P3 random plugin load failures on boot. Open since 2026-10-03, 3 comments. Could affect plugin reliability broadly.
- [#18081](https://github.com/NousResearch/hermes-agent/issues/18081) — P3 cron first-run anchor. Open since 2026-04-30, 1 comment. Longstanding automation request.
- [#71575](https://github.com/NousResearch/hermes-agent/issues/71575) — P3 Anthropic OAuth terms/opt-out. Open since 2026-07-25, 1 comment, `needs-decision`.

Open PRs that may need review:
- [#118150](https://github.com/NousResearch/hermes-agent/pull/118150) — Proactively refresh plugin OAuth tokens. Open since 2026-09-21.
- [#119684](https://github.com/NousResearch/hermes-agent/pull/119684) — OpenRouter Decisions approvals. Open since 2026-09-23.
- [#116830](https://github.com/NousResearch/hermes-agent/pull/116830) — Windows surrogate stdin tests. Open since 2026-09-20.

Overall, Hermes Agent is actively maintained and closing meaningful issues, but the P1 gateway/memory/Windows cluster and several long-lived P2 platform bugs are the main project-health risks to watch.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw (nearai/ironclaw) — Project Digest
**Period:** 2026-10-11 → 2026-10-12 | **Generated:** 2026-10-12

---

## 1. Today's Overview

IronClaw recorded a maintenance-only day: **0 issues updated, 0 new releases, and 7 PRs touched**, with every single PR authored by `dependabot[bot]`. No human-authored code, documentation, or discussion activity was observed in the 24-hour window. Three dependency PRs were closed/merged (#8114, #8103, #8123) and four remain open (#8134, #8132, #8133, #7834), continuing the project's steady dependency-hygiene cadence. Activity assessment: **low-to-moderate volume, low engineering risk, no user-facing velocity** — the repository appears to be in a quiet maintenance phase rather than active feature development.

---

## 2. Releases

**No new releases in this period.** No changelog, breaking-change, or migration data available.

---

## 3. Project Progress

No feature or fix work merged today. All three closed PRs were automated dependency bumps:

| PR | Scope | Content | Labels | Link |
|---|---|---|---|---|
| #8114 | Rust deps ("everything-else") | 31 updates; thiserror `2.0.20→2.0.21`, uuid `1.24.0→1.26.1`, base64, etc. | size: XL, risk: low, contributor: experienced | [PR #8114](https://github.com/nearai/ironclaw/pull/8114) |
| #8103 | GitHub Actions | 8 updates; claude-code-action `1.0.183→1.0.228`, setup-node `4.0.2→7.0.0` | dependencies, github_actions | [PR #8103](https://github.com/nearai/ironclaw/pull/8103) |
| #8123 | tokio-ecosystem | 3 updates; tokio-test `0.4.5→0.4.6`, tower-http, tokio-tungstenite | dependencies, rust | [PR #8123](https://github.com/nearai/ironclaw/pull/8123) |

**Observation:** The closure of #8114/#8103/#8123 coincides with the opening of newer, wider-band bumps (#8134, #8133, #8132) covering the same dependency groups at higher target versions. This is the typical dependabot "supersede and re-raise" cycle rather than independent progress.

**Open PRs awaiting merge/CI:**
- [#8134](https://github.com/nearai/ironclaw/pull/8134) — everything-else group, **34 updates** (uuid → `1.27.0`, thiserror → `2.0.21`) — next iteration of #8114
- [#8132](https://github.com/nearai/ironclaw/pull/8132) — tokio-ecosystem group, 4 updates — next iteration of #8123
- [#8133](https://github.com/nearai/ironclaw/pull/8133) — actions group, 10 updates (claude-code-action → `1.0.247`, setup-node → `7.1.0`) — next iteration of #8103
- [#7834](https://github.com/nearai/ironclaw/pull/7834) — wasm group (wasmtime, wasmtime-wasi, wit-component, wit-parser)

---

## 4. Community Hot Topics

**No community discussion signal available today.** All 7 PRs report `Comments: undefined` (no comment data) and **0 👍 reactions** each. There were **no issues updated in the last 24 hours**, so there is no topic with meaningful comment or reaction volume to rank.

**Underlying read:** With zero issue traffic and zero reactions on dependabot PRs, there is no evidence of community engagement pressure in this window. The project's inbound conversation is effectively silent — worth watching, as sustained silence on an open-source agent project can indicate either a stable/mature codebase or a lull in community participation.

---

## 5. Bugs & Stability

**No bugs, crashes, or regressions were reported or updated on 2026-10-11/12.** Zero issues were touched, and no PR in this set references a bug fix.

- Severity-ranked defect list: **empty**.
- Fix PRs in flight: **none applicable**.

Stability note (non-defect): PR [#7834](https://github.com/nearai/ironclaw/pull/7834) is labeled `risk: medium` for the **wasm/wasmtime** runtime group — the only non-trivial risk label among open PRs — and is the most stability-relevant item to monitor because it touches the sandboxing/execution layer.

---

## 6. Feature Requests & Roadmap Signals

**No feature requests were filed or updated today** (0 issues). There is no direct roadmap signal from user input in this period.

**Inferred signals from dependency direction** (weak, engineering-side only):
- Repeated tokio / tower-http / tokio-tungstenite bumps suggest active investment in **async runtime and HTTP/WebSocket transport** paths → likely server/agent-communication surface.
- The persistent wasm group bump ([#7834](https://github.com/nearai/ironclaw/pull/7834)) suggests **WASM-based plugin/tool sandboxing** remains a core architectural bet.
- Large `anthropics/claude-code-action` jumps in CI imply **heavy reliance on Claude-based automation** in the project's own workflow.

**Next-version prediction:** No user-facing feature can be credibly predicted from this dataset; the next release window, if any, would most likely be a dependency-refresh release driven by #8134/#8132/#8133.

---

## 7. User Feedback Summary

**No user feedback was captured in this window.** There are no issues, no comments, and no reactions to analyze, so no pain points, use cases, or satisfaction/dissatisfaction signals can be reported without speculation.

Data caveat: the `Comments` field for all PRs is reported as `undefined`, meaning comment-thread metrics are unavailable rather than confirmed zero for discussion depth.

---

## 8. Backlog Watch

The following open PRs are the items most deserving maintainer attention:

| PR | Age | Why it needs attention | Link |
|---|---|---|---|
| **#7834** — wasm group (4 updates) | Open since **2026-08-23** (~50 days) | Longest-stalled open item; `size: L`, `risk: medium`; touches wasmtime/wit toolchain — high blast radius if left unmerged and later rebased | [PR #7834](https://github.com/nearai/ironclaw/pull/7834) |
| **#8134** — everything-else (34 updates) | Opened 2026-10-11 | Spans 34 packages across the repo; large surface for CI/regression validation before merge | [PR #8134](https://github.com/nearai/ironclaw/pull/8134) |
| **#8132** — tokio-ecosystem (4 updates) | Opened 2026-10-11 | Core async runtime; merge promptly to avoid rebase churn against #7834 | [PR #8132](https://github.com/nearai/ironclaw/pull/8132) |
| **#8133** — actions group (10 updates) | Opened 2026-10-11 | Major Actions version jumps (setup-node `4→7`) may affect CI pipeline behavior | [PR #8133](https://github.com/nearai/ironclaw/pull/8133) |

**Backlog risk assessment:** The absence of any issue backlog is itself worth noting — either the project's issue tracker is not the primary intake channel, or Issue activity is being handled out-of-band. With no PRs older than #7834 remaining open, maintainer PR latency is otherwise healthy.

---

### Health Summary

| Dimension | Signal |
|---|---|
| Release cadence | ⚪ None this period |
| Code contribution (human) | 🔴 None observed |
| Dependency hygiene | 🟢 Active — 3 merged, 4 open |
| Issue triage | ⚪ No activity (0 issues) |
| Community engagement | 🔴 No comments/reactions recorded |
| Stability | 🟢 No bugs or regressions reported |
| Backlog risk | 🟡 #7834 stale ~50 days (wasm, medium risk) |

*All findings are strictly derived from the supplied GitHub activity snapshot for 2026-10-11/12; absence of data is reported as "none observed" rather than inferred absence of the condition itself.*

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw Project Digest — 2026-10-12

*Source: github.com/agentscope-ai/QwenPaw*

---

## 1. Today's Overview

QwenPaw showed **high maintenance throughput** on 2026-10-12: 13 issues and 23 PRs were touched in the last 24 hours, with 13 PRs merged/closed against 10 still open. Notably, a large fraction of today's closures are **long-stalled PRs from March–July 2026** (MCP hardening, context-overflow recovery, plugin-market version routing, memory backend), indicating an active backlog-cleanup push rather than purely new work. Issue traffic remains dominated by **stability regressions in v2.2.x Desktop and Codex-backend paths**, plus a growing cluster of Console/UX polish requests. No new release shipped, so users on v2.2.1/2.2.2-beta are running fixes only from unreleased branches. Overall health is **good on contribution flow, strained on release cadence** — the volume of open, unfixed v2.2.x bug reports is accumulating faster than releases can absorb them.

---

## 2. Releases

**None.** No new releases in the last 24 hours. Latest observable user-reported versions in the wild are v2.2.1 (stable), v2.2.2-beta.4, and v2.1.1b2 (older Desktop build). The absence of a release is notable given the number of merged fixes queued (see §3).

---

## 3. Project Progress — Merged / Closed PRs

13 PRs were merged or closed. The clear themes:

**Plugin ecosystem correctness**
- [#5661](https://github.com/agentscope-ai/QwenPaw/pull/5661) — Filter plugin market and CDN catalog by QwenPaw version compatibility (`qwenpaw_version` / `min_version` / `max_version`).
- [#5612](https://github.com/agentscope-ai/QwenPaw/pull/5612) — Plugin market download routes isolated by major version (v1.x vs v2.x incompatibility fix).

**Runtime resilience**
- [#2783](https://github.com/agentscope-ai/QwenPaw/pull/2783) — Auto-recover context-window overflow via one-shot memory compaction + retry.
- [#2520](https://github.com/agentscope-ai/QwenPaw/pull/2520) — Preflight active-model availability; reject activation with explicit errors.
- [#1977](https://github.com/agentscope-ai/QwenPaw/pull/1977) — Harden MCP teardown, cron/channel exception handling.
- [#2052](https://github.com/agentscope-ai/QwenPaw/pull/2052) — Degrade MCP connection errors to tool-level failures instead of chat-visible errors.

**MCP + tooling**
- [#1978](https://github.com/agentscope-ai/QwenPaw/pull/1978) — MCP import validation/preview + runtime status diagnostics.
- [#4707](https://github.com/agentscope-ai/QwenPaw/pull/4707) — Robust `ToolResponse` text-block handling (attribute vs dict access crash).

**Configurability**
- [#5732](https://github.com/agentscope-ai/QwenPaw/pull/5732) — New `none` memory backend to fully disable the memory system via Console.

**Desktop UX**
- [#5569](https://github.com/agentscope-ai/QwenPaw/pull/5569) — Two-phase splash loading to eliminate the 5–30s Windows startup white screen.

Three additional PRs closed without detail in the supplied dataset.

**Assessment:** the closures are mostly *foundational hardening* rather than headline features. Several directly address pain points still being reported by users today (context overflow, MCP noise, plugin incompatibility), which suggests the fixes may resolve open issues once a release ships.

---

## 4. Community Hot Topics

| Rank | Item | Comments | Signal |
|---|---|---|---|
| 1 | [#7311](https://github.com/agentscope-ai/QwenPaw/issues/7311) — v2.1.1b2 missing `_qwenpaw_remote_backend`, all tools broken | 5 | **Closed today.** Highest-engagement item; total tool failure on Windows Desktop. |
| 2 | [#7016](https://github.com/agentscope-ai/QwenPaw/issues/7016) — Streaming tool-call 404 / `Tool call not found` | 4 | Still open since 2026-08-14 — ~2 months unresolved. |
| 3 | [#8174](https://github.com/agentscope-ai/QwenPaw/issues/8174) — Tavily provider has no API-key config surface | 3 | Paid-quota users blocked; active discussion. |

**Underlying needs analysis:**
- **#7311 / #7016** both point at **fragile remote-backend and tool-call lifecycle plumbing**. The 404 loop in #7016 (`offload` endpoint repeatedly 404ing during streaming) suggests a session/tool-call registry desynchronization under streaming — the same class of failure that produces "all tools broken" symptoms. This is the single most impactful reliability area in the project right now.
- **#8174** reflects a **monetization/self-hosting expectation mismatch**: QwenPaw ships a built-in provider that silently uses keyless anonymous mode, with no UI to inject a user key. Users with paid API access are effectively blocked — a common pattern when built-in integrations hard-code a credential path.

---

## 5. Bugs & Stability

Ranked by severity:

**Critical**
- [#7311](https://github.com/agentscope-ai/QwenPaw/issues/7311) — *[CLOSED]* All agent tools fail (`ModuleNotFoundError: _qwenpaw_remote_backend`) on v2.1.1b2 Windows Desktop; module absent from `_internal`. **Closed today** — no fix PR explicitly linked in the dataset, so resolution should be verified before users assume a fix shipped.

**High**
- [#8173](https://github.com/agentscope-ai/QwenPaw/issues/8173) — Codex backend silently loses harness↔codex connection after restart; leaves a **permanent zombie `running` entry that blocks all recovery paths** (only chat/stop works). Affects v2.2.1. No fix PR identified.
- [#7016](https://github.com/agentscope-ai/QwenPaw/issues/7016) — Streaming tool-call offload returns 404 in a loop; tool-call not found. Open ~59 days. No fix PR identified.
- [#8179](https://github.com/agentscope-ai/QwenPaw/issues/8179) — `qwenpaw skills install <github-url>` **silently corrupts all binary files** (non-UTF-8 → U+FFFD), breaking images/fonts in installed skills. Data-integrity class bug, v2.2.1 Docker.
- [#8175](https://github.com/agentscope-ai/QwenPaw/issues/8175) — `ProviderManager.active_model` not loaded at startup, combined with silent `except Exception: pass` in `_load_agent_model_settings`; causes **nightly cron (dream) failures** across multi-agent deployments.

**Medium**
- [#8184](https://github.com/agentscope-ai/QwenPaw/issues/8184) — Doom-loop gate kills **legitimate task-authorized polling**; cron then records false success after termination. False-positive guard logic.
- [#8185](https://github.com/agentscope-ai/QwenPaw/issues/8185) — `desktop.log` never rotates on Windows; sidecar tee handle defeats `_SafeRotatingFileHandler`. Disk-growth risk.
- [#8186](https://github.com/agentscope-ai/QwenPaw/issues/8186) — Console message-navigation jump overrides manual wheel scrolling, snapping back to anchor for ~1.6s.
- [#8176](https://github.com/agentscope-ai/QwenPaw/issues/8176) — Plugin Store "Update All" skips third-party plugins (v2.2.1).

**Low / Config**
- [#8177](https://github.com/agentscope-ai/QwenPaw/issues/8177) — Hardcoded `/console` basename blocks reverse-proxy subpath deployments.

**Fix PRs in flight:**
- [#8188](https://github.com/agentscope-ai/QwenPaw/pull/8188) — Fixes approval decision race (references #8105): decision lost → tool rejected despite prior approval.
- [#8183](https://github.com/agentscope-ai/QwenPaw/pull/8183) — Preserves cancellation reasons; returns `ERROR` for recoverable timeouts so the parent agent loop continues.
- [#8182](https://github.com/agentscope-ai/QwenPaw/pull/8182) — Server-side persistence of per-chat approval level (directly relevant to #8180).
- [#7973](https://github.com/agentscope-ai/QwenPaw/pull/7973) — Recover from rejected media URLs (HTTP 400 on media → perpetual conversation failure).

---

## 6. Feature Requests & Roadmap Signals

| Issue / PR | Request | Likelihood for next release |
|---|---|---|
| [#8174](https://github.com/agentscope-ai/QwenPaw/issues/8174) | Tavily API-key config surface | **Medium-High** — small UI/config change, high user impact |
| [#8181](https://github.com/agentscope-ai/QwenPaw/issues/8181) | Session-level change notifications (long-lived event channel) to avoid hard refresh | **Medium** — architectural, touches SSE/streaming layer |
| [#8180](https://github.com/agentscope-ai/QwenPaw/issues/8180) + [#8182](https://github.com/agentscope-ai/QwenPaw/pull/8182) | Session-scoped approval level persisted server-side | **High** — PR already open, closes a documented UX/semantics gap |
| [#8177](https://github.com/agentscope-ai/QwenPaw/issues/8177) | Configurable console basename for reverse proxies | **Medium** — common self-host ask, low complexity |
| [#8176](https://github.com/agentscope-ai/QwenPaw/issues/8176) | "Update All" should include third-party plugins | **Medium-High** — pairs with today's plugin-market PR closures |
| [#8178](https://github.com/agentscope-ai/QwenPaw/pull/8178) | Nested/categorised skill directories | **Medium** — PR open, backward-compatible |
| [#8187](https://github.com/agentscope-ai/QwenPaw/pull/8187) | Extract channels into plugins with subprocess isolation | **Low for next minor** (XXXL, architectural) |
| [#8164](https://github.com/agentscope-ai/QwenPaw/pull/8164) | Native HarmonyOS client (ArkUI/ArkTS) | **Low for next minor** (XXXL, new platform) |
| [#8137](https://github.com/agentscope-ai/QwenPaw/pull/8137) | Official "reduced effects" accessibility tier | **Medium-High** — small, self-contained |
| [#8156](https://github.com/agentscope-ai/QwenPaw/pull/8156) | Coding-CLI management endpoints for worker containers | **Medium** — multi-agent deployment need |

**Prediction:** The next release is most likely dominated by **Console/server-state consistency** (#8182, #8137, #8177) and **plugin-market correctness** (#8176, building on today's #5661/#5612 closures), with stability fixes from #8188/#8183/#7973 folded in.

---

## 7. User Feedback Summary

**Pain points (dissatisfaction):**
- **Tool reliability is the #1 complaint** — #7311 (all tools broken), #7016 (404 loops), #8184 (doom-loop false positives), #8188-related approval races. Users are hitting failures at the *tool-execution layer*, which is core to an agent product.
- **Silent failures are a recurring motif**: #8175 (silent `except Exception: pass`), #8179 (silent binary corruption), #8173 (silent connection break leaving zombie state). Users explicitly call out "no log, no error" behavior.
- **Self-hosting friction**: #8177 (reverse-proxy subpath) and #8179 (Docker install) show the self-hosted segment is a real, vocal constituency with unmet deployment needs.
- **Credential/config opacity**: #8174 — users cannot tell that built-in Tavily uses keyless mode, nor supply their own key.

**Satisfaction signals:**
- High-quality, well-structured bug reports (structured templates, version pins, environment details) indicate an **engaged, technically capable community** willing to invest effort in the project.
- Several first-time and repeat contributors are active (#8178 first-time, #6499 first-time), which is a positive health signal.

**Use cases revealed:** multi-agent Docker deployments with per-workspace configs (#8175), cron/dream overnight jobs, multi-channel sessions needing cross-device consistency (#8181), and worker containers running third-party coding CLIs (#8156).

---

## 8. Backlog Watch — Needs Maintainer Attention

| Item | Age | Why it matters |
|---|---|---|
| [#7016](https://github.com/agentscope-ai/QwenPaw/issues/7016) — Streaming tool-call 404 | **59 days open** | High-severity, repeatedly reproduced, no linked fix. Longest-lived unresolved core bug. |
| [#7973](https://github.com/agentscope-ai/QwenPaw/pull/7973) — Recover from rejected media URLs | **18 days open** | Fix ready but unmerged; conversation-breaking bug persists in the meantime. |
| [#6499](https://github.com/agentscope-ai/QwenPaw/pull/6499) — Add Atlas Cloud provider | **~77 days open** | First-time-contributor PR, self-contained, blocks contributor momentum if left idle. |
| [#8183](https://github.com/agentscope-ai/QwenPaw/pull/8183) — Preserve cancellation reasons | 0 days (new) | Directly relevant to #8184-class false-positive terminations; worth fast-tracking together. |
| [#8156](https://github.com/agentscope-ai/QwenPaw/pull/8156) — Coding-CLI management endpoints | 3 days | Multi-agent ops gap; no maintainer response evident. |
| [#1977](https://github.com/agentscope-ai/QwenPaw/pull/1977), [#2783](https://github.com/agentscope-ai/QwenPaw/pull/2783), [#2520](https://github.com/agentscope-ai/QwenPaw/pull/2520) | **~200 days each (now closed)** | Welcome resolution, but the ~6-month latency on meaningful robustness PRs is a **review-throughput red flag** worth addressing structurally. |

**Backlog health verdict:** The project is clearly *processing* its backlog today, but the multi-month lag on #1977/#2783/#2520/#4707 shows the bottleneck is **maintainer review capacity**, not contributor supply. Combined with no release in 24h and a cluster of open v2.2.x regressions, the highest-leverage action would be to cut a release that ships the just-merged fixes.

---

*Digest generated from GitHub activity data for the 24-hour window ending 2026-10-12. Comment counts for PRs were unavailable (`undefined`) in the source data; PRs are therefore ranked by recency and scope label.*

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-10-12

## 1. Today's Overview

ZeroClaw showed **high-velocity, low-throughput engineering activity** on 2026-10-12: 45 PRs and 16 issues were touched in the last 24 hours, but only 2 PRs closed and 0 issues closed, with no new releases. The project is deep in a **design-heavy phase**, with six high-risk RFCs open simultaneously covering an A2A protocol crate, ephemeral agent swarms, RAG/knowledge corpus, provider routing, and runtime/gateway separation — most awaiting maintainer review or author action. A large, stacked body of PR work from a small set of contributors (`IftekharUddin`, `JordanTheJet`, `NiuBlibing`) is driving plugin/WASM hosting, capability-boundary refactors, and gateway hardening, but nearly all of it sits open and unmerged. Meanwhile, a steady drip of user-facing bug reports (pairing codes, web chat focus, memory consolidation) indicates real-world onboarding friction that is being filed faster than it is being resolved. Overall health: **strong contributor engagement and clear roadmap direction, but a widening merge backlog and a notable gap between accepted trackers/RFCs and shipped code.**

## 2. Releases

**None.** No new releases in the last 24 hours. Several open PRs carry `release:v0.8.6` and `release:v0.9.0` labels, indicating both lines are still in-flight:
- [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) — Tracker for Phase 2 runtime work (v0.8.6) and Phase 3 gateway separation (v0.9.0)
- [#11187](https://github.com/zeroclaw-labs/zeroclaw/pull/11187), [#11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261), [#11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262), [#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347) — all tagged for v0.8.6

No breaking changes or migration notes to report today.

## 3. Project Progress

Only **2 PRs were merged/closed** in the 24-hour window, versus 43 still open — a notably lopsided ratio.

**Closed today:**
- [#11531](https://github.com/zeroclaw-labs/zeroclaw/pull/11531) `[CLOSED]` — `fix(tunnel): report the URL tailscale actually serves the gateway on`. Fixed a real misreporting bug where the gateway advertised `https://<host>:<local_port>` while Tailscale actually served HTTPS on port 443. Small (`size:XS`), from a distinguished contributor.

**Advancing but not yet merged (high-signal clusters):**
- **Plugin/WASM infrastructure stack** — [#11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261), [#11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262), [#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347), [#11309](https://github.com/zeroclaw-labs/zeroclaw/pull/11309), [#11311](https://github.com/zeroclaw-labs/zeroclaw/pull/11311), [#11318](https://github.com/zeroclaw-labs/zeroclaw/pull/11318), [#11319](https://github.com/zeroclaw-labs/zeroclaw/pull/11319), [#11320](https://github.com/zeroclaw-labs/zeroclaw/pull/11320) — a deeply stacked chain (release artifacts, quickstart install, webhook ingress, RPC dispatch). Each depends on earlier PRs, so progress is effectively serialized.
- **Runtime capability boundaries** — [#11174](https://github.com/zeroclaw-labs/zeroclaw/pull/11174), [#11187](https://github.com/zeroclaw-labs/zeroclaw/pull/11187), [#11526](https://github.com/zeroclaw-labs/zeroclaw/pull/11526) — capability-taking turn constructors and honoring supplied capability boundaries.
- **Provider flexibility** — [#9809](https://github.com/zeroclaw-labs/zeroclaw/pull/9809) (multiple models per provider profile) and [#11659](https://github.com/zeroclaw-labs/zeroclaw/pull/11659) (snake_case context-overflow codes).

## 4. Community Hot Topics

Ranked by comment count among updated items:

| Item | Comments | Theme |
|---|---|---|
| [#11254](https://github.com/zeroclaw-labs/zeroclaw/issues/11254) RFC: A2A protocol crate | 7 | Cross-cutting architecture / wire model |
| [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) Runtime & gateway delivery tracker | 6 | Release planning |
| [#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025) RFC: ephemeral agent swarms w/ TUI | 4 | Multi-agent orchestration |
| [#11074](https://github.com/zeroclaw-labs/zeroclaw/issues/11074) RFC: `search_routes` provider routing | 4 | Tool/search routing |
| [#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235) RFC: Knowledge corpus (RAG) | 4 | Memory/retrieval |

**Underlying needs analysis:**
- **Interoperability** dominates. [#11254](https://github.com/zeroclaw-labs/zeroclaw/issues/11254) builds on established A2A work (#9106, #7763, #8274) and signals users want ZeroClaw to speak a standard agent-to-agent protocol rather than a bespoke one.
- **Orchestration of multiple agents** is the second recurring theme — [#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025) (swarms), [#11664](https://github.com/zeroclaw-labs/zeroclaw/issues/11664) (Dark Factory workflow), and [#11667](https://github.com/zeroclaw-labs/zeroclaw/issues/11667) (command-policy inheritance) all point to users outgrowing the single-agent model.
- **Retrieval-augmented grounding** ([#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235)) reflects demand for the agent to answer from operator-owned documents rather than only its parametric knowledge.

## 5. Bugs & Stability

Ranked by severity. No fix PRs appear linked to the newer reports yet.

**S1 — Workflow blocked:**
- [#11648](https://github.com/zeroclaw-labs/zeroclaw/issues/11648) `[Bug]` — Web dashboard pairing input is limited to **6 digits**, but the backend default `PairingCodePolicy` now issues **32 alphanumeric characters**. The CLI prints the full code, but the web dialog rejects it, effectively blocking web onboarding. High user impact; likely a quick frontend fix.

**S2 — Degraded behavior:**
- [#11655](https://github.com/zeroclaw-labs/zeroclaw/issues/11655) `[Bug]` — With the **default** `embedding_provider = "none"`, `consolidate_turn` / `conflict::check_and_resolve_conflicts` **soft-hides unrelated Core memories** when no vector scores exist. This is a data-loss-adjacent regression affecting the default configuration, making it the most concerning stability issue today.

**S3 — Minor:**
- [#11658](https://github.com/zeroclaw-labs/zeroclaw/issues/11658) `[Bug]` — Web chat input does not regain focus after a reply, requiring an extra click. Low severity but annoying UX friction in the primary interface.

**Related open correctness/architecture issues:**
- [#10290](https://github.com/zeroclaw-labs/zeroclaw/issues/10290) — Thread live config handle through `agent::run` for detached peer/subagent turns (SSRF policy propagation gap).
- [#7692](https://github.com/zeroclaw-labs/zeroclaw/issues/7692) (accepted) — Gateway router body-limit and cron-run timeout wiring tests still missing; **PR [#10310](https://github.com/zeroclaw-labs/zeroclaw/pull/10310) implements these tests** and is open, awaiting author action.

## 6. Feature Requests & Roadmap Signals

**New/updated requests today:**
- [#11664](https://github.com/zeroclaw-labs/zeroclaw/issues/11664) — **Level 5 Dark Factory Workflow**: fully orchestrated agent teams with lifecycle management.
- [#11662](https://github.com/zeroclaw-labs/zeroclaw/issues/11662) — **Separate Channels from Core**: modular channel interface (IPC, MQTT, etc.) to drop unused channels as "deadweight and security risk."
- [#11668](https://github.com/zeroclaw-labs/zeroclaw/issues/11668) — Make gateway `MAX_BODY_SIZE` configurable and return a proper error code + size (driven by multi-agent A2A relay of long-context questions).
- [#11657](https://github.com/zeroclaw-labs/zeroclaw/issues/11657) — Configurable target category for turn consolidation (`[memory] consolidation_category`).
- [#11667](https://github.com/zeroclaw-labs/zeroclaw/issues/11667) — Decide whether delegated turns inherit the caller's command policy.

**Prediction for next version (v0.8.6 / v0.9.0):**
- **Configurable gateway body limits** ([#11668](https://github.com/zeroclaw-labs/zeroclaw/issues/11668)) is a small, well-scoped ask with a concrete use case (A2A long-context relay) and is a strong candidate for a near-term patch.
- **Channel modularization** ([#11662](https://github.com/zeroclaw-labs/zeroclaw/issues/11662)) aligns directly with the v0.9.0 "gateway separation" theme in [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) — plausible for v0.9.0.
- **Swarm/Dark Factory orchestration** ([#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025), [#11664](https://github.com/zeroclaw-labs/zeroclaw/issues/11664)) is likely post-v0.9.0, as it depends on the still-unmerged A2A and capability-boundary work.
- **Multi-model provider profiles** ([#9809](https://github.com/zeroclaw-labs/zeroclaw/pull/9809), open since Aug 7) and **RAG/knowledge corpus** ([#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235)) are larger bets likely tracked for v1.0.

## 7. User Feedback Summary

**Pain points:**
- **Onboarding is broken in the web UI.** The pairing-code mismatch ([#11648](https://github.com/zeroclaw-labs/zeroclaw/issues/11648), S1) means a user following documented flows hits a hard wall in the browser. Combined with the focus bug ([#11658](https://github.com/zeroclaw-labs/zeroclaw/issues/11658)), the web dashboard is generating the most immediate negative feedback.
- **Default settings cause silent data loss.** [#11655](https://github.com/zeroclaw-labs/zeroclaw/issues/11655) affects the *out-of-the-box* configuration (`embedding_provider = "none"`), so users who never customize settings are exposed. This undermines trust in the memory subsystem.
- **Multi-agent users feel friction.** [#11668](https://github.com/zeroclaw-labs/zeroclaw/issues/11668) reports gateway body limits rejecting long-context A2A messages "in < 1 s with the bare string `length limit exceeded`" — opaque errors blocking legitimate workflows.
- **Bloat and security anxiety.** [#11662](https://github.com/zeroclaw-labs/zeroclaw/issues/11662) voices a clear user desire to shed unused channels described as "deadweight" and a "security risk," suggesting users want lean, auditable deployments.

**Satisfaction signals:** Users are deeply engaged — filing detailed RFCs, referencing specific commit SHAs and PR numbers, and building multi-agent topologies. This level of sophistication suggests a satisfied power-user base, but the accumulation of unresolved RFCs and the slow merge rate risk frustrating them.

## 8. Backlog Watch

Items needing maintainer attention due to age, size, or stalled status:

- [#9809](https://github.com/zeroclaw-labs/zeroclaw/pull/9809) — **Open since 2026-08-07 (66+ days)**, `size:XL`, `risk:high`, tagged `needs-author-action`. Multi-model-per-provider is a frequently requested capability stuck for over two months. Highest-priority backlog item.
- [#7692](https://github.com/zeroclaw-labs/zeroclaw/issues/7692) — **Open since 2026-06-15 (~4 months)**, `status:accepted`, `risk:high`. Gateway test coverage gap; its implementing PR [#10310](https://github.com/zeroclaw-labs/zeroclaw/pull/10310) is also open and flagged `needs-author-action`.
- [#9597](https://github.com/zeroclaw-labs/zeroclaw/issues/9597) — **Open since 2026-07-31**, `status:in-progress`, `follow-up`, `risk:high`. Durable/attributable peer-agent turns; foundational for the swarm work but stalled.
- [#10290](https://github.com/zeroclaw-labs/zeroclaw/issues/10290) — **Open since 2026-08-24**, `follow-up`, `risk:high`, only 1 comment. Live-config propagation for detached turns (SSRF policy correctness); security-relevant and under-discussed.
- [#11289](https://github.com/zeroclaw-labs/zeroclaw/pull/11289) — Marked **`do-not-merge`** despite being a substantial security PR (stable RPC denial reason identifiers); likely blocked on a maintainer decision.
- **Stacked plugin PR chain** ([#11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261) → [#11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262) → [#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347) etc.) — 8+ interdependent PRs all open. Any single merge decision here could unblock the v0.8.6 plugin hosting release; maintainer throughput on this chain is the key bottleneck to watch.

**Health note:** The project is architecturally ambitious and attractively active, but the **merge-to-open ratio (2:43)**, the volume of `needs-author-action` / `needs-maintainer-review` labels, and multiple XL PRs open for 1–2 months indicate a **review bottleneck**. Clearing the plugin stack and the long-stalled [#9809](https://github.com/zeroclaw-labs/zeroclaw/pull/9809) would materially improve both release cadence and contributor retention.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*