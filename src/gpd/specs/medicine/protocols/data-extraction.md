---
load_when:
  - "data extraction"
  - "data collection"
  - "extraction form"
  - "missing data"
  - "contacting authors"
  - "study characteristics"
  - "effect size extraction"
  - "outcome data"
  - "data coding"
tier: 1
context_cost: medium
---

# Data Extraction Protocol for Systematic Reviews

Data extraction is the process of systematically collecting relevant information from included studies into a structured format suitable for synthesis. It is one of the most error-prone steps in evidence synthesis -- errors in data extraction directly corrupt the meta-analysis.

**Core principle:** Every number entering a meta-analysis must be independently verified by two people using a standardized form. There is no shortcut to this discipline.

## Related Protocols

- `prisma-systematic-review.md` -- PRISMA Items 9, 10a, 10b (data collection and data items)
- `statistical-analysis.md` -- How extracted data feed into meta-analysis
- `pico-framework.md` -- PICO defines what data to extract
- `rob2-assessment.md` -- Risk of bias data extracted alongside outcome data

---

## Standardized Data Extraction Form

### Core Fields (Every Systematic Review)

**Study identification:**

| Field | Description | Example |
|-------|------------|---------|
| Study ID | First author + year | Smith 2022 |
| Full reference | Complete citation | Smith J, et al. Lancet. 2022;399:1234-42 |
| DOI | Digital Object Identifier | 10.1016/S0140-6736(22)00123-4 |
| Country | Country where study was conducted | United Kingdom |
| Funding source | Who funded the study | National Institute for Health Research |
| Conflicts of interest | Authors' declared conflicts | Lead author received consulting fees from Pharma Co |
| Trial registration | Registration number | NCT04123456 |

**Study design:**

| Field | Description | Options |
|-------|------------|---------|
| Study design | Type of study | RCT (parallel, crossover, cluster, factorial), cohort (prospective, retrospective), case-control (population-based, hospital-based), cross-sectional |
| Randomisation method | How participants were randomised | Computer-generated, minimization, quasi-random, not applicable |
| Allocation concealment | How allocation was concealed | Central telephone, sealed envelopes, not described, not applicable |
| Blinding | Who was blinded | Participants, care providers, outcome assessors, data analysts, none |
| Setting | Where study was conducted | Primary care, secondary care, tertiary hospital, community |
| Single/multi-center | Number of centers | Single center, multi-center (number of sites) |
| Duration of follow-up | Length of outcome assessment | Weeks, months, years (specify) |

**Participants:**

| Field | Description |
|-------|------------|
| Total enrolled | N randomised or enrolled per group |
| Total analysed | N analysed per group |
| Age | Mean (SD) or median (IQR) |
| Sex | N (%) female/male per group |
| Ethnicity | As reported |
| Diagnosis/condition | Diagnostic criteria used |
| Severity/stage | Baseline severity measure |
| Comorbidities | Relevant comorbidities |
| Prior treatments | Previous treatment history |

**Intervention and comparator:**

| Field | Description |
|-------|------------|
| Intervention name | Drug name, procedure, program name |
| Intervention details | Dose, frequency, route, duration |
| Comparator name | Placebo, active control, usual care |
| Comparator details | Dose, frequency, route, duration |
| Co-interventions | Additional treatments allowed in both groups |
| Adherence | Compliance rate or measure |

**Outcomes:**

For each outcome, extract:

| Field | Description |
|-------|------------|
| Outcome name | As defined in the study |
| Outcome definition | Operational definition |
| Measurement tool | Instrument used (e.g., PHQ-9, HbA1c assay) |
| Time point | When measured |
| Intervention group: N analysed | Denominator |
| Intervention group: events or mean | Numerator for binary; mean for continuous |
| Intervention group: SD or 95% CI | Measure of variability |
| Comparator group: N analysed | Denominator |
| Comparator group: events or mean | Numerator for binary; mean for continuous |
| Comparator group: SD or 95% CI | Measure of variability |
| Effect estimate (if reported) | OR, RR, HR, MD with CI |
| P-value | As reported |
| Analysis population | ITT, per-protocol, modified ITT |
| Adjusted for | Covariates in adjusted analysis |

---

## Extracting Different Data Types

### Binary Outcomes

Extract from each group: number of events and total participants (e.g., 45/200 intervention, 62/200 control).

If the study only reports a relative effect (OR, RR):

1. Check if you can reconstruct the 2x2 table from other reported numbers.
2. Use the relative effect with the control group event rate to estimate the 2x2 table.
3. Flag the estimation in the extraction form.

### Continuous Outcomes

Extract from each group: mean, standard deviation, and sample size.

**Common problems and solutions:**

| Problem | Solution |
|---------|----------|
| Median and IQR reported instead of mean and SD | Use estimation methods (Wan et al. 2014 for median + range; Luo et al. 2018 for median + IQR + sample size) |
| Standard error reported instead of SD | SD = SE x sqrt(N) |
| 95% CI reported instead of SD | SD = sqrt(N) x (upper CI - lower CI) / (2 x 1.96) |
| P-value reported instead of SD | Calculate SE from the p-value and the difference, then convert |
| Change from baseline reported instead of final values | Can combine change and final values using SMD; do not combine raw means |
| Only a graph (no numbers in text) | Use WebPlotDigitizer or similar tool; flag as estimated from graph |

### Time-to-Event Outcomes

Extract: hazard ratio, 95% CI, log-rank p-value, number of events per group, median survival per group.

If HR is not directly reported, use methods described in Tierney et al. (2007) to estimate from:

