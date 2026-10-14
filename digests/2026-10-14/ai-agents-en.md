# OpenClaw Ecosystem Digest 2026-10-14

> Issues: 500 | PRs: 500 | Projects covered: 5 | Generated: 2026-10-14 01:21 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw Deep Dive

# OpenClaw Project Digest — 2026-10-14

_Scope: based on the provided GitHub snapshot for OpenClaw. Counts reflect the last 24h; issue/PR lists are the top items by comment count or recency in the supplied sample._

## 1. Today’s Overview

OpenClaw remains a very high-activity project: 500 issues and 500 PRs were updated in the last 24h, with 208 issues closed and 185 PRs merged/closed. Two releases landed, including the stable `v2026.10.3` and beta `v2026.10.5-beta.3`, both emphasizing update/install durability and runtime stability. The project has strong community engagement—`v2026.10.3` alone reports 90 commits, 79 PRs, and 99 contributors—but the open backlog is still heavy with P0/P1 reliability issues. The dominant risk themes are update failures, session/message loss, startup-performance regressions, resource leaks, and platform-specific issues on Windows and macOS. Health assessment: active and well-contributed, but stability debt is concentrated in core operational paths and several high-severity issues still lack a new fix PR.

## 2. Releases

### [v2026.10.5-beta.3](https://github.com/openclaw/openclaw/releases/tag/v2026.10.5-beta.3)
- **Type:** beta/prerelease.
- **Highlights from provided notes:** updates are intended to leave the Gateway running; pnpm 12 installs keep the Gateway service and `openclaw` launcher on the durable install path; Windows updates accept qualified Scheduled Task accounts and their own verified driver; macOS accepts protected Bun service configuration.
- **Breaking changes:** none explicitly stated in the provided snippet.
- **Migration notes:** platform-specific updater changes suggest users on pnpm 12, Windows Scheduled Task installs, and macOS Bun-managed services should watch this beta closely. No manual migration steps are listed in the provided data.

