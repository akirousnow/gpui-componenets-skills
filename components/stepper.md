# Stepper

> 分步进度导航组件，引导用户按步骤完成多阶段流程。

源文档: https://longbridge.github.io/gpui-component/docs/components/stepper

## 简介

Stepper 是一个分步进度组件，通过一系列步骤或阶段引导用户完成任务。它非常适合多步骤表单、安装向导、结账流程等需要清晰展示进度与当前位置的场景。支持横向与纵向布局、可点击导航、已完成步骤标记以及自定义步骤渲染。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Stepper` | 结构体 | 分步组件本体 |
| `Step` | 结构体 | 单个步骤项 |
| `current(index)` | 方法 | 设置当前步骤索引 |
| `direction(axis)` | 方法 | 布局方向：`Horizontal` / `Vertical` |
| `label(title, desc)` | 方法 | 设置步骤标题与描述 |
| `icon(name)` | 方法 | 设置步骤图标 |
| `on_change(fn)` | 方法 | 步骤切换回调 |
| `StepStatus` | 枚举 | 步骤状态：`Finish` / `Process` / `Wait` |

## 基本用法

```rust
use gpui_component::stepper::{Stepper, Step};

Stepper::new()
    .current(1)
    .children(vec![
        Step::new().label("Login", "Sign in to your account").icon(IconName::User),
        Step::new().label("Verification", "Verify your information").icon(IconName::Shield),
        Step::new().label("Pay", "Complete payment").icon(IconName::CreditCard),
        Step::new().label("Done", "Finish the process").icon(IconName::CheckCircle),
    ])
```

纵向布局：

```rust
Stepper::new()
    .direction(gpui::Axis::Vertical)
    .current(2)
    .children(vec![
        Step::new().label("Step 1", "First step description"),
        Step::new().label("Step 2", "Second step description"),
        Step::new().label("Step 3", "Third step description"),
    ])
```

配合按钮控制步骤切换：

```rust
struct StepperExample {
    current_step: usize,
}

impl Render for StepperExample {
    fn render(&mut self, _window: &mut Window, _cx: &mut Context<Self>) -> impl IntoElement {
        v_flex().gap_4().p_4().child(
            Stepper::new()
                .current(self.current_step)
                .children(vec![
                    Step::new().label("Step 1", "Description 1"),
                    Step::new().label("Step 2", "Description 2"),
                    Step::new().label("Step 3", "Description 3"),
                ]),
        )
    }
}
```

## 备注

- 步骤状态会根据 `current` 索引自动计算：已完成（Finish）、进行中（Process）、等待中（Wait）。
- 支持横向和纵向两种布局，默认为横向。
- 步骤项可自定义图标，未设置时默认显示步骤序号。
