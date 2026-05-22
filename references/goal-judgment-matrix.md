# Goal Judgment Matrix

Use this reference when the project can go in multiple directions.

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

## Direction Rules

- Repair before extension when the baseline is broken.
- Discover before refactor when the structure is not understood.
- Spec before creation when the outcome is ambiguous.
- Add / Extend only when there is a base to extend.
- Polish only after the core path exists.
- Automate only after the manual workflow is understood.
- Release only after the deliverable and validation path are clear.

## Domain Adaptation

Software:
- include build, test, lint/typecheck, smoke test, and changed modules.

Automation:
- include input, trigger, output, dry run, rollback or failure handling.

Research / writing:
- include sources, evidence rules, outline, claim/evidence separation, missing information.

Data / experiment:
- include dataset/input, baseline, metric, command/log path, result table.

Media / design:
- include source assets, target format, quality checklist, export/review steps.

Deployment / delivery:
- include env vars without secrets, build/start/export, health check, rollback, handoff notes.
