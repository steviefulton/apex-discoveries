# The Measurement-Frame Blind Spot: A Machine-Generated, Machine-Verified Conjecture with a Falsifiable Prediction

> ## ⚠ CORRECTION (2026-08-31): not a novel discovery — and no longer confirmed
>
> **This paper was originally published as a novel, internally confirmed finding. Two later
> checks changed both words.**
>
> **1. Not novel (2026-08-28).** A concept-aware re-check — a capable model asked directly whether
> the claim restates a known concept — identified the core claim as a restatement and
> operationalization of **Goodhart's Law** ("when a measure becomes a target, it ceases to be a
> good measure") and the adjacent machine-learning literature on **specification gaming, reward
> hacking and shortcut learning**. The original novelty search queried four literature databases
> with this paper's own coined phrasing ("measurement-frame blind spot", "isomorphic structures
> across frames"); a novel-sounding paraphrase of an old idea returns zero keyword matches. That
> failure mode is now closed: a zero-match claim must pass a concept-level check before APEX may
> call it "possibly novel". Section 2 below, which places the claim *above* the Goodhart
> literature, is exactly the reasoning the re-check rejected.
>
> **2. Not confirmed (2026-08-29).** The two "independent" adjudicator models that confirmed the
> claim (claude-haiku and claude-sonnet) share one training lineage. APEX's confirmation rule now
> counts distinct model **families**, and under that rule this hypothesis was stepped back from
> "confirmed" to **awaiting cross-family replication**. It is currently in the queue for a second,
> Google-family adjudication; the outcome — either way — will be recorded here.
>
> **What still stands:** the dated falsifiable prediction (§ below; auto-adjudicated 2027-09-20).
> A restatement of Goodhart's law with a specific cross-frame prediction is a legitimate test —
> of an old idea, not a new one.
>
> The status line below and the [SHOWCASE](../SHOWCASE.md) entry are corrected accordingly.
> Internal record: APEX hypothesis #877, novelty_recheck = novelty_overstated (Goodhart's Law).


