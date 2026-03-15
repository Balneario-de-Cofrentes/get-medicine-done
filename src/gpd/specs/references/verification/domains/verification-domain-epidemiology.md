---
load_when:
  - "epidemiology verification"
  - "confounding control"
  - "selection bias"
  - "causal inference"
  - "Bradford Hill"
  - "DAG causal diagram"
  - "STROBE compliance"
  - "immortal time bias"
tier: 2
context_cost: large
---

# Verification Domain — Epidemiology

Confounding control, selection bias identification, information bias assessment, causal inference validity, immortal time bias detection, ecological fallacy prevention, and STROBE compliance for observational epidemiological studies.

**Load when:** Working on observational study methodology, causal inference, confounding assessment, bias identification, DAG construction, or STROBE reporting evaluation.

**Related files:**
- `../core/verification-quick-reference.md` — compact checklist (default entry point)
- `../core/verification-core.md` — foundational verification principles
- `references/verification/domains/verification-domain-statistics.md` — statistical methods (for regression and adjustment)
- `references/verification/domains/verification-domain-systematic-review.md` — systematic review (for ROBINS-I assessment)
- `references/verification/domains/verification-domain-meta-analysis.md` — meta-analysis (for pooling observational studies)

---

<confounding_control>

## Confounding Control

**Directed acyclic graphs (DAGs):**

```
A DAG encodes the causal structure among exposure (X), outcome (Y), and covariates.
Fundamental structures:

1. Confounder: Z → X and Z → Y.
   Must be adjusted for. Failure to adjust: biased estimate.

2. Mediator: X → M → Y.
   Should NOT be adjusted for when estimating total effect of X on Y.
   Adjusting for mediator estimates the direct effect (different question).

3. Collider: X → C ← Y.
   Should NOT be adjusted for. Adjusting for a collider INDUCES spurious association
   between X and Y (collider bias / Berkson's bias).

4. Instrumental variable: I → X → Y (and I has no direct effect on Y or shared causes with Y).
   Used for causal inference under unconfoundedness violations.

Verification:
1. CHECK: Is a DAG presented or can one be constructed from the study design?
2. CHECK: Adjustment set is a valid d-separation set blocking all backdoor paths
   from X to Y without opening collider paths.
   Use the backdoor criterion: adjust for a set S such that S blocks all backdoor paths
   and S does not contain any descendant of X on a causal path to Y.
3. CHECK: No overadjustment. Adjusting for descendants of the exposure (mediators)
   or for variables affected by the outcome: bias.
4. TOOLS: DAGitty (dagitty.net) can compute minimal sufficient adjustment sets
   from a specified DAG. Verify that the study's adjustment set matches.
```

**Adjustment methods:**

```
Stratification:
  Divides data into strata defined by confounders. Stratum-specific estimates then pooled.
  Limitation: Sparse strata with many confounders ("curse of dimensionality").
  Appropriate for: 1-3 binary confounders with adequate sample per stratum.

Multivariable regression:
  Adjusts for confounders as covariates.
  Verification:
  1. CHECK: Model specification appropriate (linear for continuous, logistic for binary).
  2. CHECK: Functional form of confounders (linear assumption for continuous covariates).
     Non-linear confounding missed by linear adjustment: residual confounding.
  3. CHECK: Events-per-variable (EPV) ratio for logistic regression.
     EPV < 10: unstable estimates, overfitting risk.

Propensity score methods:
  - Matching: 1:1, 1:k, or variable ratio. Check covariate balance post-matching.
  - Weighting (IPTW): Inverse probability of treatment weighting.
    Extreme weights (> 10 or > 20) indicate positivity violations — flag.
    Stabilized weights or weight truncation should be used.
  - Stratification by PS quintiles.
  Verification:
  1. CHECK: Propensity score model includes all measured confounders.
  2. CHECK: Covariate balance after matching/weighting.
     Standardized mean difference (SMD) < 0.1 for all covariates: adequate balance.
     If any SMD > 0.25: residual confounding likely.
  3. CHECK: Positivity assumption — overlap in PS distributions between groups.
     If non-overlap: treated units have no comparators — estimates are extrapolated.

Instrumental variable analysis:
  Requirements: relevance (instrument predicts exposure), exclusion restriction
  (instrument affects outcome only through exposure), independence (no shared confounders).
  Verification:
  1. CHECK: First-stage F-statistic > 10 (relevance). F < 10: weak instrument bias.
  2. CHECK: Exclusion restriction is argued from domain knowledge, not testable.
  3. CHECK: Monotonicity assumption if using LATE interpretation.
```

