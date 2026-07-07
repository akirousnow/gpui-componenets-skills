# Getting Started

> 快速上手 GPUI Component：从依赖配置到第一个窗口应用。

源文档: https://longbridge.github.io/gpui-component/docs/getting-started

## 简介

GPUI Component 是基于 [GPUI](https://gpui.rs)（Zed 编辑器所用 UI 框架）的 Rust 跨平台桌面 GUI 组件库。本文介绍如何配置依赖、运行第一个窗口，以及理解无状态元素、有状态组件、主题、尺寸与变体等核心概念。掌握这些内容即可开始构建桌面应用。

## 关键要点

- **添加依赖**：在 `Cargo.toml` 中引入 `gpui`、`gpui_platform`、`gpui-component`，可选 `gpui-component-assets`（默认图标资源）。
- **必须初始化**：在 `app.run` 闭包首行调用 `gpui_component::init(cx);`，以启用主题与全局设置。
- **窗口顶层须为 Root**：第一个 `cx.new` 必须包裹 `Root::new(view, window, cx)`。
- **无状态元素**：实现 `RenderOnce` / `IntoElement`，如 `Button`、`Tag`，状态由调用方管理。
- **有状态组件**：实现 `Render`，如 `Dropdown`、`List`、`Table`，需以 `Entity` 持有内部状态。
- **主题访问**：通过 `cx.theme()` 获取 `primary`、`background`、`foreground` 等颜色。
- **尺寸**：`.xsmall()` / `.small()` / `.medium()`（默认）/ `.large()`。
- **变体**：`.primary()` / `.danger()` / `.warning()` / `.success()` / `.ghost()` / `.outline()`。

## 代码示例

依赖配置 `Cargo.toml`：

```toml
[dependencies]
gpui = { git = "https://github.com/zed-industries/zed" }
gpui_platform = { git = "https://github.com/zed-industries/zed", features = ["font-kit"] }
gpui-component = { git = "https://github.com/longbridge/gpui-component" }
# 可选：默认打包的图标资源
gpui-component-assets = { git = "https://github.com/longbridge/gpui-component" }
anyhow = "1.0"
```

最小可运行示例：

```rust
use gpui::*;
use gpui_component::{button::*, *};

pub struct HelloWorld;

impl Render for HelloWorld {
    fn render(&mut self, _: &mut Window, _: &mut Context<Self>) -> impl IntoElement {
        div()
            .v_flex()
            .gap_2()
            .size_full()
            .items_center()
            .justify_center()
            .child("Hello, World!")
            .child(
                Button::new("ok")
                    .primary()
                    .label("Let's Go!")
                    .on_click(|_, _, _| println!("Clicked!")),
            )
    }
}

fn main() {
    let app = gpui_platform::application().with_assets(gpui_component_assets::Assets);
    app.run(move |cx| {
        // 使用任何 GPUI Component 功能前必须先调用此方法
        gpui_component::init(cx);
        cx.spawn(async move |cx| {
            cx.open_window(WindowOptions::default(), |window, cx| {
                let view = cx.new(|_| HelloWorld);
                // 窗口顶层必须是一个 Root
                cx.new(|cx| Root::new(view, window, cx))
            })
        })
        .detach();
    });
}
```

有状态组件用法（以 `InputState` 为例）：

```rust
struct MyView {
    input: Entity<InputState>,
}

impl MyView {
    fn new(window: &Window, cx: &mut Context<Self>) -> Self {
        let input = cx.new(|cx| InputState::new(window, cx).default_value("Hello 世界"));
        Self { input }
    }
}
```

## 备注

- `gpui`、`gpui_platform`、`gpui-component` 均来自 Git 仓库，未发布到 crates.io，需保证网络可访问 GitHub。
- 运行组件示例库执行 `cargo run`；指定示例可执行 `cargo run --example <name>`。
- 图标默认不内置，详见 [Icons & Assets](./assets.md)。
