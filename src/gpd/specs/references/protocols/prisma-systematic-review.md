---
load_when:
  - "systematic review"
  - "PRISMA"
  - "literature search"
  - "study selection"
  - "flow diagram"
  - "reporting guideline"
  - "meta-analysis reporting"
  - "scoping review"
  - "evidence synthesis"
  - "inclusion criteria"
  - "exclusion criteria"
tier: 1
context_cost: high
---

# PRISMA 2020 Systematic Review Protocol

PRISMA (Preferred Reporting Items for Systematic reviews and Meta-Analyses) 2020 is the updated reporting guideline for systematic reviews. It replaced PRISMA 2009 with expanded guidance covering advances in evidence synthesis methodology. Every systematic review must be reported according to PRISMA 2020 to ensure transparency, completeness, and reproducibility.

**Core principle:** A systematic review is only as credible as its reporting. If methods cannot be scrutinized, results cannot be trusted.

## Related Protocols

- `search-strategy.md` -- Detailed search syntax for PubMed, Cochrane, Embase
- `grade-certainty.md` -- Assessing certainty of the body of evidence
- `data-extraction.md` -- Standardized extraction forms and missing data handling
- `rob2-assessment.md` -- Risk of bias assessment for included RCTs
- `prospero-registration.md` -- Protocol registration before conduct

---

## PRISMA 2020 Checklist: 27 Items with Guidance

### Title

**Item 1 -- Title:** Identify the report as a systematic review.

- Use "systematic review" in the title explicitly.
- If meta-analysis was conducted, include "and meta-analysis."
- If the review is an update, state "updated systematic review."
- Example: "Efficacy of cognitive behavioral therapy for insomnia in adults: a systematic review and meta-analysis."

### Abstract

**Item 2 -- Abstract:** Provide a structured abstract including background, objectives, data sources, study eligibility criteria, participants, interventions, study appraisal and synthesis methods, results, limitations, conclusions, and registration information.

- Follow the journal's structured abstract format.
- Include the number of studies and participants.
- Report the main quantitative result with confidence interval.
- State the PROSPERO registration number or DOI of the protocol.

### Introduction

**Item 3 -- Rationale:** Describe the rationale for the review in the context of existing knowledge.

- Cite previous systematic reviews on the topic and explain why an update or new review is needed.
- Identify gaps in the evidence base.
- Describe the clinical or public health importance of the question.

**Item 4 -- Objectives:** Provide an explicit statement of the questions being addressed with reference to PICO.

- Structure objectives using the PICO framework (see `pico-framework.md`).
- Specify whether the objective is to estimate an effect, explore heterogeneity, or evaluate reporting quality.
- Example: "To estimate the effect of metformin (I) compared with sulfonylureas (C) on HbA1c reduction (O) in adults with type 2 diabetes (P)."

### Methods

**Item 5 -- Eligibility criteria:** Specify the inclusion and exclusion criteria for the review, and how studies were grouped for synthesis.

- Define PICO elements as eligibility criteria.
- Specify study designs eligible (RCTs only, or also observational).
- Define time frame, language restrictions, and publication status.
- If post-hoc changes were made to eligibility criteria, report and justify them.

**Item 6 -- Information sources:** Specify all databases, registers, websites, organisations, reference lists, and other sources searched or consulted to identify studies. Specify the date when each source was last searched or consulted.

- List all databases searched (e.g., MEDLINE via PubMed, Embase via Ovid, CENTRAL).
- Include grey literature sources (ClinicalTrials.gov, WHO ICTRP, conference abstracts).
- Report the exact date of each search.
- Describe any contact with study authors or experts.

**Item 7 -- Search strategy:** Present the full search strategies for all databases, registers, and websites, including any filters and limits used.

- Provide the complete search string for at least one database.
- Ideally provide full strategies for all databases in an appendix.
- Report use of validated search filters (e.g., Cochrane Highly Sensitive Search Strategy for RCTs).
- See `search-strategy.md` for detailed guidance on Boolean operators and MeSH terms.

**Item 8 -- Selection process:** Specify the methods used to decide whether a study met the inclusion criteria, including how many reviewers screened each record and each full-text report, whether they worked independently, and any automation tools used.

