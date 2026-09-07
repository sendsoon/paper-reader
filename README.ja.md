# paper-reader

[English](README.md) | [简体中文](README.zh-CN.md) | **日本語**

論文の URL・DOI・タイトルから本文を読み、**原文引用 → 翻訳 → 解説** の順で構造化したブリーフィングを出すエージェントスキルです。

```bash
npx skills add sendsoon/paper-reader-skill
```

## 公開事例

このスキルで書いた解説は SendSoon のナレッジベースに公開しています。各ページは公式タイトルと arXiv 記録を保ち、要旨を原文引用してから、検索可能な原句で読み進めます。

**一覧：** [https://sendsoonai.com/paper/ai](https://sendsoonai.com/paper/ai)

| 論文 | 公開ページ |
|---|---|
| Temporal Self-Distillation: Learning Visual State Tracking in Videos Without Supervision | [sendsoonai.com/paper/ai/temporal-self-distillation-s3t-explained](https://sendsoonai.com/paper/ai/temporal-self-distillation-s3t-explained) |
| FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness | [sendsoonai.com/paper/ai/flashattention-io-aware-explained](https://sendsoonai.com/paper/ai/flashattention-io-aware-explained) |
| Native Sparse Attention: Hardware-Aligned and Natively Trainable Sparse Attention | [sendsoonai.com/paper/ai/native-sparse-attention-deepseek-explained](https://sendsoonai.com/paper/ai/native-sparse-attention-deepseek-explained) |
| Attention Residuals | [sendsoonai.com/paper/ai/attention-residuals-kimi-explained](https://sendsoonai.com/paper/ai/attention-residuals-kimi-explained) |
| Kimi-Researcher: End-to-End RL Training for Emerging Agentic Capabilities | [sendsoonai.com/paper/ai/kimi-researcher-agentic-rl-explained](https://sendsoonai.com/paper/ai/kimi-researcher-agentic-rl-explained) |

公開形はスキルそのものです。タイトルは改変せず、要旨は先に原文、5–6 個の検証可能な引用と証拠の境界を置きます。

## このスキルがすること

`paper-reader` は、コーディングエージェント向けの論文読解ワークフローです（Cursor、Claude Code、Codex、および `SKILL.md` を読み込む他のエージェント）。論文の URL または DOI を渡すと、エージェントは次を行います。

1. 読める HTML または PDF 本文を見つける
2. タイトル・著者・識別子・日付を**原文のまま**取り出す
3. 要旨を原文引用し、必要なら翻訳する
4. 中核となる主張を 5–10 個選び、**原文 → 翻訳 → 解説**の順で示す
5. 証拠の境界を書く：数値がどの設定に属するか、論文が述べていないことは何か

主張を先に言い換えてから、近い引用を後付けすることは**しません**。主張は論文の文面に置きます。

既定の解説言語は英語です。中国語や日本語を指定した場合も、`>` 内は原文のままです。変わるのは翻訳と解説だけです。

## 使うとき

**論文を読む・要約する・解説する・ざっと把握する**ときに使います。

査読の模擬、採否コメント、複数査読者パネルには使いません。

## インストール

```bash
npx skills add sendsoon/paper-reader-skill
```

リポジトリをクローンし、`SKILL.md`、`references/`、`examples/` を各エージェントの skills ディレクトリにコピーしても使えます。

インストール後、[skills.sh](https://skills.sh/) のカタログページはテレメトリ到着後に表示されます。

## スキルの更新

Skills CLI で入れた場合は、GitHub から最新の `SKILL.md` を取ります。

```bash
npx skills check
npx skills update paper-reader
```

`check` は新しい版があるかだけ見ます。`update` はこのスキルを、前回入れた同じエージェントへ再インストールします。入っているスキルをすべて更新するなら：

```bash
npx skills update
```

確認を省略する：

```bash
npx skills update paper-reader -y
```

インストールコマンドをもう一度実行しても、ファイルは最新に上書きされます。

```bash
npx skills add sendsoon/paper-reader-skill
```

手でコピーした場合は、このリポジトリを `git pull` し、`SKILL.md`、`references/`、`examples/` を以前の skills ディレクトリへ上書きしてください。

## 使い方

```text
この論文を読んで：https://arxiv.org/abs/xxxx.xxxxx
論文を要約して：https://aclanthology.org/...
```

タイトルだけの場合、エージェントは同一論文であることを確認してから解説します。

## 出力

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

## リポジトリ構成

このリポジトリは**単一スキルのパッケージ**です。スキルはリポジトリ直下に置き、`npx skills` が最初に発見できるようにしています。

```text
paper-reader/
├── SKILL.md                 # エージェント入口：frontmatter + 手順
├── skills.sh.json           # skills.sh のリポジトリページ用グループ
├── README.md                # English
├── README.zh-CN.md          # 簡体字中国語
├── README.ja.md             # 日本語
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
| `README.md` / `README.zh-CN.md` / `README.ja.md` | 人向けの三言語ドキュメントです。 |
| `references/` | 本文を探すときだけ読む補足資料です。 |
| `examples/` | 出力テンプレートと、架空論文による正誤対比です。 |

## ライセンス

[MIT](LICENSE)
