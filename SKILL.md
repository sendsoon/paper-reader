---
name: paper-reader-skill
description: >
  Reads academic papers from a URL, DOI, or title (HTML or PDF) and produces a
  structured briefing: original title, authors, verbatim abstract plus
  translation, and 5–10 core claims shown as original quote → translation →
  interpretation. Does not paraphrase a claim before quoting it. Default
  briefing language is English unless the user asks otherwise. Use when the
  user provides a paper link or DOI and asks to read, summarize, interpret, or
  skim a paper. Triggers: read paper, summarize paper, interpret paper, paper
  briefing, 读论文, 总结论文, 解读论文, 论文速览, 帮我看这篇 paper, 文献解读,
  論文を読む, 論文を要約, 論文解説.
metadata:
  version: "1.2.0"
  license: MIT
---

# Academic paper reading and interpretation

## About this skill

This skill is a **generic workflow**. It is not bound to any one AI product or tool API. The executor (any model or agent) should use whatever capabilities it actually has, for example:

- "Fetch a web page" → use an available browser or fetch tool
- "Parse a PDF" → use an available PDF reader, or download the file first
- If a step is impossible (for example, no network), tell the user so. Do not invent content.

Site-specific fetch paths: [references/sources.md](references/sources.md). Full output sample: [examples/output-template.md](examples/output-template.md).

## When to use

Use this skill when the user gives a paper URL, DOI, or title, or asks to read / summarize / interpret / skim a paper (including 读论文, summarize this paper, 論文を読んで). Do **not** use it for peer review, accept/reject letters, or multi-reviewer simulation.

## Goal

Given a paper link (or a DOI/title that you first resolve), produce a structured briefing that includes:

1. Original title (verbatim source title + translation if needed; never rewrite a formal title)
2. Author list
3. Abstract (verbatim quote first, then a translation if the source language differs from the briefing language)
4. 5–10 core claims (**original quote → translation → interpretation**; do not invent a paraphrase and then attach a quote)
5. Brief summary and evidence boundary

Default briefing language is English. If the user asks for another language, only translation and interpretation change; quotes stay in the source language.

## Step 1: Identify the source site and locate readable full text

The user may give an abstract page, a PDF URL, or a conference/journal landing page. Choose a path by host:

| Site | How to recognize | Suggested path to full text |
|---|---|---|
| **arXiv** | `arxiv.org` | Prefer HTML full text `arxiv.org/html/{id}`. If no HTML version exists, take metadata from `arxiv.org/abs/{id}` and the PDF at `arxiv.org/pdf/{id}`. |
| **bioRxiv / medRxiv** | `biorxiv.org` / `medrxiv.org` | Prefer the page's "Full Text" HTML link; otherwise use the PDF link. |
| **ACL Anthology** | `aclanthology.org` | The landing page has abstract and metadata. Body text is usually a PDF such as `.../2023.acl-long.1.pdf`. |
| **OpenReview** | `openreview.net` | The forum page has abstract, authors, and reviews. Download the PDF from the page. |
| **NeurIPS / PMLR / proceedings** | `proceedings.neurips.cc`, `proceedings.mlr.press` | The paper page has an abstract; the body is a PDF link. |
| **PubMed / PMC** | `pubmed.ncbi.nlm.nih.gov` / `ncbi.nlm.nih.gov/pmc` | PMC often has full HTML. A PubMed-only page has the abstract; follow the PMC or publisher link for the body. |
| **SSRN** | `ssrn.com` | The page has an abstract. The body is usually a PDF and may require a free account. |
| **IEEE Xplore / ACM / SpringerLink / ScienceDirect / Nature / Science** | matching publisher host | Often paywalled. Public metadata is usually limited to title, authors, and abstract. If the body is inaccessible, say so and do not invent body claims. |
| **Other / unknown** | — | Fetch the given URL first. Treat a PDF response as a PDF. On an abstract page, look for Full Text / PDF / HTML links. |

If the user gave a DOI or a title rather than a URL, search first, land on one of the sites above, then follow the table.

## Step 2: Extract the body

- **HTML full text**: Read the page body. Skip nav and reference-list chrome. Keep body paragraphs, captions, and formula context. Quotes must match the HTML source.
- **PDF full text**: Extract text. Two-column layout may scramble order; restore reading order. Formulas and tables may be incomplete — that only affects skimming, not quoting. A sentence you put in `>` must be re-checked in the PDF/HTML. If you cannot verify it, do not quote it, and do not rewrite it from a "gist".
- Read the paper once. Build a map of introduction, related work, method, experiments, conclusion, and limitations. Then select quotes.

## Step 3: Extract metadata

