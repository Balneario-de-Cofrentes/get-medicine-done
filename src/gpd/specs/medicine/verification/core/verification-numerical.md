---
load_when:
  - "statistical verification"
  - "meta-analysis validation"
  - "heterogeneity check"
  - "publication bias"
  - "power analysis"
  - "automated verification"
tier: 2
context_cost: large
---

# Verification Statistical — Meta-Analysis Statistics, Power, and Automated Validation

Statistical verification for meta-analyses, power analysis, publication bias detection, heterogeneity assessment, and automated verification framework. Everything needed to validate computational medical research results.

**Load when:** Working with meta-analyses, statistical calculations, systematic review data synthesis, or any quantitative medical evidence.

**Related files:**
- `references/verification/core/verification-quick-reference.md` — compact checklist (default entry point)
- `references/verification/core/verification-core.md` — PICO consistency, sensitivity analysis, risk of bias, GRADE
- `references/verification/core/computational-verification-templates.md` — R/Python code templates
- `references/verification/core/code-testing-medical.md` — testing patterns for medical research code

---

<meta_analysis_statistics>

## Meta-Analysis Statistical Verification

Meta-analysis results must be validated at multiple levels: correct data input, appropriate model, correct computation, and meaningful output.

**Principle:** A meta-analysis result is only trustworthy if the input data is verified, the statistical model is appropriate, and the output is checked against independent computation.

**Verification hierarchy:**

```
1. Does it run?              -> Code executes without errors
2. Does it use correct data? -> Effect sizes and SEs match source papers
3. Does it use correct model? -> FE vs RE appropriate, effect measure right
4. Does it compute correctly? -> Cross-check with independent software
5. Is it robust?             -> Sensitivity analyses confirm main finding
```

Levels 1-2 are necessary but NOT sufficient. Level 4 is the minimum for a trustworthy result.

**Standard statistical verification tests:**

### Effect size recalculation

```python
import numpy as np

def verify_effect_size_from_2x2(events_tx, n_tx, events_ctrl, n_ctrl,
                                 reported_or, reported_ci, tolerance=0.05):
    """
    Recalculate OR from 2x2 table and verify against reported value.

    Args:
        events_tx: Number of events in treatment group
        n_tx: Total in treatment group
        events_ctrl: Number of events in control group
        n_ctrl: Total in control group
        reported_or: OR reported in the paper
        reported_ci: (lower, upper) CI reported
        tolerance: Relative tolerance for match
    """
    a, b = events_tx, n_tx - events_tx
    c, d = events_ctrl, n_ctrl - events_ctrl
    if any(x == 0 for x in [a, b, c, d]):
        a, b, c, d = a + 0.5, b + 0.5, c + 0.5, d + 0.5

    computed_or = (a * d) / (b * c)
    log_or = np.log(computed_or)
    se_log_or = np.sqrt(1/a + 1/b + 1/c + 1/d)

    ci_lower = np.exp(log_or - 1.96 * se_log_or)
    ci_upper = np.exp(log_or + 1.96 * se_log_or)

    or_match = abs(computed_or - reported_or) / reported_or < tolerance
    ci_match = (abs(ci_lower - reported_ci[0]) / reported_ci[0] < tolerance and
                abs(ci_upper - reported_ci[1]) / reported_ci[1] < tolerance)

    status = "PASS" if or_match and ci_match else "FAIL"
    print(f"{status} OR verification:")
    print(f"  Computed: {computed_or:.3f} [{ci_lower:.3f}, {ci_upper:.3f}]")
    print(f"  Reported: {reported_or:.3f} [{reported_ci[0]:.3f}, {reported_ci[1]:.3f}]")
    return or_match and ci_match


def verify_smd_from_means(mean_tx, sd_tx, n_tx, mean_ctrl, sd_ctrl, n_ctrl,
                           reported_smd, tolerance=0.05):
    """
    Recalculate SMD (Hedges' g) from means and SDs.
    """
    sd_pooled = np.sqrt(
        ((n_tx - 1) * sd_tx**2 + (n_ctrl - 1) * sd_ctrl**2) /
        (n_tx + n_ctrl - 2)
    )
    d = (mean_tx - mean_ctrl) / sd_pooled
    df = n_tx + n_ctrl - 2
    j = 1 - 3 / (4 * df - 1)
    g = d * j

    se_g = np.sqrt(
        (n_tx + n_ctrl) / (n_tx * n_ctrl) + g**2 / (2 * (n_tx + n_ctrl))
    ) * j

    match = abs(g - reported_smd) / max(abs(reported_smd), 0.001) < tolerance
    status = "PASS" if match else "FAIL"
    print(f"{status} SMD verification:")
    print(f"  Computed Hedges' g: {g:.4f} (SE: {se_g:.4f})")
    print(f"  Reported SMD: {reported_smd:.4f}")
    return match
```

