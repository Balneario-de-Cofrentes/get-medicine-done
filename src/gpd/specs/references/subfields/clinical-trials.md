---
load_when:
  - "clinical trial"
  - "randomized controlled trial"
  - "RCT"
  - "phase I"
  - "phase II"
  - "phase III"
  - "phase IV"
  - "adaptive trial"
  - "crossover study"
  - "CONSORT"
  - "intention to treat"
  - "sample size"
tier: 2
context_cost: medium
---

# Clinical Trials

## Core Concepts and Terminology

**Randomization:**

- Simple randomization: coin-flip equivalent; risk of imbalance in small trials
- Block randomization: ensures equal allocation within blocks; block size should vary to prevent prediction
- Stratified randomization: balances key prognostic factors across arms
- Minimization (dynamic allocation): algorithmic balancing of multiple factors; controversial whether it constitutes true randomization
- Cluster randomization: units of randomization are groups (clinics, hospitals); requires intra-cluster correlation coefficient (ICC) in sample size calculation

**Blinding:**

- Open-label: no blinding; used when blinding is impossible (surgery vs medical) or impractical
- Single-blind: participant blinded; assessor unblinded (or vice versa)
- Double-blind: participant and investigator blinded; gold standard for drug trials
- Triple-blind: adds blinding of data analyst / outcome adjudicator
- Sham procedures: for surgical or device trials; ethical debate around sham surgery
- Blinding assessment: James' Blinding Index, Bang's Blinding Index; should be reported

**Analysis Populations:**

- Intention-to-treat (ITT): all randomized patients analyzed in their assigned group regardless of compliance; preserves randomization, avoids selection bias
- Modified ITT (mITT): excludes patients who never received treatment or had no post-baseline data; common but controversial
- Per-protocol (PP): only patients who completed the protocol as planned; biased but answers "what if everyone complied?" question
- As-treated: analyzed by actual treatment received; highly susceptible to confounding
- Safety population: all patients who received at least one dose

**Phase Definitions:**

| Phase | Objective | Typical N | Design | Primary endpoint |
|-------|-----------|-----------|--------|-----------------|
| 0 (Exploratory IND) | Microdosing, PK | 10-15 | Open-label | PK parameters |
| I | Safety, dose-finding | 20-80 | Dose escalation (3+3, CRM) | MTD, DLT, AEs |
| I/II | Safety + preliminary efficacy | 50-100 | Dose-finding + expansion cohort | MTD + ORR |
| II | Efficacy signal, dose selection | 100-300 | Randomized, often single-arm | ORR, PFS |
| III | Confirmatory efficacy | 300-3000+ | Randomized, controlled, often multi-center | OS, PFS, primary efficacy |
| IV (Post-marketing) | Long-term safety, rare AEs | 1000-10000+ | Observational, registry | AE incidence, effectiveness |

**Adaptive Trial Designs:**

