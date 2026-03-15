---
load_when:
  - "statistics verification"
  - "test assumptions"
  - "multiple comparisons"
  - "power analysis"
  - "effect size"
  - "missing data"
  - "survival analysis"
  - "Bayesian analysis"
tier: 2
context_cost: large
---

# Verification Domain — Statistics

Test assumption verification, multiple comparison correction, power analysis adequacy, effect size interpretation, missing data handling, survival analysis validity, and Bayesian analysis integrity for biomedical research.

**Load when:** Working on statistical methodology, hypothesis testing, power calculations, missing data assessment, survival analysis, Bayesian inference, or effect size interpretation in clinical/epidemiological research.

**Related files:**
- `../core/verification-quick-reference.md` — compact checklist (default entry point)
- `../core/verification-core.md` — foundational verification principles
- `references/verification/domains/verification-domain-meta-analysis.md` — meta-analysis (for pooled effect measures)
- `references/verification/domains/verification-domain-clinical-trial.md` — clinical trial (for trial-specific statistics)
- `references/verification/domains/verification-domain-epidemiology.md` — epidemiology (for observational study analysis)

---

<test_assumptions>

## Test Assumption Verification

**Normality:**

```
When required: t-tests, ANOVA, linear regression residuals, Pearson correlation.
Not required: Mann-Whitney U, Kruskal-Wallis, Spearman correlation, chi-squared,
logistic regression, Cox regression.

Assessment methods:
1. Visual: Q-Q plot (quantile-quantile). Points on or near the diagonal line: normal.
   Systematic curvature: non-normal.
2. Shapiro-Wilk test: preferred for small samples (n < 50). Significant P = non-normal.
3. Kolmogorov-Smirnov test: for larger samples, but less powerful than Shapiro-Wilk.
4. Skewness and kurtosis: |skewness| > 2 or |kurtosis| > 7: substantially non-normal.

Verification:
1. CHECK: For parametric tests on continuous outcomes, is normality assessed?
   If not assessed and sample is small (n < 30): flag.
2. CHECK: Central limit theorem (CLT) invoked for large samples.
   CLT applies to the SAMPLING DISTRIBUTION of the mean, not the data distribution.
   With n > 30: test statistics are approximately normal even if data are not.
   BUT: CLT does NOT fix issues with outliers, heavy tails, or extreme skew.
3. CHECK: If normality is violated and n < 30: non-parametric alternative should be used
   or data transformed (log, square root, Box-Cox).
4. CHECK: For ANOVA, normality of RESIDUALS is what matters, not of raw data.
   ANOVA is robust to mild non-normality with balanced designs and n > 20 per group.
```

**Homoscedasticity (equal variances):**

```
When required: Student's t-test (not Welch's), one-way ANOVA, linear regression.

Assessment methods:
1. Levene's test: Tests equality of variances. Significant P = unequal variances.
   Robust to non-normality when using median (Brown-Forsythe variant).
2. Bartlett's test: More powerful but assumes normality.
3. Residual plots: residuals vs fitted values. Fan shape = heteroscedasticity.
4. Ratio of largest to smallest group variance: > 4:1 is concerning.

Verification:
1. CHECK: If Student's t-test is used (not Welch's): equal variances verified?
   Welch's t-test does NOT require equal variances and should be the default.
   Using Student's t-test without checking variances: flag.
2. CHECK: For ANOVA with unequal group sizes AND unequal variances:
   Type I error is inflated. Use Welch's ANOVA or Games-Howell post-hoc.
3. CHECK: For linear regression with heteroscedastic residuals:
   Standard errors are incorrect. Use robust (sandwich/HC) standard errors
   or weighted least squares.
4. CHECK: Log-transformation of the outcome often stabilizes variance for
   right-skewed data (costs, biomarker levels).
```

**Independence:**

