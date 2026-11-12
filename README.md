# pure-admin-doc

设计规范文档仓库：`DESIGN.md`（English）与 `DESIGN.zh-CN.md`（中文）整理 [pure-admin/vue-pure-admin](https://github.com/pure-admin/vue-pure-admin) 的视觉与令牌规范。

## Flip7 设计系统 · 视觉规范页

`pages/index.html` 是一个可交互的设计规范浏览页，参考 [Flip7 设计系统](https://designmd.ai/yiujc/flip7-card-game) 的结构，并遵循 `DESIGN.zh-CN.md` 的字号、间距、圆角、阴影与动效档位。

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

整页只使用 Flip7 一套色系，不混入其它主色：

| 角色 | 令牌 | 亮色 | 深色 |
| --- | --- | --- | --- |
| 主色 | `--c-primary` | `#2BA8A2` | `#3CC4BD` |
| 主色实底 | `--c-primary-strong` | `#1E8C86` | `#1E8C86` |
| 强调 / 奖励 | `--c-gold` | `#FFD23F` | `#FFD23F` |
| 危险 / BOOM | `--c-coral` | `#EF6C4A` | `#FF8A6A` |
| 信息 / Flip7 | `--c-sky` | `#5DADE2` | `#6FB7E8` |
| 页面底色 | `--c-bg` | `#EFF8F7` | `#0F1A19` |

`--c-success`、`--c-warning`、`--c-danger`、`--c-info` 分别映射到青绿、金色、珊瑚、天空蓝，不额外引入第五种色相。

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

- 启用 Pages（Source 需要为 `GitHub Actions`）：

  ```bash
  gh api --method POST repos/ruzi-sudo/pure-admin-doc/pages -f build_type=workflow
  ```

- 触发并查看部署：

  ```bash
  gh workflow run pages.yml --repo ruzi-sudo/pure-admin-doc
  gh run list --repo ruzi-sudo/pure-admin-doc --workflow pages.yml --limit 3
  ```
