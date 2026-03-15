---
load_when:
  - "meta-analysis verification"
  - "heterogeneity assessment"
  - "publication bias"
  - "forest plot"
  - "sensitivity analysis"
  - "effect measure selection"
  - "random effects model"
  - "pooled estimate"
tier: 2
context_cost: large
---

# Verification Domain — Meta-Analysis

Heterogeneity assessment, publication bias detection, sensitivity analysis, subgroup analysis validity, effect measure appropriateness, model selection, and forest plot accuracy for meta-analyses.

**Load when:** Working on meta-analytic methodology, heterogeneity evaluation, publication bias assessment, effect size pooling, or forest plot interpretation.

**Related files:**
- `../core/verification-quick-reference.md` — compact checklist (default entry point)
- `../core/verification-core.md` — foundational verification principles
- `references/verification/domains/verification-domain-systematic-review.md` — systematic review (for search and inclusion)
- `references/verification/domains/verification-domain-statistics.md` — statistical methods (for test assumptions)
- `references/verification/domains/verification-domain-epidemiology.md` — epidemiology (for confounding in observational pooling)

---

<heterogeneity_assessment>

## Heterogeneity Assessment

**I-squared (I²) statistic:**

```
I² = ((Q - df) / Q) * 100%
where Q = Cochran's chi-squared statistic, df = k - 1 (k = number of studies)

Interpretation (Higgins thresholds):
  I² = 0-40%:    Might not be important
  I² = 30-60%:   May represent moderate heterogeneity
  I² = 50-90%:   May represent substantial heterogeneity
  I² = 75-100%:  Considerable heterogeneity

Verification:
1. CHECK: I² alone is insufficient. A low I² with few studies does NOT mean homogeneity —
   I² has very wide confidence intervals when k < 10.
   With k = 5: 95% CI for I² = 0% is approximately [0%, 79%].
2. CHECK: I² is reported with its 95% confidence interval.
   If CI not reported: compute from H² = Q / (k-1), then I² = 1 - 1/H².
3. COMPUTE: Prediction interval — shows the range of true effects expected in a NEW study.
   If prediction interval crosses the null: effect may not be present in all settings,
   even if the pooled estimate is significant.
   PI = mu ± t_{k-2, 0.975} * sqrt(tau² + SE²_pooled)
4. DO NOT use I² to decide between fixed and random effects models.
   This is a common methodological error. Model choice depends on the research question
   (conditional vs unconditional inference), not on observed heterogeneity.
```

**Tau-squared (τ²) — between-study variance:**

```
Estimation methods:
- DerSimonian-Laird (DL): most common, but can be biased with few studies.
- Restricted maximum likelihood (REML): preferred for continuous outcomes.
- Paule-Mandel (PM): robust for small meta-analyses.
- Hartung-Knapp-Sidik-Jonkman (HKSJ): adjusts CI for tau² uncertainty.
  CRITICAL: HKSJ should be used instead of standard DL for random-effects CIs
  when k < 20. Standard DL CIs are anti-conservative with few studies.

Verification:
1. CHECK: tau² estimation method is stated. "Random effects" without specifying
   the estimator: insufficient for reproducibility.
2. CHECK: If DL is used with k < 10: recommend HKSJ adjustment.
   HKSJ typically produces wider, more honest CIs.
3. COMPUTE: tau as a percentage of the pooled effect.
   If tau > 50% of the pooled estimate: heterogeneity is clinically important
   regardless of statistical significance.
```

**Q statistic:**

```
Q = sum(w_i * (theta_i - theta_pooled)²)
Under H0 (homogeneity): Q ~ chi²(k-1)

Verification:
1. CHECK: P-value for Q-test. But NOTE: Q-test has low power when k < 10.
   Non-significant Q does NOT confirm homogeneity — only that heterogeneity
   was not detected. Use a significance threshold of 0.10, not 0.05.
2. COMPUTE: Degrees of freedom must equal k - 1.
   If df is different from k - 1: check for errors in study count or subgroup structure.
```

</heterogeneity_assessment>

<publication_bias>

## Publication Bias Detection

**Funnel plot assessment:**

```
Standard funnel plot: effect size (x-axis) vs precision (y-axis, usually SE or 1/SE).
Under no bias: symmetric inverted funnel centered on pooled estimate.

Asymmetry indicates:
- Publication bias (small negative studies unpublished)
- Small-study effects (different mechanisms in small vs large studies)
- Methodological heterogeneity (smaller studies are lower quality)

Verification:
1. CHECK: Funnel plot should be presented if k >= 10 studies.
   With fewer than 10 studies: tests for asymmetry have insufficient power.
2. CHECK: Contour-enhanced funnel plots add significance contours (P = 0.01, 0.05, 0.10).
   If missing studies fall in non-significant regions: publication bias more likely.
   If asymmetry persists even in significant regions: may reflect heterogeneity, not bias.
3. CHECK: Axis orientation. SE on y-axis (inverted: larger studies at top) is standard.
   Some plots use 1/SE: verify interpretation matches orientation.
```

