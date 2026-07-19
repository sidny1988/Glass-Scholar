# CHANGELOG — 璃墨 Glass Scholar

> Obsidian 磨砂玻璃主题。全透明光晕界面 · 极简 · 论文级排版。

## v1.0.0 · 初始发布

**日期**：2026-07-04

### 发布状态

| 检查项 | 状态 |
|---|---|
| manifest.json 合规 | ✅ name/version/minAppVersion/author/themeScheme |
| 无远程资源 | ✅ 无 @import，无外部字体/CDN |
| 深/浅双模式 | ✅ 自动跟随系统 (`themeScheme: "auto"`) |
| Style Settings | ✅ 11 项（光晕配色/模糊/强度/排版） |
| 移动端适配 | ✅ @media (max-width: 768px) |
| PDF 导出 | ✅ @media print 块（flexbox→block 防排版崩溃） |
| 变量命名规范 | ✅ --gs-* 前缀，80+ 变量 |
| 第三方插件保护 | ✅ x-color-picker / claudian / NN / ET |
| CSS 语法 | ✅ 花括号 216/216 平衡 |

### 技术要点

- 光晕层：`body::before` position:fixed + 4 椭圆径向渐变 + CSS blur filter
- 透明化：`body.theme-light/dark { --background-*: transparent }` + 30+ 容器透明规则
- 玻璃触感：`backdrop-filter: blur(12px)` + `--gs-glass-opacity: 0`（仅保留模糊无白底）
- 融合设计：间隔最小化、阴影极淡化、切换手柄透明、边框清零
- Print 修复：flexbox layout → block，fixed 伪元素 → display:none，透明变量 → 实色

### 开发历程

历经 22 版本迭代（v2.2.12 → v1.0.0），主要在三个方向打磨：
1. **透明穿透**：CSS 变量优先级、左侧栏 DOM 层级去背景、光晕全部可见
2. **界面融合**：去卡片化、去线条化、手柄透明、状态栏融合
3. **PDF 导出**：从 print 覆盖 → screen 隔离 → flex→block 布局重构

### 包含文件

```
manifest.json     theme.css     CHANGELOG.md     RELEASE.md
```

---

<details>
<summary>开发版本历史 (v2.2.13 ~ v2.2.34)</summary>

---

## v2.2.28 · PDF 导出：`@media screen` 隔离

**日期**：2026-07-04

整份主题 CSS 关进 `@media screen {}`。打印/Obsidian PDF 导出时主题规则完全不会加载，Obsidian 原生 CSS 接管 → 干净白纸。这是对 PDF 问题的根本性解决方案，而非逐条 print 覆盖的打补丁方式。

### 同步
- ✅ `D:\MyBrain` · ✅ `D:\MyNotes`

---

## v2.2.26 · PDF 导出：精准覆盖透明变量 + 修复定位约束 (已废弃)

**日期**：2026-07-04

### 根因（搜索 + 技能交叉验证）

1. **`body { position: relative; min-height: 100vh }`** — 打印时 body 被约束为视口高度，内容溢出 → 空白页泛滥
2. **`body::before / ::after { position: fixed }`** — 浏览器打印引擎不支持 `position:fixed`，伪元素在每一页重复渲染或导致布局错乱
3. **`body.theme-light { --background-*: transparent }`** — 特异性 0,1,1，之前 `@media print body { }` 只有 0,0,1 未能覆盖
4. **Obsidian 导出时强制 `.theme-light`**（来自论坛确认）→ 透明变量 100% 命中

### 修复（`@media print` 三武器）

| # | 目标 | 方式 |
|---|---|---|
| 1 | body 定位约束 | `position: static !important; min-height: auto !important` |
| 2 | 伪元素 | `body::before/::after` + 全部 8 个 `body.gs-wp-*::before` → `display:none` |
| 3 | 浅色变量 | `@media print body.theme-light`（0,1,1）→ 全部 18 个变量复位实白色 |
| 4 | 深色变量 | `@media print body.theme-dark`（0,1,1）→ 全部 18 个变量复位实深色 |
| 5 | backdrop/shadow | 关键容器 → `backdrop-filter:none; box-shadow:none` |
| 6 | 容器实白 | `.workspace-leaf` 等 → `background:#ffffff` |

