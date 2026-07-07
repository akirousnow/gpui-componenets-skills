# Sheet

> 从屏幕边缘滑入的面板组件，用于展示导航、表单或辅助内容。

源文档: https://longbridge.github.io/gpui-component/docs/components/sheet

## 简介

Sheet（又称侧滑面板）是一个从屏幕边缘滑入的导航组件，能在不占用主视图空间的前提下为内容提供额外展示区域。它常用于导航菜单、表单、设置面板或任何辅助内容。支持四个方向的滑入、可调节大小、遮罩层和自定义样式。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `open_sheet(cx, fn)` | 方法 | 以默认方向（右）打开 Sheet |
| `open_sheet_at(placement, cx, fn)` | 方法 | 以指定方向打开 Sheet |
| `close_sheet(cx)` | 方法 | 关闭当前 Sheet |
| `Placement` | 枚举 | 滑入方向：`Left` / `Right` / `Top` / `Bottom` |
| `title(str)` | 方法 | 设置标题 |
| `child(el)` | 方法 | 添加面板内容 |
| `footer(el)` | 方法 | 设置底部内容 |
| `size(px)` | 方法 | 设置面板尺寸（左右为宽，上下为高） |
| `resizable(bool)` | 方法 | 是否可拖拽调整大小（默认 true） |
| `overlay(bool)` | 方法 | 是否显示遮罩（默认 true） |
| `Root::render_sheet_layer` | 函数 | 在根视图中渲染 Sheet 图层 |

## 基本用法

首先在应用根视图设置 Sheet 图层渲染：

```rust
use gpui_component::TitleBar;

impl Render for MyApp {
    fn render(&mut self, window: &mut Window, cx: &mut Context<Self>) -> impl IntoElement {
        let sheet_layer = Root::render_sheet_layer(window, cx);
        div()
            .size_full()
            .child(v_flex().size_full().child(TitleBar::new()))
            .children(sheet_layer)
    }
}
```

打开基本 Sheet 与指定方向：

```rust
// 基本用法（默认右侧滑入）
window.open_sheet(cx, |sheet, _, _| {
    sheet.title("Navigation").child("Sheet content goes here")
});

// 指定方向与尺寸
window.open_sheet_at(Placement::Left, cx, |sheet, _, _| {
    sheet.title("Left Sheet").size(px(300.)).child("300px wide")
});
```

带表单与底部的 Sheet：

```rust
let input = cx.new(|cx| InputState::new(window, cx));

window.open_sheet(cx, |sheet, _, _| {
    sheet
        .title("User Profile")
        .child(v_flex().gap_4().child(Input::new(&input).placeholder("Full Name")))
        .footer(
            h_flex().gap_3()
                .child(Button::new("save").primary().label("Save"))
                .child(Button::new("cancel").label("Cancel")),
        )
})
```

## 备注

- 使用前需在应用根视图调用 `Root::render_sheet_layer(window, cx)` 渲染图层。
- 导入需引入 `gpui_component::WindowExt` 与 `gpui_component::Placement`。
- 默认方向为 `Placement::Right`，默认开启遮罩与可拖拽调整大小。
