# OpenClaw 生态日报 2026-10-12

> Issues: 500 | PRs: 500 | 覆盖项目: 5 个 | 生成时间: 2026-10-12 08:27 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw 项目深度报告

# OpenClaw 项目摘要 — 2026-10-12

## 1. 今日概览

OpenClaw 依然非常活跃：过去 24 小时内共有 500 个 issue 更新（357 个打开/活跃，143 个已关闭），另有 500 个 PR 更新（363 个打开，137 个已合并/关闭），并发布了两个新版本。最新版本几乎全部聚焦于更新可靠性：v2026.10.2 修复了从稳定版 2026.10.1 进行干净的原地升级，v2026.10.5-beta.1 强化了跨设备、文件系统、Windows 提权以及中断激活场景下的更新。然而，issue 跟踪器显示仍持续存在高严重级稳定性问题，尤其是内存泄漏、僵尸进程、更新失败、会话状态丢失，以及插件捕获性能回退。因此项目健康状况喜忧参半：维护吞吐量很高，但未解决的 P0/P1 issue 仍在影响生产部署，尤其是在 Windows、npm 全局安装，以及复杂的容器/文件系统环境下。

## 2. 版本发布

### v2026.10.2
链接：https://github.com/openclaw/openclaw/releases/tag/v2026.10.2

发布说明要点：
- 从稳定版 2026.10.1 进行的首次升级现在可以顺利完成。
- 成功的原地更新不再以错误告终。
- Windows、克隆文件系统以及私有 Node 的更新路径现在能够完成，而不是卡住或回滚。
- 引用修复：#168849、#169031。

### v2026.10.5-beta.1
链接：https://github.com/openclaw/openclaw/releases/tag/v2026.10.5-beta.1

发布说明要点：
- 在跨设备、文件系统、Windows 提权以及中断激活场景下实现更安全的更新。
- 可从过期租约或已变更证据中恢复，而不是让安装陷入停滞或阻止后续更新。
- 引用修复：#164636、#164769、#165257、#165863，另有一个被截断的引用。

**破坏性变更 / 迁移说明：** 提供的发布说明未提及明确的破坏性变更。主要迁移信号是更新路径的韧性得到改善，这应能减少升级失败时的手动恢复。尽管如此，许多未关闭的 issue 仍报告在 npm 全局、Windows、LXC 以及 Windows 多代理安装中出现更新失败，因此运维人员在重大升级前仍应备份状态。

## 3. 项目进展

- **今日合并/关闭的 PR：** 137 个。
- **今日关闭的 issue：** 143 个。

已关闭的 issue 表明修复已在多个领域落地：
- **更新可靠性：** #164113 — 在非特权 LXC 中，更新在 `updater-runtime-retention` 处因 FICLONE EPERM 失败；已通过可排队修复关闭。链接：https://github.com/openclaw/openclaw/issues/164113
- **包恢复：** #167506 — 删除的临时交接数据库阻塞了包恢复和后续更新；已关闭。链接：https://github.com/openclaw/openclaw/issues/167506
- **会话状态：** #69118 — 由于 `groupIntro` 漂移，Claude CLI 会话在群组频道中每轮都会重置；已作为已修复关闭。链接：https://github.com/openclaw/openclaw/issues/69118
- **模型请求序列化：** #123265 — `role:"custom"` 运行时上下文载体在每个请求中都被序列化为末尾的 `user` 消息；已关闭。链接：https://github.com/openclaw/openclaw/issues/123265
- **内存/CPU 饱和：** #136175 — 完整的本地内存重建索引使 CPU 饱和并阻塞诊断；已关闭。链接：https://github.com/openclaw/openclaw/issues/136175
- **子代理幂等性：** #37446 — 子代理超时恢复创建了重复的 API 提交；已关闭。链接：https://github.com/openclaw/openclaw/issues/37446

大量开放 PR 活动由维护者 `steipete` 推动，重点在于架构简化以及将同步 SQLite 工作移出 Gateway 线程：
- #169648 — 简化状态与进程 worker 生命周期。https://github.com/openclaw/openclaw/pull/169648
- #169584 — 通过 worker 准备执行策略。https://github.com/openclaw/openclaw/pull/169584
- #169580 — 将 Gateway 状态消费者迁至现有 worker。https://github.com/openclaw/openclaw/pull/169580
- #169566 — 通过 worker 路由会话 fork、读取和清理。https://github.com/openclaw/openclaw/pull/169566
- #169735 — 避免冗余的提醒和会话快照 SQL。https://github.com/openclaw/openclaw/pull/169735
- #169707 — 减少 Control UI 冷加载聊天渲染。https://github.com/openclaw/openclaw/pull/169707
- #169709 — 减少空闲 Gateway 事件循环采样工作。https://github.com/openclaw/openclaw/pull/169709

还有一个重要的开放 P1 PR，旨在实现更安全的更新：
- #144005 —“在迁移前备份状态，并在回滚时恢复。”仍处于打开状态，标记为 `needs proof`，范围较大。https://github.com/openclaw/openclaw/pull/144005

## 4. 社区热点

按评论数排序的最活跃 issue：

1. **#91588 — P0 Gateway 内存泄漏（38 条评论）**  
   RSS 在数天内从约 350 MB 增长到 15.5 GB，导致反复 OOM 崩溃。没有新的修复 PR。  
   https://github.com/openclaw/openclaw/issues/91588

2. **#159356 — llama.cpp 管理器报告就绪，但 embedding 子进程已退出（26 条评论）**  
   Embedding 请求返回 HTTP 500“Failed to read connection”；严重级为 P2，但会影响会话状态和崩溃循环。  
   https://github.com/openclaw/openclaw/issues/159356

3. **#97616 — P1 未回收的 hook/tool 子进程（18 条评论）**  
   主 `openclaw` 进程下僵尸进程累积，导致运行时性能下降。  
   https://github.com/openclaw/openclaw/issues/97616

4. **#148707 — P1 回复因“no active tool authority snapshot”而丢失（18 条评论）**  
   2026.9.4 回归：第二次运行挤掉了进行中的回合，导致回复丢失。  
   https://github.com/openclaw/openclaw/issues/148707

5. **#160959 — P0 Gateway 在捕获大型外部插件时阻塞数分钟（15 条评论）**  
   2026.9.6 回归；启动时以及模型运行时重新发布时事件循环被阻塞。  
   https://github.com/openclaw/openclaw/issues/160959

6. **#86996 — Active Memory + Codex app-server 路径导致延迟和停滞（15 条评论，已关闭）**  
   响应延迟过长、hook 超时、启动中止以及 Gateway 事件循环停滞。  
   https://github.com/openclaw/openclaw/issues/86996

7. **#157989 — P1 插件源码捕获每次 CLI 命令重写约 1.1–1.4 GB（15 条评论）**  
   每次 Gateway 启动约 6.5 GB 且无复用；造成严重 SSD 磨损。  
   https://github.com/openclaw/openclaw/issues/157989

**根本需求：** 用户持续要求资源稳定性、更新可靠性、插件性能以及会话/消息持久性。评论数最高的 issue 并非功能请求，而是影响生产的可靠性缺口，其中许多标记为 `clawsweeper:no-new-fix-pr` 或 `needs-maintainer-review`，这表明瓶颈是维护者关注度，而不是缺少报告。

## 5. 缺陷与稳定性

根据提供的数据按严重级排序：

### P0 / 严重
- **#91588 — Gateway 内存泄漏，RSS 350 MB → 15.5 GB，反复 OOM 崩溃。** 打开，38 条评论，无新修复 PR。  
  https://github.com/openclaw/openclaw/issues/91588
- **#160959 — Gateway 在捕获大型外部插件时事件循环阻塞数分钟（2026.9.6 回归）。** 打开，15 条评论，无新修复 PR。  
  https://github.com/openclaw/openclaw/issues/160959
- **#156112 — `openclaw update` 在全局安装交换时失败，而直接执行 `npm install -g` 可成功。** 打开，14 条评论，标记为 `manual-only`。  
  https://github.com/openclaw/openclaw/issues/156112
- **#167652 — Windows Gateway 在升级到 2026.9.9 后挂起，尽管 Doctor 报告已验证重启。** 打开，8 条评论，P0，发布阻塞项。  
  https://github.com/openclaw/openclaw/issues/167652
- **#126821 — SQLite 损坏在全新重建的 DB 上于 15–24 小时内复发（WSL2），包括 gateway 瘫痪模式。** 打开，9 条评论。  
  https://github.com/openclaw/openclaw/issues/126821
- **#112475 — 设备配对在移除后恢复失败。** 打开，8 条评论，P0。  
  https://github.com/openclaw/openclaw/issues/112475
- **#155191 — 2026.9.5 中的原生内存泄漏：RSS 每 30 秒增长约 1 GiB，而 V8 堆保持稳定。** 打开，8 条评论。  
  https://github.com/openclaw/openclaw/issues/155191

### P1 / 高
- **#97616 — 泄漏/未回收的 hook 和 tool 子进程导致僵尸进程累积和运行时性能下降。** 打开，18 条评论。  
  https://github.com/openclaw/openclaw/issues/97616
- **#148707 — 在进行中的回合被挤掉时，回复因“Reply operation has no active tool authority snapshot”而丢失。** 打开，18 条评论。  
  https://github.com/openclaw/openclaw/issues/148707
- **#145309 — `claude-cli` 后端忽略 `CLAUDE_CONFIG_DIR`，导致 transcript 缺失引发重置和故障转移。** 打开，12 条评论。  
  https://github.com/openclaw/openclaw/issues/145309
- **#104719 — memory-wiki supplement 的穷举回退忽略工具截止时间。** 打开，12 条评论。  
  https://github.com/openclaw/openclaw/issues/104719
- **#168955 — 2026.10.1 在 Windows 上的启动回归：就绪约 1030 秒，而 2026.9.9 约 410 秒。** 打开，10 条评论。  
  https://github.com/openclaw/openclaw/issues/168955
- **#158788 — Cron agent 回合在 Windows 上设置时因 `DataCloneError` 失败（2026.9.6）。** 打开，8 条评论。  
  https://github.com/openclaw/openclaw/issues/158788
- **#159512 — `claude-cli`：每次 subagent 完成都会使请求方 CLI 会话失效两次。** 打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/159512
- **#107244 — WhatsApp 群组消息从未进入入站处理；私信正常。** 打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/107244
- **#126950 — 对于非插件调用方，Telegram 不透明回调载荷（`tgcb1:`）被静默丢弃。** 打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/126950
- **#54488 — 会话通道饥饿：followup drain 独占通道，阻塞入站调度 20–30 分钟。** 打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/54488

### P2 / 中等
- **#159356 — llama.cpp 管理器报告就绪，但 embedding 子进程已退出；embedding 请求返回 HTTP 500。** 打开，26 条评论。  
  https://github.com/openclaw/openclaw/issues/159356
