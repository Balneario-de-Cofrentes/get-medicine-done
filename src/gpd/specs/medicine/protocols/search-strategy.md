---
load_when:
  - "search strategy"
  - "literature search"
  - "PubMed search"
  - "MEDLINE search"
  - "Embase search"
  - "Cochrane Library"
  - "MeSH terms"
  - "Boolean search"
  - "database search"
  - "search syntax"
  - "grey literature"
  - "search filter"
tier: 1
context_cost: medium
---

# Search Strategy Protocol for Systematic Reviews

A systematic review is only as good as its search. The search strategy must be sensitive enough to identify all relevant studies while being specific enough to keep the screening workload manageable. This protocol covers Boolean operators, controlled vocabulary, database-specific syntax, and documentation requirements for reproducible searches.

**Core principle:** A reproducible search is a documented search. Every element of the search strategy must be recorded in sufficient detail that another researcher can execute the identical search and retrieve the identical results.

## Related Protocols

- `pico-framework.md` -- PICO elements drive search term selection
- `prisma-systematic-review.md` -- PRISMA Items 6 and 7 (information sources and search strategy)
- `data-extraction.md` -- What to do with the studies found

---

## Search Development Process

### Step 1: Translate PICO into Search Concepts

Each PICO element becomes a search concept block:

```
PICO Question: In adults with type 2 diabetes (P), does metformin (I) compared
with sulfonylureas (C) reduce HbA1c (O)?

Search Concepts:
  Block 1 (P): type 2 diabetes
  Block 2 (I): metformin
  Block 3 (C): sulfonylureas
  Block 4 (O): HbA1c [often omitted to increase sensitivity]
```

**Sensitivity vs. Specificity trade-off:**

- Systematic reviews prioritize SENSITIVITY (finding all relevant studies).
- Omit outcome terms from the search unless the yield is unmanageable.
- Comparator terms may also be omitted if the comparator is broad (e.g., "any active comparator").
- Use study design filters only when necessary to manage yield.

### Step 2: Identify Search Terms for Each Concept

For each concept, identify:

1. **Controlled vocabulary terms** (MeSH for MEDLINE/PubMed, Emtree for Embase, MeSH for CENTRAL)
2. **Free-text terms** (synonyms, abbreviations, spelling variants, brand names)
3. **Truncation and wildcards** for capturing word variants

```
Concept: metformin

Controlled vocabulary (MeSH):
  "Metformin"[Mesh]

Free-text terms:
  metformin[tiab]
  glucophage[tiab]
  dimethylbiguanide[tiab]
  "dimethyl biguanide"[tiab]
```

### Step 3: Combine Terms

**Within a concept:** Use OR (union -- broadens the search)
**Between concepts:** Use AND (intersection -- narrows the search)

```
(diabetes terms) AND (metformin terms) AND (sulfonylurea terms)
```

### Step 4: Apply Limits and Filters

Filters should be used sparingly and only when validated:

| Filter Type | When to Use | Recommended Filters |
|------------|-------------|-------------------|
| Study design (RCT) | When only RCTs are eligible | Cochrane Highly Sensitive Search Strategy |
| Date | When updating a previous review | Search from the last search date of the previous review |
| Language | Generally avoid | Excluding non-English studies can introduce bias |
| Human studies | When animal studies are not relevant | Standard human filter |
| Age group | When the population is age-specific | Use with caution; indexing is imperfect |

---

## Database-Specific Syntax

### PubMed / MEDLINE

**Controlled vocabulary:** MeSH (Medical Subject Headings)

```
# MeSH term with explosion (includes narrower terms)
"Diabetes Mellitus, Type 2"[Mesh]

# MeSH term without explosion (exact heading only)
"Diabetes Mellitus, Type 2"[Mesh:NoExp]

# MeSH Major Topic (MeSH is a major focus of the article)
"Diabetes Mellitus, Type 2"[Majr]

# Free text in title and abstract
metformin[tiab]

# Free text in title only
metformin[ti]

# All fields search
metformin[All Fields]

# Truncation (finds metformin, metformins, etc.)
metformin*[tiab]

# Phrase searching
"type 2 diabetes"[tiab]

# Publication type
randomized controlled trial[pt]

# Subheading (qualifier)
"Diabetes Mellitus, Type 2/drug therapy"[Mesh]

# Date limit
"2020/01/01"[PDat] : "2024/12/31"[PDat]

# Language limit
English[lang]

# Human studies
Humans[Mesh]
```

**Complete PubMed search example:**

