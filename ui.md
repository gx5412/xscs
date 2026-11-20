# 小程序 UI 生成提示词（去商城 · 60 页）

> 用法：复制某一页的代码块，直接喂给 AI 生成页面；或使用 `ui_prompts/` 目录下的单页 txt。

> 所有提示词共用同一套设计规范（见下），保证生成结果风格一致。


## 设计规范（全局）

```
【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）
```


## 硬性要求（全局）

```
【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 页面总览

| # | 分包 | 优先级 | 路径 | 标题 | 后台功能 |
|---|---|---|---|---|---|
| 1 | 主包 | P0 | `pages/index/index` | 首页 | 广告位 / 门店列表 / 设备列表 / 站内通知 |
| 2 | print | P0 | `pages/print/home` | 打印入口 | 文件管理 / 网盘 / U盘 |
| 3 | print | P0 | `pages/print/upload` | 上传与格式检查 | 文件管理（file 18 接口） |
| 4 | print | P0 | `pages/print/config` | 打印参数配置 | 打印订单（计价） |
| 5 | print | P0 | `pages/print/preview` | 纸张预览 | 文件管理 / 打印订单 |
| 6 | print | P0 | `pages/print/payment` | 履约与支付 | 打印订单 / 支付 |
| 7 | print | P0 | `pages/print/success` | 支付成功 | 打印订单 / 设备状态 |
| 8 | print | P0 | `pages/print/copier` | 复印 / 扫描 | 复印设置（copier 6 接口） |
| 9 | print | P2 | `pages/print/guide` | 打印指南 | 资料库 / 帮助 |
| 10 | group | P0 | `pages/group/lobby` | 拼团大厅 | 拼团活动（19 接口） |
| 11 | group | P0 | `pages/group/detail` | 拼团详情 | 拼团活动 |
| 12 | group | P0 | `pages/group/order` | 拼团下单 | 拼团订单 |
| 13 | group | P1 | `pages/group/mine` | 我的拼团 | 拼团订单 |
| 14 | tools | P1 | `pages/tools/home` | 工具箱 | AI 工具（aitools）+ tool 82 接口 |
| 15 | tools | P0 | `pages/tools/idphoto` | 证件照制作 | AI 工具 / tool |
| 16 | tools | P1 | `pages/tools/cert-print` | 证件复印 | 证件打印（原版 printhu/printying/printjia） |
| 17 | tools | P0 | `pages/tools/photo-print` | 照片冲印 | tool / 打印订单 |
| 18 | tools | P0 | `pages/tools/scan` | 拍照扫描 | tool / copier |
| 19 | tools | P0 | `pages/tools/mistake` | 错题本 | AI 工具 |
| 20 | tools | P1 | `pages/tools/topdf` | 转 PDF | tool 82 接口 |
| 21 | tools | P2 | `pages/tools/longimg` | 长图转 PDF | tool |
| 22 | tools | P2 | `pages/tools/ppt` | PPT 打印 | ppt 2 接口 |
| 23 | 主包 | P0 | `pages/order/list` | 订单中心 | 打印订单 |
| 24 | 主包 | P0 | `pages/order/detail` | 取件码与订单详情 | 打印订单 / 退款 |
| 25 | 主包 | P0 | `pages/user/mine` | 我的 | 打印用户 / 会员等级 / 卡券 |
| 26 | 主包 | P0 | `pages/auth/login` | 登录 | 微信登录（wechat 4 接口） |
| 27 | 主包 | P0 | `pages/notice/list` | 消息中心 | 站内通知（notice 5 接口） |
| 28 | 主包 | P1 | `pages/search/search` | 搜索 | 资料库 / 门店 / 订单 |
| 29 | 主包 | P0 | `pages/web/web` | 网页容器 | — |
| 30 | 主包 | P0 | `pages/rules/terms` | 服务协议与隐私 | 平台参数 |
| 31 | 主包 | P2 | `pages/about/about` | 关于我们 | about |
| 32 | 主包 | P1 | `pages/result/result` | 通用结果页 | — |
| 33 | 主包 | P1 | `pages/delivery/progress` | 配送进度 | 配送管理（delivery 8 接口） |
| 34 | 主包 | P1 | `pages/workorder/list` | 售后工单 | 工单处理（workorder 5 接口） |
| 35 | account | P0 | `pages/account/member` | 会员与套餐 | 会员等级（level 4 接口） |
| 36 | account | P0 | `pages/account/coupon` | 券中心 | 卡券管理（card 6 接口） |
| 37 | account | P1 | `pages/account/wallet` | 钱包明细 | 打印用户（user 8 接口） |
| 38 | account | P1 | `pages/account/recharge` | 余额充值 | 充值（chongfen 3 接口） |
| 39 | account | P0 | `pages/account/invite` | 邀请好友 | 邀请返利（invite 6 接口） |
| 40 | account | P2 | `pages/account/raffle` | 抽奖 | 抽奖活动（raffle 8 接口） |
| 41 | account | P2 | `pages/account/sign` | 每日签到 | 签到 / 积分 |
| 42 | account | P1 | `pages/account/landing` | 新人礼包 | 卡券 / 营销 |
| 43 | account | P2 | `pages/account/rebate` | 返利明细 | 返利管理（fanli 4 接口） |
| 44 | account | P2 | `pages/account/family` | 家庭版 | 家庭版权限 |
| 45 | merchant | P0 | `pages/merchant/dashboard` | 经营看板 | 数据看板（stats 23 接口） |
| 46 | merchant | P0 | `pages/merchant/orders` | 商家订单工作台 | 打印订单 |
| 47 | merchant | P0 | `pages/merchant/devices` | 设备管理 | 设备列表 / 远程维护（machine 23 接口） |
| 48 | merchant | P0 | `pages/merchant/verify` | 取件核销（PDA） | 打印订单 |
| 49 | merchant | P1 | `pages/merchant/pricing` | 价格规则 | 平台参数 / 计价 |
| 50 | merchant | P1 | `pages/merchant/agents` | 渠道代理 | 渠道代理 / 代理收入（profit_sharing 9 接口） |
| 51 | merchant | P1 | `pages/merchant/revenue` | 收益明细 | 代理收入 / 营收统计 |
| 52 | merchant | P0 | `pages/merchant/refund` | 退款处理 | 退款审核（refund 5 接口） |
| 53 | merchant | P1 | `pages/merchant/withdraw` | 提现 | 提现审核（finance 4 接口） |
| 54 | merchant | P1 | `pages/merchant/join` | 打印机入驻 | 门店入驻（platform_apply 5 接口） |
| 55 | merchant | P0 | `pages/merchant/store` | 门店管理 | 门店列表 / 编辑 |
| 56 | merchant | P1 | `pages/merchant/bind` | 设备绑定 | 设备列表 |
| 57 | merchant | P1 | `pages/merchant/cms` | 内容管理 | 资料库（word 11 接口） |
| 58 | merchant | P2 | `pages/merchant/remoute` | 远程维护 | 远程维护（machine 23 接口） |
| 59 | 主包 | P0 | `pages/library/shelf` | 资料库与模板 | 资料库（word 11 接口） |
| 60 | merchant | P2 | `pages/admin/printpage` | 纸张统计 | 设备/统计（stats 23） |


---

# 主包（4 个 tab）（14 页）


## 首页 · `pages/index/index`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 广告位 / 门店列表 / 设备列表 / 站内通知 |
| 页面目标 | 一屏看清附近可用门店与设备、当前活动、以及最常用入口 |
| 接口 | `/wxapp/stores`<br>`/wxapp/machines/online`<br>`/wxapp/ads`<br>`/wxapp/pugin/notice/messages`<br>`/wxapp/order/list` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】首页
【路径】pages/index/index
【后台功能】广告位 / 门店列表 / 设备列表 / 站内通知
【页面目标】一屏看清附近可用门店与设备、当前活动、以及最常用入口

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 顶部搜索条（门店/文件搜索入口，圆角 9999rpx，左侧放大镜图标）
2. 轮播广告位（后台广告位配置，16:9，圆角 24rpx，下方圆点指示器）
3. 快捷入口四宫格（打 印 / 复 印 / 证件照 / 工具箱，图标+文字，白底卡片）
4. 附近门店列表（门店名 + 距离 + 空闲设备数 + 排队时长，右侧箭头）
5. 最近订单卡片（有进行中订单时显示：订单号后 4 位 + 状态徽章 + 取件码）
6. 公告条（后台站内通知，单行滚动）

【需要展示/绑定的字段】
· storeTitle ｜ distanceM ｜ idleMachines ｜ queueSeconds ｜ adImage ｜ noticeText ｜ recentOrder

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/stores
· /wxapp/machines/online
· /wxapp/ads
· /wxapp/pugin/notice/messages
· /wxapp/order/list

【交互跳转】
· 点击门店 → 打印入口（带 storeid）
· 点击快捷入口 → print/tools 对应页
· 点击最近订单 → order/detail

【必须覆盖的状态】
· 无门店 ｜ 定位被拒 ｜ 加载中 ｜ 无进行中订单

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 订单中心 · `pages/order/list`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 打印订单 |
| 页面目标 | 按状态查看全部订单 |
| 接口 | `/wxapp/order/list`<br>`/wxapp/order/detail` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】订单中心
【路径】pages/order/list
【后台功能】打印订单
【页面目标】按状态查看全部订单

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 状态 tab（全部 / 待取件 / 已完成 / 退款）
2. 订单卡片（文件缩略图 + 标题 + 页数/金额 + 状态徽章 + 操作按钮）
3. 操作区（待取件：取件码；已完成：再次打印 / 开发票）
4. 下拉刷新 + 上拉加载

【需要展示/绑定的字段】
· orderNo ｜ thumb ｜ title ｜ pages ｜ amount ｜ status ｜ pickupCode

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/order/list
· /wxapp/order/detail

【交互跳转】
· 点击 → order/detail
· 再次打印 → print/config

【必须覆盖的状态】
· 空订单 ｜ 加载更多 ｜ 网络失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 取件码与订单详情 · `pages/order/detail`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 打印订单 / 退款 |
| 页面目标 | 一页看清取件码、进度、参数、退款入口 |
| 接口 | `/wxapp/order/detail`<br>`/wxapp/order/refund`<br>`/wxapp/machines/online` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】取件码与订单详情
【路径】pages/order/detail
【后台功能】打印订单 / 退款
【页面目标】一页看清取件码、进度、参数、退款入口

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 状态头（大状态文案 + 预计完成时间）
2. 取件码卡（4 位大字 + 二维码 + 复制）
3. 出纸进度（设备 + X/Y 页 + 进度条）
4. 参数摘要（文件/页数/单双面/色彩/装订）
5. 金额明细（单价 × 页数 + 优惠 + 实付）
6. 操作区（联系客服 / 申请退款 / 再次打印）

【需要展示/绑定的字段】
· orderNo ｜ status ｜ pickupCode ｜ machine ｜ progress ｜ params ｜ amount ｜ refundable

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/order/detail
· /wxapp/order/refund
· /wxapp/machines/online

【交互跳转】
· 申请退款 → 退款弹层
· 再次打印 → print/config

【必须覆盖的状态】
· 设备离线 ｜ 已退款项 ｜ 退款审核中

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 我的 · `pages/user/mine`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 打印用户 / 会员等级 / 卡券 |
| 页面目标 | 账户资产与所有功能入口 |
| 接口 | `/wxapp/user/info`<br>`/wxapp/coupon/mine`<br>`/wxapp/user/bill` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】我的
【路径】pages/user/mine
【后台功能】打印用户 / 会员等级 / 卡券
【页面目标】账户资产与所有功能入口

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 用户头卡（头像 + 昵称 + 手机号脱敏 + 会员徽章）
2. 资产三宫格（余额 / 优惠券 / 积分，点击进明细）
3. 我的服务九宫格（我的文档 / 我的拼团 / 邀请好友 / 实名认证 / 发票 / 客服 / 关于 / 设置）
4. 商家入口（有权限时显示：商家中心）
5. 版本号（5 击切换环境）

【需要展示/绑定的字段】
· avatar ｜ nickname ｜ phoneMask ｜ memberLevel ｜ balance ｜ couponCount ｜ points ｜ isMerchant

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/user/info
· /wxapp/coupon/mine
· /wxapp/user/bill

【交互跳转】
· 余额 → account/wallet
· 券 → account/coupon
· 商家 → merchant/dashboard

【必须覆盖的状态】
· 未登录 ｜ 加载失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 登录 · `pages/auth/login`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 微信登录（wechat 4 接口） |
| 页面目标 | 微信一键登录 + 手机号绑定 |
| 接口 | `/wxapp/wechat/login`<br>`/wxapp/bindphone`<br>`/wxapp/auth/login` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】登录
【路径】pages/auth/login
【后台功能】微信登录（wechat 4 接口）
【页面目标】微信一键登录 + 手机号绑定

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 品牌区（Logo + 一句话介绍）
2. 主按钮（微信一键登录，绿色）
3. 手机号登录（输入框 + 验证码）
4. 协议勾选（我已阅读并同意 服务协议 / 隐私政策）
5. 底部（其他登录方式）

【需要展示/绑定的字段】
· openid ｜ phone ｜ code ｜ agreed

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/wechat/login
· /wxapp/bindphone
· /wxapp/auth/login

【交互跳转】
· 登录成功 → 来源页 / index

【必须覆盖的状态】
· 未勾选协议 ｜ 验证码错误 ｜ 手机号已绑定其他账号

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 消息中心 · `pages/notice/list`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 站内通知（notice 5 接口） |
| 页面目标 | 接收订单状态、活动、系统通知 |
| 接口 | `/wxapp/pugin/notice/messages`<br>`/wxapp/pugin/notice/read` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】消息中心
【路径】pages/notice/list
【后台功能】站内通知（notice 5 接口）
【页面目标】接收订单状态、活动、系统通知

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 分类 tab（全部 / 订单 / 活动 / 系统）
2. 消息列表（图标 + 标题 + 时间 + 未读红点 + 摘要）
3. 详情弹层（全文 + 跳转按钮）
4. 全部已读按钮

【需要展示/绑定的字段】
· msgId ｜ type ｜ title ｜ summary ｜ time ｜ unread ｜ link

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/pugin/notice/messages
· /wxapp/pugin/notice/read

【交互跳转】
· 订单类 → order/detail
· 活动类 → account/landing

【必须覆盖的状态】
· 无消息 ｜ 加载失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 搜索 · `pages/search/search`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 资料库 / 门店 / 订单 |
| 页面目标 | 一次搜索定位文档、模板、门店 |
| 接口 | `/wxapp/word/search`<br>`/wxapp/stores`<br>`/wxapp/order/list` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】搜索
【路径】pages/search/search
【后台功能】资料库 / 门店 / 订单
【页面目标】一次搜索定位文档、模板、门店

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 搜索框（自动聚焦 + 取消）
2. 历史与热搜（标签云）
3. 结果分组（文档 / 模板 / 门店 / 订单）
4. 空结果建议

【需要展示/绑定的字段】
· keyword ｜ history[] ｜ hot[] ｜ resultsByType

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/word/search
· /wxapp/stores
· /wxapp/order/list

【交互跳转】
· 点击结果 → 对应详情

【必须覆盖的状态】
· 无结果

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 网页容器 · `pages/web/web`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | — |
| 页面目标 | 承载 H5、协议、外链 |
| 接口 | — |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】网页容器
【路径】pages/web/web
【后台功能】—
【页面目标】承载 H5、协议、外链

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. web-view（全屏）
2. 加载中骨架
3. 加载失败重试

【需要展示/绑定的字段】
· url ｜ title

【交互跳转】
· 返回上一页

【必须覆盖的状态】
· 加载中 ｜ 加载失败 ｜ 域名未备案

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 服务协议与隐私 · `pages/rules/terms`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 平台参数 |
| 页面目标 | 展示协议与隐私政策全文 |
| 接口 | `/wxapp/about` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】服务协议与隐私
【路径】pages/rules/terms
【后台功能】平台参数
【页面目标】展示协议与隐私政策全文

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 顶部 tab（服务协议 / 隐私政策 / 退款规则）
2. 正文滚动区（富文本）
3. 底部（生效日期）

【需要展示/绑定的字段】
· docType ｜ content ｜ effectiveDate

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/about

【交互跳转】
· —

【必须覆盖的状态】
· 加载中

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 关于我们 · `pages/about/about`　[P2]

| 项 | 内容 |
|---|---|
| 后台功能 | about |
| 页面目标 | 品牌介绍、资质、联系方式 |
| 接口 | `/wxapp/about` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】关于我们
【路径】pages/about/about
【后台功能】about
【页面目标】品牌介绍、资质、联系方式

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 品牌头（Logo + 名称 + slogan）
2. 数据条（服务门店数 / 累计打印页数）
3. 资质与备案（链接列表）
4. 联系方式（客服微信 / 电话 / 邮箱）

【需要展示/绑定的字段】
· logo ｜ slogan ｜ storeCount ｜ pageCount ｜ phone

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/about

【交互跳转】
· 客服 → 会话

【必须覆盖的状态】
· 无数据

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 通用结果页 · `pages/result/result`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | — |
| 页面目标 | 统一的成功/失败/待审核反馈 |
| 接口 | — |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】通用结果页
【路径】pages/result/result
【后台功能】—
【页面目标】统一的成功/失败/待审核反馈

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 状态图标（成功/失败/待审 三态）
2. 主标题 + 副标题
3. 详情卡（可选）
4. 操作按钮组（主按钮 + 次按钮）

【需要展示/绑定的字段】
· state ｜ title ｜ subtitle ｜ detail

【交互跳转】
· 返回来源页

【必须覆盖的状态】
· —

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 配送进度 · `pages/delivery/progress`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 配送管理（delivery 8 接口） |
| 页面目标 | 查看配送状态与骑手位置 |
| 接口 | `/wxapp/delivery/detail`<br>`/wxapp/delivery/track` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】配送进度
【路径】pages/delivery/progress
【后台功能】配送管理（delivery 8 接口）
【页面目标】查看配送状态与骑手位置

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 状态时间轴（已接单 / 取件中 / 配送中 / 已送达）
2. 骑手卡片（头像 + 姓名 + 电话按钮）
3. 地图占位（骑手位置）
4. 订单摘要

【需要展示/绑定的字段】
· deliveryNo ｜ status ｜ riderName ｜ riderPhone ｜ etaMinutes ｜ address

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/delivery/detail
· /wxapp/delivery/track

【交互跳转】
· 电话 → 拨号

【必须覆盖的状态】
· 无人接单 ｜ 已送达

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 售后工单 · `pages/workorder/list`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 工单处理（workorder 5 接口） |
| 页面目标 | 提交打印质量问题并跟踪处理 |
| 接口 | `/wxapp/workorder/create`<br>`/wxapp/workorder/list` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】售后工单
【路径】pages/workorder/list
【后台功能】工单处理（workorder 5 接口）
【页面目标】提交打印质量问题并跟踪处理

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 工单类型（打印模糊 / 缺页 / 设备故障 / 其他）
2. 关联订单选择（最近 3 笔）
3. 问题描述（文本 + 图片上传）
4. 我的工单列表（状态 + 时间 + 处理结果）

【需要展示/绑定的字段】
· type ｜ orderNo ｜ desc ｜ images[] ｜ status ｜ reply

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/workorder/create
· /wxapp/workorder/list

【交互跳转】
· 提交 → result/result

【必须覆盖的状态】
· 未选订单 ｜ 提交失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 资料库与模板 · `pages/library/shelf`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 资料库（word 11 接口） |
| 页面目标 | 浏览/搜索模板与文档，一键打印 |
| 接口 | `/wxapp/word/list`<br>`/wxapp/word/detail` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】资料库与模板
【路径】pages/library/shelf
【后台功能】资料库（word 11 接口）
【页面目标】浏览/搜索模板与文档，一键打印

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 分类横向胶囊（全部 / 简历 / 合同 / 学习 / 表格 / 海报）
2. 模板卡片网格（封面 + 标题 + 页数 + 免费/付费标签）
3. 搜索入口
4. 最近使用横向列表

【需要展示/绑定的字段】
· category ｜ docId ｜ title ｜ cover ｜ pages ｜ price ｜ free

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/word/list
· /wxapp/word/detail

【交互跳转】
· 点击 → print/config（带入文件）

【必须覆盖的状态】
· 无内容 ｜ 加载中

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```



