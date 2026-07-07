# Form

> 灵活的表单容器组件，支持字段布局、校验、分组与多列响应式布局。

源文档: https://longbridge.github.io/gpui-component/docs/components/form

## 简介

Form 提供结构化的表单字段布局，支持垂直（`v_form`）与水平（`h_form`）两种布局，配合 `field()` 构建带标签、描述、必填标记的字段单元。

它还支持多列网格、列跨度（`col_span`）、列定位（`col_start`）、条件可见性以及动态描述，适用于注册表单、设置面板、联系表单等复杂场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `v_form()` | 构造器 | 创建垂直布局表单 |
| `h_form()` | 构造器 | 创建水平布局表单，标签与控件同行 |
| `field()` | 构造器 | 创建一个表单字段 |
| `.label(text)` | 方法 | 设置字段标签 |
| `.required(bool)` | 方法 | 标记为必填，标签后显示 `*` |
| `.description(text)` | 方法 | 设置字段描述文本 |
| `.description_fn(closure)` | 方法 | 动态描述，闭包返回元素 |
| `.visible(bool)` | 方法 | 条件显示/隐藏字段 |
| `.child(element)` | 方法 | 设置字段内容控件 |
| `.label_indent(bool)` | 方法 | 是否保留标签缩进（按钮行常用 `false`） |
| `.items_start()` | 方法 | 顶部对齐，适合多行内容 |
| `.columns(n)` | 方法 | 设置表单列数 |
| `.col_span(n)` | 方法 | 字段跨 n 列 |
| `.col_start(n)` | 方法 | 指定起始列 |
| `.label_width(px)` | 方法 | 水平表单中标签宽度 |
| `.large()` / `.small()` | 方法 | 表单尺寸 |
| `.gap(px)` | 方法 | 字段间距 |

## 基本用法

### 基础表单

```rust
use gpui_component::form::{field, v_form, h_form, Form, Field};

v_form()
    .child(
        field()
            .label("Name")
            .child(Input::new(&name_input))
    )
    .child(
        field()
            .label("Email")
            .child(Input::new(&email_input))
            .required(true)
    )
```

### 水平布局

```rust
h_form()
    .label_width(px(120.))
    .child(
        field()
            .label("First Name")
            .child(Input::new(&first_name))
    )
    .child(
        field()
            .label("Last Name")
            .child(Input::new(&last_name))
    )
```

### 多列表单

```rust
v_form()
    .columns(2)
    .child(
        field()
            .label("First Name")
            .child(Input::new(&first_name))
    )
    .child(
        field()
            .label("Last Name")
            .child(Input::new(&last_name))
    )
    .child(
        field()
            .label("Bio")
            .col_span(2) // 跨两列
            .child(Input::new(&bio_input))
    )
```

### 带提交按钮的表单

```rust
v_form()
    .child(field().label("Title").child(Input::new(&title)))
    .child(field().label("Content").child(Input::new(&content)))
    .child(
        field()
            .label_indent(false)
            .child(
                h_flex()
                    .gap_2()
                    .child(Button::new("save").primary().child("Save"))
                    .child(Button::new("cancel").child("Cancel"))
            )
    )
```

### 条件字段

```rust
v_form()
    .child(
        field()
            .label("Company Name")
            .visible(is_business_account)
            .child(Input::new(&company_name))
    )
    .child(
        field()
            .label("Tax ID")
            .visible(is_business_account)
            .required(is_business_account)
            .child(Input::new(&tax_id))
    )
```

## 备注

- 提交按钮行通常使用 `.label_indent(false)` 使按钮与控件区对齐。
- 多列布局配合 `col_span` 可实现跨列字段（如备注、分隔线）。
- 校验逻辑需自行实现：读取各字段 `value()` 后判断并提示。
