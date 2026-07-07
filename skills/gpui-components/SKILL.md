---
name: gpui-components
description: >-
  Reference index for the longbridge gpui-component Rust UI library. Provides
  per-component summaries, core APIs, and copy-ready Rust usage examples for 57
  components (Button, Dialog, Table, Chart, Input, Sidebar, etc.). Use when
  building GPUI / Zed-style desktop apps, importing from the gpui_component
  crate, or when the user asks about gpui-component components, props, or usage
  examples. 中文组件速查，按类别索引到每个组件的详情文档。
---

# GPUI Components 组件库速查

基于 [longbridge/gpui-component](https://longbridge.github.io/gpui-component) 的中文速查 Skill。覆盖 **7 篇基础指南 + 57 个组件**：基础指南（`guide/`）讲解入门概念与配置，组件部分（`components/`）每个都有独立的详情文档（简介、核心 API、可复制的 Rust 代码示例、备注）。

## 何时使用

- 用 [GPUI](https://github.com/zed-industries/zed) / Zed 风格构建 Rust 桌面应用时
- 从 `gpui_component` crate 引入组件、查找组件名或 API 时
- 用户询问 gpui-component 某个组件的用法、Props 或示例时

## 如何使用

1. 在下方索引表中按类别定位目标组件
2. 打开对应 `components/<name>.md` 阅读核心 API 与代码示例
3. 直接复用示例代码（均来自官方文档），按需调整

> 所有组件详情文档位于 `components/` 目录。文件名采用 kebab-case（如 `alert-dialog.md`、`color-picker.md`、`dropdown-button.md`）。

## 基础指南

使用组件前建议先了解的基础概念与配置（文档位于 `guide/` 目录）。

| 主题 | 介绍 | 详情文档 |
|------|------|----------|
| Getting Started | 快速上手 GPUI Component：从依赖配置到第一个窗口应用 | [guide/getting-started.md](guide/getting-started.md) |
| Installation | 安装系统依赖与 Rust 工具链以开始使用 GPUI Component | [guide/installation.md](guide/installation.md) |
| Icons & Assets | 为 GPUI Component 提供图标与资源：使用默认打包资源或自建资源 | [guide/assets.md](guide/assets.md) |
| Context | 理解 GPUI 中 Window、App、Context 与 Entity 四个核心概念及 `cx` 命名约定 | [guide/context.md](guide/context.md) |
| ElementId | GPUI 中用于唯一标识元素的 ID，是绑定事件与管理元素状态的基础 | [guide/element-id.md](guide/element-id.md) |
| Theme | 内置主题系统，通过 ActiveTheme trait 提供统一的颜色访问能力 | [guide/theme.md](guide/theme.md) |
| Root View | GPUI Component 功能的根 Provider，必须作为窗口的第一层子元素 | [guide/root.md](guide/root.md) |

## 组件索引

索引格式：**组件 → 组件介绍 → 详情文档**。

### 基础元素

| 组件 | 组件介绍 | 详情文档 |
|------|----------|----------|
| Avatar | 用户头像组件，支持图片展示及智能降级（首字母/占位符） | [components/avatar.md](components/avatar.md) |
| Badge | 用于显示未读消息数、状态或通知数的徽标组件 | [components/badge.md](components/badge.md) |
| Button | 多变体、多尺寸、支持图标与加载状态的按钮组件 | [components/button.md](components/button.md) |
| DropdownButton | 按钮与下拉菜单触发器的组合组件，左侧按钮和右侧触发器可独立响应事件 | [components/dropdown-button.md](components/dropdown-button.md) |
| Icon | 从内置 Lucide 图标库渲染 SVG 图标，支持自定义尺寸、颜色和旋转 | [components/icon.md](components/icon.md) |
| Image | 基于 GPUI 原生图像支持的图片组件，支持多种来源、ObjectFit 与加载/错误状态处理 | [components/image.md](components/image.md) |
| Kbd | 显示键盘快捷键，并按平台（macOS/Windows/Linux）自动格式化 | [components/kbd.md](components/kbd.md) |
| Label | 支持次级文本、高亮、掩码和自定义样式的通用文本标签组件 | [components/label.md](components/label.md) |
| Tag | 标签组件，用于对内容进行分类或标记的紧凑视觉元素 | [components/tag.md](components/tag.md) |

### 布局与容器

| 组件 | 组件介绍 | 详情文档 |
|------|----------|----------|
| Accordion | 允许用户展开/折叠内容分区的可折叠面板组件 | [components/accordion.md](components/accordion.md) |
| Collapsible | 可展开/收起的交互式容器组件 | [components/collapsible.md](components/collapsible.md) |
| GroupBox | 带可选标题的样式化容器组件，用于将相关内容分组展示 | [components/group-box.md](components/group-box.md) |
| Resizable | 可调整尺寸的分割面板组件，通过拖动分隔条动态改变各面板大小，支持水平/垂直与嵌套 | [components/resizable.md](components/resizable.md) |
| Scrollable | 可滚动容器组件，提供自定义滚动条、滚动追踪与虚拟化能力 | [components/scrollable.md](components/scrollable.md) |

### 导航

| 组件 | 组件介绍 | 详情文档 |
|------|----------|----------|
| Menu | 支持图标、快捷键、子菜单、可勾选项和自定义元素的上下文/弹出菜单组件 | [components/menu.md](components/menu.md) |
| Pagination | 分页导航组件，提供页码、上一页/下一页按钮，支持多种尺寸和紧凑模式 | [components/pagination.md](components/pagination.md) |
| Sidebar | 可组合、可主题化的侧边栏导航组件，支持折叠、嵌套菜单与响应式设计 | [components/sidebar.md](components/sidebar.md) |
| Tabs | 标签页组件，将内容组织为多个面板，同一时间只显示其中一个 | [components/tabs.md](components/tabs.md) |
| Tree | 树形组件，用于展示层级数据，支持展开/折叠、键盘导航与自定义节点渲染 | [components/tree.md](components/tree.md) |

### 表单与输入

| 组件 | 组件介绍 | 详情文档 |
|------|----------|----------|
| Calendar | 灵活的日历组件，支持月份浏览、日期导航及单日期/日期范围选择 | [components/calendar.md](components/calendar.md) |
| Checkbox | 复选框组件，允许用户在选中与未选中之间切换 | [components/checkbox.md](components/checkbox.md) |
| ColorPicker | 综合性颜色选择器，支持多种颜色格式、预设色与 Alpha 透明通道 | [components/color-picker.md](components/color-picker.md) |
| DatePicker | 灵活的日期选择器，支持单日期、日期范围、自定义格式与禁用日期 | [components/date-picker.md](components/date-picker.md) |
| Editor | 功能强大的多行文本输入组件，支持自动调整高度、语法高亮、行号与代码编辑 | [components/editor.md](components/editor.md) |
| Form | 灵活的表单容器组件，支持字段布局、校验、分组与多列响应式布局 | [components/form.md](components/form.md) |
| Input | 支持验证、掩码、前后缀元素和多种状态的灵活文本输入组件 | [components/input.md](components/input.md) |
| NumberInput | 数值输入框，内置增减按钮，支持最大/最小值、步进和千分位格式化 | [components/number-input.md](components/number-input.md) |
| OtpInput | 一次性密码（OTP）输入组件，多输入框自动聚焦并支持粘贴整段验证码 | [components/otp-input.md](components/otp-input.md) |
| Radio | 单选框组件，用于从一组互斥选项中选择一个值，支持水平/垂直布局、多尺寸与受控状态 | [components/radio.md](components/radio.md) |
| Rating | 评分组件，用星星等图标让用户对内容进行星级打分，支持半星、只读、自定义图标和数量 | [components/rating.md](components/rating.md) |
| Select | 下拉选择组件，允许用户从一组选项中选择一个或多个值 | [components/select.md](components/select.md) |
| Slider | 数值范围选择滑块组件，支持单值/区间选择、水平/垂直方向与自定义步进 | [components/slider.md](components/slider.md) |
| Stepper | 分步进度导航组件，引导用户按步骤完成多阶段流程 | [components/stepper.md](components/stepper.md) |
| Switch | 开关组件，用于在选中与未选中两种状态之间切换 | [components/switch.md](components/switch.md) |

### 反馈与覆盖层

| 组件 | 组件介绍 | 详情文档 |
|------|----------|----------|
| Alert | 用于吸引用户注意力的消息提示框，支持多种语义变体 | [components/alert.md](components/alert.md) |
| AlertDialog | 打断用户操作、要求用户确认或响应的模态对话框 | [components/alert-dialog.md](components/alert-dialog.md) |
| Dialog | 用于在应用层之上展示对话框、确认框和提示框的组件 | [components/dialog.md](components/dialog.md) |
| FocusTrap | 焦点陷阱工具，将键盘焦点限制在指定容器内，防止 Tab 导航越界 | [components/focus-trap.md](components/focus-trap.md) |
| HoverCard | 鼠标悬停在触发元素上时显示富内容的浮动浮层组件 | [components/hover-card.md](components/hover-card.md) |
| Notification | 在窗口右上角显示临时 Toast 通知，支持多种类型、自动消失和操作按钮 | [components/notification.md](components/notification.md) |
| Popover | 浮层组件，相对于触发元素显示富内容，支持多种定位、自定义内容和受控开关 | [components/popover.md](components/popover.md) |
| Sheet | 从屏幕边缘滑入的面板组件，用于展示导航、表单或辅助内容 | [components/sheet.md](components/sheet.md) |
| Tooltip | 悬浮提示组件，在鼠标悬停或聚焦时显示帮助信息，支持快捷键说明和自定义内容 | [components/tooltip.md](components/tooltip.md) |

### 数据展示

| 组件 | 组件介绍 | 详情文档 |
|------|----------|----------|
| Chart | 数据可视化图表库，提供折线图、柱状图、面积图、饼图、K线图等多种类型 | [components/chart.md](components/chart.md) |
| DataTable | 用于展示与交互的可排序、可筛选、可分页表格组件 | [components/data-table.md](components/data-table.md) |
| DescriptionList | 用于以整洁的键值对布局展示详细信息的组件 | [components/description-list.md](components/description-list.md) |
| List | 虚拟化、可搜索的列表组件，支持分组、选择和无限滚动（基于 delegate 模式） | [components/list.md](components/list.md) |
| Plot | 底层绘图库，提供 Scale、Shape 等基础构件，用于构建自定义图表与数据可视化 | [components/plot.md](components/plot.md) |
| Table | 高性能数据表格组件，支持虚拟滚动、排序、筛选与列管理，适合展示大规模数据 | [components/table.md](components/table.md) |
| VirtualList | 高性能虚拟列表组件，用于渲染大规模数据集，支持可变项目尺寸 | [components/virtual-list.md](components/virtual-list.md) |

### 状态指示

| 组件 | 组件介绍 | 详情文档 |
|------|----------|----------|
| Progress | 线性进度条组件，以 0-100 的百分比直观展示任务完成进度 | [components/progress.md](components/progress.md) |
| Skeleton | 内容加载时的动画占位符组件，用于维持布局结构并提供加载反馈 | [components/skeleton.md](components/skeleton.md) |
| Spinner | 旋转动画加载指示器，用于表示任务进行中的加载状态 | [components/spinner.md](components/spinner.md) |
| Toggle | 按钮式切换组件，用于表达开/关或选中/未选中的二元状态 | [components/toggle.md](components/toggle.md) |

### 工具与窗口

| 组件 | 组件介绍 | 详情文档 |
|------|----------|----------|
| Clipboard | 一键复制文本到剪贴板的按钮组件，复制成功后图标自动变为对勾 | [components/clipboard.md](components/clipboard.md) |
| Settings | 应用设置界面组件，提供分组的设置项与多页面管理，支持搜索过滤 | [components/settings.md](components/settings.md) |
| TitleBar | 可自定义的窗口标题栏，可替代系统默认标题栏 | [components/title-bar.md](components/title-bar.md) |

## 备注

- 组件总数：**57 个**，与官方文档组件列表一一对应。
- 基础指南：**7 篇**（Getting Started、Installation、Icons & Assets、Context、ElementId、Theme、Root View）。
- 详情文档中的代码示例均提取自官方文档原文，可直接参考使用。
- 官方文档：<https://longbridge.github.io/gpui-component/docs/components>
