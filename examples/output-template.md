# Output template

Use this shape for the final briefing. The paper below is **fictional** and exists only to show structure. Never copy these sentences into a real briefing.

```markdown
# Linear Attention as an Approximation

**Authors**: Ada Example, Lin Example
**Source**: arXiv · https://arxiv.org/abs/0000.00000 · Submitted 1 Jan 2000

## Abstract

> We study attention as a kernel smoother and report results only under a fixed sequence length of 64k tokens.

## Core claims

### 1. Problem setting

Original:
> We study attention as a kernel smoother and report results only under a fixed sequence length of 64k tokens.

Interpretation:
This sentence states the condition under which later numbers hold: a fixed length of 64k, not an arbitrary length. It does not claim the same result for longer sequences or other kernels.

### 2. What is not covered

Original:
> We do not evaluate retrieval tasks in this work.

Interpretation:
The paper draws a task boundary. You cannot infer that the method is better or worse on retrieval.

## Brief summary

This fictional paper discusses attention as a kernel smoother only under a 64k setting and states that it does not evaluate retrieval.

## Evidence boundary

The numbers belong to a fixed 64k setting. Retrieval, longer sequences, and other kernels are not covered.
```

The abstract and quotes above are already in the briefing language, so `Translation` is omitted. If the source language differs, insert a `Translation` block immediately after each `Original` quote.

## Correct vs incorrect presentation

Incorrect (forbidden): write a paraphrase first, then attach a loosely related quote.

```text
This work reduces attention complexity to linear.
> We study attention as a kernel smoother ...
```

Correct: quote first, then translate if needed, then say what the sentence supports and does not support.

```text
Original:
> We study attention as a kernel smoother and report results only under a fixed sequence length of 64k tokens.

Interpretation:
...
```

Keep `64k` as `64k`. Do not "normalize" it to `64K`.
