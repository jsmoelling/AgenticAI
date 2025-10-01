# Lab 5 — Sidekick: LangGraph Diagram 

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

# Lab 5 — Sidekick: Gradio UI Diagram

---

## Component Tree (Layout)

```mermaid
flowchart TD
  A[Blocks title Sidekick] --> M[Markdown heading Sidekick Personal Co-Worker]
  A --> S[State sidekick delete_callback free_resources]
  A --> R1[Row]
  R1 --> C[Chatbot label Sidekick height 300 type messages]

  A --> G[Group]
  G --> R2[Row]
  R2 --> T1[Textbox name message placeholder Your request...]
  G --> R3[Row]
  R3 --> T2[Textbox name success_criteria placeholder What are your success criteria?]

  A --> R4[Row]
  R4 --> B1[Button Reset variant stop]
  R4 --> B2[Button Go! variant primary]
```

---

## Event Flow and Handlers

```mermaid
flowchart LR
  subgraph App lifecycle
    UI[ui.load] -->|calls| setup
    setup -->|returns| sidekick_instance
    sidekick_instance -->|stored in| S[(gr.State)]
  end

  subgraph User input to processing
    T1[message.submit] -->|args S message success_criteria chatbot| process_message
    T2[success_criteria.submit] -->|same args| process_message
    B2[Go!.click] -->|same args| process_message
    process_message -->|returns chatbot and sidekick| C[Chatbot updates] & S
  end

  subgraph Reset flow
    B1[Reset.click] --> reset
    reset -->|returns empty inputs and new sidekick| T1clear[clear message]
    reset --> T2clear[clear success_criteria]
    reset --> Cclear[clear chatbot]
    reset --> Supd[update State with new Sidekick]
  end

  subgraph Cleanup
    S -. on delete .-> free_resources
  end

  UI --> launch[ui.launch inbrowser True]
```

---

## Notes
- Chatbot shows the conversation history your agent produces.
- Both textboxes trigger `process_message` on submit; the Go button calls the same handler.
- Reset reinitializes the Sidekick, clears inputs and Chatbot, and updates the stored state.
- `free_resources` runs when the stored state is deleted.