---

# 分包 print · 打印流程（8 页）


## 打印入口 · `pages/print/home`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 文件管理 / 网盘 / U盘 |
| 页面目标 | 让用户选择文件来源，作为打印流程的起点 |
| 接口 | `/wxapp/file/list`<br>`/wxapp/upan/list`<br>`/wxapp/xpan/files`<br>`/wxapp/word/list` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】打印入口
【路径】pages/print/home
【后台功能】文件管理 / 网盘 / U盘
【页面目标】让用户选择文件来源，作为打印流程的起点

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 顶部标题区（当前门店名 + 切换门店）
2. 来源卡片列表（本地文件 / 相册图片 / 微信文件 / 百度网盘 / U 盘 / 资料库模板 / 链接打印，每项：图标 + 标题 + 副标题 + 箭头）
3. 最近文件横向滑动（最近 6 个文件的缩略卡片）
4. 底部提示条（支持格式 jpg/png/pdf/docx/pptx，最大 100MB）

【需要展示/绑定的字段】
· sourceType ｜ recentFiles[] ｜ storeName

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/file/list
· /wxapp/upan/list
· /wxapp/xpan/files
· /wxapp/word/list

【交互跳转】
· 选择来源 → print/upload（携带 source 参数）

【必须覆盖的状态】
· 网盘未授权 ｜ U盘未插入 ｜ 无最近文件

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 上传与格式检查 · `pages/print/upload`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 文件管理（file 18 接口） |
| 页面目标 | 上传并校验文件，明确告诉用户哪些能打、哪些要转换 |
| 接口 | `/wxapp/file/list`<br>`/wxapp/file/upload`<br>`/wxapp/file/convert`<br>`/wxapp/file/remove` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】上传与格式检查
【路径】pages/print/upload
【后台功能】文件管理（file 18 接口）
【页面目标】上传并校验文件，明确告诉用户哪些能打、哪些要转换

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 顶部来源切换（本地 / 相册 / 微信文件 三个胶囊）
2. 文件卡片列表（左：类型图标 72rpx；中：文件名 + 页数 + 大小；右：状态徽章 可打印/需转换/不支持 + 删除）
3. 校验提示条（有需要转换的文件时：黄色提示 + 一键转换）
4. 空态（居中图标 + 还没有选择文件 + 选择文件按钮）
5. 底部操作条（已选 N 个 · 共 M 页 · 下一步）

