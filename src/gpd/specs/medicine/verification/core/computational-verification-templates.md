---
load_when:
  - "verification"
  - "computational check"
  - "forest plot"
  - "funnel plot"
  - "meta-analysis code"
  - "oracle"
tier: 2
context_cost: medium
---

# Computational Verification Templates

Copy-paste-ready R and Python templates for the most common medical research verification tasks. These provide the **external oracle** that breaks LLM self-consistency loops — every VERIFICATION.md must include at least one executed code block from this catalog.

**Usage:** Copy the relevant template, replace the placeholder comments with actual data from the evidence synthesis, execute via shell, and paste both the code AND output into VERIFICATION.md.

---

## Template 1: Forest Plot Generation and Verification (R)

Generate a forest plot and verify the pooled estimate using the `metafor` package.

```r
#!/usr/bin/env Rscript
# Forest plot generation and verification template
# Requires: install.packages("metafor")
library(metafor)

# === REPLACE BELOW WITH ACTUAL DATA ===
# Study data: effect sizes (yi) and standard errors (sei)
# For continuous outcomes (MD or SMD):
study_labels <- c("Study1 2020", "Study2 2021", "Study3 2022",
                  "Study4 2023", "Study5 2024")
yi <- c(-0.45, -0.32, -0.28, -0.51, -0.38)  # Effect sizes
sei <- c(0.12, 0.15, 0.18, 0.10, 0.14)       # Standard errors

# === FIT RANDOM-EFFECTS MODEL ===
res <- rma(yi = yi, sei = sei, method = "REML")

# Print summary
cat("=== Meta-Analysis Results ===\n")
cat(sprintf("Pooled estimate: %.4f [95%% CI: %.4f, %.4f]\n",
            res$beta, res$ci.lb, res$ci.ub))
cat(sprintf("z = %.3f, p = %.6f\n", res$zval, res$pval))
cat(sprintf("tau-squared = %.4f, tau = %.4f\n", res$tau2, sqrt(res$tau2)))
cat(sprintf("I-squared = %.1f%%\n", res$I2))
cat(sprintf("H-squared = %.2f\n", res$H2))
cat(sprintf("Q(%d) = %.2f, p = %.4f\n", res$k - 1, res$QE, res$QEp))

# Prediction interval
pi <- predict(res)
cat(sprintf("95%% Prediction interval: [%.4f, %.4f]\n", pi$pi.lb, pi$pi.ub))

# === GENERATE FOREST PLOT ===
pdf("forest_plot.pdf", width = 10, height = 6)
forest(res, slab = study_labels,
       xlab = "Effect Size (SMD)",
       header = "Study",
       mlab = "Random-Effects Model (REML)")
dev.off()
cat("\nForest plot saved to forest_plot.pdf\n")

# === VERIFY WITH HKSJ ADJUSTMENT ===
res_hksj <- rma(yi = yi, sei = sei, method = "REML", test = "knha")
cat(sprintf("\nHKSJ-adjusted: %.4f [%.4f, %.4f], p = %.6f\n",
            res_hksj$beta, res_hksj$ci.lb, res_hksj$ci.ub, res_hksj$pval))

# === COMPARE WITH DL ESTIMATOR ===
res_dl <- rma(yi = yi, sei = sei, method = "DL")
cat(sprintf("DL estimator: %.4f [%.4f, %.4f]\n",
            res_dl$beta, res_dl$ci.lb, res_dl$ci.ub))
```

---

## Template 2: Forest Plot Generation and Verification (Python)

