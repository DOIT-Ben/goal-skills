# Goal Judgment Matrix

Use this reference when the project can go in multiple directions.

This file is the decision engine for `goal-skills`. It should help the agent decide what goal to compile, when to ask, and how to avoid being pulled away from the user's latest intent by noisy project files.

## Input Priority

1. Current conversation: user goal, latest corrections, constraints, preferences, confirmed facts, implicit deliverables, requested delivery style, and explicit exclusions.
2. Project rules and visible evidence: `AGENTS.md`, README, docs, package files, source, scripts, logs, outputs, and git state when relevant.
3. External assumptions: only when the user requested them or when they are needed for safe validation.

If project evidence appears to contradict the current conversation, surface the conflict instead of silently replacing the user's stated intent.

## Conversation Extraction

Before reading files, extract a compact working brief from the current chat:

| Field | What to capture | Example |
| --- | --- | --- |
| Goal | The result the user wants now | "把这个工具定位成 goal compiler" |
| Constraints | Hard limits, exclusions, authorized actions | "不改代码、不提交、不推送" |
| Preferences | Style, language, detail level, workflow taste | "中文为主", "一步到位" |
| Confirmed facts | Things the user says are already decided | "只做本地文档和示例更新" |
| Implicit deliverables | Outputs implied by the request | updated README, references, evals, local install |
| Risk signals | Production, secrets, external writes, deletion, bulk operations | deploy, database, tokens, publish |
| Latest correction | The newest framing that overrides older assumptions | "不是 prompt helper，是 goal compiler" |

Do not treat "current project" as a blank check to inspect everything. Use the extracted brief to decide which files and evidence matter.

## Decision Algorithm

Use this order:

1. Extract the conversation brief.
2. Identify hard exclusions and authorization boundaries.
3. Read project rules if a repo or package is involved.
4. Inspect only evidence that can refine the goal type, scope, validation, or delivery.
5. Choose the goal type that removes the biggest blocker while honoring the conversation brief.
6. Decide whether the goal can be compiled now.
7. Output one final executable goal unless the ask/stop conditions below require candidates or a question.

Default path:

```text
conversation brief -> relevant evidence -> one preferred executable goal
```

Exception path:

```text
conflict / high risk / missing critical input / explicit option comparison -> candidate directions or one concise question
```

## Ask Or Compile

Compile a final goal when:

- the user intent is clear enough to define one observable outcome
- non-goals and risk boundaries can be inferred safely
- validation can be discovered as part of the goal
- missing details do not block a useful first loop

Ask or return candidates when:

- the user asks to compare directions
- two goals would produce incompatible deliverables
- a required action is destructive, irreversible, credential-related, or an external write
- the target conflicts with project rules or visible evidence
- the output format, destination, or authority is unclear and guessing would create work the user may reject

Prefer a final goal with a stop condition over asking for every unknown. Unknown validation command is not a blocker; include "discover validation path" as step 1.

## Confidence Levels

Use confidence to decide how much discovery belongs inside the goal.

| Confidence | Signals | Output behavior |
| --- | --- | --- |
| High | User gave target, scope, exclusions, and delivery; evidence is consistent | Final goal directly |
| Medium | Target is clear, but entry points or validation are unknown | Final goal with discovery as early step |
| Low | Project purpose, target, or delivery is unclear | Discovery-first final goal, or candidates if directions conflict |
| Blocked | Critical authority, destination, secret, destructive action, or external write is required | Ask before compiling or include explicit stop condition |

## Project State Signals

| Signal | Likely state | Better first goal |
| --- | --- | --- |
| No README, unclear folders, mixed artifacts | Unknown / messy | Discover / Map |
| Notes only, no runnable artifact | Idea / empty | Create From Zero |
| Requirements unclear, sources missing | Draft / unclear | Research / Spec |
| Existing app/tool works and user wants a new capability | Growing | Add / Extend |
| Build/start/test/export fails | Broken | Repair / Restore |
| Output works but structure is tangled | Hard to change | Refactor / Reorganize |
| Core flow works but rough UX/docs/errors | Working but rough | Polish / Improve |
| Repeated manual steps | Repetition | Automate |
| Competing solutions or metrics needed | Decision needed | Experiment / Evaluate |
| Needs deploy/export/submit/handoff | Delivery needed | Release / Publish |
| Product idea, feature concept, user problem, or market opportunity | Product idea | Idea / Product Slice |

## Goal Type Deep Matrix

