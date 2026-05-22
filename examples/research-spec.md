# Example: Research / Spec

## Input

```text
把这个目标转成goal：我还不确定这个插件应该支持哪些使用场景。先不要实现，请先调研现有文档、用户反馈和类似工具，产出一份可执行规格。
```

## Expected Behavior

- Use `Research / Spec`.
- Treat the current conversation as the first input source: the user wants evidence and a buildable spec, not implementation.
- Include source/evidence boundaries and uncertainty notes.
- Exclude code changes, release, commit, push, and product build.

## Good Output Shape

```text
Goal: Research / Spec

项目背景：
- 当前对话中，用户不确定插件应支持哪些使用场景，明确要求先调研和写规格，不实现。

目标：
- 基于现有文档、用户反馈和类似工具，产出一份可执行规格，包含目标用户、核心场景、功能边界、验证标准和后续实现建议。

范围：
- 检查 README、现有 docs、issues/反馈记录、examples、竞品或类似工具公开资料。

非目标：
- 不写代码、不重构、不发布、不提交推送。

验证标准：
- 规格中列出来源清单、关键证据、结论边界和仍不确定的问题。
```

## Bad Output / Anti-Pattern

```text
Goal: Add / Extend

直接实现所有可能的使用场景，顺手重构插件架构。
```

Why it is bad:

- Skips the requested evidence/spec phase.
- Turns uncertainty into implementation scope.
- Adds refactor work the user did not request.
