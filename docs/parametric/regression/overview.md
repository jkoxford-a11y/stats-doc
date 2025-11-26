# REGRESSION: Definitive Workflow
**Last Updated: November 2024 | Use this version for all regression analyses**

## Quick Reference
**Purpose:** Predict continuous outcomes from one or more predictors  
**When to use:** Continuous outcome + continuous predictor(s)  
**Alternative if assumptions fail:** Non-parametric alternatives or robust regression

---

## Decision Tree
```
What are you trying to do?
├─ Predict Y from ONE predictor → Simple Linear Regression
├─ Predict Y from MULTIPLE predictors → Multiple Regression
├─ Examine relationship strength → Correlation (related concept)
└─ Categorical predictors? → Use ANOVA instead (or dummy coding)
```

---

## PART 1: SIMPLE LINEAR REGRESSION

### Question
Can I predict one variable from another?

### Example
Can I predict exam scores from hours studied?

### The Logic
Find the line of best fit: Y = b₀ + b₁X
- b₀ = intercept (Y when X = 0)
- b₁ = slope (change in Y per 1-unit change in X)

---

### R Code - Simple Regression

```r
# ALWAYS visualize first!
plot(data$predictor, data$outcome,
     xlab = "Hours Studied", ylab = "Exam Score",
     pch = 19, col = "steelblue")

# Fit the model
model <- lm(outcome ~ predictor, data = data)

# Add regression line to plot
abline(model, col = "red", lwd = 2)

# Get results
summary(model)

# Check assumptions (CRITICAL!)
par(mfrow = c(2,2))
plot(model)
par(mfrow = c(1,1))

# Check normality of residuals
shapiro.test(residuals(model))

# Get confidence intervals for coefficients
confint(model)

# Descriptive statistics
mean(data$predictor, na.rm = TRUE)
sd(data$predictor, na.rm = TRUE)
mean(data$outcome, na.rm = TRUE)
sd(data$outcome, na.rm = TRUE)

# Correlation (related)
cor.test(data$predictor, data$outcome)
```

---

### Interpreting Simple Regression Output

**F-test (first thing to check):**
- Tests: Does the model explain significant variance?
- If p < .05 → model is significant, proceed to interpret slope
- If p ≥ .05 → predictor doesn't help predict outcome

**Slope (Estimate for predictor):**
- Direction: positive = both increase together, negative = inverse relationship
- Magnitude: "For every 1 [unit] increase in X, Y changes by [slope] [units]"
- p-value: If < .05, slope significantly differs from zero

**Intercept:**
- Predicted Y when X = 0
- Often not interpretable (e.g., "exam score when studied 0 hours")

**R²:**
- Proportion of variance in Y explained by X
- Ranges 0-1 (convert to percentage)
- .01 = small, .09 = medium, .25 = large (rough guidelines)

**Example Output Interpretation:**
```
F(1, 48) = 25.6, p < .001, R² = .35

"Hours studied significantly predicted exam scores, F(1, 48) = 25.6, p < .001, 
R² = .35. For every additional hour studied, exam scores increased by 
3.2 points (b = 3.2, SE = 0.63, p < .001)."
```

---

## PART 2: MULTIPLE REGRESSION

### Question
Can I predict one variable from MULTIPLE predictors?

### Example
Can I predict exam scores from hours studied AND sleep quality?

### The Logic
Y = b₀ + b₁X₁ + b₂X₂ + ... + bₖXₖ
- Each slope is the effect of that predictor **holding others constant**
- Order of predictors doesn't matter (R does it simultaneously)

---

### R Code - Multiple Regression

```r
# Visualize relationships (optional)
pairs(data[c("outcome", "predictor1", "predictor2")])
# Or individually:
plot(data$predictor1, data$outcome)
plot(data$predictor2, data$outcome)

# Fit the model (+ means multiple predictors)
model <- lm(outcome ~ predictor1 + predictor2, data = data)

# Get results
summary(model)

# Check assumptions
par(mfrow = c(2,2))
plot(model)
par(mfrow = c(1,1))

shapiro.test(residuals(model))

# Check for multicollinearity (if predictors highly correlated)
library(car)
vif(model)  # VIF > 10 suggests problem

# Get confidence intervals
confint(model)

# Compare models (if testing different predictors)
model1 <- lm(outcome ~ predictor1, data = data)
model2 <- lm(outcome ~ predictor1 + predictor2, data = data)
anova(model1, model2)  # Does adding predictor2 help?
```

