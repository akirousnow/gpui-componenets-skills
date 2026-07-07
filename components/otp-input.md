# OtpInput

> 一次性密码（OTP）输入组件，多输入框自动聚焦并支持粘贴整段验证码。

源文档: https://longbridge.github.io/gpui-component/docs/components/otp-input

## 简介

OtpInput 是一个专为一次性密码（One-Time Password）设计的输入组件。它由多个独立的输入框组成，输入后会自动跳到下一个框，并支持将整段验证码粘贴后自动分配到各框。常用于短信验证码、邮箱验证码、双因素认证（2FA）等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `OtpInput` | struct | OTP 输入组件 |
| `OtpInputState` | struct | OTP 输入状态实体 |
| `OtpInput::new(state)` | 构造函数 | 创建 OTP 输入框组 |
| `.placeholder(str)` | 方法 | 设置占位字符 |
| `.disabled(bool)` | 方法 | 设置禁用状态 |
| `.password_mode(bool)` | 方法 | 是否以密文（圆点）显示 |
| `.appearance(bool)` | 方法 | 是否启用默认样式 |
| `.gap(px)` / `.width(px)` / `.height(px)` | 方法 | 设置间距与每个框的尺寸 |
| `OtpInputState::new(count, win, cx)` | 构造函数 | 创建指定框数的状态 |
| `OtpInputState::value()` | 方法 | 获取已输入的字符串 |
| `OtpInputState::set_value(...)` | 方法 | 编程式设置值 |
| `OtpInputState::is_complete()` | 方法 | 判断是否所有框都已填入 |

## 基本用法

```rust
use gpui_component::input::{OtpInput, OtpInputState};

let otp_input = cx.new(|cx| OtpInputState::new(6, window, cx));

OtpInput::new(otp_input.clone())
    .placeholder("•")
    .width(px(48.))
    .gap(px(8.))
```

密文显示模式：

```rust
OtpInput::new(otp_input.clone())
    .password_mode(true)
```

监听输入完成并获取值：

```rust
// 在事件回调或渲染中读取
let value = otp_input.read(cx).value();
if otp_input.read(cx).is_complete() {
    // 验证码已填满，可提交校验
}
```

## 备注

- 粘贴整段验证码时会自动按字符顺序分配到各输入框。
- 默认只接受单个数字/字母字符，多余字符会被忽略。
- 可通过 `appearance(false)` 关闭默认样式以完全自定义外观。
- 输入框数量通过 `OtpInputState::new(count, ...)` 指定，常见为 4 或 6 位。