- Describe the two-stage process: title/abstract screening followed by full-text review.
- State the minimum number of independent reviewers at each stage (minimum 2).
- Describe the process for resolving disagreements (discussion, third reviewer).
- Report any use of machine learning or automation in screening (e.g., Rayyan, ASReview).
- Report inter-rater reliability (e.g., Cohen's kappa) if calculated.

**Item 9 -- Data collection process:** Specify the methods used to collect data from reports, including how many reviewers collected data from each report, whether they worked independently, any processes for obtaining or confirming data from study investigators, and any automation tools used.

- Describe the standardized data extraction form (see `data-extraction.md`).
- Specify pilot testing of the form on a subset of studies.
- Describe duplicate independent extraction and discrepancy resolution.

**Item 10a -- Data items (outcomes):** List and define all outcomes for which data were sought, specifying whether planned or post-hoc. For each outcome, state the time points and metrics.

- For each outcome: definition, measurement tool, time point(s), metric (e.g., mean difference, risk ratio).
- Distinguish primary and secondary outcomes.
- Report any prioritization among multiple time points.

**Item 10b -- Data items (other variables):** List and define all other variables for which data were sought (e.g., participant characteristics, funding sources).

- Include variables needed for subgroup analyses and assessment of bias.
- Describe how effect modifiers were selected a priori.

**Item 11 -- Study risk of bias assessment:** Specify the methods used to assess risk of bias of included studies, including which tool(s) were used, how many reviewers assessed each study, and whether they worked independently.

- Specify the tool used: RoB 2 for RCTs (see `rob2-assessment.md`), ROBINS-I for non-randomized studies (see `robins-i-assessment.md`), or Newcastle-Ottawa Scale (see `newcastle-ottawa.md`).
- Describe domain-level and overall judgments.
- State whether the assessment was outcome-specific.

**Item 12 -- Effect measures:** Specify for each outcome the effect measure(s) used in the synthesis or presentation of results.

- Binary outcomes: risk ratio (RR), odds ratio (OR), risk difference (RD).
- Continuous outcomes: mean difference (MD), standardized mean difference (SMD).
- Time-to-event: hazard ratio (HR).
- Justify the choice of effect measure.

**Item 13a -- Synthesis methods (eligibility):** Describe the processes used to decide which studies were eligible for each synthesis.

- Report the minimum number of studies required for meta-analysis.
- Describe how studies were grouped (by intervention, comparator, outcome, study design).

**Item 13b -- Synthesis methods (tabulation):** Describe any methods required to prepare the data for synthesis, such as handling of multi-arm studies and missing summary statistics.

- Describe imputation methods for missing standard deviations (e.g., from confidence intervals, p-values, or contacting authors).
- Describe conversion of medians to means if used (Wan et al. or Luo et al. methods).
- Describe handling of multi-arm trials (which comparisons included, adjustment for correlation).

**Item 13c -- Synthesis methods (statistical model):** Describe any methods used to synthesize results and provide a rationale for the choice(s).

- State the statistical model: fixed-effect (common-effect) or random-effects.
- Justify the choice: random-effects when clinical or methodological heterogeneity is expected.
- Specify the estimation method: DerSimonian-Laird, REML, Paule-Mandel, or Hartung-Knapp-Sidik-Jonkman.
- Report the software used (see `statistical-analysis.md`).

**Item 13d -- Synthesis methods (heterogeneity):** Describe any methods used to explore possible causes of heterogeneity.

- Report subgroup analyses (pre-specified, not data-dredged).
- Report meta-regression if conducted (minimum 10 studies per covariate recommended).
- Describe sensitivity analyses (e.g., excluding high risk of bias studies, leave-one-out).

**Item 13e -- Synthesis methods (robustness):** Describe any sensitivity analyses conducted to assess robustness of the synthesized results.

- Describe the specific sensitivity analyses conducted and their rationale.

**Item 13f -- Synthesis methods (certainty):** Describe any methods used to assess certainty (or confidence) in the body of evidence for an outcome.

- State use of the GRADE approach (see `grade-certainty.md`).
- Describe how Summary of Findings tables were constructed.

**Item 14 -- Reporting bias assessment:** Describe any methods used to assess risk of bias due to missing results in the synthesis.

- Describe assessment for publication bias: funnel plot, Egger's test, or more advanced methods.
- Describe assessment for selective outcome reporting within studies.
- Minimum 10 studies recommended for funnel plot assessment.

### Results

**Item 15 -- Study selection:** Describe the results of the search and the process for selecting studies, ideally with a flow diagram.

- Include the PRISMA 2020 flow diagram (updated version with separate sections for databases/registers and other sources).
- Report numbers at each stage: identified, screened, assessed for eligibility, included.
- Report reasons for exclusion at the full-text stage.

**Item 16 -- Study characteristics:** Cite each included study and present its characteristics.

- Use a characteristics of included studies table.
- Include: study design, country, setting, sample size, participant demographics, intervention details, comparator, outcomes measured, follow-up duration, funding.

**Item 17 -- Risk of bias in studies:** Present assessments of risk of bias for each included study.

- Present domain-level judgments (e.g., traffic light plots for RoB 2).
- Present summary risk of bias figures (weighted bar charts).
- Describe the impact of risk of bias on the synthesis.

**Item 18 -- Results of individual studies:** For all outcomes, present for each study (a) summary statistics for each group and (b) an effect estimate and its precision.

- Include forest plots for meta-analyzed outcomes.
- For studies not included in meta-analysis, present results narratively.

**Item 19 -- Results of syntheses:** For each synthesis, briefly summarise the characteristics and risk of bias among contributing studies; present results of all statistical syntheses conducted; if meta-analysis was done, present the summary estimate with CI and measures of heterogeneity.

- Report: pooled effect estimate, 95% CI, I-squared, tau-squared, prediction interval.
- If narrative synthesis, follow SWiM guidelines (see `narrative-synthesis.md`).

**Item 20 -- Reporting biases:** Present assessments of risk of bias due to missing results.

- Present funnel plots if generated.
- Report statistical test results (Egger's, Peters', etc.).

**Item 21 -- Certainty of evidence:** Present assessments of certainty in the body of evidence for each outcome assessed.

- Present GRADE Summary of Findings tables.
- State the overall certainty rating (high, moderate, low, very low) and justification.

### Discussion

**Item 22 -- Discussion:** Provide a general interpretation of the results in the context of other evidence.

- Discuss how findings relate to the existing evidence base.
- Discuss potential biases and limitations at the review level.
- Discuss applicability/generalizability.

### Other Information

**Item 23 -- Registration and protocol:** Provide registration information, including register name and registration number, or state where the protocol can be accessed.

- Provide the PROSPERO registration number.
- Cite the published protocol if applicable.
- Describe any deviations from the registered protocol.

**Item 24 -- Support:** Describe sources of financial or non-financial support, and the role of funders.

**Item 25 -- Competing interests:** Declare any competing interests of review authors.

**Item 26 -- Availability of data, code, and other materials:** Report which of the following are publicly available and where: template data collection forms, data extracted from studies, data used for analyses, analytic code, other materials.

**Item 27 -- Other information:** Report any other information relevant to the review not covered elsewhere.

---

## PRISMA 2020 Flow Diagram

The updated flow diagram has four sections:

```
Identification:
  Records identified from databases (n = )
  Records identified from registers (n = )
  Records identified from other sources (n = )
      ↓
  Records removed before screening:
    Duplicate records removed (n = )
    Records marked as ineligible by automation tools (n = )
    Records removed for other reasons (n = )
      ↓
Screening:
  Records screened (n = )
  Records excluded (n = )
      ↓
  Reports sought for retrieval (n = )
  Reports not retrieved (n = )
      ↓
  Reports assessed for eligibility (n = )
  Reports excluded, with reasons (n = )
      ↓
Included:
  Studies included in review (n = )
  Reports of included studies (n = )
  Studies included in quantitative synthesis (n = )
```

---

## Common Errors in PRISMA Reporting

| Error | Problem | Correction |
|-------|---------|------------|
| Missing full search strategy | Cannot reproduce the review | Include complete strategy for all databases |
| No flow diagram | Cannot assess study selection | Always include the PRISMA 2020 flow diagram |
| No protocol registration | Suspicion of post-hoc changes | Register on PROSPERO before starting |
| Confusing studies with reports | One study may have multiple publications | Track both; report studies included, not reports |
| No assessment of certainty | Cannot judge confidence in findings | Use GRADE for each critical outcome |
| Missing reasons for exclusion | Cannot assess selection bias | List reasons at full-text stage with counts |

---

## Verification Checklist

Before finalizing a systematic review report:

- [ ] Title includes "systematic review" (and "meta-analysis" if applicable)
- [ ] Abstract is structured and includes key quantitative results
- [ ] PICO question is explicitly stated
- [ ] All 27 PRISMA items addressed or justified if not applicable
- [ ] Full search strategy provided for all databases
- [ ] PRISMA 2020 flow diagram included with correct numbers
- [ ] Risk of bias assessment conducted and reported
- [ ] GRADE certainty assessment completed for each outcome
- [ ] Protocol registered and deviations reported
- [ ] Data availability statement included