### 同步
- ✅ `D:\MyBrain` · ✅ `D:\MyNotes`

**日期**：2026-07-04

### 问题

v2.2.24 的 `@media print` 太暴力：
- `*` 通配符施加 `!important` 搞乱所有元素
- `display:none` 踢掉 `.app-container > .horizontal-main-container` 等主干容器破坏页面结构
→ PDF 字体竖排、内容残缺、大量空白页

### 修复

`@media print` 从 200 行 → 45 行，只做三件事：
1. `body::before/::after` display:none（关光晕噪点）
2. body + 关键容器 → `background: #ffffff`
3. 玻璃面板 → backfrop-filter: none + box-shadow: none

其余一切交给 Obsidian 原生 PDF 导出引擎。

### 同步
- ✅ `D:\MyBrain` · ✅ `D:\MyNotes`

---

## v2.2.24 · PDF / 打印导出专属样式（已被 2.2.25 替代）

（问题：过于暴力，* 选择器 + display:none 容器 → PDF 排版崩溃）

**日期**：2026-07-04

### 修复

新增完整 `@media print {}` 块（文件末 200 行），关闭光晕+磨砂玻璃 → 干净白纸报告：
- `body::before/::after` → `display:none`
- 全部 CSS 变量重置为不透明白底
- 全局 `backdrop-filter: none !important; box-shadow: none !important`
- 代码块、引用、表格保留可读层次（淡灰底+细边框）
- 侧栏/顶栏/状态栏/弹窗全部隐藏
- `@page { margin: 2cm 2.2cm }`

### 同步
- ✅ `D:\MyBrain` · ✅ `D:\MyNotes`

---

## v2.2.23 · 完全透明实验

**日期**：2026-07-04

- `--gs-glass-opacity: 0.16 → 0`
- 四个 `--background-*` → `transparent`（浅/深色均改）

### 同步
- ✅ `D:\MyBrain` · ✅ `D:\MyNotes`

---

## v2.2.22 · 🔴 根因修复：CSS 优先级 bug — 透明背景被不透明色覆盖

**日期**：2026-07-04

### 问题

v2.2.19~v2.2.21 改了三版，递归透明了整个左侧栏 DOM 树，侧栏仍然无法显示光晕背景。用户发现关键线索：**左侧栏背景色和标签页选中态底色是同步变化的**。

### 真正根因（CSS 优先级 bug）

```
body {                    /* 优先级 0,0,1 */
  --background-secondary: color-mix(tint, 12%, transparent);
}
body.theme-light {        /* 优先级 0,1,1 → 胜出！ */
  --background-secondary: #f6f8fb;   ← 不透明纯色
}
body.theme-dark {         /* 优先级 0,1,1 → 胜出！ */
  --background-secondary: #16191e;   ← 不透明纯色
}
```

`body.theme-light` / `body.theme-dark` 的**选择器优先级更高**（0,1,1 vs 0,0,1），它们定义的不透明纯色（`#f6f8fb` / `#16191e`）覆盖了 `body {}` 中的透明 `color-mix()` 值。Obsidian 核心 CSS 中 `.mod-left-split`、`.nav-files-container` 等都引用 `var(--background-secondary)`，拿到的就是**不透明固体色**，光晕完全被遮挡。

右侧面板之所以正常，是因为它的 DOM 路径不同，受其他规则影响。

### 修复

