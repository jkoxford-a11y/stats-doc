# Other Statistical Assumptions

Beyond normality, statistical tests have several other important assumptions to check.

## 🔍 Key Assumptions by Test

### Common to All Tests

#### 1. Independence of Observations

**What it means:** Each data point should be independent—one observation shouldn't influence another.

**Violations:**
- Measuring same person multiple times (without accounting for it)
- Siblings, classmates, or other clusters
- Time series data (consecutive measurements)
- Multiple measurements from same source

**How to check:**
- Study design review (not a statistical test!)
- Think about how data were collected
- Ask: "Could one person's score affect another's?"

**Solutions:**
- Use repeated measures designs
- Use mixed models/hierarchical models
- Account for clustering in analysis

---

### For t-tests and ANOVA

#### 2. Homogeneity of Variance (Homoscedasticity)

**What it means:** Groups should have similar spread (variance).

**Check with Levene's Test:**

```r
# Load required package
library(car)

# Test for equal variances
leveneTest(outcome ~ group, data = data)

# If p > .05: variances are equal (assumption met)
# If p < .05: variances differ (assumption violated)
```

**Visual check:**

```r
# Boxplot to compare spreads
boxplot(outcome ~ group, data = data,
        main = "Check for Equal Spread")

# Groups should have similar box heights
```

**Solutions if violated:**
- Use Welch's t-test (R's default for t.test)
- Use Welch's ANOVA: `oneway.test(outcome ~ group, data = data)`
- Transform data (log, sqrt)

!!! note "Good News for t-tests"
    R's `t.test()` defaults to **Welch's t-test**, which doesn't assume equal variances!

---

### For Regression

#### 3. Linearity

**What it means:** The relationship between X and Y should be linear (straight line).

**Check with scatterplot:**

```r
# Scatterplot
plot(data$predictor, data$outcome,
     xlab = "Predictor", ylab = "Outcome",
     pch = 19, col = "steelblue")

# Add line of best fit
abline(lm(outcome ~ predictor, data = data), 
       col = "red", lwd = 2)

# Look for: points scattered around line
# Red flags: clear curve, fan shape
```

**Check residual plot:**

```r
model <- lm(outcome ~ predictor, data = data)

# Residuals vs Fitted plot
plot(fitted(model), residuals(model),
     xlab = "Fitted Values", ylab = "Residuals",
     main = "Residuals vs Fitted")
abline(h = 0, col = "red", lty = 2)

# Should see: random scatter around zero
# Red flags: systematic curve, pattern
```

**Solutions if violated:**
- Transform variables (log, sqrt, polynomial)
- Add polynomial terms: `lm(y ~ x + I(x^2))`
- Use non-linear regression

---

#### 4. No Multicollinearity (Multiple Regression)

**What it means:** Predictor variables shouldn't be too highly correlated with each other.

**Check with VIF (Variance Inflation Factor):**

```r
library(car)

model <- lm(outcome ~ pred1 + pred2 + pred3, data = data)

# Calculate VIF
vif(model)

# Interpretation:
# VIF = 1: No correlation
# VIF < 5: Usually okay
# VIF 5-10: Moderate concern
# VIF > 10: Serious problem
```

**Check correlation matrix:**

```r
# Correlations between predictors
cor(data[, c("pred1", "pred2", "pred3")])

# Look for: correlations > .80 or .90
```

**Solutions if violated:**
- Remove one of the correlated predictors
- Combine correlated predictors (e.g., average them)
- Use principal component analysis
- Use ridge regression

---

#### 5. Homoscedasticity of Residuals

**What it means:** Residual variance should be constant across fitted values.

**Check with residual plot:**

```r
model <- lm(outcome ~ predictor, data = data)

plot(fitted(model), residuals(model),
     xlab = "Fitted Values", ylab = "Residuals",
     main = "Check for Equal Spread")
abline(h = 0, col = "red", lty = 2)

# Should see: constant spread across x-axis
# Red flag: "funnel shape" (spread increases)
```

**Formal test (Breusch-Pagan):**

```r
library(lmtest)
bptest(model)

# p > .05: equal variance (good)
# p < .05: heteroscedasticity (concern)
```

**Solutions if violated:**
- Transform outcome variable (log, sqrt)
- Use weighted least squares
- Use robust standard errors

---

#### 6. No Influential Outliers

**What it means:** No single point should overly influence the results.

**Check Cook's Distance:**

```r
model <- lm(outcome ~ predictor, data = data)

# Cook's distance plot
plot(model, which = 4)

# Rule of thumb: Cook's D > 1 is concerning
# Also check: Cook's D > 4/(n-k-1) where n = sample size, k = predictors
```

**Identify influential points:**

```r
# Points with high Cook's distance
cooks_d <- cooks.distance(model)
influential <- which(cooks_d > 4/(nrow(data) - 2))

# View influential points
data[influential, ]
```

**Solutions:**
- Investigate: Is it a data entry error?
- Consider removing (with justification!)
- Use robust regression
- Report results with and without outlier

---

### For Chi-Square Tests

#### 7. Expected Frequencies ≥ 5

**What it means:** Each cell should have expected count ≥ 5.

**Check expected frequencies:**

```r
result <- chisq.test(table(data$var1, data$var2))

# View expected counts
result$expected

# All values should be ≥ 5
```

**Solutions if violated:**
- Combine categories (if logical)
- Use Fisher's Exact Test (2×2 tables only)
- Collect more data

---

## 📋 Quick Assumption Checklist

### Before Running t-test or ANOVA

```r
# ☐ Independence (design check)
# ☐ Normality
shapiro.test(residuals(model))  # or by group for t-test

# ☐ Equal variances (for standard t-test/ANOVA)
library(car)
leveneTest(outcome ~ group, data = data)
```

### Before Running Regression

```r
# ☐ Independence (design check)
# ☐ Linearity
plot(data$x, data$y)

# ☐ Normality of residuals
shapiro.test(residuals(model))

# ☐ Homoscedasticity
plot(fitted(model), residuals(model))

# ☐ No multicollinearity (if multiple predictors)
library(car)
vif(model)

# ☐ No influential outliers
plot(model, which = 4)
```

### Before Running Chi-Square

```r
# ☐ Independence (design check)
# ☐ Expected frequencies ≥ 5
result <- chisq.test(table)
result$expected
```

---

## ⚠️ What If Assumptions Are Violated?

See: [When Assumptions Fail](when-assumptions-fail.md)

---

[← Checking Normality](checking-normality.md) | [Home](../index.md) | [When Assumptions Fail →](when-assumptions-fail.md)
