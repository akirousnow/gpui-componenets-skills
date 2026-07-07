# Table

> 高性能数据表格组件，支持虚拟滚动、排序、筛选与列管理，适合展示大规模数据。

源文档: https://longbridge.github.io/gpui-component/docs/components/table

## 简介

Table 是一个功能全面的数据表格组件，专为处理大规模数据集而设计。它内置虚拟滚动机制，即使渲染上万行数据也能保持流畅；同时支持列配置、排序、行选择、自定义单元格渲染等能力，适用于财务报表、用户管理等各类表格场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Table` | struct | 表格视图组件，用于渲染表格 |
| `TableState` | struct | 表格状态管理，通过 `cx.new` 创建 |
| `TableDelegate` | trait | 表格数据委托，需实现列/行/单元格相关方法 |
| `Column` | struct | 列定义，支持宽度、排序、对齐、固定等配置 |
| `ColumnSort` | enum | 列排序方式：`Ascending` / `Descending` / `Default` |
| `ColumnFixed` | enum | 列固定位置：`Left` / `Right` |
| `TableEvent` | enum | 表格事件，如 `SelectRow`、`DoubleClickedRow`、`ColumnWidthsChanged` 等 |

TableDelegate 关键方法：

| 方法 | 说明 |
|------|------|
| `columns_count` | 返回列数 |
| `rows_count` | 返回行数 |
| `column` | 返回指定列的 `Column` 引用 |
| `render_td` | 渲染单元格内容 |
| `render_tr` | 渲染行容器（可绑定点击事件） |
| `perform_sort` | 实现列排序逻辑 |
| `context_menu` | 右键上下文菜单 |
| `is_eof` / `load_more` | 无限加载/分页支持 |

TableState 配置方法：`col_resizable`、`col_movable`、`sortable`、`row_selectable`、`col_selectable`。

## 基本用法

```rust
use std::ops::Range;
use gpui::{App, Context, Window, IntoElement};
use gpui_component::table::{Table, TableDelegate, Column, ColumnSort};

struct MyData {
    id: usize,
    name: String,
    age: u32,
    email: String,
}

struct MyTableDelegate {
    data: Vec<MyData>,
    columns: Vec<Column>,
}

impl MyTableDelegate {
    fn new() -> Self {
        Self {
            data: vec![
                MyData { id: 1, name: "John".to_string(), age: 30, email: "john@example.com".to_string() },
                MyData { id: 2, name: "Jane".to_string(), age: 25, email: "jane@example.com".to_string() },
            ],
            columns: vec![
                Column::new("id", "ID").width(60.),
                Column::new("name", "Name").width(150.).sortable(),
                Column::new("age", "Age").width(80.).sortable(),
                Column::new("email", "Email").width(200.),
            ],
        }
    }
}

impl TableDelegate for MyTableDelegate {
    fn columns_count(&self, _: &App) -> usize {
        self.columns.len()
    }

    fn rows_count(&self, _: &App) -> usize {
        self.data.len()
    }

    fn column(&self, col_ix: usize, _: &App) -> &Column {
        &self.columns[col_ix]
    }

    fn render_td(&self, row_ix: usize, col_ix: usize, _: &mut Window, _: &mut App) -> impl IntoElement {
        let row = &self.data[row_ix];
        let col = &self.columns[col_ix];

        match col.key.as_ref() {
            "id" => row.id.to_string(),
            "name" => row.name.clone(),
            "age" => row.age.to_string(),
            "email" => row.email.clone(),
            _ => "".to_string(),
        }
    }
}

// Create the table
let delegate = MyTableDelegate::new();
let state = cx.new(|cx| TableState::new(delegate, window, cx));
```

渲染表格并配置样式：

```rust
Table::new(&state)
    .stripe(true)           // Alternating row colors
    .bordered(true)           // Border around table
    .scrollbar_visible(true, true) // Vertical, horizontal scrollbars
```

## 备注

键盘快捷键：`↑/↓` 切换行，`←/→` 切换列，`Enter/Space` 选择，`Escape` 清除选择。