---

### Interpreting Multiple Regression Output

**Overall F-test:**
- Tests: Do ALL predictors together explain significant variance?
- Must be significant to interpret individual predictors

**Individual Slopes:**
- Each shows effect of that predictor **controlling for others**
- "Holding X₂ constant, for every 1-unit increase in X₁, Y changes by [slope]"
- p-value: If < .05, that predictor adds unique predictive value

**R²:**
- Total variance explained by all predictors combined
- Compare to R² from simple models to see added value

**Adjusted R²:**
- Penalizes for adding predictors that don't help
- Use when comparing models with different numbers of predictors

**Example Output Interpretation:**
```
F(2, 47) = 18.4, p < .001, R² = .44

"Hours studied and sleep quality together predicted exam scores, 
F(2, 47) = 18.4, p < .001, R² = .44. Hours studied was a significant 
predictor (b = 2.8, SE = 0.58, p < .001), as was sleep quality 
(b = 1.5, SE = 0.42, p = .001). Together, these predictors explained 
44% of the variance in exam scores."
```

---

## PART 3: ASSUMPTIONS & DIAGNOSTICS

### The Four Key Assumptions

#### 1. Linearity
**What:** Relationship between X and Y is straight (not curved)

**Check:**
- Scatterplot: Should see roughly straight pattern
- Residuals vs Fitted plot: Should be random scatter, no curve

**If violated:**
- Try transformation (log, square root, polynomial)
- Consider non-linear regression

#### 2. Independence
**What:** Each observation is independent of others

**Check:**
- Think about your data collection
- Durbin-Watson test (for time series)

**If violated:**
- Need specialized methods (time series, multilevel models)

#### 3. Normality of Residuals
**What:** Errors are normally distributed

**Check:**
- Q-Q plot: Points should follow diagonal line
- Shapiro-Wilk test: `shapiro.test(residuals(model))`
- Histogram of residuals

**If violated:**
- With n > 30, usually OK (Central Limit Theorem)
- Try transformation
- Use robust regression

#### 4. Homoscedasticity (Equal Variance)
**What:** Spread of residuals is constant across predicted values

**Check:**
- Residuals vs Fitted plot: Should see even scatter, not funnel
- Scale-Location plot: Should be horizontal

**If violated:**
- Try transformation
- Use robust standard errors
- Weighted least squares

---

### Reading Diagnostic Plots

```r
par(mfrow = c(2,2))
plot(model)
par(mfrow = c(1,1))
```

**Plot 1: Residuals vs Fitted**
- Purpose: Check linearity and equal variance
- Good: Random scatter around zero
- Bad: Curved pattern (non-linearity) or funnel shape (unequal variance)

**Plot 2: Q-Q Plot**
- Purpose: Check normality of residuals
- Good: Points follow diagonal line
- Bad: S-curve or points far from line

**Plot 3: Scale-Location**
- Purpose: Check equal variance
- Good: Horizontal line with even spread
- Bad: Rising or falling pattern

