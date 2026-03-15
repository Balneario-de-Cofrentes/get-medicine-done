---
load_when:
  - "network meta-analysis"
  - "NMA"
  - "indirect comparison"
  - "mixed treatment comparison"
  - "transitivity"
  - "consistency"
  - "ranking"
  - "SUCRA"
  - "P-score"
  - "league table"
  - "treatment network"
  - "multiple treatments"
tier: 2
context_cost: high
---

# Network Meta-Analysis Protocol

Network meta-analysis (NMA) extends pairwise meta-analysis by simultaneously synthesizing direct and indirect evidence across a network of treatment comparisons. It enables comparison of interventions that have not been directly compared in head-to-head trials, and produces a ranking of all interventions for each outcome.

**Core principle:** NMA borrows strength from indirect evidence, but this borrowing is only valid if the transitivity assumption holds. Without transitivity, indirect comparisons are biased, and the network is a house of cards.

## Related Protocols

- `statistical-analysis.md` -- Pairwise meta-analysis (prerequisite knowledge)
- `grade-certainty.md` -- GRADE for NMA (CINeMA framework)
- `prisma-systematic-review.md` -- PRISMA-NMA extension for reporting
- `search-strategy.md` -- NMA requires broader searches to capture all relevant comparisons

---

## When to Conduct NMA

### Appropriate

- Multiple interventions exist for the same condition and the clinical question is "which treatment is best?"
- Some comparisons have direct evidence, others do not.
- Decision-makers need to choose among more than two options.
- The network of evidence is connected (all treatments linked by at least one path of comparisons).

### Not Appropriate

- Network is disconnected (isolated subsets of comparisons with no bridge).
- Transitivity assumption is clearly violated.
- Only two treatments of interest exist (use pairwise meta-analysis).
- Insufficient studies to form a meaningful network.

---

## Key Assumptions

### 1. Transitivity (Similarity)

The most critical assumption. Transitivity requires that the relative effect of A vs. C estimated indirectly (through B) is the same as what would be estimated in a direct A vs. C trial.

**Condition:** Studies comparing different pairs of treatments must be sufficiently similar in effect modifiers that they could, in principle, have included any of the treatments in the network.

**Threats to transitivity:**

| Threat | Example | Assessment |
|--------|---------|------------|
| Different populations | Trials of A vs. B in mild disease; C vs. B in severe disease | Compare distribution of effect modifiers across comparisons |
| Different doses | A vs. B uses high-dose A; A vs. C uses low-dose A | Ensure intervention definitions are consistent |
| Different co-interventions | Background therapy differs across trials | Assess and report co-intervention patterns |
| Different outcome definitions | Different measurement tools or time points | Standardize outcomes |
| Different eras | Old trials for one comparison, recent for another | Medical practice may have changed |

**How to assess transitivity:**

1. List potential effect modifiers (from clinical expertise and literature).
2. Create a table of study characteristics grouped by comparison.
3. Compare the distribution of effect modifiers across comparisons.
4. If distributions differ importantly, transitivity is threatened.

### 2. Consistency (Coherence)

Consistency means that the direct and indirect evidence for the same comparison agree. This is a testable consequence of transitivity (when direct evidence exists for a comparison).

**Testing consistency:**

| Method | Description | When to Use |
|--------|-------------|-------------|
| **Node-splitting** | Compare direct and indirect estimates for each comparison | Default; tests each comparison with direct evidence |
| **Design-by-treatment interaction** | Global test across all comparisons | Overall assessment |
| **Back-calculation** | Compare direct estimate to indirect estimate | Individual comparisons |
| **Net heat plot** | Heatmap showing inconsistency hotspots | Visual exploration |

**Interpretation:**

- If the consistency test is significant for a comparison, investigate why. Different populations? Different intervention implementations?
- If global inconsistency is detected, the NMA results are unreliable and the model should be reconsidered.
- Absence of significant inconsistency does NOT prove consistency -- the test has low power with few studies.

### 3. Homogeneity

Within each comparison, the treatment effect should be reasonably homogeneous across studies (same assumption as pairwise random-effects meta-analysis).

---

## Statistical Models

### Frequentist NMA

**Using R package `netmeta`:**

```r
library(netmeta)

# Prepare data (pairwise contrasts format)
# Required: study, treatment1, treatment2, effect estimate, standard error
net <- netmeta(TE, seTE, treat1, treat2, studlab,
               data = mydata,
               sm = "RR",         # effect measure
               reference.group = "placebo",
               common = FALSE,    # random-effects model
               prediction = TRUE) # include prediction intervals

# Network graph
netgraph(net, number.of.studies = TRUE, thickness = "number.of.studies")

# Forest plot vs. reference
forest(net, reference.group = "placebo", sortvar = TE)

# League table (all pairwise comparisons)
netleague(net, digits = 2)

# Ranking (P-scores)
netrank(net, small.values = "desirable")

# Consistency assessment (node-splitting)
netsplit(net)

# Design-by-treatment interaction test
decomp.design(net)
```

### Bayesian NMA

**Using R package `gemtc` (with JAGS or OpenBUGS):**

```r
library(gemtc)

# Create network object
network <- mtc.network(data.ab = arm_level_data)

# Run NMA model
model <- mtc.model(network, type = "consistency",
                   linearModel = "random",
                   likelihood = "binom",
                   link = "log")

results <- mtc.run(model, n.adapt = 5000, n.iter = 50000, thin = 10)

# MCMC diagnostics
gelman.diag(results)
summary(results)

# Ranking (SUCRA)
ranks <- rank.probability(results)
sucra <- apply(ranks, 2, function(x) sum(cumsum(x) - x/2) / (length(x) - 1))
```

