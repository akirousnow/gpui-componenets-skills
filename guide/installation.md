# Installation

> 安装系统依赖与 Rust 工具链以开始使用 GPUI Component。

源文档: https://longbridge.github.io/gpui-component/docs/installation

## 简介

在构建基于 `gpui-component` 的应用前，需先准备运行环境：安装 Rust 工具链、Cargo 包管理器，以及各平台所需的系统依赖。GPUI Component 支持在 macOS、Windows 和 Linux 上开发，各平台提供了引导脚本来简化依赖安装。

## 关键要点

- **语言**：使用 Rust 编写，需安装 Rust 与 Cargo。
- **依赖来源**：`gpui`、`gpui_platform`、`gpui-component` 均通过 Git 仓库引入，添加到 `Cargo.toml` 的 `[dependencies]`。
- **平台脚本**：
  - Windows：在 PowerShell 中运行 `.\script\install-window.ps1` 安装工具链与依赖。
  - Linux：运行 `./script/bootstrap` 安装系统依赖。
  - macOS：按 Rust 官方指引安装工具链。
- **font-kit 特性**：`gpui_platform` 需启用 `font-kit` feature 以支持字体加载。

## 代码示例

在 `Cargo.toml` 中添加依赖：

```toml
[dependencies]
gpui = { git = "https://github.com/zed-industries/zed" }
gpui_platform = { git = "https://github.com/zed-industries/zed", features = ["font-kit"] }
gpui-component = { git = "https://github.com/longbridge/gpui-component" }
```

Windows 安装系统依赖（PowerShell）：

```powershell
.\script\install-window.ps1
```

Linux 安装系统依赖：

```bash
./script/bootstrap
```

## 备注

- GPUI Component 采用 Apache-2.0 协议，由 [Longbridge](https://longbridge.com) 开发。
- 完整依赖配置（含可选的图标资源 `gpui-component-assets`）请参考 [Getting Started](./getting-started.md)。
