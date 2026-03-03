# Module 5: Multi-Agent Systems

**Time estimate:** 1 week
**Goal:** Build systems where multiple specialized agents collaborate to solve complex tasks.

---

## Learning Objectives

By the end of this module you will:
- Design multi-agent architectures with clear role separation
- Implement orchestration patterns: sequential, parallel, hierarchical, and debate
- Understand agent interoperability protocols (MCP, A2A)
- Build a multi-agent system that outperforms a single agent on a complex task

---

## Unit 5.1: Why Multi-Agent?

### Readings

1. **Blog:** [Why Multi-Agent Workflows Fail (and How to Fix Them)](https://github.blog/ai-and-ml/generative-ai/multi-agent-workflows-often-fail-heres-how-to-engineer-ones-that-dont/) — GitHub Blog
2. **Guide:** [AI Agent Design Patterns](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns) — Microsoft Azure Architecture
3. **Survey:** [Large Language Model based Multi-Agents: A Survey](https://arxiv.org/abs/2402.01680) — Guo et al., 2024
   - Read: Abstract + Section 2 (Profiling and Communication)
4. **Paper:** [Voyager: An Open-Ended Embodied Agent with LLMs](https://arxiv.org/abs/2305.16291) — Wang et al., 2023
   - Example of a sophisticated single-agent system with curriculum, skill library, and environment feedback

### When Multi-Agent Helps vs Hurts

| Use multi-agent when... | Stay single-agent when... |
|------------------------|--------------------------|
| Tasks need different expertise/tools | Task is narrow and well-scoped |
| You want role-based separation of concerns | Adding agents adds latency without benefit |
| Parallel execution speeds things up | Communication overhead dominates |
| Debate/review improves quality | A review step is overkill |

---

## Unit 5.2: Orchestration Patterns

### Readings

1. **Guide:** [OpenAI Multi-Agent Patterns](https://openai.github.io/openai-agents-python/multi_agent/) — OpenAI Agents SDK docs
2. **Blog:** [Developer's Guide to Multi-Agent Patterns in ADK](https://developers.googleblog.com/developers-guide-to-multi-agent-patterns-in-adk/) — Google
3. **Blog:** [Event-Driven Multi-Agent Systems](https://www.confluent.io/blog/event-driven-multi-agent-systems/) — Confluent

### Patterns

**1. Sequential Pipeline**
```
Agent A → Agent B → Agent C → Final Output
```
Each agent processes and passes to the next. Simple, predictable.

**2. Parallel Fan-Out / Fan-In**
```
         ┌→ Agent B ─┐
Agent A ─┤           ├→ Agent D (aggregator)
         └→ Agent C ─┘
```
Independent subtasks run concurrently. Aggregator combines results.

**3. Hierarchical (Manager-Worker)**
```
Manager Agent
├── delegates to → Worker A
├── delegates to → Worker B
└── delegates to → Worker C
```
Manager plans and delegates. Workers execute and report back.

**4. Handoff**
```
Agent A ──handoff──→ Agent B ──handoff──→ Agent C
```
One agent decides it's no longer the right agent and transfers control.

**5. Debate / Critique**
```
Agent A (proposer) ←→ Agent B (critic) → Final Answer
```
Agents argue and refine until consensus. Improves quality on hard problems.

### Exercises

- [ ] Implement pattern 1 (sequential) in raw Python: Agent A researches a topic, Agent B writes an outline, Agent C writes the full article.
- [ ] Implement pattern 3 (hierarchical) using CrewAI or LangGraph: a Manager that delegates to 3 specialized workers.
- [ ] Implement pattern 5 (debate): two agents argue about the best approach to a coding problem. A judge agent picks the winner.

---

## Unit 5.3: Model Context Protocol (MCP)

MCP is an open standard (created by Anthropic) for connecting LLMs to external tools and data sources.

### Readings

1. **Docs:** [MCP Documentation](https://modelcontextprotocol.io/)
2. **Spec:** [MCP Specification (Nov 2025)](https://modelcontextprotocol.io/specification/2025-11-25)
3. **GitHub:** [MCP Repository](https://github.com/modelcontextprotocol/modelcontextprotocol)

### Key Concepts

- **MCP Server** — exposes tools, resources, and prompts via a standard protocol
- **MCP Client** — the LLM host that connects to MCP servers
- **Transport** — stdio (local) or HTTP+SSE (remote)
- **Why it matters** — write a tool once as an MCP server, use it from any MCP-compatible client (Claude, Cursor, VS Code, etc.)

### Exercises

- [ ] Build a simple MCP server in Python that exposes 2 tools (e.g., a file reader and a calculator)
- [ ] Connect your MCP server to an MCP client (e.g., Claude Desktop or your own client)
- [ ] Read the spec: understand Resources, Tools, and Prompts as the three primitives

---

## Unit 5.4: Agent-to-Agent Protocol (A2A)

A2A is an open standard (created by Google, donated to Linux Foundation) for agents to communicate with each other.

### Readings

1. **Docs:** [A2A Protocol Documentation](https://a2a-protocol.org/latest/)
2. **Spec:** [A2A Protocol Specification](https://a2a-protocol.org/latest/specification/)
3. **Codelab:** [A2A Purchasing Concierge](https://codelabs.developers.google.com/intro-a2a-purchasing-concierge) — Google

### Key Concepts

- **Agent Card** — a JSON document describing an agent's capabilities (like a business card)
- **Tasks** — the unit of work exchanged between agents
- **MCP vs A2A** — MCP connects an agent to **tools**; A2A connects an **agent to another agent**

### Exercises

- [ ] Complete the Google A2A codelab (purchasing concierge)
- [ ] Write a 1-paragraph explanation of when you'd use MCP vs A2A vs both

---

## Unit 5.5: Build a Multi-Agent System

### Build Exercise

Build a **code review team** with 3 agents:

1. **Developer Agent** — writes code based on a specification
2. **Reviewer Agent** — reviews the code for bugs, style, and security issues
3. **Tester Agent** — writes test cases for the code and checks if they pass

**Orchestration:** Sequential pipeline (Developer → Reviewer → Developer revises → Tester → final output)

**Requirements:**
- Each agent has a distinct system prompt and available tools
- The Reviewer can send code back to the Developer with feedback (at most 2 revision rounds)
- The Tester generates actual test cases (can use a code execution sandbox or just generate assertions)
- Log every inter-agent message for debugging

**Implement in your framework of choice** (LangGraph or CrewAI). Then **write a 1-paragraph reflection:** how does the framework help vs hinder? What would be different in raw Python?

---

## Unit 5.6: Agent Communication Patterns

The orchestration patterns in Unit 5.2 describe *workflow shapes*. This unit covers the underlying *communication mechanisms* agents use to exchange information.

### Patterns

| Pattern | How It Works | Best For |
|---------|-------------|----------|
| **Shared state** | All agents read/write a common state object (e.g., LangGraph's state dict) | Tight coordination, simple data flows |
| **Message passing** | Agents send structured messages to each other (like A2A Tasks) | Loose coupling, async workflows |
| **Blackboard** | Agents post to a shared "board"; any agent can read and react | Emergent collaboration, brainstorming |
| **Event-driven** | Agents subscribe to events and react when relevant events fire | Scalable, decoupled systems |

### Reading

- **Blog:** [Event-Driven Multi-Agent Systems](https://www.confluent.io/blog/event-driven-multi-agent-systems/) — Confluent
  - Four design patterns: orchestrator-worker, hierarchical, blackboard, market-based

### Exercises

- [ ] Identify which communication pattern your Module 5 code review team uses (most likely shared state). Redesign it as message passing — each agent sends a structured message `{"from": "reviewer", "type": "feedback", "content": "..."}` to the next agent.
- [ ] Design (on paper) a blackboard-based system: 3 analyst agents each post research findings to a shared board. A synthesizer agent reads the board and produces a combined report. What data structure would the blackboard use?

---

## Checkpoint: Module 5

- [ ] You can explain 5 multi-agent orchestration patterns and when to use each
- [ ] You can explain 4 agent communication patterns and their tradeoffs
- [ ] You've built at least 2 different multi-agent patterns
- [ ] You understand MCP (agent-to-tool) vs A2A (agent-to-agent)
- [ ] Your code review team works end-to-end
- [ ] You've read the GitHub blog post on why multi-agent workflows fail and can list 3 common failure modes
