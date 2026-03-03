# Module 6: Evaluation & Quality

**Time estimate:** 1 week
**Goal:** Build rigorous evaluation systems for agents — because you can't improve what you can't measure.

---

## Learning Objectives

By the end of this module you will:
- Design evaluation criteria for agent systems
- Implement LLM-as-judge evaluation
- Build automated eval pipelines that run on every change
- Understand agent benchmarks (SWE-bench, AgentBench) and what they measure
- Implement human-in-the-loop feedback mechanisms

---

## Unit 6.1: Why Agent Eval Is Hard

Evaluating agents is fundamentally harder than evaluating LLMs because:
- **Non-determinism** — same input can produce different tool call sequences
- **Multi-step** — you need to evaluate the process, not just the final answer
- **Side effects** — agents take actions in the real world (or simulated environments)
- **Partial credit** — an agent might get 70% of the way to a correct answer

### Readings

1. **Guide:** [Agent Quality (from Kaggle 5-Day Course)](https://www.kaggle.com/whitepaper-agents) — Google
   - Covers observability (logs, traces, metrics), evaluation frameworks, feedback loops
2. **Guide:** [o-mega AI Agent Benchmarks Guide](https://o-mega.ai/articles/the-best-ai-agent-evals-and-benchmarks-full-2025-guide)
   - Comprehensive overview of all major agent evaluation approaches and benchmarks

---

## Unit 6.2: Evaluation Dimensions

| Dimension | What It Measures | How to Measure |
|-----------|-----------------|----------------|
| **Correctness** | Is the final answer right? | Ground truth comparison, human review |
| **Faithfulness** | Does the answer match retrieved context? | Check citations against sources |
| **Tool accuracy** | Does the agent call the right tools with right args? | Trace analysis |
| **Efficiency** | How many steps/tokens/$ to reach the answer? | Step count, token usage, cost |
| **Safety** | Does the agent avoid harmful actions? | Red-teaming, guardrail tests |
| **Robustness** | Does it handle edge cases and errors? | Adversarial inputs, tool failures |
| **Latency** | How long does it take? | End-to-end timing |

---

## Unit 6.3: LLM-as-Judge

Use a separate LLM to evaluate your agent's outputs. Cheaper and faster than human evaluation, but requires careful prompt design.

### Readings

1. **Paper:** [Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena](https://arxiv.org/abs/2306.05685) — Zheng et al., 2023
   - The foundational paper on LLM-as-judge methodology. Shows strong agreement between GPT-4 judge and human evaluators.
   - Read: Abstract + Section 3 (MT-Bench) + Section 4 (Agreement analysis)
2. **Guide:** [Promptfoo — Evaluate Coding Agents](https://www.promptfoo.dev/docs/guides/evaluate-coding-agents/)
3. **Guide:** [Promptfoo — Evaluate LangGraph](https://www.promptfoo.dev/docs/guides/evaluate-langgraph/)
4. **Tool (alternative):** [RAGAS](https://docs.ragas.io/) — open-source framework for evaluating RAG pipelines with metrics like faithfulness, answer relevancy, and context precision
5. **Tool (alternative):** [DeepEval](https://docs.confident-ai.com/) — open-source evaluation framework with 14+ LLM evaluation metrics

### Implementation

```python
# Simplified LLM-as-judge pattern
judge_prompt = """
You are evaluating an AI agent's response.

Question: {question}
Expected answer: {expected}
Agent's answer: {actual}
Agent's reasoning trace: {trace}

Rate the response on:
1. Correctness (1-5): Is the final answer correct?
2. Reasoning (1-5): Was the reasoning process sound?
3. Efficiency (1-5): Did the agent use tools appropriately without unnecessary steps?

Respond in JSON: {"correctness": N, "reasoning": N, "efficiency": N, "explanation": "..."}
"""
```

### Exercises

- [ ] Build an LLM-as-judge evaluator for your Module 2 research agent. Create a test set of 10 questions with expected answers. Run the agent on all 10, then have the judge score each.
- [ ] Compare judge scores with your own manual scores. Where do they disagree? Why?
- [ ] Experiment with different judge prompts. How sensitive are the scores to prompt wording?

---

## Unit 6.4: Agent Benchmarks

### Readings

1. **SWE-bench:** https://www.swebench.com/
   - Benchmark: can an agent solve real GitHub issues? Gold standard for coding agents.
   - [Leaderboard on Vals.ai](https://www.vals.ai/benchmarks/swebench)
2. **AgentBench:** [arxiv.org/abs/2308.03688](https://arxiv.org/abs/2308.03688) — Liu et al., 2023
   - 8 environments: OS, database, web browsing, lateral thinking, card games, etc.
   - Read: Abstract + Section 3 (Benchmark design)
3. **SWE-bench Live:** https://swe-bench-live.github.io/
   - Continuously updated variant with 50 new issues added monthly

### Exercises

- [ ] Read the SWE-bench leaderboard. What scores do the top agents achieve? What does the gap between top and bottom tell you?
- [ ] Read the AgentBench paper abstract and Section 3. What are the 8 environments, and why was each chosen?

---

## Unit 6.5: Building an Eval Harness

### Build Exercise

Create a reusable evaluation harness for any agent:

**Components:**
1. **Test cases** — a JSON/YAML file with: `question`, `expected_answer`, `required_tools` (optional), `difficulty`
2. **Runner** — executes the agent on each test case, captures: final answer, full trace, token usage, latency, cost
3. **Judge** — LLM-as-judge that scores each result on correctness, reasoning, efficiency
4. **Reporter** — generates a summary: pass rate, average scores, cost breakdown, failure analysis

**Requirements:**
- At least 20 test cases across 3 difficulty levels
- Scores saved to a JSON file for tracking over time
- Runs should be reproducible (log the model, temperature, and system prompt used)

**Stretch goals:**
- [ ] Add a regression test: compare scores between two versions of your agent
- [ ] Add red-team test cases: adversarial inputs that try to make the agent misbehave
- [ ] Integrate with a CI pipeline: run evals on every commit

---

## Unit 6.6: Agent Debugging & Testing

Evaluation tells you *how good* the agent is. Debugging tells you *why it fails*.

### Debugging Techniques

| Technique | What It Reveals | How To Do It |
|-----------|----------------|-------------|
| **Trace replay** | Exact sequence of thoughts, actions, and observations | Save full traces to JSON; replay to reproduce bugs |
| **Step-through** | Where the agent goes wrong in its reasoning | Print each step with a pause; inspect before continuing |
| **Tool unit testing** | Whether tools return correct results in isolation | Test each tool with known inputs/outputs — independent of the agent |
| **Prompt diffing** | Effect of prompt changes on behavior | Run same test set with 2 prompt versions, diff the traces |
| **Failure categorization** | Patterns in how the agent fails | Tag each eval failure: wrong tool, wrong args, hallucination, loop, refusal |

### Exercises

- [ ] **Unit test your tools:** For each tool in your Module 2 agent, write 3 test cases: (a) normal input, (b) edge case, (c) error input. Run them independently of the agent. Fix any bugs.
- [ ] **Categorize failures:** Run your eval harness from Unit 6.5. For every failure, categorize it: wrong tool selection, wrong arguments, hallucinated facts, infinite loop, or other. Which category is most common?
- [ ] **Trace replay:** Save a failing trace to JSON. Modify the system prompt. Replay the same conversation up to the failure point and check if the new prompt fixes it.

---

## Unit 6.7: Human-in-the-Loop

### Key Patterns

| Pattern | When to Use | Implementation |
|---------|------------|----------------|
| **Approval gate** | Before irreversible actions | Pause agent, show plan, wait for OK |
| **Correction** | Agent made a mistake | Human edits the agent's output, agent continues with corrected context |
| **Feedback loop** | Continuous improvement | Log human corrections, use for eval and fine-tuning data |
| **Escalation** | Agent is stuck or uncertain | Agent detects low confidence, hands off to human |
| **Collaborative drafting** | Agent drafts, human refines | Agent writes first draft, human edits, agent incorporates edits and continues |
| **Progressive autonomy** | Agent earns trust over time | Start with approval gates on every action; remove gates as confidence grows based on eval scores |

### Exercises

- [ ] **Approval gate:** Add to your LangGraph agent from Module 3: before any tool call that modifies state, pause and ask the user to approve.
- [ ] **Correction flow:** When the agent gives a wrong answer, allow the user to type a correction. The agent must acknowledge the correction, update its reasoning, and continue. Test: give the agent a question, let it answer wrong, correct it, and verify it adjusts.
- [ ] **Escalation:** Add confidence estimation: after each answer, the agent rates its confidence (1-5). If confidence < 3, it automatically escalates to the user with "I'm not sure about this — here's what I found, but please verify."

---

## Checkpoint: Module 6

- [ ] You have a working eval harness with 20+ test cases and LLM-as-judge scoring
- [ ] You can explain why agent eval is harder than LLM eval
- [ ] You know what SWE-bench and AgentBench measure
- [ ] You've unit tested your tools independently of the agent
- [ ] You've categorized your agent's failure modes
- [ ] You've implemented at least 2 human-in-the-loop patterns (approval gate + one other)
- [ ] You've run your eval harness and have a baseline score for your best agent

**Key insight to internalize:** Eval is not a one-time step. It's the feedback loop that drives all improvement. Build eval first, then iterate on the agent.