```python
#!/usr/bin/env python3
"""Forest plot generation and meta-analysis verification using Python."""
import numpy as np

# === REPLACE WITH ACTUAL DATA ===
studies = [
    {"label": "Study1 2020", "yi": -0.45, "sei": 0.12},
    {"label": "Study2 2021", "yi": -0.32, "sei": 0.15},
    {"label": "Study3 2022", "yi": -0.28, "sei": 0.18},
    {"label": "Study4 2023", "yi": -0.51, "sei": 0.10},
    {"label": "Study5 2024", "yi": -0.38, "sei": 0.14},
]

yi = np.array([s["yi"] for s in studies])
sei = np.array([s["sei"] for s in studies])
vi = sei ** 2
k = len(yi)

# === FIXED-EFFECT MODEL ===
w_fe = 1.0 / vi
mu_fe = np.sum(w_fe * yi) / np.sum(w_fe)
se_fe = np.sqrt(1.0 / np.sum(w_fe))

print("=== Fixed-Effect Model ===")
print(f"Pooled: {mu_fe:.4f} [{mu_fe - 1.96*se_fe:.4f}, {mu_fe + 1.96*se_fe:.4f}]")

# === RANDOM-EFFECTS (DerSimonian-Laird) ===
Q = np.sum(w_fe * (yi - mu_fe) ** 2)
df = k - 1
C = np.sum(w_fe) - np.sum(w_fe ** 2) / np.sum(w_fe)
tau2 = max(0, (Q - df) / C)

w_re = 1.0 / (vi + tau2)
mu_re = np.sum(w_re * yi) / np.sum(w_re)
se_re = np.sqrt(1.0 / np.sum(w_re))

I2 = max(0, (Q - df) / Q * 100) if Q > df else 0

from scipy.stats import chi2, t as t_dist
p_Q = 1 - chi2.cdf(Q, df)

# Prediction interval
t_crit = t_dist.ppf(0.975, df=max(k - 2, 1))
pi_lower = mu_re - t_crit * np.sqrt(tau2 + se_re ** 2)
pi_upper = mu_re + t_crit * np.sqrt(tau2 + se_re ** 2)

print("\n=== Random-Effects Model (DL) ===")
print(f"Pooled: {mu_re:.4f} [{mu_re - 1.96*se_re:.4f}, {mu_re + 1.96*se_re:.4f}]")
print(f"tau-squared = {tau2:.4f}, tau = {np.sqrt(tau2):.4f}")
print(f"I-squared = {I2:.1f}%")
print(f"Q({df}) = {Q:.2f}, p = {p_Q:.4f}")
print(f"Prediction interval: [{pi_lower:.4f}, {pi_upper:.4f}]")

# === FOREST PLOT ===
try:
    import matplotlib.pyplot as plt

    fig, ax = plt.subplots(figsize=(10, 6))
    y_positions = range(k, 0, -1)

    for i, (y_pos, study) in enumerate(zip(y_positions, studies)):
        ci_lo = yi[i] - 1.96 * sei[i]
        ci_hi = yi[i] + 1.96 * sei[i]
        ax.plot([ci_lo, ci_hi], [y_pos, y_pos], 'b-', linewidth=1)
        weight = w_re[i] / np.sum(w_re)
        ax.plot(yi[i], y_pos, 'bs', markersize=5 + weight * 30)
        ax.text(-1.2, y_pos, study["label"], ha='right', va='center', fontsize=9)
        ax.text(0.8, y_pos, f"{yi[i]:.2f} [{ci_lo:.2f}, {ci_hi:.2f}]",
                ha='left', va='center', fontsize=8)

    # Diamond for pooled estimate
    diamond_x = [mu_re - 1.96*se_re, mu_re, mu_re + 1.96*se_re, mu_re]
    diamond_y = [0, -0.3, 0, 0.3]
    ax.fill(diamond_x, diamond_y, 'red', alpha=0.5)
    ax.axvline(x=0, color='black', linestyle='--', linewidth=0.5)
    ax.set_xlabel('Effect Size (SMD)')
    ax.set_title('Forest Plot')
    ax.set_yticks([])
    plt.tight_layout()
    plt.savefig('forest_plot.png', dpi=150)
    print("\nForest plot saved to forest_plot.png")
except ImportError:
    print("\nmatplotlib not available; skip forest plot generation")
```

---

## Template 3: Funnel Plot and Egger's Test (R)

