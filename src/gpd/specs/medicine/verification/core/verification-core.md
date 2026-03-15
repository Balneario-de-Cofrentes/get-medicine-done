---
load_when:
  - "verification"
  - "PICO consistency"
  - "sensitivity analysis"
  - "risk of bias"
  - "GRADE certainty"
  - "clinical plausibility"
  - "statistical validity"
tier: 1
context_cost: large
---

# Verification Core — Universal Medical Research Checks

PICO consistency, sensitivity analysis, risk of bias assessment, GRADE certainty evaluation, clinical plausibility, and statistical validity. These checks catch the majority of errors in systematic reviews, meta-analyses, and clinical evidence synthesis and apply to every medical research subfield.

**Load when:** Always — these are the non-negotiable checks for any medical research analysis.

**Related files:**
- `references/verification/core/verification-quick-reference.md` — compact checklist (default entry point)
- `references/verification/core/verification-numerical.md` — statistical verification, convergence, numerical validation
- `references/verification/core/verification-patterns.md` — verification pattern index
- `references/verification/core/computational-verification-templates.md` — R/Python code templates for verification
- `references/verification/core/code-testing-medical.md` — testing patterns for medical research code

---

<core_principle>
**Existence != Correctness**

An analysis existing does not mean the evidence synthesis is right. Verification must check:

1. **Exists** — Result is present (analysis complete, data extracted, code runs, forest plot generated)
2. **Methodologically consistent** — Study design appropriate, PICO elements consistent across included studies, risk of bias assessed correctly, statistical model appropriate
3. **Clinically plausible** — Effect sizes reasonable, direction makes biological sense, consistent with prior evidence, NNT/NNH meaningful
4. **Cross-validated** — Agrees with sensitivity analyses, subgroup analyses, independent methods (different effect measures, different models), and existing literature/guidelines

Levels 1-3 can often be checked programmatically. Level 4 requires deeper analysis and sometimes human judgment.

**Level 5: External Oracle** — Result verified by an independent computational system (R metafor, Python statsmodels, or Cochrane RevMan) whose output is shown in VERIFICATION.md. This is the strongest form of verification because it breaks the LLM self-consistency loop: the LLM cannot hallucinate a correct statistical software output.

Every VERIFICATION.md MUST include at least one Level 5 check — an executed code block with actual output. See `references/verification/core/computational-verification-templates.md` for copy-paste-ready templates.
</core_principle>

> **Key companion document:** See `../errors/llm-medical-errors.md` for the catalog of LLM-specific medical research error classes with detection strategies and traceability matrix.

<pico_consistency>

## PICO Consistency Checking

The most fundamental evidence synthesis check. If PICO elements are inconsistent across included studies, the synthesis is meaningless.

**Principle:** Every study included in a systematic review or meta-analysis must share compatible Population, Intervention, Comparison, and Outcome definitions. Heterogeneity in PICO elements that is not explicitly acknowledged and handled invalidates the pooled estimate.

**Automated checks:**

```python
def check_pico_consistency(studies: list[dict]) -> dict:
    """Verify PICO elements are compatible across included studies.

    Args:
        studies: List of study dicts with keys:
            population, intervention, comparator, outcome,
            outcome_measure, timepoint, setting
    """
    issues = []

    # Check outcome measure consistency
    measures = set(s["outcome_measure"] for s in studies)
    if len(measures) > 1:
        issues.append(
            f"Mixed outcome measures: {measures}. "
            "Ensure appropriate effect measure (SMD for different scales, "
            "RR/OR for binary with consistent direction)."
        )

    # Check intervention consistency
    interventions = set(s["intervention"] for s in studies)
    if len(interventions) > 1:
        issues.append(
            f"Multiple interventions: {interventions}. "
            "Consider subgroup analysis or network meta-analysis."
        )

    # Check comparator consistency
    comparators = set(s["comparator"] for s in studies)
    if len(comparators) > 1:
        issues.append(
            f"Multiple comparators: {comparators}. "
            "Active vs placebo comparisons should not be pooled naively."
        )

    # Check population overlap
    populations = set(s["population"] for s in studies)
    if len(populations) > 1:
        issues.append(
            f"Heterogeneous populations: {populations}. "
            "Verify clinical homogeneity or plan subgroup analysis."
        )

    # Check timepoint consistency
    timepoints = [s["timepoint"] for s in studies]
    if max(timepoints) / max(min(timepoints), 1) > 3:
        issues.append(
            f"Timepoints range from {min(timepoints)} to {max(timepoints)}. "
            "Large variation may introduce clinical heterogeneity."
        )

    return {"consistent": len(issues) == 0, "issues": issues}
```

**Manual verification protocol:**

1. Extract the PICO elements for every included study into a structured table
2. Verify the Population is sufficiently similar (age, disease severity, comorbidities)
3. Verify the Intervention is the same drug/dose/route/duration or that variation is pre-specified for subgroup analysis
4. Verify the Comparator is consistent (placebo vs active control should not be mixed without justification)
5. Verify the Outcome definition, measurement tool, and timepoint are compatible
6. Verify the outcome direction is consistent (higher = better vs lower = better)

**Concrete examples by study type:**

