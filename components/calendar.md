# Calendar

> 灵活的日历组件，支持月份浏览、日期导航及单日期/日期范围选择。

源文档: https://longbridge.github.io/gpui-component/docs/components/calendar

## 简介

Calendar 是一个独立的日历组件，用于展示月份视图、浏览日期，并支持选择单个日期或日期范围。它内部基于 Popover 组件，也可以作为 DatePicker（日期选择器）的基础。组件支持上下月导航、自定义日期格式、禁用日期、高亮"今天"等特性，适合用于日程安排、预订系统、报表筛选等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Calendar::new(state)` | 构造函数 | 创建日历，需传入状态 |
| `CalendarState` | struct | 日历状态，管理选中日期与视图月份 |
| `CalendarState::new(window, cx)` | 构造函数 | 创建日历状态 |
| `mode(mode)` | 方法 | 设置选择模式：单选或范围选择 |
| `value(date)` / `value_fn(cb)` | 方法 | 设置选中日期（受控） |
| `on_change(cb)` | 回调 | 日期变化回调 |
| `date_format(fmt)` | 方法 | 自定义日期格式 |
| `disabled(dates)` | 方法 | 禁用某些日期 |
| `min(date)` / `max(date)` | 方法 | 设置可选范围边界 |
| `mode` | 属性 | `single`（单选）或 `range`（范围） |

## 基本用法

基础单选日历：

```rust
use gpui_component::calendar::{Calendar, CalendarState};

let calendar_state = cx.new(|cx| CalendarState::new(window, cx));

Calendar::new(calendar_state.clone())
    .on_change(|date, window, cx| {
        println!("Selected date: {:?}", date);
    })
```

日期范围选择：

```rust
let calendar_state = cx.new(|cx| CalendarState::new(window, cx));

Calendar::new(calendar_state.clone())
    .mode(CalendarMode::Range)
    .on_change(|range, window, cx| {
        println!("Selected range: {:?}", range);
    })
```

配合 DatePicker（作为 Popover 触发器）：

```rust
use gpui_component::date_picker::DatePicker;

DatePicker::new(date_state)
    .placeholder("Select a date")
```

## 备注

- CalendarState 是核心状态管理对象，需通过 `cx.new` 创建并共享。
- 选择模式 `single` 返回单个日期，`range` 返回日期范围（起止日期）。
- 可通过 `min` / `max` 或 `disabled` 限制用户可选的日期范围。
