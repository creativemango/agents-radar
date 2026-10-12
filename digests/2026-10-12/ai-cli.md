# AI CLI 工具社区动态日报 2026-10-12

> 生成时间: 2026-10-12 08:27 UTC | 覆盖工具: 7 个

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

# 跨工具对比报告 — AI CLI 生态（2026-10-12）

## 1. 生态概览

AI CLI 生态已进入整合阶段，竞争战场已从原始模型能力转向 **Agent 可靠性、编排契约与权益正确性**。在所有七款工具中，社区报告的失败模式惊人地相似——子 Agent 静默失败、恢复会话时的会话状态损坏、MCP 认证故障，以及计费/配额不匹配——这表明该品类的痛点更多是结构性问题，而非工具特有问题。发版速度差异显著：Qwen Code 正在发布多个产物（CLI、SDK、桌面端），Gemini CLI 处于 nightly 节奏并有一波安全加固，而 Claude Code、Codex、Copilot CLI、OpenCode 和 Pi 在过去 24h 内没有发布任何内容。最活跃的讨论串不再围绕花哨功能，而是围绕 **信任**：声称成功却什么都没做的 Agent，以及与实际付费不符的套餐。

## 2. 活动对比

| 工具 | Issue（已更新） | PR（已更新） | 讨论 | 发布状态 |
|---|---|---|---|---|
| **Claude Code** | 50 | 0 | N/A（未提供） | 无 |
| **OpenAI Codex** | 未量化（列出约 10 个值得关注项） | 10（已关闭，bot 提交批次） | 活跃（Ideas / Q&A / Show & tell） | 无 |
| **Gemini CLI** | 未量化（列出约 10 个值得关注项） | 10+（安全/加固相关） | N/A（未提供） | v0.65.0 nightly |
| **GitHub Copilot CLI** | 22 | 0 | N/A（未提供） | 无 |
| **OpenCode** | 50 | 50 | N/A（未提供） | 无 |
| **Pi** | 50 | 10 个已关闭 + 1 个开放 | 有（Show & tell） | 无 |
| **Qwen Code** | 未量化（列出约 10 个值得关注项） | 10+（托管 Agent 相关） | N/A（未提供） | v0.25.1 stable + nightly + SDK v0.1.19 + desktop v0.25.1 |

*注：Issue/PR“updated”计数反映摘要报告的总数；部分摘要只列出排名最高的条目而非总数。“N/A（未提供）”表示该时期未采集到讨论数据，并不代表频道被禁用。*

## 3. 共性功能方向

**1. 子 Agent / 多 Agent 编排可靠性（近乎普遍）。**
- *Claude Code*：并行子 Agent 共享 cwd 且无警告（#101461）；后台子 Agent 因 API 错误而终止时返回截断输出，且没有错误信号（#101460）；静默写入/MCP 失败（#13890）。
- *Gemini CLI*：子 Agent 在触及 `MAX_TURNS` 后仍报告 `GOAL` 成功（#22323）；通用型 Agent 永久挂起（#21409）；浏览器 Agent 忽略设置（#22267）。
- *Qwen Code*：父级关闭会等待子轮次而非停止子轮次（#13875）；writer fencing/接管（#12952）。
- *Copilot CLI*：子 Agent 忽略模型继承设置（#4287）。
- *Pi*：针对扩展驱动运行的 `before_agent_start` 生命周期保证（#5581、#10267、#10639）。

**2. 静默/误导性失败成为首要信任问题。**
每个社区都独立指出 Agent 声称成功却什么都没做——Claude Code（#13890、#101460）、Gemini（#22323、#29713）、Copilot（#5115、#5101）、OpenCode（#42950、#54733）、Pi（#10606、#10812）、Codex（#38162、#52898）。

**3. 套餐、计费和配额状态不匹配。**
- *Claude Code*：Team Max-20x 档位请求（#47509，170 👍），Max 20x 被识别为 Pro（#98134）。
- *Codex*：各系列均出现“模型已达容量”（#43375），重置未计入（#37396），目录遗漏（#52839）。
- *Pi*：ChatGPT Pro 重置被忽略（#10480），OAuth 403（#10605）。
- *OpenCode*：DeepSeek V4 Flash 缺少隐私设置（#50155），免费层权限不一致（#50627、#54744）。

**4. MCP 集成税 — 认证、规范漂移与工具暴露。**
- *Copilot CLI*：RFC 8414 issuer 不匹配导致 Dataverse 被阻断（#5082），Atlassian Rovo 认证探测返回 400（#5118），单个标志清空所有 GitHub MCP 工具（#5101）。
- *Codex*：MCP 工具被发现，但始终无法进入 Chat（#38162）。
- *Claude Code*：2026-07-28 无状态规范破坏 header 绑定上下文（#81965），elicitation URL 模式缺口。
- *Gemini CLI*：MCP SDK 从 1.23.0 → 1.32.0 升级被打包进一个含 74 个依赖的 PR（#29664）；工具数 >128 会触发硬 400 错误（#24246）。
- *OpenCode*：MCP 发现耗尽聊天请求槽位（#53050），孤立的发现连接（#53046）。

**5. 恢复会话时的会话状态脆弱性。**
Codex（孤立的进行中轮次 #41591）、Copilot CLI（过期权限重放 #4469、限流后恢复失败 #5114）、OpenCode（V1→V2 迁移中止 #54736）、Pi（终端调整大小后冻结 #10821）。

**6. 终端/TUI 渲染保真度与可配置性。**
Claude Code（调整大小时 scrollback 损坏 #51828，鼠标点击退出选项 #75599）、Codex（Windows 粘贴快捷键、可复制的轮次提示）、Gemini（流式闪烁 #29629）、OpenCode（heredoc 着色、插件输入可读性）、Pi（同步全记录重新布局 #10821）。

## 4. 差异化分析

| 工具 | 功能重点 | 目标用户 | 技术路线 |
|---|---|---|---|
| **Claude Code** | 套餐分级、TUI 控制、子 Agent 隔离 | 使用 Agent CLI 工作流的高级用户；团队 | 专有、以 MCP 为中心；以 issue tracker 为主要渠道 |
| **OpenAI Codex** | Remote Control（移动端/桌面端）、git UI、Windows 对齐 | IDE 优先 + 贴近移动端的开发者 | 多端（TUI、桌面端、VS Code、iOS）；bot 驱动 PR 合入（`copyberry[bot]`） |
| **Gemini CLI** | Agent/编排、安全加固、节省 token 的探索 | 开源贡献者、CI/多 Agent 用户 | 原生 bash 亲和性、AST 感知探索 EPIC、nightly 节奏 |
| **GitHub Copilot CLI** | MCP 认证、ACP/无头对齐、插件生命周期 | GitHub 生态及自动化/CI 用户 | 深度 GitHub 集成；用于程序化控制的 ACP 协议 |
| **OpenCode** | 生成式 UI、提供商灵活性、免费层权限 | 多提供商用户、桌面/TUI 爱好者 | 提供商无关；以生成式 UI（`genui_render`）作为差异化点 |
| **Pi** | OpenAI/ChatGPT 认证可靠性、压缩控制、扩展 | 扩展开发者、本地模型（llama.cpp）用户 | 扩展生命周期 API；面向第三方原生应用的 RPC 模式 |
| **Qwen Code** | Managed Agent 架构（持久会话）、Web Shell、团队消息 | 企业级/多 Agent 平台构建者 | 全栈（CLI + SDK + Java runtime + Desktop）；迭代最快 |

**关键差异化点：** Claude Code 以 Agent 深度为竞争点；Codex 以多端触达为竞争点；Gemini CLI 以开源可扩展性和安全性为竞争点；Copilot CLI 以 GitHub 原生工作流为竞争点；OpenCode 以提供商中立性和生成式 UI 为竞争点；Pi 以扩展/可改造性为竞争点；而 Qwen Code 是唯一明确追求 **企业级多 Agent 平台** 叙事的工具。

## 5. 社区动能与成熟度

**快速迭代（高发布 + 高合并吞吐）：**
- **Qwen Code** — 同时多个发布（CLI、SDK、桌面端）、一个 56 条评论的架构总括 issue（#12380），以及活跃的 managed-agent PR 流。结构性速度最高；成熟度风险在于 CI 可靠性（Flyway 排序 #13922、CVE 审计失败 #13078）。
- **OpenCode** — 50 个 Issue + 50 个 PR，功能面广（生成式 UI、Linux 窗口控制、会话标签页）。吞吐量高，但显示出迁移/升级脆弱性（#54736）。
- **Gemini CLI** — Nightly 节奏；一波明显的安全加固（路径遍历 #29522、检查点隔离 #29521、A2A 信任 #29525、环境隔离 #29523）。维护者响应能力强。

**高互动、低交付节奏（Issue 主导）：**
- **Claude Code** — 50 个 Issue，零 PR/发布；互动很深（套餐分级获得 170 👍），但最关键问题的交付停滞。
- **Pi** — 50 个 Issue，多为以关闭为导向的 PR；扩展生态强，回归分诊清晰（1.1.0 bug 集群）。
- **Codex** — Bot 驱动的 PR 批次（全部 10 个由 `copyberry[bot]` 编写）表明是自动化合入而非社区自然贡献；Windows 仍是明显薄弱平台。
- **Copilot CLI** — 稳定 22 个 Issue，无 PR/发布；MCP 认证与 ACP 对齐是正在浮现的瓶颈。

**成熟度信号：** 最成熟的社区（Claude Code、Codex、Copilot）在计费与平台缺口上表现出*集中且持续*的讨论——这是存量用户触达生产极限的典型迹象。最年轻/上升期的社区（Qwen、OpenCode）则表现出*分散的*功能广度与架构变动。

## 6. 趋势信号

**1. 静默失败是 2026 年标志性的 UX 问题。**
在所有七款工具中，最令人焦虑的 bug 都是 Agent 声称成功却什么都没做。行业正趋同于需要 **可验证的执行契约**——显式错误传播、轮次末验证摘要（Claude Code #101247），以及可信状态信号。对开发者而言，这意味着设计自动化时要假定工具输出可能撒谎，并需要外部验证。

**2. 并发契约是下一个前沿。**
共享工作目录（#101461）、writer fencing（#12952）和过期通知（#101462）表明，并行 Agent 执行缺乏形式化的隔离模型。预计每个子 Agent 的沙箱化与显式生命周期归属将成为基本要求。

**3. 计费/权益是竞争负债。**
Claude Code（#47509）、Codex（#43375）、Pi（#10480）和 OpenCode（#50155）都显示权益层无法匹配用户实际购买。用户侧无法自行解决，并引发过度挫败——这是厂商系统性投入不足的可靠性维度。

**4. MCP 规范漂移带来反复出现的集成税。**
每个有 MCP 界面的工具都报告了版本相关的破坏。随着 MCP 成为事实上的标准化层，**能力协商与向后兼容**是集成方开发者的成本中心——一个信号：需要为持续的 MCP 维护预留预算。

**5. Windows 是全行业薄弱平台。**
Codex（#51981 ACL 失败、#25271 浏览器损坏）、Copilot CLI（#5119 插件卸载）和 Pi（#10801 认证锁定）都集中在 Windows 特有的打包、沙箱与权限问题上。跨平台对齐仍然服务不足。

**6. 会话持久性是硬性要求，而非锦上添花。**
恢复时损坏出现在 Codex、Copilot、OpenCode 和 Pi 中。随着会话变长、Agent 无人值守运行，**持久、可协调的会话状态**（Qwen 的 Managed Agent 工作是最明确的努力）正成为架构差异化点。

**对开发者的参考价值：** 这些趋同的痛点，为今天基于或集成这些工具的人定义了一份不可妥协事项清单：（a）绝不在没有外部验证的情况下相信 Agent 报告的成功；（b）在文件系统和会话层面隔离并发 Agent；（c）将权益/配额状态视为一等集成风险；（d）为 MCP 版本频繁变动预留预算；（e）当变化中的稳定性比冻结的功能集更重要时，优先选择发布节奏活跃的工具（Qwen、Gemini、OpenCode）。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

**注意：** 提供的快照中 PR 评论数显示为 `undefined`，因此下方排名依据仓库提供的“popular PR”排序以及近期更新/影响信号。

## 1. 顶级 Skills 排名

