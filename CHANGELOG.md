# Changelog

## 1.1.7 - 2026-05-23

- Clarified that read-only whole-project analysis is orientation only, not completion, unless the generated goal explicitly says read-only only.
- Required agents to execute the safest highest-value next step they identify, instead of handing back a roadmap or "suggested minimum next step".
- Added eval coverage for the failure mode where a long-horizon goal stops after read-only analysis with recommendations such as startup consistency or doctor warning fixes.

## 1.1.6 - 2026-05-23

- Clarified that analysis reports, dev records, roadmap documents, codebase maps, and index updates are intermediate artifacts, not completion.
- Required agents to continue from analysis into the highest-value safe finding instead of stopping after documentation-only work.
- Added eval coverage for the failure mode where a long-horizon goal stops after a first-round system analysis document.

## 1.1.5 - 2026-05-23

- Clarified that commit/push/deploy/release are normal continuation steps when the generated goal explicitly authorizes them and access exists.
- Removed routine commit/push and authorized delivery writes from default stop conditions.
- Reframed stop conditions around true blockers: actions outside the goal, destructive or irreversible operations, credential handling, unauthorized external writes, scope conflicts, or repeated validation failure.
- Added eval coverage for the failure mode where an agent stops before authorized commit/push instead of executing the goal.

## 1.1.4 - 2026-05-23

- Removed "phase complete" as a stop signal. Completion of Phase 1, a minimum loop, or a next-phase recommendation now triggers continuation, not termination.
- Clarified that only real blockers, safety issues, missing judgment, or explicit user-authorized handoffs can stop a long-horizon goal.
- Added eval coverage for the "10 minutes and stop" failure mode where the goal incorrectly ends after a minimum closed loop.

## 1.1.3 - 2026-05-23

- Tightened output rules so `$goal-skills 分析整个项目` produces a copy-pasteable long-horizon prompt instead of a long project audit report.
- Added an explicit output contract: at most one sentence or 3 short evidence bullets outside the prompt.
- Added orientation-pass guidance: inspect just enough project evidence to shape the prompt, and put deep graph/test/browser/audit work inside the generated prompt.
- Added anti-patterns and eval coverage for overlong audit-style outputs with graph/test/git logs and project ratings.

## 1.1.2 - 2026-05-23

- Reframed final output from a short executable goal document into a copy-pasteable long-horizon autonomous execution prompt.
- Added required prompt opening patterns: Chinese prompts begin with `现在给你一个目标：...`; English prompts begin with `I am giving you a goal: ...`.
- Added explicit "do not stop until complete" behavior while preserving stop conditions for destructive, credential, global, external-write, deployment, commit/push, and database actions.
- Added long-term execution map requirements covering project ultimate purpose, current stage, near-term goals, mid-term direction, long-term possibilities, and non-goals.
- Updated templates, README files, metadata, and eval coverage for long-running project-purpose analysis.

## 1.1.1 - 2026-05-22

- Repositioned the package from Codex-centered wording to a portable goal compiler for all AI agents.
- Repositioned the package as a goal compiler instead of a prompt helper.
- Made the current conversation the first input source for goals, before project files.
- Changed default behavior to generate one preferred executable `/goal` unless conflict, risk, or missing critical input requires candidates.
- Reframed the quality bar around a minimum complete loop rather than merely a small task slice.
- Expanded references into a fuller goal compiler manual with conversation extraction, decision algorithm, confidence levels, field writing rules, validation ladder, stop-condition matrix, anti-patterns, and multi-scenario walkthroughs.
- Expanded public examples from 4 to 13 files, covering all goal types plus high-risk external-write boundaries.
- Added eval coverage for automation dry run, authorized release/publish, and high-risk production/database boundaries.
- Updated README, metadata, examples, and evals to cover conversation-first, one-step goal compilation, and reference-depth behavior.

## 1.1.0 - 2026-05-22

- Expanded README guidance to explain what happens after starting a `/goal` and what goals are useful for.
- Promoted package metadata back to the 1.x line to avoid version rollback after earlier public 1.x exposure.
- Added bad-output anti-patterns to public examples.
- Added `allowed_include_any` eval fields to reduce brittle string matching across Chinese and English outputs.

## 0.2.2 - 2026-05-22

- Added language-matched Chinese and English candidate/final goal templates.
- Renamed the product idea goal type to `Idea / Product Slice` and narrowed product-methodology overlap.
- Added standalone public examples for messy projects, broken apps, product ideas, and direct repair goals.
- Added machine-checkable eval fields such as `expected_mode`, `must_include`, and `must_not_include`.
- Clarified Codex Goal behavior depends on the current Codex version and runtime.
- Documented experimental Claude Code installation and trigger caveats.

## 0.2.1 - 2026-05-22

- Added intent-style English triggers for Codex goal and task-to-goal requests.
- Tightened execution handoff rules so the skill remains goal-first.
- Clarified reference loading rules, lightweight candidate output, fallback handling, and final goal acceptance checks.
- Expanded external-write boundaries beyond GitHub.
- Added regression eval prompts for trigger, safety, fallback, and product-idea behavior.

## 0.2.0 - 2026-05-22

- Reset package version to pre-1.0 while the public API and install surfaces stabilize.
- Limited verified platform claims to Codex and marked Claude / Claude Code support as experimental.
- Added English trigger phrases for the bilingual README promise.
- Added a worked example and minimal GitHub Actions discovery check.
- Expanded scope beyond existing projects to include idea-to-product goals.
- Added Product / Idea To Shipped Product routing guidance.
- Referenced products-skills as the follow-up workflow for product clarification, planning, QA, and release handoff.

## 0.1.1 - 2026-05-22

- Clarified that GitHub delivery is optional and must not be assumed.
- Added default operation boundaries for destructive, credential, global, and external-write actions.
- Expanded the goal template with delivery surface, safety constraints, and stop conditions.

## 0.1.0 - 2026-05-22

- Initial public release.
- Exposes one main skill, `goal-skills`.
- Adds a compact judgment matrix and final `/goal` template.
- Keeps trigger phrasing focused on generating a goal.