### Pooled estimate verification

```python
def verify_fixed_effect_meta(effect_sizes, variances, reported_pooled,
                              reported_ci, tolerance=0.05):
    """
    Verify fixed-effect (inverse-variance) meta-analysis.
    """
    weights = 1.0 / np.array(variances)
    pooled = np.sum(weights * np.array(effect_sizes)) / np.sum(weights)
    se_pooled = np.sqrt(1.0 / np.sum(weights))
    ci_lower = pooled - 1.96 * se_pooled
    ci_upper = pooled + 1.96 * se_pooled

    match = abs(pooled - reported_pooled) / max(abs(reported_pooled), 0.001) < tolerance
    status = "PASS" if match else "FAIL"
    print(f"{status} Fixed-effect pooled estimate:")
    print(f"  Computed: {pooled:.4f} [{ci_lower:.4f}, {ci_upper:.4f}]")
    print(f"  Reported: {reported_pooled:.4f} [{reported_ci[0]:.4f}, {reported_ci[1]:.4f}]")
    return match


def verify_random_effects_dl(effect_sizes, variances, reported_pooled,
                              reported_ci, tolerance=0.05):
    """
    Verify DerSimonian-Laird random-effects meta-analysis.
    """
    es = np.array(effect_sizes)
    v = np.array(variances)
    w = 1.0 / v
    k = len(es)

    mu_fe = np.sum(w * es) / np.sum(w)
    Q = np.sum(w * (es - mu_fe) ** 2)
    df = k - 1
    C = np.sum(w) - np.sum(w ** 2) / np.sum(w)
    tau_sq = max(0, (Q - df) / C)

    w_re = 1.0 / (v + tau_sq)
    pooled = np.sum(w_re * es) / np.sum(w_re)
    se_pooled = np.sqrt(1.0 / np.sum(w_re))
    ci_lower = pooled - 1.96 * se_pooled
    ci_upper = pooled + 1.96 * se_pooled

    match = abs(pooled - reported_pooled) / max(abs(reported_pooled), 0.001) < tolerance
    status = "PASS" if match else "FAIL"
    print(f"{status} DL random-effects pooled estimate:")
    print(f"  Computed: {pooled:.4f} [{ci_lower:.4f}, {ci_upper:.4f}]")
    print(f"  tau-squared: {tau_sq:.4f}")
    print(f"  Reported: {reported_pooled:.4f} [{reported_ci[0]:.4f}, {reported_ci[1]:.4f}]")
    return match
```

**When to apply:**

- Every meta-analysis (verify pooled estimate with independent calculation)
- Every data extraction (recalculate effect size from raw data)
- When results disagree with prior meta-analyses on the same topic
- Before any quantitative claim in a systematic review

</meta_analysis_statistics>

<heterogeneity_verification>

## Heterogeneity Verification

Heterogeneity statistics must be computed correctly and interpreted appropriately.

**Principle:** Heterogeneity assessment is more than reporting I-squared. A thorough assessment includes Q, I-squared, tau-squared, prediction intervals, and investigation of sources.

**Comprehensive heterogeneity check:**

