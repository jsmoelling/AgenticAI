# Agentic AI Course — Week 2: OpenAI Agents SDK

**Theme:** Orchestrating multi‑agent workflows with the OpenAI Agents SDK, tracing runs, async patterns, vibe coding & debugging, tool/handoff design, and basic guardrails.  
**Labs:** Sales‑email agent (tools + handoffs), structured outputs & guardrails, and a Deep Research multi‑agent Python app.

---

## Learning goals
By the end of Week 2 you will be able to:
- Explain the **Agents / Tools / Handoffs / Guardrails** concepts and when to use each.
- Build and run a minimal Agent using the **OpenAI Agents SDK**.
- **Trace** your agent runs and interpret the trace.
- Add **tools** (Python functions) and **handoffs** (delegate to another agent).
- Use **async** to parallelize IO‑bound steps in agent workflows.
- Apply **vibe coding** techniques to iterate faster with an LLM co‑pilot.
- Integrate a simple **email tool** (SendGrid) for outbound actions.
- Produce **structured outputs** and add **guardrails** for validation.
- Compare/choose **LLM models** for your agent’s tasks.
- Assemble a small **multi‑agent** “Deep Research” app.

---

## Pre‑work (15–30 min)
- Read/skim: **OpenAI Agents SDK** overview and “Running agents” pages.  
- Open the **Tracing** page and the **Runner.run** reference for quick lookup.
- Skim the **Deep Research** cookbook to see an end‑to‑end agentic workflow.
- (Optional) Confirm a **SendGrid** free account & API key for Lab 2/3 email tool.  
- Clone **ed-donner/agents** and open the Week 2 notebooks we’ll reference.

> Tip: Set `OPENAI_API_KEY` and `SENDGRID_API_KEY` as environment variables before class.

---

## Key terminology
- **Agent** — A callable entity that takes input, reasons, and returns an output or calls a tool or **handoff**.  
- **Tool** — A function the agent may call to act in the world (e.g., send an email, search).  
- **Handoff** — An agent‑to‑agent delegation (a special kind of tool) to transfer control to a different agent with a possibly different role, tools, or system prompt.  
- **Guardrails** — Validation logic that constrains inputs/outputs (e.g., schema validation, allowlists, safety checks).  
- **Trace** — A record of the agent run: prompts, tool calls, handoffs, guardrail events, and outputs.

---

## The three core steps (SDK quickstart)
Below is a **minimal sketch** (sync form for clarity). In practice, many projects use `async`—see the next section.

```python
from openai import OpenAI
from openai_agents import Agent, Runner, tool, trace

client = OpenAI()

# 1) Create an instance of an Agent
@tool  # Example tool: simple string transform
def to_title_case(text: str) -> str:
    return text.title()

writer = Agent(
    name="Writer",
    instructions="You turn notes into polished outreach emails.",
    tools=[to_title_case],
    model="gpt-4.1-mini"  # choose an available model for your account
)

# 2) Use with trace() to track the agent
with trace("week2-quickstart") as tr:
    # 3) Call runner.run() to run the agent
    result = Runner(client).run(writer, "Draft a friendly intro for Acme Corp.")

print(result.output)           # Final string or structured object
print(tr.url if hasattr(tr, "url") else "Trace captured")  # Where supported
```

**What to look for in the trace:** prompts, tool calls (`to_title_case`), any handoffs, model generations, and the final output. Use traces to debug why an agent picked a tool or to understand **handoff** paths.

---

## Async Python primer (why & when)
Use `async` when your agent calls **network I/O** (APIs, web search, DB) or when multiple sub‑tasks can run concurrently (e.g., fetch product facts and customer info at the same time).

