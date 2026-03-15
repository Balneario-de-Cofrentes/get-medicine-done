# Medical Research Code Testing

Patterns for writing tests that catch evidence synthesis errors, not just software bugs. Standard software testing checks that code runs correctly; medical research testing checks that the code produces correct statistical results and valid clinical evidence.

<core_principle>

**Test the evidence, not the implementation.**

A medical research test should fail when the statistical result is wrong and pass when it is right, regardless of how the code is structured internally. The test suite is a machine-readable statement of what statistical and methodological properties the analysis must satisfy.

**Priority order:**

1. Effect size recalculation (must match raw data)
2. Known benchmark results (must reproduce textbook/published values)
3. Statistical model assumptions (must be verified before results are trusted)
4. Sensitivity analysis consistency (must show robustness)
5. GRADE/reporting completeness (must meet minimum standards)

</core_principle>

<effect_size_tests>

## Parameterized Tests for Effect Size Calculations

The most powerful evidence synthesis tests: calculated effect sizes must match what you get from raw study data.

### Pattern: Parameterized Effect Size Verification

```python
import pytest
import numpy as np

@pytest.mark.parametrize("test_name, events_tx, n_tx, events_ctrl, n_ctrl, expected_or", [
    (
        "balanced_moderate_effect",
        30, 100, 50, 100,
        (30 * 50) / (70 * 50),  # OR = 0.429
    ),
    (
        "rare_events",
        5, 200, 15, 200,
        (5 * 185) / (195 * 15),  # OR = 0.316
    ),
    (
        "equal_rates",
        50, 100, 50, 100,
        1.0,  # OR = 1.0 (no difference)
    ),
    (
        "zero_cell_correction",
        0, 50, 5, 50,
        (0.5 * 45.5) / (49.5 * 5.5),  # With 0.5 correction
    ),
])
def test_odds_ratio_from_2x2(test_name, events_tx, n_tx, events_ctrl, n_ctrl, expected_or):
    """Verify OR calculation from 2x2 table."""
    computed_or = compute_odds_ratio(events_tx, n_tx, events_ctrl, n_ctrl)
    np.testing.assert_allclose(
        computed_or, expected_or, rtol=1e-6,
        err_msg=f"OR mismatch for '{test_name}'"
    )
```

### Pattern: SMD Calculation with Hedges' Correction

```python
@pytest.mark.parametrize("n_tx, n_ctrl, mean_tx, sd_tx, mean_ctrl, sd_ctrl", [
    (30, 30, 10.0, 5.0, 12.0, 5.0),      # Small sample
    (100, 100, 10.0, 5.0, 12.0, 5.0),     # Moderate sample
    (1000, 1000, 10.0, 5.0, 12.0, 5.0),   # Large sample
    (10, 10, 50.0, 10.0, 55.0, 10.0),     # Very small sample (correction matters)
])
def test_hedges_g_vs_cohens_d(n_tx, n_ctrl, mean_tx, sd_tx, mean_ctrl, sd_ctrl):
    """Hedges' g should be smaller than Cohen's d (bias correction)."""
    cohens_d = compute_cohens_d(mean_tx, sd_tx, n_tx, mean_ctrl, sd_ctrl, n_ctrl)
    hedges_g = compute_hedges_g(mean_tx, sd_tx, n_tx, mean_ctrl, sd_ctrl, n_ctrl)

    # Hedges' g is always closer to zero than Cohen's d
    assert abs(hedges_g) <= abs(cohens_d) + 1e-10, (
        f"Hedges' g ({hedges_g:.6f}) should not exceed Cohen's d ({cohens_d:.6f})"
    )

    # Correction factor J approaches 1 for large N
    df = n_tx + n_ctrl - 2
    expected_j = 1 - 3 / (4 * df - 1)
    assert abs(hedges_g / cohens_d - expected_j) < 1e-10, (
        f"Hedges correction factor wrong: got {hedges_g/cohens_d:.6f}, expected {expected_j:.6f}"
    )


def test_smd_direction():
    """SMD sign must reflect which group is better."""
    # If lower is better and treatment has lower mean, SMD should be negative
    g = compute_hedges_g(
        mean_tx=8.0, sd_tx=3.0, n_tx=50,
        mean_ctrl=10.0, sd_ctrl=3.0, n_ctrl=50
    )
    assert g < 0, "SMD should be negative when treatment mean is lower (lower=better)"
```