```python
from scipy.stats import chi2, t as t_dist

def comprehensive_heterogeneity_check(effect_sizes, variances):
    """
    Full heterogeneity assessment with all relevant statistics.
    """
    es = np.array(effect_sizes)
    v = np.array(variances)
    k = len(es)
    w = 1.0 / v

    mu = np.sum(w * es) / np.sum(w)
    Q = np.sum(w * (es - mu) ** 2)
    df = k - 1
    p_Q = 1 - chi2.cdf(Q, df)

    I_sq = max(0, (Q - df) / Q * 100) if Q > df else 0
    H_sq = Q / df if df > 0 else 1.0

    C = np.sum(w) - np.sum(w ** 2) / np.sum(w)
    tau_sq_dl = max(0, (Q - df) / C)

    # Tau-squared (REML) - iterative
    tau_sq = tau_sq_dl
    for _ in range(100):
        w_re = 1.0 / (v + tau_sq)
        mu_re = np.sum(w_re * es) / np.sum(w_re)
        Q_re = np.sum(w_re * (es - mu_re) ** 2)
        denom = np.sum(w_re ** 2) - np.sum(w_re ** 2) * np.sum(w_re ** 2) / np.sum(w_re) ** 2
        if denom == 0:
            break
        tau_sq_new = max(0, tau_sq + (Q_re - df) / (np.sum(w_re) - np.sum(w_re ** 2) / np.sum(w_re)))
        if abs(tau_sq_new - tau_sq) < 1e-10:
            tau_sq = tau_sq_new
            break
        tau_sq = tau_sq_new

    # Prediction interval
    w_re = 1.0 / (v + tau_sq)
    mu_re = np.sum(w_re * es) / np.sum(w_re)
    se_re = np.sqrt(1.0 / np.sum(w_re))
    t_crit = t_dist.ppf(0.975, df=max(k - 2, 1))
    pi_lower = mu_re - t_crit * np.sqrt(tau_sq + se_re ** 2)
    pi_upper = mu_re + t_crit * np.sqrt(tau_sq + se_re ** 2)

    print(f"Heterogeneity Assessment (k = {k} studies)")
    print(f"{'='*50}")
    print(f"  Cochran's Q = {Q:.2f}, df = {df}, p = {p_Q:.4f}")
    print(f"  I-squared = {I_sq:.1f}%")
    print(f"  H-squared = {H_sq:.2f}")
    print(f"  tau-squared (DL) = {tau_sq_dl:.4f}")
    print(f"  tau-squared (REML) = {tau_sq:.4f}")
    print(f"  tau = {np.sqrt(tau_sq):.4f}")
    print(f"  Pooled estimate = {mu_re:.4f}")
    print(f"  95% CI: [{mu_re - 1.96*se_re:.4f}, {mu_re + 1.96*se_re:.4f}]")
    print(f"  95% PI: [{pi_lower:.4f}, {pi_upper:.4f}]")

    issues = []
    if I_sq > 75:
        issues.append("WARN: Considerable heterogeneity (I-squared > 75%). "
                       "Investigate sources before interpreting pooled estimate.")
    if k < 5:
        issues.append("WARN: Few studies (k < 5). Heterogeneity estimates are "
                       "imprecise. Consider HKSJ adjustment.")
    if pi_lower * pi_upper < 0:
        issues.append("WARN: Prediction interval crosses null. Effect in a "
                       "new study could go either way.")
    if p_Q < 0.10 and I_sq > 50:
        issues.append("NOTE: Significant Q test suggests true heterogeneity. "
                       "Explore via subgroup or meta-regression.")
    if I_sq == 0 and k < 10:
        issues.append("NOTE: I-squared = 0 does not prove homogeneity with "
                       "few studies. Q test has low power.")

    for issue in issues:
        print(f"  {issue}")

    return {
        "Q": Q, "df": df, "p_Q": p_Q,
        "I_squared": I_sq, "H_squared": H_sq,
        "tau_squared_dl": tau_sq_dl, "tau_squared_reml": tau_sq,
        "prediction_interval": (pi_lower, pi_upper),
        "issues": issues
    }
```

**When to apply:**

- Every meta-analysis (non-negotiable)
- Before deciding on fixed-effect vs random-effects
- Before interpreting the pooled estimate
- When planning subgroup or meta-regression analyses

</heterogeneity_verification>

<publication_bias_verification>

## Publication Bias Assessment

Publication bias threatens the validity of every meta-analysis. Assess systematically.

**Principle:** If studies with statistically significant results are more likely to be published, the meta-analytic estimate will be biased. Multiple methods should be used, as no single test is definitive.

**Publication bias assessment battery:**

