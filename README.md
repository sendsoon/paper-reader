# paper-reader

[English](#english) · [中文](#中文) · [日本語](#日本語)

Agent skill that reads an academic paper from a URL, DOI, or title, then produces a structured briefing: **original quote → translation → interpretation**.

```bash
npx skills add sendsoon/paper-reader
```

---

## English

### What this skill does

`paper-reader` is a reusable workflow for coding agents (Cursor, Claude Code, Codex, and others that load `SKILL.md`). Given a paper link or DOI, the agent:

1. Locates a readable HTML or PDF full text
2. Extracts the title, authors, identifiers, and dates **verbatim**
3. Quotes the abstract, then translates it if needed
4. Selects 5–10 core claims and presents each as **original sentence → translation → interpretation**
5. States the evidence boundary: which setting the numbers belong to, and what the paper did not claim

The skill does **not** paraphrase a claim and then look for a quote. Claims stay in the paper's words.

Default briefing language is English. If you ask for another language, quotes stay in the source language; only translation and interpretation change.

### When to use it

Use this skill to **read, summarize, interpret, or skim** a paper.

Do not use it for peer-review simulation, accept/reject letters, or multi-reviewer panels.

### Install

```bash
npx skills add sendsoon/paper-reader
```

Or clone the repository and copy `SKILL.md`, `references/`, and `examples/` into your agent's skills directory.

The [skills.sh](https://skills.sh/) catalog page updates after install telemetry arrives.

### Usage

```text
Read this paper: https://arxiv.org/abs/xxxx.xxxxx
Summarize this paper: https://arxiv.org/pdf/xxxx.xxxxx
Interpret this paper: https://aclanthology.org/...
```

A DOI or a title also works. The agent should confirm it found the same paper before briefing.

### Output

```text
# Translated title (Original Title)

**Authors**: ...
**Source**: venue + canonical URL + date

## Abstract
> verbatim abstract
translation (if needed)

## Core claims
### 1. [nav label]
Original / Translation / Interpretation

## Brief summary
## Evidence boundary
```

### Project layout

This repository is a **single-skill package**. The skill lives at the repo root so `npx skills` discovers it first.

```text
paper-reader/
├── SKILL.md                 # Agent entry: frontmatter + workflow
├── skills.sh.json           # skills.sh repo-page grouping
├── README.md                # This file (en / zh / ja)
├── LICENSE
├── references/
│   └── sources.md           # Site-specific full-text paths
└── examples/
    └── output-template.md
```

| File | Role |
|---|---|
| `SKILL.md` | What the agent executes. YAML `name` / `description` control discovery. |
| `skills.sh.json` | Groups the skill under **Research** on the skills.sh repo page. Does not change install behavior. |
| `references/` | Progressive disclosure. Read only when locating a source. |
| `examples/` | Output shape and a fictional good/bad contrast. |

### License

[MIT](LICENSE)

---

## 中文

### 技能做什么

`paper-reader` 是给编程智能体使用的论文阅读工作流（Cursor、Claude Code、Codex 及其他会加载 `SKILL.md` 的智能体）。你提供论文链接或 DOI 后，智能体会：

1. 定位可读的 HTML 或 PDF 全文
2. **逐字**提取标题、作者、标识与日期
3. 先引用摘要原文，必要时再翻译
4. 挑选 5–10 条核心观点，按 **原文 → 翻译 → 解读** 呈现
5. 写明证据边界：数字属于何种设定，原文没有声称什么

本技能**禁止**先用自己的话概括主张、再去贴一段大致对应的原文。主张以论文原句为准。

默认解读语言为英文。若你要求中文或日文，`>` 中仍放原文，只切换翻译和解读的语言。

### 何时使用

需要**读论文、总结论文、解读论文、论文速览**时使用。

不要用它做模拟审稿、拒稿意见或多审稿人评审。

### 安装

```bash
npx skills add sendsoon/paper-reader
```

也可克隆本仓库，把 `SKILL.md`、`references/`、`examples/` 复制到所用智能体的 skills 目录。

安装后，目录页由 [skills.sh](https://skills.sh/) 在收到遥测后展示。

### 用法

```text
读这篇论文：https://arxiv.org/abs/xxxx.xxxxx
帮我看一下这篇文献：10.xxxx/xxxxx
总结这篇 paper：Attention Is All You Need
```

只给标题时，智能体应先定位到同一篇论文，必要时向你确认。

### 产出

```text
# 译文标题 (Original Title)

**Authors**: ...
**Source**: 来源 + 正式链接 + 日期

## Abstract
> 原文摘要
译文（如需要）

## Core claims
### 1. [导航标签]
Original / Translation / Interpretation

## Brief summary
## Evidence boundary
```

结构化 Markdown：原标题（不得改写）、作者、来源、摘要原文与译文、5–10 条可检索的原文引用及解读，以及简要总结与证据边界。

### 项目结构

本仓库是**单技能包**。技能放在仓库根目录，以便 `npx skills` 优先发现。

```text
paper-reader/
├── SKILL.md                 # 智能体入口：frontmatter + 工作流
├── skills.sh.json           # skills.sh 仓库页分组
├── README.md                # 本文件（英 / 中 / 日）
├── LICENSE
├── references/
│   └── sources.md           # 按站点定位全文
└── examples/
    └── output-template.md
```

| 文件 | 作用 |
|---|---|
| `SKILL.md` | 智能体实际执行的说明。YAML 的 `name` / `description` 决定能否被发现。 |
| `skills.sh.json` | 在 skills.sh 仓库页把本技能归入 **Research**。不影响安装。 |
| `references/` | 按需阅读的站点抓取说明，避免把主文件撑得过长。 |
| `examples/` | 输出模板，以及虚构的正误对照。 |

### 许可证

[MIT](LICENSE)

---

## 日本語

### このスキルがすること

`paper-reader` は、コーディングエージェント向けの論文読解ワークフローです（Cursor、Claude Code、Codex、および `SKILL.md` を読み込む他のエージェント）。論文の URL または DOI を渡すと、エージェントは次を行います。

1. 読める HTML または PDF 本文を見つける
2. タイトル・著者・識別子・日付を**原文のまま**取り出す
3. 要旨を原文引用し、必要なら翻訳する
4. 中核となる主張を 5–10 個選び、**原文 → 翻訳 → 解説**の順で示す
5. 証拠の境界を書く：数値がどの設定に属するか、論文が述べていないことは何か

主張を先に言い換えてから、近い引用を後付けすることは**しません**。主張は論文の文面に置きます。

既定の解説言語は英語です。中国語や日本語を指定した場合も、`>` 内は原文のままです。変わるのは翻訳と解説だけです。

### 使うとき

**論文を読む・要約する・解説する・ざっと把握する**ときに使います。

査読の模擬、採否コメント、複数査読者パネルには使いません。

### インストール

```bash
npx skills add sendsoon/paper-reader
```

リポジトリをクローンし、`SKILL.md`、`references/`、`examples/` を各エージェントの skills ディレクトリにコピーしても使えます。

インストール後、[skills.sh](https://skills.sh/) のカタログページはテレメトリ到着後に表示されます。

### 使い方

```text
この論文を読んで：https://arxiv.org/abs/xxxx.xxxxx
論文を要約して：https://aclanthology.org/...
```

タイトルだけの場合、エージェントは同一論文であることを確認してから解説します。

### 出力

```text
# 訳題 (Original Title)

**Authors**: ...
**Source**: 出典 + 正規 URL + 日付

## Abstract
> 原文の要旨
訳（必要な場合）

## Core claims
### 1. [ナビラベル]
Original / Translation / Interpretation

## Brief summary
## Evidence boundary
```

構造化 Markdown：改変しない原題、著者、出典、要旨の原文と訳、検索可能な原文引用付きの 5–10 項目、短いまとめと証拠の境界。

### リポジトリ構成

このリポジトリは**単一スキルのパッケージ**です。スキルはリポジトリ直下に置き、`npx skills` が最初に発見できるようにしています。

```text
paper-reader/
├── SKILL.md                 # エージェント入口：frontmatter + 手順
├── skills.sh.json           # skills.sh のリポジトリページ用グループ
├── README.md                # 本ファイル（英 / 中 / 日）
├── LICENSE
├── references/
│   └── sources.md           # サイト別の本文取得経路
└── examples/
    └── output-template.md
```

| ファイル | 役割 |
|---|---|
| `SKILL.md` | エージェントが実行する手順。YAML の `name` / `description` が発見に使われます。 |
| `skills.sh.json` | skills.sh のリポジトリページで本スキルを **Research** に分類します。インストール動作は変わりません。 |
| `references/` | 本文を探すときだけ読む補足資料です。 |
| `examples/` | 出力テンプレートと、架空論文による正誤対比です。 |

### ライセンス

[MIT](LICENSE)