### Systematic review PICO checking

```
Antihypertensive meta-analysis:
  - P: Adults with essential hypertension (Stage 1-2)
    Check: Excluding secondary hypertension? Consistent BP thresholds?
    Common error: Mixing resistant hypertension with newly diagnosed
  - I: ACE inhibitors (any)
    Check: Same drug class? Same dose range? Monotherapy vs combination?
    Common error: Pooling low-dose with high-dose without subgroup analysis
  - C: Placebo or active comparator
    Check: Never pool placebo-controlled with active-comparator trials
    Common error: Mixing placebo and ARB comparators inflates ACEi effect
  - O: Systolic BP reduction at 12 weeks (mmHg)
    Check: Same measurement method? Office vs ambulatory?
    Common error: Mixing office BP with 24h ambulatory BP means

SSRI for depression meta-analysis:
  - P: Adults with MDD (DSM or ICD criteria)
    Check: Treatment-resistant vs first-episode? Severity at baseline?
    Common error: Including dysthymia or adjustment disorder
  - I: SSRI (any, therapeutic dose)
    Check: What counts as therapeutic dose? Duration adequate?
    Common error: Including sub-therapeutic doses or < 4 week trials
  - C: Placebo
    Check: Active placebo vs inert? Run-in period?
    Common error: Ignoring placebo run-in that enriches responders
  - O: Response (>=50% reduction in HAM-D or MADRS)
    Check: Which scale? What threshold? Timepoint?
    Common error: Mixing HAM-D response with remission (different thresholds)
```

### Meta-analysis PICO checking

```
Surgical intervention meta-analysis:
  - P: Adults with knee osteoarthritis (KL grade 2-4)
    Check: Consistent grading system? Same joint?
    Common error: Mixing hip and knee OA studies
  - I: Arthroscopic surgery
    Check: Same procedure type? Debridement vs lavage vs meniscectomy?
    Common error: Pooling fundamentally different surgical procedures
  - C: Sham surgery or physiotherapy
    Check: True sham (incision + no procedure) vs physio (active)?
    Common error: Pooling sham-controlled with physio-controlled trials
  - O: Pain (VAS 0-100) at 12 months
    Check: Same pain scale? Same timepoint?
    Common error: Mixing VAS (0-100mm) with NRS (0-10) without SMD
```

**Common PICO pitfalls:**

| Pitfall | Example | Detection |
|---------|---------|-----------|
| Population mixing | Adults + children pooled | Check age criteria in each study |
| Dose heterogeneity | 5mg + 20mg + 80mg pooled as "drug X" | Tabulate doses, consider dose-response subgroup |
| Comparator mixing | Placebo + active control pooled | Separate analysis by comparator type |
| Outcome scale mixing | HAM-D + MADRS pooled as MD | Use SMD or convert via established linking |
| Timepoint mixing | 4-week + 52-week pooled | Restrict to similar timepoints |
| Setting mixing | Inpatient + outpatient pooled | Consider as source of heterogeneity |
| Direction inconsistency | Some scales: high=better, others: low=better | Harmonize direction before pooling |
| Definition creep | "Response" defined differently across studies | Tabulate exact definitions |

**When to apply:**

- Every systematic review (non-negotiable: before any data extraction)
- Every meta-analysis (before pooling any estimates)
- Every network meta-analysis (transitivity assumption depends on PICO)
- When updating a review with new studies

</pico_consistency>

<sensitivity_analysis>

## Sensitivity Analysis Verification

A correct overall result must be robust to methodological decisions. If it is not, the fragility must be reported.

**Principle:** Perform pre-specified sensitivity analyses to test whether the main finding changes when methodological decisions are varied. Fragile results that reverse or lose significance under reasonable alternative assumptions require cautious interpretation.

**Standard sensitivity analyses for evidence synthesis:**

### Leave-one-out analysis

```
Remove each study in turn and re-estimate the pooled effect.
- If removing one study changes the conclusion: that study is influential.
- Report the study, investigate why (largest sample? outlier effect? high RoB?).
- Decision: Is the influential study driving the result legitimately or due to bias?

Expected behavior: Point estimate shifts modestly, CI widens slightly.
Red flag: Conclusion reverses or effect size changes by >30% when removing one study.
```

### Model comparison

```
Compare results under different statistical models:
- Fixed-effect (Mantel-Haenszel) vs Random-effects (DerSimonian-Laird)
- Random-effects with different estimators (REML, PM, DL, HKSJ)
- Bayesian vs frequentist
- Different effect measures (OR vs RR vs RD for binary; MD vs SMD for continuous)

Expected behavior: Direction and approximate magnitude agree across models.
Red flag: Fixed-effect significant but random-effects not (suggests heterogeneity
is driving the fixed-effect result via larger studies).
```

### Risk of bias sensitivity

```
Restrict analysis to low risk-of-bias studies only:
- Run main analysis on all studies
- Run restricted analysis on low-RoB studies only
- Compare results

Expected behavior: Similar direction and magnitude, possibly wider CI.
Red flag: Effect disappears or reverses when high-RoB studies are excluded
(suggests bias is inflating the effect).
```

### Publication bias sensitivity

