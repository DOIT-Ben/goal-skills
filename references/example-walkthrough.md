# Worked Examples

Use this reference when output style is unclear or when testing whether `goal-skills` follows the conversation-first, one-step goal compiler behavior.

Each walkthrough shows:

1. the user trigger
2. what the agent should extract from the current conversation
3. what evidence matters
4. the expected output shape
5. the mistake the reference is meant to prevent

## Walkthrough 1: Messy Project To Final Discovery Goal

### User Trigger

```text
先别改，先生成goal。
这个项目文件很多，我不确定下一步该干什么。
```

### Conversation Brief

- Goal: generate a goal, not edit files.
- Constraint: no changes yet.
- Implicit deliverable: one executable discovery-first goal.
- Risk: project may be messy; avoid random cleanup or refactor.

### Relevant Evidence

- README exists but is outdated.
- `src/` and `scripts/` both contain entry points.
- There is no clear test command in README.
- `package.json` has `dev`, `build`, and `test`, but the last test run is unknown.
- There are local changes in the working tree.
- User did not ask for commit, push, issue creation, PR, or release.

### Good Output Shape

```text
Goal: Discover / Map

项目背景：
- 当前对话中，用户明确要求“先别改，先生成goal”。
- 项目文件很多，下一步方向、真实入口、可运行命令和当前风险不清楚。
- 用户没有要求 commit、push、issue 创建、PR 或 release。

目标：
- 通过只读方式摸清项目结构、入口、可运行命令、验证路径、当前风险，并提出 2-3 个下一步可执行 goal。

范围：
- 检查 README、AGENTS.md、package.json、src/、scripts/、配置文件、测试目录、最近日志或错误文件。
- 可以运行只读或低风险命令，如 `git status -sb`、`git log --oneline -5`、`npm run` 或等价命令列表。

非目标：
- 不修改代码、配置、依赖、格式或文档。
- 不新增功能、不重构、不发布。
- 不提交、不推送、不创建 issue 或 PR。

交付方式：
- 本地总结报告，不依赖 GitHub。

执行步骤：
1. 检查项目规则、README、目录结构和包管理/构建配置。
2. 梳理入口、脚本、测试和验证命令。
3. 判断当前阶段、主要风险和最可能的阻塞点。
4. 输出 2-3 个下一步 goal 候选，说明原因、范围、非目标和验证方式。

验证标准：
- 报告中列出实际检查过的文件和命令。
- 每个下一步候选 goal 都有明确范围、验证方式和风险判断。

停止条件：
- 需要修改文件、安装依赖、执行破坏性命令、访问凭据、外部写入、提交或推送时，立即停止并询问用户。
```

### Prevents

- Returning only candidates when a discovery-first final goal is enough.
- Editing files even though the user said "先别改".
- Assuming GitHub delivery.

## Walkthrough 2: Current Conversation Overrides Repo Drift

### User Trigger

```text
goal-skills。把当前想法转成 goal：
我已经确认这轮只做本地文档和示例更新，不改代码、不提交、不推送。
目标是把这个工具定位成 goal compiler，而不是 prompt helper。
```

### Conversation Brief

- Goal: reframe the package as `goal compiler`.
- Constraints: docs/examples only; no code, commit, push.
- Preference: current conversation is authoritative.
- Implicit deliverables: README, skill docs, references, examples, evals, metadata if it is documentation/packaging.
- Latest correction: "not prompt helper".

### Relevant Evidence

- README still says "prompt generator" or similar.
- `SKILL.md` may mention bounded goal prompt generation.
- `references/` may be thin and still analysis-first.
- Repo has git remote, but user explicitly excluded push.

### Good Output Shape

```text
Goal: Polish / Improve

项目背景：
- 当前对话中，用户已确认只做本地文档和示例更新，不改代码、不提交、不推送。
- 用户要求把工具定位为 goal compiler，而不是 prompt helper。
- 现有 README、SKILL.md、references、examples 或 metadata 可能仍保留旧定位。

目标：
- 将公开文档、skill 行为说明、reference、示例和 eval 中的定位统一为 conversation-first goal compiler，并验证本地 skill 仍可被发现。

范围：
- 可检查并修改 README、README.en.md、SKILL.md、references/、examples/、evals/、skill.json、插件元数据和 changelog。

非目标：
- 不改业务代码或无关文件。
- 不提交、不推送、不创建 PR、不发布。
- 不扩大成完整产品方法论。

交付方式：
- 本地文件更新、校验结果和变更总结。

执行步骤：
1. 检查当前文档和元数据中所有旧定位、两步确认倾向和 reference 缺口。
2. 将定位、默认行为、示例和 eval 统一到 goal compiler。
3. 运行 JSON 解析、关键断言、skills 发现校验。
4. 总结变更、验证结果和剩余风险。

验证标准：
- `evals/evals.json` 等 JSON 可解析。
- 关键断言覆盖 current conversation first、one preferred executable goal、minimum complete loop、goal compiler。
- `npx --yes skills add . --list --full-depth` 只发现 `goal-skills`。

停止条件：
- 需要改代码、提交推送、发布、外部写入、读取凭据或执行破坏性命令时先确认。
```

