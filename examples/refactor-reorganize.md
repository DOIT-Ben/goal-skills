# Example: Refactor / Reorganize

## Input

```text
把这个目标转成goal：这个工具能跑，但导入、解析、保存都挤在一个文件里，后面很难改。请只做结构整理，保持行为不变。
```

## Expected Behavior

- Use `Refactor / Reorganize`.
- Make behavior preservation explicit.
- Include before/after validation or equivalence checks.
- Exclude feature additions, visual redesign, and dependency swaps.

## Good Output Shape

```text
Goal: Refactor / Reorganize

项目背景：
- 当前工具能运行，但导入、解析、保存逻辑集中在一个文件里，影响后续修改。
- 用户明确要求只做结构整理，保持行为不变。

目标：
- 将导入、解析、保存职责拆分到清晰模块，同时保持现有用户可见行为不变。

范围：
- 检查当前入口文件、导入逻辑、解析逻辑、保存逻辑和现有验证路径。
- 仅移动或提取与职责拆分直接相关的代码。

非目标：
- 不新增功能、不改 UI、不换依赖、不发布、不提交推送。

验证标准：
- 重构前后同一输入得到同一输出。
- 现有测试、构建或手动 smoke check 通过。
```

## Bad Output / Anti-Pattern

```text
Goal: Add / Extend

顺便加 JSON 导入、批量编辑和云同步。
```

Why it is bad:

- Adds behavior when the user asked to preserve behavior.
- Makes refactor risk much harder to verify.
