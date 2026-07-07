# ColorPicker

> 综合性颜色选择器，支持多种颜色格式、预设色与 Alpha 透明通道。

源文档: https://longbridge.github.io/gpui-component/docs/components/color-picker

## 简介

ColorPicker 提供直观的颜色选择界面，包含调色板、Hex 输入、精选颜色（Featured Colors）等功能。支持 RGB、HSL、Hex 等多种格式，并完整支持 Alpha 透明通道。状态通过 `ColorPickerState` 管理，组件通过事件订阅 `ColorPickerEvent::Change` 监听颜色变化。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `ColorPickerState::new()` | 构造方法 | 创建颜色选择器状态实体 |
| `ColorPicker::new(&state)` | 构造方法 | 创建颜色选择器组件 |
| `default_value(color)` | 方法 | 设置默认颜色 |
| `featured_colors(Vec<Hsla>)` | 方法 | 设置精选/常用颜色 |
| `icon(IconName)` | 方法 | 用图标替代默认色块 |
| `label(&str)` | 方法 | 设置标签文本 |
| `anchor(Corner)` | 方法 | 设置下拉面板锚点位置 |
| `small()` / `large()` / `xsmall()` | 样式 | 设置不同尺寸 |
| `ColorPickerEvent::Change` | 事件 | 颜色变化事件，参数为 `Option<Hsla>` |

## 基本用法

```rust
use gpui_component::color_picker::{ColorPicker, ColorPickerState, ColorPickerEvent};
```

```rust
// Create color picker state
let color_picker = cx.new(|cx|
    ColorPickerState::new(window, cx)
        .default_value(cx.theme().primary)
);

// Create the color picker component
ColorPicker::new(&color_picker)
```

事件监听示例：

```rust
let _subscription = cx.subscribe(&color_picker, |this, _, ev, _| match ev {
    ColorPickerEvent::Change(color) => {
        if let Some(color) = color {
            println!("Selected color: {}", color.to_hex());
        }
    }
});
```

自定义精选颜色与图标：

```rust
use gpui_component::{Sizable as _, IconName};

let brand_colors = vec![
    Hsla::parse_hex("#FF6B6B").unwrap(),
    Hsla::parse_hex("#4ECDC4").unwrap(),
    Hsla::parse_hex("#45B7D1").unwrap(),
];

ColorPicker::new(&color_picker)
    .featured_colors(brand_colors)
    .icon(IconName::Palette)
    .label("Brand Color")
    .large()
```

## 备注

颜色内部使用 GPUI 的 `Hsla` 格式表示；可用 `Hsla::parse_hex` 与 `color.to_hex()` 在 Hex 字符串之间互转。Alpha 透明度通过 HSLA 的第四分量控制。
