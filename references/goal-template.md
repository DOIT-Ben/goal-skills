# Goal Template

Use this reference when writing the final goal.

Start from the current conversation. The user's latest goal, constraints, preferences, corrections, confirmed facts, and implicit deliverables are the first input source. Project files and runtime evidence refine the goal; they should not erase fresh conversation intent.

## Final Long-Horizon Prompt Skeleton

Match the user's language. Use the Chinese skeleton for Chinese requests and the English skeleton for English requests.

The final artifact should be a copy-pasteable prompt for another AI agent. It should not read like an internal analysis report. Start with the target, then force long-horizon execution: analyze intent, inspect project state, infer the mature target, execute loops, validate, and continue until complete or stopped by an explicit safety condition. Phase completion is not completion; it should trigger the next phase. Read-only analysis and analysis documents are not completion; they should trigger concrete execution from the findings unless the goal explicitly says read-only only.

Keep anything outside the prompt minimal. For normal use, write one sentence and then the code block. If project orientation matters, include at most 3 short bullets before the code block. Do not include audit logs, command results, graph node counts, test counts, git status, project ratings, or old-style `Goal: [type]` documents before or after the prompt.

### Chinese Skeleton

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
- 不把未要求的 commit、push、issue、PR、发布写入目标；GitHub 只在用户明确要求或项目交付方式明确依赖 GitHub 时使用。
- 不写入外部系统，包括云服务、部署平台、数据库、包注册表、邮件、消息工具、公开网站或第三方 API，除非用户明确授权该交付路径。

五、验证标准
- [命令、产物、截图、人工检查、来源清单或指标]

六、完成标准
- [用户能看到、运行、复用或验收的成熟状态]
- 如果完整成熟目标过大，先完成一个最小可交付阶段，然后立刻进入下一阶段继续推进，不要因为阶段完成而停止。

七、停止条件
- 当前一步无法从目标、已知证据或安全约束中推导出来，且继续只会瞎猜。
- 需要删除核心文件、改全局配置、处理密钥、执行破坏性命令、未经授权的外部写入、或越过明确目标边界时，先停下来确认。
- 项目规则与目标冲突。
- 连续两轮验证失败，需要用户选择取舍。
- 当前目标已经整体达到成熟完成标准，且不存在下一步可安全推进的高价值事项。
```

### English Skeleton

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
3. Act: complete one real value-advancing change, doc, validation, fix, prototype, script, or deliverable. Read-only whole-project analysis, a report, roadmap, dev record, index update, or codebase map alone is not completion; after producing it, choose the highest-value safe action from the findings and continue.
4. Validate: run available tests, builds, scripts, checks, screenshots, manual review, or artifact validation.
5. Review: summarize what changed, validation results, residual risk, and how much closer the project is to the ultimate goal.
6. Continue: if no stop condition applies, choose the next step and keep working. Completing Phase 1, closing a minimum loop, writing the next-phase recommendation, completing read-only analysis, or producing an analysis document is not a stopping reason; it is the trigger to enter the next phase or execute the highest-value safe finding.

4. Constraints
- Keep edits surgical; do not do unrelated refactors, stack swaps, bulk formatting, or cleanup.
- Preserve user changes; do not revert user work.
- Do not delete, overwrite, or mass-move core source, config, data, models, deliverables, or rule files unless explicitly requested.
- Do not run high-risk commands such as `git reset --hard`, forced checkout, recursive delete, database wipe, or secret rotation unless explicitly approved.
- Do not expose, print, commit, or hard-code secrets, tokens, accounts, or credentials.
- Do not change global machine config, system services, scheduled tasks, shell profiles, or external accounts unless the goal explicitly requires it.
- Do not include unrequested commit, push, issue, PR, or release steps.
- Do not write to external systems such as cloud services, deployment platforms, databases, package registries, email, messaging tools, payment systems, public websites, or third-party APIs unless the goal explicitly authorizes that path and the required authority is available.

5. Validation
- [Commands, artifacts, screenshots, manual review, source list, or metrics]

6. Completion
- [Observable mature state the user can see, run, reuse, or accept]
- If the full mature target is too large, complete one minimum deliverable phase, then immediately enter the next phase and continue. Do not stop just because a phase is complete.

7. Stop Conditions
- The next step cannot be derived from the goal, evidence, or safety constraints and continuing would only be guessing.
- Stop for confirmation before deleting core files, changing global config, handling secrets, running destructive commands, unauthorized external writes, or out-of-goal actions.
- Project rules conflict with the target.
- Validation fails for two consecutive loops and the user must choose a tradeoff.
- The whole goal has reached its mature completion criteria and there is no safe, high-value next step left.
```

## Field Writing Rules

Write every field as an execution instruction, not a vague description.

