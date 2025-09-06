# Lab 1 — Visualization (Planner → Solver)

This shows the flow of **Week 1 Lab 1**: a tiny agent pattern where a *planner* generates a logic puzzle and a *solver* answers it. Copy‑paste these diagrams directly into a GitHub README or the top of your notebook.

---

## 1) Flowchart overview

```mermaid
flowchart TD
  A[Setup: load .env and check OPENAI_API_KEY] --> B[Client: OpenAI(timeout=20.0)]
  B --> C[Helper: respond(prompt, model, system, temperature, max_output_tokens)]
  C --> D[Smoke test: '2+2']
  D --> E[Planner: generate a challenging logic puzzle (question only)]
  E --> F[Solver: answer concisely (final answer + brief justification)]
  F --> G[Display: render Markdown with Question and Answer]

  %% tunables
  E -. adjust temperature for creativity .- E
  C -. adjust max_output_tokens for brevity .- F
```

---

## 2) Message flow (sequence)

```mermaid
sequenceDiagram
  autonumber
  participant Student
  participant Notebook
  participant OpenAI as OpenAI Client

  Student->>Notebook: Run Lab 1
  Note over Notebook: Setup: load .env, verify OPENAI_API_KEY

  Notebook->>OpenAI: responses.create("What is 2+2?")
  OpenAI-->>Notebook: "4" (smoke test)

  Note over Notebook: Planner step
  Notebook->>OpenAI: responses.create("Create one challenging logic puzzle...")
  OpenAI-->>Notebook: Puzzle text (question only)

  Note over Notebook: Solver step
  Notebook->>OpenAI: responses.create(puzzle, system="Concise answer; no CoT")
  OpenAI-->>Notebook: Final answer + brief justification

  Notebook-->>Student: Render Markdown (Question + Answer)
```

---

## 3) How to tweak behavior
- **Creativity**: raise/lower `temperature` in the planner call (e.g., 0.4–0.8).  
- **Brevity**: constrain `max_output_tokens` for both planner and solver.  
- **Style**: adjust the solver `system` message to control tone and formatting.  
- **Fallback**: if needed, swap the helper to Chat Completions (`respond_chat`) with the same prompts.

---

### GitHub rendering tips
- Keep the triple‑backtick fences exactly as shown: <code>```mermaid</code> … <code>```</code>  
- Avoid stray indentation before the fence.  
- If a Mermaid block fails to render, validate there are **no extra backticks** or unclosed blocks above it.