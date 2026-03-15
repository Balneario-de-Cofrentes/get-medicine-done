---
load_when:
  - "PICO"
  - "PECO"
  - "research question"
  - "clinical question"
  - "population intervention comparator outcome"
  - "eligibility criteria"
  - "inclusion criteria"
  - "review question"
  - "foreground question"
tier: 1
context_cost: low
---

# PICO Framework for Formulating Research Questions

PICO (Population, Intervention, Comparator, Outcome) is the foundational framework for structuring clinical and research questions. Well-formulated PICO questions drive every downstream decision in evidence synthesis: search strategy, eligibility criteria, data extraction, and analysis.

**Core principle:** A vague question produces a vague answer. A precise PICO question constrains the evidence space and ensures the review answers something actionable.

## Related Protocols

- `prisma-systematic-review.md` -- PICO structures the review objectives (Item 4)
- `search-strategy.md` -- PICO elements become search terms
- `data-extraction.md` -- PICO defines what data to extract
- `grade-certainty.md` -- Indirectness is assessed against the PICO question

---

## PICO Components

### P -- Population

Define the target population precisely:

| Element | Guidance | Example |
|---------|---------|---------|
| Condition/disease | Diagnostic criteria, severity, stage | Type 2 diabetes (HbA1c > 7.0%) |
| Demographics | Age, sex, ethnicity (if relevant) | Adults aged 18-65 years |
| Setting | Inpatient, outpatient, community | Primary care in high-income countries |
| Comorbidities | Include or exclude specific comorbidities | Excluding CKD stage 4-5 |
| Prior treatment | Treatment-naive, treatment-experienced | Treatment-naive (no prior antidiabetic medication) |

**Common errors:**

- Too broad: "patients with diabetes" (type 1 and type 2 have fundamentally different management).
- Too narrow: "female patients aged 50-55 with BMI 28-30 and HbA1c 7.5-8.0" (so narrow that few studies will match).
- Missing the setting: the same intervention may have different effects in primary care vs. hospital settings.

### I -- Intervention (or Exposure)

Define the intervention or exposure of interest:

| Element | Guidance | Example |
|---------|---------|---------|
| Specific agent | Drug name, class, dose, formulation | Metformin 1000mg twice daily, oral |
| Complex intervention | All components, delivery, provider | CBT: 12 weekly 60-minute sessions, delivered by licensed psychologist |
| Exposure | Type, duration, intensity, measurement | Occupational asbestos exposure > 10 fiber-years |
| Timing | When administered relative to disease course | Within 4.5 hours of acute ischemic stroke onset |
| Duration | Length of treatment/exposure | Minimum 12 weeks of continuous treatment |

**For complex interventions, use the TIDieR checklist:**
1. Brief name
2. Why: rationale/theory
3. What (materials): manuals, equipment
4. What (procedures): activities, processes
5. Who provided: expertise, background
6. How: mode of delivery (face-to-face, telephone, web)
7. Where: location
8. When and how much: dose, schedule
9. Tailoring: personalization
10. Modifications: changes during the study

### C -- Comparator

Define what the intervention is compared against:

| Comparator Type | Description | Implications |
|----------------|-------------|-------------|
| Placebo | Inactive substance matching active intervention | Estimates absolute efficacy |
| No treatment / waitlist | No intervention provided | May overestimate effect due to expectation bias |
| Usual care / standard of care | Current standard practice | Most clinically relevant comparison |
| Active comparator | Another intervention | Head-to-head comparison for clinical decisions |
| Different dose | Same drug at different dose | Dose-response assessment |
| Self-comparison (pre-post) | Same patients before/after | Weak design; no control for temporal trends |

**Choosing the right comparator is often the most important PICO decision.** Placebo comparisons tell us whether something works at all; active comparisons tell us which option is better -- the latter is usually more decision-relevant.

### O -- Outcome

Define the outcomes of interest with precision:

