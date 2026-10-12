# AI Infrastructure Digest 2026-10-12

> Generated: 2026-10-12 08:27 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross-Project AI Infrastructure Report — 2026-10-12

## 1. Ecosystem Overview

The AI infrastructure ecosystem is in a **hardening phase**, not a release-heavy one: four of six projects shipped no new release, and attention is dominated by correctness regressions, hardware bring-up, and agent-facing protocol gaps. Datacenter serving engines are racing on **MoE, MLA/attention kernels, quantized KV cache, and non-NVIDIA backends** while fighting silent corruption in prefix caching and speculative decoding. Local runtimes are broadening backend coverage and tool-calling, but multi-tenant cache isolation and Apple MLX/CUDA/Vulkan stability remain weak spots. Gateways and training stacks are moving up the stack: LiteLLM is focused on MCP auth, protocol translation, and spend accuracy; Unsloth on silent training failures, KV preemption, and making Studio agent-driveable. Across layers, the dominant workload shapes are **MoE, quantization, long-context/prefix reuse, and tool-using agents**.

---

## 2. Activity Comparison

Digest-reported 24h activity. Only vLLM and LiteLLM provided aggregate counts; other digests list individual issues/PRs but no totals.

| Project | Issues updated (24h) | PRs updated (24h) | Release status |
|---|---:|---:|---|
| **vLLM** | 86 | 464 | No new releases. 0.30.0 has a prefix-caching regression for DFlash2/DSpark; 0.29.x is last known-good for that config. |
| **SGLang** | Not reported | Not reported | No new releases. Intel XPU migration to PyTorch 2.14 in progress. |
| **llama.cpp** | Not reported | Not reported | Rolling builds b11554–b11564. **b11563 changes GGML symbol visibility to hidden** — breaking for some embedders. |
| **Ollama** | Not reported | Not reported | No releases. PR #18917 closed, bumping llama.cpp to b11521 and adding decision models + EmbeddingGemma 2. |
| **LiteLLM** | 74 | 226 | **v1.106.0-rc.1, v1.105.0, v1.104.3** released. Docker images cosign-signed. |
| **Unsloth** | Not reported | Not reported | No new releases. New Linux aarch64 cu130/Python 3.13 Studio installs move to torch 2.13. |

**Activity takeaway:** vLLM has the highest raw PR throughput by a wide margin; LiteLLM is second and is the only project with tagged releases today. SGLang, llama.cpp, Ollama, and Unsloth show dense issue/PR activity but did not publish totals.

---

## 3. Model Support Race

| Project | New models / architectures in window | Hardware / backend | Position |
|---|---|---|---|
| **vLLM** | Kimi-K3 on gfx942; MLA on Intel XPU and ROCm FP8 prefill; FlashInfer 0.7.1; embedding LoRA for BERT/RoBERTa/XLMRoberta | Intel XPU, ROCm MI355X/gfx950/gfx942, CUDA/Rubin | **Datacenter serving leader** on accelerator breadth and MLA/MoE bring-up |
| **SGLang** | Qwen-Image 2.1, MiniMax H3/FastH3/VDN, LTX-2.5/2.3; DeepSeek-V4.1-Flash SM120 field report; Qwen3.8 512-expert MoE ROCm; DSV4 NPU | Blackwell/SM120, NPU, ROCm MI355X, Intel XPU | **Strong datacenter + multimodal/diffusion**; broad but with open SM120/AMD crashes |
| **llama.cpp** | Prism Bonsai 2 27B; Gemma-4 E4B/E2B OpenCL; Xing4.0-29B-A4B in review; Hexagon IM2COL_3D; KDA hybrid tensor-split WIP; SYCL Q8_0 MoE | OpenCL, Hexagon, SYCL, Vulkan, Metal, CUDA, CPU | **Local/edge breadth leader**; experimental backends move fastest here |
| **Ollama** | llama.cpp b11521 decision models via `/v1/systemone`; EmbeddingGemma 2; d1-3B request; MLX regressions | MLX, CUDA, Vulkan | **Packaged local UX leader**, but dependent on llama.cpp and currently unstable on MLX |
| **LiteLLM** | RodiumAi provider, Wingback guardrail, virtual models; no model kernels | Provider/gateway layer | **Gateway/provider abstraction leader**, not a serving engine |
| **Unsloth** | Z-Image MLX image, Wan2.1 video, aarch64 cu130, RAG embedding GPU offload, Intel GPU request | MLX, CUDA, Intel | **Fine-tuning/Studio leader**, expanding into image/video and agent-driveable workflows |

**Who is ahead:** vLLM and SGLang lead datacenter model/hardware support. llama.cpp leads local/edge architecture breadth. Unsloth leads fine-tuning and Studio multimodal support. LiteLLM leads provider/routing integration. Ollama leads out-of-the-box local UX but lags on backend stability.

---

## 4. Performance Frontier

Optimization is concentrated in seven areas:

- **KV cache and prefix reuse:** vLLM has severe prefix-caching corruption on hybrid/state-space and speculative paths; SGLang reports DeepSeek-V4.1-Flash multi-turn prefix cache degrading to a constant ~11.5K-token hit; llama.cpp has prompt-cache contamination under concurrency; Unsloth is adding KV preemption for parallel chats.
- **Speculative decoding:** vLLM’s DFlash2/DSpark + prefix caching corrupts output; SGLang’s DFLASH/DSPARK have repetition, OOM, and first-non-greedy-request crashes; llama.cpp shows greedy-output divergence on quantized targets.
- **MoE and routing:** vLLM extends fused shared experts and AITER-tuned MoE router gates; SGLang optimizes 512-expert softmax MoE via aiter `topk_gating`; llama.cpp fixes duplicate expert IDs on Vulkan/Metal; Unsloth flags MoE-LoRA checkpoint layout risk.
- **Attention/MLA and quantized KV:** vLLM routes XPU MLA through FlashAttn MLA and adds ROCm FP8 MLA prefill; SGLang works on QSA context parallelism and fp8 KV cache; llama.cpp improves OpenCL flash attention.
- **Distributed serving and batching:** vLLM adds DeepSeek-V4 ROCm dual-batch overlap and an RFC for adaptive prefill token budget; SGLang pushes prefill context parallelism, PD disaggregation, and Mooncake decode; llama.cpp’s RPC backend crashes on large MoE sharding.
- **Quantization:** vLLM is FP8/NVFP4-heavy with an open INT8 KV request; SGLang had fp8 KV + QSA crash; llama.cpp optimizes CM2, Q4/Q8, and SYCL Q4_0; Unsloth focuses QLoRA/MLX repacking.
- **Kernels and backends:** vLLM uses AITER, FlashInfer, Triton; SGLang uses FlashInfer autotune, aiter, Triton; llama.cpp pushes Vulkan/OpenCL/SYCL/Hexagon; Ollama fights MLX/CUDA/Vulkan; Unsloth removes bitsandbytes synchronization stalls.

**Frontier takeaway:** The fastest gains are no longer only “more FLOPS.” They come from **paged/prefix cache correctness, MoE routing, quantized attention, and backend-specific kernels** — and regressions in those areas can silently break output quality.

---

## 5. Layer Positioning

| Layer | Projects | Role | Current focus |
|---|---|---|---|
| **Serving engines** | **vLLM, SGLang** | High-throughput GPU server; paged attention, continuous batching, speculative decoding, distributed serving, multi-hardware | vLLM: ROCm/XPU/CUDA, MLA, MoE, prefix-cache correctness. SGLang: PD disaggregation, context parallelism, FlashInfer autotune, diffusion/ComfyUI. |
| **Local runtimes** | **llama.cpp, Ollama** | Run models locally/edge; GGUF/GGML, broad backends, simple UX | llama.cpp: ABI visibility, Vulkan/OpenCL/SYCL/Metal correctness, spec-decode reliability. Ollama: tool-calling, MLX/CUDA/Vulkan stability, packaging. |
| **Gateway / router** | **LiteLLM** | Provider abstraction, auth, spend tracking, routing/failover, guardrails, MCP | MCP OAuth/tool catalog, Anthropic↔OpenAI translation, Gemini billing, virtual models. |
| **Training / fine-tuning** | **Unsloth** | Efficient LoRA/QLoRA, local Studio, fine-tune + inference workflows | Silent gradient bugs, MoE-LoRA exports, KV preemption, optimizer sync removal, MCP-driven Studio. |

**Overlaps:** vLLM and SGLang can sit behind LiteLLM as backends. Ollama bundles llama.cpp and adds registry/API UX. Unsloth can train models and serve them locally through Studio, overlapping with local runtimes.

---

## 6. Trend Signals

