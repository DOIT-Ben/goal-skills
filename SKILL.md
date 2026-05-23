---
name: goal-skills
description: Use when the user asks for an AI agent goal, executable goal, goal-skills, 生成goal, 把目标/想法转成goal, set a goal, convert task to goal, Codex goal, Claude Code goal, or plan before change.
---

# Goal Skills

## Purpose

Use this skill as a long-horizon goal compiler for AI agents: turn the current conversation, project state, directory, product idea, current problem, or rough target into one copy-pasteable autonomous execution prompt that can keep an AI working until the larger target is completed or a real stop condition is reached.

The job is not project management paperwork. The job is judgment:

1. Treat the current conversation as the first input source. Extract the user's goal, constraints, preferences, confirmed facts, implicit deliverables, and latest corrections before looking at files.
2. Inspect real project signals only after the conversation-derived target is clear enough to guide inspection.
3. Decide what kind of goal the situation actually needs, including idea-to-product-slice goals when the user starts from a concept instead of an existing repo.
4. Produce one preferred long-running agent prompt by default: it must start by giving the AI a concrete target, then require intent analysis, project-state analysis, long-horizon execution, repeated validation, and continued work until completion or a stop condition.
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

- the user wants an executable AI-agent goal or long-running autonomous prompt instead of immediate work
- the user asks to convert a task, product idea, bug, rough target, or project direction into a goal
- the user says to plan or define the goal before editing, implementing, refactoring, publishing, or changing files
- the user wants bounded autonomous execution with scope, constraints, validation, stop conditions, and "do not stop until done" behavior

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

Default to compiling the user's current idea into one preferred copy-pasteable long-horizon prompt in the same turn.

Use the current conversation first, then project evidence. Do not let filesystem discovery override fresh user intent, constraints, preferences, or corrections from the chat.

Before writing the final prompt, analyze the user's intent and the project state. The final output should not look like a short project-management note. It should read like an instruction the user can paste into another AI agent to make it work for a long time: "现在给你一个目标：..." / "I am giving you a goal: ...".

Important: if the user says "分析整个项目", "分析这个项目", "set a goal for this repo", or similar while invoking this skill, do not turn the response into a project audit report. Do a light orientation pass only, then output the long-horizon prompt. The analysis is input to the prompt, not the deliverable.

If the user asks for goal analysis, still recommend one primary long-horizon prompt unless the situation truly needs a choice. Use candidate directions only when the goal is conflicting, high-risk, under-specified in a way that blocks execution, or when the user explicitly asks to compare options.

If the user already gives a clear target and asks to generate a goal, produce the final copy-pasteable prompt directly.

If the user asks to execute the goal, do not treat this skill as permission to skip goal confirmation. Execute only when a final goal has been confirmed and the user explicitly asks to run it. If the goal is missing or ambiguous, generate or restate the goal first and ask for confirmation.

During any execution handoff, stay inside the confirmed goal. The prompt may require the agent to continue selecting next actions and executing loops. It should only stop for destructive, irreversible, credential-related, unauthorized external-write, or scope-expanding actions that are not already permitted by the confirmed goal.

## Long-Horizon Prompt Behavior

The default final artifact is a prompt, not a report. It should be easy for the user to copy and paste into an AI agent.

For Chinese requests, the final prompt should begin with this shape:

```text
现在给你一个目标：[从用户意图和项目状态提炼出的长期目标]。

在这个目标完成之前，不允许停止工作。你必须持续分析、执行、验证和选择下一步，直到达到完成标准或触发停止条件。
```

For English requests, the final prompt should begin with this shape:

```text
I am giving you a goal: [long-horizon target extracted from the user's intent and project state].

Do not stop working until this goal is complete. Keep analyzing, executing, validating, and choosing the next step until the completion criteria are met or a stop condition is triggered.
```

The prompt must make the agent do more than a single bounded slice when the user's request is long-horizon. It should include:

- analyze user intent first, including latest corrections and preferences
- inspect the project state second, using real evidence
- infer the project's ultimate purpose or mature target state
- build or update a long-term execution map
- execute the highest-value next step
- validate after each meaningful change
- summarize progress and immediately continue with the next step when no stop condition applies
- treat phase completion, minimum-loop completion, roadmap updates, and "next phase suggested" as continuation triggers, not stopping points
- treat analysis reports, dev-record updates, roadmap documents, codebase maps, and index updates as intermediate artifacts; after writing them, choose the highest-value safe action from the findings and continue
- treat read-only whole-project analysis as orientation only unless the goal explicitly says read-only; once it identifies a safe next fix, validation gap, command mismatch, warning, or documentation/runtime inconsistency, execute that next step instead of handing it back as a suggestion
- preserve safe boundaries for destructive, credential, global, and unauthorized external-write actions

