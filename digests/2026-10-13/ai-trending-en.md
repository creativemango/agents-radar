# AI Open Source Trends 2026-10-13

> Sources: GitHub Trending + GitHub Search API | Generated: 2026-10-13 03:08 UTC

---

# AI Open Source Trends Report — 2026-10-13

> **Filtering note:** This report includes AI/ML-relevant repositories from the trending list and topic search. Non-AI/general trending items were excluded. Star totals are copied from the source; several trending-only repos show `0` total in the provided snapshot, so today’s delta is shown as given.

## 1. Today's Highlights

Today’s AI open-source momentum is dominated by agent tooling: [morluto/rea](https://github.com/morluto/rea) exploded with **+45,327 stars today** for agentic reverse engineering, while [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) (**+2,081**) and [Alishahryar1/free-claude-code](https://github.com/Alishahryar1/free-claude-code) (**+626**) show strong demand for reusable skills and multi-harness coding workflows. Context and memory optimization remain major themes, with [mksglu/context-mode](https://github.com/mksglu/context-mode) (**+246**), [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom), [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem), and [mem0ai/mem0](https://github.com/mem0ai/mem0) all attacking token bloat and persistent agent state. Local/edge inference also registers strongly: [lyogavin/airllm](https://github.com/lyogavin/airllm) (**+879**) claims 2.8T-scale inference on a single 4GB GPU, and [microsoft/BitNet](https://github.com/microsoft/BitNet) (**+97**) pushes 1-bit LLM inference. Meanwhile, the broader topic graph shows durable leaders in RAG/knowledge infrastructure ([firecrawl/firecrawl](https://github.com/firecrawl/firecrawl), [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify), [infiniflow/ragflow](https://github.com/infiniflow/ragflow)) and agent frameworks ([NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent), [langchain-ai/langchain](https://github.com/langchain-ai/langchain), [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph)).

## 2. Top Projects by Category

### 🔧 AI Infrastructure

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [mksglu/context-mode](https://github.com/mksglu/context-mode) | TypeScript | 0 (+246) | Context-window optimization for AI coding agents, sandboxing tool output for 98% reduction, persisting session memory, and routing across 17 platforms via MCP + hooks. It matters because agent teams are prioritizing token efficiency and reliable harness integration. |
| [Alishahryar1/free-claude-code](https://github.com/Alishahryar1/free-claude-code) | Python | 0 (+626) | Multi-harness, multi-model access to Claude Code, Codex, VSCode, Pi, OpenCode, and more with 1.3B+ free tokens. The daily jump signals strong demand for lower-cost, harness-agnostic coding agents. |
| [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) | HTML | 0 (+2,081) | Editorial diagram design skills for Claude Code, Codex, GitHub Copilot, Cursor, Factory Droid, and Pi, with 44 diagram types in self-contained HTML + SVG. The +2,081 daily spike shows agent skills for high-quality visual output becoming a distinct niche. |
| [lyogavin/airllm](https://github.com/lyogavin/airllm) | Jupyter Notebook | 0 (+879) | AirLLM enables 2.8T LLM inference with a single 4GB GPU. It is a notable local/edge inference signal as developers seek larger models on constrained hardware. |
| [microsoft/BitNet](https://github.com/microsoft/BitNet) | C++ | 0 (+97) | Microsoft’s official inference framework for 1-bit LLMs. It keeps low-bit model research in focus as a path to cheaper, more efficient inference. |
| [ollama/ollama](https://github.com/ollama/ollama) | Go | 182,930 | Local model runner for Kimi, GLM, MiniMax, DeepSeek, gpt-oss, Qwen, Gemma, and more. It remains core infrastructure for private, local-first AI development. |
| [affaan-m/ECC](https://github.com/affaan-m/ECC) | JavaScript | 277,976 | Agent harness performance optimization covering skills, instincts, memory, security, and research-first development for Claude Code, Codex, Opencode, Cursor, and beyond. Its massive total star count shows how central harness optimization has become. |
| [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) | Python | 75,150 | Compresses tool outputs, logs, files, and RAG chunks before they reach the LLM, claiming 20% fewer tokens for coding agents and 60–95% fewer for JSON. It reflects the broader push to cut context cost without degrading answers. |

### 🤖 AI Agents / Workflows

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [morluto/rea](https://github.com/morluto/rea) | TypeScript | 164,280 (+45,327) | Reverse engineer anything with agents, from app behavior down to native binaries. The +45,327 daily spike makes it the standout agentic reverse-engineering launch today. |
| [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) | Python | 253,180 | The agent that grows with you. Its high total star count shows continued dominance of general-purpose personal/agent frameworks. |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Python | 147,715 | The agent engineering platform. It remains a foundational framework for building tool-using, multi-step LLM systems. |
| [langchain-ai/langgraph](https://github.com/langchain-ai/langgraph) | Python | 43,157 | Builds resilient agents with graph-style control flow. It is increasingly important for production-grade agent orchestration. |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | Python | 117,766 | Agents that use the browser. Browser/computer-use agents remain one of the most practical paths to automating real web workflows. |
| [langgenius/dify](https://github.com/langgenius/dify) | TypeScript | 158,292 | Builds agentic workflows and RAG pipelines with rich model and tool support on a collaborative workspace. It bridges prototype-to-production agent development for teams. |
| [Panniantong/Agent-Reach](https://github.com/Panniantong/Agent-Reach) | Python | 96,634 | Gives AI agents eyes to read/search Twitter, Reddit, YouTube, GitHub, Bilibili, and XiaoHongShu through one CLI with zero API fees. It addresses the critical data-access layer for autonomous agents. |
| [esengine/DeepSeek-Reasonix](https://github.com/esengine/DeepSeek-Reasonix) | Go | 35,767 | A reliable coding agent for complex software engineering tasks. It signals continued specialization in agentic software engineering. |

### 📦 AI Applications

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | Python | 155,143 (+157) | User-friendly AI interface supporting Ollama, OpenAI API, and more. It remains a leading self-hosted front end for local and API models. |
| [Mintplex-Labs/anything-llm](https://github.com/Mintplex-Labs/anything-llm) | JavaScript | 66,995 | Local-first agent experience designed to let users own their intelligence. It appeals to users who want private document/agent workflows without renting a closed platform. |
| [CherryHQ/cherry-studio](https://github.com/CherryHQ/cherry-studio) | TypeScript | 52,583 | AI productivity studio with smart chat, autonomous agents, and 300+ assistants. It unifies frontier LLM access in a desktop-style app. |
| [hugohe3/ppt-master](https://github.com/hugohe3/ppt-master) | Python | 61,186 | AI turns documents or topics into native PowerPoint decks with shapes, transitions, charts, narration, and template support. It is a strong example of vertical AI output tools moving beyond text. |
| [career-ops-hq/career-ops](https://github.com/career-ops-hq/career-ops) | JavaScript | 74,205 | Open-source AI job search agent that scans boards, scores jobs against a CV, tailors resumes/cover letters, and tracks applications. It shows agents entering high-friction personal workflows. |
| [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) | Python | 66,249 | LLM-powered multi-market stock analysis with market data, real-time news, dashboards, and automated notifications. It reflects growing demand for AI finance copilots. |
| [harry0703/MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) | Python | 130,307 | Generates HD short videos from a topic or keyword using AI models and automated workflows. Its high star count shows continued appetite for generative media applications. |

### 🧠 LLMs / Training

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [huggingface/transformers](https://github.com/huggingface/transformers) | Python | 167,611 | Hugging Face Transformers is the model-definition framework for state-of-the-art text, vision, audio, and multimodal models. It remains the default library for model loading, fine-tuning, and inference. |
| [pytorch/pytorch](https://github.com/pytorch/pytorch) | Python | 104,745 | Tensors and dynamic neural networks with strong GPU acceleration. It remains a core training and research framework. |
| [tensorflow/tensorflow](https://github.com/tensorflow/tensorflow) | C++ | 200,904 | Open-source machine learning framework for everyone. It still anchors many production ML stacks. |
| [keras-team/keras](https://github.com/keras-team/keras) | Python | 64,366 | Deep Learning for humans. It provides high-level model building on top of modern backends. |
| [scikit-learn/scikit-learn](https://github.com/scikit-learn/scikit-learn) | Python | 67,569 | Classic machine learning in Python. It remains essential for tabular ML and baseline modeling. |
| [ultralytics/ultralytics](https://github.com/ultralytics/ultralytics) | Python | 62,436 | YOLO models for detection, segmentation, classification, pose, and tracking. It keeps computer vision training and inference highly accessible. |
| [open-compass/opencompass](https://github.com/open-compass/opencompass) | Python | 7,507 | LLM evaluation platform covering OpenAI, Anthropic, Gemini, Qwen, GLM, DeepSeek, and more across 100+ datasets. It matters as model choice and safety evaluation scale. |

### 🔍 RAG / Knowledge

| Project | Lang | Stars (total / today) | Summary |
| :--- | :--- | ---: | :--- |
| [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) | TypeScript | 190,896 | Web data infrastructure for AI agents and RAG pipelines, turning websites into clean LLM-ready data. Its high total shows that data acquisition remains a bottleneck and opportunity. |
| [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify) | Python | 125,915 | Turns codebases, docs, SQL schemas, configs, and PDFs into queryable knowledge graphs via deterministic AST parsing and no vector store. It represents the growth of graph/structured RAG for code and enterprise knowledge. |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | TypeScript | 100,131 | Captures agent sessions, compresses them with AI, and injects relevant context into future sessions across Claude Code, OpenClaw, Codex, Gemini, Hermes, Copilot, OpenCode, and more. Persistent memory is becoming a must-have agent layer. |
| [infiniflow/ragflow](https://github.com/infiniflow/ragflow) | Go | 92,085 | Open-source RAG engine fusing RAG with agent capabilities for a superior context layer. It remains a leading full-stack retrieval/agent platform. |
| [unclecode/crawl4ai](https://github.com/unclecode/crawl4ai) | Python | 85,400 | Web crawler/scraper that turns any website into clean LLM-ready Markdown. It is core plumbing for RAG and agent browsing workflows. |
| [mem0ai/mem0](https://github.com/mem0ai/mem0) | Python | 67,066 | Memory layer for AI agents with drop-in production memory infrastructure. It reflects the shift from stateless prompts to persistent agent context. |
| [milvus-io/milvus](https://github.com/milvus-io/milvus) | Go | 46,366 | High-performance, cloud-native vector database for scalable ANN search. It remains a backbone for production vector retrieval. |
| [VectifyAI/PageIndex](https://github.com/VectifyAI/PageIndex) | Python | 39,084 | Vectorless, reasoning-based document index for RAG. It is a notable challenge to default vector-store architectures and points toward reasoning-heavy retrieval. |

## 3. Trend Signal Analysis

The dominant signal today is that the community is paying for agent reliability, not just agent demos. The largest daily spike came from an agentic reverse-engineering project, while several high-velocity AI coding tools focus on harness interoperability, reusable skills, context-window compression, and persistent session memory. This suggests the pain points have shifted from “can an agent do a task?” to “can it do it repeatedly, affordably, and across Claude Code, Codex, Copilot, Cursor, Gemini CLI, and other harnesses?”

Second, context and memory are becoming a first-class layer. Repositories that compress tool outputs, crawl web data, maintain long-term memory, or build codebase knowledge graphs are accumulating large star totals. RAG is also diversifying: alongside established vector databases and RAG engines, today’s data highlights vectorless/reasoning-based document indexing, deterministic AST-based graphs, embedded retrieval libraries, and storage-efficient RAG for personal devices. This looks like a move beyond naive chunk-and-embed pipelines toward structured, reasoning-oriented retrieval.

Third, local and efficient inference remains active. Projects claiming large-model inference on a single 4GB GPU, 1-bit LLM inference, and low-storage RAG show that cost and privacy are still major adoption drivers. The stack mix is also broadening: Rust, Go, Java, TypeScript, and C++ appear in agent engines, vector databases, inference frameworks, and enterprise LLM libraries, not just Python.

Finally, vertical applications are maturing quickly: job-search agents, stock-analysis copilots, PowerPoint generators, video generators, and personal assistants all appear with six-figure star totals. The overall trend is an agent ecosystem consolidating around memory, context, multi-harness support, and specialized vertical workflows, with RAG evolving into a broader knowledge infrastructure layer.

## 4. Community Hot Spots

- **Agent memory and context compression** — [mksglu/context-mode](https://github.com/mksglu/context-mode), [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem), [mem0ai/mem0](https://github.com/mem0ai/mem0), [topoteretes/cognee](https://github.com/topoteretes/cognee), and [headroomlabs-ai/headroom](https://github.com/headroomlabs-ai/headroom) are attacking token bloat, session persistence, and cross-session recall. This is likely the highest-leverage layer for production agents.
- **Agent harnesses, skills, and free multi-model access** — [Alishahryar1/free-claude-code](https://github.com/Alishahryar1/free-claude-code), [affaan-m/ECC](https://github.com/affaan-m/ECC), [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design), and [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) show demand for reusable skills, performance tuning, and harness-agnostic workflows.
- **Agentic coding and reverse engineering** — [morluto/rea](https://github.com/morluto/rea), [esengine/DeepSeek-Reasonix](https://github.com/esengine/DeepSeek-Reasonix), and [codewhale-hq/Codewhale](https://github.com/codewhale-hq/Codewhale) point to specialized coding and reverse-engineering agents that operate over real software artifacts.
- **RAG beyond vector stores** — [Graphify-Labs/graphify](https://github.com/Graphify-Labs/graphify), [VectifyAI/PageIndex](https://github.com/VectifyAI/PageIndex), [infiniflow/ragflow](https://github.com/infiniflow/ragflow), [qdrant/qdrant](https://github.com/qdrant/qdrant), [alibaba/zvec](https://github.com/alibaba/zvec), and [StarTrail-org/LEANN](https://github.com/StarTrail-org/LEANN) represent graph RAG, reasoning-based indexing, production RAG, and embedded/storage-efficient vector retrieval.
- **Local and efficient inference** — [lyogavin/airllm](https://github.com/lyogavin/airllm), [microsoft/BitNet](https://github.com/microsoft/BitNet), [ollama/ollama](https://github.com/ollama/ollama), and [StarTrail-org/LEANN](https://github.com/StarTrail-org/LEANN) keep pressure on running larger models with less GPU and storage.

---
*This digest is auto-generated by [agents-radar](https://github.com/creativemango/agents-radar).*