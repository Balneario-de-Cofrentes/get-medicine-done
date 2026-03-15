---
load_when:
  - "health economics verification"
  - "cost-effectiveness analysis"
  - "QALY calculation"
  - "budget impact"
  - "CHEERS compliance"
  - "Markov model"
  - "willingness to pay"
  - "cost model validation"
tier: 2
context_cost: large
---

# Verification Domain — Health Economics

Cost model validation, QALY calculation accuracy, sensitivity analysis, budget impact completeness, CHEERS 2022 reporting compliance, willingness-to-pay threshold application, and Markov model transition probability validation for health economic evaluations.

**Load when:** Working on cost-effectiveness analysis, cost-utility analysis, budget impact analysis, health technology assessment, economic modeling, or CHEERS reporting evaluation.

**Related files:**
- `../core/verification-quick-reference.md` — compact checklist (default entry point)
- `../core/verification-core.md` — foundational verification principles
- `references/verification/domains/verification-domain-statistics.md` — statistical methods (for uncertainty quantification)
- `references/verification/domains/verification-domain-clinical-trial.md` — clinical trial (for efficacy inputs)
- `references/verification/domains/verification-domain-systematic-review.md` — systematic review (for evidence synthesis)

---

<cost_model_validation>

## Cost Model Validation

**Perspective verification:**

```
Perspective defines which costs and outcomes are included:

1. Healthcare system / payer perspective:
   Includes: direct medical costs (hospitalizations, medications, procedures, clinic visits).
   Excludes: patient out-of-pocket, productivity losses, informal caregiving.

2. Societal perspective:
   Includes: all healthcare costs + productivity losses + patient time costs + caregiver
   costs + transportation + other non-medical costs.
   ISPOR and the Second Panel on Cost-Effectiveness recommend societal as a reference case.

3. Patient perspective:
   Includes: out-of-pocket costs, time, lost income, non-covered services.

Verification:
1. CHECK: Perspective is explicitly stated in the methods.
2. CHECK: Included costs are CONSISTENT with the stated perspective.
   Common error: Claiming societal perspective but only including direct medical costs.
   If productivity losses are excluded under societal perspective: flag.
3. CHECK: If multiple perspectives reported: clearly labeled and separately presented.
4. CHECK: Guidelines require specific perspective.
   NICE (UK): NHS and personal social services.
   CADTH (Canada): healthcare system.
   US Second Panel: both healthcare sector AND societal.
```

**Time horizon:**

```
Time horizon must be sufficient to capture all relevant costs and outcomes.

Verification:
1. CHECK: Time horizon captures the full disease trajectory.
   Chronic diseases (diabetes, heart failure): lifetime horizon typically required.
   Acute interventions (surgery for fracture): shorter horizon may be acceptable if
   no long-term consequences.
2. CHECK: If time horizon < lifetime: justify why truncation does not bias results.
   Truncation typically biases against interventions with delayed benefits.
3. CHECK: Time horizon matches the available evidence.
   Extrapolating 5-year trial data to a lifetime horizon: requires explicit
   modeling assumptions and validation against external data.
```

**Discounting:**

```
Discounting adjusts future costs and outcomes to present value.
Reflects time preference: a benefit today is worth more than the same benefit in the future.

Standard rates:
- Costs: 3-5% per year (varies by jurisdiction).
- Outcomes (QALYs): 3-5% per year (some guidelines use differential discounting).
  NICE (UK): 3.5% for both costs and outcomes.
  US: 3% for both (Second Panel).
  Netherlands: 4% costs, 1.5% outcomes (differential discounting).

Formula: PV = FV / (1 + r)^t

Verification:
1. CHECK: Discount rate stated and matches relevant guideline.
2. CHECK: BOTH costs and outcomes are discounted (common error: discounting costs but not QALYs).
3. CHECK: Undiscounted results presented as sensitivity analysis.
   Results should be presented both discounted and undiscounted.
4. CHECK: Half-cycle correction applied in Markov models (see below).
   Without half-cycle correction: systematic bias in timing of events.
```

