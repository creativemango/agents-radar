# AI 基础设施日报 2026-10-13

> 生成时间: 2026-10-13 03:08 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# 跨项目对比报告 — AI 基础设施生态
**摘要窗口：2026-10-13** · 项目：vLLM、SGLang、llama.cpp、Ollama、LiteLLM、Unsloth

*计数说明：下文的“Issues”和“PRs”是**在 24 小时摘要中浮现的条目**，并非仓库总数。它们包括开放的 bug、已关闭项、功能请求和路线图，因此请将其视为活跃度指标，而非缺陷计数。*

---

## 1. 生态概览

数据中心服务层（vLLM、SGLang、LiteLLM）今天发布了**零个稳定版**，而本地/边缘层则快速推进——llama.cpp 在一个窗口内落地了 11 个构建，Ollama 和 Unsloth 均推送了测试版。精力集中在少数前沿 MoE 架构（DeepSeek-V4/4.1、GLM-5.3-Flash、Qwen3.x、Gemma4），而最重要的在途工程是多适配器/MoE LoRA 服务（SGLang 的 21 部分运行时栈）以及预填充-解码分离作为默认的横向扩展拓扑。主要风险主题是**推测解码**，它同时是 vLLM、SGLang 和 llama.cpp 中被优化最多和最不稳定的特性。硬件广度持续扩大（AMD gfx942/gfx950/MI355X、Intel XPU/Arc、Ascend NPU、Apple MLX、Hexagon、AMX），但后端正确性错误——专家 ID 重复、FP8 加载缺口、MLX 内核恐慌——是反复出现的代价。

---

## 2. 活动对比

| 项目 | 层级 | Issues（24小时） | PRs（24小时） | 发布状态 |
|---|---|---|---|---|
| **vLLM** | 数据中心服务引擎 | ~21 | ~14 | **无发布** — 最新发布列表为空 |
| **SGLang** | 数据中心服务引擎 | ~27 | ~20（包括 21 部分 LoRA 栈 #42880–#42888） | **无发布** |
| **llama.cpp** | 本地/边缘 GGUF 运行时（ggml） | ~19 | ~23 | **11 个构建：b11567–b11577** |
| **Ollama** | 本地运行时 + 云产品 | ~26 | ~17 | **v0.40.4-rc0** |
| **LiteLLM** | LLM 网关 / 代理 | ~24 | ~14 | **无发布** |
| **Unsloth** | 微调 + Studio 运行时 | ~29 | ~15 | **v0.1.906-beta** |

**解读表格：** Ollama 和 Unsloth 的问题数量较多，反映的是活跃的缺陷/回归积压，而非新特性推进速度。llama.cpp 的当日问题与 PR 比率（~19:23）表明其修复吞吐量最健康；SGLang 正在分阶段落地最大的单一特性体（LoRA 运行时）。

---

## 3. 模型支持竞赛

| 项目 | 新落地架构 / 模型 | 显著缺口（请求或损坏） |
|---|---|---|
| **vLLM** | **Clef-Flash** 通过 `/v1/systemone` 原生支持（Qwen3.5 骨干 + 联合模式头）；针对 DSv4 稀疏索引器的 DeepGEMM 26/09/30 升级；面向 SM120/SM121 路由专家的 NVFP4 每前向 W4A4/W4A16 | DeepSeek-V4-Flash 在 SM8x 上无法运行（#50576）；GLM-5.3-Flash 在 sm_89 上无稀疏 MLA（#54059）；Qwen3.8-Flash-Next-FP8 在 ROCm 上加载失败（#58688）；Gemma4 序列分类（#43726）；Qwen3.5-397B PD 分离（#43765） |
| **SGLang** | **GLM-5.3-Flash**（共置预填充-注意力 CP + 解码/验证 TP，原生 MTP，DFlash2）；**DeepSeek-V4.1** TP2 解码融合（融合全归约 + mHC 尾声，融合逆 RoPE + WO-A + MXFP8，SM100 交换 AB 注意力）；MiniCPM-V 4.7 指南；Marlin 上的 NVFP4 MoE LoRA；XPU 预填充图；NPU 预填充 CP | AMD DSV4.1 稀疏解码因上游 `aiter` API 移除而损坏（#43765，关闭且无修复路径） |
| **llama.cpp** | **vLLM d3 / "pplx-decider"**（b11572）和 **d2**（b11571），具有更严格的张量名称验证；当日多序列修复（b11577）；Hexagon Q1_0 HVX `MUL_MAT`；Intel Arc Pro B65；通过 NCCL 的多节点 TP；RPC `supports_op` 协商 | 在 `--split-mode tensor` 下失败的 MTP/DFlash2 草稿；Vulkan `MUL_MAT_ID` 行丢失（#29821） |
| **Ollama** | llama.cpp **b11521** → 决策模型 + **EmbeddingGemma 2**；CLI 决策模型支持；Prism 支持（PR 开放）；`clef` / `clef flash` 云；glm-ocr EOT 修复 | Prism 三值 GGUFs（`PQ2_0`/`PTQ1_0`）导入失败（#18521）；请求 K2 Horizon 系列（#18698） |
| **LiteLLM** | 无新模型/硬件集成；gpt-6 / Claude 5 桥接的**成本对等测试**；Gemini 上下文缓存创建令牌现已计费 | Bedrock 自动发现请求关闭未修复（#43568） |
| **Unsloth** | **Qwen-Image-2.1-Turbo**（8 步图像生成）；来自预量化 DiT 的 **LTX-2.5 fp8/int8**；文本扩散 LLM 训练；更快的 MoE GGUFs；推测解码 | Studio 中的 Ling 3.0（#8532）；Windows 上的 Gemma4e2b（#13388）；MLX 预训练结构（#8607） |

**谁领先：**
- **SGLang 在前沿 MoE 部署深度上领先**——它是唯一为 DeepSeek-V4.1 在 TP2 上交付生产解码融合以及完整 GLM-5.3-Flash 注意力拓扑的项目。
- **vLLM 拥有最广泛的模型*和*硬件矩阵**，加上今天唯一原生新架构落地（Clef-Flash）以及前向 NVFP4 精度切换。
- **llama.cpp 在本地服务方面拥有最快的架构吸纳速度**——在一个窗口内新增两个架构加上一个正确性修复。
- **Unsloth 拥有生成式多模态赛道**（图像/视频），本集合中没有服务引擎原生覆盖。

---

## 4. 性能前沿

优化工作分散在六个不同的战场：

**KV 缓存与前缀复用**——最薄弱的一环，也是代理工作负载冲击最严重的地方。
- vLLM：`HIT_PENDING` 没有截止时间，导致请求停滞直到客户端超时（#49829）；长预填充延迟模式（#61080）和自适应预填充预算 RFC（#52906）针对尾部延迟。
- SGLang：EAGLE 推测解码 KV 索引重建现在针对 90k–170k KV-token 区间并行化（#42803）——明确是长上下文胜利。关键在于，**两个代理式 KV 复用提案（#27574、#30928）均已停止活动**，使得 RadixAttention 的字节完全相同前缀匹配成为唯一机制。
- llama.cpp：新的 `ggml_backend_sched_set_realloc_callback`（b11567）使推理中缓冲区重新预留可观测——真正的 p99 可诊断性升级。
- Unsloth：每次工具调用后全上下文重新处理（#10698）——在约 30k token 时停滞数分钟。

**量化/低精度**——头条数字所在之处。
- vLLM：NVFP4 MoE 每前向 W4A16→W4A4 切换（#54617）；但 FlashInfer NVFP4 因果预填充在 SM120 上**比 FP8 慢 1.7–1.8 倍**（#50416）。
- SGLang：FP8 MoE LoRA Triton 供应商（#42885）、针对 **B200、GB300、H200** 调优的 CuTeDSL FP8 瓦片表（#42887）、在准备好的 Marlin 权重上的 NVFP4 MoE LoRA（#42888）。
- llama.cpp：在 AMX Q4_K 解码上**令牌生成 +13%**，输出位完全相同（#30429）——今天最干净的可采用胜利；针对 gfx906/MI50 的 Q2_K VGPR 溢出修复（#29910）。
- Unsloth：LTX-2.5 避免在加载时量化 38 GB bf16 transformer，降低峰值内存和启动成本。

**内核/融合**——丰富但狭窄。vLLM 的 mamba gather 融合（7 个 Triton 内核 → 1 个，约 6 倍）和 DFlash 填充（35.8 微秒/步）明确是**孤立内核微基准**，而非端到端吞吐量声明。SGLang 提出的 **monokernel 框架**（#43949，持久内核在 1–2 次启动中覆盖解码器层的大部分）是当前最雄心勃勃的结构性想法。

**分布式/并行服务**——SGLang 的 Q4 PD 分离路线图（#43699）将 PD 定位为主流部署形态；异构 TP KV 传输现在处理混合 SWA 模型（gpt-oss-120b，TP1 预填充 / TP4 解码，#40909）；预填充 CP 是 MHA/GQA 后端中最后一个未决项（#21788）。llama.cpp 增加了多节点 NCCL 张量并行（#28967）。

**批处理与调度**——vLLM `run-batch` 现在增量流式传输响应（前端内存，#52505）；SGLang 的 `bench_serving --request-rate` 逐渐偏离计划（#43930），这**扭曲了基准基线**。

**推测解码**——自成前沿，有真正的胜利（SGLang EAGLE 索引并行）和真正的危险（§6）。

---

## 5. 层级定位

