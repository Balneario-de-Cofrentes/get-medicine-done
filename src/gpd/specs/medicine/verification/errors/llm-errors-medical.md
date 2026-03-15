---
tier: 1
context_cost: large
---

# LLM Medical Research Error Catalog

Language models make characteristic errors in medical research that differ from
human researcher errors. Human researchers make protocol deviations and calculation
mistakes; LLMs fabricate study results, hallucinate citations, confuse statistical
measures, and misapply methodological frameworks in systematic ways. This catalog
documents the most common LLM medical research error classes with detection
strategies.

Consult this catalog before trusting any LLM-generated medical research analysis.
Every error class below has been observed in production.

---

## Category 1: Study Result Fabrication

Errors where the LLM generates medical evidence that does not exist or
misrepresents real evidence.

### Error 1 — Hallucinating study results and effect sizes

**Description:** LLMs generate plausible-sounding but fabricated study outcomes,
including specific effect sizes, confidence intervals, and sample sizes for studies
that either do not exist or reported different results. The fabricated numbers are
typically within a plausible range for the clinical domain, making them difficult to
detect without source verification. Common patterns include generating "a
meta-analysis of 12 RCTs with N=3,847 showed..." when no such meta-analysis exists,
or reporting a hazard ratio of 0.73 (95% CI 0.61-0.88) for an intervention that was
never studied in this context.

**Example:** LLM claims "A 2019 Cochrane review of balneotherapy for osteoarthritis
(Fortunato et al.) pooling 8 RCTs (N=1,247) found SMD -0.42 (95% CI -0.61 to
-0.23) for pain reduction." No such Cochrane review by Fortunato exists. A real
Cochrane review (Verhagen 2015) found limited evidence with different effect sizes.
The fabricated citation has a plausible author name, reasonable sample size, and
believable effect size — all generated, not retrieved.

**Detection Method:** Verify EVERY cited study against PubMed, Cochrane Library, or
the original source. Check: (1) Does the author exist and publish in this field?
(2) Does the journal exist and did it publish this article in the stated year?
(3) Do the reported numbers (N, effect size, CI) match the actual publication? Use
DOI lookup when available. Cross-reference claims against known systematic reviews
in the Cochrane Library. Be especially suspicious of overly specific numbers (exact
sample sizes, precise p-values) provided without a DOI or PMID.

**Correction Approach:** Always require DOI or PMID for every cited study. When
summarizing evidence, state explicitly which claims are from verified sources vs
general domain knowledge. Never present fabricated numbers as if they come from a
specific study. If the actual evidence base is uncertain, say so.

### Error 2 — Inventing citations that do not exist

**Description:** LLMs construct bibliographic references that look legitimate but
point to non-existent publications. The fabricated citations typically combine real
author names from the field, real journal names, plausible years, and realistic
titles. Some variants create citations that are close to real papers but with wrong
details (wrong year, wrong journal, wrong first author). This is distinct from
Error 1 because the numbers may not be fabricated — the entire source is.

**Example:** LLM cites "Zhang L, Wang H, et al. Thermal water immersion for
fibromyalgia: a randomized controlled trial. Ann Rheum Dis. 2021;80(4):412-419."
No such paper exists in Annals of the Rheumatic Diseases. The authors are real
rheumatology researchers, the journal is real, and the title is plausible — but the
paper was never published. Searching PubMed for the title returns zero results.

**Detection Method:** Search PubMed, Google Scholar, and CrossRef for the exact
title. If not found, search for the first author + journal + year combination. If
the paper cannot be located through any database, it almost certainly does not
exist. For Cochrane reviews: verify against the Cochrane Library directly. Never
trust a citation that lacks a DOI or PMID without independent verification.

**Correction Approach:** Provide only citations that have been verified against a
bibliographic database. When generating reference lists, include DOIs or PMIDs. If
a relevant study might exist but cannot be confirmed, describe the evidence gap
honestly rather than fabricating a citation.

### Error 3 — Misattributing findings to wrong studies

**Description:** LLMs take real findings from one study and attribute them to a
different study, or combine results from multiple studies into a single fabricated
attribution. This is more insidious than pure fabrication because the findings
themselves may be real — they are just assigned to the wrong source. Common patterns
include attributing meta-analysis results to a single RCT, attributing findings from
one systematic review to another, or merging results from two studies into one.

**Example:** LLM states "The RECOVERY trial demonstrated that hydroxychloroquine
reduced mortality by 15% in hospitalized COVID-19 patients." The RECOVERY trial
actually showed NO benefit from hydroxychloroquine (RR 1.09, 95% CI 0.97-1.23) and
was one of the key trials that led to abandoning this treatment. The LLM has
confused the RECOVERY trial's hydroxychloroquine arm results with its dexamethasone
arm results (which did show mortality reduction).

**Detection Method:** Cross-reference the specific finding with the specific trial
or study. For major trials (RECOVERY, SPRINT, EMPA-REG, etc.): verify results
against the primary publication's abstract. For meta-analyses: check that the pooled
estimate matches the actual published forest plot. Be especially vigilant when the
attribution involves a well-known trial name — LLMs often swap findings between arms
or between trials in the same program.

**Correction Approach:** When citing trial results, always specify the exact arm,
comparison, and primary publication. Include the trial registration number (NCT)
when available. For landmark trials, verify results against ClinicalTrials.gov
results database or the primary publication before attributing specific numbers.

### Error 4 — Fabricating systematic review conclusions

**Description:** LLMs generate summary statements about "the evidence" that
synthesize a fabricated body of literature, creating an illusion of established
consensus where none exists or where the consensus differs from what is stated. The
LLM may correctly identify that a Cochrane review exists on a topic but then
misstate its conclusions, or claim a Cochrane review exists when only non-systematic
reviews have been published.

**Example:** LLM states "Multiple systematic reviews consistently demonstrate that
radon spa therapy significantly reduces pain in ankylosing spondylitis, with GRADE
high-certainty evidence." In reality, the evidence for radon therapy in AS is limited
to a small number of trials with methodological concerns, and no systematic review
has rated this as high-certainty evidence. The GRADE rating would more accurately be
low or very low certainty due to risk of bias and imprecision.

**Detection Method:** Verify systematic review conclusions against the original
publication. For GRADE ratings: check the actual summary-of-findings table — LLMs
frequently upgrade certainty levels. For Cochrane reviews: the plain-language summary
and abstract contain the actual conclusion. Compare the LLM's characterization of
"the evidence" against what the most recent high-quality systematic review actually
states.

**Correction Approach:** Present evidence strength using the actual GRADE rating
from the source systematic review. When no systematic review exists, state that
explicitly. Avoid language like "the evidence shows" or "systematic reviews
demonstrate" without verifying the specific systematic reviews being referenced.

### Error 5 — Generating plausible but wrong PICO elements

**Description:** LLMs construct PICO (Population, Intervention, Comparison, Outcome)
frameworks that sound reasonable but contain errors — wrong populations for the
intervention studied, interventions not actually tested, comparisons that were not
made, or outcomes that were not measured. This leads to evidence summaries that
answer a different clinical question than intended.