- **#105528 — `exec`/`read` 工具在 Windows 上静默返回空输出（2026.6.x 回归）。** 打开，10 条评论。  
  https://github.com/openclaw/openclaw/issues/105528
- **#120385 — Code Mode 工具目录在 scheduled/cron 和 memory-flush 回合中不完整；MCP 工具缺失。** 打开，10 条评论。  
  https://github.com/openclaw/openclaw/issues/120385
- **#164394 — Control UI WebChat 在滚动到历史记录时 transcript 持续抖动。** 打开，9 条评论。  
  https://github.com/openclaw/openclaw/issues/164394
- **#162914 — 使用 rootless Podman 沙箱时间歇出现“Aborted | sandbox_provisioning”失败。** 打开，9 条评论。  
  https://github.com/openclaw/openclaw/issues/162914
- **#48709 — Gemini 2.5 Pro：`textSignature` 膨胀、think 标签、文本/工具混合导致会话失败。** 打开，8 条评论。  
  https://github.com/openclaw/openclaw/issues/48709
- **#99659 — 连接 companion app 后 OpenClaw 被 OOM killer 杀死。** 打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/99659
- **#166894 — 在 Windows 多代理安装中，Gateway 重启有时会挂起 30 多分钟到 2 小时（2026.9.8）。** 打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/166894
- **#124759 — 启用“show reasoning and tool activity”时 iOS app 严重卡顿。** 打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/124759

**修复 PR 状态：** 许多高严重级 issue 带有 `clawsweeper:no-new-fix-pr`，这意味着在提供的数据中看不到关联的修复 PR。一些已关闭的 issue —— #164113、#167506、#69118、#123265、#136175、#37446 —— 表明近期已有针对更新恢复、会话哈希、请求序列化和子代理幂等性的修复落地。

## 6. 功能请求与路线图信号

- **#50093 — WhatsApp：重新连接后回填错过的消息。** P1，打开，14 条评论。用户会在中断窗口期间丢失消息。  
  https://github.com/openclaw/openclaw/issues/50093
- **#16670 — 入门向导应将 Memory/Embedding 设置列为强制步骤。** P2，打开，10 条评论。内存持久化被视为核心功能，但设置流程未提及 embedding 配置。  
  https://github.com/openclaw/openclaw/issues/16670
- **#14785 — 减少工具 schema token 开销（约 3,500 token/会话）。** P2，打开，11 条评论。每个会话都有固定的上下文税。  
  https://github.com/openclaw/openclaw/issues/14785
- **#38568 — 将上下文窗口百分比注入系统提示词的运行时部分。** P3，打开，8 条评论。对管理上下文用量的代理很有用。  
  https://github.com/openclaw/openclaw/issues/38568
- **#111143 — 允许在 Control UI 侧边栏固定插件标签页。** P3，打开，6 条评论。  
  https://github.com/openclaw/openclaw/issues/111143
- **#74481 — 从配置的 provider `/v1/models` 动态刷新目录。** P2，打开，7 条评论。解决硬编码模型目录和代理环境问题。  
  https://github.com/openclaw/openclaw/issues/74481
- **#42591 — 模块化 `install.sh` 以提升可维护性。** P3，打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/42591
- **#76247 — 跨 cron、sessions_spawn、channel_inbound、gateway_agent、cli_agent、acp_downstream 的原生调度落地 ACK / receiver-entry 遥测。** P2，打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/76247
- **#124911 — Compaction 的 `reserveTokensFloor` 忽略模型上下文窗口；存在自适应 helper 但未被使用。** P2，打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/124911

**路线图预测：** 鉴于最新版本以更新为重点，下一个稳定化窗口可能会针对会话/消息持久性和插件捕获性能。信号最强的功能请求是内存/embedding 设置（#16670）、token 开销削减（#14785）以及 WhatsApp 回填（#50093）。大型开放 PR #144005（用于更新备份/回滚）一旦满足证明要求，很可能成为下一个稳定版的候选。插件 UI 自定义和动态模型目录刷新优先级较低，但被反复请求。

## 7. 用户反馈摘要

- **对稳定性的严重不满：** 用户报告 OOM 崩溃、内存泄漏、僵尸进程、SQLite 损坏以及 gateway 挂起，使部署无法使用（#91588、#155191、#97616、#126821、#167652）。
- **更新之痛普遍存在：** `openclaw update` 在 npm 全局安装、Windows、LXC 容器和克隆文件系统上失败，而直接 npm 安装往往可以成功（#156112、#168955、#167652、#164113、#167506）。
- **性能与硬件担忧：** 插件源码捕获每条命令、每次 Gateway 启动都会重写数 GB 数据，导致 SSD 磨损和数分钟的事件循环阻塞（#160959、#157989）。
- **消息丢失和会话连续性至关重要：** 用户在负载或重连下丢失回复、WhatsApp 群组消息、Telegram 回调和会话状态（#148707、#107244、#126950、#54488）。
- **平台特定摩擦：** Windows 用户面临工具输出为空、启动缓慢、挂起和 `DataCloneError`；iOS 用户在启用 reasoning/tool activity 时出现卡顿（#105528、#166894、#158788、#124759）。
- **维护者响应速度令人担忧：** 许多高影响 issue 被标记为 `needs-maintainer-review` 或 `needs-product-decision`，并持续打开数周或数月。
- **一位用户放弃了部署：** #88087 记录了 DigitalOcean droplet 上长时间运行后台任务的糟糕 UX 以及静默 cron 唤醒失败，最终导致用户将其拆除。https://github.com/openclaw/openclaw/issues/88087
- **积极信号：** 项目非常活跃，版本发布频繁，今天还关闭了若干长期存在的 issue，包括更新和会话状态修复。用户继续提供包含环境细节的详细报告。

## 8. 积压事项观察

重要 issue/PR：陈旧、高影响，且仍在等待维护者关注或产品决策：

- **#91588 — P0 Gateway 内存泄漏，38 条评论，创建于 2026-06-09。** 无新修复 PR。最高优先级积压项。  
  https://github.com/openclaw/openclaw/issues/91588
- **#97616 — P1 僵尸进程累积，创建于 2026-06-29。** 打开，18 条评论。  
  https://github.com/openclaw/openclaw/issues/97616
- **#104719 — P1 memory-wiki supplement 忽略截止时间，创建于 2026-07-11。** 打开，12 条评论。  
  https://github.com/openclaw/openclaw/issues/104719
- **#126821 — P0 反复出现的 SQLite 损坏，创建于 2026-08-20。** 打开，9 条评论。  
  https://github.com/openclaw/openclaw/issues/126821
- **#155191 — P0 原生内存泄漏，创建于 2026-09-21。** 打开，8 条评论。  
  https://github.com/openclaw/openclaw/issues/155191
- **#167652 — P0 Windows Gateway 升级后挂起，创建于 2026-10-09。** 打开，8 条评论。  
  https://github.com/openclaw/openclaw/issues/167652
- **#50093 — P1 WhatsApp 回填，创建于 2026-03-19。** 需要产品决策和现场复现。  
  https://github.com/openclaw/openclaw/issues/50093
- **#54488 — P1 会话通道饥饿，创建于 2026-03-25。** 打开，7 条评论。  
  https://github.com/openclaw/openclaw/issues/54488
- **#14785 — P2 工具 schema token 开销，创建于 2026-02-12。** 打开，11 条评论。  
  https://github.com/openclaw/openclaw/issues/14785
- **#16670 — P2 入门流程中的内存/embedding 设置，创建于 2026-02-15。** 打开，10 条评论。  
  https://github.com/openclaw/openclaw/issues/16670
- **#38568 — P3 系统提示词中的上下文窗口百分比，创建于 2026-03-07。** 打开，8 条评论。  
  https://github.com/openclaw/openclaw/issues/38568
- **#144005 — P1 PR：在迁移前备份状态，并在回滚时恢复。** 自 2026-09-10 起打开，范围较大，标记为 `needs proof`。  
  https://github.com/openclaw/openclaw/pull/144005
- **#158387 — P2 PR：拒绝没有消息目标的频道名称。** 自 2026-09-25 起打开，标记为可供维护者查看。  
  https://github.com/openclaw/openclaw/pull/158387

**整体积压担忧：** 最关键的可靠性 issue 并非新问题；它们已积累数月，而项目在此期间发布了以更新为重点的版本。维护者评审和产品决策带宽似乎是关闭最高影响 P0/P1 缺口的核心制约因素。

---

## 横向生态对比

# 跨项目对比报告——个人 AI 助手 / Agent 开源生态系统
**时间窗口：** 2026-10-11 → 2026-10-12 | **项目：** OpenClaw、Hermes Agent、IronClaw、QwenPaw、ZeroClaw

> **方法说明：** 活跃度数据直接取自各项目的 24 小时摘要。健康度评分为分析师指定的综合评分（1–10），权衡活跃吞吐量、关闭率、发布节奏、关键积压严重程度和社区参与度——它们不是厂商或 GitHub 提供的指标。OpenClaw 的 Issue/PR 数量似乎触及 API 的 500 条上限，应理解为“≥500”。

---

## 1. 生态系统概览

个人 AI 助手 / Agent 开源生态正在**功能上趋同，结构上分化**。几乎每个活跃项目都在独立重建同一核心栈——Gateway、插件/扩展运行时、MCP 工具管线、记忆/压缩层、多渠道入口以及提供商无关的模型路由——但在语言、打包方式和目标用户押注上差异巨大。五个项目共同的主导未解问题不是能力，而是**运行可靠性**：安装/更新脆弱性、Gateway 资源泄漏、会话持久性和插件沙箱成本出现在每个积压清单中，而参与度最高的社区线程是生产事故报告，而非功能请求。第二个跨领域约束是**维护者审查带宽**：QwenPaw 和 ZeroClaw 的贡献者供给健康，但合并延迟——而非想法稀缺——才是约束瓶颈。

---

## 2. 活跃度对比

| 项目 | Issue（24 小时） | PR（24 小时） | 发布状态 | 健康度评分 |
|---|---|---|---|---|
| **OpenClaw** | 约 500 条更新（357 条开启/活跃，143 条关闭 · 28.6%） | 约 500 条更新（363 条开启，137 条合并/关闭 · 27.4%） | **2 个版本：** v2026.10.2（稳定版，更新修复），v2026.10.5-beta.1 | **6.5 / 10 — 好坏参半** |
| **Hermes Agent** | 50 条更新（10 条关闭 · 20%） | 50 条更新（18 条合并/关闭 · 36%） | **v0.21.7** 补丁（汇总约 204 个 PR） | **8.0 / 10 — 强劲** |
| **IronClaw** | 0 | 7 条更新（3 条合并，4 条开启）——**全部为 dependabot** | 无 | **6.0 / 10 — 安静/稳定** |
| **QwenPaw** | 13 条触及（至少 1 条关闭，包括 #7311） | 23 条更新（13 条合并/关闭 · 57%，10 条开启） | 无 | **7.0 / 10 — 流程良好，发布承压** |
| **ZeroClaw** | 16 条触及（**0 条关闭**） | 45 条更新（**2 条关闭** · 4.4%，43 条开启） | 无 | **5.0 / 10 — 受审查瓶颈制约** |

