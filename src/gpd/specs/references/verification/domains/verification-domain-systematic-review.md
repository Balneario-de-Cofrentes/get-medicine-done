---
load_when:
  - "systematic review verification"
  - "PRISMA compliance"
  - "search strategy validation"
  - "screening quality"
  - "risk of bias"
  - "GRADE assessment"
  - "evidence synthesis"
  - "inclusion exclusion criteria"
tier: 2
context_cost: large
---

# Verification Domain — Systematic Review

PRISMA 2020 compliance, search strategy validation, screening quality assessment, risk of bias evaluation, GRADE certainty of evidence, and data extraction accuracy for systematic reviews.

**Load when:** Working on systematic review methodology, evidence synthesis, PRISMA adherence, search strategy design, risk of bias assessment, or GRADE evaluation.

**Related files:**
- `../core/verification-quick-reference.md` — compact checklist (default entry point)
- `../core/verification-core.md` — foundational verification principles
- `references/verification/domains/verification-domain-meta-analysis.md` — meta-analysis (for quantitative synthesis)
- `references/verification/domains/verification-domain-statistics.md` — statistical methods (for effect measures)
- `references/verification/domains/verification-domain-epidemiology.md` — epidemiology (for study design assessment)

---

<prisma_compliance>

## PRISMA 2020 Compliance Checking

**27-item checklist verification:**

```
PRISMA 2020 is organized into seven sections with 27 items:

Title (1 item):
  Item 1: Identify the report as a systematic review.
  Verification: Title must contain "systematic review" (and "meta-analysis" if applicable).

Abstract (1 item):
  Item 2: Structured summary including background, objectives, data sources,
  eligibility criteria, participants, interventions, appraisal methods, results,
  limitations, conclusions, registration number.
  Verification: Check each sub-element is present. Missing registration number is a common gap.

Introduction (2 items):
  Item 3: Rationale — describe what is known and the knowledge gap.
  Item 4: Objectives — PICO(S) framework explicitly stated.
  Verification: If PICO elements are absent or vague, flag as incomplete.

Methods (12 items):
  Items 5-16 covering eligibility, information sources, search strategy,
  selection process, data collection, data items, study risk of bias,
  effect measures, synthesis methods, reporting bias, certainty assessment.
  Verification: Each item must be explicitly addressed. Protocol registration (Item 24a)
  must reference PROSPERO, OSF, or equivalent.

Results (7 items):
  Items 17-23 covering study selection, characteristics, risk of bias in studies,
  results of individual studies, synthesis results, reporting biases, certainty.
  Verification: PRISMA flow diagram (Item 17) is MANDATORY. Absence = automatic flag.

Discussion (2 items):
  Items 24-25 covering interpretation and limitations.

Other (2 items):
  Items 26-27 covering registration/protocol and funding.
```

**Common PRISMA violations to flag:**

```
1. Missing flow diagram — most frequent violation. Must show:
   - Records identified through each database
   - Records after deduplication
   - Records screened / excluded at title-abstract
   - Full-text articles assessed / excluded with reasons
   - Studies included in qualitative synthesis
   - Studies included in quantitative synthesis (if applicable)

2. Search strategy not fully reproducible — must include:
   - Complete search string for at least one database
   - Date of last search
   - Any limits applied (language, date, publication type)

3. No protocol registration — PROSPERO ID or equivalent must be stated.
   If not registered: flag and check for selective outcome reporting.

4. GRADE assessment absent — required for all key outcomes.
   If missing: certainty of evidence cannot be evaluated.
```

</prisma_compliance>

<search_strategy>

## Search Strategy Validation

**Boolean logic verification:**

```
Search strings must be validated for:
1. CORRECT BOOLEAN OPERATORS: AND narrows, OR broadens.
   Common error: Using AND where OR is needed within concept blocks.
   Example error: (diabetes AND type 2 AND type 1) — excludes studies mentioning only one type.
   Correct: (diabetes AND (type 2 OR type 1)) — captures both.

2. TRUNCATION: Wildcard placement must be appropriate.
   "random*" captures randomized, randomisation, randomization, randomly.
   But "random?" may miss terms depending on database wildcard rules.

3. PROXIMITY OPERATORS: NEAR/n, ADJ/n vary by database.
   PubMed does not support proximity operators — flag if used in MEDLINE via PubMed.

4. NESTING: Parentheses must be balanced and logically grouped by concept.
   Verification: Parse the Boolean tree. Each concept block should be
   connected with AND. Within each block, synonyms connected with OR.
```

**Database coverage assessment:**

```
Minimum database requirements by review type:
- Cochrane reviews: CENTRAL, MEDLINE, Embase (minimum)
- General health: MEDLINE + at least one other (Embase, CINAHL, PsycINFO)
- Drug safety: Add WHO ICTRP, FDA Adverse Event Reporting System
- Diagnostic accuracy: Add MEDLINE, Embase, and reference standard databases

Verification:
1. COUNT databases searched. Fewer than 2 for a health topic: flag as inadequate.
2. CHECK for trial registry searches (ClinicalTrials.gov, WHO ICTRP, EU CTR).
   Absence increases risk of publication bias.
3. CHECK for grey literature sources (conference abstracts, dissertations, preprints).
   Absence may introduce publication bias but is acceptable if justified.
```

