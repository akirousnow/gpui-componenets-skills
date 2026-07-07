# Resizable

> 可调整尺寸的分割面板组件，通过拖动分隔条动态改变各面板大小，支持水平/垂直方向与嵌套。

源文档: https://longbridge.github.io/gpui-component/docs/components/resizable

## 简介

Resizable 是一个可调整尺寸的面板组件，通过拖动面板之间的分隔条（resize handle）来动态调整各面板的大小。组件支持水平与垂直两种方向、嵌套布局、面板最小尺寸约束，以及分隔条拖动的样式与事件回调，非常适合构建可自定义布局的编辑器、侧边栏、面板等界面。

## 核心 API

### Resizable

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(id)` | 构造器 | 创建可调整尺寸容器，需传入唯一 id |
| `horizontal()` / `vertical()` | - | 设置排列方向：水平或垂直 |
| `direction(Axis)` | Axis | 设置布局轴向（Horizontal / Vertical） |
| `child(ResizablePanel)` | ResizablePanel | 添加面板（每个面板之间会自动插入分隔条） |
| `children(panels)` | IntoIterator | 批量添加面板 |
| `min_size(f32)` | f32 | 设置于拖动时容器整体最小尺寸 |

### ResizablePanel

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(content)` | impl IntoElement | 创建面板，传入内容元素 |
| `min_size(f32)` | f32 | 设置该面板最小尺寸（拖动时不会小于此值） |
| `max_size(f32)` | f32 | 设置该面板最大尺寸 |
| `initial_size(f32)` | f32 | 设置初始尺寸 |
| `id(id)` | impl IntoElementId | 为面板设置 id（用于状态持久化） |

## 基本用法

```rust
use gpui_component::resizable::{Resizable, ResizablePanel};

Resizable::new("basic-resizable")
    .horizontal()
    .child(
        ResizablePanel::new(
            div().bg(rgb(0x2d2d2d)).child("Panel 1").size_full()
        )
        .initial_size(300.)
    )
    .child(
        ResizablePanel::new(
            div().bg(rgb(0x3d3d3d)).child("Panel 2").size_full()
        )
        .initial_size(500.)
    )
```

垂直布局：

```rust
Resizable::new("vertical-resizable")
    .vertical()
    .child(
        ResizablePanel::new(
            div().bg(rgb(0x2d2d2d)).child("Top Panel").size_full()
        )
        .initial_size(200.)
    )
    .child(
        ResizablePanel::new(
            div().bg(rgb(0x3d3d3d)).child("Bottom Panel").size_full()
        )
        .initial_size(400.)
    )
```

约束面板尺寸：

```rust
Resizable::new("constrained")
    .horizontal()
    .child(
        ResizablePanel::new(
            div().child("Sidebar").size_full()
        )
        .initial_size(200.)
        .min_size(150.)
        .max_size(400.)
    )
    .child(
        ResizablePanel::new(
            div().child("Main content area").size_full()
        )
    )
```

嵌套可调整面板：

```rust
Resizable::new("nested")
    .horizontal()
    .child(
        ResizablePanel::new(
            div().child("Left Panel").size_full()
        )
        .initial_size(250.)
    )
    .child(
        ResizablePanel::new(
            Resizable::new("nested-inner")
                .vertical()
                .child(
                    ResizablePanel::new(
                        div().child("Top-Right Panel").size_full()
                    )
                    .initial_size(200.)
                )
                .child(
                    ResizablePanel::new(
                        div().child("Bottom-Right Panel").size_full()
                    )
                )
        )
    )
```

## 备注

- Resizable 会自动在相邻面板之间插入可拖动的分隔条，无需手动添加。
- 方向决定拖动轴：`horizontal()` 表示面板左右排列，分隔条沿垂直方向拖动来调整宽度；`vertical()` 则面板上下排列，分隔条水平拖动调整高度。
- 通过 `min_size` / `max_size` 约束面板尺寸可避免面板被拖得过大或过小。
- Resizable 内部支持状态持久化，可为面板指定唯一 `id` 以在重渲染时保持尺寸。
- 组件支持嵌套：一个 Resizable 的面板内容本身可以是另一个 Resizable，从而构建复杂的可调整布局。