【需要展示/绑定的字段】
· fileName ｜ fileType ｜ pageCount ｜ sizeKB ｜ status

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/file/list
· /wxapp/file/upload
· /wxapp/file/convert
· /wxapp/file/remove

【交互跳转】
· 下一步 → print/config
· 点击行 → print/preview

【必须覆盖的状态】
· 上传中（进度条） ｜ 格式不支持 ｜ 超过 100MB ｜ 转换失败可重试

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 打印参数配置 · `pages/print/config`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 打印订单（计价） |
| 页面目标 | 配置单双面/色彩/纸张/份数/装订，实时显示价格 |
| 接口 | `/wxapp/order/calculate-price` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】打印参数配置
【路径】pages/print/config
【后台功能】打印订单（计价）
【页面目标】配置单双面/色彩/纸张/份数/装订，实时显示价格

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 文件摘要卡（文件名 + 页数 + 缩略图）
2. 参数分组一（单双面：单面/双面·长边/双面·短边 胶囊）
3. 参数分组二（色彩：黑白/彩色；纸张：A4/A3/相纸）
4. 参数分组三（份数步进器 + 页码范围输入）
5. 装订选项（不装订/订书钉/胶装，带价格标签）
6. 底部价格条（预估 ¥X.XX + 下一步）

【需要展示/绑定的字段】
· isDouble ｜ isColor ｜ isA3 ｜ copies ｜ pageRange ｜ binding ｜ price

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/order/calculate-price

【交互跳转】
· 下一步 → print/preview

【必须覆盖的状态】
· 计价中 ｜ 价格失败可重试 ｜ 页数超限

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 纸张预览 · `pages/print/preview`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 文件管理 / 打印订单 |
| 页面目标 | 逐页预览最终效果，锁定版本避免出纸与预览不一致 |
| 接口 | `/wxapp/file/preview`<br>`/wxapp/order/calculate-price` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】纸张预览
【路径】pages/print/preview
【后台功能】文件管理 / 打印订单
【页面目标】逐页预览最终效果，锁定版本避免出纸与预览不一致

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 顶部工具条（页码指示 3/12 + 缩放 + 单页/双页切换）
2. 预览画布（白纸 + 内容缩略，标注页边距与装订边）
3. 版本锁定提示（L 形角标 + 版本号 + 已锁定文案）
4. 底部价格条（同 config，显示已锁定的参数摘要）
5. 确认弹层（确认后不可修改参数）

【需要展示/绑定的字段】
· pageIndex ｜ totalPages ｜ versionNo ｜ locked ｜ price

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/file/preview
· /wxapp/order/calculate-price

【交互跳转】
· 确认 → print/payment

