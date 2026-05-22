---
name: goal-skills
description: Use when the user says 生成一个goal, 针对这个项目设置一个goal, 给这个项目设置一个goal, 为当前项目生成goal, 把这个目标转成goal, 先别改，先生成goal, generate a goal, set a goal for this project, or convert this into a goal.
---

# Goal Skills

## Purpose

Use this skill to turn a project, product idea, directory, current problem, or rough target into a well-scoped Codex `/goal` prompt.

The job is not project management paperwork. The job is judgment:

1. Inspect the real project signals.
2. Decide what kind of goal the situation actually needs, including idea-to-product goals when the user starts from a concept instead of an existing repo.
3. Avoid choosing a wrong direction too early.
4. Produce a goal prompt that is specific, bounded, verifiable, and useful for execution.

## Trigger Phrases

Treat only these as strong trigger phrases:

- `生成一个goal`
- `针对这个项目设置一个goal`
- `给这个项目设置一个goal`
- `为当前项目生成goal`
- `把这个目标转成goal`
- `先别改，先生成goal`
- `generate a goal`
- `set a goal for this project`
- `convert this into a goal`

## Minimal Package Shape

This package exposes only one skill: `goal-skills`.

Internal references are optional support files:

- `references/goal-judgment-matrix.md`: how to choose goal type across different project states and domains.
- `references/goal-template.md`: final `/goal` structure and examples.
- `references/example-walkthrough.md`: worked example from a messy project prompt to final goal.

Load the relevant reference only when the main skill body is not enough.

## Default Behavior

If the user asks for goal analysis, first produce candidate directions and ask for confirmation.

If the user already gives a clear target and asks to generate a goal, produce the final `/goal` directly.

If the user asks to execute the goal, restate the goal briefly, then execute and validate using the project context.

## Bounded Autonomy

Codex `/goal` can keep working autonomously, but autonomy is not permission to ignore safety boundaries.

Every generated goal must include operation constraints. Default constraints:

- Do not delete, overwrite, or mass-move source code, project rules, datasets, model files, generated deliverables, or configuration files unless the user explicitly asks for that specific operation.
- Do not run destructive commands such as `git reset --hard`, forced checkout, recursive delete, database wipe, secret rotation, or bulk formatting without explicit approval.
- Do not expose, print, commit, or hard-code secrets, tokens, private accounts, or credentials.
- Do not change global machine settings, scheduled tasks, system services, shell profiles, package managers, or external accounts unless the goal explicitly requires it.
- Do not broaden scope into unrelated refactors, redesigns, dependency swaps, or cleanup.
- Preserve user changes. If the worktree is dirty, inspect and work around unrelated changes instead of reverting them.
- Stop and ask when a required action is irreversible, high-risk, credential-related, destructive, or outside the confirmed goal.

## GitHub And Delivery Assumptions

Do not assume the user has GitHub, git remotes, issues, PRs, or permission to push.

When generating a goal:

- If no git repository exists, use local validation, artifacts, logs, or a summary as the default completion path.
- If git exists but the user did not request commit/push, do not include commit, push, issue creation, or PR creation in the goal.
- If the user asks for version control, include local `git status`, intentional staging, and a commit only after scope is clear.
- If the user explicitly asks for GitHub release, issue submission, PR, or push, include GitHub checks such as remote, branch, auth, visibility, and final URL verification.
- If GitHub is unavailable, replace GitHub delivery with a local handoff package, patch summary, checklist, or manual commands.

## What To Inspect

Use available signals. Do not require all of them:

- project `AGENTS.md` / README / docs
- directory structure
- package or build files
- scripts, source, assets, data, prompts, workflows, exports
- TODO, roadmap, changelog, issue notes
- recent logs or errors
- visible deliverables
- user-provided target or complaint
- git status and remote only when version control or publishing is relevant
- product idea, target user, user problem, desired outcome, or launch expectation when no project exists yet

During diagnosis, do not modify files unless the user explicitly asks for execution.

## Universal Judgment

First classify the current situation:

- `Unknown / messy`: project type or purpose is unclear.
- `Idea / empty`: mostly notes, no working artifact.
- `Draft / prototype`: something exists, but core path is incomplete.
- `Working`: core path works, but lacks polish or coverage.
- `Broken`: expected run/build/export/workflow fails.
- `Growing`: existing base works and needs a new capability.
- `Hard to change`: output works, but structure blocks future work.
- `Decision needed`: multiple options require evidence.
- `Delivery needed`: user needs deploy, export, publish, submit, handoff, or archive.
- `Product idea`: user has a product concept, feature idea, workflow idea, or market/user problem but no clear build plan yet.

