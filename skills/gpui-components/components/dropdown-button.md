# DropdownButton

> 按钮与下拉菜单触发器的组合组件，左侧按钮和右侧触发器可独立响应事件。

源文档: https://longbridge.github.io/gpui-component/docs/components/dropdown_button

## 简介

DropdownButton 是 Button 与触发按钮的组合体。点击右侧触发器会展开下拉菜单，而左侧主按钮仍能独立响应点击事件，适用于“主操作 + 次要选项”的交互场景。

该组件复用了 Button 的全部能力，支持通过 `ButtonCustomVariant` 设置不同外观、通过 `Sizable` 调整尺寸，也可以添加图标与加载状态。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `DropdownButton::new(id)` | 构造器 | 创建一个 DropdownButton 实例 |
| `.button(btn)` | 方法 | 设置左侧主按钮（`Button`） |
| `.dropdown_menu(closure)` | 方法 | 设置下拉菜单内容，闭包接收 `menu`、`_`、`_` 参数 |
| `.dropdown_menu_with_anchor(anchor, closure)` | 方法 | 使用自定义锚点位置展开下拉菜单 |
| `.primary()` 等变体方法 | 方法 | 继承自 Button 的变体设置 |

## 基本用法

```rust
use gpui_component::button::{Button, DropdownButton};

DropdownButton::new("dropdown")
    .button(Button::new("btn").label("Click Me"))
    .dropdown_menu(|menu, _, _| {
        menu.menu("Option 1", Box::new(MyAction))
            .menu("Option 2", Box::new(MyAction))
            .separator()
            .menu("Option 3", Box::new(MyAction))
    })
```

### 设置为 Primary 变体

```rust
DropdownButton::new("dropdown")
    .primary()
    .button(Button::new("btn").label("Primary"))
    .dropdown_menu(|menu, _, _| {
        menu.menu("Option 1", Box::new(MyAction))
    })
```

### 自定义锚点位置

```rust
use gpui::Anchor;

DropdownButton::new("dropdown")
    .button(Button::new("btn").label("Click Me"))
    .dropdown_menu_with_anchor(Anchor::BottomRight, |menu, _, _| {
        menu.menu("Option 1", Box::new(MyAction))
    })
```

## 备注

- 左侧 Button 与右侧触发器的事件相互独立，适合“主操作 + 更多选项”的布局。
- 菜单项通过 `menu.menu(name, action)` 添加，可用 `separator()` 添加分隔线。