**Using R package `multinma` (Stan-based):**

```r
library(multinma)

# IPD and aggregate data can be combined
nma_data <- set_agd_arm(mydata, study = study, trt = trt,
                        r = responders, n = sample_size)

# Fit NMA model
fit <- nma(nma_data, trt_effects = "random",
           prior_intercept = normal(scale = 100),
           prior_trt = normal(scale = 10),
           prior_het = half_normal(scale = 1))

# Summary and plots
summary(fit)
plot(fit, ref_trt = "placebo")
```

---

## Ranking Treatments

### Methods

| Method | Framework | Description | Output |
|--------|-----------|-------------|--------|
| **SUCRA** | Bayesian | Surface Under the Cumulative Ranking curve | 0-100% (100% = always best) |
| **P-score** | Frequentist | Frequentist analogue of SUCRA | 0-1 (1 = always best) |
| **Mean rank** | Either | Average rank across iterations/simulations | 1 to k (1 = best) |
| **Probability of being best** | Either | Probability each treatment ranks first | 0-100% |
| **Rankogram** | Either | Distribution of rankings for each treatment | Histogram per treatment |

### Interpreting Rankings

**Rankings should be interpreted with extreme caution:**

1. Rankings can be misleading when treatments have similar effects -- the ranking may appear definitive when the differences are trivial.
2. Always present rankings alongside the absolute and relative effect estimates.
3. Report the 95% credible interval around the rank.
4. Consider using the "first-best" and "first-worst" probabilities together to avoid focusing only on the top.
5. Rankings do not account for safety -- a treatment ranked best for efficacy may rank worst for adverse events.

**GRADE for NMA (CINeMA):** Use the CINeMA (Confidence in Network Meta-Analysis) framework to rate the certainty of NMA estimates for each comparison. It extends GRADE with additional considerations for within-study bias, reporting bias, indirectness, imprecision, heterogeneity, and incoherence.

---

## Network Geometry

### Network Plot

The network plot shows treatments as nodes and comparisons as edges:

- **Node size:** Proportional to the number of participants receiving each treatment.
- **Edge thickness:** Proportional to the number of studies for each comparison.
- **Edge labels:** Number of studies per comparison.

### Assessing Network Structure

| Feature | Concern | Implication |
|---------|---------|-------------|
| Disconnected network | Cannot estimate all comparisons | Split into separate NMAs or report as disconnected |
| Star-shaped (all vs. placebo) | No direct head-to-head comparisons | All active-active comparisons are entirely indirect |
| One comparison dominates | Vast majority of evidence for one pair | Network heavily dependent on transitivity for other comparisons |
| Few studies per comparison | Imprecise indirect estimates | Wide confidence intervals; rankings unreliable |

---

## Reporting (PRISMA-NMA Extension)

### Additional Items Beyond Standard PRISMA

| Item | Description |
|------|------------|
| Network plot | Visual representation of the evidence network |
| Geometry assessment | Description of network structure and connectivity |
| Transitivity assessment | How transitivity was assessed and whether it held |
| Consistency assessment | Methods and results of consistency tests |
| Ranking | Method used, results with uncertainty, interpretation caveats |
| League table | All pairwise comparisons in a matrix format |
| Contribution matrix | Which studies contribute to each comparison estimate |
| CINeMA assessment | GRADE-based certainty rating for NMA estimates |

---

## Common Errors in NMA

| Error | Problem | Prevention |
|-------|---------|------------|
| Not assessing transitivity | Invalid indirect comparisons | Tabulate effect modifiers by comparison before running NMA |
| Ignoring inconsistency | Mixing inconsistent evidence | Run node-splitting for all comparisons with direct evidence |
| Over-interpreting rankings | Presenting rankings as definitive when effects are similar | Always show effect estimates alongside rankings |
| Including disconnected evidence | Cannot estimate all comparisons | Check network connectivity before analysis |
| Conflating SUCRA with probability of being best | SUCRA = 80% does NOT mean 80% probability of being best | Report both SUCRA and probability of being best |
| Not reporting all pairwise comparisons | Readers need the full picture | Include the complete league table |
| Inadequate MCMC diagnostics (Bayesian) | Unconverged chains give unreliable results | Report R-hat, effective sample size, trace plots |
| Missing treatments | Not including all relevant treatments biases rankings | Search comprehensively for all treatments used in practice |

---

## Verification Checklist

Before finalizing a network meta-analysis:

- [ ] Network is connected (all treatments linked)
- [ ] Transitivity assessed by comparing effect modifiers across comparisons
- [ ] Statistical model specified (frequentist or Bayesian, common or random effects)
- [ ] Consistency tested (node-splitting and/or global test)
- [ ] If inconsistency detected: investigated and reported; model reconsidered
- [ ] Network plot presented with node sizes and edge thicknesses
- [ ] League table with all pairwise comparisons and 95% CI/CrI
- [ ] Rankings reported with uncertainty (CrI around ranks, rankograms)
- [ ] Rankings contextualized with effect estimates (not presented in isolation)
- [ ] CINeMA/GRADE certainty assessment completed
- [ ] MCMC diagnostics reported if Bayesian (R-hat, ESS, trace plots)
- [ ] PRISMA-NMA extension items addressed
- [ ] Sensitivity analyses: excluding studies, different priors, different models
