---
load_when:
  - "narrative synthesis"
  - "qualitative synthesis"
  - "SWiM"
  - "vote counting"
  - "harvest plot"
  - "textual narrative"
  - "synthesis without meta-analysis"
  - "cannot pool"
  - "too heterogeneous"
tier: 2
context_cost: medium
---

# Narrative Synthesis Protocol (SWiM Guidance)

When meta-analysis is not possible or not appropriate, narrative synthesis provides a structured alternative for synthesizing evidence. SWiM (Synthesis Without Meta-analysis) reporting guideline ensures that narrative syntheses are transparent and reproducible rather than subjective and opaque.

**Core principle:** Narrative synthesis is not a lesser form of evidence synthesis -- it is a different form that requires its own rigorous methodology. "We could not perform meta-analysis, so we described the results" is not acceptable.

## Related Protocols

- `prisma-systematic-review.md` -- PRISMA Item 13c (synthesis methods)
- `statistical-analysis.md` -- When meta-analysis IS appropriate
- `grade-certainty.md` -- GRADE can be applied to narrative syntheses
- `data-extraction.md` -- Data extraction for narrative synthesis

---

## When to Use Narrative Synthesis Instead of Meta-Analysis

| Situation | Why Meta-Analysis Is Inappropriate | Alternative Approach |
|-----------|-----------------------------------|---------------------|
| Interventions too diverse | Combining fundamentally different interventions produces a meaningless average | Group by intervention type; synthesize narratively within groups |
| Outcomes measured differently with no common metric | Cannot calculate a common effect size | Describe direction and magnitude across studies |
| Study designs too heterogeneous | Different designs have different biases | Synthesize by design; discuss consistency |
| Insufficient data for effect size calculation | Missing means, SDs, or event counts | Use direction of effect and statistical significance |
| Single study per comparison | Nothing to pool | Present the individual study result with context |
| Conceptually inappropriate | The question is about the range of effects, not the average | Map the evidence space |

**Decision rule:** If studies can reasonably be said to estimate the same underlying quantity (even with some variation), meta-analysis is appropriate. If they estimate fundamentally different quantities, narrative synthesis is needed.

---

## SWiM Reporting Guideline: 9 Items

### Item 1: Grouping Studies for Synthesis

**Report:** How studies were grouped for synthesis and the rationale for the grouping.

**Guidance:**

- Group by: intervention type, population subgroup, outcome domain, study design, or comparison.
- Grouping should be pre-specified in the protocol.
- Each group should address a coherent question.
- One study may appear in multiple groups if it addresses multiple questions.

**Example:**
```
Studies were grouped by intervention type: (1) cognitive behavioral therapy (k=8),
(2) mindfulness-based interventions (k=5), (3) psychoeducation (k=3). Within each
group, studies were further organized by comparator (active control vs. waitlist).
```

### Item 2: Describe the Standardized Metric

**Report:** A description of the standardized metric used to compare findings across studies, and the rationale.

**Options when quantitative effect sizes cannot be calculated:**

| Metric | Description | Appropriate When |
|--------|-------------|-----------------|
| Direction of effect | Categorize as positive, negative, or no effect | Minimal quantitative data available |
| p-value | Statistical significance (with caveats) | Consistent sample sizes across studies |
| Effect size (partial) | Standardized effect size from some studies | Some but not all studies provide sufficient data |
| Vote counting based on direction | Count studies showing benefit vs. harm | Cannot calculate effect sizes; at least 2 studies |

**Note:** Simple vote counting based on statistical significance is NOT recommended (biased by sample size differences). Vote counting based on direction of effect is acceptable (Cochrane Handbook Chapter 12).

### Item 3: Describe the Synthesis Method

**Report:** A description of the methods used to synthesize the results.

**Methods for narrative synthesis:**

| Method | Description | When to Use |
|--------|-------------|-------------|
| **Structured tabulation** | Organize results in a structured table showing key features and findings | Always (baseline method) |
| **Vote counting by direction** | Count studies with positive, negative, or null effects; apply sign test | When effect sizes unavailable |
| **Harvest plot** | Visual display showing direction, sample size, and quality for each study | Multiple studies with varied methods |
| **Albatross plot** | Plot p-values against sample sizes with effect size contours | When only p-values and sample sizes available |
| **Effect direction plot** | Arrow-based visualization of effect direction across outcomes | Multiple outcomes per study |
| **Combining p-values** | Fisher's or Stouffer's method for combining p-values | When effect sizes unavailable but p-values are |

### Item 4: Describe How Studies Were Ordered or Prioritized

**Report:** Any criteria used to prioritize studies for the synthesis, including the rationale.

**Prioritization criteria:**

- Risk of bias: prioritize low risk of bias studies
- Sample size: give more weight to larger studies
- Directness: prioritize studies most closely matching the PICO
- Recency: prioritize more recent studies (when methods or context have changed)
- Design: prioritize RCTs over observational studies (for intervention questions)

### Item 5: Describe Sources of Heterogeneity

**Report:** Sources of variation explored between studies, including the rationale.

- Pre-specify potential effect modifiers in the protocol.
- Organize the investigation using the same variables planned for subgroup analysis.
- Consider: population characteristics, intervention features, setting, comparator, risk of bias, outcome measurement.

### Item 6: Describe Methods to Assess Certainty

**Report:** Methods used to assess the certainty of the synthesis findings.

