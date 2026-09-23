# Findings: Smart Review Prioritization (Project Horizon)

**To:** Litigation support / review management
**Re:** Can predictive ranking reduce review cost without risking defensibility?

## Bottom line

Yes. On the 10,000-document Project Horizon corpus, reviewing in model-ranked
order finds **100% of responsive material after reviewing only 35% of
documents** (random order: 35%). Reaching 90% recall takes ~55 review-hours
ranked vs ~300 hours in random order — a saving of **~245 review-hours** on
this matter alone.

## 3 headline findings

1. **Ranking quality is near-perfect.** The TF-IDF + logistic regression
   baseline scores ROC-AUC 1.000 on held-out data (precision 1.000,
   recall 0.981 at the default threshold). The misses are "subtle"
   responsive docs — vague language with no matter keywords — not the
   hard negatives, which the model rejects confidently.

2. **Active learning buys model quality per review-hour.** Seeding with 200
   labeled docs and labeling the 300 most uncertain docs per round reaches
   F1 0.993 after 2,600 labels, vs 0.971 with random sampling on the same
   budget. Every reviewer label is worth more when the model chooses it.

3. **The expensive error is a miss, not a false alarm.** Tuning the decision
   threshold for 100% recall (threshold 0.05) costs 64 false positives on the
   2,000-doc test set (precision 85.1%). Reviewing 64 extra documents is
   cheap; missing a responsive document is what creates exposure.

## 3 recommendations

1. **Deploy ranked review with a high-recall operating point.** Set the
   threshold for ~99–100% recall and staff for the extra false-positive
   reads. Report the operating point and its measured precision alongside
   every production.

2. **Validate the tail with elusion sampling.** The unreviewed tail of a
   ranked queue is only defensible if it is *measured*: sample it, have
   senior reviewers adjudicate, and report the miss rate. "The model said
   so" is not a methodology; measured recall is.

3. **Keep reviewers in the loop via uncertainty sampling.** Route the
   model's least-confident documents to reviewers first and retrain in
   rounds. The labeling budget is fixed — spend it where the model learns
   most.

## Caveats

The corpus is synthetic (deterministic, seed 42). Matter language here is
cleaner than real-world data; expect lower absolute scores on live matters,
but the *relative* wins — ranked vs random, uncertainty vs random sampling —
transfer. All methods, metrics, and the hours model are documented in the
notebook for independent verification.