- Group sequential: pre-planned interim analyses with alpha spending (O'Brien-Fleming, Lan-DeMets)
- Sample size re-estimation: blinded or unblinded; adjusts N based on observed variance or effect size
- Response-adaptive randomization: shifts allocation toward better-performing arm; Bayesian or frequentist
- Platform trials: master protocol evaluating multiple interventions against shared control (RECOVERY, I-SPY 2)
- Basket trials: one drug, multiple tumor types; Bayesian hierarchical borrowing across baskets
- Umbrella trials: one disease, multiple biomarker-defined subgroups, each with matched therapy
- Seamless phase II/III: single trial transitioning from dose-selection to confirmatory; requires multiplicity control

**Crossover Studies:**

- Each participant receives all treatments in sequence separated by washout periods
- Advantages: within-subject comparison eliminates between-subject variability; smaller N needed
- Risks: carryover effects, period effects, order effects
- Washout period: must exceed 5 half-lives of pharmacological effect
- Analysis: paired comparisons, mixed-effects models with period and sequence terms
- Suitable for: chronic stable conditions (hypertension, pain, asthma); NOT suitable for curative or disease-modifying treatments

## Key Research Methodologies

**Sample Size Calculation:**

- Requires: Type I error (alpha, typically 0.05), power (1-beta, typically 0.80 or 0.90), clinically meaningful difference (delta), expected variance (sigma)
- Continuous outcome: N per arm = 2 * (Z_alpha/2 + Z_beta)^2 * sigma^2 / delta^2
- Binary outcome: N per arm uses arcsine transformation or normal approximation
- Survival: requires expected event rate, accrual period, follow-up period; Schoenfeld formula for log-rank test
- Adjustment factors: dropout rate (inflate by 1/(1-d)), non-compliance, multiplicity, interim analyses
- Cluster trials: multiply by design effect DE = 1 + (m-1)*ICC where m is cluster size

**Multiplicity:**

- Multiple primary endpoints: Bonferroni, Holm, Hochberg, gatekeeping procedures
- Multiple arms: Dunnett (each vs control), Tukey (all pairwise)
- Interim analyses: alpha spending functions (O'Brien-Fleming conservative early, Pocock uniform)
- Subgroup analyses: pre-specified, limited number, interaction test rather than subgroup-specific p-values
- Family-wise error rate (FWER) vs false discovery rate (FDR): FWER for confirmatory; FDR for exploratory

**Non-Inferiority and Equivalence:**

- Non-inferiority margin: largest clinically acceptable loss of efficacy; must be justified clinically and statistically
- Testing: one-sided test; CI upper bound for risk difference (or lower bound for relative effect) must exclude the margin
- Assay sensitivity: must demonstrate the active comparator would have beaten placebo in the current trial setting
- Bio-creep: successive non-inferiority trials can lead to progressive loss of efficacy
- Equivalence: two one-sided tests (TOST); CI must fall entirely within [-delta, +delta]

## Common Study Designs

**Factorial Designs:**

- 2x2 factorial: tests two interventions simultaneously; requires assumption of no interaction
- Efficient: answers two questions for the "price" of one trial (if no interaction)
- Interaction test is underpowered: typically 4x the sample size needed to detect interaction vs main effect
- Fractional factorial: for screening many factors (early-phase, dose-finding)

**N-of-1 Trials:**

- Single patient receives alternating periods of treatment and control
- Multiple crossover periods with randomized sequence
- Best for: chronic stable conditions, symptomatic treatments, patient-centered decisions
- Analysis: paired t-test, time-series, Bayesian estimation
- Aggregation of multiple N-of-1 trials provides population-level evidence

**Pragmatic vs Explanatory Trials:**

- PRECIS-2 tool: 9 domains rated on explanatory-pragmatic continuum
- Explanatory: strict eligibility, standardized intervention, efficacy focus
- Pragmatic: broad eligibility, flexible intervention, effectiveness focus
- Real-world evidence increasingly used to complement traditional RCTs

## Important Databases and Registries

| Resource | Purpose | Access |
|----------|---------|--------|
| **ClinicalTrials.gov** | US trial registry; >450,000 studies | clinicaltrials.gov |
| **EU Clinical Trials Register** | European trials (CTR) | clinicaltrialsregister.eu |
| **ISRCTN** | International registry | isrctn.com |
| **WHO ICTRP** | Meta-registry aggregating national registries | who.int/clinical-trials-registry-platform |
| **FDAAA 801 Tracker** | Monitors results reporting compliance | fdaaa.trialstracker.net |
| **ICH Guidelines** | Harmonized regulatory guidance (E6 GCP, E9 Statistics) | ich.org |
| **CONSORT** | Reporting guideline for RCTs + extensions | consort-statement.org |
| **SPIRIT** | Protocol reporting guideline | spirit-statement.org |

## Key Journals

- **New England Journal of Medicine (NEJM)** — landmark trials
- **The Lancet** — major trials, global health
- **JAMA** — trials across specialties
- **BMJ** — pragmatic trials, methodology
- **Annals of Internal Medicine** — clinical trials, methods
- **Clinical Trials** — trial methodology journal
- **Statistics in Medicine** — statistical methods for trials
- **Controlled Clinical Trials / Contemporary Clinical Trials** — trial design
- **Trials** — open-access trial protocols and methodology

## Verification Considerations

**Protocol Consistency:**

- Check: do the published results match the pre-registered protocol (endpoints, analysis plan, sample size)?
- COMPARE project and COMPare Trials have documented widespread outcome switching
- Check: primary endpoint in registry vs publication; any discrepancies must be explained

**Statistical Analysis Verification:**

- Check: ITT analysis should be primary; PP as sensitivity analysis
- Check: handling of missing data (complete case, LOCF, multiple imputation, mixed models)
- Check: multiplicity adjustment for multiple endpoints or interim analyses
- Check: subgroup analyses should test for interaction, not just within-subgroup significance
- Check: non-inferiority trials analyzed ITT AND per-protocol (both must support conclusion)

**CONSORT Flow Diagram:**

- Check: all randomized patients accounted for through to analysis
- Check: reasons for exclusion, dropout, and loss to follow-up
- Check: numbers in analysis match reported denominators in results tables

**Regulatory and Ethical:**

- Check: trial registered before first patient enrolled (ICMJE requirement)
- Check: ethics committee / IRB approval documented
- Check: data monitoring committee (DMC/DSMB) for phase III trials
- Check: informed consent process described

## Common Pitfalls and LLM Error Risks

- **Confusing ITT with per-protocol:** LLMs may misidentify which population was analyzed, leading to incorrect interpretation of results
- **Fabricating trial registration numbers:** LLMs generate plausible but fictitious NCT numbers. Always verify at ClinicalTrials.gov
- **Misunderstanding non-inferiority:** The logic of non-inferiority is inverted from superiority. LLMs frequently apply superiority logic to non-inferiority conclusions
- **Ignoring multiplicity:** LLMs may present multiple secondary endpoints as if they all have independent statistical significance without multiplicity correction
- **Confusing statistical significance with clinical significance:** A p<0.05 for a trivially small effect size is statistically significant but clinically meaningless
- **Misinterpreting adaptive trial results:** Adaptive designs require pre-specification and alpha control; LLMs may describe ad hoc modifications as "adaptive"
- **Confusing phases:** Phase II results are hypothesis-generating, not confirmatory. LLMs sometimes present phase II data as if it were phase III evidence
- **Overgeneralizing from single trials:** Even well-designed phase III trials need replication. LLMs may present a single trial as definitive evidence
- **Getting CONSORT items wrong:** LLMs may describe CONSORT elements that belong to a different reporting guideline (e.g., STROBE items attributed to CONSORT)
- **Misattributing landmark trials:** LLMs may assign incorrect authors, journals, years, or sample sizes to well-known trials. Always verify trial citations

## Methodology Decision Tree

```
What is the research question?
├── Is this intervention effective (vs placebo/standard)?
│   ├── Drug/biologic? → Phase III double-blind RCT
│   ├── Surgical procedure? → RCT with sham or active comparator
│   ├── Complex intervention? → Pragmatic RCT (PRECIS-2 guided)
│   └── Individual patient? → N-of-1 trial
├── Is this intervention non-inferior to standard?
│   ├── Define margin from historical data → Non-inferiority RCT
│   └── Assay sensitivity? → Include placebo arm if ethical (3-arm)
├── What is the optimal dose?
│   ├── First-in-human? → Phase I dose escalation (3+3 or CRM)
│   └── Dose-response? → Phase II randomized dose-ranging
├── Multiple interventions to screen?
│   ├── Same disease? → Platform trial (adaptive, shared control)
│   ├── Multiple diseases, one drug? → Basket trial
│   └── One disease, biomarker-driven? → Umbrella trial
└── Post-marketing safety?
    ├── Rare events? → Large simple trial or registry
    └── Long-term? → Phase IV observational study
```

## Research Frontiers (2024-2026)

| Frontier | Key question | GMD suitability |
|----------|-------------|-----------------|
| **Decentralized clinical trials (DCT)** | Remote enrollment, telemedicine visits, direct-to-patient drug shipment | Good — protocol design and regulatory analysis |
| **Bayesian adaptive platform trials** | Shared control arms, response-adaptive randomization across many arms | Good — statistical methodology review |
| **Digital biomarkers and endpoints** | Wearable-derived outcomes (gait, sleep, heart rate variability) | Moderate — validation framework development |
| **Synthetic control arms** | External control from real-world data replacing placebo arm | Good — methodology and regulatory acceptance review |
| **Master protocols in rare diseases** | Basket/umbrella/platform designs overcoming small-N problem | Good — statistical and regulatory analysis |
| **AI-assisted trial design** | Machine learning for patient selection, endpoint prediction | Moderate — methodology review and validation |
