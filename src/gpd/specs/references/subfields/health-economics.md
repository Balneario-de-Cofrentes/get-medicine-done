---
load_when:
  - "health economics"
  - "cost-effectiveness"
  - "cost-utility"
  - "QALY"
  - "DALY"
  - "ICER"
  - "health technology assessment"
  - "HTA"
  - "budget impact"
  - "pharmacoeconomics"
tier: 2
context_cost: medium
---

# Health Economics

## Core Concepts and Terminology

**Types of Economic Evaluation:**

- **Cost-effectiveness analysis (CEA):** Compares interventions in terms of cost per unit of health outcome (e.g., cost per life-year gained, cost per event prevented)
- **Cost-utility analysis (CUA):** Special case of CEA where outcomes are measured in quality-adjusted life years (QALYs); the dominant form in HTA
- **Cost-benefit analysis (CBA):** Both costs and outcomes measured in monetary terms; uses willingness-to-pay (WTP); less common in healthcare
- **Cost-minimization analysis (CMA):** Assumes equivalent outcomes, compares costs only; rarely justified (outcomes are almost never truly equivalent)
- **Budget impact analysis (BIA):** Estimates the financial impact of adopting a new intervention on a specific budget over 1-5 years; does not assess value

**Key Metrics:**

- **QALY (Quality-Adjusted Life Year):** 1 year of life in full health = 1 QALY; derived from health state utility values (0 = death, 1 = perfect health; some states <0, i.e., worse than death)
- **DALY (Disability-Adjusted Life Year):** Years of life lost (YLL) + years lived with disability (YLD); used by WHO/GBD; 1 DALY = 1 year of healthy life lost
- **ICER (Incremental Cost-Effectiveness Ratio):** (Cost_new - Cost_comparator) / (Effect_new - Effect_comparator); cost per additional QALY gained
- **NMB (Net Monetary Benefit):** lambda * delta_E - delta_C; positive NMB = cost-effective at threshold lambda
- **WTP threshold:** Amount society willing to pay per QALY; varies by country ($50,000-$150,000 in US; GBP 20,000-30,000 in UK NICE; 1-3x GDP per capita by WHO suggestion)
- **Cost-effectiveness acceptability curve (CEAC):** Probability of being cost-effective across a range of WTP thresholds

**Health State Utilities:**

- **EQ-5D:** 5 dimensions (mobility, self-care, usual activities, pain/discomfort, anxiety/depression); EQ-5D-3L (3 levels) or EQ-5D-5L (5 levels); valued using country-specific tariffs
- **SF-6D:** Derived from SF-36; 6 dimensions, preference-based
- **HUI (Health Utilities Index):** HUI2 and HUI3; multi-attribute utility
- **Standard Gamble:** Direct elicitation; gold standard for von Neumann-Morgenstern utility theory
- **Time Trade-Off (TTO):** How many years in full health equivalent to X years in impaired state
- **Visual Analogue Scale (VAS):** 0-100 thermometer; not preference-based; not recommended for QALYs

**Costing:**

- **Perspective:** Societal (all costs including productivity loss, informal care), healthcare payer, hospital, patient
- **Direct medical costs:** Hospitalizations, medications, procedures, consultations
- **Direct non-medical costs:** Transportation, accommodation, home modifications
- **Indirect costs:** Productivity loss (human capital approach vs friction cost approach), presenteeism, absenteeism
- **Intangible costs:** Pain, suffering; typically captured in utility measurement, not costed separately
- **Discounting:** Future costs and outcomes discounted to present value; typically 3-5% per year (varies by country); same rate for costs and outcomes in most guidelines

## Key Research Methodologies

**Decision-Analytic Modeling:**

- **Decision tree:** For short-term decisions with discrete outcomes; branches represent chance nodes, decision nodes, terminal nodes
- **Markov model:** For chronic diseases with recurring events over time; patients transition between health states at defined intervals (cycles); half-cycle correction
- **Microsimulation:** Individual-level simulation; allows patient heterogeneity, history-dependent transitions, complex disease pathways
- **Discrete event simulation (DES):** Events occur at variable times (not fixed cycles); more flexible than Markov for complex care pathways
- **Partitioned survival analysis:** For oncology; partition time horizon into pre-progression, post-progression, dead using survival curves; assumes PFS and OS independence
- **Dynamic transmission models:** For infectious diseases; SIR, SEIR compartmental models; capture herd immunity effects