```
#1  "Diabetes Mellitus, Type 2"[Mesh]
#2  "type 2 diabetes"[tiab] OR "T2DM"[tiab] OR "NIDDM"[tiab] OR
    "non-insulin dependent diabetes"[tiab] OR "type II diabetes"[tiab]
#3  #1 OR #2
#4  "Metformin"[Mesh]
#5  metformin[tiab] OR glucophage[tiab] OR dimethylbiguanide[tiab]
#6  #4 OR #5
#7  "Sulfonylurea Compounds"[Mesh]
#8  sulfonylurea*[tiab] OR glipizide[tiab] OR glyburide[tiab] OR
    glibenclamide[tiab] OR glimepiride[tiab] OR gliclazide[tiab]
#9  #7 OR #8
#10 #3 AND #6 AND #9
#11 randomized controlled trial[pt] OR controlled clinical trial[pt] OR
    randomized[tiab] OR randomised[tiab] OR placebo[tiab] OR
    "drug therapy"[sh] OR randomly[tiab] OR trial[tiab] OR groups[tiab]
#12 animals[Mesh] NOT humans[Mesh]
#13 #11 NOT #12
#14 #10 AND #13
```

### Embase (via Ovid)

**Controlled vocabulary:** Emtree

```
# Emtree term with explosion (default)
exp non insulin dependent diabetes mellitus/

# Emtree term without explosion
*non insulin dependent diabetes mellitus/

# Free text in title
metformin.ti.

# Free text in title and abstract
metformin.ti,ab.

# Free text in title, abstract, and keywords
metformin.ti,ab,kw.

# Truncation
metformin$.ti,ab.

# Adjacency operator (words within N words of each other)
(type adj2 diabetes).ti,ab.

# Combining sets
1 and 2
1 or 2

# Limiting to humans
limit to human

# Limiting by date
limit to yr="2020-2024"

# Subheading
exp non insulin dependent diabetes mellitus/dt [Drug Therapy]
```

**Key difference from PubMed:** Embase uses the adjacency operator (adj) instead of phrase searching for flexible proximity matching. `(type adj2 diabetes)` finds "type 2 diabetes," "type II diabetes," and "type two diabetes."

### Cochrane Library (CENTRAL)

**Controlled vocabulary:** MeSH (same as MEDLINE)

```
# MeSH descriptor with explosion
[mh "Diabetes Mellitus, Type 2"]

# MeSH descriptor without explosion
[mh ^"Diabetes Mellitus, Type 2"]

# Title and abstract
metformin:ti,ab

# Title only
metformin:ti

# Keyword
metformin:kw

# Truncation
metformin*:ti,ab

# Proximity (within N words)
(type NEAR/2 diabetes):ti,ab

# Combining
#1 AND #2
#1 OR #2
```

### Additional Databases

| Database | Syntax | Controlled Vocabulary | Key Differences |
|----------|--------|----------------------|----------------|
| CINAHL (via EBSCOhost) | Similar to PubMed | CINAHL Subject Headings | Nursing and allied health focus |
| PsycINFO (via Ovid) | Ovid syntax | Thesaurus of Psychological Index Terms | Psychology and behavioral science |
| Web of Science | Topic (TS), Title (TI), Author (AU) | No controlled vocabulary | Citation searching; NEAR operator |
| Scopus | TITLE-ABS-KEY(), TITLE() | No controlled vocabulary | Largest abstract/citation database |
| WHO ICTRP | Simple keyword search | None | Trial registrations only |
| ClinicalTrials.gov | Condition, Intervention fields | None | U.S. trial registrations |

---

## Grey Literature Searching

Grey literature reduces publication bias by including unpublished and non-commercially published studies.

### Sources to Search

| Source | Content | URL |
|--------|---------|-----|
| ClinicalTrials.gov | Trial registrations and results | https://clinicaltrials.gov |
| WHO ICTRP | International trial registrations | https://trialsearch.who.int |
| OpenGrey | European grey literature | http://www.opengrey.eu |
| ProQuest Dissertations | Theses and dissertations | https://www.proquest.com |
| Conference abstracts | Conference proceedings (via Embase, Web of Science) | Database-specific |
| Preprint servers | medRxiv, bioRxiv | https://www.medrxiv.org |
| Regulatory submissions | FDA, EMA databases | https://www.fda.gov, https://www.ema.europa.eu |

### Supplementary Search Methods

| Method | Description | When to Use |
|--------|------------|-------------|
| Reference list checking | Screen reference lists of included studies and relevant reviews | Always |
| Citation tracking | Forward citation search of included studies | When key landmark studies exist |
| Contacting experts | Email researchers in the field | For unpublished data, ongoing trials |
| Hand-searching | Manually reviewing key journals | For journals not fully indexed |

---

## Search Filters

### Cochrane Highly Sensitive Search Strategy for RCTs (PubMed)

**Sensitivity-maximizing version:**

```
randomized controlled trial[pt]
OR controlled clinical trial[pt]
OR randomized[tiab]
OR randomised[tiab]
OR placebo[tiab]
OR "drug therapy"[sh]
OR randomly[tiab]
OR trial[tiab]
OR groups[tiab]
NOT (animals[Mesh] NOT humans[Mesh])
```

