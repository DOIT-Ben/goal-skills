# Example: Release / Publish

## Input

```text
把这个目标转成goal：准备发布这个公开 skill repo。请先确认远程仓库、版本号、README、安装命令和最终可见性。可以提交和推送。
```

## Expected Behavior

- Use `Release / Publish`.
- Include explicit version control and GitHub checks because the user authorized commit/push.
- Verify remote, branch, auth, visibility, install command, and final URL.
- Include rollback or stop conditions before irreversible/public actions.

## Good Output Shape

```text
Goal: Release / Publish

项目背景：
- 当前对话中，用户要求准备发布公开 skill repo，并明确授权提交和推送。
- 发布前需要确认远程仓库、版本号、README、安装命令和最终可见性。

目标：
- 完成一次可验证的公开发布准备和推送，确保 README、版本、安装命令、远程仓库和可见性一致。

范围：
- 检查 git 状态、远程、分支、版本号、README、plugin metadata、安装命令和 GitHub 可见性。
- 可以按用户授权提交和推送本次发布相关改动。

非目标：
- 不混入无关功能、不改私有配置、不发布到未确认的平台。

验证标准：
- 目标 AI 智能体的安装或导入校验通过；例如 Codex 可用 `npx --yes skills add <repo> -g -a codex -y --full-depth`，其他智能体可检查 `SKILL.md` 是否可读取。
- 推送后确认远程 URL、分支、版本和公开可见性。
```

## Bad Output / Anti-Pattern

```text
Goal: Release / Publish

不检查远程和可见性，直接 git push。
```

Why it is bad:

- Publication requires evidence, not just push success.
- Visibility and install path are part of delivery.
