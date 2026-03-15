---
bundle_id: balneotherapy-thermal-medicine
bundle_version: 1
title: Balneotherapy and Thermal Medicine
summary: Balneology-specific research work with thermal dose measurement, spa therapy outcome assessment, hydrotherapy evidence evaluation, mineral water characterization, and balneotherapy-specific methodology for musculoskeletal, dermatological, and respiratory indications.
selection_tags:
  - "framework:balneotherapy-thermal-medicine"
  - "work-mode:spa-therapy-research"
  - "validation:thermal-dose-outcomes-evidence"
trigger:
  any_terms:
    - balneotherapy
    - balneology
    - thermal medicine
    - spa therapy
    - hydrotherapy
    - mineral water
    - thermal water
    - pelotherapy
    - mud therapy
    - crenotherapy
    - thalassotherapy
    - thermal dose
    - water immersion
    - aquatic therapy
    - hot spring
    - sulfur bath
    - radon therapy
    - Dead Sea therapy
    - climatotherapy
    - health resort medicine
  any_tags:
    - acceptance-kind:balneological-assessment
    - acceptance-kind:thermal-intervention
    - acceptance-kind:spa-outcome
    - reference-role:balneology-guideline
  min_term_matches: 2
  min_tag_matches: 0
  min_score: 6
assets:
  project_types:
    - path: templates/project-types/balneotherapy-study.md
      required: true
  subfield_guides:
    - path: references/subfields/balneotherapy-thermal-medicine.md
      required: true
  verification_domains:
    - path: references/verification/domains/verification-domain-balneotherapy.md
      required: true
  protocols_core:
    - path: references/protocols/balneotherapy-methodology.md
      required: true
    - path: references/protocols/thermal-dose-measurement.md
      required: true
    - path: references/protocols/spa-therapy-outcomes.md
      required: true
    - path: references/protocols/hydrotherapy-evidence-assessment.md
  protocols_optional:
    - path: references/protocols/mineral-water-characterization.md
    - path: references/protocols/pelotherapy-protocols.md
    - path: references/protocols/climatotherapy-assessment.md
  execution_guides:
    - path: references/execution/executor-subfield-guide.md
anchor_prompts:
  - State the thermal intervention (water type, temperature, duration, frequency, mineral composition), comparator, and primary outcome before evaluating balneotherapy evidence.
  - Surface the decisive methodological challenge — whether blinding impossibility, placebo selection, thermal dose standardization, or co-intervention control — before interpreting spa therapy results.
reference_prompts:
  - Keep intervention characterization (water composition, temperature protocol, treatment duration), outcome measurement timing, and comparator definition visible through planning, execution, and verification.
  - Preserve balneological conventions (mineral classification, thermal dose units, treatment course definitions) when comparing across studies or thermal medicine traditions.
estimator_policies:
  - Distinguish specific therapeutic effects (mineral composition, thermal dose) from non-specific effects (relaxation, context, attention) when evaluating balneotherapy interventions.
  - Report whether treatment effects are attributable to thermal, chemical, mechanical, or psychosocial components rather than attributing all effects to the water itself.
  - Acknowledge blinding limitations inherent in balneotherapy research and quantify their likely impact on effect estimates.
decisive_artifact_guidance:
  - Produce intervention characterization tables with water composition (mineral content, pH, temperature), treatment protocol (duration, frequency, total sessions), and delivery method.
  - Produce outcome assessment tables specifying instruments used, timing of measurement, and minimum clinically important differences for each outcome.
  - Produce evidence quality matrices acknowledging balneotherapy-specific methodological challenges (blinding, placebo selection, standardization).
  - Preserve thermal dose calculations and exposure quantification whenever dose-response relationships are claimed.
verifier_extensions:
  - name: intervention-characterization-audit
    rationale: Balneotherapy research fails reproducibility when thermal interventions are poorly characterized — water composition, temperature, duration, and frequency must be explicit.
    check_ids:
      - "5.1"
      - "5.3"
      - "5.8"
  - name: blinding-and-placebo-audit
    rationale: Balneotherapy has inherent blinding challenges — studies must acknowledge this limitation and use appropriate comparators (tap water, different temperature, waiting list).
    check_ids:
      - "5.5"
      - "5.6"
      - "5.15"
  - name: specific-vs-nonspecific-effects-audit
    rationale: Spa therapy combines thermal, chemical, mechanical, and psychosocial components — credible evidence assessment must attempt to disentangle these.
    check_ids:
      - "5.10"
      - "5.12"
      - "5.16"
---

# Balneotherapy and Thermal Medicine Bundle

Use this bundle for balneology-specific research projects where thermal dose
measurement, spa therapy outcome assessment, mineral water characterization, and
hydrotherapy evidence evaluation matter more than generic clinical trial or
observational study methodology alone. This bundle addresses the unique
methodological challenges of balneotherapy research including blinding difficulty,
intervention standardization, and disentangling specific from non-specific effects.
