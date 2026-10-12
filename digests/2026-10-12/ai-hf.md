# Hugging Face 热门模型周报 2026-10-12

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-10-12 08:27 UTC

---

## 今日亮点

今日 Hugging Face 热门榜单由 Qwen 的开源权重生态领跑：**Qwen/Qwen3.8-27B** 以 17,530 个点赞和 6,843,108 次下载位居榜首，而 **Qwen3.8-Flash-Next** 及多个 unsloth/ISTA GGUF 转换版本显示出对本地推理的强烈需求。视频生成也是一个主要主题，**Lightricks/LTX-2.5** 获得了 7,354 个点赞和 1,726,683 次下载。社区量化持续加速，涵盖 GSQ-RCO 混合精度、三值 2 比特 GGUF 以及未经审查/消融的 Qwen-Image 和 Qwen3.8 变体。Cloudflare 的 Clef 家族以及 **facebook/sam3** 和 **convaiinnovations/laya** 等专用模型，为纯文本 LLM 之外的领域增添了多样性。

## 热门模型

### 🧠 语言模型

| 模型 | 作者 | 点赞 | 下载 | 简介 |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 17,530 | 6,843,108 | 旗舰级 Qwen3.8 多模态对话模型，支持图文到文本。它以点赞数和下载量领跑本期摘要，是值得关注的核心开源权重 Qwen 发布。 |
| [Qwen/Qwen3.8-Flash-Next](https://huggingface.co/Qwen/Qwen3.8-Flash-Next) | Qwen | 6,122 | 1,870,465 | Qwen 下一代 Flash 系列多模态对话模型，标记为 qwen4_exp。它势头强劲，获得 6,122 个点赞和 1,870,465 次下载。 |
| [deepseek-ai/DeepSeek-V4.1-Flash](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash) | deepseek-ai | 4,381 | 1,382,367 | DeepSeek 的 V4.1 Flash 开源模型，用于文本生成和图文到文本。作为快速的旗舰替代方案，它吸引了 4,381 个点赞和 1,382,367 次下载。 |
| [Cloudflare/clef](https://huggingface.co/Cloudflare/clef) | Cloudflare | 2,047 | 14,351 | Cloudflare 基于 Qwen3.5 构建的 Clef 图文到文本模型。它在 Clef 家族中点赞数最高，表明人们对 Cloudflare 的多模态 LLM 入门产品有浓厚兴趣。 |
| [Aleph-Alpha/Kolibri-1](https://huggingface.co/Aleph-Alpha/Kolibri-1) | Aleph-Alpha | 898 | 12,550 | 专注于推理的 MoE 文本生成模型，支持 vLLM。它作为高效推理工作负载的紧凑替代方案而流行。 |
| [Cloudflare/clef-flash](https://huggingface.co/Cloudflare/clef-flash) | Cloudflare | 769 | 22,123 | 面向图文到文本任务的低延迟 Clef Flash 变体。它作为 Clef 的更快伴侣而流行，下载量为 22,123 次。 |
| [LiquidAI/d1-3B](https://huggingface.co/LiquidAI/d1-3B) | LiquidAI | 315 | 10,843 | 3B 规模的小型 Liquid LFM2-VL 图文到文本模型。作为轻量级多模态聊天和设备端候选方案，它引起了关注。 |

### 🎨 多模态与生成

| 模型 | 作者 | 点赞 | 下载 | 简介 |
| :--- | :--- | ---: | ---: | :--- |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 7,354 | 1,726,683 | Lightricks 的 LTX-2.5 视频模型支持图像到视频、文本到视频和视频到视频。它是突出的视频生成发布，获得 7,354 个点赞和 1,726,683 次下载。 |
| [Qwen/Qwen-Image-2.1](https://huggingface.co/Qwen/Qwen-Image-2.1) | Qwen | 3,244 | 136,103 | Qwen-Image 2.1 家族中的官方 Qwen 图像生成和编辑模型。它因高质量的文本到图像工作流而流行，下载量为 136,103 次。 |
| [Qwen/Qwen-Image-2.1-Turbo](https://huggingface.co/Qwen/Qwen-Image-2.1-Turbo) | Qwen | 570 | 4,702 | Qwen-Image 2.1 的 Turbo 变体，用于更快的文本到图像和图像编辑扩散。它因低延迟生成而受到关注，但下载量仍处于早期阶段，为 4,702 次。 |
| [canberkkkkkk/ema-lightning](https://huggingface.co/canberkkkkkk/ema-lightning) | canberkkkkkk | 367 | 18,079 | 专注于语音合成的土耳其语文本转语音模型。它作为社区 TTS 选项而流行，下载量为 18,079 次。 |
| [Cactus-Compute/whistle](https://huggingface.co/Cactus-Compute/whistle) | Cactus-Compute | 349 | 16,434 | 来自 Cactus Compute 的设备端自动语音识别模型。它因在 cactus-needle 标签下高效的语音转文本而受到关注。 |
| [speridlabs/iris-3b](https://huggingface.co/speridlabs/iris-3b) | speridlabs | 173 | 393 | 3B 规模的像素空间文本到图像扩散模型。这是一个早期发布，有 393 次下载，人们对非潜在图像生成感兴趣。 |
| [FrancisRing/Prism](https://huggingface.co/FrancisRing/Prism) | FrancisRing | 172 | 0 | 使用视频扩散变换器的联合视频-音频生成模型。它目前有 0 次下载，但作为研究预览因点赞而流行。 |

### 🔧 专用模型

| 模型 | 作者 | 点赞 | 下载 | 简介 |
| :--- | :--- | ---: | ---: | :--- |
| [convaiinnovations/laya](https://huggingface.co/convaiinnovations/laya) | convaiinnovations | 5,507 | 44,088 | 用于校准的系统一决策的文本分类模型。它获得了 5,507 个点赞和 44,088 次下载，是一个值得注意的专用决策发布。 |
| [facebook/sam3](https://huggingface.co/facebook/sam3) | facebook | 4,099 | 2,081,596 | Meta 的 SAM3 模型，用于掩码生成和视频分割。作为基础视觉工具，它以 4,099 个点赞和 2,081,596 次下载而强劲流行。 |
| [google/embeddinggemma-2](https://huggingface.co/google/embeddinggemma-2) | google | 1,795 | 98,829 | Google 的 EmbeddingGemma 2 模型，用于特征提取和嵌入。它是一个领先的嵌入发布，获得 1,795 个点赞和 98,829 次下载。 |

### 📦 微调与量化

| 模型 | 作者 | 点赞 | 下载 | 简介 |
| :--- | :--- | ---: | ---: | :--- |
| [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF) | unsloth | 5,038 | 6,488,262 | Unsloth 对 Qwen3.8-27B 的 GGUF 量化，用于本地推理。它获得了 5,038 个点赞和 6,488,262 次下载，显示出对量化 Qwen3.8 访问的巨大需求。 |
| [abenzerps/Qwen-Image-2.1-Uncensored-Quantized](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-Quantized) | abenzerps | 4,022 | 2,188,658 | 社区量化的、未经审查的 Qwen-Image 2.1 模型，用于 ComfyUI 图像生成。它获得了 4,022 个点赞和 2,188,658 次下载，成为顶级的社区图像模型。 |
| [prism-ml/Ternary-Bonsai-2-27B-gguf](https://huggingface.co/prism-ml/Ternary-Bonsai-2-27B-gguf) | prism-ml | 2,642 | 4,549,911 | 用于 llama.cpp 和本地文本生成的三值 2 比特 GGUF 模型。它因极端量化而流行，获得 2,642 个点赞和 4,549,911 次下载。 |
| [ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF) | ISTA-DASLab | 2,166 | 1,489,676 | Qwen3.8-27B 的 GSQ-RCO 混合精度 GGUF 量化。它结合了高点赞数和 1,489,676 次下载，表明对优化 27B 推理的需求。 |
| [DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF](https://huggingface.co/DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF) | DavidAU | 1,669 | 2,016,567 | DavidAU 高度合并的、未经审查的 Qwen3.8-27B 编码器 GGUF，带有 Heretic 调优。它获得了 1,669 个点赞和 2,016,567 次下载，反映出对角色扮演/编码合并的强烈兴趣。 |
| [jialinyyzz/humanizer](https://huggingface.co/jialinyyzz/humanizer) | jialinyyzz | 1,178 | 37,647 | 基于 Gemma4-Unified 的人性化模型，以 GGUF 格式分发用于文本生成。它作为使 AI 文本更自然的社区工具而流行，下载量为 37,647 次。 |
| [ISTA-DASLab/Qwen3.8-Flash-Next-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/Qwen3.8-Flash-Next-GSQ-RCO-GGUF) | ISTA-DASLab | 810 | 4,189,006 | Qwen3.8-Flash-Next 的量化 GSQ-RCO GGUF 版本。尽管点赞数适中，但它有 4,189,006 次下载，表明大量本地推理使用。 |
| [orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF](https://huggingface.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF) | orcarouter | 779 | 614,193 | Qwen3.8-Flash-Next 的未经审查、消融的 GGUF 量化。它因不受限制的本地聊天而流行，获得 779 个点赞和 614,193 次下载。 |
| [Venastine-Research/Xing4.0-29B-A4B-GGUF](https://huggingface.co/Venastine-Research/Xing4.0-29B-A4B-GGUF) | Venastine-Research | 701 | 48,389 | Xing4.0 29B A4B 文本生成模型，随 GGUF 支持一起发布。它作为可本地运行的 MoE 风格替代方案引起了社区兴趣。 |
| [AtomicChat/Qwen-Image-2.1-Turbo-Uncensored-GGUF](https://huggingface.co/AtomicChat/Qwen-Image-2.1-Turbo-Uncensored-GGUF) | AtomicChat | 348 | 100,530 | Qwen-Image 2.1 Turbo 的未经审查 GGUF 构建，用于 ComfyUI 工作流。它因本地图像生成而流行，下载量为 100,530 次。 |
| [ConwayResearch/Underdog-Saluki-27B-1.0](https://huggingface.co/ConwayResearch/Underdog-Saluki-27B-1.0) | ConwayResearch | 339 | 108,900 | 兼容 Llama.cpp 的 2 比特 27B 模型，专注于工具和函数调用。它有 108,900 次下载，显示出对低位代理模型的需求。 |
| [unsloth/embeddinggemma-2-GGUF](https://huggingface.co/unsloth/embeddinggemma-2-GGUF) | unsloth | 271 | 59,274 | Unsloth 对 EmbeddingGemma 2 的 GGUF 量化，用于多模态嵌入和特征提取。它作为本地嵌入选项而流行，下载量为 59,274 次。 |
| [nerkyor/Qwen3.8-27B-Coder390-EfficientThink-Opus5.5-GPT6Astra-Grok4.7-DSV4Pro-K3-SFT-RLOO-MTP-DFlash2](https://huggingface.co/nerkyor/Qwen3.8-27B-Coder390-EfficientThink-Opus5.5-GPT6Astra-Grok4.7-DSV4Pro-K3-SFT-RLOO-MTP-DFlash2) | nerkyor | 257 | 56,525 | 面向编码、高效思考和推理的社区 Qwen3.8-27B 微调。尽管合并模型名称非常长，但它有 56,525 次下载。 |

## 生态信号

榜单由 Qwen 家族衍生模型主导：Qwen3.8-27B、Qwen3.8-Flash-Next、Qwen-Image 2.1 及其量化/微调兄弟姐妹占据了许多高下载量位置。开源权重势头明显：Qwen、DeepSeek、Meta、Google、Lightricks 和 Cloudflare 都发布了可下载模型，而专有 API 未出现在此热门快照中。量化是最强的基础设施信号，GGUF、GSQ-RCO 混合精度、三值 2 比特和 2 比特 llama.cpp 构建吸引了数百万次下载。社区微调也通过未经审查、消融、Heretic、编码器和角色扮演合并保持活跃，尤其是在 Qwen3.8 和 Qwen-Image 周围。视频和音频生成正在超越文本多样化：LTX-2.5 引领视频，而 TTS 和 ASR 模型如 ema-lightning 和 whistle 显示出对设备端语音的需求。嵌入和分割模型如 EmbeddingGemma 2 和 SAM3 完善了聊天之外的生态系统。

## 值得探索

- **[Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B)** — 最清晰地了解当前开源权重旗舰能力，拥有大量点赞和下载，以及多模态图文到文本支持。
- **[Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5)** — 本周最强的视频生成信号，涵盖图像到视频、文本到视频和视频到视频工作流。
- **[prism-ml/Ternary-Bonsai-2-27B-gguf](https://huggingface.co/prism-ml/Ternary-Bonsai-2-27B-gguf)** — 值得研究极端三值 2 比特量化以及在本地运行 27B 级模型的权衡。

---
*本日报由 [agents-radar](https://github.com/creativemango/agents-radar) 自动生成。*