# Label

> 支持次级文本、高亮、掩码和自定义样式的通用文本标签组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/label

## 简介

Label 是一个多用途文本标签组件，支持次级文本（可选/必填指示）、关键词高亮、敏感信息掩码和丰富的样式自定义。适用于表单标签、说明文字、金额展示、状态指示和多语言文本显示等场景。

## 核心 API

### Label 方法

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(text)` | 构造函数 | 创建带文本的标签 |
| `secondary(text)` | 方法 | 添加次级文本（如 `(optional)`、`*`） |
| `masked(bool)` | 方法 | 用圆点字符掩码显示 |
| `highlights(match)` | 方法 | 高亮匹配文本 |

### HighlightsMatch

| 变体 | 说明 |
|------|------|
| `Full(text)` | 高亮所有匹配项 |
| `Prefix(text)` | 仅高亮开头匹配 |

### 样式方法（Styled trait）

| 方法 | 说明 |
|------|------|
| `text_color(c)` | 文本颜色 |
| `text_size(s)` | 字号 |
| `text_center()` / `text_right()` | 对齐 |
| `font_semibold()` / `font_bold()` | 字重 |
| `line_height(h)` | 行高 |
| `text_xs/sm/base/lg/xl/2xl` | 预设尺寸 |

## 基本用法

```rust
use gpui_component::label::{Label, HighlightsMatch};

// 基础
Label::new("This is a label")

// 次级文本（必填/可选指示）
Label::new("Email Address").secondary("(required)")

// 高亮匹配
Label::new("Hello World Hello").highlights("Hello")

// 仅前缀高亮
Label::new("Hello World")
    .highlights(HighlightsMatch::Prefix("Hello".into()))

// 掩码敏感信息
Label::new("9,182.50 USD").text_2xl().masked(true)

// 多行换行
div().w(px(200.)).child(
    Label::new("Very long text that should wrap to next line.")
        .line_height(rems(1.8))
)
```

表单与状态示例：

```rust
// 必填字段
Label::new("Email Address")
    .secondary("*")
    .text_color(cx.theme().destructive)

// 状态指示
Label::new("✓ Verified").text_color(cx.theme().success)
```

## 备注

- 完整支持 Unicode，可用于中文、日文、Emoji
- 掩码显示为 `••••••`，适合金额等敏感数据，配合按钮切换
- 高亮会自动查找所有匹配项并按主题色着色
- 默认字号为 `text_base()`，可通过预设尺寸方法快速调整
