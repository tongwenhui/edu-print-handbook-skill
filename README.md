# edu-print-handbook-skill

<p align="center">
  一套<strong>版式锁死</strong>的高校印刷品生成系统：**一套内容模型（JSON）+ 三套视觉风格**，以 Claude Code / opencode / WorkBuddy 等 Agent Skills 形式交付。
</p>

生成**单文件 HTML** 可打印分页手册：210×285 大16开单页，打印对开 420×285 拼版（含 3mm 出血），`@media print` 直接「另存为 PDF」得到对开成品。屏幕纵向滚动一屏一卡，打印逐对开输出，屏幕所见 = 打印所得。

换专业只换「内容清单 JSON + 主题色 token」，换风格只换发起词——页面物理规格、骨架、图表纪律全部不变。

<p align="center">
  <img src="docs/preview-3styles.png" alt="三风格封面预览" width="100%">
</p>

## 效果

- **三套版式锁死风格**：极简瑞士风 / 杂志编辑风 / 东方学报风，同一份内容 JSON 可在三套间一键互转，区别只在视觉层
- **印刷级物理规格**：210×285 大 16 开单页、420×285 对开拼版、3mm 出血、安全区，`@media print` 直接「另存为 PDF」得到对开成品
- **单文件交付**：一个 .html 自包含全部样式与字体，浏览器打开即成品，发送即终稿
- **一套内容模型**：`meta.std=0.1` 结构同族，换专业只换内容 JSON + 主题色 token
- **9 套多专业主题色板**：hex + CMYK 双值（第 9 套「朱砂」为 C 风格专属），构建时自动注入
- **内置可商用字体**：MiSans / Alibaba PuHuiTi 3 / IBM Plex Mono / Noto Serif SC，打印自动内嵌
- **构建 + 校验双脚本**：build 出稿、validate 把关，0 错误才算过；checklist P0 自检
- **8px 基线网格**：数据由数据计算（`--v/--max`），一页一对开一个主体，版式永不漂移

## 30 秒开始

```bash
# 1. 安装（把仓库克隆到你的 skills 目录，这里以个人级为例）
git clone https://github.com/tongwenhui/edu-print-handbook-skill.git ~/.workbuddy/skills/edu-print-handbook-skill

# 2. 然后直接对你的 AI 助手说：
# 「帮我做一本 XX 大学的职业发展手册，杂志编辑风，主题色用学院蓝」
```

不装也可以先看效果：打开 `templates/B-editorial/prototype/` 下的原型 HTML，或在仓库「三套风格」表格里选个触发词让 AI 直接生成。

## 三套风格（按「风格关键词」路由）

| 风格 | 触发词 | 骨架 | 气质 |
|---|---|---|---|
| **A · 简洁教育 × 极简瑞士风** | 瑞士风 / 简洁 / 极简 / 干净 / minimal / swiss | L00–L11 | 无衬线、直角纯色、hairline、单一 accent、极致留白 |
| **B · 杂志编辑风**（默认） | 杂志 / 编辑风 / editorial / magazine / 图文混排 / 文章式 / 无风格词默认 | M00–M17 | 刊头 masthead、衬线大标题、pull-quote、folio 眉脚 |
| **C · 东方学报风** | 学报 / 书院 / 东方 / 竖排 / 宋体 / 宣纸 / 印章 / journal | X00–X18 | 宣纸底 + 墨色正文 + 朱砂唯一点缀、竖排目录、印章、文武线 |

路由规则：用户带风格词先命中模版；未命中或命中多套时**默认走模版 B**，用户明确点名再换风格，不猜。

## 适合 / 不适合

| 适合 ✔ | 不适合 ✘ |
|---|---|
| 职业发展手册 / 就业指导手册 | 需要 Word / PPT / 在线编辑格式的文档 |
| 人才培养方案 / 专业认知 / 招生简章 | 需要交互、视频等电子阅读体验的内容 |
| 新生手册 / 活动画册 / 学生手册 | 需要自定义任意版式的「自由排版」需求 |
| 需要送印刷厂的实体手册 | 单页非对开、无出血规格的轻量文档 |

## 示例请求（可直接复制）

```
帮我做一本「新能源汽车工程」专业的职业发展手册，极简瑞士风，主题色 eco-pine，20 页左右。
```

```
生成一份 XX 大学的就业指导手册，杂志编辑风。内容我稍后把源文档发给你。
```

```
我要一本学报风格的新生手册，竖排目录加印章那种，主题色用朱砂。
```

```
把这份内容 JSON 从杂志风转成东方学报风重新出稿。
```

## 为什么是「单文件 HTML」

- **零依赖**：一个 .html 自带全部样式与字体，浏览器打开即成品，无需设计软件
- **屏幕 = 打印**：纵向滚动一屏一卡，`@media print` 逐对开输出，所见即所得
- **可版本化**：HTML 是文本，进 git、diff、review 都方便，改稿可追溯
- **交付即终稿**：生成物自包含，发送文件就是发送成品

## 安装

**方式一：git clone（推荐）**

```bash
# 个人级（所有项目可用）
git clone https://github.com/tongwenhui/edu-print-handbook-skill.git ~/.workbuddy/skills/edu-print-handbook-skill

# Claude Code
git clone https://github.com/tongwenhui/edu-print-handbook-skill.git ~/.claude/skills/edu-print-handbook-skill

# 项目级（仅当前项目，团队共享）
git clone https://github.com/tongwenhui/edu-print-handbook-skill.git ./.workbuddy/skills/edu-print-handbook-skill
```

**方式二：把这段话发给你的 AI**