| 变量 | 旧值 (body.theme-light) | 新值 |
|---|---|---|
| `--background-primary` | `#ffffff` | `color-mix(in srgb, var(--gs-lg-tint) 20%, transparent)` |
| `--background-primary-alt` | `#f4f6fa` | `color-mix(in srgb, var(--gs-lg-tint) 28%, transparent)` |
| `--background-secondary` | `#f6f8fb` | `color-mix(in srgb, var(--gs-lg-tint) 12%, transparent)` |
| `--background-secondary-alt` | `#eef1f6` | `color-mix(in srgb, var(--gs-lg-tint) 18%, transparent)` |
| `--background-secondary-rgb` | `246, 248, 251` | `255, 255, 255` |

深色同改（`#16191e` → 对应 tint `#232a35` 的 color-mix）。

此外从 `body {}` 块移除之前的透明变量定义（反正会被覆盖，放在那是误导）。

### 测试

1. `Ctrl+R` 刷新
2. 左侧文件导航栏和 Notebook Navigator**立即显示光晕背景**
3. 光晕色彩在整个界面均匀连通，左侧栏和主内容区底色一致
4. 切深色模式同样生效

### 同步
- ✅ `D:\MyBrain` · ✅ `D:\MyNotes`

---

**日期**：2026-07-04

### 问题

v2.2.17 后侧边导航栏失去磨砂玻璃效果，变成纯白/纯黑色块，没有光晕背景效果。

### 根因

v2.2.17 把透明容器规则去 `gs-wallpaper-on` 前缀时，将 `.workspace-leaf-content` 也设为了 `background: transparent !important`。这个 `!important` 覆盖了侧栏玻璃规则（`.workspace-leaf.mod-side-dock .workspace-leaf-content`）的 `background-color: var(--gs-glass-bg)`（42% 半透明白底）。

磨砂玻璃 = 半透明白底 + backdrop-filter blur，白底被冲掉后只剩透明底 → blur 透过光晕 → 看起来是纯白/黑色块。

### 修复

| # | 改动 |
|---|---|
| 1 | 从透明容器规则中移除 `.workspace-leaf` 和 `.workspace-leaf-content`，它们由玻璃规则专门处理 |
| 2 | 顶层透明规则现在只覆盖 `.app-container / .workspace / .workspace-split / .view-content` |

### 测试

1. Obsidian `Ctrl+R` 刷新即可（未改 @settings）
2. 侧栏应显示与主内容区一致的磨砂玻璃（42% 白底 + blur 40px）
3. 光晕底色透过玻璃可见

### 同步
- ✅ `D:\MyBrain` · ✅ `D:\MyNotes`

---

## v2.2.17 · 根除侧栏暗化 + 光晕永久化（移除开关）

**日期**：2026-07-04

### 问题

① v2.2.16 的侧栏透明修复不生效——文件导航栏和 Notebook Navigator 底色依旧深于主内容区。
② 主题设计理念确定为"光晕始终开启"，不再维护非光晕模式的回退设计。

### 根因分析（侧栏暗化）

Obsidian 默认 CSS 给 `.mod-left-split`（侧栏分割容器）设置了 `background-color: var(--background-secondary)`。只清除了 `.workspace-split` 和 `.workspace-leaf-content` 的背景，**漏掉了 `.mod-left-split` / `.mod-right-split` 这两个中间容器**。

```
光晕渐变 → .mod-left-split (--bg-secondary=12% tint) ← 这层没清！→ .workspace-leaf-content (glass) → 暗化
```

### 修改清单

**A. 侧栏暗化根治**
| # | 改动 |
|---|---|
| 1 | 新增 `body .mod-left-split, body .mod-right-split { background: transparent !important; }` |

**B. 光晕永久化**
| # | 改动 |
|---|---|
| 2 | `@settings` 移除 `gs-wallpaper-on` 开关 |
| 3 | `body.gs-wallpaper-on::before` → `body::before` |
| 4 | `body.gs-wallpaper-on::after` → `body::after` |
| 5 | 删除 `body:not(.gs-wallpaper-on)::before { content: none; }` |
| 6 | 全部容器透明/桌面/移动端规则去前缀 |
| 7 | 光晕变量合并入 `body {}` 基块 |
| 8 | CSS 中 `gs-wallpaper-on` 引用：**40+ → 0** |

