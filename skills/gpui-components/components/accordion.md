# Accordion

> 允许用户展开/折叠内容分区的可折叠面板组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/accordion

## 简介

Accordion（手风琴）组件用于在有限空间内组织大量内容，用户可通过点击标题来展开或折叠对应分区。它内部基于 collapse 功能实现，支持单展开、多展开、嵌套等模式，适合用于设置面板、FAQ 列表、分组表单等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Accordion::new(id)` | 构造函数 | 创建一个新的手风琴容器 |
| `item(builder)` | 方法 | 添加一个折叠项，参数为 `AccordionItem` 的构建闭包 |
| `multiple(bool)` | 方法 | 是否允许多个项同时展开，默认 `false` |
| `bordered(bool)` | 方法 | 是否显示边框 |
| `disabled(bool)` | 方法 | 是否禁用整个手风琴 |
| `on_toggle_click(cb)` | 回调 | 切换时回调，返回当前展开项的索引列表 |
| `small()` / `large()` 等 | Sizable trait | 调整尺寸，实现 `Sizable` trait |
| `AccordionItem` | struct | 单个折叠项，提供 `title()` 和 `child()`/`content()` 方法 |

## 基本用法

```rust
use gpui_component::accordion::Accordion;

Accordion::new("my-accordion")
    .item(|item| {
        item.title("Section 1")
            .child("Content for section 1")
    })
    .item(|item| {
        item.title("Section 2")
            .child("Content for section 2")
    })
    .item(|item| {
        item.title("Section 3")
            .child("Content for section 3")
    })
```

允许多个项同时展开：

```rust
Accordion::new("my-accordion")
    .multiple(true)
    .item(|item| item.title("Section 1").child("Content 1"))
    .item(|item| item.title("Section 2").child("Content 2"))
```

监听切换事件：

```rust
Accordion::new("my-accordion")
    .on_toggle_click(|open_indices, window, cx| {
        println!("Open items: {:?}", open_indices);
    })
    .item(|item| item.title("Section 1").child("Content 1"))
```

## 备注

- 实现 `Sizable` trait，支持 `xsmall()`、`small()`、`medium()`（默认）、`large()` 等尺寸。
- 支持嵌套：在 `content()` 中放入另一个 `Accordion` 即可实现嵌套结构。
