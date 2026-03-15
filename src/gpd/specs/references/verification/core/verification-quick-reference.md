---
load_when:
  - "verification checklist"
  - "which checks to run"
  - "medical validation"
  - "quick verification"
tier: 1
context_cost: medium
---

# Verification Quick Reference

Compact reference for medical evidence verification. This file is the conceptual checklist; the stable machine-facing verifier registry lives in `gmd.core.verification_checks` and the `gmd-verification` MCP server. For full procedures, worked examples, and code templates, see the modular verification files: `references/verification/core/verification-core.md` (universal checks), `references/verification/core/verification-numerical.md` (statistical verification), `references/verification/core/computational-verification-templates.md` (R/Python templates), `references/verification/core/code-testing-medical.md` (testing patterns).

---

## Verification Checklist (14 Checks)

| # | Check | What It Catches | When to Use |
|---|-------|-----------------|-------------|
| 1 | **PICO consistency** | Mixed populations, incompatible interventions, outcome definition drift | Every systematic review, before any data synthesis |
| 2 | **Risk of bias assessment** | Unblinded trials, attrition bias, selective reporting, confounding | Every included study, using RoB 2 / ROBINS-I / QUADAS-2 |
| 3 | **Effect measure appropriateness** | OR misinterpreted as RR, MD used when SMD needed, wrong scale | Before every meta-analysis, before interpreting results |
| 4 | **Heterogeneity assessment** | Unexplained variation, inappropriate pooling, hidden clinical differences | Every meta-analysis (Q, I-squared, tau-squared, prediction interval) |
| 5 | **Statistical model validation** | Anti-conservative CIs (DL with few studies), violated assumptions | Every meta-analysis model selection |
| 6 | **Sensitivity analysis** | Fragile results, influential studies, model dependence | After every meta-analysis (leave-one-out, model comparison, RoB-restricted) |
| 7 | **Clinical plausibility** | Implausible effect sizes, wrong direction, biologically impossible | After every analysis, especially surprising results |
| 8 | **Publication bias assessment** | Missing negative studies, small-study effects, reporting bias | Every meta-analysis with >= 10 studies (funnel plot, Egger's test) |
| 9 | **GRADE certainty evaluation** | Overconfident conclusions, missing downgrade domains | Every outcome in every systematic review |
| 10 | **Data extraction verification** | Transcription errors, wrong column/timepoint, unit confusion | Every extracted data point (double-extraction) |
| 11 | **Search completeness** | Missing databases, language bias, grey literature gaps | Protocol stage and before synthesis |
| 12 | **PRISMA compliance** | Unreported methods, missing flow diagram, incomplete reporting | Before submission/publication |
| 13 | **Subgroup analysis integrity** | P-hacking, post-hoc subgroups, missing interaction tests | Every subgroup analysis (interaction p-value required) |
| 14 | **Power/precision adequacy** | Underpowered conclusions, OIS not met, wide prediction intervals | Before concluding "no effect" or "effect confirmed" |

---

## Decision Tree: Which Checks to Run

```
What type of result do you have?
|
+-- Systematic review (qualitative synthesis)
|   +-- ALWAYS: #1 PICO consistency, #2 risk of bias, #11 search completeness
|   +-- ALWAYS: #9 GRADE certainty, #12 PRISMA compliance
|   +-- If comparing studies: #7 clinical plausibility
|   +-- If narrative synthesis: #10 data extraction verification
|
+-- Meta-analysis (quantitative synthesis)
|   +-- ALWAYS: #1 PICO, #2 RoB, #3 effect measure, #4 heterogeneity
|   +-- ALWAYS: #5 model validation, #6 sensitivity, #7 plausibility
|   +-- ALWAYS: #9 GRADE, #10 data extraction
|   +-- If >= 10 studies: #8 publication bias
|   +-- If subgroups: #13 subgroup integrity
|   +-- If non-significant: #14 power adequacy
|
+-- Diagnostic test accuracy review
|   +-- ALWAYS: #1 PICO, #2 RoB (QUADAS-2), #10 data extraction
|   +-- ALWAYS: #7 plausibility (sensitivity/specificity ranges)
|   +-- If pooling: #4 heterogeneity, #5 bivariate model
|   +-- ALWAYS: #9 GRADE (for DTA)
|
+-- Network meta-analysis
|   +-- ALWAYS: All of meta-analysis checks above
|   +-- ALWAYS: Transitivity assessment (PICO across comparisons)
|   +-- ALWAYS: Consistency/coherence (node-splitting)
|   +-- ALWAYS: Ranking (SUCRA/P-score with uncertainty)
|
+-- Individual study appraisal
|   +-- ALWAYS: #2 risk of bias, #7 clinical plausibility
|   +-- If RCT: CONSORT checklist, ITT vs per-protocol
|   +-- If observational: Confounding assessment, STROBE checklist
|   +-- If diagnostic: STARD checklist, spectrum bias
|
+-- Guideline-ready evidence package
|   +-- Run ALL applicable checks from above
|   +-- ALWAYS: Summary of findings table
|   +-- ALWAYS: Evidence-to-decision framework
|   +-- Minimum: PICO, RoB, effect measure, heterogeneity,
|       sensitivity, plausibility, GRADE, data extraction,
|       search completeness, PRISMA
```

---

## Verification Theater vs Real Verification

The difference between checking that a result *exists* and checking that the *evidence synthesis is correct*.

| Verification Theater (Looks Good, Proves Nothing) | Real Verification (Actually Tests the Evidence) |
|---------------------------------------------------|-------------------------------------------------|
| "The meta-analysis code ran without errors" | "Leave-one-out shows no single study changes the conclusion, I-squared = 32%" |
| "We assessed risk of bias" | "Two reviewers independently assessed all 5 RoB 2 domains with kappa = 0.85" |
| "We searched multiple databases" | "PubMed + Embase + CENTRAL + ClinicalTrials.gov + WHO ICTRP, no language restriction, grey literature contacted" |
| "The forest plot looks reasonable" | "All CIs overlap, prediction interval excludes null, effect direction consistent across RoB strata" |
| "We used random-effects" | "HKSJ adjustment applied (k=7), REML tau-squared estimator, sensitivity to DL shows similar result" |
| "We did sensitivity analysis" | "Main result robust to: leave-one-out, low-RoB only, different effect measure, Bayesian model" |
| "The funnel plot is symmetric" | "Egger's p=0.42, trim-and-fill adds 0 studies, Copas selection model unchanged" |
| "GRADE is Moderate" | "Downgraded once for imprecision (OIS not met, CI crosses MID), no other downgrades justified" |
| "The effect is significant (p<0.05)" | "SMD = -0.35 [95% CI -0.52 to -0.18], NNT = 6, clinically meaningful per MID of 0.3 SD" |
| "Heterogeneity is low" | "I-squared = 18%, Q(8) = 9.8, p = 0.28, prediction interval [-0.62, -0.08]" |
| "Subgroup analysis shows difference" | "Interaction test p = 0.03, pre-specified in protocol, biologically plausible mechanism" |
| "Data was extracted by one reviewer" | "Double-extraction with discrepancy resolution, third reviewer arbitration for 3/45 disagreements" |

---

## In-Execution Validation (Catch Errors During the Synthesis)

Do not wait until the end. Validate after every intermediate step:

| After This | Check This | How |
|---|---|---|
| Extracting data from a study | Effect size direction, magnitude, sample size | Recalculate from 2x2 table or means/SDs |
| Calculating an effect size | Correct formula, correct inputs | Cross-check with RevMan/metafor output |
| Running a meta-analysis | Forest plot coherence, heterogeneity | Visual inspection + Q/I-squared check |
| Running sensitivity analysis | Consistency with main result | Compare direction, magnitude, significance |
| Generating funnel plot | Symmetry, outlier identification | Egger's test + visual assessment |
| Performing subgroup analysis | Interaction test, not just subgroup p-values | Formal test for subgroup differences |

**If a check fails mid-execution:** Stop. Record the failure. Diagnose before proceeding. A data extraction error in one study contaminates the entire meta-analysis.

---

## Domain Quick-Checks

### Systematic Review
- PRISMA 2020 flow diagram numbers add up at each stage
- Search strategy reproducible (full search strings provided)
- Eligibility criteria match PICO (no post-hoc exclusions)
- All pre-registered outcomes reported (compare protocol to publication)

### Meta-Analysis
- Effect measure appropriate for outcome type (binary: OR/RR/RD; continuous: MD/SMD)
- Statistical model matches heterogeneity expectation (FE if truly homogeneous, RE otherwise)
- HKSJ adjustment used when k < 10 and using random-effects
- Prediction interval reported alongside CI

### Clinical Trial Appraisal
- CONSORT checklist adherence (randomization, allocation concealment, blinding, ITT)
- Sample size calculation reported and met
- Primary outcome matches registration (ClinicalTrials.gov / ISRCTN)
- Loss to follow-up < 20% and balanced between arms

### Diagnostic Accuracy
- QUADAS-2 all domains assessed
- 2x2 table reconstructable from reported Se/Sp/prevalence
- Spectrum of disease appropriate (not extreme case-control)
- Verification bias assessed (did all patients get reference standard?)

### Observational Studies
- ROBINS-I or Newcastle-Ottawa used appropriately
- Key confounders identified and adjusted for
- Immortal time bias assessed (for cohort studies)
- Healthy user/adherer bias considered (for pharmacoepi)

### Network Meta-Analysis
- Transitivity assumption justified (PICO comparable across all direct comparisons)
- Consistency checked (node-splitting or design-by-treatment interaction)
- Ranking with uncertainty (SUCRA + 95% CrI, not just point ranks)
- Comparison-adjusted funnel plot for small-study effects

---

## Common LLM Error Patterns in Medical Research (Top 5)

1. **Data extraction errors** (wrong column, unit confusion, sign reversal): Caught by double-extraction and recalculation
2. **Effect measure confusion** (OR interpreted as RR, MD when SMD needed): Caught by PICO + scale verification
3. **Heterogeneity dismissal** (pooling with I-squared > 75% without investigation): Caught by heterogeneity checks + prediction interval
4. **Missing GRADE domains** (incomplete certainty assessment): Caught by automated GRADE completeness check
5. **Post-hoc subgroup reporting** (no interaction test, not pre-specified): Caught by protocol comparison + interaction test requirement

---

## See Also

- `references/verification/core/verification-core.md` — Universal checks: PICO consistency, sensitivity analysis, risk of bias, GRADE, statistical validity
- `references/verification/core/verification-numerical.md` — Statistical verification, convergence testing, automated verification
- `references/verification/core/verification-patterns.md` — Index pointing to modular verification files
- `references/verification/core/computational-verification-templates.md` — R/Python code templates for verification
- `references/verification/core/code-testing-medical.md` — Testing patterns for medical research code
