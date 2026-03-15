---
load_when:
  - "GRADE"
  - "certainty of evidence"
  - "quality of evidence"
  - "downgrade"
  - "upgrade"
  - "Summary of Findings"
  - "evidence rating"
  - "strength of recommendation"
  - "body of evidence"
  - "imprecision"
  - "inconsistency"
  - "indirectness"
tier: 1
context_cost: medium
---

# GRADE Approach: Assessing Certainty of Evidence

GRADE (Grading of Recommendations, Assessment, Development and Evaluations) is the internationally accepted framework for rating the certainty (quality) of a body of evidence and strength of recommendations. It is used by over 100 organizations worldwide, including WHO, Cochrane, and most guideline development groups.

**Core principle:** GRADE separates the certainty in the evidence (how much we trust the effect estimate) from the strength of the recommendation (what to do about it). High certainty evidence can lead to weak recommendations (and vice versa) depending on the balance of benefits, harms, values, and resources.

## Related Protocols

- `prisma-systematic-review.md` -- GRADE is applied within systematic reviews
- `rob2-assessment.md` -- Risk of bias assessment feeds into GRADE domain 1
- `robins-i-assessment.md` -- ROBINS-I informs GRADE for non-randomised evidence
- `statistical-analysis.md` -- Heterogeneity and imprecision calculations

---

## Starting Certainty Rating

| Study Design | Starting Certainty |
|-------------|-------------------|
| Randomised controlled trials | High (4) |
| Observational studies (cohort, case-control) | Low (2) |

The starting rating is then adjusted based on five domains for downgrading and three domains for upgrading.

---

## Five Domains for Downgrading

### Domain 1: Risk of Bias

**Question:** Are there concerns about the risk of bias in the body of evidence contributing to this outcome?

**Assessment:**

- Use RoB 2 for RCTs (see `rob2-assessment.md`) or ROBINS-I for non-randomised studies (see `robins-i-assessment.md`).
- Consider the proportion of information from studies at high risk of bias.
- Consider the likely direction and magnitude of bias.

**Decision criteria:**

| Situation | Action |
|-----------|--------|
| Most information from low risk of bias studies | No downgrade |
| Some concerns in studies contributing most weight | Consider downgrading one level (-1) |
| Most information from high risk of bias studies, or bias likely changes the effect direction | Downgrade one or two levels (-1 or -2) |

**Common patterns requiring downgrade:**

- Inadequate allocation concealment across most trials
- Substantial loss to follow-up (> 20%) without appropriate analysis
- Outcome assessors not blinded for subjective outcomes
- Selective outcome reporting across studies
- Per-protocol analysis used when substantial crossovers occurred

### Domain 2: Inconsistency

**Question:** Are the results inconsistent across studies?

**Assessment:**

- Examine the forest plot: do confidence intervals overlap? Are point estimates in the same direction?
- Examine heterogeneity statistics: I-squared, tau-squared, prediction interval.
- Can inconsistency be explained by pre-specified subgroup analyses?

**Decision criteria:**

| Situation | Action |
|-----------|--------|
| I-squared < 40%, overlapping CIs, same direction | No downgrade |
| I-squared 40-75%, some non-overlap, but same direction | Consider downgrading one level (-1) |
| I-squared > 75%, opposite directions, unexplained | Downgrade one or two levels (-1 or -2) |

**Key guidance:**

- I-squared alone is insufficient. A large I-squared with all studies showing benefit is less concerning than a moderate I-squared with studies showing both benefit and harm.
- The prediction interval is more informative than I-squared for clinical decision-making -- it shows the range of plausible effects in a new setting.
- If heterogeneity is explained by a credible subgroup analysis (interaction test p < 0.05, a priori hypothesis, consistent direction), consider not downgrading and presenting subgroup-specific estimates.

### Domain 3: Indirectness

**Question:** Is the evidence directly relevant to the review question (PICO)?

**Assessment categories:**

