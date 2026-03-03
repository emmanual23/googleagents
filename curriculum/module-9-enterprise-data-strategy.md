# Module 9: Enterprise Data & Knowledge Architecture

**Time estimate:** 1 week
**Goal:** Understand how large enterprises structure data to feed AI agents — data architecture, enterprise RAG, knowledge graphs, and open-source/local model options.

---

## Learning Objectives

By the end of this module you will:
- Assess why most enterprise AI agent projects fail (and how to avoid it)
- Design a data architecture that makes enterprise knowledge accessible to agents
- Implement enterprise RAG over heterogeneous data sources with access control
- Build agents that query structured data (SQL, APIs) alongside unstructured data (RAG)
- Evaluate when to use open-source/local models vs commercial APIs for enterprise agents

---

## Unit 9.1: The Enterprise Data Problem

Agents are only as good as the data they can access. In enterprises, data is fragmented across dozens of systems, often dirty, poorly documented, and locked behind inconsistent access controls.

### Readings

1. **Report:** [The State of AI in 2025: Agents, Innovation, and Transformation](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai) — McKinsey / QuantumBlack
   - 88% of orgs use AI in at least one function, but only 6% are "AI high performers" with 5%+ EBIT impact
   - Workflow redesign is the single biggest predictor of enterprise AI success
