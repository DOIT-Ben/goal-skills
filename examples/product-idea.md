# Example: Product Idea

## Input

```text
把这个 idea 转成 goal：我想做一个帮助独立开发者把想法拆成可执行产品计划的工具。
```

## Expected Behavior

- Use `Idea / Product Slice`.
- Do not assume an existing codebase.
- Treat the current conversation as the first input source: extract the idea, target user hint, desired outcome, and implicit deliverable.
- Generate a final goal for the minimum complete product-slice loop, not a full product methodology.
- Keep product-methodology work out of scope unless the user asks for it.

## Good Output Shape

```text
Goal: Idea / Product Slice

项目背景：
- 用户有一个产品 idea，但还没有明确的最小切片和验证方式。

目标：
- 明确目标用户、核心问题、最小可落地切片、验证方式和下一道 gate，形成从澄清到可检查产物的最小完整闭环。

非目标：
- 不直接实现完整产品。
- 不做品牌、定价、发布和长期路线图。

验证标准：
- 产出一个可执行产品切片说明。
- 写明目标用户、核心场景、成功标准和停止条件。
```

## Bad Output / Anti-Pattern

```text
Goal: Create From Zero

直接搭建完整 SaaS，包含登录、支付、团队协作、后台管理和上线发布。
```

Why it is bad:

- Treats an idea as a full product build.
- Skips target user, problem scenario, smallest slice, and evidence gate.
- Adds release and payment scope that the user did not request.
- Turns goal-skills into product-methodology work instead of goal generation.