```python
import asyncio
from openai import AsyncOpenAI
from openai_agents import Agent, AsyncRunner

client = AsyncOpenAI()

researcher = Agent(
    name="Researcher",
    instructions="Gather facts succinctly.",
    model="gpt-4.1-mini"
)

async def main():
    runner = AsyncRunner(client)
    tasks = [
        runner.run(researcher, "Summarize company news for Acme"),
        runner.run(researcher, "Summarize product reviews for Acme")
    ]
    r1, r2 = await asyncio.gather(*tasks)
    return r1.output, r2.output

facts, reviews = asyncio.run(main())
```

**Rules of thumb**
- Keep CPU‑bound work (e.g., parsing huge CSVs) in threads/processes; keep network‑bound steps `async`.
- Design **idempotent** tools—safe to retry if a coroutine fails mid‑flight.
- Add timeouts & backoff to external calls.

---

## Vibe coding & debugging (rapid iteration)
“Vibe coding” = using an LLM co‑pilot to **sketch**, **debug**, and **refactor** quickly:
1. Start from a working slice, not a blank page.  
2. Write clear goals and constraints in comments or a system prompt.  
3. Iterate: run, observe the trace, refine the prompt/tool signature.  
4. Prefer **structured outputs** early to stabilize downstream code.  
5. Keep a **rubber‑duck** log: “what I tried → what I saw → what I’ll try next.”

**In practice for Week 2:** You’ll use vibe coding to tighten your agent prompts, tool names/descriptions, and the validation that guards them.

---

## Tools: SendGrid email (for outbound actions)
We’ll add an **email tool** to let an agent send an outreach email during Labs 2/3.

```python
import os
from sendgrid import SendGridAPIClient
from sendgrid.helpers.mail import Mail
from openai_agents import tool

SENDGRID_API_KEY = os.environ.get("SENDGRID_API_KEY")

@tool
def send_email(to: str, subject: str, body_html: str):
    """Send an HTML email via SendGrid."""
    if not SENDGRID_API_KEY:
        raise RuntimeError("SENDGRID_API_KEY missing")
    message = Mail(
        from_email=("noreply@example.com", "Sales Agent"),
        to_emails=to,
        subject=subject,
        html_content=body_html,
    )
    SendGridAPIClient(SENDGRID_API_KEY).send(message)
    return "sent"
```

**Safety notes**
- Use a test mailbox. Consider an **allowlist** guardrail to restrict recipients.
- Log but **do not** store PII in prompts/traces.
- Provide a “dry‑run” mode in development.

---

## Lab 2 — Cold outreach email agent
**Goal:** Build a simple agent system that drafts and (optionally) sends a cold outreach email.

**Agent workflow**
- **Prospector** (optional): pulls a few company facts (stub or API).  
- **Writer**: composes an email using facts and user style constraints.  
- **Sender**: a **tool** that calls SendGrid (or “dry‑run”).  
- **Handoffs:** Writer → (optionally) Sender when ready to send.

**Minimum features**
- 1 agent with a **send_email** tool (above).  
- Prompt includes tone, length, target persona, and CTA.  
- **Trace** enabled; review tool‑call reasoning.  

**Stretch**
- Add a **Prospector** agent and a **handoff** to the Writer.  
- Parameterize tone (“friendly”, “technical”, “executive brief”).

**Deliverables**
- `lab2/agent.py` implementing the workflow.  
- `lab2/README.md` with setup steps, how to run, and screenshots of the **trace**.  
- Example output email (HTML).

**Acceptance hints**
- Email includes **company‑specific** hook and a clear CTA.  
- No hallucinated features; respect token/length limits.  
- If sending is enabled, **allowlist** the recipient.

---

## Lab 3 — Models, structured outputs & guardrails
**Same use‑case as Lab 2**, with three upgrades:

1) **Different LLM models**  
Try at least two models (e.g., reasoning vs. fast) and compare quality/latency/cost.

2) **Structured Outputs**  
Return a typed object to stabilize downstream logic:

```python
from pydantic import BaseModel, constr

class OutreachEmail(BaseModel):
    subject: constr(min_length=5, max_length=120)
    body_html: str
    call_to_action: constr(min_length=3)

writer.output_type = OutreachEmail
```

