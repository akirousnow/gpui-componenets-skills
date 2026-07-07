# Checkbox

> 复选框组件，允许用户在选中与未选中之间切换。

源文档: https://longbridge.github.io/gpui-component/docs/components/checkbox

## 简介

Checkbox 是用于二选一（是/否）的基础表单控件。它支持标签文本、禁用状态、部分选中（indeterminate）以及自定义图标，通过受控状态（`CheckboxState`）管理选中值。典型使用场景包括：表单中的同意条款勾选、筛选器中的多选条件、设置项开关等。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Checkbox::new(state)` | 构造函数 | 创建复选框，需传入共享状态 |
| `CheckboxState` | struct | 复选框状态管理 |
| `CheckboxState::new(window, cx)` | 构造函数 | 创建复选框状态 |
| `label(text)` | 方法 | 设置标签文本 |
| `disabled(bool)` | 方法 | 是否禁用 |
| `indeterminate(bool)` | 方法 | 设置部分选中状态 |
| `on_change(cb)` | 回调 | 选中状态变化回调，参数为布尔值 |
| `color(color)` | 方法 | 自定义选中颜色 |
| `icon(icon)` | 方法 | 自定义选中图标 |

## 基本用法

基础复选框：

```rust
use gpui_component::checkbox::{Checkbox, CheckboxState};

let state = cx.new(|cx| CheckboxState::new(window, cx));

Checkbox::new(state.clone())
    .label("Accept terms and conditions")
    .on_change(|checked, window, cx| {
        println!("Checked: {}", checked);
    })
```

禁用状态：

```rust
let state = cx.new(|cx| CheckboxState::new(window, cx));

Checkbox::new(state)
    .label("This option is disabled")
    .disabled(true)
```

部分选中（indeterminate）：

```rust
let state = cx.new(|cx| CheckboxState::new(window, cx));

Checkbox::new(state)
    .label("Select all")
    .indeterminate(true)
```

## 备注

- `CheckboxState` 是核心状态对象，需通过 `cx.new` 创建并以引用（`cx::Entity`）传入。
- `indeterminate` 状态仅影响视觉表现（显示为横线），实际值仍为布尔值。
- 可通过 `color()` 自定义选中时的背景色，默认使用主题主色。