| Goal type | Use when | Goal shape | Validation | Common wrong turn |
| --- | --- | --- | --- | --- |
| Discover / Map | Purpose, entry, or baseline is unclear | read-only project map and next-goal recommendations | files inspected, commands listed, risks summarized | editing before understanding |
| Research / Spec | outcome or evidence rules are unclear | evidence-backed spec, decision memo, or requirements | source list, assumptions, open questions | building before requirements exist |
| Create From Zero | user wants first usable artifact | smallest usable artifact plus local check | artifact opens/runs/exports and is summarized | overbuilding a full product |
| Add / Extend | base works and one capability is requested | one new capability integrated into existing pattern | focused tests/build/smoke path | refactoring unrelated code |
| Repair / Restore | expected path fails | restore a broken run/build/test/export path | reproduce or inspect failure, fix, verify pass | adding features around broken baseline |
| Refactor / Reorganize | structure blocks change but behavior should remain | local structural improvement with behavior preservation | before/after tests or manual equivalence checks | redesigning product behavior |
| Polish / Improve | core works but quality is rough | UX/docs/errors/robustness polish | screenshots, lint, docs review, smoke checks | changing scope or adding features |
| Automate | repeated manual workflow exists | script/command/workflow with dry run and logs | sample input/output, dry run, failure path | unsafe batch writes |
| Experiment / Evaluate | options need evidence | comparison with metric, table, and conclusion | reproducible command, result artifact | choosing without metric |
| Release / Publish | user needs delivery to an audience/system | verified handoff, export, deploy, submit, or archive | final URL/path, health check, visibility | publishing without authority |
| Idea / Product Slice | concept has no implementation plan | target user, problem, core loop, smallest slice, evidence gate | slice spec, prototype/doc, success gate | turning idea into full SaaS |

## Direction Rules

- Default to one preferred executable goal, not an analysis-only handoff.
- Ask for direction confirmation only when the target is conflicting, high-risk, blocked by missing critical input, or explicitly framed as an option comparison.
- A goal should complete a minimum complete loop: inspect or research, implement or compose, validate, and summarize the deliverable result.
- Repair before extension when the baseline is broken.
- Discover before refactor when the structure is not understood.
- Spec before creation when the outcome is ambiguous.
- Add / Extend only when there is a base to extend.
- Polish only after the core path exists.
- Automate only after the manual workflow is understood.
- Release only after the deliverable and validation path are clear.
- Product ideas should not be forced into an existing-project frame. Start by clarifying the smallest useful product slice, user, problem, success criteria, and evidence gate.
- When the task is clearly product-oriented, keep `goal-skills` limited to turning the idea into a bounded executable goal instead of expanding it into a full product process.

## Conflict Handling

When signals disagree, resolve in this order:

1. Explicit current user instruction.
2. Platform/tool safety limits.
3. Project `AGENTS.md` or equivalent repo rules.
4. Existing codebase patterns.
5. General best practice.

Examples:

- User says "不提交不推送", repo has GitHub remote: exclude commit/push.
- User says "只做文档", source code has obvious bug: do not fix code; mention as non-goal or residual risk.
- User asks to deploy, but no deployment authority is confirmed: include deploy prep and stop before external write.
- README says old product name, user gives new positioning: update goal around the new positioning and mention README drift as evidence to reconcile.

## Domain Adaptation

Software / code:

- include entry points, changed modules, tests, build, lint/typecheck, smoke check, and fallback when commands are unknown.

Frontend / UI:

- include target workflow, responsive states, screenshots/browser check, accessibility basics, and no unrelated redesign.

Automation:

- include input, trigger, output, dry run, idempotency, logs, rollback or failure handling.

Research / writing:

- include source range, evidence rules, outline, claim/evidence separation, uncertainty, and citation or link expectations.

Data / experiment:

- include input dataset, baseline, metric, command/log path, result table, reproducibility, and leakage or sampling risks when relevant.

Media / design:

- include source assets, target format, dimensions, quality checklist, export/review steps, and rights/asset boundaries.

Deployment / delivery:

- include env vars without secrets, build/start/export, health check, rollback, handoff notes, final URL/path, and visibility confirmation.

Product idea:

- include target user, problem, core workflow, smallest useful slice, evidence needed, success gate, and next product stage.

## Delivery Surface

Do not assume GitHub.

| User/project context | Delivery surface |
| --- | --- |
| No git repository | local artifact, summary, generated files, logs, manual checklist |
| Git repository exists, no publish request | local validation and summary; no commit/push by default |
| User asks to commit | inspect dirty tree, stage intentional files, commit with clear scope |
| User asks to push / PR / issues | verify remote, branch, auth, target repo, visibility, and final URL |
| GitHub unavailable | local patch summary, zip/handoff package, or manual command checklist |

Apply the same principle to every external system: databases, cloud storage, deployments, package registries, email, messaging tools, payment systems, public websites, and third-party APIs.

## Operation Boundary Defaults

- No destructive commands without explicit approval.
- No deleting, overwriting, or bulk-moving core files without explicit approval.
- No credential exposure or hard-coded secrets.
- No unrelated refactor, dependency swap, redesign, or cleanup.
- No global machine, account, service, or scheduled-task changes unless the goal explicitly requires it.
- Stop and ask before irreversible actions or external writes.

## Output Selection Checklist

Before writing the final response:

- Did the goal preserve the latest user correction?
- Is there exactly one primary outcome?
- Is the delivery path local unless external write was requested?
- Does the goal include non-goals that block the most tempting scope creep?
- Does the goal include a validation path or a step to discover it?
- Does the goal close the loop with a summary of changes, validation, risk, and next action?
