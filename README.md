# pure-admin-doc

Design system and visual style guidance for [pure-admin/vue-pure-admin](https://github.com/pure-admin/vue-pure-admin), formatted as `DESIGN.md` (English) and `DESIGN.zh-CN.md` (中文).

## Flip 7 score party

A static, single-file scoring page inspired by the Flip7 card-game design system, styled with the tokens documented in `DESIGN.zh-CN.md` / `DESIGN.md`.

- Demo page: [`pages/index.html`](pages/index.html)
- Design reference: [`DESIGN.zh-CN.md`](DESIGN.zh-CN.md)
- Brand reference: [Flip7 card game design system](https://designmd.ai/yiujc/flip7-card-game)

### Features

- 2–8 位玩家的派对记分（数字牌、加成牌、×2、Flip7 +15、BOOM 归零）
- 金色 / 银色 / 铜色领奖台排行榜，达到 200 分触发庆祝动画
- 亮色 / 暗色主题切换，跟随系统偏好并记忆选择
- 分数与未结算草稿保存在 `localStorage`，刷新不丢失
- 语义化令牌、键盘焦点、`prefers-reduced-motion` 支持

### Local preview

```bash
python -m http.server 8080
# 打开 http://localhost:8080/pages/
```

## GitHub Pages

页面由 GitHub Actions 发布：推送 `main` 时把 `pages/` 目录打包为 Pages 站点（见 [`.github/workflows/pages.yml`](.github/workflows/pages.yml)）。

- Demo: https://ruzi-sudo.github.io/pure-admin-doc/
- 启用 Pages（Source 需要为 `GitHub Actions`）：

  ```bash
  gh api --method POST repos/ruzi-sudo/pure-admin-doc/pages -f build_type=workflow
  ```

- 触发并查看部署：

  ```bash
  gh workflow run pages.yml --repo ruzi-sudo/pure-admin-doc
  gh run list --repo ruzi-sudo/pure-admin-doc --workflow pages.yml --limit 3
  ```
