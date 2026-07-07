# Icons & Assets

> 了解如何为 GPUI Component 提供图标与资源：使用默认打包资源或自建资源。

源文档: https://longbridge.github.io/gpui-component/docs/assets

## 简介

`IconName` 枚举与 `Icon` 元素为 GPUI 应用提供完整的图标体系。但为保持库的轻量，`gpui-component` 默认不内置任何图标 SVG 文件，而是将其拆分到独立的 `gpui-component-assets` crate 中。开发者可选择直接使用默认打包资源，也可通过 `rust-embed` 自建只含所需图标的资源源，以减小二进制体积。

## 关键要点

- **图标不内置**：`gpui-component` 未嵌入任何 SVG，需自行提供资源源。
- **默认资源 crate**：`gpui-component-assets` 包含 `assets/icons` 下全部图标文件。
- **注册资源**：创建应用时调用 `gpui_platform::application().with_assets(Assets)` 注册资源源。
- **自建资源**：用 `rust-embed` 将 SVG 嵌入二进制，并实现 `AssetSource` trait。
- **命名约定**：SVG 文件名需匹配 `IconName` 枚举，可从源码 [assets](https://github.com/longbridge/gpui-component/tree/main/crates/assets/assets/) 目录下载。
- **图标来源**：默认使用 [Lucide](https://lucide.dev) 与 [Isocons](https://isocons.app)。

## 代码示例

使用默认打包资源，先添加依赖：

```toml
[dependencies]
gpui-component = { git = "https://github.com/longbridge/gpui-component" }
gpui-component-assets = { git = "https://github.com/longbridge/gpui-component" }
```

注册默认资源源：

```rust
use gpui::*;
use gpui_component_assets::Assets;

let app = gpui_platform::application().with_assets(Assets);
```

自建资源源（基于 `rust-embed` 实现 `AssetSource`）：

```rust
use anyhow::anyhow;
use gpui::*;
use gpui_component::{v_flex, IconName, Root};
use rust_embed::RustEmbed;
use std::borrow::Cow;

/// 从 `./assets` 目录加载资源的资源源
#[derive(RustEmbed)]
#[folder = "./assets"]
#[include = "icons/**/*.svg"]
pub struct Assets;

impl AssetSource for Assets {
    fn load(&self, path: &str) -> Result<Option<Cow<'static, [u8]>>> {
        if path.is_empty() {
            return Ok(None);
        }

        Self::get(path)
            .map(|f| Some(f.data))
            .ok_or_else(|| anyhow!("could not find asset at path \"{path}\""))
    }

    fn list(&self, path: &str) -> Result<Vec<SharedString>> {
        Ok(Self::iter()
            .filter_map(|p| p.starts_with(path).then(|| p.into()))
            .collect())
    }
}
```

在视图中使用图标：

```rust
pub struct Example;

impl Render for Example {
    fn render(&mut self, _: &mut Window, _: &mut Context<Self>) -> impl IntoElement {
        v_flex()
            .gap_2()
            .size_full()
            .items_center()
            .justify_center()
            .text_center()
            .child(IconName::Inbox)
            .child(IconName::Bot)
    }
}
```

## 备注

- 自建资源时 `Cargo.toml` 需添加 `rust-embed` 依赖。
- 使用任何组件前仍需调用 `gpui_component::init(cx);` 初始化。
- 仅引入所需图标可有效降低最终二进制体积。
