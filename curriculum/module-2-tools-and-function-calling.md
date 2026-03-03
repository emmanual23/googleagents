# Module 2: Tools & Function Calling

**Time estimate:** 1 week
**Goal:** Master how LLMs call external tools via structured function calling, build tools that agents can use reliably.

---

## Learning Objectives

By the end of this module you will:
- Use native function calling / tool use APIs from OpenAI, Anthropic, and Google
- Understand structured outputs and JSON schema enforcement
- Design tools that are easy for LLMs to use correctly
- Build a tool-using agent with function calling (replacing the regex parsing from Module 1)
- Send images to LLMs for multimodal agent capabilities
- Understand the Toolformer paper and its implications

---

## Prerequisites for This Module

This module uses **JSON Schema** and **Pydantic** — here's what you need:

- **JSON Schema**: a standard for describing the structure of JSON data. Function calling APIs require tool parameters defined as JSON schemas. Learn the basics (5 min): [json-schema.org/learn](https://json-schema.org/learn/getting-started-step-by-step)
- **Pydantic**: Python library for data validation using type hints. Used for structured outputs. Quick intro (10 min): [Pydantic Getting Started](https://docs.pydantic.dev/latest/#getting-started)
- **`httpx` or `requests`**: for calling external APIs from tools. Install: `pip install httpx`

---

## Unit 2.1: Native Function Calling

In Module 1, you parsed tool calls from free-text LLM output with regex. Every major provider now has a **native function calling** API that returns structured tool calls as part of the response object — more reliable and no parsing needed.

### Readings

1. **OpenAI — Function Calling:** https://platform.openai.com/docs/guides/function-calling
2. **Anthropic — Tool Use:** https://platform.claude.com/docs/en/agents-and-tools/tool-use/implement-tool-use
3. **Google Gemini — Function Calling:** https://ai.google.dev/gemini-api/docs/function-calling
4. **Cross-provider overview:** [Prompt Engineering Guide — Function Calling](https://www.promptingguide.ai/agents/function-calling)

### Key Concepts

- **Tool definition** — JSON schema describing the function name, description, and parameters
- **Tool choice** — `auto` (LLM decides), `required` (must call a tool), `none` (no tools)
- **Parallel tool calls** — some providers support calling multiple tools in one turn
- **Tool result message** — after executing the tool, you send the result back to the LLM as a tool result

### Exercises

- [ ] Rewrite your Module 1 ReAct agent to use native function calling instead of regex parsing. Compare reliability.
- [ ] Define and register 3 tools with each provider's API. Verify the LLM calls them correctly.
- [ ] Test edge cases: what happens when the LLM is given a question that doesn't need any tools? What if it hallucinates a tool that doesn't exist?

---

## Unit 2.2: Structured Outputs

### Readings

1. **OpenAI — Structured Outputs:** https://platform.openai.com/docs/guides/structured-outputs
   - Guarantees valid JSON conforming to a schema via `strict: true`
2. **Cookbook:** [OpenAI Structured Outputs Intro](https://developers.openai.com/cookbook/examples/structured_outputs_intro/)
3. **Cross-provider guide:** [The Guide to Structured Outputs and Function Calling with LLMs](https://agenta.ai/blog/the-guide-to-structured-outputs-and-function-calling-with-llms) — Agenta

### Exercises

- [ ] Define a Pydantic model for a "research report" with fields: title, summary, sources (list), confidence_score (float 0-1). Get each provider to return data conforming to this schema.
- [ ] Build a data extraction tool: given a block of unstructured text (e.g., a news article), extract structured entities (people, organizations, dates, locations) into JSON.

---

## Unit 2.3: Designing Good Tools

Not all tools are created equal. A well-designed tool has clear descriptions, typed parameters, and handles errors gracefully. A bad tool confuses the LLM and leads to failures.

### Readings

1. **Blog:** [Writing Tools for Agents](https://www.anthropic.com/engineering/writing-tools-for-agents) — Anthropic
2. **Blog:** [Advanced Tool Use](https://www.anthropic.com/engineering/advanced-tool-use) — Anthropic

### Tool Design Principles

| Principle | Good | Bad |
|-----------|------|-----|
| Clear name | `search_web(query: str)` | `do_thing(x: str)` |
| Descriptive | "Search the web and return top 3 results" | "Searches" |
| Typed params | `temperature: float, unit: Literal["C","F"]` | `input: str` |
| Error handling | Returns `{"error": "City not found"}` | Raises unhandled exception |
| Scoped | Does one thing well | Does 5 things depending on a mode param |
| Idempotent | Safe to retry | Has side effects on retry |

### Exercises

- [ ] Audit the tools from your Module 1 agent against these principles. Rewrite them.
- [ ] Build 3 new tools: (a) a weather API tool, (b) a file reader tool, (c) a URL fetcher tool. Give each clear schemas and error handling.
- [ ] Deliberately write a bad tool (vague name, no description, untyped params). Give it to the LLM and observe how it struggles. Then fix it and compare.

---

## Unit 2.4: The Toolformer Paper

### Reading

- **Toolformer: Language Models Can Teach Themselves to Use Tools**
  - Schick et al., 2023 — [arxiv.org/abs/2302.04761](https://arxiv.org/abs/2302.04761)
  - Key insight: LLMs can be trained (self-supervised) to decide **when** and **how** to call tools
  - Read: Abstract + Section 1 (Intro) + Section 2 (Approach)

### Reflection

- [ ] How does Toolformer's approach differ from the function calling APIs you've been using? (Hint: training-time vs inference-time tool integration)
- [ ] What are the tradeoffs of each approach?

---

## Unit 2.5: Multimodal Tool Use (Vision)

Modern agents can process images alongside text. All three major providers support vision input.

### Readings

1. **Docs:** [OpenAI Vision Guide](https://platform.openai.com/docs/guides/vision)
2. **Docs:** [Anthropic Vision](https://docs.anthropic.com/en/docs/build-with-claude/vision)
3. **Docs:** [Google Gemini — Image Understanding](https://ai.google.dev/gemini-api/docs/vision)

### Exercises

- [ ] Send an image (a chart or screenshot) to each provider's API alongside a text question. Compare the quality of their visual understanding.
- [ ] Build a multimodal tool: `analyze_image(image_path: str, question: str) -> str` that sends an image to an LLM and returns the answer. Register it as a tool in your agent.
- [ ] Test: give your agent a question that requires looking at an image (e.g., "What does this chart show about Q3 revenue?"). Does it correctly decide to use the image analysis tool?

---

## Unit 2.6: Build a Tool-Using Agent

### Build Exercise

Build a research assistant agent that can answer complex questions by combining multiple tools:

**Setup note:** For web search, use [DuckDuckGo Search](https://pypi.org/project/duckduckgo-search/) (`pip install duckduckgo-search`) — free, no API key needed.

**Tools to implement:**
1. `web_search(query: str) -> list[dict]` — search the web (use DuckDuckGo or SerpAPI free tier)
2. `read_webpage(url: str) -> str` — fetch and extract text from a URL
3. `calculator(expression: str) -> str` — evaluate math
4. `note_taking(action: str, content: str) -> str` — save/retrieve notes (in-memory dict)

**Requirements:**
- Use native function calling (not regex parsing)
- Support multi-turn: the agent can call multiple tools sequentially to answer one question
- Add a simple loop budget (max 8 tool calls per question)
- Test with questions that require chaining: "Find the GDP of Japan and South Korea, calculate the ratio, and explain which is larger and by how much"

**Test your agent with:**
- A question needing 1 tool
- A question needing 3+ tools in sequence
- A question where no tools are needed
- An ambiguous question (does the agent ask for clarification or guess?)

---

## Checkpoint: Module 2

- [ ] You can define and use native function calling with at least 2 of the 3 major providers
- [ ] You understand the difference between regex-parsed tool calls (Module 1) and native function calling
- [ ] You've built a 4-tool research agent using function calling
- [ ] You can explain what makes a tool easy vs hard for an LLM to use
- [ ] You've read the Toolformer paper and can explain its approach
