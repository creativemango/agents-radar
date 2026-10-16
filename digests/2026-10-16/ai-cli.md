# AI CLI 工具社区动态日报 2026-10-16

> 生成时间: 2026-10-16 00:40 UTC | 覆盖工具: 7 个

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

# 跨工具对比报告 — AI CLI 生态
**快照日期：2026-10-16 · 来源：Claude Code、OpenAI Codex、Gemini CLI、GitHub Copilot CLI、OpenCode、Pi、Qwen Code 的社区摘要**

---

## 1. 生态概览

AI CLI 格局正围绕一个共同核心收敛——agent 循环、MCP 工具链、沙箱化 shell 执行和会话持久化——而差异化已从“能不能写代码”转向**运营信任**：agent 是否会诚实终止、沙箱是否允许真实工作流、升级是否保留状态。今日快照显示，七款工具都在发布可靠性加固，而非头条功能；社区最响亮的信号是沙箱/权限摩擦、上下文与 token 经济学，以及平台一致性（尤其是 Windows）。发布节奏依然很高（Claude Code、Copilot CLI、OpenCode、Pi、Qwen Code 均有发布；Codex 发布了一个 minor 加 alpha），但维护负载可见：静默更新回归、V2→V1 降级损坏、OAuth 锁死和 CI 抖动。二阶主题是**渠道成熟度**——有些项目通过大量 issue 流量吸收反馈，另一些则通过讨论区或扩展 API——这使得原始数量并不能很好代表健康度。

---

## 2. 活跃度对比

> **方法说明：** 各摘要采用的聚合口径不一致。若某摘要给出精确的全仓库计数（例如“39 issues updated”、“50 updated issues, 16 updated PRs”），则按该数字报告。若只提供精选列表，表格会明确说明。**“N/A”表示该摘要在该渠道没有数据，或该渠道不存在——并不意味着该工具不活跃。**

| 工具 | Issues（过去 24h） | PRs（过去 24h） | 讨论 | 发布状态 |
|---|---|---|---|---|
| **Claude Code** | 未聚合；10 个热门 + ~9 个次级讨论串 | **1** 个已更新（受吞吐量限制） | N/A（无讨论数据） | **v2.1.301** 补丁 — retry-watchdog 上限、OTel 指南 |
| **OpenAI Codex** | 未聚合；10 个热门 issue | ~16（列出 10 个 + 6 个相关预设 PR） | **10**（3 个 Ideas、1 个 Q&A、6 个 Show & tell） | **rust-v0.163.0** + alpha.10.2 — 可搜索的 `/config`、实时转向 |
| **Gemini CLI** | 未聚合；10 个热门 issue | **10** 个已更新 | N/A（无讨论数据） | **无** |
| **GitHub Copilot CLI** | **39** 个已更新（显示前 30 个；总结 10 个） | **0** 个已更新 | N/A（无讨论数据） | **v1.0.99-1** 和 **v1.0.99-0** — `--trust-folder`、经验证的插件安装、`/share`、会话沙箱化 |
| **OpenCode** | 未聚合；10 个热门 + ~7 个跟踪项 | ~15（列出 10 个 + ~5 个已合并） | N/A（无讨论数据） | **v1.19.1** — 仅 bugfix；**会话共享已停止** |
| **Pi** | **50** 个已更新 | **16** 个已更新 | **1**（Show & tell） | **v1.3.0** — 可配置的 `/share` 目标 |
| **Qwen Code** | 未聚合；10 个热门 + ~5 个次级 | ~16（列出 10 个 + ~6 个也在推进） | N/A（无讨论数据） | **6 个发布**：v0.25.2、desktop-v0.25.2、sdk-typescript-v0.1.20、preview.1、2 个 nightly |

**解读：** Pi 和 Copilot CLI 是仅有的报告精确聚合计数的仓库（分别为 50/16 和 39/0）。Claude Code 的单 PR 日和 Copilot CLI 的零 PR 日，凸显的是审查/合并瓶颈，而不是社区安静——两者的 issue 参与度都很强。

---

## 3. 共同功能方向

在**三个或更多**工具社区中出现的需求：

**沙箱、权限与安全 shell 执行**——最普遍的一组。
- *Claude Code：* `apply-seccomp` 在 Linux `/proc/self/setgroups` 上失败（#43454，最高赞的未解决 bug，已存在 6 个月）；自动模式分类器在用户明确批准后仍拒绝，且没有远程覆盖（#100370、#100374）。
- *Codex：* Windows 沙箱在 `node_repl.exe` 上的 ACL 失败（#51590）、Linux 受保护路径清理（#54445）、按项目网络访问（#53199）、权限预设（#54383/#54385）。
- *Gemini CLI：* 面向 bash 原生工作流的零依赖 OS 沙箱（#19873）；针对破坏性 git/临时脚本行为的护栏。
- *Copilot CLI：* macOS 沙箱阻止本地 Gradle 守护进程（#5105）、Linux bubblewrap 预检（#5052）、沙箱专用 git 凭据（#5102）。
- *OpenCode：* 对无法解析的命令跳过权限检查——裸重定向、PowerShell `--`（#49954、#55883；安全加固）。

**上下文、token 与存储经济学**——如今已是一等产品关切。
- *Claude Code：* 未使用的连接器/MCP 服务器在每个会话中都被计费（#102333，说一句“hi”就消耗 56.8k tokens）。
- *Codex：* `~/.codex/sessions` 达到约 165 GiB，其中约 95% 是 base64 截图（#35458）；prompt 缓存未命中（#54091）。
- *Gemini CLI：* AST 感知的读取/搜索/映射 EPIC，以减少轮次和 token 噪声（#22745–#22747）。
- *Pi：* CLI 模式下自动压缩未触发（#10330）、图像密集型会话拖住任务（#10162）、Bedrock Opus 摘要因策略违规被阻止（#10045）。
- *Qwen Code：* 大型工具输出直接让该轮硬报错，而不是优雅降级（#14331）。

**诚实的 agent 终止与验证信号。**
- *Gemini CLI：* 子 agent 在达到 `MAX_TURNS` 后仍报告 `GOAL` 成功（#22323，p1）——直接削弱 eval 信号。
- *Claude Code：* #102599（状态文档声称 CI 门禁通过，同时又有矛盾计划）；未经核验的声明主题。
- *Qwen Code：* `child_run` 卡在 `dispatch_started`，而对应进程从未存在（#13801）；误导性的超时文本在重试时重复消息（#14063）。
- *Pi：* 基于声明的模型元数据进行上下文恢复，而响应为空（PR #10964）。

**认证与凭据稳健性。**
- *Claude Code：* `session_stale_relogin` 锁存并强制登出活动会话（#93014）；macOS Keychain 2000 ms 超时被缓存为已登出（#101877）。
- *Gemini CLI：* 原子化、串行化的凭据存储写入（#29765）；`/auth signin` 不再覆盖磁盘上无关的编辑（#29762）。
- *Pi：* ChatGPT OAuth ID token 从未持久化（#10300）；Anthropic OAuth 对 Opus 5/5.5 报 `Invalid effort level`（#10063）。
- *Copilot CLI：* Entra 缓存账户选择；MCP OAuth 发现覆盖了已配置的 Bearer 头（#3100）。
- *Codex：* 为远程 exec-server 轮换 stdin 凭据（#54434）、Zsh 凭据遮蔽（#54432）。

**MCP 与工具规模正确性。**
- *Gemini CLI：* 为 `tools/list` 发现设置有时限的超时，默认 60 秒（#29767）；大型工具集报错（#24246）。
- *Copilot CLI：* 重复的 `content` + `structuredContent` 被注入上下文（#4515）；Cloudflare OAuth 成功后又报告订阅限制（#4991）。
- *OpenCode：* 全局 stdio MCP 服务器每加载一个目录就启动一次 → OOM（#51003）。
- *Pi：* 为 NVIDIA NIM 内联 `$ref` schema（PR #10521）；JSON 编码的嵌套工具参数（PR #9569）。
- *Claude Code：* 让延迟的 MCP schema 真正惰性化；插件主线程升级（#101325）。

**TUI/输入人体工学与终端正确性。**
- Copilot CLI（vi/vim 模式以 76 👍 关闭，取消排队消息）、Codex（可搜索的 `/config`、键位预设、标题/状态栏调节）、OpenCode（分页记录、⌘V 粘贴修复）、Pi（mintty/ConPTY 颜色查询泄漏、NumLock 绑定、CJK 粗体）、Qwen Code（Linux 上的 Ctrl+V 剪贴板文本）、Claude Code（触控板滚动历史、缺失的 redo 绑定）。

**更新/升级与数据完整性卫生。**
- *Claude Code：* 静默自动更新杀死 Remote Control 会话（#95364）；macOS TCC 权限随每次版本升级被重置（#74068）。
- *OpenCode：* V2→V1 降级留下混合 DB，导致每个会话都 500（#55555）；`opencode upgrade` 留下 229 字节的 `.exe` 残桩（#51553、#55888）。
- *Gemini CLI：* 当活动二进制被 PATH 遮蔽时，自动更新仍报告成功（#29757）；原子写入覆盖只读文件并拆分硬链接（#29749）。
- *Pi：* 交互模式静默忽略设置保存失败（#10168）。

---

## 4. 差异化分析

| 维度 | Claude Code | OpenAI Codex | Gemini CLI | Copilot CLI | OpenCode | Pi | Qwen Code |
|---|---|---|---|---|---|---|---|
| **主要焦点** | 可观测性优先的 agent 部署；无人值守 CI/自动化 | Windows Computer Use + 云/远程守护进程 | Agent 编排正确性；AST 工具 | GitHub 原生工作流 + MCP/插件生态 | V2 核心重写；桌面可扩展性 | 扩展 API + 多提供商正确性 | 托管式 `serve` 控制平面；多 agent |
| **目标用户** | 企业团队、自动化/CI 运维 | 企业桌面 + Windows 高级用户 | Agent 构建者、eval/benchmark 作者 | 以 GitHub 为中心的团队、企业 IdP 用户 | 插件/应用开发者、TUI 高级用户 | 多提供商折腾者、扩展作者 | 平台构建者、托管服务运营者 |
| **技术路线** | 捆绑 skills/plugins、OTel 跟踪、watchdog 配置标志 | Rust 核心、轮换远程凭据、`taskpolicy` 守护进程优先级、SQLite | JS/TS；按故障分级的 agent 注册表；原生 bash 亲和性 | 沙箱范围内的会话、经验证的 `ghapp://air/install` 插件渠道 | 核心/运行时重写；GenUI 渲染协议；GUI 插件导出 | 提供商无关目录，带元数据门控；O(n²) 流解析修复 | 双路径 TS 循环、Kubernetes 常驻工作区、仅日志崩溃恢复、多语言 SDK |
| **显著区别** | 唯一明确把 OTel 跟踪指南推入捆绑 skill 的工具 | 最深的 Computer Use/浏览器控制面，也有最深的 Windows 痛点 | 在*真实*终止语义上信号最强 | 通过持续 👍 进行社区引导（vi 模式、本地化） | 本周期唯一**移除**协作功能（会话共享）的工具 | 相对于规模，issue+PR 吞吐最高 | 唯一与 CLI 同步发布 Java/Python/TS SDK 的工具 |

**关键分歧：**
- **安全姿态分成两条路。** Claude Code 和 Codex 投资于*预防性*执行（权限分类器、沙箱监视），如今正因过度阻止和不透明而承受反弹。Gemini CLI 和 OpenCode 则投资于*评估正确性*（真实终止信号、即使解析失败也运行权限检查）。
- **可扩展性模型分化。** Claude Code（skills/plugins）、Copilot CLI（经验证的安装链接）、OpenCode（GenUI + GUI 插件导出）和 Pi（宿主扩展生命周期、`queue_update` 可观测性）都把扩展面当作产品，但 OpenCode 是唯一让 agent 把 UI *渲染进*宿主应用的工具。
- **托管模型是真正的岔路。** Qwen Code 是唯一被服务端关切主导的摘要（Workspace 绑定、TTL 子会话清理、Java SDK 审查关键项）；Codex 以远程 exec-server 凭据占据中间地带；Claude Code 的远程叙事目前是痛点（Remote Control 会话被更新丢弃、没有拒绝覆盖 UI）。

---

## 5. 社区动能与成熟度

**原始参与度最高（按讨论串深度）：**
- **OpenAI Codex** — 顶级讨论串分别有 94、82、61、49 条评论，另有覆盖 Ideas/Q&A/Show-and-tell 的 10 个健康讨论板。参与面广，但高度集中在 Windows Computer Use 故障簇。
- **Qwen Code** — 一个 68 条评论的架构讨论串（#12380）实际上起到路线图 RFC 的作用；24 小时内六个发布显示出激进节奏。
- **Claude Code** — 单 issue 热度最高（51 条评论，37 👍），也是数据集中最强的*反应*信号（#43454 有 49 👍），但 **24 小时内 1 个 PR** 表明约束在合并/审查能力，而非贡献量。

**迭代最快：**
- **Pi** — 一天内更新 50 个 issue + 16 个 PR，open→closed PR 流转很快（列出的 10 个 PR 中已有 7 个关闭）并发布 v1.3.0。规模小、速度快。
- **OpenCode** — 约 15 个 PR，多个社区提交的修复在窗口内关闭了可复现 bug（#55894、#55893、#55900、#55871）。循环很快，但 V2 迁移正产生稳定的功能对等 bug 长尾。
- **Codex** — 稳定 + alpha 双通道并行，带有基础设施级 PR（凭据轮换、SQLite journal 模式、macOS 守护进程优先级）。

**最成熟的运营姿态：**
- **Gemini CLI** — 标签纪律良好（`p1/need-retesting`、`p2`、`p3`），AST 工作有清晰的 EPIC 结构，PR 会预先处理已知故障模式（16 KB 大小的发现超时、原子凭据写入）。值得注意的是今天零发布，而跟踪器看起来仍然健康。
- **Copilot CLI** — 关闭长期社区请求（vi 模式 76 👍）并改善供应链姿态（经验证的 Skills/插件安装）——这是项目走过初始反馈分诊阶段的迹象。