### [v2026.10.3](https://github.com/openclaw/openclaw/releases/tag/v2026.10.3)
- **Type:** stable release.
- **Scale:** 90 commits, 79 PRs, 99 contributors.
- **Changes:** release notes and changelog are described as identical content in two formats; full detail is linked via the project release/docs site.
- **Breaking changes:** none explicitly stated in the provided snippet.
- **Migration notes:** none explicitly stated in the provided snippet.
- **Docs reference:** [release notes](https://docs.openclaw.ai/releases/2026.10.3)

## 3. Project Progress

- **Overall throughput:** 185 PRs merged/closed and 208 issues closed in the last 24h.
- **Visible closed/landed items in the sample:**
  - [#97616](https://github.com/openclaw/openclaw/issues/97616) — zombie/unreaped hook and tool child processes.
  - [#47910](https://github.com/openclaw/openclaw/issues/47910) — provider fallback by failure class.
  - [#154834](https://github.com/openclaw/openclaw/issues/154834) — failed subagent delivery recurring in runtime context.
  - [#45508](https://github.com/openclaw/openclaw/issues/45508) — self-hosted STT/TTS in WebChat.
  - [#152839](https://github.com/openclaw/openclaw/issues/152839) — graceful `openat2` `ENOSYS` handling for gateway state lock.
  - [#103198](https://github.com/openclaw/openclaw/issues/103198) — WebChat image attachments mapped to media store paths.
  - [#87763](https://github.com/openclaw/openclaw/issues/87763) — SSRF DNS dispatcher and `autoSelectFamily` timeout conflict.
  - [#170724](https://github.com/openclaw/openclaw/issues/170724) — macOS 2026.10.2 app update incomplete.
  - [#124731](https://github.com/openclaw/openclaw/issues/124731) — `claude-cli` queue-mode steering silently degraded.
  - Closed PRs visible in sample: [#137878](https://github.com/openclaw/openclaw/pull/137878) (CLI approvals Scope column) and [#171525](https://github.com/openclaw/openclaw/pull/171525) (Control UI locale refresh).

- **High-signal open PRs in review/advancing:**
  - [#170534](https://github.com/openclaw/openclaw/pull/170534) — honor silent turn endings without fabricated answers.
  - [#171526](https://github.com/openclaw/openclaw/pull/171526) — Muse Code sign-in from Model Setup.
  - [#169722](https://github.com/openclaw/openclaw/pull/169722) — run sessions in existing folders on paired devices.
  - [#171476](https://github.com/openclaw/openclaw/pull/171476) — validate Android phone and Wear releases in CI.
  - [#171497](https://github.com/openclaw/openclaw/pull/171497) — backport Doctor cleanup settlement to 2026.10.5.
  - [#171135](https://github.com/openclaw/openclaw/pull/171135) — keep Workboard chat catalog reads out of card history.
  - [#171447](https://github.com/openclaw/openclaw/pull/171447) — recover ACP-only stores after slow startup.
  - [#171510](https://github.com/openclaw/openclaw/pull/171510) — move bundled inference/logout SQLite into auth workers.
  - [#171529](https://github.com/openclaw/openclaw/pull/171529) — load earlier Control UI messages when chat is underfilled.
  - [#171530](https://github.com/openclaw/openclaw/pull/171530) — reduce New Session typing lag after models load.
  - [#171531](https://github.com/openclaw/openclaw/pull/171531) — backport October update/model/stability fixes to 2026.9.33.
  - [#164442](https://github.com/openclaw/openclaw/pull/164442) — long transcripts pinning a DB read snapshot during decode.

## 4. Community Hot Topics

### Most active issues by comments/reactions in the supplied sample

| Issue | Status | Comments / 👍 | Signal |
|---|---:|---:|---|
| [#148707](https://github.com/openclaw/openclaw/issues/148707) — reply lost with “no active tool authority snapshot” | OPEN | 19 / 0 | P1 session/message-loss regression on 2026.9.4 |
| [#97616](https://github.com/openclaw/openclaw/issues/97616) — unreaped hook/tool child processes | CLOSED | 19 / 1 | Process lifecycle/zombie accumulation |
| [#38327](https://github.com/openclaw/openclaw/issues/38327) — “Cannot convert undefined or null to object” with google-vertex/gemini | OPEN | 18 / 3 | P0 auth/provider regression, high reaction signal |
| [#114612](https://github.com/openclaw/openclaw/issues/114612) — memory-core SQLite unbounded growth | OPEN | 17 / 0 | Disk fill risk from no retention policy |
| [#154572](https://github.com/openclaw/openclaw/issues/154572) — `sessions_spawn` to claude-cli child fails | OPEN | 15 / 0 | Subagent/session transcript writer failure |
| [#156112](https://github.com/openclaw/openclaw/issues/156112) — `openclaw update` fails at global install swap | OPEN | 15 / 0 | P0 updater failure vs successful npm install |
| [#155859](https://github.com/openclaw/openclaw/issues/155859) — Gateway startup scales with plugin count | OPEN | 14 / 0 | P0 startup/plugin performance regression |
| [#144291](https://github.com/openclaw/openclaw/issues/144291) — config hot-reload aborts in-flight turns | OPEN | 13 / 0 | Config/session stability |
| [#47910](https://github.com/openclaw/openclaw/issues/47910) — provider fallback by failure class | CLOSED | 13 / 0 | Provider resilience feature request landed/closed |
| [#118839](https://github.com/openclaw/openclaw/issues/118839) — restart recovery claim changed before adoption | OPEN | 13 / 0 | Restart recovery/session-state regression |
| [#168955](https://github.com/openclaw/openclaw/issues/168955) — startup ready ~1030s vs ~410s on Windows | OPEN | 12 / 0 | P1 Windows startup regression, updated 2026-10-14 |
| [#101929](https://github.com/openclaw/openclaw/issues/101929) — context-overflow estimator over-counts | OPEN | 12 / 1 | Token accounting/session state |
| [#163029](https://github.com/openclaw/openclaw/issues/163029) — external plugin reload mid-turn stalls | OPEN | 11 / 0 | 40–70s main-thread stalls |
| [#158390](https://github.com/openclaw/openclaw/issues/158390) — plugin-captures tmp dirs not GC’d | OPEN | 11 / 1 | P0 disk-fill risk |
| [#161379](https://github.com/openclaw/openclaw/issues/161379) — CPU pinned by model catalog refresh loop | OPEN | 11 / 0 | P1 CPU exhaustion/regression |
| [#166894](https://github.com/openclaw/openclaw/issues/166894) — Windows gateway restart hangs 30m–2h | OPEN | 9 / 0 | P0 Windows multi-agent install |
| [#170331](https://github.com/openclaw/openclaw/issues/170331) — beta update candidate config read exits 1 | OPEN | 9 / 0 | Beta updater/systemd unit issue |
| [#48709](https://github.com/openclaw/openclaw/issues/48709) — Gemini 2.5 Pro textSignature bloat | OPEN | 8 / 0 | Session bloat/TG delivery failures, updated 2026-10-14 |
| [#99659](https://github.com/openclaw/openclaw/issues/99659) — OOM killed after companion app connected | OPEN | 7 / 1 | Memory spike/crash, updated 2026-10-14 |

### Notable PRs in the snapshot
PR comment/reaction counts were not provided. Based on status and recency, notable PRs include [#170534](https://github.com/openclaw/openclaw/pull/170534), [#171526](https://github.com/openclaw/openclaw/pull/171526), [#169722](https://github.com/openclaw/openclaw/pull/169722), [#171489](https://github.com/openclaw/openclaw/pull/171489), [#171531](https://github.com/openclaw/openclaw/pull/171531), [#171510](https://github.com/openclaw/openclaw/pull/171510), [#171528](https://github.com/openclaw/openclaw/pull/171528), [#171529](https://github.com/openclaw/openclaw/pull/171529), and [#171530](https://github.com/openclaw/openclaw/pull/171530).

**Underlying needs:** the hottest issues cluster around trust in core delivery paths—message replies, subagent completion, restart recovery, provider auth, updater reliability, and disk/memory lifecycle. Users are not asking for cosmetic features first; they need OpenClaw to keep sessions alive, avoid silent message loss, update safely, and avoid unbounded resource growth.

## 5. Bugs & Stability

### P0 issues in the snapshot
- [#156112](https://github.com/openclaw/openclaw/issues/156112) — `openclaw update` fails at global install swap on npm global install. Tagged `clawsweeper:manual-only`.
- [#155859](https://github.com/openclaw/openclaw/issues/155859) — Gateway startup wall-time scales with enabled plugin count; discord/codex/openclaw-weixin dominate budget. Tagged `no-new-fix-pr`.
- [#158390](https://github.com/openclaw/openclaw/issues/158390) — plugin-captures tmp dirs not GC’d; disk fills indefinitely. Tagged `no-new-fix-pr`.
- [#166894](https://github.com/openclaw/openclaw/issues/166894) — Windows multi-agent gateway restart hangs 30+ minutes to 2 hours. Tagged `needs-info`.
- [#167557](https://github.com/openclaw/openclaw/issues/167557) — deferred plugin data/settings migration never converges; package recovery lease identity error.
- [#166221](https://github.com/openclaw/openclaw/issues/166221) — update 2026.9.4 → 2026.9.8 fails during SQLite preflight/candidate snapshot.
- [#161921](https://github.com/openclaw/openclaw/issues/161921) — after `state-migrated-no-rollback`, Doctor and plugin update require each other.
- [#160060](https://github.com/openclaw/openclaw/issues/160060) — Windows gateway silently dies after post-standby thaw; no exit log/WER dump.
- [#161436](https://github.com/openclaw/openclaw/issues/161436) — cloud-worker placement bypasses `before_tool_call`, so policy plugins cannot guard remote workers.
- [#137085](https://github.com/openclaw/openclaw/issues/137085) — macOS app device identity stuck in `native-importing`; native gateway handshake timeout.
- [#38327](https://github.com/openclaw/openclaw/issues/38327) — google-vertex/gemini-3.1-pro-preview regression with “Cannot convert undefined or null to object”. Tagged `no-new-fix-pr`, `needs-maintainer-review`.

### P1 / high-impact issues
- [#148707](https://github.com/openclaw/openclaw/issues/148707) — reply lost when a second run displaces an in-flight turn; message-loss regression. Tagged `no-new-fix-pr`.
- [#154572](https://github.com/openclaw/openclaw/issues/154572) — `sessions_spawn` to claude-cli child fails with `SessionTranscriptWriterClaimReboundError`.
- [#144291](https://github.com/openclaw/openclaw/issues/144291) — config hot-reload aborts every in-flight agent turn.
- [#118839](https://github.com/openclaw/openclaw/issues/118839) — restart recovery claim changed before agent adoption on WebChat → Telegram-bound sessions.
- [#163029](https://github.com/openclaw/openclaw/issues/163029) — external plugin reload mid-turn causes 40–70s main-thread stalls.
- [#125764](https://github.com/openclaw/openclaw/issues/125764) — Telegram outbound sends dead-lettered after one network failure; announce/completion replies lost.
- [#161379](https://github.com/openclaw/openclaw/issues/161379) — Gateway pins a CPU core in prepared model catalog refresh loop.
- [#159512](https://github.com/openclaw/openclaw/issues/159512) — claude-cli subagent completion invalidates requester CLI session twice.
- [#90840](https://github.com/openclaw/openclaw/issues/90840) — subagent run completion delivered as raw worker output instead of parent summary.
- [#126950](https://github.com/openclaw/openclaw/issues/126950) — Telegram opaque callback payloads silently dropped for non-plugin callers.
- [#155119](https://github.com/openclaw/openclaw/issues/155119) — subagent final answer lost/truncated; duplicate settle events; delivery status stuck pending.
- [#144336](https://github.com/openclaw/openclaw/issues/144336) — deferred config-reload restart fails shutdown step with global singleton lifecycle error.
- [#128809](https://github.com/openclaw/openclaw/issues/128809) — Slack socket-mode reconnects leak sockets up to 10-connection cap.
- [#149564](https://github.com/openclaw/openclaw/issues/149564) — cron lane `scheduledToolPolicy.mode="account"` resolves to deny-all tools.
- [#168955](https://github.com/openclaw/openclaw/issues/168955) — Windows startup regression: ready ~1030s vs ~410s; `sidecars.control-ui-assets` blocks 477s.

### Crashes, regressions, and resource risks
- Crashes/OOM: [#99659](https://github.com/openclaw/openclaw/issues/99659) OOM after companion app connection; [#160060](https://github.com/openclaw/openclaw/issues/160060) silent Windows death; [#166221](https://github.com/openclaw/openclaw/issues/166221) update crash/failure.
- Regressions: [#148707](https://github.com/openclaw/openclaw/issues/148707) 2026.9.4 regression; [#38327](https://github.com/openclaw/openclaw/issues/38327) provider regression; [#155859](https://github.com/openclaw/openclaw/issues/155859) 2026.9.5 startup regression; [#168955](https://github.com/openclaw/openclaw/issues/168955) 2026.10.1 startup regression; [#163029](https://github.com/openclaw/openclaw/issues/163029) 2026.9.7 plugin reload regression; [#161379](https://github.com/openclaw/openclaw/issues/161379) 2026.9.6 CPU loop; [#144291](https://github.com/openclaw/openclaw/issues/144291) 2026.9.3 hot-reload regression.
- Resource leaks: [#97616](https://github.com/openclaw/openclaw/issues/97616) zombie processes (closed), [#114612](https://github.com/openclaw/openclaw/issues/114612) SQLite unbounded growth, [#158390](https://github.com/openclaw/openclaw/issues/158390) plugin-captures disk fill, [#164294](https://github.com/openclaw/openclaw/issues/164294) WAL checkpoint never completes.

### Fix PR coverage
Several visible PRs target stability: [#170534](https://github.com/openclaw/openclaw/pull/170534) (silent turn endings), [#171447](https://github.com/openclaw/openclaw/pull/171447) (ACP-only store recovery), [#171497](https://github.com/openclaw/openclaw/pull/171497) (Doctor cleanup), [#171510](https://github.com/openclaw/openclaw/pull/171510) (auth SQLite workers), [#171528](https://github.com/openclaw/openclaw/pull/171528) (native app reconnect messages), [#171529](https://github.com/openclaw/openclaw/pull/171529) (Control UI earlier messages), [#171530](https://github.com/openclaw/openclaw/pull/171530) (New Session typing lag), [#171531](https://github.com/openclaw/openclaw/pull/171531) (October backports), [#171402](https://github.com/openclaw/openclaw/pull/171402) (memory stalls), and [#164442](https://github.com/openclaw/openclaw/pull/164442) (transcript DB snapshot). However, many P0/P1 issues above are explicitly tagged `clawsweeper:no-new-fix-pr` or `needs-maintainer-review`, so fix coverage is uneven.

## 6. Feature Requests & Roadmap Signals

### Provider/model integration
- [#155633](https://github.com/openclaw/openclaw/issues/155633) — add Databricks Unity Gateway as an official model provider; linked implementation [#155634](https://github.com/openclaw/openclaw/pull/155634).
- [#47910](https://github.com/openclaw/openclaw/issues/47910) — provider fallback by failure class; closed, likely reflected in provider resilience work.
- [#148397](https://github.com/openclaw/openclaw/pull/148397) — qwen chat-template models honoring declared reasoning efforts.
- [#171526](https://github.com/openclaw/openclaw/pull/171526) — Muse Code sign-in from Model Setup.
- [#171531](https://github.com/openclaw/openclaw/pull/171531) — backports include Muse Wonder, MiniMax M3.1 thinking, MiniMax OAuth refresh.

### Sessions, workspaces, and agents
- [#169722](https://github.com/openclaw/openclaw/pull/169722) — run sessions in existing folders on paired devices.
- [#105617](https://github.com/openclaw/openclaw/pull/105617) — exact-turn capability projection reports.
- [#90840](https://github.com/openclaw/openclaw/issues/90840) — subagent completion should return parent/main summary rather than raw worker output.

### UI / UX / docs
- [#111143](https://github.com/openclaw/openclaw/issues/111143) — pin plugin tabs in Control UI sidebar.
- [#168703](https://github.com/openclaw/openclaw/pull/168703) — simplify Settings and configuration help.
- [#168693](https://github.com/openclaw/openclaw/pull/168693) — simplify getting started and provider setup.
- [#168702](https://github.com/openclaw/openclaw/pull/168702) — simplify reference and CI guides.
- [#171529](https://github.com/openclaw/openclaw/pull/171529), [#171530](https://github.com/openclaw/openclaw/pull/171530), [#171533](https://github.com/openclaw/openclaw/pull/171533) — Control UI polish/performance.

### Security and plugin boundaries
- [#91676](https://github.com/openclaw/openclaw/issues/91676) — opt-in `skills.workshop.allowSandboxedAgents`.
- [#161436](https://github.com/openclaw/openclaw/issues/161436) — cloud-worker placement bypasses `before_tool_call`.
- [#151345](https://github.com/openclaw/openclaw/issues/151345) — Telegram progress draft suppressed when plugin registers modifying outbound hook.
- [#171532](https://github.com/openclaw/openclaw/pull/171532) — portable Claw manifest categories.

**Prediction:** the next release window is likely to prioritize update/install reliability, session recovery, provider/model integrations, and Control UI performance. Security-boundary issues around cloud workers and sandboxed agents may become more prominent if maintainers prioritize policy-plugin coverage.

## 7. User Feedback Summary

**Main pain points from the snapshot:**
- **Updater/install reliability:** users report update failures on npm, pnpm, Windows Scheduled Tasks, macOS app/runtime mismatches, and beta candidate config reads: [#156112](https://github.com/openclaw/openclaw/issues/156112), [#166221](https://github.com/openclaw/openclaw/issues/166221), [#170331](https://github.com/openclaw/openclaw/issues/170331), [#170332](https://github.com/openclaw/openclaw/issues/170332), [#170724](https://github.com/openclaw/openclaw/issues/170724).
- **Message/session loss:** replies, Telegram sends, subagent completions, and queued steering are silently lost or dead-lettered in several reports: [#148707](https://github.com/openclaw/openclaw/issues/148707), [#125764](https://github.com/openclaw/openclaw/issues/125764), [#155119](https://github.com/openclaw/openclaw/issues/155119), [#126950](https://github.com/openclaw/openclaw/issues/126950), [#124731](https://github.com/openclaw/openclaw/issues/124731).
- **Performance/startup regressions:** plugin count, model catalog refresh, Windows boot, and external plugin reloads cause severe stalls: [#155859](https://github.com/openclaw/openclaw/issues/155859), [#168955](https://github.com/openclaw/openclaw/issues/168955), [#161379](https://github.com/openclaw/openclaw/issues/161379), [#163029](https://github.com/openclaw/openclaw/issues/163029).
- **Resource exhaustion:** zombie processes, SQLite growth, WAL checkpoint failures, and plugin-capture temp dirs create operational risk: [#97616](https://github.com/openclaw/openclaw/issues/97616), [#114612](https://github.com/openclaw/openclaw/issues/114612), [#164294](https://github.com/openclaw/openclaw/issues/164294), [#158390](https://github.com/openclaw/openclaw/issues/158390).
- **Platform-specific friction:** Windows standby/restart and macOS identity/update issues are recurring: [#166894](https://github.com/openclaw/openclaw/issues/166894), [#160060](https://github.com/openclaw/openclaw/issues/160060), [#137085](https://github.com/openclaw/openclaw/issues/137085), [#99659](https://github.com/openclaw/openclaw/issues/99659).
- **Security/policy gaps:** users want guardrails to apply consistently to cloud workers, sandboxed agents, and outbound plugin hooks: [#161436](https://github.com/openclaw/openclaw/issues/161436), [#91676](https://github.com/openclaw/openclaw/issues/91676), [#151345](https://github.com/openclaw/openclaw/issues/151345).

**Satisfaction signals:** high contributor count, active closure of long-running issues, multiple releases, and visible backport/stability PRs show maintainers are responsive.  
**Dissatisfaction signals:** repeated regressions across releases, P0 issues with no fix PR, long-lived P1/P2 reports, and operational burden for self-hosted/multi-agent/Windows users.

## 8. Backlog Watch

Important issues/PRs needing maintainer attention, especially older high-severity items or items tagged `no-new-fix-pr` / `needs-maintainer-review`.

| Item | Severity / Type | Age in snapshot | Why watch |
|---|---|---|---|
| [#38327](https://github.com/openclaw/openclaw/issues/38327) | P0 bug/regression | Created 2026-03-06 | 18 comments, 3 👍, provider auth failure, no new fix PR |
| [#114612](https://github.com/openclaw/openclaw/issues/114612) | P1 disk growth | Created 2026-07-27 | 17 comments; SQLite retention policy absent |
| [#90840](https://github.com/openclaw/openclaw/issues/90840) | P1 subagent leak | Created 2026-06-06 | 9 comments; raw worker output to user, security/session impact |
| [#125764](https://github.com/openclaw/openclaw/issues/125764) | P1 message loss | Created 2026-08-18 | 11 comments; Telegram dead-lettering after one attempt |
| [#128809](https://github.com/openclaw/openclaw/issues/128809) | P1 channel stability | Created 2026-08-24 | 6 comments; Slack socket leak up to connection cap |
| [#137085](https://github.com/openclaw/openclaw/issues/137085) | P0 macOS app | Created 2026-09-03 | 7 comments; device identity stuck and gateway handshake timeout |
| [#91676](https://github.com/openclaw/openclaw/issues/91676) | P2 security/plugin | Created 2026-06-09 | 7 comments; sandboxed-agent skill workshop opt-in |
| [#48709](https://github.com/openclaw/openclaw/issues/48709) | P2 session bloat | Created 2026-03-17 | 8 comments; stale but still updated 2026-10-14 |
| [#99659](https://github.com/openclaw/openclaw/issues/99659) | P2 crash/OOM | Created 2026-07-03 | 7 comments; companion app memory spike/OOM |
| [#105617](https://github.com/openclaw/openclaw/pull/105617) | P3 PR | Created 2026-07-12 | Long-open capability projection report; needs proof |
| [#145242](https://github.com/openclaw/openclaw/pull/145242) | P2 PR | Created 2026-09-11 | Heartbeat skips recorded as failed; needs proof |
| [#148397](https://github.com/openclaw/openclaw/pull/148397) | P2 PR | Created 2026-09-14 | Qwen reasoning effort support; needs proof |

**Backlog takeaway:** the project is not short on reported signal—it is short on resolution bandwidth for a handful of high-impact, long-lived operational bugs. Maintainer attention should likely focus first on P0 update/session/startup failures and resource-lifecycle issues, because these affect trust in everyday OpenClaw use more than new surface-area features.

---

## Cross-Ecosystem Comparison

# Cross-Project Comparison Report — 2026-10-14

## 1. Ecosystem Overview
The personal AI assistant / agent open-source landscape is bifurcating into full assistant platforms and focused agent runtimes, gateways, and extension frameworks. OpenClaw dominates activity and release cadence, while Hermes Agent, QwenPaw, ZeroClaw, and IronClaw occupy narrower but distinct niches around gateway orchestration, console UX, plugin security, and declarative extensibility. Across projects, the community conversation has shifted from raw feature acquisition toward operational trust: session durability, update/install reliability, cost observability, policy enforcement, and resource lifecycle management. Release cadence is uneven—only OpenClaw shipped releases in the window—while the mid-tier projects are heavily triage- and PR-pipeline-bound. The ecosystem is maturing from “what can an agent do?” toward “can it run reliably, securely, and affordably every day?”

## 2. Activity Comparison
Health score is an analyst composite (0–10) weighting 24h throughput, closure/merge ratio, release cadence, open critical risk, and backlog age. It is not an official project metric.

| Project | Issues 24h / closed | PRs 24h / merged-closed | Open backlog | Release status | Health |
|---|---:|---:|---|---|---:|
| **OpenClaw** | 500 / 208 | 500 / 185 | Heavy P0/P1; exact open not supplied | `v2026.10.3` stable; `v2026.10.5-beta.3` | **7.0** |
| **Hermes Agent** | 50 / 4 | 50 / 3 | 46 issues, 47 PRs | None | **6.5** |
| **IronClaw** | 1 / 0 | 1 / 0 | 1 issue, 1 PR | None | **7.0** |
| **QwenPaw** | 20 / 4 | 42 / 20 | 16 issues, 22 PRs | None | **7.5** |
| **ZeroClaw** | 24 / 0 | 50 / 5 | 24 issues, 45 PRs | None | **6.0** |

**Readout:** OpenClaw operates at roughly 10× the activity of the mid-tier projects. QwenPaw has the strongest merge throughput among mid-tier peers. ZeroClaw has a large PR pipeline but no issue closures today, indicating review/release-gating pressure. IronClaw is stable but quiet.

## 3. OpenClaw’s Position
**Advantages vs peers:** OpenClaw is the clear ecosystem leader by scale and breadth. In one 24h window it closed 208 issues, merged/closed 185 PRs, and shipped both a stable and beta release; its stable release alone reports 99 contributors. No peer approaches that contributor or throughput scale. OpenClaw also has the broadest surface area: gateway, Control UI, WebChat, Telegram/Slack, subagents, plugins, model providers, updater/Doctor, and macOS/Windows/mobile-adjacent CI validation.

**Technical approach differences:** OpenClaw is an integrated personal assistant platform rather than a single-purpose agent framework. Peers are narrower: Hermes Agent emphasizes gateway/cron, delegation, compression, and desktop; QwenPaw emphasizes console UX, plugin marketplaces, ACP, and Code Mode; ZeroClaw focuses on runtime/gateway separation, WASM plugin hosting, and cost/session correctness; IronClaw proposes declarative hosted-MCP extensions with clean host/extension boundaries.

**Community size comparison:** Based on activity, OpenClaw’s community is substantially larger—likely an order of magnitude larger than Hermes, QwenPaw, or ZeroClaw, and far larger than IronClaw. Its main risk is not adoption but stability debt: P0/P1 issues around update failures, message/session loss, startup regressions, and resource leaks remain open, often tagged `no-new-fix-pr` or `needs-maintainer-review`.

## 4. Shared Technical Focus Areas
Requirements emerging across multiple projects:

- **Session/message durability and recovery:** OpenClaw (lost replies, restart recovery), Hermes (checkpoint snapshots, silent prompt loss, gateway exits), QwenPaw (silent empty completions, wedged sessions, cron cancellation), ZeroClaw (SQLite `created_at` rewrites, trimming deletes older turns, retired gateway sockets).
- **Update/install/deployment reliability:** OpenClaw (npm/pnpm/Windows/macOS update failures), Hermes (false Remote Desktop update failure, Docker `usermod` hang), QwenPaw (Windows file locking, reverse-proxy basename, GitHub skill binary corruption), ZeroClaw (v0.8.6 release hardening, plugin host artifacts, quickstart plugin install).
- **Cost observability, token accounting, and prompt cache efficiency:** ZeroClaw (OpenRouter $0.00 spend, dropped `total_tokens`, ignored `cost.warn_at_percent`, prompt-cache busting), OpenClaw (context-overflow estimator, model catalog CPU), Hermes (Codex quota/reset), QwenPaw (model capability filtering).
- **Security boundaries and plugin governance:** all five. OpenClaw (policy hooks vs cloud workers, sandboxed agents), Hermes (Managed Scope `terminal.*` bypass, dependency advisories), IronClaw (declarative auth/egress for hosted-MCP), QwenPaw (skill path sanitization, plugin provenance/hot reload), ZeroClaw (WASM plugin host, egress denials, TLS profiles, scoped pairing tokens).
- **Model/provider routing and flexibility:** OpenClaw (provider fallback by failure class, Gemini/Vertex auth), Hermes (per-call model/reasoning overrides, provider namespace), QwenPaw (default agent model edits), ZeroClaw (model routing config, Anthropic thinking/cache).
- **Resource lifecycle and performance:** OpenClaw (startup scales with plugins, SQLite growth, temp-dir leaks, CPU loops), Hermes (memory limits, cron O(profiles) re-parses), QwenPaw (base64 history bloat, polling), ZeroClaw (JPEG memory caps, system prompt rebuilds).
- **Async/background/cron UX:** QwenPaw (task callbacks, cancellable downloads, cron cancellation), Hermes (cron retry/perf, gateway drain), ZeroClaw (SOP headless runs, Telegram 429 handling).
- **Platform/deployment polish:** OpenClaw (Windows/macOS), Hermes (macOS gateway, desktop), QwenPaw (Windows, Docker/NAS, air-gapped enterprise), ZeroClaw (Telegram/WebSocket gateway).
- **Subagent/delegation lifecycle:** OpenClaw (subagent delivery, parent summaries), Hermes (delegation overrides, subagent lifecycle facade).

## 5. Differentiation Analysis

| Project | Primary focus | Target users | Architecture / approach |
|---|---|---|---|
| **OpenClaw** | Full personal AI assistant platform | Self-hosters, power users, plugin/agent ecosystem | Integrated gateway, sessions, Control UI, multi-channel, plugins/skills, multi-platform updater |
| **Hermes Agent** | Agent framework + gateway/cron/desktop | Agent builders, contributors, Docker/remote-desktop users | Gateway/multiplex, delegation, compression, provider auth, voice, desktop |
| **IronClaw** | Secure declarative extensibility | Extension/companion builders, security-conscious teams | Host + optional Plazmi extension, hosted-MCP metadata, declarative auth/egress |
| **QwenPaw** | Console UX + plugin/skill marketplace | Console users, plugin developers, enterprise/air-gapped deployments | AgentScope Python stack, console, ACP, SQLite history, marketplaces, Code Mode |
| **ZeroClaw** | Runtime/gateway separation + cost/session correctness | Cost-sensitive advanced users, gateway operators | v0.8.6/v0.9.0 release train, WASM plugin host, model routing, session stores, Telegram |

**Key takeaway:** OpenClaw competes on breadth and integration; Hermes on agent-orchestration flexibility; QwenPaw on console and plugin marketplace experience; ZeroClaw on runtime/gateway architecture and cost fidelity; IronClaw on clean extension boundaries and declarative security surfaces.

## 6. Community Momentum & Maturity
**Activity tiers:**
- **Hyper-scale / platform leader:** OpenClaw. Rapid iteration, frequent releases, largest contributor base, but visible stability debt.
- **High-velocity mid-tier:** QwenPaw, ZeroClaw, Hermes Agent. QwenPaw shows strong merge throughput; ZeroClaw has a large but review-gated PR pipeline; Hermes is triage-heavy with multiple targeted fix PRs in flight.
- **Quiet / stable:** IronClaw. Low activity, no reported bugs, but limited momentum and maintainer attention.

**Maturity assessment:**
- **OpenClaw** is the most mature and broadest platform, but not the healthiest by stability metrics.
- **QwenPaw** is in active 2.2.x maintenance/stabilization, with critical console/media/session bugs still open.
- **ZeroClaw** is in release-hardening for v0.8.6 and architecture work for v0.9.0; many P1/S2 issues remain open.
- **Hermes Agent** is actively remediating stability and contributor-workflow issues, but merge throughput is low relative to open PRs.
- **IronClaw** is early/quiet; the main signal is a single extensibility proposal awaiting triage.

## 7. Trend Signals
1. **Reliability is the new feature.** Across OpenClaw, Hermes, QwenPaw, and ZeroClaw, users care more about no-lost-messages, durable sessions, restart recovery, and safe updates than new UI surfaces.
2. **Cost observability is becoming first-class.** ZeroClaw’s OpenRouter/token-accounting issues, prompt-cache invalidation reports, and `cost.warn_at_percent` failures show that developers need trustworthy billing and cache metrics.
3. **Security policy must follow execution context.** Cloud workers, sandboxed agents, terminal scopes, MCP/egress, plugin TLS, and dependency advisories are recurring boundary gaps.
4. **Plugin ecosystems need lifecycle maturity.** Hot reload, rollback safety, provenance, marketplace sources, theme extension points, WASM hosts, and per-agent MCP grants are active demand areas.
5. **Deployment portability is a blocker, not polish.** Windows file locking, macOS identity/gateway exits, Docker FUSE hangs, reverse-proxy subpaths, and air-gapped mirrors are recurring friction points.
6. **Async/background work needs better UX.** Callbacks, cancellable downloads, cron retries, silent cancellation warnings, and gateway drain behavior are common requests.
7. **Resource lifecycle management is operational risk.** SQLite retention, temp-dir GC, memory limits, socket leaks, zombie processes, and base64 history bloat appear across multiple projects.
8. **Model routing and provider flexibility remain strategic.** Fallback by failure class, per-call overrides, custom providers, capability-aware media filtering, and model-picker filtering are all active roadmap signals.

**Value for AI agent developers:** Build for failure first—idempotent delivery, durable session stores, clear recovery paths, policy-enforced execution boundaries, cost/token observability, plugin isolation, and cross-platform install/update. Projects that solve these operational trust issues will differentiate more than those adding new agent capabilities.

---

## Peer Project Reports

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent Project Digest — 2026-10-14

## 1. Today’s Overview
Hermes Agent remains highly active: **50 issues and 50 PRs were updated in the last 24h**, with **no new releases**. The backlog is still large — **46 open/active issues and 47 open PRs** — while only **4 issues closed and 3 PRs merged/closed**, indicating a triage-heavy day with modest merge throughput. Activity is concentrated around stability, compatibility, security-boundary, gateway/cron, Docker, desktop, and provider-auth issues. Several P1/P2 regressions remain open, but multiple targeted fix PRs are in flight, suggesting active remediation.

## 2. Releases
No new releases were published in the last 24 hours. No breaking changes or migration notes to report.

## 3. Project Progress
- **Closed/merged volume:** 4 issues closed, 3 PRs merged/closed. The provided PR dataset does not include merged-PR titles, so merged-PR attribution is limited.
- **Visible issue closures:**
  - [#62182](https://github.com/NousResearch/hermes-agent/issues/62182) — closed: status/error display no longer shows internal `custom` alias instead of the configured provider name.
  - [#77072](https://github.com/NousResearch/hermes-agent/issues/77072) — closed: Docker `usermod` hang with rclone FUSE mounts blocking s6 boot.
- **Active fix/feature PRs updated today:**
  - [#138130](https://github.com/NousResearch/hermes-agent/pull/138130) — fix pre-API compression timeout handling.
  - [#138128](https://github.com/NousResearch/hermes-agent/pull/138128) — re-probe Codex quota before benching exhausted fallback.
  - [#138125](https://github.com/NousResearch/hermes-agent/pull/138125) — scope Codex reset cooldown clearing to the redeeming account.
  - [#138111](https://github.com/NousResearch/hermes-agent/pull/138111) — make `search_files` ignore user ripgrep config.
  - [#138122](https://github.com/NousResearch/hermes-agent/pull/138122) — cron multiplex perf: resolve primary routes once per tick, part of [#138105](https://github.com/NousResearch/hermes-agent/issues/138105).
  - [#138120](https://github.com/NousResearch/hermes-agent/pull/138120) and [#138124](https://github.com/NousResearch/hermes-agent/pull/138124) — Docker subdirectory hint path mapping for [#138115](https://github.com/NousResearch/hermes-agent/issues/138115).
  - [#138126](https://github.com/NousResearch/hermes-agent/pull/138126) — voice-only auto replies.
  - [#138127](https://github.com/NousResearch/hermes-agent/pull/138127) — opt-in public speech Responses streaming.
  - [#110907](https://github.com/NousResearch/hermes-agent/pull/110907) — exact-value `.env` secret redaction.
  - [#137553](https://github.com/NousResearch/hermes-agent/pull/137553) — refuse internal wakes while gateway drains for restart.
  - [#118548](https://github.com/NousResearch/hermes-agent/pull/118548) — cron provider-unreachable retry fix for [#118536](https://github.com/NousResearch/hermes-agent/issues/118536).
  - [#103373](https://github.com/NousResearch/hermes-agent/pull/103373) — compression attempt budget re-arm for [#103355](https://github.com/NousResearch/hermes-agent/issues/103355).

## 4. Community Hot Topics
PR comment counts were not provided in the dataset; the hottest visible discussion is in issues.

| Item | Signal | Underlying need |
|---|---|---|
| [#131859](https://github.com/NousResearch/hermes-agent/issues/131859) | 38 comments, P2, blocked | Contributor workflow is blocked by an API `CreatePullRequest` permission error; needs auth/platform resolution. |
| [#5320](https://github.com/NousResearch/hermes-agent/issues/5320) | 13 comments, 3 👍 | Long-running sessions hit low memory limits; users want larger defaults or auto-scaling. |
| [#80222](https://github.com/NousResearch/hermes-agent/issues/80222) | 8 comments, 3 👍 | Delegation cannot override model/reasoning per call; users need cost/latency routing flexibility. |
| [#106592](https://github.com/NousResearch/hermes-agent/issues/106592) | 8 comments | Remote Desktop update reports failure even after successful systemd restart; users need accurate update state. |
| [#133212](https://github.com/NousResearch/hermes-agent/issues/133212) | 7 comments | Plugin orchestrators need a stable subagent lifecycle facade for steer/isolation/route/follow-up. |
| [#68536](https://github.com/NousResearch/hermes-agent/issues/68536) | 6 comments | Custom provider `/models` status filtering is missing, flooding the model picker with unavailable models. |
| [#137838](https://github.com/NousResearch/hermes-agent/issues/137838) | 6 comments, P1 | macOS gateway repeatedly exits 75 after missed liveness probes; root cause still unconfirmed. |
| [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) | 5 comments | Tracking unified gateway ownership of all local sessions; architectural consistency and session durability. |

## 5. Bugs & Stability
Ranked by severity, based on provided issue/PR metadata.

### P1
- [#137838](https://github.com/NousResearch/hermes-agent/issues/137838) — macOS gateway repeatedly exits 75 after missed liveness probes; root cause unconfirmed. **No fix PR shown.**
- [#138039](https://github.com/NousResearch/hermes-agent/issues/138039) — over-cap checkpoint store flattens every project to one snapshot, ignoring `max_snapshots`, causing silent `/rollback` loss. **No fix PR shown.**

### P2
- [#131859](https://github.com/NousResearch/hermes-agent/issues/131859) — Cannot open a PR via API: `CreatePullRequest` permission error. **Blocked; no fix PR shown.**
- [#106592](https://github.com/NousResearch/hermes-agent/issues/106592) — Remote Desktop update reports failure and leaves stale update state after successful systemd restart. **No fix PR shown.**
- [#138044](https://github.com/NousResearch/hermes-agent/issues/138044) — Multiplexed gateway ignores Managed Scope `terminal.*`; commands run on the host. **Security-boundary issue; no fix PR shown.**
- [#138105](https://github.com/NousResearch/hermes-agent/issues/138105) — Multiplex cron ticker does O(profiles) config re-parses and probes every 60s, starving Discord heartbeat. **Partial fix PR:** [#138122](https://github.com/NousResearch/hermes-agent/pull/138122).
- [#138115](https://github.com/NousResearch/hermes-agent/issues/138115) — Subdirectory `AGENTS.md` hints miss container-absolute `/workspace/...` paths with Docker backend. **Fix PRs:** [#138120](https://github.com/NousResearch/hermes-agent/pull/138120), [#138124](https://github.com/NousResearch/hermes-agent/pull/138124).
- [#134394](https://github.com/NousResearch/hermes-agent/issues/134394) — Dependency pins carry open security advisories: PyJWT, httpx2, urllib3, oauthlib. **No fix PR shown.**
- [#138088](https://github.com/NousResearch/hermes-agent/issues/138088) — Remote folder picker starts outside the managed files root. **No fix PR shown.**
- [#138047](https://github.com/NousResearch/hermes-agent/issues/138047) — Desktop first prompt in a new chat can vanish silently and never persist. **Needs repro; no fix PR shown.**
- [#137944](https://github.com/NousResearch/hermes-agent/issues/137944) — `edge-tts==7.2.7` exact pin returns 403 after Microsoft `Sec-MS-GEC` rotation; 7.3.0 fixes. **No fix PR shown.**
- [#118536](https://github.com/NousResearch/hermes-agent/issues/118536) — Cron provider-unreachable failures never retry. **Fix PR:** [#118548](https://github.com/NousResearch/hermes-agent/pull/118548).
- [#129450](https://github.com/NousResearch/hermes-agent/issues/129450) — Delegation batch reports “Every task failed” when every task succeeded. **No fix PR shown.**
- [#133421](https://github.com/NousResearch/hermes-agent/issues/133421) — Desktop browser session guard rejects the genuinely focused chat tile. **No fix PR shown.**
- [#138108](https://github.com/NousResearch/hermes-agent/issues/138108) — Matrix messages from eM Client fail to decrypt; removing Base64 padding fixes repro. **No fix PR shown.**
- [#138094](https://github.com/NousResearch/hermes-agent/issues/138094) — Dashboard Plugins page installs agent plugins into the dashboard’s own profile, not the selected one. **No fix PR shown.**
- [#138090](https://github.com/NousResearch/hermes-agent/issues/138090) — `read_file` reports “File not found” for files it cannot access due to permissions/sandbox. **No fix PR shown.**
- [#122182](https://github.com/NousResearch/hermes-agent/issues/122182) — Desktop update fails with HTTP 422 on local-only parked branch. **No fix PR shown.**
- [#138053](https://github.com/NousResearch/hermes-agent/issues/138053) — `is_local_endpoint()` treats Kubernetes `*.svc.cluster.local` as local. **No fix PR shown.**

### P3/P4 notable
- [#68536](https://github.com/NousResearch/hermes-agent/issues/68536) — `/model` picker shows unavailable Shutdown/Retiring models.
- [#103355](https://github.com/NousResearch/hermes-agent/issues/103355) — Compression attempt budget never re-arms under plugin context engine. **Fix PR:** [#103373](https://github.com/NousResearch/hermes-agent/pull/103373).
- [#138052](https://github.com/NousResearch/hermes-agent/issues/138052) — OpenCode relay message-less 400 envelope aborts turns and kills auto-titles.
- [#127856](https://github.com/NousResearch/hermes-agent/issues/127856) — Linux launcher `StartupWMClass` mismatch creates separate dock entry.
- [#63681](https://github.com/NousResearch/hermes-agent/issues/63681) — Provider namespace lost during ACP session persist; restore fails for named custom providers.

## 6. Feature Requests & Roadmap Signals
- [#5320](https://github.com/NousResearch/hermes-agent/issues/5320) — Raise/auto-scale `memory_char_limit` and surface usage pressure. Long-standing P3 feature with 3 👍.
- [#80222](https://github.com/NousResearch/hermes-agent/issues/80222) — Per-call `model` and `reasoning_effort` overrides on `delegate_task`. P2 `needs-decision`.
- [#133212](https://github.com/NousResearch/hermes-agent/issues/133212) — `subagent_lifecycle v1.1` for plugin orchestrators: steer, host-isolation transport, effort/route/follow-up. P3 `needs-decision`.
- [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) — Tracking issue for one gateway owning every local session. Architectural direction, not a single feature.

**Likely next-version candidates from active PRs:** voice-only auto replies ([#138126](https://github.com/NousResearch/hermes-agent/pull/138126)), public speech streaming for Responses ([#138127](https://github.com/NousResearch/hermes-agent/pull/138127)), Codex quota/reset fixes ([#138128](https://github.com/NousResearch/hermes-agent/pull/138128), [#138125](https://github.com/NousResearch/hermes-agent/pull/138125)), gateway drain-wake refusal ([#137553](https://github.com/NousResearch/hermes-agent/pull/137553)), cron retry/perf fixes ([#118548](https://github.com/NousResearch/hermes-agent/pull/118548), [#138122](https://github.com/NousResearch/hermes-agent/pull/138122)), compression re-arm ([#103373](https://github.com/NousResearch/hermes-agent/pull/103373)), Docker path hint fixes ([#138120](https://github.com/NousResearch/hermes-agent/pull/138120), [#138124](https://github.com/NousResearch/hermes-agent/pull/138124)), and `.env` secret redaction ([#110907](https://github.com/NousResearch/hermes-agent/pull/110907)). Memory-limit and delegation-override changes may need maintainer decisions before landing.

## 7. User Feedback Summary
**Pain points:**
- Contributors are blocked from opening PRs via API due to a permission error ([#131859](https://github.com/NousResearch/hermes-agent/issues/131859)).
- Long-running sessions hit memory limits too quickly ([#5320](https://github.com/NousResearch/hermes-agent/issues/5320)).
- Delegation is too inflexible and can report false total failure ([#80222](https://github.com/NousResearch/hermes-agent/issues/80222), [#129450](https://github.com/NousResearch/hermes-agent/issues/129450)).
- Desktop/gateway reliability remains a concern: false update failures, silent prompt loss, and gateway exits ([#106592](https://github.com/NousResearch/hermes-agent/issues/106592), [#138047](https://github.com/NousResearch/hermes-agent/issues/138047), [#137838](https://github.com/NousResearch/hermes-agent/issues/137838)).
- Docker/multiplex users face path-resolution and security-boundary issues ([#138115](https://github.com/NousResearch/hermes-agent/issues/138115), [#138044](https://github.com/NousResearch/hermes-agent/issues/138044), [#138088](https://github.com/NousResearch/hermes-agent/issues/138088)).
- TTS users are blocked by an outdated `edge-tts` pin ([#137944](https://github.com/NousResearch/hermes-agent/issues/137944)).
- Security-conscious users flagged dependency advisories still present in pinned packages ([#134394](https://github.com/NousResearch/hermes-agent/issues/134394)).

**Satisfaction signals:** closed issues [#62182](https://github.com/NousResearch/hermes-agent/issues/62182) and [#77072](https://github.com/NousResearch/hermes-agent/issues/77072) show fixes are landing. **Dissatisfaction signals:** P1 stability and data-loss issues remain open, merge throughput is low relative to the open PR backlog, and the blocked auth issue is disrupting contributor flow.

## 8. Backlog Watch
Important items that appear to need maintainer attention:
- [#131859](https://github.com/NousResearch/hermes-agent/issues/131859) — blocked auth/API issue with 38 comments; high contributor impact.
- [#5320](https://github.com/NousResearch/hermes-agent/issues/5320) — open since 2026-04-05, P3, 13 comments, 3 👍.
- [#80222](https://github.com/NousResearch/hermes-agent/issues/80222) — open since 2026-08-06, P2 `needs-decision`, 8 comments.
- [#68536](https://github.com/NousResearch/hermes-agent/issues/68536) — open since 2026-07-21, P3, 6 comments.
- [#63681](https://github.com/NousResearch/hermes-agent/issues/63681) — open since 2026-07-13, P4, 5 comments.
- [#134394](https://github.com/NousResearch/hermes-agent/issues/134394) — P2 security advisory pin issue, open since 2026-10-07.
- [#133212](https://github.com/NousResearch/hermes-agent/issues/133212) — P3 `needs-decision`, plugin orchestration API, open since 2026-10-05.
- [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) — unified gateway tracking issue, open since 2026-10-08.
- Long-open PRs needing review/merge: [#95691](https://github.com/NousResearch/hermes-agent/pull/95691) since 2026-08-26, [#114655](https://github.com/NousResearch/hermes-agent/pull/114655) since 2026-09-18, [#110907](https://github.com/NousResearch/hermes-agent/pull/110907) since 2026-09-14, [#99960](https://github.com/NousResearch/hermes-agent/pull/99960) since 2026-09-01, and [#103373](https://github.com/NousResearch/hermes-agent/pull/103373) since 2026-09-05.

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw Project Digest — 2026-10-14

## Today’s Overview
IronClaw showed low activity in the last 24 hours: 1 open issue, 1 open PR, 0 closed issues, 0 merged/closed PRs, and 0 new releases. The substantive item is a proposal for an optional hosted-MCP Plazmi extension using existing declarative auth and egress surfaces, rather than adding vendor logic to the host. The only PR is a Dependabot dependency bump for `brace-expansion` in an internal docs/architecture-video package. Overall project health appears stable but quiet, with no reported bugs or regressions and no shipped code changes today.

## Releases
No new releases were published in the last 24 hours. There are no breaking changes, migration notes, or release-specific updates to report.

## Project Progress
No merged or closed PRs and no closed issues were recorded today, so no features advanced or fixes landed in the provided window. The only open PR is maintenance-related: [PR #8137](https://github.com/nearai/ironclaw/pull/8137) bumps `brace-expansion` from 1.1.18 to 1.1.21 in `/docs/internal/architecture-video`. This remains pending review/merge and has not yet produced a shipped change.

## Community Hot Topics
No issue or PR had comments or reactions in the provided data, so there are no high-engagement threads today.

- [Issue #8136](https://github.com/nearai/ironclaw/issues/8136) — *Proposal: optional hosted-MCP Plazmi extension using declarative auth and egress* — 0 comments, 0 👍. This is the most substantive discussion item. The underlying need is extensibility without host-level vendor coupling: the author wants an optional Plazmi companion installed separately as an extension, reusing existing hosted-MCP metadata, API-key auth, and egress declarative surfaces.
- [PR #8137](https://github.com/nearai/ironclaw/pull/8137) — *chore(deps): bump brace-expansion in /docs/internal/architecture-video* — dependency-maintenance activity with no reported comments/reactions. The need here is routine dependency hygiene, not a feature or bug discussion.

## Bugs & Stability
No bugs, crashes, or regressions were reported in the provided 24-hour data. There are no bug-fix PRs to track. [PR #8137](https://github.com/nearai/ironclaw/pull/8137) is a dependency bump and may support stability/security hygiene, but the provided data does not link it to a specific confirmed bug or advisory.

## Feature Requests & Roadmap Signals
The main feature request is [Issue #8136](https://github.com/nearai/ironclaw/issues/8136): an optional hosted-MCP Plazmi extension using declarative auth and egress. This signals demand for:
- Clean extension boundaries that avoid vendor logic in the IronClaw host.
- Optional, separately installed companion integrations.
- Reuse of declarative extension surfaces for hosted-MCP metadata, API-key auth, and egress.

Prediction: this is currently a proposal with 0 comments and no maintainer response, so it is unlikely to land in the immediate next version unless triaged and accepted quickly. If adopted, it would more likely appear as an optional extension rather than core host functionality.

## User Feedback Summary
The only direct user/builder feedback today is from the author of [Issue #8136](https://github.com/nearai/ironclaw/issues/8136). The stated pain point is a preference against adding vendor-specific logic to the host. The use case is a separately installed Plazmi companion that leverages existing declarative hosted-MCP, auth, and egress seams. There is no explicit satisfaction or dissatisfaction metric in the data, but the proposal is constructive and aligned with extensibility rather than reporting a defect.

## Backlog Watch
Both current items are newly updated and awaiting maintainer attention:
- [Issue #8136](https://github.com/nearai/ironclaw/issues/8136) — open proposal, created/updated 2026-10-13, 0 comments. Needs maintainer triage or design feedback.
- [PR #8137](https://github.com/nearai/ironclaw/pull/8137) — open Dependabot dependency bump, awaiting review/merge.

No older long-unanswered issues or PRs are present in the provided dataset, so backlog risk today is limited to these two items remaining unattended.

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw Project Digest — 2026-10-14

## 1. Today's Overview
QwenPaw activity remains high: 20 issues were updated in the last 24h (16 open/active, 4 closed) and 42 PRs were updated (22 open, 20 merged/closed), with no new release. The queue is maintenance-heavy around QwenPaw 2.2.x, especially console-chat reliability, media/base64 history bloat, plugin loading/provenance, Windows file locking, and deployment configurability. Maintainers and contributors are closing fixes for timeouts, media filtering, SQLite persistence, console polling, and skill-path sanitization, while large feature PRs such as experimental Code Mode and plugin hot reload remain open. Overall project health looks active but stability-sensitive: users are filing detailed, source-level bug reports, and several critical issues still lack visible fix PRs.

## 2. Releases
No new releases in the last 24h.

## 3. Project Progress
Closed/merged PRs in the visible sample advanced several areas:

- **[#8128](https://github.com/agentscope-ai/QwenPaw/pull/8128) — `feat(skill): Move hub to plugins`**: makes Hubs installable/removable through the plugin system and adds `register_market_provider()` for custom marketplaces.
- **[#8217](https://github.com/agentscope-ai/QwenPaw/pull/8217) — `feat(console): establish UIUX foundation`**: adds route metadata, page shell, push-polling changes, and lazy-import diagnostics.
- **[#8190](https://github.com/agentscope-ai/QwenPaw/pull/8190) — `fix(agents): resolve model capabilities before filtering history media`**: prevents text-only models from receiving images from history.
- **[#8098](https://github.com/agentscope-ai/QwenPaw/pull/8098) — `fix(agents): return a result for foreground chat timeouts`**: addresses the foreground timeout behavior reported in [#7981](https://github.com/agentscope-ai/QwenPaw/issues/7981).
- **[#7964](https://github.com/agentscope-ai/QwenPaw/pull/7964) — `fix(observability): Langfuse tool observation never records tool output`**: fixes missing tool output in Langfuse spans.
- **[#8065](https://github.com/agentscope-ai/QwenPaw/pull/8065) — `fix(skills): sanitize skill_name before building staging paths`**: hardens skill staging against path traversal.
- **[#8212](https://github.com/agentscope-ai/QwenPaw/pull/8212) — `fix: finish tool streams without yielding during GeneratorExit`**: addresses async-generator finalization errors.
- **[#6459](https://github.com/agentscope-ai/QwenPaw/pull/6459) — `fix(history): harden SQLite persistence, backup, and restore`**: improves history write-through and SQLite locking behavior.
- **[#8221](https://github.com/agentscope-ai/QwenPaw/pull/8221) — `fix(console): bound background polling and unblock model picker`**: limits poll accumulation and unblocks the model picker.
- **[#8220](https://github.com/agentscope-ai/QwenPaw/pull/8220) — `chore(deps): bumping version of agentscope to 2.0.10`**: dependency update.

Closed issues today included **[#8015](https://github.com/agentscope-ai/QwenPaw/issues/8015)** (custom Skill/Plugin marketplace sources), **[#8088](https://github.com/agentscope-ai/QwenPaw/issues/8088)** (image routing hangs in a Bash+PIL loop), **[#7981](https://github.com/agentscope-ai/QwenPaw/issues/7981)** (foreground timeout handling), and **[#8186](https://github.com/agentscope-ai/QwenPaw/issues/8186)** (message-navigation jump overriding manual scrolling).

## 4. Community Hot Topics
The most-discussed issues in the snapshot cluster around reliability, extensibility, and deployment:

- **[#8071](https://github.com/agentscope-ai/QwenPaw/issues/8071) — Plugin-facing theme extension point** (3 comments, open): plugin authors want semantic token overrides, not just `colorPrimary`. This signals demand for a deeper plugin UI API.
- **[#8172](https://github.com/agentscope-ai/QwenPaw/issues/8172) — Console chat intermittently silently completes empty** (3 comments, open): a high-trust-impact bug where a turn ends in about 1 second, output is empty, and the model is never called.
- **[#8015](https://github.com/agentscope-ai/QwenPaw/issues/8015) — Configurable custom Skill/Plugin marketplace sources** (2 comments, closed): enterprise/air-gapped users need self-hosted or offline marketplace mirrors.
- **[#7056](https://github.com/agentscope-ai/QwenPaw/issues/7056) — Background Task Callback / Notification Mechanism** (2 comments, open since August): users want push-based task completion instead of polling `check_agent_task`.
- **[#8171](https://github.com/agentscope-ai/QwenPaw/issues/8171) — `send_file_to_user` audio file permanently wedges the session** (2 comments, open): LM Studio returns a 400 for audio, and the capability classifier does not recover.
- **[#8206](https://github.com/agentscope-ai/QwenPaw/issues/8206) — Windows project deletion always fails once `creator.log` is open** (2 comments, open): Windows file-locking breaks `qwenpaw-creator` project deletion.
- **[#8126](https://github.com/agentscope-ai/QwenPaw/issues/8126) — Skill-pool download should be cancellable with progress** (2 comments, open): large skill downloads still hit the 30s client timeout.

Active PRs worth watching include **[#8224](https://github.com/agentscope-ai/QwenPaw/pull/8224)** (experimental Python Code Mode), **[#7565](https://github.com/agentscope-ai/QwenPaw/pull/7565)** (clean plugin unload and rollback-safe hot reload), **[#8231](https://github.com/agentscope-ai/QwenPaw/pull/8231)** (market install provenance for “Update All”), and **[#8229](https://github.com/agentscope-ai/QwenPaw/pull/8229)** (truncate oversized tool outputs in chat history).

Underlying needs: users want dependable long-running sessions, first-class plugin extensibility, offline/enterprise deployment support, better async-task UX, and robust media handling across providers.

## 5. Bugs & Stability
Ranked by severity based on reported impact:

1. **[#8172](https://github.com/agentscope-ai/QwenPaw/issues/8172) — Console chat silent empty completion** (open): high severity; breaks user trust in console chat. No fix PR shown in the snapshot.
2. **[#8171](https://github.com/agentscope-ai/QwenPaw/issues/8171) — Audio file permanently wedges session** (open): high severity; LM Studio 400 error is not handled by fallback/classifier. Related **[#7654](https://github.com/agentscope-ai/QwenPaw/pull/7654)** remains open.
3. **[#8228](https://github.com/agentscope-ai/QwenPaw/issues/8228) — Chat history bloat from base64 tool outputs** (open): high severity; 10 MB+ histories hit the 30s console timeout. Fix PR **[#8229](https://github.com/agentscope-ai/QwenPaw/pull/8229)** is open.
4. **[#8206](https://github.com/agentscope-ai/QwenPaw/issues/8206) — Windows project deletion fails with WinError 5** (open): high for Windows users; open `creator.log` blocks directory rename/deletion.
5. **[#8225](https://github.com/agentscope-ai/QwenPaw/issues/8225) — Config save hot reload cancels running cron tasks silently** (open): high; background jobs are marked `cancelled` with no warning.
6. **[#8222](https://github.com/agentscope-ai/QwenPaw/issues/8222) — `qwenpaw acp` never loads installed plugins** (open): high for ACP users; plugin tools are missing and cross-process callbacks cannot wake sessions.
7. **[#8213](https://github.com/agentscope-ai/QwenPaw/issues/8213) — Hub restart leaves desired-running Local runtimes stopped** (open): high; cron jobs silently miss scheduled executions.
8. **[#8179](https://github.com/agentscope-ai/QwenPaw/issues/8179) — Installing skill from GitHub corrupts binary files** (open): high; non-UTF-8 bytes are replaced with U+FFFD, breaking images/fonts.
9. **[#8218](https://github.com/agentscope-ai/QwenPaw/issues/8218) — Default agent cannot modify model; deprecated model error** (open): high for configuration usability.
10. **[#8177](https://github.com/agentscope-ai/QwenPaw/issues/8177) — Console basename hardcoded to `/console`** (open): medium; blocks reverse-proxy subpath deployments.

Recently closed stability fixes include **[#8088](https://github.com/agentscope-ai/QwenPaw/issues/8088)**, **[#7981](https://github.com/agentscope-ai/QwenPaw/issues/7981)**, and **[#8186](https://github.com/agentscope-ai/QwenPaw/issues/8186)**.

## 6. Feature Requests & Roadmap Signals
Likely roadmap signals from recent requests:

- **[#8071](https://github.com/agentscope-ai/QwenPaw/issues/8071)** — plugin-facing theme extension point with semantic token overrides.
- **[#8015](https://github.com/agentscope-ai/QwenPaw/issues/8015)** / **[#8128](https://github.com/agentscope-ai/QwenPaw/pull/8128)** — customizable Skill/Plugin marketplace providers, including self-hosted and offline mirrors.
- **[#7056](https://github.com/agentscope-ai/QwenPaw/issues/7056)** — background task callback/notification mechanism for async agent workflows.
- **[#8126](https://github.com/agentscope-ai/QwenPaw/issues/8126)** — cancellable skill-pool downloads with progress reporting.
- **[#8223](https://github.com/agentscope-ai/QwenPaw/issues/8223)** / **[#8230](https://github.com/agentscope-ai/QwenPaw/pull/8230)** — optional host navigation policy shared by sidebar and settings.
- **[#8211](https://github.com/agentscope-ai/QwenPaw/issues/8211)** — i18n for the protected execution contract prompt.
- **[#8177](https://github.com/agentscope-ai/QwenPaw/issues/8177)** — configurable console basename for reverse-proxy subpath deployments.
- **[#8224](https://github.com/agentscope-ai/QwenPaw/pull/8224)** — experimental Python Code Mode for tool orchestration.
- **[#8205](https://github.com/agentscope-ai/QwenPaw/pull/8205)** — TypeSafe provider and decision-model types.

Prediction: the next QwenPaw release is likely to focus on plugin/marketplace extensibility, console reliability and UX, media/history handling, and possibly an experimental Code Mode path, rather than a major version jump.

## 7. User Feedback Summary
Users are actively testing 2.2.0, 2.2.1, and 2.2.2 beta builds across Docker/NAS, Windows Server, desktop, and ACP environments. The strongest dissatisfaction is around silent failures: empty console completions, session wedging after audio delivery, no cron-cancellation warning, and missing replies after timeouts. Users also report data-integrity concerns such as base64 bloat in chat history and binary corruption during GitHub skill installs. Deployment friction is visible in reverse-proxy subpath requests, air-gapped marketplace needs, and Windows file-locking failures. On the positive side, users are providing detailed source-level reports and maintainers are closing concrete fixes for timeouts, media filtering, SQLite history persistence, and console polling. Overall sentiment: engaged community, but reliability and deployment polish remain the main sources of frustration.

## 8. Backlog Watch
Important open items needing maintainer attention:

- **[#7056](https://github.com/agentscope-ai/QwenPaw/issues/7056) — Background Task Callback / Notification Mechanism** (open since 2026-08-15): long-standing async-workflow request with only 2 comments.
- **[#7565](https://github.com/agentscope-ai/QwenPaw/pull/7565) — `feat(plugins): add clean unload and rollback-safe hot reload`** (open since 2026-09-04, size/XXXL): critical for plugin ecosystem stability and update safety.
- **[#8071](https://github.com/agentscope-ai/QwenPaw/issues/8071) — Plugin-facing theme extension point** (open since 2026-10-01): plugin API maturity.
- **[#8126](https://github.com/agentscope-ai/QwenPaw/issues/8126) — Cancellable skill-pool download** (open since 2026-10-08): UX and timeout relief.
- **[#8179](https://github.com/agentscope-ai/QwenPaw/issues/8179) — GitHub skill install corrupts binary files** (open since 2026-10-11): data-integrity issue.
- **[#8177](https://github.com/agentscope-ai/QwenPaw/issues/8177) — Configurable console basename** (open since 2026-10-11): common self-hosting blocker.
- **[#8222](https://github.com/agentscope-ai/QwenPaw/issues/8222) — ACP does not load installed plugins** (open since 2026-10-13): plugin/ACP integration gap.
- **[#8213](https://github.com/agentscope-ai/QwenPaw/issues/8213) — Hub restart does not restore Local runtimes** (open since 2026-10-13): background reliability.
- **[#8225](https://github.com/agentscope-ai/QwenPaw/issues/8225) — Hot reload cancels cron tasks** (open since 2026-10-13): silent job loss.

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw Project Digest — 2026-10-14

## 1. Today’s Overview

ZeroClaw saw high activity in the last 24h: 24 issues were updated, all still open, and 50 PRs were updated, with 45 open and 5 merged/closed. No new releases were published. The issue queue is not shrinking today because no issues were closed, while the PR pipeline remains active but heavily stacked and review-gated. The strongest signals are around release hardening for v0.8.6, gateway/runtime work for v0.9.0, and a cluster of P1/P2 bugs affecting cost accounting, session persistence, prompt caching, and channel reliability. Overall: active and contributor-rich, but with visible stability and release-readiness pressure.

## 2. Releases

No new releases were published in the last 24h. The latest release data is empty, so there are no release notes, breaking changes, or migration notes to report.

## 3. Project Progress

- PR activity remains the main forward motion: 50 PRs updated, 45 still open, 5 merged/closed.
- The supplied top-20 PR sample shows one closed PR: [#11435](https://github.com/zeroclaw-labs/zeroclaw/pull/11435) `fix(sop): settle rejected headless runs and retain reload retries`, which addresses SOP headless run lifecycle and reload behavior.
- Several open PRs are advancing v0.8.6 / release-gate work:
  - [#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347) `feat(release): ship the WASM plugin host in supported release artifacts`
  - [#11304](https://github.com/zeroclaw-labs/zeroclaw/pull/11304) `fix(plugins): record socket and WebSocket egress denials`
  - [#11309](https://github.com/zeroclaw-labs/zeroclaw/pull/11309) `feat(quickstart): install and activate tool plugins from zeroclaw quickstart`
  - [#11228](https://github.com/zeroclaw-labs/zeroclaw/pull/11228) `fix(plugins): harden named TLS profile materialization`
- Runtime and provider correctness work is also active:
  - [#11676](https://github.com/zeroclaw-labs/zeroclaw/pull/11676) fixes SQLite session transcript `created_at` preservation.
  - [#11672](https://github.com/zeroclaw-labs/zeroclaw/pull/11672) fixes JPEG admission copying.
  - [#11647](https://github.com/zeroclaw-labs/zeroclaw/pull/11647) decouples vision support from system merge.
  - [#11651](https://github.com/zeroclaw-labs/zeroclaw/pull/11651) handles Telegram 429 `retry_after` with a shared flood gate.
  - [#11577](https://github.com/zeroclaw-labs/zeroclaw/pull/11577) adds operator-declared model routes for subagents.
- No issues were closed today, so the open issue backlog did not decrease in the reported window.

## 4. Community Hot Topics

The most active discussions are comment-driven; all listed issues show 0 👍 reactions in the supplied data.

1. [#11420](https://github.com/zeroclaw-labs/zeroclaw/issues/11420) — 8 comments  
   `SQLite session backend rewrites created_at of every message on each turn`  
   This is a P1 data-integrity issue. Users need reliable per-message timestamps for session audit, replay, and observability. A fix PR exists: [#11676](https://github.com/zeroclaw-labs/zeroclaw/pull/11676).

2. [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) — 6 comments  
   `[Tracker]: Runtime and gateway delivery - v0.8.6 and v0.9.0`  
   This is the roadmap source of truth for runtime Phase 2 and gateway separation Phase 3. It signals that maintainers are coordinating a multi-release architecture push.

3. [#11204](https://github.com/zeroclaw-labs/zeroclaw/issues/11204) — 5 comments  
   `OpenRouter spend shows $0.00 and all tokens classified "free tok"`  
   Cost telemetry is not trusted for OpenRouter users. This is a P1 observability/runtime issue and likely a release blocker for cost-sensitive deployments.

4. [#10573](https://github.com/zeroclaw-labs/zeroclaw/issues/10573) — 5 comments  
   `Bind gateway pairing tokens to roster users for scoped remote principals`  
   Accepted security/identity feature. It reflects demand for scoped remote access and tighter gateway principal control.

5. [#11613](https://github.com/zeroclaw-labs/zeroclaw/issues/11613) — 4 comments  
   `Cost ledger drops the provider's total_tokens`  
   Related to hidden reasoning tokens and provider usage accounting. Users need provider-reported totals preserved rather than recomputed incorrectly.

Underlying needs: trustworthy cost/billing data, durable session history, gateway identity/security, and predictable runtime/gateway release sequencing.

## 5. Bugs & Stability

Ranked by severity and impact, based on the supplied issue/PR data.

### S1 / Blocking

- [#11179](https://github.com/zeroclaw-labs/zeroclaw/issues/11179) — `Flaky: sop::engine pending_park_retry_respects_pending_pool_cap fails when the test crosses a second boundary`  
  S1, workflow blocked. The flaky test turns `CI Required Gate` red for unrelated PRs. No fix PR is visible in the top PR sample.

### P1 / High Severity

- [#11420](https://github.com/zeroclaw-labs/zeroclaw/issues/11420) — SQLite session backend rewrites `created_at` on every turn. Fix PR: [#11676](https://github.com/zeroclaw-labs/zeroclaw/pull/11676).
- [#11204](https://github.com/zeroclaw-labs/zeroclaw/issues/11204) — OpenRouter spend reports $0.00 and all tokens as free. No fix PR visible.
- [#10777](https://github.com/zeroclaw-labs/zeroclaw/issues/10777) — Anthropic thinking/effort config flips between turns and rewrites cached history. Status: accepted, needs repro. No fix PR visible.
- [#11606](https://github.com/zeroclaw-labs/zeroclaw/issues/11606) — `model_routing_config upsert_agent` rewrites the entire config and can fabricate/drop fields. Status: in-progress. No fix PR visible in sample.

### S2 / Degraded Behavior

- [#11696](https://github.com/zeroclaw-labs/zeroclaw/issues/11696) — system prompt rebuilt from disk every turn, busting prompt cache for live sessions. Cost/performance regression.
- [#11711](https://github.com/zeroclaw-labs/zeroclaw/issues/11711) — trimming a long conversation permanently deletes older turns from the session store. Data-loss risk.
- [#11706](https://github.com/zeroclaw-labs/zeroclaw/issues/11706) — `max_image_turns` never releases the vision route, pinning the provider to `vision_model_provider`.
- [#11708](https://github.com/zeroclaw-labs/zeroclaw/issues/11708) — `model_routing_config` omits action-specific required arguments from its tool schema. Status: in-progress.
- [#11613](https://github.com/zeroclaw-labs/zeroclaw/issues/11613) — cost ledger drops provider `total_tokens`, under-counting reasoning tokens.
- [#10950](https://github.com/zeroclaw-labs/zeroclaw/issues/10950) — `cost.warn_at_percent` warnings are ignored by the runtime.
- [#11713](https://github.com/zeroclaw-labs/zeroclaw/issues/11713) — Telegram channel ignores `edited_message` updates, so mention edits are never seen.
- [#11709](https://github.com/zeroclaw-labs/zeroclaw/issues/11709) — upgraded WebSockets outlive a gateway reload and keep serving a retired instance. Related PR [#11718](https://github.com/zeroclaw-labs/zeroclaw/pull/11718) addresses streamed webhook cancellation, but not the full retired-instance socket issue.

### S3 / Minor or Confusing

- [#11694](https://github.com/zeroclaw-labs/zeroclaw/issues/11694) — ZeroCode Cost tab mixes UTC and local-day totals without labeling.
- [#11654](https://github.com/zeroclaw-labs/zeroclaw/issues/11654) — JPEG admission copies input and parses header metadata before checking decode-memory cap. Fix PR: [#11672](https://github.com/zeroclaw-labs/zeroclaw/pull/11672).
- [#11675](https://github.com/zeroclaw-labs/zeroclaw/issues/11675) — ACP session store re-stamps `created_at` of `acp_messages` and `acp_tool_calls` rows.
- [#11710](https://github.com/zeroclaw-labs/zeroclaw/issues/11710) — truncated workspace file tells the agent to read it while prompt header forbids reading it.
- [#11703](https://github.com/zeroclaw-labs/zeroclaw/issues/11703) — deferred tool activation changes the system prompt and busts prompt cache.
- [#11704](https://github.com/zeroclaw-labs/zeroclaw/issues/11704) — channel and `agent::run` prompts carry the date, causing midnight prompt-cache misses.

## 6. Feature Requests & Roadmap Signals

- [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) remains the main roadmap tracker for v0.8.6 runtime Phase 2 and v0.9.0 gateway separation. Expect continued release-gate work around runtime, gateway, and security deliverables.
- [#10573](https://github.com/zeroclaw-labs/zeroclaw/issues/10573) — binding gateway pairing tokens to roster users is accepted and likely targeted at v0.8.6/v0.9.0 security identity work.
- [#11207](https://github.com/zeroclaw-labs/zeroclaw/issues/11207) — request to document a bounded Tsubasa custom-provider setup. Low-risk docs request.
- [#10261](https://github.com/zeroclaw-labs/zeroclaw/issues/10261) — make the filesystem dispatch-loop delivery regression load-bearing. Small test/quality task.
- [#11705](https://github.com/zeroclaw-labs/zeroclaw/issues/11705) — proposal for a Plazmi example demonstrating per-agent MCP grants and bounded catch-up.

PRs likely shaping the next release:
- v0.8.6 plugin/release work: [#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347), [#11304](https://github.com/zeroclaw-labs/zeroclaw/pull/11304), [#11309](https://github.com/zeroclaw-labs/zeroclaw/pull/11309), [#11228](https://github.com/zeroclaw-labs/zeroclaw/pull/11228).
- Runtime/model routing: [#11577](https://github.com/zeroclaw-labs/zeroclaw/pull/11577), [#11174](https://github.com/zeroclaw-labs/zeroclaw/pull/11174).
- Config/UX: [#11419](https://github.com/zeroclaw-labs/zeroclaw/pull/11419), [#11719](https://github.com/zeroclaw-labs/zeroclaw/pull/11719), [#11721](https://github.com/zeroclaw-labs/zeroclaw/pull/11721), [#11717](https://github.com/zeroclaw-labs/zeroclaw/pull/11717).

Prediction: the next visible release is most likely v0.8.6, focused on WASM plugin host artifacts, plugin security, quickstart plugin activation, and runtime cleanup. v0.9.0 will likely follow with gateway separation, scoped pairing tokens, and broader runtime/gateway architecture changes.

## 7. User Feedback Summary

The dominant user pain points are data fidelity and cost transparency:

- Cost tracking is repeatedly reported as unreliable: OpenRouter spend shows $0.00, `total_tokens` is dropped, `cost.warn_at_percent` is ignored, and ZeroCode mixes UTC/local-day totals.
- Session persistence is a major concern: SQLite and ACP transcript rewrites corrupt `created_at`, trimming deletes older turns, and per-message timeline data is lost.
- Prompt-cache invalidation is causing unnecessary cost: system prompts are rebuilt from disk, deferred tools change prompts, dates in prompts cause midnight cache misses, and Anthropic thinking config can rewrite cached history.
- Configuration tooling can be destructive: `model_routing_config` upsert actions have rewritten whole configs and omitted required arguments from schemas.
- Channel reliability issues remain: Telegram ignores edited messages and mishandles 429 rate limits; gateway reloads leave old sockets serving retired instances.
- Users are asking for more bounded, documented setups, especially custom providers, MCP grants, local Whisper models, and plugin installation flows.

Satisfaction signals: many reports are detailed, reproducible, and have been accepted or marked in-progress, suggesting an engaged advanced user base. Dissatisfaction is concentrated where observability, persistence, and cost data cannot be trusted.

## 8. Backlog Watch

Long-standing or high-importance items needing maintainer attention:

- [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) — created 2026-06-09. Runtime/gateway tracker for v0.8.6 and v0.9.0. High-risk roadmap coordination item.
- [#10573](https://github.com/zeroclaw-labs/zeroclaw/issues/10573) — created 2026-09-03. Accepted security/identity feature for scoped gateway pairing tokens.
- [#11204](https://github.com/zeroclaw-labs/zeroclaw/issues/11204) — created 2026-09-27. P1 OpenRouter cost accounting bug with no visible fix PR.
- [#10777](https://github.com/zeroclaw-labs/zeroclaw/issues/10777) — created 2026-09-11. P1 Anthropic thinking/cache issue, accepted but needs repro.
- [#11420](https://github.com/zeroclaw-labs/zeroclaw/issues/11420) — created 2026-10-02. P1 session timestamp corruption; fix PR [#11676](https://github.com/zeroclaw-labs/zeroclaw/pull/11676) exists and should be prioritized.
- [#11179](https://github.com/zeroclaw-labs/zeroclaw/issues/11179) — created 2026-09-26. S1 flaky CI test blocking PR gates.
- [#10261](https://github.com/zeroclaw-labs/zeroclaw/issues/10261) — created 2026-08-22. Small but important regression-test quality task.
- PR backlog: [#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347) is stacked and release-gated; [#11304](https://github.com/zeroclaw-labs/zeroclaw/pull/11304) is blocked; [#11174](https://github.com/zeroclaw-labs/zeroclaw/pull/11174) needs author action; [#11228](https://github.com/zeroclaw-labs/zeroclaw/pull/11228) is high-risk security hardening. These need maintainer review sequencing to keep v0.8.6 on track.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*