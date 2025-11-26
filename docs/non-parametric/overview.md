# Non-Parametric Tests

!!! info "When to Use Non-Parametric Tests"
    Non-parametric tests are alternatives to t-tests and ANOVA when your data violate assumptions (especially normality). They work with ranks instead of raw values.

## 🎯 Why Use Non-Parametric Tests?

### When Assumptions Fail

Parametric tests (t-tests, ANOVA) assume:
- Normal distribution
- Continuous data
- Equal variances (sometimes)

**If these fail:** Use non-parametric alternatives!

### Advantages
✓ No normality assumption  
✓ Robust to outliers  
✓ Work with ordinal data  
✓ Work with small samples  

### Disadvantages
✗ Less statistical power (when assumptions are met)  
✗ Can't calculate effect sizes as easily  
✗ More conservative  

---

## 🔀 Non-Parametric Alternatives

| Parametric Test | Non-Parametric Alternative | Use When |
|----------------|----------------------------|----------|
| Independent t-test | **Mann-Whitney U** | 2 independent groups, non-normal |
| Paired t-test | **Wilcoxon Signed-Rank** | 2 paired samples, non-normal |
| One-Way ANOVA | **Kruskal-Wallis** | 3+ independent groups, non-normal |
| Repeated Measures ANOVA | **Friedman Test** | 3+ paired samples, non-normal |
| Pearson correlation | **Spearman correlation** | Non-linear or ordinal data |

---

## 📊 Mann-Whitney U Test

**Purpose:** Compare two independent groups when data are non-normal

**R Code:**
```r
# Perform test
wilcox.test(outcome ~ group, data = data)

# Descriptive statistics by group
tapply(data$outcome, data$group, median)
tapply(data$outcome, data$group, IQR)

# Visualization
boxplot(outcome ~ group, data = data,
        main = "Comparison by Group",
        ylab = "Outcome")
```

**Interpreting Output:**
- **W statistic:** Test statistic (or U)
- **p-value:** If < .05, groups differ significantly
- Report **medians** instead of means

**Reporting:**
"A Mann-Whitney U test showed that Group A (Mdn = [median], IQR = [IQR]) scored significantly higher than Group B (Mdn = [median], IQR = [IQR]), U = [statistic], p = [p-value]."

---

## 🔄 Wilcoxon Signed-Rank Test

**Purpose:** Compare two paired samples when differences are non-normal

**R Code:**
```r
# Perform test
wilcox.test(data$pre, data$post, paired = TRUE)

# Descriptive statistics
median(data$pre)
IQR(data$pre)
median(data$post)
IQR(data$post)

# Visualization
boxplot(data$pre, data$post,
        names = c("Pre", "Post"),
        main = "Before vs After",
        col = c("lightblue", "lightgreen"))
```

**Interpreting Output:**
- **V statistic:** Test statistic
- **p-value:** If < .05, significant change occurred
- Report **medians**

**Reporting:**
"A Wilcoxon signed-rank test showed that scores significantly increased from pre-test (Mdn = [median], IQR = [IQR]) to post-test (Mdn = [median], IQR = [IQR]), V = [statistic], p = [p-value]."

---

## 📈 Kruskal-Wallis Test

**Purpose:** Compare 3+ independent groups when data are non-normal

**R Code:**
```r
# Perform test
kruskal.test(outcome ~ group, data = data)

# Descriptive statistics
tapply(data$outcome, data$group, median)
tapply(data$outcome, data$group, IQR)

# Visualization
boxplot(outcome ~ group, data = data,
        main = "Comparison Across Groups",
        col = rainbow(length(unique(data$group))))

# Post-hoc tests (if significant)
pairwise.wilcox.test(data$outcome, data$group,
                     p.adjust.method = "bonferroni")
```

**Interpreting Output:**
- **H statistic:** Test statistic (chi-square distribution)
- **p-value:** If < .05, at least one group differs
- **Post-hoc tests:** Show which groups differ

**Reporting:**
"A Kruskal-Wallis test showed a significant effect of group, H(df) = [H], p = [p-value]. Post-hoc Wilcoxon tests with Bonferroni correction revealed that Group A (Mdn = [median]) differed significantly from Group B (Mdn = [median]), but not from Group C."

---

## 🔁 Friedman Test

**Purpose:** Compare 3+ paired samples when data are non-normal (repeated measures)

**R Code:**
```r
# Data must be in wide format (one row per subject)
# Columns: subject, time1, time2, time3, ...

friedman.test(y = as.matrix(data[, c("time1", "time2", "time3")]))

# Descriptive statistics
apply(data[, c("time1", "time2", "time3")], 2, median)
apply(data[, c("time1", "time2", "time3")], 2, IQR)

# Visualization
boxplot(data[, c("time1", "time2", "time3")],
        names = c("Time 1", "Time 2", "Time 3"),
        main = "Changes Over Time")
```

**Reporting:**
"A Friedman test showed a significant effect of time, χ²(df) = [statistic], p = [p-value]."

---

## 📊 Spearman Correlation

**Purpose:** Measure relationship strength when data are non-normal or ordinal

**R Code:**
```r
# Perform test
cor.test(data$var1, data$var2, method = "spearman")

# Visualization
plot(data$var1, data$var2,
     main = "Relationship Between Variables",
     xlab = "Variable 1", ylab = "Variable 2",
     pch = 19, col = "steelblue")
```

**Interpreting Output:**
- **rho (ρ):** Correlation coefficient (-1 to 1)
- **p-value:** If < .05, correlation is significant
- Interpretation same as Pearson r

**Reporting:**
"A Spearman correlation showed a significant positive relationship between Variable 1 and Variable 2, ρ = [rho], p = [p-value]."

---

## 🔍 Choosing the Right Test

!!! question "Decision Guide"
    **How many groups?**
    
    - 2 independent groups → Mann-Whitney U
    - 2 paired samples → Wilcoxon Signed-Rank
    - 3+ independent groups → Kruskal-Wallis
    - 3+ paired samples → Friedman Test
    
    **Examining relationship?** → Spearman correlation

---

## ⚠️ Important Notes

!!! warning "Assumptions Still Exist!"
    Non-parametric tests still have some assumptions:
    
    - Independence of observations
    - Similar distribution shapes (for some tests)
    - Symmetric differences (Wilcoxon)

!!! tip "When in Doubt"
    If your data meet parametric assumptions, use parametric tests (they have more power). Only use non-parametric when assumptions clearly fail.

!!! note "Effect Sizes"
    Effect sizes for non-parametric tests are less standardized. You can report:
    
    - **r = Z / √N** (general effect size)
    - Median differences
    - Rank-biserial correlation

---

## 📚 Quick Reference Table

| Test | R Function | Reports | Effect Size |
|------|-----------|---------|-------------|
| Mann-Whitney U | `wilcox.test()` | U, medians | r = Z/√N |
| Wilcoxon | `wilcox.test(paired=TRUE)` | V, medians | r = Z/√N |
| Kruskal-Wallis | `kruskal.test()` | H, medians | η² (approx) |
| Friedman | `friedman.test()` | χ², medians | Kendall's W |
| Spearman | `cor.test(method="spearman")` | ρ | ρ itself |

---

[← Back to Home](../index.md) | [Decision Tree](../which-test/decision-tree.md) | [Chi-Square →](../chi-square/overview.md)
