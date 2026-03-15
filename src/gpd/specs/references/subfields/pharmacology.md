---
load_when:
  - "pharmacology"
  - "pharmacokinetics"
  - "pharmacodynamics"
  - "PK/PD"
  - "drug interaction"
  - "adverse drug reaction"
  - "therapeutic drug monitoring"
  - "clinical pharmacology"
  - "drug metabolism"
  - "CYP450"
tier: 2
context_cost: medium
---

# Pharmacology

## Core Concepts and Terminology

**Pharmacokinetics (PK) — What the Body Does to the Drug:**

- **Absorption:** Bioavailability (F) = fraction reaching systemic circulation; F=1 for IV; oral F depends on absorption and first-pass metabolism
- **Distribution:** Volume of distribution (Vd); Vd = Dose / Cp0; high Vd = extensive tissue distribution; low Vd = confined to plasma/ECF
- **Metabolism:** Phase I (oxidation, reduction, hydrolysis — primarily CYP450 enzymes) and Phase II (conjugation — glucuronidation, sulfation, acetylation, methylation)
- **Elimination:** Clearance (CL) = rate of elimination / plasma concentration; renal clearance (CLr) and hepatic clearance (CLh)
- **Half-life:** t1/2 = 0.693 * Vd / CL; time for plasma concentration to halve; steady state reached in ~5 half-lives
- **AUC (area under the curve):** Total drug exposure; AUC = Dose * F / CL; key PK parameter for bioequivalence and exposure-response
- **Cmax and Tmax:** Peak concentration and time to peak; relevant for efficacy (Cmax-driven effects) and toxicity

**CYP450 System:**

- **Major enzymes:** CYP3A4 (metabolizes ~50% of drugs), CYP2D6 (20-25%), CYP2C9 (10-15%), CYP2C19 (5%), CYP1A2 (5%)
- **Inhibitors (common):** CYP3A4: ketoconazole, itraconazole, clarithromycin, grapefruit juice; CYP2D6: fluoxetine, paroxetine, quinidine; CYP2C19: omeprazole, fluconazole
- **Inducers (common):** CYP3A4: rifampicin, carbamazepine, phenytoin, St. John's wort; CYP1A2: smoking, omeprazole
- **Pharmacogenomics:** CYP2D6 phenotypes (poor, intermediate, extensive, ultrarapid metabolizers); CYP2C19 (*2, *3 loss-of-function; *17 gain-of-function); impacts clopidogrel, codeine, tamoxifen, many psychotropics

**Pharmacodynamics (PD) — What the Drug Does to the Body:**

- **Dose-response:** Emax (maximal effect), EC50 (concentration for 50% Emax), Hill coefficient (steepness)
- **Receptor theory:** Agonist (full, partial), antagonist (competitive, non-competitive), inverse agonist
- **Therapeutic index:** TI = TD50 / ED50 (or LD50 / ED50); narrow TI drugs require monitoring (warfarin, digoxin, lithium, aminoglycosides, phenytoin)
- **Tolerance and tachyphylaxis:** Reduced response with repeated exposure; tolerance (gradual), tachyphylaxis (rapid)
- **PK/PD modeling:** Links drug exposure (PK) to pharmacological effect (PD); turnover models, indirect response models, transit compartment models

**Drug Interactions:**

- **PK interactions:** Absorption (chelation, pH, P-gp), distribution (protein binding — rarely clinically significant), metabolism (CYP inhibition/induction), elimination (renal transporter competition)
- **PD interactions:** Additive (1+1=2), synergistic (1+1>2), antagonistic (1+1<2); serotonin syndrome (SSRI + MAOI), QT prolongation (multiple QT-prolonging drugs)
- **Clinical significance grading:** Contraindicated, major (avoid), moderate (monitor), minor (awareness)
- **Time course:** Inhibition typically rapid (hours-days); induction requires protein synthesis (days-weeks); offset of induction also slow

**Adverse Drug Reactions (ADRs):**

- **Type A (augmented):** Dose-dependent, predictable, related to pharmacological action; most common (~80% of ADRs)
- **Type B (bizarre):** Dose-independent, unpredictable, immunological or idiosyncratic; includes anaphylaxis, SJS/TEN, drug-induced liver injury
- **Type C (chronic):** With chronic use (e.g., osteoporosis from corticosteroids)
- **Type D (delayed):** Teratogenicity, carcinogenicity
- **Type E (end-of-use):** Withdrawal reactions (benzodiazepines, opioids, beta-blockers)
- **Pharmacovigilance:** Post-marketing surveillance; spontaneous reporting (Yellow Card, MedWatch), signal detection, FAERS database

**Therapeutic Drug Monitoring (TDM):**

