---
load_when:
  - "PROSPERO"
  - "protocol registration"
  - "review registration"
  - "pre-registration"
  - "registered protocol"
  - "protocol deviation"
  - "registered review"
  - "systematic review protocol"
tier: 2
context_cost: low
---

# PROSPERO Registration Protocol

PROSPERO is the international prospective register of systematic reviews. Registration before or during the conduct of a review (but before data extraction) promotes transparency, reduces duplication, and minimizes the risk of reporting bias. Registration is now effectively mandatory for credible systematic reviews.

**Core principle:** Registering a protocol before seeing the results prevents post-hoc modifications that could bias the review. Like trial registration, it creates a public record of what was planned versus what was done.

## Related Protocols

- `prisma-systematic-review.md` -- PRISMA Item 23 (registration and protocol)
- `pico-framework.md` -- PICO structures the registration fields
- `search-strategy.md` -- Search strategy should be developed before registration

---

## When to Register

### Timing

| Stage | Registration Appropriate? |
|-------|--------------------------|
| Before starting the search | Ideal -- full prospective registration |
| After search, before screening | Acceptable -- still prospective for most decisions |
| During screening, before data extraction | Acceptable -- register with a note about current stage |
| After data extraction has started | Too late for PROSPERO; register elsewhere and note the timing |
| After analysis is complete | Registration is retrospective and provides less protection against bias |

### Eligibility for PROSPERO

PROSPERO accepts registration of:

- Systematic reviews of health-related outcomes
- Systematic reviews with a health-related outcome (including public health, social care, education if health outcomes included)
- Reviews of diagnostic test accuracy
- Reviews of prognosis
- Reviews of etiology/risk factors

PROSPERO does NOT currently accept:

- Scoping reviews
- Literature reviews without systematic methodology
- Reviews of animal studies (unless directly informing human health)

For reviews not eligible for PROSPERO, consider:

| Alternative | URL | Accepts |
|-------------|-----|---------|
| OSF Registries | https://osf.io/registries | Any research, including scoping reviews |
| Research Registry | https://www.researchregistry.com | Systematic reviews and other study types |
| Published protocol | Journal publication | Any type |

---

## Required Registration Fields

### Mandatory Fields

| Field | Description | Guidance |
|-------|------------|---------|
| **Review title** | Descriptive title of the review | Include key PICO elements; match the eventual publication title |
| **Anticipated start date** | When the review began or will begin | Date of first search or screening |
| **Anticipated completion date** | When the review is expected to be completed | Realistic estimate |
| **Named contact** | Person responsible for the review | Corresponding author |
| **Review team** | All team members with affiliations | Complete list; add members as they join |
| **Funding** | Sources of financial support | Report all funding, including personal/institutional |
| **Conflicts of interest** | Any conflicts of interest | Report or state "None" |
| **Review question** | The question in structured format | Use PICO/PECO structure |
| **Searches** | Databases and other sources to be searched | List all databases; provide full strategy if possible |
| **Condition or domain** | The health condition being studied | Use standard terminology (ICD, MeSH) |
| **Participants/population** | Who is included | Match PICO "P" |
| **Intervention(s)/exposure(s)** | What is being studied | Match PICO "I/E" |
| **Comparator(s)/control** | What it is being compared to | Match PICO "C" |
| **Main outcome(s)** | Primary outcomes | Specify measurement, time point |
| **Additional outcome(s)** | Secondary outcomes | Specify measurement, time point |
| **Data extraction** | How data will be extracted | Number of extractors, form details |
| **Risk of bias assessment** | Tool to be used | RoB 2, ROBINS-I, NOS (specify) |
| **Strategy for data synthesis** | Planned synthesis methods | Meta-analysis model, narrative synthesis approach |

### Optional but Recommended Fields

| Field | Description |
|-------|------------|
| **Subgroup analysis** | Pre-specified subgroup analyses |
| **Sensitivity analysis** | Planned sensitivity analyses |
| **Type of study** | Study designs to be included |
| **Language** | Language restrictions (or lack thereof) |
| **Country** | Country restrictions (or lack thereof) |
| **Other registration details** | Related registrations (e.g., Cochrane protocol) |
| **Dissemination plans** | Where the review will be published |

---

## Writing the Protocol

A registered protocol should contain sufficient detail to pre-specify all key methodological decisions. Expand on the registration fields with:

### Protocol Template

