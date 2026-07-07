# ElementId

> GPUI 中用于唯一标识元素的 ID，是绑定事件与管理元素状态的基础。

源文档: https://longbridge.github.io/gpui-component/docs/element_id

## 简介

ElementId 是 GPUI 元素的唯一标识符，用于在组件树中引用元素。在开始使用 GPUI 和 GPUI Component 之前，理解 ElementId 是必要的。

给元素添加 `id` 后，GPUI 才能为该元素绑定事件（如 `on_click`、`on_mouse_move`）。带有 `id` 的元素在 GPUI 中被称为 `Stateful<E>`。此外，GPUI 还通过 `id`（内部使用 GlobalElementId）配合 `window.use_keyled_state` 来管理部分元素的状态，因此保持 `id` 的唯一性非常重要。

## 关键要点

- **用途**：标识元素、绑定事件、管理元素状态。
- **Stateful<E>**：带 `id` 的元素即为 `Stateful<E>`，可注册交互事件。
- **唯一性范围**：`id` 只需在同一个 `Stateful<E>` 父元素的布局作用域内唯一即可，无需全局唯一。
- **GlobalElementId**：GPUI 内部会根据父元素 `id` 拼接生成全局 ID，因此不同父级下的子元素可使用相同简单 `id`。

## 代码示例

为一个 `div` 元素添加 `id`，从而支持事件绑定：

```rust
div().id("my-element").child("Hello, World!")
```

在列表场景下，由于父级 `list1` / `list2` 各自有 `id`，子项可以使用简单数字作为 `id`，GPUI 会自动拼接出全局唯一的 GlobalElementId：

```rust
div().id("app").child(
    div().id("list1").child(vec![
        div().id(1).child("Item 1"),
        div().id(2).child("Item 2"),
        div().id(3).child("Item 3"),
    ])
).child(
    div().id("list2").child(vec![
        div().id(1).child("Item 1"),
    ])
)
```

## 备注

- ElementId 既支持字符串（如 `"my-element"`），也支持整数（如 `1`）。
- 在同一父作用域内重复 `id` 可能导致状态管理冲突，需注意避免。
