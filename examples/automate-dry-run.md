# Example: Automate

## Input

```text
把这个目标转成goal：我每周都要手动整理导出的 CSV，去重、规范列名、生成摘要。请做成可重复运行的本地脚本，先支持 dry run。
```

## Expected Behavior

- Use `Automate`.
- Include input/output, dry run, idempotency, logs, and failure handling.
- Stop before destructive writes or batch overwrites unless authorized.
- Keep the first loop local.

## Good Output Shape

```text
Goal: Automate

项目背景：
- 当前对话中，用户反复手动整理 CSV：去重、规范列名、生成摘要。
- 用户要求做成本地可重复运行脚本，并先支持 dry run。

目标：
- 创建或完善一个本地脚本，对样例 CSV 做 dry run，输出去重、列名规范化和摘要预览。

范围：
- 检查现有 CSV 样例、脚本目录和输出约定。
- 实现 dry-run 路径、日志和样例输出。

非目标：
- 未经确认不覆盖原始 CSV、不批量移动文件、不上传外部系统。

验证标准：
- dry run 不修改原始文件。
- 样例输出展示将删除的重复项、列名映射和摘要内容。
- 日志说明输入、输出和失败原因。
```

## Bad Output / Anti-Pattern

```text
Goal: Automate

直接批量改写所有历史 CSV，并删除重复文件。
```

Why it is bad:

- Performs destructive batch writes before dry run.
- Lacks rollback and sample verification.
