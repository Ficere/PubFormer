# PubFormer

将一个主题或一段对话，端到端产出一篇**观点·深度分析类公众号文章**，并完成**红白色系固定配方排版**——这是 [CodeBuddy](https://www.workbuddy.cn) 的一个 Skill。

> "PubFormer" = Publish + Former：把想法**成型**并**发表**。

## 它做什么

`gzh-opinion-article` 是 `gzh-design`（公众号排版引擎）的**上层封装**：排版能力全部复用 gzh-design 的主题库与组件库，本 Skill 只固化「写什么」和「排成什么样」，避免每次重新决策。

流水线：

```
素材/对话/主题 → 补证据（搜索）→ 写 2500–3500 字稿 → 红白色系配方排版
              → 合规校验（ERROR/半角标点清零）→ 一键复制预览页
```

## 固化的决策

| 项 | 值 |
|---|---|
| 排版主题 | gzh-design「红白色系」 |
| 骨架 | 引言卡 → 前言 → 三看点导读 → 01–05 编号章节 → ∞ 结语 → END → 签名区 |
| 章内配方 | 机制章=三列数据卡；拆前提章=编号列表+金句块；数据章=来源对照表；出路章=编号列表+提示条 |
| 配额 | 红色加粗 ≤5、概念标签 2–4、数据卡 1 组、表格 1 个、金句块 2 处 |
| 章节英文标签 | `THE MECHANISM` / `THE DEFINITION` / `THE MATH` / `THE NEUROSCIENCE` / `THE WAY OUT` / `THE END` |
| 署名 | `{{作者名}}` 占位，交付时提示替换 |

## 目录结构

```
SKILL.md                       流程 + 固定决策 + 配额 + 踩坑清单
references/recipe.md           红白色系固定配方（骨架、章内组件、标记层策略）
references/writing.md          写稿规范（篇幅、证据要求、章节节奏模板）
assets/example-opinion-red.html  金标准排版成品（可直接粘贴公众号）
assets/example-opinion-red.md    对应的 Markdown 稿
```

## 安装

```bash
# 复制为一个 skill 目录，名字即 skill 名
cp -r /path/to/PubFormer ~/.codebuddy/skills/gzh-opinion-article
```

前置依赖：需要同时装有 `gzh-design` Skill（提供红白色系组件库与校验/预览脚本）。

## 使用

> 写一篇公众号文章，主题是《XXX》
>
> 按上次那篇的排版来一篇
>
> 把我们刚才讨论的内容改写成公众号长文

只想改排版、不改内容时，直接用 `gzh-design`。

## 样例

`assets/example-opinion-red.md` 是《你不是进了心流，你是在拉老虎机——Vibe Coding 成瘾的神经科学》的成稿，`assets/example-opinion-red.html` 是它排版后的公众号正文片段（打开后可直接全选复制粘贴进公众号编辑器，样式不丢失）。

## License

MIT