```
Assess the impact of potential missing studies:
- Funnel plot visual inspection
- Egger's test for small-study effects
- Trim-and-fill adjusted estimate
- Selection model approaches (Copas, Vevea-Hedges)
- P-curve or Z-curve for evidential value

Expected behavior: Symmetrical funnel, non-significant Egger's test.
Red flag: Asymmetrical funnel, significant Egger's test (p < 0.10),
trim-and-fill substantially reduces the effect.
```

### Outcome definition sensitivity

```
Vary the outcome definition:
- Per-protocol vs intention-to-treat analysis
- Different timepoints (e.g., 6 months vs 12 months)
- Different outcome thresholds (e.g., 50% vs 30% pain reduction)
- Continuous vs dichotomized outcome

Expected behavior: Consistent direction across definitions.
Red flag: ITT shows benefit but per-protocol does not (or vice versa).
```

**Verification protocol:**

```python
import numpy as np

def verify_sensitivity_robustness(main_estimate, main_ci,
                                   sensitivity_estimates, sensitivity_cis,
                                   labels, significance_threshold=0.05):
    """
    Check that sensitivity analyses support the main finding.

    Args:
        main_estimate: Point estimate from primary analysis
        main_ci: (lower, upper) confidence interval
        sensitivity_estimates: List of point estimates from sensitivity analyses
        sensitivity_cis: List of (lower, upper) CIs
        labels: List of sensitivity analysis names
    """
    main_significant = (main_ci[0] > 0) or (main_ci[1] < 0)  # CI excludes null
    main_direction = "benefit" if main_estimate < 0 else "harm"  # Assuming lower=better

    results = []
    for est, ci, label in zip(sensitivity_estimates, sensitivity_cis, labels):
        sens_significant = (ci[0] > 0) or (ci[1] < 0)
        sens_direction = "benefit" if est < 0 else "harm"
        direction_consistent = (sens_direction == main_direction)
        significance_consistent = (sens_significant == main_significant)

        change_pct = abs(est - main_estimate) / max(abs(main_estimate), 1e-10) * 100

        status = "PASS"
        if not direction_consistent:
            status = "FAIL"
        elif change_pct > 30:
            status = "WARN"
        elif not significance_consistent:
            status = "WARN"

        results.append({
            "analysis": label,
            "estimate": est,
            "ci": ci,
            "direction_consistent": direction_consistent,
            "significance_consistent": significance_consistent,
            "change_pct": change_pct,
            "status": status,
        })

    return results
```

**When to apply:**

- After every meta-analysis (non-negotiable)
- After every primary analysis in a systematic review
- When heterogeneity is moderate or high (I-squared > 50%)
- When the number of included studies is small (< 10)
- When risk of bias varies substantially across studies

</sensitivity_analysis>

<risk_of_bias>

## Risk of Bias Assessment Verification

Individual study quality directly affects the reliability of the synthesis. Risk of bias must be assessed systematically and its impact on the pooled estimate evaluated.

**Principle:** Every included study must be assessed for risk of bias using a validated, domain-appropriate tool. The assessment must be transparent, reproducible, and its impact on the synthesis quantified.

**Key risk of bias tools:**

### RoB 2 (Randomized trials)

```
Cochrane Risk of Bias 2 tool — five domains:
1. Randomization process
   - Was the allocation sequence random?
   - Was allocation concealed?
   - Were there baseline imbalances suggesting problems?

2. Deviations from intended interventions
   - Were participants aware of assignment?
   - Were there deviations due to the trial context?
   - Was an appropriate analysis used (ITT)?

3. Missing outcome data
   - Were outcome data available for all or nearly all participants?
   - Could missingness depend on the true value?
   - Was missingness handled appropriately?

4. Outcome measurement
   - Was the method appropriate?
   - Could measurement differ between groups?
   - Were assessors blinded?

5. Selection of reported result
   - Was the reported result pre-specified?
   - Were multiple eligible analyses performed?
   - Were multiple eligible outcomes measured?

Each domain: Low / Some concerns / High risk
Overall: Lowest domain = overall (worst-case approach)
```

### ROBINS-I (Non-randomized studies)

```
Risk Of Bias In Non-randomized Studies of Interventions — seven domains:
1. Confounding
2. Selection of participants
3. Classification of interventions
4. Deviations from intended interventions
5. Missing data
6. Measurement of outcomes
7. Selection of reported result

Each domain: Low / Moderate / Serious / Critical / No information
Overall: Determined by worst domain
```

### QUADAS-2 (Diagnostic accuracy studies)

```
Quality Assessment of Diagnostic Accuracy Studies — four domains:
1. Patient selection (consecutive/random? Case-control avoided? Inappropriate exclusions?)
2. Index test (pre-specified threshold? Blinded to reference?)
3. Reference standard (correct classification? Blinded to index?)
4. Flow and timing (appropriate interval? All patients receive both tests? All included in analysis?)
```

**Automated checks:**

