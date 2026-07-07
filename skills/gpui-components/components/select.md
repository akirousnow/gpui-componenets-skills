# Select

> 下拉选择组件，允许用户从一组选项中选择一个或多个值。

源文档: https://longbridge.github.io/gpui-component/docs/components/select

## 简介

Select 是一个下拉选择组件（在 `<= 0.3.x` 版本中名为 `Dropdown`，后更名为 `Select`）。它支持单选、多选、自定义选项渲染、分组、搜索过滤、异步加载以及禁用选项等功能。典型场景包括表单字段选择、过滤器、设置项选择等。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Select` | 结构体 | 下拉选择组件本体 |
| `SelectState` | 结构体 | 选择状态管理，记录当前选中值 |
| `SelectEvent` | 枚举 | 选择事件，如 `Select` / `Confirm` |
| `ListItem` | 结构体 | 选项条目，含 `value`、`label`、`disabled` 等 |
| `placeholder()` | 方法 | 设置占位提示文本 |
| `set_value()` | 方法 | 编程式设置当前选中值 |
| `on_change()` | 方法 | 监听选中变化回调 |
| `on_multi_change()` | 方法 | 多选模式下的变化回调 |

## 基本用法

定义选项并创建 `SelectState`：

```rust
use gpui_component::select::Select;
use gpui_component::list::ListItem;

pub struct SelectExample {
    language_select_state: SelectState,
}

impl SelectExample {
    pub fn new(_window: &mut Window, cx: &mut Context<Self>) -> Self {
        let language_select_state = SelectState::new(
            Some(Language::English.to_string()),
            {
                let cx = cx.to_async(cx.background_executor().clone());
                move |_query, _| {
                    let cx = cx.clone();
                    async move {
                        Language::all()
                            .iter()
                            .map(|l| {
                                ListItem::new(l.to_id())
                                    .label(l.to_string().into())
                                    .inset(true)
                                    .disabled(matches!(l, Language::French))
                            })
                            .collect::<Vec<_>>()
                    }
                }
            },
            {
                let cx = cx.to_async(cx.background_executor().clone());
                move |value: SharedString, _| {
                    let cx = cx.clone();
                    async move {
                        let _ = cx.update(|cx| { /* 处理选中值 */ });
                    }
                }
            },
        );
        Self { language_select_state }
    }
}
```

渲染 Select 组件：

```rust
impl Render for SelectExample {
    fn render(&mut self, _window: &mut Window, _cx: &mut Context<Self>) -> impl IntoElement {
        v_flex().gap_4().p_4().w(px(320.)).child(
            Select::new(self.language_select_state.clone())
                .placeholder(SharedString::from("Select a language")),
        )
    }
}
```

## 备注

- 该组件在 `<= 0.3.x` 版本中名为 `Dropdown`，新版本统一为 `Select`。
- 选项内容通过异步闭包返回 `ListItem` 列表，天然支持搜索过滤与远程数据加载。
- 可通过 `ListItem::disabled` 禁用特定选项。
