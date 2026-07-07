# Scrollable

> 可滚动容器组件，提供自定义滚动条、滚动追踪与虚拟化能力。

源文档: https://longbridge.github.io/gpui-component/docs/components/scrollable

## 简介

Scrollable 是一个功能完备的滚动容器组件，支持垂直和水平双向滚动、可定制的滚动条外观，以及针对大数据集的虚拟化渲染。典型场景包括文件浏览器、聊天消息列表、数据表格等需要高效展示大量内容的界面。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `ScrollableElement` | Trait | 提供 `overflow_scrollbar()` 等方法，为元素附加滚动条 |
| `overflow_scrollbar()` | 方法 | 同时添加双向滚动条 |
| `overflow_x_scrollbar()` | 方法 | 仅添加水平滚动条 |
| `overflow_y_scrollbar()` | 方法 | 仅添加垂直滚动条 |
| `ScrollbarAxis` | 枚举 | 控制滚动条轴向 |
| `ScrollbarShow` | 枚举 | 滚动条显示模式：`Scrolling` / `Hover` / `Always` |
| `VirtualList` | 结构体 | 虚拟化列表，高效渲染大量条目 |
| `VirtualListScrollHandle` | 结构体 | 虚拟列表滚动句柄，支持 `scroll_to_item` |
| `ScrollHandle` | 结构体 | 普通滚动句柄，支持 `set_offset` / `max_offset` |

## 基本用法

```rust
use gpui::{div, Axis};
use gpui_component::ScrollableElement;

div()
    .id("scrollable-container")
    .size_full()
    .child("Your content here")
    .overflow_scrollbar()
```

手动创建滚动条并配合 `ScrollHandle`：

```rust
use gpui_component::scroll::{ScrollableElement};

pub struct ScrollableView {
    scroll_handle: ScrollHandle,
}

impl Render for ScrollableView {
    fn render(&mut self, _: &mut Window, cx: &mut Context<Self>) -> impl IntoElement {
        div()
            .relative()
            .size_full()
            .child(
                div()
                    .id("content")
                    .track_scroll(&self.scroll_handle)
                    .overflow_scroll()
                    .size_full()
                    .child("Your scrollable content")
            )
            .vertical_scrollbar(&self.scroll_handle)
    }
}
```

使用 `VirtualList` 渲染大型数据集，并滚动到指定条目：

```rust
use gpui_component::{VirtualList, VirtualListScrollHandle};

VirtualList::new(
    self.scroll_handle.clone(),
    item_count,
    |ix, window, cx| size(px(300.), px(40.)),
    |ix, bounds, selected, window, cx| {
        div()
            .size(bounds.size)
            .child(format!("Item {}", ix))
            .into_any_element()
    },
)

// 滚动到指定项
self.scroll_handle.scroll_to_item(index, ScrollStrategy::Top);
```

## 备注

- 滚动条外观可通过主题 JSON 中的 `scrollbar.background`、`scrollbar.thumb.background` 等字段定制。
- 使用 `Theme::sync_scrollbar_appearance(cx)` 可与系统滚动条偏好同步。
- `overflow_scrollbar()` 需要元素拥有 `id`。
