# Pagination

> 分页导航组件，提供页码、上一页/下一页按钮，支持多种尺寸和紧凑模式。

源文档: https://longbridge.github.io/gpui-component/docs/components/pagination

## 简介

Pagination 组件用于在多页内容之间进行导航。它会显示页码按钮以及上一页/下一页按钮，用户点击即可切换页面。支持设置可见页码数量、紧凑模式（仅显示前后按钮）、多种尺寸和禁用状态。适合用于表格、列表、搜索结果等需要分页展示的场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Pagination` | struct | 分页组件 |
| `Pagination::new(id)` | 构造函数 | 创建分页实例 |
| `.current_page(usize)` | 方法 | 设置当前页码（从 1 开始，会自动夹紧到 1..total） |
| `.total_pages(usize)` | 方法 | 设置总页数 |
| `.visible_pages(usize)` | 方法 | 设置可见页码按钮上限，默认 5 |
| `.compact()` | 方法 | 启用紧凑模式，仅显示前后按钮 |
| `.disabled(bool)` | 方法 | 设置禁用状态 |
| `.on_click(closure)` | 方法 | 页面切换回调，参数为新页码（1-based） |
| `.xsmall()` / `.small()` / `.medium()` / `.large()` | 方法 | 实现 `Sizable` trait，设置尺寸 |
| `.with_size(Size)` | 方法 | 通过 Size 枚举设置尺寸 |

## 基本用法

```rust
use gpui_component::pagination::Pagination;

Pagination::new("my-pagination")
    .current_page(5)
    .total_pages(10)
    .on_click(|page, _, cx| {
        println!("Navigated to page: {}", page);
    })
```

设置可见页码数量：

```rust
Pagination::new("my-pagination")
    .current_page(1)
    .total_pages(50)
    .visible_pages(10)
    .on_click(|page, _, cx| {
        // Handle page change
    })
```

紧凑模式（仅显示前后按钮）：

```rust
Pagination::new("my-pagination")
    .compact()
    .current_page(3)
    .total_pages(10)
    .on_click(|page, _, cx| {
        // Handle page change
    })
```

不同尺寸：

```rust
use gpui_component::{Sizable as _, Size};

Pagination::new("my-pagination").xsmall().current_page(1).total_pages(10);
Pagination::new("my-pagination").small().current_page(1).total_pages(10);
Pagination::new("my-pagination").current_page(1).total_pages(10); // Medium (default)
Pagination::new("my-pagination").large().current_page(1).total_pages(10);
```

结合状态管理：

```rust
Pagination::new("pagination")
    .current_page(current_page)
    .total_pages(total_pages)
    .on_click({
        let entity = entity.clone();
        move |page, _, cx| {
            entity.update(cx, |this, cx| {
                this.current_page = *page;
                cx.notify();
            });
        }
    })
```

## 备注

- 页码为 1-based，`current_page` 会自动夹紧到 `[1, total_pages]` 区间。
- `on_click` 回调在用户点击页码、上一页、下一页按钮时都会触发。
- 默认尺寸为 `medium`。
- 禁用状态下所有按钮不可点击。