## Output Contract

Final output must be short outside the prompt and substantial inside the prompt.

Default shape for Chinese:

````text
这是给 AI 直接执行的长期 goal 提示词：

```text
现在给你一个目标：...
...
```
````

Default shape for English:

````text
Here is the copy-pasteable long-horizon goal prompt:

```text
I am giving you a goal: ...
...
```
````

Do not include a long project audit, score, test log, git status log, graph report, or "recommended goal" prose before the prompt. If orientation evidence is useful, include at most 3 short bullets before the prompt:

- inferred project purpose
- current stage
- biggest blocker

The prompt itself may tell the receiving AI to run graph analysis, tests, browser checks, or deeper audits. The skill invocation should not perform and report those long-running checks unless the user explicitly asks to execute an audit rather than generate a goal.

Never use the old final-output shape as the primary answer:

- `Goal: [type]`
- `项目背景 / 目标 / 范围 / 非目标 / 交付方式`
- long numbered reports with ratings like `8.4 / 10`
- "已刷新图谱 / 已跑验证 / 当前 git status" as the main deliverable

## Bounded Autonomy

An AI agent can keep working autonomously from a goal, but autonomy is not permission to ignore safety boundaries.

Every generated goal must include operation constraints. Default constraints:

- Do not delete, overwrite, or mass-move source code, project rules, datasets, model files, generated deliverables, or configuration files unless the user explicitly asks for that specific operation.
- Do not run destructive commands such as `git reset --hard`, forced checkout, recursive delete, database wipe, secret rotation, or bulk formatting without explicit approval.
- Do not expose, print, commit, or hard-code secrets, tokens, private accounts, or credentials.
- Do not change global machine settings, scheduled tasks, system services, shell profiles, package managers, or external accounts unless the goal explicitly requires it.
- Do not write to external systems such as GitHub, cloud services, deployments, databases, package registries, payment systems, email, messaging tools, public websites, or third-party APIs unless the long-horizon goal explicitly includes that delivery path and the required authority is available.
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
- If the long-horizon goal explicitly includes commit/push/deploy/release and the environment has the needed access, treat those steps as normal execution, not as a stop condition.
- If GitHub is unavailable, replace GitHub delivery with a local handoff package, patch summary, checklist, or manual commands.

Apply the same rule to all external writes. If the long-horizon goal explicitly authorizes deploy, publish, send email, database modification, third-party API mutation, cloud upload, billing/payment change, or account-affecting actions and the required authority is available, execute that path as part of the goal. Stop only when the write is unauthorized, outside the goal, missing required authority, destructive/irreversible beyond the goal, or credential-sensitive.

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

During prompt generation only, do not modify files unless the user explicitly asks for execution. This restriction is for the skill invocation that creates the goal prompt; it must not teach the receiving AI to stop at read-only diagnosis when the long-horizon goal requires execution.

For goal generation, use an orientation pass, not a full audit. Prefer reading the smallest evidence set that identifies project purpose and state:

- project rules such as `AGENTS.md`
- README or equivalent entry doc
- package/build config
- docs index or roadmap if present
- top-level source and script directories

Do not run test suites, browser flows, graph refreshes, long static analysis, or repository-wide audits just to generate the goal prompt. Put those checks inside the generated prompt for the receiving AI to execute.

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

For product ideas, generate a long-horizon prompt that can start from uncertainty without pretending the whole product is already specified. It should include the next product-delivery step and preserve the broader direction:

- clarify the user, problem, scenario, and smallest useful slice
- decide whether the idea should continue, revise, or stop
- infer the likely mature product direction and explicit assumptions
- define a buildable first slice with validation
- require the agent to continue into the next product step when the first slice is validated and no stop condition applies

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

When the user's intent is clear enough, output one copy-pasteable long-horizon prompt immediately:

Match the user's language. For Chinese requests, use the Chinese skeleton. For English requests, use the English skeleton.

The answer should be the prompt, not a report about how the prompt was derived. Keep any preface to one sentence or at most 3 evidence bullets. The first line inside the copyable block must be the target line.

