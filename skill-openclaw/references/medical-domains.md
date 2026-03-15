# Medical Domain References

Available in the [get-medicine-done](https://github.com/Balneario-de-Cofrentes/get-medicine-done) fork under `src/gpd/specs/medicine/`.

## Subfields (`medicine/subfields/`)

| File | Domain |
|------|--------|
| `balneology-thermalism.md` | Thermal medicine, hydrotherapy, spa therapy, pelotherapy |
| `clinical-trials.md` | RCT design, adaptive trials, crossover, non-inferiority |
| `epidemiology.md` | Cohort, case-control, ecological, causal inference |
| `evidence-synthesis.md` | Systematic reviews, meta-analysis, NMA, PRISMA, GRADE |
| `gerontology.md` | Aging, longevity, geriatric medicine, frailty |
| `health-economics.md` | Cost-effectiveness, QALY, budget impact, Markov models |
| `internal-medicine.md` | Cardiology, pulmonology, nephrology, endocrinology |
| `oncology.md` | Clinical oncology, immunotherapy, survival analysis |
| `pharmacology.md` | PK/PD, drug interactions, dose-response |
| `physical-medicine-rehabilitation.md` | Physiotherapy, occupational therapy |
| `psychiatry-neuroscience.md` | Mental health, neuropsychology |
| `public-health.md` | Health policy, screening, prevention |
| `surgery.md` | Surgical outcomes, perioperative care |

## Protocols (`medicine/protocols/`)

| File | Standard |
|------|----------|
| `prisma-systematic-review.md` | PRISMA 2020 (27-item checklist) |
| `consort-rct.md` | CONSORT 2010 (RCT reporting) |
| `strobe-observational.md` | STROBE (observational studies) |
| `grade-certainty.md` | GRADE (certainty of evidence) |
| `rob2-assessment.md` | Cochrane RoB 2 (RCT bias) |
| `robins-i-assessment.md` | ROBINS-I (non-randomized bias) |
| `newcastle-ottawa.md` | Newcastle-Ottawa Scale |
| `pico-framework.md` | PICO framework |
| `search-strategy.md` | Boolean, MeSH, database syntax |
| `data-extraction.md` | Standardized forms, missing data |
| `statistical-analysis.md` | R metafor/meta, forest/funnel plots |
| `narrative-synthesis.md` | SWiM guidance |
| `network-meta-analysis.md` | NMA methodology |
| `prospero-registration.md` | Protocol registration |
| `causal-inference.md` | DAGs, propensity scores, IV |

## Verification Domains (`medicine/verification/domains/`)

| File | Checks |
|------|--------|
| `verification-domain-systematic-review.md` | PRISMA compliance, search validation, screening, RoB, GRADE |
| `verification-domain-clinical-trial.md` | CONSORT, randomization, blinding, ITT, sample size |
| `verification-domain-meta-analysis.md` | I²/τ², publication bias, sensitivity, subgroup validity |
| `verification-domain-epidemiology.md` | Confounding, selection bias, causal DAGs |
| `verification-domain-health-economics.md` | Cost model, QALY, budget impact |
| `verification-domain-statistics.md` | Assumptions, multiple comparisons, power, effect sizes |

## LLM Error Catalog (`medicine/verification/errors/`)

`llm-errors-medical.md` — 13+ error classes common in LLM-assisted medical research:
RR/OR/HR confusion, p-value misinterpretation, GRADE errors, heterogeneity misreading, immortal time bias, outcome switching, NNT/NNH errors, MeSH term hallucination, etc.