| Field | Must answer | Good | Weak |
| --- | --- | --- | --- |
| Opening target | What target is the agent being given? | `现在给你一个目标：把当前项目推进到可运行、可验证、可维护的成熟版本。` | `Goal: Improve` |
| Intent and evidence | Why this target, now? | cites current conversation and visible evidence | generic project summary |
| Mature state | What should the project become? | names audience, problem, capability, and completion state | "make it better" |
| Execution map | How can the agent work for a long time without drifting? | near-term goals, mid-term direction, long-term possibilities | one short task list |
| Scope | What can be inspected or changed? | names modules, docs, commands, artifacts | "the project" |
| Non-Goals | What tempting work is excluded? | no deploy, no unrelated refactor, no push | omitted |
| Constraints | What must not be violated? | safety, project rules, secrets, external writes | generic "be careful" |
| Execution loop | What loop will the AI repeat? | observe -> decide -> act -> validate -> review -> continue | unordered checklist |
| Validation | How is done proven? | commands, files, screenshots, source list, metrics | "check it works" |
| Completion | What mature state can the user observe? | reproducible path, usable deliverable, next phase | "task complete" |
| Stop Conditions | When must the AI ask? | secrets, destructive commands, external writes, conflict | absent |

## Output Brevity Rules

- Do not output a project audit report unless the user explicitly asks for an audit report.
- Do not list every file inspected, every command run, graph node counts, test counts, or git status as the main answer.
- Do not assign project scores such as `8.4 / 10`.
- Do not wrap the final prompt in a long recommendation memo.
- Put execution work for the receiving AI inside the prompt instead of performing it during goal generation.
- If the user says `分析整个项目`, interpret it as "generate a long-horizon prompt that tells an AI how to analyze and advance the whole project", not as permission to produce a long audit.

## Validation Ladder

Choose the strongest validation level available without violating scope.

1. Static evidence: read files, inspect config, verify docs or metadata.
2. Local command: lint, typecheck, build, test, script dry run, parser check.
3. Runtime smoke: start app, hit health endpoint, open file, screenshot UI, sample output.
4. Artifact review: generated document, export, report, dataset sample, release package.
5. External confirmation: final URL, GitHub issue/PR, deployment health, cloud object, email/message sent.

Use level 5 only when the user explicitly authorized that external write. If level 2 or 3 is unavailable, include a fallback manual review and explain what blocked stronger validation.

## Stop Conditions By Risk

| Risk | Stop condition wording |
| --- | --- |
| Destructive filesystem | Stop before deleting, overwriting, or bulk-moving core files unless explicitly approved. |
| Git history | Stop before `git reset --hard`, forced checkout, rebase, force push, or broad staging unless explicitly approved. |
| Secrets | Stop before reading, printing, committing, rotating, or hard-coding credentials. |
| External write | Continue when the goal explicitly authorizes the write path and access exists; otherwise stop before deploy, publish, upload, database write, email/message send, payment action, package release, or API mutation. |
| Global machine | Stop before changing shell profiles, scheduled tasks, services, package manager config, system proxy, or account settings. |
| Scope conflict | Stop when project rules or evidence conflict with the user's stated target. |
| Validation blocked | Stop or summarize when required verification cannot run and no safe fallback exists. |

## Prompt Snippets

### Candidate Prompt

```text
先别改，先生成goal。
如果目标方向有冲突、高风险或缺少会阻塞执行的关键信息，请给出 2-3 个候选方向；否则直接生成一个首选可执行 goal。
如果你输出的是候选方向，等我确认后再生成最终 goal。
```

### Direct Final Prompt

```text
把这个目标转成goal：[目标内容]。
请先基于当前对话提取用户目标、约束、偏好、已确认信息和隐含交付物，再结合当前项目状态生成一个可以直接复制粘贴给 AI 执行的长期 goal 提示词。开头必须是“现在给你一个目标：xxx”，并明确“在这个目标完成之前，不允许停止工作”。提示词要包含真实目标分析、项目状态分析、项目终极目的、长期执行地图、持续执行循环、验证标准、完成标准和停止条件。不要默认假设有 GitHub；提交、推送、issue、PR、发布只有在我明确要求时才写入 goal。
```

### Product Idea Prompt

```text
把这个 idea 转成一个可执行 goal：[idea 内容]。
请不要假设已经有项目代码。先从当前对话提取目标用户、问题场景、偏好、约束和隐含交付物，再判断它可能演化成什么长期项目；如果适合继续，请生成一个面向任意 AI 智能体的长期执行提示词。开头必须是“现在给你一个目标：xxx”，包含目标用户、问题场景、最小可落地切片、长期演进地图、非目标、操作边界、验证标准、完成标准和停止条件。
```

### Lightweight Candidate Prompt

```text
先别改，先生成goal。
这个方向确实需要比较，请只给首选方向和一个备选方向，并说明为什么。等我确认后再生成最终 goal。
```