**Egger's test (regression-based):**

```
Egger's test: regress standardized effect (effect / SE) on precision (1 / SE).
Intercept significantly different from 0: asymmetry present.

Verification:
1. CHECK: Egger's test is only valid for ratio measures (OR, RR) with continuous SE.
   For risk differences or standardized mean differences: use alternative tests.
2. CHECK: Minimum k = 10 for adequate power. With k < 10: do not interpret.
3. CHECK: P-value threshold. Use P < 0.10 (not 0.05) due to low power.
4. NOTE: Egger's test detects small-study effects, which may have causes other
   than publication bias (true heterogeneity, methodological differences).
```

**Trim-and-fill method:**

```
Algorithm: Iteratively impute "missing" studies to make the funnel plot symmetric.
Provides an adjusted pooled estimate accounting for hypothesized missing studies.

Verification:
1. CHECK: Trim-and-fill is a SENSITIVITY analysis, not a definitive correction.
   Adjusted estimate should be compared with unadjusted — if they differ substantially,
   publication bias may be influencing the result.
2. CHECK: The number of imputed studies reported. If > 30% of observed studies: large bias.
3. CHECK: The direction of the adjusted estimate. If it crosses the null while the
   unadjusted does not: the overall conclusion is sensitive to publication bias.
4. LIMITATION: Trim-and-fill assumes a specific model of bias (symmetric suppression
   of negative results). Real-world bias patterns may be more complex.
```

**Additional methods:**

```
- P-curve analysis: Tests whether significant P-values have evidential value.
  Right-skewed P-curve: true effect likely exists.
  Flat or left-skewed: may indicate P-hacking or selective reporting.

- Selection models (Copas, Vevea-Hedges):
  Model the selection process explicitly. More sophisticated than trim-and-fill.
  Require specification of a selection function — results depend on model assumptions.

- PET-PEESE (precision-effect test / precision-effect estimate with standard error):
  Regression-based methods that estimate bias-corrected effects.
  PET when meta-analytic effect is near zero; PEESE when PET is significant.

Verification:
1. CHECK: At least two methods used for publication bias assessment.
   Single method is insufficient given the different assumptions and limitations.
2. CHECK: Concordance across methods. If all methods agree: more confident conclusion.
   If methods disagree: report the range and discuss reasons for discordance.
```

</publication_bias>

<sensitivity_analysis>

## Sensitivity Analysis

**Leave-one-out analysis:**

```
Method: Recalculate pooled estimate removing one study at a time.

Verification:
1. CHECK: If removing any single study changes the direction or significance
   of the pooled estimate: the result is NOT robust.
   Report which study, if any, is influential.
2. CHECK: If the study with the largest weight drives the result:
   evaluate that study's quality carefully — the meta-analysis is essentially
   depending on a single study.
3. PRESENT: Forest plot of leave-one-out estimates showing the range of results.
```

**Influence diagnostics:**

```
Key metrics:
- Cook's distance analog: measures each study's influence on the pooled estimate.
- DFBETAS: standardized change in the estimate when study removed.
- Hat values: leverage of each study (related to weight and extremity).
- Externally standardized residuals: identify outliers (|r| > 2 is concerning).

Baujat plot: x-axis = contribution to Q statistic, y-axis = influence on pooled estimate.
Studies in upper-right quadrant: both heterogeneous AND influential — investigate.

Verification:
1. CHECK: Outlying studies (residual > 2 or > 3) investigated for:
   - Different population
   - Different intervention dose/duration
   - Different outcome definition
   - Methodological quality issues
2. CHECK: If outliers are removed, is the decision justified a priori (not data-driven)?
   Post-hoc removal of inconvenient studies: flag as selective.
```

**Additional sensitivity analyses:**

```
Standard sensitivity analyses to check:
1. Risk of bias: Restrict to low-risk-of-bias studies only.
   If the result changes: driven by low-quality evidence.
2. Statistical model: Compare fixed vs random effects.
   If they differ meaningfully: heterogeneity is affecting the conclusion.
3. Effect measure: Compare OR vs RR (for binary outcomes).
   OR and RR diverge when events are common (baseline risk > 20%).
4. Correlation assumptions: For studies reporting multiple outcomes or time points,
   test sensitivity to assumed correlation coefficient (rho = 0.5, 0.3, 0.7).
5. Missing data: Best-case / worst-case analysis for studies with incomplete data.
   If worst-case changes the conclusion: result is fragile.
```

