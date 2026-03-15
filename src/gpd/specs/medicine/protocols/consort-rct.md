---
load_when:
  - "randomized controlled trial"
  - "RCT"
  - "CONSORT"
  - "clinical trial reporting"
  - "randomization"
  - "blinding"
  - "allocation concealment"
  - "intention to treat"
  - "per protocol"
  - "trial registration"
tier: 1
context_cost: high
---

# CONSORT 2010 Reporting Guideline for Randomized Controlled Trials

CONSORT (Consolidated Standards of Reporting Trials) 2010 is the minimum standard for reporting parallel-group RCTs. Extensions exist for cluster-randomized, crossover, non-inferiority, pragmatic, and other trial designs. This protocol covers the core 25-item checklist with detailed guidance for each item.

**Core principle:** An RCT that is poorly reported cannot be distinguished from a poorly conducted trial. CONSORT ensures readers can assess what was actually done.

## Related Protocols

- `rob2-assessment.md` -- Risk of bias assessment using the Cochrane RoB 2 tool (maps directly to CONSORT items)
- `statistical-analysis.md` -- Statistical methods for trial analysis
- `pico-framework.md` -- Framing the research question

---

## CONSORT 2010 Checklist

### Title and Abstract

**Item 1a -- Title:** Identification as a randomised trial in the title.

- Use "randomised" (or "randomized") explicitly.
- If cluster-randomised or crossover, state the design.

**Item 1b -- Abstract:** Structured summary of trial design, methods, results, and conclusions.

- Include: objectives, trial design, setting, participants, interventions, main outcomes, randomization, blinding, numbers analyzed, key results with effect sizes and confidence intervals, harms, conclusions, trial registration number, funding source.

### Introduction

**Item 2a -- Background:** Scientific background and explanation of rationale.

- Summarize the existing evidence, including previous systematic reviews.
- Explain why this trial is needed given the current evidence base.

**Item 2b -- Objectives:** Specific objectives or hypotheses.

- State primary and secondary objectives.
- Formulate using PICO structure.
- Pre-specify the direction of expected effect if the hypothesis is directional.

### Methods

**Item 3a -- Trial design:** Description of trial design (parallel, factorial, etc.) including allocation ratio.

- State the design: parallel group, factorial, crossover, cluster.
- Report the allocation ratio (e.g., 1:1, 2:1).
- If the design changed after trial commencement, report and justify.

**Item 3b -- Changes to methods:** Important changes to methods after trial commencement, with reasons.

- Report any changes to eligibility criteria, outcomes, or analyses after trial began.
- Explain the reason (e.g., slow recruitment, interim analysis).
- Clearly distinguish pre-specified from post-hoc analyses.

**Item 4a -- Participants (eligibility):** Eligibility criteria for participants.

- List all inclusion and exclusion criteria.
- Be specific enough to replicate participant selection.
- Include diagnostic criteria used (e.g., DSM-5 for mental health, specific biomarker thresholds).

**Item 4b -- Participants (settings):** Settings and locations where data were collected.

- Report the type of setting (primary care, hospital, community).
- Report geographic locations and number of centers.
- Report the period of recruitment and follow-up.

**Item 5 -- Interventions:** The interventions for each group with sufficient details to allow replication, including how and when they were actually administered.

- For drug interventions: name, dose, route, frequency, duration, manufacturer.
- For complex interventions: use TIDieR checklist (Template for Intervention Description and Replication).
- Describe the comparator in equal detail (placebo, active control, usual care).
- Report adherence to the intervention.

**Item 6a -- Outcomes (pre-specified):** Completely defined pre-specified primary and secondary outcome measures, including how and when they were assessed.

- State the primary outcome (only one recommended).
- For each outcome: definition, measurement instrument (with psychometric properties if applicable), time point(s), assessor.
- If a composite outcome, list all components.

**Item 6b -- Outcomes (changes):** Any changes to trial outcomes after the trial commenced, with reasons.

- Report any change in primary outcome, addition or removal of secondary outcomes.
- Distinguish from the registered protocol.

**Item 7a -- Sample size:** How sample size was determined.

- Report: target difference (clinically important difference), alpha, power, variance estimate, and resulting sample size.
- Report the source of the variance estimate (pilot data, previous study).
- Report adjustments for dropout, non-compliance, or clustering.
- If sample size was not formally calculated, state this explicitly.

**Item 7b -- Interim analyses:** When applicable, explanation of any interim analyses and stopping guidelines.