```
When required: All standard frequentist tests assume independent observations.

Violations:
1. Clustered data: patients within hospitals, repeated measures on same patient,
   siblings, matched pairs.
   If standard tests applied to clustered data: standard errors are TOO SMALL,
   P-values are TOO LOW, confidence intervals are TOO NARROW.
2. Temporal correlation: time-series data, pre-post designs without proper modeling.
3. Spatial correlation: geographic clustering of outcomes.

Verification:
1. CHECK: Study design. If cluster-randomized, multi-center, or repeated measures:
   clustering must be accounted for.
   Methods: mixed-effects models, GEE, cluster-robust standard errors, paired tests.
2. CHECK: Intracluster correlation coefficient (ICC) estimated for clustered data.
   If ICC is ignored: effective sample size is smaller than nominal.
   Design effect = 1 + (m - 1) × ICC, where m = average cluster size.
   Effective n = nominal n / design effect.
3. CHECK: For repeated measures: correlation structure specified
   (exchangeable, autoregressive, unstructured).
```

</test_assumptions>

<multiple_comparisons>

## Multiple Comparison Correction

**Bonferroni correction:**

```
Adjusted alpha = alpha / k, where k = number of comparisons.
Conservative: controls family-wise error rate (FWER).

Verification:
1. CHECK: Appropriate when controlling FWER is important and tests are independent or
   positively correlated.
2. CHECK: With many tests (k > 20): Bonferroni becomes extremely conservative.
   May miss true effects (inflated Type II error).
3. CHECK: Holm-Bonferroni (step-down procedure) is uniformly more powerful and should
   be preferred over standard Bonferroni. If standard Bonferroni is used when
   Holm would suffice: flag as unnecessarily conservative.
```

**False discovery rate (FDR) control:**

```
Benjamini-Hochberg (BH) procedure: Controls the expected proportion of false positives
among rejected hypotheses.

Algorithm:
1. Rank P-values: P_(1) ≤ P_(2) ≤ ... ≤ P_(k).
2. Find the largest i such that P_(i) ≤ (i/k) × alpha.
3. Reject all hypotheses with P_(j) ≤ P_(i).

FDR vs FWER:
- FDR is less conservative: more discoveries but allows some false positives.
- Appropriate for exploratory analyses, genomics, imaging, multiple secondary outcomes.
- FWER is appropriate when each individual test conclusion matters
  (e.g., multiple primary endpoints in a confirmatory trial).

Verification:
1. CHECK: Is the correction method stated?
   No correction with > 3 comparisons: flag.
2. CHECK: Is the method appropriate for the context?
   Confirmatory trial with co-primary endpoints: use FWER control (Bonferroni, Holm,
   gatekeeping, closed testing procedure).
   Exploratory subgroup analyses: FDR may be appropriate.
3. CHECK: Are the reported "adjusted P-values" correctly computed?
   For BH: adjusted P_i = min(P_(i) × k/i, 1.0), applied sequentially.
```

**Hierarchical (gatekeeping) testing:**

```
Pre-specified ordering of hypotheses. Test in order; stop when a hypothesis is not rejected.

Common in clinical trials:
1. Test primary endpoint at full alpha.
2. If primary is significant: test key secondary at full alpha.
3. Continue down the hierarchy.

Alpha is "recycled" from rejected hypotheses — no multiplicity penalty if the hierarchy holds.

Verification:
1. CHECK: Hierarchy was pre-specified in the statistical analysis plan (SAP).
   Post-hoc ordering: not a valid gatekeeping procedure.
2. CHECK: If the primary endpoint is not significant, NO secondary endpoints
   should be interpreted as confirmatory.
3. CHECK: Graphical approaches (Bretz et al.) allow more flexible alpha allocation.
   Verify the graphical procedure is fully specified (nodes, edges, weights).
```

</multiple_comparisons>

<power_analysis>

## Power Analysis Adequacy

**A priori power analysis (before the study):**

