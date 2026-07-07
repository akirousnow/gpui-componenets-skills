# Popover

> 浮层组件，相对于触发元素显示富内容，支持多种定位、自定义内容和受控开关。

源文档: https://longbridge.github.io/gpui-component/docs/components/popover

## 简介

Popover 是一个浮动浮层组件，在用户与触发元素交互（如点击）时显示。它支持四种角落定位、右键触发、自定义样式以及受控的开关状态，非常适合用于工具提示、菜单、表单等上下文信息展示。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(id)` | 构造器 | 创建 Popover，需传入唯一 id |
| `trigger(element)` | Selectable | 设置触发元素（如 Button） |
| `child(...)` / `children(...)` | Render / RenderOnce | 直接添加浮层内容 |
| `content(closure)` | FnMut | 通过闭包构建复杂内容，可访问 PopoverState/Window/Context |
| `anchor(Corner)` | Corner | 指定相对触发元素的定位角落（TopLeft/TopRight/BottomLeft/BottomRight） |
| `mouse_button(MouseButton)` | MouseButton | 指定触发鼠标按键（如右键） |
| `appearance(bool)` | bool | 关闭默认样式以完全自定义外观 |
| `open(bool)` | bool | 受控控制开关状态 |
| `on_open_change(listener)` | Fn | 监听开关状态变化 |
| `default_open(bool)` | bool | 设置初始渲染时是否默认打开 |

## 基本用法

```rust
use gpui::ParentElement as _;
use gpui_component::{button::Button, popover::Popover};

Popover::new("basic-popover")
    .trigger(Button::new("trigger").label("Click me").outline())
    .child("Hello, this is a popover!")
    .child("It appears when you click the button.")
```

自定义定位：

```rust
use gpui::Corner;

Popover::new("positioned-popover")
    .anchor(Corner::TopRight)
    .trigger(Button::new("top-right").label("Top Right").outline())
    .child("This popover appears at the top right")
```

使用 `content` 方法构建复杂内容：

```rust
use gpui::ParentElement as _;
use gpui_component::popover::Popover;

Popover::new("complex-popover")
    .anchor(Corner::BottomLeft)
    .trigger(Button::new("complex").label("Complex Content").outline())
    .content(|_, _, _| {
        div()
            .child("This popover has complex content.")
            .child(
                Button::new("action-btn")
                    .label("Perform Action")
                    .outline()
            )
    })
```

右键触发：

```rust
use gpui::MouseButton;

Popover::new("context-menu")
    .anchor(Corner::BottomRight)
    .mouse_button(MouseButton::Right)
    .trigger(Button::new("right-click").label("Right Click Me").outline())
    .child("Context Menu")
    .child(Divider::horizontal())
    .child("This is a custom context menu.")
```

手动关闭浮层（通过 `DismissEvent`）：

```rust
use gpui_component::{DismissEvent, popover::Popover};

Popover::new("dismiss-popover")
    .trigger(Button::new("dismiss").label("Dismiss Popover").outline())
    .content(|_, cx| {
        div()
            .child("Click the button below to dismiss this popover.")
            .child(
                Button::new("close-btn")
                    .label("Close Popover")
                    .on_click(cx.listener(|_, _, _, cx| {
                        cx.emit(DismissEvent);
                    }))
            )
    })
```

受控开关状态：

```rust
use gpui_component::popover::Popover;

struct MyView {
    popover_open: bool,
}

Popover::new("controlled-popover")
    .open(self.popover_open)
    .on_open_change(cx.listener(|this, open: &bool, _, cx| {
        this.popover_open = *open;
        cx.notify();
    }))
    .trigger(Button::new("control-btn").label("Control Popover").outline())
    .child("This popover's open state is controlled programmatically.")
```

## 备注

- 任何实现 `Selectable` 的元素可作为触发器（如 Button）；任何实现 `RenderOnce` 或 `Render` 的元素可作为浮层内容。
- `content` 闭包在每次渲染时都会调用，应避免在其中创建新实体或执行重操作，以免影响性能。
- 使用 `open` 受控控制时，必须在 `on_open_change` 中更新状态，否则浮层行为异常；此设置会覆盖 `default_open`。
- 通过 `appearance(false)` 关闭默认样式后，Popover 实现了 `Styled` trait，可使用 GPUI 的全部样式方法自定义外观。
