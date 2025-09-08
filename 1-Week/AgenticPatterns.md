# Agentic Patterns

This document summarizes the most common **agentic design patterns** used in building AI agents. These patterns have emerged across academic research (e.g., *Agent Design Pattern Catalogue*), industry blogs (Anthropic, OpenAI), and frameworks (LangChain, LangGraph, CrewAI).

---

## 🔑 Core Agentic Patterns

### 1. ReAct (Reason + Act)
- **Idea:** Alternates between reasoning (LLM thought) and acting (tool use).
- **Why it matters:** Provides balance between deliberation and execution.
- **Use cases:** Retrieval-augmented generation (RAG), tool workflows.

### 2. Reflection / Self-Correction
- **Idea:** Agent produces an output, then critiques or refines it.
- **Why it matters:** Improves reliability, reduces hallucinations.
- **Use cases:** Content generation, code generation, planning tasks.

### 3. Planning + Execution
- **Idea:** Separate step for planning a sequence of tasks, followed by execution.
- **Why it matters:** Brings structure and efficiency for multi-step processes.
- **Use cases:** Research tasks, workflow orchestration.

### 4. Tool Use / Function Calling
- **Idea:** Agent selects from tools (search, APIs, databases, robotics actions).
- **Why it matters:** Turns an LLM into an action-taking system.
- **Use cases:** Automation, assistants, retrieval tasks.

### 5. Memory-Augmented Agents
- **Idea:** Store and retrieve knowledge beyond the context window.
  - Short-term (scratchpads)
  - Long-term (vector databases)
  - Episodic/Semantic memory
- **Why it matters:** Enables personalization, continuity, and complex reasoning.
- **Use cases:** Personal assistants, tutoring systems.

### 6. Multi-Agent Collaboration
- **Idea:** Multiple specialized agents interact (Planner, Researcher, Coder, Critic).
- **Why it matters:** Mirrors human teamwork; improves scalability.
- **Use cases:** Complex problem solving, simulations.

---

## 🧩 Secondary / Emerging Patterns

- **Hierarchical Agents:** Manager/worker delegation setups.
- **Simulation / Roleplay Agents:** Training, synthetic data, civic engagement.
- **Governance / Safety Loops:** Critic or reviewer agents enforcing constraints.
- **MCP (Model Context Protocol):** Standardizing how agents access tools.

---

## 📊 Summary Table

| Pattern                | Purpose                                   | Typical Use Cases                        |
|-------------------------|-------------------------------------------|------------------------------------------|
| ReAct                  | Reasoning + action interleaving           | QA, RAG, tool-augmented workflows        |
| Reflection             | Self-critique and retry                   | Content generation, coding, planning     |
| Planning + Execution   | Decompose → execute                       | Multi-step tasks, orchestration          |
| Tool Use               | Invoke external functions/APIs            | Retrieval, automation, web search        |
| Memory Augmentation    | Extend context, persistence               | Personal assistants, tutoring, continuity|
| Multi-Agent Systems    | Role specialization, collaboration        | Research, civic bots, multi-domain tasks |
| Hierarchical Agents    | Delegation / supervision                  | Complex workflows, enterprise automation |
| Safety / Critic Loops  | Constraint, guardrails, governance        | Compliance, ethics, AI safety            |

---

## 🌐 Visual Diagram

```mermaid
mindmap
  root((Agentic Patterns))
    ReAct
      Reasoning
      Acting (tool calls)
    Reflection
      Self-critique
      Retry/Refine
    Planning & Execution
      Planner role
      Executor role
    Tool Use
      API calls
      Function calling
    Memory
      Short-term (scratchpad)
      Long-term (vector DB)
      Episodic/Semantic
    Multi-Agent
      Planner/Researcher/Coder
      Critic/Reviewer
    Hierarchical
      Manager agent
      Worker agents
    Safety & Governance
      Guardrails
      Oversight agents
