# Module 4: Memory & RAG

**Time estimate:** 1 week
**Goal:** Give agents long-term memory and the ability to retrieve relevant information from large knowledge bases.

---

## Learning Objectives

By the end of this module you will:
- Implement short-term (conversation) and long-term (persistent) memory for agents
- Build a RAG pipeline from scratch: embed → store → retrieve → generate
- Understand the MemGPT architecture and self-editing memory
- Design effective context windows for agents (context engineering)

---

## Unit 4.1: Agent Memory Types

### Readings

1. **Whitepaper:** [Agents — Section on Memory](https://www.kaggle.com/whitepaper-agents) — Google
   - Covers short-term (in-context) vs long-term (external storage) memory
2. **Survey:** [A Survey on LLM-based Autonomous Agents — Section 2.2 (Memory)](https://arxiv.org/abs/2308.11432) — Wang et al.
   - Taxonomizes memory: sensory, short-term, long-term; episodic, semantic, procedural
3. **Blog:** [Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Anthropic

### Memory Taxonomy

| Type | Duration | Implementation | Example |
|------|----------|---------------|---------|
| **Working memory** | Current turn | System prompt + recent messages | "The user just asked about Python" |
| **Short-term / Session** | Current conversation | Full message history (sliding window) | Multi-turn chat context |
| **Long-term / Episodic** | Across sessions | Vector DB or key-value store | "Last week user asked about React hooks" |
| **Semantic** | Permanent knowledge | RAG over documents | Company documentation, codebase |
| **Procedural** | Learned skills | Code/prompt library | "When user says X, use tool Y" |

### Exercises

- [ ] Take your Module 2 agent and add session memory: maintain full conversation history, but implement a sliding window that summarizes old messages when the context gets too long.
- [ ] Add a simple long-term memory: after each conversation, save a summary to a JSON file. At the start of the next conversation, load relevant summaries into the system prompt.

---

## Unit 4.2: RAG (Retrieval-Augmented Generation)

### Readings

1. **Free course:** [Retrieval Augmented Generation](https://learn.deeplearning.ai/courses/retrieval-augmented-generation/information) — DeepLearning.AI
2. **Guide:** [Pinecone RAG Guide](https://www.pinecone.io/learn/retrieval-augmented-generation/)
3. **Docs:** [LlamaIndex — Building an Agent](https://developers.llamaindex.ai/python/framework/understanding/agent/)

### RAG Pipeline Components

```
Documents → Chunk → Embed → Store (Vector DB) → Query → Retrieve → Augment Prompt → Generate
```

| Step | What It Does | Tools |
|------|-------------|-------|
| **Chunk** | Split documents into ~500 token segments | RecursiveTextSplitter, semantic chunking |
| **Embed** | Convert chunks to vectors | OpenAI embeddings, Cohere, open-source models |
| **Store** | Index vectors for fast similarity search | ChromaDB, Pinecone, Qdrant, pgvector |
| **Retrieve** | Find top-K relevant chunks for a query | Cosine similarity, MMR, hybrid search |
| **Augment** | Insert retrieved chunks into the LLM prompt | System prompt or user message injection |
| **Generate** | LLM produces answer grounded in retrieved context | Any LLM |

### Exercises

- [ ] Build a RAG pipeline from scratch (no frameworks): chunk a PDF, embed with OpenAI, store in ChromaDB, retrieve and answer questions. (For PDF parsing, use [PyPDF2](https://pypi.org/project/PyPDF2/) or [pdfplumber](https://pypi.org/project/pdfplumber/). For text splitting, use [LangChain's RecursiveCharacterTextSplitter](https://python.langchain.com/docs/how_to/recursive_text_splitter/) or write your own.)
- [ ] Experiment with chunk sizes (200 vs 500 vs 1000 tokens). How does it affect retrieval quality?
- [ ] Implement hybrid search: combine vector similarity with keyword search (BM25). Compare to pure vector search.
- [ ] Build a RAG agent: an agent that decides **when** to search the knowledge base vs when to answer from its own knowledge.

---

## Unit 4.3: MemGPT and Self-Editing Memory

### Readings

1. **Paper:** [MemGPT: Towards LLMs as Operating Systems](https://arxiv.org/abs/2310.08560) — Packer et al., 2023
   - Key insight: agents can manage their own memory like an OS manages virtual memory
   - Read: Abstract + Section 3 (MemGPT architecture)
2. **Docs:** [Letta (MemGPT) — Concepts](https://docs.letta.com/concepts/memgpt/)
3. **Guide:** [Letta Memory Management](https://docs.letta.com/advanced/memory-management/)
4. **Course:** [LLMs as Operating Systems: Agent Memory](https://www.letta.com/blog/deeplearning-ai-llms-as-operating-systems-agent-memory) — Letta x DeepLearning.AI

### Alternative Approaches to Agent Memory

MemGPT is one approach. Simpler alternatives exist:
- **Append-only log**: save every conversation turn to a file; retrieve relevant ones by embedding search. Simple, but grows unbounded.
- **Summary-based**: periodically summarize old conversations and replace them with the summary. Loses detail but controls size.
- **Structured key-value**: save specific facts as key-value pairs (e.g., `{"user_name": "Emmanuel", "preferred_language": "Python"}`). Precise but limited.
- MemGPT's self-editing approach is the most flexible but also the most complex. Start simple, add complexity only if needed (echoing Anthropic's "Building Effective Agents" advice).

### Key Architecture

```
Core Memory (always in context)
├── Persona block: agent's identity and instructions
└── Human block: what the agent knows about the user

Recall Memory (conversation search)
└── Full conversation history, searchable by query

Archival Memory (long-term knowledge)
└── Unlimited storage, retrieved on demand
```

The agent can **edit its own core memory** — it has tools like `core_memory_append`, `core_memory_replace`, `archival_memory_insert`, `archival_memory_search`.

### Exercises

- [ ] Implement a simplified MemGPT pattern: give your agent tools to read/write a persistent `memory.json` file. The agent decides what to remember and what to forget.
- [ ] Test: have a multi-session conversation where you tell the agent your preferences in session 1, then verify it remembers them in session 2.

---

## Unit 4.4: Context Engineering

### Readings

1. **Blog:** [Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Anthropic
2. **Kaggle whitepaper:** [Context Engineering (from Day 3)](https://www.kaggle.com/whitepaper-agents) — Google
3. **Blog:** [Writing Tools for Agents](https://www.anthropic.com/engineering/writing-tools-for-agents) — Anthropic
   - Includes guidance on keeping tool results concise to save context space

### Key Concepts

- **Context window budget** — you have N tokens; how do you allocate them between system prompt, memory, retrieved docs, conversation history, and tool results?
- **Summarization** — compress old conversation history into summaries to save tokens
- **Relevance filtering** — only inject retrieved documents that are actually relevant (score threshold)
- **Dynamic context assembly** — the content of the prompt changes based on the query

### Exercise

- [ ] Design a context budget for an agent with a 128K context window. How many tokens for: system prompt, core memory, retrieved docs, conversation history, and tool results? Justify your allocation.

---

## Checkpoint: Module 4

**Build a research agent with memory:**

**Core requirements:**
1. **RAG**: can answer questions about a set of documents you provide (at least 10 pages)
2. **Session memory**: maintains conversation context within a session
3. **Long-term memory**: remembers user preferences and facts across sessions (persisted to disk)

Test it across 3 separate sessions:
- Session 1: Tell it your name, what topics you care about. Ask it a question about your documents.
- Session 2: Ask it what your name is (should remember). Ask a follow-up question about the documents.
- Session 3: Tell it your interests changed. Verify it updates its long-term memory.

**Stretch goal (advanced):**
4. **Self-editing memory**: the agent has tools to save/update/delete its own memories (MemGPT pattern from Unit 4.3). Only attempt this after the core requirements work.

- [ ] RAG pipeline works
- [ ] Session memory works
- [ ] Long-term memory persists across sessions
- [ ] (Stretch) Agent can update its own memories
