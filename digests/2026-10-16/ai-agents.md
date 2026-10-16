# OpenClaw 生态日报 2026-10-16

> Issues: 500 | PRs: 500 | 覆盖项目: 5 个 | 生成时间: 2026-10-16 00:40 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw 项目深度报告

# OpenClaw 项目摘要 — 2026-10-16

## 1. 今日概览

OpenClaw 正处于高速发布与加固周期。过去 24 小时内，仓库有 **500 个 issue 更新**（393 个 open/active，107 个 closed）和 **500 个 PR 更新**（201 个 open，299 个 merged/closed），另有 **4 个新版本**，覆盖从 `2026.10.10-beta.1` 前沿版本到 `2026.9.33` extended-stable（LTS 等效）分支。当前主导工程主题是 **Gateway 性能与持久性**——beta 线明确将 SQLite 工作移出主线程，而 issue tracker 显示出持续聚集的 WAL/checkpoint、plugin-capture 和更新可靠性回归。总体项目健康度在吞吐量方面表现强劲（该窗口 PR 关闭率约 60%），但仍有大量未解决的 P0/P1 债务，集中在存储层稳定性和插件生命周期。

## 2. 版本发布

该窗口共发布四个版本：

### 🔷 v2026.10.10-beta.1（最新）
- **亮点：** 更快、更稳定的 Gateway。聊天轮次、会话列表、启动和空闲 Gateway 执行的 SQLite 与主线程工作大幅减少，更多数据库工作被移到 worker 线程，因此繁忙主机和大型 agent 集群能保持响应。（#167965, #168002, #168093, …）
- **破坏性变更：** 发布说明中未提及。
- **迁移说明：** 未提及；适用标准 beta 通道行为。

### 🔷 v2026.10.5-beta.4
- **亮点：** 更新更可靠——macOS 更新不再因别名临时目录而超时；systemd 用户安装会回收被遗弃的 runtime；pacman 和 FreeBSD 安装会交由包管理器处理；快照失败会在应用前暴露。
- **破坏性变更：** 未提及。

### 🔷 v2026.10.4（Stable）
- **亮点：** Gateway 在计算机睡眠和系统时钟跳变后能够存活，而不会因过期的所有权心跳而自行重启（#170790）。Gateway 关闭/重启不再在某些路径上挂起。*（提供的数据中说明已截断。）*
- **破坏性变更：** 未提及。

### 🟢 v2026.9.33（extended-stable / LTS）
- 仅面向 Gateway 的 extended-stable 版本，涵盖 2026 年 9 月末的 OpenClaw，外加 **关键安全更新、可靠性与性能修复**以及新模型支持。对于重视稳定性而非新特性的运维人员，推荐使用该分支。

**版本要点：** beta 列车正在快速推进（该窗口内有三个 beta/stable 版本），这与 #173504 和 #173523 等 PR 中正在进行的大规模 Gateway 重构一致。LTS 上的运维人员获得的是安全 backport，而不是功能更迭。

## 3. 项目进展

**过去 24 小时有 299 个 PR merged/closed**——合并节奏强劲。队列顶部的 PR 中可见值得关注的活跃工作：

