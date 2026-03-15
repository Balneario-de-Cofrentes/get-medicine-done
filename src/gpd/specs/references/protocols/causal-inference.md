---
load_when:
  - "causal inference"
  - "DAG"
  - "directed acyclic graph"
  - "propensity score"
  - "instrumental variable"
  - "Mendelian randomization"
  - "confounding"
  - "counterfactual"
  - "potential outcomes"
  - "causal diagram"
  - "backdoor criterion"
  - "collider"
  - "mediator"
  - "target trial"
  - "emulation"
tier: 2
context_cost: high
---

# Causal Inference in Medical Research

Causal inference is the set of methods and frameworks used to move from observed associations to causal conclusions. In medical research, most clinical questions are causal: "Does this treatment cause better outcomes?" This protocol covers the foundational frameworks (potential outcomes, DAGs), and key methods (propensity scores, instrumental variables, Mendelian randomization, target trial emulation).

**Core principle:** Association is not causation, but causation implies association under identifiable conditions. The goal of causal inference is to specify those conditions explicitly and assess whether they hold in the data at hand.

## Related Protocols

- `strobe-observational.md` -- Reporting observational studies (where causal inference methods are most needed)
- `robins-i-assessment.md` -- ROBINS-I uses the target trial framework
- `pico-framework.md` -- Causal questions are PICO questions

---

## Two Frameworks for Causal Inference

### 1. Potential Outcomes (Counterfactual) Framework

**Rubin causal model:**

For each individual i, define:
- Y_i(1): the outcome that WOULD occur under treatment
- Y_i(0): the outcome that WOULD occur under control

**Individual causal effect:** Y_i(1) - Y_i(0)

**Fundamental problem:** We can only observe one potential outcome for each individual. The other is the counterfactual -- it never happened and cannot be observed.

**Average Treatment Effect (ATE):** E[Y(1) - Y(0)] = E[Y(1)] - E[Y(0)]

**Key assumptions for identification:**

| Assumption | Formal Statement | Meaning |
|-----------|-----------------|---------|
| **Exchangeability** (no unmeasured confounding) | Y(a) is independent of A, given L | Treatment groups are comparable in all prognostic factors after conditioning on L |
| **Positivity** | P(A = a \| L = l) > 0 for all l | Every type of patient has a non-zero probability of receiving each treatment |
| **Consistency** | If A = a, then Y = Y(a) | The observed outcome under treatment a equals the potential outcome under a |
| **No interference** (SUTVA) | Y_i(a) does not depend on other individuals' treatment | One person's treatment does not affect another's outcome |

### 2. Structural Causal Model / DAG Framework

**Pearl's framework:**

A directed acyclic graph (DAG) encodes assumptions about the causal structure:
- **Nodes:** Variables
- **Directed edges:** Direct causal effects (A -> Y means A directly causes Y)
- **Acyclic:** No cycles (A -> B -> A is forbidden)

**Key concepts:**

| Concept | Definition | Example |
|---------|-----------|---------|
| **Confounder** | Common cause of treatment and outcome | Age causes both treatment choice and outcome |
| **Mediator** | Variable on the causal pathway | Treatment -> Adherence -> Outcome (Adherence is a mediator) |
| **Collider** | Variable caused by both treatment and outcome (or their causes) | Hospital admission caused by both disease severity and treatment |
| **Instrument** | Variable that affects outcome only through treatment | Physician preference for treatment A vs. B |

---

## Directed Acyclic Graphs (DAGs)

### Building a DAG

1. **List all relevant variables:** Treatment, outcome, potential confounders, mediators, instruments.
2. **Draw directed edges** based on substantive knowledge (not data).
3. **Include all common causes** of any two variables already in the DAG.
4. **Do not include** variables that are clearly irrelevant.
5. **Time ordering:** Edges must respect temporal ordering.

### Using DAGs to Identify Adjustment Sets

**Backdoor criterion:** A set of variables Z satisfies the backdoor criterion relative to (A, Y) if:
1. No variable in Z is a descendant of A.
2. Z blocks every path from A to Y that has an arrow into A (backdoor paths).

If a set Z satisfies the backdoor criterion, adjusting for Z identifies the causal effect of A on Y.

**d-separation rules:**

| Path Structure | Blocked by Default? | Conditioning Effect |
|---------------|---------------------|-------------------|
| A <- L -> Y (confounder) | No (open) | Conditioning on L BLOCKS the path |
| A -> M -> Y (mediator) | No (open) | Conditioning on M BLOCKS the path (do NOT condition on mediators for total effect) |
| A -> C <- Y (collider) | Yes (blocked) | Conditioning on C OPENS the path (collider bias) |

