# Module 12: Ecosystem Comparison — Hyperscalers, Foundation Model Providers & Open Source

**Time estimate:** 1 week
**Goal:** Compare and contrast the full agentic AI ecosystems of Google, Microsoft/Azure, AWS, Anthropic, OpenAI, and the open-source/independent ecosystem — models, agent platforms, infrastructure, standards, and pricing.

---

## Learning Objectives

By the end of this module you will:
- Map each provider's full ecosystem: models, agent platforms, infrastructure, standards, and pricing
- Identify each provider's strategic philosophy and where it creates lock-in vs openness
- Compare API design, function calling, and reasoning capabilities across providers
- Evaluate agent platform maturity and differentiation across all six ecosystems
- Understand the standards landscape (MCP, A2A, Agent Skills, OpenTelemetry) and each provider's stance
- Build a decision framework for selecting providers based on workload requirements
- Analyze total cost of ownership across API providers and self-hosting

---

## Unit 12.1: Strategic Overview & Provider Philosophies

### The Six Ecosystems

Each provider has a fundamentally different strategic position in the agentic AI landscape:

| Provider | Core Position | Philosophy |
|----------|--------------|------------|
| **Anthropic** | Foundation model lab | Safety-first, developer-focused, protocol creator (MCP), vertical integration around Claude |
| **OpenAI** | Foundation model lab | Product-first, consumer + developer, broadest model range, moving to platform (Responses API) |
| **Google** | Vertically integrated cloud + model | Full stack from silicon (TPUs) to models (Gemini) to platform (Vertex AI) to apps (Workspace) |
| **Microsoft/Azure** | Enterprise cloud + distribution | Model marketplace (1,800+ models), enterprise integration (M365, Copilot), open standards champion |
| **AWS** | Infrastructure-first cloud | Model-agnostic marketplace (~100 models), consumption-based runtime, framework-agnostic |
| **Open Source** | Community-driven ecosystem | Freedom, transparency, data sovereignty, cost control at scale, rapid innovation |

### Readings

1. **Blog:** [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) — Anthropic
   - Anthropic's philosophy: start simple, add complexity only when needed
