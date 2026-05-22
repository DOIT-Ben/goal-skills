# Example: Broken App

## Input

```text
convert task to goal: the app no longer starts locally. Fix only the startup path and verify the health endpoint.
```

## Expected Behavior

- Use `Repair / Restore`.
- Generate a final goal directly if the target is clear.
- Use the current conversation as the first input source: startup repair and health validation are the requested delivery, not broad refactor or deployment.
- Keep delivery local unless the user asks for commit, push, PR, release, or deployment.
- Include stop conditions before dependency installation, destructive git, external writes, or credential use.

## Good Output Shape

```text
Goal: Repair / Restore

Background:
- The app startup path is broken or unverified.
- The user wants only local startup repair and health endpoint validation.

Goal:
- Restore the local startup path and verify the health endpoint.

Non-Goals:
- Do not add features, refactor unrelated modules, deploy, commit, or push.

Validation:
- Run the local start command or documented equivalent.
- Check the health endpoint and report the exact command/result.
```

## Bad Output / Anti-Pattern

```text
Goal: Refactor / Reorganize

重构启动架构，升级依赖，顺手整理配置，然后部署到线上。
```

Why it is bad:

- Refactors before restoring the broken baseline.
- Expands scope beyond startup repair.
- Adds dependency and deployment risk without user approval.
- Does not define a local health-check validation path.
