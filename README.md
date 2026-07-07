# GPUI Components 组件库速查 Skill

[![skills.sh](https://skills.sh/b/akirousnow/gpui-componenets-skills)](https://skills.sh/akirousnow/gpui-componenets-skills)

基于 [longbridge/gpui-component](https://longbridge.github.io/gpui-component) 的中文速查 Skill，为 **57 个组件 + 7 篇基础指南** 提供核心 API 与可复制的 Rust 代码示例。

适用于使用 [GPUI](https://github.com/zed-industries/zed) / Zed 风格构建 Rust 跨平台桌面应用、或从 `gpui_component` crate 查找组件用法的场景。

## 这是什么

一个面向 AI 编码助手（Cursor / Claude Code 等）的 Agent Skill。它把官方文档整理为「按类别索引的中文速查表」，让你在写 GPUI 应用时，由 AI 直接给出准确的组件名、核心 API 与可直接复用的 Rust 示例，而不必每次翻阅官方文档。

覆盖内容：

- **7 篇基础指南**（`skills/gpui-components/guide/`）：Getting Started、Installation、Icons & Assets、Context、ElementId、Theme、Root View。
- **57 个组件**（`skills/gpui-components/components/`）：每个都有独立详情文档，包含简介、核心 API 表、Rust 代码示例与备注。

## 安装

### 通过 Cursor 使用

将本仓库克隆到你的 Cursor skills 目录，或在 Cursor 中引用 `SKILL.md`：

```bash
git clone https://github.com/akirousnow/gpui-componenets-skills.git
```

### 通过 skills.sh 安装

```bash
npx skills add akirousnow/gpui-componenets-skills
```

## 组件目录

> 完整索引见 [`SKILL.md`](skills/gpui-components/SKILL.md)，此处按类别概览。

### 基础元素

Avatar · Badge · Button · DropdownButton · Icon · Image · Kbd · Label · Tag

### 布局与容器

Accordion · Collapsible · GroupBox · Resizable · Scrollable

### 导航

Menu · Pagination · Sidebar · Tabs · Tree

### 表单与输入

Calendar · Checkbox · ColorPicker · DatePicker · Editor · Form · Input · NumberInput · OtpInput · Radio · Rating · Select · Slider · Stepper · Switch

### 反馈与覆盖层

Alert · AlertDialog · Dialog · FocusTrap · HoverCard · Notification · Popover · Sheet · Tooltip

### 数据展示

Chart · DataTable · DescriptionList · List · Plot · Table · VirtualList

### 状态指示

Progress · Skeleton · Spinner · Toggle

### 工具与窗口

Clipboard · Settings · TitleBar

## 如何使用

1. 在 [`SKILL.md`](skills/gpui-components/SKILL.md) 的索引表中按类别定位目标组件。
2. 打开对应 `skills/gpui-components/components/<name>.md` 阅读核心 API 与代码示例。
3. 直接复用示例代码（均提取自官方文档原文），按需调整。

## 备注与约定

- 组件详情文档位于 `skills/gpui-components/components/` 目录，文件名采用 kebab-case（如 `alert-dialog.md`、`color-picker.md`）。
- 基础指南位于 `skills/gpui-components/guide/` 目录。
- 仓库结构遵循 [skills.sh](https://skills.sh) / Cursor Agent Skill 约定：skill 位于 `skills/<name>/` 子目录，确保 `npx skills add` 能完整打包 `SKILL.md` 及其引用的 `components/`、`guide/` 资源。
- 代码示例均来自官方文档，可直接参考使用。
- 官方文档：<https://longbridge.github.io/gpui-component/docs/components>

## 致谢

- 组件库原作者：[longbridge/gpui-component](https://github.com/longbridge/gpui-component)
- GPUI 框架：[zed-industries/zed](https://github.com/zed-industries/zed)

## License

本项目为文档整理，随上游项目协议。
