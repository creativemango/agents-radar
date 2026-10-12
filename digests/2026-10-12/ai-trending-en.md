# AI Open Source Trends 2026-10-12

> Sources: GitHub Trending + GitHub Search API | Generated: 2026-10-12 08:27 UTC

---

Filtering note: From today’s GitHub Trending I kept `morluto/rea`, `cathrynlavery/diagram-design`, `pytorch/pytorch`, and `lyogavin/airllm` as clearly AI/ML-relevant; I excluded `AnyPS5`, `ArtCraft`, `gods-eye-view`, and `Apollo-11` as not clearly AI-focused. Topic-search repos were already AI-tagged; I selected top representative projects per category. Where a repo appears in both feeds, total stars are from Topic Search and today’s delta is from Trending; otherwise today’s delta is shown only when provided.

## 1. Today’s Highlights

- AI agents dominate today’s signal: [morluto/rea](https://github.com/morluto/rea) added `+25,599` stars, the strongest momentum event in the dataset, with agentic reverse engineering spanning app behavior to native binaries.
- Agent skills and harness tooling are becoming a product category: [diagram-design](https://github.com/cathrynlavery/diagram-design) (`+1,083`) packages diagram generation for Claude Code, Codex, Copilot, Cursor, and other coding agents.
- Core and efficient inference remain hot: [pytorch/pytorch](https://github.com/pytorch/pytorch) (`+315`) and [AirLLM](https://github.com/lyogavin/airllm) (`+548`) show sustained demand for foundation frameworks and single-GPU large-model inference.
- RAG is moving toward context engineering: graph/vectorless retrieval, persistent memory, and token compression are prominent across high-star projects.
- Local-first and vertical AI apps still attract large audiences, from [Open WebUI](https://github.com/open-webui/open-webui) to [MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) and [Cherry Studio](https://github.com/CherryHQ/cherry-studio).

## 2. Top Projects by Category

### 🔧 AI Infrastructure

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | Python | 104,671 (+315) | Tensors and dynamic neural networks in Python with strong GPU acceleration. It remains a foundational ML framework and re-entered today’s trending, signaling sustained core-infra interest. |
| [tensorflow/tensorflow](https://github.com/tensorflow/tensorflow) | C++ | 200,876 | An open-source machine learning framework for everyone. Its massive installed base keeps it a reference point for production ML infrastructure. |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 182,821 | Get up and running with Kimi, GLM, MiniMax, DeepSeek, gpt-oss, Qwen, Gemma and other models. It captures the local/multi-model runner trend and is a key distribution layer for open LLMs. |
| [lyogavin/airllm](https://github.com/lyogavin/airllm) | Jupyter Notebook | 0 (+548) | AirLLM enables 70B inference with a single 4GB GPU. It is trending today because memory efficiency remains a bottleneck for local LLM use. |
| [Picovoice/picollm](https://github.com/Picovoice/picollm) | Python | 318 | On-device LLM inference powered by X-bit quantization. It points to the growing edge-inference niche where privacy and latency matter more than raw scale. |
| [0xPlaygrounds/rig](https://github.com/0xPlaygrounds/rig) | Rust | 8,861 | Build modular and scalable LLM applications in Rust. Rust is appearing more often in AI infra as teams seek performance and safety. |
| [langchain4j/langchain4j](https://github.com/langchain4j/langchain4j) | Java | 13,225 | Idiomatic Java library for LLM-powered apps, tool calling, agents, and RAG on the JVM. It shows enterprise Java adopting LLM app patterns through unified APIs. |
| [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) | HTML | 0 (+1,083) | Editorial diagram design skill for Claude Code, Codex, GitHub Copilot, Cursor, Factory Droid, and Pi, with 44 diagram types. Its `+1,083`-star day highlights agent skill packs as a new dev-tool format. |

### 🤖 AI Agents / Workflows

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [morluto/rea](https://github.com/morluto/rea) | TypeScript | 123,626 (+25,599) | Reverse engineer anything with agents, from app behavior down to native binaries. The `+25,599`-star day is the clearest breakout signal in today’s data and places agentic reverse engineering at the center of attention. |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 277,451 | Agent harness performance optimization system with skills, instincts, memory, security, and research-first development for Claude Code, Codex, Opencode, Cursor and beyond. It reflects demand for meta-tooling around coding agents. |
| [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) | Python | 252,923 | The agent that grows with you. Its very high star count shows persistent community interest in personal, extensible agent frameworks. |
| [Significant-Gravitas/AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) | Python | 187,527 | AutoGPT is the vision of accessible AI for everyone, to use and build on. It remains a canonical reference for autonomous agent ambitions. |
| [langgenius/dify](https://github.com/langgenius/dify) | TypeScript | 158,212 | Build agentic workflows, RAG pipelines, with rich AI model and tool support in one workspace. It is a production-oriented agent/RAG platform with strong self-hosting appeal. |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Python | 147,660 | The agent engineering platform. It continues to anchor the LLM app and orchestration ecosystem. |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | Python | 117,687 | Agents that use the browser. Browser control remains one of the most practical agent capabilities for real-world automation. |
| [Panniantong/Agent-Reach](https://github.com/Panniantong/Agent-Reach) | Python | 96,281 | Give your AI agent eyes to see the entire internet, reading and searching Twitter, Reddit, YouTube, GitHub, Bilibili, and XiaoHongShu via one CLI with zero API fees. It addresses web access, a key agent bottleneck. |

### 📦 AI Applications

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | Python | 154,367 | User-friendly AI interface supporting Ollama, OpenAI API, and more. It remains one of the most popular self-hosted front ends for LLM workflows. |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 129,999 | Generate HD short videos from a topic or keyword with an automated AI workflow. It shows continued appetite for generative media automation. |
| [openbq-org/OpenBB](https://github.com/openbq-org/OpenBB) | Python | 74,135 | Open Data Platform for analysts, quants and AI agents. It positions AI agents as consumers of financial and analytical data workflows. |
| [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) | Python | 66,211 | LLM-powered multi-market stock analysis system with market data, real-time news, decision dashboards, and automated notifications. It is a vertical AI agent application for finance. |
| [hugohe3/ppt-master](https://github.com/hugohe3/ppt-master) | Python | 60,672 | AI turns documents or topics into real, native PowerPoint decks with charts, narration, and templates. It targets a high-value office automation use case. |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | TypeScript | 52,559 | AI productivity studio with smart chat, autonomous agents, and 300+ assistants. It aggregates frontier LLMs into a desktop productivity environment. |
| [jeecgboot/JeecgBoot](https://github.com/jeecgboot/JeecgBoot) | Java | 48,182 | Enterprise AI low-code platform that can generate systems from natural language and includes AI chat, knowledge base, workflow orchestration, and MCP plugins. It reflects enterprise AI adoption through low-code. |
| [ScrapeGraphAI/Scrapegraph-ai](https://github.com/ScrapeGraphAI/Scrapegraph-ai) | Python | 31,712 | Python scraper based on AI. It provides structured web data for agents and RAG pipelines, a common bottleneck for AI apps. |

### 🧠 LLMs / Training

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 167,545 | Model-definition framework for state-of-the-art text, vision, audio, and multimodal models for inference and training. It remains the central hub for open model implementations. |
| [open-compass/opencompass](https://github.com/open-compass/opencompass) | Python | 7,505 | LLM evaluation platform supporting OpenAI, Anthropic, Gemini, Qwen, GLM, DeepSeek, and 100+ datasets. Evaluation is increasingly critical as model choices multiply. |
| [zchoi/Awesome-Embodied-Robotics-and-Agent](https://github.com/zchoi/Awesome-Embodied-Robotics-and-Agent) |  | 1,899 | Curated list of embodied AI/robotics research with large language models. It tracks the fusion of LLMs, agents, and physical systems. |
| [thinkwee/AgentsMeetRL](https://github.com/thinkwee/AgentsMeetRL) | HTML | 1,863 | Awesome list for Agentic RL. It signals growing research interest in reinforcement learning for agent behavior. |
| [thinkwee/AwesomeOPD](https://github.com/thinkwee/AwesomeOPD) |  | 874 | Awesome list for On-Policy Distillation. It reflects continued work on efficient model training and distillation. |
| [chrisliu298/awesome-llm-unlearning](https://github.com/chrisliu298/awesome-llm-unlearning) |  | 630 | Resource repository for machine unlearning in large language models. Safety, compliance, and data-removal concerns are becoming first-class LLM topics. |
| [RyanLiu112/Awesome-Process-Reward-Models](https://github.com/RyanLiu112/Awesome-Process-Reward-Models) |  | 183 | Comprehensive collection of process reward models. PRMs are important for reasoning, agent training, and RL-based post-training. |

### 🔍 RAG / Knowledge

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 125,659 | Turn any codebase, docs, SQL schemas, configs, and PDFs into a queryable knowledge graph as a skill for Claude Code, Cursor, Codex, and Gemini CLI. Its no-vector-store approach is a notable counterpoint to mainstream RAG. |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | TypeScript | 99,864 | Persistent context across sessions for every agent, capturing work, compressing it with AI, and injecting relevant context into future sessions. It addresses the memory gap in coding agents. |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 92,036 | Leading open-source RAG engine that fuses RAG with Agent capabilities for a superior context layer. It is a production-grade retrieval platform with strong adoption. |
| [unclecode/crawl4ai](https://github.com/unclecode/crawl4ai) | Python | 85,333 | Open-source web crawler and scraper for LLMs and AI agents, turning any website into clean Markdown. It feeds the data-hungry RAG and agent ecosystem. |
| [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) | Python | 75,097 | Compress tool outputs, logs, files, and RAG chunks before they reach the LLM, reducing tokens for coding agents and JSON. It highlights context compression as a new RAG/agent optimization layer. |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 67,023 | The Memory Layer for AI Agents, providing drop-in memory infrastructure with persistent context. It is a leading project in the agent-memory category. |
| [run-llama/llama_index](https://github.com/run-llama/llama_index) | Python | 52,468 | Document processing platform for AI. It remains a standard framework for connecting private data to LLMs. |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 46,361 | High-performance, cloud-native vector database built for scalable vector ANN search. It represents the mature vector-database layer behind many RAG systems. |

## 3. Trend Signal Analysis

Today’s signal is overwhelmingly agent-first. The single largest momentum event is [morluto/rea](https://github.com/morluto/rea) (`+25,599` stars), an agentic reverse-engineering tool, which suggests the community is moving from generic chatbots to agents that inspect, operate, and manipulate real software. Around it, a second layer is hardening: agent harnesses and skills. [affaan-m/ECC](https://github.com/affaan-m/ECC), [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent), and similar projects compete on memory, tools, security, and performance, while [diagram-design](https://github.com/cathrynlavery/diagram-design) packages a narrow skill for multiple coding agents. That is a shift from one monolithic agent framework to composable agent capabilities.

A second signal is context engineering for RAG and agents. [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify), [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem), [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom), and [mem0ai/mem0](https://github.com/mem0ai/mem0) all attack the gap between raw retrieval and usable long-term context: knowledge graphs without vectors, cross-session memory, and token compression. Vector databases such as [milvus-io/milvus](https://github.com/milvus-io/milvus) remain foundational, but differentiation is moving upward into retrieval quality, memory, and cost control.

New stacks are visible too. Rust and Go appear in agent engines, vector databases, and RAG infrastructure; TypeScript dominates agent frontends, skills, and desktop apps; Python still leads model and training tooling. On the model side, [ollama/ollama](https://github.com/ollama/ollama) reflects a multi-vendor, local-first LLM reality with Kimi, GLM, MiniMax, DeepSeek, gpt-oss, Qwen, and Gemma support. Efficient inference projects like [lyogavin/airllm](https://github.com/lyogavin/airllm) and [Picovoice/picollm](https://github.com/Picovoice/picollm) show that large models on constrained hardware remain a priority. Finally, LLM evaluation, process reward models, agentic RL, and unlearning are appearing as focused research tracks, indicating the ecosystem is professionalizing beyond demos.

## 4. Community Hot Spots

- [morluto/rea](https://github.com/morluto/rea) — `+25,599` stars today; agentic reverse engineering is the day’s breakout. Watch how agent tooling expands into security, binary analysis, and software archaeology.
- [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) — vectorless knowledge graph for code and docs; strong signal that RAG is moving beyond embedding-only pipelines.
- [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) — persistent cross-session memory for coding agents; directly addresses one of the biggest friction points in Claude Code/Codex-style workflows.
- [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) — token compression for tool outputs, logs, and RAG chunks; cost and latency reduction is becoming a competitive layer for agents.
- [lyogavin/airllm](https://github.com/lyogavin/airllm) — 70B inference on a single 4GB GPU; efficient local inference remains a high-leverage area as open models proliferate.

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*