【必须覆盖的状态】
· 预览生成中 ｜ 锁定失败 ｜ 页数变更需重新锁定

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 履约与支付 · `pages/print/payment`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 打印订单 / 支付 |
| 页面目标 | 选择取件方式与门店，完成支付 |
| 接口 | `/wxapp/order/create`<br>`/wxapp/order/pay`<br>`/wxapp/delivery/quote`<br>`/wxapp/stores` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】履约与支付
【路径】pages/print/payment
【后台功能】打印订单 / 支付
【页面目标】选择取件方式与门店，完成支付

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 订单摘要卡（文件 + 页数 + 参数一行摘要 + 金额大字）
2. 履约方式切换（到店自取 / 配送到寝 两个大卡片，选中态描边高亮）
3. 自取区块（门店下拉 + 预计完成时间 + 排队提示）
4. 配送区块（地址选择 + 运费 + 预计送达）
5. 支付方式（微信支付，单行）
6. 底部支付条（实付 ¥X + 立即支付）

【需要展示/绑定的字段】
· fulfillType ｜ storeName ｜ etaMinutes ｜ address ｜ freight ｜ payAmount

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/order/create
· /wxapp/order/pay
· /wxapp/delivery/quote
· /wxapp/stores

【交互跳转】
· 支付成功 → print/success
· 配送 → delivery/progress

【必须覆盖的状态】
· 门店打烊 ｜ 地址未填 ｜ 支付失败可重试 ｜ 库存不足

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 支付成功 · `pages/print/success`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 打印订单 / 设备状态 |
| 页面目标 | 给出取件码与出纸进度，让用户知道什么时候来取 |
| 接口 | `/wxapp/order/detail`<br>`/wxapp/machines/online` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】支付成功
【路径】pages/print/success
【后台功能】打印订单 / 设备状态
【页面目标】给出取件码与出纸进度，让用户知道什么时候来取

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 成功头部（勾选图标 + 支付成功 + 金额）
2. 取件码卡（4 位大字 + 复制按钮 + 二维码）
3. 出纸进度（设备名 + 已出 X/Y 页 + 进度条 + 预计剩余秒数）
4. 订单摘要（订单号 + 门店 + 时间）
5. 底部操作（查看订单 / 返回首页）

【需要展示/绑定的字段】
· pickupCode ｜ orderNo ｜ machineName ｜ printedPages ｜ totalPages ｜ etaSeconds

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/order/detail
· /wxapp/machines/online

【交互跳转】
· 查看订单 → order/detail
· 返回首页 → index

【必须覆盖的状态】
· 设备离线 ｜ 排队中 ｜ 已完成 ｜ 异常需联系客服

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 复印 / 扫描 · `pages/print/copier`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 复印设置（copier 6 接口） |
| 页面目标 | 门店现场复印/扫描，参数与价格实时可见 |
| 接口 | `/wxapp/copier/settings`<br>`/wxapp/copier/price`<br>`/wxapp/copier/start` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】复印 / 扫描
【路径】pages/print/copier
【后台功能】复印设置（copier 6 接口）
【页面目标】门店现场复印/扫描，参数与价格实时可见

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 模式切换（复印 / 扫描 两个大 tab）
2. 复印参数（份数步进器、单双面、浓度滑块 5 档、缩放 100%/适配/A3→A4）
3. 扫描参数（分辨率 300/600dpi、格式 PDF/JPG、色彩）
4. 价格条（复印 ¥0.5/页 × 份数）
5. 底部主按钮（开始复印 / 开始扫描）

【需要展示/绑定的字段】
· mode ｜ copies ｜ density ｜ scale ｜ dpi ｜ format ｜ price

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/copier/settings
· /wxapp/copier/price
· /wxapp/copier/start

【交互跳转】
· 扫描完成 → 保存到相册 / 我的文档

【必须覆盖的状态】
· 设备不支持复印 ｜ 缺纸 ｜ 扫描失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 打印指南 · `pages/print/guide`　[P2]

| 项 | 内容 |
|---|---|
| 后台功能 | 资料库 / 帮助 |
| 页面目标 | 常见格式、纸张、取件问题自助解答 |
| 接口 | `/wxapp/about`<br>`/wxapp/word/list` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】打印指南
【路径】pages/print/guide
【后台功能】资料库 / 帮助
【页面目标】常见格式、纸张、取件问题自助解答

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 分类标签（格式 / 纸张 / 取件 / 计费）
2. 问答手风琴列表（问题 + 展开答案）
3. 联系客服按钮

【需要展示/绑定的字段】
· category ｜ question ｜ answer

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/about
· /wxapp/word/list

【交互跳转】
· 联系客服 → 客服会话

【必须覆盖的状态】
· 无内容

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```



---

# 分包 group · 打印拼团（4 页）


## 拼团大厅 · `pages/group/lobby`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 拼团活动（19 接口） |
| 页面目标 | 浏览进行中的打印拼团，跟团或开团 |
| 接口 | `/wxapp/group/list`<br>`/wxapp/group/detail` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】拼团大厅
【路径】pages/group/lobby
【后台功能】拼团活动（19 接口）
【页面目标】浏览进行中的打印拼团，跟团或开团

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 顶部筛选（全部 / 差 1 人 / 即将结束）
2. 拼团卡片列表（封面 + 标题 + 当前人数/成团人数 + 倒计时 + 单价对比 + 参团按钮）
3. 开团悬浮按钮（右下角圆形 +）
4. 空态

【需要展示/绑定的字段】
· groupId ｜ title ｜ joined ｜ target ｜ countdown ｜ groupPrice ｜ originPrice

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/group/list
· /wxapp/group/detail

【交互跳转】
· 参团 → group/order
· 开团 → group/order?mode=create

【必须覆盖的状态】
· 已结束 ｜ 已满员 ｜ 无进行中团

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 拼团详情 · `pages/group/detail`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 拼团活动 |
| 页面目标 | 看清拼团规则、成团进度与成员 |
| 接口 | `/wxapp/group/detail`<br>`/wxapp/group/join` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】拼团详情
【路径】pages/group/detail
【后台功能】拼团活动
【页面目标】看清拼团规则、成团进度与成员

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 头图 + 标题 + 价格对比
2. 成团进度（进度条 + 已参 N/M 人 + 倒计时）
3. 参团成员头像墙（含团长标识）
4. 拼团规则（成团条件/失败退款/有效期）
5. 底部参团条（拼团价 + 立即参团）

【需要展示/绑定的字段】
· title ｜ joined ｜ target ｜ countdown ｜ members[] ｜ rules

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/group/detail
· /wxapp/group/join

【交互跳转】
· 立即参团 → group/order

【必须覆盖的状态】
· 团已满 ｜ 团已过期 ｜ 已参过团

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 拼团下单 · `pages/group/order`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 拼团订单 |
| 页面目标 | 选择文件与参数后提交拼团订单 |
| 接口 | `/wxapp/group/order/create`<br>`/wxapp/order/calculate-price` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】拼团下单
【路径】pages/group/order
【后台功能】拼团订单
【页面目标】选择文件与参数后提交拼团订单

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 拼团摘要（标题 + 拼团价 + 剩余名额）
2. 文件选择卡（复用上传组件，显示已选文件）
3. 参数摘要（单双面/色彩/页数）
4. 金额区（原价 / 拼团省 / 实付）
5. 底部提交条

【需要展示/绑定的字段】
· groupId ｜ files[] ｜ params ｜ originAmount ｜ discount ｜ payAmount

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/group/order/create
· /wxapp/order/calculate-price

【交互跳转】
· 提交成功 → group/mine
· 支付 → 通用支付组件

【必须覆盖的状态】
· 名额已满 ｜ 文件校验失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 我的拼团 · `pages/group/mine`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 拼团订单 |
| 页面目标 | 查看自己参与/发起的拼团状态 |
| 接口 | `/wxapp/group/mine` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】我的拼团
【路径】pages/group/mine
【后台功能】拼团订单
【页面目标】查看自己参与/发起的拼团状态

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 状态 tab（进行中 / 已成团 / 已退款）
2. 拼团条目（标题 + 我的人数 + 状态徽章 + 成团时间/退款金额）
3. 空态

【需要展示/绑定的字段】
· groupId ｜ role ｜ status ｜ joinedAt ｜ refundAmount

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/group/mine

【交互跳转】
· 点击 → group/detail

【必须覆盖的状态】
· 无记录 ｜ 退款处理中

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```



---

# 分包 tools · 证照与工具（9 页）


## 工具箱 · `pages/tools/home`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | AI 工具（aitools）+ tool 82 接口 |
| 页面目标 | 汇聚所有文档/图像处理工具 |
| 接口 | `/wxapp/tool/list` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】工具箱
【路径】pages/tools/home
【后台功能】AI 工具（aitools）+ tool 82 接口
【页面目标】汇聚所有文档/图像处理工具

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 分组标题一（证照类）：证件照 / 证件复印 / 照片冲印
2. 分组标题二（文档类）：转 PDF / 长图转 PDF / PPT 打印 / 拍照扫描
3. 分组标题三（学习类）：错题本 / 试卷重排
4. 工具卡片（图标 + 名称 + 一句话说明）

