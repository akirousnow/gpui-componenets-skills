# DatePicker

> 灵活的日期选择器，支持单日期、日期范围、自定义格式与禁用日期。

源文档: https://longbridge.github.io/gpui-component/docs/components/date-picker

## 简介

DatePicker 提供日历式日期选择界面，支持单日期选择与日期范围选择，可自定义日期格式、禁用日期、预设范围等。状态通过 `DatePickerState` 管理，通过订阅 `DatePickerEvent::Change` 事件监听选择变化。基于 chrono 库处理日期。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `DatePickerState::new()` | 构造方法 | 创建单日期选择状态 |
| `DatePickerState::range()` | 构造方法 | 创建日期范围选择状态 |
| `DatePicker::new(&state)` | 构造方法 | 创建日期选择器组件 |
| `set_date(value, window, cx)` | 方法 | 设置日期，参数为 `Date` 或元组 |
| `date_format(&str)` | 方法 | 设置日期显示格式 |
| `disabled_matcher(Matcher)` | 方法 | 设置禁用日期规则 |
| `number_of_months(n)` | 方法 | 设置显示的月份数 |
| `presets(Vec<DateRangePreset>)` | 方法 | 设置预设日期/范围 |
| `placeholder(&str)` | 方法 | 设置占位文本 |
| `cleanable(bool)` | 方法 | 是否显示清除按钮 |
| `large()` / `small()` | 样式 | 设置尺寸 |
| `DatePickerEvent::Change` | 事件 | 日期变化事件，参数为 `Date` |

## 基本用法

```rust
use gpui_component::{
    date_picker::{DatePicker, DatePickerState, DateRangePreset, DatePickerEvent},
    calendar::{Date, Matcher},
};
```

基础日期选择：

```rust
let date_picker = cx.new(|cx| DatePickerState::new(window, cx));

DatePicker::new(&date_picker)
```

日期范围选择：

```rust
use chrono::{Local, Days};

let range_picker = cx.new(|cx| DatePickerState::range(window, cx));

DatePicker::new(&range_picker)
    .number_of_months(2)
```

监听选择事件：

```rust
cx.subscribe(&date_picker, |view, _, event, _| {
    match event {
        DatePickerEvent::Change(date) => match date {
            Date::Single(Some(selected_date)) => {
                println!("Single date selected: {}", selected_date);
            }
            Date::Range(Some(start), Some(end)) => {
                println!("Date range selected: {} to {}", start, end);
            }
            _ => {
                println!("Date cleared");
            }
        }
    }
});
```

禁用周末与自定义格式：

```rust
let date_picker = cx.new(|cx| {
    DatePickerState::new(window, cx)
        .date_format("%Y-%m-%d")
        .disabled_matcher(calendar::Matcher::custom(|date| {
            matches!(date.weekday(), Weekday::Sat | Weekday::Sun)
        }))
});

DatePicker::new(&date_picker)
    .placeholder("Select business day")
```

## 备注

日期格式遵循 chrono 的 `strftime` 规则，例如 `%Y-%m-%d`、`%B %d, %Y`。日期类型 `Date` 分为 `Single` 与 `Range` 两种变体，需在事件处理中分别匹配。