**Sensitivity Analysis:**

- **One-way:** Vary one parameter at a time; tornado diagram shows most influential parameters
- **Multi-way (scenario):** Vary multiple parameters simultaneously; limited number of scenarios
- **Probabilistic sensitivity analysis (PSA):** Assign distributions to all uncertain parameters; Monte Carlo simulation (1000-10,000 iterations); generates cost-effectiveness plane scatter plot and CEAC
- **Value of information analysis (VOI):** Expected Value of Perfect Information (EVPI), Expected Value of Partial Perfect Information (EVPPI); quantifies value of reducing uncertainty through further research
- **Structural uncertainty:** Test alternative model structures (e.g., different number of health states, different extrapolation methods)

**Survival Extrapolation:**

- Critical for oncology and chronic disease models
- Standard parametric: exponential, Weibull, Gompertz, log-logistic, log-normal, generalized gamma
- Model selection: AIC/BIC, visual fit, clinical plausibility of long-term extrapolation, external data validation
- Flexible models: Royston-Parmar (restricted cubic splines), fractional polynomial, mixture cure models
- Hazard function inspection: constant (exponential), monotone (Weibull, Gompertz), non-monotone (log-logistic, log-normal)

## Common Study Designs

| Design | Use | Data Source |
|--------|-----|-------------|
| Trial-based CEA | Piggyback economic evaluation on RCT | Within-trial resource use and outcomes |
| Model-based CEA | Extrapolate beyond trial, synthesize evidence | Multiple sources: trials, registry, expert opinion |
| Retrospective cohort | Real-world cost and resource use | Claims data, EHR, registry |
| Budget impact model | Financial planning for payer/hospital | Epidemiological data, market share assumptions |
| Willingness-to-pay study | Elicit monetary value of health gains | Discrete choice experiment, contingent valuation |

## Important Databases and Registries

| Resource | Purpose | Access |
|----------|---------|--------|
| **Tufts CEA Registry** | Catalog of published cost-per-QALY studies | cearegistry.org |
| **NICE Technology Appraisals** | UK HTA decisions and models | nice.org.uk |
| **CADTH** | Canadian HTA agency | cadth.ca |
| **PBAC** | Australian Pharmaceutical Benefits Advisory Committee | pbs.gov.au |
| **IQWiG** | German HTA institute | iqwig.de |
| **HAS** | French HTA authority (Haute Autorite de Sante) | has-sante.fr |
| **EUnetHTA** | European HTA network | eunethta.eu |
| **WHO-CHOICE** | Choosing Interventions that are Cost-Effective | who.int/choice |
| **NHS Reference Costs** | English hospital cost data | england.nhs.uk |
| **MEPS** | US Medical Expenditure Panel Survey | meps.ahrq.gov |

## Key Journals

- **Value in Health** — ISPOR journal; health economics methodology and applications
- **PharmacoEconomics** — pharmacoeconomic evaluations
- **Health Economics** — economic theory applied to health
- **Medical Decision Making** — decision analysis, utility measurement
- **Journal of Health Economics** — econometric methods in health
- **European Journal of Health Economics** — European health economics
- **Cost Effectiveness and Resource Allocation** — open-access
- **The Lancet** — high-profile economic evaluations alongside trials
- **BMJ** — economic evaluations, HTA methods

## Verification Considerations

**Model Validation:**

- Check: face validity (clinical experts reviewed model structure and assumptions)
- Check: internal validity (model reproduces input data, mathematical verification)
- Check: external validity (model predictions match independent data sources)
- Check: cross-validation (compare results with other published models for same disease)
- CHEERS 2022 reporting checklist compliance

**ICER Interpretation:**

