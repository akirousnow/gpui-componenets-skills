# Avatar

> 用户头像组件，支持图片展示及智能降级（首字母/占位符）。

源文档: https://longbridge.github.io/gpui-component/docs/components/avatar

## 简介

Avatar 组件用于展示用户的头像图片。当没有可用图片时，会智能降级：先尝试显示用户名称的首字母，若也没有名称则显示占位图标。组件支持多种尺寸、自定义图片源、圆角等样式，适合用于用户列表、评论、导航栏个人中心等场景。

## 核心 API

| 名称 | 类型 | 说明 |
|------|------|------|
| `Avatar::new(id)` | 构造函数 | 创建头像实例 |
| `source(url)` / `image(...)` | 方法 | 设置头像图片源 |
| `name(text)` | 方法 | 设置用户名称（用于首字母降级） |
| `avatar_name(text)` | 方法 | 设置自定义首字母文本 |
| `size(px)` / `small()` / `large()` | 方法 | 设置尺寸 |
| `square(bool)` | 方法 | 是否为方形（默认圆形） |
| `corner_radius(px)` | 方法 | 自定义圆角 |
| `AvatarBadge` | 组件 | 头像角标（如在线状态点） |

## 基本用法

基础头像：

```rust
use gpui_component::avatar::Avatar;

Avatar::new("user-avatar")
    .source("https://example.com/avatar.png")
    .name("John Doe")
```

带降级的头像（无图片时显示首字母）：

```rust
Avatar::new("user-avatar")
    .name("Alice")
```

不同尺寸与方形：

```rust
Avatar::new("avatar-small").name("Bob").small()
Avatar::new("avatar-large").name("Carol").large()
Avatar::new("avatar-square").name("Dan").square()
```

## 备注

- 图片加载失败或未提供时，自动降级为首字母，再降级为默认占位图标。
- 支持 `AvatarBadge` 在头像角落显示状态指示（如在线绿点）。
- 尺寸也可通过实现 `Sizable` trait 的方法调整。
