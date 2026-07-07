# Switch

> 开关组件，用于在选中与未选中两种状态之间切换。

源文档: https://longbridge.github.io/gpui-component/docs/components/switch

## 简介

Switch 是一个切换开关组件，用于表达二元（开/关）状态。它带有平滑的动画过渡效果，常见于设置面板、偏好选项等场景，相比 Checkbox 更适合表达"启用/禁用"这类即时生效的开关型选项。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Switch` | struct | 开关组件主体 |
| `Switch::new` | 构造函数 | 创建一个 Switch，可传入初始选中状态 |
| `checked` | 方法 | 设置是否选中 |
| `disabled` | 方法 | 设置是否禁用 |
| `on_change` | 方法/回调 | 选中状态变化时触发的回调 |
| `set_checked` | 方法 | 编程式修改选中状态 |

## 基本用法

```rust
use gpui_component::switch::Switch;

let switch = Switch::new()
    .checked(true)
    .on_change(|checked, _, _| {
        println!("Switch is now: {}", checked);
    });
```

## 备注

无特别说明。
