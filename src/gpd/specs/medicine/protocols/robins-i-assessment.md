---
load_when:
  - "ROBINS-I"
  - "non-randomized study"
  - "non-randomised"
  - "bias non-randomized"
  - "observational study bias"
  - "confounding bias"
  - "selection bias"
  - "time-varying confounding"
  - "target trial"
  - "emulation"
tier: 1
context_cost: medium
---

# ROBINS-I: Risk of Bias in Non-Randomised Studies of Interventions

ROBINS-I (Risk Of Bias In Non-randomised Studies of Interventions) is the Cochrane tool for assessing risk of bias in non-randomised studies that estimate the effect of an intervention. It structures the assessment around a hypothetical "target trial" -- the pragmatic RCT that would have answered the question if it had been conducted.

**Core principle:** All non-randomised studies have some risk of bias relative to a well-conducted RCT. ROBINS-I systematically evaluates whether the study's deviations from the target trial are likely to have introduced bias, and in what direction.

## Related Protocols

- `rob2-assessment.md` -- Equivalent tool for randomised trials
- `strobe-observational.md` -- Reporting guideline for observational studies
- `grade-certainty.md` -- ROBINS-I feeds into GRADE for non-randomised evidence
- `causal-inference.md` -- Methods to address confounding assessed in ROBINS-I

---

## The Target Trial Framework

Before assessing risk of bias, specify the target trial:

| Element | Description |
|---------|------------|
| **Eligible participants** | Who would be included in the ideal trial? |
| **Intervention** | What intervention strategies are being compared? |
| **Assignment** | How would participants be assigned? (random assignment) |
| **Follow-up** | When does follow-up start? When does it end? |
| **Outcome** | What is the outcome, and how and when is it measured? |
| **Causal contrast** | Intention-to-treat effect or per-protocol effect? |
| **Analysis plan** | How would the ideal trial be analyzed? |

The non-randomised study is then assessed for how well it emulates this target trial across seven bias domains.

---

## Seven Bias Domains

### Domain 1: Bias due to confounding

**This is typically the most critical domain for non-randomised studies.**

**Signalling questions:**

1.1 Is there potential for confounding of the effect of intervention in this study?
- If the study is a randomised trial with baseline randomisation, the answer is usually "No" (and ROBINS-I is not the appropriate tool).
- For non-randomised studies, the answer is almost always "Yes."

1.2 Was the analysis based on splitting participants into exposure groups at the start of follow-up?
- Time-zero alignment: participants must be classified at the start of follow-up (not retrospectively).
- Violation: classifying exposure based on future information introduces immortal time bias.

1.3 Were confounding domains that are appropriate to the target trial considered?
- List critical confounders (from DAG analysis, clinical knowledge, literature).
- Were these measured and adjusted for?

1.4 Were confounders measured validly and reliably?
- Self-reported? Administrative data? Direct measurement?

1.5 Did the authors use an appropriate analysis method that controlled for all the important confounding domains?
- Methods: multivariable regression, propensity score matching/weighting, instrumental variables, difference-in-differences.

1.6 If Y/PY to 1.5: Were confounding domains that were controlled for measured validly and reliably?

1.7 Did the authors control for any post-intervention variables that could have been affected by the intervention?
- Adjusting for a mediator introduces collider bias and removes part of the causal effect.

**Judgment:**

| Situation | Judgment |
|-----------|----------|
| All critical confounders appropriately controlled | Low risk |
| Most confounders controlled but residual confounding possible | Moderate risk |
| Critical confounders not controlled or inappropriate adjustment | Serious risk |
| Very strong confounding expected with no control | Critical risk |

### Domain 2: Bias in selection of participants into the study

**Signalling questions:**

2.1 Was selection of participants into the study (or into the analysis) based on participant characteristics observed after the start of intervention?
- If participants were selected based on receiving the full intervention (e.g., completing treatment), this introduces selection bias by conditioning on an event that may be affected by the intervention.

2.2 If Y/PY to 2.1: Were the post-intervention variables that influenced selection likely to be associated with intervention?

2.3 If Y/PY to 2.2: Were the post-intervention variables that influenced selection likely to be influenced by the outcome or a cause of the outcome?

2.4 Do start of follow-up and start of intervention coincide for most participants?
- If not: immortal time bias (the period between study entry and treatment start, during which the outcome cannot occur).

2.5 If Y/PY to 2.2 and 2.3, or N/PN to 2.4: Were adjustment techniques used that are likely to correct for the presence of selection biases?

**Common selection bias patterns:**

| Pattern | Example | Direction of Bias |
|---------|---------|-------------------|
| Prevalent user bias | Including patients already on treatment (survivors) | Favors treatment |
| Immortal time bias | Person-time before treatment start counted as treated | Favors treatment |
| Depletion of susceptibles | Susceptible patients already experienced the outcome before study | Unpredictable |
| Healthy user bias | Healthier patients more likely to initiate treatment | Favors treatment |
| Referral bias | Treatment group drawn from tertiary centers, controls from general population | Depends on direction |

### Domain 3: Bias in classification of interventions

**Signalling questions:**

3.1 Were intervention groups clearly defined?
- Ambiguous intervention definitions lead to misclassification.

3.2 Was the information used to define intervention groups recorded at the start of the intervention?
- Retrospective classification based on information available after intervention start can introduce bias.

3.3 Could classification of intervention status have been affected by knowledge of the outcome or risk of the outcome?
- If outcome information influenced how intervention was classified, this is a form of differential misclassification.

**Judgment:**

