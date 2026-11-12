---
version: "alpha"
name: "vue-pure-admin 设计规范（中文版）"
description: "面向 Vue 3 + Vite + Element Plus 后台管理系统的视觉设计规范，基于现有实现整理，并保留英文版 DESIGN.md 作为原始版本。"
owner: "pure-admin"
project: "vue-pure-admin"
repo: "https://github.com/pure-admin/vue-pure-admin"
status: "active"
language: "zh-CN"
---

## 概览

本文档是 `pure-admin/vue-pure-admin` 的中文设计规范说明，面向开发者、设计师与 AI 编码助手，帮助在项目中保持统一的 UI 语义、视觉层级和交互习惯。

它注重的是“可落地的规范”，而不是单纯的审美表达。也就是说：

- 颜色、间距、字体、状态要清晰
- 主题切换要可靠
- 新增页面和组件要能沿用既有规则
- 设计系统要兼顾人类和 AI 可读性

目标是让项目在不同页面、组件、布局之间保持协调，而不是每个模块都按照自己的风格自由发挥。

## 设计原则

1. 优先使用语义化颜色，不要散落大量硬编码值。
2. 优先使用 CSS 变量 / 设计令牌，不直接到处写死色号。
3. 主色用于关键动作、选中状态和视觉强调，不用于所有场景。
4. 深浅主题都需要有足够对比度，确保可读性。
5. 页面中字号、间距、圆角、分隔线要统一，避免视觉杂乱。
6. 能够跟随主题切换的内容，尽量走变量层，不要写死。
7. 文档要清晰结构化，便于团队协作与 AI 生成内容时遵循一致规则。

## 颜色系统

### 现有项目里的主题能力

项目中已经具备以下能力：

- 明亮 / 暗色模式
- 菜单主题切换
- Element Plus 主色切换
- 多套导航主题色
- 主题配置持久化存储

这些能力主要来自：

- `public/platform-config.json`
- `src/style/theme.scss`
- `src/style/dark.scss`
- `src/layout/hooks/useDataThemeChange.ts`
- `src/store/modules/epTheme.ts`

当前布局中的颜色策略并不是“每个组件都用独立随机色”，而是围绕统一语义来展开。

### 主题色举例

项目里已经存在多套菜单色方案，常见包括：

| 主题名 | 说明 | 主色示例 |
| --- | --- | --- |
| `light` | 默认亮色 | `#4091f7` |
| `default` | 默认蓝色系 | `#4091f7` |
| `saucePurple` | 紫色系 | `#693ac9` |
| `pink` | 粉色系 | `#d84493` |
| `dusk` | 暗红系 | `#e13c39` |
| `volcano` | 橙红系 | `#e85f33` |
| `mingQing` | 青绿系 | `#59bfc1` |
| `auroraGreen` | 绿色系 | `#60ac80` |

这些是“主题颜色”与“菜单风格”的表达，适用于导航和主题切换，但并不意味着所有 UI 都用同一个主色。真正的设计语义仍然应体现在组件状态、状态色和文本层面。

### 语义颜色令牌

```yaml
colors:
  primary: "#409EFF"
  menuAccent: "#4091f7"
  pageBackground: "#f0f2f5"
  surface: "#ffffff"
  textPrimary: "var(--el-text-color-primary)"
  textRegular: "var(--el-text-color-regular)"
  border: "var(--pure-border-color)"
  success: "var(--el-color-success)"
  warning: "var(--el-color-warning)"
  danger: "var(--el-color-danger)"
  info: "var(--el-color-info)"
```

这些语义令牌代表：

- 主强调色
- 页面背景
- 卡片和表层颜色
- 文本主色/次色
- 边框与分隔线
- 成功、警告、危险、信息等状态色

### 颜色使用规则

- 使用 `var(--el-color-primary)` 作为主强调色，适用于：主按钮、选中项、关键导航、筛选态、状态高亮。
- 使用 `var(--el-color-success)`、`var(--el-color-warning)`、`var(--el-color-danger)`、`var(--el-color-info)` 表达状态语义。
- 不要用主色去表示所有不同的状态，否则会让界面丢失语义。
- 使用 `var(--el-text-color-primary)` 和 `var(--el-text-color-regular)` 处理文本颜色。
- 菜单相关颜色统一使用 `--pure-theme-*` 系列变量。
- 使用 `--pure-border-color` 做常见边框和分隔线。
- 在深色模式下，要持续确认文本、图标和状态颜色的可读性。
- 能跟随主题变化的颜色，优先走变量层；不需要变化的颜色才考虑定值。