```python
import numpy as np
from scipy.stats import norm, linregress

def eggers_test(effect_sizes, standard_errors):
    """
    Egger's regression test for funnel plot asymmetry.
    Tests whether smaller studies systematically show larger effects.
    H0: No small-study effect (intercept = 0).
    """
    precision = 1.0 / np.array(standard_errors)
    z_scores = np.array(effect_sizes) / np.array(standard_errors)

    slope, intercept, r, p_value, se_slope = linregress(precision, z_scores)

    status = "PASS" if p_value > 0.10 else "WARN"
    print(f"{status} Egger's test: intercept = {intercept:.3f}, p = {p_value:.4f}")
    if p_value < 0.10:
        print("  Small-study effect detected. Consider trim-and-fill or selection models.")
    return {"intercept": intercept, "p_value": p_value, "significant": p_value < 0.10}


def peters_test(events_tx, n_tx, events_ctrl, n_ctrl, log_ors, se_log_ors):
    """
    Peters' test -- alternative to Egger's for binary outcomes.
    Regresses 1/N on standardized effect. Less biased than Egger's for ORs.
    """
    n_total = np.array(n_tx) + np.array(n_ctrl)
    inv_n = 1.0 / n_total
    z_scores = np.array(log_ors) / np.array(se_log_ors)

    slope, intercept, r, p_value, se_slope = linregress(inv_n, z_scores)

    status = "PASS" if p_value > 0.10 else "WARN"
    print(f"{status} Peters' test: intercept = {intercept:.3f}, p = {p_value:.4f}")
    return {"intercept": intercept, "p_value": p_value}


def trim_and_fill(effect_sizes, variances, side="left"):
    """
    Duval & Tweedie trim-and-fill for estimating adjusted pooled effect.
    Identifies asymmetry in the funnel plot and imputes missing studies.
    """
    es = np.array(effect_sizes)
    v = np.array(variances)
    k = len(es)

    w = 1.0 / v
    mu = np.sum(w * es) / np.sum(w)

    n_missing = 0
    for i in range(k):
        mirror = 2 * mu - es[i]
        has_mirror = np.any(np.abs(es - mirror) < 0.1 * np.std(es))
        if not has_mirror and ((side == "left" and es[i] > mu) or
                                (side == "right" and es[i] < mu)):
            n_missing += 1

    imputed_es = list(es)
    imputed_v = list(v)
    for i in range(n_missing):
        if side == "left":
            idx = np.argmax(es - mu)
        else:
            idx = np.argmin(es - mu)
        mirror_es = 2 * mu - es[idx]
        imputed_es.append(mirror_es)
        imputed_v.append(v[idx])

    w_adj = 1.0 / np.array(imputed_v)
    mu_adj = np.sum(w_adj * np.array(imputed_es)) / np.sum(w_adj)

    print(f"Trim-and-fill: {n_missing} studies imputed")
    print(f"  Original pooled: {mu:.4f}")
    print(f"  Adjusted pooled: {mu_adj:.4f}")
    print(f"  Change: {abs(mu_adj - mu):.4f}")

    return {
        "n_imputed": n_missing,
        "original_pooled": mu,
        "adjusted_pooled": mu_adj,
    }
```

**Interpretation guidelines:**

| Test | Result | Interpretation |
|------|--------|---------------|
| Funnel plot | Symmetric | No visual evidence of publication bias |
| Funnel plot | Asymmetric (gap bottom-left for positive effects) | Small negative studies may be missing |
| Egger's test | p > 0.10 | No statistical evidence of small-study effects |
| Egger's test | p < 0.10 | Small-study effects present (not necessarily publication bias) |
| Trim-and-fill | 0 studies imputed | No adjustment needed |
| Trim-and-fill | Studies imputed, estimate changes substantially | Publication bias may inflate the effect |
| P-curve | Right-skewed | Evidential value present (true effect likely) |
| P-curve | Flat or left-skewed | P-hacking suspected or no true effect |

**When to apply:**

- Every meta-analysis with >= 10 studies (fewer = low power for all tests)
- When small studies show systematically larger effects
- When results seem "too good to be true"
- When industry-funded studies dominate

</publication_bias_verification>

<power_analysis_verification>

## Power and Precision Verification

Before concluding "no effect" or "confirmed effect", verify that the meta-analysis had adequate power to detect a clinically meaningful difference.

**Principle:** A non-significant result from an underpowered meta-analysis is not evidence of no effect. Optimal Information Size (OIS) provides the sample size threshold.

**Power analysis for meta-analysis:**

