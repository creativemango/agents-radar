# AI Infrastructure Digest 2026-10-14

> Generated: 2026-10-14 01:21 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

**Data note:** Only LiteLLM and Unsloth published total 24h issue/PR counts in the supplied digests. The other projects provide highlighted issues/PRs only, so “n/r” below means not reported in the source digest.

## 1. Ecosystem Overview

As of 2026-10-14, the AI infrastructure stack is in a hardening phase: no major serving engine — vLLM, SGLang, llama.cpp, or Ollama — shipped a core release, while the gateway and fine-tuning layers moved with LiteLLM `v1.106.0-rc.2` and Unsloth `v0.1.907-beta`. The dominant work is correctness and stability around frontier hybrid models — DeepSeek-V4, GLM-5.3, Qwen3.8, Kimi-K3, Mamba/GDN/MoE — rather than headline feature launches. Performance effort is concentrating on KV-cache/prefix reuse, persistent kernels, PD disaggregation, and hardware-specific quantization paths across SM120, SM90, ROCm, Intel XPU, Arm, and Hexagon. At the same time, gateway and local runtimes face tool-calling/MCP, structured-output, and migration/regression risks, making version pinning and targeted integration testing the practical priority for operators.

## 2. Activity Comparison

| Project | 24h Issues | 24h PRs | Release status | Activity character |
|---|---:|---:|---|---|
| **vLLM** | n/r | n/r | No release; `v0.31.0-cu129` container broken by CUDA/torchcodec mismatch | Heavy correctness/refactor: Ampere DeepSeek-V4-Flash blocker, prefix-cache+MTP corruption, MoE LoRA CUDA-graph deadlock, MonoKernel/DeepGEMM refactors |
| **SGLang** | n/r | n/r | No release; no API/config migrations | CI flakiness dominates; Monokernel RFC, PD disaggregation roadmap, stale-KV HiCache bug, prefill CP expansion |
| **llama.cpp** | n/r | n/r | Builds `b11598`→`b11609`; chat output format breaking change | Security hardening, chat-template part-array output, Arm SME2, Hexagon power modes, SYCL/Vulkan work |
| **Ollama** | n/r | n/r | No release | Throughput regression, cloud `glm-5.3-flash` failures, legacy GGUF migration PR, tool-call+JSON bug |
| **LiteLLM** | 83 updated | 500 updated | `v1.106.0-rc.2` | Very high churn: MCP/tool schemas, proxy UX, token counting, telemetry, provider additions |
| **Unsloth** | 147 touched | 252 touched | `v0.1.907-beta` | Performance-focused release; Studio, video/DiT LoRA, DGX Spark clustering, MoE cache placement |

## 3. Model Support Race

True *shipped* support in this window is limited: most frontier-model work is in open PRs, blockers, or optimization roadmaps. The datacenter serving race remains between vLLM and SGLang, while llama.cpp leads local/quantized hardware breadth.

| Project | New model/architecture moves | Hardware/backend moves | Position |
|---|---|---|---|
| **vLLM** | DeepSeek-V4-Flash still blocked on Ampere/SM8x; Qwen3.8-2.4T-A95B, Qwen3.8-Flash-Next, GLM-5.3, Kimi-K3, MiniMax-M3, hybrid Mamba/GDN, MLA, MoE | Intel XPU MLA, AMD MI355X/gfx950 Quark MXFP4, SM120 gaps, NixlConnector hybrid MLA P/D, spec-decode draft fixes | Broadest frontier datacenter coverage, but Ampere DeepSeek-V4 gap is a major blocker |
| **SGLang** | GLM-5/DeepSeek-V3.2 ROCm DCP, Qwen3.8-Flash-Next, Kimi-K3, Qwen3.5 text checkpoints, Ling-2.6 linear-attention spec decode | ROCm, TRT-LLM MHA prefill CP, FlashInfer prefill CP, SM120 field reports, T-Head SAIL/PPU | Strong challenger; ahead in PD/CP matrix and ROCm disaggregation |
| **llama.cpp** | MiniCPM-V 4.7, Maion-Coder, Qwen4exp/multimodal MTP, Laguna YaRN | Arm SME2 Q4_K/Q6_K, Qualcomm Hexagon power modes, Intel Vulkan FA, SYCL graph replay | Clear leader in local/edge quantization and hardware breadth |
| **Ollama** | K2 Horizon and Snapdragon X Elite are request-only; Qwen3.5 thinking defaults; MiniCPM-V 4.7 load failure | CUDA ports of Metal kernels, Apple M5 Metal fix, MLX metadata/UI, CUDA runtime packaging | Lags on new model/hardware support; depends heavily on llama.cpp/MLX upstream |
| **LiteLLM** | OpenInfer, Laya/Bespoke, MiniMax-H3/H3-Max video, Gemini 4 Argon long decodes, Databricks message shaping | No CUDA/ROCm/Metal changes | Gateway/provider-surface leader |
| **Unsloth** | Wan2.2 video LoRA training, LTX-2.5 defaults, DGX Spark/GB10 two-node clustering, H100 xformers fallback | DGX Spark aarch64/sm_121a, Windows old-driver fallback, H100 sm_90 | Fine-tuning/video/Studio leader; not a datacenter serving engine |

**Who is ahead?** vLLM and SGLang are ahead for datacenter serving of frontier models. vLLM has broader frontier model coverage but is blocked on Ampere for DeepSeek-V4-Flash; SGLang is stronger in PD disaggregation, prefill context parallelism, and ROCm paths. llama.cpp remains the local/quantized runtime benchmark. LiteLLM leads the gateway layer. Unsloth leads fine-tuning, video/DiT, and Studio-style edge workflows.

## 4. Performance Frontier

Optimization effort is fragmented by layer and hardware, with several recurring themes:

- **KV cache and prefix reuse:** vLLM is fighting prefix-caching+MTP corruption on hybrid Mamba/GDN, NVFP4 KV cache is 1.7–1.8× slower than FP8 on SM120, and P/D KV transfer is descriptor-bound for GLM-5.3 on GB200. SGLang is fixing HiCache host-pool allocation and a silent stale-KV early-send bug. llama.cpp tightened `--cache-ram` enforcement and prompt-cache eviction. Unsloth claims 95% cache hit and up to 5× inference gains in `v0.1.907-beta`.
- **Distributed/PD serving:** vLLM advanced NixlConnector hybrid MLA pairing but still has TP worker hangs and MoE+fully-sharded LoRA CUDA-graph deadlocks. SGLang is pushing Q4 PD disaggregation, ROCm DCP, prefill CP auto-selection, and the Weight Cache Daemon, which cuts Qwen3-235B FP8 weight load from ~306–327s to `<1s`. Unsloth added DGX Spark two-node clustering and async replica routing.
- **Persistent kernels and JIT reduction:** vLLM’s MonoKernel RFC and DeepGEMM MegaMoE work target per-layer persistent kernels and less runtime compilation. SGLang’s Monokernel RFC proposes persistent kernels spanning most of a decoder layer. Both signal a broader move away from launch-overhead-heavy decode graphs.
- **Quantization and hardware-specific kernels:** vLLM is balancing FP8, NVFP4, Quark MXFP4, and SM120/SM90 constraints. SGLang has an NVFP4 W4A4 marlin crash on SM100 and an AMD DSV4 FP8 verify path. llama.cpp added Arm SME2 `Q4_K`/`Q6_K` matmul and Hexagon power controls. Unsloth is aligning LTX-2.5 on INT8-ConvRot and skipping broken H100 FA3 paths.
- **Batching/scheduling:** vLLM is fixing spec-decode token budgets and rejection-kernel warmup. SGLang optimized chunked-prefill mamba state tracking and prefill CP BCG selection. Ollama’s cgroup-unaware thread defaults can collapse CPU-container throughput by ~45×. llama.cpp still has `ubatch > 128` long-context failures on some Vulkan paths.
- **Gateway performance:** LiteLLM is not a serving engine, but its telemetry/cost work — `AggregatingSink`, fixed-bucket histograms, Prometheus label fixes, PTU splitting, vLLM passthrough spend logs — targets enterprise observability and cost accuracy.

## 5. Layer Positioning

| Layer | Projects | Primary concerns |
|---|---|---|
| **Datacenter serving engine** | vLLM, SGLang | GPU utilization, frontier model support, PD disaggregation, MoE/MLA/hybrid correctness, distributed serving |
| **Local/edge runtime** | llama.cpp, Ollama | GGUF/quantized inference, CPU/GPU/NPU portability, desktop UX, offline/edge deployment |
| **LLM gateway** | LiteLLM | Multi-provider routing, auth, cost tracking, MCP/tool schemas, observability, provider translation |
| **Training/fine-tuning** | Unsloth | LoRA/QLoRA, Studio workflows, video/DiT training, DGX Spark clustering, export to serving stacks |

Overlaps are increasing. Unsloth Studio is becoming a serving plane with OpenAI/vLLM-compatible surfaces, MCP installs, and two-node DGX Spark routing. Ollama integrates llama.cpp and MLX but trails upstream model support. LiteLLM sits in front of vLLM, SGLang, Ollama, and hosted APIs, so its bugs often become production-facing even when the serving engine is healthy.

## 6. Trend Signals

