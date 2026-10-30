# Data dictionary

All tables use one header row. Missing numeric values are empty cells, not zero. Unless a column explicitly says `percent`, rates, standard errors, and confidence bounds use the **0–1 proportion scale**. Linear-model bounds are retained even if they fall outside that interval.

## Identifiers and joins

`learner_id` is a release-specific pseudonym (`L0001`–`L0924`). `pair_id` is a release-specific paired-sentence pseudonym (`P0001`–`P5124`). Neither is an original corpus identifier. Join sentence and learner-by-relation records to `data/learners.csv` using `learner_id`; join arc records to sentence coverage using `pair_id`. Arc rows are uniquely identified by `(pair_id, corrected_token_id)`; learner-by-relation rows by `(learner_id, relation)`.

## Learners and sentence coverage

- `L1`: one of the ten reported first-language groups, not a causal attribution.
- `year`: examination year (2000 or 2001), not the release year or the TLE random train/dev/test split.
- `n_sentence_pairs`: number of available paired TLE sentences for the learner, not all sentences in the original script.
- `original_tokens`, `corrected_tokens`: token counts in each annotation.
- `aligned_tokens`: one-to-one aligned token pairs under the main alignment procedure.
- `spelling_restored_alignments`: aligned pairs whose original lexical forms differ case-insensitively after eligible spelling restoration.
- `unaligned_original_tokens`, `unaligned_corrected_tokens`: tokens without a counterpart under that alignment.

## Arc-level flags

`relation` is the corrected-side UD relation, retaining subtypes. `original_relation` is the aligned learner token's UD relation, or `UNALIGNED` when no eligible dependent alignment exists. `corrected_token_id` is the within-sentence token index, not a learner identifier.

All flags are 0/1:

| Field | Meaning |
|---|---|
| `dependent_aligned` | A non-punctuation learner dependent has been aligned to the corrected dependent. |
| `eligible_arc` | The dependent is aligned and the relevant governors are identifiable across the pair; ROOT is treated as a common sentinel. |
| `label_mismatch_aligned` | The aligned dependents have different UD labels; its denominator is all aligned dependents. |
| `label_only` | Eligible arc with a label mismatch but no aligned-governor mismatch. |
| `head_only` | Eligible arc with an aligned-governor mismatch but no label mismatch. |
| `both` | Eligible arc with both mismatches. |
| `arc_mismatch` | `label_only + head_only + both`; zero outside the comparable denominator is a storage convention, not evidence of correctness. |
| `unresolved_governor` | The dependent is aligned but the arc cannot enter the comparable denominator. |

Word-order differences are not a separate inconsistency flag. They can affect alignment and may be reflected in label/governor correspondence. Missing or unaligned target dependents remain outside `eligible_arc`.

## Relation counts and rates

- `corrected_opportunities`: corrected-side non-punctuation, non-root opportunities.
- `aligned_dependents`: sum of `dependent_aligned`.
- `eligible_arcs`: sum of `eligible_arc`.
- `label_mismatches_aligned`: sum of `label_mismatch_aligned`.
- `label_only`, `head_only`, `both`, `mismatches`: corresponding flag sums; `mismatches` sums `arc_mismatch`.
- `unresolved_governors`: aligned dependents whose governors cannot be compared.
- `unmatched_dependents`: `corrected_opportunities - aligned_dependents`.
- `arc_mismatch_rate`: `mismatches / eligible_arcs`.
- `label_mismatch_rate_aligned`: `label_mismatches_aligned / aligned_dependents`. This is not interchangeable with the preceding rate.
- `unmatched_rate`: `unmatched_dependents / corrected_opportunities`.
- `equal_learner_rate`: mean of observed learner-specific relation rates, giving each eligible learner equal weight.
- `pooled_rate`: summed numerators divided by summed denominators.
- `n_learners`: number contributing at least one comparable opportunity for that relation and group.

The main arc inconsistency rate conditions on comparability. It is not an overall grammatical-error rate.

## Adjusted profiles and uncertainty

`l1_relation_clc_adjusted.csv` contains equal-learner OLS estimates from `relation rate ~ L1 + centred CLC`, with sum coding and HC3 robust covariance. `reference_CLC` is the unrounded learner-weighted CLC mean; its manuscript display is 27.882. `n_learners_model` is relation-specific. `adjusted_rate`, `standard_error_HC3`, `ci_low`, `ci_high`, and `CLC_coefficient` retain the original model output.

For `relation_summary.csv` and `l1_relation_equal_learner.csv`, `ci_low`/`ci_high` are learner-bootstrap 95% intervals (2,000 resamples). For adjusted profiles, they are pointwise HC3 normal-approximation intervals, not pairwise tests. No Tukey-adjusted pairwise comparisons are supplied by this relation-diagnostic release.

`l1_profile_table_adjusted.csv` contains the selected `ccomp`, `obl`, `obj`, and `det` adjusted estimates as **proportions**, despite manuscript tables displaying percentages. `adjusted_profile_ranges.csv` has relation-specific minimum and maximum adjusted estimates across L1 groups; these do not establish an L1 ranking.

## Other result tables

- `label_confusions.csv`: counts of differing `original_relation`/corrected `relation` combinations among aligned dependents, including cases without comparable governors. It has a different denominator from eligible arcs.
- `learner_prevalence_min5.csv`: learners with at least five comparable instances for a relation. `learner_prevalence` is the proportion with at least one inconsistency, not a stable competence estimate.
- `alignment_sensitivity.csv`: L1-by-relation counts under each alignment method.
- `alignment_sensitivity_by_relation.csv` / `alignment_sensitivity_overall.csv`: comparable dependencies, mismatches, and their ratio under each method.
- `alignment_method`: `spell_window` (main, spelling restoration and window ±3), `exact_window` (same window without spelling restoration), or `spell_global` (full-sentence monotonic exact sequence alignment after restoration).
- `sample_by_l1.csv`: learner and paired-sentence counts by L1.

The JSON metadata records the original dependency-diagnostic software versions and random seeds. The checksum manifest records the bytes actually published.