```python
from scipy.stats import norm

def optimal_information_size_binary(p_ctrl, rr_target, alpha=0.05, power=0.80):
    """
    Calculate Optimal Information Size for binary outcome.

    OIS = sample size a single well-powered trial would need.
    The meta-analysis total N should exceed OIS.

    Args:
        p_ctrl: Expected event rate in control group
        rr_target: Minimum clinically important RR to detect
        alpha: Significance level
        power: Desired power
    """
    p_tx = p_ctrl * rr_target
    z_alpha = norm.ppf(1 - alpha / 2)
    z_beta = norm.ppf(power)

    n_per_arm = ((z_alpha * np.sqrt(2 * p_ctrl * (1 - p_ctrl)) +
                   z_beta * np.sqrt(p_ctrl * (1 - p_ctrl) + p_tx * (1 - p_tx))) /
                  (p_ctrl - p_tx)) ** 2

    ois = int(np.ceil(2 * n_per_arm))
    print(f"OIS for binary outcome:")
    print(f"  Control event rate: {p_ctrl:.3f}")
    print(f"  Target RR: {rr_target:.2f} (treatment rate: {p_tx:.3f})")
    print(f"  OIS: {ois} total participants")
    return ois


def optimal_information_size_continuous(smd_target, alpha=0.05, power=0.80):
    """
    Calculate OIS for continuous outcome.

    Args:
        smd_target: Minimum clinically important SMD
    """
    z_alpha = norm.ppf(1 - alpha / 2)
    z_beta = norm.ppf(power)
    n_per_arm = int(np.ceil(((z_alpha + z_beta) / smd_target) ** 2))
    ois = 2 * n_per_arm

    print(f"OIS for continuous outcome:")
    print(f"  Target SMD: {smd_target:.2f}")
    print(f"  OIS: {ois} total participants")
    return ois


def verify_precision_adequacy(total_n, ois, ci_lower, ci_upper,
                               clinical_threshold):
    """
    Check whether meta-analysis has adequate precision.
    """
    issues = []

    if total_n < ois:
        issues.append(
            f"Total N ({total_n}) < OIS ({ois}). "
            "Meta-analysis may be underpowered. "
            "Downgrade GRADE for imprecision."
        )

    if ci_lower < clinical_threshold < ci_upper:
        issues.append(
            f"CI [{ci_lower:.3f}, {ci_upper:.3f}] crosses clinical "
            f"decision threshold ({clinical_threshold}). "
            "Downgrade GRADE for imprecision."
        )

    if total_n >= ois and not (ci_lower < clinical_threshold < ci_upper):
        print("PASS: Adequate precision (OIS met, CI does not cross threshold)")
    else:
        for issue in issues:
            print(f"WARN: {issue}")

    return {"adequate": len(issues) == 0, "issues": issues}
```

**When to apply:**

- Before concluding "no effect" (absence of evidence != evidence of absence)
- Before GRADE assessment of imprecision
- When planning a new trial to fill evidence gaps
- When the CI is wide relative to the clinical decision threshold

</power_analysis_verification>

<cross_check_literature>

## Cross-Check with Known Results

Compare your meta-analysis results against published systematic reviews, clinical guidelines, and established evidence.

**Principle:** Medical evidence exists in a web of interconnected knowledge. A new synthesis should be consistent with established evidence or the disagreement must be explained.

**Cross-check hierarchy:**

| Priority | Check Against | Confidence if Agrees |
|----------|---------------|---------------------|
| 1 | Cochrane review on same topic | Very high |
| 2 | Published meta-analysis in major journal | High |
| 3 | Clinical practice guideline recommendation | High |
| 4 | Individual large RCT (>1000 participants) | Medium-high |
| 5 | Observational study estimates | Medium |
| 6 | Biological plausibility / mechanism | Low (but violation is a red flag) |

**Standard references by evidence type:**

| Domain | Key References |
|--------|---------------|
| Drug efficacy | Cochrane Library, NICE guidelines, FDA labels |
| Diagnostic accuracy | Cochrane DTA reviews, STARD-reported studies |
| Surgical procedures | Cochrane, GRADE guidelines, specialty society guidelines |
| Public health | WHO guidelines, CDC recommendations, USPSTF |
| Drug safety | FDA Adverse Event Reporting System, EMA safety reports |
| Epidemiology | GBD study, WHO disease burden estimates |

**When to apply:**

- Before reporting any pooled estimate
- When a result will inform clinical guidelines
- When your result disagrees with an existing Cochrane review
- When using a new method on a previously reviewed question

</cross_check_literature>

## Meta-Analysis Verification Checklist

- [ ] Data extraction: all effect sizes recalculated from raw data (2x2 tables, means/SDs)
- [ ] Effect measure: appropriate for outcome type and consistent across studies
- [ ] Statistical model: FE vs RE justified, tau-squared estimator stated
- [ ] Heterogeneity: Q, I-squared, tau-squared, prediction interval all reported
- [ ] Sensitivity: leave-one-out, model comparison, RoB-restricted analyses done
- [ ] Publication bias: funnel plot + Egger's test (if k >= 10)
- [ ] Power: OIS calculated, precision adequate for conclusion
- [ ] Cross-check: compared with existing reviews / guidelines
- [ ] GRADE: certainty assessed with imprecision domain informed by OIS
- [ ] Forest plot: generated, correctly labeled, effect direction consistent
- [ ] Subgroups: interaction test used (not just comparing subgroup p-values)
- [ ] Code: analysis script available and reproducible

