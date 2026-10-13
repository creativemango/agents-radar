# AI CLI 工具社区动态日报 2026-10-13

> 生成时间: 2026-10-13 03:08 UTC | 覆盖工具: 7 个

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

# 跨工具 AI CLI 对比报告 — 2026-10-13

## 1. 生态概览

AI CLI 格局正从终端聊天转向生产级智能体运行时。在 Claude Code、Codex、Gemini CLI、Copilot CLI、OpenCode、Pi 和 Qwen Code 中，互动最高的议题如今是会话持久性、权限正确性、成本治理、多智能体编排和跨平台可靠性，而不再只是原始模型质量。发布节奏不一：Copilot CLI、Pi、Gemini CLI 和 Qwen Code 发布了更新；Claude Code、Codex 和 OpenCode 没有发布，但 issue/PR 活跃。企业关注点——托管配置、BYOK 路由、MCP 认证、沙箱策略——如今与撤销、终端渲染、剪贴板行为等个人痛点并列。共同信号是：智能体工作流已从演示走向需要回滚、可观测性和可执行策略的系统。

## 2. 活动对比

| 工具 | Issues（24h） | PR（24h） | 讨论（24h） | 发布状态 |
|---|---:|---:|---:|---|
| Claude Code | 10 个重点 | 0 个已更新 | N/A* | 24 小时内无 |
| OpenAI Codex | 50 个已更新 | 24 个已更新 | 4 | 24 小时内无 |
| Gemini CLI | 14 个重点（10 个热门 + 4 个也活跃） | 15 个重点/已合并 | N/A* | v0.65.0-nightly.20261013.g51617c1bc |
| GitHub Copilot CLI | 35 个已更新（显示 30 个） | 0 个已更新 | N/A* | v1.0.97-2, v1.0.97-1, v1.0.97-0, v1.0.96 |
| OpenCode | 10 个重点 | 10 个重点 | N/A* | 24 小时内无 |
| Pi | 10 个重点 | 11 个重点 | 1 | v1.2.0 |
| Qwen Code | 10 个重点 | 10 个重点 | N/A* | v0.25.1-nightly.20261012.18f80f5666 |

\*N/A = 源摘要未提供讨论数据；这不是零计数，也不一定意味着讨论被禁用。

## 3. 共同功能方向

| 主题 | 工具 | 具体需求 |
|---|---|---|
| 会话安全、回滚、恢复 | Codex, Claude, OpenCode, Pi, Copilot, Gemini, Qwen | `/undo`、回滚未跟踪/未提交的工作；防止压缩后上下文丢失；溢出/重试后恢复；恢复动态激活的工具；取消溯源；可抗重启的恢复。 |
| 权限/沙箱/hook 优先级 | Claude, Gemini, Copilot, OpenCode, Pi, Codex | 显式优先级：hooks > settings > guard heuristics；修复过宽的 worktree/沙箱拒绝；强制执行按命令和 webfetch 的拒绝规则；保护会话目录安全；恢复已保存的线程权限。 |
| 模型/提供商回退与容量 | Codex, OpenCode, Claude, Copilot, Pi | 跨模型故障转移；有配额但仍出现容量错误；BYOK 短名称与子智能体路由；自动模式分类器回退；感知密钥的 OpenRouter 目录。 |
| 成本治理与用量可观测性 | Claude, Codex, Copilot, Pi, Gemini | 强制智能体/并发限制；预算上限与支出可见性；配置用量限制警告；提示缓存；模型调用 OTel；节省 token 的上下文。 |
| 多智能体持久性/可观测性 | Qwen, Gemini, Codex, Claude, Pi, Copilot | 智能体身份/团队消息传递；共享内存；子智能体状态与轨迹；MCP 进程泄漏隔离；并发上限；按请求的 taskId/purpose；BYOK 子智能体默认值。 |
| 托管/企业配置与认证 | OpenCode, Copilot, Codex, Qwen | 在 Windows/Linux/macOS 上发现系统/托管配置；托管策略韧性；Entra/WAM 认证；企业弃用横幅；已认证账号可见性。 |
| 跨平台可靠性 | Codex, Claude, Copilot, OpenCode, Pi, Qwen, Gemini | Windows/WSL/macOS/Linux 启动、认证、剪贴板、shell 路径、打包、内核/资源泄漏、Wayland、ripgrep SIGSEGV。 |
| MCP/工具生态扩展 | Gemini, Copilot, Codex, Qwen, OpenCode, Claude | 工具数量上限超过约 128；MCP OAuth/协议保持最新；持久 MCP 生命周期；按调用排除服务器；OAuth 资源编码；过期认证状态。 |
| 静默失败与正确性 | Claude, Gemini, Pi, OpenCode, Copilot, Codex | LSP gitignore 过滤；Bash 反斜杠折叠；UTF-16/32 空内容；NAME_MAX 失败；遗漏目录项；被丢弃的会话缺陷；图像附件丢失；重复 Host 头。 |
| 终端/TUI 用户体验 | Gemini, Pi, Copilot, OpenCode, Qwen, Codex | tmux 闪烁/OSC52；调整大小后刷新；调试控制台布局；剪贴板可靠性；终端状态恢复；鼠标/焦点转义泄漏；键盘历史；内联图像。 |

## 4. 差异化分析

| 工具 | 功能重点 | 目标用户 | 技术路径 |
|---|---|---|---|
| Claude Code | 文件写入能力、桌面/沙箱权限、hooks、插件 API、成本护栏 | Anthropic/终端+桌面团队 | 第一方模型、桌面/CLI 对等性、策略 hooks、worktree/沙箱强制执行 |
| OpenAI Codex | 会话安全/撤销、守护进程可选启用、模型目录/容量、浏览器/Computer Use、Windows | OpenAI/ChatGPT 高级用户、企业 | CLI/app-server、ChatGPT 认证、MCP 进程模型、跨平台修复 |
| Gemini CLI | 子智能体可靠性、AST 感知上下文、沙箱、浏览器智能体、TUI 渲染 | Google/Gemini/Code Assist、重度 MCP 用户 | Gemini 原生、智能体注册表、子智能体、sandbox.toml、nightly 发布 |
| GitHub Copilot CLI | 企业认证、BYOK 路由、智能体 hooks、内联图像、MCP | GitHub 企业/托管设备群 | Copilot+BYOK、按智能体作用域的声明式 hooks、快速补丁发布 |
| OpenCode | V2 对等性、提供商网关兼容性、配置优先级、自托管、桌面/TUI | 多提供商/自托管高级用户 | 模型无关、Drizzle/DBMS、插件/工具、V2 重写 |
| Pi | 持久运行时、SDK 可扩展性、OpenRouter 感知密钥模型、TUI/协议 | SDK 嵌入方、生产智能体开发者 | 持久运行时 API、按密钥模型发现、扩展 hooks、Nix/Bun |
| Qwen Code | 托管智能体、多智能体持久性、K8s 运行时、Web Shell | 企业/云托管智能体用户 | 托管智能体双路径、ACP/daemon、Java/Python SDK、Kubernetes |

最清晰的分野在于第一方、绑定模型的工具（Claude Code、Codex、Gemini CLI）与模型无关/企业路由工具（OpenCode、Pi、Copilot CLI BYOK）之间。第一方工具针对深度模型集成和桌面/CLI 对等性进行优化；模型无关工具则在提供商灵活性、托管配置和自托管上竞争。Qwen Code 最偏向基础设施，推动托管智能体架构和 Kubernetes 运行时，而 Pi 最偏向 SDK/持久运行时。

## 5. 社区动能与成熟度

OpenAI Codex 在此快照中拥有最高原始活动量：50 个已更新 issue、24 个已更新 PR、4 个讨论，其中 `/undo`（#9203）有 93 条评论和 479 👍——这是最强的单一社区需求信号。GitHub Copilot CLI 以四个版本展现出很高的发布速度，但评审带宽似乎被发布流程占用：0 个 PR 更新、35 个 issue 更新。Claude Code 尽管没有发布或 PR，仍保持高参与度：其最高票 issue 有 78 👍，已关闭的压缩 bug 有 30 👍，表明其成熟用户群体对静默上下文丢失很敏感。Gemini CLI 和 Qwen Code 通过 nightly 发布以及围绕智能体可靠性和托管智能体架构的 PR 集群快速迭代。OpenCode 和 Pi 很活跃，但仍处于变动更剧烈的阶段：OpenCode 面临 V2 对等性疲劳，而 Pi 正在扩展其持久运行时和 SDK 表面。总体成熟度最高的是 Claude Code、Codex、Copilot CLI 和 Gemini CLI；OpenCode、Pi 和 Qwen Code 迭代很快，但仍在稳定核心语义。

## 6. 趋势信号

- **智能体可靠性如今已是基本门槛。** 虚假成功状态、卡死、会话污染和上下文丢失在 Gemini、Codex、Pi、OpenCode 和 Claude 中反复出现。开发者在增加更多自主性之前，需要确定性的终止、取消、恢复和可观测性。
- **权限模型需要显式优先级。** Claude 的 hook 覆盖 bug、Gemini 的沙箱规则、Copilot 的 hook 组合以及 OpenCode 的 webfetch 拒绝都指向同一需求：有文档记录的优先级和可审计的强制执行。
- **成本与容量韧性是产品功能。** 跨模型回退、BYOK 路由、感知配额的模型目录、预算上限和支出可见性出现在 Codex、OpenCode、Claude、Copilot 和 Pi 中。速率限制和容量错误如今是一等 UX 失败。
- **多智能体编排正在进入生产环境。** Qwen 的托管智能体架构、Gemini 的子智能体可观测性、Codex 的 MCP 进程泄漏以及 Pi 的按请求 taskId 请求，表明市场需要智能体身份、团队消息传递、共享内存和资源隔离。
- **企业可管理性是一个竞争维度。** 托管配置发现、Entra/WAM 认证、策略韧性和弃用横幅正在阻塞 Copilot、OpenCode、Codex 和 Qwen 中的托管设备群。
- **跨平台一致性仍是主要差异化因素。** Windows/WSL 是 Codex、Copilot、OpenCode、Pi 和 Claude 的主要 bug 集群；Linux 打包和 Wayland 影响 Qwen 和 Gemini；macOS Gatekeeper 和崩溃影响 Copilot 和 Codex。
- **MCP 仍在成熟中。** 工具数量上限、OAuth/协议保持最新、持久 MCP 生命周期和按调用排除服务器在 Gemini、Copilot、Codex、Qwen、OpenCode 和 Claude 中都很活跃。
- **静默失败越来越不可接受。** LSP 过滤、Bash 转义、文件编码、被丢弃的会话缺陷和图像附件丢失都会侵蚀信任。开发者想要显眼、可操作的错误，而不是表面成功。
- **TUI/终端用户体验仍然重要。** 闪烁、剪贴板、调整大小、键盘历史和终端状态恢复在 Gemini、Pi、Copilot、OpenCode 和 Qwen 中反复出现。对于 CLI 智能体而言，终端就是产品界面。
- **模型无关和 BYOK 需求正在上升。** OpenCode 的回退请求、Pi 的按密钥 OpenRouter 发现以及 Copilot 的 BYOK 路由回归表明，企业和高级用户越来越希望在不让智能体质量下降的情况下实现提供商可移植性。

**给技术决策者的底线结论：** 优先考虑持久会话状态、显式权限优先级、提供商回退、成本护栏和跨平台 CI。这些不再是差异化因素——它们是生产级智能体 CLI 工作流的最低要求。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills — 社区亮点报告
**来源：** github.com/anthropics/skills · **数据截至：** 2026-10-13

