# Editor

> 功能强大的多行文本输入组件，支持自动调整高度、语法高亮、行号与代码编辑。

源文档: https://longbridge.github.io/gpui-component/docs/components/editor

## 简介

Editor（基于 `InputState`）扩展了基础输入框的能力，支持多行文本、自动增高、语法高亮、行号显示与代码编辑，适用于表单、代码编辑器和内容编辑场景。

代码编辑模式基于 tree-sitter 进行语法高亮，使用 ropey 进行文本存储与操作，专为高性能和大文件处理设计。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `InputState::new(window, cx)` | 构造器 | 创建输入状态实例 |
| `.multi_line(bool)` | 方法 | 是否启用多行模式（Textarea） |
| `.rows(n)` | 方法 | 设置固定行数 |
| `.auto_grow(min, max)` | 方法 | 自动增高，指定最小/最大行数 |
| `.code_editor(lang)` | 方法 | 启用代码编辑模式并指定语言 |
| `.line_number(bool)` | 方法 | 是否显示行号 |
| `.searchable(bool)` | 方法 | 是否启用 Ctrl+F 搜索 |
| `.show_whitespaces(bool)` | 方法 | 是否显示空白字符 |
| `.soft_wrap(bool)` | 方法 | 是否启用软换行 |
| `.tab_size(TabSize)` | 方法 | 设置制表符大小与是否使用空格 |
| `.validate(closure)` | 方法 | 设置校验闭包，返回布尔值 |
| `.placeholder(text)` | 方法 | 设置占位文本 |
| `.default_value(text)` | 方法 | 设置默认内容 |
| `.insert(text, window, cx)` | 方法 | 在光标处插入文本 |
| `.replace(text, window, cx)` | 方法 | 替换全部内容 |
| `.set_cursor_position(pos, window, cx)` | 方法 | 设置光标位置 |
| `.cursor_position()` | 方法 | 获取光标位置 |
| `.value()` | 方法 | 获取当前内容 |

## 基本用法

### 多行文本框

```rust
use gpui_component::input::{InputState, Input};

let state = cx.new(|cx|
    InputState::new(window, cx)
        .multi_line(true)
        .placeholder("Enter your message...")
);

Input::new(&state)
```

### 自动增高

```rust
let state = cx.new(|cx|
    InputState::new(window, cx)
        .auto_grow(1, 5) // min_rows: 1, max_rows: 5
        .placeholder("Type here and watch it grow...")
);

Input::new(&state)
```

### 代码编辑器

```rust
let state = cx.new(|cx|
    InputState::new(window, cx)
        .code_editor("rust") // 语法高亮语言
        .line_number(true)   // 显示行号
        .searchable(true)    // 启用搜索
        .show_whitespaces(true)
        .default_value("fn main() {\n    println!(\"Hello, world!\");\n}")
);

Input::new(&state)
    .h_full()
```

### 文本操作

```rust
// 在光标处插入
state.update(cx, |state, cx| {
    state.insert("inserted text", window, cx);
});

// 替换全部内容
state.update(cx, |state, cx| {
    state.replace("new content", window, cx);
});

// 设置光标位置
state.update(cx, |state, cx| {
    state.set_cursor_position(Position { line: 2, character: 5 }, window, cx);
});
```

### 事件处理

```rust
cx.subscribe_in(&state, window, |view, state, event, window, cx| {
    match event {
        InputEvent::Change => {
            let content = state.read(cx).value();
            println!("Content changed: {} characters", content.len());
        }
        InputEvent::PressEnter { secondary } => {
            if secondary {
                println!("Shift+Enter pressed");
            } else {
                println!("Enter pressed");
            }
        }
        InputEvent::Focus => println!("focused"),
        InputEvent::Blur => println!("blurred"),
    }
});
```

## 备注

- 多行模式默认开启软换行，关闭后可水平滚动。
- 通过 `Ctrl+F`（Mac 为 `Cmd+F`）触发搜索栏。
- 可用 `.appearance(false)` 移除默认外观以自定义容器样式。
