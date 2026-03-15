---
load_when:
  - "observational study"
  - "cohort study"
  - "case-control"
  - "cross-sectional"
  - "STROBE"
  - "epidemiological study"
  - "exposure"
  - "confounding"
  - "observational reporting"
tier: 1
context_cost: high
---

# STROBE Checklist for Observational Studies

STROBE (Strengthening the Reporting of Observational Studies in Epidemiology) provides a checklist of 22 items for reporting cohort, case-control, and cross-sectional studies. Unlike RCTs, observational studies cannot rely on randomization to control confounding, making transparent reporting of design decisions and analytical choices especially critical.

**Core principle:** Observational studies are vulnerable to confounding, selection bias, and information bias. STROBE ensures these threats are documented so readers can judge the validity of causal inferences.

## Related Protocols

- `robins-i-assessment.md` -- Risk of bias in non-randomised studies (ROBINS-I)
- `newcastle-ottawa.md` -- Quality assessment with Newcastle-Ottawa Scale
- `causal-inference.md` -- DAGs, propensity scores, instrumental variables
- `pico-framework.md` -- PECO (Population, Exposure, Comparator, Outcome) for observational studies

---

## Study Design Characteristics

| Design | Direction | Sampling | Key Measure | When to Use |
|--------|-----------|----------|-------------|-------------|
| **Cohort** | Forward (exposure to outcome) | By exposure status | Relative risk, incidence rate ratio | Common exposures, multiple outcomes |
| **Case-control** | Backward (outcome to exposure) | By outcome status | Odds ratio | Rare outcomes, single outcome |
| **Cross-sectional** | Snapshot at one time point | Entire population or sample | Prevalence ratio, prevalence OR | Prevalence estimation, hypothesis generation |
| **Nested case-control** | Case-control within a cohort | Cases + matched controls from cohort | Odds ratio (approximates HR) | Expensive measurements within large cohort |

---

## STROBE Checklist: 22 Items with Guidance

### Title and Abstract

**Item 1 -- Title and abstract:** (a) Indicate the study's design with a commonly used term in the title or abstract. (b) Provide an informative and balanced summary.

- Use "cohort study," "case-control study," or "cross-sectional study" explicitly.
- Abstract should include: objective, design, setting, participants, exposure/outcome definitions, main results with measures of association and precision, conclusions.

### Introduction

**Item 2 -- Background/rationale:** Explain the scientific background and rationale for the investigation being reported.

- Describe the current state of knowledge, including prior systematic reviews.
- Explain why an observational design was chosen over an experimental design (or why a trial is not feasible/ethical).

**Item 3 -- Objectives:** State specific objectives, including any pre-specified hypotheses.

- Use PECO (Population, Exposure, Comparator, Outcome) for observational studies.
- Distinguish confirmatory (pre-specified hypothesis) from exploratory analyses.

### Methods

**Item 4 -- Study design:** Present key elements of study design early in the paper.

- State the study design and justify the choice.
- For cohort studies: prospective or retrospective? Open or closed cohort?
- For case-control: population-based or hospital-based? Matched or unmatched?
- For cross-sectional: single time point or repeated cross-sections?

**Item 5 -- Setting:** Describe the setting, locations, and relevant dates, including periods of recruitment, exposure, follow-up, and data collection.

- Report the calendar period.
- Describe the source population from which participants were drawn.
- Report any lag time between exposure assessment and outcome ascertainment.

**Item 6 -- Participants:**

- (a) Cohort: Give the eligibility criteria, sources and methods of selection. Describe methods of follow-up.
- (a) Case-control: Give eligibility criteria, sources and methods of case ascertainment and control selection. Give the rationale for the choice of cases and controls.
- (a) Cross-sectional: Give eligibility criteria, sources and methods of selection.
- (b) Cohort: For matched studies, give matching criteria and number of exposed and unexposed.
- (b) Case-control: For matched studies, give matching criteria and the number of controls per case.

**Item 7 -- Variables:** Clearly define all outcomes, exposures, predictors, potential confounders, and effect modifiers. Give diagnostic criteria.

- Report the operational definition of the exposure (how measured, by whom, when).
- Report the operational definition of the outcome (diagnostic criteria, ICD codes, adjudication process).
- List all potential confounders identified a priori (preferably using a DAG, see `causal-inference.md`).
- Distinguish confounders from mediators -- adjusting for mediators removes part of the causal effect.

**Item 8 -- Data sources/measurement:** For each variable of interest, give sources of data and details of methods of assessment (measurement). Describe comparability of assessment methods if there is more than one group.

- Report the measurement instruments used (validated questionnaires, laboratory assays, administrative codes).
- Report inter-rater and intra-rater reliability if applicable.
- For case-control studies: was exposure measurement identical for cases and controls? (recall bias concern).

**Item 9 -- Bias:** Describe any efforts to address potential sources of bias.

- Address confounding: list adjustment variables and the rationale (ideally DAG-based).
- Address selection bias: describe how the study population relates to the target population.
- Address information bias: describe misclassification potential (differential vs. non-differential).
- Address reverse causation: describe temporal relationship between exposure and outcome.

**Item 10 -- Study size:** Explain how the study size was arrived at.

- Report sample size calculation if performed (effect size, alpha, power, variance).
- If no formal calculation: report the available sample and discuss whether the study was adequately powered.
- For case-control: report the case-to-control ratio and rationale.

**Item 11 -- Quantitative variables:** Explain how quantitative variables were handled in the analyses. If applicable, describe which groupings were chosen and why.

