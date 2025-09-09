# Agentic AI Engineering – Study Guide (2025 Cohort)

## Purpose
Give the cohort a shared vocabulary, quick context from industry, and lightweight checks so everyone can reason about when and how to use agents — not just how to code them.  

This guide is a **living document**: add notes, questions, and insights as you progress each week.

---

## How to Use This Guide
- Skim the **core readings** each week (10–15 min)  
- Work through **labs** during sessions  
- Use the **glossary and cheat sheets** when frameworks introduce their own terms  
- Add your own **reflections** at the end of each week  

---

## Week 1 – Fundamentals & Shared Vocabulary

**Core Readings (10–15 min)**  
- [Anthropic — Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) *(principles for designing useful/safe agents)*  
- [Anthropic — Model Context Protocol (MCP)](https://www.anthropic.com/news/model-context-protocol) *(emerging interoperability standard)*  
- [Sourcegraph — Revenge of the Junior Developer](https://sourcegraph.com/blog/revenge-of-the-junior-developer) *(market framing: completions → chat → agentic)*  
- [Hugging Face — What are Agents?](https://huggingface.co/learn/agents-course/en/unit1/what-are-agents) *(Hugging Face Agents Course)*

**Reflection Prompts**  
- What’s the difference between a workflow and an agent?  
- When would you *not* use an agent?  
- What new term stood out this week?  

---

## Week 2 – OpenAI Agents SDK
**Primers**  
- [Overview](https://platform.openai.com/docs/guides/agents)  
- [Python SDK Docs](https://openai.github.io/openai-agents-python/)  

**Focus:** agents • tools • handoffs • guardrails • sessions  

---

## Week 3 – CrewAI
**Primers**  
- [Docs](https://docs.crewai.com)  

**Focus:** agents • tasks • tools • crews/flows • role/goal specs  

---

## Week 4 – LangGraph
**Primers**  
- [Overview](https://www.langchain.com/langgraph)  
- [Docs](https://langchain-ai.github.io/langgraph/)  

**Focus:** nodes/edges • state store • checkpointing • HITL pauses • durable execution  

---

## Week 5 – AutoGen
**Primers**  
- [GitHub](https://github.com/microsoft/autogen)  

**Focus:** multi-agent chat • coordinator/worker roles • autonomy policies  

---

## Week 6 – Model Context Protocol (MCP)
**Primers**  
- [Anthropic Announcement](https://www.anthropic.com/news/model-context-protocol)  

**Focus:** agent-computer interface (ACI) • tool servers/clients • open standardization  

---

## Glossary (Living Reference)

### Agent Basics  
- **Agent** — A system that uses a language model in a **loop of reasoning and acting**, guided by goals and context, to choose which tools or steps to take. Unlike a static workflow, agents can adapt dynamically and pursue objectives, but they must be designed with constraints, safety checks, and clear termination rules (Anthropic – *Building Effective Agents*).  
- **Framework** — A structured environment or toolkit that provides the **building blocks** for agents: APIs, orchestration patterns, memory, guardrails, and integration points with tools. Frameworks (e.g., OpenAI Agents SDK, CrewAI, LangGraph, AutoGen) standardize how agents are created, controlled, and deployed.  
- **MCP (Model Context Protocol)** — An **open interoperability standard** that defines how agents connect to external tools and data sources. In MCP, servers expose capabilities (tools, data, functions), while clients (agents) can discover and call them in a consistent way. This makes agent systems more modular, auditable, and easier to integrate across vendors (Anthropic – *Model Context Protocol*).  

### Control & Safety  
- **Guardrail** — Constraint preventing unsafe/undesired actions (e.g., only allow SQL read-only).  
- **HITL (Human-in-the-Loop)** — Planned pause/approval or escalation to a human.  

### Execution & Memory  
- **Session & Memory** — State carried across steps/runs. Treat as versioned/audited data.  
- **Checkpointing** — Persisting state to resume, branch, or debug long-running agents.  

### Governance & Metrics  
- **Termination Criteria** — Clear stopping rules (goal achieved, budget exceeded, no progress).  
- **Evals & Metrics** — Evidence of success: cost, latency, safety incidents, regression tests.  

---

## Term-to-Framework Mapping Cheat Sheet

| Concept            | OpenAI Agents SDK   | CrewAI             | LangGraph                           | AutoGen                       |
|--------------------|----------------------|--------------------|-------------------------------------|-------------------------------|
| **Agent**          | agents              | agents             | nodes                               | chat agents                   |
| **Tool**           | tools               | tools              | edges + functions                   | workers / tools               |
| **Handoff**        | handoffs            | crews/flows        | state transitions                   | coordinator → worker           |
| **Guardrails**     | guardrails          | role/goal specs    | HITL pauses, schemas                | autonomy policies              |
| **Memory/State**   | sessions            | task context       | state store, checkpointing          | conversation context           |

---

## Notes & Reflections (for you to fill in)
- Week 1 notes:  
- Week 2 notes:  
- …  