```
Required inputs:
1. Effect size: minimum clinically important difference or smallest effect of interest.
2. Variability: SD for continuous, event rate for binary, based on prior data.
3. Alpha level: typically 0.05 (two-sided).
4. Power: typically 0.80 or 0.90.
5. Statistical test to be used.

Verification:
1. CHECK: Effect size is clinically justified, not just statistically convenient.
   "We powered the study to detect a 1-point difference on a 100-point scale":
   is 1 point clinically meaningful? If not: overpowered for a trivial effect.
2. CHECK: Variability estimate is sourced from existing data (pilot, literature).
   If from a very different population: may not transfer.
3. CHECK: Sample size accounts for:
   - Expected dropout rate
   - Clustering (if cluster-randomized): inflate by design effect
   - Multiple primary endpoints: adjust alpha
   - Non-compliance: inflate for dilution of treatment effect
4. CHECK: Software and method documented for reproducibility.
```

**Post-hoc power analysis (INVALID):**

```
CRITICAL: Post-hoc (observed/retrospective) power is a DIRECT MONOTONIC
TRANSFORMATION of the P-value. It provides ZERO additional information.

If P = 0.05 → observed power ≈ 50% (always, for any test).
If P = 0.50 → observed power ≈ 8%.
If P = 0.001 → observed power ≈ 99%.

This is a mathematical tautology, not an informative analysis.

Verification:
1. FLAG: Any post-hoc power calculation. Common phrasing:
   "The study may have been underpowered as post-hoc power was only 30%."
   This is meaningless — it just restates that P was not significant.
2. CORRECT ALTERNATIVE: Report confidence intervals and interpret relative to MCID.
   If the 95% CI excludes the MCID: the study rules out a clinically meaningful effect
   regardless of the P-value.
3. CORRECT ALTERNATIVE: Report the detectable effect size at 80% power given the
   actual sample size achieved. This IS informative and avoids the tautology.
```

</power_analysis>

<effect_size>

## Effect Size Interpretation

**Cohen's d and Hedges' g:**

```
d = (mean1 - mean2) / SD_pooled

Cohen's benchmarks: 0.2 = small, 0.5 = medium, 0.8 = large.
These benchmarks are CONTEXT-DEPENDENT. A d = 0.3 may be huge in one field and trivial
in another. Always interpret effect sizes in clinical context, not just by benchmarks.

Hedges' g: bias-corrected version of d. Important when sample sizes are small (n < 20).
g = d × (1 - 3 / (4(n1 + n2) - 9))

Verification:
1. CHECK: Is the pooled SD used (Cohen's d) or the control group SD (Glass's delta)?
   These differ when group SDs are unequal.
2. CHECK: For pre-post designs, Cohen's d using between-subject SD is different from
   Cohen's d using within-subject SD. Specify which.
3. CHECK: Hedges' g used for small samples? If n < 20 and Cohen's d is reported:
   bias may be 5-10%.
```

**Number needed to treat/harm (NNT/NNH):**

```
NNT = 1 / absolute risk reduction (ARR)
ARR = control event rate (CER) - experimental event rate (EER)

NNT interpretation: Number of patients who must be treated for one additional patient
to benefit (or be harmed).

Verification:
1. CHECK: NNT is calculated from ABSOLUTE (not relative) risk reduction.
   RRR = 50% sounds impressive, but if CER = 2%: ARR = 1%, NNT = 100.
   If CER = 40%: ARR = 20%, NNT = 5. Same RRR, very different clinical impact.
2. CHECK: NNT is presented with confidence interval.
   CI for NNT can include infinity (when the CI for ARR crosses zero) and negative values.
   Proper CI includes NNT and NNH ranges.
3. CHECK: NNT is time-specific. NNT over 5 years ≠ NNT over 1 year.
   If not specified: flag.
4. CHECK: NNT should be calculated from the study's actual event rates,
   not from pooled meta-analytic relative effects applied to a single baseline risk
   (unless baseline risk is specified).
```

**Absolute risk and relative risk measures:**