```markdown
# Protocol: [Review Title]

## Registration
PROSPERO ID: CRD42026XXXXXX
Date registered: YYYY-MM-DD
Date last amended: YYYY-MM-DD

## Background
[Rationale for the review: what is known, what is not known, why this review is needed]

## Objectives
[PICO-structured question]

## Eligibility Criteria
### Inclusion
- Population: [specific criteria]
- Intervention: [specific criteria]
- Comparator: [specific criteria]
- Outcomes: [specific criteria]
- Study design: [specific criteria]

### Exclusion
- [Specific exclusion criteria]

## Information Sources
- Databases: [list with platforms]
- Grey literature: [sources]
- Other: [reference lists, expert contact, etc.]

## Search Strategy
[Full search strategy for at least one database; others in appendix]

## Study Selection
[Screening process: stages, number of reviewers, software, conflict resolution]

## Data Extraction
[Form description, pilot testing, number of extractors, conflict resolution]

## Risk of Bias Assessment
[Tool(s), number of assessors, conflict resolution]

## Data Synthesis
### Quantitative synthesis (meta-analysis)
[Model, estimation method, software, heterogeneity assessment]

### Qualitative synthesis (narrative)
[Method, when used instead of meta-analysis]

### Subgroup analyses
[Pre-specified subgroups with rationale]

### Sensitivity analyses
[Pre-specified sensitivity analyses]

## Assessment of Certainty of Evidence
[GRADE framework: domains, Summary of Findings table]

## Ethics and Dissemination
[Ethical approval (usually not needed for reviews), target journal, other dissemination]

## Timeline
[Milestones with estimated dates]

## Author Contributions
[Roles: search design, screening, extraction, analysis, writing]
```

---

## Amendments to the Protocol

Amendments are expected and acceptable, but must be documented transparently.

### When Amendments Are Needed

- Eligibility criteria need refinement based on the evidence found.
- New relevant outcomes identified during screening.
- Planned meta-analysis not feasible (insufficient studies or data).
- Additional databases identified for searching.
- Change in analysis method based on data characteristics.

### How to Document Amendments

| Element | Requirement |
|---------|------------|
| Date of amendment | When the change was made |
| What was changed | Specific element modified |
| Rationale | Why the change was necessary |
| Stage of review | At what point the change was made (before/after screening, before/after extraction) |

**In PROSPERO:** Update the registration with amendments. PROSPERO maintains a version history.

**In the publication:** Report all amendments in the methods section or supplementary materials, per PRISMA Item 23.

### Amendments That Raise Concerns

| Amendment | Concern Level | Why |
|-----------|--------------|-----|
| Changing primary outcome after seeing results | High | Suggests outcome switching |
| Adding subgroup analyses after seeing results | High | Suggests data-driven hypotheses presented as pre-specified |
| Narrowing eligibility to exclude unfavorable studies | High | Suggests selection bias |
| Broadening search after initial results | Low | Improves comprehensiveness |
| Changing from meta-analysis to narrative synthesis | Low | Based on data characteristics, not results |
| Adding a sensitivity analysis | Low | Improves robustness assessment |

---

## Benefits of Registration

| Benefit | Mechanism |
|---------|-----------|
| Reduces reporting bias | Pre-specifying outcomes prevents outcome switching |
| Reduces duplication | Other teams can check PROSPERO before starting a similar review |
| Increases transparency | Public record of planned vs. actual methods |
| Improves quality | Forces protocol development before conduct |
| Journal requirement | Many journals require PROSPERO registration for systematic review submissions |
| PRISMA compliance | PRISMA Item 23 requires registration information |

---

## Common Mistakes in Registration

| Mistake | Impact | Prevention |
|---------|--------|------------|
| Registering after results are known | Retrospective registration provides no bias protection | Register before data extraction |
| Vague eligibility criteria | Allows post-hoc inclusion/exclusion decisions | Be specific about PICO elements |
| Not specifying primary outcome | Allows outcome switching | Name one primary outcome |
| Not listing subgroup analyses | Post-hoc subgroups presented as pre-specified | List all planned subgroups |
| Not updating registration | Published protocol does not match registration | Update PROSPERO with amendments |
| Copying methods verbatim without thinking | Registration becomes a formality rather than a planning tool | Use registration as an opportunity for careful protocol development |

---

## Verification Checklist

Before finalizing PROSPERO registration:

- [ ] Review question structured using PICO/PECO
- [ ] Primary outcome explicitly named (one preferred)
- [ ] Eligibility criteria specific enough to be applied consistently
- [ ] All databases to be searched are listed
- [ ] Search strategy developed for at least one database
- [ ] Risk of bias tool specified
- [ ] Synthesis method described (meta-analysis model or narrative approach)
- [ ] Subgroup analyses pre-specified with rationale
- [ ] Sensitivity analyses pre-specified
- [ ] All team members listed with affiliations
- [ ] Funding and conflicts of interest declared
- [ ] Registration submitted before data extraction begins
- [ ] Registration number noted for inclusion in the manuscript