| 层级 | 项目 | 角色与差异化因素 |
|---|---|---|
| **数据中心服务引擎** | **vLLM**、**SGLang** | GPU 优先、多 GPU/PD 感知、OpenAI 兼容 API。vLLM 在硬件/模型广度上竞争，并正在吸收网关关注点（可选 `fastapi-guard` 中间件，#60988）。SGLang 在前沿模型深度、长上下文/前缀密集型工作负载和 PD 分离上竞争，并进行最大的多适配器押注。两者现在都在交付以前属于网关层的功能（认证、速率限制、日志记录）。 |
| **本地/边缘运行时** | **llama.cpp**、**Ollama** | llama.cpp 是引擎 + ggml 后端层：GGUF、CPU/消费级 GPU/Apple/Hexagon、多后端。Ollama 是上层的产品化层——模型管理、CLI、桌面、云托管模型——并通过版本升级（b11521）显式跟踪 llama.cpp 上游。Ollama 现在通过云模型和工具调用向上扩展栈。 |
| **网关/代理** | **LiteLLM** | 不是推理引擎。拥有跨提供商协议转换、认证、预算、成本核算和 MCP 网关。其护城河是提供商覆盖广度和计费正确性——而非内核吞吐量。在边缘直接受到引擎原生中间件的威胁，但通过成本/可观测性/MCP 深度实现差异化。 |
| **训练/微调 + 本地应用运行时** | **Unsloth** | 微调、量化、GGUF 导出，以及 Studio 作为应用界面。独特地在一个栈中跨越 **训练 → 量化 → 服务 → 代理**，今天扩展到浏览器代理执行和图像/视频生成。将 llama.cpp 和 vLLM 作为后端嵌入，而非与之竞争。 |

**边界模糊是故事主线：** 引擎正在添加网关中间件，Ollama 正在添加云/代理功能，Unsloth 正在添加代理运行时，LiteLLM 正在添加 MCP 网关。集成表面——而非原始内核速度——是各层碰撞的地方。

---

## 6. 趋势信号

1. **DeepSeek-V4 / 前沿 MoE 是唯一的集成战场。** 每个服务项目都在那里花费其最高信号的周期，而 AMD 始终是最后被支持的平台（SM8x 缺口、gfx950 稀疏解码中断、MI355X 追踪器）。
2. **推测解码是生态系统中风险最高的生产特性。** 四个运行时中有三个将其报告为最大不稳定源：SGLang 在任何 `temperature > 0` 时杀死服务器的 SIGKILL（#43412）、GLM-5.3 + DFLASH 静默质量崩溃（#40843）；llama.cpp 在量化目标上的分歧（#25618）和多 GPU 拆分上的 PP 减半（#27428）；Unsloth 的草稿路径；vLLM 的 DFlash/DSpark 在接受前缀缓存下的崩溃（#47930）。
3. **多 LoRA / 多适配器服务是下一个解锁点——并且会引起动荡。** SGLang 的 21 部分栈（跨 Triton、CuTeDSL、Marlin 的 MoE LoRA，用于 FP8/NVFP4/BF16）是任何地方落地的最大特性。与此同时，vLLM 有一个未解决的 CUDA 图死锁，涉及融合 MoE LoRA + `--fully-sharded-loras`（#60845）。本季度计划评估，而非采用。
4. **代理工作负载已经超越了引擎 KV 能力。** 位置无关和程序化 KV 复用提案在 SGLang 中已死；引擎仍依赖字节完全相同的前缀匹配。今天的实际缓解措施是提示稳定性工程（固定前缀、有序工具模式）——加上对 llama.cpp 的 RAM 支持提示缓存在并发负载下可能**跨对话**泄露内容的认识（#27148），这对多租户服务器是一个数据隔离问题。
5. **多租户和计费正确性现在是基础设施关注点。** LiteLLM 的 v1.93.0 Redis 故障破坏了缓存*和*预算计数器（#34614），v1.92 缺失 `budget_fallbacks` 列导致 Admin UI 阻塞（#33074），并且代理管理员 JWT 之前绕过了个人 `max_budget`（#41226）。Unsloth 修补了聊天工具工作区中的符号链接跟随以及 HF 令牌覆盖错误。
6. **硬件多样化是真实的但脆弱的。** AMD gfx942/gfx950、Intel XPU/Arc B65、Ascend NPU 预填充 CP、Apple MLX、Hexagon HVX、Arm AMX 均有活动——然而 AMD DSV4.1 中断来自 **SGLang 无法控制的上游 `aiter` API 移除**，MLX SDPA 恐慌阻止了 Ollama 在 Apple Silicon 上运行。
7. **发布节奏正在分化。** 数据中心和网关层没有发布稳定版；本地/打包层发布了测试版和 11 个 llama.cpp 构建。固定并基准测试的纪律是强制性的：Ollama 据报告相比 v0.40.1 有 25–30% 吞吐量回归（#18968），而 LiteLLM 的公告使 v1.93.0/v1.92 成为不安全默认值。

**作为代理/应用开发者应关注什么：**
- 在扩展代理循环之前测量**每次工具调用的 TTFT**——Unsloth 的重新处理回归（#10698）和普遍的 KV 复用缺口都在这里浮出水面。
- **在您确切的安装方法上验证推测解码**（pip 包 vs. 容器 vs. 源码），使用采样流量，并在量化目标上与非推测基线进行 diff。
- 将**内核级基准视为仅方向性参考**——vLLM 的 6 倍 mamba 融合和 35.8 微秒 DFlash 节省是孤立测量；要求端到端数字。
- 如果您运营长上下文或尾部延迟敏感型集群，请跟踪 SGLang 的 PD 分离路线图（#43699）和 vLLM 的长预填充延迟模式（#61080）。
- 观察**引擎原生中间件**（vLLM 和 SGLang 中的 fastapi-guard）是否会侵蚀独立网关层——如果您正在标准化 LiteLLM 用于认证和速率限制而非成本/MCP/协议转换，则与此相关。

