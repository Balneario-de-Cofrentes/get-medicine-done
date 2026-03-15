# Research Proposal Template

Write a proposal file with at minimum these sections. GPD's `--auto` mode reads this to generate the scoping contract, PICO, and research plan.

## Minimal Proposal (Systematic Review)

```markdown
# Title: [Descriptive title following PRISMA guidance]

## Research Question
[Single clear question in PICO format]

## PICO
- **P:** [Population with inclusion criteria]
- **I:** [Intervention with specifics]
- **C:** [Comparator(s)]
- **O:** Primary: [outcome + measure]. Secondary: [outcomes]

## Study Types
[e.g., RCTs, quasi-experimental, observational]

## Databases
[e.g., PubMed, Cochrane CENTRAL, Embase, PEDro]

## Expected Timeline
[Brief phases and duration]
```

## Minimal Proposal (Primary Research / Clinical Trial)

```markdown
# Title: [Study title]

## Hypothesis
[Primary hypothesis]

## Study Design
[e.g., Parallel-group RCT, crossover, cohort]

## Population
[Inclusion/exclusion criteria]

## Intervention and Control
[Details of both arms]

## Outcomes
Primary: [outcome + measurement + timepoint]
Secondary: [outcomes]

## Sample Size
[Estimated N with justification if available]

## Analysis Plan
[Statistical approach]
```

## Minimal Proposal (Narrative Review / Other)

```markdown
# Title: [Review title]

## Objective
[What this review aims to achieve]

## Scope
[Topics covered, boundaries]

## Methods
[Search strategy, synthesis approach]

## Expected Output
[Type of deliverable: review article, report, guideline]
```

## Tips

- Be specific in PICO — vague populations or interventions produce vague scoping contracts
- Include "stop conditions" if known (e.g., "<3 RCTs found → switch to narrative synthesis")
- Mention target journal if writing for publication (affects reporting standards)
- GPD will ask for missing elements if `--auto` can't infer them
