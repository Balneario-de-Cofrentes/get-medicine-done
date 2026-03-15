---
load_when:
  - "oncology"
  - "cancer"
  - "tumor"
  - "tumour"
  - "chemotherapy"
  - "immunotherapy"
  - "checkpoint inhibitor"
  - "precision oncology"
  - "overall survival"
  - "progression-free survival"
  - "RECIST"
tier: 2
context_cost: medium
---

# Oncology

## Core Concepts and Terminology

**Cancer Staging:**

- **TNM system (AJCC/UICC):** T = tumor size/extent, N = lymph node involvement, M = distant metastasis; combined into overall stages I-IV
- **Performance status:** ECOG (0-5; 0=fully active, 5=dead) or Karnofsky (0-100; 100=normal); critical for treatment decisions and trial eligibility
- **Histological grading:** Grade 1 (well-differentiated) to Grade 3/4 (poorly differentiated); varies by tumor type
- **Molecular classification:** Increasingly supplements histology; e.g., breast cancer (ER, PR, HER2, Ki67 → Luminal A/B, HER2+, triple-negative), lung cancer (EGFR, ALK, ROS1, KRAS, PD-L1)

**Clinical Endpoints:**

- **Overall survival (OS):** Time from randomization to death from any cause; gold standard; unbiased by assessment timing
- **Progression-free survival (PFS):** Time from randomization to disease progression (by RECIST) or death; widely used surrogate; influenced by assessment schedule
- **Objective response rate (ORR):** Proportion achieving complete or partial response (CR + PR) by RECIST 1.1; used in phase II
- **Disease control rate (DCR):** CR + PR + stable disease
- **Duration of response (DoR):** Time from first response to progression
- **Time to progression (TTP):** Censors deaths; NOT recommended as primary endpoint
- **Patient-reported outcomes (PROs):** Quality of life (EORTC QLQ-C30, FACT), symptom burden; increasingly required by regulators

**RECIST 1.1 Criteria:**

- **Complete response (CR):** Disappearance of all target lesions; lymph nodes <10mm short axis
- **Partial response (PR):** >=30% decrease in sum of longest diameters of target lesions
- **Progressive disease (PD):** >=20% increase in sum plus >=5mm absolute increase; or new lesions
- **Stable disease (SD):** Neither sufficient shrinkage for PR nor sufficient increase for PD
- **iRECIST:** Modified criteria for immunotherapy allowing confirmation of progression (due to pseudoprogression)

**Immunotherapy:**

- **Checkpoint inhibitors:** Anti-PD-1 (nivolumab, pembrolizumab), anti-PD-L1 (atezolizumab, durvalumab, avelumab), anti-CTLA-4 (ipilimumab, tremelimumab)
- **Biomarkers:** PD-L1 expression (TPS, CPS; different assays/platforms), tumor mutational burden (TMB), microsatellite instability (MSI-H/dMMR), specific gene expression signatures
- **Immune-related adverse events (irAEs):** Colitis, hepatitis, pneumonitis, dermatitis, endocrinopathies; managed with corticosteroids and immunosuppression; grading by CTCAE
- **Pseudoprogression:** Initial apparent tumor growth followed by response; occurs in ~5-10% of patients on immunotherapy
- **Hyperprogression:** Accelerated tumor growth after immunotherapy initiation; controversial definition and mechanism
- **CAR-T therapy:** Chimeric antigen receptor T-cells; approved for B-cell lymphomas (CD19), multiple myeloma (BCMA); cytokine release syndrome (CRS) and neurotoxicity (ICANS) as major toxicities

**Precision Oncology:**

- **Molecular tumor board:** Multidisciplinary review of genomic profiling results (NGS panels, WES, WGS) to guide treatment
- **Companion diagnostics:** FDA-approved tests required for specific drug use (e.g., HER2 IHC/FISH for trastuzumab)
- **Tissue-agnostic approvals:** Pembrolizumab for MSI-H/dMMR tumors regardless of histology; larotrectinib for NTRK fusion-positive tumors
- **Liquid biopsy:** ctDNA for mutation detection, treatment monitoring, minimal residual disease (MRD)
- **Tumor mutational burden (TMB):** Surrogate for neoantigen load; TMB-H (>=10 mut/Mb) approved as pan-tumor biomarker for pembrolizumab

## Key Research Methodologies

**Oncology Trial Design:**

