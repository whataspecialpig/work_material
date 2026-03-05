# GUI 开发常见英文词汇表

整理自 Dear ImGui 项目的实际代码和概念。

## 分类索引
- [1. 核心概念](#1-核心概念)
- [2. 窗口系统](#2-窗口系统)
- [3. 渲染相关](#3-渲染相关)
- [4. 输入输出](#4-输入输出)
- [5. 布局和定位](#5-布局和定位)
- [6. 控件 Widgets](#6-控件-widgets)
- [7. 字体和文本](#7-字体和文本)
- [8. 事件系统](#8-事件系统)
- [9. 图形基础](#9-图形基础)
- [10. 状态管理](#10-状态管理)

---

## 1. 核心概念

### Context (上下文)
- **概念**: GUI 系统的全局状态容器
- **例句**: `ImGui::CreateContext()` 创建 ImGui 上下文
- **说明**: 包含所有窗口、字体、状态等数据

### Frame (帧)
- **概念**: GUI 的一次完整更新周期
- **例句**: `ImGui::NewFrame()` 开始新的一帧
- **说明**: 通常对应屏幕刷新一次（如 60 FPS = 每秒 60 帧）

### Backend (后端)
- **概念**: 连接 GUI 库与平台/渲染器的适配层
- **例句**: `imgui_impl_sdl3.cpp` 是 SDL3 平台后端
- **说明**: 分为 Platform Backend（平台）和 Renderer Backend（渲染器）

### API (Application Programming Interface)
- **概念**: 应用程序编程接口
- **例句**: `IMGUI_API` 宏标记公开的函数
- **说明**: 库提供给用户调用的函数集合

---

## 2. 窗口系统

### Window (窗口)
- **概念**: GUI 的基本容器单元
- **例句**: `ImGui::Begin("My Window")` 创建窗口
- **说明**: 可以包含各种控件

### Viewport (视口)
- **概念**: 显示区域、可视范围
- **例句**: `ImGuiViewport* viewport = ImGui::GetMainViewport()`
- **说明**: 多视口支持多个独立窗口

### Child Window (子窗口)
- **概念**: 嵌套在主窗口内的子区域
- **例句**: `ImGui::BeginChild("child1")`
- **说明**: 可以有独立的滚动条

### Modal (模态窗口)
- **概念**: 阻止用户操作其他窗口的弹窗
- **例句**: `ImGui::OpenPopup("Delete?")` 打开模态弹窗
- **说明**: 必须关闭才能继续操作

### Popup (弹出窗口)
- **概念**: 临时显示的小窗口
- **例句**: `ImGui::BeginPopup("popup")`
- **说明**: 常用于右键菜单、提示等

### Tooltip (工具提示)
- **概念**: 鼠标悬停时显示的提示信息
- **例句**: `ImGui::SetTooltip("Hover text")`
- **说明**: 通常用于解释控件功能

### Collapse (折叠)
- **概念**: 收起/展开窗口内容
- **例句**: 点击窗口标题栏的箭头
- **说明**: 折叠后只显示标题栏

### Docking (停靠)
- **概念**: 窗口吸附到边缘或其他窗口
- **例句**: ImGui Docking 分支功能
- **说明**: 类似 IDE 的窗口布局系统

---

## 3. 渲染相关

### Render (渲染)
- **概念**: 将数据绘制到屏幕上
- **例句**: `ImGui::Render()` 生成渲染数据
- **说明**: 将 UI 转换为图形命令

### Draw Call (绘制调用)
- **概念**: 一次 GPU 绘制命令
- **例句**: `ImDrawCmd` 结构体
- **说明**: 数量越少性能越好

### Vertex (顶点)
- **概念**: 图形的基本点
- **例句**: `ImDrawVert` 包含位置、UV、颜色
- **说明**: 三角形由 3 个顶点组成

### Index (索引)
- **概念**: 指向顶点的引用
- **例句**: `ImDrawIdx` 通常是 16 位或 32 位
- **说明**: 避免重复顶点数据

### Buffer (缓冲区)
- **概念**: 存储数据的内存块
- **例句**: `VtxBuffer`（顶点缓冲）、`IdxBuffer`（索引缓冲）
- **说明**: GPU 从缓冲区读取数据渲染

### Texture (纹理)
- **概念**: GPU 上的图片数据
- **例句**: `ImTextureID` 标识一个纹理
- **说明**: 用于显示图片、字体等

### Atlas (图集)
- **概念**: 多个小图打包成一张大纹理
- **例句**: `ImFontAtlas` 字体图集
- **说明**: 减少纹理切换，提升性能

### Shader (着色器)
- **概念**: GPU 程序，处理顶点和像素
- **例句**: ImGui 使用简单的顶点/片段着色器
- **说明**: GLSL、HLSL 等着色器语言

### VBO (Vertex Buffer Object)
- **概念**: OpenGL 的顶点缓冲对象
- **例句**: 存储顶点数据到 GPU
- **说明**: 提高渲染效率

### VAO (Vertex Array Object)
- **概念**: OpenGL 的顶点数组对象
- **例句**: 记录顶点属性配置
- **说明**: 简化顶点数据绑定

### Clip (裁剪)
- **概念**: 限制绘制区域
- **例句**: `ImDrawCmd::ClipRect`
- **说明**: 超出区域的内容不显示

### Scissor (剪刀测试)
- **概念**: GPU 的矩形裁剪功能
- **例句**: `glScissor(x, y, w, h)`
- **说明**: 实现 Clip 的硬件加速

### Alpha (透明度)
- **概念**: 颜色的不透明度
- **例句**: RGBA 中的 A 通道
- **说明**: 0.0 = 完全透明，1.0 = 完全不透明

### Blend (混合)
- **概念**: 新旧像素的混合方式
- **例句**: `glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA)`
- **说明**: 实现透明效果

---

## 4. 输入输出

### Input (输入)
- **概念**: 用户的操作数据
- **例句**: 鼠标、键盘、触摸等
- **说明**: 通过 `ImGuiIO` 结构体管理

### Event (事件)
- **概念**: 系统或用户触发的动作
- **例句**: `SDL_Event` 鼠标点击事件
- **说明**: 需要在主循环中处理

### Mouse (鼠标)
- **概念**: 指针设备
- **例句**: `io.MousePos` 鼠标位置
- **说明**: 包括位置、按钮、滚轮

### Keyboard (键盘)
- **概念**: 按键输入设备
- **例句**: `io.AddKeyEvent(ImGuiKey_Space, true)`
- **说明**: ImGui 使用虚拟按键枚举

### Gamepad / Joystick (游戏手柄)
- **概念**: 游戏控制器
- **例句**: `ImGuiKey_GamepadStart`
- **说明**: 支持手柄导航

### Touch (触摸)
- **概念**: 触摸屏输入
- **例句**: `ImGuiMouseSource_TouchScreen`
- **说明**: 可以区分鼠标和触摸

### Hover (悬停)
- **概念**: 鼠标停留在某个元素上
- **例句**: `ImGui::IsItemHovered()` 检测悬停
- **说明**: 常用于显示提示

### Focus (焦点)
- **概念**: 当前接收输入的元素
- **例句**: `ImGui::SetKeyboardFocusHere()`
- **说明**: 影响键盘输入目标

### Capture (捕获)
- **概念**: 独占输入事件
- **例句**: `io.WantCaptureMouse` ImGui 想捕获鼠标
- **说明**: 避免输入同时影响 GUI 和应用

### Cursor (光标)
- **概念**: 鼠标指针的形状
- **例句**: `ImGuiMouseCursor_Hand` 手型光标
- **说明**: 提示用户当前操作类型

### IME (Input Method Editor 输入法)
- **概念**: 多字节字符输入系统
- **例句**: 中文、日文输入
- **说明**: 需要平台后端支持

---

## 5. 布局和定位

### Layout (布局)
- **概念**: 控件的排列方式
- **例句**: 水平布局、垂直布局
- **说明**: ImGui 默认垂直堆叠

### Positioning (定位)
- **概念**: 确定元素的位置
- **例句**: `ImGui::SetCursorPos()`
- **说明**: 可以是绝对或相对位置

### Spacing (间距)
- **概念**: 元素之间的空白
- **例句**: `ImGui::Spacing()` 添加垂直间距
- **说明**: 影响视觉密度

### Padding (内边距)
- **概念**: 内容到边界的距离
- **例句**: `style.WindowPadding`
- **说明**: 影响窗口内容边距

### Margin (外边距)
- **概念**: 元素外部的空白
- **例句**: 元素之间的距离
- **说明**: ImGui 通过 Spacing 实现

### Align (对齐)
- **概念**: 元素的对齐方式
- **例句**: 左对齐、居中、右对齐
- **说明**: `ImGui::AlignTextToFramePadding()`

### SameLine (同一行)
- **概念**: 下一个控件不换行
- **例句**: `ImGui::SameLine()`
- **说明**: 实现横向布局

### Indent (缩进)
- **概念**: 向右偏移
- **例句**: `ImGui::Indent(16.0f)`
- **说明**: 常用于树形结构

### Scroll (滚动)
- **概念**: 内容超出区域时移动视图
- **例句**: `ImGui::SetScrollY(100.0f)`
- **说明**: 支持垂直和水平滚动

### Wrap (换行)
- **概念**: 文本自动折行
- **例句**: `ImGui::TextWrapped("long text...")`
- **说明**: 根据宽度自动换行

### Clipping (裁剪)
- **概念**: 限制内容显示区域
- **例句**: 滚动区域外的内容被裁剪
- **说明**: 提高性能，避免绘制不可见内容

---

## 6. 控件 Widgets

### Widget (控件/组件)
- **概念**: GUI 的交互元素
- **例句**: 按钮、滑块、输入框等
- **说明**: ImGui 的主要构建块

### Button (按钮)
- **概念**: 可点击的矩形控件
- **例句**: `if (ImGui::Button("OK")) { }`
- **说明**: 返回 true 表示被点击

### Checkbox (复选框)
- **概念**: 布尔值切换控件
- **例句**: `ImGui::Checkbox("Enable", &flag)`
- **说明**: 直接修改绑定的变量

### Radio Button (单选按钮)
- **概念**: 多选一的控件
- **例句**: `ImGui::RadioButton("Option A", &value, 0)`
- **说明**: 同组中只能选一个

### Slider (滑块)
- **概念**: 拖动选择数值的控件
- **例句**: `ImGui::SliderFloat("value", &f, 0.0f, 1.0f)`
- **说明**: 可视化数值范围

### Input / InputText (输入框)
- **概念**: 文本输入控件
- **例句**: `ImGui::InputText("name", buf, 256)`
- **说明**: 支持各种文本编辑功能

### Combo / Dropdown (下拉框)
- **概念**: 可展开的选项列表
- **例句**: `ImGui::Combo("items", &item, items_array)`
- **说明**: 节省空间的多选项控件

### ListBox (列表框)
- **概念**: 可滚动的选项列表
- **例句**: `ImGui::ListBox("list", &selection, items, count)`
- **说明**: 比 Combo 更直观

### Tree / TreeNode (树形控件)
- **概念**: 层级展开的结构
- **例句**: `if (ImGui::TreeNode("Node")) { }`
- **说明**: 常用于文件浏览器

### Menu (菜单)
- **概念**: 命令列表
- **例句**: `ImGui::BeginMenu("File")` 菜单栏
- **说明**: 支持嵌套子菜单

### Tab (标签页)
- **概念**: 切换不同页面的控件
- **例句**: `ImGui::BeginTabBar("tabs")`
- **说明**: 组织大量内容

### Table (表格)
- **概念**: 二维数据展示
- **例句**: `ImGui::BeginTable("table", 3)`
- **说明**: 支持排序、筛选等

### Plot (图表)
- **概念**: 数据可视化
- **例句**: `ImGui::PlotLines("Graph", data, count)`
- **说明**: 绘制折线图、直方图等

### Color Picker (颜色选择器)
- **概念**: 选择颜色的控件
- **例句**: `ImGui::ColorEdit3("color", rgb)`
- **说明**: 可视化 RGB/HSV 颜色

### ProgressBar (进度条)
- **概念**: 显示进度的控件
- **例句**: `ImGui::ProgressBar(0.5f)`
- **说明**: 0.0 到 1.0 表示进度

### Image (图片)
- **概念**: 显示纹理/图片
- **例句**: `ImGui::Image(texture, ImVec2(100, 100))`
- **说明**: 可以作为按钮

### Separator (分隔符)
- **概念**: 视觉分隔线
- **例句**: `ImGui::Separator()`
- **说明**: 分组内容

---

## 7. 字体和文本

### Font (字体)
- **概念**: 字符的视觉样式
- **例句**: `ImFont* font = io.Fonts->AddFontFromFileTTF(...)`
- **说明**: 支持 TTF/OTF 格式

### Glyph (字形)
- **概念**: 单个字符的图形表示
- **例句**: `ImFontGlyph` 结构体
- **说明**: 包含字符在图集中的位置

### Rasterize (栅格化)
- **概念**: 将矢量字体转为像素
- **例句**: stb_truetype 栅格化引擎
- **说明**: 生成可显示的位图

### Kerning (字距调整)
- **概念**: 调整字符间距
- **例句**: 'AV' 之间的间距优化
- **说明**: 提高可读性

### Baseline (基线)
- **概念**: 文字的参考线
- **例句**: 字母 'p' 下方超出基线
- **说明**: 对齐文本的基准

### Ascent / Descent (上升/下降)
- **概念**: 字符相对基线的高度
- **例句**: Ascent = 基线到顶部，Descent = 基线到底部
- **说明**: 决定行高

### Leading / Line Height (行距/行高)
- **概念**: 两行文本之间的距离
- **例句**: `style.TextLineHeight`
- **说明**: 影响文本密度

### UTF-8 / Unicode
- **概念**: 字符编码标准
- **例句**: 支持国际化文本
- **说明**: ImGui 内部使用 UTF-8

### Codepoint (码点)
- **概念**: Unicode 字符的数值
- **例句**: 'A' = U+0041 = 65
- **说明**: 字符的唯一标识

### Glyph Range (字形范围)
- **概念**: 要加载的字符区间
- **例句**: `io.Fonts->GetGlyphRangesChinese()`
- **说明**: 减少内存占用

---

## 8. 事件系统

### Callback (回调)
- **概念**: 事件触发时调用的函数
- **例句**: `ImDrawCallback`
- **说明**: 用户自定义处理逻辑

### Hook (钩子)
- **概念**: 拦截或扩展功能的机制
- **例句**: `ImGuiContextHook`
- **说明**: 在特定时机执行代码

### Signal / Slot (信号/槽)
- **概念**: Qt 的事件机制（其他框架）
- **例句**: `connect(button, SIGNAL(clicked()))`
- **说明**: ImGui 不使用，但其他 GUI 常见

### Delegate (委托)
- **概念**: 将任务委托给其他对象
- **例句**: C# 的委托模式
- **说明**: 类似回调

### Observer (观察者)
- **概念**: 监听状态变化的设计模式
- **例句**: 订阅-发布模式
- **说明**: 常见于 MVC 架构

---

## 9. 图形基础

### Pixel (像素)
- **概念**: 屏幕的最小单位
- **例句**: 1920x1080 分辨率 = 1920 像素宽
- **说明**: 物理屏幕点

### DPI (Dots Per Inch)
- **概念**: 每英寸点数（分辨率密度）
- **例句**: Retina 显示器通常 > 200 DPI
- **说明**: 影响 UI 缩放

### Scale (缩放)
- **概念**: 尺寸的比例调整
- **例句**: `style.ScaleAllSizes(2.0f)` 放大 2 倍
- **说明**: 适配不同 DPI 屏幕

### Resolution (分辨率)
- **概念**: 屏幕像素数量
- **例句**: 1920x1080（宽x高）
- **说明**: 影响显示清晰度

### Aspect Ratio (宽高比)
- **概念**: 宽度与高度的比例
- **例句**: 16:9、4:3
- **说明**: 影响窗口形状

### RGBA (Red Green Blue Alpha)
- **概念**: 颜色表示方式
- **例句**: (255, 0, 0, 255) = 红色不透明
- **说明**: 4 个通道各 0-255

### HSV / HSL (色相-饱和度-明度)
- **概念**: 另一种颜色表示
- **例句**: 颜色选择器常用
- **说明**: 更符合人类认知

### Primitive (图元)
- **概念**: 基本图形单位
- **例句**: 点、线、三角形
- **说明**: GPU 绘制的基础

### Triangle (三角形)
- **概念**: 最基本的多边形
- **例句**: 所有 2D/3D 图形都由三角形组成
- **说明**: GPU 硬件优化的图元

### Quad (四边形)
- **概念**: 4 个顶点的矩形
- **例句**: 实际由 2 个三角形组成
- **说明**: UI 元素的常见形状

### Stroke (描边)
- **概念**: 图形的轮廓线
- **例句**: `ImGui::AddRect(..., thickness)`
- **说明**: 与 Fill（填充）相对

### Fill (填充)
- **概念**: 图形内部的颜色
- **例句**: `ImGui::AddRectFilled(...)`
- **说明**: 与 Stroke（描边）相对

---

## 10. 状态管理

### State (状态)
- **概念**: 系统当前的数据快照
- **例句**: 窗口打开/关闭状态
- **说明**: GUI 大量依赖状态

### Stateless (无状态)
- **概念**: 不保存状态的设计
- **例句**: REST API 的无状态性
- **说明**: ImGui 尽量减少状态存储

### Immediate Mode (即时模式)
- **概念**: 每帧重新描述 UI
- **例句**: ImGui 的核心理念
- **说明**: 与 Retained Mode 相对

### Retained Mode (保留模式)
- **概念**: 构建 UI 树并保持
- **例句**: Qt、WPF 的方式
- **说明**: 需要同步状态

### ID / Handle (标识符/句柄)
- **概念**: 唯一标识对象
- **例句**: `ImGuiID`、`ImTextureID`
- **说明**: 用于查找和管理资源

### Stack (栈)
- **概念**: 后进先出的数据结构
- **例句**: `ImGui::PushStyleColor()` / `PopStyleColor()`
- **说明**: 管理嵌套状态

### Flag (标志位)
- **概念**: 布尔选项的集合
- **例句**: `ImGuiWindowFlags_NoResize | ImGuiWindowFlags_NoMove`
- **说明**: 用位运算组合多个选项

### Dirty (脏标记)
- **概念**: 数据已修改，需要更新
- **例句**: `if (is_dirty) { rebuild(); }`
- **说明**: 避免不必要的重建

---

## 附录：常见缩写

| 缩写 | 全称 | 中文 |
|------|------|------|
| GUI | Graphical User Interface | 图形用户界面 |
| API | Application Programming Interface | 应用程序编程接口 |
| SDK | Software Development Kit | 软件开发工具包 |
| CPU | Central Processing Unit | 中央处理器 |
| GPU | Graphics Processing Unit | 图形处理器 |
| FPS | Frames Per Second | 每秒帧数 |
| DPI | Dots Per Inch | 每英寸点数 |
| VBO | Vertex Buffer Object | 顶点缓冲对象 |
| VAO | Vertex Array Object | 顶点数组对象 |
| UV | U-V Coordinates | 纹理坐标 |
| RGB | Red Green Blue | 红绿蓝 |
| RGBA | Red Green Blue Alpha | 红绿蓝透明度 |
| HSV | Hue Saturation Value | 色相饱和度明度 |
| TTF | TrueType Font | TrueType 字体 |
| OTF | OpenType Font | OpenType 字体 |
| IME | Input Method Editor | 输入法编辑器 |
| DLL | Dynamic Link Library | 动态链接库 |
| ABI | Application Binary Interface | 应用程序二进制接口 |

---

## 学习建议

1. **从高频词开始**: Window, Widget, Render 等核心概念
2. **分类记忆**: 按照功能模块分组学习
3. **结合代码**: 在 ImGui 源码中查找这些词的实际用法
4. **动手实践**: 使用这些 API 创建简单的 GUI
5. **查阅文档**: 遇到不懂的词查阅官方文档

---

最后更新: 2026-03-05
基于项目: Dear ImGui v1.92.7