| Element | Guidance | Example |
|---------|---------|---------|
| Outcome domain | Broad category | Glycemic control |
| Specific measure | Instrument or definition | HbA1c (%) |
| Time point | When measured | At 12 months |
| Metric | How summarized | Mean change from baseline |
| Threshold | Clinically important difference | 0.5% reduction in HbA1c |

**Outcome hierarchy:**

1. **Patient-important outcomes** (mortality, morbidity, quality of life, functional status)
2. **Clinical events** (hospitalization, disease recurrence, treatment failure)
3. **Surrogate outcomes** (biomarkers, imaging findings)

GRADE requires at least one patient-important outcome. Surrogate outcomes should be clearly identified and their validity as proxies justified.

**Distinguish outcome types:**

| Type | Definition | Examples |
|------|-----------|---------|
| Efficacy | Does the intervention produce the intended effect? | Symptom reduction, disease remission |
| Effectiveness | Does the intervention work in real-world settings? | Hospitalization rates, all-cause mortality |
| Safety/harms | Does the intervention cause adverse effects? | Serious adverse events, treatment discontinuation |
| Patient-reported | Outcomes assessed by patients themselves | Quality of life (EQ-5D), pain (VAS), patient satisfaction |

---

## PICO Variants

### PECO (Observational Studies)

For observational studies, "Intervention" becomes "Exposure":

- **P**opulation: Adults aged > 50 years
- **E**xposure: Long-term statin use (> 5 years)
- **C**omparator: No statin use
- **O**utcome: Incident Alzheimer's disease

### PICOS (Adding Study Design)

Add the fifth element "S" for study design when formulating eligibility criteria:

- **S**tudy design: Randomised controlled trials only

### PICOT (Adding Time Frame)

Add "T" for the time frame of outcome assessment:

- **T**ime: 12-month follow-up

### PICOTS (Full Framework)

Combine all elements for the most complete specification:

| Element | Specification |
|---------|--------------|
| P | Adults with moderate-to-severe major depressive disorder (PHQ-9 >= 15) |
| I | Escitalopram 10-20 mg daily |
| C | Sertraline 50-200 mg daily |
| O | Response (>= 50% reduction in PHQ-9) at 8 weeks |
| T | 8-week acute treatment phase |
| S | Randomised controlled trials |

---

## From PICO to Search Strategy

Each PICO element generates search terms:

```
P: type 2 diabetes
  → MeSH: "Diabetes Mellitus, Type 2"[Mesh]
  → Free text: "type 2 diabetes" OR "T2DM" OR "NIDDM" OR "non-insulin dependent"

I: metformin
  → MeSH: "Metformin"[Mesh]
  → Free text: "metformin" OR "glucophage" OR "dimethylbiguanide"

C: sulfonylureas
  → MeSH: "Sulfonylurea Compounds"[Mesh]
  → Free text: "sulfonylurea*" OR "glipizide" OR "glyburide" OR "glimepiride" OR "glibenclamide"

O: HbA1c
  → MeSH: "Glycated Hemoglobin A"[Mesh]
  → Free text: "HbA1c" OR "glycated hemoglobin" OR "glycosylated hemoglobin" OR "A1C"
```

Combine within elements with OR, between elements with AND:

```
(P terms) AND (I terms) AND (C terms)
```

Note: Many search strategies omit the outcome terms to increase sensitivity, then filter by outcome during screening.

See `search-strategy.md` for complete guidance on search construction.

---

## From PICO to Eligibility Criteria

| PICO Element | Inclusion Criterion | Exclusion Criterion |
|-------------|--------------------|--------------------|
| P | Adults >= 18 years with confirmed type 2 diabetes (HbA1c > 7.0%) | Type 1 diabetes, gestational diabetes, age < 18 |
| I | Metformin monotherapy, any dose, oral, >= 12 weeks | Metformin in combination with other agents |
| C | Any sulfonylurea, any dose, >= 12 weeks | No comparator group |
| O | HbA1c measured at >= 12 weeks | Studies not reporting HbA1c |
| S | RCTs (individual or cluster) | Non-randomised studies, case reports |