> 帮我安装 edu-print-handbook-skill。请把 https://github.com/tongwenhui/edu-print-handbook-skill 克隆到 ~/.workbuddy/skills/edu-print-handbook-skill，安装完成后检查 SKILL.md 和 templates/ 目录是否完整。

## 使用流程（开发者）

```bash
# 1. 写内容清单 JSON（参考 templates/<X>/content/example-xinnengyuan.json）
# 2. 构建
node templates/B-editorial/scripts/build-magazine.mjs \
  templates/B-editorial/content/example-xinnengyuan.json \
  ./新能源汽车工程-职业发展手册.html
# 3. 校验（0 错误才算过）
node templates/B-editorial/scripts/validate-magazine.mjs \
  ./新能源汽车工程-职业发展手册.html
# 4. 浏览器打开 → 另存为 PDF → 核对 432×291mm 对开拼版
open ./新能源汽车工程-职业发展手册.html
```

完整工作流（澄清 → 选主题 → 写 JSON → 构建 → 校验 → checklist 自检 → 打印核对）见各模版 `README.md` 与 `SKILL.md`。

## 核心设计原则

1. **单主题单 accent** — 一份印刷品只用一套主题色，accent 小面积点缀，禁止大面积铺色
2. **直角纯色** — 不允许渐变 / 阴影 / 圆角（hairline 分割线除外）
3. **网格至上** — 8px 基线网格，对齐严谨；「留白」与「故意错位」都是建立在对齐自知之上的有意选择
4. **数据由数据计算** — 图表长度全部由 `--v/--max` 计算，每个数据点有类别 + 数值标签
5. **一页（一对开）一个主体** — 溢出先删内容/拆页，不压字号硬塞
6. **印刷先于屏幕** — 出血 3mm、安全区、字体本地化嵌入为 P0
7. **三套共享一个物理规格** — 都出 210×285 对开，内容 JSON 可互转

## 目录结构

```
edu-print-handbook-skill/
├── SKILL.md                 ← 风格路由 + 通用纪律（Skill 入口）
├── PLAN.md                  ← 三风格实施计划
├── _shared/                 ← 三套共用，勿复制
│   ├── fonts/               ← 可商用字体（见 fonts/README.md 版权声明）
│   ├── placeholders/        ← 中性占位校徽（logo-placeholder.svg）
│   └── themes.md            ← 9 套主题色板（hex + CMYK）
└── templates/
    ├── A-career/            ← 模版 A（瑞士风）：README + assets/template.html + references/ + scripts/ + content/
    ├── B-editorial/         ← 模版 B（杂志风）：同上
    └── C-journal/           ← 模版 C（学报风）：同上
```

每个模版目录**自包含**：`assets/template.html`（单文件种子，类名唯一来源）、`references/`（layouts 骨架库 + checklist）、`scripts/`（build + validate）、`content/`（示例 JSON）。

## 主题色预设

| 主题 | 定位 | 风格 |
|---|---|---|
| academy-blue / engineering-terra / eco-pine / academic-teal | 教育 · 理工 · 生态 · 学术 | A / B 通用 |
| swiss-ochre / burgundy / slate-blue / deep-aubergine | 瑞士 · 酒红 · 藏蓝 · 深紫 | A / B 通用 |
| cinnabar（朱砂） | 宣纸底 + 墨 + 朱砂唯一点缀 | C 专属 |

主题色一律从 `_shared/themes.md` 9 选 1，**只许选不许自定义 hex**；构建脚本自动注入 token。

## 字体版权声明

内置字体均为可商用字型：MiSans、Alibaba PuHuiTi 3、IBM Plex Mono、Noto Serif SC。来源与许可明细见 **[`_shared/fonts/README.md`](_shared/fonts/README.md)**——随仓库发布请保留该文件。

## 镜像分享说明

本仓库可安全公开分享（镜像 / 同行评审 / 教学演示）：

- 三套模版内置的是**中性占位校徽**（`_shared/placeholders/logo-placeholder.svg`）与**仅作版式演示的示例插图**（各模版 `content/example-*.json` 引用的 `images/` 配图）。
- **正式交付或送印前必须替换**：① 占位校徽 → 真实校徽（A/B 白色版、C 墨色单色版）；② 示例插图 → 真实岗位 / 场景照片（全出血 ≥ 300dpi，插图 ≥ 200dpi）。
- 各模版 `README.md` Step 4 与 `references/checklist.md` 的 P0 项均包含该替换检查。

## 参考作品

- 新能源汽车工程 · 职业发展手册（三模版示例，各 20–22 页）
- 风格参考：Josef Müller-Brockmann 网格系统 / The New York Times Magazine 栏目系统 / 中华书局古籍版式

## FAQ

**能做 Word / PPT 吗？**
不能。本 skill 只产出单文件 HTML 与「另存为 PDF」的对开印刷成品，这是印刷级规格的前提。

**换学校 / 换专业要改代码吗？**
不用。只换内容 JSON（`content/<专业>.json`）与主题色 token，重跑 build 即可。

**做完了想换风格怎么办？**
同一份内容 JSON 在三套间可直接互转，换模版重跑 build 即可，内容不用重写。

**字体商用安全吗？**
安全。全部内置可商用字体，来源与许可明细见 `_shared/fonts/README.md`。

**占位校徽可以直接送印吗？**
不可以。送印前必须替换为真实校徽与实拍图（见「镜像分享说明」），否则无法通过 P0 检查。

**支持哪些 AI 环境？**
支持 Agent Skills 机制的环境：Claude Code、opencode、WorkBuddy 等。

## License

[MIT](LICENSE)