**Residual confounding:**

```
Even with adjustment, residual confounding persists when:
1. Unmeasured confounders exist (no data collected).
2. Confounders are measured with error (misclassification dilutes adjustment).
3. Functional form of confounders is incorrect in the model.

Quantitative bias analysis (QBA):
- E-value: Minimum strength of unmeasured confounding (on RR scale) needed to
  explain away the observed association.
  E-value = RR + sqrt(RR × (RR - 1))
  Larger E-value: harder for unmeasured confounding to explain the result.

Verification:
1. CHECK: Is the potential for unmeasured confounding acknowledged?
2. CHECK: Is an E-value or other sensitivity analysis for unmeasured confounding presented?
3. CHECK: Is the E-value plausible? Compare with known confounder-exposure and
   confounder-outcome associations in the literature.
   If E-value = 1.5 and known confounders have RR > 3: confounding is plausible.
```

</confounding_control>

<selection_bias>

## Selection Bias Identification and Mitigation

**Types of selection bias:**

```
1. Prevalent case bias (Neyman bias):
  Studying prevalent rather than incident cases. Survivors overrepresented.
  Diseases with high early mortality or rapid recovery are missed.
  Verification: CHECK if study enrolled incident or prevalent cases.
  If prevalent: risk factors for survival are conflated with risk factors for disease.

2. Self-selection bias (volunteer bias):
  Participants who volunteer differ from the target population.
  Typically healthier, more educated, more health-conscious.
  "Healthy volunteer effect" in occupational studies.
  Verification: COMPARE demographics of participants with the source population.

3. Berkson's bias (admission rate bias):
  Hospital-based case-control studies: exposure associated with hospitalization
  independent of the disease creates spurious association.
  Verification: CHECK if controls are hospital-based. If yes: assess whether the
  conditions leading to hospitalization are related to the exposure.

4. Loss to follow-up bias (attrition):
  Differential loss between exposed and unexposed groups.
  Verification:
  - CHECK: Follow-up rates per group. Differential loss > 5%: concerning.
  - CHECK: Reasons for loss. If related to the outcome: biased estimates.
  - COMPUTE: Worst-case analysis — assume all lost in exposed group had event,
    all lost in unexposed did not. If conclusion changes: result is fragile.

5. Collider-stratification bias:
  Conditioning on a common effect of exposure and outcome (or their causes).
  "Index event bias" in studies of prognosis after disease diagnosis.
  Verification: CHECK if analysis conditions on a variable that is caused by both
  exposure and outcome. If yes: collider bias is present.
```

**Mitigation strategies:**

```
1. Inverse probability of censoring/selection weights (IPCW/IPSW).
   Weight observations by inverse probability of being selected/remaining in study.
   Verification: Check for extreme weights and positivity.

2. Sensitivity analysis for selection bias:
   Bound estimates under different assumptions about the selected-out population.

3. Target trial emulation framework:
   Structure observational analysis to mimic a hypothetical RCT.
   Explicitly define: eligibility, treatment strategies, assignment, follow-up start,
   outcomes, causal contrast, analysis plan.
   Verification: CHECK each element of the target trial is specified.
```

</selection_bias>

<information_bias>

## Information Bias

**Recall bias:**

