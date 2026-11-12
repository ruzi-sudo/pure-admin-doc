# pure-admin-doc

设计规范文档仓库：`DESIGN.md`（English）与 `DESIGN.zh-CN.md`（中文）整理 [pure-admin/vue-pure-admin](https://github.com/pure-admin/vue-pure-admin) 的视觉与令牌规范。

## 设计系统规范页

`pages/index.html` 是一个可交互的设计规范浏览页，采用常规设计系统文档站的页面结构（顶栏控制 → 示例界面 → 规范总览 → 字号 / 组件 / 阴影 / 动效 / 原则），内容为中后台通用规范。

- Demo：https://ruzi-sudo.github.io/pure-admin-doc/
- 页面源码：[`pages/index.html`](pages/index.html)
- 设计文档：[`DESIGN.zh-CN.md`](DESIGN.zh-CN.md)

### 页面结构

1. 顶栏：浅色 / 深色、可视化 / 源码切换，以及「复制全部规范」
2. Hero：示例后台界面（侧边栏 + 顶栏 + 内容区 + 右侧洞察栏）
3. 规范总览板：颜色、字体、组件小样三栏
4. 字号体系、按钮、卡片、区块标题与 Logo、阴影层级、动效、Do / Don't
5. 源码视图：完整 Markdown 规范，可一键复制

### 配色规则

单一 sky blue 主色 + 中性色，页面内不引入第二套品牌色：

| 角色 | 令牌 | 亮色 | 深色 |
| --- | --- | --- | --- |
| 主色 | `--c-primary` | `#3E9FDD` | `#6FC0EA` |
| 主色实底（白字） | `--c-primary-strong` | `#1F7FB8` | `#1F7FB8` |
| 主色提亮 | `--c-primary-light` | `#7FC6EC` | `#93D3F3` |
| 主色浅底 | `--c-primary-bg` | `#EAF5FC` | `#12293A` |
| 页面底 | `--c-bg` | `#F7FBFE` | `#0E1720` |
| 面板衬底 | `--c-surface-base` | `#F1F8FD` | `#111F2B` |
| 正文文字 | `--c-text` | `#1E2A33` | `#E6F0F7` |

- `--c-danger` / `--c-danger-bg` 仅用于删除等破坏性状态，不作为品牌色大面积使用。
- 主色实底统一使用 `--c-primary-strong`，保证白字对比度 ≥ 4.5:1。

### 交互

- 点击任意规范块 → 复制该模块的 Markdown 片段（悬停显示「⧉ 复制规范」，支持键盘 Enter / Space）
- 「可视化 / 源码」切换，源码视图展示由同一份数据生成的完整规范
- 主题与视图选择保存在 `localStorage`

### 本地预览

```bash
python -m http.server 8080
# 打开 http://localhost:8080/pages/
```

## GitHub Pages

页面由 GitHub Actions 发布：推送 `main` 时把 `pages/` 目录打包为 Pages 站点（见 [`.github/workflows/pages.yml`](.github/workflows/pages.yml)）。

```bash
# 首次启用（Source 需要为 GitHub Actions）
gh api --method POST repos/ruzi-sudo/pure-admin-doc/pages -f build_type=workflow

# 触发并查看部署
gh workflow run pages.yml --repo ruzi-sudo/pure-admin-doc
gh run list --repo ruzi-sudo/pure-admin-doc --workflow pages.yml --limit 3
```
