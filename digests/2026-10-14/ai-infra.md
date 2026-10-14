# AI 基础设施日报 2026-10-14

> 生成时间: 2026-10-14 01:21 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

**数据说明：** 在所提供的摘要中，只有 LiteLLM 和 Unsloth 公布了 24 小时 Issue/PR 总数。其他项目仅提供重点 Issue/PR，因此下文的“n/r”表示源摘要中未报告。

## 1. 生态概览

截至 2026-10-14，AI 基础设施栈正处于加固阶段：vLLM、SGLang、llama.cpp 或 Ollama 等主要服务引擎均未发布核心版本，而网关与微调层则有 LiteLLM `v1.106.0-rc.2` 和 Unsloth `v0.1.907-beta` 的动向。主导工作并非头条功能发布，而是围绕前沿混合模型——DeepSeek-V4、GLM-5.3、Qwen3.8、Kimi-K3、Mamba/GDN/MoE——的正确性与稳定性。性能工作正集中在 KV 缓存/前缀复用、持久化 kernel、PD 分离，以及跨 SM120、SM90、ROCm、Intel XPU、Arm 和 Hexagon 的硬件专用量化路径。与此同时，网关与本地运行时面临 tool-calling/MCP、结构化输出以及迁移/回归风险，因此对运维者而言，锁定版本与针对性集成测试是实际优先事项。

## 2. 活动对比

| 项目 | 24 小时 Issue 数 | 24 小时 PR 数 | 发布状态 | 活动特征 |
|---|---:|---:|---|---|
| **vLLM** | n/r | n/r | 无发布；`v0.31.0-cu129` 容器因 CUDA/torchcodec 不匹配而损坏 | 大量正确性/重构工作：Ampere DeepSeek-V4-Flash 阻塞项、前缀缓存+MTP 损坏、MoE LoRA CUDA graph 死锁、MonoKernel/DeepGEMM 重构 |
| **SGLang** | n/r | n/r | 无发布；无 API/配置迁移 | CI 不稳定问题占主导；Monokernel RFC、PD 分离路线图、stale-KV HiCache bug、prefill CP 扩展 |
| **llama.cpp** | n/r | n/r | 构建版本 `b11598`→`b11609`；聊天输出格式破坏性变更 | 安全加固、chat-template 分片数组输出、Arm SME2、Hexagon 电源模式、SYCL/Vulkan 工作 |
| **Ollama** | n/r | n/r | 无发布 | 吞吐量回退、云端 `glm-5.3-flash` 故障、旧版 GGUF 迁移 PR、tool-call+JSON bug |
| **LiteLLM** | 83 个有更新 | 500 个有更新 | `v1.106.0-rc.2` | 变动极高：MCP/tool schema、代理 UX、token 计数、遥测、provider 新增 |
| **Unsloth** | 147 个被触及 | 252 个被触及 | `v0.1.907-beta` | 以性能为重点的发布；Studio、视频/DiT LoRA、DGX Spark 集群、MoE 缓存放置 |

## 3. 模型支持竞赛

真正*已发布*的支持在此窗口内有限：大多数前沿模型工作仍处于开放 PR、阻塞项或优化路线图中。数据中心服务竞赛仍在 vLLM 与 SGLang 之间展开，而 llama.cpp 在本地/量化硬件覆盖面上领先。

| 项目 | 新模型/架构动向 | 硬件/后端动向 | 定位 |
|---|---|---|---|
| **vLLM** | DeepSeek-V4-Flash 在 Ampere/SM8x 上仍受阻；Qwen3.8-2.4T-A95B、Qwen3.8-Flash-Next、GLM-5.3、Kimi-K3、MiniMax-M3、混合 Mamba/GDN、MLA、MoE | Intel XPU MLA、AMD MI355X/gfx950 Quark MXFP4、SM120 缺口、NixlConnector 混合 MLA P/D、spec-decode draft 修复 | 前沿数据中心覆盖面最广，但 Ampere 上的 DeepSeek-V4 缺口是重大阻塞项 |
| **SGLang** | GLM-5/DeepSeek-V3.2 ROCm DCP、Qwen3.8-Flash-Next、Kimi-K3、Qwen3.5 文本 checkpoint、Ling-2.6 线性注意力 spec decode | ROCm、TRT-LLM MHA prefill CP、FlashInfer prefill CP、SM120 现场报告、T-Head SAIL/PPU | 强劲挑战者；在 PD/CP 矩阵与 ROCm 分离方面领先 |
| **llama.cpp** | MiniCPM-V 4.7、Maion-Coder、Qwen4exp/多模态 MTP、Laguna YaRN | Arm SME2 Q4_K/Q6_K、Qualcomm Hexagon 电源模式、Intel Vulkan FA、SYCL graph replay | 在本地/边缘量化与硬件覆盖面上明显领先 |
| **Ollama** | K2 Horizon 与 Snapdragon X Elite 仅处于请求阶段；Qwen3.5 thinking 默认值；MiniCPM-V 4.7 加载失败 | Metal kernel 的 CUDA 移植、Apple M5 Metal 修复、MLX 元数据/UI、CUDA 运行时打包 | 在新模型/硬件支持上落后；高度依赖 llama.cpp/MLX 上游 |
| **LiteLLM** | OpenInfer、Laya/Bespoke、MiniMax-H3/H3-Max 视频、Gemini 4 Argon 长解码、Databricks 消息塑形 | 无 CUDA/ROCm/Metal 变更 | 网关/provider 接口层领导者 |
| **Unsloth** | Wan2.2 视频 LoRA 训练、LTX-2.5 默认值、DGX Spark/GB10 双节点集群、H100 xformers 回退 | DGX Spark aarch64/sm_121a、Windows 旧驱动回退、H100 sm_90 | 微调/视频/Studio 领导者；不是数据中心服务引擎 |

**谁领先？** 在数据中心服务前沿模型方面，vLLM 与 SGLang 领先。vLLM 的前沿模型覆盖面更广，但在 Ampere 上的 DeepSeek-V4-Flash 受阻；SGLang 在 PD 分离、prefill 上下文并行与 ROCm 路径上更强。llama.cpp 仍是本地/量化运行时的基准。LiteLLM 领跑网关层。Unsloth 在微调、视频/DiT 以及 Studio 式边缘工作流方面领先。

## 4. 性能前沿

优化工作按层级与硬件呈现碎片化，有几个反复出现的主题：

- **KV 缓存与前缀复用：** vLLM 正在对抗混合 Mamba/GDN 上前缀缓存+MTP 损坏；在 SM120 上，NVFP4 KV 缓存比 FP8 慢 1.7–1.8×；在 GB200 上，针对 GLM-5.3 的 P/D KV 传输受 descriptor 限制。SGLang 正在修复 HiCache host-pool 分配以及一个会静默提前发送 stale KV 的 bug。llama.cpp 收紧了 `--cache-ram` 强制执行与 prompt 缓存淘汰。Unsloth 在 `v0.1.907-beta` 中声称实现 95% 缓存命中率与最高 5× 推理增益。
- **分布式/PD 服务：** vLLM 推进了 NixlConnector 混合 MLA 配对，但仍存在 TP worker 挂起以及 MoE+fully-sharded LoRA CUDA graph 死锁。SGLang 正在推进 Q4 PD 分离、ROCm DCP、prefill CP 自动选择，以及 Weight Cache Daemon，后者将 Qwen3-235B FP8 权重加载从约 306–327s 降至 `<1s`。Unsloth 新增了 DGX Spark 双节点集群与异步副本路由。
- **持久化 kernel 与 JIT 削减：** vLLM 的 MonoKernel RFC 与 DeepGEMM MegaMoE 工作瞄准逐层持久化 kernel 和更少的运行时编译。SGLang 的 Monokernel RFC 提出覆盖解码器层大部分过程的持久化 kernel。二者都表明业界正更广泛地摆脱启动开销沉重的 decode graph。
- **量化与硬件专用 kernel：** vLLM 正在平衡 FP8、NVFP4、Quark MXFP4 与 SM120/SM90 约束。SGLang 在 SM100 上存在 NVFP4 W4A4 marlin 崩溃，并有一条 AMD DSV4 FP8 验证路径。llama.cpp 新增了 Arm SME2 `Q4_K`/`Q6_K` matmul 与 Hexagon 电源控制。Unsloth 正在让 LTX-2.5 对齐 INT8-ConvRot，并跳过损坏的 H100 FA3 路径。
- **批处理/调度：** vLLM 正在修复 spec-decode token 预算与 rejection kernel 预热。SGLang 优化了 chunked-prefill mamba 状态跟踪与 prefill CP BCG 选择。Ollama 不感知 cgroup 的线程默认值可能使 CPU 容器吞吐量骤降约 45×。llama.cpp 在某些 Vulkan 路径上仍有 `ubatch > 128` 的长上下文失败。
- **网关性能：** LiteLLM 不是服务引擎，但其遥测/成本工作——`AggregatingSink`、固定桶直方图、Prometheus 标签修复、PTU 拆分、vLLM 透传支出日志——瞄准企业可观测性与成本准确性。

## 5. 层级定位

| 层级 | 项目 | 主要关注点 |
|---|---|---|
| **数据中心服务引擎** | vLLM、SGLang | GPU 利用率、前沿模型支持、PD 分离、MoE/MLA/混合正确性、分布式服务 |
| **本地/边缘运行时** | llama.cpp、Ollama | GGUF/量化推理、CPU/GPU/NPU 可移植性、桌面 UX、离线/边缘部署 |
| **LLM 网关** | LiteLLM | 多 provider 路由、认证、成本跟踪、MCP/tool schema、可观测性、provider 转换 |
| **训练/微调** | Unsloth | LoRA/QLoRA、Studio 工作流、视频/DiT 训练、DGX Spark 集群、导出到服务栈 |

重叠正在增加。Unsloth Studio 正在成为一个服务平面，具备 OpenAI/vLLM 兼容接口、MCP 安装以及双节点 DGX Spark 路由。Ollama 集成了 llama.cpp 与 MLX，但在上游模型支持上落后。LiteLLM 位于 vLLM、SGLang、Ollama 与托管 API 之前，因此其 bug 常常即使服务引擎健康也会暴露到生产环境。

## 6. 趋势信号