**Sensitivity- and precision-maximizing version:**

```
randomized controlled trial[pt]
OR controlled clinical trial[pt]
OR randomized[tiab]
OR randomised[tiab]
OR placebo[tiab]
OR "clinical trials as topic"[Mesh:NoExp]
OR randomly[tiab]
NOT (animals[Mesh] NOT humans[Mesh])
```

### Systematic Review Filter (PubMed)

```
systematic review[pt]
OR meta-analysis[pt]
OR "systematic review"[tiab]
OR "meta-analysis"[tiab]
OR "MEDLINE"[tiab]
OR "Cochrane"[tiab]
OR "systematic search"[tiab]
```

---

## Documenting the Search

### Required Documentation (PRISMA Items 6-7)

For each database searched, record:

| Element | Example |
|---------|---------|
| Database name and platform | MEDLINE via PubMed |
| Date of search | 2026-03-15 |
| Date range covered | Inception to 2026-03-15 |
| Complete search strategy | Full line-by-line strategy with hit counts |
| Filters applied | Cochrane RCT filter, Humans |
| Total records retrieved | 1,247 |

### Search Strategy Documentation Template

```
Database: MEDLINE via PubMed
Date searched: 2026-03-15
Date range: inception to 2026-03-15

#   Search terms                                          Results
1   "Diabetes Mellitus, Type 2"[Mesh]                    198,432
2   "type 2 diabetes"[tiab] OR "T2DM"[tiab] ...         245,891
3   #1 OR #2                                              312,567
4   "Metformin"[Mesh]                                      28,432
5   metformin[tiab] OR glucophage[tiab] ...                32,109
6   #4 OR #5                                               35,672
7   "Sulfonylurea Compounds"[Mesh]                         12,456
8   sulfonylurea*[tiab] OR glipizide[tiab] ...             15,234
9   #7 OR #8                                               18,901
10  #3 AND #6 AND #9                                        2,345
11  Cochrane RCT filter                                    ---
12  #10 AND #11                                              876
```

### Peer Review of Search Strategy

The PRESS (Peer Review of Electronic Search Strategies) checklist should be used to peer-review the search before execution:

1. **Translation of the research question:** Do the search concepts match the PICO?
2. **Boolean and proximity operators:** Correctly used (OR within concepts, AND between)?
3. **Subject headings:** Appropriate MeSH/Emtree selected? Explosion correct?
4. **Text word searching:** Synonyms, abbreviations, truncation, spelling variants included?
5. **Spelling, syntax, line numbers:** No typographical errors? Correct syntax for the platform?
6. **Limits and filters:** Appropriate? Validated? Potentially excluding relevant studies?

---

## Common Search Errors

| Error | Impact | Prevention |
|-------|--------|------------|
| Missing synonyms | Relevant studies not found | Use multiple databases, check MeSH tree, consult domain experts |
| Incorrect Boolean logic | Wrong combination of terms | AND narrows, OR broadens -- verify logic with sample known references |
| Over-reliance on MeSH | Recently published articles not yet indexed | Always combine MeSH with free-text terms |
| Not exploding MeSH terms | Missing studies indexed under narrower terms | Use explosion unless you specifically need the exact heading |
| Truncating too aggressively | Irrelevant results (e.g., "met*" retrieves "metabolic") | Truncate at a meaningful root (e.g., "metformin" not "met*") |
| Language restrictions | Potential bias toward English-language studies | Avoid language restrictions; translate if needed |
| Not searching grey literature | Publication bias | Include trial registries, preprints, conference abstracts |
| Not checking known references | No validation of search sensitivity | Test search against a set of known included studies from previous reviews |

---

## Search Validation

### Reference Set Validation

Before finalizing the search strategy:

1. Identify 5-10 studies known to be relevant (from previous reviews, scoping searches, expert consultation).
2. Run the search strategy.
3. Verify that all known relevant studies are retrieved.
4. If any are missed, analyze why and modify the strategy.

This is the search analogue of the predict-derive-verify cycle: you predict what the search should find, execute it, and verify.

---

## Verification Checklist

Before finalizing the search strategy:

- [ ] PICO elements translated into search concepts
- [ ] Both controlled vocabulary (MeSH/Emtree) and free-text terms included for each concept
- [ ] Synonyms, abbreviations, spelling variants, and brand names included
- [ ] Boolean logic correct (OR within concepts, AND between)
- [ ] Truncation used appropriately (not too aggressively)
- [ ] Search tested on at least one database and validated against known references
- [ ] Search adapted for each database (syntax differs between PubMed, Embase, CENTRAL)
- [ ] Grey literature sources included (trial registries, preprints)
- [ ] Supplementary methods planned (reference checking, citation tracking)
- [ ] PRESS peer review completed
- [ ] Complete documentation for each database (date, strategy, results count)