1. **Hybrid-model correctness is the top risk.** Prefix caching + MTP, MLA LoRA targeting, stale KV in PD HiCache, and MoE/TP/LoRA CUDA-graph deadlocks are silent-corruption class bugs, not clean crashes. Serving operators should disable risky combinations until fixes land.
2. **Hardware fragmentation is worsening.** SM120 NVFP4 KV regression, SM100 marlin crash, SM90 `dsv4_topk` issues, AMD ROCm DCP, Intel XPU MLA, Arm SME2, Qualcomm Hexagon, DGX Spark/GB10 — there is no single “validated” configuration. Pin per-hardware stacks.
3. **Persistent kernels are the next architectural battleground.** vLLM and SGLang both have Monokernel/MonoKernel RFCs, aiming to collapse decode-layer launch overhead. This will matter most for low-latency, high-concurrency agent serving.
4. **PD disaggregation is moving from demo to production, but KV transfer is the bottleneck.** vLLM’s descriptor-bound GB200 result and SGLang’s stale-KV HiCache bug show that disaggregation correctness and transfer efficiency are not solved.
5. **Gateway/tooling fragility is a first-class app risk.** LiteLLM’s MCP/tool-schema issues, Ollama’s `format: json` tool-call failure, and Anthropic `/v1/messages` compatibility work across vLLM/SGLang mean agent developers must test tool schemas, token counting, and streaming fallback attribution explicitly.
6. **Release discipline matters more than version freshness.** vLLM’s `v0.31.0-cu129` container is broken; Ollama reports a 25–30% throughput regression vs `v0.40.1`; LiteLLM is an RC; Unsloth is a beta. For production, pin known-good versions and validate against your own prompt mix.

**What agent/application developers should watch:**
- Always set `max_new_tokens` explicitly on SGLang gRPC; unset values silently cap at 128 tokens.
- Avoid `format: json` with tool calls on Ollama until `#18975` lands.
- Validate MCP/tool schemas and `/v1/messages/count_tokens` if using LiteLLM; array tool params currently trigger `KeyError: 'items'`.
- Avoid MoE + TP + fully-sharded LoRA + CUDA graphs on vLLM `v0.28.0`; avoid NVFP4 KV cache on SM120; avoid `--fp4-gemm-backend marlin` on SM100 NVFP4 W4A4.
- Audit MLA LoRA adapters for silent non-application on DeepSeek/GLM/Kimi MLA architectures.
- Re-verify LoRA seed reproducibility on NVIDIA/AMD/Intel until Unsloth `#13637` lands.
- Treat chat-template output format changes in llama.cpp as a breaking integration point for prompt-string consumers.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-10-14

## 1. Today's Highlights

