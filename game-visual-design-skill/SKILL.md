---
name: game-visual-design
version: 3.0
description: |
  游戏美术与运营视觉一体化设计 Skill。覆盖 UI 规范、UI 组件拆解、组件切图、主页面生成、
  场景设计、图标设计、角色设计、Logo 设计、运营视觉(Banner/海报/活动页/开屏) 等模块。
  支持独立调用(单模块出图) 与 链式调用(项目级视觉体系搭建),内建 Style Bible 与
  Asset Card Schema,保证跨模块视觉一致性,并可直接输出 Midjourney / SD / Nano-banana
  等模型可用的 Prompt。
  v3 相对 v2 的增量:新增「0. 标准执行流水线 / 决策树」、参考图反推 Style Bible 的具体方法、
  AI 文字安全策略、多语言本地化与色弱无障碍规范、Bible 版本管理与复用、落地执行工具绑定。
triggers:
  - 帮我设计游戏 UI / 主界面 / HUD / 弹窗
  - 出一套图标 / 技能图标 / 装备图标
  - 设计角色 / 立绘 / Q版 / 三视图
  - 做游戏 Logo / 标题字
  - 出运营 Banner / 活动开屏 / 海报 / 拉新素材
  - 场景原画 / 关卡背景 / 世界观场景
  - 建立视觉规范 / Style Bible / 项目视觉体系
inputs_required:
  - 项目名 / 玩法品类 / 目标受众
  - 世界观关键词 或 参考图 URL
  - 目标平台(iOS / Android / 手游竖屏 / PC 横屏)
  - 分辨率 / 安全区(默认 1080×1920 竖屏,750×1334 iPhone 基准)
  - 已有资产(Logo、主色、字体、角色) —— 有则遵循,无则由 Style Bible 生成
outputs:
  - Style Bible(色板 / 字体 / 材质 / 光影 / 关键词 / 版本记录)
  - Asset Cards(每个模块一张,含 Prompt / 尺寸 / 命名 / 交付格式)
  - 出图 Prompt(中英双语,可直接投喂 MJ / SD / Nano-banana / 本环境 image_gen)
  - 目录化交付清单(figma / png / psd 分层建议)
category: 视觉设计 / 游戏美术 / 运营视觉
---

# 游戏美术与运营视觉设计 Skill(整合版 v3)

> 本 Skill 把三份原始文档合并为**一份可直接作为 SKILL.md 使用**的完整规范:
> ①《游戏美术与运营视觉设计 Skill(增强版)》 ②《游戏 UI 设计规范文档》 ③《增强版 game-visual-design Sub-skill》
> 目标:让 Agent 在任意一个"出图 / 出规范 / 出组件"的需求下,都能沿着相同的路由与模板产出可交付资产。
>
> **v3 阅读建议**:第 0 节是唯一必须从第一步就遵守的流水线;其余各节按路由到的模块点读。不要从头到尾朗读全文。

---

## 0. 标准执行流水线(每次任务必走,先读这里)

所有调用(独立 / 链式 / 复用)统一走以下 7 步,任何一步不通过就停下,不要跳步:

```
① 理解需求 → ② 定 Style Bible → ③ 路由模块 → ④ 出 Asset Card → ⑤ 出图 → ⑥ 校验 → ⑦ 交付归档
    │              │                │            │            │        │        │
    │ 项目/品类/    │ 有则复用(§1.4) │ §2 路由表    │ §13 Schema  │ §3-12  │ §17 验收 │ §16 目录
    │ 受众/平台/    │ 无则生成(§3.1) │             │ 用户确认后  │ 各模块  │ Checklist│
    │ 参考图        │ 或反推(§3.2)  │             │ 再出图      │ Prompt │          │
    └──────────────┴──────────────┴─────────────┴─────────────┴────────┴──────────┴─────────
```

**决策树(快速判断调用模式)**

