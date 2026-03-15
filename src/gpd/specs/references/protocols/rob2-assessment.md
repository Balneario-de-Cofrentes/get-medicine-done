---
load_when:
  - "risk of bias"
  - "RoB 2"
  - "RoB2"
  - "bias assessment"
  - "Cochrane risk of bias"
  - "randomization bias"
  - "attrition bias"
  - "detection bias"
  - "reporting bias"
  - "selection bias"
  - "signalling questions"
tier: 1
context_cost: medium
---

# Cochrane RoB 2 Tool: Risk of Bias Assessment for Randomized Trials

The Cochrane Risk of Bias 2 (RoB 2) tool is the standard instrument for assessing risk of bias in randomized controlled trials included in systematic reviews. It replaced the original Cochrane RoB tool with a more structured, domain-based approach using signalling questions that lead to algorithmic judgments.

**Core principle:** RoB 2 assesses the risk that the trial result is biased, not the quality of the trial report. Poor reporting may indicate high risk, but risk of bias judgments should be based on what was done, not just what was described. Contact study authors when reporting is unclear.

## Related Protocols

- `consort-rct.md` -- CONSORT items map directly to RoB 2 domains
- `grade-certainty.md` -- RoB 2 judgments feed into GRADE Domain 1 (risk of bias)
- `robins-i-assessment.md` -- Equivalent tool for non-randomised studies

---

## Structure of RoB 2

RoB 2 assesses bias for a specific **result** of a specific **outcome** in a trial. Different outcomes within the same trial may have different risk of bias judgments.

Two variants exist:

| Variant | Analysis Type | Use When |
|---------|--------------|----------|
| RoB 2 for ITT/mITT | Intention-to-treat or modified ITT effect | Assessing the effect of assignment to intervention |
| RoB 2 for per-protocol | Per-protocol or as-treated effect | Assessing the effect of adhering to intervention |

The choice depends on the effect of interest in the systematic review, not on what the trial reported.

---

## Five Domains

### Domain 1: Risk of bias arising from the randomization process

**Signalling questions:**

1.1 Was the allocation sequence random?
- Yes: computer-generated random numbers, random number table with concealment.
- Probably yes: described as "random" without details, but allocation was concealed.
- No: alternation, date of birth, hospital number.
- No information.

1.2 Was the allocation sequence concealed until participants were enrolled and assigned to interventions?
- Yes: central telephone/web-based randomisation, sequentially numbered opaque sealed envelopes.
- Probably yes: envelopes described, but no mention of opaque/sealed.
- No: open random number table, envelopes that were not sealed/opaque.
- No information.

1.3 Did baseline differences between intervention groups suggest a problem with the randomization process?
- Yes: large or prognostically important imbalances unlikely due to chance.
- Probably yes: some potentially important imbalances.
- No: groups appear well balanced on known prognostic factors.
- No information.

**Judgment algorithm:**

```
If 1.1 = Yes/PY AND 1.2 = Yes/PY AND 1.3 = No/PN → Low risk
If 1.1 = No OR 1.2 = No → High risk
If 1.3 = Yes → High risk (irrespective of 1.1 and 1.2)
Otherwise → Some concerns
```

### Domain 2: Risk of bias due to deviations from intended interventions

**For the effect of assignment (ITT):**

2.1 Were participants aware of their assigned intervention during the trial?
2.2 Were carers and people delivering the interventions aware of participants' assigned intervention during the trial?
2.3 If Y/PY/NI to 2.1 or 2.2: Were there deviations from the intended intervention that arose because of the trial context?
2.4 If Y/PY to 2.3: Were these deviations likely to have affected the outcome?
2.5 If Y/PY to 2.4: Were these deviations from intended intervention balanced between groups?
2.6 Was an appropriate analysis used to estimate the effect of assignment to intervention?
2.7 If N/PN/NI to 2.6: Was there potential for a substantial impact (on the result) of the failure to analyse participants in the group to which they were randomized?

**Key consideration:** For the ITT effect, deviations that arise in usual practice (outside the trial context) do not constitute bias. Only deviations that arise BECAUSE of the trial context (e.g., unblinding leading to differential co-interventions) are relevant.

**Judgment algorithm:**

```
If participants/carers blinded AND appropriate analysis → Low risk
If deviations arose but were balanced between groups → Low risk
If deviations arose, were unbalanced, and likely affected outcome → High risk
If inappropriate analysis with potential substantial impact → High risk
Otherwise → Some concerns
```

**For the per-protocol effect:**

2.1 Were participants aware of their assigned intervention during the trial?
2.2 Were carers and people delivering the interventions aware of participants' assigned intervention?
2.3 [If Y/PY/NI to 2.1 or 2.2]: Were important non-protocol interventions balanced across intervention groups?
2.4 [If N/PN/NI to 2.3]: Were there failures in implementing the intervention that could have affected the outcome?
2.5 [If Y/PY/NI to 2.4]: Was an appropriate analysis used to estimate the effect of adhering to the intervention?

### Domain 3: Risk of bias due to missing outcome data

**Signalling questions:**

3.1 Were data for this outcome available for all, or nearly all, participants randomized?
- "Nearly all" = > 95% of randomized participants have outcome data.

3.2 If N/PN to 3.1: Is there evidence that the result was not biased by missing outcome data?
- Evidence from: analysis methods accounting for missing data (e.g., multiple imputation under MAR), sensitivity analyses showing robustness, reasons for missingness unrelated to the outcome.

3.3 If N/PN to 3.2: Could missingness in the outcome depend on its true value?
- Consider whether dropout might be related to treatment failure, adverse events, or early recovery.

3.4 If Y/PY to 3.3: Is it likely that missingness in the outcome depended on its true value?

**Judgment algorithm:**

