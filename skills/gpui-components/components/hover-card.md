# HoverCard

> 鼠标悬停在触发元素上时显示富内容的浮动浮层组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/hover-card

## 简介

HoverCard 组件用于在鼠标悬停在触发元素上时展示富内容，非常适合预览用户资料、链接预览和其他上下文信息，无需点击。该组件支持可配置的打开和关闭延迟时间，可避免鼠标快速移动时的闪烁问题。它类似于 Popover 组件，但由悬停而非点击触发，并提供了更精细的时序控制以获得更流畅的用户体验。

## 核心 API

### HoverCard 方法

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(id)` | 构造函数 | 创建带唯一 ID 的新 HoverCard |
| `trigger(trigger)` | 方法 | 设置触发悬停的元素 |
| `content(content)` | 方法 | 设置内容构建函数，仅在打开时调用 |
| `open_delay(duration)` | 方法 | 设置显示前的延迟（默认 600ms） |
| `close_delay(duration)` | 方法 | 设置隐藏前的延迟（默认 300ms） |
| `anchor(anchor)` | 方法 | 设置定位方式（默认 TopCenter） |
| `on_open_change(callback)` | 方法 | 打开状态变化时的回调 |
| `appearance(appearance)` | 方法 | 启用/禁用默认样式（默认 true） |

### HoverCardState 方法

| 名称 | 类型 | 说明 |
|------|------|------|
| `is_open()` | 方法 | 检查 HoverCard 当前是否打开 |

## 基本用法

```rust
use gpui::{ParentElement as _, Styled as _};
use gpui_component::{hover_card::HoverCard, v_flex};

HoverCard::new("basic")
    .trigger(
        div()
            .child("Hover over me")
            .text_color(cx.theme().primary)
            .cursor_pointer()
            .text_sm()
    )
    .child(
        v_flex()
            .gap_2()
            .child(
                div()
                    .child("This is a hover card")
                    .font_semibold()
                    .text_sm()
            )
            .child(
                div()
                    .child("You can display rich content when hovering over a trigger element.")
                    .text_color(cx.theme().muted_foreground)
                    .text_sm()
            )
    )
```

自定义打开/关闭时延：

```rust
HoverCard::new("fast-open")
    .open_delay(Duration::from_millis(200))
    .close_delay(Duration::from_millis(100))
    .trigger(Button::new("fast").label("Fast Open (200ms)").outline())
    .child(div().child("This hover card opens after 200ms").text_sm())
```

## 备注

- HoverCard 仅支持鼠标交互，不支持键盘导航；如需键盘可访问内容，请使用 Popover
- 避免嵌套 HoverCard，会造成混乱的用户体验
- 定位支持 6 种选项：TopLeft、TopCenter、TopRight、BottomLeft、BottomCenter、BottomRight
- 建议时延配置：标准内容 600ms/300ms，快速预览 500ms/200ms，工具提示 300ms/100ms