| 排名 | Skill / PR | 作用 | 讨论 / 关注亮点 | 状态 |
|---|---|---|---|---|
| 1 | **mcp-builder: MCP v2 compatibility fix** — [#1742](https://github.com/anthropics/skills/pull/1742) | 修复 `mcp>=2` 支持：将 `streamablehttp_client` 更新为 `streamable_http_client`，并把自定义 headers 迁移到 `create_mcp_http_client` / `http_client`。 | 解决 issue #1668；影响很大，因为 MCP builder 是工具/服务器集成的核心基础设施。更新于 2026-10-08。 | OPEN |
| 2 | **skill-creator: trigger eval isolation + Windows/runtime handling** — [#1298](https://github.com/anthropics/skills/pull/1298) | 修复触发评估中的漏检/无效评分：worker 探测竞争、Windows 子进程管道失败、无关工具干扰，以及运行时失败被当作未触发的问题。 | 直接针对 skill 评估与优化的可靠性。更新于 2026-09-16。 | OPEN |
| 3 | **proofcore-contract-auditor** — [#1771](https://github.com/anthropics/skills/pull/1771) | 新的 Web3 Agent Skill，用于静态分析 Solidity/Rust 智能合约，并通过 ProofCore 的零存储 Merkle 协议将审计证明锚定到 TON 上。 | 向智能合约公证/审计领域的新扩展；活跃的新 skill 提案。更新于 2026-09-16。 | OPEN |
| 4 | **docx: detect orphaned comments** — [#1734](https://github.com/anthropics/skills/pull/1734) | 针对 `.docx` 文档中的孤立批注。 | 摘要为空，因此关注度是根据标题和文档类 skill 相关性推断的。更新于 2026-09-25。 | OPEN |
| 5 | **md2video-audio** — [#1703](https://github.com/anthropics/skills/pull/1703) | 新的零成本 skill，可将 Markdown 编译为带逼真配音的 MP4 视频，使用 Marp 及相关工具链。 | 将 Skills 扩展到媒体生成/文档转视频工作流。更新于 2026-09-15。 | OPEN |
| 6 | **notion-spec-to-implementation + quantitative-resume-auditor** — [#1245](https://github.com/anthropics/skills/pull/1245) | 新增两个 skill：一个将产品/技术规格转化为 Notion 实施任务；另一个对简历进行量化审计。 | 将工作流自动化与文档/招聘用例相结合。更新于 2026-09-30。 | OPEN |
| 7 | **skill-creator: harden eval viewer** — [#1961](https://github.com/anthropics/skills/pull/1961) | 对 `generate_review.py` 和 `viewer.html` 进行安全加固：脚本逃逸、DNS rebinding、跨站 POST 以及转义缺口。 | 面向核心元 skill 的安全类 PR；最近于 2026-10-11 更新。 | OPEN |
| 8 | **docx: LibreOffice timeout as error + output verification** — [#1792](https://github.com/anthropics/skills/pull/1792) | 让 `accept_changes.py` 在 LibreOffice 超时时返回错误，并验证输出 DOCX 不再包含修订标记。 | 面向文档工作流的可靠性修复；更新于 2026-09-25。 | OPEN |

## 2. 社区需求趋势

从 Issue 列表来看，最强烈的需求并不只是“更多 Skills”，而是围绕 Skills 更好的**信任、可靠性和上下文效率**。

- **安全与信任边界** — `anthropic/` 命名空间下的社区 skill 可以冒充官方 skill：[#492](https://github.com/anthropics/skills/issues/492)。相关担忧包括 eval-viewer XSS（[#1394](https://github.com/anthropics/skills/issues/1394)）、SharePoint 权限安全（[#1175](https://github.com/anthropics/skills/issues/1175)）以及 agent 治理提案（[#412](https://github.com/anthropics/skills/issues/412)）。
- **Skill 评估与触发可靠性** — 用户报告 `run_eval.py` 从不触发 skills/commands（[#556](https://github.com/anthropics/skills/issues/556)）、parallel-worker UUID 交叉匹配（[#1352](https://github.com/anthropics/skills/issues/1352)），以及静默 benchmark 失败（[#1383](https://github.com/anthropics/skills/issues/1383)）。
- **上下文窗口与 token 效率** — `claude-api` skill 急切注入约 156k tokens（[#1487](https://github.com/anthropics/skills/issues/1487)），以及重叠插件带来的重复 skills（[#189](https://github.com/anthropics/skills/issues/189)），凸显了对更精简、更懒加载 skill 加载方式的需求。
- **记忆与紧凑 agent 状态** — 关于 `compact-memory` 的提案：一种用于紧凑持久化 agent 状态的符号化表示法：[#1329](https://github.com/anthropics/skills/issues/1329)。
- **团队/组织分发** — 对全组织 skill 共享的需求，而非手动下载/上传 `.skill` 文件：[#228](https://github.com/anthropics/skills/issues/228)。
- **质量门禁与验证工作流** — 关于任务前校准、对抗性审查和交付验证的提案：[#1385](https://github.com/anthropics/skills/issues/1385)。
- **Skill 编写最佳实践** — 请求让 `skill-creator` 更可操作、更 token 高效，而不是面向人类的文档：[#202](https://github.com/anthropics/skills/issues/202)。

## 3. 高潜力待处理 Skills

这些是近期有活动、影响核心 skill 或直接关联 issue 的开放 PR——很可能很快落地或塑造生态。

- **mcp-builder MCP v2 修复** — [#1742](https://github.com/anthropics/skills/pull/1742)：针对 `mcp>=2` 的核心兼容性修复；更新于 2026-10-08。
- **skill-creator eval-viewer 加固** — [#1961](https://github.com/anthropics/skills/pull/1961)：本地 eval viewer 的安全修复；更新于 2026-10-11。
- **webapp-testing：移除 `shell=True`** — [#1980](https://github.com/anthropics/skills/pull/1980)：缓解命令注入风险（CWE-78）；创建于 2026-10-06。
- **docx：LibreOffice 超时/错误验证** — [#1792](https://github.com/anthropics/skills/pull/1792)：让文档转换失败可见并得到验证；更新于 2026-09-25。
- **algorithmic-art：修复 `wrapAround()`** — [#1977](https://github.com/anthropics/skills/pull/1977)：修正环绕行为；修复 #1897；更新于 2026-10-07。
- **webapp-testing：修正 textarea/select 报告** — [#1976](https://github.com/anthropics/skills/pull/1976)：改进元素发现；修复 #1891；更新于 2026-10-07。
- **skill-creator：直接执行 `package_skill.py`** — [#1681](https://github.com/anthropics/skills/pull/1681)：修复 `ModuleNotFoundError` 和过时路径；更新于 2026-10-08。
- **proofcore-contract-auditor** — [#1771](https://github.com/anthropics/skills/pull/1771)：新的 Web3 智能合约审计 skill；更新于 2026-09-16。

## 4. Skills 生态洞察

社区最集中的需求是**可信、安全、可靠且 token 高效的 Skill 基础设施**——尤其是命名空间完整性、触发/评估正确性以及上下文窗口纪律——而不仅仅是更多领域专用的 Skills。

---

# Claude Code 社区文摘 — 2026-10-12

## 1. 今日亮点

过去 24 小时内没有新的发布或拉取请求，所有活动都集中在问题追踪器上，共有 50 个问题被更新。主要讨论线程是**套餐经济学**：长期存在的 Team 套餐分级请求（#47509，60 条评论 / 170 👍）以及围绕 Max 20x 订阅的两项计费/认证回归问题持续吸引着稳定的参与。另一组新提交的问题（均为今日提交）指出了**并行和后台子代理的正确性问题**——共享工作目录、缺少错误信号以及过时的监控通知——这表明代理编排的可靠性正成为新兴的前沿关注点。

## 2. 发布

过去 24 小时内无。

## 3. 关键 PR 进展

过去 24 小时内没有拉取请求被更新。

## 4. 热门问题

1. **[#47509 — Team plan needs a Max 20x equivalent tier](https://github.com/anthropics/claude-code/issues/47509)** (OPEN, 60 comments, 170 👍)
   该批次中参与度最高的问题。Team 席位最高只能达到 Premium（6.25 倍 Pro），而使用代理式 CLI 工作流的高级用户表示这不够用；他们希望在 Team 计费中提供 Max-20x 级别的层级。👍 数量相对评论数之高，表明这是广泛的潜在需求，而非小众抱怨。

2. **[#51828 — Scrollback duplication on terminal resize in 2.1.116](https://github.com/anthropics/claude-code/issues/51828)** (OPEN, has repro, 29 comments, 36 👍)
   在 macOS 上调整大小后，VS Code 集成终端中出现可复现的 TUI 损坏。渲染错误获得高 👍 意味着它扰乱了日常工作流；“在版本 X 中持续存在”的表述暗示它已经历了多次修复尝试但依然存在。

3. **[#98134 — Active Apple Max 20x subscription recognized as Pro](https://github.com/anthropics/claude-code/issues/98134)** (OPEN, area:auth, 11 comments)
   macOS 上的权益错误分类：支付 Max 20x 费用的订阅者被限制在 Pro 限额内。与 #47509 和 #61828 一起构成了一个连贯的主题：“计费层与用户实际购买的内容不匹配”。

4. **[#64479 — Edit tool fails on mixed literal/escape Unicode](https://github.com/anthropics/claude-code/issues/64479)** (OPEN, has repro, platform:linux, 18 comments)
   一个多行 `old_string` 中同一个字符既以字面码点出现又以 `\uXXXX` 转义出现，导致回退交换逻辑失效。表情符号数量少但评论持续不断，表明核心工具中存在顽固的正确性错误。

5. **[#75599 — Granular control over mouse click behavior in interactive menus](https://github.com/anthropics/claude-code/issues/75599)** (OPEN, enhancement, 11 comments, 23 👍)
   自 v2.1.181 起，在权限提示、`/model`、`/config` 和 `AskUserQuestion` 对话框中，单击会立即确认选择。用户希望有退出选项——这是一个小的可配置性缺口，但对人体工学的影响不成比例。

6. **[#45775 — Gmail MCP regression: `create_draft` missing `threadId`](https://github.com/anthropics/claude-code/issues/45775)** (CLOSED, invalid, area:mcp, 31 comments, 25 👍)
   当日评论数最高的已关闭问题。在 31 条评论后被关闭为*无效*，这很可能意味着底层信号存在于上游连接器而非 CLI 中——值得关注其是否复发。

7. **[#13890 — Subagents silently fail to write files or call MCP tools](https://github.com/anthropics/claude-code/issues/13890)** (CLOSED, has repro, 14 comments, 12 👍)
   Task 工具子代理认为操作已成功，但没有任何内容写入磁盘。静默失败模式使其成为一个信任问题，而不仅仅是一个错误。

8. **[#101461 — Parallel subagents share a working directory with no warning](https://github.com/anthropics/claude-code/issues/101461)** (OPEN, filed 2026-10-12)
   两个并发的 Agent 工具调用继承相同的 cwd；并发的构建/测试写入共享输出可能会静默损坏产物。第一天提交的问题，对并行代理执行具有明确的架构影响。

9. **[#101460 — Background subagent dying on API error returns truncated result with no error signal](https://github.com/anthropics/claude-code/issues/101460)** (OPEN, filed 2026-10-12)
   重试耗尽后，后台子代理返回部分输出，没有失败标记。与 #13890 相同的信任侵蚀模式，现在出现在后台执行路径中。

10. **[#100723 — GitHub integration: cloud routines cannot `add_repo` to push](https://github.com/anthropics/claude-code/issues/100723)** (OPEN, platform:web, area:routines)
    无人值守的定时例程期望将 HTML 文件推送到私有仓库，但报告没有 `add_repo` 能力。这阻碍了例程所宣传的“设置后即忘”自动化故事。

*其他值得注意的：* [#94373](https://github.com/anthropics/claude-code/issues/94373)（桌面通知应区分权限等待和任务完成）、[#87978](https://github.com/anthropics/claude-code/issues/87978)（VoiceOver/屏幕阅读器回合完成信号）、[#101356](https://github.com/anthropics/claude-code/issues/101356)（Opus 5.5 将用户自有源代码的加固误分类为恶意）、以及 [#93246](https://github.com/anthropics/claude-code/issues/93246)（桌面浏览器面板在 HTTP Basic Auth 上循环）。

## 5. 热门讨论

此期间未提供讨论数据。

## 6. 功能请求趋势

- **套餐分级与成本透明度。** #47509（Team Max-20x 层级）、#98134（Max 20x 被检测为 Pro）以及已关闭的 #61828（过早出现“达到使用限制”）共同构成了最响亮的单一请求：使权益和计量与用户所支付的内容匹配，并为高级用户提供有余量的层级。
- **TUI 可配置性与渲染保真度。** #75599（鼠标点击退出选项）、#76988（在 SIGWINCH 上重新运行 `statusLine`）、#89824（助手消息上的表情符号反应）以及 #51828（调整大小导致回滚缓冲区损坏）表明用户需要既更可控又更稳定的终端 UI。
- **可组合的终端功能。** #101247 要求 harness——而非模型——在回合结束时发出验证摘要，这是一个值得注意的转变，即更信任工具而非模型自我报告的主张。
- **桌面通知语义。** #94373 和 #67992 都希望在 OS/桌面层提供更丰富的会话状态：区分权限被阻止和已完成，并通过徽章可靠地显示定时/例程会话。
- **子代理隔离与可观测性。** #101461 和 #101460 请求每个子代理的工作目录和显式错误传播——这是 Agent 工具真正并发契约的开端。
- **MCP 规范时效性。** #48164、#88075 和 #69087（均已关闭）围绕 URL 模式 elicitation 支持聚集，而 #81965 指出在 2026-07-28 无状态规范下头部绑定上下文丢失——MCP 版本漂移是反复出现的集成税。
- **可访问性。** #87978 是最明确的信号，表明屏幕阅读器用户需要明确的回合完成功能，而不是一堵流式 token 的墙。

## 7. 开发者痛点

- **计费与权益偏离现实。** 使用计数器、套餐层级和订阅检测都在产生不匹配（#47509、#98134、#61828）。这引发了最多的参与和最多的挫败感，因为用户端无法解决。
- **静默失败是最糟糕的失败。** 在 #13890、#101460 和 #77325 中，代理报告成功但什么都没做，或者返回截断输出而没有错误标记。开发者无法在结果撒谎的工具上构建可靠的自动化。
- **多代理运行中的并发危险。** 共享工作目录（#101461）加上过时、被取代的通知（#101462）意味着并行代理工作负载可能在无人察觉的情况下损坏状态——这是一个将随采用率增长而扩大的信任问题。
- **MCP 生态系统的动荡。** 新的规范修订（2026-07-28 无状态、elicitation URL 模式）反复破坏现有服务器并留下能力协商缺口（#81965、#88075、#48164、#69087、#47733）。集成作者承担维护成本。
- **跨宿主环境的终端/TUI 脆弱性。** 调整大小处理、VS Code 集成、全屏对话框裁剪以及状态行过时（#51828、#69087、#76988）对 Claude Code 使用最频繁的环境中的用户影响尤为严重。
- **被阻止的无人值守自动化。** 例程和 GitHub 集成问题（#100723、#67992）恰恰在无人值守可靠性最重要的地方削弱了云/定时任务的价值主张。

---

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区摘要 — 2026-10-12

## 1. 今日亮点
过去 24 小时内没有发布新版本；活动主要由长期存在的 bug 讨论串和自动化贡献者（`copyberry[bot]`）提交的大批已关闭 TUI/app-server PR 主导。互动最高的问题仍然是 **iOS/macOS 远程控制回归**（#36040，79 条评论）以及 **Windows 平台缺口**，涉及沙箱、浏览器自动化和本地项目持久化。增强方面，社区呼声日益高涨，要求恢复桌面侧边栏中**可见的 Commit/Push 控件**（#47511，46 👍）。

## 2. 发布
过去 24 小时内未发布任何版本。

## 3. 热门 Issue

1. **#36040 — iOS Remote 仅列出最近有聊天的项目**（OPEN，79 条评论，👍5）—— 流量最高的讨论串。与 macOS 桌面主机配对的远程控制会静默隐藏没有最近聊天活动的项目，使移动端在实际切换项目时不可靠。[链接](https://github.com/openai/codex/issues/36040)
2. **#25271 — Computer Use 在 Windows 上无法确定 Chrome URL**（OPEN，56 条评论，👍12）—— 即使 `chrome://newtab/` 也无法完成 URL 解析，阻碍 Windows 上的浏览器驱动工作流。该问题长期存在（自 5 月起）且没有确认修复。[链接](https://github.com/openai/codex/issues/25271)
3. **#43375 — 多个 GPT-5/GPT-6 模型返回“Selected model is at capacity”**（OPEN，30 条评论，👍19）—— 容量错误横跨多个模型，因此切换模型无法恢复会话。直接影响付费层级的可靠性。[链接](https://github.com/openai/codex/issues/43375)
4. **#47511 — Commit 和 Push 按钮缺失**（OPEN，19 条评论，👍46）—— 桌面版 26.917.51856 中的回归，将核心 git 操作埋入未标记的溢出菜单。当前 Issue 集中反应数最高；与增强请求 #47897 重复。[链接](https://github.com/openai/codex/issues/47511)
5. **#41591 — 孤立的 inProgress 轮次会隐藏后续已完成的轮次（macOS）**（OPEN，15 条评论，👍3）—— 长对话中卡住的轮次会隐藏后续输出，且在重启后仍然存在，意味着会话存储中持久化了错误状态。[链接](https://github.com/openai/codex/issues/41591)
6. **#38162 — 本地 MCP 工具已被发现但未暴露给 Chat 会话**（OPEN，12 条评论，👍2）—— MCP 服务器在 Windows 上成功启动，但工具从未到达 Chat，这是静默的能力缺口，而非硬错误。[链接](https://github.com/openai/codex/issues/38162)
7. **#51981 — Windows 沙箱 ACL 校验在 `node_repl.exe` 上失败，出现 os error 32**（OPEN，11 条评论）—— 阻止 Chrome 控制*和* shell 命令，表明沙箱令牌/ACL 层是多个 Windows 浏览器自动化 bug 背后的共同根因。[链接](https://github.com/openai/codex/issues/51981)
8. **#52839 — 在 ChatGPT 认证下，Sol Ultrafast 从第一方模型目录中缺失**（OPEN，10 条评论）—— 启用了 `fast_mode`/`ultrafast_mode` 且没有目录覆盖的用户仍然无法选择该模型；这是权益/目录管道不匹配。[链接](https://github.com/openai/codex/issues/52839)
9. **#49735 — Token 预算提醒会促使长时间运行的 agent 过早结束**（OPEN，5 条评论）—— 一份实质性的模型行为报告，要求进行运行时和训练消融；值得注意，因为它针对的是智能体可靠性而非 UI。[链接](https://github.com/openai/codex/issues/49735)
10. **#50084 — VS Code Codex 扩展在轮次完成后卡在流式传输状态**（OPEN，3 条评论，👍6）—— 只有重启 Extension Host 才能清除；重新加载 webview 不够。对 IDE 优先的用户影响很大。[链接](https://github.com/openai/codex/issues/50084)

**同样值得关注：** #52917 / #53305（Windows 桌面版丢失本地项目目录；#53305 此后已关闭）、#52836（state-DB 回填卡在 `running` → “Organization settings could not be loaded”）、#50986（Codex Cloud 运行时没有出站网络）、#32241（macOS Remote Control “stream became unknown” 重置）、#44334（项目会话堆满全局 Recents）。

## 4. 关键 PR 进展
以下 10 个 PR 均于 **2026-10-11 关闭，且作者为 `copyberry[bot]`**，表明它们是一批自动化/agent 驱动的合并，而非独立的社区贡献。

1. **#53224 — 在 TUI 中支持 Windows 剪贴板文本粘贴快捷键** —— `Ctrl+V` 以前仅尝试粘贴图像（在文本剪贴板上会报错）；`Shift+Insert` 现在走粘贴路径。[链接](https://github.com/openai/codex/pull/53224)
2. **#53201 — 在线程归档后保持 agents 导航可用** —— 修复当线程在输入框为空时被外部归档后，左箭头 agents 快捷键失效的问题。[链接](https://github.com/openai/codex/pull/53201)
3. **#53173 — 允许从 TUI 重启受管后台服务器** —— 让用户无需离开 TUI 即可刷新过期的启动配置警告。[链接](https://github.com/openai/codex/pull/53173)
4. **#53156 — 简化自定义终端绘制 API** —— 移除 `Terminal::autoresize` 和可失败的 `try_draw` 辅助函数；渲染统一到 `draw_with_size`。这是一次降低 TUI 渲染复杂度的内部清理。[链接](https://github.com/openai/codex/pull/53156)
5. **#53155 — 在关闭期间保持远程 app-server 事件可投递** —— 防止正在处理中的通知在 WebSocket 关闭帧发送之前杀死 worker。与远程控制稳定性问题直接相关。[链接](https://github.com/openai/codex/pull/53155)
6. **#53153 — 在 TUI 帮助中显示子代理导航快捷键** —— 将 `Alt+Left` / `Alt+Right`（“Subagents (empty prompt)”）添加到快捷键浮层，并更新快照。[链接](https://github.com/openai/codex/pull/53153)
7. **#53152 — 允许选择和复制 TUI 轮次提示** —— 将工作/完成提示行注册为可通过鼠标选择和复制。[链接](https://github.com/openai/codex/pull/53152)
8. **#53150 — 在冷恢复时恢复线程自有权限** —— 修复冷恢复会用当前默认值替换已保存的未命名权限，包括早于设置更新的首轮上下文。与安全相关。[链接](https://github.com/openai/codex/pull/53150)
9. **#53148 — 修复待处理 Vim Replace 按键的启动草稿恢复** —— 修正中断的 Vim Replace 之后未发送文本的恢复（例如 `abcd` → `Xbcd`，而不是 `Xabcd`）。[链接](https://github.com/openai/codex/pull/53148)
10. **#53109 — 在插件列表输出中优雅处理管道破裂** —— `codex plugin list | head` 不再 panic；表格和 JSON 输出使用可失败的 writer。[链接](https://github.com/openai/codex/pull/53109)

## 5. 热门讨论

**想法**
- **#14595 — “Remote control wen?”**（7 条评论，👍18）—— 对一等公民远程控制的持续需求，用户反馈第三方变通方案（farfield）不够好。这是该组中信号最强的路线图请求。[链接](https://github.com/openai/codex/discussions/14595)
- **#53061 — 当 GitHub Actions CI 完成后恢复现有本地 Codex 聊天** —— 请求事件触发器，以便 Codex 在 CI 完成后继续 PR 工作流，而不是手动输入“continue”提示或轮询。[链接](https://github.com/openai/codex/discussions/53061)
- **#53294 — 对 Cloud-Managed/Enterprise 层抑制 `config.toml` 弃用警告** —— 建议当旧键来自受管配置而非用户时，抑制无法关闭的横幅。[链接](https://github.com/openai/codex/discussions/53294)
- **#53216 — 自适应自动推理模式** —— 希望自动选择推理级别，以减少简单任务上的用量浪费。[链接](https://github.com/openai/codex/discussions/53216)

**问答**
- **#53079 — [ANSWERED] 使用 Codex 构建可靠的全栈工作流**（3 条评论）—— 功能分支、结构化 agent 工作流、专业实践；已解答。[链接](https://github.com/openai/codex/discussions/53079)
- **#53199 — 当选择器中缺少权限配置文件时，为单个项目启用网络访问** —— 希望按项目开放网络访问，同时保持文件系统沙箱和其他项目受限。[链接](https://github.com/openai/codex/discussions/53199)

**展示与分享**
- **#42049 — Bundled-marketplace 刷新泄漏暂存目录（约 31 GiB）** —— 323 个被遗弃的 `openai-bundled.staging-*` 目录导致 macOS 上磁盘严重耗尽。[链接](https://github.com/openai/codex/discussions/42049)
- **#51406 — “No Comment” PostToolUse hook** —— 用于移除 Codex 写下的聊天叙述式注释的 hook；解决长会话中指令淡出的问题。[链接](https://github.com/openai/codex/discussions/51406)
- **#53043 — plan-ledger** —— 记录 `/plan` 达成的内容，并标记计划之外变更文件的 Hooks + CLI。[链接](https://github.com/openai/codex/discussions/53043)
- **#53200 — Reset Alerts** —— 针对 Codex 速率限制重置的 SMS（美国/加拿大）/ Telegram 通知，以及重置仪表板。[链接](https://github.com/openai/codex/discussions/53200)

## 6. 功能请求趋势
- **远程控制成熟度** —— 移动端/桌面端远程控制是请求最多的单一方向：项目列表一致性（#36040）、流可靠性（#32241、#35727），以及通用路线图请求（#14595）。
- **按项目的网络与权限范围控制** —— #53199 及相关沙箱问题表明，用户需要细粒度、持久且不会削弱文件系统隔离的网络授权。
- **恢复/保留直接 git 操作** —— #47511 和 #47897 都要求可见的 Commit/Push 控件，反对 UI 简化。
- **会话与项目目录卫生** —— 让项目会话不进入全局 Recents（#44334），阻止崩溃/重启时目录丢失（#52917、#53305），并在冷恢复后恢复状态（#53150）。
- **自适应推理 / 用量效率** —— #53216 以及速率限制讨论串（#37396、#51026）显示出对自动选择推理级别的需求。
- **CI/CD 与事件驱动的 agent 继续执行** —— #53061 要求 Codex 能在外部事件（GitHub Actions）发生时恢复工作，而不是依赖手动提示。
- **IDE/扩展可靠性** —— #50084（VS Code 卡在流式传输）反映出不断增长的 IDE 优先用户群，其故障模式各不相同。

## 7. 开发者痛点
- **Windows 显然是薄弱平台。** 沙箱 ACL 失败（#51981）、浏览器/Computer Use 损坏（#25271、#45340、#52898）、MCP 工具无法到达 Chat（#38162）以及项目目录丢失（#52917、#53305）都集中在相同构建上——表明存在共同的 Windows 沙箱/状态层根因。
- **远程控制因状态问题失去信任。** 项目从移动端消失（#36040），活动任务被 writer 锁阻塞（#51053），转录在序列缺口后截断（#35727），健康流也会重置（#32241）。PR #53155 只解决了关闭投递这一部分。
- **速率限制与权益带来的意外。** 多个模型系列出现“Model is at capacity”（#43375），重置已宣传但未到账（#37396），用量重置不可用（#51026），以及子代理立即因 `usage_limit_exceeded` 失败（#32432）。
- **静默能力缺口比响亮错误更糟。** MCP 工具被发现但未暴露（#38162）、浏览器后端被检测到但不可用（#52898），以及 ChatGPT 认证下模型目录缺失（#52839），都失败时没有可操作的诊断信息。
- **会话状态很脆弱。** 孤立的进行中轮次（#41591）、重启后聊天消失（#38927），以及 VS Code 扩展卡在流式传输（#50084），迫使用户重启并手动恢复。
- **桌面应用的磁盘卫生。** 被遗弃的 marketplace 暂存目录占用约 31 GiB（#42049），对重度用户而言是未解决的操作风险。
- **以简化为名的 UI 回归。** 将 Commit/Push 埋藏起来，在所有开放 Issue 中获得了最高的单一反应数（👍46）。

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区文摘 — 2026-10-12

## 1. 今日亮点

今日活动集中在 **agent/subagent 工作流**：一大批长期存在的 `area/agent` issue 重新进入 `status/need-retesting`（#22323、#21409、#21968、#22267、#21983），表明维护者正在验证近期围绕 subagent 终止语义、挂起和配置传播的修复。在 PR 方面，一批安全加固补丁已合入或关闭——包括外部检查器的环境隔离、glob 路径遏制、检查点路径遍历以及 A2A 工作区信任——同时还有针对 JSON 序列化、Gemini 3 多模态工具响应和流式闪烁的核心修复。

## 2. 发布

- **v0.65.0-nightly.20261012.g9b6e0265d** — 今日发布的 nightly 版本，伴随自动版本号提升 PR [#29714](https://github.com/google-gemini/gemini-cli/pull/29714)。除比较 diff 外无面向用户的变更日志：[compare v0.65.0-nightly.20261010...v0.65.0-nightly.20261012](https://github.com/google-gemini/gemini-cli/compare/v0.65.0-nightly.20261010.g9b6e0265d...v0.65.0-nightly.20261012.g9b6e0265d)

## 3. 热门 Issue

1. **[#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — MAX_TURNS 后 subagent 恢复被报告为 GOAL 成功（P1，13 条评论，👍2）** — `codebase_investigator` subagent 声称 `status: "success"` / `Termination Reason: GOAL`，但实际上在执行任何分析之前就达到了轮次限制。这是今日讨论最多的 issue，因为它无声地破坏了 agent 可靠性信号——也就是“agent 说完成了，但什么都没发生”背后的核心失败模式。
2. **[#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — 零依赖操作系统沙箱与执行后意图路由（P2，9 条评论，👍1）** — 提议利用 Gemini 3 对 bash 的原生亲和性，在沙箱内串联 POSIX 工具，而不是将它们隐藏在定制工具背后。这是一个投入较大的设计讨论，将重塑 CLI 处理文件探索和编辑的方式。
3. **[#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — 通用 agent 永久挂起（P1，8 条评论，👍8）** — 今日社区反响最多：委托给通用 agent 会让即使琐碎的操作（创建文件夹）也无限期挂起；禁用 subagent 委托是唯一的解决方法。现正在重新测试。
4. **[#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — 评估 AST 感知的文件读取、搜索和映射（EPIC，7 条评论）** — 跟踪 AST 感知工具是否能减少错位读取、token 噪声和轮次数量。标志着向结构化代码理解而非原始文本猛灌的战略转变。
5. **[#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — Gemini 没有充分使用技能和子 agent（7 条评论）** — 虽是轶事但引起广泛共鸣：模型忽略自定义技能和 subagent，除非明确指示，这削弱了整个可扩展性叙事。
6. **[#22267](https://github.com/google-gemini/gemini-cli/issues/22267) — 浏览器 Agent 忽略 `settings.json` 覆盖，例如 `maxTurns`（4 条评论）** — `AgentRegistry` 在初始化时正确合并设置，但浏览器 Agent 在运行时绕过它们——这是一个影响任何调优部署的配置契约 bug。
7. **[#22232](https://github.com/google-gemini/gemini-cli/issues/22232) — 浏览器 agent 会话接管 / 锁恢复（P3，4 条评论）** — 请求将锁定持久浏览器配置文件上的“快速失败”行为替换为自动接管孤儿会话。对 CI 和多 agent 使用具有高价值。
8. **[#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — 浏览器 subagent 在 Wayland 上失败（P1，4 条评论，👍1）** — 报告在 Wayland 会话中出现 GOAL 终止但实际未完成；Linux 桌面用户仍然是一个反复出现的盲点。
9. **[#20079](https://github.com/google-gemini/gemini-cli/issues/20079) — `~/.gemini/agents/` 中的符号链接 agent 文件未被识别（4 条评论）** — 阻碍了 dotfile 管理器和工作流；需要更多信息，但这是 agent 发现中一个简单而具体的缺口。
10. **[#24246](https://github.com/google-gemini/gemini-cli/issues/24246) — 超过 128 个工具时出现 400 错误（3 条评论）** — 来自 MCP 服务器和 subagent 的工具数量爆炸触发硬 API 失败；agent 需要更智能的工具作用域。

*另值得关注：* **[#29713](https://github.com/google-gemini/gemini-cli/issues/29713)**（新增，等待分类）— `list_directory` 静默丢弃路径验证失败的条目，报告成功但不完整的列表。

## 4. 关键 PR 进展

1. **[#29626](https://github.com/google-gemini/gemini-cli/pull/29626) — fix(core): 在 JSON 序列化中保留共享引用（P2）** — `safeJsonStringify` 使用了单一的全局 `WeakSet`，将合法共享的（非循环）对象替换为 `[Circular]`；影响 OTel 指标和任何重新序列化的遥测负载。
2. **[#29611](https://github.com/google-gemini/gemini-cli/pull/29611) — fix(core): 针对带点号的 Gemini 3 模型和别名的多模态函数响应（P2）** — 在 `supportsMultimodalFunctionResponse()` 中解析别名和带点号的版本，防止在 `gemini-3.8-flash` 等模型上出现无效的兄弟部件。
3. **[#29621](https://github.com/google-gemini/gemini-cli/pull/29621) — fix(core): 保留 subagent 多模态工具响应部件（P2）** — 当本地 subagent 将结果反馈给模型时，与函数响应一起发出的图像数据被丢弃。
4. **[#29527](https://github.com/google-gemini/gemini-cli/pull/29527) — fix(core): 确保请求内容不以模型轮次结尾（P1，已关闭）** — 修复了 `/rewind`、流中断或剥离尾部用户轮次后的 `400 Bad Request`。关闭了长期存在的 P1 可靠性缺口。
5. **[#29523](https://github.com/google-gemini/gemini-cli/pull/29523) — fix(core): 为外部安全检查器提供最小环境和上限输出（P2，已关闭）** — 阻止将 `GEMINI_API_KEY` 和其他机密泄露到第三方检查器进程，并限制无界的 stdout 累积。
6. **[#29522](https://github.com/google-gemini/gemini-cli/pull/29522) — fix(core): 将 glob 工具匹配保持在已验证的搜索目录内（已关闭）** — 绝对 glob 模式（例如 `/etc/*.conf`）以前会逃逸已验证的 `dir_path`；这是对 glob 工具的真正路径遍历修复。
7. **[#29521](https://github.com/google-gemini/gemini-cli/pull/29521) — fix(core): 遏制遗留检查点路径（P1，已关闭）** — 像 `x/../../secret` 这样的标签解析到检查点目录之外；加固了检查点读取/删除路径。
8. **[#29525](https://github.com/google-gemini/gemini-cli/pull/29525) — fix(a2a-server): 绝不从请求 `agentSettings` 推导工作区信任（已关闭）** — 使 `createTask()` 与其他规范化设置的入口点保持一致，关闭了 A2A 服务器中的信任提升向量。
9. **[#29629](https://github.com/google-gemini/gemini-cli/pull/29629) — fix(cli): 限制待处理纯文本高度以减少流式闪烁（P2）** — 限制流式 markdown 高度，以避免每次更新时全屏清除并重绘；直接解决了 [#21924](https://github.com/google-gemini/gemini-cli/issues/21924) 中的闪烁问题。
10. **[#29709](https://github.com/google-gemini/gemini-cli/pull/29709) — fix(vscode-ide-companion): 跟踪所有 `activate()` 可处置对象（P2）** — `context.subscriptions.push(...)` 中的逗号表达式导致注册泄露且未被处置。修复 #27790。

*补充：* **[#29664](https://github.com/google-gemini/gemini-cli/pull/29664)**（P1，XL）捆绑了 74 个 npm 依赖项升级，包括 `@modelcontextprotocol/sdk` 1.23.0 → 1.32.0——鉴于 MCP 表面积，值得审查关注。**[#29705](https://github.com/google-gemini/gemini-cli/pull/29705)** 修复了 `formatDuration` 单位边界舍入（`1000ms` → `1.0s`）。

## 5. 热门讨论

本期未提供讨论数据。

## 6. 功能请求趋势

- **更智能、更自主的 subagent 编排** — 多个讨论主题汇聚于 agent *选择*使用技能/subagent（[#21968](https://github.com/google-gemini/gemini-cli/issues/21968)），使用共享内存并行运行它们（[#18287](https://github.com/google-gemini/gemini-cli/issues/18287)），并暴露其轨迹以供审查（[#22598](https://github.com/google-gemini/gemini-cli/issues/22598)、[#21763](https://github.com/google-gemini/gemini-cli/issues/21763)）。
- **节省 token、结构感知的代码探索** — AST 感知 EPIC（[#22745](https://github.com/google-gemini/gemini-cli/issues/22745)、[#22746](https://github.com/google-gemini/gemini-cli/issues/22746)、[#22747](https://github.com/google-gemini/gemini-cli/issues/22747)）加上“Tactful Extraction”（[#19561](https://github.com/google-gemini/gemini-cli/issues/19561)）针对约 36.6k token/轮的基线和大型文件上下文膨胀。
- **持久化、基于文件的任务跟踪** — [#18836](https://github.com/google-gemini/gemini-cli/issues/18836) 和 [#21000](https://github.com/google-gemini/gemini-cli/issues/21000) 提议用真实文件上的 CRUD 替换上下文内的 `WriteToDo`，以在上下文腐烂和会话边界中存活。
- **默认更安全的执行** — [#22672](https://github.com/google-gemini/gemini-cli/issues/22672)（劝阻 `git reset --force` 式破坏性）和 [#19873](https://github.com/google-gemini/gemini-cli/issues/19873)（操作系统沙箱）都主张在不牺牲原生 bash 能力的情况下设置护栏。
- **按工作区策略和自我认知** — [#18397](https://github.com/google-gemini/gemini-cli/issues/18397)（工作区范围策略而非全局）和 [#21432](https://github.com/google-gemini/gemini-cli/issues/21432)（准确自我描述标志/热键）。

## 7. 开发者痛点

- **静默或误导性的 agent 结果。** Subagent 在实际上达到 `MAX_TURNS` 时报告 `GOAL`/`success`（[#22323](https://github.com/google-gemini/gemini-cli/issues/22323)），或在 Wayland 上直接失败（[#21983](https://github.com/google-gemini/gemini-cli/issues/21983)）；`list_directory` 报告成功但省略文件（[#29713](https://github.com/google-gemini/gemini-cli/issues/29713)）。对 agent 状态输出的信任是首要的反复出现的投诉。
- **挂起和交互式提示死锁。** 通用 agent 无限期停滞（[#21409](https://github.com/google-gemini/gemini-cli/issues/21409)），像 `create vite app` 这样的脚手架流程卡在交互式提示上（[#22465](https://github.com/google-gemini/gemini-cli/issues/22465)）。
- **配置未生效。** 浏览器 Agent 忽略 `settings.json` 覆盖（[#22267](https://github.com/google-gemini/gemini-cli/issues/22267)），符号链接的 agent 定义未被发现（[#20079](https://github.com/google-gemini/gemini-cli/issues/20079)）。
- **工具表面积扩展限制。** 随着 MCP 和 subagent 工具数量增长，超过 128/400 个工具时出现硬 400 错误（[#24246](https://github.com/google-gemini/gemini-cli/issues/24246)）。
- **工作区卫生和终端用户体验。** 模型在目录树中散落临时脚本（[#23571](https://github.com/google-gemini/gemini-cli/issues/23571)），不一致的 `\n` 转义（[#22466](https://github.com/google-gemini/gemini-cli/issues/22466)），以及调整大小/流式闪烁（[#21924](https://github.com/google-gemini/gemini-cli/issues/21924)，正由 [#29629](https://github.com/google-gemini/gemini-cli/pull/29629) 解决）。

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI — 社区摘要
**日期：** 2026-10-12 · **来源：** [github.com/github/copilot-cli](https://github.com/github/copilot-cli)

---

## 1. 今日亮点

过去 24 小时内没有新的发布、拉取请求或讨论帖，但 issue 活动非常密集：共有 22 个 issue 更新，重点关注 **MCP 身份验证失败**、**ACP/非交互式功能对等缺口** 以及 **Windows 上的插件生命周期问题**。若干长期存在的渲染和会话恢复 bug 被关闭，而新一批开放 issue 则凸显了远程会话（移动端附件）和 agent 循环可靠性方面的摩擦。

---

## 2. 发布

*过去 24 小时内没有新版本发布。*

---

## 3. 热点 Issue

1. **[#5119 [OPEN] Windows: uninstalling an installed plugin fails with "Access is denied" (os error 5)](https://github.com/github/copilot-cli/issues/5119)**
   新提交且已分类。卸载 `workiq` 插件会因操作系统级权限错误而失败。这个问题很重要，因为它使 Windows 用户无法干净地移除插件——这一生命周期缺口将阻碍插件生态的采用。目前还没有评论，因此维护者尚未回应。

2. **[#5118 [CLOSED] Atlassian Rovo MCP Server connector fails auth probe with HTTP 400, blocking all Atlassian tool calls](https://github.com/github/copilot-cli/issues/5118)**
   内置的 Atlassian Rovo 连接器无法通过授权探测，所有 Jira/Confluence 工具调用都返回 `401 Unauthorized`。影响很大：它会硬性阻断进行中的 agent 会话。当天即关闭，说明分类/修复路径很快，或者通过文档方案解决。

3. **[#5101 [OPEN] `--add-github-mcp-tool issue_write` causes no MCP tools to be available](https://github.com/github/copilot-cli/issues/5101)**
   单个 CLI 标志似乎会静默禁用整个 GitHub MCP 服务器工具集，而不是将其收窄到 `issue_write`。对于将该标志写入脚本的自动化和 CI 用例来说，这是一个危险的陷阱。目前评论量很少，但严重性很高。

4. **[#5049 [OPEN] Computer Use plugin unavailable in ACP mode despite being enabled in CLI (Windows, 1.0.91)](https://github.com/github/copilot-cli/issues/5049)**
   ACP 会通告 `/computer`，但捆绑插件及其 MCP 服务器从未在会话中出现。这代表了一种更普遍的模式：交互式 CLI 功能无法延续到 ACP/无头模式。

5. **[#5115 [OPEN] ACP: `/rename` reports success but doesn't rename the session](https://github.com/github/copilot-cli/issues/5115)**
   该命令会在 `available_commands_update` 中通告，并在约 1 ms 内返回带有成功消息的 `end_turn`，但重命名从未发生。ACP 协议中的静默空操作对于构建在其上的工具尤其有害。

6. **[#5116 [OPEN] Intermittent 400 internal server error on response continuation after tool output](https://github.com/github/copilot-cli/issues/5116)**
   多步会话在通过 `previous_response_id` 提交延续时，会间歇性地以 `Execution failed: 400 internal server error` 失败。会话中途不稳定的终止是长时间 agent 运行中最具破坏性的故障模式之一。

7. **[#5114 [OPEN] Copilot CLI doesn't continue when prompted to "continue" after hitting the session limit](https://github.com/github/copilot-cli/issues/5114)**
   在遇到 429 限制并等待五小时后，会话仍然拒绝恢复，并报告先前的限制“仍在阻塞”工作。用户正在浪费时间等待实际上不会为该会话重置的限制。

8. **[#4469 [CLOSED] Orphaned `permission.requested` event replays on every session resume](https://github.com/github/copilot-cli/issues/4469)**
   一个被反复恢复的会话会重放针对 10 天前已完成命令的目录访问提示，而且该提示无法关闭。这有力地说明了会话状态（事件日志）没有与已完成的工作进行协调。在经历了较长的生命周期后关闭（8 月提交，10 月关闭）。

9. **[#3755 [CLOSED] Reasoning/thinking display garbles streamed text with duplicated overlapping chunks](https://github.com/github/copilot-cli/issues/3755)**
   在 `showReasoning: true` 时，流式推理会重复发出单词边界（“from” → “fromply from”）。这是本批中互动最多的条目（6 条评论，2 个 👍），也是一个长期存在的终端渲染投诉；现已关闭。

10. **[#5082 [CLOSED] MCP OAuth: Dataverse MCP rejected by RFC 8414 issuer check (`login.windows.net` vs `login.microsoftonline.com`)](https://github.com/github/copilot-cli/issues/5082)**
    严格的 issuer 与 metadata URL 比较会阻止 Dataverse/Dynamics 365 MCP 身份验证，无论是否使用静态 `oauthClientId`。这关系到任何使用别名 issuer 主机的企业 IdP——一种值得关注的合规与互操作之间的张力。

---

## 4. 关键 PR 进展

*过去 24 小时内没有拉取请求更新。*

---

## 5. 热门讨论

*本周期未提供讨论数据。*

---

## 6. 功能请求趋势

从 22 个已更新 issue 中提炼：

- **插件生命周期管理** — Windows 上安装/卸载的可靠性 ([#5119](https://github.com/github/copilot-cli/issues/5119)) 以及自动清理过期的已安装版本（仅保留最新三个，[#5077](https://github.com/github/copilot-cli/issues/5077)）。
- **ACP / 非交互式功能对等** — 捆绑插件和会话命令在 `--acp --stdio` 中必须与交互式使用时表现一致 ([#5049](https://github.com/github/copilot-cli/issues/5049), [#5115](https://github.com/github/copilot-cli/issues/5115))。
- **远程与移动会话能力** — 附件（文件/图片）在桌面端/Web 端受支持，但在 GitHub Mobile 的撰写界面中没有任何入口 ([#5117](https://github.com/github/copilot-cli/issues/5117))。
- **忽略文件语义** — 明确 `.copilotignore` 的行为，尤其是嵌套忽略文件 ([#3785](https://github.com/github/copilot-cli/issues/3785))。
- **终端用户体验打磨** — 更清晰的通知/标签页标题，以区分“工作中”与“等待输入” ([#5078](https://github.com/github/copilot-cli/issues/5078))，并持续修复推理流和快捷键的渲染问题。
- **会话连续性** — 在速率限制后可靠恢复，并且不重放过期的权限事件 ([#4469](https://github.com/github/copilot-cli/issues/4469), [#5114](https://github.com/github/copilot-cli/issues/5114))。

---

## 7. 开发者痛点

- **MCP 身份验证和 OAuth 是最大的可靠性风险。** RFC 8414 issuer 不匹配 ([#5082](https://github.com/github/copilot-cli/issues/5082))、Atlassian Rovo 探测失败 ([#5118](https://github.com/github/copilot-cli/issues/5118))，以及单个标志清除所有 GitHub MCP 工具 ([#5101](https://github.com/github/copilot-cli/issues/5101))，都指向一个脆弱的集成层，会静默降低 agent 能力。
- **瞬时服务器错误会中断长时间 agent 运行。** 在 subagent/fleet 执行期间反复出现“server error, retrying”刷屏 ([#4226](https://github.com/github/copilot-cli/issues/4226))，以及 `previous_response_id` 延续时间歇性出现 400 ([#5116](https://github.com/github/copilot-cli/issues/5116))，使多步工作不可靠。
- **会话状态在恢复时未对账。** 过期的权限事件会为已完成的命令重放 ([#4469](https://github.com/github/copilot-cli/issues/4469))，而受速率限制的会话在上限窗口过后仍无法继续 ([#5114](https://github.com/github/copilot-cli/issues/5114))。
- **配置并不总是被遵守。** Subagent 忽略模型继承设置 ([#4287](https://github.com/github/copilot-cli/issues/4287))、在 `extensions_reload` 之后钩子被静默丢弃 ([#3167](https://github.com/github/copilot-cli/issues/3167))，以及 `"bell": true` 不生效 ([#3573](https://github.com/github/copilot-cli/issues/3573))，都会削弱对设置界面的信任。
- **终端输入/渲染边缘情况依然存在。** 右键复制同时粘贴到输入框 ([#4126](https://github.com/github/copilot-cli/issues/4126))、在 `ask_user` 中按 Home/End 会滚动时间线 ([#5113](https://github.com/github/copilot-cli/issues/5113))，以及推理流乱码 ([#3755](https://github.com/github/copilot-cli/issues/3755))，都表明键盘/渲染 bug 持续不断。
- **Windows 特有的插件摩擦。** 卸载时出现访问被拒错误 ([#5119](https://github.com/github/copilot-cli/issues/5119)) 以及 Windows 上 ACP 插件不可用 ([#5049](https://github.com/github/copilot-cli/issues/5049))，表明平台特有的打包/权限处理需要关注。

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区摘要 — 2026-10-12

## 今日亮点
过去 24 小时内没有发布新版本，但有 50 个 issue 和 50 个 PR 有更新。社区关注点集中在免费套餐/权限不一致、提供商设置不匹配，以及服务重启、迁移和静默失败方面的可靠性缺口。PR 动态显示生成式 UI、会话/标签页管理、MCP 连接治理和 TUI 可读性方面有所进展。

## 热点 Issue
1. **#50155 — DeepSeek V4 Flash 需要 Global regions，但缺少 Privacy 设置** [OPEN，13 条评论，10 👍]  
   付费 OpenCode Go 用户遇到上游错误，提示他们选择 Global regions，但据称该设置不可用。高点赞数使其成为显眼的订阅/提供商配置阻塞问题。  
   [anomalyco/opencode#50155](https://github.com/anomalyco/opencode/issues/50155)

2. **#42950 — 间歇性 socket 断开导致静默中断（无 UI 错误）** [CLOSED，11 条评论，1 👍]  
   `big-pickle` 流可能会因 `AI_APICallError` 和反复出现的 “Aborted” 日志而中断，而 UI 不会暴露该失败。已关闭，但它凸显了用户对流可靠性的期望。  
   [anomalyco/opencode#42950](https://github.com/anomalyco/opencode/issues/42950)

3. **#50627 — 对自定义 agent 拒绝 `shell *` 会破坏免费套餐** [OPEN，9 条评论]  
   在自定义主 agent 上添加 shell 拒绝权限后，免费套餐模型会失败并提示 “can only be used from within OpenCode”，即使在 TUI 内也是如此。这指向令人困惑的权限策略交互。  
   [anomalyco/opencode#50627](https://github.com/anomalyco/opencode/issues/50627)

4. **#43596 — 可配置重试策略：暴露 maxRetries / initialDelay / backoffFactor / maxDelay** [CLOSED，8 条评论，13 👍]  
   用户希望可配置重试行为，因为硬编码重试会在提供商错误仍是暂时性时中止轮次。较高的 👍 数表明这是一项广泛需求的控制能力。  
   [anomalyco/opencode#43596](https://github.com/anomalyco/opencode/issues/43596)

5. **#54267 — 服务重新启动失败，提示 “Managed service port already in use”** [OPEN，7 条评论]  
   可复现问题：先前的后台服务尚未退出，导致 `opencode` 在固定的托管服务端口上重新启动失败。服务生命周期健壮性仍是实际痛点。  
   [anomalyco/opencode#54267](https://github.com/anomalyco/opencode/issues/54267)

6. **#47742 — 支持共享 `.agents/` 目录和基于 Manifest 的发现** [OPEN，5 条评论，2 👍]  
   该提案旨在减少工具专属 agent 目录造成的仓库蔓延并提升互操作性。这表明对跨 agent 配置标准的需求日益增长。  
   [anomalyco/opencode#47742](https://github.com/anomalyco/opencode/issues/47742)

7. **#54477 — GitHub action：权限解析为 “ask” 时永久挂起** [CLOSED，4 条评论]  
   `anomalyco/opencode/github` action 会无限期阻塞，因为处理器没有订阅任何权限事件。对 CI/自动化用户很重要，不过现已关闭。  
   [anomalyco/opencode#54477](https://github.com/anomalyco/opencode/issues/54477)

8. **#54744 — 免费套餐 403 拒绝受限子 agent，即使已允许 bash** [OPEN，3 条评论]  
   新报告称，工具受限的自定义子 agent 在免费套餐模型上失败，而内置 agent 成功。这强化了免费套餐/权限混乱这一主题。  
   [anomalyco/opencode#54744](https://github.com/anomalyco/opencode/issues/54744)

9. **#54733 — TUI 在已弃用模型上无限挂起并显示 spinner** [OPEN，2 条评论]  
   当模型在服务端被弃用后，TUI 会一直转圈，且 `esc` 无法中断，而 `opencode run` 能正确报告问题。TUI 中的错误暴露需要改进。  
   [anomalyco/opencode#54733](https://github.com/anomalyco/opencode/issues/54733)

10. **#54736 — V1 迁移因旧版消息 ID 不符合 `Session.Message.ID` schema 而中止** [OPEN，2 条评论]  
    从 V1 升级到 v2.0.27 可能失败，因为旧版消息 ID 不匹配新的 `msg_` schema。这对现有用户是严重的升级阻塞问题。  
    [anomalyco/opencode#54736](https://github.com/anomalyco/opencode/issues/54736)

## 重点 PR 进展
1. **#54747 — feat(app)：为 GUI 添加生成式 UI**  
   Agent 可以通过 `genui_render` 输出 OpenUI Lang，在模型仍在流式传输时进行交互式渲染；按钮点击和表单提交会作为用户消息返回。这是 GUI 能力的一次重要扩展。  
   [anomalyco/opencode#54747](https://github.com/anomalyco/opencode/pull/54747)

2. **#54749 — feat(app,tui)：将别处创建的会话作为后台标签页打开**  
   可选择启用一项设置，以展示由 `opencode run`、SDK、codemode 或其他客户端创建的会话，而无需手动搜索。重复/已关闭的配套项：#54746。  
   [anomalyco/opencode#54749](https://github.com/anomalyco/opencode/pull/54749)

3. **#54748 — refactor(app)：从服务端已查看水位标记推导未读状态**  
   使桌面端未读状态与 TUI 已使用的服务端会话跟踪保持一致，移除了桌面端本地的通知列表行为。  
   [anomalyco/opencode#54748](https://github.com/anomalyco/opencode/pull/54748)

4. **#52338 — feat(desktop)：在 Linux 上集成原生窗口控件**  
   使用 Electron 43 在 Linux 上添加原生窗口控件叠加层，关闭 #36225。使 Linux 桌面端与早先的 Windows 叠加层工作保持一致。  
   [anomalyco/opencode#52338](https://github.com/anomalyco/opencode/pull/52338)

5. **#53925 — fix(server)：避免为待处理的 prompt 读取启动冷 Location**  
   在没有 prompt 存在时，避免获取会话的完整 Location 图，改为使用作用域内的缓存检查。关闭 #53918。  
   [anomalyco/opencode#53925](https://github.com/anomalyco/opencode/pull/53925)

6. **#53050 — fix(app)：在 MCP 发现期间预留聊天请求槽位**  
   将 `/api/command`、`/api/mcp` 及嵌套路由纳入慢请求配额，使 MCP 发现不会饿死聊天请求。  
   [anomalyco/opencode#53050](https://github.com/anomalyco/opencode/pull/53050)

7. **#53048 — fix(app)：无需重新加载即可重试失败的会话元数据**  
   将会话错误边界重置回调传递给错误页面，使会话加载失败后无需完整重新加载即可恢复。  
   [anomalyco/opencode#53048](https://github.com/anomalyco/opencode/pull/53048)

8. **#53046 — fix(mcp)：回收仅用于发现的连接**  
   延迟启动 MCP，并释放仅为命令/工具/资源发现而打开的连接。修复了仅用于发现的 MCP 连接问题 #51003。  
   [anomalyco/opencode#53046](https://github.com/anomalyco/opencode/pull/53046)

9. **#54707 — feat(tui)：将插件和 MCP 工具输入显示为可读块**  
   提升插件/MCP 工具调用的 TUI 可检查性。指出 v1 现在是维护分支；计划在迁移后推出 v2 版本。  
   [anomalyco/opencode#54707](https://github.com/anomalyco/opencode/pull/54707)

10. **#54705 — feat(tui)：按自身语言为 bash 命令中的 heredoc 着色**  
    基于 #54704 的 bash 命令着色，增加语言感知的 heredoc 高亮。这是更广泛的 TUI 可读性改进的一部分。  
    [anomalyco/opencode#54705](https://github.com/anomalyco/opencode/pull/54705)

## 功能请求趋势
- **权限/策略清晰度：** 免费套餐资格、shell 拒绝行为和子 agent 工具限制是反复出现的主题（#50627、#54744）。
- **提供商/设置 UX：** DeepSeek V4 Flash 缺少 Global 隐私设置（#50155）、`/connect` 中的交互式自定义提供商注册（#49091），以及模型选择器为空的问题（#54092）。
- **Agent 配置互操作性：** 共享 `.agents/` 目录和基于 manifest 的发现（#47742）。
- **TUI/CLI 人体工学：** 专用文件附件按钮（#54743）、关闭按钮位置（#54438）、可读的插件/MCP 工具输入（#54703），以及 bash/heredoc 语法着色（#54700、#54701）。
- **可靠性/可运维性：** 可配置重试策略（#43596）、服务重启/端口处理（#54267）、迁移/升级健壮性（#54736），以及更清晰的已弃用模型错误（#54733）。
- **可扩展性/集成：** Reverse Engineer Anything（#54587）、生成式 UI（#54747），以及原生 Linux 窗口控件（#52338）。

## 开发者痛点
- **免费套餐权限门控不一致且不透明：** 有效的应用内请求可能失败并提示 “can only be used from within OpenCode”，而自定义子 agent 即使允许 bash 也可能返回 403（#50627、#54744）。
- **静默失败和挂起：** socket 断开但没有 UI 错误（#42950）、已弃用模型 spinner 无错误（#54733），以及 GitHub Action 权限 `ask` 无限期挂起（#54477）。
- **升级/迁移脆弱性：** V1→V2 迁移 schema 校验可能因旧版消息 ID 而中止（#54736），并且有报告称更新到 2.0 失败（#54469）。
- **托管服务生命周期问题：** 先前的后台服务尚未退出时，重新启动可能失败（#54267）。
- **提供商/模型配置不匹配：** DeepSeek V4 Flash 缺少 Global 隐私设置（#50155）、模型选择器为空（#54092），以及 Copilot URL 不正确（#45302）。
- **重试/退避不可配置：** 硬编码的重试限制会过早中止暂时性提供商错误（#43596）。
- **TUI/桌面端打磨缺口：** 泰文组合标记宽度处理（#49348）、关闭按钮位置（#54438）、文件附件流程（#54743）、插件/MCP 输入可读性（#54703），以及 bash 着色（#54700/#54701）。
- **MCP/请求资源压力：** MCP 发现可能占用慢请求槽位，并让仅用于发现的连接保持打开（#53050、#53046）。

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi 社区摘要 — 2026-10-12

## 今日亮点
过去 24 小时内没有新版本发布，但 issue 流量很大：共有 50 个 issue 被更新，主要集中在 OpenAI/ChatGPT 身份验证和配额状态 bug，以及 codemode 和 TUI 中的一组 1.1.0 回归问题。PR 方面，维护者关闭了多个长期存在的扩展生命周期修复——尤其是 `before_agent_start` 现在会为由自定义消息启动的运行触发（[#10739](https://github.com/earendil-works/pi/pull/10739)）——并交付了 [#10353](https://github.com/earendil-works/pi/issues/10353) 中请求的 OpenRouter 按密钥模型过滤。唯一仍开放的 PR 是 [#10795](https://github.com/earendil-works/pi/pull/10795)，一个 Anthropic 工具结果图像修复。

## 热门 Issue

1. **[#4945](https://github.com/earendil-works/pi/issues/4945) — openai-codex 连接可靠性问题**（OPEN，inprogress，82 条评论，35 👍）
   社区中最热门的讨论帖，热度遥遥领先。通过 `openai-codex` 使用 `gpt-5.5` 时，交互式 TUI 会间歇性卡在 `Working...`，没有流式文本、没有工具调用，也没有错误；唯一的退出方式是 `Esc`，这会记录一次已中止的回合。高点赞数表明该问题被广泛复现，并非边缘情况。

2. **[#10480](https://github.com/earendil-works/pi/issues/10480) — 直接 openai 连接无法识别手动使用限制重置**（OPEN，18 条评论）
   在直接 OpenAI 连接上，已积存的 ChatGPT Pro 重置被忽略，导致用户被锁定，直到他们 `/logout` 并通过 `openai-codex` 重新登录。这是一个令人痛苦的计费/权益正确性 bug，但有已知的临时解决办法。

3. **[#10605](https://github.com/earendil-works/pi/issues/10605) — ChatGPT/OpenAI OAuth 403 问题**（OPEN，10 条评论，1 👍）
   Plus 层级用户在登出/登录后遇到 `subscription_sharing_user_not_eligible`。与 #10480 属于同一类问题，很可能是更广泛的凭据/权益状态问题的一部分。

4. **[#7553](https://github.com/earendil-works/pi/issues/7553) — 可为压缩配置 thinking level/model**（OPEN，inprogress/to-discuss，10 条评论，3 👍）
   压缩会无条件继承会话的 thinking level，因此摘要会消耗与正常回合相同的推理预算。这是一个范围明确、被反复请求的调节选项，面向在推理模型上运行自动压缩的用户。

5. **[#10267](https://github.com/earendil-works/pi/issues/10267) — 非用户提示运行中 `before_agent_start` 的提示文本被丢弃**（CLOSED，p0，9 条评论，4 👍）
   被标记为 P0，因为该丢弃会导致在后台任务通知、计划模式继续、重试和恢复时，整个提示被重新计费。已由 [#10739](https://github.com/earendil-works/pi/pull/10739) 修复。

6. **[#5581](https://github.com/earendil-works/pi/issues/5581) — `pi.sendMessage()` 配合 `triggerTurn: true` 会绕过 `before_agent_start`**（CLOSED，9 条评论，4 👍）
   这是一系列扩展 bug 的根本原因：agent 循环被直接通过 `_runAgentPrompt` 启动，跳过了 `emitBeforeAgentStart`，并在运行中途静默更改系统提示。

7. **[#9602](https://github.com/earendil-works/pi/issues/9602) — 压缩可能因包含此前请求中省略的 thinking 消息而溢出**（OPEN，9 条评论）
   通过 llama.cpp 使用本地 Qwen3.8 时，之前请求中排除的 thinking 内容仍会被计入压缩，导致溢出。这是日益增长的压缩正确性积压中的又一项。

8. **[#10645](https://github.com/earendil-works/pi/issues/10645) — `resizeImage` 在编译后的 Bun 可执行文件中解析为 null**（OPEN，inprogress，6 条评论）
   来自 `v0.87.1`/`v1.0.4` 的独立二进制文件会静默丢弃所有图像附件，因为 `resizeImage` 在 Bun 编译下失败。这是影响 RPC 集成方的功能性回归，可通过模拟网关清晰复现。

9. **[#10821](https://github.com/earendil-works/pi/issues/10821) — 长时间会话中终端 resize 会冻结 UI**（CLOSED，2 条评论）
   每个 resize 帧都会同步重新布局整个会话记录；在 Windows Terminal/VS Code 终端中严重退化。这是 1.1.0 时代的 TUI 性能回归。

10. **[#10678](https://github.com/earendil-works/pi/issues/10678) — codemode `joinAdjacentText` 是二次复杂度**（CLOSED，3 条评论，1 👍）
    在 1.1.0 中引入：脚本调用 `text()` 数万次会阻塞宿主线程，因为连接同步运行且具有二次成本。修复速度很快，但这是一个明显的发布质量疏漏。

## 关键 PR 进展

1. **[#10739](https://github.com/earendil-works/pi/pull/10739) — 为由自定义消息启动的运行触发 `before_agent_start`**（CLOSED）
   关闭了 [#5581](https://github.com/earendil-works/pi/issues/5581) 和 [#10267](https://github.com/earendil-works/pi/issues/10267)：下一回合刷新不再抹去处理器添加的提示片段，从而保持系统提示稳定和提供商提示缓存。

2. **[#10672](https://github.com/earendil-works/pi/pull/10672) — 仅列出某个密钥可使用的 OpenRouter 模型**（CLOSED）
   将目录与 `GET /models/user` 结合，保留来自已认证响应的上下文大小、最大输出和定价。实现了 [#10353](https://github.com/earendil-works/pi/issues/10353)。

3. **[#10820](https://github.com/earendil-works/pi/pull/10820) — 按端点而非提供商 id 发送 `x-opencode-session`**（CLOSED）
   泛化了 [#9326](https://github.com/earendil-works/pi/pull/9326)：任何路由到 OpenCode zen 端点的提供商现在都会获得按对话的路由头，从而避免 `400 MissingSessionID`。

4. **[#10819](https://github.com/earendil-works/pi/pull/10819) — 在 minimal-mode 示例中使用 `registerToolRenderer`**（CLOSED）
   文档修复，但后果真实：使用 `pi.registerTool({...})` 重新注册内置项会完全替换 `execute`，静默破坏那些重路由内置工具执行的扩展。

5. **[#10813](https://github.com/earendil-works/pi/pull/10813) — 保留客户端加载的 Anthropic 工具引用**（CLOSED）
   在 Anthropic 提供商路径中保持客户端提供的工具引用不变。

6. **[#10774](https://github.com/earendil-works/pi/pull/10774) — 在 herdr 中启用 kitty 图像**（CLOSED）
   回退了 1.1.0 中一个非预期的回归：尽管 herdr 支持 kitty 图形协议，`#10573` 却为 herdr 窗格返回 `images: null`。

7. **[#10810](https://github.com/earendil-works/pi/pull/10810) — 支持在 `CompactionStatusIndicator` 中覆盖 `colorFn` 选项**（CLOSED）
   修复了 [#10809](https://github.com/earendil-works/pi/issues/10809)；为未设置 `borderColor` 的主题，将压缩指示器与 `WorkingIndicator` 对齐。

8. **[#10805](https://github.com/earendil-works/pi/pull/10805) — 支持扩展会话 cwd**（CLOSED）
   为 `ctx.newSession()` 添加 `cwd`，为 `ctx.switchSession()` 添加 `cwdOverride`，完善了扩展此前已可通过 `pi.exec` 脚本化的 git-worktree 工作流。

9. **[#10751](https://github.com/earendil-works/pi/pull/10751) — 使用 pi.dev 配置 schema**（CLOSED）
   将 pi.dev schema 端点作为生成的设置、模型、键位绑定和主题 schema 的规范 `$id` 值，并在内置项/示例中采用已发布的主题 schema。

10. **[#10795](https://github.com/earendil-works/pi/pull/10795) — 从 Anthropic 错误工具结果中移除图像**（OPEN）
    唯一仍开放的 PR；通过从 Anthropic 错误工具结果中剥离图像来修复 [#10794](https://github.com/earendil-works/pi/issues/10794)，并附有前后会话跟踪。

## 热门讨论

**展示与分享**
- **[#10808](https://github.com/earendil-works/pi/discussions/10808) — Pippa，面向日常 Pi 任务的原生 Mac 界面**（1 👍）
  一个免费、MIT 许可的 Swift macOS 应用，通过 `pi --mode rpc` 与 Pi 通信，让非终端用户可以将一段文字或文件夹拖放到浮动胶囊上并选择任务。本地模型通过 llama.cpp 运行，并在其上叠加 Mac 集成；作者正在招募测试者。

## 功能请求趋势

- **压缩控制与正确性**：整个时间窗口中最突出的主题。请求涵盖为压缩设置专用的 thinking level/model（[#7553](https://github.com/earendil-works/pi/issues/7553)）、thinking 消息导致的溢出（[#9602](https://github.com/earendil-works/pi/issues/9602)）、预提示压缩期间的中止语义（[#10289](https://github.com/earendil-works/pi/issues/10289)），以及传输设置不应用于摘要请求（[#10781](https://github.com/earendil-works/pi/issues/10781)）。
- **提供商权益与模型发现**：用户希望 Pi 的 UI 反映真实账户状态，而不是静态目录——按密钥过滤 OpenRouter 模型（[#10353](https://github.com/earendil-works/pi/issues/10353)）、遵循 ChatGPT 使用量重置（[#10480](https://github.com/earendil-works/pi/issues/10480)），以及在已保存的默认模型消失时发出警告，而不是静默回退（[#10812](https://github.com/earendil-works/pi/issues/10812)）。
- **扩展生命周期保证**：一致的 `before_agent_start` 语义（[#5581](https://github.com/earendil-works/pi/issues/5581)、[#10267](https://github.com/earendil-works/pi/issues/10267)、[#10639](https://github.com/earendil-works/pi/issues/10639)）、安全的渲染器覆盖（[#10819](https://github.com/earendil-works/pi/pull/10819)），以及面向 worktree 流程的会话 cwd 控制（[#10805](https://github.com/earendil-works/pi/pull/10805)）。
- **工具/MCP 声明保真度**：提示段落必须与实际暴露的工具匹配——通过 `--no-tools`/`--exclude-tools` 移除的 MCP 服务器仍被声明（[#10635](https://github.com/earendil-works/pi/issues/10635)），codemode 会丢弃 `minimum`/`maximum`/`default` 等输入约束（[#10707](https://github.com/earendil-works/pi/issues/10707)），并且请求支持 Anthropic `thinking.display: "updates"`（[#10804](https://github.com/earendil-works/pi/issues/10804)）。
- **无头/无人值守 agent 健壮性**：针对 max-token 停止的显式继续路径（[#10799](https://github.com/earendil-works/pi/issues/10799)）、自定义提供商上的通用分类器模型支持（[#10791](https://github.com/earendil-works/pi/issues/10791)），以及通过 ChatGPT `file_id` 引用实现更便宜的图像处理（[#8617](https://github.com/earendil-works/pi/issues/8617)）。

## 开发者痛点

- **提供商连接性和账户状态是最反复出现的挫败点。** `openai-codex` 上的 TUI 静默卡死（[#4945](https://github.com/earendil-works/pi/issues/4945)）以及 403/配额状态不匹配（[#10480](https://github.com/earendil-works/pi/issues/10480)、[#10605](https://github.com/earendil-works/pi/issues/10605)）迫使用户登录、采用变通方法并中止回合。
- **静默失败多于明显错误。** 被丢弃的 RPC 提示却以 `success: true` 确认（[#10606](https://github.com/earendil-works/pi/issues/10606)）、跳过 `before_agent_start` 调用（[#10639](https://github.com/earendil-works/pi/issues/10639)）、忽略默认模型（[#10812](https://github.com/earendil-works/pi/issues/10812)），以及输出限制停止后无声结束（[#10799](https://github.com/earendil-works/pi/issues/10799)），都会耗费调试时间，因为没有任何信息浮出水面。
- **1.1.0 回归集群。** codemode 文本连接的二次复杂度（[#10678](https://github.com/earendil-works/pi/issues/10678)）、resize 时同步重新布局整个会话记录（[#10821](https://github.com/earendil-works/pi/issues/10821)），以及损坏的 herdr 内联图像（[#10774](https://github.com/earendil-works/pi/pull/10774)），都表明 TUI 和 codemode 路径的发布测试存在缺口。
- **生命周期 bug 导致的提示缓存和成本回归。** 当贡献的提示文本被丢弃时，整个提示被重新计费（[#10267](https://github.com/earendil-works/pi/issues/10267)），以及运行中途更改系统提示破坏提供商缓存（[#10739](https://github.com/earendil-works/pi/pull/10739)），都直接冲击用户的钱包。
- **平台特定的尖锐问题。** Windows 上认证锁获取因瞬态 `EPERM` 失败（[#10801](https://github.com/earendil-works/pi/issues/10801)）、macOS 上 bash 超时后残留的 Playwright Chrome 进程（[#10797](https://github.com/earendil-works/pi/issues/10797)）、Windows + Zellij 下鼠标滚轮目标混乱（[#9656](https://github.com/earendil-works/pi/issues/9656)），以及编译后的 Bun 二进制文件中损坏的图像附件（[#10645](https://github.com/earendil-works/pi/issues/10645)），都使平台覆盖持续处于关键路径上。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区简报 — 2026-10-12

数据来源：[github.com/QwenLM/qwen-code](https://github.com/QwenLM/qwen-code)

---

## 1. 今日亮点

发布活动围绕 **v0.25.1** 线展开，配套 SDK（TypeScript v0.1.19）和 Desktop（v0.25.1）制品。主要工程主题仍是 **Managed Agent 架构**（持久化 Session、writer fencing/takeover、团队消息、L4 Shell/MCP 生命周期），它继续推动大多数高热度 issue 和 PR。与此同时，围绕 **UTF-8 BOM 配置文件静默失败** 和 **CI/发布工具链可靠性**（Flyway 排序、CVE 审计、重复发布说明）的一批新报告，凸显出配置健壮性和自动化方面日益增长的摩擦。

---

## 2. 发布

| 发布 | 说明 |
|---|---|
| [v0.25.1](https://github.com/QwenLM/qwen-code/releases) | 稳定的 CLI 发布版本。自动生成的说明；唯一实质性功能条目提到在 Android Activity 重建后恢复纯文本连接以及经验证的 session/workspace 路由。说明中未填充完整变更列表。 |
| [v0.25.1-nightly.20261011.01d3e6d81a](https://github.com/QwenLM/qwen-code/releases) | Nightly 包含 `fix(managed-agent): connector and broker robustness`（#13330）以及额外的 managed-agent 工作。 |
| [sdk-typescript-v0.1.19](https://github.com/QwenLM/qwen-code/releases) | 捆绑 CLI 版本 0.25.1（与 SDK 从同一源分支/ref 构建）。发布正文还引用了捆绑的 CLI 0.25.0，表明说明生成可能陈旧/重复。 |
| [desktop-v0.25.1](https://github.com/QwenLM/qwen-code/releases) | Desktop 发布包含 `fix(serve): preserve session creation failure diagnostics`（#12331）和 `feat(sdk-java): Add managed runtime` 工作；提供 `.deb`（amd64/arm64）和 AppImage 制品。 |

> ⚠️ 注意：本时间窗口的发布说明似乎部分重复——与开放 issue [#13925](https://github.com/QwenLM/qwen-code/issues/13925) 直接相关。

---

## 3. 热门 Issue

1. **[#12380 — Proposal(serve): Define Managed Agent dual-path architecture and staged delivery](https://github.com/QwenLM/qwen-code/issues/12380)** *（56 条评论，OPEN）*
   总体设计：在保留 TypeScript agent loop 的同时，将推理与工具环境供给解耦，并具备持久化 Session 所有权、Workspace 绑定、可恢复的工具执行以及稳定的 Web Shell 界面。这是仓库中讨论最多的 issue，也是大多数 managed-agent 工作流（Stage G、H3、H4）的父项。

2. **[#13078 — Daily dependency CVE audit failed](https://github.com/QwenLM/qwen-code/issues/13078)** *（18 条评论，OPEN）*
   一个定时安全审计任务反复失败，维护者正在调查它是真实的高危发现，还是 npm audit 端点不稳定。社区高度关注，因为它把关供应链卫生。

3. **[#12952 — Stage G: authoritative Session history, writer fencing and takeover](https://github.com/QwenLM/qwen-code/issues/12952)** *（8 条评论，OPEN）*
   定义了将 Session 历史/检查点外部化以及在移除 owner affinity 前证明 writer fencing 的切片和退出检查。这是多写入者/托管 session 的架构枢纽。

4. **[#13680 — [P1] Desktop Linux vendored ripgrep rewritten post-link, SIGSEGVs on startup probe](https://github.com/QwenLM/qwen-code/issues/13680)** *（4 条评论，OPEN）*
   `x64-linux` 的 `rg` 二进制被注入了 `DT_RUNPATH $ORIGIN`，且 ELF program headers 被移动，导致每次启动探测都崩溃。本时间窗口唯一的 P1——Linux 桌面用户的硬阻塞。

5. **[#13807 — "ERROR 500 An unsupported generation guide was used" with macOS Foundation Models as Fast Model](https://github.com/QwenLM/qwen-code/issues/13807)** *（6 条评论，OPEN）*
   工具摘要正常，但在通过 `openai:system…` provider 字符串使用 `fm serve` 时，fast-model 路径失败。指向本地 OpenAI 兼容端点的 provider/路由边界情况。

6. **[#13924 — Publish Qwen Code Desktop on Flathub with a working update channel](https://github.com/QwenLM/qwen-code/issues/13924)** *（5 条评论，OPEN）*
   请求通过 Flathub 分发，以便 Linux 应用商店（例如 Mint Software Manager）能够上架该应用并提供更新。基于已经发布的 `.deb`/AppImage 制品。

7. **[#13875 — [H4f] Parent close waits for running child Turn instead of stopping it](https://github.com/QwenLM/qwen-code/issues/13875)** *（4 条评论，OPEN）*
   先取消再关闭会让子 Session 继续执行。这是多 agent 生命周期语义中的正确性缺口，直接影响取消保证。

8. **[#13922 — Flyway migrations numbered below main's newest pass both version checks](https://github.com/QwenLM/qwen-code/issues/13922)** *（4 条评论，OPEN）*
   两个唯一性门禁只检查重复项，不检查顺序，因此升级后的数据库可能拒绝启动。值得注意的是，它会让有问题的迁移以绿色状态合并。

9. **[#13944 / #13943 — UTF-8 BOM config files silently skipped](https://github.com/QwenLM/qwen-code/issues/13944)** *（3 条评论，OPEN）*
   带有 BOM 的 `.toml` 自定义命令文件会被丢弃（`Unknown character "65279"`），而带 BOM 前缀的 `.lsp.json` 会导致整个文件被丢弃，且仅有 debug 日志——因此没有 LSP 服务器启动，用户也看不到错误。相关：[#13710](https://github.com/QwenLM/qwen-code/issues/13710) 因 Claude MCP 导入而关闭。

10. **[#13925 — Release notes duplicate PRs released in previous releases](https://github.com/QwenLM/qwen-code/issues/13925)** *（3 条评论，OPEN）*
    Desktop 发布说明在不断累加条目，而不是限定在当前发布——这对跟踪实际发布内容的下游用户来说是一个明显的信任问题。

*另值得关注：* [#13951](https://github.com/QwenLM/qwen-code/issues/13951)（CI checkout `pack has 1 unresolved delta`）、[#13920](https://github.com/QwenLM/qwen-code/issues/13920)（`sanitizeName` 中 CJK 队友名称坍缩）、[#13942](https://github.com/QwenLM/qwen-code/issues/13942)（多 provider、相同 model ID 切换摩擦）、[#13930](https://github.com/QwenLM/qwen-code/issues/13930)（H3 held-Runtime drain 后续事项）。

---

## 4. 关键 PR 进展

1. **[#13940 — feat(managed-agent): Java mirror of managed-event-envelope contract (MQ2a)](https://github.com/QwenLM/qwen-code/pull/13940)**
   EventTransport MQ2 阶段的第一个子切片：冻结的 `managed-event-envelope/1` 契约的 Java 镜像，以及一个证明跨语言一致性的 fixture consumer。

2. **[#13950 — feat(mods): Add tool interception and permission checks (PR3a)](https://github.com/QwenLM/qwen-code/pull/13950)**
   为可选启用的 Mods 添加 `tool.call` / `tool.check` 钩子：重写参数、拒绝调用、返回合成 JSON，并检查/替换原生工具结果——每次 `next` 都会重新进入原生验证、钩子和权限流程。

3. **[#13952 — feat(web-shell): Bridge trajectory history and live lifecycle state](https://github.com/QwenLM/qwen-code/pull/13952)**
   通过 session subscription 将历史 trajectory 窗口连接到有界的 request/tool 生命周期状态；重新连接会保留页面、选择、过滤器和滚动位置。

4. **[#13878 — feat(web-shell): Add workspace file browser and previews](https://github.com/QwenLM/qwen-code/pull/13878)**
   右侧面板中的 Files 操作，带有全宽 workspace 树、Markdown/HTML/图片渲染、只读源码检查，以及可调整大小/移除的树。

5. **[#13957 — fix(cli): Preserve Markdown in height-bounded tool results](https://github.com/QwenLM/qwen-code/pull/13957)**
   在已完成的 CLI 工具结果中恢复 Markdown 格式，同时保留终端高度限制、截断提示和展开功能——高度计算会考虑换行、列表标记、代码行号和表格。

6. **[#13777 — feat(managed-agent): Recover Hosted G3 parked turns](https://github.com/QwenLM/qwen-code/pull/13777)**
   在 Hosted 栈上实现 G3 parked turns 的恢复，这是迈向 Stage G 退出标准的关键持久化步骤。

7. **[#13931 — feat(managed-agent): L4 Shell and MCP session lifecycle](https://github.com/QwenLM/qwen-code/pull/13931)**
   为 `hosted-workspace-shell/1` 和 `hosted-workspace-mcp/1` 添加持久化 close/delete，在分离 writer 之前恰好一次地结算业务效果和 SessionEnd/SessionDelete 钩子。

8. **[#13955 — feat(managed-agent): Member-to-member messages ride the session_command channel (H4e-b3c)](https://github.com/QwenLM/qwen-code/pull/13955)**
   团队成员可以通过持久化 command channel 端到端互发消息，且不会在 lead 上启动 model turn。配套：[#13928](https://github.com/QwenLM/qwen-code/pull/13928)（member board tools）。

9. **[#13815 — fix(managed-agent): Numeric parity and family-list hoisting with end-to-end admission and wedge witnesses](https://github.com/QwenLM/qwen-code/pull/13815)**
   修复 capture-publication reserve 拒绝后台 Shell/Monitor payload 的问题，并在评审反复后添加端到端 admission 测试以及 wedge witnesses。([#13918](https://github.com/QwenLM/qwen-code/pull/13918) 解决相关的 tenant-wide InnoDB lock blast radius。)

10. **[#13954 — fix(ci): Enforce Flyway migration ordering](https://github.com/QwenLM/qwen-code/pull/13954)**
    两个 Flyway 门禁现在都会拒绝新添加的版本，除非它们严格大于扫描模块和 SQL/Java 位置中已发布的最高版本——直接关闭了 [#13922](https://github.com/QwenLM/qwen-code/issues/13922) 的缺口。

*另需注意：* [#13654](https://github.com/QwenLM/qwen-code/pull/13654)（异步 tool-publication 验证）、[#13642](https://github.com/QwenLM/qwen-code/pull/13642)（有界 JDBC 历史保留）、[#13929](https://github.com/QwenLM/qwen-code/pull/13929)（Monitor 自动停止退出证据）、[#13938](https://github.com/QwenLM/qwen-code/pull/13938)（已关闭 — Web Shell 语音反馈本地化）、[#13335](https://github.com/QwenLM/qwen-code/pull/13335)（已关闭 — config/API 表面卫生）。

---

## 5. 热门讨论

本时间窗口未提供讨论数据。本节省略。

---

## 6. 功能请求趋势

- **Managed Agent / 多 agent 平台（主导）：** 持久化 Session 历史和所有权、writer fencing 与接管、workspace 绑定、可恢复/经验证的工具执行、父子生命周期正确性、团队成员间消息和共享板，以及持久化 Shell/MCP session 生命周期。见 [#12380](https://github.com/QwenLM/qwen-code/issues/12380)、[#12952](https://github.com/QwenLM/qwen-code/issues/12952)、[#13875](https://github.com/QwenLM/qwen-code/issues/13875)、[#13271](https://github.com/QwenLM/qwen-code/issues/13271)。
- **Linux / 平台分发：** 通过 Flathub 上架并提供可用的更新通道，同时保留现有的 `.deb` 和 AppImage 目标。见 [#13924](https://github.com/QwenLM/qwen-code/issues/13924)。
- **Web Shell 人体工学：** 侧边栏中的 workspace/project 固定（[#12444](https://github.com/QwenLM/qwen-code/issues/12444)，重复的 [#13888](https://github.com/QwenLM/qwen-code/issues/13888) 已关闭）、文件浏览和预览、本地化且可关闭的语音反馈。
- **模型 provider 灵活性：** 多个 provider 服务相同 model ID，但名称可区分并支持快速切换（[#13942](https://github.com/QwenLM/qwen-code/issues/13942)）；记录本地/OpenAI 兼容 provider，例如 Rapid-MLX（[#13934](https://github.com/QwenLM/qwen-code/issues/13934)）；将 macOS Foundation Models 作为 fast model（[#13807](https://github.com/QwenLM/qwen-code/issues/13807)）。
- **配置健壮性：** 优雅处理 UTF-8 BOM 以及其他编码怪癖，覆盖 TOML、JSON、LSP 和导入的 Claude MCP 配置（[#13944](https://github.com/QwenLM/qwen-code/issues/13944)、[#13943](https://github.com/QwenLM/qwen-code/issues/13943)、[#13710](https://github.com/QwenLM/qwen-code/issues/13710)）。
- **CI/CD 和发布工具链可靠性：** CVE 审计稳定性、迁移排序强制、发布说明去重，以及更准确的 review-automation 解析（[#13078](https://github.com/QwenLM/qwen-code/issues/13078)、[#13922](https://github.com/QwenLM/qwen-code/issues/13922)、[#13925](https://github.com/QwenLM/qwen-code/issues/13925)、[#13735](https://github.com/QwenLM/qwen-code/issues/13735)）。

---

## 7. 开发者痛点

- **畸形配置的静默失败。** 带 BOM 前缀的 `.toml`、`.lsp.json` 和导入的 Claude 配置会被丢弃，且仅有 debug 级日志，造成令人困惑的症状（缺少斜杠命令、“No LSP servers configured”、无诊断信息）。同一时间窗口内出现多个独立报告者，表明有必要进行一次编码加固清理。
- **本不该通过却通过的 CI 门禁。** Flyway 检查允许无序迁移，依赖 CVE 审计任务长期失败，集成流水线遇到 Git 级错误（`pack has 1 unresolved delta`）——侵蚀了对绿色构建的信心。
- **发布说明准确性。** Desktop 发布之间重复的 PR 条目让用户难以知道实际发布了什么；v0.25.1 的说明同样内容不足。
- **多 agent 流程中的生命周期正确性。** 父级关闭时等待而不是停止子 turn，以及恢复受阻的 session 通过 tenant-wide locks 卡住无关 session，都是高影响的可靠性问题。
- **国际化边界情况。** CJK 队友名称在 `sanitizeName` 中坍缩为空，迫使模型发明拼音回退——这对采用 Agent Teams 的非英语用户来说是正确性问题。
- **Token/上下文核算缺口。** 压缩大小基于推断的窗口，而不是服务器报告的上限（[#13432](https://github.com/QwenLM/qwen-code/issues/13432)，现已关闭），反映出围绕上下文限制行为的反复敏感。
- **多 provider 模型切换摩擦。** 用户必须在 CLI 中更改 provider 才能访问不同厂商提供的相同模型；缺少按 provider 的别名是一个反复出现的可用性抱怨。

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*