```r
#!/usr/bin/env Rscript
# Funnel plot and publication bias assessment
library(metafor)

# === REPLACE WITH ACTUAL DATA ===
yi <- c(-0.45, -0.32, -0.28, -0.51, -0.38, -0.22, -0.41,
        -0.55, -0.30, -0.35, -0.48, -0.26)
sei <- c(0.12, 0.15, 0.18, 0.10, 0.14, 0.20, 0.11,
         0.09, 0.16, 0.13, 0.12, 0.19)

res <- rma(yi = yi, sei = sei, method = "REML")

# === FUNNEL PLOT ===
pdf("funnel_plot.pdf", width = 8, height = 6)
funnel(res, xlab = "Effect Size", ylab = "Standard Error",
       main = "Funnel Plot")
dev.off()
cat("Funnel plot saved to funnel_plot.pdf\n")

# === EGGER'S TEST ===
egger <- regtest(res, model = "lm")
cat(sprintf("\n=== Egger's Test ===\n"))
cat(sprintf("Intercept: %.3f (SE: %.3f)\n", egger$est, egger$se))
cat(sprintf("z = %.3f, p = %.4f\n", egger$zval, egger$pval))
if (egger$pval < 0.10) {
  cat("WARNING: Significant small-study effect (p < 0.10)\n")
} else {
  cat("PASS: No significant small-study effect\n")
}

# === PETERS' TEST (for binary outcomes) ===
# Uncomment if using log(OR) with sample sizes:
# peters <- regtest(res, model = "lm", predictor = "ni")

# === TRIM AND FILL ===
tf <- trimfill(res)
cat(sprintf("\n=== Trim and Fill ===\n"))
cat(sprintf("Studies imputed: %d\n", tf$k0))
cat(sprintf("Original pooled: %.4f [%.4f, %.4f]\n",
            res$beta, res$ci.lb, res$ci.ub))
cat(sprintf("Adjusted pooled: %.4f [%.4f, %.4f]\n",
            tf$beta, tf$ci.lb, tf$ci.ub))

# Adjusted funnel plot
pdf("funnel_plot_trimfill.pdf", width = 8, height = 6)
funnel(tf, xlab = "Effect Size", ylab = "Standard Error",
       main = "Funnel Plot (Trim and Fill)")
dev.off()
cat("Trim-and-fill funnel plot saved to funnel_plot_trimfill.pdf\n")
```

---

## Template 4: Funnel Plot and Egger's Test (Python)

```python
#!/usr/bin/env python3
"""Funnel plot and Egger's test in Python."""
import numpy as np
from scipy.stats import linregress

# === REPLACE WITH ACTUAL DATA ===
yi = np.array([-0.45, -0.32, -0.28, -0.51, -0.38, -0.22,
               -0.41, -0.55, -0.30, -0.35, -0.48, -0.26])
sei = np.array([0.12, 0.15, 0.18, 0.10, 0.14, 0.20,
                0.11, 0.09, 0.16, 0.13, 0.12, 0.19])

# === EGGER'S TEST ===
precision = 1.0 / sei
z_scores = yi / sei
slope, intercept, r, p_value, se_slope = linregress(precision, z_scores)

print("=== Egger's Test ===")
print(f"Intercept: {intercept:.3f}")
print(f"p-value: {p_value:.4f}")
print(f"{'WARN: Small-study effect detected' if p_value < 0.10 else 'PASS: No small-study effect'}")

# === FUNNEL PLOT ===
try:
    import matplotlib.pyplot as plt

    fig, ax = plt.subplots(figsize=(8, 6))

    # Pooled estimate for reference line
    vi = sei ** 2
    w = 1.0 / vi
    mu = np.sum(w * yi) / np.sum(w)

    ax.scatter(yi, sei, s=50, c='blue', alpha=0.7)
    ax.axvline(x=mu, color='red', linestyle='--', label=f'Pooled = {mu:.3f}')
    ax.invert_yaxis()
    ax.set_xlabel('Effect Size')
    ax.set_ylabel('Standard Error')
    ax.set_title('Funnel Plot')

    # Pseudo-confidence region
    se_range = np.linspace(0.001, max(sei) * 1.2, 100)
    ax.plot(mu - 1.96 * se_range, se_range, 'k--', linewidth=0.5)
    ax.plot(mu + 1.96 * se_range, se_range, 'k--', linewidth=0.5)
    ax.legend()

    plt.tight_layout()
    plt.savefig('funnel_plot.png', dpi=150)
    print("\nFunnel plot saved to funnel_plot.png")
except ImportError:
    print("\nmatplotlib not available; skip funnel plot")
```

---

## Template 5: Heterogeneity Statistics (R)

