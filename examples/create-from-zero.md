# Example: Create From Zero

## Input

```text
把这个目标转成goal：这个目录现在只有几条笔记。请从零做一个能本地打开的最小可用 HTML 工具，用来整理我的 prompt 资产。
```

## Expected Behavior

- Use `Create From Zero`.
- Treat notes as starting evidence, not as a full app specification.
- Generate a goal for the first usable local artifact.
- Keep the scope to one smallest working loop.

## Good Output Shape

```text
Goal: Create From Zero

项目背景：
- 当前目录只有笔记，没有可运行工具。
- 用户希望从零生成一个可本地打开的最小 HTML 工具，用来整理 prompt 资产。

目标：
- 创建一个本地可打开的最小 HTML 工具，支持录入、查看和简单筛选 prompt 资产。

范围：
- 读取现有笔记，提取最小字段。
- 生成一个静态 HTML/CSS/JS 产物或等价本地文件。

非目标：
- 不做后端、登录、云同步、数据库、发布或复杂标签体系。

验证标准：
- 本地打开 HTML 后，核心录入和查看流程可用。
- 总结产物路径和手动检查结果。
```

## Bad Output / Anti-Pattern

```text
Goal: Release / Publish

直接搭建完整 SaaS，接入账号、云数据库、支付和线上部署。
```

Why it is bad:

- Overbuilds from a notes-only starting point.
- Adds external services and release scope.
- Misses the smallest useful local artifact.