**Model structure verification:**

```
Decision tree models:
- Appropriate for short time horizons with clearly defined pathways.
- Verification: All branches are mutually exclusive and collectively exhaustive.
  Branch probabilities at each node sum to 1.0.

Markov models:
- Appropriate for chronic diseases with recurring events over time.
- States must be mutually exclusive and exhaustive.
- Verification: See Markov model section below.

Discrete event simulation (DES):
- Appropriate for complex patient pathways, queuing, resource constraints.
- Verification: Model convergence — run sufficient iterations (typically > 1000)
  until mean costs/QALYs stabilize (< 1% change with more iterations).

Partitioned survival models:
- Common in oncology. Area under survival curves = time in health state.
- Verification: Survival curves cannot cross (states must be properly ordered).
  Sum of time in all states must equal total time horizon.
```

</cost_model_validation>

<qaly_calculation>

## QALY Calculation Accuracy

**Utility weight verification:**

```
QALYs = sum over time of (utility weight × time in health state).
Utility weights range from 0 (death) to 1 (perfect health).
Negative values possible (states worse than death).

Utility elicitation methods:
1. Standard gamble (SG): Theoretically grounded in expected utility theory.
   Values tend to be higher than TTO.
2. Time trade-off (TTO): Trade years of life for better quality.
   More intuitive than SG.
3. Visual analogue scale (VAS): Rate health on a 0-100 scale.
   NOT recommended as primary method — not preference-based.
4. Multi-attribute utility instruments:
   - EQ-5D-5L: NICE preferred. Valuation sets are country-specific.
   - SF-6D (from SF-36): Broader health dimensions but narrower range.
   - HUI3: Includes sensory attributes (vision, hearing).

Verification:
1. CHECK: Utility values sourced from appropriate population.
   General population preferences are standard for resource allocation decisions.
   Patient preferences may differ (adaptation/coping elevates own health rating).
2. CHECK: Utility values match the health states defined in the model.
   Using a single utility for "cancer" when the model distinguishes stages: insufficient.
3. CHECK: Utility values are within plausible ranges.
   Common reference values:
   - Perfect health: 1.0
   - Stable angina: 0.77
   - Post-stroke (moderate disability): 0.51-0.71
   - Metastatic cancer on treatment: 0.50-0.70
   - Severe heart failure (NYHA IV): 0.50-0.60
   - End-stage renal disease on dialysis: 0.56-0.69
   Values outside published ranges without justification: flag.
4. CHECK: Disutility decrements for adverse events.
   Duration of disutility × magnitude. Short-duration events (nausea for 1 week)
   have minimal QALY impact but should still be included if differential between arms.
5. CHECK: Utility values NOT double-counted.
   If baseline utility already reflects comorbidity burden, applying additional
   decrements for the same comorbidity: double-counting.
```

**QALY computation verification:**

```
For Markov models:
QALY per cycle = sum over states (proportion in state × utility × cycle length)

With half-cycle correction:
QALY = 0.5 × (Q_start + Q_end) × cycle_length for each cycle.

Verification:
1. CHECK: Total QALYs are plausible.
   Remaining QALYs for a 65-year-old in good health: approximately 10-15 discounted QALYs.
   If model produces > 20 discounted QALYs from age 65: check for errors.
2. CHECK: QALY difference between arms.
   Very small QALY gains (< 0.01) may not be clinically meaningful.
   Very large QALY gains (> 5) in a non-curative intervention: verify.
3. CHECK: Death state has utility = 0 and accrues zero QALYs.
   Common error: absorbing death state accrues QALYs by not properly zeroing utility.
```

</qaly_calculation>

<sensitivity_analysis_econ>

## Sensitivity Analysis for Health Economic Models

