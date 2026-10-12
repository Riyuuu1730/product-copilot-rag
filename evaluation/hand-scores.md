# Hand scores for eval/results.csv

How we scored (do this on every row):

1. **Refusal vs your label** — already in the CSV (`refused` True/False).
2. **Grounded?** — Could this answer have come from the PDFs, without extra invented facts?
   - Pass = mostly from the docs
   - Partial = right topic but mixed, swapped, or padded
   - Fail = refused when it should answer, or invented content
3. **Citation?** — Does it point to a **real filename** (and page), not `[source.pdf]`?

Correct `no` refusals are N/A for citation (saying “Not covered…” is the whole answer).

| # | Question | Your label | Auto refused? | Grounded | Citation | Notes |
|---|---|---|---|---|---|---|
| 1 | What is TAM SAM SOM? | yes | no | Partial | Fail | Right PDF, but TAM/SAM definitions look swapped. No `[file.pdf, p.x]`. |
| 2 | What is Jobs to be Done? | yes | no | Pass | Partial | Christensen PDF is right. Tags are `[source.pdf, p.5]` — fake name. |
| 3 | How do you write a user story? | yes | no | Partial | Pass | Real files cited. Stitches two PDFs; some “three purposes” may not be from the story PDF. |
| 4 | What metrics matter for a product? | yes | **yes (wrong)** | Fail | N/A | Top source is `Metrics that matter.pdf` — retrieve OK, generate refused. |
| 5 | TAM/SAM/SOM used for fundraising? | yes | no | Pass | Partial | Use-case is on-topic. Cite is `[MasterClass.pdf, p.3]` not the full filename. |
| 6 | CEO personal phone number? | no | yes | Pass (refuse) | N/A | Clean refuse. |
| 7 | Stock price in 2019? | no | yes | Pass (refuse) | N/A | Clean refuse. |
| 8 | Office WiFi password? | no | yes | Pass (refuse) | N/A | Clean refuse. |
| 9 | Customer journey for subscription model? | yes | no | Partial | Pass | Right file. “Subscription / acquisition / onboarding” may be extra vs the excerpt. |
| 10 | Explain about RAG technology? | no | yes | Pass (refuse) | N/A | Clean refuse. RAG is not in these PDFs. |
| 11 | Explain about different sprint. | yes | no | Partial | Pass | Scrum/XP look doc-based. “Productboard Sprint” as a sprint *type* is a stretch. |
| 12 | Role of product owner in agile? | yes | no | Partial | Pass | Real cite, very thin; “inbound only” may be incomplete. |
| 13 | Software engineering roadmap / career? | no | yes* | Fail | Fail | Wrote a PM essay, *then* “Not covered…”. Auto-score = refuse; product = hallucination + refuse. |
| 14 | Explain about project management role? | no | no | Pass (from docs) | Pass | Label `no` is the issue: `The_Product_Manager.pdf` *does* describe PM. Copilot was more right than the label. |

## Write-up numbers (honest)

- Auto refusal on `no`: **5/6**
- Auto false refusal on `yes`: **1/8**
- Clean refuses among the 6 `no`s: **4/6** (rows 6–8, 10). Row 13 is a dirty refuse. Row 14 is not a refuse (label mismatch).
- Strong grounded answers among 8 `yes`: **2/8** (JTBD, fundraising). Rest partial or fail.
- Real filename citations among 7 `yes` that answered: **4/7** (user story, journey, sprint, product owner).

What you’d fix next: stricter citations (or Claude), don’t count “not covered” if there is extra text, retune metrics question (chunking or k), fix eval labels that are actually in the corpus.