### Fallback Prompt

```text
把这个目标转成goal：[目标内容]。
如果项目证据不足、README 缺失、入口不清楚或验证命令未知，不要编造。请生成一个低风险 discovery-first 的长期执行提示词，把补齐证据、判断项目终极目的和建立长期执行地图作为第一轮任务，并列出停止条件。
```

## Domain Minimums

Software / code:

- Background names project type, entry point, related modules, and unknowns.
- Validation includes runnable commands, tests, build, lint/typecheck, smoke check, or a fallback manual check.
- Stop conditions cover destructive git, core file deletion, dependency installation, and unauthorized external release.

Frontend / UI:

- Background names target workflow and affected view/component.
- Scope includes responsive states and interaction states when relevant.
- Validation includes browser smoke, screenshot, layout/text overlap check, accessibility basics, or manual review.
- Non-goals exclude redesigns not requested by the user.

Product idea:

- Background names target user, problem scenario, smallest product slice, and key assumptions.
- Validation includes user evidence, prototype/copy/flow artifact, success standard, and next gate.
- Non-goals avoid full product build, branding, pricing, launch, and long roadmap unless requested.

Research / writing:

- Background names question, source range, output format, and evidence requirements.
- Validation includes source list, links/citations, conclusion boundaries, and uncertainty notes.
- Stop conditions cover inaccessible sources, evidence conflict, paywall/login, and sensitive data.

Data / automation:

- Background names input, output, data range, runtime environment, and failure modes.
- Validation includes dry run, sample output, logs, idempotency, rollback, or failure handling.
- Stop conditions cover batch writes, deletion, external API writes, and privacy leakage.

Deployment / release:

- Background names target environment, audience, version, permission, and rollback path.
- Validation includes build artifact, health check, final URL/path, visibility, and release notes.
- Stop conditions cover unauthorized production changes, missing account authority, billing risk, and irreversible release.

## Anti-Patterns

| Anti-pattern | Why it fails | Fix |
| --- | --- | --- |
| "Improve the project" | no observable end state | define one deliverable and validation |
| "Inspect everything" | wastes time and ignores user intent | inspect evidence tied to scope |
| "Ask before every unknown" | prevents one-step goal compilation | include discovery in the goal unless critical |
| "Default to GitHub" | assumes external write and permissions | local delivery unless requested |
| "Small task only" | loses autonomous value | long-horizon execution map and repeatable loops |
| "Audit report instead of prompt" | user cannot paste it into another AI as the goal | output one short preface plus the copy-pasteable prompt |
| "Ran the whole repo analysis while generating the goal" | wastes time and buries the actual prompt | do only orientation; put deep analysis steps inside the prompt |
| "Phase complete, so stop" | long-horizon goals die after 10 minutes | review the phase and immediately enter the next safe phase |
| "Analysis report complete, so stop" | the project did not advance beyond diagnosis | pick the highest-value safe finding and execute it next |
| "Read-only analysis complete, so stop" | the agent only described work instead of doing it | execute the safest high-value next step unless the goal explicitly says read-only only |
| "No non-goals" | invites scope creep | list tempting exclusions |
| "Validation: manual check" only | too weak when commands exist | use strongest safe validation level |
| "Deploy if needed" | unauthorized external write | stop before deploy unless explicitly authorized |

## Final Quality Checklist

Before returning a final goal, confirm:

- Current conversation appears in the background.
- Latest correction is honored.
- Output starts as a copy-pasteable AI prompt.
- Chinese output starts with `现在给你一个目标：`; English output starts with `I am giving you a goal:`.
- Prompt explicitly says not to stop until the goal is complete or a stop condition is triggered.
- Prompt explicitly says phase completion, minimum-loop completion, and next-phase suggestions are not stopping reasons.
- Prompt explicitly says analysis reports, dev records, indexes, roadmaps, and codebase maps are intermediate artifacts, not stopping reasons.
- Prompt explicitly says read-only analysis is only orientation unless the goal explicitly says read-only only.
- Prompt treats commit/push/deploy/release as normal execution when the goal explicitly authorizes them and access exists.
- Anything outside the prompt is no more than one sentence or 3 short evidence bullets.
- Output does not include project ratings, long audit logs, graph/test/git command summaries, or old-style `Goal: [type]` documents.
- Goal has one primary long-horizon target.
- Project ultimate purpose or mature state is analyzed before execution.
- Long-term execution map is included for broad project goals.
- Scope and non-goals are both explicit.
- Delivery path does not assume external systems.
- Steps form repeatable observe -> decide -> act -> validate -> review -> continue loops.
- Validation uses the strongest safe ladder level.
- Stop conditions cover destructive, credential, external-write, global, and conflict risks.
- Completion standard is observable by the user.
