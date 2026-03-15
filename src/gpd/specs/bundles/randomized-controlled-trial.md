---
bundle_id: randomized-controlled-trial
bundle_version: 1
title: Randomized Controlled Trial
summary: Interventional study work with CONSORT-compliant reporting, randomization integrity, blinding assessment, intention-to-treat analysis, sample size justification, and adverse event monitoring.
selection_tags:
  - "framework:randomized-controlled-trial"
  - "work-mode:interventional-study"
  - "validation:consort-randomization-blinding"
trigger:
  any_terms:
    - randomized controlled trial
    - RCT
    - CONSORT
    - randomization
    - allocation concealment
    - blinding
    - double-blind
    - intention to treat
    - per protocol
    - sample size calculation
    - power analysis
    - adverse events
    - placebo controlled
    - parallel group
    - crossover trial
    - non-inferiority
    - superiority trial
    - interim analysis
    - DSMB
  any_tags:
    - acceptance-kind:interventional
    - acceptance-kind:efficacy-assessment
    - acceptance-kind:safety-assessment
    - reference-role:trial-protocol
  min_term_matches: 2
  min_tag_matches: 0
  min_score: 6
assets:
  project_types:
    - path: templates/project-types/clinical-trial.md
      required: true
  subfield_guides:
    - path: references/subfields/clinical-trials.md
      required: true
  verification_domains:
    - path: references/verification/domains/verification-domain-clinical-trials.md
      required: true
  protocols_core:
    - path: references/protocols/consort-reporting.md
      required: true
    - path: references/protocols/randomization-blinding.md
      required: true
    - path: references/protocols/intention-to-treat.md
      required: true
    - path: references/protocols/sample-size-power.md
  protocols_optional:
    - path: references/protocols/adaptive-trial-design.md
    - path: references/protocols/non-inferiority-design.md
  execution_guides:
    - path: references/execution/executor-subfield-guide.md
anchor_prompts:
  - State the primary outcome, randomization method, blinding level, and sample size justification before analyzing trial results.
  - Surface the decisive methodological concern — whether allocation concealment, attrition bias, or selective reporting — before interpreting efficacy claims.
reference_prompts:
  - Keep CONSORT flow diagram, ITT population definition, and pre-specified analysis plan visible through planning, execution, and verification.
  - Preserve outcome definitions, time points, and handling of missing data when comparing across trial reports or literature.
estimator_policies:
  - Distinguish efficacy (ITT) from per-protocol estimates and explain what each tells about the intervention.
  - Report whether treatment effects are driven by true efficacy, differential attrition, or unblinding rather than collapsing results into a single p-value.
  - Separate statistical significance from clinical significance using minimum clinically important differences (MCID) when available.
decisive_artifact_guidance:
  - Produce CONSORT flow diagrams with participant counts at enrollment, allocation, follow-up, and analysis.
  - Produce primary outcome analysis tables with effect estimates, confidence intervals, and p-values for both ITT and per-protocol populations.
  - Produce adverse event summary tables with incidence rates and serious adverse event narratives.
  - Preserve sample size calculations showing assumptions (alpha, power, effect size, dropout rate) when they anchor the trial's credibility.
verifier_extensions:
  - name: randomization-and-blinding-audit
    rationale: Trial validity depends on intact randomization, allocation concealment, and blinding — failures here bias all downstream results.
    check_ids:
      - "5.1"
      - "5.3"
      - "5.8"
  - name: itt-and-attrition-audit
    rationale: Intention-to-treat integrity and handling of missing data are the most common sources of bias in trial reporting.
    check_ids:
      - "5.5"
      - "5.6"
      - "5.10"
  - name: adverse-event-completeness-audit
    rationale: Selective reporting of adverse events distorts the benefit-risk assessment and can cause patient harm.
    check_ids:
      - "5.12"
      - "5.15"
      - "5.16"
---

# Randomized Controlled Trial Bundle

Use this bundle for interventional study projects where CONSORT-compliant reporting,
randomization integrity, blinding assessment, ITT analysis, and adverse event
monitoring matter more than observational study methodology alone.