**One-way (deterministic) sensitivity analysis:**

```
Vary each parameter individually across a plausible range while holding others at base case.

Tornado diagram: Ranks parameters by impact on the ICER.
Verification:
1. CHECK: All key parameters varied (not just a select few).
   Must include: efficacy, costs of intervention and comparator, utility values,
   discount rate, time horizon.
2. CHECK: Parameter ranges are justified (95% CI, published ranges, expert opinion).
   Arbitrary ranges (±20% for everything): insufficient — use evidence-based ranges.
3. CHECK: Threshold analysis for key parameters.
   At what parameter value does the ICER cross the WTP threshold?
   If threshold is close to the base case: result is fragile.
```

**Probabilistic sensitivity analysis (PSA):**

```
All parameters varied simultaneously according to probability distributions.
Generates a distribution of cost-effectiveness outcomes.

Distribution selection:
- Probabilities and utilities: Beta distribution (bounded 0-1).
- Relative risks / odds ratios: Lognormal distribution (bounded > 0).
- Costs: Gamma distribution (bounded > 0, right-skewed).
- Resource use counts: Poisson or negative binomial.

Verification:
1. CHECK: Appropriate distributions assigned to each parameter type.
   Normal distribution for a probability bounded 0-1: technically possible but
   can produce impossible values — prefer Beta.
2. CHECK: Number of iterations sufficient. Minimum 1000, prefer 5000-10000.
   Check convergence: mean ICER should stabilize.
3. CHECK: Cost-effectiveness acceptability curve (CEAC) presented.
   Shows probability of cost-effectiveness at each WTP threshold.
4. CHECK: Cost-effectiveness plane (scatter plot) presented.
   Northeast quadrant: more effective, more costly (most common).
   Southeast: dominant (more effective, less costly).
   Northwest: dominated (less effective, more costly).
5. CHECK: Expected value of perfect information (EVPI) calculated.
   EVPI quantifies the maximum value of eliminating all parameter uncertainty.
   High EVPI relative to research cost: additional research is warranted.
```

**Scenario analysis:**

```
Structural sensitivity analysis testing alternative model assumptions.

Common scenarios to verify:
1. Different time horizons (5-year, 10-year, lifetime).
2. Different discount rates (0%, 3%, 5%).
3. Different comparators (next best alternative, current standard of care).
4. Different data sources for key parameters.
5. Different extrapolation methods for survival data (Weibull, log-logistic, etc.).
6. Different model structures (Markov vs partitioned survival).

Verification:
1. CHECK: At least 3-5 clinically meaningful scenarios explored.
2. CHECK: Results presented for each scenario with interpretation of changes.
3. CHECK: If the conclusion (cost-effective or not) changes across plausible scenarios:
   the result is NOT robust. This must be acknowledged.
```

</sensitivity_analysis_econ>

<budget_impact>

## Budget Impact Analysis

**Completeness verification:**

```
Budget impact analysis (BIA) estimates the financial consequences of adopting
a new intervention from the perspective of a specific budget holder.

Required elements:
1. Target population: Number of eligible patients (incidence-based or prevalence-based).
2. Market uptake curve: Projected adoption rate over time (typically 1-5 years).
3. Costs included: Drug acquisition, administration, monitoring, adverse events,
   displaced therapies (cost offsets from replaced treatments).
4. Time horizon: Typically 1-5 years (short-term budget planning).
5. Current treatment mix: Distribution of existing treatments in the population.

Verification:
1. CHECK: Population estimate is sourced from epidemiological data.
   If prevalence × eligible fraction gives implausible numbers: flag.
2. CHECK: Market uptake is realistic and sourced (analogy with similar launches,
   expert opinion, market research).
   Immediate 100% uptake: unrealistic for most interventions.
3. CHECK: Cost offsets from displaced therapies are included.
   If the new treatment replaces an existing expensive treatment:
   net budget impact may be much smaller than gross drug cost.
4. CHECK: Both gross and net budget impact reported.
   Gross = total cost of new intervention.
   Net = gross - cost offsets from displaced therapies and avoided healthcare use.
5. CHECK: No discounting in BIA (unlike CEA).
   BIA uses actual (undiscounted) costs to reflect real budget expenditures.
   If discounting is applied: flag (ISPOR BIA guidelines recommend no discounting).
```