- **Title**: Match the source-page Title character for character. Add a translation only if the title is not already in the briefing language. A lecture-style subtitle must not replace the paper title.
- **Identifiers and dates**: arXiv ID, abs URL, Submitted / version date must match the source page. Do not mix versions.
- **Authors**: List every author with original spelling. Do not translate names. Affiliations may be listed if available. In the interpretation, say "the paper", "the text", or "this work" — do not turn author names into classroom subjects.
- **Abstract**: Quote the abstract verbatim in `>` first, then translate it if needed. Do not give a paraphrase without the original. Keep numbers, units, benchmark names, and symbols as written.

## Step 4: Select 5–10 core claims as original → translation → interpretation

Pick 5–10 original sentences or short passages that carry the paper's contribution. Cover different parts when possible:

- Motivation / problem statement (1–2)
- Key method or model choices (3–4)
- Key experimental findings (3–4)
- Limitations, future work, or main conclusions (1–2)

If unsure, write fewer items. Do not add claims the paper did not make. Default is 5–10; many briefings work with 5–6 verifiable quotes. Prefer fewer checked quotes over padded rewrites.

### Presentation rules (avoid ambiguity)

The claim is the original sentence. Interpretation only explains what that sentence already says. Do not summarize first and translate second — a summary rewrites scope, comparators, and quantities.

`>` contains only paper/report sentences that you can search for verbatim in the PDF/HTML. Do not rewrite:

1. The formal title (must match the source-page Title)
2. arXiv ID and `https://arxiv.org/abs/<id>`; do not mix version dates
3. Submitted / version dates
4. The quoted sentence (punctuation, case, hyphens)
5. Numbers, units, and benchmark names (`64k` must stay `64k`)
6. Formula symbols taken from the paper

Only translation and interpretation may paraphrase lightly. Before any interpretation, give the same original quote.

### Per-claim format

```
[Claim N] [short nav label — for lookup only; not a claim the paper never made]

Original:
> Verbatim paper/report sentence(s). You may include 1–3 adjacent sentences
> to keep a complete proposition. Do not stitch distant paragraphs, drop
> qualifiers, or rewrite numbers and symbols.

Translation:
A corresponding translation of the quote above. Omit this block if the
source is already in the briefing language.
Keep numbers, units, benchmark names, and symbols as written.

Interpretation:
3–5 sentences. First locate the quote in the argument and explain the
mechanism. Then mark the evidence boundary (the setting it applies to,
what it does not cover, inferences the paper did not make).
Do not add unstated claims. Avoid empty praise such as "this is important".
```

Incorrect (forbidden): write "this work reduces attention to linear complexity", then attach a loosely related quote.

Correct: give the original `>` first, then translate if needed, then say what the sentence supports and does not support.

## Step 5: Assemble the final document

Use this Markdown shape:

```markdown
# Translated title (Original Title, character-for-character with the source page)

**Authors**: Author A, Author B, Author C...
**Source**: (venue + canonical URL + date; use the arXiv abs page)

## Abstract

> Verbatim abstract

(Translation only if the abstract is not already in the briefing language)

## Core claims

### 1. [nav label]
Original / Translation / Interpretation (Step 4 format)

### 2. [nav label]
...

... (5–10 items; every item needs a searchable original quote)

## Brief summary
(2–3 sentences; only contributions the original text already supports)

## Evidence boundary
(Which setting the numbers/conclusions belong to; which inferences the paper did not make)
```

If the paper title is already in the briefing language, use that title once. Do not invent a second title.

## Output form

- For a quick read, show the Markdown in the chat. Do not write a file.
- If the user asks to save, export, or generate a file, write a `.md` file.

## Edge cases

- **Body inaccessible** (paywall, login, network): Tell the user why. Brief from title, authors, and abstract when those are available. Do not invent body details. Quotes you can make must still be verbatim. Do not rewrite inaccessible text as "the paper says".
- **Paper is not in English**: `>` still holds the original language. `Translation` is the briefing-language rendering. `Interpretation` uses the briefing language. Do not rewrite the original into English and then quote that rewrite.
- **User wants a different number of claims** (for example, "give me 5 points"): Use that count. Every item still starts with the original quote.
- **Title or keywords only**: Search first. Prefer open platforms (arXiv, ACL Anthology, OpenReview). Confirm it is the same paper, and ask the user if needed.
- **User asks for a briefing language other than English**: Quotes stay original. Translation and interpretation switch to the requested language. The no-rewrite rule does not change.

## Further reading

- [references/sources.md](references/sources.md) — locate full text by site
- [examples/output-template.md](examples/output-template.md) — output template and correct vs incorrect contrast