```
Absolute measures: RD (risk difference), ARR, NNT.
Relative measures: RR (relative risk), OR (odds ratio), HR (hazard ratio).

Verification:
1. CHECK: BOTH absolute and relative measures reported.
   Reporting only OR = 0.70 without baseline risk: reader cannot assess clinical significance.
   "30% relative reduction" without absolute numbers: misleading if baseline risk is low.
2. CHECK: Clinical significance vs statistical significance.
   A statistically significant RR = 0.98 with very large n: unlikely to be clinically meaningful.
   A non-significant RR = 0.60 with small n: may be clinically important — underpowered.
3. CHECK: For rare events, OR ≈ RR. For common events (> 20%), OR overestimates RR.
   If OR is interpreted as RR for a common outcome: flag.
```

</effect_size>

<missing_data>

## Missing Data Handling

**Missing data mechanisms:**

```
1. MCAR (Missing Completely At Random):
   Probability of missingness does not depend on observed or unobserved data.
   Example: lab sample randomly lost in transport.
   Test: Little's MCAR test. Compare observed means by missingness pattern.
   If MCAR: complete case analysis is unbiased (but less efficient).

2. MAR (Missing At Random):
   Probability of missingness depends on OBSERVED data but not on unobserved values.
   Example: younger patients more likely to miss follow-up visits,
   and age is recorded for everyone.
   If MAR: multiple imputation and mixed models are valid.
   Complete case analysis may be biased.

3. MNAR (Missing Not At Random):
   Probability of missingness depends on the UNOBSERVED value itself.
   Example: patients with worse outcomes drop out because they feel too sick.
   If MNAR: no standard method fully corrects the bias.
   Sensitivity analyses (pattern-mixture models, selection models, tipping-point
   analysis) are required.

Verification:
1. CHECK: Missing data mechanism is discussed (MCAR/MAR/MNAR).
   If not discussed: flag — the choice of handling method cannot be justified.
2. CHECK: Amount of missing data reported per variable and per analysis.
   > 5% missing: potential for bias.
   > 20% missing: results are highly sensitive to missing data assumptions.
   > 40% missing: results should be interpreted with extreme caution.
3. CHECK: Patterns of missingness. Is it monotone (dropout) or intermittent?
   Monotone: common in longitudinal studies (once a patient drops out, all future data missing).
   Intermittent: missed visits but patient returns — different handling methods.
```

**Imputation methods:**

```
1. Single imputation methods (generally INADEQUATE for primary analysis):
   - Mean imputation: WRONG — reduces variability, biases correlations.
   - Last observation carried forward (LOCF): WRONG — biased under most conditions.
     NRC (2010) report explicitly recommends against LOCF as primary.
   - Baseline observation carried forward (BOCF): Conservative but biased.
   - Worst-case imputation: Sensitivity analysis only, not primary.

2. Multiple imputation (MI):
   Gold standard for MAR data.
   Procedure: create m imputed datasets (m ≥ 20 recommended), analyze each,
   combine results using Rubin's rules.
   Verification:
   - CHECK: Number of imputations (m). m < 5: insufficient for reasonable SE estimates.
     m should be at least equal to the percentage of incomplete cases.
   - CHECK: Imputation model includes all variables used in the analysis model
     PLUS auxiliary variables that predict missingness or the outcome.
     Omitting variables from the imputation model: biased imputation.
   - CHECK: Imputation model is compatible with the analysis model.
     Example: if analysis uses interaction terms, imputation should include them.
   - CHECK: Convergence of MCMC (if using FCS/MICE): trace plots should show no trend.

3. Maximum likelihood methods:
   Mixed-effects models (LMM, GLMM) handle MAR data without explicit imputation.
   Use all available data from each participant.
   Verification: CHECK that the model assumptions (random effects distribution,
   correlation structure) are appropriate.

4. Inverse probability weighting (IPW):
   Weight complete cases by inverse probability of being observed.
   Requires correct model for the probability of missingness.
   Verification: CHECK for extreme weights, positivity violations.
```

</missing_data>

<survival_analysis>

## Survival Analysis Validity

**Proportional hazards assumption (Cox model):**

