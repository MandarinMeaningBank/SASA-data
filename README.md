# SASA data

Numerical supplementary data for Structure-Aware Syntactic Accuracy (SASA) research on the Treebank of Learner English (TLE).

## Scope of this release

This release contains dependency-relation diagnostics for **5,124 sentence pairs from 924 learners and 10 L1 groups**. It is a **data-only, exploratory release**, not a release of the original SASA scoring or BERT prediction experiments.

**Sentence-level SASA scores are outside the scope of this deposit and are not included.** Scores were not recovered or recomputed for this release.

## Files

- [`data/learners.csv`](data/learners.csv): 924 pseudonymous learners, L1 group, examination year, and number of paired sentences.
- [`data/sentence_coverage.csv`](data/sentence_coverage.csv): 5,124 pseudonymous sentence pairs and token/alignment coverage counts. This file **does not contain SASA scores**.
- [`data/learner_relation_rates.csv.gz`](data/learner_relation_rates.csv.gz): 19,243 learner-by-relation observations, including inconsistency numerators, denominators, and rates.
- [`data/arc_disagreements.csv.gz`](data/arc_disagreements.csv.gz): 84,010 reference-dependency opportunities, with comparison eligibility and disjoint inconsistency flags. No lexical forms or source text are included.
- [`results/`](results/): pooled and equal-learner relation profiles, CLC-adjusted estimates, label-confusion counts, alignment sensitivity, and sample composition.
- [`metadata.json`](metadata.json): versions, analysis settings, provenance hashes, units, and omitted datasets.
- [`validation_checks.json`](validation_checks.json): checks performed on this release.
- [`manifest_sha256.csv`](manifest_sha256.csv): file sizes, row counts, and SHA-256 checksums. The manifest does not hash itself.

The two `.csv.gz` files are ordinary gzip-compressed UTF-8 CSV files. Decompress them with a standard archive utility; no repository code is required.

## Interpretation

Dependency categories are defined by their labels in the corrected structures. Tokens labelled `punct` or `root`, and tokens with UPOS `PUNCT`, were excluded. A dependency is comparable only when its dependent is aligned and its governors can be identified across the two structures. An inconsistency is a **dependency-label mismatch, an aligned-governor mismatch, or both**. Unaligned dependents and unresolved governors are excluded from that denominator, not counted as automatically correct or incorrect.

The main diagnostics contain 63,497 comparable dependencies and 1,527 inconsistencies (2.4048%). They use **UD 2.3** and are not an exact decomposition of the historical SASA implementation using UD 1.x annotations. See [definitions and units](DATA_DICTIONARY.md) and [limitations](KNOWN_LIMITATIONS.md).

## Privacy and source access

Learner and pair identifiers have been replaced with new pseudonyms. Original/corrected sentences, token strings, FCE XML, original document paths, identifier crosswalks, individual CLC/E1–E4 marks, age information, model vectors, prediction files, and code are not redistributed here. The original corpus must be obtained from its authorised source under its own terms. See [sources and use conditions](DATA_SOURCES_AND_TERMS.md).

The omission of individual CLC marks and the original-to-release identifier crosswalk limits independent reproduction of the proficiency-adjusted models from this repository alone. The adjusted estimates, standard errors, intervals, model sample sizes, and reference CLC mean are retained as numerical research outputs.

## Citation of upstream data

Berzak, Y., et al. (2016). [Universal Dependencies for Learner English](https://aclanthology.org/P16-1070/).

Yannakoudakis, H., Briscoe, T., & Medlock, B. (2011). [A New Dataset and Method for Automatically Grading ESOL Texts](https://aclanthology.org/P11-1019/).

Prepared on 30 October 2026. This release adds no code and makes no new confirmatory statistical claims.