**值得关注的脆弱性指标：**
- **Claude Code：** 2.37337.1 的环境选择器重构引发了多线 Desktop 回归潮（侧边栏分组、Routines 被隐藏、静默 worktree 会话、thinking 显示）——用户明确要求开关和回滚。
- **OpenCode：** 双向 V2 迁移丢失（向前丢失 LSP/formatter，向后混合 DB 500）是这组中最高严重度的问题。
- **Qwen Code：** 持续不断的 `qwen-code-dev-bot` 主分支失败流，加上把超时从 30 秒扩到 120 秒的 deflake PR，表明存在与负载相关的测试脆弱性。
- **Claude Code / Copilot CLI / Qwen Code：** 在本数据集中实际上没有可用的讨论渠道——反馈完全经由 issue 跟踪器汇集，这偏向 bug 报告而非设计讨论。

---

## 6. 趋势信号

**1. “agent 真的成功了吗？”正成为一等 API 关切。**
Gemini CLI 的 `MAX_TURNS`→`GOAL` 误报（#22323）、Qwen Code 的幽灵 `dispatch_started` 运行（#13801）和 Claude Code 未经核验的 CI 通过声明（#102599），是同一失败在三种架构中的体现。预计会有人要求机器可检查的终止契约和第三方可验证的运行日志。对于面向自主/CI 用途的工具构建者，这是杠杆最高的缺口。

**2. 上下文经济学已从优化项变成产品本身。**
现在每款工具都有具体的成本故事：说“hi”消耗 56.8k tokens（Claude Code）、165 GiB 的会话截图（Codex）、把 AST 感知读取作为 token 削减策略（Gemini CLI）、不触发的压缩（Pi）、大型输出硬失败（Qwen Code）。对开发者而言：把上下文预算管理当作架构要求，而不是调优步骤——延迟工具 schema、用 artifact 引用代替内联负载、优雅降级正在成为入场门槛。

**3. 沙箱 UX 是每个平台上最突出的未解决摩擦。**
七款工具中有六款报告了活跃的沙箱阻塞，涵盖 Linux seccomp、macOS Gradle localhost、Windows ACL 和 shell-scanner 绕过。两类不同抱怨正在浮现：沙箱**阻止合法工作**（Gradle 守护进程、本地网络、气隙构建），以及权限分类器**覆盖用户明确批准**且没有逃生口（Claude Code #100370/#100374）。发布可审计、按项目、可快速覆盖策略的厂商，会比那些收紧默认值的厂商更快赢得开发者信任。

**4. Windows 一致性现在是竞争差异化因素，而非脚注。**
Codex（Computer Use、MXC 启动器、沙箱 ACL、模型目录）和 Pi（ConPTY 颜色查询、BEL 劫持编辑器）承载着最大的平台特定问题簇；OpenCode 有损坏的 npm 升级路径和 Windows ConPTY 上的 raw 模式泄漏。对于以 Windows 开发机队为标准化的团队，这目前是工具就绪度的最佳单一指标。

**5. 更新机制是信任面。**
杀死活动会话的静默更新（Claude Code #95364）、TCC 权限随每次版本升级重置（#74068）、PATH 遮蔽下自动更新假成功（Gemini CLI #29757），以及升级后 229 字节的损坏二进制（OpenCode），对信任的损害都超过它们发布的功能。发布工程、原子迁移和降级安全正在成为正当的评估标准。

**6. MCP 标准化了协议，但没有标准化语义。**
重复的 `content`/`structuredContent`、OAuth 与 Bearer 回退歧义、按目录的进程爆炸、10 分钟发现停滞，以及 `$ref` schema 拒绝，全都是*实现*缺口。生态需要为 MCP 客户端建立事实上的一致性配置——结果格式偏好、发现超时、认证优先级和 schema 规范化。

**7. 远程与多 agent 工作负载需要生命周期治理。**
Claude Code 丢弃 Remote Control 会话且缺少远程批准 UI，Codex 的远程凭据轮换和将事件投递进已打开聊天的请求，Qwen Code 的僵尸进程存活 48 天且子会话无限累积，以及 OpenCode 的 `pair --remote` 503 且无重置路径，全都指向同一方向：TTL、资源核算和可恢复的会话所有权是下一层基础设施。

**8. 可观测性正从第三方附加项升级为随附指南。**
Claude Code 把 OpenTelemetry 跟踪指南加入其捆绑的 `claude-api` skill，Gemini CLI 为 bug 报告收集子 agent 轨迹，Codex 用 SHA-256 元数据记录本地图像输入，这些都表明 agent 运行正被视为可审计的生产工作负载。基于这些工具构建的开发者应假定 trace 导出、运行来源和 artifact 谱系会成为预期——并应现在就为此埋点。

---

**对决策者的底线：** 如果你的优先事项是自主 CI/可验证结果，就重视 Gemini CLI 的终止语义工作和 Claude Code 的可观测性推进，同时打折看待 Claude Code 的审查速度和 Desktop 动荡风险。对于 Windows 密集型机队，目前没有一款工具是干净的——Codex 能力最深，积压也最深。对于插件/平台构建者，OpenCode 和 Pi 提供最活跃的扩展面；对于多语言 SDK 集成，Qwen Code 独特地同步发布 Java/Python/TS。七款工具整体而言，应假定沙箱策略、上下文预算和升级安全，比模型质量更能决定真实世界的采用。

---

## 各工具详细报告

<details>
<summary><strong>Claude Code</strong> — <a href="https://github.com/anthropics/claude-code">anthropics/claude-code</a></summary>

## Claude Code Skills 社区热点

