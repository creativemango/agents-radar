# OpenClaw Ecosystem Digest 2026-10-13

> Issues: 500 | PRs: 500 | Projects covered: 5 | Generated: 2026-10-13 03:08 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# OpenClaw Project Digest — 2026-10-13

## 1. Today's Overview
OpenClaw remains a very high-activity project: **500 issues and 500 PRs were updated in the last 24h**, with **335 open/active issues** and **314 open PRs**. Throughput is strong — **165 issues closed** and **186 PRs merged/closed** in the same window. Two releases landed, both heavily focused on update recovery, handoff reliability, and Windows/macOS install paths. Health is mixed: maintainers are shipping recovery fixes and closing high-severity session/crash items, but multiple **P0 update, Windows service, memory-leak, and disk-growth issues remain open**. Overall: active and responsive, with update/install reliability as the dominant risk area.

## 2. Releases
| Release | Type | Key Changes | Breaking / Migration Notes |
|---|---|---|---|
| [v2026.10.5-beta.2](https://github.com/openclaw/openclaw/releases) | Beta | `openclaw update` settles stale handoff leases, completed receipts, and legacy 2026.9.8 recovery records instead of stopping on `update-recovery-pending`; keeps service heap limit and webhook listeners; handles Windows short-* paths (release note truncated). | No explicit breaking changes in supplied notes. Migration focus: stalled/legacy update recovery. |
| [v2026.10.2](https://github.com/openclaw/openclaw/releases) | Stable-channel upgrade | First upgrade from stable 2026.10.1 now completes cleanly; successful in-place updates no longer end in errors; Windows, cloned-filesystem, and private-Node update paths finish instead of stalling or rolling back (#168849, #169031). | No explicit breaking changes in supplied notes. Migration focus: 2026.10.1 upgrades and in-place update recovery. |

## 3. Project Progress
- **186 PRs merged/closed** and **165 issues closed** in the last 24h.
- Visible closed PRs in the top activity slice:
  - [#170497](https://github.com/openclaw/openclaw/pull/170497) — restore original Control UI startup performance budgets.
  - [#170533](https://github.com/openclaw/openclaw/pull/170533) — consolidate chat and Solid presentation paths.
- Closed issues show progress across crash, session, update, and model-correctness areas:
  - [#137729](https://github.com/openclaw/openclaw/issues/137729) — unguarded `.trim()` crash fix pattern.
  - [#157126](https://github.com/openclaw/openclaw/issues/157126) — claude-cli MCP bridge request-scope issue.
  - [#167506](https://github.com/openclaw/openclaw/issues/167506) — deleted temporary handoff DB blocking update recovery.
  - [#69118](https://github.com/openclaw/openclaw/issues/69118) — Claude CLI group-channel session reset.
  - [#158936](https://github.com/openclaw/openclaw/issues/158936) — macOS readiness watchdog restart loop.
  - [#168750](https://github.com/openclaw/openclaw/issues/168750) — update activation Doctor failure.
  - [#127239](https://github.com/openclaw/openclaw/issues/127239) — context-window fallback to hardcoded 200k.
  - [#9986](https://github.com/openclaw/openclaw/issues/9986) — model fallback on context length exceeded.
  - [#119692](https://github.com/openclaw/openclaw/issues/119692) — MiniMax-M3 usage-accounting drop.
  - [#95746](https://github.com/openclaw/openclaw/issues/95746) — memory-core dreaming context exhaustion.
  - [#39811](https://github.com/openclaw/openclaw/issues/39811) — unvalidated model names.
  - [#164214](https://github.com/openclaw/openclaw/issues/164214) — package publication recovery stuck in `publishing`.
- Open PRs advancing major themes: update recovery, Doctor repairs, gateway responsiveness, CLI request-scope isolation, secrets input safety, Control UI Solid migration, plugin runtime control, memory-wiki status, and session/transcript authority.

## 4. Community Hot Topics
### Most Active Issues
| Rank | Issue | Comments | Underlying Need |
|---:|---|---:|---|
| 1 | [#159356](https://github.com/openclaw/openclaw/issues/159356) — llama.cpp manager reports ready while embedding child exits; HTTP 500 | 26 | Child-process health checks and memory-pressure diagnostics for embedding stacks. |
| 2 | [#159662](https://github.com/openclaw/openclaw/issues/159662) — `prepared-model-catalog.worker.js` unbounded memory leak, ~4–5 GB/h | 22 | Urgent memory-leak fix independent of provider/plugin config. |
| 3 | [#160959](https://github.com/openclaw/openclaw/issues/160959) — Gateway blocks for minutes capturing large external plugins | 20 | Non-blocking plugin capture / event-loop starvation fix. |
| 4 | [#50093](https://github.com/openclaw/openclaw/issues/50093) — WhatsApp backfill missed messages after reconnection | 20 | Channel reconnection recovery and message-loss prevention. |
| 5 | [#97616](https://github.com/openclaw/openclaw/issues/97616) — unreaped hook/tool child processes and zombie accumulation | 18 | Process reaping and long-running runtime degradation fix. |
| 6 | [#78308](https://github.com/openclaw/openclaw/issues/78308) — MCP tool-call channel-mediated approval / consent envelope | 18 | Security and consent gating for MCP side effects. |
| 7 | [#137729](https://github.com/openclaw/openclaw/issues/137729) — unguarded `.trim()` crash in transcript replay/error classification | 14 | Defensive coding; closed. |
| 8 | [#157126](https://github.com/openclaw/openclaw/issues/157126) — claude-cli MCP bridge inherits first request scope | 14 | Correct request-scope isolation; closed, with related PR work. |
| 9 | [#156112](https://github.com/openclaw/openclaw/issues/156112) — `openclaw update` fails at global install swap | 14 | Updater reliability when direct `npm install -g` succeeds. |
| 10 | [#114414](https://github.com/openclaw/openclaw/issues/114414) — dated TODO sweep | 13 | Maintainability/tech-debt cleanup. |
| 11 | [#146860](https://github.com/openclaw/openclaw/issues/146860) — Windows managed update handoff cannot obtain process-start identity | 12 | Windows Scheduled Task / InteractiveToken update reliability. |
| 12 | [#101929](https://github.com/openclaw/openclaw/issues/101929) — context-overflow precheck over-counts 2.3–2.6x | 12 | Accurate token accounting to avoid false truncation recovery. |

### Hot PRs
PR comment counts were not supplied in the snapshot; the following are high-signal PRs from the top activity slice.
- [#170534](https://github.com/openclaw/openclaw/pull/170534) — fix silent turn endings without fabricated answers; XL, P2, merge risks.
- [#169299](https://github.com/openclaw/openclaw/pull/169299) — Doctor prioritizes unresolved findings and explains repairs.
- [#166782](https://github.com/openclaw/openclaw/pull/166782) — keep Gateway responsive during reply-tagged streams.
- [#157144](https://github.com/openclaw/openclaw/pull/157144) — CLI bridge tool calls in their own run request scope; incorporated into main via #165432.
- [#167250](https://github.com/openclaw/openclaw/pull/167250) — prevent malformed UTF-8 from overwriting stored secrets.
- [#168368](https://github.com/openclaw/openclaw/pull/168368) — Mattermost agent buttons render as plain text.
- [#170527](https://github.com/openclaw/openclaw/pull/170527) — trigger model fallback for provider-wrapped errors.
- [#170548](https://github.com/openclaw/openclaw/pull/170548) — keep manual cron runs from losing tool access.
- [#169172](https://github.com/openclaw/openclaw/pull/169172) — tool-call text no longer leaks into chat output when chunks split a marker.
- [#170553](https://github.com/openclaw/openclaw/pull/170553) — prevent memory-wiki status timeouts on large vaults.
- [#169848](https://github.com/openclaw/openclaw/pull/169848) — `lightContext` ignored by CLI-backend subagents.
- [#170152](https://github.com/openclaw/openclaw/pull/170152) — let plugins steer a session’s active run.
- [#170555](https://github.com/openclaw/openclaw/pull/170555) — join lifecycle publication during Gateway shutdown.
- [#170536](https://github.com/openclaw/openclaw/pull/170536) — stop rechecking qualified target absence in sessions.
- [#169692](https://github.com/openclaw/openclaw/pull/169692) — render New Session with Solid.
- [#170387](https://github.com/openclaw/openclaw/pull/170387) — port shared pickers, dialogs, and toast to Solid 2.

## 5. Bugs & Stability
Ranked by supplied severity. “Fix PR” reflects visible labels/linked PRs in the snapshot.

### P0 / Critical
| Issue | Problem | Fix PR Status |
|---|---|---|
| [#159662](https://github.com/openclaw/openclaw/issues/159662) | Unbounded memory leak in `prepared-model-catalog.worker.js`, ~4–5 GB/h | No visible fix PR. |
| [#160959](https://github.com/openclaw/openclaw/issues/160959) | Gateway blocks for minutes capturing large external plugins (2026.9.6 regression) | No new fix PR. |
| [#156112](https://github.com/openclaw/openclaw/issues/156112) | `openclaw update` fails at global install swap while direct npm install succeeds | Manual-only; no visible fix PR. |
| [#146860](https://github.com/openclaw/openclaw/issues/146860) | Windows managed update handoff stalls with `InteractiveToken` Scheduled Task | No visible fix PR. |
| [#158390](https://github.com/openclaw/openclaw/issues/158390) | Plugin-captures tmp dirs never GC’d; disk fills indefinitely | No new fix PR. |
| [#167652](https://github.com/openclaw/openclaw/issues/167652) | Windows Gateway hangs after 2026.9.9 upgrade despite Doctor verified restart | Needs live repro; no fix. |
| [#160060](https://github.com/openclaw/openclaw/issues/160060) | Windows Gateway silently dies ~54 min after standby thaw | No fix. |
| [#166894](https://github.com/openclaw/openclaw/issues/166894) | Gateway restart hangs 30+ min to 2h on Windows multi-agent installs | No fix. |
| [#138560](https://github.com/openclaw/openclaw/issues/138560) | Windows Control UI update aborts with `managed-service-handoff-failed` | No fix. |
| [#153426](https://github.com/openclaw/openclaw/issues/153426) | `MEMORY.md` / `USER.md` permanently excluded by provenance ratchet | No fix. |

### P1 / High
| Issue | Problem | Fix PR Status |
|---|---|---|
| [#97616](https://github.com/openclaw/openclaw/issues/97616) | Unreaped hook/tool child processes cause zombie accumulation | No fix. |
| [#101929](https://github.com/openclaw/openclaw/issues/101929) | Context-overflow precheck over-counts ~2.3–2.6x vs billed usage | No fix. |
| [#168955](https://github.com/openclaw/openclaw/issues/168955) | Startup regression in 2026.10.1: ready ~1030s vs ~410s on Windows | No fix. |
| [#154891](https://github.com/openclaw/openclaw/issues/154891) | Failed/rolled-back config hot-reload bricks unrelated plugins until restart | No fix. |
| [#132303](https://github.com/openclaw/openclaw/issues/132303) | `agents.list[].tools.deny` not enforced for claude-cli backend | Security; no fix. |
| [#126950](https://github.com/openclaw/openclaw/issues/126950) | Telegram opaque callback payloads silently dropped | No fix. |
| [#128916](https://github.com/openclaw/openclaw/issues/128916) | Errored message-tool-only runs end silent | No fix. |
| [#156353](https://github.com/openclaw/openclaw/issues/156353) | Chat fails after plugin reload with transcript admission identity mismatch | No fix. |
| [#77249](https://github.com/openclaw/openclaw/issues/77249) | Slack reconnect supervisor hangs on zombie WSS | No fix. |
| [#125826](https://github.com/openclaw/openclaw/issues/125826) | Discord voice replies double-spoken and double-written | No fix. |

### New / Recently Updated
- [#170331](https://github.com/openclaw/openclaw/issues/170331) — beta.2 update candidate config read exits 1 and leaves systemd unit unchanged.
- [#170332](https://github.com/openclaw/openclaw/issues/170332) — beta update continues after live state DB changes during original-state capture.
- [#164394](https://github.com/openclaw/openclaw/issues/164394) — Control UI WebChat transcript jitters continuously.
- [#108379](https://github.com/openclaw/openclaw/issues/108379) — duplicate assistant generation for Xiaomi MiMo.
- [#168413](https://github.com/openclaw/openclaw/issues/168413) — `memory.search.extraPaths` content never recalled; stale reindex lock.
- [#99659](https://github.com/openclaw/openclaw/issues/99659) — OpenClaw OOM-killed after companion app connected.

### Potential Fix PRs in Queue
- [#170527](https://github.com/openclaw/openclaw/pull/170527) — model fallback for provider-wrapped errors (fixes #170388).
- [#170548](https://github.com/openclaw/openclaw/pull/170548) — cron manual runs keep tool access (related #170470).
- [#170534](https://github.com/openclaw/openclaw/pull/170534) — silent turn endings without fabricated answers (related #170147, #150539).
- [#166782](https://github.com/openclaw/openclaw/pull/166782) — Gateway responsive during reply-tagged streams (closes #166781).
- [#169172](https://github.com/openclaw/openclaw/pull/169172) — tool-call text leak fix (fixes #169161).
- [#170553](https://github.com/openclaw/openclaw/pull/170553) — memory-wiki status timeout fix (closes #170466).
- [#169848](https://github.com/openclaw/openclaw/pull/169848) — `lightContext` for CLI subagents (closes #169704).
- [#167250](https://github.com/openclaw/openclaw/pull/167250) / [#167251](https://github.com/openclaw/openclaw/pull/167251) — malformed UTF-8 secret/model input safety.
- [#157144](https://github.com/openclaw/openclaw/pull/157144) — CLI bridge request scope (incorporated via #165432; related #157126).
- [#170556](https://github.com/openclaw/openclaw/pull/170556) — worker transcript authority.
- [#170555](https://github.com/openclaw/openclaw/pull/170555) — Gateway shutdown lifecycle publication.
- [#169299](https://github.com/openclaw/openclaw/pull/169299) — Doctor repair/reporting improvements.

## 6. Feature Requests & Roadmap Signals
| Request | Status | Likely Signal |
|---|---|---|
| [#50093](https://github.com/openclaw/openclaw/issues/50093) — WhatsApp backfill missed messages | P1, stale, needs product decision | Channel reliability and missed-message recovery. |
| [#78308](https://github.com/openclaw/openclaw/issues/78308) — MCP channel-mediated approval / consent envelope | P2 security | Consent/approval pipeline for MCP side effects. |
| [#16670](https://github.com/openclaw/openclaw/issues/16670) — onboarding wizard should include Memory/Embedding setup | P2, 2 👍 | Better first-run memory configuration. |
| [#76493](https://github.com/openclaw/openclaw/issues/76493) — allow SecretRef objects in `mcp.servers[].env` | P2, 4 👍 | Secret indirection and secure MCP configuration. |
| [#111143](https://github.com/openclaw/openclaw/issues/111143) — pin plugin tabs in Control UI sidebar | P3, 1 👍 | UI customization for plugin surfaces. |
| [#114414](https://github.com/openclaw/openclaw/issues/114414) — dated TODO sweep | P3 | Maintainability/tech-debt reduction. |
| [#169254](https://github.com/openclaw/openclaw/issues/169254) — promote JSON filter/delete fields to columns | P3 | PostgreSQL portability and data-model cleanup. |

**Closed feature signals:** [#9986](https://github.com/openclaw/openclaw/issues/9986) model fallback on context length exceeded, [#39811](https://github.com/openclaw/openclaw/issues/39811) model-name validation, [#127239](https://github.com/openclaw/openclaw/issues/127239) real context-window catalog values, and [#119692](https://github.com/openclaw/openclaw/issues/119692) MiniMax-M3 usage accounting.

**Prediction:** The next beta/stable line will likely continue the **update-recovery and Windows/macOS handoff** theme. After that, the strongest roadmap signals are **Control UI Solid migration**, **plugin runtime steering/GC**, **memory/embedding onboarding**, **MCP approval/SecretRef security**, and **channel message-recovery reliability**.

## 7. User Feedback Summary
- **Update/install frustration is the top pain point.** Users report `openclaw update` failing or stalling while direct npm installs succeed, plus Windows Scheduled Task, macOS launchd, beta-update, and handoff failures. Examples: [#156112](https://github.com/openclaw/openclaw/issues/156112), [#146860](https://github.com/openclaw/openclaw/issues/146860), [#167506](https://github.com/openclaw/openclaw/issues/167506), [#168750](https://github.com/openclaw/openclaw/issues/168750), [#170331](https://github.com/openclaw/openclaw/issues/170331), [#170332](https://github.com/openclaw/openclaw/issues/170332), [#138560](https://github.com/openclaw/openclaw/issues/138560), [#164214](https://github.com/openclaw/openclaw/issues/164214), [#158936](https://github.com/openclaw/openclaw/issues/158936).
- **Stability and resource exhaustion remain high-impact.** Memory leaks ([#159662](https://github.com/openclaw/openclaw/issues/159662), [#159356](https://github.com/openclaw/openclaw/issues/159356), [#99659](https://github.com/openclaw/openclaw/issues/99659)), zombie processes ([#97616](https://github.com/openclaw/openclaw/issues/97616)), disk growth ([#158390](https://github.com/openclaw/openclaw/issues/158390)), restart hangs ([#166894](https://github.com/openclaw/openclaw/issues/166894), [#167652](https://github.com/openclaw/openclaw/issues/167652), [#160060](https://github.com/openclaw/openclaw/issues/160060)), and startup regression ([#168955](https://github.com/openclaw/openclaw/issues/168955)).
- **Message delivery/reliability use cases matter.** WhatsApp missed messages ([#50093](https://github.com/openclaw/openclaw/issues/50093)), Telegram dropped callbacks ([#126950](https://github.com/openclaw/openclaw/issues/126950)), Discord double voice ([#125826](https://github.com/openclaw/openclaw/issues/125826)), silent failed runs ([#128916](https://github.com/openclaw/openclaw/issues/128916)), and plugin-reload chat failure ([#156353](https://github.com/openclaw/openclaw/issues/156353)).
- **Security/consent feedback is growing.** MCP tool approval ([#78308](https://github.com/openclaw/openclaw/issues/78308)), SecretRef support ([#76493](https://github.com/openclaw/openclaw/issues/76493)), `tools.deny` enforcement ([#132303](https://github.com/openclaw/openclaw/issues/132303)), and memory provenance trust ([#153426](https://github.com/openclaw/openclaw/issues/153426)).
- **Satisfaction/dissatisfaction:** The community is highly engaged with detailed repros, and maintainers are closing many issues/PRs. Dissatisfaction concentrates on unresolved P0 update/Windows reliability and long-lived items stuck in “needs product decision” or “needs live repro.”

## 8. Backlog Watch
Important issues needing maintainer/product/security attention:

| Issue | Age / Status | Why It Needs Attention |
|---|---|---|
| [#50093](https://github.com/openclaw/openclaw/issues/50093) | 2026-03-19, P1, stale, needs product decision | WhatsApp backfill is a persistent message-loss feature gap. |
| [#97616](https://github.com/openclaw/openclaw/issues/97616) | 2026-06-29, P1, needs live repro | Zombie process accumulation degrades long-running installs. |
| [#78308](https://github.com/openclaw/openclaw/issues/78308) | 2026-05-06, P2 security, needs security/product review | MCP tool-call consent is a security-boundary feature. |
| [#16670](https://github.com/openclaw/openclaw/issues/16670) | 2026-02-15, P2, 2 👍 | Memory/embedding onboarding is important for first-run success. |
| [#76493](https://github.com/openclaw/openclaw/issues/76493) | 2026-05-03, P2, 4 👍 | SecretRef in MCP env is a high-demand security improvement. |
| [#77249](https://github.com/openclaw/openclaw/issues/77249) | 2026-05-04, P1 | Slack reconnect supervisor can hang on zombie WSS. |
| [#125826](https://github.com/openclaw/openclaw/issues/125826) | 2026-08-18, P1 | Discord voice double-delivery affects user trust. |
| [#101929](https://github.com/openclaw/openclaw/issues/101929) | 2026-07-08, P1, needs maintainer review | Context estimator over-counting causes false truncation/recovery. |
| [#156112](https://github.com/openclaw/openclaw/issues/156112) | 2026-09-23, P0, manual-only | Updater fails while direct npm install succeeds. |
| [#146860](https://github.com/openclaw/openclaw/issues/146860) | 2026-09-13, P0 | Windows managed update handoff stalls. |
| [#154891](https://github.com/openclaw/openclaw/issues/154891) | 2026-09-21, P1 | Failed hot-reload can brick unrelated plugins until restart. |
| [#158390](https://github.com/openclaw/openclaw/issues/158390) | 2026-09-25, P0 | Plugin-capture temp dirs fill disk indefinitely. |
| [#132303](https://github.com/openclaw/openclaw/issues/132303) | 2026-08-29, P1 security | `tools.deny` silently ignored for claude-cli backend. |
| [#153426](https://github.com/openclaw/openclaw/issues/153426) | 2026-09-20, P0 security/session | Curated memory roots can be permanently excluded. |
| [#167652](https://github.com/openclaw/openclaw/issues/167652) | 2026-10-09, P0, needs live repro | Windows Gateway hangs after upgrade despite verified restart. |
| [#160060](https://github.com/openclaw/openclaw/issues/160060) | 2026-09-28, P0 | Windows Gateway silently dies after standby thaw. |
| [#166894](https://github.com/openclaw/openclaw/issues/166894) | 2026-10-08, P0 | Windows multi-agent restart hangs for 30+ min to 2h. |
| [#164294](https://github.com/openclaw/openclaw/issues/164294) | 2026-10-03, P2 | SQLite WAL checkpoint never completes. |
| [#138560](https://github.com/openclaw/openclaw/issues/138560) | 2026-09-04, P0 | Windows Control UI update aborts. |
| [#164799](https://github.com/openclaw/openclaw/issues/164799) | 2026-10-04, P1 | Agents admit one-by-one after version change; readyz 503 until restart. |

**PRs needing maintainer attention:**
- [#157106](https://github.com/openclaw/openclaw/pull/157106) — catalog list fails when Codex thread previews are huge; P1, needs proof.
- [#146986](https://github.com/openclaw/openclaw/pull/146986) — logs tail filter by minimum severity; open since 2026-09-13.
- [#169649](https://github.com/openclaw/openclaw/pull/169649) — reuse SQLite validation after clean shutdown; waiting on author.
- [#170152](https://github.com/openclaw/openclaw/pull/170152) — plugins steer a session’s active run; needs proof/security-boundary review.
- [#170536](https://github.com/openclaw/openclaw/pull/170536) — stop rechecking qualified target absence; waiting on author.

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — Personal AI Assistant / Agent OSS Ecosystem  
**Data date:** 2026-10-13  
**Projects:** OpenClaw, Hermes Agent, IronClaw, QwenPaw, ZeroClaw  
**Note:** Health scores are subjective 0–10 composites based on supplied digest data: activity, release cadence, responsiveness, open risk, and review/merge throughput.

---

## 1. Ecosystem Overview

The ecosystem is highly stratified: OpenClaw operates at a different scale from all peers, with hundreds of daily issue/PR updates and multiple releases, while Hermes Agent, QwenPaw, and ZeroClaw show active but still open-heavy development. Across projects, the dominant engineering themes are converging: update/install recovery, memory and context durability, multi-agent orchestration, observability/cost accounting, security/consent gating, and cross-platform reliability. Maturity varies sharply — OpenClaw is the broadest and most production-oriented, Hermes and QwenPaw are in active feature/maintenance phases, ZeroClaw is pre-release, and IronClaw is primarily benchmark/evaluation tracking. Review bandwidth, not contributor supply, appears to be the bottleneck for most projects except OpenClaw. Differentiation is still meaningful: OpenClaw is a general platform, Hermes emphasizes hybrid remote/local execution and observability, QwenPaw targets creators/self-hosters, ZeroClaw pushes multi-agent/A2A architecture, and IronClaw tracks model-quality failures.

---

## 2. Activity Comparison

| Project | Issues Updated / Status | PRs Updated / Status | Release Status | Health Score |
|---|---:|---:|---|---:|
| **OpenClaw** | **500** updated; 335 open/active, 165 closed | **500** updated; 314 open, 186 merged/closed | **2 releases:** v2026.10.5-beta.2, v2026.10.2 | **7.0/10** — high throughput and release cadence, but multiple P0 update/Windows/memory/disk risks remain |
| **Hermes Agent** | **50** updated; 20 open, 30 closed | **50** updated; 43 open, 7 merged/closed | **v0.21.7** patch; rolls up ~204 PRs since v0.21.6 | **6.5/10** — active, but open-PR review bottleneck and Langfuse duplicate cluster |
| **IronClaw** | **1** updated; 1 open, 0 closed | **0** updated; 0 open, 0 merged | **None** | **3.0/10** — low engineering throughput; benchmark/model-quality tracking only |
| **QwenPaw** | **17** updated; 15 open, 2 closed | **24** updated; 20 open, 4 merged/closed | **None** in window; Creator 2.0.1 merged via PR #8121 | **8.0/10** — strong same-day fix velocity, but Windows desktop/console issues remain |
| **ZeroClaw** | **33** updated; 29 open, ~4 closed | **50** updated; 44 open, 6 merged/closed | **None** in window; v0.8.6/v0.9.0 trackers active | **7.0/10** — high contributor momentum, but decision/review latency is significant |

---

## 3. OpenClaw’s Position

**Advantages vs peers**
- **Scale:** OpenClaw’s 500 issues + 500 PRs updated in 24h is roughly **10× Hermes/ZeroClaw** and **20–30× QwenPaw/IronClaw**. It also closed **165 issues** and merged/closed **186 PRs** in the same window.
- **Release cadence:** Two releases landed, including a stable-channel upgrade and a beta focused on update recovery and Windows/macOS install paths.
- **Breadth:** OpenClaw spans updater, gateway, managed service handoff, Control UI, plugins, memory, MCP security, channels, and session/transcript authority. Peers are narrower: Hermes on hybrid execution/observability, QwenPaw on creator/self-hosting, ZeroClaw on multi-agent/A2A, IronClaw on benchmarks.
- **Community size:** Hot issues attract 14–26 comments with detailed repros; peer projects typically see 4–17 comments on top issues.
- **Maintainer throughput:** OpenClaw is the only project in this set with merge/close volume exceeding open-PR accumulation.

**Technical approach differences**
- OpenClaw behaves like a **core platform/reference implementation**: stable/beta channels, Doctor repair flows, in-place update recovery, managed service handoff, Control UI Solid migration, plugin runtime steering, and MCP/secret safety.
- Peers are more specialized: Hermes is pushing remote-agent/local-tool execution; QwenPaw is building creator/console/self-hosting workflows; ZeroClaw is designing A2A/swarms/plugin/WASM; IronClaw is evaluating model-quality failures.

**Primary risk**
- OpenClaw’s advantage is offset by unresolved **P0 update/install, Windows service hang, memory leak, disk-growth, and process-zombie issues**. Its health is “high activity, mixed reliability” rather than clean stability.

---

## 4. Shared Technical Focus Areas

| Focus Area | Projects | Specific Needs |
|---|---|---|
| **Update/install/rollback reliability** | OpenClaw, QwenPaw, ZeroClaw, Hermes | OpenClaw: updater fails/stalls, Windows/macOS handoff, beta recovery. QwenPaw: Windows stale PID/file-lock failures. ZeroClaw: plugin install/update hardening. Hermes: patch rollup and desktop onboarding. |
| **Persistent memory, context, RAG** | OpenClaw, ZeroClaw, QwenPaw, Hermes | OpenClaw: memory leaks, MEMORY.md exclusion, memory-wiki, embedding onboarding. ZeroClaw: persistent-memory parity, RAG corpus, prompt-cache-friendly personality snapshots. QwenPaw: context compaction. Hermes: remote memory/session/skills. |
| **Multi-agent/A2A/swarm orchestration** | ZeroClaw, OpenClaw, Hermes, QwenPaw | ZeroClaw: A2A protocol, swarm TUI, durable peer-agent turns, dark-factory workflow. OpenClaw: multi-agent restart hangs, session/transcript authority, plugin steering. Hermes: remote/local hybrid execution. QwenPaw: worker containers and coding-CLI management. |
| **Observability, cost, and privacy** | Hermes, ZeroClaw, OpenClaw, QwenPaw, IronClaw | Hermes: Langfuse cost/session/masking failures. ZeroClaw: UTC/local cost totals, model-routing config. OpenClaw: usage accounting, context over-count, memory-wiki status. QwenPaw: Langfuse tool observations. IronClaw: failure taxonomy and benchmark classification. |
| **Security, consent, and secrets** | OpenClaw, ZeroClaw, Hermes, QwenPaw | OpenClaw: MCP approval, SecretRef, tools.deny enforcement, malformed UTF-8 secrets. ZeroClaw: pairing tokens, gateway body size, scoped logs, TLS. Hermes: trace masking/user_id. QwenPaw: BYO Tavily/Firecrawl keys. |
| **Cross-platform desktop/Windows reliability** | OpenClaw, QwenPaw, Hermes, ZeroClaw | OpenClaw: Windows service/update hangs. QwenPaw: Creator crashes, log rotation, file locks. Hermes: desktop app/onboarding. ZeroClaw: ZeroCode TUI key handling. |
| **Channel/message delivery reliability** | OpenClaw, Hermes, ZeroClaw, QwenPaw | OpenClaw: WhatsApp backfill, Telegram callbacks, Discord voice, Slack WSS. Hermes: Telegram DM topics, DingTalk media. ZeroClaw: channel separation, Discord plugin. QwenPaw: multi-device message sync. |
| **Tool-call/streaming lifecycle correctness** | OpenClaw, QwenPaw, ZeroClaw | OpenClaw: silent turn endings, tool-call text leaks, CLI bridge scope. QwenPaw: approval race, offload 404, silent empty completion. ZeroClaw: dropped ask_user/poll, tool-call envelopes in prose. |
| **Self-hosting, BYO keys, reverse proxy, modularity** | QwenPaw, ZeroClaw, Hermes, OpenClaw | QwenPaw: Tavily/Firecrawl, basename, NAS/Docker. ZeroClaw: separate channels from core, configurable body size. Hermes: self-hosted TTS compatibility. OpenClaw: private-Node update paths. |

---

## 5. Differentiation Analysis

| Project | Feature Focus | Target Users | Architectural Signals |
|---|---|---|---|
| **OpenClaw** | Broad personal assistant platform: updater, gateway, Control UI, plugins, channels, memory, MCP security | Power users, self-hosters, plugin developers, teams needing reliability | Core gateway/service, managed handoff, Doctor, Solid UI, plugin runtime, session/transcript authority |
| **Hermes Agent** | Hybrid remote agent + local tool execution, observability, desktop/connectors | Privacy/latency-sensitive users, multi-profile users, Langfuse observers | Desktop + gateway + connectors + CLI ownership refactor; remote memory/sessions with local tools |
| **IronClaw** | Benchmark failure taxonomy and model-quality tracking | Researchers, benchmark maintainers, model evaluators | Benchmark harness/daily taxonomy; low runtime feature activity in this window |
| **QwenPaw** | Creator/media production, Windows desktop, console self-hosting, multi-device UX | Creators, NAS/Docker self-hosters, multi-device users | Creator plugin, PawApp console, worker containers, reverse-proxy support, provider config |
| **ZeroClaw** | Multi-agent orchestration, A2A, plugin/WASM channels, ZeroCode TUI | Platform builders, operators, multi-agent workflow designers | Gateway/runtime, plugin/WASM stack, A2A protocol crate, TUI, persistent-memory parity |

**Key takeaway:** OpenClaw competes on breadth and production readiness; Hermes on hybrid execution and observability; QwenPaw on creator/self-hosting ergonomics; ZeroClaw on multi-agent protocol architecture; IronClaw on evaluation rather than runtime features.

---

## 6. Community Momentum & Maturity

**Activity tiers**
- **Tier 1 — Extreme scale:** OpenClaw. 500/500 issue/PR updates, 165 issues closed, 186 PRs merged/closed, two releases.
- **Tier 2 — High activity, open-heavy:** ZeroClaw, Hermes Agent, QwenPaw. These projects show strong contributor energy but review/merge bottlenecks.
- **Tier 3 — Low visible activity:** IronClaw. One benchmark-taxonomy issue, no PRs, no releases.

**Rapid iteration**
- **ZeroClaw:** Same-day bug→fix PRs; pre-release v0.8.6/v0.9.0 trackers; active RFC pipeline.
- **QwenPaw:** Multiple first-time contributors opened fixes on the same day issues were filed; four PRs merged/closed.
- **Hermes:** Patch v0.21.7 rolled up ~204 PRs, but 43 open PRs vs 7 merged/closed signals review pressure.
- **OpenClaw:** Rapid closure of crash/session/update issues, but P0 reliability backlog keeps risk elevated.

**Stabilizing vs pre-release**
- **Most mature/broadest:** OpenClaw.
- **Near-stable with patch cadence:** Hermes Agent.
- **Production self-hosting with Windows trust gaps:** QwenPaw.
- **Pre-release architecture push:** ZeroClaw.
- **Niche evaluation tracking:** IronClaw.

**Review bottleneck**
- Hermes: **43 open vs 7 merged/closed**.
- ZeroClaw: **44 open vs 6 merged/closed**.
- QwenPaw: **20 open vs 4 merged/closed**.
- OpenClaw is the exception: **186 merged/closed** in 24h.

---

## 7. Trend Signals

1. **Updaters/installers are now first-class agent infrastructure.**  
   OpenClaw, QwenPaw, and ZeroClaw all show pain around updates, install handoff, stale PIDs, file locks, and recovery. Developers should treat update/rollback as a core subsystem, not packaging glue.

2. **Memory, context, and RAG are becoming the durable differentiator.**  
   OpenClaw’s memory leaks/exclusions, ZeroClaw’s memory parity/RAG corpus, QwenPaw’s context compaction, and Hermes’ remote memory/session needs point to memory durability, provenance, and prompt-cache efficiency as core requirements.

3. **Multi-agent orchestration is moving from ad hoc config to protocol.**  
   ZeroClaw’s A2A/RFC/swarm work, OpenClaw’s multi-agent restart/session authority issues, and Hermes’ remote/local hybrid request show demand for first-class agent-to-agent messaging, durable turns, and session ownership.

4. **Observability and cost accounting must be correct and privacy-aware.**  
   Hermes’ Langfuse duplicate cluster, ZeroClaw’s cost-tab issues, OpenClaw’s usage/context accounting bugs, and QwenPaw’s Langfuse tool-observation gap indicate that traces, costs, and masking are production requirements.

5. **Security/consent is expanding beyond API keys.**  
   MCP approvals, SecretRef, tools.deny enforcement, pairing tokens, scoped logs, trace masking, and BYO provider keys are recurring demands. Consent gating for tool side effects is becoming a platform feature.

6. **Cross-platform Windows/desktop reliability is a competitive risk.**  
   OpenClaw and QwenPaw both have Windows-specific P0/high issues: service hangs, file locks, stale PID handling, log rotation. Agent developers must account for Windows process, file, and service semantics.

7. **Channel/message reliability remains core UX.**  
   WhatsApp backfill, Telegram DM routing, Discord voice duplication, Slack WSS hangs, DingTalk media, and multi-device sync all appear across projects. Message-loss prevention and reconnection recovery are not edge cases.

8. **Silent failures are especially damaging.**  
   OpenClaw silent turn endings, QwenPaw silent empty completion and binary corruption, ZeroClaw dropped elicitations and false-success cron — users lose trust when agents fail without clear signals.

9. **Modularity and self-hosting demand is rising.**  
   Users want BYO keys, reverse-proxy subpaths, separate channels from core, plugin hot reload/unload, reduced-effects modes, and NAS/Docker support. “Don’t ship what I don’t use” is a real security and operational preference.

10. **For AI agent developers, the build checklist is clear:**  
    Recoverable updates, supervised processes, durable memory with provenance, protocol-level multi-agent support, accurate cost/observability, consent-gated tools, cross-platform service handling, and explicit failure surfacing.

**Bottom line:** OpenClaw leads on scale, breadth, and release throughput, but its unresolved P0 reliability items are the ecosystem’s most visible risk. Hermes, QwenPaw, and ZeroClaw are iterating quickly in differentiated niches, while IronClaw remains a low-activity evaluation signal. The next phase of competition will likely be decided by update/install reliability, memory/RAG durability, multi-agent protocols, observability correctness, and Windows/desktop trust.

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-10-13

## 1. Today's Overview

Hermes Agent shows **high activity** on 2026-10-13: 50 issues were updated, split across 20 open/active and 30 closed, while 50 PRs were updated with 43 open and only 7 merged/closed. The issue side is heavily shaped by a large Langfuse observability bug/duplicate cluster, which accounts for much of the closed volume. PR flow is open-heavy, suggesting active development but also a possible review/merge bottleneck. The project also shipped one new patch release, **v0.21.7**, rolling up roughly 204 PRs since v0.21.6. Overall project health is active, but maintainer attention is needed on long-running decisions, duplicate triage, and several P2 platform bugs.

## 2. Releases

### v0.21.7 — Patch Release
- **Release date:** October 11, 2026
- **Type:** Patch release; rolls up ~204 PRs merged since v0.21.6 into a stable release.
- **Targets:** Desktop app (macOS Apple silicon, Windows), Microsoft Store/MSIX, installer, Docker, and Hermes Cloud.
- **Breaking changes / migration notes:** None explicitly stated in the provided release excerpt. The full curated release notes were truncated, so maintainers should verify the complete note before upgrading.
- **Assessment:** This is a stabilization rollup rather than a feature release. No security advisories or migration requirements are visible in the supplied data.

## 3. Project Progress

### Merged/Closed PRs Today
The dataset reports **7 merged/closed PRs**, though only some appear in the supplied top 20:
- [#137426](https://github.com/NousResearch/hermes-agent/pull/137426) — `feat(connectors): manage_connections display, a browsable connector card`. Adds a connector directory card and lets the model show connection status in chat.
- [#137525](https://github.com/NousResearch/hermes-agent/pull/137525) — `fix(solstice): offer Gemini 3.1 Pro, 3.8 Flash and 3.5 Flash Lite`. Updates the Solstice Gemini OAuth provider’s hardcoded model list.

### Open PRs Advancing
The open PR queue is broad and includes desktop, gateway, CLI, plugins, platform integrations, and security work:
- [#137522](https://github.com/NousResearch/hermes-agent/pull/137522) — desktop first-run onboarding chat and `set_theme`.
- [#137420](https://github.com/NousResearch/hermes-agent/pull/137420) — deletes the old agentic setup flow.
- [#125028](https://github.com/NousResearch/hermes-agent/pull/125028) — P1 CLI Ownership Refactor Phase 2: gateway control and topology.
- [#31871](https://github.com/NousResearch/hermes-agent/pull/31871) — DingTalk inbound media caching.
- [#96917](https://github.com/NousResearch/hermes-agent/pull/96917) — DingTalk outbound image/file upload.
- [#127820](https://github.com/NousResearch/hermes-agent/pull/127820) — security dependency bump for `httpx2`.
- [#137564](https://github.com/NousResearch/hermes-agent/pull/137564) — browser real-profile browser selection.
- [#137560](https://github.com/NousResearch/hermes-agent/pull/137560) — composer reply suggestions after each turn.
- [#137566](https://github.com/NousResearch/hermes-agent/pull/137566), [#137556](https://github.com/NousResearch/hermes-agent/pull/137556), [#137559](https://github.com/NousResearch/hermes-agent/pull/137559), [#137562](https://github.com/NousResearch/hermes-agent/pull/137562), [#137563](https://github.com/NousResearch/hermes-agent/pull/137563) — gateway/desktop/session/profile fixes.

### Closed Issues Indicating Triage/Fix Progress
Many closed issues today belong to the Langfuse observability plugin:
- [#72661](https://github.com/NousResearch/hermes-agent/issues/72661) — Langfuse `total_cost` zero despite per-type breakdown.
- [#72484](https://github.com/NousResearch/hermes-agent/issues/72484) — custom endpoint credentials rejected by key prefix.
- [#71556](https://github.com/NousResearch/hermes-agent/issues/71556) — random `session_id` instead of stable gateway session key.
- [#81730](https://github.com/NousResearch/hermes-agent/issues/81730) — session tags not reaching child observations.
- [#58322](https://github.com/NousResearch/hermes-agent/issues/58322) — `service.name=unknown_service`.
- [#59026](https://github.com/NousResearch/hermes-agent/issues/59026) — Langfuse SDK removed after update.
- [#32005](https://github.com/NousResearch/hermes-agent/issues/32005) — same trace ID reused for all turns.
- [#95057](https://github.com/NousResearch/hermes-agent/issues/95057) — OTel `Failed to detach context` errors.

## 4. Community Hot Topics

### Most Active Issues
1. [#18715](https://github.com/NousResearch/hermes-agent/issues/18715) — **Support remote Hermes agent with local tool execution**  
   - Open, P2, `needs-decision`, 26 comments, 40 👍.  
   - Underlying need: users want a hybrid architecture where memory/sessions/skills/model config stay on a remote Hermes instance, but tools execute locally for privacy, latency, or local resource access. This is the strongest community signal in the dataset.

2. [#136567](https://github.com/NousResearch/hermes-agent/issues/136567) — **Telegram DM topics: opening message delivered thread-less → lobby recovery misroutes it**  
   - Open, P2, 17 comments, 3 👍.  
   - Underlying need: reliable Telegram forum/DM topic routing. This affects message delivery and session state for Telegram power users.

3. Langfuse observability cluster — e.g. [#52389](https://github.com/NousResearch/hermes-agent/issues/52389), [#51399](https://github.com/NousResearch/hermes-agent/issues/51399), [#63787](https://github.com/NousResearch/hermes-agent/issues/63787), [#67544](https://github.com/NousResearch/hermes-agent/issues/67544)  
   - Many closed duplicates and several open/closed bugs. High comment volume despite many duplicate closures.  
   - Underlying needs: reliable observability, correct cost accounting, stable session chaining, profile-level attribution, and privacy/masking of exported inputs/outputs.

### Notable PRs
PR comment counts are reported as `undefined` in the supplied data, so they cannot be ranked by comments. By scope and priority:
- [#137522](https://github.com/NousResearch/hermes-agent/pull/137522) — desktop onboarding/theme, likely high user-facing impact.
- [#125028](https://github.com/NousResearch/hermes-agent/pull/125028) — P1 CLI ownership refactor, important architectural work.
- [#31871](https://github.com/NousResearch/hermes-agent/pull/31871) and [#96917](https://github.com/NousResearch/hermes-agent/pull/96917) — DingTalk media reliability.
- [#137560](https://github.com/NousResearch/hermes-agent/pull/137560) — composer reply suggestions, a UX enhancement.

## 5. Bugs & Stability

Ranked by severity and user impact:

1. **P2 — [#136567](https://github.com/NousResearch/hermes-agent/issues/136567) Telegram DM topic misrouting**  
   Opening message in a new Telegram topic is delivered without `message_thread_id`, causing lobby recovery to misroute it to the previous topic. Affects message delivery and session state. No direct fix PR is visible in the supplied top PR list.

2. **P2 — [#73470](https://github.com/NousResearch/hermes-agent/issues/73470) OpenAI-compatible TTS `response_format=opus` failure**  
   Self-hosted OpenAI-compatible TTS backends such as Speaches/Kokoro reject `opus`, producing no audio. Reported as still failing after #57184. No fix PR visible in the supplied data.

3. **P2 — [#137544](https://github.com/NousResearch/hermes-agent/issues/137544) Billing plans show “Downgrade” on higher tiers while on Free**  
   Created today. On the Free plan, Plus and Super show “Downgrade” instead of “Choose/Upgrade.” Potential billing/UX risk if users click a destructive action. No fix PR visible yet.

4. **P3 / privacy — [#67544](https://github.com/NousResearch/hermes-agent/issues/67544) Langfuse traces export unredacted inputs/outputs**  
   No masking hook and no `user_id`. This is P3 by label but high impact for privacy-sensitive deployments. Still open.

5. **Closed Langfuse stability cluster**  
   Multiple Langfuse bugs were closed today, including silent placeholder-key failures, session chaining, cost totals, service naming, SDK removal after update, and OTel context errors. This reduces duplicate backlog but suggests a broader plugin reliability and documentation problem.

## 6. Feature Requests & Roadmap Signals

- [#18715](https://github.com/NousResearch/hermes-agent/issues/18715) — Remote Hermes agent with local tool execution. **Strong roadmap candidate** due to 40 👍 and 26 comments, but marked `needs-decision`; likely requires architectural design and may not ship immediately.
- [#67544](https://github.com/NousResearch/hermes-agent/issues/67544) — Langfuse masking/user_id. Likely to be bundled with future observability hardening.
- [#26455](https://github.com/NousResearch/hermes-agent/issues/26455) — Expose `profile_name` as trace `userId`. Closed today; may indicate per-profile observability is being addressed.
- [#137522](https://github.com/NousResearch/hermes-agent/pull/137522) — Desktop first-run onboarding and `set_theme`. Likely for the next desktop-facing release.
- [#137420](https://github.com/NousResearch/hermes-agent/pull/137420) — Delete old agentic setup flow. Breaking-ish cleanup; may land after the onboarding switch-over.
- [#137560](https://github.com/NousResearch/hermes-agent/pull/137560) — Composer reply suggestions. Could appear in an upcoming TUI/Desktop release.
- [#137564](https://github.com/NousResearch/hermes-agent/pull/137564) — Browser real-profile browser selection. Useful for Chromium-family users.
- [#96917](https://github.com/NousResearch/hermes-agent/pull/96917) — DingTalk outbound media. Likely targeted at platform completeness.

**Prediction:** The next patch/minor release will likely include desktop onboarding/theme, connector management, browser profile selection, gateway/session fixes, and possibly DingTalk media. The remote/local hybrid tool execution request is more likely a v0.22+ roadmap item because it requires a maintainer decision and architectural work.

## 7. User Feedback Summary

- **Hybrid deployment demand:** Users want remote Hermes for memory/sessions/skills, but local tool execution for privacy, latency, and local resource access. This is the clearest high-value unmet need.
- **Telegram reliability:** Telegram DM/forum-topic users are hitting message routing failures, which breaks session continuity.
- **Observability frustration:** Langfuse users report silent failures, missing traces, zero costs, incorrect session chaining, and unredacted data export. The volume of duplicate issues suggests poor error surfacing and unclear setup documentation.
- **Self-hosted TTS incompatibility:** Users running OpenAI-compatible TTS backends are blocked when Hermes requests `opus`.
- **Billing UX confusion:** The “Downgrade” button on higher tiers while on Free is not cosmetic; it risks accidental downgrade clicks.
- **Multi-profile usage:** Multiple requests and PRs target per-profile search, STT behavior, trace attribution, and observability, indicating growing multi-profile adoption.
- **Overall satisfaction signal:** High engagement — 40 👍 on #18715 — shows strong user investment. Dissatisfaction is concentrated in silent observability failures and platform-specific message/media handling.

## 8. Backlog Watch

Important items needing maintainer attention:

- [#18715](https://github.com/NousResearch/hermes-agent/issues/18715) — Open since 2026-05-02, 26 comments, 40 👍, `needs-decision`. High community demand but unresolved architecture question.
- [#73470](https://github.com/NousResearch/hermes-agent/issues/73470) — Open since 2026-07-28, P2 TTS `opus` regression. No visible fix PR.
- [#67544](https://github.com/NousResearch/hermes-agent/issues/67544) — Open since 2026-07-19, privacy/masking gap in Langfuse exports.
- [#125028](https://github.com/NousResearch/hermes-agent/pull/125028) — P1 CLI Ownership Refactor Phase 2, open since 2026-09-27. High-priority architectural PR.
- [#31871](https://github.com/NousResearch/hermes-agent/pull/31871) — DingTalk inbound media, open since 2026-05-25. Long-running platform fix.
- [#96917](https://github.com/NousResearch/hermes-agent/pull/96917) — DingTalk outbound media, open since 2026-08-28. Complements the inbound fix.
- [#128778](https://github.com/NousResearch/hermes-agent/pull/128778) — Config read-only path fix, open since 2026-09-30.
- [#131067](https://github.com/NousResearch/hermes-agent/pull/131067) — Desktop sandbox/GPU recovery markers, open since 2026-10-02, platform/Windows and security-boundary risks.
- [#124754](https://github.com/NousResearch/hermes-agent/pull/124754) — Gateway back-online announcement after supervised restart, open since 2026-09-27.

**Bottom line:** Hermes Agent is highly active and shipping patches, but the open PR queue and several P2 issues need focused review. The Langfuse duplicate cluster should be consolidated into a single canonical issue or documentation fix to reduce community noise.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-10-13

**Data snapshot:** Issues updated in last 24h: 1 open/active, 0 closed. PRs updated: 0. New releases: 0.

## 1. Today's Overview
IronClaw activity was minimal over the last 24h, with only one open issue updated and no PR or release activity. The sole item is #8135, “Daily ironclaw failure taxonomy — 2026-10-12,” which reports benchmark failures rather than code changes. It covers an officeqa run with 52 non-pass tasks, all classified as genuine model-quality errors, with no exceptions found. Community engagement is flat: 0 comments and 0 reactions. Overall health signal: low engineering throughput and no release/migration impact; the main visible activity is ongoing benchmark and model-quality tracking.

## 2. Releases
No new releases in the reporting window.

## 3. Project Progress
- Merged/closed PRs today: **0**
- Features advanced or fixed: **none reported via PRs**
- Open issue activity: [#8135](https://github.com/nearai/ironclaw/issues/8135) remains open. It appears to be a taxonomy/benchmark tracking item, not a code-progress item.

## 4. Community Hot Topics
Only one active item in the window:

- [#8135 [OPEN] Daily ironclaw failure taxonomy — 2026-10-12](https://github.com/nearai/ironclaw/issues/8135) — 0 comments, 0 👍

Underlying need: systematic classification of benchmark failures to separate model-quality errors from agent, tooling, or infrastructure defects. The issue links to an [officeqa benchmark run](https://nearai.github.io/benchmarks/#/runs/ironclaw/officeqa/c0bf84d6-1954-42ef-8378-20f32b394f1e) and states that 52 non-pass tasks were all genuine model-quality errors. There is no community discussion yet.

## 5. Bugs & Stability
- [#8135](https://github.com/nearai/ironclaw/issues/8135) — **Severity: Medium for benchmark/model quality; not a runtime crash.**
  - Reports 52 non-pass officeqa tasks, all attributed to genuine model-quality errors, with no exceptions found.
  - No crashes, regressions, or runtime stability issues are reported in the supplied data.
  - No fix PRs exist in this snapshot.
  - The provided summary is truncated after “DeepSeek-V4-Flas…”, so the full model/task breakdown is unavailable.

## 6. Feature Requests & Roadmap Signals
- No explicit feature requests were opened or updated in the last 24h.
- Roadmap signal: recurring daily failure taxonomy reports suggest continued demand for benchmark observability, automated failure classification, and model-quality tracking.
- Predicting next-version scope: no concrete feature can be inferred from this single issue.

## 7. User Feedback Summary
- Source: [#8135](https://github.com/nearai/ironclaw/issues/8135), authored by pranavraja99.
- Pain point: officeqa benchmark failures — 52 non-pass tasks classified as model-quality errors.
- Use case: automated daily benchmark failure analysis for IronClaw.
- Satisfaction/dissatisfaction: not measurable from this data; no comments or reactions were recorded.

## 8. Backlog Watch
- [#8135](https://github.com/nearai/ironclaw/issues/8135) is recent (created/updated 2026-10-12) and has no comments. It is not long-unanswered, but may need maintainer triage if the daily taxonomy requires follow-up.
- No other important open issues or PRs appear in the supplied 24h window requiring backlog attention.

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw Project Digest — 2026-10-13

## 1. Today's Overview

QwenPaw is in a high-activity maintenance and feature-development phase: 17 issues were touched in the last 24 hours (15 open/active, 2 closed) and 24 PRs were updated (20 open, 4 merged/closed), with **no new releases**. The day's throughput is balanced — two long-standing bugs were closed (#8105 approval buttons, #8073 conversation page), and several high-signal fix PRs were opened or advanced against fresh bug reports, notably Windows/Desktop creator-plugin crashes and console UX regressions. Feature work is heavily community-driven: three separate first-time contributors opened PRs implementing user-requested features (Tavily key config, Firecrawl provider, nested skill directories) on the same day they were requested. The dominant risk theme is **Windows desktop stability** (stale PID probes, file-lock deletion failures, non-rotating logs) and **Console frontend correctness** (scroll anchoring, multi-device message sync, GPU cost). Overall project health looks strong on responsiveness, though the open-PR queue (20) and a small set of stale, high-impact issues warrant maintainer attention.

## 2. Releases

No new releases in the last 24 hours. (The most recent release event was PR #8121, which was closed/merged — QwenPaw Creator 1.3.0 → 2.0.1 with controlled media production, discussed in Section 3.)

## 3. Project Progress

Four PRs were merged/closed today:

- **[PR #8188] fix(approval): preserve decisions and handle timeout races** — Closes **#8105**, the long-standing bug where tool approval always resolved to "reject." Root cause was a race: decisions completed before the tool entered its wait state were lost. This is the highest-impact fix of the day. https://github.com/agentscope-ai/QwenPaw/pull/8188
- **[PR #8121] feat(creator): release 2.0.1 with controlled media production** (size/XXXL) — Ships the QwenPaw Creator plugin bump 1.3.0 → 2.0.1, carrying only post-upstream-release plugin work. https://github.com/agentscope-ai/QwenPaw/pull/8121
- **[PR #8193] fix(context): reclaim active results earlier and verify overflow recovery** — Improves context compaction so long active turns stop accumulating recoverable tool output beyond the trigger threshold. https://github.com/agentscope-ai/QwenPaw/pull/8193
- **[Issue #8073] V2.2.2.beta4 unable to access conversation page on LAN** — Closed; a regression affecting LAN clients opening the chat page. https://github.com/agentscope-ai/QwenPaw/issues/8073

Features advanced but still open: reverse-proxy basename support (#8197), per-chat approval level persistence (#8182), coding-CLI management endpoints (#8156), plugin hot reload/rollback (#7565), and the "reduced effects" console tier (#8137).

## 4. Community Hot Topics

Ranked by comment count in the last 24h:

1. **#8174 — `web_search` Tavily provider has no API-key config surface** (4 comments) — Keyless-only anonymous mode makes paid Tavily quota unusable and gives no clean fallback on 401/429/432. A first-time contributor already opened the fix, **PR #8204**. https://github.com/agentscope-ai/QwenPaw/issues/8174
2. **#7016 — 工具调用 404 (`Tool call not found` on `/offload`)** (4 comments) — Streaming sessions repeatedly hit a 404 on the offload endpoint. Open since 2026-08-14, still without a linked fix PR. https://github.com/agentscope-ai/QwenPaw/issues/7016
3. **#8172 — Console intermittent silent empty completion** (3 comments) — A run finishes in ~1s with empty output and the model is never called, only on the embedded PawApp console channel. High severity, no fix PR yet. https://github.com/agentscope-ai/QwenPaw/issues/8172

Underlying needs: users are pushing toward **production-grade self-hosting** (custom API keys, reverse-proxy subpaths, multi-provider web_search, server-side persistence) and **desktop reliability**, while the repeated 404/silent-completion reports point to fragility in the streaming/run lifecycle for embedded and remote clients.

## 5. Bugs & Stability

Ranked by severity (all open unless noted):

| # | Issue | Severity | Fix PR? |
|---|-------|----------|---------|
| 1 | **#8172** Console silent empty completion — model never invoked, no reply rendered (v2.2.0 Docker/NAS) | Critical (silent data/UX failure) | ❌ None |
| 2 | **#8179** Installing a skill from GitHub silently corrupts **binary files** (all non-UTF-8 bytes → U+FFFD; images/fonts broken) | High (silent data corruption) | ✅ PR #8198 |
| 3 | **#8206** Project deletion always fails on Windows once `creator.log` is open (WinError 5 on directory rename) | High | ❌ None |
| 4 | **#8194** `qwenpaw-creator` startup hook aborts on stale PID — `os.kill(pid, 0)` raises `SystemError`, not caught by `except OSError` | High (app won't start) | ✅ PR #8196 |
| 5 | **#8184** Doom-loop gate kills legitimate task-authorized polling; cron then records **false success** after termination | High (wrong state + silent failure) | ✅ PR #8192 |
| 6 | **#8185** `desktop.log` never rotates on Windows (sidecar tee handle defeats rotating handler) | Medium (disk exhaustion over time) | ❌ None |
| 7 | **#8186** Message-navigation jump overrides manual wheel scrolling, snapping back for ~1.6s | Medium (UX) | ✅ PR #8201 |
| 8 | **#7016** Tool-call 404 loop on `/offload` during streaming | Medium (open since Aug) | ❌ None |
| 9 | **#8202** A message sent on one device is invisible on the other until the turn finishes | Medium (multi-device UX) | ✅ PR #8203 |
| 10 | **#8105** Tool approval buttons both execute "reject" | High — **CLOSED** ✅ | ✅ PR #8188 (merged) |

Notable pattern: five of today's ten issues have a same-or-next-day fix PR, indicating strong contributor-to-maintainer velocity. The two Windows creator-plugin failures (#8206, #8194) share a root theme — **Windows file/process semantics not accounted for** — and may merit a consolidated hardening pass.

## 6. Feature Requests & Roadmap Signals

Requested features (with implementation status):

- **Configurable `web_search` providers** — Tavily API-key surface (#8174, PR #8204) and Firecrawl as a third provider (#8199, PR #8200). Strong signal that **multi-provider, BYO-key web search** lands soon.
- **Reverse-proxy subpath support** — configurable console basename (#8177, PR #8197); important for self-hosted multi-service domains.
- **Console "reduced effects" tier** (#8135, PR #8137) — GPU load reduction on integrated graphics; positions Console for lower-end/NAS hardware.
- **Server-side per-chat approval mode** (#8182) — moves approval level off browser `localStorage` for cross-device consistency.
- **Model pool deletion** (#8195) — only enable/disable exists; deprecated models cannot be purged.
- **Coding-CLI management endpoints for worker containers** (#8156) — manage `qwen-code`/`opencode` model and config per worker.
- **Decision model type + JEV provider** (#8205) — backward-compatible `model_type` (`chat`/`decision`) hinting at upcoming routing/structured-decision features.
- **Nested/categorised skill directories** (#8178) — `<root>/<category>/<skill>/SKILL.md` layout.
- **Plugin hot reload / clean unload** (#7565, size/XXXL) — longer-horizon infrastructure work.
- **ESP32 hardware integration à la Meta Muse** (#8189) — exploratory, unlikely in near-term release.

**Prediction:** the next minor/beta wave likely bundles Tavily/Firecrawl provider config, console basename configurability, and the Windows creator-plugin fixes; decision-model plumbing (#8205) suggests a subsequent release targeting agent routing.

## 7. User Feedback Summary

- **Pain points:** paid Tavily users cannot spend their own quota (#8174); binary assets are silently destroyed on GitHub skill install (#8179); Windows users cannot delete projects or even start Creator when a stale PID exists (#8206, #8194); tool approvals were functionally unusable until today's fix (#8105); LAN clients couldn't open the chat page (#8073).
- **Use cases observed:** NAS/Docker self-hosting behind reverse proxies, multi-device chat (web + mobile, #8202), multi-agent worker containers running third-party coding CLIs (#8156), and external hardware integration (#8189).
- **Satisfaction signals:** rapid turnaround — four fixes merged/closed and multiple same-day response PRs from first-time contributors. Users are engaging constructively (detailed repro steps, version/env metadata).
- **Dissatisfaction signals:** recurring **Windows desktop/creator reliability** complaints, silent failures (empty completion, binary corruption, false-success cron), and desktop.log growth — a consistent "trust in the desktop build" theme.

## 8. Backlog Watch

Items needing maintainer attention:

- **#7016 — 工具调用 404** — Open since **2026-08-14** (~2 months), 4 comments, no linked fix PR. Streaming offload 404 loop is a core-path defect. https://github.com/agentscope-ai/QwenPaw/issues/7016
- **PR #6384 — `AskUserQuestion` interactive tool** — Open since **2026-07-23** (~3 months) and "Under Review"; large user-facing capability stalled in review. https://github.com/agentscope-ai/QwenPaw/pull/6384
- **PR #7565 — Plugin clean unload / rollback-safe hot reload** — size/XXXL, open since **2026-09-04**, touching plugin lifecycle; high architectural value, slow to land. https://github.com/agentscope-ai/QwenPaw/pull/7565
- **PR #7964 — Langfuse tool observations never record output** — Under Review since **2026-09-24**; observability correctness for tool spans. https://github.com/agentscope-ai/QwenPaw/pull/7964
- **Issue #8195 — Model pool has no delete/remove action** — Small, clear UX gap; low effort, no fix PR yet. https://github.com/agentscope-ai/QwenPaw/issues/8195

Open-PR volume (20) is trending upward relative to merges (4), which — combined with multi-month "Under Review" items — suggests **review bandwidth is the primary bottleneck** rather than contribution supply.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-10-13

## 1. Today's Overview

ZeroClaw remains in a high-activity, pre-release phase: **33 issues and 50 PRs were touched in the last 24h**, yet **0 new releases** shipped. The backlog skews heavily open — **29 of 33 active issues** and **44 of 50 PRs are still open** — indicating substantial incoming work relative to merge throughput. Activity is concentrated in four recurring themes: **persistent memory**, the **A2A protocol**, the **plugin/WASM channel stack**, and the **ZeroCode TUI**. Three issues closed on 2026-10-13 (a browser/search tool-semantics bug, plugin channel-instance seeding, and dead-worker settlement recovery), but the ratio of open-to-closed suggests **reviewer/merge bandwidth, not contributor supply, is the current bottleneck**. Notably, several of today's newest bug reports already have companion fix PRs opened the same day, which is a healthy sign for responsiveness.

## 2. Releases

No new releases in the reporting window. (**Omit** per instructions — no changelog, breaking changes, or migration notes to report.)

Two active release-gated trackers remain the roadmap anchors: [#7432 Runtime and gateway delivery — v0.8.6 and v0.9.0](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) and the plugin-related work tagged `release:v0.8.6` ([#10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999), [PR #11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262), [PR #11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261)).

## 3. Project Progress

**Closed/merged items (4 issues closed in the window; 3 visible):**

| Item | Type | Outcome |
|---|---|---|
| [#11108](https://github.com/zeroclaw-labs/zeroclaw/issues/11108) | Bug (tools) | **Closed** — `map_tool_name_alias()` no longer rewrites `browser_open`/`browser`/`web_search` into `shell`, preserving built-in tool semantics |
| [#10996](https://github.com/zeroclaw-labs/zeroclaw/issues/10996) | Feature (plugins) | **Closed** — channel-instance configuration/grants seeding during plugin install |
| [#10331](https://github.com/zeroclaw-labs/zeroclaw/issues/10331) | Bug (runtime/daemon) | **Closed** — recovery of terminal settlement intents abandoned by a dead worker |
| (4th closed issue) | — | Not in top-30 sample |

**PR activity (6 merged/closed; none visible in the top-20 sample, which is entirely OPEN):** The visible PR set is dominated by fixes and follow-ups that advance existing epics rather than new subsystems. Standouts opened/first-seen today (2026-10-13):

- [PR #11698](https://github.com/zeroclaw-labs/zeroclaw/pull/11698) — `fix(runtime)`: snapshot personality files per session so edits no longer bust the prompt cache (addresses [#11696](https://github.com/zeroclaw-labs/zeroclaw/issues/11696)).
- [PR #11693](https://github.com/zeroclaw-labs/zeroclaw/pull/11693) — `fix(tools)`: make `model_routing_config upsert_agent` preserve omitted fields and profile references (addresses [#11606](https://github.com/zeroclaw-labs/zeroclaw/issues/11606)).
- [PR #11695](https://github.com/zeroclaw-labs/zeroclaw/pull/11695) — `fix(zerocode)`: align Cost-tab "By Period" totals to UTC (addresses [#11694](https://github.com/zeroclaw-labs/zeroclaw/issues/11694)).
- [PR #11624](https://github.com/zeroclaw-labs/zeroclaw/pull/11624) — `fix(zerocode)`: answer dropped elicitations and record them (addresses [#11623](https://github.com/zeroclaw-labs/zeroclaw/issues/11623)).
- [PR #11700](https://github.com/zeroclaw-labs/zeroclaw/pull/11700) — `fix(zerocode)`: keep composer/completion keystrokes out of the transcript.

**Epic progress signals:** The plugin stack by @IftekharUddin continues to mature ([PR #11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261), [#11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262), [#11228](https://github.com/zeroclaw-labs/zeroclaw/pull/11228)), and ZeroCode UX/context work advances ([PR #10905](https://github.com/zeroclaw-labs/zeroclaw/pull/10905) manual context compaction, [#11622](https://github.com/zeroclaw-labs/zeroclaw/pull/11622) transcript timestamps).

## 4. Community Hot Topics

Ranked by comment volume:

1. **[#8692 — Maintainer decision queue for RFCs and design issues](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)** — 15 comments. The single most-discussed item. *Underlying need:* a transparent, prioritized decision pipeline for RFCs; the volume of design RFCs is outpacing maintainer decisions.
2. **[#8891 — Persistent memory: wire curation/relevance/operability planes to parity](https://github.com/zeroclaw-labs/zeroclaw/issues/8891)** — 11 comments. Tracks 7 open items (4 issues, 3 PRs). *Need:* cross-session memory parity with mature peer runtimes.
3. **[#11254 — RFC: A2A protocol crate (zeroclaw-a2a)](https://github.com/zeroclaw-labs/zeroclaw/issues/11254)** — 9 comments. *Need:* a first-class agent-to-agent wire model and discovery surface, building on #9106/#7763.
4. **[#11420 — SQLite session backend rewrites `created_at`](https://github.com/zeroclaw-labs/zeroclaw/issues/11420)** — 7 comments, `priority:p1`. High-engagement correctness bug.
5. **[#7432 — Runtime & gateway delivery tracker (v0.8.6/v0.9.0)](https://github.com/zeroclaw-labs/zeroclaw/issues/7432)** — 6 comments. Release-critical coordination.
6. **[#11235 — RFC: Knowledge corpus (RAG)](https://github.com/zeroclaw-labs/zeroclaw/issues/11235)** — 6 comments. *Need:* operator-owned document retrieval.
7. **[#11074 — `[[search_routes]]` provider routing for web_search_tool](https://github.com/zeroclaw-labs/zeroclaw/issues/11074)** — 5 comments. *Need:* per-query provider selection for source quality/corroboration.
8. **[#10025 — RFC: zeroclaw swarm (ephemeral agent swarms + TUI)](https://github.com/zeroclaw-labs/zeroclaw/issues/10025)** and **[#10573 — Bind pairing tokens to roster users](https://github.com/zeroclaw-labs/zeroclaw/issues/10573)** — 4 comments each.

**Analysis:** Community demand clusters around **multi-agent orchestration, durable memory/RAG, and identity-scoped access** — i.e., users are pushing ZeroClaw from a single-assistant tool toward a coordinated, multi-tenant agent platform.

## 5. Bugs & Stability

Ranked by severity/priority:

| Severity | Item | Status | Fix PR? |
|---|---|---|---|
| **S1 – workflow blocked** | [#11648](https://github.com/zeroclaw-labs/zeroclaw/issues/11648) Web dashboard pairing input caps at 6 digits while backend issues 32-char alphanumeric codes | OPEN | Not indicated |
| **p1** | [#11420](https://github.com/zeroclaw-labs/zeroclaw/issues/11420) SQLite rewrites `created_at` for every message each turn (per-message times lost) | OPEN | Not indicated |
| **p1 / high risk** | [#11606](https://github.com/zeroclaw-labs/zeroclaw/issues/11606) `model_routing_config upsert_agent` rewrites whole config, fabricates profiles, drops fields, resets limits | OPEN, in-progress | ✅ [PR #11693](https://github.com/zeroclaw-labs/zeroclaw/pull/11693) |
| **S2** | [#11655](https://github.com/zeroclaw-labs/zeroclaw/issues/11655) Core conflict supersede hides unrelated memories when recall has no vector scores | OPEN | Not indicated |
| **S2 (cost)** | [#11696](https://github.com/zeroclaw-labs/zeroclaw/issues/11696) System prompt rebuilt from disk every turn; personality edit busts cache for all live sessions | OPEN | ✅ [PR #11698](https://github.com/zeroclaw-labs/zeroclaw/pull/11698) + [#11702](https://github.com/zeroclaw-labs/zeroclaw/pull/11702) |
| **S2** | [#11623](https://github.com/zeroclaw-labs/zeroclaw/issues/11623) ZeroCode drops pending `ask_user`/`poll`, leaving 600s timeouts and no record | OPEN | ✅ [PR #11624](https://github.com/zeroclaw-labs/zeroclaw/pull/11624) |
| **S3** | [#11658](https://github.com/zeroclaw-labs/zeroclaw/issues/11658) Web chat input doesn't regain focus after reply | OPEN | Not indicated |
| **S3** | [#11694](https://github.com/zeroclaw-labs/zeroclaw/issues/11694) Cost tab mixes UTC and local-day totals without labeling | OPEN | ✅ [PR #11695](https://github.com/zeroclaw-labs/zeroclaw/pull/11695) |
| **S3** | [#11691](https://github.com/zeroclaw-labs/zeroclaw/issues/11691) Paused-queue hint shows ⌥P, which types π in macOS Terminal | OPEN | Related [PR #11700](https://github.com/zeroclaw-labs/zeroclaw/pull/11700) (key handling) |

**Assessment:** No crashes reported; issues are correctness/UX/cost regressions rather than hard failures. Positive signal: **three of today's newest bugs already have same-day fix PRs**, and one long-standing tool-semantics bug ([#11108](https://github.com/zeroclaw-labs/zeroclaw/issues/11108)) closed. The p1 SQLite timestamp bug ([#11420](https://github.com/zeroclaw-labs/zeroclaw/issues/11420)) is the most notable *unaddressed* stability item.

## 6. Feature Requests & Roadmap Signals

**Most likely to land next (tied to v0.8.6/v0.9.0 trackers or already in PR):**
- **Plugin system hardening/completion** — channel-instance seeding ([#10996](https://github.com/zeroclaw-labs/zeroclaw/issues/10996), closed), Discord plugin proof ([#10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999)), staged package replacement ([PR #11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261)), `plugin update` CLI ([PR #11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262)), TLS profile hardening ([PR #11228](https://github.com/zeroclaw-labs/zeroclaw/pull/11228)).
- **Gateway separation / security** — configurable `MAX_BODY_SIZE` ([#11668](https://github.com/zeroclaw-labs/zeroclaw/issues/11668)), pairing-token-to-roster binding ([#10573](https://github.com/zeroclaw-labs/zeroclaw/issues/10573)), scoped-principal log access ([PR #11326](https://github.com/zeroclaw-labs/zeroclaw/pull/11326)).
- **ZeroCode UX** — context compaction ([PR #10905](https://github.com/zeroclaw-labs/zeroclaw/pull/10905)), transcript timestamps ([PR #11622](https://github.com/zeroclaw-labs/zeroclaw/pull/11622)), draggable session rows ([PR #11573](https://github.com/zeroclaw-labs/zeroclaw/pull/11573)).

**Medium-term / RFC-stage (needs maintainer decision via [#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)):**
- **A2A protocol crate** ([#11254](https://github.com/zeroclaw-labs/zeroclaw/issues/11254)) — cross-cutting architecture RFC.
- **Knowledge corpus / RAG** ([#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235)) — needs-author-action.
- **Ephemeral agent swarms** ([#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025)) and **Level 5 "Dark Factory" workflow** ([#11664](https://github.com/zeroclaw-labs/zeroclaw/issues/11664)) — strong user appetite for orchestration.
- **Separate Channels from Core** ([#11662](https://github.com/zeroclaw-labs/zeroclaw/issues/11662)) — reduce deadweight/attack surface.
- **Durable peer-agent turns** ([#9597](https://github.com/zeroclaw-labs/zeroclaw/issues/9597)), **search_routes** ([#11074](https://github.com/zeroclaw-labs/zeroclaw/issues/11074)), **turn-consolidation target category** ([#11657](https://github.com/zeroclaw-labs/zeroclaw/issues/11657)).

**Prediction:** The next release (v0.8.6) will most likely ship **plugin-install/update hardening, gateway security scoping, and ZeroCode context/UX fixes**. A2A, swarm, and RAG remain post-v0.9.0 architecture bets.

## 7. User Feedback Summary

- **Multi-agent orchestration is the dominant unmet need.** [#11664](https://github.com/zeroclaw-labs/zeroclaw/issues/11664) ("Level 5 Dark Factory Workflow"), [#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025) (swarm TUI), and [#11254](https://github.com/zeroclaw-labs/zeroclaw/issues/11254) (A2A) all express the same pain: *"standing up a team of agents requires config surgery… there is no orchestrator."*
- **"Don't ship what I don't use."** [#11662](https://github.com/zeroclaw-labs/zeroclaw/issues/11662) calls unused channels "deadweight and a security risk" — users want modular channel separation.
- **Multi-agent relay friction is real.** [#11668](https://github.com/zeroclaw-labs/zeroclaw/issues/11668) reports long-context A2A `message/send` payloads being rejected by the gateway body limit in <1s with the opaque string `length limit exceeded` — a concrete production use case (front-desk → specialist agents).
- **Operator UX polish matters.** Browser/search tools being silently rewritten to `shell` ([#11108](https://github.com/zeroclaw-labs/zeroclaw/issues/11108)), pairing dialogs that break with 32-char codes ([#11648](https://github.com/zeroclaw-labs/zeroclaw/issues/11648)), focus loss after replies ([#11658](https://github.com/zeroclaw-labs/zeroclaw/issues/11658)), and mislabeled cost periods ([#11694](https://github.com/zeroclaw-labs/zeroclaw/issues/11694)) show users are running ZeroClaw in real multi-agent setups and hitting ergonomic edges.
- **Satisfaction signal:** fast same-day bug→fix turnaround on several items suggests an engaged maintainer group; dissatisfaction centers on **review latency and unclear RFC decisions**, not capability.

## 8. Backlog Watch

Items updated recently but long-open, needing maintainer attention (age = from creation to 2026-10-13):

| Item | Age | Why it needs attention |
|---|---|---|
| [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) Runtime & gateway delivery tracker (v0.8.6/v0.9.0) | ~126 days | Release-critical source of truth; still open |
| [#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) Maintainer decision queue | ~101 days | Most-commented item; RFC decision bottleneck |
| [#8891](https://github.com/zeroclaw-labs/zeroclaw/issues/8891) Persistent memory parity tracker | ~96 days | 7 open sub-items; high-severity dependency |
| [#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025) zeroclaw swarm RFC | ~58 days, `needs-author-action` | Popular orchestration RFC awaiting input |
| [#9597](https://github.com/zeroclaw-labs/zeroclaw/issues/9597) Durable/attributable peer-agent turns | ~74 days, `in-progress` | Blocks reliable multi-agent lifecycle |
| [#10290](https://github.com/zeroclaw-labs/zeroclaw/issues/10290) Thread live config handle into detached turns | ~50 days | Security-relevant follow-up (SSRF policy propagation) |
| [PR #10499](https://github.com/zeroclaw-labs/zeroclaw/pull/10499) validate persistent config writes | ~43 days, `needs-author-action`, size:XL | High-risk config correctness |
| [PR #10446](https://github.com/zeroclaw-labs/zeroclaw/pull/10446) reject tool-call envelopes leaked into prose | ~45 days, `needs-maintainer-review` | Production-observed bug on `gpt-5.6`/codex |
| [PR #10502](https://github.com/zeroclaw-labs/zeroclaw/pull/10502) relocate tool-result images (OpenAI-compat 400) | ~43 days, `needs-author-action` | Blocks OpenAI-compatible providers |
| [PR #10905](https://github.com/zeroclaw-labs/zeroclaw/pull/10905) manual recoverable context compaction | ~27 days, size:XL | High-value ZeroCode feature stuck in review |

**Health read:** The project has strong contributor momentum and a clear roadmap spine (memory → plugins → gateway separation → A2A/swarm). The primary risk is **decision and review latency**: three trackers with no-stale status and several `needs-author-action`/`needs-maintainer-review` PRs are accumulating. Clearing the maintainer decision queue ([#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)) and unblocking the v0.8.6 plugin/gateway PR stack would most improve merge throughput.

---
*Data notes: PR comment counts were not populated in the source sample (`undefined`), so PR ranking here reflects recency and state rather than comment volume. Only 3 of 4 closed issues and none of the 6 merged/closed PRs appear in the top-30/top-20 samples; details for the remainder are unavailable.*

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*