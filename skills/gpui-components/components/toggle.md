# Toggle

> 按钮式切换组件，用于表达开/关或选中/未选中的二元状态。

源文档: https://longbridge.github.io/gpui-component/docs/components/toggle

## 简介

Toggle 是一个按钮样式的切换组件，用于表达开/关或选中/未选中状态。与 Switch 不同，Toggle 表现为可按压的按钮，按下即激活选中态，常用于工具栏中的格式开关、单选组等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Toggle` | struct | 切换组件主体 |
| `Toggle::new` | 构造函数 | 创建 Toggle，传入唯一 id |
| `checked` | 方法 | 设置选中状态 |
| `on_click` / `on_toggle` | 方法/回调 | 点击/状态切换时触发 |
| `icon` | 方法 | 设置按钮图标 |
| `disabled` | 方法 | 是否禁用 |

## 基本用法

```rust
use gpui_component::toggle::Toggle;

let toggle = Toggle::new("bold-toggle")
    .icon(IconName::Bold)
    .checked(is_bold)
    .on_click(|checked, _, _| {
        println!("Bold toggled: {}", checked);
    });
```

## 备注

无特别说明。