```
需求进来
├─ 已有该项目的 Style Bible? ── 是 → 直接复用(§1.4),跳到 ③
│                             └─ 否/不确定 → 生成或参考图反推(§3),走 ②
├─ 用户只要"一个模块"的一两张图? ── 是 → 独立调用(§1.1)
├─ 用户要"从零搭一套视觉体系"?  ── 是 → 链式调用(§1.2,M0→M8)
├─ 用户要"把已有主图拆成切图"?  ── 是 → 链式 D→S(M2→M3)
└─ 用户只是"改/审已有资产"?     ── 是 → 复用 Bible,只进对应模块,不改别处
```

**铁律(硬约束)**
1. 任何一次出图,**先出 Asset Card、用户确认后再出图**(CP3)。未经确认不得跳步。
2. 每次出图的 Prompt **必须嵌入 Style Bible 关键词**(色板 hex + 材质 + 光影词),否则视为不合格。
3. AI 出图**默认不含可读关键文字**(见 §7.5 文字安全策略);标题/数值/按钮文字一律分层后处理或单独标注,不得让生成模型写正文。

---

## 1. 独立调用 / 链式调用 / 复用协议

### 1.1 独立调用(Single-Shot)
- 用户只想解决**一个模块**的问题(如只要图标)。
- Agent 检查是否有 Style Bible;**无则用默认 Bible + 用户关键词临时生成**,并在交付里注明"未接入项目 Style Bible,建议后续统一"。
- 输出:1 张 Asset Card + Prompt + 出图建议。

### 1.2 链式调用(Chain / Project Mode)
- 用户要"从零搭一套""统一全项目视觉"。
- 走 **M0 → M8** 全流程,每个 Checkpoint(§15)需用户确认后再进下一环。
- 中间产物统一沉淀到 `/deliverables/{project}/` 目录结构(§16)。
- 链式**默认顺序建议**:M0 Style Bible → M8 Logo → M1 UI Spec → M2 组件库 → M3 切图 → M7 角色 → M4 主界面 → M6 图标 → M5 场景 → M9 运营。可根据项目优先级调整,但 M0 永远第一步。

### 1.3 Style Bible 优先级(硬约束)
1. 用户显式指定 > 2. 项目已有 Bible > 3. 参考图反推 > 4. 默认 Bible

### 1.4 Bible 版本管理与复用(新增)
- 每次 Bible 定稿写入 `style-bible.md`,并记录 `version` 与 `revision log`(变更了什么、为什么)。
- 复用规则:同一 `project` 名下出图**必须引用**该 project 的 Bible;不同 project 之间**不得串用**主色/材质。
- 反推出的 Bible 在 `palette` 每条色值后标注 `source: 参考图1` 等溯源,便于审校。
- 引用格式:Asset Card 中 `style_bible_ref: style-bible.md#v1.2`(见 §13)。

---

## 2. 模块路由表(Module Router)

