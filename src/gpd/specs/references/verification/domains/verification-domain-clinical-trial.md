---
load_when:
  - "clinical trial verification"
  - "CONSORT compliance"
  - "randomization integrity"
  - "blinding adequacy"
  - "intention to treat"
  - "sample size calculation"
  - "protocol deviation"
  - "RCT quality"
tier: 2
context_cost: large
---

# Verification Domain — Clinical Trial

CONSORT compliance, randomization integrity, blinding adequacy, ITT vs per-protocol analysis, sample size calculations, outcome pre-specification, and protocol deviation handling for clinical trials.

**Load when:** Working on RCT methodology, CONSORT reporting, randomization assessment, blinding evaluation, sample size justification, or protocol compliance.

**Related files:**
- `../core/verification-quick-reference.md` — compact checklist (default entry point)
- `../core/verification-core.md` — foundational verification principles
- `references/verification/domains/verification-domain-systematic-review.md` — systematic review (for RoB 2 assessment)
- `references/verification/domains/verification-domain-statistics.md` — statistical methods (for power and analysis)
- `references/verification/domains/verification-domain-meta-analysis.md` — meta-analysis (for pooling trial results)

---

<consort_compliance>

## CONSORT Compliance Checking

**25-item checklist (CONSORT 2010, updated extensions available):**

```
Title and Abstract:
  Item 1a: Identification as a randomised trial in the title.
  Item 1b: Structured summary of trial design, methods, results, conclusions.
  Verification: Title MUST contain "randomised" or "randomized".
  Absence in title: automatic CONSORT violation.

Introduction:
  Item 2a: Scientific background and explanation of rationale.
  Item 2b: Specific objectives or hypotheses.

Methods:
  Item 3a: Description of trial design (parallel, crossover, factorial, etc.)
           including allocation ratio.
  Item 3b: Important changes to methods after trial commencement, with reasons.
  Item 4a: Eligibility criteria for participants.
  Item 4b: Settings and locations where data were collected.
  Item 5: Interventions for each group with sufficient details to allow replication.
  Verification: TIDieR checklist compliance for intervention description.
  Item 6a: Completely defined pre-specified primary and secondary outcome measures,
           including how and when they were assessed.
  Item 6b: Any changes to trial outcomes after the trial commenced, with reasons.
  Item 7a: How sample size was determined.
  Item 7b: When applicable, explanation of any interim analyses and stopping guidelines.
  Item 8a: Method used to generate the random allocation sequence.
  Item 8b: Type of randomisation; details of any restriction (e.g., blocking, stratification).
  Item 9: Mechanism used to implement the random allocation sequence, describing
          steps to conceal the sequence until interventions were assigned.
  Item 10: Who generated the sequence, who enrolled participants, who assigned to interventions.
  Item 11a: Blinding — who was blinded after assignment (participants, care providers,
            outcome assessors).
  Item 11b: Description of similarity of interventions if blinded.
  Item 12a: Statistical methods used to compare groups for primary and secondary outcomes.
  Item 12b: Methods for additional analyses (subgroup, adjusted).

Results:
  Item 13a: Flow of participants through each stage (CONSORT flow diagram MANDATORY).
  Item 13b: For each group, losses and exclusions after randomisation, together with reasons.
  Item 14a: Dates defining periods of recruitment and follow-up.
  Item 14b: Why the trial ended or was stopped.
  Item 15: Baseline table showing demographics and clinical characteristics for each group.
  Item 16: Number of participants analysed in each group and whether analysis was ITT.
  Item 17a: For each primary and secondary outcome, results for each group with
            effect size and precision (e.g., 95% CI).
  Item 17b: For binary outcomes, presentation of both absolute and relative effect sizes.
  Item 18: Results of any other analyses (subgroup, adjusted), distinguishing
           pre-specified from exploratory.
  Item 19: All important harms or unintended effects in each group.

Discussion:
  Item 20: Trial limitations, addressing sources of potential bias, imprecision.
  Item 21: Generalisability (external validity).
  Item 22: Interpretation consistent with results, balancing benefits and harms.

Other:
  Item 23: Registration number and name of trial registry.
  Item 24: Where the full trial protocol can be accessed.
  Item 25: Sources of funding and other support, role of funders.
```

**Critical CONSORT violations to flag:**

```
1. No flow diagram (Item 13a) — most important single item.
2. No trial registration (Item 23) — raises concern about selective reporting.
3. Sample size calculation absent (Item 7a) — cannot assess if trial was adequately powered.
4. Primary outcome changed post-hoc (Item 6b) — requires ClinicalTrials.gov history check.
5. Baseline table (Item 15) with P-values for between-group comparisons:
   This is WRONG — randomisation makes baseline significance testing inappropriate.
   Baseline differences should be assessed clinically, not statistically.
```