- **Superiority trials:** Standard: experimental vs standard of care; OS or PFS as primary endpoint
- **Phase I oncology:** Dose escalation (3+3 traditional; CRM/BOIN model-based; accelerated titration); DLT assessment window typically 28 days
- **Phase II:** Simon two-stage (single arm), randomized phase II (experimental vs control), single-arm with historical control
- **Randomized phase III:** Definitive efficacy; stratification by key prognostic factors; interim analyses with alpha spending
- **Biomarker-stratified designs:** Enrichment (only biomarker-positive); all-comers with prospective subgroup analysis; umbrella/basket
- **Window-of-opportunity:** Short treatment course between diagnosis and definitive surgery; paired biopsies for biomarker discovery

**Survival Analysis:**

- **Kaplan-Meier:** Non-parametric survival curve estimation; log-rank test for group comparison (assumes proportional hazards)
- **Cox proportional hazards:** HR as effect measure; check PH assumption (Schoenfeld residuals, log-log plot); time-dependent covariates if PH violated
- **Restricted mean survival time (RMST):** Area under survival curve up to a specified time; does not require PH assumption; increasingly used when PH is violated
- **Milestone analysis:** Survival probability at fixed time points (e.g., 12-month OS rate, 24-month PFS rate)
- **Cure models:** Mixture cure models for immunotherapy (plateau in survival curve suggests cured fraction)
- **Competing risks:** Cumulative incidence function (Fine-Gray) when non-cancer death competes with cancer-specific events

**Response Assessment:**

- RECIST 1.1 for solid tumors; iRECIST for immunotherapy
- Cheson criteria for lymphoma (Lugano classification)
- IMWG criteria for multiple myeloma
- RANO criteria for brain tumors
- Independent central review (blinded) vs investigator assessment; discordance can be substantial

## Common Study Designs

| Design | Phase | Key Feature | Example |
|--------|-------|------------|---------|
| 3+3 dose escalation | I | Rule-based, 3 patients per dose level | Traditional oncology phase I |
| CRM / BOIN | I | Model-based dose escalation | More efficient, targets specific DLT rate |
| Simon two-stage | II | Early futility stopping | Single-arm activity assessment |
| Randomized phase II | II | Active comparator, randomized | Activity estimation with less bias |
| Superiority phase III | III | OS or PFS primary endpoint | Definitive efficacy trial |
| Basket trial | II/III | One drug, multiple tumor types | Vemurafenib in BRAF V600E pan-cancer |
| Umbrella trial | II/III | One disease, biomarker-directed arms | Lung-MAP (squamous NSCLC) |
| Platform trial | II/III | Master protocol, add/drop arms | I-SPY 2 (breast cancer) |

## Important Databases and Registries

| Resource | Purpose | Access |
|----------|---------|--------|
| **ClinicalTrials.gov** | Oncology trial registration | clinicaltrials.gov |
| **SEER** | US cancer incidence, survival | seer.cancer.gov |
| **GLOBOCAN / GCO** | Global cancer statistics (IARC) | gco.iarc.fr |
| **cBioPortal** | Cancer genomics data | cbioportal.org |
| **TCGA (The Cancer Genome Atlas)** | Multi-omic tumor profiling | cancer.gov/tcga |
| **COSMIC** | Catalogue of Somatic Mutations in Cancer | cancer.sanger.ac.uk/cosmic |
| **NCCN Guidelines** | Clinical practice guidelines | nccn.org |
| **ESMO Guidelines** | European oncology guidelines | esmo.org |
| **OncoKB** | Precision oncology knowledge base | oncokb.org |
| **CIViC** | Clinical Interpretation of Variants in Cancer | civicdb.org |

## Key Journals

- **New England Journal of Medicine** — practice-changing oncology trials
- **The Lancet Oncology** — oncology-specific trials and reviews
- **Journal of Clinical Oncology (JCO)** — ASCO journal; trials, guidelines
- **Annals of Oncology** — ESMO journal
- **Nature Medicine** — translational oncology
- **Cancer Discovery** — AACR; mechanisms and translational
- **JAMA Oncology** — clinical oncology
- **Journal of the National Cancer Institute (JNCI)** — epidemiology, clinical
- **Clinical Cancer Research** — translational
- **European Journal of Cancer** — European oncology

## Verification Considerations

**Survival Analysis Validity:**

- Check: are Kaplan-Meier curves provided? (not just median survival)
- Check: is the proportional hazards assumption met? If not, alternative approaches (RMST, milestone) should be considered
- Check: is median follow-up sufficient to capture events? (reverse Kaplan-Meier for median follow-up)
- Check: are immature data (few events, short follow-up) acknowledged?
- Check: for immunotherapy, is there a plateau in the tail of the KM curve? Does this persist with extended follow-up?