2. **Report:** [State of Generative AI in the Enterprise](https://www.deloitte.com/us/en/what-we-do/capabilities/applied-artificial-intelligence/content/state-of-generative-ai-in-enterprise.html) — Deloitte
   - 2,800+ director-to-C-suite respondents across 16 countries; 78% plan to increase AI spending
3. **Press release:** [Gartner Identifies Top 10 Strategic Technology Trends for 2025](https://www.gartner.com/en/newsroom/press-releases/2024-10-21-gartner-identifies-the-top-10-strategic-technology-trends-for-2025) — Gartner
   - Agentic AI named #1 strategic technology trend; 15% of day-to-day work decisions predicted to be made by agents by 2028
4. **Press release:** [Gartner Predicts Over 40% of Agentic AI Projects Will Be Canceled by End of 2027](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027) — Gartner
   - 40%+ will fail due to escalating costs, unclear business value, or inadequate risk controls
5. **Report:** [IBM Study: Chief Data Officers Redefine Strategies](https://newsroom.ibm.com/2025-11-13-ibm-study-chief-data-officers-redefine-strategies-as-ai-ambitions-outpace-readiness) — IBM
   - Only 26% of CDOs are confident their data can support AI-enabled revenue; 25%+ of orgs lose $5M+ annually to poor data quality

### Key Statistics

| Stat | Source |
|------|--------|
| 88% of orgs use AI somewhere, but only 6% see meaningful EBIT impact | McKinsey 2025 |
| 40%+ of agentic AI projects will be canceled by 2027 | Gartner 2025 |
| 25%+ of organizations lose $5M+ annually to poor data quality | IBM 2025 |
| 43% of COOs cite data quality as their top priority | IBM 2025 |
| Organizations with governance push 12x more AI projects to production | Databricks 2026 |

### Exercises

- [ ] Read the McKinsey and Gartner reports. Write a 1-page memo: "Why do most enterprise AI agent projects fail?" Identify the top 3 failure modes.
- [ ] For an enterprise you know (employer, client, or public case study), map out where data lives: which systems, what formats, who owns it, what access controls exist.

---

## Unit 9.2: Data Architecture for Agents

### Readings

1. **Guide:** [Data Mesh for AI: Complete Guide](https://www.informatica.com/resources/articles/ai-data-mesh.html) — Informatica
   - Four principles of data mesh: domain ownership, data as a product, self-serve platform, federated governance
2. **Article:** [Unlocking Enterprise Data Connectivity with Data Mesh and AI Mesh](https://dxc.com/insights/knowledge-base/data-mesh-ai-mesh-enterprise-data-connectivity) — DXC Technology
   - Agents dynamically access data via mesh, with high-volume flows structured and low-volume handled by agent swarms
3. **Article:** [AI Agentic Mesh: Building Enterprise Autonomy](https://www.computer.org/publications/tech-news/trends/ai-agentic-mesh) — IEEE Computer Society
   - Defines the Agentic Mesh as a structured, networked fabric for scaling autonomy with governance
4. **Architecture:** [The Agentic Enterprise: IT Architecture for the AI-Powered Future](https://architect.salesforce.com/fundamentals/agentic-enterprise-it-architecture) — Salesforce Architects
   - Adds four new AI-specific layers (including a Semantic Layer) to existing enterprise IT architecture
5. **Architecture:** [Enterprise Agentic Architecture and Design Patterns](https://architect.salesforce.com/fundamentals/enterprise-agentic-architecture) — Salesforce Architects
   - Pattern-based methodology favoring multi-agent over monolithic architectures

### Key Architectural Concepts

| Concept | What It Means for Agents |
|---------|------------------------|
| **Data Mesh** | Decentralized ownership; each domain publishes data as a product that agents can consume |
| **Agentic Mesh** | Agents themselves form a mesh — specialized agents connect dynamically, governed centrally |
| **Semantic Layer** | A unified metadata layer that maps business concepts to physical data, so agents query by meaning not table names |
| **Data Products** | Self-describing, discoverable datasets with SLAs — the ideal input for agent tools |
| **Federated Governance** | Central policies, local enforcement — critical when multiple agents access different data domains |

### Exercises

- [ ] Design (on paper) a data architecture for a hypothetical enterprise with: CRM (Salesforce), internal wiki (Confluence), chat (Slack), code repos (GitHub), and a data warehouse (Snowflake). How would agents access each? What's the integration pattern?
- [ ] Read the Salesforce Agentic Enterprise architecture. Draw the four new AI-specific layers and explain what each does.

---

## Unit 9.3: Enterprise RAG at Scale

Scaling RAG from a demo with 10 documents to an enterprise with millions of documents across dozens of systems.

### Readings

1. **Guide:** [What Are RAG Models? A Guide to Enterprise AI](https://www.glean.com/blog/rag-models-enterprise-ai) — Glean
   - RAG across Confluence, SharePoint, Slack; 25-30% cost reductions and 40% faster information discovery
2. **Guide:** [RAG in 2025: The Enterprise Guide](https://datanucleus.dev/rag-and-agentic-ai/what-is-rag-enterprise-guide-2025) — Data Nucleus
   - How RAG matured from simple retriever-generator into enterprise intelligence architectures
3. **Technical guide:** [Real-time Enterprise RAG with SharePoint](https://pathway.com/blog/enterprise_rag_sharepoint) — Pathway
   - Real-time RAG pipelines with continuous indexing and enterprise-grade permissions
4. **Guide:** [Enterprise RAG Predictions for 2025](https://www.vectara.com/blog/top-enterprise-rag-predictions) — Vectara
   - Agentic RAG: agents decide when and what to retrieve, not just a fixed pipeline
5. **Guide:** [The Definitive Guide to AI-Based Enterprise Search](https://www.glean.com/blog/the-definitive-guide-to-ai-based-enterprise-search-for-2025) — Glean

### Enterprise RAG Challenges (vs Demo RAG)

| Challenge | Demo RAG | Enterprise RAG |
|-----------|----------|----------------|
| **Scale** | 10-100 documents | Millions of documents across dozens of sources |
| **Freshness** | Static index | Real-time indexing as documents change |
| **Access control** | None | Row-level and doc-level permissions must be respected |
| **Data types** | Text/PDF | Text, PDF, slides, spreadsheets, images, code, chat messages |
| **Quality** | Curated | Duplicate, outdated, contradictory information |
| **Connectors** | One source | Confluence, SharePoint, Slack, Google Drive, Notion, Jira, databases, etc. |
| **Latency** | Seconds OK | Sub-second for interactive use cases |
| **Evaluation** | Manual spot-check | Automated eval with ground truth, regression testing |

### Exercises

- [ ] Design (on paper) an enterprise RAG architecture for a 10,000-person company. Include: data sources, connectors, chunking strategy, embedding pipeline, vector store, retrieval strategy, access control, and freshness guarantees.
- [ ] Implement a multi-source RAG prototype: index documents from 2 different sources (e.g., local markdown files + a public API), with metadata-based filtering.
- [ ] Add access control: tag each document with a "department" field. At query time, filter results to only documents the user's department can access.

---

## Unit 9.4: Knowledge Graphs and Structured Data

Enterprise data isn't just unstructured text — it includes databases, CRMs, ERPs, and knowledge graphs.

### Readings

1. **Paper:** [Knowledge Graphs as a Source of Trust for LLM-Powered Enterprise QA](https://www.sciencedirect.com/science/article/pii/S1570826824000441) — Sequeda et al., Journal of Web Semantics, 2024
   - GPT-4 QA on SQL databases: 16% accuracy zero-shot → 54% with knowledge graph representation
2. **Article:** [Knowledge Graphs: The Missing Link in Enterprise AI](https://www.cio.com/article/3808569/knowledge-graphs-the-missing-link-in-enterprise-ai.html) — CIO.com
3. **Trends:** [Top Knowledge Management Trends — 2025](https://enterprise-knowledge.com/top-knowledge-management-trends-2025/) — Enterprise Knowledge
4. **Article:** [Enterprise AI Powered by Knowledge Graphs](https://blog.metaphacts.com/from-data-to-decisions-how-enterprise-ai-powered-by-knowledge-graphs-is-redefining-business-intelligence) — metaphacts

### SQL Prerequisite

> If you're new to SQL, work through [SQLBolt](https://sqlbolt.com/) (free, interactive, ~2 hours) before the exercises below.

### When to Use What

| Data Type | Agent Access Pattern | Technology |
|-----------|---------------------|------------|
| Unstructured docs (wiki, PDFs) | RAG (embed → retrieve) | Vector DB + embeddings |
| Structured data (databases, CRM) | Text-to-SQL or API tools | SQL agent, API wrapper tools |
| Semi-structured (JSON, logs) | Parse + RAG or tool-based query | Custom parsers + RAG |
| Relationships & ontologies | Graph traversal | Knowledge graph (Neo4j, etc.) |
| Real-time data (dashboards, metrics) | Direct API access | Tool calling to live APIs |

### Exercises

- [ ] Build a Text-to-SQL agent: given a SQLite database, the agent converts natural language questions to SQL, executes, and explains results. (Use the [Chinook sample database](https://github.com/lerocha/chinook-database) if you need a dataset.)
- [ ] Build a hybrid agent: add a RAG tool alongside the SQL tool. The agent decides whether to query the database or search documents based on the question.
- [ ] Read the Sequeda et al. paper. Why does a knowledge graph improve accuracy so dramatically?

---

## Unit 9.5: Open-Source & Local Models for Agents

Enterprise data residency, cost constraints, and air-gapped environments often require running models locally.

### Readings

1. **Docs:** [Ollama](https://ollama.com/) — run open-source models locally with one command
2. **Docs:** [vLLM](https://docs.vllm.ai/) — high-throughput LLM serving engine
3. **Guide:** [Hugging Face — Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard) — benchmark comparisons of open-source models
4. **Blog:** [Search Isn't Dead: From RAG to Agents with Vector Databases](https://thenewstack.io/search-isnt-dead-from-rag-to-agents-with-vector-databases/) — The New Stack
   - Sub-200ms latency at billions of vectors is the enterprise standard

### Model Landscape

| Model Family | Sizes | Strengths | Agent Suitability |
|-------------|-------|-----------|-------------------|
| **Llama 3** (Meta) | 8B, 70B, 405B | Strong reasoning, open weights | Good for tool calling at 70B+ |
| **Mistral / Mixtral** | 7B, 8x7B, Large | Efficient, multilingual | Good for European data residency |
| **Qwen 2.5** (Alibaba) | 7B, 72B | Strong coding, function calling | Good for code agents |
| **Gemma 2** (Google) | 9B, 27B | Efficient, permissive license | Good for lightweight agents |

### When to Use Local vs API

| Factor | Use API | Use Local |
|--------|---------|-----------|
| **Data sensitivity** | Data can leave your network | Data must stay on-premise |
| **Latency needs** | Latency tolerance > 1s | Need sub-100ms first token |
| **Scale** | Variable workload | Predictable, high throughput |
| **Budget** | Pay-per-token is cheaper at low volume | GPU ownership is cheaper at high volume |
| **Capability needs** | Need frontier model quality | 70B model is sufficient for your task |

### Exercises

- [ ] Install Ollama and run a 7-8B model locally. Rebuild your Module 1 ReAct agent using the local model. Compare quality vs Claude/GPT on your eval set.
- [ ] Benchmark: run your Module 6 eval harness on (a) a local 8B model, (b) a local 70B model (if your hardware supports it), (c) Claude Sonnet, (d) GPT-4o. Create a cost vs quality chart.
- [ ] Implement model routing with a local model: use the local model for simple tool selection, and the API model for complex reasoning and final answers. Measure cost savings.

---

## Checkpoint: Module 9

**Build an enterprise RAG agent with structured data access:**

1. **Multi-source RAG**: index documents from at least 2 sources with metadata-based access control
2. **Structured data tool**: a Text-to-SQL tool that queries a SQLite database
3. **Hybrid routing**: the agent decides whether to use RAG (unstructured search) or SQL (structured query) based on the question
4. **Access control**: documents are tagged by department; the agent respects user permissions
5. **Local model option**: the agent works with both a commercial API and a local model (via Ollama)

Test with:
- "What was our Q3 revenue?" (should use SQL)
- "What does our PTO policy say about carryover?" (should use RAG)
- "How does our Q3 revenue compare to the targets mentioned in the strategy doc?" (should use both)
- Run as user from Department A — verify they can't see Department B's documents

- [ ] Multi-source RAG works with access control
- [ ] Text-to-SQL tool works
- [ ] Agent routes between RAG and SQL correctly
- [ ] Agent works with both API and local model
- [ ] All exercises from Units 9.1-9.5 completed

---

## Frontier & 2026-27 Outlook

Enterprise AI adoption is accelerating but remains unevenly distributed. **Gartner predicts 40% of enterprise apps will have embedded AI agents by end of 2026**, up from less than 5% in 2025 — a dramatic acceleration. McKinsey's 2025 State of AI report found that **88% of enterprises use AI regularly**, but only 23% are scaling agentic AI specifically. The gap between "experimenting with AI" and "deploying agents in production" remains large, driven primarily by data quality issues (IBM reports 25%+ of organizations lose $5M+ annually to poor data quality) and inadequate governance (Gartner predicts 40%+ of agentic AI projects will be canceled by end of 2027 due to escalating costs and unclear business value).

**GraphRAG for enterprise multi-hop reasoning** has matured from a research concept to a production pattern. Knowledge graph integration enables agents to answer questions that require synthesizing information across multiple documents and data sources — the Sequeda et al. study showed knowledge graph-backed QA jumping from 16% to 54% accuracy on enterprise databases. Open-source models are becoming viable for enterprise deployment: GPT-OSS runs on a single 80GB GPU, Qwen3.5-9B runs on consumer laptops, and DeepSeek-R1 offers frontier reasoning under an MIT license. For enterprises with data residency requirements or air-gapped environments, the open-source ecosystem now provides genuine alternatives to commercial APIs.

### Watch List
- **Enterprise RAG evolution** — from simple retrieve-generate to agentic RAG with GraphRAG, dynamic query refinement, and access-controlled multi-source retrieval
- **Open-source model deployment** — regulated industries (finance, healthcare, government) are evaluating open-source models for on-premise agent deployments where data cannot leave the network
- **Data quality as the bottleneck** — the primary blocker for enterprise agent success is not model capability but data accessibility, quality, and governance
