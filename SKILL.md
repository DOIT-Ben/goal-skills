---
name: goal-skills
description: Use when the user asks for an AI agent goal, executable goal, goal-skills, 生成goal, 把目标/想法转成goal, set a goal, convert task to goal, Codex goal, Claude Code goal, or plan before change.
---

# Goal Skills

## Purpose

Use this skill as a goal compiler for AI agents: turn the current conversation, project state, directory, product idea, current problem, or rough target into one executable agent goal.

The job is not project management paperwork. The job is judgment:

1. Treat the current conversation as the first input source. Extract the user's goal, constraints, preferences, confirmed facts, implicit deliverables, and latest corrections before looking at files.
2. Inspect real project signals only after the conversation-derived target is clear enough to guide inspection.
3. Decide what kind of goal the situation actually needs, including idea-to-product-slice goals when the user starts from a concept instead of an existing repo.
4. Produce one preferred executable goal by default: specific, bounded, verifiable, and useful for autonomous execution to a deliverable result.
5. Stop for clarification only when the target conflicts with evidence or rules, risk is high, or a critical input is missing.

## Trigger Phrases

Treat these as strong phrase triggers:

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

Also trigger on clear intent, even when the exact phrase differs:

- the user wants an executable AI-agent goal instead of immediate work
- the user asks to convert a task, product idea, bug, rough target, or project direction into a goal
- the user says to plan or define the goal before editing, implementing, refactoring, publishing, or changing files
- the user wants bounded autonomous execution with scope, constraints, validation, and stop conditions

Do not trigger for generic planning requests unless the user mentions `goal`, agent goal, `/goal`, Claude Code goal, or asks to turn the plan into an executable goal.

## Minimal Package Shape

This package exposes only one skill: `goal-skills`.

Internal references are optional support files:

- `references/goal-judgment-matrix.md`: how to choose goal type across different project states and domains.
- `references/goal-template.md`: final goal structure and examples.
- `references/example-walkthrough.md`: worked example from a messy project prompt to final goal.
- `examples/`: public examples covering all goal types, conversation-first behavior, local-only delivery, authorized release, dry-run automation, and high-risk external-write boundaries.

Load references by rule:

- Load `references/goal-judgment-matrix.md` when the direction is ambiguous, the project is messy, the request spans multiple possible goal types, or the input is a product idea.
- Load `references/goal-template.md` whenever writing, revising, or checking a final goal.
- Load `references/example-walkthrough.md` or `examples/` when output style is unclear, when adding or reviewing examples, or when testing common behavior.

## Default Behavior

Default to compiling the user's current idea into one preferred executable goal in the same turn.

Use the current conversation first, then project evidence. Do not let filesystem discovery override fresh user intent, constraints, preferences, or corrections from the chat.

If the user asks for goal analysis, still recommend one primary executable goal unless the situation truly needs a choice. Use candidate directions only when the goal is conflicting, high-risk, under-specified in a way that blocks execution, or when the user explicitly asks to compare options.

If the user already gives a clear target and asks to generate a goal, produce the final goal directly.

If the user asks to execute the goal, do not treat this skill as permission to skip goal confirmation. Execute only when a final goal has been confirmed and the user explicitly asks to run it. If the goal is missing or ambiguous, generate or restate the goal first and ask for confirmation.

During any execution handoff, stay inside the confirmed goal. Stop before destructive, irreversible, credential-related, external-write, or scope-expanding actions unless the user explicitly approves that specific action.

## Bounded Autonomy

An AI agent can keep working autonomously from a goal, but autonomy is not permission to ignore safety boundaries.

Every generated goal must include operation constraints. Default constraints:

