# T-TESTS: Definitive Workflow
**Last Updated: November 2024 | Use this version for all t-test analyses**

## Quick Reference
**Purpose:** Compare means between groups or against a known value  
**When to use:** Continuous outcome variable, approximately normal data  
**Alternative if assumptions fail:** Mann-Whitney U or Wilcoxon signed-rank (Section 6)

---

## Decision Tree
```
How many groups do you have?
├─ ONE group (compare to known value) → One-Sample t-test
├─ TWO independent groups → Independent Samples t-test  
└─ TWO measurements (same people) → Paired t-test
```

---

## 1. ONE-SAMPLE T-TEST
**Question:** Does my sample mean differ from a known population value?

**Example:** Do my students' IQ scores differ from the national average (100)?

### R Code
```r
# Check normality first
hist(data$score)
shapiro.test(data$score)  # If p > .05, data are normal enough

# Run test
t.test(data$score, mu = 100)  # Replace 100 with your comparison value

# Get descriptives
mean(data$score, na.rm = TRUE)
sd(data$score, na.rm = TRUE)
```

### Interpreting Output
- **t-value:** How many standard errors your mean is from the comparison value
- **p-value:** If < .05, your mean significantly differs from the comparison value
- **95% CI:** Range likely to contain the true mean; if it doesn't include your comparison value (e.g., 100), the result is significant

### Reporting Template
"A one-sample t-test showed that the sample mean (M = [mean], SD = [sd]) was significantly [higher/lower] than [comparison value], t([df]) = [t-value], p = [p-value]."

**Example:** "A one-sample t-test showed that the sample mean (M = 105.3, SD = 12.4) was significantly higher than 100, t(49) = 3.02, p = .004."

---

## 2. INDEPENDENT SAMPLES T-TEST
**Question:** Do two separate groups have different means?

**Example:** Do students who drank coffee have faster reaction times than those who didn't?

### R Code
```r
# Prepare data
data$group <- as.factor(data$group)  # Make sure group is a factor

# Check normality for EACH group
tapply(data$score, data$group, shapiro.test)

# Visualize
boxplot(score ~ group, data = data, 
        xlab = "Group", ylab = "Score",
        col = c("lightblue", "lightcoral"))

# Run test (Welch's t-test is the default and safest)
t.test(score ~ group, data = data)

# Get descriptives by group
aggregate(score ~ group, data = data, FUN = mean)
aggregate(score ~ group, data = data, FUN = sd)
aggregate(score ~ group, data = data, FUN = length)  # Sample sizes

# Effect size (if significant)
library(effsize)
cohen.d(score ~ group, data = data)
```

### Interpreting Output
- **t-value:** Size of the difference between groups (in standard error units)
- **p-value:** If < .05, groups differ significantly
- **95% CI:** Range for the true difference between means; if it includes 0, not significant
- **Cohen's d:** Effect size (0.2 = small, 0.5 = medium, 0.8 = large)

### Reporting Template
"An independent samples t-test showed that Group A (M = [mean1], SD = [sd1]) scored significantly [higher/lower] than Group B (M = [mean2], SD = [sd2]), t([df]) = [t-value], p = [p-value], d = [effect size]."

**Example:** "An independent samples t-test showed that the coffee group (M = 245.3, SD = 18.2 ms) had significantly faster reaction times than the control group (M = 280.5, SD = 22.1 ms), t(38) = 5.43, p < .001, d = 1.72 (large effect)."

---

## 3. PAIRED T-TEST
**Question:** Did scores change from Time 1 to Time 2 for the same people?

**Example:** Did students' anxiety scores decrease after the intervention?

### R Code
```r
# Check normality of DIFFERENCES (not the raw scores!)
differences <- data$post - data$pre
hist(differences)
shapiro.test(differences)

# Visualize
boxplot(data$pre, data$post, 
        names = c("Pre", "Post"),
        ylab = "Score",
        col = c("lightblue", "lightgreen"))

# Run test
t.test(data$pre, data$post, paired = TRUE)

# Get descriptives
mean(data$pre, na.rm = TRUE)
sd(data$pre, na.rm = TRUE)
mean(data$post, na.rm = TRUE)
sd(data$post, na.rm = TRUE)

# Effect size
library(effsize)
cohen.d(data$pre, data$post, paired = TRUE)
```

### Interpreting Output
- **t-value:** How much scores changed (in standard error units)
- **p-value:** If < .05, there was a significant change
- **95% CI:** Range for the true mean difference; if it doesn't include 0, significant
- **Cohen's d:** Effect size of the change

### Reporting Template
"A paired samples t-test showed that scores significantly [increased/decreased] from pre-test (M = [mean1], SD = [sd1]) to post-test (M = [mean2], SD = [sd2]), t([df]) = [t-value], p = [p-value], d = [effect size]."

**Example:** "A paired samples t-test showed that anxiety significantly decreased from pre-intervention (M = 42.3, SD = 8.1) to post-intervention (M = 28.7, SD = 6.4), t(29) = 8.21, p < .001, d = 1.85 (large effect)."

---

## Assumptions Check

### Normality
- **Check:** Shapiro-Wilk test + histogram or Q-Q plot
- **If violated:** Use non-parametric alternative (see Section 6)
- **Note:** For independent and paired t-tests with n > 30 per group, t-tests are robust to moderate violations

### Independence
- Each observation should be independent
- For paired tests, pairs should be independent of other pairs

### Homogeneity of Variance (Independent t-test only)
- **Don't worry about this** - R's default Welch's t-test handles unequal variances
- If you want to check anyway: `var.test(score ~ group, data = data)`

---

## Common Mistakes & Solutions

❌ **Mistake:** Forgetting to make group variable a factor
✅ **Solution:** Always use `data$group <- as.factor(data$group)` first

❌ **Mistake:** Using independent t-test for repeated measures data
✅ **Solution:** Same people measured twice = paired t-test

❌ **Mistake:** For paired test, checking normality of raw scores instead of differences
✅ **Solution:** Create difference scores first, then check normality

❌ **Mistake:** Running t-test with 3+ groups
✅ **Solution:** Use ANOVA instead (Section 5.2)

❌ **Mistake:** Reporting "t = 2.45, p = .018" without context
✅ **Solution:** Always include means, SDs, and interpretation

---

## Quick Troubleshooting

**Error: "grouping factor must have exactly 2 levels"**
→ Check: `table(data$group)` - you might have 3+ groups or typos in group names

**Error: "not enough observations"**
→ Check for NAs: `sum(is.na(data$score))` - add `na.rm = TRUE` to calculations

**p-value exactly = 1.000**
→ Your groups have identical means - this is unusual, check your data

**Very small p-value (e.g., 2.2e-16)**
→ Report as "p < .001" in your write-up

---

## Effect Size Reference
| Cohen's d | Interpretation |
|-----------|----------------|
| 0.0 - 0.2 | Negligible     |
| 0.2 - 0.5 | Small          |
| 0.5 - 0.8 | Medium         |
| 0.8+      | Large          |

---

## Related Sections
- **Assumptions failing?** → See Section 4: Checking Normality
- **Need non-parametric alternative?** → See Section 6: Non-Parametric Tests
- **3+ groups?** → See Section 5.2: ANOVA
- **Continuous predictor?** → See Section 5.3: Regression

---

**See Section 10 (Archives) for:**
- Excel-based t-test instructions
- Alternative workflow formats
- Historical versions
