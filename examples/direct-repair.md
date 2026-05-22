# Example: Direct Repair Goal

## Input

```text
把这个目标转成goal：修复这个项目的本地启动流程，并验证 health 接口。不要提交推送。
```

## Expected Behavior

- Return the final goal directly.
- Preserve the current conversation constraints before reading project files.
- Include local-only delivery.
- Explicitly exclude commit, push, issue, PR, release, deployment, and other external writes.
- Include a minimum complete loop: inspect startup evidence, repair the startup path, validate health, and summarize results.
- Include validation commands or a step to discover the correct validation path if unknown.

## Good Output Shape

```text
Goal: Repair / Restore

目标：
- 修复本地启动流程，并验证 health 接口可用。

交付方式：
- 本地验证结果和简短总结。

约束：
- 不提交、不推送、不创建 issue 或 PR。
- 不部署、不写入数据库、不调用外部写 API。

停止条件：
- 需要安装依赖、改全局配置、处理密钥、执行破坏性命令或外部写入时先确认。
```

## Bad Output / Anti-Pattern

```text
Goal: Repair / Restore

修复启动、提交所有改动、推送到 GitHub，并顺手部署验证。
```

Why it is bad:

- Violates the user's "不要提交推送" instruction.
- Adds external write and deployment work.
- Does not keep delivery local.
- Fails to stop before high-risk or unauthorized actions.
