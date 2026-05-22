# Goal Skills

[中文说明](README.md)

> Stop letting AI jump straight into work. Teach it to write goals first.

`goal-skills` is a lightweight skill for generating project-aware Codex `/goal` prompts.

It exists for one job: inspect a project, directory, problem, or rough target, choose the right goal direction, and produce a bounded, verifiable `/goal`.

## What is a goal

A goal is a clear statement of what this round should accomplish.

It is not just “fix it.” It should say:

- what to fix
- what counts as done
- what to inspect before changing things
- what is explicitly out of scope

## Why goals matter

AI often wants to start working immediately. That sounds productive, but it can waste time if the direction is fuzzy. A goal helps keep the work bounded, reduces rework, and makes the next step easier to verify.

## Trigger Phrases

The Chinese trigger phrasing is intentionally narrow:

- `生成一个goal`
- `针对这个项目设置一个goal`
- `给这个项目设置一个goal`
- `为当前项目生成goal`
- `把这个目标转成goal`
- `先别改，先生成goal`

## What It Does

It selects among common goal directions:

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

It is not a project-management template pack. It does not require status documents or heavyweight process files.

## Structure

```text
goal-skills/
  SKILL.md
  references/
    goal-judgment-matrix.md
    goal-template.md
```

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