## Systematic Review Reporting Checklist

- [ ] PRISMA 2020: all 27 items addressed
- [ ] Flow diagram: numbers add up at each stage
- [ ] Search strategy: full strategy for at least one database reproduced
- [ ] Study characteristics table: PICO for each study
- [ ] Risk of bias table: all domains for all studies
- [ ] Summary of findings: GRADE for each outcome
- [ ] Protocol registration: PROSPERO number or equivalent
- [ ] Deviations: any protocol deviations documented

<automated_verification_script>

## Automated Verification Approach

For the verification subagent, use this pattern:

```python
class MedicalVerifier:
    """Automated verification of medical evidence synthesis."""

    def __init__(self, tolerance=0.05):
        self.tolerance = tolerance
        self.results = []

    def check_pico_consistency(self, studies):
        """Verify PICO elements are compatible across studies."""
        pass

    def check_effect_size(self, raw_data, reported_es, reported_se):
        """Recalculate effect size from raw data and compare."""
        pass

    def check_heterogeneity(self, effect_sizes, variances):
        """Compute and verify heterogeneity statistics."""
        es = np.array(effect_sizes)
        v = np.array(variances)
        w = 1.0 / v
        mu = np.sum(w * es) / np.sum(w)
        Q = np.sum(w * (es - mu) ** 2)
        k = len(es)
        df = k - 1
        I_sq = max(0, (Q - df) / Q * 100) if Q > df else 0

        self.results.append({
            "check": "Heterogeneity",
            "passed": I_sq < 75,
            "detail": f"I-squared = {I_sq:.1f}%, Q = {Q:.2f}, df = {df}"
        })
        return I_sq < 75

    def check_publication_bias(self, effect_sizes, standard_errors):
        """Run Egger's test for publication bias."""
        pass

    def check_sensitivity_robustness(self, main_estimate, sensitivity_estimates):
        """Verify main result is robust to sensitivity analyses."""
        pass

    def check_grade_completeness(self, grade_data):
        """Verify all GRADE domains are assessed."""
        required = ["risk_of_bias", "inconsistency", "indirectness",
                     "imprecision", "publication_bias"]
        missing = [d for d in required if d not in grade_data]
        passed = len(missing) == 0
        self.results.append({
            "check": "GRADE completeness",
            "passed": passed,
            "detail": f"Missing domains: {missing}" if missing else "All domains assessed"
        })
        return passed

    def check_prisma_compliance(self, prisma_items):
        """Verify PRISMA 2020 reporting completeness."""
        total_items = 27
        reported = sum(1 for v in prisma_items.values() if v)
        passed = reported >= total_items * 0.9
        self.results.append({
            "check": "PRISMA compliance",
            "passed": passed,
            "detail": f"{reported}/{total_items} items reported"
        })
        return passed

    def generate_report(self):
        """Generate VERIFICATION.md content."""
        lines = ["# Verification Report\n"]
        passed = sum(1 for r in self.results if r["passed"])
        total = len(self.results)
        lines.append(f"**Overall: {passed}/{total} checks passed**\n")

        for r in self.results:
            symbol = "PASS" if r["passed"] else "FAIL"
            lines.append(f"- {symbol} {r['check']}: {r['detail']}")

        return "\n".join(lines)
```

Run these checks against each evidence synthesis artifact. Aggregate results into VERIFICATION.md.

</automated_verification_script>

## Pre-Checkpoint Automation

For automation-first checkpoint patterns, computational environment management, and error recovery protocols, see:

**references/orchestration/checkpoints.md** -> `<automation_reference>` section

Key principles:

- GMD sets up verification environment BEFORE presenting checkpoints
- Users review results (forest plots, funnel plots, GRADE tables), not raw output
- Computational lifecycle: set up environment, run checks, present results
- Package installation: auto-install where safe, checkpoint for user choice otherwise
- Error handling: fix statistical issues before checkpoint, never present checkpoint with failed verification

## See Also

- `references/verification/core/verification-quick-reference.md` -- Compact checklist (default entry point)
- `references/verification/core/verification-core.md` -- PICO consistency, sensitivity analysis, risk of bias, GRADE
- `references/verification/core/computational-verification-templates.md` -- R/Python code templates
- `references/verification/core/code-testing-medical.md` -- Testing patterns for medical research code
