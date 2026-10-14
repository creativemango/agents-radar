# OpenClaw 生态日报 2026-10-14

> Issues: 500 | PRs: 500 | 覆盖项目: 5 个 | 生成时间: 2026-10-14 01:21 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw 项目深度报告

# OpenClaw 项目摘要 — 2026-10-14

_范围：基于所提供的 OpenClaw GitHub 快照。计数反映过去 24 小时；issue/PR 列表是所提供样本中按评论数或新近度排序的顶部条目。_

## 1. 今日概览

OpenClaw 仍然是一个活跃度非常高的项目：过去 24 小时内有 500 个 issue 和 500 个 PR 被更新，其中 208 个 issue 已关闭，185 个 PR 已合并/关闭。发布了两个版本，包括稳定版 `v2026.10.3` 和 beta 版 `v2026.10.5-beta.3`，两者都强调更新/安装持久性和运行时稳定性。项目社区参与度很强——仅 `v2026.10.3` 就报告了 90 次提交、79 个 PR 和 99 位贡献者——但开放的 backlog 仍然积压着大量 P0/P1 可靠性 issue。主要风险主题是更新失败、会话/消息丢失、启动性能回归、资源泄漏，以及 Windows 和 macOS 上的平台特定问题。健康评估：活跃且贡献充足，但稳定性债务集中在核心运维路径上，并且若干高严重性 issue 仍缺少新的修复 PR。

## 2. 发布

### [v2026.10.5-beta.3](https://github.com/openclaw/openclaw/releases/tag/v2026.10.5-beta.3)
- **类型：** beta/预发布。
- **所提供说明中的亮点：** 更新旨在让 Gateway 保持运行；pnpm 12 安装会让 Gateway 服务和 `openclaw` 启动器位于持久安装路径上；Windows 更新接受符合条件的 Scheduled Task 账户及其自身已验证的驱动；macOS 接受受保护的 Bun 服务配置。
- **破坏性变更：** 所提供片段中未明确说明。
- **迁移说明：** 平台特定的更新器变更表明，使用 pnpm 12、Windows Scheduled Task 安装和 macOS Bun 管理服务的用户应密切关注此 beta。所提供数据中未列出手动迁移步骤。