```python
def verify_risk_of_bias_assessment(rob_data: list[dict],
                                     tool: str = "RoB2") -> dict:
    """Verify risk of bias assessment completeness and consistency.

    Args:
        rob_data: List of study RoB assessments, each with domain scores
        tool: "RoB2", "ROBINS-I", or "QUADAS-2"
    """
    issues = []

    # Define expected domains per tool
    domains = {
        "RoB2": ["randomization", "deviations", "missing_data",
                  "measurement", "reporting"],
        "ROBINS-I": ["confounding", "selection", "classification",
                      "deviations", "missing_data", "measurement", "reporting"],
        "QUADAS-2": ["patient_selection", "index_test",
                      "reference_standard", "flow_timing"],
    }

    expected_domains = domains.get(tool, [])

    for study in rob_data:
        study_id = study.get("study_id", "unknown")

        # Check all domains assessed
        for domain in expected_domains:
            if domain not in study:
                issues.append(
                    f"{study_id}: Missing assessment for domain '{domain}'"
                )

        # Check overall judgment consistency
        domain_scores = [study.get(d) for d in expected_domains if d in study]
        if tool == "RoB2":
            valid_scores = {"low", "some_concerns", "high"}
            for d in expected_domains:
                score = study.get(d)
                if score and score not in valid_scores:
                    issues.append(
                        f"{study_id}: Invalid score '{score}' for {d}"
                    )

            # Overall should be at least as bad as worst domain
            worst = "low"
            for score in domain_scores:
                if score == "high":
                    worst = "high"
                elif score == "some_concerns" and worst == "low":
                    worst = "some_concerns"
            if study.get("overall") and study["overall"] != worst:
                issues.append(
                    f"{study_id}: Overall '{study['overall']}' inconsistent "
                    f"with worst domain '{worst}'"
                )

        # Check for supporting justification
        if not study.get("justification"):
            issues.append(f"{study_id}: No justification text provided")

    return {"complete": len(issues) == 0, "issues": issues}
```

**Verification protocol:**

1. Confirm the correct RoB tool was used for the study design (RoB 2 for RCTs, ROBINS-I for non-randomized, QUADAS-2 for diagnostic)
2. Verify all domains are assessed for every study (no missing domains)
3. Verify overall judgment is consistent with domain-level judgments
4. Verify supporting justification is provided for each domain judgment
5. Verify inter-rater agreement was assessed (at least 2 reviewers, kappa reported)
6. Verify the impact of RoB on the synthesis was explored (sensitivity analysis excluding high-RoB studies)

**When to apply:**

- Every systematic review (non-negotiable)
- After constructing evidence tables
- Before interpreting pooled estimates
- When results are driven by studies with high or unclear risk of bias

</risk_of_bias>

<grade_certainty>

## GRADE Certainty Evaluation

The overall certainty of evidence determines how much confidence we can place in the effect estimate. GRADE provides a structured, transparent framework.

**Principle:** For each outcome, rate the certainty of evidence as High, Moderate, Low, or Very Low by starting at the default level (High for RCTs, Low for observational) and downgrading or upgrading based on specific criteria.

**GRADE domains for downgrading (RCTs start at High):**

```
1. Risk of bias (study limitations)
   - Downgrade if: Majority of evidence is at high or unclear RoB
   - Do not downgrade if: Low RoB studies dominate the pooled estimate
   - Key: Consider the weight of high-RoB studies in the meta-analysis

2. Inconsistency (heterogeneity)
   - Downgrade if: I-squared > 50-75% with unexplained heterogeneity
   - Downgrade if: Point estimates vary widely in direction or magnitude
   - Do not downgrade if: All studies show same direction, I-squared < 40%
   - Key: Prediction interval crossing the null is more informative than I-squared alone

3. Indirectness
   - Downgrade if: Population, intervention, comparator, or outcome
     differs from the target question
   - Downgrade if: Only surrogate outcomes available (BP instead of stroke)
   - Do not downgrade if: Studies directly address the review question
   - Key: Surrogate outcomes warrant downgrade even if well-validated

4. Imprecision
   - Downgrade if: Confidence interval crosses the clinical decision threshold
   - Downgrade if: Optimal Information Size (OIS) not met
   - Downgrade if: Few events (< 300 for binary outcomes as rule of thumb)
   - Key: OIS = sample size a single adequately powered trial would need

5. Publication bias
   - Downgrade if: Funnel plot asymmetry + significant Egger's test
   - Downgrade if: Industry-funded studies overrepresented
   - Downgrade if: Small number of small studies (< 10 studies)
   - Do not downgrade if: Comprehensive search + funnel symmetry
   - Key: Cannot assess with < 10 studies; note limitation instead
```

**GRADE domains for upgrading (observational start at Low):**

```
1. Large magnitude of effect
   - Upgrade if: RR > 2 or RR < 0.5 (one level)
   - Upgrade if: RR > 5 or RR < 0.2 (two levels)
   - Key: Only if no plausible confounders could explain the magnitude

2. Dose-response gradient
   - Upgrade if: Clear dose-response relationship
   - Key: Biological plausibility must support the gradient

3. All plausible confounders would reduce the effect
   - Upgrade if: Residual confounding would bias TOWARD the null
   - Key: This is the most conservative upgrade criterion
```

**Automated checks:**

