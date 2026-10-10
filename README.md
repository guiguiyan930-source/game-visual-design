# game-visual-design

游戏美术与运营视觉一体化设计 Skill。覆盖 **UI 规范、UI 组件拆解、组件切图、主页面生成、场景设计、图标设计、角色设计、Logo 设计、运营视觉(Banner/海报/活动页/开屏)** 等模块。

支持**独立调用**(单模块出图)与**链式调用**(项目级视觉体系搭建),内建 Style Bible 与 Asset Card Schema,保证跨模块视觉一致性,可直接输出 MJ / SD / Nano-banana 等模型可用的 Prompt。

- 版本:v3.0
- 语言:中文(含中英双语出图 Prompt)
- 类别:视觉设计 / 游戏美术 / 运营视觉

---

## 特性

- **统一执行流水线**:§0 定义了 7 步标准流程 + 决策树,任意需求都能快速判断调用模式。
- **Style Bible 一致性**:色板 / 材质 / 光影 / 关键词跨模块强制复用;支持参考图反推并溯源。
- **AI 文字安全策略**(v3 新增):关键文字不入图,分层叠加,避免生成模型写错字。
- **本地化 & 无障碍**(v3 新增):文本溢出预留、中文字体 fallback、RTL 镜像、色弱可读。
- **落地执行绑定**(v3 新增):说明在豆包办公环境走 `image_gen` / `image_edit`,MJ 专属参数不硬编码。
- **Checkpoint 强制确认**:Style Bible / Layout / Asset Card / 首版 / 交付前五个确认点,未确认不跳步。

---

## 目录结构

```
game-visual-design-skill/
├── SKILL.md          # 技能本体(可直接作为 Agent Skill 使用)
├── README.md         # 本说明
├── LICENSE           # MIT License
├── .gitignore
└── examples/
    └── quickstart.md # 快速上手示例(3 种调用模式)
```

---

## 使用方法

1. 将 `SKILL.md` 放入你使用的 Agent 技能根目录(如 `.skills/game-visual-design/`)。
2. 或用自然语言直接触发,例如:
   - "帮我设计游戏 UI / 主界面 / HUD / 弹窗"
   - "出一套图标 / 技能图标 / 装备图标"
   - "设计角色 / 立绘 / Q版 / 三视图"
   - "做游戏 Logo / 标题字"
   - "出运营 Banner / 活动开屏 / 海报"
   - "从零搭一套项目视觉体系"
3. 所有调用统一走 §0 流水线:先定 Style Bible → 路由模块 → 出 Asset Card → 出图 → 校验 → 交付。

> 详细规则见 `SKILL.md` 的 §0–§20,各模块按路由到的编号点读即可,无需通读全文。

---

## 版本历史

- **v3.0**(2026):新增 §0 流水线与决策树、§3.2 参考图反推法、§7.5 文字安全策略、§4.2 本地化、§4.3 无障碍、§1.4 Bible 版本管理、§19 落地执行绑定;Prompt 参数改为按当前模型填写。

---

## 许可

MIT License.见 `LICENSE` 文件。
