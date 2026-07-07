# Kbd

> 显示键盘快捷键，并按平台（macOS/Windows/Linux）自动格式化。

源文档: https://longbridge.github.io/gpui-component/docs/components/kbd

## 简介

Kbd 组件用于展示键盘快捷键和组合键，并能根据运行平台自动适配显示风格：macOS 使用符号（⌃ ⌥ ⇧ ⌘），Windows/Linux 使用文本（Ctrl、Alt、Shift、Win）。常用于菜单项、快捷键帮助面板和行内说明文档。

## 核心 API

### Kbd 方法

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(keystroke)` | 构造函数 | 从 `Keystroke` 创建 |
| `appearance(bool)` | 方法 | 是否显示样式化背景（默认 true） |
| `text_color` / `border_color` / `bg` | 方法 | 自定义样式（Styled trait） |

### 关联函数

| 名称 | 说明 |
|------|------|
| `Kbd::format(&keystroke)` | 返回纯文本格式，不渲染样式 |
| `Kbd::binding_for_action(action, None, window)` | 获取 Action 的首个快捷键 |
| `Kbd::binding_for_action(action, Some(ctx), window)` | 在指定上下文中获取快捷键 |
| `Kbd::binding_for_action_in(action, &focus_handle, window)` | 在指定焦点元素下获取快捷键 |

### 平台显示差异示例

| 输入 | macOS | Windows/Linux |
|------|-------|---------------|
| `cmd-a` | ⌘A | Win+A |
| `ctrl-shift-a` | ⌃⇧A | Ctrl+Shift+A |
| `escape` | ⎋ | Esc |
| `enter` | ⏎ | Enter |
| `left` | ← | Left |

## 基本用法

```rust
use gpui_component::kbd::Kbd;
use gpui::Keystroke;

// 从 keystroke 创建
let kbd = Kbd::new(Keystroke::parse("cmd-shift-p").unwrap());

// 或直接转换
let kbd: Kbd = Keystroke::parse("escape").unwrap().into();

// 不带样式背景
Kbd::new(Keystroke::parse("cmd-s").unwrap()).appearance(false)
```

快捷键帮助面板：

```rust
use gpui::{div, h_flex, v_flex};

v_flex()
    .gap_2()
    .child(
        h_flex().gap_2().items_center()
            .child("Open command palette:")
            .child(Kbd::new(Keystroke::parse("cmd-shift-p").unwrap()))
    )
    .child(
        h_flex().gap_2().items_center()
            .child("Save file:")
            .child(Kbd::new(Keystroke::parse("cmd-s").unwrap()))
    )
```

从 Action 绑定获取：

```rust
if let Some(kbd) = Kbd::binding_for_action(&MyAction {}, None, window) {
    // 显示绑定的快捷键
}
```

## 备注

- macOS 修饰键顺序：Control、Option、Shift、Command
- 默认样式：边框 + 次要文字色 + 小圆角 + 极小内边距，禁用 flex-shrink
- 可通过 `Styled` trait 完全自定义颜色与边框
- 行内文档建议用 `Kbd::format` 获取纯文本以降低渲染开销
