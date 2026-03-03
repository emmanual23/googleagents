# Agentic AI Curriculum

A structured, vendor-neutral learning path from zero to state-of-the-art in agentic AI.

## How This Curriculum Works

- **12 modules**, each in its own file — work through them in order
- Each module has: learning objectives, readings, hands-on exercises, and checkpoints
- Estimated total: **12-16 weeks** at ~10 hrs/week (adjust to your pace)
- All readings and resources are free unless marked otherwise

## Prerequisites

- Basic programming comfort (any language — Python taught in Module 0)
- A computer with Python 3.10+ and internet access
- API keys: OpenAI, Anthropic, and Google (free tiers are sufficient)

## Module Map

| Module | Title | Est. Time | What You'll Build |
|--------|-------|-----------|-------------------|
| [0](module-0-foundations.md) | Foundations: Python, LLMs, Prompts | 1-2 weeks | Your first API calls to 3 providers |
| [1](module-1-agent-fundamentals.md) | Agent Fundamentals | 1 week | ReAct agent from scratch in raw Python |
| [2](module-2-tools-and-function-calling.md) | Tools, Function Calling & Vision | 1 week | Tool-using multimodal agent |
| [3](module-3-anthropic-sdk-and-api.md) | Anthropic SDK & API Deep Dive | 1 week | Claude platform mastery: Agent SDK, MCP, caching, citations, extended thinking |
| [4](module-4-agent-frameworks.md) | Agent Frameworks & Local Models | 1-2 weeks | Same agent in LangGraph, CrewAI, smolagents + Ollama |
| [5](module-5-memory-and-rag.md) | Memory & RAG | 1 week | RAG-powered research agent with long-term memory |
| [6](module-6-multi-agent-systems.md) | Multi-Agent Systems | 1 week | Multi-agent team with communication patterns |
| [7](module-7-evaluation.md) | Evaluation, Debugging & Quality | 1 week | Eval harness + debugging + human-in-the-loop |
| [8](module-8-production.md) | Production & Deployment | 1-2 weeks | Streaming, secure, versioned agent as REST API |
| [9](module-9-sota-frontiers.md) | SOTA & Frontiers | Ongoing | Fine-tuning, multimodal, computer use, agent UIs, ethics |
| [10](module-10-enterprise-data-strategy.md) | Enterprise Data & Knowledge Architecture | 1 week | Enterprise RAG with SQL, access control + local models |
| [11](module-11-enterprise-operations.md) | Enterprise Operations & Governance | 1-2 weeks | Security, compliance, multi-tenant, data flywheel, regulated industries + strategy doc |

## Guiding Principles

1. **Build before you framework** — Modules 0-2 use raw Python + API calls. No frameworks until Module 4.
2. **Vendor-neutral** — every hands-on exercise works with Claude, GPT, or Gemini. You'll try all three, plus open-source local models.
3. **Theory meets practice** — each module pairs papers/readings with code you write.
4. **Modular** — skip modules you already know, revisit ones you need to deepen.
5. **Security-first** — prompt injection, guardrails, and access control are core topics, not afterthoughts.
6. **Enterprise-grounded** — Modules 10-11 ground everything in real enterprise data strategy, governance, and production patterns backed by McKinsey, Gartner, and cloud provider reference architectures.

## What Changed in the Gap-Fix Update

The following gaps were identified via meta-review and addressed:

| Gap | Fix | Where |
|-----|-----|-------|
| Async Python not taught | Added prerequisite primer | M8 Unit 8.1 |
| Streaming not covered | Added dedicated unit | M8 Unit 8.4 |
| Prompt injection surface-level | Expanded to full unit with taxonomy + red-teaming exercises | M8 Unit 8.6 |
| No prompt/agent versioning | Added dedicated unit | M8 Unit 8.7 |
| Open-source/local LLMs absent | Added unit in M4 + enterprise section in M10 | M4 Unit 4.5, M10 Unit 10.5 |
| Fine-tuning not covered | Added unit with distillation, LoRA, data flywheel connection | M9 Unit 9.2 |
| Multimodal only mentioned | Added vision unit in M2 + expanded M9 unit | M2 Unit 2.5, M9 Unit 9.3 |
| Agent UIs not covered | Added unit with Chainlit, Streamlit, Gradio | M9 Unit 9.6 |
| Ethics only compliance-focused | Added dedicated ethics unit | M9 Unit 9.7 |
| M10 too dense (9 units) | Split into M10 (data/knowledge) + M11 (operations/governance) | M10, M11 |
| M5 checkpoint too ambitious | Made self-editing memory a stretch goal | M5 Checkpoint |
| M6 checkpoint too heavy | Implement once + written reflection (was "implement twice") | M6 Checkpoint |
| Agent debugging missing | Added debugging & testing unit | M7 Unit 7.6 |
| HITL only approval gates | Added correction, escalation, progressive autonomy patterns | M7 Unit 7.7 |
| Agent communication patterns missing | Added shared state, message passing, blackboard, event-driven | M6 Unit 6.6 |
| LLM-as-judge single-sourced | Added Zheng et al. paper, RAGAS, DeepEval | M7 Unit 7.3 |
| MemGPT single-sourced | Added alternative approaches + DeepLearning.AI course | M5 Unit 5.3 |
| Prerequisite gaps | Added JSON Schema, Pydantic, SQL, FastAPI primers | M2, M8, M10 |
| PDF parsing not specified | Added PyPDF2/pdfplumber references | M5 Unit 5.2 |
| DuckDuckGo setup missing | Added `duckduckgo-search` package reference | M2 Unit 2.6 |
| M1 survey overload | Made Xi et al. survey optional | M1 Unit 1.4 |
| M10 no code exercise | Added code-focused checkpoint (enterprise RAG + SQL + access control) | M10 Checkpoint |
| M11 disconnected from code | Checkpoint requires adding enterprise features to M10 agent | M11 Checkpoint |

## 2026-27 Watch List

Cross-cutting trends shaping the agentic AI landscape — referenced throughout the module frontier sections:

1. **Reasoning models converging** — all providers merging thinking/reasoning into flagship models; test-time compute scaling replaces "bigger model" as the primary capability lever
2. **MCP as universal standard** — donated to Linux Foundation AAIF, adopted by OpenAI, Google, Microsoft; 10,000+ community servers
3. **Open-source parity approaching** — benchmark gap narrowed from 15-20 to ~9 points; Qwen3, Llama 4, DeepSeek-R1, GPT-OSS provide genuine alternatives to commercial APIs
4. **Agentic coding revolution** — Claude Code (~$2B ARR), Cursor (360K paying), Codex CLI, Gemini CLI, Kiro IDE reshaping software engineering
5. **Computer use agents maturing** — OSAgent surpassed human baseline on OSWorld (76.26%); hybrid screenshot + accessibility tree approach dominant
6. **Prompt injection unsolved** — adaptive attacks bypass all published defenses at 90%+ success; defense-in-depth remains the only viable strategy
7. **EU AI Act enforcement** — August 2, 2026 deadline for high-risk AI compliance; penalties up to EUR 35M or 7% global turnover
8. **Enterprise adoption accelerating** — Gartner: 40% of enterprise apps with agents by end 2026 (from <5% in 2025); 75% require security/compliance as prerequisites

## Progress Tracking

Use the checkboxes in each module to track completion. Each module ends with a checkpoint — a mini-project that proves you've internalized the material.