- Indications: narrow therapeutic index, unpredictable PK, concentration-effect relationship established, assay available
- Common TDM drugs: vancomycin, aminoglycosides, lithium, digoxin, phenytoin, carbamazepine, valproic acid, ciclosporin, tacrolimus, sirolimus, methotrexate, theophylline
- Trough levels: sample just before next dose (most common)
- AUC-guided dosing: vancomycin AUC/MIC target 400-600; increasingly preferred over trough-only monitoring

## Key Research Methodologies

**PK Studies:**

- **Single ascending dose (SAD):** First-in-human; PK parameters at escalating doses
- **Multiple ascending dose (MAD):** Steady-state PK; accumulation assessment
- **Bioequivalence:** Two formulations compared; 90% CI of AUC and Cmax ratios must fall within 80-125%
- **Food effect:** Fasted vs fed PK comparison
- **Drug-drug interaction studies:** Probe substrate + inhibitor/inducer; compare PK parameters
- **Special populations:** Renal impairment, hepatic impairment, pediatric, elderly, obesity; dose adjustment guidance
- **Population PK (PopPK):** Nonlinear mixed-effects modeling (NONMEM, Monolix); identifies covariates affecting PK; sparse sampling designs

**PD and PK/PD Studies:**

- Exposure-response analysis: relate drug exposure (AUC, Cmin, Cmax) to efficacy and safety outcomes
- Dose-response modeling: Emax, sigmoid Emax, linear, log-linear
- Model-informed drug development (MIDD): PK/PD models to optimize dosing, support regulatory decisions

**Pharmacovigilance Studies:**

- Spontaneous reporting analysis: disproportionality measures (PRR, ROR, IC, EBGM)
- Prescription event monitoring (PEM): cohort-based active surveillance
- Signal detection and evaluation: WHO-UMC, EMA, FDA methodologies

## Common Study Designs

| Design | Application | Key Feature |
|--------|------------|-------------|
| SAD/MAD | Phase I PK characterization | Dose escalation, healthy volunteers |
| Bioequivalence (crossover) | Generic drug approval | 2-period, 2-sequence crossover |
| DDI study (crossover) | Interaction characterization | Probe substrate +/- interacting drug |
| PopPK (sparse sampling) | PK in patients within clinical trials | NONMEM, leverages routine samples |
| Exposure-response | Dose selection, labeling | Link PK to efficacy/safety |
| TDM cohort | Validate TDM targets | Retrospective or prospective |

## Important Databases and Registries

| Resource | Purpose | Access |
|----------|---------|--------|
| **FDA FAERS** | US adverse event reporting system | fda.gov/drugs/surveillance |
| **EudraVigilance** | European ADR database | adrreports.eu |
| **WHO VigiBase** | Global ICSR database (Uppsala Monitoring Centre) | vigiaccess.org |
| **DrugBank** | Comprehensive drug and drug target database | go.drugbank.com |
| **PharmGKB** | Pharmacogenomics knowledge base | pharmgkb.org |
| **CPIC** | Clinical Pharmacogenetics Implementation Consortium | cpicpgx.org |
| **FDA Drug Labels (DailyMed)** | Prescribing information | dailymed.nlm.nih.gov |
| **BNF (British National Formulary)** | UK drug formulary | bnf.nice.org.uk |
| **Lexicomp / Micromedex** | Drug interaction checkers | commercial |
| **Flockhart Table** | CYP450 drug interactions table | Maintained at Indiana University |

## Key Journals

- **Clinical Pharmacology & Therapeutics** — ASCPT journal; clinical pharmacology
- **British Journal of Clinical Pharmacology** — clinical pharmacology
- **European Journal of Clinical Pharmacology** — European clinical pharmacology
- **Journal of Pharmacokinetics and Pharmacodynamics** — PK/PD modeling
- **CPT: Pharmacometrics & Systems Pharmacology** — quantitative pharmacology
- **Drug Safety** — pharmacovigilance, ADRs
- **Pharmacoepidemiology and Drug Safety** — drug safety epidemiology
- **Clinical Pharmacokinetics** — PK reviews and analyses
- **Therapeutic Drug Monitoring** — TDM methodology and clinical application
- **The Journal of Clinical Pharmacology** — ACCP journal

## Verification Considerations

**PK Parameter Plausibility:**

- Check: is the reported Vd physiologically plausible? (Vd of ~3L = plasma only; ~15L = ECF; ~42L = total body water; >42L = tissue accumulation)
- Check: is the half-life consistent with Vd and CL? t1/2 = 0.693 * Vd / CL
- Check: does the reported bioavailability make sense for the route and drug properties?
- Check: is time to steady state approximately 5 * t1/2?

