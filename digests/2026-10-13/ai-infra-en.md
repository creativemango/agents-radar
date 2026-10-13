# AI Infrastructure Digest 2026-10-13

> Generated: 2026-10-13 03:08 UTC | Projects covered: 6

- [vLLM](https://github.com/vllm-project/vllm)
- [SGLang](https://github.com/sgl-project/sglang)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
- [Ollama](https://github.com/ollama/ollama)
- [LiteLLM](https://github.com/BerriAI/litellm)
- [Unsloth](https://github.com/unslothai/unsloth)

---

## Cross-Project Comparison

# Cross-Project Comparison Report — AI Infrastructure Ecosystem
**Digest window: 2026-10-13** · Projects: vLLM, SGLang, llama.cpp, Ollama, LiteLLM, Unsloth

*Counting note: "Issues" and "PRs" below are items **surfaced in the 24h digest**, not repository totals. They include open bugs, closed items, feature requests and roadmaps, so treat them as an activity indicator, not a defect count.*

---

## 1. Ecosystem Overview

The datacenter serving tier (vLLM, SGLang, LiteLLM) shipped **zero stable releases** today, while the local/edge tier moved fast — llama.cpp landed 11 builds in a single window and both Ollama and Unsloth pushed betas. Effort is concentrated on a narrow set of frontier MoE architectures (DeepSeek-V4/4.1, GLM-5.3-Flash, Qwen3.x, Gemma4), and the most consequential in-flight engineering is multi-adapter/MoE LoRA serving (SGLang's 21-part runtime stack) and prefill-decode disaggregation as the default scale-out topology. The dominant risk theme is **speculative decoding**, which is simultaneously the most actively optimized and the least stable feature across vLLM, SGLang and llama.cpp. Hardware breadth keeps expanding (AMD gfx942/gfx950/MI355X, Intel XPU/Arc, Ascend NPU, Apple MLX, Hexagon, AMX), but backend correctness bugs — duplicate expert IDs, FP8 load gaps, MLX kernel panics — are the recurring tax.

---

## 2. Activity Comparison

| Project | Layer | Issues (24h) | PRs (24h) | Release status |
|---|---|---|---|---|
| **vLLM** | Datacenter serving engine | ~21 | ~14 | **No release** — latest release list empty |
| **SGLang** | Datacenter serving engine | ~27 | ~20 (incl. 21-part LoRA stack #42880–#42888) | **No release** |
| **llama.cpp** | Local/edge GGUF runtime (ggml) | ~19 | ~23 | **11 builds: b11567–b11577** |
| **Ollama** | Local runtime + cloud product | ~26 | ~17 | **v0.40.4-rc0** |
| **LiteLLM** | LLM gateway / proxy | ~24 | ~14 | **No release** |
| **Unsloth** | Fine-tuning + Studio runtime | ~29 | ~15 | **v0.1.906-beta** |

**Reading the table:** issue-heavy counts for Ollama and Unsloth reflect active defect/regression backlogs rather than forward feature velocity. llama.cpp's same-day issue-to-PR ratio (~19:23) indicates the healthiest fix-throughput; SGLang is landing the largest single feature body (LoRA runtime) in staged parts.

---

## 3. Model Support Race

| Project | New architectures / models landed | Notable gaps (requested or broken) |
|---|---|---|
| **vLLM** | **Clef-Flash** native support via `/v1/systemone` (Qwen3.5 backbone + joint schema head); DeepGEMM 26/09/30 bump for DSv4 sparse indexer; NVFP4 per-forward W4A4/W4A16 for SM120/SM121 routed experts | DeepSeek-V4-Flash unrunnable on SM8x (#50576); GLM-5.3-Flash no sparse-MLA on sm_89 (#54059); Qwen3.8-Flash-Next-FP8 fails to load on ROCm (#58688); Gemma4 sequence classification (#43726); Qwen3.5-397B PD disaggregation (#43765) |
| **SGLang** | **GLM-5.3-Flash** (colocated prefill-attention CP + decode/verify TP, native MTP, DFlash2); **DeepSeek-V4.1** TP2 decode fusions (fused all-reduce + mHC epilogues, fused inverse-RoPE + WO-A + MXFP8, SM100 swap-AB attention); MiniCPM-V 4.7 cookbook; NVFP4 MoE LoRA on Marlin; XPU prefill graph; NPU prefill CP | AMD DSV4.1 sparse decode broken by upstream `aiter` API removal (#43765, closed without fix path) |
| **llama.cpp** | **vLLM d3 / "pplx-decider"** (b11572) and **d2** (b11571) with stricter tensor-name validation; same-day multi-sequence fix (b11577); Hexagon Q1_0 HVX `MUL_MAT`; Intel Arc Pro B65; multi-node TP via NCCL; RPC `supports_op` negotiation | Failling MTP/DFlash2 drafts under `--split-mode tensor`; Vulkan `MUL_MAT_ID` row loss (#29821) |
| **Ollama** | llama.cpp **b11521** → decision models + **EmbeddingGemma 2**; CLI decision-model support; Prism support (PR open); `clef` / `clef flash` cloud; glm-ocr EOT fix | Prism ternary GGUFs (`PQ2_0`/`PTQ1_0`) import failures (#18521); K2 Horizon family requested (#18698) |
| **LiteLLM** | No new model/hardware integration; **cost-parity tests** for gpt-6 / Claude 5 bridge; Gemini context-cache creation tokens now billed | Bedrock autodiscovery request closed unfixed (#43568) |
| **Unsloth** | **Qwen-Image-2.1-Turbo** (8-step image gen); **LTX-2.5 fp8/int8** from pre-quantized DiTs; text-diffusion LLM training; faster MoE GGUFs; speculative decoding | Ling 3.0 in Studio (#8532); Gemma4e2b on Windows (#13388); MLX pretraining structure (#8607) |

**Who's ahead:**
- **SGLang leads on frontier-MoE deployment depth** — it is the only project shipping production decode fusions for DeepSeek-V4.1 at TP2 and full GLM-5.3-Flash attention topology.
- **vLLM has the broadest model *and* hardware matrix**, plus the only native new-architecture landing today (Clef-Flash) and forward NVFP4 precision-switching.
- **llama.cpp has the fastest architecture intake velocity** for local serving — two new architectures plus a correctness fix inside one window.
- **Unsloth owns the generative-multimodal lane** (image/video) that no serving engine in this set covers natively.

---

## 4. Performance Frontier

Optimization effort is split across six distinct battlegrounds:

**KV cache & prefix reuse** — the weakest link, and the one agent workloads hit hardest.
- vLLM: `HIT_PENDING` has no deadline, stalling requests until client timeout (#49829); long-prefill defer mode (#61080) and adaptive prefill budget RFC (#52906) target tail latency.
- SGLang: EAGLE spec-decode KV index rebuilds now parallelized for the 90k–170k KV-token regime (#42803) — explicitly a long-context win. Critically, **both agentic KV-reuse proposals (#27574, #30928) went inactive**, leaving RadixAttention's byte-identical prefix matching as the only mechanism.
- llama.cpp: new `ggml_backend_sched_set_realloc_callback` (b11567) makes mid-inference buffer re-reservation observable — a real p99 diagnosability upgrade.
- Unsloth: full-context reprocessing after every tool call (#10698) — minutes of stall at ~30k tokens.

**Quantization / low precision** — where the headline numbers live.
- vLLM: NVFP4 MoE per-forward W4A16→W4A4 switching (#54617); but FlashInfer NVFP4 causal prefill is **1.7–1.8x slower than FP8 on SM120** (#50416).
- SGLang: FP8 MoE LoRA Triton vendor (#42885), CuTeDSL FP8 tile tables tuned for **B200, GB300, H200** (#42887), NVFP4 MoE LoRA on prepared Marlin weights (#42888).
- llama.cpp: **+13% token generation** on AMX Q4_K decode with bit-identical output (#30429) — the cleanest adoptable win today; Q2_K VGPR spill fix for gfx906/MI50 (#29910).
- Unsloth: LTX-2.5 avoids quantizing a 38 GB bf16 transformer at load, cutting peak memory and startup cost.

**Kernels / fusion** — abundant but narrow. vLLM's mamba gather fusion (7 Triton kernels → 1, ~6x) and DFlash padding (35.8 µs/step) are explicitly **isolated kernel microbenchmarks**, not end-to-end throughput claims. SGLang's proposed **monokernel framework** (#43949, persistent kernels covering most of a decoder layer in 1–2 launches) is the most ambitious structural idea in flight.

**Distributed / parallel serving** — SGLang's Q4 PD-disaggregation roadmap (#43699) positions PD as the mainstream deployment shape; heterogeneous TP KV transfer now handles hybrid SWA models (gpt-oss-120b, TP1 prefill / TP4 decode, #40909); prefill CP is the last open item across MHA/GQA backends (#21788). llama.cpp added multi-node NCCL tensor parallelism (#28967).

**Batching & scheduling** — vLLM `run-batch` now streams responses incrementally (frontend memory, #52505); SGLang's `bench_serving --request-rate` drifts progressively behind schedule (#43930), which **skews benchmark baselines**.

**Speculative decoding** — a frontier of its own, with real wins (SGLang EAGLE index parallelism) and real hazards (§6).

---

## 5. Layer Positioning

| Layer | Projects | Role and differentiator |
|---|---|---|
| **Datacenter serving engine** | **vLLM**, **SGLang** | GPU-first, multi-GPU/PD-aware, OpenAI-compatible APIs. vLLM competes on hardware/model breadth and is absorbing gateway concerns (opt-in `fastapi-guard` middleware, #60988). SGLang competes on frontier-model depth, long-context/prefix-heavy workloads and PD disaggregation, and is making the biggest multi-adapter bet. Both are now shipping features that were previously gateway-layer (auth, rate limiting, logging). |
| **Local / edge runtime** | **llama.cpp**, **Ollama** | llama.cpp is the engine + ggml backend layer: GGUF, CPU/consumer-GPU/Apple/Hexagon, multi-backend. Ollama is the productized layer on top — model management, CLI, desktop, cloud-hosted models — and explicitly tracks llama.cpp upstream via version bumps (b11521). Ollama now stretches up-stack with cloud models and tool calling. |
| **Gateway / proxy** | **LiteLLM** | Not an inference engine. Owns cross-provider protocol translation, auth, budgets, cost accounting, and the MCP gateway. Its moat is breadth of provider coverage and billing correctness — not kernel throughput. Directly threatened at the edges by engine-native middleware, but differentiated by cost/observability/MCP depth. |
| **Training / fine-tuning + local app runtime** | **Unsloth** | Fine-tuning, quantization, GGUF export, and Studio as the app surface. Uniquely spans **train → quantize → serve → agent** in one stack, and today extended into browser-agent execution and image/video generation. Embeds llama.cpp and vLLM as backends rather than competing with them. |

**Boundary blurring is the story:** engines are adding gateway middleware, Ollama is adding cloud/agent features, Unsloth is adding an agent runtime, and LiteLLM is adding an MCP gateway. Integration surface — not raw kernel speed — is where the layers are colliding.

---

## 6. Trend Signals

1. **DeepSeek-V4 / frontier MoE is the single integration battleground.** Every serving project is spending its highest-signal cycles there, and AMD is consistently the last platform supported (SM8x gap, gfx950 sparse-decode break, MI355X trackers).
2. **Speculative decoding is the highest-risk production feature in the ecosystem.** Three of four runtimes report it as a top instability source: SGLang's server-killing SIGKILL on any `temperature > 0` (#43412), GLM-5.3 + DFLASH silent quality collapse (#40843); llama.cpp divergence on quantized targets (#25618) and PP halving on multi-GPU splits (#27428); Unsloth's draft paths; vLLM's DFlash/DSpark acceptance collapse under prefix caching (#47930).
3. **Multi-LoRA / multi-adapter serving is the next unlock — and it will cause churn.** SGLang's 21-part stack (MoE LoRA across Triton, CuTeDSL, Marlin for FP8/NVFP4/BF16) is the largest feature landing anywhere. Meanwhile vLLM has an open CUDA-graphs deadlock with fused-MoE LoRA + `--fully-sharded-loras` (#60845). Plan evaluation, not adoption, this quarter.
4. **Agentic workloads have outrun engine KV capabilities.** Position-independent and programmatic KV reuse proposals are dead in SGLang; engines still depend on byte-identical prefix matching. Practical mitigation today is prompt-stability engineering (fixed prefixes, ordered tool schemas) — plus awareness that llama.cpp's RAM-backed prompt cache can leak content **across conversations** under concurrent load (#27148), a data-isolation issue for multi-tenant servers.
5. **Multi-tenancy and billing correctness are now infrastructure concerns.** LiteLLM's v1.93.0 Redis failure breaks caching *and* budget counters (#34614), v1.92's missing `budget_fallbacks` column blocks Admin UI (#33074), and a proxy-admin JWT previously bypassed personal `max_budget` (#41226). Unsloth patched symlink-following into chat tool workspaces and an HF token-clobbering bug.
6. **Hardware diversification is real but fragile.** AMD gfx942/gfx950, Intel XPU/Arc B65, Ascend NPU prefill CP, Apple MLX, Hexagon HVX, Arm AMX all saw activity — yet the AMD DSV4.1 break came from an **upstream `aiter` API removal outside SGLang's control**, and MLX SDPA panics block Ollama on Apple Silicon.
7. **Release cadence is bifurcating.** Datacenter and gateway tiers shipped nothing stable; local/packaged tiers shipped betas and 11 llama.cpp builds. Pin-and-benchmark discipline is mandatory: Ollama carries a reported 25–30% throughput regression vs v0.40.1 (#18968), and LiteLLM's advisories make v1.93.0/v1.92 unsafe defaults.

**What to watch as an agent/application developer:**
- Measure **TTFT per tool call** before scaling agent loops — Unsloth's reprocessing regression (#10698) and the universal KV-reuse gap both surface here.
- **Validate speculative decoding on your exact install method** (pip bundle vs. container vs. source) with sampled traffic, and diff against a non-speculative baseline on quantized targets.
- Treat **kernel-level benchmarks as directional only** — vLLM's 6x mamba fusion and 35.8 µs DFlash saving are isolated measurements; demand end-to-end numbers.
- Track SGLang's PD-disaggregation roadmap (#43699) and vLLM's long-prefill defer mode (#61080) if you operate long-context or tail-latency-sensitive fleets.
- Watch whether **engine-native middleware** (fastapi-guard in both vLLM and SGLang) erodes the standalone-gateway tier — relevant if you're standardizing on LiteLLM for auth and rate limiting rather than for cost/MCP/protocol translation.

---

## Per-Project Reports

<details>
<summary><strong>vLLM</strong> — <a href="https://github.com/vllm-project/vllm">vllm-project/vllm</a></summary>

# vLLM Digest — 2026-10-13
Source: [vllm-project/vllm](https://github.com/vllm-project/vllm)

## 1. Today’s Highlights
No new vLLM releases landed in the last 24h. The highest-signal activity is around DeepSeek-V4/DSv4.1: an SM8x/Ampere support request with 109 comments, a DSv4 CUDA-graph compressor fix, and multiple high-concurrency stability reports. On the performance side, a mamba align gather fusion claims a 6x kernel-level speedup, while a new long-prefill defer mode and optional API security middleware are moving through review.

## 2. Releases & Breaking Changes
- **No new releases.** The latest releases list is empty.
- **Mamba config deprecation/cleanup:** [PR #60043](https://github.com/vllm-project/vllm/pull/60043) removes leftovers of `mamba_cache_mode="all"` and deprecates `--mamba-cache-mode`; [Issue #60838](https://github.com/vllm-project/vllm/issues/60838) reports `--mamba-block-size` has no effect in valid configs after that removal. Mamba/hybrid cache tuning should be re-validated.
- **New opt-in scheduler flag:** [PR #61080](https://github.com/vllm-project/vllm/pull/61080) adds `--long-prefill-token-threshold-defer`, off by default.
- **Optional API middleware:** [PR #60988](https://github.com/vllm-project/vllm/pull/60988) adds optional `fastapi-guard` security middleware for in-process auth, rate limiting, and logging.
- No API-breaking changes surfaced in the last 24h.

## 3. New Model & Hardware Support
- **Clef-Flash:** [PR #61308](https://github.com/vllm-project/vllm/pull/61308) adds native support for `Cloudflare/clef-flash` via `/v1/systemone`, reusing the Qwen3.5 backbone and joint schema head.
- **DeepSeek-V4-Flash on Ampere:** [Issue #50576](https://github.com/vllm-project/vllm/issues/50576) requests SM8x support — A100/A800, RTX 30xx — for DeepSeek-V4-Flash / 0731 DSpark; currently not runnable on SM8x.
- **DeepGEMM for DSv4:** [PR #61149](https://github.com/vllm-project/vllm/pull/61149) bumps vendored DeepGEMM to the 26/09/30 release and adapts the sparse indexer.
- **GLM-5.3-Flash on Ada:** [Issue #54059](https://github.com/vllm-project/vllm/issues/54059) reports no sparse-MLA attention path on sm_89 / RTX 4090.
- **Gemma4 classification:** [Issue #43726](https://github.com/vllm-project/vllm/issues/43726) requests `Gemma4ForSequenceClassification`.
- **Hybrid SSM/GDN PD disaggregation:** [Issue #43765](https://github.com/vllm-project/vllm/issues/43765) requests PD disaggregation / KV transfer for Qwen3.5-397B-A17B-W8A8.
- **ROCm FP8 load gap:** [Issue #58688](https://github.com/vllm-project/vllm/issues/58688) reports Qwen3.8-Flash-Next-FP8 cannot load on ROCm due to missing FP8 `weight_scale`.
- **AMD MI355X trackers:** [Issue #57149](https://github.com/vllm-project/vllm/issues/57149) and [Issue #59575](https://github.com/vllm-project/vllm/issues/59575) track gfx950/MI355X optimization for Qwen3.8-2.4T-A95B and Qwen3.8-Flash-Next.
- **NVFP4 on SM12x:** [PR #54617](https://github.com/vllm-project/vllm/pull/54617) adds per-forward W4A4/W4A16 selection for routed experts.

## 4. Performance & Optimization
- **Mamba gather fusion:** [PR #58737](https://github.com/vllm-project/vllm/pull/58737) fuses the mamba align block-table gather from 7 Triton kernels to 1, ~6x speedup on all shapes for the isolated kernel. Scope note: kernel-level microbenchmark, not an end-to-end throughput claim.
- **DFlash spec-decode padding:** [PR #58633](https://github.com/vllm-project/vllm/pull/58633) parallelises DFlash draft-input padding, saving up to 35.8 µs/step from one workgroup. Also kernel-level.
- **Long-prefill deferral:** [PR #61080](https://github.com/vllm-project/vllm/pull/61080) adds a mode where one long prefill per step runs after waiting requests with leftover budget, if above threshold. Opt-in.
- **NVFP4 MoE precision switching:** [PR #54617](https://github.com/vllm-project/vllm/pull/54617) selects W4A16 for small token counts and W4A4 for larger counts on SM120/SM121.
- **MoE router fusion:** [PR #57410](https://github.com/vllm-project/vllm/pull/57410) fuses Transformers-backend MoE routers that score after top-k.
- **Frontend batch streaming:** [PR #52505](https://github.com/vllm-project/vllm/pull/52505) makes `vllm run-batch` write responses incrementally instead of buffering until the end, reducing frontend memory.
- **Known perf gaps:** [Issue #50416](https://github.com/vllm-project/vllm/issues/50416) reports FlashInfer NVFP4 KV-cache causal prefill is ~1.7–1.8x slower than FP8 on SM120; [Issue #34635](https://github.com/vllm-project/vllm/issues/34635) reports `AllReduceFusionPass(config)` takes 1s+ to construct; [Issue #48229](https://github.com/vllm-project/vllm/issues/48229) reports a sporadic 0.7–2.0s frozen V1 engine step once per generate batch.
- **Active optimization tracks:** [Issue #57406](https://github.com/vllm-project/vllm/issues/57406) GLM-5.3, [Issue #57149](https://github.com/vllm-project/vllm/issues/57149) and [Issue #59575](https://github.com/vllm-project/vllm/issues/59575) AMD MI355X, [Issue #52906](https://github.com/vllm-project/vllm/issues/52906) adaptive prefill token budget RFC.

## 5. Stability & Regressions
Ranked by severity:
1. **TP hang / EngineDeadError** — [Issue #41530](https://github.com/vllm-project/vllm/issues/41530): RPC `sample_tokens` timeout with DeepSeek-V4-Pro, TP=8, MTP speculative decoding. Open; no fix PR in the last-24h data.
2. **CUDA-graphs deadlock with fused-MoE LoRA** — [Issue #60845](https://github.com/vllm-project/vllm/issues/60845): vLLM 0.28.0, Qwen3.5-compatible 35B-A3B MoE, TP=2/4, `--fully-sharded-loras`. Open; no fix PR noted.
3. **Triton illegal memory access** — [Issue #56389](https://github.com/vllm-project/vllm/issues/56389): DeepSeek-V4.1-Flash `dsv4_topk` MoE routing kernel under high concurrency on H20. Mitigation: `max_num_seqs=256`. Open.
4. **ROCm engine stall** — [Issue #57429](https://github.com/vllm-project/vllm/issues/57429): GLM-5.3, TP8, v0.29.0 on gfx942, stalls inside an aiter fused MoE step. Open.
5. **ROCm FP8 load failure** — [Issue #58688](https://github.com/vllm-project/vllm/issues/58688): Qwen3.8-Flash-Next-FP8 missing FP8 `weight_scale`. Open.
6. **DSv4 CUDA-graph logprob drift** — [PR #59709](https://github.com/vllm-project/vllm/pull/59709): C128 compressor skip baked into CUDA graphs caused decode logprobs drift; fix PR open.
7. **Spec-decode acceptance collapse** — [Issue #47930](https://github.com/vllm-project/vllm/issues/47930): DFlash/DSpark draft acceptance collapses with automatic prefix caching. Open.
8. **KV offload timeout risk** — [Issue #49829](https://github.com/vllm-project/vllm/issues/49829): `HIT_PENDING` has no deadline; stalled writes can defer requests until client timeout. Open.
9. **Mamba block-size no-op** — [Issue #60838](https://github.com/vllm-project/vllm/issues/60838): `--mamba-block-size` has no effect after removal of `mamba_cache_mode="all"`. Open.
10. **MiniCPM-V-4.6 load failure** — [Issue #48449](https://github.com/vllm-project/vllm/issues/48449): vLLM 0.25.0 `qkqkv_proj` missing in `MiniCPMV4_6ViTWindowAttentionSelfAttn`. Open.
11. **Structured output / tool calling** — [Issue #55552](https://github.com/vllm-project/vllm/issues/55552): closed; `tool_choice="required"` not enforced with Qwen3.8-Flash-Next and xgrammar FSM errors. Verify fix if affected.
12. **Pending bugfix PRs:** [PR #61272](https://github.com/vllm-project/vllm/pull/61272) tokenizer `code_revision`; [PR #57797](https://github.com/vllm-project/vllm/pull/57797) RL attention sinks on layerwise reload; [PR #58524](https://github.com/vllm-project/vllm/pull/58524) off-grid CUDA graph capture sizes.

## 6. What This Means for Application Developers
- **No new release to adopt today.** Pin known-good versions; re-test Mamba/hybrid configs if you use `--mamba-block-size` or `--mamba-cache-mode`.
- **DeepSeek-V4/DSv4.1 on H20:** cap `max_num_seqs=256` for DeepSeek-V4.1-Flash high-concurrency to avoid the Triton illegal memory access in #56389. If using DeepSeek-V4-Pro with TP=8 and MTP speculative decoding, watch for the TP hang in #41530.
- **MoE + LoRA + TP:** avoid `--fully-sharded-loras` with CUDA graphs under concurrent traffic until #60845 is resolved.
- **Long-context serving:** the new long-prefill defer mode (#61080) may help tail latency but is off by default; adaptive prefill budgeting is still an RFC (#52906).
- **API deployments:** consider the opt-in fastapi-guard middleware (#60988) if you lack a reverse proxy for auth, rate limiting, and logging.
- **Batch workloads:** `vllm run-batch` now streams responses in input order, reducing frontend memory for large batches (#52505).
- **AMD/ROCm:** MI355X optimization is active, but Qwen3.8-Flash-Next-FP8 remains blocked on ROCm by #58688.
- **Kernel improvements are narrow:** #58737 and #58633 are isolated kernel wins; validate end-to-end before assuming throughput gains.

</details>

<details>
<summary><strong>SGLang</strong> — <a href="https://github.com/sgl-project/sglang">sgl-project/sglang</a></summary>

# SGLang Digest — 2026-10-13

## Today's Highlights
The DeepSeek V4 roadmap closed out ([#23602](https://github.com/sgl-project/sglang/issues/23602), 75 comments) with W4A16 Hopper support and Marlin follow-ups completed, while the Q4 Prefill-Decode Disaggregation roadmap was published ([#43699](https://github.com/sgl-project/sglang/issues/43699)) positioning PD as the default scale-out path. The largest in-flight effort is the 21-part LoRA runtime stack, which is landing MoE LoRA support across Triton, CuTeDSL, and Marlin vendors ([#42880](https://github.com/sgl-project/sglang/pull/42880)–[#42888](https://github.com/sgl-project/sglang/pull/42888)). On the risk side, a reproducible server-killing crash in speculative decoding on non-greedy requests ([#43412](https://github.com/sgl-project/sglang/issues/43412)) remains open.

## Releases & Breaking Changes
No new releases in the last 24h. Compatibility items worth noting:

- **[#43765](https://github.com/sgl-project/sglang/issues/43765) [CLOSED] — AMD DSV4.1 sparse decode broken by external dep removal.** `compact_attention_hip` imports `aiter`'s `pa_decode_sparse`, which aiter removed on 2026-09-18; upgrading aiter does *not* fix it. Affects gfx950 users on recent main.
- **[#30781](https://github.com/sgl-project/sglang/issues/30781) [CLOSED] — gateway/router protocol drift.** `sgl-model-gateway` v0.3.2 rejected `/v1/responses` tools with `type: "custom"`, breaking OpenAI Codex CLI compatibility; root cause was the Rust `openai-protocol` crate lagging Python `protocol.py`.
- **[#33385](https://github.com/sgl-project/sglang/issues/33385) [CLOSED] — offload behavior was not gated by flag.** Decode-mode retract crashed with `NotImplementedError` because KV offload was unconditional rather than gated on `--disaggregation-decode-enable-offload-kvcache`.
- **[#43610](https://github.com/sgl-project/sglang/pull/43610) — optional new dependency.** Adds `fastapi-guard` middleware (rate limiting, IP controls, payload screening) for deployments without a gateway; state is per-worker, so Redis is recommended for multi-worker.
- **[#42085](https://github.com/sgl-project/sglang/issues/42085) [OPEN] — silently ignored config.** `--bf16-gemm-backend gemv` is accepted but never used by unquantized linear layers.

## New Model & Hardware Support
- **GLM-5.3-Flash** — colocated prefill attention CP + decode/target-verify attention TP, with native MTP and DFlash2 support ([#43972](https://github.com/sgl-project/sglang/pull/43972), draft).
- **DeepSeek-V4.1** — TP2 decode path now reaches the small-batch decode fusions previously gated to TP4 (fused all-reduce + mHC epilogues, fused inverse-RoPE + WO-A + MXFP8, SM100 swap-AB decode attention) ([#43838](https://github.com/sgl-project/sglang/pull/43838)). Separate V4.1 optimization tracking in [#42170](https://github.com/sgl-project/sglang/issues/42170).
- **MiniCPM-V 4.7** — cookbook added ahead of model support merge ([#43829](https://github.com/sgl-project/sglang/pull/43829)).
- **NVFP4 MoE LoRA on Marlin** — new provider over prepared Marlin weights ([#42888](https://github.com/sgl-project/sglang/pull/42888)).
- **XPU** — full prefill graph backend for capture/replay parity with decode ([#30980](https://github.com/sgl-project/sglang/pull/30980)).
- **NPU** — prefill CP fix for generic (non-DSA) Ascend attention backends, covering Qwen3/Qwen3-MoE ([#41564](https://github.com/sgl-project/sglang/pull/41564)); conv layout alignment for mamba checkpoint-pool copies ([#43845](https://github.com/sgl-project/sglang/pull/43845)); `prefill_complete` now supported ([#43970](https://github.com/sgl-project/sglang/pull/43970)).
- **Heterogeneous TP** — PD KV transfer now handles hybrid SWA models (e.g. `gpt-oss-120b` with prefill TP1 / decode TP4) ([#40909](https://github.com/sgl-project/sglang/pull/40909)).

## Performance & Optimization
- **EAGLE spec-decode KV index building parallelized** over token blocks for flashinfer draft decode and verify/draft-extend (flashinfer, flashinfer_mla, aiter). Targets the 90k–170k KV-token long-context regime where index rebuilds were serial per request ([#42803](https://github.com/sgl-project/sglang/pull/42803)).
- **Startup tuning now includes FlashInfer's default tactic (`-1`)**, avoiding selection of an explicitly tuned tactic that is slower than the default for the profiling inputs ([#43927](https://github.com/sgl-project/sglang/pull/43927)).
- **LoRA runtime stack (parts 13–21)** — MoE LoRA stage kernels with FP8 row quantization and masked/contiguous row domains ([#42880](https://github.com/sgl-project/sglang/pull/42880)); CuTeDSL BF16 grouped GEMMs for SM90 (WGMMA) and SM100 (tcgen05) ([#42884](https://github.com/sgl-project/sglang/pull/42884)); packed MoE schedules ([#42883](https://github.com/sgl-project/sglang/pull/42883)); FP8 MoE LoRA Triton vendor ([#42885](https://github.com/sgl-project/sglang/pull/42885)); tuned CuTeDSL FP8 tile tables for **B200, GB300, and H200** ([#42887](https://github.com/sgl-project/sglang/pull/42887)).
- **Roadmaps in flight:** Prefill Context Parallelism for remaining MHA/GQA backends (flashinfer/trtllm-mha) is the last open item in [#21788](https://github.com/sgl-project/sglang/issues/21788); diffusion Q4 targets MiniMax-H3 with communication/compute overlap and validated GB200/GB300 recipes ([#43313](https://github.com/sgl-project/sglang/issues/43313)); block-diffusion (dLLM) serving roadmap continues in [#39499](https://github.com/sgl-project/sglang/issues/39499).
- **Proposed:** a monokernel framework (persistent kernels covering most of one decoder layer in one or two launches), with per-model specs and a CI gate ([#43949](https://github.com/sgl-project/sglang/issues/43949)).

## Stability & Regressions
Ranked by severity:

1. **[#43412](https://github.com/sgl-project/sglang/issues/43412) [OPEN] — spec decoding SIGKILLs the server process tree on the first sampled request.** Greedy (temp 0) works; any `temperature > 0` dies. Triggered by flashinfer verify-sampling JIT failing when a pip-installed bundle's bundled `cccl` conflicts with the pip cu13 toolkit. No fix PR referenced — **avoid enabling speculative decoding on pip-only installs with sampled traffic.**
2. **[#43557](https://github.com/sgl-project/sglang/issues/43557) [OPEN] — PD runtime role switch crashes with illegal memory access** after decode → prefill → decode, on sglang 0.5.21 with mooncake backend, TP1, `--enable-pd-role-switch` (`Qwen/Qwen3.5-0.8B`).
3. **[#40843](https://github.com/sgl-project/sglang/issues/40843) [OPEN] — GLM-5.3 + DFLASH spec decoding produces severe repetition and degenerate loops**, i.e. a correctness failure, not just a crash.
4. **[#43526](https://github.com/sgl-project/sglang/issues/43526) [OPEN] — accuracy issue with gfx942 block-FP8 linear AITER CK blockscale GEMM** (AMD).
5. **[#43765](https://github.com/sgl-project/sglang/issues/43765) [CLOSED] — DSV4.1 sparse decode on gfx950 fails with `ModuleNotFoundError`** due to an upstream aiter API removal.
6. **[#42085](https://github.com/sgl-project/sglang/issues/42085) [OPEN] — GEMV BF16 GEMM backend flag silently no-ops**, so users may benchmark a path that was never taken.
7. **[#43709](https://github.com/sgl-project/sglang/issues/43709) [OPEN] — transcription multipart upload has no application-level size limit** (resource-exhaustion exposure on endpoints that accept uploads).
8. **[#43930](https://github.com/sgl-project/sglang/issues/43930) [OPEN] — `bench_serving --request-rate` drifts progressively behind schedule**, so achieved request rate understates requested rate and skews benchmark results.
9. **[#42752](https://github.com/sgl-project/sglang/issues/42752) [OPEN] — CI flaky-test / infrastructure tracker** (49 comments) monitoring NVIDIA `PR Test Base` and `PR Test Extra`; complements the older CI tracker #17050. Expect elevated review latency on CI-gated PRs.
10. Closed-inactive correctness/robustness bugs resurfaced in the update window: [#29998](https://github.com/sgl-project/sglang/issues/29998) (`runai_streamer` silently corrupting GLM-5.2 under TP8), [#34384](https://github.com/sgl-project/sglang/issues/34384) (DSpark ragged CUDA Graph geometry), [#34399](https://github.com/sgl-project/sglang/issues/34399) (paged KV allocator launching kernels before OOM check), [#26618](https://github.com/sgl-project/sglang/issues/26618) (FlashInfer MIS delimiter indices after radix-cache prefix hits), [#31019](https://github.com/sgl-project/sglang/issues/31019) (GPT-OSS Harmony malformed output with `require_reasoning` + `json_schema`), [#31766](https://github.com/sgl-project/sglang/issues/31766) (prefill-side fd exhaustion).

## What This Means for Application Developers
- **Multi-tenant / multi-adapter serving is the big unlock coming.** The 21-part LoRA stack covers MoE LoRA for frontier serving shapes (FP8, NVFP4, BF16) with vendor-specific GEMMs for Hopper/Blackwell. If you serve many fine-tunes per base model, plan to evaluate this once the stack merges — but it is a large, staged landing, so expect churn.
- **Agentic KV reuse is still not solved at the engine level.** Both agent-oriented proposals — Programmatic KV Cache ([#27574](https://github.com/sgl-project/sglang/issues/27574), now closed) and Position-Independent KV Cache Reuse ([#30928](https://github.com/sgl-project/sglang/issues/30928), closed) — went inactive. Agent/RAG workloads with shared system prompts, tool schemas, and retrieved documents at varying offsets still rely on RadixAttention's byte-identical prefix matching; do your own prompt-stability engineering (fixed prefixes, ordered tool schemas) to maximize cache hits.
- **Be conservative with speculative decoding in production right now.** With sampled traffic it can take down the whole server ([#43412](https://github.com/sgl-project/sglang/issues/43412)), and with GLM-5.3 + DFLASH it silently degrades output quality ([#40843](https://github.com/sgl-project/sglang/issues/40843)). Validate on your exact install method (pip bundle vs. container) before enabling.
- **If you run PD disaggregation with role switching, pin to a known-good version** and rehearse the decode → prefill → decode transition ([#43557](https://github.com/sgl-project/sglang/issues/43557)). The Q4 roadmap ([#43699](https://github.com/sgl-project/sglang/issues/43699)) indicates this topology is now the mainstream deployment shape, so fixes should land fast.
- **Gateway users relying on non-standard OpenAI tool types** should make sure their router is on a build with the synced protocol crate ([#30781](https://github.com/sgl-project/sglang/issues/30781)); otherwise Codex-CLI-style clients will get request rejections.
- **Hardware coverage continues to broaden** — NPU prefill CP, XPU prefill graphs, and AMD gfx942/gfx950 all saw activity today, but AMD sparse-decode paths have external dependency risk beyond SGLang's control ([#43765](https://github.com/sgl-project/sglang/issues/43765)).

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

# Ollama Digest — 2026-10-13

## 1. Today’s Highlights
Ollama shipped `v0.40.4-rc0`, updating llama.cpp to b11521 for decision models and EmbeddingGemma 2, and reusing verified manifests/loaded models in inference routes ([PR #18917](https://github.com/ollama/ollama/pull/18917), [PR #18947](https://github.com/ollama/ollama/pull/18947)). The most serious active signal is a reported 25–30% throughput regression across all models vs v0.40.1 ([Issue #18968](https://github.com/ollama/ollama/issues/18968)), alongside multiple MLX SDPA kernel panics on Apple Silicon in 0.40.x ([Issue #18966](https://github.com/ollama/ollama/issues/18966), [Issue #18940](https://github.com/ollama/ollama/issues/18940)). Tooling work continues: an open PR would allow tool calls with structured output, addressing the current `format: json` tool-call breakage ([Issue #18944](https://github.com/ollama/ollama/issues/18944), [PR #18975](https://github.com/ollama/ollama/pull/18975)).

## 2. Releases & Breaking Changes
- **v0.40.4-rc0** — RC targeting v0.40.4. Includes llama.cpp b11521 for decision models and EmbeddingGemma 2, plus server reuse of verified manifests and loaded models in inference routes ([release](https://github.com/ollama/ollama/releases/tag/v0.40.4-rc0), [PR #18917](https://github.com/ollama/ollama/pull/18917), [PR #18947](https://github.com/ollama/ollama/pull/18947)). Release notes are truncated; the MLX change is not fully visible.
- **No explicit breaking API/config changes** were called out in the provided data. Operationally, watch scheduler reload behavior when request `shift` differs from the loaded runner ([Issue #18950](https://github.com/ollama/ollama/issues/18950)).
- **Docs cleanup**: stale `OLLAMA_TMPDIR` and `%TEMP%` references removed in [PR #18970](https://github.com/ollama/ollama/pull/18970); docs restructuring reverted in [PR #18960](https://github.com/ollama/ollama/pull/18960).

## 3. New Model & Hardware Support
- **Decision models + EmbeddingGemma 2**: llama.cpp bumped to b11521; CLI decision-model support added in [PR #18962](https://github.com/ollama/ollama/pull/18962); MLX decision-model JSON state prompt fix in [PR #18974](https://github.com/ollama/ollama/pull/18974).
- **Prism models**: open PR adds Prism model support ([PR #18967](https://github.com/ollama/ollama/pull/18967)); Prism ternary GGUFs (`PQ2_0`/`PTQ1_0`) remain a tracking issue with import failures ([Issue #18521](https://github.com/ollama/ollama/issues/18521)).
- **K2 Horizon family**: feature request for `k2-horizon` architecture, sizes 0.9B–36B MoE ([Issue #18698](https://github.com/ollama/ollama/issues/18698)).
- **Cloud models**: PR introduces `clef` and `clef flash` on cloud ([PR #18969](https://github.com/ollama/ollama/pull/18969)).
- **Legacy glm-ocr GGUFs**: PR registers `<|user|>` as EOT to prevent runaway output ([PR #17195](https://github.com/ollama/ollama/pull/17195)).
- **Windows model storage**: PR finds models through Windows junctions/volume mount points, fixing empty `ollama list` with `OLLAMA_MODELS` on NTFS mount paths ([PR #18949](https://github.com/ollama/ollama/pull/18949), [Issue #18921](https://github.com/ollama/ollama/issues/18921)).
- **Backend/hardware issues**: CUDA illegal memory access on RTX 5090 with Cohere MoE ([Issue #18642](https://github.com/ollama/ollama/issues/18642)); Vulkan access violation on AMD RX 6800 XT ([Issue #18557](https://github.com/ollama/ollama/issues/18557)); Windows CUDA backend `.dll` left as `.tmp` causing CPU fallback ([Issue #18712](https://github.com/ollama/ollama/issues/18712)); MLX SDPA panics on Apple Silicon ([Issue #18966](https://github.com/ollama/ollama/issues/18966), [Issue #18940](https://github.com/ollama/ollama/issues/18940)).

## 4. Performance & Optimization
- **Reported 25–30% throughput regression** across all models vs v0.40.1, with lower tokens/sec ([Issue #18968](https://github.com/ollama/ollama/issues/18968)). No fix PR listed.
- **Prompt timing metric fix**: open PR rates prompt timings by evaluated tokens, fixing inflated `prompt_per_second` on warm prompt cache ([PR #18964](https://github.com/ollama/ollama/pull/18964), [Issue #18948](https://github.com/ollama/ollama/issues/18948)).
- **KV/VRAM accounting**: open PR mirrors GraphSize KV accounting in `PredictServerVRAM`, targeting Qwen loading/memory prediction issues ([PR #17615](https://github.com/ollama/ollama/pull/17615)).
- **Scheduler reload behavior**: proposal to launch shift-capable runners and enforce `shift:false` per request, avoiding reload when `resolveContextShift` differs ([Issue #18950](https://github.com/ollama/ollama/issues/18950)).
- **MLX memory leak**: closed issue reports ~0.43 GiB leaked per tool-calling request outside the prefix-cache budget with `qwen3.6:27b-mlx` ([Issue #18620](https://github.com/ollama/ollama/issues/18620)).
- **KV cache warm-up**: feature request for system-prompt KV cache pre-warm to reduce first-response latency with large system/tool prompts ([Issue #18952](https://github.com/ollama/ollama/issues/18952)).
- **Inference route reuse**: v0.40.4-rc0 reuses verified manifests and loaded models in inference routes ([PR #18947](https://github.com/ollama/ollama/pull/18947)).

## 5. Stability & Regressions
Ranked by severity:
1. **MLX SDPA kernel panics on Apple Silicon (0.40.x)** — “Maximum threads per threadgroup is 896 but requested 1024/960” on M2 Max, M6, etc., with Qwen 3.8, Gemma 4, `qwen3.6:35b-mlx`, and `gemma4:e2b-mlx`. Multiple duplicates: [Issue #18846](https://github.com/ollama/ollama/issues/18846) closed, [Issue #18856](https://github.com/ollama/ollama/issues/18856) closed, [Issue #18940](https://github.com/ollama/ollama/issues/18940) closed, [Issue #18966](https://github.com/ollama/ollama/issues/18966) open, [Issue #18885](https://github.com/ollama/ollama/issues/18885) open. Related multimodal embedding small-`num_ctx` 500 fixed by [PR #18973](https://github.com/ollama/ollama/pull/18973).
2. **25–30% performance regression** vs v0.40.1 across models ([Issue #18968](https://github.com/ollama/ollama/issues/18968)).
3. **Tool calls broken with `format: json`** — model cannot use tool calls when JSON format is set ([Issue #18944](https://github.com/ollama/ollama/issues/18944)). Open PR [PR #18975](https://github.com/ollama/ollama/pull/18975) would allow tool calls with structured output.
4. **CUDA illegal memory access on RTX 5090** with Cohere MoE architecture on Windows ([Issue #18642](https://github.com/ollama/ollama/issues/18642)).
5. **Vulkan crash on AMD RX 6800 XT** — access violation loading any model ([Issue #18557](https://github.com/ollama/ollama/issues/18557)).
6. **Windows CUDA backend auto-update failure** — `ggml-cuda.dll` left as `.tmp`, 0 B VRAM / CPU fallback ([Issue #18712](https://github.com/ollama/ollama/issues/18712)).
7. **Scheduler reloads model on `shift` mismatch** — can cause latency/throughput churn ([Issue #18950](https://github.com/ollama/ollama/issues/18950)).
8. **Windows `ollama list` empty with NTFS mount point** — fix PR open ([Issue #18921](https://github.com/ollama/ollama/issues/18921), [PR #18949](https://github.com/ollama/ollama/pull/18949)).
9. **Qwen3-Coder tool parser corrupts numbers outside int64** — e.g., `1e20` becomes `9223372036854775807` ([Issue #18421](https://github.com/ollama/ollama/issues/18421)).
10. **Cloud regressions**: JSON reply schema ignored by cloud model ([Issue #12362](https://github.com/ollama/ollama/issues/12362)); `deepseek-v4.1-flash:cloud` 500 with image input over ~655,360 tokens ([Issue #18853](https://github.com/ollama/ollama/issues/18853)).
11. **Other platform bugs**: second `ollama run` stuck on Raspberry Pi/Debian 13 ([Issue #18796](https://github.com/ollama/ollama/issues/18796)); Windows GPU discovery console flashes ([Issue #18954](https://github.com/ollama/ollama/issues/18954)); Windows tray server never starts ([Issue #18507](https://github.com/ollama/ollama/issues/18507) closed); Windows Defender false positive ([Issue #2519](https://github.com/ollama/ollama/issues/2519) closed); `manifests-v2` strange aliases ([Issue #18870](https://github.com/ollama/ollama/issues/18870) closed); `ffn_down_exps.weight size overflows` with gpt-oss ([Issue #18869](https://github.com/ollama/ollama/issues/18869) closed).

## 6. What This Means for Application Developers
- **Pin/benchmark before upgrading to 0.40.x**: the 25–30% regression and MLX panics make `v0.40.4-rc0` risky for production Apple Silicon and latency-sensitive deployments. If on MLX, consider staying on 0.35.x until SDPA panics are fixed.
- **Structured output + tools**: if you use `format: json` with tool calls, expect breakage today; track [PR #18975](https://github.com/ollama/ollama/pull/18975). Validate tool-call arguments for numeric precision, especially with Qwen3-Coder ([Issue #18421](https://github.com/ollama/ollama/issues/18421)).
- **Cloud outputs**: JSON schema enforcement is not reliable for some cloud models; validate server-side rather than trusting schema ([Issue #12362](https://github.com/ollama/ollama/issues/12362)).
- **Windows model storage**: if `OLLAMA_MODELS` lives on a junction/mount point, `ollama list` may be empty until [PR #18949](https://github.com/ollama/ollama/pull/18949) lands.
- **Metrics**: the prompt timing fix in [PR #18964](https://github.com/ollama/ollama/pull/18964) will change warm-cache `prompt_per_second`; adjust dashboards accordingly.
- **Agent latency**: track the system-prompt KV cache warm-up request ([Issue #18952](https://github.com/ollama/ollama/issues/18952)) and scheduler shift-reload fix ([Issue #18950](https://github.com/ollama/ollama/issues/18950)) for long-context agent workloads.
- **Parser fixes to watch**: Cohere block ordering ([PR #18965](https://github.com/ollama/ollama/pull/18965)), FunctionGemma inter-call content ([PR #18961](https://github.com/ollama/ollama/pull/18961)), Gemma4 whitespace ([PR #17570](https://github.com/ollama/ollama/pull/17570)), and glm-ocr EOT ([PR #17195](https://github.com/ollama/ollama/pull/17195)) are relevant if your agents depend on those model families.

</details>

<details>
<summary><strong>LiteLLM</strong> — <a href="https://github.com/BerriAI/litellm">BerriAI/litellm</a></summary>

# LiteLLM Digest — 2026-10-13

## Today's Highlights
- No new releases; a critical Redis cache regression in v1.93.0 (#34614) remains open, breaking caching and budget counters.
- Billing correctness improved: Gemini context cache creation tokens now billed (#46144), Azure Claude web search billing fixed for deployment-owned pricing (#46135), and bridge cost parity tests added for gpt-6/Claude 5 (#46139, #46140, #46143, #46141, #46130, #46137).
- MCP gateway gets progress forwarding and prompt cancellation (#46134); Anthropic `/v1/messages` now maps 503 to 529 `overloaded_error` (#46063).

## Releases & Breaking Changes
- **No new releases.**
- **v1.93.0 regression:** Redis cache fails with `TypeError: AbstractConnection.__init__() got an unexpected keyword argument 'ssl_check_hostname'`. Breaks caching and budget counters. [Issue #34614](https://github.com/BerriAI/litellm/issues/34614)
- **v1.92 regression:** Missing `budget_fallbacks` column causes fatal issues (no Admin UI login, etc.). [Issue #33074](https://github.com/BerriAI/litellm/issues/33074)
- **Pinned OpenTelemetry SDK 1.28.0** blocking upgrades; request to move to 1.40 line or relax range. [Issue #29407](https://github.com/BerriAI/litellm/issues/29407)

## New Model & Hardware Support
- No new model or hardware support announcements in this window. (Bedrock autodiscovery feature request closed: [Issue #43568](https://github.com/BerriAI/litellm/issues/43568))

## Performance & Optimization
- **Prometheus multiproc dir isolation:** Each proxy boot now gets its own auto-created `PROMETHEUS_MULTIPROC_DIR`, preventing one proxy from wiping another's live counters. [PR #42660](https://github.com/BerriAI/litellm/pull/42660)
- **MCP gateway progress/cancellation:** Progress messages forwarded through originating SDK channel; HTTP cancellation now promptly cancels upstream work. [PR #46134](https://github.com/BerriAI/litellm/pull/46134)
- **Pass-through endpoint CPU fix:** Unbounded registry growth causing 100% CPU is closed. [Issue #26081](https://github.com/BerriAI/litellm/issues/26081)
- No concrete throughput/latency numbers reported.

## Stability & Regressions
Ranked by severity:
1. **Redis cache failure (v1.93.0)** — breaks caching and budget counters. Open. [Issue #34614](https://github.com/BerriAI/litellm/issues/34614)
2. **budget_fallbacks column missing (v1.92)** — fatal Admin UI issues. Open. [Issue #33074](https://github.com/BerriAI/litellm/issues/33074)
3. **Proxy-admin JWT bypasses personal max_budget** — security/billing bypass. Closed. [Issue #41226](https://github.com/BerriAI/litellm/issues/41226)
4. **Streaming 500: `NoneType + int`** when backend chunk omits `choices[].index`. Open. [Issue #46047](https://github.com/BerriAI/litellm/issues/46047)
5. **websearch_interception leaks max_parallel_requests slot** per streamed `/v1/messages` with `web_search` tool. Open. [Issue #46093](https://github.com/BerriAI/litellm/issues/46093)
6. **SSE error codes outside 100-599 dropped** — router fallback/cooldown never triggers. Open. [Issue #31284](https://github.com/BerriAI/litellm/issues/31284)
7. **Deployment custom pricing ignored** after model-cost-map reload (`input_cost_per_token: 0`). Open. [Issue #30081](https://github.com/BerriAI/litellm/issues/30081)
8. **Bedrock audio transcription HTTP 400** due to system message incompatible with audio blocks. Open. [Issue #41544](https://github.com/BerriAI/litellm/issues/41544)
9. **A2AConfig drops temperature/max_tokens/top_p** silently. Open. [Issue #45944](https://github.com/BerriAI/litellm/issues/45944)
10. **Key expires timestamp ignored** — persisted expiry derived from duration instead. Open. [Issue #45236](https://github.com/BerriAI/litellm/issues/45236)
11. **Non-admin users cannot set max_budget on personal keys via UI.** Open. [Issue #33212](https://github.com/BerriAI/litellm/issues/33212)
12. **Master key auth error missing client IP.** Open. [Issue #33150](https://github.com/BerriAI/litellm/issues/33150)
13. **Model alias permission denied before alias resolution.** Open. [Issue #25281](https://github.com/BerriAI/litellm/issues/25281)
14. **Codex CLI Responses API tool-call state not closed correctly.** Open. [Issue #30603](https://github.com/BerriAI/litellm/issues/30603)
15. **Flaky MCP schema validation test** (~25% failure). Open. [Issue #46071](https://github.com/BerriAI/litellm/issues/46071)

Fixed/closed today:
- Gemini 3 temperature injection removed ([Issue #38663](https://github.com/BerriAI/litellm/issues/38663), [Issue #45333](https://github.com/BerriAI/litellm/issues/45333))
- Gemini tool-use prompt tokens now billed ([Issue #45991](https://github.com/BerriAI/litellm/issues/45991))
- MCP binary resources returned correctly ([Issue #32330](https://github.com/BerriAI/litellm/issues/32330))
- MCP OAuth 401 + RFC 9728 metadata ([Issue #31296](https://github.com/BerriAI/litellm/issues/31296))
- Pass-through endpoint CPU ([Issue #26081](https://github.com/BerriAI/litellm/issues/26081))
- Minimax coding plan API auth ([Issue #24483](https://github.com/BerriAI/litellm/issues/24483))

## What This Means for Application Developers
- **Upgrade carefully:** v1.93.0 Redis bug and v1.92 budget_fallbacks issue can break caching and Admin UI. Test before deploying.
- **Billing accuracy:** If you use Gemini context caching or Azure Claude web search, ensure you're on a build with the latest billing fixes; underbilling/overbilling was present.
- **MCP users:** The gateway now forwards progress and cancels upstream work promptly — better UX and resource utilization.
- **Anthropic /v1/messages:** 503 responses now map to 529 overloaded_error, aligning with Anthropic's contract and improving Claude Code retry behavior.
- **Observability:** W3C `traceresponse` header on traced responses ([PR #42079](https://github.com/BerriAI/litellm/pull/42079)) helps correlate gateway requests with traces.
- **Cost tracking:** Batch JSONL line items can be stored in callbacks ([PR #41691](https://github.com/BerriAI/litellm/pull/41691)) for per-request cost reconciliation.
- **Guardrails:** Wingback guardrail integration added ([PR #46136](https://github.com/BerriAI/litellm/pull/46136)).
- **Watch open bugs:** Key expiration timestamps, non-admin max_budget UI, model alias permissions, and streaming `choices[].index` issues may affect production; follow linked issues for fixes.

</details>

<details>
<summary><strong>Unsloth</strong> — <a href="https://github.com/unslothai/unsloth">unslothai/unsloth</a></summary>

# Unsloth Digest — 2026-10-13

Source: [github.com/unslothai/unsloth](https://github.com/unslothai/unsloth)

## 1. Today's Highlights

**v0.1.906-beta** lands two headline capabilities: agentic **Browser Use** (models can open, read, click, type and scroll pages) and **Qwen-Image-2.1-Turbo** (8-step image generation), plus text-diffusion LLM training, faster MoE GGUFs and speculative decoding. In parallel, the browser agent gained low-level control — coordinate clicks, mouse moves, drags and approved script execution ([PR #13505](https://github.com/unslothai/unsloth/pull/13505)) — and LTX-2.5 fp8/int8 now loads pre-quantized DiTs instead of quantizing a 38 GB bf16 transformer at load ([PR #13516](https://github.com/unslothai/unsloth/pull/13516)). The most consequential open regression remains repeated full-context reprocessing after every tool call in Studio ([Issue #10698](https://github.com/unslothai/unsloth/issues/10698)).

## 2. Releases & Breaking Changes

- **v0.1.906-beta** — Browser Use + Qwen-Image-2.1-Turbo. Also ships text-diffusion LLM training, faster MoE GGUFs, and speculative decoding (release note truncated in source). No migration steps required.
- **Studio CLI: `--models-dir PATH`** (repeatable) added to `unsloth studio` for pointing at local model folders; landed via [PR #13435](https://github.com/unslothai/unsloth/pull/13435), closes [Issue #8250](https://github.com/unslothai/unsloth/issues/8250).
- **Credential behavior change:** passing `token=` to `FastLanguageModel`/`FastModel.from_pretrained` previously overwrote the `hf auth login` session. That is fixed in [PR #13486](https://github.com/unslothai/unsloth/pull/13486) — previously a read token passed for a gated model could silently break subsequent pushes.
- **Docker:** `UNSLOTH_STUDIO_KEEP_LEGACY=0` now propagates through `docker/run.sh` ([PR #13488](https://github.com/unslothai/unsloth/pull/13488)).
- **API gap (still open):** per-quantization selection is not exposed in the Model List API, so Q8 vs Q4_K_M cannot be chosen programmatically — [Issue #9340](https://github.com/unslothai/unsloth/issues/9340).
- **Test/CI-adjacent:** Studio launcher now supports Kaggle-backed notebooks with Cloudflare quick tunnel ([PR #6987](https://github.com/unslothai/unsloth/pull/6987)).

## 3. New Model & Hardware Support

- **Qwen-Image-2.1-Turbo** — image generation in 8 steps; shipped in v0.1.906-beta. A related classification bug (`general.architecture=qwen_image21` diffusion GGUFs filed as text-generation and hidden from the Images page, AMD ROCm) is closed: [Issue #13328](https://github.com/unslothai/unsloth/issues/13328).
- **LTX-2.5 fp8/int8** — loads now seeded from hosted pre-quantized DiTs (`unsloth/LTX-2.5-FP8`) instead of quantizing at runtime ([PR #13516](https://github.com/unslothai/unsloth/pull/13516)); VAE fixture, catalog row and Video page defaults in [PR #13500](https://github.com/unslothai/unsloth/pull/13500).
- **Text-diffusion LLM training** and faster **MoE GGUF** support shipped in v0.1.906-beta.
- **Non-CUDA accelerators:** vLLM resume/teardown now releases device cache through the active accelerator (NPU/XPU) rather than silently no-op'ing off CUDA — [PR #13438](https://github.com/unslothai/unsloth/pull/13438).
- **Local model providers:** On-Device detection now covers LM Studio, Ollama, custom folders, `./models`, Hermes and oMLX with working reveal/delete — [PR #13507](https://github.com/unslothai/unsloth/pull/13507). Audio.cpp umbrella-repo models (`audio-cpp/audio.cpp-gguf`) now list/reveal/delete by folder — [PR #13519](https://github.com/unslothai/unsloth/pull/13519).
- **Requested, not yet supported:** Ling 3.0 in Studio ([Issue #8532](https://github.com/unslothai/unsloth/issues/8532), 👍5); Gemma4e2b on Windows ([Issue #13388](https://github.com/unslothai/unsloth/issues/13388)); multiple concurrent servers with separate llama.cpp configs ([Issue #8488](https://github.com/unslothai/unsloth/issues/8488), closed); custom `.crt`/`.key` SSL instead of Cloudflare-only HTTPS ([Issue #8716](https://github.com/unslothai/unsloth/issues/8716)); MLX pretraining/corpus-packing structure ([Issue #8607](https://github.com/unslothai/unsloth/issues/8607)).

## 4. Performance & Optimization

- **Speculative decoding** and **faster MoE GGUF** paths in v0.1.906-beta (release note truncated; no published benchmarks).
- **8-step image generation** for Qwen-Image-2.1-Turbo — meaningful step-count reduction for diffusion serving.
- **LTX-2.5 load time:** avoids quantizing the 38 GB bf16 transformer at load, cutting peak memory and startup work ([PR #13516](https://github.com/unslothai/unsloth/pull/13516)).
- **Regression — context reprocessing:** after every tool call the full context appears to be reprocessed; at ~30k tokens this means multi-minute waits per tool call, attributed to a change introduced since a prior Studio update — [Issue #10698](https://github.com/unslothai/unsloth/issues/10698). No fix PR linked.
- **Regression — CPU saturation:** 100% CPU spikes during token generation, tracked since [Issue #6046](https://github.com/unslothai/unsloth/issues/6046).
- **VRAM/RAM offload not honored on ROCm:** with "No RAM Offload" checked, the model is still unloaded from VRAM and parked in RAM on AMD ROCm — [Issue #10341](https://github.com/unslothai/unsloth/issues/10341).
- **UI jank:** large code blocks cause interface lag ([Issue #10769](https://github.com/unslothai/unsloth/issues/10769)); Library card hover/selection contrast and time-pill centering adjusted ([PR #13509](https://github.com/unslothai/unsloth/pull/13509), [#13518](https://github.com/unslothai/unsloth/pull/13518)).

## 5. Stability & Regressions

Ranked by severity; fix status noted.

1. **Tool-call context reprocessing (performance regression, no fix)** — minutes of stall per tool call at long context. [Issue #10698](https://github.com/unslothai/unsloth/issues/10698)
2. **OOM at long context on large MoE** — Qwen2.5 72B 4-bit OOMs at 32k on A800 while 24k is stable; contradicts published 80k+ claims for Llama 3.3 70B. [Issue #2937](https://github.com/unslothai/unsloth/issues/2937)
3. **Desktop drag & drop broken** — files silently not attached on drop, no error surfaced. [Issue #13473](https://github.com/unslothai/unsloth/issues/13473)
4. **Desktop AppImage unusable window sizing on Wayland/GNOME/Arch** — cannot resize, frame-rate issues, multi-monitor. [Issue #12845](https://github.com/unslothai/unsloth/issues/12845)
5. **Chat client crashes** — `tapClientLookup: Index 1 out of bounds`, malformed message parts, duplicate `toolCallId` on "New chat". [Issue #10288](https://github.com/unslothai/unsloth/issues/10288)
6. **`UNSLOTH_RETURN_LOGITS=1` silently reset to `0` during training** — breaks custom validation metrics on Gemma 3. Marked fixed, pending confirmation. [Issue #3071](https://github.com/unslothai/unsloth/issues/3071)
7. **Blackwell (RTX 5090) startup failure** — `PY_SSIZE_T_CLEAN macro must be defined`; closed. [Issue #2760](https://github.com/unslothai/unsloth/issues/2760)
8. **Qwen3 `deallocating None` after minutes** — conflict between gradient checkpointing and memory management; closed. [Issue #2955](https://github.com/unslothai/unsloth/issues/2955)
9. **vLLM backend fails to load** on local RTX 5060 Ti (`vllm_process failed to load!`); closed. [Issue #2963](https://github.com/unslothai/unsloth/issues/2963)
10. **Model loading failures** — `unsloth/GLM-OCR` ([Issue #4269](https://github.com/unslothai/unsloth/issues/4269)) and `ggml-org/sesame-csm-1b-GGUF` ([Issue #9430](https://github.com/unslothai/unsloth/issues/9430)).
11. **Quality regressions** — Qwen3 reasoning ability disappears post-fine-tune ([Issue #3005](https://github.com/unslothai/unsloth/issues/3005)); Gemma4-31B-it degenerates into repetition ([Issue #5124](https://github.com/unslothai/unsloth/issues/5124)); fine-tuned Whisper-large-v3 underperforms base on Persian ([Issue #2942](https://github.com/unslothai/unsloth/issues/2942)).
12. **Misc Studio breakage** — dataset download fails ([Issue #9775](https://github.com/unslothai/unsloth/issues/9775)), macOS text-encoding errors ([Issue #8594](https://github.com/unslothai/unsloth/issues/8594)), LoftQ training error on qwen2.5-derived models ([Issue #6958](https://github.com/unslothai/unsloth/issues/6958)), no auto-load with OpenCode ([Issue #9171](https://github.com/unslothai/unsloth/issues/9171)).

**Security fixes landed today:**
- Symlink-following prevented when linking files into a chat's tool workspace (from the latest security scan) — [PR #13513](https://github.com/unslothai/unsloth/pull/13513).
- HF auth token no longer clobbered by `from_pretrained(token=...)` — [PR #13486](https://github.com/unslothai/unsloth/pull/13486).

## 6. What This Means for Application Developers

- **Browser Use makes Unsloth a viable local agent runtime.** Models can now navigate, click by ref *or* coordinate, drag and run approved scripts ([PR #13505](https://github.com/unslothai/unsloth/pull/13505)). If you build browser agents, evaluate this against headless-Chrome stacks — but budget for the context-reprocessing regression (#10698), which makes long-context tool loops expensive today.
- **Tool-calling workloads need a context-management strategy now.** The reprocessing regression plus the new "Context compacted" inline divider ([PR #13502](https://github.com/unslothai/unsloth/pull/13502)) signal that prompt caching across tool turns is not yet reliable. Measure TTFT per tool call before scaling agent loops.
- **Multimodal serving is broadening fast.** Qwen-Image-2.1-Turbo (8 steps), LTX-2.5 fp8/int8 pre-quantized DiTs and text-diffusion training mean one runtime increasingly covers image, video and audio pipelines — audio.cpp and whisper.cpp update flows are being consolidated into a single card ([PR #13497](https://github.com/unslothai/unsloth/pull/13497)).
- **Pin your versions if you rely on quantization selection.** The Model List API still cannot express per-quantization targets ([Issue #9340](https://github.com/unslothai/unsloth/issues/9340)); clients that resolve models by repo and precision need an out-of-band mapping.
- **Non-CUDA deployments are getting real attention** (NPU/XPU cache release in [PR #13438](https://github.com/unslothai/unsloth/pull/13438)), but AMD/ROCm users still report offload and diffusion-classification bugs (#10341, #13328) — validate memory behavior explicitly on ROCm rather than trusting the offload toggle.

</details>

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*