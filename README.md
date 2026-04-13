# Second Brain / 第二大脑

> A personal knowledge base built with Obsidian, synced via GitHub, and maintained by Claude Code.
>
> 基于 Obsidian 的个人知识库，通过 GitHub 同步，由 Claude Code 维护。

---

**EN | [中文](#中文)**

<details>
<summary><b>English</b> — click to collapse</summary>

## Prerequisites

- **[Obsidian](https://obsidian.md/)** — The local-first markdown app that renders your vault
- **[Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview)** — The AI agent that classifies, writes, links, and commits notes on your behalf
- **Git** — For version control and sync

That's it. Claude Code handles all commits, linking, and organization automatically.

## How It Works

```
You → Paste raw content into Claude Code chat
Claude → Classifies, processes, links, and commits
Vault → Grows as a connected knowledge graph
```

You don't need to worry about folders, templates, or linking. Just give Claude content and it figures out where things go.

## Quick Start

### 1. Clone this repository

```bash
git clone https://github.com/hiyuantang/second-brain.git
```

**Do not fork** — just clone. If you want a private copy, create a new private repo and push here instead of to origin.

### 2. Open in Obsidian

Open Obsidian → "Open folder as vault" → select the `Second Brain/` folder.

### 3. Start feeding it content

Paste anything into the Claude Code chat:

```
"Save this article about habit formation: https://..."
"Here are my highlights from Atomic Habits..."
"Idea: compound effects apply to learning too"
"Today I had a meeting with Sarah about the project..."
```

Claude will classify, write notes, link them together, and commit for you.

## Vault Structure

```
Second Brain/
├── Home.md                  # Vault entry point
├── 00 - Inbox/              # Quick capture, pending triage
├── 01 - Daily/              # Daily notes (YYYY-MM-DD.md)
├── 02 - Projects/           # Active projects with deadlines
├── 03 - Areas/              # Ongoing responsibilities
├── 04 - Resources/          # External reference material (articles, books, tools)
├── 05 - Knowledge/          # Permanent notes — your synthesized ideas
├── 06 - Archive/            # Completed and inactive items
├── 90 - Templates/          # Note templates
├── 91 - Attachments/        # Images, PDFs, media
├── 99 - Meta/               # System documentation
└── .obsidian/               # Obsidian configuration
```

The vault combines **PARA** (actionability-based organization) with **Zettelkasten** (atomic, linked permanent notes).

## What You Can Feed Claude

| Content | Example | What Claude Does |
|---------|---------|-----------------|
| Article URL or text | "Save this: https://..." | Creates a Literature Note + extracts Permanent Notes |
| Book highlights | "Highlights from Deep Work..." | Creates a Book Note + key idea notes |
| Quick idea | "Idea: the best way to learn is to teach it" | Creates a Fleeting Note, promotes to Permanent later |
| Meeting notes | "Met with Alex about the new project..." | Creates/updates a Project Note |
| Daily log | "Today I worked on X, learned Y..." | Appends to today's Daily Note |
| Bulk content | "Here are 5 articles I want to save" | Processes each with individual commits |

## Retrieving Info

Open the vault in Obsidian and use:

| Method | Shortcut | Best For |
|--------|----------|----------|
| Quick Switcher | `Cmd/Ctrl+O` | Finding a note by title |
| Global Search | `Cmd/Ctrl+Shift+F` | Full-text search |
| Tag Search | `tag:#topic` | Finding notes on a topic |
| Graph View | Visual node map | Discovering idea clusters |
| Backlinks | Bottom of each note | Seeing what references this note |
| MOCs | Maps of Content in `05 - Knowledge/MOCs/` | Browsing a topic systematically |

Or ask Claude directly — it will search and summarize for you.

## How to Expand

- **New resource types**: "I want to save podcast episodes. Create a template and folder for it."
- **New knowledge domains**: "I'm learning cryptography. Set up a knowledge area for it."
- **MOCs (Maps of Content)**: When a topic hits 5+ notes, Claude suggests creating an MOC. Or just ask.

## Tag Conventions

- **Type tags**: `#type/daily`, `#type/fleeting`, `#type/literature`, `#type/permanent`, `#type/book`, `#type/project`, `#type/moc`, `#type/review`
- **Status tags**: `#status/active`, `#status/on-hold`, `#status/completed`, `#status/archived`
- **Topic tags**: lowercase, hyphenated (e.g., `#productivity`, `#machine-learning`)
- **Source tags**: `#source/article`, `#source/book`, `#source/podcast`, `#source/video`, `#source/conversation`

## Maintenance

### Claude's Responsibilities
- Classify and ingest all incoming content
- Link new notes to existing ones
- Create commits with conventional commit messages
- Suggest MOCs when topics grow large
- Periodically flag orphaned or unlinked notes

### Your Responsibilities
- Review and triage flagged items when Claude asks
- Complete monthly/annual reviews
- Archive completed projects
- Provide feedback if categorization isn't working

### Review Cadence

| Frequency | Action |
|-----------|--------|
| **Daily** | Feed Claude content as it comes up |
| **Weekly** | Review the week's ingestions, ask Claude for a summary |
| **Monthly** | Complete a Monthly Review |
| **Yearly** | Annual reflection, prune stale areas, set new priorities |

</details>

---

<a id="中文"></a>

<details>
<summary><b>中文</b> — 点击展开</summary>

## 前置要求

- **[Obsidian](https://obsidian.md/)** — 本地优先的 Markdown 应用，用于渲染你的知识库
- **[Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview)** — AI 代理，负责分类、撰写、链接和提交笔记
- **Git** — 用于版本控制和同步

就这些。Claude Code 会自动处理所有提交、链接和组织工作。

## 工作原理

```
你 → 在 Claude Code 对话中粘贴原始内容
Claude → 分类、处理、链接、提交
知识库 → 成长为互联的知识图谱
```

你不需要关心文件夹、模板或链接。把内容给 Claude，它自己决定放哪里。

## 快速开始

### 1. 克隆此仓库

```bash
git clone https://github.com/hiyuantang/second-brain.git
```

**不要 Fork** — 直接克隆就行。如果你想要私密副本，创建一个新的私有仓库，推送到那里而不是 origin。

### 2. 在 Obsidian 中打开

打开 Obsidian → "打开文件夹作为仓库" → 选择 `Second Brain/` 文件夹。

### 3. 开始喂内容

把任何内容粘贴到 Claude Code 对话中：

```
"保存这篇关于习惯养成的文章：https://..."
"以下是《原子习惯》的读书笔记..."
"一个想法：复利效应也适用于学习"
"今天和 Sarah 开了项目会议..."
```

Claude 会分类、写笔记、建立链接、并提交。

## 知识库结构

```
Second Brain/
├── Home.md                  # 仓库入口
├── 00 - Inbox/              # 快速捕获，待分类
├── 01 - Daily/              # 每日笔记 (YYYY-MM-DD.md)
├── 02 - Projects/           # 进行中的项目（有截止日期）
├── 03 - Areas/              # 持续的职责
├── 04 - Resources/          # 外部参考资料（文章、书籍、工具）
├── 05 - Knowledge/          # 永久笔记 — 你综合后的想法
├── 06 - Archive/            # 已完成和不活跃的内容
├── 90 - Templates/          # 笔记模板
├── 91 - Attachments/        # 图片、PDF、媒体
├── 99 - Meta/               # 系统文档
└── .obsidian/               # Obsidian 配置
```

知识库结合了 **PARA**（基于行动力的组织方式）和 **Zettelkasten**（原子化、互联的永久笔记）。

## 可以喂给 Claude 的内容

| 内容类型 | 示例 | Claude 会做什么 |
|----------|------|----------------|
| 文章链接或文本 | "保存这篇：https://..." | 创建文献笔记 + 提取永久笔记 |
| 书籍亮点 | "《深度工作》的读书笔记..." | 创建书籍笔记 + 关键想法 |
| 快速想法 | "想法：最好的学习方式是教别人" | 创建闪念笔记，之后升级为永久笔记 |
| 会议笔记 | "和 Alex 开了新项目的会..." | 创建/更新项目笔记 |
| 每日记录 | "今天做了 X，学了 Y..." | 追加到今天的每日笔记 |
| 批量内容 | "这 5 篇文章我想保存" | 逐篇处理，分别提交 |

## 检索信息

在 Obsidian 中打开知识库后：

| 方法 | 快捷键 | 适用场景 |
|------|--------|----------|
| 快速切换器 | `Cmd/Ctrl+O` | 按标题查找笔记 |
| 全局搜索 | `Cmd/Ctrl+Shift+F` | 全文搜索 |
| 标签搜索 | `tag:#topic` | 查找某主题的所有笔记 |
| 关系图谱 | 可视化节点 | 发现想法集群 |
| 反向链接 | 每篇文章底部 | 查看引用此笔记的内容 |
| MOC（内容地图） | `05 - Knowledge/MOCs/` 中 | 系统性地浏览某主题 |

或者直接问 Claude —— 它会帮你搜索和总结。

## 如何扩展

- **新资源类型**："我想保存播客。帮我创建模板和文件夹。"
- **新知识领域**："我在学密码学。帮我建立知识领域。"
- **MOC（内容地图）**：当某主题积累到 5+ 笔记时，Claude 会主动建议创建 MOC。你也可以直接要求。

## 标签规范

- **类型标签**：`#type/daily`、`#type/fleeting`、`#type/literature`、`#type/permanent`、`#type/book`、`#type/project`、`#type/moc`、`#type/review`
- **状态标签**：`#status/active`、`#status/on-hold`、`#status/completed`、`#status/archived`
- **主题标签**：小写，连字符分隔（如 `#productivity`、`#machine-learning`）
- **来源标签**：`#source/article`、`#source/book`、`#source/podcast`、`#source/video`、`#source/conversation`

## 维护

### Claude 的职责
- 分类和摄入所有内容
- 将新笔记与已有内容建立链接
- 使用规范的提交信息创建提交
- 当主题变大时建议创建 MOC
- 定期标记孤立或未链接的笔记

### 你的职责
- 当 Claude 提示时审核和分类待处理项
- 完成月度/年度回顾
- 归档已完成的项目
- 如果分类不合理，提供反馈

### 回顾节奏

| 频率 | 操作 |
|------|------|
| **每日** | 随时喂 Claude 内容 |
| **每周** | 回顾本周摄入内容，让 Claude 总结 |
| **每月** | 完成月度回顾 |
| **每年** | 年度反思，清理过期内容，设定新优先级 |

</details>
