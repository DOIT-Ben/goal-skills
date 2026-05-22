# Example: Broken App

## Input

```text
convert task to goal: the app no longer starts locally. Fix only the startup path and verify the health endpoint.
```

## Expected Behavior

- Use `Repair / Restore`.
- Generate a final `/goal` directly if the target is clear.
- Keep delivery local unless the user asks for commit, push, PR, release, or deployment.
- Include stop conditions before dependency installation, destructive git, external writes, or credential use.

## Good Output Shape

```text
/goal Repair / Restore

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
