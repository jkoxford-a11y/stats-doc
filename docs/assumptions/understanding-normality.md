# Understanding Normality Assumptions

!!! info "The Conceptual Foundation"
    This page explains **why** normality matters and **what** you're actually checking. Master these concepts before diving into the mechanics.

## The Big Question: Why Do We Care About Normality?

Statistical tests like t-tests, ANOVA, and regression make mathematical assumptions to ensure their p-values and confidence intervals are accurate. One key assumption is normality — but **normality of what, exactly?** This is where most confusion begins.

---

## 🎯 Core Principle: It's About the Errors, Not the Data

!!! success "The Most Important Thing You'll Learn"
    **Normality is not a property of your data — it's a property of the errors in your statistical model.**

This single principle will guide everything you do. Let's break it down.

---

## What Are Residuals (Errors)?

### The Simple Definition

A **residual** is what's left over after your model has done its best:

```
Residual = What you observed - What your model predicted
```

Think of it like this: You're trying to predict someone's test score based on hours studied. Your model might predict 85%, but they actually scored 90%. That +5% difference? That's the **residual** — the part your model couldn't explain.

### A Concrete Example

Imagine measuring reaction times for two groups:

- **Coffee group:** 250ms, 260ms, 245ms, 255ms (mean = 252.5ms)
- **No coffee group:** 280ms, 290ms, 275ms, 285ms (mean = 282.5ms)

When you run an ANOVA:

1. The model predicts each coffee person will have a reaction time of **252.5ms**
2. The model predicts each no-coffee person will have a reaction time of **282.5ms**
3. The residuals are each person's deviation from their group's mean

So for the person who scored 260ms in the coffee group:

```
Residual = 260 - 252.5 = +7.5ms
```

---

## When Do Residuals Exist?

### Tests WITH Explicit Models (Check Residuals)

These tests fit a model to your data, creating predictions and therefore residuals:

- **ANOVA:** Predicts group means
- **Regression:** Predicts Y from X values
- **ANCOVA:** Predicts adjusted group means
- **Mixed models:** Predicts considering multiple factors

!!! warning "For these tests: Check RESIDUALS, not raw data!"

### Tests WITHOUT Explicit Models (Check Raw Data)

These tests work directly with your data without creating a predictive model:

- **One-sample t-test:** Compares data to a single value
- **Independent t-test:** Compares two groups (though you could think of this as a simple model)
- **Paired t-test:** Works with difference scores
- **Correlation:** Examines relationships without prediction

!!! note "For these tests: Check the DATA itself (or differences for paired tests)"

---

## Why This Distinction Matters

### Scenario 1: Your Raw Data is Skewed

Reaction times are famously right-skewed — most people respond quickly, but a few are very slow, creating a long tail. You might panic seeing this skewed distribution.

**BUT:** After accounting for group differences (coffee vs. no coffee), the **residuals** might be perfectly normal! The groups explain the skew, leaving normally distributed errors.

!!! success "Verdict"
    Your ANOVA is valid even with skewed raw data, as long as residuals are normal.

### Scenario 2: Perfect Normal Data, Non-Normal Residuals

Your depression scores might look beautifully normal overall. But if you're predicting depression from stress levels, and there's a non-linear relationship you didn't account for, your **residuals** might be severely non-normal.

!!! warning "Verdict"
    Your regression is problematic even with normal raw data, because residuals aren't normal.

---

## The Practical Impact

When residuals/errors are non-normal:

❌ **P-values become unreliable** — you might claim significance when there isn't any (or vice versa)  
❌ **Confidence intervals are wrong** — your 95% CI might actually capture the true value 87% or 99% of the time  
❌ **Predictions are suboptimal** — your model isn't capturing patterns it should  

---

## Common Misconceptions Clarified

### ❌ Misconception 1: "My dependent variable must be normal"

**Reality:** Only for simple tests without models. For ANOVA/regression, only residuals matter.

### ❌ Misconception 2: "All my variables need to be normal"

**Reality:** Predictor variables (X) don't need to be normal at all! Only the outcome variable's residuals matter.

### ❌ Misconception 3: "I should check normality before and after my analysis"

**Reality:** You can't check residuals before running the model—residuals don't exist yet! Run the analysis first, then check residuals.

### ❌ Misconception 4: "Slight non-normality ruins everything"

**Reality:** Statistical tests are **robust** to moderate violations, especially with larger samples (n > 30 per group). Severe violations are the real concern.

---

## When Does Normality Really Matter?

### ✓ You Can Relax If:

- **Large samples** (n > 30 per group) — Central Limit Theorem protects you
- **Symmetric distributions** — Even if not perfectly normal, symmetric is usually fine
- **Mild violations** — Slightly off from perfect normality is almost always okay

### ⚠️ Be Concerned If:

- **Small samples** (n < 30 per group) AND severe skewness
- **Extreme outliers** pulling residuals far from normal
- **Bimodal residuals** — suggests you're missing an important grouping variable
- **Systematic patterns** in residual plots (curved, funnel-shaped)

---

## The Bottom Line

!!! tip "Key Takeaways"
    1. Check **residuals** for model-based tests (ANOVA, regression)
    2. Check **raw data** (or differences) for simple tests (t-tests, correlation)
    3. Run your analysis **first**, then check residuals
    4. Normality violations matter more with **small samples**
    5. Use **visual** and **statistical** methods together
    6. When in doubt, try a **non-parametric alternative**

---

## Next Steps

**Ready to check normality?** → [Checking Normality Workflow](checking-normality.md)

**Need more practice?** → [Interactive Normality Modules](../modules/overview.md#1-checking-normality-series-5-modules)

**Assumptions failing?** → [When Assumptions Fail](when-assumptions-fail.md)

---

[← Back to Home](../index.md) | [Checking Normality →](checking-normality.md)
