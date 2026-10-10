# 快速上手 Quickstart

以下 3 个示例对应 `SKILL.md` §18 的三种调用模式。所有示例都遵循 §0 标准流水线:①理解需求 → ②Style Bible → ③路由 → ④Asset Card → ⑤出图 → ⑥校验 → ⑦归档。

---

## A. 只要一张主界面(独立调用)

> **用户**:帮我出一张仙侠手游的主界面

**流程**
1. 无项目 Bible → 用默认 Bible + 用户关键词生成临时 Style Bible(§3.1)。
2. CP2 出 Layout Blueprint(含文字占位区,§7.2)。
3. CP3 出 Asset Card(含 `text_strategy`,§13)。
4. 出图:画面留空文字(negative 加 `readable words`,§7.5)。
5. 文字分层叠加 → 验收(§17)→ 交付(§16)。

**交付物**
```
/deliverables/{project}/04_main_screens/
  main-screen.png            # 1080×1920 精修
  main-screen-layers.psd     # 背景/角色/UI/特效 4 层
  main-screen-annotated.png  # 标注间距/组件 ID
```

---

## B. 从零搭一套(链式调用)

> **用户**:我要从零做一款二次元卡牌手游的视觉体系

**流程**:`M0 Style Bible → M8 Logo → M1 UI Spec → M2 组件库 → M3 切图 → M7 角色 → M4 主界面 → M6 图标 → M5 场景 → M9 首发运营`
每个 Checkpoint(§15)经用户确认后再进下一环。

**交付物**:见 `SKILL.md` §16 完整目录标准。

---

## C. 单次运营 campaign(复用 Bible)

> **用户**:下周要上 SSR 新角色,出一套开屏 + banner + 方图

**流程**
1. 读取项目 Bible + 角色卡(§10.2)。
2. 判断驱动力"新角色曝光"(§12.2)。
3. 生成 3 尺寸 Asset Card(标题区留空,§7.5)。
4. 出图 → 文字/标题排版端补 → 附文案 3 版(copy.md)。

**交付物**
```
/deliverables/{project}/09_campaign/{campaign}/
  splash.png            # 1080×1920
  banner-16x9.png
  campaign-1x1.png      # 社媒方图
  copy.md               # 主标/副标/CTA 3 版
```

---

## 铁律(所有模式)

- 任何一次出图,先出 Asset Card、用户确认后再出图(CP3)。
- 出图 Prompt 必须嵌入 Style Bible 关键词(色板 hex + 材质 + 光影)。
- 关键文字(标题/数值/按钮)不入生成图,分层后处理。