| Type of Indirectness | Example |
|---------------------|---------|
| Population | Studies in adults, question about adolescents |
| Intervention | Studies of drug A, question about drug B (same class) |
| Comparator | Studies vs. placebo, question vs. active control |
| Outcome | Studies measure surrogate (blood pressure), question about clinical endpoint (stroke) |
| Setting | Studies from tertiary hospitals, question about primary care |
| Head-to-head absent | Only indirect comparison available via network meta-analysis |

**Decision criteria:**

| Situation | Action |
|-----------|--------|
| PICO matches directly | No downgrade |
| One element of PICO differs but is plausibly applicable | Consider downgrading one level (-1) |
| Multiple elements differ or surrogate outcome used without validated association | Downgrade one or two levels (-1 or -2) |

**Surrogate outcomes:**

- Only use surrogates when a validated association with patient-important outcomes exists.
- Validated surrogates: HbA1c for diabetic complications (moderate validation), viral load for HIV progression (strong validation).
- Questionable surrogates: bone mineral density for fractures (weak validation), tumor response for overall survival (depends on cancer type).

### Domain 4: Imprecision

**Question:** Are the results precise enough for decision-making?

**Assessment:**

- Examine the 95% confidence interval of the pooled estimate.
- Consider whether the CI crosses the threshold of clinical importance (the minimally important difference, MID).
- Consider the optimal information size (OIS): the total sample size the meta-analysis would need if it were a single adequately powered trial.

**Decision criteria:**

| Situation | Action |
|-----------|--------|
| CI excludes no effect AND excludes the MID in the opposite direction, OIS met | No downgrade |
| CI crosses either the null or the MID but not both | Consider downgrading one level (-1) |
| CI crosses both the null and the MID, or OIS not met and CI includes appreciable benefit and harm | Downgrade two levels (-2) |

**Optimal Information Size (OIS):**

- Calculate as: the sample size needed for a single trial to detect the MID with alpha = 0.05 and power = 80%.
- If the total participants in the meta-analysis are fewer than the OIS, consider downgrading for imprecision even if the CI does not cross the null.
- For binary outcomes: typically requires at least 300 events for precise estimates.

### Domain 5: Publication Bias

**Question:** Is there a high probability that studies with particular results are missing?

**Assessment:**

- Examine funnel plot asymmetry (requires at least 10 studies).
- Apply statistical tests: Egger's test (continuous outcomes), Peters' test (binary outcomes).
- Consider comprehensive search for grey literature and trial registries.
- Compare results of published vs. unpublished studies if available.
- Industry funding patterns and their association with positive results.

**Decision criteria:**

| Situation | Action |
|-----------|--------|
| Comprehensive search, no funnel plot asymmetry, registered protocols found | No downgrade |
| Some funnel plot asymmetry or gaps in the search | Consider downgrading one level (-1) |
| Strong evidence of publication bias (missing small negative studies, industry sponsorship patterns) | Downgrade one or two levels (-1 or -2) |

---

## Three Domains for Upgrading (Observational Studies)

These domains can upgrade observational evidence (starting at low) but are NOT commonly applied. Each must have strong justification.

### Upgrade 1: Large Magnitude of Effect

| Magnitude | Threshold | Action |
|-----------|-----------|--------|
| Large effect | RR > 2 or RR < 0.5 based on consistent evidence, no plausible confounders | Upgrade one level (+1) |
| Very large effect | RR > 5 or RR < 0.2 based on direct evidence, no major threats to validity | Upgrade two levels (+2) |

**Example:** Smoking and lung cancer (RR > 10), hip replacement for end-stage osteoarthritis.

**Caution:** The effect must be consistent across studies, and plausible confounders would reduce (not increase) the observed effect.

### Upgrade 2: Dose-Response Gradient

If a clear dose-response gradient exists (more exposure leads to proportionally more effect), upgrade one level (+1).

**Requirements:**

- At least three exposure levels.
- Monotonic (or biologically plausible non-monotonic) relationship.
- Consistent across studies.
- No threshold effect that explains away confounding.

### Upgrade 3: Plausible Confounding Would Reduce Effect

