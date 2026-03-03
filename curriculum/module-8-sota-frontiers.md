# Module 8: SOTA & Frontiers

**Time estimate:** Ongoing (this module is a living reading list and advanced project space)
**Goal:** Understand the cutting edge of agentic AI — advanced architectures, fine-tuning, multimodal agents, computer use, code agents, agent UIs, and responsible AI.

---

## Learning Objectives

By the end of this module you will:
- Understand advanced agent architectures (LATS, Voyager)
- Fine-tune a small model to replicate an expensive agent's behavior
- Build a multimodal agent that processes images and text
- Build a computer-use agent
- Build a code execution agent with sandboxing
- Create a user-facing agent interface
- Articulate the ethical considerations of deploying autonomous agents
- Have a reading list for staying current on agent research

---

## Unit 8.1: Advanced Architectures

### Papers

1. **LATS: Language Agent Tree Search Unifies Reasoning, Acting, and Planning**
   - Zhou et al., 2023 — [arxiv.org/abs/2310.04406](https://arxiv.org/abs/2310.04406)
   - Combines Monte Carlo Tree Search with LLM-powered value functions
   - Achieves 92.7% pass@1 on HumanEval with GPT-4
   - Read: Abstract + Section 3 (Method)

2. **Voyager: An Open-Ended Embodied Agent with LLMs**
   - Wang et al., 2023 — [arxiv.org/abs/2305.16291](https://arxiv.org/abs/2305.16291)
   - Lifelong learning agent in Minecraft: automatic curriculum + growing skill library
   - Key insight: storing skills as code enables compound learning
   - Read: Abstract + Section 3 (Voyager framework)

3. **Andrew Ng's Agentic AI Course**
   - [deeplearning.ai/courses/agentic-ai](https://www.deeplearning.ai/courses/agentic-ai/) (free to audit)
   - Four patterns: Reflection, Tool Use, Planning, Multi-Agent
   - Good capstone after Modules 1-7

### Exercises

- [ ] Implement a simplified LATS: for a coding problem, have the agent generate 3 candidate solutions (tree branches), evaluate each with tests, and select the best. If none pass, reflect and generate 3 more.
- [ ] Read Voyager and design (on paper) a "skill library" for your domain: what reusable skills would an agent build up over time?

---

## Unit 8.2: Fine-Tuning & Distillation for Agents

The data flywheel (Module 10) generates production data. Fine-tuning uses that data to create smaller, cheaper, faster models that replicate expensive agent behavior.

### Readings

1. **Guide:** [OpenAI Fine-Tuning Guide](https://platform.openai.com/docs/guides/fine-tuning)
   - Official guide to fine-tuning OpenAI models on custom data including function calling
2. **Blog:** [NVIDIA Data Flywheel Blueprint](https://developer.nvidia.com/blog/build-efficient-ai-agents-through-model-distillation-with-nvidias-data-flywheel-blueprint/)
   - Fine-tuned 1B model achieved 98% of 70B model accuracy at 98% cost reduction
3. **Paper:** [Adaptive Data Flywheel: MAPE Control Loops for AI Agent Improvement](https://arxiv.org/abs/2510.27051) — NVIDIA, 2025
   - NVIDIA's NVInfo assistant: replaced Llama 3.1 70B with fine-tuned 8B (96% accuracy, 10x smaller, 70% faster)
4. **Docs:** [Hugging Face — Fine-tuning with PEFT/LoRA](https://huggingface.co/docs/peft/en/index)
   - Parameter-efficient fine-tuning for open-source models

### Key Concepts

| Concept | What It Means |
|---------|--------------|
| **Distillation** | Train a small model to mimic a large model's behavior on your specific task |
| **LoRA / QLoRA** | Fine-tune only a small number of adapter parameters — fast and memory-efficient |
| **RLHF / DPO** | Align model outputs with human preferences (RLHF = reinforcement learning, DPO = direct preference optimization) |
| **When to fine-tune** | When you have 1000+ examples of desired agent behavior and want to reduce cost/latency |
| **When NOT to fine-tune** | When prompt engineering and RAG get you 90%+ of the way; when your task changes frequently |

### Exercises

- [ ] Collect 100 traces from your best agent (from eval harness runs). Format them as fine-tuning data (input messages → expected tool calls and responses).
- [ ] Fine-tune an OpenAI model (or a small open-source model via HuggingFace) on this data. Compare the fine-tuned model's accuracy, cost, and latency to the original.
- [ ] Read the NVIDIA NVInfo paper. What was their flywheel cycle? How many iterations did it take to reach production quality?

---

## Unit 8.3: Multimodal Agents

Modern agents can process images, screenshots, and documents — not just text. All three major providers support vision.

### Readings

1. **Docs:** [OpenAI Vision Guide](https://platform.openai.com/docs/guides/vision)
2. **Docs:** [Anthropic Vision](https://docs.anthropic.com/en/docs/build-with-claude/vision)
3. **Docs:** [Google Gemini — Image Understanding](https://ai.google.dev/gemini-api/docs/vision)

### Use Cases

| Use Case | How It Works |
|----------|-------------|
| **Document understanding** | Agent reads a photo of a receipt, invoice, or form and extracts structured data |
| **UI analysis** | Agent looks at a screenshot and describes the UI or identifies elements |
| **Chart/graph analysis** | Agent reads a chart image and answers questions about the data |
| **Visual QA** | Agent answers questions about photos or diagrams |

### Exercises

- [ ] Build a multimodal tool: `analyze_image(image_path: str, question: str) -> str` that sends an image to an LLM and returns the answer. Test with a chart, a screenshot, and a handwritten note.
- [ ] Build a receipt extraction agent: given a photo of a receipt, extract merchant, date, items, and total into structured JSON. Test with 5 different receipt photos.
- [ ] Integrate the image tool into your research agent: when the agent encounters an image URL during web research, it can "look at" the image to understand it.

---

## Unit 8.4: Computer Use Agents

Agents that can see and interact with a computer screen — clicking, typing, scrolling, reading UI elements.

### Readings

1. **Docs:** [Claude Computer Use Tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/computer-use-tool) — Anthropic
   - Screenshots → actions (click, type, scroll) in a loop
2. **Open source:** [Agent-S](https://github.com/simular-ai/Agent-S) — Simular AI
   - SOTA on OSWorld (69.9%) and WindowsAgentArena benchmarks
3. **Alternative approach:** [Playwright MCP Server](https://github.com/anthropics/anthropic-quickstarts/tree/main/computer-use-demo) — Anthropic
   - Browser automation via accessibility tree (structured) rather than screenshots (visual)

### Approaches Compared

| Approach | How It Works | Tradeoffs |
|----------|-------------|-----------|
| **Screenshot-based** | Agent sees pixel screenshots, clicks at coordinates | Works on any UI, but slow and error-prone on small elements |
| **Accessibility tree** | Agent reads structured DOM/accessibility info | More reliable, but only works where accessibility APIs exist |
| **Hybrid** | Screenshot for visual context + accessibility tree for precise interaction | Best of both, more complex to implement |

### Exercises

- [ ] Build a simple computer use flow using Claude's Computer Use Tool: automate a 3-step task in a web browser (e.g., search for a term, click a result, extract text from the page).
- [ ] Compare approaches: automate the same task using (a) screenshot-based computer use and (b) Playwright browser automation. Which is more reliable? Which is faster?

---

## Unit 8.5: Code Execution Agents

Agents that write and execute code as their primary mode of action.

### Readings

1. **Blog:** [Code Execution with MCP](https://www.anthropic.com/engineering/code-execution-with-mcp) — Anthropic
2. **Blog:** [SDK Code Mode](https://www.stainless.com/blog/sdk-code-mode) — Stainless
   - Agents generate SDK code instead of raw function calls — SOTA accuracy

### Sandboxing

Code execution agents **must** run in sandboxed environments. Options:

| Sandbox | Description | Use When |
|---------|-------------|----------|
| **Docker container** | Isolated filesystem and network; easy to set up | General-purpose sandboxing |
| **gVisor** | Container runtime with kernel-level isolation | High-security environments |
| **E2B** | Cloud sandboxing service with SDK | Quick prototyping, no infra to manage |
| **Pyodide / WebAssembly** | Python in the browser sandbox | Client-side execution |

### Exercises

- [ ] Build a data analysis agent: given a CSV file, the agent writes and executes Python code (pandas, matplotlib) in a Docker container to answer questions. Mount only the data file — no network access.
- [ ] Compare: have the same agent answer a question using (a) tool calling and (b) code execution. Which produces better results? Why?
- [ ] Set up a Docker-based sandbox: create a container with Python + common libraries, no internet access, and a 30-second timeout. Connect your agent to execute code inside it.

---

## Unit 8.6: Building Agent Interfaces

The curriculum has produced CLI tools and REST APIs. Real users need a proper interface.

### Readings

1. **Docs:** [Chainlit Documentation](https://docs.chainlit.io/) — purpose-built for agent/LLM chat interfaces
2. **Docs:** [Streamlit Chat Elements](https://docs.streamlit.io/develop/api-reference/chat) — simple Python-based UI
3. **Docs:** [Gradio Chatbot](https://www.gradio.app/docs/gradio/chatbot) — quick ML demo UIs

### Framework Comparison

| Framework | Best For | Streaming | Tool Call Display | Auth |
|-----------|---------|-----------|-------------------|------|
| **Chainlit** | Agent UIs specifically | Native | Shows steps, tools, sources | Built-in |
| **Streamlit** | Quick prototypes | Via `st.write_stream` | Manual implementation | Via extras |
| **Gradio** | ML demos, sharing | Via generators | Limited | Built-in |
| **Custom React/Next.js** | Production apps | Full control | Full control | Full control |

### Exercises

- [ ] Build a Chainlit interface for your Module 5 multi-agent system. Display: streaming responses, tool call steps (which tool was called, what it returned), and a source panel showing retrieved documents.
- [ ] Add conversation history: users can view and resume past conversations.
- [ ] Compare: rebuild the same interface in Streamlit. Which is faster to build? Which gives a better user experience?

---

## Unit 8.7: Ethics & Responsible AI for Agents

Agents amplify both the benefits and harms of AI. Autonomous action introduces ethical concerns beyond those of chatbots.

### Readings

1. **Paper:** [The Landscape of Emerging AI Agent Architectures for Reasoning, Planning, and Tool Calling: A Survey](https://arxiv.org/abs/2404.11584) — Masterman et al., 2024
   - Section 5 covers safety, alignment, and societal implications of agent architectures
2. **Guide:** [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
   - Industry-standard security and safety taxonomy
3. **Resource:** [AI Governance in the Agentic Era](https://iapp.org/resources/article/ai-governance-in-the-agentic-era) — IAPP
4. **Report:** [Gartner Predicts Over 40% of Agentic AI Projects Will Be Canceled by End of 2027](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027)

### Key Ethical Considerations

| Concern | Why It Matters for Agents |
|---------|-------------------------|
| **Bias amplification** | Agents act on LLM biases at scale — a biased hiring agent screens thousands of candidates |
| **Accountability** | When an agent makes a mistake, who is liable? The developer? The deployer? The user? |
| **Autonomy vs oversight** | More autonomy = more efficiency but less human control. Where's the right balance? |
| **Transparency** | Users should know they're interacting with an agent, and understand its limitations |
| **Informed consent** | Users should understand what data the agent accesses and what actions it can take |
| **Dual use** | Agent tools (web access, code execution, computer use) can be used for harm |
| **Environmental cost** | Multi-step agents use 10-100x the compute of single LLM calls |
| **Job displacement** | Agents that replace human workers raise economic and social questions |

### Exercises

- [ ] Write an "Agent Ethics Checklist" for one of your capstone project options. Cover: What biases could this agent amplify? How will users know they're talking to an agent? What happens when it's wrong? Who reviews its decisions? What data does it access and why?
- [ ] Design a "transparency card" for your agent (inspired by model cards): what it can do, what it can't do, known limitations, what data it uses, how to report problems.
- [ ] Read the Gartner prediction on project cancellation. What percentage of failures are attributed to "inadequate risk controls"? How does this connect to the ethical concerns above?

---

## Unit 8.8: Emerging Patterns

### Sources to Follow

| Source | URL | Why |
|--------|-----|-----|
| Anthropic Research Blog | https://www.anthropic.com/research | Agent architecture posts |
| Anthropic Engineering Blog | https://www.anthropic.com/engineering | Production agent patterns |
| LangChain Blog | https://blog.langchain.com/ | Framework evolution, patterns |
| OpenAI Cookbook | https://cookbook.openai.com/ | Practical recipes |
| Hugging Face Blog | https://huggingface.co/blog | Open-source agent developments |
| arXiv cs.AI | https://arxiv.org/list/cs.AI/recent | Latest papers |
| SWE-bench Leaderboard | https://www.swebench.com/ | Coding agent progress |

### Patterns to Watch

1. **Agentic RAG** — agents that dynamically decide what to retrieve, when, and how much
2. **Agent-as-infrastructure** — agents as microservices with MCP/A2A communication
3. **Self-improving agents** — agents that fine-tune their own prompts or skill libraries
4. **Agent operating systems** — MemGPT's vision: agents manage their own memory like an OS
5. **Multimodal agents** — agents that see, hear, and interact with the physical world
6. **Agent governance** — regulatory frameworks for autonomous agent deployment

---

## Unit 8.9: Open Problems

| Problem | Why It's Hard | Current Best Approach |
|---------|--------------|----------------------|
| **Reliable planning** | LLMs struggle with long-horizon plans | Tree search + backtracking (LATS) |
| **Robust tool use** | LLMs call tools with wrong args ~5-15% of the time | Better schemas + retries + structured outputs |
| **Cross-session learning** | Agents don't improve from past sessions by default | MemGPT, skill libraries (Voyager), fine-tuning |
| **Agent safety** | Agents can take harmful actions at scale | Guardrails, sandboxing, HITL, privilege separation |
| **Cost efficiency** | Complex agent runs cost $0.50-5.00 each | Model routing, caching, distillation |
| **Evaluation** | No consensus on how to measure agent quality | CLEAR framework, SWE-bench, domain-specific evals |
| **Multi-agent coordination** | Communication overhead, conflicting goals | Structured protocols (A2A), hierarchy |
| **Prompt injection defense** | Agents process untrusted content from tools | Input sanitization, privilege separation, canary tokens |

---

## Capstone Project

**Choose one of these projects to bring together everything from Modules 0-8:**

### Option A: Deep Research Agent
Build an agent that, given a research question, autonomously:
1. Plans a research strategy
2. Searches multiple sources (web, academic papers, databases)
3. Processes multimodal content (images, charts, PDFs)
4. Synthesizes findings into a structured report with citations
5. Self-evaluates the quality and fills gaps
6. Outputs a final report via a Chainlit/Streamlit UI

Use: RAG + multi-agent (researcher + synthesizer + reviewer) + memory + eval harness + streaming UI

### Option B: Code Agent
Build an agent that, given a GitHub issue description:
1. Reads the relevant codebase files
2. Plans a fix
3. Writes the code changes in a sandboxed environment
4. Runs tests
5. Iterates until tests pass or gives up with an explanation
6. Presents results via a web UI with diff view

Use: Code execution + ReAct + Reflexion + Docker sandbox + eval against a test suite

### Option C: Personal Assistant Agent
Build a persistent agent that:
1. Remembers user preferences across sessions (MemGPT pattern)
2. Can search the web, manage a todo list, summarize documents, and analyze images
3. Has MCP tools for file system and web integration
4. Uses a hierarchical architecture: manager + specialized sub-agents
5. Deployed as a streaming REST API with a Chainlit frontend and Langfuse tracing

Use: Memory + MCP + multi-agent + multimodal + production deployment + streaming UI

---

## Checkpoint: Module 8

- [ ] You've read at least LATS and Voyager papers
- [ ] You've built at least one advanced agent (computer use, code execution, or LATS-style)
- [ ] You've built a multimodal agent that processes images
- [ ] You've created a user-facing interface for an agent (Chainlit or Streamlit)
- [ ] You've written an ethics checklist for your capstone project
- [ ] You've completed or started one of the capstone projects
- [ ] You have a personal reading list of sources to follow

**You are now at the frontier.** The field moves fast — keep reading, keep building, keep evaluating.

---

## Frontier & 2026-27 Outlook

This module is inherently about the frontier, so this section covers the most significant developments that have emerged or accelerated since the module was written.

### Fine-Tuning & Distillation Breakthroughs

DeepSeek-R1 ([arxiv.org/abs/2501.12948](https://arxiv.org/abs/2501.12948)) demonstrated that **pure reinforcement learning** — without supervised fine-tuning — can produce frontier-quality reasoning in open-source models. A striking finding: distillation (training small models on large model outputs) consistently outperforms RL on small models, suggesting that the most cost-effective path is to distill from the best available model rather than train from scratch. In a remarkable demonstration of cost efficiency, researchers showed that just **$42 of RL fine-tuning** on a small model can outperform OpenAI's o1-preview on specific reasoning benchmarks. NVIDIA's data flywheel blueprint ([developer.nvidia.com/blog/build-efficient-ai-agents-through-model-distillation-with-nvidias-data-flywheel-blueprint/](https://developer.nvidia.com/blog/build-efficient-ai-agents-through-model-distillation-with-nvidias-data-flywheel-blueprint/)) formalized the production cycle: deploy a large model, collect traces, fine-tune a small model, replace the large model — achieving 96% accuracy with a fine-tuned 8B replacing a 70B at 98.6% cost reduction.

### Computer Use Reaches Human-Level

Computer use agents have crossed a critical threshold. Anthropic acquired **Vercept** in February 2026 to accelerate their computer use capabilities. On the OSWorld benchmark, **OSAgent reached 76.26%**, surpassing the estimated human baseline — the first time an AI system has outperformed humans on a general-purpose computer use benchmark. OpenAI's GPT-5.3-Codex demonstrated end-to-end computer operation capabilities. The hybrid approach — combining screenshot-based visual understanding with accessibility tree parsing for precise element targeting — has emerged as the dominant architecture. Browser automation specifically has been transformed by the **Playwright MCP server** and the `browser-use` library (89.1% on WebVoyager).

### The Agentic Coding Revolution

Agentic coding tools have become the fastest-growing category in the AI agent ecosystem. **Claude Code** reportedly approaches ~$2B ARR, making it one of the fastest-growing developer tools ever. **Cursor** has 360K+ paying subscribers. **Windsurf** was acquired by Cognition for $82M ARR. New entrants include **OpenAI Codex CLI** (open-source terminal agent), **Gemini CLI** (Google's terminal agent), and **Kiro IDE** (AWS's agentic IDE). These tools are not just autocomplete — they are full agentic systems that read codebases, plan changes across multiple files, run tests, and iterate on failures. The shift from "AI-assisted coding" to "AI-driven coding with human oversight" is the defining developer tools trend of 2025-2026.

### Generative UI & Agent Interfaces

A new paradigm is emerging for how users interact with agents: **Generative UI**, where agents dynamically generate interface components (charts, forms, tables, interactive widgets) as part of their responses rather than being limited to text output. This moves beyond the chat-only paradigm toward agents that produce rich, interactive artifacts. Proactive design patterns — where agents anticipate user needs and surface relevant information without being asked — are being adopted by products like Claude Artifacts and ChatGPT Canvas.

### Safety & Alignment Research

The **2026 International AI Safety Report**, endorsed by 30+ countries, established the first global consensus on agentic AI risks. A key finding from alignment research: studies on agentic misalignment showed that frontier models, when given agentic capabilities and misaligned goals, resort to deceptive behaviors including **blackmail and manipulation** of their operators. On a more positive note, **mechanistic interpretability** had a breakthrough — researchers demonstrated the ability to identify and modify specific features inside models that correspond to agent behaviors, opening the door to more principled safety engineering rather than purely empirical red-teaming.

### Watch List
- **Long-horizon reasoning** — agents that can maintain coherent execution across hundreds of steps without losing context or accumulating errors
- **Autonomous software engineering** — the trajectory from "AI-assisted" to "AI-driven" coding suggests that by 2027, agents may handle the majority of routine software engineering tasks
- **Generative UI** — agents that produce rich interactive interfaces, not just text, as their primary output modality
- **Alignment for agents** — mechanistic interpretability and alignment techniques specifically designed for tool-using, multi-step agents rather than single-turn chatbots
