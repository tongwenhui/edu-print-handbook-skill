---
name: edu-print-handbook-skill
description: 生成高校印刷手册（单文件 HTML 可打印分页，210×285 大16开，打印对开 420×285 拼版），一套内容模型 + 三套版式锁死风格，按「风格关键词」路由——默认无风格词走模版B 杂志编辑风（杂志/编辑风/editorial/magazine/图文混排/文章式）；模版A 简洁教育×极简瑞士风（瑞士风/简洁/极简/minimal/swiss/干净）；模版C 东方学报风（学报/书院/东方/竖排/文人/journal/宋体/宣纸/印章）。三套共用同一内容 JSON 模型与 9 套多专业主题色板（hex+CMYK 双值，第 9 套朱砂为模版 C 专属）、MiSans+阿里普惠可商用字体、@media print 直接出对开 PDF；换专业只换内容与配色，换风格只换发起词，版式永不漂移。当用户需要制作职业发展手册/就业指导手册/人才培养方案手册/专业认知手册等高校印刷品，或提到上述风格关键词时使用。
---

# 高校印刷手册 Skill（三风格路由版）

> 一套**版式锁死**的高校印刷品生成系统：**一套内容模型（JSON）+ 三套视觉风格**。
> 同一本手册（职业发展 / 就业指导 / 人才培养方案…）可以按喜好任选风格重输出，
> 换专业换内容与配色，换风格换发起词——页面物理规格、骨架、图表纪律全部不变。

## 路由（第一步，必做）

**根据用户请求中的「风格意愿」命中唯一模版，然后只加载该模版目录下的资源——不要同时读三套。**

| 风格关键词（用户表达的风格倾向） | 模版 | 目录 | 入口文档 |
|------|------|------|----------|
| 瑞士风 / 简洁 / 极简 / 干净 / minimal / swiss / neutral | **A · 简洁教育 × 极简瑞士风** | `templates/A-career/` | `templates/A-career/README.md` |
| 杂志 / 编辑风 / 杂志编辑 / 内容页 / 青志识 / 图文混排 / 文章式 / editorial / magazine / liker / 默认可（无风格词） | **B · 杂志编辑风**（默认） | `templates/B-editorial/` | `templates/B-editorial/README.md` |
| 学报 / 书院 / 东方 / 文人 / 竖排 / 宋体 / 宣纸 / 印章 / journal / oriental | **C · 东方学报风** | `templates/C-journal/` | `templates/C-journal/README.md` |

路由规则：

1. 用户表达的是内容需求（要做什么手册）+ 可选风格词。**先命中风格模版，再按该模版 README 定内容**。
2. 风格词命中多套或未命中任何套时，**默认走模版 B（杂志编辑风）**；用户明确点名再换风格，不猜。
3. 内容类型（职业发展 / 就业指导 / 培养方案 / 专业认知…）不影响路由——三套都能做同族内容，区别只在视觉。
4. 同一份内容 JSON 可在三套间互转（物理规格与内容模型同族），把「换风格」等同于「换种子 + 骨架 + 脚本」。
5. 各模版工作流结构相同（澄清 → 选主题 → 写内容 JSON → 构建 → 校验 → checklist 自检 → 打印核对），差异集中在 `references/layouts.md` 的骨架与 `assets/template.html` 的视觉层。

## 三套风格速览（同一内容模型，同一物理规格）

| | A · 极简瑞士风 | B · 杂志编辑风 | C · 东方学报风 |
|---|---|---|---|
| 物理规格 | 210×285 大16开，对开 420×285，横版滚动 / 逐对开打印 | 同左（与 A 同规格，保证内容互通） | 同左 |
| 风格调性 | 无衬线、直角纯色、hairline、单一 accent、极致留白 | 大标题页码 / 眉脚 / 引文块 / 图文混排 / 分栏网格 / masthead | 宣纸底 + 墨色正文 + 朱砂唯一点缀、竖排目录、印章、文武线、中文序数 |
| 骨架编号 | L00–L11 | M00–M17 | X00–X18 |
| 构建脚本 | `scripts/build-handbook.mjs` | `scripts/build-magazine.mjs` | `scripts/build-journal.mjs` |
| 校验脚本 | `scripts/validate-handbook.mjs` | `scripts/validate-magazine.mjs` | `scripts/validate-journal.mjs` |

## 共享资源（三套共用，勿复制）