Chinese skeleton:

```text
现在给你一个目标：[从用户意图和项目状态提炼出的长期目标]。

在这个目标完成之前，不允许停止工作。你必须持续分析、执行、验证和选择下一步，直到达到完成标准或触发停止条件。

一、先理解真实目标
- 先分析当前对话里的用户意图、约束、偏好、最新修正和隐含交付物。
- 再检查项目状态：[需要阅读的项目规则、README、docs、源码、配置、脚本、测试、产物或工作流]。
- 判断这个项目的终极目的、当前阶段、最大价值机会、最大阻塞点和成熟状态。
- 如果证据不足，不要编造；先列出不确定点，并把补证据作为第一轮任务。

二、长期执行地图
- 项目终极愿景：[项目成熟后应该服务谁、解决什么问题、交付什么能力]
- 当前阶段：[Unknown / messy、Idea、Draft、Working、Broken、Growing、Hard to change、Delivery needed 等]
- 近期目标：[1-3 个最高价值目标]
- 中期方向：[项目继续变有用需要补齐的能力、质量、文档、体验或流程]
- 长期可能性：[值得保留但暂不投入的探索方向]
- 明确非目标：[不要做的方向，避免跑偏]

三、持续执行循环
1. 观察：读取证据，确认当前状态和最值得推进的问题。
2. 判断：选择对长期目标最有价值、风险最低的下一步。
3. 行动：完成一个真实推进项目价值的改动、文档、验证、修复、原型、脚本或交付物。只读全项目分析、只写分析报告、路线图、dev-record、索引或代码地图不算完成；写完后必须从发现的问题里选一个最高价值且安全的执行项继续推进。
4. 验证：运行可用的测试、构建、脚本、检查、截图、人工复核或产物验证。
5. 复盘：说明完成了什么、验证结果、剩余风险、项目离终极目标近了多少。
6. 继续：如果没有触发停止条件，自动选择下一步并继续工作。完成 Phase 1、完成一个最小闭环、写出下一阶段建议、完成只读分析、沉淀一份分析文档，都不是停止理由；这些只意味着你应该进入下一阶段或执行分析中发现的最高价值安全事项。

四、执行约束
- 保持外科式改动，不做与目标无关的重构、换栈、批量格式化或清理。
- 保留用户已有改动，不回滚用户改动。
- 不删除、覆盖、批量移动核心源码、配置、数据、模型、产物或规则文件，除非用户明确要求。
- 不执行 `git reset --hard`、强制 checkout、递归删除、数据库清空、密钥轮换等高风险操作，除非用户明确批准。
- 不暴露、打印、提交或硬编码密钥、token、账号、凭据。
- 不改全局机器配置、系统服务、计划任务、shell 配置、外部账号，除非目标明确要求。
- 不写入外部系统，包括 GitHub、云服务、部署平台、数据库、包注册表、邮件、消息工具、公开网站或第三方 API，除非用户明确授权该交付路径。

五、验证标准
- [命令、产物、截图、人工检查、来源清单或指标]

六、完成标准
- [用户能看到、运行、复用或验收的成熟状态]
- 如果完整成熟目标过大，先完成一个最小可交付阶段，然后立刻进入下一阶段继续推进，不要因为阶段完成而停止。

七、停止条件
- 当前一步无法从目标、已知证据或安全约束中推导出来，且继续只会瞎猜。
- 需要破坏性、不可逆、凭据相关、未经授权的外部写入、或会越过明确目标边界的操作。
- 项目规则与目标冲突。
- 连续两轮验证失败，需要用户选择取舍。
- 当前目标已经整体达到成熟完成标准，且不存在下一步可安全推进的高价值事项。
```

English skeleton:

