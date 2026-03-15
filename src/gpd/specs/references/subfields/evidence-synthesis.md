---
load_when:
  - "systematic review"
  - "meta-analysis"
  - "evidence synthesis"
  - "PRISMA"
  - "forest plot"
  - "heterogeneity"
  - "network meta-analysis"
  - "umbrella review"
  - "Cochrane"
  - "GRADE"
tier: 2
context_cost: medium
---

# Evidence Synthesis

## Core Concepts and Terminology

**Systematic Reviews:**

- A systematic review applies a pre-specified, reproducible search strategy to identify, appraise, and synthesize all relevant studies addressing a focused question
- Protocol registration: PROSPERO (CRD), OSF, or Cochrane; must be registered before starting
- PICO(S) framework: Population, Intervention, Comparator, Outcome, Study design — defines the review question
- Eligibility criteria: explicit inclusion/exclusion criteria applied consistently
- Search strategy: minimum two databases (MEDLINE + Cochrane CENTRAL); additional sources depending on topic
- Grey literature: conference abstracts, dissertations, regulatory documents, trial registries — reduces publication bias

**Meta-Analysis:**

- Statistical pooling of results from multiple independent studies
- Fixed-effect model: assumes one true effect size; all studies estimate the same parameter; weighted by inverse variance
- Random-effects model: assumes true effect sizes vary across studies; incorporates between-study variance (tau-squared); DerSimonian-Laird, REML, Paule-Mandel, or Hartung-Knapp-Sidik-Jonkman estimators
- Effect measures: risk ratio (RR), odds ratio (OR), risk difference (RD), mean difference (MD), standardized mean difference (SMD / Cohen's d / Hedges' g), hazard ratio (HR)
- SMD conventions: 0.2 small, 0.5 medium, 0.8 large (Cohen); Hedges' g corrects for small-sample bias
- Inverse variance weighting: w_i = 1/var_i (fixed-effect); w_i = 1/(var_i + tau^2) (random-effects)

**Heterogeneity:**

