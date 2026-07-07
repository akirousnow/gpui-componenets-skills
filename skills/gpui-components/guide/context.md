# Context

> 理解 GPUI 中 Window、App、Context 与 Entity 四个核心概念及 `cx` 命名约定。

源文档: https://longbridge.github.io/gpui-component/docs/context

## 简介

在 GPUI 中，`Window`、`App`、`Context` 与 `Entity` 是最重要的四个概念，它们几乎出现在所有函数签名中。理解它们的职责划分，是编写 GPUI 应用的基础。GPUI 采用统一的 `cx` 命名约定来表示 `App` 与 `Context<Self>`，遵循该约定可提升代码的可读性与一致性。

## 关键要点

| 概念 | 类型 | 职责 |
| --- | --- | --- |
| **App** | `&mut App` | 应用全局上下文，管理应用级状态、窗口、全局数据与主题 |
| **Window** | `&mut Window` | 单个窗口，负责渲染、焦点、鼠标与窗口尺寸等窗口级信息 |
| **Context\<T\>** | `&mut Context<T>` | 特定实体 `T` 的上下文，提供对该实体的状态更新、订阅与通知能力 |
| **Entity** | `Entity<T>` | 可观察的状态单元（Model），通过 `cx.new(|cx| ...)` 创建，可被多个视图共享 |

- **命名约定**：始终用 `cx` 表示 `App` 或 `Context<Self>`，用 `window` 表示 `Window`。
- **`window` 在前**：函数签名通常为 `(&mut self, window: &mut Window, cx: &mut Context<Self>)`。
- **Entity 创建**：通过 `cx.new(|cx| State::new(window, cx))` 创建并返回 `Entity<State>`。
- **全局访问 vs 实体访问**：`App` 用于无实体的全局操作，`Context<T>` 用于针对特定实体的操作。

## 代码示例

四种上下文在不同场景中的典型签名：

```rust
// 应用/窗口级初始化，cx 为 &mut App
fn new(window: &mut Window, cx: &mut App) {}

// 无状态元素，cx 为 &mut App
impl RenderOnce for MyElement {
    fn render(self, window: &mut Window, cx: &mut App) {}
}

// 有状态视图，cx 为 &mut Context<Self>
impl Render for MyView {
    fn render(&mut self, window: &mut Window, cx: &mut Context<Self>) {}
}
```

创建并持有 `Entity`（以 `InputState` 为例）：

```rust
struct MyView {
    input: Entity<InputState>,
}

impl MyView {
    fn new(window: &mut Window, cx: &mut Context<Self>) -> Self {
        // cx.new 返回 Entity<InputState>
        let input = cx.new(|cx| InputState::new(window, cx).default_value("Hello 世界"));
        Self { input }
    }

    fn render(&mut self, _: &mut Window, _: &mut Context<Self>) -> impl IntoElement {
        self.input.clone()
    }
}
```

## 备注

- `cx` 既可能是 `&mut App` 也可能是 `&mut Context<T>`，需结合上下文判断具体类型。
- 深入了解可参考 [GPUI 官方文档](https://docs.rs/gpui/latest/gpui/) 中 `Window`、`App`、`Context`、`Entity` 的 API 说明。
- 无特别说明。