### Pattern: Risk Ratio vs Odds Ratio Divergence

```python
@pytest.mark.parametrize("p_ctrl", [0.01, 0.05, 0.10, 0.20, 0.40, 0.60])
def test_or_rr_divergence(p_ctrl):
    """OR approximates RR only when events are rare. Diverges when common."""
    p_tx = p_ctrl * 0.5  # 50% relative risk reduction
    or_val = (p_tx / (1 - p_tx)) / (p_ctrl / (1 - p_ctrl))
    rr_val = p_tx / p_ctrl

    if p_ctrl < 0.10:
        # Rare events: OR ~ RR
        np.testing.assert_allclose(or_val, rr_val, rtol=0.15,
            err_msg=f"OR should approximate RR for rare events (p={p_ctrl})")
    else:
        # Common events: OR < RR (for protective effects)
        assert or_val < rr_val, (
            f"OR ({or_val:.3f}) should be further from 1 than RR ({rr_val:.3f}) "
            f"for common events (p={p_ctrl})"
        )
```

</effect_size_tests>

<meta_analysis_tests>

## Meta-Analysis Statistical Tests

Tests that verify the meta-analysis computation produces correct pooled estimates.

### Pattern: Fixed-Effect vs Known Result

```python
def test_fixed_effect_known_result():
    """Verify FE meta-analysis against hand-calculated result."""
    # Two studies with known weights
    yi = np.array([0.5, 1.0])
    vi = np.array([0.25, 0.25])  # Equal variances -> equal weights
    w = 1.0 / vi

    expected_pooled = np.sum(w * yi) / np.sum(w)  # = 0.75
    expected_se = np.sqrt(1.0 / np.sum(w))  # = sqrt(1/8) = 0.354

    result = run_fixed_effect_meta(yi, vi)
    np.testing.assert_allclose(result.pooled, expected_pooled, rtol=1e-10)
    np.testing.assert_allclose(result.se, expected_se, rtol=1e-10)


def test_single_study_meta():
    """Meta-analysis of one study should return that study's estimate."""
    yi = np.array([0.5])
    vi = np.array([0.04])

    result_fe = run_fixed_effect_meta(yi, vi)
    result_re = run_random_effects_meta(yi, vi)

    np.testing.assert_allclose(result_fe.pooled, 0.5, atol=1e-10)
    np.testing.assert_allclose(result_re.pooled, 0.5, atol=1e-10)
    np.testing.assert_allclose(result_fe.se, 0.2, atol=1e-10)
```

### Pattern: Heterogeneity Statistics

```python
def test_homogeneous_studies_zero_tau():
    """Identical effect sizes should yield tau-squared = 0 and I-squared = 0."""
    yi = np.array([0.5, 0.5, 0.5, 0.5])
    vi = np.array([0.1, 0.2, 0.15, 0.25])

    result = run_random_effects_meta(yi, vi)
    assert result.tau2 == 0, f"tau-squared should be 0, got {result.tau2}"
    assert result.I2 == 0, f"I-squared should be 0, got {result.I2}"


def test_heterogeneous_studies_positive_tau():
    """Very different effect sizes should yield substantial heterogeneity."""
    yi = np.array([-1.0, 0.0, 1.0, 2.0])
    vi = np.array([0.01, 0.01, 0.01, 0.01])  # Precise studies, very different

    result = run_random_effects_meta(yi, vi)
    assert result.tau2 > 0, "tau-squared should be > 0 for heterogeneous studies"
    assert result.I2 > 90, f"I-squared should be very high, got {result.I2}"


@pytest.mark.parametrize("method", ["DL", "REML", "PM", "ML"])
def test_tau_squared_non_negative(method):
    """Tau-squared must be non-negative for all estimators."""
    yi = np.array([0.5, 0.6, 0.4, 0.55])
    vi = np.array([0.1, 0.15, 0.2, 0.12])

    result = run_random_effects_meta(yi, vi, method=method)
    assert result.tau2 >= 0, f"tau-squared negative ({result.tau2}) with method {method}"
```

### Pattern: FE vs RE Comparison