2. **Blog:** [OpenAI for Developers 2025](https://developers.openai.com/blog/openai-for-developers-2025/) — OpenAI
   - The shift from Chat Completions + Assistants to the unified Responses API
3. **Architecture:** [The Agentic Enterprise: IT Architecture for the AI-Powered Future](https://architect.salesforce.com/fundamentals/agentic-enterprise-it-architecture) — Salesforce Architects
   - Vendor-neutral view of how enterprise agent architecture layers together
4. **Blog:** [Agent Development Kit: Easy to Build Multi-Agent Applications](https://developers.googleblog.com/en/agent-development-kit-easy-to-build-multi-agent-applications/) — Google
   - Google's open-source, model-agnostic approach to agent development
5. **Blog:** [Introducing Amazon Bedrock AgentCore](https://aws.amazon.com/blogs/aws/introducing-amazon-bedrock-agentcore-securely-deploy-and-operate-ai-agents-at-any-scale/) — AWS
   - AWS's framework-agnostic, consumption-based agent runtime
6. **Blog:** [Empowering Innovation: The Next Generation of Phi](https://azure.microsoft.com/en-us/blog/empowering-innovation-the-next-generation-of-the-phi-family/) — Microsoft
   - Microsoft's multi-model strategy: OpenAI frontier + Phi small models + open catalog

### Key Strategic Tensions

| Tension | Players on Each Side |
|---------|---------------------|
| **Vertical integration vs marketplace** | Google (vertical) vs AWS (marketplace) vs Azure (hybrid) |
| **Own models vs host all models** | Anthropic/OpenAI (own) vs AWS (host all) vs Google/Azure (both) |
| **Protocol creator vs adopter** | Anthropic (MCP), Google (A2A) vs everyone else adopting |
| **Consumer-first vs enterprise-first** | OpenAI (consumer) vs Azure/AWS (enterprise) vs Anthropic (shifting to both) |
| **Open-source vs proprietary** | Meta/Alibaba/DeepSeek (open) vs Anthropic/OpenAI (closed weights) vs Google/Microsoft (both) |

### Exercises

- [ ] For each of the six ecosystems, write a one-paragraph summary of their strategic position and primary differentiator
- [ ] Identify which providers create the most lock-in and where. Which components are portable vs proprietary?

---

## Unit 12.2: Models & APIs

### Model Families Compared

| Provider | Flagship | Mid-Tier | Budget | Reasoning | Context Window |
|----------|----------|----------|--------|-----------|---------------|
| **Anthropic** | Opus 4.6 | Sonnet 4.6 | Haiku 4.5 | Extended thinking (configurable budget) | 200K (1M beta) |
| **OpenAI** | GPT-5 / o3-pro | GPT-4.1 / o3 | GPT-4.1-nano / o4-mini | o-series (dedicated reasoning models) | 1M (GPT-4.1), 200K (o-series) |
| **Google** | Gemini 3.1 Pro | Gemini 2.5 Pro | Gemini 2.5 Flash-Lite | Configurable thinking budgets, Deep Think mode | 1M (2M coming) |
| **AWS** | Nova 2 Pro | Nova 2 Lite | Nova Micro | Configurable thinking budgets | 1M |
| **Microsoft** | GPT-4o/o3 (via Azure OpenAI) | Phi-4 (14B) | Phi-4-mini (3.8B) | Phi-4-reasoning (SFT on o3-mini) | Same as OpenAI + 128K Phi |
| **Open Source** | Llama 4 Maverick (400B MoE) | Qwen 3 235B / DeepSeek-R1 | Qwen 3 7B / Gemma 3 | DeepSeek-R1 (MIT license) | 10M (Llama 4 Scout) |

### API Design Philosophy

| Aspect | Anthropic | OpenAI | Google | AWS |
|--------|-----------|--------|--------|-----|
| **Primary API** | Messages API | Responses API (replacing Chat Completions + Assistants) | Gemini API / Vertex AI | Converse API (unified across all models) |
| **Tool calling** | `tool_use` content blocks with `tool_result` | Function calling with JSON Schema | Native function calling | Unified tool calling across ~100 models |
| **Reasoning** | Extended thinking (interleaved, summarized) | Separate o-series models with reasoning tokens | Configurable thinking budgets per request | Model-dependent |
| **Caching** | Prompt caching with breakpoints (90% savings) | Automatic prompt caching (50-90% savings) | Context caching API | No native caching (use application-level) |
| **Multimodal** | Vision + PDF + citations | Vision + audio + DALL-E + Sora | Vision + audio + video (native) | Model-dependent |
| **Streaming** | Server-sent events | Server-sent events | Server-sent events | Server-sent events |
| **Batch** | 50% discount, 24hr processing | 50% discount, 24hr processing | Batch prediction API | 50% discount on supported models |

### Unique API Capabilities

| Capability | Provider | Description |
|-----------|----------|-------------|
| **Citations** | Anthropic | API-level source attribution from document content blocks |
| **Extended thinking** | Anthropic | Visible chain-of-thought with configurable token budgets; adaptive and interleaved modes |
| **Responses API** | OpenAI | Server-managed conversation state with built-in tools (web search, code interpreter, file search) |
| **Agentic Vision** | Google | Iterative image manipulation with code execution (Gemini 3 Flash) |
| **Converse API** | AWS | Single API for tool calling across ~100 models from different providers |
| **Model Optimizer** | Google | Single meta-endpoint with dynamic routing and pricing across Gemini models |

### Readings

1. **Docs:** [Messages API — Anthropic](https://platform.claude.com/docs/en/api/messages)
2. **Docs:** [Responses vs Chat Completions — OpenAI](https://platform.openai.com/docs/guides/responses-vs-chat-completions)
3. **Docs:** [Gemini API Models — Google](https://ai.google.dev/gemini-api/docs/models)
4. **Docs:** [Converse API — AWS Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/conversation-inference-call.html)
5. **Docs:** [Azure OpenAI Service — Microsoft](https://learn.microsoft.com/en-us/azure/ai-services/openai/)

### Exercises

- [ ] Pick a task that requires tool calling (e.g., "look up the weather in 3 cities and compare"). Implement it using the Anthropic Messages API and the OpenAI Responses API. Compare: token usage, latency, code complexity, and output quality.
- [ ] Compare how each provider handles reasoning/thinking. For the same complex math problem, test: Claude with extended thinking, o3 with reasoning tokens, and Gemini 2.5 Pro with a thinking budget. Document the differences in output structure and cost.

---

## Unit 12.3: Agent Platforms & Frameworks

### Provider Agent Platforms

| Provider | Agent SDK/Framework | Managed Runtime | Low-Code/No-Code | Coding Agent |
|----------|-------------------|-----------------|-------------------|-------------|
| **Anthropic** | Claude Agent SDK (JS/Python) | — | — | Claude Code (~$2.5B ARR) |
| **OpenAI** | Agents SDK (Python/TS, open-source) | — | — | Codex (cloud agent) |
| **Google** | Agent Development Kit (ADK, open-source) | Vertex AI Agent Engine | Agentspace Agent Designer | Gemini CLI |
| **Microsoft** | Agent Framework (SK + AutoGen, open-source) | Azure AI Foundry Agent Service | Copilot Studio | GitHub Copilot |
| **AWS** | — (framework-agnostic) | Bedrock AgentCore | PartyRock (playground) | Amazon Q Developer |
| **Open Source** | LangGraph, CrewAI, smolagents, DSPy | LangGraph Cloud | — | Aider, Continue |

### Agent SDK Comparison

| Dimension | Claude Agent SDK | OpenAI Agents SDK | Google ADK | MS Agent Framework |
|-----------|-----------------|-------------------|------------|-------------------|
| **Language** | JavaScript, Python | Python, TypeScript | Python, Java (TS coming) | Python, .NET |
| **License** | Proprietary | MIT | Apache 2.0 | MIT |
| **Model lock-in** | Claude only | OpenAI default, extensible | Model-agnostic (200+ models) | Multi-model (via catalog) |
| **Built-in tools** | Read, Write, Edit, Bash, Glob, Grep | Web search, file search, code interpreter, computer use | MCP tools, code execution | Azure AI Search, Functions, connectors |
| **Multi-agent** | Subagent delegation | Handoff protocol | ADK orchestration | Group chat, debate, reflection |
| **Key differentiator** | Powers Claude Code; deep MCP integration | Lightest weight; Responses API integration | Broadest model support; A2A + MCP | Enterprise integration (M365, Teams) |

### Managed Agent Runtimes Compared

| Dimension | Vertex AI Agent Engine | Azure AI Foundry Agent Service | Bedrock AgentCore |
|-----------|----------------------|-------------------------------|-------------------|
| **Framework support** | ADK, LangGraph, CrewAI, custom | Agent Framework, LangChain, OpenAI SDK, custom | Any framework, any model |
| **Model support** | Gemini, Claude, Mistral, others | 1,800+ model catalog | ~100 models (Claude, Llama, Mistral, Nova, etc.) |
| **Memory** | Sessions + Memory Bank (long-term) | Long-term memory store | Episodic memory (learns from experience) |
| **Billing model** | Per-second vCPU + GiB-hour | Free runtime (pay for consumed tokens/tools) | Per-second, no charge during I/O wait |
| **Observability** | Cloud Trace (OpenTelemetry) | Application Insights (OpenTelemetry) | CloudWatch + X-Ray + AgentCore Evaluations |
| **Evaluation** | Vertex AI Evaluation | Azure AI Evaluation | 13 pre-built evaluators |
| **Guardrails** | Vertex AI Guardrails | Azure AI Content Safety | Bedrock Guardrails |
| **Key differentiator** | Deepest Gemini integration | M365 one-click publish | Framework-agnostic + I/O wait savings |

### Readings

1. **Blog:** [Building Agents with the Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk) — Anthropic
2. **Docs:** [OpenAI Agents SDK](https://openai.github.io/openai-agents-python/) — OpenAI
3. **Docs:** [Agent Development Kit](https://google.github.io/adk-docs/) — Google
4. **Docs:** [Azure AI Foundry Agent Service](https://learn.microsoft.com/en-us/azure/foundry/agents/overview) — Microsoft
5. **Docs:** [Amazon Bedrock AgentCore](https://aws.amazon.com/bedrock/agentcore/) — AWS
6. **Blog:** [LangChain and LangGraph 1.0 Milestones](https://blog.langchain.com/langchain-langgraph-1dot0/) — LangChain

### Open-Source Framework Landscape

| Framework | Architecture | Best For | Model Lock-in |
|-----------|-------------|----------|---------------|
| **LangGraph 1.0** | Graph-based execution, durable state | Complex stateful agents, human-in-the-loop | None (any model) |
| **CrewAI** | Role-based multi-agent crews | Team-based collaboration, beginner-friendly | None |
| **smolagents** | Code-first execution (Python, not JSON) | Rapid prototyping, ~30% fewer tokens | None |
| **LlamaIndex** | Data-centric agent workflows | RAG-heavy agents, document processing | None |
| **DSPy** | Declarative prompt optimization | Research, automated prompt engineering | None |
| **AutoGen/AG2** | Async conversation among agents | Research, complex multi-agent debate | None |

### Exercises

- [ ] Compare the "hello world" agent across three ecosystems: build a simple tool-using agent with Claude Agent SDK, OpenAI Agents SDK, and Google ADK. Compare lines of code, setup complexity, and developer experience.
- [ ] For each managed runtime (Vertex AI Agent Engine, Azure AI Foundry Agent Service, Bedrock AgentCore), list: supported frameworks, billing model, built-in capabilities, and lock-in risks. Which would you choose for: (a) a startup, (b) a Google Workspace enterprise, (c) a multi-cloud enterprise?

---

## Unit 12.4: Infrastructure & Cloud Services

### Vector Database Options by Provider

| Provider | Native Vector DB | Third-Party Options |
|----------|-----------------|-------------------|
| **Google** | Vertex AI Vector Search, Firestore Vector, AlloyDB AI, Spanner, Bigtable, Memorystore | Pinecone, Weaviate on GCP Marketplace |
| **Microsoft** | Azure AI Search, Cosmos DB (DiskANN), Azure SQL | Pinecone, Qdrant on Azure Marketplace |
| **AWS** | OpenSearch Serverless, MemoryDB, Aurora pgvector | Pinecone, Weaviate, Milvus on AWS Marketplace |
| **Open Source** | Chroma, Qdrant, Weaviate, Milvus, pgvector | Self-hosted or managed (Pinecone, Zilliz) |

### Observability & Tracing

| Provider | Native Observability | Standard |
|----------|---------------------|----------|
| **Google** | Cloud Trace, Cloud Monitoring, Cloud Logging | OpenTelemetry |
| **Microsoft** | Application Insights, unified multi-agent tracing | OpenTelemetry (contributing new multi-agent conventions) |
| **AWS** | CloudWatch, X-Ray, AgentCore Evaluations (13 evaluators) | CloudWatch-native |
| **Anthropic** | Console usage dashboard | — (relies on third-party: Langfuse, Datadog) |
| **OpenAI** | Platform usage dashboard | — (relies on third-party) |
| **Open Source** | Langfuse (open-source), LangWatch, Agenta | OpenTelemetry GenAI Semantic Conventions |

### Deployment Models

| Provider | Serverless | Containers | Edge/Local | Unique |
|----------|-----------|------------|------------|--------|
| **Google** | Cloud Functions, Agent Engine | Cloud Run, GKE | Gemma on-device | Agent Engine per-second billing |
| **Microsoft** | Azure Functions | Container Apps, AKS | Phi on-device, ONNX | M365 Copilot integration |
| **AWS** | Lambda (+ Durable Functions), AgentCore | ECS, EKS | — | Lambda I/O wait = no charge |
| **Anthropic** | API only | — | — | Batch API (50% off) |
| **OpenAI** | API only | — | — | Responses API server-side tools |
| **Open Source** | Together AI, Groq, Fireworks | Ollama, vLLM, TGI | Ollama local, LM Studio | Full control, no vendor dependency |

### Local Inference Infrastructure

| Tool | Use Case | Key Capability |
|------|----------|---------------|
| **Ollama** | Easiest local deployment | One-command model download and serving; OpenAI-compatible API; streaming tool calls |
| **vLLM** | Production self-hosting | PagedAttention (50%+ memory savings), 2-4x throughput for concurrent requests |
| **Hugging Face TGI** | Production alternative | High-throughput, extensive model support |
| **LM Studio** | Desktop experimentation | GUI-based, beginner-friendly |

### Readings

1. **Docs:** [Vertex AI Vector Search](https://cloud.google.com/vertex-ai/docs/vector-search/overview) — Google
2. **Docs:** [Azure Cosmos DB Vector Search](https://learn.microsoft.com/en-us/azure/cosmos-db/vector-search) — Microsoft
3. **Docs:** [OpenSearch Serverless Vector Engine](https://aws.amazon.com/opensearch-service/serverless-vector-database/) — AWS
4. **Blog:** [Azure AI Foundry: Advancing OpenTelemetry and Delivering Unified Multi-Agent Observability](https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/azure-ai-foundry-advancing-opentelemetry-and-delivering-unified-multi-agent-obse/4456039) — Microsoft
5. **Blog:** [AI Agent Observability](https://opentelemetry.io/blog/2025/ai-agent-observability/) — OpenTelemetry
6. **Docs:** [Ollama](https://ollama.com/) — Ollama
7. **Docs:** [vLLM](https://docs.vllm.ai/) — vLLM

### Exercises

- [ ] Design a vector database selection matrix for three scenarios: (a) a prototype with <100K documents, (b) a production system with 10M documents on GCP, (c) a multi-cloud enterprise with data residency requirements. Justify your choice for each.
- [ ] Compare observability capabilities: deploy a simple agent that uses 2 tools, and trace it with (a) Google Cloud Trace, (b) Langfuse (open-source), and (c) Azure Application Insights. Compare trace detail, setup effort, and cost.

---

## Unit 12.5: Standards & Protocols

### The Standards Landscape

Three protocols are shaping agent interoperability:

| Standard | Created By | Governed By | Purpose | Adoption Status |
|----------|-----------|-------------|---------|----------------|
| **MCP** (Model Context Protocol) | Anthropic (Nov 2024) | AAIF / Linux Foundation (Dec 2025) | Agent-to-tool connectivity | De facto universal standard: 97M+ monthly SDK downloads, 10K+ servers. Adopted by OpenAI, Google, Microsoft, AWS |
| **A2A** (Agent-to-Agent Protocol) | Google (Apr 2025) | Linux Foundation (Jun 2025) | Agent-to-agent communication | 100+ supporters (AWS, Microsoft, Cisco, SAP). v0.3 with gRPC. Slower momentum vs MCP |
| **Agent Skills** | Anthropic (Dec 2025) | Open standard at agentskills.io | Procedural knowledge for agents | Beta. Partners: Atlassian, Canva, Cloudflare, Figma, Notion, Ramp, Sentry |

Additional standards:
- **AGENTS.md** (OpenAI) — machine-readable agent capability descriptions; donated to AAIF
- **OpenTelemetry GenAI Semantic Conventions** — standardized agent tracing (contributions from Microsoft, Google, IBM)

### Provider Stance on Standards

| Provider | MCP | A2A | Agent Skills | OpenTelemetry |
|----------|-----|-----|-------------|---------------|
| **Anthropic** | Creator; donated to AAIF | Not adopted | Creator; open standard | Third-party integration |
| **OpenAI** | Adopted (Mar 2025); AAIF co-founder | Not adopted | — | Third-party integration |
| **Google** | Full support (managed MCP servers for GCP services) | Creator; donated to Linux Foundation | — | Cloud Trace (OTel-based) |
| **Microsoft** | Copilot Studio + Foundry MCP Server | Adopted (May 2025) | — | Contributing new multi-agent conventions |
| **AWS** | AgentCore Gateway (MCP bridge for any API) | Founding validator | — | CloudWatch (X-Ray) |
| **Open Source** | Universal adoption (LangChain, LlamaIndex, CrewAI, etc.) | Limited adoption | — | GenAI Semantic Conventions active development |

### MCP vs A2A: Complementary or Competing?

| Dimension | MCP | A2A |
|-----------|-----|-----|
| **Scope** | Agent ↔ Tool | Agent ↔ Agent |
| **Analogy** | USB — connect any peripheral | HTTP — communicate between services |
| **Transport** | Stdio, Streamable HTTP | HTTP, gRPC (v0.3) |
| **Discovery** | Server declares tools, resources, prompts | Agent Cards for capability advertisement |
| **Governance** | AAIF (Linux Foundation) | Linux Foundation |
| **Momentum** | Dominant (97M+ monthly downloads) | Growing but slower (100+ supporters) |

Google positions them as complementary: MCP for tool access, A2A for agent collaboration. In practice, MCP's broader adoption means many teams use MCP for both agent-to-tool and lightweight agent-to-agent communication.

### The AAIF (Agentic AI Foundation)

Launched December 2025 under the Linux Foundation. Co-founded by Anthropic, Block, and OpenAI. Supported by Google, Microsoft, AWS, Cloudflare, Bloomberg. Three founding projects:
- **MCP** (Anthropic) — agent-to-tool protocol
- **goose** (Block) — open-source AI agent framework
- **AGENTS.md** (OpenAI) — agent capability descriptions

### Readings

1. **Blog:** [Donating the Model Context Protocol](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation) — Anthropic
2. **Press:** [Linux Foundation Announces the Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) — Linux Foundation
3. **Blog:** [Google Cloud Donates A2A to Linux Foundation](https://developers.googleblog.com/en/google-cloud-donates-a2a-to-linux-foundation/) — Google
4. **Blog:** [Equipping Agents for the Real World with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) — Anthropic
5. **Blog:** [Empowering Multi-Agent Apps with the Open A2A Protocol](https://www.microsoft.com/en-us/microsoft-cloud/blog/2025/05/07/empowering-multi-agent-apps-with-the-open-agent2agent-a2a-protocol/) — Microsoft
6. **Blog:** [Announcing Official MCP Support for Google Services](https://cloud.google.com/blog/products/ai-machine-learning/announcing-official-mcp-support-for-google-services) — Google
7. **Docs:** [MCP with Amazon Bedrock Agents](https://aws.amazon.com/blogs/machine-learning/harness-the-power-of-mcp-servers-with-amazon-bedrock-agents/) — AWS

### Exercises

- [ ] Create a standards adoption matrix: for each of the six ecosystems, list which standards they support natively, which they've contributed to, and which they've created. What does this tell you about their strategic intent?
- [ ] Design a multi-agent architecture for a hypothetical enterprise that uses agents from three different providers (e.g., a Claude agent for research, a GPT agent for coding, a Gemini agent for data analysis). How would MCP and/or A2A enable this? What are the practical challenges?

---

## Unit 12.6: Pricing & Total Cost of Ownership

### API Token Pricing Comparison (per 1M tokens, March 2026)

#### Flagship Models

| Provider | Model | Input | Output | Cached Input |
|----------|-------|-------|--------|-------------|
| **Anthropic** | Opus 4.6 | $5.00 | $25.00 | $0.50 (90% saving) |
| **OpenAI** | GPT-5 | $1.25 | $10.00 | $0.125 (90% saving) |
| **Google** | Gemini 3 Pro | $2.00 | $12.00 | — |
| **AWS** | Nova 2 Pro | $1.25 | $10.00 | — |
| **Azure** | GPT-4o (via Azure OpenAI) | $5.00 | $15.00 | — |

#### Mid-Tier Models

| Provider | Model | Input | Output |
|----------|-------|-------|--------|
| **Anthropic** | Sonnet 4.6 | $3.00 | $15.00 |
| **OpenAI** | GPT-4.1 | $2.00 | $8.00 |
| **Google** | Gemini 2.5 Pro | $1.25 | $10.00 |
| **AWS** | Nova 2 Lite | $0.30 | $2.50 |
| **Azure** | o3 | $2.00 | $8.00 |

#### Budget Models

| Provider | Model | Input | Output |
|----------|-------|-------|--------|
| **Anthropic** | Haiku 4.5 | $1.00 | $5.00 |
| **OpenAI** | GPT-4.1-nano | $0.10 | $0.40 |
| **Google** | Gemini 2.5 Flash-Lite | $0.10 | $0.40 |
| **AWS** | Nova Micro | $0.035 | $0.14 |

#### Reasoning Models

| Provider | Model | Input | Output | Notes |
|----------|-------|-------|--------|-------|
| **Anthropic** | Opus 4.6 (extended thinking) | $5.00 | $25.00 | Thinking tokens billed as output |
| **OpenAI** | o3 | $2.00 | $8.00 | Reasoning tokens billed as output |
| **OpenAI** | o3-pro | $20.00 | $80.00 | Maximum reasoning compute |
| **Google** | Gemini 2.5 Pro (thinking) | $1.25 | $10.00 | Configurable thinking budget |

### Cost Reduction Mechanisms

| Mechanism | Anthropic | OpenAI | Google | AWS |
|-----------|-----------|--------|--------|-----|
| **Prompt caching** | 90% savings (5-min), breakpoints | 50-90% (automatic) | Context caching API | Application-level |
| **Batch processing** | 50% discount (24hr) | 50% discount (24hr) | Batch prediction | 50% on supported models |
| **Free tier** | — | — | 5-15 RPM, 1K req/day | — |
| **Fine-tuning** | — | Available (GPT-4.1, 4o) | SFT, RFT on Vertex AI | Nova Forge ($100K+) |

### Platform & Infrastructure Costs

| Service | Google | Microsoft | AWS |
|---------|--------|-----------|-----|
| **Agent runtime** | $0.00994/vCPU-hr | Free (pay for tokens/tools) | Per-second, no charge during I/O wait |
| **Vector search** | Vertex AI Vector Search (usage-based) | Azure AI Search ($249+/mo standard) | OpenSearch Serverless (~$350+/mo minimum) |
| **Enterprise AI suite** | Gemini Enterprise (Agentspace) | M365 Copilot ($30/user/mo) | Amazon Q ($20-25/user/mo) |

### Self-Hosting Economics

| Factor | Threshold | Detail |
|--------|----------|--------|
| **Break-even point** | ~2M tokens/day | Below this, APIs are typically cheaper |
| **7B-13B model** | 1x 80GB GPU | ~$2-3K/mo cloud (A100/H100) |
| **70B model** | 4-8x GPUs | ~$8-12K/mo cloud |
| **Hidden costs** | +$200K initial | 2 engineers x 4 months deployment engineering |
| **Ongoing ops** | +$12-15K/mo | On-call, monitoring, upgrades |
| **Peak handling** | 2-4x overprovisioning | GPU capacity for traffic spikes |

**Rule of thumb:** Self-hosting saves money at scale (>2M tokens/day, >70% utilization) but adds significant operational complexity. A hybrid approach — open-source models for high-volume simple tasks, API models for complex reasoning — often delivers the best cost/quality tradeoff.

### Readings

1. **Docs:** [Anthropic API Pricing](https://platform.claude.com/docs/en/about-claude/pricing) — Anthropic
2. **Docs:** [OpenAI API Pricing](https://openai.com/api/pricing/) — OpenAI
3. **Docs:** [Gemini API Pricing](https://ai.google.dev/gemini-api/docs/pricing) — Google
4. **Docs:** [Amazon Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/) — AWS
5. **Docs:** [Azure OpenAI Pricing](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/openai-service/) — Microsoft
6. **Article:** [LLM Total Cost of Ownership 2025](https://www.ptolemay.com/post/llm-total-cost-of-ownership) — Ptolemay
7. **Article:** [Self-Hosting LLMs: Hidden Costs](https://azumo.com/artificial-intelligence/ai-insights/self-hosting-llms-cost) — Azumo

### Exercises

- [ ] Build a cost model spreadsheet: for a workload of 10M tokens/day (70% input, 30% output), calculate the monthly cost using: (a) Anthropic Sonnet 4.6, (b) OpenAI GPT-4.1, (c) Google Gemini 2.5 Pro, (d) AWS Nova 2 Pro, (e) self-hosted Llama 4 70B on 4x A100s. Include prompt caching and batch savings where applicable.
- [ ] For the same workload, calculate the cost if 80% of requests can be handled by a budget model (Haiku, nano, Flash-Lite, Nova Micro) with only 20% requiring the mid-tier model. How much does intelligent routing save?

---

## Unit 12.7: Decision Framework

### When to Choose Each Provider

| Scenario | Recommended | Why |
|----------|------------|-----|
| **Best reasoning quality** | Anthropic (Opus 4.6) or OpenAI (o3-pro) | Highest benchmark scores on complex reasoning |
| **Lowest cost at scale** | AWS Nova Micro/Lite or Google Flash-Lite | $0.035-0.10 input per 1M tokens |
| **Enterprise M365 integration** | Microsoft Azure | Native Copilot, Teams, SharePoint integration |
| **Multi-model flexibility** | AWS Bedrock | ~100 models, single API, switch without code changes |
| **Data sovereignty / air-gapped** | Open Source (Ollama + Llama/Qwen/DeepSeek) | Full control, no data leaves your network |
| **Agent developer experience** | Anthropic (Claude Code) or OpenAI (Agents SDK) | Purpose-built agent SDKs with best DX |
| **Managed agent runtime** | Google (Agent Engine) or AWS (AgentCore) | Framework-agnostic, per-second billing |
| **Coding agent** | Anthropic (Claude Code) | ~$2.5B ARR, dominant market position |
| **Multimodal (video + audio)** | Google (Gemini) | Native video/audio understanding |
| **Maximum context window** | Google (Gemini 2M) or Meta (Llama 4 Scout 10M) | Longest context windows available |
| **Regulated industry** | Azure or AWS | Most compliance certifications, data residency options |
| **Startup / prototyping** | Google (free tier) or Open Source (Ollama) | Zero cost to start |

### Multi-Provider Architecture Patterns

**Pattern 1: Model Routing**
Use different providers for different task types:
- Budget model (Nova Micro / Flash-Lite) for classification and routing
- Mid-tier model (Sonnet / GPT-4.1) for standard agent tasks
- Frontier model (Opus / o3) for complex reasoning
- Open-source model (Llama / Qwen) for high-volume, low-complexity tasks

**Pattern 2: Cloud + Model Separation**
- Cloud infrastructure on one provider (GCP, Azure, or AWS)
- Models from a different provider via API (Anthropic or OpenAI)
- Tools connected via MCP (provider-neutral)
- Example: AWS infrastructure + Claude models + MCP tools

**Pattern 3: Hybrid Self-Hosted + API**
- Self-host open-source models for high-volume, latency-sensitive tasks
- API calls to frontier models for complex reasoning
- Route based on task complexity, cost budget, or latency requirements

### Migration & Portability Checklist

| Component | Portability | Migration Effort |
|-----------|------------|-----------------|
| **MCP tools** | High — standard protocol | Low (reuse across all providers) |
| **Agent logic** | Medium — varies by SDK | Medium (rewrite SDK-specific code) |
| **Prompt engineering** | Low — model-specific | High (prompts need re-tuning per model) |
| **Vector embeddings** | Low — model-specific | High (re-embed entire corpus) |
| **Cloud infrastructure** | Low — provider-specific | High (rebuild deployment pipeline) |
| **Evaluation datasets** | High — provider-neutral | Low (reuse across all providers) |

### Readings

1. **Report:** [The State of AI in 2025](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai) — McKinsey
   - 88% of enterprises use AI; only 6% are "AI high performers"
2. **Guide:** [Best AI Agent Frameworks in 2025](https://langwatch.ai/blog/best-ai-agent-frameworks-in-2025-comparing-langgraph-dspy-crewai-agno-and-more) — LangWatch
3. **Guide:** [Comparing Open-Source AI Agent Frameworks](https://langfuse.com/blog/2025-03-19-ai-agent-comparison) — Langfuse

### Exercises

- [ ] You are the CTO of a 500-person fintech company. Your agents need to: (1) process customer support tickets, (2) analyze financial documents, (3) generate compliance reports, and (4) answer employee questions from an internal knowledge base. Design a multi-provider architecture. Justify each provider choice, estimate monthly costs, and identify lock-in risks.
- [ ] Create a migration plan: your team currently uses OpenAI GPT-4o for all agent workloads via Azure OpenAI. Leadership wants to evaluate switching the reasoning-heavy workloads to Anthropic Claude and the high-volume classification to self-hosted Qwen 3. What changes? What stays? Estimate the effort and cost impact.

---

## Checkpoint: Module 12

**Produce an Ecosystem Comparison Report:**

Write a structured comparison document (3-5 pages) that a technical leader could use to make provider selection decisions. Include:

1. **Executive summary**: 1-paragraph positioning of each ecosystem
2. **Models & APIs table**: flagship, mid-tier, and budget models with key capabilities and pricing
3. **Agent platform comparison**: SDKs, managed runtimes, and unique capabilities
4. **Standards posture**: MCP, A2A, Agent Skills adoption for each provider
5. **Cost analysis**: for a defined workload (you choose the parameters), compare monthly costs across at least 4 providers
6. **Recommendation matrix**: which provider for which use case, with justification
7. **Risk assessment**: lock-in risks, portability concerns, and mitigation strategies

- [ ] Report covers all six ecosystems with accurate, current data
- [ ] Pricing analysis uses real numbers with sources cited
- [ ] Recommendations are justified with specific technical reasoning
- [ ] Lock-in and portability risks are clearly identified
- [ ] All exercises from Units 12.1-12.7 completed

---

## Frontier & 2026-27 Outlook

The agentic AI ecosystem is consolidating rapidly around a few key dynamics. **MCP has won the tool protocol war** — with 97M+ monthly SDK downloads and adoption by every major provider, it is the de facto standard for agent-to-tool connectivity. A2A remains relevant for agent-to-agent communication but has not achieved the same ubiquity. The Agentic AI Foundation (AAIF), co-founded by Anthropic, Block, and OpenAI under the Linux Foundation, signals that even fierce competitors see value in shared infrastructure standards.

**The model pricing race is compressing margins dramatically.** AWS Nova Micro at $0.035/1M input tokens and Google Flash-Lite at $0.10/1M input tokens have made basic AI capabilities essentially free at moderate volumes. Meanwhile, frontier reasoning models (o3-pro at $20/1M input, Opus 4.6 at $5/1M input) maintain significant premiums, creating a 500x price range within the market. This bifurcation is driving adoption of **intelligent routing** — using cheap models for simple tasks and expensive models only when needed. The open-source ecosystem continues to close the gap: Qwen 3 and DeepSeek-R1 perform competitively with commercial models at the 70B+ parameter range, and the benchmark gap has narrowed from 15-20 points in 2024 to approximately 9 points in early 2026.

**Managed agent runtimes are the new battleground.** Google's Vertex AI Agent Engine, AWS's Bedrock AgentCore, and Microsoft's Azure AI Foundry Agent Service are all competing on framework-agnosticism, per-second billing, and built-in capabilities (memory, evaluation, guardrails). The key differentiator is shifting from "which models do you support?" (all support many) to "how easily can I deploy, observe, evaluate, and govern agents in production?" Anthropic and OpenAI, as model labs without cloud infrastructure, face a strategic question: build their own platforms or partner with hyperscalers for deployment. Claude Code's ~$2.5B ARR demonstrates that a model lab can build a massive product business, but enterprise deployment still typically runs through AWS Bedrock, Azure OpenAI, or Google Vertex AI.

### Watch List
- **MCP ecosystem maturation** — MCP Apps (interactive UI components from tool calls) and managed MCP servers from cloud providers are extending the protocol beyond simple tool connectivity
- **Agent runtime convergence** — all three hyperscalers converging on framework-agnostic, per-second, consumption-based agent runtimes; differentiation shifting to memory, evaluation, and governance
- **Model commodity pricing** — budget models approaching $0.01/1M tokens; intelligent routing between cheap and expensive models becoming a core architectural pattern
- **Open-source reasoning models** — DeepSeek-R1 (MIT license) and Qwen 3 reasoning variants narrowing the gap with o3/Opus on complex tasks
- **Standards consolidation** — will A2A merge into or complement MCP? Will Agent Skills become an industry standard? Watch AAIF governance decisions
