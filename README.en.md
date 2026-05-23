# Goal Skills

> If your token budget is roomy enough, come play goal.
> Let the agent do more than talk.

[中文说明](README.md)

`goal-skills` is a long-horizon goal compiler for AI agents: it extracts the real intent, ultimate project purpose, boundaries, and validation standards from the current conversation, project state, or product idea, then generates a copy-pasteable prompt an AI agent can keep executing until the larger goal is complete.

The package name stays plural, but it exposes one public entry point: `goal-skills`.

## One-Liner

Compile the user's latest goal, constraints, preferences, and implicit deliverables into a prompt that starts with a concrete target and keeps an AI executing, validating, and continuing until completion or a stop condition.

Different agents have different execution mechanisms. This skill writes bounded, verifiable, portable long-horizon prompts instead of binding the workflow to one platform.

It works for existing projects and for early product ideas that need to become executable long-horizon agent prompts.

## What Is An Agent Goal

An agent goal is a long-horizon execution prompt for AI agents: you give the AI a verifiable target, and it can plan, act, test, review, and iterate until the target is complete, a stop condition is reached, the budget is exhausted, or you stop it manually. In Codex it can be used as `/goal`; in Claude Code, Cursor, Devin, custom agents, or other tools that can read Markdown/rules, it can be used as a task brief or execution constraint.

So a goal is not just “fix this.” It is closer to a long-running task brief:

- what target to start with
- what scope can be changed
- what should stay out of scope
- how the AI should analyze the user's intent and project state first
- how the AI should keep looping through observe / decide / act / validate / review / continue
- when the AI should stop and ask the user

## What Happens After You Start A Goal

After you hand a goal to an agent, the agent is no longer only answering one prompt. It keeps working around the goal you gave it. This skill helps define that work before it starts:

- what to analyze first
- what can and cannot be changed
- what commands, artifacts, or evidence should verify completion
- when the AI should keep going and when it should stop and ask you

The value of a goal is not pretty wording. The value is turning a fuzzy request into long-running, verifiable work that can actually mature the project.

## What Goals Are Useful For

Use a goal as the AI worker's boundary and completion standard:

- map a project before changing it
- infer the mature target state before execution
- turn repair, refactor, release, automation, and evaluation work into long-running loops
- make risky actions explicit, such as deleting files, changing global config, pushing, publishing, or writing to external systems
- make the final result verifiable instead of ending at “seems fine”

## Why Generate a Goal First

AI often wants to start working immediately. That sounds productive, but it can waste time when the direction is fuzzy.

Generating a goal first clarifies direction, scope, validation, stop conditions, and the do-not-stop-until-done framing before the agent starts execution. That makes the AI behave more like a persistent worker and less like a patch machine guessing its way through the project.

Persistent work does not mean “anything goes.” A good goal also defines operation boundaries: do not delete core files, do not change global settings, do not expose secrets, do not assume commit/push, and stop before high-risk actions.

## What This Skill Does

The first input source is the current conversation: the user's latest objective, constraints, preferences, corrections, confirmed facts, and implicit deliverables. Project files and runtime evidence refine the goal; they do not replace fresh conversation intent.

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

Then it generates one preferred executable long-horizon prompt by default. It returns candidate directions only when the target conflicts, risk is high, a critical input is missing, or the user explicitly asks to compare options.

It is not a project-management template pack. It does not require status documents or heavyweight process files.

It also does not assume GitHub. Without GitHub, the goal can use local validation, artifacts, logs, or a summary as the delivery path. Commit, push, issues, PRs, and releases are included only when the user explicitly asks for them.

For product ideas, `goal-skills` turns the idea into an executable long-horizon prompt that can start with the smallest useful slice and then continue into the broader product direction. Deeper product clarification, judgment, planning, QA, and release handoff can optionally continue in [products-skills](https://github.com/DOIT-Ben/products-skills).

## Trigger Phrases

Trigger phrasing is intentionally narrow in both Chinese and English:

- `生成一个goal`
- `针对这个项目设置一个goal`
- `给这个项目设置一个goal`
- `为当前项目生成goal`
- `把这个目标转成goal`
- `先别改，先生成goal`
- `generate a goal`
- `agent goal`
- `AI agent goal`
- `set a goal for this project`
- `convert this into a goal`
- `Codex goal`
- `Claude Code goal`
- `/goal prompt`
- `convert task to goal`
- `plan before change`

This is not a generic planning template. Trigger it when the user clearly wants a goal, an executable agent task, a task-to-goal conversion, or a plan-before-change handoff. `Codex goal`, `Claude Code goal`, and `/goal prompt` are compatibility triggers, not the capability boundary.

## Install

The core package is a portable Markdown skill. Any agent that can read `SKILL.md`, rule files, or task briefs can use it. Codex and Claude Code are only adapter examples below.

Codex install example:

```powershell
npx skills add https://github.com/DOIT-Ben/goal-skills -g -a codex -y --full-depth
```

For local development from the repository root:

```powershell
npx skills add . -g -a codex -y --full-depth
```

Claude Code install example:

```powershell
npx skills add https://github.com/DOIT-Ben/goal-skills -g -a claude-code -y --full-depth
```

Other agents can read [SKILL.md](SKILL.md), [references](references/), and [examples](examples/README.md) directly. If a platform does not support skills CLI, import `SKILL.md` as a rule or tool instruction.

## Examples

- [examples index](examples/README.md)
- [messy-project](examples/messy-project.md)
- [broken-app](examples/broken-app.md)
- [product-idea](examples/product-idea.md)
- [direct-repair](examples/direct-repair.md)
- [research-spec](examples/research-spec.md)
- [create-from-zero](examples/create-from-zero.md)
- [add-extend](examples/add-extend.md)
- [refactor-reorganize](examples/refactor-reorganize.md)
- [polish-improve](examples/polish-improve.md)
- [automate-dry-run](examples/automate-dry-run.md)
- [experiment-evaluate](examples/experiment-evaluate.md)
- [release-publish](examples/release-publish.md)
- [high-risk-external-write](examples/high-risk-external-write.md)

## Typical Use

```text
$goal-skills

把这个目标转成goal：修复这个项目的本地启动流程，并验证 health 接口。
请生成最终 goal，不要提交推送。
```

When the direction truly needs comparison:

```text
$goal-skills

先别改，先生成goal。
这个项目方向不确定，请先给 2-3 个候选方向和推荐选择。
```

## Package Shape

```text
goal-skills/
  README.md
  README.en.md
  SKILL.md
  skill.json
  references/
    goal-judgment-matrix.md
    goal-template.md
    example-walkthrough.md
  examples/
    README.md
    messy-project.md
    broken-app.md
    product-idea.md
    direct-repair.md
    research-spec.md
    create-from-zero.md
    add-extend.md
    refactor-reorganize.md
    polish-improve.md
    automate-dry-run.md
    experiment-evaluate.md
    release-publish.md
    high-risk-external-write.md
  evals/
    evals.json
```