**Biomarker Validation:**

- Check: is the biomarker assay validated analytically (reproducibility, concordance between platforms)?
- Check: was the cut-point pre-specified or data-driven? Data-driven cut-points are inflated
- Check: is the biomarker predictive (guides treatment) or prognostic (predicts outcome regardless of treatment)?
- Check: was the biomarker analysis pre-planned (confirmatory) or post-hoc (exploratory)?

**Regulatory Context:**

- Check: is accelerated approval based on ORR/PFS confirmed by subsequent OS benefit?
- Check: have any accelerated approvals been withdrawn for lack of confirmatory benefit?
- Check: is the indication consistent with the trial population?

**Toxicity Reporting:**

- Check: adverse events reported using CTCAE grading (grade 1-5)?
- Check: are treatment-related deaths reported separately?
- Check: for immunotherapy, are irAEs specifically captured and managed per guidelines?

## Common Pitfalls and LLM Error Risks

- **Confusing PFS and OS:** PFS is a surrogate endpoint; improvements in PFS do not always translate to OS benefit. LLMs may present PFS gains as equivalent to survival improvement
- **Ignoring non-proportional hazards:** Immunotherapy often shows delayed separation of survival curves (crossing curves early). HR is misleading in this setting. LLMs rarely flag this
- **Misinterpreting response rates:** ORR of 40% does not mean 40% of patients are cured; most responses are partial and temporary. LLMs may overstate the clinical significance of response rates
- **Fabricating trial results:** LLMs generate plausible but entirely fictitious median OS, PFS, ORR values, and trial names. Always verify against published data
- **Confusing tumor types:** Molecular subtypes within a cancer type have different prognoses and treatments (e.g., EGFR-mutant vs KRAS-mutant NSCLC). LLMs may give generic advice
- **Wrong RECIST criteria:** Using RECIST 1.0 instead of 1.1; confusing target vs non-target lesions; applying solid tumor criteria to lymphoma
- **Misunderstanding accelerated approval:** A drug with accelerated approval based on ORR is not fully proven to improve survival. LLMs may present accelerated approval drugs as having the same evidence base as full approval
- **Survivorship bias in reporting:** Case reports and series of long-term survivors are subject to extreme selection bias. LLMs may cite them as representative
- **Pseudoprogression overestimation:** Pseudoprogression is uncommon (~5-10%); LLMs may overstate its frequency, leading to delayed recognition of true progression

## Methodology Decision Tree

```
What is the oncology research question?
├── First-in-human safety and dosing
│   ├── Cytotoxic? → 3+3 or accelerated titration
│   ├── Targeted/immunotherapy? → CRM or BOIN (lower toxicity expected)
│   └── Combination? → Dose-finding in two dimensions (BOIN-combo)
├── Activity (efficacy signal)
│   ├── No comparator available? → Single-arm phase II (Simon two-stage)
│   ├── Active comparator exists? → Randomized phase II
│   └── Biomarker-selected? → Enrichment or basket design
├── Confirmatory efficacy
│   ├── OS feasible? → Phase III with OS primary
│   ├── OS impractical (long follow-up, crossover)? → PFS primary, OS secondary
│   └── Rare cancer? → International collaboration, may accept PFS/ORR
├── Biomarker discovery/validation
│   ├── Discovery? → Paired biopsies, window-of-opportunity
│   ├── Validation? → Prospectively designed substudy within RCT
│   └── Companion diagnostic? → Enrichment trial with CDx
└── Evidence synthesis
    ├── Treatment comparison? → Meta-analysis (pairwise or NMA)
    ├── Surrogate validation? → Meta-analysis of surrogacy (R2 for trial-level)
    └── Guideline development? → GRADE-based systematic review
```

## Research Frontiers (2024-2026)

| Frontier | Key question | GMD suitability |
|----------|-------------|-----------------|
| **Antibody-drug conjugates (ADCs)** | Trastuzumab deruxtecan, sacituzumab govitecan; expanding indications | Good — evidence synthesis |
| **Bispecific antibodies** | T-cell engagers across tumor types | Good — trial design review |
| **Minimal residual disease (MRD)** | ctDNA as surrogate endpoint for adjuvant therapy | Good — surrogate validation |
| **Neoadjuvant immunotherapy** | pCR as endpoint; de-escalation strategies | Good — evidence synthesis |
| **AI in pathology and radiology** | Automated tumor grading, radiomics for outcome prediction | Moderate — validation framework |
| **Cancer interception** | Early detection (multi-cancer blood tests like Galleri), chemoprevention | Good — evidence assessment |