**Example:** For a question about balneotherapy in rheumatoid arthritis, LLM
constructs PICO with I = "sulfur-rich thermal water at 38-40C for 20 minutes daily,
3 weeks" when the actual studies used varying temperatures (34-39C), durations
(15-30 min), frequencies (3-7x/week), and mineral compositions. The overly specific
PICO implies a standardized intervention that does not exist in the literature,
making the evidence summary misleadingly precise.

**Detection Method:** Compare the PICO elements against actual included studies.
Verify that: (1) the population described matches the eligibility criteria of cited
studies, (2) the intervention described matches what was actually delivered, (3) the
comparator matches what was used, (4) the outcomes listed were actually measured and
reported. Be especially suspicious of overly specific intervention descriptions in
fields where interventions are poorly standardized.

**Correction Approach:** Extract PICO elements directly from the methods sections of
primary studies rather than constructing them from general knowledge. Acknowledge
heterogeneity in intervention delivery across studies. When intervention
standardization is poor, describe the range of approaches used rather than a single
idealized protocol.

---

## Category 2: Statistical Errors

Errors in the interpretation, calculation, or reporting of statistical measures in
medical research.

### Error 6 — Confusing relative risk vs odds ratio vs hazard ratio

**Description:** LLMs interchange relative risk (RR), odds ratio (OR), and hazard
ratio (HR) as if they were equivalent measures. For rare outcomes (<10% event rate),
OR approximates RR; for common outcomes, OR exaggerates the effect. HR incorporates
time-to-event information that RR and OR do not. LLMs frequently report an OR as if
it were an RR, making interventions appear more effective than they are for common
outcomes.

**Example:** Study reports OR = 0.50 for a treatment reducing infection rates from
40% to 25%. LLM states "the treatment reduced infection risk by 50% (RR 0.50)." The
actual RR is 25%/40% = 0.625, meaning a 37.5% relative risk reduction, not 50%. The
OR overstates the effect because the outcome is common (40% baseline rate). For a
systematic review pooling ORs, interpreting them as RRs systematically overstates
the intervention's benefit.

**Detection Method:** Check the original study's statistical methods section to
identify which measure was used. Verify the measure matches the study design: RR for
cohort studies and RCTs, OR for case-control studies, HR for time-to-event analyses.
When the baseline event rate exceeds 10%, verify that OR is not being interpreted as
RR. Apply the Zhang and Yu formula to convert OR to RR when needed: RR = OR /
(1 - P_0 + P_0 * OR) where P_0 is the baseline risk.

**Detection Method:** Check the original study's statistical methods section to
identify which measure was used. Verify the measure matches the study design: RR for
cohort studies and RCTs, OR for case-control studies, HR for time-to-event analyses.
When the baseline event rate exceeds 10%, verify that OR is not being interpreted as
RR.

**Correction Approach:** Use the correct effect measure for the study design. When
converting between measures, show the conversion explicitly. State the baseline
event rate to contextualize relative measures. Always accompany relative measures
with absolute risk differences when possible.

### Error 7 — Wrong interpretation of p-values and confidence intervals

**Description:** LLMs misinterpret p-values as the probability that the null
hypothesis is true, or as the probability that the result occurred by chance. They
also misinterpret confidence intervals as containing the true value with X%
probability (a Bayesian interpretation of a frequentist construct). These
misinterpretations lead to wrong conclusions about the strength and meaning of
evidence.

**Example:** LLM states "The p-value of 0.03 means there is only a 3% probability
that the treatment is ineffective." This is wrong. The p-value is the probability
of observing data at least as extreme as the actual data, given that the null
hypothesis is true. A p=0.03 does NOT mean there is a 97% probability the treatment
works. Similarly, "the 95% CI of 0.5-0.9 means there is a 95% probability the true
effect is between 0.5 and 0.9" is a Bayesian interpretation that is not what a
frequentist CI means.

**Detection Method:** Check whether p-value interpretations claim probability of the
hypothesis rather than probability of the data. Correct interpretation: "If the null
hypothesis were true, the probability of seeing this result (or more extreme) is p."
Check whether CI interpretations claim probability containment for a specific
interval rather than describing the long-run coverage property of the procedure.

**Correction Approach:** State p-values as: "Under the null hypothesis, the
probability of observing an effect this large or larger is p=X." State CIs as: "If
we repeated this study many times, 95% of the calculated intervals would contain the
true value." Always accompany statistical significance with effect size and clinical
significance.

### Error 8 — Incorrect NNT/NNH calculations

**Description:** LLMs miscalculate the Number Needed to Treat (NNT) or Number
Needed to Harm (NNH), either by using relative rather than absolute risk reduction
in the formula, by inverting the calculation, or by computing NNT from odds ratios
without conversion. NNT = 1/ARR (absolute risk reduction). Using relative risk
reduction instead yields a much smaller (more favorable) NNT.

**Example:** Baseline risk 20%, treatment reduces to 15%. ARR = 5%, RRR = 25%.
Correct NNT = 1/0.05 = 20. LLM calculates NNT = 1/0.25 = 4, using relative risk
reduction instead of absolute risk reduction. This makes the treatment appear 5
times more effective than it is. For a meta-analysis, this error propagates to all
derived NNT calculations and can mislead clinical decision-making.

**Detection Method:** Verify NNT calculations by checking: (1) the denominator is
the absolute risk difference (control event rate minus treatment event rate), not the
relative risk reduction; (2) NNT = 1/ARR, not 1/RRR; (3) when derived from OR, the
Peto or Sugihara conversion is used correctly. Cross-check: NNT should be larger
than 1/baseline_risk for modest effects.

**Correction Approach:** Always compute NNT from absolute risk differences. State
the baseline risk, treatment risk, ARR, and NNT together so the calculation is
transparent. When pooling NNTs across studies with different baseline risks, compute
NNT at specific clinically relevant baseline risks rather than using a single pooled
NNT.

### Error 9 — Misinterpreting heterogeneity statistics (I-squared, tau-squared)

**Description:** LLMs misinterpret I-squared as measuring the magnitude of
between-study variation rather than the proportion of total variability due to
heterogeneity. An I-squared of 75% does not mean the effect sizes vary by 75% — it
means 75% of the observed variance is attributable to real between-study differences
rather than sampling error. LLMs also confuse tau-squared (absolute between-study
variance) with I-squared (relative measure) and apply arbitrary thresholds
(25%/50%/75%) without context.

**Example:** LLM states "I-squared = 80% indicates the effect sizes differ by 80%
across studies, suggesting very high inconsistency." This conflates the proportion
of variance due to heterogeneity with the magnitude of effect size differences. Five
large studies with effect sizes of 0.49, 0.50, 0.50, 0.51, 0.51 could have
I-squared = 80% because the studies are so precise that even tiny differences are
detectable, while five small studies with effects ranging from 0.2 to 0.8 might have
I-squared = 30% because sampling error dominates. Tau-squared and prediction
intervals are needed to assess the magnitude of heterogeneity.