---

## Worked Examples

### Example 1: Therapeutic Question (Treatment)

**Clinical scenario:** A guideline panel needs to recommend between two first-line treatments for hypertension.

| Element | Specification |
|---------|--------------|
| P | Adults with primary (essential) hypertension, no prior cardiovascular events |
| I | ACE inhibitors (any agent, any dose) |
| C | Thiazide diuretics (any agent, any dose) |
| O | Primary: major cardiovascular events (composite of MI, stroke, CV death). Secondary: all-cause mortality, individual components, adverse events, treatment discontinuation |
| T | Minimum 1-year follow-up |
| S | RCTs |

**PICO sentence:** In adults with primary hypertension and no prior cardiovascular events (P), do ACE inhibitors (I) compared with thiazide diuretics (C) reduce major cardiovascular events (O)?

### Example 2: Diagnostic Question

For diagnostic accuracy reviews, use PIRD (Population, Index test, Reference standard, Diagnosis):

| Element | Specification |
|---------|--------------|
| P | Adults presenting to emergency department with chest pain |
| I (Index test) | High-sensitivity troponin T (hs-cTnT), single measurement at presentation |
| R (Reference standard) | Adjudicated diagnosis of acute MI using serial troponin and clinical assessment |
| D (Diagnosis/Target condition) | Acute myocardial infarction |

### Example 3: Prognostic Question

For prognostic factor reviews, use PICOTS with the prognostic factor as the "intervention":

| Element | Specification |
|---------|--------------|
| P | Adults diagnosed with stage III non-small cell lung cancer |
| I (Prognostic factor) | PD-L1 expression (tumor proportion score >= 50%) |
| C | PD-L1 expression (tumor proportion score < 50%) |
| O | Overall survival, progression-free survival |
| T | Minimum 2-year follow-up |
| S | Cohort studies (prospective or retrospective) |

### Example 4: Etiology/Exposure Question

| Element | Specification |
|---------|--------------|
| P | Children aged 0-12 years |
| E | Screen time > 2 hours daily |
| C | Screen time <= 2 hours daily |
| O | Primary: obesity (BMI > 95th percentile). Secondary: sleep quality, academic performance |
| T | Minimum 6 months of exposure |
| S | Cohort studies and cross-sectional studies |

---

## Refining the PICO: Decision Points

| Decision | Options | Trade-off |
|----------|---------|-----------|
| Population breadth | Broad (all diabetes) vs. narrow (T2DM with CKD) | Broad increases quantity but decreases applicability; narrow increases relevance but may find few studies |
| Intervention specificity | Drug class vs. individual drug | Class-level answers broader questions; drug-level is more directly actionable |
| Comparator choice | Placebo vs. active | Placebo comparison measures efficacy; active comparison informs clinical decisions |
| Outcome selection | Surrogate vs. clinical | Surrogates available sooner; clinical outcomes matter more to patients |
| Study design | RCTs only vs. including observational | RCTs reduce confounding; observational studies may be the only evidence for harms or rare exposures |

**When in doubt:** Start broad, then narrow based on the volume and relevance of the evidence found during scoping.

---

## Verification Checklist

Before finalizing a PICO question:

- [ ] Each element (P, I, C, O) is explicitly defined with operational criteria
- [ ] Population is specific enough to be meaningful but broad enough to find evidence
- [ ] Intervention is described with sufficient detail for replication
- [ ] Comparator is clinically relevant (not just placebo if active comparison matters)
- [ ] At least one patient-important outcome is included
- [ ] Outcomes include both benefits and harms
- [ ] Time frames are specified for outcomes
- [ ] The PICO question is answerable (evidence likely exists)
- [ ] The PICO translates directly to eligibility criteria
- [ ] The PICO translates directly to search terms
