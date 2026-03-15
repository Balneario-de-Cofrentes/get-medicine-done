---
bundle_id: health-economics-evaluation
bundle_version: 1
title: Health Economics Evaluation
summary: Economic evaluation work with CHEERS-compliant reporting, cost-effectiveness and cost-utility modeling, QALY estimation, sensitivity analysis, and budget impact assessment.
selection_tags:
  - "framework:health-economics"
  - "work-mode:economic-evaluation"
  - "validation:cheers-qaly-sensitivity"
trigger:
  any_terms:
    - health economics
    - cost-effectiveness
    - cost-utility
    - cost-benefit
    - CHEERS
    - QALY
    - DALY
    - ICER
    - incremental cost-effectiveness ratio
    - willingness to pay
    - budget impact
    - Markov model
    - decision tree
    - microsimulation
    - sensitivity analysis
    - probabilistic sensitivity analysis
    - discount rate
    - time horizon
    - EQ-5D
    - health utility
  any_tags:
    - acceptance-kind:economic-evaluation
    - acceptance-kind:cost-model
    - acceptance-kind:utility-assessment
    - reference-role:hta-guideline
  min_term_matches: 2
  min_tag_matches: 0
  min_score: 6
assets:
  project_types:
    - path: templates/project-types/health-economics.md
      required: true
  subfield_guides:
    - path: references/subfields/health-economics.md
      required: true
  verification_domains:
    - path: references/verification/domains/verification-domain-health-economics.md
      required: true
  protocols_core:
    - path: references/protocols/cheers-reporting.md
      required: true
    - path: references/protocols/cost-effectiveness-modeling.md
      required: true
    - path: references/protocols/qaly-estimation.md
      required: true
    - path: references/protocols/sensitivity-analysis.md
  protocols_optional:
    - path: references/protocols/budget-impact-analysis.md
    - path: references/protocols/markov-cohort-modeling.md
  execution_guides:
    - path: references/execution/executor-subfield-guide.md
anchor_prompts:
  - State the perspective (societal, healthcare payer, patient), time horizon, discount rate, and comparator before modeling costs and outcomes.
  - Surface the decisive model assumption — whether transition probabilities, utility weights, or cost inputs — before interpreting ICER estimates.
reference_prompts:
  - Keep CHEERS checklist compliance, model structure documentation, and input parameter sources visible through planning, execution, and verification.
  - Preserve currency, price year, discount rate, and willingness-to-pay threshold conventions when comparing across economic evaluations.
estimator_policies:
  - Distinguish deterministic from probabilistic sensitivity analysis results and explain what each contributes to decision-making.
  - Report whether ICER estimates are driven by cost assumptions, effectiveness inputs, or utility weights rather than presenting a single point estimate.
  - Separate structural uncertainty (model choice) from parameter uncertainty (input distributions) in sensitivity analyses.
decisive_artifact_guidance:
  - Produce model structure diagrams (decision trees or Markov state-transition diagrams) with all health states, transitions, and absorbing states.
  - Produce cost-effectiveness planes and acceptability curves from probabilistic sensitivity analysis.
  - Produce tornado diagrams from one-way sensitivity analysis identifying the most influential parameters.
  - Preserve budget impact projections with uptake scenarios when they anchor reimbursement decisions.
verifier_extensions:
  - name: cheers-and-model-transparency-audit
    rationale: Economic evaluations fail credibility when CHEERS items are incomplete or model assumptions are opaque.
    check_ids:
      - "5.1"
      - "5.3"
      - "5.8"
  - name: qaly-and-utility-audit
    rationale: QALY estimates require validated utility weights from appropriate populations — wrong utilities invalidate the entire ICER.
    check_ids:
      - "5.5"
      - "5.6"
      - "5.15"
  - name: sensitivity-and-uncertainty-audit
    rationale: Economic conclusions without adequate sensitivity analysis hide decision-relevant uncertainty from policymakers.
    check_ids:
      - "5.10"
      - "5.12"
      - "5.16"
---

# Health Economics Evaluation Bundle

Use this bundle for economic evaluation projects where CHEERS-compliant reporting,
cost-effectiveness modeling, QALY estimation, and comprehensive sensitivity analysis
matter more than clinical effectiveness assessment alone.