**Detection Method:** Check whether I-squared is being interpreted as proportion of
variance (correct) vs magnitude of effect variation (wrong). Verify that tau-squared
or prediction intervals are used to assess the magnitude of between-study variation.
Check whether the LLM's heterogeneity assessment considers study precision: high
I-squared with low tau-squared means studies are precise but slightly different —
not necessarily problematic.

**Correction Approach:** Report both I-squared (proportion) and tau-squared
(magnitude) when assessing heterogeneity. Use prediction intervals to show the range
of plausible true effects across settings. Interpret I-squared in context: consider
the number and precision of studies, not just the percentage. Avoid rigid thresholds
(25/50/75%) without clinical context.

### Error 10 — Immortal time bias in survival analysis

**Description:** LLMs fail to recognize or correctly handle immortal time bias in
observational studies. Immortal time bias occurs when the period between cohort entry
and treatment initiation is misclassified as treated person-time. During this period,
subjects must survive (are "immortal") to receive the treatment, creating a spurious
survival advantage. LLMs also confuse immortal time bias with lead time bias and
time-dependent confounding.

**Example:** In a study of statin use after myocardial infarction, patients are
classified as "statin users" if they filled a statin prescription within 90 days
post-MI. The time between MI and first prescription is attributed to the statin
group as treated time. LLM analyzes this study and reports "statin users had 30%
lower mortality (HR 0.70)" without flagging that the immortal time between MI and
prescription start artificially favors the statin group. Correcting for immortal
time bias by using a time-dependent exposure model often attenuates or eliminates
the protective association.

**Detection Method:** Check whether the exposure in a cohort study requires surviving
to a certain time point. If yes: verify that a time-dependent analysis (landmark
analysis or time-varying Cox model) was used. If exposure is classified at a fixed
time point using information from after baseline, immortal time bias is present.
Look for phrases like "patients who received X within Y days" — the "within Y days"
implies a survival requirement.

**Correction Approach:** Use time-dependent exposure classification in Cox models.
Alternatively, use landmark analysis (define exposure status at a fixed time point
and analyze only patients who survived to that point). Report whether immortal time
was present and how it was handled. Quantify the bias by comparing results with and
without the correction.

### Error 11 — Informative censoring in survival analysis

**Description:** LLMs assume censoring is non-informative (random) when it may be
related to the outcome. In clinical trials, patients may drop out because of
deteriorating health (informative censoring), making the remaining population look
healthier. LLMs often fail to assess whether the censoring mechanism is related to
the outcome, and standard Kaplan-Meier and Cox regression assume non-informative
censoring.

**Example:** In a cancer clinical trial, 30% of patients in the treatment arm
discontinued due to adverse events. LLM reports the Kaplan-Meier survival curve
without noting that patients who discontinued due to toxicity are censored at
discontinuation. If these patients have worse outcomes after discontinuation, the
survival curve overestimates the treatment benefit. The LLM treats per-protocol
censoring as if it were administrative censoring (end of study), which has
fundamentally different implications.

**Detection Method:** Check the reasons for censoring: administrative (end of study,
lost to follow-up for non-health reasons) vs potentially informative (adverse events,
disease progression, treatment switch). If dropout rates differ between arms or are
related to health status, censoring may be informative. Verify that sensitivity
analyses (inverse probability of censoring weights, worst-case analysis) were
conducted.

**Correction Approach:** Tabulate censoring reasons by treatment arm. Conduct
sensitivity analyses assuming different censoring mechanisms: (1) non-informative
(standard), (2) worst-case (all censored patients had events), (3) inverse
probability of censoring weighting. Report how conclusions change under different
censoring assumptions.

### Error 12 — Confusing fixed-effect vs random-effects models

**Description:** LLMs apply fixed-effect models when random-effects are appropriate,
or vice versa, or describe the models incorrectly. A fixed-effect model assumes all
studies estimate the same underlying effect; a random-effects model assumes the true
effect varies across studies. LLMs often state that random-effects models are "more
conservative" — this is only true when heterogeneity is present; with zero
heterogeneity, both models give identical results. When heterogeneity is high,
random-effects models give MORE weight to smaller studies, which can introduce
small-study bias.

**Example:** LLM states "We used a random-effects model because it is more
conservative and appropriate when studies differ." With I-squared = 0% and no
clinical heterogeneity, the random-effects and fixed-effect models give identical
results, and the choice is moot. Conversely, when I-squared = 85% and funnel plot
asymmetry suggests publication bias, using a random-effects model gives
disproportionate weight to small positive studies, potentially INFLATING the pooled
effect rather than being conservative.

**Detection Method:** Check whether the model choice is justified by the
heterogeneity assessment. Verify the LLM's characterization of the model properties.
Fixed-effect: assumes one true effect, weights by inverse variance (large studies
dominate). Random-effects: assumes distribution of true effects, incorporates
between-study variance (smaller studies get relatively more weight). Check whether
the conclusion about "conservative" is appropriate given the specific heterogeneity
pattern.

**Correction Approach:** Choose the model based on the scientific question and
heterogeneity assessment, not as a default. Report both models when they disagree
substantially. When using random-effects with significant heterogeneity, check for
small-study effects (funnel plot, Egger test) because the model upweights
potentially biased small studies. Use prediction intervals with random-effects
models.

---

## Category 3: Methodology Errors

Errors in the application of research methodology frameworks, study design
principles, and reporting standards.

### Error 13 — Misapplying inclusion/exclusion criteria

**Description:** LLMs construct or evaluate eligibility criteria that are internally
inconsistent, overly broad, or mismatched with the research question. Common
patterns include including study designs that cannot answer the question (e.g., case
reports in a systematic review of treatment efficacy), applying wrong age or disease
severity thresholds, or confusing eligibility criteria across different review
protocols.

**Example:** For a systematic review of balneotherapy efficacy in knee
osteoarthritis, LLM includes studies of aquatic exercise (pool-based physiotherapy)
as balneotherapy studies. Aquatic exercise involves active exercise in water;
balneotherapy involves passive immersion in mineral/thermal water. Including aquatic
exercise studies conflates two different interventions and inflates the evidence
base, producing a pooled estimate that does not answer the original question about
balneotherapy.

**Detection Method:** Compare each included study's intervention against the
pre-specified PICO criteria. Verify that: (1) the study design matches what was
specified (RCT only, or observational allowed), (2) the population matches (disease,
severity, age), (3) the intervention is what was specified (not a related but
different intervention), (4) the comparator is appropriate. Check for scope creep:
are studies included that stretch the criteria?

**Correction Approach:** Define eligibility criteria precisely before screening. For
intervention reviews: specify the intervention components, delivery method, and
distinguishing features from related interventions. Use a pilot screening phase to
calibrate inclusion decisions. Document borderline decisions and their rationale.

### Error 14 — Cherry-picking outcomes (outcome switching)