3) **Guardrails**  
- Validate the **recipient** (`@example.com` allowlist).  
- Enforce **tone** choices; reject disallowed claims.  
- Add **content policy** checks before send.  
- Fail closed: on validation error, return a repair prompt rather than sending.

**Deliverables**
- `lab3/agent.py` (or notebook) with model switch, typed outputs, and guardrails.  
- A brief **comparison** note: model choice, speed, cost, quality.

---

## Lab 5 — Deep Research multi‑agent app (Python)
We’ll assemble a small multi‑agent pipeline modeled on the reference app:

**Files**
- `deep_research.py` (entry point)
- `planner_agent.py` (turns a query into a plan)
- `search_agent.py` (executes web/API searches)
- `writer_agent.py` (synthesizes findings for the audience)
- `email_agent.py` (optional: mails the report)
- `research_manager.py` (coordinates agents, merges results)

**High‑level flow**
```
User Query
   ↓
Planner ── plan → ResearchManager
                 ├─ parallel → SearchAgent (N sources, async)
                 └─ collect → Writer → (handoff) EmailAgent? → Done
```

**What to implement**
- **Planner** emits a list of tasks with sources/criteria.  
- **Manager** runs tasks **concurrently** with `AsyncRunner`.  
- **Writer** produces a structured report (`title, bullets, sources`).  
- (Optional) **EmailAgent** sends final report to a test inbox.  
- **Trace** the entire run; confirm tool calls and handoffs.

**Deliverables**
- Working CLI (e.g., `python deep_research.py "renewable H2 market 2030"`).  
- A saved report (Markdown/HTML) with **citations/sources**.  
- A short note on where async improved latency.

---

## Assessment rubric (Week 2)
| Dimension | Excellent | Satisfactory | Needs Work |
|---|---|---|---|
| Agent Design | Clear roles, correct tools, sensible handoffs | Roles present; minor mismatches | Unclear roles or missing tools |
| Tracing & Debugging | Uses trace to diagnose + improve | Trace reviewed | Trace ignored |
| Async & Performance | Parallelizes IO with timeouts & retries | Some async used | Purely sequential; fragile |
| Validation & Safety | Structured outputs + guardrails + allowlists | Basic validation | No validation; risky actions |
| Communication | Readme, comments, and outputs are clear | Mostly clear | Hard to follow |

---

## Suggested homework
- Add a **SentimentChecker** tool and vary outreach tone by target persona.  
- Expand guardrails (e.g., marketing claims allowlist + safe‑words ban).  
- Add a **feedback loop**: track opens/replies and adapt next emails.  
- Swap in a different LLM and compare traces + cost reports.

---

## Reference links
- OpenAI Agents SDK docs (overview, running agents, tracing, runner):  
  - https://openai.github.io/openai-agents-python/  
  - https://openai.github.io/openai-agents-python/running_agents/  
  - https://openai.github.io/openai-agents-python/tracing/  
  - https://openai.github.io/openai-agents-python/ref/run/
- OpenAI Cookbook — Deep Research with Agents:  
  - https://cookbook.openai.com/examples/deep_research_api/introduction_to_deep_research_api_agents
- “A Practical Guide to Building Agents” (handoffs & patterns, PDF):  
  - https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf
- Async Python primer (reference course notebooks):  
  - https://github.com/ed-donner/agents/blob/main/guides/11_async_python.ipynb
- Vibe coding & debugging notebook:  
  - https://github.com/ed-donner/agents/blob/main/guides/07_vibe_coding_and_debugging.ipynb
- Week 2 Lab 2/3 and Deep Research references:  
  - https://github.com/ed-donner/agents/blob/main/2_openai/2_lab2.ipynb  
  - https://github.com/ed-donner/agents/tree/main/2_openai/deep_research
- SendGrid (Email API): https://sendgrid.com/