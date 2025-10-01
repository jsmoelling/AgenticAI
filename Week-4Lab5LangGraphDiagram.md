# Lab 5 — Sidekick: LangGraph Diagram 

This version fixes Mermaid syntax so it renders on GitHub.

## Main Flow

```mermaid
flowchart TD
    Start([START]) --> Worker[worker]
    Worker -->|tool calls present| Tools[tools]
    Worker -->|no tool calls| Evaluator[evaluator]
    Tools --> Worker
    Evaluator -->|criteria met or user input needed| End([END])
    Evaluator -->|continue working| Worker
```

## Checkpointing (Memory)

```mermaid
flowchart LR
    MemorySaver[(MemorySaver)]
    Worker[worker]
    Tools[tools]
    Evaluator[evaluator]

    MemorySaver -. persists .- Worker
    MemorySaver -. persists .- Tools
    MemorySaver -. persists .- Evaluator
```

## Notes
- **worker**: primary reasoning node bound to tools; may produce tool calls.
- **tools**: runs any tool calls created by the worker, then returns to worker.
- **evaluator**: provides feedback and decides whether to finish or loop.
- **MemorySaver**: persists state between steps using a thread ID.