【需要展示/绑定的字段】
· toolKey ｜ name ｜ desc ｜ icon

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/tool/list

【交互跳转】
· 点击 → 对应工具页

【必须覆盖的状态】
· 工具维护中

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 证件照制作 · `pages/tools/idphoto`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | AI 工具 / tool |
| 页面目标 | 选规格、换底色、生成排版照 |
| 接口 | `/wxapp/tool/idphoto/specs`<br>`/wxapp/tool/idphoto/generate` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】证件照制作
【路径】pages/tools/idphoto
【后台功能】AI 工具 / tool
【页面目标】选规格、换底色、生成排版照

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 规格选择（一寸/二寸/小二寸/签证等，横向胶囊）
2. 照片上传区（虚线框 + 相机/相册两个按钮）
3. 换底颜色（蓝/白/红/灰 圆形色块）
4. 预览与微调（裁剪框 + 缩放）
5. 排版选项（单张/六寸排版 8 张）
6. 底部生成按钮（价格标签）

【需要展示/绑定的字段】
· specKey ｜ bgColor ｜ cropRect ｜ layout ｜ price

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/tool/idphoto/specs
· /wxapp/tool/idphoto/generate

【交互跳转】
· 生成 → 保存到相册 / 直接打印

【必须覆盖的状态】
· 人脸检测失败 ｜ 生成中（进度） ｜ 规格不支持

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 证件复印 · `pages/tools/cert-print`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 证件打印（原版 printhu/printying/printjia） |
| 页面目标 | 户口本/营业执照/驾照等套版复印 |
| 接口 | `/wxapp/tool/cert/templates`<br>`/wxapp/tool/cert/compose`<br>`/wxapp/copier/start` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】证件复印
【路径】pages/tools/cert-print
【后台功能】证件打印（原版 printhu/printying/printjia）
【页面目标】户口本/营业执照/驾照等套版复印

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 证件类型卡（户口本 / 营业执照 / 驾照 / 身份证，图标 + 说明）
2. 拍摄引导（示意图 + 对齐框提示）
3. 已拍页缩略图（可重拍/删除）
4. 排版预览（A4 上 2/4 张排布）
5. 底部按钮（复印）

【需要展示/绑定的字段】
· certType ｜ shots[] ｜ layout ｜ price

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/tool/cert/templates
· /wxapp/tool/cert/compose
· /wxapp/copier/start

【交互跳转】
· 复印 → 设备出纸 / print/success

【必须覆盖的状态】
· 拍摄模糊提示 ｜ 缺页提示

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 照片冲印 · `pages/tools/photo-print`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | tool / 打印订单 |
| 页面目标 | 选相纸与尺寸，冲印生活照 |
| 接口 | `/wxapp/order/calculate-price`<br>`/wxapp/tool/photo/print` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】照片冲印
【路径】pages/tools/photo-print
【后台功能】tool / 打印订单
【页面目标】选相纸与尺寸，冲印生活照

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 相片选择（九宫格多选 + 已选数量）
2. 尺寸选择（6寸/7寸/证件/钱包照，卡片带尺寸标注）
3. 相纸质感（光面/绒面/亚光，卡片带说明）
4. 份数步进
5. 底部价格条（¥X/张 × 份数）

【需要展示/绑定的字段】
· photos[] ｜ sizeKey ｜ craftKey ｜ copies ｜ price

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/order/calculate-price
· /wxapp/tool/photo/print

【交互跳转】
· 下一步 → print/payment

【必须覆盖的状态】
· 相册无权限 ｜ 照片过大

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 拍照扫描 · `pages/tools/scan`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | tool / copier |
| 页面目标 | 把纸质文档拍成清晰 PDF |
| 接口 | `/wxapp/tool/scan/compose` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】拍照扫描
【路径】pages/tools/scan
【后台功能】tool / copier
【页面目标】把纸质文档拍成清晰 PDF

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 拍摄区（取景框 + 自动边缘检测黄框提示）
2. 拍摄后裁剪/旋转工具条
3. 多页管理（缩略图列表 + 页码 + 删除）
4. 输出设置（黑白/彩色、PDF/JPG、300/600dpi）
5. 底部按钮（生成 PDF）

【需要展示/绑定的字段】
· shots[] ｜ edgeDetected ｜ colorMode ｜ format ｜ dpi

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/tool/scan/compose

【交互跳转】
· 生成 → 我的文档 / 直接打印 / 打印流程

【必须覆盖的状态】
· 边缘未识别 ｜ 生成中 ｜ 保存相册失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 错题本 · `pages/tools/mistake`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | AI 工具 |
| 页面目标 | 拍试卷、自动切题、重排出干净练习卷 |
| 接口 | `/wxapp/tool/mistake/parse`<br>`/wxapp/tool/mistake/compose` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】错题本
【路径】pages/tools/mistake
【后台功能】AI 工具
【页面目标】拍试卷、自动切题、重排出干净练习卷

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 上传区（拍试卷 / 相册选）
2. 识别结果（题目卡片列表，可勾选保留/删除）
3. 重排设置（每题留白行数、是否保留原题号、纸张大小）
4. 预览与生成（两栏对比：原图 vs 重排）
5. 底部按钮（生成可打印 PDF）

【需要展示/绑定的字段】
· pages[] ｜ questions[] ｜ blankLines ｜ keepNumbering ｜ paperSize

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/tool/mistake/parse
· /wxapp/tool/mistake/compose

【交互跳转】
· 生成 → 保存 / 打印流程

【必须覆盖的状态】
· 识别失败 ｜ 题目切分不准可手动框选

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 转 PDF · `pages/tools/topdf`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | tool 82 接口 |
| 页面目标 | 图片/Word/PPT 一键转 PDF |
| 接口 | `/wxapp/tool/convert` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】转 PDF
【路径】pages/tools/topdf
【后台功能】tool 82 接口
【页面目标】图片/Word/PPT 一键转 PDF

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 上传区（多文件）
2. 转换选项（合并为一个 PDF / 分文件、页面方向、压缩质量）
3. 转换队列（每项：文件名 + 进度 + 状态）
4. 底部按钮（开始转换）

【需要展示/绑定的字段】
· files[] ｜ merge ｜ orientation ｜ quality ｜ progress

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/tool/convert

【交互跳转】
· 转换完成 → 下载 / 打印

【必须覆盖的状态】
· 失败可重试 ｜ 超过大小限制

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 长图转 PDF · `pages/tools/longimg`　[P2]

| 项 | 内容 |
|---|---|
| 后台功能 | tool |
| 页面目标 | 把长截图切成 A4 分页 PDF |
| 接口 | `/wxapp/tool/longimg/split` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】长图转 PDF
【路径】pages/tools/longimg
【后台功能】tool
【页面目标】把长截图切成 A4 分页 PDF

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 长图上传 + 预览
2. 切分设置（每页高度自动/手动、是否加页边距）
3. 分页预览（缩略图列表）
4. 底部按钮（生成 PDF）

【需要展示/绑定的字段】
· image ｜ sliceMode ｜ margin ｜ pages

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/tool/longimg/split

【交互跳转】
· 生成 → 打印

【必须覆盖的状态】
· 图片过大

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## PPT 打印 · `pages/tools/ppt`　[P2]

| 项 | 内容 |
|---|---|
| 后台功能 | ppt 2 接口 |
| 页面目标 | PPT 按多页合一打印 |
| 接口 | `/wxapp/ppt/convert` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】PPT 打印
【路径】pages/tools/ppt
【后台功能】ppt 2 接口
【页面目标】PPT 按多页合一打印

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. PPT 上传
2. 排版选项（每页 1/2/4/6 张、是否加边框、备注页）
3. 预览
4. 底部按钮

【需要展示/绑定的字段】
· file ｜ slidesPerPage ｜ withBorder ｜ withNotes

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/ppt/convert

【交互跳转】
· 完成 → 打印流程