If all plausible residual confounders would act in the direction of reducing the observed effect (or toward the null), the true effect is likely larger than observed. Upgrade one level (+1).

**Example:** If healthier people are more likely to receive a treatment (healthy user bias), and the treatment still shows harm, the true harm is likely larger than observed.

---

## Certainty Rating Summary

| Level | Definition | Implication |
|-------|-----------|-------------|
| **High** | Very confident the true effect lies close to the estimate | Further research is very unlikely to change confidence in the estimate |
| **Moderate** | Moderately confident; the true effect is likely close to the estimate, but there is a possibility that it is substantially different | Further research is likely to have an important impact on confidence and may change the estimate |
| **Low** | Limited confidence; the true effect may be substantially different from the estimate | Further research is very likely to have an important impact on confidence and is likely to change the estimate |
| **Very low** | Very little confidence; the true effect is likely substantially different from the estimate | Any estimate of effect is very uncertain |

---

## Summary of Findings Table

A GRADE Summary of Findings (SoF) table presents, for each outcome:

```
| Outcome | No. of participants (studies) | Certainty (GRADE) | Relative effect (95% CI) | Absolute effect (95% CI) |
|---------|------------------------------|-------------------|-------------------------|-------------------------|
| Mortality | 2400 (5 RCTs) | Moderate (downgraded for imprecision) | RR 0.85 (0.71 to 1.02) | 23 fewer per 1000 (from 45 fewer to 3 more) |
```

**Key elements:**

- List the most important outcomes (typically 7 or fewer).
- Include both benefits and harms.
- Report relative AND absolute effects.
- State the certainty rating with footnotes explaining each downgrade/upgrade.
- Use standardized footnote format: "Downgraded one level for [domain]: [specific reason]."

---

## GRADE Evidence Profile

For transparency, produce an evidence profile that shows the rating for each domain:

```
| Outcome | No. studies | Risk of bias | Inconsistency | Indirectness | Imprecision | Publication bias | Upgrading | Overall certainty |
|---------|-------------|-------------|---------------|-------------|------------|-----------------|-----------|-------------------|
| Mortality | 5 RCTs | Not serious | Not serious | Not serious | Serious (-1) | None detected | None | Moderate |
```

---

## Common GRADE Errors

| Error | Problem | Correction |
|-------|---------|------------|
| Downgrading per study instead of per body | GRADE rates the body of evidence, not individual studies | Assess across all studies contributing to the outcome |
| Using I-squared alone for inconsistency | I-squared is a proportion, not a clinically meaningful measure | Also consider direction of effects, prediction interval, and subgroup explanations |
| Not specifying the MID for imprecision | Cannot assess imprecision without knowing what difference matters | Define the MID a priori based on clinical judgment or anchor-based methods |
| Confusing GRADE certainty with study quality | A well-conducted study can contribute to low certainty evidence | Certainty is about the body of evidence for a specific outcome |
| Upgrading RCT evidence | Upgrading domains apply only to observational evidence | RCTs start at high; they can only be downgraded |
| Double-counting risk of bias | Assessing risk of bias in GRADE and then also in the forest plot | Use risk of bias assessment once; GRADE synthesizes it |

---

## Verification Checklist

Before finalizing GRADE assessments:

- [ ] Starting certainty correctly assigned (high for RCTs, low for observational)
- [ ] All five downgrading domains assessed for each outcome
- [ ] Risk of bias assessment uses a validated tool (RoB 2, ROBINS-I)
- [ ] Imprecision assessed against a pre-specified MID
- [ ] Inconsistency considers direction of effects, not just I-squared
- [ ] Indirectness assessed for all PICO elements
- [ ] Publication bias assessed with funnel plots (if >= 10 studies) and search comprehensiveness
- [ ] Upgrading domains only applied to observational evidence with strong justification
- [ ] Summary of Findings table includes absolute and relative effects
- [ ] Footnotes explain each rating decision
- [ ] Certainty labels match definitions (high/moderate/low/very low)
