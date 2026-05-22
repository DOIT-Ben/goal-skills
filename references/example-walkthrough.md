# Worked Example: Messy Project To Final Goal

This walkthrough shows how `goal-skills` should behave when the user points at a messy project and asks for a goal.

## User Trigger

```text
先别改，先生成goal。
这个项目文件很多，我不确定下一步该干什么。
```

## Context The Agent Inspects

Example evidence:

- README exists but is outdated.
- `src/` and `scripts/` both contain entry points.
- There is no clear test command in README.
- `package.json` has `dev`, `build`, and `test`, but the last test run is unknown.
- There are local changes in the working tree.
- User did not ask for commit, push, issue creation, PR, or release.

## Candidate Direction Output

```text
### 当前判断
- 项目/任务类型：软件 / 工具项目
- 当前阶段：Unknown / messy
- 最大不确定点：真实入口、可运行命令、当前坏点还不清楚
- 最大阻塞点：没有可靠基线，直接新增或重构容易跑偏

### 推荐方向
- 首选：Discover / Map
- 原因：先建立项目地图、运行入口、验证命令和风险清单，再决定修复或扩展
- 不建议现在做：Add / Extend、Refactor / Reorganize、Release / Publish

### 候选 Goals

#### A. Discover / Map
- 适合原因：项目结构混乱且缺少可靠基线
- 会做什么：只读检查目录、README、package scripts、入口、测试命令、最近错误，输出下一步建议
- 不会做什么：不改代码、不格式化、不提交、不推送
- 预期结果：得到项目地图、可运行命令、当前风险和下一个最小 goal
- 验证方式：列出实际检查过的文件和命令；必要时只运行只读或低风险命令
- 风险：低

#### B. Repair / Restore
- 适合原因：如果启动或测试已经明确失败，可以优先修复基线
- 会做什么：定位一个阻塞性失败并修复到可验证通过
- 不会做什么：不做新功能、不重构无关模块、不发布
- 预期结果：恢复本地最小运行或测试路径
- 验证方式：运行对应 start/build/test 或 smoke check
- 风险：中

### 需要你确认
请选择 A / B，或告诉我你想优先达成的结果。确认后我再生成最终 `/goal`。
```

## User Confirms

```text
选 A，先摸清项目，不要改文件。
```

## Final Goal

```text
/goal Discover / Map

项目背景：
- 当前是一个结构不清晰的软件 / 工具项目。
- README、脚本入口、测试命令和当前工作树状态需要重新确认。
- 用户希望先判断下一步，不希望直接修改文件。

目标：
- 通过只读方式摸清项目结构、入口、可运行命令、验证路径、当前风险，并提出 2-3 个下一步可执行 goal。

范围：
- 检查 README、AGENTS.md、package.json、src/、scripts/、配置文件、测试目录、最近日志或错误文件。
- 可以运行只读或低风险命令，如 `git status -sb`、`git log --oneline -5`、`npm run` 或等价命令列表。

非目标：
- 不修改代码、配置、依赖、格式或文档。
- 不新增功能、不重构、不发布。
- 不提交、不推送、不创建 issue 或 PR。

交付方式：
- 本地总结报告，不依赖 GitHub。

约束：
- 不删除、覆盖、批量移动任何文件。
- 不暴露密钥、token、账号或本地隐私路径。
- 不执行破坏性命令或全局配置修改。
- 如果发现需要高风险操作或外部写入，先停止并询问用户。

执行步骤：
1. 检查项目规则、README、目录结构和包管理/构建配置。
2. 梳理入口、脚本、测试和验证命令。
3. 判断当前阶段、主要风险和最可能的阻塞点。
4. 输出 2-3 个下一步 goal 候选，说明原因、范围、非目标和验证方式。

验证标准：
- 报告中列出实际检查过的文件和命令。
- 每个下一步候选 goal 都有明确范围、验证方式和风险判断。

完成标准：
- 用户能根据报告选择下一个可执行 `/goal`。

停止条件：
- 需要修改文件、安装依赖、执行破坏性命令、访问凭据、外部写入、提交或推送时，立即停止并询问用户。
```
