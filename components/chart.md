# Chart

> 数据可视化图表库，提供折线图、柱状图、面积图、饼图、K线图等多种类型。

源文档: https://longbridge.github.io/gpui-component/docs/components/chart

## 简介

Chart 是一套综合性的图表组件库，提供折线图（Line）、柱状图（Bar）、面积图（Area）、饼图（Pie）和 K 线图（Candlestick）等多种图表类型。图表支持流畅的动画过渡、自定义样式、悬浮提示（tooltip）、图例（legend），并具备自动坐标轴缩放能力，适合用于数据分析、财务看板、监控面板等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Line::new(series)` | 组件 | 创建折线图 |
| `Bar::new(series)` | 组件 | 创建柱状图 |
| `Area::new(series)` | 组件 | 创建面积图 |
| `Pie::new(series)` | 组件 | 创建饼图 |
| `Candlestick::new(series)` | 组件 | 创建 K 线图 |
| `ChartSeries` | struct | 图表数据系列 |
| `Point` / `BarPoint` / `PiePoint` | struct | 数据点类型 |
| `color(color)` | 方法 | 设置系列颜色 |
| `name(text)` | 方法 | 设置系列名称（用于图例） |
| `show_axis(bool)` | 方法 | 是否显示坐标轴 |
| `show_grid(bool)` | 方法 | 是否显示网格线 |
| `show_legend(bool)` | 方法 | 是否显示图例 |
| `tooltip(bool)` | 方法 | 是否启用悬浮提示 |
| `on_hover(cb)` | 回调 | 鼠标悬浮数据点回调 |

## 基本用法

折线图：

```rust
use gpui_component::chart::{Line, ChartSeries, Point};

let series = ChartSeries::new("Revenue")
    .point(Point::new(0.0, 100.0))
    .point(Point::new(1.0, 200.0))
    .point(Point::new(2.0, 150.0))
    .point(Point::new(3.0, 300.0))
    .color(rgb(0x1e90ff));

Line::new(series).show_axis(true).show_grid(true)
```

柱状图：

```rust
use gpui_component::chart::{Bar, ChartSeries, BarPoint};

let series = ChartSeries::new("Sales")
    .point(BarPoint::new("Jan", 100.0))
    .point(BarPoint::new("Feb", 200.0))
    .point(BarPoint::new("Mar", 150.0));

Bar::new(series).show_axis(true)
```

多系列与图例：

```rust
let series_a = ChartSeries::new("2024").point(...).color(rgb(0xff6b6b));
let series_b = ChartSeries::new("2025").point(...).color(rgb(0x4ecdc4));

Line::new(vec![series_a, series_b])
    .show_legend(true)
    .tooltip(true)
```

## 备注

- 不同图表类型使用不同的数据点结构：Line/Area 用 `Point`，Bar 用 `BarPoint`，Pie 用 `PiePoint`。
- 图表支持动画过渡，数据更新时会自动平滑过渡。
- K 线图（Candlestick）使用 OHLC 数据格式（开盘、最高、最低、收盘）。
