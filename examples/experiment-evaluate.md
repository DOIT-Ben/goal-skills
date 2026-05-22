# Example: Experiment / Evaluate

## Input

```text
把这个目标转成goal：我有两种分类方案，不知道哪个更好。请用现有样例做一个小实验，给出指标、结果表和推荐结论。
```

## Expected Behavior

- Use `Experiment / Evaluate`.
- Define baseline, metric, sample scope, reproducible command, and result artifact.
- Avoid choosing a winner before evidence.
- Exclude production changes and broad refactor.

## Good Output Shape

```text
Goal: Experiment / Evaluate

项目背景：
- 当前对话中，用户有两种分类方案，需要用现有样例比较优劣。

目标：
- 基于现有样例运行一个小实验，输出指标、结果表、误差分析和推荐结论。

范围：
- 检查两种分类方案、样例数据、可运行脚本和现有评估方式。
- 如无评估脚本，先创建最小本地评估流程。

非目标：
- 不替换生产方案、不大规模重构、不发布。

验证标准：
- 记录可复现实验命令、样例范围、指标定义和结果表。
- 推荐结论必须对应实验证据和适用边界。
```

## Bad Output / Anti-Pattern

```text
Goal: Add / Extend

直接把看起来更好的方案上线。
```

Why it is bad:

- Skips metric and comparison.
- Turns evaluation into unverified implementation.
