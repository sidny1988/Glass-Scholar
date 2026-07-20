# 璃墨 Glass Scholar — 发布记录

> Obsidian 磨砂玻璃主题。极简·通透·论文级排版。

---

## v1.0.0 · 初始发布

**日期**：2026-07-04

### 主题特性

| 特性 | 说明 |
|---|---|
| **全透明光晕界面** | 8 种纯 CSS 径向渐变配色（4 深 + 4 浅），色彩穿透所有面板直达眼底 |
| **零边框零边界** | 无硬分割线、无卡片阴影、调整手柄透明化，一整块连续玻璃 |
| **论文级排版** | 衬线/无衬线切换、两端对齐、段首缩进、首字母下沉、可调行距行宽 |
| **深/浅双模式** | `themeScheme: "auto"` 自动跟随系统 |
| **Style Settings 支持** | 光晕配色、模糊度、强度、字体、排版等 10 项可调 |
| **自动隐藏侧栏** | 鼠标左边缘触发展开（可选） |
| **PDF 导出** | `@media print` 块确保导出正常白纸报告 |
| **移动端适配** | `@media (max-width: 768px)` 规则 |
| **插件友好** | 排除 Editing Toolbar 颜色选择器、Claudian、Notebook Navigator 样式污染 |
| **社区合规** | 无远程字体、无外部资源依赖 |

### 技术架构

- **文件名**：`theme.css`（1340+ 行） + `manifest.json`
- **变量前缀**：`--gs-*`（Glass Scholar）
- **核心机制**：
  - `body::before`：`position: fixed` 径向渐变光晕层
  - `body::after`：极淡纸纹噪点层
  - `--background-*: transparent`：全界面透明穿透
  - `backdrop-filter: blur(12px)`：面板微磨砂触感
  - `--gs-glass-opacity: 0`：玻璃白底归零，光晕最大化可见

### CSS 规则统计

| 指标 | 数值 |
|---|---|
| 总行数 | ~1,350 |
| `!important` 使用 | ~195 处（玻璃透明场景必需） |
| Style Settings 配置项 | 11 项 |
| 光晕配色 | 8 种 |
| CSS 自定义变量 | 80+ 个 |

### 开发历程

本主题历经 20+ 次迭代（v2.2.12 → v1.0.0），在玻璃透明度、界面融合、PDF 导出三个方向上反复打磨。完整开发记录见 `CHANGELOG.md`。

### 依赖

- Obsidian ≥ 1.0.0
- 推荐安装：Style Settings 插件（可选配置）
- 推荐安装：Translucent Background 插件（配合光晕效果更佳）
