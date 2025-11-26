# ANOVA: Definitive Workflow
**Last Updated: November 2024 | Use this version for all ANOVA analyses**

## Quick Reference
**Purpose:** Compare means across three or more groups  
**When to use:** One continuous outcome + one or more categorical predictors  
**Alternative if assumptions fail:** Kruskal-Wallis test (Section 6)

---

## Decision Tree
```
How many factors (independent variables)?
├─ ONE factor (3+ groups) → One-Way ANOVA
└─ TWO factors → Two-Way ANOVA
    ├─ Test main effects only → Use +
    └─ Test interaction → Use *
```

---

## 1. ONE-WAY ANOVA
**Question:** Do 3+ groups have different means?

**Example:** Do reaction times differ across three caffeine doses (0mg, 100mg, 200mg)?

### R Code
```r
# Prepare data
data$group <- as.factor(data$group)  # CRITICAL: must be a factor

# Visualize first (always!)
boxplot(score ~ group, data = data,
        xlab = "Group", ylab = "Score",
        col = rainbow(length(unique(data$group))))

# Fit the model
model <- aov(score ~ group, data = data)

# Check assumptions on RESIDUALS (not raw data!)
# Normality
shapiro.test(residuals(model))
hist(residuals(model), main = "Residuals Distribution")

# Diagnostic plots
par(mfrow = c(2,2))
plot(model)
par(mfrow = c(1,1))

# If assumptions OK, proceed:
summary(model)

# If significant, run post-hoc tests
TukeyHSD(model)

# Get descriptives
aggregate(score ~ group, data = data, FUN = mean)
aggregate(score ~ group, data = data, FUN = sd)
aggregate(score ~ group, data = data, FUN = length)

# Effect size
library(effectsize)
eta_squared(model)
```

### Interpreting Output

**From summary(model):**
- **F-value:** Ratio of between-group to within-group variance
- **Pr(>F):** p-value; if < .05, at least one group differs
- **Does NOT tell you which groups differ** - need post-hoc for that

**From TukeyHSD(model):**
- **diff:** Mean difference between those two groups
- **p adj:** Adjusted p-value (controls for multiple comparisons)
- If p adj < .05, those two specific groups differ significantly

**Effect Size (η²):**
| Value | Interpretation |
|-------|----------------|
| .01   | Small          |
| .06   | Medium         |
| .14   | Large          |

### Reporting Template
"A one-way ANOVA showed a significant effect of [factor] on [outcome], F([df1], [df2]) = [F-value], p = [p-value], η² = [effect size]. Post-hoc Tukey tests revealed that [specific comparisons]."

**Example:** "A one-way ANOVA showed a significant effect of caffeine dose on reaction time, F(2, 57) = 12.43, p < .001, η² = .30 (large effect). Post-hoc Tukey tests revealed that both the 100mg group (M = 285ms, SD = 23) and 200mg group (M = 265ms, SD = 19) were significantly faster than the 0mg control group (M = 325ms, SD = 28), but the two caffeine groups did not differ from each other."

---

## 2. TWO-WAY ANOVA (Main Effects Only)
**Question:** Do two factors independently affect the outcome?

