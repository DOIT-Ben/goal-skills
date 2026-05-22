# Changelog

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
