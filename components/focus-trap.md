# FocusTrap

> 焦点陷阱工具，将键盘焦点限制在指定容器内，防止 Tab 导航越界。

源文档: https://longbridge.github.io/gpui-component/docs/components/focus-trap

## 简介

FocusTrap 用于约束键盘焦点，使 Tab/Shift+Tab 仅在容器内部循环。这是模态对话框、Sheet 等遮罩类组件实现无障碍访问的关键能力。

注意：Dialog 和 Sheet 组件已内置焦点陷阱，仅在自定义模态组件时才需手动使用 `focus_trap()`。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `FocusTrapElement` | 类型 | 焦点陷阱元素类型 |
| `FocusTrapContainer` | 类型 | 包裹容器，注册为焦点陷阱区域 |
| `.focus_trap(id, &handle)` | 方法 | 在容器上启用焦点陷阱，传入唯一 ID 和 `FocusHandle` |
| `cx.focus_handle()` | 方法 | 创建一个 `FocusHandle` 实例 |

## 基本用法

```rust
use gpui_component::FocusTrapElement;

let container_handle = cx.focus_handle();

v_flex()
    .child(Button::new("btn1").label("Button 1"))
    .child(Button::new("btn2").label("Button 2"))
    .child(Button::new("btn3").label("Button 3"))
    .focus_trap("trap1", &container_handle)
// Tab 循环: btn1 -> btn2 -> btn3 -> btn1
```

### 嵌套焦点陷阱

```rust
let outer_handle = cx.focus_handle();
let inner_handle = cx.focus_handle();

div()
    .child(
        v_flex()
            .gap_4()
            .child(Button::new("outer-1").label("Outer Button 1"))
            .child(
                h_flex()
                    .gap_2()
                    .child(Button::new("inner-1").label("Inner Button 1"))
                    .child(Button::new("inner-2").label("Inner Button 2"))
                    .focus_trap("inner", &inner_handle)
            )
            .child(Button::new("outer-2").label("Outer Button 2"))
            .focus_trap("outer", &outer_handle)
    )
```

### 条件启用

```rust
let content = v_flex()
    .gap_2()
    .child(Button::new("btn1").label("Button 1"))
    .child(Button::new("btn2").label("Button 2"));

if self.is_modal {
    content.focus_trap("conditional", &self.container_handle).into_any_element()
} else {
    content.into_any_element()
}
```

## 备注

- 必须提供关闭/退出焦点陷阱的方式（如 ESC 键、关闭按钮）。
- 嵌套陷阱时，最内层陷阱优先级最高。
- 仅在真正的模态交互中使用，避免滥用影响导航体验。