**Description:** LLMs selectively report favorable outcomes while omitting
unfavorable ones, or present secondary outcomes as if they were primary outcomes.
This is analogous to outcome switching in clinical trials but occurs in the evidence
synthesis and interpretation stage. LLMs may also highlight subgroup analyses that
show benefit while ignoring the overall null result.

**Example:** A systematic review examines balneotherapy for fibromyalgia. The
protocol specifies pain (VAS) as the primary outcome. The pooled estimate for pain
shows no significant effect (SMD -0.18, 95% CI -0.42 to 0.06). LLM reports:
"Balneotherapy significantly improved quality of life in fibromyalgia patients
(SMD -0.35, 95% CI -0.58 to -0.12)," leading with a significant secondary outcome
while burying or omitting the null primary outcome.

**Detection Method:** Compare reported outcomes against the protocol or registration
(PROSPERO for systematic reviews, ClinicalTrials.gov for trials). Check whether the
primary outcome is reported prominently. If only secondary or subgroup results are
highlighted, check whether the primary outcome was null. Verify that all
pre-specified outcomes are reported, not just favorable ones.

**Correction Approach:** Report all pre-specified outcomes, leading with the primary
outcome regardless of whether it is statistically significant. Clearly label primary
vs secondary outcomes. When highlighting subgroup findings, present the overall
result first and note that subgroup results are exploratory.

### Error 15 — Confusing association with causation

**Description:** LLMs draw causal conclusions from observational data without
adequate justification. Phrases like "X causes Y" or "X reduces the risk of Y"
imply causation, but observational studies can only establish association unless
specific causal inference methods (IV, RD, difference-in-differences) are used with
their assumptions explicitly tested. Even well-adjusted multivariable models do not
establish causation due to unmeasured confounding.

**Example:** LLM states "Regular spa therapy prevents cardiovascular events, as
demonstrated by a prospective cohort study showing 25% lower CVD incidence among
spa users." The cohort study shows an association, not causation. Spa users likely
differ from non-users in income, health behavior, access to healthcare, and baseline
health status. Even after adjusting for measured confounders, unmeasured confounding
(healthy user bias) could fully explain the association.

**Detection Method:** Check the study design: RCTs can support causal claims;
observational studies generally cannot without strong causal inference methods. Look
for causal language ("prevents," "causes," "reduces risk") applied to observational
findings. Verify whether the study used causal inference methods (IV, Mendelian
randomization, regression discontinuity) and whether the identifying assumptions
were tested.

**Correction Approach:** Use associational language for observational findings:
"associated with," "correlated with," "observed alongside." Reserve causal language
for RCTs and well-designed causal inference studies. When reporting observational
associations, explicitly list potential unmeasured confounders and alternative
explanations. Apply Bradford Hill criteria for causal reasoning when appropriate, but
note their limitations.

### Error 16 — Wrong GRADE downgrading/upgrading rationale

**Description:** LLMs misapply the GRADE framework by downgrading or upgrading
evidence certainty for wrong reasons, applying the wrong number of levels, or
misidentifying which domains apply. Common errors: downgrading RCT evidence for
"lack of blinding" when the outcome is objective and unlikely to be affected by
blinding; upgrading observational evidence for "large effect" when confounding is
plausible; failing to downgrade for serious imprecision when confidence intervals
cross the null and the MCID.

**Example:** LLM rates evidence from three small RCTs of thermal water immersion for
chronic low back pain as GRADE "moderate" certainty. The studies have: (1) high risk
of bias due to impossible blinding in 2/3 studies, (2) serious inconsistency
(I-squared=72%), (3) indirect population (two studies included mixed chronic pain,
not specifically low back pain), (4) serious imprecision (total N=87, wide CIs
crossing the MCID). Correct rating: very low certainty (start high for RCTs,
downgrade 1 for risk of bias, 1 for inconsistency, 1 for indirectness, 1 for
imprecision = 4 downgrades). LLM only downgraded once for risk of bias.

**Detection Method:** Verify each GRADE domain was assessed: (1) risk of bias,
(2) inconsistency, (3) indirectness, (4) imprecision, (5) publication bias. Check
the starting level (high for RCTs, low for observational). Verify each downgrade has
explicit rationale. Check that upgrading criteria for observational evidence (large
effect, dose-response, all plausible confounding would reduce effect) are genuinely
met.

**Correction Approach:** Apply GRADE systematically using the published framework.
Document the rationale for each domain assessment. Use the GRADE handbook criteria
for deciding "serious" vs "very serious" concerns. Produce a summary-of-findings
table showing the rating for each outcome. Avoid "split the difference" reasoning —
apply each domain independently.

### Error 17 — Confusing ITT with per-protocol analysis

**Description:** LLMs confuse intention-to-treat (ITT) and per-protocol (PP)
analyses, misstate which is the primary analysis for different trial designs, or fail
to recognize when the difference between ITT and PP results suggests bias. ITT
preserves randomization and avoids attrition bias but may underestimate efficacy; PP
removes non-adherers but is vulnerable to selection bias. For superiority trials, ITT
is primary; for non-inferiority trials, both ITT and PP should be reported because
ITT is anti-conservative for non-inferiority.

**Example:** In a non-inferiority trial of a new NSAID vs ibuprofen for
osteoarthritis pain, LLM reports only the ITT result showing non-inferiority. The
per-protocol analysis (excluding 22% of patients who switched treatments) shows the
new NSAID is inferior. For non-inferiority trials, ITT is anti-conservative (biases
toward showing non-inferiority) because it dilutes real differences. Reporting only
ITT in a non-inferiority context can falsely support a non-inferiority conclusion.

**Detection Method:** Check the trial design (superiority vs non-inferiority vs
equivalence). Verify which analysis population is reported as primary. For
superiority: ITT should be primary. For non-inferiority: both ITT and PP should be
reported, and non-inferiority should be demonstrated in BOTH populations. If only one
is reported, the omitted analysis may have shown different results. Check the
proportion of protocol deviations — if high (>10%), ITT and PP may diverge
substantially.

**Correction Approach:** Report both ITT and PP analyses for all trial designs. For
non-inferiority trials: require non-inferiority in both ITT and PP populations.
State the proportion of protocol deviations, reasons for deviation, and whether
deviations were balanced between arms. Discuss whether the ITT-PP divergence affects
conclusions.

### Error 18 — Wrong MeSH term selection for systematic review searches

**Description:** LLMs select inappropriate or incomplete MeSH terms for systematic
review search strategies, leading to missed studies or excessive noise. Common errors
include using only free-text terms without MeSH equivalents, using non-existent MeSH
terms, confusing MeSH with Emtree (EMBASE thesaurus), or failing to explode MeSH
terms to capture narrower headings.

**Example:** For a systematic review of hydrotherapy, LLM constructs a PubMed search
using MeSH term "Hydrotherapy" without realizing that "Balneology" is a separate
MeSH term and many relevant studies are indexed under "Balneology" but not
"Hydrotherapy." The search also omits "Mineral Waters" and "Hot
Temperature/therapeutic use." Missing these terms means the search will fail to
retrieve studies indexed primarily under these alternative headings, potentially
missing 30-40% of relevant literature.

