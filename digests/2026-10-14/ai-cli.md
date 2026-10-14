# AI CLI 工具社区动态日报 2026-10-14

> 生成时间: 2026-10-14 01:21 UTC | 覆盖工具: 7 个

- [Claude Code](https://github.com/anthropics/claude-code)
- [OpenAI Codex](https://github.com/openai/codex)
- [Gemini CLI](https://github.com/google-gemini/gemini-cli)
- [GitHub Copilot CLI](https://github.com/github/copilot-cli)
- [OpenCode](https://github.com/anomalyco/opencode)
- [Pi](https://github.com/earendil-works/pi)
- [Qwen Code](https://github.com/QwenLM/qwen-code)
- [Claude Code Skills](https://github.com/anthropics/skills)

---

## 横向对比

# 跨工具 AI CLI 生态报告 — 2026-10-14

> **数据说明：** 摘要提供的是“热门/精选”条目，而非仓库全量统计。下文的计数是摘要中可见的计数。**N/A** 表示未提供讨论数据或该渠道未被使用；并不一定意味着零活动。

---

## 1. 生态概览

AI CLI 生态正围绕三个难题收敛：**agent 成本/限额执行**、**长会话持久性**和**跨界面可靠性**。部分工具的发布节奏仍然很快——Codex、Gemini CLI、Copilot CLI 和 Qwen Code 都发布了构建版本——而 Claude Code、OpenCode 和 Pi 在该窗口内没有发布。Issue 跟踪器显示，用户越来越把这些工具视为生产自动化界面，而非聊天玩具：他们要求支出上限、可安全重启的会话、子 agent 可观测性、沙箱清晰性，以及可预测的 MCP/插件行为。与此同时，provider/model 灵活性——BYOK、兼容 OpenAI 的后端、Bedrock/Vertex 路由，以及快速的模型 API 漂移——已成为主要差异点和痛点。Windows、IDE 一致性和终端渲染在原本成熟的产品中仍是持续摩擦点。

---

## 2. 活动对比

| 工具 | Issues 出现数 | PR 出现数 | 讨论出现数 | 发布状态 |
|---|---:|---:|---:|---|
| **Claude Code** | 10 个热门；issue 流量很高 | 2 | N/A | 过去 24 小时无发布 |
| **OpenAI Codex** | 10 个热门 | 10 个精选 | 7 | 3 个 Rust alpha：`0.163.0-alpha.8/.9/.10` |
| **Gemini CLI** | 10 个热门 + 亦活跃 | 10 个关键 + 7 个关注列表 | N/A | `v0.64.0`；`v0.65.0-preview.0`；nightly |
| **GitHub Copilot CLI** | 10 个热门 | 0 个更新 | N/A | `v1.0.98-1`；`v1.0.98-0`；`v1.0.97`；`v1.0.97-3` |
| **OpenCode** | 10 个热门 | 10 个关键 + 2 个值得注意 | N/A | 过去 24 小时无发布 |
| **Pi** | 10 个热门 | 10 个关键 | N/A | 过去 24 小时无发布 |
| **Qwen Code** | 10 个热门 + 荣誉提名 | 10 个关键 + 6 个值得注意 | N/A | `v0.25.2-preview.0`；`v0.25.1-nightly` |

**备注：**
- Claude Code 仍由 issue 驱动；外部 PR 量极少。
- Codex 是这一组中讨论面最丰富的，有 Show-and-tell 和 Q&A 活动。
- Copilot CLI 发布了多个补丁版本，但在该窗口内没有 PR 更新。
- Gemini CLI、OpenCode、Pi 和 Qwen Code 都显示出活跃的、基于 PR 的开发。

---

## 3. 共同功能方向

### 成本与 token 治理
- **Claude Code：** 每会话支出上限、后台子 agent 的 token/轮次/时间上限、编排器并发限制。
- **OpenCode：** 可配置的 prompt 缓存 TTL、重试/退避策略、成本/延迟治理。
- **Codex：** token 回执工具、重置提醒、成本估算社区项目。
- **Pi：** 保留缓存的 reasoning-effort 变更、压缩 token 上限可靠性。
- **Gemini CLI / Qwen Code：** 节省 token 的上下文访问、响应式压缩正确性、工具加载成本评估。

**共同需求：** 从被动应对成本意外，转向可强制执行的预算、缓存控制和可见的支出去向归因。

### 会话持久性、恢复与压缩
- **Claude Code：** 计划任务静默失败、每日重新登录、陈旧的云端分支。
- **Codex：** 流断开、远程压缩失败、跨设备状态同步、队列管理。
- **OpenCode：** V2 迁移隐藏会话、重连失败、压缩后工具失忆。
- **Pi：** 恢复的会话丢失动态激活的工具、压缩摘要上限。
- **Qwen Code：** 托管会话接管丢失、harness 死亡后唤醒输入未执行。
- **Copilot CLI：** 会话重命名/退出不一致、CLI/桌面端同步缺口。

**共同需求：** 会话必须能在重启、网络中断、压缩、升级和多界面交接后存活，且不发生静默状态丢失。

### 子 agent/编排可靠性与可观测性
- **Claude Code：** 未设上限的后台子 agent、编排器忽略项目限制、并行 agent 导致 VS Code OOM。
- **Gemini CLI：** 子 agent 挂起、MAX_TURNS 被误报为成功、browser-agent 设置被忽略。
- **Pi：** 工具状态恢复和畸形工具调用处理。
- **Qwen Code：** 多 agent 持久性、子结果恢复、broker 接管。
- **Codex：** Guardian 审查处理、异步压缩、daemon 重启行为。

**共同需求：** 可执行的并发限制、准确的终止报告、审批门禁，以及可审计的子 agent 操作。

### IDE、操作系统与跨界面一致性
- **Claude Code：** Zed 集成、VS Code 聊天语法高亮、Windows 安装程序、Linux 桌面认证。
- **Codex：** Windows 桌面版、VS Code 沙箱、iOS 远程项目。
- **Copilot CLI：** Windows MCP OAuth 循环、企业策略摩擦、CLI/桌面端同步。
- **OpenCode：** Windows 桌面终端、V2 非 git 会话可见性。
- **Gemini CLI：** Wayland 浏览器子 agent 失败、tmux 渲染。
- **Pi：** TUI Home/End、超链接、tmux 图片、rxvt-unicode 滚轮行为。

**共同需求：** 用户期望在终端、IDE、桌面端、Web 和移动端获得相同的核心体验。

### 沙箱、权限与安全
- **Claude Code：** 安全过滤器对合法安全工作的误报。
- **Gemini CLI：** 只读不可信工作区、操作系统沙箱、最小权限默认值。
- **Codex：** 沙箱/审批透明度、按项目网络访问、陈旧锁。
- **Copilot CLI：** 文件范围写权限、过度强势的提示。
- **Qwen Code：** provider 错误诊断、托管 agent 隔离。
- **Pi：** OAuth 身份持久性、无副作用的 SDK 注册。

**共同需求：** 安全控制必须可解释、有范围限制，且不能阻挡合法的逆向工程、安全研究或本地开发。

### Provider/model 兼容性与 BYOK
- **Copilot CLI：** BYOK 模型发现、短名称匹配、按 agent 的默认模型、采样默认值。
- **OpenCode：** GitHub Copilot provider、Bedrock 区域目录、兼容 OpenAI 的 Vertex。
- **Pi：** provider 路由、模型别名、Gemini 4 思考控制、GPT-6 reasoning-effort 缓存。
- **Qwen Code：** vLLM/SGLang 工具序列化和上下文溢出检测。
- **Codex：** 面向 ChatGPT 用户的模型目录/认证一致性。
- **Gemini CLI / Copilot CLI：** Gemini 4 Argon 支持进行中/已发布。

**共同需求：** 在快速变化的模型 API 之上提供稳定抽象，并具备清晰的认证、目录发现和后端特定行为。

### MCP/插件扩展性
- **Claude Code：** Mods/hooks/plugins 大型讨论帖、WebView/GUI 市场请求。
- **Codex：** MCP/连接器、浏览器/computer-use 稳健性、插件/技能元数据。
- **Copilot CLI：** 插件 hooks、MCP 状态同步、OAuth 初始化。
- **OpenCode：** 插件权限断言、远程 Figma MCP、web 搜索插件。
- **Qwen Code：** ACP 延迟 MCP 工具、MCP 服务器生命周期韧性。
- **Pi：** provider 扩展、MCP 客户端识别。

**共同需求：** MCP 和插件正成为一等集成路径，但认证、发现、状态和 hook 可靠性仍不成熟。

---

## 4. 差异化分析

| 工具 | 功能重点 | 目标用户 | 技术路径 |
|---|---|---|---|
| **Claude Code** | 通过 Mods 实现扩展性、成本治理、IDE 一致性、安全过滤器行为 | 以 Anthropic 为中心的专业开发者、安全工程师、团队 | TUI + IDE 扩展、hooks/plugins、后台子 agent；issue 驱动路线图 |
| **OpenAI Codex** | Windows 桌面版、Computer Use/浏览器/MCP、队列管理、跨设备同步 | OpenAI/ChatGPT 用户、Windows 桌面和 VS Code 用户 | Rust CLI/runtime、app-server daemon、Guardian 审查、异步压缩 |
| **Gemini CLI** | Google 模型集成、子 agent 可靠性、终端渲染、沙箱 | Google Cloud/Vertex 用户、开源贡献者、Linux/tmux 用户 | Node/TypeScript CLI、ACP/A2A 服务器、浏览器 agent、操作系统沙箱、PR 驱动 |
| **GitHub Copilot CLI** | 企业 GitHub 集成、BYOK、MCP/插件 hooks、Windows 认证 | GitHub 企业用户、Copilot 订阅者、BYOK/自定义 provider 用户 | CLI + ACP、插件 hooks、MCP、频繁补丁发布 |
| **OpenCode** | V2 稳定化、provider 广度、插件权限、流式性能 | 开源/自托管用户、多 provider 开发者 | 服务器/客户端架构、TUI/桌面端、托管配置、V2 API 迁移 |
| **Pi** | Provider/model 路由、会话/工具状态持久性、TUI 打磨、Nix 打包 | 终端高级用户、多 provider/Nix 用户、嵌入方 | Provider 抽象、SDK、TUI、导出/Web UI 实验 |
| **Qwen Code** | 托管 managed-agent 持久性、Java SDK、多 agent 协调 | 企业/托管 agent 用户、Java/自托管后端 | Runtime Broker、MySQL 持久化、ACP、managed-agent 控制平面 |

**关键差异点：**
- **Claude Code** 最偏向扩展性和治理，但其外部 PR 文化较弱。
- **Codex** 正在推进前沿的桌面/浏览器/Computer Use 功能，Windows 是其最大短板。
- **Gemini CLI** 在开源 PR 吞吐量和 Google 模型集成方面最强，但子 agent 挂起和 TUI 渲染问题反复出现。
- **Copilot CLI** 以企业和 GitHub 为先，且重度依赖 BYOK；其 Windows/MCP 认证问题是主要拖累。
- **OpenCode** 正处于 V2 过渡期，主要押注是 provider 广度和插件权限化。
- **Pi** 是面向高级用户/多 provider 的工具，强项是 provider 路由，同时 TUI 问题突出。
- **Qwen Code** 最聚焦企业托管 agent，持久性和 provider 兼容性是首要问题。

---

## 5. 社区势头与成熟度

### 最高活跃度社区
- **Claude Code：** 该组中互动最高的 issue（`#91870`，256 条评论；`#18435`，895 个赞）。信号强，但外部 PR 贡献稀疏。
- **OpenAI Codex：** issue 互动高（`#49458`，79 条评论）、PR 批次活跃，并且是唯一拥有丰富 Discussions 界面的项目。alpha 节奏快。
- **Gemini CLI：** PR 量高、发布稳定，围绕子 agent 和渲染的 issue 变动活跃。开源贡献模式强。
- **OpenCode：** issue/PR 流非常活跃，尤其是 V2 稳定化；免费模型配额讨论帖评论很多（`#15585`，56 条评论）。
- **Qwen Code：** 聚焦但活跃；managed-agent 持久性和 provider 兼容性推动了 P1/P2 issue 和 PR。

### 更成熟但 PR 节奏较低
- **GitHub Copilot CLI：** 发布频繁且 issue 跟踪器活跃，但该窗口内没有 PR 更新。成熟度体现在发布工程上；社区贡献似乎更偏 issue 驱动。
- **Pi：** 社区较小，但在 issue 和 PR 上都很活跃。在 provider/model 路由和 TUI 边缘情况方面技术深度强。

### 势头信号
- **快速迭代：** Codex（三个 alpha）、Gemini CLI（preview + nightly + stable）、Copilot CLI（四个补丁版本）、Qwen Code（preview + nightly）。
- **稳定化模式：** OpenCode 明显处于 V2 稳定化阶段；Pi 正在加固会话/工具状态。
- **信任敏感阶段：** 配额模糊、静默失败和成本超支越来越被视为信任问题，而不只是 bug。

---

## 6. 趋势信号

### 1. 成本控制正成为核心产品功能
在 Claude Code、Codex、OpenCode、Pi 和 Qwen Code 中，用户都想要可强制执行的预算、缓存 TTL 控制、token 回执和审批门禁。率先提供一等支出治理的工具将赢得生产信任。

### 2. 长运行会话必须可安全重启且可安全压缩
会话丢失、工具状态失忆、流断开和压缩失败出现在每个主要工具中。下一个竞争基线是跨 CLI、IDE、桌面端和移动端的**持久、可恢复、可观测会话**。

### 3. 子 agent 编排需要运行时执行
并发上限、token/轮次/时间限制、准确的终止原因和子 agent 跟踪是反复出现的诉求。能够忽略书面限制的编排器是生产风险。

### 4. BYOK 和 provider 抽象已是基本门槛
Copilot CLI、OpenCode、Pi 和 Qwen Code 都显示出明显的 BYOK/自定义 provider 痛点。开发者想要模型发现、别名、按 agent 的默认值、正确采样和后端特定错误处理。

### 5. MCP/插件生态正在成熟但仍脆弱
插件 hooks、MCP 认证、工具发现和上下文合并在 Claude Code、Codex、Copilot CLI、OpenCode 和 Qwen Code 中都很活跃。生态需要更清晰的状态、重试和权限模型。

### 6. Windows 和非 VS Code 界面仍未被充分服务
Windows 桌面版、Zed、Wayland、tmux 和 Linux 认证问题反复出现。跨平台一致性现在是基线预期，而不是差异点。

### 7. 安全控制不得阻挡合法工作
安全研究中的安全过滤器误报、沙箱困惑和不透明策略阻断反复出现。制胜姿态是**有范围、可解释、可在审计下绕过**的安全——而非一刀切拒绝。

### 8. 终端 UX 仍然重要
闪烁、边框溢出、Home/End 行为、超链接和调整大小处理仍是高频投诉。即使是高级 agent 用户，大部分时间也花在终端里。

### 对开发者的参考价值
- **如果构建 agent 工具：** 优先考虑成本上限、会话持久性和子 agent 可观测性。
- **如果集成 provider：** 假设模型 API 会漂移，并从第一天起就为 BYOK/自定义后端设计。
- **如果发布插件/MCP：** 尽早投入认证、发现、状态和 hook 可靠性。
- **如果面向企业：** Windows、策略错误、托管配置和可审计性不可妥协。
- **如果面向高级用户：** TUI 打磨、provider 路由和跨界面连续性驱动留存。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills 社区亮点 — 截至 2026-10-14

> 数据说明：所提供数据集中 PR 评论数为 `undefined`。下方 PR 排名沿用仓库“按评论数排序”的前 20 顺序；所列 PR 当前均为 **OPEN**。

## 1. 热门 Skills 排名 —— 最受关注的 PR

1. **[mcp-builder：支持 mcp>=2 streamable_http_client 导入与自定义 headers](https://github.com/anthropics/skills/pull/1742)** — OPEN  
   修复 `mcp-builder` 中的 MCP v2 兼容性：`streamablehttp_client` 重命名，以及通过 `create_mcp_http_client` / `http_client` 支持自定义 headers。关联 issue #1668。高影响的核心 Skill 维护。

2. **[skill-creator：隔离触发器评估并处理 Windows 与运行时故障](https://github.com/anthropics/skills/pull/1298)** — OPEN  
   解决触发器评估中的漏报/无效分数、Windows 子进程管道问题，以及运行时故障错误通过负例的问题。直接强化 Skill 编写闭环。

3. **[proofcore-contract-auditor：用于智能合约公证](https://github.com/anthropics/skills/pull/1771)** — OPEN  
   新的 Web3 Skill，用于 Solidity/Rust 合约静态分析，并通过 Merkle 协议在 TON 上锚定加密审计证明。

4. **[检测孤立 docx 评论](https://github.com/anthropics/skills/pull/1734)** — OPEN  
   通过检测孤立评论改进 `docx` Skill。摘要很简短，但属于活跃的文档处理集群。

5. **[新增 md2video-audio skill](https://github.com/anthropics/skills/pull/1703)** — OPEN  
   新的零成本 Skill，使用 Marp 及相关工具将 Markdown 编译为带旁白的 MP4 视频。

6. **[新增 notion-spec-to-implementation 和 quantitative-resume-auditor skills](https://github.com/anthropics/skills/pull/1245)** — OPEN  
   新的生产力 Skills：一个将 Notion 规格转化为带验收标准的实现任务；另一个定量审计简历。

7. **[skill-creator：加固 eval viewer — 脚本逃逸、DNS rebinding、跨站 POST、转义](https://github.com/anthropics/skills/pull/1961)** — OPEN  
   针对 `skill-creator` 使用的本地 eval viewer 进行安全加固。重要，因为该 viewer 会渲染不可信的、由模型编写的评估输出。

8. **[docx：将 LibreOffice 超时报告为错误并验证输出](https://github.com/anthropics/skills/pull/1792)** — OPEN  
   防止 `soffice` 超时后误报成功，并验证修订标记（`w:ins` / `w:del` / `w:moveFrom` / `w:moveTo`）确实已被移除。

## 2. 社区需求趋势 —— 来自 Issues

评论最多的 Issues 显示，需求集中在**平台信任、Skill 可靠性和企业就绪**，以及若干特定领域的自动化方向。

- **安全、信任边界与治理**  
  [#492 — `anthropic/` 命名空间下的社区 Skills 会导致信任边界滥用](https://github.com/anthropics/skills/issues/492)（46 条评论）是迄今为止讨论最多的 issue。相关：[#1394 eval-viewer XSS](https://github.com/anthropics/skills/issues/1394)、[#1175 SharePoint 访问控制担忧](https://github.com/anthropics/skills/issues/1175)、[#412 agent 治理提案](https://github.com/anthropics/skills/issues/412)。

- **Skill 生命周期可靠性：评估、触发器、基准测试、打包**  
  [#556 run_eval.py 从不触发 skills/commands](https://github.com/anthropics/skills/issues/556)（12 条评论）、[#1383 静默基准测试失败](https://github.com/anthropics/skills/issues/1383)、[#1352 并行 worker 交叉匹配 skill UUID](https://github.com/anthropics/skills/issues/1352)、[#1390 mcp-builder 评估得分为 0/N](https://github.com/anthropics/skills/issues/1390)、[#202 skill-creator 最佳实践更新](https://github.com/anthropics/skills/issues/202)。

- **企业共享与管理**  
  [#228 在 Claude.ai 中启用组织级 skill 共享](https://github.com/anthropics/skills/issues/228)（16 条评论，8 👍）请求提供共享 Skill 库和直接共享。[#189 来自 `document-skills` 和 `example-skills` 的重复 skills](https://github.com/anthropics/skills/issues/189)（6 条评论）凸显了打包/重叠问题。

- **上下文效率与记忆压缩**  
  [#1487 `claude-api` 在一次工具调用中注入约 156k tokens](https://github.com/anthropics/skills/issues/1487) 和 [#1329 compact-memory 符号表示法](https://github.com/anthropics/skills/issues/1329)（9 条评论）表明需要 token 高效的 Skill 设计和紧凑的 agent 状态。

- **MCP/API 集成正确性**  
  MCP v2 导入/header 修复和评估失败表明，社区希望 Skills 与快速演进的 MCP 和 Claude API 接口保持兼容。

- **领域特定自动化**  
  反复出现的方向包括文档质量/排版、ODT/ODS、DOCX 评论处理、Markdown 转视频、HPC/Slurm 工作流、E2E 浏览器测试、智能合约审计、Notion 规格转任务自动化，以及简历审计。

## 3. 高潜力待合并 Skills

由于 PR 评论数不可用，此选择依据近期更新活动、关联 issue 和可能的生态影响。截至数据快照，所有 PR 均为 OPEN 且尚未合并。

| PR | Skill / 领域 | 为何可能很快落地 |
|---|---|---|
| [#1742](https://github.com/anthropics/skills/pull/1742) | `mcp-builder` MCP v2 兼容性 | 2026-10-08 更新；修复 issue #1668；对 MCP Skills 至关重要。 |
| [#1298](https://github.com/anthropics/skills/pull/1298) | `skill-creator` 触发器评估 | 2026-09-16 更新；修复 Windows/运行时评估失败。 |
| [#1961](https://github.com/anthropics/skills/pull/1961) | `skill-creator` eval-viewer 安全性 | 2026-10-11 更新；安全加固可能优先处理。 |
| [#1997](https://github.com/anthropics/skills/pull/1997) | `skill-creator` YAML frontmatter 解析 | 2026-10-13 更新；针对评估/描述优化的近期正确性修复。 |
| [#1980](https://github.com/anthropics/skills/pull/1980) | `webapp-testing` shell 注入 | 2026-10-06 更新；移除 `shell=True` 命令注入风险。 |
| [#1977](https://github.com/anthropics/skills/pull/1977) | `algorithmic-art` wrapAround | 2026-10-07 更新；修复负值回绕 bug (#1897)。 |
| [#1681](https://github.com/anthropics/skills/pull/1681) | `package_skill.py` 直接执行 | 2026-10-08 更新；改进 Skill 打包/开发者体验。 |
| [#1730](https://github.com/anthropics/skills/pull/1730) | `claude-api` 失效 URL | 2026-10-04 更新；文档正确性，低风险合并。 |
| [#1771](https://github.com/anthropics/skills/pull/1771) | `proofcore-contract-auditor` | 新的 Web3 智能合约审计 Skill；2026-09-16 更新。 |
| [#1703](https://github.com/anthropics/skills/pull/1703) | `md2video-audio` | 新的媒体生成 Skill；2026-09-15 更新。 |
| [#1245](https://github.com/anthropics/skills/pull/1245) | Notion 规格 + 简历审计器 | 新的生产力 Skills；2026-09-30 更新。 |
| [#1615](https://github.com/anthropics/skills/pull/1615) | `scnet-hpc` | 新的 HPC/Slurm Skill；2026-08-24 更新。 |
| [#822](https://github.com/anthropics/skills/pull/822) | AWT E2E 测试 | 新的浏览器测试 Skill；2026-09-19 更新。 |
| [#514](https://github.com/anthropics/skills/pull/514) | `document-typography` | 新的文档质量 Skill；2026-03-13 更新。 |
| [#486](https://github.com/anthropics/skills/pull/486) | ODT/ODS Skill | 新的 OpenDocument Skill；2026-04-14 更新。 |
| [#83](https://github.com/anthropics/skills/pull/83) | skill-quality + skill-security 分析器 | 新的元 Skills；2026-01-07 更新。 |

## 4. Skills 生态洞察

社区最集中的需求并非某个单一领域 Skill，而是一个经过加固的 Skills 平台：安全的信任边界、可靠的触发器/评估基础设施，以及企业级共享能力，领域 Skills 则在此基础上构建。

---

# Claude Code 社区摘要 — 2026-10-14

## 1. 今日亮点

过去 24 小时内没有新版本发布，但 issue 流量依旧很大：**Mods** 可扩展性超级主题帖（#91870）目前已有 256 条评论，团队正在梳理关于新插件/钩子能力的反馈。今日主要社区信号是**失控的 agent 成本**（未设上限的后台子 agent、编排器无视项目限制）以及 **IDE/平台一致性**需求（Zed 集成、VS Code 聊天渲染）。一位报告者提交的大批 `[Bug][cyber]` 安全过滤器误报被集中关闭，大多被标为重复或过期。

## 2. 版本发布

过去 24 小时内无。

## 3. 热门 Issue

1. **[#91870](https://github.com/anthropics/claude-code/issues/91870) — Mods：让 Claude 的可扩展性提升 10 倍** · 256 条评论 · 👍131
   新可扩展性能力（hooks + plugins）的核心讨论帖。维护者在 10 月 1 日发布了社区微更新，确认正在根据反馈积极迭代。这是仓库中互动量最高的 issue，也是追踪 Mods API 方向的最佳位置。

2. **[#18435](https://github.com/anthropics/claude-code/issues/18435) — Claude Desktop 中的多账户管理** · 200 条评论 · 👍895
   本数据集中得票最高的单个未关闭请求。用户希望在工作/个人账户之间切换配置文件而无需重新认证。自 1 月以来一直存在且没有已交付的解决方案——这明确表明账户易用性是顶级缺口。

3. **[#26457](https://github.com/anthropics/claude-code/issues/26457) — Windows 11 重装因签名验证失败** · 31 条评论 · 👍20
   长期存在的安装程序/签名 bug 阻止 Windows 用户升级或重装。严重性高，因为用户会滞留在旧版本上且没有变通办法。

4. **[#32362](https://github.com/anthropics/claude-code/issues/32362) — Zed IDE 集成支持** · 20 条评论 · 👍54
   在 Zed 下 `/ide` 返回“No available IDEs detected”。此处得票第二高的条目；社区越来越期望在 VS Code 和 JetBrains 之外也能获得一致体验。

5. **[#64968](https://github.com/anthropics/claude-code/issues/64968) — VS Code 聊天面板中的语法高亮** · 15 条评论 · 👍37
   无论语言标记或主题如何，围栏代码块都渲染为纯单色文本。一个被重新打开的请求——值得注意的是它此前曾被关闭一次，而用户仍在推动。

6. **[#89812](https://github.com/anthropics/claude-code/issues/89812) — Linux Desktop 强制每日重新登录（`session_stale_relogin`）** · 15 条评论 · 👍5
   提权认证会话大约每天过期，而 Cowork 文件夹授权要求登录两次。可复现、跨多平台，并且会干扰无头/桌面工作流。

7. **[#95966](https://github.com/anthropics/claude-code/issues/95966) — 计划任务静默跳过触发窗口** · 10 条评论
   在一个有记录的 5 天窗口内，任务未能触发，没有错误，也没有运行历史记录，而主机和调度器都明显处于运行状态。自动化基础能力中的静默失败是信任问题。

8. **[#94013](https://github.com/anthropics/claude-code/issues/94013) — 后台子 agent 没有 token/轮次/时间上限** · 5 条评论
   三个后台研究 agent 消耗了 **170 万 token**，没有审批关卡，也没有可见成本。与下文日益增长的成本控制问题群直接相关。

9. **[#101901](https://github.com/anthropics/claude-code/issues/101901) — VS Code 扩展宿主在 2 个以上并行子 agent 时 OOM 崩溃** · 2 条评论（10/13 提交）
   在 Windows 上启动两个并行后台 `Agent` 子 agent 后，扩展宿主会在数秒到一分钟内终止。三次缓解尝试均失败——这是 IDE 中并行 agent 工作流的硬性阻碍。

10. **[#101650](https://github.com/anthropics/claude-code/issues/101650) — 编排器无视书面 agent/批次限制，烧掉约 12% 的每周 Max 额度** · 2 条评论
    一次 Opus 编排会话超过了项目声明的 6 个 agent 并发上限，然后因自身负载导致的超时重试了两个小时，最终没有任何合并。这是迄今最清晰的证据，表明 agent 级护栏并未在运行时得到执行。

## 4. 关键 PR 进展

今日 PR 量极少——仅两项，均已关闭，且都没有引入产品功能：

- **[#44393](https://github.com/anthropics/claude-code/pull/44393) — fix: 在 backfill-duplicate-comments 脚本中遵循 `DAYS_BACK` 环境变量**（已关闭）
  工作流传入了 `DAYS_BACK`，但脚本忽略了它，总是回退到 `MIN/MAX_ISSUE_NUMBER`。修复会计算截止日期并按创建日期过滤，并提前终止分页。仅影响工具链，但它触及了重复评论分诊路径，而今日许多 issue 关闭都经过该路径。

- **[#1](https://github.com/anthropics/claude-code/pull/1) — 创建 SECURITY.md**（已关闭）
  来自 2025 年 2 月的历史性整理 PR，本周期关闭。无功能变更。

> 注意：稀疏的 PR 列表本身也值得指出——这个仓库似乎主要通过 issue 而非外部 PR 来吸收社区贡献，因此 issue tracker 才是真正的产品路线图信号。

## 5. 热门讨论

本周期未提供讨论数据。

## 6. 功能请求趋势

从开放的功能增强 issue 中提炼：

- **可扩展性 / Mods 平台** — 当前最大的活跃主题。请求集中在让 Mods 成为真正可用的插件界面：[#91870](https://github.com/anthropics/claude-code/issues/91870)（核心超级主题帖）、[#99213](https://github.com/anthropics/claude-code/issues/99213)（用于本地页面的 WebView 窗格，无需服务器）以及 [#101910](https://github.com/anthropics/claude-code/issues/101910)（用 Web GUI 替换 TUI 的 Plugins/Marketplace/MCP 编辑器）。
- **成本与 token 治理** — [#95964](https://github.com/anthropics/claude-code/issues/95964) 请求一个在*交互*模式下也生效的每会话支出上限（`--max-budget-usd` 仅适用于打印模式）；[#94013](https://github.com/anthropics/claude-code/issues/94013) 希望对后台子 agent 设置上限和审批关卡；[#101650](https://github.com/anthropics/claude-code/issues/101650) 展示了限制未被执行时的故障模式。
- **IDE 一致性** — [#32362](https://github.com/anthropics/claude-code/issues/32362)（Zed）和 [#64968](https://github.com/anthropics/claude-code/issues/64968)（VS Code 聊天语法高亮）代表了跨编辑器一致体验的需求，而不是只支持 VS Code/JetBrains 的故事。
- **多账户 / 认证易用性** — [#18435](https://github.com/anthropics/claude-code/issues/18435)（配置文件切换）加上 [#89812](https://github.com/anthropics/claude-code/issues/89812)（会话持久化）表明认证是 Desktop 上最大的摩擦点。
- **Desktop 可靠性与自动化正确性** — [#26457](https://github.com/anthropics/claude-code/issues/26457)（Windows 安装程序）、[#95966](https://github.com/anthropics/claude-code/issues/95966)（计划任务）、[#93585](https://github.com/anthropics/claude-code/issues/93585)（云会话检出过期分支）。

## 7. 开发者痛点

- **无上限的自主支出。** 最尖锐且增长最快的挫败点。后台子 agent 在没有 token/轮次/时间上限的情况下运行（[#94013](https://github.com/anthropics/claude-code/issues/94013)），`--max-budget-usd` 不适用于交互式会话（[#95964](https://github.com/anthropics/claude-code/issues/95964)），并且已观察到编排器直接无视项目写入的并发限制（[#101650](https://github.com/anthropics/claude-code/issues/101650)）。用户报告称，在没有任何合并的情况下，花掉了每周计划的相当一部分。
- **合法安全工作中的安全过滤器误报。** 单个报告者的 `[Bug][cyber]` 问题群（[#75210](https://github.com/anthropics/claude-code/issues/75210)、[#75104](https://github.com/anthropics/claude-code/issues/75104)、[#88612](https://github.com/anthropics/claude-code/issues/88612)、[#88613](https://github.com/anthropics/claude-code/issues/88613)、[#75187](https://github.com/anthropics/claude-code/issues/75187) 等）在 10/14 被批量关闭为重复/过期。这种模式——在用户自己的硬件上进行*会话中止*拦截、数据包捕获、内存转储和固件检查——对安全工程师和逆向工程师来说是一个反复出现且可复现的工作流杀手，而重复关闭策略有可能掩盖信号。
- **Desktop 上的认证与会话反复折腾。** Linux 上大约每天强制重新登录（[#89812](https://github.com/anthropics/claude-code/issues/89812)），且不支持多账户（[#18435](https://github.com/anthropics/claude-code/issues/18435)）——895 个赞且仍在增加。
- **静默失败比显式失败更糟。** 从不触发且不记录任何内容的计划任务（[#95966](https://github.com/anthropics/claude-code/issues/95966)），以及在没有明确错误的情况下扣留提示的使用额度门控（[#93886](https://github.com/anthropics/claude-code/issues/93886)），比崩溃更能侵蚀对自动化的信任。
- **IDE 中的 agent 编排不具备负载安全性。** 仅有两个并行子 agent 时，VS Code 扩展宿主就会 OOM 崩溃（[#101901](https://github.com/anthropics/claude-code/issues/101901)），而子 agent 权限提示缺少主 agent 的自动拒绝超时，导致无人值守的 bypass 模式运行无限期停滞（[#99899](https://github.com/anthropics/claude-code/issues/99899)）。
- **平台缺口依然存在。** Windows 安装程序签名验证（[#26457](https://github.com/anthropics/claude-code/issues/26457)）、Web/云会话上的过期分支（[#93585](https://github.com/anthropics/claude-code/issues/93585)），以及充值后信用余额不一致（[#31537](https://github.com/anthropics/claude-code/issues/31537)）。

---
*本摘要根据 2026-10-14 的 github.com/anthropics/claude-code 活动生成。此窗口期内没有可用的新版本或讨论数据。*

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区摘要 — 2026-10-14

## 1. 今日亮点
Codex 发布了三个 Rust alpha 版本（`0.163.0-alpha.8` 至 `.10`），除版本名称外没有实质性发布说明，表明预发布迭代仍在继续。社区关注度仍高度集中在 Windows 桌面端可靠性：Computer Use/浏览器/MCP 故障、流断开、压缩失败以及跨设备状态同步占据 Issue 追踪器主导地位。PR 活动关闭了一大批围绕后台压缩、Guardian 审核处理、Windows WinGet 守护进程安装、TUI 键位绑定以及 app-server 守护进程重启的内部改进。

## 2. 发布
- [rust-v0.163.0-alpha.10](https://github.com/openai/codex/releases/tag/rust-v0.163.0-alpha.10) — 发布说明仅写明“Release 0.163.0-alpha.10”；未提供详细变更。
- [rust-v0.163.0-alpha.9](https://github.com/openai/codex/releases/tag/rust-v0.163.0-alpha.9) — 同上：没有实质性变更日志。
- [rust-v0.163.0-alpha.8](https://github.com/openai/codex/releases/tag/rust-v0.163.0-alpha.8) — 同上：没有实质性变更日志。

**分析师注：** 快速的 alpha 节奏表明 Rust CLI/运行时正在积极进行内部迭代，但由于没有变更日志，用户应将这些构建视为不稳定的预发布版本。

## 3. 热门 Issue
1. **[#49458](https://github.com/openai/codex/issues/49458) — Windows 上以点号启动的本地任务缺少 Computer Use 工具，而普通本地 Codex 会话正常。**  
   高影响的 Windows 回归：以点号启动的任务失去 Computer Use 能力。**79 条评论 / 26 👍** 使其成为本期互动最多的问题。

2. **[#53188](https://github.com/openai/codex/issues/53188) — Windows 浏览器工具初始化失败：MXC 启动器 HRESULT `0x80070003`。**  
   浏览器工具初始化失败，阻碍 Windows 用户使用基于浏览器的工作流。**19 条评论**，但尚无点赞。

3. **[#52839](https://github.com/openai/codex/issues/52839) — 使用 ChatGPT 身份验证时，Sol Ultrafast 未出现在第一方模型目录中。**  
   模型目录/认证一致性问题：付费 ChatGPT 用户无法看到或选择 Sol Ultrafast。**19 条评论** 表明存在明显困惑和复现兴趣。

4. **[#52919](https://github.com/openai/codex/issues/52919) — 自 10 月 10 日起，Windows / Codex Desktop + VS Code 出现间歇性流断开和重连失败。**  
   连接不稳定影响桌面端和 IDE 扩展工作流。**14 条评论 / 2 👍**。

5. **[#28864](https://github.com/openai/codex/issues/28864) — 在 TUI 中添加快捷键以编辑或移除特定的已排队后续输入。**  
   长期存在的增强请求，获得社区强力支持：用户需要对排队提示进行更细粒度控制。**13 条评论 / 24 👍**。

6. **[#47559](https://github.com/openai/codex/issues/47559) — VS Code agent 沙箱在命令运行前失败：mountinfo 路径不是绝对路径。**  
   标记为 **CLOSED**，但仍值得注意，因为沙箱失败会完全阻止命令执行。**12 条评论 / 6 👍**。

7. **[#48504](https://github.com/openai/codex/issues/48504) — Windows MCP HTTP 响应失败，报错“error decoding response body”；应用连接器不可用。**  
   Windows 上的 MCP/连接器故障影响可扩展性和工具集成。**10 条评论**。

8. **[#36454](https://github.com/openai/codex/issues/36454) — iOS 远程项目列表未与 Codex Desktop 同步。**  
   跨设备状态不匹配：Desktop 未被视作 Projects 的唯一可信来源。**10 条评论**。

9. **[#35485](https://github.com/openai/codex/issues/35485) — Windows Desktop 捆绑的 `node_repl` MCP 进程按线程泄漏，每线程一个。**  
   捆绑 MCP 运行时中的资源泄漏可能降低长时间运行的 Windows Desktop 会话性能。**9 条评论**。

10. **[#50789](https://github.com/openai/codex/issues/50789) — macOS 浏览器访问仍因会话级拒绝来源而被阻止，尽管有全局/站点允许设置。**  
    浏览器权限状态在全局、站点和会话范围之间似乎不一致。**8 条评论**。

## 4. 关键 PR 进展
*以下精选 PR 在本期均标记为 CLOSED；评论数未公开。*

1. **[#53800](https://github.com/openai/codex/pull/53800) — 将后台压缩接入回合生命周期。**  
   当启用 `AsyncCompaction` 时，在符合条件的回合前、回合中和回合后检查点启动后台压缩。对长时间运行会话的稳定性很重要。

2. **[#53811](https://github.com/openai/codex/pull/53811) — 支持从 Windows WinGet 包安装守护进程。**  
   修复布局不匹配问题：WinGet 将 CLI/code-mode 主机放在包根目录，导致守护进程安装失败。

3. **[#53816](https://github.com/openai/codex/pull/53816) — 在交互式 CLI 恢复期间拒绝 Guardian 审核线程。**  
   阻止恢复内部 Guardian 审核线程，并提示改为恢复父会话。

4. **[#53805](https://github.com/openai/codex/pull/53805) — 为工作区路由配置错误添加安全诊断。**  
   改进故障排查，同时避免在原始配置错误中暴露机密。

5. **[#53794](https://github.com/openai/codex/pull/53794) — 在工作进程取消期间保留 rollout 迁移锁。**  
   防止其他写入者/迁移触碰仍被已取消工作进程压缩或解压的文件。

6. **[#53790](https://github.com/openai/codex/pull/53790) — 修复 macOS 用户验证提示的取消问题。**  
   解决 Keychain 签名上下文中生物识别提示无法可靠取消的问题。

7. **[#53763](https://github.com/openai/codex/pull/53763) — 用解析后的插件和技能元数据丰富 V2 能力发现。**  
   返回完全限定技能名称、解析后的依赖项和策略元数据；共享 `agents/openai.yaml` 解析。

8. **[#53754](https://github.com/openai/codex/pull/53754) — 在 TUI 键位映射中支持 Command/Super 快捷键。**  
   将 `cmd`/`command` 别名规范化为 `super`，并支持捕获、显示和使用这些绑定。

9. **[#53751](https://github.com/openai/codex/pull/53751) — 重启 app-server 守护进程时应用功能覆盖。**  
   确保在守护进程重启期间遵守 `--enable`、`--disable` 和布尔 `-c features.*` 覆盖。

10. **[#53733](https://github.com/openai/codex/pull/53733) — 将 Guardian v2 异步分类默认设为 conversation 模式。**  
    在没有覆盖/模型默认值时将 `conversation` 设为默认，同时保留 `snapshot` 作为显式覆盖。

## 5. 热门讨论

### 展示与分享
- **[#53200](https://github.com/openai/codex/discussions/53200) — Reset Alerts：用于 Codex 重置的短信/Telegram 通知。**  
  社区构建的服务，用于跟踪 Codex 重置，无需刷新社交媒体。**1 条评论 / 2 👍**。
- **[#53721](https://github.com/openai/codex/discussions/53721) — tokenreceipt：按 API 标价计算 Codex CLI 会话费用。**  
  读取现有会话日志并打印收据样式成本估算。**1 条评论 / 1 👍**。
- **[#53492](https://github.com/openai/codex/discussions/53492) — Secure Browser Use for Zen：用于 Codex 浏览器控制的 Firefox/Zen 桥接。**  
  独立兼容层，将已登录的 Codex 侧边栏/浏览器控制带到 Zen。**1 条评论 / 1 👍**。
- **[#52198](https://github.com/openai/codex/discussions/52198) — cloud-alter-ego：为 Codex 和 Claude Code 提供持久记忆。**  
  跨 agent 工作流学习用户偏好、项目和过往会话。**1 条评论 / 1 👍**。
- **[#53552](https://github.com/openai/codex/discussions/53552) — NOS：粘贴 HTML 页面，在屏幕上修正措辞，并以链接分享。**  
  浏览器内 HTML 编辑工作流，带渲染预览和可分享链接。**1 条评论 / 1 👍**。
- **[#53803](https://github.com/openai/codex/discussions/53803) — swe-mux：运行多个 Codex 会话并显示实时状态，且可在手机上访问相同会话。**  
  在真实终端中运行真正的 `codex` CLI 会话，并显示等待/工作中/完成状态。**1 👍**。

### 问答
- **[#53199](https://github.com/openai/codex/discussions/53199) — 当自定义权限配置文件未出现在选择器中时，如何仅为一个本地 Codex Desktop 项目启用网络访问？**  
  用户希望为一个项目保持持久网络访问，同时在别处保持工作区文件系统沙箱和网络限制。**1 条评论 / 1 👍**。

**想法：** 本期未出现带 Ideas 标签的讨论。

## 6. 功能请求趋势
- **TUI/CLI/Desktop 中的队列管理：** 编辑、移除和清空已排队的后续输入；让已消费的提示可见并避免队列阻塞。参见 [#28864](https://github.com/openai/codex/issues/28864), [#51150](https://github.com/openai/codex/issues/51150)。
- **上下文感知的 composer 建议：** 在回合完成后建议符合逻辑的下一步提示。参见 [#42587](https://github.com/openai/codex/issues/42587)。
- **跨界面同步：** iOS 远程项目、Windows Chat/Work 切换、worktree 菜单和 Dots 电脑命名应反映一致的唯一可信来源。参见 [#36454](https://github.com/openai/codex/issues/36454), [#52811](https://github.com/openai/codex/issues/52811), [#49638](https://github.com/openai/codex/issues/49638), [#53016](https://github.com/openai/codex/issues/53016)。
- **沙箱、权限和审批透明度：** 按项目网络访问、更清晰的政策阻止解释、一致的审批行为以及更少的过期锁。参见 [#53134](https://github.com/openai/codex/issues/53134), [#53821](https://github.com/openai/codex/issues/53821), [#41305](https://github.com/openai/codex/issues/41305), [#53199](https://github.com/openai/codex/discussions/53199)。
- **上下文/压缩可靠性与可观测性：** 修复流断开、HTTP 400、远程压缩失败和 token 计数器不匹配。参见 [#53203](https://github.com/openai/codex/issues/53203), [#52406](https://github.com/openai/codex/issues/52406), [#43648](https://github.com/openai/codex/issues/43648)。
- **浏览器/Computer Use/MCP 健壮性：** 初始化失败、权限范围缺陷、进程泄漏、连接器不可用和工具调用停滞。参见 [#49458](https://github.com/openai/codex/issues/49458), [#53188](https://github.com/openai/codex/issues/53188), [#50789](https://github.com/openai/codex/issues/50789), [#48504](https://github.com/openai/codex/issues/48504), [#35485](https://github.com/openai/codex/issues/35485)。
- **CLI/TUI 打磨：** Command/Super 快捷键、选择即复制、TUI 启动韧性和守护进程打包。参见 [#53754](https://github.com/openai/codex/pull/53754), [#53753](https://github.com/openai/codex/pull/53753), [#52775](https://github.com/openai/codex/issues/52775), [#53811](https://github.com/openai/codex/pull/53811)。
- **Agent 记忆和会话可观测性：** 围绕持久记忆、成本收据、重置提醒和多会话仪表盘的社区项目。参见讨论 [#52198](https://github.com/openai/codex/discussions/52198), [#53721](https://github.com/openai/codex/discussions/53721), [#53200](https://github.com/openai/codex/discussions/53200), [#53803](https://github.com/openai/codex/discussions/53803)。

## 7. 开发者痛点
- **Windows Desktop 仍是摩擦最大的平台。** 以点号启动的任务缺少 Computer Use 工具、浏览器/MCP 初始化失败、进程泄漏、流断开、Work 会话卡住以及嵌入式浏览器崩溃都集中在 Windows 上。参见 [#49458](https://github.com/openai/codex/issues/49458), [#53188](https://github.com/openai/codex/issues/53188), [#48504](https://github.com/openai/codex/issues/48504), [#35485](https://github.com/openai/codex/issues/35485), [#53822](https://github.com/openai/codex/issues/53822), [#53820](https://github.com/openai/codex/issues/53820)。
- **长时间运行的会话很脆弱。** 流断开、远程压缩失败、HTTP 400 和不一致的上下文计数器阻碍持续工作。参见 [#52919](https://github.com/openai/codex/issues/52919), [#53203](https://github.com/openai/codex/issues/53203), [#52406](https://github.com/openai/codex/issues/52406), [#43648](https://github.com/openai/codex/issues/43648)。
- **沙箱和审批语义令人困惑。** 用户报告“Ask for approval”被忽略、命令被政策阻止却无解释、过期的 `.git/index.lock` 文件以及缺少按项目网络权限配置文件。参见 [#53134](https://github.com/openai/codex/issues/53134), [#53821](https://github.com/openai/codex/issues/53821), [#41305](https://github.com/openai/codex/issues/41305), [#53199](https://github.com/openai/codex/discussions/53199)。
- **队列管理不成熟。** 已消费的提示可能仍然不可见，已排队的提示可能被阻塞，且没有明确方式编辑/移除/清空队列。参见 [#51150](https://github.com/openai/codex/issues/51150), [#28864](https://github.com/openai/codex/issues/28864)。
- **跨设备和远程状态不是权威来源。** 项目列表、Chat/Work UI、worktree 操作和 Dots 电脑名称在不同界面之间无法保持同步。参见 [#36454](https://github.com/openai/codex/issues/36454), [#52811](https://github.com/openai/codex/issues/52811), [#49638](https://github.com/openai/codex/issues/49638), [#53016](https://github.com/openai/codex/issues/53016)。
- **MCP/连接器可能静默降级。** 解码失败、应用连接器不可用、泄漏的 `node_repl` 进程以及工具调用停滞数小时，使可扩展性不可靠。参见 [#48504](https://github.com/openai/codex/issues/48504), [#35485](https://github.com/openai/codex/issues/35485), [#47910](https://github.com/openai/codex/issues/47910), [#50329](https://github.com/openai/codex/issues/50329)。
- **认证/连接可能对环境敏感。** 一些用户遇到特定地区/ISP 的认证失败，而移动热点却可用；Windows 用户报告间歇性重连失败。参见 [#52738](https://github.com/openai/codex/issues/52738), [#52919](https://github.com/openai/codex/issues/52919)。
- **CLI 升级仍会产生边缘情况回归。** 升级后 TUI 启动失败、托管 app-server socket 问题以及模型目录/认证不匹配仍是活跃关切。参见 [#52775](https://github.com/openai/codex/issues/52775), [#50932](https://github.com/openai/codex/issues/50932), [#52839](https://github.com/openai/codex/issues/52839)。

---

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区文摘 — 2026-10-14

## 1. 今日亮点

发布活动保持稳定：**v0.64.0** 已发布，**v0.65.0-preview.0** 上线，针对不可信工作区进行了安全相关的加固，新的 nightly 版本推出了扩展加载弹性修复。与此同时，问题跟踪器中以 **subagent 可靠性**（挂起、MAX_TURNS 误报、浏览器代理设置被忽略）和 **终端渲染稳定性**（闪烁、调整大小、边框溢出）为主，今日大部分 PR 都针对 UI/渲染正确性。值得注意的是，PR #29737 提议提供对 **Gemini 4 Argon** 模型的一流支持。

## 2. 发布

- **v0.65.0-preview.0** — [repo](https://github.com/google-gemini/gemini-cli)
  - `fix(cli)`：在不可信文件夹中强制只读工作区设置（[#29583](https://github.com/google-gemini/gemini-cli/pull/29583)）—— 在打开不可信项目时收紧安全态势。
  - `fix(core)`：恢复会话时避免重复的工具响应轮次 —— 解决损坏/重放会话历史的一个来源。
- **v0.65.0-nightly.20261013.g51617c1bc**
  - `fix(cli)`：当一个扩展未通过允许列表检查时，继续加载其他扩展（[#29720](https://github.com/google-gemini/gemini-cli/pull/29720)）—— 一个坏扩展不再阻塞其余扩展。
  - [完整变更日志对比](https://github.com/google-gemini/gemini-cli/compare/v0.65.0-nightly.20261012.g9b6e0265d...v0.65.0-nightly.20261013.g51617c1bc)
- **v0.64.0**
  - `refactor(a2a-server)`：V1 → V2 设置迁移逻辑（[#29450](https://github.com/google-gemini/gemini-cli/pull/29450)）。
  - `fix(acp)`：桥接 `PromptResponse.usage` 并发出 `usage_update` 通知（[#29389](https://github.com/google-gemini/gemini-cli/pull/29389)）。

## 3. 热门问题

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — MAX_TURNS 后的 Subagent 恢复被报告为 GOAL 成功**（13 条评论，2 👍，p1）
   `codebase_investigator` 报告 `status: "success"` / `Termination Reason: "GOAL"`，尽管在进行任何分析之前就已达到轮次限制。静默截断伪装成成功，破坏了任何基于 subagent 输出的 agent 工作流的信任 —— 因此引发了持续、大量的讨论。

2. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — 零依赖 OS 沙箱与执行后意图路由**（9 条评论，1 👍，p2）
   提议让 Gemini 3 模型利用其原生的 bash 亲和力（`grep`、`cat`、`sed`、`awk`），同时通过 OS 级沙箱加上执行后的意图路由来不牺牲安全性。这是关于 agent 获得多少 shell 自由度的核心设计问题。

3. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — 通用 agent 挂起**（8 条评论，**8 👍**，p1）
   委派给通用 agent 会无限期挂起，即使是创建文件夹这样的琐碎任务；禁用 subagent 延迟可以绕过此问题。在前 30 列表中点赞数最高，表明广泛复现，而非边缘情况。

4. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — 评估 AST 感知文件读取、搜索和映射的影响**（7 条评论，p2，EPIC）
   跟踪 AST 感知工具是否能减少未对齐的读取、轮次和 token 噪声。这是一项基础性效率投资，其他几个问题（#22746、#22747）都依赖于此。

5. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini 未充分利用技能和子代理**（7 条评论，p2）
   虽是轶事但广泛呼应：模型仅在明确指示时才调用自定义技能/子代理，即使明显相关。这是委派挂起问题的另一面 —— 两个方向的路由可靠性。

6. **[#22267](https://github.com/google-gemini/gemini-cli/issues/22267) — 浏览器 Agent 忽略 `settings.json` 覆盖（例如 `maxTurns`）**（4 条评论，p2）
   `AgentRegistry` 正确合并设置，但浏览器 Agent 绕过它们。静默不生效的配置是团队的经典调试陷阱。

7. **[#22232](https://github.com/google-gemini/gemini-cli/issues/22232) — 浏览器 agent 弹性：自动会话接管和锁恢复**（4 条评论，p3）
   请求用恢复逻辑替换当前在锁定浏览器配置文件上的快速失败行为（持久会话模式与孤儿进程）。

8. **[#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — 浏览器 subagent 在 Wayland 上失败**（4 条评论，1 👍，p1，`agent/browser`）
   报告在 Wayland 上失败时出现 "Browser Agent Finished" 且 `Termination Reason: GOAL` —— Linux 桌面用户仍然是浏览器 agent 的二等路径。

9. **[#21000](https://github.com/google-gemini/gemini-cli/issues/21000) — 为任务跟踪器试验原生文件工具**（4 条评论，p3）
   探索将待办/任务跟踪器迁移到原生文件工具，与 #18836 中基于文件的持久化跟踪提案相关。

10. **[#20079](https://github.com/google-gemini/gemini-cli/issues/20079) — `~/.gemini/agents/filename.md` 符号链接未被识别为 agent**（4 条评论，p2，需信息）
    阻碍了 dotfile 管理器和 stow 风格的工作流，其中 agent 定义是符号链接而非真实文件。

*同样活跃：* [#24246](https://github.com/google-gemini/gemini-cli/issues/24246)（工具超过 128 个时出现 400 错误），[#23571](https://github.com/google-gemini/gemini-cli/issues/23571)（模型分散临时脚本），[#22672](https://github.com/google-gemini/gemini-cli/issues/22672)（agent 应避免破坏性的 `git reset --force`），[#22465](https://github.com/google-gemini/gemini-cli/issues/22465)（卡在交互式 `vite` 提示符），[#21763](https://github.com/google-gemini/gemini-cli/issues/21763)（bug 报告省略 subagent 上下文）。

## 4. 关键 PR 进展

1. **[#29725](https://github.com/google-gemini/gemini-cli/pull/29725) — `fix(ui)`：同步输出 + 隔离的 spinner 渲染**（p2，size/xl）
   将交互式 UI 帧写入包装在终端同步输出序列中，以消除 `tmux` 等多路复用器中的闪烁。可能是目前正在进行的最用户可见的渲染修复。

2. **[#29728](https://github.com/google-gemini/gemini-cli/pull/29728) — `fix(cli)`：防止快速 Ctrl-C 中断导致会话中毒**（已关闭，p1）
   等待 `cancelOngoingRequest` 而不是分离触发，关闭了双重 `Ctrl+C` 在 `/quit` 运行之前毒化会话的竞态。高严重性 UX 修复。

3. **[#29737](https://github.com/google-gemini/gemini-cli/pull/29737) — `feat(models)`：添加 Gemini 4 Argon 模型支持**
   在 `VALID_GEMINI_MODELS` 中注册 `gemini-4-argon`，其聊天配置扩展了 `chat-base-3`，因此无需自定义覆盖即可选择。如果被接受，这是当天头条模型可用性变更。

4. **[#29736](https://github.com/google-gemini/gemini-cli/pull/29736) — `refactor(core,cli)`：从调度器发出工具调用组，移除 UI 前瞻**
   将工具调用分组从 `useGeminiStream` 移到调度器中，移除阻碍已完成 subagent 调用的脆弱前瞻。与 subagent 轮次报告 bug 直接相关。

5. **[#29742](https://github.com/google-gemini/gemini-cli/pull/29742) — `fix(cli)`：防止可滚动工具容器中右侧边框溢出**（p2）
   修复展开的 `WriteFile`/`Edit` 视图以及存在滚动条时的备用缓冲区工具结果中间歇性断开的边框。

6. **[#29695](https://github.com/google-gemini/gemini-cli/pull/29695) — `fix(ui)`：调试控制台高度 + `terminalBuffer` 闪烁**（p2）
   修正 `F12` 布局计算，并通过静态历史记忆化和安全缓存失效启用增量渲染。

7. **[#29718](https://github.com/google-gemini/gemini-cli/pull/29718) — `fix(cli)`：为无根 Podman 沙箱添加 `--userns=keep-id`**（p2）
   通过将调用者的 UID 正确映射到容器命名空间，解决无根 Podman 用户的 `EACCES` 失败。

8. **[#29536](https://github.com/google-gemini/gemini-cli/pull/29536) — `fix(grep)`：通过显式 `-e` 防止命令行选项注入**（已关闭，p2，安全）
   通过为 `git grep` 和系统 `grep` 强制执行严格的参数分离，加固 `packages/core/src/tools/grep.ts` 以抵御 CWE-88。

9. **[#29678](https://github.com/google-gemini/gemini-cli/pull/29678) — `fix(cli)`：在解析设置占位符之前加载环境变量**（已关闭，p2）
   修复 `_doLoadSettings`/`loadEnvironment` 中的加载顺序竞态，其中 `${VAR}` 占位符在 `.env` 文件填充 `process.env` 之前就被展开。

10. **[#29703](https://github.com/google-gemini/gemini-cli/pull/29703) — `fix(core)`：保持原子写入临时文件名在 `NAME_MAX` 以内**
    较长的目标文件名（215–255 字节）因 `.<uuid>.tmp` 后缀超过 255 字节限制而失败，报错 `ENAMETOOLONG`。

*同样值得关注：* [#29722](https://github.com/google-gemini/gemini-cli/pull/29722)（仅含空白的 UTF-16/32 文件被视为空），[#29735](https://github.com/google-gemini/gemini-cli/pull/29735)（扩展 `downloadFile` 流错误处理），[#29563](https://github.com/google-gemini/gemini-cli/pull/29563)（截断时保留行终止符），[#29573](https://github.com/google-gemini/gemini-cli/pull/29573)（沙箱镜像名称中的注册表端口），[#29644](https://github.com/google-gemini/gemini-cli/pull/29644)（终端宽度变化时恢复防抖静态刷新），[#29740](https://github.com/google-gemini/gemini-cli/pull/29740)（升级到 `0.66.0-nightly`），[#29741](https://github.com/google-gemini/gemini-cli/pull/29741)（v0.64.0 变更日志）。

## 5. 热门讨论

此时间窗口未提供讨论数据；本节省略。

## 6. 功能请求趋势

- **Subagent 可观测性和正确性。** 一个明显的问题集群要求准确的终止报告（#22323）、通过 `/chat share` 可见的 subagent 轨迹（#22598），以及 `/bug` 报告中包含 subagent 上下文（#21763）。用户希望审计 subagent 实际做了什么，而不仅仅是它们声称的。
- **更智能的委派和自我认知。** 作为可靠性 bug 的补充，用户希望模型主动*选择*技能/子代理（#21968），并准确描述自己的 CLI 标志、热键和调用模式（#21432）。
- **节省 token、结构感知的上下文访问。** AST 感知读取/搜索/映射（#22745、#22746、#22747）加上“Tactful Extraction”精确读取（#19561）表明明确需求：减少每轮约 36.6k token 的基线，避免大文件灌入。
- **持久化、文件支持的任务跟踪。** #18836 和 #21000 都推动从上下文内 `WriteToDo` 转向 CRUD 风格的持久任务状态，以对抗上下文腐烂和跨会话记忆丢失。
- **沙箱和最小权限默认值。** 零依赖 OS 沙箱（#19873）、工作区范围策略（#18397）、只读不可信文件夹以及避免破坏性命令（#22672）构成了连贯的安全方向主题。
- **并行/共享内存多 agent 协作。** #18287（因并行 subagent 支持而受阻）和 #18285（通过 `settings.json` 发现 subagent）标志着单 subagent 稳定性之后的下一个前沿。
- **终端渲染健壮性。** 调整大小无闪烁（#21924）、tmux 同步和边框完整性是反复出现的体验改善需求。

## 7. 开发者痛点

- **挂起和停滞且无有用诊断。** 通用 agent 无限期挂起（#21409）以及卡在 `vite` 等交互式提示符（#22465）都表现为“工具突然停止”，没有超时或解释性错误。
- **静默错误配置。** 加载但从未应用的设置 —— 浏览器 Agent 忽略 `maxTurns`（#22267）、`settings.json` 值、`.env` 占位符过早解析（#29678）—— 迫使开发者对自己的配置进行源码级调试。
- **终端渲染不稳定。** `tmux` 中的闪烁、调整大小时的布局破坏（#21924、#29695）以及可滚动容器中的边框溢出（#29742）降低了核心 TUI 体验，尤其是对多路复用器用户。
- **会话状态损坏。** 恢复时重复的工具响应轮次（v0.65.0-preview.0）、快速 `Ctrl+C` 毒化会话（#29728）以及围绕 `\n` 的转义 bug（#22466）削弱了对长时间运行会话的信心。
- **Linux/容器上的沙箱和权限摩擦。** 无根 Podman `EACCES`（#29718）以及带注册表端口的镜像名称解析（#29573）表明容器路径仍需打磨。
- **工具数量和工作区卫生限制。** 工具超过约 128 个时出现 400 错误（#24246）以及模型将临时脚本分散到各目录（#23571）造成清理开销和大型设置的硬上限。
- **Agent 文件系统约定脆弱。** 符号链接的 agent 定义未被识别（#20079）破坏了拥有许多 agent 的用户的合理 dotfile 管理。

---
*数据来源：[github.com/google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli) · 编译于 2026-10-14*

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区摘要 — 2026-10-14

## 1. 今日亮点
Copilot CLI 发布了 **v1.0.98-1**，支持 **Gemini 4 Argon**，并修复了 MCP/会话同步、恢复会话时的权限处理以及终端输入保留问题。最新发布批次还包括 v1.0.97-3/v1.0.97，针对插件认证重试、TLS 拦截代理和 GitHub SCP 远程解析的修复。社区关注点仍集中在 **BYOK/自定义提供商模型处理**、**会话生命周期一致性** 以及 **Windows/MCP 认证可靠性**。

## 2. 发布版本
- **v1.0.98-1** — 新增 Gemini 4 Argon 支持。修复了分屏视图切换后 MCP 状态和 `/mcp` 与活动会话保持同步的问题；恢复会话时现在会尊重已放弃的待处理权限任务，仅继续活动的权限请求；普通键入字符会被保留。  
  https://github.com/github/copilot-cli/releases/tag/v1.0.98-1
- **v1.0.98-0** — 修复和变更；提供的数据中无详细说明。  
  https://github.com/github/copilot-cli/releases/tag/v1.0.98-0
- **v1.0.97** — 为自定义智能体新增智能体作用域的声明式钩子；远程插件市场/插件获取在 HTTPS 认证失败后，会使用已登录的 GitHub token 重试一次；当省略 `close_notify` 时，TLS 拦截代理后的流式响应能够完成；修复了 GitHub SCP 解析。  
  https://github.com/github/copilot-cli/releases/tag/v1.0.97
- **v1.0.97-3** — 修复了远程插件市场认证重试、TLS 代理流式完成，以及带绝对路径的 SCP 远程，使仓库所有者/名称能够正确解析。  
  https://github.com/github/copilot-cli/releases/tag/v1.0.97-3

## 3. 热门 Issue
- **#4082 [OPEN] Copilot CLI 与 Copilot 桌面应用之间的跨应用会话同步** — 3 条评论 / 13 👍。高需求工作流缺口：在 CLI 中启动的会话在桌面端不可见，反之亦然。  
  https://github.com/github/copilot-cli/issues/4082
- **#1724 [OPEN] 显示当前 TODO 状态** — 2 条评论 / 12 👍。用户希望像其他编码智能体一样，在侧边栏中展示智能体的 TODO 状态。  
  https://github.com/github/copilot-cli/issues/1724
- **#4358 [OPEN] BYOK：从提供商的 `/models` 端点填充 `/model` 选择器** — 2 条评论 / 9 👍。当前 BYOK 流程只暴露一个已配置模型，迫使用户重启才能切换模型。  
  https://github.com/github/copilot-cli/issues/4358
- **#5115 [OPEN] ACP：`/rename` 报告成功但不会重命名会话** — 5 条评论。尽管宣传了该命令，却破坏了 ACP/非交互式自动化的预期。  
  https://github.com/github/copilot-cli/issues/5115
- **#4950 [OPEN] BYOK 自定义提供商：CLI 强制贪婪采样（`temperature=0`），导致推理模型退化并静默挂起** — 3 条评论。自 1.0.81 以来，对 vLLM/OpenAI 兼容自定义提供商而言是严重正确性问题。  
  https://github.com/github/copilot-cli/issues/4950
- **#5126 [OPEN] BYOK：内置智能体从不使用其默认模型，而是静默运行在会话模型上** — 1 条评论。影响 BYOK 下的 `explore`、`task` 和 `research` 智能体行为。  
  https://github.com/github/copilot-cli/issues/5126
- **#5127 [OPEN] 使用 BYOK 提供商的 SDK：短模型名不会匹配到提供商模型** — 1 条评论。自定义智能体会失败，并且短会话模型可能产生误导性的 “No GitHub OAuth token” 错误。  
  https://github.com/github/copilot-cli/issues/5127
- **#5134 [OPEN] Windows：失控的 MCP OAuth 初始化会反复打开浏览器会话；Maximum update depth exceeded** — 0 条评论。新增但关键：会阻止 Windows 上的正常启动/认证。  
  https://github.com/github/copilot-cli/issues/5134
- **#5122 [OPEN] `userPromptSubmitted`：只有最后一个插件的 `additionalContext` 能到达模型** — 0 条评论。多插件钩子工作流中存在静默数据丢失；其他钩子会运行，但其上下文被丢弃。  
  https://github.com/github/copilot-cli/issues/5122
- **#5130 [OPEN] 围绕“Managed account policy”的持续错误消息** — 0 条评论 / 1 👍。至少自 1.0.94 起就有企业/策略摩擦报告。  
  https://github.com/github/copilot-cli/issues/5130

## 4. 关键 PR 进展
提供的数据集中过去 24 小时内没有拉取请求更新。没有可总结的 PR 进展。  
https://github.com/github/copilot-cli/pulls

## 5. 功能请求趋势
- **BYOK/自定义提供商成熟度**：从提供商 `/models` 生成 `/model` 选择器、短名称匹配、按智能体设置默认模型、在 `providers.json` 中提供凭据命令，以及对采样参数的控制。  
  https://github.com/github/copilot-cli/issues/4358 · https://github.com/github/copilot-cli/issues/5127 · https://github.com/github/copilot-cli/issues/5126 · https://github.com/github/copilot-cli/issues/4936 · https://github.com/github/copilot-cli/issues/4950
- **会话生命周期与命名**：跨应用会话同步、从会话视图/侧边栏重命名、可靠的 ACP 重命名、持久会话名称，以及嵌套会话切换后可预测的 `/exit` 行为。  
  https://github.com/github/copilot-cli/issues/4082 · https://github.com/github/copilot-cli/issues/5128 · https://github.com/github/copilot-cli/issues/5115 · https://github.com/github/copilot-cli/issues/5132 · https://github.com/github/copilot-cli/issues/5135
- **智能体可观测性与控制**：展示 TODO 状态、提示/响应计时，以及防止启动后 `--agent` 选择被丢弃。  
  https://github.com/github/copilot-cli/issues/1724 · https://github.com/github/copilot-cli/issues/5125 · https://github.com/github/copilot-cli/issues/5124
- **权限与中断 UX**：文件范围的写权限、权限系统 Epic、在用户输入期间等待，以及避免恢复期间陷入终端安全死胡同。  
  https://github.com/github/copilot-cli/issues/316 · https://github.com/github/copilot-cli/issues/285 · https://github.com/github/copilot-cli/issues/5131 · https://github.com/github/copilot-cli/issues/5123
- **企业/平台就绪度**：系统级 Windows 安装程序、减少托管账户策略错误，以及稳定的 MCP OAuth 初始化。  
  https://github.com/github/copilot-cli/issues/5129 · https://github.com/github/copilot-cli/issues/5130 · https://github.com/github/copilot-cli/issues/5134
- **插件/MCP 健壮性**：合并插件 `additionalContext`、MCP 状态同步，以及云智能体 Playwright MCP 恢复。  
  https://github.com/github/copilot-cli/issues/5122 · https://github.com/github/copilot-cli/issues/4931

## 6. 开发者痛点
- **BYOK 提供商仍然脆弱**：模型发现、短名称解析、默认智能体模型、采样默认值和认证错误是反复出现的阻碍。
- **会话状态在不同界面之间不一致**：ACP 重命名、侧边栏命名、分屏视图 MCP 状态、嵌套 `/exit` 以及 CLI/桌面同步都会造成困惑。
- **Windows 与企业认证/策略摩擦**：失控的 MCP OAuth 浏览器循环、持续的通知徽标、用户配置文件安装，以及“Managed account policy”错误都属于高摩擦问题。
- **权限提示过于强势**：用户报告在输入时被打断、写权限范围过宽，以及终端安全拒绝阻止会话恢复。
- **插件/MCP 钩子可靠性仍需改进**：某条钩子路径只采纳最后一个插件的上下文，且较旧的报告仍会出现 MCP 工具选择循环。
- **边缘情况网络/远程解析问题已得到积极修复**：TLS 代理 `close_notify`、SCP 风格的 GitHub 远程，以及插件认证重试，表明仍在持续加固。

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区简报 — 2026-10-14

## 1. 今日亮点

过去 24 小时没有发布任何版本；今日活动以 V2 稳定性工作为主——会话环境继承、重连韧性以及托管配置优先级都已作为 PR 落地，针对的是新提交的复现用例。与此同时，本时段讨论最热的帖子仍是免费模型配额问题（#15585，56 条评论），而 V2 功能对齐缺口（todo 工具、时间戳开关、非 git 会话可见性）继续推动待办事项。

## 2. 发布

过去 24 小时无发布。

## 3. 热门 Issue

1. **[#15585 [CLOSED] "free usage exceed" on all free models](https://github.com/anomalyco/opencode/issues/15585)** — 56 条评论，17 👍。本窗口内讨论最多的单项：1.2.15 用户在所有三个免费模型上遇到相同的配额错误，引发“免费”层级是否带有未披露限制的疑问。评论量（以及高 👍 数）表明，围绕免费模型配额的沟通是一个信任问题，而不仅仅是 bug。

2. **[#42421 [OPEN] [2.0] Add todo support for v2](https://github.com/anomalyco/opencode/issues/42421)** — 27 条评论，5 👍。在 V1 中，模型通过 `todowrite`/`todoread` 读写其原生 TODO 列表；在 V2 中，工具目录不再暴露这些工具。高评论数反映出 TODO 界面对 agent 工作流引导有多么核心。

3. **[#34644 [OPEN] GitHub Copilot provider not registered (Student plan, auto-only)](https://github.com/anomalyco/opencode/issues/34644)** — 9 条评论，**22 👍**。OAuth 完成，但 `github-copilot` 从未出现在模型选择器中。这是此处反应最高的开放 issue，表明有相当规模的学生订阅者受阻。

4. **[#30158 [CLOSED] Terminal button in web UI disappears since v1.15.12](https://github.com/anomalyco/opencode/issues/30158)** — 17 条评论，14 👍。该回归已通过清晰二分定位跟踪（1.15.11 正常，1.15.12 不正常），现已关闭。这是小 UI 回归造成不成比例干扰的典型例子。

5. **[#51949 [OPEN] After auto-compact, agent stops using top-level tools](https://github.com/anomalyco/opencode/issues/51949)** — 8 条评论。压缩后，agent 将所有调用都经由 `execute → tools.shell` 路由，报告其工具未注册，并失败。对于压缩不可避免的长会话，这可能很严重。

6. **[#49827 [OPEN] migrate-v1: non-git sessions hidden after V1→V2 upgrade](https://github.com/anomalyco/opencode/issues/49827)** — 7 条评论。V1 的 `global` 项目中的会话从 TUI 选择器中消失，因为 V2 创建按目录划分的项目并过滤到当前项目。数据完好，这让迁移的观感更糟。

7. **[#51109 [OPEN] Configurable prompt-cache TTL per agent and provider/model](https://github.com/anomalyco/opencode/issues/51109)** — 7 条评论。缓存默认开启，但 TTL 固定为 Anthropic 的 5 分钟临时窗口，等待子 agent 的编排器会反复承受缓存未命中。这是一个具体、由成本驱动的对 `opencode.jsonc` 配置的请求。

8. **[#55189 [OPEN] Desktop terminal fails on Windows: `opencode-pty` not in $PATH](https://github.com/anomalyco/opencode/issues/55189)** — 5 条评论。切换终端会得到 "Desktop IPC handler failed"，并伴随服务端 `503 Executable not found in $PATH`。当天提交并复现；Windows 桌面仍是最粗糙的平台。

9. **[#55228 [OPEN] [reproduced] `session.environment` not applied to subagent sessions](https://github.com/anomalyco/opencode/issues/55228)** — 4 条评论。`PUT /api/session/{id}/environment` 对父 shell 有效，但子会话继承的是服务进程环境。环境在 agent 树中泄漏既是正确性问题，也是安全问题。

10. **[#50672 [OPEN] Hardcoded default port 49374 breaks second client](https://github.com/anomalyco/opencode/issues/50672)** — 6 条评论。托管服务绑定机器全局端口，而注册是按用户的，因此共享回环地址上的第二个客户端（多用户机器、WSL 镜像网络）只会以通用超时失败。

## 4. 关键 PR 进展

1. **[#55257 fix(core): inherit the parent's session environment in child sessions](https://github.com/anomalyco/opencode/pull/55257)** — 关闭 #55228；`Session.create` 已经继承 location、metadata 和 permissions，因此 environment 是疏忽而非设计选择。从复现到补丁的周转很快。

2. **[#55258 fix(tui): tolerate transient failures in the post-reconnect location sync](https://github.com/anomalyco/opencode/pull/55258)** — 关闭 #55250，即网络重连后出现虚假的 "Session location unavailable" 提示。在服务器/代理稳定期间，将同步视为尽力而为。

3. **[#55149 fix(tui): keep long-pending requests past Bun's fetch deadline](https://github.com/anomalyco/opencode/pull/55149)** — 关闭 #54974。TUI 的普通 `OpenCode.make(...)` 客户端继承了 Bun 的 5 分钟 fetch 截止时间，导致合法长请求被终止。

4. **[#54930 feat(plugin): let plugins assert permissions](https://github.com/anomalyco/opencode/pull/54930)** — 为插件添加强进程内权限断言路径，填补了一个真实缺口：此前只有 `list`/`get`/`reply` 以及 `evaluate` 钩子，而 HTTP 路由不会等待回复。

5. **[#54023 fix(core): coordinate credential refreshes across locations](https://github.com/anomalyco/opencode/pull/54023)** — 通过 `credentials.lock(id, operation)` 串行化刷新，使不同 Locations 共享一次刷新并观察到相同结果，而不是相互竞争。

6. **[#55261 feat(tui): add prompt history scope setting](https://github.com/anomalyco/opencode/pull/55261)** — 通过可选的 `prompt.history_scope` 处理 #52154；`global` 保留当前的跨会话回忆，`session` 将 Up/Down 限定到当前对话。

7. **[#55267 fix(core): give managed configuration highest precedence](https://github.com/anomalyco/opencode/pull/55267)** — 最后加载系统配置目录和 macOS 托管偏好设置，使管理员强制设置覆盖全局和项目配置。补充了 #54921（#51107 / #53074）中已关闭的发现修复。

8. **[#55262 feat(core): show only Bedrock models the region serves](https://github.com/anomalyco/opencode/pull/55262)** — 通过并行 `foundation-models` 探测并设置 10 秒超时，实现按区域列出模型，减少 Bedrock 用户的噪音和选择失败。

9. **[#48431](https://github.com/anomalyco/opencode/pull/48431) + [#48432](https://github.com/anomalyco/opencode/pull/48432) fix(tui): coalesce delta writes / grow markdown live tail in place** — 均已关闭；共同消除了客户端流式路径中的 O(n²) 成本（关闭 #36043）。两者是独立层，可以任意顺序合并——这是本窗口内影响最大的性能工作。

10. **[#55274 add SerpApi websearch to opencode](https://github.com/anomalyco/opencode/pull/55274)** — V1 web 搜索插件的 V2 移植（取代 #52480 谱系）。带有 `needs:title`/`needs:compliance` 标签，因此预计合并前会有审查反复。

*另外值得注意：* [#55270 fix(ci): push daily stats with the OpenCode app token](https://github.com/anomalyco/opencode/pull/55270)（STATS.md 自 2026-08-27 起陈旧）以及 [#55269 (closed) fix(plugin): resolve the figma plugin's peer from the workspace](https://github.com/anomalyco/opencode/pull/55269)，后者在反复失败后解除了 `dev` 上的 `publish` 阻塞。

## 5. 热门讨论

本窗口未提供讨论数据。

## 6. 功能请求趋势

- **V2 与 V1 的功能对齐是主导主题。** 原生 TODO 工具（#42421）、每条消息时间戳（#49742）、提示历史作用域（#52154 → #55261）、托管配置目录（#51107 → #55267）以及非 git/全局会话可见性（#49827）都是“V1 有、V2 丢了”的请求。
- **成本与延迟治理。** 按 agent 和 provider/model 可配置的提示缓存 TTL（#51109），以及可配置的重试/退避策略，而不是固定指数退避（#41709）。
- **更深的插件面。** 进程内权限断言（#54930）、在 AI-SDK 路径上拦截出站请求（#42846），以及并行钩子批次中的逐调用错误保真度（#50331）。
- **提供商广度与正确性。** GitHub Copilot Student 仅 auto 支持（#34644）、通过 google-vertex 使用 OpenAI 兼容模型（#49741）、区域准确的 Bedrock 目录（#55262），以及第三方 web 搜索（#55274）。
- **将 MCP 作为一等集成路径。** 远程 Figma MCP 支持（#54805）和健壮的回环 OAuth 回调处理（#55201）。
- **长生命周期会话管理。** 面向长时间运行工作的会话级进程注册表（#53614）、向子 agent 传播环境（#55228），以及按会话隔离消息历史（#52154）。

## 7. 开发者痛点

- **配额与权益不明确。** 56 条评论的免费模型帖子（#15585）和未注册的 Copilot 提供商（#34644）都指向不清晰或静默不支持的订阅状态。
- **V2 迁移摩擦。** 缺失工具、隐藏历史、丢失托管配置以及缺少 UI 可供性，让升级感觉像数据丢失，即使实际什么都没丢。
- **流式与渲染性能。** O(n²) 流路径（#36043、#48431/#48432）和逐 delta markdown 重新解析（#54428）仍然是长会话的首要性能投诉。
- **文件监视器与重载不稳定。** 全局配置目录内的任何写入都会触发未去抖的兼容性重载——同时 `~/.claude/skills` 会循环重新订阅（#53393）。
- **Windows 损坏程度不成比例。** `shell: pwsh` 被忽略而改用 PowerShell 5.1（#41426），以及桌面终端缺少 `opencode-pty`（#55189）。
- **网络脆弱性。** websocket 中断后无法恢复的会话（#35029）、重连时虚假的 "location unavailable" 提示（#55250），以及硬编码服务端口在用户间冲突（#50672）。
- **上下文压缩导致 agent 行为退化。** 压缩后工具失忆（#51949）、被空 assistant 消息卡住的会话（#53571），以及排队消息在每个 agent 步骤重新注入一次（#55204），都表明 turn/prompt 组装层需要加固。

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi 社区摘要 — 2026-10-14

## 今日亮点
过去 24 小时内没有发布新版本。在 50 个更新的 issue 和 14 个更新的 PR 中，活动集中在 provider/model 路由正确性、OAuth 身份持久化、会话/工具状态恢复，以及模型 API 漂移（如 Gemini 4 思考控制和 GPT-6 推理强度缓存）等方面。PR 工作正在为这些可靠性路径落地修复，同时还包括 Web UI、Nix 打包和导出改进。

## 热门 Issue（10）
- [Issue #10314](https://github.com/earendil-works/pi/issues/10314) — **重新考虑全屏模式下的 Home/End 默认行为？** `[CLOSED]`  
  一场参与度很高的 UX 争论：Home/End 应保留经典行编辑行为，还是采用新的全屏 TUI 滚动？25 条评论和 5 👍 表明社区对终端交互默认行为有强烈兴趣。

- [Issue #4180](https://github.com/earendil-works/pi/issues/4180) — **链接不再可点击** `[CLOSED]`  
  alternate-term-mode 更改后出现回归，破坏了可点击超链接。19 条评论表明，对于通过 agent 跟踪网络来源的用户来说，这是一个持续存在的困扰。

- [Issue #10300](https://github.com/earendil-works/pi/issues/10300) — **ChatGPT OAuth ID token 未持久化，导致扩展无法访问账户身份** `[OPEN]`  
  OAuth 凭据转换会丢弃 ID token，因此扩展无法可靠访问 ChatGPT 账户身份。15 条评论以及安全/身份影响使其成为优先问题。

- [Issue #5916](https://github.com/earendil-works/pi/issues/5916) — **支持带模型别名的 provider 扩展并改进搜索** `[OPEN]` `[bug, inprogress]`  
  通过 `models.json` 配置 OpenRouter/自定义 provider 的用户需要模型别名和更好的发现能力。14 条评论反映出对更灵活 provider 配置的持续需求。

- [Issue #9075](https://github.com/earendil-works/pi/issues/9075) — **压缩摘要会继承自适应模型上的会话思考等级并触达输出上限** `[OPEN]`  
  自适应思考的 Anthropic 模型在压缩摘要期间可能会确定性地触达 token 上限。10 条评论和 4 👍 突显了长会话中的严重可靠性问题。

- [Issue #10853](https://github.com/earendil-works/pi/issues/10853) — **GPT-6 的缓存保留式推理强度变更** `[OPEN]` `[prio:p0]`  
  会话中途更改思考等级会重写请求前缀并错过缓存；在 Codex 上还会丢弃 WebSocket continuation。5 条评论，p0 优先级。

- [Issue #10299](https://github.com/earendil-works/pi/issues/10299) — **openai-completions：来自 OpenRouter/GLM 的空 tool call 会永久卡死会话** `[OPEN]` `[bug]`  
  格式错误的空 `tool_calls` 条目会被保存并重放，永久破坏后续轮次。报告者提供了复现和针对 #4854 的修复，因此非常可操作。

- [Issue #10785](https://github.com/earendil-works/pi/issues/10785) — **恢复会话会丢弃动态激活的工具** `[CLOSED]` `[prio:p0]`  
  会话期间激活的可选/延迟工具在恢复后丢失，且下一轮会记录它们的移除。一个关键的会话状态 bug，有 3 条评论。

- [Issue #10797](https://github.com/earendil-works/pi/issues/10797) — **macOS：bash 超时导致 Playwright Chrome 在分离的进程组中继续运行** `[OPEN]`  
  被中断的前台任务可能会在 macOS 上泄漏 Chrome 进程。3 条评论；对通过 Pi 运行浏览器自动化的用户很重要。

- [Issue #10868](https://github.com/earendil-works/pi/issues/10868) — **pi-ai：无效的 `\u` 转义导致 tool-call 参数丢失字段** `[CLOSED]` `[bug, prio:p0]`  
  诸如 `\url` 之类的无效 JSON 转义可能导致流式 tool-call 解析中静默丢失字段。2 条评论，但由于会破坏工具参数，严重性为 p0。

## 关键 PR 进展（10）
- [PR #10913](https://github.com/earendil-works/pi/pull/10913) — **通过统一的 provider 选择策略解析模型引用** `[OPEN]`  
  修复 #10160：`--model provider/id` 应遵循凭据，而不是按目录顺序选择第一个网关。

- [PR #10910](https://github.com/earendil-works/pi/pull/10910) — **将 NVIDIA 默认模型设为 Nemotron 3.5 Super VL** `[OPEN]`  
  将 NVIDIA 默认模型切换为 `nvidia/nemotron-3.5-super-vl-120b-a12b`。在该模型公开前为草稿；取代 #10906。

- [PR #10528](https://github.com/earendil-works/pi/pull/10528) — **重构 Nix 部分** `[OPEN]`  
  重做 Nix 打包：清理 `package.nix`，移除 maps/type 文件，使用 `makeBinaryWrapper`，并跳过版本检查。

- [PR #10902](https://github.com/earendil-works/pi/pull/10902) — **对 Gemini 4 模型使用 thinking levels** `[CLOSED]`  
  修复错误发送 `thinkingBudget` 的 Gemini 4 请求；Vertex 会以 400 拒绝仅预算控制。相关重复项：[PR #10899](https://github.com/earendil-works/pi/pull/10899)。

- [PR #10896](https://github.com/earendil-works/pi/pull/10896) — **新增 Web UI** `[CLOSED]`  
  一个值得注意的功能 PR，引入了 Web UI，不过所提供的摘要为空。值得关注其未来在 Pi 可用性方面的工作。

- [PR #10521](https://github.com/earendil-works/pi/pull/10521) — **为 NVIDIA NIM 模型内联 `$ref` 工具 schema** `[OPEN]`  
  修复 #10270：当 schema 使用本地 `$ref` 时，NIM 模型可能将对象字段返回为 JSON 字符串，导致 `validateToolArguments` 失败。

- [PR #10795](https://github.com/earendil-works/pi/pull/10795) — **从 Anthropic 错误工具结果中移除图像** `[CLOSED]`  
  通过防止在 Anthropic 工具错误结果中发送图像内容来修复 #10794，避免 API 失败。

- [PR #10887](https://github.com/earendil-works/pi/pull/10887) — **在恢复会话时恢复活动工具** `[CLOSED]`  
  修复 #10785：恢复的会话现在会还原记录的 tool loadout，并在其上应用 `--tools +name/-name`。

- [PR #10886](https://github.com/earendil-works/pi/pull/10886) — **将 GPT-6 强度变更作为 `configuration_update` 项发送** `[CLOSED]`  
  关闭 #10853：将首次强度保留在请求级别，并将后续变更作为配置更新发送，从而保留缓存和 Codex WebSocket continuation。

- [PR #10718](https://github.com/earendil-works/pi/pull/10718) — **在 `--export` HTML 中包含系统提示词** `[OPEN]`  
  通过包含当前分支中记录的系统提示词，使 CLI HTML 导出与交互式 `/export` 一致。

## 功能请求趋势
未提供 Discussion 数据，因此以下趋势提炼自 Issue 和 PR。

- **Provider/模型路由与配置**  
  用户希望有更清晰的 provider 选择、模型别名、感知凭据的路由，以及系统级配置优先级。示例：[#10913](https://github.com/earendil-works/pi/pull/10913)、[#5916](https://github.com/earendil-works/pi/issues/5916)、[#10895](https://github.com/earendil-works/pi/issues/10895)、[#10889](https://github.com/earendil-works/pi/issues/10889)。

- **会话与工具状态持久性**  
  恢复活动工具、保留压缩基线、处理摘要生成 token 上限，以及修复提示模板参数偏移，都是反复出现的主题。示例：[#10785](https://github.com/earendil-works/pi/issues/10785)、[#10904](https://github.com/earendil-works/pi/issues/10904)、[#9075](https://github.com/earendil-works/pi/issues/9075)、[#10867](https://github.com/earendil-works/pi/issues/10867)。

- **终端/TUI 交互打磨**  
  Home/End 行为、可点击链接、硬件光标定位、tmux 内联图像、键盘输入恢复，以及 rxvt-unicode 滚轮处理仍是活跃的 UX 关注点。示例：[#10314](https://github.com/earendil-works/pi/issues/10314)、[#4180](https://github.com/earendil-works/pi/issues/4180)、[#10878](https://github.com/earendil-works/pi/issues/10878)、[#10872](https://github.com/earendil-works/pi/pull/10872)。

- **安全、身份与信任**  
  OAuth token 持久化、MCP 客户端标识、系统级配置优先级，以及无副作用的 SDK 运行时注册，对嵌入方和企业信任很重要。示例：[#10300](https://github.com/earendil-works/pi/issues/10300)、[#10883](https://github.com/earendil-works/pi/issues/10883)、[#10458](https://github.com/earendil-works/pi/issues/10458)。

- **打包与生态兼容性**  
  Nix 构建、锁定版本 provider SDK 的更新、Bun/Node 兼容性、proper-lockfile 维护，以及 Web UI/导出工具，都是反复出现的生态请求。示例：[#10881](https://github.com/earendil-works/pi/issues/10881)、[#10909](https://github.com/earendil-works/pi/issues/10909)、[#10912](https://github.com/earendil-works/pi/issues/10912)、[#10896](https://github.com/earendil-works/pi/pull/10896)。

## 开发者痛点
- **恢复/压缩后的静默状态损坏** —— 动态激活的工具消失、压缩基线漂移，摘要生成可能触达 token 上限：[#10785](https://github.com/earendil-works/pi/issues/10785)、[#10904](https://github.com/earendil-works/pi/issues/10904)、[#9075](https://github.com/earendil-works/pi/issues/9075)、[#10908](https://github.com/earendil-works/pi/issues/10908)。

- **模型 API 漂移** —— Gemini 4 拒绝 `thinkingBudget`，GPT-6 推理强度变更破坏缓存连续性，来自 OpenRouter/GLM 的格式错误 tool call 会卡死会话：[#10898](https://github.com/earendil-works/pi/issues/10898)、[#10853](https://github.com/earendil-works/pi/issues/10853)、[#10299](https://github.com/earendil-works/pi/issues/10299)、[#10868](https://github.com/earendil-works/pi/issues/10868)。

- **OAuth 与认证摩擦** —— ChatGPT ID token 未持久化，Copilot 设备轮询在 WSL 时钟偏移下可能无限失败：[#10300](https://github.com/earendil-works/pi/issues/10300)、[#10694](https://github.com/earendil-works/pi/pull/10694)。

- **Provider/配置意外** —— provider 缺失、别名/搜索缺口，以及不感知凭据的模型路由会造成困惑：[#10889](https://github.com/earendil-works/pi/issues/10889)、[#5916](https://github.com/earendil-works/pi/issues/5916)、[#10913](https://github.com/earendil-works/pi/pull/10913)。

- **终端/TUI 边缘情况** —— Home/End 默认行为、超链接、光标跳动、tmux 图像、stdin 缓冲，以及 rxvt-unicode 滚轮行为持续产生报告：[#10314](https://github.com/earendil-works/pi/issues/10314)、[#4180](https://github.com/earendil-works/pi/issues/4180)、[#10862](https://github.com/earendil-works/pi/issues/10862)、[#10884](https://github.com/earendil-works/pi/issues/10884)。

- **构建与兼容性回归** —— Nix v1.2.0 构建失败、SDK/TypeBox 版本锁定、Pi Codemode 兼容性，以及扩展缓存清理会影响下游宿主：[#10881](https://github.com/earendil-works/pi/issues/10881)、[#10909](https://github.com/earendil-works/pi/issues/10909)、[#10865](https://github.com/earendil-works/pi/issues/10865)、[#10911](https://github.com/earendil-works/pi/issues/10911)。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区简报 — 2026-10-14

## 1. 今日亮点

`managed-agent` 产品线的发布工程仍在继续：今天同时发布了预览版（`v0.25.2-preview.0`）和 nightly，头条是来自 #12692 R2 审查的连接器/Broker 稳健性修复，以及 H4e-b2 工作的启动。Issue 流量主要由 **托管 Managed Agent 的持久性与恢复缺口** 占据——P1 bug 包括唤醒输入、子结果和会话接管在 Harness 重启后丢失或被 409 拒绝。第二类报告集中在 **提供商兼容性**：严格的 OpenAI 兼容后端（vLLM/SGLang）在工具序列化或上下文溢出检测上整体失败，而由此产生的 CLI 错误无法诊断。

## 2. 发布

过去 24 小时内发布了两个构建：

- **v0.25.2-preview.0** — https://github.com/QwenLM/qwen-code/releases
  - `fix(managed-agent)`：来自 #12692 R2 审查的连接器和 Broker 稳健性后续修复（#13330，@wenshao）
  - `feat(managed-agent)`：H4e-b2（源代码数据中发布说明被截断）
- **v0.25.1-nightly.20261013.6212189f1f** — 相同的连接器/Broker 修复加上 H4e-b2 功能切片

两者实际上是同一内容线；preview 是稳定化切版，nightly 是提交 `6212189f1f` 的 CI 验证快照。

## 3. 热门 Issue

1. **#14116 — P1：Harness 死亡前提交的唤醒输入永远不会执行，直到某个无关操作加载 Session** (https://github.com/QwenLM/qwen-code/issues/14116)
   监控通知、频道消息、子结果、自动化触发和会话消息都会持久化提交，但只有在托管 Harness 上加载了 Session 时才会执行。这是下方多个崩溃恢复 bug 的根因 issue。

2. **#14114 — P1：ACP 会话从不宣告会话启动后发现的 MCP 工具** (https://github.com/QwenLM/qwen-code/issues/14114)
   `qwen --acp` 会从 `session/new` 注册 MCP 服务器，但永远不会处理 deferred-tool 提醒，因此模型无法看到或使用这些工具。Headless 模式正常——这是一个明显、可复现的集成回归。

3. **#14112 — 基于 vLLM 的 OpenAI 兼容提供商每个请求都返回 400** (https://github.com/QwenLM/qwen-code/issues/14112)
   无参数工具序列化时缺少 `parameters` 字段，并且会把 `tool_choice: "required"` 发送给拒绝它的后端。已在 TheHive AI 上使用 DeepSeek/GLM 模型复现——实际上对该类提供商构成了硬阻塞。

4. **#14115 — 无响应体的提供商 400 无法诊断** (https://github.com/QwenLM/qwen-code/issues/14115)
   CLI 只显示 `[API Error: 400 status code (no body)]`。请求在 CLI 错误和 `OPENAI_ERROR` 日志中都暴露 `errorResponse`、headers 和 `x-request-id`。直接放大了 #14112。

5. **#14096 / #14097 — vLLM 和 SGLang 的上下文溢出错误未被识别** (https://github.com/QwenLM/qwen-code/issues/14096, https://github.com/QwenLM/qwen-code/issues/14097)
   `getContextLengthExceededInfo` 不匹配这两个服务器的措辞，因此响应式压缩永远不会触发，长会话会永久卡住。两位独立报告者（均为 @Shizoqua）为两个后端提交了相同的根因。

6. **#14062 — 托管 `send_message` 即使在其大小限制内，仍可能导致整个发送方 Turn 失败** (https://github.com/QwenLM/qwen-code/issues/14062)
   `encodeToolInput` 溢出会被抛出，而不是作为拒绝返回，因此模型永远得不到“缩短并重试”的信号。本地字节检查与实际编码器不一致。

7. **#14063 — `session_command` 超时文本让 `member_message` 发送方去“读看板”；重试会重复消息** (https://github.com/QwenLM/qwen-code/issues/14063)
   误导性证据文本加上非幂等重试——这是多智能体协调的正确性问题，标签为 `status/ready-for-agent`。

8. **#14110 — 构建失败：`packages/cli` 使用了固定版本 `ink@7.0.3` 未导出的 ink API** (https://github.com/QwenLM/qwen-code/issues/14110)
   在 v0.25.1 上干净执行 `pnpm install --frozen-lockfile && pnpm build` 会出现 51 个 TypeScript 错误。这是一个阻塞贡献者上手的问题（例如缺少 `ReadonlyFrame` 导出）。

9. **#13787 — XML 恢复在大型多调用响应上重复进行前缀扫描** (https://github.com/QwenLM/qwen-code/issues/13787)
   在包含许多有效工具调用的响应上出现延迟回归；复现表将 `main` 与 PR #13579 进行对比。对智能体循环性能敏感。

10. **#12333 — eval：用本地 CLI A/B 验证工具加载成本和任务质量** (https://github.com/QwenLM/qwen-code/issues/12333)
   该时间窗口内评论最多的 issue（16 条评论），现已 **CLOSED**。范围被收窄为现有本地 CLI A/B 证据，而不是要求标准套件运行器；其后续 **#14065** 记录了三个 arm-B 失败会话作为具体反例。

其他值得关注：**#11272**（取消 stdio MCP 调用会在 Channel 模式下永久杀死服务器）、**#13283**（LSP pull 能力从未被读取 → 15 秒超时否决工作区报告）、**#14075**（Static 模式在终端宽度处孤立 1–2 个字符）、**#14122**（Stage D7 `java_durable` 准入配置——#12867 最后未完成的部分）。

## 4. 关键 PR 进展

1. **#14123 — fix(managed-agent)：让 held-Runtime 排空在 Harness 接管后仍能抗重启** (https://github.com/QwenLM/qwen-code/pull/14123)
   held-Runtime 排空义务此前仅存在于进程本地内存中，因此重启会导致 Session 的关闭应答不正确。这是持久化修复；#13986 是自动修复接管的变体。

2. **#14069 — feat(managed-agent)：将持久化整合为 32 表基线** (https://github.com/QwenLM/qwen-code/pull/14069)
   将 63 张业务表合并为共享的 32 表 MySQL 8.4 基线；服务器和 Runtime Broker 现在使用同一个带版本的 DDL 源，已退役的迁移序列被归档。（Closed。）

3. **#14073 — feat(providers)：将 QwenCloud 添加为第一个 `/auth` 条目** (https://github.com/QwenLM/qwen-code/pull/14073)
   根据 #14025 产品裁决，将 QwenCloud 置于 ModelStudio 之前；两个条目共存，现有 ModelStudio 凭据/端点不受影响。

4. **#13996 — feat(sdk-java)：添加私有 CSI Session 和原生 SQL 授权** (https://github.com/QwenLM/qwen-code/pull/13996)
   从 #13526 提取：持久化请求固定、写入者/首次激活语义、原生日志/资源证明、不可变 SQL 授权，以及经过认证的 worker 绑定/历史/执行。

5. **#13642 — feat(runtime-broker)：终端 JDBC 历史的有界保留** (https://github.com/QwenLM/qwen-code/pull/13642)
   可选启用清理退役绑定下的旧终端历史，在专用调度器上脱离请求路径运行，先删除执行记录，再删除会话和绑定。

6. **#13325 — fix(managed-agent)：关闭 #12692 上关键的 R2 审查发现** (https://github.com/QwenLM/qwen-code/pull/13325)
   修复八个 Critical，包括 InnoDB 锁顺序反转，以及键集分页遍历可变 `updated_at` 的问题。与今日构建中的发布说明修复配套。

7. **#13737 — fix(rewind)：通过源身份解析旧版 Turn** (https://github.com/QwenLM/qwen-code/pull/13737)
   Rewind 现在通过源身份关联可见消息、模型条目和记录的边界——恢复的旧版密钥不再能授权文件恢复。

8. **#13816 — fix(managed-agent)：当工具 Turn 进入恢复阻塞时交还 Workspace 挂载** (https://github.com/QwenLM/qwen-code/pull/13816)
   新增一个范围很窄的 Broker 内部 `POST /tool-sessions/{runtimeSessionId}:release-mount`，只释放 Workspace 执行租约，而不扰动其他状态。

9. **#14120 — test(managed-agent)：FG8e/FG8f monitor_run 与后台 shell 故障门** (https://github.com/QwenLM/qwen-code/pull/14120)
   多进程集成测试，驱动真实打包的 Harness、带嵌入式 Runtime Broker 的 in-JVM Spring 控制平面，以及带 JDBC 基准真值的真实 MySQL。

10. **#14021 — fix(managed agent)：在实时 Turn 中结算从未启动的后台派发** (https://github.com/QwenLM/qwen-code/pull/14021)
   失败的后台 Shell 派发现在会在原 Broker 证明没有进程被准入时结算其子任务，而 present/unknown/rejected 结果保持未结算。

同样值得注意：**#13994**（使用 `qwen-code` 标识符进行 OSC 7501 终端状态报告）、**#14035**（Web Shell Markdown 的 RTL/bidi 隔离）、**#13957**（在高度受限的 CLI 工具结果中保留 Markdown）、**#14107/#14108**（托管恢复测试窗口放宽，以及针对 CI 不稳定的 shell 生命周期 journal-head 断言修复）、**#9305**（短 VP 内容底部对齐——长期存在的 UX 修复）。

## 5. 热门讨论

本时间窗口未提供 Discussions 数据；本节省略。

## 6. 功能请求趋势

- **Managed Agent 持久性与多智能体会话管理**——主导主题。请求集中在抗重启的 Runtime 排空义务、独立于 Session 加载的唤醒输入执行、actor 角色/租户隔离验收（#13535）、`java_durable` 准入配置（#14122），以及子智能体工具行为（#14065）。
- **更广泛的提供商兼容性**——明确要求为 vLLM 风格后端提供正确的工具 schema 序列化（#14112）、为 vLLM 和 SGLang 提供提供商特定的上下文溢出检测（#14096/#14097），以及为无响应体的 4xx 响应提供更丰富的诊断（#14115）。
- **上下文与 token 管理**——响应式压缩正确性、工具加载成本与任务质量评估（#12333）、XML 工具调用恢复性能（#13787），以及将 auto-recall hook 上下文与本地记忆去重（#14089）。
- **认证/提供商表面扩展**——QwenCloud 成为与 ModelStudio 并列的一等 `/auth` 条目（#14073）。
- **MCP 生态广度**——ACP 延迟工具宣告（#14114）、MCP 服务器生命周期韧性（#11272），以及 SerpApi 等第三方 MCP 网页搜索服务的文档（#13092）。
- **核心工具与环境集成**——LSP pull 能力协商（#13283）、斜杠命令/CLI 人体工学，以及桌面/Web Shell 打磨（logo 刷新 #14038、原生拖放 #14049）。

## 7. 开发者痛点

- **托管会话状态在重启和接管过程中丢失。** 并发接管加载发生竞争，其中一个会被 `409 hosted_session_already_attached` 拒绝，使被搁置的 Turn 付出完整退避步骤的代价（#14070）；控制平面被杀后的重新附加也会类似失败（#13838）；恢复的子答案会一直未被读取，直到下一个无关输入（#14047）。
- **无法诊断的错误。** 无响应体的提供商 400（#14115）、上下文溢出措辞不匹配（#14096/#14097），以及误导性的超时证据文本（#14063）都让用户得不到可操作的信号——而在重试场景中，还会产生重复副作用。
- **自托管 OpenAI 兼容后端是二等公民。** 基于 vLLM 的提供商每个请求都失败（#14112），且 vLLM 和 SGLang 的溢出都不会触发压缩。
- **构建与 CI 摩擦。** `pnpm build` 在固定版本 `ink@7.0.3` 上开箱即失败（#14110），主分支 CI 失败被自动化机器人反复提交（#12714、#14072），后续 PR 放宽了时序窗口并修复恢复断言（#14107、#14108）。
- **MCP 服务器生命周期脆弱。** 取消长时间运行的 stdio 工具调用会在 Channel 模式下杀死 MCP 服务器，且没有恢复路径（#11272）。
- **终端渲染打磨。** Static 模式的自动换行在全宽下会丢掉 1–2 个字符（#14075），而短视口内容对齐自 8 月以来一直是未解决的抱怨（#9305）。
- **报告的大小限制与实际执行的限制不匹配。** 托管 `send_message` 通过了本地检查，但在编码器中抛出异常（#14062）；`qwen batch` 接受词法上不同但解析到同一文件的路径，从而产生重复请求（#14041）。

---

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*