</budget_impact>

<cheers_compliance>

## CHEERS 2022 Reporting Compliance

**CHEERS 2022 checklist (28 items):**

```
Key items most frequently violated:

Item 3 — Population and subgroups: Characteristics of the target population,
  including subgroups. If equity considerations are relevant: include.

Item 7 — Comparators: All relevant comparators described and justified.
  Common error: comparing only with placebo/no treatment when active comparators exist.

Item 10 — Measurement and valuation of outcomes:
  How QALYs or DALYs were estimated, including utility elicitation method and source.
  Verification: CHECK method is preference-based and population-specific.

Item 13 — Analytics and assumptions:
  All structural assumptions described and justified.
  Verification: CHECK model assumptions are listed explicitly, not buried in supplementary.

Item 17 — Characterizing uncertainty:
  Effects of uncertainty on results and conclusions.
  Verification: CHECK that BOTH deterministic and probabilistic sensitivity analyses
  are reported. PSA alone is insufficient — one-way analysis identifies key drivers.

Item 19 — Characterizing heterogeneity:
  Differences in costs, outcomes, or cost-effectiveness among subgroups.
  Verification: CHECK if clinically relevant subgroups are analyzed
  (e.g., age groups, disease severity, comorbidities).

Item 21 — Limitations:
  All potential limitations, including data quality, model structure limitations,
  generalizability concerns.
  Verification: CHECK if key limitations are acknowledged.
  Missing: unmeasured uncertainty, extrapolation beyond trial data, transferability.

Item 24 — Source of funding and conflicts of interest.
  Verification: CHECK if the analysis was industry-funded and if authors have
  financial conflicts. Industry-funded analyses show systematic bias toward
  favorable cost-effectiveness findings — results should be scrutinized.

Item 28 — Transparency:
  Model code, input data, or access to the model should be available.
  Verification: CHECK if model is available for review/replication.
  Closed models with no access: reproducibility cannot be verified.
```

</cheers_compliance>

<wtp_threshold>

## Willingness-to-Pay Threshold Application

**Threshold values by jurisdiction:**

```
ICER interpretation: Additional cost per QALY gained.

Common thresholds:
- UK (NICE): GBP 20,000-30,000 per QALY.
  Exceptions: End-of-life criteria (up to GBP 50,000), highly specialized technologies.
- US: $50,000-150,000 per QALY (no official threshold; WHO suggested 1-3x GDP per capita).
  $100,000 per QALY is commonly used as a benchmark.
- Canada (CADTH): CAD 50,000 per QALY (implicit).
- Australia (PBAC): No explicit threshold; decisions based on clinical need and budget.
- WHO (now deprecated): 1-3x GDP per capita per DALY averted.
  This threshold has been criticized as too high for low-income countries.

Verification:
1. CHECK: Threshold matches the jurisdiction of the analysis.
   Using a US threshold ($100K/QALY) for a UK analysis: inappropriate.
2. CHECK: Currency year is stated and consistent.
   All costs must be in the same currency year. Inflation adjustment if sources span years.
3. CHECK: If ICER is near the threshold (within 20%): the conclusion is sensitive
   to small parameter changes. PSA and scenario analyses are critical.
4. CHECK: Dominated strategies (more costly AND less effective) should be excluded
   from the efficiency frontier. ICER is only meaningful for non-dominated strategies.
5. CHECK: Extended dominance — a strategy may be ruled out if a combination of two
   other strategies is more efficient. Verify extended dominance was assessed.
```

