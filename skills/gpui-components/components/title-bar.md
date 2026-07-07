# TitleBar

> 可自定义的窗口标题栏，可替代系统默认标题栏。

源文档: https://longbridge.github.io/gpui-component/docs/components/title-bar

## 简介

TitleBar 提供一个可自定义的窗口标题栏，用于替代操作系统的默认标题栏。它内置平台相关的窗口控制按钮（最小化、最大化、关闭），并支持自定义内容与布局，常用于需要统一跨平台外观或集成自定义工具栏的桌面应用。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `TitleBar` | struct | 标题栏组件主体 |
| `TitleBar::new` | 构造函数 | 创建标题栏 |
| `title` | 方法 | 设置标题文字 |
| `child` / `content` | 方法 | 设置自定义内容区（如菜单栏、搜索框） |
| `show_traffic_lights` | 方法 | 是否显示窗口控制按钮（macOS 风格红绿灯） |

## 基本用法

```rust
use gpui_component::title_bar::TitleBar;

let title_bar = TitleBar::new()
    .title("My Application")
    .child(
        // 自定义标题栏内容，如菜单或工具栏
        div().child("Custom Content"),
    );
```

## 备注

无特别说明。
