# Notification

> 在窗口右上角显示临时 Toast 通知，支持多种类型、自动消失和操作按钮。

源文档: https://longbridge.github.io/gpui-component/docs/components/notification

## 简介

Notification 是一个 Toast 通知系统，用于向用户展示临时消息。通知会出现在窗口的右上角，可在超时后自动消失。支持多种变体（info、success、warning、error）、自定义内容、标题和操作按钮。非常适合用于状态更新、操作确认和用户反馈场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Notification` | struct | 通知构建器，用于构造通知实例 |
| `NotificationType` | enum | 通知类型：`Info`、`Success`、`Warning`、`Error` |
| `Notification::new()` | 构造函数 | 创建一个默认通知 |
| `Notification::info(msg)` / `success` / `warning` / `error` | 构造函数 | 快捷创建指定类型的通知 |
| `.title(str)` | 方法 | 设置通知标题 |
| `.message(str)` | 方法 | 设置通知消息内容 |
| `.with_type(NotificationType)` | 方法 | 设置通知类型 |
| `.autohide(bool)` | 方法 | 是否自动隐藏，默认 true（5 秒后） |
| `.action(closure)` | 方法 | 添加操作按钮 |
| `.on_click(closure)` | 方法 | 点击通知时的回调 |
| `.content(closure)` | 方法 | 自定义通知内容元素 |
| `.id::<T>()` / `.id1::<T>(id)` | 方法 | 设置唯一 ID，便于后续移除或更新 |
| `window.push_notification(...)` | WindowExt 方法 | 推送通知到通知层 |
| `window.remove_notification::<T>(cx)` | WindowExt 方法 | 根据类型 ID 移除通知 |
| `Root::render_notification_layer` | 函数 | 在根视图渲染通知层 |

## 基本用法

```rust
use gpui_component::notification::{Notification, NotificationType};
use gpui_component::WindowExt;

// 简单字符串通知
window.push_notification("This is a notification.", cx);

// 使用 Notification builder
Notification::new()
    .message("Your changes have been saved.")
```

应用根视图需渲染通知层：

```rust
use gpui_component::TitleBar;

struct MyApp {
    view: AnyView,
}

impl Render for MyApp {
    fn render(&mut self, window: &mut Window, cx: &mut Context<Self>) -> impl IntoElement {
        let notification_layer = Root::render_notification_layer(window, cx);

        div()
            .size_full()
            .child(
                v_flex()
                    .size_full()
                    .child(TitleBar::new())
                    .child(div().flex_1().overflow_hidden().child(self.view.clone())),
            )
            .children(notification_layer)
    }
}
```

不同类型的通知：

```rust
window.push_notification((NotificationType::Info, "File saved successfully."), cx);
window.push_notification((NotificationType::Success, "Payment processed."), cx);
window.push_notification((NotificationType::Warning, "Network unstable."), cx);
window.push_notification((NotificationType::Error, "Failed to save file."), cx);
```

带标题和操作按钮的通知：

```rust
Notification::new()
    .title("Update Available")
    .message("A new version is ready to install.")
    .with_type(NotificationType::Info)
```

使用唯一 ID 管理通知（可后续移除）：

```rust
struct UpdateNotification;

Notification::new()
    .id::<UpdateNotification>()
    .message("System update available")
    .autohide(false)

// 后续移除
window.remove_notification::<UpdateNotification>(cx);
```

## 备注

- 通知位置固定在窗口右上角，最多同时显示 10 条。
- 默认自动隐藏延时为 5 秒，鼠标悬停时计时器暂停。
- 出现动画 0.25s（下滑淡入），消失动画 0.15s（右滑淡出）。
- 错误类通知建议关闭自动隐藏并要求用户操作。
