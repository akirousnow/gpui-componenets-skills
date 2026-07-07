# Progress

> 线性进度条组件，以 0-100 的百分比直观展示任务完成进度。

源文档: https://longbridge.github.io/gpui-component/docs/components/progress

## 简介

Progress 是一个线性进度条组件，通过 0 到 100 的数值直观显示任务完成比例。组件自带平滑动画、主题自适应配色（背景半透明、填充全透明），并且圆角会随完成状态变化，非常适合用于上传、下载、安装等场景的进度反馈。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `new()` | 构造器 | 创建进度条实例 |
| `value(f32)` | f32 | 设置进度值（0-100），超出范围会被自动钳制到边界 |
| 主题色 `progress_bar` | 主题字段 | 填充使用该色全透明，背景使用其 20% 透明度 |

## 基本用法

```rust
use gpui_component::progress::Progress;

Progress::new()
    .value(50.0)
```

不同进度值示例：

```rust
Progress::new().value(0.0)   // 起始
Progress::new().value(25.0)  // 部分
Progress::new().value(75.0)  // 接近完成
Progress::new().value(100.0) // 完成
```

动态更新进度：

```rust
struct MyComponent {
    progress_value: f32,
}

impl MyComponent {
    fn update_progress(&mut self, new_value: f32) {
        self.progress_value = new_value.clamp(0.0, 100.0);
    }

    fn render(&mut self, _: &mut Window, cx: &mut Context<Self>) -> impl IntoElement {
        v_flex()
            .gap_3()
            .child(
                h_flex()
                    .gap_2()
                    .child(Button::new("decrease").label("-").on_click(
                        cx.listener(|this, _, _, _| {
                            this.update_progress(this.progress_value - 10.0);
                        })
                    ))
                    .child(Button::new("increase").label("+").on_click(
                        cx.listener(|this, _, _, _| {
                            this.update_progress(this.progress_value + 10.0);
                        })
                    ))
            )
            .child(Progress::new().value(self.progress_value))
            .child(format!("{}%", self.progress_value as i32))
    }
}
```

文件上传进度：

```rust
struct FileUpload {
    bytes_uploaded: u64,
    total_bytes: u64,
}

impl FileUpload {
    fn progress_percentage(&self) -> f32 {
        if self.total_bytes == 0 {
            0.0
        } else {
            (self.bytes_uploaded as f32 / self.total_bytes as f32) * 100.0
        }
    }

    fn render(&mut self, _: &mut Window, _: &mut Context<Self>) -> impl IntoElement {
        v_flex()
            .gap_2()
            .child("Uploading file...")
            .child(Progress::new().value(self.progress_percentage()))
            .child(format!(
                "{} / {} bytes",
                self.bytes_uploaded, self.total_bytes
            ))
    }
}
```

## 备注

- 取值小于 0 会被钳制为 0%，大于 100 会钳制为 100%；任何负值都显示为 0%。
- 进度条默认从左到右填充，部分进度时仅左侧圆角，完成时左右两侧都圆角。
- 进度条高度默认 8px，圆角跟随主题；背景始终使用填充色的半透明版本。
- 建议在进度条旁搭配文本提示（百分比、剩余时间、状态），并在完成或出错时给出明确反馈。
