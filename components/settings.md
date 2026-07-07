# Settings

> 应用设置界面组件，提供分组的设置项与多页面管理，支持搜索过滤。

源文档: https://longbridge.github.io/gpui-component/docs/components/settings

## 简介

Settings 组件（自 v0.5.0 起）用于构建应用程序的设置界面，类似 macOS / iOS 的系统设置。它支持将设置项按页面（Page）和分组（Group）组织，并能通过标题与描述进行搜索过滤，仅展示相关设置项。典型场景包括应用的偏好设置、配置面板等。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Settings` | 结构体 | 主设置组件，承载多个 `SettingPage` |
| `SettingPage` | 结构体 | 一组相关设置分组的页面，可设置图标、默认展开、可重置 |
| `SettingGroup` | 结构体 | 基于分组样式的相关设置项集合 |
| `SettingItem` | 结构体 | 单个设置项，含标题、描述与字段 |
| `SettingField` | 枚举 | 字段类型：`switch` / `checkbox` / `input` / `dropdown` / `number_input` / `render` / `element` |
| `NumberFieldOptions` | 结构体 | 数字字段配置，含 `min`、`max` 等 |
| `SettingFieldElement` | Trait | 自定义复杂字段元素的接口 |

布局层级为：`Settings` > `SettingPage` > `SettingGroup` > `SettingItem`（标题 + 描述 + `SettingField`）。

## 基本用法

```rust
use gpui_component::setting::{Settings, SettingPage, SettingGroup, SettingItem, SettingField, NumberFieldOptions};

Settings::new("app-settings")
    .pages(vec![
        SettingPage::new("General")
            .default_open(true)
            .resettable(true)
            .groups(vec![
                SettingGroup::new().title("Appearance").items(vec![
                    SettingItem::new(
                        "Dark Mode",
                        SettingField::switch(
                            |cx: &App| cx.theme().mode.is_dark(),
                            |val: bool, cx: &mut App| { /* 处理主题切换 */ },
                        ),
                    )
                    .description("Switch between light and dark themes."),
                ]),
                SettingGroup::new().title("Font").items(vec![
                    SettingItem::new(
                        "Font Size",
                        SettingField::number_input(
                            NumberFieldOptions { min: 8.0, max: 72.0, ..Default::default() },
                            |cx: &App| 14.0,
                            |val: f64, cx: &mut App| { /* 处理字号变化 */ },
                        ),
                    ),
                ]),
            ]),
    ])
```

## 备注

- 自 v0.5.0 起提供。
- 实现 `Sizable` trait，支持 `xsmall()` / `small()` / `medium()` / `large()` / `with_size(Size)` 尺寸控制。
- 可通过 `SettingItem::render` 闭包或 `SettingFieldElement` trait 创建完全自定义的字段。
