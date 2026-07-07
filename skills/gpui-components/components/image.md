# Image

> 基于 GPUI 原生图像支持的图片组件，支持多种来源、ObjectFit 选项与加载/错误状态处理。

源文档: https://longbridge.github.io/gpui-component/docs/components/image

## 简介

Image 组件基于 GPUI 原生图像能力，支持 URL、本地文件和 SVG 等多种图片来源，并提供完善的尺寸控制、ObjectFit 缩放策略以及加载状态与错误处理能力。适用于头像、产品图、Banner、图库等场景。

## 核心 API

### 核心函数

| 名称 | 说明 |
|------|------|
| `img(source)` | 从 `ImageSource` 创建图像元素 |

### ImageSource 来源类型

| 类型 | 示例 |
|------|------|
| `String` / `&str` | `"https://example.com/image.jpg"` |
| 本地路径 | `"assets/logo.png"` |
| `SharedUri` | `SharedUri::from("file://path")` |
| Data URI | `"data:image/png;base64,..."` |

### ObjectFit 选项

| 值 | 说明 |
|------|------|
| `ObjectFit::Cover` | 填满容器，可能裁剪 |
| `ObjectFit::Contain` | 完整显示在容器内，保留比例 |
| `ObjectFit::Fill` | 拉伸填满容器，可能变形 |
| `ObjectFit::ScaleDown` | 类似 Contain 但不放大 |
| `ObjectFit::None` | 原始尺寸，可能溢出 |

### 常用样式方法

| 方法 | 说明 |
|------|------|
| `w(h)` / `h(h)` / `size(s)` | 设置宽/高/两者 |
| `w_full()` / `h_full()` / `size_full()` | 撑满容器 |
| `object_fit(fit)` | 设置缩放方式 |
| `rounded(r)` / `opacity(v)` | 圆角与透明度 |

## 基本用法

```rust
use gpui::{img, ImageSource, ObjectFit};

// 简单图片
img("https://example.com/image.jpg")

// 固定尺寸
img("https://example.com/photo.jpg")
    .w(px(300.))
    .h(px(200.))
    .object_fit(ObjectFit::Cover)

// 正方形头像
img("https://example.com/avatar.jpg")
    .size(px(100.))
    .object_fit(ObjectFit::Cover)
```

带错误处理与图标占位：

```rust
// 头像示例
fn custom_avatar(src: &str, size: f32) -> impl IntoElement {
    div()
        .size(px(size))
        .rounded_full()
        .overflow_hidden()
        .border_2()
        .child(
            img(src)
                .size_full()
                .object_fit(ObjectFit::Cover)
        )
}
```

## 备注

- 完全基于 GPUI 原生图像渲染，自动处理缓存与内存
- SVG 图形可用 `text_color()` 着色以适配主题
- 跨平台行为一致（Windows、macOS、Linux）
- 建议配合骨架屏（Skeleton）和错误状态 Icon 以保持布局稳定