> **数据说明：** PR 数据流对每一条记录都提供 `Comments: undefined`，因此无法从该数据集中还原 PR 级评论数。下文的 PR 排名因此遵循 API 所称的“按评论数排序”顺序，并按更新时间新近度、issue 关联和变更范围加权。Issue 排名使用已验证的评论数。

---

## 1. 热门 Skills 排名

| # | Skill / PR | 作用 | 讨论亮点 | 状态 |
|---|---|---|---|---|
| 1 | **mcp-builder：支持 MCP ≥2.0** — [PR #1742](https://github.com/anthropics/skills/pull/1742) | 适配 `connections.py` 中 `streamable_http_client` 的重命名，并将自定义请求头移至 `create_mcp_http_client` / `http_client`。 | 直接关闭 #1668；MCP 2.0 的破坏性变更是 builder 工具链中影响最大的单一回归。更新于 2026-10-08。 | 开放中 |
| 2 | **skill-creator：隔离触发器评测 / Windows + 运行时故障** — [PR #1298](https://github.com/anthropics/skills/pull/1298) | 修复 worker 交叉匹配、Windows 上子进程管道 `select()` 失败，以及运行时错误被评分判定为非触发的问题。 | 与 issue #1352 和 #1383 重叠；“触发器假阴性率”这一类问题是仓库中报告最多的缺陷族。 | 开放中 |
| 3 | **skill-creator：加固评测查看器** — [PR #1961](https://github.com/anthropics/skills/pull/1961) | 修复 `generate_review.py` + `viewer.html` 中的脚本逃逸、DNS rebinding、跨站 POST 以及转义缺口。 | 对渲染不可信模型输出的本地查看器进行安全加固；与 issue #1394（属性安全的 `escapeHtml`）互补。 | 开放中 |
| 4 | **proofcore-contract-auditor** — [PR #1771](https://github.com/anthropics/skills/pull/1771) | 新的 Agent Skill，用于静态分析 Solidity/Rust 合约，并在 TON 上锚定 Merkle 审计证明。 | 代表“垂直领域 Skill”浪潮；引发了对官方 Skills 内部引入第三方协议依赖的疑问。 | 开放中（草案类） |
| 5 | **md2video-audio** — [PR #1703](https://github.com/anthropics/skills/pull/1703) | 零成本流水线，将 Markdown → Marp 幻灯片 → MP4，并带合成配音。 | 热门的“内容生产”方向；讨论集中在运行时成本和外部工具链（Marp、TTS）的可复现性。 | 开放中 |
| 6 | **docx：LibreOffice 超时处理** — [PR #1792](https://github.com/anthropics/skills/pull/1792) | 在 `soffice` 超时时返回错误，并在声称成功前验证 `w:ins`/`w:del` 是否已移除。 | 属于文档 Skill 可靠性讨论串的一部分，与 [PR #1734](https://github.com/anthropics/skills/pull/1734)（孤立的 docx 评论）并列。 | 开放中 |
| 7 | **notion-spec-to-implementation + quantitative-resume-auditor** — [PR #1245](https://github.com/anthropics/skills/pull/1245) | 将规格说明转为带验收标准的 Notion 任务；按量化标准审计简历。 | 存续时间最长的活跃 PR（创建于 2026-06-02，更新于 2026-09-30）——体现了工作流自动化需求。 | 开放中 |
| 8 | **skill-creator：直接执行 `package_skill.py`** — [PR #1681](https://github.com/anthropics/skills/pull/1681) | 修复独立运行打包器时的 `ModuleNotFoundError: scripts.quick_validate`；更新过时的 CLI 文档。 | 与 #1742 同一作者；表明贡献者在打包路径上的持续投入。 | 开放中 |

**观察：** 前 20 名中的每一条都是 **开放中** —— 可见集合中没有任何已合并 PR。相对于涌入的新 Skills 数量，提交队列既深且推进缓慢。

---

## 2. 社区需求趋势

提炼自热门 Issue（评论数已验证）：

1. **Skill 编写与评测工具可靠性——主导性主题。**
   - [#556](https://github.com/anthropics/skills/issues/556)（12 条评论，👍7）：`run_eval.py` 从不触发 Skills——所有查询的触发率均为 0%。
   - [#1352](https://github.com/anthropics/skills/issues/1352)（4，👍2）：并行 worker 交叉匹配 skill UUID → 触发率系统性错误。
   - [#1383](https://github.com/anthropics/skills/issues/1383)（4）：六个可复现的 `skill-creator` 故障，包括目录布局不匹配和基准差值方向颠倒。
   - [#202](https://github.com/anthropics/skills/issues/202)（8，已关闭）：`skill-creator` 应重写为操作性 skill，而非开发者文档。

2. **安全与信任边界。**
   - [#492](https://github.com/anthropics/skills/issues/492)（46 条评论，👍2）——讨论最多的单个 issue：社区 Skills 以 `anthropic/` 命名空间发布，使冒充和权限提升成为可能。
   - [#1394](https://github.com/anthropics/skills/issues/1394)：评测查看器中的显示路径 XSS。
   - [#1175](https://github.com/anthropics/skills/issues/1175, CLOSED)：访问控制是否应放在 `SKILL.md` 内。

3. **分发与组织级共享。**
   - [#228](https://github.com/anthropics/skills/issues/228)（16，👍8）：在 Claude.ai 中实现组织级 skill 共享，而非手动交换 `.skill` 文件。
   - [#189](https://github.com/anthropics/skills/issues/189)（6，👍9）：`document-skills` 与 `example-skills` 安装内容相同 → 重复加载上下文。

4. **上下文窗口经济学。**
   - [#1487](https://github.com/anthropics/skills/issues/1487)：`claude-api` 在一次工具调用中主动注入约 156k tokens。
   - [#1329](https://github.com/anthropics/skills/issues/1329)：提出 `compact-memory` skill，使用符号表示法表示 agent 状态。

5. **治理 / 质量门禁类 Skills。**
   - [#412](https://github.com/anthropics/skills/issues/412, CLOSED) 和 [#1385](https://github.com/anthropics/skills/issues/1385)：策略执行、对抗性审查、交付验证流水线。

**最受需求的新 Skill 方向：** skill QA/安全审计、组织分发工具、上下文高效的记忆表示，以及工作流转换（规格 → 实现，文档 → 媒体）。

---

## 3. 高潜力待处理 Skills（活跃、未合并）

按活动新近度 + issue 关联度排名——最有可能接下来落地：

| PR | Skill | 为何可能落地 |
|---|---|---|
| [#1742](https://github.com/anthropics/skills/pull/1742) | mcp-builder MCP ≥2 支持 | 更新于 2026-10-08；修复了针对已发布依赖的具体且可复现的破坏。 |
| [#1961](https://github.com/anthropics/skills/pull/1961) | skill-creator 评测查看器加固 | 更新于 2026-10-11（前 20 名中最新的活动）；安全修复通常会被快速处理。 |
| [#1298](https://github.com/anthropics/skills/pull/1298) | skill-creator 触发器评测隔离 | 更新于 2026-09-16；与独立提交的 issue #1352/#1383 收敛。 |
| [#1681](https://github.com/anthropics/skills/pull/1681) | skill-creator 独立打包器 | 更新于 2026-10-08；范围窄、风险低、单模块修复。 |
| [#1792](https://github.com/anthropics/skills/pull/1792) | docx 超时/验证 | 更新于 2026-09-25；纠正错误的成功报告——明确的正确性收益。 |
| [#1730](https://github.com/anthropics/skills/pull/1730) | claude-api 死链修复 | 更新于 2026-10-04；URL 已验证 HTTP 200，极易合并。 |
| [#1977](https://github.com/anthropics/skills/pull/1977) | algorithmic-art `wrapAround()` | 更新于 2026-10-07；修复 #1897，diff 极小。 |
| [#1976](https://github.com/anthropics/skills/pull/1976) | webapp-testing 元素发现 | 更新于 2026-10-07；修复 #1891，diff 极小。 |

*具有真实势头但需要更重审查的新 skill 提案：* [#1771](https://github.com/anthropics/skills/pull/1771)（Web3 审计）、[#1703](https://github.com/anthropics/skills/pull/1703)（md2video-audio）、[#1615](https://github.com/anthropics/skills/pull/1615)（scnet-hpc）、[#1245](https://github.com/anthropics/skills/pull/1245)（Notion/简历）。

---

## 4. Skills 生态洞察

**社区的需求并不集中在发明新 Skills，而是集中在让 Skill 生命周期变得可信——修复 skill-creator 评测工具链，加固 Skill 安全与命名空间来源，并降低上下文成本——而新领域 Skills 的涌入速度已超过维护积压的消化能力。**

---

---

# Claude Code 社区摘要 — 2026-10-13

## 1. 今日亮点

过去 24 小时内没有新的发布或 pull request 落地，因此社区活动完全由 issue 驱动。队列顶部被长期存在、高互动量的讨论串占据：一个 40 条评论、要求允许 Claude 写入/更新项目文件的功能请求（#16550，78 👍），以及一个现已关闭的 bug 报告，关于空闲压缩静默丢弃工作上下文（#98747，30 👍）。第二组痛点正围绕 **worktree/sandbox 权限强制执行** 形成——Bash 防护覆盖显式的 `PreToolUse: allow` 钩子，并拒绝不含 git 的命令——同时还有 Bash 转义、LSP 结果过滤以及 Desktop/VS Code 会话生命周期中平台特有的正确性 bug。

## 2. 发布

过去 24 小时内没有。

## 3. 热点 Issue

1. **[#16550](https://github.com/anthropics/claude-code/issues/16550) — 允许 Claude 写入/更新项目文件** `OPEN` `enhancement, area:core, area:desktop` · 40 条评论 · 78 👍
   这是目前讨论量遥遥领先的未关闭条目，最早可追溯到 1 月，至今仍在更新。极高的 👍/评论比表明这是广泛的潜在需求，而非争议性辩论——用户希望文件写入能力成为一等公民，并将其定位为 core/desktop 能力，而不是变通方案。

2. **[#98747](https://github.com/anthropics/claude-code/issues/98747) — 空闲压缩静默丢弃工作上下文（2.1.286）** `CLOSED` `bug, platform:macos, area:core` · 22 条评论 · 30 👍
   自 2.1.286 起，空闲会话会在 prompt 缓存过期前被压缩，没有退出选项，也没有警告，而且该事件被记录为“manual”。对于长时间运行的会话，这会丢弃会话建立时所依赖的上下文基础。虽然现已关闭，但互动程度表明用户对静默上下文丢失非常敏感。

3. **[#91495](https://github.com/anthropics/claude-code/issues/91495) — 内置浏览器忽略“Allow all websites”站点权限** `OPEN` `bug, platform:macos, area:permissions, area:browser-extension, area:desktop` · 19 条评论 · 23 👍
   这是 Claude Code Desktop 中的权限策略违规：应用级别的允许列表未被嵌入式浏览器遵守。较高的评论数加上可观的 👍 表明，这正在阻碍真实的浏览器自动化工作流，而不是外观上的小问题。

4. **[#88561](https://github.com/anthropics/claude-code/issues/88561) — Bash 工具在 shell 解析前将 `\\` 折叠为 `\`** `OPEN` `bug, has repro, platform:windows, area:bash` · 7 条评论 · 4 👍
   反斜杠折叠发生在单引号、双引号和带引号的 heredoc 内部，破坏了 POSIX 关于单引号保留反斜杠的保证。会静默损坏正则表达式和 Windows 路径——这是一个正确性 bug，在用户怀疑到 harness 之前会耗费数小时调试。有复现。

5. **[#68083](https://github.com/anthropics/claude-code/issues/68083) — Desktop “Auto-fix CI and address comments”开关未应用于 `gh` 创建的 PR，且未持久化** `OPEN` `bug, platform:macos, area:desktop` · 7 条评论 · 8 👍
   该全局开关既不会应用于从本地会话通过 `gh` 打开的 PR，也不会持久化到 `claude_desktop_config.json` 中。作者明确将其与已关闭的 #51765 区分开来，将其定性为现有配置损坏，而不是新请求——这表明桌面端配置语义需要澄清。

6. **[#78263](https://github.com/anthropics/claude-code/issues/78263) — 为 auto-mode 分类器添加 API fallback** `OPEN` `enhancement, area:model, area:permissions` · 5 条评论 · 4 👍
   一次 Sonnet 5 1M 故障导致运行停止，因为 auto-mode 分类器没有 fallback。用户希望自动降级到另一个模型，而不是直接硬停止——这是一个原本不可见的子系统中的韧性缺口，会直接中断智能体式工作。

7. **[#95122](https://github.com/anthropics/claude-code/issues/95122) — Worktree 隔离拒绝不含 git 的 Bash 命令（3 天内 75 次拒绝）** `OPEN` `bug, has repro, area:bash, area:sandbox, platform:wsl` · 3 条评论
   在 2.1.272/2.1.273 的 worktree 隔离会话中，不包含 `git` 且仅引用 worktree 内部路径的 Bash 调用会被拒绝，这与 2.1.257 的 changelog 相矛盾。量化的拒绝次数有力地说明 sandbox 防护范围过宽。

8. **[#100385](https://github.com/anthropics/claude-code/issues/100385) — Worktree Bash 防护覆盖显式的 `PreToolUse` `permissionDecision: allow`** `OPEN` `bug, has repro, area:bash, area:hooks, area:sandbox` · 1 条评论 · 1 👍
   钩子的 allow 判定先被记录到 transcript 中，*然后*才发生拒绝，而且没有任何设置或规则能够覆盖它。这破坏了“钩子对权限拥有最终决定权”的契约——对于依赖策略钩子的团队来说是一个严重问题。

9. **[#101476](https://github.com/anthropics/claude-code/issues/101476) — LSP 工具在 gitignored 路径中丢弃 `findReferences`/`goToDefinition` 结果** `OPEN` `bug, has repro, area:tools, area:lsp` · 1 条评论
   `incomingCalls` 和 `hover` 返回相同位置，证明 gopls 已对它们建立索引——过滤发生在 Claude Code 中，而 `respectGitignore: false` 没有效果。在 monorepo 和 vendored 目录树中会静默丢失代码智能信息。

10. **[#101650](https://github.com/anthropics/claude-code/issues/101650) — Orchestrating 会话无视已写入的 agent 限制，烧掉每周 Max 额度的约 12%，未合并任何内容** `OPEN` `bug, area:cost, area:model, area:agents` · 1 条评论
    一个 Opus 协调器运行了约为项目文档所载 6-agent 并发上限的 1.5–2 倍，然后对自己造成的负载超时重试了两个小时。这是一个最坏情况下的成本/护栏案例，把 agent 限制、超时处理和预算可见性串联在一起。

## 4. 关键 PR 进展

无——过去 24 小时内没有 pull request 更新。

## 5. 热门讨论

本周期未提供讨论数据；本节省略。

## 6. 功能请求趋势

- **自动系统的退出选项与 fallback 控制。** 空闲压缩（#98747）和 auto-mode 分类器（#78263）都触发了对优雅降级或显式禁用开关的请求。用户接受自动化，但不接受没有逃生通道的自动化。
- **Agent 编排护栏与成本治理。** #101650 要求强制执行项目声明的并发/agent 限制，并更好地处理自己造成的超时。预计会有更多关于预算上限和每会话支出可见性的诉求。
- **Desktop 与终端对齐。** #100913 要求无头会话交接（prompt + model + effort，无需点击），以避开重新发送上下文的成本；#68083 要求桌面端配置真正持久化并生效。Desktop 正在向“可配置的终端”收敛。
- **Mods/插件 API 扩展。** #101377（通过 `tool.call` 钩子按调用设置 sandbox 拒绝规则）和 #99425（为插件伙伴增加吉祥物槽位）表明插件接口面仍然年轻，并且在安全和 UX 两端都有人在试探。
- **更细粒度的 sandbox 与钩子权威。** #100385 和 #95122 都指向同一方向：用户希望有清晰、有文档的优先级顺序，即 hooks > settings > guard heuristics，并且没有静默覆盖。
- **工具语义正确优先于新增工具。** LSP gitignore 处理（#101476）、Bash 反斜杠处理（#88561）和 MCP OAuth 资源编码（#101205）都是要求现有工具按文档行为运行的请求。

## 7. 开发者痛点

- **静默且具破坏性的状态变更。** 空闲压缩丢弃上下文（#98747）、shell 解析前 `\\` → `\` 损坏（#88561），以及 LSP 结果从 gitignored 路径中消失（#101476），都在没有清晰、可操作警告的情况下发生。
- **权限模型不一致。** Desktop 站点权限被忽略（#91495）、钩子显式的 `allow` 被 worktree 防护覆盖（#100385），以及 Chrome 扩展的批准弹窗在已批准的会话中再次触发（#101654），都会侵蚀对权限系统的信任。
- **会话生命周期脆弱。** 在单一账户上切换组织会丢弃正在运行的 Code 会话（#94507）、VS Code 扩展宿主重启会让面板永久冻结（#94743），并且页脚 PR 指示器会连续数周停留在早已合并的 PR 上（#100360）。
- **过宽的 sandbox 拒绝。** Worktree 隔离拒绝不含 git 的 Bash（#95122）以及 `PreToolUse` 覆盖 bug（#100385）迫使用户寻找变通方案，或彻底放弃隔离。
- **失控 agent 导致的成本爆炸。** #101650 量化了风险：一个违反自己写入限制的会话，可能消耗每周额度的可观一部分，却产生零合并输出。用户缺少实时断路器。
- **平台特有的配置损坏。** Windows 上 `~/.claude.json` 累积大小写不同的项目键，导致文件无法解析（#90438），再加上 Linux/Bun 上“Compacting conversation…”期间出现 SIGILL（#101648），表明配置和运行时稳定性仍因平台而差异巨大。

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区摘要 — 2026-10-13

## 今日亮点
过去 24 小时内没有发布新版本。活动主要由 50 个已更新 issue、24 个已更新 PR 和 4 个讨论构成，最强的社区信号集中在恢复 `/undo` 以及让 CLI 后台守护进程改为选择性启用。PR 活动主要是已关闭的机器人提交修复，涵盖代理安全、终端/TUI 正确性、线程权限恢复、WSL 剪贴板可靠性以及 Windows 构建/运行时隔离。

## 热点问题
1. **[#9203 — Please make "/undo" back](https://github.com/openai/codex/issues/9203)** — 93 条评论，479 👍。需求最高的会话安全请求：用户报告 Codex 会删除未跟踪文件或修改未提交的工作，并希望有可靠的

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区摘要 — 2026-10-13

## 1. 今日亮点

nightly 渠道上发布日较为平静：`v0.65.0-nightly.20261013.g51617c1bc` 发布了一个健壮性修复，使某个扩展未通过允许列表检查时，不再阻止其他扩展加载。Issue 活动主要集中在代理可靠性上——一份 P1 报告称子代理的 `MAX_TURNS` 中断被错误标记为 `GOAL` 成功，一个长期存在的通用代理挂起问题（8 👍），以及一组浏览器代理 bug。PR 方面，社区正在大力推动终端渲染稳定性（tmux 闪烁、调整大小刷新、调试控制台布局）以及核心文件处理中的边界情况正确性。

## 2. 发布

**v0.65.0-nightly.20261013.g51617c1bc** — [发布](https://github.com/google-gemini/gemini-cli/releases)
- `fix(cli)`：当一个扩展未通过允许列表检查时，继续加载其他扩展（[#29720](https://github.com/google-gemini/gemini-cli/pull/29720)）——避免单个有问题/配置错误的扩展拖垮整个扩展加载路径。
- [完整变更日志](https://github.com/google-gemini/gemini-cli/compare/v0.65.0-nightly.20261012.g9b6e0265d...v0.65.0-nightly.20261013.g51617c1bc)

## 3. 热门 Issue

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — MAX_TURNS 后的子代理恢复被报告为 GOAL 成功（P1，13 条评论）** — 今天流量最高的 Issue。`codebase_investigator` 在尚未进行任何分析前就触达轮次上限，却返回 `status: "success"` / `Termination Reason: "GOAL"`。这是一个信任正确性 bug：根据 status 分支的调用方会得到静默的假阳性，对自动化/评估流水线尤其有害。
2. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — 通用代理永久挂起（P1，8 👍）** — 本组中反应最多的 Issue。一旦 CLI 交给通用代理处理，即使是琐碎任务（创建文件夹）也会无限期挂起；当前 workaround 是指示模型不要使用子代理。高反应数表明问题具有广泛可复现性。
3. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — 零依赖 OS 沙箱与执行后意图路由（P2，9 条评论）** — 提议顺应 Gemini 3 对 bash 的原生亲和性，对 POSIX shell 工作流进行沙箱化，而不是限制它们。这是一场设计层面的讨论：如何在不以牺牲一方为代价的情况下兼顾模型能力与用户安全。
4. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini 不够充分地使用技能和子代理（P2，7 条评论）** — 虽属个例但持续存在：即使有描述完善的 `gradle`/`git` 技能可用，模型除非被明确告知，否则很少调用它们。这直接限制了自定义技能功能的价值。
5. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — 评估 AST 感知的文件读取、搜索与映射（P2 epic，7 条评论）** — 追踪 AST 感知工具能否减少轮次、降低错位读取，并减少 `codebase_investigator` 中的 token 噪声。已衍生出子 Issue（[#22746](https://github.com/google-gemini/gemini-cli/issues/22746)、[#22747](https://github.com/google-gemini/gemini-cli/issues/22747)），评估 tilth、glyph 和 ast-grep 等工具。
6. **[#22267](https://github.com/google-gemini/gemini-cli/issues/22267) — 浏览器 Agent 忽略 `settings.json` 覆盖项（P2）** — `AgentRegistry` 正确合并了配置，但 Browser Agent 将其丢弃（例如 `maxTurns`）。这是一个干净、可清晰复现的配置传递 bug。
7. **[#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — 浏览器子代理在 Wayland 上失败（P1）** — 在 Linux/Wayland 上失败时仍报告 `Termination Reason: GOAL`，加剧了 #22323 中的误导性状态问题。Linux 桌面用户实际上被阻止使用浏览器自动化。
8. **[#24246](https://github.com/google-gemini/gemini-cli/issues/24246) — 可用工具超过 128 个时出现 400 错误（P2）** — 代理不会缩减已启用的工具集，因此大型工具清单会直接导致请求失败。这对连接了许多服务器的 MCP 重度用户尤其相关。
9. **[#22672](https://github.com/google-gemini/gemini-cli/issues/22672) — Agent 应停止/劝阻破坏性行为（P2）** — 在存在更安全替代方案时，模型仍会使用 `git reset` / `--force`，并且需要围绕数据库等有状态资源设置更强的护栏。该 Issue 与安全相关，且由维护者提交。
10. **[#21763](https://github.com/google-gemini/gemini-cli/issues/21763) — `/bug` 报告不包含子代理上下文（P1）** — bug 报告只捕获主会话，导致子代理失败对维护者而言实际上无法复现。修复成本低，对提升分诊质量杠杆很高。

*同样活跃：* [#20079](https://github.com/google-gemini/gemini-cli/issues/20079)（符号链接的 agent `.md` 文件被忽略）、[#23571](https://github.com/google-gemini/gemini-cli/issues/23571)（tmp 编辑脚本散落在工作区各处）、[#22186](https://github.com/google-gemini/gemini-cli/issues/22186)（output-hook 崩溃）、[#22232](https://github.com/google-gemini/gemini-cli/issues/22232)（浏览器配置文件锁恢复）。

## 4. 关键 PR 进展

1. **[#29728](https://github.com/google-gemini/gemini-cli/pull/29728) — fix(cli): 防止快速 Ctrl-C 导致会话中毒（P1）** — 在工具执行/流式传输期间连续两次 `Ctrl+C`，会让未被 await 的 `cancelOngoingRequest()` 与 `/quit` 发生竞态。这是针对非常常见用户操作的高严重性 UX 修复。
2. **[#29725](https://github.com/google-gemini/gemini-cli/pull/29725) — fix(ui): 同步输出 + 隔离 spinner 渲染** — 将帧写入包裹在终端同步输出序列中，以消除 `tmux` 和其他多路复用器下的闪烁。直接解决了长期存在的 TUI 渲染抱怨。
3. **[#29695](https://github.com/google-gemini/gemini-cli/pull/29695) — fix(ui): 调试控制台高度计算与 `terminalBuffer` 闪烁** — 修复 `F12` 调试控制台布局，以及增量渲染和带安全缓存失效的静态历史记忆化。
4. **[#29644](https://github.com/google-gemini/gemini-cli/pull/29644) — fix(cli): 恢复终端宽度变化时的防抖静态 UI 刷新** — 重新引入调整大小时 100 ms 防抖的 `refreshStatic()`，在终端变宽/变窄时恢复正确的内联渲染。
5. **[#29703](https://github.com/google-gemini/gemini-cli/pull/29703) — fix(core): 使原子写入临时文件名保持在 NAME_MAX 限制内** — 215–255 字节的文件名会因 `.<uuid>.tmp` 后缀超过 255 字节限制而失败并报 `ENAMETOOLONG`。这是一个干净且诊断充分的文件系统正确性修复。
6. **[#29726](https://github.com/google-gemini/gemini-cli/pull/29726) — fix(sandbox): 解析命令名时忽略 trap wrapper** — 在 POSIX 上，被包裹的命令被误识别为 `trap`，从而静默禁用按命令生效的 `sandbox.toml` 规则和会话批准。这是一个有实际意义的安全相关修复。
7. **[#29722](https://github.com/google-gemini/gemini-cli/pull/29722) — fix(core): 将仅含空白字符的 UTF-16/UTF-32 文件视为空** — `isEmpty()` 总是将采样字节按 UTF-8 解码，留下 `trim()` 无法去除的 NUL。虽小，但代表了文件工具链中的编码边界情况。
8. **[#29727](https://github.com/google-gemini/gemini-cli/pull/29727) — fix(core): 报告被省略的 `list_directory` 条目** — 以前，当路径验证拒绝文件时，该工具会静默返回零条目；现在会在不放松限制的情况下，将拒绝信息同时呈现给模型和用户。
9. **[#29536](https://github.com/google-gemini/gemini-cli/pull/29536) — fix(grep): 防止命令行选项注入（已关闭）** — 通过为 `git grep` 和系统 `grep` 显式添加 `-e` 模式分隔符，加固 `packages/core/src/tools/grep.ts`（CWE-88）。这是工具层值得注意的安全加固。
10. **[#29568](https://github.com/google-gemini/gemini-cli/pull/29568) — fix(core): ChatRecordingService 中的仅追加增量补丁（已关闭，P1）** — 用增量 delta 和有界窗口化替代全历史 `{ $set: { messages } }` 重写和无界内存保留。这是长会话和 `/chat share` 的可扩展性改进。

*今天还合并/关闭：* [#29532](https://github.com/google-gemini/gemini-cli/pull/29532)（尊重 `RetryInfo` 的零延迟，而不是将其归类为终止性配额错误）、[#29535](https://github.com/google-gemini/gemini-cli/pull/29535)（在 Code Assist 认证中尊重允许的引导层级）、[#29638](https://github.com/google-gemini/gemini-cli/pull/29638)（激活时移除 VS Code 市场更新检查）、[#29635](https://github.com/google-gemini/gemini-cli/pull/29635)（为非 TTY 测试运行 mock `isHeadlessMode`）。

## 5. 热门讨论

本周期未提供讨论数据，因此省略本节。

## 6. 功能请求趋势

- **代理编排与可靠性** — 在 [#21409](https://github.com/google-gemini/gemini-cli/issues/21409)、[#22323](https://github.com/google-gemini/gemini-cli/issues/22323)、[#21968](https://github.com/google-gemini/gemini-cli/issues/21968)、[#20195](https://github.com/google-gemini/gemini-cli/issues/20195)、[#18287](https://github.com/google-gemini/gemini-cli/issues/18287) 中反复出现的主题：正确的终止语义、可靠的技能/子代理调用，以及最终实现带共享内存的并行子代理协作。
- **子代理可观测性** — [#22598](https://github.com/google-gemini/gemini-cli/issues/22598)（为子代理轨迹提供 `/chat share`）和 [#21763](https://github.com/google-gemini/gemini-cli/issues/21763)（在 `/bug` 中包含子代理上下文）都要求同一件事：让子代理内部状态可检查，而非不透明。
- **节省 token、结构感知的上下文** — AST epic（[#22745](https://github.com/google-gemini/gemini-cli/issues/22745)、[#22746](https://github.com/google-gemini/gemini-cli/issues/22746)、[#22747](https://github.com/google-gemini/gemini-cli/issues/22747)）加上 [#19561](https://github.com/google-gemini/gemini-cli/issues/19561) “Tactful Extraction”，目标是通过精准读取来优化每轮约 36.6k token 的基线。
- **持久化、基于文件的任务状态** — [#18836](https://github.com/google-gemini/gemini-cli/issues/18836) 和 [#21000](https://github.com/google-gemini/gemini-cli/issues/21000) 提议用磁盘上的 CRUD 任务跟踪替代上下文内的 `WriteToDo`，以经受上下文腐化和会话边界。
- **沙箱与安全护栏** — [#19873](https://github.com/google-gemini/gemini-cli/issues/19873)、[#22672](https://github.com/google-gemini/gemini-cli/issues/22672) 和 [#18397](https://github.com/google-gemini/gemini-cli/issues/18397) 都汇聚到按工作区策略、执行后意图路由，以及劝阻破坏性 git/数据库操作上。
- **终端 UX 与配置保真度** — 无闪烁调整大小（[#21924](https://github.com/google-gemini/gemini-cli/issues/21924)）、浏览器代理配置覆盖（[#22267](https://github.com/google-gemini/gemini-cli/issues/22267)），以及代理对自身 flags/热键的自我认知（[#21432](https://github.com/google-gemini/gemini-cli/issues/21432)）。

## 7. 开发者痛点

- **挂起且没有退出路径。** 通用代理永久阻塞（[#21409](https://github.com/google-gemini/gemini-cli/issues/21409)）以及 CLI 在交互式提示上卡住（[#22465](https://github.com/google-gemini/gemini-cli/issues/22465)，例如 `create vite`），迫使用户杀掉会话并丢失状态。
- **失败伪装成成功。** `MAX_TURNS` 被报告为 `Termination Reason: GOAL`（[#22323](https://github.com/google-gemini/gemini-cli/issues/22323)、[#21983](https://github.com/google-gemini/gemini-cli/issues/21983)），会破坏任何根据代理状态分支的自动化或评估。
- **代理忽略配置。** 浏览器 Agent 丢弃 `settings.json` 覆盖项（[#22267](https://github.com/google-gemini/gemini-cli/issues/22267)），以及 POSIX 上静默跳过沙箱规则（[#29726](https://github.com/google-gemini/gemini-cli/pull/29726)），削弱了用户依赖的配置模型。
- **工具数量上限。** 一旦启用的工具超过约 128 个，请求就会以 400 错误失败（[#24246](https://github.com/google-gemini/gemini-cli/issues/24246)）——这对 MCP 重度配置是真正的阻塞问题。
- **终端渲染噪声。** 反复出现闪烁、调整大小伪影和布局 bug 的报告（[#21924](https://github.com/google-gemini/gemini-cli/issues/21924)），目前正被一组开放的 PR 集中处理（[#29725](https://github.com/google-gemini/gemini-cli/pull/29725)、[#29695](https://github.com/google-gemini/gemini-cli/pull/29695)、[#29644](https://github.com/google-gemini/gemini-cli/pull/29644)）。
- **工作区卫生与破坏性命令。** 散落在各目录中的 tmp 编辑脚本（[#23571](https://github.com/google-gemini/gemini-cli/issues/23571)）以及下意识使用 `git reset --force`（[#22672](https://github.com/google-gemini/gemini-cli/issues/22672)）带来清理和安全负担。
- **核心工具中的边界情况正确性。** 符号链接的 agent 定义（[#20079](https://github.com/google-gemini/gemini-cli/issues/20079)）、错误的 `\n` 转义（[#22466](https://github.com/google-gemini/gemini-cli/issues/22466)）、UTF-16/32 空值检查（[#29722](https://github.com/google-gemini/gemini-cli/pull/29722)）以及 `NAME_MAX` 失败（[#29703](https://github.com/google-gemini/gemini-cli/pull/29703)），都指向一个仍有锋利边缘的文件系统层。
- **配额/重试误分类。** `RetryInfo` 的零延迟被当作终止性错误处理（[#29532](https://github.com/google-gemini/gemini-cli/pull/29532)），表明微小的错误处理细节会造成不成比例的用户侧失败。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI — 社区摘要
**日期：2026-10-13** | 来源：[github.com/github/copilot-cli](https://github.com/github/copilot-cli)

---

## 1. 今日亮点

Copilot CLI 快速发布了一系列补丁（v1.0.96 → v1.0.97-2），主要内容包括 **agent 作用域声明式 hooks**、**默认启用内联图像渲染**，以及修复了当仓库定义超过 100 个 agents 时的自定义 agent 发现问题。与此同时，issue 跟踪器被一组 **BYOK（Bring Your Own Key）模型路由回归**（Issues #5126、#5127、#5103）以及长期存在的 **Windows/企业认证失败**问题主导，后者仍在阻碍受管设备群。值得注意的是，过去 24 小时内没有 pull request 被更新。

---

## 2. 发布

本窗口期内有四个版本发布，其中 v1.0.97 是包含功能的版本，另有两个后续补丁。

### [v1.0.97-2](https://github.com/github/copilot-cli/releases/tag/v1.0.97-2)
- **已修复：** 自定义 agent 发现现在即使在仓库定义超过 100 个 agents 或 agent 文件嵌套很深时也能正常工作。

### [v1.0.97-1](https://github.com/github/copilot-cli/releases/tag/v1.0.97-1)
- 小幅修复和变更（未提供详细 changelog）。

### [v1.0.97-0](https://github.com/github/copilot-cli/releases/tag/v1.0.97-0)
- **新增：** 面向自定义 agent 的 agent 作用域声明式 hooks。
- **改进：** 内联图像现在默认在支持的终端上渲染，无需实验模式。可通过 `/settings inlineImages false` 禁用。
- **已修复：** Autopilot 现在会正确处理 `preToolUse` hook 的 `"ask"` 决策，而不是拒绝它。

### [v1.0.96](https://github.com/github/copilot-cli/releases/tag/v1.0.96) — 2026-10-12
- 交互式沙箱设置现在会建议可能的环境 secrets，并允许你在保存前添加 masking hosts。
- `/model` 和 `/config model` 中的模型 ID 现在不区分大小写，并会保存规范 ID。
- `/allow-all` 现在在企业策略仍在解析期间也可在启动时使用。
- 源数据中其余变更已截断。

---

## 3. 热点 Issue

*(按评论数、近期性和影响选出的 10 个值得关注项)*

1. **[#970 — macOS Gatekeeper blocks Copilot app under corporate security policy](https://github.com/github/copilot-cli/issues/970)** · OPEN · 8 条评论 · 👍 22
   每次 Homebrew 升级都会触发“Apple 无法验证‘copilot’不含恶意软件。”。这是跟踪器中反应最高的问题；每次都需要手动在“隐私与安全性”中批准。对 macOS 企业用户而言是持续存在的安装摩擦成本。

2. **[#5068 — Windows: MCP Entra sign-in fails with "scopes could not be safely validated"](https://github.com/github/copilot-cli/issues/5068)** · OPEN · 3 条评论 · 👍 12
   在每次全新的交互式登录时，都会阻碍对受 Entra 保护的 MCP 服务器（例如 Azure DevOps MCP）进行认证。鉴于企业对 Azure 的依赖，社区对此高度认同。

3. **[#3808 — Enhance prompt caching for Claude Sonnet to reduce latency and token costs](https://github.com/github/copilot-cli/issues/3808)** · OPEN · 3 条评论 · 👍 8
   长期存在的功能请求，指出 Anthropic 的 prompt caching 未用于重复上下文或长 system prompt。对重度用户有直接的成本/延迟影响。

4. **[#5114 — Session doesn't continue after hitting limit and waiting 5 hours](https://github.com/github/copilot-cli/issues/5114)** · OPEN · 3 条评论
   即使经过所述冷却时间，会话仍会被过期的 429 运行时上限阻塞，导致部分完成的工作被搁置。

5. **[#5100 — Session event delivery permanently fails after one 120s host-ack timeout](https://github.com/github/copilot-cli/issues/5100)** · OPEN · 1 条评论
   一次 `tool.execution_partial_result` ack 超时就会使整个长时间运行的会话不可用，直到 resume。这对 agentic 工作流是严重的可靠性问题。

6. **[#5103 — BYOK: sub-agents always use the session's wire API, causing 400s](https://github.com/github/copilot-cli/issues/5103)** · OPEN · 1 条评论
   `COPILOT_PROVIDER_WIRE_API` 会泄漏到运行不同系列模型的 sub-agents 中。核心 BYOK 路由缺陷。

7. **[#5126 — BYOK: built-in agents (explore, task, research) silently run on the session model](https://github.com/github/copilot-cli/issues/5126)** · OPEN (triage) · 0 条评论
   内置 agent 默认模型（例如 `gpt-5.6-luna`）被忽略，在 BYOK 配置中静默降低 agent 行为。

8. **[#5127 — BYOK: short model names aren't matched to provider models; misleading "No GitHub OAuth token" error](https://github.com/github/copilot-cli/issues/5127)** · OPEN (triage) · 1 条评论
   提供商限定 ID（`copilot/gpt-5.4-mini`）可用，但短名称会失败并给出令人困惑的认证错误。新提交——可能会获得关注。

9. **[#5109 — copilot-cli is unusable: managed account policy cannot be refreshed](https://github.com/github/copilot-cli/issues/5109)** · OPEN · 1 条评论
   当受管策略解析失败时，企业用户在信任提示处陷入重试循环。这是整个 CLI 在受管环境中的硬阻塞。

10. **[#5105 — macOS sandbox blocks Gradle daemon connection despite local networking allowed](https://github.com/github/copilot-cli/issues/5105)** · OPEN · 2 条评论
    即使允许出站 + 本地网络，Gradle 的 daemon/client localhost 握手仍被阻止，破坏了沙箱内的 JVM 构建。

**另需注意（近期已关闭）：** [#4960](https://github.com/github/copilot-cli/issues/4960)（企业自定义模型可选）、[#5101](https://github.com/github/copilot-cli/issues/5101)（`--add-github-mcp-tool issue_write` 未产生任何工具）、[#5079](https://github.com/github/copilot-cli/issues/5079)（2026-07-28 协议上的 MCP ping）、[#5082](https://github.com/github/copilot-cli/issues/5082)（Dataverse MCP RFC 8414 issuer 不匹配）、[#5118](https://github.com/github/copilot-cli/issues/5118)（Atlassian Rovo MCP auth probe 400）、以及 [#5030](https://github.com/github/copilot-cli/issues/5030)（ACP 模式无法启动自定义 agents）。

---

## 4. 关键 PR 进展

**过去 24 小时内没有 pull request 被更新（总计：0 项）。** 本摘要周期没有 PR 活动可总结——审查带宽似乎被 v1.0.97 发布节奏占用。

---

## 5. 热门讨论

*源数据集中未提供 GitHub Discussions 数据。本节省略。*

---

## 6. 功能请求趋势

从本窗口期的开放 issue 和已关闭请求中提炼：

- **BYOK / 自定义提供商对等性** — 最大的新兴主题。用户希望短模型名称可被解析、按 agent 的模型默认值被尊重、按 sub-agent 选择 wire API，以及提供商限定 ID 与短模型 ID 之间行为一致（[#5127](https://github.com/github/copilot-cli/issues/5127), [#5126](https://github.com/github/copilot-cli/issues/5126), [#5103](https://github.com/github/copilot-cli/issues/5103)）。
- **成本与延迟优化** — 明确要求为 Claude Sonnet 提供 Anthropic 风格的 prompt caching（[#3808](https://github.com/github/copilot-cli/issues/3808)）。
- **agent 执行的可观测性** — 请求提供 prompt/response 计时和执行时长跟踪（[#5125](https://github.com/github/copilot-cli/issues/5125)）。
- **MCP 协议时效性** — 支持 MCP `2026-07-28` Multi Round-Trip Requests / URL elicitation，目前已部分解决（[#4834](https://github.com/github/copilot-cli/issues/4834), [#5079](https://github.com/github/copilot-cli/issues/5079)）。
- **企业策略韧性** — 使 CLI 在受管策略仍在解析期间可用，并让 `/allow-all` 可用（[#5109](https://github.com/github/copilot-cli/issues/5109)，已在 v1.0.96 中部分解决）。

---

## 7. 开发者痛点

跟踪器中反复出现的挫败感，按频率和严重程度排序：

1. **企业及身份认证很脆弱。** Entra/WAM broker 登录在 Windows 上崩溃或失败（[#5088](https://github.com/github/copilot-cli/issues/5088) — `msalruntime.dll` 中的 `0xc0000005`），MCP OAuth scope 验证失败（[#5068](https://github.com/github/copilot-cli/issues/5068)），受管策略解析无限循环（[#5109](https://github.com/github/copilot-cli/issues/5109)）。这是受管部署中破坏性最强的单一类别。
2. **BYOK 模型路由不一致。** 内置 agents、sub-agents 和短模型名称的行为都不正确，常常静默失败或给出误导性错误消息（[#5126](https://github.com/github/copilot-cli/issues/5126), [#5127](https://github.com/github/copilot-cli/issues/5127), [#5103](https://github.com/github/copilot-cli/issues/5103)）。
3. **会话可靠性会永久下降。** 一次 ack 超时（[#5100](https://github.com/github/copilot-cli/issues/5100)）或一次过期的 rate-limit 上限（[#5114](https://github.com/github/copilot-cli/issues/5114)）就会使会话报废，强制 resume 并丢失上下文。
4. **沙箱对真实构建工具限制过严。** 即使启用了本地网络，Gradle daemon 的 localhost 连接仍被阻止（[#5105](https://github.com/github/copilot-cli/issues/5105)）。
5. **异常退出时终端状态未恢复。** 鼠标跟踪和焦点报告转义序列泄漏到父 shell，使选项卡在重置前无法使用（[#5120](https://github.com/github/copilot-cli/issues/5120)）——这是 CLI 工具中反复出现的抱怨。
6. **启动竞态条件会静默丢弃配置。** `--agent <name>` 在启动后约 1 秒于登录 / MCP 重载期间被取消选择（[#5124](https://github.com/github/copilot-cli/issues/5124)）。
7. **Windows 上的插件生命周期不可靠。** 卸载插件失败并显示 `Access is denied (os error 5)`（[#5119](https://github.com/github/copilot-cli/issues/5119)）。
8. **macOS 安装摩擦。** 每次 Homebrew 升级时的 Gatekeeper 验证警告都强制手动批准（[#970](https://github.com/github/copilot-cli/issues/970), 👍 22）。
9. **Hook 组合会静默丢失数据。** 当多个插件在 `userPromptSubmitted` 上返回 `additionalContext` 时，只有最后一个插件的文本会到达模型（[#5122](https://github.com/github/copilot-cli/issues/5122)）。
10. **ACP/非交互模式落后于交互模式。** `/rename` 报告成功但实际是空操作，自定义 agents 之前无法通过 task tool 启动（[#5115](https://github.com/github/copilot-cli/issues/5115), [#5030](https://github.com/github/copilot-cli/issues/5030)）。

---

*本摘要基于截至 2026-10-13 的 24 小时窗口内的 GitHub issue/release 元数据生成。Issue 数量：35 个已更新（显示 30 个），0 个 PR 已更新。*

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

**今日亮点**  
过去 24 小时内没有新版本发布，但围绕 V2 一致性、托管配置发现、提供商可靠性以及桌面端/Windows UX 的活动依然频繁。长期存在的原生模型回退请求（[#7602](https://github.com/anomalyco/opencode/issues/7602)）已关闭，获得 120 👍 和 31 条评论，表明对跨模型故障转移的需求强烈。PR 工作集中在配置优先级、compaction 循环修复、MCP 认证状态、OTLP 头解码以及 Code Mode 的权限门控上。

**版本发布**  
过去 24 小时内没有发布新版本。

**热门 Issue**
- [#7602](https://github.com/anomalyco/opencode/issues/7602) — **[已关闭] 原生模型回退/故障转移支持**。31 条评论，120 👍。允许在出错/速率限制时在不同模型之间回退；社区最强烈的韧性需求。
- [#14212](https://github.com/anomalyco/opencode/issues/14212) — **[已关闭] 支持更多 DBMS 用于 OpenCode 状态存储**。15 条评论，24 👍。Drizzle 迁移为自托管/企业使用场景打开了通往 Postgres 及其他数据库的大门。
- [#50168](https://github.com/anomalyco/opencode/issues/50168) — **[开放中] 桌面端缩放重启后重置为 100%**。7 条评论，3 👍。桌面端基础 UX 持久化缺陷；与 [#54990](https://github.com/anomalyco/opencode/issues/54990) 重复。
- [#52281](https://github.com/anomalyco/opencode/issues/52281) — **[开放中] Windows 子进程 `cmd.exe` 闪出控制台并抢占焦点**。6 条评论。即使设置了 `windowsHide`，仍会影响 git 探测和 MCP 启动；Windows 可用性摩擦很大。
- [#42135](https://github.com/anomalyco/opencode/issues/42135) — **[已关闭] `deepseek-v4-pro` 在 opencode-go `/responses` 上多轮请求失败**。7 条评论。网关/提供商兼容性问题：首轮正常，后续轮次失败。
- [#42091](https://github.com/anomalyco/opencode/issues/42091) — **[已关闭] `deepseek-v4-pro` 在 zen/go `/v1/messages` 上返回 400 `Empty input messages`**。6 条评论。Anthropic 兼容端点拒绝字符串内容，而 `deepseek-v4-flash` 正常。
- [#51107](https://github.com/anomalyco/opencode/issues/51107) — **[开放中] V2 不会从 `%ProgramData%\opencode` 加载托管配置**。3 条评论，6 👍。相比 V1，企业/管理员托管设置出现回归。
- [#53074](https://github.com/anomalyco/opencode/issues/53074) — **[开放中] v2 中未遵循 `/etc/opencode`**。3 条评论，1 👍。这是托管配置一致性问题的 Linux 对应项。
- [#42250](https://github.com/anomalyco/opencode/issues/42250) — **[已关闭] 加载失败的工具文件会导致整个会话/注册表崩溃**。4 条评论。单个损坏的 `.opencode/tool/*.ts` 文件就可能让项目无法使用。
- [#42259](https://github.com/anomalyco/opencode/issues/42259) — **[已关闭] 会话运行缺陷被静默丢弃**。3 条评论。UI 显示空闲，而不是暴露工具/解析器/提供商故障；可观测性差。

**关键 PR 进展**
- [#54921](https://github.com/anomalyco/opencode/pull/54921) — **[开放中] fix(core)：以最低优先级发现系统配置目录**。修复 [#51107](https://github.com/anomalyco/opencode/issues/51107) 和 [#53074](https://github.com/anomalyco/opencode/issues/53074)；恢复 V1 托管/系统配置一致性。
- [#54994](https://github.com/anomalyco/opencode/pull/54994) — **[开放中] fix(opencode)：停止 compaction 循环和陈旧的 compaction 重试**。关闭 [#54993](https://github.com/anomalyco/opencode/issues/54993)；停止上下文溢出后反复进行的 compaction。
- [#54976](https://github.com/anomalyco/opencode/pull/54976) — **[开放中] fix(core)：根据 webfetch 权限对 Code Mode 抓取进行门控**。关闭 [#54595](https://github.com/anomalyco/opencode/issues/54595)；确保 `"webfetch": "deny"` 适用于 Code Mode。
- [#54226](https://github.com/anomalyco/opencode/pull/54226) — **[已关闭] fix(mcp)：当工具调用因 401 被拒绝时将服务器标记为 `needs_auth`**。关闭 [#53942](https://github.com/anomalyco/opencode/issues/53942)；阻止 OAuth 刷新失败后 MCP 状态仍显示陈旧的“Connected”。
- [#54234](https://github.com/anomalyco/opencode/pull/54234) — **[已关闭] fix(cli)：在 v2 上恢复 `models --refresh` 标志**。恢复 V2 重写中丢弃的、已有文档记录的 V1 CLI 标志。
- [#55004](https://github.com/anomalyco/opencode/pull/55004) — **[开放中] fix(util)：解码 OTLP 头值**。关闭 [#55002](https://github.com/anomalyco/opencode/issues/55002)；修复 `Bearer%20test-token` 到达 OTLP 收集器而不是 `Bearer test-token` 的问题。
- [#54728](https://github.com/anomalyco/opencode/pull/54728) — **[开放中] feat(schema)：识别用于客户端生成的 branded ID**。参考 [#43886](https://github.com/anomalyco/opencode/issues/43886)；是客户端品牌激活和类型化 ID 的前提条件。
- [#52980](https://github.com/anomalyco/opencode/pull/52980) — **[开放中] fix(core)：保留被中断的工具检查点**。关闭 [#39565](https://github.com/anomalyco/opencode/issues/39565)；将检查点保留到异步清理和发布完成为止。
- [#52978](https://github.com/anomalyco/opencode/pull/52978) — **[开放中] fix(core)：通过环境读取工作区文件**。关闭 [#44568](https://github.com/anomalyco/opencode/issues/44568)；将工作区 FS 读取路由到 `Environment.files`。
- [#54949](https://github.com/anomalyco/opencode/pull/54949) — **[开放中] feat(tui)：提示起始高度、百分比尺寸和主屏幕布局设置**。关闭 [#54947](https://github.com/anomalyco/opencode/issues/54947)；增加 TUI 布局自定义，并计划移植到 v2。

**功能请求趋势**
- **提供商/模型韧性**：跨模型回退和故障转移仍是最突出的诉求（[#7602](https://github.com/anomalyco/opencode/issues/7602)）。
- **配置与存储可扩展性**：更多 DBMS 选项（[#14212](https://github.com/anomalyco/opencode/issues/14212)）以及系统/托管配置优先级（[#51107](https://github.com/anomalyco/opencode/issues/51107)、[#53074](https://github.com/anomalyco/opencode/issues/53074)）。
- **V2 迁移一致性**：托管配置、插件、CLI 标志、MCP 扁平条目和默认 agent 仍需进行一致性工作（[#48201](https://github.com/anomalyco/opencode/issues/48201)、[#37225](https://github.com/anomalyco/opencode/issues/37225)）。
- **桌面端/TUI 打磨**：缩放持久化（[#50168](https://github.com/anomalyco/opencode/issues/50168)）、纯文本模型的图像缩略图（[#54991](https://github.com/anomalyco/opencode/issues/54991)）、快速跳转侧边栏（[#32165](https://github.com/anomalyco/opencode/issues/32165)）以及 TUI 布局设置（[#54949](https://github.com/anomalyco/opencode/pull/54949)）。
- **插件/工具生态健壮性**：本地插件路径（[#49608](https://github.com/anomalyco/opencode/issues/49608)、[#46551](https://github.com/anomalyco/opencode/issues/46551)）以及损坏的工具文件导致注册表崩溃（[#42250](https://github.com/anomalyco/opencode/issues/42250)）。
- **隐私与权限**：保护 grep/glob 结果中的 `.env`（[#17073](https://github.com/anomalyco/opencode/issues/17073)），并对 Code Mode 抓取进行门控（[#54976](https://github.com/anomalyco/opencode/pull/54976)）。
- **提供商网关兼容性**：deepseek-v4-pro 问题（[#42135](https://github.com/anomalyco/opencode/issues/42135)、[#42091](https://github.com/anomalyco/opencode/issues/42091)）以及 Anthropic 兼容的工具使用（[#34331](https://github.com/anomalyco/opencode/issues/34331)）。
- **可靠性/性能**：空闲 CPU/RSS（[#42299](https://github.com/anomalyco/opencode/issues/42299)、[#42271](https://github.com/anomalyco/opencode/issues/42271)）、静默会话缺陷（[#42259](https://github.com/anomalyco/opencode/issues/42259)）以及服务器 400（[#54230](https://github.com/anomalyco/opencode/issues/54230)）。
- **国际化**：zh/zht 字典缺少 93 个英文键，导致 UI 混用语言（[#53858](https://github.com/anomalyco/opencode/issues/53858)）。

**开发者痛点**
- **V2 回归疲劳**：配置优先级、插件路径、CLI 标志、默认 agent 和 MCP 扁平配置不断打破 V1 预期。
- **Windows 摩擦**：可见的 `cmd.exe` 闪窗和焦点抢占（[#52281](https://github.com/anomalyco/opencode/issues/52281)）、缩放重置（[#50168](https://github.com/anomalyco/opencode/issues/50168)）以及托管配置路径缺口（[#51107](https://github.com/anomalyco/opencode/issues/51107)）。
- **静默失败**：会话缺陷被丢弃（[#42259](https://github.com/anomalyco/opencode/issues/42259)）、插件路径被静默丢弃（[#46551](https://github.com/anomalyco/opencode/issues/46551)）、MCP 认证状态过期（[#54226](https://github.com/anomalyco/opencode/pull/54226)）以及空 GLM 流（[#42325](https://github.com/anomalyco/opencode/issues/42325)）。
- **提供商/网关不兼容**：deepseek-v4-pro 多轮和 `/v1/messages` 失败（[#42135](https://github.com/anomalyco/opencode/issues/42135)、[#42091](https://github.com/anomalyco/opencode/issues/42091)）。
- **资源泄漏/性能**：空闲 serve CPU 突发（[#42299](https://github.com/anomalyco/opencode/issues/42299)）、高 RSS（[#42271](https://github.com/anomalyco/opencode/issues/42271)）以及 Bun 段错误（[#54228](https://github.com/anomalyco/opencode/issues/54228)）。
- **插件/工具加载脆弱性**：一个损坏的工具文件就可能导致整个注册表崩溃（[#42250](https://github.com/anomalyco/opencode/issues/42250)）；本地插件文件路径被拒绝或丢弃（[#49608](https://github.com/anomalyco/opencode/issues/49608)、[#46551](https://github.com/anomalyco/opencode/issues/46551)）。
- **模型回退限制**：没有跨模型故障转移意味着长时间运行的 agent 仍会在速率限制或提供商错误时停滞（[#7602](https://github.com/anomalyco/opencode/issues/7602)）。
- **配置发现不一致**：V2 中未一致遵循 `/etc/opencode`、`%ProgramData%\opencode` 和 macOS 托管路径（[#53074](https://github.com/anomalyco/opencode/issues/53074)、[#51107](https://github.com/anomalyco/opencode/issues/51107)）。
- **权限/安全缺口**：通过 grep/glob 暴露 `.env`（[#17073](https://github.com/anomalyco/opencode/issues/17073)）以及 Code Mode 绕过 webfetch deny（[#54976](https://github.com/anomalyco/opencode/pull/54976)）。

---

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi 社区文摘 — 2026-10-13

## 今日亮点
Pi **v1.2.0** 发布，带来按密钥的 OpenRouter 模型发现：模型列表、上下文大小、最大输出和定价现在会反映你的 OpenRouter 密钥实际具备的能力，包括在 `models.json` 中配置的区域端点。与此同时，问题跟踪器上主要是 Windows/WSL 终端边缘情况（#6187、#9361）、会话恢复保真度问题（#10785、#10864），以及一批 durable-runtime API 请求（#10845、#10846），这标志着 SDK 在生产环境中的使用正在增长。

## 发布

**[v1.2.0](https://github.com/earendil-works/pi/releases/tag/v1.2.0)**
- **按密钥的 OpenRouter 模型列表**——配置 OpenRouter 密钥后，Pi 现在只列出该密钥被允许使用的模型，并直接从密钥的权限中提取上下文大小、最大输出 token 数和价格（包括 `models.json` 中定义的区域端点）。这填补了一个长期存在的缺口：用户可能选中其密钥护栏会拒绝的模型。相关工作通过 [PR #10569](https://github.com/earendil-works/pi/pull/10569) 和 [#10672](https://github.com/earendil-works/pi/pull/10672) 落地。

## 热门 Issue

1. **[#6187 [CLOSED] Pi login hangs in WSL after browser-based GitHub Copilot device authorization](https://github.com/earendil-works/pi/issues/6187)** — 28 条评论。这是该时间段内讨论量最高的问题：WSL 客户端始终观察不到已完成的 Copilot 设备授权，并无限期挂起。已关闭，可能由 [PR #10694](https://github.com/earendil-works/pi/pull/10694) 中的 OAuth 轮询边距修复解决；该修复针对导致持续 `slow_down` 失败的 WSL 时钟漂移。

2. **[#9361 [OPEN] Windows: settings `shellPath` is non-deterministically ignored when extensions are loaded](https://github.com/earendil-works/pi/issues/9361)** — 14 条评论。`~/.pi/agent/settings.json` 中有效的 `shellPath` 会在任意扩展加载后被静默绕过，回退到 Git Bash 检测，然后回退到 `PATH` 中的 WSL `System32\bash.exe`。这种非确定性使其尤其难以诊断；这是当前最严重的 Windows 可靠性未解决问题。

3. **[#9688 [CLOSED] Regression: clipboard copy doesn't work anymore](https://github.com/earendil-works/pi/issues/9688)** — 10 条评论，2 👍。针对 #9618 的修复把 OSC 52 发送范围收窄到仅 SSH 会话，破坏了容器/交互式用例。这是定向修复引入回归的典型例子——社区的 2 个赞凸显了它对远程开发工作流的影响。

4. **[#9391 [CLOSED] Stale signed thinking blocks replayed after compaction (Anthropic `prefix_binding_mismatch`)](https://github.com/earendil-works/pi/issues/9391)** — 7 条评论。手动 compaction 后，每轮都会重新发送 15 个过期的 thinking 块，而 Anthropic 每次都会丢弃它们。长会话中既有静默上下文丢失，又有每请求开销——对于任何进行长时间 Anthropic 工作的用户来说，这是一个正确性问题。

5. **[#10645 [OPEN][inprogress] `resizeImage` resolves null in compiled (Bun) executables](https://github.com/earendil-works/pi/issues/10645)** — 7 条评论。自 0.87.x 起，独立二进制文件中的所有图像附件都被静默省略；通过捕获出站 provider 请求复现。标记为进行中；[PR #10834](https://github.com/earendil-works/pi/pull/10834) 中提出了回退修复。

6. **[#10705 [OPEN] `abort()` from `agent_settled` resolves but a deferred continuation still runs](https://github.com/earendil-works/pi/issues/10705)** — 4 条评论。在 SDK 1.1.0 中，中止会话的宿主仍可能看到之后发起新的 provider HTTP 请求。对于需要硬停止保证（计费、安全、速率限制）的 SDK 嵌入者来说，这很重要。

7. **[#10785 [OPEN][prio:p0] Resuming a session drops dynamically activated tools](https://github.com/earendil-works/pi/issues/10785)** — 3 条评论。使用 `defaultActive: false` 或 `exposure: "deferred"` 注册的工具会在恢复时停用，然后被记录为已从工具列表中移除。任何依赖延迟工具暴露的长时间运行 agent 工作流都会在重启后中断。

8. **[#10628 [OPEN] OSC 52 copy in fullscreen TUI is swallowed by tmux (no passthrough wrap)](https://github.com/earendil-works/pi/issues/10628)** — 3 条评论，3 👍。Pi 写入原始 OSC 52；tmux 会将其吸收到自己的粘贴缓冲区，而不是转发。这是该时间段内 👍 最多的未解决问题，反映出一个广泛的无头/SSH-over-tmux 用户群。

9. **[#10770 [OPEN] Default permissions of `.pi/agent/sessions` should be `0700`](https://github.com/earendil-works/pi/issues/10770)** — 2 条评论。会话转录默认以全局可读方式创建。一个很小但高杠杆的隐私/安全加固请求，实现方式直接。

10. **[#10798 [OPEN] Concurrent pi processes race managed npm install on shared `.pi/npm` prefix (no install lock)](https://github.com/earendil-works/pi/issues/10798)** — 2 条评论。并发的 pi 进程在共享 `.pi/npm` prefix 上竞争托管 npm 安装（没有安装锁）。并行的 subagent 子会话同时调用 `npm install --prefix <project>/.pi/npm` 会破坏托管包存储。随着多 agent fan-out 成为常见模式，这是一个结构性缺口。

## 关键 PR 进展

1. **[#10694 [OPEN] fix(ai): adapt OAuth device polling margin after `slow_down`](https://github.com/earendil-works/pi/pull/10694)** — 回应 #6187 背后的 WSL 时钟漂移根因；增大轮询间隔边距，使 Copilot OAuth 能够收敛，而不是在限流错误上循环。

2. **[#10528 [OPEN] refactor Nix packaging](https://github.com/earendil-works/pi/pull/10528)** — 清理 `package.nix`，移除 source map/类型声明，采用 `makeBinaryWrapper`，跳过版本检查。提升基于 Nix 的部署的可复现性。

3. **[#10855 [CLOSED] fix(ai): map minimal thinking to low on Xiaomi MiMo models](https://github.com/earendil-works/pi/pull/10855)** — MiMo 的 API 会以 400 拒绝 `reasoning_effort: "minimal"`；catalog 现在设置 `thinkingLevelMap: { minimal: "low" }`。修复 #10818。

4. **[#10569](https://github.com/earendil-works/pi/pull/10569) / [#10672 [CLOSED] feat(ai,coding-agent): filter OpenRouter models by key availability](https://github.com/earendil-works/pi/pull/10672)** — v1.2.0 头条功能背后的实现：模型通过已认证的 `GET /api/v1/models/user` 过滤，每个模型的上下文/输出/定价取自密钥视图，并遵守区域护栏。

5. **[#10847 [OPEN] fix: preserve legacy model catalog route on publish](https://github.com/earendil-works/pi/pull/10847)** — Publisher 在每次发布时替换旧 catalog 条目，破坏了 Pi 0.80.8–0.84.4 客户端（它们从 pi.dev 拉取，但仍使用 `openai-completions`）。修复 #9099。

6. **[#10718 [OPEN] fix(coding-agent): include system prompt in `--export` HTML](https://github.com/earendil-works/pi/pull/10718)** — 使非交互式导出与交互式 `/export` 保持一致，从当前分支恢复系统提示词，以便审计。

7. **[#10533 [CLOSED] fix(durable): reject waits that close a cycle](https://github.com/earendil-works/pi/pull/10533)** — 循环等待现在会在导致问题的等待处失败，而不是挂起，与现有的 self/owner-wait 行为一致。修复 #10411；这是 durable runtime 的重要健壮性胜利。

8. **[#10834 [CLOSED] fix(coding-agent): fall back after Bun image worker null](https://github.com/earendil-works/pi/pull/10834)** — 将 Bun 编译的 resize worker 返回的 `null` 视为未命中，并继续回退到 URL/http worker，从而恢复独立二进制文件中的图像附件（#10645）。

9. **[#10830 [CLOSED] fix(coding-agent): bound retries across successful tool turns](https://github.com/earendil-works/pi/pull/10830)** — 以前，成功的 `toolUse` 响应会重置 `retry.maxRetries`，因此交错出现 error/toolUse 的序列可能永远重试。现在重试在整个 turn 内受限。

10. **[#10820 [CLOSED] fix(ai): send `x-opencode-session` by endpoint, not provider id](https://github.com/earendil-works/pi/pull/10820)** — 泛化 #9326 的修复：现在根据 endpoint 而非硬编码的 provider-id 列表添加每对话路由 header，修复其他代理 OpenCode zen endpoint 的 provider。

*另请注意：* [#10739](https://github.com/earendil-works/pi/pull/10739) 修复了由 `pi.sendMessage(..., { triggerTurn: true })` 启动的运行会跳过 `before_agent_start` 的问题；此前这会在运行中破坏系统提示词，并破坏 provider 侧的提示缓存。

## 热门讨论

**想法 / 问答**

- **[#10841 [General] Where should a package put a one-shot pre-turn routing decision?](https://github.com/earendil-works/pi/discussions/10841)** — MatPilot 团队正在构建一个按 prompt 分类的层，由 Python `semantic-router` sidecar 支持（预算 <100 ms），用于决定运行哪个研究工作流，并询问异步、一次性的 pre-turn 决策应放在包生命周期的什么位置。尚无回复，但它是一个具体信号：扩展作者需要一个官方认可的“带副作用的异步 I/O 的 pre-turn hook”——在精神上与 [#10832](https://github.com/earendil-works/pi/issues/10832) 相关，后者请求包装/观察会话模型调用。

## 功能请求趋势

- **密钥感知、可选加入的模型发现。** v1.2.0 解决了按密钥的 OpenRouter 过滤；[#10866](https://github.com/earendil-works/pi/issues/10866) 请求增加 `modelDiscovery` 设置，使 `/model` 和 `/scoped-models` 可以排除仅通过环境变量启用的 provider。用户想要更紧凑、更少噪声的 catalog。[#10147](https://github.com/earendil-works/pi/issues/10147) 推动将 `/scoped-models` 合并进 `/model`。
- **扩展 API 表面扩张。** [#10832](https://github.com/earendil-works/pi/issues/10832)（为 OpenTelemetry 包装/观察模型调用）、[#10845](https://github.com/earendil-works/pi/issues/10845)（为每个模型请求标识 `taskId`/`purpose`）、[#10846](https://github.com/earendil-works/pi/issues/10846)（为 Worker 环境提供可注入的 daemon 进程）以及 [#10833](https://github.com/earendil-works/pi/issues/10833)（消息事件上的 session entry id）都请求更深入、结构化的可观测性 hook。
- **隐私与多进程安全。** [#10770](https://github.com/earendil-works/pi/issues/10770)（0700 会话目录）和 [#10798](https://github.com/earendil-works/pi/issues/10798)（共享 `.pi/npm` 的安装锁）都是为了让本地足迹在真实使用中安全——包括并行 subagent。
- **协议/终端兼容性。** [#10628](https://github.com/earendil-works/pi/issues/10628)（tmux OSC 52 透传）、[#10863](https://github.com/earendil-works/pi/issues/10863)（overlay 绘制在内联图像下方）、[#10862](https://github.com/earendil-works/pi/issues/10862)（鼠标选择下的硬件光标列）以及 [#10836](https://github.com/earendil-works/pi/issues/10836)（在 Anthropic OAuth 中保留大小写不同的工具名）构成了一连串稳定的 TUI 和 wire-protocol 打磨请求。
- **工具路由指引。** [#10860](https://github.com/earendil-works/pi/issues/10860) 请求内置 `grep`/`bash` 描述说明何时文本搜索是错误的工具——这是对工具选择的一个轻量 prompt-engineering 杠杆。

## 开发者痛点

- **WSL 和 Windows 仍然是摩擦最大的平台。** #6187（28 条评论）和 #9361（14 条评论）在该时间段内居首，涵盖 OAuth 时序、shell 解析和 PATH 回退行为。底层的 WSL 时钟漂移问题正在 OAuth 层打补丁，而不是从根源修复。
- **会话持久化在边缘场景有损。** 恢复会话会静默丢弃动态激活的工具（#10785），而一个存续时间超过其发布版本的会话，在 `pi update` 修剪旧构建后会丢失尚未加载的 provider（#10864：`Cannot find module`）。两者都是“直到出问题前一切看起来都正常”的故障模式。
- **静默降级而非响亮失败。** 图像附件在编译后的二进制文件中消失（#10645），MCP direct-exposure 工具被 `--tools` 静默丢弃（#10838），即使 tmux 吞掉了序列，剪贴板仍会闪现“Copied!”（#10628）。用户反复报告：操作实际未成功，却被告知成功。
- **取消和重试语义并不严密。** `abort()` 之后仍可能跟随一个活跃的 provider 请求（#10705），而交错出现的成功工具轮次可以无限重置重试预算（#10830）。对 SDK 嵌入者而言，这会直接转化为无界成本和控制流违规。
- **Anthropic 特有的 wire-format 脆弱性。** compaction 后重放的过期 thinking 块（#9391）以及 OAuth adapter 下的工具名大小写冲突（#10836），都要求用户理解 provider 内部行为才能绕过。
- **依赖范围与 catalog 漂移。** [#10865](https://github.com/earendil-works/pi/issues/10865) 显示 `^1.0.4` 解析到 `pi-codemode@1.2.0`，并在 Node 22.23.3 下加载失败；而 [#10847](https://github.com/earendil-works/pi/pull/10847) 显示 catalog 发布会覆盖旧客户端路由——跨 monorepo 的版本兼容管理仍是一笔反复出现的税。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区摘要 — 2026-10-13

## 今日亮点
2026-10-13 摘要主要由 Managed Agent 架构与多智能体持久性工作占据：一个高热度双路径提案（#12380）、Kubernetes 运行时追踪（#13395），以及关于成员间消息传递、基于 ACP/daemon 的 agent 身份、L4 Shell/MCP 生命周期的新 PR。最新 nightly 版本包含 managed-agent connector/broker 稳健性改进和更多 managed-agent 功能工作。与此同时，开发者正积极标记 CI checkout 不稳定、Linux Desktop 打包崩溃和 UI/渲染问题，将它们视为近期可靠性隐患。

## 版本发布
- **v0.25.1-nightly.20261012.18f80f5666** — [发布链接](https://github.com/QwenLM/qwen-code/releases/tag/v0.25.1-nightly.20261012.18f80f5666)  
  可见变更包括由 @wenshao 在 [#13330](https://github.com/QwenLM/qwen-code/pull/13330) 中提交的 `fix(managed-agent): connector and broker robustness from the #12692 R2 review`，以及至少一条在 release notes 中被截断的额外 managed-agent 功能条目。这是 nightly，不是稳定版。

## 热门 Issue
- **#12380 — proposal(serve): 定义 Managed Agent 双路径架构与分阶段交付** ([link](https://github.com/QwenLM/qwen-code/issues/12380)) — 56 条评论。关于持久化 Sessions、Workspace bindings、可恢复工具执行，以及在保留 TypeScript agent loop 的同时提供稳定 Web Shell 路径的核心架构讨论串。社区反应强烈，仍处于 need-discussion 状态。
- **#13395 — tracking(runtime): Kubernetes 工具运行时进展与跨平台交付门禁** ([link](https://github.com/QwenLM/qwen-code/issues/13395)) — 19 条评论。追踪 K8s 工具运行时交付和平台分发门禁相对于当前 main 基线的进展。对基础设施和发布准备度具有高参考价值。
- **#13492 — XML 工具调用恢复会丢弃包含带引号工具标记的外层调用** ([link](https://github.com/QwenLM/qwen-code/issues/13492)) — 11 条评论。核心工具调用解析 bug：带引号的工具标记已部分修复，但外层调用恢复仍待 PR #13579 完成。对 agent 可靠性很重要。
- **#13785 — Multi-Agent API: 公共契约上的 agent-identity 维度** ([link](https://github.com/QwenLM/qwen-code/issues/13785)) — 7 条评论。要求多智能体执行在现有公共契约上可归因、树形化且可中断。标记为 ready-for-human 和 need-discussion。
- **#13502 — 取消溯源：从 PR 13436 中推迟的五项决策** ([link](https://github.com/QwenLM/qwen-code/issues/13502)) — 6 条评论。涉及取消语义、旧式时间戳兼容性和终端记录。当前受阻；对会话管理正确性很重要。
- **#14011 — Harness SWE: 测试 Qwen Code 自身功能的 feature-type 基准任务** ([link](https://github.com/QwenLM/qwen-code/issues/14011)) — 5 条评论。提议为 todo tracking、plan mode、skills/workflows 和 subagent delegation 提供贴近实际的功能实现基准。这是新的且及时，用于衡量功能质量。
- **#13898 — Web Shell: 显示后台 subagent 的实时状态** ([link](https://github.com/QwenLM/qwen-code/issues/13898)) — 5 条评论。请求为后台 Agent 运行提供始终可见的状态：running/done、已用时间、剩余数量。直接解决多智能体可观测性问题。
- **#13837 — 聊天视图破坏混合 RTL/LTR 文本：代码块被强制为 LTR，无 bidi 隔离** ([link](https://github.com/QwenLM/qwen-code/issues/13837)) — 5 条评论。影响希伯来语/阿拉伯语与英文技术字符串混排。带有 `feature/need-help` 和 `welcome-pr`，表明维护者开放态度。
- **#14004 — 将 agentic harness 与 LLM 订阅分离，以消除利益冲突** ([link](https://github.com/QwenLM/qwen-code/issues/14004)) — 4 条评论。认为同一供应商既控制 token 定价/配额，又控制消耗它们的 harness。这是一场政策/产品讨论，状态为 need-discussion。
- **#13680 — Linux Desktop 内置 ripgrep 在每次启动探测时发生 SIGSEGV** ([link](https://github.com/QwenLM/qwen-code/issues/13680)) — 4 条评论，已关闭。P1 平台 bug：内置的 x64-linux ripgrep 在链接后被重写，导致启动崩溃。虽已关闭，但代表了打包脆弱性。

## 关键 PR 进展
- **#13996 — feat(sdk-java): 添加私有 CSI Session 与原生 SQL authority** ([link](https://github.com/QwenLM/qwen-code/pull/13996)) — 添加显式 Session 创建、持久化请求固定、原生 journal/resource proof、不可变 SQL 执行授权，以及经过认证的 worker bind/history/execute。一个重要的 SDK Java authority 模块。
- **#13955 — feat(managed-agent): 成员间消息走 session_command 通道（H4e-b3c）** ([link](https://github.com/QwenLM/qwen-code/pull/13955)) — 使开放团队成员能够端到端互相发消息，而无需启动 lead model turn；`send_message` 现在支持 `to`。
- **#13902 — feat: 将 agent identity 和 Agent Team 交付投射到 ACP/daemon wire 上（refs #13785）** ([link](https://github.com/QwenLM/qwen-code/pull/13902)) — 通过 `_meta.agent` 元数据和不同的 subagent/team 协作列，将 agent identity 和团队协调投射到 ACP/daemon frames 上。
- **#13931 — feat(managed-agent): 添加 L4 Shell 与 MCP 会话生命周期** ([link](https://github.com/QwenLM/qwen-code/pull/13931)) — 为 `hosted-workspace-shell/1` 和 `hosted-workspace-mcp/1` 添加持久化 close/delete，在分离前结算 effects 以及 SessionEnd/SessionDelete hooks。
- **#13973 — feat(web-shell): 为 Managed Agents 复用标准 UI** ([link](https://github.com/QwenLM/qwen-code/pull/13973)) — Managed Agent 创建和对话复用标准 Web Shell 欢迎页、composer、消息布局以及原生 Workspace/model 控件。
- **#14003 — feat(cli): 添加可重复的 --disable-mcp-server，以在每次调用时排除服务器** ([link](https://github.com/QwenLM/qwen-code/pull/14003)) — 添加可重复的 CLI flag，用于在单次调用中排除指定名称的 MCP 服务器，而不会修改 `settings.json`。
- **#14016 — fix(cli): 在 Virtualized History 中将裸 ↑/↓ 还给输入历史** ([link](https://github.com/QwenLM/qwen-code/pull/14016)) — 恢复裸方向键的输入历史循环；transcript 滚动改为 `Shift+↑/↓`、`PgUp/PgDn` 和 `Ctrl+Home/End`。
- **#13731 — fix(channels): 拆分消息时保留 Markdown 结构** ([link](https://github.com/QwenLM/qwen-code/pull/13731)) — DingTalk 和 WeCom 的 Markdown 拆分现在会在 Markdown 边界附近以约 3,800 单位为目标断行，保留链接、表格和代码块。
- **#13862 — fix(sdk-python): 在有效 PATH 上解析 Windows 命令 shim** ([link](https://github.com/QwenLM/qwen-code/pull/13862)) — 修复 Windows 上 npm 通过 `qwen.cmd` 暴露 CLI 时的 Python SDK 启动问题；解析会基于有效的子 PATH 返回绝对路径。
- **#13642 — feat(runtime-broker): 为终端 JDBC 历史添加有界保留** ([link](https://github.com/QwenLM/qwen-code/pull/13642)) — 为已退役绑定下的旧终端 Runtime Broker 历史添加可选的有界清理，在专用 scheduler 上于请求处理之外运行。

## 功能请求趋势
- **Managed-agent 与 multi-agent 持久性**： #12380, #13785, #13902, #13955, #13931, #13898, #13937, #13838, #13984. 最强的方向是持久化会话所有权、agent identity、团队协调与可观测性。
- **跨平台运行时与打包**： #13395, #13680, #13862, #13967, #13642. 请求集中在 Kubernetes 运行时、Windows shim、Linux 打包和 IDE 本地化。
- **UI/UX、i18n 与无障碍**： #13898, #13837, #6149, #13870, #13967, #13998, #14016. 用户希望 Web Shell/Desktop/TUI 打磨更好、支持 bidi、区域设置感知 UI 和可预测的键盘行为。
- **会话生命周期、取消与恢复**： #12380, #13502, #13838, #13984, #13992, #13937. 多个 issue 针对可抵御重启的恢复、取消溯源和 held-runtime 清理。
- **MCP 与工具/搜索集成**： #13987, #14003, #14009, #13931. 请求包括免费 web-search MCP providers、按调用排除 MCP 以及持久化 MCP 会话生命周期。
- **内存、上下文与基准质量**： #13612, #14011, #13926. 对可控内存提取频率和测试 Qwen Code 原生功能的基准兴趣渐增。
- **CI/测试可靠性**： #13951, #12714, #14012, #13935. 持续有请求要求稳定集成测试和 managed-agent fixtures。

## 开发者痛点
- **Managed-agent 恢复边缘情况**： #13838, #13937, #13984, #13992, #13816, #13986. 开发者反复遇到重启、重新附着、proof-gate 和 mount-release 等边角情况，这些情况发生在控制平面或 harness 中断之后。
- **CI 不稳定与基础设施抖动**： #13951, #12714, #14012, #13935. 诸如 `fatal: pack has 1 unresolved delta` 和 main 分支测试失败等故障拖慢 PR 速度。
- **平台特定启动/配置失败**： #13680, #13943, #13130, #13862. Linux ripgrep SIGSEGV、`.lsp.json` BOM 解析、Windows 命令 shim 和 trusted-workspace 锁定仍摩擦很大。
- **CLI/settings 重算 bug**： #14009, #14003. 像 `--disable-mcp-server` 这样的临时 CLI 覆盖项在 settings 重算时可能丢失，导致已禁用的服务器重新连接。
- **UI/渲染回归与易用性**： #13837, #6149, #13998, #14016, #13870. 混合 RTL/LTR 文本、TUI 滚动/链接交互、turn 折叠隐藏最终答案以及键盘历史行为是反复出现的抱怨。
- **内存提取控制**： #13612. #13571 之后的后续 issue 表明需要更细的频率、边界 flush、窗口边界和遥测。
- **发布流程质量**： #13925. 发布说明中重复 PR 的问题已被报告并关闭，但该 issue 凸显了持续存在的发布说明规范性问题。

---

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*