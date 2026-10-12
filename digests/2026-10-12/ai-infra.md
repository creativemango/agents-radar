# AI 基础设施日报 2026-10-12

> 生成时间: 2026-10-12 08:27 UTC | 覆盖项目: 6 个

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## 横向对比

# 跨项目 AI 基础设施报告 — 2026-10-12

## 1. 生态系统概览

AI 基础设施生态系统正处于**加固阶段**，而非以发布为主的阶段：六个项目中有四个没有发布新版本，关注点主要集中在正确性回归、硬件适配以及面向智能体的协议缺口上。数据中心服务引擎正在 **MoE、MLA/注意力 kernel、量化 KV cache 和非 NVIDIA 后端**上展开竞争，同时还要应对前缀缓存和投机解码中的静默损坏问题。本地运行时正在扩展后端覆盖和工具调用，但多租户缓存隔离以及 Apple MLX/CUDA/Vulkan 稳定性仍是薄弱环节。网关和训练栈正在向更高层演进：LiteLLM 聚焦于 MCP 鉴权、协议转换和费用准确性；Unsloth 则关注静默训练失败、KV 抢占以及让 Studio 可由智能体驱动。在各层中，主导型工作负载形态是 **MoE、量化、长上下文/前缀复用以及使用工具的智能体**。

---

## 2. 活动对比

摘要报告的 24 小时活动。只有 vLLM 和 LiteLLM 提供了汇总计数；其他摘要列出了单个 issue/PR，但没有总数。

| 项目 | 24 小时内更新的 Issue | 24 小时内更新的 PR | 发布状态 |
|---|---:|---:|---|
| **vLLM** | 86 | 464 | 无新版本发布。0.30.0 在 DFlash2/DSpark 上存在前缀缓存回归；0.29.x 是该配置下最后一个已知良好版本。 |
| **SGLang** | 未报告 | 未报告 | 无新版本发布。Intel XPU 向 PyTorch 2.14 的迁移正在进行中。 |
| **llama.cpp** | 未报告 | 未报告 | 滚动构建 b11554–b11564。**b11563 将 GGML 符号可见性改为 hidden** — 会破坏某些嵌入方。 |
| **Ollama** | 未报告 | 未报告 | 无版本发布。PR #18917 已关闭，将 llama.cpp 升级到 b11521，并添加决策模型 + EmbeddingGemma 2。 |
| **LiteLLM** | 74 | 226 | **v1.106.0-rc.1、v1.105.0、v1.104.3** 已发布。Docker 镜像经 cosign 签名。 |
| **Unsloth** | 未报告 | 未报告 | 无新版本发布。新的 Linux aarch64 cu130/Python 3.13 Studio 安装迁移到 torch 2.13。 |

**活动要点：** vLLM 的原始 PR 吞吐量遥遥领先；LiteLLM 位居第二，并且是今天唯一有标签发布的项目。SGLang、llama.cpp、Ollama 和 Unsloth 的 issue/PR 活动密集，但没有公布总数。

---

## 3. 模型支持竞赛

| 项目 | 本窗口内新增模型/架构 | 硬件/后端 | 定位 |
|---|---|---|---|
| **vLLM** | Kimi-K3 运行于 gfx942；Intel XPU 和 ROCm FP8 prefill 上的 MLA；FlashInfer 0.7.1；面向 BERT/RoBERTa/XLMRoberta 的 embedding LoRA | Intel XPU、ROCm MI355X/gfx950/gfx942、CUDA/Rubin | 在加速器覆盖面和 MLA/MoE 适配方面为**数据中心服务领导者** |
| **SGLang** | Qwen-Image 2.1、MiniMax H3/FastH3/VDN、LTX-2.5/2.3；DeepSeek-V4.1-Flash SM120 现场报告；Qwen3.8 512 专家 MoE ROCm；DSV4 NPU | Blackwell/SM120、NPU、ROCm MI355X、Intel XPU | **数据中心 + 多模态/扩散模型实力强**；覆盖面广，但 SM120/AMD 崩溃问题仍未解决 |
| **llama.cpp** | Prism Bonsai 2 27B；Gemma-4 E4B/E2B OpenCL；Xing4.0-29B-A4B 审核中；Hexagon IM2COL_3D；KDA 混合张量分割 WIP；SYCL Q8_0 MoE | OpenCL、Hexagon、SYCL、Vulkan、Metal、CUDA、CPU | **本地/边缘覆盖面领导者**；实验性后端在这里推进最快 |
| **Ollama** | 通过 `/v1/systemone` 使用 llama.cpp b11521 决策模型；EmbeddingGemma 2；d1-3B 请求；MLX 回归 | MLX、CUDA、Vulkan | **打包式本地用户体验领导者**，但依赖 llama.cpp，且目前在 MLX 上不稳定 |
| **LiteLLM** | RodiumAi provider、Wingback guardrail、虚拟模型；无模型 kernel | Provider/网关层 | **网关/provider 抽象领导者**，而非服务引擎 |
| **Unsloth** | Z-Image MLX 图像、Wan2.1 视频、aarch64 cu130、RAG embedding GPU 卸载、Intel GPU 请求 | MLX、CUDA、Intel | **微调/Studio 领导者**，正在扩展到图像/视频和可由智能体驱动的工作流 |

**谁领先：** vLLM 和 SGLang 在数据中心模型/硬件支持上领先。llama.cpp 在本地/边缘架构覆盖面上领先。Unsloth 在微调和 Studio 多模态支持上领先。LiteLLM 在 provider/路由集成上领先。Ollama 在开箱即用的本地用户体验上领先，但在后端稳定性上落后。

---

## 4. 性能前沿

优化集中在七个领域：

- **KV cache 与前缀复用：** vLLM 在混合/状态空间和投机路径上存在严重的前缀缓存损坏；SGLang 报告 DeepSeek-V4.1-Flash 多轮前缀缓存退化为恒定的约 11.5K token 命中；llama.cpp 在并发下存在 prompt 缓存污染；Unsloth 正在为并行聊天添加 KV 抢占。
- **投机解码：** vLLM 的 DFlash2/DSpark + 前缀缓存会损坏输出；SGLang 的 DFLASH/DSPARK 存在重复、OOM 以及首个非贪婪请求崩溃；llama.cpp 在量化目标上出现贪婪输出分歧。
- **MoE 与路由：** vLLM 扩展融合共享专家和 AITER 调优的 MoE 路由门控；SGLang 通过 aiter `topk_gating` 优化 512 专家 softmax MoE；llama.cpp 修复 Vulkan/Metal 上的重复专家 ID；Unsloth 标记 MoE-LoRA checkpoint 布局风险。
- **Attention/MLA 与量化 KV：** vLLM 通过 FlashAttn MLA 路由 XPU MLA，并添加 ROCm FP8 MLA prefill；SGLang 致力于 QSA 上下文并行和 fp8 KV cache；llama.cpp 改进 OpenCL flash attention。
- **分布式服务与批处理：** vLLM 添加 DeepSeek-V4 ROCm 双批重叠，并为自适应 prefill token 预算提出 RFC；SGLang 推进 prefill 上下文并行、PD 分离和 Mooncake decode；llama.cpp 的 RPC 后端在大型 MoE 分片时崩溃。
- **量化：** vLLM 重度依赖 FP8/NVFP4，并有一个未解决的 INT8 KV 请求；SGLang 出现 fp8 KV + QSA 崩溃；llama.cpp 优化 CM2、Q4/Q8 和 SYCL Q4_0；Unsloth 聚焦 QLoRA/MLX 重打包。
- **Kernel 与后端：** vLLM 使用 AITER、FlashInfer、Triton；SGLang 使用 FlashInfer autotune、aiter、Triton；llama.cpp 推进 Vulkan/OpenCL/SYCL/Hexagon；Ollama 与 MLX/CUDA/Vulkan 问题作斗争；Unsloth 移除 bitsandbytes 同步停顿。

**前沿要点：** 最快的收益不再只是“更多 FLOPS”。它们来自 **paged/前缀缓存正确性、MoE 路由、量化注意力和后端专用 kernel** — 而这些领域的回归可能会静默破坏输出质量。

---

## 5. 层次定位

| 层次 | 项目 | 角色 | 当前重点 |
|---|---|---|---|
| **服务引擎** | **vLLM、SGLang** | 高吞吐 GPU 服务器；paged attention、连续批处理、投机解码、分布式服务、多硬件 | vLLM：ROCm/XPU/CUDA、MLA、MoE、前缀缓存正确性。SGLang：PD 分离、上下文并行、FlashInfer autotune、扩散/ComfyUI。 |
| **本地运行时** | **llama.cpp、Ollama** | 在本地/边缘运行模型；GGUF/GGML、广泛后端、简单 UX | llama.cpp：ABI 可见性、Vulkan/OpenCL/SYCL/Metal 正确性、spec-decode 可靠性。Ollama：工具调用、MLX/CUDA/Vulkan 稳定性、打包。 |
| **网关/路由器** | **LiteLLM** | Provider 抽象、鉴权、费用跟踪、路由/故障转移、护栏、MCP | MCP OAuth/工具目录、Anthropic↔OpenAI 转换、Gemini 计费、虚拟模型。 |
| **训练/微调** | **Unsloth** | 高效 LoRA/QLoRA、本地 Studio、微调 + 推理工作流 | 静默梯度 bug、MoE-LoRA 导出、KV 抢占、优化器同步移除、MCP 驱动 Studio。 |

**重叠：** vLLM 和 SGLang 可作为后端位于 LiteLLM 之后。Ollama 捆绑 llama.cpp 并增加 registry/API UX。Unsloth 可以训练模型并通过 Studio 本地提供服务，与本地运行时重叠。

---

## 6. 趋势信号