**Interpreting ICERs:**

```
ICER = (Cost_new - Cost_comparator) / (QALY_new - QALY_comparator)

Quadrant interpretation:
- Northeast (more effective, more costly): ICER is positive. Compare with threshold.
- Southeast (more effective, less costly): DOMINANT. New intervention preferred.
- Northwest (less effective, more costly): DOMINATED. Comparator preferred.
- Southwest (less effective, less costly): ICER is positive. Rare — cost savings
  per QALY lost. Threshold comparison applies inversely.

Verification:
1. CHECK: ICER calculation is arithmetic correct.
   Incremental cost / incremental QALY. Signs must be consistent.
2. CHECK: If QALY difference is very small (< 0.01): ICER is extremely sensitive
   to small changes in QALY. Confidence interval for ICER may be meaningless.
   Use net monetary benefit (NMB) instead: NMB = WTP × ΔQALY - ΔCost.
3. CHECK: Multiple comparators — use incremental analysis on the efficiency frontier.
   Pairwise ICERs (each vs a common comparator) can be misleading.
   Sequential ICERs against the next less effective non-dominated strategy are correct.
```

</wtp_threshold>

<markov_model>

## Markov Model Transition Probability Validation

**Transition matrix verification:**

```
A Markov model consists of health states and transition probabilities.

Transition matrix P: element p_ij = probability of moving from state i to state j in one cycle.

Requirements:
1. Row sums = 1.0 (each row represents all possible transitions from that state).
   Verification: COMPUTE sum of each row. Any row sum ≠ 1.0: ERROR.
2. All entries >= 0 (probabilities cannot be negative).
   Verification: CHECK no negative entries.
3. Absorbing states (death): row has 1.0 on the diagonal, 0 elsewhere.
   Once dead, always dead.
4. Tunnel states: if a health state should have a mandatory minimum duration,
   use tunnel states (sequential transient states that force time in the state).

Rate-to-probability conversion:
  p = 1 - exp(-r × t) where r = rate, t = cycle length.
  DO NOT use p = r × t for rates (valid only for very small rates, r × t < 0.05).
  Verification: CHECK that rates are properly converted, especially for long cycles.
  If cycle length = 1 year and annual rate = 0.5: p = 1 - exp(-0.5) = 0.393 (NOT 0.5).

Combining transition probabilities:
  If two independent events can occur: p_combined = 1 - (1 - p_A)(1 - p_B).
  NOT p_A + p_B (which can exceed 1.0).
  Verification: CHECK that competing risks are handled correctly.
```

**Half-cycle correction:**

```
In discrete-time Markov models, events are assumed to occur at the beginning
or end of each cycle. Half-cycle correction assumes events occur, on average,
at the midpoint of the cycle.

Methods:
1. Half-cycle correction: Add half a cycle of the starting distribution at the beginning
   and subtract half a cycle at the end.
   QALY_corrected = 0.5 × Q_0 + Q_1 + Q_2 + ... + Q_{T-1} + 0.5 × Q_T

2. Simpson's 1/3 rule or trapezoidal rule for more accurate approximation.

Verification:
1. CHECK: Half-cycle correction applied. Without it: systematic bias
   proportional to cycle length. For annual cycles: up to 0.5 QALY error.
2. CHECK: Alternatively, shorter cycle length (monthly instead of annual)
   reduces the need for half-cycle correction.
3. CHECK: Half-cycle correction applied to BOTH costs and QALYs.
```

**Model validation approaches:**

