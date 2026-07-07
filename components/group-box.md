# GroupBox

> 带可选标题的样式化容器组件，用于将相关内容分组展示。

源文档: https://longbridge.github.io/gpui-component/docs/components/group-box

## 简介

GroupBox 是一个多功能容器组件，用于将相关内容分组到一起，并支持可选的边框、背景色和标题。它为表单控件、设置面板和其他相关 UI 元素提供视觉组织和语义分组能力。典型使用场景包括表单分节、设置面板分组、通知偏好设置等。

## 核心 API

### GroupBox 方法

| 名称 | 类型 | 说明 |
|------|------|------|
| `new()` | 构造函数 | 创建带默认设置的新 GroupBox |
| `variant(variant)` | 方法 | 设置 GroupBox 的变体类型 |
| `fill()` | 方法 | 使用 Fill 变体（带背景色） |
| `outline()` | 方法 | 使用 Outline 变体（带边框） |
| `id(id)` | 方法 | 设置元素的 ID |
| `title(title)` | 方法 | 设置 GroupBox 的标题/头部 |
| `title_style(style)` | 方法 | 自定义标题样式 |
| `content_style(style)` | 方法 | 自定义内容区域样式 |

### GroupBoxVariant 变体

| 变体 | 说明 |
|------|------|
| `Normal` | 默认变体，无背景无边框 |
| `Fill` | 带背景色和内边距 |
| `Outline` | 带边框和内边距，无背景色 |

## 基本用法

```rust
use gpui_component::group_box::{GroupBox, GroupBoxVariant};

GroupBox::new()
    .child("Subscriptions")
    .child(Checkbox::new("all").label("All"))
    .child(Checkbox::new("newsletter").label("Newsletter"))
    .child(Button::new("save").primary().label("Save"))
```

变体使用示例：

```rust
// Fill 变体 - 带背景色
GroupBox::new()
    .fill()
    .title("Settings")
    .child("Content with background")

// Outline 变体 - 带边框
GroupBox::new()
    .outline()
    .title("Preferences")
    .child("Content with border")
```

## 备注

- 建议在分组表单控件时始终包含描述性标题
- 使用 `fill()` 用于主要内容分组，`outline()` 用于次要分组
- 可与表单、对话框配合使用；如需可折叠分组内容，可考虑使用 Accordion
