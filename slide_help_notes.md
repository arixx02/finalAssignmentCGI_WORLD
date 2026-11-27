# Slide Help Notes — for writing the deck yourself

**Your file:** `ArielDeBrito.ipynb` (re-run it in Colab first — this is your reproducibility proof)
**Limit:** 15 slides total, *including* title, TOC, dividers and references → PDF, < 10 MB
**Audience:** client executives with no ML background
**Grading emphasis:** the *business proposal*, not the model

Use these notes as scaffolding. The wording is yours — every number below is copied
straight out of the notebook outputs, so you can paste them safely.

---

## 0. The three blocks you MUST write in words

The rubric says: *"Please explicitly write the model name, evaluation metric, and
score in the slide. Using only visualizations (screenshots, figures, etc.) is not
enough."* So at least two slides need a text block like this:

**Model A — paste verbatim**

```
Model:      XGBoost binary classifier (gradient-boosted decision trees)
Target:     customer churn (yes/no)
Evaluation: ROC-AUC (threshold-free ranking quality)
Score:      0.6964 on a held-out 30% test set (n = 30,000)
Also:       accuracy 0.6377 vs 0.5044 baseline; F1 0.6380
Why this:   handles mixed scales, missing values and non-linear effects;
            gain-based importance tells us which features to act on
```

**Model B — paste verbatim**

```
Model:      XGBoost regressor (gradient-boosted decision trees)
Target:     average monthly revenue per customer (rev_Mean, USD)
Evaluation: R² and MAE on a held-out 30% test set (n = 29,893)
Score:      R² = 0.8881, MAE = $7.34, RMSE = $15.05
Baseline:   always predict the average → MAE = $28.82 (the model is 4× better)
Why this:   we need a dollar figure per customer, not a high/low bucket,
            because the lift calculation multiplies it out
```

**Optional one-liner for both:** *"A deliberately leaky first version scored
R² = 0.9775; we removed the columns that were just restatements of the target and
reported the honest 0.8881 instead."* — this shows the markers you know what
leakage is. It is a strong 10-second answer if asked.

---

## 1. Suggested 15-slide structure

> If you want section dividers, drop the Agenda (slide 2) and merge Model A's two
> slides (8 + 9) — that frees 2 slots.

| # | Slide | Figure / asset | Headline message (yours to polish) |
|---|---|---|---|
| 1 | Title | — | "Business revenue lift from churn and customer usage — a two-model proposal for Company A" + your name + date |
| 2 | Agenda | — | 4 sections: Problem → Evidence → Models → Proposal |
| 3 | Market context & the client's problem | — | Mature market: growth = more revenue per existing subscriber, not more subscribers. Retention is the cheapest revenue lever. |
| 4 | Data | `03_missing_values`, `04_correlation_heatmap` | 100,000 customers, two linked tables, 100 columns; missingness concentrated in household data and *handled*, not guessed |
| 5 | The finding that shapes everything | `01_churn_balance`, `02_revenue_by_churn` | Churn ≈ 50% and **churners spend the same as stayers** → value alone and risk alone both fail |
| 6 | Our approach | — (draw it in shapes) | Two models → multiply → one ranked list → simulate the campaign |
| 7 | Signals we engineered | `05_engineered_signals_vs_churn` | 19 new features built for business meaning (usage trend, overage share, failed calls, equipment age) |
| 8 | **Model A results** | `06_churn_model_eval` | **Name / metric / score block** + ROC curve + confusion matrix |
| 9 | What drives churn | `07_churn_importance` | 10 of the top 15 features are things the business can actually change |
| 10 | **Model B results** | `08_revenue_model_fit`, `09_revenue_importance` | **Name / metric / score block** + what really drives a customer's monthly bill |
| 11 | Prioritising customers | `10_risk_deciles_gains` | The combined score captures **32.6%** of churned revenue from the top 10% — vs 17.7% for churn-only |
| 12 | Campaign economics | `11_lift_vs_baseline` | **$41,549/month → $498,586/year net lift**, 1.08× ROI |
| 13 | When is it still worth it? | `12_sensitivity_heatmap` | Break-even at only **~15%** churn reduction with a 5% discount — the black line |
| 14 | **The recommendation** | `13_segment_levers` | Where the money is: overage exposure, equipment age, the 1–2-year renewal window |
| 15 | Assumptions, risks & references | — | The 8-row assumptions register + full reference list |

