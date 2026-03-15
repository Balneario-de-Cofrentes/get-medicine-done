---
load_when:
  - "verification"
  - "evidence check"
  - "which verification file"
tier: 1
context_cost: small
---

# Verification Patterns — Index

This document has been split into modular files for better context efficiency. Load only the files relevant to your current work.

## Quick Start

For most verification needs, start with the quick reference:
- **`references/verification/core/verification-quick-reference.md`** — Compact 14-check checklist, decision tree, domain quick-checks (~9KB)

## Modular Files

| File | Size | Content | Load When |
|------|------|---------|-----------|
| `references/verification/core/verification-core.md` | ~18KB | PICO consistency, sensitivity analysis, risk of bias assessment, GRADE certainty, statistical validity, clinical plausibility, in-execution validation | Always — universal checks for any medical research |
| `references/verification/core/verification-numerical.md` | ~15KB | Meta-analysis statistics, heterogeneity verification, publication bias assessment, power analysis, automated verification framework | Working with meta-analyses or quantitative synthesis |
| `references/verification/core/computational-verification-templates.md` | ~12KB | R and Python code templates for forest plots, funnel plots, heterogeneity, sensitivity analysis, power analysis | Need copy-paste-ready code for verification |
| `references/verification/core/code-testing-medical.md` | ~10KB | Testing patterns for medical research code: meta-analysis scripts, data extraction, effect size calculation | Writing or validating R/Python analysis code |

## Typical Loading Patterns

**Systematic review:** `references/verification/core/verification-core.md` (PICO, RoB, GRADE)
**Meta-analysis:** `references/verification/core/verification-core.md` + `references/verification/core/verification-numerical.md`
**Running R/Python analysis:** `references/verification/core/verification-numerical.md` + `references/verification/core/computational-verification-templates.md`
**Testing analysis code:** `references/verification/core/code-testing-medical.md` + `references/verification/core/computational-verification-templates.md`
**Diagnostic accuracy review:** `references/verification/core/verification-core.md` + `references/verification/core/verification-numerical.md`
**Network meta-analysis:** All core files
**Guideline-ready evidence package:** All core files

## See Also

- `references/verification/core/verification-quick-reference.md` — Compact checklist (default entry point)
- `references/verification/core/verification-core.md` — Universal checks
- `references/verification/core/verification-numerical.md` — Statistical verification
- `references/verification/core/computational-verification-templates.md` — Code templates
- `references/verification/core/code-testing-medical.md` — Testing patterns
- `../../medicine-subfields.md` — Subfield-specific methods, tools, and pitfalls
- `references/orchestration/checkpoints.md` — Pre-checkpoint automation and computational environment management