**Drug Interaction Assessment:**

- Check: is the interaction mechanism specified (CYP inhibition, induction, transporter)?
- Check: is the magnitude of interaction clinically significant? (AUC fold-change: <2 = weak, 2-5 = moderate, >5 = strong inhibitor)
- Check: are time-dependent effects considered? (induction takes days-weeks; inhibition is faster)
- Check: are in vitro findings corroborated by clinical data?

**Pharmacogenomics Validity:**

- Check: is the gene-drug pair supported by CPIC or DPWG guidelines?
- Check: are allele frequencies appropriate for the study population (vary dramatically by ethnicity)?
- Check: is the evidence level sufficient for clinical implementation (CPIC Level A/B vs C/D)?

**ADR Causality:**

- Check: is causality assessed using validated algorithms (Naranjo, WHO-UMC)?
- Check: is rechallenge/dechallenge information provided?
- Check: are alternative causes excluded?

## Common Pitfalls and LLM Error Risks

- **Confusing PK and PD:** LLMs may mix up pharmacokinetic parameters (what the body does to the drug) with pharmacodynamic effects (what the drug does to the body)
- **Wrong CYP enzyme:** LLMs may attribute drug metabolism to the wrong CYP enzyme. Always verify against DrugBank or the drug label
- **Ignoring active metabolites:** Some drugs (codeine->morphine, clopidogrel->active thiol, tamoxifen->endoxifen) require metabolic activation. LLMs may discuss the parent drug without considering the active metabolite
- **Fabricating PK parameters:** LLMs generate plausible but incorrect half-lives, bioavailabilities, and Vd values. Always verify against published PK data
- **Oversimplifying drug interactions:** LLMs may present in vitro interaction data as clinically significant, or miss clinically important interactions
- **Confusing inhibition and induction timelines:** Inhibition is usually rapid; induction requires days to weeks for enzyme synthesis. LLMs may not distinguish these temporal differences
- **Wrong dosing adjustments:** LLMs may provide incorrect renal or hepatic dose adjustments. Always verify against the prescribing information
- **Misinterpreting TDM targets:** Therapeutic ranges vary by indication, assay, and population. LLMs may apply one context's target range universally
- **Ignoring protein binding displacement:** While frequently cited, protein binding displacement is rarely clinically significant (free drug concentration rapidly re-equilibrates). LLMs may overstate its importance
- **Misattributing pharmacogenomic associations:** LLMs may incorrectly associate genes with drugs (e.g., CYP2D6 relevance to a drug primarily metabolized by CYP3A4)

## Methodology Decision Tree

```
What is the pharmacology research question?
├── PK characterization
│   ├── First-in-human? → SAD/MAD in healthy volunteers
│   ├── Special population? → Dedicated renal/hepatic impairment study
│   ├── Drug interaction? → Crossover DDI study with probe substrate
│   └── Population PK? → Sparse sampling within phase II/III, NONMEM analysis
├── Dose optimization
│   ├── Exposure-response known? → PK/PD modeling, simulation-based dose selection
│   ├── TDM feasible? → Prospective TDM study with target validation
│   └── Pediatric extrapolation? → Physiologically-based PK (PBPK) modeling
├── Drug safety
│   ├── Signal detection? → Disproportionality analysis in FAERS/VigiBase
│   ├── Characterize ADR? → Case-control within pharmacovigilance database
│   └── QT prolongation? → Thorough QT study (ICH E14) or concentration-QTc analysis
├── Pharmacogenomics
│   ├── Discovery? → GWAS for drug response phenotype
│   ├── Validation? → Replication in independent cohort
│   └── Implementation? → CPIC guideline adherence, clinical decision support
└── Bioequivalence
    ├── Immediate release? → 2-period crossover, AUC + Cmax
    ├── Modified release? → Additional fed-state study
    └── Highly variable drug? → Reference-scaled or replicate crossover design
```

## Research Frontiers (2024-2026)

| Frontier | Key question | GMD suitability |
|----------|-------------|-----------------|
| **Model-informed drug development (MIDD)** | PK/PD models to replace some clinical studies (FDA/EMA endorsed) | Good — methodology review |
| **Physiologically-based PK (PBPK)** | Mechanistic prediction of DDIs, pediatric dosing, organ impairment | Good — model validation assessment |
| **Real-world TDM optimization** | Machine learning for individualized dosing | Moderate — validation framework |
| **Pharmacogenomics implementation** | Barriers and facilitators to clinical PGx adoption | Good — implementation science review |
| **Drug repurposing** | Systematic approaches to identifying new indications | Good — evidence synthesis |
| **Polypharmacy management** | AI-assisted interaction checking, deprescribing tools | Good — methodology assessment |