</consort_compliance>

<randomization_integrity>

## Randomization Integrity

**Allocation sequence generation:**

```
Acceptable methods:
- Computer-generated random numbers
- Random number tables
- Minimization (with a random element)
- Stratified block randomization

Unacceptable methods:
- Alternation (every other patient)
- Date of birth (odd/even)
- Hospital record number
- Day of the week
- Clinician preference

Verification:
1. CHECK: Method of sequence generation is explicitly stated.
   "Patients were randomized" without details: insufficient — flag.
2. CHECK: Block size disclosed? Fixed blocks of 2 are predictable.
   Variable block sizes (e.g., 4, 6, 8) are preferred.
   If block size equals number of strata cells: possible unmasking.
3. CHECK: Stratification factors are clinically justified and limited in number.
   Rule of thumb: no more than 2-3 stratification factors.
   Over-stratification with small sample: incomplete blocks, imbalance.
```

**Allocation concealment:**

```
Adequate concealment methods:
- Central randomization (telephone, web-based)
- Sequentially numbered, opaque, sealed envelopes (SNOSE)
- Pharmacy-controlled allocation
- Identical containers with coded labels

Inadequate concealment methods:
- Open random number table
- Alternation
- Unsealed envelopes
- Assignment envelopes that are not opaque (can be read with backlight)

Verification:
1. CHECK: Concealment method is explicitly described. Missing = unclear risk.
2. CHECK for cluster trials: Was concealment maintained at the cluster level?
   Individual concealment in cluster RCTs is often impossible — method must address this.
3. COMPUTE: Baseline imbalance test. If groups differ substantially on key prognostic
   factors despite randomization: possible concealment failure.
   Use standardized differences (> 0.1 is a flag, > 0.25 is concerning).
```

</randomization_integrity>

<blinding_adequacy>

## Blinding Adequacy

**Assessment of blinding for each stakeholder:**

```
Participant blinding:
- Can participants distinguish treatment from placebo?
- For drug trials: identical appearance, taste, smell, packaging.
- For surgical trials: sham procedures must be ethically justified.
- For behavioral interventions: blinding often impossible — must acknowledge.

Personnel blinding (care providers):
- Do care providers know the allocation?
- Potential impact: differential co-interventions, encouragement, monitoring.
- In open-label trials: assess if co-interventions were balanced.

Outcome assessor blinding:
- Is the outcome subjective or objective?
  Subjective outcomes (pain scores, quality of life): blinding CRITICAL.
  Objective outcomes (mortality, lab values): blinding less critical but still preferred.
- Were outcome assessors independent from care providers?

Verification:
1. CHECK: Blinding status explicitly stated for each of the three groups.
   "Double-blind" without specifying who: insufficient — flag.
   "Double-blind" typically means participant + care provider OR participant + assessor.
   "Triple-blind" adds data analysts. Confirm what was meant.
2. CHECK: Was blinding tested? (asking participants to guess allocation)
   James blinding index or Bang blinding index can quantify blinding success.
   If > 75% correctly guessed their allocation: blinding may be broken.
3. CHECK: For open-label trials, were outcomes adjudicated by a blinded committee?
   Open-label with subjective primary outcome and no blinded adjudication: HIGH risk of bias.
```

**Blinding assessment by outcome type:**

```
Hard outcomes (mortality, myocardial infarction with objective criteria):
  Low risk even without blinding, provided ascertainment is complete.

Soft outcomes (symptom scales, clinician-rated severity, function):
  High risk without assessor blinding. Expect effect inflation of 0.2-0.5 SMD
  in unblinded vs blinded assessment of subjective outcomes.

Patient-reported outcomes (PROs):
  Require participant blinding to be trustworthy.
  If unblinded: expect Hawthorne effect and placebo response to inflate differences.

Verification:
1. MATCH: Each primary outcome with the appropriate blinding requirement.
2. FLAG: Subjective primary outcome + open-label design = high risk of performance/detection bias.
3. CHECK: Were there any emergency unblindings? Rate and balance between groups.
   Unblinding rate > 10%: may compromise integrity.
```

</blinding_adequacy>

<analysis_approach>

## ITT vs Per-Protocol Analysis

**Intention-to-treat (ITT) principle:**

