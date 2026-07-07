# AlertDialog

> 打断用户操作、要求用户确认或响应的模态对话框。

源文档: https://longbridge.github.io/gpui-component/docs/components/alert-dialog

## 简介

AlertDialog 是基于 Dialog 构建的模态对话框，专门用于确认类场景（如删除、危险操作、重要通知）。相比 Dialog，它做了如下约定：默认点击遮罩不关闭、默认无关闭按钮、底部按钮居中对齐，并提供更精简的 API。提供声明式（trigger + content）和命令式（`open_alert_dialog`）两套使用方式。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `AlertDialog::new(cx)` | 构造函数 | 创建 AlertDialog（声明式） |
| `trigger(element)` | 方法 | 设置触发元素，点击后打开对话框 |
| `content(builder)` | 方法 | 通过闭包构建对话框内容（声明式） |
| `title(text)` | 方法 | 设置标题（命令式 API） |
| `description(text)` | 方法 | 设置描述（命令式 API） |
| `icon(icon)` | 方法 | 设置图标 |
| `button_props(props)` | 方法 | 配置按钮文字、样式、显隐 |
| `width(px)` | 方法 | 设置宽度，默认 `420px` |
| `overlay_closable(bool)` | 方法 | 点击遮罩是否关闭，默认 `false` |
| `close_button(bool)` | 方法 | 是否显示关闭按钮，默认 `false` |
| `keyboard(bool)` | 方法 | 是否支持 ESC 关闭，默认 `true` |
| `on_ok(cb)` / `on_cancel(cb)` | 回调 | 确认/取消回调，返回 `true` 关闭对话框 |
| `on_close(cb)` | 回调 | 对话框关闭后回调 |
| `window.open_alert_dialog(cx, builder)` | 函数 | 命令式打开对话框 |
| `DialogAction` / `DialogClose` | 组件 | 包裹按钮，自动触发确认/取消动作 |

## 基本用法

声明式 API（配合 `DialogAction` / `DialogClose`）：

```rust
use gpui_component::dialog::{AlertDialog, DialogAction, DialogClose,
    DialogHeader, DialogTitle, DialogDescription, DialogFooter};

AlertDialog::new(cx)
    .trigger(Button::new("show-alert").outline().label("Show Alert"))
    .on_ok(|_, window, cx| {
        window.push_notification("You confirmed!", cx);
        true
    })
    .on_cancel(|_, window, cx| {
        window.push_notification("You cancelled!", cx);
        true
    })
    .content(|content, _, cx| {
        content
            .child(
                DialogHeader::new()
                    .child(DialogTitle::new().child("Confirm Action"))
                    .child(DialogDescription::new().child("Do you want to proceed?"))
            )
            .child(
                DialogFooter::new()
                    .child(
                        DialogClose::new().child(
                            Button::new("cancel").outline().label("Cancel")
                        )
                    )
                    .child(
                        DialogAction::new().child(
                            Button::new("ok").primary().label("Confirm")
                        )
                    )
            )
    })
```

命令式 API：

```rust
use gpui_component::WindowExt;

window.open_alert_dialog(cx, |alert, _, _| {
    alert
        .title("Delete File")
        .description("Are you sure you want to delete this file? This action cannot be undone.")
        .show_cancel(true)
        .on_ok(|_, window, cx| {
            window.push_notification("File deleted", cx);
            true
        })
})
```

## 备注

- 在 `on_ok` / `on_cancel` 中返回 `false` 可阻止对话框关闭。
- 使用前需设置应用根视图以渲染对话框层，详见 Dialog 文档。
- 危险操作建议用 `ButtonVariant::Danger` 样式明确意图。
