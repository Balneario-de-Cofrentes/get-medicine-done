---
load_when:
  - "Newcastle-Ottawa"
  - "NOS"
  - "cohort quality"
  - "case-control quality"
  - "observational study quality"
  - "star rating"
  - "study quality assessment"
tier: 2
context_cost: low
---

# Newcastle-Ottawa Scale for Observational Studies

The Newcastle-Ottawa Scale (NOS) is a widely used tool for assessing the quality of non-randomised studies (cohort and case-control) in systematic reviews. It uses a "star system" to evaluate three broad domains: selection, comparability, and outcome (cohort) or exposure (case-control). While simpler than ROBINS-I, it is acceptable for systematic reviews where a rapid assessment is needed and is recommended by Cochrane for some review types.

**Core principle:** The NOS evaluates methodological quality, not reporting quality. A study can be well-reported but methodologically weak, or poorly reported but methodologically sound.

## Related Protocols

- `robins-i-assessment.md` -- More detailed bias assessment tool (preferred for Cochrane reviews)
- `strobe-observational.md` -- Reporting guideline (complements NOS quality assessment)
- `grade-certainty.md` -- NOS scores contribute to GRADE risk of bias domain

---

## When to Use NOS vs ROBINS-I

| Criterion | NOS | ROBINS-I |
|-----------|-----|----------|
| Complexity | Simple, quick | Detailed, time-intensive |
| Output | Star score (max 9) | Judgment per domain (low to critical) |
| Domain coverage | 3 broad domains | 7 specific domains |
| Confounding assessment | Single comparability item | Detailed with signalling questions |
| Cochrane recommendation | Acceptable | Preferred |
| Time per study | 10-15 minutes | 30-60 minutes |
| Best for | Rapid reviews, large number of studies | Cochrane reviews, GRADE assessments |

---

## NOS for Cohort Studies (Maximum 9 Stars)

### Selection (Maximum 4 Stars)

**1. Representativeness of the exposed cohort**

| Criterion | Stars |
|-----------|-------|
| Truly representative of the average population in the community (describe the population) | 1 star |
| Somewhat representative of the average population | 1 star |
| Selected group of users (e.g., nurses, volunteers) | 0 stars |
| No description of the derivation of the cohort | 0 stars |

**Guidance:** A cohort drawn from a defined population (e.g., national health registry, census-based sample) is representative. A cohort of volunteers or a specific occupational group is less representative. No star if the source population is not described.

**2. Selection of the non-exposed cohort**

| Criterion | Stars |
|-----------|-------|
| Drawn from the same community as the exposed cohort | 1 star |
| Drawn from a different source | 0 stars |
| No description of the derivation of the non-exposed cohort | 0 stars |

**Guidance:** The non-exposed group should come from the same source population as the exposed group. Different source populations introduce selection bias. Best practice: the same database or registry.

**3. Ascertainment of exposure**

| Criterion | Stars |
|-----------|-------|
| Secure record (e.g., surgical records, prescription database) | 1 star |
| Structured interview | 1 star |
| Written self-report | 0 stars |
| No description | 0 stars |

**Guidance:** Objective, prospectively recorded exposure data are best. Self-reported exposure is susceptible to recall bias and misclassification.

**4. Demonstration that outcome of interest was not present at start of study**

| Criterion | Stars |
|-----------|-------|
| Yes (described or evident from the design) | 1 star |
| No or not stated | 0 stars |

**Guidance:** For incidence studies, participants with prevalent outcomes must be excluded at baseline. Without this, prevalent cases contaminate the incidence estimate.

### Comparability (Maximum 2 Stars)

**5. Comparability of cohorts on the basis of the design or analysis**

This is the most important item. Award up to 2 stars based on whether the study controls for confounders:

| Criterion | Stars |
|-----------|-------|
| Study controls for the most important factor (specify) | 1 star |
| Study controls for any additional important factor (specify) | 1 star |
| Study does not control for important factors | 0 stars |

**Guidance:**

- Before assessment, the review team should define which confounders are "most important" and which are "additional important" for the specific research question.
- "Most important" typically includes the strongest confounder (e.g., age for many diseases, smoking for lung cancer studies).
- "Additional important" includes the next most critical confounders.
- Control can be through design (matching, restriction) or analysis (regression, stratification, propensity scores).

### Outcome (Maximum 3 Stars)

**6. Assessment of outcome**

| Criterion | Stars |
|-----------|-------|
| Independent blind assessment | 1 star |
| Record linkage (e.g., hospital discharge records, death certificates) | 1 star |
| Self-report | 0 stars |
| No description | 0 stars |

**7. Was follow-up long enough for outcomes to occur?**

| Criterion | Stars |
|-----------|-------|
| Yes (select an adequate follow-up period for outcome of interest) | 1 star |
| No | 0 stars |

**Guidance:** The adequate duration depends on the outcome. For cancer outcomes, years of follow-up may be needed. For acute events, shorter follow-up may suffice. The review team should specify the minimum adequate follow-up duration before assessment.

**8. Adequacy of follow-up of cohorts**

| Criterion | Stars |
|-----------|-------|
| Complete follow-up: all participants accounted for | 1 star |
| Participants lost to follow-up unlikely to introduce bias (< 20% lost, or description of those lost suggesting no systematic difference) | 1 star |
| Follow-up rate < 80% and no description of those lost | 0 stars |
| No statement | 0 stars |

---

