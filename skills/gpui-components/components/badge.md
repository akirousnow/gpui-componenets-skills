# Badge

> 用于显示未读消息数、状态或通知数的徽标组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/badge

## 简介

Badge 是一个通用的徽标组件，可以在其他元素（如头像、图标、列表项）右上角显示数字计数、红点或自定义内容，用于提示未读消息数、新状态或通知。支持数字溢出显示（如 `99+`）、单独使用、自定义颜色与位置等特性。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Badge::new(id)` | 构造函数 | 创建徽标实例 |
| `count(n)` | 方法 | 设置数字计数，超出 `max` 显示为 `max+` |
| `dot(bool)` | 方法 | 是否显示为红点（不显示数字） |
| `content(element)` | 方法 | 设置自定义内容（非数字） |
| `max(n)` | 方法 | 最大显示数字，默认 `99` |
| `color(color)` | 方法 | 自定义徽标颜色 |
| `placement(pos)` | 方法 | 设置徽标位置（如右上角） |
| `child(element)` | 方法 | 设置被包裹的目标元素 |
| `BadgePlacement` | enum | 徽标位置枚举 |

## 基本用法

基础数字徽标：

```rust
use gpui_component::badge::Badge;

Badge::new("notification-badge")
    .count(5)
    .child(Avatar::new("user").name("John"))
```

红点模式：

```rust
Badge::new("dot-badge")
    .dot(true)
    .child(Icon::new(IconName::Bell))
```

超出最大值显示：

```rust
Badge::new("overflow-badge")
    .count(120)
    .max(99)  // 显示为 "99+"
    .child(Icon::new(IconName::Mail))
```

自定义内容：

```rust
Badge::new("custom-badge")
    .content(Label::new("New"))
    .child(Icon::new(IconName::Settings))
```

## 备注

- `count` 为 0 时徽标自动隐藏。
- `dot(true)` 模式下忽略 `count`，只显示一个小红点。
- 可通过 `BadgePlacement` 调整徽标在目标元素上的位置。