</sensitivity_analysis>

<subgroup_analysis>

## Subgroup Analysis Validity

**Requirements for credible subgroup analyses:**

```
The Sun et al. (2010) / Oxman & Guyatt criteria for credible subgroup effects:

1. Pre-specified: Was the subgroup hypothesis stated before data were analyzed?
   Post-hoc subgroups: hypothesis-generating only, not confirmatory.
2. Small number: Were only a few subgroup analyses performed?
   > 5 subgroups: high risk of false positives — apply correction.
3. Direction predicted: Was the direction of the subgroup effect pre-specified?
4. Consistent across studies: Is the subgroup effect consistent across related studies?
5. Indirect comparison: Subgroup comparisons are BETWEEN-study (indirect) unless
   within-study subgroup data are used.
6. Statistical interaction test: P-value for interaction, NOT separate P-values per subgroup.
   CRITICAL: A significant effect in one subgroup and non-significant in another does NOT
   demonstrate a subgroup difference. The interaction test is required.

Verification:
1. CHECK: Interaction P-value is reported (not just within-subgroup P-values).
2. CHECK: Number of subgroup analyses. With 10 subgroups at alpha = 0.05:
   probability of at least one false positive = 1 - 0.95^10 = 40%.
3. CHECK: Was a correction for multiple comparisons applied?
   Bonferroni: alpha / number of subgroups.
   If no correction and > 3 subgroups: interpret cautiously.
4. CHECK: Are subgroups based on study-level characteristics (ecological) or
   individual-level data (IPD)? Study-level subgroups are subject to ecological bias.
   Individual participant data (IPD) meta-analysis is the gold standard.
```

</subgroup_analysis>

<effect_measures>

## Effect Measure Appropriateness

**Selecting the correct effect measure:**

```
Binary outcomes:
- Risk Ratio (RR): Preferred for cohort studies, RCTs. Intuitive interpretation.
  RR is NOT constant across baseline risks (non-collapsibility for rare events handled).
- Odds Ratio (OR): Required for case-control studies.
  Preferred for logistic regression and when events are rare (OR ≈ RR when events < 10%).
  With common events (> 20%): OR overestimates RR — flag if conflated.
- Risk Difference (RD): Absolute measure. Clinical relevance depends on baseline risk.
  RD = absolute risk reduction. NNT = 1 / RD.
  RD often heterogeneous across studies with different baseline risks.
- Hazard Ratio (HR): Time-to-event data. Requires proportional hazards assumption.
  HR ≠ RR in most situations. Do not pool HRs with RRs.

Continuous outcomes:
- Mean Difference (MD): Same scale across all studies (e.g., all use mmHg for BP).
  Preferred when the outcome measure is identical.
- Standardized Mean Difference (SMD): Different scales measuring the same construct.
  Cohen's d (biased for small samples) vs Hedges' g (bias-corrected).
  SMD = (mean1 - mean2) / SD_pooled.
  Interpretation: 0.2 small, 0.5 medium, 0.8 large (Cohen's benchmarks).

Verification:
1. CHECK: Effect measure matches the study design and outcome type.
2. CHECK: All studies use the SAME effect measure in the meta-analysis.
   Mixing RR and OR: methodological error — flag.
3. CHECK: For MD, units are consistent (convert if necessary before pooling).
4. CHECK: For HR, proportional hazards assumption verified in individual studies.
   If PH violated: pooling HRs may be meaningless.
5. CHECK: For OR with common events: is OR being interpreted as if it were RR?
   If control group event rate > 20% and OR is described as "risk": flag.
```

</effect_measures>

<model_selection>

## Model Selection

**Fixed effects vs random effects:**

```
Fixed effects (common effect model):
- Assumes all studies estimate the SAME true underlying effect.
- Appropriate when: studies are functionally identical (same population,
  intervention, comparator, outcome, setting) — rare in practice.
- Weights: inverse variance (larger studies dominate).
- Inference: conditional — applies to the specific set of studies included.

Random effects (random effects model):
- Assumes true effects VARY across studies (each study estimates its own true effect).
- The pooled estimate is the MEAN of the distribution of true effects.
- Appropriate when: studies differ in populations, settings, or implementations.
- Weights: inverse variance + tau² (gives relatively more weight to smaller studies).
- Inference: unconditional — generalizes to a broader population of settings.

Verification:
1. CHECK: Model choice is justified based on the research question, not I² value.
   "We used random effects because I² was high": incorrect justification.
   Correct: "We used random effects because clinical and methodological diversity
   across studies means a common effect is implausible."
2. CHECK: With random effects, is the prediction interval reported?
   The pooled mean alone does not describe the distribution of effects.
3. CHECK: With few studies (k < 5), random effects tau² estimation is imprecise.
   Fixed effects may be more appropriate, but acknowledge limited generalizability.
4. CHECK: Hartung-Knapp-Sidik-Jonkman (HKSJ) modification applied for random effects?
   Standard DerSimonian-Laird random effects with k < 20: anti-conservative CIs.
   HKSJ correction is recommended as default by the Cochrane Handbook.
```

