# AI Open Source Trends 2026-10-14

> Sources: GitHub Trending + GitHub Search API | Generated: 2026-10-14 01:21 UTC

---

# AI Open Source Trends Report — 2026-10-14

**Filter note:** Of today's 9 trending repos, 6 are AI-relevant. Excluded as non-AI/general tooling: `bilawalsidhu/gods-eye-view` (browser satellite simulator), `storytold/artcraft` (creative crafting engine), `xtool-org/xtool` (Xcode replacement). From the 80-repo topic search, classical ML/CV and general-purpose entries (TensorFlow, scikit-learn, Julia, netdata, CS course lists, YOLO, tesseract, qlib, etc.) were folded in only where they carry clear AI-platform weight.

---

## 1. Today's Highlights

The dominant story today is **agent scaffolding, not models**: `morluto/rea` pulled in an extraordinary **+54,679 stars in a single day** for agent-driven reverse engineering, while a dense cluster of context/memory utilities (`context-mode`, `headroom`, `claude-mem`, `mem0`) converged on the same problem — shrinking and persisting agent context. A second wave is **"agent skills" as a distribution format**: `vercel-labs/skills` ships `npx skills`, and `diagram-design` explicitly targets Claude Code, Codex, Copilot, Cursor, Factory Droid and Pi in one package. **Local-first AI apps** are also surging — `VoiceStudio` (+1,748) as a fully local ElevenLabs replacement across 646 languages, and `AirLLM` (+1,001) promising 2.8T-parameter inference on a single 4GB GPU. Meanwhile, RAG is visibly diversifying away from plain vector search toward knowledge graphs, "vectorless" reasoning indexes and embedded databases.

---

## 2. Top Projects by Category