```
Definition: Differential recall of exposures between cases and controls.
Cases (with disease) tend to recall exposures more thoroughly than controls.

Verification:
1. CHECK: Study design. Recall bias primarily affects case-control studies.
   Cohort studies with prospective exposure measurement: minimal recall bias.
2. CHECK: Exposure measurement method.
   Self-reported questionnaires: high risk.
   Medical records, biomarkers, registries: low risk.
3. CHECK: Was recall differential assessed?
   Validation sub-study comparing self-report with objective records.
   Discordance rate between cases and controls: measure of recall bias.
4. EXPECTED DIRECTION: Recall bias typically inflates associations (cases over-report exposure).
```

**Misclassification:**

```
Non-differential misclassification:
  Exposure or outcome measurement error that is equally distributed across groups.
  Effect on binary exposure with binary outcome: biases toward the null.
  EXCEPTION: Non-differential misclassification of a CONFOUNDER does NOT bias toward null —
  it results in residual confounding with unpredictable direction.

Differential misclassification:
  Error rate differs between exposure groups or between cases/controls.
  Can bias in either direction — magnitude and direction depend on the pattern.

Verification:
1. CHECK: Sensitivity and specificity of exposure measurement.
   Perfect classification: Se = 1, Sp = 1.
   If Se or Sp < 0.80: substantial misclassification — quantify bias.
2. CHECK: Sensitivity and specificity of outcome ascertainment.
   For disease outcomes: diagnostic criteria and validation against gold standard.
3. COMPUTE: Bias-adjusted estimate using simple quantitative bias analysis:
   Corrected 2x2 table using Se and Sp values:
   A_corrected = (A - (1-Sp)*N1) / (Se + Sp - 1)
   where A = observed cases in exposed, N1 = total exposed.
4. CHECK: If multiple exposure categories: non-differential misclassification
   can bias in ANY direction (not necessarily toward null).
```

</information_bias>

<causal_inference>

## Causal Inference Validity

**Bradford Hill criteria (viewpoints for causation):**

```
Nine viewpoints (not rigid criteria, but considerations):

1. Strength of association: Larger effect sizes are less likely due to confounding alone.
   But weak associations can still be causal (e.g., passive smoking and lung cancer).

2. Consistency: Association observed in different populations, settings, designs.
   Verification: CHECK if the finding has been replicated.

3. Specificity: One exposure leads to one outcome.
   Largely outdated — most exposures cause multiple outcomes. Low weight.

4. Temporality: Exposure MUST precede outcome. This is the only absolute criterion.
   Verification: CHECK temporal sequence. Cross-sectional studies CANNOT establish temporality.
   Reverse causation is a real threat in case-control and cross-sectional designs.

5. Biological gradient (dose-response): Higher exposure → higher risk.
   Verification: CHECK for monotonic dose-response. Non-linear relationships (U-shaped,
   threshold) do not violate causality but require explanation.

6. Plausibility: Biologically plausible mechanism.
   Limited by current knowledge — absence of known mechanism does not negate causation.

7. Coherence: Finding does not conflict with known biology.
   Similar to plausibility but broader (epidemiological and laboratory evidence agree).

8. Experiment: Removal of exposure reduces disease (intervention evidence).
   Strongest evidence from RCTs. Cessation studies in observational contexts.

9. Analogy: Similar exposures cause similar outcomes.
   Weak criterion — used for hypothesis generation.

Verification:
1. DO NOT treat Bradford Hill as a checklist with a threshold score.
   They are viewpoints to weigh collectively, not requirements to satisfy.
2. CHECK: Temporality is established (mandatory).
3. CHECK: Authors consider alternative explanations (confounding, bias, chance)
   before asserting causation.
```

**Counterfactual framework:**