### Prevents

- Letting stale README wording override the user's latest product positioning.
- Saying "repo has GitHub, so include push".
- Treating "prompt helper" as an acceptable synonym after the user rejected it.

## Walkthrough 3: Product Idea To Minimum Complete Slice

### User Trigger

```text
把这个 idea 转成 goal：
我想做一个帮助独立开发者把想法拆成可执行产品计划的工具。
```

### Conversation Brief

- Goal: convert a product idea into an executable goal.
- Constraint: no existing code is implied.
- Implicit deliverable: product-slice goal, not full SaaS implementation.
- Risk: overbuilding product methodology, login, payment, launch, or roadmap.

### Relevant Evidence

- No repo evidence is required.
- If a repo exists, inspect only if the user says this idea belongs to that repo.
- The first useful loop is product clarification and slice definition, not implementation.

### Good Output Shape

```text
Goal: Idea / Product Slice

项目背景：
- 当前对话中，用户提出一个帮助独立开发者把想法拆成可执行产品计划的工具。
- 目前没有明确目标用户细分、核心场景、最小产品切片或验证方式。

目标：
- 产出一个可执行的最小产品切片说明，包含目标用户、问题场景、核心工作流、非目标、验证方式、成功标准和下一道 gate。

范围：
- 梳理用户、场景、输入输出、核心点击流或文档流。
- 形成一份 任意 AI 智能体可继续执行的产品切片目标。

非目标：
- 不直接实现完整产品。
- 不做登录、支付、团队协作、品牌、定价、发布和长期路线图。

执行步骤：
1. 提取当前想法中的用户、问题和预期结果。
2. 明确 1 个最小可落地切片和 1 个核心使用场景。
3. 写出输入、输出、成功标准、验证方式和下一道 gate。
4. 总结继续、调整或暂停的判断依据。

验证标准：
- 产物能回答谁在什么场景下用它解决什么问题。
- 产物包含最小切片、非目标、成功标准和验证 gate。
```

### Prevents

- Turning an idea into a full product build.
- Requiring `products-skills`.
- Skipping target user and evidence gate.

## Walkthrough 4: High-Risk Delivery Requires Candidate Or Confirmation

### User Trigger

```text
把这个目标转成 goal：
修复启动问题，然后直接部署到生产环境。如果数据库需要迁移也一起做。
```

### Conversation Brief

- Goal: repair startup.
- Requested delivery includes production deployment and possible database migration.
- Risk: external write, production change, database mutation.
- Critical missing input: authority, environment, rollback, migration safety.

### Good Output Shape

This should not silently compile a goal that deploys and migrates. Either ask one concise confirmation question or produce a goal that repairs locally and stops before external writes.

```text
Goal: Repair / Restore

项目背景：
- 当前对话中，用户希望修复启动问题，并提到生产部署和可能的数据库迁移。
- 部署和数据库迁移属于高风险外部写入，尚未确认权限、环境、回退路径和迁移范围。

目标：
- 先在本地修复并验证启动路径；整理部署和数据库迁移所需的检查清单，但不执行外部写入。

非目标：
- 未经确认不部署到生产。
- 未经确认不修改数据库、不运行迁移、不推送、不发布。

停止条件：
- 需要生产部署、数据库写入、迁移、凭据、外部服务写操作或不可逆操作时，先停下来确认。
```

### Prevents

- Treating "if needed" as permission for database writes.
- Mixing local repair and production release into one unchecked operation.
- Hiding risk inside the goal steps.

## Walkthrough 5: Missing Evidence Still Produces A Useful Goal

### User Trigger

```text
Convert this task to goal: audit the current project, but README and package files seem missing.
```

### Conversation Brief

- Goal: audit the current project.
- Known evidence gap: README and package files may be missing.
- Constraint: do not invent commands or structure.

### Good Output Shape

```text
Goal: Discover / Map

Background:
- The user wants an audit of the current project.
- README and package files may be missing, so project type, entry points, and validation path are low-confidence.

Goal:
- Build a read-only project map and identify the actual entry points, deliverables, validation options, and missing evidence.

Scope:
- Inspect visible rules, folder structure, config files, source/assets, scripts, logs, and generated outputs.

Non-Goals:
- Do not assume npm, pytest, build scripts, GitHub, or deployment.
- Do not edit files.

Validation:
- List inspected files and commands.
- Mark unknown validation paths as unknown instead of inventing them.

Stop Conditions:
- Stop before editing files, installing dependencies, destructive commands, credentials, external writes, commit, push, deploy, or publish.
```

### Prevents

- Inventing `npm test` or `pytest`.
- Pretending missing evidence is confirmed.
- Returning a vague "audit project" goal without a concrete deliverable.
