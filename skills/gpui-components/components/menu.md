# Menu

> 支持图标、快捷键、子菜单、可勾选项和自定义元素的上下文菜单与弹出菜单组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/menu

## 简介

Menu 组件提供上下文菜单（右键菜单）和弹出菜单（下拉菜单），内置图标、键盘快捷键、子菜单、分隔符、可勾选项和自定义元素等功能，并具备完整的键盘导航与无障碍支持。菜单项与 GPUI 的 Action 系统深度集成，可自动显示绑定的快捷键。

## 核心 API

### 核心 struct / trait

| 名称 | 类型 | 说明 |
|------|------|------|
| `PopupMenu` | 菜单构建器 | 链式构建菜单内容 |
| `PopupMenuItem` | 项 | 不依赖 Action 的自定义菜单项 |
| `ContextMenuExt` | trait | 提供 `context_menu` 扩展方法 |
| `DropdownMenu` | trait | 按钮等元素的下拉菜单扩展 |

### PopupMenu 常用方法

| 名称 | 说明 |
|------|------|
| `menu(name, action)` | 添加菜单项（关联 Action） |
| `menu_with_icon(name, icon, action)` | 带图标菜单项 |
| `menu_with_check(name, checked, action)` | 可勾选菜单项 |
| `menu_with_disabled(name, action, disabled)` | 禁用菜单项 |
| `separator()` | 分隔线 |
| `label(text)` | 非交互分组标签 |
| `link(name, url)` / `link_with_icon` | 外部链接项 |
| `submenu(name, window, cx, closure)` | 子菜单 |
| `submenu_with_icon` | 带图标子菜单 |
| `item(PopupMenuItem)` | 不依赖 Action 的自定义项 |
| `menu_element(action, closure)` | 自定义元素菜单项 |
| `action_context(focus_handle)` | 设置 Action 焦点上下文 |
| `scrollable(bool)` / `max_h(h)` | 滚动与尺寸 |
| `min_w(w)` / `max_w(w)` | 宽度约束 |

### PopupMenuItem 方法

| 名称 | 说明 |
|------|------|
| `new(name)` | 创建自定义项 |
| `.icon(icon)` / `.disabled(bool)` | 图标与禁用 |
| `.on_click(closure)` | 点击回调 |

## 基本用法

```rust
use gpui_component::{menu::{PopupMenu, PopupMenuItem, ContextMenuExt}, Button};
use gpui::{actions, Action};

actions!(my_app, [Copy, Paste, Delete]);

// 右键上下文菜单
div()
    .child("Right click me")
    .context_menu(|menu, window, cx| {
        menu.menu("Copy", Box::new(Copy))
            .menu("Paste", Box::new(Paste))
            .separator()
            .menu("Delete", Box::new(Delete))
    })
```

下拉菜单与子菜单：

```rust
Button::new("menu-btn")
    .label("Open Menu")
    .dropdown_menu(|menu, window, cx| {
        menu.menu_with_icon("Search", IconName::Search, Box::new(Search))
            .separator()
            .submenu("File", window, cx, |submenu, window, cx| {
                submenu.menu("New", Box::new(NewFile))
                      .menu("Open", Box::new(OpenFile))
            })
    })
```

不使用 Action 的自定义项：

```rust
menu.item(
    PopupMenuItem::new("Custom Action")
        .icon(IconName::Star)
        .on_click(|window, cx| println!("Clicked!"))
)
```

## 备注

- 菜单项推荐使用 Action 模式，可自动显示快捷键并与 GPUI 键绑定系统集成
- 启用 `scrollable()` 后避免再使用子菜单，否则会有可用性问题
- 支持的键盘操作：`↑↓` 导航、`←→` 子菜单、`Enter/Space` 激活、`Esc` 关闭、`Tab` 关闭并切焦
- 可通过 `menu_element` / `menu_element_with_check` 插入任意复杂内容（如状态行）