1. **正确性成为新的性能瓶颈。** 前缀缓存和投机解码在混合、状态空间和 MoE 模型上并非安全的默认选项。关注 vLLM #60174/#53912、SGLang #40843/#41076/#43412 以及 llama.cpp #25618/#27148。
2. **硬件碎片化正在加速。** ROCm MI355X/gfx942、Intel XPU、SM120/Blackwell、Apple MLX、NPU、Vulkan/OpenCL/SYCL/Hexagon — 部署矩阵现在会实质性影响正确性和吞吐量。
3. **MoE 是默认的高端工作负载。** 路由、重复专家 ID、融合专家、MoE LoRA 和量化 MoE 出现在 vLLM、SGLang、llama.cpp 和 Unsloth 中。
4. **量化正在进入 KV cache 和注意力。** FP8、FP4/NVFP4、INT8 请求以及后端特定的 KV 行为现在是一级部署决策。
5. **智能体工具链对生产至关重要，但仍然脆弱。** 关注 Ollama 的 Qwen 3.8 流式 500 错误和 `format: "json"` 工具调用失败、vLLM 的 MiniMax-M3 流式工具调用边界情况，以及 LiteLLM 的 MCP OAuth/工具目录风险。
6. **网关准确性与网关正常运行时间同样重要。** LiteLLM 的 Gemini 工具使用计费缺口、`no-log` 费用跳过以及 Anthropic↔OpenAI 转换缺口可能会扭曲租户预算或破坏 Claude Code 风格的客户端。
7. **本地多租户服务需要默认隔离。** llama.cpp 由 RAM 支持的 prompt 缓存可能在并发下污染对话；Ollama 的 `keep_alive` 溢出和 Windows CUDA 损坏是运维陷阱。
8. **微调仍然存在静默失败模式。** Unsloth 的 Gemma 4 融合损失零梯度 bug 和 MoE-LoRA checkpoint 布局隐患意味着团队必须验证梯度和导出，而不仅仅是损失曲线。

**智能体/应用开发者现在应关注：**
- 如果使用前缀缓存、投机解码或混合/MoE 模型，请固定服务引擎版本。
- 在流式和非流式模式下测试工具调用；在 Ollama 上，在 #18944 修复之前，避免将 `format: "json"` 与工具调用一起使用。
- 将 MCP OAuth 刷新和工具目录固定视为 LiteLLM 中的生产问题。
- 不要根据 Ollama 上兼容 OpenAI 的 `timings.prompt_per_second` 来基准化 LiteLLM 容量 — 它目前虚高。
- 在发布 MoE 或 Gemma 4 适配器之前，验证微调梯度和 checkpoint 转换器。
- 对于本地部署，在启用多租户并发之前，审计 prompt 缓存隔离和后端稳定性。

---

## 各项目详细报告

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM 摘要 — 2026-10-12

*来源：github.com/vllm-project/vllm · 过去 24 小时更新 86 个 issue 和 464 个 PR · 无新版本发布*

---

## 1. 今日亮点

