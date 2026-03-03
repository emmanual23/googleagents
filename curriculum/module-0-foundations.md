# Module 0: Foundations — Python, LLMs, and Prompts

**Time estimate:** 1-2 weeks
**Goal:** Understand what LLMs are, call three different provider APIs, and write effective prompts.

---

## Learning Objectives

By the end of this module you will:
- Explain how transformer-based LLMs work at a conceptual level
- Set up a Python development environment
- Make API calls to OpenAI, Anthropic, and Google Gemini
- Apply core prompt engineering techniques (few-shot, chain-of-thought, role prompts)
- Understand tokens, context windows, temperature, and other key parameters

---

## Unit 0.1: How LLMs Work

### Readings

1. **Watch:** [Intro to Large Language Models](https://www.youtube.com/watch?v=zjkBMFhNj_g) — Andrej Karpathy (1 hr)
   - Covers training, inference, scaling laws, and emerging capabilities
2. **Read:** [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) — Jay Alammar
   - Visual walkthrough of self-attention, encoder/decoder architecture
3. **Read:** [The Illustrated GPT-2](https://jalammar.github.io/illustrated-gpt2/) — Jay Alammar
   - How decoder-only models (the architecture behind GPT, Claude, Gemini) generate text
4. **Optional paper:** [Attention Is All You Need](https://arxiv.org/abs/1706.03762) — Vaswani et al., 2017
   - The original transformer paper. Read for depth, not required to proceed.

### Exercises

- [ ] After watching Karpathy's talk, write a 1-paragraph summary of how LLM training works (pre-training vs fine-tuning)
- [ ] After reading Alammar's posts, draw (on paper or whiteboard) a diagram of how self-attention works in your own words

---

## Unit 0.2: Python Setup

> Skip this unit if you're already comfortable with Python 3.10+, virtual environments, and pip.

### Readings

1. **Course:** [AI Python for Beginners](https://learn.deeplearning.ai/courses/ai-python-for-beginners/information) — Andrew Ng / DeepLearning.AI (free)
   - Python fundamentals taught in the context of AI applications
2. **Alternative:** [CS50P](https://cs50.harvard.edu/python/) — Harvard (free)
   - More rigorous, covers functions, file I/O, libraries, testing

### Setup Steps

- [ ] Install Python 3.10+ from python.org
- [ ] Create a project folder and virtual environment: `python -m venv .venv`
- [ ] Install core packages: `pip install openai anthropic google-genai httpx python-dotenv`
- [ ] Create a `.env` file with your API keys:
  ```
  OPENAI_API_KEY=sk-...
  ANTHROPIC_API_KEY=sk-ant-...
  GOOGLE_API_KEY=AI...
  ```

---

## Unit 0.3: Your First API Calls

### Readings

1. **OpenAI Quickstart:** https://platform.openai.com/docs/quickstart
2. **Anthropic Quickstart:** https://docs.anthropic.com/en/docs/quickstart-guide
3. **Gemini Quickstart:** https://ai.google.dev/gemini-api/docs/quickstart

### Exercises

- [ ] Write a Python script that sends the same prompt to all three providers and prints their responses side by side
- [ ] Experiment with `temperature` (0.0 vs 1.0) — send the same prompt 5 times at each setting and observe variance
- [ ] Measure token counts: send a 500-word prompt and note input/output token usage from each provider's response metadata
- [ ] Calculate cost: using each provider's pricing page, compute the cost of 1000 calls at your prompt size

### Key Concepts to Understand

- **Tokens** — how text is split, why it matters for cost and context limits
- **Context window** — maximum input + output tokens (varies by model)
- **Temperature** — controls randomness (0 = deterministic, 1 = creative)
- **System vs user messages** — how the message array structures a conversation
- **Streaming** — receiving tokens as they're generated vs waiting for full response

---

## Unit 0.4: Prompt Engineering

### Readings

1. **Guide:** [Anthropic Prompt Engineering Overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
2. **Interactive tutorial:** [Anthropic Prompt Engineering Tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial) — 9-chapter Jupyter notebook series
3. **Guide:** [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
4. **Reference:** [DAIR.AI Prompt Engineering Guide](https://www.promptingguide.ai/) — model-agnostic, community-maintained

### Core Techniques to Practice

| Technique | What It Does | When to Use |
|-----------|-------------|-------------|
| Zero-shot | Direct instruction, no examples | Simple, well-defined tasks |
| Few-shot | Include input/output examples | Pattern matching, formatting |
| Chain-of-thought | "Think step by step" | Reasoning, math, logic |
| Role/persona | "You are a..." | Domain-specific expertise |
| Structured output | "Respond in JSON with keys..." | When you need parseable output |

### Exercises

- [ ] Work through at least chapters 1-5 of the Anthropic interactive tutorial
- [ ] Rewrite this bad prompt into a good one: `"Tell me about dogs"` → craft a prompt that returns a structured comparison of 3 dog breeds in JSON
- [ ] Chain-of-thought exercise: get an LLM to solve `(17 * 23) + (45 / 9) - 12` correctly by asking it to show its work
- [ ] Few-shot exercise: provide 3 examples of text-to-SQL translation, then give it a new natural language query

---

## Unit 0.5: Embeddings and Vector Search (Intro)

### Readings

1. **Guide:** [OpenAI Embeddings Guide](https://platform.openai.com/docs/guides/embeddings)
2. **Explainer:** [What are Vector Embeddings](https://www.pinecone.io/learn/vector-embeddings/) — Pinecone
3. **Deep dive (optional):** [What Are Embeddings?](https://vickiboykis.com/what_are_embeddings/) — Vicki Boykis (70+ pages, comprehensive)

### Exercises

- [ ] Generate embeddings for 10 sentences using OpenAI's API. Compute cosine similarity between pairs. Verify that semantically similar sentences score higher.
- [ ] Install `chromadb`, create a local collection, insert 20 text chunks, and query with natural language to see what comes back.

---

## Checkpoint: Module 0

**Build this to confirm you're ready for Module 1:**

Write a CLI chatbot that:
1. Lets the user pick a provider (OpenAI / Anthropic / Gemini) at startup
2. Maintains conversation history (multi-turn)
3. Supports a `/system` command to change the system prompt mid-conversation
4. Prints token usage after each response
5. Uses at least one prompt engineering technique (e.g., a system prompt with role + structured output instructions)

This should be ~100-150 lines of Python. No frameworks needed — just the raw SDKs.