1. **Correctness is the new performance bottleneck.** Prefix caching and speculative decoding are not safe defaults across hybrid, state-space, and MoE models. Watch vLLM #60174/#53912, SGLang #40843/#41076/#43412, and llama.cpp #25618/#27148.
2. **Hardware fragmentation is accelerating.** ROCm MI355X/gfx942, Intel XPU, SM120/Blackwell, Apple MLX, NPU, Vulkan/OpenCL/SYCL/Hexagon — the deployment matrix now materially affects both correctness and throughput.
3. **MoE is the default high-end workload.** Routing, duplicate expert IDs, fused experts, MoE LoRA, and quantized MoE appear across vLLM, SGLang, llama.cpp, and Unsloth.
4. **Quantization is moving into KV cache and attention.** FP8, FP4/NVFP4, INT8 requests, and backend-specific KV behavior are now first-order deployment decisions.
5. **Agent tooling is production-critical and still brittle.** Watch Ollama’s Qwen 3.8 streaming 500 and `format: "json"` tool-call failures, vLLM’s MiniMax-M3 streaming tool-call edge case, and LiteLLM’s MCP OAuth/tool-catalog risks.
6. **Gateway accuracy matters as much as gateway uptime.** LiteLLM’s Gemini tool-use billing gap, `no-log` spend skip, and Anthropic↔OpenAI translation gaps can distort tenant budgets or break Claude Code-style clients.
7. **Local multi-tenant serving needs isolation defaults.** llama.cpp’s RAM-backed prompt cache can contaminate conversations under concurrency; Ollama’s `keep_alive` overflow and Windows CUDA breakage are operational traps.
8. **Fine-tuning still has silent failure modes.** Unsloth’s Gemma 4 fused-loss zero-gradient bug and MoE-LoRA checkpoint-layout hazard mean teams must validate gradients and exports, not just loss curves.

**What agent/application developers should watch now:**
- Pin serving-engine versions if using prefix caching, speculative decoding, or hybrid/MoE models.
- Test tool calls in both streaming and non-streaming modes; avoid `format: "json"` with tool calls on Ollama until #18944 is fixed.
- Treat MCP OAuth refresh and tool-catalog pinning as production concerns in LiteLLM.
- Do not baseline LiteLLM capacity from OpenAI-compatible `timings.prompt_per_second` on Ollama — it is currently inflated.
- Verify fine-tuning gradients and checkpoint converters before shipping MoE or Gemma 4 adapters.
- For local deployments, audit prompt-cache isolation and backend stability before enabling multi-tenant concurrency.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-10-12

*Source: github.com/vllm-project/vllm · 86 issues and 464 PRs updated in the last 24h · No new releases*

---

## 1. Today's Highlights

