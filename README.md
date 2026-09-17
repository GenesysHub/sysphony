# SYSPhONY

**The MCP platform where every capability is a tool, for humans and AI agents.**

One endpoint for UI, workflows, data and agents, with any model.

> **Testing phase · internal use.** Sysphony is already used and tested on internal
> projects; it will soon open for private experimentation and, once all phases are
> complete, be released as open source.

🔗 **Live page:** open `index.html` (or the GitHub Pages URL of this repo).

---

## sysphony · /ˈsɪs.fə.ni/ · *noun*

1. The coordinated and harmonious integration of multiple systems, components, or
   processes that function as a unified whole.
2. *(Figurative)* A complex and elegant arrangement of interconnected parts operating
   seamlessly, like a symphony orchestra.

### Sysphony Platform — modern definition

A **tool-centric** cognitive architecture built around one idea: **everything is a
tool**. A single [MCP](https://modelcontextprotocol.io) layer allows people and agents
to invoke tools to **generate interfaces**, **compose workflows**, **query knowledge and
databases**, and **perform semantic search**. **Model-independent**
([OpenRouter](https://openrouter.ai), [Ollama](https://ollama.com),
[OpenAI](https://openai.com), [Anthropic](https://www.anthropic.com), or any compatible
endpoint) and **database-flexible** (SQLite, [MongoDB](https://www.mongodb.com),
[PostgreSQL](https://www.postgresql.org)).

---

## About · Everything is a tool

**Sysphony** moves beyond the chatbot paradigm. At its core is a
**[Model Context Protocol (MCP)](https://modelcontextprotocol.io) aggregator**: it
connects MCP servers and makes **every capability a tool**. Tools are not a box in the
diagram: they are the **connections themselves**, the means through which the system
operates. Generative UI, a workflow, a database, a knowledge base: each is reached as a
tool.

Because everything is a tool, the **user can reach them directly**: execute a workflow,
query a database, access a knowledge base, or drive the UI. The same tools can perform
different tasks depending on how they are configured. Nothing is a special case: the
system grows in capability without multiplying paradigms.

The **AI** works through a **router agent**: the router analyzes the task and selects the
most appropriate **sub-agents**. Each sub-agent operates through one or more **domains**,
which provide access to specific resources, knowledge, or tools. **Sub-agents and domains
are distinct, composable entities**: a domain can be shared by multiple sub-agents, while
a sub-agent can use multiple domains (*AI → router agent → sub-agent → domain → tools*).
This separation makes the architecture modular: agents define behavior and operational
context, while domains encapsulate reusable capabilities and resources. A tool action can
also close the loop: a **WebSocket** event emitted by a tool can **wake the agent**,
allowing the system to react autonomously to events.

Under the hood: an [Express](https://expressjs.com) + WebSocket core that scales through
role separation — a **state server** for persistence and real-time events, and
**stateless compute nodes** for executing tools and agents. It works with MCP clients such
as [Claude Desktop](https://claude.ai/download), [Cursor](https://cursor.com),
[Codex](https://developers.openai.com/codex), [Kiro](https://kiro.dev), and any compatible
client. From single-node mode to a full cluster on [AWS](https://aws.amazon.com), the same
[Docker](https://www.docker.com) image can be deployed anywhere.

---

## Core capabilities · The tools layer in action

### Foundation · MCP federation
It all starts from a single **MCP aggregator**: memory, workflow, browser automation
([Stagehand](https://www.stagehand.dev)), fetch, search, and agent invocation are already
built-in tools. Connect any **external MCP server** and its capabilities join the same
registry, callable by humans and agents alike: one access point to everything. See the
[MCP specification](https://modelcontextprotocol.io).

### Compose · Workflows and generative UI
If every capability is a tool, you can compose it. Chain tools into a visual **workflow**,
with conditional branches and parallel steps: the whole graph becomes a **callable tool**
with an auto-generated schema (on [React Flow](https://reactflow.dev)). The same logic
applies to the interface: agents discover components, create new ones, and reshape layouts
in real time. `JSON in → UI out`, with no redesign each time.

### Data · Knowledge, databases and semantic search
Data is the fuel of agents, so it too is a tool. Query **SQLite**,
[MongoDB](https://www.mongodb.com), and [PostgreSQL](https://www.postgresql.org) through
one interface, and load datasets into **knowledge domains** indexed for full-text and
meaning. On top runs a **hybrid keyword + vector retrieval**, with embeddings stored in
the database itself: the basis of RAG for every agent and workflow.

### Experts · Domains and MoE-RAG agents
Around each resource (tool, workflow, database, or knowledge base) you can create a
**domain**: an expert constrained to that resource, with its own sources, memory, and
tasks. An **agent** is then a **mixture-of-experts**, a router plus the domains that
compose it, and the router routes each request to the right expert. Domains are
composable: one domain serves many agents and an agent holds many domains. Add a resource,
get a domain, reuse it anywhere, with no duplication.

### Runtime · Model-independent and real-time events
Execution is not tied to any provider: [OpenRouter](https://openrouter.ai),
[Ollama](https://ollama.com), [OpenAI](https://openai.com),
[Anthropic](https://www.anthropic.com), [Groq](https://groq.com), or local models, as long
as the runtime supports that model or endpoint. While tools run, **WebSocket channels**
stream execution events, tool activity, agent messages, and app state: the compute → state
relay keeps UI and agents always in sync.

### Deploy · Client-independent and cluster-ready
As an MCP server, it works with [Claude Desktop](https://claude.ai/download),
[Cursor](https://cursor.com), [Codex](https://developers.openai.com/codex),
[Kiro](https://kiro.dev), and other compatible MCP clients. The same
[Docker](https://www.docker.com) image goes from a local machine to a cluster scaled on
[AWS ECS Fargate](https://aws.amazon.com/ecs/fargate/), with peer registration via HMAC.

---

## FAQ · why it is built this way

The questions I asked myself while building Sysphony, and the reasoning behind each
choice, without beating around the bush.

### Why tool-centric, with a single interface?
In years of programming I have seen functions get lost, duplicated, and disappear into the
ether of libraries. **MCPs** instead provide a **registry of reusable tools**, natively
callable by AI as well. Hence a **multi-MCP** structure: MCP servers dedicated to specific
projects, with workflows composed of internal and external MCPs. A kind of inception,
where **tools create projects and other tools, and then execute them**: without writing
code, through configuration. What was needed was a uniform system usable in the same way
by people and agents.

### In what sense are workflows tools?
Composing tools to create **"complex tools"** is the key to sub-MCPs: give an agent a
specific MCP and you limit its area of action and knowledge. A workflow is not just a
tool: it has **object** nodes (which define the schema), **agent** nodes (inputs such as
context, lateral tools, and typed output), and **condition** nodes for branching
execution. **Edges** connect specific values from input/output schemas and handle lists,
map, and foreach over arrays, making **parallel batch execution** straightforward. Each
workflow can have **cron and WebSocket** triggers and a **per-step history**: complete
debugging and logs.

### Why should the UI be generative?
The internet is changing: traditional websites risk becoming increasingly rigid interfaces
compared with what agents can do. What matters is **information and action**, and the UI
should be **generated from data** and from the interests of the person using it. Why give
everyone the same panel with a thousand buttons? Someone who lives in Excel can have an
Excel-style UI; someone who wants more can build their own panel, with or without an
agent's help. And generative UI lets the agent **show you the data**: no longer just a
chat, but an interface that the AI composes and adapts proactively.

### How do knowledge and semantic search work?
Working with many agents and large amounts of data, the problem is retrieving the **right
information** across different topics: **context is the heart of an agent**, the foundation
of its continuity and operation. Since everything passes from the **memory** tool to the
database server, I built a simple way to create and manage new DBs, adding **chunking and
vectorization** to the most important ones, so knowledge tools remain reliable and focused.

### What is the MoE-RAG router, and why domains instead of a single agent?
I do not want to have to reinitialize a session for every task. A context that grows
indefinitely can saturate the agent: higher cost, lower precision, and sessions that become
difficult to manage. The answer is a **mixture-of-experts**: a **lightweight, always-fresh
router agent** (each call is a new session, but with persistent base context) that observes
state and task, selects the appropriate **sub-agents**, and lets them operate through their
assigned domains. **Sub-agents and domains are distinct, reusable entities**: a sub-agent
can use one or more domains, while the same domain can be shared by multiple sub-agents. The
router can query the knowledge of other experts, invoke them, emit events, and talk to the
user. It is not a chat: it is **context-based continuity**, written in the background by
other agents, filtered, and ready to explore. The router can write directives, create new
sub-agents and domains, and **schedule its own execution over time**. Domains are
specializations (database, knowledge, tools, sites) that sub-agents can compose as needed.
*The entity is the binary, the model is the carriage.*

### Is it tied to a specific provider or model?
No, it is an architectural requirement: **no dependency on a single provider**. As an MCP
server, it can work with local or third-party MCP clients and, depending on the runtime and
integration, with different models and providers. The tools layer remains independent of the
underlying model. Thanks to **WebSocket** triggers, the router also gains a **temporal
dimension**: it can schedule future self-executing events, pause, and, when awakened, check
the state of jobs, tasks, and agents.

---

## Tech

[Express](https://expressjs.com) · WebSocket · [MCP](https://modelcontextprotocol.io) ·
[OpenRouter](https://openrouter.ai) / [Ollama](https://ollama.com) · SQLite / MongoDB /
PostgreSQL · [React Flow](https://reactflow.dev) ·
[Stagehand](https://www.stagehand.dev) · [Docker](https://www.docker.com) ·
[AWS ECS Fargate](https://aws.amazon.com/ecs/fargate/) · HMAC cluster auth

---

*EST. 2026 — [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0)*
