# Button

> 多变体、多尺寸、支持图标与加载状态的按钮组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/button

## 简介

Button 是最基础的交互组件，提供 primary、danger、ghost、link 等多种视觉变体，支持 outline 描边、compact 紧凑模式、多种尺寸，并可携带图标、加载态、选中态等状态。同时还提供 `ButtonGroup` 按钮组和 `DropdownButton` 下拉按钮，覆盖绝大多数按钮交互场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Button::new(id)` | 构造函数 | 创建按钮 |
| `label(text)` | 方法 | 设置按钮文字 |
| `primary()` / `danger()` / `warning()` 等 | 方法 | 设置按钮变体 |
| `outline()` | 方法 | 描边样式（可与变体组合） |
| `ghost()` / `link()` / `text()` | 方法 | 透明/链接/文字样式 |
| `compact()` | 方法 | 减小内边距的紧凑样式 |
| `icon(name)` / `icon(Icon)` | 方法 | 设置图标 |
| `disabled(bool)` | 方法 | 禁用状态 |
| `loading(bool)` | 方法 | 加载状态 |
| `selected(bool)` | 方法 | 选中状态 |
| `tooltip(text)` | 方法 | 悬浮提示 |
| `on_click(cb)` | 回调 | 点击回调 |
| `ButtonGroup::new(id)` | 组件 | 按钮组容器 |
| `DropdownButton::new(id)` | 组件 | 下拉按钮 |
| `ButtonCustomVariant::new(cx)` | 组件 | 自定义变体 |

## 基本用法

基础按钮与变体：

```rust
use gpui_component::button::{Button, ButtonGroup, DropdownButton};

Button::new("my-button")
    .label("Click me")
    .on_click(|_, _, _| {
        println!("Button clicked!");
    })

// 带变体
Button::new("btn-primary").primary().label("Primary")
Button::new("btn-danger").danger().label("Delete")
Button::new("btn-ghost").ghost().label("Ghost")
```

带图标与尺寸：

```rust
use gpui_component::{Icon, IconName};

Button::new("btn")
    .icon(IconName::Check)
    .label("Confirm")

Button::new("btn").small().label("Small")
Button::new("btn").large().label("Large")
```

按钮组与切换组：

```rust
ButtonGroup::new("btn-group")
    .child(Button::new("btn1").label("One"))
    .child(Button::new("btn2").label("Two"))
    .child(Button::new("btn3").label("Three"))

ButtonGroup::new("toggle-group")
    .multiple(true)
    .child(Button::new("btn1").label("Option 1").selected(true))
    .child(Button::new("btn2").label("Option 2"))
    .on_click(|selected_indices, _, _| {
        println!("Selected: {:?}", selected_indices);
    })
```

## 备注

- Button 实现 `Sizable` trait，支持 `xsmall()`、`small()`、`medium()`（默认）、`large()` 尺寸。
- `outline()` 不是独立变体，可与任意变体组合使用。
- 通过 `ButtonCustomVariant::new(cx)` 可自定义颜色、前景、边框、hover、active 等样式。
