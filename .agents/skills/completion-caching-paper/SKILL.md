---
name: completion-caching-paper
description: Orchestrates writing and review of the Completion-Caching design paper — napkin-math systems argument for LLM provider completion caching, no production deployment yet. Use when editing manuscript/, notes.md, or discussing completion caching, prefill/decode cost, gating, top-k projective pooling, semantic caches, or fleet-level savings.
paths:
  - "manuscript/**"
  - "notes.md"
icon: book-open
color: blue
---

# Completion-Caching paper workflow

Design / systems paper for an arXiv-style preprint. Not empirical: no deployment numbers, no benchmark tables, no invented hit rates.

## Paper facts (do not drift)

- **Contribution:** completion caching as a first-class serving decision — cache $(Q, C(Q))$, not KV blocks or prefixes alone.
- **Criterion:** answer acceptability $\mathcal{A}(Q, C)$, not query embedding similarity.
- **Evidence types allowed:** cited serving literature, published model specs (e.g. Llama 3.1 70B), stated assumptions, napkin-math identities in `manuscript/sections/02-cost-model.tex`.
- **Explicit non-claims:** no production deployment yet; conservative gating; long-tail still decodes.
- **Vocabulary (keep consistent):** completion caching, semantic cache, prefix/KV cache, prefill, decode, acceptability, portable vs context-bound intents, top-k projective pooling.

## Default pipeline

Load sibling skills on demand — read their full `SKILL.md` before executing. Do not load every skill up front.

| Step | Skill | When |
|------|-------|------|
| 1. Voice & structure | `paper-ecosystem`, then `paper-writing` | Drafting or restructuring sections |
| 2. Claim discipline | `ai-research-writing-skill` | New sections, major edits, before calling a draft "done" |
| 3. AI tells | `avoid-ai-writing` | Detect mode to audit; rewrite/edit mode to fix prose |
| 4. Rhythm | `latex-rhythm-refiner` | After drafting, if paragraphs read uniform |
| 5. Citations | `verify-citations` | After any `.bib` change; target `manuscript/refs.bib` |
| 6. Claims audit | `verify-claims` | Before submission-style passes |
| 7. Related work | `find-papers` → `draft-related-work` | Thin or missing prior-work positioning |
| 8. Review | `simulate-reviewers` | Red-team before external eyes |
| 9. Health check | `assess-paper` | "Is this ready?" / prioritization |

Skip experiment-design phases in `research-paper-writing`; use its **position paper** guidance only.

## Repo layout

```text
manuscript/
├── main.tex
├── refs.bib
├── sections/          # 01-introduction, 02-cost-model, 03-method, …
└── figures/
notes.md               # scratch ideas (e.g. top-k projective pooling)
```

## Citations (LaTeX / BibTeX)

For LaTeX papers always use BibTeX citations that can be cross-referenced to an existing paper like on Google Scholar.

- Cite with `\citet` / `\citep` / `\cite` keys — never hand-typed `(Author, YEAR)` in the `.tex`.
- Every key must exist in `manuscript/refs.bib`.
- Every `.bib` entry must map to a real, lookup-able paper (Google Scholar, Crossref, DBLP, Semantic Scholar, or arXiv). Fetch the BibTeX from that record; never invent titles, authors, years, DOIs, or keys.
- After any `.bib` change, run `verify-citations` against `manuscript/refs.bib`.

## Claim–evidence rules for this paper

1. **Cost numbers** must trace to Section 2 assumptions (model size, $S$, tokens decoded, bandwidth bounds) or a cited source — never to invented measurements.
2. **Hit-rate / savings** claims must be conditional ("at X% hit rate under assumptions Y") unless citing external data.
3. **Prior work** comparisons (PagedAttention, GPTCache, REST, etc.) must match what those papers actually claim — use `verify-citations` and primary sources.
4. **Method spec** (gating, top-k pooling) may be proposed design; label it as specification, not evaluated system.
5. Weaken or delete any claim that cannot be mapped to evidence; do not patch with confident prose.

## Suggested artifacts (create in repo root when useful)

- `claim_evidence_map.md` — claim · section · evidence · status
- `reviewer_risks.md` — output of simulate-reviewers pass

## Build

Compile from `manuscript/` when checking layout:

```bash
cd manuscript && latexmk -pdf main.tex
```

## Custom Mode

Pin this skill (Option+Enter / Alt+Enter) for a full manuscript-editing session so the pipeline stays in context across turns.
