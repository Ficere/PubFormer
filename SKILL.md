---
name: gzh-opinion-article
version: 1.0.0
display_name: 公众号观点长文流水线
display_name_en: WeChat Opinion Longform Pipeline
description: 把一个主题/一段对话/一份素材，端到端产出一篇观点·深度分析类公众号文章并完成红白色系排版。固定使用 gzh-design 的「红白色系」主题与本文固化的配方（引言卡 → 前言 → 三看点导读 → 5–6 个编号章节 → ∞ 结语 → END → 签名区），自带数据卡、来源对照表、金句块、编号列表的用量配额与写稿规范。触发场景：(1) 用户说「写一篇公众号文章」「把这个写成公众号」「按上次那篇的排版来一篇」；(2) 需要把一段讨论/调研结论改写成观点长文；(3) 想要复现红白色系编号章节+数据卡+金句块的排版风格。仅做排版不改内容时转 gzh-design。
description_zh: 公众号观点长文端到端流水线：从主题或对话素材出发，先写出 2500–3500 字的深度分析稿，再用红白色系固定配方排版、校验、生成一键复制预览页。内置写稿规范、组件配额、章节英文标签、署名占位与踩坑清单，保证每篇气质稳定。
description_en: End-to-end pipeline for WeChat opinion longform — turn a topic or conversation into a 2500-3500 word analytical article, then typeset it with the pinned Red-White recipe (intro card, 3-point TOC, numbered sections, data cards, quote blocks, END divider, signature), validate it, and ship a one-click copy preview page.
---

# 公众号观点长文流水线

## 定位

本 skill 是 **gzh-design 的上层封装**：把「写什么」和「排成什么样」一起固化，避免每次重新决策主题、配方和配额。

- 排版能力**全部复用 gzh-design**（主题库、组件库、校验脚本、预览脚本）；本 skill 只负责**决策与流程**，不另造组件、不手写 HTML。
- 适用题材：**观点 / 深度分析 / 科技与社会评论 / 机制拆解**。教程、盘点、访谈请直接用 gzh-design 自选主题。

---

## 固定决策（不要再问用户）

以下四项已固化，直接执行；只有用户明确要求时才偏离。

| 决策项 | 固化值 |
|---|---|
| 排版主题 | gzh-design 的**红白色系**（`references/theme-red-white.md`） |
| 文章类型 | 观点 / 深度分析 |
| 骨架顺序 | 引言卡 → 前言正文 → 三看点导读 → 编号章节 01–05 → ∞ 结语 → END → 签名区 |
| 署名 | `{{作者名}}` + `{{一句话简介}}` 占位，交付时提示替换 |

---

## 工作流

### 1. 收素材 · 补证据

来源可以是：当前对话的结论、用户给的文档、URL、或一句主题。

- 事实性主张必须有出处，先搜索再写；**每篇至少 3 条可核查证据**（研究机构名 / 论文 / 实验数据 / 具名人物原话）。
- 优先收集带**数字**的证据，数据卡和来源对照表需要它们。
- 找不到可靠数据时改写表述，不编造数字与链接。

### 2. 写 Markdown 稿

规范见 [references/writing.md](references/writing.md)。硬性要求：

- 篇幅 **2500–3500 字**；
- 结构：开头 `> 引言金句`（与外标题视角错开）→ 前言 2–3 段 → `##` 章节 5–6 个 → `##` 结语章；
- 每章只讲**一个机制**，配 1–2 个数据或引语；
- 正文标点一律全角，直引号当场转弯引号；
- 存到 `/workspace/{主题slug}.md`。

### 3. 用红白色系排版

调用 `Skill` 工具加载 **gzh-design**，并明确传入：

- 主题：**红白色系**（已选定，跳过它的主题确认提问）；
- 文章类型：观点 / 深度分析；
- 配方：**按 [references/recipe.md](references/recipe.md) 执行**，它是本篇排版的唯一配方来源；
- 参考样例：`assets/example-opinion-red.html`（本次产出的金标准，照它的层次与密度走）。

gzh-design 的硬性红线同样适用且不得违反：所有文字节点 `<span leaf="">` 包裹、禁 `<style>/<script>/<div>/class/id/float/position/grid`、HTML 一律从红白色系组件库取。

### 4. 校验 · 预览

产物落在 `/workspace/{文件名}_排版_红白色系(red-white).html`，然后**必须**跑：

```bash
python3 /root/.codebuddy/skills/skill_2097954066825015296/gzh-design/scripts/validate_gzh_html.py "/workspace/{文件名}_排版_红白色系(red-white).html"
python3 /root/.codebuddy/skills/skill_2097954066825015296/gzh-design/scripts/wrap_preview.py "/workspace/{文件名}_排版_红白色系(red-white).html"
```

**ERROR 和半角标点 WARNING 都必须清零**才交付。最常见的返工点是英文直引号——写 HTML 时直接用弯引号，不要事后替换。

### 5. 交付

`present_files` 依次给：预览页 HTML → 干净正文 HTML → Markdown 稿。文字里说明：打开预览页点右上角「复制到公众号」→ 编辑器粘贴；并提醒替换签名区两个占位符。

---

## 不可违反的配额

超出会让排版变花、失去红白风格的克制感：

- **红色加粗锚点 ≤ 5 处**；核心概念标签（浅红底深红字）`2–4 个`
- **数据卡组最多 1 组**（三列优先）；**表格最多 1 个**（用于数字↔来源对照）
- **粉底左竖条金句块 2 处**（拆前提章 + 机制章各一）；居中金句 ≤ 2 处
- 编号列表每章最多 1 组，每组 3–5 项
- 每段淡粉下划线 `1–3` 处，不整段划线

---

## 踩坑清单

1. 忘 `<span leaf="">` 包裹 → 粘贴后样式整片丢失（靠校验脚本兜底）
2. 直引号 / 半角逗号句号 → 校验报 WARNING，必须清零
3. 红底白字标签只在引言卡内用；正文标签一律浅红底深红字
4. 签名区全文唯一，且在最末尾；原文末尾已有签名段就并入它，不要并存
5. 目录导读是**精选 3 个看点**，不是完整章节列表
6. 结语章编号用 `∞` + `THE END`，中间章节不能用变体编号
7. 图片 `<img>` 用 `max-width:100%;height:auto`，不要 `width:100%` 拉伸小图
8. 代码块每行一个 `<p style="margin:0">`，禁用 `white-space:pre`
9. 不得增删原文实质内容；不凭空编造图片 URL
