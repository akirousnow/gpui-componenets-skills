# Clipboard

> 一键复制文本到剪贴板的按钮组件，复制成功后图标自动变为对勾。

源文档: https://longbridge.github.io/gpui-component/docs/components/clipboard

## 简介

Clipboard 组件提供了一种将文本或其他数据复制到用户剪贴板的便捷方式。它渲染为一个带复制图标的按钮，在内容成功复制后图标会变为对勾。组件支持静态值（`value`）和动态内容（`value_fn` 闭包），常用于复制 API Key、分享链接、配合输入框作为后缀使用。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Clipboard::new(id)` | 构造函数 | 创建剪贴板按钮 |
| `value(text)` | 方法 | 设置静态待复制文本 |
| `value_fn(closure)` | 方法 | 设置动态内容闭包，点击时才计算值 |
| `on_copied(cb)` | 回调 | 复制成功后回调，参数为已复制的值 |

## 基本用法

基础复制：

```rust
use gpui_component::clipboard::Clipboard;

Clipboard::new("my-clipboard")
    .value("Text to copy")
    .on_copied(|value, window, cx| {
        window.push_notification(format!("Copied: {}", value), cx)
    })
```

动态值（`value_fn`，点击时才计算）：

```rust
let state = some_state.clone();
Clipboard::new("dynamic-clipboard")
    .value_fn(move |_, cx| {
        state.read(cx).get_current_value()
    })
    .on_copied(|value, window, cx| {
        window.push_notification(format!("Copied: {}", value), cx)
    })
```

作为输入框后缀使用：

```rust
use gpui_component::input::{InputState, Input};

let url_state = cx.new(|cx| InputState::new(window, cx).default_value("https://github.com"));

Input::new(&url_state)
    .suffix(
        Clipboard::new("url-clipboard")
            .value_fn({
                let state = url_state.clone();
                move |_, cx| state.read(cx).value()
            })
            .on_copied(|value, window, cx| {
                window.push_notification(format!("URL copied: {}", value), cx)
            })
    )
```

## 备注

- 目前仅支持复制文本字符串，底层使用 GPUI 的 `ClipboardItem::new_string()`。
- 支持 UTF-8 编码内容，跨平台剪贴板集成。
- `value_fn` 适合内容依赖当前状态或计算开销较大的场景，只在点击时才执行。