### 动态色与固定色

项目里需区分两种颜色：

- 动态色：跟随主题切换，如 `--el-color-primary`、菜单变量。
- 固定色：为特定场景强制固化的颜色，如某种特殊品牌色或演示色。

固定色需要说明理由，并且在亮/暗模式下都需要验证可读性与一致性。

## 字体和字号

### 字体家族

项目默认字体来自 `src/style/reset.scss`：

```css
"Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB",
"Microsoft YaHei", "微软雅黑", Arial, sans-serif
```

这种字体栈适合中文后台管理系统和中英混排场景。

### 推荐字号体系

```yaml
typography:
  fontFamily: '"Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", "微软雅黑", Arial, sans-serif'
  lineHeight: 1.5
  fontSize:
    caption: "12px"
    compact: "13px"
    body: "14px"
    bodyLarge: "16px"
    sectionTitle: "18px"
    pageTitle: "20px"
  fontWeight:
    regular: 400
    medium: 500
    semibold: 600
    bold: 700
```

### 字号使用规则

- `14px`：适用于菜单、标签、二级信息、常规控件。
- `16px`：适用于正文、表单内容等日常信息。
- `18px ~ 20px`：适用于页面标题和重点内容。
- `12px ~ 13px`：适用于辅助信息、说明和注释。
- 多个界面之间尽量保持一致，不要随机给一个区块加超大或超小字号。
- 如果可能，尽量使用一套层级化字号，而不是单个组件里自由定义。

## 布局与间距

### 现有布局尺寸

在 `src/style/sidebar.scss` 中，项目已经定义了关键尺寸：

| 元素 | 当前值 |
| --- | ---: |
| 纵向展开侧边栏 | `210px` |
| 收起侧边栏 | `54px` |
| 顶部导航高度 | `48px` |
| 菜单项高度 | `50px` |
| 标签栏高度 | `34px` |
| 全局行高 | `1.5` |

这部分内容是基础结构，不要随意调整，必须在响应式、移动端和收起状态下一起检查。

### 推荐间距系统

```yaml
spacing:
  1: "4px"
  2: "8px"
  3: "12px"
  4: "16px"
  5: "20px"
  6: "24px"
  8: "32px"
```

### 布局原则

- 页面中卡片、表格、表单、分组之间要保持稳定节奏。
- 保持对齐：字段、标题、控件和表格列应对齐。
- 避免“为了填补空白而堆很多卡片”的局面。
- 侧边栏、顶部栏和内容区应在不同布局模式下都保持可用。
- 不要只改一处尺寸而不检查移动端和收起状态。

## 阴影与层级

项目整体偏稳重，阴影不宜过强，更多用于轻微的层级提示。

```yaml
elevation:
  none: "none"
  subtle: "0 1px 2px rgb(0 0 0 / 8%)"
  popover: "0 2px 8px rgb(0 0 0 / 15%)"
```

原则：

- 优先用轻微阴影与边框替代强烈 3D 效果。
- 深色模式下要重新验证阴影是否过强或模糊。
- 复杂阴影不适合后台管理场景，应保持简洁和清楚。

## 圆角与形状

项目采用适度圆润的圆角，不做过分“量感型”的圆角设计。

```yaml
rounded:
  small: "3px"
  default: "4px"
  medium: "6px"
  pill: "9999px"
```

规则：

- 小控件和标签适合 `small` / `default`。
- 表单、卡片一般用 `default` / `medium`。
- 只在明显的胶囊型控件中使用 `pill`。
- 同一页面里不要混用太多风格完全不同的圆角。

## 组件视觉规范

### 导航菜单

菜单样式与主题变量绑定，重点参考：

- `--pure-theme-menu-bg`
- `--pure-theme-menu-text`
- `--pure-theme-menu-hover`
- `--pure-theme-sub-menu-bg`
- `--pure-theme-sub-menu-active-text`
- `--pure-theme-menu-title-hover`
- `--pure-theme-menu-active-before`

菜单样式位于 `src/style/sidebar.scss`，基本要求：

- 当前导航项比非当前项更突出。
- 悬停态应轻微变化，不要产生大幅跳动。
- 页内主色与菜单主题色应保持统一语义。

### 标签页 / Tabs