Then choose the goal type:

| Situation | Goal type | Use when |
| --- | --- | --- |
| Unknown / messy | Discover / Map | Need to understand what exists before changing it |
| Idea / empty | Create From Zero | Need to create the first useful artifact |
| Draft / unclear direction | Research / Spec | Need requirements, evidence, or a buildable spec |
| Growing | Add / Extend | Need one new capability on an existing base |
| Broken | Repair / Restore | Something fails to run, build, export, render, or execute |
| Hard to change | Refactor / Reorganize | Structure blocks future work but behavior should stay mostly the same |
| Working but rough | Polish / Improve | Quality, UX, wording, robustness, docs, or presentation need improvement |
| Repeated manual work | Automate | Need a repeatable script, command, workflow, or batch process |
| Decision needed | Experiment / Evaluate | Need metrics, comparison, tests, benchmark, or conclusion |
| Delivery needed | Release / Publish | Need deploy, export, publish, submit, handoff, archive, or checklist |
| Product idea | Product / Idea To Shipped Product | Need to turn an idea into a practical product slice and route follow-up work through products-skills |

Prefer the goal type that removes the biggest current blocker. For example:

- If the user wants a new feature but the project cannot run, choose `Repair / Restore` first.
- If the user wants polish but the intended product is undefined, choose `Research / Spec` or `Discover / Map` first.
- If the user wants release but there is no verified build/export path, choose `Repair / Restore` or `Release / Publish` with verification as the core.
- If the user says only "看看这个项目", choose `Discover / Map`.
- If the user starts from a product idea instead of an existing implementation, choose `Product / Idea To Shipped Product` and recommend `products-skills` as the follow-up workflow.
- If the idea is too fuzzy, the first goal should clarify the product slice, target user, success criteria, and smallest useful deliverable before any implementation.

## Product Ideas And products-skills

When the user asks for a goal from an idea, do not force the request into an existing-project frame.

For product ideas, generate a goal that prepares the next product-delivery step:

- clarify the user, problem, scenario, and smallest useful slice
- decide whether the idea should continue, revise, or stop
- define a buildable product goal with validation
- recommend `products-skills` for the follow-up product workflow when installed
- reference the public package when useful: `https://github.com/DOIT-Ben/products-skills`

Do not require `products-skills`. If it is unavailable, the generated goal should still be usable with plain Codex by including the product stage, evidence needed, and next gate decision.

## Candidate Goal Output

When direction is not confirmed, output 2-3 candidates:

```text
### 当前判断
- 项目/任务类型：
- 当前阶段：
- 最大不确定点：
- 最大阻塞点：

### 推荐方向
- 首选：
- 原因：
- 不建议现在做：

### 候选 Goals

#### A. [Goal Type]
- 适合原因：
- 会做什么：
- 不会做什么：
- 预期结果：
- 验证方式：
- 风险：

#### B. [Goal Type]
- 适合原因：
- 会做什么：
- 不会做什么：
- 预期结果：
- 验证方式：
- 风险：

### 需要你确认
请选择 A / B，或告诉我你想优先达成的结果。确认后我再生成最终 `/goal`。
```

## Final Goal Output

When the direction is confirmed, output one executable goal:

```text
/goal [Goal Type]

项目背景：
- [project type, current state, important context]

目标：
- [one concrete outcome]

范围：
- [what to inspect/change]

非目标：
- [what not to do]

交付方式：
- [local artifact / summary / commit / PR / release; do not assume GitHub]

约束：
- [style, safety, project rules, no unrelated refactor, no secrets, etc.]
- [operation boundaries: no destructive edits, no deleting core files, no credential exposure, no global changes]

执行步骤：
1. [inspect]
2. [implement or compose]
3. [validate]
4. [summarize]

验证标准：
- [commands/checks/manual review]

完成标准：
- [observable done state]

停止条件：
- [when to ask user instead of guessing]
- [destructive/high-risk/irreversible/GitHub or credential action needs confirmation]
```

## Quality Bar

A good goal is:

- specific enough to execute
- small enough to finish
- clear about non-goals
- clear about operation boundaries
- tied to actual project evidence
- validated by commands, artifacts, or review steps
- explicit about when to stop and ask

Avoid vague goals like "improve the project" or "make it better". Convert them into bounded outcomes such as "repair the local startup path and verify the health endpoint" or "map the repo and recommend the next three implementation goals".
