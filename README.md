# paper-reader

**English** | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

Agent skill that reads an academic paper from a URL, DOI, or title, then produces a structured briefing: **original quote → translation → interpretation**.

```bash
npx skills add sendsoon/paper-reader
```

## Live examples

Published briefings from this skill are on the SendSoon knowledge base. Each page keeps the official title and arXiv record, quotes the abstract, then walks the paper with verbatim sentences.

**Hub:** [https://sendsoonai.com/paper/ai](https://sendsoonai.com/paper/ai)

| Paper | Live page |
|---|---|
| Temporal Self-Distillation: Learning Visual State Tracking in Videos Without Supervision | [sendsoonai.com/paper/ai/temporal-self-distillation-s3t-explained](https://sendsoonai.com/paper/ai/temporal-self-distillation-s3t-explained) |
| FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness | [sendsoonai.com/paper/ai/flashattention-io-aware-explained](https://sendsoonai.com/paper/ai/flashattention-io-aware-explained) |
| Native Sparse Attention: Hardware-Aligned and Natively Trainable Sparse Attention | [sendsoonai.com/paper/ai/native-sparse-attention-deepseek-explained](https://sendsoonai.com/paper/ai/native-sparse-attention-deepseek-explained) |
| Attention Residuals | [sendsoonai.com/paper/ai/attention-residuals-kimi-explained](https://sendsoonai.com/paper/ai/attention-residuals-kimi-explained) |
| Kimi-Researcher: End-to-End RL Training for Emerging Agentic Capabilities | [sendsoonai.com/paper/ai/kimi-researcher-agentic-rl-explained](https://sendsoonai.com/paper/ai/kimi-researcher-agentic-rl-explained) |

These pages are the public shape of the skill: title unaltered, abstract quoted first, then 5–6 searchable quotes with interpretation and an evidence boundary.

## What this skill does

`paper-reader` is a reusable workflow for coding agents (Cursor, Claude Code, Codex, and others that load `SKILL.md`). Given a paper link or DOI, the agent:

1. Locates a readable HTML or PDF full text
2. Extracts the title, authors, identifiers, and dates **verbatim**
3. Quotes the abstract, then translates it if needed
4. Selects 5–10 core claims and presents each as **original sentence → translation → interpretation**
5. States the evidence boundary: which setting the numbers belong to, and what the paper did not claim

The skill does **not** paraphrase a claim and then look for a quote. Claims stay in the paper's words.

Default briefing language is English. If you ask for another language, quotes stay in the source language; only translation and interpretation change.

## When to use it

Use this skill to **read, summarize, interpret, or skim** a paper.

Do not use it for peer-review simulation, accept/reject letters, or multi-reviewer panels.

## Install

```bash
npx skills add sendsoon/paper-reader
```

Or clone the repository and copy `SKILL.md`, `references/`, and `examples/` into your agent's skills directory.

The [skills.sh](https://skills.sh/) catalog page updates after install telemetry arrives.

## Update the skill

If you installed with the Skills CLI, pull the latest `SKILL.md` from GitHub:

```bash
npx skills check
npx skills update paper-reader
```

`check` only reports whether a newer hash exists. `update` reinstalls this skill into the same agents as before. To refresh every installed skill:

```bash
npx skills update
```

Skip the confirm prompt with `-y`:

```bash
npx skills update paper-reader -y
```

Re-running the install command also refreshes the files:

```bash
npx skills add sendsoon/paper-reader
```

If you copied files by hand, `git pull` this repository and copy `SKILL.md`, `references/`, and `examples/` over the previous copy.

## Usage

```text
Read this paper: https://arxiv.org/abs/xxxx.xxxxx
Summarize this paper: https://arxiv.org/pdf/xxxx.xxxxx
Interpret this paper: https://aclanthology.org/...
```

A DOI or a title also works. The agent should confirm it found the same paper before briefing.

## Output

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

## Project layout

This repository is a **single-skill package**. The skill lives at the repo root so `npx skills` discovers it first.

```text
paper-reader/
├── SKILL.md                 # Agent entry: frontmatter + workflow
├── skills.sh.json           # skills.sh repo-page grouping
├── README.md                # English
├── README.zh-CN.md          # Simplified Chinese
├── README.ja.md             # Japanese
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
| `README.md` / `README.zh-CN.md` / `README.ja.md` | Human-facing docs in three languages. |
| `references/` | Progressive disclosure. Read only when locating a source. |
| `examples/` | Output shape and a fictional good/bad contrast. |

## License

[MIT](LICENSE)
