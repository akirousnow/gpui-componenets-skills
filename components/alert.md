# Alert

> 用于吸引用户注意力的消息提示框，支持多种语义变体。

源文档: https://longbridge.github.io/gpui-component/docs/components/alert

## 简介

Alert 是一个通用的消息提示组件，用于向用户展示重要的提示信息。它支持 info（信息）、success（成功）、warning（警告）、danger（危险）等多种语义变体，并可携带标题、描述、图标等内容。典型使用场景包括表单校验提示、操作结果反馈、重要公告展示等。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Alert::new()` | 构造函数 | 创建 Alert 实例 |
| `variant(v)` | 方法 | 设置变体：`Info`、`Success`、`Warning`、`Danger` |
| `title(text)` | 方法 | 设置标题 |
| `child(element)` | 方法 | 设置自定义内容（如描述文本） |
| `icon(icon)` / `no_icon()` | 方法 | 自定义或隐藏图标 |
| `closable(bool)` / `on_close(cb)` | 方法 | 是否可关闭及关闭回调 |
| `AlertVariant` | enum | 变体枚举 |

## 基本用法

基础用法：

```rust
use gpui_component::alert::{Alert, AlertVariant};

Alert::new()
    .variant(AlertVariant::Info)
    .title("This is an info alert")
    .child("Here is some additional information.")
```

不同变体：

```rust
Alert::new().variant(AlertVariant::Success).title("Operation successful")
Alert::new().variant(AlertVariant::Warning).title("This action may be risky")
Alert::new().variant(AlertVariant::Danger).title("Something went wrong")
```

带关闭按钮：

```rust
Alert::new()
    .variant(AlertVariant::Warning)
    .title("Warning")
    .closable(true)
    .on_close(|_, _, _| {
        println!("Alert closed");
    })
```

## 备注

- 每种变体有对应的默认图标和颜色样式。
- 可通过 `no_icon()` 隐藏默认图标，或用 `icon()` 替换为自定义图标。
