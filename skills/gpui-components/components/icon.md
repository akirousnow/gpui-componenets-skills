# Icon

> 从内置 Lucide 图标库渲染 SVG 图标，支持自定义尺寸、颜色和旋转。

源文档: https://longbridge.github.io/gpui-component/docs/components/icon

## 简介

Icon 组件用于渲染来自内置图标库（基于 Lucide.dev）的 SVG 图标，支持自定义大小、颜色和旋转变换。组件要求用户在 assets bundle 中提供 SVG 文件，并会自动将图标名称映射到对应的 SVG 文件路径。典型使用场景包括按钮图标、状态指示器、导航箭头、菜单项装饰等。

## 核心 API

### Icon 方法

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(icon)` | 构造函数 | 从 `IconName` 或另一个 `Icon` 创建新图标 |
| `path(path)` | 方法 | 设置自定义 SVG 文件路径 |
| `rotate(radians)` | 方法 | 按指定弧度旋转图标 |
| `transform(transformation)` | 方法 | 应用自定义变换 |
| `empty()` | 方法 | 创建空图标（用于自定义路径） |

### IconName 方法

| 名称 | 类型 | 说明 |
|------|------|------|
| `path()` | 方法 | 获取该图标的 SVG 文件路径 |
| `view(cx)` | 方法 | 直接从图标名创建视图实体 |

### 尺寸方法（Sizable trait）

| 名称 | 说明 |
|------|------|
| `xsmall()` | 超小尺寸 12px |
| `small()` | 小尺寸 14px |
| `medium()` | 中尺寸 16px（默认） |
| `large()` | 大尺寸 24px |
| `with_size(px(n))` | 自定义像素尺寸 |

### 常用 IconName

- 导航：`ArrowUp/Down/Left/Right`, `ChevronUp/Down/Left/Right`
- 操作：`Check`, `Close`, `Plus`, `Minus`, `Copy`, `Delete`, `Search`
- UI：`Menu`, `Settings`, `Eye`, `EyeOff`, `Bell`, `Info`
- 状态：`CircleCheck`, `CircleX`, `TriangleAlert`, `LoaderCircle`

## 基本用法

```rust
use gpui_component::{Icon, IconName};

// 使用 IconName enum 直接
IconName::Heart

// 或显式创建 Icon
Icon::new(IconName::Heart)

// 自定义尺寸与颜色
Icon::new(IconName::Search)
    .medium()
    .text_color(cx.theme().muted_foreground)

// 旋转图标
use gpui::Radians;
Icon::new(IconName::ArrowUp)
    .rotate(Radians::from_degrees(90.))

// 自定义 SVG 文件
Icon::empty()
    .path("icons/my-custom-icon.svg")
    .large()
    .text_color(cx.theme().primary)
```

## 备注

- SVG 文件必须由用户在 assets bundle 中提供，默认放在 `icons/` 目录下
- 名称映射：`IconName::CircleCheck` → `icons/circle-check.svg`
- 图标默认 flex-shrink-0，不会在 flex 布局中被压缩
- 默认尺寸匹配当前文本大小；Lucide 图标在 16px 下表现最佳