- Do not delete, overwrite, or mass-move source code, project rules, datasets, model files, generated deliverables, or configuration files unless the user explicitly asks for that specific operation.
- Do not run destructive commands such as `git reset --hard`, forced checkout, recursive delete, database wipe, secret rotation, or bulk formatting without explicit approval.
- Do not expose, print, commit, or hard-code secrets, tokens, private accounts, or credentials.
- Do not change global machine settings, scheduled tasks, system services, shell profiles, package managers, or external accounts unless the goal explicitly requires it.
- Do not write to external systems such as GitHub, cloud services, deployments, databases, package registries, payment systems, email, messaging tools, public websites, or third-party APIs unless the user explicitly asks for that exact delivery path.
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

Apply the same rule to all external writes. If a goal would deploy, publish, send email, modify a database, call a third-party write API, upload to cloud storage, change billing/payment state, or affect public/private accounts, include a stop-and-confirm step unless the user already authorized that exact action.

## What To Inspect

Use available signals. Do not require all of them:

- current conversation, including latest user goal, constraints, preferences, corrections, confirmed facts, implicit deliverables, and requested delivery style
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
| Product idea | Idea / Product Slice | Need to turn an idea into a practical product slice |

Prefer the goal type that removes the biggest current blocker. For example:

- If the user wants a new feature but the project cannot run, choose `Repair / Restore` first.
- If the user wants polish but the intended product is undefined, choose `Research / Spec` or `Discover / Map` first.
- If the user wants release but there is no verified build/export path, choose `Repair / Restore` or `Release / Publish` with verification as the core.
- If the user says only "看看这个项目", choose `Discover / Map`.
- If the user starts from a product idea instead of an existing implementation, choose `Idea / Product Slice`.
- If the idea is too fuzzy, the first goal should clarify the product slice, target user, success criteria, and smallest useful deliverable before any implementation.

## Product Ideas

When the user asks for a goal from an idea, do not force the request into an existing-project frame.

For product ideas, generate a goal that prepares only the next product-delivery step:

- clarify the user, problem, scenario, and smallest useful slice
- decide whether the idea should continue, revise, or stop
- define a buildable product goal with validation

Do not turn this skill into a product-methodology package. If the user wants deeper product brainstorming, product judgment, planning, QA, or release handoff, mention `products-skills` once as an optional follow-up and keep the generated goal usable with any plain AI agent.

## Candidate Goal Output

Candidate output is the exception, not the default. Use it only when choosing the wrong direction would be likely or costly, when the user explicitly asks for alternatives, or when a missing critical input blocks a complete executable goal.

When candidates are needed, choose the smallest useful candidate format.

Match the user's language. Use Chinese labels for Chinese requests and English labels for English requests. Do not mix Chinese headings with English placeholder text.

Use lightweight mode for explicit quick comparisons or moderate uncertainty: give one recommended direction and one backup direction.

Use full mode for high uncertainty, high-risk delivery work, or conflicting signals: give 2-3 candidates.

Chinese lightweight format:

```text
### 当前判断
- 当前阶段：
- 最大阻塞点：

### 推荐方向
- 首选：
- 理由：
- 备选：

### 需要你确认
如果这个方向对，确认后我生成最终 goal。
```

English lightweight format:

```text
### Current Read
- Current state:
- Biggest blocker:

### Recommended Direction
- Primary:
- Why:
- Backup:

### Confirmation Needed
If this direction looks right, confirm and I will generate the final goal.
```

Chinese full format:

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

#### A. [目标类型]
- 适合原因：
- 会做什么：
- 不会做什么：
- 预期结果：
- 验证方式：
- 风险：

#### B. [目标类型]
- 适合原因：
- 会做什么：
- 不会做什么：
- 预期结果：
- 验证方式：
- 风险：

### 需要你确认
请选择 A / B，或告诉我你想优先达成的结果。确认后我再生成最终 goal。
```

English full format:

```text
### Current Read
- Project/task type:
- Current state:
- Biggest uncertainty:
- Biggest blocker:

### Recommended Direction
- Primary:
- Why:
- Not recommended now:

### Candidate Goals

#### A. [goal type]
- Fit:
- Will do:
- Will not do:
- Expected result:
- Validation:
- Risk:

#### B. [goal type]
- Fit:
- Will do:
- Will not do:
- Expected result:
- Validation:
- Risk:

