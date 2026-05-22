# Goal Skills

[English](README.en.md)

> 别再让 AI 上来就干活，先让它学会写 goals。

`goal-skills` 是一个轻量的 Codex `/goal` 提示词生成 skill。

包名保留为 `goal-skills`，但公开入口只有一个：`goal-skills`。

## 一句话

给 Codex Goal 模式准备一份靠谱的任务目标，让 AI 不只是回答你一句话，而是围绕一个可验证目标持续执行下去。

具体行为取决于 Codex 当前版本和运行环境；这个 skill 负责生成边界清晰、可验证的 `/goal`，不保证某个平台一定按同一种方式执行。

它不只适合已有项目，也适合从一个 idea 开始，把想法变成可以推进的 `/goal`。

## 什么是 Codex Goal

Codex 的 `/goal` 是一种自主任务模式：你给 AI 一个可验证目标，它通常会围绕这个目标持续规划、执行、测试、复盘和迭代，直到目标完成、预算耗尽，或者你手动停止。具体行为以你正在使用的 Codex 版本和运行环境为准。

所以 goal 不是普通聊天里的“帮我修一下”，而是更像一份任务书：

- 要达成什么结果
- 哪些范围可以动
- 哪些事情不要做
- 怎么验证已经完成
- 什么时候应该停下来问用户

## 为什么先生成 goal

AI 很容易一上来就热情开工，结果先忙半天，方向却没对齐。

先生成 goal，就是先把方向、边界、验证标准和停止条件写清楚。这样 Codex 进入 `/goal` 后，才更像一个会持续推进的 AI 员工，而不是一个边做边猜的代码补丁机。

这里的“持续执行”不是不择手段。一个好的 goal 也应该写清楚操作边界：不能删核心文件、不能乱改全局配置、不能泄露密钥、不能默认提交推送，遇到高风险动作要停下来确认。

## 这个 skill 做什么

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

然后生成一个适合放进 Codex `/goal` 的提示词。

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
- `set a goal for this project`
- `convert this into a goal`
- `Codex goal`
- `/goal prompt`
- `convert task to goal`
- `plan before change`

这不是泛用计划模板；只有当用户明确想生成 goal、Codex `/goal`、把任务转成 goal，或要求“先定目标再改动”时才触发。

## 推荐用法

先让它判断方向：

```text
$goal-skills

先别改，先生成goal。
请判断当前项目最适合的下一步 goal 类型，并给出 2-3 个候选方向。
```

确认方向后生成最终 goal：

```text
$goal-skills

选 A。请生成最终可执行 /goal。
```

如果你已经有明确目标：

```text
$goal-skills

把这个目标转成goal：修复这个项目的本地启动流程，并验证 health 接口。
请生成最终 /goal。
```

如果你只有一个产品想法：

```text
$goal-skills

把这个目标转成goal：我想做一个帮助独立开发者把想法拆成可执行产品计划的工具。
请先按 idea 到落地的方式生成 /goal。
```

## 安装

目前只验证了 Codex 安装；Claude / Claude Code 仍标记为 experimental。

推荐用 skills CLI 安装：

```powershell
npx skills add https://github.com/DOIT-Ben/goal-skills -g -a codex -y --full-depth
```

本地开发时也可以在仓库根目录运行：

```powershell
npx skills add . -g -a codex -y --full-depth
```

实验性 Claude Code 安装：

```powershell
npx skills add https://github.com/DOIT-Ben/goal-skills -g -a claude-code -y --full-depth
```

Claude / Claude Code 支持还没有作为正式兼容面验证。不同平台可能读取 `SKILL.md`、`skill.json` 或插件元数据的方式不同；如果触发不稳定，请显式使用 `$goal-skills` 或包含 `/goal prompt`、`Codex goal` 这类触发词。

## 文件结构

```text
goal-skills/
  README.md
  README.en.md
  SKILL.md
  skill.json
  AGENTS.md
  references/
    goal-judgment-matrix.md
    goal-template.md
    example-walkthrough.md
  examples/
    messy-project.md
    broken-app.md
    product-idea.md
    direct-repair.md
  evals/
    evals.json
```

`AGENTS.md` 只是这个技能包自己的说明，不要覆盖到其他项目根目录。
