# Agentic Patterns – Integrated with Anthropic Principles

This document combines a taxonomy of **common agentic design patterns** with guidance from Anthropic’s [*Building Effective Agents*](https://www.anthropic.com/engineering/building-effective-agents). Together, they provide a clearer picture of how to design and reason about AI agents effectively.

---

## ⚖️ Workflows vs. Agents

Anthropic emphasizes an important distinction:

- **Workflows** – Linear, predefined processes (e.g., prompt chaining, routing, orchestrated steps).
- **Agents** – Systems where an LLM **decides dynamically** how to act, which tools to use, and when to stop.

👉 Many real-world systems blend these two approaches. Workflows offer structure, while agentic autonomy adds flexibility.

---

## 🔑 Core Agentic Patterns

### 1. ReAct (Reason + Act)
- **Idea:** Agent alternates between reasoning steps and tool actions.
- **Why it matters:** Transparent reasoning, effective in tool-augmented tasks.
- **Anthropic Link:** Core part of the *augmented LLM* model.

---

### 2. Reflection / Self-Correction
- **Idea:** Agent critiques and improves its own output (Reflexion loops).
- **Why it matters:** Boosts reliability, reduces hallucinations.
- **Anthropic Link:** Fits within iterative workflow designs that favor refinement.

---

### 3. Planning + Execution
- **Idea:** Agent decomposes a problem into sub-tasks (plan) and then executes them.
- **Why it matters:** Reduces token waste, enforces structure on complex tasks.
- **Anthropic Link:** Equivalent to **prompt chaining** workflows.

---

### 4. Tool Use / Function Calling
- **Idea:** Agents invoke APIs, search tools, databases, or actuators.
- **Why it matters:** Enables agents to take action beyond text generation.
- **Anthropic Link:** Anchored in the **augmented LLM** (tools, retrieval, code execution, files, caching).
- **Note:** The **Model Context Protocol (MCP)** is emerging as a standard for tool integration.

---

### 5. Memory-Augmented Agents
- **Idea:** Beyond the context window, agents use:
  - Short-term scratchpads
  - Long-term vector stores
  - Episodic or semantic memory
- **Why it matters:** Provides persistence, personalization, and continuity.
- **Anthropic Link:** Part of augmenting LLMs to extend their effective context.

---

### 6. Multi-Agent Collaboration
- **Idea:** Specialized agents (Planner, Researcher, Critic, Synthesizer) interact.
- **Why it matters:** Mirrors human teamwork; allows parallelization.
- **Anthropic Example:** *Orchestrator–worker* architecture in Anthropic’s Research system.
  - Orchestrator spawns subagents in parallel.
  - Subagents gather/search/summarize independently.
  - Orchestrator synthesizes and iterates.
- **Trade-offs:** Richer reasoning, but higher latency and cost.

---

## 🧩 Secondary / Emerging Patterns

- **Hierarchical Agents** – Manager delegates to workers (variant of orchestrator–worker).
- **Simulation / Roleplay Agents** – Agents emulate roles or personas (training, synthetic data).
- **Governance / Safety Loops** – Critic or reviewer agents filter, enforce policies, or add guardrails.
- **Routing / Parallelization Workflows** – As described by Anthropic, workflows can branch or run in parallel to speed up results.

---

## 📊 Crosswalk Table

| Pattern Group             | Anthropic Equivalent / Insight                              |
|----------------------------|-------------------------------------------------------------|
| ReAct                     | Core of *augmented LLM* reasoning–action loops              |
| Reflection                | Iterative refinement within workflows                       |
| Planning + Execution      | Prompt chaining and orchestration workflows                 |
| Tool Use                  | Augmented LLM with MCP, code exec, file API, caching        |
| Memory Augmentation       | Long-term extensions to LLM context                         |
| Multi-Agent Systems       | Orchestrator–worker architecture for parallelism            |
| Hierarchical Agents       | Variant of orchestrator–worker                              |
| Governance / Safety Loops | Critic/oversight agents, ensuring reliability & compliance  |

---

## 🌐 Visual Diagram

```mermaid
mindmap
  root((Agentic Patterns))
    Workflows
      Prompt chaining
      Routing
      Parallelization
    Agents
      ReAct
      Reflection
      Planning & Execution
      Tool Use
      Memory
      Multi-Agent
      Hierarchical
      Safety & Governance
