# DescriptionList

> 用于以整洁的键值对布局展示详细信息的组件。

源文档: https://longbridge.github.io/gpui-component/docs/components/description-list

## 简介

DescriptionList 用于以结构化布局展示键值对，支持水平/垂直布局、多列、分隔线和不同尺寸，适合展示元数据、规格说明或摘要信息。通过 `DescriptionItem` 构建每一项，可控制跨列数（span）和自定义内容元素。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `DescriptionList::new()` | 构造方法 | 创建描述列表（默认水平布局） |
| `DescriptionList::horizontal()` | 构造方法 | 创建水平布局列表 |
| `DescriptionList::vertical()` | 构造方法 | 创建垂直布局列表 |
| `columns(n)` | 方法 | 设置列数 |
| `child(key, value, span)` | 方法 | 添加一项 |
| `children([DescriptionItem])` | 方法 | 批量添加项 |
| `divider()` / `DescriptionItem::Divider` | 方法 | 添加分隔线 |
| `bordered(bool)` | 方法 | 是否显示边框 |
| `label_width(px)` | 方法 | 设置标签宽度（水平布局） |
| `large()` / `small()` | 样式 | 设置尺寸 |
| `DescriptionItem::new(label).value(v).span(n)` | 构建器 | 构建单个描述项 |

## 基本用法

```rust
use gpui_component::description_list::{DescriptionList, DescriptionItem, DescriptionText};
```

基础用法：

```rust
DescriptionList::new()
    .child("Name", "GPUI Component", 1)
    .child("Version", "0.1.0", 1)
    .child("License", "Apache-2.0", 1)
```

使用 DescriptionItem 构建器与多列跨列：

```rust
DescriptionList::new()
    .columns(3)
    .child(DescriptionItem::new("Name").value("GPUI Component").span(1))
    .children([
        DescriptionItem::new("Version").value("0.1.0").span(1),
        DescriptionItem::new("License").value("Apache-2.0").span(1),
        DescriptionItem::new("Description")
            .value("Full-featured UI components for desktop applications")
            .span(3),
    ])
```

带分隔线与无边框：

```rust
DescriptionList::new()
    .bordered(false)
    .item("Name", "GPUI Component", 1)
    .divider()
    .item("Author", "Longbridge", 1)
```

## 备注

建议列数控制在 3-4 列以获得最佳可读性；值字段可传入任意 `AnyElement`（如 TextView）以实现富文本内容。