- Kaplan-Meier curves (using digitization and reconstruction)
- Log-rank p-value and number of events
- O-E and variance

### Ordinal or Count Outcomes

Extract: category counts per group, or mean count + SD per group.

Consider whether to dichotomize (losing information) or use proportional odds models.

---

## Handling Missing Data

### Decision Tree for Missing Data

```
Data needed for synthesis but not reported
  ├── Can be calculated from other reported data?
  │   └── Yes → Calculate and document the derivation
  │   └── No ↓
  ├── Available from trial registration, protocol, or supplementary materials?
  │   └── Yes → Extract from that source, cite it
  │   └── No ↓
  ├── Available from a companion publication or conference abstract?
  │   └── Yes → Extract from that source, cite it
  │   └── No ↓
  ├── Contact corresponding author
  │   └── Response received → Extract and note "personal communication"
  │   └── No response after 2 attempts (2 weeks apart) ↓
  ├── Can be imputed using established methods?
  │   └── Yes → Impute, document method, include in sensitivity analysis
  │   └── No ↓
  └── Exclude from quantitative synthesis, include narratively
```

### Author Contact Protocol

1. **First contact:** Email the corresponding author. Include:
   - Your name and institutional affiliation
   - Brief description of the systematic review
   - Specific data requested (be precise: "mean and SD of HbA1c at 12 months for intervention and control groups")
   - Explanation of why the data are needed
   - Deadline (typically 2-4 weeks)

2. **Follow-up:** If no response after 2 weeks, send a second email to the corresponding author and any co-authors whose contact information is available.

3. **Documentation:** Record all contact attempts, dates, and responses. Report in the systematic review how many authors were contacted, how many responded, and what data were obtained.

### Imputation Methods

| Missing Element | Imputation Method | Reference |
|----------------|-------------------|-----------|
| SD from SE | SD = SE x sqrt(N) | Standard formula |
| SD from 95% CI | SD = sqrt(N) x (upper - lower) / 3.92 | Standard formula (assuming normality) |
| SD from p-value | Via t-statistic | Cochrane Handbook Section 6.5.2.3 |
| SD from range | SD ~ range / 4 (for N < 70) or range / 6 (for N > 70) | Hozo et al. 2005 |
| Mean from median | Methods depend on distribution | Wan et al. 2014, Luo et al. 2018, McGrath et al. 2020 |
| SD from IQR | SD ~ IQR / 1.35 (assuming normality) | Standard approximation |
| Correlation for change scores | Impute from similar studies or use r = 0.5 | Cochrane Handbook Section 6.5.2.8 |

**Always conduct sensitivity analysis** varying imputed values across plausible ranges.

---

## Pilot Testing the Extraction Form

Before full extraction:

1. Select 3-5 representative included studies (varying in design, completeness, quality).
2. Have both extractors independently extract data from these studies.
3. Compare extractions and identify discrepancies.
4. Revise the form to resolve ambiguities.
5. Repeat with 1-2 additional studies if substantial changes were made.

**Goal:** Inter-extractor agreement should be high (> 90% concordance on key fields) before proceeding to full extraction.

---

## Handling Multiple Reports of the Same Study

One study may produce multiple publications (main paper, secondary analysis, long-term follow-up, post-hoc subgroup analysis). These must be linked to avoid double-counting.

### Linking Reports

1. Check author lists, study names, registration numbers, recruitment periods, sample sizes, and settings.
2. If the same participants appear in multiple reports, link them as one study with multiple reports.
3. Use the most complete or most recent report for each data element.
4. Document which report was the source for each extracted data point.

### Priority for Data Sources

| Priority | Source | Rationale |
|----------|--------|-----------|
| 1 | Trial registration results database | Often includes complete data without selective reporting |
| 2 | Main publication (journal article) | Peer-reviewed, most accessible |
| 3 | Supplementary materials / appendices | May contain data not in main text |
| 4 | Published protocol | Useful for assessing selective reporting |
| 5 | Conference abstracts | May contain preliminary data; verify against final publication |
| 6 | Author communication | Unpublished data; note as personal communication |
| 7 | FDA/EMA assessment reports | For drug trials; often more complete than publications |

---

## Quality Control

### Dual Independent Extraction

- Two extractors independently complete the form for every included study.
- Compare all fields; resolve discrepancies by discussion or third reviewer.
- Calculate agreement statistics (percent agreement, kappa) for a random subset.
- Flag and re-extract any studies where agreement is low.

### Verification of Calculations

If any data were calculated or transformed (e.g., SD from CI, mean from median):

- Document the formula used.
- Have the second extractor verify the calculation independently.
- Include the original reported values alongside the derived values.

### Data Entry Verification

- Double-check that numbers entered match the source.
- Common transcription errors: decimal point placement, sign of the effect, mixing up intervention and control groups.
- Use range checks: means should be within the measurement scale, SDs should be positive, proportions should be 0-1.

---

## Verification Checklist

Before finalizing data extraction:

- [ ] Standardized extraction form pilot-tested on 3-5 studies
- [ ] Dual independent extraction completed for all studies
- [ ] Discrepancies resolved by discussion or third reviewer
- [ ] Multiple reports of the same study linked and deduplicated
- [ ] All derivations and imputations documented with formulas
- [ ] Author contact attempts documented with dates and outcomes
- [ ] Data sources prioritized (registration > publication > supplements > communication)
- [ ] Range checks performed on extracted data
- [ ] Sensitivity analyses planned for imputed data
- [ ] Extraction data stored in reproducible format (spreadsheet, database)
