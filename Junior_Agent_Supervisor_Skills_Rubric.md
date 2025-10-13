# Junior “Agent‑Supervisor” Skills Rubric (Use for hiring, upskilling, and reviews)

**How to read:** Each competency has level descriptors. Target **Working** within 60 days; aim for **Advanced** within 6–9 months.

| Competency | Foundational | Working (Target) | Advanced |
|---|---|---|---|
| **Task Decomposition** | Breaks a feature into steps with guidance. | Writes a clear change plan (scope, risks, acceptance criteria). | Anticipates cross‑repo impacts; sequences multi‑PR epics. |
| **Test‑First Thinking** | Runs existing tests; understands pass/fail. | Adds/updates unit tests from requirements before code changes. | Designs golden tests & property‑based tests; increases coverage strategically. |
| **Prompting for Code Changes** | Uses templates to ask an agent for small edits. | Writes precise, policy‑aware prompts (PII, licensing, style) tied to tests. | Chains prompts across files; constrains agents to safe ops; documents limitations. |
| **Agent Orchestration** | Executes agent suggestions in IDE. | Uses agent tools (search, run, diff, revert); monitors/aborts bad runs. | Plans multi‑step refactors/migrations; schedules agent jobs, reviews telemetry. |
| **Code Review & Security** | Comments on style and obvious bugs. | Enforces branch rules; checks SAST/SCA; rejects unverifiable diffs. | Identifies design weaknesses; ensures threat‑model and compliance notes in PR. |
| **Repo Hygiene & Tooling** | Navigates branches; basic Git. | Uses PR templates, conventional commits, and CI status gates. | Authors pre‑commit hooks; optimizes CI (caching, flaky‑test triage). |
| **Data/Context Prep** | Locates specs and relevant files. | Curates minimal context windows (paths, snippets, APIs) for the agent. | Builds reusable context packs; automates retrieval (RAG) for large repos. |
| **Troubleshooting & Debugging** | Reproduces issues locally. | Uses logs, stack traces, and failing tests to drive agent fixes. | Minimizes MTTR with bisecting, sandbox runs, and targeted instrumentation. |
| **SDLC Integration** | Follows team ceremonies. | Aligns work to tickets; links PRs; updates change logs and docs. | Defines working agreements; improves Definition of Done with agent steps. |
| **Ethics & Governance** | Knows basic policy. | Applies redaction/licensing rules; records model/tool use in PR. | Proposes policy updates; runs post‑mortems on agent incidents. |
| **Communication & Docs** | Summarizes progress in stand‑ups. | Writes speclets/PR summaries anyone can follow. | Communicates trade‑offs to stakeholders; mentors peers. |
| **Measurement & Analytics** | Reads dashboard metrics. | Tracks own PR cycle time and defect rates; reports improvements. | Designs team scorecards; correlates metrics to business outcomes. |

## Evaluation Guide
- **Evidence**: Link to PRs with agent-generated diffs/tests, before→after metrics, and post‑mortems.  
- **Calibration**: Use shadow reviews by seniors for first 4–6 weeks.  
- **Progression**: Promotion to *Reviewer* requires 3+ multi‑file PRs with measurable improvements and clean audit trails.

## Suggested Learning Path (6 weeks)
- Weeks 1–2: Playbook basics, Git + CI guardrails, test-first prompts.  
- Weeks 3–4: Multi-file refactors, security checks, telemetry reading.  
- Weeks 5–6: Epic decomposition, golden tests, lightweight post‑mortems.
