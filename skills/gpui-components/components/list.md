# List

> 虚拟化、可搜索的列表组件，支持分组、选择和无限滚动（基于 delegate 模式）。

源文档: https://longbridge.github.io/gpui-component/docs/components/list

## 简介

List 是一个强大的列表组件，提供虚拟化渲染、内置搜索、分组（sections）、表头/表脚、选择状态和无限滚动等能力。它基于 delegate 模式构建，允许灵活的数据管理和自定义项渲染，适合文件浏览器、联系人列表、命令面板等数据密集场景。

## 核心 API

### 核心 struct / trait

| 名称 | 类型 | 说明 |
|------|------|------|
| `List` | 组件 | 列表渲染组件 |
| `ListState` | 状态 | 列表状态，持有一个 delegate |
| `ListDelegate` | trait | 数据 delegate，需用户实现 |
| `ListItem` | 项 | 标准列表项 |
| `ListSeparatorItem` | 项 | 分隔项 |
| `ListEvent` | 事件 | 选择/确认/取消事件 |
| `IndexPath` | 索引 | 定位 `section` + `row` |

### ListDelegate 关键方法（需实现）

| 方法 | 说明 |
|------|------|
| `items_count(section, cx)` | 指定分组下项数 |
| `render_item(ix, window, cx)` | 渲染单个项 |
| `set_selected_index(ix, window, cx)` | 设置选中项 |

### ListDelegate 可选方法

| 方法 | 说明 |
|------|------|
| `sections_count(cx)` | 分组数量 |
| `render_section_header/footer` | 分组头/脚 |
| `perform_search(query, window, cx)` | 搜索处理 |
| `loading(cx)` / `render_loading` | 加载状态 |
| `has_more(cx)` / `load_more` | 无限滚动 |
| `render_empty` | 空状态 |

### List / ListState 配置方法

| 方法 | 说明 |
|------|------|
| `ListState::new(delegate, window, cx)` | 创建状态 |
| `.searchable(true)` | 显示搜索框 |
| `List::new(&state).max_h(h)` | 最大高度 |
| `.scrollbar_visible(false)` | 隐藏滚动条 |
| `.paddings(edges)` | 内边距 |
| `state.scroll_to_item(ix, strategy, window, cx)` | 滚动到项 |

## 基本用法

```rust
use gpui_component::list::{List, ListState, ListDelegate, ListItem, ListEvent};
use gpui_component::IndexPath;

struct MyDelegate { items: Vec<String>, selected_index: Option<IndexPath> }

impl ListDelegate for MyDelegate {
    type Item = ListItem;

    fn items_count(&self, _section: usize, _cx: &App) -> usize { self.items.len() }

    fn render_item(&mut self, ix: IndexPath, _window: &mut Window, _cx: &mut Context<TableState<Self>>) -> Option<Self::Item> {
        self.items.get(ix.row).map(|item| {
            ListItem::new(ix)
                .child(Label::new(item.clone()))
                .selected(Some(ix) == self.selected_index)
        })
    }

    fn set_selected_index(&mut self, ix: Option<IndexPath>, _window: &mut Window, cx: &mut Context<ListState<Self>>) {
        self.selected_index = ix;
        cx.notify();
    }
}

let delegate = MyDelegate { items: vec!["Item 1".into(), "Item 2".into()], selected_index: None };
let state = cx.new(|cx| ListState::new(delegate, window, cx));

div().child(List::new(&state))
```

启用搜索：

```rust
let state = cx.new(|cx| ListState::new(delegate, window, cx).searchable(true));
List::new(&state)
```

监听事件：

```rust
cx.subscribe(&state, |_, _, event: &ListEvent, _| match event {
    ListEvent::Select(ix) => println!("Selected: {:?}", ix),
    ListEvent::Confirm(ix) => println!("Confirmed: {:?}", ix),
    ListEvent::Cancel => println!("Cancelled"),
});
```

## 备注

- `items_count` 为 0 的分组会被自动隐藏（不渲染表头/表脚）
- 通过 `has_more` + `load_more` + `load_more_threshold` 实现无限滚动
- `ListItem` 支持 `selected`、`confirmed`、`disabled`、`suffix`、`check_icon` 等
- 使用 `ListSeparatorItem` 渲染分隔线
