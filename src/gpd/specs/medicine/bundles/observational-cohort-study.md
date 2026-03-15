---
bundle_id: observational-cohort-study
bundle_version: 1
title: Observational Cohort Study
summary: Non-interventional longitudinal study work with STROBE-compliant reporting, confounding control, ROBINS-I risk-of-bias assessment, Newcastle-Ottawa quality scoring, and follow-up adequacy evaluation.
selection_tags:
  - "framework:observational-cohort"
  - "work-mode:longitudinal-study"
  - "validation:strobe-confounding-robins"
trigger:
  any_terms:
    - cohort study
    - observational study
    - STROBE
    - confounding
    - propensity score
    - ROBINS-I
    - Newcastle-Ottawa
    - case-control
    - cross-sectional
    - exposure assessment
    - follow-up
    - loss to follow-up
    - immortal time bias
    - time-varying confounding
    - instrumental variable
    - directed acyclic graph
    - DAG
    - selection bias
  any_tags:
    - acceptance-kind:observational
    - acceptance-kind:confounding-control
    - acceptance-kind:bias-assessment
    - reference-role:epidemiological
  min_term_matches: 2
  min_tag_matches: 0
  min_score: 6
assets:
  project_types:
    - path: templates/project-types/observational-study.md
      required: true
  subfield_guides:
    - path: references/subfields/epidemiology.md
      required: true
  verification_domains:
    - path: references/verification/domains/verification-domain-epidemiology.md
      required: true
  protocols_core:
    - path: references/protocols/strobe-reporting.md
      required: true
    - path: references/protocols/confounding-control.md
      required: true
    - path: references/protocols/robins-i-assessment.md
      required: true
    - path: references/protocols/newcastle-ottawa.md
  protocols_optional:
    - path: references/protocols/propensity-score-methods.md
    - path: references/protocols/instrumental-variables.md
    - path: references/protocols/dag-causal-inference.md
  execution_guides:
    - path: references/execution/executor-subfield-guide.md
anchor_prompts:
  - State the exposure, outcome, comparator group, and confounding control strategy before interpreting observational associations.
  - Surface the decisive bias threat — whether unmeasured confounding, immortal time bias, or selection bias — before drawing causal conclusions.
reference_prompts:
  - Keep STROBE checklist compliance, DAG assumptions, and confounding adjustment methods visible through planning, execution, and verification.
  - Preserve exposure definitions, outcome ascertainment methods, and follow-up periods when comparing across observational studies.
estimator_policies:
  - Distinguish association from causation explicitly — observational estimates are not causal without strong design features (e.g., instrumental variables, regression discontinuity).
  - Report how confounding was addressed (matching, stratification, regression, propensity score, IV) and which confounders remain uncontrolled.
  - Quantify the impact of loss to follow-up and missing data on effect estimates using sensitivity analyses.
decisive_artifact_guidance:
  - Produce DAGs showing assumed causal structure, identified confounders, and adjustment strategy.
  - Produce ROBINS-I or Newcastle-Ottawa quality assessment tables for each included study.
  - Produce confounder-adjusted effect estimates with confidence intervals, alongside unadjusted estimates to show confounding impact.
  - Preserve follow-up adequacy tables showing attrition rates, reasons for loss, and comparison of completers vs non-completers.
verifier_extensions:
  - name: confounding-and-bias-audit
    rationale: Observational studies fail validity when confounding is inadequately controlled or key biases (immortal time, selection) are unaddressed.
    check_ids:
      - "5.1"
      - "5.3"
      - "5.8"
  - name: robins-i-and-quality-audit
    rationale: Risk-of-bias assessment for non-randomized studies requires domain-specific judgment that LLMs frequently get wrong.
    check_ids:
      - "5.5"
      - "5.6"
      - "5.15"
  - name: follow-up-and-attrition-audit
    rationale: Inadequate follow-up and differential attrition silently invalidate cohort study conclusions.
    check_ids:
      - "5.10"
      - "5.12"
      - "5.16"
---

# Observational Cohort Study Bundle

Use this bundle for non-interventional longitudinal study projects where STROBE
compliance, confounding control, ROBINS-I assessment, and follow-up adequacy
matter more than interventional trial methodology alone.