</model_selection>

<forest_plot>

## Forest Plot Accuracy

**Verification checklist:**

```
1. CHECK: Each study represented with:
   - Point estimate (square) with size proportional to weight.
   - 95% confidence interval (horizontal line).
   - Numeric values printed alongside.

2. CHECK: Pooled estimate (diamond) with:
   - Width representing the 95% CI.
   - Center aligned with the pooled point estimate.
   - Clearly labeled as fixed or random effects.

3. CHECK: Scale and axis:
   - Ratio measures (RR, OR, HR): plotted on LOG scale. Linear scale distorts symmetry.
     If log scale not used for ratio measures: flag as presentation error.
   - Difference measures (MD, RD, SMD): plotted on linear scale.
   - Null line at 1.0 (ratio) or 0 (difference).

4. CHECK: Study identification:
   - Author and year for each study.
   - Subgroup labels if stratified.
   - Total events and sample sizes per group.

5. CHECK: Consistency:
   - Pooled estimate within the range of individual studies.
     If the pooled estimate is outside the range of ALL individual estimates:
     possible calculation error — flag.
   - Study with the widest CI should have the smallest weight (smallest square).
   - Study weights should sum to 100% (within rounding).

6. CHECK: Computational accuracy:
   - Verify at least one study's CI matches the reported data:
     For RR: ln(RR) ± 1.96 * SE(ln(RR)); exponentiate for CI.
     SE(ln(RR)) = sqrt(1/a - 1/(a+b) + 1/c - 1/(c+d)) for a 2x2 table.
   - Verify the pooled estimate matches the weighted average of individual estimates.
```

</forest_plot>

## Worked Examples

### Heterogeneity assessment with prediction interval

```python
import numpy as np
from scipy.stats import t as t_dist

# Five RCTs reporting risk ratios (on log scale)
log_RR = np.array([-0.22, -0.35, -0.10, -0.51, -0.15])
SE = np.array([0.12, 0.15, 0.10, 0.20, 0.11])
k = len(log_RR)

# Fixed-effect pooled estimate (inverse-variance)
w = 1 / SE**2
theta_fixed = np.sum(w * log_RR) / np.sum(w)

# Cochran's Q
Q = np.sum(w * (log_RR - theta_fixed)**2)
df = k - 1
p_Q = 1 - __import__('scipy').stats.chi2.cdf(Q, df)

# DerSimonian-Laird tau²
C = np.sum(w) - np.sum(w**2) / np.sum(w)
tau2 = max(0, (Q - df) / C)
I2 = max(0, (Q - df) / Q * 100)

# Random-effects pooled estimate
w_re = 1 / (SE**2 + tau2)
theta_re = np.sum(w_re * log_RR) / np.sum(w_re)
SE_re = 1 / np.sqrt(np.sum(w_re))

# 95% prediction interval
t_crit = t_dist.ppf(0.975, k - 2)
PI_lower = theta_re - t_crit * np.sqrt(tau2 + SE_re**2)
PI_upper = theta_re + t_crit * np.sqrt(tau2 + SE_re**2)

print(f"Pooled log(RR) = {theta_re:.3f}, RR = {np.exp(theta_re):.2f}")
print(f"95% CI: [{np.exp(theta_re - 1.96*SE_re):.2f}, {np.exp(theta_re + 1.96*SE_re):.2f}]")
print(f"I² = {I2:.1f}%, Q = {Q:.2f}, P = {p_Q:.3f}")
print(f"tau² = {tau2:.4f}")
print(f"95% Prediction interval: [{np.exp(PI_lower):.2f}, {np.exp(PI_upper):.2f}]")
# If the prediction interval crosses 1.0: the effect may not be present in all settings.
```

### Detecting publication bias with Egger's test

```python
from scipy.stats import linregress

# Standardized effect vs precision
std_effect = log_RR / SE   # y: effect / SE
precision = 1 / SE          # x: 1 / SE

slope, intercept, r, p_value, se_slope = linregress(precision, std_effect)

print(f"Egger's test intercept: {intercept:.3f}, P = {p_value:.3f}")
# Intercept significantly different from 0 at P < 0.10: suggests asymmetry.
# Note: with only k = 5 studies, Egger's test has very low power.
# Report but do not over-interpret.
```
