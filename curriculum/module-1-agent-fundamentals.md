# Module 1: Agent Fundamentals

**Time estimate:** 1 week
**Goal:** Understand what makes an LLM an "agent," learn the core reasoning patterns, and build a ReAct agent from scratch.

---

## Learning Objectives

By the end of this module you will:
- Define what an AI agent is and how it differs from a chatbot
- Explain the ReAct (Reasoning + Acting) loop
- Understand Chain-of-Thought, Tree of Thoughts, and Reflexion patterns
- Build a working ReAct agent in raw Python (no frameworks)
- Read and understand foundational agent papers

---

## Unit 1.1: What Is an AI Agent?

### Readings

1. **Blog:** [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) — Anthropic (Erik Schluntz, Barry Zhang, Dec 2024)
   - Key distinction: **workflows** (developer-defined orchestration) vs **agents** (LLM-directed control flow)
   - Start simple, add complexity only when needed
2. **Blog:** [What is an Agent?](https://blog.langchain.com/what-is-an-agent/) — LangChain
   - Defines the "agentic spectrum" — from simple chains to fully autonomous agents
3. **Whitepaper:** [Agents](https://www.kaggle.com/whitepaper-agents) — Google (Wiesinger, Marlow, Vuskovic, 2024)
   - Comprehensive framework: models, tools, orchestration layer
4. **Course (optional):** [Generative AI for Everyone](https://www.coursera.org/learn/generative-ai-for-everyone) — Andrew Ng
   - Broader context on where agents fit in the GenAI landscape

### Key Concepts

- **Agent = LLM + Tools + Loop** — the LLM decides what action to take, observes the result, and decides the next action
- **Orchestration layer** — the code that manages the agent loop (prompt → LLM → parse action → execute → observe → repeat)
- **Agentic spectrum** — not binary; systems can be more or less agentic
- **When NOT to use agents** — simple retrieval, single-shot tasks, deterministic workflows

---

## Unit 1.2: Reasoning Patterns

### Readings (Papers)

Read these in order — each builds on the previous:

1. **Chain-of-Thought Prompting Elicits Reasoning in Large Language Models**
   - Wei et al., 2022 — [arxiv.org/abs/2201.11903](https://arxiv.org/abs/2201.11903)
   - Key idea: adding "Let's think step by step" dramatically improves reasoning
   - Read: Abstract + Section 2 (Chain-of-Thought) + Section 3 (Results)

2. **ReAct: Synergizing Reasoning and Acting in Language Models**
   - Yao et al., 2022 — [arxiv.org/abs/2210.03629](https://arxiv.org/abs/2210.03629)
   - Key idea: interleave **Thought** (reasoning) → **Action** (tool call) → **Observation** (result) in a loop
   - Read: Abstract + Section 1 (Intro) + Section 3 (ReAct method) + Figure 1

3. **Tree of Thoughts: Deliberate Problem Solving with Large Language Models**
   - Yao et al., 2023 — [arxiv.org/abs/2305.10601](https://arxiv.org/abs/2305.10601)
   - Key idea: explore multiple reasoning paths as a tree, evaluate and backtrack
   - Read: Abstract + Section 2 (Framework)

4. **Reflexion: Language Agents with Verbal Reinforcement Learning**
   - Shinn et al., 2023 — [arxiv.org/abs/2303.11366](https://arxiv.org/abs/2303.11366)
   - Key idea: after failing, the agent writes a verbal self-reflection stored in memory, then retries
   - Read: Abstract + Section 3 (Reflexion framework)

### Exercises

- [ ] For each paper, write a 3-sentence summary: (1) what problem it solves, (2) how it works, (3) what result it achieves
- [ ] Draw the ReAct loop as a flowchart: Prompt → Thought → Action → Observation → (loop or finish)

---

## Unit 1.3: Build a ReAct Agent from Scratch

### Reference

- **Guide:** [Anthropic Cookbook — Agent Patterns](https://github.com/anthropics/anthropic-cookbook/tree/main/patterns/agents)
- **Course:** [AI Agents in LangGraph](https://www.deeplearning.ai/short-courses/ai-agents-in-langgraph/) — first lesson builds an agent from scratch before introducing LangGraph (free)

### Build Exercise

Build a ReAct agent in ~200 lines of Python with **no frameworks**. Use any LLM provider.

**Requirements:**
1. A system prompt that instructs the LLM to use the Thought/Action/Observation format
2. A tool registry: a Python dict mapping tool names to callable functions
3. At least 2 tools:
   - `calculator(expression: str) -> str` — evaluates a math expression
   - `wikipedia_search(query: str) -> str` — fetches the first paragraph from Wikipedia's API
4. A loop that:
   - Sends the conversation to the LLM
   - Parses the response for an Action (tool call) or Final Answer
   - If Action: executes the tool, appends the Observation, loops back
   - If Final Answer: returns the result
   - Max iterations: 10 (prevent infinite loops)
5. Test with: "What is the population of France divided by the population of Belgium?"
   - The agent should search Wikipedia for both populations, then use the calculator

**Implementation tips:**
- Use regex or string parsing to extract Action and Action Input from the LLM response
- Format: `Action: tool_name` / `Action Input: {"param": "value"}` / `Observation: <tool output>`
- Start with the system prompt — this is the most important part

### Stretch Goals

- [ ] Add a `web_search` tool using a free API (e.g., DuckDuckGo)
- [ ] Add retry logic: if the LLM outputs malformed actions, re-prompt with an error message
- [ ] Add logging: print each Thought/Action/Observation step in color (use `rich` library)

---

## Unit 1.4: Survey of Agent Architectures

### Readings (Surveys)

1. **A Survey on Large Language Model based Autonomous Agents**
   - Wang et al., 2023 — [arxiv.org/abs/2308.11432](https://arxiv.org/abs/2308.11432)
   - The first comprehensive survey. Read: Section 1 (Intro) + Section 2 (Agent construction framework)
   - Covers: profile, memory, planning, and action modules

2. **Optional (advanced):** The Rise and Potential of Large Language Model Based Agents: A Survey
   - Xi et al., 2023 — [arxiv.org/abs/2309.07864](https://arxiv.org/abs/2309.07864)
   - 86-page survey. Useful reference for later modules, but not required now. Revisit when you reach Module 5 (multi-agent systems).

### Key Architecture Patterns to Understand

| Pattern | Description | Example |
|---------|-------------|---------|
| **ReAct** | Reason then act, observe, repeat | Q&A with tool use |
| **Plan-and-Execute** | Make a plan first, then execute steps | Complex multi-step tasks |
| **Reflexion** | Try, fail, reflect, retry with memory | Code generation with tests |
| **Tree of Thoughts** | Explore multiple paths, backtrack | Puzzle solving, creative writing |
| **LLM Compiler** | Parallelize independent steps from a plan | Speeding up multi-tool workflows |

---

## Checkpoint: Module 1

**You should now be able to:**

- [ ] Explain the difference between a chatbot, a workflow, and an agent
- [ ] Describe the ReAct loop from memory
- [ ] Have a working ReAct agent in raw Python that uses at least 2 tools
- [ ] Summarize at least 3 of the 4 papers from Unit 1.2

**Self-test:** Give your agent a question it hasn't seen that requires multiple tool calls. Does it solve it? If not, debug the system prompt or parsing logic.