**MeSH/controlled vocabulary verification:**

```
1. VERIFY: MeSH terms used match the PICO elements.
   Use the MeSH Browser (meshb.nlm.nih.gov) to confirm term validity.
2. CHECK: Explosion — MeSH terms in MEDLINE auto-explode to include narrower terms.
   If "Diabetes Mellitus" is used without [MeSH], free-text searching misses indexed articles.
3. COMPARE: MeSH strategy vs free-text strategy. Optimal searches combine both.
   MeSH-only searches miss recently indexed articles (indexing lag ~ 2-12 weeks).
4. VERIFY: Embase uses Emtree, not MeSH. Direct copy of MEDLINE strategy to Embase
   without Emtree translation: flag as methodological error.
```

</search_strategy>

<screening_quality>

## Screening Quality Assessment

**Inter-rater reliability:**

```
Screening must involve at least two independent reviewers for:
1. Title-abstract screening
2. Full-text eligibility assessment
3. Data extraction (at minimum a random sample)

Verification:
1. CHECK: Were two reviewers used? Single-reviewer screening: flag as high risk.
2. CHECK: Was a calibration/pilot phase conducted?
   Best practice: pilot on first 50-100 records, discuss disagreements, refine criteria.
3. CHECK: How were disagreements resolved?
   Acceptable: consensus discussion, third reviewer adjudication.
   Unacceptable: single reviewer makes final decision without process.
```

**Kappa statistics for agreement:**

```
Cohen's kappa interpretation:
  kappa < 0.00: Poor (less than chance)
  0.00-0.20: Slight
  0.21-0.40: Fair
  0.41-0.60: Moderate
  0.61-0.80: Substantial
  0.81-1.00: Almost perfect

Verification:
1. COMPUTE or CHECK: Kappa for title-abstract screening.
   kappa < 0.60: screening criteria may be ambiguous — flag for review.
   kappa < 0.40: serious concern — criteria need revision and re-screening.
2. Prevalence-adjusted bias-adjusted kappa (PABAK) should be reported when
   prevalence of included studies is very low (< 5% inclusion rate), as standard
   kappa is paradoxically low with high agreement but rare events.
3. For multiple reviewers: use Fleiss' kappa (not Cohen's).
```

</screening_quality>

<risk_of_bias>

## Risk of Bias Assessment

**RoB 2 for randomized controlled trials:**

```
RoB 2 (revised Cochrane tool) assesses five domains:
1. Bias arising from the randomization process
   - Was allocation sequence random?
   - Was allocation concealed until assignment?
   - Were baseline differences suggestive of problems?

2. Bias due to deviations from intended interventions
   - Effect of assignment (intention-to-treat) vs effect of adherence
   - Were participants aware of assignment?
   - Were deviations balanced between groups?

3. Bias due to missing outcome data
   - Were outcome data available for all or nearly all participants?
   - Was evidence that result was not biased by missing data?
   - Could missingness depend on the true outcome value?

4. Bias in measurement of the outcome
   - Was the outcome measure appropriate?
   - Was the assessor aware of intervention received?
   - Were methods comparable across groups?

5. Bias in selection of the reported result
   - Was the analysis pre-specified?
   - Were multiple eligible analyses performed?
   - Were multiple eligible outcome measurements?

Verification:
1. Each domain must be rated: Low risk / Some concerns / High risk.
2. Overall judgment: High risk if ANY domain is high risk.
   Some concerns if ANY domain has some concerns and none is high risk.
3. Signal questions must be answered explicitly. Missing answers: flag as incomplete.
```

**ROBINS-I for non-randomized studies:**

```
ROBINS-I assesses seven domains:
1. Bias due to confounding
2. Bias in selection of participants
3. Bias in classification of interventions
4. Bias due to deviations from intended interventions
5. Bias due to missing data
6. Bias in measurement of outcomes
7. Bias in selection of reported result

Verification:
1. Confounding domain requires specification of critical confounders a priori.
   If confounders not listed: assessment cannot be validated.
2. Each domain rated: Low / Moderate / Serious / Critical / No information.
3. Overall: worst domain rating determines overall rating.
4. ROBINS-I is more conservative than RoB 2 — most observational studies
   should have at least "moderate" risk. Rating all domains "low": flag for review.
```

</risk_of_bias>

<grade_assessment>

## GRADE Certainty of Evidence

**GRADE evaluation framework:**

