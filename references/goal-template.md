# Goal Template

Use this reference when writing the final goal.

Start from the current conversation. The user's latest goal, constraints, preferences, corrections, confirmed facts, and implicit deliverables are the first input source. Project files and runtime evidence refine the goal; they should not erase fresh conversation intent.

## Final Goal Skeleton

Match the user's language. Use the Chinese skeleton for Chinese requests and the English skeleton for English requests.

### Chinese Skeleton

```text
Goal: [目标类型]

项目背景：
- [当前对话中用户真正想达成的结果、约束、偏好和已确认信息]
- [当前项目是什么]
- [当前状态和关键证据]

目标：
- [一个具体、可验证的结果]
- [如果是 idea，从目标用户、问题、最小产品切片和落地验证角度描述]

范围：
- [要检查或修改的文件、模块、资产、流程]

非目标：
- [明确不做的事情，避免跑偏]

交付方式：
- [本地验证 / 产物说明 / 提交 / PR / 发布；不要默认假设 GitHub]

约束：
- [项目规则、风格、安全边界、不要硬编码密钥、不要无关重构]
- 不删除、覆盖、批量移动核心源码、配置、数据、模型、产物或规则文件，除非用户明确要求。
- 不执行 `git reset --hard`、强制 checkout、递归删除、数据库清空、密钥轮换、批量格式化等高风险操作，除非用户明确批准。
- 不暴露、打印、提交或硬编码密钥、token、账号、凭据。
- 不改全局机器配置、系统服务、计划任务、shell 配置、外部账号，除非目标明确要求。
- 不把未要求的 commit、push、issue、PR、发布写入目标；GitHub 只在用户明确要求或项目交付方式明确依赖 GitHub 时使用。
- 不写入外部系统，包括云服务、部署平台、数据库、包注册表、邮件、消息工具、支付系统、公开网站或第三方 API，除非用户明确授权该交付路径。

执行步骤：
1. 先检查现状和关键证据。
2. 按最小完整闭环完成目标。
3. 做领域对应验证。
4. 总结变更、验证结果、剩余风险和下一步。

验证标准：
- [命令、产物、截图、人工检查、来源清单或指标]

完成标准：
- [用户能看到或复用的完成状态]

停止条件：
- [缺少关键输入、发现高风险操作、目标冲突、验证无法完成]
- 需要删除核心文件、改全局配置、处理密钥、执行破坏性命令、外部写入、提交推送、部署或发布时，如果用户没有明确授权，先停下来确认。
```

### English Skeleton

```text
Goal: [Goal Type]

Background:
- [User's actual goal, constraints, preferences, confirmed facts, and implicit deliverables from the current conversation]
- [What the project or idea is]
- [Current state and key evidence]

Goal:
- [One concrete, verifiable outcome]
- [For an idea, describe target user, problem, smallest product slice, and validation]

Scope:
- [Files, modules, assets, workflow, or evidence to inspect or change]

Non-Goals:
- [What not to do]

Delivery:
- [Local validation / artifact / summary / commit / PR / release; do not assume GitHub]

Constraints:
- [Project rules, style, safety boundaries, no hard-coded secrets, no unrelated refactor]
- Do not delete, overwrite, or mass-move core source, config, data, models, deliverables, or rule files unless explicitly requested.
- Do not run high-risk commands such as `git reset --hard`, forced checkout, recursive delete, database wipe, secret rotation, or bulk formatting unless explicitly approved.
- Do not expose, print, commit, or hard-code secrets, tokens, accounts, or credentials.
- Do not change global machine config, system services, scheduled tasks, shell profiles, or external accounts unless the goal explicitly requires it.
- Do not include unrequested commit, push, issue, PR, or release steps.
- Do not write to external systems such as cloud services, deployment platforms, databases, package registries, email, messaging tools, payment systems, public websites, or third-party APIs unless explicitly authorized.

Steps:
1. Inspect current state and key evidence.
2. Complete the minimum complete loop.
3. Run domain-appropriate validation.
4. Summarize changes, validation, residual risks, and next steps.

Validation:
- [Commands, artifacts, screenshots, manual review, source list, or metrics]

Completion:
- [Observable done state]

Stop Conditions:
- [Missing critical input, high-risk operation, goal conflict, or failed validation]
- Stop for confirmation before deleting core files, changing global config, handling secrets, running destructive commands, external writes, commit/push, deployment, or release unless explicitly authorized.
```