## NOS for Case-Control Studies (Maximum 9 Stars)

### Selection (Maximum 4 Stars)

**1. Is the case definition adequate?**

| Criterion | Stars |
|-----------|-------|
| Yes, with independent validation (e.g., ICD codes confirmed by chart review) | 1 star |
| Yes, e.g., record linkage or based on self-reports | 0 stars |
| No description | 0 stars |

**2. Representativeness of the cases**

| Criterion | Stars |
|-----------|-------|
| Consecutive or obviously representative series of cases | 1 star |
| Potential for selection biases or not stated | 0 stars |

**3. Selection of controls**

| Criterion | Stars |
|-----------|-------|
| Community controls (same population as cases) | 1 star |
| Hospital controls | 0 stars |
| No description | 0 stars |

**Guidance:** Hospital controls may have different exposure patterns than the general population (Berkson's bias). Community controls from the same source population are preferred.

**4. Definition of controls**

| Criterion | Stars |
|-----------|-------|
| No history of disease (endpoint) | 1 star |
| No description of source | 0 stars |

### Comparability (Maximum 2 Stars)

**5. Comparability of cases and controls on the basis of the design or analysis**

Same as for cohort studies: up to 2 stars for controlling important confounders.

### Exposure (Maximum 3 Stars)

**6. Ascertainment of exposure**

| Criterion | Stars |
|-----------|-------|
| Secure record (e.g., surgical records) | 1 star |
| Structured interview where blind to case/control status | 1 star |
| Interview not blinded to case/control status | 0 stars |
| Written self-report or medical record review (unblinded) | 0 stars |
| No description | 0 stars |

**7. Same method of ascertainment for cases and controls**

| Criterion | Stars |
|-----------|-------|
| Yes | 1 star |
| No | 0 stars |

**8. Non-response rate**

| Criterion | Stars |
|-----------|-------|
| Same rate for both groups | 1 star |
| Non-respondents described | 0 stars |
| Rate different and no designation | 0 stars |

---

## Interpreting NOS Scores

There is no universally agreed-upon threshold for "high quality" vs "low quality." Common conventions:

| Score Range | Conventional Interpretation |
|-------------|---------------------------|
| 7-9 stars | High quality |
| 4-6 stars | Moderate quality |
| 0-3 stars | Low quality |

**Caution with thresholds:**

- These cutpoints are arbitrary and not empirically validated.
- A study with 7 stars that lacks comparability control (0/2 on comparability) is fundamentally different from a 7-star study with full comparability.
- Consider presenting domain-level scores alongside total scores.
- Some review teams use NOS scores as continuous variables in meta-regression rather than as categorical quality thresholds.

---

## Modifications and Extensions

### Modified NOS

Many systematic reviews modify the NOS for their specific question. Common modifications:

- Specifying the exact confounders required for comparability stars.
- Defining minimum follow-up duration.
- Adjusting exposure ascertainment criteria for the specific exposure.
- Adding items for specific bias concerns (e.g., immortal time bias).

**When modifying:** Describe and justify all modifications in the systematic review protocol. Pre-specify modifications before study assessment.

### Cross-Sectional Studies

The original NOS does not cover cross-sectional studies. Adaptations have been developed (e.g., by Herzog et al. 2013) but are less standardized. For cross-sectional studies, consider using the JBI (Joanna Briggs Institute) critical appraisal checklist instead.

---

## Presenting NOS Results

### Table Format

```
| Study | Selection (4) | Comparability (2) | Outcome/Exposure (3) | Total (9) |
|-------|--------------|-------------------|---------------------|-----------|
| Smith 2020 | **** | ** | ** | 8 |
| Jones 2019 | *** | * | *** | 7 |
| Lee 2021 | ** | - | ** | 4 |
```

### Using NOS in Meta-Analysis

- **Sensitivity analysis:** Exclude studies below a quality threshold and compare the pooled estimate.
- **Subgroup analysis:** Stratify by quality level (high vs. moderate vs. low).
- **Meta-regression:** Use NOS total score or domain scores as covariates.
- **Do not use NOS scores as weights in meta-analysis** -- this approach is not recommended by Cochrane.

---

## Limitations of NOS

| Limitation | Implication |
|-----------|-------------|
| Inter-rater reliability can be low | Use two independent assessors; calculate and report kappa |
| Scoring is crude (yes/no) | Cannot capture gradations of quality within items |
| Equal weighting of items | Comparability (confounding control) is arguably more important than representativeness |
| No explicit confounding assessment | Does not use DAGs or systematic confounder identification |
| Total score combines domains that should not be combined | A study with high selection but zero comparability is different from one with moderate scores across all domains |
| No formal algorithm for overall judgment | Relies on arbitrary thresholds |

**Recommendation:** Use ROBINS-I for the primary assessment if resources allow. Use NOS as a supplementary or rapid assessment tool. Always report domain-level scores, not just totals.

---

## Verification Checklist

Before finalizing NOS assessments:

- [ ] Correct NOS form used (cohort vs. case-control)
- [ ] Pre-specified confounders for comparability domain documented in protocol
- [ ] Pre-specified adequate follow-up duration documented (cohort studies)
- [ ] Two independent assessors with kappa reported for inter-rater agreement
- [ ] Domain-level scores presented alongside total scores
- [ ] Modifications to NOS documented and justified
- [ ] NOS scores integrated into sensitivity or subgroup analysis
- [ ] Limitations of the NOS approach acknowledged in the review