### Confirmation Needed
Choose A / B, or tell me which outcome to prioritize. After confirmation I will generate the final goal.
```

## Final Goal Output

When the user's intent is clear enough, output one executable goal immediately:

Match the user's language. For Chinese requests, use the Chinese skeleton. For English requests, use the English skeleton.

Chinese skeleton:

```text
Goal: [目标类型]

项目背景：
- [项目类型、当前状态、关键证据]

目标：
- [一个具体可验证的结果]

范围：
- [要检查或修改的内容]

非目标：
- [明确不做的事情]

交付方式：
- [本地产物 / 总结 / commit / PR / release；不要默认假设 GitHub]

约束：
- [风格、安全、项目规则、不要无关重构、不要泄露密钥]
- [操作边界：不做破坏性改动、不删除核心文件、不暴露凭据、不改全局配置]

执行步骤：
1. [检查证据]
2. [实现或整理]
3. [验证]
4. [总结]

验证标准：
- [命令、检查、人工复核或产物]

完成标准：
- [用户可观察的完成状态]

停止条件：
- [什么时候停下来问用户]
- [破坏性、高风险、不可逆、外部写入、GitHub 或凭据动作需要确认]
```

English skeleton:

```text
Goal: [Goal Type]

Background:
- [project type, current state, key evidence]

Goal:
- [one concrete, verifiable outcome]

Scope:
- [what to inspect or change]

Non-Goals:
- [what not to do]

Delivery:
- [local artifact / summary / commit / PR / release; do not assume GitHub]

Constraints:
- [style, safety, project rules, no unrelated refactor, no secrets]
- [operation boundaries: no destructive edits, no deleting core files, no credential exposure, no global changes]

Steps:
1. [inspect evidence]
2. [implement or compose]
3. [validate]
4. [summarize]

Validation:
- [commands, checks, manual review, artifacts, or metrics]

Completion:
- [observable done state]

Stop Conditions:
- [when to ask the user instead of guessing]
- [destructive, high-risk, irreversible, external-write, GitHub, or credential actions need confirmation]
```

## Fallback Handling

If project evidence is missing or inaccessible, do not invent certainty.

- If README, package files, docs, or entry points are missing, state the missing evidence and generate a discovery-first goal.
- If the current directory cannot be read, ask for the correct path or user-provided context before generating a project-specific goal.
- If validation commands are unknown, include "discover validation path" as an early step instead of inventing a command.
- If dependencies are unavailable, avoid install commands by default; include dependency inspection and a stop condition before installation.
- If the user-provided target conflicts with project rules or visible state, surface the conflict and ask whether to prioritize the target or the rule.
- If confidence is low, mark the goal as low-confidence and make evidence gathering the first deliverable.

## Goal Acceptance Checklist

Before returning a final goal, check that it includes:

- one concrete outcome, not a vague improvement theme
- project or idea context grounded first in the current conversation, then in visible evidence or user-provided facts
- explicit scope and non-goals
- a local delivery path when GitHub or external services were not requested
- operation boundaries for destructive, credential, global, and external-write actions
- ordered execution steps that start with evidence gathering when the state is unclear
- validation standards using commands, artifacts, screenshots, source lists, manual review, or measurable checks
- completion criteria the user can observe
- stop conditions for missing inputs, high risk, external writes, irreversible actions, or scope conflicts
- a minimum complete loop: investigate or inspect, implement or compose, validate, and summarize the deliverable result

## Quality Bar

A good goal is:

- specific enough to execute
- bounded enough to finish without drifting
- complete enough to close a minimum complete loop, not merely a tiny TODO slice
- clear about non-goals
- clear about operation boundaries
- tied to the current conversation and actual project evidence
- validated by commands, artifacts, or review steps
- explicit about when to stop and ask

Avoid vague goals like "improve the project" or "make it better". Convert them into bounded outcomes such as "repair the local startup path and verify the health endpoint" or "map the repo and recommend the next three implementation goals".