```r
#!/usr/bin/env Rscript
# Comprehensive heterogeneity assessment
library(metafor)

# === REPLACE WITH ACTUAL DATA ===
yi <- c(-0.45, -0.32, -0.28, -0.51, -0.38)
sei <- c(0.12, 0.15, 0.18, 0.10, 0.14)

# === MULTIPLE TAU-SQUARED ESTIMATORS ===
methods <- c("DL", "REML", "PM", "ML", "HS", "SJ", "HE", "EB")
cat("=== Tau-squared Estimators Comparison ===\n")
cat(sprintf("%-6s  %10s  %10s  %10s\n", "Method", "tau2", "pooled", "I2"))

for (method in methods) {
  tryCatch({
    res <- rma(yi = yi, sei = sei, method = method)
    cat(sprintf("%-6s  %10.4f  %10.4f  %10.1f%%\n",
                method, res$tau2, res$beta, res$I2))
  }, error = function(e) {
    cat(sprintf("%-6s  %10s\n", method, "FAILED"))
  })
}

# === PREFERRED MODEL (REML + HKSJ) ===
res <- rma(yi = yi, sei = sei, method = "REML", test = "knha")
cat(sprintf("\n=== Preferred: REML + HKSJ ===\n"))
cat(sprintf("Pooled: %.4f [%.4f, %.4f]\n", res$beta, res$ci.lb, res$ci.ub))
cat(sprintf("t = %.3f, p = %.6f\n", res$zval, res$pval))

# Prediction interval
pi <- predict(res)
cat(sprintf("Prediction interval: [%.4f, %.4f]\n", pi$pi.lb, pi$pi.ub))

# === BAUJAT PLOT (influential studies) ===
pdf("baujat_plot.pdf", width = 8, height = 6)
baujat(res)
dev.off()
cat("\nBaujat plot saved to baujat_plot.pdf\n")
```

---

## Template 6: Leave-One-Out Sensitivity Analysis (R)

```r
#!/usr/bin/env Rscript
# Leave-one-out sensitivity analysis
library(metafor)

# === REPLACE WITH ACTUAL DATA ===
study_labels <- c("Study1 2020", "Study2 2021", "Study3 2022",
                  "Study4 2023", "Study5 2024")
yi <- c(-0.45, -0.32, -0.28, -0.51, -0.38)
sei <- c(0.12, 0.15, 0.18, 0.10, 0.14)

res <- rma(yi = yi, sei = sei, method = "REML", slab = study_labels)

# === LEAVE-ONE-OUT ===
loo <- leave1out(res)
cat("=== Leave-One-Out Sensitivity Analysis ===\n")
cat(sprintf("%-20s  %8s  %8s  %8s  %6s\n",
            "Omitted", "Pooled", "CI.lb", "CI.ub", "I2"))

for (i in 1:length(yi)) {
  cat(sprintf("%-20s  %8.4f  %8.4f  %8.4f  %5.1f%%\n",
              study_labels[i], loo$estimate[i],
              loo$ci.lb[i], loo$ci.ub[i], loo$I2[i]))
}

cat(sprintf("\n%-20s  %8.4f  %8.4f  %8.4f  %5.1f%%\n",
            "ALL STUDIES", res$beta, res$ci.lb, res$ci.ub, res$I2))

# === CHECK ROBUSTNESS ===
main_sig <- (res$ci.lb > 0) | (res$ci.ub < 0)
any_reversal <- any((loo$ci.lb > 0) != (res$ci.lb > 0) |
                     (loo$ci.ub < 0) != (res$ci.ub < 0))

if (any_reversal) {
  cat("\nWARN: Removing at least one study changes statistical significance.\n")
} else {
  cat("\nPASS: Result robust to leave-one-out.\n")
}

# === FOREST PLOT OF LEAVE-ONE-OUT ===
pdf("leave_one_out.pdf", width = 10, height = 6)
plot(loo, transf = exp)  # Remove transf for non-ratio measures
dev.off()
cat("Leave-one-out plot saved to leave_one_out.pdf\n")
```

---

## Template 7: Leave-One-Out Sensitivity Analysis (Python)