标签栏样式在 `src/layout/components/lay-tag/index.scss`。选中态通常使用：

- `var(--el-color-primary)`
- `var(--el-text-color-primary)`

规则：

- 激活状态应明显但不刺眼。
- 关闭按钮要保持可见和可点。
- 深浅模式下都需要保证对比度。

### 按钮

- 主按钮：用于关键动作，例如保存、提交、创建、确认。
- 次按钮：用于辅助动作，例如取消、后退、查看详情。
- 危险按钮：用于删除、清空、移除等高风险操作。
- 不要在一个小区域里同时放多个争夺注意力的主按钮。

### 表单

- 标签、输入框、提示信息和校验信息应分层清晰。
- 焦点态使用主色强调，不能到处使用不同颜色做焦点。
- 禁用状态应明显且可读。
- 表单不需要过度装饰，保持结构清晰为主。

### 卡片、表格、弹框

- 通过表层色和边框进行稳定结构表达。
- 表格要保持合适密度，不要压缩到难以阅读。
- 弹框和层叠组件应有清晰层级，不应出现过强的阴影破坏信息结构。
- 深色模式下需要验证卡片和表格的可读性。

## 动效

```yaml
motion:
  standardDuration: "0.3s"
  quickDuration: "0.12s"
  easing: "ease"
```

项目整体偏简洁、功能导向，动效应服务于反馈与状态变化，而不是视觉炫技。

建议：

- 选中、悬停、展开和收起可用轻微过渡。
- 动画不宜过长，也不要太强烈。
- 新交互尽量兼容 `prefers-reduced-motion` 这种减弱动画方案。

## 主题模式与配置

默认平台配置来自：

- `public/platform-config.json`

相关逻辑来自：

- `src/layout/hooks/useDataThemeChange.ts`
- `src/store/modules/epTheme.ts`
- `src/layout/components/lay-setting/index.vue`

新增或修改主题时，建议遵循：

1. 在 `src/style/theme.scss` 里定义主题色。
2. 在 `useDataThemeChange.ts` 中同步主题名和颜色值。
3. 检查 `--el-color-primary` 与菜单变量是否同步更新。
4. 验证亮/暗模式及不同布局下结果是否正常。
5. 检查是否有硬编码色值残留。

## 可访问性

- 文字必须具备足够对比度，尤其在深色模式中。
- 不能仅靠颜色区分状态；要结合文本、图标、结构。
- 键盘焦点必须明显。
- 悬停态不能破坏可读性。
- 触控区域需要兼顾移动端可用性。

## Do / Don’t

### Do

- 使用语义化变量和令牌。
- 让页面信息层级稳定。
- 保持主色用于关键动作。
- 遵循统一的字号、间距、圆角与状态色。
- 检查深浅模式和移动端布局。

### Don’t

- 不要到处硬编码主色。
- 不要让状态色失去语义。
- 不要在一个页面中混入过多随机颜色。
- 不要忽略对比度测试。
- 不要在主题切换场景下使用固定值。

## 实施参考

- `public/platform-config.json`
- `src/style/index.scss`
- `src/style/theme.scss`
- `src/style/dark.scss`
- `src/style/reset.scss`
- `src/style/sidebar.scss`
- `src/style/element-plus.scss`
- `src/layout/hooks/useDataThemeChange.ts`
- `src/store/modules/epTheme.ts`
- `src/layout/components/lay-setting/index.vue`
- `src/layout/components/lay-tag/index.scss`

## 变更检查清单

每次调整视觉风格前，检查：

- [ ] 颜色是否具有明确的语义。
- [ ] 亮暗模式下是否可读。
- [ ] 主题切换是否保持正常。
- [ ] 字号和间距是否符合层级。
- [ ] 纵向、横向、混合布局和移动端是否正常。
- [ ] 是否有新的硬编码颜色或随机样式被遗漏。
- [ ] 文档是否需要同步更新。

## 总结

这个项目的视觉规范偏向于“实用型后台系统设计”，它的关键目标是：

- 有清晰的信息层级
- 有明确的状态语义
- 能灵活支持主题切换
- 适合团队协作和 AI 生成组件

最核心的规则可以概括为：

- 设计元素由令牌驱动
- 主题、状态和组件行为由变量统一控制
- 只有确实需要固定不变的视觉场景才允许硬编码

这使它既适用于团队维护，也适用于 AI 辅助开发时保持一致风格。
