# Sidebar

> 可组合、可主题化的侧边栏导航组件，支持折叠、嵌套菜单与响应式设计。

源文档: https://longbridge.github.io/gpui-component/docs/components/sidebar

## 简介

Sidebar 是一个灵活的侧边栏组件，为应用提供导航结构。它支持折叠状态、嵌套菜单项、头部和底部区域，以及响应式设计。适用于应用导航面板、管理后台仪表盘和复杂层级界面。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Sidebar` | 结构体 | 主侧边栏，通过 `new(side)` 创建，默认宽度 255px |
| `SidebarHeader` | 结构体 | 头部区域，支持下拉菜单 |
| `SidebarFooter` | 结构体 | 底部区域 |
| `SidebarGroup` | 结构体 | 带标签的菜单分组 |
| `SidebarMenu` | 结构体 | 菜单容器 |
| `SidebarMenuItem` | 结构体 | 菜单项，支持图标、子菜单、后缀 |
| `SidebarToggleButton` | 结构体 | 折叠/展开切换按钮 |
| `Side` | 枚举 | 放置方向：`Left` / `Right` |
| `collapsed(bool)` | 方法 | 设置折叠状态 |
| `collapsible(bool)` | 方法 | 是否可折叠（默认 true） |

## 基本用法

```rust
use gpui_component::{sidebar::*, Side};

Sidebar::new(Side::Left)
    .header(SidebarHeader::new().child("My Application"))
    .child(
        SidebarGroup::new("Navigation").child(
            SidebarMenu::new()
                .child(
                    SidebarMenuItem::new("Dashboard")
                        .icon(IconName::LayoutDashboard)
                        .on_click(|_, _, _| println!("Dashboard clicked")),
                )
                .child(
                    SidebarMenuItem::new("Settings")
                        .icon(IconName::Settings),
                ),
        ),
    )
    .footer(SidebarFooter::new().child("User Profile"))
```

带徽标和开关后缀的菜单项：

```rust
SidebarMenuItem::new("Notifications")
    .icon(IconName::Bell)
    .suffix(Badge::new().count(5).child("5"));

SidebarMenuItem::new("Dark Mode")
    .icon(IconName::Moon)
    .suffix(Switch::new("dark-mode").checked(true).xsmall());
```

可折叠侧边栏与切换按钮：

```rust
Sidebar::new(Side::Left)
    .collapsed(collapsed)
    .collapsible(true)
    .child(SidebarGroup::new("Menu").child(
        SidebarMenu::new().child(SidebarMenuItem::new("Files").icon(IconName::Folder)),
    ));

SidebarToggleButton::left().collapsed(collapsed).on_click(|_, _, _| {
    collapsed = !collapsed;
})
```

## 备注

- 侧边栏使用专用主题色：`sidebar`、`sidebar_foreground`、`sidebar_border`、`sidebar_accent` 等，可在主题 JSON 中定制。
- `SidebarMenuItem` 通过 `children` 支持嵌套子菜单。