1. **混合模型正确性是首要风险。** 前缀缓存 + MTP、MLA LoRA 目标定位、PD HiCache 中的 stale KV，以及 MoE/TP/LoRA CUDA graph 死锁，都是静默损坏类 bug，而不是干净崩溃。服务运维者应禁用风险组合，直到修复落地。
2. **硬件碎片化正在加剧。** SM120 NVFP4 KV 回退、SM100 marlin 崩溃、SM90 `dsv4_topk` 问题、AMD ROCm DCP、Intel XPU MLA、Arm SME2、Qualcomm Hexagon、DGX Spark/GB10——不存在单一的“已验证”配置。请按硬件锁定技术栈。
3. **持久化 kernel 是下一个架构战场。** vLLM 与 SGLang 都有 Monokernel/MonoKernel RFC，目标是压缩解码层启动开销。这对低延迟、高并发的 agent 服务最为关键。
4. **PD 分离正从演示走向生产，但 KV 传输是瓶颈。** vLLM 在 GB200 上受 descriptor 限制的结果，以及 SGLang 的 stale-KV HiCache bug，表明分离的正确性与传输效率尚未解决。
5. **网关/工具链脆弱性是一等应用风险。** LiteLLM 的 MCP/tool-schema 问题、Ollama 的 `format: json` tool-call 失败，以及 vLLM/SGLang 上针对 Anthropic `/v1/messages` 的兼容工作，意味着 agent 开发者必须显式测试 tool schema、token 计数以及流式回退归因。
6. **发布纪律比版本新鲜度更重要。** vLLM 的 `v0.31.0-cu129` 容器已损坏；Ollama 报告相比 `v0.40.1` 出现 25–30% 吞吐量回退；LiteLLM 是 RC；Unsloth 是 beta。生产环境请锁定已知良好版本，并针对自己的 prompt 组合进行验证。

**Agent/应用开发者应关注什么：**
- 在 SGLang gRPC 上始终显式设置 `max_new_tokens`；未设置的值会静默限制为 128 个 token。
- 在 `#18975` 落地前，避免在 Ollama 上将 `format: json` 与 tool calls 一起使用。
- 若使用 LiteLLM，请验证 MCP/tool schema 与 `/v1/messages/count_tokens`；数组型 tool 参数当前会触发 `KeyError: 'items'`。
- 在 vLLM `v0.28.0` 上避免 MoE + TP + fully-sharded LoRA + CUDA graphs；在 SM120 上避免 NVFP4 KV 缓存；在 SM100 NVFP4 W4A4 上避免 `--fp4-gemm-backend marlin`。
- 审计 MLA LoRA 适配器是否在 DeepSeek/GLM/Kimi MLA 架构上静默未生效。
- 在 Unsloth `#13637` 落地前，重新验证 NVIDIA/AMD/Intel 上的 LoRA seed 可复现性。
- 将 llama.cpp 中 chat-template 输出格式变更视为 prompt 字符串消费方的破坏性集成点。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 摘要 — 2026-10-14

## 1. 今日亮点

