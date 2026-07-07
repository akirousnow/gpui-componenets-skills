# VirtualList

> 高性能虚拟列表组件，用于渲染大规模数据集，支持可变项目尺寸。

源文档: https://longbridge.github.io/gpui-component/docs/components/virtual-list

## 简介

VirtualList 是一个高性能的虚拟化列表组件，用于高效渲染大规模数据集。它通过虚拟滚动技术只渲染可见区域的列表项，即使数据量达到数万条也能保持流畅，支持可变项目高度，适用于聊天记录、日志、长列表等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `UniformList` | struct | 等高项虚拟列表（固定行高） |
| `ListState` | struct | 列表状态管理 |
| `ListState::new` | 构造函数 | 创建列表状态 |
| `reset_count` | 方法 | 重置列表项数量 |
| `scroll_to` | 方法 | 滚动到指定项 |

## 基本用法

```rust
use gpui_component::list::{UniformList, ListState};

let state = cx.new(|cx| ListState::new(0, cx));

// 渲染等高虚拟列表
UniformList::new(cx.view().entity_id(), state.clone(), total_count, cx)
    .render_item(move |ix, _, _| {
        div().child(format!("Item {}", ix))
    })
```

## 备注

对于等高列表项使用 `UniformList` 性能更优；可变高度场景可参考 Table 中的虚拟滚动实现。