> 数据来源: [anthropics/skills](https://github.com/anthropics/skills)

# Claude Code Skills — 社区亮点报告
**来源：** github.com/anthropics/skills · **数据截至：** 2026-10-16

> ⚠️ **数据说明：** PR 数据集报告所有 50 个 pull request 的 `Comments: undefined`。因此下方 PR 排名依据的是*活跃度新旧、关联 issue 讨论量以及影响广度*的综合指标，而非原始评论数。Issue 排名使用实际评论数。

---

## 1. 热门 Skills 排名

| # | Skill / PR | 功能 | 讨论亮点 | 状态 |
|---|---|---|---|---|
| 1 | **skill-creator — eval viewer 加固** ([PR #1961](https://github.com/anthropics/skills/pull/1961)) | 加固本地 eval 查看器（`generate_review.py` + `viewer.html`），防御脚本逃逸、DNS 重绑定、跨站 POST 和转义漏洞 | 直接回应 [Issue #1394](https://github.com/anthropics/skills/issues/1394)（属性不安全的 `escapeHtml`、展示路径 XSS）。在本地渲染不受信任的模型输出——影响面很大 | OPEN（更新于 10-11） |
| 2 | **skill-creator — 触发 eval 隔离 + Windows/运行时故障** ([PR #1298](https://github.com/anthropics/skills/pull/1298)) | 隔离每个 worker 的命令探测，修复 Windows 管道上的 `select()`，阻止无关工具中止扫描；运行时故障不再被判定为“未触发” | 合并了 skill-creator 最大的一簇 bug：[#1352](https://github.com/anthropics/skills/issues/1352)（并行 worker 交叉匹配 UUID → 假阴性触发率）、[#1383](https://github.com/anthropics/skills/issues/1383)、[#556](https://github.com/anthropics/skills/issues/556) | OPEN（更新于 09-16） |
| 3 | **mcp-builder — mcp>=2 `streamable_http_client` + 自定义请求头** ([PR #1742](https://github.com/anthropics/skills/pull/1742)) | 将连接脚本迁移到更名后的 `streamable_http_client` API；通过 `create_mcp_http_client` / `http_client` 传递自定义请求头 | 修复 [#1668](https://github.com/anthropics/skills/issues/1668)。与 [Issue #1390](https://github.com/anthropics/skills/issues/1390) 并列（evaluation.py 对每个真实 MCP 服务器评分均为 0/N）——mcp-builder 是工具链中最薄弱的一环 | OPEN（更新于 10-08） |
| 4 | **skill-creator — YAML frontmatter 标量解析** ([PR #1997](https://github.com/anthropics/skills/pull/1997)) | 将 `parse_skill_md` 中手写的去引号/续行拼接替换为正确的 YAML 标量解析 | `SKILL.md` 与优化命令实际看到的内容之间出现静默元数据漂移——会污染 eval 输入 | OPEN（更新于 10-13） |
| 5 | **webapp-testing — 移除 `shell=True`** ([PR #1980](https://github.com/anthropics/skills/pull/1980)) | 重写 `with_server.py`，在仍支持 `cd <dir> && <command>` 链式命令的同时避免 shell 命令注入（CWE-78） | 针对 agent 驱动执行路径的安全修复；属于不断扩大的安全焦点 PR 流的一部分 | OPEN（更新于 10-06） |
| 6 | **slack-gif-creator — 感知文本边界的居中** ([PR #2004](https://github.com/anthropics/skills/pull/2004)) | 当 `centered=True` 时，使用 Pillow 墨迹边界框偏移，而非简单地将宽/高减半 | 在 Pillow 12.3.0 上测得可复现的错位（请求 caption 位于 y=64，渲染出的墨迹中心却在 y=68） | OPEN（更新于 10-14） |
| 7 | **document-typography** ([PR #514](https://github.com/anthropics/skills/pull/514)) | 新增排版 QC skill：AI 生成文档中的孤词换行、寡行段落、编号错位 | 本批中“影响每一份生成文档”范围最广的声明；长期存在（创建于 03-04） | OPEN |
| 8 | **notion-spec-to-implementation + quantitative-resume-auditor** ([PR #1245](https://github.com/anthropics/skills/pull/1245)) | 双 skill 捆绑包：将产品/技术规格转化为带验收标准的可执行 Notion 任务；另附简历量化审计器 | 代表“工作流自动化”提交潮；4 个多月后仍未合并 | OPEN（更新于 10-15） |

**另见值得关注：** [PR #1977](https://github.com/anthropics/skills/pull/1977)（algorithmic-art `wrapAround()` 双端 bug，修复 #1897）、[PR #1980](https://github.com/anthropics/skills/pull/1980)、[PR #1792](https://github.com/anthropics/skills/pull/1792)（docx LibreOffice 超时被误报为成功）、[PR #1730](https://github.com/anthropics/skills/pull/1730)（失效文档 URL）、[PR #486](https://github.com/anthropics/skills/pull/486)（ODT skill）、[PR #83](https://github.com/anthropics/skills/pull/83)（skill-quality-analyzer + skill-security-analyzer）。

---

## 2. 社区需求趋势

从排名前 15 的 Issues 中提炼：

1. **信任与命名空间完整性（最强烈的信号）。** [Issue #492](https://github.com/anthropics/skills/issues/492) — *46 条评论，是整个数据集中讨论量遥遥领先的一项* — 警告社区 skills 以 `anthropic/` 命名空间发布，会冒充官方 skills 并助长信任边界滥用。这是社区的头号关切。
2. **真正可用的评估基础设施。** 一组密集的 bug 报告针对的是测量层，而非任何单一 skill：[Issue #556](https://github.com/anthropics/skills/issues/556)（所有查询的触发率均为 0%）、[#1352](https://github.com/anthropics/skills/issues/1352)（并行 worker 产生假阴性）、[#1383](https://github.com/anthropics/skills/issues/1383)（六个可复现的 skill-creator 故障）、[#1390](https://github.com/anthropics/skills/issues/1390)（mcp-builder eval 评分为 0/N）。
3. **分发与组织级共享。** [Issue #228](https://github.com/anthropics/skills/issues/228)（16 条评论，8 个 👍）要求产品内提供共享 skill 库，而不是手动传递 `.skill` 文件；[Issue #189](https://github.com/anthropics/skills/issues/189)（9 个 👍）报告 `document-skills` 和 `example-skills` 安装相同内容并污染上下文窗口。
4. **上下文/token 效率。** [Issue #1487](https://github.com/anthropics/skills/issues/1487) 报告 `claude-api` 在单次工具调用中急切注入约 156k tokens，耗尽窗口。
5. **记忆与状态压缩。** [Issue #1329](https://github.com/anthropics/skills/issues/1329) 提出 `compact-memory` —— 用符号表示法压缩 agent 状态 —— 这是一个真正的新能力方向，而非 bug 修复。
6. **治理/安全模式。** [Issue #412](https://github.com/anthropics/skills/issues/412) 提出 `agent-governance`（策略执行、威胁检测、信任评分、审计轨迹）；[Issue #1385](https://github.com/anthropics/skills/issues/1385) 提出三闸门推理质量流水线。
7. **文档脚本正确性。** [Issue #2018](https://github.com/anthropics/skills/issues/2018) — `merge_runs.py` 丢弃 run 局部命名空间声明并写出无效 XML，却报告成功；直接 `.docx` 模式会覆盖输入归档。

**净解读：** 需求明显偏向**加固与元工具**（skill-creator、mcp-builder、打包、评估、安全），远多于全新的领域 skills。新 skill 提案（Notion、ODT、排版、HPC、治理、记忆）确实存在，但引发的讨论远少于工具/安全问题。

---

## 3. 高潜力待合并 Skills

活跃、未合并且维护者/作者有明显迭代证据的 PR——最有可能落地：

| PR | Skill | 为什么可能很快落地 |
|---|---|---|
| [#1742](https://github.com/anthropics/skills/pull/1742) | mcp-builder API 迁移 | 修复一个已跟踪 issue（#1668）；作者从 09-08 活跃到 10-08 |
| [#1298](https://github.com/anthropics/skills/pull/1298) | skill-creator 触发评估 | 一次解决三个分别提交的 issue；迭代更新至 09-16 |
| [#1961](https://github.com/anthropics/skills/pull/1961) | skill-creator viewer 加固 | 安全范围，关联 #1394；10-11 刷新 |
| [#1997](https://github.com/anthropics/skills/pull/1997) | skill-creator YAML 解析 | eval 流水线中的正确性修复；10-13 刷新 |
| [#2004](https://github.com/anthropics/skills/pull/2004) | slack-gif-creator 居中 | 小而明确、可复现；10-14 刷新 |
| [#1977](https://github.com/anthropics/skills/pull/1977) | algorithmic-art `wrapAround()` | 关闭 #1897；干净的单函数修复 |
| [#1980](https://github.com/anthropics/skills/pull/1980) | webapp-testing `shell=True` | 移除 CWE-78，并有文档化的迁移路径 |

*停滞但仍开放：* [#514](https://github.com/anthropics/skills/pull/514)（document-typography，最后触及 03-13），[#1245](https://github.com/anthropics/skills/pull/1245)（10-15 有触及，但自 06-02 起开放），[#83](https://github.com/anthropics/skills/pull/83)（最后触及 01-07）。

---

## 4. Skills 生态洞察

**社区最集中的需求并不是新的 Skills，而是一条值得信赖的 Skill 供应链——命名空间/安全完整性、正确的评估与触发测量，以及可靠的打包——因为元工具（skill-creator、mcp-builder、分发）几乎承载了所有讨论、bug 和安全发现。**

---

# Claude Code 社区文摘 — 2026-10-16

## 1. 今日亮点
发布了一个补丁版本（v2.1.301）：为无人值守重试看门狗增加了可配置的使用量上限等待上限，并在 `claude-api` 技能中加入了 OpenTelemetry 追踪指南——这显然是在推动以可观测性为先的 agent 部署。社区关注点仍主要由**由 2.37337.1 环境选择器更新引入的桌面应用回归问题**主导（侧边栏分组和 Routines 区域被移除），此外，越来越多用户抱怨**自动模式权限分类器过度拦截合法的、用户已批准的工作**。在 CLI 一侧，沙箱、认证和上下文成本问题仍在持续累积。

## 2. 版本发布
**v2.1.301**（过去 24 小时）：
- 新增 `CLAUDE_CODE_RETRY_WATCHDOG_MAX_USAGE_LIMIT_WAIT_MS` —— 允许无人值守重试模式（`CLAUDE_CODE_RETRY_WATCHDOG`）在使用量限制重置时间过于遥远时中止请求，而不是无限期阻塞。直接面向长时间运行的 CI/自动化用例。
- 在内置的 `claude-api` 技能中新增 OpenTelemetry 追踪指南（源数据中的发布说明被截断）。

此时间窗口内没有其他版本发布。

## 3. 热门 Issue

1. **[#102088](https://github.com/anthropics/claude-code/issues/102088)** — *Desktop 2.37337.1："Group by" 已从 Code 标签页侧边栏移除*（51 条评论，👍37）。当日互动量最高的条目：现在会话只能按环境分组。这是同一构建版本中一系列桌面 UX 回归的旗舰问题——社区热度高、影响面广。

2. **[#43454](https://github.com/anthropics/claude-code/issues/43454)** — *`apply-seccomp` 在 Linux 上失败：无法写入 `/proc/self/setgroups`*（29 条评论，👍49）。一个存在六个月的沙箱 bug，也是数据集中 👍 信号最强的问题。在受影响的内核上，它完全阻碍了 Linux 沙箱功能，且仍未修复，是最明显的长尾可靠性债务。

3. **[#95364](https://github.com/anthropics/claude-code/issues/95364)** — *桌面端自动更新静默退出并重启，导致所有 Remote Control 会话丢失*（11 条评论）。仍处于开放状态的报告：空闲窗口的“隐形更新”会在多台机器上杀死远程会话。这削弱了 Claude Code 远程/无人值守使用的核心承诺。

4. **[#102158](https://github.com/anthropics/claude-code/issues/102158)** — *Desktop 2.37337.1（macOS）：Routines 侧边栏区域被隐藏且没有开关*（9 条评论）。#102088 的伴随回归——计划任务结果从侧边栏消失。这表明环境选择器重构没有经过完整的回归测试。

5. **[#93014](https://github.com/anthropics/claude-code/issues/93014)** — *`session_stale_relogin` 锁存并强制登出活跃会话*（6 条评论，👍6）。OAuth 刷新以 `invalid_grant` 失败，而该锁存会持续强制重新登录。认证不稳定，会在会话中途打断工作。

6. **[#100370](https://github.com/anthropics/claude-code/issues/100370)** — *自动模式分类器阻止将 PR 合并到用户自己的仓库*（3 条评论）。一个固定工作流（commit → PR → merge → deploy）因“Merge Without Review”理由被阻止。这指向分类器启发式与用户声明的技能流水线之间的不匹配。

7. **[#100374](https://github.com/anthropics/claude-code/issues/100374)** — *自动模式在用户明确批准后仍持续拒绝，且远程用户没有批准路径*（3 条评论）。与 #100370 直接相关：通过 Remote Control 远程操作时，完全没有 UI 可供覆盖拒绝。

8. **[#102333](https://github.com/anthropics/claude-code/issues/102333)** — *未使用的连接器和 MCP 服务器仍会在每个会话中消耗 token*（2 条评论）。一个只说了“hi”的会话已经占用 56.8k token 的上下文。将上下文膨胀与成本和延迟联系起来；是近期产品工作的有力候选。

9. **[#102496](https://github.com/anthropics/claude-code/issues/102496)** — *新的 Desktop 会话会静默地在 worktree 中启动，且没有设置可禁用它*（1 条评论）。该行为在环境选择器更新后静默改变；关联到此前的因不活跃而关闭的 #53545。

10. **[#102245](https://github.com/anthropics/claude-code/issues/102245)** — *Windows：空闲的 Claude Code 会话进程会将一个线程固定在 100% CPU 上持续数小时*（1 条评论，有复现）。桌面端托管的会话在空闲时烧掉一个核心——对笔记本用户来说是电池/散热问题。

**今日其他活跃项：** 同一报告者提交了一组**网络安全误报**（#[88808](https://github.com/anthropics/claude-code/issues/88808)、#[88718](https://github.com/anthropics/claude-code/issues/88718)、#[88683](https://github.com/anthropics/claude-code/issues/88683)、#[85393](https://github.com/anthropics/claude-code/issues/85393)、#[88926](https://github.com/anthropics/claude-code/issues/88926)）——均被作为重复问题关闭，但又再次出现，每个都描述了 `session-halted` 阻止已授权工作。还值得注意：**[#102599](https://github.com/anthropics/claude-code/issues/102599)**（Claude Code 自行提交了一个流程违规）、**[#102604](https://github.com/anthropics/claude-code/issues/102604)**（某些会话中 thinking 块折叠为一行/被省略）、**[#101877](https://github.com/anthropics/claude-code/issues/101877)**（macOS Keychain 2000 ms 超时被缓存为已登出 30 秒）、**[#74068](https://github.com/anthropics/claude-code/issues/74068)**（macOS TCC 提示标题只有裸版本号，权限每次更新都会重置）、以及 **[#102537](https://github.com/anthropics/claude-code/issues/102537)**（触控板滚动无法浏览命令历史）。

## 4. 重点 PR 进展

过去 24 小时内只有 **一个** PR 有更新——异常安静，本身就值得注意：

1. **[#95698](https://github.com/anthropics/claude-code/pull/95698)** — `fix(plugins): run ralph-wiggum and output-style .sh hooks through bash with a quoted path`（开放，由 `claude[bot]` 提交）。三个内置插件将 hook 脚本注册为裸的、未加引号的路径（例如 `${CLAUDE_PLUGIN_ROOT}/hooks/stop-hook.sh`），当插件根目录包含空格，或脚本缺少 shebang/可执行位时就会出错。该修复通过 `bash` 执行并对路径加引号。引用 #95673，并解决了 #78490 中插件相关的那一半。

> 注：此时间窗口内 PR 吞吐量太低，无法编制 top-10 列表；可将此视为一个信号：当前瓶颈是评审/合并活动，而非贡献量。

## 5. 热门讨论
*省略——此数据集中未提供讨论数据。*

## 6. 功能请求趋势
从带有 enhancement 标签的开放 issue 中提炼：

- **上下文与 token 经济学**（#102333）：让延迟加载的 MCP 工具 schema 真正惰性化，这样空闲连接器就不会在每次会话启动时消耗 token。最明确的成本导向诉求。
- **Desktop Code 标签页自定义**（#102088、#102158、#102219）：恢复/扩展分组控件，在侧边栏中显示 Routines，并允许插件向 Session Rail 贡献行。用户希望 Desktop Code 标签页可配置，而不是由产品强加一套固定方案。
- **编辑器/键位绑定完整性**（#76684）：添加 redo（vim NORMAL 模式下的 `Ctrl+R`，以及可绑定的 `chat:redo`）——存在 undo 却没有 redo，这每天都让 vim 用户感到意外。
- **权限模型逃生舱**（#100370、#100374、#101463）：在用户明确批准后，提供可靠的方式来覆盖自动模式分类器，包括没有 UI 的远程/Cowork 会话。
- **插件生态管道**（#101325）：为 `cc-plugin-you-should-know` 提供主线程直连，以便插件发现结果可以有选择地升级处理，而不是复制粘贴。

## 7. 开发者痛点

- **小版本更新中交付的桌面回归。** 2.37337.1 环境选择器版本移除/改变了侧边栏分组，隐藏了 Routines 区域，静默启用了 worktree 会话，并改变了 thinking 显示。多份报告将此描述为未经测试的重构；用户要求提供开关和回滚。
- **破坏状态的更新机制。** `#95364`（隐形更新在会话中途退出应用，导致 Remote Control 丢失）和 `#74068`（macOS TCC 权限在每次版本升级时重置）让频繁发布对日常用户来说代价高昂。
- **自动模式分类器 vs. 真实工作流。** 反复出现明确批准*之后*仍被拒绝、原因不透明、没有覆盖路径的报告——对远程/无头会话尤其痛苦（#100370、#100374、#101463）。
- **认证与凭据脆弱性。** OAuth `session_stale_relogin` 锁存（#93014）以及 Keychain 读取超时被缓存为“未登录”（#101877）会在负载下产生虚假登出。
- **安全过滤器误报导致会话停止。** 反复出现的带 `cyber` 标签的一类问题（Opus 4.8、`session-halted`）持续被作为重复问题关闭，却看不到系统性修复，报告者也不断回归。
- **Linux 上的沙箱摩擦。** `apply-seccomp` 在 `/proc/self/setgroups` 上失败（#43454）仍是点赞最高的开放 bug，六个月来没有解决。
- **后台资源浪费。** Windows 上空闲 Desktop 会话固定一个 CPU 线程（#102245），加上未使用连接器造成的每会话上下文膨胀（#102333）——这些性能和成本问题会随常开使用而放大。
- **模型自律/验证。** #102599（一份状态文档声称 CI 门禁通过，而计划却给出相反说法）以及 agent 未经验证声明这一普遍主题，反映出对长时间运行的自主任务有更强流程保证的需求。

</details>

<details>
<summary><strong>OpenAI Codex</strong> — <a href="https://github.com/openai/codex">openai/codex</a></summary>

# OpenAI Codex 社区文摘 — 2026-10-16

## 今日要点
Codex 发布了 **rust-v0.163.0**，新增可搜索的 `/config` 体验、对受支持偏好设置的立即应用、需要重启的指示，以及通过后续消息引导正在进行的响应的能力。问题跟踪器仍被 **Windows Computer Use/沙箱故障** 和 **模型可用性问题** 所主导，而 PR 活动则聚焦于远程凭据安全、macOS 守护进程性能、SQLite 可靠性和 TUI 自定义。

## 发布
- **[rust-v0.163.0](https://github.com/openai/codex/releases/tag/rust-v0.163.0)** — 新的 `/config` 允许用户按类别搜索偏好设置，立即更改受支持的设置，并查看哪些更改需要重启。用户还可以在 Codex 响应时发送后续消息，以立即引导它；中断一轮现在会停止活动的 Code Mode 单元。  
- **[rust-v0.163.0-alpha.10.2](https://github.com/openai/codex/releases/tag/rust-v0.163.0-alpha.10.2)** — 已发布 Alpha 版本。

## 热门问题
1. **[#52407](https://github.com/openai/codex/issues/52407)** — Windows Dots CUA MXC 启动器在直接 shell 恢复后失败，报 `HRESULT 0x80070003`。高影响的 Windows Computer Use 阻塞问题；94 条评论，10 👍。  
2. **[#49458](https://github.com/openai/codex/issues/49458)** — Windows dot-started 本地任务缺少 Computer Use 工具，而普通本地 Codex 会话正常工作。强烈的社区信号：82 条评论，26 👍。  
3. **[#25271](https://github.com/openai/codex/issues/25271)** — 在 Windows 上，Computer Use 无法确定 Chrome URL，即使是在 `chrome://newtab/` 上也是如此。长期存在的浏览器控制故障；61 条评论，12 👍。  
4. **[#54083](https://github.com/openai/codex/issues/54083)** — Windows Chat 在所有思考级别下对 5.6 和 6 都报告“Model not found”，而 Web 端成功。模型路由/可用性回归；49 条评论。  
5. **[#51590](https://github.com/openai/codex/issues/51590)** — Windows 沙箱无法打开正在运行的 `node_repl.exe` 以更新 ACL（错误 32），阻塞 Computer Use 和 shell。沙箱初始化痛点；41 条评论，11 👍。  
6. **[#53188](https://github.com/openai/codex/issues/53188)** — Windows 浏览器工具无法使用 MXC 启动器初始化，报 `HRESULT 0x80070003`。与 #52407 属于同一启动器系列；39 条评论。  
7. **[#52747](https://github.com/openai/codex/issues/52747)** — Windows Desktop 出现未知注入的 `code_mode_tool_description_first` 以及不透明的强制清理拒绝。配置/沙箱不透明问题；22 条评论，3 👍。  
8. **[#52839](https://github.com/openai/codex/issues/52839)** — Sol Ultrafast 在 ChatGPT 认证下从第一方模型目录中缺失。影响模型发现和入门；20 条评论。  
9. **[#37754](https://github.com/openai/codex/issues/37754)** — TUI 恢复失败，因为尚不支持 `list_turns`。破坏 CLI 会话连续性；18 条评论，4 👍。  
10. **[#35458](https://github.com/openai/codex/issues/35458)** — Desktop 截图在每次压缩时都会完整重新持久化，并被子代理分支继承；`~/.codex/sessions` 达到约 165 GiB，约 95% 为 base64 图像。严重的存储/上下文膨胀；14 条评论。

## 重要 PR 进展
- **[#54445](https://github.com/openai/codex/pull/54445)** — 从 Linux 沙箱中移除过时的受保护路径监控；缺失的元数据路径已经会获得只读合成挂载。  
- **[#54434](https://github.com/openai/codex/pull/54434)** — 为远程 exec-server 添加轮换 stdin 凭据，包括初始凭据超时和轮换支持。  
- **[#54432](https://github.com/openai/codex/pull/54432)** — 支持通过 Zsh shell 快照进行远程凭据掩蔽，将掩蔽扩展到 Bash 之外。  
- **[#54421](https://github.com/openai/codex/pull/54421)** — 将对已验证默认守护进程的显式连接视为本地，防止 TUI 连接被误分类为远程。  
- **[#54420](https://github.com/openai/codex/pull/54420)** — 在已完成的图像生成项上记录本地图像输入，并附带路径和 SHA-256 元数据。  
- **[#54414](https://github.com/openai/codex/pull/54414)** — 允许在有活动后代的情况下挂起根轮次，改进会话恢复和关闭行为。  
- **[#54401](https://github.com/openai/codex/pull/54401)** — 在 macOS 上通过 `taskpolicy -b` 以后台优先级运行受管守护进程，减少对前台的影响。  
- **[#54398](https://github.com/openai/codex/pull/54398)** — 将生成的图像项存储为带 `fileId` 的附件引用，仅在需要时回退到内联结果。  
- **[#54389](https://github.com/openai/codex/pull/54389)** — 启动时保留现有 SQLite 日志模式，以避免另一个连接持有写锁时发生故障。  
- **[#54388](https://github.com/openai/codex/pull/54388)** — 为终端标题和状态行布局添加快捷预设，保留项顺序和别名。  
- 相关的 TUI 预设工作也已在 **[#54387](https://github.com/openai/codex/pull/54387)**、**[#54385](https://github.com/openai/codex/pull/54385)**、**[#54383](https://github.com/openai/codex/pull/54383)**、**[#54381](https://github.com/openai/codex/pull/54381)**、**[#54380](https://github.com/openai/codex/pull/54380)** 和 **[#54379](https://github.com/openai/codex/pull/54379)** 中落地。

## 热门讨论
### 想法
- **[#54390](https://github.com/openai/codex/discussions/54390)** — “最新的 CLI 版本感觉像是一种倒退。”用户认为会话/线程增加了 CLI 臃肿，GUI 功能应保留在 GUI 中；1 条评论，1 👍。  
- **[#50754](https://github.com/openai/codex/discussions/50754)** — 功能请求：将事件投递到现有的本地 Codex Desktop 聊天中，而无需轮询或启动第二个 app-server 会话。  
- **[#54124](https://github.com/openai/codex/discussions/54124)** — 在模型选择器中自定义按键绑定，以更快切换模型。  

### 问答
- **[#53199](https://github.com/openai/codex/discussions/53199)** — 已回答：当选择器中缺少自定义权限配置文件时，如何仅为某一个本地 Codex Desktop 项目启用网络访问。  

### 展示与分享
- **[#54333](https://github.com/openai/codex/discussions/54333)** — Agent Standup：从本地 rollout 文件以只读方式将 Codex 会话回放为实时“团队通话”。  
- **[#54166](https://github.com/openai/codex/discussions/54166)** — 使用 Codex 构建：CSV Check Studio / CSV Cleanup Kit，一个本地 CSV 预检检查器，可保留冲突的联系人 ID。  
- **[#53955](https://github.com/openai/codex/discussions/53955)** — Unmix：移除已记录的开发决策，并通过 Codex MCP 调整保留的更改。  
- **[#53200](https://github.com/openai/codex/discussions/53200)** — Reset Alerts：在美国/加拿大通过 SMS，在其他地区通过 Telegram 发送 Codex 重置公告。  
- **[#53492](https://github.com/openai/codex/discussions/53492)** — Secure Browser Use for Zen：用于 Codex 浏览器控制的 Firefox/Zen 桥接。  
- **[#51825](https://github.com/openai/codex/discussions/51825)** — Project Architect：面向跨聊天和检查点的长期运行 AI 编码项目的开源 Codex 技能。

## 功能请求趋势
- **TUI/CLI 自定义与克制：** 输入撤销/重做（#2379）、自定义模型选择器按键绑定（#54124）、提示/权限/标题/状态栏预设（#54379–#54388），以及有关 CLI 正在变得像 GUI 一样臃肿的抱怨（#54390）。  
- **会话连续性与集成：** 可靠的 TUI 恢复（#37754、#54073）、将事件投递到已打开的 Desktop 聊天（#50754）、远程 dot 会话读取（#50157），以及更好的轮次挂起/恢复（#54414）。  
- **权限、安全与网络控制：** 按项目网络访问（#53199）、内联权限配置文件选择（#54383）、将当前权限设置保存为预设（#54385），以及远程凭据掩蔽/轮换（#54432、#54434）。  
- **存储与资源治理：** 截图持久化控制（#35458）、隐藏的记忆生成配额使用（#51177）、macOS 内存/交换耗尽（#44996）、子代理内存泄漏（#23260），以及提示缓存未命中（#54091）。  
- **Windows 对等性：** Computer Use、浏览器控制、沙箱设置、模型目录以及对时区敏感的模型可用性仍然是请求修复最多的一类问题。  
- **远程/云工作流可靠性：** Codex Cloud 库传输（#51737）、Sites 发布的安全凭据传输（#52801），以及远程会话兼容性（#50157）。

## 开发者痛点
- **Windows Computer Use 和沙箱不稳定：** MXC 启动器反复出现 `HRESULT 0x80070003` 故障（#52407、#53188）、dot-started 任务缺少 Computer Use 工具（#49458）、Chrome URL 检测失败（#25271）、`node_repl.exe` 上的 ACL 错误 32（#51590），以及不透明的 code-mode/沙箱拒绝（#52747）。  
- **Windows 上的模型可用性困惑：** GPT-5.6/GPT-6 出现“Model not found”（#54083、#54209）、中国标准时间触发该问题（#54340），以及 Sol Ultrafast 从第一方目录中缺失（#52839）。  
- **会话/恢复可靠性：** TUI 恢复在 `list_turns` 上失败（#37754）、`codex resume` 因 `thread/read failed` 失败（#54073）、项目成员资格回退（#53761）、聊天响应消失（#52750），以及缺少共享项目/聊天（#47366）。  
- **资源膨胀与配额意外：** 截图持久化达到约 165 GiB（#35458）、13.6 GB macOS 内存报告和交换耗尽（#44996）、子代理内存泄漏（#23260）、隐藏的记忆生成配额消耗（#51177），以及缓存输入丢失（#54091）。  
- **配置与权限不透明：** Guardian 静默省略 `auto_review.extra_policy`（#50309）、按工具插件审批模式被忽略（#41167）、选择器中缺少自定义权限配置文件（#53199），以及安全的 OpenAI Platform 工具在 Windows 上不可用（#54154）。  
- **远程/云连接阻塞问题：** Codex Cloud 库下载失败（#51737）、Sites 发布因缺少安全凭据传输而被阻塞（#52801）、远程 dot 会话格式不兼容（#50157），以及 Windows 桌面中的 Cloudflare 403（#53515）。

</details>

<details>
<summary><strong>Gemini CLI</strong> — <a href="https://github.com/google-gemini/gemini-cli">google-gemini/gemini-cli</a></summary>

# Gemini CLI 社区文摘 — 2026-10-16

## 今日亮点
过去 24 小时内没有发布新版本。活动主要集中在智能体可靠性和安全性方面：最热门的问题是 p1 报告：子智能体在达到 `MAX_TURNS` 后仍可能报告 `GOAL` 成功（[#22323](https://github.com/google-gemini/gemini-cli/issues/22323)），而高互动帖子涵盖通用智能体挂起（[#21409](https://github.com/google-gemini/gemini-cli/issues/21409)）、bash 沙盒化（[#19873](https://github.com/google-gemini/gemini-cli/issues/19873)）、AST 感知的代码库工具（[#22745](https://github.com/google-gemini/gemini-cli/issues/22745)）以及浏览器智能体故障/配置（[#22267](https://github.com/google-gemini/gemini-cli/issues/22267)、[#21983](https://github.com/google-gemini/gemini-cli/issues/21983)）。PR 流量集中在配置/认证正确性、凭据存储安全、MCP 发现超时以及终端/IDE 边缘情况。

## 热门问题
- [#22323](https://github.com/google-gemini/gemini-cli/issues/22323) — **子智能体 `MAX_TURNS` 被报告为 `GOAL` 成功。** 一个 `codebase_investigator` 子智能体报告 `status: success` 和 `Termination Reason: GOAL`，尽管它在进行分析之前就达到了最大轮次限制。这削弱了对智能体结果和评估信号的信任。**反应：** 13 条评论，2 👍；p1/need-retesting。
- [#19873](https://github.com/google-gemini/gemini-cli/issues/19873) — **为 bash 原生工作流提供零依赖的操作系统沙盒。** 提议利用 Gemini 3 模型对 bash 的原生亲和力，配合沙盒化和执行后意图路由，在不影响安全性的前提下允许 POSIX 工具链式调用。**反应：** 9 条评论，1 👍；大型 p2 增强。
- [#21409](https://github.com/google-gemini/gemini-cli/issues/21409) — **通用智能体无限期挂起。** 用户报告称，即使只是创建简单文件夹，委派给通用智能体也可能永远挂起；避免使用子智能体是一种变通方法。**反应：** 8 条评论，8 👍；p1 缺陷。
- [#22745](https://github.com/google-gemini/gemini-cli/issues/22745) — **AST 感知的文件读取、搜索和映射 EPIC。** 跟踪 AST 感知工具能否减少轮次、token 噪声和错位读取。相关：[#22746](https://github.com/google-gemini/gemini-cli/issues/22746) 和 [#22747](https://github.com/google-gemini/gemini-cli/issues/22747)。**反应：** 7 条评论，1 👍。
- [#21968](https://github.com/google-gemini/gemini-cli/issues/21968) — **Gemini 未充分利用技能和子智能体。** 轶事报告称，除非明确指示，自定义技能/子智能体很少被调用。这影响了可扩展性和智能体自主性。**反应：** 7 条评论；p2/need-retesting。
- [#22267](https://github.com/google-gemini/gemini-cli/issues/22267) — **浏览器智能体忽略 `settings.json` 覆盖。** `maxTurns` 和其他设置由 `AgentRegistry` 合并，但浏览器智能体未遵循，打破了用户配置预期。**反应：** 4 条评论；p2 缺陷。
- [#22232](https://github.com/google-gemini/gemini-cli/issues/22232) — **浏览器会话锁恢复。** 请求在持久浏览器配置文件被孤儿实例锁定时，自动进行会话接管/恢复，而不是快速失败。**反应：** 4 条评论；p3 功能。
- [#21983](https://github.com/google-gemini/gemini-cli/issues/21983) — **浏览器子智能体在 Wayland 上失败。** 浏览器子智能体报告 `GOAL` 终止，但在 Wayland 下失败。这是一个平台特定的可靠性阻碍。**反应：** 4 条评论，1 👍；p1/agent-browser。
- [#21000](https://github.com/google-gemini/gemini-cli/issues/21000) — **用于任务跟踪器的原生文件工具。** 维护者正在试验使用原生文件工具来创建和维护任务跟踪器，而不是依赖上下文内跟踪。**反应：** 4 条评论；p3/customer issue。
- [#20079](https://github.com/google-gemini/gemini-cli/issues/20079) — **符号链接的智能体文件未被识别。** `~/.gemini/agents/filename.md` 符号链接未被发现为智能体，使 dotfile 管理和智能体设置复杂化。**反应：** 4 条评论；p2/need-info。

## 关键 PR 进展
- [#29762](https://github.com/google-gemini/gemini-cli/pull/29762) — **更改认证时保留磁盘编辑。** 防止 `/auth signin` 覆盖 Gemini CLI 启动后编辑的不相关设置。p1/core。
- [#29767](https://github.com/google-gemini/gemini-cli/pull/29767) — **为 MCP `tools/list` 发现设置短超时限制。** 避免 MCP 服务器从不响应 `tools/list` 时出现静默 10 分钟停滞；默认发现超时为 60 秒。
- [#29765](https://github.com/google-gemini/gemini-cli/pull/29765) — **原子且串行化的文件凭据存储写入。** 防止多个进程并发写入或写入中断时出现静默凭据丢失或用户锁定。
- [#29756](https://github.com/google-gemini/gemini-cli/pull/29756) — **可配置的 API 请求头/正文超时。** 添加设置以在所有认证类型下提高模型 API 超时，解决长时间思考轮次中的 `UND_ERR_BODY_TIMEOUT`。
- [#29749](https://github.com/google-gemini/gemini-cli/pull/29749) — **在原子 `writeTextFile` 中遵循只读文件和硬链接。** 修复了原子重命名引入的 `0444` 文件静默覆盖和硬链接分裂。
- [#29750](https://github.com/google-gemini/gemini-cli/pull/29750) — **在加载 `.env` 文件后解析设置占位符。** 修复了加载顺序缺陷：仅在 `.env` 中定义的占位符导致设置验证失败。已关闭。
- [#29769](https://github.com/google-gemini/gemini-cli/pull/29769) — **加固 `SKILL.md` frontmatter 解析。** 处理单行描述、BOM、围栏标记后的尾部空格以及平台特定格式。已关闭。
- [#29771](https://github.com/google-gemini/gemini-cli/pull/29771) — **仅对真正为空的续写进行自动提醒，并等待提醒。** 修复了在返回无可见文本的工具循环后即发即弃的提醒。
- [#29757](https://github.com/google-gemini/gemini-cli/pull/29757) — **验证活动二进制更新以防止 PATH 遮蔽导致的误报成功。** 确保当活动二进制被遮蔽或由其他包管理器管理时，自动更新不会报告成功。
- [#29760](https://github.com/google-gemini/gemini-cli/pull/29760) — **在 `getSafeGitEnv` 中禁用交互式凭据/SSH 提示。** 防止后台 Git 扩展检查劫持 stdin/TUI 并冻结终端。已关闭。

## 功能请求趋势
- **智能体编排可靠性和可观测性。** 强烈需求正确的子智能体终止语义、可共享的子智能体轨迹、缺陷报告中的子智能体上下文，以及更好的技能/子智能体调用。示例：[#22323](https://github.com/google-gemini/gemini-cli/issues/22323)、[#22598](https://github.com/google-gemini/gemini-cli/issues/22598)、[#21763](https://github.com/google-gemini/gemini-cli/issues/21763)、[#21968](https://github.com/google-gemini/gemini-cli/issues/21968)。
- **安全的 shell 执行和沙盒化。** 用户希望 Gemini 的 bash 亲和力能够安全使用，具备操作系统级沙盒化和针对破坏性命令的防护措施。示例：[#19873](https://github.com/google-gemini/gemini-cli/issues/19873)、[#22672](https://github.com/google-gemini/gemini-cli/issues/22672)、[#23571](https://github.com/google-gemini/gemini-cli/issues/23571)。
- **AST 感知、节省 token 的代码库智能。** 对 AST 感知的读取/搜索/映射以及精准提取以减少轮次和上下文膨胀的持续兴趣。示例：[#22745](https://github.com/google-gemini/gemini-cli/issues/22745)、[#22746](https://github.com/google-gemini/gemini-cli/issues/22746)、[#22747](https://github.com/google-gemini/gemini-cli/issues/22747)、[#19561](https://github.com/google-gemini/gemini-cli/issues/19561)。
- **持久的、基于文件的任务跟踪。** 提议用 CRUD 文件跟踪替换上下文内的 `WriteToDo`，并试验原生文件工具。示例：[#18836](https://github.com/google-gemini/gemini-cli/issues/18836)、[#21000](https://github.com/google-gemini/gemini-cli/issues/21000)。
- **浏览器智能体的弹性和配置。** 请求设置覆盖、会话接管/锁恢复以及 Wayland 支持。示例：[#22267](https://github.com/google-gemini/gemini-cli/issues/22267)、[#22232](https://github.com/google-gemini/gemini-cli/issues/22232)、[#21983](https://github.com/google-gemini/gemini-cli/issues/21983)。
- **可扩展性/配置正确性和工具扩展。** 通过 `settings.json` 发现子智能体、符号链接支持、键绑定数组、技能解析以及 MCP/工具数量限制。示例：[#18285](https://github.com/google-gemini/gemini-cli/issues/18285)、[#20079](https://github.com/google-gemini/gemini-cli/issues/20079)、[#24246](https://github.com/google-gemini/gemini-cli/issues/24246)。

## 开发者痛点
- **智能体挂起和静默超时。** 通用智能体挂起、交互式提示死锁和 MCP 发现停滞是反复出现的可靠性阻碍。参见 [#21409](https://github.com/google-gemini/gemini-cli/issues/21409)、[#22465](https://github.com/google-gemini/gemini-cli/issues/22465)、[#29767](https://github.com/google-gemini/gemini-cli/pull/29767)。
- **误导性的成功/终止信号。** 智能体可能报告 `GOAL` 成功，而实际上达到轮次限制或失败，掩盖了未完成的工作。参见 [#22323](https://github.com/google-gemini/gemini-cli/issues/22323)、[#21983](https://github.com/google-gemini/gemini-cli/issues/21983)、[#21763](https://github.com/google-gemini/gemini-cli/issues/21763)。
- **配置被忽略或覆盖。** 浏览器智能体忽略 `settings.json`，符号链接的智能体被遗漏，认证/设置流程可能覆盖编辑或过早解析占位符。参见 [#22267](https://github.com/google-gemini/gemini-cli/issues/22267)、[#20079](https://github.com/google-gemini/gemini-cli/issues/20079)、[#29762](https://github.com/google-gemini/gemini-cli/pull/29762)、[#29750](https://github.com/google-gemini/gemini-cli/pull/29750)。
- **不安全的文件/git 行为。** 原子写入可能覆盖只读文件或分裂硬链接，模型可能创建分散的临时脚本或使用破坏性 git 命令。参见 [#29749](https://github.com/google-gemini/gemini-cli/pull/29749)、[#22672](https://github.com/google-gemini/gemini-cli/issues/22672)、[#23571](https://github.com/google-gemini/gemini-cli/issues/23571)。
- **终端/TUI/IDE 集成不稳定。** 调整大小时闪烁、凭据/SSH 提示劫持 stdin、diff 标签页焦点窃取以及 IDE 重连问题仍然令人痛苦。参见 [#21924](https://github.com/google-gemini/gemini-cli/issues/21924)、[#29760](https://github.com/google-gemini/gemini-cli/pull/29760)、[#28183](https://github.com/google-gemini/gemini-cli/pull/28183)、[#29744](https://github.com/google-gemini/gemini-cli/pull/29744)。
- **工具数量和 MCP 扩展限制。** CLI 在大型工具集下遇到错误，MCP 服务器可能停滞发现。参见 [#24246](https://github.com/google-gemini/gemini-cli/issues/24246)、[#29767](https://github.com/google-gemini/gemini-cli/pull/29767)。

---

</details>

<details>
<summary><strong>GitHub Copilot CLI</strong> — <a href="https://github.com/github/copilot-cli">github/copilot-cli</a></summary>

# GitHub Copilot CLI 社区摘要 — 2026-10-16

## 今日亮点
过去 24 小时内有两次发布：[v1.0.99-1](https://github.com/github/copilot-cli/releases/tag/v1.0.99-1) 为一次性 prompt/管道会话新增了 `--trust-folder`，并通过 `ghapp://air/install?...` 支持安装经过验证的 Skills/插件；而 [v1.0.99-0](https://github.com/github/copilot-cli/releases/tag/v1.0.99-0) 新增了 `/share` 操作菜单和会话级沙箱。社区关注点仍集中在输入/会话 UX 和平台可靠性上：长期存在的 vi/vim 输入请求以 76 👍 关闭，多个 sandbox/MCP 问题仍在推动讨论。过去 24 小时内没有拉取请求更新。

## 发布

### [v1.0.99-1](https://github.com/github/copilot-cli/releases/tag/v1.0.99-1)
- **新增：** 为通过 `-p` 或管道 stdin 启动的一次性 prompt 模式会话新增 `--trust-folder`。
- **新增：** 通过 `copilot plugin install "ghapp://air/install?..."` 从 GitHub 安装链接安装经过验证的 Skills 和插件。
- **改进：** Microsoft Entra 的 `/login` 现在允许选择缓存账户。

### [v1.0.99-0](https://github.com/github/copilot-cli/releases/tag/v1.0.99-0)
- **新增：** 不带参数的 `/share` 会打开全屏操作菜单，用于分享当前会话。
- **改进：** `/sandbox` 可以为当前会话保持启用沙箱，而无需全局启用。
- **改进：** 受信任仓库插件会在聊天运行前自动安装并更新。
- 来源中的其他发布说明文本被截断。

## 热门 Issue
过去 24 小时内有 39 个 issue 更新；来源按评论数展示了前 30 个。以下为 10 个值得注意的条目，依据参与度、影响力和时效性选出。

- **[#13 — CLI input should have a vi/vim input mode](https://github.com/github/copilot-cli/issues/13)** — `CLOSED` | 14 条评论 | 👍76。这是一个长期被请求的模态编辑模式，面向键盘驱动用户。它在获得强力支持后关闭，确认了输入人体工学是社区最关注的问题之一。

- **[#1857 — Allow users to cancel or remove enqueued messages before execution](https://github.com/github/copilot-cli/issues/1857)** — `OPEN` | 12 条评论 | 👍30。通过 `Ctrl+Q` / `Ctrl+Enter` 排队的消息在 agent 忙碌时或 `/compact` 期间无法移除。这会在长时间运行的任务中造成意外 prompt。

- **[#4998 — Copilot CLI unusable after macOS update/reboot because `.mcp-writer.binding` persists stale filesystem device ID](https://github.com/github/copilot-cli/issues/4998)** — `CLOSED` | 11 条评论 | 👍11。一个特定于平台的 MCP 状态问题，会在 macOS 更新后破坏新建和恢复的会话。虽已关闭，但凸显了持久 MCP 绑定的脆弱性。

- **[#5105 — macOS sandbox blocks Gradle daemon connection despite local networking being allowed](https://github.com/github/copilot-cli/issues/5105)** — `OPEN` | 5 条评论。Gradle daemon 在 localhost 上启动，但客户端无法在 macOS 沙箱内连接。这会对 Java/Gradle 构建工作流产生重大影响。

- **[#4991 — MCP: Cloudflare connection fails with “Subscription limit reached” after successful OAuth](https://github.com/github/copilot-cli/issues/4991)** — `CLOSED` | 5 条评论。OAuth 成功，但运行时报出订阅限制错误，随后又误导性地提示需要认证。这表明 MCP 认证/错误处理仍然复杂。

- **[#1575 — Allow Copilot to see the output from user shell commands](https://github.com/github/copilot-cli/issues/1575)** — `CLOSED` | 3 条评论 | 👍4。用户希望手动运行的 shell 命令及其输出被纳入 Copilot 的上下文。该功能请求已关闭，但指向更深层的上下文捕获需求。

- **[#4515 — Copilot CLI exposes both MCP `content` and `structuredContent`](https://github.com/github/copilot-cli/issues/4515)** — `OPEN` | 3 条评论。当 MCP 工具结果同时包含这两个字段时，Copilot 似乎会把两者都加入上下文。预期行为是优先使用 `structuredContent`。

- **[#3100 — HTTP MCP server with Bearer token fails OAuth discovery instead of falling back to headers auth](https://github.com/github/copilot-cli/issues/3100)** — `OPEN` | 1 条评论 | 👍10。一种常见的 `.mcp.json` 配置模式会失效，因为 CLI 会尝试 OAuth 发现，而不是使用已配置的 Authorization 头。尽管评论数少，但社区信号很强。

- **[#5139 — Claude models (and Gemini Argon) fail with HTTP 400 “request body is not valid JSON”](https://github.com/github/copilot-cli/issues/5139)** — `OPEN` | `triage` | 1 条评论。据报道，Claude Sonnet 5.5、Claude Opus 5.5 和 Gemini Argon 会失败，而其他模型正常工作。跨模型兼容性回归是影响很大的问题。

- **[#4407 — Add Chinese (zh-CN) UI localization for the GitHub Copilot desktop app](https://github.com/github/copilot-cli/issues/4407)** — `OPEN` | 1 条评论 | 👍10。请求为菜单、设置以及终端/CLI 集成提供本地化。强烈反响表明对非英语 UX 的需求。

## 关键 PR 进展
过去 24 小时内没有拉取请求更新（`Total: 0`）。今天没有可总结的 PR。

## 功能请求趋势
- **输入与键盘控制：** vi/vim 模式 ([#13](https://github.com/github/copilot-cli/issues/13))、取消已排队消息 ([#1857](https://github.com/github/copilot-cli/issues/1857))、通过 `commandHistoryMaxSize` 提高 prompt 历史记录 ([#5159](https://github.com/github/copilot-cli/issues/5159))，以及可重新用途的顶部标签页 ([#5153](https://github.com/github/copilot-cli/issues/5153))。
- **沙箱与权限灵活性：** macOS 上的 localhost/Gradle 访问 ([#5105](https://github.com/github/copilot-cli/issues/5105))、Linux bubblewrap 预检修复 ([#5052](https://github.com/github/copilot-cli/issues/5052))、带空格的 `commandIdentifiers` ([#4150](https://github.com/github/copilot-cli/issues/4150))，以及区分沙箱 git 凭据 ([#5102](https://github.com/github/copilot-cli/issues/5102))。
- **MCP 与认证健壮性：** 优先使用 `structuredContent` ([#4515](https://github.com/github/copilot-cli/issues/4515))、使用 Bearer token 回退而不是 OAuth 发现 ([#3100](https://github.com/github/copilot-cli/issues/3100))、面向来宾账户的租户特定 MCP 认证 ([#5157](https://github.com/github/copilot-cli/issues/5157))，以及仅注册表模式的云 MCP 策略处理 ([#5155](https://github.com/github/copilot-cli/issues/5155))。
- **模型兼容性与路由：** Claude/Gemini 的无效 JSON 失败 ([#5139](https://github.com/github/copilot-cli/issues/5139))、不支持的子代理推理强度 ([#4473](https://github.com/github/copilot-cli/issues/4473))、Sol 上下文预算拒绝 ([#5151](https://github.com/github/copilot-cli/issues/5151))，以及 Gemini 缓存遥测异常 ([#5161](https://github.com/github/copilot-cli/issues/5161))。
- **上下文、记忆与工具输出：** Copilot 能够看到手动运行的 shell 命令输出 ([#1575](https://github.com/github/copilot-cli/issues/1575))，以及保留 plan 模式引导响应 ([#5160](https://github.com/github/copilot-cli/issues/5160))。
- **本地化与无障碍：** 中文 UI 本地化 ([#4407](https://github.com/github/copilot-cli/issues/4407))。
- **浏览器自动化可发现性：** 与 `/computer` 并列提供 `/browser` 或 `/playwright` ([#5152](https://github.com/github/copilot-cli/issues/5152))。
- **会话生命周期：** 带 `--new-session-id` 的 `/restart` ([#5147](https://github.com/github/copilot-cli/issues/5147))，以及会话名称回退为关联 PR 标题 ([#5158](https://github.com/github/copilot-cli/issues/5158))。

## 开发者痛点
- **沙箱与平台摩擦仍是最大的一类问题：** macOS 沙箱阻止本地 Gradle daemon 连接 ([#5105](https://github.com/github/copilot-cli/issues/5105))、Linux bubblewrap 命名空间预检失败 ([#5052](https://github.com/github/copilot-cli/issues/5052))、macOS MCP 绑定状态陈旧 ([#4998](https://github.com/github/copilot-cli/issues/4998))，以及沙箱 git 凭据限制 ([#5102](https://github.com/github/copilot-cli/issues/5102))。
- **MCP 认证/配置仍容易出错：** Bearer token 落入 OAuth 发现 ([#3100](https://github.com/github/copilot-cli/issues/3100))、来宾账户租户选择 ([#5157](https://github.com/github/copilot-cli/issues/5157))、Cloudflare OAuth/订阅错误 ([#4991](https://github.com/github/copilot-cli/issues/4991))，以及重复的 `content`/`structuredContent` 上下文 ([#4515](https://github.com/github/copilot-cli/issues/4515))。
- **模型特定回归中断工作流：** Claude/Gemini 上的 HTTP 400 无效 JSON 失败 ([#5139](https://github.com/github/copilot-cli/issues/5139))、子代理不支持的推理强度 ([#4473](https://github.com/github/copilot-cli/issues/4473))，以及首个 prompt 之前的上下文预算拒绝 ([#5151](https://github.com/github/copilot-cli/issues/5151))。
- **输入与会话控制缺口：** 无法取消已排队消息 ([#1857](https://github.com/github/copilot-cli/issues/1857))、`/restart` 与 `--new-session-id` 冲突 ([#5147](https://github.com/github/copilot-cli/issues/5147))、prompt 历史记录上限 ([#5159](https://github.com/github/copilot-cli/issues/5159))，以及 plan 模式引导响应消失 ([#5160](https://github.com/github/copilot-cli/issues/5160))。
- **平台特定的 skill/plugin 发现问题持续存在：** Windows `skill` 工具无法在 `~/.agents/skills` 下找到有效 skills ([#4401](https://github.com/github/copilot-cli/issues/4401))，即便发布说明已改进经过验证的插件/skill 安装。

</details>

<details>
<summary><strong>OpenCode</strong> — <a href="https://github.com/anomalyco/opencode">anomalyco/opencode</a></summary>

# OpenCode 社区简报 — 2026-10-16

## 今日亮点
V2 迁移持续主导社区关注：LSP/格式化器对等性、agent 模型解析和降级安全性都是活跃讨论串，过去 24 小时内不断有可复现的 bug 报告提交。平台方面，团队在 v1.19.1 中停用了会话分享，并正在桌面应用中推进生成式 UI 和 GUI 插件扩展能力。Windows/升级路径和权限检查缺陷构成了互动量最高的一批问题。

## 版本发布
**v1.19.1** — 仅包含核心 bug 修复：
- **会话分享已停用。** 现有分享链接和会话分享命令不再可用，且不支持从分享 URL 导入会话。预计依赖分享链接进行协作的用户会有后续讨论。

## 热门 Issue

1. **[#50672 — 硬编码默认端口 49374 导致第二个客户端无法连接](https://github.com/anomalyco/opencode/issues/50672)**（10 条评论，OPEN）
   托管后台服务绑定的是机器全局端口，而注册是按用户进行的，因此共享回环地址（多用户主机、WSL 镜像网络）上的任何第二个客户端都会遇到通用超时。对共享开发机影响很大；是今天评论最多的 issue。

2. **[#53756 — Bedrock 推理配置文件 ARN 为 Claude 获取 Nova 格式的 effort 变体](https://github.com/anomalyco/opencode/issues/53756)**（9 条评论，OPEN）
   由 Claude 支持的应用程序推理配置文件收到 Nova 风格的 `reasoningConfig`，导致 400 错误。这是早先 V1 报告在 V2 中的对应问题——对 Bedrock 企业用户而言是真正的阻塞项。

3. **[#55629 — Big Pickle 的 “THINKING” 渗入答案](https://github.com/anomalyco/opencode/issues/55629)**（9 条评论，OPEN，待关闭/分类）
   `/think` 标签原样出现在输出中，要么是模型错误发出，要么是客户端解析错误。用户报告最近四个版本都存在该问题。

4. **[#50916 — v2 中的 LSP 支持被砍掉了？](https://github.com/anomalyco/opencode/issues/50916)**（7 条评论，10 👍，OPEN）
   迁移文档称 V2 不运行语言服务器、不暴露 LSP 工具，也不生成诊断信息，社区因此要求澄清。这是本组中反响最高的 issue——一项核心能力回退。

5. **[#38528 — V2：移植 LSP 和格式化器服务](https://github.com/anomalyco/opencode/issues/38528)**（6 条评论，11 👍，OPEN）
   这是在 V2 Core/运行时架构下恢复 LSP/格式化器行为的跟踪 issue。11 个 👍 表明它是 V2 中最受期待的功能。

6. **[#55163 — V2 TUI 在 Windows ConPTY 上 /exit 后让终端停留在 raw 模式](https://github.com/anomalyco/opencode/issues/55163)**（8 条评论，OPEN）
   在 Alacritty + zellij-windows 下退出可能损坏 shell 提示符或导致多路复用器崩溃。这是 V1 报告在 V2 中的对应问题；对 Windows TUI 用户影响显著。

7. **[#51003 — 全局 stdio MCP 服务器会为每个已加载目录各启动一次](https://github.com/anomalyco/opencode/issues/51003)**（6 条评论，OPEN）
   打开大量目录的客户端（例如每个聊天一个目录）会使 MCP 进程成倍增加，直到主机 OOM、健康检查超时。这是多目录工作流的扩展性隐患。

8. **[#55727 — 新会话忽略 Agent `model` 配置](https://github.com/anomalyco/opencode/issues/55727)**（3 条评论，已复现，OPEN）
   在 2.0.29 中，`agents.<id>.model` 和 agent frontmatter 被忽略；`opencode run` 始终使用默认模型。已复现，且已有社区 PR（[#55894](https://github.com/anomalyco/opencode/pull/55894)）。

9. **[#55886 — 工具调用中途服务器重启会破坏下一个 Anthropic 请求](https://github.com/anomalyco/opencode/issues/55886)**（2 条评论，OPEN）
   被中断的 `shell` 工具调用会重放格式错误的 `tool_use.input`，Anthropic 以 `400 Input should be an object` 拒绝。一次重启破坏了所有三个有实时工具调用的会话——已由 PR [#55900](https://github.com/anomalyco/opencode/pull/55900) 修复。

10. **[#55525 — 管道 stdout 恰好在 64 KiB 处被截断](https://github.com/anomalyco/opencode/issues/55525)**（3 条评论，已复现，OPEN）
    `opencode --completions fish` 在 65,536 字节处截断，破坏了 Homebrew 生成的 fish 补全。已由 PR [#55893](https://github.com/anomalyco/opencode/pull/55893) 修复。

*同样值得关注：* [#54428](https://github.com/anomalyco/opencode/issues/54428) 和 [#54305](https://github.com/anomalyco/opencode/issues/54305)（streaming-markdown 渲染性能）、[#55555](https://github.com/anomalyco/opencode/issues/55555)（V2→v1 降级留下混合数据库，所有会话 500）、[#51553](https://github.com/anomalyco/opencode/issues/51553)/[#55888](https://github.com/anomalyco/opencode/issues/55888)（npm 12 升级跳过 postinstall，留下 229 字节的 `opencode.exe`）、[#55726](https://github.com/anomalyco/opencode/issues/55726)（`max_tokens` 与 `max_completion_tokens` 一起发送，严格端点拒绝）。

## 关键 PR 进展

1. **[#54747 — feat(app): 在 GUI 中实现生成式 UI](https://github.com/anomalyco/opencode/pull/54747)**（OPEN）
   Agent 通过 `genui_render` 发出一个小型 GenUI 程序；应用在调用流式传输时用 OpenCode 自己的组件渲染它，并提供渲染验证反馈。默认开启，位于 Settings → Experimental 下进行开关控制。

2. **[#55901 — fix(infra): 使用 Workers Static Assets 部署 beta Web 应用](https://github.com/anomalyco/opencode/pull/55901)**（OPEN）
   修复了 Office 预览上线后 `beta.opencode.ai` 部署失败并报错代码 10024 的问题（一个 27 MB 的 wasm 资源超过了 25 MiB 的 KV 限制）。

3. **[#55900 — fix(core): 让被中断的工具调用对 Anthropic 保持可重放](https://github.com/anomalyco/opencode/pull/55900)**（OPEN，关闭 #55886）
   确保已中止的 `streaming`/`running` 工具调用在重放时发出有效的 `tool_use` 负载，而不是 `input: null`。

4. **[#55893 — fix(cli): 让大块 stdout 写入突破单个管道缓冲区](https://github.com/anomalyco/opencode/pull/55893)**（OPEN，关闭 #55525）
   根因追溯到 stdout/stderr 被遗留为非阻塞状态；恢复了约 68 KiB 的补全输出通过管道传输的能力。

5. **[#55246 — feat(desktop): 通过 `./gui` 导出发布 GUI 插件](https://github.com/anomalyco/opencode/pull/55246)**（OPEN）
   `opencode.json` 中的插件在 `server` 和 `./tui` 条目之外新增 GUI 部分。内置桌面功能变为内置插件，列在 Settings → Plugins 中。

6. **[#51195 — feat(tui): 分页 transcript 输出实验](https://github.com/anomalyco/opencode/pull/51195)**（OPEN，关闭 #51203）
   为提升可读性增加分页 transcript 输出——直接回应用户反馈阅读长流式 transcript 导致眼睛疲劳的问题。

7. **[#55894 — fix(core): 遵循所选 agent 配置的模型](https://github.com/anomalyco/opencode/pull/55894)**（OPEN，关闭 #55727）
   社区贡献者修复了新会话忽略 agent 级 `model` 的问题。

8. **[#49954](https://github.com/anomalyco/opencode/pull/49954) / [#55883 — fix(core): 即使命令无法解析也运行 shell 权限检查](https://github.com/anomalyco/opencode/pull/55883)**（OPEN / CLOSED）
   被 shell 扫描器分解为零个节点的命令（裸重定向、带 `--` 的 PowerShell）此前会跳过 `permission.assert` 并在未检查的情况下运行。这是一项有意义的安全加固。

9. **[#55871 — fix(tui): 在舍入边界将紧凑形式进位到 M](https://github.com/anomalyco/opencode/pull/55871)**（CLOSED，关闭 #55256）
   `Locale.number` 在舍入前就选择单位，导致 999,950–999,999 被打印为 `1000.0K`。贡献者修复，已合并。

10. **[#55020](https://github.com/anomalyco/opencode/pull/55020) / [#54482 — fix: 粘贴到未聚焦的 composer；在 `/btw` 附带提问中包含附加图片](https://github.com/anomalyco/opencode/pull/54482)**（OPEN）
    两个输入路径上的小毛病：`⌘V` 在会话 composer/嵌入式终端中插入字面量 `v`，以及 `/btw` 丢弃刚附加的图片。

*另已合并：* [#55248](https://github.com/anomalyco/opencode/pull/55248)（Commentary 时间线分类）、[#55889](https://github.com/anomalyco/opencode/pull/55889)（文件树中长文件名淡出）、[#55879](https://github.com/anomalyco/opencode/pull/55879)（切换侧边终端时 Review 窗格保持稳定）、[#55899](https://github.com/anomalyco/opencode/pull/55899)（重新读取链接时文件视图保持原位）、[#55756](https://github.com/anomalyco/opencode/pull/55756)（冷激活前发布 prompt-prep 状态）。

## 功能请求趋势
- **V2 与 V1 的对等性是首要诉求。** LSP 诊断和格式化器服务（[#38528](https://github.com/anomalyco/opencode/issues/38528)、[#50916](https://github.com/anomalyco/opencode/issues/50916)）拥有最强烈的 👍 信号，而迁移摩擦正在产生一长串对等性 bug。
- **扩展性与 UI 界面能力。** 生成式 UI（[#54747](https://github.com/anomalyco/opencode/pull/54747)）和 GUI 插件导出（[#55246](https://github.com/anomalyco/opencode/pull/55246)）表明团队正在推动让桌面应用可被 agent 和第三方插件编程。
- **结构化、分页、可读的输出。** 分页 transcript 实验（[#51195](https://github.com/anomalyco/opencode/pull/51195)）、commentary 时间线分类以及 thinking 文本颜色修复（[#55686](https://github.com/anomalyco/opencode/issues/55686)）共同构成了一个连贯的 TUI 可读性主题。
- **远程/便携会话访问。** OpenTunnel 可靠性（[#54624](https://github.com/anomalyco/opencode/issues/54624)、[#55873](https://github.com/anomalyco/opencode/issues/55873)、[#55864](https://github.com/anomalyco/opencode/issues/55864)）是尚未满足的需求，尤其是在分享链接已经消失之后。
- **更安全的 shell 执行。** 对无法解析命令的权限评估（[#49954](https://github.com/anomalyco/opencode/pull/49954)、[#55883](https://github.com/anomalyco/opencode/pull/55883)）反映了对纵深防御的需求。

## 开发者痛点
- **V2 迁移双向都有损失。** V1→V2 丢失 LSP/格式化器行为和 agent `model` 解析；V2→V1 降级会留下混合数据库，导致所有现有会话返回 HTTP 500（[#55555](https://github.com/anomalyco/opencode/issues/55555)）。
- **Windows/npm 上的升级路径很脆弱。** `opencode upgrade` 报告成功，却留下一个 229 字节的文本存根作为 `bin/opencode.exe`，破坏下一次调用（[#51553](https://github.com/anomalyco/opencode/issues/51553)、[#55888](https://github.com/anomalyco/opencode/issues/55888)）——postinstall 被跳过。
- **TUI/终端状态管理。** Windows ConPTY raw 模式泄漏（[#55163](https://github.com/anomalyco/opencode/issues/55163)）、markdown 光标偏移损坏（[#48714](https://github.com/anomalyco/opencode/issues/48714)）以及柔和颜色回归（[#55686](https://github.com/anomalyco/opencode/issues/55686)）显示出 V1 与 V2 之间的渲染器/收尾回归。
- **资源扩展。** 每个已加载目录一个 MCP 进程（[#51003](https://github.com/anomalyco/opencode/issues/51003)）以及位置失效时重复的 fff 引擎（[#54631](https://github.com/anomalyco/opencode/issues/54631)）会在多会话工作流中耗尽内存。
- **流式渲染成本。** 两份性能分析报告量化了 session-ui markdown store 中每个增量的主线程成本（[#54305](https://github.com/anomalyco/opencode/issues/54305)、[#54428](https://github.com/anomalyco/opencode/issues/54428)）——占主导的是块重新解析，而不是 store 遍历。
- **提供商集成回归。** 严格的 OpenAI 兼容端点拒绝 `max_tokens` + `max_completion_tokens`（[#55726](https://github.com/anomalyco/opencode/issues/55726)）、Bedrock ARN 处理（[#53756](https://github.com/anomalyco/opencode/issues/53756)）以及中断调用重放（[#55886](https://github.com/anomalyco/opencode/issues/55886)）都会破坏此前可用的配置。
- **远程配对不稳定。** `pair --remote` 返回 503，以及卡住的待处理隧道且没有重置路径（[#55873](https://github.com/anomalyco/opencode/issues/55873)、[#54624](https://github.com/anomalyco/opencode/issues/54624)），让用户没有恢复机制。

*本期间未提供 Discussion 数据，因此省略第 5 节。*

</details>

<details>
<summary><strong>Pi</strong> — <a href="https://github.com/earendil-works/pi">earendil-works/pi</a></summary>

# Pi 社区文摘 — 2026-10-16

来源：[earendil-works/pi](https://github.com/earendil-works/pi)。过去 24 小时快照：1 个版本发布、50 个更新的 issue、16 个更新的 PR、1 个讨论。

## 1. 今日亮点

- **v1.3.0** 通过 `sessionShareDestination`（`auto`、`gist` 或 `radius`）为 `/share` 上传目标增加可配置选项。
- 互动最高的 issue 集中在 **OAuth/身份持久化**、**终端兼容性回归**、**全屏渲染性能** 和 **上下文/压缩可靠性**。
- PR 队列聚焦于健壮性：上下文恢复、流式 JSON 解析性能、小上下文压缩调优、模型兼容性以及扩展 UI 生命周期修复。

## 2. 版本发布

- **[v1.3.0](https://github.com/earendil-works/pi/releases/tag/v1.3.0)** — 新功能：使用 `sessionShareDestination` 选择 `/share` 的上传位置，支持 `auto`、`gist` 或 `radius`。有关会话共享/导出行为，请参阅链接的发布说明和文档。

## 3. 热门 Issue

1. **[#10300 — ChatGPT OAuth ID token is not persisted](https://github.com/earendil-works/pi/issues/10300)**  
   `credentialFromTokenResponse` 省略了 ID token，因此扩展无法访问账户身份；token 刷新也走同一路径。**反应：** 16 条评论，开放中 — 高影响的认证/扩展问题。

2. **[#10256 — Terminal color query replies leak into prompt and BEL opens external editor](https://github.com/earendil-works/pi/issues/10256)**  
   影响 Windows 上 0.99.x 的 mintty/ConPTY；终端回复出现在提示符中，BEL 会触发编辑器。**反应：** 12 条评论，4 👍 — 值得注意的 Windows 终端回归。

3. **[#9549 — Large transcripts re-render every frame; resize re-emits whole transcript](https://github.com/earendil-works/pi/issues/9549)**  
   全屏模式在处理大型记录时会占满一个核心，在核心较少的 Windows Terminal 配置上尤其如此。**反应：** 11 条评论，1 👍 — 持续的性能痛点。

4. **[#9335 — openai-responses: support `configuration_update` for cache-preserving reasoning changes](https://github.com/earendil-works/pi/issues/9335)**  
   将允许用户更改推理强度，而不会破坏 GPT-6 的提示缓存。**反应：** 11 条评论，8 👍，以 no-action 关闭 — 尽管关闭，需求强烈。

5. **[#10840 — GPT-6.1 Sol still uses 272K after Codex default increased to 500K](https://github.com/earendil-works/pi/issues/10840)**  
   过期的目录元数据使 Pi 的上下文窗口和压缩预算落后于上游 Codex。**反应：** 10 条评论，已关闭 — 模型目录漂移问题。

6. **[#10063 — Anthropic OAuth returns `Invalid effort level` for Opus 5/5.5 and Fable 5](https://github.com/earendil-works/pi/issues/10063)**  
   直接的 Anthropic OAuth 请求在 default/low/high 思考模式下失败。**反应：** 8 条评论，开放中 — 阻碍 Anthropic OAuth 用户。

7. **[#10074 — Anthropic tool calls corrupt non-ASCII edit arguments](https://github.com/earendil-works/pi/issues/10074)**  
   在 `\uXXXX` 中丢失 `u` 会导致控制字符并偶尔造成文件损坏，尤其是在韩文文本中。**反应：** 8 条评论，开放中 — 数据完整性风险。

8. **[#10162 — Too many input images stop the agent task](https://github.com/earendil-works/pi/issues/10162)**  
   会破坏依赖自动压缩的长时间自主会话。**反应：** 7 条评论，开放中 — 影响 agent 持久性。

9. **[#10330 — Auto-compaction does not start in CLI mode](https://github.com/earendil-works/pi/issues/10330)**  
   `pi --mode json` 从不自动压缩，而 TUI 模式正常。**反应：** 7 条评论，开放中 — CLI 可靠性缺口。

10. **[#10045 — Auto-compaction of Opus 5.5 on Bedrock blocked by Anthropic policy](https://github.com/earendil-works/pi/issues/10045)**  
    长会话在上下文接近 ~98% 时失败，因为摘要被当作策略违规阻止。**反应：** 6 条评论，更新于 2026-10-16 — 对 Bedrock/Opus 用户至关重要。

## 4. 关键 PR 进展

1. **[PR #11016 — Fix/copilot request size overflow](https://github.com/earendil-works/pi/pull/11016)** *(OPEN)*  
   针对包含图像的大型会话；`/compact` 可手动缓解，但该 PR 旨在实现自动恢复。

2. **[PR #11008 — Throttle streaming partial-json re-parse](https://github.com/earendil-works/pi/pull/11008)** *(CLOSED)*  
   修复 O(n²) 的 Anthropic 流解析问题，该问题在多 MB 工具参数上可能使事件循环卡住数分钟。

3. **[PR #10964 — Base context recovery on declared model metadata](https://github.com/earendil-works/pi/pull/10964)** *(CLOSED)*  
   当一个回合耗尽上下文时，Pi 通过一次压缩和继续来恢复，而不是返回空响应。

4. **[PR #11009 — Use 90% threshold for small-context compaction](https://github.com/earendil-works/pi/pull/11009)** *(CLOSED)*  
   通过更改触发阈值，避免 200k–400k 上下文模型过早压缩。

5. **[PR #11007 — Check model catalog against latest release before publishing](https://github.com/earendil-works/pi/pull/11007)** *(CLOSED)*  
   防止目录更新要求旧版 Pi 发布不具备的代码，这种故障曾在 kimi-coding 中出现。

6. **[PR #11005 — Avoid `thinkingBudget` and MINIMAL on unmapped Gemini 4 models](https://github.com/earendil-works/pi/pull/11005)** *(CLOSED)*  
   针对 Gemini 4 自定义 `models.json` 定义和 `pi-ai` 调用方遇到 HTTP 400 的 P0 修复。

7. **[PR #11010 — Include allowed values in enum validation errors](https://github.com/earendil-works/pi/pull/11010)** *(CLOSED)*  
   向模型提供有效的枚举集合，减少重试往返和手动检查 schema。

8. **[PR #9569 — Coerce JSON-encoded object and array tool arguments](https://github.com/earendil-works/pi/pull/9569)** *(OPEN)*  
   恢复模型错误地双重编码为 JSON 字符串的嵌套工具参数。

9. **[PR #10521 — Inline `$ref` tool schemas for NVIDIA NIM models](https://github.com/earendil-works/pi/pull/10521)** *(OPEN)*  
   修复 NVIDIA NIM 模型将本地 `$ref` 对象作为 JSON 字符串返回的问题，`validateToolArguments` 目前会拒绝这些内容。

10. **[PR #11000 — Drop thinking from other models instead of sending as text](https://github.com/earendil-works/pi/pull/11000)** *(CLOSED)*  
    防止在切换模型后，旧模型的推理块被作为可见文本重放。

## 5. 热门讨论

**Show and tell**

- **[#10974 — swe-mux: run pi sessions side by side and from your phone](https://github.com/earendil-works/pi/discussions/10974)**  
  原生 Pi 状态检测、标签页/窗格、开发服务器预览，以及通过实时终端会话进行移动访问。**反应：** 1 👍。

在提供的 24 小时数据中，没有出现 Ideas 或 Q&A 讨论。

## 6. 功能请求趋势

- **扩展 API 成熟度：** 更多可观测性和生命周期控制 — 为已解释错误抑制 bug 提示（[#10354](https://github.com/earendil-works/pi/issues/10354)），为注入输入发送 `queue_update`（[#10317](https://github.com/earendil-works/pi/issues/10317)），持久化宿主扩展（[#10386](https://github.com/earendil-works/pi/issues/10386)），从 `agent_settled` 管理 prompt 生命周期（[#10388](https://github.com/earendil-works/pi/issues/10388)），以及原子化托管会话空闲关闭（[#11018](https://github.com/earendil-works/pi/issues/11018)）。
- **提供商/模型元数据正确性：** 保留缓存的推理控制（[#9335](https://github.com/earendil-works/pi/issues/9335)）、上下文窗口更新（[#10840](https://github.com/earendil-works/pi/issues/10840)）、Gemini 思考签名（[#10157](https://github.com/earendil-works/pi/issues/10157)）以及 OAuth 身份持久化（[#10300](https://github.com/earendil-works/pi/issues/10300)）。
- **上下文与压缩控制：** CLI 自动压缩（[#10330](https://github.com/earendil-works/pi/issues/10330)）、Bedrock Opus 策略变通方案（[#10045](https://github.com/earendil-works/pi/issues/10045)）、图像密集型会话限制（[#10162](https://github.com/earendil-works/pi/issues/10162)）以及更保守的小上下文压缩（[PR #11009](https://github.com/earendil-works/pi/pull/11009)）。
- **性能与成本意识：** 全屏记录渲染（[#9549](https://github.com/earendil-works/pi/issues/9549)）、codemode 高亮复用（[#10971](https://github.com/earendil-works/pi/issues/10971)），以及避免在运行中系统提示头部移动时重复计费（[#10024](https://github.com/earendil-works/pi/issues/10024)）。
- **终端/UX 健壮性：** CJK 粗体渲染（[#10154](https://github.com/earendil-works/pi/issues/10154)）、NumLock/kitty 键绑定（[#10352](https://github.com/earendil-works/pi/issues/10352)）、粘贴/编辑器边界情况（[#10351](https://github.com/earendil-works/pi/issues/10351)）以及 OSC 冲突（[#11017](https://github.com/earendil-works/pi/issues/11017)）。

## 7. 开发者痛点

- **OAuth 与提供商认证碎片化：** ChatGPT OAuth ID token 持久化（[#10300](https://github.com/earendil-works/pi/issues/10300)）和 Anthropic OAuth effort-level 失败（[#10063](https://github.com/earendil-works/pi/issues/10063)）仍然阻力很大。
- **终端/平台回归：** mintty/ConPTY 颜色查询、BEL 打开外部编辑器（[#10256](https://github.com/earendil-works/pi/issues/10256)）、NumLock 键绑定不匹配（[#10352](https://github.com/earendil-works/pi/issues/10352)）、stdin `read EIO` 崩溃（[#10272](https://github.com/earendil-works/pi/issues/10272)）以及 OSC 标记冲突（[#11017](https://github.com/earendil-works/pi/issues/11017)）。
- **上下文/压缩可靠性：** CLI 自动压缩未触发（[#10330](https://github.com/earendil-works/pi/issues/10330)）、Bedrock Opus 策略阻止（[#10045](https://github.com/earendil-works/pi/issues/10045)）、恢复的上下文级别错误（[#10082](https://github.com/earendil-works/pi/issues/10082)）以及图像密集型会话停止任务（[#10162](https://github.com/earendil-works/pi/issues/10162)）。
- **性能瓶颈：** 全屏记录重渲染（[#9549](https://github.com/earendil-works/pi/issues/9549)）、O(n²) 流式部分 JSON 解析（[PR #11008](https://github.com/earendil-works/pi/pull/11008)）以及重复的 codemode 高亮（[#10971](https://github.com/earendil-works/pi/issues/10971)）。
- **工具调用/数据完整性：** 损坏的非 ASCII 编辑参数（[#10074](https://github.com/earendil-works/pi/issues/10074)）、JSON 编码的嵌套参数（[PR #9569](https://github.com/earendil-works/pi/pull/9569)）、`$ref` schema 拒绝（[PR #10521](https://github.com/earendil-works/pi/pull/10521)）以及丢失的 Gemini 思考签名（[#10157](https://github.com/earendil-works/pi/issues/10157)）。
- **扩展易用性：** 注入输入没有回执（[#10317](https://github.com/earendil-works/pi/issues/10317)）、延迟 `prompt()` 行为静默（[#10388](https://github.com/earendil-works/pi/issues/10388)）以及自定义 UI 覆盖层关闭 bug（[PR #10999](https://github.com/earendil-works/pi/pull/10999)）。
- **配置持久化：** 交互模式静默忽略设置保存失败，例如只读 `settings.json`（[#10168](https://github.com/earendil-works/pi/issues/10168)）。

</details>

<details>
<summary><strong>Qwen Code</strong> — <a href="https://github.com/QwenLM/qwen-code">QwenLM/qwen-code</a></summary>

# Qwen Code 社区摘要 — 2026-10-16

## 1. 今日亮点

v0.25.2 系列已在 CLI、Desktop 和 TypeScript SDK（后者现在捆绑 CLI 0.25.2）上发布，带来又一大批 `managed-agent` 加固——尤其是基于 Session 消息的 H4e-b2 lead mailbox，以及来自 #12692 R2 评审的 connector/broker 健壮性修复。活动主要集中在 Hosted/serve 控制平面：故障门测试覆盖（FG8/FG9）、会话生命周期清理，以及仅 journal 的崩溃恢复。与此同时，面向用户的 bug 报告集中在优雅降级（大量工具输出撑爆上下文窗口）、长生命周期会话中的资源泄漏，以及 Web Shell 与终端的功能对等。

## 2. 发布

| 发布 | 说明 |
|---|---|
| [v0.25.2](https://github.com/QwenLM/qwen-code/releases) | 稳定 CLI 发布。亮点：`feat(managed-agent): H4e-b2` — Hosted 团队 lead 的 mailbox 通过 Session 消息投递（[#13892](https://github.com/QwenLM/qwen-code/pull/13892)）；来自 #12692 R2 评审的 connector 与 broker 健壮性修复（[#13330](https://github.com/QwenLM/qwen-code/pull/13330)）。无已知破坏性变更。 |
| [desktop-v0.25.2](https://github.com/QwenLM/qwen-code/releases) | 基于同一变更集的 Desktop 构建，包含 broker/connector 健壮性修复。 |
| [sdk-typescript-v0.1.20](https://github.com/QwenLM/qwen-code/releases) | 捆绑 CLI 0.25.2（从与 SDK 相同的 branch/ref 构建）。 |
| [v0.25.2-preview.1](https://github.com/QwenLM/qwen-code/releases) | 0.25.2 系列的预览版本。 |
| [v0.25.2-nightly.20261015.c24e99e9cb](https://github.com/QwenLM/qwen-code/releases) | 包含 `fix(managed-agent): admit team_plan_approval into the journal-only interrupted-turn settle`（[#13847](https://github.com/QwenLM/qwen-code/pull/13847)）。 |
| [v0.25.1-nightly.20261015.415925cbda](https://github.com/QwenLM/qwen-code/releases) | Connector/broker 健壮性切片；其发布工作流在 `quality` 作业上失败（[#14243](https://github.com/QwenLM/qwen-code/issues/14243)，已关闭）。 |

**主题：** 无破坏性变更；几乎所有发布内容都是 Hosted/managed-agent 可靠性。

## 3. 热门 Issue

1. **[#12380](https://github.com/QwenLM/qwen-code/issues/12380) — proposal(serve): Managed Agent 双路径架构与分阶段交付** *(68 条评论，进行中，P2)*
   tracker 中最大的单一讨论。提出保留 TypeScript agent loop，同时将模型推理与工具环境供给解耦，并赋予 Sessions 持久所有权、Workspace 绑定和可恢复的工具执行。实际上这是 `qwen serve` 中其他一切事项的架构路线图 issue；评论量反映的是持续的设计辩论，而非 bug 分诊。

2. **[#14282](https://github.com/QwenLM/qwen-code/issues/14282) — feat(artifacts): 当 agent 修改现有文件时记录并更新会话 artifacts** *(8 条评论)*
   目前，artifacts 仅在 `write_file` 创建带有允许列表扩展名的文件时注册；对现有文件的更新依赖模型调用 `record_artifact`。请求提供一致的创建与更新生命周期，以便可靠跟踪交付物。对任何基于会话 artifacts 构建的人来说都具有高杠杆作用。

3. **[#14234](https://github.com/QwenLM/qwen-code/issues/14234) — PR #14021 中延迟的评审发现：在 live turn 中结算从未启动的后台 dispatch** *(7 条评论)*
   明确从已合并修复中延迟的事项，已针对评审过的 head 确认，会改变失败路径的并发语义。说明此仓库中反复出现的模式：评审发现被推迟到受跟踪的后续事项中，而不是阻塞合并。

4. **[#13801](https://github.com/QwenLM/qwen-code/issues/13801) — `child_run` 卡在 `dispatch_started`，在进程可证明从未启动时永不结算** *(6 条评论，已关闭)*
   一个 `child_run` 会提交 `run.state: "running"` 且 `execution: "dispatch_started"`，同时不存在 `:process` 账本行和 cgroup 单元——因此公共 tasks API 报告一个从未存在的任务正在运行。这是可观察任务状态机中的正确性 bug；现已关闭。

5. **[#14063](https://github.com/QwenLM/qwen-code/issues/14063) — Hosted `session_command` 超时文本误导发送方；重复发送会复制消息** *(6 条评论)*
   超时证据文本告诉 `member_message` 发送方“去读 board”，而重试会复制消息。这既是面向模型的提示质量问题，也是多 agent 会话中真实存在的重复风险。

6. **[#13063](https://github.com/QwenLM/qwen-code/issues/13063) — 自治工具运行期间的事件驱动记忆召回** *(5 条评论，P3)*
   目前记忆召回仅在携带查询文本的轮次入口点触发，因此长时间自治工具运行在原始提示之后永远不会重新选择记忆。请求由 Advisor 触发的中途召回。这是一个核心的上下文/性能方向。

7. **[#14331](https://github.com/QwenLM/qwen-code/issues/14331) — 大量工具输出可能填满上下文窗口并导致运行失败，而不是优雅降级** *(4 条评论，P2)*
   当工具在接近上下文限制时返回大结果，运行会硬报错（“model stream ended after a tool result without visible progress”），用户失去该轮次。这是本周期用户可见程度最高的可靠性抱怨之一。

8. **[#14288](https://github.com/QwenLM/qwen-code/issues/14288) — 僵尸（defunct）会话子进程在长时间运行/挂起的会话下累积** *(4 条评论，P3)*
   在 Linux 上跨 0.22.2–0.25.1 观察到持续 48 天的僵尸进程，每天有 10–20 个并行会话。这是经典的长会话资源泄漏领域。

9. **[#14289](https://github.com/QwenLM/qwen-code/issues/14289) — feat(serve): 在可配置 TTL 后自动移除已完成的子会话** *(4 条评论)*
   通过 `create_sub_session` 创建的子会话会堆积在 Web Shell 侧边栏中，没有 UI 或 API 移除路径。与 PR [#14335](https://github.com/QwenLM/qwen-code/pull/14335) 直接配对。

10. **[#14314](https://github.com/QwenLM/qwen-code/issues/14314) — Grep 在运行搜索前拒绝有效的 ripgrep 内联标志** *(3 条评论，评审中)*
    验证使用 JavaScript 的 `RegExp` 解析模式，而 ripgrep 使用 Rust 的 regex 引擎执行它们，因此像 `(?i)hello` 和 `(?-i)TODO` 这样的有效模式会被拒绝。一个清晰、高信号的工具有正确性 bug，修复已在评审中。

*同时活跃：* [#14278](https://github.com/QwenLM/qwen-code/issues/14278) `/branch` 被不可见的后台请求阻塞（已关闭），[#14310](https://github.com/QwenLM/qwen-code/issues/14310) Python SDK 在 CLI 失败时丢弃缓冲消息，[#14312](https://github.com/QwenLM/qwen-code/issues/14312) web terminal 在大段粘贴后仍保持输入阻塞，[#14325](https://github.com/QwenLM/qwen-code/issues/14325) `/compress` 后出现错误的“interrupted request”横幅。

## 4. 关键 PR 进展

1. **[#13805](https://github.com/QwenLM/qwen-code/pull/13805) — feat(core): 根据经验信号门控 AutoSkill 评审**
   AutoSkill 评审现在根据*已接受*经验而不是原始工具量来获得资格（例如：≥5 次已接受完成调用，且同一工具有失败/恢复，或 ≥20 次且有实质性工作）。这是对自评审触发方式的有意义变更。

2. **[#13526](https://github.com/QwenLM/qwen-code/pull/13526) — feat(runtime): 添加常驻 Kubernetes Workspace 文件运行时**
   可选的 K8s 运行时，其中每个 tenant/Workspace 拥有一个常驻 Pod 和固定 CSI PVC；关闭 Session 会结算工作并撤销工具上下文，同时保留运行时绑定。这是 managed-agent 路线图中基础设施最重的角落。

3. **[#14319](https://github.com/QwenLM/qwen-code/pull/14319) — feat(managed-agent): 接入静态 Skills 与扩展配置**
   将静态 Skills 及扩展配置接入 managed sessions——位于 Session/mailbox 管道之上的能力层。

4. **[#14335](https://github.com/QwenLM/qwen-code/pull/14335) — feat(serve): 在可配置 TTL 后自动移除已完成的子会话**
   添加 `serve.subSessionCompletionTtlHours`（默认 24；`0` 保留今天的永久保留行为）。直接回应 [#14289](https://github.com/QwenLM/qwen-code/issues/14289)。

5. **[#14333](https://github.com/QwenLM/qwen-code/pull/14333) — test(managed-agent): FG9a/FG9f `session_message` 与 `session_command` 中继故障门**
   多进程故障注入门，在真实控制平面上证明两个跨 Session 中继域的 replay 稳定性。Stage F FG9 的切片 1（[#14164](https://github.com/QwenLM/qwen-code/issues/14164)）。

6. **[#14334](https://github.com/QwenLM/qwen-code/pull/14334) — test(managed-agent): FG8a/FG8b/FG8d monitor_run 与 background-shell 故障门**
   添加 lost-reply、store-failure 以及 cancellation/drain-release-split 门，扩展了来自 [#14120](https://github.com/QwenLM/qwen-code/pull/14120) 的 FG8 工作。

7. **[#13314](https://github.com/QwenLM/qwen-code/pull/13314) — fix(sdk-java): 关闭 #12654 的 Hosted Harness 评审 critical**
   落地对私有 Java 客户端合并后评审中 11 个 Critical 和 2 个 Minor 发现的修复，并关闭针对该 PR 本身提交的第二轮评审的关闭项。这是该仓库长尾评审债务工作流的显著例子。

8. **[#14302](https://github.com/QwenLM/qwen-code/pull/14302) — fix(providers): 验证 daemon 凭据并关闭 QwenCloud 后续事项**
   daemon 现在会在持久化 provider API key *之前*进行验证，以 `invalid_api_key` 拒绝错误计费系列的 key，而不是稍后失败。在 [#14073](https://github.com/QwenLM/qwen-code/pull/14073) 之上关闭一批 [#14257](https://github.com/QwenLM/qwen-code/issues/14257) 后续事项。

9. **[#13997](https://github.com/QwenLM/qwen-code/pull/13997) — fix(cli): 保持预运行时 `/acp` WebSocket 升级，而不是落到 406**
   快速路径 `qwen serve` 启动会在运行时应用存在之前接受 HTTP 连接；升级请求现在会等待，而不是返回 406。修复 [#13995](https://github.com/QwenLM/qwen-code/issues/13995) 中跟踪的 E2E 失败。

10. **[#14172](https://github.com/QwenLM/qwen-code/pull/14172) — feat(cli): 在 Linux 上用 Ctrl+V 粘贴剪贴板文本**
    `Ctrl+V` 现在在没有图像时读取剪贴板*文本*；此前 TUI 中唯一的剪贴板读取器是图像路径。小而立即能感受到的体验改进。

*同时推进：* [#14277](https://github.com/QwenLM/qwen-code/pull/14277) 合并 server/broker 源（已关闭），[#14306](https://github.com/QwenLM/qwen-code/pull/14306) 使 hosted prompts 与声明的工具对齐（已关闭），[#14056](https://github.com/QwenLM/qwen-code/pull/14056) 在崩溃后恢复 Hosted 仅 journal 工具批次，[#14033](https://github.com/QwenLM/qwen-code/pull/14033) 将 workspace-lock 重试预算放宽到最坏约 5 秒，[#14202](https://github.com/QwenLM/qwen-code/pull/14202) 消除 workflow-completion E2E 预算抖动（30s → 120s），[#13568](https://github.com/QwenLM/qwen-code/pull/13568) 将 LSP 文件查询路由到适用的服务器，[#14220](https://github.com/QwenLM/qwen-code/pull/14220) 移除移动端 composer 历史箭头按钮。

## 5. 热门讨论

本期未提供 Discussion 数据。*（按格式省略本节。）*

## 6. 功能请求趋势

- **Managed Agent / Hosted serve 成熟度。** 主导方向：双路径架构（[#12380](https://github.com/QwenLM/qwen-code/issues/12380)）、持久 Session 所有权、mailbox-over-Session-messages、Kubernetes workspace 运行时（[#13526](https://github.com/QwenLM/qwen-code/pull/13526)），以及静态 Skills 接线（[#14319](https://github.com/QwenLM/qwen-code/pull/14319)）。
- **会话生命周期卫生。** Artifact 创建与更新生命周期（[#14282](https://github.com/QwenLM/qwen-code/issues/14282)）、已完成子会话的 TTL 自动移除（[#14289](https://github.com/QwenLM/qwen-code/issues/14289)），以及正确的恢复横幅（[#14325](https://github.com/QwenLM/qwen-code/issues/14325)）。
- **带优雅降级的上下文与 token 管理。** 大量工具输出应降级而不是硬失败（[#14331](https://github.com/QwenLM/qwen-code/issues/14331)）；自治运行中途的事件驱动记忆召回（[#13063](https://github.com/QwenLM/qwen-code/issues/13063)）。
- **Web Shell 与终端对等。** `ui.customWittyPhrases` / `ui.accessibility.enableLoadingPhrases` 在浏览器中被忽略（[#14324](https://github.com/QwenLM/qwen-code/issues/14324)）；没有子会话管理 UI（[#14289](https://github.com/QwenLM/qwen-code/issues/14289)）。
- **跨语言 SDK 健壮性。** Python SDK 消息丢失和 `ProcessExitError` 掩盖（[#14310](https://github.com/QwenLM/qwen-code/issues/14310)）；Java Hosted Harness 评审 critical（[#13314](https://github.com/QwenLM/qwen-code/pull/13314)）；TypeScript SDK 现在与 CLI 一同版本化。
- **工具正确性。** ripgrep 原生模式验证（[#14314](https://github.com/QwenLM/qwen-code/issues/14314)）、LSP 查询路由（[#13568](https://github.com/QwenLM/qwen-code/pull/13568)），以及 Hosted prompts 与声明的工具对齐（[#14306](https://github.com/QwenLM/qwen-code/pull/14306)）。
- **平台打磨。** Linux 剪贴板文本粘贴（[#14172](https://github.com/QwenLM/qwen-code/pull/14172)）、VS Code companion bundle 卫生（[#14291](https://github.com/QwenLM/qwen-code/issues/14291)）。

## 7. 开发者痛点

1. **在期望优雅降级的地方硬失败。** [#14331](https://github.com/QwenLM/qwen-code/issues/14331)：接近上下文限制的大量工具结果会杀掉整个轮次，而不是摘要/截断。用户丢失工作并必须重启。
2. **不透明或误导性的错误状态。** [#14278](https://github.com/QwenLM/qwen-code/issues/14278) `/branch` 声称“a response or tool call is in progress”，却没有点名不可见的阻塞者（例如 auto-compact）；[#14063](https://github.com/QwenLM/qwen-code/issues/14063) 超时文本让发送方“去读 board”，并在重试时复制消息；[#14325](https://github.com/QwenLM/qwen-code/issues/14325) 成功 `/compress` 后出现错误的“interrupted request”横幅。
3. **长生命周期、多会话工作负载中的资源泄漏。** 僵尸子进程存活长达 48 天（[#14288](https://github.com/QwenLM/qwen-code/issues/14288)），以及侧边栏中子会话无上限累积直到 daemon 重启（[#14289](https://github.com/QwenLM/qwen-code/issues/14289)）——两者均由每天运行 10–20 个并行会话的用户报告。
4. **SDK/终端路径中的静默数据丢失。** 当 CLI 在异步迭代之间死亡时，缓冲消息被丢弃且 `ProcessExitError` 被隐藏（[#14310](https://github.com/QwenLM/qwen-code/issues/14310)）；web terminal 可能一直输入阻塞，因为 PTY 输出被当作输入确认（[#14312](https://github.com/QwenLM/qwen-code/issues/14312)）。
5. **CI 和发布不稳定。** 源源不断的 `qwen-code-dev-bot` main-branch 失败（[#12714](https://github.com/QwenLM/qwen-code/issues/12714)、[#13995](https://github.com/QwenLM/qwen-code/issues/13995)、[#14028](https://github.com/QwenLM/qwen-code/issues/14028)、[#14183](https://github.com/QwenLM/qwen-code/issues/14183)、[#14189](https://github.com/QwenLM/qwen-code/issues/14189)、[#14191](https://github.com/QwenLM/qwen-code/issues/14191)），外加一次失败的 nightly 发布（[#14243](https://github.com/QwenLM/qwen-code/issues/14243)）。若干修复是 deflake（例如 [#14202](https://github.com/QwenLM/qwen-code/pull/14202) 将回复预算从 30s 扩大到 120s），暗示与分片负载相关的时序脆弱性。
6. **评审债务跟踪开销。** 大型合并延迟记录（[#14234](https://github.com/QwenLM/qwen-code/issues/14234)、[#14257](https://github.com/QwenLM/qwen-code/issues/14257)、[#13901](https://github.com/QwenLM/qwen-code/issues/13901)）显示 PR 在评审轮次之前合并，留下正确性工作要在后续事项中追赶——对速度高效，但给 managed-agent 领域带来越来越重的维护税。

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*