```
The Cox proportional hazards model assumes that the hazard ratio is CONSTANT over time.
h(t|X) = h_0(t) × exp(beta × X)

Assessment methods:
1. Schoenfeld residuals: Plot scaled Schoenfeld residuals vs time.
   Non-zero slope: PH violation. Test: global and per-covariate P-values.
2. Log-log plot: Plot log(-log(S(t))) vs log(t) for each group.
   Parallel curves: PH satisfied. Crossing curves: PH violated.
3. Time-varying coefficient: Include interaction of covariate with time.
   If significant: hazard ratio changes over time.

Verification:
1. CHECK: PH assumption tested for each covariate.
   Especially for the primary exposure variable.
2. CHECK: If PH is violated:
   - Stratified Cox model (for covariates violating PH but not of primary interest).
   - Time-varying coefficients (for the primary exposure).
   - Restricted mean survival time (RMST): avoids PH assumption entirely.
   - Accelerated failure time (AFT) models: parametric alternative.
3. CHECK: PH violation is common in oncology (treatment effect wanes or emerges late).
   Immunotherapy trials with delayed separation of Kaplan-Meier curves:
   HR from Cox model is misleading — use RMST or milestone survival.
```

**Censoring validity:**

```
Types of censoring:
1. Right censoring: Event has not occurred by end of follow-up.
   Most common. Standard survival methods assume non-informative censoring.
2. Left censoring: Event occurred before observation period.
   Requires interval-censoring methods.
3. Interval censoring: Event occurred between two observation times.
   Common in screening studies (cancer detected at screen visit).

Non-informative censoring assumption:
Censoring is independent of the event — censored patients have the same future risk
as those still under observation.

Verification:
1. CHECK: Is censoring non-informative?
   Informative censoring: patients censor BECAUSE of their prognosis
   (e.g., switch to a different treatment due to disease progression).
   If informative: standard KM and Cox estimates are BIASED.
2. CHECK: Censoring rates are reported per group.
   Differential censoring between groups: potential informative censoring.
3. CHECK: Administrative censoring (end of study) is non-informative by definition.
   Loss to follow-up is potentially informative — investigate reasons.
4. CHECK: Competing risks. If death from other causes prevents observation of the event:
   Kaplan-Meier overestimates cumulative incidence.
   Use cumulative incidence function (CIF) with Fine-Gray model for competing risks.
```

**Kaplan-Meier verification:**

```
1. CHECK: At-risk table shown below the KM plot (number at risk at each time point).
   Missing at-risk table: flag — cannot assess reliability of tail estimates.
2. CHECK: Tail of KM curve. If < 10% of patients remain at risk:
   estimates are very imprecise. Use RMST up to a clinically meaningful time point.
3. CHECK: Median survival with 95% CI reported.
   If median not reached: report survival at a milestone time point (e.g., 1-year, 5-year).
4. CHECK: KM curves should start at S(0) = 1.0 and be non-increasing.
   Any increase in KM curve: ERROR in computation.
```

</survival_analysis>

<bayesian_analysis>

## Bayesian Analysis Integrity

**Prior sensitivity analysis:**

```
Prior types:
1. Non-informative (vague): N(0, 10000), Uniform(-inf, inf).
   Minimal influence on posterior. Results should approximate frequentist.
   Verification: If using "non-informative" priors, compare with frequentist estimate.
   Large discrepancy: prior is more informative than claimed.

2. Weakly informative: Regularizing priors that prevent extreme values.
   N(0, 1) or N(0, 2.5) for standardized regression coefficients (Gelman recommendation).
   Verification: Prior should not dominate data. Prior-to-posterior ratio should show learning.

3. Informative: Based on previous data or expert opinion.
   Must be explicitly justified and sourced.
   Verification: Is the prior source documented? Is it from the same population/context?
   Using a prior from a different clinical setting without justification: flag.

4. Skeptical prior: Centered on null (no effect) with moderate precision.
   Used for sensitivity analysis to test robustness of findings to prior skepticism.

Prior sensitivity analysis:
1. Run analysis with at least 3 different priors (non-informative, weakly informative,
   and one informative or skeptical).
2. If conclusions change substantially across priors: data are not sufficiently informative.
   The result depends heavily on prior assumptions — flag.
3. Report prior-data conflict diagnostics. If the prior is in the tail of the
   likelihood: prior and data disagree — investigate.

Verification:
1. CHECK: Prior distributions are explicitly stated for all parameters.
2. CHECK: Sensitivity to prior is assessed (minimum: one alternative prior).
3. CHECK: For clinical trial analysis with informative priors:
   regulatory agencies (FDA) generally require additional frequentist analysis.
```