**解读表格：** OpenClaw 的原始吞吐量约为最接近同行的 **10 倍**，比 IronClaw 高出两个数量级，但其关闭率（约 27–29%）落后于 Hermes 和 QwenPaw，因为流入量巨大。ZeroClaw 表现出相反的病理——贡献者流入高，但 **合并:开启比为 2:43**——表明瓶颈在审查而非需求。QwenPaw 的 57% PR 关闭率是这一组中最健康的，得益于有意识地清理数月积压，尽管已有一批合并修复排队，却未发布任何版本。

---

## 3. OpenClaw 的定位

**规模优势具有决定性，但也是一把双刃剑。** OpenClaw 在 24 小时内 Issue+PR 合计约 1,000 次触及，令 Hermes（约 100）、QwenPaw（约 36）、ZeroClaw（约 61）和 IronClaw（约 7）相形见绌。它占据事实上的“核心参考”地位，拥有生态中最广的平台覆盖面：Windows、macOS、iOS、WSL2、LXC、Docker、npm-global、克隆文件系统以及多 Agent Windows 安装都出现在其跟踪器中。

**与同行的技术路径对比：**

| 维度 | OpenClaw | 同行对比 |
|---|---|---|
| 运行时 | Node/TS 单体 Gateway + 同步 SQLite 状态 | ZeroClaw/IronClaw 为 Rust；Hermes 为 Python + 桌面外壳 |
| 进行中的重构 | 通过 worker 将 SQLite/状态工作**移出 Gateway 线程**（steipete PR 链 #169648/#169580/#169566） | ZeroClaw 通过**运行时/Gateway 分离**追求等效目标（v0.9.0 RFC #7432） |
| 插件模型 | 外部插件的源码捕获（每个 CLI 命令重写 **1.1–1.4 GB**，每次 Gateway 启动约 6.5 GB——#157989） | IronClaw/ZeroClaw 使用 **WASM 沙箱**；QwenPaw 转向子进程隔离通道（#8187） |
| 会话模型 | 会话泳道 + 工具权限快照（#148707 回复丢失回归的来源） | Hermes 使用带空闲回收器的桌面后端池；ZeroClaw 追求持久化对等 Agent 轮次（#9597） |

**社区规模对比：** OpenClaw 是唯一一个跟踪器因 API 限制而饱和的项目。Hermes 是可信的第二梯队。QwenPaw 和 ZeroClaw 拥有积极参与、技术成熟但规模小得多的用户基础。IronClaw 在窗口期内显示 **零社区活动**。

**同行胜过它之处：** OpenClaw 最关键的缺陷已经陈旧——P0 Gateway 内存泄漏（#91588）自 2026-06-09 起一直开启，有 38 条评论且没有修复 PR，P0 SQLite 损坏（#126821）自 2026-08-20 起也是如此。Hermes 和 QwenPaw *今天* 关闭了相对严重的问题。OpenClaw 的问题总体上不是响应能力，而是**在聚焦更新的发布工作中对长期存续 P0 的分诊优先级排序**。

---

## 4. 共同技术关注领域

这些需求独立出现在 **三个或更多** 项目中——有力证明这是真正的生态系统级需求，而非单项目偏好。

| 跨领域需求 | OpenClaw | Hermes | QwenPaw | ZeroClaw | IronClaw |
|---|---|---|---|---|---|
| **安装/更新可靠性与权限** | npm-global、Windows、LXC 失败（#156112、#167652）；回滚 PR #144005 | Windows 提权安装的所有权阻碍后续更新（#126860） | Docker 安装损坏（#8179）；快速开始 PR | 堆叠的快速开始/安装 PR（#11347） | CI/依赖项升级 |
| **Gateway 资源治理（FD/进程/内存）** | RSS 350 MB→15.5 GB（#91588）；僵尸子进程（#97616） | SQLite FD 泄漏 → 256-fd launchd 限制（#96027） | 桌面后端池泄漏（#105239，已关闭） | Gateway body-limit/timeout 接线（#7692） | wasmtime 风险 PR #7834 |
| **记忆正确性与透明度** | memory-wiki 截止日期（#104719）；embedding 设置引导（#16670） | `/memory approve all` 破坏性覆盖（#136738） | `none` 记忆后端（#5732）；整合类别（#11657） | 默认配置 Core memory 软隐藏（#11655） | — |
| **插件生命周期与沙箱** | 捕获成本/SSD 磨损（#157989、#160959） | 插件自动安装/回填（#137033、#134537） | 插件市场版本路由（#5661、#5612） | WASM 插件托管链（8+ PR） | wasmtime 组升级 |
| **会话/状态持久性** | 泳道饥饿（#54488）；回复丢失（#148707） | Telegram 主题误路由（#136567）；`/resume` 编号（#60136） | 流式工具调用 404（#7016，59d）；僵尸 `running` 状态（#8173） | 持久化对等 Agent 轮次（#9597） | — |
| **提供商怪癖处理** | claude-cli `CLAUDE_CONFIG_DIR`（#145309） | DeepSeek `max_tokens` 截断（#126305）；Z.AI `reasoning_effort`（#133595） | Codex 后端；Tavily 密钥暴露面（#8174） | 路由 RFC（#11074）；溢出码（#11659） | — |
| **审批/安全透明度** | 工具权限快照 | 混淆命令审批门（#25793）；ACP 中的 MCP 审批（#135522） | 审批竞态（#8188）；会话范围审批（#8182） | 命令策略继承（#11667）；SSRF 传播（#10290） | — |
| **多 Agent 编排** | 子 Agent 幂等性（#37446） | Kanban worker 工作区（#91568） | Coding-CLI worker 容器（#8156） | Swarms（#10025）；Dark Factory（#11664）；A2A（#11254） | — |

**MCP** 明确出现在 OpenClaw、Hermes 和 QwenPaw 中（加固、审批桥接、导入验证）。**A2A** 目前是 ZeroClaw 的差异化优势。**将 Windows 作为一等平台**是 OpenClaw、Hermes 和 QwenPaw 共同的痛点。

---

## 5. 差异化分析

| | **OpenClaw** | **Hermes Agent** | **IronClaw** | **QwenPaw** | **ZeroClaw** |
|---|---|---|---|---|---|
| **主要焦点** | 广度：每个渠道、操作系统、部署模式 | 桌面优先助手 + 团队工作流（Kanban、MS Store） | 安全沙箱化执行基础设施 | 自托管 Console + 插件市场 + 多 Agent Docker | 协议优先的多 Agent 运行时 |
| **目标用户** | 高级用户、自托管者、集成者 | 消费级桌面用户、小团队 | Rust/基础设施开发者 | 自托管运维者、CJK/HarmonyOS 社区 | 高级多 Agent 构建者 |
| **语言/技术栈** | TS/Node、SQLite | Python + 桌面外壳、ACP 桥接 | Rust、tokio、wasmtime | 服务器 + Web Console、Codex 后端 | Rust |
| **插件模型** | 源码捕获（高成本） | 插件目录 + 自动安装 | WASM 沙箱 | 版本路由插件市场 | WASM + 能力边界 |
| **独特押注** | 参考实现与广泛覆盖 | 发布纪律 + 桌面打磨 | 最小人工干预面；通过 `claude-code-action` 实现 CI 自动化 | Console UX + 无障碍层级（#8137） | A2A 互操作性 + RFC 驱动设计 |

**架构断层线：** 基于 Rust 的项目（IronClaw、ZeroClaw）正趋同于将 **WASM 沙箱 + 能力边界** 作为安全原语，而 JS/Python 项目（OpenClaw、Hermes）仍在管理 **进程级隔离和源码捕获成本**。OpenClaw 的插件模型是生态中已知最大的效率负债；Rust 项目在这一维度上实际上领先一代。

---

## 6. 社区势能与成熟度

**第一梯队——超大规模、容量受限：** **OpenClaw。** 生态中速度最高，但处于应对 P0 积压的被动模式。发布最快（24 小时内 2 个），但发布一直狭隘地聚焦更新，而内存/进程泄漏持续数月。

**第二梯队——积极稳定并持续发布：** **Hermes Agent。** 最均衡的画像：高吞吐量、实际发布了一个版本、关闭了有意义的安全和生命周期问题（#25793、#105239、#127450），以及一波协调进行中的修复（压缩重试、Windows 状态探测、TUI 锁定）。**当前健康度与规模之比最佳。**

**第三梯队——正在改善但发布匮乏：** **QwenPaw。** 贡献流强劲（57% PR 关闭率），积极清理 6 个月前的 PR 积压，并有高质量社区报告——但在 v2.2.x 回归累积（#8173、#8179、#8175）时 **没有发布版本**。最高杠杆的单一行动是发布一个版本。

**第三梯队 b——重设计、受审查瓶颈制约：** **ZeroClaw。** 六个同时进行的高风险 RFC，架构上雄心勃勃，深度参与的高级用户——但 **4.4% 的 PR 关闭率** 和一个停滞 50 天的 wasm 相关 PR（#7834 相关链）。更多是在 *设计* 上迭代，而非 *代码*。

**第四梯队——安静维护：** **IronClaw。** 零 Issue、零人工贡献、仅 dependabot 活动。要么是成熟/冻结的代码库，要么是休眠信号——从本次快照无法判断。

**快速迭代：** OpenClaw、Hermes、QwenPaw。**稳定中：** Hermes、QwenPaw。**设计中：** ZeroClaw。**静止：** IronClaw。

---

## 7. 趋势信号

**1. 可靠性就是产品，而不是功能。** 在全部五个项目中，评论最多的条目都是事件报告：内存泄漏、FD 耗尽、僵尸进程、静默数据损坏。*对开发者而言：* 资源治理（RSS、FD、子进程）、原子状态和崩溃恢复如今是采用的基本门槛——而不是路线图事项。

**2. 更新/安装正在成为一等子系统。** 跨平台提权语义、失败回滚和迁移前备份独立出现在 OpenClaw（PR #144005）、Hermes（#126860）和 QwenPaw（#8179）中。*对开发者而言：* 从第一天起就以原子性和权限边界意识设计更新路径；它是用户流失的前三大来源之一。

**3. 插件沙箱正在转向 WASM 和进程隔离。** IronClaw（wasmtime）、ZeroClaw（WASM 托管链）和 QwenPaw（#8187 子进程隔离）都在远离进程内插件执行。OpenClaw 每个命令 GB 级的源码捕获是一个警示性反例。*对开发者而言：* 在插件 API 稳定之前选择隔离边界——事后改造代价高昂。

**4. 记忆/上下文管理是正确性的新前沿。** 压缩、上下文溢出恢复、token 预算和 RAG 语料库在五个项目中都出现。关键的是，用户抱怨的故障模式是 **静默且破坏性的**——QwenPaw 的二进制损坏（#8179）、ZeroClaw 的默认配置记忆隐藏（#11655）、Hermes 不透明的 `/memory approve all`（#136738）。