- Report how continuous exposures were modeled: linear, categorical (quartiles, clinical cutpoints), splines, fractional polynomials.
- If categorized: justify the cutpoints (clinical guidelines, distribution-based, prior literature).
- Avoid arbitrary dichotomization of continuous variables (loss of information and power).

**Item 12 -- Statistical methods:**

- (a) Describe all statistical methods, including those used to control for confounding.
- (b) Describe any methods used to examine subgroups and interactions.
- (c) Explain how missing data were addressed.
- (d) Cohort: If applicable, explain how loss to follow-up was addressed. Case-control: If applicable, explain how matching of cases and controls was addressed. Cross-sectional: If applicable, describe analytical methods taking account of sampling strategy.
- (e) Describe any sensitivity analyses.

Methods for confounding control:

| Method | When to Use | Limitation |
|--------|-------------|------------|
| Multivariable regression | Standard approach, sufficient events per variable | Linearity assumption, residual confounding |
| Propensity score matching | Many confounders, treatment comparisons | Unmeasured confounding remains |
| Inverse probability weighting | Time-varying confounding | Extreme weights, positivity violations |
| Instrumental variables | Unmeasured confounding | Requires valid instrument |
| Difference-in-differences | Policy changes, natural experiments | Parallel trends assumption |

### Results

**Item 13 -- Participants:**

- (a) Report numbers at each stage: potentially eligible, examined for eligibility, confirmed eligible, included in study, completed follow-up, analyzed.
- (b) Give reasons for non-participation at each stage.
- (c) Consider use of a flow diagram.

**Item 14 -- Descriptive data:**

- (a) Give characteristics of study participants and information on exposures and potential confounders.
- (b) Indicate number of participants with missing data for each variable of interest.
- (c) Cohort: Summarise follow-up time (e.g., average and total amount).

**Item 15 -- Outcome data:**

- Cohort: Report numbers of outcome events or summary measures over time.
- Case-control: Report numbers in each exposure category, or summary measures of exposure.
- Cross-sectional: Report numbers of outcome events or summary measures.

**Item 16 -- Main results:**

- (a) Give unadjusted estimates and, if applicable, confounder-adjusted estimates and their precision. Make clear which confounders were adjusted for and why they were included.
- (b) Report category boundaries when continuous variables were categorized.
- (c) If relevant, consider translating estimates of relative risk into absolute risk for a meaningful time period.

**Item 17 -- Other analyses:** Report other analyses done, e.g., analyses of subgroups and interactions, and sensitivity analyses.

- Distinguish pre-specified from post-hoc analyses.
- Report E-values for sensitivity to unmeasured confounding.
- Report negative control analyses if conducted.

### Discussion

**Item 18 -- Key results:** Summarise key results with reference to study objectives.

**Item 19 -- Limitations:** Discuss limitations of the study, taking into account sources of potential bias or imprecision. Discuss both direction and magnitude of any potential bias.

- Address residual confounding explicitly.
- Discuss potential selection bias and information bias.
- Quantify the impact of missing data through sensitivity analyses.

**Item 20 -- Interpretation:** Give a cautious overall interpretation of results considering objectives, limitations, multiplicity of analyses, results from similar studies, and other relevant evidence.

- Avoid causal language unless the design supports it (see `causal-inference.md`).
- Use "associated with" rather than "caused" unless strong causal inference methods were used.
- Discuss the Bradford Hill criteria if making a causal argument.

**Item 21 -- Generalisability:** Discuss the generalisability (external validity) of the study results.

**Item 22 -- Funding:** Give the source of funding and the role of the funders.

---

## Design-Specific Considerations

### Cohort Studies

- **Immortal time bias:** Time between cohort entry and treatment initiation during which the outcome cannot occur. Solution: time-dependent exposure classification or exclude the immortal person-time.
- **Healthy worker effect:** Working populations are healthier than general populations. Solution: use appropriate reference group.
- **Loss to follow-up:** If differential by exposure status, biases the result. Report completeness of follow-up and compare those lost versus retained.

### Case-Control Studies

- **Recall bias:** Cases may recall exposures differently than controls. Solution: use objective exposure data (medical records, biomarkers) when possible.
- **Selection of controls:** Controls should represent the source population that gave rise to the cases. Hospital-based controls may have different exposure patterns than population-based controls.
- **Matching:** Matching on a confounder requires conditional analysis (conditional logistic regression). Matching on a non-confounder reduces efficiency.
- **Odds ratio interpretation:** In rare disease settings (< 10% incidence), OR approximates RR. For common outcomes, OR overestimates RR.

### Cross-Sectional Studies

- **Temporal ambiguity:** Cannot establish temporal sequence between exposure and outcome. Interpret cautiously.
- **Prevalence vs incidence:** Prevalence is affected by both incidence and duration. Prevalent cases overrepresent chronic/slowly resolving conditions.
- **Neyman bias:** If the exposure affects survival, prevalent cases may not represent incident cases.

---

## Verification Checklist

Before finalizing an observational study report:

- [ ] Study design named in the title or abstract
- [ ] PECO question explicitly stated
- [ ] Eligibility criteria fully described
- [ ] Exposure and outcome operationally defined
- [ ] Confounders identified a priori (ideally via DAG) and adjustment strategy described
- [ ] Both unadjusted and adjusted estimates reported with 95% CI
- [ ] Missing data quantified and handling described
- [ ] Flow diagram included showing participant flow
- [ ] Sensitivity analyses addressing unmeasured confounding reported
- [ ] Causal language avoided unless supported by design and analysis
- [ ] Design-specific biases (immortal time, recall, selection) addressed
