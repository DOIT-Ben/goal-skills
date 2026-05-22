# Example: Product Idea

## Input

```text
把这个 idea 转成 goal：我想做一个帮助独立开发者把想法拆成可执行产品计划的工具。
```

## Expected Behavior

- Use `Idea / Product Slice`.
- Do not assume an existing codebase.
- Generate a goal for the smallest useful product slice, not a full product methodology.
- Keep product-methodology work out of scope unless the user asks for it.

## Good Output Shape

```text
/goal Idea / Product Slice

项目背景：
- 用户有一个产品 idea，但还没有明确的最小切片和验证方式。

目标：
- 明确目标用户、核心问题、最小可落地切片、验证方式和下一道 gate。

非目标：
- 不直接实现完整产品。
- 不做品牌、定价、发布和长期路线图。

验证标准：
- 产出一个可执行产品切片说明。
- 写明目标用户、核心场景、成功标准和停止条件。
```
