# paper-reader

**English** | [简体中文](README.zh-CN.md) | [日本語](README.ja.md)

Agent skill that reads an academic paper from a URL, DOI, or title, then produces a structured briefing: **original quote → translation → interpretation**.

```bash
npx skills add sendsoon/paper-reader
```

## Live examples

Published briefings from this skill are on the SendSoon knowledge base. Each page keeps the official title and arXiv record, quotes the abstract, then walks the paper with verbatim sentences.

**Hub:** [https://sendsoonai.com/docs](https://sendsoonai.com/docs)

| Paper | Live page |
|---|---|
| FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness | [sendsoonai.com/docs/flashattention-io-aware-explained](https://sendsoonai.com/docs/flashattention-io-aware-explained) |
| Native Sparse Attention: Hardware-Aligned and Natively Trainable Sparse Attention | [sendsoonai.com/docs/native-sparse-attention-deepseek-explained](https://sendsoonai.com/docs/native-sparse-attention-deepseek-explained) |
| Attention Residuals | [sendsoonai.com/docs/attention-residuals-kimi-explained](https://sendsoonai.com/docs/attention-residuals-kimi-explained) |
| Kimi-Researcher: End-to-End RL Training for Emerging Agentic Capabilities | [sendsoonai.com/docs/kimi-researcher-agentic-rl-explained](https://sendsoonai.com/docs/kimi-researcher-agentic-rl-explained) |

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