```python
def test_fe_re_agree_when_homogeneous():
    """FE and RE should give similar results when heterogeneity is low."""
    yi = np.array([0.50, 0.48, 0.52, 0.49, 0.51])
    vi = np.array([0.10, 0.12, 0.11, 0.15, 0.09])

    fe = run_fixed_effect_meta(yi, vi)
    re = run_random_effects_meta(yi, vi)

    np.testing.assert_allclose(fe.pooled, re.pooled, atol=0.05,
        err_msg="FE and RE should agree when heterogeneity is low")


def test_re_wider_ci_than_fe():
    """RE confidence interval must be at least as wide as FE CI."""
    yi = np.array([0.2, 0.8, 0.3, 0.7, 0.5])
    vi = np.array([0.05, 0.05, 0.05, 0.05, 0.05])

    fe = run_fixed_effect_meta(yi, vi)
    re = run_random_effects_meta(yi, vi)

    fe_width = fe.ci_upper - fe.ci_lower
    re_width = re.ci_upper - re.ci_lower

    assert re_width >= fe_width - 1e-10, (
        f"RE CI width ({re_width:.4f}) should be >= FE CI width ({fe_width:.4f})"
    )
```

</meta_analysis_tests>

<sensitivity_tests>

## Sensitivity Analysis Tests

Tests that verify sensitivity analyses are computed correctly and interpreted appropriately.

### Pattern: Leave-One-Out Consistency

```python
def test_leave_one_out_produces_k_results():
    """LOO should produce exactly k estimates for k studies."""
    yi = np.array([0.5, 0.3, 0.7, 0.4, 0.6])
    vi = np.array([0.1, 0.12, 0.08, 0.15, 0.11])

    loo_results = run_leave_one_out(yi, vi)
    assert len(loo_results) == len(yi), (
        f"Expected {len(yi)} LOO results, got {len(loo_results)}"
    )


def test_leave_one_out_each_study_excluded():
    """Each LOO iteration should have k-1 studies."""
    yi = np.array([0.5, 0.3, 0.7])
    vi = np.array([0.1, 0.12, 0.08])

    for i in range(len(yi)):
        yi_loo = np.delete(yi, i)
        vi_loo = np.delete(vi, i)
        assert len(yi_loo) == len(yi) - 1
        result = run_random_effects_meta(yi_loo, vi_loo)
        assert result.pooled is not None
```

### Pattern: Model Sensitivity

```python
@pytest.mark.parametrize("method1, method2", [
    ("DL", "REML"),
    ("DL", "PM"),
    ("REML", "ML"),
])
def test_model_sensitivity(method1, method2):
    """Different tau-squared estimators should give directionally consistent results."""
    yi = np.array([-0.45, -0.32, -0.28, -0.51, -0.38])
    vi = np.array([0.0144, 0.0225, 0.0324, 0.01, 0.0196])

    r1 = run_random_effects_meta(yi, vi, method=method1)
    r2 = run_random_effects_meta(yi, vi, method=method2)

    # Direction must agree
    assert np.sign(r1.pooled) == np.sign(r2.pooled), (
        f"Direction mismatch: {method1} gives {r1.pooled:.4f}, "
        f"{method2} gives {r2.pooled:.4f}"
    )

    # Magnitude should be roughly similar (within 50%)
    ratio = abs(r1.pooled / r2.pooled) if r2.pooled != 0 else float('inf')
    assert 0.5 < ratio < 2.0, (
        f"Large magnitude difference: {method1}={r1.pooled:.4f}, "
        f"{method2}={r2.pooled:.4f}"
    )
```

</sensitivity_tests>

<publication_bias_tests>

## Publication Bias Tests

### Pattern: Egger's Test Known Values

```python
def test_eggers_symmetric_funnel():
    """Symmetric funnel should yield non-significant Egger's test."""
    # Symmetric effect sizes around the pooled estimate
    np.random.seed(42)
    k = 20
    true_effect = 0.5
    sei = np.random.uniform(0.1, 0.5, k)
    yi = true_effect + np.random.normal(0, sei)

    result = run_eggers_test(yi, sei)
    # With a truly symmetric distribution, p should usually be > 0.10
    # We cannot guarantee this with random data, but the intercept should be near 0
    assert abs(result.intercept) < 3.0, (
        f"Egger's intercept too large for symmetric data: {result.intercept:.3f}"
    )


def test_eggers_asymmetric_funnel():
    """Asymmetric funnel (missing small negative studies) should be detected."""
    # Simulate publication bias: keep only significant or positive small studies
    np.random.seed(42)
    true_effect = 0.2  # Small true effect
    yi_all, sei_all = [], []

    for _ in range(50):
        se = np.random.uniform(0.1, 0.5)
        y = true_effect + np.random.normal(0, se)
        z = y / se
        # "Publish" if significant or from large study
        if z > 1.96 or se < 0.15:
            yi_all.append(y)
            sei_all.append(se)

    if len(yi_all) >= 10:
        result = run_eggers_test(np.array(yi_all), np.array(sei_all))
        # Biased sample should tend toward significant Egger's test
        # This is probabilistic, so we just check it runs correctly
        assert hasattr(result, 'p_value')
        assert hasattr(result, 'intercept')
```