**Detection Method:** Verify each MeSH term exists in the NLM MeSH Browser
(meshb.nlm.nih.gov). Check that the search includes both MeSH terms (for indexed
articles) and free-text synonyms (for recently published, not-yet-indexed articles).
Verify that MeSH terms are exploded where appropriate. Cross-check the search
against known relevant studies — if key studies are not retrieved, the search
strategy is incomplete.

**Correction Approach:** Build search strategies iteratively: start with known
relevant studies, identify their MeSH indexing, then construct the search to retrieve
those studies plus additional ones. Use both MeSH and free-text terms combined with
OR. Test the search against a validation set of known relevant studies. Consult a
medical librarian for complex searches.

---

## Category 4: Evidence Assessment Errors

Errors in evaluating the quality, applicability, and completeness of medical
evidence bodies.

### Error 19 — Publication bias unawareness

**Description:** LLMs summarize published evidence without acknowledging that the
available literature is a biased sample of all conducted research. Studies with
positive or statistically significant results are more likely to be published,
creating a systematically distorted evidence base. LLMs rarely spontaneously assess
publication bias or note its likely direction, and when they do, they often
misinterpret funnel plot asymmetry.

**Example:** LLM summarizes "All 6 published RCTs of mud therapy for knee
osteoarthritis show statistically significant pain reduction." This sounds
compelling but ignores that negative trials are less likely to be published. Trial
registries (ClinicalTrials.gov, WHO ICTRP) show 11 registered trials of mud therapy
for knee OA — 5 have no published results. If the unpublished 5 were negative, the
true effect is likely smaller than the published summary suggests. The LLM's
uncritical summary of "all published studies agree" may reflect publication bias,
not true efficacy.

**Detection Method:** Check trial registries for registered but unpublished studies.
Assess funnel plot symmetry and perform Egger's test when >= 10 studies are
available. Compare the number of registered trials with published results. Use GRADE
to assess the likelihood of publication bias. Look for the "too good to be true"
pattern: if all available studies are positive, especially small ones, publication
bias is likely.

**Correction Approach:** Always check trial registries for unpublished studies.
Include funnel plots and formal tests for asymmetry when the number of studies
permits. Note the direction and likely impact of publication bias on pooled
estimates. Use selection models (Copas, Vevea-Hedges) or trim-and-fill as
sensitivity analyses. Apply GRADE downgrading for publication bias when warranted.

### Error 20 — Inappropriate pooling of heterogeneous studies

**Description:** LLMs pool studies in meta-analysis that should not be combined due
to fundamental differences in populations, interventions, comparators, or outcomes.
The statistical test for heterogeneity (Q-test, I-squared) may suggest pooling is
acceptable even when clinical heterogeneity makes the pooled estimate meaningless.
LLMs focus on statistical homogeneity while ignoring clinical and methodological
heterogeneity.

**Example:** LLM pools three balneotherapy studies: Study A tested sulfur-rich water
at 38C for knee OA, Study B tested radon-containing water at 34C for ankylosing
spondylitis, and Study C tested Dead Sea mineral water at 37C for psoriatic
arthritis. Despite different mineral compositions, temperatures, diseases, and
pathophysiological mechanisms, the LLM combines them because "all are balneotherapy
studies" and reports a pooled SMD. The pooled estimate is statistically homogeneous
(I-squared = 15%) but clinically meaningless — it answers no useful clinical
question.

**Detection Method:** Assess clinical heterogeneity BEFORE statistical
heterogeneity. Ask: "Would a clinician treat these populations with these
interventions interchangeably?" If not, pooling is inappropriate regardless of
I-squared. Check whether the pooled estimate answers a meaningful clinical question.
Verify that the PICO elements are sufficiently similar across studies to justify a
single summary effect.

**Correction Approach:** Pool only studies that are clinically similar enough that a
single summary estimate is meaningful. When clinical heterogeneity precludes pooling,
use narrative synthesis or present results in subgroups defined by clinically
meaningful characteristics (disease, intervention type, comparator). Always assess
clinical heterogeneity before statistical heterogeneity.

### Error 21 — Wrong risk of bias domain judgments

**Description:** LLMs make incorrect risk-of-bias assessments using RoB2 (for RCTs)
or ROBINS-I (for observational studies), either by misclassifying domains, by being
systematically too lenient or too harsh, or by applying criteria from one tool to the
other. Common errors include rating an open-label trial as "low risk" for blinding
when the outcome is subjective, or rating a trial with 40% attrition as "low risk"
for missing outcome data.

**Example:** For an RoB2 assessment of a balneotherapy RCT: the trial is open-label
(patients know they receive thermal water vs tap water), the primary outcome is
patient-reported pain (VAS), and 25% of patients dropped out differentially (18% in
control vs 8% in treatment). LLM rates Domain 4 (measurement of outcome) as "low
risk" because "validated instruments were used." This is wrong — with an open-label
design and subjective outcome, performance and detection bias are serious concerns.
The LLM confused instrument validity with blinding adequacy.

**Detection Method:** Verify each RoB2 domain against the signaling questions in the
tool. Domain 1 (randomization): adequate generation and concealment? Domain 2
(deviations): blinding of participants and personnel? Domain 3 (missing data):
differential attrition? Domain 4 (measurement): blinded outcome assessment for
subjective outcomes? Domain 5 (selection): pre-registered protocol available? Check
that the overall judgment follows the "worst domain" rule (overall high risk if any
domain is high risk).

**Correction Approach:** Apply RoB2 and ROBINS-I using the official signaling
questions, not general impressions. For each domain, answer the signaling questions
sequentially and derive the domain judgment from the algorithm. Document the
rationale for each judgment. For subjective outcomes in open-label trials, default
to "some concerns" or "high risk" for bias in measurement of the outcome.

### Error 22 — Ignoring PICO mismatches across studies

**Description:** LLMs fail to identify or adequately address differences in PICO
elements across studies included in a review, treating all studies as if they
answered the same question. Subtle PICO mismatches — different disease severity
thresholds, different outcome measurement instruments, different follow-up
durations — can make pooled estimates misleading even when the broad PICO appears
similar.

**Example:** A review of balneotherapy for osteoarthritis pools studies using WOMAC
total score (0-96) with studies using WOMAC pain subscale (0-20) with studies using
VAS pain (0-100). While all measure "pain-related outcomes," they capture different
constructs (total disability vs pain component vs pain intensity). LLM converts all
to standardized mean differences and pools them, but the SMD interpretation differs
because the constructs differ. A 0.5 SD improvement in WOMAC total is not clinically
equivalent to a 0.5 SD improvement in VAS pain.

**Detection Method:** Compare outcome definitions, measurement instruments, and
timepoints across included studies. Check whether different instruments measure the
same construct. Verify that the population severity, disease stage, and comorbidity
profiles are comparable. Assess whether follow-up durations are similar enough that
effects at different timepoints can be meaningfully combined.