**Common DAG errors:**

| Error | Consequence | Prevention |
|-------|-------------|------------|
| Adjusting for a mediator | Removes part of the causal effect; may introduce collider bias | Identify mediators before analysis; exclude from adjustment set |
| Adjusting for a collider | Opens a previously blocked path; introduces bias | Identify colliders in DAG; exclude from adjustment set |
| Missing a confounder | Residual confounding; biased estimate | Use domain expertise to identify all relevant common causes |
| Adjusting for a descendant of treatment | May partially adjust away the effect | Check variable's position relative to treatment in DAG |

### DAG Software

| Tool | URL | Features |
|------|-----|----------|
| DAGitty | http://www.dagitty.net | Web-based, finds adjustment sets, tests testable implications |
| ggdag (R) | CRAN | R package for drawing and analyzing DAGs |
| causaldag (Python) | PyPI | Python package for DAG operations |

---

## Propensity Score Methods

The propensity score e(X) = P(A = 1 | X) is the probability of receiving treatment given observed covariates.

**Balancing property:** Within strata of the propensity score, the distribution of observed covariates is balanced between treated and untreated.

### Methods Using Propensity Scores

| Method | Description | Strengths | Weaknesses |
|--------|-------------|-----------|------------|
| **Matching** | Match treated to untreated on propensity score | Intuitive, visual balance check | Discards unmatched participants |
| **IPTW** (Inverse Probability of Treatment Weighting) | Weight each observation by 1/e(X) or 1/(1-e(X)) | Uses all data, handles time-varying confounding | Extreme weights destabilize estimates |
| **Stratification** | Stratify by propensity score quintiles | Simple | Coarse adjustment; residual confounding within strata |
| **Regression adjustment** | Include propensity score as covariate | Uses all data | Model misspecification risk |
| **Doubly robust** | Combine IPTW with outcome regression | Consistent if either model is correct | More complex; may amplify bias if both models wrong |

### Practical Steps for Propensity Score Analysis

1. **Model the propensity score:** Logistic regression or machine learning (gradient boosting, random forest).
2. **Check overlap (positivity):** Plot propensity score distributions for treated and untreated. Trim or truncate if extreme values exist.
3. **Match or weight:** Apply chosen method.
4. **Assess balance:** Standardized mean differences (SMD < 0.1 is generally acceptable) for all covariates.
5. **Estimate the effect:** Analyze the matched/weighted sample.
6. **Sensitivity analysis:** Assess sensitivity to unmeasured confounding (E-value, Rosenbaum bounds).

### The E-Value

The E-value quantifies how strong unmeasured confounding would need to be to explain away the observed association.

```
E-value = RR + sqrt(RR x (RR - 1))

where RR is the observed risk ratio (or point estimate converted to RR scale)
```

**Interpretation:** "An unmeasured confounder would need to be associated with both the treatment and the outcome by a risk ratio of at least [E-value] to fully explain the observed association, above and beyond the measured confounders."

A large E-value suggests the result is robust to unmeasured confounding. A small E-value (close to 1) suggests the result is fragile.

---

## Instrumental Variables (IV)

An instrument Z is a variable that:
1. **Relevance:** Z is associated with the treatment A.
2. **Exclusion restriction:** Z affects the outcome Y ONLY through A.
3. **Independence:** Z is not associated with unmeasured confounders of A-Y.

### Common Instruments in Medical Research

| Instrument | Treatment | Rationale |
|-----------|-----------|-----------|
| Physician prescribing preference | Drug A vs. Drug B | Physicians vary in preference; preference is not related to individual patient prognosis |
| Distance to specialty center | Receiving specialized treatment | Distance affects access but not underlying prognosis |
| Calendar time / guideline change | Receiving the guideline-recommended treatment | Treatment patterns change; patient mix may not |
| Genetic variant (Mendelian randomization) | Biomarker level / exposure | Genetic variants are randomly allocated at conception |

### IV Estimation

**Two-stage least squares (2SLS):**

- Stage 1: Regress A on Z (predict treatment from instrument).
- Stage 2: Regress Y on predicted A (estimate effect using predicted treatment).

**Interpretation:** IV estimates the Local Average Treatment Effect (LATE) -- the causal effect among "compliers" (those whose treatment is influenced by the instrument). This is NOT the ATE.

---

## Mendelian Randomization (MR)

MR uses genetic variants as instruments to estimate causal effects of modifiable exposures on health outcomes.

