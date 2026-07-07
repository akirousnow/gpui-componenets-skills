# Input

> 支持验证、掩码、前后缀元素和多种状态的灵活文本输入组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/input

## 简介

Input 是一个灵活的文本输入组件，支持校验、输入掩码、前缀/后缀元素以及多种状态（禁用、密码可见性切换等）。它通过 `InputState` 管理状态，适合用于表单、搜索框、密码输入、金额输入等场景。

## 核心 API

### InputState 方法

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(window, cx)` | 构造函数 | 创建输入状态 |
| `placeholder(text)` | 方法 | 设置占位文本 |
| `default_value(value)` | 方法 | 设置默认值 |
| `masked(bool)` | 方法 | 是否掩码显示（密码） |
| `clean_on_escape()` | 方法 | 按 ESC 清空 |
| `validate(closure)` | 方法 | 自定义校验 |
| `pattern(regex)` | 方法 | 正则校验 |
| `mask_pattern(pattern)` | 方法 | 输入掩码 |
| `context_menu(bool)` | 方法 | 是否启用右键菜单 |

### Input 方法

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(&state)` | 构造函数 | 绑定到 InputState |
| `placeholder` / `prefix(el)` / `suffix(el)` | 方法 | 占位/前缀/后缀 |
| `cleanable(bool)` | 方法 | 显示清除按钮 |
| `mask_toggle()` | 方法 | 密码可见性切换按钮 |
| `disabled(bool)` | 方法 | 禁用输入 |
| `appearance(bool)` | 方法 | 是否启用默认样式 |
| `large()` / `small()` | 方法 | 尺寸控制（默认 medium） |
| `context_menu(closure)` | 方法 | 自定义右键菜单 |

### InputEvent 事件

| 事件 | 说明 |
|------|------|
| `InputEvent::Change` | 内容变化 |
| `InputEvent::PressEnter { secondary }` | 按下回车 |
| `InputEvent::Focus` / `Blur` | 获得/失去焦点 |

## 基本用法

```rust
use gpui_component::input::{InputState, Input};

let input = cx.new(|cx| InputState::new(window, cx).placeholder("Enter your name..."));

Input::new(&input)
```

带前缀图标与密码切换：

```rust
// 搜索框
let search = cx.new(|cx| InputState::new(window, cx).placeholder("Search..."));
Input::new(&search).prefix(Icon::new(IconName::Search).small())

// 密码框
let pwd = cx.new(|cx|
    InputState::new(window, cx).masked(true).default_value("password123")
);
Input::new(&pwd).mask_toggle()
```

监听事件：

```rust
cx.subscribe_in(&input, window, |view, state, event, window, cx| {
    match event {
        InputEvent::Change => println!("{}", state.read(cx).value()),
        InputEvent::PressEnter { .. } => {}
        InputEvent::Focus | InputEvent::Blur => {}
    }
});
```

## 备注

- 掩码模式支持：`MaskPattern::Number { separator, fraction }` 用于数字千分位与小数
- 掩码占位符：`9`=可选数字、`#`=数字、`A`=字母
- 通过 `appearance(false)` 可去除默认样式，嵌入自定义容器
- 菜单项与 GPUI 的 Action 系统集成，可自动显示快捷键