**Example:** Do caffeine AND time-of-day both affect alertness (but don't interact)?

### R Code
```r
# Prepare data
data$factorA <- as.factor(data$factorA)
data$factorB <- as.factor(data$factorB)

# Visualize
boxplot(score ~ factorA * factorB, data = data)

# Fit model (+ means main effects only, no interaction)
model2 <- aov(score ~ factorA + factorB, data = data)

# Check assumptions
shapiro.test(residuals(model2))
plot(model2)

# View results
summary(model2)

# Post-hocs (if significant)
TukeyHSD(model2)

# Descriptives by both factors
aggregate(score ~ factorA + factorB, data = data, FUN = mean)
aggregate(score ~ factorA + factorB, data = data, FUN = sd)

# Effect sizes
eta_squared(model2)
```

### Interpreting Output
- You get **two separate F-tests** - one for each factor
- Each p-value tells you if that factor matters
- Post-hoc tests show which specific levels differ

---

## 3. TWO-WAY ANOVA (With Interaction)
**Question:** Do two factors interact - does the effect of one depend on the other?

**Example:** Does caffeine's effect on performance differ between morning and evening?

### R Code
```r
# Prepare data
data$factorA <- as.factor(data$factorA)
data$factorB <- as.factor(data$factorB)

# Visualize interaction
interaction.plot(data$factorA, data$factorB, data$score,
                xlab = "Factor A", ylab = "Score", 
                trace.label = "Factor B")

# Fit model (* includes main effects AND interaction)
model3 <- aov(score ~ factorA * factorB, data = data)

# Check assumptions
shapiro.test(residuals(model3))
plot(model3)

# View results
summary(model3)

# Post-hocs
TukeyHSD(model3)

# Descriptives for each cell
aggregate(score ~ factorA + factorB, data = data, FUN = mean)
aggregate(score ~ factorA + factorB, data = data, FUN = sd)

# Effect sizes
eta_squared(model3)
```

### Interpreting Interaction

**If interaction is significant (p < .05):**
→ **Interpret the interaction FIRST**, before looking at main effects
→ Main effects are misleading when there's an interaction

**How to describe an interaction:**
"The effect of [Factor A] depends on the level of [Factor B]"

**Interaction plot patterns:**

```
Parallel lines = NO interaction     Crossing/diverging = INTERACTION
     •——————•                              •
     •——————•                            •    •
                                      •         •
```

### Reporting Template
"A two-way ANOVA revealed a significant interaction between [A] and [B], F([df1], [df2]) = [F], p = [p]. Simple effects analysis showed that [describe pattern]. Main effects: [report if interaction not significant]."

**Example:** "A two-way ANOVA revealed a significant interaction between caffeine and time-of-day, F(2, 84) = 8.32, p < .001, η² = .17. Caffeine improved performance in the morning (0mg: M = 72, 200mg: M = 89) but had no effect in the evening (0mg: M = 68, 200mg: M = 70)."

---

## 4. REPEATED MEASURES ANOVA
**Question:** Did scores change across 3+ time points (same people)?

**Example:** Did anxiety decrease across baseline, week 4, and week 8?

### Option 1: Use Multiple Paired T-Tests (RECOMMENDED FOR BEGINNERS)
```r
# For 3 time points, run 3 comparisons
t.test(data$time1, data$time2, paired = TRUE)
t.test(data$time1, data$time3, paired = TRUE)
t.test(data$time2, data$time3, paired = TRUE)

# Apply Bonferroni correction manually
# New alpha = .05 / 3 = .017
# Only call p < .017 significant
```

### Option 2: True Repeated Measures ANOVA
**Requires data in LONG format:**
```
subject | time | score
1       | 1    | 45
1       | 2    | 52
1       | 3    | 58
2       | 1    | 42
...
```

```r
# Using ez package
install.packages("ez")
library(ez)

ezANOVA(
  data = data_long,
  dv = score,           # Outcome
  wid = subject,        # Subject ID
  within = time,        # Repeated factor
  detailed = TRUE
)

# Post-hoc if significant
pairwise.t.test(data_long$score, data_long$time, 
                paired = TRUE, 
                p.adjust.method = "bonferroni")
```

---

## Assumptions Checklist

### ✓ Independence
- Each participant should be independent
- For repeated measures, repeated observations from same person are NOT independent (that's the point!)

### ✓ Normality of Residuals
**CRITICAL:** Check residuals, NOT raw data!
```r
shapiro.test(residuals(model))
hist(residuals(model))
```
- If violated + n < 30 per group → consider Kruskal-Wallis
- If violated + n > 30 per group → usually OK to proceed with caution

### ✓ Homogeneity of Variance
Check with diagnostic plots:
```r
plot(model)  # Look at "Residuals vs Fitted" plot
```
- Should see random scatter, no funnel shape
- If violated → note in limitations, consider Welch's ANOVA

### ✓ No Major Outliers
Check with diagnostic plots - look for points with high Cook's distance

---

## Common Mistakes & Solutions

❌ **Mistake:** Forgetting to convert grouping variables to factors
✅ **Solution:** Always use `as.factor()` first - ANOVA won't work otherwise

❌ **Mistake:** Checking normality of raw data instead of residuals
✅ **Solution:** Fit model first, THEN check `residuals(model)`

❌ **Mistake:** Not running post-hocs when ANOVA is significant
✅ **Solution:** Significant F-test just means "not all groups equal" - use TukeyHSD to find which differ

❌ **Mistake:** Using ANOVA for only 2 groups
✅ **Solution:** Use t-test for 2 groups (gives same result but easier)

❌ **Mistake:** Ignoring significant interaction and reporting main effects
✅ **Solution:** If interaction p < .05, interpret interaction first

❌ **Mistake:** Using + when you meant * (or vice versa)
✅ **Solution:** 
- `A + B` = main effects only (no interaction)
- `A * B` = main effects AND interaction

---

## Effect Size Reference
| η² (eta-squared) | Interpretation |
|------------------|----------------|
| .01              | Small          |
| .06              | Medium         |
| .14              | Large          |

---

## Quick Troubleshooting

**Error: "contrasts can be applied only to factors"**
→ Your grouping variable isn't a factor: `data$group <- as.factor(data$group)`

**Post-hocs showing everything significant**
→ Check if you have huge sample size (makes trivial differences "significant")
→ Look at effect sizes and actual mean differences

**F-statistic is huge (like F = 1847)**
→ Probably data entry error or wrong test - check your variables

**Can't find TukeyHSD results for interaction**
→ Use: `TukeyHSD(model, "factorA:factorB")` to get interaction comparisons

---

## Related Sections
- **Only 2 groups?** → Use t-test instead (Section 5.1)
- **Assumptions failing?** → See Section 4: Checking Normality
- **Need non-parametric alternative?** → Kruskal-Wallis (Section 6)
- **Continuous predictor?** → Use regression instead (Section 5.3)

---

**See Section 10 (Archives) for:**
- Alternative workflow formats
- Historical versions
- Additional examples
