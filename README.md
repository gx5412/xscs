# MoeFlow 智慧打印 · 小程序 UI 原型（aqua_sky）

60 页微信小程序界面的 HTML 原型 + 预览图，配套一套「aqua_sky」品牌设计系统与 AI 生成提示词。

## 目录结构

```
.
├── aqua_sky/          设计系统（DESIGN.md：色板/圆角/字号/间距）
├── ui.md              60 页 UI 生成提示词（可直接喂给 AI 生成页面）
├── <页面名>/
│   ├── code.html      该页原型的完整 HTML
│   └── screen.png     该页截图
└── .gitattributes     统一 LF
```

## 设计系统（aqua_sky）

| 角色 | 色值 |
|---|---|
| 主色 | `#864D61` |
| 主色浅底 | `#FFD6E2` |
| 次色 | `#3A637C` |
| 紫 | `#6F5092` |
| 薄荷 | `#C1F0DB` |
| 蜜桃 | `#FFD1BA` |
| 暖黄 | `#FFF4BD` |
| 页面底 | `#FFF8F8` |
| 正文 | `#514347` |
| 次要文字 | `#8A7A7F` |
| 描边 | `#F0E4E7` |

- 卡片圆角 `24rpx`｜胶囊 `9999rpx`｜小元素 `12rpx`
- 卡片阴影 `0 8rpx 24rpx rgba(134,77,97,.08)`
- 字号只用 `24 / 28 / 32 / 36 / 44rpx`
- 间距只用 `8 / 16 / 24 / 32 / 48rpx`
- 字体 Plus Jakarta Sans（英文数字）+ 系统中文
- 单位 `rpx`（750 设计稿基准）

## 页面清单

按业务域分组（共 60 页）：

### 打印主流程
`moeflow_smartprint_1` · `moeflow_smartprint_2` · `moeflow_smartprint_3` ·
`moeflow_preview` · `moeflow_web` · `moeflow_copier` · `moeflow_guide` ·
`pdf_moeflow_pdf` · `ppt_moeflow_ppt` · `pdf_moeflow_longimage` · `pdf`

### 拼团
`moeflow_group` · `moeflow_group_order`

### 证照与工具
`ai_1` · `ai_2` · `ai_moeflow_mistake` · `moeflow_photo` · `moeflow_tools`

### 账户与营销
`moeflow_wallet_1` · `moeflow_wallet_2` · `moeflow_refund` · `moeflow_revenue` ·
`moeflow_withdraw` · `moeflow_rebate` · `moeflow_check_in` · `moeflow_newcomer` ·
`moeflow_lucky_draw` · `moeflow_family` · `moeflow_library`

### 商家中心
`moeflow_merchant_1` · `moeflow_merchant_2` · `moeflow_fleet` · `moeflow_pricing` ·
`moeflow_agents` · `moeflow_verify` · `moeflow_stats` · `moeflow_remote` ·
`moeflow_bind` · `moeflow_cms` · `moeflow_join`

### 运营支持
`moeflow_delivery` · `moeflow_support` · `moeflow_status` · `moeflow_group` ·
`moeflow_about` · `moeflow_rules` · `moeflow_1` · `moeflow_2` ·
`moeflow_mistake` · `moeflow_mistake`

### 编号页（`_1` ~ `_13`）
早期批量编号的页面，功能与上面重复，保留作参考。

## 怎么用

**看设计**：直接在浏览器打开任意 `<页面名>/code.html`（纯静态，无依赖）。

**改设计**：改 HTML 里的 Tailwind class 或 `aqua_sky/DESIGN.md` 的 token。

**生成新页面**：打开 `ui.md`，搜索目标页名，复制对应代码块喂给 AI，
生成结果会与这 60 页保持同一套视觉规范。

## 技术备注

- 全部为**静态 HTML**（Tailwind CDN 版式），无构建步骤
- 图标为内联 SVG 或 Material Symbols
- 尺寸按 375×812（iPhone X 基准）设计
