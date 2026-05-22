# Goal Template

Use this reference when writing the final `/goal`.

## Final Goal Skeleton

Match the user's language. Use the Chinese skeleton for Chinese requests and the English skeleton for English requests.

### Chinese Skeleton

```text
/goal [目标类型]

项目背景：
- [当前项目是什么]
- [当前状态和关键证据]
- [用户真正想达成的结果]

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
2. 按最小闭环完成目标。
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
/goal [Goal Type]

Background:
- [What the project or idea is]
- [Current state and key evidence]
- [What the user actually wants to achieve]

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
2. Complete the smallest useful loop.
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

## Candidate Prompt

```text
先别改，先生成goal。
请检查项目现状，判断最适合的下一步 goal 类型，给出 2-3 个候选方向，并说明每个方向会做什么、不会做什么、怎么验证。
等我确认方向后，再生成最终 /goal。
```

## Direct Final Prompt

```text
把这个目标转成goal：[目标内容]。
请结合当前项目生成一个可执行 /goal，包含背景、目标、范围、非目标、交付方式、操作约束、执行步骤、验证标准、完成标准和停止条件。不要默认假设有 GitHub；提交、推送、issue、PR、发布只有在我明确要求时才写入 goal。
```

## Product Idea Prompt

```text
把这个 idea 转成一个可执行 /goal：[idea 内容]。
请不要假设已经有项目代码。先判断它是否属于从想法到最小产品切片的目标；如果是，请生成一个面向 Codex Goal 模式的 /goal，包含目标用户、问题场景、最小可落地切片、非目标、操作边界、验证标准、完成标准和停止条件。
```

## Lightweight Candidate Prompt

```text
先别改，先生成goal。
这是一个小任务，请只给首选方向和一个备选方向，并说明为什么。等我确认后再生成最终 /goal。
```

## Fallback Prompt

```text
把这个目标转成goal：[目标内容]。
如果项目证据不足、README 缺失、入口不清楚或验证命令未知，不要编造。请生成一个低风险 discovery-first /goal，把补齐证据作为第一步，并列出停止条件。
```

## Domain Minimums

Software / code:

- 背景要写明项目类型、入口、相关模块或未知项。
- 验证标准至少包含可运行命令、测试、构建、lint/typecheck、smoke check 或无法验证时的替代人工检查。
- 停止条件要覆盖破坏性 git、核心文件删除、依赖安装和外部发布。

Product idea:

- 背景要写明目标用户、问题场景、最小可落地切片和关键假设。
- 验证标准至少包含用户证据、原型/文案/流程产物、成功标准和下一道 gate。
- 非目标要避免直接做全量产品、品牌包装或发布。

Research / writing:

- 背景要写明问题、来源范围、输出形式和证据要求。
- 验证标准至少包含来源清单、引用/链接、结论边界和不确定性说明。
- 停止条件要覆盖来源不可访问、证据冲突或需要付费/登录/敏感数据。

Data / automation:

- 背景要写明输入、输出、数据范围、运行环境和失败模式。
- 验证标准至少包含 dry run、样例输出、日志、回滚或失败处理。
- 停止条件要覆盖批量写入、删除、外部 API 写操作和隐私数据外泄。

Deployment / release:

- 背景要写明目标环境、交付对象、版本、权限和回退路径。
- 验证标准至少包含构建产物、健康检查、最终 URL/产物路径和可见性确认。
- 停止条件要覆盖生产变更、外部发布、账号权限、账单风险和不可逆发布。
