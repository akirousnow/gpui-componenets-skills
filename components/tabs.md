# Tabs

> 标签页组件，将内容组织为多个面板，同一时间只显示其中一个。

源文档: https://longbridge.github.io/gpui-component/docs/components/tabs

## 简介

Tabs 是一个标签页界面组件，用于将内容划分为多个分区（tab panels），每次只显示一个面板。适合在有限空间内组织多组相关内容，常见于设置页、详情页的多视图切换场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `TabBar` | struct | 标签栏容器，承载多个 Tab |
| `Tab` | struct | 单个标签项 |
| `Tab::new` | 构造函数 | 创建一个 Tab，传入唯一 id |
| `TabBar::new` | 构造函数 | 创建标签栏 |
| `on_change` | 方法 | 激活标签切换时触发的回调 |

## 基本用法

```rust
use gpui_component::tab::{Tab, TabBar};

// 定义标签 id
let tab_dashboard = gpui::ElementId::from("dashboard");
let tab_analytics = gpui::ElementId::from("analytics");

let tab_bar = TabBar::new()
    .tab(Tab::new(tab_dashboard).label("Dashboard"))
    .tab(Tab::new(tab_analytics).label("Analytics"))
    .on_change(|active_tab, _, _| {
        println!("Active tab changed: {:?}", active_tab);
    });

// 根据当前激活的 tab 渲染对应内容面板
```

## 备注

无特别说明。