【必须覆盖的状态】
· 转换失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```



---

# 分包 account · 账户与营销（10 页）


## 会员与套餐 · `pages/account/member`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 会员等级（level 4 接口） |
| 页面目标 | 展示会员权益并引导开通/续费 |
| 接口 | `/wxapp/level/list`<br>`/wxapp/level/order/create`<br>`/wxapp/promotion/coupon/list` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】会员与套餐
【路径】pages/account/member
【后台功能】会员等级（level 4 接口）
【页面目标】展示会员权益并引导开通/续费

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 当前等级卡（等级名 + 有效期 + 折扣率）
2. 权益对比表（折扣 / 免运费 / 优先出纸 / 专属券）
3. 套餐卡列表（月卡/季卡/年卡，价格 + 已售 + 推荐标签）
4. 次卡区块（10 次 / 30 次打印卡）
5. 底部开通条

【需要展示/绑定的字段】
· level ｜ expireAt ｜ discount ｜ plans[] ｜ coupons[]

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/level/list
· /wxapp/level/order/create
· /wxapp/promotion/coupon/list

【交互跳转】
· 开通 → 通用支付

【必须覆盖的状态】
· 已是最高级 ｜ 续费失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 券中心 · `pages/account/coupon`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 卡券管理（card 6 接口） |
| 页面目标 | 领券、查看我的券、按可用性筛选 |
| 接口 | `/wxapp/promotion/coupon/list`<br>`/wxapp/coupon/mine`<br>`/wxapp/promotion/coupon/claim` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】券中心
【路径】pages/account/coupon
【后台功能】卡券管理（card 6 接口）
【页面目标】领券、查看我的券、按可用性筛选

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 我的券 tab（可用 / 已用 / 已过期）
2. 领券区（可领取的券，票券形态：左面额 + 右门槛 + 领取按钮）
3. 券卡（左色块：¥X 或 X 折；右：名称 + 门槛 + 有效期 + 去使用）

【需要展示/绑定的字段】
· couponId ｜ amount ｜ threshold ｜ scope ｜ expireAt ｜ status

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/promotion/coupon/list
· /wxapp/coupon/mine
· /wxapp/promotion/coupon/claim

【交互跳转】
· 去使用 → index / print/home

【必须覆盖的状态】
· 无可领券 ｜ 已领完 ｜ 即将过期

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 钱包明细 · `pages/account/wallet`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 打印用户（user 8 接口） |
| 页面目标 | 余额、收支流水 |
| 接口 | `/wxapp/user/bill`<br>`/wxapp/user/recharge` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】钱包明细
【路径】pages/account/wallet
【后台功能】打印用户（user 8 接口）
【页面目标】余额、收支流水

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 余额卡（大字金额 + 充值按钮）
2. 筛选（全部 / 消费 / 充值 / 退款）
3. 流水列表（类型图标 + 说明 + 时间 + ±金额）
4. 空态

【需要展示/绑定的字段】
· balance ｜ billType ｜ amount ｜ time ｜ remark

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/user/bill
· /wxapp/user/recharge

【交互跳转】
· 充值 → account/recharge

【必须覆盖的状态】
· 无流水

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 余额充值 · `pages/account/recharge`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 充值（chongfen 3 接口） |
| 页面目标 | 选档位充值，到账即时 |
| 接口 | `/wxapp/user/recharge`<br>`/wxapp/user/recharge/options` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】余额充值
【路径】pages/account/recharge
【后台功能】充值（chongfen 3 接口）
【页面目标】选档位充值，到账即时

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 当前余额
2. 档位网格（50/100/200/500，带赠送标签）
3. 自定义金额输入
4. 支付方式
5. 底部按钮（立即充值）

【需要展示/绑定的字段】
· balance ｜ amount ｜ bonus ｜ payType

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/user/recharge
· /wxapp/user/recharge/options

【交互跳转】
· 成功 → result/result

【必须覆盖的状态】
· 未实名限制 ｜ 支付失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 邀请好友 · `pages/account/invite`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 邀请返利（invite 6 接口） |
| 页面目标 | 生成专属海报/链接，查看返佣 |
| 接口 | `/wxapp/promotion/invite/info`<br>`/wxapp/promotion/invite/records`<br>`/wxapp/promotion/invite/poster` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】邀请好友
【路径】pages/account/invite
【后台功能】邀请返利（invite 6 接口）
【页面目标】生成专属海报/链接，查看返佣

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 收益卡（累计返佣 + 可提现 + 已邀请人数）
2. 邀请海报（预览 + 保存到相册 + 分享按钮）
3. 专属邀请码（大字 + 复制）
4. 返佣规则（折叠）
5. 邀请记录列表

【需要展示/绑定的字段】
· inviteCode ｜ totalCommission ｜ withdrawable ｜ invitedCount ｜ records[]

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/promotion/invite/info
· /wxapp/promotion/invite/records
· /wxapp/promotion/invite/poster

【交互跳转】
· 提现 → merchant/withdraw（有权限）

【必须覆盖的状态】
· 无邀请记录 ｜ 海报生成中

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 抽奖 · `pages/account/raffle`　[P2]

| 项 | 内容 |
|---|---|
| 后台功能 | 抽奖活动（raffle 8 接口） |
| 页面目标 | 消耗抽奖次数抽奖品 |
| 接口 | `/wxapp/promotion/raffle/info`<br>`/wxapp/promotion/raffle/draw`<br>`/wxapp/promotion/raffle/mine` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】抽奖
【路径】pages/account/raffle
【后台功能】抽奖活动（raffle 8 接口）
【页面目标】消耗抽奖次数抽奖品

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 奖品转盘（九宫格/圆形，高亮轮转）
2. 剩余次数 + 抽奖按钮
3. 我的奖品列表（奖品名 + 时间 + 领取状态）
4. 中奖弹层（奖品图 + 领取按钮）

【需要展示/绑定的字段】
· prizes[] ｜ chances ｜ prizeName ｜ claimStatus

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/promotion/raffle/info
· /wxapp/promotion/raffle/draw
· /wxapp/promotion/raffle/mine

【交互跳转】
· 领取 → 我的券 / 联系客服

【必须覆盖的状态】
· 无抽奖次数 ｜ 奖品已抽完

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 每日签到 · `pages/account/sign`　[P2]

| 项 | 内容 |
|---|---|
| 后台功能 | 签到 / 积分 |
| 页面目标 | 连续签到领积分 |
| 接口 | `/wxapp/user/sign`<br>`/wxapp/user/sign/info` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】每日签到
【路径】pages/account/sign
【后台功能】签到 / 积分
【页面目标】连续签到领积分

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 连续天数卡（N 天 + 日历格）
2. 7 日奖励条（每日奖励，已签打勾）
3. 签到按钮（今日已签置灰）
4. 积分说明

【需要展示/绑定的字段】
· streak ｜ todaySigned ｜ rewards[] ｜ points

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/user/sign
· /wxapp/user/sign/info

【交互跳转】
· —

【必须覆盖的状态】
· 今日已签 ｜ 断签重置提示

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 新人礼包 · `pages/account/landing`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 卡券 / 营销 |
| 页面目标 | 新用户领取专属券包 |
| 接口 | `/wxapp/promotion/coupon/list`<br>`/wxapp/promotion/coupon/claim` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】新人礼包
【路径】pages/account/landing
【后台功能】卡券 / 营销
【页面目标】新用户领取专属券包

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 礼包头图（渐变 + 券叠层）
2. 券包列表（每张券：面额 + 门槛 + 有效期）
3. 一键领取按钮
4. 已领取状态

【需要展示/绑定的字段】
· giftId ｜ coupons[] ｜ claimed

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/promotion/coupon/list
· /wxapp/promotion/coupon/claim

【交互跳转】
· 领取 → account/coupon

【必须覆盖的状态】
· 已领取 ｜ 活动结束

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 返利明细 · `pages/account/rebate`　[P2]

| 项 | 内容 |
|---|---|
| 后台功能 | 返利管理（fanli 4 接口） |
| 页面目标 | 查看返利流水与结算状态 |
| 接口 | `/wxapp/promotion/fanli/records` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】返利明细
【路径】pages/account/rebate
【后台功能】返利管理（fanli 4 接口）
【页面目标】查看返利流水与结算状态

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 汇总卡（累计返利 / 待结算 / 已到账）
2. 流水列表（来源用户 + 金额 + 状态 + 时间）
3. 说明（结算周期）

【需要展示/绑定的字段】
· total ｜ pending ｜ settled ｜ records[]

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/promotion/fanli/records

【交互跳转】
· —

【必须覆盖的状态】
· 无返利

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 家庭版 · `pages/account/family`　[P2]

| 项 | 内容 |
|---|---|
| 后台功能 | 家庭版权限 |
| 页面目标 | 开通家庭版并管理成员 |
| 接口 | `/wxapp/user/family/info`<br>`/wxapp/user/family/invite` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】家庭版
【路径】pages/account/family
【后台功能】家庭版权限
【页面目标】开通家庭版并管理成员

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 家庭版权益（成员数 / 共享额度 / 免运费）
2. 成员列表（头像 + 昵称 + 本月用量）
3. 邀请成员按钮
4. 开通/续费条

【需要展示/绑定的字段】
· plan ｜ members[] ｜ quotaUsed ｜ expireAt

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/user/family/info
· /wxapp/user/family/invite

【交互跳转】
· 邀请 → 分享

【必须覆盖的状态】
· 未开通 ｜ 成员已满

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```