**Correction Approach:** Group studies by specific outcome instrument when possible.
When using SMD to combine different instruments, acknowledge the construct
differences and interpret cautiously. Perform subgroup analyses by instrument type,
follow-up duration, and population characteristics. Present individual study results
alongside any pooled estimate so readers can assess comparability.

---

## Category 5: Clinical Interpretation Errors

Errors in translating statistical findings into clinical meaning and applicability.

### Error 23 — Statistical significance vs clinical significance confusion

**Description:** LLMs treat statistically significant results as clinically
meaningful and statistically non-significant results as clinically unimportant,
without considering effect size magnitude, minimum clinically important difference
(MCID), or precision. A large trial can detect trivially small effects as
statistically significant; a small trial can miss clinically important effects due to
low power.

**Example:** A large RCT (N=2,000) finds that a new analgesic reduces pain by 3 mm
on a 100 mm VAS compared to placebo (p=0.01, 95% CI 1-5 mm). The MCID for VAS pain
is 10-15 mm. LLM states: "The new analgesic demonstrated statistically significant
pain reduction (p=0.01)," implying clinical benefit. The 3 mm reduction is far below
the MCID — patients would not perceive this difference. Conversely, a small pilot
study (N=30) of balneotherapy shows 15 mm pain reduction (not significant, p=0.08,
95% CI -2 to 32 mm). The effect size is clinically meaningful but imprecisely
estimated. LLM dismisses this as "no significant effect."

**Detection Method:** Compare effect sizes against established MCIDs for the outcome.
For pain VAS: MCID is approximately 10-15 mm. For WOMAC: MCID varies by subscale.
For SF-36: MCID is approximately 3-5 points per domain. Check whether the CI
excludes clinically trivial effects (not just zero). A statistically significant
result whose CI includes effects below the MCID may not be clinically meaningful.

**Correction Approach:** Always report effect sizes alongside p-values. Compare
effect sizes to established MCIDs. Interpret CIs in relation to both zero
(statistical significance) and the MCID (clinical significance). Use the framework:
significant + clinically meaningful (clear benefit), significant + clinically trivial
(statistical artifact of large N), non-significant + CI excludes MCID (evidence of
no meaningful effect), non-significant + CI includes MCID (inconclusive,
underpowered).

### Error 24 — Ignoring baseline risk in absolute effect calculations

**Description:** LLMs report relative effects without accounting for baseline risk,
which determines the absolute clinical impact. A 30% relative risk reduction means
very different things depending on baseline risk: for a 1% baseline risk, ARR is
0.3% (NNT=333); for a 50% baseline risk, ARR is 15% (NNT=7). LLMs often present
relative effects as if they have universal clinical meaning independent of baseline
risk.

**Example:** LLM states "Balneotherapy reduces the risk of OA flares by 40%
(RR 0.60)." Without baseline risk, this is uninterpretable. If baseline flare rate
is 80% per year (severe OA), ARR = 32% and NNT = 3 — highly clinically relevant. If
baseline flare rate is 5% per year (mild OA), ARR = 2% and NNT = 50 — modest
benefit. The LLM's failure to anchor the relative effect to a baseline risk makes
the finding sound uniformly impressive when its clinical impact varies enormously
across patient populations.

**Detection Method:** Check whether absolute effects (ARR, NNT) are provided
alongside relative effects (RR, OR, HR). Verify that baseline risks are stated for
the relevant population. When relative effects are applied to clinical
decision-making, check whether the baseline risk of the target population matches
the study population. Compute absolute effects at multiple clinically relevant
baseline risks.

**Correction Approach:** Always pair relative effects with absolute effects computed
at clinically relevant baseline risks. Use visual aids (Cates plots, icon arrays) to
communicate absolute effects to patients. When evidence comes from populations with
different baseline risks than the target population, explicitly compute the expected
absolute effect for the target population.

### Error 25 — Wrong extrapolation across populations

**Description:** LLMs apply evidence from one population to a different population
without justification or without noting the extrapolation. Treatment effects can
differ across age groups, ethnicities, disease severities, and healthcare settings.
LLMs frequently generalize trial results from specialized academic centers to
community settings, from adults to elderly, or from one disease subtype to another.

**Example:** Evidence for balneotherapy in fibromyalgia comes primarily from studies
in European health resorts among middle-aged women with moderate disease severity and
no major psychiatric comorbidity. LLM applies these results to "fibromyalgia
patients" generally, including elderly patients, men, patients with severe disease,
and those with comorbid depression. The original study populations may not represent
these subgroups, and the treatment effect may differ. Additionally, the health resort
context (3-week residential treatment with multiple co-interventions) may not
translate to outpatient thermal pool sessions.

**Detection Method:** Check the population characteristics of the evidence source
against the target population. Identify potential effect modifiers: age, sex, disease
severity, comorbidities, treatment setting, healthcare system. Verify whether
subgroup analyses support or refute homogeneity of treatment effect across relevant
subgroups. Note when evidence is being extrapolated beyond the studied population.

**Correction Approach:** State the population characteristics of the evidence base
explicitly. When extrapolating to different populations, acknowledge the
extrapolation and state the assumptions. Identify potential effect modifiers and
whether they were tested. When possible, use individual patient data meta-analysis
to explore treatment effect heterogeneity across patient characteristics.

### Error 26 — Ignoring competing risks

**Description:** LLMs analyze time-to-event outcomes without accounting for
competing risks — events that prevent the outcome of interest from occurring. In
elderly populations, death from any cause competes with disease-specific events.
Standard Kaplan-Meier and Cox regression treat competing events as censoring, which
is informative (not random) and biases the estimated cumulative incidence upward.
LLMs rarely recognize when competing risks are relevant or recommend appropriate
methods (Fine-Gray subdistribution hazard, cause-specific hazard).

**Example:** In a study of balneotherapy for prevention of falls in elderly patients,
the outcome is time to first fall. Patients who die before falling are censored in
the Kaplan-Meier analysis. If mortality differs between treatment groups (e.g.,
sicker patients in one group die before they can fall), the KM estimate of
cumulative fall incidence is biased. In an elderly population with high mortality,
this bias can be substantial — overestimating the cumulative incidence of falls in
both groups but differentially depending on mortality rates.

**Detection Method:** Assess whether competing events are present. Common competing
risks in medical research: death (competes with any non-fatal outcome), disease
progression (competes with treatment response), loss to follow-up due to health
deterioration. When competing risks are present, verify that appropriate methods were
used: cause-specific hazards AND cumulative incidence functions (Aalen-Johansen
estimator) rather than 1-KM. Check whether the Fine-Gray model was considered.

**Correction Approach:** When competing risks are present, report both cause-specific
hazard ratios (for etiology) and subdistribution hazard ratios (for
prognosis/prediction). Use the Aalen-Johansen estimator instead of 1-KM for
cumulative incidence. Acknowledge which competing events were considered and how they
were handled. In elderly populations, always assess whether death is a relevant
competing risk.

### Error 27 — Misinterpreting non-inferiority trial results