```
If data available for nearly all (>95%) → Low risk
If evidence that result not biased (e.g., sensitivity analysis) → Low risk
If missingness could not depend on true value → Low risk
If missingness likely depends on true value → High risk
Otherwise → Some concerns
```

**Practical thresholds:**

| Missing data | Risk Level | Condition |
|-------------|------------|-----------|
| < 5% | Usually low | If balanced between groups |
| 5-20% | Some concerns to high | Depends on reasons and differential missingness |
| > 20% | Usually high | Unless strong sensitivity analysis demonstrates robustness |

### Domain 4: Risk of bias in measurement of the outcome

**Signalling questions:**

4.1 Was the method of measuring the outcome inappropriate?
- Consider validity and reliability of the measurement tool.

4.2 Could measurement or ascertainment of the outcome have differed between intervention groups?
- Consider whether the same measurement protocol was used across groups.

4.3 If N/PN/NI to 4.2: Were outcome assessors aware of the intervention received by study participants?
- Blinded outcome assessment prevents this bias.

4.4 If Y/PY/NI to 4.3: Could assessment of the outcome have been influenced by knowledge of intervention received?
- Objective outcomes (death, laboratory values) are less susceptible.
- Subjective outcomes (pain, functional status, clinician-assessed) are highly susceptible.

4.5 If Y/PY/NI to 4.4: Is it likely that assessment of the outcome was influenced by knowledge of intervention received?

**Judgment algorithm:**

```
If outcome measurement appropriate AND assessors blinded → Low risk
If outcome measurement appropriate AND outcome is objective → Low risk
If assessors unblinded AND subjective outcome AND likely influenced → High risk
Otherwise → Some concerns
```

**Outcome susceptibility:**

| Outcome Type | Susceptibility to Detection Bias | Examples |
|-------------|--------------------------------|----------|
| All-cause mortality | Very low | Death certificate |
| Laboratory values | Low | HbA1c, serum creatinine |
| Clinical events (adjudicated) | Low to moderate | MI (adjudicated by blinded committee) |
| Clinician-assessed | High | Disease activity scores, response criteria |
| Patient-reported | Moderate to high | Pain scales, quality of life |

### Domain 5: Risk of bias in selection of the reported result

**Signalling questions:**

5.1 Were the data that produced this result analysed in accordance with a pre-specified analysis plan that was finalized before unblinded outcome data were available for analysis?
- Check the trial registration (ClinicalTrials.gov) and statistical analysis plan.

5.2 [If applicable:] Is the numerical result being assessed likely to have been selected, on the basis of the results, from multiple eligible outcome measurements?
- Multiple measurement tools for the same outcome (e.g., three different depression scales).
- Multiple time points.

5.3 [If applicable:] Is the numerical result being assessed likely to have been selected, on the basis of the results, from multiple eligible analyses of the data?
- Different analysis populations (ITT vs. per-protocol).
- Different methods for handling missing data.
- Adjusted vs. unadjusted analyses.

**Judgment algorithm:**

```
If analysed per pre-specified plan AND result not likely selected → Low risk
If no pre-specified plan OR multiple eligible measurements/analyses with result likely selected → High risk
Otherwise → Some concerns
```

**Red flags for selective reporting:**

- Primary outcome changed from registration to publication.
- Multiple outcomes measured but only favorable ones reported.
- Subgroup analyses reported without pre-specification.
- Unexplained discrepancies between protocol and published report.

---

## Overall Risk of Bias Judgment

| Domain Judgments | Overall Judgment |
|-----------------|-----------------|
| All domains: Low risk | **Low risk of bias** |
| Some concerns in at least one domain, but no high risk | **Some concerns** |
| High risk in at least one domain | **High risk of bias** |
| Some concerns in multiple domains, increasing likelihood that the result is biased | **High risk of bias** |

**The last rule is important:** Multiple "some concerns" across domains can compound to create an overall high risk of bias, even when no single domain is judged high risk.

---

## Presenting RoB 2 Results

### Traffic Light Plot

Show domain-level judgments for each study using the standard color coding:

| Color | Judgment |
|-------|----------|
| Green | Low risk |
| Yellow | Some concerns |
| Red | High risk |

### Summary Bar Chart

Show the proportion of studies at each risk of bias level for each domain. Weight by the contribution of each study to the meta-analysis (by sample size or inverse variance weight).

### Tools

- **robvis:** R package and Shiny app for creating RoB 2 visualizations (https://www.riskofbias.info/welcome/robvis-visualization-tool).
- **The official RoB 2 Excel template** from the Cochrane Methods group.

---

## Mapping RoB 2 to GRADE

When moving from study-level RoB 2 judgments to the GRADE body-of-evidence assessment:

1. Weight each study's contribution (by sample size or inverse variance weight in the meta-analysis).
2. If most of the weight comes from studies at low risk of bias: no downgrade.
3. If most of the weight comes from studies with some concerns: consider downgrading one level.
4. If most of the weight comes from high risk studies, or if bias likely changes the effect direction: downgrade one or two levels.

Consider conducting sensitivity analysis excluding high risk of bias studies to assess the robustness of the pooled estimate.

---

## Verification Checklist

Before finalizing RoB 2 assessments:

- [ ] RoB 2 variant selected matches the effect of interest (assignment vs. adherence)
- [ ] Assessment performed for each outcome, not just per study
- [ ] All signalling questions answered with supporting justification
- [ ] Domain judgments follow the algorithmic guidance
- [ ] Overall judgment accounts for compounding of multiple "some concerns"
- [ ] Trial registrations checked for selective reporting (Domain 5)
- [ ] Two independent assessors with consensus or third reviewer resolution
- [ ] Traffic light plot and summary bar chart prepared
- [ ] RoB 2 results mapped to GRADE risk of bias domain
