# Goal Skills

[English](README.en.md)

> 别再让 AI 上来就干活，先让它学会写 goals。

`goal-skills` 是一个轻量的 Codex goal 提示词生成 skill。

它的目的很单一：针对某个项目、目录、问题或目标，先做通用判断，再生成更靠谱的 `/goal` 提示词。

## 什么时候用

这些说法应该触发它：

- `生成一个goal`
- `针对这个项目设置一个goal`
- `给这个项目设置一个goal`
- `为当前项目生成goal`
- `把这个目标转成goal`
- `先别改，先生成goal`

## 它做什么

它会判断当前更适合哪类 goal：

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

它不是项目管理模板包，不要求维护一堆状态文档。

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
```

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

## 安装

推荐用 skills CLI 安装：

```powershell
npx skills add https://github.com/DOIT-Ben/goal-skills -g -a codex claude-code -y --full-depth
```

本地开发时也可以在仓库根目录运行：

```powershell
npx skills add . -g -a codex claude-code -y --full-depth
```

`AGENTS.md` 只是这个技能包自己的说明，不要覆盖到其他项目根目录。
