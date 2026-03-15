---
load_when:
  - "meta-analysis"
  - "forest plot"
  - "funnel plot"
  - "heterogeneity"
  - "I-squared"
  - "tau-squared"
  - "random effects"
  - "fixed effect"
  - "metafor"
  - "meta package"
  - "pooled estimate"
  - "DerSimonian-Laird"
  - "REML"
  - "prediction interval"
  - "subgroup analysis"
  - "meta-regression"
  - "publication bias"
  - "Egger"
  - "trim and fill"
tier: 1
context_cost: high
---

# Statistical Analysis Protocol for Meta-Analysis

This protocol covers the statistical methods used in pairwise meta-analysis: model selection, effect size calculation, heterogeneity assessment, subgroup analysis, meta-regression, sensitivity analysis, and publication bias assessment. Implementation focuses on R packages `metafor` and `meta`.

**Core principle:** The statistical model must match the clinical and methodological context. A random-effects model is not always correct; a fixed-effect model is not always wrong. The choice must be justified, not defaulted.

## Related Protocols

- `data-extraction.md` -- Data entering the meta-analysis
- `grade-certainty.md` -- Imprecision and inconsistency feed into GRADE
- `prisma-systematic-review.md` -- PRISMA Items 13a-13e (synthesis methods)
- `network-meta-analysis.md` -- Extension to indirect comparisons
- `narrative-synthesis.md` -- When meta-analysis is not appropriate

---

## Choosing the Statistical Model

### Fixed-Effect (Common-Effect) Model

**Assumption:** All studies estimate the same underlying true effect. Differences between studies are due only to sampling error.

**When appropriate:**

- Studies are clinically and methodologically homogeneous.
- The goal is to estimate the common effect in the specific populations studied.
- Few studies (2-3) make random-effects estimation unreliable.

### Random-Effects Model

**Assumption:** Each study estimates a different (but related) true effect. The true effects are drawn from a distribution with mean mu and variance tau-squared.

**When appropriate:**

- Studies differ in populations, settings, intervention details, or other factors that could modify the true effect.
- The goal is to estimate the mean effect across the distribution of true effects.
- The review intends the results to be generalizable beyond the specific studies.

### Practical Guidance

| Scenario | Recommended Model |
|----------|------------------|
| Identical protocol, multi-site trial | Fixed-effect |
| Different trials, same intervention/population | Random-effects |
| Very few studies (< 5) | Fixed-effect (random-effects tau-squared unreliable) or HKSJ |
| I-squared = 0% | Either; fixed-effect and random-effects give identical results |
| I-squared > 0% | Random-effects, but interpret with prediction interval |

**The prediction interval** is more informative than the pooled estimate for clinical decisions. It shows the range of true effects that could be expected in a new setting, accounting for between-study heterogeneity.

---

## Effect Size Measures

### Binary Outcomes

| Measure | Formula | Properties | When to Use |
|---------|---------|------------|-------------|
| Risk Ratio (RR) | (a/n1) / (c/n2) | Multiplicative scale, intuitive | Default for most reviews |
| Odds Ratio (OR) | (ad) / (bc) | Multiplicative scale, symmetric | Case-control studies; logistic regression |
| Risk Difference (RD) | (a/n1) - (c/n2) | Absolute scale, additive | When absolute effects matter; NNT = 1/RD |

**Zero-cell corrections:**

When a study has zero events in one or both arms:

| Method | Implementation | When to Use |
|--------|---------------|-------------|
| Continuity correction | Add 0.5 to all cells | Default in many software packages; biased for unbalanced groups |
| Treatment arm continuity correction | Add proportional to group sizes | Better for unbalanced groups |
| Peto OR method | No correction needed; uses hypergeometric distribution | Balanced groups, low event rates, no substantial treatment effect |
| Exact methods | Conditional or unconditional exact tests | Very sparse data |
| Exclude studies with zero events in both arms | Remove double-zero studies | Common but loses information |
| Beta-binomial model | Full Bayesian model | Most principled approach for sparse data |

### Continuous Outcomes

| Measure | Formula | Properties | When to Use |
|---------|---------|------------|-------------|
| Mean Difference (MD) | mean1 - mean2 | Same scale as measurement | All studies use same measurement |
| Standardized Mean Difference (SMD) | (mean1 - mean2) / SD_pooled | Unitless | Studies use different scales measuring same construct |
| Ratio of Means | mean1 / mean2 | Multiplicative | When baseline values differ substantially |

