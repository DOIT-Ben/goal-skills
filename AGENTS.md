# AGENTS.md

## Role

This repository is a lightweight skill package for generating project-aware Codex `/goal` prompts.

The only exposed skill is `goal-skills`.

## Purpose

Use this package to:

- inspect a project or task context
- turn an early product idea into a Codex `/goal`
- choose the most suitable goal type
- avoid premature execution when direction is unclear
- produce one bounded, verifiable `/goal`

## Trigger Phrases

Treat these as strong triggers:

- 生成一个goal
- 针对这个项目设置一个goal
- 给这个项目设置一个goal
- 为当前项目生成goal
- 把这个目标转成goal
- 先别改，先生成goal
- generate a goal
- set a goal for this project
- convert this into a goal
- Codex goal
- /goal prompt
- convert task to goal
- plan before change

## Scope

Keep the package lightweight.

- Do not add project-management document templates by default.
- Do not expose separate child skills unless the user explicitly asks for that structure.
- Put supporting material under `references/`.
- Keep `goal-skills` as the single stable entry point.
- Do not assume GitHub, commits, pushes, issues, PRs, or releases when generating goals.
- Generated goals must include operation boundaries and stop conditions for destructive, external, credential-related, or irreversible actions.
- Treat deployments, databases, cloud services, package registries, email, messaging tools, payment systems, public websites, and third-party APIs as external writes that need explicit authorization.
- When the input is a product idea, recommend `products-skills` as the follow-up product workflow instead of making `goal-skills` a full product-delivery system.

## Validation

After editing this package, verify discovery with:

```powershell
npx --yes skills add . --list --full-depth
```

Expected result: `Found 1 skill`, named `goal-skills`.
