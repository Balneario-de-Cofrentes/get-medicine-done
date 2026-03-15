---
load_when:
  - "epidemiology"
  - "cohort study"
  - "case-control"
  - "incidence"
  - "prevalence"
  - "odds ratio"
  - "relative risk"
  - "confounding"
  - "surveillance"
  - "outbreak"
  - "STROBE"
tier: 2
context_cost: medium
---

# Epidemiology

## Core Concepts and Terminology

**Measures of Disease Frequency:**

- **Incidence rate (person-time):** Number of new cases / total person-time at risk; units are per person-year (or 1000 person-years, etc.)
- **Cumulative incidence (risk):** Number of new cases / population at risk at start of period; dimensionless proportion (0 to 1)
- **Prevalence:** Number of existing cases / total population at a point in time (point prevalence) or over a period (period prevalence)
- **Relationship:** Prevalence = Incidence x Duration (for steady-state conditions)
- **Attack rate:** Cumulative incidence during an outbreak (misnomer: it is a proportion, not a rate)

**Measures of Association:**

- **Relative risk (RR):** Risk in exposed / Risk in unexposed; from cohort studies; RR=1 means no association
- **Odds ratio (OR):** Odds of exposure in cases / Odds of exposure in controls; approximates RR when disease is rare (<10%)
- **Hazard ratio (HR):** Instantaneous rate ratio from Cox proportional hazards model; assumes proportional hazards
- **Risk difference (RD):** Absolute difference in risk; RD = Risk_exposed - Risk_unexposed
- **Number needed to treat (NNT):** 1 / |RD|; number of patients to treat to prevent one event
- **Population attributable fraction (PAF):** Proportion of cases attributable to exposure; PAF = p_e(RR-1) / (1 + p_e(RR-1)) where p_e is exposure prevalence

**Bias:**

- **Selection bias:** Systematic differences in who is included in the study; includes Berkson's bias (hospital-based), volunteer bias, loss to follow-up bias, immortal time bias
- **Information bias:** Systematic errors in measurement; includes recall bias, interviewer bias, misclassification (differential vs non-differential)
- **Confounding:** A third variable associated with both exposure and outcome, not on the causal pathway; control via restriction, matching, stratification, multivariable adjustment, or randomization
- **Effect modification (interaction):** The effect of exposure differs across levels of a third variable; should be reported, not "controlled for"
- **Immortal time bias:** Person-time during which the outcome cannot occur is misclassified; leads to apparently protective effects of exposure

**Causal Inference:**

- **Bradford Hill criteria (1965):** Strength, consistency, specificity, temporality, biological gradient, plausibility, coherence, experiment, analogy — guidelines, not checklist
- **Directed Acyclic Graphs (DAGs):** Formal representation of causal assumptions; identifies confounders, mediators, colliders
- **Collider bias:** Conditioning on a collider (common effect of two variables) creates spurious association
- **Instrumental variables:** Variable affecting exposure but not outcome except through exposure; Mendelian randomization uses genetic variants as instruments
- **Target trial emulation:** Framework for designing observational analyses to mimic a hypothetical RCT

## Key Research Methodologies

**Cohort Studies:**

- Prospective: enroll disease-free, follow forward in time; gold standard for incidence
- Retrospective: use existing records to reconstruct cohort and follow-up
- Advantages: temporal sequence, multiple outcomes, incidence calculation
- Disadvantages: expensive, slow, loss to follow-up, not feasible for rare diseases
- Analysis: Kaplan-Meier survival curves, Cox regression, Poisson regression for rates, propensity score methods

**Case-Control Studies:**

- Start with cases (disease) and controls (no disease); look back at exposure
- Selection of controls is critical: should represent the population giving rise to cases
- Matching: frequency or individual; analyzed with conditional logistic regression
- Nested case-control: within a defined cohort; more efficient than full cohort analysis
- Case-cohort: subcohort serves as controls for multiple outcomes
- Case-crossover: each case serves as own control; for transient exposures triggering acute events

**Cross-Sectional Studies:**

- Snapshot: exposure and outcome measured simultaneously
- Cannot establish temporal sequence (except for time-invariant exposures like genetics)
- Yields prevalence, not incidence; prevalence OR, not RR
- Useful for: health surveys, needs assessments, disease burden estimation

**Ecological Studies:**

- Unit of analysis is populations, not individuals
- Ecological fallacy: associations at group level may not hold at individual level
- Useful for: hypothesis generation, policy evaluation, geographic comparisons
- Examples: country-level correlations between diet and disease rates

**Outbreak Investigation:**

- Steps: verify diagnosis, confirm outbreak, define case, count cases, describe by person/place/time, develop hypotheses, test hypotheses, implement control, communicate
- Epidemic curve: histogram of case counts over time; shape suggests source type (point, continuous, propagated)
- Attack rate tables: exposure-specific attack rates for hypothesis testing
- Analytical studies: cohort (if defined population) or case-control (if population undefined)

## Common Study Designs

| Design | Direction | Measure | Best for |
|--------|-----------|---------|----------|
| Prospective cohort | Forward | RR, HR, incidence | Common exposures, multiple outcomes |
| Retrospective cohort | Backward then forward | RR, HR | Occupational, registry-based |
| Case-control | Backward | OR | Rare diseases, outbreak investigation |
| Nested case-control | Within cohort | OR approximating RR | Biomarker studies within cohorts |
| Cross-sectional | Snapshot | Prevalence, prevalence OR | Disease burden, health surveys |
| Ecological | Population-level | Correlation | Hypothesis generation |
| Case-crossover | Self-matched | OR | Transient exposures, acute events |

## Important Databases and Registries