```
Causal effect = Y(1) - Y(0) for each individual.
Fundamental problem: only one potential outcome is observed per individual.

Average treatment effect (ATE): E[Y(1) - Y(0)]
Average treatment effect on the treated (ATT): E[Y(1) - Y(0) | A = 1]

Required assumptions:
1. Consistency: The observed outcome under treatment A = a equals the potential outcome Y(a).
   Violated when multiple versions of treatment exist.
2. Exchangeability (no unmeasured confounding): Y(a) ⊥ A | L for all a.
   Conditional on measured confounders L, treatment groups are comparable.
3. Positivity: P(A = a | L = l) > 0 for all l with P(L = l) > 0.
   Every covariate stratum has some treated and some untreated individuals.

Verification:
1. CHECK: Consistency — is the treatment well-defined?
   "Physical activity" without specifying type, frequency, duration: ill-defined intervention.
2. CHECK: Exchangeability — are all important confounders measured and adjusted for?
   Use DAGs to identify the required adjustment set.
3. CHECK: Positivity — are there covariate strata with no treated or no untreated?
   Structural violations (e.g., pregnant men): handled by restriction.
   Random violations (sparse data): handled by smoothing or weight truncation.
```

</causal_inference>

<immortal_time_bias>

## Immortal Time Bias Detection

**Definition and mechanism:**

```
Immortal time bias occurs when a period of follow-up during which the outcome
CANNOT occur is misclassified or mishandled in the analysis.

Classic scenario: Comparing users vs non-users of a medication when medication
use is determined during follow-up (not at baseline).
- Time from cohort entry to first prescription = "immortal" for the exposed group
  (patient must survive to receive the drug).
- If this immortal time is attributed to the exposed group: biased in favor of treatment.
- If this immortal time is excluded entirely: person-time is misallocated.

Verification:
1. CHECK: Is exposure status determined at a fixed baseline (cohort entry)?
   If yes: no immortal time bias.
   If exposure status is determined during follow-up: immortal time bias is possible.
2. CHECK: How is pre-exposure person-time handled?
   CORRECT: Time-varying exposure analysis (person contributes unexposed time until
   first exposure, then exposed time thereafter).
   WRONG: Classifying entire follow-up as exposed based on ever-use.
   WRONG: Excluding pre-exposure time from the unexposed group but including it
   for the exposed group.
3. MAGNITUDE: Immortal time bias typically produces 10-50% relative risk reduction
   in favor of the treatment. Suspicious "protective effects" of medications should
   be checked for this bias.
```

**Detection strategies:**

```
1. CHECK: Time zero definition. Does follow-up start at a uniform point for all participants?
   If exposed start at first prescription and unexposed start at cohort entry: biased design.
2. CHECK: Landmark analysis used? A landmark (fixed time point) after which exposure status
   is assessed eliminates immortal time bias but requires all participants to survive to landmark.
3. CHECK: Time-varying Cox model used? Properly handles time-varying exposure.
   But verify that exposure status is updated correctly (lag time considerations for drugs
   with delayed onset of action).
4. SENSITIVITY: Compare results with different exposure lag times (0, 30, 90, 180 days).
   If results are very sensitive to lag: immortal time bias or time-window bias may be present.
```

</immortal_time_bias>

<ecological_fallacy>

## Ecological Fallacy Prevention

**Definition:**

```
Ecological fallacy: Inferring individual-level associations from group-level
(aggregate) data. The association observed at the group level may not hold —
or may even reverse — at the individual level.

Classic example: Country-level fat intake vs breast cancer rates show strong
positive correlation, but individual-level studies show weaker or null association.
Aggregate data confounded by national wealth, healthcare access, screening rates.

Verification:
1. CHECK: Unit of analysis. If data are at group level (countries, regions, hospitals)
   but conclusions are about individuals: ecological fallacy risk.
2. CHECK: Cross-level inference. Statements like "patients in hospitals with higher
   volume have better outcomes, therefore higher volume improves patient outcomes"
   commits the ecological fallacy — patient-level confounding is not addressed.
3. CHECK: Within-group variability. Aggregation masks individual variation.
   Simpson's paradox: the direction of association can reverse at different levels of aggregation.
```

**When ecological studies are valid:**

