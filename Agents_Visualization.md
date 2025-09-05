# Visualizing Agentic Systems (GitHub‑safe Mermaid)


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
    M[Memory / Session State]
    TCrit{Termination Check?}
  end

  subgraph Safeguards
    GR[Guardrails / Policies]
    H[HITL Approval?]
    B[Budget / Timeout]
  end

  subgraph Env[Environment and Tools]
    Tool1[APIs]
    Tool2[DB or Vector Store]
    Tool3[Web or Search]
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
  TCrit -->|Yes| O[Output or Report]
```

---

## 2) Agent Anatomy (what is inside an agent)

### 2A. Minimal version (no subgraphs, maximally compatible)

```mermaid
flowchart TB
  IN[Inputs: goals, messages, events] --> P[Policy and Instructions]
  P --> R[Reasoning Engine or LLM]
  R --> MS[Memory Store]
  R --> TS[Tool Adapters]
  TS --> TOOLS[APIs, DBs, Search, File IO]
  R --> DATA[Docs, Vector DB, Knowledge Base]
  GA[Guardrails: schemas, allowlists, PII filters] --> R
  AU[Autonomy: step limits, budgets, timeouts] --> R
  EV[Evals and Tracing Hooks] --> R
  R --> OUT[Outputs: answers, actions, artifacts]
```

### 2B. Subgraph version

```mermaid
flowchart TB
  subgraph Agent
    P[Policy and Instructions]
    R[Reasoning Engine or LLM]
    MS[Memory Store]
    EV[Evals and Tracing Hooks]
    GA[Guardrails: schemas, allowlists, PII filters]
    AU[Autonomy: step limits, budgets, timeouts]
    TS[Tool Adapters]
  end

  subgraph Interfaces
    IN[Inputs: goals, user messages, events]
    OUT[Outputs: answers, actions, artifacts]
  end

  subgraph External
    TOOLS[APIs, DBs, Search, File IO]
    DATA[Docs, Vector DB, Knowledge Base]
  end

  IN --> P
  P --> R
  R --> MS
  R --> TS
  TS --> TOOLS
  R --> DATA
  GA --> R
  AU --> R
  EV --> R
  R --> OUT
```

---

## 3) Orchestration Patterns (from simple to complex)

```mermaid
flowchart LR
  A1[Single Agent] --> OUT1[Result]

  subgraph Crew
    C[Coordinator Agent] --> W1[Worker 1]
    C --> W2[Worker 2]
    W1 --> C
    W2 --> C
  end

  subgraph Graph
    N1[Node Retrieve] --> N2[Node Reason]
    N2 -->|tool call| N3[Node Act]
    N3 --> N4{Check Termination?}
    N4 -- No --> N2
    N4 -- Yes --> N5[Emit Result]
  end
```

---

## 4) HITL and Guardrails (who is in control)

```mermaid
sequenceDiagram
  participant U as User
  participant A as Agent
  participant G as Guardrail or Policy
  participant H as Human Reviewer
  participant T as Tool or API

  U->>A: Goal or Task
  A->>G: Validate request (policy, schema)
  G-->>A: Allowed or Blocked or Redact
  A->>T: Tool call if allowed
  T-->>A: Observation
  A->>H: HITL checkpoint (risky action)
  H-->>A: Approve or Revise or Reject
  A-->>U: Result and Trace
```

---

## 5) MCP (Model Context Protocol) as the ACI

```mermaid
flowchart LR
  A[Agent or Client] <-- MCP --> S1[MCP Server Files]
  A <-- MCP --> S2[MCP Server SQL]
  A <-- MCP --> S3[MCP Server Web]
  S1 --- ToolA[read or write files]
  S2 --- ToolB[parameterized queries]
  S3 --- ToolC[search, fetch, crawl]
```

---

## 6) Autonomy and Termination (state view)

```mermaid
stateDiagram-v2
  [*] --> Plan
  Plan --> Act
  Act --> Observe
  Observe --> Check
  Check --> Plan: Not done and within budget
  Check --> [*]: Goal met
  Check --> [*]: Budget or timeout exceeded
  Check --> [*]: No progress
```

---

## 7) Workflow vs Agent (decision helper)

```mermaid
flowchart TB
  Q{Is control flow known and stable?}
  Q -->|Yes| WF[Use a Workflow or Script]
  Q -->|No| Q2{Does the task need tool choice and iteration?}
  Q2 -->|No| WF
  Q2 -->|Yes| Q3{Are guardrails and HITL in place?}
  Q3 -->|No| PREP[Add guardrails, budgets, checkpoints]
  PREP --> Q3
  Q3 -->|Yes| AGT[Use an Agentic Loop]
```

---

## 8) Evals and Telemetry (close the loop)

```mermaid
flowchart LR
  SPEC[Task Spec and Testcases] --> RUN[Run Agent]
  RUN --> TRACE[Collect Traces and Spans]
  RUN --> METRICS[Success, Cost, Latency, Incidents]
  TRACE --> REG[Regression and Diff Tools]
  METRICS --> REG
  REG --> IMP[Refine prompts, tools, policies]
  IMP --> RUN
```

---

