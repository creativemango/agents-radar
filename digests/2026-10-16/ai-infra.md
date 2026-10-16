# AI 基础设施日报 2026-10-16

> 生成时间: 2026-10-16 00:40 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# 跨项目 AI 基础设施对比 — 2026-10-16

## 1. 生态系统概览
AI 基础设施活动仍然极度活跃，但重心已从原始吞吐量转向**正确性、缓存安全性和智能体/工具可靠性**。服务引擎正竞相在 GB300、Blackwell SM120、ROCm/MI355X 和 Apple/消费级硬件上支持 DeepSeek-V4.1-Flash、GLM-5.3、Qwen3.x、Kimi K3、Muse Spark Open 和 Nemotron 3.5。与此同时，多个高严重度问题都是静默正确性 bug——PD 分离中的陈旧 KV、前缀缓存跨请求污染、工具定义被剥离，以及网关预算/支出绕过。稳定版发布有限：llama.cpp 发布 v0.7.0，Unsloth 发布 beta，LiteLLM 发布预发布版，而 vLLM/SGLang/Ollama 在此窗口无新版本。

## 2. 活跃度对比
*N/R = 摘要未披露 24 小时总数。在可获得时，数量按“过去 24 小时内更新”统计。*

| 项目 | 过去 24 小时更新的 Issue | 过去 24 小时更新的 PR | 发布状态 |
|---|---:|---:|---|
| **vLLM** | 121 | 500 | 无新版本；无正式破坏性变更 |
| **SGLang** | N/R | N/R | 无新版本；正在移除 Python 3.10；无 API 移除 |
| **llama.cpp** | 47 | 149 | **v0.7.0 / b11669**；b11671 Muse Spark Open；b11658 maion-coder；b11662；b11643；b11642 |
| **Ollama** | N/R | N/R | 过去 24 小时无版本发布 |
| **LiteLLM** | N/R | N/R | **v1.107.0-dev.2**、**v1.106.0-rc.4** 预发布版；cosign Docker 验证 |
| **Unsloth** | N/R | N/R | **v0.1.908-beta** |

**解读：** vLLM 和 llama.cpp 披露的原始更新量最大。vLLM 以 PR 为主且聚焦正确性；llama.cpp 正在交付发布内容。SGLang 主要由 CI/稳定性和 SM120/GLM-5.3 问题占据。Ollama、LiteLLM 和 Unsloth 的活动分别集中在智能体/工具正确性、网关可运维性以及训练/模型接入上。

## 3. 模型支持竞赛
**已发布/已落地支持：**
- **llama.cpp：** v0.7.0 新增 GLM5-Next MTP、K2 Horizon dense/MoVA、Prism 以及主机内存 MoE 专家缓存。b11671 新增 **Muse Spark Open** 文本/视觉/音频。b11658 新增 **maion-coder** 以及 draft-model 支持。
- **Unsloth：** v0.1.908-beta 新增 **NVIDIA Nemotron 3.5 / Bolt（24 GB）**、**Meta Muse Spark Open**、文本扩散 LLM、DeepSpeed 训练，并改进了面向编码智能体的提示缓存。FLUX.3 Image Klein 加载已在 PR 中。
- **Ollama：** 通过升级 llama.cpp 修复 MiniCPM-V 4.7 projector 支持；新增 Q2_0 GGUF 支持；新增 Clippy Vision 和 Clef Cloud。
- **LiteLLM：** 不是模型内核项目，但在提供方兼容性上有所推进，包括 Bedrock Claude Web 身份、OpenAI `gpt-transcribe`、Anthropic compaction blocks、Bedrock Sonnet 5.5 / Haiku 5.5 token 计数、Kimi K3 / DeepSeek framing，以及 Databricks gpt-5.6 工具/推理问题。

**进行中/尚未发布：**
- **vLLM：** Muse Spark Open、ROCm 上的 Step-5-Preview、DeepSeek-V4.1-Flash GB300 低延迟解码、ROCm fused QK-norm + RoPE + gate kernels。Nemotron 3.5 ASR 流式、Phi4Flash 和 ViT full CUDA graph 属于请求/RFC。
- **SGLang：** DeepSeek-V4.1-Flash 的 SM120 现场报告；GLM-5.3-Flash SM120 跟踪；Hopper CuTe DSL KDA verify kernel；AMD AITER/DSv4 GEMM 工作；Muse Spark Open NIXL KV packing。

**谁领先？**
- **已发布的本地模型支持广度：** llama.cpp 和 Unsloth 领先。
- **新加速器上的数据中心服务支持：** vLLM 和 SGLang 领先，尤其是在 DeepSeek-V4.1-Flash、GLM-5.3、ROCm/MI355X 和 GB300/SM120 方面。
- **提供方/API 兼容性：** LiteLLM 领先。
- **智能体正确性：** 由于原生 `/api/chat` 工具定义问题，Ollama 目前落后。

## 4. 性能前沿
优化工作集中在五个方面：

**KV 缓存与前缀缓存**
- vLLM：PD 分离前缀缓存污染、KV connector 缓存有效性、模型定制 KVCache 规划、Mamba block-size 回归。
- SGLang：HiCache 陈旧 KV 静默损坏、统一内存池预热/压缩、NIXL KV packing。
- llama.cpp：提示缓存现在仅保存可恢复的 slot 状态；`--moe-cache-mib` 主机专家缓存。
- Unsloth：用于并发聊天的 KV 抢占、面向编码智能体的提示缓存。

**批处理与解码延迟**
- vLLM：DeepSeek-V4.1-Flash GB300 低延迟解码、DSpark 投机解码、FA2 短查询/长上下文注意力。
- SGLang：面向 EAGLE 低并发解码的 AMD FP8 quant+GEMM 融合、PP-spec 比较跳过。
- Ollama：MLX 引擎仍为单请求串行；无连续批处理。
- Unsloth：通过 llama-server KV parking/preemption 实现并行聊天服务。

**量化与低位格式**
- llama.cpp：Q1_0/Q2_0 numpy 支持、Hexagon Q1_0 提案、重新打包重构。
- Ollama：Q2_0 GGUF 张量/文件类型支持。
- Unsloth：compile 下的 4-bit MoE 反量化、FLUX.3 INT8/FP8/GGUF。
- vLLM/SGLang：NVFP4 OOM、W4A16 路由专家、FP8 KV。

**分布式服务**
- vLLM：TP4 + DSpark、PD 分离、MoE 精确步长、混合 GDN 前缀缓存命中。
- SGLang：NIXL TP 异构传输、PD prefill + HiCache、DCP planner 崩溃。
- llama.cpp：MTP 多 GPU 提示处理回归；Vulkan 多设备 + CPU-MoE 变慢。
- LiteLLM：网关负载测试、重试/退避、WebSocket 支出核算、Rust 路由对齐。

**内核**
- vLLM：FA2 优化、ROCm Triton 融合内核、Mamba2 NaN 修复。
- SGLang：AMD FP8 GEMM 融合、Hopper 上的 CuTe DSL KDA verify、deepstack CUDA-graph 处理。
- llama.cpp：Metal BF16 tensor-API flash attention、Vulkan f16 KV striding、Hexagon 张量重新打包。
- Unsloth：ROCm flash attention 计时探测、`torch.compile` 外的 MoE 反量化、混代 GPU 能力检测。

## 5. 层级定位

| 层级 | 项目 | 主要关注点 | 今日信号 |
|---|---|---|---|
| **服务引擎** | vLLM, SGLang | 高吞吐分布式 GPU 推理、分页注意力、PD 分离、投机解码 | 前缀缓存/PD 中的正确性 bug 是最大风险；新硬件支持活跃 |
| **本地运行时** | llama.cpp, Ollama | 单节点/桌面/边缘推理、GGUF、广泛硬件 | llama.cpp 正在交付 MoE offload/spec decode；Ollama 在智能体/工具调用正确性上挣扎 |
| **网关** | LiteLLM | 多提供方路由、认证、预算、支出日志、重试、MCP | 预算绕过、支出表膨胀、RAM/OOM 和 `/v1/messages` 兼容性是关键风险 |
| **训练/微调** | Unsloth | LoRA/RL/GRPO、模型接入、Studio UI、AMD/NVIDIA 支持 | 对 Nemotron 3.5/Muse Spark 的 beta 支持；多 GPU 微调仍在路线图，沙箱不够严密 |

**重叠：** Ollama 底层依赖 llama.cpp/MLX。Unsloth 也涉及推理/Studio 服务。vLLM 和 SGLang 越来越多地用于 RL rollout 和智能体后端，这使它们暴露在训练循环正确性要求之下。

## 6. 趋势信号

1. **静默正确性如今是主导性的基础设施风险。**  
   vLLM 的 PD 前缀缓存串扰、SGLang 的 HiCache 陈旧 KV bug、Ollama 的工具定义被剥离，以及 LiteLLM 的预算/支出绕过，都会在没有明确错误的情况下失败。智能体开发者应添加输出金丝雀、禁用有风险的前缀缓存，并监控支出表。

2. **新模型支持速度在本地运行时和训练栈中最高。**  
   llama.cpp 和 Unsloth 已发布 Muse Spark Open、Nemotron 3.5、GLM5-Next、K2 Horizon 和文本扩散支持。vLLM/SGLang 仍在为新加速器启用 DeepSeek-V4.1-Flash、GLM-5.3 和 Qwen3.x。

3. **硬件多样化正在加速——硬件特定回归也在加速。**  
   GB300、Blackwell SM120、ROCm MI355X、Strix Halo、Vulkan、Metal、Hexagon 和 MIG 均出现。SM120 明确是“最少经过实战检验”的；在修复前的构建上，不应信任多 GPU 异构和 MIG 决策。

4. **投机解码和 MoE offload 是主要性能杠杆——但也伴随回归。**  
   vLLM DSpark、llama.cpp LiliCorr/MTP、SGLang EAGLE/DSPARK 和 MoE 主机缓存带来大幅收益，但 MTP 多 GPU 提示处理和 Vulkan MTP 断言是当前活跃的回归。

5. **网关运营化正在成熟。**  
   LiteLLM 正在增加重试/退避、WebSocket 支出核算、负载测试套件、CLI 冷启动改进和 Rust 路由对齐。对于生产环境，成本/预算正确性和 `/v1/messages` 稳定性仍是关注项。

6. **智能体工作负载正在给每一层带来压力。**  
   工具 schema 校验开销、流式解析器 chunk 边界 bug、GLM-5.3-Flash 退化、沙箱逃逸风险和空工具定义均存在。工具调用应优先使用 `/v1` 而非 Ollama 原生 `/api/chat`；在 vLLM 中校验 strict/required/named 工具选择；并将 Unsloth 沙箱视为尚不是安全边界。