### [v2026.10.3](https://github.com/openclaw/openclaw/releases/tag/v2026.10.3)
- **类型：** 稳定版发布。
- **规模：** 90 次提交、79 个 PR、99 位贡献者。
- **变更：** 发布说明和 changelog 被描述为两种格式的相同内容；完整细节通过项目发布/文档站点链接。
- **破坏性变更：** 所提供片段中未明确说明。
- **迁移说明：** 所提供片段中未明确说明。
- **文档引用：** [发布说明](https://docs.openclaw.ai/releases/2026.10.3)

## 3. 项目进展

- **总体吞吐量：** 过去 24 小时内 185 个 PR 已合并/关闭，208 个 issue 已关闭。
- **样本中可见的已关闭/已落地条目：**
  - [#97616](https://github.com/openclaw/openclaw/issues/97616) — 僵尸/未回收的 hook 和工具子进程。
  - [#47910](https://github.com/openclaw/openclaw/issues/47910) — 按故障类别进行提供商回退。
  - [#154834](https://github.com/openclaw/openclaw/issues/154834) — 子代理投递失败在运行时上下文中反复出现。
  - [#45508](https://github.com/openclaw/openclaw/issues/45508) — WebChat 中自托管 STT/TTS。
  - [#152839](https://github.com/openclaw/openclaw/issues/152839) — 为 gateway 状态锁优雅处理 `openat2` `ENOSYS`。
  - [#103198](https://github.com/openclaw/openclaw/issues/103198) — WebChat 图片附件映射到 media store 路径。
  - [#87763](https://github.com/openclaw/openclaw/issues/87763) — SSRF DNS 调度器与 `autoSelectFamily` 超时冲突。
  - [#170724](https://github.com/openclaw/openclaw/issues/170724) — macOS 2026.10.2 应用更新不完整。
  - [#124731](https://github.com/openclaw/openclaw/issues/124731) — `claude-cli` 队列模式 steering 静默降级。
  - 样本中可见的已关闭 PR：[#137878](https://github.com/openclaw/openclaw/pull/137878)（CLI 审批 Scope 列）和 [#171525](https://github.com/openclaw/openclaw/pull/171525)（Control UI locale 刷新）。

- **审查中/推进中的高信号开放 PR：**
  - [#170534](https://github.com/openclaw/openclaw/pull/170534) — 尊重静默轮次结束，不编造答案。
  - [#171526](https://github.com/openclaw/openclaw/pull/171526) — 从 Model Setup 登录 Muse Code。
  - [#169722](https://github.com/openclaw/openclaw/pull/169722) — 在配对设备的现有文件夹中运行会话。
  - [#171476](https://github.com/openclaw/openclaw/pull/171476) — 在 CI 中验证 Android 手机和 Wear 发布。
  - [#171497](https://github.com/openclaw/openclaw/pull/171497) — 将 Doctor 清理结算回移到 2026.10.5。
  - [#171135](https://github.com/openclaw/openclaw/pull/171135) — 让 Workboard 聊天目录读取不进入卡片历史。
  - [#171447](https://github.com/openclaw/openclaw/pull/171447) — 在启动缓慢后恢复仅 ACP 的存储。
  - [#171510](https://github.com/openclaw/openclaw/pull/171510) — 将捆绑的 inference/logout SQLite 移入 auth worker。
  - [#171529](https://github.com/openclaw/openclaw/pull/171529) — 当聊天内容不足时加载更早的 Control UI 消息。
  - [#171530](https://github.com/openclaw/openclaw/pull/171530) — 减少模型加载后 New Session 的输入延迟。
  - [#171531](https://github.com/openclaw/openclaw/pull/171531) — 将十月更新/模型/稳定性修复回移到 2026.9.33。
  - [#164442](https://github.com/openclaw/openclaw/pull/164442) — 长 transcript 在解码期间钉住 DB 读快照。

## 4. 社区热门话题

### 所提供样本中按评论/反应数最活跃的 issue

| Issue | Status | Comments / 👍 | Signal |
|---|---:|---:|---|
| [#148707](https://github.com/openclaw/openclaw/issues/148707) — 回复丢失，报“no active tool authority snapshot” | OPEN | 19 / 0 | P1 会话/消息丢失回归，发生于 2026.9.4 |
| [#97616](https://github.com/openclaw/openclaw/issues/97616) — 未回收的 hook/工具子进程 | CLOSED | 19 / 1 | 进程生命周期/僵尸进程累积 |
| [#38327](https://github.com/openclaw/openclaw/issues/38327) — google-vertex/gemini 报“Cannot convert undefined or null to object” | OPEN | 18 / 3 | P0 认证/提供商回归，高反应信号 |
| [#114612](https://github.com/openclaw/openclaw/issues/114612) — memory-core SQLite 无限增长 | OPEN | 17 / 0 | 无保留策略导致磁盘写满风险 |
| [#154572](https://github.com/openclaw/openclaw/issues/154572) — `sessions_spawn` 到 claude-cli 子进程失败 | OPEN | 15 / 0 | 子代理/会话记录写入器失败 |
| [#156112](https://github.com/openclaw/openclaw/issues/156112) — `openclaw update` 在全局安装交换阶段失败 | OPEN | 15 / 0 | P0 更新器失败，而 npm install 成功 |
| [#155859](https://github.com/openclaw/openclaw/issues/155859) — Gateway 启动随插件数量增长 | OPEN | 14 / 0 | P0 启动/插件性能回归 |
| [#144291](https://github.com/openclaw/openclaw/issues/144291) — 配置热重载中止进行中的轮次 | OPEN | 13 / 0 | 配置/会话稳定性 |
| [#47910](https://github.com/openclaw/openclaw/issues/47910) — 按故障类别进行提供商回退 | CLOSED | 13 / 0 | 提供商韧性功能请求已落地/关闭 |
| [#118839](https://github.com/openclaw/openclaw/issues/118839) — 重启恢复 claim 在接管前发生变化 | OPEN | 13 / 0 | 重启恢复/会话状态回归 |
| [#168955](https://github.com/openclaw/openclaw/issues/168955) — Windows 上启动就绪约 1030 秒，对比约 410 秒 | OPEN | 12 / 0 | P1 Windows 启动回归，更新于 2026-10-14 |
| [#101929](https://github.com/openclaw/openclaw/issues/101929) — 上下文溢出估算器高估 | OPEN | 12 / 1 | Token 计数/会话状态 |
| [#163029](https://github.com/openclaw/openclaw/issues/163029) — 回合中途外部插件重载卡住 | OPEN | 11 / 0 | 主线程卡顿 40–70 秒 |
| [#158390](https://github.com/openclaw/openclaw/issues/158390) — plugin-captures 临时目录未被 GC | OPEN | 11 / 1 | P0 磁盘写满风险 |
| [#161379](https://github.com/openclaw/openclaw/issues/161379) — 模型目录刷新循环占满 CPU | OPEN | 11 / 0 | P1 CPU 耗尽/回归 |
| [#166894](https://github.com/openclaw/openclaw/issues/166894) — Windows gateway 重启挂起 30 分钟–2 小时 | OPEN | 9 / 0 | P0 Windows 多代理安装 |
| [#170331](https://github.com/openclaw/openclaw/issues/170331) — beta 更新候选配置读取退出码 1 | OPEN | 9 / 0 | Beta 更新器/systemd unit 问题 |
| [#48709](https://github.com/openclaw/openclaw/issues/48709) — Gemini 2.5 Pro textSignature 膨胀 | OPEN | 8 / 0 | 会话膨胀/TG 投递失败，更新于 2026-10-14 |
| [#99659](https://github.com/openclaw/openclaw/issues/99659) — companion app 连接后 OOM killed | OPEN | 7 / 1 | 内存飙升/崩溃，更新于 2026-10-14 |

### 快照中值得注意的 PR
未提供 PR 的评论/反应数。根据状态和近期活动，值得注意的 PR 包括 [#170534](https://github.com/openclaw/openclaw/pull/170534)、[#171526](https://github.com/openclaw/openclaw/pull/171526)、[#169722](https://github.com/openclaw/openclaw/pull/169722)、[#171489](https://github.com/openclaw/openclaw/pull/171489)、[#171531](https://github.com/openclaw/openclaw/pull/171531)、[#171510](https://github.com/openclaw/openclaw/pull/171510)、[#171528](https://github.com/openclaw/openclaw/pull/171528)、[#171529](https://github.com/openclaw/openclaw/pull/171529) 和 [#171530](https://github.com/openclaw/openclaw/pull/171530)。

**底层需求：** 最热门的 issue 都集中在核心交付路径的信任上——消息回复、子代理完成、重启恢复、提供商认证、更新器可靠性，以及磁盘/内存生命周期。用户并不是首先要求装饰性功能；他们需要 OpenClaw 保持会话存活、避免静默消息丢失、安全更新，并避免资源无限增长。

## 5. Bug 与稳定性

### 快照中的 P0 issue
- [#156112](https://github.com/openclaw/openclaw/issues/156112) — `openclaw update` 在 npm 全局安装的全局安装交换阶段失败。标记为 `clawsweeper:manual-only`。
- [#155859](https://github.com/openclaw/openclaw/issues/155859) — Gateway 启动墙钟时间随启用的插件数量增长；discord/codex/openclaw-weixin 占据主要开销。标记为 `no-new-fix-pr`。
- [#158390](https://github.com/openclaw/openclaw/issues/158390) — plugin-captures 临时目录未被 GC；磁盘无限增长。标记为 `no-new-fix-pr`。
- [#166894](https://github.com/openclaw/openclaw/issues/166894) — Windows 多代理 gateway 重启挂起 30 多分钟到 2 小时。标记为 `needs-info`。
- [#167557](https://github.com/openclaw/openclaw/issues/167557) — 延迟的插件数据/设置迁移永不收敛；包恢复租约身份错误。
- [#166221](https://github.com/openclaw/openclaw/issues/166221) — 更新 2026.9.4 → 2026.9.8 在 SQLite 预检/候选快照期间失败。
- [#161921](https://github.com/openclaw/openclaw/issues/161921) — 在 `state-migrated-no-rollback` 后，Doctor 和插件更新相互依赖。
- [#160060](https://github.com/openclaw/openclaw/issues/160060) — Windows gateway 在待机恢复后静默死亡；没有退出日志/WER 转储。
- [#161436](https://github.com/openclaw/openclaw/issues/161436) — cloud-worker 放置绕过 `before_tool_call`，因此策略插件无法保护远程 worker。
- [#137085](https://github.com/openclaw/openclaw/issues/137085) — macOS 应用设备身份卡在 `native-importing`；原生 gateway 握手超时。
- [#38327](https://github.com/openclaw/openclaw/issues/38327) — google-vertex/gemini-3.1-pro-preview 回归，报“Cannot convert undefined or null to object”。标记为 `no-new-fix-pr`、`needs-maintainer-review`。

### P1 / 高影响 issue
- [#148707](https://github.com/openclaw/openclaw/issues/148707) — 第二次运行挤掉进行中的轮次时回复丢失；消息丢失回归。标记为 `no-new-fix-pr`。
- [#154572](https://github.com/openclaw/openclaw/issues/154572) — `sessions_spawn` 到 claude-cli 子进程失败，报 `SessionTranscriptWriterClaimReboundError`。
- [#144291](https://github.com/openclaw/openclaw/issues/144291) — 配置热重载会中止每个进行中的 agent 轮次。
- [#118839](https://github.com/openclaw/openclaw/issues/118839) — 在 WebChat → Telegram 绑定的会话上，重启恢复 claim 在 agent 接管前发生变化。
- [#163029](https://github.com/openclaw/openclaw/issues/163029) — 回合中途外部插件重载导致主线程卡顿 40–70 秒。
- [#125764](https://github.com/openclaw/openclaw/issues/125764) — Telegram 出站发送在一次网络故障后被死信；公告/完成回复丢失。
- [#161379](https://github.com/openclaw/openclaw/issues/161379) — Gateway 在 prepared 模型目录刷新循环中占满一个 CPU 核心。
- [#159512](https://github.com/openclaw/openclaw/issues/159512) — claude-cli 子代理完成会两次使请求方 CLI 会话失效。
- [#90840](https://github.com/openclaw/openclaw/issues/90840) — 子代理运行完成时交付的是原始 worker 输出，而不是父级摘要。
- [#126950](https://github.com/openclaw/openclaw/issues/126950) — Telegram 不透明回调 payload 对非插件调用方被静默丢弃。
- [#155119](https://github.com/openclaw/openclaw/issues/155119) — 子代理最终答案丢失/截断；重复 settle 事件；投递状态卡在 pending。
- [#144336](https://github.com/openclaw/openclaw/issues/144336) — 延迟的配置重载重启在关闭步骤失败，报全局单例生命周期错误。
- [#128809](https://github.com/openclaw/openclaw/issues/128809) — Slack socket-mode 重连泄漏 socket，直到 10 连接上限。
- [#149564](https://github.com/openclaw/openclaw/issues/149564) — cron lane `scheduledToolPolicy.mode="account"` 解析为拒绝所有工具。
- [#168955](https://github.com/openclaw/openclaw/issues/168955) — Windows 启动回归：就绪约 1030 秒，对比约 410 秒；`sidecars.control-ui-assets` 阻塞 477 秒。

### 崩溃、回归和资源风险
- 崩溃/OOM：[#99659](https://github.com/openclaw/openclaw/issues/99659) companion app 连接后 OOM；[#160060](https://github.com/openclaw/openclaw/issues/160060) Windows 静默死亡；[#166221](https://github.com/openclaw/openclaw/issues/166221) 更新崩溃/失败。
- 回归：[#148707](https://github.com/openclaw/openclaw/issues/148707) 2026.9.4 回归；[#38327](https://github.com/openclaw/openclaw/issues/38327) 提供商回归；[#155859](https://github.com/openclaw/openclaw/issues/155859) 2026.9.5 启动回归；[#168955](https://github.com/openclaw/openclaw/issues/168955) 2026.10.1 启动回归；[#163029](https://github.com/openclaw/openclaw/issues/163029) 2026.9.7 插件重载回归；[#161379](https://github.com/openclaw/openclaw/issues/161379) 2026.9.6 CPU 循环；[#144291](https://github.com/openclaw/openclaw/issues/144291) 2026.9.3 热重载回归。
- 资源泄漏：[#97616](https://github.com/openclaw/openclaw/issues/97616) 僵尸进程（已关闭），[#114612](https://github.com/openclaw/openclaw/issues/114612) SQLite 无限增长，[#158390](https://github.com/openclaw/openclaw/issues/158390) plugin-captures 磁盘写满，[#164294](https://github.com/openclaw/openclaw/issues/164294) WAL checkpoint 永不完成。

### 修复 PR 覆盖情况
若干可见 PR 针对稳定性：[#170534](https://github.com/openclaw/openclaw/pull/170534)（静默轮次结束）、[#171447](https://github.com/openclaw/openclaw/pull/171447)（仅 ACP 存储恢复）、[#171497](https://github.com/openclaw/openclaw/pull/171497)（Doctor 清理）、[#171510](https://github.com/openclaw/openclaw/pull/171510)（auth SQLite worker）、[#171528](https://github.com/openclaw/openclaw/pull/171528)（原生应用重连消息）、[#171529](https://github.com/openclaw/openclaw/pull/171529)（Control UI 更早消息）、[#171530](https://github.com/openclaw/openclaw/pull/171530)（New Session 输入延迟）、[#171531](https://github.com/openclaw/openclaw/pull/171531)（十月回移）、[#171402](https://github.com/openclaw/openclaw/pull/171402)（内存卡顿）和 [#164442](https://github.com/openclaw/openclaw/pull/164442)（transcript DB 快照）。然而，上述许多 P0/P1 issue 被明确标记为 `clawsweeper:no-new-fix-pr` 或 `needs-maintainer-review`，因此修复覆盖并不均衡。

## 6. 功能请求与路线图信号

### 提供商/模型集成
- [#155633](https://github.com/openclaw/openclaw/issues/155633) — 将 Databricks Unity Gateway 添加为官方模型提供商；关联实现 [#155634](https://github.com/openclaw/openclaw/pull/155634)。
- [#47910](https://github.com/openclaw/openclaw/issues/47910) — 按故障类别进行提供商回退；已关闭，可能已体现在提供商韧性工作中。
- [#148397](https://github.com/openclaw/openclaw/pull/148397) — qwen 聊天模板模型遵循声明的 reasoning effort。
- [#171526](https://github.com/openclaw/openclaw/pull/171526) — 从 Model Setup 登录 Muse Code。
- [#171531](https://github.com/openclaw/openclaw/pull/171531) — 回移包括 Muse Wonder、MiniMax M3.1 thinking、MiniMax OAuth 刷新。

### 会话、工作区和代理
- [#169722](https://github.com/openclaw/openclaw/pull/169722) — 在配对设备的现有文件夹中运行会话。
- [#105617](https://github.com/openclaw/openclaw/pull/105617) — 精确轮次能力投影报告。
- [#90840](https://github.com/openclaw/openclaw/issues/90840) — 子代理完成应返回父级/主摘要，而不是原始 worker 输出。

### UI / UX / 文档
- [#111143](https://github.com/openclaw/openclaw/issues/111143) — 在 Control UI 侧边栏固定插件标签页。
- [#168703](https://github.com/openclaw/openclaw/pull/168703) — 简化 Settings 和配置帮助。
- [#168693](https://github.com/openclaw/openclaw/pull/168693) — 简化快速开始和提供商设置。
- [#168702](https://github.com/openclaw/openclaw/pull/168702) — 简化参考和 CI 指南。
- [#171529](https://github.com/openclaw/openclaw/pull/171529)、[#171530](https://github.com/openclaw/openclaw/pull/171530)、[#171533](https://github.com/openclaw/openclaw/pull/171533) — Control UI 打磨/性能。

### 安全与插件边界
- [#91676](https://github.com/openclaw/openclaw/issues/91676) — 可选启用 `skills.workshop.allowSandboxedAgents`。
- [#161436](https://github.com/openclaw/openclaw/issues/161436) — cloud-worker 放置绕过 `before_tool_call`。
- [#151345](https://github.com/openclaw/openclaw/issues/151345) — 当插件注册修改出站 hook 时，Telegram 进度草稿被抑制。
- [#171532](https://github.com/openclaw/openclaw/pull/171532) — 可移植 Claw manifest 类别。

**预测：** 下一个发布窗口很可能优先考虑更新/安装可靠性、会话恢复、提供商/模型集成和 Control UI 性能。如果维护者优先考虑策略插件覆盖，围绕 cloud worker 和沙箱代理的安全边界问题可能会变得更加突出。

## 7. 用户反馈摘要

**快照中的主要痛点：**
- **更新器/安装可靠性：** 用户报告在 npm、pnpm、Windows Scheduled Tasks、macOS 应用/运行时不匹配以及 beta 候选配置读取中出现更新失败：[#156112](https://github.com/openclaw/openclaw/issues/156112)、[#166221](https://github.com/openclaw/openclaw/issues/166221)、[#170331](https://github.com/openclaw/openclaw/issues/170331)、[#170332](https://github.com/openclaw/openclaw/issues/170332)、[#170724](https://github.com/openclaw/openclaw/issues/170724)。
- **消息/会话丢失：** 多份报告显示回复、Telegram 发送、子代理完成和排队 steering 被静默丢失或死信：[#148707](https://github.com/openclaw/openclaw/issues/148707)、[#125764](https://github.com/openclaw/openclaw/issues/125764)、[#155119](https://github.com/openclaw/openclaw/issues/155119)、[#126950](https://github.com/openclaw/openclaw/issues/126950)、[#124731](https://github.com/openclaw/openclaw/issues/124731)。
- **性能/启动回归：** 插件数量、模型目录刷新、Windows 启动和外部插件重载导致严重卡顿：[#155859](https://github.com/openclaw/openclaw/issues/155859)、[#168955](https://github.com/openclaw/openclaw/issues/168955)、[#161379](https://github.com/openclaw/openclaw/issues/161379)、[#163029](https://github.com/openclaw/openclaw/issues/163029)。
- **资源耗尽：** 僵尸进程、SQLite 增长、WAL checkpoint 失败和 plugin-capture 临时目录带来运维风险：[#97616](https://github.com/openclaw/openclaw/issues/97616)、[#114612](https://github.com/openclaw/openclaw/issues/114612)、[#164294](https://github.com/openclaw/openclaw/issues/164294)、[#158390](https://github.com/openclaw/openclaw/issues/158390)。
- **平台特定摩擦：** Windows 待机/重启和 macOS 身份/更新问题反复出现：[#166894](https://github.com/openclaw/openclaw/issues/166894)、[#160060](https://github.com/openclaw/openclaw/issues/160060)、[#137085](https://github.com/openclaw/openclaw/issues/137085)、[#99659](https://github.com/openclaw/openclaw/issues/99659)。
- **安全/策略缺口：** 用户希望护栏能一致地应用于 cloud worker、沙箱代理和出站插件 hook：[#161436](https://github.com/openclaw/openclaw/issues/161436)、[#91676](https://github.com/openclaw/openclaw/issues/91676)、[#151345](https://github.com/openclaw/openclaw/issues/151345)。

**满意度信号：** 贡献者数量高、长期 issue 被积极关闭、多次发布以及可见的回移/稳定性 PR 表明维护者响应积极。  
**不满意度信号：** 跨版本反复出现回归、P0 issue 没有修复 PR、长期存在的 P1/P2 报告，以及自托管/多代理/Windows 用户承受的运维负担。

## 8. Backlog 观察

需要维护者关注的重要 issue/PR，尤其是较老的高严重性条目，或被标记为 `no-new-fix-pr` / `needs-maintainer-review` 的条目。

| Item | Severity / Type | Age in snapshot | Why watch |
|---|---|---|---|
| [#38327](https://github.com/openclaw/openclaw/issues/38327) | P0 bug/回归 | 创建于 2026-03-06 | 18 条评论，3 👍，提供商认证失败，无新修复 PR |
| [#114612](https://github.com/openclaw/openclaw/issues/114612) | P1 磁盘增长 | 创建于 2026-07-27 | 17 条评论；缺少 SQLite 保留策略 |
| [#90840](https://github.com/openclaw/openclaw/issues/90840) | P1 子代理泄漏 | 创建于 2026-06-06 | 9 条评论；向用户输出原始 worker 输出，影响安全/会话 |
| [#125764](https://github.com/openclaw/openclaw/issues/125764) | P1 消息丢失 | 创建于 2026-08-18 | 11 条评论；Telegram 一次尝试后即进入死信 |
| [#128809](https://github.com/openclaw/openclaw/issues/128809) | P1 通道稳定性 | 创建于 2026-08-24 | 6 条评论；Slack socket 泄漏直至连接上限 |
| [#137085](https://github.com/openclaw/openclaw/issues/137085) | P0 macOS 应用 | 创建于 2026-09-03 | 7 条评论；设备身份卡住且 gateway 握手超时 |
| [#91676](https://github.com/openclaw/openclaw/issues/91676) | P2 安全/插件 | 创建于 2026-06-09 | 7 条评论；沙箱代理 skill workshop 可选启用 |
| [#48709](https://github.com/openclaw/openclaw/issues/48709) | P2 会话膨胀 | 创建于 2026-03-17 | 8 条评论；陈旧但仍在 2026-10-14 更新 |
| [#99659](https://github.com/openclaw/openclaw/issues/99659) | P2 崩溃/OOM | 创建于 2026-07-03 | 7 条评论；companion app 内存飙升/OOM |
| [#105617](https://github.com/openclaw/openclaw/pull/105617) | P3 PR | 创建于 2026-07-12 | 长期开放的能力投影报告；需要证明 |
| [#145242](https://github.com/openclaw/openclaw/pull/145242) | P2 PR | 创建于 2026-09-11 | Heartbeat 跳过被记录为失败；需要证明 |
| [#148397](https://github.com/openclaw/openclaw/pull/148397) | P2 PR | 创建于 2026-09-14 | Qwen reasoning effort 支持；需要证明 |

**Backlog 结论：** 项目并不缺少已报告的信号——缺少的是解决少量高影响、长期存在的运维 bug 的带宽。维护者的注意力应首先集中在 P0 更新/会话/启动失败和资源生命周期问题上，因为相比新的表面功能，这些问题更影响用户对日常 OpenClaw 使用的信任。

---

## 横向生态对比

# 跨项目对比报告 — 2026-10-14

## 1. 生态系统概览
个人 AI 助手 / agent 开源格局正分化为全功能助手平台，以及更聚焦的 agent 运行时、网关和扩展框架。OpenClaw 在活跃度和发布节奏上占据主导，而 Hermes Agent、QwenPaw、ZeroClaw 和 IronClaw 则占据更窄但更明确的细分生态位，分别围绕网关编排、控制台 UX、插件安全和声明式可扩展性。跨项目来看，社区讨论已从单纯获取功能转向运营信任：会话持久性、更新/安装可靠性、成本可观测性、策略执行和资源生命周期管理。发布节奏并不均衡——该时间窗口内只有 OpenClaw 发布了版本——而中游项目则深陷问题分诊和 PR 流水线。生态系统正在从“agent 能做什么？”走向“它能否每天可靠、安全且成本可控地运行？”

## 2. 活跃度对比
健康分是分析师综合评分（0–10），权重包括 24 小时吞吐量、关闭/合并比率、发布节奏、未决关键风险和积压时长。它不是官方项目指标。

| 项目 | 24 小时 Issues / 已关闭 | 24 小时 PRs / 已合并-关闭 | 未决积压 | 发布状态 | 健康分 |
|---|---:|---:|---|---|---:|
| **OpenClaw** | 500 / 208 | 500 / 185 | 大量 P0/P1；未提供确切未决数量 | `v2026.10.3` 稳定版；`v2026.10.5-beta.3` | **7.0** |
| **Hermes Agent** | 50 / 4 | 50 / 3 | 46 个 issue，47 个 PR | 无 | **6.5** |
| **IronClaw** | 1 / 0 | 1 / 0 | 1 个 issue，1 个 PR | 无 | **7.0** |
| **QwenPaw** | 20 / 4 | 42 / 20 | 16 个 issue，22 个 PR | 无 | **7.5** |
| **ZeroClaw** | 24 / 0 | 50 / 5 | 24 个 issue，45 个 PR | 无 | **6.0** |

**解读：** OpenClaw 的活跃度约为中游项目的 10 倍。QwenPaw 在中游同行中合并吞吐量最强。ZeroClaw 有庞大的 PR 流水线，但今天没有 issue 关闭，表明存在审查/发布门禁压力。IronClaw 稳定但安静。

## 3. OpenClaw 的定位
**相对同行的优势：** OpenClaw 在规模和广度上显然是生态系统领导者。在一个 24 小时窗口内，它关闭了 208 个 issue，合并/关闭了 185 个 PR，并同时发布了稳定版和 beta 版；仅其稳定版就报告了 99 位贡献者。没有同行能接近这种贡献者或吞吐量规模。OpenClaw 的覆盖面也最广：网关、Control UI、WebChat、Telegram/Slack、子 agent、插件、模型提供商、更新器/Doctor，以及 macOS/Windows/移动端相关的 CI 验证。

**技术路线差异：** OpenClaw 是集成的个人助手平台，而非单一用途的 agent 框架。同行更聚焦：Hermes Agent 强调网关/cron、委托、压缩和桌面端；QwenPaw 强调控制台 UX、插件市场、ACP 和 Code Mode；ZeroClaw 聚焦运行时/网关分离、WASM 插件托管以及成本/会话正确性；IronClaw 提出声明式托管 MCP 扩展，并保持清晰的主机/扩展边界。

**社区规模对比：** 从活跃度看，OpenClaw 的社区规模大得多——可能比 Hermes、QwenPaw 或 ZeroClaw 大一个数量级，并远超 IronClaw。它的主要风险不是采用度，而是稳定性债务：围绕更新失败、消息/会话丢失、启动回归和资源泄漏的 P0/P1 issue 仍然未决，且常被标记为 `no-new-fix-pr` 或 `needs-maintainer-review`。

## 4. 共同技术关注领域
多个项目共同浮现的需求：

- **会话/消息持久性与恢复：** OpenClaw（丢失回复、重启恢复）、Hermes（检查点快照、提示词静默丢失、网关退出）、QwenPaw（静默空补全、会话卡死、cron 取消）、ZeroClaw（SQLite `created_at` 重写、修剪删除较早轮次、已退役网关套接字）。
- **更新/安装/部署可靠性：** OpenClaw（npm/pnpm/Windows/macOS 更新失败）、Hermes（误报 Remote Desktop 更新失败、Docker `usermod` 挂起）、QwenPaw（Windows 文件锁定、反向代理基名、GitHub 技能二进制损坏）、ZeroClaw（v0.8.6 发布加固、插件宿主产物、快速启动插件安装）。
- **成本可观测性、token 计量与提示词缓存效率：** ZeroClaw（OpenRouter $0.00 花费、丢弃 `total_tokens`、忽略 `cost.warn_at_percent`、提示词缓存失效）、OpenClaw（上下文溢出估算器、模型目录 CPU）、Hermes（Codex 配额/重置）、QwenPaw（模型能力过滤）。
- **安全边界与插件治理：** 全部五个项目。OpenClaw（策略钩子 vs 云 worker、沙箱化 agent）、Hermes（Managed Scope `terminal.*` 绕过、依赖公告）、IronClaw（托管 MCP 的声明式认证/出站）、QwenPaw（技能路径净化、插件来源/热重载）、ZeroClaw（WASM 插件宿主、出站拒绝、TLS 配置文件、作用域配对 token）。
- **模型/提供商路由与灵活性：** OpenClaw（按故障类别进行提供商回退、Gemini/Vertex 认证）、Hermes（按调用覆盖模型/推理、提供商命名空间）、QwenPaw（默认 agent 模型编辑）、ZeroClaw（模型路由配置、Anthropic thinking/cache）。
- **资源生命周期与性能：** OpenClaw（启动开销随插件增长、SQLite 增长、临时目录泄漏、CPU 循环）、Hermes（内存限制、cron O(profiles) 重新解析）、QwenPaw（base64 历史膨胀、轮询）、ZeroClaw（JPEG 内存上限、系统提示词重建）。
- **异步/后台/cron UX：** QwenPaw（任务回调、可取消下载、cron 取消）、Hermes（cron 重试/性能、网关排空）、ZeroClaw（SOP 无头运行、Telegram 429 处理）。
- **平台/部署打磨：** OpenClaw（Windows/macOS）、Hermes（macOS 网关、桌面端）、QwenPaw（Windows、Docker/NAS、气隙隔离企业环境）、ZeroClaw（Telegram/WebSocket 网关）。
- **子 agent/委托生命周期：** OpenClaw（子 agent 投递、父级摘要）、Hermes（委托覆盖、子 agent 生命周期门面）。

## 5. 差异化分析

| 项目 | 主要焦点 | 目标用户 | 架构 / 方法 |
|---|---|---|---|
| **OpenClaw** | 完整的个人 AI 助手平台 | 自托管用户、高级用户、插件/agent 生态 | 集成网关、会话、Control UI、多渠道、插件/技能、多平台更新器 |
| **Hermes Agent** | Agent 框架 + 网关/cron/桌面端 | Agent 构建者、贡献者、Docker/远程桌面用户 | 网关/多路复用、委托、压缩、提供商认证、语音、桌面端 |
| **IronClaw** | 安全的声明式可扩展性 | 扩展/配套工具构建者、注重安全的团队 | 主机 + 可选 Plazmi 扩展、托管 MCP 元数据、声明式认证/出站 |
| **QwenPaw** | 控制台 UX + 插件/技能市场 | 控制台用户、插件开发者、企业/气隙部署 | AgentScope Python 技术栈、控制台、ACP、SQLite 历史、市场、Code Mode |
| **ZeroClaw** | 运行时/网关分离 + 成本/会话正确性 | 对成本敏感的高级用户、网关运维者 | v0.8.6/v0.9.0 发布列车、WASM 插件宿主、模型路由、会话存储、Telegram |

**关键结论：** OpenClaw 在广度和集成度上竞争；Hermes 在 agent 编排灵活性上竞争；QwenPaw 在控制台和插件市场体验上竞争；ZeroClaw 在运行时/网关架构和成本保真度上竞争；IronClaw 在清晰的扩展边界和声明式安全表面上竞争。

## 6. 社区势能与成熟度
**活跃度分层：**
- **超大规模/平台领导者：** OpenClaw。快速迭代、频繁发布、贡献者基础最大，但稳定性债务明显。
- **高速中游：** QwenPaw、ZeroClaw、Hermes Agent。QwenPaw 展现出强劲的合并吞吐量；ZeroClaw 有庞大但受审查门禁限制的 PR 流水线；Hermes 以分诊为主，并有多个针对性修复 PR 正在推进。
- **安静/稳定：** IronClaw。活跃度低，没有报告 bug，但势能和维护者关注有限。

**成熟度评估：**
- **OpenClaw** 是最成熟、最宽广的平台，但按稳定性指标并非最健康。
- **QwenPaw** 正处于 2.2.x 活跃维护/稳定化阶段，关键的控制台/媒体/会话 bug 仍未解决。
- **ZeroClaw** 正在为 v0.8.6 进行发布加固，并为 v0.9.0 开展架构工作；许多 P1/S2 issue 仍未解决。
- **Hermes Agent** 正在积极修复稳定性和贡献者工作流问题，但相对于未决 PR，合并吞吐量较低。
- **IronClaw** 处于早期/安静状态；主要信号是一个等待分诊的可扩展性提案。

## 7. 趋势信号
1. **可靠性就是新功能。** 在 OpenClaw、Hermes、QwenPaw 和 ZeroClaw 中，用户更关心不丢消息、持久会话、重启恢复和安全更新，而不是新的 UI 界面。
2. **成本可观测性正在成为一等公民。** ZeroClaw 的 OpenRouter/token 计量问题、提示词缓存失效报告以及 `cost.warn_at_percent` 失败表明，开发者需要可信的计费和缓存指标。
3. **安全策略必须跟随执行上下文。** 云 worker、沙箱化 agent、终端作用域、MCP/出站、插件 TLS 和依赖公告是反复出现的边界缺口。
4. **插件生态需要生命周期成熟度。** 热重载、回滚安全、来源追溯、市场源、主题扩展点、WASM 宿主和按 agent 的 MCP 授权都是活跃需求领域。
5. **部署可移植性是阻塞项，而非打磨项。** Windows 文件锁定、macOS 身份/网关退出、Docker FUSE 挂起、反向代理子路径和气隙镜像都是反复出现的摩擦点。
6. **异步/后台工作需要更好的 UX。** 回调、可取消下载、cron 重试、静默取消警告和网关排空行为是常见请求。
7. **资源生命周期管理是运营风险。** SQLite 保留策略、临时目录 GC、内存限制、套接字泄漏、僵尸进程和 base64 历史膨胀在多个项目中出现。
8. **模型路由和提供商灵活性仍具战略意义。** 按故障类别回退、按调用覆盖、自定义提供商、能力感知媒体过滤和模型选择器过滤都是活跃的路线图信号。

**对 AI agent 开发者的价值：** 优先为失败而构建——幂等投递、持久会话存储、清晰的恢复路径、策略强制的执行边界、成本/token 可观测性、插件隔离以及跨平台安装/更新。解决这些运营信任问题的项目，将比那些只增加新 agent 能力的项目更具差异化。

---

## 同赛道项目详细报告

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目摘要 — 2026-10-14

## 1. 今日概览
Hermes Agent 依然高度活跃：**过去 24 小时内有 50 个 issue 和 50 个 PR 被更新**，**没有新版本发布**。积压仍然很大——**46 个开放/活跃 issue 和 47 个开放 PR**——而仅**有 4 个 issue 关闭、3 个 PR 合并/关闭**，表明这是以问题分诊为主的一天，合并吞吐量适中。活动集中在稳定性、兼容性、安全边界、网关/cron、Docker、桌面和提供商认证等问题上。若干 P1/P2 回归仍未关闭，但多个针对性修复 PR 正在进行中，表明修复工作活跃。

## 2. 发布
过去 24 小时内未发布新版本。没有需要报告的破坏性变更或迁移说明。

## 3. 项目进展
- **关闭/合并量：** 4 个 issue 关闭，3 个 PR 合并/关闭。所提供的 PR 数据集不包含已合并 PR 的标题，因此已合并 PR 的归因有限。
- **可见的 issue 关闭：**
  - [#62182](https://github.com/NousResearch/hermes-agent/issues/62182) — 已关闭：状态/错误显示不再以内部 `custom` 别名代替已配置的提供商名称。
  - [#77072](https://github.com/NousResearch/hermes-agent/issues/77072) — 已关闭：rclone FUSE 挂载导致 Docker `usermod` 挂起并阻塞 s6 启动。
- **今日更新的活跃修复/功能 PR：**
  - [#138130](https://github.com/NousResearch/hermes-agent/pull/138130) — 修复 API 调用前压缩超时处理。
  - [#138128](https://github.com/NousResearch/hermes-agent/pull/138128) — 在停用已耗尽的回退项前重新探测 Codex 配额。
  - [#138125](https://github.com/NousResearch/hermes-agent/pull/138125) — 将 Codex 重置冷却清除限定到执行兑换的账户。
  - [#138111](https://github.com/NousResearch/hermes-agent/pull/138111) — 让 `search_files` 忽略用户 ripgrep 配置。
  - [#138122](https://github.com/NousResearch/hermes-agent/pull/138122) — cron 多路复用性能：每个 tick 只解析一次主路由，属于 [#138105](https://github.com/NousResearch/hermes-agent/issues/138105) 的一部分。
  - [#138120](https://github.com/NousResearch/hermes-agent/pull/138120) 和 [#138124](https://github.com/NousResearch/hermes-agent/pull/138124) — 针对 [#138115](https://github.com/NousResearch/hermes-agent/issues/138115) 的 Docker 子目录提示路径映射。
  - [#138126](https://github.com/NousResearch/hermes-agent/pull/138126) — 仅语音自动回复。
  - [#138127](https://github.com/NousResearch/hermes-agent/pull/138127) — 可选启用的公开语音 Responses 流式传输。
  - [#110907](https://github.com/NousResearch/hermes-agent/pull/110907) — 精确值 `.env` 密钥脱敏。
  - [#137553](https://github.com/NousResearch/hermes-agent/pull/137553) — 网关因重启排空时拒绝内部唤醒。
  - [#118548](https://github.com/NousResearch/hermes-agent/pull/118548) — 针对 [#118536](https://github.com/NousResearch/hermes-agent/issues/118536) 的 cron 提供商不可达重试修复。
  - [#103373](https://github.com/NousResearch/hermes-agent/pull/103373) — 针对 [#103355](https://github.com/NousResearch/hermes-agent/issues/103355) 的压缩尝试预算重新启用。

## 4. 社区热点
数据集中未提供 PR 评论数；可见的最热讨论集中在 issue 中。

| 事项 | 信号 | 底层需求 |
|---|---|---|
| [#131859](https://github.com/NousResearch/hermes-agent/issues/131859) | 38 条评论，P2，已阻塞 | 贡献者工作流因 API `CreatePullRequest` 权限错误而受阻；需要认证/平台侧解决。 |
| [#5320](https://github.com/NousResearch/hermes-agent/issues/5320) | 13 条评论，3 👍 | 长时间运行的会话触及低内存限制；用户希望更大的默认值或自动扩缩。 |
| [#80222](https://github.com/NousResearch/hermes-agent/issues/80222) | 8 条评论，3 👍 | 委托无法按调用覆盖 model/reasoning；用户需要成本/延迟路由灵活性。 |
| [#106592](https://github.com/NousResearch/hermes-agent/issues/106592) | 8 条评论 | Remote Desktop 更新即使在 systemd 重启成功后仍报告失败；用户需要准确的更新状态。 |
| [#133212](https://github.com/NousResearch/hermes-agent/issues/133212) | 7 条评论 | 插件编排器需要稳定的 subagent 生命周期门面，用于 steer/隔离/路由/跟进。 |
| [#68536](https://github.com/NousResearch/hermes-agent/issues/68536) | 6 条评论 | 自定义提供商的 `/models` 状态过滤缺失，导致模型选择器被不可用模型淹没。 |
| [#137838](https://github.com/NousResearch/hermes-agent/issues/137838) | 6 条评论，P1 | macOS 网关在错过存活探针后反复以退出码 75 退出；根因仍未确认。 |
| [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) | 5 条评论 | 跟踪统一网关对所有本地会话的所有权；架构一致性与会话持久性。 |

## 5. Bug 与稳定性
根据提供的 issue/PR 元数据，按严重程度排序。

### P1
- [#137838](https://github.com/NousResearch/hermes-agent/issues/137838) — macOS 网关在错过存活探针后反复以退出码 75 退出；根因未确认。**未显示修复 PR。**
- [#138039](https://github.com/NousResearch/hermes-agent/issues/138039) — 超容量检查点存储将每个项目压平为一个快照，忽略 `max_snapshots`，导致 `/rollback` 静默丢失。**未显示修复 PR。**

### P2
- [#131859](https://github.com/NousResearch/hermes-agent/issues/131859) — 无法通过 API 打开 PR：`CreatePullRequest` 权限错误。**受阻；未显示修复 PR。**
- [#106592](https://github.com/NousResearch/hermes-agent/issues/106592) — Remote Desktop 更新在 systemd 重启成功后报告失败并留下过期的更新状态。**未显示修复 PR。**
- [#138044](https://github.com/NousResearch/hermes-agent/issues/138044) — 多路复用网关忽略 Managed Scope `terminal.*`；命令在宿主机上运行。**安全边界问题；未显示修复 PR。**
- [#138105](https://github.com/NousResearch/hermes-agent/issues/138105) — 多路复用 cron ticker 每 60 秒执行 O(profiles) 次配置重新解析和探测，导致 Discord 心跳饥饿。**部分修复 PR：** [#138122](https://github.com/NousResearch/hermes-agent/pull/138122)。
- [#138115](https://github.com/NousResearch/hermes-agent/issues/138115) — 使用 Docker 后端时，子目录 `AGENTS.md` 提示未命中容器绝对路径 `/workspace/...`。**修复 PR：** [#138120](https://github.com/NousResearch/hermes-agent/pull/138120)、[#138124](https://github.com/NousResearch/hermes-agent/pull/138124)。
- [#134394](https://github.com/NousResearch/hermes-agent/issues/134394) — 依赖固定版本带有未解决的安全公告：PyJWT、httpx2、urllib3、oauthlib。**未显示修复 PR。**
- [#138088](https://github.com/NousResearch/hermes-agent/issues/138088) — 远程文件夹选择器从受管理文件根目录之外启动。**未显示修复 PR。**
- [#138047](https://github.com/NousResearch/hermes-agent/issues/138047) — 桌面端新聊天中的第一条提示可能静默消失且从不持久化。**需要复现；未显示修复 PR。**
- [#137944](https://github.com/NousResearch/hermes-agent/issues/137944) — `edge-tts==7.2.7` 精确固定版本在 Microsoft `Sec-MS-GEC` 轮换后返回 403；7.3.0 已修复。**未显示修复 PR。**
- [#118536](https://github.com/NousResearch/hermes-agent/issues/118536) — cron 提供商不可达故障从不重试。**修复 PR：** [#118548](https://github.com/NousResearch/hermes-agent/pull/118548)。
- [#129450](https://github.com/NousResearch/hermes-agent/issues/129450) — 委托批次在每个任务都成功时报告“Every task failed”。**未显示修复 PR。**
- [#133421](https://github.com/NousResearch/hermes-agent/issues/133421) — 桌面端浏览器会话守卫拒绝真正获得焦点的聊天磁贴。**未显示修复 PR。**
- [#138108](https://github.com/NousResearch/hermes-agent/issues/138108) — 来自 eM Client 的 Matrix 消息解密失败；移除 Base64 填充可修复复现。**未显示修复 PR。**
- [#138094](https://github.com/NousResearch/hermes-agent/issues/138094) — Dashboard Plugins 页面将 agent 插件安装到 dashboard 自身的 profile，而不是所选 profile。**未显示修复 PR。**
- [#138090](https://github.com/NousResearch/hermes-agent/issues/138090) — `read_file` 对因权限/沙箱无法访问的文件报告“File not found”。**未显示修复 PR。**
- [#122182](https://github.com/NousResearch/hermes-agent/issues/122182) — 桌面端更新在仅本地的 parked 分支上因 HTTP 422 失败。**未显示修复 PR。**
- [#138053](https://github.com/NousResearch/hermes-agent/issues/138053) — `is_local_endpoint()` 将 Kubernetes `*.svc.cluster.local` 视为本地。**未显示修复 PR。**

### P3/P4 值得注意
- [#68536](https://github.com/NousResearch/hermes-agent/issues/68536) — `/model` 选择器显示不可用的 Shutdown/Retiring 模型。
- [#103355](https://github.com/NousResearch/hermes-agent/issues/103355) — 在插件上下文引擎下，压缩尝试预算从不重新启用。**修复 PR：** [#103373](https://github.com/NousResearch/hermes-agent/pull/103373)。
- [#138052](https://github.com/NousResearch/hermes-agent/issues/138052) — OpenCode relay 的无消息 400 envelope 会中止轮次并使自动标题失效。
- [#127856](https://github.com/NousResearch/hermes-agent/issues/127856) — Linux 启动器 `StartupWMClass` 不匹配会创建单独的 dock 条目。
- [#63681](https://github.com/NousResearch/hermes-agent/issues/63681) — ACP 会话持久化期间提供商命名空间丢失；具名自定义提供商恢复失败。

## 6. 功能请求与路线图信号
- [#5320](https://github.com/NousResearch/hermes-agent/issues/5320) — 提高/自动扩缩 `memory_char_limit` 并暴露使用压力。长期存在的 P3 功能，3 👍。
- [#80222](https://github.com/NousResearch/hermes-agent/issues/80222) — 在 `delegate_task` 上按调用覆盖 `model` 和 `reasoning_effort`。P2 `needs-decision`。
- [#133212](https://github.com/NousResearch/hermes-agent/issues/133212) — 面向插件编排器的 `subagent_lifecycle v1.1`：steer、宿主隔离传输、effort/route/follow-up。P3 `needs-decision`。
- [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) — 跟踪 issue：由一个网关拥有所有本地会话。这是架构方向，而非单一功能。

**根据活跃 PR 看，下一版本可能包含：** 仅语音自动回复（[#138126](https://github.com/NousResearch/hermes-agent/pull/138126)）、Responses 的公开语音流式传输（[#138127](https://github.com/NousResearch/hermes-agent/pull/138127)）、Codex 配额/重置修复（[#138128](https://github.com/NousResearch/hermes-agent/pull/138128)、[#138125](https://github.com/NousResearch/hermes-agent/pull/138125)）、网关排空期间拒绝唤醒（[#137553](https://github.com/NousResearch/hermes-agent/pull/137553)）、cron 重试/性能修复（[#118548](https://github.com/NousResearch/hermes-agent/pull/118548)、[#138122](https://github.com/NousResearch/hermes-agent/pull/138122)）、压缩重新启用（[#103373](https://github.com/NousResearch/hermes-agent/pull/103373)）、Docker 路径提示修复（[#138120](https://github.com/NousResearch/hermes-agent/pull/138120)、[#138124](https://github.com/NousResearch/hermes-agent/pull/138124)）以及 `.env` 密钥脱敏（[#110907](https://github.com/NousResearch/hermes-agent/pull/110907)）。内存限制和委托覆盖变更在落地前可能需要维护者决策。

## 7. 用户反馈总结
**痛点：**
- 贡献者因权限错误而无法通过 API 打开 PR（[#131859](https://github.com/NousResearch/hermes-agent/issues/131859)）。
- 长时间运行的会话过快触及内存限制（[#5320](https://github.com/NousResearch/hermes-agent/issues/5320)）。
- 委托过于不灵活，并且可能误报全部失败（[#80222](https://github.com/NousResearch/hermes-agent/issues/80222)、[#129450](https://github.com/NousResearch/hermes-agent/issues/129450)）。
- 桌面/网关可靠性仍令人担忧：虚假更新失败、提示静默丢失以及网关退出（[#106592](https://github.com/NousResearch/hermes-agent/issues/106592)、[#138047](https://github.com/NousResearch/hermes-agent/issues/138047)、[#137838](https://github.com/NousResearch/hermes-agent/issues/137838)）。
- Docker/多路复用用户面临路径解析和安全边界问题（[#138115](https://github.com/NousResearch/hermes-agent/issues/138115)、[#138044](https://github.com/NousResearch/hermes-agent/issues/138044)、[#138088](https://github.com/NousResearch/hermes-agent/issues/138088)）。
- TTS 用户因过时的 `edge-tts` 固定版本而受阻（[#137944](https://github.com/NousResearch/hermes-agent/issues/137944)）。
- 关注安全的用户指出固定版本依赖包中仍存在安全公告（[#134394](https://github.com/NousResearch/hermes-agent/issues/134394)）。

**满意信号：** 已关闭的 issue [#62182](https://github.com/NousResearch/hermes-agent/issues/62182) 和 [#77072](https://github.com/NousResearch/hermes-agent/issues/77072) 表明修复正在落地。**不满意信号：** P1 稳定性和数据丢失问题仍未关闭，相对于开放的 PR 积压，合并吞吐量偏低，且受阻的认证问题正在扰乱贡献者流程。

## 8. 积压事项观察
看起来需要维护者关注的重要事项：
- [#131859](https://github.com/NousResearch/hermes-agent/issues/131859) — 已阻塞的认证/API issue，有 38 条评论；对贡献者影响较大。
- [#5320](https://github.com/NousResearch/hermes-agent/issues/5320) — 自 2026-04-05 开放，P3，13 条评论，3 👍。
- [#80222](https://github.com/NousResearch/hermes-agent/issues/80222) — 自 2026-08-06 开放，P2 `needs-decision`，8 条评论。
- [#68536](https://github.com/NousResearch/hermes-agent/issues/68536) — 自 2026-07-21 开放，P3，6 条评论。
- [#63681](https://github.com/NousResearch/hermes-agent/issues/63681) — 自 2026-07-13 开放，P4，5 条评论。
- [#134394](https://github.com/NousResearch/hermes-agent/issues/134394) — P2 安全公告固定版本 issue，自 2026-10-07 开放。
- [#133212](https://github.com/NousResearch/hermes-agent/issues/133212) — P3 `needs-decision`，插件编排 API，自 2026-10-05 开放。
- [#134889](https://github.com/NousResearch/hermes-agent/issues/134889) — 统一网关跟踪 issue，自 2026-10-08 开放。
- 长期开放且需要审查/合并的 PR：[#95691](https://github.com/NousResearch/hermes-agent/pull/95691) 自 2026-08-26，[#114655](https://github.com/NousResearch/hermes-agent/pull/114655) 自 2026-09-18，[#110907](https://github.com/NousResearch/hermes-agent/pull/110907) 自 2026-09-14，[#99960](https://github.com/NousResearch/hermes-agent/pull/99960) 自 2026-09-01，以及 [#103373](https://github.com/NousResearch/hermes-agent/pull/103373) 自 2026-09-05。

---

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目简报 — 2026-10-14

## 今日概览
IronClaw 在过去 24 小时内活动较少：1 个未关闭 issue，1 个未关闭 PR，0 个已关闭 issue，0 个已合并/已关闭 PR，0 个新版本发布。实质性事项是一项关于可选 hosted-MCP Plazmi 扩展的提案，该扩展使用现有的声明式 auth 和 egress 接口，而不是向宿主添加厂商逻辑。唯一的 PR 是 Dependabot 对内部 docs/architecture-video 包中 `brace-expansion` 的依赖升级。整体项目健康状况看起来稳定但平静，今日没有报告 bug 或回归，也没有交付代码变更。

## 发布
过去 24 小时内没有发布新版本。没有需要报告的破坏性变更、迁移说明或版本专属更新。

## 项目进展
今日没有记录到已合并或已关闭的 PR，也没有已关闭的 issue，因此在给定时间窗口内没有功能推进或修复落地。唯一未关闭的 PR 与维护相关：[PR #8137](https://github.com/nearai/ironclaw/pull/8137) 将 `/docs/internal/architecture-video` 中的 `brace-expansion` 从 1.1.18 升级到 1.1.21。该 PR 仍待审查/合并，尚未产生已交付的变更。

## 社区热点
在提供的数据中，没有任何 issue 或 PR 获得评论或反应，因此今日没有高互动讨论串。

- [Issue #8136](https://github.com/nearai/ironclaw/issues/8136) — *提案：使用声明式 auth 和 egress 的可选 hosted-MCP Plazmi 扩展* — 0 条评论，0 👍。这是最具实质性的讨论项。底层需求是在不进行宿主级厂商耦合的情况下实现可扩展性：作者希望有一个可选的 Plazmi 伴随组件，作为扩展单独安装，并复用现有的 hosted-MCP 元数据、API-key auth 和 egress 声明式接口。
- [PR #8137](https://github.com/nearai/ironclaw/pull/8137) — *chore(deps): bump brace-expansion in /docs/internal/architecture-video* — 依赖维护活动，没有报告评论/反应。这里的需求是常规依赖卫生，而不是功能或 bug 讨论。

## Bug 与稳定性
在提供的 24 小时数据中，没有报告 bug、崩溃或回归。没有需要跟踪的 bug 修复 PR。[PR #8137](https://github.com/nearai/ironclaw/pull/8137) 是依赖升级，可能有助于稳定性/安全性卫生，但提供的数据未将其与特定已确认 bug 或安全公告关联。

## 功能请求与路线图信号
主要功能请求是 [Issue #8136](https://github.com/nearai/ironclaw/issues/8136)：一个使用声明式 auth 和 egress 的可选 hosted-MCP Plazmi 扩展。这表明有以下需求：
- 清晰的扩展边界，避免在 IronClaw 宿主中加入厂商逻辑。
- 可选的、单独安装的配套集成。
- 为 hosted-MCP 元数据、API-key auth 和 egress 复用声明式扩展接口。

预测：目前这是一项有 0 条评论且没有维护者回应的提案，因此除非能迅速被分诊并接受，否则不太可能在紧接的下一个版本中落地。如果被采纳，它更可能作为可选扩展出现，而不是核心宿主功能。

## 用户反馈摘要
今日唯一的直接用户/构建者反馈来自 [Issue #8136](https://github.com/nearai/ironclaw/issues/8136) 的作者。所陈述的痛点是倾向于不向宿主添加厂商特定逻辑。用例是一个单独安装的 Plazmi 伴随组件，利用现有的声明式 hosted-MCP、auth 和 egress 接缝。数据中没有明确的满意度或不满意度指标，但该提案是建设性的，并与可扩展性一致，而不是在报告缺陷。

## Backlog 观察
当前两个事项都是最近更新，等待维护者关注：
- [Issue #8136](https://github.com/nearai/ironclaw/issues/8136) — 未关闭的提案，创建/更新于 2026-10-13，0 条评论。需要维护者分诊或设计反馈。
- [PR #8137](https://github.com/nearai/ironclaw/pull/8137) — 未关闭的 Dependabot 依赖升级，等待审查/合并。

提供的数据集中没有更早的长期未回应 issue 或 PR，因此今日 backlog 风险仅限于这两个事项仍无人处理。

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw 项目摘要 —— 2026-10-14

## 1. 今日概览
QwenPaw 活跃度依旧很高：过去 24 小时内有 20 个 issue 更新（16 个开启/活跃，4 个已关闭），42 个 PR 更新（22 个开启，20 个已合并/关闭），无新版本发布。队列以维护为主，围绕 QwenPaw 2.2.x，尤其是控制台聊天可靠性、媒体/base64 历史记录膨胀、插件加载/来源、Windows 文件锁定和部署可配置性。维护者和贡献者正在关闭针对超时、媒体过滤、SQLite 持久化、控制台轮询和技能路径清理的修复，而诸如实验性 Code Mode 和插件热重载等大型功能 PR 仍处于开启状态。整体项目健康度看起来活跃但对稳定性敏感：用户正在提交详细的、源码级 bug 报告，且若干关键问题仍没有可见的修复 PR。

## 2. 发布
过去 24 小时无新版本发布。

## 3. 项目进展
可见样本中已关闭/合并的 PR 在多个方面取得进展：

- **[#8128](https://github.com/agentscope-ai/QwenPaw/pull/8128) — `feat(skill): Move hub to plugins`**：使 Hub 可通过插件系统安装/移除，并为自定义市场新增 `register_market_provider()`。
- **[#8217](https://github.com/agentscope-ai/QwenPaw/pull/8217) — `feat(console): establish UIUX foundation`**：新增路由元数据、页面外壳、推送轮询变更和懒加载导入诊断。
- **[#8190](https://github.com/agentscope-ai/QwenPaw/pull/8190) — `fix(agents): resolve model capabilities before filtering history media`**：防止纯文本模型从历史记录中接收图像。
- **[#8098](https://github.com/agentscope-ai/QwenPaw/pull/8098) — `fix(agents): return a result for foreground chat timeouts`**：修复 [#7981](https://github.com/agentscope-ai/QwenPaw/issues/7981) 中报告的前台超时行为。
- **[#7964](https://github.com/agentscope-ai/QwenPaw/pull/7964) — `fix(observability): Langfuse tool observation never records tool output`**：修复 Langfuse span 中工具输出缺失的问题。
- **[#8065](https://github.com/agentscope-ai/QwenPaw/pull/8065) — `fix(skills): sanitize skill_name before building staging paths`**：加固技能暂存路径，防范路径遍历。
- **[#8212](https://github.com/agentscope-ai/QwenPaw/pull/8212) — `fix: finish tool streams without yielding during GeneratorExit`**：解决异步生成器终结错误。
- **[#6459](https://github.com/agentscope-ai/QwenPaw/pull/6459) — `fix(history): harden SQLite persistence, backup, and restore`**：改进历史写穿和 SQLite 锁定行为。
- **[#8221](https://github.com/agentscope-ai/QwenPaw/pull/8221) — `fix(console): bound background polling and unblock model picker`**：限制轮询累积并解除模型选择器阻塞。
- **[#8220](https://github.com/agentscope-ai/QwenPaw/pull/8220) — `chore(deps): bumping version of agentscope to 2.0.10`**：依赖更新。

今日关闭的问题包括 **[#8015](https://github.com/agentscope-ai/QwenPaw/issues/8015)**（自定义 Skill/Plugin 市场来源）、**[#8088](https://github.com/agentscope-ai/QwenPaw/issues/8088)**（图像路由在 Bash+PIL 循环中挂起）、**[#7981](https://github.com/agentscope-ai/QwenPaw/issues/7981)**（前台超时处理），以及 **[#8186](https://github.com/agentscope-ai/QwenPaw/issues/8186)**（消息导航跳转覆盖手动滚动）。

## 4. 社区热点话题
快照中讨论最多的问题集中在可靠性、可扩展性和部署方面：

- **[#8071](https://github.com/agentscope-ai/QwenPaw/issues/8071) — 面向插件的主题扩展点**（3 条评论，开启）：插件作者希望有语义化 token 覆盖，而不仅仅是 `colorPrimary`。这表明对更深层插件 UI API 的需求。
- **[#8172](https://github.com/agentscope-ai/QwenPaw/issues/8172) — 控制台聊天间歇性静默完成且结果为空**（3 条评论，开启）：一个严重影响信任的 bug：一轮对话约 1 秒结束，输出为空，且模型从未被调用。
- **[#8015](https://github.com/agentscope-ai/QwenPaw/issues/8015) — 可配置的自定义 Skill/Plugin 市场来源**（2 条评论，已关闭）：企业/气隙用户需要自托管或离线市场镜像。
- **[#7056](https://github.com/agentscope-ai/QwenPaw/issues/7056) — 后台任务回调 / 通知机制**（2 条评论，自 8 月起开启）：用户希望基于推送的任务完成通知，而不是轮询 `check_agent_task`。
- **[#8171](https://github.com/agentscope-ai/QwenPaw/issues/8171) — `send_file_to_user` 音频文件永久卡死会话**（2 条评论，开启）：LM Studio 对音频返回 400，而能力分类器未能恢复。
- **[#8206](https://github.com/agentscope-ai/QwenPaw/issues/8206) — 一旦 `creator.log` 打开，Windows 项目删除总是失败**（2 条评论，开启）：Windows 文件锁定会破坏 `qwenpaw-creator` 项目删除。
- **[#8126](https://github.com/agentscope-ai/QwenPaw/issues/8126) — 技能池下载应可取消并显示进度**（2 条评论，开启）：大型技能下载仍会触发 30 秒客户端超时。

值得关注的活跃 PR 包括 **[#8224](https://github.com/agentscope-ai/QwenPaw/pull/8224)**（实验性 Python Code Mode）、**[#7565](https://github.com/agentscope-ai/QwenPaw/pull/7565)**（干净的插件卸载与可回滚安全热重载）、**[#8231](https://github.com/agentscope-ai/QwenPaw/pull/8231)**（面向“全部更新”的市场安装来源），以及 **[#8229](https://github.com/agentscope-ai/QwenPaw/pull/8229)**（截断聊天历史中过大的工具输出）。

底层需求：用户需要可靠的长时间运行会话、一流的插件可扩展性、离线/企业部署支持、更好的异步任务用户体验，以及跨提供商的稳健媒体处理。

## 5. Bug 与稳定性
按报告影响排序的严重程度：

1. **[#8172](https://github.com/agentscope-ai/QwenPaw/issues/8172) — 控制台聊天静默完成且结果为空**（开启）：高严重性；破坏用户对控制台聊天的信任。快照中未显示修复 PR。
2. **[#8171](https://github.com/agentscope-ai/QwenPaw/issues/8171) — 音频文件永久卡死会话**（开启）：高严重性；LM Studio 400 错误未被回退/分类器处理。相关 **[#7654](https://github.com/agentscope-ai/QwenPaw/pull/7654)** 仍处于开启状态。
3. **[#8228](https://github.com/agentscope-ai/QwenPaw/issues/8228) — 来自 base64 工具输出的聊天历史膨胀**（开启）：高严重性；10 MB+ 历史记录会触发 30 秒控制台超时。修复 PR **[#8229](https://github.com/agentscope-ai/QwenPaw/pull/8229)** 已开启。
4. **[#8206](https://github.com/agentscope-ai/QwenPaw/issues/8206) — Windows 项目删除失败并报 WinError 5**（开启）：对 Windows 用户而言为高；打开的 `creator.log` 会阻塞目录重命名/删除。
5. **[#8225](https://github.com/agentscope-ai/QwenPaw/issues/8225) — 配置保存热重载会静默取消正在运行的 cron 任务**（开启）：高；后台任务被标记为 `cancelled`，且没有任何警告。
6. **[#8222](https://github.com/agentscope-ai/QwenPaw/issues/8222) — `qwenpaw acp` 从不加载已安装插件**（开启）：对 ACP 用户而言为高；插件工具缺失，跨进程回调无法唤醒会话。
7. **[#8213](https://github.com/agentscope-ai/QwenPaw/issues/8213) — Hub 重启后本应运行的 Local 运行时仍处于停止状态**（开启）：高；cron 任务静默错过计划执行。
8. **[#8179](https://github.com/agentscope-ai/QwenPaw/issues/8179) — 从 GitHub 安装技能会损坏二进制文件**（开启）：高；非 UTF-8 字节被替换为 U+FFFD，导致图像/字体损坏。
9. **[#8218](https://github.com/agentscope-ai/QwenPaw/issues/8218) — 默认 agent 无法修改模型；出现已弃用模型错误**（开启）：对配置可用性而言为高。
10. **[#8177](https://github.com/agentscope-ai/QwenPaw/issues/8177) — 控制台 basename 硬编码为 `/console`**（开启）：中；阻塞反向代理子路径部署。

最近关闭的稳定性修复包括 **[#8088](https://github.com/agentscope-ai/QwenPaw/issues/8088)**、**[#7981](https://github.com/agentscope-ai/QwenPaw/issues/7981)** 和 **[#8186](https://github.com/agentscope-ai/QwenPaw/issues/8186)**。

## 6. 功能请求与路线图信号
近期请求中可能出现的路线图信号：

- **[#8071](https://github.com/agentscope-ai/QwenPaw/issues/8071)** — 面向插件的主题扩展点，支持语义化 token 覆盖。
- **[#8015](https://github.com/agentscope-ai/QwenPaw/issues/8015)** / **[#8128](https://github.com/agentscope-ai/QwenPaw/pull/8128)** — 可自定义的 Skill/Plugin 市场提供商，包括自托管和离线镜像。
- **[#7056](https://github.com/agentscope-ai/QwenPaw/issues/7056)** — 面向异步 agent 工作流的后台任务回调/通知机制。
- **[#8126](https://github.com/agentscope-ai/QwenPaw/issues/8126)** — 可取消且带进度报告的技能池下载。
- **[#8223](https://github.com/agentscope-ai/QwenPaw/issues/8223)** / **[#8230](https://github.com/agentscope-ai/QwenPaw/pull/8230)** — 可选的宿主导航策略，由侧边栏和设置共享。
- **[#8211](https://github.com/agentscope-ai/QwenPaw/issues/8211)** — 受保护执行契约提示的 i18n。
- **[#8177](https://github.com/agentscope-ai/QwenPaw/issues/8177)** — 可配置的控制台 basename，用于反向代理子路径部署。
- **[#8224](https://github.com/agentscope-ai/QwenPaw/pull/8224)** — 用于工具编排的实验性 Python Code Mode。
- **[#8205](https://github.com/agentscope-ai/QwenPaw/pull/8205)** — TypeSafe provider 和决策模型类型。

预测：下一个 QwenPaw 版本可能聚焦于插件/市场可扩展性、控制台可靠性与用户体验、媒体/历史处理，并可能包含实验性 Code Mode 路径，而不是一次大版本跃升。

## 7. 用户反馈摘要
用户正在 Docker/NAS、Windows Server、桌面和 ACP 环境中积极测试 2.2.0、2.2.1 和 2.2.2 beta 构建。最强烈的不满集中在静默失败上：控制台空完成、音频发送后会话卡死、无 cron 取消警告，以及超时后无回复。用户还报告了数据完整性问题，例如聊天历史中的 base64 膨胀，以及 GitHub 技能安装期间的二进制损坏。在反向代理子路径请求、气隙市场需求和 Windows 文件锁定失败中，部署摩擦明显可见。积极的一面是，用户正在提供详细的源码级报告，维护者也在关闭针对超时、媒体过滤、SQLite 历史持久化和控制台轮询的具体修复。整体情绪：社区参与度高，但可靠性和部署打磨仍是主要的挫败感来源。

## 8. 待办观察
需要维护者关注的重要未关闭事项：

- **[#7056](https://github.com/agentscope-ai/QwenPaw/issues/7056) — 后台任务回调 / 通知机制**（自 2026-08-15 起开启）：长期存在的异步工作流请求，仅有 2 条评论。
- **[#7565](https://github.com/agentscope-ai/QwenPaw/pull/7565) — `feat(plugins): add clean unload and rollback-safe hot reload`**（自 2026-09-04 起开启，size/XXXL）：对插件生态稳定性和更新安全性至关重要。
- **[#8071](https://github.com/agentscope-ai/QwenPaw/issues/8071) — 面向插件的主题扩展点**（自 2026-10-01 起开启）：插件 API 成熟度。
- **[#8126](https://github.com/agentscope-ai/QwenPaw/issues/8126) — 可取消的技能池下载**（自 2026-10-08 起开启）：用户体验与超时缓解。
- **[#8179](https://github.com/agentscope-ai/QwenPaw/issues/8179) — GitHub 技能安装损坏二进制文件**（自 2026-10-11 起开启）：数据完整性问题。
- **[#8177](https://github.com/agentscope-ai/QwenPaw/issues/8177) — 可配置的控制台 basename**（自 2026-10-11 起开启）：常见的自托管阻塞点。
- **[#8222](https://github.com/agentscope-ai/QwenPaw/issues/8222) — ACP 不加载已安装插件**（自 2026-10-13 起开启）：插件/ACP 集成缺口。
- **[#8213](https://github.com/agentscope-ai/QwenPaw/issues/8213) — Hub 重启后不恢复 Local 运行时**（自 2026-10-13 起开启）：后台可靠性。
- **[#8225](https://github.com/agentscope-ai/QwenPaw/issues/8225) — 热重载取消 cron 任务**（自 2026-10-13 起开启）：静默任务丢失。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目摘要 — 2026-10-14

## 1. 今日概览

ZeroClaw 在过去 24 小时内活动频繁：更新了 24 个 issue，全部仍处于开启状态；更新了 50 个 PR，其中 45 个开启、5 个已合并/关闭。没有发布新版本。今天 issue 队列没有缩小，因为没有 issue 被关闭；PR 流水线仍然活跃，但积压严重且受审查门控。最强的信号集中在 v0.8.6 的发布加固、v0.9.0 的 gateway/runtime 工作，以及一组影响成本核算、会话持久化、提示缓存和频道可靠性的 P1/P2 bug。总体而言：活跃且贡献者众多，但存在明显的稳定性和发布就绪压力。

## 2. 版本发布

过去 24 小时内没有发布新版本。最新发布数据为空，因此没有发布说明、破坏性变更或迁移说明可报告。

## 3. 项目进展

- PR 活动仍是主要推进力：更新了 50 个 PR，45 个仍开启，5 个已合并/关闭。
- 提供的 top-20 PR 样本显示有一个已关闭 PR：[#11435](https://github.com/zeroclaw-labs/zeroclaw/pull/11435) `fix(sop): settle rejected headless runs and retain reload retries`，它处理 SOP 无头运行生命周期和重载行为。
- 若干开启的 PR 正在推进 v0.8.6 / 发布门控工作：
  - [#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347) `feat(release): ship the WASM plugin host in supported release artifacts`
  - [#11304](https://github.com/zeroclaw-labs/zeroclaw/pull/11304) `fix(plugins): record socket and WebSocket egress denials`
  - [#11309](https://github.com/zeroclaw-labs/zeroclaw/pull/11309) `feat(quickstart): install and activate tool plugins from zeroclaw quickstart`
  - [#11228](https://github.com/zeroclaw-labs/zeroclaw/pull/11228) `fix(plugins): harden named TLS profile materialization`
- Runtime 和提供商正确性工作也在进行：
  - [#11676](https://github.com/zeroclaw-labs/zeroclaw/pull/11676) 修复 SQLite 会话记录 `created_at` 保留问题。
  - [#11672](https://github.com/zeroclaw-labs/zeroclaw/pull/11672) 修复 JPEG 准入复制问题。
  - [#11647](https://github.com/zeroclaw-labs/zeroclaw/pull/11647) 将视觉支持与系统合并解耦。
  - [#11651](https://github.com/zeroclaw-labs/zeroclaw/pull/11651) 使用共享洪泛门处理 Telegram 429 `retry_after`。
  - [#11577](https://github.com/zeroclaw-labs/zeroclaw/pull/11577) 为子代理添加操作员声明的模型路由。
- 今天没有 issue 被关闭，因此在报告窗口内 open issue 积压没有减少。

## 4. 社区热点

最活跃的讨论由评论驱动；在提供的数据中，所有列出的 issue 均显示 0 个 👍 反应。

1. [#11420](https://github.com/zeroclaw-labs/zeroclaw/issues/11420) — 8 条评论  
   `SQLite session backend rewrites created_at of every message on each turn`  
   这是一个 P1 数据完整性问题。用户需要可靠的逐消息时间戳，用于会话审计、重放和可观测性。已有修复 PR：[#11676](https://github.com/zeroclaw-labs/zeroclaw/pull/11676)。

2. [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) — 6 条评论  
   `[Tracker]: Runtime and gateway delivery - v0.8.6 and v0.9.0`  
   这是 runtime 第二阶段和 gateway 分离第三阶段的路线图事实来源。它表明维护者正在协调一次跨多个版本的架构推进。

3. [#11204](https://github.com/zeroclaw-labs/zeroclaw/issues/11204) — 5 条评论  
   `OpenRouter spend shows $0.00 and all tokens classified "free tok"`  
   OpenRouter 用户的成本遥测不可信。这是一个 P1 可观测性/runtime 问题，并且很可能是成本敏感部署的发布阻塞项。

4. [#10573](https://github.com/zeroclaw-labs/zeroclaw/issues/10573) — 5 条评论  
   `Bind gateway pairing tokens to roster users for scoped remote principals`  
   已被接受的安全/身份功能。它反映了对范围化远程访问和更严格 gateway 主体控制的需求。

5. [#11613](https://github.com/zeroclaw-labs/zeroclaw/issues/11613) — 4 条评论  
   `Cost ledger drops the provider's total_tokens`  
   与隐藏推理 token 和提供商使用量核算相关。用户需要保留提供商报告的总量，而不是被错误地重新计算。

底层需求：可信的成本/计费数据、持久的会话历史、gateway 身份/安全，以及可预测的 runtime/gateway 发布排期。

## 5. Bug 与稳定性

根据提供的 issue/PR 数据，按严重性和影响排序。

### S1 / 阻塞

- [#11179](https://github.com/zeroclaw-labs/zeroclaw/issues/11179) — `Flaky: sop::engine pending_park_retry_respects_pending_pool_cap fails when the test crosses a second boundary`  
  S1，工作流阻塞。这个 flaky 测试会让不相关的 PR 的 `CI Required Gate` 变红。在 top PR 样本中没有可见的修复 PR。

### P1 / 高严重性

- [#11420](https://github.com/zeroclaw-labs/zeroclaw/issues/11420) — SQLite 会话后端在每轮重写 `created_at`。修复 PR：[#11676](https://github.com/zeroclaw-labs/zeroclaw/pull/11676)。
- [#11204](https://github.com/zeroclaw-labs/zeroclaw/issues/11204) — OpenRouter 消费显示 $0.00，且所有 token 都被视为免费。没有可见的修复 PR。
- [#10777](https://github.com/zeroclaw-labs/zeroclaw/issues/10777) — Anthropic thinking/effort 配置在轮次之间翻转，并重写缓存历史。状态：已接受，需要复现。没有可见的修复 PR。
- [#11606](https://github.com/zeroclaw-labs/zeroclaw/issues/11606) — `model_routing_config upsert_agent` 重写整个配置，并且可能伪造/丢弃字段。状态：进行中。样本中没有可见的修复 PR。

### S2 / 行为降级

- [#11696](https://github.com/zeroclaw-labs/zeroclaw/issues/11696) — 系统提示每轮都从磁盘重建，破坏活跃会话的提示缓存。成本/性能回归。
- [#11711](https://github.com/zeroclaw-labs/zeroclaw/issues/11711) — 修剪长对话会从会话存储中永久删除较早的轮次。数据丢失风险。
- [#11706](https://github.com/zeroclaw-labs/zeroclaw/issues/11706) — `max_image_turns` 从不释放视觉路由，将提供商固定在 `vision_model_provider`。
- [#11708](https://github.com/zeroclaw-labs/zeroclaw/issues/11708) — `model_routing_config` 在其工具 schema 中省略了动作特定的必需参数。状态：进行中。
- [#11613](https://github.com/zeroclaw-labs/zeroclaw/issues/11613) — 成本账本丢弃提供商的 `total_tokens`，导致推理 token 计数偏低。
- [#10950](https://github.com/zeroclaw-labs/zeroclaw/issues/10950) — `cost.warn_at_percent` 警告被 runtime 忽略。
- [#11713](https://github.com/zeroclaw-labs/zeroclaw/issues/11713) — Telegram 频道忽略 `edited_message` 更新，因此提及编辑永远不会被看到。
- [#11709](https://github.com/zeroclaw-labs/zeroclaw/issues/11709) — 升级后的 WebSocket 在 gateway 重载后仍然存活，并继续为已退役实例提供服务。相关 PR [#11718](https://github.com/zeroclaw-labs/zeroclaw/pull/11718) 处理了流式 webhook 取消，但没有解决完整的已退役实例 socket 问题。

### S3 / 轻微或令人困惑

- [#11694](https://github.com/zeroclaw-labs/zeroclaw/issues/11694) — ZeroCode 成本标签页混合 UTC 和本地日总计，且没有标注。
- [#11654](https://github.com/zeroclaw-labs/zeroclaw/issues/11654) — JPEG 准入在检查解码内存上限之前复制输入并解析头部元数据。修复 PR：[#11672](https://github.com/zeroclaw-labs/zeroclaw/pull/11672)。
- [#11675](https://github.com/zeroclaw-labs/zeroclaw/issues/11675) — ACP 会话存储重新标记 `acp_messages` 和 `acp_tool_calls` 行的 `created_at`。
- [#11710](https://github.com/zeroclaw-labs/zeroclaw/issues/11710) — 被截断的工作区文件告诉 agent 读取它，而提示头又禁止读取它。
- [#11703](https://github.com/zeroclaw-labs/zeroclaw/issues/11703) — 延迟工具激活会改变系统提示并破坏提示缓存。
- [#11704](https://github.com/zeroclaw-labs/zeroclaw/issues/11704) — 频道和 `agent::run` 提示携带日期，导致午夜提示缓存未命中。

## 6. 功能请求与路线图信号

- [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) 仍然是 v0.8.6 runtime 第二阶段和 v0.9.0 gateway 分离的主要路线图跟踪项。预计会继续围绕 runtime、gateway 和安全交付物进行发布门控工作。
- [#10573](https://github.com/zeroclaw-labs/zeroclaw/issues/10573) — 将 gateway 配对 token 绑定到 roster 用户已被接受，并且很可能针对 v0.8.6/v0.9.0 的安全身份工作。
- [#11207](https://github.com/zeroclaw-labs/zeroclaw/issues/11207) — 请求记录一个受限的 Tsubasa 自定义提供商设置。低风险文档请求。
- [#10261](https://github.com/zeroclaw-labs/zeroclaw/issues/10261) — 使文件系统分发循环投递回归测试成为关键测试。小型测试/质量任务。
- [#11705](https://github.com/zeroclaw-labs/zeroclaw/issues/11705) — 提议一个 Plazmi 示例，演示按 agent 的 MCP 授权和受限追赶。

可能影响下一版本的 PR：
- v0.8.6 插件/发布工作：[#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347), [#11304](https://github.com/zeroclaw-labs/zeroclaw/pull/11304), [#11309](https://github.com/zeroclaw-labs/zeroclaw/pull/11309), [#11228](https://github.com/zeroclaw-labs/zeroclaw/pull/11228)。
- Runtime/模型路由：[#11577](https://github.com/zeroclaw-labs/zeroclaw/pull/11577), [#11174](https://github.com/zeroclaw-labs/zeroclaw/pull/11174)。
- 配置/UX：[#11419](https://github.com/zeroclaw-labs/zeroclaw/pull/11419), [#11719](https://github.com/zeroclaw-labs/zeroclaw/pull/11719), [#11721](https://github.com/zeroclaw-labs/zeroclaw/pull/11721), [#11717](https://github.com/zeroclaw-labs/zeroclaw/pull/11717)。

预测：下一个可见版本最可能是 v0.8.6，重点包括 WASM 插件宿主产物、插件安全、quickstart 插件激活和 runtime 清理。v0.9.0 很可能随后跟进，带来 gateway 分离、范围化配对 token，以及更广泛的 runtime/gateway 架构变更。

## 7. 用户反馈摘要

用户的主要痛点集中在数据保真度和成本透明度：

- 成本跟踪被反复报告为不可靠：OpenRouter 消费显示 $0.00，`total_tokens` 被丢弃，`cost.warn_at_percent` 被忽略，ZeroCode 混合 UTC/本地日总计。
- 会话持久化是主要担忧：SQLite 和 ACP 记录重写会破坏 `created_at`，修剪会删除较早轮次，并且逐消息时间线数据会丢失。
- 提示缓存失效正在造成不必要的成本：系统提示从磁盘重建，延迟工具改变提示，提示中的日期导致午夜缓存未命中，并且 Anthropic thinking 配置可能重写缓存历史。
- 配置工具可能具有破坏性：`model_routing_config` 的 upsert 操作重写了整个配置，并从 schema 中省略了必需参数。
- 频道可靠性问题仍然存在：Telegram 忽略已编辑消息，并错误处理 429 速率限制；gateway 重载后旧 socket 仍在为已退役实例提供服务。
- 用户要求更多受限且有文档的设置，尤其是自定义提供商、MCP 授权、本地 Whisper 模型和插件安装流程。

满意度信号：许多报告详细、可复现，并且已被接受或标记为进行中，表明有一个积极参与的高级用户群体。不满主要集中在可观测性、持久化和成本数据无法信任的地方。

## 8. 积压事项观察

长期存在或高重要性、需要维护者关注的条目：

- [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) — 创建于 2026-06-09。v0.8.6 和 v0.9.0 的 runtime/gateway 跟踪项。高风险路线图协调事项。
- [#10573](https://github.com/zeroclaw-labs/zeroclaw/issues/10573) — 创建于 2026-09-03。已接受的安全/身份功能，用于范围化 gateway 配对 token。
- [#11204](https://github.com/zeroclaw-labs/zeroclaw/issues/11204) — 创建于 2026-09-27。P1 OpenRouter 成本核算 bug，没有可见的修复 PR。
- [#10777](https://github.com/zeroclaw-labs/zeroclaw/issues/10777) — 创建于 2026-09-11。P1 Anthropic thinking/cache 问题，已接受但需要复现。
- [#11420](https://github.com/zeroclaw-labs/zeroclaw/issues/11420) — 创建于 2026-10-02。P1 会话时间戳损坏；修复 PR [#11676](https://github.com/zeroclaw-labs/zeroclaw/pull/11676) 已存在，应优先处理。
- [#11179](https://github.com/zeroclaw-labs/zeroclaw/issues/11179) — 创建于 2026-09-26。S1 不稳定 CI 测试，阻塞 PR 门控。
- [#10261](https://github.com/zeroclaw-labs/zeroclaw/issues/10261) — 创建于 2026-08-22。小型但重要的回归测试质量任务。
- PR 积压：[#11347](https://github.com/zeroclaw-labs/zeroclaw/pull/11347) 堆叠且受发布门控；[#11304](https://github.com/zeroclaw-labs/zeroclaw/pull/11304) 被阻塞；[#11174](https://github.com/zeroclaw-labs/zeroclaw/pull/11174) 需要作者行动；[#11228](https://github.com/zeroclaw-labs/zeroclaw/pull/11228) 是高风险安全加固。需要维护者安排审查顺序，以保持 v0.8.6 按计划推进。

---

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*