### 🔧 AI Infrastructure

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 183,061 | Local model runtime that has become the default on-ramp for open-weight LLMs; the current model list (Kimi, GLM, MiniMax, DeepSeek, gpt-oss, Qwen, Gemma) is a live index of how fast new weights reach local machines. Still the highest-star inference tool in the dataset. |
| [lyogavin/airllm](https://github.com/lyogavin/airllm) | Jupyter Notebook | 0 (+1,001) | Claims 2.8T-parameter LLM inference on a single 4GB GPU via layer-wise loading. Worth attention because it attacks the memory wall that keeps frontier-scale inference inside datacenters, and it added ~1,000 stars today alone. |
| [mksglu/context-mode](https://github.com/mksglu/context-mode) | TypeScript | 0 (+969) | Context-window optimization for coding agents: sandboxes tool output (claimed 98% reduction), persists session memory, and routes across 17 platforms via MCP + hooks. Its ~1k daily stars show context economy is now a first-class infrastructure layer. |
| [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) | Python | 75,263 | Compresses tool outputs, logs, files and RAG chunks before they hit the LLM — 20% fewer tokens for coding agents, 60–95% for JSON. Ships as library, proxy and MCP server, making token reduction deployable without code changes. |
| [vercel-labs/skills](https://github.com/vercel-labs/skills) | TypeScript | 0 (+190) | The open "agent skills" tool, installable via `npx skills`. Signals that skills are being standardized as a package format rather than hand-written prompts, with a major vendor backing the convention. |
| [0xPlaygrounds/rig](https://github.com/0xPlaygrounds/rig) | Rust | 8,870 | Modular, scalable LLM application framework in Rust. Notable as part of the clear Rust-ward shift in agent/inference infrastructure (alongside lancedb, qdrant and Codewhale). |
| [microsoft/multilspy](https://github.com/microsoft/multilspy) | Python | 612 | LSP client library for building language-server-powered applications — the plumbing behind code-aware agents. Low stars but strategic: it is Microsoft's substrate for IDE-grade code intelligence in AI tooling. |

### 🤖 AI Agents / Workflows

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [morluto/rea](https://github.com/morluto/rea) | TypeScript | 208,310 (+54,679) | Agentic reverse engineering: from app behaviour down to native binaries. Today's runaway #1 with +54,679 stars — the strongest single-day momentum signal in the entire dataset and evidence that agents are moving into security/binary analysis. |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 278,622 | "Agent harness performance optimization" system covering skills, instincts, memory, security and research-first development for Claude Code, Codex, Opencode and Cursor. Its 278k stars make it the largest agent-tooling repo in the set, and it defines the emerging "harness engineering" niche. |
| [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) | Python | 253,500 | A general-purpose personal agent that "grows with you," from a lab with a strong open-model pedigree. Its scale shows demand for vendor-neutral, long-lived personal agents rather than single-session assistants. |
| [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) | JavaScript | 162,776 | Makes an AI agent reason like "the laziest senior dev in the room" — minimal, code-you-never-wrote output. A behavioural-tuning skill rather than a framework, and a good example of the new prompt/behaviour-pack genre. |
| [langgenius/dify](https://github.com/langgenius/dify) | TypeScript | 158,388 | Agentic workflow and RAG pipeline builder with broad model/tool support, deployable cloud, VPC or self-hosted. Remains the reference "prototype-to-production" agent platform for teams. |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | Python | 117,886 | Agents that drive the browser. Sustained 100k+ stars confirm browser control has become the default action surface for general-purpose agents, rather than an experimental capability. |
| [Panniantong/Agent-Reach](https://github.com/Panniantong/Agent-Reach) | Python | 97,003 | One CLI giving agents read/search access to Twitter, Reddit, YouTube, GitHub, Bilibili and XiaoHongShu with zero API fees. Notable for solving the data-access bottleneck that gates most social/web agents. |
| [HKUDS/nanobot](https://github.com/HKUDS/nanobot) | Python | 48,999 | Ultra-lightweight self-hosted personal agent framework with WebUI, tools, memory, MCP, multi-agent workflows and chat-app integration. Represents the "small, hackable, self-hosted" counter-trend to heavy harness platforms. |

### 📦 AI Applications

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [debpalash/VoiceStudio](https://github.com/debpalash/VoiceStudio) | Python | 0 (+1,748) | Fully local ElevenLabs alternative: voice cloning, voice design, video dubbing, dictation, transcription and audiobook creation in 646 languages. Its +1,748 daily stars show privacy-preserving voice AI is now a mainstream demand, not a niche. |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | Python | 155,582 | User-friendly AI interface supporting Ollama, OpenAI-compatible APIs and more. Still the most-starred end-user AI UI, and the front door for most self-hosted LLM deployments. |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 130,531 | One-click HD short-video generation from a topic or keyword using LLM-driven automated workflows. A persistent top performer showing the durability of autonomous media-generation pipelines. |
| [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) | JavaScript | 67,034 | Local-first agent and document-chat workspace with an explicit "own your intelligence" pitch. Reflects the privacy/ownership framing now shared by VoiceStudio, open-webui and LEANN. |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | TypeScript | 52,600 | AI productivity studio with smart chat, autonomous agents and 300+ assistants unified across frontier LLMs. Notable for bundling agent capability into a consumer-grade desktop client. |
| [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) | Python | 66,302 | LLM-powered multi-market stock analysis with multi-source quotes, live news, dashboards and scheduled zero-cost runs. A strong example of vertical "personal analyst" agents replacing dashboards. |
| [hugohe3/ppt-master](https://github.com/hugohe3/ppt-master) | Python | 61,682 | Turns documents or topics into native PowerPoint decks with real shapes, transitions, charts and audio narration from speaker notes. Document-to-deck is proving one of the most reliably monetizable agent verticals. |
| [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) | HTML | 0 (+1,815) | Editorial diagram design pack for Claude Code, Codex, Copilot, Cursor, Factory Droid and Pi — 44 diagram types as self-contained HTML + SVG. At +1,815 stars today it shows agent-consumable design assets can trend as hard as code. |

### 🧠 LLMs / Training

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [f/prompts.chat](https://github.com/f/prompts.chat) | HTML | 172,874 | The community prompt library formerly known as Awesome ChatGPT Prompts, self-hostable for private org use. Still the largest prompt-engineering asset in the ecosystem, and the reference point for prompt-as-artifact workflows. |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 167,683 | The model-definition framework for state-of-the-art text, vision, audio and multimodal models, for both inference and training. Remains the canonical implementation layer that nearly every other project in this report builds on. |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | Python | 104,852 | Tensors and dynamic neural networks with strong GPU acceleration. The training substrate of the ecosystem; its position is stable but it continues to anchor every fine-tuning and post-training workflow. |
| [keras-team/keras](https://github.com/keras-team/keras) | Python | 64,371 | Deep learning for humans — a multi-backend high-level API. Relevant today as the accessible entry point for teams doing lightweight fine-tuning without deep framework plumbing. |
| [open-compass/opencompass](https://github.com/open-compass/opencompass) | Python | 7,511 | LLM evaluation platform covering OpenAI, Anthropic, Gemini, Qwen, GLM, DeepSeek and more across 100+ datasets spanning knowledge, reasoning, coding, long-context and safety. Evaluation is increasingly the bottleneck for shipping, so this matters more each quarter. |
| [thinkwee/AgentsMeetRL](https://github.com/thinkwee/AgentsMeetRL) | HTML | 1,861 | Curated list for Agentic RL — reinforcement learning applied to agent policies and tool use. A clear marker of where post-training is heading: from static SFT to RL on multi-step agent trajectories. |
| [RyanLiu112/Awesome-Process-Reward-Models](https://github.com/RyanLiu112/Awesome-Process-Reward-Models) | — | 183 | Comprehensive collection of process reward models (PRMs). PRMs are the enabling primitive for reasoning-heavy post-training, and this list tracks a niche that is rapidly becoming central to frontier training pipelines. |

### 🔍 RAG / Knowledge

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Python | 147,790 | Self-described "agent engineering platform" that grew out of the original LLM app framework. It remains the widest-adopted composition layer for retrieval and tool-using agents. |
| [Shubhamsaboo/awesome-llm-apps](https://github.com/Shubhamsaboo/awesome-llm-apps) | Python | 141,604 | 100+ free, open-source AI agents, agent skills and RAG apps. Functions as the de facto reference catalogue for practitioners choosing an architecture. |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 126,211 | Turns any codebase — docs, SQL schemas, configs, PDFs — into a queryable knowledge graph via local deterministic AST parsing, with every edge explained and no vector store. A direct challenge to embedding-first retrieval for code. |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | TypeScript | 100,449 | Captures agent session activity, compresses it with AI and injects relevant context into future sessions across Claude Code, OpenClaw, Codex, Gemini, Hermes, Copilot and OpenCode. Cross-agent persistent memory at 100k stars shows memory is now table stakes. |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 92,159 | Open-source RAG engine fusing retrieval with agent capabilities to form a "context layer" for LLMs. One of the few RAG engines written in Go, reflecting the performance-first turn in retrieval infrastructure. |
| [unclecode/crawl4ai](https://github.com/unclecode/crawl4ai) | Python | 85,465 | Web crawler/scraper that converts any site into clean, LLM-ready Markdown, self-hosted or via cloud. Ingestion quality remains the top practical failure mode in RAG, which keeps this in the top tier. |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 67,135 | Drop-in memory infrastructure giving agents persistent, production-grade context. Part of the same memory/context gold rush as claude-mem and cognee, but positioned as a platform layer rather than an agent plugin. |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 46,376 | High-performance, cloud-native vector database for scalable ANN search. Anchors the "big vector DB" tier alongside qdrant (35,068), weaviate (16,880), lancedb (11,666) and newcomers like alibaba/zvec (16,107) and orama (10,574). |

---

## 3. Trend Signal Analysis

Today's data shows attention rotating from raw model capability to the **harness around the model**. The largest signal is `morluto/rea` (+54,679 stars in one day), an agentic reverse-engineering tool, immediately followed by a dense cluster attacking the same bottleneck: `context-mode`, `headroom`, `claude-mem` and `mem0` all shrink, compress or persist agent context. Token economy is no longer a feature — it is a product category.

Second, the **"agent skill" abstraction has gone mainstream**. `vercel-labs/skills` ships npx-installable skills; `diagram-design` targets six different coding agents at once; `ECC` and `ponytail` sell harness performance and behaviour tuning. This is a direct downstream effect of the Claude Code / Codex CLI ecosystems and the MCP + hooks pattern that `context-mode` also builds on.

Third, **retrieval is diversifying away from naive vector search**: `graphify` uses deterministic AST knowledge graphs with no vector store, `PageIndex` (39,113) markets vectorless reasoning-based RAG, `LEANN` (13,024) claims 97% storage savings, and embedded/in-process databases (`zvec`, `orama`, `lancedb`) keep gaining. Rust recurs across agent engines, vector stores and inference (`rig`, `qdrant`, `lancedb`, `Codewhale`).

Finally, **local-first remains the strongest consumer undercurrent**: VoiceStudio (+1,748) for local voice cloning, anything-llm, open-webui, and AirLLM's single-4GB-GPU 2.8T inference. Ollama's model roster (Kimi, GLM, MiniMax, DeepSeek, gpt-oss, Qwen) shows how quickly new open-weight releases — and new post-training methods like PRMs and on-policy distillation — propagate into local runtimes.

---

## 4. Community Hot Spots

- **[mksglu/context-mode](https://github.com/mksglu/context-mode) + [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) + [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)** — the context/memory optimization layer. Three independent projects hit 75k–100k stars addressing token bloat and session persistence; whoever standardizes this layer captures a tax on every agent call.
- **[morluto/rea](https://github.com/morluto/rea)** — +54,679 stars in one day. Agentic reverse engineering signals a new high-value domain (security, binary and behavioural analysis) where agents have clear economic leverage.
- **[vercel-labs/skills](https://github.com/vercel-labs/skills) and agent-skill packs** — a vendor-backed package format for agent capabilities. Worth tracking because skill distribution, not model choice, is becoming the differentiator between agent setups.
- **[Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) + [VectifyAI/PageIndex](https://github.com/VectifyAI/PageIndex) + [StarTrail-org/LEANN](https://github.com/StarTrail-org/LEANN)** — vectorless and graph-based retrieval. If these hold up, the default RAG stack (embeddings + vector DB) may not be the default for long.
- **[lyogavin/airllm](https://github.com/lyogavin/airllm) + [debpalash/VoiceStudio](https://github.com/debpalash/VoiceStudio)** — the local-first frontier: 2.8T-parameter inference on a 4GB GPU and a 646-language local voice stack. This is where hardware constraints and privacy demand are creating the sharpest product pull.

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*