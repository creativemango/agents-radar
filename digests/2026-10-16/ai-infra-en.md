# AI Infrastructure Digest 2026-10-16

> Generated: 2026-10-16 00:40 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross-Project AI Infrastructure Comparison — 2026-10-16

## 1. Ecosystem Overview
AI infrastructure activity remains extremely high, but the center of gravity has shifted from raw throughput to **correctness, cache safety, and agent/tool reliability**. Serving engines are racing to support DeepSeek-V4.1-Flash, GLM-5.3, Qwen3.x, Kimi K3, Muse Spark Open, and Nemotron 3.5 across GB300, Blackwell SM120, ROCm/MI355X, and Apple/consumer hardware. At the same time, several high-severity issues are silent correctness bugs — stale KV in PD disaggregation, prefix-cache cross-request contamination, stripped tool definitions, and gateway budget/spend bypasses. Stable releases are limited: llama.cpp shipped v0.7.0, Unsloth shipped a beta, LiteLLM shipped prereleases, and vLLM/SGLang/Ollama had no new releases in this window.

## 2. Activity Comparison
*N/R = digest does not disclose 24h totals. Counts are reported "updated in last 24h" where available.*

| Project | Issues updated (24h) | PRs updated (24h) | Release status |
|---|---:|---:|---|
| **vLLM** | 121 | 500 | No new releases; no formal breaking changes |
| **SGLang** | N/R | N/R | No new releases; Python 3.10 removal in progress; no API removals |
| **llama.cpp** | 47 | 149 | **v0.7.0 / b11669**; b11671 Muse Spark Open; b11658 maion-coder; b11662; b11643; b11642 |
| **Ollama** | N/R | N/R | No releases in last 24h |
| **LiteLLM** | N/R | N/R | **v1.107.0-dev.2**, **v1.106.0-rc.4** prereleases; cosign Docker verification |
| **Unsloth** | N/R | N/R | **v0.1.908-beta** |

**Readout:** vLLM and llama.cpp disclose the largest raw update volumes. vLLM is PR-heavy and correctness-focused; llama.cpp is shipping release content. SGLang is dominated by CI/stability and SM120/GLM-5.3 issues. Ollama, LiteLLM, and Unsloth show concentrated activity around agent/tool correctness, gateway operability, and training/model onboarding respectively.

## 3. Model Support Race
**Shipped / landed support:**
- **llama.cpp:** v0.7.0 adds GLM5-Next MTP, K2 Horizon dense/MoVA, Prism, and host-memory MoE expert cache. b11671 adds **Muse Spark Open** text/vision/audio. b11658 adds **maion-coder** plus draft-model support.
- **Unsloth:** v0.1.908-beta adds **NVIDIA Nemotron 3.5 / Bolt (24 GB)**, **Meta Muse Spark Open**, text-diffusion LLMs, DeepSpeed training, and improved prompt caching for coding agents. FLUX.3 Image Klein loading is in PR.
- **Ollama:** MiniCPM-V 4.7 projector support fixed via llama.cpp bump; Q2_0 GGUF support added; Clippy Vision and Clef Cloud added.
- **LiteLLM:** Not a model-kernel project, but provider compatibility advanced for Bedrock Claude web identity, OpenAI `gpt-transcribe`, Anthropic compaction blocks, Bedrock Sonnet 5.5 / Haiku 5.5 token counting, Kimi K3 / DeepSeek framing, and Databricks gpt-5.6 tool/reasoning issues.

**In-flight / not yet released:**
- **vLLM:** Muse Spark Open, Step-5-Preview on ROCm, DeepSeek-V4.1-Flash GB300 low-latency decode, ROCm fused QK-norm + RoPE + gate kernels. Nemotron 3.5 ASR streaming, Phi4Flash, and ViT full CUDA graph are requests/RFCs.
- **SGLang:** SM120 field reports for DeepSeek-V4.1-Flash; GLM-5.3-Flash SM120 tracking; Hopper CuTe DSL KDA verify kernel; AMD AITER/DSv4 GEMM work; Muse Spark Open NIXL KV packing.

**Who is ahead?**
- **Breadth of shipped local model support:** llama.cpp and Unsloth are ahead.
- **Data-center serving enablement on new accelerators:** vLLM and SGLang are ahead, especially for DeepSeek-V4.1-Flash, GLM-5.3, ROCm/MI355X, and GB300/SM120.
- **Provider/API compatibility:** LiteLLM is ahead.
- **Agentic correctness:** Ollama is currently behind due to native `/api/chat` tool-definition issues.

## 4. Performance Frontier
Optimization effort is concentrated in five areas:

**KV cache and prefix caching**
- vLLM: PD disaggregation prefix-cache contamination, KV connector cache validity, model-customized KVCache planning, Mamba block-size regression.
- SGLang: HiCache stale-KV silent corruption, unified memory pool prewarm/compaction, NIXL KV packing.
- llama.cpp: prompt cache now saves only restorable slot states; `--moe-cache-mib` host expert cache.
- Unsloth: KV preemption for concurrent chats, prompt caching for coding agents.

**Batching and decode latency**
- vLLM: DeepSeek-V4.1-Flash GB300 low-latency decode, DSpark speculative decoding, FA2 short-query/long-context attention.
- SGLang: AMD FP8 quant+GEMM fusion for EAGLE low-concurrency decode, PP-spec compare skipping.
- Ollama: MLX engine remains single-request serial; no continuous batching.
- Unsloth: parallel chat serving via llama-server KV parking/preemption.

**Quantization and low-bit formats**
- llama.cpp: Q1_0/Q2_0 numpy support, Hexagon Q1_0 proposal, repacking rework.
- Ollama: Q2_0 GGUF tensor/file-type support.
- Unsloth: 4-bit MoE dequant under compile, FLUX.3 INT8/FP8/GGUF.
- vLLM/SGLang: NVFP4 OOM, W4A16 routed experts, FP8 KV.

**Distributed serving**
- vLLM: TP4 + DSpark, PD disaggregation, MoE exact strides, hybrid GDN prefix-cache hits.
- SGLang: NIXL TP-heterogeneous transfer, PD prefill + HiCache, DCP planner crashes.
- llama.cpp: MTP multi-GPU prompt-processing regression; Vulkan multi-device + CPU-MoE slowdown.
- LiteLLM: Gateway load testing, retry/backoff, WebSocket spend accounting, Rust route parity.

**Kernels**
- vLLM: FA2 optimization, ROCm Triton fused kernels, Mamba2 NaN fixes.
- SGLang: AMD FP8 GEMM fusion, CuTe DSL KDA verify on Hopper, deepstack CUDA-graph handling.
- llama.cpp: Metal BF16 tensor-API flash attention, Vulkan f16 KV striding, Hexagon tensor repacking.
- Unsloth: ROCm flash attention timing probe, MoE dequant outside `torch.compile`, mixed-generation GPU capability detection.

## 5. Layer Positioning

| Layer | Projects | Primary concern | Today’s signal |
|---|---|---|---|
| **Serving engine** | vLLM, SGLang | High-throughput distributed GPU inference, paged attention, PD disaggregation, speculative decoding | Correctness bugs in prefix cache/PD are the top risk; new hardware enablement is active |
| **Local runtime** | llama.cpp, Ollama | Single-node/desktop/edge inference, GGUF, broad hardware | llama.cpp shipping MoE offload/spec decode; Ollama struggling with agent/tool-calling correctness |
| **Gateway** | LiteLLM | Multi-provider routing, auth, budgets, spend logs, retries, MCP | Budget bypass, spend-table inflation, RAM/OOM, and `/v1/messages` compatibility are key risks |
| **Training / fine-tuning** | Unsloth | LoRA/RL/GRPO, model onboarding, Studio UI, AMD/NVIDIA support | Beta support for Nemotron 3.5/Muse Spark; multi-GPU fine-tuning still roadmap, sandboxing not airtight |

**Overlaps:** Ollama depends on llama.cpp/MLX underneath. Unsloth also touches inference/Studio serving. vLLM and SGLang are increasingly used for RL rollout and agent backends, which exposes them to training-loop correctness requirements.

## 6. Trend Signals

1. **Silent correctness is now the dominant infrastructure risk.**  
   vLLM’s PD prefix-cache crosstalk, SGLang’s HiCache stale-KV bug, Ollama’s stripped tool definitions, and LiteLLM’s budget/spend bypasses all fail without clear errors. Agent developers should add output canaries, disable risky prefix caching, and monitor spend tables.

2. **New model support velocity is highest in local runtimes and training stacks.**  
   llama.cpp and Unsloth shipped Muse Spark Open, Nemotron 3.5, GLM5-Next, K2 Horizon, and text-diffusion support. vLLM/SGLang are still enabling DeepSeek-V4.1-Flash, GLM-5.3, and Qwen3.x on new accelerators.

3. **Hardware diversification is accelerating — and so are hardware-specific regressions.**  
   GB300, Blackwell SM120, ROCm MI355X, Strix Halo, Vulkan, Metal, Hexagon, and MIG all appear. SM120 is explicitly “least-battle-tested”; multi-GPU heterogeneous and MIG decisions should not be trusted on pre-fix builds.

4. **Speculative decoding and MoE offload are the main performance levers — with regressions.**  
   vLLM DSpark, llama.cpp LiliCorr/MTP, SGLang EAGLE/DSPARK, and MoE host caching offer large gains, but MTP multi-GPU prompt processing and Vulkan MTP assertions are active regressions.

5. **Gateway operationalization is maturing.**  
   LiteLLM is adding retry/backoff, WebSocket spend accounting, load-test kits, CLI cold-start improvements, and Rust route parity. For production, cost/budget correctness and `/v1/messages` stability remain the watch items.