**SMD conventions (Cohen's d):**

| Value | Interpretation |
|-------|---------------|
| 0.2 | Small effect |
| 0.5 | Medium effect |
| 0.8 | Large effect |

**Hedges' g** (bias-corrected SMD) is preferred over Cohen's d, especially for small samples. The correction factor J = 1 - 3/(4df - 1).

### Time-to-Event Outcomes

Use the hazard ratio (HR) on the log scale. Extract log(HR) and its SE from each study.

Methods for estimating HR when not directly reported: see Tierney et al. (2007).

---

## Heterogeneity Assessment

### Statistics

| Statistic | Interpretation | Limitation |
|-----------|---------------|------------|
| **Cochran's Q** | Test of homogeneity (H0: all effects equal) | Low power with few studies; overpowered with many |
| **I-squared** | Proportion of total variability due to heterogeneity | Depends on study size; same tau-squared gives different I-squared in small vs. large studies |
| **Tau-squared** | Between-study variance | On the squared scale of the effect measure; hard to interpret directly |
| **Tau** | Between-study standard deviation | On the scale of the effect measure; more interpretable |
| **H-squared** | Ratio of Q to its df | H-squared = 1 implies no heterogeneity |
| **Prediction interval** | Range of plausible true effects in a new study | Most clinically relevant; may include null even when pooled estimate does not |

### Interpretation Guidelines

```
I-squared rough guide (use with caution):
  0-40%:    might not be important
  30-60%:   may represent moderate heterogeneity
  50-90%:   may represent substantial heterogeneity
  75-100%:  considerable heterogeneity
```

**I-squared alone is NOT sufficient.** Always also consider:
- Direction of effects (all positive vs. mixed directions)
- Overlap of confidence intervals
- Clinical significance of the range of effects
- The prediction interval

---

## Estimation Methods for Random-Effects Models

| Method | Description | Pros | Cons |
|--------|-------------|------|------|
| **DerSimonian-Laird (DL)** | Moments-based estimator | Simple, widely used | Underestimates tau-squared; CI too narrow |
| **REML** | Restricted maximum likelihood | Less biased than DL | Requires iterative computation |
| **Paule-Mandel (PM)** | Iterative generalized Q-statistic | Good performance with few studies | Less familiar |
| **HKSJ** | Hartung-Knapp-Sidik-Jonkman | Better CI coverage, uses t-distribution | Can give wider CIs; requires REML or PM for tau-squared |

**Recommendation:** Use REML for tau-squared estimation combined with the HKSJ adjustment for the confidence interval of the pooled estimate. This is the current best practice for standard meta-analyses.

---

## Implementation in R

### Using `metafor`

```r
library(metafor)

# Binary outcome (log risk ratio)
dat <- escalc(measure = "RR", ai = events_i, bi = noevent_i,
              ci = events_c, di = noevent_c, data = mydata)

# Random-effects model with REML
res <- rma(yi, vi, data = dat, method = "REML")

# With HKSJ adjustment
res_hksj <- rma(yi, vi, data = dat, method = "REML", test = "knha")

# Forest plot
forest(res, slab = dat$study, header = TRUE,
       xlab = "Risk Ratio", atransf = exp,
       order = "obs", xlim = c(-3, 4))

# Funnel plot
funnel(res, xlab = "Risk Ratio", atransf = exp)

# Prediction interval
predict(res, transf = exp, digits = 3)

# Continuous outcome (mean difference)
dat_cont <- escalc(measure = "MD",
                   m1i = mean_i, sd1i = sd_i, n1i = n_i,
                   m2i = mean_c, sd2i = sd_c, n2i = n_c,
                   data = mydata_cont)

res_cont <- rma(yi, vi, data = dat_cont, method = "REML", test = "knha")
```

### Using `meta`

```r
library(meta)

# Binary outcome
m_bin <- metabin(event.e = events_i, n.e = n_i,
                 event.c = events_c, n.c = n_c,
                 studlab = study, data = mydata,
                 sm = "RR", method.tau = "REML",
                 hakn = TRUE, prediction = TRUE)

# Continuous outcome
m_cont <- metacont(n.e = n_i, mean.e = mean_i, sd.e = sd_i,
                   n.c = n_c, mean.c = mean_c, sd.c = sd_c,
                   studlab = study, data = mydata_cont,
                   sm = "MD", method.tau = "REML",
                   hakn = TRUE, prediction = TRUE)

# Forest plot
forest(m_bin, sortvar = TE, prediction = TRUE,
       label.left = "Favours intervention",
       label.right = "Favours control")

# Funnel plot
funnel(m_bin)
```

---

## Subgroup Analysis

### When to Conduct

- Subgroups must be pre-specified in the protocol.
- Minimum 2 studies per subgroup.
- Biological or clinical rationale for the subgroup hypothesis.

### Statistical Approach

```r
# Subgroup analysis in metafor
res_sub <- rma(yi, vi, data = dat, mods = ~ subgroup, method = "REML")

# Test for subgroup differences
# The test for the moderator (QM) tests whether the subgroup explains heterogeneity

# In meta package
m_sub <- update(m_bin, subgroup = subgroup_var, print.subgroup.name = TRUE)
# Reports test for subgroup differences automatically
```

**Report the test for interaction** (between-subgroup difference), NOT within-subgroup p-values. A significant effect within one subgroup and a non-significant effect within another does NOT demonstrate a subgroup difference.

### Credibility of Subgroup Findings (ICEMAN criteria)

1. Was the subgroup hypothesis pre-specified?
2. Is the subgroup variable a baseline characteristic (not post-randomisation)?
3. Is there a plausible biological mechanism?
4. Is the test for interaction statistically significant?
5. Is the subgroup finding consistent across related outcomes?
6. Is the subgroup finding consistent across studies?
7. Was the analysis one of a small number of pre-specified subgroup hypotheses?

---

## Meta-Regression

### When to Use

- At least 10 studies per covariate (rule of thumb).
- To explore sources of heterogeneity when subgroup analysis is too coarse.
- Pre-specified covariates only.

```r
# Meta-regression in metafor
res_reg <- rma(yi, vi, mods = ~ mean_age + year + dose,
               data = dat, method = "REML", test = "knha")

# Proportion of heterogeneity explained
# R-squared analog: compare tau-squared from model with and without moderators
```

**Ecological fallacy warning:** Meta-regression uses study-level (aggregate) data, not individual patient data. A relationship between study-level mean age and effect size does NOT necessarily mean that age modifies the effect at the individual level.

---

## Sensitivity Analyses

### Standard Sensitivity Analyses

| Analysis | Purpose | Method |
|----------|---------|--------|
| Excluding high risk of bias studies | Test robustness to study quality | Re-run meta-analysis without high RoB studies |
| Leave-one-out | Identify influential studies | Sequentially remove each study |
| Different effect measure | Test robustness to measure choice | Compare RR vs. OR analysis |
| Different model | Test robustness to model assumptions | Compare fixed-effect vs. random-effects |
| Different tau-squared estimator | Test robustness to estimation method | Compare DL vs. REML |
| Excluding imputed data | Test robustness to missing data handling | Re-run without studies where data were imputed |
| Different inclusion decisions | Test robustness to borderline decisions | Include/exclude borderline studies |

```r
# Leave-one-out analysis in metafor
leave1out(res)

# Influence diagnostics
inf <- influence(res)
plot(inf)
```

---

## Publication Bias Assessment

### Visual Assessment

**Funnel plot:** Plot effect size (x-axis) against precision (y-axis, usually SE). In the absence of publication bias, the plot should be symmetric around the pooled estimate.

### Statistical Tests

| Test | Outcome Type | What It Tests | Minimum Studies |
|------|-------------|---------------|-----------------|
| **Egger's test** | Continuous | Asymmetry via regression of effect on SE | 10 |
| **Peters' test** | Binary | Regression of effect on 1/total sample size | 10 |
| **Begg's test** | Any | Rank correlation between effect and variance | 10 |
| **Harbord's test** | Binary (OR) | Modified Egger's for odds ratios | 10 |

```r
# Egger's test in metafor
regtest(res, model = "lm")

# Peters' test (for binary outcomes)
regtest(res, model = "lm", predictor = "ni")

# Trim and fill (sensitivity analysis, not a correction)
tf <- trimfill(res)
funnel(tf)
```

### Advanced Methods

| Method | Description | Use When |
|--------|-------------|----------|
| **Trim and fill** | Imputes missing studies to symmetrize funnel plot | Sensitivity analysis; do not use as primary correction |
| **Selection models** | Models the probability of publication as a function of significance | Plausible publication selection mechanism |
| **PET-PEESE** | Precision-Effect Test / Precision-Effect Estimate with Standard Error | When publication bias is suspected |
| **p-curve** | Distribution of significant p-values | Detecting p-hacking / evidential value |
| **Copas selection model** | Maximum likelihood approach modeling selection | When selection probability can be estimated |

---

## Reporting Results

### Forest Plot Requirements

- Study labels (author, year)
- Point estimate per study with 95% CI
- Weight per study (graphically and numerically)
- Pooled estimate with 95% CI (diamond)
- Prediction interval (if random-effects)
- Heterogeneity statistics (I-squared, tau-squared, Q p-value)
- Test of overall effect (z or t statistic, p-value)
- Axis labels and favors labels ("Favours intervention" / "Favours control")

### Numerical Reporting

```
Pooled RR = 0.78 (95% CI 0.65 to 0.93; p = 0.007)
I-squared = 42% (95% CI 0% to 72%)
Tau-squared = 0.04 (SE = 0.03)
Prediction interval: 0.52 to 1.17
Number of studies: 12
Total participants: 3,456
```

---

## Verification Checklist

Before finalizing the meta-analysis:

- [ ] Effect measure appropriate for the outcome type (RR for binary, MD/SMD for continuous, HR for time-to-event)
- [ ] Statistical model justified (fixed vs. random) with rationale
- [ ] Estimation method specified (REML recommended; HKSJ adjustment applied)
- [ ] Heterogeneity assessed using multiple metrics (I-squared, tau-squared, prediction interval)
- [ ] Forest plot includes all required elements
- [ ] Subgroup analyses pre-specified with interaction tests reported
- [ ] Meta-regression has adequate studies per covariate (>=10)
- [ ] Sensitivity analyses conducted (leave-one-out, excluding high RoB, varying model)
- [ ] Publication bias assessed with funnel plot and statistical test (if >= 10 studies)
- [ ] Prediction interval reported alongside pooled estimate
- [ ] Zero-cell handling documented if applicable
- [ ] All code reproducible and documented
