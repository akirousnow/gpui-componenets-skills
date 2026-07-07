# Skeleton

> 内容加载时的动画占位符组件，用于维持布局结构并提供加载反馈。

源文档: https://longbridge.github.io/gpui-component/docs/components/skeleton

## 简介

Skeleton 组件在真实内容加载期间显示动画占位内容，为用户提供视觉反馈表明内容正在加载，并帮助维持加载期间的布局结构。它内置脉冲动画（透明度在 100% 到 50% 之间循环），适用于卡片、列表、表格、表单等加载状态。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Skeleton` | 结构体 | 占位符组件本体 |
| `new()` | 方法 | 创建骨架占位符 |
| `secondary()` | 方法 | 使用次要变体（50% 透明度，更柔和） |

Skeleton 没有预定义尺寸变体，直接使用 gpui 的尺寸工具方法（`w`、`h`、`size`、`rounded` 等）来设定形状。

## 基本用法

文本行与头像占位符：

```rust
// Single line of text
Skeleton::new().w(px(250.)).h_4().rounded_md()

// Avatar placeholder
Skeleton::new().size_12().rounded_full()

// Multiple text lines
v_flex()
    .gap_2()
    .child(Skeleton::new().w(px(250.)).h_4().rounded_md())
    .child(Skeleton::new().w(px(200.)).h_4().rounded_md())
```

加载中的资料卡片：

```rust
v_flex()
    .gap_4().p_4().rounded_lg()
    .child(
        h_flex().gap_3().items_center()
            .child(Skeleton::new().size_12().rounded_full()) // Avatar
            .child(
                v_flex().gap_2()
                    .child(Skeleton::new().w(px(120.)).h_4().rounded_md()) // Name
                    .child(Skeleton::new().w(px(100.)).h_3().rounded_md()), // Email
            ),
    )
    .child(
        v_flex().gap_2()
            .child(Skeleton::new().w_full().h_4().rounded_md()) // Bio line 1
            .child(Skeleton::new().w(px(200.)).h_4().rounded_md()), // Bio line 2
    )
```

条件渲染：

```rust
if loading {
    Skeleton::new().w(px(200.)).h_4().rounded_md()
} else {
    div().child("Actual content here")
}
```

## 备注

- 内置脉冲动画持续 2 秒，使用 ease-in-out 缓动，不可禁用。
- 主题色为 `skeleton.background`（默认回退到 `secondary`），可在主题 JSON 中定制。