- GRADE can be applied to narrative syntheses.
- The five GRADE domains still apply; imprecision is assessed based on the range and direction of effects rather than confidence interval width.
- State explicitly which GRADE domains led to downgrading.

### Item 7: Describe Limits to the Synthesis

**Report:** A description of why full quantitative synthesis (meta-analysis) was not possible, and the limitations of the narrative approach used.

- Be specific about what prevented meta-analysis.
- Acknowledge limitations of the alternative method (e.g., vote counting ignores effect magnitude).
- Discuss how these limitations affect confidence in the findings.

### Item 8: Present Results

**Report:** For each synthesis, present the standardized metric alongside study identifiers and study characteristics.

**Structured results table example:**

```
| Study | N | Population | Intervention | Outcome | Direction | Magnitude | p-value | RoB |
|-------|---|-----------|-------------|---------|-----------|-----------|---------|-----|
| Smith 2020 | 150 | Adults, MDD | CBT 12wk | PHQ-9 | + | Large (d=0.8) | <0.001 | Low |
| Jones 2019 | 80 | Adults, MDD | CBT 8wk | BDI-II | + | Medium (d=0.5) | 0.02 | Some |
| Lee 2021 | 200 | Elderly, MDD | CBT 16wk | HAMD | + | Small (d=0.3) | 0.04 | High |
```

**Organize by grouping:** Present results within each pre-specified group, then discuss patterns across groups.

### Item 9: Report Limitations of the Synthesis

**Report:** Describe the limitations of the synthesis method and how they affect the interpretation.

---

## Structured Narrative Synthesis Framework

### Step 1: Preliminary Synthesis

**Goal:** Organize and describe the evidence.

1. Tabulate study characteristics and results.
2. Identify patterns: Do most studies show the same direction? Are there outliers?
3. Calculate effect sizes where possible (even if formal pooling is not done).
4. Create visual displays (harvest plots, albatross plots).

### Step 2: Explore Relationships

**Goal:** Investigate why results differ across studies.

1. Compare results across pre-specified subgroups.
2. Examine whether study quality explains discrepant findings.
3. Assess whether population, intervention, or methodological differences explain variation.
4. Use structured approaches: conceptual mapping, textual descriptions of patterns.

### Step 3: Assess Robustness

**Goal:** Determine how confident you should be in the findings.

1. Consider the weight of evidence: How many studies? How large? How well-conducted?
2. Assess consistency: Do studies point in the same direction? How consistently?
3. Apply GRADE: Rate the certainty of the body of evidence.
4. Identify what additional evidence would change the conclusion.

---

## Vote Counting by Direction of Effect

When quantitative synthesis is impossible, vote counting based on direction provides a structured summary.

### Method

1. For each study-comparison-outcome, classify the direction of effect as:
   - **Positive:** Favors intervention
   - **Negative:** Favors control
   - **No effect:** Null or trivial effect

2. Count the number in each category.

3. Apply the sign test: Under the null hypothesis of no effect, the probability of observing k or more studies in one direction out of n total follows a binomial distribution with p = 0.5.

```
Sign test: p = P(X >= k | n, p=0.5)
where X ~ Binomial(n, 0.5)
```

4. **Visualize with an effect direction plot** showing arrows for each study grouped by outcome.

### Limitations

- Ignores the magnitude of effects.
- Ignores the precision of individual studies.
- Large studies and small studies get equal weight.
- Cannot estimate the size of the effect.

Despite these limitations, vote counting by direction is recommended by Cochrane when effect sizes cannot be calculated. It is superior to simply listing results study by study.

---

## Harvest Plots

A harvest plot displays one bar per study, positioned to show the direction of effect, with height indicating sample size and shading indicating methodological quality.

```
                      Favours           No            Favours
                      control          effect       intervention
                     ─────────────────────────────────────────
Large, low RoB:                                      ████
Large, high RoB:     ████
Medium, low RoB:                                      ███
Medium, high RoB:                      ███
Small, low RoB:                                        ██
Small, high RoB:     ██                 ██
```

**Elements:**
- **Position:** Left (favours control), center (no effect), right (favours intervention)
- **Height:** Proportional to sample size
- **Shading:** Indicates risk of bias (darker = lower risk)
- **Grouping:** Organized by outcome or subgroup

---

## Combining Narrative and Quantitative Synthesis

A systematic review can combine both approaches:

1. **Quantitative synthesis (meta-analysis)** for groups of studies where pooling is appropriate.
2. **Narrative synthesis** for groups where pooling is not appropriate.
3. **Overall narrative integration** combining insights from both approaches.

This is common in mixed-methods systematic reviews or when the evidence base is diverse.

---

## Verification Checklist

Before finalizing a narrative synthesis:

- [ ] Rationale for not conducting meta-analysis explicitly stated
- [ ] Studies grouped for synthesis with pre-specified rationale
- [ ] Standardized metric selected and justified
- [ ] Synthesis method described in sufficient detail for replication
- [ ] Structured results table presented for each synthesis group
- [ ] Visual displays included (harvest plot, effect direction plot, or albatross plot)
- [ ] Sources of heterogeneity explored systematically
- [ ] Vote counting (if used) based on direction, NOT statistical significance
- [ ] Sign test applied if vote counting used
- [ ] GRADE certainty assessment applied
- [ ] Limitations of the narrative approach acknowledged
- [ ] All 9 SWiM items addressed
