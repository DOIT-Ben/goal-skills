# Example: Polish / Improve

## Input

```text
把这个目标转成goal：核心功能已经能用，但 README 太像内部说明，用户看不懂。请把 README 改成面向安装和使用的公开文档。
```

## Expected Behavior

- Use `Polish / Improve`.
- Treat the current conversation as a documentation polish request, not a code task.
- Include public-reader framing, installation, usage, examples, and caveats.
- Exclude feature changes and publishing.

## Good Output Shape

```text
Goal: Polish / Improve

项目背景：
- 当前对话中，用户说明核心功能已经可用，但 README 仍像内部说明。
- 目标读者是准备安装和使用该工具的公开用户。

目标：
- 将 README 改写为面向公开用户的安装和使用文档，让读者能理解工具用途、安装方式、典型用法和限制。

范围：
- 修改 README，必要时同步 README.en.md 或 examples 索引。

非目标：
- 不改代码、不新增功能、不提交推送、不发布。

验证标准：
- README 首屏清楚说明定位和入口。
- 安装命令、典型使用方式、示例和限制说明完整。
- 文档不泄露个人路径、账号或私有信息。
```

## Bad Output / Anti-Pattern

```text
Goal: Add / Extend

新增功能、改 API，并顺手重写 README。
```

Why it is bad:

- Ignores that core function already works.
- Turns documentation polish into product implementation.
