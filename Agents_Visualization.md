# Visualizing Agentic Systems (Handout)

These diagrams are **ready to paste** into your Study_Guide.md or GitHub docs. They visualize agents per our definition: *a system that uses an LLM in a loop of reasoning + acting, guided by goals and context, choosing tools, with guardrails, HITL, memory, and clear termination criteria.*

---

## 1) Agent Control Loop (core mental model)

```mermaid
flowchart LR
  subgraph Inputs
    G[Goals / Tasks]
    C[Context / Instructions]
    D[Retrieved Data]
  end

  subgraph Agent
    R[Reasoner / Planner]
    TSel[Tool Selector]
    M[(Memory / Session State)]
    TCrit{Termination Check?}
  end

  subgraph Safeguards
    GR[Guardrails / Policies]
    H[HITL Approval?]
    B[Budget / Timeout]
  end

  subgraph Env[Environment & Tools]
    Tool1[(APIs)]
    Tool2[(DB / Vector Store)]
    Tool3[(Web / Search)]
  end

  Inputs --> R
  M --> R
  R --> GR
  GR --> R

  R --> TSel
  TSel -->|choose| Tool1
  TSel -->|choose| Tool2
  TSel -->|choose| Tool3
  Tool1 -->|observation| R
  Tool2 -->|observation| R
  Tool3 -->|observation| R

  R --> M
  R --> TCrit
  B -. monitors .- R
  H --> R
  TCrit -->|No| R
  TCrit -->|Yes| O[(Output / Report / Action Plan)]
```

**Read it like this:** goals + context feed a **Reasoner** that selects tools, collects observations, updates **Memory**, checks **guardrails/budget/HITL**, and repeats until **termination criteria** is met.

---

## 2) Agent Anatomy (what’s inside an agent)

```mermaid
flowchart TB
  subgraph Agent[Agent]
    P[Policy / Instructions]
    R[Reasoning Engine (LLM)]
    MS[(Memory Store)]
    EV[Evals / Tracing Hooks]
    GA[Guardrails: schemas • allowlists • PII filters]
    AU[Autonomy: step limits • budgets • timeouts]
    TS[Tooling Adapters]
  end

  subgraph Interfaces
    IN[Inputs: goals • user msgs • events]
    OUT[Outputs: answers • actions • artifacts]
  end

  subgraph External
    TOOLS[(APIs • DBs • Search • File I/O)]
    DATA[(Docs • Vector DB • KB)]
  end

  IN --> P --> R
  R -->|read/write| MS
  R -->|via| TS --> TOOLS
  R --> DATA
  GA --> R
  AU --> R
  EV --> R
  R --> OUT
```

**Notes:** treat **Memory** as data you can version/audit. **Guardrails** and **Autonomy** are first‑class, not afterthoughts.

---

## 3) Orchestration Patterns (from simple to complex)

```mermaid
flowchart LR
  A1[Single Agent] --> OUT1[Result]

  subgraph Crew[Coordinator + Workers]
    C[Coordinator Agent] --> W1[Worker 1]
    C --> W2[Worker 2]
    W1 --> C
    W2 --> C
  end

  subgraph Graph[LangGraph-style]
    N1[Node: Retrieve] --> N2[Node: Reason]
    N2 -->|tool call| N3[Node: Act]
    N3 --> N4{Check Termination?}
    N4 -- No --> N2
    N4 -- Yes --> N5[Emit Result]
  end
```

**Map to frameworks:** Single agent (OpenAI Agents SDK); Coordinator/workers (AutoGen, CrewAI); Graph loop (LangGraph).

---

## 4) HITL & Guardrails (who’s in control)

```mermaid
sequenceDiagram
  participant U as User
  participant A as Agent
  participant G as Guardrail/Policy
  participant H as Human Reviewer
  participant T as Tool/API

  U->>A: Goal / Task
  A->>G: Validate request (policy, schema)
  G-->>A: Allowed / Blocked / Redact
  A->>T: Tool call (if allowed)
  T-->>A: Observation
  A->>H: HITL checkpoint (risky action)
  H-->>A: Approve / Revise / Reject
  A-->>U: Result + Trace
```

**Idea:** Explicit **checkpoints** for risky actions (payments, data writes).

---

## 5) MCP (Model Context Protocol) as the ACI

```mermaid
flowchart LR
  A[Agent / Client] <-- MCP --> S1[MCP Server: Files]
  A <-- MCP --> S2[MCP Server: DB/SQL]
  A <-- MCP --> S3[MCP Server: Web/Search]
  S1 --- ToolA[(read/write files)]
  S2 --- ToolB[(parameterized queries)]
  S3 --- ToolC[(search • fetch • crawl)]
```

**Why it matters:** **Standardizes** how agents discover/call tools & data. Servers expose capabilities; clients (agents) consume them consistently.

---

## 6) Autonomy & Termination (state view)

```mermaid
stateDiagram-v2
  [*] --> Plan
  Plan --> Act
  Act --> Observe
  Observe --> Check
  Check --> Plan: Not done, within budget
  Check --> [*]: Goal met
  Check --> [*]: Budget/timeout exceeded
  Check --> [*]: No progress (stall)
```

**Make this concrete** with thresholds: step_limit, money_limit, time_limit, no_improvement_k.

---

## 7) Workflow vs Agent (decision helper)

```mermaid
flowchart TB
  Q{Is control flow known and stable?}
  Q -->|Yes| WF[Use a Workflow/Script]
  Q -->|No| Q2{Does the task require tool choice/iteration?}
  Q2 -->|No| WF
  Q2 -->|Yes| Q3{Is risk bounded with guardrails/HITL?}
  Q3 -->|No| PREP[Add guardrails, budgets, checkpoints]
  PREP --> Q3
  Q3 -->|Yes| AGT[Use an Agentic Loop]
```

**Rule of thumb:** Prefer scripts for deterministic tasks; reserve agents for **open‑ended** goals needing tool choice + iteration.

---

## 8) Evals & Telemetry (close the loop)

```mermaid
flowchart LR
  SPEC[Task Spec & Testcases] --> RUN[Run Agent]
  RUN --> TRACE[Collect Traces/Spans]
  RUN --> METRICS[Success • Cost • Latency • Incidents]
  TRACE --> REG[Regression & Diff Tools]
  METRICS --> REG
  REG --> IMP[Refine prompts • tools • policies]
  IMP --> RUN
```

**Outcome:** A measurable improvement loop, not vibes.

---

### Tips for Use
- Paste any diagram into GitHub markdown (Mermaid supported).  
- Create variants per framework (OpenAI Agents SDK, CrewAI, LangGraph, AutoGen).  
- Keep **guardrails/HITL/budgets** visible in every picture to reinforce safety and control.
