# DataTable

> 用于展示与交互的可排序、可筛选、可分页表格组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/data-table

## 简介

DataTable 是一个高性能表格组件，支持列排序、滚动加载（懒加载）、自定义单元格渲染、选中行、分页等特性。基于虚拟列表实现，能高效渲染大量数据。通过 `TableDelegate` trait 自定义数据源和单元格行为，可灵活适配各种业务场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `TableDelegate` trait | trait | 定义表格数据源，需实现 `col_count`、`row_count`、`col_name` 等 |
| `DataTable::new(&delegate)` | 构造方法 | 用 delegate 创建表格 |
| `col_count(&self)` | usize | 列数 |
| `row_count(&self)` | usize | 行数 |
| `col_name(&self, i)` | &str | 第 i 列名称 |
| `render_td(&self, row, col, cx)` | AnyElement | 渲染单元格内容 |
| `render_th(&self, col, cx)` | AnyElement | 渲染表头单元格 |
| `can_sort_by(&self, col)` | bool | 该列是否支持排序 |
| `set_sort(&self, col, asc)` | 方法 | 设置当前排序列与方向 |
| `set_selected_row(&self, row)` | 方法 | 设置选中行 |
| `max_h_*()` / `min_w_*()` | 样式 | 控制表格尺寸 |

## 基本用法

```rust
use gpui_component::table::{DataTable, TableDelegate};
```

定义数据委托（delegate）：

```rust
struct MyTable {
    rows: Vec<Vec<String>>,
}

impl TableDelegate for MyTable {
    fn cols_count(&self) -> usize {
        3
    }
    fn rows_count(&self) -> usize {
        self.rows.len()
    }
    fn col_name(&self, i: usize) -> SharedString {
        ["Name", "Age", "City"][i].into()
    }
    fn render_td(&self, row: usize, col: usize, _cx: &mut Context<Self>) -> AnyElement {
        div().child(self.rows[row][col].clone()).into_any_element()
    }
}
```

创建表格组件：

```rust
let table = cx.new(|cx| MyTable { rows });

DataTable::new(&table)
    .max_h_120()
```

## 备注

实现 `TableDelegate` 时务必返回正确的 `cols_count` 与 `rows_count`；渲染逻辑放在 `render_td` 中，可返回任意 `AnyElement` 实现自定义样式。
