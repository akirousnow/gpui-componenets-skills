# Tooltip

> 悬浮提示组件，在鼠标悬停或聚焦时显示帮助信息，支持快捷键说明和自定义内容。

源文档: https://longbridge.github.io/gpui-component/docs/components/tooltip

## 简介

Tooltip 是一个多功能悬浮提示组件，在用户悬停或聚焦目标元素时显示帮助性文字。它支持展示键盘快捷键说明和自定义富文本内容，常用于解释按钮用途、补充操作提示等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Tooltip` | struct | 提示组件主体 |
| `Tooltip::new` | 构造函数 | 创建提示，传入文本内容 |
| `child` | 方法 | 设置自定义内容（富文本） |
| `keymap` | 方法 | 设置关联的快捷键（会显示对应的按键） |
| `disabled` | 方法 | 是否禁用提示 |

通常通过 `.tooltip()` 方法挂载到目标元素上。

## 基本用法

```rust
use gpui_component::tooltip::Tooltip;

// 普通文字提示
let button = Button::new("save")
    .icon(IconName::Save)
    .tooltip(|_, _| Tooltip::new("Save file"));

// 带快捷键的提示
let button = Button::new("undo")
    .icon(IconName::Undo)
    .tooltip(|_, _| Tooltip::new("Undo").keymap("cmd-z"));
```

## 备注

无特别说明。
