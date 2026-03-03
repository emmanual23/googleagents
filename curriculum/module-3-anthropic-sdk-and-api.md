# Module 3: Anthropic SDK & API Deep Dive

**Time estimate:** 1 week
**Goal:** Master the Anthropic Claude platform — from core API features to the Agent SDK, MCP, and production-grade patterns. This deep dive prepares you to leverage Claude's unique capabilities in the frameworks and production modules that follow.

---

## Learning Objectives

By the end of this module you will:
- Use every major feature of the Claude Messages API (streaming, tool use, vision, PDFs, citations, extended thinking, prompt caching, batches)
- Build agents with the Claude Agent SDK (the same engine that powers Claude Code)
- Build and connect MCP servers for tool integration
- Optimize cost and latency with prompt caching, model routing, and batch processing
- Apply Claude-specific prompt engineering techniques

---

## Prerequisites

- Completed Modules 0-2 (you can make API calls, use function calling, and build tool-using agents)
- Anthropic API key with at least Tier 1 access
- Python 3.10+ with `anthropic` SDK installed: `pip install anthropic`

---

## Unit 3.1: The Claude Platform

### Readings

1. **Docs:** [Claude API Overview](https://docs.anthropic.com/en/api/getting-started) — authentication, versioning, base URL
2. **Docs:** [Models Overview](https://platform.claude.com/docs/en/about-claude/models/overview) — Opus 4.6, Sonnet 4.6, Haiku 4.5, context windows, pricing
3. **Docs:** [Messages API](https://docs.anthropic.com/en/api/messages) — the core endpoint
4. **Docs:** [Rate Limits & Usage Tiers](https://docs.anthropic.com/en/api/rate-limits) — tiers 1-4, RPM/token limits
5. **Free course:** [Building with the Claude API](https://anthropic.skilljar.com/claude-with-the-anthropic-api) — Anthropic Academy

### Key Concepts

| Concept | What It Means |
|---------|--------------|
| **Messages API** | All interactions go through `POST /v1/messages` — no separate completions, chat, or assistant endpoints |
| **System prompt as top-level param** | Unlike OpenAI, the system prompt is a dedicated parameter, not a message role |
| **Content blocks** | Responses contain typed content blocks: `text`, `tool_use`, `thinking`, `citations` |
| **Usage tiers** | Tier 1 ($5 deposit) through Tier 4 ($400+ deposit) — each unlocks higher rate limits |

### Model Selection Guide

| Model | Best For | Input / Output (per MTok) | Max Context |
|-------|---------|--------------------------|-------------|
| **Claude Opus 4.6** | Complex reasoning, coding, analysis | $5 / $25 | 1M tokens |
| **Claude Sonnet 4.6** | Best balance of quality and speed | $3 / $15 | 1M tokens |
| **Claude Haiku 4.5** | Fast tasks, classification, routing | $1 / $5 | 1M tokens |

### Exercises

- [ ] Read the Messages API docs. Send a multi-turn conversation with system prompt, user messages, and assistant prefill (ending with a partial assistant message to guide the response).
- [ ] Write a script that lists all available models using the [List Models API](https://docs.anthropic.com/en/api/models-list).
- [ ] Complete the "Building with the Claude API" Anthropic Academy course.

---

## Unit 3.2: Claude-Specific Prompt Engineering

Claude has distinct prompting patterns that differ from other providers.

### Readings

1. **Guide:** [Prompt Engineering Overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
2. **Guide:** [Claude 4 Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
3. **Tutorial:** [Interactive Prompt Engineering Tutorial](https://github.com/anthropics/prompt-eng-interactive-tutorial) — 9-chapter Jupyter notebook series with exercises
4. **Course:** [Anthropic Courses on GitHub](https://github.com/anthropics/courses) — includes "Real World Prompting" and "Prompt Evaluations"
5. **Coursera:** [Mastering Claude AI: Prompting, APIs, RAG, and MCP](https://www.coursera.org/specializations/mastering-claude-ai-prompting-apis-rag-and-mcp) — 7-module Anthropic specialization (free to audit)

### Claude-Specific Techniques

| Technique | How It Works | Example |
|-----------|-------------|---------|
| **XML tags for structure** | Claude responds well to `<instructions>`, `<context>`, `<example>` tags | `<context>{{document}}</context>` |
| **Assistant prefill** | End with a partial assistant message to control output format | `{"role": "assistant", "content": "{"result":"}` |
| **Thinking out loud** | Ask Claude to reason in `<thinking>` tags before answering | "Think step by step in <thinking> tags, then answer." |
| **Role and personality** | Claude's system prompt supports rich persona definition | "You are a senior Python developer who writes concise, idiomatic code." |
| **Structured output** | Use `output_config` with JSON schema (beta) or prompt-based JSON enforcement | `output_config: {schema: {...}}` |

### Exercises

- [ ] Work through at least chapters 1-7 of the interactive prompt engineering tutorial.
- [ ] Practice assistant prefill: get Claude to output valid JSON without any schema enforcement by starting the assistant message with `{`.
- [ ] Compare: send the same complex prompt to Claude with and without XML-tagged structure. Evaluate output quality.
- [ ] Read the Claude 4 Best Practices guide. Implement 3 techniques you haven't tried before.

---

## Unit 3.3: Tool Use — Anthropic's Implementation

You learned generic function calling in Module 2. This unit covers Anthropic's specific tool use features.

### Readings

1. **Docs:** [Tool Use Overview](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview)
2. **Blog:** [Writing Tools for Agents](https://www.anthropic.com/engineering/writing-tools-for-agents) — Anthropic Engineering
3. **Blog:** [Advanced Tool Use](https://www.anthropic.com/engineering/advanced-tool-use) — Anthropic Engineering

### Anthropic-Specific Tool Features

| Feature | What It Does | When to Use |
|---------|-------------|-------------|
| **Client tools** | You define and execute tools locally | Standard function calling |
| **Server tools** | Anthropic executes tools on their infrastructure | Web search, code execution, web fetch |
| **`strict: true`** | Guarantees tool call arguments conform to JSON schema | When you need guaranteed schema compliance |
| **`tool_choice: auto/any/tool/none`** | Fine-grained control over when Claude calls tools | `any` = must call a tool; `tool` = must call a specific tool |
| **Parallel tool calls** | Claude can request multiple tool calls in one response | Independent operations that can run concurrently |
| **`pause_turn` stop reason** | Claude pauses mid-turn for tool execution, then continues | Multi-step tool chains within a single turn |
| **Tool Search Tool** | Dynamic tool discovery from large catalogs | When you have 100+ tools — avoids context window bloat |

### Server-Side Tools (Anthropic-Hosted)

| Tool | What It Does | Pricing |
|------|-------------|---------|
| **Web Search** (`web_search_20260209`) | Real-time web search | Per-search pricing |
| **Web Fetch** (`web_fetch_20260209`) | Retrieve full content from URLs | Per-fetch pricing |
| **Code Execution** (`code_execution_20250522`) | Sandboxed Python execution | Free when combined with web search/fetch |

### Exercises

- [ ] Implement all 4 tool choice modes (`auto`, `any`, `tool`, `none`) and observe Claude's behavior with each.
- [ ] Build an agent that uses the server-side **web search tool** alongside your own client-side tools. Have Claude decide when to search the web vs use your tools.
- [ ] Test `strict: true` vs non-strict tool definitions. Create a tool with a complex nested schema and verify strict mode guarantees valid arguments.
- [ ] Implement parallel tool calls: give Claude 3 independent tasks that each need a tool call. Verify it requests all 3 in one response.

---

## Unit 3.4: Extended Thinking

Extended thinking is Claude's chain-of-thought reasoning, visible in the API response. This is Anthropic's approach to reasoning models — a parameter on any model, not a separate model family.

### Readings

1. **Docs:** [Extended Thinking](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking)
2. **Blog:** [Building Effective Agents — Planning Patterns](https://www.anthropic.com/research/building-effective-agents) — Anthropic

### Key Concepts

- **Budget tokens:** `thinking: {type: "enabled", budget_tokens: N}` — Claude uses up to N tokens for reasoning
- **Adaptive thinking (Opus 4.6):** `thinking: {type: "adaptive"}` — Claude decides how much to think based on problem complexity
- **Summarized thinking:** Claude 4 models return a summary of their reasoning (you're billed for the full thinking, but see a summary)
- **Interleaved thinking:** Claude can reason between tool calls — think, call tool, think again, call another tool
- **Incompatibilities:** Cannot use with `temperature`/`top_k` or forced tool use (`tool_choice: tool`)

### Exercises

- [ ] Send the same hard math problem with and without extended thinking. Compare accuracy and latency.
- [ ] Experiment with budget sizes: try 1000, 5000, and 20000 budget tokens on the same problem. How does reasoning quality change?
- [ ] Build an agent with interleaved thinking: enable extended thinking alongside tool use. Observe the thinking blocks between tool calls in the response.
- [ ] Compare: extended thinking vs "think step by step" in the prompt. When does each approach work better?

---

## Unit 3.5: Vision, PDFs, and Citations

Claude's multimodal capabilities go beyond basic image input.

### Readings

1. **Docs:** [Vision](https://docs.anthropic.com/en/docs/build-with-claude/vision) — image input
2. **Docs:** [PDF Support](https://docs.anthropic.com/en/docs/build-with-claude/pdf-support) — native PDF processing
3. **Docs:** [Citations](https://docs.anthropic.com/en/docs/build-with-claude/citations) — source-grounded responses
4. **Docs:** [Files API](https://docs.anthropic.com/en/docs/build-with-claude/files) — upload and reference files (beta)

### Feature Details

| Feature | What It Does | Key Details |
|---------|-------------|-------------|
| **Vision** | Process images in messages | JPEG/PNG/GIF/WebP; up to 100 images; base64 or URL input; cost: `(w*h)/750` tokens |
| **PDF support** | Native PDF understanding | Up to 100 pages, 32MB; visual + text extraction; chart/diagram analysis |
| **Citations** | Source-grounded responses | Enable on document blocks; response includes citation references to exact source locations; `cited_text` is free (not billed) |
| **Files API** | Upload once, reference by ID | Beta; 500MB/file, 100GB/org; avoids re-sending large files |

### Citations — Unique to Anthropic

Citations are a differentiated feature not available from other providers. When enabled, Claude's responses include inline references pointing to exact locations in your source documents:

```python
# Enable citations on a document block
message = client.messages.create(
    model="claude-sonnet-4-6-20250929",
    messages=[{
        "role": "user",
        "content": [
            {"type": "document", "source": {...}, "citations": {"enabled": True}},
            {"type": "text", "text": "What are the key findings?"}
        ]
    }]
)
# Response contains text blocks interleaved with citation blocks
# pointing to page numbers (PDF), character indices (text), or block indices (custom)
```

### Exercises

- [ ] Send a PDF document to Claude with citations enabled. Verify that the response includes citation references pointing to specific pages.
- [ ] Build a document Q&A tool: given a PDF, answer questions with inline citations. Display citations as footnotes.
- [ ] Upload a file via the Files API. Reference it by `file_id` in multiple messages without re-uploading. Compare latency vs re-sending the file each time.
- [ ] Process a mixed document: a PDF containing both text and charts. Ask questions that require understanding the chart visuals.

---

## Unit 3.6: Prompt Caching and Cost Optimization

### Readings

1. **Docs:** [Prompt Caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)
2. **Docs:** [Batch Processing](https://platform.claude.com/docs/en/build-with-claude/batch-processing)
3. **Docs:** [Token Counting API](https://docs.anthropic.com/en/api/messages-count-tokens)

### Prompt Caching

Prompt caching dramatically reduces cost and latency for repeated system prompts, few-shot examples, and large context.

| Aspect | Details |
|--------|---------|
| **Cache write** | 25% surcharge on first use |
| **Cache read** | 10% of base input cost (90% savings) |
| **Default TTL** | 5 minutes (resets on each cache hit) |
| **Extended TTL** | 1 hour at 2x cache write price |
| **Breakpoints** | Up to 4 explicit `cache_control` breakpoints |
| **Minimum tokens** | 1,024 (Haiku) to 4,096 (Opus) |

**Where to cache:** System prompts, tool definitions, few-shot examples, large documents, and any content that stays the same across requests.

### Batch Processing

The Message Batches API provides 50% cost reduction for non-time-sensitive workloads.

| Aspect | Details |
|--------|---------|
| **Discount** | 50% on all usage |
| **Batch size** | Up to 100,000 requests or 256MB |
| **Completion time** | Most within 1 hour, 24-hour expiration |
| **Feature support** | ALL Messages API features (including tools, vision, thinking) |

### Exercises

- [ ] Implement prompt caching: build a chatbot with a large system prompt (2000+ tokens). Send 10 messages and observe `cache_creation_input_tokens` vs `cache_read_input_tokens` in the usage metadata. Calculate savings.
- [ ] Use the Token Counting API (`/v1/messages/count_tokens`) to count tokens before sending. Build a budget tracker that warns when approaching context limits.
- [ ] Build a batch evaluation pipeline: create 50 test cases, submit as a batch, and compare cost to sending them individually.
- [ ] Combine optimizations: implement prompt caching + model routing (Haiku for classification, Sonnet for generation) on the same task. Measure total cost reduction.

---

## Unit 3.7: Streaming

### Readings

1. **Docs:** [Streaming Messages](https://docs.anthropic.com/en/api/messages-streaming) — SSE event reference

### Event Types

Claude's streaming uses Server-Sent Events with typed events:

| Event | When It Fires |
|-------|--------------|
| `message_start` | Beginning of response, includes model/usage metadata |
| `content_block_start` | New content block (text, tool_use, thinking) |
| `content_block_delta` | Incremental content: `text_delta`, `input_json_delta`, `thinking_delta`, `citations_delta` |
| `content_block_stop` | Content block complete |
| `message_delta` | Final metadata: stop reason, output token count |
| `message_stop` | Response complete |

### Exercises

- [ ] Implement streaming for a multi-turn conversation. Print tokens as they arrive.
- [ ] Stream a tool-using agent: detect `tool_use` content blocks in the stream, execute tools, and send results back. Handle the full agentic loop with streaming.
- [ ] Stream with extended thinking enabled. Observe `thinking_delta` events. Display the thinking process in a different color/style from the final response.

---

## Unit 3.8: The Claude Agent SDK

The Agent SDK is the same engine that powers Claude Code — a production-grade framework for building agentic applications.

### Readings

1. **Docs:** [Agent SDK Overview](https://platform.claude.com/docs/en/agent-sdk/overview)
2. **Blog:** [Building Agents with the Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk)
3. **GitHub:** [Claude Agent SDK — Python](https://github.com/anthropics/claude-agent-sdk-python)
4. **GitHub:** [Agent SDK Demos](https://github.com/anthropics/claude-agent-sdk-demos)
5. **Blog:** [Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)

### Key Concepts

| Concept | What It Does |
|---------|-------------|
| **Agentic loop** | Built-in tool execution loop — Claude calls tools, SDK executes, sends results back automatically |
| **Built-in tools** | Read, Write, Edit, Bash, Glob, Grep, WebSearch, WebFetch — the same tools Claude Code uses |
| **MCP integration** | Native support for connecting MCP servers as tool sources |
| **Hooks** | Lifecycle callbacks: before/after tool execution, on error, on message |
| **Subagent delegation** | Spawn child agents for parallel subtasks |
| **Session management** | Persistent sessions with context carry-over |

### Exercises

- [ ] Install the Agent SDK and build a simple agent that can read files and search the web. Compare the code to your raw API agent from Module 2.
- [ ] Clone the [claude-agent-sdk-demos](https://github.com/anthropics/claude-agent-sdk-demos) repo. Run at least 2 demo applications and study their architecture.
- [ ] Build a code review agent: given a file path, the agent reads the code, analyzes it, and writes suggestions to a review file. Use the built-in Read, Write, and Bash tools.
- [ ] Implement hooks: add a `before_tool_execution` hook that logs every tool call with a timestamp.

---

## Unit 3.9: Model Context Protocol (MCP)

MCP is an open standard created by Anthropic for connecting AI agents to external tools and data sources. You'll encounter MCP again in Module 6 (Multi-Agent Systems), but this unit teaches you to build with it.

### Readings

1. **Docs:** [MCP Documentation](https://modelcontextprotocol.io/)
2. **Spec:** [MCP Specification](https://modelcontextprotocol.io/specification/2025-11-25)
3. **Free courses:** [MCP Introduction](https://anthropic.skilljar.com/) + [Advanced MCP](https://anthropic.skilljar.com/) — Anthropic Academy
4. **GitHub:** [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)
5. **GitHub:** [MCP Reference Servers](https://github.com/modelcontextprotocol/servers) — collection of reference implementations
6. **DeepLearning.AI:** [Building Toward Computer Use with Anthropic](https://learn.deeplearning.ai/courses/building-toward-computer-use-with-anthropic) — includes MCP and tool use (taught by Anthropic's Head of Curriculum)

### MCP Architecture

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  MCP Client  │────→│  MCP Server  │────→│  External    │
│  (your app)  │←────│  (tool host) │←────│  Service     │
└──────────────┘     └──────────────┘     └──────────────┘

Three primitives:
- Tools: functions the server exposes (e.g., search_database, read_file)
- Resources: data the server provides (e.g., file contents, database rows)
- Prompts: reusable prompt templates the server offers

Two transports:
- stdio: local process communication (for CLI tools)
- Streamable HTTP: remote server communication (replacing SSE transport)
```

### Exercises

- [ ] Complete the MCP Introduction course on Anthropic Academy.
- [ ] Build an MCP server in Python that exposes 3 tools: a file reader, a calculator, and a note-taking tool. Use the [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk).
- [ ] Connect your MCP server to the Claude Agent SDK. Verify the agent can discover and call your tools.
- [ ] Build a Resource: add a resource to your MCP server that exposes a directory listing. Connect it and have Claude use the resource.
- [ ] Browse the [MCP reference servers](https://github.com/modelcontextprotocol/servers). Install and test 2 community servers (e.g., filesystem, GitHub, database).

---

## Unit 3.10: Agent Architecture Patterns

Anthropic has published definitive guides on building agents. This unit covers the patterns you'll use throughout the rest of the curriculum.

### Readings

1. **Blog:** [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) — the foundational reference for agent architecture
2. **Blog:** [How We Built Our Multi-Agent Research System](https://www.anthropic.com/engineering/multi-agent-research-system)
3. **Blog:** [Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
4. **Blog:** [Equipping Agents for the Real World with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
5. **Cookbook:** [Agent Patterns](https://github.com/anthropics/anthropic-cookbook/tree/main/patterns/agents) — runnable implementations of each pattern
6. **DeepLearning.AI:** [Agent Skills with Anthropic](https://learn.deeplearning.ai/courses/agent-skills-with-anthropic) — taught by Anthropic engineer

### The Six Composable Patterns

From "Building Effective Agents":

| Pattern | Description | When to Use |
|---------|-------------|-------------|
| **Augmented LLM** | LLM + retrieval + tools (no loop) | Single-step tasks with context |
| **Prompt Chaining** | Output of one LLM call is input to next | Multi-step workflows with clear stages |
| **Routing** | Classify input, route to specialized handler | Different question types need different approaches |
| **Parallelization** | Run multiple LLM calls concurrently | Independent subtasks, voting/ensemble |
| **Orchestrator-Workers** | Central LLM plans and delegates to specialized workers | Complex tasks needing dynamic decomposition |
| **Evaluator-Optimizer** | One LLM generates, another evaluates and provides feedback | Iterative refinement to a quality threshold |

### Agent Skills (Beta)

Agent Skills are modular packages of instructions, scripts, and resources that transform a general agent into a domain specialist. They provide a standardized way to equip agents with specialized knowledge and capabilities.

### Exercises

- [ ] Read "Building Effective Agents" end-to-end. For each of the 6 patterns, write a 1-sentence description of when you would use it in your own projects.
- [ ] Clone the [agent patterns cookbook](https://github.com/anthropics/anthropic-cookbook/tree/main/patterns/agents). Run the prompt chaining and routing examples. Modify one to work with your own use case.
- [ ] Implement the Evaluator-Optimizer pattern: have one Claude call generate a response, and another evaluate it on quality criteria. Loop until the evaluator is satisfied (max 3 rounds).
- [ ] Read the context engineering blog post. Redesign the context budget from your Module 2 agent using the principles described.

---

## Additional Resources

### Anthropic Academy (Free Courses with Certificates)

- [Anthropic Academy](https://anthropic.skilljar.com/) — 13 free self-paced courses
- Recommended: Claude 101, Building with the Claude API, MCP Introduction, Advanced MCP, Claude Code in Action

### Key Engineering Blog Posts

- [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) — foundational agent architecture reference
- [Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Advanced Tool Use](https://www.anthropic.com/engineering/advanced-tool-use)
- [Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)

### DeepLearning.AI Courses (Anthropic Partnership)

- [Building Toward Computer Use with Anthropic](https://learn.deeplearning.ai/courses/building-toward-computer-use-with-anthropic) — taught by Anthropic's Head of Curriculum
- [Agent Skills with Anthropic](https://learn.deeplearning.ai/courses/agent-skills-with-anthropic)
- [Claude Code: A Highly Agentic Coding Assistant](https://learn.deeplearning.ai/courses/claude-code-a-highly-agentic-coding-assistant)

### Community Resources

- [Nader Dabit — Complete Guide to Building Agents with Claude Agent SDK](https://nader.substack.com/p/the-complete-guide-to-building-agents)
- [Lee Hanchung — Claude Agent Skills: A First Principles Deep Dive](https://leehanchung.github.io/blogs/2025/10/26/claude-skills-deep-dive/)
- [Simon Willison — Code with Claude Live Blog](https://simonwillison.net/2025/May/22/code-with-claude-live-blog/)

### Official Repos

| Repository | URL |
|-----------|-----|
| Python SDK | https://github.com/anthropics/anthropic-sdk-python |
| Agent SDK (Python) | https://github.com/anthropics/claude-agent-sdk-python |
| Agent SDK Demos | https://github.com/anthropics/claude-agent-sdk-demos |
| Cookbooks | https://github.com/anthropics/claude-cookbooks |
| Quickstarts | https://github.com/anthropics/claude-quickstarts |
| Courses | https://github.com/anthropics/courses |
| Prompt Engineering Tutorial | https://github.com/anthropics/prompt-eng-interactive-tutorial |
| MCP Python SDK | https://github.com/modelcontextprotocol/python-sdk |
| MCP Reference Servers | https://github.com/modelcontextprotocol/servers |

---

## Checkpoint: Module 3

**Build an Anthropic-native research agent that showcases platform-specific features:**

1. Uses the **Claude Agent SDK** (not raw API) with at least 3 tools (1 MCP server + 2 client tools)
2. **Extended thinking** enabled for complex reasoning steps
3. **Citations** on document-based answers (at least 1 PDF or text document as source)
4. **Prompt caching** on the system prompt and tool definitions
5. **Streaming** output with thinking and tool call events displayed to the user

**Test with:**
- A question that requires web search + document analysis (should use extended thinking)
- A question about a provided PDF (should return citations pointing to specific pages)
- Run the same 10 queries twice — verify prompt cache hits on the second run and measure cost savings

- [ ] Agent uses Claude Agent SDK with MCP integration
- [ ] Extended thinking produces visible reasoning
- [ ] Citations reference specific source locations
- [ ] Prompt caching reduces cost on repeated queries
- [ ] Streaming displays thinking, tool calls, and response incrementally
- [ ] You've completed at least 2 Anthropic Academy courses
- [ ] You've read "Building Effective Agents" and can describe all 6 patterns

**Self-test:** Can you explain to someone why they'd use the Claude Agent SDK instead of raw API calls? Can you list 3 features unique to Claude that other providers don't offer?

---

## Frontier & 2026-27 Outlook

Anthropic's platform is evolving rapidly along multiple axes. The **Agent SDK** represents a strategic bet that the same agentic engine powering Claude Code should be available to all developers — it provides built-in tools, MCP integration, session management, and subagent delegation out of the box. The **Agent Skills** framework (beta, December 2025) introduces a standard for packaging domain knowledge into composable modules, adopted by Microsoft, OpenAI, Atlassian, and Figma as an open standard. Server-side tools (web search, code execution, web fetch) are expanding Claude from a text-in/text-out API to a platform that can autonomously gather information and execute code.

On the API side, several features differentiate Claude from competitors: **citations** provide verifiable source grounding with no equivalent in other providers; **extended thinking with summarization** gives developers reasoning transparency while billing efficiency; **interleaved thinking** enables reasoning between tool calls; and **prompt caching with explicit breakpoints** offers fine-grained control over what gets cached. The MCP ecosystem continues to grow — over 10,000 community servers, donated to the Linux Foundation, and now integrated at the API level via the MCP Connector (server-side MCP server connections). Claude Code itself (reportedly approaching ~$2B ARR) demonstrates what's possible when these platform features are composed into a coherent agentic product.

### Watch List
- **Agent SDK evolution** — expect more built-in tools, improved session management, and tighter MCP integration
- **Server-side tool expansion** — Anthropic is adding more hosted tools (memory, code execution); watch for database, email, and calendar tools
- **MCP Connector maturation** — server-side MCP integration eliminates the need for local MCP server processes
- **Agent Skills ecosystem** — as the standard gains adoption, expect a marketplace of pre-built skills for common domains