**Rationale:** Genetic variants are allocated randomly at conception (Mendel's laws), so they are not confounded by postnatal factors. This provides a "natural experiment."

### Assumptions

1. **Relevance:** The genetic variant is associated with the exposure (check with F-statistic > 10).
2. **Independence:** The variant is not associated with confounders of the exposure-outcome relationship.
3. **Exclusion restriction:** The variant affects the outcome ONLY through the exposure (no horizontal pleiotropy).

### Methods

| Method | Description | Assumption Relaxed |
|--------|-------------|-------------------|
| **IVW** (Inverse-Variance Weighted) | Standard MR with multiple instruments | None (requires all 3 assumptions) |
| **MR-Egger** | Intercept test for directional pleiotropy | Relaxes exclusion restriction (allows directional pleiotropy) |
| **Weighted median** | Median of individual variant estimates | Valid if >= 50% of weight comes from valid instruments |
| **MR-PRESSO** | Detects and corrects for outlier instruments | Identifies individual pleiotropic variants |
| **Multivariable MR** | Adjusts for measured pleiotropic pathways | Controls for known pleiotropy |

### Sensitivity Analyses for MR

1. **MR-Egger intercept test:** Tests for directional pleiotropy.
2. **Cochran's Q:** Tests for heterogeneity across instruments (suggests pleiotropy).
3. **Leave-one-out:** Sequentially remove each variant; check stability.
4. **Steiger filtering:** Remove variants that explain more variance in the outcome than the exposure.
5. **Bidirectional MR:** Test the reverse direction (outcome -> exposure) to assess reverse causation.

---

## Target Trial Emulation

Target trial emulation is a framework for designing observational studies as if they were emulating a hypothetical pragmatic randomized trial. Developed by Hernan and Robins.

### Protocol

| Trial Element | Specification | Observational Study Implementation |
|--------------|--------------|-----------------------------------|
| Eligibility | Who is eligible | Apply same criteria to the observational dataset |
| Treatment strategies | What interventions are compared | Define intervention groups based on observed treatment patterns |
| Assignment | How participants are assigned | Not random; use causal inference methods to adjust |
| Follow-up start | When follow-up begins (time zero) | Align treatment initiation and follow-up start (avoid immortal time bias) |
| Follow-up end | When follow-up ends | Same censoring rules as the target trial |
| Outcome | What is measured | Same outcome definition |
| Causal contrast | ITT or per-protocol | Specify; per-protocol requires adjustment for time-varying confounding |
| Analysis | How to analyze | Clone-censor-weight approach, g-methods, or similar |

### Common Biases Avoided by Target Trial Emulation

| Bias | How Emulation Prevents It |
|------|--------------------------|
| Immortal time bias | Aligns time zero with treatment initiation |
| Selection bias (prevalent user) | Restricts to new users (new-user design) |
| Time-related biases | Explicit time zero specification |
| Undefined causal contrast | Forces specification of the causal question |

---

## Choosing the Right Method

| Clinical Question | Available Data | Recommended Method |
|------------------|---------------|-------------------|
| Treatment effect, many confounders measured | Large observational database | Propensity score matching/IPTW + E-value |
| Treatment effect, unmeasured confounding suspected | Data with valid instrument | Instrumental variables |
| Causal effect of biomarker/exposure | GWAS summary statistics available | Mendelian randomization |
| Treatment effect from observational data | Longitudinal data with treatment changes | Target trial emulation with g-methods |
| Mediation (how treatment works) | RCT or observational with mediator measured | Causal mediation analysis |
| Time-varying treatment and confounding | Panel data with repeated measures | Marginal structural models (IPTW over time) |

---

## Verification Checklist

Before finalizing a causal inference analysis:

- [ ] Causal question explicitly stated (ATE, ATT, or LATE)
- [ ] DAG constructed based on domain knowledge (not data-driven)
- [ ] Adjustment set identified using backdoor criterion
- [ ] Mediators identified and NOT adjusted for (unless mediation analysis)
- [ ] Colliders identified and NOT adjusted for
- [ ] Positivity (overlap) assessed and documented
- [ ] For propensity scores: balance assessed using SMD (< 0.1)
- [ ] For IV: instrument strength checked (F > 10), exclusion restriction justified
- [ ] For MR: pleiotropy assessed using MR-Egger, weighted median, MR-PRESSO
- [ ] E-value or other sensitivity analysis for unmeasured confounding
- [ ] Causal language only used when assumptions are met and explicitly stated
- [ ] Target trial protocol specified if emulating a trial from observational data