过去 24 小时没有发布，但活动频繁：仓库中最热的帖子仍是 DeepSeek-V4-Flash 的 Ampere/SM8x 阻塞问题（[#50576](https://github.com/vllm-project/vllm/issues/50576)，110 条评论，16 👍），表明对 A100/A800 服务 V4 系列有真实需求。正确性工作主导了队列其余部分——混合 Mamba/GDN 模型上的前缀缓存 + MTP 损坏（[#53912](https://github.com/vllm-project/vllm/issues/53912)）以及全分片 MoE LoRA 的 CUDA-graph 死锁（[#60845](https://github.com/vllm-project/vllm/issues/60845)）是两个影响最大的未修复 bug。与此同时，两项大型架构重构取得进展：统一的 DeepGEMM MegaMoE 专家层（[#61538](https://github.com/vllm-project/vllm/pull/61538)）和用于逐层持久化内核的 MonoKernel 抽象（[RFC #60904](https://github.com/vllm-project/vllm/issues/60904)，[PR #61311](https://github.com/vllm-project/vllm/pull/61311)）。

## 2. 发布与破坏性变更

- **过去 24 小时无发布。**
- **容器回归（已发布产物）：** `vllm/vllm-openai:v0.31.0-cu129` 启动失败，因为捆绑的 `torchcodec 0.17.0` 在 CUDA 12.9 镜像中链接到了 CUDA 13（`libcudart.so.13`）——[issue #60406](https://github.com/vllm-project/vllm/issues/60406)。在修复前请固定到较早镜像或重新构建。
- **进行中的配置接口：** ROCm 新增线性后端选择器（`--linear-backend triton`，用于 per-token FP8 动态缩放），见 [PR #61091](https://github.com/vllm-project/vllm/pull/61091)；MoE 后端内部实现正在 [PR #61538](https://github.com/vllm-project/vllm/pull/61538) 中整合到单一 feature-matrix 层之后（DeepSeek V4/V4.1 + Kimi K3 MegaMoE），这可能会改变内部标志。

## 3. 新模型与硬件支持

- **Ampere / SM8x（A100/A800、RTX 30xx）仍无法服务 DeepSeek-V4-Flash** 或 `-0731` checkpoint；这是社区参与度最高的开放 tracker——[issue #50576](https://github.com/vllm-project/vllm/issues/50576)。
- **Intel XPU MLA：** XPU MLA 支持正在集成到现有 `FLASH_ATTN_MLA` 后端，而不是单独路径——[PR #58297](https://github.com/vllm-project/vllm/pull/58297)。
- **AMD MI355X / gfx950（Quark MXFP4）：** 针对 `Qwen3.8-2.4T-A95B` 的活跃优化 tracker——[issue #57149](https://github.com/vllm-project/vllm/issues/57149)——以及 `Qwen3.8-Flash-Next`（GatedDeltaNet + QuerySparse Attention + HyperConnection residual）——[issue #59575](https://github.com/vllm-project/vllm/issues/59575)。
- **MonoKernels RFC：** 提议约定 `vllm/models/<model>/<vendor>/`，其中层循环调用 `mono.forward_layer(...)`，以在单个持久化内核中启动整个 decode 层——[issue #60904](https://github.com/vllm-project/vllm/issues/60904)，草稿实现 [PR #61311](https://github.com/vllm-project/vllm/pull/61311)。
- **Spec-decode 草稿模型：** 异构词表草稿模型（`use_heterogeneous_vocab=True`）正在 Model Runner V2 上恢复——[PR #61536](https://github.com/vllm-project/vllm/pull/61536)；DFlash/DFlash2 全注意力草稿层已修复，以便对 speculators checkpoint 保持双向——[PR #61537](https://github.com/vllm-project/vllm/pull/61537)。
- **混合 MLA P/D 配对：** `NixlConnector` 现在可配对不可整除的混合 MLA 块大小（例如 ROCm 上的 Kimi-K3 P TP2 / D TP1）——[PR #61002](https://github.com/vllm-project/vllm/pull/61002)。
- **SM120 缺口：** `Inkling-Small-NVFP4` 失败，因为 `tml_fa4` 在 SM 12x 上缺少 paged-KV/split-KV——[issue #51405](https://github.com/vllm-project/vllm/issues/51405)；block-FP8 cutlass 路径排除了 SM89（Ada），尽管其具有原生 FP8 tensor core——[issue #58241](https://github.com/vllm-project/vllm/issues/58241)。
- **混合 SSM/GDN PD 分离** 对于 Qwen3.5-397B-A17B-W8A8 等模型仍是开放功能请求——[issue #43765](https://github.com/vllm-project/vllm/issues/43765)。

## 4. 性能与优化

- **NVFP4 KV cache 在 SM120 上是回归：** FlashInfer NVFP4 causal prefill 实测为 **~145.8 ms vs FP8**，即在消费级 Blackwell 上匹配的 48-kernel paged-prefill 段中**慢 1.7–1.8×**——[issue #50416](https://github.com/vllm-project/vllm/issues/50416)。目前建议在 SM120 上优先使用 FP8 KV cache。
- **P/D KV 传输受 descriptor 限制，而非带宽限制：** GB200 上的 GLM-5.3 每次 TP-rank 传输会发出 **91k–120k 个 KV descriptor**，使得 MNNVL/`cuda_ipc` 在此工作负载下比 RDMA 更慢——[issue #55434](https://github.com/vllm-project/vllm/issues/55434)。
- **Spec-decode 接受率回归：** 当混合 KV cache 管理器将滑动窗口草稿实体化为真实 SWA 组（BLHNC、NIXL P/D）时，DFlash2 损失 **~5.5 个接受点**——[issue #60560](https://github.com/vllm-project/vllm/issues/60560)；调度器 token 预算修复已在 [PR #59468](https://github.com/vllm-project/vllm/pull/59468) 中提出，CUDA top-k/top-p 拒绝内核预热见 [PR #59260](https://github.com/vllm-project/vllm/pull/59260)。
- **Qwen4Exp PLE 内存修复：** 将卸载的 PLE 表固定为其精确大小，使每 rank 主机内存从 **65.2 GiB → 47.8 GiB**，并让 TP2 Qwen4Exp 在 2× DGX Spark 上以 `--gpu-memory-utilization 0.78` 启动——[PR #59848](https://github.com/vllm-project/vllm/pull/59848)；decode 步骤冷页预读后续工作在 [PR #58835](https://github.com/vllm-project/vllm/pull/58835)。
- **ROCm FP8 线性路径：** 原生 per-token FP8 Triton scaled-MM 后端，并针对 RDNA4（gfx1201 / Radeon AI PRO R9700）调优——[PR #61091](https://github.com/vllm-project/vllm/pull/61091)。
- **YaRN 缓存大小计算：** 通用 YaRN 仅预计算 `original_max_position × factor` 行，在更大的 `max_position_embeddings` 下会越界读取——已通过至少按 `max_position` 确定大小修复——[PR #61495](https://github.com/vllm-project/vllm/pull/61495)。
- **JIT 消除 tracker** 继续推进：运行时 JIT 编译为零，DeepSeek V4 模型级 de-JIT 项仍开放——[issue #49349](https://github.com/vllm-project/vllm/issues/49349)。
- **AMD CI 对齐：** 请求将 MI300/MI325/MI355 加入 nightly 性能 + 准确率回归仪表盘，以匹配 B200/H200/GB200 的覆盖——[issue #43916](https://github.com/vllm-project/vllm/issues/43916)。

## 5. 稳定性与回归

按严重程度排序：

1. **CUDA graphs 死锁，fused-MoE + 跨 TP 的全分片 LoRA（v0.28.0）。** 在 TP=2/TP=4 下服务兼容 Qwen3.5 的 35B-A3B MoE，并搭配多个 LoRA adapter 和 `--fully-sharded-loras` 时出现间歇性挂起；TP=2 在并发上升前可服务。没有关联修复 PR——[issue #60845](https://github.com/vllm-project/vllm/issues/60845)。
2. **前缀缓存 + MTP 在混合 Mamba/GDN 模型上损坏输出（v0.28.0）。** 报告为已关闭 #43559 的未修复复发——[issue #53912](https://github.com/vllm-project/vllm/issues/53912)。在解决前，请在混合模型上禁用前缀缓存或 MTP。
3. **GLM-5.3-Flash ROCm kpool indexer 覆盖自身 KV cache**——后端宣称的内核块大小与内核实际使用不符；4096 个 pool 条目写入 864 个槽位，**约 79% 的 indexer key 错误**，导致长上下文静默退化。现已 **CLOSED**——[issue #54359](https://github.com/vllm-project/vllm/issues/54359)。
4. **GLM-5.2-FP8 + 前缀缓存 + FP8 KV cache 在重复相同提示时返回空内容/乱码（v0.27.1）**——[issue #60494](https://github.com/vllm-project/vllm/issues/60494)。
5. **面向 MLA `kv_b_proj` 的 LoRA 会被静默忽略，从未生效**——decode 通过 absorption 绕过它，prefill 通过过期的 pre-wrap 引用绕过；影响 DeepSeek V2/V3/R1/V3.2、GLM MoE DSA、Kimi K2——[issue #48974](https://github.com/vllm-project/vllm/issues/48974)。
6. **TP worker 挂起 → `EngineDeadError`，RPC `sample_tokens` 超时**，出现在 DeepSeek-V4-Pro、TP=8、MTP 投机解码下——[issue #41530](https://github.com/vllm-project/vllm/issues/41530)。
7. **`dsv4_topk` Triton 在高并发下于 H20（SM90）发生非法内存访问**，出现在 DeepSeek-V4.1-Flash 下；通过 `max_num_seqs=256` 缓解。**CLOSED**——[issue #56389](https://github.com/vllm-project/vllm/issues/56389)。
8. **`_flash_attn_fwd()` 意外的 kwarg `dynamic_causal`** 导致 main 上的 `minimax-m3` 崩溃——[issue #46914](https://github.com/vllm-project/vllm/issues/46914)。
9. **MiniCPM-V-4.6 在 v0.25.0 上加载失败**（`no module named 'qkqkv_proj'`）——[issue #48449](https://github.com/vllm-project/vllm/issues/48449)。
10. **V1 多进程引擎中偶发 0.7–2.0 秒的引擎步骤冻结**，约每个 generate 批次一次；设置 `VLLM_ENABLE_V1_MULTIPROCESSING=0` 后消失——[issue #48229](https://github.com/vllm-project/vllm/issues/48229)。
11. **Whisper 片段时间戳每段漂移约 0.5 秒**，针对超过 30 秒的音频——[issue #32588](https://github.com/vllm-project/vllm/issues/32588)。
12. **进行中的修复 PR：** 在逐层 RL 权重更新期间，attention sink 未通过 `weight_loader` 重新加载——[PR #57797](https://github.com/vllm-project/vllm/pull/57797)；更广泛的 reload/refit-equals-fresh 修复——[PR #61287](https://github.com/vllm-project/vllm/pull/61287)；AITER indexer 拒绝未构建的 index-head 数量，而不是崩溃——[PR #60990](https://github.com/vllm-project/vllm/pull/60990)。

## 6. 对应用开发者的意义

- **如果你服务混合 Mamba/GDN 模型（或任何带 MTP 的模型），请将前缀缓存视为不安全**，直到 [#53912](https://github.com/vllm-project/vllm/issues/53912) 落地——损坏是静默的，而不是崩溃。
- **审计 MLA 模型上的 LoRA adapter。** 在 DeepSeek/GLM/Kimi MLA 架构上，面向 `kv_b_proj` 的 adapter 不产生效果也没有警告（[#48974](https://github.com/vllm-project/vllm/issues/48974)）。如果你的微调似乎没有行为变化，这可能是根因之一。
- **避免在 v0.28.0 上组合使用 MoE + 张量并行 + `--fully-sharded-loras` + CUDA graphs**（[#60845](https://github.com/vllm-project/vllm/issues/60845)）；回退到非分片 LoRA 或单个 TP rank。
- **KV cache dtype 选择现在有硬件特定的权衡：** FP8 仍是安全默认；NVFP4 KV cache 目前在 SM120 上的 prefill 更*慢*（[#50416](https://github.com/vllm-project/vllm/issues/50416)）。
- **在 vLLM 上运行 Claude Code 的 Agent 开发者**应关注加固 `/v1/messages`（Anthropic API）的 RFC，以支持约 25 个工具的请求、嵌套 JSON schema 和交错思考——[issue #58647](https://github.com/vllm-project/vllm/issues/58647)。
- **多模态负载缩减：** token-in/token-out RFC 提议将 `pixel_values` 预处理推迟到 `/generate`，让你在分离式和聚合式拓扑中都能将 tokenization 从引擎节点卸载——[issue #46722](https://github.com/vllm-project/vllm/issues/46722)。
- **SDK 兼容性警告：** 从 `/v1/chat/completions` 响应中省略未设置的 vLLM 扩展字段的提议，会使 OpenAI SDK（`extra="allow"`）的 `resp.prompt_token_ids` 从 `None` 变成 `AttributeError`——见 [issue #53349](https://github.com/vllm-project/vllm/issues/53349) 中的更正。
- **Embedding/pooling 用户：** 在 0.29.0 中，`token_embed` 和 `token_classify` 请求仍被强制排除在前缀缓存读取之外——[issue #60947](https://github.com/vllm-project/vllm/issues/60947)。
- **固定你的容器 tag。** `v0.31.0-cu129` 镜像因 `torchcodec` 捆绑包与 CUDA 主版本不匹配而损坏（[#60406](https://github.com/vllm-project/vllm/issues/60406)）。

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 摘要 — 2026-10-14

## 1. 今日亮点

CI 稳定性工作主导了当天，flaky 测试跟踪器（[#42752](https://github.com/sgl-project/sglang/issues/42752)，53 条评论）活动量最大，因为 NVIDIA `PR Test Base`/`PR Test Extra` 运行继续暴露基础设施故障。技术方面，一个新的 [RFC] Monokernel 框架提案（[#43949](https://github.com/sgl-project/sglang/issues/43949)）已提交讨论，同时 PD Disaggregation（[#43699](https://github.com/sgl-project/sglang/issues/43699)）和 Prefill Context Parallelism（[#21788](https://github.com/sgl-project/sglang/issues/21788)）的 Q4 路线图继续充实。10-13 提交了三个新的正确性/崩溃 bug，且仍处于打开状态 —— 尤其是 PD prefill 配合 HiCache 时的静默 stale-KV 问题。

## 2. 发布与破坏性变更

过去 24 小时无新发布。未宣布 API/配置迁移。

有一个行为陷阱需要注意：原生 gRPC `Generate` 在未设置 `max_new_tokens` 时会将输出静默限制在 128 个 token（[#44121](https://github.com/sgl-project/sglang/issues/44121)）—— 在问题解决前，客户端应显式传入该参数。

## 3. 新模型与硬件支持

- **T-Head SAIL / PPU 路线图** —— 提议为 Zhenwu 810 / 810E / M890P 卡提供一等支持（[#37519](https://github.com/sgl-project/sglang/issues/37519)）。
- **面向 GLM-5 和 DeepSeek-V3.2 的 ROCm decode context parallel（DCP）** 正在评审中，在 `--dcp-size` 下将 MLA KV cache 跨 rank 分片（[PR #42618](https://github.com/sgl-project/sglang/pull/42618)）。
- **Prefill CP 支持矩阵扩展（Q4）** —— TRT-LLM MHA 后端已合入（[#31732](https://github.com/sgl-project/sglang/pull/31732)），GLM5.3-Flash 已合入（[#42044](https://github.com/sgl-project/sglang/pull/42044)）；FlashInfer 后端（[#33226](https://github.com/sgl-project/sglang/pull/33226)）、Qwen3.8-Flash-Next（[#39724](https://github.com/sgl-project/sglang/pull/39724)）和 Kimi-K3（[#38344](https://github.com/sgl-project/sglang/pull/38344)）仍待处理（[#21788](https://github.com/sgl-project/sglang/issues/21788)）。
- **Qwen3.5 纯文本 checkpoint** —— 已注册 `Qwen3_5ForCausalLM` / `Qwen3_5MoeForCausalLM`，使 `AutoModelForCausalLM` 风格的 SFT 配置可加载（[PR #27899](https://github.com/sgl-project/sglang/pull/27899)）。
- **Anthropic API 兼容性** —— `/v1/messages` 现在接受 `document` 内容块，用于 PDF/纯文本摄取（[PR #31179](https://github.com/sgl-project/sglang/pull/31179)）。
- **线性注意力的投机解码** —— 为 Lightning Attention（Ling-2.6-flash）新增可选的 cuLA KVBuffer 后端（[PR #29737](https://github.com/sgl-project/sglang/pull/29737)）。
- **SM120 现场报告** —— 持续收集 DeepSeek-V4.1-Flash 和 GLM-5.3-Flash 在 8× RTX PRO 6000 纯 PCIe 拓扑上的社区数据（[#40877](https://github.com/sgl-project/sglang/issues/40877)）。

## 4. 性能与优化

- **Weight Cache Daemon 路线图**（[#33522](https://github.com/sgl-project/sglang/issues/33522)）：第 1 阶段已通过按 rank 的 CUDA-IPC 权重服务，将 Qwen3-235B FP8 的权重加载从约 306–327 秒降至 **<1 秒**；路线图跟踪后续阶段，以实现引擎快速恢复。
- **[RFC] Monokernel 框架**（[#43949](https://github.com/sgl-project/sglang/issues/43949)）：提议使用跨解码器层大部分范围的持久化 kernel，以插件形式暴露，由模型提供 spec，并由 SGLang 侧提供路由/安全 hook。
- **HiCache 主机内存池分配**（[PR #43978](https://github.com/sgl-project/sglang/pull/43978)）：并行、由 THP 支持的主机内存池分配，以消除 `alloc_mmap` 中单线程 `MAP_POPULATE` 预缺页瓶颈。
- **Chunked-prefill mamba 优化**（[PR #30369](https://github.com/sgl-project/sglang/pull/30369)）：对中间 prefill chunk 跳过 mamba/SSM 状态跟踪，因为只有最后一个 chunk 会进入 radix cache。
- **DeepSeek V4.1 优化路线图**（[#42170](https://github.com/sgl-project/sglang/issues/42170)）：mHC 清理已合入，`q_rope_store` 已并入 `fused_q_norm_rope`；mHC SP + engram 融合正在进行（[#43065](https://github.com/sgl-project/sglang/pull/43065)）。
- **Prefill CP BCG 自动选择**（[PR #44022](https://github.com/sgl-project/sglang/pull/44022)）：支持的 `(ContextParallelSize, ...)` 配置会自动选择可断开的 CUDA graph；DSV4 因 token-ID 分片不匹配而暂时排除。
- **AMD DSV4 FP8 验证**（[PR #44003](https://github.com/sgl-project/sglang/pull/44003)）：在 FP8 target-verify 期间将 SWA ring 写入融合的 norm/rope kernel 内。
- **KVPress 集成**（[#10585](https://github.com/sgl-project/sglang/issues/10585)）：开放的 good-first-issue，旨在为长上下文服务启用更多 KV-cache 压缩启发式方法。

## 5. 稳定性与回归

按严重程度排序：

1. **PD prefill + HiCache 中的静默 stale KV** —— [OPEN] [#44070](https://github.com/sgl-project/sglang/issues/44070)。使用默认 `SGLANG_DISAGG_PREFILL_EARLY_SEND_CACHED_PREFIX=1` 时，对于仅驻留在主机内存中的前缀，提前发送 cached-prefix 可能会在 HiCache 回载完成前将 KV 传输到 decode。正确性 bug，未引用修复 PR。
2. **SM100 上使用 `--fp4-gemm-backend marlin` 的 NVFP4 W4A4 崩溃** —— [OPEN] [#43988](https://github.com/sgl-project/sglang/issues/43988)。`mm_fp4` 没有 `'marlin'` 后端，因此该 flag 在 NVFP4 W4A4 checkpoint 上会崩溃。未引用修复 PR。
3. **gRPC 输出被静默限制为 128 个 token** —— [OPEN] [#44121](https://github.com/sgl-project/sglang/issues/44121)。在未设置 `max_new_tokens` 时触发；影响原生 gRPC `Generate`。
4. **GLM-5.3-Flash 在 SM120 上 CUDA graph 捕获时发生 FA4 崩溃** —— [OPEN] [#42012](https://github.com/sgl-project/sglang/issues/42012)。在 `fa4` 中 hybrid-extend reshape 失败；`triton` 目前是 RTX PRO 6000 上唯一可用的 attention 后端。
5. **sgl-deep-gemm 0.2.0 SM90 权重缩放别名** —— [OPEN] [#39684](https://github.com/sgl-project/sglang/issues/39684)。Transform 返回非拥有型别名；是 #32188 中 activation-scale 修复的后续工作。
6. **加载 PyTorch checkpoint 时的不安全 pickle 回退** —— [OPEN PR #42760](https://github.com/sgl-project/sglang/pull/42760)。移除 `_load_pt_file` 和旧式 tar checkpoint 路径中的 `weights_only=False` 回退（安全加固）。
7. **CI flaky / 基础设施故障** —— [OPEN] [#42752](https://github.com/sgl-project/sglang/issues/42752)（53 条评论）以及长期跟踪器 [#17050](https://github.com/sgl-project/sglang/issues/17050)（最近自动更新：1 个 broken、4 个 flaky、1165 个最近已修复）。
8. **自定义 all-reduce 死锁已修复** —— [CLOSED PR #31135](https://github.com/sgl-project/sglang/pull/31135)。两个 CUDA stream 在同一 communicator 上发起 all-reduce 可能导致 GPU-pinned 无限挂起；修复已合入。
9. **今日已解决**：DeepSeekV4TokenToKVPool `get_cpu_copy()` retract 崩溃（[#33385](https://github.com/sgl-project/sglang/issues/33385)，已关闭）、FlashInfer `RadixTopKRenormProbKernel_MultiCTA` coredump（[#32283](https://github.com/sgl-project/sglang/issues/32283)，已关闭，不活跃）、`bench_serving --dataset-name openai` 重复计算输入 token（[#43929](https://github.com/sgl-project/sglang/issues/43929)，已关闭）。

## 6. 对应用开发者的意义

- **gRPC 客户端：始终显式设置 `max_new_tokens`。** 在 [#44121](https://github.com/sgl-project/sglang/issues/44121) 修复前，未设置该值会静默截断到 128 个 token —— 生产中很容易忽略。
- **PD 分离 + HiCache 用户应审查提前前缀发送。** 如果你依赖主机层前缀缓存且使用默认 `SGLANG_DISAGG_PREFILL_EARLY_SEND_CACHED_PREFIX=1`，[#44070](https://github.com/sgl-project/sglang/issues/44070) 可能为共享前缀提供 stale KV —— 在修复前考虑禁用提前发送。
- **在 SM100 NVFP4 W4A4 上避免使用 `--fp4-gemm-backend marlin`**（[#43988](https://github.com/sgl-project/sglang/issues/43988)）；请使用受支持的后端。
- **兼容 Anthropic 的应用获得原生文档摄取能力。** `/v1/messages` 的 `document` 内容块现在可用（[PR #31179](https://github.com/sgl-project/sglang/pull/31179)），对将 PDF 传递到上游的 RAG 流水线很有用。
- **更快的重启即将到来。** Weight Cache Daemon 的演进路线（#33522）将大模型权重加载从约 5 分钟降至亚秒级；如果你部署需要频繁发布的大型 FP8 MoE 模型，请关注该路线图。
- **容量规划应关注的路线图：** Q4 PD Disaggregation（[#43699](https://github.com/sgl-project/sglang/issues/43699)）和 Prefill CP（[#21788](https://github.com/sgl-project/sglang/issues/21788)）—— 两者都扩展模型/后端矩阵，因此今天验证过的配置可能会获得新的更快默认值（例如 [PR #44022](https://github.com/sgl-project/sglang/pull/44022) 中的自动 BCG）。

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 摘要 — 2026-10-14

## 1. 今日亮点

以安全与稳定性为重的一天：`llama-server` 获得两项加固修复——d1-omni/kev 特殊 token 上的 `std::regex` 栈溢出崩溃（b11608，[#30243](https://github.com/ggml-org/llama.cpp/pull/30243)），以及请求解析的 JSON 嵌套深度限制（[#30242](https://github.com/ggml-org/llama.cpp/pull/30242)）。聊天模板化流水线迈出值得注意的 API 一步：格式化输出现在以 *parts* 数组而非单个字符串返回（b11607，[#30389](https://github.com/ggml-org/llama.cpp/pull/30389)）。在硬件方面，Arm KleidiAI 新增 SME2 `Q4_K`/`Q6_K` matmul 支持（b11604，[#29219](https://github.com/ggml-org/llama.cpp/pull/29219)），Hexagon 新增可配置的 HTP DCVS/HMX 功耗模式（b11601，[#30423](https://github.com/ggml-org/llama.cpp/pull/30423)）。

## 2. 发布与破坏性变更

- **聊天输出格式变更** — 格式化后的聊天输出现在是一个带类型 parts 的数组，而不是普通字符串（b11607，[#30389](https://github.com/ggml-org/llama.cpp/pull/30389)）。将格式化后的 prompt 作为字符串来索引的下游消费者（server bindings、grammar 钩子、工具）必须适配。这是 [#28249](https://github.com/ggml-org/llama.cpp/issues/28249) 的第 2 步；预计后续会启用按 part 进行 special-token 解析。
- **特殊 token 转义重写**（b11608，[#30243](https://github.com/ggml-org/llama.cpp/pull/30243)）— 替换 `std::regex_replace`；行为意图保持等价，但任何依赖 `<|name|>` 风格标记精确转义语义的代码都应重新验证。
- **后端搜索路径错误处理简化**（b11606，[#28367](https://github.com/ggml-org/llama.cpp/pull/28367)）— 后端库加载失败时的错误消息已重构；日志抓取工具可能需要更新。
- 本窗口内构建：b11598 → b11609。

## 3. 新模型与硬件支持

- **Arm SME2 量化 matmul** — KleidiAI 后端新增 SME2 `Q4_K` 和 `Q6_K` matmul 路径（b11604，[#29219](https://github.com/ggml-org/llama.cpp/pull/29219)）。
- **Qualcomm Hexagon** — 通过 `GGML_HEXAGON_POWER_MODE` 和 `run.py` 中的 `--hex-power-mode` 标志暴露可配置的 HTP DCVS/HMX 功耗模式（b11601，[#30423](https://github.com/ggml-org/llama.cpp/pull/30423)）。
- **Laguna 模型** — YaRN beta 元数据现在从 GGUF 加载（b11598，[#30331](https://github.com/ggml-org/llama.cpp/pull/30331)）。
- **MiniCPM-V 4.7** — 支持 PR 已打开，复用 4.6 路径并加入 3D RoPE（[#29416](https://github.com/ggml-org/llama.cpp/pull/29416)，已关闭）。
- **Maion-Coder 架构** — 新的原生架构支持正在审查中（[#29778](https://github.com/ggml-org/llama.cpp/pull/29778)）。
- **Qwen4exp / 多模态 MTP** — 专家投影 padding、64 位数组元数据加载，以及多模态 MTP 合并在一个 PR 中（[#30316](https://github.com/ggml-org/llama.cpp/pull/30316)）。
- **Intel Vulkan FA prefill 内核** — 面向 Intel GPU 的 prefill flash-attention shader（[#29357](https://github.com/ggml-org/llama.cpp/pull/29357)）。
- **SYCL 图记录/重放** — 将 CUDA graphs 移植到 SYCL（[#28725](https://github.com/ggml-org/llama.cpp/pull/28725)）。

## 4. 性能与优化

- **Vulkan `pool_2d`** — 当池化窗口超过一个 subgroup 时，现在由整个 subgroup 计算每个输出（b11603，[#30119](https://github.com/ggml-org/llama.cpp/pull/30119)）。这与视觉/嵌入工作负载相关。
- **Meta 后端** — arena 重新分配时重建所有子图，修复陈旧的 graph handle（b11602，[#30453](https://github.com/ggml-org/llama.cpp/pull/30453)）。
- **无缓存 attention KQ bias padding** — T5 风格相对位置 bias 现在零填充到 256 宽的 KV padding（b11600，[#30446](https://github.com/ggml-org/llama.cpp/pull/30446)）。
- **SYCL 内存/性能工作** — 修复 `mul_mat` 中越界 strided 读取、split-buffer 和 host-pool 错误（[#29889](https://github.com/ggml-org/llama.cpp/pull/29889)，可合并）；编译器警告/死代码清理（[#30271](https://github.com/ggml-org/llama.cpp/pull/30271)）。一个 SYCL **PP 性能回归**仍处于打开状态（[#30248](https://github.com/ggml-org/llama.cpp/issues/30248)）。
- **服务器 prompt 缓存** — 严格的 `--cache-ram` 强制执行已落地（[#25070](https://github.com/ggml-org/llama.cpp/pull/25070)），后续修复防止缓存逐出一个即将由传入请求恢复的 prompt（[#30484](https://github.com/ggml-org/llama.cpp/pull/30484)）；进一步工作减少附带的前缀逐出（[#30319](https://github.com/ggml-org/llama.cpp/pull/30319)）。
- **`MUL_MAT_ID` 跳过语义** — 跨后端 PR 允许 `-1` 专家索引完全跳过计算（[#26631](https://github.com/ggml-org/llama.cpp/pull/26631)）。

## 5. 稳定性与回归

按严重程度排序：

1. **崩溃：`llama-server` 在特殊 token 转义时栈溢出** — 使用 d1-omni/kev 模型调用 `/v1/systemone`；libstdc++ 对 `<|name|>` 的递归导致 HTTP 线程栈溢出。已在 b11608 中**修复**（[#30243](https://github.com/ggml-org/llama.cpp/pull/30243)）。
2. **崩溃：深度嵌套 JSON** — 由递归树遍历（jinja `tojson`、json-schema-to-grammar、`/v1/*`）导致 Linux 上 SIGSEGV / macOS 上 SIGBUS。**修复 PR** 将嵌套限制为 128 层（[#30242](https://github.com/ggml-org/llama.cpp/pull/30242)）。
3. **CUDA 崩溃：`fattn.cu:579` 致命错误** — 在 2×RTX3090 上使用 Gemma 4 31B + MTP + tensor split 编辑系统消息后服务器中止。**未解决，未经确认**（[#24440](https://github.com/ggml-org/llama.cpp/issues/24440)）。
4. **SM_60（Tesla P100）质量损失** — FP32 数学运算被静默地以 FP16 执行；下游 fork 已有修复，但上游没有。**未解决**（[#25593](https://github.com/ggml-org/llama.cpp/issues/25593)）。
5. **MCP stdio 服务器死锁** — 由配置启动的 MCP 服务器在工具调用 >~1–5 KB 时永久挂起。**未解决**（[#28723](https://github.com/ggml-org/llama.cpp/issues/28723)）。
6. **Vulkan 设备丢失 / PP 性能下降** — 多个报告：ANV FA 回退到 SCALAR 导致 O(N²) PP 和设备丢失（[#27638](https://github.com/ggml-org/llama.cpp/issues/27638)）；RX 7900 XTX 上 `ErrorDeviceLost`（[#30010](https://github.com/ggml-org/llama.cpp/issues/30010)）；Strix Halo 上 `ubatch > 128` 时 MiniMax-M2 长上下文崩溃（[#30364](https://github.com/ggml-org/llama.cpp/issues/30364)）；自 b10780 起 RDNA3 PP 回归（[#28752](https://github.com/ggml-org/llama.cpp/issues/28752)，已关闭）。
7. **Metal OOM 误报** — 初始化期间发生致命 Metal OOM 后，服务器报告“模型已加载”并绑定端口；随后每个请求都返回 500（[#27309](https://github.com/ggml-org/llama.cpp/issues/27309)）。
8. **`--cache-ram -1` 不是“无限制”** — Token 上限仍为 `n_ctx`；混合模型每个短 prompt 增长约 640 MiB RAM（[#29324](https://github.com/ggml-org/llama.cpp/issues/29324)）。
9. **`--n-cpu-moe` 低于阈值** — 加载 MTP draft 模型时因 `invalid vector subscript` 崩溃（[#27717](https://github.com/ggml-org/llama.cpp/issues/27717)）。
10. **json-schema-to-grammar 字段排序 bug** — 可选属性列在必需属性之前时会被丢弃，阻碍按 schema 排序的输出（[#30321](https://github.com/ggml-org/llama.cpp/issues/30321)）。
11. **编译：Termux 上的 Vulkan** — Shader 优化失败，报 `Invalid capability operand: 5447`（[#28234](https://github.com/ggml-org/llama.cpp/issues/28234)）。

本周期还关闭/过期的有：OpenVINO AVX-512 `STATUS_ILLEGAL_INSTRUCTION`（[#28726](https://github.com/ggml-org/llama.cpp/issues/28726)）、DFlash2 tensor-split 断言（[#27819](https://github.com/ggml-org/llama.cpp/issues/27819)）、DFlash drafter 在数组 `sliding_window_pattern` 上绑定失败（[#26894](https://github.com/ggml-org/llama.cpp/issues/26894)）、MCP 服务器 UI 显示回归（[#26497](https://github.com/ggml-org/llama.cpp/issues/26497)）。

## 6. 这对应用开发者意味着什么

- **审查你的 chat-template 消费者。** 转向基于 parts 的格式化输出（[#30389](https://github.com/ggml-org/llama.cpp/pull/30389)）是迈向选择性 special-token 解析的第一步；如果你通过字符串拼接 `format_chat` 输出来构造 prompt，请在你发布下一个版本前对照新结构验证。
- **如果你运行 d1-omni/kev 或暴露 system-prompt 端点，请尽快升级。** b11608 修复了一个可远程触发的栈溢出崩溃（[#30243](https://github.com/ggml-org/llama.cpp/pull/30243)）；JSON 嵌套修复（[#30242](https://github.com/ggml-org/llama.cpp/pull/30242)）应会在接下来落地。
- **Prompt 缓存计算已改变。** `--cache-ram` 现在是硬限制（[#25070](https://github.com/ggml-org/llama.cpp/pull/25070)），槽位不再逐出一个请求即将复用的 prompt（[#30484](https://github.com/ggml-org/llama.cpp/pull/30484)）。重新调整缓存大小；注意 [#29324](https://github.com/ggml-org/llama.cpp/issues/29324) 意味着 `-1` 不代表无限制。
- **Agent/MCP 部署：** stdio MCP 死锁（[#28723](https://github.com/ggml-org/llama.cpp/issues/28723)）尚未解决——保持工具调用载荷较小，或在修复前优先使用 HTTP MCP 传输。
- **Intel GPU 用户：** SYCL 正在积极加固，但目前存在 PP 回归（[#30248](https://github.com/ggml-org/llama.cpp/issues/30248)）；Intel/AMD 上的 Vulkan 在长上下文时仍存在设备丢失风险（[#30364](https://github.com/ggml-org/llama.cpp/issues/30364)，[#30010](https://github.com/ggml-org/llama.cpp/issues/30010)）。升级前固定构建版本并测试长上下文批次。
- **Arm 边缘/嵌入式：** SME2 `Q4_K`/`Q6_K`（[#29219](https://github.com/ggml-org/llama.cpp/pull/29219)）和 Hexagon 功耗模式控制（[#30423](https://github.com/ggml-org/llama.cpp/pull/30423)）值得在新硬件上针对每瓦吞吐量进行基准测试。

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 摘要 — 2026-10-14

## 1. 今日要点
过去 24 小时内没有新发布。最紧迫的事项是有报告称相对 v0.40.1 出现 25–30% 吞吐量回退，以及 `glm-5.3-flash:cloud` 上持续的云端生成失败。后端围绕 MLX/CUDA 内核、旧版 GGUF 迁移，以及恢复启用结构化 JSON 输出时的工具调用行为展开工作。

## 2. 发布与破坏性变更
- 过去 24 小时内无新发布。
- PR #18882（open）：移除 llama.cpp 兼容性补丁，并在加载时迁移旧版 GGUF；将原子替换 manifest，并在转换/启动 GC 期间回收未被引用的旧版 blob。可能影响迁移/磁盘。https://github.com/ollama/ollama/pull/18882
- PR #18990（open）：Modelfile 中的 `SYSTEM ""` 将清空系统提示词，而不是继承基础模型的系统提示词。https://github.com/ollama/ollama/pull/18990
- PR #18991（open）：允许模型元数据设置默认思考行为；较小的 Qwen 3.5 模型可能会在禁用思考的情况下重新发布。https://github.com/ollama/ollama/pull/18991
- PR #18993（open）：MLX 版本升级；注明应在 #18991 之后合并，并且此升级会加剧较小 Qwen 3.5 模型中的思考循环。https://github.com/ollama/ollama/pull/18993

## 3. 新模型与硬件支持
- Snapdragon X Elite NPU/Adreno GPU 支持请求仍处于开放状态，已有 68 👍：#5360。https://github.com/ollama/ollama/issues/5360
- K2 Horizon（`k2-horizon`）模型系列支持请求：0.9B–36B MoE，官方 GGUF：#18698。https://github.com/ollama/ollama/issues/18698
- 结构化输出下的工具调用：PR #18975 提议允许模型原生的工具调用前缀，同时在未选择工具时保持格式化响应受约束；通过 llama-server/MLX grammar 携带 Qwen 3.5 和通用工具调用标签。https://github.com/ollama/ollama/pull/18975
- CUDA 后端：已关闭的 PR #18657 将仅限 Metal 的自定义内核（`mamba2_scan`、`depthwise_conv_silu`、`gated_delta`）移植到 CUDA，减少计算图回退。https://github.com/ollama/ollama/pull/18657
- Apple Metal：已关闭的 PR #14604 修复 Apple M5 `MTLGPUFamilyApple10` 设备上的 `static_assert` 崩溃。https://github.com/ollama/ollama/pull/14604
- MLX 元数据/UI：#18971 报告 ollama.com 上 `embeddinggemma-2` 标签缺少 MLX 徽章。https://github.com/ollama/ollama/issues/18971
- CUDA 运行时打包：PR #17956 去重 Windows/Linux MLX CUDA 构建中的 CUDA 运行时载荷。https://github.com/ollama/ollama/pull/17956
- 集成：PR #18989 向 `ollama launch` 添加仅限 Runner 的 Wirken agent gateway 集成。https://github.com/ollama/ollama/pull/18989

## 4. 性能与优化
- #18968：有报告称所有模型相对 v0.40.1 的 tokens/s 降低 25–30%。尚无修复 PR。https://github.com/ollama/ollama/issues/18968
- #17916：默认 `n_threads` 忽略 cgroup v2 CPU 配额和 cpuset；受 CPU 限制的容器出现约 45 倍吞吐量骤降。规避方法：显式设置线程/CPU 预算。https://github.com/ollama/ollama/issues/17916
- #18232：Blackwell RTX 5060 Ti 上可复现的 CUDA 崩溃；根因已定位为 `num_ctx` 影响 Flash Attention MMA 内核的共享内存分配。https://github.com/ollama/ollama/issues/18232
- #17380：Windows/RTX 5070 Ti 上预热时偶发 `CUDA error: shared object initialization failed`，并静默回退到 `CUDA_Host` → CPU pinned-buffer。https://github.com/ollama/ollama/issues/17380
- PR #18657：Metal 内核的 CUDA 移植减少了 Mamba-2/conv/gated-delta 路径的图算子回退。https://github.com/ollama/ollama/pull/18657
- PR #18882：刷新 llama.cpp 并移除私有兼容性补丁，加载时进行旧版 GGUF 转换。https://github.com/ollama/ollama/pull/18882

## 5. 稳定性与回归
按严重程度排序：
1. #18980 — 自 10-10 起 `glm-5.3-flash:cloud` 出现大量生成问题；报告称 5–8% 失败/退化，宿主机侧数据被置零。对云用户而言为严重。https://github.com/ollama/ollama/issues/18980
2. #18968 — 所有模型相对 v0.40.1 出现 25–30% 性能回退。高。https://github.com/ollama/ollama/issues/18968
3. #18796 — 在 Raspberry Pi 5/Debian 13 上，第二次 `ollama run` 挂起且无输出。高。https://github.com/ollama/ollama/issues/18796
4. #18983 — `openbmb/minicpm-v4.7:35b-a3b` 在 Windows 0.40.3 上因 `unknown projector type: minicpmv4_7` / CLIP 加载失败而无法加载。对视觉用户为高。https://github.com/ollama/ollama/issues/18983
5. #18944 — 设置 `format: json` 时工具调用失败；PR #18975 是提议的修复。对 agent 开发者而言为高。https://github.com/ollama/ollama/issues/18944
6. #18232 / #17380 — Windows/Blackwell 上的 CUDA 崩溃；#18232 已有根因分析，尚无修复 PR。高。https://github.com/ollama/ollama/issues/18232 · https://github.com/ollama/ollama/issues/17380
7. #17916 — 因线程默认值不感知 cgroup 导致 CPU 容器吞吐量骤降。中高。https://github.com/ollama/ollama/issues/17916
8. #18716 — `ollama pull qwen3.6` 失败并报 `redirect target not allowed`。中。https://github.com/ollama/ollama/issues/18716
9. #18971 — 某些仅 MLX 的 `embeddinggemma-2` 标签缺少 MLX 徽章。低。https://github.com/ollama/ollama/issues/18971
10. #2519 — Windows Defender 误报警告已关闭。低/信息性。https://github.com/ollama/ollama/issues/2519

## 6. 对应用开发者的意义
- 在 #18975 落地前，避免将 `format: json` 与工具调用一起使用；否则使用普通工具调用模式，或单独校验工具调用输出。https://github.com/ollama/ollama/issues/18944
- 如果吞吐量重要，将 v0.40.1 视为基线，并固定/测试近期版本；#18968 报告了显著回退。https://github.com/ollama/ollama/issues/18968
- 在受 CPU 限制的容器中，显式设置线程数/CPU 预算，而不要依赖默认值；#17916 显示了与 cgroup 相关的严重骤降。https://github.com/ollama/ollama/issues/17916
- 云用户应添加重试/回退，并审查数据保留条款；`glm-5.3-flash:cloud` 不稳定（#18980）以及缓存输入/ZDR 定价问题（#17489）仍未解决。https://github.com/ollama/ollama/issues/18980 · https://github.com/ollama/ollama/issues/17489
- Windows CUDA/Blackwell 部署应跟踪 #17380 和 #18232；在修复落地前，更改 `num_ctx` 可能避免 Flash Attention MMA 共享内存崩溃。https://github.com/ollama/ollama/issues/18232
- 如果 #18882 合并，请为首次加载时的旧版 GGUF 转换以及 manifest/blob 生命周期变更做好规划；如果需要可复现性，请固定/备份模型。https://github.com/ollama/ollama/pull/18882
- 对于 Snapdragon X Elite（#5360）和 K2 Horizon（#18698），硬件/模型覆盖仍仅处于请求阶段；不要假定已具备生产支持。https://github.com/ollama/ollama/issues/5360 · https://github.com/ollama/ollama/issues/18698

---

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 摘要 — 2026-10-14

来源: [github.com/BerriAI/litellm](https://github.com/BerriAI/litellm)

## 1. 今日亮点

- LiteLLM 发布了 [v1.106.0-rc.2](https://github.com/BerriAI/litellm/releases/tag/v1.106.0-rc.2)；提供的发布说明重点在 cosign Docker 镜像签名验证，未见迁移或 API 破坏性变更说明。
- 活跃度依然非常高：过去 24 小时内更新了 83 个 issue 和 500 个 PR。MCP/工具处理、代理管理 UX、token 计数和 provider 转换 bug 主导了 issue 变动。
- 一个规模可观的遥测/可观测性 PR 堆栈正在推进，同时新增了 OpenInfer、Laya/Bespoke、MiniMax video、Gemini long-decode continuation 等 provider。

## 2. 发布与破坏性变更

- [v1.106.0-rc.2](https://github.com/BerriAI/litellm/releases/tag/v1.106.0-rc.2) — 候选版本。所有 Docker 镜像均使用 cosign 签名；发布使用 commit `0112e53` 中引入的同一密钥。提供的数据中没有 API/配置迁移说明。
- [PR #45497](https://github.com/BerriAI/litellm/pull/45497) — `fix(rag)!`：使用团队 provider 凭据，允许 viewer 和 JWT 团队访问，并更改 `/rag/ingest` 和 `/rag/query` 的失败行为。对于 RAG 部署应视为可能具有破坏性。
- 今天没有其他确认的破坏性变更出现。

## 3. 新模型与硬件支持

- [PR #41034](https://github.com/BerriAI/litellm/pull/41034) — 新增 `openinfer` 作为 OpenAI 兼容 provider，路由 `openinfer/<model>`，并包含支出跟踪目录费率。
- [PR #45260](https://github.com/BerriAI/litellm/pull/45260) — 将 Laya 和 Bespoke Nimble 添加到 Add Model 表单，并注册 `laya/*` / `bespoke/*` Decisions 路由。
- [PR #43705](https://github.com/BerriAI/litellm/pull/43705) — 通过 `/v1/videos` 新增 MiniMax-H3 和 H3-Max 视频生成，包含 create/status/content/list/delete 路由和按秒计价。
- [PR #46240](https://github.com/BerriAI/litellm/pull/46240) — Gemini 4 Argon 长解码使用 `continuationToken` 自动继续；解决每次调用 32K / 每流 64K 输出上限。
- [PR #45686](https://github.com/BerriAI/litellm/pull/45686) — Databricks 非 Claude 模型现在接收单个纯文本块字符串，避免 `json_object` 400 错误。
- 提供的数据中没有 CUDA/ROCm/Metal/CPU 或量化格式变更。

## 4. 性能与优化

- [PR #45487](https://github.com/BerriAI/litellm/pull/45487) — 新增 `AggregatingSink`、固定桶直方图、`Report` 和 `HttpExporter`，以减少每请求遥测量和流量形态暴露。
- [PR #45489](https://github.com/BerriAI/litellm/pull/45489) — `TelemetryMiddleware` 按客户端视角对 LLM、MCP 和 A2A 请求计时，并记录 provider 尝试。
- [PR #45490](https://github.com/BerriAI/litellm/pull/45490) — 针对隔离网络代理的本地报告表、持久实例 ID 和管理员导出路由。
- [PR #45494](https://github.com/BerriAI/litellm/pull/45494) — Admin UI 遥测设置页面和受控的页面导航事件。
- [PR #46267](https://github.com/BerriAI/litellm/pull/46267) — 通过跳过指标已携带的标签，修复 Prometheus 自定义元数据标签；此前会破坏项目 key 的延迟、请求计数和部署指标。
- [PR #43043](https://github.com/BerriAI/litellm/pull/43043) — 按团队拆分 Azure PTU 部署，支持每团队 PTU 上限和 PTU 小时报告。
- 成本/计费准确性：[PR #45733](https://github.com/BerriAI/litellm/pull/45733) 为 `/vllm` 透传调用写入支出日志；[PR #43667](https://github.com/BerriAI/litellm/pull/43667) 对每个非流式 MCP 自动执行轮次计费；[PR #46288](https://github.com/BerriAI/litellm/pull/46288) 按部署费率为 Bedrock 跨区域 profile 定价。
- [PR #46332](https://github.com/BerriAI/litellm/pull/46332) — 从成本映射中移除 `azure_ai/claude-fable-5-5`，因为 Azure 未列出它。

## 5. 稳定性与回归

大致按严重程度排序：

- [Issue #46045](https://github.com/BerriAI/litellm/issues/46045) — 设置了 tools 且未配置 `_skip_mcp_handler` 时出现 `ModuleNotFoundError: No module named 'mcp'`。未显示修复 PR。
- [Issue #46052](https://github.com/BerriAI/litellm/issues/46052) — 对于没有 `items` 的数组工具参数，`token_counter` 抛出 `KeyError: 'items'`，导致 `/v1/messages/count_tokens` 返回 HTTP 500。影响许多 MCP 工具 schema。未显示修复 PR。
- [Issue #46209](https://github.com/BerriAI/litellm/issues/46209) — 在工具结果之后，Azure 托管的 `mistral-medium-3-5` 出现 `TypeError: can only concatenate str (not "list") to str`，尤其是较长结果；影响流式和非流式。未显示修复 PR。
- [Issue #46247](https://github.com/BerriAI/litellm/issues/46247) — `bedrock_mantle` 地理推理 profile 返回空的 `completed` 响应，使用量为零。未显示修复 PR。
- [Issue #17475](https://github.com/BerriAI/litellm/issues/17475) — 代理不再向非管理员用户显示模型列表；仅管理员视图正常。14 条评论，6 个赞。未显示修复 PR。
- [Issue #44093](https://github.com/BerriAI/litellm/issues/44093) — MCP 工具 pin 忽略 annotations 和 `outputSchema`，因此 `readOnlyHint` 可能变为破坏性却不会触发漂移告警。未显示修复 PR。
- [Issue #45645](https://github.com/BerriAI/litellm/issues/45645) — Responses API 流中 429 可能被记录到 fallback 部署上，并错误地将其冷却。未显示修复 PR。
- [Issue #40780](https://github.com/BerriAI/litellm/issues/40780) — 带 `openai/` 前缀的自托管模型将 `/v1/messages` 路由到 Responses API，静默破坏 vLLM、llama.cpp、SGLang、TGI 和 LM Studio 的多模态。未显示修复 PR。
- [Issue #41361](https://github.com/BerriAI/litellm/issues/41361) — `/v1/messages` 会丢弃 Claude Opus 4.8 / Fable 5 / Fable 5.1 的仅签名 thinking 块，丢失推理内容。未显示修复 PR。
- [Issue #46076](https://github.com/BerriAI/litellm/issues/46076) — 管理端点会在 500 响应中返回原始 Postgres 异常文本。部分修复：[PR #46330](https://github.com/BerriAI/litellm/pull/46330) 仅隐藏项目创建时的 DB 错误。
- [Issue #45988](https://github.com/BerriAI/litellm/issues/45988) — 启动 schema 检查建议对分区表 `LiteLLM_SpendLogs` 进行无效的主键重写。未显示修复 PR。
- [Issue #46107](https://github.com/BerriAI/litellm/issues/46107) — 逐页 PDF token 计数对所有 provider 都使用 Anthropic 定价，因此 Router 调用前检查会拒绝 Gemini/OpenAI PDF 请求。未显示修复 PR。
- [Issue #46198](https://github.com/BerriAI/litellm/issues/46198) — dashboard 会为非管理员用户调用仅限管理员的 `/health/license`，导致日志中充斥 ERROR 堆栈跟踪。未显示修复 PR。
- [Issue #46095](https://github.com/BerriAI/litellm/issues/46095) — OpenRouter 流式 chunk 报告的是请求的 router 名称，而不是 OpenRouter 实际服务的模型。未显示修复 PR。
- [Issue #41544](https://github.com/BerriAI/litellm/issues/41544) — Bedrock 音频转录因 system 消息与 audio 块不兼容而失败，返回 HTTP 400。未显示修复 PR。
- [Issue #28539](https://github.com/BerriAI/litellm/issues/28539) — `Custom_openaiException`：`AsyncCompletions.create()` 收到意外的 `client_metadata`。未显示修复 PR。
- [Issue #33365](https://github.com/BerriAI/litellm/issues/33365) — LiteLLM 无法在隔离网络环境中安装。未显示修复 PR。
- [Issue #33405](https://github.com/BerriAI/litellm/issues/33405) — 代理固定 `gunicorn>=23,<24`；公告 AIKIDO-2026-10742 标记请求走私，并建议使用 `gunicorn` 26。未显示修复 PR。
- 低严重程度：[Issue #46212](https://github.com/BerriAI/litellm/issues/46212) — mcp-server CI 分片中的 MCP 并发 schema 验证测试不稳定。

过去一天关闭/解决：

- [Issue #32281](https://github.com/BerriAI/litellm/issues/32281) — MCP 工具转换在 `/v1/chat/completions` 配合 `hosted_vllm` 时丢弃了 `function` 包装键。
- [Issue #16060](https://github.com/BerriAI/litellm/issues/16060) — `usage-based-routing-v2` 错误地未找到部署。
- [Issue #29826](https://github.com/BerriAI/litellm/issues/29826) — MCP `ListResourcesResult.resources` 对缺失字段的容错。
- [Issue #32280](https://github.com/BerriAI/litellm/issues/32280) — `/anthropic` 透传日志在 `count_tokens` 上因 `KeyError: 'content'` 崩溃。
- [Issue #35790](https://github.com/BerriAI/litellm/issues/35790) — Vertex AI Claude `json_tool_call` 响应未解包。
- [Issue #32118](https://github.com/BerriAI/litellm/issues/32118) — 搜索工具支持多个 API key 并轮换。

其他值得注意的修复 PR：

- [PR #46187](https://github.com/BerriAI/litellm/pull/46187) — 当通用 userinfo 响应不是 JSON 时，SSO 回退到 `id_token` claims。
- [PR #45686](https://github.com/BerriAI/litellm/pull/45686) — Databricks 非 Claude 消息塑形修复。
- [PR #46267](https://github.com/BerriAI/litellm/pull/46267) — Prometheus 标签重复修复。
- [PR #46288](https://github.com/BerriAI/litellm/pull/46288) — Bedrock 跨区域成本选择器修复。

## 6. 这对应用开发者意味着什么

- 本周 MCP/工具使用是风险最高的领域。请验证 `mcp` 安装，测试带数组参数的工具 schema，并且如果依赖 `/v1/messages/count_tokens` 或 MCP 自动执行，请固定 LiteLLM 版本。
- 最近升级后，非管理员代理用户可能会看到空的模型列表。管理员在大范围推出前应验证 viewer/internal-user 角色的 dashboard 行为。
- 特定 provider 的回归正在多个路径上活跃：Mistral 工具续写、Bedrock 地理推理 profile、`openai/` 自托管多模态路由、OpenRouter 流式模型归属以及 PDF token 计数。如果使用这些路径，请添加针对性集成测试。
- 需要监控流式 fallback 健康状态：流中 429 可能被归因到 fallback 部署，并错误地将其冷却。
- 对于企业/隔离网络部署，遥测和成本核算 PR 值得跟踪。vLLM 透传、MCP 自动执行和 Bedrock 跨区域定价的支出日志正在被修正，因此升级后请重新验证成本 dashboard。
- 安全卫生：管理端点中的原始 DB 错误和 `gunicorn <24` 公告是仍未解决的事项。自托管运营者应考虑立即缓解或规划升级。

---

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 摘要 — 2026-10-14

**仓库：** [unslothai/unsloth](https://github.com/unslothai/unsloth) · 活动窗口：过去 24 小时 · 147 个 issue / 252 个 PR 被触及

---

## 1. 今日亮点

Unsloth 发布了 **v0.1.907-beta**，这是一个以性能为重点的版本，宣称多聊天 UI/UX 最高可**快 10 倍**，推理最高可**快 5 倍**，并据称有 **95% 缓存命中率**，同时包含 MoE "Strata" 风格修复和通用减速修复。与此同时，Studio 方向正围绕 **DGX Spark 双节点集群、视频/DiT LoRA 训练（Wan2.2、LTX-2.5）以及 MoE 缓存放置正确性**收敛（[PR #13674](https://github.com/unslothai/unsloth/pull/13674)、[PR #10323](https://github.com/unslothai/unsloth/pull/10323)、[PR #10280](https://github.com/unslothai/unsloth/pull/10280)）。2026-10-13 关闭了一大批较旧的 bug 报告（多 GPU OOM、批量 TTS、Windows Gemma4e2b、多 LAN 用户模型同步），这可能反映的是分诊（triage）而非已验证修复——在依赖它们之前，请对照你的版本进行确认。

---

## 2. 发布与破坏性变更

- **v0.1.907-beta — “大幅性能提升 + 更多 Browser Use”** —— 对 UI/UX 和推理进行了大规模性能优化；更好的压缩（compaction）语义现在适用于*所有* API，并改进了 UI 处理。该版本未随附明确的 API/配置迁移说明。说明：[releases](https://github.com/unslothai/unsloth/releases)
- **Studio 默认值的行为变更（评审中）：** LTX-2.5 现在默认使用托管的 **INT8-ConvRot DiT**，即使 bf16 能装入 VRAM，也与 ComfyUI 模板保持一致；int8 文本编码器为可选启用（[PR #13618](https://github.com/unslothai/unsloth/pull/13618)）。如果你固定了 Studio 视频默认值，请在升级前审查。
- **线程变更（评审中）：** macOS 上的 sd.cpp 将不再假设 SMT，而是使用所有性能核心，而不是 `cpu_count() // 2`（[PR #13349](https://github.com/unslothai/unsloth/pull/13349)）——这会影响 Apple Silicon 和 CI runner 上的 CPU 侧图像生成吞吐量。

---

## 3. 新模型与硬件支持

| 项目 | 状态 | 链接 |
|---|---|---|
| Studio DiT 训练器中的 **Wan2.2-TI2V-5B LoRA 训练**（首个被标记为可训练的视频系列；计划后续支持 A14B 双专家） | PR 开放中 | [#13654](https://github.com/unslothai/unsloth/pull/13654) |
| **LTX-2.5** Studio 默认值与 ComfyUI 对齐（托管 INT8-ConvRot DiT，可选启用 int8 文本编码器） | PR 开放中 | [#13618](https://github.com/unslothai/unsloth/pull/13618) |
| **DGX Spark / GB10（aarch64、sm_121a、CUDA 13）** —— 单节点支持，以及基于 ConnectX-7 200GbE 的双节点集群、流水线训练、链路诊断 | PR 开放中 | [#10280](https://github.com/unslothai/unsloth/pull/10280) |
| **Windows NVIDIA 驱动早于 CUDA 12** —— 回退到 Vulkan 或 CPU llama.cpp 捆绑包，而不是注定失败的源码构建/回滚 | PR 开放中 | [#13671](https://github.com/unslothai/unsloth/pull/13671) |
| **H100（sm_90）+ xformers** —— 当构建缺少 sm_90a kernel 时跳过 FA3 | PR 开放中 | [#13619](https://github.com/unslothai/unsloth/pull/13619) |
| 已请求，尚未落地：Voxtral（👍14） | Issue 开放中 | [#3013](https://github.com/unslothai/unsloth/issues/3013) |
| 已请求：Studio 中的 Ling 3.0（👍5） | Issue 已关闭 | [#8532](https://github.com/unslothai/unsloth/issues/8532) |
| 已请求：NPU 后端（👍8） | Issue 已关闭 | [#1794](https://github.com/unslothai/unsloth/issues/1794) |
| 已请求：Studio 的 Flatpak 打包（👍30 —— 信号最强的平台诉求） | Issue 开放中 | [#4380](https://github.com/unslothai/unsloth/issues/4380) |
| 已请求：面向基于 processor 的（多模态）模型的 sample packing —— 目前仅支持纯文本 LLM | Issue 开放中 | [#4120](https://github.com/unslothai/unsloth/issues/4120) |
| 已请求：在 PyTorch/CUDA 上进行多图像 Clef/Jev 决策模型训练+预测 | Issue 已关闭 | [#13461](https://github.com/unslothai/unsloth/issues/13461) |
| AMD RX 5700XT 在 Unsloth Desktop 中未被检测到 | Issue 已关闭 | [#8529](https://github.com/unslothai/unsloth/issues/8529) |

---

## 4. 性能与优化

- **版本层面：** 多个并发聊天下 UI/UX 最高**快 10 倍**；推理最高**快 5 倍**，并达到 **95% 缓存命中率**；MoE "Strata" 风格修复；压缩（compaction）已在所有 API 中可用。[releases](https://github.com/unslothai/unsloth/releases)
- **MoE 缓存放置防护已完成 + spill 微批提升已恢复** —— 来自 #13600 的五个 Codex 发现项曾因 squash-merge 被丢弃，现已 cherry-pick 回来；与 MoE serving 内存行为和 spill 处理直接相关。[PR #13674](https://github.com/unslothai/unsloth/pull/13674)
- **带异步副本路由器的双 Spark serving 编排器** —— 在成对 DGX Spark 上加载 GGUF 时，从 `spark_cluster.recommend_topology` 中选择三种拓扑之一，在工作负载允许时让两个节点都保持忙碌，并且绝不阻塞事件循环。[PR #10323](https://github.com/unslothai/unsloth/pull/10323)
- **H100 FA3 跳过**避免了 `wgmma.fence without CUTE_ARCH_MMA_SM90A_ENABLED` 路径，该路径目前会间接在 cuBLAS 调用上崩溃。[PR #13619](https://github.com/unslothai/unsloth/pull/13619)
- **Apple Silicon sd.cpp 线程** —— 目前使用 `os.cpu_count() // 2`；在 3 核 macOS-15 runner 上，这只有 1 个线程。修复后恢复完整的性能核心使用。[PR #13349](https://github.com/unslothai/unsloth/pull/13349)
- **长上下文聊天延迟**（`#12552`）和**多聊天变慢**是本次发布背后用户可见的驱动因素；该 issue 已关闭。[Issue #12552](https://github.com/unslothai/unsloth/issues/12552)

---

## 5. 稳定性与回归

大致按严重程度/影响范围排序：

**高**
- **使用 conda-forge xformers 的 H100 训练崩溃** —— 在第一步崩溃，日志刷满 `Attempting to use wgmma.fence without CUTE_ARCH_MMA_SM90A_ENABLED`，随后在一个无关的 cuBLAS 调用上挂掉。**修复 PR 已存在，但处于开放/未合并状态** → [#13619](https://github.com/unslothai/unsloth/pull/13619)，上游 [#3963](https://github.com/unslothai/unsloth/issues/3963)。
- **在 Qwen3-MoE 上使用 `fast_inference=True` 的 GRPO + LoRA** —— `AttributeError: 'Qwen3MoeForCausalLM' object has no attribute 'vllm_engine'`；回退到非常慢的非 vLLM 路径。本窗口内关闭时没有关联修复 → [#2741](https://github.com/unslothai/unsloth/issues/2741)。
- **多 GPU 训练 OOM**，发生在一次 6-GPU 云端运行中（以 inactive 关闭——应视为未解决）→ [#4685](https://github.com/unslothai/unsloth/issues/4685)。
- **Mistral Small 3.1/3.2 SFT 产生极高 loss**，使用 SFTTrainer + PEFT 时 → [#2786](https://github.com/unslothai/unsloth/issues/2786)。

**中**
- **多用户 LAN/本地 serving 不同步** —— 模型无法在多个账户之间一致地保持加载（报告配置为 2x RTX PRO 6000 Blackwell 96GB）。已关闭 → [#12365](https://github.com/unslothai/unsloth/issues/12365)。
- **Gemma4e2b 在 Windows 上不可用** → [#13388](https://github.com/unslothai/unsloth/issues/13388)。
- **混合文本+图像的批量推理**（`Received inconsistently sized batches of images (1) and text (4)`，Gemma 3 4B）→ [#3094](https://github.com/unslothai/unsloth/issues/3094)。
- **Windows Desktop 安装回滚**，发生在 GT 650M / 驱动报告 CUDA 10.1 时 —— 在预构建回退之后仍走到了 `winget` OpenSSL 开发步骤。**修复 PR 开放中** → [#13671](https://github.com/unslothai/unsloth/pull/13671)。
- **Claude CLI 会话在向纯文本 GGUF 粘贴图像后被污染** —— 被拒绝的图像会留在重新发送的历史中，因此后续每个 prompt 都会因相同的视觉错误而失败。**修复 PR 开放中** → [#13652](https://github.com/unslothai/unsloth/pull/13652)。
- **NVIDIA/AMD/Intel 上 LoRA 初始化忽略 Random Seed** —— 只有数据顺序发生变化；使用 seed 42 和 7 的两次运行从相同的 adapter 权重（seed 3407）开始。Apple Silicon 已经正确。**修复 PR 开放中** → [#13637](https://github.com/unslothai/unsloth/pull/13637)。

**低 / 外观与 CI**
- Orpheus TTS 在 batch size > 1 时无效 dim —— [已修复，待确认] → [#3123](https://github.com/unslothai/unsloth/issues/3123)。
- RTX 4080 上模型加载失败 → [#4626](https://github.com/unslothai/unsloth/issues/4626)。
- 写成 `$3$` 的行内数学渲染为损坏的价格文本 → **修复 PR 开放中** [#13649](https://github.com/unslothai/unsloth/pull/13649)。
- 思考轨迹 chevron 方向与整条轨迹样式调整 → [#13669](https://github.com/unslothai/unsloth/pull/13669)、[#13665](https://github.com/unslothai/unsloth/pull/13665)。
- 打开 Recipe 对话框时触发提示 tooltip → [#13668](https://github.com/unslothai/unsloth/pull/13668)。
- `studio-windows-ui-smoke.yml` 上的 `Chat UI Tests` 不稳定（仅等待 composer）→ [#13672](https://github.com/unslothai/unsloth/pull/13672)；main 在 `probe_dismiss_guard.py` 处变红，二分定位到 inline-message-actions 变更，已修复并关闭 → [#13670](https://github.com/unslothai/unsloth/pull/13670)。
- **安全：** 当存在 `Authorization` 头时，Deep Research 查询清洗器泄露了凭据 —— 标签规则匹配到了 scheme（`Bearer`/`Basic`/`token`），并遮蔽了错误的 token。**修复 PR 开放中** → [#13499](https://github.com/unslothai/unsloth/pull/13499)。

---

## 6. 对应用开发者的意义

- **长上下文 agent 循环变得更便宜。** 95% 缓存命中 / 最高 5 倍推理提升的说法，加上“面向所有 API 的更好 compaction”，正好针对多轮、工具调用模式中 KV 复用主导成本的问题。采用前，请在你自己的 prompt 组合上，用 v0.1.907-beta 与你固定版本做基准对比。
- **MoE serving 仍是最锋利的边缘。** 缓存放置、spill 微批处理，以及 MoE 的 vLLM 集成（`#2741`、`#13674`）是活跃的断层线。如果你在网关后面提供 Qwen3-MoE 或类似模型，请固定版本并验证 `fast_inference` 路径。
- **在非 Apple 硬件上，LoRA seed 的可复现性目前已被破坏**，直到 [#13637](https://github.com/unslothai/unsloth/pull/13637) 落地。今天不要在 NVIDIA/AMD/Intel 上按 seed 比较微调运行结果。
- **Studio 正在变成一个 serving 平面，而不仅仅是一个 UI。** Scheduled Routines 工作流（[#13539](https://github.com/unslothai/unsloth/pull/13539)）、双 Spark 异步副本路由（[#10323](https://github.com/unslothai/unsloth/pull/10323)）、一键 MCP 安装（[#10822](https://github.com/unslothai/unsloth/issues/10822)），以及 Ollama 兼容的 chat 配置导出（[#4660](https://github.com/unslothai/unsloth/issues/4660)）都指向在 OpenAI/vLLM 兼容接口之后的自托管推理——如果你运行边缘推理，值得评估。
- **Browser 面板 + 远程访问变得可用了，而不只是外观。** 页面现在可以调用其自己站点的 API（[#13664](https://github.com/unslothai/unsloth/pull/13664)），这才让 "Browser Use" agent 真正可用——在启用远程访问前，审查 origin/permission 模型。
- **通过 vLLM / AWQ 提供微调后的 LoRA 产物仍是集成痛点**（[#464](https://github.com/unslothai/unsloth/issues/464)，22 条评论）。应规划显式的导出/量化步骤，而不是假设可直接替换兼容。

> **注意事项：** issue 状态反映的是 2026-10-13/14 的 GitHub 状态。若干 "CLOSED" 项目在本窗口内没有关联修复提交（有些标记为 `inactive`），且除非另有说明，PR 摘要均由作者撰写且尚未合并。在做出升级决策前，请对照 `main` 验证。

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*