```python
def verify_grade_assessment(grade_data: dict) -> dict:
    """Verify GRADE assessment completeness and internal consistency.

    Args:
        grade_data: Dict with keys: study_design, risk_of_bias,
            inconsistency, indirectness, imprecision, publication_bias,
            large_effect, dose_response, confounders_reduce,
            overall_certainty
    """
    issues = []

    # Starting level
    if grade_data["study_design"] == "RCT":
        starting_level = 4  # High
    elif grade_data["study_design"] == "observational":
        starting_level = 2  # Low
    else:
        issues.append(f"Unknown study design: {grade_data['study_design']}")
        starting_level = 2

    # Count downgrades
    downgrade_domains = [
        "risk_of_bias", "inconsistency", "indirectness",
        "imprecision", "publication_bias"
    ]
    total_downgrades = 0
    for domain in downgrade_domains:
        level = grade_data.get(domain, 0)
        if level not in [0, -1, -2]:
            issues.append(f"Invalid downgrade for {domain}: {level}")
        total_downgrades += abs(level)

    # Count upgrades (only for observational)
    upgrade_domains = ["large_effect", "dose_response", "confounders_reduce"]
    total_upgrades = 0
    for domain in upgrade_domains:
        level = grade_data.get(domain, 0)
        if grade_data["study_design"] == "RCT" and level > 0:
            issues.append(f"Upgrade '{domain}' applied to RCT evidence — unusual")
        total_upgrades += level

    # Calculate expected level
    expected_level = max(1, min(4, starting_level - total_downgrades + total_upgrades))
    level_names = {1: "Very Low", 2: "Low", 3: "Moderate", 4: "High"}
    expected_name = level_names.get(expected_level, "Unknown")

    stated = grade_data.get("overall_certainty", "")
    if stated and stated != expected_name:
        issues.append(
            f"Stated certainty '{stated}' does not match calculated "
            f"'{expected_name}' (start={starting_level}, "
            f"downgrades={total_downgrades}, upgrades={total_upgrades})"
        )

    # Check all domains assessed
    for domain in downgrade_domains:
        if domain not in grade_data:
            issues.append(f"Missing GRADE domain: {domain}")

    # Check justification
    if not grade_data.get("justification"):
        issues.append("No justification text for GRADE assessment")

    return {
        "valid": len(issues) == 0,
        "expected_certainty": expected_name,
        "issues": issues
    }
```

**When to apply:**

- Every systematic review outcome (non-negotiable per Cochrane, AHRQ, WHO)
- After completing the meta-analysis and sensitivity analyses
- Before writing the summary of findings table
- When communicating results to clinical guideline panels

</grade_certainty>

<statistical_validity>

## Statistical Validity Verification

Even if the clinical question and evidence are sound, the statistical analysis must be appropriate and its assumptions met.

**Principle:** Every statistical test has assumptions. Violating them does not always invalidate results, but the impact must be understood and reported. Key areas: model assumptions, multiplicity, power, and appropriate effect measures.

**Key statistical checks:**

### Assumption checking for meta-analysis

```
Random-effects model assumptions:
  - True effects are normally distributed across studies
    Check: Profile likelihood, permutation test for heterogeneity
    Violation impact: Moderate (DL is robust to mild non-normality)

  - Within-study variances are known (not estimated)
    Check: Large studies (>100 per arm) approximate this well
    Violation impact: Use Hartung-Knapp-Sidik-Jonkman (HKSJ) adjustment
    for small meta-analyses (< 5 studies) — standard DL is anti-conservative

  - Independence of effect estimates across studies
    Check: Are studies from the same research group? Multi-arm trials?
    Violation impact: Underestimated standard error; use robust variance
    estimation or multivariate methods for dependent estimates

Fixed-effect model assumptions:
  - One true effect across all studies (no heterogeneity)
    Check: I-squared, Q statistic, prediction interval
    Violation impact: CI too narrow, misleading precision
    Key: Even I-squared = 0% doesn't prove homogeneity with few studies
```

### Effect measure appropriateness

```
Binary outcomes:
  - Risk ratio (RR): Interpretable, consistent across baseline risks
    When: Event rate < 20%, or event rate similar across studies
  - Odds ratio (OR): Mathematically convenient, symmetric
    When: Case-control studies, logistic regression, rare events
  - Risk difference (RD): Clinically intuitive (NNT = 1/RD)
    When: Baseline risk matters for decision-making
  - Hazard ratio (HR): Time-to-event data
    When: Survival analysis, varying follow-up

  Common error: Using OR when events are common (>20%) and
  interpreting as RR. OR overestimates RR when events are common.
  Correction: RR = OR / (1 - p_control + p_control * OR)

Continuous outcomes:
  - Mean difference (MD): Same measurement scale across studies
    When: All studies use the same instrument (e.g., all use HAM-D)
  - Standardized mean difference (SMD): Different scales
    When: Different instruments measure the same construct
    Hedges' g preferred over Cohen's d (bias correction for small N)
  - Ratio of means: When proportional change is meaningful
    When: Lab values, biological measurements

  Common error: Using MD when scales differ (e.g., mixing VAS 0-100
  with NRS 0-10). Result is uninterpretable.
  Common error: Forgetting Hedges' g correction for small samples
  (Cohen's d overestimates effect in small studies).
```

