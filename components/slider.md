# Slider

> 数值范围选择滑块组件，支持单值/区间选择、水平/垂直方向与自定义步进。

源文档: https://longbridge.github.io/gpui-component/docs/components/slider

## 简介

Slider 是一个用于在指定范围内选择数值的滑块组件。它同时支持单值和区间（range）选择模式、水平和垂直两种方向、自定义样式以及步进间隔。典型场景包括音量控制、价格区间筛选、图片裁剪参数调节等。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Slider` | 结构体 | 滑块组件本体 |
| `SliderMode` | 枚举 | 选择模式：`Single`（单值）/ `Range`（区间） |
| `SliderOrientation` | 枚举 | 方向：`Horizontal` / `Vertical` |
| `new(start, end, cx)` | 方法 | 创建滑块，需传入起始/结束值与上下文 |
| `start(value)` | 方法 | 设置最小值 |
| `end(value)` | 方法 | 设置最大值 |
| `step_interval(value)` | 方法 | 设置步进间隔 |
| `mode(mode)` | 方法 | 设置单值或区间模式 |
| `orientation(orient)` | 方法 | 设置水平或垂直方向 |
| `small_step_interval(value)` | 方法 | 设置小步长（用于精细调节） |
| `disabled(bool)` | 方法 | 是否禁用 |
| `on_change(fn)` | 方法 | 值变化回调 |
| `on_release(fn)` | 方法 | 释放滑块回调 |

## 基本用法

```rust
use gpui_component::slider::{Slider, SliderMode};

struct SliderExample {
    font_size_slider: Slider,
}

impl SliderExample {
    pub fn new(_window: &mut Window, cx: &mut Context<Self>) -> Self {
        let font_size_slider = Slider::new(8.0, 72.0, cx)
            .step_interval(1.0)
            .mode(SliderMode::Single)
            .on_change(cx.listener(|this, value: f64, _, cx| {
                this.font_size = value;
                cx.notify();
            }));

        Self { font_size_slider }
    }
}

impl Render for SliderExample {
    fn render(&mut self, _window: &mut Window, _cx: &mut Context<Self>) -> impl IntoElement {
        v_flex()
            .gap_4()
            .p_4()
            .child(self.font_size_slider.clone())
    }
}
```

区间选择（双滑块）：

```rust
let price_range_slider = Slider::new(0.0, 1000.0, cx)
    .mode(SliderMode::Range)
    .step_interval(10.0)
    .on_change(cx.listener(|this, value: f64, _, cx| {
        // value 为双滑块中当前变化的那个值
    }))
    .on_release(cx.listener(|this, _, window, cx| {
        // 释放时获取完整区间
        let (start, end) = this.price_range_slider.range_value();
    }));
```

垂直方向：

```rust
Slider::new(0.0, 100.0, cx)
    .orientation(SliderOrientation::Vertical)
    .h(px(200.))
```

## 备注

- 单值模式下用 `value()` 获取值；区间模式下用 `range_value()` 获取 `(start, end)`。
- 可通过 `set_value()` / `set_range_value()` 编程式设置当前值。
- 支持小步长精细调节：使用方向键时按 `small_step_interval` 移动。
