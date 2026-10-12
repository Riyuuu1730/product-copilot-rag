# Product Copilot — Evaluation

> **Current status:** one exploratory run with 14 questions, scored using the supplied manual review notes. These results describe this small test set; they are not a general model-accuracy benchmark.

## What was tested

The test set combines questions expected to be answerable from the supplied Product Management PDFs and questions expected to be unanswerable from those PDFs. The review considered three things separately: whether refusal behaviour matched the expected label, whether the output was grounded in the source material, and whether citations pointed to real filenames and useful page references.

| Item | Count / setup |
|---|---:|
| Questions in this run | 14 |
| Initially labelled answerable (`yes`) | 8 |
| Initially labelled unanswerable (`no`) | 6 |
| Retrieval setting recorded in the supplied export | Not recorded |
| 5-chunk vs. 10-chunk comparison available from these files | No |

The underlying PDFs and retrieved passages were not included in the supplied CSVs. The grounding and citation scores below reproduce the human scoring notes supplied with this run; they have not been independently re-scored against the original PDFs in this package.

## Results at a glance

| Check | Result | Interpretation |
|---|---:|---|
| Output `refused=true` for questions initially labelled unanswerable | 5/6 (83.3%) | Includes one mixed answer that gave unsupported content before saying it was not covered. |
| Clean refusals among questions initially labelled unanswerable | 4/6 (66.7%) | Four outputs simply said the information was not covered. |
| Output `refused=true` for questions initially labelled answerable | 1/8 (12.5%) | The metrics question was refused even though `Metrics that matter.pdf` was the top retrieved source. |
| Fully grounded answers among initially answerable questions | 2/8 (25.0%) | Five were scored partial and one failed. |
| Real filename citations among answerable questions that returned an answer | 4/7 (57.1%) | Two were partial and one failed. A real filename alone does not prove every claim is supported. |

**How to read these numbers:** the `refused` field is a Boolean flag, not a complete quality measure. Question 13 shows why: the flag is true, but the output first gives a multi-step roadmap and only then says “Not covered”. The manual `refusal_assessment` field distinguishes clean refusals from mixed behaviour.

## Grounding breakdown for the 8 initially answerable questions

| Manual grounding score | Cases | Share |
|---|---:|---:|
| Pass | 2 | 25.0% |
| Partial | 5 | 62.5% |
| Fail | 1 | 12.5% |

The “partial” group includes answers that were on topic but appeared to mix material across PDFs, add unsupported details, or omit nuance. The full question-level notes are in [`results-scored.csv`](../evaluation/results-scored.csv).

## Citation breakdown for answerable questions that returned an answer

| Manual citation score | Cases | Share |
|---|---:|---:|
| Pass | 4 | 57.1% |
| Partial | 2 | 28.6% |
| Fail | 1 | 14.3% |

Examples from the manual review:

- **Placeholder citations:** the Jobs to be Done answer used `[source.pdf, p.5]` rather than the real filename.
- **Incomplete filename:** the fundraising answer used `[MasterClass.pdf, p.3]` rather than the full source filename.
- **Missing citation:** the TAM/SAM/SOM answer did not provide a usable filename-and-page citation.
- **Citation does not equal grounding:** a real filename was present in several partial answers, but parts of the answer may not have been supported by the cited excerpt.

## Important test-label issue

The question “Explain about project management role?” is currently labelled `no` in `questions.csv`, but the manual review found that `The_Product_Manager.pdf` describes project-manager responsibilities and that the answer appears grounded in that file. This label needs to be checked against the source corpus before using the case in a refusal metric. The headline counts above deliberately preserve the original labels so the results remain traceable to the submitted test set.

If you later correct this label, record the change and recompute the relevant denominators. Do not silently relabel the case after seeing the model output.

## Files for inspection

- [`questions.csv`](../evaluation/questions.csv) — the original questions and expected-answerability labels.
- [`results.csv`](../evaluation/results.csv) — the raw output export, preserved without manual-score columns.
- [`results-scored.csv`](../evaluation/results-scored.csv) — raw outputs plus manual grounding, citation, refusal-quality scores, and notes.
- [`hand-scores.md`](../evaluation/hand-scores.md) — the supplied scoring notes and original summary.
- [`evaluation-cases-template.csv`](../evaluation/evaluation-cases-template.csv) — template for future test cases.

## What I would fix next

1. **Enforce source-grounded citations.** Require exact source filenames and valid page references; reject placeholders such as `[source.pdf, p.5]`.
2. **Separate refusal states.** Record at least `clean refusal`, `answer`, and `mixed answer + refusal`; do not rely only on the Boolean `refused` flag.
3. **Investigate the metrics false refusal.** The top source was `Metrics that matter.pdf`, so inspect the retrieved chunks, refusal condition, and generation prompt before changing retrieval parameters.
4. **Reduce unsupported expansion.** Evaluate claim-by-claim support for long answers, especially where the model merges several documents.
5. **Audit question labels before scoring.** Verify that each “unanswerable” question is truly absent from the document set.
6. **Run a controlled retrieval comparison.** Re-run the same fixed questions with 5 and 10 chunks, recording the setting for every run. The supplied results file does not identify a retrieval setting, so no comparison between those settings can be claimed yet.

## Evaluation limitations

This is a small, manually scored prototype evaluation. It is useful for surfacing failure modes, not for making broad claims about reliability. The scoring depends on the provided human review notes, and the original PDFs/retrieved passages are not bundled here for independent verification. The test set, rubric, raw outputs, and question-level notes should be retained with any future rerun so that changes can be compared consistently.
