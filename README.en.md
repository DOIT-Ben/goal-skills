# Goal Skills

[中文说明](README.md)

> Stop letting AI jump straight into work. Teach it to write goals first.

`goal-skills` is a lightweight skill for generating Codex `/goal` prompts.

The package name stays plural, but it exposes one public entry point: `goal-skills`.

## One-Liner

Prepare a clear goal for Codex Goal mode so the AI can keep working toward a verifiable outcome instead of only answering one prompt.

Exact behavior depends on the current Codex version and runtime. This skill generates bounded, verifiable `/goal` prompts; it does not guarantee that every platform executes them in the same way.

It works for existing projects and for early product ideas that need to become executable `/goal` prompts.

## What Is Codex Goal Mode

Codex `/goal` is an autonomous task mode: you give the AI a verifiable goal, and it typically keeps planning, acting, testing, reviewing, and iterating until the goal is complete, the budget is exhausted, or you stop it manually. Exact behavior depends on the Codex version and runtime you are using.

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
- Idea / Product Slice

Then it generates a prompt suitable for Codex `/goal`.

It is not a project-management template pack. It does not require status documents or heavyweight process files.

It also does not assume GitHub. Without GitHub, the goal can use local validation, artifacts, logs, or a summary as the delivery path. Commit, push, issues, PRs, and releases are included only when the user explicitly asks for them.

For product ideas, `goal-skills` only turns the idea into an executable smallest-product-slice goal. Deeper product clarification, judgment, planning, QA, and release handoff can optionally continue in [products-skills](https://github.com/DOIT-Ben/products-skills).

## Trigger Phrases

Trigger phrasing is intentionally narrow in both Chinese and English:

- `生成一个goal`
- `针对这个项目设置一个goal`
- `给这个项目设置一个goal`
- `为当前项目生成goal`
- `把这个目标转成goal`
- `先别改，先生成goal`
- `generate a goal`
- `set a goal for this project`
- `convert this into a goal`
- `Codex goal`
- `/goal prompt`
- `convert task to goal`
- `plan before change`

This is not a generic planning template. Trigger it when the user clearly wants a goal, a Codex `/goal`, a task-to-goal conversion, or a plan-before-change handoff.

## Install

Only Codex is verified today; Claude / Claude Code stay experimental.

```powershell
npx skills add https://github.com/DOIT-Ben/goal-skills -g -a codex -y --full-depth
```

For local development from the repository root:

```powershell
npx skills add . -g -a codex -y --full-depth
```

Experimental Claude Code install:

```powershell
npx skills add https://github.com/DOIT-Ben/goal-skills -g -a claude-code -y --full-depth
```

Claude / Claude Code support is not part of the verified compatibility surface yet. Different platforms may read `SKILL.md`, `skill.json`, or plugin metadata differently; if triggering is unstable, use `$goal-skills` explicitly or include trigger phrases such as `/goal prompt` or `Codex goal`.

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

## Package Shape

```text
goal-skills/
  README.md
  README.en.md
  SKILL.md
  skill.json
  AGENTS.md
  references/
    goal-judgment-matrix.md
    goal-template.md
    example-walkthrough.md
  examples/
    messy-project.md
    broken-app.md
    product-idea.md
    direct-repair.md
  evals/
    evals.json
```