```
Starting certainty:
  RCTs start at HIGH
  Observational studies start at LOW

Five domains for downgrading (each can reduce by 1 or 2 levels):
1. Risk of bias — based on RoB 2 / ROBINS-I across studies
2. Inconsistency — heterogeneity in effects across studies (check I², prediction intervals)
3. Indirectness — population, intervention, comparator, or outcome differs from review question
4. Imprecision — wide confidence intervals, small sample size, few events
5. Publication bias — asymmetric funnel plot, missing studies

Three domains for upgrading (observational studies only):
1. Large magnitude of effect (RR > 2 or < 0.5 with no plausible confounding)
2. Dose-response gradient
3. All plausible confounding would reduce the observed effect

Final certainty levels:
  HIGH: Very confident the true effect lies close to the estimate.
  MODERATE: Moderately confident; true effect likely close but may be substantially different.
  LOW: Limited confidence; true effect may be substantially different.
  VERY LOW: Very little confidence; true effect likely substantially different.

Verification:
1. CHECK: Starting level matches study design (RCT = high, observational = low).
2. CHECK: Each downgrade decision is explicitly justified with evidence.
   Vague statements like "some concerns" without specifics: flag.
3. CHECK: Imprecision — use optimal information size (OIS).
   If total sample < OIS: downgrade for imprecision.
   OIS = sample needed for a single adequately powered trial of the same effect.
4. CHECK: Upgrading is RARE and requires strong justification.
   Upgrading observational evidence to HIGH: extremely unusual — flag for scrutiny.
```

</grade_assessment>

<data_extraction>

## Data Extraction Accuracy

**Extraction verification checklist:**

```
1. VERIFY: Extracted values match source publication.
   Common errors:
   - Confusing SD with SE (SE = SD / sqrt(n)). If SE is used as SD: effect sizes are inflated.
   - Extracting median as mean for skewed data. For skewed data, use Wan/Luo methods for conversion.
   - Using per-protocol numbers instead of ITT numbers.
   - Extracting unadjusted instead of adjusted estimates (or vice versa).

2. VERIFY: Time points are consistent across studies.
   Mixing 6-month and 12-month outcomes in the same meta-analysis: flag unless justified.

3. VERIFY: Unit consistency.
   Blood glucose in mg/dL vs mmol/L (conversion factor: 18.018).
   Body weight in kg vs lbs. Blood pressure in mmHg (should be universal).

4. VERIFY: Double data extraction was performed.
   If only single extraction: error rate estimated at 15-20% per study.
   Minimum acceptable: single extraction with independent verification of a random 20% sample.
```

**Inclusion/exclusion criteria consistency:**

```
1. VERIFY: Criteria are applied identically across all candidate studies.
   If a study is excluded for a reason that applies to an included study: flag inconsistency.

2. VERIFY: Criteria match the registered protocol.
   Post-hoc changes to eligibility criteria must be documented and justified.
   Undocumented changes suggest selective inclusion — flag as high risk.

3. VERIFY: Language restrictions are documented.
   English-only restriction may introduce language bias for certain topics.
   If English-only and non-English studies exist on the topic: acknowledge limitation.

4. VERIFY: Date restrictions are justified.
   Arbitrary date cutoffs without justification: flag.
   Acceptable justifications: intervention not available before date, guideline change.
```

</data_extraction>

## Worked Examples

### PRISMA flow diagram validation

```
Reported numbers:
  Database records: MEDLINE (450) + Embase (380) + CENTRAL (120) = 950
  After deduplication: 620
  Screened: 620
  Excluded at title-abstract: 550
  Full-text assessed: 70
  Excluded at full-text: 45 (reasons: wrong population 20, wrong intervention 15,
                                      wrong outcome 5, wrong design 5)
  Included in qualitative synthesis: 25
  Included in meta-analysis: 18

Verification:
1. Arithmetic: 620 - 550 = 70 (full-text assessed). CORRECT.
2. 70 - 45 = 25 (included). CORRECT.
3. Exclusion reasons sum: 20 + 15 + 5 + 5 = 45. CORRECT.
4. Deduplication rate: (950 - 620) / 950 = 34.7%.
   Typical MEDLINE-Embase overlap: 30-50%. 34.7% is PLAUSIBLE.
5. Inclusion rate: 25 / 620 = 4.0%. Typical for focused clinical questions.
   If inclusion rate > 20%: search may be too narrow.
   If inclusion rate < 1%: search may be too broad or criteria too restrictive.
```

### Kappa calculation for screening agreement

```python
# Two reviewers screened 200 abstracts
# Reviewer A included 30, Reviewer B included 35
# Both agreed to include: 25, both agreed to exclude: 160
# A include / B exclude: 5, A exclude / B include: 10

a, b, c, d = 25, 5, 10, 160  # 2x2 table
n = a + b + c + d  # 200

p_observed = (a + d) / n  # (25 + 160) / 200 = 0.925
p_A_yes = (a + b) / n     # 30 / 200 = 0.15
p_B_yes = (a + c) / n     # 35 / 200 = 0.175
p_expected = p_A_yes * p_B_yes + (1 - p_A_yes) * (1 - p_B_yes)
# p_expected = 0.15 * 0.175 + 0.85 * 0.825 = 0.02625 + 0.70125 = 0.7275

kappa = (p_observed - p_expected) / (1 - p_expected)
# kappa = (0.925 - 0.7275) / (1 - 0.7275) = 0.1975 / 0.2725 = 0.725

# kappa = 0.725 -> Substantial agreement. Acceptable for screening.
# If kappa < 0.60: revise eligibility criteria and re-pilot.
```