**Convergence diagnostics:**

```
MCMC (Markov Chain Monte Carlo) convergence must be verified before interpreting results.

Diagnostic tools:
1. Trace plots: Chains should look like "hairy caterpillars" — no trends, no steps,
   no periodic patterns. Multiple chains should overlap.
2. Gelman-Rubin statistic (R-hat): Compares between-chain and within-chain variance.
   R-hat < 1.01: converged (strict criterion, recommended).
   R-hat < 1.1: acceptable (common but less strict).
   R-hat > 1.1: NOT converged — increase iterations or reparameterize.
3. Effective sample size (ESS): Accounts for autocorrelation within chains.
   ESS > 400 for reliable posterior mean and CI estimation.
   ESS > 4000 for reliable tail probability estimation.
   Bulk ESS and tail ESS should both be reported.
4. Autocorrelation plots: Autocorrelation should drop to near zero within 10-20 lags.
   High autocorrelation: chain is moving slowly — increase thinning or reparameterize.
5. Divergent transitions (HMC/NUTS): ANY divergent transitions indicate the
   posterior geometry is difficult. Results may be biased.
   Fix: increase adapt_delta, reparameterize, or use a different model.

Verification:
1. CHECK: Number of chains (minimum 4 recommended).
2. CHECK: Warm-up (burn-in) period discarded.
3. CHECK: R-hat reported for all parameters. Any R-hat > 1.01: flag.
4. CHECK: ESS reported. Bulk ESS or tail ESS < 400: flag.
5. CHECK: Divergent transitions reported. If > 0: results may be unreliable.
```

**Posterior interpretation:**

```
1. Credible intervals (CrI) vs confidence intervals (CI):
   95% CrI: 95% probability that the parameter lies in this interval (given data and prior).
   95% CI: 95% of such intervals contain the true parameter (frequentist interpretation).
   DO NOT interpret CrI as CI or vice versa. Language matters.

2. Posterior probability of meaningful effect:
   P(theta > MCID | data) = proportion of posterior above the MCID threshold.
   This directly answers the clinical question: "What is the probability the treatment
   benefit exceeds the minimum clinically important difference?"

3. Bayes factor (BF):
   BF > 3: moderate evidence for H1. BF > 10: strong. BF > 100: decisive.
   BF < 1/3: moderate evidence for H0.
   1/3 < BF < 3: inconclusive.
   Verification: BF is sensitive to prior specification. Always report the prior used.

4. Region of Practical Equivalence (ROPE):
   Define a region around null (e.g., -0.1 to 0.1 for standardized effect).
   If 95% HDI (highest density interval) falls entirely within ROPE: practically equivalent.
   If 95% HDI falls entirely outside ROPE: practically different.
   If overlap: inconclusive.

Verification:
1. CHECK: Posterior summaries include median or mean, 95% CrI, and ESS.
2. CHECK: Language is appropriate (posterior probability, not P-value).
3. CHECK: For decision-making: loss function or utility analysis specified.
```

</bayesian_analysis>

## Worked Examples

### Multiple comparison correction

