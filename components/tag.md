# Tag

> 标签组件，用于对内容进行分类或标记的紧凑视觉元素。

源文档: https://longbridge.github.io/gpui-component/docs/components/tag

## 简介

Tag 是一个多功能标签组件，用于分类和标记内容。标签是紧凑的视觉元素，可配合不同颜色表达状态、类型或属性，常见于列表项标记、表单选择、文章分类等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Tag` | struct | 标签组件主体 |
| `Tag::new` | 构造函数 | 创建标签，传入文本内容 |
| `color` | 方法 | 设置标签颜色（支持主题色） |
| `variant` | 方法 | 设置视觉变体（如填充、描边等） |
| `closable` | 方法 | 是否可关闭（带关闭按钮） |
| `on_close` | 方法 | 关闭时触发的回调 |

## 基本用法

```rust
use gpui_component::tag::Tag;

let tag = Tag::new("Active")
    .color(cx.theme().green);

// 可关闭标签
let closable_tag = Tag::new("Filter")
    .closable()
    .on_close(|_, _, _| {
        println!("Tag closed");
    });
```

## 备注

无特别说明。
