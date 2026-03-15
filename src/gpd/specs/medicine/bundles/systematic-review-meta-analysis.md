---
bundle_id: systematic-review-meta-analysis
bundle_version: 1
title: Systematic Review and Meta-Analysis
summary: Evidence synthesis work with PRISMA-compliant search, GRADE certainty assessment, risk-of-bias evaluation, quantitative pooling, and narrative synthesis for heterogeneous evidence.
selection_tags:
  - "framework:systematic-review"
  - "work-mode:evidence-synthesis"
  - "validation:prisma-grade-rob"
trigger:
  any_terms:
    - systematic review
    - meta-analysis
    - PRISMA
    - GRADE
    - risk of bias
    - forest plot
    - funnel plot
    - heterogeneity
    - I-squared
    - random effects model
    - fixed effect model
    - narrative synthesis
    - data extraction
    - search strategy
    - PICO framework
    - publication bias
    - sensitivity analysis
    - subgroup analysis
  any_tags:
    - acceptance-kind:evidence-synthesis
    - acceptance-kind:quality-assessment
    - acceptance-kind:statistical-pooling
    - reference-role:guideline
  min_term_matches: 2
  min_tag_matches: 0
  min_score: 6
assets:
  project_types:
    - path: templates/project-types/systematic-review.md
      required: true
  subfield_guides:
    - path: references/subfields/evidence-synthesis.md
      required: true
  verification_domains:
    - path: references/verification/domains/verification-domain-systematic-review.md
      required: true
  protocols_core:
    - path: references/protocols/prisma-systematic-review.md
      required: true
    - path: references/protocols/grade-certainty.md
      required: true
    - path: references/protocols/rob2-assessment.md
      required: true
    - path: references/protocols/search-strategy.md
  protocols_optional:
    - path: references/protocols/network-meta-analysis.md
    - path: references/protocols/individual-participant-data.md
  execution_guides:
    - path: references/execution/executor-subfield-guide.md
anchor_prompts:
  - State the PICO question, eligibility criteria, and primary outcome before screening or data extraction.
  - Surface the decisive quality assessment — whether GRADE certainty, RoB2 domain judgments, or publication bias diagnostics — before interpreting pooled results.
reference_prompts:
  - Keep PRISMA flow, search strategy documentation, and GRADE evidence profiles visible through planning, execution, and verification.
  - Preserve statistical conventions (effect measure, model choice, heterogeneity metrics) when comparing across analyses or literature.
estimator_policies:
  - Distinguish statistical heterogeneity (I², tau²), clinical heterogeneity (population/intervention differences), and methodological heterogeneity (study design differences) instead of collapsing them into one metric.
  - Report whether disagreement across studies comes from PICO differences, risk-of-bias variation, or true effect variation.
decisive_artifact_guidance:
  - Produce PRISMA flow diagrams documenting the screening and inclusion process with exact counts at each stage.
  - Produce forest plots with individual study weights, pooled estimates, confidence intervals, and heterogeneity statistics.
  - Produce GRADE summary-of-findings tables with certainty ratings and rationale for each outcome.
  - Preserve funnel plots or Egger test results whenever publication bias is a plausible threat to conclusions.
verifier_extensions:
  - name: prisma-completeness-audit
    rationale: Systematic reviews fail reproducibility when PRISMA items are incomplete or search strategies are not fully documented.
    check_ids:
      - "5.1"
      - "5.3"
      - "5.8"
  - name: grade-certainty-and-rob-audit
    rationale: Evidence certainty claims need explicit GRADE domain assessments and consistent risk-of-bias judgments across included studies.
    check_ids:
      - "5.5"
      - "5.6"
      - "5.15"
  - name: statistical-pooling-audit
    rationale: Meta-analytic estimates require appropriate model selection, heterogeneity assessment, and sensitivity analyses to be credible.
    check_ids:
      - "5.10"
      - "5.12"
      - "5.16"
---

# Systematic Review and Meta-Analysis Bundle

Use this bundle for evidence synthesis projects where PRISMA-compliant reporting,
GRADE certainty assessment, risk-of-bias evaluation, and quantitative or narrative
synthesis of primary studies matter more than single-study appraisal alone.