- **[PR #173523](https://github.com/openclaw/openclaw/pull/173523)** — *refactor: simplify infra and plugin lifecycle bookkeeping*（steipete）。从生命周期路径中移除冗余的 admission、producer-owned fact copying 以及不受支持的 reentrant coordination。标记为 `merge-risk: 🚨 availability`、`security-sensitive-changed`。
- **[PR #173504](https://github.com/openclaw/openclaw/pull/173504)** — *refactor(gateway): remove reload debt and startup claims*。削减 Gateway 启动和配置重载中重叠的 reservation/proxy-owner/restart-debt 状态。
- **[PR #173579](https://github.com/openclaw/openclaw/pull/173579)** — *fix: resume interrupted tasks in their original session*。WebChat 的 “Resume” 现在会在同一会话中继续已保存的任务，保留身份、工作区、参与者和子任务。
- **[PR #173574](https://github.com/openclaw/openclaw/pull/173574)** — *perf(runtime): stop GC telemetry from waking idle workers*（已关闭）。与 v2026.10.10-beta.1 的 “稳定空闲 Gateway” 主题直接一致。
- **[PR #173548](https://github.com/openclaw/openclaw/pull/173548)** — *perf(subagents): release settled run text from memory*（已关闭）。堆分析发现注册表中保留了约 25 MiB 的任务文本——这是一项内存压力修复。
- **[PR #173266](https://github.com/openclaw/openclaw/pull/173266)** — *fix(control-ui): retain cached conversation titles on warm reload*。
- **[PR #173572](https://github.com/openclaw/openclaw/pull/173572)** — *fix(cron): ignore rate-limit cooldowns when Doctor records an automation account*。

**进阶主题：** Gateway 生命周期简化、subagent 内存回收、Control UI Solid 迁移性能清理，以及 CI 基础设施投入（通过 #173581/#173582 进行 Crabbox runner 配置）。

**值得关注的关闭项：** Issue #164113（非特权 LXC 中的 FICLONE EPERM，P0）、#121617（压缩后 “Already compacted” 误分类，P0）、#136262（openai-completions 流式文本重复，P1）和 #170332（实时 DB 变更后 beta 更新仍继续）均在该窗口关闭。

## 4. 社区热点话题

按过去 24 小时评论量排序：

1. **[Issue #143524](https://github.com/openclaw/openclaw/issues/143524) — 128 条评论。** *Agent SQLite WAL 在数天内增长到 1.4–2.8 GB，尽管设置了 `wal_autocheckpoint=1000`；阻塞 gateway 启动（Windows，2026.9.2/9.3）。* P0，`impact:message-loss`，`impact:crash-loop`。尽管评论数庞大，它仍处于 open 状态并带有 `clawsweeper:no-new-fix-pr`，表明尚未附加修复 PR。**底层需求：** 在长期存活的进程内 reader 下实现持久的 WAL checkpoint——这是下面另外两个 issue 中也出现的反复出现的存储架构问题。

2. **[Issue #160959](https://github.com/openclaw/openclaw/issues/160959) — 25 条评论。** *Gateway 在捕获大型外部插件时阻塞数分钟（2026.9.6 回归）。* P0，diamond-lobster 评级。**需求：** plugin capture 必须增量/异步，且不得阻塞 Gateway 事件循环。

3. **[Issue #148707](https://github.com/openclaw/openclaw/issues/148707) — 20 条评论。** *当第二次 run 挤掉进行中的 turn 时，回复因 “Reply operation has no active tool authority snapshot” 而丢失。* P1，`impact:session-state`，`impact:message-loss`。**需求：** 在 session-state 竞态下提供安全的 turn displacement / 消息丢失保证。

4. **[Issue #157989](https://github.com/openclaw/openclaw/issues/157989) — 18 条评论。** *插件源码捕获每条 CLI 命令重写约 1.1–1.4 GB，每次 Gateway 启动约 6.5 GB；严重 SSD 磨损。* P1。**需求：** 跨调用进行捕获去重与复用。

5. **[Issue #68596](https://github.com/openclaw/openclaw/issues/68596) — 17 条评论，8 个 👍（最高 reaction）。** *Feature Request：可配置的 streaming watchdog 超时阈值。* **需求：** 运行长推理模型（kimi-k2.5、DeepSeek-R1）的用户需要调整 30 秒 watchdog，目前它会产生虚假的 “backend may have dropped this run” 警告。

6. **[Issue #164294](https://github.com/openclaw/openclaw/issues/164294) — 17 条评论。** *Gateway state DB WAL 自动 checkpoint 永远无法完成（被进程内 reader 钉住）。* **需求：** 与 #143524 同属根因家族——进程内 reader 让 checkpoint 进展饥饿。与 v2026.10.10-beta.1 的 “将 DB 工作移到 worker 线程” 工作直接相关。

7. **[PR #173523](https://github.com/openclaw/openclaw/pull/173523)** 和 **[PR #173504](https://github.com/openclaw/openclaw/pull/173504)** — 按审查活动排名位于队列顶部的 PR，二者都是来自维护者账号（steipete）的 XL 规模 Gateway 重构，并被标记需进行 availability 和 security-boundary 审查。

**分析：** 评论密集的 issue 紧密围绕三个根因聚集：(a) 并发 reader 下的 SQLite WAL/checkpoint 生命周期，(b) plugin capture 成本（CPU、I/O、SSD 磨损），以及 (c) turn 竞态下的消息/session-state 丢失。这些正是当前 beta 线和 Gateway 重构瞄准的领域。

## 5. Bug 与稳定性

**P0 / 发布阻断级（open）：**

| 问题 | 影响 | 状态 |
|---|---|---|
| [#143524](https://github.com/openclaw/openclaw/issues/143524) — WAL 增长到 2.8 GB，阻塞启动 | 消息丢失、崩溃循环 | OPEN，无修复 PR，128 条评论 |
| [#160959](https://github.com/openclaw/openclaw/issues/160959) — Gateway 在 plugin capture 上阻塞数分钟 | 崩溃循环 | OPEN，需要维护者审查 |
| [#70903](https://github.com/openclaw/openclaw/issues/70903) — 基于文件的 provider cooldown 在计费恢复后仍阻塞用户数小时 | auth-provider、UX 发布阻断 | OPEN（stale），无修复 PR |
| [#152275](https://github.com/openclaw/openclaw/issues/152275) — 提交后插件激活失败导致 model catalog 和 reply dispatch 失效，直到重启 | 消息丢失、崩溃循环 | OPEN |
| [#171918](https://github.com/openclaw/openclaw/issues/171918) — Safari 27.0.1 间歇性拒绝 Control UI 入口图 | UX 发布阻断 | OPEN，有合成复现 |
| [#161921](https://github.com/openclaw/openclaw/issues/161921) — 在 `state-migrated-no-rollback` 之后，`doctor --fix` 和 `plugins update` 互相死锁 | UX 发布阻断 | OPEN |

**P1 严重级（open）：**

- [#148707](https://github.com/openclaw/openclaw/issues/148707) — 进行中的 turn 被挤掉时回复丢失。
- [#157989](https://github.com/openclaw/openclaw/issues/157989) — plugin capture 导致 SSD 磨损。
- [#159512](https://github.com/openclaw/openclaw/issues/159512) — 每次 subagent 完成都会使请求方 CLI 会话失效两次（claude-cli）。
- [#164799](https://github.com/openclaw/openclaw/issues/164799) — 版本变更后 agent 接纳缓慢；`readyz` 在重启前返回 503。
- [#81182](https://github.com/openclaw/openclaw/issues/81182) — overflow recovery 应截断 tool results，而不是等待约 900 秒的 auto-compaction 超时。
- [#101793](https://github.com/openclaw/openclaw/issues/101793) — Signal 上工具调用前的 assistant 文本被静默丢弃。
- [#77717](https://github.com/openclaw/openclaw/issues/77717) — Feishu channel bot 身份恢复竞态 → 永久断连。
- [#170956](https://github.com/openclaw/openclaw/issues/170956) — Windows 启动缓慢、插件关闭不干净、`schtasks` 权限错误。

**值得注意的 P2 稳定性事项：** [#172091](https://github.com/openclaw/openclaw/issues/172091)（worker SQLite 事务被永久钉在 open 状态，阻塞 shared-state checkpoint——提交于 2026-10-14，与 beta Gateway 工作直接相邻）、[#99659](https://github.com/openclaw/openclaw/issues/99659)（companion app 连接后 OOM kill）、[#81484](https://github.com/openclaw/openclaw/issues/81484)（Discord guild 回复回归，重复出站循环）、[#166133](https://github.com/openclaw/openclaw/issues/166133)（MCP 整数参数被强制转换为字符串）。

**窗口内关闭（稳定性胜利）：** [#164113](https://github.com/openclaw/openclaw/issues/164113)（非特权 LXC 中的 FICLONE EPERM）、[#121617](https://github.com/openclaw/openclaw/issues/121617)（压缩后 guard 误分类）、[#136262](https://github.com/openclaw/openclaw/issues/136262)（openai-completions 裸 `text_delta` 导致消息内容翻倍）、[#170332](https://github.com/openclaw/openclaw/issues/170332)（实时 DB 变更后 beta 更新仍继续）、[#77298](https://github.com/openclaw/openclaw/issues/77298)（cron `consecutiveErrors` 因重启中断而虚高）。

**修复 PR 状态：** 大多数 P0 issue 带有 `clawsweeper:no-new-fix-pr`——即**尚未附加修复 PR**。例外是 #164113 和 #121617，二者现已关闭。活跃的 Gateway 重构 PR（#173504、#173523）可能间接涵盖若干 WAL/生命周期修复，但并未明确关联为修复。

## 6. 功能请求与路线图信号

高信号功能请求，按可能接近路线图的程度分组：

**可能很快落地（与活跃 PR/重构工作一致）：**
- [#79902](https://github.com/openclaw/openclaw/issues/79902) — 在 database-first runtime 之上提供 companion 友好的 SQLite transcript/session 接缝。Control UI Solid 迁移和搜索能力重构（[PR #172681](https://github.com/openclaw/openclaw/pull/172681)）正朝这个方向推进。
- [#172091](https://github.com/openclaw/openclaw/issues/172091) / [#164294](https://github.com/openclaw/openclaw/issues/164294) — WAL/checkpoint 正确性。v2026.10.10-beta.1 的 “worker-thread DB” 推进是自然的载体。
- [#81182](https://github.com/openclaw/openclaw/issues/81182) — 在完整 auto-compaction 超时前截断 tool results。低风险、高 UX 价值；memory-core 已经是活跃 PR 目标（[#172681](https://github.com/openclaw/openclaw/pull/172681)）。

**下一周期可能的候选（范围清晰、工作量适中）：**
- [#68596](https://github.com/openclaw/openclaw/issues/68596)（8 👍）— 可配置的 streaming watchdog 超时。来自推理模型用户的强烈需求。
- [#67413](https://github.com/openclaw/openclaw/issues/67413)（5 👍）— 按 agent 配置 dreaming，以防止 cron 内存清理期间 OOM。
- [#67419](https://github.com/openclaw/openclaw/issues/67419) — 停止每轮重新注入 bootstrap 文件（浪费 20–30% token）。与 context-budget 问题配套。
- [#71142](https://github.com/openclaw/openclaw/issues/71142) — Control UI 可配置上传大小限制（当前硬编码为 5 MB）。
- [#71452](https://github.com/openclaw/openclaw/issues/71452) — 为 `message list` / `channel-list` 添加分页，而不是硬编码截断为 25 项。

**更长周期 / 架构级：**
- [#63990](https://github.com/openclaw/openclaw/issues/63990) — 多索引 embedding memory，具备模型感知 failover（不混合向量空间）。
- [#138245](https://github.com/openclaw/openclaw/issues/138245) — 输出 guard，在伪 tool-call 以纯文本形式发出并污染会话历史之前将其捕获。
- [#66252](https://github.com/openclaw/openclaw/issues/66252) — 面向多语言部署的按 agent TTS/STT 覆盖。
- [#103764](https://github.com/openclaw/openclaw/issues/103764) — Kotlin Multiplatform 共享移动端逻辑。
- [#63930](https://github.com/openclaw/openclaw/issues/63930) — Anthropic advisor beta 服务端工具支持。

**下一版本预测：** 鉴于 v2026.10.10-beta.1 的 Gateway 性能方向，预计接下来的 beta 将包括：(a) 与 worker-thread DB 工作绑定的进一步 WAL/checkpoint 修复，(b) plugin-capture 复用/去重工作，以及 (c) 增量完成的 Control UI Solid 迁移。streaming-watchdog 可配置性请求（#68596）具有最清晰的用户需求与工作量之比，是纳入的强候选。

## 7. 用户反馈总结

**满意信号：**
- 用户正在积极对版本进行压力测试，并提交详细、高质量的复现（diamond 和 lobster 评级的 issue），这反映出社区参与度高、技术成熟。
- 对提出可配置性的功能请求，reaction 偏向正面（#68596 有 8 👍，#67413 有 5 👍），表明用户相信维护者会回应具体、范围明确的请求。
- extended-stable / LTS 线（#v2026.9.33）是一个成熟的运营信号，表明项目不仅服务于折腾党，也在服务保守的生产用户。

**痛点（排序）：**
1. **存储层不稳定占主导。** 评论最多的三个 issue（#143524、#164294、#172091）都是同一根因的变体：SQLite WAL checkpoint 因长期存活的 reader 而无法推进。用户报告多 GB 的 WAL 文件、SSD 磨损和 Gateway 启动失败——其中一些自 9 月以来一直 open，且为 P0。
2. **plugin capture 带来的性能回归**（#160959、#157989）——Gateway 停顿数分钟，每次 Gateway 启动约 6.5 GB I/O。使用大型插件树的用户受到实质性阻塞。
3. **更新可靠性**——macOS 临时目录超时、LXC seccomp/FICLONE 失败、systemd 遗弃以及 `state-migrated-no-rollback` 死锁。该窗口内多个版本都专门为此发布，表明维护者听到了这些痛点。
4. **会话/消息丢失竞态**——被挤掉的 turn、Signal 文本丢失、subagent 完成副作用（#148707、#101793、#159512、#96975）。用户感知到回复投递的不确定性，这对个人助理产品来说是最糟糕的一类 bug。
5. **Token/context 经济性**——bootstrap 重新注入（#67419）、context-overflow 过度计数（[#101929](https://github.com/openclaw/openclaw/issues/101929)）以及 Gemini `textSignature` 膨胀（[#48709](https://github.com/openclaw/openclaw/issues/48709)）都直接转化为用户成本和延迟。

**总体情绪：** 用户投入且精准，但大量 P0/P1 存储和生命周期 bug 表明，生产运维人员被要求吸收真实风险。快速 beta 节奏和明确的 LTS 分支是正确的缓解措施，但关闭 WAL 问题簇应当是首要任务。

## 8. 积压观察

重要、长期存在和/或等待维护者关注的 issue 和 PR：

**存储层（最高优先级——合并为单一工作流）：**
- [#143524](https://github.com/openclaw/openclaw/issues/143524) — WAL 增长到 2.8 GB。自 **2026-09-09** 起 open，128 条评论，无修复 PR。跟踪器中讨论最多的单个 issue。
- [#164294](https://github.com/openclaw/openclaw/issues/164294) — WAL 自动 checkpoint 永远无法完成。自 **2026-10-03** 起 open，17 条评论。
- [#172091](https://github.com/openclaw/openclaw/issues/172091) — worker SQLite 事务被永久钉在 open 状态。提交于 **2026-10-14**；WAL 家族的最新成员，也是对当前 beta 最可行动的一项。

**长期 stale 但影响重大：**
- [#68596](https://github.com/openclaw/openclaw/issues/68596) — 可配置 streaming watchdog。自 **2026-04-18** 起 open（约 6 个月），在 top-50 中 reaction 数最高。虽标记为 `stale`，但显然仍然相关。
- [#70903](https://github.com/openclaw/openclaw/issues/70903) — 持久 provider cooldown 在计费恢复后仍阻塞用户。自 **2026-04-24** 起 open，P0，stale，未附加修复 PR。
- [#67419](https://github.com/openclaw/openclaw/issues/67419) — 每轮重新注入 bootstrap 文件浪费 20–30% context。自 **2026-04-15** 起 open，P2。
- [#67413](https://github.com/openclaw/openclaw/issues/67413) — 按 agent 的 dreaming 配置（防止 OOM）。自 **2026-04-15** 起 open，5 👍。
- [#69242](https://github.com/openclaw/openclaw/issues/69242) — Linux `exec` 在没有 OOM 证据的情况下 SIGKILL 广泛的 find/grep。自 **2026-04-20** 起 open，stale，需要信息。
- [#72015](https://github.com/openclaw/openclaw/issues/72015) — active-memory 在多 agent Gateway 上阻塞回复。自 **2026-04-26** 起 open。

**需要维护者关注的 PR：**
- [PR #173523](https://github.com/openclaw/openclaw/pull/173523) — `status: ⏳ waiting on author`、`merge-risk: 🚨 availability`、`security-sensitive-changed`，XL 规模。这是一项承重重构；此处的审查延迟会阻塞下游修复。
- [PR #173504](https://github.com/openclaw/openclaw/pull/173504) — 与上述相同标记；移除 Gateway reload 债务是可预测启动行为的前提。
- [PR #171990](https://github.com/openclaw/openclaw/pull/171990) — *fix: silence replies ending in a standalone `NO_REPLY`*。自 **2026-10-14** 起 open，P1，触及所有主要 channel（Discord、Matrix、Mattermost、Teams、Slack、Telegram、Feishu）。跨消息表面的广泛影响范围。
- [PR #157806](https://github.com/openclaw/openclaw/pull/157806) — *fix(sessions): preserve requester identity at execution admission*。自 **2026-09-25** 起 open，P1，`security-sensitive-changed`，`merge-risk: 🚨 security-boundary`。长期运行，且对身份/权限至关重要。
- [PR #172681](https://github.com/openclaw/openclaw/pull/172681) — *refactor(search): engine-owned full-text and vector capabilities*。自 **2026-10-15** 起 open，XL，memory-core——与上文若干搜索/记忆功能请求相关。

---

**底线：** OpenClaw 的发布速度和 PR 吞吐量健康，v2026.10.10-beta.1 线正确瞄准 Gateway 最大的结构性弱点（主线程上的同步 SQLite 和 plugin-capture 工作）。主要健康风险是**未解决的 WAL/checkpoint 簇**（#143524、#164294、#172091），它具有 P0 严重性、已开放数月且没有关联修复 PR——它应当成为下一发布周期的明确锚点。次要关注项是长期 stale 的 P0 provider-cooldown bug（#70903）和影响范围广泛的 `NO_REPLY` PR（#171990）。

---

## 横向生态对比

# 跨项目对比报告 — 个人 AI 助手 / Agent 开源生态  
**日期：** 2026-10-16  
**项目：** OpenClaw、Hermes Agent、IronClaw、QwenPaw、ZeroClaw

## 1. 生态概览

个人 AI 助手 / Agent 开源格局正在分化为三个梯队：OpenClaw 处于超大规模，Hermes/QwenPaw/ZeroClaw 处于积极产品化阶段，IronClaw 则处于安静的、聚焦评估的细分领域。在活跃项目中，主导性的工程压力集中在**网关/会话层**：单所有者会话状态、持久化存储、插件生命周期、多渠道投递和提供商路由。安全与可观测性正从可选功能转变为发布门禁，尤其是在插件、MCP/审批流程、凭据和多用户身份方面。四个项目的贡献量仍然健康，但在 OpenClaw 之外，瓶颈是合并/评审吞吐量，而非贡献者兴趣。

## 2. 活动对比

| 项目 | Issues 更新数 | PRs 更新数 | 发布状态 | 健康分* |
|---|---:|---:|---|---:|
| **OpenClaw** | 500（393 open、107 closed） | 500（201 open、299 merged/closed） | 4 个发布：`2026.10.10-beta.1`、`2026.10.5-beta.4`、`2026.10.4` 稳定版、`2026.9.33` LTS | **8.0 / 10** |
| **Hermes Agent** | 50（48 open、2 closed） | 50（46 open、4 merged/closed） | 1 个纠正性发布：`v0.22.2` | **6.5 / 10** |
| **IronClaw** | 1（1 open、0 closed） | 0（0 open、0 merged/closed） | 无 | **3.5 / 10** |
| **QwenPaw** | 29（22 open、7 closed） | 23（20 open、3 merged/closed） | 1 个稳定版发布：`v2.2.2` | **7.0 / 10** |
| **ZeroClaw** | 27（27 open、0 closed） | 50（48 open、2 closed） | 无 | **5.5 / 10** |

\*健康分是跨活跃度、合并速度、发布节奏、未解决 P0/P1 严重度以及积压趋势的定性综合评分。它不是厂商指标。

## 3. OpenClaw 的定位

OpenClaw 以巨大优势成为生态中的规模领先者：**24 小时内更新 500 个 issues 和 500 个 PR**，**299 个 PR 已合并/关闭**，并有**四个发布**，其中包括扩展稳定/LTS 分支。Hermes、QwenPaw 和 ZeroClaw 的同类活动大约低 10 倍，IronClaw 则基本沉寂。

**相对同类的优势：**
- 最大的社区和维护者吞吐量。
- 成熟的 LTS/稳定版/beta 发布结构。
- 最广泛的操作面：Gateway、Control UI、插件、渠道、cron、subagents、存储、更新。
- 用户成熟度高：详细的 P0/P1 复现、diamond/lobster 评级的报告。

**技术路线差异：**
- 数据库优先运行时，明确将 SQLite 工作从主线程移至工作线程。
- 大规模 Gateway 生命周期重构，以消除重载债务并改善启动/空闲行为。
- 插件捕获和生命周期记账正在简化，但仍是主要风险领域。
- LTS 分支服务于保守型运维者；beta 线承载激进的 Gateway 加固。

**关键风险：** 未解决的 **SQLite WAL/checkpoint 集群**（`#143524`、`#164294`、`#172091`）是 P0，已持续数月，且没有关联的修复 PR。插件捕获成本和更新可靠性是次要但重要的问题。OpenClaw 是最成熟的项目，但其存储层债务是生态中最大的单一可靠性风险。

## 4. 共同技术关注领域

| 需求 | 项目 | 具体需求 |
|---|---|---|
| **网关/会话生命周期正确性** | OpenClaw、Hermes、QwenPaw、ZeroClaw | 单所有者会话、安全的轮次替换、不静默取消 cron/agent、重启韧性、进程监督。 |
| **持久化存储/检查点** | OpenClaw、ZeroClaw、Hermes、QwenPaw | WAL 检查点完成、持久化配置写入、会话状态持久性、竞态下不丢消息。 |
| **插件/扩展生命周期** | OpenClaw、ZeroClaw、QwenPaw、Hermes | 异步/去重插件捕获、热重载/卸载、WASM 隔离、出站拒绝、MCP 授权、可安全回滚的更新。 |
| **渠道/消息传递可靠性** | OpenClaw、ZeroClaw、QwenPaw、Hermes | 幂等投递、附件处理、自动回复循环、`\Seen` 状态、本地化、不丢失 Signal/Discord/email/WhatsApp 消息。 |
| **可观测性/进度可见性** | ZeroClaw、QwenPaw、OpenClaw、Hermes | 工具回执、部分输出、跨设备消息可见性、可配置看门狗、可信的发布元数据。 |
| **提供商/模型路由与流式传输** | Hermes、QwenPaw、ZeroClaw、OpenClaw、IronClaw | 原子化提供商/模型覆盖、流式传输正确性、解析器兼容性、过期 `base_url` 修复、模型质量评估。 |
| **安全/身份/凭据** | ZeroClaw、Hermes、OpenClaw、QwenPaw | OIDC、多用户隔离、审批令牌、依赖通告、凭据作用域、机密信息脱敏。 |

## 5. 差异化分析

| 项目 | 目标用户 | 功能重点 | 技术架构 |
|---|---|---|---|
| **OpenClaw** | 生产环境运维者、大型 agent 集群、高级用户 | 广泛的个人助手平台；Gateway、Control UI、渠道、cron、subagents、LTS | 数据库优先运行时；SQLite 迁移至工作线程；单体 Gateway，采用 LTS/beta 拆分 |
| **Hermes Agent** | 多界面本地用户、homelab、多提供商配置 | 跨 CLI/TUI/Desktop/ACP/cron/bots 的统一网关；MCP 与审批安全 | 本地会话网关拥有所有界面；提供商路由与凭据加固 |
| **IronClaw** | 基准/评估团队、模型质量研究人员 | 故障分类法、officeqa 诊断、模型质量分诊 | 评估/基准测试框架；并非面向终端用户的助手平台 |
| **QwenPaw** | 多设备、多 agent 桌面用户；第三方 harness 用户 | 控制台/桌面端/聊天终端、技能池、插件中心、多 agent 编排 | 基于 AgentScope 的控制台 + Hub + 桌面端；插件目录与 ACP 会话 |
| **ZeroClaw** | 自托管、注重安全的运维者；Rust/WASM 生态 | WASM 插件、渠道（email/WhatsApp/Telegram/Slack）、OIDC/多用户 | Rust 运行时 + 网关分离；WASM 插件宿主；预发布 v0.8.6/v0.9.0 |

## 6. 社区动能与成熟度

**活跃度分层：**
1. **超大规模 / 成熟：** OpenClaw — 极高速度、四个发布、LTS，但有显著的 P0 存储债务。
2. **积极产品化 / 趋于稳定：** Hermes、QwenPaw、ZeroClaw。
   - **Hermes：** 统一网关之后的余波；纠正性发布；定向修复 PR；因错误提交发布引发的信任问题。
   - **QwenPaw：** 稳定版 `v2.2.2`；控制台/桌面端工作强劲；但积压增长，且存在关键生命周期/取消 bug。
   - **ZeroClaw：** 贡献量极高，但 **48 个 open PR vs 2 个 closed**；评审不足；窗口期内无发布。
3. **低活跃 / 细分：** IronClaw — 1 个 issue、0 个 PR、0 个发布；聚焦基准测试且沉寂。

**快速迭代：** OpenClaw、ZeroClaw、QwenPaw。  
**趋于稳定：** Hermes、OpenClaw LTS 分支。  
**成熟度排名：** OpenClaw > QwenPaw ≈ Hermes > ZeroClaw > IronClaw。

## 7. 趋势信号

1. **网关整合是新的架构战场。** 项目正在收敛到单一网关，由它拥有跨 CLI、桌面端、bots、cron 和 subagents 的会话。难题在于所有权、重启韧性和避免静默取消。
2. **存储工作正在移出主线程。** OpenClaw 的 beta 明确将 SQLite 移至工作线程；同类项目也表现出相关的持久性和配置写入顾虑。异步存储正成为响应式 agent 集群的基线预期。
3. **插件生命周期是安全与可靠性风险面。** OpenClaw、ZeroClaw、QwenPaw 和 Hermes 都有进行中的插件/MCP/审批工作。需求包括：去重捕获、热重载、回滚安全、出站控制和供应链补丁。
4. **多渠道可靠性已是基本门槛。** 邮件循环、WhatsApp 语音消息丢失、Signal 文本丢失、Discord 异步结果失败以及本地化回退，都是反复出现的用户可见故障。幂等性和投递保证如今是核心产品要求。
5. **可观测性是 UX 功能，而不仅是诊断。** 用户想要工具回执、部分输出、进度流和跨设备可见性。静默的长时运行轮次是 ZeroClaw 和 QwenPaw 中重复最多的 UX 抱怨。
6. **提供商路由与流式传输正确性仍然脆弱。** 过期的 `base_url`、强制非流式路径、OpenCode 会话头、DSML 解析以及可配置看门狗表明，模型/提供商抽象仍不成熟。
7. **上下文/token 经济性是直接的成本与延迟杠杆。** 引导重注入、过大的工具结果、base64 膨胀和输出边界控制，是 OpenClaw、QwenPaw 和 ZeroClaw 中活跃关注点。
8. **身份、多用户和凭据治理正在上升。** ZeroClaw 的 OIDC/多用户追踪、Hermes 的 MCP/审批安全，以及 OpenClaw 的安全敏感 PR，表明企业/自托管部署正在将安全前置。
9. **平台一致性很重要。** Windows 启动、macOS LaunchAgent 身份、Linux socket 路径和非英语区域设置都是反复出现的摩擦点。跨平台可靠性是成熟度信号。
10. **评估/模型质量分类法正在出现。** IronClaw 的故障分类法表明需要系统化的基准诊断，尤其是在故障源于模型质量而非平台 bug 时。

---

## 同赛道项目详细报告

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目摘要 — 2026-10-16

## 1. 今日概览
Hermes Agent 正处于高活跃度的稳定化阶段。过去 24 小时内，有 50 个 issue 和 50 个 PR 被更新：48 个 issue 仍处于开放/活跃状态，46 个 PR 开放，4 个 PR 已合并/关闭。当前的主线是统一网关 PR [#106742](https://github.com/NousResearch/hermes-agent/pull/106742) 合并后的余波，由 issue [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) 跟踪，已有 69 条评论，并衍生出多个修复分包 issue。已发布修复版本 [v0.22.2](https://github.com/NousResearch/hermes-agent/releases/tag/v0.22.2)，用于替代因错误 commit 构建的 v0.22.1。项目整体健康度活跃但风险偏高：维护者和贡献者正通过有针对性的 PR 响应，但 P1/P2 网关、供应商路由、平台和安全问题仍然存在。

## 2. 发布
### v0.22.2 — October 15, 2026
- **链接：** [v0.22.2](https://github.com/NousResearch/hermes-agent/releases/tag/v0.22.2)
- **变更：** v0.22.2 是一个修复版本。v0.22.1 因错误 commit 构建、带上了未发布的网关改动，且 Microsoft Store 应用可能卡在“Connecting.”
- **破坏性变更：** 所提供的发布说明中未明确说明。
- **迁移说明：** 使用 v0.22.1 的用户应更新到 v0.22.2。发布说明标注了一个已知问题，影响在 0.22.1 网关下运行过的会话，但提供的摘录被截断。
- **相关发布元数据 issue：** 由于 [#135217](https://github.com/NousResearch/hermes-agent/issues/135217) 以及现已关闭的重复项 [#139514](https://github.com/NousResearch/hermes-agent/issues/139514)，CLI/banner 的发布日期可能仍显示旧日期。

## 3. 项目进展
- **已合并/关闭的 PR：** 过去 24 小时内有 4 个 PR 被合并或关闭，但提供的 top-20 列表仅包含开放的 PR，因此无法根据该数据确定具体的已合并 PR。
- **统一网关后续：** [#106742](https://github.com/NousResearch/hermes-agent/pull/106742) 于 Oct 14 合并。合并后修复正通过 [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) 和分包 issue 组织：
  - [#139158](https://github.com/NousResearch/hermes-agent/issues/139158) — 经典 CLI，16 项发现
  - [#139160](https://github.com/NousResearch/hermes-agent/issues/139160) — 会话创建、模型选择、凭据，17 项发现
  - [#139161](https://github.com/NousResearch/hermes-agent/issues/139161) — Ink TUI 和 Desktop 会话、命令、实时事件，19 项发现
  - [#139162](https://github.com/NousResearch/hermes-agent/issues/139162) — 消息机器人、cron、Kanban worker，10 项发现
- **已关闭 issue：** [#139514](https://github.com/NousResearch/hermes-agent/issues/139514) 已作为重复项关闭；底层发布日期陈旧问题仍在 [#135217](https://github.com/NousResearch/hermes-agent/issues/135217) 中开放。
- **进行中的活跃修复 PR：** [#139521](https://github.com/NousResearch/hermes-agent/pull/139521) cron 不活跃，[#139522](https://github.com/NousResearch/hermes-agent/pull/139522) 浏览器回退工具，[#139524](https://github.com/NousResearch/hermes-agent/pull/139524) 桌面回退模型选择器，[#139353](https://github.com/NousResearch/hermes-agent/pull/139353) Azure CLI 认证，[#139519](https://github.com/NousResearch/hermes-agent/pull/139519) Windows bot 轮次锁，[#139358](https://github.com/NousResearch/hermes-agent/pull/139358) Docker agent 路径，[#130459](https://github.com/NousResearch/hermes-agent/pull/130459) Anthropic 凭据停用，[#117280](https://github.com/NousResearch/hermes-agent/pull/117280) Discord 管理操作，以及 [#139516](https://github.com/NousResearch/hermes-agent/pull/139516) 插件目录上下文引擎。

## 4. 社区热点
- [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) — **69 条评论。** 跟踪统一网关的合并后修复。底层需求：稳定一个网关，使其在 CLI、TUI、Desktop、ACP、cron、机器人、Windows、会话状态和消息投递之间拥有全部本地会话。
- [#125095](https://github.com/NousResearch/hermes-agent/issues/125095) — **8 条评论，1 👍。** `claude-subscription-directsdk` 被强制走非流式路径，导致 240s 陈旧看门狗杀死健康的长 Opus 响应。需求：供应商插件流式正确性。
- [#28024](https://github.com/NousResearch/hermes-agent/issues/28024) — **7 条评论。** 功能请求：提供白名单化的 `config_set` 工具，让 agent 能安全地自配置非安全设置。需求：带配置安全性的 agent 自主性。
- [#138843](https://github.com/NousResearch/hermes-agent/issues/138843) — **7 条评论，1 👍。** 本地 `session.create` 供应商覆盖保留了陈旧的 `model.base_url`，将新供应商的密钥发送到旧端点并产生 401。需求：原子化供应商/模型路由更新。
- [#123118](https://github.com/NousResearch/hermes-agent/issues/123118) — **5 条评论。** macOS 网关 LaunchAgent 在“隐私与安全性”中显示为不透明的 `osascript`。需求：可识别的 Hermes 后台服务。
- [#135217](https://github.com/NousResearch/hermes-agent/issues/135217) — **5 条评论。** `hermes --version` 和 banner 显示陈旧的发布日期。需求：可信的构建/发布元数据。
- [#133744](https://github.com/NousResearch/hermes-agent/issues/133744) — **5 条评论。** 重启后 PID 复用落在启动 tick 漂移容忍范围内，导致受监督网关永久卡死。需求：有韧性的进程监督。
- [#113094](https://github.com/NousResearch/hermes-agent/issues/113094) — **5 条评论。** `browser_vault_enter_code` 会漏掉没有 `maxlength`/`autocomplete` 的分段 OTP 控件。需求：更好的浏览器认证自动化。

## 5. Bug 与稳定性
按严重程度排序。以下并非完整列表；共有 50 个 issue 被更新，48 个仍然活跃。

### P1
- [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) — **跟踪：一个网关拥有所有本地会话 — 合并后修复。** 影响 agent、CLI、网关、TUI、ACP、cron、Desktop、Windows、会话、profiles。修复工作拆分在 [#139158](https://github.com/NousResearch/hermes-agent/issues/139158)、[#139160](https://github.com/NousResearch/hermes-agent/issues/139160)、[#139161](https://github.com/NousResearch/hermes-agent/issues/139161) 和 [#139162](https://github.com/NousResearch/hermes-agent/issues/139162)。

### P2
- [#138843](https://github.com/NousResearch/hermes-agent/issues/138843) — 供应商覆盖保留陈旧的 `base_url`，导致 401。分包/修复路径：[#139160](https://github.com/NousResearch/hermes-agent/issues/139160)。
- [#123118](https://github.com/NousResearch/hermes-agent/issues/123118) — macOS LaunchAgent 显示为 `osascript`，无法识别为 Hermes。
- [#133744](https://github.com/NousResearch/hermes-agent/issues/133744) — 重启后 PID 复用导致受监督网关卡死。
- [#132492](https://github.com/NousResearch/hermes-agent/issues/132492) — 1Password vault 填充无法确定页面 origin。
- [#134394](https://github.com/NousResearch/hermes-agent/issues/134394) — 依赖固定版本带有未关闭的安全公告：PyJWT 2.13.0、httpx2 2.7.0、urllib3 2.7.0、oauthlib 3.3.1。
- [#139091](https://github.com/NousResearch/hermes-agent/issues/139091) — 稳定版更新 10s merge-base 探测超时，即使两个 commit 都在本地。
- [#137843](https://github.com/NousResearch/hermes-agent/issues/137843) — 切换会话后 Desktop 工作区文件面板卡在加载中。
- [#138261](https://github.com/NousResearch/hermes-agent/issues/138261) — 由 `model.default` 命名的模型被模型切换验证器拒绝。
- [#139135](https://github.com/NousResearch/hermes-agent/issues/139135) — Discord 异步子任务完成，但持久结果仍为 pending；父任务不会恢复。
- [#139429](https://github.com/NousResearch/hermes-agent/issues/139429) — 启用备份时，`hermes update` 会将 root 拥有的备份文件放入 profile 备份目录中。
- [#70387](https://github.com/NousResearch/hermes-agent/issues/70387) — OpenRouter `response_cache` 默认启用，存在重放陈旧工具调用的风险。
- [#137645](https://github.com/NousResearch/hermes-agent/issues/137645) — 当用户是本地账户时，Windows 域加入网关安装因 `USERDOMAIN` 污染而失败。
- [#122786](https://github.com/NousResearch/hermes-agent/issues/122786) — 当 scratch + task-id 会话名超过 agent-browser 的 103-byte socket 上限时，浏览器工具在 Linux 上失败。

### P3
- [#139138](https://github.com/NousResearch/hermes-agent/issues/139138) — 带有含糊 `permission_denied` 状态的 Discord/bot-mailbox 回执在每次网关扫描时被以未知 schema 拒绝。
- [#139492](https://github.com/NousResearch/hermes-agent/issues/139492) — `/context` 将虚构的 256K 回退值显示为真实上下文窗口。
- [#138785](https://github.com/NousResearch/hermes-agent/issues/138785) — Anthropic `AuthError` 没有 code，因此 kanban worker 退出 1 而不是 75，并触发 failure breaker。
- [#120263](https://github.com/NousResearch/hermes-agent/issues/120263) — Azure Foundry `gpt-6-luna` 在启用函数工具和推理时需要 Responses API。
- [#113094](https://github.com/NousResearch/hermes-agent/issues/113094) — 浏览器 vault OTP 输入漏掉分段控件。
- [#135217](https://github.com/NousResearch/hermes-agent/issues/135217) — `hermes --version` 和 banner 中的发布日期陈旧。
- [#139514](https://github.com/NousResearch/hermes-agent/issues/139514) — 已作为重复项关闭；陈旧的 `__release_date__` 仍然存在。

### 值得关注的修复 PR
针对多个稳定性/安全领域，已有针对性的开放 PR：[#139521](https://github.com/NousResearch/hermes-agent/pull/139521) cron 不活跃，[#139522](https://github.com/NousResearch/hermes-agent/pull/139522) 浏览器回退，[#139524](https://github.com/NousResearch/hermes-agent/pull/139524) 桌面 profile 回退选择器，[#139353](https://github.com/NousResearch/hermes-agent/pull/139353) Azure CLI 认证，[#139519](https://github.com/NousResearch/hermes-agent/pull/139519) Windows bot 轮次锁，[#139358](https://github.com/NousResearch/hermes-agent/pull/139358) Docker 容器路径，[#130459](https://github.com/NousResearch/hermes-agent/pull/130459) Anthropic 计划窗口凭据停用，[#95756](https://github.com/NousResearch/hermes-agent/pull/95756) MCP 持久授权，[#112415](https://github.com/NousResearch/hermes-agent/pull/112415) 审批载荷令牌，[#116140](https://github.com/NousResearch/hermes-agent/pull/116140) cron 监控变更，[#136635](https://github.com/NousResearch/hermes-agent/pull/136635) MCP 认证暂挂，[#136636](https://github.com/NousResearch/hermes-agent/pull/136636) MCP OAuth 供应商，[#136634](https://github.com/NousResearch/hermes-agent/pull/136634) 审批拒绝规则，[#132395](https://github.com/NousResearch/hermes-agent/pull/132395) 浏览器 stderr 尾部，[#136637](https://github.com/NousResearch/hermes-agent/pull/136637) CDP URL 处理，[#136638](https://github.com/NousResearch/hermes-agent/pull/136638) CDP 重拨日志，[#136639](https://github.com/NousResearch/hermes-agent/pull/136639) vault 卡到期日期，以及 [#137370](https://github.com/NousResearch/hermes-agent/pull/137370) vault 卡密钥脱敏。

## 6. 功能请求与路线图信号
- [#28024](https://github.com/NousResearch/hermes-agent/issues/28024) — 白名单化的 `config_set` 工具，用于 agent 自配置。P3，7 条评论，`needs-decision`。
- [#107937](https://github.com/NousResearch/hermes-agent/issues/107937) — 在 `delegate_task` 上为每任务固定供应商/模型，用于跨供应商子 agent。P3，`needs-decision`。
- [#90435](https://github.com/NousResearch/hermes-agent/issues/90435) — 将 Desktop Preview 和 Browser 统一为面向人与 agent 的 Workspace Viewer。P3，创新。
- [#80828](https://github.com/NousResearch/hermes-agent/issues/80828) — 当配置了多个 Copilot 凭据时，提供 Desktop UI 查看/选择当前活跃凭据。P3。
- [#117280](https://github.com/NousResearch/hermes-agent/pull/117280) — Discord 频道、角色和服务器管理操作。开放 PR。
- [#139516](https://github.com/NousResearch/hermes-agent/pull/139516) — 插件目录新增：`hermes-vcc-engine`。开放 PR。
- [#139353](https://github.com/NousResearch/hermes-agent/pull/139353) — 面向 Foundry 的 profile 范围 Azure CLI 认证。开放 PR。
- [#130459](https://github.com/NousResearch/hermes-agent/pull/130459) — 当计划窗口用尽时停用 Anthropic 凭据。开放 PR。

**预测：** 下一个补丁版本，很可能是 v0.22.3，将以统一网关合并后修复、发布元数据修正、Windows/Docker 路径修复，以及 MCP/审批安全加固为主。带有开放 PR 的功能请求——Discord 管理操作、插件目录、Azure CLI 认证、Anthropic 凭据停用——如果经过评审并合并，有可能进入即将发布的版本。`needs-decision` 项（`config_set`、delegate 供应商固定、Workspace Viewer）进入下一个补丁的可能性较低。

## 7. 用户反馈摘要
- **痛点：** 统一网关回归影响会话、消息投递、兼容性和 Windows；错误 commit 的 v0.22.1 发布和过时发布日期引发信任问题；供应商路由错误产生 401；macOS“隐私与安全性”、Windows 域加入安装和 Linux 浏览器 socket 上的平台特定故障；固定依赖中存在未关闭的安全公告；OTP 和 vault origin 的浏览器自动化缺口；Desktop 文件面板卡在加载中；Discord 异步 delegate 结果不恢复；profile 备份目录内出现 root 拥有的备份文件。
- **满意信号：** 维护者正在积极分类、将网关余波拆分为分包 issue，并开启有针对性的 PR。反应总体较少，少数 issue 获得 1 个 👍。
- **不满信号：** 用户对备份文件行为、macOS 上不透明的 `osascript` 身份、UI 面板卡住、401 供应商错误、网关进程卡死和版本输出陈旧给出了直率反馈。
- **真实用例：** 多供应商 homelab 子 agent、语音/STT/TTS 配置、基于浏览器的 1Password/OTP 流程、Discord 机器人管理、Azure Foundry、Copilot 凭据选择，以及桌面工作区预览。

## 8. 积压观察
长期未回应或高影响、需要维护者关注的条目：
- [#28024](https://github.com/NousResearch/hermes-agent/issues/28024) — 创建于 May 18；`config_set` 工具仍未关闭，7 条评论。
- [#70387](https://github.com/NousResearch/hermes-agent/issues/70387) — 创建于 Jul 23；P2 OpenRouter `response_cache` 陈旧工具调用风险。
- [#80828](https://github.com/NousResearch/hermes-agent/issues/80828) — 创建于 Aug 7；Desktop Copilot 凭据 UI，仅 1 条评论。
- [#90435](https://github.com/NousResearch/hermes-agent/issues/90435) — 创建于 Aug 20；Workspace Viewer 提案，3 条评论。
- [#95756](https://github.com/NousResearch/hermes-agent/pull/95756) — 创建于 Aug 26；P2 MCP 持久授权 PR，安全边界。
- [#112415](https://github.com/NousResearch/hermes-agent/pull/112415) — 创建于 Sep 15；审批载荷令牌 PR，安全敏感。
- [#113094](https://github.com/NousResearch/hermes-agent/issues/113094) — 创建于 Sep 16；分段 OTP 控件，5 条评论。
- [#116140](https://github.com/NousResearch/hermes-agent/pull/116140) — 创建于 Sep 19；P2 cron 监控在准入拒绝方面的变更。
- [#120263](https://github.com/NousResearch/hermes-agent/issues/120263) — 创建于 Sep 23；Azure Foundry Responses API 要求。
- [#122786](https://github.com/NousResearch/hermes-agent/issues/122786) — 创建于 Sep 25；P2 浏览器 socket 路径长度失败。
- [#123118](https://github.com/NousResearch/hermes-agent/issues/123118) — 创建于 Sep 25；P2 macOS LaunchAgent 隐私/身份识别。
- [#125095](https://github.com/NousResearch/hermes-agent/issues/125095) — 创建于 Sep 27；`claude-subscription-directsdk` 流式路径 bug，8 条评论。
- [#132492](https://github.com/NousResearch/hermes-agent/issues/132492) — 创建于 Oct 3；P2 1Password vault origin 判定。
- [#133744](https://github.com/NousResearch/hermes-agent/issues/133744) — 创建于 Oct 6；P2 PID 复用导致网关卡死。
- [#134394](https://github.com/NousResearch/hermes-agent/issues/134394) — 创建于 Oct 7；P2 依赖安全公告。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目摘要 — 2026-10-16

## 1. 今日概览
IronClaw 在 2026-10-16 的活动极少：过去 24 小时有 1 个 issue 更新，PR 活动为 0 个打开/合并/关闭，并有 0 个新版本发布。唯一活跃条目是 open issue #8138，即 2026-10-15 的每日失败分类，评论 0、反应 0。报告窗口内未记录任何代码变更、发布产物或 issue 关闭。从代码流角度看，项目健康状况显得平静/稳定，但工程吞吐量和社区参与度非常低。唯一活跃 issue 涉及基准测试失败分类，而不是已确认的代码缺陷或功能请求。

## 2. 版本发布
报告窗口内无新版本发布。无可报告的变更日志、破坏性变更、迁移说明或发布资产。

## 3. 项目进展
今日无合并或关闭的 PR，因此没有功能推进，也没有修复落地。无 issue 关闭。本窗口项目进展：无记录。

## 4. 社区热点
- **#8138 [OPEN] 每日 ironclaw 失败分类 — 2026-10-15**  
  作者：pranavraja99 | 评论：0 | 👍：0  
  链接：https://github.com/nearai/ironclaw/issues/8138  
  这是唯一活跃的 issue，不过按评论或反应来看并不“热门”。其背后需求是系统化跟踪和分类基准测试失败，尤其是所链接的 officeqa 运行中 50 个未通过任务。issue 摘要表明，这些失败“绝大多数是真实的模型质量错误”，涉及 DeepSeek-V4-Flash，这说明更偏向 eval/模型质量分诊，而非 IronClaw 代码立即不稳定。  
  基准测试链接：https://nearai.github.io/benchmarks/#/runs/ironclaw/officeqa/417ae5e9-500f-43f5-8a6b-91f739687305

## 5. Bug 与稳定性
提供的数据中未出现明确的 bug、崩溃或回归报告。Issue #8138 记录了 50 个未通过的 officeqa 任务，但摘要将其主要描述为模型质量错误，而非 IronClaw 基础设施故障。严重性排名：对项目代码稳定性而言为 **信息性 / 低**。不存在修复 PR，窗口内有 0 个 PR 更新。  
链接：https://github.com/nearai/ironclaw/issues/8138

## 6. 功能请求与路线图信号
数据集中没有功能请求。唯一与路线图相关的信号是持续投入自动化失败分类和基准测试诊断，这可能最终影响 eval 工具、模型路由或质量报告。由于 PR 为 0、版本发布为 0，没有足够证据预测下一版本内容。

## 7. 用户反馈摘要
未捕获到直接用户反馈、满意度或不满意度信号。唯一由作者驱动的输入来自 pranavraja99 的失败分类 issue，这表明需要更清晰地诊断基准测试失败和模型质量限制。由于评论为 0、反应为 0，无法根据这些数据评估社区情绪。

## 8. 积压事项观察
- **#8138 [OPEN] 每日 ironclaw 失败分类 — 2026-10-15**  
  链接：https://github.com/nearai/ironclaw/issues/8138  
  这是数据集中唯一打开的 issue，目前有 0 条评论。它还不算长期未答复，因为创建/更新于 2026-10-15，但如果持续未处理，可能需要维护者分诊或关联的 eval 后续跟进。提供的数据中不存在其他长期未答复的 issue 或 PR。

---

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw 项目摘要 — 2026-10-16

## 1. 今日概览

QwenPaw 活动保持高位：过去 24 小时内有 29 个 issue 有活动（22 个仍开放，7 个已关闭），23 个 PR 有变动（20 个开放，3 个已合并/关闭），同时发布了稳定的 **v2.2.2**。issue 队列以 bug 报告为主，而非功能讨论，多智能体运行时、cron/热重载以及 Windows 桌面稳定性问题在 2.2.1–2.2.2b5 多个版本中反复出现。维护者在修复上的产出显而易见——一批控制台、桌面端和插件目录修复已落地或取得进展——但开放:关闭比例（22:7）表明积压增长速度超过清理速度。评论最多的帖子（#8120 页面加载失败、#7599 OpenCode `MissingSessionID`）已关闭，表明对提供商集成回归进行了积极分诊。整体项目健康度在交付节奏上稳健（稳定版 + beta 版本、活跃的首次贡献者），但在桌面/Windows 可靠性和长时任务生命周期正确性方面吃紧。

## 2. 发布

**v2.2.2（稳定版）** — 发布于约 2026-10-15，发布验证记录在 [#8262](https://github.com/agentscope-ai/QwenPaw/issues/8262)。发布说明通过 [#7928](https://github.com/agentscope-ai/QwenPaw/pull/7928) 准备（已关闭）。

变更日志中可见的亮点：
- **控制台与工作区：** 六套主题调色板，支持自定义颜色和圆角半径、实时预览和持久化偏好（[#7741](https://github.com/agentscope-ai/QwenPaw/pull/7741)）；内嵌于 Chat 中的经过身份验证的多标签终端。

破坏性变更 / 迁移说明：所提供的发布摘录中未说明。此前的 beta 分支（v2.2.2-beta.6，[#8258](https://github.com/agentscope-ai/QwenPaw/issues/8258)）已成功关闭安装验证，因此稳定版路径已在各平台验证。

## 3. 项目进展

**已合并/关闭的 PR（3 个）：**
- [#8259](https://github.com/agentscope-ai/QwenPaw/pull/8259) — `feat(tools): add structured human-in-the-loop questions`。交付 `ask_user_question` 工具，让 agent 可在回合中途暂停、收集结构化答案，并在同一回合内恢复（复用 AgentScope 2.0.10 的 `AskUserParams`/`AskUserAnswer`）。这是交互式 agent 流程的重要能力提升。
- [#7928](https://github.com/agentscope-ai/QwenPaw/pull/7928) — `chore: release notes for v2.2.2`。
- [#8260](https://github.com/agentscope-ai/QwenPaw/pull/8260) — `fix(desktop): correct Windows listen backlog and chunk recovery state`。将复用端口的监听 backlog 从 1 提升到 2048，并修复 chunk 恢复期间出现的虚假错误页面——与“页面加载失败”投诉直接相关。

**推进中的修复 PR（开放，审核中）：**
- [#8263](https://github.com/agentscope-ai/QwenPaw/pull/8263) — 解除配置编辑器阻碍（继续编辑 / 放弃并关闭工作流）。
- [#8229](https://github.com/agentscope-ai/QwenPaw/pull/8229) — 截断聊天历史中过大的 base64 工具输出（解决 10 MB+ 响应和 30 秒控制台超时）。
- [#8261](https://github.com/agentscope-ai/QwenPaw/pull/8261) — 使 Hub 插件目录与已安装状态保持一致。
- [#8257](https://github.com/agentscope-ai/QwenPaw/pull/8257) — 在设置中心遵循菜单可见性。
- [#8203](https://github.com/agentscope-ai/QwenPaw/pull/8203) — 将用户自己的消息作为新运行流的第一帧回显（跨设备可见性缺口）。
- [#8012](https://github.com/agentscope-ai/QwenPaw/pull/8012) — Telegram：正确渲染每个围栏代码块（带符号的信息字符串，例如 `c++`）。

**正在推进的大型功能：** PawApp SDK/控制平面重新设计（[#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874)）、社区/收件箱集成（[#7903](https://github.com/agentscope-ai/QwenPaw/pull/7903)）、实时语音聊天（[#7785](https://github.com/agentscope-ai/QwenPaw/pull/7785)）、作为插件的通道并带子进程隔离（[#8187](https://github.com/agentscope-ai/QwenPaw/pull/8187)）、插件热重载/卸载（[#7565](https://github.com/agentscope-ai/QwenPaw/pull/7565)）以及 OpenUI 内联渲染（[#8248](https://github.com/agentscope-ai/QwenPaw/pull/8248)）。

## 4. 社区热门话题

| 条目 | 评论 | 信号 |
|---|---|---|
| [#8120](https://github.com/agentscope-ai/QwenPaw/issues/8120) `[CLOSED]` 多设备频繁出现“页面加载失败”（2.2.2b4） | 8 | 参与度最高的帖子；分诊后关闭。桌面/Web 可靠性是用户可见的最大痛点。 |
| [#7599](https://github.com/agentscope-ai/QwenPaw/issues/7599) `[CLOSED]` OpenCode Go 模型因 `MissingSessionID` 失败（400） | 5 | 提供商集成故障；与相关的 [#7531](https://github.com/agentscope-ai/QwenPaw/issues/7531) 一同关闭。 |
| [#8126](https://github.com/agentscope-ai/QwenPaw/issues/8126) `[OPEN]` 将技能池下载改为可取消的后台任务 | 4 | #8055 的后续；用户在大技能包上触发客户端 30 秒超时。近期工作的有力候选。 |
| [#8252](https://github.com/agentscope-ai/QwenPaw/issues/8252) `[OPEN]` 智能体间调用被静默取消（`CancelledError`） | 3 | 多智能体集群用户丢失产物，且未记录发起者/原因。高严重性正确性缺口。 |
| [#7531](https://github.com/agentscope-ai/QwenPaw/issues/7531) `[CLOSED]` OpenCode API 现在需要 `x-opencode-session` 请求头 | 3 | 与 #7599 同一提供商集群；供应商侧 API 变更。 |

**潜在需求：** (a) 用户运行真正的多设备、多智能体集群，并期望确定性的生命周期语义（无静默取消、重载时不丢数据）；(b) 第三方提供商兼容性（OpenCode、Tavily、Firecrawl）是反复出现的摩擦点，用户希望在内置目录中呈现并维护。

## 5. Bug 与稳定性

按严重性排序（影响 × 可复现性）：

1. **严重 — 智能体间会话被静默取消** [#8252](https://github.com/agentscope-ai/QwenPaw/issues/8252)（开放，2.2.1，Windows）。`CancelledError` 以同一秒成对到达，产物归零，且 `qwenpaw/hooks/error/error_hook.py:90` 处的取消路径既不记录发起者也不记录原因。尚无修复 PR。
2. **严重 — 热重载取消正在运行的 cron 任务** [#8225](https://github.com/agentscope-ai/QwenPaw/issues/8225)（开放，2.2.1）。通过 `PUT /api/config/heartbeat` 保存心跳设置会中止进行中的 cron 作业，记录为 `cancelled`，且没有用户通知。无修复 PR。
3. **高 — Hub 重启会丢弃期望运行的运行时；cron 作业被静默跳过** [#8213](https://github.com/agentscope-ai/QwenPaw/issues/8213)（开放）。本地进程 provisioner 在重启后不恢复运行时；Hub 可能看起来健康，但计划任务被错过。
4. **高 — Doom-loop 门控杀死合法轮询** [#8184](https://github.com/agentscope-ai/QwenPaw/issues/8184)（开放）。门控无法区分“卡住重复”和“等待长时间运行的后台任务”，随后 cron 会记录一次假成功。
5. **高 — 频繁页面加载失败** [#8120](https://github.com/agentscope-ai/QwenPaw/issues/8120) `[CLOSED]`。在多台设备上复现；可能已通过 [#8260](https://github.com/agentscope-ai/QwenPaw/pull/8260)（监听 backlog 1 → 2048）和 chunk 恢复修复得到缓解。
6. **中高 — 插件热安装在发行版补丁版 Python 3.11 上中止** [#8265](https://github.com/agentscope-ai/QwenPaw/issues/8265)（开放，2.2.2）。在 `loader.py:201/465/1094` 处无条件调用 `importlib.invalidate_caches()`；在某些构建上抛出异常并阻止安装。
7. **中高 — `qwenpaw acp` 从不加载插件** [#8222](https://github.com/agentscope-ai/QwenPaw/issues/8222)（开放）。在 ACP 会话中从未实例化 `PluginLoader`，因此插件工具和跨进程任务回调不可用。
8. **中 — `qwenpaw-creator` Windows 失败：** 项目删除因打开的 `creator.log` 被阻塞（WinError 5，[#8206](https://github.com/agentscope-ai/QwenPaw/issues/8206)），以及因过期 pid 导致启动中止（`SystemError` 未捕获，[#8194](https://github.com/agentscope-ai/QwenPaw/issues/8194)）。
9. **中 — Cron 作业编辑器损坏值** [#8249](https://github.com/agentscope-ai/QwenPaw/issues/8249)（2.2.2 beta5）：默认时间重置为 00:00，且 Request Content 渲染为 `[object Object]`。
10. **中 — 消息导航目录定位错误** [#8251](https://github.com/agentscope-ai/QwenPaw/issues/8251)（2.2.2b4）；是 #8186/#8214 导航修复的后续。
11. **中 — `desktop.log` 在 Windows 上从不轮转** [#8185](https://github.com/agentscope-ai/QwenPaw/issues/8185)：sidecar tee 句柄使 `_SafeRotatingFileHandler` 失效。
12. **低/已解决 — 模型弃用处理** [#8218](https://github.com/agentscope-ai/QwenPaw/issues/8218) `[CLOSED]`（`mimo-v2.5-free` 已弃用 → `mimo-v2.6-flash-free`）；提供商会话头问题 [#7531](https://github.com/agentscope-ai/QwenPaw/issues/7531)/[#7599](https://github.com/agentscope-ai/QwenPaw/issues/7599) `[CLOSED]`。

**模式：** cron/任务生命周期取消类 bug（#8252、#8225、#8213、#8184）构成一个连贯簇群，是价值最高的修复领域；目前均无关联的修复 PR。

## 6. 功能请求与路线图信号

- **可取消且带进度的技能池下载** [#8126](https://github.com/agentscope-ai/QwenPaw/issues/8126) — 直接承接 #8055 中已合并的工作；鉴于由维护者发起的拆分，可能很快落地。
- **Firecrawl 作为第三个 `web_search` 提供商** [#8199](https://github.com/agentscope-ai/QwenPaw/issues/8199) — **实现 PR 已开放**（[#8200](https://github.com/agentscope-ai/QwenPaw/pull/8200)，首次贡献者，审核中）。合并概率高。
- **通过凭据存储配置 Tavily API key** [#8174](https://github.com/agentscope-ai/QwenPaw/issues/8174) → PR [#8204](https://github.com/agentscope-ai/QwenPaw/pull/8204)（审核中）。
- **CommandCode 和 Hugging Face Inference Providers 的提供商预设** [#8233](https://github.com/agentscope-ai/QwenPaw/issues/8233) — 低工作量、目录扩展；有可能进入 2.2.x 补丁。
- **模型池中的删除操作** [#8195](https://github.com/agentscope-ai/QwenPaw/issues/8195) — 清除已弃用模型；与 #8218 自然配对。
- **进行中运行期间的跨设备消息可见性** [#8202](https://github.com/agentscope-ai/QwenPaw/issues/8202) — **修复 PR 已开放**（[#8203](https://github.com/agentscope-ai/QwenPaw/pull/8203)）。
- **受保护执行契约提示词的 i18n** [#8211](https://github.com/agentscope-ai/QwenPaw/issues/8211) — 扩展现有的 `SCROLL_SYSTEM_PROMPT_TEMPLATES` 机制。
- **可选的主机导航策略** [#8223](https://github.com/agentscope-ai/QwenPaw/issues/8223) — 企业/精选部署控制，与进行中的 [#8257](https://github.com/agentscope-ai/QwenPaw/pull/8257) 相关。
- **Claude Code 第三方 harness** [#7396](https://github.com/agentscope-ai/QwenPaw/issues/7396) — 目录条目在 `harnesses/registry.py` 中仍是占位符。

**对下一版本（2.2.3 / 2.3.0）的预测：** Firecrawl + Tavily 凭据支持、跨设备消息回显、消息导航修复、插件目录/已安装状态修复，以及桌面启动/backlog 修复。更大的项目（实时语音、作为插件的通道、PawApp SDK 重新设计、OpenUI）看起来面向次版本/主版本，而非补丁版本。

## 7. 用户反馈总结

**痛点：**
- **桌面/Web 可靠性与启动延迟。** #8120（多设备页面加载失败）和 [#8264](https://github.com/agentscope-ai/QwenPaw/issues/8264)（“启动缓慢，到达主界面前很容易超过 10 秒；感觉比 workbuddy 和 Bean Bag 更差”）显示出与竞品助手之间的感知体验差距。
- **配置变更下的任务可靠性。** 用户仅因保存设置（#8225）或重启 Hub（#8213）就会丢失正在运行的 cron 作业，而且失败是静默的。
- **多智能体编排脆弱性。** #8252 的集群用户运行的是 Orchestrator/Reasoner/Executor/Reviewer/Defender 拓扑，并配有 Feishu 通道网关——这是一个复杂的生产用例，其会话在没有诊断信息的情况下被取消。
- **Windows 是二等平台。** #8185、#8194、#8206 都集中在 Windows 上（日志轮转、过期 pid、文件锁）。
- **提供商漂移。** OpenCode 请求头/会话要求（#7531、#7599）和模型弃用（#8218）会在用户未操作的情况下破坏原本可用的设置。

**满意信号：** 对 2.2.x 控制台投入（主题、Chat 内终端）反响积极，对第三方 harness 功能（支持 Codex 和 Qoder；#7396 中热切期待 Claude Code）热情高涨。首次贡献者正在积极提交 PR（#8012、#8200、#8204、#8259）——社区漏斗健康。不满主要集中在可靠性和感知延迟上，而非缺少功能。

## 8. 积压事项观察

长期存在或高重要性但近期缺乏解决的条目：

- [#7396](https://github.com/agentscope-ai/QwenPaw/issues/7396) — *Claude Code 作为第三方 agent harness——状态与路线图？* 自 **2026-08-28** 开放；两条评论，没有维护者给出路线图答复。用户可以在源码中看到占位符，因此沉默是明显的缺口。
- [#7565](https://github.com/agentscope-ai/QwenPaw/pull/7565) — *干净的插件卸载与回滚安全的热重载*（开放于 **2026-09-04**，size/XXXL）。与今天的插件热安装失败（#8265）和热重载取消（#8225）直接相关；需要审核带宽。
- [#7785](https://github.com/agentscope-ai/QwenPaw/pull/7785) — *实时语音聊天*（开放于 **2026-09-15**，size/XXXL）。长期运行的功能 PR，没有可见的合并信号。
- [#7874](https://github.com/agentscope-ai/QwenPaw/pull/7874) — *重新设计 PawApp SDK 和应用控制平面*（开放于 **2026-09-19**，size/XXXL）。注意 #8266 明确表示 Data App 1.0.3 *独立于* Host vNext 发布，这意味着该重新设计距离落地仍很远——值得发布公开状态说明。
- [#7903](https://github.com/agentscope-ai/QwenPaw/pull/7903) — *集成 QwenPaw 社区和收件箱*（开放于 **2026-09-20**，size/XXXL，`[wip]`）。
- [#8012](https://github.com/agentscope-ai/QwenPaw/pull/8012) — 来自首次贡献者的 Telegram 围栏代码渲染修复（开放于 **2026-09-29**）；小、用户可见，是快速审核以留住贡献者的良好候选。

**建议：** 优先让维护者回应 #7396（路线图明确性），并对 #7565 进行一轮审核，因为插件生命周期正确性支撑着今天三个未关闭 bug。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目摘要 — 2026-10-16

## 1. 今日概览

ZeroClaw 仍处于高变动的预发布开发阶段：过去 24 小时内更新了 27 个 issue 和 50 个 PR，但没有新版本发布，关闭的 issue 为零，仅关闭了两个 PR（一个是被取代的 Dependabot 版本提升，一个是安全补丁）。开放 PR 队列已增长至 48 项，其中多项为 `size:XL` / `risk:high`，并标记为 `release:v0.8.6` 或 `release:v0.9.0`，表明相对于合并吞吐量存在大量审查积压。缺陷活动严重偏向 **email 和 WhatsApp 渠道实现**，同一位贡献者在一天内提交了一组 S2（行为降级）报告。战略工作继续围绕 WASM 插件系统、runtime/gateway 分离以及 OIDC/多用户安全里程碑展开，这些仍作为开放里程碑跟踪，而非已交付。整体健康度：**非常活跃，但合并和发布匮乏**——项目积累已验证工作的速度快于维护者关闭它们的速度。

## 2. 发布

过去 24 小时内没有新发布。没有可报告的发布说明、破坏性变更或迁移指南。

## 3. 项目进展

今天只有两个 PR 离开开放队列：

- **#11755 [CLOSED]** — `fix(deps): patch Wasmtime and Rustls security advisories`。将锁定的 Wasmtime 系列从 `48.0.5 → 48.0.6` 提升（以及 Pulley/Cranelift 对齐），并将三个直接插件依赖的最低版本提高到 `48.0.6`，解决了 **14 个 Wasmtime RustSec 发现项**。这是今天最具体的稳定性胜利。([PR #11755](https://github.com/zeroclaw-labs/zeroclaw/pull/11755))
- **#11723 [CLOSED]** — Dependabot `rust-all` 组版本提升（30 项更新），因被 **#11776** 取代而关闭（同一组，31 项更新）。([PR #11723](https://github.com/zeroclaw-labs/zeroclaw/pull/11723), [PR #11776](https://github.com/zeroclaw-labs/zeroclaw/pull/11776))

功能工作在审查中推进但未合并，尤其是来自 IftekharUddin 的 **插件生命周期栈**（#11236 恢复不完整安装，#11261 分阶段准入替换，#11262 经验证的 `zeroclaw plugin update`，#11309 quickstart 插件安装，#11347 在发布制品中提供 WASM 插件宿主，#11311 制品冒烟测试框架，#11304 出口拒绝日志记录）。根据 OIDC 跟踪器（#8289），核心 OIDC 栈以及整合后的 enrollment/gateway/private-memory 切片（#11082）此前已经落地，使该项工作进入收尾阶段。

## 4. 社区热点

按过去 24 小时窗口的评论数排序：

| 条目 | 评论数 | 主题 |
|---|---|---|
| [#7952](https://github.com/zeroclaw-labs/zeroclaw/issues/7952) | 8 | 发布形态困惑——可选的 `channels-full` 预构建包 vs. 精简默认包 |
| [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) | 7 | runtime Phase 2（v0.8.6）和 gateway 分离（v0.9.0）的跟踪器 |
| [#10531](https://github.com/zeroclaw-labs/zeroclaw/issues/10531) | 6 | Delegate 子代理进度可见性（工具回执、部分输出） |
| [#8763](https://github.com/zeroclaw-labs/zeroclaw/issues/8763) | 4 | 用于 subagent 活动和可展开工具结果的 ZeroCode UI |
| [#8289](https://github.com/zeroclaw-labs/zeroclaw/issues/8289) | 4 | OIDC 规范主体 / 入站认证跟踪器 |
| [#10426](https://github.com/zeroclaw-labs/zeroclaw/issues/10426) | 3 | Telegram 中面向用户的代理进度 |

**底层需求分析：** 评论领先项围绕两种不同的焦虑聚集。第一，**运维人员信心**——用户无法看到被委派的子代理正在做什么（#10531、#8763、#10426），而发布打包争论（#7952）反映了当已配置渠道无法与随附二进制配合工作时的摩擦。第二，**架构持久性**——runtime/gateway（#7432）和 OIDC（#8289）跟踪器是长期存在的事实来源，这表明贡献者希望在进一步构建这些接口面之前获得契约稳定性。

## 5. 缺陷与稳定性

按报告严重程度排序：

**S1 — 工作流受阻**
- [#11130](https://github.com/zeroclaw-labs/zeroclaw/issues/11130) — *DeepSeek DSML 工具调用标记未被解析；原始标记泄漏到渠道，且该轮次静默结束。* 根本原因在共享工具调用解析器中，症状出现在 `channel` 和 `runtime`。`status:in-progress`，因此据信修复正在进行。([related #11742](https://github.com/zeroclaw-labs/zeroclaw/issues/11742) 将此扩大为 `custom` OpenAI 兼容提供商默认回退到文本。）

**S2 — 行为降级**
- [#10114](https://github.com/zeroclaw-labs/zeroclaw/issues/10114)（p1）— `max_tool_result_chars` 是固定的 50,000 默认值，与模型实际上下文窗口无关。
- [#10116](https://github.com/zeroclaw-labs/zeroclaw/issues/10116)（p1，`follow-up`）— 过大的工具结果按字节从中间向外截断；应像 `web_fetch` 那样溢出到文件句柄。
- [#11745](https://github.com/zeroclaw-labs/zeroclaw/issues/11745) — Email 渠道未按文档存储入站附件。
- [#11749](https://github.com/zeroclaw-labs/zeroclaw/issues/11749) — Email 渠道会回复自动回复、退信和邮件列表邮件，可能导致 **邮件循环**。
- [#11760](https://github.com/zeroclaw-labs/zeroclaw/issues/11760) — Email 渠道在每次重启后都会重新回复活跃邮件（活动模式从未设置 `\Seen`）。
- [#11761](https://github.com/zeroclaw-labs/zeroclaw/issues/11761) — Email 渠道会漏掉 fetch 期间到达的邮件，直到下一次 IDLE 超时（默认 29 分钟）。
- [#11764](https://github.com/zeroclaw-labs/zeroclaw/issues/11764) — WhatsApp Web 会静默丢弃未转写的语音笔记。
- [#11773](https://github.com/zeroclaw-labs/zeroclaw/issues/11773) — 区域操作系统语言环境（`fr_FR`、`es_ES`、`ja_JP`）回退到英语。
- [#11771](https://github.com/zeroclaw-labs/zeroclaw/issues/11771) — Bootstrap 文件上限不一致：gateway 轮次为 20,000 字符，而 channel/agent-loop 轮次为 6,000 字符（使用 `compact_context` 时）。

**S3 — 轻微**
- [#11769](https://github.com/zeroclaw-labs/zeroclaw/issues/11769) — 四个渠道测试硬编码英文通知文本，在其他语言环境下失败。
- [#11758](https://github.com/zeroclaw-labs/zeroclaw/issues/11758) — WhatsApp Web 在其监听器停止后仍会继续打印配对二维码约 2.5 分钟。

**是否有修复 PR？** 部分有。[#11297](https://github.com/zeroclaw-labs/zeroclaw/pull/11297) 恢复了 `keep_tool_context_turns` 语义，[#10391](https://github.com/zeroclaw-labs/zeroclaw/pull/10391) 收紧了有界 delegate 的工作区/工具/命令策略，[#11672](https://github.com/zeroclaw-labs/zeroclaw/pull/11672) 修复了 JPEG 准入投影，Wasmtime/Rustls 公告也已修补（#11755）。**对于 email 集群或 WhatsApp 集群缺陷（#11745、#11749、#11760、#11761、#11764、#11758）没有可见的修复 PR，这是今天最令人担忧的稳定性缺口。**

## 6. 功能请求与路线图信号

今天更新的具体功能请求，在存在发布标签时映射到这些标签：

- **发布/打包：** [#7952](https://github.com/zeroclaw-labs/zeroclaw/issues/7952) — 可选的广泛渠道预构建包（`channels-full`）。
- **Agent 可观测性：** [#10531](https://github.com/zeroclaw-labs/zeroclaw/issues/10531)、[#8763](https://github.com/zeroclaw-labs/zeroclaw/issues/8763)、[#10426](https://github.com/zeroclaw-labs/zeroclaw/issues/10426)、[#11743](https://github.com/zeroclaw-labs/zeroclaw/issues/11743) — 面向 delegate、ZeroCode、Telegram 和 Slack 的进度/思考流。
- **RFC：** [#11735](https://github.com/zeroclaw-labs/zeroclaw/issues/11735) — 在轮次边界对工具和 delegate 结果进行有界处理（直接解决与 #10114/#10116 相同的痛点）。
- **Email 渠道加固：** [#11747](https://github.com/zeroclaw-labs/zeroclaw/issues/11747)（SMTP STARTTLS / Office 365:587），[#11751](https://github.com/zeroclaw-labs/zeroclaw/issues/11751)（用于私有 CA 的 `tls_ca_cert_path`）。
- **配置/UX：** [#10665](https://github.com/zeroclaw-labs/zeroclaw/issues/10665)（可配置的 ZeroCode 每窗格会话限制），[#11753](https://github.com/zeroclaw-labs/zeroclaw/issues/11753)（可配置的确认回应表情）。

**对下一个版本（v0.8.6，然后是 v0.9.0）的预测：** 最强的近期信号是 **WASM 插件生命周期**——六个堆叠的、标记为 `release:v0.8.6` 的 PR（#11236、#11261、#11262、#11309、#11347、#11311），加上已落地的安全补丁（#11755）。可能一同推进的还有：十一项内置工具默认配置（#11221）以及 #7432 跟踪器中的 runtime 交付项。OIDC（#8289）和多用户隔离（#8290）明确为 `release:v0.9.0`，并且仍是收尾跟踪器而非新工作。轮次边界有界化 RFC（#11735）如果被接受，可能会在 v0.9.x 周期中被纳入。

## 7. 用户反馈摘要

**真实用户表达的痛点：**

- **长时间运行的轮次静默。** Telegram（#10426）上的运维人员以及一般性的委派代理（#10531）描述了因没有中间进度浮现而“看起来卡住”的请求。这是各 issue 中重复次数最多的 UX 抱怨。
- **Email 渠道无法获得生产环境信任。** 一位贡献者（RustLangLatam）在两天内提交了五个 email issue，涵盖附件、自动回复循环、重启后的 `\Seen` 状态、IDLE 超时邮件丢失、STARTTLS 以及私有 CA 信任。这读起来像是一位运维人员尝试真实邮箱部署并撞了墙。
- **配置意外。** #7952（渠道不在精简预构建包中）和 #11742（`custom` 提供商静默默认回退到文本）都反映了默认值不符合文档预期时的困惑。#11771（不同轮次路径的 bootstrap 上限不同）属于同一类问题：“配置说一套，runtime 做另一套。”
- **本地化被视为二等公民。** #11773 和 #11769 显示非英语语言环境（`fr_FR`、`es_ES`、`ja_JP`）降级为英语或导致测试失败。

**满意信号：** 贡献者参与度高，正在产出详细、范围明确的缺陷报告，并附带文件/行号引用；安全补丁和活跃的 `p1` 进行中项目表明在高严重性路径上具有响应能力。**不满意**集中在渠道可靠性和可见性上——这些是今天数据中 PR 覆盖最少的领域。

## 8. 待办积压观察

需要维护者关注的长期存在或高重要性事项：

- [#7952](https://github.com/zeroclaw-labs/zeroclaw/issues/7952) — 自 **2026-06-19** 起开放，8 条评论，尽管有 `status:accepted` 和 `risk:high`，却被标记为 `status:icebox`。该窗口内评论数最高的 issue，但没有可见进展。
- [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) — 自 **2026-06-09** 起开放，是 v0.8.6/v0.9.0 runtime/gateway 跟踪器；实际上决定了接下来两个版本的发布。
- [#8289](https://github.com/zeroclaw-labs/zeroclaw/issues/8289) 和 [#8290](https://github.com/zeroclaw-labs/zeroclaw/issues/8290) — 均自 **2026-06-24** 起开放，`topic:identity-access`、`risk:high`，被标记为 v0.9.0 的收尾跟踪器。它们需要明确的批准或关闭，以解除下游规划阻塞。
- [PR #10430](https://github.com/zeroclaw-labs/zeroclaw/pull/10430) — Gemini 语音到语音渠道，自 **2026-08-28** 起开放，`needs-author-action`、`status:parking-lot`、`size:XL`。
- [PR #10391](https://github.com/zeroclaw-labs/zeroclaw/pull/10391) — 有界 delegate 策略执行，自 **2026-08-26** 起开放，`needs-author-action`；与 delegate 边界 RFC（#11735）直接相关。
- [PR #10499](https://github.com/zeroclaw-labs/zeroclaw/pull/10499) — 持久化配置写入验证，自 **2026-08-31** 起开放，`needs-author-action`、`risk:high`。
- [PR #11221](https://github.com/zeroclaw-labs/zeroclaw/pull/11221) — 十一项内置工具默认配置，自 **2026-09-29** 起开放，`needs-author-action`、`release:v0.8.6` **和** `release:v0.9.0`；一项发布门控变更，尽管积极 rebase 但仍未合并。
- [PR #11304](https://github.com/zeroclaw-labs/zeroclaw/pull/11304) — 插件出口拒绝日志记录，明确为 `status:blocked`。
- [#11130](https://github.com/zeroclaw-labs/zeroclaw/issues/11130) — S1 DSML 解析器缺陷，`status:in-progress`，但没有可见的关联 PR；鉴于其阻塞工作流的严重程度，值得检查状态。

**底线：** 项目的瓶颈是审查吞吐量，而不是贡献量。面对 48 个开放 PR、2 个关闭，以及一个在 `needs-author-action` 中老化的发布门控项（#11221），最健康的下一步将是对 v0.8.6 插件栈和未处理的 email/WhatsApp 缺陷集群进行分诊。

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*