### 同步
- ✅ `D:\MyBrain` · ✅ `D:\MyNotes`

---

## v2.2.16 · 侧栏底色统一：消除玻璃层叠暗化

**日期**：2026-07-04

### 问题

光晕模式下，左侧文件导航栏和 Notebook Navigator 背景色明显比主内容区更深，没形成"全界面统一底色连通"的视觉效果。

### 根因

1. 侧栏 `.workspace-leaf.mod-side-dock .workspace-leaf-content` 被覆盖为 `--gs-glass-bg-alt`（80% 玻璃），主内容用 `--gs-glass-bg`（100%）。配色不同 + 各自 `backdrop-filter: blur(40px)` → 色差明显。
2. `.nav-files-container` 继承 12% tint 微底色，再叠 alt-glass blur → 暗化累积。
3. Notebook Navigator 面板容器可能有额外 background。

### 修复

| # | 改动 |
|---|---|
| 1 | 侧栏玻璃从 alt-group 摘出，显式设为 `--gs-glass-bg`（与主内容一致），去 `box-shadow` |
| 2 | 光晕模式 `.nav-files-container` / `.nav-buttons-container` → `background: transparent !important` |
| 3 | Notebook Navigator 全部容器（container/header/panel/footer/calendar） → 全透明，去 blur + shadow |
| 4 | NN 日历单元格（之前已覆盖）继续保护 |

### 同步
- ✅ `D:\MyBrain` · ✅ `D:\MyNotes`

---

## v2.2.15 · 界面融合：全面去线条化与沉浸增强

**日期**：2026-07-04

### 设计目标

整个界面呈现"一整块连续玻璃"的通透感，消除所有硬边界、切割线、阴影带来的板块隔离感，仅靠透明度与模糊度分层。

### 修改清单

| # | 位置 | 改动 |
|---|---|---|
| 1 | 桌面卡片布局 | `padding: 12px → 6px`, `gap: 10px → 2px`, `border-radius: 16px → 6px`, `box-shadow` 从 `var(--gs-glass-shadow)` → `0 1px 3px rgba(0,0,0,0.03)` |
| 2 | 状态栏 | 从浮动气泡（`margin + border-radius + box-shadow`）改为融合底条（`margin:0, border-radius:0, box-shadow:none, background:transparent`） |
| 3 | 滚动条 | `width: 10px → 5px`, `border-radius: 8px → 4px`, `border: 2px → 1px` |
| 4 | 面板分界线 | 新增：`.workspace-split` 的 `border-left/border-top` 全部移除（`none !important`） |
| 5 | 调整手柄 | 新增：`.workspace-split-resize-handle` 默认透明，hover 时才以 20% 透明度微光显现 |
| 6 | 设置项分隔 | 新增：`.setting-item` 去掉 `border-top`，`border-bottom` 改为 `--gs-glass-border-2`（极淡） |
| 7 | 菜单右键 | 新增：`.menu / .dropdown-menu` 无边框，`box-shadow` 从默认 → `0 4px 16px rgba(0,0,0,0.06)` |
| 8 | 菜单分隔线 | 新增：`.menu-separator` 极淡线 `--gs-glass-border-2`，左右留 8px margin |
| 9 | 建议容器 | 新增：`.suggestion-container` 无边框，柔影 `0 4px 18px`，`border-radius: 12px` |
| 10 | 文件导航引导线 | 新增：缩进辅助线改为 `--gs-glass-border-2`（原为 Obsidian 默认的明显线） |
| 11 | 属性面板 | 新增：`.metadata-property` 项间分隔线极淡化 |
| 12 | 命令面板 | 新增：`.prompt / .modal` 柔影 `0 8px 32px`，`border-radius: 12px` |
| 13 | 标签激活态 | `box-shadow: none`（原来有 `inset 0 1px 1px` 的高光边） |
| 14 | 文件列表 hover | `box-shadow: none`（原来 hover 有 `inset 0 1px 1px` 纹路） |
| 15 | 编辑器行号区 | `.cm-gutters` 去掉右边界分隔线 |
| 16 | 深色模式适配 | 调整手柄 hover 26% 透明度，菜单/命令面板暗影加深 |