### Power analysis

```python
def verify_meta_analysis_power(studies, effect_size, alpha=0.05, power_target=0.80):
    """
    Check whether the meta-analysis has adequate statistical power.

    Optimal Information Size (OIS): the total sample size that a single
    trial would need to detect the target effect at the specified power.

    Args:
        studies: List of dicts with 'n_treatment' and 'n_control'
        effect_size: Clinically meaningful effect size to detect
        alpha: Significance level
        power_target: Desired power
    """
    from scipy.stats import norm
    import numpy as np

    # Total sample size in meta-analysis
    total_n = sum(s['n_treatment'] + s['n_control'] for s in studies)

    # OIS for continuous outcome (two-sample z-test approximation)
    z_alpha = norm.ppf(1 - alpha / 2)
    z_beta = norm.ppf(power_target)
    ois_per_arm = 2 * ((z_alpha + z_beta) / effect_size) ** 2
    ois_total = 2 * ois_per_arm

    # Adjust for heterogeneity (multiply OIS by 1 / (1 - I_squared))
    # This is approximate but gives the right order of magnitude

    powered = total_n >= ois_total
    status = "PASS" if powered else "WARN"
    print(f"{status} Power check: total N = {total_n}, OIS = {ois_total:.0f}")
    print(f"  Meta-analysis {'meets' if powered else 'does NOT meet'} OIS")
    return powered
```

### Multiple comparisons

```
When testing multiple outcomes, subgroups, or timepoints:

  - Pre-specify primary outcome and analysis
  - Adjust alpha for multiple primary outcomes (Bonferroni, Holm, FDR)
  - Label secondary outcomes as exploratory
  - Distinguish confirmatory from exploratory subgroup analyses

  Common error: Testing 10 subgroups at alpha=0.05 and reporting
  the one significant result as a "finding" (p-hacking).
  Expected false positives: 0.5 out of 10 tests at alpha=0.05.

  Common error: Changing the primary outcome after seeing results
  (outcome switching). Detectable by comparing protocol to publication.

  Red flag: Subgroup analysis that was not pre-specified shows a
  significant interaction while the overall result is null.
```

### Heterogeneity assessment

```python
def verify_heterogeneity(effect_sizes, variances, model="RE"):
    """
    Assess and verify heterogeneity statistics.

    Returns Q, I-squared, tau-squared, H-squared, prediction interval.
    """
    import numpy as np

    k = len(effect_sizes)
    weights = 1.0 / np.array(variances)
    weighted_mean = np.sum(weights * np.array(effect_sizes)) / np.sum(weights)

    # Cochran's Q
    Q = np.sum(weights * (np.array(effect_sizes) - weighted_mean) ** 2)
    df = k - 1
    # p-value from chi-squared distribution
    from scipy.stats import chi2
    p_Q = 1 - chi2.cdf(Q, df)

    # I-squared
    I_sq = max(0, (Q - df) / Q * 100) if Q > 0 else 0

    # Tau-squared (DerSimonian-Laird)
    C = np.sum(weights) - np.sum(weights ** 2) / np.sum(weights)
    tau_sq = max(0, (Q - df) / C)

    # Prediction interval (where the true effect in a NEW study might lie)
    se_pooled = np.sqrt(1.0 / np.sum(weights))
    from scipy.stats import t as t_dist
    t_crit = t_dist.ppf(0.975, df=max(k - 2, 1))
    pi_lower = weighted_mean - t_crit * np.sqrt(tau_sq + se_pooled ** 2)
    pi_upper = weighted_mean + t_crit * np.sqrt(tau_sq + se_pooled ** 2)

    # Interpretation
    if I_sq < 25:
        heterogeneity_level = "low"
    elif I_sq < 50:
        heterogeneity_level = "moderate"
    elif I_sq < 75:
        heterogeneity_level = "substantial"
    else:
        heterogeneity_level = "considerable"

    print(f"Heterogeneity assessment (k={k} studies):")
    print(f"  Q = {Q:.2f}, df = {df}, p = {p_Q:.4f}")
    print(f"  I-squared = {I_sq:.1f}% ({heterogeneity_level})")
    print(f"  tau-squared = {tau_sq:.4f}")
    print(f"  Prediction interval: [{pi_lower:.3f}, {pi_upper:.3f}]")

    issues = []
    if I_sq > 75 and p_Q < 0.10:
        issues.append("Considerable heterogeneity — explore sources before pooling")
    if k < 5:
        issues.append("Few studies — heterogeneity estimates imprecise, "
                      "consider HKSJ method")
    if (pi_lower < 0 < pi_upper) and not (weighted_mean > pi_lower and weighted_mean < pi_upper):
        issues.append("Prediction interval crosses null — the effect in a "
                      "new study setting could go either way")

    return {
        "Q": Q, "p_Q": p_Q, "I_squared": I_sq, "tau_squared": tau_sq,
        "prediction_interval": (pi_lower, pi_upper),
        "heterogeneity_level": heterogeneity_level,
        "issues": issues
    }
```

**When to apply:**