**Description:** LLMs misinterpret non-inferiority trials by: (1) treating a
non-inferiority conclusion as evidence of equivalence or superiority, (2) ignoring
the non-inferiority margin and its clinical justification, (3) not recognizing that
the margin should preserve a clinically meaningful fraction of the active
comparator's effect, or (4) switching between superiority and non-inferiority
interpretations post-hoc. Non-inferiority does not mean "equally effective" — it
means "not unacceptably worse."

**Example:** A non-inferiority trial compares a new spa treatment to established
physiotherapy for chronic low back pain, with a non-inferiority margin of 15 mm on
VAS pain (0-100). The result shows the spa treatment is 12 mm worse than
physiotherapy (95% CI: 2-22 mm worse). The upper bound of the CI (22 mm) exceeds
the margin (15 mm), so non-inferiority is not demonstrated. LLM states: "The spa
treatment showed similar pain reduction to physiotherapy, with only a 12 mm
difference." This misrepresents a failed non-inferiority trial as a positive
finding.

**Detection Method:** Verify the non-inferiority margin is stated and clinically
justified (typically preserving 50-80% of the active comparator's effect over
placebo). Check whether the entire CI falls within the margin (non-inferiority
demonstrated) or the CI crosses the margin (non-inferiority not demonstrated).
Assess whether the margin was pre-specified or chosen post-hoc. Verify that the
interpretation matches the statistical result.

**Correction Approach:** Report the non-inferiority margin, its justification, and
whether the CI is entirely within the margin. Use a figure showing the CI relative
to the margin and zero. Do not describe a failed non-inferiority trial as showing
"similar" or "comparable" results. Distinguish non-inferiority (not unacceptably
worse) from equivalence (truly similar) and superiority (better).

### Error 28 — Misinterpreting diagnostic accuracy measures

**Description:** LLMs confuse sensitivity, specificity, positive predictive value
(PPV), and negative predictive value (NPV), or fail to account for prevalence when
interpreting predictive values. High sensitivity and specificity do not guarantee
high PPV in low-prevalence settings. LLMs also confuse pre-test probability with
prevalence and post-test probability with predictive value.

**Example:** A screening test for a rare condition has sensitivity 95% and
specificity 95%. LLM states "the test correctly identifies 95% of cases and has a
95% chance of being correct when positive." In a population with 1% prevalence:
PPV = (0.95 * 0.01) / (0.95 * 0.01 + 0.05 * 0.99) = 0.161, meaning only 16% of
positive tests are true positives. The LLM's "95% chance of being correct" is wrong
by a factor of 6. This error leads to overconfident clinical decision-making based
on positive screening results.

**Detection Method:** Verify that predictive values are computed with the appropriate
prevalence for the clinical setting. Check whether sensitivity/specificity (which
are prevalence-independent) are being confused with PPV/NPV (which are
prevalence-dependent). Apply Bayes' theorem: PPV = (sensitivity * prevalence) /
(sensitivity * prevalence + (1-specificity) * (1-prevalence)). Verify that the
LLM's interpretation accounts for the clinical setting's prevalence, not the study's
case-control design prevalence.

**Correction Approach:** Always state the prevalence or pre-test probability when
reporting predictive values. Compute PPV and NPV for the target clinical setting's
prevalence, not the study's prevalence. Use likelihood ratios (which are
prevalence-independent) for communicating diagnostic test performance across
settings. Present Fagan nomograms or 2x2 natural frequency tables for clinical
communication.

### Error 29 — Overlooking multiplicity and multiple comparisons

**Description:** LLMs fail to flag or adjust for multiple statistical tests. When a
study tests 20 outcomes, approximately 1 will be "significant" at p<0.05 by chance
alone. LLMs often highlight individual significant p-values from exploratory analyses
without noting the multiplicity problem or whether adjustments (Bonferroni, Holm,
Benjamini-Hochberg) were applied.

**Example:** A balneotherapy RCT tests 15 secondary outcomes. LLM highlights the 2
that reached p<0.05 as "significant benefits" without noting that with 15 tests,
approximately 0.75 false positives are expected at alpha=0.05. After Bonferroni
correction (alpha = 0.05/15 = 0.0033), neither result is significant. The LLM's
selective reporting of unadjusted p-values inflates the apparent efficacy of the
intervention.

**Detection Method:** Count the number of statistical tests performed. Check whether
the study pre-specified a primary outcome or adjusted for multiple comparisons. For
exploratory analyses with many outcomes, apply the Bonferroni correction as a rough
check: p * number_of_tests > 0.05 suggests the finding may be a false positive.
Verify whether the study distinguished confirmatory (primary) from exploratory
(secondary) analyses.

**Correction Approach:** Clearly distinguish pre-specified primary outcomes from
exploratory secondary outcomes. Apply appropriate multiplicity adjustments for
multiple primary outcomes or co-primary endpoints. Report all tested outcomes, not
just significant ones. Label post-hoc and subgroup analyses as exploratory and
interpret accordingly.

### Error 30 — Misapplying meta-regression

**Description:** LLMs use meta-regression to explore sources of heterogeneity but
make common errors: testing too many covariates relative to the number of studies
(minimum 10 studies per covariate), drawing ecological inferences about individual
patients from study-level associations, or treating meta-regression R-squared as
evidence of a causal relationship between the covariate and the effect size.

**Example:** With 8 studies of balneotherapy for OA, LLM performs meta-regression
with 4 covariates (treatment duration, water temperature, mineral concentration,
mean patient age). With only 8 studies and 4 covariates, the analysis is severely
underpowered and prone to overfitting. LLM reports "higher water temperature was
associated with larger treatment effects (p=0.04, R-squared=0.52)." This finding is
unreliable due to limited studies, is an ecological association (study-level, not
patient-level), and multiple testing across 4 covariates was not adjusted for.

**Detection Method:** Check the ratio of studies to covariates (minimum 10:1 per
Cochrane guidance). Verify that ecological fallacy is acknowledged — study-level
associations do not apply to individuals. Check for multiplicity adjustment if
multiple covariates were tested. Assess whether confounding between covariates was
addressed (e.g., treatment duration and temperature may be correlated with study
quality).

**Correction Approach:** Limit meta-regression covariates to 1 per 10 studies.
Pre-specify covariates based on clinical rationale, not data-driven selection.
Explicitly note the ecological level of analysis. Report all tested covariates, not
just significant ones. Use permutation tests for significance when the number of
studies is small. Present results as hypothesis-generating, not confirmatory.

### Error 31 — Misunderstanding crossover trial analysis

**Description:** LLMs analyze crossover trials as parallel-group trials, ignoring
period effects, carryover effects, and the paired nature of the data. In a crossover
design, each patient serves as their own control, and the paired analysis is more
powerful. Ignoring the pairing wastes statistical power. Failing to test for
carryover (the treatment in period 1 affecting outcomes in period 2) can invalidate
the entire crossover analysis.