```python
import numpy as np

# 10 secondary outcomes tested, uncorrected P-values
p_values = np.array([0.003, 0.012, 0.028, 0.035, 0.041, 0.055, 0.078, 0.120, 0.340, 0.890])
alpha = 0.05
k = len(p_values)

# Bonferroni
bonferroni_threshold = alpha / k
sig_bonferroni = p_values < bonferroni_threshold
print(f"Bonferroni threshold: {bonferroni_threshold:.4f}")
print(f"Bonferroni significant: {np.where(sig_bonferroni)[0] + 1}")
# Only outcome 1 (P = 0.003) is significant at 0.005

# Holm-Bonferroni (step-down)
sorted_idx = np.argsort(p_values)
sorted_p = p_values[sorted_idx]
holm_thresholds = alpha / (k - np.arange(k))
sig_holm = np.zeros(k, dtype=bool)
for i in range(k):
    if sorted_p[i] <= holm_thresholds[i]:
        sig_holm[sorted_idx[i]] = True
    else:
        break
print(f"Holm significant: {np.where(sig_holm)[0] + 1}")
# Outcomes 1, 2 are significant (more powerful than Bonferroni)

# Benjamini-Hochberg (FDR)
sorted_p = np.sort(p_values)
bh_thresholds = (np.arange(1, k + 1) / k) * alpha
max_i = 0
for i in range(k):
    if sorted_p[i] <= bh_thresholds[i]:
        max_i = i + 1
sig_bh = p_values <= sorted_p[max_i - 1] if max_i > 0 else np.zeros(k, dtype=bool)
print(f"BH (FDR) significant: {np.where(sig_bh)[0] + 1}")
# Outcomes 1, 2, 3, 4, 5 are significant at FDR = 0.05
```

### Missing data: comparing approaches

```python
# Simulated trial: 200 patients, 25% dropout in treatment arm, 15% in control
# Dropout correlated with poor outcomes (MNAR-like)

# Complete case analysis: biased — sicker patients dropped out
# Effect appears LARGER because poor-outcome patients are excluded from treatment arm.

# LOCF: biased — carries forward last observation, ignores trajectory.
# If patients drop out while deteriorating: LOCF freezes them at a better value.

# Multiple imputation (MI): Valid under MAR.
# But if dropout is truly MNAR (related to unobserved outcomes): MI is also biased.

# Tipping-point analysis:
# After MI under MAR, systematically shift imputed values for dropouts:
# "What if all dropouts in the treatment arm had outcomes delta worse than imputed?"
# Find delta at which the treatment effect becomes non-significant.
# If delta is small (e.g., < 0.5 SD): result is sensitive to missing data assumptions.
# If delta is large (e.g., > 2 SD): result is robust.

# Example output:
# MI (MAR): Treatment effect = 3.2 points, 95% CI [1.8, 4.6], P = 0.001
# Tipping point at delta = 1.5 SD: effect = 1.0, CI [-0.4, 2.4], P = 0.08
# Interpretation: if dropouts in treatment arm had outcomes 1.5 SD worse than
# predicted under MAR, the treatment effect becomes non-significant.
# Clinical judgment: is 1.5 SD shift plausible? If yes: result is fragile.
```

### Survival analysis: testing proportional hazards

```python
# After fitting Cox model: cox_model.fit(X, duration, event)
# Schoenfeld residuals test:

# Method 1: Statistical test
# from lifelines import CoxPHFitter
# cph = CoxPHFitter()
# cph.fit(df, duration_col='time', event_col='event')
# cph.check_assumptions(df, p_value_threshold=0.05, show_plots=True)

# If PH is violated for the treatment variable:
# Option A: Restricted Mean Survival Time (RMST)
# RMST = integral of S(t) from 0 to tau (a pre-specified time horizon)
# Difference in RMST between groups: model-free summary of treatment effect
# Does NOT require PH assumption.

# Option B: Time-varying coefficients
# Include treatment × log(time) interaction in Cox model.
# Allows HR to change over time.
# HR(t) = exp(beta + gamma * log(t))
# If gamma > 0: HR increases over time (effect diminishes)
# If gamma < 0: HR decreases over time (effect strengthens)

# Option C: Milestone analysis
# Compare survival at pre-specified time points (e.g., 1-year, 2-year survival)
# Use Kaplan-Meier with Greenwood confidence intervals
# No model assumptions needed
```