- Every meta-analysis (model selection, effect measure, heterogeneity)
- Every statistical test (assumption checking)
- Every subgroup analysis (multiple comparisons, interaction tests)
- Before reporting any p-value or confidence interval

</statistical_validity>

<clinical_plausibility>

## Clinical Plausibility Checks

Even if a result is statistically significant and methodologically sound, it must make clinical and biological sense.

**Principle:** Medical research results exist within a web of biological mechanisms, clinical experience, and prior evidence. A new result should be consistent with established knowledge or, if surprising, the novelty must be justified.

**Universal plausibility checks:**

| Check | Condition | If Violated |
|-------|-----------|-------------|
| Effect size magnitude | Within historical range for the intervention class | Check for data extraction errors, unit errors |
| Effect direction | Consistent with known mechanism of action | Check PICO, verify no sign reversal |
| NNT/NNH reasonableness | NNT > 1, NNH > 1, clinically meaningful | Check arithmetic, baseline risk |
| Biological plausibility | Mechanism exists linking intervention to outcome | Discuss in limitations if no mechanism known |
| Dose-response consistency | Larger dose = larger effect (usually) | Check for non-monotonic relationship explanation |
| Temporal plausibility | Effect appears within biologically plausible timeframe | Check timepoint appropriateness |
| Baseline risk compatibility | Control group event rate matches epidemiological data | Check population selection, ascertainment bias |
| Subgroup coherence | Subgroup effects are consistent with biological reasoning | Check for p-hacking, interaction test |

**Domain-specific plausibility:**

### Pharmacological interventions

```
- Effect onset matches pharmacokinetics (e.g., antidepressants take 2-4 weeks)
- Effect size comparable to same drug class (e.g., SSRIs: SMD ~ 0.3 vs placebo)
- Dose-response present (unless plateau reached)
- Side effect profile consistent with mechanism
- Common error: Reporting a huge effect size (SMD > 1.0) from small trials
  (small-study effect, publication bias likely)
```

### Surgical interventions

```
- Surgical skill/volume effects plausible
- Recovery timeline matches procedure complexity
- Effect of sham control appropriate (placebo surgery has substantial effects)
- Common error: Comparing experienced surgeons (intervention) with
  non-specialist care (control) — confounded by expertise
```

### Diagnostic tests

```
- Sensitivity and specificity within plausible ranges for the test type
- Likelihood ratios meaningful (LR+ > 10 or LR- < 0.1 for definitive tests)
- Prevalence-adjusted PPV/NPV make clinical sense
- Common error: Reporting sensitivity/specificity from case-control studies
  with extreme populations (spectrum bias inflates accuracy)
```

### Screening interventions

```
- Lead-time bias accounted for (earlier detection != longer survival)
- Length-time bias accounted for (slow-growing detected more often)
- Overdiagnosis quantified (detecting disease that would never cause harm)
- Common error: Reporting improved 5-year survival as evidence of
  screening benefit without addressing these biases
```

**Order-of-magnitude benchmarks for medical research:**

```
Pharmacological effect sizes (vs placebo):
  - Small: SMD 0.2 (e.g., SSRIs for mild depression)
  - Medium: SMD 0.5 (e.g., statins for LDL reduction)
  - Large: SMD 0.8 (e.g., insulin for diabetic ketoacidosis)
  - Implausibly large: SMD > 1.5 in a placebo-controlled trial
    (suspect small-study bias or flawed methodology)

Risk reductions:
  - Aspirin for secondary CVD prevention: RR ~ 0.80
  - Statins for major vascular events: RR ~ 0.75
  - ACE inhibitors for heart failure mortality: RR ~ 0.77
  - Antibiotics for sepsis: RR varies widely by pathogen

NNT ranges:
  - Excellent: NNT 2-5 (e.g., antibiotics for bacterial meningitis)
  - Good: NNT 5-20 (e.g., statins for high-risk CVD prevention)
  - Moderate: NNT 20-100 (e.g., aspirin for primary prevention)
  - Marginal: NNT > 100 (e.g., screening for rare conditions)

  Red flag: NNT < 2 from a trial (implies almost all patients benefit,
  which is rare outside acute infectious disease treatment).
```

**When to apply:**

- After every meta-analysis (does the pooled estimate make clinical sense?)
- When effect sizes are unusually large or unusually small
- When results contradict clinical experience or biological mechanisms
- When presenting results to clinical decision-makers

</clinical_plausibility>

<in_execution_validation>

## In-Execution Validation Patterns

Validate intermediate results during the evidence synthesis process — catching errors as they happen rather than after all analyses are complete.

**Core Principle: Validate as you go, not after the fact.** Every step that produces a result should validate that result before the next step consumes it. Data extraction errors propagate and compound — a sign error in one study's effect size contaminates the entire meta-analysis.

### Validation Hierarchy

Apply in order of diagnostic power (cheapest and most informative first):

1. **Data Extraction Check (Every Study):** Before moving to synthesis, verify extracted data against the source paper.
2. **Effect Size Calculation Check (When Calculated):** Verify the conversion formula and input values before including in meta-analysis.
3. **PICO Consistency Check (Before Pooling):** Verify clinical homogeneity before running any meta-analysis.
4. **Statistical Sanity Check (After Every Analysis):** After generating results, check for implausible values before interpreting.

