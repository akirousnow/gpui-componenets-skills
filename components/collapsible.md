# Collapsible

> 可展开/收起的交互式容器组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/collapsible

## 简介

Collapsible 是一个可展开与收起的交互元素，常用于在有限空间内按需展示更多内容。通过 `open` 方法控制展开状态，`content` 添加的子元素在收起时会被隐藏。可配合任意 UI 元素，例如切换按钮来触发展开/收起。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `new()` | 构造方法 | 创建 Collapsible 实例 |
| `open(bool)` | 布尔值 | 控制展开/收起状态 |
| `child(element)` | 元素 | 添加始终可见的子元素 |
| `content(element)` | 元素 | 添加仅在展开时显示的内容 |
| `max_w_*()` | 样式 | 设置最大宽度等样式 |
| `gap_*()` | 样式 | 设置子元素间距 |

## 基本用法

```rust
use gpui_component::collapsible::Collapsible;
```

```rust
Collapsible::new()
    .max_w_128()
    .gap_1()
    .open(self.open)
    .child(
        "This is a collapsible component. \
        Click the header to expand or collapse the content.",
    )
    .content(
        "This is the full content of the Collapsible component. \
        It is only visible when the component is expanded. \n\
        You can put any content you like here, including text, images, \
        or other UI elements.",
    )
    .child(
        h_flex().justify_center().child(
            Button::new("toggle1")
                .icon(IconName::ChevronDown)
                .label("Show more")
                .when(open, |this| {
                    this.icon(IconName::ChevronUp).label("Show less")
                })
                .xsmall()
                .link()
                .on_click({
                    cx.listener(move |this, _, _, cx| {
                        this.open = !this.open;
                        cx.notify();
                    })
                }),
        ),
    )
```

## 备注

使用 `open` 方法控制收起状态；为 `false` 时，通过 `content` 方法添加的子元素会被隐藏。