```
1. The intervention or exposure truly operates at the group level
   (e.g., water fluoridation, air pollution policy, hospital-level protocols).
2. The research question is explicitly about group-level effects.
3. Multilevel modeling is used to separate group-level and individual-level effects.

Verification:
1. CHECK: If ecological design, is the exposure inherently group-level?
2. CHECK: Are multilevel models used to account for clustering?
3. CHECK: Is the ecological correlation interpreted cautiously with appropriate caveats?
```

</ecological_fallacy>

<strobe_compliance>

## STROBE Compliance

**STROBE checklist (22 items for observational studies):**

```
Key items to verify (most commonly violated):

Item 6 — Eligibility criteria: Clearly defined inclusion/exclusion criteria.
Item 7 — Variables: All variables defined, including confounders, effect modifiers.
Item 9 — Bias: Direction and magnitude of potential biases discussed.
Item 12 — Statistical methods:
  (a) All methods described, including confounding control.
  (b) Subgroup and interaction analyses.
  (c) Missing data handling.
  (d) Loss to follow-up (cohort).
  (e) Sensitivity analyses.
Item 13 — Participants: Flow diagram or numbers at each stage.
Item 14 — Descriptive data: Demographics, exposures, confounders, follow-up time.
Item 16 — Main results:
  (a) Unadjusted AND adjusted estimates with CIs.
  (b) Category boundaries for continuous variables.
  (c) Absolute measures if meaningful (not just ORs or HRs).

Verification:
1. CHECK: Both crude and adjusted estimates reported (Item 16a).
   Reporting ONLY adjusted estimates: cannot assess confounding magnitude.
2. CHECK: Confounders listed and rationale for inclusion (Item 7, 16a).
3. CHECK: Missing data — number of missing values per variable reported (Item 12c).
4. CHECK: Loss to follow-up quantified and compared between groups (Item 12d).
5. CHECK: Study design identified in title or abstract (cohort, case-control, cross-sectional).
```

</strobe_compliance>

## Worked Examples

### Detecting immortal time bias in a pharmacoepidemiology study

```
Study claims: "Statin use associated with 30% reduction in cancer mortality
(HR 0.70, 95% CI 0.62-0.80)."

Verification checklist:
1. Exposure definition: "Ever use of statins during follow-up."
   FLAG: Ever-use during follow-up = classic immortal time bias setup.
   Patients must survive long enough to receive a statin prescription.

2. Time zero: "Cohort entry at first healthcare contact."
   Pre-statin time is "immortal" for the statin group.

3. Analysis method: "Cox proportional hazards comparing statin users vs non-users."
   FLAG: Time-fixed exposure in Cox model with time-varying exposure status.

4. Correction: Re-analyze with time-varying exposure.
   Typically reduces HR toward null: e.g., HR 0.70 → HR 0.92.
   The 30% "benefit" was largely an artifact of immortal time bias.
```

### E-value calculation for unmeasured confounding

```python
import math

# Observed association: RR = 2.0, lower 95% CI = 1.5
RR = 2.0
RR_lower = 1.5

# E-value for point estimate
E_value = RR + math.sqrt(RR * (RR - 1))
print(f"E-value for RR = {RR}: {E_value:.2f}")
# E-value = 2.0 + sqrt(2.0) = 3.41

# E-value for lower CI bound
E_value_ci = RR_lower + math.sqrt(RR_lower * (RR_lower - 1))
print(f"E-value for lower CI = {RR_lower}: {E_value_ci:.2f}")
# E-value = 1.5 + sqrt(0.75) = 2.37

# Interpretation:
# An unmeasured confounder would need to be associated with BOTH the exposure
# and the outcome by a risk ratio of at least 3.41 (point estimate) or 2.37
# (to move the CI to include null) to explain away the observed association.
# Compare with known confounders: if smoking has RR ~ 2 for both exposure
# and outcome, E-value of 3.41 suggests confounding alone is unlikely.
```
