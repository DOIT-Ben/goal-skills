# Goal Skills

[中文说明](README.md)

> Stop letting AI jump straight into work. Teach it to write goals first.

`goal-skills` is a lightweight skill for generating Codex `/goal` prompts.

## One-Liner

Prepare a clear goal for Codex Goal mode so the AI can keep working toward a verifiable outcome instead of only answering one prompt.

It works for existing projects and for early product ideas that need to become executable `/goal` prompts.

## What Is Codex Goal Mode

Codex `/goal` is an autonomous task mode: you give the AI a verifiable goal, and it keeps planning, acting, testing, reviewing, and iterating until the goal is complete, the budget is exhausted, or you stop it manually.

So a goal is not just “fix this.” It is closer to a task brief:

- what outcome to reach
- what scope can be changed
- what should stay out of scope
- how completion should be verified
- when the AI should stop and ask the user

## Why Generate a Goal First

AI often wants to start working immediately. That sounds productive, but it can waste time when the direction is fuzzy.

Generating a goal first clarifies direction, scope, validation, and stop conditions before Codex enters `/goal`. That makes the AI behave more like a persistent worker and less like a patch machine guessing its way through the project.

Persistent work does not mean “anything goes.” A good goal also defines operation boundaries: do not delete core files, do not change global settings, do not expose secrets, do not assume commit/push, and stop before high-risk actions.

## What This Skill Does

`goal-skills` chooses the most suitable goal direction:

- Discover / Map
- Research / Spec
- Create From Zero
- Add / Extend
- Repair / Restore
- Refactor / Reorganize
- Polish / Improve
- Automate
- Experiment / Evaluate
- Release / Publish
- Product / Idea To Shipped Product

Then it generates a prompt suitable for Codex `/goal`.

It is not a project-management template pack. It does not require status documents or heavyweight process files.

It also does not assume GitHub. Without GitHub, the goal can use local validation, artifacts, logs, or a summary as the delivery path. Commit, push, issues, PRs, and releases are included only when the user explicitly asks for them.

For product ideas, `goal-skills` can generate an idea-to-delivery goal and recommend [products-skills](https://github.com/DOIT-Ben/products-skills) for follow-up product clarification, judgment, planning, QA, and release handoff.

## Trigger Phrases

The Chinese trigger phrasing is intentionally narrow:

- `生成一个goal`
- `针对这个项目设置一个goal`
- `给这个项目设置一个goal`
- `为当前项目生成goal`
- `把这个目标转成goal`
- `先别改，先生成goal`

## Install

```powershell
npx skills add https://github.com/DOIT-Ben/goal-skills -g -a codex claude-code -y --full-depth
```

For local development from the repository root:

```powershell
npx skills add . -g -a codex claude-code -y --full-depth
```

## Typical Use

```text
$goal-skills

先别改，先生成goal。
请判断当前项目最适合的下一步 goal 类型，并给出 2-3 个候选方向。
```

Then:

```text
$goal-skills

选 A。请生成最终可执行 /goal。
```
