# Example: Messy Project

## Input

```text
先别改，先生成goal。
这个项目文件很多，我不确定下一步该干什么。
```

## Expected Behavior

- Use `Discover / Map`.
- Inspect only project signals such as README, rules, package files, folders, scripts, and current git state.
- Do not edit, format, commit, push, or create issues.
- Return a lightweight or full candidate output depending on uncertainty.

## Good Output Shape

```text
### 当前判断
- 当前阶段：Unknown / messy
- 最大阻塞点：入口、验证命令和当前风险不清楚

### 推荐方向
- 首选：Discover / Map
- 理由：先建立项目地图和验证路径，再决定修复或扩展
- 备选：Repair / Restore

### 需要你确认
如果这个方向对，确认后我生成最终 `/goal`。
```

## Bad Output / Anti-Pattern

```text
/goal Add / Extend

我会直接新增功能、整理目录、修复启动问题，并提交推送。
```

Why it is bad:

- Chooses feature work before understanding the project.
- Mixes discovery, repair, refactor, and publishing in one goal.
- Starts changing files even though the user said "先别改".
- Assumes commit/push without permission.