```
Definition: All randomized participants are analyzed in the group to which they were
assigned, regardless of actual treatment received, adherence, or withdrawal.

Verification:
1. CHECK: Denominator in primary analysis = number randomized per group.
   If denominator < randomized: not true ITT — check what was excluded.
2. CHECK: "Modified ITT" (mITT) — common but problematic.
   mITT typically excludes patients who never received treatment or had no post-baseline data.
   If mITT is used: verify exclusion criteria are pre-specified and balanced between groups.
   Exclusion of > 5% of randomized patients from mITT: flag.
3. CHECK: How were missing outcomes handled in the ITT analysis?
   - Complete case analysis: biased if missingness is related to outcome.
   - Last observation carried forward (LOCF): OUTDATED, biased — flag if used as primary.
   - Multiple imputation: preferred if properly implemented.
   - Mixed-effects models: handle missing data under MAR assumption.
```

**Per-protocol analysis:**

```
Definition: Analyzes only participants who completed the protocol as intended.

Role: Supportive analysis for efficacy (what happens under ideal adherence).
       Not suitable as primary analysis for superiority trials.
       For non-inferiority/equivalence trials: BOTH ITT and per-protocol should be primary.

Verification:
1. CHECK: Per-protocol exclusion criteria defined a priori.
2. CHECK: Proportion excluded from per-protocol.
   > 20% excluded: results may not represent the randomized population.
3. CHECK: Reasons for protocol violation are balanced between groups.
   Differential dropout (e.g., more adverse event withdrawals in treatment arm):
   per-protocol analysis will be biased in favor of treatment.
4. COMPARE: ITT and per-protocol results. If they diverge substantially:
   investigate whether adherence or dropout patterns explain the difference.
   Concordant results increase confidence.
```

</analysis_approach>

<sample_size>

## Sample Size Adequacy and Power Calculations

**Elements of a valid sample size calculation:**

```
Required components:
1. Primary outcome specified
2. Expected effect size (minimum clinically important difference — MCID)
3. Expected variability (SD for continuous, event rate for binary)
4. Significance level (alpha, typically 0.05 two-sided)
5. Power (1 - beta, typically 0.80 or 0.90)
6. Allowance for dropout/loss to follow-up
7. Statistical test to be used

Verification:
1. CHECK: All seven components are stated.
   Missing MCID: cannot assess if trial was designed to detect a clinically meaningful effect.
2. CHECK: MCID is clinically reasonable.
   For patient-reported outcomes: published MCIDs exist for most validated instruments.
   If claimed effect size exceeds typical MCIDs by > 2x: may be unrealistic.
3. CHECK: Variability estimate is sourced (pilot study, literature, clinical experience).
   Underestimating SD leads to underpowered trials.
4. CHECK: Dropout allowance is realistic.
   If 10% dropout assumed but actual dropout is 30%: trial is underpowered.
```

**Post-hoc power analysis:**

```
IMPORTANT: Post-hoc (observed) power analysis is INVALID and should be flagged.
Observed power is a direct transformation of the P-value and provides no new information.
If P > 0.05 and authors compute "observed power = 30%": this is tautological, not informative.

Valid approach for negative trials:
1. Report the confidence interval — it shows the range of effects compatible with the data.
2. Assess if the CI excludes the MCID — if the upper bound of the CI is below the MCID,
   the trial can conclude the treatment effect is smaller than clinically meaningful.
3. Report conditional power for futility in adaptive designs.

Verification:
1. FLAG: Any post-hoc power calculation presented to "explain" a negative result.
2. CHECK: For negative trials, is the 95% CI presented and interpreted relative to MCID?
3. CHECK: Was the trial adequately powered for the observed control group event rate?
   If control event rate was much lower than expected: trial may be underpowered
   even if sample size met the original calculation.
```

</sample_size>

<outcome_specification>

## Primary/Secondary Outcome Pre-specification

**Verification against trial registration:**

```
1. OBTAIN: Original registration record from ClinicalTrials.gov, ISRCTN, or equivalent.
2. COMPARE: Primary outcome(s) in the publication vs registration.
   Changes from registration to publication:
   - Outcome switched from secondary to primary: SELECTIVE REPORTING — flag.
   - Outcome definition changed (e.g., 30-day to 90-day mortality): flag.
   - Composite outcome components changed: flag.
   - Analysis method changed (e.g., continuous to dichotomized): flag.
3. CHECK: ClinicalTrials.gov history of changes tab shows all amendments with dates.
   Changes made AFTER enrollment began are particularly concerning.

Common selective reporting patterns:
- Registered primary outcome is negative → reported as secondary or not reported.
- Multiple secondary outcomes registered → only significant ones reported.
- Composite outcome reported when individual components are not significant.
- Time point for assessment changed to one that showed significance.
```

