# Radio

> 单选框组件，用于从一组互斥选项中选择一个值，支持水平/垂直布局、多尺寸、受控状态与回调监听。

源文档: https://longbridge.github.io/gpui-component/docs/components/radio

## 简介

Radio 是一组可勾选按钮（单选按钮），同一时刻最多只有一个处于选中状态。当需要让用户在多个互斥选项中选一个时使用。推荐通过 `RadioGroup` 管理互斥选项组，它支持横向/纵向布局、整体禁用、按索引选中及选择变化回调。

## 核心 API

### Radio

| 名称 | 类型 | 说明 |
|------|------|------|
| `new(id)` | 构造器 | 创建单选项，需传入唯一 id |
| `label(text)` | impl IntoLabel | 设置选项显示文本 |
| `checked(bool)` | bool | 设置选中状态 |
| `disabled(bool)` | bool | 是否禁用此选项 |
| `on_click(fn)` | Fn(&bool) | 点击回调，接收新的选中状态 |
| `tab_stop(bool)` | bool | 是否参与 Tab 导航（默认 true） |
| `tab_index(isize)` | isize | 设置 Tab 顺序索引（默认 0） |
| `xsmall()` / `small()` / `large()` | - | 尺寸（默认 medium） |

### RadioGroup

| 名称 | 类型 | 说明 |
|------|------|------|
| `horizontal(id)` / `vertical(id)` | 构造器 | 创建横向/纵向单选组 |
| `layout(Axis)` | Axis | 设置布局方向（Vertical / Horizontal） |
| `child(Radio)` | Radio | 添加单个单选项 |
| `children(items)` | IntoIterator | 批量添加（字符串迭代器会生成默认 Radio） |
| `selected_index(Option<usize>)` | Option<usize> | 受控设置选中项索引 |
| `disabled(bool)` | bool | 禁用组内所有单选项 |
| `on_change(fn)` | Fn(&usize) | 选择变化回调，接收选中索引 |

## 基本用法

```rust
use gpui_component::radio::{Radio, RadioGroup};

Radio::new("radio-option-1")
    .label("Option 1")
    .checked(false)
    .on_click(|checked, _, _| {
        println!("Radio is now: {}", checked);
    })
```

受控单选按钮：

```rust
struct MyView {
    radio_checked: bool,
}

impl Render for MyView {
    fn render(&mut self, _: &mut Window, cx: &mut Context<Self>) -> impl IntoElement {
        Radio::new("radio")
            .label("Select this option")
            .checked(self.radio_checked)
            .on_click(cx.listener(|view, checked, _, cx| {
                view.radio_checked = *checked;
                cx.notify();
            }))
    }
}
```

RadioGroup（推荐用法）：

```rust
struct MyView {
    selected_option: Option<usize>,
}

impl Render for MyView {
    fn render(&mut self, _: &mut Window, cx: &mut Context<Self>) -> impl IntoElement {
        RadioGroup::horizontal("options")
            .children(["Option 1", "Option 2", "Option 3"])
            .selected_index(self.selected_option)
            .on_change(cx.listener(|view, selected_index: &usize, _, cx| {
                view.selected_option = Some(*selected_index);
                cx.notify();
            }))
    }
}
```

不同尺寸：

```rust
Radio::new("small").label("Small").xsmall()
Radio::new("medium").label("Medium") // default
Radio::new("large").label("Large").large()
```

禁用状态：

```rust
Radio::new("disabled")
    .label("Disabled option")
    .disabled(true)
    .checked(false)

Radio::new("disabled-checked")
    .label("Disabled and checked")
    .checked(true)
    .disabled(true)
```

带附加描述的多行标签：

```rust
Radio::new("custom")
    .label("Primary option")
    .child(
        div()
            .text_color(cx.theme().muted_foreground)
            .child("This is additional descriptive text that provides more context.")
    )
    .w(px(300.))
```

自定义 Tab 顺序：

```rust
Radio::new("radio")
    .label("Custom tab order")
    .tab_index(2)
    .tab_stop(true)
```

## 备注

- 互斥选择请优先使用 `RadioGroup`，而非手动管理多个独立 `Radio`。
- RadioGroup 的 `on_change` 回调接收的是选中项的**索引**（`&usize`），而非值；选中状态通过 `selected_index(Option<usize>)` 受控设置。
- RadioGroup 上设置 `disabled(true)` 会禁用组内所有单选项。
- Radio 与 RadioGroup 均实现 `Styled` trait，可自定义样式；Radio 还实现 `Sizable` trait 支持四种尺寸。
- 选项较多时用纵向布局，选项较少时可用横向布局。