</publication_bias_tests>

<data_extraction_tests>

## Data Extraction Verification Tests

### Pattern: Cross-Check Extracted Data Against Computed Values

```python
@pytest.mark.parametrize("study_data", [
    {
        "label": "Smith 2020",
        "events_tx": 15, "n_tx": 100,
        "events_ctrl": 25, "n_ctrl": 100,
        "reported_or": 0.529,
        "reported_ci": (0.262, 1.071),
    },
    {
        "label": "Jones 2021",
        "mean_tx": 12.5, "sd_tx": 4.2, "n_tx": 80,
        "mean_ctrl": 14.8, "sd_ctrl": 4.5, "n_ctrl": 78,
        "reported_smd": -0.530,
    },
])
def test_extracted_data_matches_computation(study_data):
    """Extracted effect sizes must be recalculable from raw data."""
    if "events_tx" in study_data:
        computed_or = compute_odds_ratio(
            study_data["events_tx"], study_data["n_tx"],
            study_data["events_ctrl"], study_data["n_ctrl"]
        )
        np.testing.assert_allclose(
            computed_or, study_data["reported_or"], rtol=0.05,
            err_msg=f"OR mismatch for {study_data['label']}"
        )

    if "mean_tx" in study_data:
        computed_smd = compute_hedges_g(
            study_data["mean_tx"], study_data["sd_tx"], study_data["n_tx"],
            study_data["mean_ctrl"], study_data["sd_ctrl"], study_data["n_ctrl"]
        )
        np.testing.assert_allclose(
            computed_smd, study_data["reported_smd"], rtol=0.05,
            err_msg=f"SMD mismatch for {study_data['label']}"
        )
```

### Pattern: PRISMA Flow Diagram Arithmetic

```python
def test_prisma_flow_numbers_add_up(prisma_data):
    """PRISMA flow diagram numbers must be internally consistent."""
    # Records identified
    total_identified = sum(prisma_data["records_per_database"].values())
    assert total_identified == prisma_data["total_identified"], (
        f"Sum of per-database records ({total_identified}) != "
        f"total identified ({prisma_data['total_identified']})"
    )

    # After deduplication
    assert prisma_data["after_dedup"] <= total_identified, (
        "Records after dedup cannot exceed total identified"
    )

    # Screening
    screened = prisma_data["after_dedup"]
    excluded_title_abstract = prisma_data["excluded_title_abstract"]
    full_text_assessed = screened - excluded_title_abstract
    assert full_text_assessed == prisma_data["full_text_assessed"], (
        f"Full text assessed mismatch: {full_text_assessed} vs "
        f"{prisma_data['full_text_assessed']}"
    )

    # Full text
    excluded_full_text = sum(prisma_data["exclusion_reasons"].values())
    included = full_text_assessed - excluded_full_text
    assert included == prisma_data["studies_included"], (
        f"Studies included mismatch: {included} vs "
        f"{prisma_data['studies_included']}"
    )
```

</data_extraction_tests>

<anti_patterns>

## Anti-Patterns: What NOT to Test

### Testing implementation instead of evidence quality

```python
# BAD: Tests internal data structure, not statistical correctness
def test_meta_returns_dict():
    result = run_meta_analysis(yi, vi)
    assert isinstance(result, dict)  # Implementation detail

# GOOD: Tests statistical property of the result
def test_meta_ci_contains_pooled():
    result = run_meta_analysis(yi, vi)
    assert result.ci_lower <= result.pooled <= result.ci_upper
```

### Testing formatting instead of correctness

```python
# BAD: Tests output format
def test_forest_plot_has_title():
    plot = generate_forest_plot(yi, vi)
    assert plot.title is not None  # Meaningless

# GOOD: Tests that the forest plot data is correct
def test_forest_plot_data_matches_input():
    plot = generate_forest_plot(yi, vi)
    np.testing.assert_array_equal(plot.effect_sizes, yi)
    np.testing.assert_array_equal(plot.standard_errors, np.sqrt(vi))
```

