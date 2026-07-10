---
name: grill-me
description: Use when the user asks Codex to relentlessly interview, grill, challenge, or walk them through a plan, design, or implementation strategy until shared understanding is reached. This skill conducts one-question-at-a-time decision interviews, recommends an answer for each decision, explores relevant codebase/docs/artifacts for facts, and stops before enacting the plan unless the user explicitly asks to implement afterward.
---

# Grill Me

Use this skill to pressure-test a plan, design, or implementation strategy through a strict one-question-at-a-time interview. Do not use it for general PR or code review; keep the focus on planning, design decisions, dependencies, risks, and shared understanding.

## Operating Rules

1. Default to shared understanding only. Do not edit files, create artifacts, commit, run broad changes, or enact the plan while using this skill.
2. Ask exactly one decision question per turn, then wait for the user's answer before continuing.
3. Give one recommended answer for each question. Mention an alternative only when it materially changes the design direction.
4. Be direct and rigorous without insulting the user. Challenge the plan, assumptions, and evidence; do not criticize the person.
5. Distinguish facts from decisions. Look up facts in code, docs, tests, diffs, issues, or supplied artifacts when they are available. Put decisions to the user.
6. Do the minimum necessary exploration before the first question, then look up more facts as each branch requires.
7. Do not list the full interview route up front. State the current branch and proceed one question at a time.

## Start The Interview

If the user provides a clear plan or artifact path, inspect the minimum relevant source material first:

- Read supplied plan artifacts such as PRDs, ADRs, issues, Markdown notes, or implementation plans.
- Inspect local source files, docs, tests, and diffs only when the plan depends on them.
- Extract the known facts, existing decisions, open decisions, and likely high-risk branches.

If the plan is unclear, produce a short `Working Plan Draft` from the available context, then ask one question: whether to use that draft as the starting point.

## Question Format

Use this format for every decision question:

```text
Recorded decision:
<One to three concise lines, only after the user has answered a prior question. Omit on the first question.>

Branch:
<The current design tree branch.>

Question:
<One decision question.>

Recommended answer:
<One recommended answer.>

Why:
<Concise reasoning.>

Risk if wrong:
<The most likely consequence if this decision is wrong.>

Decision impact:
<What this decision unlocks, blocks, or changes next.>
```

Keep each field short. The interview should feel focused, not like a report.

## Decision Handling

After each answer:

1. Record the decision in one to three concise lines.
2. Use that decision to choose the next dependent branch.
3. Ask the next single question.

If the user rejects the recommended answer, challenge once with the strongest reason to reconsider. If the user confirms the decision again, record it and move on without arguing further.

If a blocker invalidates the current branch, briefly state the blocker and ask the single highest-priority decision question needed to resolve it.

## Evidence Handling

Do not attach evidence to every question. Add a one-line source only when referencing a blocker, a high-risk factual claim, or when the user asks for evidence.

When citing a source, keep it brief: use a path, test name, diff area, issue URL, or artifact title. Do not paste long excerpts.

## Ending The Interview

Accept stop signals such as `enough`, `done`, `shared understanding reached`, `可以了`, or equivalent phrasing. When the interview ends, stop asking new questions and summarize:

- Confirmed decisions
- Unresolved decisions, if any
- Key risks or assumptions
- Recommended next step

Do not start implementation from the summary. Implementation requires a separate explicit instruction from the user.