---

# 分包 merchant · 商家中心（15 页）


## 经营看板 · `pages/merchant/dashboard`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 数据看板（stats 23 接口） |
| 页面目标 | 今日经营核心指标一屏看清 |
| 接口 | `/wxapp/admin/stats/overview`<br>`/wxapp/admin/machines` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】经营看板
【路径】pages/merchant/dashboard
【后台功能】数据看板（stats 23 接口）
【页面目标】今日经营核心指标一屏看清

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 日期切换（今日 / 昨日 / 近 7 天）
2. KPI 四宫格（营收 / 订单数 / 打印页数 / 客单价）
3. 趋势图（营收折线，7 天）
4. 设备状态条（在线 X / 离线 Y / 故障 Z）
5. 待办卡（待接单 / 待退款 / 待提现 数字徽章）

【需要展示/绑定的字段】
· revenue ｜ orders ｜ pages ｜ avgOrder ｜ online ｜ offline ｜ fault ｜ todo

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/stats/overview
· /wxapp/admin/machines

【交互跳转】
· 待接单 → merchant/orders
· 待退款 → merchant/refund

【必须覆盖的状态】
· 无数据 ｜ 加载失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 商家订单工作台 · `pages/merchant/orders`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 打印订单 |
| 页面目标 | 接单、标记完成、处理异常 |
| 接口 | `/wxapp/admin/order/list`<br>`/wxapp/admin/order/accept`<br>`/wxapp/admin/order/complete` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】商家订单工作台
【路径】pages/merchant/orders
【后台功能】打印订单
【页面目标】接单、标记完成、处理异常

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 状态 tab（待接单 / 打印中 / 待取件 / 已完成）
2. 订单行（订单号 + 尾号 + 文件 + 页数 + 金额 + 时间 + 状态）
3. 操作按钮（接单 / 开始打印 / 完成 / 异常）
4. 批量操作条（全选 + 批量完成）

【需要展示/绑定的字段】
· orderNo ｜ tailNo ｜ files ｜ pages ｜ amount ｜ status ｜ createdAt

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/order/list
· /wxapp/admin/order/accept
· /wxapp/admin/order/complete

【交互跳转】
· 异常 → workorder 详情

【必须覆盖的状态】
· 无待处理 ｜ 操作失败

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 设备管理 · `pages/merchant/devices`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 设备列表 / 远程维护（machine 23 接口） |
| 页面目标 | 查看设备在线/耗材，下发远程指令 |
| 接口 | `/wxapp/admin/machines`<br>`/wxapp/admin/machines/{id}/command` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】设备管理
【路径】pages/merchant/devices
【后台功能】设备列表 / 远程维护（machine 23 接口）
【页面目标】查看设备在线/耗材，下发远程指令

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 设备卡片列表（设备名 + 型号 + 在线状态灯 + 纸张余量 + 墨粉余量 + 队列数）
2. 快捷操作（清空队列 / 重启 / 测试页）
3. 详情弹层（累计页数 / 最近日志 / 磨损度）

【需要展示/绑定的字段】
· machineId ｜ name ｜ model ｜ online ｜ paperLeft ｜ tonerLeft ｜ queue

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/machines
· /wxapp/admin/machines/{id}/command

【交互跳转】
· 详情 → 日志

【必须覆盖的状态】
· 设备离线 ｜ 指令下发中 ｜ 耗材告警

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 取件核销（PDA） · `pages/merchant/verify`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 打印订单 |
| 页面目标 | 扫码或输码快速核销取件，大按钮适配手持设备 |
| 接口 | `/wxapp/admin/order/verify`<br>`/wxapp/admin/order/lookup` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】取件核销（PDA）
【路径】pages/merchant/verify
【后台功能】打印订单
【页面目标】扫码或输码快速核销取件，大按钮适配手持设备

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 扫码按钮（超大圆形，主色） + 手动输码入口
2. 输入码（4 位数字，超大字体，数字键盘）
3. 核销结果卡（订单信息 + 已核销徽章）
4. 最近核销记录（时间 + 码 + 操作员）

【需要展示/绑定的字段】
· pickupCode ｜ orderNo ｜ verifiedAt ｜ operator

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/order/verify
· /wxapp/admin/order/lookup

【交互跳转】
· 核销成功 → 下一条

【必须覆盖的状态】
· 码不存在 ｜ 已核销过 ｜ 订单未完成

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 价格规则 · `pages/merchant/pricing`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 平台参数 / 计价 |
| 页面目标 | 配置黑白/彩色/A3/装订单价与会员折扣 |
| 接口 | `/wxapp/admin/settings/price`<br>`/wxapp/admin/settings` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】价格规则
【路径】pages/merchant/pricing
【后台功能】平台参数 / 计价
【页面目标】配置黑白/彩色/A3/装订单价与会员折扣

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 基础单价（黑白单面 / 双面 / 彩色 / A3 加价，数字输入 + 单位）
2. 装订价格（订书钉 / 胶装）
3. 会员折扣（普通 / 银卡 / 金卡 百分比）
4. 分时价格（夜间折扣开关 + 时段）
5. 保存条

【需要展示/绑定的字段】
· bwSingle ｜ bwDouble ｜ color ｜ a3Extra ｜ memberRates ｜ nightDiscount

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/settings/price
· /wxapp/admin/settings

【交互跳转】
· 保存 → 提示成功

【必须覆盖的状态】
· 校验失败 ｜ 保存中

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 渠道代理 · `pages/merchant/agents`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 渠道代理 / 代理收入（profit_sharing 9 接口） |
| 页面目标 | 管理下级代理与分佣比例 |
| 接口 | `/wxapp/admin/agents`<br>`/wxapp/admin/agents/rate` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】渠道代理
【路径】pages/merchant/agents
【后台功能】渠道代理 / 代理收入（profit_sharing 9 接口）
【页面目标】管理下级代理与分佣比例

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 汇总卡（代理数 / 本月分佣 / 待结算）
2. 代理列表（昵称 + 手机号脱敏 + 邀请人数 + 分佣比例 + 状态）
3. 分佣设置（比例滑块）
4. 收益明细入口

【需要展示/绑定的字段】
· agentCount ｜ commission ｜ pending ｜ agentRate ｜ agents[]

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/agents
· /wxapp/admin/agents/rate

【交互跳转】
· 收益明细 → merchant/revenue

【必须覆盖的状态】
· 无代理 ｜ 比例超限

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 收益明细 · `pages/merchant/revenue`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 代理收入 / 营收统计 |
| 页面目标 | 按日查看分佣与结算状态 |
| 接口 | `/wxapp/admin/profit/list` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】收益明细
【路径】pages/merchant/revenue
【后台功能】代理收入 / 营收统计
【页面目标】按日查看分佣与结算状态

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 筛选（今日/本月/自定义）
2. 汇总（分佣总额 / 已结算 / 待结算）
3. 明细列表（日期 + 来源 + 金额 + 状态）
4. 导出按钮（可选）

【需要展示/绑定的字段】
· dateRange ｜ total ｜ settled ｜ pending ｜ records[]

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/profit/list

【交互跳转】
· —

【必须覆盖的状态】
· 无数据

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 退款处理 · `pages/merchant/refund`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 退款审核（refund 5 接口） |
| 页面目标 | 审核用户退款申请，通过或驳回 |
| 接口 | `/wxapp/admin/refund/list`<br>`/wxapp/admin/refund/approve`<br>`/wxapp/admin/refund/reject` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】退款处理
【路径】pages/merchant/refund
【后台功能】退款审核（refund 5 接口）
【页面目标】审核用户退款申请，通过或驳回

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 状态 tab（待处理 / 已通过 / 已驳回）
2. 申请卡（订单号 + 用户 + 金额 + 原因 + 时间）
3. 处理按钮（同意 / 驳回 + 驳回理由输入）
4. 凭证图片预览

【需要展示/绑定的字段】
· refundNo ｜ orderNo ｜ user ｜ amount ｜ reason ｜ images[] ｜ status

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/refund/list
· /wxapp/admin/refund/approve
· /wxapp/admin/refund/reject

【交互跳转】
· 处理完成 → 列表刷新

