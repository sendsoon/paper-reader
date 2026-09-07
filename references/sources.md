# Source-specific full-text paths

Read this file when the user gives a paper URL, DOI, or title and you need to locate a readable full text. The workflow in `SKILL.md` still applies: quote only what you can verify in HTML or PDF.

If the input is a DOI or a title rather than a URL, search first and land on one of the sites below before extracting text.

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

## DOI and title resolution

1. Resolve a DOI via `https://doi.org/{doi}` and then apply the table above to the landing host.
2. Prefer an open version (arXiv, PMC, ACL Anthology, OpenReview) over a paywalled publisher page when both exist and identify the same work.
3. Confirm title, year, and author overlap before treating a search result as the requested paper. Ask the user if two candidates remain.

## Quote verification

- HTML quotes must match the HTML source, not a later PDF extract.
- PDF quotes must be re-checked in the PDF after extraction. Column order and formulas may be wrong in the extract.
- If a sentence cannot be found again in the source, do not quote it and do not rewrite it as "the paper says".