| ID | 模块 | 触发词 | 前置依赖 | 交付物 |
|---|---|---|---|---|
| **M0** | Style Bible | 视觉体系 / 定调 | 无 | style-bible.md + 色板图 |
| **M1** | UI 设计规范 | UI 规范 / 设计语言 | M0 | ui-spec.md |
| **M2** | UI 组件拆解 | 组件库 / 原子化 | M1 | components.json + 组件图 |
| **M3** | 组件切图 | 切图 / 九宫格 / @2x@3x | M2 | /slices/*.png |
| **M4** | 主页面生成 | 主界面 / 首页 / HUD | M1(+M2) | main-screen.png + 分层 psd |
| **M5** | 场景设计 | 场景 / 背景 / 关卡 | M0 | scene-*.png |
| **M6** | 图标设计 | 图标 / icon / 技能图标 | M0 | /icons/*.png |
| **M7** | 角色设计 | 角色 / 立绘 / 三视图 | M0 | character-*.png |
| **M8** | Logo 设计 | Logo / 标题字 | M0 | logo.svg + logo.png |
| **M9** | 运营视觉 | Banner / 开屏 / 活动页 | M0(+M8) | banner-*.png |

调用示例:`M0 → M1 → M2 → M3 → M4`(搭一套可交互的主界面)
`M0 → M8 → M9`(做一次运营 campaign)

> 注意:路由表用**统一 M 编号**,与第 0 节决策树一一对应;不要在交付物里混用"P/I/C/L"等旧缩写,避免歧义。

---

## 3. Style Bible(M0)

### 3.1 必填字段
```yaml
project: 项目代号
version: v1.0                    # 每次定稿 +1,记录在 revision log
genre: [卡牌/MMO/SLG/休闲/二次元/…]
audience: [年龄/性别/文化圈层]
tone: [热血/治愈/暗黑/赛博/国风/…]
palette:
  primary: "#FFB300"      # 主色  (source: 无/参考图N)
  secondary: "#1E1E2E"    # 辅色
  accent: "#00E5FF"       # 点缀
  neutral: ["#F5F5F7","#8A8A99","#1A1A1F"]
typography:
  title: "站酷酷黑 / Cinzel"
  body:  "苹方 / Inter"
  fallback: ["PingFang SC", "Microsoft YaHei", "Noto Sans SC"]  # 新增:中文字体回退链
material: [金属/玉石/丝绸/赛博玻璃/…]
lighting: [高对比逆光/柔和顶光/霓虹反射/…]
mj_keywords: "high fantasy, rim light, volumetric fog, ornate gold trim, 8k, artstation"
mj_negative: "low quality, watermark, text, blurry"
revision_log:
  - { v: v1.0, date: "2025-01-01", note: "初版" }
```

### 3.2 参考图反推 Style Bible(新增 · 具体方法)
当用户只给参考图、没有现成 Bible 时,按以下步骤反推,并把溯源写进 palette:
1. **取色**:对每张参考图提取主色 / 辅色 / 点缀色(取最典型 3 个 hex + 中性灰),记录 `source: 参考图N`。
2. **定材质**:看画面高频材质(金属 / 布料 / 玻璃 / 玉石 / 丝绸),写成关键词。
3. **定光影**:看光源方向与氛围(逆光 / 顶光 / 霓虹 / 暖调),写成 lighting 词。
4. **定风格**:看渲染风格(写实 / 二次元 / 半写实 / 像素),转成 mj_keywords 前缀。
5. **沉淀**:生成 `style-bible.md` + `palette.png`,并在 Checkpoint CP1 请用户确认色板与关键词;有歧义(如"偏暗"还是"偏亮")以用户补充为准。
6. **防串用**:反推出的 Bible 只服务于当前 project,不跨项目复用(§1.4)。

### 3.3 输出物
- `style-bible.md`(以上 yaml + 参考图 mood board 3–6 张)
- `palette.png`(色板矩形图,标注 hex)
- `style-bible-thumb.png`(320×320 缩略,用于 Asset Card 引用)

---

## 4. UI 设计规范(M1)

> 沿用《游戏 UI 设计规范文档》主干,并做手游竖屏优化。新增本地化与无障碍两条规范。

### 4.1 基础网格与安全区
- 基准分辨率:**1080 × 1920**(9:16 竖屏);横屏项目改 1920×1080。
- 8pt Grid:所有间距为 8 的倍数(4pt 可作 icon 内边距例外)。
- 安全区:上 88pt(状态栏 + 刘海)/ 下 68pt(Home Indicator)/ 左右 24pt。
- 触控热区:最小 **44 × 44 pt**,主 CTA 建议 88pt 高。
- 横屏项目安全区:左右各加横向留白,适配手柄 / 刘海区域,详见项目 ui-spec。

### 4.2 排版层级
| Level | 用途 | 字号(pt) | 字重 | 行高 |
|---|---|---|---|---|
| H1 | 页面标题 | 44 | Bold | 1.2 |
| H2 | 模块标题 | 32 | Bold | 1.25 |
| H3 | 卡片标题 | 24 | Medium | 1.3 |
| Body | 正文 | 28 | Regular | 1.4 |
| Caption | 说明/时间 | 22 | Regular | 1.35 |
| Number | 数值(血/金币) | 32–56 | Black + 描边 | 1 |

**本地化排版(新增)**
- 每种字号**预留 30% 文本溢出空间**:标题/按钮文案按最长语种(德文/日文常更长)校验,不因翻译撑破容器。
- 中文字体必须给 **fallback 回退链**(见 §3.1),避免缺字成"豆腐块"。
- RTL 语言(阿拉伯 / 希伯来):布局镜像,主 CTA 从右到左;Asset Card 里标注 `rtl: true`。
- 数值/时间用「文本 + 图标」而非纯文字,便于数字本地化(千分位 / 小数点差异)。

### 4.3 颜色 Token
```json
{
  "color.brand.primary": "#FFB300",
  "color.brand.primary.pressed": "#E09B00",
  "color.text.default": "#F5F5F7",
  "color.text.subtle": "#8A8A99",
  "color.bg.card": "rgba(30,30,46,0.85)",
  "color.state.success": "#00C853",
  "color.state.warning": "#FF9100",
  "color.state.danger":  "#FF3D57",
  "color.rarity.n":  "#B0B0B0",
  "color.rarity.r":  "#4FC3F7",
  "color.rarity.sr": "#AB47BC",
  "color.rarity.ssr":"#FFB300",
  "color.rarity.ur": "#FF3D57"
}
```
- 所有取色**走 Token,不硬编码 hex**;跨模块只引用 Token 名。
- 状态色(success/warning/danger)**不得只靠颜色区分**,必须叠加图标 / 形状 / 文案(色弱与色盲用户可读)。

### 4.4 圆角 / 描边 / 阴影
- 圆角:小组件 8pt / 卡片 16pt / 大容器 24pt。
- 描边:主 CTA 2pt outer,颜色为主色 -20% 明度。
- 阴影:`0 4 12 rgba(0,0,0,0.35)`(卡片) / `0 0 24 主色 60%`(高亮辉光)。

### 4.5 动效原则
- 进出场 240ms,easeOutCubic;弹窗 320ms,overshoot 1.05。
- 数值滚动 600ms,ease-out;金币/伤害数字带 particle。
- 按钮反馈:按下 scale 0.96、亮度 -8%、震动 haptic light。
- 动画导出优先 **Lottie(JSON)或序列帧**,避免手写补间导致跨端不一致。

---

## 5. UI 组件拆解(M2)

### 5.1 原子化分层
```
Atom      : Button / Icon / Tag / Badge / Divider / Avatar
Molecule  : ItemCard / SkillSlot / ResourceBar / TabItem
Organism  : TopBar / BottomNav / RewardPopup / BagGrid
Template  : MainScreen / BattleHUD / ShopPage
```

### 5.2 组件字段(components.json)
```json
{
  "id": "btn.primary",
  "name": "主 CTA 按钮",
  "size": {"w": 480, "h": 88},
  "padding": [24, 32],
  "states": ["default","pressed","disabled","loading"],
  "tokens": {
    "bg": "color.brand.primary",
    "text": "color.text.default",
    "radius": 16,
    "shadow": "shadow.card"
  },
  "slots": ["icon-left","label","badge-right"],
  "usage": "全局主要操作,单页最多 1 个",
  "min_hit_area": 44,
  "rtl_mirror": false
}
```

### 5.3 常用组件清单(手游最小集)
- 顶栏 TopBar(头像 / 昵称 / 等级 / 体力 / 金币 / 钻石 / 设置)
- 底部 Tab(首页 / 背包 / 商城 / 社交 / 活动)
- 弹窗 Popup(通用 / 奖励 / 二次确认 / 抽卡结果)
- 卡片 ItemCard(稀有度描边 + 底纹 + 数量 badge)
- 技能槽 SkillSlot(CD 遮罩 + 数字倒计时)
- 资源条 ResourceBar(HP/MP/EXP,支持渐变 + 分段)
- 抽奖轮盘 / 十连按钮 / 卡池切页
- Loading / Toast / GuideMask(新手引导遮罩+镂空)

---

## 6. 组件切图(M3)

### 6.1 命名规范
`{组件}_{状态}_{密度}.png`,如 `btn_primary_pressed@3x.png`
- 密度:@1x / @2x / @3x(手游默认 @3x 出,程序按需缩)
- 九宫格切图需附 `.9.png` 或 `slice.json`(标注可拉伸区域)

### 6.2 交付目录
```
/slices/
  /button/
    btn_primary_default@3x.png
    btn_primary_pressed@3x.png
    btn_primary_disabled@3x.png
  /icon/
  /card/
  /popup/
      popup_bg.9.png
      popup_bg_slice.json
```

### 6.3 切图 Checklist
- [ ] 透明背景 PNG-24
- [ ] 上下左右预留 2px 透明防锯齿
- [ ] 九宫格中心区不含渐变文字
- [ ] 图标统一画布(如 144×144),视觉重心居中
- [ ] 命名全小写 + 下划线,无中文无空格
- [ ] 涉及本地化语言的文字切片单独交付,不烧进背景图

---

## 7. 主页面生成(M4)

### 7.1 输入
- 页面类型(主城 / 战斗 HUD / 背包 / 商城 / 活动首页)
- 需要展示的信息模块(资源、公告、任务、Boss 入口…)
- 已锁定的 Style Bible + 组件库

### 7.2 Layout Blueprint(以主城为例)
```
┌──────────────────────────────┐  ← TopBar(头像+资源)
│                              │
│        [主视觉/主角展示]      │  ← 60% 视觉焦点
│                              │
│  [公告]      [签到]  [活动]   │  ← 侧边浮标
│                              │
│  [任务卡片 · 滚动 3 张]       │
│                              │
├──────────────────────────────┤
│ 主线  副本  Boss  社交  抽卡 │  ← 底部 5 大入口
└──────────────────────────────┘
```
- Blueprint 需明确**文字占位区**(标题/数值/按钮文字的位置与大小),见 §7.5。

### 7.3 出图 Prompt 模板(MJ 系 / Nano-banana)
```
Game main screen UI, {genre} mobile game portrait 9:16,
hero character standing center with {lighting} rim light,
top bar shows avatar / gold / gem / energy,
bottom navigation with 5 icon buttons {主线/副本/Boss/社交/抽卡},
floating quest cards on right side,
palette: {primary hex} {secondary hex} {accent hex},
material: {material keywords},
ornate frames, glowing particles, high detail, artstation trending
--ar 9:16 --style raw
Negative: text artifacts, watermark, extra fingers, low res, readable words
```
> 参数 `--v 6 / --niji 6 / --s xxx` 按**当前所用模型的真实版本**填写,不要照抄;落地细节见 §19。

### 7.4 交付
- `main-screen.png`(1080×1920,一版精修)
- `main-screen-layers.psd`(背景 / 角色 / UI / 特效 4 层)
- `main-screen-annotated.png`(标注间距 / 组件 ID)

### 7.5 AI 文字安全策略(新增 · 所有含 UI 的模块必守)
生成模型在长文字、中文、多行排版上**极易出错**,规则如下:
1. **关键文字不入图**:标题、数值、按钮名、完整句子,**不交给生成模型渲染**,在分层 PSD 里作为独立文字层叠加,或由程序端用字体渲染。
2. 出图 Prompt 一律在 negative 里写 `readable words, text`(如上),让画面保持"文字空白区"。
3. 若用户坚持要"图里带标题",只允许**极短标题**(≤4 个字 / 单行),生成后立即人工核对字形,错字则改用 §1.1 的分层方案。
4. 运营图(§12)的"大标题"同理:优先空出标题区,文案走 copy.md 由设计端排版,确保 3 秒可读且无错别字。

---

## 8. 场景设计(M5)

### 8.1 场景类型
主城 / 副本 / Boss 战 / 剧情演出 / 世界地图 / 关卡缩略

### 8.2 构图三段式
- **前景**:承载互动物件(NPC / 传送门 / 宝箱)
- **中景**:主视觉焦点(建筑 / Boss / 地标)
- **远景**:氛围与世界观(山脉 / 城市天际 / 星空)

### 8.3 Prompt 模板
```
{genre} game scene concept art, {location description},
three-plane composition: foreground {…}, midground {…}, background {…},
{time-of-day} lighting, {weather}, {material palette},
epic scale, cinematic, matte painting style, 16:9
--ar 16:9
```

### 8.4 交付
- `scene-{name}-wide.png`(2560×1440 主图)
- `scene-{name}-thumb.png`(缩略 512)
- 可选:分层 PSD(天空 / 远景 / 中景 / 前景 / 光效)

---

## 9. 图标设计(M6)

### 9.1 分类
- 功能图标(设置 / 邮件 / 返回)- 线性,24–48pt
- 技能图标 - 半写实,128–256pt,带外框
- 装备/道具图标 - 写实带材质,含稀有度底
- 货币/资源图标 - 3D 微立体,发光

### 9.2 统一规范
- 画布:功能 48×48 / 技能 144×144 / 装备 160×160
- 视觉重心:几何中心 ± 4px
- 外描边:2–4px,颜色与稀有度绑定
- 出图批量:一组 6–12 个,同一 seed / 同一光影(保持一致性,见 §13 seed)

### 9.3 Prompt 模板(技能图标)
```
Game skill icon set, {element theme e.g. fire/ice/lightning},
square 1:1, centered composition, ornate metal frame,
{rarity color} glow, semi-realistic painterly style,
consistent lighting from top-left, black background isolate
--ar 1:1
```

---

## 10. 角色设计(M7)

### 10.1 交付形态
- 立绘(全身 / 半身,透明背景)
- 三视图(正 / 侧 / 背,可选 3/4)
- Q 版头像(圆形裁切 512×512)
- 表情差分(默认 / 喜 / 怒 / 伤 / 惊 6 张起)

### 10.2 角色卡(Character Card)
```yaml
name: 星辰剑姬 · 玖
role: DPS / 近战 / 单体爆发
faction: 星辉联邦
element: 光
silhouette: 修长 + 长剑 + 飘带,识别度靠剑柄星芒
palette: 主色 #E7D9FF 辅色 #6A4CFF 点缀 #FFD54F
costume_keywords: 星纹铠甲, 白金丝绸, 星光粒子
personality: 冷静 / 使命感 / 微傲
```

### 10.3 Prompt 模板
```
Full body character concept art, {name}, {role}, {faction},
{costume keywords}, palette {hex1 hex2 hex3},
dynamic pose, rim light from behind, transparent background,
{art style e.g. anime / semi-realistic / painterly},
high detail face, ornate armor design, 2:3
--ar 2:3
```
> 二次元风格追加 `--niji` 风格参数(以当前模型为准,见 §19)。

### 10.4 一致性要点
- 同一角色的立绘 / 三视图 / Q版 / 差分,**锁定同一 Character Card**(骨架、配色、材质词),避免同一角色在多个交付里"换脸"。

---

## 11. Logo 设计(M8)

### 11.1 输入
- 品牌名(中/英/双语)
- 品类(MMO 需厚重 / 休闲需圆润 / 二次元需灵动)
- 是否含吉祥物 / 图腾

### 11.2 结构
- 主 Logo(标题字 + 图形符号)
- 副标(slogan / 世界观短句)
- 单色版 / 反白版 / 极简 icon 版

### 11.3 Prompt 模板
```
Game logo design, title "{name}", {genre} mobile game,
metallic {primary hex} lettering with {material} texture,
ornate emblem behind text featuring {symbol e.g. dragon/sword/star},
glowing rune particles, black background, centered, 16:9
--ar 16:9
```
> Logo 的**标题字形是文字**,AI 极易写错。若产出字形错误,回退到 §7.5 方案:图形由 AI 出、文字由矢量排版(最终 `logo-master.svg` 必须用矢量重建字形,不依赖生成模型的字)。

### 11.4 交付
- `logo-master.svg`(矢量,程序可缩放)
- `logo-@3x.png`(1024 高分)
- `logo-mono-white.png` / `logo-mono-black.png`
- `logo-usage.md`(最小尺寸 / 安全边距 / 禁用场景)

---

## 12. 运营视觉(M9)

### 12.1 类型
开屏(9:16 全屏)/ Banner(16:9 或 3:1)/ 活动首页(9:16 长图)/
拉新素材(1:1 / 9:16 短视频封面)/ 节日海报(A4 竖版)

### 12.2 转化驱动力(先判断,再出图)
| 驱动力 | 视觉锁定 | 例子 |
|---|---|---|
| 新角色曝光 | 角色 70% + 名字大字 + 稀有度光效 | SSR 上线 |
| 折扣促销 | 数字/百分号最大 + 商品堆叠 | 618 首充 |
| 节日情绪 | 节日符号 + 品牌色 + 角色变装 | 春节 / 圣诞 |
| 拉新裂变 | 利益点(免费领 / 送 SSR) + 二维码/按钮 | 老拉新 |

### 12.3 Prompt 模板(开屏)
```
Mobile game splash screen 9:16, {campaign theme},
hero character in dynamic pose center,
big title "{event name}" with {primary hex} gradient,
subtitle "{slogan}", ornate frame, particle effects,
palette {hex list}, cinematic lighting, high contrast
--ar 9:16
```
> 标题/文案区按 §7.5 处理:AI 只出画面 + 空标题区,文字由排版端补(对应 `copy.md` 的 3 版文案)。

### 12.4 交付
- `campaign-{name}-splash.png`(1080×1920)
- `campaign-{name}-banner-16x9.png`
- `campaign-{name}-1x1.png`(社媒方图)
- `copy.md`(主标 / 副标 / CTA 文案 3 版)

---

## 13. Asset Card Schema(所有模块通用)

每次出图前 Agent 必须先生成一张 Asset Card,用户确认后再出图。

```yaml
asset_id: M4-001
module: 主页面生成 (M4)
project: 星辰纪元
brief: 主城首页,展示主角+资源+5大入口
style_bible_ref: style-bible.md#v1.2
size: 1080x1920
safe_area: top 88 / bottom 68 / lr 24
palette: ["#FFB300","#1E1E2E","#00E5FF"]
material: ["ornate gold","volumetric fog"]
lighting: "rim light from top-left"
text_strategy: "标题/数值分层叠加,画面留空"(§7.5)   # 新增:文字策略必填
prompt_positive: |
  Game main screen UI, high fantasy mobile game portrait 9:16, ...
prompt_negative: "text artifacts, watermark, low res, readable words"
model: <当前落地模型,见 §19>
seed: 1234567
deliver:
  - main-screen.png
  - main-screen-layers.psd
  - main-screen-annotated.png
checkpoint: 需用户确认 Layout Blueprint 后再出图
```

---

## 14. Prompt 通用模板骨架

```
[主体+镜头] + [风格+艺术家] + [材质+光影] + [色板 hex] + [构图+比例] + [细节强度]
--ar {比例} --style raw
Negative: low quality, watermark, text, blurry, extra fingers, jpeg artifacts, readable words
```

**建议比例参数(由落地模型决定,此处为默认)**
- 手游竖屏:`--ar 9:16`
- 横屏场景:`--ar 16:9`
- 图标:`--ar 1:1`
- 风格强度 / 迭代版本(`--s / --v / --niji`):**按当前模型文档填写,不照抄历史版本**(见 §19)。

---

## 15. Checkpoint 机制(强制确认点)

| CP | 时机 | 需用户确认 |
|---|---|---|
| CP1 | Style Bible 定稿 | 色板 / 材质 / 关键词(含反推溯源) |
| CP2 | Layout Blueprint | 组件布局与信息层级 |
| CP3 | Asset Card | Prompt / 尺寸 / 交付格式 / 文字策略 |
| CP4 | 首版出图 | 是否进入精修 / 分层 |
| CP5 | 交付前 | 命名 / 目录 / 切图完整性 |

未经确认,Agent 不得跳步。

---

## 16. 交付目录标准

```
/deliverables/{project}/
  /00_style_bible/
    style-bible.md
    palette.png
    moodboard/*.jpg
  /01_ui_spec/
    ui-spec.md
    tokens.json
  /02_components/
    components.json
    /preview/*.png
  /03_slices/
    /button/  /icon/  /card/  /popup/
  /04_main_screens/
    main-screen-*.png / .psd / -annotated.png
  /05_scenes/
  /06_icons/
  /07_characters/
  /08_logo/
    logo-master.svg
    logo-@3x.png
    logo-usage.md
  /09_campaign/
    /2025-06-618/
      splash.png / banner.png / copy.md
  /_asset_cards/
    M0-*.yaml … M9-*.yaml
```

---

## 17. 验收 Checklist(交付前必过)

**通用**
- [ ] 每个交付物都有对应 Asset Card
- [ ] Prompt / seed / model 已归档,可复现
- [ ] 文件命名符合规范(全小写 / 下划线 / 版本号)
- [ ] 目录结构与 §16 一致

**Style 一致性**
- [ ] 色板 hex 与 Style Bible 一致(±5% 明度容差)
- [ ] 光源方向全局一致
- [ ] 材质关键词跨模块复用
- [ ] 反推色值带 `source` 溯源;Bible 有 version 与 revision log

**UI / 组件**
- [ ] 触控热区 ≥ 44pt
- [ ] 文字对比度 WCAG AA(≥ 4.5:1)
- [ ] 组件状态齐全(default/pressed/disabled)
- [ ] 九宫格切图正确标注可拉伸区
- [ ] 本地化:标题预留 30% 溢出、中文字体有 fallback、RTL 项目标注镜像(§4.2)
- [ ] 状态不只靠颜色区分(叠加图标/形状)(§4.3)

**AI 文字安全**
- [ ] 关键文字(标题/数值/按钮)未交给生成模型,已在分层/排版端处理(§7.5)
- [ ] 画面文字区为空,negative 含 `readable words`

**运营视觉**
- [ ] 主标 / 副标 / CTA 层级清晰(3 秒读懂)
- [ ] 品牌色占比 ≥ 30%
- [ ] 合规:无未授权 IP、无绝对化用词、字体/素材版权有来源

---

## 18. 快速调用示例

**A. 只要一张主界面(独立)**
> 用户:帮我出一张仙侠手游的主界面
> Agent:①快速套默认 Bible + 用户关键词生成临时 Style Bible → ②CP2 出 Layout(含文字占位区)→ ③CP3 出 Asset Card(含 text_strategy)→ ④出图(画面留空文字)→ ⑤文字分层 → ⑥验收 → ⑦交付

**B. 从零搭一套(链式)**
> 用户:我要从零做一款二次元卡牌手游的视觉体系
> Agent:M0 Style Bible → M8 Logo → M1 UI Spec → M2 组件库 → M3 切图 → M7 角色 → M4 主界面 → M6 图标 → M5 场景 → M9 首发运营

**C. 单次运营 campaign**
> 用户:下周要上 SSR 新角色,出一套开屏+banner+方图
> Agent:读取项目 Bible + 角色卡 → 判断驱动力"新角色曝光" → 生成 3 尺寸 Asset Card(标题区留空)→ 出图 → 文字/标题排版端补 → 附文案 3 版

---

## 19. 落地执行(本环境工具绑定 · 新增)

- 本技能产出的 Prompt 是**"提示词语言规范"**,在**豆包办公环境**内实际出图走 `image_gen` / `image_edit`(seedream 系列),不直接投喂 MJ/SD。
- 落地规则:
  1. 调用出图工具前,先把 §3–§12 的 Prompt 按当前工具支持的字段(比例用 width/height 换算,如 9:16 → 1080×1920)转写。
  2. `--ar / --v / --niji / --s` 等 MJ 专属参数**不传给本地工具**,只保留画面描述语义。
  3. 需要修图 / 换元素 / 复刻参考图时用 `image_edit`(传参考图 URL + 编辑指令);从零生成用 `image_gen`。
  4. 分层 PSD / 矢量 svg 由本环境可用工具生成或标注,程序端负责最终合成。
- 若用户指定使用 MJ / SD / Nano-banana,则把 §3–§12 的 Prompt 原样输出给用户,并按其文档补 `--v` 版本号。

---

## 20. 版本与维护

- **v3.0(本次整合)**:新增 §0 流水线与决策树、§3.2 参考图反推法、§7.5 文字安全策略、§4.2 本地化、§4.3 无障碍、§1.4 Bible 版本管理、§19 落地执行绑定;Prompt 参数改为"按当前模型"而非硬编码旧版本号。
- 维护建议:当出现新的出图模型 / 平台安全区 / 无障碍规范时,只更新对应小节并 +1 version,不改动其余模块,保持 Agent 可点读。

*Skill version: v3.0 · 整合日期: 2026 · 维护:视觉设计中台*