【必须覆盖的状态】
· 无可处理 ｜ 处理中

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 提现 · `pages/merchant/withdraw`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 提现审核（finance 4 接口） |
| 页面目标 | 申请提现并查看审核进度 |
| 接口 | `/wxapp/admin/withdraw/apply`<br>`/wxapp/admin/withdraw/list` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】提现
【路径】pages/merchant/withdraw
【后台功能】提现审核（finance 4 接口）
【页面目标】申请提现并查看审核进度

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 可提现余额卡
2. 提现表单（金额输入 + 全部提现 + 到账方式）
3. 申请记录（金额 + 状态 + 时间 + 驳回原因）
4. 规则说明（到账周期 / 最低金额）

【需要展示/绑定的字段】
· withdrawable ｜ amount ｜ status ｜ records[] ｜ minAmount

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/withdraw/apply
· /wxapp/admin/withdraw/list

【交互跳转】
· 提交 → result/result

【必须覆盖的状态】
· 低于最低金额 ｜ 审核中 ｜ 已到账

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 打印机入驻 · `pages/merchant/join`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 门店入驻（platform_apply 5 接口） |
| 页面目标 | 提交入驻资料并跟踪审核 |
| 接口 | `/wxapp/admin/apply/submit`<br>`/wxapp/admin/apply/status` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】打印机入驻
【路径】pages/merchant/join
【后台功能】门店入驻（platform_apply 5 接口）
【页面目标】提交入驻资料并跟踪审核

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 入驻流程条（提交资料 → 审核 → 开通）
2. 表单（门店名 / 联系人 / 手机 / 地址 / 设备型号与数量）
3. 证件上传（营业执照 / 身份证正反面）
4. 提交按钮 + 审核状态卡

【需要展示/绑定的字段】
· storeName ｜ contact ｜ phone ｜ address ｜ machines ｜ licenses[] ｜ status

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/apply/submit
· /wxapp/admin/apply/status

【交互跳转】
· 提交 → result/result

【必须覆盖的状态】
· 资料不全 ｜ 审核驳回（显示原因） ｜ 已通过

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 门店管理 · `pages/merchant/store`　[P0]

| 项 | 内容 |
|---|---|
| 后台功能 | 门店列表 / 编辑 |
| 页面目标 | 增改门店营业信息与营业时间 |
| 接口 | `/wxapp/admin/stores`<br>`/wxapp/admin/stores/save` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】门店管理
【路径】pages/merchant/store
【后台功能】门店列表 / 编辑
【页面目标】增改门店营业信息与营业时间

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 门店列表（门店名 + 地址 + 设备数 + 营业状态开关）
2. 编辑弹层（名称 / 地址 / 电话 / 营业时间 / 经纬度）
3. 新增门店按钮

【需要展示/绑定的字段】
· storeId ｜ title ｜ address ｜ phone ｜ openTime ｜ machines ｜ open

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/stores
· /wxapp/admin/stores/save

【交互跳转】
· 保存 → 列表刷新

【必须覆盖的状态】
· 地址解析失败 ｜ 保存中

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 设备绑定 · `pages/merchant/bind`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 设备列表 |
| 页面目标 | 扫码绑定打印机到门店 |
| 接口 | `/wxapp/admin/machines/bind`<br>`/wxapp/admin/machines/lookup` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】设备绑定
【路径】pages/merchant/bind
【后台功能】设备列表
【页面目标】扫码绑定打印机到门店

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 扫码区（大按钮 + 手输序列号）
2. 设备信息预览（型号 / 固件 / 上次在线）
3. 绑定目标门店选择
4. 绑定按钮

【需要展示/绑定的字段】
· serial ｜ model ｜ firmware ｜ lastSeen ｜ storeId

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/machines/bind
· /wxapp/admin/machines/lookup

【交互跳转】
· 绑定成功 → merchant/devices

【必须覆盖的状态】
· 序列号无效 ｜ 已被绑定

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 内容管理 · `pages/merchant/cms`　[P1]

| 项 | 内容 |
|---|---|
| 后台功能 | 资料库（word 11 接口） |
| 页面目标 | 维护小程序端资料库与模板货架 |
| 接口 | `/wxapp/admin/word/list`<br>`/wxapp/admin/word/save` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】内容管理
【路径】pages/merchant/cms
【后台功能】资料库（word 11 接口）
【页面目标】维护小程序端资料库与模板货架

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 内容列表（封面 + 标题 + 分类 + 状态 + 排序）
2. 新增/编辑（标题 / 分类 / 封面 / 文件 / 是否上架）
3. 批量上下架

【需要展示/绑定的字段】
· docId ｜ title ｜ category ｜ cover ｜ status ｜ sort

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/word/list
· /wxapp/admin/word/save

【交互跳转】
· 保存 → 列表刷新

【必须覆盖的状态】
· 上传中 ｜ 分类为空

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 远程维护 · `pages/merchant/remoute`　[P2]

| 项 | 内容 |
|---|---|
| 后台功能 | 远程维护（machine 23 接口） |
| 页面目标 | 下发重启/校准/清队列等远程指令 |
| 接口 | `/wxapp/admin/machines/{id}/command`<br>`/wxapp/admin/machines/{id}/logs` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】远程维护
【路径】pages/merchant/remoute
【后台功能】远程维护（machine 23 接口）
【页面目标】下发重启/校准/清队列等远程指令

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 设备选择
2. 指令列表（重启 / 校准 / 清队列 / 打印测试页 / 更新固件）
3. 执行记录（时间 + 指令 + 结果）

【需要展示/绑定的字段】
· machineId ｜ command ｜ result ｜ time

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/machines/{id}/command
· /wxapp/admin/machines/{id}/logs

【交互跳转】
· —

【必须覆盖的状态】
· 设备离线 ｜ 指令超时

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```


## 纸张统计 · `pages/admin/printpage`　[P2]

| 项 | 内容 |
|---|---|
| 后台功能 | 设备/统计（stats 23） |
| 页面目标 | 按设备与纸张规格统计消耗 |
| 接口 | `/wxapp/admin/stats/paper` |

**提示词（直接复制）**

```text
【任务】生成一个微信小程序页面 UI（uni-app + Vue3 + `<script setup>` + SCSS，750rpx 基准）

【页面】纸张统计
【路径】pages/admin/printpage
【后台功能】设备/统计（stats 23）
【页面目标】按设备与纸张规格统计消耗

【设计规范】（必须严格遵守）
· 配色：主色 #864D61 ｜ 主色浅底 #FFD6E2 ｜ 次色 #3A637C ｜ 紫 #6F5092 ｜ 薄荷 #C1F0DB ｜ 蜜桃 #FFD1BA ｜ 暖黄 #FFF4BD
· 中性：页面底 #FFF8F8 ｜ 卡片白 #FFFFFF ｜ 正文 #514347 ｜ 次要 #8A7A7F ｜ 描边 #F0E4E7
· 圆角：卡片 24rpx ｜ 胶囊/标签 9999rpx ｜ 小元素 12rpx
· 阴影：0 8rpx 24rpx rgba(134,77,97,.08)（卡片）/ 0 2rpx 8rpx rgba(134,77,97,.06)（浮层）
· 字号：只用 24 / 28 / 32 / 36 / 44rpx（正文 28，标题 32，大数字 44）
· 间距：只用 8 / 16 / 24 / 32 / 48rpx
· 字体：Plus Jakarta Sans（数字与英文）+ 系统中文字体
· 单位：rpx（750 设计稿基准）

【页面结构】（从上到下按此顺序）
1. 时间筛选
2. 汇总卡（总页数 / A4 / A3 / 相纸）
3. 设备维度表（设备名 + 页数 + 占比条）
4. 导出

【需要展示/绑定的字段】
· dateRange ｜ total ｜ byPaper ｜ byMachine[]

【数据来源接口（只做调用，不要写死数据）】
· /wxapp/admin/stats/paper

【交互跳转】
· —

【必须覆盖的状态】
· 无数据

【硬性要求】
1. 不要使用任何 emoji 当图标；图标统一写 `<mf-icon name="xxx" size="32" />`（本地 PNG 图标组件）
2. 卡片不要用超大圆角（禁止 9999rpx 卡片），胶囊只用于标签/按钮/头像
3. 禁止大面积渐变背景（仅头图/礼包头允许）
4. 每个列表页必须实现：加载中 / 空态 / 失败重试 三态
5. 金额、页数用 tabular-nums 等宽对齐，右对齐
6. 输出 `<template>` + `<script setup lang="ts">` + `<style scoped lang="scss">` 三段
7. 不要在页面上写死假数据；数据用 ref 声明 + onLoad 里调用接口填充
```
