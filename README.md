# Glass Scholar · 璃墨

[![GitHub release](https://img.shields.io/github/v/release/sidny1988/glass-scholar)](https://github.com/sidny1988/glass-scholar/releases)

**A frosted-glass Obsidian theme** — fully transparent glow interface with minimal design and academic-grade typography.

> Apple Vision Pro-inspired transparency: vibrant CSS radial-gradient glows shine through every panel. No borders, no boundaries, no isolation. One continuous sheet of glass.

**Obsidian 磨砂玻璃主题** — 全透明光晕界面 · 极简 · 论文级排版。

> 苹果 Vision Pro 式的通透美学：色彩光晕穿透所有面板直达眼底，无边框、无边界、无隔离。一整块连续玻璃。

![Light mode](https://via.placeholder.com/800x450/f0f4ff/4a6fa5?text=璃墨+Glass+Scholar+-+Light+Mode)
![Dark mode](https://via.placeholder.com/800x450/1a1d23/7aa2d6?text=璃墨+Glass+Scholar+-+Dark+Mode)

---

## 特性

- 🌈 **全透明光晕界面** — 8 种纯 CSS 径向渐变配色，色彩穿透所有面板
- 🪟 **微磨砂触感** — `backdrop-filter: blur` 面板，仅保留模糊不挡底色
- 🚫 **零边框零边界** — 无硬分割线、无卡片阴影、调整手柄透明化
- 📖 **论文级排版** — 衬线字体、两端对齐、段首缩进、首字母下沉、学术三线表
- 🌓 **深/浅双模式** — 自动跟随系统（`themeScheme: "auto"`）
- 📱 **移动端适配** — 手机/平板自动调整间距与字号
- 📄 **PDF 导出** — 专用 `@media print` 规则确保导出正常
- 🔌 **Style Settings** — 11 项可视化配置，无需编辑 CSS

---

## 安装

### 从 Obsidian 社区主题市场安装（推荐）

1. 打开 Obsidian → **设置 → 外观 → 主题 → 浏览**
2. 搜索 **"璃墨 Glass Scholar"**
3. 点击安装并启用

### 手动安装

1. 从 [Releases](https://github.com/sidny1988/glass-scholar/releases) 下载 `theme.css` 和 `manifest.json`
2. 放入 `<你的仓库>/.obsidian/themes/璃墨 Glass Scholar/`
3. Obsidian → 设置 → 外观 → 主题 → 选择「璃墨 Glass Scholar」

---

## 配置（需安装 Style Settings 插件）

| 分类 | 选项 | 默认值 |
|---|---|---|
| **背景光晕** | 光晕配色（8 种） | 晨曦蓝 |
| | 光晕模糊 (px) | 40 |
| | 光晕强度 | 1.0 |
| **界面行为** | 自动隐藏侧边工具栏 | 关闭 |
| **论文排版** | 正文使用衬线字体 | 关闭 |
| | 两端对齐 | 开启 |
| | 段首 2em 缩进 | 开启 |
| | 首段首字母下沉 | 关闭 |
| | 行距 | 1.85 |
| | 正文最大宽度 (ch) | 72 |

---

## 推荐搭配

- **[Style Settings](https://github.com/mgmeyers/obsidian-style-settings)** — 可视化配置所有选项
- **[Translucent Background](https://github.com/mProjectsCode/obsidian-translucent-background)** — 窗口级透明增强

---

## 字体说明

主题使用系统本地字体链，**无远程加载依赖**：

- **界面**：SF Pro → PingFang SC → 微软雅黑 → 系统 UI
- **正文**：系统默认（衬线模式切换为 Source Serif 4 → Noto Serif SC → 宋体）
- **代码**：JetBrains Mono → SF Mono → Menlo → Consolas

---

## 兼容性

- Obsidian ≥ 1.0.0
- 桌面端（Windows / macOS / Linux）最佳体验
- 移动端（iOS / Android）已适配

---

## License

MIT © 2026 Glass Scholar