```python
#!/usr/bin/env python3
"""Leave-one-out sensitivity analysis in Python."""
import numpy as np

# === REPLACE WITH ACTUAL DATA ===
studies = ["Study1 2020", "Study2 2021", "Study3 2022",
           "Study4 2023", "Study5 2024"]
yi = np.array([-0.45, -0.32, -0.28, -0.51, -0.38])
sei = np.array([0.12, 0.15, 0.18, 0.10, 0.14])
vi = sei ** 2

def dl_meta(y, v):
    """DerSimonian-Laird random-effects meta-analysis."""
    w = 1.0 / v
    mu_fe = np.sum(w * y) / np.sum(w)
    Q = np.sum(w * (y - mu_fe) ** 2)
    df = len(y) - 1
    C = np.sum(w) - np.sum(w ** 2) / np.sum(w)
    tau2 = max(0, (Q - df) / C)
    w_re = 1.0 / (v + tau2)
    mu = np.sum(w_re * y) / np.sum(w_re)
    se = np.sqrt(1.0 / np.sum(w_re))
    I2 = max(0, (Q - df) / Q * 100) if Q > df else 0
    return mu, se, mu - 1.96 * se, mu + 1.96 * se, tau2, I2

# Full model
mu_all, se_all, ci_lo_all, ci_hi_all, tau2_all, I2_all = dl_meta(yi, vi)

print("=== Leave-One-Out Sensitivity Analysis ===")
print(f"{'Omitted':<20} {'Pooled':>8} {'CI.lb':>8} {'CI.ub':>8} {'I2':>6}")

any_reversal = False
main_sig = (ci_lo_all > 0) or (ci_hi_all < 0)

for i in range(len(yi)):
    y_loo = np.delete(yi, i)
    v_loo = np.delete(vi, i)
    mu, se, ci_lo, ci_hi, tau2, I2 = dl_meta(y_loo, v_loo)
    loo_sig = (ci_lo > 0) or (ci_hi < 0)
    if loo_sig != main_sig:
        any_reversal = True
    print(f"{studies[i]:<20} {mu:8.4f} {ci_lo:8.4f} {ci_hi:8.4f} {I2:5.1f}%")

print(f"\n{'ALL STUDIES':<20} {mu_all:8.4f} {ci_lo_all:8.4f} {ci_hi_all:8.4f} {I2_all:5.1f}%")

if any_reversal:
    print("\nWARN: Removing at least one study changes statistical significance.")
else:
    print("\nPASS: Result robust to leave-one-out.")
```

---

## Template 8: Power Analysis / Optimal Information Size (R)

```r
#!/usr/bin/env Rscript
# Power analysis and Optimal Information Size
# For continuous outcomes
cat("=== OIS for Continuous Outcome ===\n")

smd_target <- 0.3  # Minimum clinically important difference (SMD)
alpha <- 0.05
power <- 0.80

z_alpha <- qnorm(1 - alpha / 2)
z_beta <- qnorm(power)
n_per_arm <- ceiling(((z_alpha + z_beta) / smd_target)^2)
ois <- 2 * n_per_arm

cat(sprintf("Target SMD: %.2f\n", smd_target))
cat(sprintf("Alpha: %.2f, Power: %.2f\n", alpha, power))
cat(sprintf("OIS: %d per arm, %d total\n", n_per_arm, ois))

# For binary outcomes
cat("\n=== OIS for Binary Outcome ===\n")

p_ctrl <- 0.20   # Control event rate
rr_target <- 0.75 # Target relative risk reduction
p_tx <- p_ctrl * rr_target

n_per_arm_binary <- ceiling(
  ((z_alpha * sqrt(2 * p_ctrl * (1 - p_ctrl)) +
    z_beta * sqrt(p_ctrl * (1 - p_ctrl) + p_tx * (1 - p_tx))) /
   (p_ctrl - p_tx))^2
)
ois_binary <- 2 * n_per_arm_binary

cat(sprintf("Control rate: %.2f, Treatment rate: %.3f\n", p_ctrl, p_tx))
cat(sprintf("Target RR: %.2f\n", rr_target))
cat(sprintf("OIS: %d per arm, %d total\n", n_per_arm_binary, ois_binary))

# Compare with actual meta-analysis total N
total_n_actual <- 850  # REPLACE with actual total
cat(sprintf("\nActual meta-analysis N: %d\n", total_n_actual))
cat(sprintf("OIS met: %s\n", ifelse(total_n_actual >= ois, "YES", "NO")))
```

---

## Template 9: Power Analysis (Python)

```python
#!/usr/bin/env python3
"""Power analysis and Optimal Information Size calculation."""
from scipy.stats import norm
import numpy as np

def ois_continuous(smd, alpha=0.05, power=0.80):
    z_a = norm.ppf(1 - alpha / 2)
    z_b = norm.ppf(power)
    n_per_arm = int(np.ceil(((z_a + z_b) / smd) ** 2))
    return 2 * n_per_arm

def ois_binary(p_ctrl, rr, alpha=0.05, power=0.80):
    p_tx = p_ctrl * rr
    z_a = norm.ppf(1 - alpha / 2)
    z_b = norm.ppf(power)
    n_per_arm = int(np.ceil(
        ((z_a * np.sqrt(2 * p_ctrl * (1 - p_ctrl)) +
          z_b * np.sqrt(p_ctrl * (1 - p_ctrl) + p_tx * (1 - p_tx))) /
         (p_ctrl - p_tx)) ** 2
    ))
    return 2 * n_per_arm

# === REPLACE WITH ACTUAL VALUES ===
print("=== Continuous Outcome ===")
ois_c = ois_continuous(smd=0.3)
print(f"OIS for SMD=0.3: {ois_c} total participants")

print("\n=== Binary Outcome ===")
ois_b = ois_binary(p_ctrl=0.20, rr=0.75)
print(f"OIS for CER=20%, RR=0.75: {ois_b} total participants")

# Compare with actual
total_n = 850  # REPLACE
print(f"\nActual N = {total_n}")
print(f"OIS (continuous) met: {'YES' if total_n >= ois_c else 'NO'}")
print(f"OIS (binary) met: {'YES' if total_n >= ois_b else 'NO'}")
```