```text
I am giving you a goal: [long-horizon target extracted from the user's intent and project state].

Do not stop working until this goal is complete. Keep analyzing, executing, validating, and choosing the next step until the completion criteria are met or a stop condition is triggered.

1. Understand the real target first
- Analyze the current conversation for the user's intent, constraints, preferences, latest corrections, and implicit deliverables.
- Then inspect project state: [project rules, README, docs, source, config, scripts, tests, artifacts, or workflows to read].
- Infer the project's ultimate purpose, current stage, highest-value opportunity, biggest blocker, and mature target state.
- If evidence is missing, do not invent certainty; list the uncertainty and make evidence gathering the first loop.

2. Maintain a long-term execution map
- Ultimate vision: [who the mature project serves, what problem it solves, what capability it delivers]
- Current stage: [Unknown / messy, Idea, Draft, Working, Broken, Growing, Hard to change, Delivery needed, etc.]
- Near-term goals: [1-3 highest-value goals]
- Mid-term direction: [capabilities, quality, docs, UX, or workflow gaps to close]
- Long-term possibilities: [promising directions to preserve without pursuing immediately]
- Explicit non-goals: [directions to avoid]

3. Continuous execution loop
1. Observe: read evidence and identify the most valuable current problem.
2. Decide: choose the next step with the best long-term value and acceptable risk.
3. Act: complete one real value-advancing change, doc, validation, fix, prototype, script, or deliverable.
4. Validate: run available tests, builds, scripts, checks, screenshots, manual review, or artifact validation.
5. Review: summarize what changed, validation results, residual risk, and how much closer the project is to the ultimate goal.
6. Continue: if no stop condition applies, choose the next step and keep working. Completing Phase 1, closing a minimum loop, or writing the next-phase recommendation is not a stopping reason; it is the trigger to enter the next phase.

4. Constraints
- Keep edits surgical; do not do unrelated refactors, stack swaps, bulk formatting, or cleanup.
- Preserve user changes; do not revert user work.
- Do not delete, overwrite, or mass-move core source, config, data, models, deliverables, or rule files unless explicitly requested.
- Do not run high-risk commands such as `git reset --hard`, forced checkout, recursive delete, database wipe, or secret rotation unless explicitly approved.
- Do not expose, print, commit, or hard-code secrets, tokens, accounts, or credentials.
- Do not change global machine config, system services, scheduled tasks, shell profiles, or external accounts unless the goal explicitly requires it.
- Do not write to external systems such as GitHub, cloud services, deployment platforms, databases, package registries, email, messaging tools, public websites, or third-party APIs unless the goal explicitly authorizes that path and the required authority is available.

5. Validation
- [Commands, artifacts, screenshots, manual review, source list, or metrics]

6. Completion
- [Observable mature state the user can see, run, reuse, or accept]
- If the full mature target is too large, complete one minimum deliverable phase, then immediately enter the next phase and continue. Do not stop just because a phase is complete.

7. Stop Conditions
- The next step cannot be derived from the goal, evidence, or safety constraints and continuing would only be guessing.
- A destructive, irreversible, credential-related, unauthorized external-write, or out-of-goal action is needed.
- Project rules conflict with the target.
- Validation fails for two consecutive loops and the user must choose a tradeoff.
- The whole goal has reached its mature completion criteria and there is no safe, high-value next step left.
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

- one long-horizon target, not a vague improvement theme
- project or idea context grounded first in the current conversation, then in visible evidence or user-provided facts
- a prompt-first opening the user can copy and paste into another AI agent
- explicit "do not stop until complete" behavior with safe stop conditions
- explicit "phase completion is not completion" behavior
- commit/push/deploy are continuation steps when the goal explicitly authorizes them and access exists
- analysis/report/documentation-only loops must roll into a concrete next action instead of stopping
- a project-purpose or mature-state analysis step before execution
- a long-term execution map when the user wants broad, long-running work
- explicit scope and non-goals
- a local delivery path when GitHub or external services were not requested
- operation boundaries for destructive, credential, global, and unauthorized external-write actions
- ordered execution loops that start with evidence gathering when the state is unclear and continue when no stop condition applies
- validation standards using commands, artifacts, screenshots, source lists, manual review, or measurable checks
- completion criteria the user can observe
- stop conditions for true blockers, high risk, unauthorized external writes, irreversible actions, or scope conflicts
- a repeatable execution loop: investigate or inspect, decide, act, validate, summarize, and continue until complete or stopped

## Quality Bar

A good goal is:

- specific enough to execute
- expansive enough to support long-running autonomous work without collapsing into a tiny TODO slice
- bounded by safety constraints, non-goals, validation, and stop conditions so it does not drift
- complete enough to keep repeating meaningful inspect / act / validate / review loops
- clear about non-goals
- clear about operation boundaries
- tied to the current conversation and actual project evidence
- validated by commands, artifacts, or review steps
- explicit about when to stop and ask

Avoid vague goals like "improve the project" or "make it better". Convert them into bounded outcomes such as "repair the local startup path and verify the health endpoint" or "map the repo and recommend the next three implementation goals".