```
1. Internal validation:
   - Trace outputs sum to total cohort at each cycle.
   - Costs and QALYs are non-negative per cycle.
   - Absorbing state grows monotonically.
   - Model reproduces input data when run with input parameters.

2. External validation:
   - Compare model predictions with independent data sources.
   - Life expectancy from model vs life tables.
   - Event rates from model vs observed epidemiological data.
   - Verification: COMPUTE model-predicted survival curve and overlay
     with observed Kaplan-Meier from a validation dataset.

3. Cross-validation:
   - Compare with other published models of the same disease.
   - If results differ substantially: investigate structural differences.

4. Predictive validation:
   - Can the model predict outcomes in a dataset not used for calibration?
   - Strongest form of validation but rarely feasible.

Verification:
1. CHECK: At least internal validation is reported.
2. CHECK: External validation against independent data attempted.
3. CHECK: Model calibration process described if parameters were fitted to data.
```

</markov_model>

## Worked Examples

### ICER calculation and threshold comparison

```python
# Two-arm cost-effectiveness analysis
cost_new = 45000       # Total discounted cost, new treatment ($)
cost_comp = 32000      # Total discounted cost, comparator ($)
qaly_new = 8.5         # Total discounted QALYs, new treatment
qaly_comp = 7.8        # Total discounted QALYs, comparator

delta_cost = cost_new - cost_comp   # $13,000
delta_qaly = qaly_new - qaly_comp   # 0.7 QALYs

ICER = delta_cost / delta_qaly
print(f"ICER = ${ICER:,.0f} per QALY gained")
# ICER = $18,571 per QALY

# Threshold comparison
WTP_NICE_low = 20000    # GBP (but using USD here for illustration)
WTP_NICE_high = 30000
WTP_US = 100000

print(f"Cost-effective at NICE lower threshold ({WTP_NICE_low})? {ICER < WTP_NICE_low}")
print(f"Cost-effective at US threshold ({WTP_US})? {ICER < WTP_US}")

# Net monetary benefit at US threshold
NMB = WTP_US * delta_qaly - delta_cost
print(f"NMB at US threshold: ${NMB:,.0f}")
# NMB = $100,000 × 0.7 - $13,000 = $57,000 > 0 → cost-effective
```

### Markov transition probability validation

```python
import numpy as np

# 3-state Markov model: Healthy, Sick, Dead
# Annual transition probabilities
P = np.array([
    [0.85, 0.10, 0.05],  # From Healthy
    [0.20, 0.50, 0.30],  # From Sick
    [0.00, 0.00, 1.00],  # From Dead (absorbing)
])

# Verification 1: Row sums
row_sums = P.sum(axis=1)
print(f"Row sums: {row_sums}")
assert np.allclose(row_sums, 1.0), "ERROR: Row sums must equal 1.0"

# Verification 2: Non-negative
assert (P >= 0).all(), "ERROR: Negative probabilities found"

# Verification 3: Absorbing state
assert P[2, 2] == 1.0 and P[2, 0] == 0 and P[2, 1] == 0, "ERROR: Death not absorbing"

# Verification 4: Rate-to-probability conversion check
# If annual mortality rate for Healthy = 0.05, convert:
rate = 0.05
p_correct = 1 - np.exp(-rate)  # 0.04877
p_approximate = rate             # 0.05 (linear approximation)
print(f"Correct probability: {p_correct:.5f}, Approximate: {p_approximate:.5f}")
print(f"Error from linear approximation: {abs(p_approximate - p_correct)/p_correct*100:.1f}%")
# Error ~ 2.5% for rate = 0.05 — acceptable for small rates but not for larger ones.

# Run model for 20 cycles (years) with half-cycle correction
n_cycles = 20
cohort = np.array([1.0, 0.0, 0.0])  # Start in Healthy
utilities = np.array([0.90, 0.60, 0.00])  # Utility weights

qaly_total = 0.5 * np.dot(cohort, utilities)  # Half-cycle start
for cycle in range(n_cycles):
    cohort = cohort @ P
    weight = 0.5 if cycle == n_cycles - 1 else 1.0
    qaly_total += weight * np.dot(cohort, utilities)

print(f"Total QALYs over {n_cycles} years (with half-cycle correction): {qaly_total:.2f}")
```