### 测试方法

1. `Ctrl+R` 刷新后观察整体界面
2. 检查：侧栏与主内容之间无竖线切割，仅靠颜色/模糊自然分区
3. 拖拽面板边界时手柄才可见，松手后消失
4. 设置页各条目之间分隔线极细极淡
5. 右键菜单、自动补全无边框仅靠阴影浮出
6. 滚动条缩到 5px 宽，不滚动时几乎看不见

### 同步
- ✅ `D:\MyBrain\.obsidian\themes\璃墨 Glass Scholar\`
- ✅ `D:\MyNotes\.obsidian\themes\璃墨 Glass Scholar\`

---
> 格式：版本号 / 问题描述 / 根因分析 / 修改清单 / 测试方法 / 影响范围

---

## v2.2.14 · 全面合规审计（基于 kepano/obsidian-skills + obsidian-theme-dev 最佳实践）

**日期**：2026-07-04

### 审计依据

加载了以下技能包进行全面对照审查：
- `@davidvkimball/obsidian-dev-skills` — theme-best-practices / theme-coding-conventions
- `kepano/obsidian-skills` — obsidian-cli / obsidian-markdown 等官方便携
- 主要引用文件：`theme-best-practices.md` (CSS variables)、`theme-coding-conventions.md` (`!important` 使用规范、远程资源禁止)

### 问题与修复

#### 1. 🔴 远程字体加载 — 社区合规阻断项
- **根因**：第 11 行 `@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono...')` 违反社区主题规范（禁止远程资源、离线不可用、隐私风险）
- **修复**：移除 `@import`，改为注释说明。`--font-monospace-theme` 中 `JetBrains Mono` 保留为首选字体名，回退链覆盖 SF Mono → Menlo → Consolas → monospace
- **影响**：已安装 JetBrains Mono 的用户无变化；未安装的用户自动使用本地等宽字体

#### 2. 🟡 `--background-secondary-rgb` 深色模式重复定义（bug）
- **根因**：`body.theme-dark` 块中 `--background-secondary-rgb` 定义了两次 —— 第 257 行正确值 `22, 25, 30`，第 263 行被浅色值 `246, 248, 251` 覆盖
- **修复**：移除第 263 行的错误重复定义
- **影响**：深色模式下涉及 `rgba(var(--background-secondary-rgb), ...)` 计算的颜色现在正确

#### 3. 🟢 重复 `::selection` 规则
- **根因**：同一规则出现在两处（原第 941 行、第 1063 行）
- **修复**：删除第 1063 行的重复定义
- **影响**：无功能变化，纯代码清洁

#### 4. 🟢 空占位注释清理
- **根因**：原第 670-676 行有三个无 CSS 的空注释块（"行内代码"、"属性面板"、"Editing Toolbar"）
- **修复**：删除空注释
- **影响**：无

#### 5. 🟢 新增 `--nav-item-*` CSS 变量
- **根因**：文件列表（File Explorer）样式之前依赖 `.tree-item-self`、`.nav-file-title` 等高特异性选择器。根据 best practices，应使用 Obsidian 原生导航变量 API
- **修复**：在 `body.theme-light` 和 `body.theme-dark` 中新增：
  ```css
  --nav-item-color
  --nav-item-color-hover
  --nav-item-color-active
  --nav-item-background-hover
  --nav-item-background-active
  ```
  浅色/深色分别使用不同的 hover/active 透明度以匹配各自色调
- **影响**：文件列表 hover/active 状态更一致，且用户可用 CSS snippet 通过这些变量自定义

### 最佳实践合规状态（审计后）