7. **本周期中，锁定版本比往常更重要。**  
   SGLang 正在为 CUDA 构建移除 Python 3.10；Unsloth 正在以 transformers 5.20.0 和 TRL < 0.28 为基准重新调整；LiteLLM 预发布版仅支持 cosign；llama.cpp 的 v0.7.0 改变了提示缓存和 MTP 行为。在 CI 和生产环境中有意识地冻结版本。

---

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 摘要 — 2026-10-16

**活动快照：** 过去 24 小时内没有新版本发布。121 个 issue 和 500 个 PR 有更新。工作集中在 DeepSeek-V4.1-Flash 低延迟路径、ROCm 模型启用、注意力/内核优化，以及围绕前缀缓存和 KV 连接器的正确性修复。

## 1. 今日亮点
过去 24 小时内没有新的 vLLM 版本发布，但开发活动仍然活跃。当前最值得关注的进行中工作包括面向 GB300 的 DeepSeek-V4.1-Flash 低延迟解码栈（[#61868](https://github.com/vllm-project/vllm/pull/61868), [#61869](https://github.com/vllm-project/vllm/pull/61869)）、ROCm Step-5-Preview 支持（[#61837](https://github.com/vllm-project/vllm/pull/61837)），以及面向短查询/长上下文工作负载的 FA2 注意力优化（[#61784](https://github.com/vllm-project/vllm/pull/61784)）。最严重的未解决正确性问题是启用自动前缀缓存的 PD 分离场景中的跨请求上下文污染（[#61160](https://github.com/vllm-project/vllm/issues/61160)）；相关的 KV 连接器缓存有效性 bug 也仍处于打开状态（[#61768](https://github.com/vllm-project/vllm/issues/61768)）。

## 2. 版本发布与破坏性变更
- 过去 24 小时内没有版本发布。
- 没有正式的破坏性变更公告。配置注意：在移除 `mamba_cache_mode="all"` 后，`--mamba-block-size` 据报告不再生效（[#60838](https://github.com/vllm-project/vllm/issues/60838)）。

## 3. 新模型与硬件支持
- **Muse Spark Open** 支持 PR 已打开：[#61879](https://github.com/vllm-project/vllm/pull/61879)。依赖一个 FlashInfer PR。
- **ROCm 上的 Step-5-Preview** PR 已打开：[#61837](https://github.com/vllm-project/vllm/pull/61837)。新增 `amd/` 实现，使用可移植 Triton 内核实现压缩稀疏注意力。NVIDIA 侧所提及的实现仅限于 SM90/SM103。
- **Nemotron 3.5 ASR Streaming**（`nemotron-3.5-asr-streaming-0.6b`）功能请求：[#47455](https://github.com/vllm-project/vllm/issues/47455)。
- **V1 中的 Phi4Flash** 功能请求：[#23957](https://github.com/vllm-project/vllm/issues/23957)。
- **ViT Full CUDA Graph** RFC，面向 Qwen3-VL、Qwen3.5、GLM-V 和 Kimi K2.5 等多模态模型：[#38175](https://github.com/vllm-project/vllm/issues/38175)。
- **面向 Qwen3-Next/Qwen3.5 的 ROCm 融合 QK-norm + RoPE + gate Triton 内核**：[#51406](https://github.com/vllm-project/vllm/pull/51406)。
- **GB300 上的 DeepSeek-V4.1-Flash** 低延迟解码与 DSpark 草稿路径：[#61868](https://github.com/vllm-project/vllm/pull/61868), [#61869](https://github.com/vllm-project/vllm/pull/61869)。

## 4. 性能与优化
- **DeepSeek-V4.1-Flash GB300**：可选启用的低批量解码路径，采用 TP4 + DSpark 推测解码（[#61868](https://github.com/vllm-project/vllm/pull/61868)）；DSpark 草稿获得自己的解码执行器、融合 Markov 循环以及 post-forward CUDA graph（[#61869](https://github.com/vllm-project/vllm/pull/61869)）。
- **FA2 短查询注意力**：面向 SM8x GPU 上长缓存上下文中的短查询，采用 head-packed、KV-split 的上下文注意力（[#61784](https://github.com/vllm-project/vllm/pull/61784)）。
- **ROCm/AMD 跟踪项**：Qwen3.8-2.4T-A95B MI355X（[#57149](https://github.com/vllm-project/vllm/issues/57149)）、Qwen3.8-Flash-Next MI355X（[#59575](https://github.com/vllm-project/vllm/issues/59575)）、GLM 5.3（[#57406](https://github.com/vllm-project/vllm/issues/57406)）以及 Kimi K3（[#50587](https://github.com/vllm-project/vllm/issues/50587)）。
- **编译器/内核策略**：将编译器融合移植为手动融合的 RFC（[#43224](https://github.com/vllm-project/vllm/issues/43224)）；`vllm/models` 中 MonoKernels 的 RFC（[#60904](https://github.com/vllm-project/vllm/issues/60904)）。
- **KV/缓存规划**：模型定制化 KVCache 规划的 RFC（[#44276](https://github.com/vllm-project/vllm/issues/44276)）。
- **迭代指标**：可选 GPU 计时（[#61690](https://github.com/vllm-project/vllm/pull/61690)）、工作负载快照（[#61691](https://github.com/vllm-project/vllm/pull/61691)）和 FPM v1 导出（[#52061](https://github.com/vllm-project/vllm/pull/52061)）。
- **模拟/基准测试**：基于 CPU 的 CI 与硬件感知性能模拟 RFC（[#61406](https://github.com/vllm-project/vllm/issues/61406)）；BYOG 可复现评测/基准测试 RFC（[#61698](https://github.com/vllm-project/vllm/issues/61698)）。
- **需跟踪的回归**：自 #56876 以来，SM12x 上的 MoE 解码约慢 15%，与 DeepGEMM 连续布局对齐从 64 改为 128 有关（[#58624](https://github.com/vllm-project/vllm/issues/58624)）。
- **加载耗时问题**：ModelOpt Llama-4 检查点从 CPU 页缓存加载需要 5 分钟以上（[#31624](https://github.com/vllm-project/vllm/issues/31624)）。

## 5. 稳定性与回归
按严重程度排序。

- **严重 — 跨请求 KV 污染**：在 NIXL prefill/decode 分离中启用自动前缀缓存时，响应可能间歇性包含其他请求的内容；可通过 `--no-enable-prefix-caching` 规避（[#61160](https://github.com/vllm-project/vllm/issues/61160)）。**仍打开，本数据中无修复 PR。**
- **严重 — 静默 CUDA IMA**：在 RTX 3090 上，hybrid GDN + MTP k=3 + 异步调度可能在非法内存访问后以退出码 0 退出；此前的修复均未解决（[#53726](https://github.com/vllm-project/vllm/issues/53726)）。**仍打开。**
- **高 — 共享内存广播超时**：“No available shared memory broadcast block found in 60 seconds” 有 54 条评论；表明存在挂起或长时间编译/量化工作（[#35465](https://github.com/vllm-project/vllm/issues/35465)）。**仍打开。**
- **高 — KV 连接器缓存失效**：在加载失败处理之前中止，会使无效的前缀缓存块仍可被复用（[#61768](https://github.com/vllm-project/vllm/issues/61768)）。**仍打开。**
- **高 — LoRA 在 MLA 上静默失效**：针对 `kv_b_proj` 的适配器从未被应用；decode 通过吸收机制绕过，prefill 使用过时的 pre-wrap 引用（[#48974](https://github.com/vllm-project/vllm/issues/48974)）。**仍打开。**
- **高 — 包装器模型的 LoRA 静默失效**：当 LM 挂载在 `language_model` 下时，文本 PEFT LoRA 可能静默失效（[#48019](https://github.com/vllm-project/vllm/issues/48019)）。**仍打开。**
- **中高 — guidance 下工具调用损坏**：guidance 后端以 `KeyError: 'triggers'` 拒绝结构化工具标签；strict/required/named `tool_choice` 失败（[#55152](https://github.com/vllm-project/vllm/issues/55152)）。**仍打开。**
- **中高 — Blackwell OOM**：在 16 GB Blackwell 上使用 NVFP4 Qwen3.6-35B-A3B 时，在 `profile_run` 内部分配的 FlashInfer b12x SM120 MoE 工作区会 OOM（[#49476](https://github.com/vllm-project/vllm/issues/49476)）。**仍打开。**
- **中 — Mamba 配置回归**：移除 `mamba_cache_mode="all"` 后，`--mamba-block-size` 不再生效（[#60838](https://github.com/vllm-project/vllm/issues/60838)）。**仍打开。**
- **中 — ROCm P/D 挂起**：在 AMD GPU P/D 运行中，MoRI/MoRIIO READ 模式 KV 传输可能挂起或仅部分完成（[#45749](https://github.com/vllm-project/vllm/issues/45749)）。**仍打开，已陈旧。**
- **中 — Ray CPU 开销**：使用 Ray 分布式流水线并行时，每个节点有 3 个核心 CPU 占用率达到 100%（[#21231](https://github.com/vllm-project/vllm/issues/21231)）。**仍打开。**
- **LoRA/RL RFC**：跟踪 RL rollout 中未解决的 LoRA 正确性与生命周期问题（[#48297](https://github.com/vllm-project/vllm/issues/48297)）。**仍打开。**
- **已修复/已关闭**：`/v1/embeddings` 在输入被截断时死锁（[#55427](https://github.com/vllm-project/vllm/issues/55427)）；ROCm Kimi K2.5 MXFP4 decode ITL 回归（[#45943](https://github.com/vllm-project/vllm/issues/45943)）；重复的 hash-routing 方法导致 mypy 失败（[#61924](https://github.com/vllm-project/vllm/pull/61924)）；DP=1 的 TP 分片 routed-expert 捕获（[#61388](https://github.com/vllm-project/vllm/pull/61388)）。
- **待合并修复 PR**：`moe_forward` 的 MoE 精确步幅（[#61749](https://github.com/vllm-project/vllm/pull/61749)）；sm90 上 Mamba2 chunk 扫描 NaN（[#61718](https://github.com/vllm-project/vllm/pull/61718)）；MTP 下 hybrid GDN 前缀缓存命中（[#52244](https://github.com/vllm-project/vllm/pull/52244)）；ROCm DFlash2 滑动窗口前缀注意力 NaN（[#53628](https://github.com/vllm-project/vllm/pull/53628)）；reload/refit 修复（[#61287](https://github.com/vllm-project/vllm/pull/61287)）。

## 6. 对应用开发者的意义
- 如果你在以 PD 分离方式提供推理服务并使用前缀缓存，请在 [#61160](https://github.com/vllm-project/vllm/issues/61160) 修复前**禁用自动前缀缓存**（`--no-enable-prefix-caching`）。所报告的串扰是正确性和隐私风险。
- 使用 guidance 后端的工具调用应用应验证 strict/required/named `tool_choice`；[#55152](https://github.com/vllm-project/vllm/issues/55152) 报告其已损坏。
- 在 MLA 模型或包装器架构上使用 LoRA 的用户应验证适配器是否实际生效；存在静默失效 bug 仍未解决（[#48974](https://github.com/vllm-project/vllm/issues/48974)、[#48019](https://github.com/vllm-project/vllm/issues/48019)）。
- Mamba 用户不应依赖 `--mamba-block-size`，直到 [#60838](https://github.com/vllm-project/vllm/issues/60838) 解决。
- GB300 上的 DeepSeek-V4.1-Flash 用户可预期从 [#61868](https://github.com/vllm-project/vllm/pull/61868) 和 [#61869](https://github.com/vllm-project/vllm/pull/61869) 获得近期延迟改进，但这些是可选启用路径，且尚未发布。
- ROCm 团队有新的模型启用和内核融合工作正在进行（[#61837](https://github.com/vllm-project/vllm/pull/61837)、[#51406](https://github.com/vllm-project/vllm/pull/51406)）；请跟踪 AMD 针对 Qwen3.8、GLM 和 Kimi 的性能跟踪项。
- 今天没有可升级或降级的版本；请优先监控上述严重正确性问题，而不是进行常规版本升级。

---

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 摘要 — 2026-10-16

## 1. 今日要点

CI 稳定性工作占据今日关注焦点：flaky-test 跟踪器 [#42752](https://github.com/sgl-project/sglang/issues/42752)（83 条评论）以及现已关闭的 CI 维护模式 issue [#21065](https://github.com/sgl-project/sglang/issues/21065)，反映出 `PR Test Base/Extra` 运行仍在持续波动。在 serving 侧，当天最严重的信号是 PD prefill + HiCache 中的一个**静默正确性**缺陷（[#44070](https://github.com/sgl-project/sglang/issues/44070)），其中 decode 可能收到陈旧 KV；同时还出现一批 SM120 / GLM-5.3-Flash 报告。性能工作覆盖 AMD FP8 GEMM 融合、NIXL TP 异构传输，以及对 CuTe DSL KDA verify 内核的 Hopper 支持。

## 2. 发布与破坏性变更

过去 24 小时无新版本发布。

- **正在进行 Python 3.10 移除** — [#44393](https://github.com/sgl-project/sglang/pull/44393) 将剩余的 CUDA `sglang-kernel` wheel 路径（`cp310-abi3`）及相关 CI/发布任务迁移到 Python ≥3.11，接续 #43021。非 CUDA 硬件路径明确不在本 PR 范围内。任何为 CUDA 构建固定 3.10 运行时的用户都应计划升级。
- **今日未宣布 API/配置移除**；用于可选启用 `toktier` tokenizer 后端的 RFC（[#44374](https://github.com/sgl-project/sglang/issues/44374)）仅处于提案阶段。

## 3. 新模型与硬件支持

- **SM120（RTX PRO 6000 Blackwell，仅 PCIe）** — 关于 DeepSeek-V4.1-Flash 在 8× RTX PRO 6000 上的现场报告，包含实测 prefill/decode 以及投机解码 A/B（[#40877](https://github.com/sgl-project/sglang/issues/40877)）。另一个跟踪 issue 覆盖 GLM-5.3-Flash 在 SM120 TP2 上所需的修复，涉及 W4A16 路由专家、FP8 KV、视觉和原生 MTP（[#37813](https://github.com/sgl-project/sglang/issues/37813)）。
- **Hopper（SM90）KDA verify 内核** — [#44612](https://github.com/sgl-project/sglang/pull/44612) 将融合的 CuTe DSL KDA（`fused_kda_decode_mtp_dspark`）verify 内核扩展到 SM90；此前因成对 FP32 指令仅限 SM100+。
- **AMD / ROCm** — AITER 升级就绪跟踪器仍活跃（[#21302](https://github.com/sgl-project/sglang/issues/21302)）；PR [#44008](https://github.com/sgl-project/sglang/pull/44008) 携带调优后的 DSv4 GEMM 行和 mHC `tile_m=16`，直到上游 AITER 发布这些内容。扩散多模态单元测试正在改为 ROCm 感知（[#44599](https://github.com/sgl-project/sglang/pull/44599)）。
- **Tokenizer 后端 RFC** — `toktier`，一个可选启用的精确增量式与 chunk 并行 CPU tokenizer 后端（[#44374](https://github.com/sgl-project/sglang/issues/44374)）。
- **Monokernel 框架 RFC** — 树内 `python/sglang/experimental/monokernels/` 插件模型，用于逐模型融合内核（[#43949](https://github.com/sgl-project/sglang/issues/43949)）。

## 4. 性能与优化

- **AMD MI355X，Qwen3.5-397B-A17B-FP8**：将 1x128 激活量化（约 4 µs）融合进 small-M block-scale GEMM（约 9 µs），按每个 dense FP8 projection 计 — 面向 EAGLE 低并发 decode（[#42229](https://github.com/sgl-project/sglang/pull/42229)）。
- **AMD linear-attn**：在 Qwen3.5-397B 的 GDN/KDA verify 路径上跳过未使用的 PP-spec 比较（[#43861](https://github.com/sgl-project/sglang/pull/43861)）。
- **用于 TP 异构传输的 NIXL KV packing**（Muse Spark Open）（[#44615](https://github.com/sgl-project/sglang/pull/44615)）。
- **统一内存池**：启动时预热 compaction move，并按 page id 复制整页 page envelope，从而从服务路径中移除首次使用时的内核编译/加载和 `_assert_async` 开销（[#44592](https://github.com/sgl-project/sglang/pull/44592)）。
- **Diffusion 运行时导入**：在约 10k 模块导入期间推迟完整 GC 回收，避免重复的完整堆追踪（[#44606](https://github.com/sgl-project/sglang/pull/44606)）。
- **工具 schema 校验**：用 `jsonschema-rs` 替换纯 Python 的 `Draft202012Validator.check_schema`（约 0.7 ms/工具；在 API 事件循环上，一个 18 工具 agent 请求约 12 ms）（[#43274](https://github.com/sgl-project/sglang/pull/43274)）。
- **CUDA graph**：让 deepstack media prefill 保持 eager，这样 Qwen3-VL 系列的 graph replay 不会复用陈旧的 `input_deepstack_embeds`（[#44607](https://github.com/sgl-project/sglang/pull/44607)）。

## 5. 稳定性与回归

按严重程度排序；有修复 PR 的已注明。

1. **静默 KV 损坏 — PD prefill + HiCache**（[#44070](https://github.com/sgl-project/sglang/issues/44070)）：在默认 `SGLANG_DISAGG_PREFILL_EARLY_SEND_CACHED_PREFIX=1` 下，当两个请求共享一个其 KV 仅存在于主机内存中的前缀时，decode 可能对部分前缀收到陈旧 KV。会静默产生错误结果，而不是崩溃。未引用修复 PR。
2. **客户端断开导致整引擎崩溃**（[#39216](https://github.com/sgl-project/sglang/issues/39216)）：未捕获的 `asyncio.CancelledError` 绕过 `except Exception`；在 4× RTX 6000D / DeepSeek-V4.1-Flash 上可复现。
3. **整个 TP 组 OOM 崩溃**（[#41076](https://github.com/sgl-project/sglang/issues/41076)）：DeepSeek-V4.1-Flash + DSPARK 下 `SparsePrefillWorkspace` 无界分配。
4. **Kimi K3 decode 崩溃**（[#34920](https://github.com/sgl-project/sglang/issues/34920)）：PD disagg + `--dcp-size 8` + DSPARK target-verify 在 `dcp/planner.py` 中触发 `cumsum(extend_prefix_lens=None)`；所有 TP rank 均挂掉。
5. **GLM-5.3-Flash 在 DFLASH 投机解码下输出退化**（[#40843](https://github.com/sgl-project/sglang/issues/40843)）：推理输出中出现严重重复/循环。
6. **GLM-5.3-Flash 在 agentic 提示下出现 `!` 循环**（[#36669](https://github.com/sgl-project/sglang/issues/36669)）：复杂 system prompt + 多个工具 + `tool_choice=auto` 会使 thinking 输出退化。
7. **SM120 attention 后端矩阵**（[#42012](https://github.com/sgl-project/sglang/issues/42012)）：fa4 在 hybrid extend reshape 的 CUDA-graph capture 时崩溃；在测试过的 `v0.5.20` 镜像上，triton 是唯一可用的后端。
8. **NIXL PD abort 挂起**（[#44369](https://github.com/sgl-project/sglang/issues/44369)）：abort 可能让所有默认传输 worker 一直等待健康的非终止 handle。
9. **Qwen3.8-Flash-Next 静默语义损坏**（[#37111](https://github.com/sgl-project/sglang/issues/37111)）：GB10 TP2 上的 QSA + NEXTN decode graph。
10. **同步请求预处理阻塞事件循环**（[#30770](https://github.com/sgl-project/sglang/issues/30770)），导致 health endpoint 延迟 — 仅 CPU 的确定性复现。
11. **无界多模态音频解码**（[#43708](https://github.com/sgl-project/sglang/issues/43708)）。
12. **次要/测试基础设施**：`WeightChecker.reset_tensors` 用随机值填充 `MRotaryEmbedding.axis_map`（[#44501](https://github.com/sgl-project/sglang/issues/44501)）；profiler 文件名路径穿越，经由调用方控制的 `output_dir`/`profile_prefix`（[#44609](https://github.com/sgl-project/sglang/pull/44609)，修复 PR 开放中）。
13. **CI 抖动**：跟踪器 [#42752](https://github.com/sgl-project/sglang/issues/42752)；NIXL 存储测试共享固定的 `/tmp` 目录（[#44613](https://github.com/sgl-project/sglang/pull/44613)，修复 PR 开放中）；hicache prefetch 超时预算在每次重试时重新发放（[#44614](https://github.com/sgl-project/sglang/pull/44614)，修复 PR 开放中）。

今日关闭：[#34340](https://github.com/sgl-project/sglang/issues/34340)（SM103 被误门控为 SM100 系列，cutedsl TGV BF16 GEMM / trtllm-gen MoE finalize）、[#36333](https://github.com/sgl-project/sglang/issues/36333)（来自断开流式客户端的僵尸请求）、[#21065](https://github.com/sgl-project/sglang/issues/21065)（CI 维护模式）、[#23494](https://github.com/sgl-project/sglang/issues/23494)（AMD Q2 路线图，标记为不活跃）、[#41303](https://github.com/sgl-project/sglang/pull/41303)（DeepSeek-V4.1 SWA 有界重放窗口）、[#44599](https://github.com/sgl-project/sglang/pull/44599)（ROCm 感知 diffusion 测试）。

## 6. 对应用开发者的意义

- **仔细验证 PD + 前缀缓存。** 如果你在 HiCache 下运行 disaggregated prefill，并带有共享 system prompt（常见的 agent 场景），[#44070](https://github.com/sgl-project/sglang/issues/44070) 意味着相同前缀可能产生细微错误的输出，且不会暴露任何错误。在修复前可考虑设置 `SGLANG_DISAGG_PREFILL_EARLY_SEND_CACHED_PREFIX=0`，并对共享前缀流量添加输出质量金丝雀。
- **针对断开连接引发的故障进行加固。** [#39216](https://github.com/sgl-project/sglang/issues/39216) 是客户端 abort 时整引擎崩溃，[#44369](https://github.com/sgl-project/sglang/issues/44369) 可能卡死传输 worker。网关应对连接错误进行重试，但要预期有一部分 abort 会导致整个副本宕机 — 运行 N+1 容量并自动化重启。
- **Agentic 工作负载有两个独立风险。** GLM-5.3-Flash 在多工具提示下（[#36669](https://github.com/sgl-project/sglang/issues/36669)）和 DFLASH 投机解码下（[#40843](https://github.com/sgl-project/sglang/issues/40843)）都会退化；为这些路由添加重复看门狗或禁用 DFLASH。另外，工具 schema 校验在 API 事件循环上每个工具约耗时 0.7 ms，因此 10–20 工具的 agent 在生成开始前要付出 7–14 ms（[#43274](https://github.com/sgl-project/sglang/pull/43274) 是审查中的修复）。
- **SM120 部署是实战检验最少的路径。** 多份独立报告都给出相同建议：triton 是可靠的 attention 后端，fa4 会破坏 CUDA-graph capture，且 GLM-5.3-Flash 在投产前仍需要一组修复（[#42012](https://github.com/sgl-project/sglang/issues/42012)、[#37813](https://github.com/sgl-project/sglang/issues/37813)、[#40877](https://github.com/sgl-project/sglang/issues/40877)）。
- **如果你使用 AMD/MI355X，有低成本收益可拿** — FP8 quant+GEMM 融合（[#42229](https://github.com/sgl-project/sglang/pull/42229)）正针对交互式 agent 所处的低并发 EAGLE decode 场景。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 摘要 — 2026-10-16

数据快照：过去 24 小时内更新了 149 个 PR 和 47 个 issue。

## 1. 今日亮点
- **v0.7.0 发布**，带来 LiliCorr 投机解码、GLM5-Next 的 MTP，以及通过 `--moe-cache-mib` 启用的主机内存 MoE 专家缓存；据报道，在专家被卸载时可获得最高 **约 2.2 倍解码加速** ([v0.7.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.7.0), [b11669](https://github.com/ggml-org/llama.cpp/releases/tag/b11669))。
- **模型支持扩展**：新增 Muse Spark Open（文本/视觉/音频潜在 MoE）([#30616](https://github.com/ggml-org/llama.cpp/pull/30616))、maion-coder/draft-model 支持 ([#29778](https://github.com/ggml-org/llama.cpp/pull/29778))，以及 v0.7.0 中的 K2 Horizon dense/MoVA 和 Prism。
- **后端修复已落地**：Vulkan 并发设备初始化 ([#29766](https://github.com/ggml-org/llama.cpp/pull/29766))、AMD Vulkan f16 KV 跨步 ([#27703](https://github.com/ggml-org/llama.cpp/pull/27703)) 以及 Metal BF16 tensor-API flash attention ([#30491](https://github.com/ggml-org/llama.cpp/pull/30491))；围绕 MTP 多 GPU 和 Metal MoE 段错误的活跃回归仍然存在。

## 2. 发布与破坏性变更
- **v0.7.0 / b11669** — 版本号提升并发布，包含 LiliCorr 投机解码、GLM5-Next MTP、`--moe-cache-mib`、K2 Horizon（dense 与 MoVA）、Prism 以及其他模型更新 ([v0.7.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.7.0), [PR #30603](https://github.com/ggml-org/llama.cpp/pull/30603))。
- **b11671** — 新增 Muse Spark Open，支持文本、视觉和音频；采用潜在空间 MoE，残差流 8192，4096 中 256 个专家，top-8 sigmoid 路由 ([b11671](https://github.com/ggml-org/llama.cpp/releases/tag/b11671), [PR #30616](https://github.com/ggml-org/llama.cpp/pull/30616))。
- **b11658** — 新增 maion-coder 架构、draft-model 支持，并移除 maion 上下文移位处理 ([b11658](https://github.com/ggml-org/llama.cpp/releases/tag/b11658), [PR #29778](https://github.com/ggml-org/llama.cpp/pull/29778))。
- **b11662** — ggml 同步 ([b11662](https://github.com/ggml-org/llama.cpp/releases/tag/b11662))。
- **b11643** — 修复仅嵌入图变为空并导致输入未分配的问题 ([b11643](https://github.com/ggml-org/llama.cpp/releases/tag/b11643), [PR #30522](https://github.com/ggml-org/llama.cpp/pull/30522))。
- **b11642** — 服务器现在仅将可恢复的 slot 状态保存到提示缓存；与 rerank/embedding/completion 的 slot 行为相关 ([b11642](https://github.com/ggml-org/llama.cpp/releases/tag/b11642), [PR #30498](https://github.com/ggml-org/llama.cpp/pull/30498))。
- 未明确列出 API 破坏性变更。如果你依赖提示缓存和嵌入图行为，请检查这些路径。

## 3. 新模型与硬件支持
- **Muse Spark Open** — 文本/视觉/音频，潜在空间 MoE，残差 8192，4096 中 256 个专家，top-8 sigmoid 路由 ([PR #30616](https://github.com/ggml-org/llama.cpp/pull/30616))。
- **maion-coder** — 新架构及 draft-model 接线；移除了上下文移位函数 ([PR #29778](https://github.com/ggml-org/llama.cpp/pull/29778))。
- **K2 Horizon / Prism / GLM5-Next MTP** — 包含在 v0.7.0 模型批次中 ([v0.7.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.7.0))。
- **Metal** — 新增 tensor API flash-attention 内核的 BF16 变体，移至 `kernels/fa_tensor.metal` ([PR #30491](https://github.com/ggml-org/llama.cpp/pull/30491))。
- **Hexagon** — 重构张量重打包，支持 tiled/flat 量化及 FP 权重 ([PR #30635](https://github.com/ggml-org/llama.cpp/pull/30635))；提议支持 Q1_0 ([PR #30122](https://github.com/ggml-org/llama.cpp/pull/30122))。
- **Vulkan/AMD** — 跨步 f16 KV 拷贝仅执行一次，使读取分散到各内存通道 ([PR #27703](https://github.com/ggml-org/llama.cpp/pull/27703))。
- **mtmd/audio** — 新增 `mtmd_audio_stream` 和 voxtral-realtime 支持 ([PR #30629](https://github.com/ggml-org/llama.cpp/pull/30629))。
- **量化工具** — gguf-py 支持 Q1_0/Q2_0 numpy 反量化/量化 ([PR #30630](https://github.com/ggml-org/llama.cpp/pull/30630), [issue #30626](https://github.com/ggml-org/llama.cpp/issues/30626))。
- **转换** — pplx-decider 支持纯文本检查点 ([PR #30575](https://github.com/ggml-org/llama.cpp/pull/30575))；修复 Granite Switch 块数问题 ([PR #30621](https://github.com/ggml-org/llama.cpp/pull/30621), [issue #30617](https://github.com/ggml-org/llama.cpp/issues/30617))；修复 Granite speech `conv_norm` 映射回归 ([PR #30628](https://github.com/ggml-org/llama.cpp/pull/30628))。

## 4. 性能与优化
- **MoE 主机缓存** — v0.7.0 中的 `--moe-cache-mib` 在专家被卸载时可提供最高 **约 2.2 倍解码** ([v0.7.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.7.0))。
- **投机解码** — v0.7.0 新增 LiliCorr 和 GLM5-Next MTP ([v0.7.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.7.0))。
- **Vulkan** — 修复并发首设备初始化；测试现在并行加载模型以覆盖该场景 ([PR #29766](https://github.com/ggml-org/llama.cpp/pull/29766))。
- **Vulkan/AMD** — f16 KV 跨步拷贝减少内存通道热点 ([PR #27703](https://github.com/ggml-org/llama.cpp/pull/27703))。
- **Metal** — BF16 tensor-API flash attention 内核提升 BF16 路径覆盖 ([PR #30491](https://github.com/ggml-org/llama.cpp/pull/30491))。
- **服务器提示缓存** — 仅保存可恢复的 slot 状态，减少 rerank/embedding 任务造成的缓存污染 ([PR #30498](https://github.com/ggml-org/llama.cpp/pull/30498))。
- **`--moe-cache-topk`** — 已关闭的 PR 提议限制主机缓存 MoE 层上的活跃专家数 ([PR #30593](https://github.com/ggml-org/llama.cpp/pull/30593))。
- **Vulkan FA 反量化** — PR 检查合并后的 K+V f16 暂存区大小是否超过设备限制 ([PR #30487](https://github.com/ggml-org/llama.cpp/pull/30487))。
- **回归观察** — draft-MTP 在多 GPU 层拆分时使提示处理大约减半 ([issue #27428](https://github.com/ggml-org/llama.cpp/issues/27428))；Vulkan 多设备 + CPU-MoE 的提示处理在 #29741 后变慢 ([issue #30547](https://github.com/ggml-org/llama.cpp/issues/30547))；qwen35 在 RTX 5090 上于原生 Linux 达到约 76% roofline ([issue #28196](https://github.com/ggml-org/llama.cpp/issues/28196))。

## 5. 稳定性与回归
按严重程度排序：

- **严重 — Metal 段错误**：Nemotron 3.5 Bolt 74B-A9B 在 Metal 上于 `ggml_metal_op_mul_mat_id` 崩溃 ([issue #30610](https://github.com/ggml-org/llama.cpp/issues/30610))。
- **严重 — llama-server VRAM DoS**：服务器在某些请求下可能耗尽 VRAM ([issue #30577](https://github.com/ggml-org/llama.cpp/issues/30577))。
- **严重 — 内存 DoS**：客户端设置的 `X-Conversation-Id` 会禁用断开连接取消，并累积无上限的可恢复流会话 ([issue #29498](https://github.com/ggml-org/llama.cpp/issues/29498))。
- **高 — MTP 提示处理回归**：draft-MTP 在多 GPU 层拆分时使 PP 减半；单 GPU 正常 ([issue #27428](https://github.com/ggml-org/llama.cpp/issues/27428))。
- **高 — Vulkan 提示处理缓慢**：多设备 + `cpu-moe` MoE 在 #29741 后出现回归 ([issue #30547](https://github.com/ggml-org/llama.cpp/issues/30547))。
- **高 — `launch_fattn` 中的断言**：RX6950XT 上的 HIP/ROCm ([issue #27674](https://github.com/ggml-org/llama.cpp/issues/27674))。
- **高 — Vulkan MTP 断言**：在张量拆分下进行投机 draft 解码时出现 `GGML_ASSERT(neq0 == HSK)` ([issue #29418](https://github.com/ggml-org/llama.cpp/issues/29418))。
- **高 — CUDA 地址未对齐**：RTX 5090 Laptop 上长提示触发 `ggml_cuda_op_unary<op_sigmoid>` ([issue #28877](https://github.com/ggml-org/llama.cpp/issues/28877))。
- **高 — Vulkan 生成极慢**：自 b11594 以来出现性能回归 ([issue #30552](https://github.com/ggml-org/llama.cpp/issues/30552))。
- **今日已修复/关闭**：Vulkan MiniMax-M2 在长上下文/ubatch >128 时提示崩溃 ([issue #30364](https://github.com/ggml-org/llama.cpp/issues/30364))；Granite Switch 转换块数错误 ([issue #30617](https://github.com/ggml-org/llama.cpp/issues/30617), 修复 [PR #30621](https://github.com/ggml-org/llama.cpp/pull/30621))；slot KV 恢复空操作 ([issue #26676](https://github.com/ggml-org/llama.cpp/issues/26676))；rerank 提示缓存内存增长 ([issue #26293](https://github.com/ggml-org/llama.cpp/issues/26293))。
- **进行中的修复 PR**：SYCL IQ3 崩溃修复 ([PR #30604](https://github.com/ggml-org/llama.cpp/pull/30604))；组合 draft-MTP + 外部 draft 初始化崩溃 ([PR #27897](https://github.com/ggml-org/llama.cpp/pull/27897), 相关 [issue #27839](https://github.com/ggml-org/llama.cpp/issues/27839))；Vulkan FA 反量化合并 K+V 暂存区检查 ([PR #30487](https://github.com/ggml-org/llama.cpp/pull/30487))。

## 6. 对应用开发者的意义
- **升级到 v0.7.0 以获得 MoE 卸载和投机解码**，但由于多 GPU 提示处理回归，生产环境前需在多 GPU 上验证 MTP ([issue #27428](https://github.com/ggml-org/llama.cpp/issues/27428))。
- **服务器 API 正在扩展**：提议通过 `/v1/realtime` WebSocket 实现实时转录 ([PR #30636](https://github.com/ggml-org/llama.cpp/pull/30636))；embeddings `dimensions` 支持已在 PR 中 ([PR #24898](https://github.com/ggml-org/llama.cpp/pull/24898), [issue #25210](https://github.com/ggml-org/llama.cpp/issues/25210))；提议 HF 模型发现/下载 UI ([PR #29584](https://github.com/ggml-org/llama.cpp/pull/29584))。
- **提示缓存行为已变更** — 仅保存可恢复的 slot 状态，因此 rerank/embedding 工作负载不应再污染 completion 缓存 ([PR #30498](https://github.com/ggml-org/llama.cpp/pull/30498))。
- **新模型集成需要检查模板** — K2 Horizon 在低/中推理强度下可能将最终文本/工具调用留在 `reasoning_content` 中 ([issue #30246](https://github.com/ggml-org/llama.cpp/issues/30246))；Qwen3-Coder 推理/内容分隔符修复仍开放 ([PR #30557](https://github.com/ggml-org/llama.cpp/pull/30557))。
- **后端运维者应谨慎固定版本/测试**：Metal 存在 MoE 段错误 ([issue #30610](https://github.com/ggml-org/llama.cpp/issues/30610))；Vulkan 存在 MTP/tensor-split 和 CPU-MoE 回归 ([issue #29418](https://github.com/ggml-org/llama.cpp/issues/29418), [issue #30547](https://github.com/ggml-org/llama.cpp/issues/30547))；CUDA sm_120 存在长提示 sigmoid 问题 ([issue #28877](https://github.com/ggml-org/llama.cpp/issues/28877))。
- **面向不可信客户端的部署应缓解 DoS 风险**：VRAM 耗尽 ([issue #30577](https://github.com/ggml-org/llama.cpp/issues/30577)) 以及通过 `X-Conversation-Id` 导致的可恢复流内存增长 ([issue #29498](https://github.com/ggml-org/llama.cpp/issues/29498))。
- **量化/工具收益**：Q1_0/Q2_0 numpy 支持改进 gguf-py 工作流 ([PR #30630](https://github.com/ggml-org/llama.cpp/pull/30630))；Hexagon Q1_0 和重打包工作仍在继续 ([PR #30122](https://github.com/ggml-org/llama.cpp/pull/30122), [PR #30635](https://github.com/ggml-org/llama.cpp/pull/30635))。

---

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 摘要 — 2026-10-16

## 1. 今日亮点

过去 24 小时内没有新版本发布，但 issue 跟踪器主要被 **agent/tool-calling 正确性问题** 和 **模型拉取回归** 占据：据报告，原生 `POST /api/chat` 路径会在工具定义到达模型之前将其剥离，而多个 0.40.x 用户无法通过 SOCKS 代理拉取模型，或反复遇到重定向/重试失败。积极的一面是，上游 llama.cpp 集成工作取得进展——MiniCPM-V 4.7 projector 支持和 Q2_0 量化均有修复 PR，GGUF v4 解析改进（64KB DMA 对齐）也已提出。

---

## 2. 发布与破坏性变更

过去 24 小时内没有发布任何版本。以下为进行中 PR 的背景信息：

- **llama.cpp 升级至 b11669** ([#19032](https://github.com/ollama/ollama/pull/19032), 已关闭) — 底层运行时变更；注意，正是 b11669 启用了 Q2_0 GGUF 支持。
- **Q2_0 GGUF 张量/文件类型支持** ([#18967](https://github.com/ollama/ollama/pull/18967), 已关闭) — 此前 `ollama create`/模型加载会失败并报 `unsupported tensor "token_embd.weight" size overflows`；新增张量类型 42 / 文件类型 41（64 个权重，每块 18 字节）。在旧版本上导入的现有 Q2_0 GGUF 将需要新运行时。
- 未宣布任何 API/配置弃用或迁移步骤。（[#8018](https://github.com/ollama/ollama/pull/8018) —— 将 `/delete` 改为 POST —— 是一个旧的、已关闭的 PR，并非当前变更。）

---

## 3. 新模型与硬件支持

- **MiniCPM-V 4.7 (`openbmb/minicpm-v4.7:35b-a3b`)** — 加载失败 `unknown projector type: minicpmv4_7` ([#18983](https://github.com/ollama/ollama/issues/18983), 已关闭) 已通过将固定的 llama.cpp 从 b11521 → b11551 更新修复 ([#19024](https://github.com/ollama/ollama/pull/19024))。
- **Q2_0 量化** — GGUF reader 现已理解 Q2_0 ([#18967](https://github.com/ollama/ollama/pull/18967))；这对极低位模型导入（例如 Prism ML 发布）很重要。
- **GGUF v4 规范** — 提议为 GPU DMA 引入 64KB（65536）页对齐，以实现零拷贝张量映射，并支持 O(1) token-span seek ([#19037](https://github.com/ollama/ollama/pull/19037))。
- **视觉模型** — 新增 Clippy Vision ([#19033](https://github.com/ollama/ollama/pull/19033))。
- **云** — Clef 现已在 Ollama Cloud 上可用 ([#19036](https://github.com/ollama/ollama/pull/19036))。
- **后端说明** — 没有新的 CUDA/ROCm/Metal 后端支持落地；若干 ROCm/CUDA 问题仍处于开放状态（见下文）。

---

## 4. 性能与优化

- **CPU 线程/cgroup 不匹配** — `n_threads` 由宿主机核心数推导，忽略 cgroup v2 `cpu.max` 和 `cpuset`。在 CPU 受限容器中，这会导致自旋等待与 CFS 节流形成护送效应，据报告造成 **约 45 倍吞吐量崩塌**。([#17916](https://github.com/ollama/ollama/issues/17916))
- **MLX 引擎为单请求串行** — 无论 `OLLAMA_NUM_PARALLEL` 如何设置，请求都严格按 FIFO 处理，没有连续批处理。一个 issue 已关闭 ([#17666](https://github.com/ollama/ollama/issues/17666))，而批处理/并行解码的功能请求仍处于开放状态 ([#17280](https://github.com/ollama/ollama/issues/17280), 👍 4)。
- **回归报告：文本分类器模型在 0.40.4 上比 0.40.3 更慢**（nimble、clef-flash、tev1）— 尚未确认，需要基准测试。([#19002](https://github.com/ollama/ollama/issues/19002))
- **GGUF v4 seek/对齐** — 提议 64KB DMA 对齐和 O(1) 批量 token-span 丢弃，以加快模型检查/模型操作。([#19037](https://github.com/ollama/ollama/pull/19037))

---

## 5. 稳定性与回归（按严重程度排序）

**严重 — 正确性/数据完整性**
- **云端生成退化：`glm-5.3-flash:cloud` 有 5–8% 的请求失败**，并报告出现宿主机侧数据区域被清零。长期存在，22 条评论，未关联修复。([#18980](https://github.com/ollama/ollama/issues/18980))
- **原生 `/api/chat` 在整个安装范围内将工具定义渲染为空** — 模型会回显 `{"name": ""}`/`{{ <nil> <nil> [] {}}}`，且从不发出 `tool_calls`；`/v1` 能正确渲染工具。对 agent 影响很大。([#19040](https://github.com/ollama/ollama/issues/19040))
- **Go API 工具调用参数会静默丢失数值精度**（普通 + 流式路径）— 已有修复 PR。([#18942](https://github.com/ollama/ollama/issues/18942), fix [#18941](https://github.com/ollama/ollama/pull/18941))

**高 — 崩溃/挂起**
- **RTX 5090 上的 CUDA 非法内存访问（MUL_MAT）**：Windows 上运行 Cohere MoE 时发生，随后 llama-server 崩溃（`0xc0000409`）。([#18642](https://github.com/ollama/ollama/issues/18642))
- **ROCm：`gemma4:26b-a4b`** 在将 experts 卸载到 CPU 时于 `hipGraphInstantiate` 中中止（OOM），随后每次加载都发生段错误；变通方案为 `GGML_CUDA_DISABLE_GRAPHS=1`。([#19015](https://github.com/ollama/ollama/issues/19015))
- **Qwen3.5 推理期间发生 A100 PCIe 总线丢失（Xid79）**：出现在 0.34.4/0.35.0 上，需要整节点重启恢复。([#19006](https://github.com/ollama/ollama/issues/19006))
- **解析器在孤立分隔符工具调用值上 panic**（`value[8:0]` 切片越界）— 修复 PR 开放中。([#17492](https://github.com/ollama/ollama/pull/17492))
- **工具调用开标签在流式分块边界处丢失**（DeepSeek3、Cogito、LFM2 解析器）— 修复 PR 通过 219 个测试。([#19028](https://github.com/ollama/ollama/pull/19028))
- **`gemma4` 在 `think: false` 时于工具响应后返回空回复** — 空 thought 块从未关闭。([#18861](https://github.com/ollama/ollama/issues/18861))

**中 — 拉取/传输与启动**
- **自 0.40.4 起，`ollama pull` 在使用 SOCKS 代理时失效**（0.34.4 上正常）— 已关闭。([#19035](https://github.com/ollama/ollama/issues/19035))
- **拉取失败：`redirect target not allowed` / R2 端点 DNS 解析** ([#18716](https://github.com/ollama/ollama/issues/18716)) 以及 **`max retries exceeded`** ([#18842](https://github.com/ollama/ollama/issues/18842))。
- **blob 下载期间磁盘已满却被误报为成功** — 传输层忽略了写入错误；修复 PR 开放中。([#18813](https://github.com/ollama/ollama/pull/18813))
- **回归：`timed out waiting for llama-server to start`**：使用 virtio-gpu/drm 原生上下文时出现；0.32.9 正常，0.32.10+ 损坏。([#18123](https://github.com/ollama/ollama/issues/18123))

**低 — 桌面/API 边缘情况**
- **Windows 托盘应用在端口 11434 已被占用时，会永远约每秒启动一次 `ollama serve`**。([#19013](https://github.com/ollama/ollama/issues/19013))
- **桌面应用内存泄漏** — 上下文菜单标签 C 字符串被分配后从未释放。([#19029](https://github.com/ollama/ollama/issues/19029))
- **云端 `mistral-large-4` 在 `/v1/responses` 上以 HTTP 400 拒绝 `role: "developer"`**，阻塞 Codex 风格客户端。([#19021](https://github.com/ollama/ollama/issues/19021))
- **`bge-m3` embedding 在某些 UTF-8 markdown 文件上返回 HTTP 500** `json: unsupported value: NaN`。([#15582](https://github.com/ollama/ollama/issues/15582))
- **依赖公告（SCA）：seroval `fromJSON()` Promise resolver 类型混淆** — 已在 ≥1.5.3 中修补。([#19034](https://github.com/ollama/ollama/issues/19034))
- **Windows 上缺少 Claude Desktop 开关。** ([#18164](https://github.com/ollama/ollama/issues/18164))

---

## 6. 这对应用开发者意味着什么

- **目前，对于工具调用 agent，优先使用 `/v1` 而非原生 `/api/chat`。** 据报告，原生路由会清空工具定义，而 OpenAI 兼容路由能正确渲染它们 ([#19040](https://github.com/ollama/ollama/issues/19040))。在发布前端到端验证工具 schema。
- **当 Go 客户端往返工具调用参数时，警惕静默数值截断** — 应用待合并的修复，或添加你自己的无损解码 ([#18942](https://github.com/ollama/ollama/issues/18942))。
- **流式解析器正在被加固**，以处理分块边界标签拆分和孤立分隔符 panic；如果你运行 DeepSeek3、Cogito、LFM2 或 FunctionGemma 模型，请在这些修复合并后固定到包含它们的构建 ([#19028](https://github.com/ollama/ollama/pull/19028), [#17492](https://github.com/ollama/ollama/pull/17492))。
- **Responses-API 兼容性正在改善**：现在历史记录中容忍来自外部提供商的 compaction 项 ([#19003](https://github.com/ollama/ollama/pull/19003))，并且对于非思考模型会忽略过期的 `reasoning.effort` ([#19010](https://github.com/ollama/ollama/pull/19010)) — 如果你在会话中途切换模型，这很有用。
- **容器化部署：显式设置 `OLLAMA_NUM_THREADS`。** 默认值忽略 cgroup 配额，在 CPU 受限的 pod 中可能造成高达约 45 倍的吞吐量损失 ([#17916](https://github.com/ollama/ollama/issues/17916))。
- **在 Apple Silicon 上，不要假设存在并发。** MLX runner 无论 `OLLAMA_NUM_PARALLEL` 如何都会将请求串行化；请据此设计你的排队/背压 ([#17280](https://github.com/ollama/ollama/issues/17280))。
- **关注云可靠性**：如果你通过 Ollama Cloud 路由，注意 `glm-5.3-flash:cloud`（报告 5–8% 失败率）和 `mistral-large-4` 拒绝 developer 角色的问题 ([#18980](https://github.com/ollama/ollama/issues/18980), [#19021](https://github.com/ollama/ollama/issues/19021))。
- **低位与多模态导入正在扩展** — Q2_0 GGUF 支持 ([#18967](https://github.com/ollama/ollama/pull/18967)) 和 MiniCPM-V 4.7 ([#19024](https://github.com/ollama/ollama/pull/19024)) 都需要运行时更新；在生产环境中依赖它们之前，请分阶段升级。

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 摘要 — 2026-10-16
来源：[BerriAI/litellm](https://github.com/BerriAI/litellm)

## 1. 今日亮点
LiteLLM 仅有的新发布是预发布版 `v1.107.0-dev.2` 和 `v1.106.0-rc.4`，两者都聚焦于 cosign Docker 镜像签名验证，而非功能变更。最严重的未关闭报告仍然是 v1.82.3 中的预算执行绕过（[#26672](https://github.com/BerriAI/litellm/issues/26672)）、Kubernetes 上持续的 RAM 增长/OOM（[#27954](https://github.com/BerriAI/litellm/issues/27954)），以及未认证客户端通过失败的认证尝试虚增支出表（[#42689](https://github.com/BerriAI/litellm/issues/42689)）。进行中的 PR 工作集中在 Rust/原生路由与 Python 的对齐（[#46764](https://github.com/BerriAI/litellm/pull/46764), [#46788](https://github.com/BerriAI/litellm/pull/46788), [#46787](https://github.com/BerriAI/litellm/pull/46787)）、重试/退避正确性（[#46730](https://github.com/BerriAI/litellm/pull/46730)）、WebSocket 支出核算（[#46768](https://github.com/BerriAI/litellm/pull/46768)），以及通过 ID-JAG/JWT-bearer 实现 MCP 跨应用访问（[#46748](https://github.com/BerriAI/litellm/pull/46748), [#46747](https://github.com/BerriAI/litellm/pull/46747), [#46745](https://github.com/BerriAI/litellm/pull/46745)）。

## 2. 发布与破坏性变更
- `v1.107.0-dev.2` — dev 预发布版；发布说明仅涵盖 Docker cosign 验证。[发布](https://github.com/BerriAI/litellm/releases/tag/v1.107.0-dev.2)
- `v1.106.0-rc.4` — RC 预发布版；同样的 cosign 验证说明。[发布](https://github.com/BerriAI/litellm/releases/tag/v1.106.0-rc.4)
- 发布说明中未描述破坏性 API/配置变更。运维提示：所有 LiteLLM Docker 镜像均使用 cosign 签名；请使用 [commit `0112e53`](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0) 中的密钥进行验证。
- 留意尚未发布、即将到来的行为变更：重试将遵循 `Retry-After`，并使用指数退避+抖动，同时过滤瞬时错误（[#46730](https://github.com/BerriAI/litellm/pull/46730)）；token 计数新增 `litellm_count_source` / `litellm_count_fallback_model` 以及可配置 framing（[#46777](https://github.com/BerriAI/litellm/pull/46777)）；Rust 的 `/v1/messages` 和 `/v1/responses` body-param 过滤向 Python 规则靠拢（[#46764](https://github.com/BerriAI/litellm/pull/46764), [#46788](https://github.com/BerriAI/litellm/pull/46788), [#46787](https://github.com/BerriAI/litellm/pull/46787)）。

## 3. 新模型与硬件支持
- 本窗口期内没有新的 CUDA/ROCm/Metal/CPU 后端、加速器或量化格式。
- Bedrock Claude Platform：PR [#46670](https://github.com/BerriAI/litellm/pull/46670) 在 web identity 会话策略中授予 `sts:GetWebIdentityToken` 和 `sts:TagGetWebIdentityToken`，修复 OIDC 认证的 403。
- OpenAI 音频：PR [#44021](https://github.com/BerriAI/litellm/pull/44021) 将 `gpt-transcribe` 路由到 GPT 音频转录配置；修复缺少 `response_format` 导致的 400 和健康检查失败。
- Anthropic：PR [#46754](https://github.com/BerriAI/litellm/pull/46754) 在 `/v1/responses` 上将 Anthropic compaction 块以 compaction item 形式往返传递，防止重复进行全历史摘要。
- Token 计数：PR [#46777](https://github.com/BerriAI/litellm/pull/46777) 新增可配置的 `token_count_framing` 和 `token_count_fallback_model`，以及计数来源报告；针对 Bedrock Sonnet 5.5 / Haiku 5.5 的 404 和 Kimi K3 / DeepSeek 的估算 framing。
- Router 上下文：PR [#46197](https://github.com/BerriAI/litellm/pull/46197) 允许在成本映射一致时，由精确的 `base_model` 放宽部署的上下文窗口。
- 仍开放的模型/后端 bug：Databricks gpt-5.6 系列 tool + `reasoning_effort`（[#43708](https://github.com/BerriAI/litellm/issues/43708)）；带 `openai/` 前缀的自托管模型在 `/v1/messages` 上被路由到 Responses API，破坏多模态（[#40780](https://github.com/BerriAI/litellm/issues/40780)）。已关闭：Moonshot/Kimi K2.6 reasoning-content 工具调用问题（[#26156](https://github.com/BerriAI/litellm/issues/26156)）；OCI Grok/coding-agent 工具 schema 问题（[#31449](https://github.com/BerriAI/litellm/issues/31449)）。

## 4. 性能与优化
- CLI 冷启动：PR [#46803](https://github.com/BerriAI/litellm/pull/46803) 启动 `lite`/`litellm-proxy` 时不再导入完整 LiteLLM SDK；在 1 CPU VM 上避免约 3,000 个模块和 1.5–4 秒，有客户案例报告每条命令最高可达 20 秒。
- 网关负载测试：PR [#46806](https://github.com/BerriAI/litellm/pull/46806) 新增负载测试套件、Docker 构建开关和 `TCP_NODELAY`；`FAST=1` 跳过 LTO，将构建时间从约 13 分钟降至约 5 分钟。
- 每请求性能核算：PR [#46798](https://github.com/BerriAI/litellm/pull/46798) 使 `postgres_calls_per_request` 和 `redis_calls_per_request` 变为每请求统计，而非整服务器总计；PR [#46774](https://github.com/BerriAI/litellm/pull/46774) 将性能指标/限制导出为 JUnit 属性。
- 重试行为：PR [#46730](https://github.com/BerriAI/litellm/pull/46730) 将近乎瞬时重试（<0.2ms）替换为感知 `Retry-After` 的指数退避+抖动，并过滤 401 等不可恢复错误。
- 缓存：issue [#28427](https://github.com/BerriAI/litellm/issues/28427) 已关闭 — prompt-caching router affinity TTL 被硬编码为 5 分钟，破坏了 1 小时临时缓存路由。
- 内存压力：issue [#27954](https://github.com/BerriAI/litellm/issues/27954) 仍开放 — Kubernetes 中 LiteLLM pod 会达到 RAM 上限并随时间崩溃；在修复落地前请持续监控。
- WebSocket 支出：PR [#46768](https://github.com/BerriAI/litellm/pull/46768) 对每个原生 Responses WebSocket 轮次写入一条 spend-log 记录，而不是在 socket 关闭时写一条，避免 pod 重启时丢失支出。

## 5. 稳定性与回归
按严重程度排序。

**严重**
- [#27954](https://github.com/BerriAI/litellm/issues/27954) 开放 — 严重的 RAM 增长导致 Kubernetes pod OOM/崩溃；8 👍。未引用修复 PR。
- [#26672](https://github.com/BerriAI/litellm/issues/26672) 开放 — v1.82.3 中，key/user 的 `max_budget` 预算执行被绕过，即使支出超过限制。
- [#42689](https://github.com/BerriAI/litellm/issues/42689) 开放 — 每次使用未知 key 的失败认证都会写入各自的支出行，因此未认证客户端可以虚增用量表。
- [#26399](https://github.com/BerriAI/litellm/issues/26399) 已关闭 — Bedrock passthrough 路由绕过了 `key.models` / `user.models` 允许列表。

**高**
- [#38358](https://github.com/BerriAI/litellm/issues/38358) 开放 — 当上游接受 TCP 但不发送首字节时，`litellm_settings.request_timeout` 从不触发。
- [#32357](https://github.com/BerriAI/litellm/issues/32357) 开放 — `/v1/messages` 适配器错误编码推理模型：文本块内的 `thinking_delta` 和重复的 `message_start` 导致 Anthropic SDK / Claude Code 内容为空。
- [#31909](https://github.com/BerriAI/litellm/issues/31909) 开放 — `mcp_semantic_tool_filter` 会为标准的嵌套 OpenAI `tools` 格式剥离所有工具。
- [#46403](https://github.com/BerriAI/litellm/issues/46403) 已关闭 — 当提供商复用响应 ID（Ollama `chatcmpl-<0..999>`）时，SpendLogs 行被静默丢弃；`request_id` 主键冲突。
- [#46407](https://github.com/BerriAI/litellm/issues/46407) 开放 — `gpt-image` 成本忽略 EU 数据驻留加价和部署级 `output_cost_per_image_token`。
- [#40780](https://github.com/BerriAI/litellm/issues/40780) 开放 — 带 `openai/` 前缀的自托管模型将 `/v1/messages` 路由到 Responses API，静默丢失多模态支持。
- [#43708](https://github.com/BerriAI/litellm/issues/43708) 开放 — 在 `/chat/completions` 上，Databricks 托管的 gpt-5.6 系列函数工具因 `reasoning_effort` 错误而失败。

**中 / 启动与 MCP**
- [#30425](https://github.com/BerriAI/litellm/issues/30425) 开放 — 配置数据库后，代理启动时出现 `Failed to import Prisma client`。
- [#33431](https://github.com/BerriAI/litellm/issues/33431) 开放 — 编辑配置中的 MCP 服务器身份字段会静默孤立现有授权。
- [#33492](https://github.com/BerriAI/litellm/issues/33492) 开放 — OpenAPI-spec MCP 无法将 `extra_headers` 转发到上游后端。
- [#33671](https://github.com/BerriAI/litellm/issues/33671) 开放 — 独立 Bedrock Guardrails 集成从不发送 AWS `trace` 参数，丢失评估细节。
- [#46764](https://github.com/BerriAI/litellm/pull/46764) 审查中的高风险 PR — Rust `/v1/messages` body-param 对齐；[#46788](https://github.com/BerriAI/litellm/pull/46788), [#46787](https://github.com/BerriAI/litellm/pull/46787) 为较低风险的 Rust 桥接对齐工作。

**窗口期内已关闭的正确性修复**
- [#25429](https://github.com/BerriAI/litellm/issues/25429) `chatgpt/gpt-5.4` 最终 Responses 输出为空 / completion bridge 失败。
- [#14004](https://github.com/BerriAI/litellm/issues/14004) 当预算超限时，免费/无成本自托管模型被阻止。
- [#31447](https://github.com/BerriAI/litellm/issues/31447) `team_member_budget` 替换了整个团队元数据对象。
- [#27038](https://github.com/BerriAI/litellm/issues/27038) `disable_end_user_cost_tracking` 未控制 SpendLogs / DailyEndUserSpend 写入。
- [#31587](https://github.com/BerriAI/litellm/issues/31587) 当 `ANTHROPIC_API_KEY` 缺失/无效时，`/v1/skills` 返回 500。
- [#26156](https://github.com/BerriAI/litellm/issues/26156) Moonshot/Kimi K2.6 多轮工具调用丢失 `reasoning_content`。
- [#27184](https://github.com/BerriAI/litellm/issues/27184) Bedrock Claude 4.6 `tool_choice.type` required 错误。
- [#37175](https://github.com/BerriAI/litellm/issues/37175) Anthropic `/v1/messages` → Responses 适配器丢弃图像 `detail` 并错误标记 `thinking`。
- [#31449](https://github.com/BerriAI/litellm/issues/31449) 来自 coding agents 的 OCI 提供商工具请求失败。
- [#28444](https://github.com/BerriAI/litellm/issues/28444) Post API hook 未对 passthrough 端点启用。
- [#24628](https://github.com/BerriAI/litellm/issues/24628) S3 文件名过长。
- [#25456](https://github.com/BerriAI/litellm/issues/25456) Chat completions → Responses bridge 未处理带 URL 的 `file_id`。
- [#25191](https://github.com/BerriAI/litellm/issues/25191) 通过 `/chat/completions` 访问 Bedrock 时未触发 `websearch_interception`。
- [#45411](https://github.com/BerriAI/litellm/issues/45411) 尽管 Bedrock Invoke 支持，Claude Code 的 per-turn-control 仍对 Bedrock 被过滤。

## 6. 这对应用开发者意味着什么
- 预算/成本安全：如果你依赖预算、免费模型路由或终端用户支出跟踪，请针对 [#26672](https://github.com/BerriAI/litellm/issues/26672)、[#14004](https://github.com/BerriAI/litellm/issues/14004)、[#42689](https://github.com/BerriAI/litellm/issues/42689)、[#46403](https://github.com/BerriAI/litellm/issues/46403)、[#46407](https://github.com/BerriAI/litellm/issues/46407) 进行验证。为 SpendLogs 增长和 request-ID 冲突添加告警。
- Claude Code / Anthropic SDK：将 `/v1/messages` 视为仍在稳定中。跟踪 [#32357](https://github.com/BerriAI/litellm/issues/32357)、[#40780](https://github.com/BerriAI/litellm/issues/40780)、[#37175](https://github.com/BerriAI/litellm/issues/37175)，以及 Rust 对齐 PR [#46764](https://github.com/BerriAI/litellm/pull/46764)、[#46788](https://github.com/BerriAI/litellm/pull/46788)。在 #40780 修复前，避免通过 `/v1/messages` 使用带 `openai/` 前缀的自托管多模态模型。
- MCP/agents：ID-JAG/JWT-bearer 和 SSO 断言捕获即将到来（[#46748](https://github.com/BerriAI/litellm/pull/46748), [#46747](https://github.com/BerriAI/litellm/pull/46747), [#46745](https://github.com/BerriAI/litellm/pull/46745)），但在生产前请验证工具过滤（[#31909](https://github.com/BerriAI/litellm/issues/31909)）和 MCP 身份/请求头行为（[#33431](https://github.com/BerriAI/litellm/issues/33431), [#33492](https://github.com/BerriAI/litellm/issues/33492)）。
- 网关可运维性：预计 CLI 启动（[#46803](https://github.com/BerriAI/litellm/pull/46803)）、重试（[#46730](https://github.com/BerriAI/litellm/pull/46730)）、WebSocket 支出核算（[#46768](https://github.com/BerriAI/litellm/pull/46768)）和负载测试工具（[#46806](https://github.com/BerriAI/litellm/pull/46806)）会更好。对于 Kubernetes，请监控 RAM 增长（[#27954](https://github.com/BerriAI/litellm/issues/27954)）。
- Bedrock/模型兼容性：使用 [#46670](https://github.com/BerriAI/litellm/pull/46670) 处理 Claude Platform web identity；关注 [#33671](https://github.com/BerriAI/litellm/issues/33671) 的 Guardrails trace、[#43708](https://github.com/BerriAI/litellm/issues/43708) 的 Databricks gpt-5.6，以及 [#46777](https://github.com/BerriAI/litellm/pull/46777) 的 token 计数。
- Docker 供应链：对于 v1.106/v1.107 系列镜像，请使用 [commit `0112e53`](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0) 中的密钥验证 cosign 签名。

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 摘要 — 2026-10-16

## 1. 今日亮点

Unsloth 发布了 **v0.1.908-beta**，新增 NVIDIA Nemotron 3.5（包括可在 24 GB 内存上运行的 Bolt 变体）和 Meta Muse Spark Open，并加入文本扩散 LLM 支持、DeepSpeed 训练，以及为编码 agent 改进的提示词缓存。同一时间窗口还出现了一批密集的 Studio 基础设施 PR，目标包括多 GPU 放置、NVIDIA MIG 掩码、AMD 安装器路由和 ROCm attention 选择——其中许多仍处于开放和审查中。在 issue tracker 上，若干长期存在的 AMD/Studio bug 已关闭，而模型重新加载后的 KV-cache 持久化和多 GPU 微调仍然开放。

## 2. 发布与破坏性变更

- **v0.1.908-beta — NVIDIA Nemotron 3.5 + Meta Muse Spark Open**：新的模型家族、文本扩散 LLM、DeepSpeed 训练支持、为编码 agent 提供更好的提示词缓存。Beta 通道，因此请视为尚未固定用于生产。
- **安全/依赖基线已变动** — [PR #13949](https://github.com/unslothai/unsloth/pull/13949) 将 `pip scan-packages` 审计的基线重设为 **unsloth-zoo 2026.10.6** 和 **transformers 5.20.0**。上游文件变更使固定的扫描条目失效；如果你使用自有的审计门禁从源码构建，预计需要重设基线。
- **TRL 兼容性收窄** — [PR #13911](https://github.com/unslothai/unsloth/pull/13911) 除非 `fast_inference` 共享权重，否则将 TRL 的 vLLM 权重同步限制在 TRL < 0.28。之前的行为会在 TRL 0.22–0.27 上无条件将 `GRPOTrainer._move_model_to_vllm` 置为空操作，而这仅在 `fast_inference=True` 时才正确。如果你固定 TRL 版本，这一点很重要。
- 本时间窗口未宣布任何公共 API 或配置弃用。

## 3. 新模型与硬件支持

- **NVIDIA Nemotron 3.5 / Nemotron 3.5 “Bolt”** — 可在 24 GB 内存上运行；指南见 [unsloth.ai/docs/models/nemotron](https://unsloth.ai/docs/models/nemotro)。
- **Meta Muse Spark Open** — 同一 beta 版本中支持本地运行。
- **文本扩散 LLM** — v0.1.908-beta 中新增一等路径。
- **FLUX.3 Image Klein（distilled + base）、INT8/FP8/GGUF** — [PR #13933](https://github.com/unslothai/unsloth/pull/13933) 添加对 `black-forest-labs/flux-3-image-klein[-base]` 的加载支持，这些模型仅以 Modular Diffusers blocks 形式提供（`modular_model_index.json`，而非 `model_index.json`）。
- **旧版仓库的 4-bit 回退** — [PR #13941](https://github.com/unslothai/unsloth/pull/13941) 修复了 Tulu-3-8B/70B、SOLAR-10.7B、OpenThinker-7B、Hermes-3-Llama-3.1-405B、Pixtral-12B 在默认 4-bit 加载时的 “repository not found”，通过回退到基础模型实现。
- **AMD/ROCm**：针对 Strix APU + 独立显卡组合的安装器路由修复（[PR #13931](https://github.com/unslothai/unsloth/pull/13931)）、Windows `hipinfo` 双重掩码修复（[PR #13925](https://github.com/unslothai/unsloth/pull/13925)），以及 MIG 掩码/切片感知的大小计算（[PR #13927](https://github.com/unslothai/unsloth/pull/13927)）。
- **已请求但尚未支持**：Cohere Command A+ 218B/A25B（[#5952](https://github.com/unslothai/unsloth/issues/5952)，已关闭，未注明已支持）。

## 4. 性能与优化

- **并发聊天的 KV 抢占** — [PR #10301](https://github.com/unslothai/unsloth/pull/10301)：并行聊天共享一个 KV cache；llama-server 通过 `--preempt-ram` 负责停放/暂存，取代了每个聊天各自完整预填充的模式。这是对 [#9037](https://github.com/unslothai/unsloth/issues/9037) 中长期上下文重新预填充问题的结构性修复。
- **ROCm flash attention 选择** — [PR #13792](https://github.com/unslothai/unsloth/pull/13792) 用整块计时探测取代硬编码的模型族表，因此可在 gfx11（例如 Strix Halo gfx1151）上为任意模型形状选用 flash_attn，而不是总是回退到 torch SDPA/AOTriton。
- **编译下的 MoE 4-bit 反量化** — [PR #13946](https://github.com/unslothai/unsloth/pull/13946) 在 `UNSLOTH_MOE_SELECTIVE_DEQUANT=1` / `UNSLOTH_MOE_DEQUANT_CHUNK_EXPERTS=N` 时，将选择启用的 `bnb` 4-bit MoE experts 保持在 `torch.compile` 块之外。
- **混合代际 NVIDIA 主机** — [PR #13923](https://github.com/unslothai/unsloth/pull/13923) 根据所有可见 GPU 而非仅 device 0 来决定 `bfloat16`、compile 和 TMA，修复异构节点上静默的能力不匹配。
- **GPU 放置** — [PR #13928](https://github.com/unslothai/unsloth/pull/13928) 让图像/视频自动加载选择最空闲的 GPU，而不是 PCI slot 0。
- **Chat UI** — [PR #13919](https://github.com/unslothai/unsloth/pull/13919) 通过默认禁用代码行窗口化来修复滚动条卡死。
- **版本层面**：v0.1.908-beta 中的 DeepSpeed 训练路径，以及为编码 agent 重构的提示词缓存。

## 5. 稳定性与回归

按严重程度排序；若干高严重性项已有修复或已关闭。

1. **ROCm 预编译二进制在 `hipStreamCreateWithFlags` 处因 OOM 崩溃** — [#12205](https://github.com/unslothai/unsloth/issues/12205)（已关闭）：`b11160-mix-a6922cc` 的预编译 `linux/rocm` 资产会失败，而 *同一* 提交的源码构建则可正常工作。已由 AMD 安装器/架构路由 PR（#13931、#13925）缓解。
2. **Studio 多 GPU 大小计算错误与加载到错误显卡** — MIG UUID 掩码被当作“无掩码”，导致按完整的 40/80 GB 卡计算大小（[#13927](https://github.com/unslothai/unsloth/pull/13927)）；图像/视频自动加载跑在繁忙的 device 0 上（[#13928](https://github.com/unslothai/unsloth/pull/13928)）。修复 PR 开放中。
3. **长聊天在模型重新加载后重做完整预填充** — [#9037](https://github.com/unslothai/unsloth/issues/9037)（开放中）：slot KV 仅在空闲自动卸载时持久化。没有已合并的修复；#10301 是预期的补救方案。
4. **共享本地服务器上的多账号模型同步失败** — [#12365](https://github.com/unslothai/unsloth/issues/12365)（已关闭）：当前加载的模型在局域网用户之间无法保持一致。
5. **Qwen-Image-2.1 编辑期间 `diffusion_prompt_cache.py` 中出现 CUBLAS 错误** — [#13638](https://github.com/unslothai/unsloth/issues/13638)（已关闭）。
6. **Strix Halo（Windows）默认使用 Vulkan，预填充缓慢** — [#10656](https://github.com/unslothai/unsloth/issues/10656) 和 [#5405](https://github.com/unslothai/unsloth/issues/5405)（均已关闭）：GPU 未充分利用，或在 Docker 中回退到 CPU。
7. **AMD Docker 容器在 CPU 而非 Strix Halo APU 上运行** — [#5405](https://github.com/unslothai/unsloth/issues/5405)（已关闭）。
8. **Windows MXC Sandbox 启用失败，且说明不清晰** — [#13656](https://github.com/unslothai/unsloth/issues/13656)（开放中，未固定）。
9. **Model Hub 适配过滤器泄漏超出预算的模型** — [#13945](https://github.com/unslothai/unsloth/pull/13945)（修复开放中）以及过滤器行为 bug [#13613](https://github.com/unslothai/unsloth/issues/13613)（已关闭）。
10. **Vision-Wayland/NVIDIA 合成** — [#13892](https://github.com/unslothai/unsloth/pull/13892)（已关闭）：优先使用 dmabuf 而非静态关闭合成，以避免 Wayland Error 71。
11. **通过未列出的命令逃逸沙箱** — [PR #13942](https://github.com/unslothai/unsloth/pull/13942)（开放中）：在“Approve for me”下，`ffmpeg`/`pandoc` 绝对路径写到了沙箱之外，因为只检查了 `cp`/`mv`。应将 agent 沙箱视为尚不严密。
12. **Embeddinggemma-2 文件嵌入在索引阶段失败** — [#13663](https://github.com/unslothai/unsloth/issues/13663)（已关闭）。

## 6. 这对应用开发者意味着什么

- **24 GB 的 Nemotron 3.5 Bolt 是可行的单张消费级 GPU 服务目标。** 如果你正在为基于 Nemotron/Muse Spark 的 agent 规划推理主机，24 GB 档位现在是下限，而不再是 48–80 GB 显卡。
- **并行聊天/会话服务是当前活跃的工作流。** [#10301](https://github.com/unslothai/unsloth/pull/10301) 转向由一个 llama-server 持有一个 KV cache，并带抢占/停放机制。如果你在一个 Studio 实例上复用许多 agent 会话，预计每会话的有效上下文将取决于抢占策略——在该功能落地前，请有意固定你的 Studio 版本。
- **同构 GPU 假设并不安全。** [#13923](https://github.com/unslothai/unsloth/pull/13923) 和 [#13927](https://github.com/unslothai/unsloth/pull/13927) 都修复了此前仅依据 device 0 或整卡 VRAM 做出的决策。在混合代际或 MIG 分区的集群上，不要相信修复前构建的自动放置。
- **AMD 支持正在改善，但在安装时仍然脆弱。** Strix Halo CPU 回退和 ROCm OOM bug 已关闭，但修复属于安装器/架构路由变更——请重新构建或重新运行安装器，而不是复用现有的 ROCm wheels。
- **通过 Studio 进行多 GPU 微调仍在路线图上，尚未发布。** [#5764](https://github.com/unslothai/unsloth/issues/5764) 处于开放状态并标记为 `on roadmap`；目前请使用带 `device_map` 的 Python API 进行多卡 LoRA。
- **偏好调优的功能对等性仍是缺口。** [#5291](https://github.com/unslothai/unsloth/issues/5291) 请求将 GRPO 的分块/隐藏状态优化移植到 ORPO/DPO/CPO——请为这些训练器预留比 GRPO 更多的 VRAM。
- **沙箱尚不是安全边界。** 在 [#13942](https://github.com/unslothai/unsloth/pull/13942) 合并之前，不要依赖“Approve for me”模式来约束通过任意二进制文件写入的工具命令。
- **本周期工具链固定很重要**：transformers 5.20.0 和 TRL < 0.28 的行为变更正在进行中；请在 CI 中显式冻结它们。

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*