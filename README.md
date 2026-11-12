# pure-admin-doc

设计规范文档仓库：`DESIGN.md`（English）与 `DESIGN.zh-CN.md`（中文）整理 [pure-admin/vue-pure-admin](https://github.com/pure-admin/vue-pure-admin) 的视觉与令牌规范。

## PureAdmin 设计系统规范页

`pages/index.html` 按 vue-pure-admin 的界面规则组织的设计规范示例：顶栏 48px、标签栏 34px、侧边栏 210px，支持多主题色与明暗模式，点击任意模块可复制 Markdown 规范。

- Demo：https://ruzi-sudo.github.io/pure-admin-doc/
- 页面源码：[`pages/index.html`](pages/index.html)
- 设计文档：[`DESIGN.zh-CN.md`](DESIGN.zh-CN.md)

### 页面结构

1. 顶栏：品牌、横向导航、搜索 / 通知 / 用户 / 项目配置
2. 标签栏：首页 / 视觉规范，右侧显示当前主题色
3. 内容区：面包屑 + 页面标题 + 规范操作（可视化 / 源码 / 主题配置 / 复制全部）
4. 仪表盘示例：4 个统计卡（含迷你折线）、分组柱状图、覆盖率进度、规范明细表、最新动态
5. 规范分区：颜色（8 套主题色 + 语义令牌）、字体（6 档字号）、组件（按钮 / 标签 / 输入框 / 分页 / 卡片 / Logo）、阴影层级、动效、设计原则
6. 源码视图：完整 Markdown 规范，可一键复制

### 多主题配置（右上角齿轮 / 主题配置）

| 配置项 | 可选值 |
| --- | --- |
| 整体风格 | 亮色 / 深色 / 自动（跟随系统） |
| 主题色 | 默认蓝 `#409EFF`、酱紫 `#693AC9`、粉色 `#D84493`、深红 `#E13C39`、火山 `#E85F33`、明青 `#59BFC1`、极光绿 `#60AC80`、石墨蓝 `#4C6A92` |
| 导航模式 | 横向 / 纵向（210px 侧边栏） |
| 界面效果 | 灰色模式（grayscale）、色弱模式（invert） |

配置项保存在 `localStorage`，刷新后保留；也可从「颜色规范」直接点击色卡切换主题色。

### 令牌约定

- 主题色由 `--theme-color` 驱动，派生 `--c-primary`、`--c-primary-strong`（实底按钮，保证白字对比度）、`--c-primary-bg`、`--c-primary-line`、`--c-primary-light`
- 语义状态色沿用 Element Plus：`--c-success` / `--c-warning` / `--c-danger` / `--c-info`，只表达状态
- 布局尺寸：顶栏 `48px`、标签栏 `34px`、侧边栏 `210px`、圆角 `4px / 6px`
- 字号：20 / 18 / 16 / 14 / 13 / 12，行高 1.5
- 动效：标准 300ms、快速 120ms，支持 `prefers-reduced-motion`

### 交互

- 点击任意规范块 → 复制该模块 Markdown 片段（悬停「⧉ 复制规范」，支持 Enter / Space）
- 侧边导航会随滚动高亮当前分区
- 主题与视图切换即时生效，无刷新

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
