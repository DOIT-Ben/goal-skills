# Example: Add / Extend

## Input

```text
把这个目标转成goal：现有本地工具已经能导入 CSV。请新增一个 JSON 导入入口，并复用现有校验逻辑。不要重构整体结构。
```

## Expected Behavior

- Use `Add / Extend`.
- Preserve the current conversation constraint: add one capability, no broad refactor.
- Include a validation path for both existing CSV behavior and new JSON behavior.
- Exclude unrelated UI redesign, dependency swaps, and publishing.

## Good Output Shape

```text
Goal: Add / Extend

项目背景：
- 当前对话中，用户说明现有工具已支持 CSV 导入。
- 用户只要求新增 JSON 导入入口，并复用现有校验逻辑。

目标：
- 在现有导入流程中新增 JSON 导入入口，并验证 JSON 与 CSV 都走同一套校验规则。

范围：
- 检查现有 CSV 导入模块、校验函数、测试或样例数据。
- 只修改新增 JSON 导入所需的文件。

非目标：
- 不重构整体结构、不重写校验系统、不做 UI 大改、不发布。

验证标准：
- JSON 样例能成功导入。
- 错误 JSON 能触发现有校验错误。
- CSV 旧路径仍可用。
```

## Bad Output / Anti-Pattern

```text
Goal: Refactor / Reorganize

重写整个导入架构，替换校验库，并重新设计全部页面。
```

Why it is bad:

- Violates the user's "不要重构整体结构".
- Turns one extension into broad architecture work.
- Risks breaking existing CSV behavior.