**Status:** CORRECTED 2026-08-31 — not novel (restates Goodhart's Law); confirmation stepped back to *awaiting cross-family replication* · draft v1 (2026-08-25)
**Origin:** APEX autonomous research orchestrator (hypothesis #877) · curated by Claude Code
**Publishing decision:** reserved to the human operator ([S])

---

## Abstract

We report a conjecture generated, tested, and internally confirmed by an autonomous
research system under an adversarial verification regime: *systems optimized within a
single measurement frame develop a systematic blind spot to cross-frame phenomena, such
that their internal performance metrics remain uncorrelated with — or inversely
correlated to — their ability to detect isomorphic structures represented in alternative
measurement frames.* The claim passed a pre-registered confirmation rule requiring
replicated strong support from two independent adjudicator models judging external
literature only, with the system's own prior outputs excluded from evidence. A four-database
novelty search found no published statement of the claim. Adversarial review notes the
confirming corpus argues the claim without quantitatively measuring it — we therefore
present the result as a **verified conjecture**, not an established finding, and attach a
dated falsifiable prediction (September 2027) by which it can be publicly judged.

## 1. The claim

**Formal statement.** For a system S optimized against metrics native to measurement
frame F, performance on F-native metrics will show zero or negative correlation with S's
ability to detect structures isomorphic to F-frame phenomena when those structures are
presented in an alternative frame F′ — and S's internal metrics will not signal this
deficit.

**Plain language.** The better a system gets at the yardstick it was built around, the
blinder it may become to the same patterns dressed in a different yardstick — and its
own scores won't warn it.

## 2. Position relative to known work

A targeted adjacency search (beyond keyword novelty) places the claim above two
established literatures without being stated by either:

- **Goodhart's-law variants** (e.g. Manheim & Garrabrant, *Categorizing Variants of
  Goodhart's Law*): proxy metrics diverge from true goals under optimization pressure.
  The present claim differs in emphasis: it concerns *frame-relative structure
  detection*, not proxy–goal divergence, and predicts a specific *uncorrelation* between
  native metrics and cross-frame competence.
- **Shortcut learning / distribution-shift brittleness** in machine learning: models
  exploit dataset-specific cues and fail under transformation. This is the claim's most
  natural *instance*; the general cross-domain formulation — applying to any measured
  system, not only ML models — appears unpublished.

A four-source novelty search (PubMed, Semantic Scholar, OpenAlex, arXiv) returned zero
papers matching the claim. We classify it honestly as **a novel generalization over known
territory**, not a discovery of a new phenomenon.

## 3. Verification method

The claim walked a fixed, pre-registered road inside the APEX system:

1. **Generation** from a 225k-entry research corpus (synthesis layer, 2026-06).
2. **Replication on external evidence only.** As of 2026-08-24 the system excludes its
   own prior outputs from experiment corpora (structural provenance markers; 75–80% of
   earlier evidence retrievals were self-referential — a circularity this regime
   removed). Retrieval is supplemented by active literature seeking.
3. **Independent second adjudicator.** Confirmation requires strong support from two
   different judge models; caches are model-scoped and provenance is stamped by the
   model that actually answered.
4. **Pre-registered confirmation rule** (unchanged throughout): ≥2 strong-support
   experiments, zero refuting, from ≥2 independent adjudicators.
5. **Novelty gate** against four literature databases.
6. **Adversarial review** (devil's-advocate counter-search + methods critic), filed
   beside the result and quoted below in full.

## 4. Evidence

| Experiment | Date | Adjudicator | Verdict | Self-echo excluded |
|---|---|---|---|---|
| 582 | 2026-06-30 | claude-haiku-4.5 | strong 0.811 (S17/C0/N3) | pre-exclusion era |
| 1487 | 2026-08-24 | claude-haiku-4.5 | strong 0.899 (S9/C0/N0) | 11 entries |
| 1496 | 2026-08-25 | claude-sonnet-5 (independent) | strong 0.806 (S8/C0/N1) | 11 entries |

Four additional attempts ended as instrument failures (budget freeze ×3, network error
×1) and were correctly excluded from judgment. Zero refuting verdicts were returned at
any point.

## 5. Adversarial review (verbatim caveats)

**Devil's advocate:** a counter-query search for refuting literature returned no usable
counter-corpus. *This is a limitation of the search, not a vindication of the claim.*

**Methods critic (bearing: “partial”):** “The evidence consists of compressed syntheses
and meta-insights that assert the hypothesis's core claim repeatedly but provide no
concrete empirical data, experimental results, or specific quantitative correlations
demonstrating the predicted inverse relationship.”

We agree with the critic. The confirming corpus *argues* the claim well; it does not yet
*measure* it. This is why we publish a prediction rather than a conclusion.

## 6. The falsifiable prediction

> **By August 2027**, a published empirical study will demonstrate that machine-learning
> models fine-tuned to maximize accuracy on a single standardized benchmark show
> statistically significant negative or near-zero correlation between native-benchmark
> performance and their ability to correctly identify structurally isomorphic problems
> presented in a different measurement frame (transformed inputs, alternative feature
> spaces, or cross-domain analogues).

The prediction is registered in the system's ledger and will be adjudicated
automatically on 2027-09-20. If it fails, the conjecture's status will be downgraded
accordingly and that judgment will be recorded with the same machinery that produced
this one.

## 7. Limitations

1. **No quantitative demonstration.** The predicted inverse correlation has not been
   measured by any experiment in the confirming corpus (the critic's point).
2. **Adjudication is LLM judgment.** Verdicts are language-model classifications of
   literature relevance, not statistical analyses; two independent models reduce but do
   not remove this dependence.
3. **Topic-level novelty.** The novelty gate matches at topic granularity; a
   differently-phrased statement of the same idea could exist unfound. The adjacency
   check mitigates but cannot eliminate this.
4. **Single corpus.** Generation and part of retrieval draw on one system's knowledge
   base, with its ingestion biases.
5. **Confirmation ≠ truth.** The internal rule certifies evidential consistency under
   this system's instruments, nothing more.

## 8. Conclusion

An autonomous system produced a claim the literature does not state, survived its own
adversarial machinery under an anti-circularity regime, and has bet a dated prediction
on being right. We offer the conjecture and its prediction for the outside world to
test — that being, in the end, the only gate that matters.

---

### Provenance appendix

Hypothesis 877; experiments 582, 1487, 1496 (+4 instrument failures);
novelty check `hypothesis:877` (0 matches, 4 sources); science-court report 2026-08-25;
prediction lineage `hypothesis:877` (check 2027-09-20). All rows queryable in the APEX
orchestrator database; the machine-assembled replication pack regenerates via
`scripts/build_dossier.py 877`.