6. **Agent workloads are stressing every layer.**  
   Tool schema validation costs, streaming parser chunk-boundary bugs, GLM-5.3-Flash degeneration, sandbox escape risks, and empty tool definitions are all present. Prefer `/v1` over Ollama native `/api/chat` for tool calling; validate strict/required/named tool choices in vLLM; and treat Unsloth sandboxing as not yet a security boundary.

7. **Pinning matters more than usual this cycle.**  
   SGLang is removing Python 3.10 for CUDA builds; Unsloth is rebaselining against transformers 5.20.0 and TRL < 0.28; LiteLLM prereleases are cosign-only; llama.cpp’s v0.7.0 changes prompt-cache and MTP behavior. Freeze versions deliberately in CI and production.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-10-16

**Activity snapshot:** No new releases. 121 issues and 500 PRs were updated in the last 24h. Work is concentrated on DeepSeek-V4.1-Flash low-latency paths, ROCm model enablement, attention/kernel optimization, and correctness fixes around prefix caching and KV connectors.

## 1. Today's Highlights
No new vLLM releases landed, but development activity remains high. The most notable in-flight work is the DeepSeek-V4.1-Flash low-latency decode stack for GB300 ([#61868](https://github.com/vllm-project/vllm/pull/61868), [#61869](https://github.com/vllm-project/vllm/pull/61869)), ROCm Step-5-Preview support ([#61837](https://github.com/vllm-project/vllm/pull/61837)), and FA2 attention optimization for short-query/long-context workloads ([#61784](https://github.com/vllm-project/vllm/pull/61784)). The most severe open correctness issue is cross-request context contamination in PD disaggregation with automatic prefix caching ([#61160](https://github.com/vllm-project/vllm/issues/61160)); a related KV-connector cache-validity bug is also open ([#61768](https://github.com/vllm-project/vllm/issues/61768)).

## 2. Releases & Breaking Changes
- No releases in the last 24h.
- No formal breaking-change announcements. Config caveat: `--mamba-block-size` reportedly has no effect after the removal of `mamba_cache_mode="all"` ([#60838](https://github.com/vllm-project/vllm/issues/60838)).

## 3. New Model & Hardware Support
- **Muse Spark Open** support PR open: [#61879](https://github.com/vllm-project/vllm/pull/61879). Depends on a FlashInfer PR.
- **Step-5-Preview on ROCm** PR open: [#61837](https://github.com/vllm-project/vllm/pull/61837). Adds an `amd/` implementation with portable Triton kernels for compressed sparse attention. The NVIDIA side is referenced as limited to SM90/SM103.
- **Nemotron 3.5 ASR Streaming** (`nemotron-3.5-asr-streaming-0.6b`) feature request: [#47455](https://github.com/vllm-project/vllm/issues/47455).
- **Phi4Flash in V1** feature request: [#23957](https://github.com/vllm-project/vllm/issues/23957).
- **ViT Full CUDA Graph** RFC for multimodal models such as Qwen3-VL, Qwen3.5, GLM-V, and Kimi K2.5: [#38175](https://github.com/vllm-project/vllm/issues/38175).
- **ROCm fused QK-norm + RoPE + gate Triton kernel** for Qwen3-Next/Qwen3.5: [#51406](https://github.com/vllm-project/vllm/pull/51406).
- **DeepSeek-V4.1-Flash on GB300** low-latency decode and DSpark draft paths: [#61868](https://github.com/vllm-project/vllm/pull/61868), [#61869](https://github.com/vllm-project/vllm/pull/61869).

## 4. Performance & Optimization
- **DeepSeek-V4.1-Flash GB300**: opt-in low-batch decode paths with TP4 + DSpark speculative decoding ([#61868](https://github.com/vllm-project/vllm/pull/61868)); DSpark draft gets its own decode executor, fused Markov loop, and post-forward CUDA graph ([#61869](https://github.com/vllm-project/vllm/pull/61869)).
- **FA2 short-query attention**: head-packed, KV-split context attention for short queries over long cached contexts on SM8x GPUs ([#61784](https://github.com/vllm-project/vllm/pull/61784)).
- **ROCm/AMD trackers**: Qwen3.8-2.4T-A95B MI355X ([#57149](https://github.com/vllm-project/vllm/issues/57149)), Qwen3.8-Flash-Next MI355X ([#59575](https://github.com/vllm-project/vllm/issues/59575)), GLM 5.3 ([#57406](https://github.com/vllm-project/vllm/issues/57406)), and Kimi K3 ([#50587](https://github.com/vllm-project/vllm/issues/50587)).
- **Compiler/kernel strategy**: RFC to port compiler fusions to manual fusion ([#43224](https://github.com/vllm-project/vllm/issues/43224)); RFC for MonoKernels in `vllm/models` ([#60904](https://github.com/vllm-project/vllm/issues/60904)).
- **KV/cache planning**: RFC for model-customized KVCache planning ([#44276](https://github.com/vllm-project/vllm/issues/44276)).
- **Iteration metrics**: optional GPU timing ([#61690](https://github.com/vllm-project/vllm/pull/61690)), workload snapshots ([#61691](https://github.com/vllm-project/vllm/pull/61691)), and FPM v1 export ([#52061](https://github.com/vllm-project/vllm/pull/52061)).
- **Simulation/benchmarking**: CPU-based CI and hardware-aware performance simulation RFC ([#61406](https://github.com/vllm-project/vllm/issues/61406)); BYOG reproducible evals/benchmarks RFC ([#61698](https://github.com/vllm-project/vllm/issues/61698)).
- **Regression to track**: MoE decode ~15% slower on SM12x since #56876, tied to DeepGEMM contiguous-layout alignment changing from 64 to 128 ([#58624](https://github.com/vllm-project/vllm/issues/58624)).
- **Load-time issue**: ModelOpt Llama-4 checkpoints take 5+ minutes to load from CPU page cache ([#31624](https://github.com/vllm-project/vllm/issues/31624)).

## 5. Stability & Regressions
Ranked by severity.

- **Critical — cross-request KV contamination**: with automatic prefix caching in NIXL prefill/decode disaggregation, responses can intermittently include content from other requests; clean with `--no-enable-prefix-caching` ([#61160](https://github.com/vllm-project/vllm/issues/61160)). **Open, no fix PR in this data.**
- **Critical — silent CUDA IMA**: hybrid GDN + MTP k=3 + async scheduling on RTX 3090 can exit 0 after an illegal memory access; persists through prior fixes ([#53726](https://github.com/vllm-project/vllm/issues/53726)). **Open.**
- **High — shared-memory broadcast timeout**: “No available shared memory broadcast block found in 60 seconds” with 54 comments; indicates hangs or long compilation/quantization work ([#35465](https://github.com/vllm-project/vllm/issues/35465)). **Open.**
- **High — KV connector cache invalidity**: abort before load-failure processing leaves invalid prefix-cache blocks reusable ([#61768](https://github.com/vllm-project/vllm/issues/61768)). **Open.**
- **High — LoRA silent no-op on MLA**: adapters targeting `kv_b_proj` are never applied; decode bypasses via absorption, prefill uses a stale pre-wrap reference ([#48974](https://github.com/vllm-project/vllm/issues/48974)). **Open.**
- **High — LoRA silent no-op for wrapper models**: text PEFT LoRAs can silently no-op when the LM is mounted under `language_model` ([#48019](https://github.com/vllm-project/vllm/issues/48019)). **Open.**
- **Medium-high — tool-calling broken with guidance**: guidance backend rejects structural tool tags with `KeyError: 'triggers'`; strict/required/named `tool_choice` fails ([#55152](https://github.com/vllm-project/vllm/issues/55152)). **Open.**
- **Medium-high — Blackwell OOM**: FlashInfer b12x SM120 MoE workspace allocated inside `profile_run` OOMs on 16 GB Blackwell with NVFP4 Qwen3.6-35B-A3B ([#49476](https://github.com/vllm-project/vllm/issues/49476)). **Open.**
- **Medium — Mamba config regression**: `--mamba-block-size` has no effect after `mamba_cache_mode="all"` removal ([#60838](https://github.com/vllm-project/vllm/issues/60838)). **Open.**
- **Medium — ROCm P/D hangs**: MoRI/MoRIIO READ-mode KV transfer can hang or partially complete on AMD GPU P/D runs ([#45749](https://github.com/vllm-project/vllm/issues/45749)). **Open, stale.**
- **Medium — Ray CPU overhead**: 100% CPU usage on 3 cores per node with Ray distributed pipeline parallel ([#21231](https://github.com/vllm-project/vllm/issues/21231)). **Open.**
- **LoRA/RL RFC**: tracks unresolved LoRA correctness and lifecycle issues for RL rollout ([#48297](https://github.com/vllm-project/vllm/issues/48297)). **Open.**
- **Fixed/closed**: `/v1/embeddings` deadlock with truncated input ([#55427](https://github.com/vllm-project/vllm/issues/55427)); ROCm Kimi K2.5 MXFP4 decode ITL regression ([#45943](https://github.com/vllm-project/vllm/issues/45943)); duplicate hash-routing method causing mypy failures ([#61924](https://github.com/vllm-project/vllm/pull/61924)); TP-sharded routed-expert capture for DP=1 ([#61388](https://github.com/vllm-project/vllm/pull/61388)).
- **Fix PRs pending**: MoE exact strides for `moe_forward` ([#61749](https://github.com/vllm-project/vllm/pull/61749)); Mamba2 chunk scan NaN on sm90 ([#61718](https://github.com/vllm-project/vllm/pull/61718)); hybrid GDN prefix-cache hits under MTP ([#52244](https://github.com/vllm-project/vllm/pull/52244)); ROCm DFlash2 sliding-window prefix attention NaNs ([#53628](https://github.com/vllm-project/vllm/pull/53628)); reload/refit fixes ([#61287](https://github.com/vllm-project/vllm/pull/61287)).

## 6. What This Means for Application Developers
- If you run PD-disaggregated serving with prefix caching, **disable automatic prefix caching** (`--no-enable-prefix-caching`) until [#61160](https://github.com/vllm-project/vllm/issues/61160) is fixed. The reported crosstalk is a correctness and privacy risk.
- Tool-calling apps using the guidance backend should validate strict/required/named `tool_choice`; it is reported broken in [#55152](https://github.com/vllm-project/vllm/issues/55152).
- LoRA users on MLA models or wrapper architectures should verify adapters are actually applied; silent no-op bugs are open ([#48974](https://github.com/vllm-project/vllm/issues/48974), [#48019](https://github.com/vllm-project/vllm/issues/48019)).
- Mamba users should not rely on `--mamba-block-size` until [#60838](https://github.com/vllm-project/vllm/issues/60838) is resolved.
- DeepSeek-V4.1-Flash on GB300 users can expect near-term latency improvements from [#61868](https://github.com/vllm-project/vllm/pull/61868) and [#61869](https://github.com/vllm-project/vllm/pull/61869), but these are opt-in paths and not yet released.
- ROCm teams get new model enablement and kernel fusion work in progress ([#61837](https://github.com/vllm-project/vllm/pull/61837), [#51406](https://github.com/vllm-project/vllm/pull/51406)); track the AMD performance trackers for Qwen3.8, GLM, and Kimi.
- No release to upgrade or downgrade today; prioritize monitoring the critical correctness issues above over routine version bumps.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-10-16

## 1. Today's Highlights

CI stability work dominates the radar: the flaky-test tracker [#42752](https://github.com/sgl-project/sglang/issues/42752) (83 comments) and the now-closed CI Maintenance Mode issue [#21065](https://github.com/sgl-project/sglang/issues/21065) reflect continued churn in `PR Test Base/Extra` runs. On the serving side, the day's most serious signal is a **silent-correctness** bug in PD prefill + HiCache ([#44070](https://github.com/sgl-project/sglang/issues/44070)) where decode can receive stale KV, alongside a cluster of SM120 / GLM-5.3-Flash reports. Performance work landed across AMD FP8 GEMM fusion, NIXL TP-heterogeneous transfer, and Hopper support for the CuTe DSL KDA verify kernel.

## 2. Releases & Breaking Changes

No new releases in the last 24h.

- **Python 3.10 removal in progress** — [#44393](https://github.com/sgl-project/sglang/pull/44393) moves the remaining CUDA `sglang-kernel` wheel paths (`cp310-abi3`) and associated CI/release jobs to Python ≥3.11, following #43021. Non-CUDA hardware paths are explicitly out of scope for this PR. Anyone pinning a 3.10 runtime for CUDA builds should plan a bump.
- **No API/config removals** announced today; the RFC for an opt-in `toktier` tokenizer backend ([#44374](https://github.com/sgl-project/sglang/issues/44374)) is proposal-stage only.

## 3. New Model & Hardware Support

- **SM120 (RTX PRO 6000 Blackwell, PCIe-only)** — Field report for DeepSeek-V4.1-Flash on 8× RTX PRO 6000 with measured prefill/decode and a speculative-decoding A/B ([#40877](https://github.com/sgl-project/sglang/issues/40877)). A separate tracking issue covers required fixes for GLM-5.3-Flash on SM120 TP2 with W4A16 routed experts, FP8 KV, vision, and native MTP ([#37813](https://github.com/sgl-project/sglang/issues/37813)).
- **Hopper (SM90) KDA verify kernel** — [#44612](https://github.com/sgl-project/sglang/pull/44612) extends the fused CuTe DSL KDA (`fused_kda_decode_mtp_dspark`) verify kernel to SM90; previously gated to SM100+ due to paired FP32 instructions.
- **AMD / ROCm** — AITER upgrade readiness tracker remains active ([#21302](https://github.com/sgl-project/sglang/issues/21302)); PR [#44008](https://github.com/sgl-project/sglang/pull/44008) carries tuned DSv4 GEMM rows and mHC `tile_m=16` until upstream AITER ships them. Diffusion multimodal unit tests are being made ROCm-aware ([#44599](https://github.com/sgl-project/sglang/pull/44599)).
- **Tokenizer backend RFC** — `toktier`, an opt-in exact incremental and chunk-parallel CPU tokenizer backend ([#44374](https://github.com/sgl-project/sglang/issues/44374)).
- **Monokernel framework RFC** — in-tree `python/sglang/experimental/monokernels/` plugin model for per-model fused kernels ([#43949](https://github.com/sgl-project/sglang/issues/43949)).

## 4. Performance & Optimization

- **AMD MI355X, Qwen3.5-397B-A17B-FP8**: fuse 1x128 activation quant (~4 µs) into the small-M block-scale GEMM (~9 µs) per dense FP8 projection — targeted at EAGLE low-concurrency decode ([#42229](https://github.com/sgl-project/sglang/pull/42229)).
- **AMD linear-attn**: skip the unused PP-spec compare on the GDN/KDA verify path for Qwen3.5-397B ([#43861](https://github.com/sgl-project/sglang/pull/43861)).
- **NIXL KV packing for TP-heterogeneous transfer** (Muse Spark Open) ([#44615](https://github.com/sgl-project/sglang/pull/44615)).
- **Unified memory pool**: prewarm the compaction move at startup and copy whole page envelopes by page id, removing first-use kernel compile/load and `_assert_async` cost from the serving path ([#44592](https://github.com/sgl-project/sglang/pull/44592)).
- **Diffusion runtime import**: defer full GC collections during ~10k-module import, avoiding repeated full heap traces ([#44606](https://github.com/sgl-project/sglang/pull/44606)).
- **Tool schema validation**: replace pure-Python `Draft202012Validator.check_schema` (~0.7 ms/tool, ~12 ms for an 18-tool agent request on the API event loop) with `jsonschema-rs` ([#43274](https://github.com/sgl-project/sglang/pull/43274)).
- **CUDA graph**: keep deepstack media prefills eager so Qwen3-VL-family graph replays don't reuse a stale `input_deepstack_embeds` ([#44607](https://github.com/sgl-project/sglang/pull/44607)).

## 5. Stability & Regressions

Ranked by severity; fix PRs noted where present.

1. **Silent KV corruption — PD prefill + HiCache** ([#44070](https://github.com/sgl-project/sglang/issues/44070)): with the default `SGLANG_DISAGG_PREFILL_EARLY_SEND_CACHED_PREFIX=1`, decode can receive stale KV for part of a prefix when two requests share a prefix whose KV is only in host memory. Silent wrong results, not a crash. No fix PR referenced.
2. **Whole-engine crash on client disconnect** ([#39216](https://github.com/sgl-project/sglang/issues/39216)): uncaught `asyncio.CancelledError` bypasses `except Exception`; reproducible on 4× RTX 6000D / DeepSeek-V4.1-Flash.
3. **OOM crash of the entire TP group** ([#41076](https://github.com/sgl-project/sglang/issues/41076)): unbounded `SparsePrefillWorkspace` allocation with DeepSeek-V4.1-Flash + DSPARK.
4. **Kimi K3 decode crash** ([#34920](https://github.com/sgl-project/sglang/issues/34920)): PD disagg + `--dcp-size 8` + DSPARK target-verify hits `cumsum(extend_prefix_lens=None)` in `dcp/planner.py`; all TP ranks die.
5. **GLM-5.3-Flash degenerate output with DFLASH speculative decoding** ([#40843](https://github.com/sgl-project/sglang/issues/40843)): severe repetition/loops in reasoning output.
6. **GLM-5.3-Flash `!`-loop under agentic prompts** ([#36669](https://github.com/sgl-project/sglang/issues/36669)): complex system prompt + multiple tools + `tool_choice=auto` degenerates thinking output.
7. **SM120 attention backend matrix** ([#42012](https://github.com/sgl-project/sglang/issues/42012)): fa4 crashes at CUDA-graph capture on hybrid extend reshape; triton is the only working backend on the tested `v0.5.20` image.
8. **NIXL PD abort hang** ([#44369](https://github.com/sgl-project/sglang/issues/44369)): abort can leave all default transfer workers waiting on healthy non-terminal handles.
9. **Qwen3.8-Flash-Next silent semantic corruption** ([#37111](https://github.com/sgl-project/sglang/issues/37111)): QSA + NEXTN decode graph on GB10 TP2.
10. **Synchronous request preprocessing blocks the event loop** ([#30770](https://github.com/sgl-project/sglang/issues/30770)), delaying health endpoints — deterministic CPU-only repro.
11. **Unbounded multimodal audio decode** ([#43708](https://github.com/sgl-project/sglang/issues/43708)).
12. **Minor/test-infra**: `WeightChecker.reset_tensors` filling `MRotaryEmbedding.axis_map` with random values ([#44501](https://github.com/sgl-project/sglang/issues/44501)); profiler filename path-traversal via caller-controlled `output_dir`/`profile_prefix` ([#44609](https://github.com/sgl-project/sglang/pull/44609), fix open).
13. **CI flakiness**: tracker [#42752](https://github.com/sgl-project/sglang/issues/42752); NIXL storage tests sharing fixed `/tmp` dirs ([#44613](https://github.com/sgl-project/sglang/pull/44613), fix open); hicache prefetch timeout budget re-issued per retry ([#44614](https://github.com/sgl-project/sglang/pull/44614), fix open).

Closed today: [#34340](https://github.com/sgl-project/sglang/issues/34340) (SM103 mis-gated as SM100 family, cutedsl TGV BF16 GEMM / trtllm-gen MoE finalize), [#36333](https://github.com/sgl-project/sglang/issues/36333) (zombie request from disconnected streaming client), [#21065](https://github.com/sgl-project/sglang/issues/21065) (CI maintenance mode), [#23494](https://github.com/sgl-project/sglang/issues/23494) (AMD Q2 roadmap, marked inactive), [#41303](https://github.com/sgl-project/sglang/pull/41303) (DeepSeek-V4.1 SWA bounded-replay window), [#44599](https://github.com/sgl-project/sglang/pull/44599) (ROCm-aware diffusion tests).

## 6. What This Means for Application Developers

- **Validate PD + prefix caching carefully.** If you run disaggregated prefill with HiCache and shared system prompts (the normal agent case), [#44070](https://github.com/sgl-project/sglang/issues/44070) means identical prefixes can yield subtly wrong outputs with no error surfaced. Consider `SGLANG_DISAGG_PREFILL_EARLY_SEND_CACHED_PREFIX=0` until fixed, and add output-quality canaries on shared-prefix traffic.
- **Harden against disconnect-driven failures.** [#39216](https://github.com/sgl-project/sglang/issues/39216) is an engine-wide crash on client abort, and [#44369](https://github.com/sgl-project/sglang/issues/44369) can wedge transfer workers. Gateways should retry on connection errors, but expect some fraction of aborts to take a whole replica down — run N+1 capacity and automate restarts.
- **Agentic workloads have two independent hazards.** GLM-5.3-Flash degenerates under multi-tool prompts ([#36669](https://github.com/sgl-project/sglang/issues/36669)) and under DFLASH speculative decoding ([#40843](https://github.com/sgl-project/sglang/issues/40843)); add repetition watchdogs or disable DFLASH for these routes. Separately, tool-schema validation costs ~0.7 ms per tool on the API event loop, so 10–20 tool agents pay 7–14 ms before generation starts ([#43274](https://github.com/sgl-project/sglang/pull/43274) is the fix in review).
- **SM120 deployments are the least-battle-tested path.** Multiple independent reports converge on the same guidance: triton is the reliable attention backend, fa4 breaks CUDA-graph capture, and GLM-5.3-Flash still needs a fix set before production ([#42012](https://github.com/sgl-project/sglang/issues/42012), [#37813](https://github.com/sgl-project/sglang/issues/37813), [#40877](https://github.com/sgl-project/sglang/issues/40877)).
- **Cheap wins are available if you're on AMD/MI355X** — the FP8 quant+GEMM fusion ([#42229](https://github.com/sgl-project/sglang/pull/42229)) targets exactly the low-concurrency EAGLE decode regime that interactive agents run in.

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-10-16

Data snapshot: 149 PRs and 47 issues updated in the last 24h.

## 1. Today's Highlights
- **v0.7.0 lands** with LiliCorr speculative decoding, MTP for GLM5-Next, and a host-memory MoE expert cache via `--moe-cache-mib`, reported up to **~2.2x decode speedup** when experts are offloaded ([v0.7.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.7.0), [b11669](https://github.com/ggml-org/llama.cpp/releases/tag/b11669)).
- **Model support expands** with Muse Spark Open (text/vision/audio latent MoE) ([#30616](https://github.com/ggml-org/llama.cpp/pull/30616)), maion-coder/draft-model support ([#29778](https://github.com/ggml-org/llama.cpp/pull/29778)), plus K2 Horizon dense/MoVA and Prism in v0.7.0.
- **Backend fixes landed** for Vulkan concurrent device init ([#29766](https://github.com/ggml-org/llama.cpp/pull/29766)), AMD Vulkan f16 KV striding ([#27703](https://github.com/ggml-org/llama.cpp/pull/27703)), and Metal BF16 tensor-API flash attention ([#30491](https://github.com/ggml-org/llama.cpp/pull/30491)); active regressions remain around MTP multi-GPU and Metal MoE segfaults.

## 2. Releases & Breaking Changes
- **v0.7.0 / b11669** — version bump and release containing LiliCorr speculative decoding, GLM5-Next MTP, `--moe-cache-mib`, K2 Horizon (dense and MoVA), Prism, and additional model updates ([v0.7.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.7.0), [PR #30603](https://github.com/ggml-org/llama.cpp/pull/30603)).
- **b11671** — adds Muse Spark Open with text, vision, and audio support; latent-space MoE with 8192 residual stream, 256 experts in 4096, top-8 sigmoid routing ([b11671](https://github.com/ggml-org/llama.cpp/releases/tag/b11671), [PR #30616](https://github.com/ggml-org/llama.cpp/pull/30616)).
- **b11658** — adds maion-coder architecture, draft-model support, and removes maion context-shift handling ([b11658](https://github.com/ggml-org/llama.cpp/releases/tag/b11658), [PR #29778](https://github.com/ggml-org/llama.cpp/pull/29778)).
- **b11662** — ggml sync ([b11662](https://github.com/ggml-org/llama.cpp/releases/tag/b11662)).
- **b11643** — fixes embedding-only graphs becoming empty and leaving inputs unallocated ([b11643](https://github.com/ggml-org/llama.cpp/releases/tag/b11643), [PR #30522](https://github.com/ggml-org/llama.cpp/pull/30522)).
- **b11642** — server now saves only restorable slot states to the prompt cache; relevant to rerank/embedding/completion slot behavior ([b11642](https://github.com/ggml-org/llama.cpp/releases/tag/b11642), [PR #30498](https://github.com/ggml-org/llama.cpp/pull/30498)).
- No explicit API-breaking changes were called out. Review prompt-cache and embedding-graph behavior if you depend on those paths.

## 3. New Model & Hardware Support
- **Muse Spark Open** — text/vision/audio, latent-space MoE with residual 8192 and 256 experts in 4096, top-8 sigmoid routing ([PR #30616](https://github.com/ggml-org/llama.cpp/pull/30616)).
- **maion-coder** — new architecture plus draft-model wiring; context-shift function removed ([PR #29778](https://github.com/ggml-org/llama.cpp/pull/29778)).
- **K2 Horizon / Prism / GLM5-Next MTP** — included in v0.7.0 model batch ([v0.7.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.7.0)).
- **Metal** — adds BF16 variant of the tensor API flash-attention kernel, moved to `kernels/fa_tensor.metal` ([PR #30491](https://github.com/ggml-org/llama.cpp/pull/30491)).
- **Hexagon** — tensor repacking rework with tiled/flat quantized and FP weight support ([PR #30635](https://github.com/ggml-org/llama.cpp/pull/30635)); Q1_0 support proposed ([PR #30122](https://github.com/ggml-org/llama.cpp/pull/30122)).
- **Vulkan/AMD** — strided f16 KV copy done once so reads spread across memory channels ([PR #27703](https://github.com/ggml-org/llama.cpp/pull/27703)).
- **mtmd/audio** — adds `mtmd_audio_stream` and voxtral-realtime support ([PR #30629](https://github.com/ggml-org/llama.cpp/pull/30629)).
- **Quantization tooling** — Q1_0/Q2_0 numpy de/quantization support in gguf-py ([PR #30630](https://github.com/ggml-org/llama.cpp/pull/30630), [issue #30626](https://github.com/ggml-org/llama.cpp/issues/30626)).
- **Conversion** — text-only checkpoints supported in pplx-decider ([PR #30575](https://github.com/ggml-org/llama.cpp/pull/30575)); Granite Switch block-count fix ([PR #30621](https://github.com/ggml-org/llama.cpp/pull/30621), [issue #30617](https://github.com/ggml-org/llama.cpp/issues/30617)); Granite speech `conv_norm` mapping regression fixed ([PR #30628](https://github.com/ggml-org/llama.cpp/pull/30628)).

## 4. Performance & Optimization
- **MoE host cache** — `--moe-cache-mib` in v0.7.0 gives up to **~2.2x decode** when experts are offloaded ([v0.7.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.7.0)).
- **Speculative decoding** — LiliCorr and GLM5-Next MTP added in v0.7.0 ([v0.7.0](https://github.com/ggml-org/llama.cpp/releases/tag/v0.7.0)).
- **Vulkan** — concurrent first-device init fixed; tests now load models in parallel to exercise it ([PR #29766](https://github.com/ggml-org/llama.cpp/pull/29766)).
- **Vulkan/AMD** — f16 KV strided copy reduces memory-channel hotspots ([PR #27703](https://github.com/ggml-org/llama.cpp/pull/27703)).
- **Metal** — BF16 tensor-API flash attention kernel improves BF16 path coverage ([PR #30491](https://github.com/ggml-org/llama.cpp/pull/30491)).
- **Server prompt cache** — only restorable slot states are saved, reducing cache pollution from rerank/embedding tasks ([PR #30498](https://github.com/ggml-org/llama.cpp/pull/30498)).
- **`--moe-cache-topk`** — closed PR proposes capping active experts on host-cached MoE layers ([PR #30593](https://github.com/ggml-org/llama.cpp/pull/30593)).
- **Vulkan FA dequant** — PR checks combined K+V f16 scratch size against device limits ([PR #30487](https://github.com/ggml-org/llama.cpp/pull/30487)).
- **Regression watch** — draft-MTP roughly halves prompt processing on multi-GPU layer split ([issue #27428](https://github.com/ggml-org/llama.cpp/issues/27428)); Vulkan multi-device + CPU-MoE prompt processing slower after #29741 ([issue #30547](https://github.com/ggml-org/llama.cpp/issues/30547)); qwen35 on RTX 5090 at ~76% roofline on native Linux ([issue #28196](https://github.com/ggml-org/llama.cpp/issues/28196)).

## 5. Stability & Regressions
Ranked by severity:

- **Critical — Metal segfault**: Nemotron 3.5 Bolt 74B-A9B crashes in `ggml_metal_op_mul_mat_id` on Metal ([issue #30610](https://github.com/ggml-org/llama.cpp/issues/30610)).
- **Critical — llama-server VRAM DoS**: server can exhaust VRAM under certain requests ([issue #30577](https://github.com/ggml-org/llama.cpp/issues/30577)).
- **Critical — memory DoS**: client-set `X-Conversation-Id` disables disconnect cancellation and accumulates uncapped resumable-stream sessions ([issue #29498](https://github.com/ggml-org/llama.cpp/issues/29498)).
- **High — MTP prompt-processing regression**: draft-MTP halves PP on multi-GPU layer split; single GPU fine ([issue #27428](https://github.com/ggml-org/llama.cpp/issues/27428)).
- **High — Vulkan slow prompt processing**: multi-device + `cpu-moe` MoE regression after #29741 ([issue #30547](https://github.com/ggml-org/llama.cpp/issues/30547)).
- **High — assertion in `launch_fattn`**: HIP/ROCm on RX6950XT ([issue #27674](https://github.com/ggml-org/llama.cpp/issues/27674)).
- **High — Vulkan MTP assertion**: `GGML_ASSERT(neq0 == HSK)` during speculative draft decoding with tensor split ([issue #29418](https://github.com/ggml-org/llama.cpp/issues/29418)).
- **High — CUDA misaligned address**: `ggml_cuda_op_unary<op_sigmoid>` with long prompts on RTX 5090 Laptop ([issue #28877](https://github.com/ggml-org/llama.cpp/issues/28877)).
- **High — Vulkan generation crawl**: performance regression since b11594 ([issue #30552](https://github.com/ggml-org/llama.cpp/issues/30552)).
- **Fixed/closed today**: Vulkan MiniMax-M2 prompt collapse at long context/ubatch >128 ([issue #30364](https://github.com/ggml-org/llama.cpp/issues/30364)); Granite Switch conversion block-count bug ([issue #30617](https://github.com/ggml-org/llama.cpp/issues/30617), fix [PR #30621](https://github.com/ggml-org/llama.cpp/pull/30621)); slot KV restore no-op ([issue #26676](https://github.com/ggml-org/llama.cpp/issues/26676)); rerank prompt-cache memory growth ([issue #26293](https://github.com/ggml-org/llama.cpp/issues/26293)).
- **Fix PRs in flight**: SYCL IQ3 crash fix ([PR #30604](https://github.com/ggml-org/llama.cpp/pull/30604)); combined draft-MTP + external draft init crash ([PR #27897](https://github.com/ggml-org/llama.cpp/pull/27897), related [issue #27839](https://github.com/ggml-org/llama.cpp/issues/27839)); Vulkan FA dequant combined K+V scratch check ([PR #30487](https://github.com/ggml-org/llama.cpp/pull/30487)).

## 6. What This Means for Application Developers
- **Upgrade to v0.7.0 for MoE offload and speculative decoding**, but validate MTP on multi-GPU before production because of the multi-GPU prompt-processing regression ([issue #27428](https://github.com/ggml-org/llama.cpp/issues/27428)).
- **Server APIs are expanding**: realtime transcription via `/v1/realtime` WebSocket is proposed ([PR #30636](https://github.com/ggml-org/llama.cpp/pull/30636)); embeddings `dimensions` support is in PR ([PR #24898](https://github.com/ggml-org/llama.cpp/pull/24898), [issue #25210](https://github.com/ggml-org/llama.cpp/issues/25210)); HF model discovery/download UI is proposed ([PR #29584](https://github.com/ggml-org/llama.cpp/pull/29584)).
- **Prompt cache behavior changed** — only restorable slot states are saved, so rerank/embedding workloads should no longer pollute completion cache ([PR #30498](https://github.com/ggml-org/llama.cpp/pull/30498)).
- **New model integrations need template checks** — K2 Horizon can leave final text/tool calls in `reasoning_content` at low/medium effort ([issue #30246](https://github.com/ggml-org/llama.cpp/issues/30246)); Qwen3-Coder reasoning/content separator fix is open ([PR #30557](https://github.com/ggml-org/llama.cpp/pull/30557)).
- **Backend operators should pin/test carefully**: Metal has a MoE segfault ([issue #30610](https://github.com/ggml-org/llama.cpp/issues/30610)); Vulkan has MTP/tensor-split and CPU-MoE regressions ([issue #29418](https://github.com/ggml-org/llama.cpp/issues/29418), [issue #30547](https://github.com/ggml-org/llama.cpp/issues/30547)); CUDA sm_120 has long-prompt sigmoid issues ([issue #28877](https://github.com/ggml-org/llama.cpp/issues/28877)).
- **Untrusted-client deployments should mitigate DoS risks**: VRAM exhaustion ([issue #30577](https://github.com/ggml-org/llama.cpp/issues/30577)) and resumable-stream memory growth via `X-Conversation-Id` ([issue #29498](https://github.com/ggml-org/llama.cpp/issues/29498)).
- **Quantization/tooling gains**: Q1_0/Q2_0 numpy support improves gguf-py workflows ([PR #30630](https://github.com/ggml-org/llama.cpp/pull/30630)); Hexagon Q1_0 and repacking work continue ([PR #30122](https://github.com/ggml-org/llama.cpp/pull/30122), [PR #30635](https://github.com/ggml-org/llama.cpp/pull/30635)).

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-10-16

## 1. Today's Highlights

No new releases landed in the last 24h, but the issue tracker is dominated by **agent/tool-calling correctness problems** and **model-pull regressions**: the native `POST /api/chat` path is reportedly stripping tool definitions before they reach the model, while multiple users on 0.40.x cannot pull models through SOCKS proxies or hit repeated redirect/retry failures. On the positive side, upstream llama.cpp integration work advanced — MiniCPM-V 4.7 projector support and Q2_0 quantization both have fix PRs, and GGUF v4 parsing improvements (64KB DMA alignment) are proposed.

---

## 2. Releases & Breaking Changes

No releases published in the last 24h. Context from in-flight PRs:

- **llama.cpp bumped to b11669** ([#19032](https://github.com/ollama/ollama/pull/19032), closed) — underlying runtime move; note that b11669 is what enables Q2_0 GGUF support.
- **Q2_0 GGUF tensor/file-type support** ([#18967](https://github.com/ollama/ollama/pull/18967), closed) — previously `ollama create`/model load failed with `unsupported tensor "token_embd.weight" size overflows`; adds tensor type 42 / file type 41 (64 weights, 18 bytes per block). Existing Q2_0 GGUFs imported on older builds will need the new runtime.
- No API/config deprecations or migration steps were announced. ([#8018](https://github.com/ollama/ollama/pull/8018) — changing `/delete` to POST — is an old, closed PR, not an active change.)

---

## 3. New Model & Hardware Support

- **MiniCPM-V 4.7 (`openbmb/minicpm-v4.7:35b-a3b`)** — load failure `unknown projector type: minicpmv4_7` ([#18983](https://github.com/ollama/ollama/issues/18983), closed) fixed by updating the pinned llama.cpp from b11521 → b11551 ([#19024](https://github.com/ollama/ollama/pull/19024)).
- **Q2_0 quantization** — GGUF reader now understands Q2_0 ([#18967](https://github.com/ollama/ollama/pull/18967)); relevant for extreme-low-bit model imports (e.g. Prism ML releases).
- **GGUF v4 spec** — proposed 64KB (65536) GPU DMA page alignment for zero-copy tensor mapping plus O(1) token-span seek ([#19037](https://github.com/ollama/ollama/pull/19037)).
- **Vision models** — Clippy Vision added ([#19033](https://github.com/ollama/ollama/pull/19033)).
- **Cloud** — Clef is now available on Ollama Cloud ([#19036](https://github.com/ollama/ollama/pull/19036)).
- **Backend note** — no new CUDA/ROCm/Metal backend support landed; several ROCm/CUDA issues remain open (see below).

---

## 4. Performance & Optimization

- **CPU-thread/cgroup mismatch** — `n_threads` is derived from host core count, ignoring cgroup v2 `cpu.max` and `cpuset`. In CPU-limited containers this causes spin-wait convoying against CFS throttling, reported as **~45x throughput collapse**. ([#17916](https://github.com/ollama/ollama/issues/17916))
- **MLX engine is single-request serial** — requests are processed strictly FIFO regardless of `OLLAMA_NUM_PARALLEL`, with no continuous batching. One issue closed ([#17666](https://github.com/ollama/ollama/issues/17666)), the feature request for batching/parallel decode remains open ([#17280](https://github.com/ollama/ollama/issues/17280), 👍 4).
- **Regression report: text-classifier models slower on 0.40.4 vs 0.40.3** (nimble, clef-flash, tev1) — unconfirmed, needs benchmarks. ([#19002](https://github.com/ollama/ollama/issues/19002))
- **GGUF v4 seek/alignment** — proposed 64KB DMA alignment and O(1) bulk token-span discard for faster model inspection/model ops. ([#19037](https://github.com/ollama/ollama/pull/19037))

---

## 5. Stability & Regressions (ranked by severity)

**Critical — correctness/data integrity**
- **Cloud generation degeneration: `glm-5.3-flash:cloud` failing 5–8% of requests**, with reports of zeroed host-side data areas. Long-running, 22 comments, no fix linked. ([#18980](https://github.com/ollama/ollama/issues/18980))
- **Native `/api/chat` renders EMPTY tool definitions install-wide** — models echo `{"name": ""}`/`{{ <nil> <nil> [] {}}}` and never emit `tool_calls`; `/v1` renders tools correctly. High impact for agents. ([#19040](https://github.com/ollama/ollama/issues/19040))
- **Go API tool-call arguments silently lose numeric precision** (normal + streaming paths) — fix PR exists. ([#18942](https://github.com/ollama/ollama/issues/18942), fix [#18941](https://github.com/ollama/ollama/pull/18941))

**High — crashes / hangs**
- **CUDA illegal memory access (MUL_MAT) on RTX 5090** with Cohere MoE on Windows, followed by llama-server crash (`0xc0000409`). ([#18642](https://github.com/ollama/ollama/issues/18642))
- **ROCm: `gemma4:26b-a4b`** with experts offloaded to CPU aborts in `hipGraphInstantiate` (OOM), then segfaults on every load; workaround `GGML_CUDA_DISABLE_GRAPHS=1`. ([#19015](https://github.com/ollama/ollama/issues/19015))
- **A100 PCIe bus loss (Xid79) during Qwen3.5 inference** on 0.34.4/0.35.0, with node-wide reboot recovery. ([#19006](https://github.com/ollama/ollama/issues/19006))
- **Parser panic on lone delimiter tool-call value** (`value[8:0]` slice out of range) — fix PR open. ([#17492](https://github.com/ollama/ollama/pull/17492))
- **Tool-call opening tags lost across streaming chunk boundaries** (DeepSeek3, Cogito, LFM2 parsers) — fix PR with 219 passing tests. ([#19028](https://github.com/ollama/ollama/pull/19028))
- **`gemma4` with `think: false` returns empty reply after a tool response** — empty thought block never closed. ([#18861](https://github.com/ollama/ollama/issues/18861))

**Medium — pull/transfer and startup**
- **`ollama pull` broken with SOCKS proxy since 0.40.4** (works on 0.34.4) — closed. ([#19035](https://github.com/ollama/ollama/issues/19035))
- **Pull failures: `redirect target not allowed` / DNS resolution of R2 endpoint** ([#18716](https://github.com/ollama/ollama/issues/18716)) and **`max retries exceeded`** ([#18842](https://github.com/ollama/ollama/issues/18842)).
- **Disk-full during blob download falsely reported as success** — transfer layer ignored write errors; fix PR open. ([#18813](https://github.com/ollama/ollama/pull/18813))
- **Regression: `timed out waiting for llama-server to start`** with virtio-gpu/drm native context; 0.32.9 works, 0.32.10+ breaks. ([#18123](https://github.com/ollama/ollama/issues/18123))

**Low — desktop / API edge cases**
- **Windows tray app spawns `ollama serve` ~once per second** forever when port 11434 is already occupied. ([#19013](https://github.com/ollama/ollama/issues/19013))
- **Desktop app memory leak** — context-menu label C strings allocated and never freed. ([#19029](https://github.com/ollama/ollama/issues/19029))
- **Cloud `mistral-large-4` rejects `role: "developer"` with HTTP 400** at `/v1/responses`, blocking Codex-style clients. ([#19021](https://github.com/ollama/ollama/issues/19021))
- **`bge-m3` embedding returns HTTP 500** `json: unsupported value: NaN` on certain UTF-8 markdown files. ([#15582](https://github.com/ollama/ollama/issues/15582))
- **Dependency advisory (SCA): seroval `fromJSON()` Promise resolver type confusion** — patched in ≥1.5.3. ([#19034](https://github.com/ollama/ollama/issues/19034))
- **Claude Desktop toggle missing on Windows.** ([#18164](https://github.com/ollama/ollama/issues/18164))

---

## 6. What This Means for Application Developers

- **Prefer `/v1` over native `/api/chat` for tool-calling agents right now.** The native route reportedly empties tool definitions, while the OpenAI-compatible route renders them correctly ([#19040](https://github.com/ollama/ollama/issues/19040)). Validate tool schemas end-to-end before shipping.
- **Beware silent numeric truncation in Go clients** when round-tripping tool-call arguments — apply the pending fix or add your own lossless decoding ([#18942](https://github.com/ollama/ollama/issues/18942)).
- **Streaming parsers are being hardened** for chunk-boundary tag splits and lone-delimiter panics; if you run DeepSeek3, Cogito, LFM2, or FunctionGemma models, pin to a build containing these fixes once merged ([#19028](https://github.com/ollama/ollama/pull/19028), [#17492](https://github.com/ollama/ollama/pull/17492)).
- **Responses-API compatibility is improving**: foreign provider compaction items are now tolerated in history ([#19003](https://github.com/ollama/ollama/pull/19003)), and stale `reasoning.effort` is ignored for non-thinking models ([#19010](https://github.com/ollama/ollama/pull/19010)) — useful if you switch models mid-session.
- **Containerized deployments: set `OLLAMA_NUM_THREADS` explicitly.** The default ignores cgroup quotas, which can cost up to ~45x throughput in CPU-limited pods ([#17916](https://github.com/ollama/ollama/issues/17916)).
- **On Apple Silicon, don't assume concurrency.** The MLX runner serializes requests regardless of `OLLAMA_NUM_PARALLEL`; build your queueing/backpressure accordingly ([#17280](https://github.com/ollama/ollama/issues/17280)).
- **Watch cloud reliability** for `glm-5.3-flash:cloud` (5–8% failure rate reported) and `mistral-large-4` developer-role rejection if you route through Ollama Cloud ([#18980](https://github.com/ollama/ollama/issues/18980), [#19021](https://github.com/ollama/ollama/issues/19021)).
- **Low-bit and multimodal imports are expanding** — Q2_0 GGUF support ([#18967](https://github.com/ollama/ollama/pull/18967)) and MiniCPM-V 4.7 ([#19024](https://github.com/ollama/ollama/pull/19024)) both require a runtime update; stage upgrades before relying on them in production.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-10-16
Source: [BerriAI/litellm](https://github.com/BerriAI/litellm)

## 1. Today's Highlights
LiteLLM’s only new releases are prereleases `v1.107.0-dev.2` and `v1.106.0-rc.4`, both focused on cosign Docker image signature verification rather than feature changes. The most serious open reports remain budget-enforcement bypass in v1.82.3 ([#26672](https://github.com/BerriAI/litellm/issues/26672)), sustained RAM growth/OOM on Kubernetes ([#27954](https://github.com/BerriAI/litellm/issues/27954)), and unauthenticated clients inflating spend tables via failed auth attempts ([#42689](https://github.com/BerriAI/litellm/issues/42689)). In-flight PR work is concentrated on Rust/native route parity with Python ([#46764](https://github.com/BerriAI/litellm/pull/46764), [#46788](https://github.com/BerriAI/litellm/pull/46788), [#46787](https://github.com/BerriAI/litellm/pull/46787)), retry/backoff correctness ([#46730](https://github.com/BerriAI/litellm/pull/46730)), WebSocket spend accounting ([#46768](https://github.com/BerriAI/litellm/pull/46768)), and MCP cross-app access via ID-JAG/JWT-bearer ([#46748](https://github.com/BerriAI/litellm/pull/46748), [#46747](https://github.com/BerriAI/litellm/pull/46747), [#46745](https://github.com/BerriAI/litellm/pull/46745)).

## 2. Releases & Breaking Changes
- `v1.107.0-dev.2` — dev prerelease; release note only covers Docker cosign verification. [Release](https://github.com/BerriAI/litellm/releases/tag/v1.107.0-dev.2)
- `v1.106.0-rc.4` — RC prerelease; same cosign verification note. [Release](https://github.com/BerriAI/litellm/releases/tag/v1.106.0-rc.4)
- No breaking API/config changes are described in the release notes. Operational note: all LiteLLM Docker images are signed with cosign; verify using the key from [commit `0112e53`](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0).
- Watch upcoming behavior changes, not yet released: retries will honor `Retry-After` and use exponential backoff+jitter while filtering transient errors ([#46730](https://github.com/BerriAI/litellm/pull/46730)); token counting adds `litellm_count_source` / `litellm_count_fallback_model` and configurable framing ([#46777](https://github.com/BerriAI/litellm/pull/46777)); Rust `/v1/messages` and `/v1/responses` body-param filtering moves toward Python rules ([#46764](https://github.com/BerriAI/litellm/pull/46764), [#46788](https://github.com/BerriAI/litellm/pull/46788), [#46787](https://github.com/BerriAI/litellm/pull/46787)).

## 3. New Model & Hardware Support
- No new CUDA/ROCm/Metal/CPU backends, accelerators, or quantization formats are present in this window.
- Bedrock Claude Platform: PR [#46670](https://github.com/BerriAI/litellm/pull/46670) grants `sts:GetWebIdentityToken` and `sts:TagGetWebIdentityToken` in the web identity session policy, fixing 403s for OIDC auth.
- OpenAI audio: PR [#44021](https://github.com/BerriAI/litellm/pull/44021) routes `gpt-transcribe` to the GPT audio transcription config; fixes missing `response_format` 400s and health-check failures.
- Anthropic: PR [#46754](https://github.com/BerriAI/litellm/pull/46754) round-trips Anthropic compaction blocks as compaction items on `/v1/responses`, preventing repeated full-history summarization.
- Token counting: PR [#46777](https://github.com/BerriAI/litellm/pull/46777) adds configurable `token_count_framing` and `token_count_fallback_model`, plus count-source reporting; targets Bedrock Sonnet 5.5 / Haiku 5.5 404s and Kimi K3 / DeepSeek estimate framing.
- Router context: PR [#46197](https://github.com/BerriAI/litellm/pull/46197) lets an exact `base_model` widen a deployment’s context window when the cost map agrees.
- Model/backend bugs still open: Databricks gpt-5.6 family tool + `reasoning_effort` ([#43708](https://github.com/BerriAI/litellm/issues/43708)); `openai/`-prefixed self-hosted models routed to Responses API on `/v1/messages`, breaking multimodal ([#40780](https://github.com/BerriAI/litellm/issues/40780)). Closed: Moonshot/Kimi K2.6 reasoning-content tool-call issue ([#26156](https://github.com/BerriAI/litellm/issues/26156)); OCI Grok/coding-agent tool schema issue ([#31449](https://github.com/BerriAI/litellm/issues/31449)).

## 4. Performance & Optimization
- CLI cold start: PR [#46803](https://github.com/BerriAI/litellm/pull/46803) starts `lite`/`litellm-proxy` without importing the full LiteLLM SDK; avoids ~3,000 modules and 1.5–4s on a 1 CPU VM, with a reported customer case up to 20s per command.
- Gateway load testing: PR [#46806](https://github.com/BerriAI/litellm/pull/46806) adds a load-test kit, Docker build knobs, and `TCP_NODELAY`; `FAST=1` skips LTO for ~5 min builds instead of ~13 min.
- Per-request perf accounting: PR [#46798](https://github.com/BerriAI/litellm/pull/46798) makes `postgres_calls_per_request` and `redis_calls_per_request` per-request rather than whole-server totals; PR [#46774](https://github.com/BerriAI/litellm/pull/46774) exports perf metrics/limits as JUnit properties.
- Retry behavior: PR [#46730](https://github.com/BerriAI/litellm/pull/46730) replaces near-instant retries (<0.2ms) with `Retry-After`-aware exponential backoff+jitter and filters unrecoverable errors like 401s.
- Caching: issue [#28427](https://github.com/BerriAI/litellm/issues/28427) closed — prompt-caching router affinity TTL was hardcoded to 5 minutes and broke 1-hour ephemeral cache routing.
- Memory pressure: issue [#27954](https://github.com/BerriAI/litellm/issues/27954) remains open — LiteLLM pods reach RAM limits and crash over time in Kubernetes; monitor until a fix lands.
- WebSocket spend: PR [#46768](https://github.com/BerriAI/litellm/pull/46768) writes one spend-log row per native Responses WebSocket turn instead of one row at socket close, avoiding lost spend on pod restart.

## 5. Stability & Regressions
Ranked by severity.

**Critical**
- [#27954](https://github.com/BerriAI/litellm/issues/27954) open — heavy RAM growth leads to Kubernetes pod OOM/crashes; 8 👍. No fix PR referenced.
- [#26672](https://github.com/BerriAI/litellm/issues/26672) open — budget enforcement bypassed in v1.82.3 for key/user `max_budget` despite spend exceeding limits.
- [#42689](https://github.com/BerriAI/litellm/issues/42689) open — every failed auth with an unknown key writes its own spend rows, so an unauthenticated client can inflate usage tables.
- [#26399](https://github.com/BerriAI/litellm/issues/26399) closed — Bedrock passthrough routes bypassed `key.models` / `user.models` allowlists.

**High**
- [#38358](https://github.com/BerriAI/litellm/issues/38358) open — `litellm_settings.request_timeout` never fires when upstream accepts TCP but sends no first byte.
- [#32357](https://github.com/BerriAI/litellm/issues/32357) open — `/v1/messages` adapter mis-encodes reasoning models: `thinking_delta` inside text block and duplicate `message_start` cause empty content in Anthropic SDK / Claude Code.
- [#31909](https://github.com/BerriAI/litellm/issues/31909) open — `mcp_semantic_tool_filter` strips all tools for standard nested OpenAI `tools` format.
- [#46403](https://github.com/BerriAI/litellm/issues/46403) closed — SpendLogs rows silently dropped when provider reuses response IDs (Ollama `chatcmpl-<0..999>`); `request_id` primary-key collision.
- [#46407](https://github.com/BerriAI/litellm/issues/46407) open — `gpt-image` cost ignores EU data-residency uplift and deployment-level `output_cost_per_image_token`.
- [#40780](https://github.com/BerriAI/litellm/issues/40780) open — `openai/`-prefixed self-hosted models route `/v1/messages` to Responses API, silently losing multimodal support.
- [#43708](https://github.com/BerriAI/litellm/issues/43708) open — function tools fail with `reasoning_effort` error for Databricks-hosted gpt-5.6 family on `/chat/completions`.

**Medium / startup and MCP**
- [#30425](https://github.com/BerriAI/litellm/issues/30425) open — `Failed to import Prisma client` on proxy startup after DB configuration.
- [#33431](https://github.com/BerriAI/litellm/issues/33431) open — editing config MCP server identity fields silently orphans existing grants.
- [#33492](https://github.com/BerriAI/litellm/issues/33492) open — OpenAPI-spec MCP cannot forward `extra_headers` to upstream backend.
- [#33671](https://github.com/BerriAI/litellm/issues/33671) open — standalone Bedrock Guardrails integration never sends AWS `trace` param, losing assessment detail.
- [#46764](https://github.com/BerriAI/litellm/pull/46764) high-risk PR in review — Rust `/v1/messages` body-param parity; [#46788](https://github.com/BerriAI/litellm/pull/46788), [#46787](https://github.com/BerriAI/litellm/pull/46787) lower-risk Rust bridge parity work.

**Closed correctness fixes in the window**
- [#25429](https://github.com/BerriAI/litellm/issues/25429) `chatgpt/gpt-5.4` empty final Responses output / completion bridge failure.
- [#14004](https://github.com/BerriAI/litellm/issues/14004) Free/no-cost self-hosted models were blocked when budget exceeded.
- [#31447](https://github.com/BerriAI/litellm/issues/31447) `team_member_budget` replaced entire team metadata object.
- [#27038](https://github.com/BerriAI/litellm/issues/27038) `disable_end_user_cost_tracking` did not gate SpendLogs / DailyEndUserSpend writes.
- [#31587](https://github.com/BerriAI/litellm/issues/31587) `/v1/skills` returned 500 when `ANTHROPIC_API_KEY` was missing/invalid.
- [#26156](https://github.com/BerriAI/litellm/issues/26156) Moonshot/Kimi K2.6 multi-turn tool calling lost `reasoning_content`.
- [#27184](https://github.com/BerriAI/litellm/issues/27184) Bedrock Claude 4.6 `tool_choice.type` required error.
- [#37175](https://github.com/BerriAI/litellm/issues/37175) Anthropic `/v1/messages` → Responses adapter dropped image `detail` and mislabeled `thinking`.
- [#31449](https://github.com/BerriAI/litellm/issues/31449) OCI provider tool requests from coding agents failed.
- [#28444](https://github.com/BerriAI/litellm/issues/28444) Post API hook not enabled for passthrough endpoints.
- [#24628](https://github.com/BerriAI/litellm/issues/24628) S3 filename too long.
- [#25456](https://github.com/BerriAI/litellm/issues/25456) Chat completions → Responses bridge did not handle `file_id` with URL.
- [#25191](https://github.com/BerriAI/litellm/issues/25191) `websearch_interception` not triggered for Bedrock via `/chat/completions`.
- [#45411](https://github.com/BerriAI/litellm/issues/45411) Claude Code per-turn-control filtered for Bedrock though Bedrock Invoke supports it.

## 6. What This Means for Application Developers
- Budget/cost safety: If you rely on budgets, free-model routing, or end-user spend tracking, validate against [#26672](https://github.com/BerriAI/litellm/issues/26672), [#14004](https://github.com/BerriAI/litellm/issues/14004), [#42689](https://github.com/BerriAI/litellm/issues/42689), [#46403](https://github.com/BerriAI/litellm/issues/46403), and [#46407](https://github.com/BerriAI/litellm/issues/46407). Add alerting for SpendLogs growth and request-ID collisions.
- Claude Code / Anthropic SDK: Treat `/v1/messages` as still stabilizing. Track [#32357](https://github.com/BerriAI/litellm/issues/32357), [#40780](https://github.com/BerriAI/litellm/issues/40780), [#37175](https://github.com/BerriAI/litellm/issues/37175), and the Rust parity PRs [#46764](https://github.com/BerriAI/litellm/pull/46764), [#46788](https://github.com/BerriAI/litellm/pull/46788). Avoid `openai/`-prefixed self-hosted multimodal models through `/v1/messages` until #40780 is fixed.
- MCP/agents: ID-JAG/JWT-bearer and SSO assertion capture are coming ([#46748](https://github.com/BerriAI/litellm/pull/46748), [#46747](https://github.com/BerriAI/litellm/pull/46747), [#46745](https://github.com/BerriAI/litellm/pull/46745)), but validate tool filtering ([#31909](https://github.com/BerriAI/litellm/issues/31909)) and MCP identity/header behavior ([#33431](https://github.com/BerriAI/litellm/issues/33431), [#33492](https://github.com/BerriAI/litellm/issues/33492)) before production.
- Gateway operability: Expect better CLI startup ([#46803](https://github.com/BerriAI/litellm/pull/46803)), retries ([#46730](https://github.com/BerriAI/litellm/pull/46730)), WebSocket spend accounting ([#46768](https://github.com/BerriAI/litellm/pull/46768)), and load-test tooling ([#46806](https://github.com/BerriAI/litellm/pull/46806)). For Kubernetes, monitor RAM growth ([#27954](https://github.com/BerriAI/litellm/issues/27954)).
- Bedrock/model compatibility: Use [#46670](https://github.com/BerriAI/litellm/pull/46670) for Claude Platform web identity; watch [#33671](https://github.com/BerriAI/litellm/issues/33671) for Guardrails trace, [#43708](https://github.com/BerriAI/litellm/issues/43708) for Databricks gpt-5.6, and [#46777](https://github.com/BerriAI/litellm/pull/46777) for token counting.
- Docker supply chain: Verify cosign signatures for images from the v1.106/v1.107 lines using the key from [commit `0112e53`](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0).

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-10-16

## 1. Today's Highlights

Unsloth shipped **v0.1.908-beta**, adding NVIDIA Nemotron 3.5 (including a Bolt variant that runs in 24 GB) and Meta Muse Spark Open, plus text-diffusion LLM support, DeepSpeed training, and improved prompt caching for coding agents. The same window saw a dense batch of Studio infrastructure PRs targeting multi-GPU placement, NVIDIA MIG masks, AMD installer routing, and ROCm attention selection — much of it still open and under review. On the issue tracker, several long-standing AMD/Studio bugs closed, while KV-cache persistence across model reloads and multi-GPU fine-tuning remain open.

## 2. Releases & Breaking Changes

- **v0.1.908-beta — NVIDIA Nemotron 3.5 + Meta Muse Spark Open**: new model families, text-diffusion LLMs, DeepSpeed training support, better prompt caching for coding agents. Beta channel, so treat as not production-pinned.
- **Security/dependency baseline moved** — [PR #13949](https://github.com/unslothai/unsloth/pull/13949) rebaselines the `pip scan-packages` audit against **unsloth-zoo 2026.10.6** and **transformers 5.20.0**. Upstream file changes invalidated pinned scan entries; if you build from source with your own audit gates, expect to rebaseline.
- **TRL compatibility narrowing** — [PR #13911](https://github.com/unslothai/unsloth/pull/13911) keeps TRL's vLLM weight sync on TRL < 0.28 unless `fast_inference` shares weights. Prior behavior unconditionally no-op'd `GRPOTrainer._move_model_to_vllm` on TRL 0.22–0.27, which is only correct with `fast_inference=True`. Relevant if you pin TRL.
- No public API or config deprecations were announced in this window.

## 3. New Model & Hardware Support

- **NVIDIA Nemotron 3.5 / Nemotron 3.5 "Bolt"** — runs on 24 GB of memory; guide at [unsloth.ai/docs/models/nemotron](https://unsloth.ai/docs/models/nemotro).
- **Meta Muse Spark Open** — local run support in the same beta release.
- **Text-diffusion LLMs** — first-class path added in v0.1.908-beta.
- **FLUX.3 Image Klein (distilled + base), INT8/FP8/GGUF** — [PR #13933](https://github.com/unslothai/unsloth/pull/13933) adds loading for `black-forest-labs/flux-3-image-klein[-base]`, which ship only as Modular Diffusers blocks (`modular_model_index.json`, not `model_index.json`).
- **4-bit fallback for legacy repos** — [PR #13941](https://github.com/unslothai/unsloth/pull/13941) fixes "repository not found" on default 4-bit loads for Tulu-3-8B/70B, SOLAR-10.7B, OpenThinker-7B, Hermes-3-Llama-3.1-405B, Pixtral-12B by falling back to base models.
- **AMD/ROCm**: installer routing fixes for Strix APU + discrete card combos ([PR #13931](https://github.com/unslothai/unsloth/pull/13931)), Windows `hipinfo` double-mask fix ([PR #13925](https://github.com/unslothai/unsloth/pull/13925)), and MIG mask/slice-aware sizing ([PR #13927](https://github.com/unslothai/unsloth/pull/13927)).
- **Requested but not yet supported**: Cohere Command A+ 218B/A25B ([#5952](https://github.com/unslothai/unsloth/issues/5952), closed without support noted).

## 4. Performance & Optimization

- **KV preemption for concurrent chats** — [PR #10301](https://github.com/unslothai/unsloth/pull/10301): parallel chats share one KV cache; llama-server owns parking via `--preempt-ram`, replacing the per-chat full-prefill model. This is the structural fix for the long-context re-prefill complaint in [#9037](https://github.com/unslothai/unsloth/issues/9037).
- **ROCm flash attention selection** — [PR #13792](https://github.com/unslothai/unsloth/pull/13792) replaces a hard-coded model-family table with a whole-block timing probe, so flash_attn can be picked for any model shape on gfx11 (e.g. Strix Halo gfx1151) instead of always falling back to torch SDPA/AOTriton.
- **MoE 4-bit dequant under compile** — [PR #13946](https://github.com/unslothai/unsloth/pull/13946) keeps opted-in `bnb` 4-bit MoE experts outside the `torch.compile` block for `UNSLOTH_MOE_SELECTIVE_DEQUANT=1` / `UNSLOTH_MOE_DEQUANT_CHUNK_EXPERTS=N`.
- **Mixed-generation NVIDIA hosts** — [PR #13923](https://github.com/unslothai/unsloth/pull/13923) derives `bfloat16`, compile, and TMA decisions from every visible GPU rather than device 0 alone, fixing silent capability mismatches on heterogeneous nodes.
- **GPU placement** — [PR #13928](https://github.com/unslothai/unsloth/pull/13928) makes image/video auto-load pick the freest GPU instead of PCI slot 0.
- **Chat UI** — [PR #13919](https://github.com/unslothai/unsloth/pull/13919) fixes scrollbar freezes by disabling code line windowing by default.
- **Release-level**: DeepSpeed training path and reworked prompt caching for coding agents in v0.1.908-beta.

## 5. Stability & Regressions

Ranked by severity; several high-severity items already have fixes or are closed.

1. **ROCm prebuilt binary crashes with OOM at `hipStreamCreateWithFlags`** — [#12205](https://github.com/unslothai/unsloth/issues/12205) (CLOSED): the prebuilt `linux/rocm` asset for `b11160-mix-a6922cc` fails while a source build of the *same* commit works. Mitigated by the AMD installer/arch-routing PRs (#13931, #13925).
2. **Studio multi-GPU mis-sizing and wrong-card loads** — MIG UUID masks were treated as "no mask," causing sizing against full 40/80 GB cards ([#13927](https://github.com/unslothai/unsloth/pull/13927)); auto image/video loads ran on a busy device 0 ([#13928](https://github.com/unslothai/unsloth/pull/13928)). Fix PRs open.
3. **Long chats redo full prefill after model reload** — [#9037](https://github.com/unslothai/unsloth/issues/9037) (OPEN): slot KV is only persisted on idle auto-unload. No merged fix; #10301 is the intended remedy.
4. **Multi-account model sync failure on shared local server** — [#12365](https://github.com/unslothai/unsloth/issues/12365) (CLOSED): current loaded model does not stay consistent across LAN users.
5. **CUBLAS error in `diffusion_prompt_cache.py` during Qwen-Image-2.1 edit** — [#13638](https://github.com/unslothai/unsloth/issues/13638) (CLOSED).
6. **Strix Halo (Windows) defaults to Vulkan, slow prefill** — [#10656](https://github.com/unslothai/unsloth/issues/10656) and [#5405](https://github.com/unslothai/unsloth/issues/5405) (both CLOSED): GPU underutilized or CPU fallback in Docker.
7. **AMD Docker container runs on CPU instead of Strix Halo APU** — [#5405](https://github.com/unslothai/unsloth/issues/5405) (CLOSED).
8. **Windows MXC Sandbox enablement fails with unclear instructions** — [#13656](https://github.com/unslothai/unsloth/issues/13656) (OPEN, unpinned).
9. **Model Hub fit filter leaks over-budget models** — [#13945](https://github.com/unslothai/unsloth/pull/13945) (open fix) and filter behavior bugs [#13613](https://github.com/unslothai/unsloth/issues/13613) (CLOSED).
10. **Vision-Wayland/NVIDIA compositing** — [#13892](https://github.com/unslothai/unsloth/pull/13892) (CLOSED): prefer dmabuf over static compositing-off to avoid Wayland Error 71.
11. **Sandbox escape via unlisted commands** — [PR #13942](https://github.com/unslothai/unsloth/pull/13942) (OPEN): `ffmpeg`/`pandoc` absolute paths wrote outside the sandbox under "Approve for me" because only `cp`/`mv` were checked. Treat agent sandboxing as not yet airtight.
12. **Embeddinggemma-2 file embedding failure at index stage** — [#13663](https://github.com/unslothai/unsloth/issues/13663) (CLOSED).

## 6. What This Means for Application Developers

- **Nemotron 3.5 Bolt at 24 GB is a viable single-consumer-GPU serving target.** If you are sizing an inference box for a Nemotron/Muse Spark-based agent, the 24 GB tier is now the floor rather than a 48–80 GB card.
- **Parallel chat/session serving is the active workstream.** [#10301](https://github.com/unslothai/unsloth/pull/10301) moves to one llama-server holding one KV cache with preemption/parking. If you multiplex many agent sessions against one Studio instance, expect your effective context-per-session to depend on preemption policy — pin your Studio version deliberately until this lands.
- **Homogeneous GPU assumptions are unsafe.** Both [#13923](https://github.com/unslothai/unsloth/pull/13923) and [#13927](https://github.com/unslothai/unsloth/pull/13927) fix decisions previously made from device 0 or whole-card VRAM. On mixed-generation or MIG-partitioned fleets, do not trust pre-fix builds' auto-placement.
- **AMD support is improving but still install-time-fragile.** The Strix Halo CPU-fallback and ROCm OOM bugs are closed, but the fixes are installer/arch-routing changes — rebuild or re-run the installer rather than reusing existing ROCm wheels.
- **Multi-GPU fine-tuning via Studio is still on the roadmap, not shipped.** [#5764](https://github.com/unslothai/unsloth/issues/5764) is open and tagged `on roadmap`; use the Python API with `device_map` for multi-card LoRA today.
- **Preference-tuning parity is a gap.** [#5291](https://github.com/unslothai/unsloth/issues/5291) requests porting GRPO's chunked/hidden-state optimizations to ORPO/DPO/CPO — budget more VRAM for those trainers than for GRPO.
- **Sandboxing is not yet a security boundary.** Until [#13942](https://github.com/unslothai/unsloth/pull/13942) merges, do not rely on "Approve for me" mode to contain tool commands that write via arbitrary binaries.
- **Toolchain pinning matters this cycle**: transformers 5.20.0 and TRL < 0.28 behavior changes are in flight; freeze them explicitly in CI.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*