**Plot 4: Residuals vs Leverage**
- Purpose: Identify influential outliers
- Good: All points inside dashed lines (Cook's distance)
- Bad: Points outside dashed lines with high Cook's distance

---

### Dealing with Outliers

```r
# Find influential points
influential <- which(cooks.distance(model) > 1)
data[influential, ]  # Look at these cases

# Options:
# 1. Check for data entry errors
# 2. Run analysis with and without outliers
# 3. Report both results if they differ substantially
# 4. Use robust regression if many outliers
```

---

## PART 4: ADVANCED TOPICS

### Interactions

**When:** Effect of X₁ depends on level of X₂

```r
# Add interaction term with *
model_int <- lm(outcome ~ predictor1 * predictor2, data = data)
summary(model_int)

# If interaction is significant, interpret it FIRST
# Main effects become "simple effects" when interaction present

# Visualize interaction
library(interactions)
interact_plot(model_int, pred = predictor1, modx = predictor2)
```

**Interpreting Interactions:**
"The effect of hours studied on exam scores depends on sleep quality. 
For students with poor sleep, studying had little effect (b = 1.2, ns), 
but for well-rested students, each hour studied improved scores by 
4.5 points (b = 4.5, p < .001)."

### Categorical Predictors

```r
# R automatically creates dummy variables
data$group <- as.factor(data$group)
model <- lm(outcome ~ group, data = data)

# This is equivalent to ANOVA!
# Coefficients show difference from reference group
```

### Polynomial Regression (Curved Relationships)

```r
# Add squared term for curve
model_quad <- lm(outcome ~ predictor + I(predictor^2), data = data)
summary(model_quad)

# Compare to linear model
anova(model_linear, model_quad)
```

---

## PART 5: REPORTING TEMPLATES

### Simple Regression
"[Predictor] significantly predicted [outcome], b = [slope], SE = [se], 
t([df]) = [t-value], p = [p-value], R² = [R-squared]. For every [unit] 
increase in [predictor], [outcome] [increased/decreased] by [slope] [units]."

### Multiple Regression
"[List predictors] together predicted [outcome], F([df1], [df2]) = [F], 
p = [p], R² = [R-squared]. [Predictor1] was a significant predictor 
(b = [slope1], p = [p1]), as was [predictor2] (b = [slope2], p = [p2]). 
Together, these predictors explained [R²%] of the variance."

### With Interaction
"There was a significant interaction between [X1] and [X2], b = [slope], 
p = [p]. Simple slopes analysis revealed that [describe the pattern]."

---

## Common Mistakes & Solutions

❌ **Mistake:** Not visualizing data before modeling  
✅ **Solution:** ALWAYS plot first - may reveal non-linearity, outliers

❌ **Mistake:** Checking normality of raw Y instead of residuals  
✅ **Solution:** Fit model first, THEN check `residuals(model)`

❌ **Mistake:** Including categorical predictors without factoring  
✅ **Solution:** Use `as.factor()` or use ANOVA instead

❌ **Mistake:** Interpreting R² as "accuracy"  
✅ **Solution:** R² is variance explained, not prediction accuracy

❌ **Mistake:** Adding predictors without theory  
✅ **Solution:** Have theoretical reason for each predictor

❌ **Mistake:** Ignoring multicollinearity  
✅ **Solution:** Check VIF, avoid highly correlated predictors

❌ **Mistake:** Using regression for causal inference  
✅ **Solution:** Regression shows prediction/association, not causation

---

## Quick Troubleshooting

**Error: "NA/NaN/Inf in foreign function call"**
→ Missing data or infinite values: Check with `summary(data)`

**R² is negative**
→ Impossible - check your model specification

**All predictors non-significant but F-test significant**
→ Multicollinearity - check correlations and VIF

**Huge R² (> .90)**
→ Probably included the outcome as a predictor by mistake

**Weird residual patterns**
→ May need transformation or different model

---

## Effect Size & Power

**R² as Effect Size:**
- .01 = small (1% variance explained)
- .09 = medium (9% variance explained)
- .25 = large (25% variance explained)

**Sample Size Planning:**
- Rule of thumb: Need at least 10-15 cases per predictor
- For R² = .10 detection with 80% power: ~100 participants
- Use G*Power for precise calculation

---

## Related Sections
- **Understanding assumptions** → Section 4: Checking Normality
- **Categorical outcome** → Use logistic regression (advanced)
- **Count outcome** → Use Poisson regression (advanced)
- **Categorical predictors** → See ANOVA (Section 5.2)
- **Diagnostic plots** → Section 5.3.4: Regression Diagnostics

---

**See Section 10 (Archives) for:**
- Alternative workflow formats
- Excel-based methods
- Historical versions
- Step-by-step examples
