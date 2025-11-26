# Effect Size Quick Reference

!!! info "What Are Effect Sizes?"
    Effect sizes measure the **magnitude** of a difference or relationship. Unlike p-values, effect sizes are not influenced by sample size and provide **practical significance**.

---

## Interpretation Guidelines

### Cohen's d (for t-tests)

**Measures:** Standardized difference between two means

| d value | Interpretation | Example |
|---------|----------------|---------|
| 0.0 - 0.2 | Negligible | Barely noticeable difference |
| 0.2 - 0.5 | Small | Noticeable but modest effect |
| 0.5 - 0.8 | Medium | Moderate, visible difference |
| 0.8+ | Large | Substantial, important difference |

**In R:**
```r
library(effsize)
cohen.d(outcome ~ group, data = data)
```

---

### η² (Eta Squared - for ANOVA)

**Measures:** Proportion of variance in outcome explained by the factor(s)

| η² value | Interpretation | Variance Explained |
|----------|----------------|--------------------|
| 0.01 - 0.06 | Small | 1-6% of variance |
| 0.06 - 0.14 | Medium | 6-14% of variance |
| 0.14+ | Large | 14%+ of variance |

**In R:**
```r
library(effectsize)
eta_squared(model)
```

---

### R² (for Regression and Correlation)

**Measures:** Proportion of variance in outcome explained by predictor(s)

| R² value | Interpretation | Practical Meaning |
|----------|----------------|-------------------|
| 0.01 - 0.09 | Small | Weak predictive power |
| 0.09 - 0.25 | Medium | Moderate predictive power |
| 0.25+ | Large | Strong predictive power |

**In R:**
```r
summary(model)$r.squared
```

---

### Cramér's V (for Chi-Square)

**Measures:** Strength of association between categorical variables

| V value | df = 1 | df = 2 | df = 3+ |
|---------|--------|--------|---------|
| Small | 0.10 | 0.07 | 0.06 |
| Medium | 0.30 | 0.21 | 0.17 |
| Large | 0.50 | 0.35 | 0.29 |

**Note:** df = min(rows-1, cols-1)

**In R:**
```r
library(lsr)
cramersV(table(data$var1, data$var2))
```

---

### r (Correlation Coefficient)

**Measures:** Strength and direction of linear relationship

| r value | Interpretation | Relationship Strength |
|---------|----------------|-----------------------|
| 0.0 - 0.1 | Negligible | No meaningful relationship |
| 0.1 - 0.3 | Small | Weak relationship |
| 0.3 - 0.5 | Medium | Moderate relationship |
| 0.5 - 0.7 | Large | Strong relationship |
| 0.7+ | Very Large | Very strong relationship |

**In R:**
```r
cor.test(data$var1, data$var2)
```

---

## Important Notes

!!! warning "Statistical vs. Practical Significance"
    A result can be **statistically significant** (p < .05) but have a **small effect size**. Always report both!

!!! tip "Context Matters"
    Effect size interpretation depends on your field. A "small" effect in medicine might be life-saving.

!!! success "Sample Size Independence"
    Unlike p-values, effect sizes are not inflated by large sample sizes.

!!! note "APA Requirements"
    APA style requires reporting effect sizes for all inferential tests.

!!! info "Direction Matters"
    For d and r, negative values indicate direction—magnitude is what matters for interpretation.

---

## Quick Conversions

- **r² = R²** (same thing)
- **d ≈ 2r** (for similar sample sizes)
- **η² ≈ R²** (for ANOVA vs. regression on same data)

---

## When to Use Each

| Test | Effect Size | R Package |
|------|-------------|-----------|
| One-Sample t-test | Cohen's d | `effsize` |
| Independent t-test | Cohen's d | `effsize` |
| Paired t-test | Cohen's d | `effsize` |
| One-Way ANOVA | η² | `effectsize` |
| Two-Way ANOVA | η² | `effectsize` |
| Regression | R² | Built-in |
| Correlation | r | Built-in |
| Chi-Square | Cramér's V | `lsr` |

---

[← Back to Home](../index.md) | [Decision Tree →](../which-test/decision-tree.md)