---

## Template 10: GRADE Imprecision Assessment (Python)

```python
#!/usr/bin/env python3
"""GRADE imprecision domain assessment."""
import numpy as np
from scipy.stats import norm

# === REPLACE WITH ACTUAL VALUES ===
pooled_estimate = -0.35       # Pooled SMD (or log(RR), etc.)
ci_lower = -0.52
ci_upper = -0.18
total_n = 850                 # Total participants in meta-analysis
mid = 0.2                     # Minimal important difference (on same scale)
                              # For SMD: often 0.2 (small) or 0.5 (moderate)
                              # For log(RR): often log(0.75) = -0.29

# === OIS CHECK ===
smd_target = mid
z_a = norm.ppf(0.975)
z_b = norm.ppf(0.80)
ois = 2 * int(np.ceil(((z_a + z_b) / smd_target) ** 2))

# === IMPRECISION ASSESSMENT ===
print("=== GRADE Imprecision Assessment ===")
print(f"Pooled estimate: {pooled_estimate:.3f} [{ci_lower:.3f}, {ci_upper:.3f}]")
print(f"MID: {mid:.3f}")
print(f"Total N: {total_n}, OIS: {ois}")

downgrade_reasons = []

# Check 1: OIS
if total_n < ois:
    downgrade_reasons.append(f"Total N ({total_n}) < OIS ({ois})")

# Check 2: CI crosses null
ci_crosses_null = ci_lower < 0 < ci_upper
if ci_crosses_null:
    downgrade_reasons.append("CI crosses null (no effect line)")

# Check 3: CI crosses MID threshold
# For benefit direction (negative = better):
ci_crosses_mid = ci_upper > -mid  # Upper CI crosses above -MID
if ci_crosses_mid and not ci_crosses_null:
    downgrade_reasons.append(f"CI upper bound ({ci_upper:.3f}) crosses MID threshold ({-mid:.3f})")

# Check 4: Wide CI relative to estimate
ci_width = ci_upper - ci_lower
if ci_width > 2 * abs(pooled_estimate):
    downgrade_reasons.append(f"CI width ({ci_width:.3f}) > 2x estimate magnitude ({2*abs(pooled_estimate):.3f})")

if len(downgrade_reasons) == 0:
    print("\nPASS: No imprecision concerns")
    print("GRADE imprecision: Do not downgrade")
elif len(downgrade_reasons) == 1:
    print(f"\nWARN: {downgrade_reasons[0]}")
    print("GRADE imprecision: Consider downgrading one level")
else:
    print(f"\nFAIL: Multiple imprecision concerns:")
    for r in downgrade_reasons:
        print(f"  - {r}")
    print("GRADE imprecision: Downgrade one or two levels")
```

---

## Usage in VERIFICATION.md

Every VERIFICATION.md must include at least ONE executed code block. The format:

````markdown
### Computational Oracle: [Check Name]

**Template:** [which template above]
**Test:** [what is being verified]

```r
# [Actual code with data from this review — NOT placeholders]
library(metafor)
yi <- c(-0.45, -0.32, -0.28, -0.51, -0.38)
sei <- c(0.12, 0.15, 0.18, 0.10, 0.14)
res <- rma(yi = yi, sei = sei, method = "REML")
print(res)
```

**Output:**
```
[Actual execution output pasted here]
```

**Verdict:** PASS / FAIL / INCONCLUSIVE
````

The presence of at least one such block (with both code AND output) is a **hard requirement** for VERIFICATION.md completeness. A VERIFICATION.md without computational output blocks is flagged as INCOMPLETE regardless of other content.