**5. 静默失败是被提及最多的单一不满模式。** QwenPaw（#8173、#8175、#8179）、OpenClaw（#159356 embedding 子进程在“ready”时退出）和 Hermes（#135522 静默拒绝审批）都出现相同表述：“没有日志，没有错误。” *对开发者而言：* 可调试性和显式失败暴露面是竞争性功能，尤其对自托管运维者而言。

**6. 协议互操作性正围绕 MCP 和 A2A 整合。** MCP 现在在 OpenClaw、Hermes 和 QwenPaw 中被默认假定；A2A 是 ZeroClaw 的战略押注（#11254）。*对开发者而言：* 针对这些协议构建工具和 Agent 接口，而不是定制传输格式——预期寿命更长。

**7. 多 Agent 编排正在从实验走向产品。** Kanban worker（Hermes）、子 Agent 幂等性（OpenClaw）、worker 容器（QwenPaw）以及 swarms/Dark Factory（ZeroClaw）表明用户正在超越单 Agent 拓扑，需要生命周期、隔离和策略继承原语。

**8. 提供商怪癖仍是未被抽象掉的成本。** DeepSeek、Z.AI、Gemini、claude-cli、Codex 和自定义前缀提供商各自产生项目特定 bug。*对开发者而言：* 假定提供商异质性是永久的；将模型路由、token 限制默认值和凭据路径视为可配置表面，而非常量。

**9. 审查带宽，而非贡献者供给，是生态系统的约束瓶颈。** OpenClaw（维护者标记 `needs-maintainer-review`）、QwenPaw（约 6 个月的 PR 延迟，现在正在清理）和 ZeroClaw（2:43 合并比）都表现出相同病理。

**10. 成本和硬件外部性正在成为 UX 关注点。** OpenClaw 每次启动约 6.5 GB 的插件捕获导致 SSD 磨损（#157989），这是一个早期信号：对于长时间运行的 Agent 部署，**资源效率正在进入面向用户的价值主张**。

---

## 同赛道项目详细报告

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目摘要 — 2026-10-12

范围：基于所提供的 `NousResearch/hermes-agent` 24 小时 GitHub 快照以及所示评论最多的 issue/PR。

## 1. 今日概览

Hermes Agent 显示出高维护活动：50 个 issue 更新，50 个 PR 更新，18 个 PR 合并/关闭，10 个 issue 关闭，并发布新的补丁版本 `v0.21.7`。项目正处于积极的稳定化阶段，许多修复落地于网关、桌面端、插件、会话和平台特定行为。然而，若干 P1/P2 可靠性问题仍然开放，尤其是围绕网关文件描述符泄漏、记忆审批安全性、Windows 权限、Telegram 路由和特定提供商的压缩失败。整体而言，吞吐方面的健康度很强，但开放积压中仍包含高影响的稳定性和平台兼容性问题。

## 2. 发布

