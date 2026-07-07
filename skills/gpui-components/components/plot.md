# Plot

> 底层绘图库，提供 Scale、Shape 等基础构件，用于构建自定义图表与数据可视化。

源文档: https://longbridge.github.io/gpui-component/docs/components/plot

## 简介

`plot` 模块提供低层构建块，包括比例尺（Scale）、图形（Shape）和坐标轴（PlotAxis），是高层 `Chart` 组件的基础。适用于需要自定义图表实现的场景。

比例尺负责将数据维度映射到视觉表示；图形负责实际绘制柱、线、面积、饼图等。

## 核心 API

### Scale（比例尺）

| 名称 | 类型 | 说明 |
|------|------|------|
| `ScaleLinear::new(domain, range)` | 构造器 | 连续定量域到连续范围的线性映射 |
| `ScaleBand::new(domain, range)` | 构造器 | 离散域到连续范围，用于柱状图 |
| `ScalePoint::new(domain, range)` | 构造器 | 离散域到连续范围中的点 |
| `ScaleOrdinal::new(domain, range)` | 构造器 | 离散域到离散范围（如颜色） |
| `.tick(&value)` | 方法 | 返回值对应的像素位置 |
| `.band_width()` | 方法 | 返回每个带宽的宽度（ScaleBand） |
| `.map(&value)` | 方法 | 返回映射后的值（ScaleOrdinal） |

### Shape（图形）

| 名称 | 类型 | 说明 |
|------|------|------|
| `Bar::new()` | 构造器 | 柱状图柱体 |
| `Line::new()` | 构造器 | 折线图线条，支持 `.dot()` 显示数据点 |
| `Area::new()` | 构造器 | 面积图区域 |
| `Pie::new()` | 构造器 | 饼图布局计算 |
| `Arc::new()` | 构造器 | 圆弧形状，配合 Pie 渲染 |
| `Stack::new()` | 构造器 | 堆叠布局计算 |
| `.data(data)` | 方法 | 绑定数据 |
| `.paint(&bounds, window, cx)` | 方法 | 执行绘制 |

### 组件

| 名称 | 类型 | 说明 |
|------|------|------|
| `PlotAxis::new()` | 构造器 | 渲染坐标轴及刻度标签 |

## 基本用法

### ScaleLinear

```rust
use gpui_component::plot::scale::{ScaleLinear, ScaleBand};

let scale = ScaleLinear::new(
    vec![0., 100.],   // Domain (数据值)
    vec![0., 500.]    // Range (像素坐标)
);

scale.tick(&50.); // 返回像素位置
```

### ScaleBand

```rust
let scale = ScaleBand::new(
    vec!["A", "B", "C"],
    vec![0., 300.]
)
.padding_inner(0.1)
.padding_outer(0.1);

scale.band_width(); // 每个带宽的宽度
scale.tick(&"A");   // 带A的起始位置
```

### Bar（柱状图）

```rust
use gpui_component::plot::shape::Bar;

Bar::new()
    .data(data)
    .band_width(30.)
    .x(|d| x_scale.tick(&d.category))
    .y0(|d| y_scale.tick(&0.).unwrap())
    .y1(|d| y_scale.tick(&d.value))
    .fill(|d| color_scale.map(&d.category))
    .paint(&bounds, window, cx);
```

### Line（折线图）

```rust
use gpui_component::plot::shape::Line;

Line::new()
    .data(data)
    .x(|d| x_scale.tick(&d.date))
    .y(|d| y_scale.tick(&d.value))
    .stroke(cx.theme().chart_1)
    .stroke_width(px(2.))
    .paint(&bounds, window);
```

### 带数据点的线图

```rust
Line::new()
    .data(data)
    .x(|d| x_scale.tick(&d.date))
    .y(|d| y_scale.tick(&d.value))
    .dot()
    .dot_size(px(4.))
    .paint(&bounds, window);
```

### Pie 与 Arc（饼图）

```rust
use gpui_component::plot::shape::{Pie, Arc};

// 1. 计算饼图布局
let pie = Pie::new()
    .value(|d| Some(d.value))
    .pad_angle(0.05);

let arcs = pie.arcs(&data);

// 2. 渲染弧形
let arc_shape = Arc::new()
    .inner_radius(0.)
    .outer_radius(100.);

for arc_data in arcs {
    arc_shape.paint(
        &arc_data,
        color_scale.map(&arc_data.data.category),
        None,
        None,
        &bounds,
        window
    );
}
```

### PlotAxis（坐标轴）

```rust
use gpui_component::plot::PlotAxis;

PlotAxis::new()
    .x(height)
    .x_label(labels)
    .stroke(cx.theme().border)
    .paint(&bounds, window, cx);
```

## 备注

- Plot 是底层库，高层图表可直接使用 `Chart` 组件。
- 自定义图表需实现 `Plot` trait 的 `paint` 方法，在其中组合 Scale 与 Shape。
- Pie 图的 `inner_radius` 大于 0 时即为环形图。
