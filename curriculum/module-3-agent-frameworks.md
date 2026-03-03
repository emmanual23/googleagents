# Module 3: Agent Frameworks

**Time estimate:** 1-2 weeks
**Goal:** Learn the major agent frameworks, rebuild your agent in each, and understand when to use which.

---

## Learning Objectives

By the end of this module you will:
- Build agents with LangGraph, CrewAI, smolagents, and at least one provider SDK
- Understand the tradeoffs between frameworks
- Make an informed choice of framework for a given problem
- Know when raw Python is better than any framework

---

## Unit 3.1: LangGraph

The most popular framework for building stateful, graph-based agent workflows. From the LangChain team.

### Readings

1. **Docs:** [LangGraph Documentation](https://langchain-ai.github.io/langgraph/)
2. **Free course:** [Intro to LangGraph](https://academy.langchain.com/courses/intro-to-langgraph) — LangChain Academy
3. **Free course:** [AI Agents in LangGraph](https://www.deeplearning.ai/short-courses/ai-agents-in-langgraph/) — DeepLearning.AI
4. **Deeper course:** [LangGraph Essentials](https://academy.langchain.com/courses/langgraph-essentials-python) — LangChain Academy

### Key Concepts

- **State** — a typed dict or Pydantic model that flows through the graph
- **Nodes** — functions that read state, do work, and return updated state
- **Edges** — connections between nodes (can be conditional)
- **Checkpointing** — built-in persistence for pausing/resuming agents
- **Human-in-the-loop** — interrupt the graph, get human input, resume

### Exercises

- [ ] Complete the LangChain Academy Intro to LangGraph course
- [ ] Rebuild your Module 2 research agent as a LangGraph graph with: a router node, tool nodes, and a response node
- [ ] Add human-in-the-loop: before executing any tool, pause and ask the user for confirmation
- [ ] Add checkpointing: save state to SQLite, kill the script, restart, and resume from where you left off

---

## Unit 3.2: CrewAI

A role-based multi-agent framework. You define agents with roles, goals, and backstories, then organize them into crews.

### Readings

1. **Docs:** [CrewAI Documentation](https://docs.crewai.com/)
2. **Quickstart:** [CrewAI Quickstart](https://docs.crewai.com/en/quickstart)
3. **Free course:** [Multi-AI Agent Systems with CrewAI](https://www.deeplearning.ai/short-courses/multi-ai-agent-systems-with-crewai/) — DeepLearning.AI
4. **Examples:** [CrewAI Quickstarts (GitHub)](https://github.com/crewAIInc/crewAI-quickstarts)

### Key Concepts

- **Agent** — has a role, goal, backstory, and available tools
- **Task** — a specific unit of work assigned to an agent
- **Crew** — a team of agents that collaborate (sequential or hierarchical)
- **Process** — how the crew is orchestrated (sequential, hierarchical, or consensual)

### Exercises

- [ ] Complete the DeepLearning.AI CrewAI short course
- [ ] Build a content creation crew: Researcher agent (searches web), Writer agent (drafts article), Editor agent (reviews and improves). Run them sequentially.
- [ ] Try hierarchical process: add a Manager agent that delegates to the others
- [ ] Compare: how does the same task feel in CrewAI vs your raw Python agent vs LangGraph?

---

## Unit 3.3: Hugging Face smolagents

A minimal (~1000 lines of code), code-first agent library. Agents write Python code as their actions instead of calling named tools.

### Readings

1. **Docs:** [smolagents Documentation](https://huggingface.co/docs/smolagents/en/index)
2. **Tutorial:** [Build Your First Agent](https://huggingface.co/learn/agents-course/en/unit1/tutorial) — HF Agents Course
3. **Tools guide:** [smolagents Tools](https://huggingface.co/docs/smolagents/en/tutorials/tools)

### Key Concepts

- **Code agents** — the LLM writes executable Python code as its action (not just tool names)
- **Tool Hub** — tools can be shared on the Hugging Face Hub
- **Minimal abstraction** — intentionally simple, easy to read the source code

### Exercises

- [ ] Build a smolagent that can search the web and do math. Compare the "code as action" approach vs function calling.
- [ ] Read the smolagents source code (~1000 LOC). Understand the agent loop.
- [ ] Write a custom tool and register it with a smolagent.

---

## Unit 3.4: Provider SDKs (OpenAI Agents SDK, Anthropic Agent SDK)

Each major provider now offers their own agent SDK.

### Readings

1. **OpenAI Agents SDK:** https://openai.github.io/openai-agents-python/
   - Lightweight: Agents, Handoffs, Guardrails, Tracing
   - [GitHub](https://github.com/openai/openai-agents-python)
2. **Anthropic Agent SDK:** https://platform.claude.com/docs/en/agent-sdk/overview
   - Agentic loop with tool use, MCP integration, guardrails
   - [GitHub — Python](https://github.com/anthropics/claude-agent-sdk-python)
3. **Google ADK:** https://google.github.io/adk-docs/
   - Model-agnostic, supports MCP and A2A
   - [GitHub](https://github.com/google/adk-python)

### Exercises

- [ ] Build the same simple agent (web search + calculator) with at least 2 of the 3 provider SDKs
- [ ] Note the differences: how does each SDK handle the agent loop, tool definition, error handling, and tracing?

---

## Unit 3.5: Open-Source & Local Models

Agent frameworks aren't locked to commercial APIs. You can run agents with open-source models locally.

### Readings

1. **Docs:** [Ollama](https://ollama.com/) — run open-source models locally with one command
2. **Docs:** [vLLM](https://docs.vllm.ai/) — high-throughput LLM serving engine for production
3. **Leaderboard:** [Hugging Face Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard) — compare open-source models

### Key Insight

Most agent frameworks (LangGraph, CrewAI, smolagents) support OpenAI-compatible APIs. Ollama and vLLM expose this API, so you can swap in a local model with a 1-line config change.

### Exercises

- [ ] Install Ollama. Pull a model (`ollama pull llama3.1:8b`). Point your LangGraph or CrewAI agent at `http://localhost:11434/v1` using an OpenAI-compatible client. Does the agent still work? Where does it break?
- [ ] Compare: run your Module 2 eval test set against (a) a local 8B model, (b) Claude Sonnet, (c) GPT-4o. Note accuracy, latency, and cost for each.
- [ ] When would you choose local over API? Write 3 scenarios where local models are the right choice (hint: data residency, cost at scale, air-gapped environments).

---

## Unit 3.6: Framework Comparison

### Decision Matrix

| Criterion | Raw Python | LangGraph | CrewAI | smolagents | Provider SDKs |
|-----------|-----------|-----------|--------|------------|---------------|
| **Learning curve** | Lowest | Medium | Low | Low | Low-Medium |
| **Flexibility** | Maximum | High | Medium | Medium | Medium |
| **Multi-agent** | Manual | Subgraphs | Native (Crews) | Limited | Varies |
| **Persistence** | Manual | Built-in | Limited | None | Varies |
| **Human-in-loop** | Manual | Built-in | Callback | None | Varies |
| **Observability** | Manual | LangSmith | Limited | Basic | Built-in trace |
| **Vendor lock-in** | None | LangChain ecosystem | None | HF ecosystem | Provider-locked |
| **Production-ready** | Depends on you | Yes | Growing | No (experimental) | Yes |
| **Best for** | Learning, custom needs | Complex stateful workflows | Role-based teams | Prototyping, education | Provider-aligned apps |

### Exercise

- [ ] Write a 1-page document: "Which framework would I choose for [a project you care about] and why?" Reference specific features.

---

## Checkpoint: Module 3

- [ ] You've built agents in at least LangGraph + CrewAI + one provider SDK
- [ ] You can articulate the tradeoffs between frameworks
- [ ] You've completed at least 2 of the free courses listed in this module
- [ ] You have a written framework comparison for your own reference

**Self-test:** If someone asked you "should I use LangGraph or CrewAI for X?", can you give a nuanced answer?

---

## Frontier & 2026-27 Outlook

The framework landscape has consolidated around graph-based execution while simultaneously fragmenting with new provider-specific SDKs. LangGraph reached v1.0 GA in October 2025 and is used in production by Uber, LinkedIn, and Klarna for complex stateful agent workflows. Meanwhile, every major provider launched their own agent SDK: OpenAI Agents SDK (lightweight: Agents, Handoffs, Guardrails, Tracing), Google ADK (model-agnostic with MCP and A2A support), and Anthropic Claude Agent SDK (agentic loop with built-in MCP integration). Microsoft merged AutoGen and Semantic Kernel into a unified Agent Framework. The trend is clear: graph-based state machines for complex workflows, provider SDKs for simpler single-agent tasks.

The open-source model ecosystem has exploded. Llama 4 introduced mixture-of-experts (MoE) architecture with the Scout variant supporting 10M token context. Qwen3 shipped under Apache 2.0 with strong function-calling capabilities. DeepSeek-R1 demonstrated that pure reinforcement learning can produce frontier-quality reasoning under an MIT license. Mistral 3 and Gemma 3 round out the mid-tier. OpenAI released GPT-OSS, their first open-weights model, which runs on a single 80GB GPU. The benchmark gap between open-source and commercial models has narrowed from 15-20 points to roughly 9 points, making local deployment viable for an increasing range of agent use cases.

### Watch List
- **Framework convergence** — all major frameworks are converging on graph-based execution with nodes, edges, and conditional routing
- **"Agentic mesh" pattern** — agents as microservices in a mesh, communicating via MCP/A2A rather than monolithic multi-agent systems
- **Open-source model parity** — the gap continues to narrow; by 2027, open-source models may match commercial quality for most agent tasks