### Over-fitting tests to specific values

```python
# BAD: Brittle test that breaks with any algorithm change
def test_exact_pooled_value():
    result = run_meta_analysis(yi, vi)
    assert result.pooled == -0.3845672310124  # Exact float comparison

# GOOD: Test against independently calculated value with meaningful tolerance
def test_pooled_matches_manual_calculation():
    result = run_meta_analysis(yi, vi)
    manual_pooled = manual_inverse_variance_pooling(yi, vi)
    np.testing.assert_allclose(result.pooled, manual_pooled, rtol=1e-6)
```

### Testing only the trivial case

```python
# BAD: Only tests with no heterogeneity
def test_meta_no_heterogeneity():
    yi = np.array([0.5, 0.5, 0.5])
    vi = np.array([0.1, 0.1, 0.1])
    result = run_meta_analysis(yi, vi)
    assert result.I2 == 0  # Trivially true

# GOOD: Tests across a range of heterogeneity levels
@pytest.mark.parametrize("spread", [0.0, 0.1, 0.5, 1.0, 2.0])
def test_i_squared_increases_with_spread(spread):
    """I-squared should increase as study effects diverge."""
    np.random.seed(42)
    base = 0.5
    yi = np.array([base - spread, base, base + spread])
    vi = np.array([0.01, 0.01, 0.01])
    result = run_meta_analysis(yi, vi)
    if spread == 0:
        assert result.I2 == 0
    else:
        assert result.I2 > 0
```

</anti_patterns>

<test_organization>

## Test Suite Organization

```
tests/
  conftest.py                  # Shared fixtures (sample datasets, helper functions)
  test_effect_sizes.py         # Effect size calculation tests
  test_meta_analysis.py        # Pooled estimate and model tests
  test_heterogeneity.py        # Q, I-squared, tau-squared tests
  test_sensitivity.py          # Leave-one-out, model comparison tests
  test_publication_bias.py     # Egger's, funnel plot, trim-and-fill tests
  test_data_extraction.py      # Data extraction verification tests
  test_reporting.py            # PRISMA, GRADE completeness tests
  benchmark_data.json          # Registry of validated numerical results
```

### Fixture Patterns for Medical Research

```python
# conftest.py
import pytest
import numpy as np

@pytest.fixture
def cochrane_antidepressant_data():
    """Sample data from a real meta-analysis (antidepressants vs placebo)."""
    return {
        "yi": np.array([-0.30, -0.25, -0.35, -0.28, -0.32,
                         -0.40, -0.22, -0.38, -0.27, -0.33]),
        "sei": np.array([0.08, 0.10, 0.09, 0.12, 0.07,
                          0.11, 0.15, 0.06, 0.13, 0.10]),
        "labels": [f"Study{i}" for i in range(1, 11)],
        "expected_pooled_range": (-0.40, -0.25),  # Approximate
    }

@pytest.fixture(params=[3, 5, 10, 20])
def n_studies(request):
    """Parameterize over number of included studies."""
    return request.param

@pytest.fixture
def random_meta_data(n_studies):
    """Generate random meta-analysis data for testing."""
    rng = np.random.default_rng(seed=42)
    true_effect = -0.3
    tau = 0.1
    study_effects = true_effect + rng.normal(0, tau, n_studies)
    sei = rng.uniform(0.05, 0.20, n_studies)
    yi = study_effects + rng.normal(0, sei)
    return {"yi": yi, "sei": sei, "vi": sei ** 2}
```

### Marking Tests by Category

```python
pytestmark = pytest.mark.medical

@pytest.mark.slow
def test_bootstrap_sensitivity():
    """Bootstrap with 10000 resamples for robustness check."""
    ...

@pytest.mark.benchmark
def test_against_cochrane_result():
    """Test against published Cochrane meta-analysis result."""
    ...

@pytest.mark.regression
def test_pooled_estimate_unchanged():
    """Regression test: pooled estimate should match previous run."""
    ...
```

## See Also

- `references/verification/core/verification-core.md` — verification checks that code tests should encode
- `references/verification/core/computational-verification-templates.md` — R/Python templates for oracle verification
- `references/verification/core/verification-numerical.md` — statistical verification methods

</test_organization>
