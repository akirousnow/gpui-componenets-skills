# Tree

> 树形组件，用于展示层级数据，支持展开/折叠、键盘导航与自定义节点渲染。

源文档: https://longbridge.github.io/gpui-component/docs/components/tree

## 简介

Tree 是一个多功能树形组件，用于展示具有层级关系的数据。它支持展开/折叠节点、键盘导航和自定义节点渲染，非常适合文件浏览器、导航菜单或任何嵌套层级数据的展示场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Tree` | struct | 树组件主体 |
| `TreeDelegate` | trait | 树数据委托，需实现节点相关方法 |
| `TreeDelegate` 方法 `children_count` | 返回某节点的子节点数 |
| `TreeDelegate` 方法 `render_item` | 渲染单个节点 |
| `TreeDelegate` 方法 `is_expanded` | 判断节点是否展开 |
| `TreeDelegate` 方法 `set_expanded` | 设置节点展开/折叠 |

## 基本用法

```rust
use gpui_component::tree::{Tree, TreeDelegate};

struct MyNode {
    id: String,
    name: String,
    children: Vec<MyNode>,
}

struct MyTreeDelegate {
    root: MyNode,
    expanded: std::collections::HashSet<String>,
}

impl TreeDelegate for MyTreeDelegate {
    // 实现节点数量、渲染、展开/折叠等逻辑
    fn children_count(&self, ...) -> usize { ... }
    fn render_item(&self, ...) -> impl IntoElement { ... }
    fn is_expanded(&self, ...) -> bool { ... }
}

let tree = Tree::new(MyTreeDelegate { ... });
```

## 备注

无特别说明。