**Example:** A crossover RCT of balneotherapy (2-week thermal water vs 2-week tap
water with 4-week washout) is analyzed by LLM as if it were a parallel-group trial:
comparing mean pain scores in the thermal water periods vs tap water periods without
accounting for within-patient pairing. This loses the primary advantage of the
crossover design (controlling for between-patient variability) and inflates the
standard error. The LLM also does not test for carryover effects, which would
indicate the 4-week washout was insufficient and the design is invalid.

**Detection Method:** Check whether the analysis accounts for the paired structure
(paired t-test, mixed model with patient as random effect). Verify that carryover
was tested (typically by comparing period 1 results between sequences). Check
whether the washout period is clinically justified (long enough for the treatment
effect to dissipate). For studies where carryover is detected, verify that only
period 1 data is used.

**Correction Approach:** Analyze crossover trials using paired methods. Test for
carryover effects (sequence-by-period interaction). If carryover is significant, use
only period 1 data (effectively a parallel-group analysis). Report within-patient
differences, not between-group differences. Justify the washout duration based on
the intervention's known duration of effect.

### Error 32 — Failure to assess indirectness of evidence

**Description:** LLMs fail to downgrade evidence for indirectness — when the
available evidence does not directly match the clinical question in terms of
population, intervention, comparator, or outcome. This is a distinct GRADE domain
that LLMs frequently overlook, especially when the mismatch is subtle (e.g.,
surrogate outcomes, comparisons via a common comparator rather than head-to-head).

**Example:** Evidence for balneotherapy in osteoarthritis comes from studies
measuring pain at 2-4 weeks post-treatment. The clinical question is about long-term
functional outcomes at 6-12 months. LLM presents the short-term pain evidence as
directly answering the long-term function question. Short-term pain reduction may
not translate to sustained functional improvement — the evidence is indirect for the
actual clinical question. Similarly, if the clinical question compares balneotherapy
to exercise therapy but only placebo-controlled studies exist, the comparison is
indirect.

**Detection Method:** For each GRADE domain, assess: (1) Population indirectness: do
study populations match the target population? (2) Intervention indirectness: does
the studied intervention match the intervention of interest? (3) Comparator
indirectness: is the comparison direct (head-to-head) or indirect (via common
comparator)? (4) Outcome indirectness: are the outcomes measured the ones of clinical
interest, or are they surrogates?

**Correction Approach:** Explicitly assess indirectness for each GRADE domain. When
evidence is indirect, state the nature of the indirectness and its likely direction
of bias. For indirect comparisons, use network meta-analysis methodology and note
the transitivity assumption. For surrogate outcomes, assess the strength of the
surrogate-clinical outcome relationship. Downgrade GRADE certainty for serious
indirectness.

### Error 33 — Confusing per-patient and per-event analyses

**Description:** LLMs confuse analyses that count patients (proportion experiencing
at least one event) with analyses that count events (total number of events). For
recurrent outcomes (falls, exacerbations, adverse events), the choice between
per-patient and per-event analysis affects both the effect estimate and its
interpretation. Per-event analyses must account for clustering within patients.

**Example:** A study of fall prevention reports 45 falls among 30 patients in the
treatment group vs 78 falls among 30 patients in the control group. LLM reports
"the treatment reduced falls by 42% (45/78)." This is a per-event rate ratio, not a
per-patient risk ratio. The per-patient analysis might show 18/30 (60%) vs 22/30
(73%) experiencing at least one fall, giving RR = 0.82 (18% reduction). The
per-event analysis gives a larger apparent effect because some patients in the
control group had many recurrent falls. Neither is wrong, but they answer different
questions.

**Detection Method:** Check whether the outcome is per-patient (proportion with at
least one event) or per-event (total events or event rate). Verify that per-event
analyses account for within-patient clustering (negative binomial, GEE, or frailty
models). Check whether the unit of analysis matches the unit of randomization.

**Correction Approach:** Report both per-patient and per-event analyses when outcomes
are recurrent. For per-event analyses, use methods that account for within-patient
correlation (negative binomial regression, GEE with robust standard errors). Clearly
state which analysis is being reported and what clinical question it answers.

### Error 34 — Mishandling cluster-randomized trials

**Description:** LLMs analyze cluster-randomized trials at the individual level
without accounting for the intra-cluster correlation, inflating the effective sample
size and producing falsely narrow confidence intervals. When cluster trials are
included in meta-analyses, failure to adjust for clustering biases the pooled
estimate and gives the cluster trial too much weight.

**Example:** A cluster-RCT randomizes 20 thermal spas (clusters) to offer
balneotherapy programs or usual care, with 50 patients per spa (N=1,000 total). LLM
analyzes this as if 1,000 individuals were independently randomized. With an
intra-cluster correlation (ICC) of 0.05, the design effect is
1 + (50-1) * 0.05 = 3.45, meaning the effective sample size is 1000/3.45 = 290, not
1000. CIs should be roughly sqrt(3.45) = 1.86 times wider than the LLM's naive
calculation. The LLM's analysis overstates precision by nearly 2-fold.

**Detection Method:** Check whether the randomization unit matches the analysis unit.
If clusters (hospitals, clinics, practices) were randomized but individuals are
analyzed, verify that clustering was accounted for (GEE, mixed models,
design-effect adjustment). For meta-analyses including cluster trials: verify that
the effective sample size was used, not the total number of individuals.

**Correction Approach:** Analyze cluster-randomized trials using methods that account
for clustering: mixed-effects models with cluster as random intercept, GEE with
exchangeable correlation, or design-effect adjustment. When including cluster trials
in meta-analyses, reduce the effective sample size by the design effect:
N_effective = N / (1 + (m-1)*ICC) where m is the average cluster size.

### Error 35 — Ecological fallacy in evidence interpretation

**Description:** LLMs draw individual-level conclusions from group-level (ecological)
data. In medical research, this occurs when population-level associations (countries
with higher spa use have lower arthritis prevalence) are interpreted as
individual-level effects (spa use reduces arthritis risk). The ecological fallacy
can produce associations that are opposite in direction to the individual-level
truth.

**Example:** LLM cites country-level data showing that Mediterranean countries with
high spa usage have lower cardiovascular mortality than Northern European countries
with low spa usage, concluding "spa therapy may reduce cardiovascular risk." This
ecological association ignores that Mediterranean populations differ in diet,
physical activity, climate, social cohesion, and many other factors. The spa-CVD
association at the country level does not establish an individual-level spa-CVD
effect. In fact, within Mediterranean countries, spa users and non-users may have
identical cardiovascular outcomes.

**Detection Method:** Identify the unit of observation: is the data at the individual
or group level? If group-level data is used to draw individual-level conclusions, the
ecological fallacy applies. Check whether individual-level studies (cohort,
case-control, RCT) support the same association. Be especially suspicious of
cross-country or cross-region comparisons used to support individual-level treatment
effects.

**Correction Approach:** Use individual-level data for individual-level conclusions.
When ecological data is the only evidence available, label it as ecological and note
the fallacy risk. Do not use ecological associations to justify clinical
recommendations without individual-level supporting evidence.
