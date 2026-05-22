# Example: Messy Project

## Input

```text
先别改，先生成goal。
这个项目文件很多，我不确定下一步该干什么。
```

## Expected Behavior

- Use `Discover / Map`.
- Treat the current conversation as the first input source: the user wants a goal, not file edits, and the project direction is unclear.
- Inspect only project signals such as README, rules, package files, folders, scripts, and current git state after preserving that conversation intent.
- Do not edit, format, commit, push, or create issues.
- Return one final discovery-first goal by default. Give candidates only if evidence reveals conflicting directions or a blocking missing input.

## Good Output Shape

```text
Goal: Discover / Map

项目背景：
- 当前对话中，用户明确要求“先别改，先生成goal”。
- 项目文件很多，下一步方向、入口和验证路径不清楚。

目标：
- 通过只读检查建立项目地图、关键入口、验证命令、当前风险和下一步建议。

执行步骤：
1. 读取项目规则、README、目录、脚本和可见产物。
2. 梳理项目结构、入口、运行/测试/构建路径和阻塞点。
3. 给出 2-3 个后续可执行 goal 建议。
4. 总结证据、风险和建议优先级。
```

## Bad Output / Anti-Pattern

```text
Goal: Add / Extend

我会直接新增功能、整理目录、修复启动问题，并提交推送。
```

Why it is bad:

- Chooses feature work before understanding the project.
- Mixes discovery, repair, refactor, and publishing in one goal.
- Starts changing files even though the user said "先别改".
- Assumes commit/push without permission.