No releases shipped in the last 24 hours, but activity is heavy: the top thread in the repo remains the Ampere/SM8x blocker for DeepSeek-V4-Flash ([#50576](https://github.com/vllm-project/vllm/issues/50576), 110 comments, 16 👍), signaling real demand for A100/A800 serving of the V4 family. Correctness work dominates the rest of the queue — prefix-caching + MTP corruption on hybrid Mamba/GDN models ([#53912](https://github.com/vllm-project/vllm/issues/53912)) and a CUDA-graph deadlock with fully-sharded MoE LoRA ([#60845](https://github.com/vllm-project/vllm/issues/60845)) are the two most consequential open bugs. In parallel, two large architectural refactors advanced: a unified DeepGEMM MegaMoE expert layer ([#61538](https://github.com/vllm-project/vllm/pull/61538)) and the MonoKernel abstraction for per-layer persistent kernels ([RFC #60904](https://github.com/vllm-project/vllm/issues/60904), [PR #61311](https://github.com/vllm-project/vllm/pull/61311)).

## 2. Releases & Breaking Changes

- **No releases in the last 24h.**
- **Container regression (shipped artifact):** `vllm/vllm-openai:v0.31.0-cu129` fails at startup because bundled `torchcodec 0.17.0` links against CUDA 13 (`libcudart.so.13`) inside a CUDA 12.9 image — [issue #60406](https://github.com/vllm-project/vllm/issues/60406). Pin to an earlier image or rebuild until fixed.
- **In-flight config surface:** ROCm gains a new linear-backend selector (`--linear-backend triton` for per-token FP8 dynamic scaling) in [PR #61091](https://github.com/vllm-project/vllm/pull/61091); MoE backend internals are being consolidated behind a single feature-matrix layer in [PR #61538](https://github.com/vllm-project/vllm/pull/61538) (DeepSeek V4/V4.1 + Kimi K3 MegaMoE), which may shift internal flags.

## 3. New Model & Hardware Support

- **Ampere / SM8x (A100/A800, RTX 30xx) still cannot serve DeepSeek-V4-Flash** or the `-0731` checkpoint; open tracker with the most community engagement — [issue #50576](https://github.com/vllm-project/vllm/issues/50576).
- **Intel XPU MLA:** XPU MLA support is being integrated into the existing `FLASH_ATTN_MLA` backend rather than a separate path — [PR #58297](https://github.com/vllm-project/vllm/pull/58297).
- **AMD MI355X / gfx950 (Quark MXFP4):** active optimization trackers for `Qwen3.8-2.4T-A95B` — [issue #57149](https://github.com/vllm-project/vllm/issues/57149) — and `Qwen3.8-Flash-Next` (GatedDeltaNet + QuerySparse Attention + HyperConnection residual) — [issue #59575](https://github.com/vllm-project/vllm/issues/59575).
- **MonoKernels RFC:** proposed convention `vllm/models/<model>/<vendor>/` where a layer loop calls `mono.forward_layer(...)` to launch an entire decode layer in one persistent kernel — [issue #60904](https://github.com/vllm-project/vllm/issues/60904), draft impl [PR #61311](https://github.com/vllm-project/vllm/pull/61311).
- **Spec-decode draft models:** heterogeneous-vocab draft models (`use_heterogeneous_vocab=True`) are being restored on Model Runner V2 — [PR #61536](https://github.com/vllm-project/vllm/pull/61536); DFlash/DFlash2 full-attention draft layers fixed to stay bidirectional for speculators checkpoints — [PR #61537](https://github.com/vllm-project/vllm/pull/61537).
- **Hybrid MLA P/D pairing:** `NixlConnector` now pairs non-divisible hybrid MLA block sizes (e.g. Kimi-K3 P TP2 / D TP1 on ROCm) — [PR #61002](https://github.com/vllm-project/vllm/pull/61002).
- **SM120 gaps:** `Inkling-Small-NVFP4` fails because `tml_fa4` lacks paged-KV/split-KV on SM 12x — [issue #51405](https://github.com/vllm-project/vllm/issues/51405); block-FP8 cutlass path excludes SM89 (Ada) despite native FP8 tensor cores — [issue #58241](https://github.com/vllm-project/vllm/issues/58241).
- **Hybrid SSM/GDN PD disaggregation** remains an open feature request for models like Qwen3.5-397B-A17B-W8A8 — [issue #43765](https://github.com/vllm-project/vllm/issues/43765).

## 4. Performance & Optimization

- **NVFP4 KV cache is a regression on SM120:** FlashInfer NVFP4 causal prefill measured at **~145.8 ms vs FP8**, i.e. **1.7–1.8× slower** across a matched 48-kernel paged-prefill segment on consumer Blackwell — [issue #50416](https://github.com/vllm-project/vllm/issues/50416). Prefer FP8 KV cache on SM120 for now.
- **P/D KV transfer is descriptor-bound, not bandwidth-bound:** GLM-5.3 on GB200 emits **91k–120k KV descriptors per TP-rank transfer**, making MNNVL/`cuda_ipc` slower than RDMA for this workload — [issue #55434](https://github.com/vllm-project/vllm/issues/55434).
- **Spec-decode acceptance regression:** DFlash2 loses **~5.5 acceptance points** when the hybrid KV cache manager materializes the sliding-window draft as a real SWA group (BLHNC, NIXL P/D) — [issue #60560](https://github.com/vllm-project/vllm/issues/60560); scheduler token-budget fix proposed in [PR #59468](https://github.com/vllm-project/vllm/pull/59468), and CUDA top-k/top-p rejection kernel warmup in [PR #59260](https://github.com/vllm-project/vllm/pull/59260).
- **Qwen4Exp PLE memory fix:** pinning the offloaded PLE table at its exact size drops host memory from **65.2 GiB → 47.8 GiB per rank** and lets TP2 Qwen4Exp boot on 2× DGX Spark at `--gpu-memory-utilization 0.78` — [PR #59848](https://github.com/vllm-project/vllm/pull/59848); decode-step cold-page readahead follow-up in [PR #58835](https://github.com/vllm-project/vllm/pull/58835).
- **ROCm FP8 linear path:** native per-token FP8 Triton scaled-MM backend with RDNA4 (gfx1201 / Radeon AI PRO R9700) tuning — [PR #61091](https://github.com/vllm-project/vllm/pull/61091).
- **YaRN cache sizing:** generic YaRN precomputed only `original_max_position × factor` rows, reading out of bounds for larger `max_position_embeddings` — fixed by sizing to at least `max_position` — [PR #61495](https://github.com/vllm-project/vllm/pull/61495).
- **JIT elimination tracker** continues: zero runtime JIT compilation, with DeepSeek V4 model-level de-JIT items still open — [issue #49349](https://github.com/vllm-project/vllm/issues/49349).
- **AMD CI parity:** request to add MI300/MI325/MI355 to nightly performance + accuracy regression dashboards, matching B200/H200/GB200 coverage — [issue #43916](https://github.com/vllm-project/vllm/issues/43916).

## 5. Stability & Regressions

Ranked by severity:

1. **CUDA graphs deadlock, fused-MoE + fully-sharded LoRA across TP (v0.28.0).** Intermittent hangs serving a Qwen3.5-compatible 35B-A3B MoE with TP=2/TP=4 plus multiple LoRA adapters and `--fully-sharded-loras`; TP=2 serves until concurrency rises. No fix PR linked — [issue #60845](https://github.com/vllm-project/vllm/issues/60845).
2. **Prefix caching + MTP corrupts output on hybrid Mamba/GDN models (v0.28.0).** Reported as an unfixed recurrence of closed #43559 — [issue #53912](https://github.com/vllm-project/vllm/issues/53912). Disable prefix caching or MTP on hybrid models until resolved.
3. **GLM-5.3-Flash ROCm kpool indexer overwrites its own KV cache** — backend advertises a kernel block size the kernels don't use; 4096 pool entries written into 864 slots, **~79% of indexer keys wrong**, silent long-context degradation. Now **CLOSED** — [issue #54359](https://github.com/vllm-project/vllm/issues/54359).
4. **GLM-5.2-FP8 + prefix caching + FP8 KV cache returns empty content / gibberish on repeated identical prompts (v0.27.1)** — [issue #60494](https://github.com/vllm-project/vllm/issues/60494).
5. **LoRA targeting MLA `kv_b_proj` is silently never applied** — decode bypasses it via absorption, prefill via a stale pre-wrap reference; affects DeepSeek V2/V3/R1/V3.2, GLM MoE DSA, Kimi K2 — [issue #48974](https://github.com/vllm-project/vllm/issues/48974).
6. **TP worker hang → `EngineDeadError`, RPC `sample_tokens` timeout** with DeepSeek-V4-Pro, TP=8, MTP speculative decoding — [issue #41530](https://github.com/vllm-project/vllm/issues/41530).
7. **`dsv4_topk` Triton illegal memory access on H20 (SM90) under high concurrency** with DeepSeek-V4.1-Flash; mitigated by `max_num_seqs=256`. **CLOSED** — [issue #56389](https://github.com/vllm-project/vllm/issues/56389).
8. **`_flash_attn_fwd()` unexpected kwarg `dynamic_causal`** crashes `minimax-m3` on main — [issue #46914](https://github.com/vllm-project/vllm/issues/46914).
9. **MiniCPM-V-4.6 fails to load on v0.25.0** (`no module named 'qkqkv_proj'`) — [issue #48449](https://github.com/vllm-project/vllm/issues/48449).
10. **Sporadic 0.7–2.0 s frozen engine step** in V1 multiprocess engine, ~once per generate batch; disappears with `VLLM_ENABLE_V1_MULTIPROCESSING=0` — [issue #48229](https://github.com/vllm-project/vllm/issues/48229).
11. **Whisper segment timestamps drift ~0.5 s per segment** for audio >30 s — [issue #32588](https://github.com/vllm-project/vllm/issues/32588).
12. **Fix PRs in flight:** attention sinks not reloaded through `weight_loader` during layerwise RL weight updates — [PR #57797](https://github.com/vllm-project/vllm/pull/57797); broader reload/refit-equals-fresh fixes — [PR #61287](https://github.com/vllm-project/vllm/pull/61287); AITER indexer rejecting unbuilt index-head counts instead of crashing — [PR #60990](https://github.com/vllm-project/vllm/pull/60990).

## 6. What This Means for Application Developers

- **If you serve hybrid Mamba/GDN models (or any model with MTP), treat prefix caching as unsafe** until [#53912](https://github.com/vllm-project/vllm/issues/53912) lands — corruption is silent, not a crash.
- **Audit LoRA adapters on MLA models.** Adapters targeting `kv_b_proj` on DeepSeek/GLM/Kimi MLA architectures produce no effect and no warning ([#48974](https://github.com/vllm-project/vllm/issues/48974)). If your fine-tune appears to have no behavioral change, this is a candidate root cause.
- **Avoid the combination of MoE + tensor parallelism + `--fully-sharded-loras` + CUDA graphs** on v0.28.0 ([#60845](https://github.com/vllm-project/vllm/issues/60845)); fall back to non-sharded LoRA or a single TP rank.
- **KV cache dtype choices now have hardware-specific tradeoffs:** FP8 remains the safe default; NVFP4 KV cache is currently *slower* for prefill on SM120 ([#50416](https://github.com/vllm-project/vllm/issues/50416)).
- **Agent developers running Claude Code against vLLM** should follow the RFC hardening `/v1/messages` (Anthropic API) for ~25-tool requests, nested JSON schemas and interleaved thinking — [issue #58647](https://github.com/vllm-project/vllm/issues/58647).
- **Multimodal payload reduction:** the token-in/token-out RFC proposes deferring `pixel_values` preprocessing to `/generate`, letting you offload tokenization off the engine node in both disaggregated and aggregated topologies — [issue #46722](https://github.com/vllm-project/vllm/issues/46722).
- **SDK compatibility caution:** proposals to omit unset vLLM extension fields from `/v1/chat/completions` responses would turn `resp.prompt_token_ids` from `None` into `AttributeError` for the OpenAI SDK (`extra="allow"`) — see the correction in [issue #53349](https://github.com/vllm-project/vllm/issues/53349).
- **Embedding/pooling users:** `token_embed` and `token_classify` requests are still force-excluded from prefix-cache reads in 0.29.0 — [issue #60947](https://github.com/vllm-project/vllm/issues/60947).
- **Pin your container tags.** The `v0.31.0-cu129` image is broken by a CUDA-major-mismatched `torchcodec` bundle ([#60406](https://github.com/vllm-project/vllm/issues/60406)).

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-10-14

## 1. Today's Highlights

CI stability work dominated the day, with the flaky-test tracker ([#42752](https://github.com/sgl-project/sglang/issues/42752), 53 comments) drawing the most activity as NVIDIA `PR Test Base`/`PR Test Extra` runs continue to surface infrastructure failures. On the technical side, a new [RFC] Monokernel framework proposal ([#43949](https://github.com/sgl-project/sglang/issues/43949)) landed for discussion, while the Q4 roadmaps for PD Disaggregation ([#43699](https://github.com/sgl-project/sglang/issues/43699)) and Prefill Context Parallelism ([#21788](https://github.com/sgl-project/sglang/issues/21788)) continued to fill in. Three new correctness/crash bugs were filed on 10-13 and remain open — notably a silent stale-KV issue in PD prefill with HiCache.

## 2. Releases & Breaking Changes

No new releases in the last 24h. No API/config migrations announced.

One behavioral trap to be aware of: native gRPC `Generate` silently caps output at 128 tokens when `max_new_tokens` is unset ([#44121](https://github.com/sgl-project/sglang/issues/44121)) — clients should pass the parameter explicitly until this is resolved.

## 3. New Model & Hardware Support

- **T-Head SAIL / PPU roadmap** — first-class support proposed for Zhenwu 810 / 810E / M890P cards ([#37519](https://github.com/sgl-project/sglang/issues/37519)).
- **ROCm decode context parallel (DCP)** for GLM-5 and DeepSeek-V3.2 is in review, sharding MLA KV cache across ranks under `--dcp-size` ([PR #42618](https://github.com/sgl-project/sglang/pull/42618)).
- **Prefill CP support matrix expanding (Q4)** — TRT-LLM MHA backend landed ([#31732](https://github.com/sgl-project/sglang/pull/31732)), GLM5.3-Flash landed ([#42044](https://github.com/sgl-project/sglang/pull/42044)); FlashInfer backend ([#33226](https://github.com/sgl-project/sglang/pull/33226)), Qwen3.8-Flash-Next ([#39724](https://github.com/sgl-project/sglang/pull/39724)) and Kimi-K3 ([#38344](https://github.com/sgl-project/sglang/pull/38344)) still pending ([#21788](https://github.com/sgl-project/sglang/issues/21788)).
- **Qwen3.5 text-only checkpoints** — `Qwen3_5ForCausalLM` / `Qwen3_5MoeForCausalLM` registered so `AutoModelForCausalLM`-style SFT configs load ([PR #27899](https://github.com/sgl-project/sglang/pull/27899)).
- **Anthropic API compatibility** — `/v1/messages` now accepts `document` content blocks for PDF/plain-text ingestion ([PR #31179](https://github.com/sgl-project/sglang/pull/31179)).
- **Speculative decoding for linear attention** — optional cuLA KVBuffer backend for Lightning Attention (Ling-2.6-flash) added ([PR #29737](https://github.com/sgl-project/sglang/pull/29737)).
- **SM120 field reports** — ongoing community data for DeepSeek-V4.1-Flash and GLM-5.3-Flash on 8× RTX PRO 6000 PCIe-only topologies ([#40877](https://github.com/sgl-project/sglang/issues/40877)).

## 4. Performance & Optimization

- **Weight Cache Daemon roadmap** ([#33522](https://github.com/sgl-project/sglang/issues/33522)): Phase 1 already cuts weight load from ~306–327s to **<1s** on Qwen3-235B FP8 via per-rank CUDA-IPC weight serving; roadmap tracks the next phases for fast engine recovery.
- **[RFC] Monokernel framework** ([#43949](https://github.com/sgl-project/sglang/issues/43949)): proposes persistent kernels spanning most of a decoder layer, exposed as a plugin with model-supplied specs and SGLang-side routing/safety hooks.
- **HiCache host pool allocation** ([PR #43978](https://github.com/sgl-project/sglang/pull/43978)): parallel, THP-backed host pool allocation to remove the single-thread `MAP_POPULATE` prefault bottleneck in `alloc_mmap`.
- **Chunked-prefill mamba optimization** ([PR #30369](https://github.com/sgl-project/sglang/pull/30369)): skip mamba/SSM state tracking for intermediate prefill chunks, since only the final chunk lands in the radix cache.
- **DeepSeek V4.1 optimization roadmap** ([#42170](https://github.com/sgl-project/sglang/issues/42170)): mHC cleanup merged, `q_rope_store` folded into `fused_q_norm_rope`; mHC SP + engram fusion under way ([#43065](https://github.com/sgl-project/sglang/pull/43065)).
- **Prefill CP BCG auto-selection** ([PR #44022](https://github.com/sgl-project/sglang/pull/44022)): supported `(ContextParallelSize, ...)` configs automatically select breakable CUDA graphs; DSV4 temporarily excluded due to token-ID sharding mismatch.
- **AMD DSV4 FP8 verify** ([PR #44003](https://github.com/sgl-project/sglang/pull/44003)): writes the SWA ring inside the fused norm/rope kernel during FP8 target-verify.
- **KVPress integration** ([#10585](https://github.com/sgl-project/sglang/issues/10585)): open good-first-issue to enable more KV-cache compression heuristics for long-context serving.

## 5. Stability & Regressions

Ranked by severity:

1. **Silent stale KV in PD prefill + HiCache** — [OPEN] [#44070](https://github.com/sgl-project/sglang/issues/44070). With the default `SGLANG_DISAGG_PREFILL_EARLY_SEND_CACHED_PREFIX=1`, the early cached-prefix send can transfer KV to decode before the HiCache load-back completes, for prefixes resident only in host memory. Correctness bug, no fix PR referenced.
2. **NVFP4 W4A4 crash with `--fp4-gemm-backend marlin` on SM100** — [OPEN] [#43988](https://github.com/sgl-project/sglang/issues/43988). `mm_fp4` has no `'marlin'` backend, so the flag crashes on NVFP4 W4A4 checkpoints. No fix PR referenced.
3. **gRPC output silently capped at 128 tokens** — [OPEN] [#44121](https://github.com/sgl-project/sglang/issues/44121). Triggered when `max_new_tokens` is unset; affects native gRPC `Generate`.
4. **GLM-5.3-Flash FA4 crash on SM120 at CUDA-graph capture** — [OPEN] [#42012](https://github.com/sgl-project/sglang/issues/42012). Hybrid-extend reshape fails in `fa4`; `triton` is currently the only working attention backend on RTX PRO 6000.
5. **sgl-deep-gemm 0.2.0 SM90 weight-scale alias** — [OPEN] [#39684](https://github.com/sgl-project/sglang/issues/39684). Transform returns a non-owning alias; follow-up to the activation-scale fix in #32188.
6. **Unsafe pickle fallback when loading PyTorch checkpoints** — [OPEN PR #42760](https://github.com/sgl-project/sglang/pull/42760). Removes `weights_only=False` fallback in `_load_pt_file` and legacy tar checkpoint paths (security hardening).
7. **CI flakiness / infra failures** — [OPEN] [#42752](https://github.com/sgl-project/sglang/issues/42752) (53 comments) and long-running tracker [#17050](https://github.com/sgl-project/sglang/issues/17050) (last auto-update: 1 broken, 4 flaky, 1165 recently fixed).
8. **Custom all-reduce deadlock fixed** — [CLOSED PR #31135](https://github.com/sgl-project/sglang/pull/31135). Two CUDA streams issuing all-reduce on the same communicator could hang GPU-pinned indefinitely; fix landed.
9. **Resolved today**: DeepSeekV4TokenToKVPool `get_cpu_copy()` retract crash ([#33385](https://github.com/sgl-project/sglang/issues/33385), closed), FlashInfer `RadixTopKRenormProbKernel_MultiCTA` coredump ([#32283](https://github.com/sgl-project/sglang/issues/32283), closed, inactive), `bench_serving --dataset-name openai` double-counting input tokens ([#43929](https://github.com/sgl-project/sglang/issues/43929), closed).

## 6. What This Means for Application Developers

- **gRPC clients: always set `max_new_tokens` explicitly.** Until [#44121](https://github.com/sgl-project/sglang/issues/44121) is fixed, unset values silently truncate at 128 tokens — easy to miss in production.
- **PD disaggregation + HiCache users should audit early prefix send.** If you rely on host-tier prefix caching and the default `SGLANG_DISAGG_PREFILL_EARLY_SEND_CACHED_PREFIX=1`, [#44070](https://github.com/sgl-project/sglang/issues/44070) can serve stale KV for shared prefixes — consider disabling early send until patched.
- **Avoid `--fp4-gemm-backend marlin` on SM100 NVFP4 W4A4** ([#43988](https://github.com/sgl-project/sglang/issues/43988)); use a supported backend.
- **Anthropic-compatible apps get native document ingestion.** `/v1/messages` `document` content blocks now work ([PR #31179](https://github.com/sgl-project/sglang/pull/31179)), useful for RAG pipelines that pass PDFs upstream.
- **Faster restarts are coming.** The Weight Cache Daemon trajectory (#33522) takes large-model weight load from ~5 minutes to sub-second; if you deploy large FP8 MoE models with frequent rollouts, watch this roadmap.
- **Roadmaps to track for capacity planning:** Q4 PD Disaggregation ([#43699](https://github.com/sgl-project/sglang/issues/43699)) and Prefill CP ([#21788](https://github.com/sgl-project/sglang/issues/21788)) — both expand the model/backend matrix, so configurations validated today may gain new, faster defaults (e.g., automatic BCG in [PR #44022](https://github.com/sgl-project/sglang/pull/44022)).

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-10-14

## 1. Today's Highlights

A security- and stability-heavy day: `llama-server` got two hardening fixes — a `std::regex` stack-overflow crash on d1-omni/kev special tokens (b11608, [#30243](https://github.com/ggml-org/llama.cpp/pull/30243)) and a JSON nesting-depth limit for request parsing ([#30242](https://github.com/ggml-org/llama.cpp/pull/30242)). The chat templating pipeline took a notable API step with formatted output now returned as an array of *parts* rather than a single string (b11607, [#30389](https://github.com/ggml-org/llama.cpp/pull/30389)). On the hardware side, Arm KleidiAI gained SME2 `Q4_K`/`Q6_K` matmul support (b11604, [#29219](https://github.com/ggml-org/llama.cpp/pull/29219)) and Hexagon picked up configurable HTP DCVS/HMX power modes (b11601, [#30423](https://github.com/ggml-org/llama.cpp/pull/30423)).

## 2. Releases & Breaking Changes

- **Chat output format change** — Formatted chat output is now an array of typed parts instead of a plain string (b11607, [#30389](https://github.com/ggml-org/llama.cpp/pull/30389)). Downstream consumers that index the formatted prompt as a string (server bindings, grammar hooks, tooling) must adapt. This is step 2 of [#28249](https://github.com/ggml-org/llama.cpp/issues/28249); expect a follow-up enabling per-part special-token parsing.
- **Special-token escaping rewrite** (b11608, [#30243](https://github.com/ggml-org/llama.cpp/pull/30243)) — Replaces `std::regex_replace`; behavior is intended to be equivalent, but any code depending on exact escaping semantics for `<|name|>`-style markers should be re-verified.
- **Backend search-path error handling simplified** (b11606, [#28367](https://github.com/ggml-org/llama.cpp/pull/28367)) — Error messages when a backend library fails to load were reworked; log-scraping tooling may need updates.
- Builds in this window: b11598 → b11609.

## 3. New Model & Hardware Support

- **Arm SME2 quantized matmul** — KleidiAI backend adds SME2 `Q4_K` and `Q6_K` matmul paths (b11604, [#29219](https://github.com/ggml-org/llama.cpp/pull/29219)).
- **Qualcomm Hexagon** — Configurable HTP DCVS/HMX power modes exposed via `GGML_HEXAGON_POWER_MODE` and a `--hex-power-mode` flag in `run.py` (b11601, [#30423](https://github.com/ggml-org/llama.cpp/pull/30423)).
- **Laguna models** — YaRN beta metadata is now loaded from GGUF (b11598, [#30331](https://github.com/ggml-org/llama.cpp/pull/30331)).
- **MiniCPM-V 4.7** — Support PR open, reusing the 4.6 path with added 3D RoPE ([#29416](https://github.com/ggml-org/llama.cpp/pull/29416), closed).
- **Maion-Coder architecture** — New native arch support under review ([#29778](https://github.com/ggml-org/llama.cpp/pull/29778)).
- **Qwen4exp / multimodal MTP** — Padding for expert projections, 64-bit array metadata loading, and multimodal MTP in one PR ([#30316](https://github.com/ggml-org/llama.cpp/pull/30316)).
- **Intel Vulkan FA prefill kernel** — Prefill flash-attention shader targeting Intel GPUs ([#29357](https://github.com/ggml-org/llama.cpp/pull/29357)).
- **SYCL graph record/replay** — Port of CUDA graphs to SYCL ([#28725](https://github.com/ggml-org/llama.cpp/pull/28725)).

## 4. Performance & Optimization

- **Vulkan `pool_2d`** — When the pooling window exceeds a subgroup, a whole subgroup now computes each output (b11603, [#30119](https://github.com/ggml-org/llama.cpp/pull/30119)). Relevant to vision/embedding workloads.
- **Meta backend** — All subgraphs are recreated when the arena is reallocated, fixing stale graph handles (b11602, [#30453](https://github.com/ggml-org/llama.cpp/pull/30453)).
- **No-cache attention KQ bias padding** — T5-style relative position bias is now zero-padded to the 256-wide KV padding (b11600, [#30446](https://github.com/ggml-org/llama.cpp/pull/30446)).
- **SYCL memory/perf work** — Fixes for out-of-bounds strided reads in `mul_mat`, split-buffer and host-pool errors ([#29889](https://github.com/ggml-org/llama.cpp/pull/29889), merge-ready); compiler-warning/dead-code cleanup ([#30271](https://github.com/ggml-org/llama.cpp/pull/30271)). A SYCL **PP performance regression** is still open ([#30248](https://github.com/ggml-org/llama.cpp/issues/30248)).
- **Server prompt cache** — Strict `--cache-ram` enforcement landed ([#25070](https://github.com/ggml-org/llama.cpp/pull/25070)), with a follow-up preventing cache eviction of a prompt an incoming request is about to restore ([#30484](https://github.com/ggml-org/llama.cpp/pull/30484)); further work reduces incidental prefix eviction ([#30319](https://github.com/ggml-org/llama.cpp/pull/30319)).
- **`MUL_MAT_ID` skip semantics** — Cross-backend PR allows `-1` expert indices to skip computation entirely ([#26631](https://github.com/ggml-org/llama.cpp/pull/26631)).

## 5. Stability & Regressions

Ranked by severity:

1. **Crash: `llama-server` stack overflow on special-token escaping** — `/v1/systemone` with d1-omni/kev models; libstdc++ recursion on `<|name|>` caused HTTP-thread stack overflow. **Fixed** in b11608 ([#30243](https://github.com/ggml-org/llama.cpp/pull/30243)).
2. **Crash: deeply nested JSON** — SIGSEGV on Linux / SIGBUS on macOS from recursive tree walks (jinja `tojson`, json-schema-to-grammar, `/v1/*`). **Fix PR** limits nesting to 128 levels ([#30242](https://github.com/ggml-org/llama.cpp/pull/30242)).
3. **CUDA crash: `fattn.cu:579` fatal error** — Server aborts after editing the system message with Gemma 4 31B + MTP + tensor split on 2×RTX3090. **Open, unconfirmed** ([#24440](https://github.com/ggml-org/llama.cpp/issues/24440)).
4. **SM_60 (Tesla P100) quality loss** — FP32 math silently executed in FP16; fixes exist in downstream forks but not upstream. **Open** ([#25593](https://github.com/ggml-org/llama.cpp/issues/25593)).
5. **MCP stdio server deadlock** — Config-spawned MCP servers hang permanently on tool calls >~1–5 KB. **Open** ([#28723](https://github.com/ggml-org/llama.cpp/issues/28723)).
6. **Vulkan device loss / PP degradation** — Multiple reports: ANV FA fallback to SCALAR giving O(N²) PP and device loss ([#27638](https://github.com/ggml-org/llama.cpp/issues/27638)); `ErrorDeviceLost` on RX 7900 XTX ([#30010](https://github.com/ggml-org/llama.cpp/issues/30010)); MiniMax-M2 long-context collapse with `ubatch > 128` on Strix Halo ([#30364](https://github.com/ggml-org/llama.cpp/issues/30364)); RDNA3 PP regression since b10780 ([#28752](https://github.com/ggml-org/llama.cpp/issues/28752), closed).
7. **Metal OOM false positive** — Server reports "model loaded" and binds the port after a fatal Metal OOM during init; every request then 500s ([#27309](https://github.com/ggml-org/llama.cpp/issues/27309)).
8. **`--cache-ram -1` is not "no limit"** — Token cap stays at `n_ctx`; hybrid models grow ~640 MiB RAM per short prompt ([#29324](https://github.com/ggml-org/llama.cpp/issues/29324)).
9. **`--n-cpu-moe` below threshold** — Crashes MTP draft model load with `invalid vector subscript` ([#27717](https://github.com/ggml-org/llama.cpp/issues/27717)).
10. **json-schema-to-grammar field ordering bug** — Optional property listed before a required one gets dropped, blocking schema-ordered output ([#30321](https://github.com/ggml-org/llama.cpp/issues/30321)).
11. **Compile: Vulkan on Termux** — Shader optimization fails with `Invalid capability operand: 5447` ([#28234](https://github.com/ggml-org/llama.cpp/issues/28234)).

Also closed/stale this cycle: OpenVINO AVX-512 `STATUS_ILLEGAL_INSTRUCTION` ([#28726](https://github.com/ggml-org/llama.cpp/issues/28726)), DFlash2 tensor-split assertion ([#27819](https://github.com/ggml-org/llama.cpp/issues/27819)), DFlash drafter bind failure on array `sliding_window_pattern` ([#26894](https://github.com/ggml-org/llama.cpp/issues/26894)), MCP server UI display regression ([#26497](https://github.com/ggml-org/llama.cpp/issues/26497)).

## 6. What This Means for Application Developers

- **Audit your chat-template consumers.** The move to part-based formatted output ([#30389](https://github.com/ggml-org/llama.cpp/pull/30389)) is the first step toward selective special-token parsing; if you construct prompts by string-concatenating `format_chat` output, verify against the new structure before the next release you ship.
- **Upgrade promptly if you run d1-omni/kev or expose the system-prompt endpoint.** b11608 closes a remotely triggerable stack-overflow crash ([#30243](https://github.com/ggml-org/llama.cpp/pull/30243)); the JSON-nesting fix ([#30242](https://github.com/ggml-org/llama.cpp/pull/30242)) should land next.
- **Prompt-cache math changed.** `--cache-ram` is now a hard limit ([#25070](https://github.com/ggml-org/llama.cpp/pull/25070)) and slots no longer evict the prompt a request is about to reuse ([#30484](https://github.com/ggml-org/llama.cpp/pull/30484)). Re-tune cache sizing; note [#29324](https://github.com/ggml-org/llama.cpp/issues/29324) means `-1` does not mean unlimited.
- **Agent/MCP deployments:** the stdio MCP deadlock ([#28723](https://github.com/ggml-org/llama.cpp/issues/28723)) is unresolved — keep tool-call payloads small or prefer HTTP MCP transports until it is fixed.
- **Intel GPU users:** SYCL is getting active hardening but currently has a PP regression ([#30248](https://github.com/ggml-org/llama.cpp/issues/30248)); Vulkan on Intel/AMD still risks device loss at long context ([#30364](https://github.com/ggml-org/llama.cpp/issues/30364), [#30010](https://github.com/ggml-org/llama.cpp/issues/30010)). Pin builds and test long-context batches before upgrading.
- **Arm edge/embedded:** SME2 `Q4_K`/`Q6_K` ([#29219](https://github.com/ggml-org/llama.cpp/pull/29219)) and Hexagon power-mode control ([#30423](https://github.com/ggml-org/llama.cpp/pull/30423)) are worth benchmarking for throughput-per-watt on new hardware.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-10-14

## 1. Today's Highlights
No new releases were published in the last 24h. The most urgent items are a reported 25–30% throughput regression versus v0.40.1 and continued cloud generation failures on `glm-5.3-flash:cloud`. Backend work is active around MLX/CUDA kernels, legacy GGUF migration, and restoring tool-call behavior when structured JSON output is enabled.

## 2. Releases & Breaking Changes
- No new releases in last 24h.
- PR #18882 (open): remove llama.cpp compatibility patches and migrate legacy GGUFs on load; would replace manifests atomically and reclaim unreferenced legacy blobs during conversion/startup GC. Potential migration/disk impact. https://github.com/ollama/ollama/pull/18882
- PR #18990 (open): `SYSTEM ""` in a Modelfile would clear the system prompt instead of inheriting the base model's. https://github.com/ollama/ollama/pull/18990
- PR #18991 (open): allow model metadata to set default thinking behavior; smaller Qwen 3.5 models may be republished with thinking disabled. https://github.com/ollama/ollama/pull/18991
- PR #18993 (open): MLX version bump; notes it should merge after #18991 and that the bump worsens thinking loops in smaller Qwen 3.5 models. https://github.com/ollama/ollama/pull/18993

## 3. New Model & Hardware Support
- Snapdragon X Elite NPU/Adreno GPU support request remains open with 68 👍: #5360. https://github.com/ollama/ollama/issues/5360
- K2 Horizon (`k2-horizon`) model family support request: 0.9B–36B MoE, official GGUF: #18698. https://github.com/ollama/ollama/issues/18698
- Tool calls with structured output: PR #18975 proposes allowing model-native tool-call prefixes while keeping formatted responses constrained when no tool is chosen; carries Qwen 3.5 and generic tool-call tags through llama-server/MLX grammar. https://github.com/ollama/ollama/pull/18975
- CUDA backend: closed PR #18657 ports Metal-only custom kernels (`mamba2_scan`, `depthwise_conv_silu`, `gated_delta`) to CUDA, reducing graph fallback. https://github.com/ollama/ollama/pull/18657
- Apple Metal: closed PR #14604 fixes a `static_assert` crash on Apple M5 `MTLGPUFamilyApple10` devices. https://github.com/ollama/ollama/pull/14604
- MLX metadata/UI: #18971 reports missing MLX badges for `embeddinggemma-2` tags on ollama.com. https://github.com/ollama/ollama/issues/18971
- CUDA runtime packaging: PR #17956 deduplicates CUDA runtime payloads across Windows/Linux MLX CUDA builds. https://github.com/ollama/ollama/pull/17956
- Integration: PR #18989 adds a Runner-only Wirken agent gateway integration to `ollama launch`. https://github.com/ollama/ollama/pull/18989

## 4. Performance & Optimization
- #18968: report of 25–30% lower tokens/s across all models vs v0.40.1. No fix PR listed yet. https://github.com/ollama/ollama/issues/18968
- #17916: default `n_threads` ignores cgroup v2 CPU quota and cpuset; CPU-limited containers see ~45x throughput collapse. Workaround: set explicit thread/CPU budget. https://github.com/ollama/ollama/issues/17916
- #18232: reproducible CUDA crash on Blackwell RTX 5060 Ti; root cause identified as `num_ctx` affecting Flash Attention MMA kernel shared-memory allocation. https://github.com/ollama/ollama/issues/18232
- #17380: intermittent `CUDA error: shared object initialization failed` at warmup on Windows/RTX 5070 Ti, with silent `CUDA_Host` → CPU pinned-buffer fallback. https://github.com/ollama/ollama/issues/17380
- PR #18657: CUDA ports of Metal kernels reduce graph-op fallback for Mamba-2/conv/gated-delta paths. https://github.com/ollama/ollama/pull/18657
- PR #18882: refreshes llama.cpp and removes private compatibility patches, with legacy GGUF conversion on load. https://github.com/ollama/ollama/pull/18882

## 5. Stability & Regressions
Ranked by severity:
1. #18980 — massive generation problems since 10-10 with `glm-5.3-flash:cloud`; reports 5–8% failure/degeneration, host-side zeroed data. Critical for cloud users. https://github.com/ollama/ollama/issues/18980
2. #18968 — 25–30% performance regression across all models vs v0.40.1. High. https://github.com/ollama/ollama/issues/18968
3. #18796 — on Raspberry Pi 5/Debian 13, second `ollama run` hangs with no output. High. https://github.com/ollama/ollama/issues/18796
4. #18983 — `openbmb/minicpm-v4.7:35b-a3b` fails to load with `unknown projector type: minicpmv4_7` / CLIP load failure on Windows 0.40.3. High for vision users. https://github.com/ollama/ollama/issues/18983
5. #18944 — tool calls fail when `format: json` is set; PR #18975 is the proposed fix. High for agent developers. https://github.com/ollama/ollama/issues/18944
6. #18232 / #17380 — CUDA crashes on Windows/Blackwell; #18232 has a root-cause analysis, no fix PR listed. High. https://github.com/ollama/ollama/issues/18232 · https://github.com/ollama/ollama/issues/17380
7. #17916 — CPU container throughput collapse due to cgroup-unaware thread defaults. Medium-high. https://github.com/ollama/ollama/issues/17916
8. #18716 — `ollama pull qwen3.6` fails with `redirect target not allowed`. Medium. https://github.com/ollama/ollama/issues/18716
9. #18971 — MLX badges missing for some MLX-only `embeddinggemma-2` tags. Low. https://github.com/ollama/ollama/issues/18971
10. #2519 — Windows Defender false-positive alert closed. Low/informational. https://github.com/ollama/ollama/issues/2519

## 6. What This Means for Application Developers
- Avoid `format: json` with tool calls until #18975 lands; otherwise use plain tool-calling mode or validate tool-call output separately. https://github.com/ollama/ollama/issues/18944
- Treat v0.40.1 as a baseline if throughput matters and pin/test recent versions; #18968 reports a significant regression. https://github.com/ollama/ollama/issues/18968
- In CPU-limited containers, explicitly set thread counts/CPU budgets rather than relying on defaults; #17916 shows severe cgroup-related collapse. https://github.com/ollama/ollama/issues/17916
- Cloud users should add retries/fallbacks and review data-retention terms; `glm-5.3-flash:cloud` instability (#18980) and cached-input/ZDR pricing questions (#17489) remain unresolved. https://github.com/ollama/ollama/issues/18980 · https://github.com/ollama/ollama/issues/17489
- Windows CUDA/Blackwell deployments should track #17380 and #18232; changing `num_ctx` may avoid the Flash Attention MMA shared-memory crash until a fix lands. https://github.com/ollama/ollama/issues/18232
- If #18882 merges, plan for legacy GGUF conversion and manifest/blob lifecycle changes on first load; pin/back up models if reproducibility is required. https://github.com/ollama/ollama/pull/18882
- Hardware/model coverage is still request-only for Snapdragon X Elite (#5360) and K2 Horizon (#18698); do not assume production support yet. https://github.com/ollama/ollama/issues/5360 · https://github.com/ollama/ollama/issues/18698

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-10-14

Source: [github.com/BerriAI/litellm](https://github.com/BerriAI/litellm)

## 1. Today's Highlights

- LiteLLM published [v1.106.0-rc.2](https://github.com/BerriAI/litellm/releases/tag/v1.106.0-rc.2); the supplied release notes focus on cosign Docker image signature verification, with no migration or API-breaking notes visible.
- Activity remains very high: 83 issues and 500 PRs updated in the last 24h. MCP/tool handling, proxy admin UX, token counting, and provider translation bugs dominate issue churn.
- A substantial telemetry/observability PR stack is moving forward, alongside provider additions such as OpenInfer, Laya/Bespoke, MiniMax video, and Gemini long-decode continuation.

## 2. Releases & Breaking Changes

- [v1.106.0-rc.2](https://github.com/BerriAI/litellm/releases/tag/v1.106.0-rc.2) — release candidate. All Docker images are signed with cosign; releases use the same key introduced in commit `0112e53`. No API/config migration instructions in the supplied data.
- [PR #45497](https://github.com/BerriAI/litellm/pull/45497) — `fix(rag)!`: uses team provider credentials, allows viewer and JWT team access, and changes failure behavior for `/rag/ingest` and `/rag/query`. Treat as potentially breaking for RAG deployments.
- No other confirmed breaking changes surfaced today.

## 3. New Model & Hardware Support

- [PR #41034](https://github.com/BerriAI/litellm/pull/41034) — adds `openinfer` as an OpenAI-compatible provider, routes `openinfer/<model>`, and includes spend tracking catalog rates.
- [PR #45260](https://github.com/BerriAI/litellm/pull/45260) — adds Laya and Bespoke Nimble to the Add Model form and registers `laya/*` / `bespoke/*` Decisions routes.
- [PR #43705](https://github.com/BerriAI/litellm/pull/43705) — adds MiniMax-H3 and H3-Max video generation via `/v1/videos`, with create/status/content/list/delete routes and per-second pricing.
- [PR #46240](https://github.com/BerriAI/litellm/pull/46240) — Gemini 4 Argon long decodes auto-continue using `continuationToken`; addresses 32K per-call / 64K per-stream output caps.
- [PR #45686](https://github.com/BerriAI/litellm/pull/45686) — Databricks non-Claude models now receive a single plain text block string, avoiding `json_object` 400s.
- No CUDA/ROCm/Metal/CPU or quantization-format changes were present in the supplied data.

## 4. Performance & Optimization

- [PR #45487](https://github.com/BerriAI/litellm/pull/45487) — adds `AggregatingSink`, fixed-bucket histograms, `Report`, and `HttpExporter` to reduce per-request telemetry volume and traffic-shape exposure.
- [PR #45489](https://github.com/BerriAI/litellm/pull/45489) — `TelemetryMiddleware` times LLM, MCP, and A2A requests as the client sees them and records provider attempts.
- [PR #45490](https://github.com/BerriAI/litellm/pull/45490) — local report table, persistent instance ID, and admin export route for air-gapped proxies.
- [PR #45494](https://github.com/BerriAI/litellm/pull/45494) — Admin UI telemetry settings page and gated page-navigation events.
- [PR #46267](https://github.com/BerriAI/litellm/pull/46267) — fixes Prometheus custom metadata labels by skipping labels a metric already carries; previously broke latency, request-count, and deployment metrics for project keys.
- [PR #43043](https://github.com/BerriAI/litellm/pull/43043) — splits Azure PTU deployments across teams with per-team PTU ceilings and PTU-hour reporting.
- Cost/billing accuracy: [PR #45733](https://github.com/BerriAI/litellm/pull/45733) writes spend logs for `/vllm` passthrough calls; [PR #43667](https://github.com/BerriAI/litellm/pull/43667) bills every non-stream MCP auto-execute round; [PR #46288](https://github.com/BerriAI/litellm/pull/46288) prices Bedrock cross-region profiles at deployment rates.
- [PR #46332](https://github.com/BerriAI/litellm/pull/46332) — removes `azure_ai/claude-fable-5-5` from cost maps because Azure does not list it.

## 5. Stability & Regressions

Ranked roughly by severity:

- [Issue #46045](https://github.com/BerriAI/litellm/issues/46045) — `ModuleNotFoundError: No module named 'mcp'` when tools are set and `_skip_mcp_handler` is not configured. No fix PR shown.
- [Issue #46052](https://github.com/BerriAI/litellm/issues/46052) — `token_counter` raises `KeyError: 'items'` for array tool params without `items`, causing HTTP 500 from `/v1/messages/count_tokens`. Affects many MCP tool schemas. No fix PR shown.
- [Issue #46209](https://github.com/BerriAI/litellm/issues/46209) — `TypeError: can only concatenate str (not "list") to str` with Azure-hosted `mistral-medium-3-5` after tool results, especially longer results; affects streaming and non-streaming. No fix PR shown.
- [Issue #46247](https://github.com/BerriAI/litellm/issues/46247) — `bedrock_mantle` geo inference profile returns empty `completed` responses with zero usage. No fix PR shown.
- [Issue #17475](https://github.com/BerriAI/litellm/issues/17475) — proxy no longer shows model lists to non-admin users; admin-only view works. 14 comments, 6 upvotes. No fix PR shown.
- [Issue #44093](https://github.com/BerriAI/litellm/issues/44093) — MCP tool pin ignores annotations and `outputSchema`, so `readOnlyHint` can flip to destructive without a drift alert. No fix PR shown.
- [Issue #45645](https://github.com/BerriAI/litellm/issues/45645) — Responses API mid-stream 429 can be recorded against the fallback deployment and cool it down incorrectly. No fix PR shown.
- [Issue #40780](https://github.com/BerriAI/litellm/issues/40780) — `openai/`-prefixed self-hosted models route `/v1/messages` to the Responses API, silently breaking multimodal for vLLM, llama.cpp, SGLang, TGI, and LM Studio. No fix PR shown.
- [Issue #41361](https://github.com/BerriAI/litellm/issues/41361) — `/v1/messages` drops signature-only thinking blocks for Claude Opus 4.8 / Fable 5 / Fable 5.1, losing reasoning content. No fix PR shown.
- [Issue #46076](https://github.com/BerriAI/litellm/issues/46076) — management endpoints return raw Postgres exception text in 500 responses. Partial fix: [PR #46330](https://github.com/BerriAI/litellm/pull/46330) hides project-creation DB errors only.
- [Issue #45988](https://github.com/BerriAI/litellm/issues/45988) — startup schema check suggests an invalid primary-key rewrite for partitioned `LiteLLM_SpendLogs`. No fix PR shown.
- [Issue #46107](https://github.com/BerriAI/litellm/issues/46107) — per-page PDF token counting uses Anthropic pricing for every provider, so Router pre-call checks reject Gemini/OpenAI PDF requests. No fix PR shown.
- [Issue #46198](https://github.com/BerriAI/litellm/issues/46198) — dashboard calls admin-only `/health/license` for non-admin users, flooding logs with ERROR stacktraces. No fix PR shown.
- [Issue #46095](https://github.com/BerriAI/litellm/issues/46095) — OpenRouter streaming chunks report the requested router name instead of the model OpenRouter actually served. No fix PR shown.
- [Issue #41544](https://github.com/BerriAI/litellm/issues/41544) — Bedrock audio transcription fails with HTTP 400 because system messages are incompatible with audio blocks. No fix PR shown.
- [Issue #28539](https://github.com/BerriAI/litellm/issues/28539) — `Custom_openaiException`: `AsyncCompletions.create()` receives unexpected `client_metadata`. No fix PR shown.
- [Issue #33365](https://github.com/BerriAI/litellm/issues/33365) — LiteLLM cannot be installed in an air-gapped environment. No fix PR shown.
- [Issue #33405](https://github.com/BerriAI/litellm/issues/33405) — proxy pins `gunicorn>=23,<24`; advisory AIKIDO-2026-10742 flags request smuggling and recommends `gunicorn` 26. No fix PR shown.
- Low severity: [Issue #46212](https://github.com/BerriAI/litellm/issues/46212) — flaky MCP concurrent schema-validation test in the mcp-server CI shard.

Closed/resolved in the last day:

- [Issue #32281](https://github.com/BerriAI/litellm/issues/32281) — MCP tool conversion dropped `function` wrapper key on `/v1/chat/completions` with `hosted_vllm`.
- [Issue #16060](https://github.com/BerriAI/litellm/issues/16060) — `usage-based-routing-v2` erroneously found no deployment.
- [Issue #29826](https://github.com/BerriAI/litellm/issues/29826) — MCP `ListResourcesResult.resources` tolerance for missing field.
- [Issue #32280](https://github.com/BerriAI/litellm/issues/32280) — `/anthropic` passthrough logging crashed with `KeyError: 'content'` on `count_tokens`.
- [Issue #35790](https://github.com/BerriAI/litellm/issues/35790) — Vertex AI Claude `json_tool_call` response not unwrapped.
- [Issue #32118](https://github.com/BerriAI/litellm/issues/32118) — multiple API keys with rotation for search tools.

Other notable fix PRs:

- [PR #46187](https://github.com/BerriAI/litellm/pull/46187) — SSO falls back to `id_token` claims when generic userinfo response is not JSON.
- [PR #45686](https://github.com/BerriAI/litellm/pull/45686) — Databricks non-Claude message shaping fix.
- [PR #46267](https://github.com/BerriAI/litellm/pull/46267) — Prometheus label duplication fix.
- [PR #46288](https://github.com/BerriAI/litellm/pull/46288) — Bedrock cross-region cost selector fix.

## 6. What This Means for Application Developers

- MCP/tool use is the highest-risk area this week. Validate `mcp` installation, test tool schemas with array params, and pin LiteLLM versions if you rely on `/v1/messages/count_tokens` or MCP auto-execution.
- Non-admin proxy users may see blank model lists after recent upgrades. Admins should verify dashboard behavior for viewer/internal-user roles before rolling out broadly.
- Provider-specific regressions are active across Mistral tool continuations, Bedrock geo inference profiles, `openai/` self-hosted multimodal routing, OpenRouter streaming model attribution, and PDF token counting. Add targeted integration tests if you use these paths.
- Streaming fallback health needs monitoring: a mid-stream 429 can be attributed to the fallback deployment and cool it down incorrectly.
- Telemetry and cost-accounting PRs are worth tracking for enterprise/air-gapped deployments. Spend logs for vLLM passthrough, MCP auto-execute, and Bedrock cross-region pricing are being corrected, so re-verify cost dashboards after upgrade.
- Security hygiene: raw DB errors in management endpoints and the `gunicorn <24` advisory are open items. Self-hosted operators should consider immediate mitigation or upgrade planning.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-10-14

**Repo:** [unslothai/unsloth](https://github.com/unslothai/unsloth) · Activity window: last 24h · 147 issues / 252 PRs touched

---

## 1. Today's Highlights

Unsloth shipped **v0.1.907-beta**, a performance-focused release claiming up to **10x faster multi-chat UI/UX** and up to **5x faster inference** backed by a reported **95% cache hit rate** plus MoE "Strata"-style fixes and general slowdown fixes. In parallel, the Studio track is consolidating around **DGX Spark two-node clustering, video/DiT LoRA training (Wan2.2, LTX-2.5), and MoE cache-placement correctness** ([PR #13674](https://github.com/unslothai/unsloth/pull/13674), [PR #10323](https://github.com/unslothai/unsloth/pull/10323), [PR #10280](https://github.com/unslothai/unsloth/pull/10280)). A large batch of older bug reports (multi-GPU OOM, batch TTS, Windows Gemma4e2b, multi-LAN-user model sync) was closed on 2026-10-13, which may reflect triage rather than verified fixes — confirm against your version before relying on them.

---

## 2. Releases & Breaking Changes

- **v0.1.907-beta — "Large Performance Gains + More Browser Use"** — major perf pass across UI/UX and inference; better compaction semantics that now work for *all* APIs and improved UI handling. No explicit API/config migration notes were published with the release. Notes: [releases](https://github.com/unslothai/unsloth/releases)
- **Behavioral change in Studio defaults (in review):** LTX-2.5 now defaults to the hosted **INT8-ConvRot DiT** even when bf16 fits in VRAM, matching ComfyUI templates; int8 text encoder is opt-in ([PR #13618](https://github.com/unslothai/unsloth/pull/13618)). If you pin Studio video defaults, review before upgrading.
- **Threading change (in review):** sd.cpp on macOS will stop assuming SMT and use all performance cores instead of `cpu_count() // 2` ([PR #13349](https://github.com/unslothai/unsloth/pull/13349)) — affects CPU-side image gen throughput on Apple Silicon and CI runners.

---

## 3. New Model & Hardware Support

| Item | Status | Link |
|---|---|---|
| **Wan2.2-TI2V-5B LoRA training** in Studio DiT trainer (first video family marked trainable; A14B two-expert follow-up planned) | PR open | [#13654](https://github.com/unslothai/unsloth/pull/13654) |
| **LTX-2.5** Studio defaults aligned with ComfyUI (hosted INT8-ConvRot DiT, opt-in int8 text encoder) | PR open | [#13618](https://github.com/unslothai/unsloth/pull/13618) |
| **DGX Spark / GB10 (aarch64, sm_121a, CUDA 13)** — single-node support plus two-node clustering over ConnectX-7 200GbE, pipeline training, link diagnostics | PR open | [#10280](https://github.com/unslothai/unsloth/pull/10280) |
| **Windows NVIDIA drivers older than CUDA 12** — fall back to Vulkan or CPU llama.cpp bundle instead of a doomed source build/rollback | PR open | [#13671](https://github.com/unslothai/unsloth/pull/13671) |
| **H100 (sm_90) + xformers** — skip FA3 when the build lacks sm_90a kernels | PR open | [#13619](https://github.com/unslothai/unsloth/pull/13619) |
| Requested, not landed: Voxtral (👍14) | Issue open | [#3013](https://github.com/unslothai/unsloth/issues/3013) |
| Requested: Ling 3.0 in Studio (👍5) | Issue closed | [#8532](https://github.com/unslothai/unsloth/issues/8532) |
| Requested: NPU backend (👍8) | Issue closed | [#1794](https://github.com/unslothai/unsloth/issues/1794) |
| Requested: Flatpak packaging for Studio (👍30 — highest-signal platform ask) | Issue open | [#4380](https://github.com/unslothai/unsloth/issues/4380) |
| Requested: sample packing for processor-based (multimodal) models — currently text-only LLMs | Issue open | [#4120](https://github.com/unslothai/unsloth/issues/4120) |
| Requested: multi-image Clef/Jev decision-model train+predict on PyTorch/CUDA | Issue closed | [#13461](https://github.com/unslothai/unsloth/issues/13461) |
| AMD RX 5700XT not detected in Unsloth Desktop | Issue closed | [#8529](https://github.com/unslothai/unsloth/issues/8529) |

---

## 4. Performance & Optimization

- **Release-level:** up to **10x** faster UI/UX with multiple concurrent chats; up to **5x** faster inference with a **95% cache hit rate**; MoE "Strata"-style fixes; compaction available across all APIs. [releases](https://github.com/unslothai/unsloth/releases)
- **MoE cache placement guards completed + spill micro-batch raise restored** — five Codex findings from #13600 that were dropped by a squash-merge are cherry-picked back; directly relevant to MoE serving memory behavior and spill handling. [PR #13674](https://github.com/unslothai/unsloth/pull/13674)
- **Two-Spark serving orchestrator with async replica router** — picks one of three topologies from `spark_cluster.recommend_topology` when loading a GGUF on paired DGX Sparks, keeping both nodes busy when the workload allows and never blocking the event loop. [PR #10323](https://github.com/unslothai/unsloth/pull/10323)
- **H100 FA3 skip** avoids the `wgmma.fence without CUTE_ARCH_MMA_SM90A_ENABLED` path that currently dies indirectly on a cuBLAS call. [PR #13619](https://github.com/unslothai/unsloth/pull/13619)
- **Apple Silicon sd.cpp threading** — currently uses `os.cpu_count() // 2`; on a 3-core macOS-15 runner that is 1 thread. Fix restores full performance-core usage. [PR #13349](https://github.com/unslothai/unsloth/pull/13349)
- **Long-context chat lag** (`#12552`) and **multi-chat slowdowns** were the user-visible drivers behind this release; the issue is closed. [Issue #12552](https://github.com/unslothai/unsloth/issues/12552)

---

## 5. Stability & Regressions

Ranked roughly by severity/blast radius:

**High**
- **H100 training crash with conda-forge xformers** — crashes on first step, log floods with `Attempting to use wgmma.fence without CUTE_ARCH_MMA_SM90A_ENABLED`, then dies on an unrelated cuBLAS call. **Fix PR exists but is open/unmerged** → [#13619](https://github.com/unslothai/unsloth/pull/13619), upstream [#3963](https://github.com/unslothai/unsloth/issues/3963).
- **GRPO + LoRA on Qwen3-MoE with `fast_inference=True`** — `AttributeError: 'Qwen3MoeForCausalLM' object has no attribute 'vllm_engine'`; falls back to very slow non-vLLM path. Closed without a linked fix in this window → [#2741](https://github.com/unslothai/unsloth/issues/2741).
- **Multi-GPU training OOM** on a 6-GPU cloud run (closed as inactive — treat as unresolved) → [#4685](https://github.com/unslothai/unsloth/issues/4685).
- **Mistral Small 3.1/3.2 SFT produces extremely high loss** with SFTTrainer + PEFT → [#2786](https://github.com/unslothai/unsloth/issues/2786).

**Medium**
- **Multi-user LAN/local serving desync** — model does not stay loaded consistently across accounts (2x RTX PRO 6000 Blackwell 96GB reported). Closed → [#12365](https://github.com/unslothai/unsloth/issues/12365).
- **Gemma4e2b unusable on Windows** → [#13388](https://github.com/unslothai/unsloth/issues/13388).
- **Batch inference with mixed text+image** (`Received inconsistently sized batches of images (1) and text (4)`, Gemma 3 4B) → [#3094](https://github.com/unslothai/unsloth/issues/3094).
- **Windows Desktop install rollback** on GT 650M / driver reporting CUDA 10.1 — `winget` OpenSSL dev step reached after prebuilt fallback. **Fix PR open** → [#13671](https://github.com/unslothai/unsloth/pull/13671).
- **Claude CLI session poisoned after pasting an image into a text-only GGUF** — the rejected image stays in resent history so every subsequent prompt fails with the same vision error. **Fix PR open** → [#13652](https://github.com/unslothai/unsloth/pull/13652).
- **Random Seed ignored for LoRA init** on NVIDIA/AMD/Intel — only data order changed; two runs with seeds 42 and 7 started from the same adapter weights (seed 3407). Apple Silicon already correct. **Fix PR open** → [#13637](https://github.com/unslothai/unsloth/pull/13637).

**Low / cosmetic & CI**
- Orpheus TTS invalid dim with batch size > 1 — [fixed, pending confirmation] → [#3123](https://github.com/unslothai/unsloth/issues/3123).
- Model load failure on RTX 4080 → [#4626](https://github.com/unslothai/unsloth/issues/4626).
- Inline math written as `$3$` renders as broken pricing text → **fix PR open** [#13649](https://github.com/unslothai/unsloth/pull/13649).
- Thinking-trace chevron direction and whole-trace restyle → [#13669](https://github.com/unslothai/unsloth/pull/13669), [#13665](https://github.com/unslothai/unsloth/pull/13665).
- Hint tooltip fires on Recipe dialog open → [#13668](https://github.com/unslothai/unsloth/pull/13668).
- Flaky `Chat UI Tests` on `studio-windows-ui-smoke.yml` (wait on composer only) → [#13672](https://github.com/unslothai/unsloth/pull/13672); main was red at `probe_dismiss_guard.py`, bisected to the inline-message-actions change, fixed and closed → [#13670](https://github.com/unslothai/unsloth/pull/13670).
- **Security:** Deep Research query scrubber leaked the credential when an `Authorization` header was present — the label rule matched the scheme (`Bearer`/`Basic`/`token`) and redacted the wrong token. **Fix PR open** → [#13499](https://github.com/unslothai/unsloth/pull/13499).

---

## 6. What This Means for Application Developers

- **Long-context agent loops get cheaper.** The 95% cache-hit / up-to-5x inference claims plus "better compaction for all APIs" target exactly the multi-turn, tool-calling pattern where KV reuse dominates cost. Benchmark v0.1.907-beta against your pinned version on your own prompt mix before adopting.
- **MoE serving is still the sharp edge.** Cache placement, spill micro-batching, and vLLM integration for MoE (`#2741`, `#13674`) are the active fault lines. If you serve Qwen3-MoE or similar behind a gateway, pin versions and validate `fast_inference` paths.
- **Reproducibility on non-Apple hardware is currently broken for LoRA seeds** until [#13637](https://github.com/unslothai/unsloth/pull/13637) lands. Do not compare fine-tune runs by seed on NVIDIA/AMD/Intel today.
- **Studio is becoming a serving plane, not just a UI.** Scheduled Routines workflows ([#13539](https://github.com/unslothai/unsloth/pull/13539)), two-Spark async replica routing ([#10323](https://github.com/unslothai/unsloth/pull/10323)), one-click MCP installs ([#10822](https://github.com/unslothai/unsloth/issues/10822)), and Ollama-compatible chat config export ([#4660](https://github.com/unslothai/unsloth/issues/4660)) point at self-hosted inference behind an OpenAI/vLLM-compatible surface — worth evaluating if you run edge inference.
- **Browser panel + remote access got functional, not just cosmetic.** Pages can now call their own site's API ([#13664](https://github.com/unslothai/unsloth/pull/13664)), which is what makes "Browser Use" agents actually work — review the origin/permission model before enabling remote access.
- **Serving fine-tuned LoRA artifacts via vLLM / AWQ remains an integration pain point** ([#464](https://github.com/unslothai/unsloth/issues/464), 22 comments). Plan an explicit export/quantize step rather than assuming drop-in compatibility.

> **Caveat:** issue status reflects GitHub state on 2026-10-13/14. Several "CLOSED" items have no linked fix commit in this window (some marked `inactive`), and PR summaries are author-written and unmerged unless noted. Verify against `main` before making upgrade decisions.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*