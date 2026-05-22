# Goal Skills

> 如果你的 token 预算还算宽裕，那就来玩 goal 吧。
> 别让 AI 只会接话，让它把事做完。

[English](README.en.md)

`goal-skills` 是一个面向所有 AI 智能体的 goal 编译器：它从当前对话、项目现状或产品想法中提取目标、边界和验证标准，生成一个 AI 可以自主执行到交付结果的 goal。

包名保留为 `goal-skills`，但公开入口只有一个：`goal-skills`。

## 一句话

把用户刚刚说清楚的目标、约束、偏好和隐含交付物，编译成一份边界清楚、可验证、能形成最小完整闭环的 AI 任务目标。

不同智能体的执行机制不同；这个 skill 负责生成边界清晰、可验证、可迁移的 goal 任务书，不把能力边界绑定到某一个平台。

它不只适合已有项目，也适合从一个 idea 开始，把想法变成可以一次推进到可检查结果的 agent goal。

## 什么是 Agent Goal

这里的 goal 是给智能体的可执行任务书：你给 AI 一个可验证目标，它就能围绕这个目标持续规划、执行、测试、复盘和迭代，直到目标完成、预算耗尽，或者你手动停止。不同工具的入口不同：在 Codex 里可以作为 `/goal` 使用，在 Claude Code、Cursor、Devin、自建 agent 或其他能读取 Markdown/规则文件的智能体里，可以作为任务 brief 或执行约束使用。

所以 goal 不是普通聊天里的“帮我修一下”，而是更像一份任务书：

- 要达成什么结果
- 哪些范围可以动
- 哪些事情不要做
- 怎么验证已经完成
- 什么时候应该停下来问用户

## 生成 goal 之后会发生什么

你把一个 goal 交给智能体之后，它不再只是回答一句话，而是会围绕这个目标持续推进。这个 skill 的作用，是在开始前把这件事写清楚：

- 它要先做什么
- 它可以改什么、不能改什么
- 它要用什么命令或证据验证
- 它什么时候应该停下来问你

所以 goal 的价值不在于“写得漂亮”，而在于把模糊任务变成可执行、可验证、可收口的工作单。

## goal 有什么用

你可以把 goal 当成给 AI 的工作边界和完成标准：

- 先摸清项目，避免一上来乱改
- 把修复、重构、发布、自动化、评估编译成能闭环完成的任务
- 让 AI 知道哪些动作要停下来确认，比如删文件、改全局配置、推送、发布、写外部系统
- 让最后结果可验证，而不是只停留在“我觉得差不多了”

## 为什么先生成 goal

AI 很容易一上来就热情开工，结果先忙半天，方向却没对齐。

先生成 goal，就是先把方向、边界、验证标准和停止条件写清楚。这样智能体开始执行后，才更像一个会持续推进的 AI 员工，而不是一个边做边猜的代码补丁机。

这里的“持续执行”不是不择手段。一个好的 goal 也应该写清楚操作边界：不能删核心文件、不能乱改全局配置、不能泄露密钥、不能默认提交推送，遇到高风险动作要停下来确认。

## 这个 skill 做什么

`goal-skills` 的第一输入源是当前对话：用户刚刚表达的目标、约束、偏好、已确认信息、最新修正和隐含交付物。项目文件、目录和运行证据用于校准这个目标，而不是覆盖当前对话里的需求。

`goal-skills` 会先判断当前输入更适合哪类 goal：

- Discover / Map：先摸清项目
- Research / Spec：先调研或写规格
- Create From Zero：从零创建
- Add / Extend：增加能力
- Repair / Restore：修复坏掉的东西
- Refactor / Reorganize：重构或整理结构
- Polish / Improve：打磨质量
- Automate：自动化重复流程
- Experiment / Evaluate：实验、比较、评估
- Release / Publish：发布、交付、导出、归档
- Idea / Product Slice：从产品 idea 到最小可落地切片

然后默认生成一个首选可执行 goal。只有目标冲突、风险较高、缺少关键输入，或用户明确要求比较方向时，才先给候选方向让用户选择。

它不是项目管理模板包，不要求维护一堆状态文档。

它也不会默认假设你有 GitHub。没有 GitHub 时，goal 会以本地验证、产物、日志或总结作为交付方式；只有你明确要求提交、推送、issue、PR 或发布时，才会把 GitHub 步骤写进 goal。

如果输入是产品 idea，`goal-skills` 只负责把 idea 转成一个可执行的最小产品切片 goal。更深的产品澄清、判断、计划、QA 和发布交接，可以后续选择 [products-skills](https://github.com/DOIT-Ben/products-skills)。

## 触发口径

这些说法应该触发它：

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

这不是泛用计划模板；只有当用户明确想生成 goal、智能体可执行任务、把任务转成 goal，或要求“先定目标再改动”时才触发。`Codex goal`、`Claude Code goal`、`/goal prompt` 只是兼容触发词，不是能力边界。

## 推荐用法

一步生成首选 goal：

```text
$goal-skills

把这个目标转成goal：修复这个项目的本地启动流程，并验证 health 接口。
请生成最终 goal，不要提交推送。
```

需要比较方向时再让它给候选：

```text
$goal-skills

先别改，先生成goal。
这个项目方向不确定，请先给 2-3 个候选方向和推荐选择。
```

如果你只有一个产品想法：

```text
$goal-skills

把这个目标转成goal：我想做一个帮助独立开发者把想法拆成可执行产品计划的工具。
请先按 idea 到落地的方式生成 goal。
```

## 安装

这个 repo 的核心是通用 Markdown skill：任何能读取 `SKILL.md`、规则文件或任务 brief 的智能体都可以使用。Codex 和 Claude Code 只是下面给出的两个安装适配示例。

Codex 安装示例：

```powershell
npx skills add https://github.com/DOIT-Ben/goal-skills -g -a codex -y --full-depth
```

本地开发时也可以在仓库根目录运行：

```powershell
npx skills add . -g -a codex -y --full-depth
```

Claude Code 安装示例：

```powershell
npx skills add https://github.com/DOIT-Ben/goal-skills -g -a claude-code -y --full-depth
```

其他智能体可以直接读取 [SKILL.md](SKILL.md)、[references](references/) 和 [examples](examples/README.md)。如果某个平台没有 skills CLI，直接把 `SKILL.md` 作为规则或工具说明导入即可。

## 例子

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

## 文件结构

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
