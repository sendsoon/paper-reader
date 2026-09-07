# paper-reader-skill

[English](README.md) | **简体中文** | [日本語](README.ja.md)

给编程智能体用的论文阅读技能：从链接、DOI 或标题读取论文，按 **原文引用 → 翻译 → 解读** 产出结构化速览。

```bash
npx skills add sendsoon/paper-reader-skill
```

## 案例网页

本技能产出的公开解读已发布在 SendSoon 知识库。每页保留官方标题与 arXiv 记录，先引用摘要原文，再按可检索原句逐条解读。

**总览：** [https://sendsoonai.com/paper/ai](https://sendsoonai.com/paper/ai)

| 论文 | 案例页 |
|---|---|
| Temporal Self-Distillation: Learning Visual State Tracking in Videos Without Supervision | [sendsoonai.com/paper/ai/temporal-self-distillation-s3t-explained](https://sendsoonai.com/paper/ai/temporal-self-distillation-s3t-explained) |
| FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness | [sendsoonai.com/paper/ai/flashattention-io-aware-explained](https://sendsoonai.com/paper/ai/flashattention-io-aware-explained) |
| Native Sparse Attention: Hardware-Aligned and Natively Trainable Sparse Attention | [sendsoonai.com/paper/ai/native-sparse-attention-deepseek-explained](https://sendsoonai.com/paper/ai/native-sparse-attention-deepseek-explained) |
| Attention Residuals | [sendsoonai.com/paper/ai/attention-residuals-kimi-explained](https://sendsoonai.com/paper/ai/attention-residuals-kimi-explained) |
| Kimi-Researcher: End-to-End RL Training for Emerging Agentic Capabilities | [sendsoonai.com/paper/ai/kimi-researcher-agentic-rl-explained](https://sendsoonai.com/paper/ai/kimi-researcher-agentic-rl-explained) |

这些页面就是技能的公开形态：标题不改写、摘要先原文、5–6 段可核验引用，并写明证据边界。

## 技能做什么

`paper-reader-skill` 是给编程智能体使用的论文阅读工作流（Cursor、Claude Code、Codex 及其他会加载 `SKILL.md` 的智能体）。你提供论文链接或 DOI 后，智能体会：

1. 定位可读的 HTML 或 PDF 全文
2. **逐字**提取标题、作者、标识与日期
3. 先引用摘要原文，必要时再翻译
4. 挑选 5–10 条核心观点，按 **原文 → 翻译 → 解读** 呈现
5. 写明证据边界：数字属于何种设定，原文没有声称什么

本技能**禁止**先用自己的话概括主张、再去贴一段大致对应的原文。主张以论文原句为准。

默认解读语言为英文。若你要求中文或日文，`>` 中仍放原文，只切换翻译和解读的语言。

## 何时使用

需要**读论文、总结论文、解读论文、论文速览**时使用。

不要用它做模拟审稿、拒稿意见或多审稿人评审。

## 安装

```bash
npx skills add sendsoon/paper-reader-skill
```

也可克隆本仓库，把 `SKILL.md`、`references/`、`examples/` 复制到所用智能体的 skills 目录。

安装后，目录页由 [skills.sh](https://skills.sh/) 在收到遥测后展示。

## 如何更新技能

若当初用 Skills CLI 安装，从 GitHub 拉取最新的 `SKILL.md`：

```bash
npx skills check
npx skills update paper-reader-skill
```

`check` 只检查是否有新版本。`update` 会把本技能重装到原先那些智能体。要更新本机已安装的全部技能：

```bash
npx skills update
```

跳过确认提示：

```bash
npx skills update paper-reader-skill -y
```

再执行一次安装命令也会覆盖为最新文件：

```bash
npx skills add sendsoon/paper-reader-skill
```

若是手动复制安装的，先 `git pull` 本仓库，再把 `SKILL.md`、`references/`、`examples/` 覆盖到原来的 skills 目录。

## 用法

```text
读这篇论文：https://arxiv.org/abs/xxxx.xxxxx
帮我看一下这篇文献：10.xxxx/xxxxx
总结这篇 paper：Attention Is All You Need
```

只给标题时，智能体应先定位到同一篇论文，必要时向你确认。

## 产出

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

## 项目结构

本仓库是**单技能包**。技能放在仓库根目录，以便 `npx skills` 优先发现。

```text
paper-reader-skill/
├── SKILL.md                 # 智能体入口：frontmatter + 工作流
├── skills.sh.json           # skills.sh 仓库页分组
├── README.md                # English
├── README.zh-CN.md          # 简体中文
├── README.ja.md             # 日本語
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
| `README.md` / `README.zh-CN.md` / `README.ja.md` | 给人看的三语说明。 |
| `references/` | 按需阅读的站点抓取说明。 |
| `examples/` | 输出模板，以及虚构的正误对照。 |

## 许可证

[MIT](LICENSE)