过去 24 小时没有新版本发布，因此关注点仍集中在正确性和平台适配。最主要的主题是 **混合/状态空间模型与投机解码路径上的前缀缓存损坏**：多个独立报告（[#60174](https://github.com/vllm-project/vllm/issues/60174)、[#53912](https://github.com/vllm-project/vllm/issues/53912)、[#57267](https://github.com/vllm-project/vllm/issues/57267)、[#57032](https://github.com/vllm-project/vllm/issues/57032)）描述了缓存命中后的静默输出损坏，针对 GDN/MTP 情况的修复 PR 正在推进（[#52244](https://github.com/vllm-project/vllm/pull/52244)）。与此同时，ROCm 势头强劲：MI355X 上针对 Qwen3.8-2.4T-A95B 和 Qwen3.8-Flash-Next 的新性能追踪器，以及 Kimi-K3 mono-decode 正在从 gfx950 移植到 gfx942（[#60997](https://github.com/vllm-project/vllm/pull/60997)）。

---

## 2. 发布与破坏性变更

无。过去 24 小时没有发布新 tag 或 release artifact，今天也没有公布 API/配置格式变更。

给跟踪版本的人的提示：[#60174](https://github.com/vllm-project/vllm/issues/60174) 报告了一个 **0.30.0 引入的行为回归**（DFlash2/DSpark drafter 上的前缀缓存），因此对于该配置，0.29.x 是最后一个已知良好的版本线。

---

## 3. 新模型与硬件支持

- **Intel XPU — MLA 支持**：PR [#58297](https://github.com/vllm-project/vllm/pull/58297) 通过现有的 `FLASH_ATTN_MLA` 后端在 XPU 上路由 MLA，使用 `vllm-xpu-kernels` 的 varlen flash attention，将 `q = [q_nope, q_pe]` 打包以对 latent cache 进行计算。
- **ROCm — MLA FP8 prefill（可选启用）**：PR [#59124](https://github.com/vllm-project/vllm/pull/59124) 添加了 `ROCM_AITER_FLYDSL_FP8`，这是一个面向 MI355X（gfx950）上 Kimi-K3 的 FlyDSL FP8 MLA prefill 后端。
- **ROCm — gfx942 上的 Kimi-K3**：PR [#60997](https://github.com/vllm-project/vllm/pull/60997) 将仅支持 gfx950 的 mono decode kernel 移植到 gfx942（256 个 CTA × 512 个线程，arch 现在成为 build key 的一部分）。
- **FlashInfer 0.7.1 升级**：PR [#60948](https://github.com/vllm-project/vllm/pull/60948) 将 CUDA 和 Rubin 构建中的 `flashinfer-python` 从 `0.7.0.post1` 升级到 `0.7.1`，显式选择 CuTe DSL KDA prefill 后端，以避免 B200 Kimi-K3 CI 中出现的 inference-tensor 崩溃。
- **Embedding LoRA**：PR [#61033](https://github.com/vllm-project/vllm/pull/61033) 为 `BertModel`、`RobertaModel` 和 `XLMRobertaModel`（BGE、all-MiniLM、multilingual-e5 系列）启用了 LoRA adapter。
- **功能请求（开放中）**：INT8 KV-cache 量化仍仅支持 FP8；请求跟踪于 [#33480](https://github.com/vllm-project/vllm/issues/33480)（22 条评论，12 👍）。

---

## 4. 性能与优化

**已合入 / 评审中的 PR**
- DeepSeek-V4 ROCm：针对采用 flattened expert TP 的数据并行部署，prefill 计算/通信双批重叠（DBO）— [#57773](https://github.com/vllm-project/vllm/pull/57773)。
- ROCm：通过 `clean_logits=False` 跳过 sparse prefill MQA-logits buffer 上的 `torch.full(..., -inf)` FP32 填充 — [#51314](https://github.com/vllm-project/vllm/pull/51314)。
- ROCm：MoE router gate 使用 AITER 调优的 GEMM（此前受 Hopper/Blackwell 限制）— [#50535](https://github.com/vllm-project/vllm/pull/50535)。
- ROCm：为 Qwen3-Next/Qwen3.5 启用融合 QK-norm + RoPE + gate 的 Triton kernel — [#51406](https://github.com/vllm-project/vllm/pull/51406)。
- ROCm：GLM-5.3-Flash 的 AITER 融合 shared experts 扩展到 gfx942（MI300X/MI325X）上的 TP2/4/8 — [#60604](https://github.com/vllm-project/vllm/pull/60604)。

**已报告的回归 / 尚未解决的性能差距**
- **SM120 + DeepSeek-V4.1-Flash**（8× RTX PRO 6000）：在 `--enforce-eager` 下 decode 吞吐极低，且 CUDA graphs 不可用；维护者被要求优先处理 SM120 graph capture — [#56892](https://github.com/vllm-project/vllm/issues/56892)。
- **SM120 上的 NVFP4 KV cache**：causal paged prefill 实测约 145.8 ms，相比 FP8 慢约 1.7–1.8× — [#50416](https://github.com/vllm-project/vllm/issues/50416)。
- **RDNA4（gfx1201）**：自 v0.28 起会选中 `RowWiseTorchFP8ScaledMMLinearKernel`，导致 decode 损失 5–24% — [#57838](https://github.com/vllm-project/vllm/issues/57838)。
- **Pooling 吞吐**：自 torch 2.11 起（vLLM v0.20.0+）回归约 18%，仍未解决 — [#52630](https://github.com/vllm-project/vllm/issues/52630)。
- **RDNA hybrid-Mamba**：回退到 Triton paged attention，长上下文下 decode 性能崩塌；已确定的修复是由 `on_gfx12x()` 门控的 split-KV decode kernel — [#50264](https://github.com/vllm-project/vllm/issues/50264)。
- **RFC — 自适应 prefill token 预算**：提议让 `max_num_batched_tokens` 能响应压力，而不是对长上下文工作负载保持静态 — [#52906](https://github.com/vllm-project/vllm/issues/52906)。

---

## 5. 稳定性与回归

按严重程度排序：

1. **静默输出损坏，DFlash2/DSpark + 前缀缓存**（0.30.0/0.31.0，Qwen3.8-27B，TP=1）— 在缓存命中时可复现；0.29.0 通过，MTP 不受影响。尚未关联修复 PR。[#60174](https://github.com/vllm-project/vllm/issues/60174)
2. **Hybrid Mamba/GDN + 前缀缓存 + MTP 仍会损坏输出** — 24 条评论；此前的 issue #43559 已关闭但未修复。修复正在评审：PR [#52244](https://github.com/vllm-project/vllm/pull/52244) 恢复 MTP 下的 GDN 前缀缓存命中（Qwen3.5-122B-A10B）。[#53912](https://github.com/vllm-project/vllm/issues/53912)
3. **CUDA graph 死锁，跨 TP rank 的 fully-sharded fused-MoE LoRA**（0.28.0，TP=2/4，35B-A3B MoE + 多个 adapter）— 在并发流量下挂起。[#60845](https://github.com/vllm-project/vllm/issues/60845)
4. **ROCm/gfx942 引擎在 AITER fused MoE 内部停滞**（GLM-5.3，TP8，v0.29.0）— 生成从 >2,000 tok/s 变为零且无报错，KV cache 约 20% 满。[#57429](https://github.com/vllm-project/vllm/issues/57429)
5. **Hybrid Mamba2 非确定性**，在 `mamba_cache_mode="all"` + 前缀缓存下 — 相同 batch 输入产生不同输出。[#57267](https://github.com/vllm-project/vllm/issues/57267)
6. **Gemma-4 MTP 投机解码在 CUDA graph capture 期间崩溃** — `_suppress_token_ids` 是 Python list，而不是 device tensor。[#48503](https://github.com/vllm-project/vllm/issues/48503)
7. **Gemma-4 MoE 初始化挂起和段错误**（0.20.x，2× RTX 3090，PCIe，无 NVLink）。[#42390](https://github.com/vllm-project/vllm/issues/42390)
8. **Sparse MLA indexer 在 KV sizing 之后分配 `expanded_block_table_buffer`** — 1M 上下文下约 1 GiB（B200，Model Runner V2，DSA 模型）。[#60450](https://github.com/vllm-project/vllm/issues/60450)
9. **MiniMax-M3（MXFP8）流式工具调用**：reasoning 关闭后立即出现空 delta + `finish_reason=stop`；相同的非流式 payload 成功。[#52613](https://github.com/vllm-project/vllm/issues/52613)
10. **Rust 前端一致性与路由修复**：`qwen3_xml` 工具解析现在与 Python parser 匹配（PR [#60136](https://github.com/vllm-project/vllm/pull/60136)），并且请求移除/中止后的 DP 路由已修正（PR [#59631](https://github.com/vllm-project/vllm/pull/59631)）。

今日关闭：gpt-oss 20B 工具调用 tokenizer bug（`Unexpected token 12606 while expecting start token 200006`）— [#22519](https://github.com/vllm-project/vllm/issues/22519)，在 39 条评论和 50 👍 后关闭。

---

## 6. 对应用开发者的意义

- **升级前审计前缀缓存。** 如果你服务 hybrid Mamba/GDN、Qwen3.8，或任何使用 DFlash/DSpark/MTP 投机解码的模型，前缀缓存目前是正确性风险，而不只是性能调优开关。在 0.30/0.31 上，安全选项是：固定到 0.29.x、禁用前缀缓存，或在 [#60174](https://github.com/vllm-project/vllm/issues/60174) 和 [#53912](https://github.com/vllm-project/vllm/issues/53912) 解决前禁用投机路径。
- **流式工具调用仍有边缘情况。** MiniMax-M3 流式工具调用失败（[#52613](https://github.com/vllm-project/vllm/issues/52613)）说明 agent loop 中应保留非流式回退路径，或根据预期工具调用输出来校验 `finish_reason`。长期存在的 gpt-oss 20B 工具调用 tokenizer bug 现已关闭（[#22519](https://github.com/vllm-project/vllm/issues/22519)）。
- **Rust 前端采用者应等待一致性 PR。** `VLLM_USE_RUST_FRONTEND=1` 搭配 `--tool-call-parser qwen3_xml` 在 [#60136](https://github.com/vllm-project/vllm/pull/60136) 合入前会产生错误输出；[#59314](https://github.com/vllm-project/vllm/pull/59314) 正在添加 per-request metrics（`--enable-per-request-metrics`），这对网关侧的 SLO 跟踪很有用。
- **LoRA 部署获得真正的正确性修复。** PEFT `rank_pattern` / `alpha_pattern` 之前被静默忽略，意味着 per-module adapter 以错误的 scaling 被服务 — 见 PR [#59801](https://github.com/vllm-project/vllm/pull/59801)。如果你提供带 per-module rank 的多租户 adapter，这会影响输出质量，而不只是指标。
- **本轮硬件选择比往常更重要。** SM120 上的 NVFP4 KV cache 在 prefill 上比 FP8 慢约 1.7–1.8×（[#50416](https://github.com/vllm-project/vllm/issues/50416)），而 SM120 上的 DeepSeek-V4.1 实际上需要 CUDA graphs 才能获得可用的 decode（[#56892](https://github.com/vllm-project/vllm/issues/56892)）。对 AMD 而言，MI355X Qwen3.8 tracker（[#57149](https://github.com/vllm-project/vllm/issues/57149)、[#59575](https://github.com/vllm-project/vllm/issues/59575)）表明 MXFP4 路径正在积极调优 — 预计会有可观的收益以 stacked PR 的形式落地。

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang 摘要 — 2026-10-12

## 1. 今日亮点
今天没有新的 SGLang 版本发布，但活动集中在 SM120/Blackwell 加固、投机解码正确性和 Q4 路线图上。PD 分离 Q4 路线图（[#43699](https://github.com/sgl-project/sglang/issues/43699)）和 DeepSeek V4.1 优化路线图（[#42170](https://github.com/sgl-project/sglang/issues/42170)）已更新，同时性能工作聚焦于 FlashInfer prefill 自动调优（[#42736](https://github.com/sgl-project/sglang/pull/42736)）、ROCm MoE 路由（[#40708](https://github.com/sgl-project/sglang/pull/40708)）和融合式 NaN 校验（[#43720](https://github.com/sgl-project/sglang/pull/43720)）。围绕 DFLASH/DSPARK 投机解码、fp8 KV cache 配合 QSA attention 以及 mamba cache 分配，仍有多个高严重性问题未关闭。

## 2. 版本发布与破坏性变更
- 过去 24 小时内**无新版本发布**。
- 没有已记录的破坏性 API/config 变更。
- 依赖迁移正在进行：Intel XPU 后端已升级到 PyTorch 2.14（[PR #42003](https://github.com/sgl-project/sglang/pull/42003)）。

## 3. 新模型与硬件支持
- **Diffusion/ComfyUI**：SGLD 集成模式下支持 Qwen-Image 2.1（[PR #43581](https://github.com/sgl-project/sglang/pull/43581)）；MiniMax H3、FastH3 和 VDN（[PR #43504](https://github.com/sgl-project/sglang/pull/43504)）；LTX-2.5 / LTX-2.3（[PR #43583](https://github.com/sgl-project/sglang/pull/43583)）。
- **Blackwell/SM120**：DeepSeek-V4.1-Flash 在 8× RTX PRO 6000 SM120 PCIe 上的生产现场报告（[Issue #40877](https://github.com/sgl-project/sglang/issues/40877)）；SM120 优化计划已关闭/不活跃（[Issue #19637](https://github.com/sgl-project/sglang/issues/19637)）；GLM-5.3-Flash SM120 fa4 后端崩溃仍未关闭（[Issue #42012](https://github.com/sgl-project/sglang/issues/42012)）。
- **NPU**：DSV4 A5 压缩器路由到显式 SWA 映射状态表（[PR #40816](https://github.com/sgl-project/sglang/pull/40816)）；修复 NPU 上 HiCache tensor-bool 二义性问题（[PR #43308](https://github.com/sgl-project/sglang/pull/43308)）。
- **ROCm**：Qwen3.8 512-expert MoE 通过 aiter `topk_gating` 路由（[PR #40708](https://github.com/sgl-project/sglang/pull/40708)）；由于 aiter 移除了 `pa_decode_sparse`，AMD DSV4 `compact_attention_hip` 出现故障（[Issue #43765](https://github.com/sgl-project/sglang/issues/43765)）。
- **Intel XPU**：PyTorch 2.14 升级（[PR #42003](https://github.com/sgl-project/sglang/pull/42003)）。
- **其他**：Moore Threads MUSA 支持路线图（[Issue #16565](https://github.com/sgl-project/sglang/issues/16565)）；SM120 的 B12X FlashInfer NVFP4 MoE 集成已关闭/不活跃（[Issue #33709](https://github.com/sgl-project/sglang/issues/33709)）。

## 4. 性能与优化
- **默认启用 FlashInfer prefill 自动调优**，适用于所有生成目标，包括 EAGLE/EAGLE3/STANDALONE draft；取代 DeepSeek-V4.1 和 GLM-5.3-Flash 的按模型 hook（[PR #42736](https://github.com/sgl-project/sglang/pull/42736)）。
- **ROCm 512-expert softmax MoE 路由**：通过 aiter `topk_gating`，在 8× MI355X 上，不使用 MTP 时输出 tok/s 提升 1.48%，使用 NEXTN MTP 时提升 2.22%（[PR #40708](https://github.com/sgl-project/sglang/pull/40708)）。
- **融合异步 NaN 校验**到单个只读 Triton kernel 中（[PR #43720](https://github.com/sgl-project/sglang/pull/43720)）。
- 修复 **TopK v2 threshold bucket 溢出**，避免静默选择错误值（[PR #43801](https://github.com/sgl-project/sglang/pull/43801)）。
- **Prefill Context Parallelism 路线图**：兼容 allreduce 融合、CP size != MoE DP size、MLA 模型和 SWA；更多 MHA/GQA 后端仍待处理（[Issue #21788](https://github.com/sgl-project/sglang/issues/21788)）。
- **Mamba chunked prefill** 保持在 mamba chunk grid 上，以便 DCP 可以捐赠 checkpoint（[PR #41701](https://github.com/sgl-project/sglang/pull/41701)）。
- **QSA Context Parallel 优化**（[PR #43792](https://github.com/sgl-project/sglang/pull/43792)）；**Mooncake decode host 接收**（[PR #43781](https://github.com/sgl-project/sglang/pull/43781)）；**DSpark/Dflash graph-eager 对齐与 Domino 候选池**（[PR #43790](https://github.com/sgl-project/sglang/pull/43790)、[PR #43791](https://github.com/sgl-project/sglang/pull/43791)）。

## 5. 稳定性与回归
按严重程度排序：

- **严重正确性问题 — DFLASH 投机解码**：GLM-5.3 使用 DFLASH 投机解码时，会在推理/输出中产生严重重复/退化循环（[Issue #40843](https://github.com/sgl-project/sglang/issues/40843)）。相关 DFLASH verify-mask 策略修复：[PR #43054](https://github.com/sgl-project/sglang/pull/43054)。
- **严重 OOM/崩溃 — DSPARK**：DeepSeek-V4.1-Flash + DSPARK 无界分配 `SparsePrefillWorkspace`，导致整个 TP 组 OOM 崩溃（[Issue #41076](https://github.com/sgl-project/sglang/issues/41076)）。
- **严重崩溃 — 投机解码**：首个非贪心请求会使服务器崩溃；FlashInfer verify-sampling JIT 失败并杀死进程树（[Issue #43412](https://github.com/sgl-project/sglang/issues/43412)）。
- **AMD 故障**：DSV4 `compact_attention_hip` 导入了 aiter 已移除的 `pa_decode_sparse` 模块，导致 gfx950 上的稀疏解码失效（[Issue #43765](https://github.com/sgl-project/sglang/issues/43765)）。
- **今日已关闭**：由于 bf16 query × fp8 key 的 `tl.dot` 不匹配，fp8 KV cache 导致 QSA 稀疏 attention kernel 崩溃（[Issue #43741](https://github.com/sgl-project/sglang/issues/43741)）。
- **Cache 回归**：DeepSeek-V4.1-Flash 多轮 prefix cache 退化为恒定的约 11.5K token 命中；60–80K 对话前缀每一轮都会被重新 prefill（[Issue #43738](https://github.com/sgl-project/sglang/issues/43738)）。
- **PD / 内存**：运行时角色切换在 decode → prefill → decode 后因非法内存访问而崩溃（[Issue #43557](https://github.com/sgl-project/sglang/issues/43557)）；当所有缓存状态都被锁定时，mamba cache 分配断言会杀死引擎（[Issue #43204](https://github.com/sgl-project/sglang/issues/43204)）。
- **SM120 attention**：GLM-5.3-Flash fa4 在 CUDA-graph 捕获时崩溃；triton 是唯一可用的后端（[Issue #42012](https://github.com/sgl-project/sglang/issues/42012)）。
- **其他未关闭 bug**：unified_cache MAMBA 驱逐会剪枝节点而不是降级，破坏 host-tier loadback（[Issue #33713](https://github.com/sgl-project/sglang/issues/33713)）；`--bf16-gemm-backend gemv` 对未量化线性层被忽略（[Issue #42085](https://github.com/sgl-project/sglang/issues/42085)）；`get_token_ids_logprobs` 返回 `[]` 而不是 tensor，并导致 decode logprob 后处理崩溃（[Issue #43154](https://github.com/sgl-project/sglang/issues/43154)）；多模态预处理期间音频解码无界（[Issue #43708](https://github.com/sgl-project/sglang/issues/43708)）。Qwen-VL 视频解码内存限制的修复 PR：[PR #43800](https://github.com/sgl-project/sglang/pull/43800)。
- **已关闭/不活跃**：DP 健康检查路由扰动（[Issue #35241](https://github.com/sgl-project/sglang/issues/35241)）；CUDA graph 下的 DeepEP 惰性初始化（[Issue #29942](https://github.com/sgl-project/sglang/issues/29942)）；DSV4 indexer 中 1M-token prefill OOM（[Issue #34155](https://github.com/sgl-project/sglang/issues/34155)）；Kimi-K3 的 DSpark CUDA 启动失败（[Issue #34522](https://github.com/sgl-project/sglang/issues/34522)）；DSV4 DSpark topk=192 SM120 崩溃（[Issue #33943](https://github.com/sgl-project/sglang/issues/33943)）；HIP/ROCm SWA 前缀复用 RFC（[Issue #34562](https://github.com/sgl-project/sglang/issues/34562)）；禁用多模态权重加载（[Issue #28194](https://github.com/sgl-project/sglang/issues/28194)）。

## 6. 这对应用开发者意味着什么
- **投机解码并非在所有模型上都默认安全。** 如果你使用 DFLASH/DSPARK/DSpark 服务 GLM-5.3 或 DeepSeek-V4.1，请对非贪心流量进行门控，并监控是否出现重复、OOM 或非法内存访问（[Issue #40843](https://github.com/sgl-project/sglang/issues/40843)、[Issue #41076](https://github.com/sgl-project/sglang/issues/41076)、[Issue #43412](https://github.com/sgl-project/sglang/issues/43412)）。
- **在验证之前，避免将 fp8 KV cache 与 QSA/Qwen4-Exp 一起使用。** 该崩溃问题截至今日已关闭，但在生产使用前请先测试（[Issue #43741](https://github.com/sgl-project/sglang/issues/43741)）。
- **SM120/RTX PRO 6000 用户应谨慎选择 attention 后端。** DSV4.1-Flash 有可用的生产配置，但 fa4 可能崩溃；triton 仍是回退方案（[Issue #40877](https://github.com/sgl-project/sglang/issues/40877)、[Issue #42012](https://github.com/sgl-project/sglang/issues/42012)）。
- **AMD 用户必须协调 aiter 版本。** 已移除的 API 会破坏 gfx950 上的 DSV4 稀疏解码（[Issue #43765](https://github.com/sgl-project/sglang/issues/43765)）。
- **预期 FlashInfer 自动调优、ROCm MoE 路由和融合校验会带来性能收益**，但升级前请在你的工作负载上验证（[PR #42736](https://github.com/sgl-project/sglang/pull/42736)、[PR #40708](https://github.com/sgl-project/sglang/pull/40708)、[PR #43720](https://github.com/sgl-project/sglang/pull/43720)）。
- **Diffusion/ComfyUI 支持正在扩展**，覆盖 Qwen-Image、MiniMax H3 和 LTX-2.5/2.3；如果你通过 SGLang 提供多模态生成服务，这会很有用（[PR #43581](https://github.com/sgl-project/sglang/pull/43581)、[PR #43504](https://github.com/sgl-project/sglang/pull/43504)、[PR #43583](https://github.com/sgl-project/sglang/pull/43583)）。

---

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp 摘要 — 2026-10-12

## 1. 今日亮点

后端工作密集的一天：`ggml` 现在默认使用隐藏符号可见性（b11563），收紧了共享库 ABI 暴露面；同时，针对重复专家 ID 的 MoE 相关正确性修复已进入 Vulkan（b11554），对应的 Metal 修复正在审核中（#30373）。OpenCL 获得了 Gemma-4 decode 和 flash-attention 改进（b11559），首个对 Prism Bonsai 2 27B 的运行时支持并入代码树（b11556）。投机解码仍是问题跟踪器中正确性和性能抱怨的主要来源。

## 2. 发布与破坏性变更

- **b11563 — `ggml`：默认符号可见性改为 hidden**（[#30014](https://github.com/ggml-org/llama.cpp/pull/30014)，[发布](https://github.com/ggml-org/llama.cpp/releases/tag/b11563)）。此前通过 `GGML_API` 泄漏的内部符号不再导出。**迁移说明：** 下游项目如果链接了非公开的 GGML 符号（直接链接或通过 `dlsym`），可能需要改为静态构建，或向上游提交显式导出。对嵌入方而言，这是今天影响最大的一项变更。
- **b11557 — RPC：`GGML_RPC_DEBUG` 变为详细级别**（[#29544](https://github.com/ggml-org/llama.cpp/pull/29544)）。现在按整数解析：`0`/未设置表示禁用，`1–3` 会输出越来越详细的事件/命令/传输日志。将 `GGML_RPC_DEBUG=1` 当作布尔值使用的脚本仍可工作，但日志量会增加。
- **b11556 — Prism Bonsai 2 27B 运行时支持**（[#29600](https://github.com/ggml-org/llama.cpp/pull/29600)）。新增（并折叠）Hadamard 张量；转换工具已更新。
- 该时间窗口内的其他构建：b11564、b11562、b11560、b11559、b11558、b11555、b11554 — 见下文。

## 3. 新模型与硬件支持

- **Prism Bonsai 2 27B** — 运行时支持已合入（b11556，[#29600](https://github.com/ggml-org/llama.cpp/pull/29600)）。
- **OpenCL 上的 Gemma-4 E4B/E2B GPU decode** — 扩展了 FA 路径，包括面向 Gemma-4 的 `dk512` 和改进的 `dk64`（[b11559, #30266](https://github.com/ggml-org/llama.cpp/pull/30266)）。
- **Xing4.0-29B-A4B（TeleAI）** — 模型支持 + 转换正在审核中（[#29012](https://github.com/ggml-org/llama.cpp/pull/29012)）。
- **Hexagon：`IM2COL_3D`** 带 DMA 路径，并降低了标量 fp16 转换开销（[b11560, #30305](https://github.com/ggml-org/llama.cpp/pull/30305)）；面向 Clef-Flash 决策模型（文本 + 视觉）的 Hexagon 工作正在审核中（[#30143](https://github.com/ggml-org/llama.cpp/pull/30143)）。
- **KDA 混合模型：tensor-split 支持** — WIP（[#30277](https://github.com/ggml-org/llama.cpp/pull/30277)）。
- **SYCL：重排 Q8_0 MoE 权重** — 按专家的 `[qs][d]` 布局，并重排 matvec 调度（[#29337](https://github.com/ggml-org/llama.cpp/pull/29337)）。
- **SYCL 构建灵活性** — 现在可以用 `ExternalProject` 和 icpx 构建后端，而代码树其余部分使用其他编译器（例如 ROCm）（[#29506](https://github.com/ggml-org/llama.cpp/pull/29506)）。

## 4. 性能与优化

- **Vulkan CM2 量化块大小** — 三元表达式避免 CM2 matmul 中的 switch lowering；块大小已特化（[b11564, #30164](https://github.com/ggml-org/llama.cpp/pull/30164)）。
- **Vulkan f16 FMA packing** — 在没有 dot-product 操作的地方，标量 FMA 变为 `v_pk_fma_f16`（位级完全相同的顺序），针对 RADV packing（[#29877](https://github.com/ggml-org/llama.cpp/pull/29877)）。
- **OpenCL flash-attention** — 为 E4B 和 E2B 启用 Gemma-4 GPU decode（[b11559](https://github.com/ggml-org/llama.cpp/releases/tag/b11559)）。
- **OpenCL kernel/image 修复** — Q4_K 权重 image 超出设备限制时的回退；subgroup 安全的 `rms_norm`；Q4_0 broadcast 修复（[b11558, #30310](https://github.com/ggml-org/llama.cpp/pull/30310)）。
- **SYCL Q4_0 dequantize 合并** — 修复了权重重排后首次 decode 之后出现的 prompt processing 变慢问题（[#30374](https://github.com/ggml-org/llama.cpp/pull/30374)）。
- **CPU：AVX2 构建现在在可用时使用 F16C**（[b11555, #30270](https://github.com/ggml-org/llama.cpp/pull/30270)）。
- **Vulkan `mul_mat_id` 重复 ID** — row-ID hoisting 扩展为始终应用，当 ID 重复时计算某个专家的每一行，移除了一个逐循环路径（[b11554, #29998](https://github.com/ggml-org/llama.cpp/pull/29998)）。
- **CI：覆盖所有 AMD 架构的 HIP register-spill 检查**（[#30348](https://github.com/ggml-org/llama.cpp/pull/30348)）。
- **回归观察：** 对于某个 GDN-hybrid 模型，Vulkan prompt processing 在 b10455 到 b11429 之间于 RDNA1（gfx1010）上仍慢约 45%，TG 不受影响（[#30039](https://github.com/ggml-org/llama.cpp/issues/30039)）。此前报告的 RTX 5060 Ti 上使用 `NV_coopmat2` 时 Q8_0 PP 出现 16–19% 回归的问题，自 #25773 起已关闭（[#30000](https://github.com/ggml-org/llama.cpp/issues/30000)）。

## 5. 稳定性与回归

按严重程度排序：

1. **大 MoE 模型下 RPC 后端崩溃** — 在预热解码时出现“Remote RPC server crashed or returned malformed response”（`ggml-rpc.cpp:566`），将 DeepSeek-R1-671B 分片到 3× NVIDIA GB10（DGX Spark）节点上时 100% 复现。尚未关联修复 PR（[#30052](https://github.com/ggml-org/llama.cpp/issues/30052)）。
2. **RTX 5090 Laptop / sm_120 上的 CUDA graphs 挂起** — RC watchdog + Xid 8；`GGML_CUDA_DISABLE_GRAPHS=1` 是完整的规避方案。仍开放（[#27330](https://github.com/ggml-org/llama.cpp/issues/27330)）。
3. **投机（MTP）解码期间 Flash-attention 维度断言** — Vulkan 上出现 `GGML_ASSERT(neq0 == HSK)`（[#29418](https://github.com/ggml-org/llama.cpp/issues/29418)），以及 SYCL 上使用 gemma4-assistant 时出现 head 维度不匹配崩溃（[#29419](https://github.com/ggml-org/llama.cpp/issues/29419)）。
4. **投机解码正确性** — 在*量化*目标上，贪心采样时 draft-model 投机与 vanilla 输出发生分歧（bf16 一致），同一目标上的 ngram 投机也发生分歧。开放中，30 条评论（[#25618](https://github.com/ggml-org/llama.cpp/issues/25618)）。相关：启用投机后，服务器对每个 token 返回 `logprob 0.0`（[#27972](https://github.com/ggml-org/llama.cpp/issues/27972)）；ngram-cache 的 `begin()` 是空操作，接受率从 **86% → 11%** 下降，并变得比不投机还慢（[#27852](https://github.com/ggml-org/llama.cpp/issues/27852)）。
5. **服务器 prompt/KV 缓存污染** — 基于 RAM 的 prompt cache（`--cache-ram`/`--cache-idle-slots`，两者默认开启）在并发负载下可能将一段无关的已完成对话恢复到新 slot 中（[#27148](https://github.com/ggml-org/llama.cpp/issues/27148)）。对多租户部署影响很大。
6. **视觉模型 KV 缓存保存损坏** — 对启用视觉的模型，`/slots/3?action=save` 失败；现已关闭（[#19466](https://github.com/ggml-org/llama.cpp/issues/19466)）。
7. **OpenVINO 崩溃（STATUS_ILLEGAL_INSTRUCTION）** 归因于 Core Ultra 7 265K 上的 AVX-512；开放中（[#28726](https://github.com/ggml-org/llama.cpp/issues/28726)）。
8. **Metal `MUL_MAT_ID` 重复 ID** — 行错误/丢失，并可能溢出到下一个专家切片；修复 PR 开放中（[#30373](https://github.com/ggml-org/llama.cpp/pull/30373)）。
9. **针对越界读取的加固 PR** — `ggml_roll` 的有符号溢出防护在 `INT_MIN` 下被绕过（[#30375](https://github.com/ggml-org/llama.cpp/pull/30375)）；`ggml_ssm_scan` 用未检查的 `ids` 索引状态（[#30372](https://github.com/ggml-org/llama.cpp/pull/30372)）；`ggml_opt_fit` 传入 8 字节的 `epoch`，但读取的是 28 字节的 params 结构体（[#30371](https://github.com/ggml-org/llama.cpp/pull/30371)）。
10. **工具请求后的服务器检查点选择** — 次优的检查点选择会在多轮工具流程中抹掉有效上下文；修复开放中（[#24891](https://github.com/ggml-org/llama.cpp/pull/24891)）。
11. **SYCL 内存错误** 出现在 `mul_mat`、split buffer 和 host pool 中（[#29889](https://github.com/ggml-org/llama.cpp/pull/29889)）；DMMV 行尾保护（[#29338](https://github.com/ggml-org/llama.cpp/pull/29338)）；此前的 tensor-split 内存泄漏已关闭（[#27845](https://github.com/ggml-org/llama.cpp/issues/27845)）。
12. **Jinja `tojson` 浮点格式化** — 当前会截断为 6 位有效数字并丢弃 `.0`，与 Jinja2/Python 不一致（`48.858844` → `48.8588`）；修复开放中（[#30362](https://github.com/ggml-org/llama.cpp/pull/30362)）。

## 6. 这对应用开发者意味着什么

- **嵌入方和打包方应针对 b11563 进行测试。** 切换到隐藏符号可见性可能会破坏依赖泄漏的内部 GGML 符号的构建。同样，Homebrew 的 0.5.0 打包历来不包含 Web UI（[#29849](https://github.com/ggml-org/llama.cpp/issues/29849)）— 请确认你的发行版包含 `tools/server` 资产。
- **对于量化目标，将投机解码视为可选功能。** 多个开放 issue 显示，当目标权重为 Q4_K_M 级别时，会出现贪心输出分歧和无效 logprob，此外还有一个 ngram-cache 状态 bug 会逆转预期加速效果。在生产环境启用前，请针对每个模型/量化验证接受率和输出确定性。
- **在多租户服务器中审查 prompt-cache 默认值。** `--cache-ram` 和 `--cache-idle-slots` 默认启用，并且已有文档记录了并发下跨对话污染案例（[#27148](https://github.com/ggml-org/llama.cpp/issues/27148)）。禁用它们会以延迟换取隔离性。
- **在 GB10/统一内存平台上，通过 `ggml-rpc-server` 进行分布式 MoE 服务尚未达到生产安全级别**（[#30052](https://github.com/ggml-org/llama.cpp/issues/30052)）。请预留回退到单节点或单主机内 tensor-split 的方案。
- **Intel 和 Adreno 用户可获得实际收益。** OpenCL FA/Gemma-4 decode 工作、image 限制回退以及 SYCL Q4_0/MoE 重排修复显著改善 prompt processing — 如果你遇到 decode 后变慢，请拉取最新构建。
- **如今 Vulkan/Metal 上的 MoE 更正确了。** 重复专家 ID（在 top-k 路由中很常见）现在已在 Vulkan 的 prepass 中处理，并在 [#30373](https://github.com/ggml-org/llama.cpp/pull/30373) 中为 Metal 修复；如果你在这些后端上服务 MoE 模型，请更新。
- **如果你部署在 RX 5700 XT 级别硬件上，请关注 RDNA1 PP 回归**（[#30039](https://github.com/ggml-org/llama.cpp/issues/30039)）；生成速度不受影响，因此长 prompt agent 是主要风险。

---

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama 摘要 — 2026-10-12

## 今日要闻

过去 24 小时没有新版本发布，但最大已落地变更是 PR #18917（已关闭），将 llama.cpp 升级到 b11521，并通过 `/v1/systemone` 新增对决策模型的内置支持，以及 **EmbeddingGemma 2**。当日活动主要由 **工具调用正确性** 主导：长期存在的 Qwen 3.8 流式失败（#17778，52 条评论）、设置 `format: "json"` 后工具调用出错（#18944），以及 Go API 客户端中的静默数值精度丢失（#18942）。硬件方面，Apple Silicon 上的 MLX 持续产生崩溃报告（#18940、#18885），Windows 更新正在导致 CUDA 后端损坏（#18712）。

## 发布与破坏性变更

过去 24 小时无新版本发布。

进行中或近期已关闭的破坏性/行为变更：

- **`reasoning_effort: "xhigh"` 被重写为 `"max"`** 导致 Qwen3.8 系列模型通过 `/v1/chat/completions` 请求时出现 HTTP 500（已关闭，#18943） — https://github.com/ollama/ollama/issues/18943
- **`OLLAMA_NUM_PARALLEL` 被静默固定为 1**，涉及 `server/sched.go` 中十一种混合/循环架构（`qwen35moe`、`mllama`、`qwen3vl*` 等）（已关闭，#18938）；提议的修复新增 `OLLAMA_PARALLEL_HYBRID`（#18939） — https://github.com/ollama/ollama/issues/18938 | https://github.com/ollama/ollama/pull/18939
- **`keep_alive` int64 溢出** 会导致模型立即卸载，而不是保持驻留；计划通过钳制修复（#18946） — https://github.com/ollama/ollama/pull/18946
- **`manifests-v2`** 别名行为受到质疑，此前 `ollama list` 中出现异常标签别名（已关闭，#18870） — https://github.com/ollama/ollama/issues/18870

## 新模型与硬件支持

- **llama.cpp b11521（PR #18917，已关闭）** — 新增对决策模型（`/v1/systemone`，提示词由 llama.cpp 构建，移除 Ollama 自己的 scorer/Clef 补丁）以及 **EmbeddingGemma 2** 的内置支持 — https://github.com/ollama/ollama/pull/18917
- **d1-3B / d1-omni-600M 请求（#18890）** — LiquidAI 决策模型，支持图像（以及音频，600M）；目前失败并报 `unsupported decision encoding "lfm2-d1"` — https://github.com/ollama/ollama/issues/18890
- **EmbeddingGemma 2 在 Linux 上拉取失败（#18825，14 👍）** — `embeddinggemma-2:740m` 需要 MLX 运行时，因此 Linux 用户无法拉取 — https://github.com/ollama/ollama/issues/18825
- **MLX 后端回归（#18885、#18940）** — 在 M 系列芯片上使用小型 Gemma 4 MLX 模型时出现 `mlx runner failed: panic`；M2 Max 上 `head_dim=256`（Qwen 3.8:27b / Gemma 4:31b）因 threadgroup 请求为 960、超过硬件上限 896 而触发 SDPA 内核崩溃 — https://github.com/ollama/ollama/issues/18885 | https://github.com/ollama/ollama/issues/18940
- **Vulkan AMD 崩溃（#18557）** — 在 RX 6800 XT（驱动 32.0.21045.5002）上加载任意模型时出现 `0xc0000005` 访问冲突；CPU 路径不受影响 — https://github.com/ollama/ollama/issues/18557

## 性能与优化

- **OpenAI 兼容 timings 被夸大（#18948）** — `/v1/*` 上的 `timings.prompt_per_second` 将总 prompt token 数除以仅未缓存时间，导致 prompt 吞吐量按缓存命中率被高估 — https://github.com/ollama/ollama/issues/18948
- **混合模型并行（#18939，未关闭）** — 解除单槽位固定，使混合/循环模型可使用多个序列（llama-server 已运行 `kv_unified=false`，每个序列一个 KV cache） — https://github.com/ollama/ollama/pull/18939
- **`shift` 不匹配时调度器重载（#18950）** — `server/sched.go` 中的 `needsReload` 会在请求的 `shift` 与已加载 runner 不同时强制重载；提议的修复是以支持 shift 的方式启动，并按请求强制 `shift:false`（在 `gemma4-26b`、131072 ctx、ROCm RX 7900 XTX 上观察到） — https://github.com/ollama/ollama/issues/18950
- **经过验证的 manifest/模型复用（PR #18947，未关闭）** — 将命名 manifest 解析和 runner 选择整合为一次经过验证的冷查找，并在 generate/chat/embed/System 中复用 — https://github.com/ollama/ollama/pull/18947
- **多 GPU VRAM 核算（#18349）** — 调度器用发现名称（`CUDA1`）去查以子 llama-server 日志名称（`CUDA0`）为键的映射表，从而与实际用量发生偏离 — https://github.com/ollama/ollama/issues/18349
- **Thinking token 预算（PR #17566，未关闭）** — 按请求/按模型限制推理 token，以阻止模型在 thinking 块中循环并耗尽整个上下文 — https://github.com/ollama/ollama/pull/17566

## 稳定性与回归

按严重程度排序：

1. **MLX SDPA 内核崩溃，Apple Silicon（#18940，未关闭）** — M2 Max 上 `head_dim=256` 时在 prefill 期间发生严重 panic；尚无修复 PR — https://github.com/ollama/ollama/issues/18940
2. **MLX runner panic（#18885，未关闭）** — 在 `gemma4:e2b-mlx`、Mac mini M6 / 16 GB 上反复失败 — https://github.com/ollama/ollama/issues/18885
3. **AMD 上 Vulkan 访问冲突（#18557，未关闭）** — 任意模型都会以 `exit status 0xc0000005` 使 `llama-server` 崩溃；与 #18494 是同一底层问题 — https://github.com/ollama/ollama/issues/18557
4. **Windows 自动更新将 `ggml-cuda.dll` 留为 `.tmp`（#18712，未关闭）** — VRAM 检测为 0 B，静默回退到 CPU；PR #18951 保留更早的升级日志以辅助诊断 — https://github.com/ollama/ollama/issues/18712 | https://github.com/ollama/ollama/pull/18951
5. **Qwen 3.8 流式 500：“no user query found in messages”（#17778，未关闭，30 👍，52 条评论）** — 在 205k 上下文的工具调用循环期间发生；是参与度最高的未关闭 bug — https://github.com/ollama/ollama/issues/17778
6. **设置 `format: "json"` 后工具调用不可用（#18944，未关闭）** — 模型要么在响应中内联调用，要么伪造一个；不设置 `format` 时可正常工作 — https://github.com/ollama/ollama/issues/18944
7. **Ollama Cloud：`glm-5.3-flash` 流永不终止（#18926，未关闭）** — 工作已完成，但客户端在退出时约 0% CPU 挂起 — https://github.com/ollama/ollama/issues/18926
8. **Windows 上模型路径位于 NTFS 挂载点时 `ollama list` 为空（#18921，未关闭）** — 修复 PR #18949 通过 junction/卷挂载点解析模型 — https://github.com/ollama/ollama/issues/18921 | https://github.com/ollama/ollama/pull/18949
9. **Raspberry Pi 5：第二次 `ollama run` 挂起（#18796，未关闭）** — 第一次运行正常，后续运行停滞且无输出 — https://github.com/ollama/ollama/issues/18796
10. **Go API 在工具调用参数中丢失数值精度（#18942，未关闭）** — `9007199254740993` 被解码为 `9007199254740992`；修复 PR #18941 带有回归测试 — https://github.com/ollama/ollama/issues/18942 | https://github.com/ollama/ollama/pull/18941
11. **`qwen3-coder:480b-cloud` 忽略 Cloud JSON schema（#12362，未关闭）** — 在本地 `qwen3-coder:30b` 上正常；云端行为不同 — https://github.com/ollama/ollama/issues/12362

近期已关闭（无需操作）：#12940（Windows runner 退出状态 2）、#7288（Windows embedding `wsarecv`）、#13037（`ollama run` 在 Windows 上未启动服务器）、#18447（canonicalization 导致间歇性 “model not found”）、#8431（`bge-large` GGML 断言，通过 `num_ctx` 解决）、#6887（reader-lm temperature）、#8329（dolphin3 tools 模板）。

## 这对应用开发者意味着什么

- **工具调用目前是薄弱环节。** 如果你的智能体依赖结构化工具调用，避免将其与 `"format": "json"` 结合使用（#18944），并注意 Qwen 3.8 可能在流中途循环进入 500（#17778）。如果你使用 **Go 客户端**，数值型工具调用参数会静默舍入——请锁定 #18941 中的修复，或在修复落地前将高精度整数作为字符串传递（#18942）。
- **云端与本地不可互换。** 两个未关闭报告（#12362、#18926）显示，云端模型在 JSON schema 遵循和流终止方面与本地行为存在差异。如果你在两者之间路由，请测试两条路径。
- **推理控制正变得更严格。** `xhigh`→`max` 重写（#18943）和提议的 token 预算（#17566）表明 `think`/`reasoning_effort` 语义仍在变化——请将其视为对版本敏感，并按模型系列测试聊天模板。
- **Mac 用户升级前应验证。** MLX 在 `head_dim=256` 模型（#18940）和小型 Gemma 4 MLX 变体（#18885）上发生 panic，使 Apple Silicon 目前成为最新模型系列的一个坎坷目标平台。
- **关注运维相关修复：** `keep_alive` 溢出（#18946）可能在第一个请求时就卸载长期驻留的模型，而 OpenAI 兼容的 `timings.prompt_per_second`（#18948）目前被夸大——不要用这些数字建立容量基线。
- **Windows 部署：** 自动更新后验证 `ggml-cuda.dll` 完整性（#18712），并在 #18949 落地前避免将 `OLLAMA_MODELS` 放在 NTFS 挂载点之下（#18921）。

---

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM 摘要 — 2026-10-12

**范围：** [github.com/BerriAI/litellm](https://github.com/BerriAI/litellm) — 过去 24 小时内更新了 74 个 issue 和 226 个 PR。

## 1. 今日亮点
LiteLLM 发布了 [v1.106.0-rc.1](https://github.com/BerriAI/litellm/releases/tag/v1.106.0-rc.1)、[v1.105.0](https://github.com/BerriAI/litellm/releases/tag/v1.105.0) 和 [v1.104.3](https://github.com/BerriAI/litellm/releases/tag/v1.104.3)，提供的发布说明细节仅有 Docker cosign 签名验证。Issue 讨论仍集中在 MCP 网关认证/工具目录正确性、Anthropic↔OpenAI 转换缺口，以及 Gemini token 计费。开放的 PR 新增了 [虚拟模型路由](https://github.com/BerriAI/litellm/pull/46024)、[RodiumAi 提供商支持](https://github.com/BerriAI/litellm/pull/46023)、[Wingback 护栏](https://github.com/BerriAI/litellm/pull/38685) 和 [定时邮件支出报告](https://github.com/BerriAI/litellm/pull/45954)。

## 2. 发布与破坏性变更
- **v1.106.0-rc.1** — 发布候选版：[发布](https://github.com/BerriAI/litellm/releases/tag/v1.106.0-rc.1)
- **v1.105.0** — [发布](https://github.com/BerriAI/litellm/releases/tag/v1.105.0)
- **v1.104.3** — [发布](https://github.com/BerriAI/litellm/releases/tag/v1.104.3)
- **Docker 签名：** 所有 LiteLLM Docker 镜像均使用 cosign 和 [commit `0112e53`](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0) 中引入的密钥进行签名。
- 提供的发布数据中未包含明确的破坏性 API/配置变更或迁移说明。请将 v1.106.0-rc.1 视为预发布版。

## 3. 新模型与硬件支持
- 本时间窗口内未出现新的 CUDA/ROCm/Metal/CPU、量化或本地模型后端支持。
- **RodiumAi OpenAI 兼容提供商** — PR #46023 新增 `rodiumai`，用于非洲/移动支付接入：[PR #46023](https://github.com/BerriAI/litellm/pull/46023)
- **Wingback 护栏提供商** — PR #38685 新增 `guardrail: wingback`，用于运行时安全：[PR #38685](https://github.com/BerriAI/litellm/pull/38685)
- **用于提供商路由的虚拟模型** — PR #46024 将部署分组到 `virtual_model/<name>` 下，支持偏好/故障转移：[PR #46024](https://github.com/BerriAI/litellm/pull/46024)
- **AWS Bedrock 自动发现** — 功能请求 #43568 已关闭；数据中没有已落地支持的细节：[Issue #43568](https://github.com/BerriAI/litellm/issues/43568)

## 4. 性能与优化
- 提供的数据中没有落地的吞吐量、延迟或内存基准测试。
- 路由/故障转移工作正在进行：
  - 用于灵活提供商路由和故障转移的虚拟模型：[PR #46024](https://github.com/BerriAI/litellm/pull/46024)
  - 会话亲和性固定（session-affinity pin）在饱和且另一部署空闲时让出：[Issue #45868](https://github.com/BerriAI/litellm/issues/45868)
  - 复杂度路由器应钳制而不是覆盖调用方的 `max_tokens`：[Issue #44869](https://github.com/BerriAI/litellm/issues/44869)
- 测试/基础设施可靠性：
  - Ownership 代理 DB 连接池余量：[PR #46025](https://github.com/BerriAI/litellm/pull/46025)
  - Redis 端口冲突重试：[PR #46019](https://github.com/BerriAI/litellm/pull/46019)

## 5. 稳定性与回归
按影响排序。

**严重 — 计费/支出正确性**
- Gemini 工具使用的提示 token 被计入 `prompt_tokens`，但没有计费（#45991）。修复 PR：#46017（已关闭）、#46021（开放）。[Issue #45991](https://github.com/BerriAI/litellm/issues/45991) · [PR #46017](https://github.com/BerriAI/litellm/pull/46017) · [PR #46021](https://github.com/BerriAI/litellm/pull/46021)
- `no-log: true` 在成功请求时会跳过代理自身的支出跟踪（#44503）。[Issue #44503](https://github.com/BerriAI/litellm/issues/44503)
- 部署自定义定价（`input_cost_per_token: 0`）在模型成本映射重新加载后被静默忽略；按请求定价会覆盖规范条目（#30081）。[Issue #30081](https://github.com/BerriAI/litellm/issues/30081)

**高 — MCP 与协议转换**
- MCP Gateway OAuth 凭据不会刷新；token 过期后工具会消失（#33072）。[Issue #33072](https://github.com/BerriAI/litellm/issues/33072)
- MCP 工具固定机制忽略注解和 `outputSchema`；`readOnlyHint` 可能变为破坏性操作，且没有漂移告警（#44093）。[Issue #44093](https://github.com/BerriAI/litellm/issues/44093)
- Anthropic `/v1/messages` 在转换为 OpenAI 工具消息时丢弃 `tool_result.is_error`（#44979）。[Issue #44979](https://github.com/BerriAI/litellm/issues/44979)
- Claude Code 自动分类器在 GPT-5.6 模型上失败：对于 OpenAI 格式提供商，`/v1/messages` 不会做 `stop_sequences` → `stop` 的转换，导致 400（#33075）。[Issue #33075](https://github.com/BerriAI/litellm/issues/33075)
- A2AConfig 声明支持 `temperature`、`max_tokens` 和 `top_p`，但随后静默丢弃这三者（#45944）。[Issue #45944](https://github.com/BerriAI/litellm/issues/45944)
- 转换为 Gemini 格式时，Responses API 命名空间工具会被剥离；仅保留名称（#29854）。[Issue #29854](https://github.com/BerriAI/litellm/issues/29854)

**中 — 路由器/提供商边界情况**
- `router_settings.enable_pre_call_checks: true` 不会在 `/v1/messages` 上强制执行部署 RPM（#45987）。[Issue #45987](https://github.com/BerriAI/litellm/issues/45987)
- Gemini 3 提供商总是注入 `temperature=1.0`；Google 可能会在即将推出的模型上拒绝采样参数（#45333）。[Issue #45333](https://github.com/BerriAI/litellm/issues/45333)
- Databricks chat→Responses→chat 桥接会将字符串用户内容转换为文本块，从而破坏 `json_object`（#45618）。[Issue #45618](https://github.com/BerriAI/litellm/issues/45618)
- Databricks 托管的 GPT-5.6 系列函数工具在 `/chat/completions` 上因 `reasoning_effort` 错误而失败（#43708）。[Issue #43708](https://github.com/BerriAI/litellm/issues/43708)
- 即使 Bedrock Invoke 支持 per-turn-control，它也会被过滤掉；Claude Code 因 `messages.1.output_config` 额外输入而失败（#45411）。[Issue #45411](https://github.com/BerriAI/litellm/issues/45411)
- `/v1/audio/transcriptions` 在 `response_format=text` 时返回 JSON，而不是 `text/plain`（#31457）。[Issue #31457](https://github.com/BerriAI/litellm/issues/31457)
- `is_gpt_reasoning_series_name` 检查的是原始模型路径，而不是规范化名称（#45998）。[Issue #45998](https://github.com/BerriAI/litellm/issues/45998)
- UI 在添加 Model LiteLLM Params 后无法重新编辑（#23998）。[Issue #23998](https://github.com/BerriAI/litellm/issues/23998)
- 即使用户拥有模型访问权限，UI 也会强制为个人密钥选择团队（#27292）。[Issue #27292](https://github.com/BerriAI/litellm/issues/27292)

**修复 PR / 已关闭 issue**
- 分区 SpendLogs 启动 schema 检查修复：[PR #46022](https://github.com/BerriAI/litellm/pull/46022)
- Vertex embedding `billableCharacterCount` 映射：[PR #46008](https://github.com/BerriAI/litellm/pull/46008)
- 路由器默认回退全局泄漏测试修复：[PR #46014](https://github.com/BerriAI/litellm/pull/46014)
- Anthropic `extra_body.thinking` 提升：[PR #45032](https://github.com/BerriAI/litellm/pull/45032)
- 虚拟密钥模型别名编辑：[PR #45412](https://github.com/BerriAI/litellm/pull/45412)
- 路由器调用方回退保留：[PR #45099](https://github.com/BerriAI/litellm/pull/45099)

## 6. 对应用开发者的意义
- **MCP agent 构建者：** 将 OAuth token 刷新和工具目录固定视为生产风险。token 过期后工具可能消失（#33072），固定目录可能会错过破坏性注解变更（#44093）。
- **兼容 Anthropic 的客户端：** 如果你将 Claude Code 或 Anthropic 风格请求路由到 OpenAI 格式提供商，请在上线前验证 `stop_sequences` 和 `tool_result.is_error` 的处理（#33075、#44979）。
- **对支出敏感的部署：** 审计 Gemini 工具使用计费、`no-log` 请求和自定义模型定价。计费不足或跳过支出日志会扭曲租户预算（#45991、#44503、#30081）。
- **路由器运维人员：** 虚拟模型（#46024）和会话亲和性让出（#45868）对可靠性很有前景，但 `/v1/messages` 的 RPM 强制执行仍不一致（#45987）。
- **提供商集成：** RodiumAi（#46023）和 Wingback（#38685）扩展了覆盖范围和运行时安全；Bedrock 自动发现仍然只是一个已关闭的请求（#43568）。
- **UI/开发体验：** 密钥别名编辑（#45412）、Playground IME 处理（#35622）和 AI Hub pill 颜色（#35624）正在进行；模型参数重新编辑 bug（#23998）仍未解决。

---

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth 摘要 — 2026-10-12

## 1. 今日亮点
过去 24 小时内没有新版本发布，但 Studio 开发进展迅速：面向 Apple Silicon 的可选 MLX 图像引擎新增 Z-Image 支持，Wan2.1 视频系列现已被识别，MCP 访问让编码智能体可通过 API 密钥驱动 Studio。在训练/运行时方面，一项 bitsandbytes 优化器同步优化和进行中的 KV 抢占工作旨在解决实际吞吐量和多聊天内存压力。多个长期存在的正确性报告已关闭，但至少一个静默梯度 bug 和一个 MoE-LoRA 检查点布局隐患值得关注。

## 2. 发布与破坏性变更
- 过去 24 小时内无新版本发布；未声明破坏性变更。
- 仅限全新安装的说明：[PR #13380](https://github.com/unslothai/unsloth/pull/13380) 将新的 Linux aarch64 cu130 / Python 3.13 Studio 安装（Grace、GH200、GB200、DGX Spark）迁移到 torch 2.13。现有安装保留当前 torch。

## 3. 新模型与硬件支持
- **Apple Silicon / MLX 图像生成**：[PR #13379](https://github.com/unslothai/unsloth/pull/13379) 为 Studio 增加可选启用的 MLX 图像引擎，首批支持 Z-Image（Turbo 和 base），在用户 GGUF 上以进程内方式运行 mflux，并将 Q4_0/Q4_1/Q8_0 块重新打包为 MLX affine group-32 权重。
- **Wan2.1 视频**：[PR #13376](https://github.com/unslothai/unsloth/pull/13376) 新增 T2V 1.3B/14B 系列以及 I2V 和重新打包/GGUF 变体（city96、QuantStack），这些此前因不受支持而被拒绝。
- **Linux aarch64 CUDA**：[PR #13380](https://github.com/unslothai/unsloth/pull/13380) 将 aarch64 cu130 安装与 x86_64 torch 2.13 路线对齐，后者是 flash-attn 2.8.4 / causal-conv1d 1.7.0 / mamba-ssm 2.3.2.post1 wheel 所依赖的。
- **RAG 嵌入 GPU 卸载**：[PR #13365](https://github.com/unslothai/unsloth/pull/13365)（修复 [#11768](https://github.com/unslothai/unsloth/issues/11768)）提供了一个嵌入设备设置，使 Studio/Desktop 能将文档索引移出 CPU；此前只有 `RAG_EMBED_DEVICE` 可用。
- **Intel GPU**：[#12836](https://github.com/unslothai/unsloth/issues/12836)（已关闭）要求提供有文档记录的 Intel GPU 安装版本锁定信息，因为未添加自动检测。
- **扩散分类**：[#13328](https://github.com/unslothai/unsloth/issues/13328) 报告 `general.architecture=qwen_image21` GGUF 被归类为文本生成，并从图像页面隐藏（已关闭）。

## 4. 性能与优化
- **移除优化器同步**：[PR #13389](https://github.com/unslothai/unsloth/pull/13389) 在非分页优化器步骤中跳过 bitsandbytes 的逐参数 `torch.cuda.synchronize()`（在分页优化器预取变更后已无必要），从而减少训练期间的主机侧停顿。
- **面向并行聊天的 KV 抢占**：[PR #10301](https://github.com/unslothai/unsloth/pull/10301) 和 [PR #10358](https://github.com/unslothai/unsloth/pull/10358) 重构了 Studio 的 `--parallel N --kv-unified` 启动方式，使并行聊天共享一个 KV 缓存，而不是相互饿死/中止；#10358 与 unslothai/llama.cpp#260（`--preempt-ram`）配套，让 llama-server 自行将槽位暂存到主机 RAM 中。
- **UI 滚动性能**：[PR #13255](https://github.com/unslothai/unsloth/pull/13255) 针对包含大段响应、思考块、代码和 LaTeX 的长对话，这些目前会拖慢整个界面。
- **首 token 超时**：[PR #13390](https://github.com/unslothai/unsloth/pull/13390) 使首 token 等待时限可配置（默认 20 分钟，可设为 1–10,080 分钟或不限），面向较慢的本地 GGUF 硬件。
- **可观测性**：[PR #13351](https://github.com/unslothai/unsloth/pull/13351) 增加了可选的详细 Live 监视器，可显示 GPU 负载、温度和功耗（涵盖 [#12589](https://github.com/unslothai/unsloth/issues/12589)、[#9015](https://github.com/unslothai/unsloth/issues/9015)、[#8527](https://github.com/unslothai/unsloth/issues/8527)）。
- **回归观察**：[#10698](https://github.com/unslothai/unsloth/issues/10698)（已关闭）报告每次工具调用后整个上下文都会被重新处理——超过 30k token 后出现数分钟卡顿。[#5752](https://github.com/unslothai/unsloth/issues/5752) 指出 Studio QLoRA 期间显存异常偏高。

## 5. 稳定性与回归
按严重程度排序：

1. **静默训练失败（正确性）** — [#5230](https://github.com/unslothai/unsloth/issues/5230)：Gemma 4（`gemma-4-26B-A4B-it`，可能包括 E2B/E4B/31B）的 fused-loss 内核在使用 last-assistant-only 标签掩码时产生零梯度；训练运行无报错，但模型从不更新。规避方法：`UNSLOTH_RETURN_LOGITS=1`。相关 [#3071](https://github.com/unslothai/unsloth/issues/3071) 报告同一环境变量在训练开始后静默恢复为 `"0"`（已关闭，待确认）。
2. **静默检查点损坏风险** — [#6930](https://github.com/unslothai/unsloth/issues/6930)：通过 PEFT `target_parameters` 进行的 MoE-LoRA 会按专家逐块地将 `lora_B` 打包成 2D 张量；该布局未记录文档，并会静默破坏下游转换器。
3. **Studio 服务器死锁** — [#9008](https://github.com/unslothai/unsloth/issues/9008)：所有线程都阻塞在 `sqlite3.connect()`/`close()`，套接字打开但无法接受请求（已关闭）。
4. **加载架构时崩溃** — [#4037](https://github.com/unslothai/unsloth/issues/4037)：在 Jamba2-Mini int4 上出现 `JambaAttention has no attribute feed_forward`（已关闭，35 条评论）。
5. **GPU/CPU 错误路由** — [#4644](https://github.com/unslothai/unsloth/issues/4644)（7 👍，已关闭）和 [#5941](https://github.com/unslothai/unsloth/issues/5941)（已关闭）：尽管有 NVIDIA GPU，Studio 仍在 CPU 上运行推理。
6. **精度/内存** — [#5344](https://github.com/unslothai/unsloth/issues/5344)（safetensors 以完整 BF16/FP16 加载，而非 4-bit QLoRA，约 15.7 GB 显存，已关闭）；[#11792](https://github.com/unslothai/unsloth/issues/11792)（Qwen-Image-2.1 Q4_K_M 在 M5 Max 48 GB 上 OOM，已关闭）；[#3226](https://github.com/unslothai/unsloth/issues/3226)（Gemma 3N OOM，但 Gemma 3 不会，已关闭）。
7. **UI 崩溃/回归** — [#10288](https://github.com/unslothai/unsloth/issues/10288)（`tapClientLookup` 越界、重复 `toolCallId`），[#12845](https://github.com/unslothai/unsloth/issues/12845)（OPEN：AppImage 窗口无法调整大小，Arch/GNOME/Wayland 上帧率问题），[#10769](https://github.com/unslothai/unsloth/issues/10769)（OPEN：大型代码块导致界面卡顿），[#7877](https://github.com/unslothai/unsloth/issues/7877)（Ollama 模型不再可见；上下文大小膨胀约 50%）。
8. **工具调用循环** — [#7045](https://github.com/unslothai/unsloth/issues/7045)：在 Studio 聊天/API 中，模型在工具调用期间停止生成，直到重新发送消息。

## 6. 对应用开发者意味着什么
- **立即修补 Gemma 4 训练**：如果你使用 fused loss 和 last-assistant-only 掩码微调 Gemma 4，请验证梯度不是静默为零。设置 `UNSLOTH_RETURN_LOGITS=1` 以进行自定义指标验证，并确认运行时它保持设置（[#5230](https://github.com/unslothai/unsloth/issues/5230)、[#3071](https://github.com/unslothai/unsloth/issues/3071)）。
- **审计 MoE-LoRA 导出**：如果你通过 `target_parameters` 训练 Qwen3-30B-A3B 风格的 MoE 适配器，并将检查点交付给第三方转换器，请对照 [#6930](https://github.com/unslothai/unsloth/issues/6930) 验证 `lora_B` 解包——该故障是静默的。
- **多租户 Studio 服务正变得更安全**：KV 抢占堆栈（[#10301](https://github.com/unslothai/unsloth/pull/10301)、[#10358](https://github.com/unslothai/unsloth/pull/10358)）旨在让并行聊天共享一个缓存；预计并发运行智能体时，槽位驱逐导致的中止会更少。
- **智能体现在可以驱动 Studio**：[PR #13118](https://github.com/unslothai/unsloth/pull/13118) 暴露了由 `sk-unsloth-...` 密钥认证的 `/mcp/`，使 Claude Code/Codex 能够操作 Studio；[PR #13372](https://github.com/unslothai/unsloth/pull/13372) 为聊天模型提供浏览器工具，[PR #13353](https://github.com/unslothai/unsloth/pull/13353) 允许项目在现有本地文件夹中运行工具。
- **如果选择启用，RAG 索引默认不再受限于 CPU**：在设置中设置嵌入设备，而不是依赖 `RAG_EMBED_DEVICE`（[PR #13365](https://github.com/unslothai/unsloth/pull/13365)）。
- **慢速硬件调优**：如果本地 GGUF 后端超过默认超时，请开放新的首 token 等待时限（[PR #13390](https://github.com/unslothai/unsloth/pull/13390)）。
- **磁盘布局**：[PR #13373](https://github.com/unslothai/unsloth/pull/13373) 将 uv、其缓存和托管的 Python 保留在 `UNSLOTH_STUDIO_HOME` 内，解决了面向 Windows/受限配置文件磁盘设置的 [#11327](https://github.com/unslothai/unsloth/issues/11327) 的部分问题。
- **界面注意事项**：目前 Studio 在超长且包含大量代码/LaTeX 的对话中表现不佳（[#13255](https://github.com/unslothai/unsloth/pull/13255) 正在进行中，[#10769](https://github.com/unslothai/unsloth/issues/10769)）；在该修复落地前，请避免在桌面 UI 中渲染超长上下文历史记录。

</details>

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*