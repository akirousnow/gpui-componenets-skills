# NumberInput

> 数值输入框，内置增减按钮，支持最大/最小值、步进和千分位格式化。

源文档: https://longbridge.github.io/gpui-component/docs/components/number-input

## 简介

NumberInput 是一个专用于数值输入的组件，内置增/减按钮，支持最大/最小值校验、步进值以及带千分位的数字格式化。适合用于计数器、金额输入、数量选择等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `NumberInput` | struct | 数值输入组件 |
| `NumberInputEvent` | enum | 数值输入事件，含 `Step(StepAction)` |
| `StepAction` | enum | 步进动作：`Increment`、`Decrement` |
| `InputState` | struct | 输入状态实体（复用通用 Input 的状态） |
| `MaskPattern::Number` | enum | 数字格式化模式，含 `separator` 和 `fraction` |
| `NumberInput::new(state)` | 构造函数 | 创建数值输入框 |
| `.placeholder(str)` | 方法 | 设置占位文本 |
| `.prefix(el)` / `.suffix(el)` | 方法 | 添加前缀/后缀元素 |
| `.appearance(bool)` | 方法 | 是否启用默认样式 |
| `.disabled(bool)` | 方法 | 设置禁用状态 |
| `.small()` / `.large()` | 方法 | 设置尺寸 |
| `NumberInput::increment(state, win, cx)` | 关联函数 | 编程式递增 |
| `NumberInput::decrement(state, win, cx)` | 关联函数 | 编程式递减 |
| `InputState::pattern(regex)` | 方法 | 设置正则校验（如仅整数） |
| `InputState::mask_pattern(...)` | 方法 | 设置数字格式化 |
| `InputState::unmask_value()` | 方法 | 获取未格式化的真实数值 |

## 基本用法

```rust
use gpui_component::input::{InputState, NumberInput, NumberInputEvent, StepAction};

// 基础数值输入
let number_input = cx.new(|cx|
    InputState::new(window, cx)
        .placeholder("Enter number")
        .default_value("1")
);

NumberInput::new(&number_input)
```

带千分位的金额输入：

```rust
use gpui_component::input::MaskPattern;

let currency_input = cx.new(|cx|
    InputState::new(window, cx)
        .placeholder("Amount")
        .mask_pattern(MaskPattern::Number {
            separator: Some(','),
            fraction: Some(2),
        })
);

NumberInput::new(&currency_input).prefix(div().child("$"))
```

监听增减按钮事件：

```rust
cx.subscribe_in(&number_input, window, |view, state, event, window, cx| {
    match event {
        NumberInputEvent::Step(StepAction::Increment) => {
            view.value += 1;
            state.update(cx, |input, cx| {
                input.set_value(view.value.to_string(), window, cx);
            });
        }
        NumberInputEvent::Step(StepAction::Decrement) => {
            view.value -= 1;
            state.update(cx, |input, cx| {
                input.set_value(view.value.to_string(), window, cx);
            });
        }
    }
});
```

## 备注

- 键盘支持：`↑` 递增、`↓` 递减、`Enter` 确认、`Escape` 清空（如启用）。
- 数值校验需在客户端和后端都做，建议设置合理的范围和步进值。
- 可用 `appearance(false)` 关闭默认样式以便自定义容器外观。