### v0.21.7
- 发布日期：October 11, 2026
- 链接：[Hermes Agent v0.21.7](https://github.com/NousResearch/hermes-agent/releases/tag/v0.21.7)
- 提供的说明：补丁版本，汇总了自 `v0.21.6` 以来合并的约 204 个 PR，形成面向桌面应用（macOS Apple 芯片、Windows）、Microsoft Store 和 MSIX、安装器、Docker 以及 Hermes Cloud 的稳定版本。
- 破坏性变更/迁移说明：提供的发布摘录中未说明。除非完整整理说明另有说明，否则应将其视为补丁/稳定汇总版本。
- 注意：源中的发布说明文本在“Full curated notes for this…”之后被截断，因此无法从这些数据中获得完整变更细节。

## 3. 项目进展

今日窗口关闭了 10 个 issue，并合并/关闭了 18 个 PR。可见进展包括：

- [#137033](https://github.com/NousResearch/hermes-agent/pull/137033) — Home Assistant 不再自动安装到从未使用过它的配置文件中；失败的插件自动安装会退避，而不是永久每小时重试。这也解决了 [#132814](https://github.com/NousResearch/hermes-agent/issues/132814)。
- [#127666](https://github.com/NousResearch/hermes-agent/pull/127666) — 桌面端 transcript 重复行修复：当已提交页面已经带有待处理的实时行时，防止一条 assistant 回复被渲染两次。
- 已关闭 issue [#105239](https://github.com/NousResearch/hermes-agent/issues/105239) — 桌面端后端池资源泄漏；已打开聊天的后端不受 `maxBackends` 和空闲回收器约束，导致常驻进程被占用。
- 已关闭 issue [#127450](https://github.com/NousResearch/hermes-agent/issues/127450) — `hermes update` 在拥有许多 checkpoint 项目的主机上不再卡顿约 11 分钟。
- 已关闭 issue [#60136](https://github.com/NousResearch/hermes-agent/issues/60136) — 网关 `/resume <number>` 现在遵循 `/sessions search` 结果编号。
- 已关闭 issue [#110799](https://github.com/NousResearch/hermes-agent/issues/110799) — 无人值守记忆审查不再将 Hermes 生成的压缩交接内容视为用户证据。
- 已关闭 issue [#34046](https://github.com/NousResearch/hermes-agent/issues/34046) — `baoyu-infographic` 技能不再静默绕过 `image_generate`。
- 已关闭安全 issue [#25793](https://github.com/NousResearch/hermes-agent/issues/25793) — 针对 shell 惯用法混淆的危险命令的审批门禁缺口。
- 已关闭 issue [#16979](https://github.com/NousResearch/hermes-agent/issues/16979) — QQ Bot 文件附件在下载失败时不再被静默丢弃。
- 已关闭功能/文档 issue [#94793](https://github.com/NousResearch/hermes-agent/issues/94793) 和 [#22699](https://github.com/NousResearch/hermes-agent/issues/22699) — 辅助审查 reasoning effort 和 `gpt-realtime-2` 文档。

进行中的开放 PR 还针对网关压缩、Windows 状态探测、工具参数修复、TUI 事件锁定、Mattermost 循环遏制、Discord 输入循环、UV 链接模式、仪表板 profile 查询参数、Kanban 依赖、Gemini TTS 以及网关守卫性能等方面的稳定性。

## 4. 社区热门话题

按评论数统计最活跃的 issue：

1. [#136567](https://github.com/NousResearch/hermes-agent/issues/136567) — **13 条评论** — Telegram DM 话题：新建话题的开启消息以无 thread 方式投递，导致 lobby 恢复将其错误路由到上一个话题。开放，P2。底层需求：正确的 Telegram forum/thread 会话映射和消息投递保证。
2. [#135522](https://github.com/NousResearch/hermes-agent/issues/135522) — **4 条评论** — 在 ACP 会话中，具备写入能力的 MCP 工具审批被静默拒绝。开放，P2。底层需求：审批提示必须在 Hermes 与 ACP 客户端之间正确桥接。
3. [#132814](https://github.com/NousResearch/hermes-agent/issues/132814) — **4 条评论** — Home Assistant 迁移将插件安装到从未使用过 HA 的配置文件中。已关闭；通过 [#137033](https://github.com/NousResearch/hermes-agent/pull/137033) 修复。
4. [#105239](https://github.com/NousResearch/hermes-agent/issues/105239) — **4 条评论** — 桌面端后端池让已打开聊天的后端保持常驻，每个进程占用约 120 MB。已关闭。
5. [#96027](https://github.com/NousResearch/hermes-agent/issues/96027) — **4 条评论** — 网关在 `state.db`/WAL 上泄漏 SQLite 文件描述符；可能耗尽 launchd 的 256-fd 限制。开放，P1。需求：长期运行网关的资源卫生。
6. [#126305](https://github.com/NousResearch/hermes-agent/issues/126305) — **4 条评论** — DeepSeek 压缩摘要总是被截断，因为提供方静默将 `max_tokens` 默认设为 8192，低于约 14K 的目标。开放，P2。
7. [#136738](https://github.com/NousResearch/hermes-agent/issues/136738) — **4 条评论** — `/memory approve all` 可以应用破坏性的整条条目覆盖，而不显示被覆盖的内容。开放，P1，needs-decision。
8. [#127450](https://github.com/NousResearch/hermes-agent/issues/127450) — **4 条评论** — `hermes update` 在拥有许多 checkpoint 项目的主机上卡顿。已关闭。

总体反应活动较低：[#126860](https://github.com/NousResearch/hermes-agent/issues/126860) 有 2 个 👍，而 [#22699](https://github.com/NousResearch/hermes-agent/issues/22699)、[#71575](https://github.com/NousResearch/hermes-agent/issues/71575) 和 [#94793](https://github.com/NousResearch/hermes-agent/issues/94793) 各有 1 个 👍。

## 5. 缺陷与稳定性

根据可用标签按严重程度排序。

### P1 — 最高影响
- [#96027](https://github.com/NousResearch/hermes-agent/issues/96027) — 网关在 `state.db` 上泄漏 SQLite 连接；22 天后有 48 个 db FD 和 46 个 WAL FD。开放。此快照中看不到直接修复 PR。
- [#136738](https://github.com/NousResearch/hermes-agent/issues/136738) — 记忆写入审批：`/memory approve all` 应用破坏性提案而不显示被覆盖内容。开放，needs-decision。
- [#126860](https://github.com/NousResearch/hermes-agent/issues/126860) — Windows：提升权限的安装/更新使 `%LOCALAPPDATA%\hermes\tools` 条目归管理员所有；之后非提升权限的 `hermes update` 因 WinError 5 失败。开放，重复，2 个 👍。

### P2 — 显著
- [#136567](https://github.com/NousResearch/hermes-agent/issues/136567) — Telegram DM 话题开启消息被无 thread 误路由。开放。
- [#135522](https://github.com/NousResearch/hermes-agent/issues/135522) — 在 ACP 会话中，具备写入能力的 MCP 工具审批被静默拒绝。开放。
- [#126305](https://github.com/NousResearch/hermes-agent/issues/126305) — DeepSeek 压缩因 `max_tokens` 默认值而总是截断。开放。
- [#131409](https://github.com/NousResearch/hermes-agent/issues/131409) — Docker/远程终端后端按进程运行 prompt-backend-probe，每轮聊天创建一个容器。开放。
- [#132558](https://github.com/NousResearch/hermes-agent/issues/132558) — `exclude-newer-package` 省略精确固定版本，导致在不含 PEP 691 upload-time 的索引上解析失败。开放。
- [#102425](https://github.com/NousResearch/hermes-agent/issues/102425) — 对于自定义提供方，`model_overrides` 永不匹配，因为 `custom:` 前缀未被剥离。开放。
- [#91568](https://github.com/NousResearch/hermes-agent/issues/91568) — Kanban Docker worker 工作区需要按任务限定并由主机支持。开放。
- [#135344](https://github.com/NousResearch/hermes-agent/issues/135344) — Kanban 调度器 `_pid_recycled` 的精确相等检查会误判存活的 macOS worker 并复制它们。开放。
- 此窗口已关闭：[#105239](https://github.com/NousResearch/hermes-agent/issues/105239) 桌面端后端池泄漏；[#127450](https://github.com/NousResearch/hermes-agent/issues/127450) 更新卡顿。

### P3 — 值得注意
- [#132259](https://github.com/NousResearch/hermes-agent/issues/132259) — 每次启动时约有 5 个随机平台插件因“dictionary changed size during iteration”而加载失败。开放。
- [#134537](https://github.com/NousResearch/hermes-agent/issues/134537) — 未经授权的 Home Assistant 升级会安装 root 所有的文件和 git 子模块，阻止未来更新。开放。
- [#104865](https://github.com/NousResearch/hermes-agent/issues/104865) — 并发 hook 回调被丢弃，而不是串行化。开放。
- [#105123](https://github.com/NousResearch/hermes-agent/issues/105123) — `disk-cleanup` hook 在包含 `\n` 转义的带引号终端参数上因 ENAMETOOLONG 崩溃。开放。
- [#137017](https://github.com/NousResearch/hermes-agent/issues/137017) — 记忆提供方 `holographic` 未安装；自动安装失败。开放，今日新增。
- [#137010](https://github.com/NousResearch/hermes-agent/issues/137010) — 多路复用网关会话重置插件检查仅在 root 作用域运行，但 `pre_gateway_dispatch` 是按消息 profile 的。开放，今日新增。
- [#133595](https://github.com/NousResearch/hermes-agent/issues/133595) — Z.AI `glm-5.3-flash` 以 HTTP 400 / 代码 1210 拒绝 `reasoning_effort=medium`。开放。
- 已关闭：[#25793](https://github.com/NousResearch/hermes-agent/issues/25793) 审批门禁混淆；[#16979](https://github.com/NousResearch/hermes-agent/issues/16979) QQ Bot 附件丢弃。

进行中的修复 PR 包括 [#137051](https://github.com/NousResearch/hermes-agent/pull/137051) 网关压缩重试，[#137050](https://github.com/NousResearch/hermes-agent/pull/137050) Windows 状态探测，[#137032](https://github.com/NousResearch/hermes-agent/pull/137032) 工具参数修复，[#137038](https://github.com/NousResearch/hermes-agent/pull/137038) TUI 事件锁定，[#137037](https://github.com/NousResearch/hermes-agent/pull/137037) Mattermost 机器人循环，[#137042](https://github.com/NousResearch/hermes-agent/pull/137042) Discord 输入循环，[#137040](https://github.com/NousResearch/hermes-agent/pull/137040) UV 链接模式，以及 [#137039](https://github.com/NousResearch/hermes-agent/pull/137039) Kanban 依赖编辑。

## 6. 功能请求与路线图信号

开放功能请求：
- [#71575](https://github.com/NousResearch/hermes-agent/issues/71575) — 在 OAuth 选择时呈现 Anthropic 消费者条款约束，并为 Claude Code 凭据回退提供退出选项。P3，`needs-decision`，1 个 👍。
- [#18081](https://github.com/NousResearch/hermes-agent/issues/18081) — 允许基于间隔的 cron 任务指定首次运行锚点，将频率与注册时间解耦。P3，长期存在。

面向功能的 PR：
- [#137049](https://github.com/NousResearch/hermes-agent/pull/137049) — 通过语义同义词、CJK 支持和紧凑 schema 增强工具搜索。
- [#119684](https://github.com/NousResearch/hermes-agent/pull/119684) — 为智能命令审批支持 OpenRouter Decisions 审批。
- [#137039](https://github.com/NousResearch/hermes-agent/pull/137039) — 桌面端 Kanban 依赖编辑。
- [#137043](https://github.com/NousResearch/hermes-agent/pull/137043) — 将插件目录中的 `agora` 重新固定到 v2.0.12。
- [#137044](https://github.com/NousResearch/hermes-agent/pull/137044) — 将 Vornik 配套目录更新到 0.19.0。

已关闭的功能/文档信号：
- [#94793](https://github.com/NousResearch/hermes-agent/issues/94793) — `auxiliary.review.reasoning_effort` 支持。
- [#22699](https://github.com/NousResearch/hermes-agent/issues/22699) — 将 `gpt-realtime-2` 记录为受支持的 OpenAI Realtime 模型。

预测：`v0.21.7` 之后的下一个次要版本可能会包含工具搜索/CJK 改进、OpenRouter Decisions 审批、插件目录刷新和 Kanban 依赖 UI。cron 首次运行锚定和 Anthropic OAuth 退出选项可能仍受维护者决策阻塞。

## 7. 用户反馈摘要

真实用户痛点集中在跨平台可靠性和插件生命周期：
- Windows 安装/更新所有权：[#126860](https://github.com/NousResearch/hermes-agent/issues/126860) 显示提升权限安装会破坏之后的非提升权限更新。
- 插件自动安装/迁移：[#132814](https://github.com/NousResearch/hermes-agent/issues/132814)、[#134537](https://github.com/NousResearch/hermes-agent/issues/134537) 和 [#137017](https://github.com/NousResearch/hermes-agent/issues/137017) 反映了对不需要的 Home Assistant 安装、root 所有文件以及自动安装失败的沮丧。
- 审批安全/透明度：[#135522](https://github.com/NousResearch/hermes-agent/issues/135522) 和 [#136738](https://github.com/NousResearch/hermes-agent/issues/136738) 表明用户需要可见、可信的审批流程。
- 平台/会话路由：[#136567](https://github.com/NousResearch/hermes-agent/issues/136567) 凸显 Telegram 线程边界情况。
- 提供商怪癖：[#126305](https://github.com/NousResearch/hermes-agent/issues/126305) 和 [#133595](https://github.com/NousResearch/hermes-agent/issues/133595) 表明提供商特定默认值仍会导致会话失败。
- 资源/稳定性：[#96027](https://github.com/NousResearch/hermes-agent/issues/96027) 和已关闭的 [#105239](https://github.com/NousResearch/hermes-agent/issues/105239) 反映长期运行的网关/桌面端资源问题。

反馈中活跃的使用场景包括 Telegram 私信/话题、带有 `trust: untrusted` 的 MCP 工具、ACP 会话、DeepSeek 辅助压缩、Docker 远程终端、Kanban worker、Home Assistant、记忆提供方、cron 自动化以及 Windows/macOS 桌面端安装。满意度体现在多个重要 issue 的快速关闭，但不满仍集中在安装所有权、插件迁移行为、审批清晰度和平台特定故障上。也存在多语言参与，例如 [#137017](https://github.com/NousResearch/hermes-agent/issues/137017) 中的中文报告。

## 8. 积压事项观察

需要维护者关注的重要开放事项：

- [#96027](https://github.com/NousResearch/hermes-agent/issues/96027) — P1 网关 SQLite FD 泄漏。自 2026-08-27 开放，4 条评论。对长期运行网关高风险。
- [#126860](https://github.com/NousResearch/hermes-agent/issues/126860) — P1 Windows 提升权限安装/更新权限问题。自 2026-09-28 开放，3 条评论，2 个 👍。重复但影响用户。
- [#126305](https://github.com/NousResearch/hermes-agent/issues/126305) — P2 DeepSeek 压缩截断。自 2026-09-28 开放，4 条评论。阻碍可靠的长会话。
- [#91568](https://github.com/NousResearch/hermes-agent/issues/91568) — P2 Kanban Docker worker 工作区。自 2026-08-21 开放，3 条评论。影响 Docker/Kanban 工作流。
- [#102425](https://github.com/NousResearch/hermes-agent/issues/102425) — P2 `model_overrides` 自定义提供方前缀不匹配。自 2026-09-03 开放，2 条评论。
- [#132259](https://github.com/NousResearch/hermes-agent/issues/132259) — P3 启动时随机插件加载失败。自 2026-10-03 开放，3 条评论。可能广泛影响插件可靠性。
- [#18081](https://github.com/NousResearch/hermes-agent/issues/18081) — P3 cron 首次运行锚点。自 2026-04-30 开放，1 条评论。长期存在的自动化请求。
- [#71575](https://github.com/NousResearch/hermes-agent/issues/71575) — P3 Anthropic OAuth 条款/退出选项。自 2026-07-25 开放，1 条评论，`needs-decision`。

可能需要审查的开放 PR：
- [#118150](https://github.com/NousResearch/hermes-agent/pull/118150) — 主动刷新插件 OAuth token。自 2026-09-21 开放。
- [#119684](https://github.com/NousResearch/hermes-agent/pull/119684) — OpenRouter Decisions 审批。自 2026-09-23 开放。
- [#116830](https://github.com/NousResearch/hermes-agent/pull/116830) — Windows surrogate stdin 测试。自 2026-09-20 开放。

总体而言，Hermes Agent 维护活跃，并正在关闭有意义的 issue，但 P1 网关/记忆/Windows 集群以及若干长期存在的 P2 平台 bug 是主要需要关注的项目健康风险。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw (nearai/ironclaw) — 项目摘要
**周期：** 2026-10-11 → 2026-10-12 | **生成于：** 2026-10-12

---

## 1. 今日概览

IronClaw 记录了一个纯维护日：**0 个 issue 更新，0 个新 release，7 个 PR 被触及**，且每个 PR 均由 `dependabot[bot]` 创建。在 24 小时窗口内未观察到人工编写的代码、文档或讨论活动。三个依赖 PR 已关闭/合并（#8114、#8103、#8123），四个仍处于打开状态（#8134、#8132、#8133、#7834），延续了项目稳定的依赖卫生节奏。活动评估：**中低量级，工程风险低，无面向用户的推进速度**——该仓库似乎处于安静的维护阶段，而非活跃的功能开发。

---

## 2. 发布

**本周期无新发布。** 无 changelog、breaking-change 或迁移数据可用。

---

## 3. 项目进展

今日未合并任何功能或修复工作。所有三个已关闭 PR 均为自动化依赖升级：

| PR | 范围 | 内容 | 标签 | 链接 |
|---|---|---|---|---|
| #8114 | Rust 依赖（“everything-else”） | 31 项更新；thiserror `2.0.20→2.0.21`，uuid `1.24.0→1.26.1`，base64 等 | size: XL, risk: low, contributor: experienced | [PR #8114](https://github.com/nearai/ironclaw/pull/8114) |
| #8103 | GitHub Actions | 8 项更新；claude-code-action `1.0.183→1.0.228`，setup-node `4.0.2→7.0.0` | dependencies, github_actions | [PR #8103](https://github.com/nearai/ironclaw/pull/8103) |
| #8123 | tokio-ecosystem | 3 项更新；tokio-test `0.4.5→0.4.6`，tower-http，tokio-tungstenite | dependencies, rust | [PR #8123](https://github.com/nearai/ironclaw/pull/8123) |

**观察：** #8114/#8103/#8123 的关闭与更新、更宽范围的升级（#8134、#8133、#8132）的打开同时发生，这些升级以更高的目标版本覆盖相同的依赖组。这是典型的 dependabot “取代并重新提出”周期，而非独立进展。

**等待合并/CI 的打开 PR：**
- [#8134](https://github.com/nearai/ironclaw/pull/8134) — everything-else 组，**34 项更新**（uuid → `1.27.0`，thiserror → `2.0.21`）——#8114 的下一轮迭代
- [#8132](https://github.com/nearai/ironclaw/pull/8132) — tokio-ecosystem 组，4 项更新——#8123 的下一轮迭代
- [#8133](https://github.com/nearai/ironclaw/pull/8133) — actions 组，10 项更新（claude-code-action → `1.0.247`，setup-node → `7.1.0`）——#8103 的下一轮迭代
- [#7834](https://github.com/nearai/ironclaw/pull/7834) — wasm 组（wasmtime、wasmtime-wasi、wit-component、wit-parser）

---

## 4. 社区热点

**今日无社区讨论信号可用。** 所有 7 个 PR 均报告 `Comments: undefined`（无评论数据），且每个 PR 均为 **0 👍 反应**。过去 24 小时内**没有 issue 更新**，因此没有评论或反应量有意义、可供排名的话题。

**底层解读：** 在零 issue 流量和 dependabot PR 零反应的情况下，此窗口中没有证据表明存在社区参与压力。项目的外部对话输入实际上处于沉寂状态——值得关注，因为一个开源 agent 项目持续沉默，可能表明代码库稳定/成熟，也可能表明社区参与进入低谷。

---

## 5. Bugs & Stability

**在 2026-10-11/12 未报告或更新任何 bug、崩溃或回归。** 零 issue 被触及，且此集合中无 PR 引用 bug 修复。

- 按严重程度排序的缺陷列表：**空**。
- 进行中的修复 PR：**不适用**。

稳定性说明（非缺陷）：PR [#7834](https://github.com/nearai/ironclaw/pull/7834) 被标记为 `risk: medium`，针对 **wasm/wasmtime** 运行时组——这是打开 PR 中唯一一个非平凡风险标签，也是最值得监控的稳定性相关项，因为它触及沙箱/执行层。

---

## 6. 功能请求与路线图信号

**今日未提交或更新任何功能请求**（0 个 issue）。本周期没有来自用户输入的直接路线图信号。

**从依赖方向推断的信号**（弱，仅工程侧）：
- 反复出现的 tokio / tower-http / tokio-tungstenite 升级表明对 **异步运行时和 HTTP/WebSocket 传输**路径有持续投入 → 很可能是服务器/agent 通信面。
- 持续存在的 wasm 组升级（[#7834](https://github.com/nearai/ironclaw/pull/7834)）表明 **基于 WASM 的插件/工具沙箱**仍是核心架构押注。
- CI 中 `anthropics/claude-code-action` 的大幅跃升意味着项目自身工作流**重度依赖基于 Claude 的自动化**。

**下一版本预测：** 无法从该数据集可靠预测任何面向用户的功能；如果有下一发布窗口，最可能是由 #8134/#8132/#8133 驱动的依赖刷新发布。

---

## 7. 用户反馈摘要

**此窗口未捕获任何用户反馈。** 没有 issue、没有评论，也没有反应可供分析，因此无法在不做推测的情况下报告任何痛点、用例或满意/不满意信号。

数据注意事项：所有 PR 的 `Comments` 字段均报告为 `undefined`，这意味着评论线程指标不可用，而非讨论深度确认为零。

---

## 8. 待办事项关注

以下打开 PR 是最值得维护者关注的条目：

| PR | 时长 | 为何需要关注 | 链接 |
|---|---|---|---|
| **#7834** — wasm 组（4 项更新） | 自 **2026-08-23** 起打开（约 50 天） | 停滞最久的打开条目；`size: L`、`risk: medium`；触及 wasmtime/wit 工具链——若长期不合并并随后变基，影响范围大 | [PR #7834](https://github.com/nearai/ironclaw/pull/7834) |
| **#8134** — everything-else（34 项更新） | 于 2026-10-11 打开 | 横跨仓库内 34 个 package；合并前有大量 CI/回归验证面 | [PR #8134](https://github.com/nearai/ironclaw/pull/8134) |
| **#8132** — tokio-ecosystem（4 项更新） | 于 2026-10-11 打开 | 核心异步运行时；及时合并以避免与 #7834 产生变基摩擦 | [PR #8132](https://github.com/nearai/ironclaw/pull/8132) |
| **#8133** — actions 组（10 项更新） | 于 2026-10-11 打开 | Actions 重大版本跃升（setup-node `4→7`）可能影响 CI 流水线行为 | [PR #8133](https://github.com/nearai/ironclaw/pull/8133) |

**待办风险评估：** 没有任何 issue backlog 本身值得注意——要么该项目的 issue tracker 不是主要接收渠道，要么 Issue 活动正在带外处理。由于没有比 #7834 更旧的 PR 仍处于打开状态，维护者的 PR 延迟在其他方面是健康的。

---

### 健康摘要

| 维度 | 信号 |
|---|---|
| 发布节奏 | ⚪ 本周期无 |
| 代码贡献（人工） | 🔴 未观察到 |
| 依赖卫生 | 🟢 活跃——3 个已合并，4 个打开 |
| Issue 分诊 | ⚪ 无活动（0 个 issue） |
| 社区参与 | 🔴 未记录评论/反应 |
| 稳定性 | 🟢 未报告 bug 或回归 |
| 待办风险 | 🟡 #7834 停滞约 50 天（wasm，中风险） |

*所有发现均严格源自所提供的 2026-10-11/12 GitHub 活动快照；数据缺失均报告为“未观察到”，而非推断该状况本身不存在。*

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw 项目摘要 — 2026-10-12

*来源：github.com/agentscope-ai/QwenPaw*

---

## 1. 今日概览

QwenPaw 在 2026-10-12 展现出**高维护吞吐量**：过去 24 小时内有 13 个 issue 和 23 个 PR 被处理，其中 13 个 PR 已合并/关闭，10 个仍开放。值得注意的是，今日关闭的 PR 中有很大一部分是**2026 年 3 月至 7 月长期停滞的 PR**（MCP 加固、上下文溢出恢复、插件市场版本路由、内存后端），这表明项目正在进行积极的积压清理，而非纯粹的新工作。Issue 流量仍以 **v2.2.x Desktop 和 Codex 后端路径的稳定性回归**为主，此外还有越来越多 Console/UX 打磨请求。没有发布新版本，因此 v2.2.1/2.2.2-beta 用户只能运行来自未发布分支的修复。整体健康度在**贡献流上良好，在发布节奏上紧张**——开放的、未修复的 v2.2.x bug 报告积累速度快于版本发布可消化的速度。

---

## 2. 发布

**无。** 过去 24 小时内没有新版本。可观测到的用户报告版本为 v2.2.1（稳定版）、v2.2.2-beta.4 和 v2.1.1b2（较旧的 Desktop 构建）。鉴于已排队合并的修复数量（见 §3），没有发布新版本这一点值得注意。

---

## 3. 项目进展 — 已合并/关闭的 PR

13 个 PR 已合并或关闭。主题清晰：

**插件生态正确性**
- [#5661](https://github.com/agentscope-ai/QwenPaw/pull/5661) — 按 QwenPaw 版本兼容性（`qwenpaw_version` / `min_version` / `max_version`）过滤插件市场和 CDN 目录。
- [#5612](https://github.com/agentscope-ai/QwenPaw/pull/5612) — 插件市场下载路由按主版本隔离（v1.x 与 v2.x 不兼容修复）。

**运行时韧性**
- [#2783](https://github.com/agentscope-ai/QwenPaw/pull/2783) — 通过一次性内存压缩 + 重试自动恢复上下文窗口溢出。
- [#2520](https://github.com/agentscope-ai/QwenPaw/pull/2520) — 预检活动模型可用性；以明确错误拒绝激活。
- [#1977](https://github.com/agentscope-ai/QwenPaw/pull/1977) — 加固 MCP 拆卸、cron/channel 异常处理。
- [#2052](https://github.com/agentscope-ai/QwenPaw/pull/2052) — 将 MCP 连接错误降级为工具级失败，而不是聊天可见错误。

**MCP + 工具链**
- [#1978](https://github.com/agentscope-ai/QwenPaw/pull/1978) — MCP 导入验证/预览 + 运行时状态诊断。
- [#4707](https://github.com/agentscope-ai/QwenPaw/pull/4707) — 健壮的 `ToolResponse` 文本块处理（属性访问与 dict 访问崩溃）。

**可配置性**
- [#5732](https://github.com/agentscope-ai/QwenPaw/pull/5732) — 新增 `none` 内存后端，可通过 Console 完全禁用内存系统。

**桌面端 UX**
- [#5569](https://github.com/agentscope-ai/QwenPaw/pull/5569) — 两阶段启动画面加载，消除 5–30 秒 Windows 启动白屏。

另外三个 PR 在提供的数据集中未提供详细信息即关闭。

**评估：** 这些关闭主要是*基础性加固*，而非头条功能。其中若干直接针对用户今天仍在报告的痛点（上下文溢出、MCP 噪声、插件不兼容），这表明一旦发布版本，这些修复可能解决开放 issue。

---

## 4. 社区热门话题

| 排名 | 条目 | 评论数 | 信号 |
|---|---|---|---|
| 1 | [#7311](https://github.com/agentscope-ai/QwenPaw/issues/7311) — v2.1.1b2 缺少 `_qwenpaw_remote_backend`，所有工具失效 | 5 | **今日关闭。** 参与度最高条目；Windows Desktop 上全部工具失败。 |
| 2 | [#7016](https://github.com/agentscope-ai/QwenPaw/issues/7016) — 流式工具调用 404 / `Tool call not found` | 4 | 自 2026-08-14 起仍开放——约 2 个月未解决。 |
| 3 | [#8174](https://github.com/agentscope-ai/QwenPaw/issues/8174) — Tavily provider 没有 API-key 配置入口 | 3 | 付费配额用户受阻；讨论活跃。 |

**底层需求分析：**
- **#7311 / #7016** 都指向**脆弱的远程后端和工具调用生命周期管道**。#7016 中的 404 循环（`offload` 端点在流式传输期间反复 404）表明流式传输下会话/工具调用注册表失步——这类故障会产生“所有工具失效”的症状。这是当前项目中最具影响力的可靠性领域。
- **#8174** 反映出**变现/自托管的期望错配**：QwenPaw 内置了一个 provider，静默使用无密钥匿名模式，且没有 UI 可注入用户密钥。拥有付费 API 访问权限的用户实际上被阻塞——这是内置集成硬编码凭证路径时的常见模式。

---

## 5. Bug 与稳定性

按严重程度排序：

**严重**
- [#7311](https://github.com/agentscope-ai/QwenPaw/issues/7311) — *[已关闭]* 在 v2.1.1b2 Windows Desktop 上所有 agent 工具失败（`ModuleNotFoundError: _qwenpaw_remote_backend`）；模块在 `_internal` 中缺失。**今日关闭**——数据集中没有明确关联的修复 PR，因此在用户假定修复已发布前应验证解决情况。

**高**
- [#8173](https://github.com/agentscope-ai/QwenPaw/issues/8173) — Codex 后端在重启后静默丢失 harness↔codex 连接；留下**永久僵尸 `running` 条目，阻塞所有恢复路径**（仅 chat/stop 有效）。影响 v2.2.1。未确定修复 PR。
- [#7016](https://github.com/agentscope-ai/QwenPaw/issues/7016) — 流式工具调用 offload 循环返回 404；工具调用未找到。开放约 59 天。未确定修复 PR。
- [#8179](https://github.com/agentscope-ai/QwenPaw/issues/8179) — `qwenpaw skills install <github-url>` **静默损坏所有二进制文件**（非 UTF-8 → U+FFFD），破坏已安装技能中的图像/字体。数据完整性类 bug，v2.2.1 Docker。
- [#8175](https://github.com/agentscope-ai/QwenPaw/issues/8175) — `ProviderManager.active_model` 在启动时未加载，加上 `_load_agent_model_settings` 中静默的 `except Exception: pass`；导致多 agent 部署中**夜间 cron (dream) 失败**。

**中**
- [#8184](https://github.com/agentscope-ai/QwenPaw/issues/8184) — Doom-loop 门控杀死**合法的任务授权轮询**；cron 随后在终止后记录虚假成功。误报防护逻辑。
- [#8185](https://github.com/agentscope-ai/QwenPaw/issues/8185) — `desktop.log` 在 Windows 上从不轮转；sidecar tee 句柄使 `_SafeRotatingFileHandler` 失效。磁盘增长风险。
- [#8186](https://github.com/agentscope-ai/QwenPaw/issues/8186) — Console 消息导航跳转覆盖手动滚轮滚动，在约 1.6 秒内弹回锚点。
- [#8176](https://github.com/agentscope-ai/QwenPaw/issues/8176) — Plugin Store“全部更新”跳过第三方插件（v2.2.1）。

**低 / 配置**
- [#8177](https://github.com/agentscope-ai/QwenPaw/issues/8177) — 硬编码的 `/console` 基路径阻塞反向代理子路径部署。

**进行中的修复 PR：**
- [#8188](https://github.com/agentscope-ai/QwenPaw/pull/8188) — 修复审批决策竞态（引用 #8105）：决策丢失 → 尽管先前已批准，工具仍被拒绝。
- [#8183](https://github.com/agentscope-ai/QwenPaw/pull/8183) — 保留取消原因；对可恢复超时返回 `ERROR`，以便父 agent 循环继续。
- [#8182](https://github.com/agentscope-ai/QwenPaw/pull/8182) — 服务端持久化按聊天设置的审批级别（与 #8180 直接相关）。
- [#7973](https://github.com/agentscope-ai/QwenPaw/pull/7973) — 从被拒绝的媒体 URL 恢复（媒体 HTTP 400 → 永久对话失败）。

---

## 6. 功能请求与路线图信号

| Issue / PR | 请求 | 下个版本可能性 |
|---|---|---|
| [#8174](https://github.com/agentscope-ai/QwenPaw/issues/8174) | Tavily API-key 配置入口 | **中高** — UI/配置改动小，用户影响大 |
| [#8181](https://github.com/agentscope-ai/QwenPaw/issues/8181) | 会话级变更通知（长生命周期事件通道）以避免硬刷新 | **中** — 架构性，涉及 SSE/流式层 |
| [#8180](https://github.com/agentscope-ai/QwenPaw/issues/8180) + [#8182](https://github.com/agentscope-ai/QwenPaw/pull/8182) | 会话范围审批级别服务端持久化 | **高** — PR 已开放，关闭一个有记录的 UX/语义缺口 |
| [#8177](https://github.com/agentscope-ai/QwenPaw/issues/8177) | 可配置的反向代理 console 基路径 | **中** — 常见自托管诉求，复杂度低 |
| [#8176](https://github.com/agentscope-ai/QwenPaw/issues/8176) | “全部更新”应包含第三方插件 | **中高** — 与今日插件市场 PR 关闭相呼应 |
| [#8178](https://github.com/agentscope-ai/QwenPaw/pull/8178) | 嵌套/分类技能目录 | **中** — PR 开放，向后兼容 |
| [#8187](https://github.com/agentscope-ai/QwenPaw/pull/8187) | 将 channel 抽取为带子进程隔离的插件 | **低（下一个小版本）**（XXXL，架构性） |
| [#8164](https://github.com/agentscope-ai/QwenPaw/pull/8164) | 原生 HarmonyOS 客户端（ArkUI/ArkTS） | **低（下一个小版本）**（XXXL，新平台） |
| [#8137](https://github.com/agentscope-ai/QwenPaw/pull/8137) | 官方“减少效果”无障碍层级 | **中高** — 小而独立 |
| [#8156](https://github.com/agentscope-ai/QwenPaw/pull/8156) | 面向 worker 容器的 Coding-CLI 管理端点 | **中** — 多 agent 部署需求 |

**预测：** 下一个版本很可能以 **Console/服务端状态一致性**（#8182、#8137、#8177）和**插件市场正确性**（#8176，基于今日 #5661/#5612 关闭）为主，并纳入来自 #8188/#8183/#7973 的稳定性修复。

---

## 7. 用户反馈摘要

**痛点（不满意）：**
- **工具可靠性是头号抱怨** — #7311（所有工具失效）、#7016（404 循环）、#8184（doom-loop 误报）、#8188 相关审批竞态。用户在*工具执行层*遇到失败，而这是 agent 产品的核心。
- **静默失败是反复出现的主题**：#8175（静默 `except Exception: pass`）、#8179（静默二进制损坏）、#8173（静默连接中断留下僵尸状态）。用户明确提到“无日志、无错误”行为。
- **自托管摩擦**：#8177（反向代理子路径）和 #8179（Docker 安装）表明自托管群体真实且活跃，其部署需求未得到满足。
- **凭证/配置不透明**：#8174 — 用户无法知道内置 Tavily 使用无密钥模式，也无法提供自己的密钥。

**满意信号：**
- 高质量、结构良好的 bug 报告（结构化模板、版本锁定、环境细节）表明社区参与度高、技术能力强，愿意为项目投入精力。
- 多位首次及重复贡献者活跃（#8178 首次、#6499 首次），这是积极健康信号。

**揭示的使用场景：** 带按工作区配置的多 agent Docker 部署（#8175）、cron/dream 夜间任务、需要跨设备一致性的多 channel 会话（#8181），以及运行第三方 coding CLI 的 worker 容器（#8156）。

---

## 8. 积压观察 — 需要维护者关注

| 条目 | 年龄 | 为什么重要 |
|---|---|---|
| [#7016](https://github.com/agentscope-ai/QwenPaw/issues/7016) — 流式工具调用 404 | **开放 59 天** | 高严重性，反复复现，无关联修复。持续时间最长的未解决核心 bug。 |
| [#7973](https://github.com/agentscope-ai/QwenPaw/pull/7973) — 从被拒绝的媒体 URL 恢复 | **开放 18 天** | 修复已就绪但未合并；期间破坏对话的 bug 持续存在。 |
| [#6499](https://github.com/agentscope-ai/QwenPaw/pull/6499) — 添加 Atlas Cloud provider | **开放约 77 天** | 首次贡献者 PR，自包含，若闲置会阻碍贡献者势头。 |
| [#8183](https://github.com/agentscope-ai/QwenPaw/pull/8183) — 保留取消原因 | 0 天（新） | 与 #8184 类误报终止直接相关；值得一起快速推进。 |
| [#8156](https://github.com/agentscope-ai/QwenPaw/pull/8156) — Coding-CLI 管理端点 | 3 天 | 多 agent 运维缺口；未见维护者响应。 |
| [#1977](https://github.com/agentscope-ai/QwenPaw/pull/1977), [#2783](https://github.com/agentscope-ai/QwenPaw/pull/2783), [#2520](https://github.com/agentscope-ai/QwenPaw/pull/2520) | **各约 200 天（现已关闭）** | 解决值得欢迎，但有意义的健壮性 PR 约 6 个月延迟是**审查吞吐量红旗**，值得从结构上解决。 |

**积压健康度结论：** 项目今天显然正在处理积压，但 #1977/#2783/#2520/#4707 的多月延迟表明瓶颈是**维护者审查能力**，而非贡献者供给。结合 24 小时内无发布以及一批开放的 v2.2.x 回归，最高杠杆的行动是切出一个版本，发布刚合并的修复。

---

*摘要是根据截至 2026-10-12 的 24 小时窗口内的 GitHub 活动数据生成的。源数据中 PR 的评论数不可用（`undefined`）；因此 PR 按新近度和范围标签排序。*

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目简报 — 2026-10-12

## 1. 今日概览

ZeroClaw 在 2026-10-12 展现出**高速度、低吞吐的工程活动**：过去 24 小时内有 45 个 PR 和 16 个 issue 被触及，但仅关闭了 2 个 PR、0 个 issue，且没有新版本发布。项目正处于**设计密集型阶段**，同时有六个高风险 RFC 处于开启状态，涵盖 A2A 协议 crate、临时智能体集群、RAG/知识语料库、提供商路由以及运行时/网关分离——其中大多数在等待维护者审查或作者处理。来自少量贡献者（`IftekharUddin`、`JordanTheJet`、`NiuBlibing`）的大量堆叠 PR 工作，正在推动插件/WASM 托管、能力边界重构与网关加固，但几乎全部仍处于开启且未合并状态。与此同时，面向用户的缺陷报告持续出现（配对码、Web 聊天焦点、记忆整合），表明真实世界的上手摩擦正在以快于解决的速度被提交。整体健康度：**贡献者参与度高、路线图方向清晰，但合并积压不断扩大，已接受的追踪项/RFC 与已交付代码之间存在显著差距。**

## 2. 版本发布

**无。** 过去 24 小时没有新版本发布。若干开启中的 PR 带有 `release:v0.8.6` 和 `release:v0.9.0` 标签，表明两条版本线仍在推进：
- [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) — Phase 2 运行时工作（v0.8.6）与 Phase 3 网关分离（v0.9.0）的追踪项
- [#11187](https://github.com/zeroclaw-labs/zeroclaw/pull/11187)、[#11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261)、[#11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262)、[#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347) — 均标记为 v0.8.6

今日无需报告破坏性变更或迁移说明。

## 3. 项目进展

24 小时窗口内仅**合并/关闭了 2 个 PR**，而仍有 43 个开启——比例明显失衡。

**今日关闭：**
- [#11531](https://github.com/zeroclaw-labs/zeroclaw/pull/11531) `[CLOSED]` — `fix(tunnel): report the URL tailscale actually serves the gateway on`。修复了一个真实的误报缺陷：网关对外声明 `https://<host>:<local_port>`，而 Tailscale 实际上在 443 端口提供 HTTPS。改动很小（`size:XS`），来自一位杰出贡献者。

**推进中但尚未合并（高信号集群）：**
- **插件/WASM 基础设施栈** — [#11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261)、[#11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262)、[#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347)、[#11309](https://github.com/zeroclaw-labs/zeroclaw/pull/11309)、[#11311](https://github.com/zeroclaw-labs/zeroclaw/pull/11311)、[#11318](https://github.com/zeroclaw-labs/zeroclaw/pull/11318)、[#11319](https://github.com/zeroclaw-labs/zeroclaw/pull/11319)、[#11320](https://github.com/zeroclaw-labs/zeroclaw/pull/11320) — 一条高度堆叠的链条（发布产物、快速开始安装、webhook 入口、RPC 分发）。每个 PR 都依赖更早的 PR，因此进展实际上被串行化。
- **运行时能力边界** — [#11174](https://github.com/zeroclaw-labs/zeroclaw/pull/11174)、[#11187](https://github.com/zeroclaw-labs/zeroclaw/pull/11187)、[#11526](https://github.com/zeroclaw-labs/zeroclaw/pull/11526) — 接收能力参数的轮次构造器，并遵循传入的能力边界。
- **提供商灵活性** — [#9809](https://github.com/zeroclaw-labs/zeroclaw/pull/9809)（每个提供商配置可包含多个模型）与 [#11659](https://github.com/zeroclaw-labs/zeroclaw/pull/11659)（snake_case 上下文溢出码）。

## 4. 社区热门话题

按更新项中的评论数排序：

| 条目 | 评论数 | 主题 |
|---|---|---|
| [#11254](https://github.com/zeroclaw-labs/zeroclaw/issues/11254) RFC：A2A 协议 crate | 7 | 横切架构 / 线路模型 |
| [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) 运行时与网关交付追踪 | 6 | 版本规划 |
| [#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025) RFC：带 TUI 的临时智能体集群 | 4 | 多智能体编排 |
| [#11074](https://github.com/zeroclaw-labs/zeroclaw/issues/11074) RFC：`search_routes` 提供商路由 | 4 | 工具/搜索路由 |
| [#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235) RFC：知识语料库（RAG） | 4 | 记忆/检索 |

**底层需求分析：**
- **互操作性**占主导。[#11254](https://github.com/zeroclaw-labs/zeroclaw/issues/11254) 构建在已确立的 A2A 工作（#9106、#7763、#8274）之上，表明用户希望 ZeroClaw 使用标准 agent-to-agent 协议，而不是定制协议。
- **多智能体编排**是第二个反复出现的主题——[#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025)（集群）、[#11664](https://github.com/zeroclaw-labs/zeroclaw/issues/11664)（Dark Factory 工作流）和 [#11667](https://github.com/zeroclaw-labs/zeroclaw/issues/11667)（命令策略继承）都指向用户已超出单智能体模型。
- **检索增强式依据**（[#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235)）反映出用户要求智能体能够基于运营方自有文档作答，而不只是依赖其参数化知识。

## 5. 缺陷与稳定性

按严重程度排序。较新的报告似乎尚未关联修复 PR。

**S1 — 工作流受阻：**
- [#11648](https://github.com/zeroclaw-labs/zeroclaw/issues/11648) `[Bug]` — Web 仪表盘配对输入被限制为 **6 位数字**，但后端默认 `PairingCodePolicy` 现在签发 **32 位字母数字字符**。CLI 会打印完整配对码，但 Web 对话框会拒绝它，实际上阻止了 Web 端上手。用户影响高；很可能是一个快速前端修复。

**S2 — 行为降级：**
- [#11655](https://github.com/zeroclaw-labs/zeroclaw/issues/11655) `[Bug]` — 在**默认** `embedding_provider = "none"` 下，当不存在向量分数时，`consolidate_turn` / `conflict::check_and_resolve_conflicts` 会**软隐藏无关的 Core 记忆**。这是一个接近数据丢失的回归，影响默认配置，使其成为今日最令人担忧的稳定性问题。

**S3 — 轻微：**
- [#11658](https://github.com/zeroclaw-labs/zeroclaw/issues/11658) `[Bug]` — Web 聊天输入框在回复后不会重新获得焦点，需要额外点击一次。严重性低，但在主界面中造成恼人的 UX 摩擦。

**相关未决的正确性/架构问题：**
- [#10290](https://github.com/zeroclaw-labs/zeroclaw/issues/10290) — 将实时配置句柄贯穿 `agent::run`，用于分离的对等/子智能体轮次（SSRF 策略传播缺口）。
- [#7692](https://github.com/zeroclaw-labs/zeroclaw/issues/7692)（已接受）— 网关路由器请求体限制与 cron 运行超时接线测试仍然缺失；**PR [#10310](https://github.com/zeroclaw-labs/zeroclaw/pull/10310) 实现了这些测试**，并且处于开启状态，等待作者处理。

## 6. 功能请求与路线图信号

**今日新增/更新的请求：**
- [#11664](https://github.com/zeroclaw-labs/zeroclaw/issues/11664) — **Level 5 Dark Factory 工作流**：具备生命周期管理的完全编排式智能体团队。
- [#11662](https://github.com/zeroclaw-labs/zeroclaw/issues/11662) — **将 Channels 从 Core 中分离**：模块化通道接口（IPC、MQTT 等），以便丢弃未使用的通道，因其是“累赘和安全风险”。
- [#11668](https://github.com/zeroclaw-labs/zeroclaw/issues/11668) — 让网关 `MAX_BODY_SIZE` 可配置，并返回恰当的错误码 + 大小（由多智能体 A2A 中继长上下文问题驱动）。
- [#11657](https://github.com/zeroclaw-labs/zeroclaw/issues/11657) — 轮次整合的可配置目标类别（`[memory] consolidation_category`）。
- [#11667](https://github.com/zeroclaw-labs/zeroclaw/issues/11667) — 决定委派轮次是否继承调用方的命令策略。

**下一版本预测（v0.8.6 / v0.9.0）：**
- **可配置网关请求体限制**（[#11668](https://github.com/zeroclaw-labs/zeroclaw/issues/11668)）是一个范围明确的小需求，且有具体用例（A2A 长上下文中继），是近期补丁的有力候选。
- **通道模块化**（[#11662](https://github.com/zeroclaw-labs/zeroclaw/issues/11662)）直接契合 [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) 中 v0.9.0 的“网关分离”主题——有可能进入 v0.9.0。
- **集群/Dark Factory 编排**（[#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025)、[#11664](https://github.com/zeroclaw-labs/zeroclaw/issues/11664)）很可能在 v0.9.0 之后，因为它依赖仍未合并的 A2A 与能力边界工作。
- **多模型提供商配置**（[#9809](https://github.com/zeroclaw-labs/zeroclaw/pull/9809)，自 8 月 7 日起开启）与 **RAG/知识语料库**（[#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235)）是更大的押注，很可能面向 v1.0 跟踪。

## 7. 用户反馈摘要

**痛点：**
- **Web UI 中的上手流程已损坏。** 配对码不匹配（[#11648](https://github.com/zeroclaw-labs/zeroclaw/issues/11648)，S1）意味着用户按照文档流程操作时，会在浏览器中撞上硬墙。再加上焦点缺陷（[#11658](https://github.com/zeroclaw-labs/zeroclaw/issues/11658)），Web 仪表盘正在产生最直接的负面反馈。
- **默认设置导致静默数据丢失。** [#11655](https://github.com/zeroclaw-labs/zeroclaw/issues/11655) 影响*开箱即用*配置（`embedding_provider = "none"`），因此从未自定义设置的用户会暴露在风险中。这削弱了对记忆子系统的信任。
- **多智能体用户感到摩擦。** [#11668](https://github.com/zeroclaw-labs/zeroclaw/issues/11668) 报告网关请求体限制会在“不到 1 秒内，仅以裸字符串 `length limit exceeded`”拒绝长上下文 A2A 消息——不透明的错误阻塞了合法工作流。
- **臃肿与安全焦虑。** [#11662](https://github.com/zeroclaw-labs/zeroclaw/issues/11662) 表达了用户明确希望摆脱未使用通道的愿望，这些通道被描述为“累赘”和“安全风险”，表明用户想要精简、可审计的部署。

**满意度信号：** 用户参与度很高——提交详细 RFC、引用具体 commit SHA 和 PR 编号，并构建多智能体拓扑。这种成熟度表明存在满意的重度用户群，但未解决 RFC 的堆积和缓慢的合并速度有让他们受挫的风险。

## 8. 积压观察

因存续时间、规模或停滞状态而需要维护者关注的条目：

- [#9809](https://github.com/zeroclaw-labs/zeroclaw/pull/9809) — **自 2026-08-07 开启（66+ 天）**，`size:XL`，`risk:high`，标记为 `needs-author-action`。每个提供商支持多模型是一项频繁被请求的能力，却已停滞超过两个月。最高优先级积压项。
- [#7692](https://github.com/zeroclaw-labs/zeroclaw/issues/7692) — **自 2026-06-15 开启（约 4 个月）**，`status:accepted`，`risk:high`。网关测试覆盖缺口；其实施 PR [#10310](https://github.com/zeroclaw-labs/zeroclaw/pull/10310) 也处于开启状态，并被标记为 `needs-author-action`。
- [#9597](https://github.com/zeroclaw-labs/zeroclaw/issues/9597) — **自 2026-07-31 开启**，`status:in-progress`，`follow-up`，`risk:high`。持久/可归因的对等智能体轮次；是集群工作的基础，但已停滞。
- [#10290](https://github.com/zeroclaw-labs/zeroclaw/issues/10290) — **自 2026-08-24 开启**，`follow-up`，`risk:high`，仅 1 条评论。分离轮次的实时配置传播（SSRF 策略正确性）；与安全相关但讨论不足。
- [#11289](https://github.com/zeroclaw-labs/zeroclaw/pull/11289) — 被标记为 **`do-not-merge`**，尽管是一个实质性的安全 PR（稳定的 RPC 拒绝原因标识符）；可能因维护者决策而受阻。
- **堆叠插件 PR 链**（[#11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261) → [#11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262) → [#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347) 等）——8+ 个相互依赖的 PR 全部开启。这里任何单个合并决策都可能解除 v0.8.6 插件托管发布的阻塞；维护者在这条链上的吞吐量是值得关注的关键瓶颈。

**健康度说明：** 该项目在架构上雄心勃勃，且活跃度很有吸引力，但**合并与开启比例（2:43）**、大量 `needs-author-action` / `needs-maintainer-review` 标签，以及多个已开启 1–2 个月的 XL PR，表明存在**审查瓶颈**。清理插件栈和长期停滞的 [#9809](https://github.com/zeroclaw-labs/zeroclaw/pull/9809) 将实质性改善发布节奏和贡献者留存。

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*