### When to Validate

| After This | Validate This | How |
|---|---|---|
| Extracting data from a study | Effect size, sample size, variance | Double-extract, check against tables/figures |
| Calculating an effect size | Direction, magnitude, SE | Recalculate from 2x2 table or means/SDs |
| Running a meta-analysis | Forest plot, heterogeneity, pooled estimate | Visual inspection + statistical checks |
| Running a sensitivity analysis | Consistency with main analysis | Compare direction, magnitude, significance |
| Generating a funnel plot | Symmetry, outliers | Visual + Egger's test |
| Performing subgroup analysis | Interaction test, within-subgroup estimates | Formal interaction p-value, not just comparing subgroup p-values |

### Validation Failure Protocol

When a validation check fails during execution:

1. **Stop the current analysis.** Do not proceed to interpretation.
2. **Record the failure** in the SUMMARY with details (expected, obtained, magnitude of discrepancy).
3. **Attempt to diagnose** using the data extraction: re-check source paper, recalculate effect size.
4. **If fixable within scope:** Fix, re-validate, continue.
5. **If not fixable:** Complete the current step with the failure noted, flag in SUMMARY as blocking.

</in_execution_validation>

## Systematic Review Verification Checklist

- [ ] PICO consistency: all included studies address the same question
- [ ] Search completeness: multiple databases, grey literature, no language restriction
- [ ] PRISMA flow diagram: numbers add up at each stage
- [ ] Data extraction: double-extracted, discrepancies resolved
- [ ] Risk of bias: assessed with appropriate tool for all studies
- [ ] Effect measure: appropriate for outcome type and pooling method
- [ ] Statistical model: appropriate for expected heterogeneity
- [ ] Heterogeneity: quantified and explored (I-squared, tau-squared, prediction interval)
- [ ] Sensitivity analyses: pre-specified and conducted
- [ ] Publication bias: assessed (if >= 10 studies)
- [ ] GRADE: certainty rated for each outcome

## Paper-Ready Result Checklist

- [ ] All applicable checks passed (see domain-specific files)
- [ ] Confidence intervals: reported for all estimates
- [ ] Heterogeneity: I-squared, tau-squared, prediction interval reported
- [ ] NNT/NNH: calculated for clinically meaningful threshold
- [ ] GRADE certainty: stated for each outcome
- [ ] Summary of findings table: complete per Cochrane standards
- [ ] Forest plot: generated, labeled, readable
- [ ] Funnel plot: generated if >= 10 studies
- [ ] Protocol registration: CRD/PROSPERO number stated
- [ ] Deviations from protocol: documented and justified
- [ ] Code/data available: analysis scripts and extracted data shared

## When to Require Human Verification

Some medical research checks cannot be fully automated. Flag these for human review:

**Always human:**

- Clinical interpretation of results (is the effect clinically meaningful, not just statistically significant?)
- Appropriateness of the research question (does this address a real clinical need?)
- Assessment of indirectness (are the included studies relevant to the target population?)
- Novel results that contradict current guidelines (error or new evidence?)
- Ethical considerations (should this change practice? Is equipoise maintained?)
- Patient-important outcome selection (are we measuring what matters to patients?)
- Guideline panel recommendations based on the evidence

**Human if uncertain:**

- Whether heterogeneity reflects true clinical differences or methodological noise
- Whether a large effect in observational studies reflects confounding or a true signal
- Whether surrogate outcomes are valid proxies for patient-important outcomes
- Whether to downgrade GRADE for indirectness (judgment call on relevance)
- Interpretation of conflicting subgroup analyses

## Adversarial Verification (Red Team Check)

For results with HIGH clinical impact (would change guidelines or prescribing), spawn a lightweight "red team" check:
- Goal: Find an error in this synthesis
- Try: data extraction error, wrong effect direction, PICO inconsistency, wrong statistical model
- Try: missing studies, publication bias, selective outcome reporting, p-hacking
- Try: construct a scenario where the recommendation based on this evidence causes harm
- Report: Either a specific error found, or "no error found after N checks"

## Common LLM Error Patterns in Medical Research (Top 5)

1. **Data extraction errors** (wrong column, wrong timepoint, wrong group): Caught by double-extraction and recalculation from 2x2 tables
2. **Effect direction errors** (benefit reported as harm or vice versa): Caught by biological plausibility check and visual inspection of forest plot
3. **Effect measure confusion** (OR interpreted as RR, MD used when SMD needed): Caught by PICO consistency check and scale verification
4. **Heterogeneity ignored** (pooling studies with I-squared > 75% without investigation): Caught by heterogeneity assessment and prediction interval
5. **Missing GRADE domains** (incomplete certainty assessment): Caught by automated GRADE completeness check

## See Also

- `references/verification/core/verification-quick-reference.md` — Compact checklist (default entry point)
- `references/verification/core/verification-numerical.md` — Statistical verification, convergence testing, automated verification
- `references/verification/core/verification-patterns.md` — Verification pattern index
- `references/verification/core/computational-verification-templates.md` — R/Python code templates
- `references/verification/core/code-testing-medical.md` — Testing patterns for medical research code