**Outcome measurement validity:**

```
1. CHECK: Validated measurement instrument used?
   For PROs: psychometric properties (reliability, validity, responsiveness) documented.
2. CHECK: Measurement timing consistent across groups.
3. CHECK: Outcome assessor training documented.
4. CHECK: For composite outcomes:
   - Clinical coherence (components should have similar importance to patients).
   - Direction of effect should be consistent across components.
   - Dominant component analysis: if one component drives the composite, report it.
   - Competing risks: mortality in composite prevents observation of non-fatal events.
```

</outcome_specification>

<protocol_deviations>

## Protocol Deviation Handling

**Classification and impact assessment:**

```
Major deviations (affect primary endpoint or safety):
- Wrong treatment administered
- Eligibility criteria violated (enrolled ineligible patient)
- Unblinding of treatment allocation
- Missed primary outcome assessment
- Prohibited concomitant medication affecting primary endpoint

Minor deviations (procedural, unlikely to affect outcome):
- Slight timing variations in assessments (within predefined window)
- Administrative errors not affecting treatment or outcome
- Minor documentation deficiencies

Verification:
1. CHECK: Deviations are classified and counted per group.
   If deviations are unbalanced between groups: investigate if treatment-related.
2. CHECK: Impact assessment for each major deviation.
3. CHECK: Were deviation handling rules pre-specified in the protocol/SAP?
4. CHECK: Were ICH E9(R1) estimand considerations addressed?
   Intercurrent events (treatment discontinuation, rescue medication, death)
   must be handled according to a pre-specified strategy:
   - Treatment policy: analyze regardless of intercurrent event (ITT-like)
   - Composite: combine with intercurrent event as part of outcome
   - Hypothetical: estimate effect if intercurrent event had not occurred
   - While-on-treatment: analyze data up to intercurrent event
   - Principal stratum: analyze subgroup that would not experience the event
```

**GCP compliance verification:**

```
1. CHECK: Independent data safety monitoring board (DSMB) in place for:
   - Trials with mortality/serious morbidity outcomes
   - Trials with vulnerable populations
   - Trials with potential for early harm signal
2. CHECK: Protocol amendments documented with dates and regulatory approval.
3. CHECK: Informed consent process described.
4. CHECK: Adverse event reporting completeness.
   Reporting only "treatment-related" AEs: biased — flag.
   All AEs should be reported with relatedness assessment.
```

</protocol_deviations>

## Worked Examples

### Detecting outcome switching via registry comparison

```
Published trial reports:
  Primary outcome: "Composite of cardiovascular death, MI, stroke at 12 months"
  Result: HR 0.82, 95% CI 0.68-0.99, P = 0.04

ClinicalTrials.gov original registration:
  Primary outcome: "Cardiovascular death at 24 months"
  Secondary: "Composite of CV death, MI, stroke at 24 months"

Verification:
1. Primary outcome SWITCHED from single (CV death) to composite. FLAG.
2. Time point CHANGED from 24 months to 12 months. FLAG.
3. Check history tab: amendment date AFTER first patient enrolled. FLAG.
4. Conclusion: HIGH risk of selective outcome reporting.
   The original primary outcome (CV death alone at 24 months) should be
   requested from authors and evaluated as the true primary analysis.
```

### Sample size verification for a binary outcome trial

```python
from scipy.stats import norm
import math

# Published sample size justification:
# "We estimated 500 patients per group to detect a 10% absolute difference
#  (30% vs 20% event rate) with 80% power at alpha = 0.05 two-sided."

p1, p2 = 0.30, 0.20  # control vs treatment event rates
alpha = 0.05
power = 0.80
z_alpha = norm.ppf(1 - alpha / 2)  # 1.96
z_beta = norm.ppf(power)           # 0.842

# Standard formula for two proportions
p_bar = (p1 + p2) / 2
n = ((z_alpha * math.sqrt(2 * p_bar * (1 - p_bar)) +
      z_beta * math.sqrt(p1 * (1 - p1) + p2 * (1 - p2))) / (p1 - p2)) ** 2

print(f"Required n per group: {math.ceil(n)}")
# n ~ 294 per group (without continuity correction)
# n ~ 313 per group (with continuity correction)

# Published claim of 500 per group includes ~60% inflation for dropout.
# If actual dropout < 30%, 500 is adequate.
# If actual dropout > 40%, trial is likely underpowered for the stated effect.
```