| Database | Scope | Access |
|----------|-------|--------|
| **WHO Global Health Observatory** | Global health statistics | who.int/data/gho |
| **CDC WONDER** | US mortality, natality, cancer, environmental data | wonder.cdc.gov |
| **IHME Global Burden of Disease** | Disease burden estimates worldwide | healthdata.org/gbd |
| **UK Biobank** | 500K prospective cohort; genetics, imaging, lifestyle | ukbiobank.ac.uk |
| **Framingham Heart Study** | Cardiovascular epidemiology since 1948 | framinghamheartstudy.org |
| **NHANES** | US population health surveys | cdc.gov/nchs/nhanes |
| **EPIC (European Prospective Investigation into Cancer)** | Cancer and nutrition cohort | epic.iarc.fr |
| **SEER** | US cancer incidence and survival | seer.cancer.gov |
| **OpenSAFELY** | UK electronic health records for research | opensafely.org |
| **Clinical Practice Research Datalink (CPRD)** | UK primary care records | cprd.com |

## Key Journals

- **American Journal of Epidemiology** — foundational epidemiology
- **International Journal of Epidemiology** — global epi, methods
- **Epidemiology** — causal inference, methods
- **European Journal of Epidemiology** — European epi research
- **Journal of Clinical Epidemiology** — clinical epi methodology
- **Annals of Epidemiology** — applied epidemiology
- **Emerging Infectious Diseases** — outbreak and surveillance
- **The Lancet Public Health** — population-level studies
- **BMJ** — observational studies, EBM

## Verification Considerations

**Causal Diagram Review:**

- Check: is a DAG specified? Does it encode the authors' causal assumptions?
- Check: are confounders identified from the DAG, not just "traditional" adjustments?
- Check: are colliders avoided in the adjustment set?
- Check: is there risk of M-bias, butterfly bias, or overadjustment?

**Bias Assessment:**

- Use ROBINS-I (Risk Of Bias In Non-randomized Studies of Interventions) for intervention studies
- Use Newcastle-Ottawa Scale for cohort and case-control studies
- Check: is immortal time bias possible? Was it addressed?
- Check: is information bias assessed (sensitivity analysis for misclassification)?

**Statistical Analysis Verification:**

- Check: is the measure of association appropriate for the study design (RR for cohort, OR for case-control)?
- Check: are confidence intervals reported alongside p-values?
- Check: is the model specification appropriate (logistic, Cox, Poisson)?
- Check: are assumptions tested (proportional hazards for Cox, linearity for continuous variables)?

**STROBE Compliance:**

- Check: participant flow clearly described (eligible, enrolled, analyzed)
- Check: missing data reported and handling method described
- Check: sensitivity analyses for key assumptions

## Common Pitfalls and LLM Error Risks

- **Conflating OR and RR:** LLMs frequently interpret odds ratios as if they were relative risks. OR approximates RR only when disease prevalence is <10%
- **Ignoring immortal time bias:** LLMs may fail to identify this bias in observational studies, leading to erroneous conclusions about protective effects
- **Ecological fallacy:** LLMs may draw individual-level conclusions from ecological data (e.g., country-level correlations)
- **Confusing incidence and prevalence:** These are fundamentally different measures; LLMs may use them interchangeably
- **Presenting association as causation:** Observational studies establish association, not causation. LLMs may use causal language inappropriately
- **Reverse causation:** LLMs may fail to consider that the outcome could cause the exposure (especially in cross-sectional studies)
- **Fabricating epidemiological statistics:** LLMs generate plausible but fictitious disease rates, prevalences, and relative risks. Always verify against primary data sources
- **Getting Bradford Hill wrong:** LLMs may present the criteria as a checklist for "proving" causation; they are considerations, not a scoring system
- **Misapplying NNT:** NNT depends on baseline risk; LLMs may present a single NNT as universal when it varies by population
- **Collider stratification bias:** LLMs rarely flag this subtlety; conditioning on a common effect of exposure and outcome creates spurious associations

## Methodology Decision Tree

```
What is the research question?
├── Etiology / Risk factor identification
│   ├── Common exposure, rare outcome? → Case-control study
│   ├── Common exposure, common outcome? → Cohort study
│   ├── Transient exposure, acute outcome? → Case-crossover
│   └── Genetic risk factor? → Mendelian randomization
├── Disease frequency / Burden
│   ├── Point-in-time? → Cross-sectional survey
│   ├── Over time? → Surveillance / repeated cross-sections
│   └── Global comparison? → GBD data analysis
├── Prognosis / Natural history
│   ├── Short-term? → Prospective cohort with defined inception point
│   └── Long-term? → Retrospective cohort using registry data
├── Outbreak investigation
│   ├── Defined population? → Cohort analysis with attack rates
│   └── Undefined population? → Case-control study
└── Causal inference from observational data
    ├── Target trial emulation
    ├── Instrumental variables / Mendelian randomization
    └── Difference-in-differences / interrupted time series
```

## Research Frontiers (2024-2026)

| Frontier | Key question | GMD suitability |
|----------|-------------|-----------------|
| **Target trial emulation** | Can observational data reliably mimic RCTs? | Excellent — methodology review |
| **Mendelian randomization advances** | Two-sample MR, multivariable MR, MR-PRESSO for pleiotropy | Good — causal inference analysis |
| **Causal machine learning** | TMLE, causal forests for heterogeneous effects | Good — methodology comparison |
| **Pandemic preparedness epidemiology** | Real-time surveillance, wastewater, genomic epi | Moderate — review and synthesis |
| **Digital epidemiology** | Mobile data, social media, EHR-based studies | Good — methodological assessment |
| **Climate and health** | Heat-related mortality, vector-borne disease shift | Good — ecological and time-series analysis |
