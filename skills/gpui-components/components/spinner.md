# Spinner

> 旋转动画加载指示器，用于表示任务进行中的加载状态。

源文档: https://longbridge.github.io/gpui-component/docs/components/spinner

## 简介

Spinner 组件显示一个旋转的加载动画，非常适合在内容加载、操作处理或任务执行期间向用户提供反馈。与 Skeleton（占位骨架）不同，Spinner 更适合场景不明确或无法预估内容布局时的加载状态，例如提交表单、请求网络数据等。可通过 `duration()` 自定义旋转速度。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Spinner` | 结构体 | 旋转加载指示器 |
| `new()` | 方法 | 创建默认 Spinner |
| `duration(ms)` | 方法 | 设置旋转一周的时长（毫秒） |
| `color(color)` | 方法 | 自定义颜色 |

Spinner 没有内置尺寸变体，可直接使用 gpui 尺寸方法（`size`、`w`、`h` 等）调整大小。

## 基本用法

```rust
use gpui_component::Spinner;

// 基本用法
Spinner::new()

// 自定义大小和速度
Spinner::new()
    .size(px(24.))
    .duration(std::time::Duration::from_millis(1000))
```

带文字的加载状态：

```rust
use gpui_component::Spinner;

h_flex()
    .gap_2()
    .items_center()
    .child(Spinner::new().size(px(16.)))
    .child(div().child("Loading..."))
```

按钮内加载状态：

```rust
use gpui_component::{Button, Spinner};

Button::new("submit")
    .primary()
    .label("Submitting")
    .disabled(true)
    .icon(Spinner::new().size(px(16.)))
```

自定义颜色：

```rust
Spinner::new()
    .size(px(32.))
    .color(rgb(0x007AFF))
```

## 备注

- 默认颜色取自主题的 `muted_foreground`，可通过 `color()` 自定义。
- 旋转动画使用 CSS transform，性能良好。
- 与 Skeleton 不同，Spinner 适用于无法预估内容布局的未知加载场景。