No release landed in the last 24 hours, so attention stays on correctness and platform bring-up. The dominant theme is **prefix-caching corruption on hybrid/state-space models and speculative-decode paths**: multiple independent reports ([#60174](https://github.com/vllm-project/vllm/issues/60174), [#53912](https://github.com/vllm-project/vllm/issues/53912), [#57267](https://github.com/vllm-project/vllm/issues/57267), [#57032](https://github.com/vllm-project/vllm/issues/57032)) describe silent output corruption after cache hits, with a fix PR in flight for the GDN/MTP case ([#52244](https://github.com/vllm-project/vllm/pull/52244)). In parallel, ROCm momentum is strong: new performance trackers for Qwen3.8-2.4T-A95B and Qwen3.8-Flash-Next on MI355X, plus Kimi-K3 mono-decode being ported from gfx950 to gfx942 ([#60997](https://github.com/vllm-project/vllm/pull/60997)).

---

## 2. Releases & Breaking Changes

None. No new tags or release artifacts were published in the last 24h, and no API/config-format changes were announced today.

Note for anyone tracking versions: [#60174](https://github.com/vllm-project/vllm/issues/60174) reports a **behavioral regression introduced in 0.30.0** (prefix caching on DFlash2/DSpark drafters), which makes 0.29.x the last known-good line for that configuration.

---

## 3. New Model & Hardware Support

- **Intel XPU — MLA support**: PR [#58297](https://github.com/vllm-project/vllm/pull/58297) routes XPU MLA through the existing `FLASH_ATTN_MLA` backend using `vllm-xpu-kernels` varlen flash attention, packing `q = [q_nope, q_pe]` against the latent cache.
- **ROCm — MLA FP8 prefill (opt-in)**: PR [#59124](https://github.com/vllm-project/vllm/pull/59124) adds `ROCM_AITER_FLYDSL_FP8`, a FlyDSL FP8 MLA prefill backend targeting Kimi-K3 on MI355X (gfx950).
- **ROCm — Kimi-K3 on gfx942**: PR [#60997](https://github.com/vllm-project/vllm/pull/60997) ports the gfx950-only mono decode kernel to gfx942 (256 CTAs × 512 threads, arch now part of the build key).
- **FlashInfer 0.7.1 upgrade**: PR [#60948](https://github.com/vllm-project/vllm/pull/60948) bumps `flashinfer-python` from `0.7.0.post1` to `0.7.1` for CUDA and Rubin builds, explicitly selecting the CuTe DSL KDA prefill backend to avoid an inference-tensor crash seen in B200 Kimi-K3 CI.
- **Embedding LoRA**: PR [#61033](https://github.com/vllm-project/vllm/pull/61033) enables LoRA adapters for `BertModel`, `RobertaModel`, and `XLMRobertaModel` (BGE, all-MiniLM, multilingual-e5 families).
- **Feature request (open)**: INT8 KV-cache quantization is still FP8-only; request tracked in [#33480](https://github.com/vllm-project/vllm/issues/33480) (22 comments, 12 👍).

---

## 4. Performance & Optimization

**Landed / in-review PRs**
- DeepSeek-V4 ROCm: prefill compute/comm dual-batch-overlap (DBO) for data-parallel deployments with flattened expert TP — [#57773](https://github.com/vllm-project/vllm/pull/57773).
- ROCm: skip the `torch.full(..., -inf)` FP32 fill on the sparse prefill MQA-logits buffer via `clean_logits=False` — [#51314](https://github.com/vllm-project/vllm/pull/51314).
- ROCm: use AITER tuned GEMM for the MoE router gate (previously Hopper/Blackwell-gated) — [#50535](https://github.com/vllm-project/vllm/pull/50535).
- ROCm: enable the fused QK-norm + RoPE + gate Triton kernel for Qwen3-Next/Qwen3.5 — [#51406](https://github.com/vllm-project/vllm/pull/51406).
- ROCm: AITER fused shared experts for GLM-5.3-Flash extended to gfx942 (MI300X/MI325X) at TP2/4/8 — [#60604](https://github.com/vllm-project/vllm/pull/60604).

**Reported regressions / outstanding performance gaps**
- **SM120 + DeepSeek-V4.1-Flash** (8× RTX PRO 6000): extremely low decode throughput under `--enforce-eager` and unusable CUDA graphs; maintainers asked to prioritize SM120 graph capture — [#56892](https://github.com/vllm-project/vllm/issues/56892).
- **NVFP4 KV cache on SM120**: causal paged prefill measured at ~145.8 ms vs. FP8, i.e. ~1.7–1.8× slower — [#50416](https://github.com/vllm-project/vllm/issues/50416).
- **RDNA4 (gfx1201)**: `RowWiseTorchFP8ScaledMMLinearKernel` is selected since v0.28 and costs 5–24% decode — [#57838](https://github.com/vllm-project/vllm/issues/57838).
- **Pooling throughput**: ~18% regression since torch 2.11 (vLLM v0.20.0+), still open — [#52630](https://github.com/vllm-project/vllm/issues/52630).
- **RDNA hybrid-Mamba**: falls back to Triton paged attention and decode collapses at long context; fix identified as a split-KV decode kernel gated by `on_gfx12x()` — [#50264](https://github.com/vllm-project/vllm/issues/50264).
- **RFC — adaptive prefill token budget**: proposes making `max_num_batched_tokens` pressure-responsive instead of static for long-context workloads — [#52906](https://github.com/vllm-project/vllm/issues/52906).

---

## 5. Stability & Regressions

Ranked by severity:

1. **Silent output corruption, DFlash2/DSpark + prefix caching** (0.30.0/0.31.0, Qwen3.8-27B, TP=1) — reproducible on cache hit; 0.29.0 passes and MTP is unaffected. No fix PR linked yet. [#60174](https://github.com/vllm-project/vllm/issues/60174)
2. **Hybrid Mamba/GDN + prefix caching + MTP still corrupts output** — 24 comments; prior issue #43559 was closed but unfixed. Fix in review: PR [#52244](https://github.com/vllm-project/vllm/pull/52244) restores GDN prefix-cache hits under MTP (Qwen3.5-122B-A10B). [#53912](https://github.com/vllm-project/vllm/issues/53912)
3. **CUDA graph deadlock, fully-sharded fused-MoE LoRA across TP ranks** (0.28.0, TP=2/4, 35B-A3B MoE + multiple adapters) — hangs under concurrent traffic. [#60845](https://github.com/vllm-project/vllm/issues/60845)
4. **ROCm/gfx942 engine stall inside AITER fused MoE** (GLM-5.3, TP8, v0.29.0) — generation goes from >2,000 tok/s to zero with no error, KV cache ~20% full. [#57429](https://github.com/vllm-project/vllm/issues/57429)
5. **Hybrid Mamba2 non-determinism** under `mamba_cache_mode="all"` + prefix caching — identical batch inputs produce different outputs. [#57267](https://github.com/vllm-project/vllm/issues/57267)
6. **Gemma-4 MTP speculative decoding crash during CUDA graph capture** — `_suppress_token_ids` is a Python list, not a device tensor. [#48503](https://github.com/vllm-project/vllm/issues/48503)
7. **Gemma-4 MoE initialization hang and segfault** (0.20.x, 2× RTX 3090, PCIe, no NVLink). [#42390](https://github.com/vllm-project/vllm/issues/42390)
8. **Sparse MLA indexer allocates `expanded_block_table_buffer` after KV sizing** — ~1 GiB at 1M context (B200, Model Runner V2, DSA models). [#60450](https://github.com/vllm-project/vllm/issues/60450)
9. **MiniMax-M3 (MXFP8) streaming tool calls**: empty delta + `finish_reason=stop` right after reasoning closes; identical non-streaming payload succeeds. [#52613](https://github.com/vllm-project/vllm/issues/52613)
10. **Rust frontend parity and routing fixes**: `qwen3_xml` tool parsing now matches the Python parser (PR [#60136](https://github.com/vllm-project/vllm/pull/60136)), and DP routing is corrected after request removal/abort (PR [#59631](https://github.com/vllm-project/vllm/pull/59631)).

Closed today: gpt-oss 20B tool-call tokenizer bug (`Unexpected token 12606 while expecting start token 200006`) — [#22519](https://github.com/vllm-project/vllm/issues/22519), closed after 39 comments and 50 👍.

---

## 6. What This Means for Application Developers

- **Audit prefix caching before upgrading.** If you serve hybrid Mamba/GDN, Qwen3.8, or any model with DFlash/DSpark/MTP speculative decoding, prefix caching is currently a correctness risk, not just a perf knob. On 0.30/0.31 the safe options are: pin 0.29.x, disable prefix caching, or disable the speculative path until [#60174](https://github.com/vllm-project/vllm/issues/60174) and [#53912](https://github.com/vllm-project/vllm/issues/53912) are resolved.
- **Tool-calling over streaming still has edge cases.** The MiniMax-M3 streaming tool-call failure ([#52613](https://github.com/vllm-project/vllm/issues/52613)) argues for a non-streaming fallback path in agent loops, or validating `finish_reason` against expected tool-call output. The long-standing gpt-oss 20B tool-call tokenizer bug is now closed ([#22519](https://github.com/vllm-project/vllm/issues/22519)).
- **Rust frontend adopters should wait for parity PRs.** `VLLM_USE_RUST_FRONTEND=1` with `--tool-call-parser qwen3_xml` produces broken output until [#60136](https://github.com/vllm-project/vllm/pull/60136) lands; per-request metrics (`--enable-per-request-metrics`) are being added in [#59314](https://github.com/vllm-project/vllm/pull/59314), useful for SLO tracking on the gateway side.
- **LoRA deployments get a real correctness fix.** PEFT `rank_pattern` / `alpha_pattern` were silently ignored, meaning per-module adapters were served with the wrong scaling — see PR [#59801](https://github.com/vllm-project/vllm/pull/59801). If you ship multi-tenant adapters with per-module ranks, this was affecting output quality, not just metrics.
- **Hardware selection matters more than usual this cycle.** NVFP4 KV cache on SM120 is ~1.7–1.8× slower than FP8 for prefill ([#50416](https://github.com/vllm-project/vllm/issues/50416)), and DeepSeek-V4.1 on SM120 effectively requires CUDA graphs for usable decode ([#56892](https://github.com/vllm-project/vllm/issues/56892)). For AMD, the MI355X Qwen3.8 trackers ([#57149](https://github.com/vllm-project/vllm/issues/57149), [#59575](https://github.com/vllm-project/vllm/issues/59575)) signal that MXFP4 paths are actively being tuned — expect meaningful gains to land as stacked PRs.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-10-12

## 1. Today's Highlights
No new SGLang release landed today, but activity is concentrated on SM120/Blackwell hardening, speculative-decoding correctness, and Q4 roadmaps. The PD disaggregation Q4 roadmap ([#43699](https://github.com/sgl-project/sglang/issues/43699)) and DeepSeek V4.1 optimization roadmap ([#42170](https://github.com/sgl-project/sglang/issues/42170)) were updated, while performance work targets FlashInfer prefill autotuning ([#42736](https://github.com/sgl-project/sglang/pull/42736)), ROCm MoE routing ([#40708](https://github.com/sgl-project/sglang/pull/40708)), and fused NaN validation ([#43720](https://github.com/sgl-project/sglang/pull/43720)). Several high-severity issues remain open around DFLASH/DSPARK speculative decoding, fp8 KV cache with QSA attention, and mamba cache allocation.

## 2. Releases & Breaking Changes
- **No new releases** in the last 24h.
- No documented breaking API/config changes.
- Dependency migration in progress: Intel XPU backend upgraded to PyTorch 2.14 ([PR #42003](https://github.com/sgl-project/sglang/pull/42003)).

## 3. New Model & Hardware Support
- **Diffusion/ComfyUI**: Qwen-Image 2.1 in SGLD integrated mode ([PR #43581](https://github.com/sgl-project/sglang/pull/43581)); MiniMax H3, FastH3, and VDN ([PR #43504](https://github.com/sgl-project/sglang/pull/43504)); LTX-2.5 / LTX-2.3 ([PR #43583](https://github.com/sgl-project/sglang/pull/43583)).
- **Blackwell/SM120**: production field report for DeepSeek-V4.1-Flash on 8× RTX PRO 6000 SM120 PCIe ([Issue #40877](https://github.com/sgl-project/sglang/issues/40877)); SM120 optimization plan closed/inactive ([Issue #19637](https://github.com/sgl-project/sglang/issues/19637)); GLM-5.3-Flash SM120 fa4 backend crash remains open ([Issue #42012](https://github.com/sgl-project/sglang/issues/42012)).
- **NPU**: DSV4 A5 compressor routed to explicit SWA-mapped state table ([PR #40816](https://github.com/sgl-project/sglang/pull/40816)); HiCache tensor-bool ambiguity fix for NPU ([PR #43308](https://github.com/sgl-project/sglang/pull/43308)).
- **ROCm**: Qwen3.8 512-expert MoE routing through aiter `topk_gating` ([PR #40708](https://github.com/sgl-project/sglang/pull/40708)); AMD DSV4 `compact_attention_hip` breaks because aiter removed `pa_decode_sparse` ([Issue #43765](https://github.com/sgl-project/sglang/issues/43765)).
- **Intel XPU**: PyTorch 2.14 upgrade ([PR #42003](https://github.com/sgl-project/sglang/pull/42003)).
- **Other**: Moore Threads MUSA support roadmap ([Issue #16565](https://github.com/sgl-project/sglang/issues/16565)); B12X FlashInfer NVFP4 MoE integration for SM120 closed/inactive ([Issue #33709](https://github.com/sgl-project/sglang/issues/33709)).

## 4. Performance & Optimization
- **FlashInfer prefill autotune by default** for all generation targets, including EAGLE/EAGLE3/STANDALONE drafts; replaces per-model hooks for DeepSeek-V4.1 and GLM-5.3-Flash ([PR #42736](https://github.com/sgl-project/sglang/pull/42736)).
- **ROCm 512-expert softmax MoE routing** via aiter `topk_gating`: +1.48% output tok/s without MTP, +2.22% with NEXTN MTP on 8× MI355X ([PR #40708](https://github.com/sgl-project/sglang/pull/40708)).
- **Fused async NaN validation** into one read-only Triton kernel ([PR #43720](https://github.com/sgl-project/sglang/pull/43720)).
- **TopK v2 threshold bucket overflow fix** to avoid silently selecting incorrect values ([PR #43801](https://github.com/sgl-project/sglang/pull/43801)).
- **Prefill Context Parallelism roadmap**: compatibility with allreduce fusion, CP size != MoE DP size, MLA models, and SWA; more MHA/GQA backends still pending ([Issue #21788](https://github.com/sgl-project/sglang/issues/21788)).
- **Mamba chunked prefill** kept on mamba chunk grid so DCP can donate checkpoints ([PR #41701](https://github.com/sgl-project/sglang/pull/41701)).
- **QSA Context Parallel optimization** ([PR #43792](https://github.com/sgl-project/sglang/pull/43792)); **Mooncake decode host receive** ([PR #43781](https://github.com/sgl-project/sglang/pull/43781)); **DSpark/Dflash graph-eager alignment and Domino candidate pool** ([PR #43790](https://github.com/sgl-project/sglang/pull/43790), [PR #43791](https://github.com/sgl-project/sglang/pull/43791)).

## 5. Stability & Regressions
Ranked by severity:

- **Critical correctness — DFLASH spec decoding**: GLM-5.3 with DFLASH speculative decoding produces severe repetition/degenerate loops in reasoning/output ([Issue #40843](https://github.com/sgl-project/sglang/issues/40843)). Related DFLASH verify-mask policy fix: [PR #43054](https://github.com/sgl-project/sglang/pull/43054).
- **Critical OOM/crash — DSPARK**: DeepSeek-V4.1-Flash + DSPARK unbounded `SparsePrefillWorkspace` allocation OOM-crashes the whole TP group ([Issue #41076](https://github.com/sgl-project/sglang/issues/41076)).
- **Critical crash — speculative decoding**: First non-greedy request crashes the server; FlashInfer verify-sampling JIT fails and kills the process tree ([Issue #43412](https://github.com/sgl-project/sglang/issues/43412)).
- **AMD breakage**: DSV4 `compact_attention_hip` imports aiter’s removed `pa_decode_sparse` module, breaking sparse decode on gfx950 ([Issue #43765](https://github.com/sgl-project/sglang/issues/43765)).
- **Closed today**: fp8 KV cache crashed QSA sparse attention kernels due to bf16 query × fp8 key `tl.dot` mismatch ([Issue #43741](https://github.com/sgl-project/sglang/issues/43741)).
- **Cache regression**: DeepSeek-V4.1-Flash multi-turn prefix cache degrades to a constant ~11.5K-token hit; 60–80K conversation prefixes are re-prefilled every turn ([Issue #43738](https://github.com/sgl-project/sglang/issues/43738)).
- **PD / memory**: Runtime role switch crashes with illegal memory access after decode → prefill → decode ([Issue #43557](https://github.com/sgl-project/sglang/issues/43557)); mamba cache allocation assertion kills the engine when all cached states are locked ([Issue #43204](https://github.com/sgl-project/sglang/issues/43204)).
- **SM120 attention**: GLM-5.3-Flash fa4 crashes at CUDA-graph capture; triton is the only working backend ([Issue #42012](https://github.com/sgl-project/sglang/issues/42012)).
- **Other open bugs**: unified_cache MAMBA eviction prunes nodes instead of downgrading, breaking host-tier loadback ([Issue #33713](https://github.com/sgl-project/sglang/issues/33713)); `--bf16-gemm-backend gemv` is ignored for unquantized linear layers ([Issue #42085](https://github.com/sgl-project/sglang/issues/42085)); `get_token_ids_logprobs` returns `[]` instead of a tensor and crashes decode logprob post-processing ([Issue #43154](https://github.com/sgl-project/sglang/issues/43154)); unbounded audio decode during multimodal preprocessing ([Issue #43708](https://github.com/sgl-project/sglang/issues/43708)). Fix PR for Qwen-VL video decode memory bound: [PR #43800](https://github.com/sgl-project/sglang/pull/43800).
- **Closed/inactive**: DP health-check routing perturbation ([Issue #35241](https://github.com/sgl-project/sglang/issues/35241)); DeepEP lazy init under CUDA graph ([Issue #29942](https://github.com/sgl-project/sglang/issues/29942)); 1M-token prefill OOM in DSV4 indexer ([Issue #34155](https://github.com/sgl-project/sglang/issues/34155)); DSpark CUDA launch failure for Kimi-K3 ([Issue #34522](https://github.com/sgl-project/sglang/issues/34522)); DSV4 DSpark topk=192 SM120 crash ([Issue #33943](https://github.com/sgl-project/sglang/issues/33943)); HIP/ROCm SWA prefix reuse RFC ([Issue #34562](https://github.com/sgl-project/sglang/issues/34562)); disable multimodal weights loading ([Issue #28194](https://github.com/sgl-project/sglang/issues/28194)).

## 6. What This Means for Application Developers
- **Speculative decoding is not safe by default across all models.** If you serve GLM-5.3 or DeepSeek-V4.1 with DFLASH/DSPARK/DSpark, gate non-greedy traffic and monitor for repetition, OOM, or illegal memory access ([Issue #40843](https://github.com/sgl-project/sglang/issues/40843), [Issue #41076](https://github.com/sgl-project/sglang/issues/41076), [Issue #43412](https://github.com/sgl-project/sglang/issues/43412)).
- **Avoid fp8 KV cache with QSA/Qwen4-Exp until validated.** The crashing issue is closed as of today, but test before production use ([Issue #43741](https://github.com/sgl-project/sglang/issues/43741)).
- **SM120/RTX PRO 6000 users should choose attention backends carefully.** DSV4.1-Flash has a working production config, but fa4 may crash; triton remains the fallback ([Issue #40877](https://github.com/sgl-project/sglang/issues/40877), [Issue #42012](https://github.com/sgl-project/sglang/issues/42012)).
- **AMD users must coordinate aiter versions.** Removed APIs break DSV4 sparse decode on gfx950 ([Issue #43765](https://github.com/sgl-project/sglang/issues/43765)).
- **Expect performance upside from FlashInfer autotune, ROCm MoE routing, and fused validation**, but validate on your workload before upgrading ([PR #42736](https://github.com/sgl-project/sglang/pull/42736), [PR #40708](https://github.com/sgl-project/sglang/pull/40708), [PR #43720](https://github.com/sgl-project/sglang/pull/43720)).
- **Diffusion/ComfyUI support is expanding** for Qwen-Image, MiniMax H3, and LTX-2.5/2.3; useful if you serve multimodal generation through SGLang ([PR #43581](https://github.com/sgl-project/sglang/pull/43581), [PR #43504](https://github.com/sgl-project/sglang/pull/43504), [PR #43583](https://github.com/sgl-project/sglang/pull/43583)).

</details>

<details>
<summary><strong>llama.cpp</strong> — <a href="https://github.com/ggml-org/llama.cpp">ggml-org/llama.cpp</a></summary>

# llama.cpp Digest — 2026-10-12

## 1. Today's Highlights

A heavy backend day: `ggml` now defaults to hidden symbol visibility (b11563), tightening the shared-library ABI surface, while MoE-related correctness fixes for duplicate expert IDs landed for Vulkan (b11554) with a matching Metal fix in review (#30373). OpenCL gained Gemma-4 decode and flash-attention improvements (b11559), and the first runtime support for Prism Bonsai 2 27B joined the tree (b11556). Speculative decoding remains the dominant source of correctness and performance complaints in the issue tracker.

## 2. Releases & Breaking Changes

- **b11563 — `ggml`: default symbol visibility changed to hidden** ([#30014](https://github.com/ggml-org/llama.cpp/pull/30014), [release](https://github.com/ggml-org/llama.cpp/releases/tag/b11563)). Internal symbols that previously leaked via `GGML_API` are no longer exported. **Migration note:** downstream projects that linked against non-public GGML symbols (directly or via `dlsym`) may need to build statically or upstream an explicit export. This is the most consequential change of the day for embedders.
- **b11557 — RPC: `GGML_RPC_DEBUG` becomes a verbosity level** ([#29544](https://github.com/ggml-org/llama.cpp/pull/29544)). Now parsed as an integer: `0`/unset disables, `1–3` emit increasingly detailed event/command/transport logs. Scripts that set `GGML_RPC_DEBUG=1` expecting a boolean still work, but log volume increases.
- **b11556 — Prism Bonsai 2 27B runtime support** ([#29600](https://github.com/ggml-org/llama.cpp/pull/29600)). Adds (and folds) Hadamard tensors; conversion tooling updated.
- Other builds in the window: b11564, b11562, b11560, b11559, b11558, b11555, b11554 — see below.

## 3. New Model & Hardware Support

- **Prism Bonsai 2 27B** — runtime support landed (b11556, [#29600](https://github.com/ggml-org/llama.cpp/pull/29600)).
- **Gemma-4 E4B/E2B GPU decode on OpenCL** — extended FA paths, including `dk512` for Gemma-4 and improved `dk64` ([b11559, #30266](https://github.com/ggml-org/llama.cpp/pull/30266)).
- **Xing4.0-29B-A4B (TeleAI)** — model support + conversion in review ([#29012](https://github.com/ggml-org/llama.cpp/pull/29012)).
- **Hexagon: `IM2COL_3D`** with DMA paths and reduced scalar fp16 conversion overhead ([b11560, #30305](https://github.com/ggml-org/llama.cpp/pull/30305)); Hexagon work for the Clef-Flash decision model (text + vision) in review ([#30143](https://github.com/ggml-org/llama.cpp/pull/30143)).
- **KDA hybrid models: tensor-split support** — WIP ([#30277](https://github.com/ggml-org/llama.cpp/pull/30277)).
- **SYCL: reordered Q8_0 MoE weights** — per-expert `[qs][d]` layout with reordered matvec dispatch ([#29337](https://github.com/ggml-org/llama.cpp/pull/29337)).
- **SYCL build flexibility** — backend can now be built with `ExternalProject` using icpx while the rest of the tree uses another compiler (e.g. ROCm) ([#29506](https://github.com/ggml-org/llama.cpp/pull/29506)).

## 4. Performance & Optimization

- **Vulkan CM2 quant block size** — ternary expressions avoid switch lowering in CM2 matmul; block size specialized ([b11564, #30164](https://github.com/ggml-org/llama.cpp/pull/30164)).
- **Vulkan f16 FMA packing** — scalar FMAs become `v_pk_fma_f16` (bit-identical ordering) where no dot-product op exists, targeting RADV packing ([#29877](https://github.com/ggml-org/llama.cpp/pull/29877)).
- **OpenCL flash-attention** — Gemma-4 GPU decode enabled for E4B and E2B ([b11559](https://github.com/ggml-org/llama.cpp/releases/tag/b11559)).
- **OpenCL kernel/image fixes** — fallback when Q4_K weight images exceed device limits; subgroup-safe `rms_norm`; Q4_0 broadcast fix ([b11558, #30310](https://github.com/ggml-org/llama.cpp/pull/30310)).
- **SYCL Q4_0 dequantize coalescing** — fixes a prompt-processing slowdown that appeared after the first decode once weights were reordered ([#30374](https://github.com/ggml-org/llama.cpp/pull/30374)).
- **CPU: AVX2 builds now use F16C when available** ([b11555, #30270](https://github.com/ggml-org/llama.cpp/pull/30270)).
- **Vulkan `mul_mat_id` duplicate IDs** — row-ID hoisting extended to always apply, computing every row of an expert when IDs repeat, removing a per-loop path ([b11554, #29998](https://github.com/ggml-org/llama.cpp/pull/29998)).
- **CI: HIP register-spill checks across all AMD architectures** ([#30348](https://github.com/ggml-org/llama.cpp/pull/30348)).
- **Regression watch:** Vulkan prompt processing remains **~45% slower on RDNA1 (gfx1010)** between b10455 and b11429 for a GDN-hybrid model, TG unaffected ([#30039](https://github.com/ggml-org/llama.cpp/issues/30039)). A previously reported **16–19% Q8_0 PP regression on RTX 5060 Ti with `NV_coopmat2`** since #25773 was closed ([#30000](https://github.com/ggml-org/llama.cpp/issues/30000)).

## 5. Stability & Regressions

Ranked by severity:

1. **RPC backend crash with large MoE models** — “Remote RPC server crashed or returned malformed response” (`ggml-rpc.cpp:566`) at warmup decode, 100% reproducible when sharding DeepSeek-R1-671B across 3× NVIDIA GB10 (DGX Spark) nodes. No fix PR linked yet ([#30052](https://github.com/ggml-org/llama.cpp/issues/30052)).
2. **CUDA graphs hang on RTX 5090 Laptop / sm_120** — RC watchdog + Xid 8; `GGML_CUDA_DISABLE_GRAPHS=1` is a complete workaround. Still open ([#27330](https://github.com/ggml-org/llama.cpp/issues/27330)).
3. **Flash-attention dimension assertions during speculative (MTP) decoding** — `GGML_ASSERT(neq0 == HSK)` on Vulkan ([#29418](https://github.com/ggml-org/llama.cpp/issues/29418)) and a head-dimension mismatch crash on SYCL with gemma4-assistant ([#29419](https://github.com/ggml-org/llama.cpp/issues/29419)).
4. **Speculative decoding correctness** — draft-model speculation diverges from vanilla output under greedy sampling on *quantized* targets (bf16 matches), and ngram speculation on the same target also diverges. Open, 30 comments ([#25618](https://github.com/ggml-org/llama.cpp/issues/25618)). Related: server returns `logprob 0.0` for every token when speculation is enabled ([#27972](https://github.com/ggml-org/llama.cpp/issues/27972)); ngram-cache `begin()` is a no-op, acceptance drops **86% → 11%** and becomes slower than no speculation ([#27852](https://github.com/ggml-org/llama.cpp/issues/27852)).
5. **Server prompt/KV cache contamination** — RAM-backed prompt cache (`--cache-ram`/`--cache-idle-slots`, both default-on) can restore an unrelated finished conversation into a fresh slot under concurrent load ([#27148](https://github.com/ggml-org/llama.cpp/issues/27148)). High impact for multi-tenant deployments.
6. **Vision-model KV cache save broken** — `/slots/3?action=save` fails for vision-enabled models; now closed ([#19466](https://github.com/ggml-org/llama.cpp/issues/19466)).
7. **OpenVINO crash (STATUS_ILLEGAL_INSTRUCTION)** attributed to AVX-512 on Core Ultra 7 265K; open ([#28726](https://github.com/ggml-org/llama.cpp/issues/28726)).
8. **Metal `MUL_MAT_ID` with duplicate IDs** — wrong/dropped rows and potential overflow into the next expert slice; fix PR open ([#30373](https://github.com/ggml-org/llama.cpp/pull/30373)).
9. **Hardening PRs against out-of-bounds reads** — `ggml_roll` signed-overflow guard defeated for `INT_MIN` ([#30375](https://github.com/ggml-org/llama.cpp/pull/30375)); `ggml_ssm_scan` indexes state with unchecked `ids` ([#30372](https://github.com/ggml-org/llama.cpp/pull/30372)); `ggml_opt_fit` passes an 8-byte `epoch` where a 28-byte params struct is read ([#30371](https://github.com/ggml-org/llama.cpp/pull/30371)).
10. **Server checkpoint selection after tool requests** — suboptimal checkpoint selection erases valid context in multi-turn tool flows; fix open ([#24891](https://github.com/ggml-org/llama.cpp/pull/24891)).
11. **SYCL memory errors** in `mul_mat`, split buffer, and host pool ([#29889](https://github.com/ggml-org/llama.cpp/pull/29889)); DMMV row-tail guard ([#29338](https://github.com/ggml-org/llama.cpp/pull/29338)); earlier tensor-split memory leak closed ([#27845](https://github.com/ggml-org/llama.cpp/issues/27845)).
12. **Jinja `tojson` float formatting** — currently truncates to 6 significant digits and drops `.0`, diverging from Jinja2/Python (`48.858844` → `48.8588`); fix open ([#30362](https://github.com/ggml-org/llama.cpp/pull/30362)).

## 6. What This Means for Application Developers

- **Embedders and packagers should test against b11563.** The switch to hidden symbol visibility can break builds that relied on leaked internal GGML symbols. Likewise, Homebrew’s 0.5.0 packaging historically shipped without the web UI ([#29849](https://github.com/ggml-org/llama.cpp/issues/29849)) — verify your distribution includes `tools/server` assets.
- **Treat speculative decoding as opt-in for quantized targets.** Multiple open issues show greedy-output divergence and invalid logprobs when target weights are Q4_K_M-class, plus an ngram-cache state bug that inverts the expected speedup. Validate acceptance rates and output determinism per model/quant before enabling in production.
- **Audit prompt-cache defaults in multi-tenant servers.** `--cache-ram` and `--cache-idle-slots` are enabled by default and have a documented cross-conversation contamination case under concurrency ([#27148](https://github.com/ggml-org/llama.cpp/issues/27148)). Disabling them trades latency for isolation.
- **Distributed MoE serving over `ggml-rpc-server` is not yet production-safe** on GB10/unified-memory platforms ([#30052](https://github.com/ggml-org/llama.cpp/issues/30052)). Budget for fallback to single-node or tensor-split within one host.
- **Intel and Adreno users get real gains.** OpenCL FA/Gemma-4 decode work, image-limit fallbacks, and SYCL Q4_0/MoE reorder fixes materially improve prompt processing — pull latest builds if you were seeing post-decode slowdowns.
- **MoE on Vulkan/Metal is more correct today.** Duplicate expert IDs (common with top-k routing) are now handled in Vulkan’s prepass and fixed for Metal in [#30373](https://github.com/ggml-org/llama.cpp/pull/30373); if you serve MoE models on those backends, update.
- **Watch the RDNA1 PP regression** ([#30039](https://github.com/ggml-org/llama.cpp/issues/30039)) if you deploy on RX 5700 XT-class hardware; generation speed is unaffected, so long-prompt agents are the main risk.

</details>

<details>
<summary><strong>Ollama</strong> — <a href="https://github.com/ollama/ollama">ollama/ollama</a></summary>

# Ollama Digest — 2026-10-12

## Today's Highlights

No new releases shipped in the last 24 hours, but the biggest landed change is PR #18917 (closed), bumping llama.cpp to b11521 and adding built-in support for decision models via `/v1/systemone` plus EmbeddingGemma 2. The day's activity is dominated by **tool-calling correctness**: a long-running Qwen 3.8 streaming failure (#17778, 52 comments), tool calls breaking when `format: "json"` is set (#18944), and silent numeric precision loss in the Go API client (#18942). On the hardware side, MLX on Apple Silicon continues to generate crash reports (#18940, #18885), and Windows updates are causing CUDA backend breakage (#18712).

## Releases & Breaking Changes

No releases in the last 24h.

Breaking/behavioral changes in flight or recently closed:

- **`reasoning_effort: "xhigh"` rewritten to `"max"`** caused HTTP 500 on Qwen3.8-family models over `/v1/chat/completions` (closed, #18943) — https://github.com/ollama/ollama/issues/18943
- **`OLLAMA_NUM_PARALLEL` silently pinned to 1** for eleven hybrid/recurrent architectures (`qwen35moe`, `mllama`, `qwen3vl*`, etc.) in `server/sched.go` (closed, #18938); proposed fix adds `OLLAMA_PARALLEL_HYBRID` (#18939) — https://github.com/ollama/ollama/issues/18938 | https://github.com/ollama/ollama/pull/18939
- **`keep_alive` int64 overflow** unloads models immediately instead of keeping them resident; fix planned via clamping (#18946) — https://github.com/ollama/ollama/pull/18946
- **`manifests-v2`** alias behavior questioned after odd tag aliases appeared in `ollama list` (closed, #18870) — https://github.com/ollama/ollama/issues/18870

## New Model & Hardware Support

- **llama.cpp b11521 (PR #18917, closed)** — adds built-in support for decision models (`/v1/systemone`, prompt built by llama.cpp, removing Ollama's own scorer/Clef patch) and **EmbeddingGemma 2** — https://github.com/ollama/ollama/pull/18917
- **d1-3B / d1-omni-600M request (#18890)** — LiquidAI decision models with image (and audio, 600M) support; currently fail with `unsupported decision encoding "lfm2-d1"` — https://github.com/ollama/ollama/issues/18890
- **EmbeddingGemma 2 pull fails on Linux (#18825, 14 👍)** — `embeddinggemma-2:740m` requires MLX runtime, so Linux users cannot pull it — https://github.com/ollama/ollama/issues/18825
- **MLX backend regressions (#18885, #18940)** — `mlx runner failed: panic` on M-series with small Gemma 4 MLX models; SDPA kernel panic on M2 Max with `head_dim=256` (Qwen 3.8:27b / Gemma 4:31b) due to threadgroup request of 960 vs hardware max 896 — https://github.com/ollama/ollama/issues/18885 | https://github.com/ollama/ollama/issues/18940
- **Vulkan AMD crash (#18557)** — `0xc0000005` access violation loading any model on RX 6800 XT (driver 32.0.21045.5002); CPU path unaffected — https://github.com/ollama/ollama/issues/18557

## Performance & Optimization

- **OpenAI-compatible timings inflated (#18948)** — `timings.prompt_per_second` on `/v1/*` divides total prompt tokens by uncached-only time, overstating prompt throughput by the cache hit ratio — https://github.com/ollama/ollama/issues/18948
- **Hybrid-model parallelism (#18939, open)** — lifts the one-slot pin so hybrid/recurrent models can use multiple sequences (llama-server already runs `kv_unified=false`, one KV cache per sequence) — https://github.com/ollama/ollama/pull/18939
- **Scheduler reload on `shift` mismatch (#18950)** — `needsReload` in `server/sched.go` forces a reload when a request's `shift` differs from the loaded runner; proposed fix is to launch shift-capable and enforce `shift:false` per request (observed with `gemma4-26b`, 131072 ctx, ROCm RX 7900 XTX) — https://github.com/ollama/ollama/issues/18950
- **Verified manifest/model reuse (PR #18947, open)** — consolidates named-manifest resolution and runner selection into one verified cold lookup, reused across generate/chat/embed/System — https://github.com/ollama/ollama/pull/18947
- **Multi-GPU VRAM accounting (#18349)** — scheduler reads discovery names (`CUDA1`) against maps keyed by child llama-server log names (`CUDA0`), diverging from actual usage — https://github.com/ollama/ollama/issues/18349
- **Thinking token budget (PR #17566, open)** — per-request/per-model bound on reasoning tokens to stop models looping inside thinking blocks and burning the full context — https://github.com/ollama/ollama/pull/17566

## Stability & Regressions

Ranked by severity:

1. **MLX SDPA kernel panic, Apple Silicon (#18940, open)** — hard panic during prefill on M2 Max with `head_dim=256`; no fix PR yet — https://github.com/ollama/ollama/issues/18940
2. **MLX runner panic (#18885, open)** — repeated failures on `gemma4:e2b-mlx`, Mac mini M6 / 16 GB — https://github.com/ollama/ollama/issues/18885
3. **Vulkan access violation on AMD (#18557, open)** — any model crashes `llama-server` with `exit status 0xc0000005`; same underlying issue as #18494 — https://github.com/ollama/ollama/issues/18557
4. **Windows auto-update leaves `ggml-cuda.dll` as `.tmp` (#18712, open)** — 0 B VRAM detection, silent CPU fallback; PR #18951 preserves earlier upgrade logs to aid diagnosis — https://github.com/ollama/ollama/issues/18712 | https://github.com/ollama/ollama/pull/18951
5. **Qwen 3.8 streaming 500: "no user query found in messages" (#17778, open, 30 👍, 52 comments)** — occurs during tool-call loops with 205k context; the highest-engagement open bug — https://github.com/ollama/ollama/issues/17778
6. **Tool calls unusable with `format: "json"` (#18944, open)** — model either inlines the call in the response or fabricates one; works without `format` — https://github.com/ollama/ollama/issues/18944
7. **Ollama Cloud: `glm-5.3-flash` stream never terminates (#18926, open)** — work completes but the client hangs at ~0% CPU on exit — https://github.com/ollama/ollama/issues/18926
8. **Windows `ollama list` empty with NTFS mount-point model path (#18921, open)** — fix PR #18949 resolves models through junctions/volume mount points — https://github.com/ollama/ollama/issues/18921 | https://github.com/ollama/ollama/pull/18949
9. **Raspberry Pi 5: second `ollama run` hangs (#18796, open)** — first run works, subsequent runs stall with no output — https://github.com/ollama/ollama/issues/18796
10. **Go API loses numeric precision in tool-call arguments (#18942, open)** — `9007199254740993` decodes as `9007199254740992`; fix PR #18941 with regression tests — https://github.com/ollama/ollama/issues/18942 | https://github.com/ollama/ollama/pull/18941
11. **Cloud JSON schema ignored by `qwen3-coder:480b-cloud` (#12362, open)** — works locally on `qwen3-coder:30b`; cloud differs — https://github.com/ollama/ollama/issues/12362

Recently closed (no action needed): #12940 (Windows runner exit status 2), #7288 (Windows embedding `wsarecv`), #13037 (`ollama run` not starting server on Windows), #18447 (intermittent "model not found" from canonicalization), #8431 (`bge-large` GGML assert, resolved via `num_ctx`), #6887 (reader-lm temperature), #8329 (dolphin3 tools template).

## What This Means for Application Developers

- **Tool calling is the weak spot right now.** If your agent depends on structured tool calls, avoid combining them with `"format": "json"` (#18944) and be aware that Qwen 3.8 can loop into a 500 mid-stream (#17778). If you use the **Go client**, numeric tool-call arguments silently round — pin to the fix in #18941 or pass high-precision integers as strings until it lands (#18942).
- **Cloud and local are not interchangeable.** Two open reports (#12362, #18926) show cloud models diverging from local behavior on JSON schema adherence and stream termination. Test both paths if you route between them.
- **Reasoning controls are getting stricter.** The `xhigh`→`max` rewrite (#18943) and the proposed token budget (#17566) indicate `think`/`reasoning_effort` semantics are in flux — treat them as version-sensitive and test chat templates per model family.
- **Mac users should validate before upgrading.** MLX panics on `head_dim=256` models (#18940) and the small Gemma 4 MLX variants (#18885) make Apple Silicon a rough target for the newest model families right now.
- **Watch ops-adjacent fixes:** `keep_alive` overflow (#18946) can unload a long-pinned model on request one, and OpenAI-compatible `timings.prompt_per_second` (#18948) is currently inflated — do not baseline capacity from those numbers.
- **Windows deployments:** verify `ggml-cuda.dll` integrity after auto-updates (#18712) and avoid placing `OLLAMA_MODELS` behind an NTFS mount point until #18949 lands (#18921).

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-10-12

**Scope:** [github.com/BerriAI/litellm](https://github.com/BerriAI/litellm) — 74 issues and 226 PRs updated in the last 24h.

## 1. Today's Highlights
LiteLLM published [v1.106.0-rc.1](https://github.com/BerriAI/litellm/releases/tag/v1.106.0-rc.1), [v1.105.0](https://github.com/BerriAI/litellm/releases/tag/v1.105.0), and [v1.104.3](https://github.com/BerriAI/litellm/releases/tag/v1.104.3), with Docker cosign-signature verification as the only release-note detail provided. Issue traffic remains concentrated on MCP gateway auth/tool-catalog correctness, Anthropic↔OpenAI translation gaps, and Gemini token accounting. Open PRs add [virtual-model routing](https://github.com/BerriAI/litellm/pull/46024), [RodiumAi provider support](https://github.com/BerriAI/litellm/pull/46023), [Wingback guardrails](https://github.com/BerriAI/litellm/pull/38685), and [scheduled email spend reports](https://github.com/BerriAI/litellm/pull/45954).

## 2. Releases & Breaking Changes
- **v1.106.0-rc.1** — release candidate: [Release](https://github.com/BerriAI/litellm/releases/tag/v1.106.0-rc.1)
- **v1.105.0** — [Release](https://github.com/BerriAI/litellm/releases/tag/v1.105.0)
- **v1.104.3** — [Release](https://github.com/BerriAI/litellm/releases/tag/v1.104.3)
- **Docker signing:** all LiteLLM Docker images are signed with cosign using the key introduced in [commit `0112e53`](https://github.com/BerriAI/litellm/commit/0112e53046018d726492c814b3644b7d376029d0).
- No explicit breaking API/config changes or migration notes were included in the provided release data. Treat v1.106.0-rc.1 as pre-release.

## 3. New Model & Hardware Support
- No new CUDA/ROCm/Metal/CPU, quantization, or local model-backend support appeared in this window.
- **RodiumAi OpenAI-compatible provider** — PR #46023 adds `rodiumai` for Africa/Mobile Money access: [PR #46023](https://github.com/BerriAI/litellm/pull/46023)
- **Wingback guardrail provider** — PR #38685 adds `guardrail: wingback` for runtime security: [PR #38685](https://github.com/BerriAI/litellm/pull/38685)
- **Virtual models for provider routing** — PR #46024 groups deployments behind `virtual_model/<name>` with preference/failover: [PR #46024](https://github.com/BerriAI/litellm/pull/46024)
- **AWS Bedrock autodiscovery** — feature request #43568 closed; no landed support details in the data: [Issue #43568](https://github.com/BerriAI/litellm/issues/43568)

## 4. Performance & Optimization
- No throughput, latency, or memory benchmarks landed in the provided data.
- Routing/failover work in progress:
  - Virtual models for flexible provider routing and failover: [PR #46024](https://github.com/BerriAI/litellm/pull/46024)
  - Session-affinity pin yielding when saturated and another deployment is idle: [Issue #45868](https://github.com/BerriAI/litellm/issues/45868)
  - Complexity router should clamp rather than overwrite caller `max_tokens`: [Issue #44869](https://github.com/BerriAI/litellm/issues/44869)
- Test/infra reliability:
  - Ownership proxy DB pool headroom: [PR #46025](https://github.com/BerriAI/litellm/pull/46025)
  - Redis port collision retry: [PR #46019](https://github.com/BerriAI/litellm/pull/46019)

## 5. Stability & Regressions
Ranked by impact.

**Critical — billing/spend correctness**
- Gemini tool-use prompt tokens are added to `prompt_tokens` but not billed (#45991). Fix PRs: #46017 (closed), #46021 (open). [Issue #45991](https://github.com/BerriAI/litellm/issues/45991) · [PR #46017](https://github.com/BerriAI/litellm/pull/46017) · [PR #46021](https://github.com/BerriAI/litellm/pull/46021)
- `no-log: true` skips the proxy's own spend tracking on successful requests (#44503). [Issue #44503](https://github.com/BerriAI/litellm/issues/44503)
- Deployment custom pricing (`input_cost_per_token: 0`) silently ignored after model-cost-map reload; per-request pricing overwrites canonical entries (#30081). [Issue #30081](https://github.com/BerriAI/litellm/issues/30081)

**High — MCP and protocol translation**
- MCP Gateway OAuth credentials are not refreshed; tools disappear after token expiry (#33072). [Issue #33072](https://github.com/BerriAI/litellm/issues/33072)
- MCP tool pin ignores annotations and `outputSchema`; `readOnlyHint` can flip to destructive with no drift alert (#44093). [Issue #44093](https://github.com/BerriAI/litellm/issues/44093)
- Anthropic `/v1/messages` drops `tool_result.is_error` when translating to OpenAI tool messages (#44979). [Issue #44979](https://github.com/BerriAI/litellm/issues/44979)
- Claude Code auto classifier fails for GPT-5.6 models: `/v1/messages` does not translate `stop_sequences` → `stop` for OpenAI-format providers, causing 400 (#33075). [Issue #33075](https://github.com/BerriAI/litellm/issues/33075)
- A2AConfig declares `temperature`, `max_tokens`, and `top_p` as supported, then silently drops all three (#45944). [Issue #45944](https://github.com/BerriAI/litellm/issues/45944)
- Responses API namespace tools are stripped when converting to Gemini format; only name preserved (#29854). [Issue #29854](https://github.com/BerriAI/litellm/issues/29854)

**Medium — router/provider edge cases**
- `router_settings.enable_pre_call_checks: true` does not enforce deployment RPM on `/v1/messages` (#45987). [Issue #45987](https://github.com/BerriAI/litellm/issues/45987)
- Gemini 3 provider always injects `temperature=1.0`; Google may reject sampling params on upcoming models (#45333). [Issue #45333](https://github.com/BerriAI/litellm/issues/45333)
- Databricks chat→Responses→chat bridge converts string user content to text blocks, breaking `json_object` (#45618). [Issue #45618](https://github.com/BerriAI/litellm/issues/45618)
- Databricks-hosted GPT-5.6 family function tools fail with `reasoning_effort` error on `/chat/completions` (#43708). [Issue #43708](https://github.com/BerriAI/litellm/issues/43708)
- Bedrock per-turn-control is filtered out even though Bedrock Invoke supports it; Claude Code fails with `messages.1.output_config` extra inputs (#45411). [Issue #45411](https://github.com/BerriAI/litellm/issues/45411)
- `/v1/audio/transcriptions` returns JSON for `response_format=text` instead of `text/plain` (#31457). [Issue #31457](https://github.com/BerriAI/litellm/issues/31457)
- `is_gpt_reasoning_series_name` checks raw model path instead of normalized name (#45998). [Issue #45998](https://github.com/BerriAI/litellm/issues/45998)
- UI cannot re-edit Model LiteLLM Params once added (#23998). [Issue #23998](https://github.com/BerriAI/litellm/issues/23998)
- UI forces team selection for personal keys even when user has model access (#27292). [Issue #27292](https://github.com/BerriAI/litellm/issues/27292)

**Fix PRs / closed issues**
- Partitioned SpendLogs startup schema check fix: [PR #46022](https://github.com/BerriAI/litellm/pull/46022)
- Vertex embedding `billableCharacterCount` mapping: [PR #46008](https://github.com/BerriAI/litellm/pull/46008)
- Router default fallback global leak test fix: [PR #46014](https://github.com/BerriAI/litellm/pull/46014)
- Anthropic `extra_body.thinking` promotion: [PR #45032](https://github.com/BerriAI/litellm/pull/45032)
- Virtual key model alias editing: [PR #45412](https://github.com/BerriAI/litellm/pull/45412)
- Router caller fallback preservation: [PR #45099](https://github.com/BerriAI/litellm/pull/45099)

## 6. What This Means for Application Developers
- **MCP agent builders:** treat OAuth token refresh and tool-catalog pinning as production risks. Tools can vanish after token expiry (#33072), and pinned catalogs may miss destructive annotation changes (#44093).
- **Anthropic-compatible clients:** if you route Claude Code or Anthropic-style requests to OpenAI-format providers, validate `stop_sequences` and `tool_result.is_error` handling before rollout (#33075, #44979).
- **Spend-sensitive deployments:** audit Gemini tool-use billing, `no-log` requests, and custom model pricing. Under-billing or skipped spend logs can distort tenant budgets (#45991, #44503, #30081).
- **Router operators:** virtual models (#46024) and session-affinity yield (#45868) are promising for reliability, but `/v1/messages` RPM enforcement remains inconsistent (#45987).
- **Provider integrations:** RodiumAi (#46023) and Wingback (#38685) expand reach and runtime security; Bedrock autodiscovery is still only a closed request (#43568).
- **UI/dev experience:** key alias editing (#45412), Playground IME handling (#35622), and AI Hub pill colors (#35624) are in flight; the model-param re-edit bug (#23998) remains open.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-10-12

## 1. Today's Highlights
No new releases landed in the last 24h, but Studio development is moving fast: an opt-in MLX image engine for Apple Silicon adds Z-Image support, Wan2.1 video families are now recognized, and MCP access lets coding agents drive Studio with API keys. On the training/runtime side, a bitsandbytes optimizer-sync optimization and the ongoing KV-preemption work target real throughput and multi-chat memory pressure. Several long-running correctness reports were closed, but at least one silent-gradient bug and a MoE-LoRA checkpoint-layout hazard warrant attention.

## 2. Releases & Breaking Changes
- No new releases in the last 24h; no declared breaking changes.
- Note for new installs only: [PR #13380](https://github.com/unslothai/unsloth/pull/13380) moves new Linux aarch64 cu130 / Python 3.13 Studio installs (Grace, GH200, GB200, DGX Spark) to torch 2.13. Existing installs keep their current torch.

## 3. New Model & Hardware Support
- **Apple Silicon / MLX image generation**: [PR #13379](https://github.com/unslothai/unsloth/pull/13379) adds an opt-in MLX image engine for Studio starting with Z-Image (Turbo and base), running mflux in-process on the user's GGUF and repacking Q4_0/Q4_1/Q8_0 blocks into MLX affine group-32 weights.
- **Wan2.1 video**: [PR #13376](https://github.com/unslothai/unsloth/pull/13376) adds T2V 1.3B/14B families plus I2V and repackaged/GGUF variants (city96, QuantStack) that were previously refused as unsupported.
- **Linux aarch64 CUDA**: [PR #13380](https://github.com/unslothai/unsloth/pull/13380) aligns aarch64 cu130 installs with the x86_64 torch 2.13 route that flash-attn 2.8.4 / causal-conv1d 1.7.0 / mamba-ssm 2.3.2.post1 wheels depend on.
- **RAG embedding GPU offload**: [PR #13365](https://github.com/unslothai/unsloth/pull/13365) (fixes [#11768](https://github.com/unslothai/unsloth/issues/11768)) exposes an embedding device setting so Studio/Desktop can move document indexing off CPU; previously only `RAG_EMBED_DEVICE` worked.
- **Intel GPU**: [#12836](https://github.com/unslothai/unsloth/issues/12836) (closed) asks for documented Intel GPU install pins since auto-detection was not added.
- **Diffusion classification**: [#13328](https://github.com/unslothai/unsloth/issues/13328) reports `general.architecture=qwen_image21` GGUFs being filed as text-generation and hidden from the Images page (closed).

## 4. Performance & Optimization
- **Optimizer sync removal**: [PR #13389](https://github.com/unslothai/unsloth/pull/13389) skips bitsandbytes' per-parameter `torch.cuda.synchronize()` in non-paged optimizer steps (unnecessary after the paged-optimizer prefetch change), cutting host-side stalls during training.
- **KV preemption for parallel chats**: [PR #10301](https://github.com/unslothai/unsloth/pull/10301) and [PR #10358](https://github.com/unslothai/unsloth/pull/10358) rework Studio's `--parallel N --kv-unified` launch so parallel chats share one KV cache instead of starving/aborting each other; #10358 pairs with unslothai/llama.cpp#260 (`--preempt-ram`) so llama-server parks slots in host RAM itself.
- **UI scroll performance**: [PR #13255](https://github.com/unslothai/unsloth/pull/13255) targets long conversations with large responses, thinking blocks, code and LaTeX that currently degrade the whole interface.
- **First-token timeout**: [PR #13390](https://github.com/unslothai/unsloth/pull/13390) makes the first-token waiting limit configurable (default 20 min, 1–10,080 min, or unlimited) for slow local GGUF hardware.
- **Observability**: [PR #13351](https://github.com/unslothai/unsloth/pull/13351) adds an optional detailed Live monitor with GPU load, temperature and power (covers [#12589](https://github.com/unslothai/unsloth/issues/12589), [#9015](https://github.com/unslothai/unsloth/issues/9015), [#8527](https://github.com/unslothai/unsloth/issues/8527)).
- **Regression watch**: [#10698](https://github.com/unslothai/unsloth/issues/10698) (closed) reports the entire context being reprocessed after every tool call — multi-minute stalls past 30k tokens. [#5752](https://github.com/unslothai/unsloth/issues/5752) flags unusually high VRAM during Studio QLoRA.

## 5. Stability & Regressions
Ranked by severity:

1. **Silent training failure (correctness)** — [#5230](https://github.com/unslothai/unsloth/issues/5230): Gemma 4 (`gemma-4-26B-A4B-it`, likely E2B/E4B/31B) fused-loss kernel produces zero gradients with last-assistant-only label masking; training runs without error but the model never updates. Workaround: `UNSLOTH_RETURN_LOGITS=1`. Related [#3071](https://github.com/unslothai/unsloth/issues/3071) reports that same env var silently reverting to `"0"` after training starts (closed pending confirmation).
2. **Silent checkpoint corruption risk** — [#6930](https://github.com/unslothai/unsloth/issues/6930): MoE-LoRA via PEFT `target_parameters` packs `lora_B` block-wise per expert into 2D tensors; the layout is undocumented and silently breaks downstream converters.
3. **Studio server deadlock** — [#9008](https://github.com/unslothai/unsloth/issues/9008): all threads blocked in `sqlite3.connect()`/`close()`, socket open but no requests accepted (closed).
4. **Crash on architecture load** — [#4037](https://github.com/unslothai/unsloth/issues/4037): `JambaAttention has no attribute feed_forward` on Jamba2-Mini int4 (closed, 35 comments).
5. **GPU/CPU misrouting** — [#4644](https://github.com/unslothai/unsloth/issues/4644) (7 👍, closed) and [#5941](https://github.com/unslothai/unsloth/issues/5941) (closed): Studio running inference on CPU despite an NVIDIA GPU.
6. **Precision/memory** — [#5344](https://github.com/unslothai/unsloth/issues/5344) (safetensors loading in full BF16/FP16 instead of 4-bit QLoRA, ~15.7 GB VRAM, closed); [#11792](https://github.com/unslothai/unsloth/issues/11792) (Qwen-Image-2.1 Q4_K_M OOM on M5 Max 48 GB, closed); [#3226](https://github.com/unslothai/unsloth/issues/3226) (Gemma 3N OOM but not Gemma 3, closed).
7. **UI crashes/regressions** — [#10288](https://github.com/unslothai/unsloth/issues/10288) (`tapClientLookup` out-of-bounds, duplicate `toolCallId`), [#12845](https://github.com/unslothai/unsloth/issues/12845) (OPEN: AppImage window can't resize, framerate issues on Arch/GNOME/Wayland), [#10769](https://github.com/unslothai/unsloth/issues/10769) (OPEN: large codeblocks lag the interface), [#7877](https://github.com/unslothai/unsloth/issues/7877) (Ollama models no longer visible; context size inflated ~50%).
8. **Tool-calling loops** — [#7045](https://github.com/unslothai/unsloth/issues/7045): model stops generating during tool calls in Studio chat/API until the message is resent.

## 6. What This Means for Application Developers
- **Patch Gemma 4 training immediately**: if you fine-tune Gemma 4 with fused loss and last-assistant-only masking, verify gradients aren't silently zero. Set `UNSLOTH_RETURN_LOGITS=1` for custom-metric validation, and confirm it stays set at runtime ([#5230](https://github.com/unslothai/unsloth/issues/5230), [#3071](https://github.com/unslothai/unsloth/issues/3071)).
- **Audit MoE-LoRA exports**: if you train Qwen3-30B-A3B-style MoE adapters via `target_parameters` and ship checkpoints to third-party converters, validate `lora_B` unpacking against [#6930](https://github.com/unslothai/unsloth/issues/6930) — the failure is silent.
- **Multi-tenant Studio serving is getting safer**: the KV-preemption stack ([#10301](https://github.com/unslothai/unsloth/pull/10301), [#10358](https://github.com/unslothai/unsloth/pull/10358)) is aimed at parallel chats sharing one cache; expect fewer slot-eviction aborts when running agents concurrently.
- **Agents can now drive Studio**: [PR #13118](https://github.com/unslothai/unsloth/pull/13118) exposes `/mcp/` authenticated by `sk-unsloth-...` keys so Claude Code/Codex can operate Studio; [PR #13372](https://github.com/unslothai/unsloth/pull/13372) gives chat models browser tools, and [PR #13353](https://github.com/unslothai/unsloth/pull/13353) lets projects run tools in existing local folders.
- **RAG indexing is no longer CPU-bound by default if you opt in**: set the embedding device in Settings instead of relying on `RAG_EMBED_DEVICE` ([PR #13365](https://github.com/unslothai/unsloth/pull/13365)).
- **Slow-hardware tuning**: expose the new first-token waiting limit ([PR #13390](https://github.com/unslothai/unsloth/pull/13390)) if local GGUF backends exceed the default timeout.
- **Disk layout**: [PR #13373](https://github.com/unslothai/unsloth/pull/13373) keeps uv, its cache and managed Python inside `UNSLOTH_STUDIO_HOME`, addressing part of [#11327](https://github.com/unslothai/unsloth/issues/11327) for Windows/limited-profile-disk setups.
- **Interface caveat**: Studio performs poorly on very long, code/LaTeX-heavy conversations today ([#13255](https://github.com/unslothai/unsloth/pull/13255) in flight, [#10769](https://github.com/unslothai/unsloth/issues/10769)); avoid rendering extreme-context histories in the desktop UI until that lands.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*