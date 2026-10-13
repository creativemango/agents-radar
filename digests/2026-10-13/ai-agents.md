# OpenClaw 生态日报 2026-10-13

> Issues: 500 | PRs: 500 | 覆盖项目: 5 个 | 生成时间: 2026-10-13 03:08 UTC

- [OpenClaw](https://github.com/openclaw/openclaw)
- [Hermes Agent](https://github.com/nousresearch/hermes-agent)
- [IronClaw](https://github.com/nearai/ironclaw)
- [QwenPaw](https://github.com/agentscope-ai/QwenPaw)
- [ZeroClaw](https://github.com/zeroclaw-labs/zeroclaw)

---

## OpenClaw 项目深度报告

# OpenClaw 项目摘要 — 2026-10-13

## 1. 今日概览
OpenClaw 仍是一个活跃度极高的项目：**过去 24 小时内更新了 500 个 issue 和 500 个 PR**，其中**335 个开放/活跃 issue**、**314 个开放 PR**。吞吐量强劲——同一时间窗口内**关闭了 165 个 issue**、**合并/关闭了 186 个 PR**。发布了两个版本，均重点聚焦更新恢复、交接可靠性以及 Windows/macOS 安装路径。健康状况喜忧参半：维护者正在发布恢复修复并关闭高严重级别的会话/崩溃问题，但仍有多个 **P0 级更新、Windows 服务、内存泄漏和磁盘增长问题处于开放状态**。总体而言：活跃且响应及时，但更新/安装可靠性是主要风险领域。

## 2. 发布
| 版本 | 类型 | 关键变更 | 破坏性变更 / 迁移说明 |
|---|---|---|---|
| [v2026.10.5-beta.2](https://github.com/openclaw/openclaw/releases) | Beta | `openclaw update` 会处理陈旧的 handoff 租约、已完成的回执以及旧版 2026.9.8 恢复记录，而不是卡在 `update-recovery-pending`；保留服务堆限制和 webhook 监听器；处理 Windows short-* 路径（发布说明已截断）。 | 提供的说明中无明确破坏性变更。迁移重点：停滞/旧版更新恢复。 |
| [v2026.10.2](https://github.com/openclaw/openclaw/releases) | 稳定渠道升级 | 从稳定版 2026.10.1 进行的首次升级现在可顺利完成；成功的原地更新不再以错误结束；Windows、克隆文件系统和私有 Node 更新路径能够完成，而不是停滞或回滚（#168849、#169031）。 | 提供的说明中无明确破坏性变更。迁移重点：2026.10.1 升级和原地更新恢复。 |

## 3. 项目进展
- 过去 24 小时内**合并/关闭了 186 个 PR**，**关闭了 165 个 issue**。
- 在头部活跃切片中可见的已关闭 PR：
  - [#170497](https://github.com/openclaw/openclaw/pull/170497) — 恢复原始 Control UI 启动性能预算。
  - [#170533](https://github.com/openclaw/openclaw/pull/170533) — 合并 chat 与 Solid 展示路径。
- 已关闭 issue 显示在崩溃、会话、更新和模型正确性领域取得进展：
  - [#137729](https://github.com/openclaw/openclaw/issues/137729) — 未防护的 `.trim()` 崩溃修复模式。
  - [#157126](https://github.com/openclaw/openclaw/issues/157126) — claude-cli MCP 桥接请求作用域问题。
  - [#167506](https://github.com/openclaw/openclaw/issues/167506) — 已删除的临时 handoff DB 阻塞更新恢复。
  - [#69118](https://github.com/openclaw/openclaw/issues/69118) — Claude CLI 群组频道会话重置。
  - [#158936](https://github.com/openclaw/openclaw/issues/158936) — macOS 就绪状态看门狗重启循环。
  - [#168750](https://github.com/openclaw/openclaw/issues/168750) — 更新激活 Doctor 失败。
  - [#127239](https://github.com/openclaw/openclaw/issues/127239) — 上下文窗口回退到硬编码 200k。
  - [#9986](https://github.com/openclaw/openclaw/issues/9986) — 上下文长度超限时的模型回退。
  - [#119692](https://github.com/openclaw/openclaw/issues/119692) — MiniMax-M3 用量核算丢失。
  - [#95746](https://github.com/openclaw/openclaw/issues/95746) — memory-core dreaming 上下文耗尽。
  - [#39811](https://github.com/openclaw/openclaw/issues/39811) — 未验证的模型名称。
  - [#164214](https://github.com/openclaw/openclaw/issues/164214) — 包发布恢复卡在 `publishing`。
- 推进主要主题的开放 PR：更新恢复、Doctor 修复、Gateway 响应性、CLI 请求作用域隔离、密钥输入安全、Control UI Solid 迁移、插件运行时控制、memory-wiki 状态以及会话/记录权威。

## 4. 社区热门话题
### 最活跃 Issue
| 排名 | Issue | 评论数 | 底层需求 |
|---:|---|---:|---|
| 1 | [#159356](https://github.com/openclaw/openclaw/issues/159356) — llama.cpp manager 报告就绪，但 embedding 子进程退出；HTTP 500 | 26 | 面向 embedding 栈的子进程健康检查和内存压力诊断。 |
| 2 | [#159662](https://github.com/openclaw/openclaw/issues/159662) — `prepared-model-catalog.worker.js` 无界内存泄漏，~4–5 GB/h | 22 | 与 provider/plugin 配置无关的紧急内存泄漏修复。 |
| 3 | [#160959](https://github.com/openclaw/openclaw/issues/160959) — Gateway 在捕获大型外部插件时阻塞数分钟 | 20 | 非阻塞插件捕获 / 事件循环饥饿修复。 |
| 4 | [#50093](https://github.com/openclaw/openclaw/issues/50093) — WhatsApp 重连后回填遗漏消息 | 20 | 频道重连恢复和消息丢失预防。 |
| 5 | [#97616](https://github.com/openclaw/openclaw/issues/97616) — 未回收的 hook/tool 子进程与僵尸进程累积 | 18 | 进程回收和长期运行运行时退化修复。 |
| 6 | [#78308](https://github.com/openclaw/openclaw/issues/78308) — MCP 工具调用经频道中介的批准 / 同意信封 | 18 | 针对 MCP 副作用的安全与同意门控。 |
| 7 | [#137729](https://github.com/openclaw/openclaw/issues/137729) — transcript 重放/错误分类中未防护的 `.trim()` 崩溃 | 14 | 防御性编码；已关闭。 |
| 8 | [#157126](https://github.com/openclaw/openclaw/issues/157126) — claude-cli MCP 桥接继承首个请求作用域 | 14 | 正确的请求作用域隔离；已关闭，并有相关 PR 工作。 |
| 9 | [#156112](https://github.com/openclaw/openclaw/issues/156112) — `openclaw update` 在全局安装切换时失败 | 14 | 直接 `npm install -g` 成功时更新器的可靠性。 |
| 10 | [#114414](https://github.com/openclaw/openclaw/issues/114414) — 带日期 TODO 清理 | 13 | 可维护性/技术债清理。 |
| 11 | [#146860](https://github.com/openclaw/openclaw/issues/146860) — Windows 托管更新交接无法获取进程启动身份 | 12 | Windows 计划任务 / InteractiveToken 更新可靠性。 |
| 12 | [#101929](https://github.com/openclaw/openclaw/issues/101929) — 上下文溢出预检查多计 2.3–2.6 倍 | 12 | 准确 token 核算，避免误触发截断恢复。 |

### 热门 PR
快照中未提供 PR 评论数；以下是头部活跃切片中的高信号 PR。
- [#170534](https://github.com/openclaw/openclaw/pull/170534) — 修复静默轮次结束，避免编造答案；XL、P2、合并风险。
- [#169299](https://github.com/openclaw/openclaw/pull/169299) — Doctor 优先处理未解决发现并解释修复。
- [#166782](https://github.com/openclaw/openclaw/pull/166782) — 在带回复标记的流期间保持 Gateway 响应。
- [#157144](https://github.com/openclaw/openclaw/pull/157144) — CLI 桥接工具调用位于各自的运行请求作用域；已通过 #165432 并入 main。
- [#167250](https://github.com/openclaw/openclaw/pull/167250) — 防止畸形 UTF-8 覆盖已存储的密钥。
- [#168368](https://github.com/openclaw/openclaw/pull/168368) — Mattermost 代理按钮渲染为纯文本。
- [#170527](https://github.com/openclaw/openclaw/pull/170527) — 为 provider 包装的错误触发模型回退。
- [#170548](https://github.com/openclaw/openclaw/pull/170548) — 防止手动 cron 运行丢失工具访问权限。
- [#169172](https://github.com/openclaw/openclaw/pull/169172) — 当分块拆分标记时，工具调用文本不再泄漏到聊天输出。
- [#170553](https://github.com/openclaw/openclaw/pull/170553) — 防止大型 vault 上 memory-wiki 状态超时。
- [#169848](https://github.com/openclaw/openclaw/pull/169848) — CLI 后端子代理忽略 `lightContext`。
- [#170152](https://github.com/openclaw/openclaw/pull/170152) — 允许插件引导会话的活动运行。
- [#170555](https://github.com/openclaw/openclaw/pull/170555) — 在 Gateway 关闭期间加入生命周期发布。
- [#170536](https://github.com/openclaw/openclaw/pull/170536) — 停止在会话中重新检查合格目标缺失。
- [#169692](https://github.com/openclaw/openclaw/pull/169692) — 使用 Solid 渲染 New Session。
- [#170387](https://github.com/openclaw/openclaw/pull/170387) — 将共享选择器、对话框和 toast 移植到 Solid 2。

## 5. Bug 与稳定性
按所提供的严重程度排序。“修复 PR”反映快照中可见的标签/关联 PR。

### P0 / 严重
| Issue | Problem | Fix PR Status |
|---|---|---|
| [#159662](https://github.com/openclaw/openclaw/issues/159662) | `prepared-model-catalog.worker.js` 中的无界内存泄漏，~4–5 GB/h | 无可见修复 PR。 |
| [#160959](https://github.com/openclaw/openclaw/issues/160959) | Gateway 在捕获大型外部插件时阻塞数分钟（2026.9.6 回归） | 无新修复 PR。 |
| [#156112](https://github.com/openclaw/openclaw/issues/156112) | `openclaw update` 在全局安装切换时失败，而直接 npm 安装成功 | 仅手动；无可见修复 PR。 |
| [#146860](https://github.com/openclaw/openclaw/issues/146860) | Windows 托管更新交接在使用 `InteractiveToken` 计划任务时停滞 | 无可见修复 PR。 |
| [#158390](https://github.com/openclaw/openclaw/issues/158390) | 插件捕获临时目录从不被 GC；磁盘无限填满 | 无新修复 PR。 |
| [#167652](https://github.com/openclaw/openclaw/issues/167652) | Windows Gateway 在升级到 2026.9.9 后挂起，尽管 Doctor 已验证重启 | 需要现场复现；无修复。 |
| [#160060](https://github.com/openclaw/openclaw/issues/160060) | Windows Gateway 在待机唤醒后 ~54 分钟静默死亡 | 无修复。 |
| [#166894](https://github.com/openclaw/openclaw/issues/166894) | 在 Windows 多代理安装中，Gateway 重启挂起 30+ 分钟至 2 小时 | 无修复。 |
| [#138560](https://github.com/openclaw/openclaw/issues/138560) | Windows Control UI 更新因 `managed-service-handoff-failed` 中止 | 无修复。 |
| [#153426](https://github.com/openclaw/openclaw/issues/153426) | `MEMORY.md` / `USER.md` 被溯源棘轮永久排除 | 无修复。 |

### P1 / 高
| Issue | Problem | Fix PR Status |
|---|---|---|
| [#97616](https://github.com/openclaw/openclaw/issues/97616) | 未回收的 hook/tool 子进程导致僵尸进程累积 | 无修复。 |
| [#101929](https://github.com/openclaw/openclaw/issues/101929) | 上下文溢出预检查相较于计费用量多计 ~2.3–2.6 倍 | 无修复。 |
| [#168955](https://github.com/openclaw/openclaw/issues/168955) | 2026.10.1 中的启动回归：在 Windows 上就绪 ~1030s vs ~410s | 无修复。 |
| [#154891](https://github.com/openclaw/openclaw/issues/154891) | 失败/回滚的配置热重载会使无关插件变砖，直到重启 | 无修复。 |
| [#132303](https://github.com/openclaw/openclaw/issues/132303) | `agents.list[].tools.deny` 未对 claude-cli 后端强制执行 | 安全；无修复。 |
| [#126950](https://github.com/openclaw/openclaw/issues/126950) | Telegram 不透明回调负载被静默丢弃 | 无修复。 |
| [#128916](https://github.com/openclaw/openclaw/issues/128916) | 仅使用消息工具且出错的运行静默结束 | 无修复。 |
| [#156353](https://github.com/openclaw/openclaw/issues/156353) | 插件重载后因 transcript 准入身份不匹配导致聊天失败 | 无修复。 |
| [#77249](https://github.com/openclaw/openclaw/issues/77249) | Slack 重连监控器在僵尸 WSS 上挂起 | 无修复。 |
| [#125826](https://github.com/openclaw/openclaw/issues/125826) | Discord 语音回复被重复朗读和重复写入 | 无修复。 |

### 新增 / 最近更新
- [#170331](https://github.com/openclaw/openclaw/issues/170331) — beta.2 更新候选配置读取以退出码 1 退出，并保持 systemd 单元不变。
- [#170332](https://github.com/openclaw/openclaw/issues/170332) — beta 更新在原始状态捕获期间实时状态 DB 发生变化后仍继续。
- [#164394](https://github.com/openclaw/openclaw/issues/164394) — Control UI WebChat transcript 持续抖动。
- [#108379](https://github.com/openclaw/openclaw/issues/108379) — Xiaomi MiMo 的助手生成重复。
- [#168413](https://github.com/openclaw/openclaw/issues/168413) — `memory.search.extraPaths` 内容从未被召回；存在陈旧的重索引锁。
- [#99659](https://github.com/openclaw/openclaw/issues/99659) — 配套应用连接后 OpenClaw 被 OOM kill。

### 队列中的潜在修复 PR
- [#170527](https://github.com/openclaw/openclaw/pull/170527) — 针对 provider 包装错误的模型回退（修复 #170388）。
- [#170548](https://github.com/openclaw/openclaw/pull/170548) — cron 手动运行保留工具访问权限（相关 #170470）。
- [#170534](https://github.com/openclaw/openclaw/pull/170534) — 静默轮次结束且不编造答案（相关 #170147、#150539）。
- [#166782](https://github.com/openclaw/openclaw/pull/166782) — 在带回复标记的流期间保持 Gateway 响应（关闭 #166781）。
- [#169172](https://github.com/openclaw/openclaw/pull/169172) — 工具调用文本泄漏修复（修复 #169161）。
- [#170553](https://github.com/openclaw/openclaw/pull/170553) — memory-wiki 状态超时修复（关闭 #170466）。
- [#169848](https://github.com/openclaw/openclaw/pull/169848) — 用于 CLI 子代理的 `lightContext`（关闭 #169704）。
- [#167250](https://github.com/openclaw/openclaw/pull/167250) / [#167251](https://github.com/openclaw/openclaw/pull/167251) — 畸形 UTF-8 密钥/模型输入安全。
- [#157144](https://github.com/openclaw/openclaw/pull/157144) — CLI 桥接请求作用域（通过 #165432 并入；相关 #157126）。
- [#170556](https://github.com/openclaw/openclaw/pull/170556) — worker transcript 权威。
- [#170555](https://github.com/openclaw/openclaw/pull/170555) — Gateway 关闭生命周期发布。
- [#169299](https://github.com/openclaw/openclaw/pull/169299) — Doctor 修复/报告改进。

## 6. 功能请求与路线图信号
| Request | Status | Likely Signal |
|---|---|---|
| [#50093](https://github.com/openclaw/openclaw/issues/50093) — WhatsApp 回填遗漏消息 | P1，陈旧，需要产品决策 | 频道可靠性和遗漏消息恢复。 |
| [#78308](https://github.com/openclaw/openclaw/issues/78308) — MCP 经频道中介的批准 / 同意信封 | P2 安全 | 针对 MCP 副作用的同意/批准流水线。 |
| [#16670](https://github.com/openclaw/openclaw/issues/16670) — 新手引导向导应包含 Memory/Embedding 设置 | P2，2 👍 | 更好的首次运行内存配置。 |
| [#76493](https://github.com/openclaw/openclaw/issues/76493) — 允许在 `mcp.servers[].env` 中使用 SecretRef 对象 | P2，4 👍 | 密钥间接引用和安全的 MCP 配置。 |
| [#111143](https://github.com/openclaw/openclaw/issues/111143) — 在 Control UI 侧边栏中固定插件标签页 | P3，1 👍 | 针对插件界面的 UI 自定义。 |
| [#114414](https://github.com/openclaw/openclaw/issues/114414) — 带日期 TODO 清扫 | P3 | 可维护性/技术债降低。 |
| [#169254](https://github.com/openclaw/openclaw/issues/169254) — 将 JSON 过滤/删除字段提升为列 | P3 | PostgreSQL 可移植性和数据模型清理。 |

**已关闭功能信号：** [#9986](https://github.com/openclaw/openclaw/issues/9986) 上下文长度超限时的模型回退，[#39811](https://github.com/openclaw/openclaw/issues/39811) 模型名称验证，[#127239](https://github.com/openclaw/openclaw/issues/127239) 真实上下文窗口目录值，以及 [#119692](https://github.com/openclaw/openclaw/issues/119692) MiniMax-M3 用量核算。

**预测：** 下一个 beta/稳定版系列很可能会继续 **更新恢复和 Windows/macOS 交接** 主题。此后，最强的路线图信号是 **Control UI Solid 迁移**、**插件运行时引导/GC**、**memory/embedding 新手引导**、**MCP 批准/SecretRef 安全** 以及 **频道消息恢复可靠性**。

## 7. 用户反馈摘要
- **更新/安装挫败感是最大痛点。** 用户报告 `openclaw update` 失败或停滞，而直接 npm 安装成功，此外还有 Windows 计划任务、macOS launchd、beta 更新和交接失败。示例：[#156112](https://github.com/openclaw/openclaw/issues/156112)、[#146860](https://github.com/openclaw/openclaw/issues/146860)、[#167506](https://github.com/openclaw/openclaw/issues/167506)、[#168750](https://github.com/openclaw/openclaw/issues/168750)、[#170331](https://github.com/openclaw/openclaw/issues/170331)、[#170332](https://github.com/openclaw/openclaw/issues/170332)、[#138560](https://github.com/openclaw/openclaw/issues/138560)、[#164214](https://github.com/openclaw/openclaw/issues/164214)、[#158936](https://github.com/openclaw/openclaw/issues/158936)。
- **稳定性和资源耗尽仍然影响巨大。** 内存泄漏（[#159662](https://github.com/openclaw/openclaw/issues/159662)、[#159356](https://github.com/openclaw/openclaw/issues/159356)、[#99659](https://github.com/openclaw/openclaw/issues/99659)）、僵尸进程（[#97616](https://github.com/openclaw/openclaw/issues/97616)）、磁盘增长（[#158390](https://github.com/openclaw/openclaw/issues/158390)）、重启挂起（[#166894](https://github.com/openclaw/openclaw/issues/166894)、[#167652](https://github.com/openclaw/openclaw/issues/167652)、[#160060](https://github.com/openclaw/openclaw/issues/160060)）以及启动回归（[#168955](https://github.com/openclaw/openclaw/issues/168955)）。
- **消息投递/可靠性用例很重要。** WhatsApp 遗漏消息（[#50093](https://github.com/openclaw/openclaw/issues/50093)）、Telegram 丢弃回调（[#126950](https://github.com/openclaw/openclaw/issues/126950)）、Discord 双重语音（[#125826](https://github.com/openclaw/openclaw/issues/125826)）、静默失败运行（[#128916](https://github.com/openclaw/openclaw/issues/128916)）以及插件重载聊天失败（[#156353](https://github.com/openclaw/openclaw/issues/156353)）。
- **安全/同意反馈正在增加。** MCP 工具批准（[#78308](https://github.com/openclaw/openclaw/issues/78308)）、SecretRef 支持（[#76493](https://github.com/openclaw/openclaw/issues/76493)）、`tools.deny` 强制执行（[#132303](https://github.com/openclaw/openclaw/issues/132303)）以及记忆溯源信任（[#153426](https://github.com/openclaw/openclaw/issues/153426)）。
- **满意度/不满意度：** 社区高度参与，提供了详细复现，维护者也关闭了许多 issue/PR。不满意度集中在未解决的 P0 更新/Windows 可靠性，以及长期卡在“需要产品决策”或“需要现场复现”的条目上。

## 8. 积压观察
需要维护者/产品/安全关注的重要 issue：

| Issue | Age / Status | Why It Needs Attention |
|---|---|---|
| [#50093](https://github.com/openclaw/openclaw/issues/50093) | 2026-03-19，P1，陈旧，需要产品决策 | WhatsApp 回填是一个持续存在的消息丢失功能缺口。 |
| [#97616](https://github.com/openclaw/openclaw/issues/97616) | 2026-06-29，P1，需要现场复现 | 僵尸进程累积会降低长期运行安装的性能。 |
| [#78308](https://github.com/openclaw/openclaw/issues/78308) | 2026-05-06，P2 安全，需要安全/产品审查 | MCP 工具调用同意是一项安全边界功能。 |
| [#16670](https://github.com/openclaw/openclaw/issues/16670) | 2026-02-15，P2，2 👍 | Memory/embedding 新手引导对首次运行成功很重要。 |
| [#76493](https://github.com/openclaw/openclaw/issues/76493) | 2026-05-03，P2，4 👍 | 在 MCP env 中支持 SecretRef 是一项高需求的安全改进。 |
| [#77249](https://github.com/openclaw/openclaw/issues/77249) | 2026-05-04，P1 | Slack 重连监控器可能因僵尸 WSS 挂起。 |
| [#125826](https://github.com/openclaw/openclaw/issues/125826) | 2026-08-18，P1 | Discord 语音双重投递影响用户信任。 |
| [#101929](https://github.com/openclaw/openclaw/issues/101929) | 2026-07-08，P1，需要维护者审查 | 上下文估算器多计会导致误触发截断/恢复。 |
| [#156112](https://github.com/openclaw/openclaw/issues/156112) | 2026-09-23，P0，仅手动 | 更新器失败，而直接 npm 安装成功。 |
| [#146860](https://github.com/openclaw/openclaw/issues/146860) | 2026-09-13，P0 | Windows 托管更新交接停滞。 |
| [#154891](https://github.com/openclaw/openclaw/issues/154891) | 2026-09-21，P1 | 失败的热重载可能使无关插件变砖，直到重启。 |
| [#158390](https://github.com/openclaw/openclaw/issues/158390) | 2026-09-25，P0 | 插件捕获临时目录无限填满磁盘。 |
| [#132303](https://github.com/openclaw/openclaw/issues/132303) | 2026-08-29，P1 安全 | claude-cli 后端静默忽略 `tools.deny`。 |
| [#153426](https://github.com/openclaw/openclaw/issues/153426) | 2026-09-20，P0 安全/会话 | 精选记忆根可能被永久排除。 |
| [#167652](https://github.com/openclaw/openclaw/issues/167652) | 2026-10-09，P0，需要现场复现 | Windows Gateway 在升级后挂起，尽管已验证重启。 |
| [#160060](https://github.com/openclaw/openclaw/issues/160060) | 2026-09-28，P0 | Windows Gateway 在待机唤醒后静默死亡。 |
| [#166894](https://github.com/openclaw/openclaw/issues/166894) | 2026-10-08，P0 | Windows 多代理重启挂起 30+ 分钟至 2 小时。 |
| [#164294](https://github.com/openclaw/openclaw/issues/164294) | 2026-10-03，P2 | SQLite WAL checkpoint 从不完成。 |
| [#138560](https://github.com/openclaw/openclaw/issues/138560) | 2026-09-04，P0 | Windows Control UI 更新中止。 |
| [#164799](https://github.com/openclaw/openclaw/issues/164799) | 2026-10-04，P1 | 版本变更后代理逐个接入；readyz 503 直到重启。 |

**需要维护者关注的 PR：**
- [#157106](https://github.com/openclaw/openclaw/pull/157106) — 当 Codex 线程预览巨大时，catalog 列表失败；P1，需要证据。
- [#146986](https://github.com/openclaw/openclaw/pull/146986) — 按最低严重级别过滤日志尾部；自 2026-09-13 起开放。
- [#169649](https://github.com/openclaw/openclaw/pull/169649) — 在干净关闭后复用 SQLite 验证；等待作者。
- [#170152](https://github.com/openclaw/openclaw/pull/170152) — 插件引导会话的活动运行；需要证据/安全边界审查。
- [#170536](https://github.com/openclaw/openclaw/pull/170536) — 停止重新检查合格目标缺失；等待作者。

---

## 横向生态对比

# 跨项目对比报告 — 个人 AI 助手 / Agent 开源生态系统  
**数据日期：** 2026-10-13  
**项目：** OpenClaw、Hermes Agent、IronClaw、QwenPaw、ZeroClaw  
**说明：** 健康评分为基于所提供摘要数据的主观 0–10 综合分：活跃度、发布节奏、响应速度、开放风险和评审/合并吞吐量。

---

## 1. 生态系统概览

该生态系统高度分层：OpenClaw 的运营规模与所有同行都不在同一层级，每天有数百次 Issue/PR 更新和多个发布，而 Hermes Agent、QwenPaw 和 ZeroClaw 则展现出活跃但仍以开放 Issue 堆积为主的开发。跨项目来看，主导工程主题正在趋同：更新/安装恢复、内存与上下文持久性、多智能体编排、可观测性/成本核算、安全/同意门控，以及跨平台可靠性。成熟度差异很大——OpenClaw 最广泛、最具生产导向，Hermes 和 QwenPaw 处于活跃的功能/维护阶段，ZeroClaw 处于预发布阶段，IronClaw 主要是基准/评估跟踪。评审带宽，而非贡献者供给，似乎是除 OpenClaw 外大多数项目的瓶颈。差异化仍然显著：OpenClaw 是通用平台，Hermes 强调混合远程/本地执行和可观测性，QwenPaw 面向创作者/自托管者，ZeroClaw 推进多智能体/A2A 架构，IronClaw 跟踪模型质量故障。

---

## 2. 活跃度对比

| 项目 | Issue 更新数 / 状态 | PR 更新数 / 状态 | 发布状态 | 健康评分 |
|---|---:|---:|---|---:|
| **OpenClaw** | **500** 个已更新；335 个开放/活跃，165 个已关闭 | **500** 个已更新；314 个开放，186 个已合并/关闭 | **2 个发布：** v2026.10.5-beta.2、v2026.10.2 | **7.0/10** — 吞吐量和发布节奏高，但仍存在多个 P0 级更新/Windows/内存/磁盘风险 |
| **Hermes Agent** | **50** 个已更新；20 个开放，30 个已关闭 | **50** 个已更新；43 个开放，7 个已合并/关闭 | **v0.21.7** 补丁；汇总自 v0.21.6 以来的约 204 个 PR | **6.5/10** — 活跃，但存在开放 PR 评审瓶颈和 Langfuse 重复问题簇 |
| **IronClaw** | **1** 个已更新；1 个开放，0 个已关闭 | **0** 个已更新；0 个开放，0 个已合并 | **无** | **3.0/10** — 工程吞吐量低；仅跟踪基准/模型质量 |
| **QwenPaw** | **17** 个已更新；15 个开放，2 个已关闭 | **24** 个已更新；20 个开放，4 个已合并/关闭 | 窗口内**无**；Creator 2.0.1 已通过 PR #8121 合并 | **8.0/10** — 当日修复速度强，但 Windows 桌面/控制台问题仍存在 |
| **ZeroClaw** | **33** 个已更新；29 个开放，约 4 个已关闭 | **50** 个已更新；44 个开放，6 个已合并/关闭 | 窗口内**无**；v0.8.6/v0.9.0 跟踪器活跃 | **7.0/10** — 贡献者势头高，但决策/评审延迟显著 |

---

## 3. OpenClaw 的定位

**相对同行的优势**
- **规模：** OpenClaw 在 24 小时内更新了 500 个 Issue + 500 个 PR，约为 Hermes/ZeroClaw 的 **10×**，以及 QwenPaw/IronClaw 的 **20–30×**。它还在同一窗口内关闭了 **165 个 Issue**，并合并/关闭了 **186 个 PR**。
- **发布节奏：** 落地了两个发布，包括一次稳定通道升级，以及一个聚焦更新恢复和 Windows/macOS 安装路径的 beta。
- **广度：** OpenClaw 覆盖更新器、网关、托管服务交接、Control UI、插件、内存、MCP 安全、渠道以及会话/转录权威。同行更窄：Hermes 聚焦混合执行/可观测性，QwenPaw 聚焦创作者/自托管，ZeroClaw 聚焦多智能体/A2A，IronClaw 聚焦基准测试。
- **社区规模：** 热门 Issue 能吸引 14–26 条评论，并带有详细复现；同行项目的热门 Issue 通常有 4–17 条评论。
- **维护者吞吐量：** OpenClaw 是该集合中唯一一个合并/关闭量超过开放 PR 累积量的项目。

**技术路线差异**
- OpenClaw 表现得像**核心平台/参考实现**：稳定/beta 通道、Doctor 修复流程、原地更新恢复、托管服务交接、Control UI 的 Solid 迁移、插件运行时调控，以及 MCP/密钥安全。
- 同行更加专业化：Hermes 推进远程智能体/本地工具执行；QwenPaw 构建创作者/控制台/自托管工作流；ZeroClaw 设计 A2A/群体/插件/WASM；IronClaw 评估模型质量故障。

**主要风险**
- OpenClaw 的优势被未解决的 **P0 更新/安装、Windows 服务挂起、内存泄漏、磁盘增长和进程僵尸问题** 所抵消。其健康状况是“高活跃、可靠性参差”，而非干净的稳定性。

---

## 4. 共享技术关注领域

| 关注领域 | 项目 | 具体需求 |
|---|---|---|
| **更新/安装/回滚可靠性** | OpenClaw, QwenPaw, ZeroClaw, Hermes | OpenClaw：更新器失败/停滞，Windows/macOS 交接，beta 恢复。QwenPaw：Windows 陈旧 PID/文件锁失败。ZeroClaw：插件安装/更新加固。Hermes：补丁汇总和桌面端引导。 |
| **持久内存、上下文、RAG** | OpenClaw, ZeroClaw, QwenPaw, Hermes | OpenClaw：内存泄漏、MEMORY.md 排除、memory-wiki、embedding 引导。ZeroClaw：持久内存对等、RAG 语料库、对提示缓存友好的个性快照。QwenPaw：上下文压缩。Hermes：远程内存/会话/技能。 |
| **多智能体/A2A/群体编排** | ZeroClaw, OpenClaw, Hermes, QwenPaw | ZeroClaw：A2A 协议、群体 TUI、持久对等智能体轮次、暗工厂工作流。OpenClaw：多智能体重启挂起、会话/转录权威、插件调控。Hermes：远程/本地混合执行。QwenPaw：worker 容器和 coding-CLI 管理。 |
| **可观测性、成本与隐私** | Hermes, ZeroClaw, OpenClaw, QwenPaw, IronClaw | Hermes：Langfuse 成本/会话/脱敏失败。ZeroClaw：UTC/本地成本总计、模型路由配置。OpenClaw：用量核算、上下文超计、memory-wiki 状态。QwenPaw：Langfuse 工具观测。IronClaw：故障分类法和基准分类。 |
| **安全、同意与密钥** | OpenClaw, ZeroClaw, Hermes, QwenPaw | OpenClaw：MCP 审批、SecretRef、tools.deny 强制执行、畸形 UTF-8 密钥。ZeroClaw：配对令牌、网关请求体大小、作用域日志、TLS。Hermes：跟踪脱敏/user_id。QwenPaw：BYO Tavily/Firecrawl 密钥。 |
| **跨平台桌面/Windows 可靠性** | OpenClaw, QwenPaw, Hermes, ZeroClaw | OpenClaw：Windows 服务/更新挂起。QwenPaw：Creator 崩溃、日志轮转、文件锁。Hermes：桌面应用/引导。ZeroClaw：ZeroCode TUI 按键处理。 |
| **渠道/消息投递可靠性** | OpenClaw, Hermes, ZeroClaw, QwenPaw | OpenClaw：WhatsApp 回填、Telegram 回调、Discord 语音、Slack WSS。Hermes：Telegram DM 主题、DingTalk 媒体。ZeroClaw：渠道分离、Discord 插件。QwenPaw：多设备消息同步。 |
| **工具调用/流式生命周期正确性** | OpenClaw, QwenPaw, ZeroClaw | OpenClaw：静默轮次结束、工具调用文本泄漏、CLI 桥接作用域。QwenPaw：审批竞态、卸载 404、静默空完成。ZeroClaw：丢弃 ask_user/poll、自然语言文本中的工具调用信封。 |
| **自托管、BYO 密钥、反向代理、模块化** | QwenPaw, ZeroClaw, Hermes, OpenClaw | QwenPaw：Tavily/Firecrawl、basename、NAS/Docker。ZeroClaw：将渠道与核心分离、可配置请求体大小。Hermes：自托管 TTS 兼容性。OpenClaw：私有 Node 更新路径。 |

---

## 5. 差异化分析

| 项目 | 功能重点 | 目标用户 | 架构信号 |
|---|---|---|---|
| **OpenClaw** | 广泛的个人助手平台：更新器、网关、Control UI、插件、渠道、内存、MCP 安全 | 高级用户、自托管者、插件开发者、需要可靠性的团队 | 核心网关/服务、托管交接、Doctor、Solid UI、插件运行时、会话/转录权威 |
| **Hermes Agent** | 混合远程智能体 + 本地工具执行、可观测性、桌面/连接器 | 对隐私/延迟敏感的用户、多配置文件用户、Langfuse 观察者 | 桌面 + 网关 + 连接器 + CLI 归属重构；带本地工具的远程内存/会话 |
| **IronClaw** | 基准故障分类法和模型质量跟踪 | 研究人员、基准维护者、模型评估者 | 基准测试框架/每日分类法；本窗口内运行时功能活动低 |
| **QwenPaw** | 创作者/媒体制作、Windows 桌面、控制台自托管、多设备 UX | 创作者、NAS/Docker 自托管者、多设备用户 | Creator 插件、PawApp 控制台、worker 容器、反向代理支持、提供商配置 |
| **ZeroClaw** | 多智能体编排、A2A、插件/WASM 渠道、ZeroCode TUI | 平台构建者、运维人员、多智能体工作流设计者 | 网关/运行时、插件/WASM 栈、A2A 协议 crate、TUI、持久内存对等 |

**关键要点：** OpenClaw 以广度和生产就绪度竞争；Hermes 以混合执行和可观测性竞争；QwenPaw 以创作者/自托管易用性竞争；ZeroClaw 以多智能体协议架构竞争；IronClaw 以评估而非运行时功能竞争。

---

## 6. 社区势头与成熟度

**活跃度分层**
- **第 1 层 — 极端规模：** OpenClaw。500/500 个 Issue/PR 更新，165 个 Issue 关闭，186 个 PR 合并/关闭，两个发布。
- **第 2 层 — 高活跃、开放积压多：** ZeroClaw、Hermes Agent、QwenPaw。这些项目展现出强劲的贡献者能量，但存在评审/合并瓶颈。
- **第 3 层 — 可见活跃度低：** IronClaw。一个基准分类法 Issue，无 PR，无发布。

**快速迭代**
- **ZeroClaw：** 当日 bug→修复 PR；预发布 v0.8.6/v0.9.0 跟踪器；活跃的 RFC 流水线。
- **QwenPaw：** 多位首次贡献者在 Issue 提交当天就打开了修复；四个 PR 合并/关闭。
- **Hermes：** 补丁 v0.21.7 汇总了约 204 个 PR，但 43 个开放 PR 对 7 个合并/关闭，表明评审压力。
- **OpenClaw：** 快速关闭崩溃/会话/更新 Issue，但 P0 可靠性积压使风险居高不下。

**稳定化 vs 预发布**
- **最成熟/最广泛：** OpenClaw。
- **接近稳定，有补丁节奏：** Hermes Agent。
- **生产级自托管，但存在 Windows 信任缺口：** QwenPaw。
- **预发布架构推进：** ZeroClaw。
- **利基评估跟踪：** IronClaw。

**评审瓶颈**
- Hermes：**43 个开放 vs 7 个合并/关闭**。
- ZeroClaw：**44 个开放 vs 6 个合并/关闭**。
- QwenPaw：**20 个开放 vs 4 个合并/关闭**。
- OpenClaw 是例外：24 小时内 **186 个合并/关闭**。

---

## 7. 趋势信号

1. **更新器/安装器如今是一等 Agent 基础设施。**  
   OpenClaw、QwenPaw 和 ZeroClaw 都表现出围绕更新、安装交接、陈旧 PID、文件锁和恢复的痛点。开发者应将更新/回滚视为核心子系统，而不是打包胶水。

2. **内存、上下文和 RAG 正成为持久差异化因素。**  
   OpenClaw 的内存泄漏/排除、ZeroClaw 的内存对等/RAG 语料库、QwenPaw 的上下文压缩，以及 Hermes 的远程内存/会话需求，都指向内存持久性、来源溯源和提示缓存效率等核心要求。

3. **多智能体编排正从临时配置走向协议。**  
   ZeroClaw 的 A2A/RFC/群体工作、OpenClaw 的多智能体重启/会话权威问题，以及 Hermes 的远程/本地混合请求，表明市场需要一等智能体间消息传递、持久轮次和会话所有权。

4. **可观测性与成本核算必须准确且兼顾隐私。**  
   Hermes 的 Langfuse 重复问题簇、ZeroClaw 的成本标签页问题、OpenClaw 的用量/上下文核算 bug，以及 QwenPaw 的 Langfuse 工具观测缺口，都表明跟踪、成本和脱敏是生产要求。

5. **安全/同意正在超越 API 密钥。**  
   MCP 审批、SecretRef、tools.deny 强制执行、配对令牌、作用域日志、跟踪脱敏和 BYO 提供商密钥都是反复出现的需求。针对工具副作用的同意门控正成为平台功能。

6. **跨平台 Windows/桌面可靠性是竞争风险。**  
   OpenClaw 和 QwenPaw 都有 Windows 特有的 P0/高优先级 Issue：服务挂起、文件锁、陈旧 PID 处理、日志轮转。Agent 开发者必须考虑 Windows 的进程、文件和服务语义。

7. **渠道/消息可靠性仍然是核心 UX。**  
   WhatsApp 回填、Telegram DM 路由、Discord 语音重复、Slack WSS 挂起、DingTalk 媒体和多设备同步都出现在各个项目中。防止消息丢失和重连恢复并非边缘场景。

8. **静默失败尤其具有破坏性。**  
   OpenClaw 的静默轮次结束、QwenPaw 的静默空完成和二进制文件损坏、ZeroClaw 的丢弃引导请求和虚假成功 cron——当 Agent 在没有明确信号的情况下失败时，用户会失去信任。

9. **模块化与自托管需求正在上升。**  
   用户想要 BYO 密钥、反向代理子路径、将渠道与核心分离、插件热重载/卸载、减少副作用模式，以及 NAS/Docker 支持。“不要交付我不使用的东西”是真实的安全与运维偏好。

10. **对 AI Agent 开发者而言，构建清单很清晰：**  
    可恢复更新、受监督进程、带来源溯源的持久内存、协议级多智能体支持、准确的成本/可观测性、同意门控工具、跨平台服务处理，以及显式暴露失败。

**底线：** OpenClaw 在规模、广度和发布吞吐上领先，但其未解决的 P0 可靠性事项是该生态系统最显眼的风险。Hermes、QwenPaw 和 ZeroClaw 正在差异化细分领域中快速迭代，而 IronClaw 仍是低活跃度的评估信号。下一阶段竞争很可能由更新/安装可靠性、内存/RAG 持久性、多智能体协议、可观测性正确性以及 Windows/桌面信任决定。

---

## 同赛道项目详细报告

<details>
<summary><strong>Hermes Agent</strong> — <a href="https://github.com/nousresearch/hermes-agent">nousresearch/hermes-agent</a></summary>

# Hermes Agent 项目摘要 — 2026-10-13

## 1. 今日概览

2026-10-13 Hermes Agent 活跃度**高**：共有 50 个 issue 更新，其中 20 个开放/活跃、30 个已关闭；50 个 PR 更新，43 个开放，仅 7 个已合并/关闭。Issue 侧很大程度上受一个大型 Langfuse 可观测性 bug/重复项集群影响，该集群占据了已关闭数量的大部分。PR 流以开放为主，表明开发活跃，但也可能存在审查/合并瓶颈。项目还发布了一个新的补丁版本 **v0.21.7**，汇总了自 v0.21.6 以来约 204 个 PR。整体项目健康度活跃，但维护者需要关注长期未决决策、重复项分诊以及若干 P2 平台 bug。

## 2. 发布

### v0.21.7 — 补丁版本
- **发布日期：** October 11, 2026
- **类型：** 补丁版本；将自 v0.21.6 以来合并的约 204 个 PR 汇总为稳定版本。
- **目标平台：** 桌面应用（macOS Apple silicon、Windows）、Microsoft Store/MSIX、安装器、Docker 和 Hermes Cloud。
- **破坏性变更/迁移说明：** 提供的发布摘录中未明确说明。完整整理版发布说明被截断，因此维护者在升级前应核实完整说明。
- **评估：** 这是一次稳定性汇总，而非功能版本。所提供数据中未看到安全公告或迁移要求。

## 3. 项目进展

### 今日合并/关闭的 PR
数据集报告 **7 个已合并/关闭的 PR**，不过提供的前 20 个中仅出现部分：
- [#137426](https://github.com/NousResearch/hermes-agent/pull/137426) — `feat(connectors): manage_connections display, a browsable connector card`。新增连接器目录卡片，并让模型可在聊天中显示连接状态。
- [#137525](https://github.com/NousResearch/hermes-agent/pull/137525) — `fix(solstice): offer Gemini 3.1 Pro, 3.8 Flash and 3.5 Flash Lite`。更新 Solstice Gemini OAuth provider 中硬编码的模型列表。

### 推进中的开放 PR
开放 PR 队列范围广泛，涵盖桌面、gateway、CLI、插件、平台集成和安全工作：
- [#137522](https://github.com/NousResearch/hermes-agent/pull/137522) — 桌面首次运行引导聊天和 `set_theme`。
- [#137420](https://github.com/NousResearch/hermes-agent/pull/137420) — 删除旧的 agentic 设置流程。
- [#125028](https://github.com/NousResearch/hermes-agent/pull/125028) — P1 CLI Ownership Refactor Phase 2：gateway 控制与拓扑。
- [#31871](https://github.com/NousResearch/hermes-agent/pull/31871) — DingTalk 入站媒体缓存。
- [#96917](https://github.com/NousResearch/hermes-agent/pull/96917) — DingTalk 出站图片/文件上传。
- [#127820](https://github.com/NousResearch/hermes-agent/pull/127820) — `httpx2` 安全依赖升级。
- [#137564](https://github.com/NousResearch/hermes-agent/pull/137564) — 浏览器真实 profile 浏览器选择。
- [#137560](https://github.com/NousResearch/hermes-agent/pull/137560) — 每轮对话后的 composer 回复建议。
- [#137566](https://github.com/NousResearch/hermes-agent/pull/137566)、[#137556](https://github.com/NousResearch/hermes-agent/pull/137556)、[#137559](https://github.com/NousResearch/hermes-agent/pull/137559)、[#137562](https://github.com/NousResearch/hermes-agent/pull/137562)、[#137563](https://github.com/NousResearch/hermes-agent/pull/137563) — gateway/桌面/session/profile 修复。

### 表明分诊/修复进展的已关闭 issue
今日许多已关闭 issue 属于 Langfuse 可观测性插件：
- [#72661](https://github.com/NousResearch/hermes-agent/issues/72661) — Langfuse `total_cost` 为零，尽管有按类型细分。
- [#72484](https://github.com/NousResearch/hermes-agent/issues/72484) — 自定义 endpoint 凭据因 key 前缀被拒绝。
- [#71556](https://github.com/NousResearch/hermes-agent/issues/71556) — 随机 `session_id`，而非稳定的 gateway session key。
- [#81730](https://github.com/NousResearch/hermes-agent/issues/81730) — session 标签未传递到子 observation。
- [#58322](https://github.com/NousResearch/hermes-agent/issues/58322) — `service.name=unknown_service`。
- [#59026](https://github.com/NousResearch/hermes-agent/issues/59026) — 更新后 Langfuse SDK 被移除。
- [#32005](https://github.com/NousResearch/hermes-agent/issues/32005) — 所有轮次复用同一个 trace ID。
- [#95057](https://github.com/NousResearch/hermes-agent/issues/95057) — OTel `Failed to detach context` 错误。

## 4. 社区热点

### 最活跃 issue
1. [#18715](https://github.com/NousResearch/hermes-agent/issues/18715) — **支持远程 Hermes agent 并本地执行工具**  
   - 开放，P2，`needs-decision`，26 条评论，40 👍。  
   - 潜在需求：用户希望采用混合架构，将 memory/sessions/skills/model config 保留在远程 Hermes 实例上，但工具在本地执行，以满足隐私、延迟或本地资源访问需求。这是数据集中最强的社区信号。

2. [#136567](https://github.com/NousResearch/hermes-agent/issues/136567) — **Telegram DM topics：开场消息以无 thread 方式送达 → lobby recovery 将其错误路由**  
   - 开放，P2，17 条评论，3 👍。  
   - 潜在需求：可靠的 Telegram forum/DM topic 路由。这会影响 Telegram 重度用户的消息送达和 session 状态。

3. Langfuse 可观测性集群 — 例如 [#52389](https://github.com/NousResearch/hermes-agent/issues/52389)、[#51399](https://github.com/NousResearch/hermes-agent/issues/51399)、[#63787](https://github.com/NousResearch/hermes-agent/issues/63787)、[#67544](https://github.com/NousResearch/hermes-agent/issues/67544)  
   - 许多已关闭重复项和若干开放/关闭 bug。尽管许多重复项被关闭，评论量仍然很高。  
   - 潜在需求：可靠的可观测性、正确的成本核算、稳定的 session 链式关联、profile 级归因，以及导出输入/输出的隐私/脱敏。

### 值得注意的 PR
所提供数据中 PR 评论数报告为 `undefined`，因此无法按评论数排序。按范围和优先级：
- [#137522](https://github.com/NousResearch/hermes-agent/pull/137522) — 桌面引导/主题，可能具有较高用户可见影响。
- [#125028](https://github.com/NousResearch/hermes-agent/pull/125028) — P1 CLI Ownership Refactor，重要架构工作。
- [#31871](https://github.com/NousResearch/hermes-agent/pull/31871) 和 [#96917](https://github.com/NousResearch/hermes-agent/pull/96917) — DingTalk 媒体可靠性。
- [#137560](https://github.com/NousResearch/hermes-agent/pull/137560) — composer 回复建议，一项 UX 增强。

## 5. Bug 与稳定性

按严重程度和用户影响排序：

1. **P2 — [#136567](https://github.com/NousResearch/hermes-agent/issues/136567) Telegram DM topic 错误路由**  
   新 Telegram topic 中的开场消息在送达时缺少 `message_thread_id`，导致 lobby recovery 将其错误路由到上一个 topic。影响消息送达和 session 状态。所提供的前列 PR 列表中未见直接修复 PR。

2. **P2 — [#73470](https://github.com/NousResearch/hermes-agent/issues/73470) OpenAI 兼容 TTS `response_format=opus` 失败**  
   自托管的 OpenAI 兼容 TTS 后端（如 Speaches/Kokoro）拒绝 `opus`，导致没有音频。报告称在 #57184 之后仍然失败。所提供数据中未见修复 PR。

3. **P2 — [#137544](https://github.com/NousResearch/hermes-agent/issues/137544) 在 Free 计划下，更高档位显示“Downgrade”**  
   今日创建。在 Free 计划下，Plus 和 Super 显示“Downgrade”而非“Choose/Upgrade”。如果用户点击破坏性操作，存在计费/UX 风险。尚未见修复 PR。

4. **P3 / 隐私 — [#67544](https://github.com/NousResearch/hermes-agent/issues/67544) Langfuse trace 导出未脱敏的输入/输出**  
   没有 masking hook，也没有 `user_id`。按标签为 P3，但对隐私敏感部署影响很大。仍处于开放状态。

5. **已关闭的 Langfuse 稳定性集群**  
   今日关闭了多个 Langfuse bug，包括静默 placeholder-key 失败、session 链式关联、成本总计、服务命名、更新后 SDK 移除，以及 OTel context 错误。这减少了重复积压，但也表明更广泛的插件可靠性和文档问题。

## 6. 功能请求与路线图信号

- [#18715](https://github.com/NousResearch/hermes-agent/issues/18715) — 远程 Hermes agent 与本地工具执行。**强力路线图候选**，因为有 40 👍 和 26 条评论，但标记为 `needs-decision`；可能需要架构设计，未必会立即发布。
- [#67544](https://github.com/NousResearch/hermes-agent/issues/67544) — Langfuse masking/user_id。可能与未来的可观测性强化捆绑。
- [#26455](https://github.com/NousResearch/hermes-agent/issues/26455) — 将 `profile_name` 暴露为 trace `userId`。今日关闭；可能表明按 profile 的可观测性正在处理。
- [#137522](https://github.com/NousResearch/hermes-agent/pull/137522) — 桌面首次运行引导和 `set_theme`。可能面向下一个桌面版本。
- [#137420](https://github.com/NousResearch/hermes-agent/pull/137420) — 删除旧 agentic 设置流程。略带破坏性的清理；可能在引导切换后落地。
- [#137560](https://github.com/NousResearch/hermes-agent/pull/137560) — Composer 回复建议。可能出现在即将到来的 TUI/Desktop 版本中。
- [#137564](https://github.com/NousResearch/hermes-agent/pull/137564) — 浏览器真实 profile 浏览器选择。对 Chromium 系列用户有用。
- [#96917](https://github.com/NousResearch/hermes-agent/pull/96917) — DingTalk 出站媒体。可能旨在完善平台完整性。

**预测：** 下一个补丁/次版本可能会包含桌面引导/主题、连接器管理、浏览器 profile 选择、gateway/session 修复，以及可能的 DingTalk 媒体。远程/本地混合工具执行请求更可能是 v0.22+ 路线图项目，因为它需要维护者决策和架构工作。

## 7. 用户反馈摘要

- **混合部署需求：** 用户希望远程 Hermes 负责 memory/sessions/skills，但本地执行工具，以满足隐私、延迟和本地资源访问需求。这是最明确的高价值未满足需求。
- **Telegram 可靠性：** Telegram DM/forum-topic 用户遇到消息路由失败，破坏了 session 连续性。
- **可观测性挫败感：** Langfuse 用户报告静默失败、trace 缺失、成本为零、session 链式关联错误，以及未脱敏的数据导出。重复 issue 的数量表明错误呈现不佳和设置文档不清晰。
- **自托管 TTS 不兼容：** 运行 OpenAI 兼容 TTS 后端的用户在 Hermes 请求 `opus` 时被阻塞。
- **计费 UX 混乱：** 在 Free 计划下更高档位显示“Downgrade”按钮并非外观问题；它存在误点降级的风险。
- **多 profile 使用：** 多个请求和 PR 针对按 profile 搜索、STT 行为、trace 归因和可观测性，表明多 profile 采用率正在增长。
- **整体满意度信号：** 参与度高 — #18715 获得 40 👍 — 表明用户投入强烈。不满主要集中在静默可观测性失败和平台特定的消息/媒体处理上。

## 8. 积压观察

需要维护者关注的重要事项：

- [#18715](https://github.com/NousResearch/hermes-agent/issues/18715) — 自 2026-05-02 开放，26 条评论，40 👍，`needs-decision`。社区需求高，但架构问题未解决。
- [#73470](https://github.com/NousResearch/hermes-agent/issues/73470) — 自 2026-07-28 开放，P2 TTS `opus` 回归。未见修复 PR。
- [#67544](https://github.com/NousResearch/hermes-agent/issues/67544) — 自 2026-07-19 开放，Langfuse 导出中的隐私/脱敏缺口。
- [#125028](https://github.com/NousResearch/hermes-agent/pull/125028) — P1 CLI Ownership Refactor Phase 2，自 2026-09-27 开放。高优先级架构 PR。
- [#31871](https://github.com/NousResearch/hermes-agent/pull/31871) — DingTalk 入站媒体，自 2026-05-25 开放。长期运行的平台修复。
- [#96917](https://github.com/NousResearch/hermes-agent/pull/96917) — DingTalk 出站媒体，自 2026-08-28 开放。补充入站修复。
- [#128778](https://github.com/NousResearch/hermes-agent/pull/128778) — Config 只读路径修复，自 2026-09-30 开放。
- [#131067](https://github.com/NousResearch/hermes-agent/pull/131067) — 桌面沙箱/GPU 恢复标记，自 2026-10-02 开放，平台/Windows 和安全边界风险。
- [#124754](https://github.com/NousResearch/hermes-agent/pull/124754) — 受监督重启后 gateway 重新上线公告，自 2026-09-27 开放。

**要点：** Hermes Agent 高度活跃并持续发布补丁，但开放 PR 队列和若干 P2 issue 需要集中审查。Langfuse 重复集群应合并为一个规范 issue 或文档修复，以减少社区噪音。

</details>

<details>
<summary><strong>IronClaw</strong> — <a href="https://github.com/nearai/ironclaw">nearai/ironclaw</a></summary>

# IronClaw 项目简报 — 2026-10-13

**数据快照：** 过去 24 小时内更新的 Issue：1 个开放/活跃，0 个已关闭。PR 更新：0。新版本发布：0。

## 1. 今日概览
过去 24 小时内 IronClaw 活动极少，仅有一个开放 Issue 有更新，没有 PR 或发布活动。唯一条目是 #8135，“Daily ironclaw failure taxonomy — 2026-10-12”，它报告的是基准测试失败，而非代码变更。该条目涉及一次 officeqa 运行，包含 52 个未通过任务，全部归类为真实的模型质量错误，未发现例外。社区互动平淡：0 条评论、0 个回应。整体健康信号：工程吞吐量低，且无发布/迁移影响；主要可见活动是持续的基准测试与模型质量跟踪。

## 2. 发布
报告窗口内无新版本发布。

## 3. 项目进展
- 今日合并/关闭的 PR：**0**
- 推进或修复的功能：**未通过 PR 报告任何一项**
- 开放 Issue 活动：[#8135](https://github.com/nearai/ironclaw/issues/8135) 仍处于开放状态。它看起来是分类体系/基准测试跟踪项，而非代码进展项。

## 4. 社区热门话题
窗口内只有一个活跃条目：

- [#8135 [OPEN] Daily ironclaw failure taxonomy — 2026-10-12](https://github.com/nearai/ironclaw/issues/8135) — 0 条评论，0 👍

底层需求：对基准测试失败进行系统化分类，以区分模型质量错误与 agent、工具链或基础设施缺陷。该 Issue 链接到一次 [officeqa 基准测试运行](https://nearai.github.io/benchmarks/#/runs/ironclaw/officeqa/c0bf84d6-1954-42ef-8378-20f32b394f1e)，并指出 52 个未通过任务全部是真实的模型质量错误。目前尚无社区讨论。

## 5. Bug 与稳定性
- [#8135](https://github.com/nearai/ironclaw/issues/8135) — **严重性：对基准测试/模型质量为中等；不是运行时崩溃。**
  - 报告 52 个未通过的 officeqa 任务，全部归因于真实的模型质量错误，未发现例外。
  - 所提供数据中未报告崩溃、回归或运行时稳定性问题。
  - 此快照中不存在修复 PR。
  - 提供的摘要截断于 “DeepSeek-V4-Flas…”，因此无法获得完整的模型/任务细分。

## 6. 功能请求与路线图信号
- 过去 24 小时内没有打开或更新任何明确的功能请求。
- 路线图信号：每日重复出现的失败分类报告表明，对基准测试可观测性、自动化失败分类和模型质量跟踪的持续需求。
- 预测下一版本范围：无法从这一个 Issue 推断出具体功能。

## 7. 用户反馈摘要
- 来源：[#8135](https://github.com/nearai/ironclaw/issues/8135)，作者为 pranavraja99。
- 痛点：officeqa 基准测试失败 — 52 个未通过任务被归类为模型质量错误。
- 用例：面向 IronClaw 的自动化每日基准测试失败分析。
- 满意度/不满意度：无法从这些数据衡量；未记录到评论或回应。

## 8. Backlog 观察
- [#8135](https://github.com/nearai/ironclaw/issues/8135) 是近期事项（创建/更新于 2026-10-12），且没有评论。它并非长期未回复，但如果每日分类体系需要跟进，可能需要维护者分流。
- 在所提供 24 小时窗口内，未出现其他需要 Backlog 关注的重要开放 Issue 或 PR。

</details>

<details>
<summary><strong>QwenPaw</strong> — <a href="https://github.com/agentscope-ai/QwenPaw">agentscope-ai/QwenPaw</a></summary>

# QwenPaw 项目摘要 — 2026-10-13

## 1. 今日概览

QwenPaw 正处于高活跃度的维护与功能开发阶段：过去 24 小时内处理了 17 个 issue（15 个开放/活跃，2 个已关闭），更新了 24 个 PR（20 个开放，4 个已合并/关闭），并且**没有新发布**。当天吞吐量较为均衡——两个长期存在的 bug 被关闭（#8105 审批按钮、#8073 对话页面），同时针对新提交的 bug 报告，多个高信号修复 PR 被打开或推进，尤其是 Windows/桌面端 creator 插件崩溃和控制台 UX 回归。功能工作高度由社区驱动：三位不同的首次贡献者在同一天针对用户请求的功能（Tavily 密钥配置、Firecrawl 提供商、嵌套 skill 目录）提交了 PR。主要风险主题是 **Windows 桌面稳定性**（残留 PID 探测、文件锁删除失败、日志不轮转）和 **Console 前端正确性**（滚动锚定、多设备消息同步、GPU 开销）。整体项目健康度在响应速度方面表现强劲，不过开放的 PR 队列（20 个）和一小批停滞的高影响 issue 值得维护者关注。

## 2. 发布

过去 24 小时内没有新发布。（最近一次发布事件是 PR #8121，已关闭/合并——QwenPaw Creator 1.3.0 → 2.0.1，带受控媒体生产功能，将在第 3 节讨论。）

## 3. 项目进展

今天有四个 PR 被合并/关闭：

- **[PR #8188] fix(approval): 保留决策并处理超时竞态** — 关闭 **#8105**，这个长期存在的 bug 中工具审批总是解析为“reject”。根因是竞态：在工具进入等待状态前完成的决策会丢失。这是当天影响最大的修复。https://github.com/agentscope-ai/QwenPaw/pull/8188
- **[PR #8121] feat(creator): 发布 2.0.1，带受控媒体生产**（size/XXXL）—— 发布 QwenPaw Creator 插件升级 1.3.0 → 2.0.1，仅包含上游发布后的插件工作。https://github.com/agentscope-ai/QwenPaw/pull/8121
- **[PR #8193] fix(context): 更早回收活跃结果并验证溢出恢复** — 改进上下文压缩，使长时间活跃的轮次不再累积超过触发阈值的可恢复工具输出。https://github.com/agentscope-ai/QwenPaw/pull/8193
- **[Issue #8073] V2.2.2.beta4 在 LAN 上无法访问对话页面** — 已关闭；这是一个影响 LAN 客户端打开聊天页面的回归。https://github.com/agentscope-ai/QwenPaw/issues/8073

已推进但仍开放的功能：反向代理 basename 支持（#8197）、按聊天持久化审批级别（#8182）、coding-CLI 管理端点（#8156）、插件热重载/回滚（#7565），以及“减弱效果”控制台档位（#8137）。

## 4. 社区热门话题

按过去 24 小时评论数排名：

1. **#8174 — `web_search` Tavily 提供商没有 API-key 配置入口**（4 条评论）—— 仅支持无密钥的匿名模式，导致付费 Tavily 配额无法使用，并且在 401/429/432 时没有干净的降级方案。一位首次贡献者已经提交了修复，**PR #8204**。https://github.com/agentscope-ai/QwenPaw/issues/8174
2. **#7016 — 工具调用 404（`/offload` 上出现 `Tool call not found`）**（4 条评论）—— 流式会话在 offload 端点上反复遇到 404。自 2026-08-14 起开放，仍无关联修复 PR。https://github.com/agentscope-ai/QwenPaw/issues/7016
3. **#8172 — Console 间歇性静默空完成**（3 条评论）—— 一次运行约 1 秒内结束且输出为空，模型从未被调用，仅发生在嵌入式 PawApp 控制台通道上。高严重性，尚无修复 PR。https://github.com/agentscope-ai/QwenPaw/issues/8172

底层需求：用户正推动走向**生产级自托管**（自定义 API key、反向代理子路径、多提供商 web_search、服务端持久化）和**桌面端可靠性**，而反复出现的 404/静默完成报告则指向嵌入式与远程客户端的流式/运行生命周期存在脆弱性。

## 5. 缺陷与稳定性

按严重性排序（除非注明，否则均为开放状态）：

| # | Issue | 严重性 | 修复 PR？ |
|---|-------|----------|---------|
| 1 | **#8172** Console 静默空完成 — 模型从未被调用，未渲染任何回复（v2.2.0 Docker/NAS） | 严重（静默数据/UX 故障） | ❌ 无 |
| 2 | **#8179** 从 GitHub 安装 skill 会静默损坏 **二进制文件**（所有非 UTF-8 字节 → U+FFFD；图像/字体损坏） | 高（静默数据损坏） | ✅ PR #8198 |
| 3 | **#8206** 在 Windows 上只要 `creator.log` 打开，项目删除就总是失败（重命名目录时 WinError 5） | 高 | ❌ 无 |
| 4 | **#8194** `qwenpaw-creator` 启动钩子在残留 PID 上中止 — `os.kill(pid, 0)` 抛出 `SystemError`，而 `except OSError` 未捕获 | 高（应用无法启动） | ✅ PR #8196 |
| 5 | **#8184** Doom-loop 门控杀死合法的任务授权轮询；终止后 cron 会记录 **虚假成功** | 高（错误状态 + 静默失败） | ✅ PR #8192 |
| 6 | **#8185** `desktop.log` 在 Windows 上从不轮转（sidecar tee 句柄使轮转处理器失效） | 中（随时间耗尽磁盘） | ❌ 无 |
| 7 | **#8186** 消息导航跳转覆盖手动滚轮滚动，约 1.6 秒后弹回 | 中（UX） | ✅ PR #8201 |
| 8 | **#7016** 流式处理期间在 `/offload` 上出现工具调用 404 循环 | 中（自 8 月起开放） | ❌ 无 |
| 9 | **#8202** 在一台设备上发送的消息在另一台设备上不可见，直到该轮次结束 | 中（多设备 UX） | ✅ PR #8203 |
| 10 | **#8105** 工具审批按钮都会执行“reject” | 高 — **已关闭** ✅ | ✅ PR #8188（已合并） |

值得注意的模式：今天十个 issue 中有五个在当天或次日就有了修复 PR，表明贡献者到维护者的流转速度很强。两个 Windows creator 插件故障（#8206、#8194）共享一个根因主题——**未考虑 Windows 文件/进程语义**——可能值得进行一次整合加固。

## 6. 功能请求与路线图信号

请求的功能（含实现状态）：

- **可配置的 `web_search` 提供商** — Tavily API-key 配置界面（#8174、PR #8204）以及 Firecrawl 作为第三个提供商（#8199、PR #8200）。强烈表明**多提供商、自带密钥的 web search** 即将落地。
- **反向代理子路径支持** — 可配置的 console basename（#8177、PR #8197）；对自托管多服务域名很重要。
- **Console “reduced effects” 档位**（#8135、PR #8137）—— 降低集成显卡上的 GPU 负载；使 Console 面向低端/NAS 硬件。
- **服务端按聊天审批模式**（#8182）—— 将审批级别从浏览器 `localStorage` 移出，以实现跨设备一致性。
- **模型池删除**（#8195）—— 目前仅有启用/禁用；已弃用模型无法清除。
- **worker 容器的 coding-CLI 管理端点**（#8156）—— 按 worker 管理 `qwen-code`/`opencode` 模型与配置。
- **决策模型类型 + JEV 提供商**（#8205）—— 向后兼容的 `model_type`（`chat`/`decision`），暗示即将推出路由/结构化决策功能。
- **嵌套/分类 skill 目录**（#8178）—— `<root>/<category>/<skill>/SKILL.md` 布局。
- **插件热重载 / 干净卸载**（#7565，size/XXXL）—— 更长期的基础设施工作。
- **类似 Meta Muse 的 ESP32 硬件集成**（#8189）—— 探索性，近期发布不太可能。

**预测：** 下一波 minor/beta 版本可能捆绑 Tavily/Firecrawl 提供商配置、console basename 可配置性以及 Windows creator 插件修复；decision-model 管道（#8205）暗示后续版本将面向 agent 路由。

## 7. 用户反馈总结

- **痛点：** 付费 Tavily 用户无法使用自己的配额（#8174）；从 GitHub 安装 skill 时二进制资源被静默破坏（#8179）；当存在残留 PID 时，Windows 用户无法删除项目，甚至无法启动 Creator（#8206、#8194）；在今天的修复之前，工具审批实际上不可用（#8105）；LAN 客户端无法打开聊天页面（#8073）。
- **观察到的用例：** 反向代理后的 NAS/Docker 自托管、多设备聊天（web + 移动端，#8202）、运行第三方 coding CLI 的多 agent worker 容器（#8156），以及外部硬件集成（#8189）。
- **满意信号：** 快速周转——四个修复被合并/关闭，以及来自首次贡献者的多个当天响应 PR。用户以建设性方式参与（详细复现步骤、版本/环境元数据）。
- **不满信号：** 反复出现的 **Windows 桌面/creator 可靠性** 投诉、静默失败（空完成、二进制损坏、cron 虚假成功）以及 desktop.log 增长——一个一致的“对桌面构建的信任”主题。

## 8. 积压事项观察

需要维护者关注的事项：

- **#7016 — 工具调用 404** — 自 **2026-08-14** 起开放（约 2 个月），4 条评论，无关联修复 PR。流式 offload 404 循环是核心路径缺陷。https://github.com/agentscope-ai/QwenPaw/issues/7016
- **PR #6384 — `AskUserQuestion` 交互式工具** — 自 **2026-07-23** 起开放（约 3 个月）且处于“Under Review”；大型用户可见能力卡在审查中。https://github.com/agentscope-ai/QwenPaw/pull/6384
- **PR #7565 — 插件干净卸载 / 可回滚安全热重载** — size/XXXL，自 **2026-09-04** 起开放，涉及插件生命周期；架构价值高，但落地缓慢。https://github.com/agentscope-ai/QwenPaw/pull/7565
- **PR #7964 — Langfuse 工具观测从不记录输出** — 自 **2026-09-24** 起处于 Under Review；工具 span 的可观测性正确性。https://github.com/agentscope-ai/QwenPaw/pull/7964
- **Issue #8195 — 模型池没有删除/移除操作** — 小而明确的 UX 缺口；工作量低，但尚无修复 PR。https://github.com/agentscope-ai/QwenPaw/issues/8195

开放 PR 数量（20）相对于合并数（4）呈上升趋势，这与持续数月的“Under Review”事项相结合，表明**审查带宽是主要瓶颈**，而非贡献供给。

</details>

<details>
<summary><strong>ZeroClaw</strong> — <a href="https://github.com/zeroclaw-labs/zeroclaw">zeroclaw-labs/zeroclaw</a></summary>

# ZeroClaw 项目摘要 — 2026-10-13

## 1. 今日概览

ZeroClaw 仍处于高活跃度的预发布阶段：过去 24 小时内**触及 33 个 issue 和 50 个 PR**，但**没有发布任何新版本**。积压严重偏向未关闭——**33 个活跃 issue 中有 29 个**、**50 个 PR 中有 44 个仍处于打开状态**——表明相对于合并吞吐量而言，新增工作相当可观。活动集中在四个反复出现的主题：**持久化记忆**、**A2A 协议**、**插件/WASM 通道栈**以及 **ZeroCode TUI**。2026-10-13 关闭了三个 issue（浏览器/搜索工具语义 bug、插件通道实例种子数据、死 worker 结算恢复），但打开与关闭的比例表明，**当前瓶颈是评审/合并带宽，而非贡献者供给**。值得注意的是，今天最新提交的若干 bug 报告中，已有同日打开的配套修复 PR，这是响应能力健康的信号。

## 2. 发布

报告窗口内没有新版本发布。（按说明**省略**——没有需要报告的更新日志、破坏性变更或迁移说明。）

两个活跃的、由发布门控的跟踪项仍是路线图锚点：[#7432 Runtime and gateway delivery — v0.8.6 and v0.9.0](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) 以及标记为 `release:v0.8.6` 的插件相关工作（[#10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999)、[PR #11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262)、[PR #11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261)）。

## 3. 项目进展

**已关闭/已合并事项（窗口内关闭 4 个 issue；可见 3 个）：**

| 事项 | 类型 | 结果 |
|---|---|---|
| [#11108](https://github.com/zeroclaw-labs/zeroclaw/issues/11108) | Bug（工具） | **已关闭**——`map_tool_name_alias()` 不再将 `browser_open`/`browser`/`web_search` 重写为 `shell`，保留内置工具语义 |
| [#10996](https://github.com/zeroclaw-labs/zeroclaw/issues/10996) | 功能（插件） | **已关闭**——插件安装期间播种通道实例配置/授权 |
| [#10331](https://github.com/zeroclaw-labs/zeroclaw/issues/10331) | Bug（运行时/守护进程） | **已关闭**——恢复死 worker 遗弃的终端结算意图 |
| （第 4 个已关闭 issue） | — | 不在前 30 条样本中 |

**PR 活动（6 个已合并/已关闭；前 20 条样本中不可见，前 20 条全部为 OPEN）：** 可见 PR 集合主要由推进现有 epic 的修复和后续工作组成，而非新子系统。今天（2026-10-13）新打开/首次出现的亮点：

- [PR #11698](https://github.com/zeroclaw-labs/zeroclaw/pull/11698) —— `fix(runtime)`：按会话快照 personality 文件，使编辑不再击穿提示缓存（解决 [#11696](https://github.com/zeroclaw-labs/zeroclaw/issues/11696)）。
- [PR #11693](https://github.com/zeroclaw-labs/zeroclaw/pull/11693) —— `fix(tools)`：让 `model_routing_config upsert_agent` 保留被省略字段和 profile 引用（解决 [#11606](https://github.com/zeroclaw-labs/zeroclaw/issues/11606)）。
- [PR #11695](https://github.com/zeroclaw-labs/zeroclaw/pull/11695) —— `fix(zerocode)`：将 Cost 标签页的 “By Period” 合计对齐到 UTC（解决 [#11694](https://github.com/zeroclaw-labs/zeroclaw/issues/11694)）。
- [PR #11624](https://github.com/zeroclaw-labs/zeroclaw/pull/11624) —— `fix(zerocode)`：应答被丢弃的 elicitation 并记录它们（解决 [#11623](https://github.com/zeroclaw-labs/zeroclaw/issues/11623)）。
- [PR #11700](https://github.com/zeroclaw-labs/zeroclaw/pull/11700) —— `fix(zerocode)`：让 composer/completion 的按键不进入 transcript。

**Epic 进展信号：** @IftekharUddin 的插件栈持续成熟（[PR #11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261)、[#11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262)、[#11228](https://github.com/zeroclaw-labs/zeroclaw/pull/11228)），ZeroCode UX/上下文工作也在推进（[PR #10905](https://github.com/zeroclaw-labs/zeroclaw/pull/10905) 手动上下文压缩、[#11622](https://github.com/zeroclaw-labs/zeroclaw/pull/11622) transcript 时间戳）。

## 4. 社区热门话题

按评论量排序：

1. **[#8692 — Maintainer decision queue for RFCs and design issues](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)** —— 15 条评论。讨论最多的单项。*底层需求：* 为 RFC 建立透明、按优先级排序的决策流水线；设计 RFC 的数量正超过维护者决策速度。
2. **[#8891 — Persistent memory: wire curation/relevance/operability planes to parity](https://github.com/zeroclaw-labs/zeroclaw/issues/8891)** —— 11 条评论。跟踪 7 个打开事项（4 个 issue、3 个 PR）。*需求：* 与成熟同类运行时实现跨会话记忆对齐。
3. **[#11254 — RFC: A2A protocol crate (zeroclaw-a2a)](https://github.com/zeroclaw-labs/zeroclaw/issues/11254)** —— 9 条评论。*需求：* 一等公民的 agent-to-agent 线格式与发现界面，基于 #9106/#7763。
4. **[#11420 — SQLite session backend rewrites `created_at`](https://github.com/zeroclaw-labs/zeroclaw/issues/11420)** —— 7 条评论，`priority:p1`。高参与度的正确性 bug。
5. **[#7432 — Runtime & gateway delivery tracker (v0.8.6/v0.9.0)](https://github.com/zeroclaw-labs/zeroclaw/issues/7432)** —— 6 条评论。发布关键协调项。
6. **[#11235 — RFC: Knowledge corpus (RAG)](https://github.com/zeroclaw-labs/zeroclaw/issues/11235)** —— 6 条评论。*需求：* 运营方自有的文档检索。
7. **[#11074 — `[[search_routes]]` provider routing for web_search_tool](https://github.com/zeroclaw-labs/zeroclaw/issues/11074)** —— 5 条评论。*需求：* 按查询选择 provider，以提升来源质量/交叉验证。
8. **[#10025 — RFC: zeroclaw swarm (ephemeral agent swarms + TUI)](https://github.com/zeroclaw-labs/zeroclaw/issues/10025)** 和 **[#10573 — Bind pairing tokens to roster users](https://github.com/zeroclaw-labs/zeroclaw/issues/10573)** —— 各 4 条评论。

**分析：** 社区需求集中在**多代理编排、持久记忆/RAG 和按身份限定的访问**——也就是说，用户正推动 ZeroClaw 从单助手工具走向协调的多租户代理平台。

## 5. Bug 与稳定性

按严重性/优先级排序：

| 严重性 | 事项 | 状态 | 修复 PR？ |
|---|---|---|---|
| **S1 – 工作流受阻** | [#11648](https://github.com/zeroclaw-labs/zeroclaw/issues/11648) Web dashboard 配对输入上限为 6 位数字，而后端签发 32 字符字母数字代码 | OPEN | 未标明 |
| **p1** | [#11420](https://github.com/zeroclaw-labs/zeroclaw/issues/11420) SQLite 每轮为每条消息重写 `created_at`（逐消息时间丢失） | OPEN | 未标明 |
| **p1 / 高风险** | [#11606](https://github.com/zeroclaw-labs/zeroclaw/issues/11606) `model_routing_config upsert_agent` 重写整个配置，伪造 profile，丢弃字段，重置限制 | OPEN，进行中 | ✅ [PR #11693](https://github.com/zeroclaw-labs/zeroclaw/pull/11693) |
| **S2** | [#11655](https://github.com/zeroclaw-labs/zeroclaw/issues/11655) 当 recall 没有向量分数时，核心冲突取代会隐藏不相关记忆 | OPEN | 未标明 |
| **S2（成本）** | [#11696](https://github.com/zeroclaw-labs/zeroclaw/issues/11696) 系统提示每轮从磁盘重建；personality 编辑会击穿所有活跃会话的缓存 | OPEN | ✅ [PR #11698](https://github.com/zeroclaw-labs/zeroclaw/pull/11698) + [#11702](https://github.com/zeroclaw-labs/zeroclaw/pull/11702) |
| **S2** | [#11623](https://github.com/zeroclaw-labs/zeroclaw/issues/11623) ZeroCode 丢弃待处理的 `ask_user`/`poll`，留下 600s 超时且无记录 | OPEN | ✅ [PR #11624](https://github.com/zeroclaw-labs/zeroclaw/pull/11624) |
| **S3** | [#11658](https://github.com/zeroclaw-labs/zeroclaw/issues/11658) 回复后 Web 聊天输入未重新获得焦点 | OPEN | 未标明 |
| **S3** | [#11694](https://github.com/zeroclaw-labs/zeroclaw/issues/11694) Cost 标签页混合 UTC 和本地日合计且未标注 | OPEN | ✅ [PR #11695](https://github.com/zeroclaw-labs/zeroclaw/pull/11695) |
| **S3** | [#11691](https://github.com/zeroclaw-labs/zeroclaw/issues/11691) 暂停队列提示显示 ⌥P，在 macOS Terminal 中会输入 π | OPEN | 相关 [PR #11700](https://github.com/zeroclaw-labs/zeroclaw/pull/11700)（按键处理） |

**评估：** 未报告崩溃；问题属于正确性/UX/成本回退，而非硬故障。积极信号：**今天最新 bug 中有三个已有同日修复 PR**，并且一个长期存在的工具语义 bug（[#11108](https://github.com/zeroclaw-labs/zeroclaw/issues/11108)）已关闭。p1 SQLite 时间戳 bug（[#11420](https://github.com/zeroclaw-labs/zeroclaw/issues/11420)）是最显著的*未处理*稳定性事项。

## 6. 功能请求与路线图信号

**最可能下一步落地（与 v0.8.6/v0.9.0 跟踪项绑定或已在 PR 中）：**
- **插件系统加固/完成** —— 通道实例播种（[#10996](https://github.com/zeroclaw-labs/zeroclaw/issues/10996)，已关闭）、Discord 插件验证（[#10999](https://github.com/zeroclaw-labs/zeroclaw/issues/10999)）、分阶段包替换（[PR #11261](https://github.com/zeroclaw-labs/zeroclaw/pull/11261)）、`plugin update` CLI（[PR #11262](https://github.com/zeroclaw-labs/zeroclaw/pull/11262)）、TLS profile 加固（[PR #11228](https://github.com/zeroclaw-labs/zeroclaw/pull/11228)）。
- **网关分离/安全** —— 可配置 `MAX_BODY_SIZE`（[#11668](https://github.com/zeroclaw-labs/zeroclaw/issues/11668)）、配对令牌到 roster 的绑定（[#10573](https://github.com/zeroclaw-labs/zeroclaw/issues/10573)）、按作用域 principal 的日志访问（[PR #11326](https://github.com/zeroclaw-labs/zeroclaw/pull/11326)）。
- **ZeroCode UX** —— 上下文压缩（[PR #10905](https://github.com/zeroclaw-labs/zeroclaw/pull/10905)）、transcript 时间戳（[PR #11622](https://github.com/zeroclaw-labs/zeroclaw/pull/11622)）、可拖动会话行（[PR #11573](https://github.com/zeroclaw-labs/zeroclaw/pull/11573)）。

**中期/RFC 阶段（需通过 [#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) 由维护者决策）：**
- **A2A 协议 crate**（[#11254](https://github.com/zeroclaw-labs/zeroclaw/issues/11254)）—— 横切架构 RFC。
- **知识语料库/RAG**（[#11235](https://github.com/zeroclaw-labs/zeroclaw/issues/11235)）—— 需要作者行动。
- **临时代理集群**（[#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025)）和 **Level 5 “Dark Factory” 工作流**（[#11664](https://github.com/zeroclaw-labs/zeroclaw/issues/11664)）—— 用户对编排有强烈兴趣。
- **将 Channels 从 Core 中分离**（[#11662](https://github.com/zeroclaw-labs/zeroclaw/issues/11662)）—— 降低死重/攻击面。
- **持久化 peer-agent 轮次**（[#9597](https://github.com/zeroclaw-labs/zeroclaw/issues/9597)）、**search_routes**（[#11074](https://github.com/zeroclaw-labs/zeroclaw/issues/11074)）、**turn-consolidation 目标类别**（[#11657](https://github.com/zeroclaw-labs/zeroclaw/issues/11657)）。

**预测：** 下一个版本（v0.8.6）最可能交付**插件安装/更新加固、网关安全作用域，以及 ZeroCode 上下文/UX 修复**。A2A、swarm 和 RAG 仍是 v0.9.0 之后的架构押注。

## 7. 用户反馈摘要

- **多代理编排是主要未满足需求。** [#11664](https://github.com/zeroclaw-labs/zeroclaw/issues/11664)（“Level 5 Dark Factory Workflow”）、[#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025)（swarm TUI）和 [#11254](https://github.com/zeroclaw-labs/zeroclaw/issues/11254)（A2A）都表达了同一痛点：*“搭建一组代理需要配置手术……没有编排器。”*
- **“不要交付我用不到的东西。”** [#11662](https://github.com/zeroclaw-labs/zeroclaw/issues/11662) 称未使用的通道是“死重和安全风险”——用户希望模块化通道分离。
- **多代理中继摩擦真实存在。** [#11668](https://github.com/zeroclaw-labs/zeroclaw/issues/11668) 报告，长上下文 A2A `message/send` 载荷在 <1s 内被网关 body 限制拒绝，并返回不透明字符串 `length limit exceeded`——这是一个具体生产用例（前台 → 专家代理）。
- **运营方 UX 打磨很重要。** 浏览器/搜索工具被静默重写为 `shell`（[#11108](https://github.com/zeroclaw-labs/zeroclaw/issues/11108)）、32 字符代码会破坏配对对话框（[#11648](https://github.com/zeroclaw-labs/zeroclaw/issues/11648)）、回复后焦点丢失（[#11658](https://github.com/zeroclaw-labs/zeroclaw/issues/11658)）以及成本周期误标（[#11694](https://github.com/zeroclaw-labs/zeroclaw/issues/11694)）表明用户正在真实多代理场景中运行 ZeroClaw，并遇到人体工学边界问题。
- **满意度信号：** 多项 bug→修复当日快速周转，表明维护者团队投入度高；不满集中在**评审延迟和不清晰的 RFC 决策**，而非能力。

## 8. 积压事项观察

近期有更新但长期打开、需要维护者关注的事项（年龄 = 从创建到 2026-10-13）：

| 事项 | 年龄 | 为何需要关注 |
|---|---|---|
| [#7432](https://github.com/zeroclaw-labs/zeroclaw/issues/7432) 运行时与网关交付跟踪项（v0.8.6/v0.9.0） | 约 126 天 | 发布关键的事实来源；仍打开 |
| [#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692) 维护者决策队列 | 约 101 天 | 评论最多的事项；RFC 决策瓶颈 |
| [#8891](https://github.com/zeroclaw-labs/zeroclaw/issues/8891) 持久化记忆对齐跟踪项 | 约 96 天 | 7 个打开子项；高严重性依赖 |
| [#10025](https://github.com/zeroclaw-labs/zeroclaw/issues/10025) zeroclaw swarm RFC | 约 58 天，`needs-author-action` | 受欢迎的编排 RFC，等待输入 |
| [#9597](https://github.com/zeroclaw-labs/zeroclaw/issues/9597) 持久/可归属的 peer-agent 轮次 | 约 74 天，`in-progress` | 阻碍可靠多代理生命周期 |
| [#10290](https://github.com/zeroclaw-labs/zeroclaw/issues/10290) 将实时配置句柄贯穿到分离轮次 | 约 50 天 | 安全相关后续（SSRF 策略传播） |
| [PR #10499](https://github.com/zeroclaw-labs/zeroclaw/pull/10499) 验证持久化配置写入 | 约 43 天，`needs-author-action`，size:XL | 高风险配置正确性 |
| [PR #10446](https://github.com/zeroclaw-labs/zeroclaw/pull/10446) 拒绝泄漏到散文中的工具调用信封 | 约 45 天，`needs-maintainer-review` | 在 `gpt-5.6`/codex 上生产观察到的 bug |
| [PR #10502](https://github.com/zeroclaw-labs/zeroclaw/pull/10502) 迁移工具结果图像（OpenAI 兼容 400） | 约 43 天，`needs-author-action` | 阻碍 OpenAI 兼容 provider |
| [PR #10905](https://github.com/zeroclaw-labs/zeroclaw/pull/10905) 手动可恢复上下文压缩 | 约 27 天，size:XL | 高价值 ZeroCode 功能卡在评审中 |

**健康度解读：** 项目拥有强劲的贡献者动能和清晰的路线图主干（记忆 → 插件 → 网关分离 → A2A/swarm）。主要风险是**决策与评审延迟**：三个无 stale 状态的跟踪项以及若干 `needs-author-action`/`needs-maintainer-review` PR 正在累积。清理维护者决策队列（[#8692](https://github.com/zeroclaw-labs/zeroclaw/issues/8692)）并解除 v0.8.6 插件/网关 PR 栈的阻塞，最能改善合并吞吐量。

---
*数据说明：源样本中 PR 评论数未填充（`undefined`），因此此处 PR 排名反映的是时效性和状态，而非评论量。4 个已关闭 issue 中只有 3 个、6 个已合并/已关闭 PR 中没有一个出现在前 30/前 20 条样本中；其余事项的详情不可用。*

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*