| Situation | Judgment |
|-----------|----------|
| Intervention well defined and recorded prospectively | Low risk |
| Minor ambiguity but unlikely to be differential | Moderate risk |
| Classification possibly influenced by outcome knowledge | Serious risk |

### Domain 4: Bias due to deviations from intended interventions

**Signalling questions:**

4.1 Were there deviations from the intended intervention beyond what would be expected in usual practice?
4.2 If Y/PY to 4.1: Were these deviations from intended intervention unbalanced between groups and likely to have affected the outcome?
4.3 Were important co-interventions balanced across intervention groups?
4.4 Was the intervention implemented successfully for most participants?
4.5 Did study participants adhere to the assigned intervention regimen?
4.6 If N/PN to 4.3, 4.4, or 4.5: Was an appropriate analysis used to estimate the effect of starting and adhering to the intervention?

**Key consideration:** For the intention-to-treat effect (effect of initiating the intervention), only deviations that arise from the study context (not usual practice) matter. For the per-protocol effect (effect of sustained adherence), all deviations from the protocol matter, and appropriate methods (e.g., inverse probability of censoring weights, g-estimation) are needed.

### Domain 5: Bias due to missing data

**Signalling questions:**

5.1 Were outcome data available for all, or nearly all, participants?
5.2 Were participants excluded due to missing data on intervention status?
5.3 Were participants excluded due to missing data on other variables needed for the analysis?
5.4 If Y/PY to 5.1, 5.2, or 5.3: Are the proportion of participants and reasons for missing data similar across interventions?
5.5 If Y/PY to 5.1, 5.2, or 5.3: Is there evidence that results were robust to the presence of missing data?

**Judgment follows similar logic to RoB 2 Domain 3**, but with additional consideration of missing confounder data (which can bias the confounding adjustment).

### Domain 6: Bias in measurement of outcomes

**Signalling questions:**

6.1 Could the outcome measure have been influenced by knowledge of the intervention received?
6.2 Were outcome assessors aware of the intervention received by study participants?
6.3 Were the methods of outcome assessment comparable across intervention groups?
6.4 Were any systematic errors in measurement of the outcome related to intervention status?

**Assessment parallels RoB 2 Domain 4.** Objective outcomes (mortality, laboratory values) are at lower risk than subjective outcomes (clinician-assessed, patient-reported) when blinding is absent.

### Domain 7: Bias in selection of the reported result

**Signalling questions:**

7.1 Is the reported effect estimate likely to be selected, on the basis of the results, from multiple outcome measurements within the outcome domain?
7.2 Is the reported effect estimate likely to be selected, on the basis of the results, from multiple analyses of the intervention-outcome relationship?
7.3 Is the reported effect estimate likely to be selected, on the basis of the results, from different subgroups?

**Assessment parallels RoB 2 Domain 5.** Check the study protocol/registration against the published report for discrepancies.

---

## Overall Risk of Bias Judgment

| Level | Interpretation |
|-------|---------------|
| **Low risk** | Comparable to a well-performed RCT; all domains at low risk |
| **Moderate risk** | Sound for a non-randomised study; some concerns in at least one domain but no serious risk |
| **Serious risk** | Important problems in at least one domain |
| **Critical risk** | Too problematic to provide useful evidence; at least one domain at critical risk |
| **No information** | Insufficient reporting to judge risk of bias |

**Practical reality:** Most non-randomised studies will be at least moderate risk of bias. A judgment of "low risk" (comparable to an RCT) is uncommon and requires near-perfect confounding control.

---

## ROBINS-I vs RoB 2 Comparison

| Feature | RoB 2 | ROBINS-I |
|---------|-------|----------|
| Study type | Randomised trials | Non-randomised studies |
| Number of domains | 5 | 7 |
| Confounding domain | Not applicable (randomization handles it) | Primary domain, often most critical |
| Selection domain | Not in original (covered by randomization) | Includes immortal time bias, prevalent user bias |
| Baseline comparability | Guaranteed by randomization | Must be assessed explicitly |
| Expected minimum risk | Low | Moderate (at best) |
| Target trial concept | The trial itself is the target | Hypothetical ideal trial must be specified |

---

## Practical Tips

1. **Always specify the target trial first.** Without it, assessors cannot judge what "ideal" looks like.
2. **Use a directed acyclic graph (DAG)** to identify confounders, mediators, and colliders before assessing Domain 1 (see `causal-inference.md`).
3. **Immortal time bias is underrecognized.** If there is any gap between cohort entry and treatment initiation, check Domain 2 carefully.
4. **Two assessors independently** is strongly recommended; disagreements should be resolved by discussion.
5. **Document the reasoning** for each signalling question, not just the yes/no answer.
6. **A "critical" judgment in any domain** means the study should generally be excluded from the synthesis or clearly flagged.

---

## Verification Checklist

Before finalizing ROBINS-I assessments:

- [ ] Target trial specified for the review question
- [ ] Assessment is outcome-specific (different outcomes may have different risk profiles)
- [ ] Domain 1 (confounding): critical confounders identified via DAG or clinical expertise
- [ ] Domain 2 (selection): immortal time bias and prevalent user bias assessed
- [ ] Domain 3 (classification): intervention groups clearly defined and prospectively recorded
- [ ] Domain 4 (deviations): analysis method matches the causal contrast of interest
- [ ] Domain 5 (missing data): missing confounder data addressed, not just missing outcomes
- [ ] Domain 6 (measurement): outcome objectivity and blinding assessed
- [ ] Domain 7 (reporting): protocol/registration compared with published report
- [ ] Overall judgment accounts for the most critical domain
- [ ] Two independent assessors with documented reasoning