---

## 2. Slide-by-slide notes

**Slide 3 — Market context (needs citations on-slide)**
- GSMA: mobile generated **$7.6 trillion = 6.4% of global GDP in 2025**; **45% of
  operators now name revenue monetisation a strategic priority** → growth comes from
  the installed base. *(footnote: GSMA, The Mobile Economy 2026)*
- HBR/Bain: acquiring a customer costs **5–25×** more than retaining one; **+5%
  retention = +25–95% profit**. *(footnote: Gallo, HBR 2014)*
- Client problem in one line: *"Company A has ~100,000 customers and no way to say
  who is leaving or what each is worth."*

**Slide 5 — the pivot of the whole deck**
- Say this out loud: *"Churners spend \$58.21 a month, stayers spend \$59.22. So a
  'save our best customers' campaign would target the wrong people."*
- Then: *"And a churn-only campaign would happily save a \$5 customer with the same
  enthusiasm as a \$500 one."* → this is the sentence that justifies two models.

**Slide 6 — draw the pipeline (no figure exists, use SmartArt/shapes)**
```
[Usage & billing data] ──► MODEL A: will they leave?   ──► p(churn)
                                                        │
[Customer profile]     ──► MODEL B: what are they worth?─► revenue
                                                        │
                                              p(churn) × revenue
                                                        │
                                              = REVENUE-AT-RISK ranking
                                                        │
                                              campaign simulation ──► lift $
```

**Slide 8 — Model A**
- Metrics: ROC-AUC **0.6964**, accuracy **0.6377** (baseline 0.5044), F1 0.6380.
- Read the confusion matrix for them: at threshold 0.50 the model finds 9,576 of
  14,869 churners.
- Business read of the operating point: *contacting the top 10% of scores, the batch
  churns **79.3%** of the time versus a **49.6%** base rate — a 1.6× concentration.*
- If challenged on "is 0.70 good?": churn is genuinely hard to predict from billing
  history alone; the model's job is **ranking**, and ranking is exactly what the
  simulation needs (point at slide 11).

**Slide 10 — Model B**
- R² **0.8881**, MAE **\$7.34** vs **\$28.82** baseline, RMSE \$15.05.
- Mention the leakage audit honestly — it is a differentiator, not a weakness.
- Top revenue drivers (figure 09): usage volume and overage behaviour, not demographics.

**Slide 11 — the money slide for the two-model argument**

| Top 10% of base contacted | Churned revenue captured |
|---|---|
| Random | 10% |
| Customer value only (Model B) | 26.5% |
| Churn risk only (Model A) | 17.7% |
| **Revenue-at-risk (A × B)** | **32.6%** |

**Slide 12 — economics (base case)**
- Budget: **9,964 customers** (top 10%).
- Cost: **\$38,489/month** = \$15,942 outreach + \$22,546 discounts.
- Revenue kept: **\$80,038/month**.
- **Net: \$41,549/month → \$498,586/year. ROI 1.08×.**
- **+$302,315/year vs spending the same budget on churn probability alone (+154%)**;
  +\$469,147 vs random.
- Assumptions in a corner: \$2/contact, 80% reach, 40% accept, 5% discount, 30%
  efficacy, 1-month horizon ×12.

**Slide 13 — sensitivity**
- The black line is break-even; everything green is profit.
- Read-out: *"the campaign stays profitable once the offer removes just ~15% of a
  customer's churn risk at a 5% discount — 10% if no discount is needed."*

**Slide 14 — recommendation (the actual proposal)**
- **Who:** top 10% by revenue-at-risk (9,964 customers).
- **What:** plan migration that removes the overage bill shock + handset renewal path
  for accounts over 18 months of equipment age.