## Field Writing Rules

Write every field as an execution instruction, not a vague description.

| Field | Must answer | Good | Weak |
| --- | --- | --- | --- |
| Goal type | What work mode is this? | `Goal: Repair / Restore` | `Goal: Improve` |
| Background | Why this goal, now? | cites current conversation and visible evidence | generic project summary |
| Goal | What single result will exist? | "Restore local startup and verify `/health`" | "Improve startup" |
| Scope | What can be inspected or changed? | names modules, docs, commands, artifacts | "the project" |
| Non-Goals | What tempting work is excluded? | no deploy, no unrelated refactor, no push | omitted |
| Delivery | Where does the result land? | local summary, artifact path, commit only if requested | assumes GitHub |
| Constraints | What must not be violated? | safety, project rules, secrets, external writes | generic "be careful" |
| Steps | What loop will the AI follow? | inspect -> implement/compose -> validate -> summarize | unordered checklist |
| Validation | How is done proven? | commands, files, screenshots, source list, metrics | "check it works" |
| Completion | What can the user observe? | reproducible path or readable deliverable | "task complete" |
| Stop Conditions | When must the AI ask? | secrets, destructive commands, external writes, conflict | absent |

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
| External write | Stop before deploy, publish, upload, database write, email/message send, payment action, package release, or API mutation. |
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
请先基于当前对话提取用户目标、约束、偏好、已确认信息和隐含交付物，再结合当前项目生成一个可执行 goal。包含背景、目标、范围、非目标、交付方式、操作约束、执行步骤、验证标准、完成标准和停止条件。默认形成调研/实现/验证/总结的最小完整闭环。不要默认假设有 GitHub；提交、推送、issue、PR、发布只有在我明确要求时才写入 goal。
```

### Product Idea Prompt

```text
把这个 idea 转成一个可执行 goal：[idea 内容]。
请不要假设已经有项目代码。先从当前对话提取目标用户、问题场景、偏好、约束和隐含交付物，再判断它是否属于从想法到最小产品切片的目标；如果是，请生成一个面向任意 AI 智能体的 goal，包含目标用户、问题场景、最小可落地切片、非目标、操作边界、验证标准、完成标准和停止条件。
```

### Lightweight Candidate Prompt

```text
先别改，先生成goal。
这个方向确实需要比较，请只给首选方向和一个备选方向，并说明为什么。等我确认后再生成最终 goal。
```

### Fallback Prompt

```text
把这个目标转成goal：[目标内容]。
如果项目证据不足、README 缺失、入口不清楚或验证命令未知，不要编造。请生成一个低风险 discovery-first goal，把补齐证据作为第一步，并列出停止条件。
```

## Domain Minimums

Software / code:

- Background names project type, entry point, related modules, and unknowns.
- Validation includes runnable commands, tests, build, lint/typecheck, smoke check, or a fallback manual check.
- Stop conditions cover destructive git, core file deletion, dependency installation, and external release.

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
- Stop conditions cover production changes, external publishing, account authority, billing risk, and irreversible release.

## Anti-Patterns

| Anti-pattern | Why it fails | Fix |
| --- | --- | --- |
| "Improve the project" | no observable end state | define one deliverable and validation |
| "Inspect everything" | wastes time and ignores user intent | inspect evidence tied to scope |
| "Ask before every unknown" | prevents one-step goal compilation | include discovery in the goal unless critical |
| "Default to GitHub" | assumes external write and permissions | local delivery unless requested |
| "Small task only" | loses autonomous value | minimum complete loop |
| "No non-goals" | invites scope creep | list tempting exclusions |
| "Validation: manual check" only | too weak when commands exist | use strongest safe validation level |
| "Deploy if needed" | unauthorized external write | stop before deploy unless explicitly authorized |

## Final Quality Checklist

Before returning a final goal, confirm:

- Current conversation appears in the background.
- Latest correction is honored.
- Goal has one primary outcome.
- Scope and non-goals are both explicit.
- Delivery path does not assume external systems.
- Steps form inspect/research -> implement/compose -> validate -> summarize.
- Validation uses the strongest safe ladder level.
- Stop conditions cover destructive, credential, external-write, global, and conflict risks.
- Completion standard is observable by the user.
