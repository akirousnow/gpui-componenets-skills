# Dialog

> 用于在应用层之上展示对话框、确认框和提示框的组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/dialog

## 简介

Dialog 用于创建对话框、确认框和提示框，支持遮罩层、键盘快捷键（ESC 关闭）、嵌套对话框和自定义样式。需先在应用根视图中通过 `Root::render_dialog_layer` 渲染对话框层，然后通过 `window.open_dialog` 打开对话框。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Root::render_dialog_layer(window, cx)` | 方法 | 在根视图渲染对话框层 |
| `window.open_dialog(cx, closure)` | 方法 | 打开对话框 |
| `window.close_dialog(cx)` | 方法 | 关闭当前对话框 |
| `dialog.title(&str)` | 方法 | 设置标题 |
| `dialog.child(element)` | 方法 | 设置内容 |
| `dialog.footer(closure)` | 方法 | 设置底部按钮区 |
| `dialog.confirm()` | 方法 | 切换为确认框模式 |
| `dialog.alert()` | 方法 | 切换为提示框模式 |
| `dialog.on_ok(closure)` | 方法 | 设置确认回调，返回 `bool`（true 关闭） |
| `dialog.on_cancel(closure)` | 方法 | 设置取消回调 |
| `dialog.button_props(DialogButtonProps)` | 方法 | 自定义按钮文本与样式 |
| `dialog.overlay(bool)` | 方法 | 是否显示遮罩层 |
| `dialog.keyboard(bool)` | 方法 | 是否启用 ESC 关闭 |
| `dialog.close_button(bool)` | 方法 | 是否显示关闭按钮 |

## 基本用法

```rust
use gpui_component::dialog::DialogButtonProps;
use gpui_component::WindowExt;
```

根视图渲染对话框层（必需）：

```rust
use gpui_component::TitleBar;

impl Render for MyApp {
    fn render(&mut self, window: &mut Window, cx: &mut Context<Self>) -> impl IntoElement {
        let dialog_layer = Root::render_dialog_layer(window, cx);

        div()
            .size_full()
            .child(
                v_flex()
                    .size_full()
                    .child(TitleBar::new())
                    .child(div().flex_1().overflow_hidden().child(self.view.clone())),
            )
            .children(dialog_layer)
    }
}
```

基础对话框：

```rust
window.open_dialog(cx, |dialog, _, _| {
    dialog
        .title("Welcome")
        .child("This is a dialog dialog.")
})
```

确认框：

```rust
window.open_dialog(cx, |dialog, _, _| {
    dialog
        .confirm()
        .child("Are you sure you want to delete this item?")
        .on_ok(|_, window, cx| {
            window.push_notification("Item deleted", cx);
            true
        })
})
```

自定义按钮文本：

```rust
use gpui_component::button::ButtonVariant;

window.open_dialog(cx, |dialog, _, _| {
    dialog
        .confirm()
        .child("Update available. Restart now?")
        .button_props(
            DialogButtonProps::default()
                .cancel_text("Later")
                .ok_text("Restart Now")
                .ok_variant(ButtonVariant::Danger)
        )
        .on_ok(|_, window, cx| {
            window.push_notification("Restarting...", cx);
            true
        })
})
```

## 备注

使用前必须在根视图中调用 `Root::render_dialog_layer`，否则对话框不会显示。`on_ok`/`on_cancel` 回调返回 `true` 表示关闭对话框，返回 `false` 则保持打开。