| 检查项 | 状态 |
|---|---|
| 无远程字体/资源 | ✅ 已修复 |
| 使用 Obsidian CSS 变量优先于硬编码 | ✅ `--nav-item-*` 已补充 |
| `!important` 最小化 | ⚠️ 玻璃效果场景下不可避免，已标注原因 |
| 主题前缀变量命名 (`--gs-*`) | ✅ 一致 |
| 深/浅双模式覆盖 | ✅ 完整 |
| 移动端适配 | ✅ 有 `@media (max-width: 768px)` 规则 |
| 低特异性选择器优先 | ⚠️ 弹窗/按钮区域因需覆盖 Obsidian 默认样式，选择性使用高特异性 |
| manifest.json 有效 | ✅ 所有必填字段完整 |

### 同步
- ✅ `D:\MyBrain\.obsidian\themes\璃墨 Glass Scholar\`
- ✅ `D:\MyNotes\.obsidian\themes\璃墨 Glass Scholar\`

---

---

## v2.2.13 · Editing Toolbar 颜色选择器样式污染

**日期**：2026-07-04

### 问题描述

Editing Toolbar 插件的颜色选择器（`.x-color-picker-wrapper`）弹出时，**交互面板中间出现一层不该有的悬浮阴影**，整个面板像是被外层玻璃面板"包裹"了一层，破坏了原生气泡外观。

### 根因分析

颜色选择器的子面板（Recent colors、调色板、色值输入框等）被以下主题规则命中：

| 行号（旧） | 规则 | 影响 |
|---|---|---|
| 384-404 | 全局磨砂玻璃面板 | backdrop-filter + box-shadow |
| 751-756 | `.popover` 玻璃效果 | backdrop-filter + 柔和阴影 |
| **795-799** | **交互弹层强模糊** | **`backdrop-filter: blur(36px)` + `box-shadow: 0 18px 52px`** ← 主要污染源 |
| 845-851 | 深色模式 `.popover` 兜底 | 强制深色背景 |

`:not(.x-color-picker-wrapper *)` 排除只针对选择器**内部**的元素，但 `.x-color-picker-wrapper` 的**祖先**和它内部的子面板仍被全局弹层规则命中。

### 修改清单

1. **逐条排除**（5 处）：在以下规则末尾追加 `:not(.x-color-picker-wrapper):not(.x-color-picker-wrapper *)` 双层排除
   - 全局磨砂玻璃规则（workspace-leaf-content / modal / notice）
   - 玻璃面板 alt 规则（tab-header / view-header / status-bar）
   - `.modal-container .modal { border-radius / overflow }`
   - 交互弹层强模糊规则（含 claudian-* 系列）
   - 深色模式 `.popover` 兜底

2. **新增全局重置块**（高优先级兜底）：
   ```css
   .x-color-picker-wrapper,
   .x-color-picker-wrapper *,
   .x-color-picker-wrapper *::before,
   .x-color-picker-wrapper *::after {
     backdrop-filter: none !important;
     -webkit-backdrop-filter: none !important;
     box-shadow: none !important;
     filter: none !important;
     transform: none !important;
     border: none !important;
     border-radius: 0 !important;
     overflow: visible !important;
   }
   .x-color-picker-wrapper {
     background-color: var(--background-primary) !important;
   }
   ```

### 测试方法

1. Obsidian `Ctrl+R` 重新加载 CSS
2. Editing Toolbar 插件 → 选中文字 → 颜色按钮 → 弹出颜色选择器
3. 检查项：
   - ✅ 色块阵列无外层悬浮阴影
   - ✅ Recent colors 区域与主面板融为一体
   - ✅ 浅色 / 深色模式都跟随主题
   - ✅ 关闭/开启背景光晕都不影响颜色选择器外观

### 影响范围

- **仅限**：Editing Toolbar 插件的颜色选择器
- **不影响**：其他弹窗、菜单、popover 的玻璃效果

### 同步

- ✅ `D:\MyBrain\.obsidian\themes\璃墨 Glass Scholar\`
- ✅ `D:\MyNotes\.obsidian\themes\璃墨 Glass Scholar\`

---
</details>
