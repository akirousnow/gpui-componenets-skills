# Rating

> 评分组件，用星星等图标让用户对内容进行星级打分，支持半星、只读、自定义图标和数量。

源文档: https://longbridge.github.io/gpui-component/docs/components/rating

## 简介

Rating 是一个星级评分组件，常用于让用户对商品、文章等内容进行评级。组件支持半星精度、最大星数、只读展示、自定义图标与尺寸，以及受控状态管理和变化回调，非常适合评价、反馈等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(id)` | 构造器 | 创建评分组件，需传入唯一 id |
| `allow_half(bool)` | bool | 是否允许半星选择（默认 false） |
| `max_stars(value)` | usize | 设置最大星星数量（默认 5） |
| `value(f32)` | f32 | 受控设置当前评分值 |
| `icon_size(f32)` | f32 | 设置星星图标尺寸（默认 16） |
| `disabled(bool)` | bool | 是否禁用评分交互 |
| `on_change(listener)` | Fn(&f32) | 监听评分变化，回调接收新值 |

## 基本用法

```rust
use gpui_component::rating::Rating;

Rating::new("rating")
    .value(3.0)
    .max_stars(5)
```

允许半星：

```rust
Rating::new("half-rating")
    .allow_half(true)
    .value(3.5)
```

只读评分（用于展示）：

```rust
Rating::new("readonly-rating")
    .value(4.0)
    .disabled(true)
```

受控状态管理：

```rust
use gpui_component::rating::Rating;

struct MyView {
    rating: f32,
}

impl Render for MyView {
    fn render(&mut self, _: &mut Window, cx: &mut Context<Self>) -> impl IntoElement {
        Rating::new("rating")
            .value(self.rating)
            .max_stars(5)
            .allow_half(true)
            .on_change(cx.listener(|view, value: &f32, _, cx| {
                view.rating = *value;
                cx.notify();
            }))
    }
}
```

自定义最大星数和尺寸：

```rust
Rating::new("custom-rating")
    .max_stars(10)
    .icon_size(20.0)
    .value(7.0)
```

## 备注

- 启用 `allow_half` 后，鼠标悬停和点击会以半星为最小单位；否则只能选整星。
- 仅展示评价时，可设置 `disabled(true)` 使其变为只读，不响应交互但仍显示星值。
- `value` 超出 `0..=max_stars` 范围时会被钳制到边界；负值显示为 0 星。
- 默认图标为星形（`IconName::Star`），通过 `icon` 方法可自定义任意图标。