- Check: is the ICER compared against the appropriate WTP threshold for the jurisdiction?
- Check: is the ICER in the northeast quadrant (more effective, more costly) or is it in a different quadrant?
- Check: is the ICER dominated (more costly AND less effective)? Extended dominance?
- Check: is incremental analysis performed correctly (order interventions by cost, remove dominated, calculate sequential ICERs)?

**Sensitivity Analysis Adequacy:**

- Check: PSA performed (not just one-way)?
- Check: parameter distributions appropriate (beta for probabilities, gamma/lognormal for costs, beta for utilities)?
- Check: structural uncertainty explored?
- Check: CEAC reported?

**Transferability:**

- Check: are results applicable to the target jurisdiction (different costs, healthcare system, clinical practice, utility values)?
- Check: are country-specific unit costs and utility tariffs used?

## Common Pitfalls and LLM Error Risks

- **Confusing CEA and CUA:** Cost-effectiveness uses natural units (life-years, events); cost-utility uses QALYs. LLMs may conflate these
- **Misinterpreting ICER:** An ICER of $50,000/QALY means each additional QALY costs $50,000, NOT that the total cost is $50,000. LLMs frequently misinterpret ICERs
- **Ignoring discounting:** Costs and outcomes in the future must be discounted. LLMs may present undiscounted results as final
- **Wrong WTP threshold:** WTP thresholds vary by country. LLMs may apply US thresholds to UK analyses or vice versa
- **Confusing QALY and DALY:** QALYs measure health gained (higher is better); DALYs measure health lost (lower is better). LLMs sometimes confuse the direction
- **Fabricating cost data:** LLMs generate plausible but fictitious cost estimates. Always verify against real cost databases
- **Confusing perspective:** Societal perspective includes productivity losses; healthcare payer does not. LLMs may mix perspectives within one analysis
- **Presenting deterministic results without PSA:** A single ICER point estimate without uncertainty analysis is insufficient. LLMs may present deterministic results as definitive
- **Ignoring time horizon:** Chronic diseases require lifetime horizon; using short horizons underestimates long-term benefits. LLMs may not flag inappropriate time horizons
- **Misunderstanding dominance:** "Dominated" means the intervention is both more costly and less effective — it should be rejected regardless of WTP threshold. LLMs sometimes miss this

## Methodology Decision Tree

```
What is the economic question?
├── Is intervention X cost-effective vs comparator Y?
│   ├── Alongside a trial? → Trial-based CEA/CUA
│   ├── Need to extrapolate beyond trial? → Model-based CEA/CUA
│   │   ├── Acute condition? → Decision tree
│   │   ├── Chronic condition? → Markov model or microsimulation
│   │   └── Infectious disease? → Dynamic transmission model
│   └── Multiple comparators? → Fully incremental analysis with ICER frontier
├── What is the financial impact of adoption?
│   └── → Budget impact analysis (BIA); ISPOR guidelines
├── What is the value of further research?
│   └── → Value of information analysis (EVPI, EVPPI)
├── What do patients value?
│   ├── Health state utilities? → EQ-5D + country tariff; TTO/SG if needed
│   └── Preferences for attributes? → Discrete choice experiment
└── For HTA submission?
    ├── NICE? → Follow NICE reference case (NHS/PSS perspective, 3.5% discount, EQ-5D, lifetime)
    ├── CADTH? → Canadian guidelines
    └── Other? → Follow national guidelines
```

## Research Frontiers (2024-2026)

| Frontier | Key question | GMD suitability |
|----------|-------------|-----------------|
| **Distributional CEA** | Incorporating equity weights and health inequality impacts | Good — methodology review |
| **Real-world evidence for HTA** | Using RWE to supplement/replace trial data in models | Good — methodological assessment |
| **Multi-criteria decision analysis (MCDA)** | Moving beyond QALY to incorporate multiple value dimensions | Good — framework comparison |
| **Digital health economics** | Valuing digital therapeutics, AI-assisted diagnostics, telemedicine | Good — methodology development |
| **Precision medicine economics** | Cost-effectiveness of biomarker-guided therapy | Good — model-based evaluation |
| **Planetary health economics** | Environmental costs in health economic evaluation | Moderate — emerging methodology |
