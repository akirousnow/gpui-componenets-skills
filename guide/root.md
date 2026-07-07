# Root View

> GPUI Component 功能的根 Provider，必须作为窗口的第一层子元素。

源文档: https://longbridge.github.io/gpui-component/docs/root

## 简介

`Root` 是在窗口中启用 GPUI Component 功能的根组件。**必须**将 `Root` 作为窗口（window）的第一层子元素，否则组件将出现不可预期的行为。

`Root` 接收一个 view 并将其包装起来，同时完成组件库所需的初始化工作。此外，`Root` 还支持通过 `Styled` 链式方法设置窗口级基础样式（如字体、字号），所有子 view 都会继承这些样式。

## 关键要点

- **必需位置**：`Root` 必须是窗口的第一层子元素（first level child）。
- **初始化**：在使用任何 GPUI Component 功能前，必须调用 `gpui_component::init(cx)`。
- **构造方式**：`Root::new(view.into(), window, cx)` 接收 view、window 和 context。
- **样式继承**：通过 `Styled` trait 方法在 `Root` 上设置的基础样式会被所有子 view 继承。
- **默认字体**：默认使用 `.SystemUIFont`（系统 UI 字体）。

## 代码示例

在窗口中使用 `Root` 作为第一层子元素：

```rust
fn main() {
    let app = Application::new();

    app.run(move |cx| {
        // This must be called before using any GPUI Component features.
        gpui_component::init(cx);

        cx.spawn(async move |cx| {
            cx.open_window(WindowOptions::default(), |window, cx| {
                let view = cx.new(|_| Example);
                // This first level on the window, should be a Root.
                cx.new(|cx| Root::new(view.into(), window, cx))
            })?;

            Ok::<_, anyhow::Error>(())
        })
        .detach();
    });
}
```

通过 `Styled` 方法在 `Root` 上设置窗口级基础样式：

```rust
Root::new(view.into(), window, cx)
    // This default is `.SystemUIFont` it will use system UI font.
    .font_family("Your Special Font")
    .text_sm()
```

## 备注

- `gpui_component::init(cx)` 只需调用一次，通常放在 `Application::run` 回调的开头。
- 不使用 `Root` 或未将其置于第一层会导致弹出菜单、Tooltip 等组件行为异常。
- `.font_family`、`.text_sm` 等方法来自 GPUI 的 `Styled` trait。
