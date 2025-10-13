# AI Coding Agents: One‑Pager for Leadership (Q4 2025)

## Executive Summary
Agentic coding has crossed from pilots to production. Teams that pair developers with governed coding agents (IDE-integrated, repo‑aware, policy‑aware) are shipping faster with fewer regressions. The skills premium is shifting from “just code” to **directing the work**: decomposition, test‑first change plans, guardrails, and code review. Entry‑level roles are evolving into **agent‑supervisor apprenticeships**.

## Why This Matters Now
- **Adoption inflection:** Enterprise IDEs ship agent capabilities (multi‑file edits, PRs, terminal runs) with auditing and team policies.
- **Backlog compression:** Migrations, refactors, and test coverage work become tractable.
- **Risk moves upstream:** The failure mode is no longer “can we code it” but “did we specify, test, and govern it.”

## What “Good” Looks Like This Quarter
- Standardized **Agent Playbook** (prompts, test plans, repo rules, licensing/PII hints).
- **Guardrails:** branch protections, CI checks, dependency/licensing scans, model-use policy.
- **Agent-in-the-loop SDLC:** agents propose diffs + tests; humans gate merges.
- **Metrics:** beyond acceptance rate; measure time‑to‑green, escaped defect rate, and security findings.

## Risks & Mitigations
- **Spec drift / hallucination →** demand test-first change plans and golden tests.
- **Policy breaches (PII, licenses) →** pre‑commit hooks + CI license/SCA + repo “team rules.”
- **Skill gap →** targeted upskilling: juniors as *agent leads*; seniors as reviewers/governors.

## Near‑Term Metrics (report monthly)
- Cycle time to green (PR open→CI pass→merge)
- % PRs with agent‑generated tests accepted
- Escaped defects / 1k merged LOC
- Security findings trend (SAST/SCA)
- Backlog burn on migration/refactor epics

## 30‑60‑90 Day Plan
**Days 0‑30 (Pilot):** pick 1–2 repos (low blast radius); define Playbook; wire CI guardrails; baseline metrics.  
**Days 31‑60 (Expand):** add repo‑wide refactor/migration epics; instrument analytics; start junior agent‑supervisor cohort.  
**Days 61‑90 (Standardize):** roll out Playbook org‑wide; publish metrics; formalize apprentice→reviewer progression.

## Investment
- Tools: IDE agents + CI security checks (SAST/SCA) + policy templates.
- People: 1 enablement lead, 2–3 repo champions, a junior cohort (6–10) in supervised rotations.
- Governance: lightweight review board to evolve the Playbook and metrics.