- **I-squared (I2):** Proportion of total variability due to between-study heterogeneity; 25% low, 50% moderate, 75% high — BUT depends on precision of included studies; not an absolute measure
- **Tau-squared (tau2):** Absolute between-study variance; more informative than I2 for clinical interpretation
- **Q statistic (Cochran's Q):** Chi-squared test for heterogeneity; low power with few studies
- **Prediction interval:** Range within which the effect of a future study would fall; much wider than confidence interval; should always be reported alongside pooled estimate
- **Sources:** Clinical heterogeneity (populations, interventions), methodological heterogeneity (study design, risk of bias), statistical heterogeneity (I2, tau2)

**Publication Bias:**

- Studies with positive/significant results are more likely to be published
- Detection: funnel plot (visual), Egger's regression test (continuous outcomes), Peters' test (binary outcomes), Harbord's test (OR), contour-enhanced funnel plot
- Adjustment: trim-and-fill (Duval & Tweedie), selection models (Copas, Vevea-Hedges), PET-PEESE, p-curve, p-uniform
- Small-study effects: not always publication bias; may reflect genuine differences in study populations or interventions
- Funnel plot asymmetry: unreliable with <10 studies

**Risk of Bias Assessment:**

- **RCTs:** Cochrane Risk of Bias tool (RoB 2.0) — 5 domains: randomization, deviations from intended interventions, missing outcome data, outcome measurement, selective reporting
- **Observational studies:** ROBINS-I (7 domains); Newcastle-Ottawa Scale (selection, comparability, outcome/exposure)
- **Diagnostic studies:** QUADAS-2 (patient selection, index test, reference standard, flow and timing)
- **Prognostic studies:** QUIPS (study participation, attrition, prognostic factor measurement, outcome measurement, confounding, statistical analysis)

## Key Research Methodologies

**Network Meta-Analysis (NMA):**

- Compares multiple interventions simultaneously by combining direct and indirect evidence
- Geometry: treatment network with nodes (treatments) and edges (direct comparisons)
- Transitivity assumption: distribution of effect modifiers is similar across comparisons
- Consistency assumption: direct and indirect evidence agree; test with node-splitting or design-by-treatment interaction
- Models: frequentist (graph-theoretical, multivariate meta-analysis) or Bayesian (Markov chain Monte Carlo)
- Output: league table of all pairwise comparisons, ranking (SUCRA, P-scores, mean rank), forest plot of NMA estimates
- CINeMA: Confidence In Network Meta-Analysis framework for certainty assessment

**Umbrella Reviews (Reviews of Reviews):**

- Synthesize existing systematic reviews/meta-analyses on a topic
- Do NOT re-extract primary study data (unless conducting an overlapping meta-analysis)
- Address overlap: Corrected Covered Area (CCA) to quantify primary study overlap across reviews
- Quality assessment: AMSTAR 2 (A MeaSurement Tool to Assess systematic Reviews)
- Useful for: mapping evidence across related interventions/outcomes, identifying evidence gaps

**Scoping Reviews:**

- Map available evidence on a broad topic; identify knowledge gaps
- Do NOT typically include meta-analysis or formal risk of bias assessment
- PRISMA-ScR reporting guideline
- Useful for: emerging fields, heterogeneous literature, informing future systematic review questions

**Living Systematic Reviews:**

- Continuously updated as new evidence becomes available
- Require: ongoing search, pre-specified update triggers, cumulative meta-analysis with appropriate alpha spending
- Published by Cochrane and others for rapidly evolving topics (e.g., COVID-19 treatments)

**Individual Participant Data (IPD) Meta-Analysis:**

- Obtains raw data from each included study
- Advantages: standardized analysis, subgroup analysis at individual level, time-to-event modeling
- Two-stage: analyze each study, then pool (preserves clustering)
- One-stage: single mixed model with study as random effect
- Challenges: data sharing, harmonization, missing data handling

## Common Study Designs

| Type | What it pools | Key method | Reporting guideline |
|------|---------------|------------|---------------------|
| Pairwise meta-analysis | Two treatments, direct comparison | Inverse-variance, DerSimonian-Laird | PRISMA |
| Network meta-analysis | Multiple treatments, direct + indirect | Bayesian MCMC, frequentist graph-theory | PRISMA-NMA |
| Umbrella review | Existing systematic reviews | AMSTAR 2, overlap matrices | Umbrella-specific PRISMA |
| Dose-response meta-analysis | Multiple dose levels | Restricted cubic splines, one-stage | PRISMA |
| Diagnostic test accuracy | Sensitivity/specificity | Bivariate/HSROC model | PRISMA-DTA |
| Prognostic factor | Associations with outcomes | Two-stage with extraction challenges | PRISMA-P (being developed) |
| IPD meta-analysis | Individual-level data | One-stage or two-stage mixed models | PRISMA-IPD |

## Important Databases and Registries

| Resource | Purpose | Access |
|----------|---------|--------|
| **Cochrane Library** | Gold-standard systematic reviews | cochranelibrary.com |
| **PROSPERO** | Systematic review protocol registration | crd.york.ac.uk/prospero |
| **PubMed / MEDLINE** | Biomedical literature; use Cochrane Highly Sensitive Search Strategy for RCTs | pubmed.ncbi.nlm.nih.gov |
| **Embase** | Broader than MEDLINE; more European/pharma content | embase.com |
| **CENTRAL** | Cochrane Central Register of Controlled Trials | cochranelibrary.com |
| **CINAHL** | Nursing and allied health literature | ebsco.com/products/research-databases/cinahl |
| **Web of Science / Scopus** | Citation tracking, grey literature | webofscience.com / scopus.com |
| **OpenGrey / Grey Literature Report** | Unpublished studies | variable availability |
| **GRADE Working Group** | Evidence certainty framework | gradeworkinggroup.org |

## Key Journals

- **Journal of Clinical Epidemiology** — evidence synthesis methodology
- **Research Synthesis Methods** — statistical methods for meta-analysis
- **Systematic Reviews** — open-access; reviews and protocols
- **BMJ Evidence-Based Medicine** — EBM methodology
- **Cochrane Database of Systematic Reviews** — the Cochrane reviews themselves
- **The Lancet** — high-profile meta-analyses
- **JAMA** — methodological articles, major reviews
- **BMJ** — evidence synthesis, clinical guidelines
- **Annals of Internal Medicine** — ACP clinical guidelines incorporating evidence synthesis

## Verification Considerations

**Search Completeness:**

- Check: at least two databases searched; ideally MEDLINE + Embase + CENTRAL
- Check: search strategy reported in full (appendix); validated by librarian/information specialist
- Check: reference lists of included studies screened
- Check: grey literature and trial registries searched
- Check: no language restrictions unless justified

**Data Extraction Accuracy:**

- Check: dual extraction (two reviewers independently) with discrepancy resolution
- Check: effect measure correctly extracted (OR vs RR vs HR; intention-to-treat vs per-protocol)
- Check: variance/SE correctly derived (from CI, p-value, or event counts)
- Check: time-to-event data correctly handled (HR, not OR of events at fixed time)

**Meta-Analysis Validity:**

- Check: appropriate model choice (fixed vs random effects); random-effects nearly always preferred in clinical research
- Check: heterogeneity assessed and explored (I2, tau2, prediction interval, subgroup/meta-regression)
- Check: publication bias assessed if >=10 studies (funnel plot + statistical test)
- Check: sensitivity analyses performed (leave-one-out, excluding high-risk-of-bias studies, alternative effect measures)
- Check: GRADE certainty assessed for each outcome

**NMA-Specific:**

- Check: transitivity assumption discussed and justified
- Check: consistency assessed (node-splitting, design-by-treatment)
- Check: network geometry adequate (no fragile connections based on single small study)
- Check: ranking interpreted with uncertainty (not just point estimates of SUCRA/P-score)

## Common Pitfalls and LLM Error Risks

- **Confusing fixed-effect and random-effects models:** LLMs may not understand when each is appropriate or may confuse the interpretation of their results
- **Misinterpreting I-squared:** I2 is NOT the percentage of variation that is "real" — it is the proportion of total variability due to heterogeneity. LLMs often provide incorrect interpretations
- **Ignoring prediction intervals:** The pooled estimate with CI tells you about the average effect; the prediction interval tells you about the range of effects in individual settings. LLMs almost never report prediction intervals
- **Fabricating Cochrane review conclusions:** LLMs generate plausible Cochrane review summaries that do not exist. Always verify against the actual Cochrane Library
- **Misunderstanding GRADE:** LLMs may present GRADE as a simple scoring system rather than a structured judgment framework requiring domain expertise
- **Conflating narrative and systematic reviews:** A narrative review is NOT a systematic review. LLMs may use these terms interchangeably
- **Ignoring the direction of effect in forest plots:** LLMs may misread which side favors treatment vs control
- **Incorrect variance derivation:** Converting between SE, CI, and p-value requires specific formulas that LLMs sometimes apply incorrectly
- **Overcounting in umbrella reviews:** The same primary study may appear in multiple included reviews, inflating apparent evidence. LLMs rarely flag this overlap issue
- **Misapplying NMA ranking:** SUCRA/P-score rankings without uncertainty intervals are misleading; LLMs tend to present rankings as definitive

## Methodology Decision Tree

```
What is the synthesis question?
├── How effective is intervention A vs B?
│   ├── Enough RCTs (>=2)? → Pairwise meta-analysis
│   ├── Mix of RCTs and observational? → Separate analyses, compare
│   └── Only observational studies? → Pool with caution, GRADE down for study design
├── Which of many interventions is best?
│   ├── Connected network of trials? → Network meta-analysis
│   └── Disconnected network? → Cannot do NMA; component NMA or pairwise only
├── What is already known (existing reviews)?
│   ├── Multiple reviews exist? → Umbrella review
│   └── Few reviews, heterogeneous? → New systematic review
├── What evidence exists (broad scope)?
│   ├── Emerging field? → Scoping review
│   └── Well-defined question? → Systematic review
├── Diagnostic test accuracy?
│   └── → HSROC or bivariate model; PRISMA-DTA
├── Dose-response?
│   └── → Dose-response meta-analysis with splines
└── Rapidly evolving evidence?
    └── → Living systematic review
```

## Software and Tools

| Tool | Purpose | Access |
|------|---------|--------|
| **RevMan (Review Manager)** | Cochrane review authoring, basic meta-analysis | training.cochrane.org/online-learning/core-software/revman |
| **R (meta, metafor, netmeta, gemtc)** | Comprehensive meta-analysis in R | cran.r-project.org |
| **Stata (metan, network)** | Meta-analysis and NMA commands | stata.com |
| **CMA (Comprehensive Meta-Analysis)** | Commercial GUI-based meta-analysis | meta-analysis.com |
| **BUGS/JAGS/Stan** | Bayesian NMA and complex models | various |
| **GRADEpro GDT** | GRADE evidence profiles and SoF tables | gradepro.org |
| **Covidence** | Screening and data extraction platform | covidence.org |
| **Rayyan** | Free screening tool | rayyan.ai |
| **ROB 2 / ROBINS-I** | Risk of bias assessment tools | riskofbias.info |

## Research Frontiers (2024-2026)

| Frontier | Key question | GMD suitability |
|----------|-------------|-----------------|
| **AI-assisted screening and extraction** | Can LLMs reliably screen titles/abstracts and extract data? | Excellent — methodology validation |
| **Component NMA** | Decomposing complex interventions into components | Good — statistical methodology |
| **Living evidence ecosystems** | Automated search, continuous update, dynamic guidelines | Good — infrastructure and methodology |
| **Individual participant data sharing** | Overcoming barriers to IPD meta-analysis | Moderate — policy and methodology |
| **Certainty of evidence in NMA** | CINeMA framework refinement | Good — methodological evaluation |
| **Handling non-randomized evidence in NMA** | Integrating RCTs and observational studies | Good — methodology development |