---

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 摘要 — 2026-10-13
来源：[vllm-project/vllm](https://github.com/vllm-project/vllm)

## 1. 今日亮点
过去 24 小时内没有新的 vLLM 版本发布。信号最强的活动围绕 DeepSeek-V4/DSv4.1：一个关于 SM8x/Ampere 支持的请求已有 109 条评论，一个 DSv4 CUDA-graph 压缩器修复，以及多份高并发稳定性报告。性能方面，一个 mamba align gather 融合声称在 kernel 层面带来 6 倍加速，同时新的 long-prefill defer 模式和可选的 API 安全中间件正在评审中。

## 2. 版本发布与破坏性变更
- **没有新版本发布。** 最新发布列表为空。
- **Mamba 配置弃用/清理：** [PR #60043](https://github.com/vllm-project/vllm/pull/60043) 移除了 `mamba_cache_mode="all"` 的残留内容，并弃用 `--mamba-cache-mode`；[Issue #60838](https://github.com/vllm-project/vllm/issues/60838) 报告称，在该移除之后，`--mamba-block-size` 在有效配置中不再生效。应重新验证 Mamba/混合缓存调优。
- **新增可选调度器标志：** [PR #61080](https://github.com/vllm-project/vllm/pull/61080) 添加了 `--long-prefill-token-threshold-defer`，默认关闭。
- **可选 API 中间件：** [PR #60988](https://github.com/vllm-project/vllm/pull/60988) 添加了可选的 `fastapi-guard` 安全中间件，用于进程内认证、限流和日志记录。
- 过去 24 小时内没有出现 API 破坏性变更。

## 3. 新模型与硬件支持
- **Clef-Flash：** [PR #61308](https://github.com/vllm-project/vllm/pull/61308) 通过 `/v1/systemone` 为 `Cloudflare/clef-flash` 添加原生支持，复用 Qwen3.5 骨干和联合 schema head。
- **Ampere 上的 DeepSeek-V4-Flash：** [Issue #50576](https://github.com/vllm-project/vllm/issues/50576) 请求为 DeepSeek-V4-Flash / 0731 DSpark 提供 SM8x 支持——A100/A800、RTX 30xx——目前在 SM8x 上无法运行。
- **面向 DSv4 的 DeepGEMM：** [PR #61149](https://github.com/vllm-project/vllm/pull/61149) 将 vendored DeepGEMM 升级到 26/09/30 版本，并适配稀疏 indexer。
- **Ada 上的 GLM-5.3-Flash：** [Issue #54059](https://github.com/vllm-project/vllm/issues/54059) 报告在 sm_89 / RTX 4090 上没有 sparse-MLA attention 路径。
- **Gemma4 分类：** [Issue #43726](https://github.com/vllm-project/vllm/issues/43726) 请求支持 `Gemma4ForSequenceClassification`。
- **混合 SSM/GDN PD 分离：** [Issue #43765](https://github.com/vllm-project/vllm/issues/43765) 请求为 Qwen3.5-397B-A17B-W8A8 提供 PD 分离 / KV 传输。
- **ROCm FP8 加载缺口：** [Issue #58688](https://github.com/vllm-project/vllm/issues/58688) 报告 Qwen3.8-Flash-Next-FP8 因缺少 FP8 `weight_scale` 而无法在 ROCm 上加载。
- **AMD MI355X 跟踪项：** [Issue #57149](https://github.com/vllm-project/vllm/issues/57149) 和 [Issue #59575](https://github.com/vllm-project/vllm/issues/59575) 跟踪针对 Qwen3.8-2.4T-A95B 和 Qwen3.8-Flash-Next 的 gfx950/MI355X 优化。
- **SM12x 上的 NVFP4：** [PR #54617](https://github.com/vllm-project/vllm/pull/54617) 为 routed experts 添加逐 forward 的 W4A4/W4A16 选择。

## 4. 性能与优化
- **Mamba gather 融合：** [PR #58737](https://github.com/vllm-project/vllm/pull/58737) 将 mamba align block-table gather 从 7 个 Triton kernel 融合为 1 个，在隔离 kernel 的所有 shape 上带来约 6 倍加速。范围说明：这是 kernel 级微基准测试，并非端到端吞吐量结论。
- **DFlash 推测解码 padding：** [PR #58633](https://github.com/vllm-project/vllm/pull/58633) 并行化 DFlash draft-input padding，从单个 workgroup 节省最多 35.8 µs/step。同样是 kernel 级。
- **长 prefill 延迟执行：** [PR #61080](https://github.com/vllm-project/vllm/pull/61080) 添加了一种模式：如果超过阈值，每步有一个长 prefill 在等待请求之后、利用剩余预算运行。选择启用。
- **NVFP4 MoE 精度切换：** [PR #54617](https://github.com/vllm-project/vllm/pull/54617) 在 SM120/SM121 上，对较小 token 数选择 W4A16，对较大 token 数选择 W4A4。
- **MoE router 融合：** [PR #57410](https://github.com/vllm-project/vllm/pull/57410) 融合 Transformers 后端中在 top-k 之后打分的 MoE router。
- **前端批处理流式输出：** [PR #52505](https://github.com/vllm-project/vllm/pull/52505) 使 `vllm run-batch` 增量写入响应，而不是缓冲到结束，从而降低前端内存占用。
- **已知性能缺口：** [Issue #50416](https://github.com/vllm-project/vllm/issues/50416) 报告在 SM120 上 FlashInfer NVFP4 KV-cache causal prefill 比 FP8 慢约 1.7–1.8 倍；[Issue #34635](https://github.com/vllm-project/vllm/issues/34635) 报告 `AllReduceFusionPass(config)` 构造需要 1 秒以上；[Issue #48229](https://github.com/vllm-project/vllm/issues/48229) 报告每个 generate batch 会出现一次偶发的 0.7–2.0 秒 V1 engine step 冻结。
- **活跃优化跟踪：** [Issue #57406](https://github.com/vllm-project/vllm/issues/57406) GLM-5.3，[Issue #57149](https://github.com/vllm-project/vllm/issues/57149) 和 [Issue #59575](https://github.com/vllm-project/vllm/issues/59575) AMD MI355X，[Issue #52906](https://github.com/vllm-project/vllm/issues/52906) 自适应 prefill token 预算 RFC。

## 5. 稳定性与回归
按严重程度排序：
1. **TP 挂起 / EngineDeadError** — [Issue #41530](https://github.com/vllm-project/vllm/issues/41530)：DeepSeek-V4-Pro、TP=8、MTP 推测解码下出现 RPC `sample_tokens` 超时。仍开放；过去 24 小时数据中没有修复 PR。
2. **fused-MoE LoRA 下的 CUDA-graphs 死锁** — [Issue #60845](https://github.com/vllm-project/vllm/issues/60845)：vLLM 0.28.0、Qwen3.5 兼容的 35B-A3B MoE、TP=2/4、`--fully-sharded-loras`。仍开放；未注明修复 PR。
3. **Triton 非法内存访问** — [Issue #56389](https://github.com/vllm-project/vllm/issues/56389)：H20 上高并发下的 DeepSeek-V4.1-Flash `dsv4_topk` MoE routing kernel。缓解措施：`max_num_seqs=256`。仍开放。
4. **ROCm 引擎停滞** — [Issue #57429](https://github.com/vllm-project/vllm/issues/57429)：GLM-5.3、TP8、gfx942 上的 v0.29.0，在 aiter fused MoE step 内停滞。仍开放。
5. **ROCm FP8 加载失败** — [Issue #58688](https://github.com/vllm-project/vllm/issues/58688)：Qwen3.8-Flash-Next-FP8 缺少 FP8 `weight_scale`。仍开放。
6. **DSv4 CUDA-graph logprob 漂移** — [PR #59709](https://github.com/vllm-project/vllm/pull/59709)：固化进 CUDA graphs 的 C128 compressor skip 导致 decode logprobs 漂移；修复 PR 开放中。
7. **推测解码接受率崩塌** — [Issue #47930](https://github.com/vllm-project/vllm/issues/47930)：启用自动前缀缓存后，DFlash/DSpark draft 接受率崩塌。仍开放。
8. **KV offload 超时风险** — [Issue #49829](https://github.com/vllm-project/vllm/issues/49829)：`HIT_PENDING` 没有截止时间；停滞的写入可能将请求推迟到客户端超时。仍开放。
9. **Mamba block-size 无效** — [Issue #60838](https://github.com/vllm-project/vllm/issues/60838)：移除 `mamba_cache_mode="all"` 后，`--mamba-block-size` 不再生效。仍开放。
10. **MiniCPM-V-4.6 加载失败** — [Issue #48449](https://github.com/vllm-project/vllm/issues/48449)：vLLM 0.25.0 中 `MiniCPMV4_6ViTWindowAttentionSelfAttn` 缺少 `qkqkv_proj`。仍开放。
11. **结构化输出 / 工具调用** — [Issue #55552](https://github.com/vllm-project/vllm/issues/55552)：已关闭；在 Qwen3.8-Flash-Next 下 `tool_choice="required"` 未被强制执行，并出现 xgrammar FSM 错误。如受影响请验证修复。
12. **待处理 bugfix PR：** [PR #61272](https://github.com/vllm-project/vllm/pull/61272) tokenizer `code_revision`；[PR #57797](https://github.com/vllm-project/vllm/pull/57797) layerwise reload 上的 RL attention sinks；[PR #58524](https://github.com/vllm-project/vllm/pull/58524) off-grid CUDA graph capture sizes。

## 6. 对应用开发者的意义
- **今天没有新版本可供采用。** 固定使用已知良好的版本；如果使用 `--mamba-block-size` 或 `--mamba-cache-mode`，请重新测试 Mamba/混合配置。
- **H20 上的 DeepSeek-V4/DSv4.1：** 对于 DeepSeek-V4.1-Flash 高并发场景，将 `max_num_seqs=256` 作为上限，以避免 #56389 中的 Triton 非法内存访问。如果使用 TP=8 和 MTP 推测解码的 DeepSeek-V4-Pro，请留意 #41530 中的 TP 挂起。
- **MoE + LoRA + TP：** 在 #60845 解决之前，避免在并发流量下将 `--fully-sharded-loras` 与 CUDA graphs 一起使用。
- **长上下文服务：** 新的 long-prefill defer 模式（#61080）可能有助于尾延迟，但默认关闭；自适应 prefill 预算仍是一个 RFC（#52906）。
- **API 部署：** 如果缺少用于认证、限流和日志记录的反向代理，可以考虑可选的 fastapi-guard 中间件（#60988）。
- **批处理工作负载：** `vllm run-batch` 现在按输入顺序流式返回响应，减少大批量场景下的前端内存占用（#52505）。
- **AMD/ROCm：** MI355X 优化活跃，但 Qwen3.8-Flash-Next-FP8 在 ROCm 上仍被 #58688 阻塞。
- **Kernel 改进范围有限：** #58737 和 #58633 是孤立的 kernel 优化；在假定吞吐量提升之前，请先进行端到端验证。

---

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 摘要 — 2026-10-13

## 今日亮点
DeepSeek V4 路线图已收官（[#23602](https://github.com/sgl-project/sglang/issues/23602)，75 条评论），W4A16 Hopper 支持与 Marlin 后续事项均已完成；与此同时，Q4 Prefill-Decode 分离路线图已发布（[#43699](https://github.com/sgl-project/sglang/issues/43699)），将 PD 定位为默认的横向扩展路径。当前规模最大的进行中工作是 21 部分的 LoRA runtime stack，正在向 Triton、CuTeDSL 和 Marlin 各 vendor 落地 MoE LoRA 支持（[#42880](https://github.com/sgl-project/sglang/pull/42880)–[#42888](https://github.com/sgl-project/sglang/pull/42888)）。风险方面，speculative decoding 在非 greedy 请求上可复现地导致服务器进程被杀死的崩溃（[#43412](https://github.com/sgl-project/sglang/issues/43412)）仍处于 open 状态。

## 发布与破坏性变更
过去 24 小时内无新版本发布。值得注意的兼容性事项：

- **[#43765](https://github.com/sgl-project/sglang/issues/43765) [CLOSED] — AMD DSV4.1 稀疏 decode 因外部依赖移除而损坏。** `compact_attention_hip` 导入了 `aiter` 的 `pa_decode_sparse`，而 aiter 已于 2026-09-18 移除该接口；升级 aiter *并不能* 修复。影响近期 main 上的 gfx950 用户。
- **[#30781](https://github.com/sgl-project/sglang/issues/30781) [CLOSED] — gateway/router 协议漂移。** `sgl-model-gateway` v0.3.2 拒绝了 `type: "custom"` 的 `/v1/responses` tools，破坏了 OpenAI Codex CLI 兼容性；根因是 Rust `openai-protocol` crate 落后于 Python `protocol.py`。
- **[#33385](https://github.com/sgl-project/sglang/issues/33385) [CLOSED] — offload 行为未受 flag 门控。** Decode 模式 retract 因 `NotImplementedError` 崩溃，原因是 KV offload 无条件执行，而非由 `--disaggregation-decode-enable-offload-kvcache` 门控。
- **[#43610](https://github.com/sgl-project/sglang/pull/43610) — 可选新依赖。** 为没有 gateway 的部署新增 `fastapi-guard` 中间件（速率限制、IP 控制、载荷筛查）；状态是 per-worker 的，因此多 worker 场景推荐使用 Redis。
- **[#42085](https://github.com/sgl-project/sglang/issues/42085) [OPEN] — 被静默忽略的配置。** `--bf16-gemm-backend gemv` 会被接受，但从未被未量化的线性层使用。

## 新模型与硬件支持
- **GLM-5.3-Flash** — 同址部署的 prefill attention CP + decode/target-verify attention TP，原生支持 MTP 和 DFlash2（[#43972](https://github.com/sgl-project/sglang/pull/43972)，draft）。
- **DeepSeek-V4.1** — TP2 decode 路径现在可以使用此前仅限 TP4 的小批量 decode 融合（融合 all-reduce + mHC epilogues、融合 inverse-RoPE + WO-A + MXFP8、SM100 swap-AB decode attention）（[#43838](https://github.com/sgl-project/sglang/pull/43838)）。V4.1 优化另有跟踪：[#42170](https://github.com/sgl-project/sglang/issues/42170)。
- **MiniCPM-V 4.7** — 在模型支持合并前先添加了 cookbook（[#43829](https://github.com/sgl-project/sglang/pull/43829)）。
- **Marlin 上的 NVFP4 MoE LoRA** — 基于已准备的 Marlin 权重的新 provider（[#42888](https://github.com/sgl-project/sglang/pull/42888)）。
- **XPU** — 完整的 prefill graph 后端，实现与 decode 对等的 capture/replay（[#30980](https://github.com/sgl-project/sglang/pull/30980)）。
- **NPU** — 面向通用（非 DSA）Ascend attention 后端的 prefill CP 修复，覆盖 Qwen3/Qwen3-MoE（[#41564](https://github.com/sgl-project/sglang/pull/41564)）；mamba checkpoint-pool 拷贝的 conv layout 对齐（[#43845](https://github.com/sgl-project/sglang/pull/43845)）；现已支持 `prefill_complete`（[#43970](https://github.com/sgl-project/sglang/pull/43970)）。
- **异构 TP** — PD KV 传输现在可以处理混合 SWA 模型（例如 `gpt-oss-120b`，prefill TP1 / decode TP4）（[#40909](https://github.com/sgl-project/sglang/pull/40909)）。

## 性能与优化
- **EAGLE spec-decode KV 索引构建已并行化**，面向 flashinfer draft decode 和 verify/draft-extend（flashinfer、flashinfer_mla、aiter）按 token block 并行。目标是 90k–170k KV token 的长上下文场景，此前索引重建在每个请求内是串行的（[#42803](https://github.com/sgl-project/sglang/pull/42803)）。
- **启动调优现在包含 FlashInfer 的默认 tactic（`-1`）**，避免选中对 profiling 输入而言比默认更慢的显式调优 tactic（[#43927](https://github.com/sgl-project/sglang/pull/43927)）。
- **LoRA runtime stack（第 13–21 部分）** — 带 FP8 行量化和 masked/contiguous 行域的 MoE LoRA stage kernels（[#42880](https://github.com/sgl-project/sglang/pull/42880)）；面向 SM90（WGMMA）和 SM100（tcgen05）的 CuTeDSL BF16 grouped GEMM（[#42884](https://github.com/sgl-project/sglang/pull/42884)）；packed MoE schedules（[#42883](https://github.com/sgl-project/sglang/pull/42883)）；FP8 MoE LoRA Triton vendor（[#42885](https://github.com/sgl-project/sglang/pull/42885)）；为 **B200、GB300 和 H200** 调优的 CuTeDSL FP8 tile tables（[#42887](https://github.com/sgl-project/sglang/pull/42887)）。
- **进行中的路线图：** 剩余 MHA/GQA 后端（flashinfer/trtllm-mha）的 Prefill Context Parallelism 是 [#21788](https://github.com/sgl-project/sglang/issues/21788) 中最后一个未完成事项；diffusion Q4 以 MiniMax-H3 为目标，包含通信/计算重叠以及已验证的 GB200/GB300 配方（[#43313](https://github.com/sgl-project/sglang/issues/43313)）；block-diffusion（dLLM）服务路线图在 [#39499](https://github.com/sgl-project/sglang/issues/39499) 中继续推进。
- **提议：** 一个 monokernel 框架（persistent kernels，在一次或两次 launch 中覆盖一个 decoder 层的大部分），包含按模型划分的规格和 CI 门禁（[#43949](https://github.com/sgl-project/sglang/issues/43949)）。

## 稳定性与回归
按严重程度排序：

1. **[#43412](https://github.com/sgl-project/sglang/issues/43412) [OPEN] — spec decoding 在首个 sampled 请求上 SIGKILL 服务器进程树。** Greedy（temp 0）可用；任何 `temperature > 0` 都会挂掉。触发原因是当 pip 安装包自带的 `cccl` 与 pip cu13 toolkit 冲突时，flashinfer verify-sampling JIT 失败。未引用修复 PR——**在仅 pip 安装且有 sampled 流量的环境中避免启用 speculative decoding。**
2. **[#43557](https://github.com/sgl-project/sglang/issues/43557) [OPEN] — PD runtime 角色切换因非法内存访问而崩溃**，发生在 decode → prefill → decode 之后，环境为 sglang 0.5.21、mooncake 后端、TP1、`--enable-pd-role-switch`（`Qwen/Qwen3.5-0.8B`）。
3. **[#40843](https://github.com/sgl-project/sglang/issues/40843) [OPEN] — GLM-5.3 + DFLASH spec decoding 产生严重重复和退化循环**，即正确性失败，而不仅仅是崩溃。
4. **[#43526](https://github.com/sgl-project/sglang/issues/43526) [OPEN] — gfx942 block-FP8 linear AITER CK blockscale GEMM 的精度问题**（AMD）。
5. **[#43765](https://github.com/sgl-project/sglang/issues/43765) [CLOSED] — gfx950 上的 DSV4.1 稀疏 decode 因 `ModuleNotFoundError` 失败**，原因是上游 aiter API 移除。
6. **[#42085](https://github.com/sgl-project/sglang/issues/42085) [OPEN] — GEMV BF16 GEMM 后端 flag 静默 no-op**，因此用户可能基准测试的是一条实际从未走到的路径。
7. **[#43709](https://github.com/sgl-project/sglang/issues/43709) [OPEN] — transcription multipart upload 没有应用层大小限制**（在接受上传的端点上存在资源耗尽暴露面）。
8. **[#43930](https://github.com/sgl-project/sglang/issues/43930) [OPEN] — `bench_serving --request-rate` 逐渐落后于计划**，因此实际达到的请求速率低于请求速率，并使基准测试结果产生偏差。
9. **[#42752](https://github.com/sgl-project/sglang/issues/42752) [OPEN] — CI flaky-test / 基础设施跟踪帖**（49 条评论），监控 NVIDIA `PR Test Base` 和 `PR Test Extra`；补充了更早的 CI 跟踪帖 #17050。预计 CI 门控 PR 的审查延迟会升高。
10. 因不活跃而关闭的正确性/健壮性 bug 在更新窗口内重新浮现：[#29998](https://github.com/sgl-project/sglang/issues/29998)（`runai_streamer` 在 TP8 下静默损坏 GLM-5.2）、[#34384](https://github.com/sgl-project/sglang/issues/34384)（DSpark ragged CUDA Graph geometry）、[#34399](https://github.com/sgl-project/sglang/issues/34399)（paged KV allocator 在 OOM 检查之前就启动 kernels）、[#26618](https://github.com/sgl-project/sglang/issues/26618)（radix-cache prefix 命中后 FlashInfer MIS delimiter indices）、[#31019](https://github.com/sgl-project/sglang/issues/31019)（GPT-OSS Harmony 在 `require_reasoning` + `json_schema` 下输出格式错误）、[#31766](https://github.com/sgl-project/sglang/issues/31766)（prefill 侧 fd 耗尽）。

## 对应用开发者的意义
- **多租户 / 多适配器服务是即将到来的重大解锁。** 21 部分的 LoRA stack 覆盖面向前沿服务形态（FP8、NVFP4、BF16）的 MoE LoRA，并为 Hopper/Blackwell 提供厂商特定 GEMM。如果你为每个基础模型服务大量 fine-tune，计划在该 stack 合并后评估——但这是一次大型、分阶段落地，预计会有变动。
- **面向 agent 的 KV 复用仍未在引擎层面解决。** 两个 agent 导向的提案——Programmatic KV Cache（[#27574](https://github.com/sgl-project/sglang/issues/27574)，现已关闭）和 Position-Independent KV Cache Reuse（[#30928](https://github.com/sgl-project/sglang/issues/30928)，已关闭）——都转为不活跃。具有共享 system prompts、tool schemas 以及不同偏移检索文档的 Agent/RAG 工作负载，仍依赖 RadixAttention 的字节级相同前缀匹配；请自行做 prompt 稳定性工程（固定前缀、有序 tool schemas）以最大化缓存命中。
- **目前在生产环境中对 speculative decoding 要保守。** 在 sampled 流量下它可能拖垮整个服务器（[#43412](https://github.com/sgl-project/sglang/issues/43412)），而在 GLM-5.3 + DFLASH 下它会静默降低输出质量（[#40843](https://github.com/sgl-project/sglang/issues/40843)）。启用前请在你确切的安装方式（pip bundle vs. container）上验证。
- **如果你运行带角色切换的 PD disaggregation，请固定到已知良好版本**，并演练 decode → prefill → decode 转换（[#43557](https://github.com/sgl-project/sglang/issues/43557)）。Q4 路线图（[#43699](https://github.com/sgl-project/sglang/issues/43699)）表明该拓扑现已成为主流部署形态，因此修复应该会很快落地。
- **依赖非标准 OpenAI tool 类型的 gateway 用户**应确保其 router 使用已同步 protocol crate 的构建（[#30781](https://github.com/sgl-project/sglang/issues/30781)）；否则 Codex-CLI 风格客户端将收到请求拒绝。
- **硬件覆盖持续扩大**——NPU prefill CP、XPU prefill graphs 和 AMD gfx942/gfx950 今天都有活动，但 AMD 稀疏 decode 路径存在 SGLang 无法控制的外部依赖风险（[#43765](https://github.com/sgl-project/sglang/issues/43765)）。

---

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-10-13

## 1. Today's Highlights

A rapid-fire release window (b11567–b11577) landed new model support for the vLLM "d2" and "d3" architectures plus a follow-up multi-sequence correctness fix, alongside a graph-level fix padding no-cache attention KV to 256 for head size 512 on CUDA. Backend correctness was the dominant theme: Metal's `mul_mat_id` was fixed for duplicate expert IDs, while Vulkan's equivalent bug (#29821) remains open. Performance work continues on several fronts, with a reported **+13% token generation** on AMX CPUs for Q4_K decode.

## 2. Releases & Breaking Changes

- **b11567 — new scheduling API**: `ggml_backend_sched_realloc_callback` / `ggml_backend_sched_set_realloc_callback` lets applications observe when graph buffers are re-reserved mid-inference. Additive, but the first step toward surfacing reallocation-driven latency spikes. ([PR #30329](https://github.com/ggml-org/llama.cpp/pull/30329))
- **b11570 — build system change**: `llama_build()` / `llama_build_and_test()` now accept a `LINK` keyword (default `llama-common`); ggml-only tests link only `ggml`. Downstream CMake consumers that relied on implicit `llama-common` linkage for their own test targets should audit their call sites. ([PR #30299](https://github.com/ggml-org/llama.cpp/pull/30299))
- **CI/release packaging**: OpenVINO release binaries now build with `GGML_NATIVE=OFF`, `GGML_BACKEND_DL=ON`, and `GGML_CPU_ALL_VARIANTS=ON`, fixing AVX-512 `STATUS_ILLEGAL_INSTRUCTION` crashes on non-AVX-512 hosts. Broken OpenVINO download links in release notes were also repaired. ([PR #30324](https://github.com/ggml-org/llama.cpp/pull/30324), [PR #30401](https://github.com/ggml-org/llama.cpp/pull/30401))

## 3. New Model & Hardware Support

- **vLLM d3 / "pplx-decider" models** (b11572, [PR #30383](https://github.com/ggml-org/llama.cpp/pull/30383)) and **d2** (b11571, [PR #30378](https://github.com/ggml-org/llama.cpp/pull/30378)) — new architecture support with stricter tensor-name validation and documented naming rules; b11577 fixes a non-causal + hybrid + multi-sequence graph case ([PR #30408](https://github.com/ggml-org/llama.cpp/pull/30408)).
- **Hexagon Q1_0** — native HVX `MUL_MAT` for Q1_0, direct `{-1,+1}` expansion against tiled Q8_0 activations, plus 2D/4D/matvec workers ([PR #30122](https://github.com/ggml-org/llama.cpp/pull/30122)).
- **Intel Arc Pro B65 (Vulkan)** — shader core count added; the device reports as BMG G31, same die as B70 with cores disabled ([PR #30431](https://github.com/ggml-org/llama.cpp/pull/30431)).
- **Multi-node tensor parallelism via NCCL** — new backend collective interface implemented in CUDA and wired into the RPC server; stacked on prior multi-GPU work ([PR #28967](https://github.com/ggml-org/llama.cpp/pull/28967)).
- **Hexagon HTP power profiles** — `GGML_HEXAGON_POWER_MODE` selects one of eight DCVS/HMX profiles, with `burst` preserved as default ([PR #30423](https://github.com/ggml-org/llama.cpp/pull/30423)).
- **RPC capability negotiation** — RPC client now queries the remote backend's `supports_op` instead of assuming universal op support ([PR #28571](https://github.com/ggml-org/llama.cpp/pull/28571)).

## 4. Performance & Optimization

- **AMX Q4_K decode: +13% tg** — M=1 VNNI kernel now walks each Q4_K group across output columns and broadcasts Q8 activation dwords directly, rather than column-by-column. Numerically identical output order ([PR #30429](https://github.com/ggml-org/llama.cpp/pull/30429)).
- **Vulkan f16 accumulation packing** — packed FMA path for f16 accumulation without a dot-product op; `dot_product` variant packs two rows per f16 FMA and was renamed to `dot_product_v2` (b11573, [PR #29877](https://github.com/ggml-org/llama.cpp/pull/29877)).
- **Vulkan RMS norm** — workgroup reduction replaced with two-stage subgroup reductions; benchmarked on B70 Arc Pro, RTX 4060 Ti, and 7900 XT ([PR #29882](https://github.com/ggml-org/llama.cpp/pull/29882)).
- **Hexagon CPY/CONCAT round 3** — converting copies moved into per-DMA-descriptor row blocks so short rows are no longer DMA-latency bound; adds a threaded DMA/HVX gather-row helper (b11574, [PR #30341](https://github.com/ggml-org/llama.cpp/pull/30341)).
- **Q2_K VGPR spill fix (CUDA/AMD)** — softer unroll and removal of an unnecessary temporary loop to eliminate heavy mmq register spilling on gfx906/MI50 ([PR #29910](https://github.com/ggml-org/llama.cpp/pull/29910)).
- **Logits buffer allocation skipped** when the graph produces no logits — saves ~1 MiB host RAM per output for large-vocab embedding/reranking models (bge-m3: 250002, EmbeddingGemma: 262144) ([PR #30255](https://github.com/ggml-org/llama.cpp/pull/30255)).
- **No-cache attention graph fix** (b11576): KV dimension padded to 256 so `FLASH_ATTN_EXT` on CUDA satisfies the multiple-of-`FATTN_KQ_STRIDE` requirement for head size 512 ([PR #30416](https://github.com/ggml-org/llama.cpp/pull/30416)).

## 5. Stability & Regressions

**High severity**
- **Server RAM-backed prompt cache cross-conversation leakage** — `server_prompt_cache` (`--cache-ram`, default enabled) can restore content from an unrelated finished conversation into a fresh slot under concurrent load. This is a correctness *and* data-isolation issue for multi-tenant servers. No fix PR yet. ([Issue #27148](https://github.com/ggml-org/llama.cpp/issues/27148))
- **CUDA graphs hang GPU channel** (RC watchdog + Xid 8) on RTX 5090 Laptop / sm_120; `GGML_CUDA_DISABLE_GRAPHS=1` is a complete workaround. ([Issue #27330](https://github.com/ggml-org/llama.cpp/issues/27330))

**Correctness**
- **Speculative decoding divergence on quantized targets** — greedy (`temp=0`, `top_k=1`) draft-mtp/draft-dspark can diverge from vanilla output with Q4_K_M targets while bf16 targets match. ([Issue #25618](https://github.com/ggml-org/llama.cpp/issues/25618))
- **Vulkan `MUL_MAT_ID` row loss** — duplicate expert IDs in a token's row produce partial/dropped output when `n > 8`, due to group-counting mismatch between slot counting and `count_experts`. Open. ([Issue #29821](https://github.com/ggml-org/llama.cpp/issues/29821))
- **Metal `mul_mat_id` duplicate IDs** — same class of bug; **fixed** in b11569, which also corrected kernel naming and the previous one-match-per-token / `ne21`-capacity assumption. ([PR #30373](https://github.com/ggml-org/llama.cpp/pull/30373))
- **Nondeterministic FA prefill** with Qwen 3.5 9B on RDNA3 (ROCm/HIP) for multimodal requests since b10905. ([Issue #30175](https://github.com/ggml-org/llama.cpp/issues/30175))
- **DFlash2 / MTP draft failures with tensor split** — assertion failures and FA head-dim mismatches across CUDA/SYCL/Vulkan when combining speculative decoding with `--split-mode tensor`. Multiple reports; largely unresolved. ([Issue #27819](https://github.com/ggml-org/llama.cpp/issues/27819), [Issue #24440](https://github.com/ggml-org/llama.cpp/issues/24440), [Issue #29418](https://github.com/ggml-org/llama.cpp/issues/29418), [Issue #29419](https://github.com/ggml-org/llama.cpp/issues/29419))
- **Server `batch_view` offset not propagated to `ctx_dft`** — subtler batch-retry path in `update_slots()` can mis-address the draft context. ([Issue #24840](https://github.com/ggml-org/llama.cpp/issues/24840))

**Performance regressions / backend stability**
- **draft-mtp halves prompt processing** on multi-GPU layer split (single GPU unaffected). ([Issue #27428](https://github.com/ggml-org/llama.cpp/issues/27428))
- **Glimmer: memory growth + prefill slowdown after first `mmproj` use.** ([Issue #26873](https://github.com/ggml-org/llama.cpp/issues/26873))
- **Vulkan `ErrorDeviceLost`** on 7900 XTX during decode. ([Issue #30010](https://github.com/ggml-org/llama.cpp/issues/30010))
- **OpenVINO**: `Cannot get length of dynamic dimension` when `-fa off` (V cache transposed past FA-layout assumptions) — **fix PR open** ([PR #30350](https://github.com/ggml-org/llama.cpp/pull/30350)); separate i5-1345u exception still open ([Issue #27546](https://github.com/ggml-org/llama.cpp/issues/27546)).
- **HIP gfx1151 (Strix Halo, Windows)**: `SOLVE_TRI` aborts with rocBLAS `hipErrorInvalidImage`, affecting unfused gated-delta-net paths (Qwen3.5 / Kimi-linear). ([Issue #27557](https://github.com/ggml-org/llama.cpp/issues/27557))
- **ggml-rpc SIGABRT on unreachable endpoint** — startup failures now fail gracefully rather than killing the process ([PR #30415](https://github.com/ggml-org/llama.cpp/pull/30415)); SYCL `get_rows` on reordered quantized weights fixed ([PR #30430](https://github.com/ggml-org/llama.cpp/pull/30430)).

**Resolved/closed**
- KV cache save via `/slots/N?action=save` for vision models ([#19466](https://github.com/ggml-org/llama.cpp/issues/19466)); Qwen3-Coder tool-call parser ([#15012](https://github.com/ggml-org/llama.cpp/issues/15012)); Metal tensor API silently inert on M5+/A19+ due to unset `MTLCompileOptions.languageVersion` ([#27473](https://github.com/ggml-org/llama.cpp/issues/27473)); Vulkan Q8_0 PP regression after #25773 ([#30000](https://github.com/ggml-org/llama.cpp/issues/30000)).

## 6. What This Means for Application Developers

- **New model coverage is expanding fast but sequence-handling edge cases lag.** The vLLM d2/d3 additions landed with a same-day multi-sequence fix (b11577). If you serve these models, validate non-causal + hybrid attention configurations with batched, multi-slot requests before production.
- **Multi-tenant server deployments need attention.** The prompt-cache leak (#27148) is the most consequential open item for anyone running `llama-server` with `--cache-ram` defaults under concurrency — it can surface content from an unrelated conversation. Consider disabling cross-slot caching until patched.
- **Speculative decoding remains the least stable surface.** Divergence on quantized targets, PP halving on multi-GPU splits, and tensor-split crashes with MTP drafts all remain open. Avoid combining `--split-mode tensor` with draft/MTP speculation on quantized targets in production, and validate greedy outputs against a non-speculative baseline.
- **Duplicate-expert-ID handling is a recurring backend bug class.** Metal is fixed; Vulkan is not. MoE workloads with skewed router distributions should pin to a build ≥ b11569 on Metal, and treat Vulkan MoE output as suspect until #29821 closes.
- **Observability for latency spikes improves.** The new `ggml_backend_sched_set_realloc_callback` (b11567) lets you detect when KV/graph buffers are re-reserved mid-inference — useful for correlating p99 latency jumps with memory pressure rather than attributing them to the model.
- **CPU throughput on AMX servers improves measurably.** The Q4_K decode path (+13% tg) is worth adopting for CPU-only or CPU-offload decode tiers, with bit-identical outputs easing rollout.
- **Aligned build flags matter for shipped binaries.** The OpenVINO AVX-512 crash reinforces checking `GGML_NATIVE=OFF` in any distribution pipeline — the same class of failure applies to self-built binaries moved between hosts.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 摘要 — 2026-10-13

## 1. 今日亮点
Ollama 发布了 `v0.40.4-rc0`，将 llama.cpp 更新至 b11521，以支持决策模型和 EmbeddingGemma 2，并在推理路由中复用已验证的 manifests/已加载模型（[PR #18917](https://github.com/ollama/ollama/pull/18917)、[PR #18947](https://github.com/ollama/ollama/pull/18947)）。最严重的活跃信号是据报告所有模型相较 v0.40.1 出现 25–30% 吞吐量回归（[Issue #18968](https://github.com/ollama/ollama/issues/18968)），以及 0.40.x 中 Apple Silicon 上多起 MLX SDPA 内核 panic（[Issue #18966](https://github.com/ollama/ollama/issues/18966)、[Issue #18940](https://github.com/ollama/ollama/issues/18940)）。工具支持工作仍在继续：一个开放 PR 将允许在结构化输出中使用工具调用，解决当前 `format: json` 工具调用损坏的问题（[Issue #18944](https://github.com/ollama/ollama/issues/18944)、[PR #18975](https://github.com/ollama/ollama/pull/18975)）。

## 2. 版本发布与破坏性变更
- **v0.40.4-rc0** — 面向 v0.40.4 的 RC。包含用于决策模型和 EmbeddingGemma 2 的 llama.cpp b11521，以及服务器在推理路由中复用已验证的 manifests 和已加载模型（[发布](https://github.com/ollama/ollama/releases/tag/v0.40.4-rc0)、[PR #18917](https://github.com/ollama/ollama/pull/18917)、[PR #18947](https://github.com/ollama/ollama/pull/18947)）。发布说明被截断；MLX 变更无法完全看到。
- **提供的数据中未明确指出破坏性 API/配置变更**。从运维角度，注意当请求 `shift` 与已加载 runner 不同时调度器的重载行为（[Issue #18950](https://github.com/ollama/ollama/issues/18950)）。
- **文档清理**：在 [PR #18970](https://github.com/ollama/ollama/pull/18970) 中移除了过时的 `OLLAMA_TMPDIR` 和 `%TEMP%` 引用；在 [PR #18960](https://github.com/ollama/ollama/pull/18960) 中回退了文档重构。

## 3. 新模型与硬件支持
- **决策模型 + EmbeddingGemma 2**：llama.cpp 升级至 b11521；在 [PR #18962](https://github.com/ollama/ollama/pull/18962) 中添加了 CLI 决策模型支持；在 [PR #18974](https://github.com/ollama/ollama/pull/18974) 中修复了 MLX 决策模型 JSON 状态提示。
- **Prism 模型**：开放 PR 添加 Prism 模型支持（[PR #18967](https://github.com/ollama/ollama/pull/18967)）；Prism 三进制 GGUF（`PQ2_0`/`PTQ1_0`）仍是一个存在导入失败的跟踪 issue（[Issue #18521](https://github.com/ollama/ollama/issues/18521)）。
- **K2 Horizon 系列**：关于 `k2-horizon` 架构的功能请求，规模为 0.9B–36B MoE（[Issue #18698](https://github.com/ollama/ollama/issues/18698)）。
- **云模型**：PR 在云上引入 `clef` 和 `clef flash`（[PR #18969](https://github.com/ollama/ollama/pull/18969)）。
- **旧版 glm-ocr GGUF**：PR 将 `<|user|>` 注册为 EOT，以防止输出失控（[PR #17195](https://github.com/ollama/ollama/pull/17195)）。
- **Windows 模型存储**：PR 通过 Windows junction/卷挂载点查找模型，修复在 NTFS 挂载路径上使用 `OLLAMA_MODELS` 时 `ollama list` 为空的问题（[PR #18949](https://github.com/ollama/ollama/pull/18949)、[Issue #18921](https://github.com/ollama/ollama/issues/18921)）。
- **后端/硬件问题**：RTX 5090 搭配 Cohere MoE 出现 CUDA 非法内存访问（[Issue #18642](https://github.com/ollama/ollama/issues/18642)）；AMD RX 6800 XT 上出现 Vulkan 访问冲突（[Issue #18557](https://github.com/ollama/ollama/issues/18557)）；Windows CUDA 后端 `.dll` 被保留为 `.tmp`，导致 CPU 回退（[Issue #18712](https://github.com/ollama/ollama/issues/18712)）；Apple Silicon 上出现 MLX SDPA panic（[Issue #18966](https://github.com/ollama/ollama/issues/18966)、[Issue #18940](https://github.com/ollama/ollama/issues/18940)）。

## 4. 性能与优化
- **据报告，所有模型相较 v0.40.1 出现 25–30% 吞吐量回归**，tokens/sec 更低（[Issue #18968](https://github.com/ollama/ollama/issues/18968)）。暂无修复 PR。
- **提示计时指标修复**：开放 PR 按已评估的 token 计算提示计时，修复热提示缓存下 `prompt_per_second` 虚高的问题（[PR #18964](https://github.com/ollama/ollama/pull/18964)、[Issue #18948](https://github.com/ollama/ollama/issues/18948)）。
- **KV/VRAM 核算**：开放 PR 在 `PredictServerVRAM` 中镜像 GraphSize KV 核算，目标是解决 Qwen 加载/内存预测问题（[PR #17615](https://github.com/ollama/ollama/pull/17615)）。
- **调度器重载行为**：提议启动支持 shift 的 runner，并按请求强制 `shift:false`，避免在 `resolveContextShift` 不同时发生重载（[Issue #18950](https://github.com/ollama/ollama/issues/18950)）。
- **MLX 内存泄漏**：已关闭 issue 报告使用 `qwen3.6:27b-mlx` 时，每次工具调用请求在前缀缓存预算之外泄漏约 0.43 GiB（[Issue #18620](https://github.com/ollama/ollama/issues/18620)）。
- **KV 缓存预热**：功能请求：对系统提示 KV 缓存进行预热，以降低大型系统/工具提示下的首次响应延迟（[Issue #18952](https://github.com/ollama/ollama/issues/18952)）。
- **推理路由复用**：v0.40.4-rc0 在推理路由中复用已验证的 manifests 和已加载模型（[PR #18947](https://github.com/ollama/ollama/pull/18947)）。

## 5. 稳定性与回归
按严重程度排序：
1. **Apple Silicon 上的 MLX SDPA 内核 panic（0.40.x）** — 在 M2 Max、M6 等设备上，搭配 Qwen 3.8、Gemma 4、`qwen3.6:35b-mlx` 和 `gemma4:e2b-mlx` 时出现“Maximum threads per threadgroup is 896 but requested 1024/960”。多个重复项：[Issue #18846](https://github.com/ollama/ollama/issues/18846) 已关闭，[Issue #18856](https://github.com/ollama/ollama/issues/18856) 已关闭，[Issue #18940](https://github.com/ollama/ollama/issues/18940) 已关闭，[Issue #18966](https://github.com/ollama/ollama/issues/18966) 开放，[Issue #18885](https://github.com/ollama/ollama/issues/18885) 开放。相关多模态嵌入在小 `num_ctx` 下的 500 已由 [PR #18973](https://github.com/ollama/ollama/pull/18973) 修复。
2. **25–30% 性能回归**：所有模型相较 v0.40.1 出现 25–30% 性能回归（[Issue #18968](https://github.com/ollama/ollama/issues/18968)）。
3. **使用 `format: json` 时工具调用损坏** — 设置 JSON 格式后模型无法使用工具调用（[Issue #18944](https://github.com/ollama/ollama/issues/18944)）。开放 PR [PR #18975](https://github.com/ollama/ollama/pull/18975) 将允许在结构化输出中使用工具调用。
4. **在 Windows 上，RTX 5090 搭配 Cohere MoE 架构出现 CUDA 非法内存访问**（[Issue #18642](https://github.com/ollama/ollama/issues/18642)）。
5. **AMD RX 6800 XT 上 Vulkan 崩溃** — 加载任何模型时发生访问冲突（[Issue #18557](https://github.com/ollama/ollama/issues/18557)）。
6. **Windows CUDA 后端自动更新失败** — `ggml-cuda.dll` 被保留为 `.tmp`，VRAM 为 0 B / 回退到 CPU（[Issue #18712](https://github.com/ollama/ollama/issues/18712)）。
7. **`shift` 不匹配时调度器重载模型** — 可能导致延迟/吞吐量波动（[Issue #18950](https://github.com/ollama/ollama/issues/18950)）。
8. **Windows 上使用 NTFS 挂载点时 `ollama list` 为空** — 修复 PR 已开放（[Issue #18921](https://github.com/ollama/ollama/issues/18921)、[PR #18949](https://github.com/ollama/ollama/pull/18949)）。
9. **Qwen3-Coder 工具解析器损坏 int64 范围外的数字** — 例如 `1e20` 变成 `9223372036854775807`（[Issue #18421](https://github.com/ollama/ollama/issues/18421)）。
10. **云回归**：云模型忽略 JSON 回复 schema（[Issue #12362](https://github.com/ollama/ollama/issues/12362)）；`deepseek-v4.1-flash:cloud` 在图像输入超过约 655,360 tokens 时返回 500（[Issue #18853](https://github.com/ollama/ollama/issues/18853)）。
11. **其他平台 bug**：在 Raspberry Pi/Debian 13 上第二次 `ollama run` 卡住（[Issue #18796](https://github.com/ollama/ollama/issues/18796)）；Windows GPU 发现控制台闪烁（[Issue #18954](https://github.com/ollama/ollama/issues/18954)）；Windows 托盘服务器始终无法启动（[Issue #18507](https://github.com/ollama/ollama/issues/18507) 已关闭）；Windows Defender 误报（[Issue #2519](https://github.com/ollama/ollama/issues/2519) 已关闭）；`manifests-v2` 出现奇怪别名（[Issue #18870](https://github.com/ollama/ollama/issues/18870) 已关闭）；gpt-oss 出现 `ffn_down_exps.weight size overflows`（[Issue #18869](https://github.com/ollama/ollama/issues/18869) 已关闭）。

## 6. 对应用开发者的意义
- **升级到 0.40.x 前先固定版本/做基准测试**：25–30% 的回归和 MLX panic 使 `v0.40.4-rc0` 对生产 Apple Silicon 和延迟敏感型部署存在风险。如果使用 MLX，考虑留在 0.35.x，直到 SDPA panic 被修复。
- **结构化输出 + 工具**：如果你将 `format: json` 与工具调用一起使用，目前预计会出问题；关注 [PR #18975](https://github.com/ollama/ollama/pull/18975)。请校验工具调用参数的数值精度，尤其是使用 Qwen3-Coder 时（[Issue #18421](https://github.com/ollama/ollama/issues/18421)）。
- **云输出**：对某些云模型而言，JSON schema 强制执行不可靠；应在服务端进行校验，而不是信任 schema（[Issue #12362](https://github.com/ollama/ollama/issues/12362)）。
- **Windows 模型存储**：如果 `OLLAMA_MODELS` 位于 junction/挂载点上，在 [PR #18949](https://github.com/ollama/ollama/pull/18949) 合入前，`ollama list` 可能为空。
- **指标**：[PR #18964](https://github.com/ollama/ollama/pull/18964) 中的提示计时修复将改变热缓存下的 `prompt_per_second`；请相应调整仪表盘。
- **Agent 延迟**：对于长上下文 agent 工作负载，跟踪系统提示 KV 缓存预热请求（[Issue #18952](https://github.com/ollama/ollama/issues/18952)）和调度器 shift 重载修复（[Issue #18950](https://github.com/ollama/ollama/issues/18950)）。
- **值得关注的解析器修复**：Cohere 块排序（[PR #18965](https://github.com/ollama/ollama/pull/18965)）、FunctionGemma 调用间内容（[PR #18961](https://github.com/ollama/ollama/pull/18961)）、Gemma4 空白字符（[PR #17570](https://github.com/ollama/ollama/pull/17570)）和 glm-ocr EOT（[PR #17195](https://github.com/ollama/ollama/pull/17195)）——如果你的 agent 依赖这些模型系列，则这些修复相关。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 摘要 — 2026-10-13

## 今日亮点
- 无新版本发布；v1.93.0 中的严重 Redis 缓存回归（#34614）仍未解决，导致缓存和预算计数器失效。
- 计费正确性改进：Gemini 上下文缓存创建 token 现会计费（#46144），Azure Claude 网络搜索计费已针对部署自有定价修复（#46135），并为 gpt-6/Claude 5 增加了桥接成本一致性测试（#46139、#46140、#46143、#46141、#46130、#46137）。
- MCP 网关支持进度转发和提示取消（#46134）；Anthropic `/v1/messages` 现在将 503 映射为 529 `overloaded_error`（#46063）。

## 版本发布与破坏性变更
- **无新版本发布。**
- **v1.93.0 回归：** Redis 缓存失败，报错 `TypeError: AbstractConnection.__init__() got an unexpected keyword argument 'ssl_check_hostname'`。导致缓存和预算计数器失效。[Issue #34614](https://github.com/BerriAI/litellm/issues/34614)
- **v1.92 回归：** 缺少 `budget_fallbacks` 列导致致命问题（无法登录 Admin UI 等）。[Issue #33074](https://github.com/BerriAI/litellm/issues/33074)
- **OpenTelemetry SDK 锁定在 1.28.0**，阻碍升级；有人请求迁移到 1.40 系列或放宽版本范围。[Issue #29407](https://github.com/BerriAI/litellm/issues/29407)

## 新模型与硬件支持
- 本时间窗口内没有新模型或硬件支持公告。（Bedrock 自动发现功能请求已关闭：[Issue #43568](https://github.com/BerriAI/litellm/issues/43568)）

## 性能与优化
- **Prometheus 多进程目录隔离：** 每次代理启动现在都会获得自己自动创建的 `PROMETHEUS_MULTIPROC_DIR`，防止一个代理清除另一个代理的实时计数器。[PR #42660](https://github.com/BerriAI/litellm/pull/42660)
- **MCP 网关进度/取消：** 进度消息通过发起请求的 SDK 通道转发；HTTP 取消现在会及时取消上游工作。[PR #46134](https://github.com/BerriAI/litellm/pull/46134)
- **透传端点 CPU 修复：** 导致 100% CPU 的无界注册表增长问题已关闭。[Issue #26081](https://github.com/BerriAI/litellm/issues/26081)
- 未报告具体的吞吐量/延迟数据。

## 稳定性与回归
按严重程度排序：
1. **Redis 缓存失败（v1.93.0）** — 导致缓存和预算计数器失效。仍开放。[Issue #34614](https://github.com/BerriAI/litellm/issues/34614)
2. **budget_fallbacks 列缺失（v1.92）** — Admin UI 致命问题。仍开放。[Issue #33074](https://github.com/BerriAI/litellm/issues/33074)
3. **代理管理员 JWT 绕过个人 max_budget** — 安全/计费绕过。已关闭。[Issue #41226](https://github.com/BerriAI/litellm/issues/41226)
4. **流式 500：`NoneType + int`**，当后端 chunk 省略 `choices[].index` 时发生。仍开放。[Issue #46047](https://github.com/BerriAI/litellm/issues/46047)
5. **websearch_interception 泄漏 max_parallel_requests 槽位**，每次使用 `web_search` 工具流式调用 `/v1/messages` 时发生。仍开放。[Issue #46093](https://github.com/BerriAI/litellm/issues/46093)
6. **100-599 之外的 SSE 错误码被丢弃** — 路由器回退/冷却永远不会触发。仍开放。[Issue #31284](https://github.com/BerriAI/litellm/issues/31284)
7. **部署自定义定价被忽略**：在 model-cost-map 重新加载后（`input_cost_per_token: 0`）。仍开放。[Issue #30081](https://github.com/BerriAI/litellm/issues/30081)
8. **Bedrock 音频转写 HTTP 400**，原因是系统消息与音频块不兼容。仍开放。[Issue #41544](https://github.com/BerriAI/litellm/issues/41544)
9. **A2AConfig 静默丢弃 temperature/max_tokens/top_p**。仍开放。[Issue #45944](https://github.com/BerriAI/litellm/issues/45944)
10. **Key 过期时间戳被忽略** — 持久化的过期时间改为从时长推导。仍开放。[Issue #45236](https://github.com/BerriAI/litellm/issues/45236)
11. **非管理员用户无法通过 UI 为个人 key 设置 max_budget。** 仍开放。[Issue #33212](https://github.com/BerriAI/litellm/issues/33212)
12. **Master key 认证错误缺少客户端 IP。** 仍开放。[Issue #33150](https://github.com/BerriAI/litellm/issues/33150)
13. **模型别名权限在别名解析前被拒绝。** 仍开放。[Issue #25281](https://github.com/BerriAI/litellm/issues/25281)
14. **Codex CLI Responses API 工具调用状态未正确关闭。** 仍开放。[Issue #30603](https://github.com/BerriAI/litellm/issues/30603)
15. **不稳定的 MCP schema 校验测试**（约 25% 失败）。仍开放。[Issue #46071](https://github.com/BerriAI/litellm/issues/46071)

今日修复/关闭：
- 移除了 Gemini 3 temperature 注入（[Issue #38663](https://github.com/BerriAI/litellm/issues/38663)、[Issue #45333](https://github.com/BerriAI/litellm/issues/45333)）
- Gemini 工具使用提示 token 现会计费（[Issue #45991](https://github.com/BerriAI/litellm/issues/45991)）
- MCP 二进制资源现在正确返回（[Issue #32330](https://github.com/BerriAI/litellm/issues/32330)）
- MCP OAuth 401 + RFC 9728 元数据（[Issue #31296](https://github.com/BerriAI/litellm/issues/31296)）
- 透传端点 CPU（[Issue #26081](https://github.com/BerriAI/litellm/issues/26081)）
- Minimax 编码计划 API 认证（[Issue #24483](https://github.com/BerriAI/litellm/issues/24483)）

## 这对应用开发者意味着什么
- **谨慎升级：** v1.93.0 的 Redis bug 和 v1.92 的 budget_fallbacks 问题可能破坏缓存和 Admin UI。部署前请测试。
- **计费准确性：** 如果你使用 Gemini 上下文缓存或 Azure Claude 网络搜索，请确保所用构建包含最新计费修复；此前存在少计费/多计费问题。
- **MCP 用户：** 网关现在会转发进度并及时取消上游工作 — 带来更好的用户体验和资源利用率。
- **Anthropic /v1/messages：** 503 响应现在映射为 529 overloaded_error，与 Anthropic 的契约保持一致，并改进 Claude Code 的重试行为。
- **可观测性：** 被追踪响应上的 W3C `traceresponse` 头（[PR #42079](https://github.com/BerriAI/litellm/pull/42079)）有助于将网关请求与 trace 关联。
- **成本跟踪：** 批量 JSONL 行项目可以存储在回调中（[PR #41691](https://github.com/BerriAI/litellm/pull/41691)），用于逐请求成本对账。
- **护栏：** 新增 Wingback 护栏集成（[PR #46136](https://github.com/BerriAI/litellm/pull/46136)）。
- **关注未解决的 bug：** Key 过期时间戳、非管理员 max_budget UI、模型别名权限以及流式 `choices[].index` 问题可能影响生产环境；请跟进关联 issue 以获取修复。

---

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 摘要 — 2026-10-13

来源：[github.com/unslothai/unsloth](https://github.com/unslothai/unsloth)

## 1. 今日亮点

**v0.1.906-beta** 带来两项核心能力：智能体式 **Browser Use**（模型可以打开、阅读、点击、输入和滚动页面）以及 **Qwen-Image-2.1-Turbo**（8 步图像生成），此外还有文本扩散 LLM 训练、更快的 MoE GGUF 和投机解码。与此同时，浏览器智能体获得了底层控制能力——坐标点击、鼠标移动、拖拽以及经批准的脚本执行 ([PR #13505](https://github.com/unslothai/unsloth/pull/13505)) ——并且 LTX-2.5 fp8/int8 现在加载预量化的 DiT，而不是在加载时量化 38 GB bf16 transformer ([PR #13516](https://github.com/unslothai/unsloth/pull/13516))。影响最大的未解决回归仍然是 Studio 中每次工具调用后重复完整上下文再处理 ([Issue #10698](https://github.com/unslothai/unsloth/issues/10698))。

## 2. 发布与破坏性变更

- **v0.1.906-beta** — Browser Use + Qwen-Image-2.1-Turbo。还附带文本扩散 LLM 训练、更快的 MoE GGUF 和投机解码（源中发布说明被截断）。无需迁移步骤。
- **Studio CLI: `--models-dir PATH`**（可重复）已添加到 `unsloth studio`，用于指向本地模型文件夹；通过 [PR #13435](https://github.com/unslothai/unsloth/pull/13435) 合入，关闭 [Issue #8250](https://github.com/unslothai/unsloth/issues/8250)。
- **凭据行为变更：** 将 `token=` 传给 `FastLanguageModel`/`FastModel.from_pretrained` 此前会覆盖 `hf auth login` 会话。该问题已在 [PR #13486](https://github.com/unslothai/unsloth/pull/13486) 修复——此前为受限模型传入的只读 token 可能悄无声息地破坏后续 push。
- **Docker：** `UNSLOTH_STUDIO_KEEP_LEGACY=0` 现在会通过 `docker/run.sh` 传播 ([PR #13488](https://github.com/unslothai/unsloth/pull/13488))。
- **API 缺口（仍未解决）：** Model List API 未暴露按量化方案选择的能力，因此无法以编程方式选择 Q8 还是 Q4_K_M——[Issue #9340](https://github.com/unslothai/unsloth/issues/9340)。
- **测试/CI 相关：** Studio 启动器现在支持由 Kaggle 支持的 notebook 配合 Cloudflare quick tunnel ([PR #6987](https://github.com/unslothai/unsloth/pull/6987))。

## 3. 新模型与硬件支持

- **Qwen-Image-2.1-Turbo** — 8 步图像生成；随 v0.1.906-beta 发布。一个相关分类 bug（`general.architecture=qwen_image21` 的扩散 GGUF 被归为文本生成并从 Images 页面隐藏，AMD ROCm）已关闭：[Issue #13328](https://github.com/unslothai/unsloth/issues/13328)。
- **LTX-2.5 fp8/int8** — 现在从托管的预量化 DiT（`unsloth/LTX-2.5-FP8`）初始化加载，而不是在运行时量化 ([PR #13516](https://github.com/unslothai/unsloth/pull/13516))；VAE fixture、catalog 行和 Video 页面默认值见 [PR #13500](https://github.com/unslothai/unsloth/pull/13500)。
- **文本扩散 LLM 训练**和更快的 **MoE GGUF** 支持随 v0.1.906-beta 发布。
- **非 CUDA 加速器：** vLLM resume/teardown 现在通过当前激活的加速器（NPU/XPU）释放设备缓存，而不是在非 CUDA 环境下静默 no-op——[PR #13438](https://github.com/unslothai/unsloth/pull/13438)。
- **本地模型提供方：** On-Device 检测现在覆盖 LM Studio、Ollama、自定义文件夹、`./models`、Hermes 和 oMLX，并支持可用的 reveal/delete——[PR #13507](https://github.com/unslothai/unsloth/pull/13507)。Audio.cpp 伞形仓库模型（`audio-cpp/audio.cpp-gguf`）现在可按文件夹列出/显示/删除——[PR #13519](https://github.com/unslothai/unsloth/pull/13519)。
- **已请求、尚未支持：** Studio 中的 Ling 3.0（[Issue #8532](https://github.com/unslothai/unsloth/issues/8532)，👍5）；Windows 上的 Gemma4e2b（[Issue #13388](https://github.com/unslothai/unsloth/issues/13388)）；使用独立 llama.cpp 配置的多个并发服务器（[Issue #8488](https://github.com/unslothai/unsloth/issues/8488)，已关闭）；使用自定义 `.crt`/`.key` SSL 而不是仅 Cloudflare HTTPS（[Issue #8716](https://github.com/unslothai/unsloth/issues/8716)）；MLX 预训练/语料打包结构（[Issue #8607](https://github.com/unslothai/unsloth/issues/8607)）。

## 4. 性能与优化

- **投机解码**和 **更快的 MoE GGUF** 路径随 v0.1.906-beta 发布（发布说明被截断；未发布基准测试）。
- Qwen-Image-2.1-Turbo 的 **8 步图像生成**——显著减少扩散服务的步数。
- **LTX-2.5 加载时间：** 避免在加载时量化 38 GB bf16 transformer，降低峰值内存和启动开销 ([PR #13516](https://github.com/unslothai/unsloth/pull/13516))。
- **回归——上下文再处理：** 每次工具调用后似乎都会重新处理完整上下文；在约 30k token 时，这意味着每次工具调用要等待数分钟，归因于此前某次 Studio 更新引入的变更——[Issue #10698](https://github.com/unslothai/unsloth/issues/10698)。尚无关联修复 PR。
- **回归——CPU 饱和：** token 生成期间出现 100% CPU 峰值，自 [Issue #6046](https://github.com/unslothai/unsloth/issues/6046) 以来一直存在。
- **ROCm 上未遵循 VRAM/RAM offload：** 在 AMD ROCm 上，即使勾选“No RAM Offload”，模型仍会从 VRAM 卸载并停放到 RAM——[Issue #10341](https://github.com/unslothai/unsloth/issues/10341)。
- **UI 卡顿：** 大代码块会导致界面卡顿（[Issue #10769](https://github.com/unslothai/unsloth/issues/10769)）；Library 卡片悬停/选中对比度和 time-pill 居中已调整（[PR #13509](https://github.com/unslothai/unsloth/pull/13509)，[#13518](https://github.com/unslothai/unsloth/pull/13518)）。

## 5. 稳定性与回归问题

按严重程度排序；已注明修复状态。

1. **工具调用上下文再处理（性能回归，无修复）** — 长上下文下每次工具调用停顿数分钟。[Issue #10698](https://github.com/unslothai/unsloth/issues/10698)
2. **大型 MoE 长上下文 OOM** — Qwen2.5 72B 4-bit 在 A800 上 32k 时 OOM，而 24k 稳定；这与 Llama 3.3 70B 已发布 80k+ 的说法相矛盾。[Issue #2937](https://github.com/unslothai/unsloth/issues/2937)
3. **桌面端拖放损坏** — 拖放文件时静默未附加，不显示错误。[Issue #13473](https://github.com/unslothai/unsloth/issues/13473)
4. **Wayland/GNOME/Arch 上桌面 AppImage 窗口尺寸不可用** — 无法调整大小、帧率问题、多显示器。[Issue #12845](https://github.com/unslothai/unsloth/issues/12845)
5. **聊天客户端崩溃** — `tapClientLookup: Index 1 out of bounds`、消息部分格式错误、“New chat”时重复 `toolCallId`。[Issue #10288](https://github.com/unslothai/unsloth/issues/10288)
6. **训练期间 `UNSLOTH_RETURN_LOGITS=1` 被静默重置为 `0`** — 破坏 Gemma 3 上的自定义验证指标。标记为已修复，待确认。[Issue #3071](https://github.com/unslothai/unsloth/issues/3071)
7. **Blackwell（RTX 5090）启动失败** — `PY_SSIZE_T_CLEAN macro must be defined`；已关闭。[Issue #2760](https://github.com/unslothai/unsloth/issues/2760)
8. **Qwen3 几分钟后 `deallocating None`** — 梯度检查点与内存管理冲突；已关闭。[Issue #2955](https://github.com/unslothai/unsloth/issues/2955)
9. **本地 RTX 5060 Ti 上 vLLM 后端加载失败**（`vllm_process failed to load!`）；已关闭。[Issue #2963](https://github.com/unslothai/unsloth/issues/2963)
10. **模型加载失败** — `unsloth/GLM-OCR`（[Issue #4269](https://github.com/unslothai/unsloth/issues/4269)）和 `ggml-org/sesame-csm-1b-GGUF`（[Issue #9430](https://github.com/unslothai/unsloth/issues/9430)）。
11. **质量回归** — Qwen3 推理能力在微调后消失（[Issue #3005](https://github.com/unslothai/unsloth/issues/3005)）；Gemma4-31B-it 退化为重复（[Issue #5124](https://github.com/unslothai/unsloth/issues/5124)）；微调后的 Whisper-large-v3 在波斯语上表现不及 base（[Issue #2942](https://github.com/unslothai/unsloth/issues/2942)）。
12. **其他 Studio 故障** — 数据集下载失败（[Issue #9775](https://github.com/unslothai/unsloth/issues/9775)）、macOS 文本编码错误（[Issue #8594](https://github.com/unslothai/unsloth/issues/8594)）、qwen2.5 衍生模型上的 LoftQ 训练错误（[Issue #6958](https://github.com/unslothai/unsloth/issues/6958)）、OpenCode 无自动加载（[Issue #9171](https://github.com/unslothai/unsloth/issues/9171)）。

**今日已合入的安全修复：**
- 将文件链接到聊天的工具工作区时防止跟随符号链接（来自最新安全扫描）——[PR #13513](https://github.com/unslothai/unsloth/pull/13513)。
- HF 认证 token 不再被 `from_pretrained(token=...)` 覆盖——[PR #13486](https://github.com/unslothai/unsloth/pull/13486)。

## 6. 对应用开发者的意义

- **Browser Use 使 Unsloth 成为可行的本地智能体运行时。** 模型现在可以导航，按 ref *或*坐标点击、拖拽并运行经批准的脚本 ([PR #13505](https://github.com/unslothai/unsloth/pull/13505))。如果你构建浏览器智能体，可以将其与 headless-Chrome 技术栈对比评估——但要为上下文再处理回归（#10698）做好预算，它会让当今的长上下文工具循环变得昂贵。
- **工具调用工作负载现在需要上下文管理策略。** 再处理回归加上新的“Context compacted”内联分隔符 ([PR #13502](https://github.com/unslothai/unsloth/pull/13502)) 表明，跨工具轮次的提示缓存尚不可靠。在扩展智能体循环之前，请测量每次工具调用的 TTFT。
- **多模态服务正在快速扩展。** Qwen-Image-2.1-Turbo（8 步）、LTX-2.5 fp8/int8 预量化 DiT 以及文本扩散训练意味着一个运行时正越来越多地覆盖图像、视频和音频管线——audio.cpp 和 whisper.cpp 的更新流程正在被合并到单张卡片中 ([PR #13497](https://github.com/unslothai/unsloth/pull/13497))。
- **如果你依赖量化选择，请锁定版本。** Model List API 仍无法表达按量化方案的目标 ([Issue #9340](https://github.com/unslothai/unsloth/issues/9340))；按仓库和精度解析模型的客户端需要带外映射。
- **非 CUDA 部署正获得真正关注**（[PR #13438](https://github.com/unslothai/unsloth/pull/13438) 中的 NPU/XPU 缓存释放），但 AMD/ROCm 用户仍报告 offload 和扩散分类 bug（#10341、#13328）——请在 ROCm 上显式验证内存行为，而不是信任 offload 开关。

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*