```
edu-print-handbook-skill/
├── SKILL.md                 ← 你正在读（风格路由 + 通用纪律）
├── PLAN.md                  ← 三风格实施计划（B/C 落地蓝本）
├── _shared/
│   ├── fonts/               ← 可商用字体（MiSans + 阿里普惠 + IBM Plex Mono + Noto Serif SC，46MB 只此一份）
│   └── themes.md            ← 9 套预设主题色板（hex + CMYK 双值，三套共用）
└── templates/
    ├── A-career/  (瑞士风)  ← assets/template.html + references/ + scripts/ + content/ + README.md
    ├── B-editorial/ (杂志风) ← 同构
    └── C-journal/ (东方学报风) ← 同构
```

- 主题色一律从 `_shared/themes.md` 9 选 1（`academy-blue` / `engineering-terra` / `eco-pine` / `academic-teal` / `swiss-ochre` / `burgundy` / `slate-blue` / `deep-aubergine` / `cinnabar`），**只许选不许自定义 hex**；构建脚本自动注入 token。`cinnabar` 朱砂为模版 C 专属（宣纸底 + 墨 + 朱砂唯一点缀）。
- 各模版的构建脚本会自动把 `_shared/fonts/` 同步到生成物旁，保证打印内嵌。
- 内容 JSON **结构同族**：`meta.std=0.1` + cover + chapters（job/table 等模块按内容类型选配）。三套互转时只需在目标模版重跑 build，不必重打内容。

## 通用工作流（三套同构，细节见各自 README）

1. **路由**：按上表命中风格模版，进入对应目录读 README。
2. **澄清**：手册类型与专业名/学校名/年份、主题色、源文档、风格确认（用户带风格词则已锁定）、硬约束——不基于猜测开写。
3. **写内容 JSON**：结构化进该模版 `content/<专业>.json`（参考各自 example；可复用其它模版的 JSON）。
4. **构建**：`node templates/<X>/scripts/build-*.mjs content/<专业>.json <输出.html>`。
5. **校验**：`node templates/<X>/scripts/validate-*.mjs <输出.html>`，0 错误才算过。
6. **checklist 自检**：对照该模版 `references/checklist.md`，P0 项必须全过。
7. **打印核对**：浏览器「另存为 PDF」核对物理尺寸与拼版。

**铁律**：绝不在生成的 HTML 上手改（下次生成会被覆盖）；改样式 = 改该模版 `assets/template.html` 或构建脚本后重新生成。

## 核心设计原则（三套共同遵守；违反任何一条都会从「印刷品」掉回「网页截图」）

1. **单主题单 accent** — 一份印刷品只用一套主题色（`_shared/themes.md` 9 选 1），accent 小面积点缀，禁止大面积铺色。
2. **直角纯色** — 不允许渐变 / 阴影 / 圆角（hairline 分割线除外）；B/C 可玩更大字号与撞色，但克制在装饰层。
3. **网格至上** — 8px 基线网格，对齐严谨；B「留白与分栏」、C「故意错位」都是建立在对齐自知之上的有意选择。
4. **数据由数据计算** — 图表长度全部由 `--v/--max` 计算；每个数据点有类别标签 + 数值标签。
5. **一页（一对开）一个主体** — 固定内容块不跨页自然流动；溢出先删内容/拆页，不压字号硬塞。
6. **印刷先于屏幕** — 出血 3mm、安全区、字体本地化嵌入为 P0。
7. **三套共享一个物理规格** — 都出 210×285 对开，内容 JSON 可互转；风格差异全部封装在模版层。

## 版权与素材（GitHub 发布前必读）

- **字体版权声明**：全部内置字体为可商用字型，各字体来源与许可见 `_shared/fonts/README.md`（MiSans / Alibaba PuHuiTi 3 / IBM Plex Mono / Noto Serif SC）。**请保留该声明文件随仓库发布。**
- **镜像分享说明**：本仓库可安全公开分享（镜像 / 同行评审 / 教学演示）——三套模版内置的是**中性占位校徽**（`_shared/placeholders/logo-placeholder.svg`）与**仅作版式演示的示例插图**。**正式交付或送印前必须替换**：① 占位校徽 → 真实校徽（白色版 / 墨色单色版）；② 示例插图 → 真实岗位 / 场景照片。各模版 README Step 4 与 checklist.md 均有对应 P0 项。