- Report the stopping rules (O'Brien-Fleming, Lan-DeMets spending function).
- Report whether a Data Safety Monitoring Board was used.
- Report whether the trial was stopped early and why.

**Item 8a -- Randomisation sequence generation:** Method used to generate the random allocation sequence.

- Specify the method: computer-generated random numbers, random number table, minimization.
- If stratified, list the stratification factors.
- If block randomised, report block sizes (variable block sizes preferred to prevent prediction).

**Item 8b -- Randomisation type:** Type of randomisation; details of any restriction.

- Simple, blocked, stratified, adaptive, minimization.
- If blocked: variable or fixed block sizes.

**Item 9 -- Allocation concealment:** Mechanism used to implement the random allocation sequence, describing steps taken to conceal the sequence until interventions were assigned.

- Describe the concealment method: central telephone system, web-based system, sequentially numbered opaque sealed envelopes.
- This is distinct from blinding -- allocation concealment prevents foreknowledge of assignment, blinding prevents knowledge after assignment.
- Inadequate concealment (e.g., alternation, birth date, open random number tables) biases enrollment.

**Item 10 -- Implementation:** Who generated the random allocation sequence, who enrolled participants, and who assigned participants to interventions.

- These should be different people to prevent selection bias.
- If the same person performed multiple roles, explain why and what safeguards were in place.

**Item 11a -- Blinding:** If done, who was blinded after assignment (participants, care providers, outcome assessors) and how.

- Describe the blinding method: identical placebo, sham procedure, matched packaging.
- If blinding was not possible (e.g., surgical vs. medical intervention), state this and explain what steps were taken to minimize bias (blinded outcome assessment).
- Report success of blinding if assessed (e.g., James blinding index, Bang blinding index).

**Item 11b -- Blinding (similarity):** If relevant, description of the similarity of interventions.

- Describe how placebo or sham matched the active intervention (appearance, taste, smell, administration route).

**Item 12a -- Statistical methods (primary):** Statistical methods used to compare groups for primary and secondary outcomes.

- Specify the statistical test or model for each outcome.
- Report the analysis population: intention-to-treat (ITT), modified ITT, per-protocol.
- Report handling of missing data: complete case analysis, multiple imputation, mixed models.
- For the primary analysis, use ITT as the default. Per-protocol analysis is supportive.

**Item 12b -- Statistical methods (additional):** Methods for additional analyses, such as subgroup analyses and adjusted analyses.

- Report pre-specified subgroup analyses and the test for interaction (not separate subgroup p-values).
- Report any adjusted analyses and the rationale for adjustment variables.
- Clearly label exploratory (post-hoc) analyses.

### Results

**Item 13a -- Participant flow:** For each group, the numbers of participants randomly assigned, receiving intended treatment, and analysed for the primary outcome.

- Include the CONSORT flow diagram.
- Report numbers at each stage: assessed for eligibility, excluded (with reasons), randomised, allocated, received intervention, lost to follow-up (with reasons), analysed.

**Item 13b -- Losses and exclusions:** For each group, losses and exclusions after randomisation, together with reasons.

- Report timing and reasons for discontinuation.
- Report differential loss between groups (a red flag for bias).

**Item 14a -- Recruitment:** Dates defining the periods of recruitment and follow-up.

- Report first enrollment date, last enrollment date, and date of last follow-up.

**Item 14b -- Trial end:** Why the trial ended or was stopped.

- If stopped early: report the reason (futility, efficacy, harm, administrative).

**Item 15 -- Baseline data:** A table showing baseline demographic and clinical characteristics for each group.

- Report the Table 1 with means (SD) or medians (IQR) for continuous variables and counts (%) for categorical variables.
- Do NOT perform statistical tests comparing baseline characteristics between randomized groups. Any imbalance is due to chance and p-values are meaningless here.
- Important variables: age, sex, disease severity, comorbidities, concomitant medications.

**Item 16 -- Numbers analysed:** For each group, number of participants included in each analysis and whether the analysis was by original assigned groups.

- State the analysis population for each outcome.
- If ITT was modified (e.g., excluding participants who never received any treatment), justify.

**Item 17a -- Outcomes and estimation:** For each primary and secondary outcome, results for each group, and the estimated effect size and its precision (such as 95% confidence interval).

- Report both the point estimate and 95% CI.
- For binary outcomes: events/total in each group + RR or OR with CI.
- For continuous outcomes: mean (SD) in each group + mean difference with CI.
- For time-to-event: HR with CI and Kaplan-Meier curves.
- Report absolute effects, not just relative effects.

**Item 17b -- Binary outcomes:** For binary outcomes, presentation of both absolute and relative effect sizes is recommended.

- Report NNT (number needed to treat) or NNH (number needed to harm) when appropriate.

**Item 18 -- Ancillary analyses:** Results of any other analyses performed, including subgroup analyses and adjusted analyses, distinguishing pre-specified from exploratory.

- Label each analysis as pre-specified or post-hoc.
- For subgroup analyses, report the interaction p-value, not within-subgroup p-values.

**Item 19 -- Harms:** All important harms or unintended effects in each group.

- Report adverse events by group using a structured table.
- Report serious adverse events separately.
- Do not limit to "statistically significant" adverse events; report all important ones.
- Consider using the CONSORT extension for harms.

### Discussion

**Item 20 -- Limitations:** Trial limitations, addressing sources of potential bias, imprecision, and multiplicity of analyses.

- Address internal validity (bias) and external validity (generalizability).
- Discuss the impact of missing data.
- If the trial was underpowered for a specific outcome, state this explicitly.

**Item 21 -- Generalisability:** Generalisability of the trial findings.

- Discuss how the trial population compares to the target population.
- Discuss setting-specific factors that might limit applicability.

**Item 22 -- Interpretation:** Interpretation consistent with results, balancing benefits and harms, and considering other relevant evidence.

- Do not overstate findings beyond what the data support.
- Place findings in context of the broader evidence base.
- If non-significant: discuss as "no evidence of a difference," not "no difference."

### Other Information

**Item 23 -- Registration:** Registration number and name of trial registry.

- ClinicalTrials.gov, ISRCTN, or WHO ICTRP partner registry.
- Must be registered before enrollment of the first participant.

**Item 24 -- Protocol:** Where the full trial protocol can be accessed.

- Cite the published protocol or provide a URL.

**Item 25 -- Funding:** Sources of funding and other support, role of funders.

- Describe the funder's role in design, conduct, analysis, and reporting.

---

## CONSORT Flow Diagram

```
Assessed for eligibility (n = )
  ↓
Excluded (n = )
  Not meeting inclusion criteria (n = )
  Declined to participate (n = )
  Other reasons (n = )
  ↓
Randomised (n = )
  ├── Allocated to intervention (n = )     ├── Allocated to control (n = )
  │   Received intervention (n = )         │   Received control (n = )
  │   Did not receive (n = , reasons)      │   Did not receive (n = , reasons)
  │                                         │
  │   Lost to follow-up (n = , reasons)    │   Lost to follow-up (n = , reasons)
  │   Discontinued (n = , reasons)         │   Discontinued (n = , reasons)
  │                                         │
  │   Analysed (n = )                      │   Analysed (n = )
  │   Excluded from analysis (n = , why)   │   Excluded from analysis (n = , why)
```

---

## Key Methodological Concepts

### Intention-to-Treat vs Per-Protocol

| Analysis | Includes | Preserves Randomisation | Bias Direction |
|----------|----------|------------------------|----------------|
| ITT | All randomised participants, as randomised | Yes | Conservative (biases toward null) |
| Per-protocol | Only those who completed treatment as assigned | No | May overestimate efficacy |
| Modified ITT | All randomised who received at least one dose | Partially | Between ITT and per-protocol |

**Default:** ITT for superiority trials. Per-protocol for non-inferiority trials (ITT is anti-conservative for non-inferiority).

### Multiplicity

When testing multiple outcomes or subgroups, the family-wise error rate inflates. Strategies:

- Pre-specify a single primary outcome.
- Use hierarchical testing (gatekeeping) for co-primary outcomes.
- Adjust alpha for multiple comparisons (Bonferroni, Holm, Hochberg).
- Frame secondary outcomes as hypothesis-generating.

---

## Verification Checklist

Before finalizing an RCT report:

- [ ] "Randomised" appears in the title
- [ ] CONSORT flow diagram present with numbers at every stage
- [ ] Randomisation method, allocation concealment, and blinding fully described
- [ ] Primary outcome pre-specified and analysis population stated
- [ ] Effect estimates reported with 95% CI for all outcomes
- [ ] Both absolute and relative effects reported for binary outcomes
- [ ] Harms reported by group
- [ ] Trial registered and registration number provided
- [ ] Baseline table present without between-group p-values
- [ ] ITT analysis as primary (or justified deviation)
- [ ] Subgroup analyses report interaction tests, not within-group p-values