- **When:** the 1–2-year renewal window (42,868 customers, 51.8% churn).
- **Evidence:** high-overage customers are 14.7% of the base but churn 56.1% vs 48.0%
  *and* spend \$98.08 vs \$46.25 → they are both the most valuable and the most
  fragile, and they are 57% of the contact list.
- **Rejected hypothesis (say it proudly):** falling usage looked promising in EDA but
  does not hold in the scored population, so it is **not** in the offer.
- Expected impact: **$498,586 net revenue lift per year.**

**Slide 15 — assumptions, risks & references**
- Paste the 8-row assumptions register from the notebook (contact cost, contact
  rate, acceptance, discount, efficacy, horizon, churn window, constant prices).
- Honest risk to name yourself: this dataset records ~50% churn over its observation
  window, so absolute dollars are large; *the ranking effect — the reason to use two
  models — does not depend on that*, but a conservative scenario would scale the
  absolute lift down.
- References (see below).

---

## 3. References (paste into Omnicampus + footnote on slides)

1. GSMA (2026). *The Mobile Economy 2026.* https://www.gsma.com/solutions-and-impact/connectivity-for-good/mobile-economy/ — report PDF: https://www.gsma.com/solutions-and-impact/connectivity-for-good/mobile-economy/wp-content/uploads/2026/02/The-Mobile-Economy-2026.pdf
2. Gallo, A. (2014). "The Value of Keeping the Right Customers." *Harvard Business Review.* https://hbr.org/2014/10/the-value-of-keeping-the-right-customers
3. Chen, T. & Guestrin, C. (2016). *XGBoost: A Scalable Tree Boosting System.* KDD. https://arxiv.org/abs/1603.02754
4. Pedregosa et al. (2011). *Scikit-learn: Machine Learning in Python.* JMLR 12. Metrics: https://scikit-learn.org/stable/modules/model_evaluation.html
5. Company A (client-provided). `ENG_Company _A_ Dataset Overview.docx`; `telecom/Client.csv`, `telecom/Record.csv`.
6. OpenAI/this tooling — the generative-AI chat history you were asked to attach
   (local record: `chat_history.md` / `chat_history.txt`).

---

## 4. Rubric self-check before you export

- [ ] Model **name**, evaluation **metric**, and **score** written in words on slides 8 and 10
- [ ] Why *each* model was chosen, and how it was built (one line each)
- [ ] At least one market/external source **cited on the slide** where it appears, plus the reference list
- [ ] A recommendation with a **dollar figure** and **stated assumptions**
- [ ] No ML jargon unexplained (say "a score from 0 to 1 = likelihood", not "probability calibration")
- [ ] Logical chain visible: data → finding → model → ranking → simulation → recommendation
- [ ] All 13 figures used or deliberately dropped; none stretched beyond readability
- [ ] PDF, ≤ 15 slides, < 10 MB, named `ArielDeBrito.pdf`
- [ ] Notebook named `ArielDeBrito.ipynb` (done) and re-run in Colab

## 5. Five questions a CFO will ask — have these ready

1. *"Why 10%? Why not 5% or 20%?"* → the notebook's simulation cell takes a budget
   parameter; re-run it at any size in seconds. At 10% the ROI is 1.08×; the gains
   chart (slide 11) shows the capture curve, so you can pick the knee.
2. *"What if your efficacy assumption is wrong?"* → slide 13 answers it: break-even
   is ~15% efficacy at a 5% discount; below that the campaign still pays for itself.
3. *"What does a contact cost?"* → $2.00, and it is the most transparent line in the
   model — change it and re-run.
4. *"Is 0.70 AUC reliable?"* → it is a held-out score on 30,000 customers, never
   trained on; and the model's purpose is ranking, which slide 11 validates against
   *realised* churned revenue.
5. *"Why should we trust the two-model design?"* → slide 11's table: the combined
   ranking captures 32.6% of lost revenue where churn-only captures 17.7%.
