# Theme

> GPUI Component 内置的主题系统，通过 ActiveTheme trait 提供统一的颜色访问能力。

源文档: https://longbridge.github.io/gpui-component/docs/theme

## 简介

所有 GPUI Component 组件都通过内置的 Theme 系统支持主题切换。通过 `ActiveTheme` trait，可以在组件中访问当前主题的颜色。

若要使用当前主题中的颜色，需要确保你的组件或 view 拥有 App context（即能访问到 `cx`）。

## 关键要点

- **ActiveTheme trait**：访问主题颜色的统一入口，需在组件中 `use gpui_component::{ActiveTheme as _}`。
- **颜色访问**：通过 `cx.theme().primary`、`cx.theme().background`、`cx.theme().foreground` 等属性获取当前主题颜色。
- **内置主题**：在 [themes 目录](https://github.com/longbridge/gpui-component/tree/main/themes) 中提供 20+ 个内置主题。
- **ThemeRegistry**：用于加载和管理主题，支持监听主题目录变化（`watch_dir`）并热重载。

## 代码示例

在组件中访问当前主题颜色：

```rust
use gpui_component::{ActiveTheme as _};

// Access theme colors in your components
cx.theme().primary
cx.theme().background
cx.theme().foreground
```

使用 `ThemeRegistry` 从 `./themes` 目录加载并监听主题变化，加载完成后应用到全局主题：

```rust
use std::path::PathBuf;
use gpui::{App, SharedString};
use gpui_component::{Theme, ThemeRegistry};

pub fn init(cx: &mut App) {
    let theme_name = SharedString::from("Ayu Light");
    // Load and watch themes from ./themes directory
    if let Err(err) = ThemeRegistry::watch_dir(PathBuf::from("./themes"), cx, move |cx| {
        if let Some(theme) = ThemeRegistry::global(cx)
            .themes()
            .get(&theme_name)
            .cloned()
        {
            Theme::global_mut(cx).apply_config(&theme);
        }
    }) {
        tracing::error!("Failed to watch themes directory: {}", err);
    }
}
```

## 备注

- 主题文件为 `.toml` 格式，放在 `themes` 目录下即可被 `ThemeRegistry` 识别。
- 使用 `Theme::global_mut(cx).apply_config(&theme)` 可在运行时动态切换主题。
- 自定义颜色时，建议遵循内置主题的字段命名，以保持与组件的兼容性。
