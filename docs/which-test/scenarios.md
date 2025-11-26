# Common Research Scenarios

Real research questions matched to the appropriate statistical test.

## 🔬 Research Questions → Tests

### Scenario 1: Coffee and Reaction Time
**Question:** Does drinking coffee affect reaction time compared to no coffee?

**Setup:**
- Two groups: Coffee vs No Coffee
- Outcome: Reaction time (continuous)
- Different people in each group

**Test:** Independent t-test  
**See:** [T-Tests](../parametric/t-tests/overview.md)

---

### Scenario 2: Teaching Methods
**Question:** Do three teaching methods produce different test scores?

**Setup:**
- Three groups: Lecture, Discussion, Flipped
- Outcome: Test scores (continuous)
- Different students in each group

**Test:** One-Way ANOVA  
**See:** [ANOVA](../parametric/anova/overview.md)

---

### Scenario 3: Before and After Training
**Question:** Did scores improve after the training program?

**Setup:**
- Same people measured twice: Before and After
- Outcome: Skill scores (continuous)
- Paired measurements

**Test:** Paired t-test  
**See:** [T-Tests](../parametric/t-tests/overview.md)

---

### Scenario 4: Caffeine × Time of Day
**Question:** Do caffeine AND time of day both affect performance?

**Setup:**
- Two factors: Caffeine level (None/Low/High) × Time (Morning/Evening)
- Outcome: Performance score (continuous)
- Testing main effects and interaction

**Test:** Two-Way ANOVA  
**See:** [ANOVA](../parametric/anova/overview.md)

---

### Scenario 5: Study Hours → GPA
**Question:** Can I predict GPA from study hours?

**Setup:**
- Predictor: Study hours per week (continuous)
- Outcome: GPA (continuous)
- Want to predict one from the other

**Test:** Simple Linear Regression  
**See:** [Regression](../parametric/regression/overview.md)

---

### Scenario 6: Gender × Major Choice
**Question:** Is gender related to major choice?

**Setup:**
- Two categorical variables: Gender × Major
- Count data (frequencies)
- Testing association

**Test:** Chi-Square Test of Independence  
**See:** [Chi-Square](../chi-square/overview.md)

---

### Scenario 7: Drug A vs Drug B vs Placebo
**Question:** Which treatment is most effective?

**Setup:**
- Three independent groups
- Outcome: Symptom severity (continuous)
- Want to know if any differ

**Test:** One-Way ANOVA + post-hoc tests  
**See:** [ANOVA](../parametric/anova/overview.md)

---

### Scenario 8: Stress Over Semester
**Question:** Does stress change across beginning, middle, and end of semester?

**Setup:**
- Same students measured 3 times
- Outcome: Stress score (continuous)
- Repeated measurements

**Test:** Repeated Measures ANOVA  
**See:** [ANOVA](../parametric/anova/overview.md)

---

### Scenario 9: Non-Normal Data, Two Groups
**Question:** Do meditation and control groups differ on anxiety?

**Setup:**
- Two independent groups
- Outcome: Anxiety scores (highly skewed, non-normal)
- Assumptions violated

**Test:** Mann-Whitney U Test  
**See:** [Non-Parametric](../non-parametric/overview.md)

---

### Scenario 10: Multiple Predictors
**Question:** Can I predict burnout from workload, social support, and sleep quality?

**Setup:**
- Multiple predictors (all continuous)
- One continuous outcome
- Want to see unique contribution of each

**Test:** Multiple Regression  
**See:** [Regression](../parametric/regression/overview.md)

---

## 🎯 Quick Pattern Recognition

| Your Situation | Pattern | Test |
|----------------|---------|------|
| 1 group vs known value | Compare to standard | One-Sample t-test |
| 2 independent groups | Group comparison | Independent t-test |
| Same people, 2 times | Before/after | Paired t-test |
| 3+ independent groups | Multiple group comparison | One-Way ANOVA |
| 2 factors | Factorial design | Two-Way ANOVA |
| Same people, 3+ times | Longitudinal | Repeated Measures ANOVA |
| Continuous predictor → continuous outcome | Prediction | Regression |
| 2 categorical variables | Association | Chi-Square |
| Height ↔ Weight | Relationship strength | Correlation |

---

[← Decision Tree](decision-tree.md) | [Home](../index.md) | [Effect Sizes →](../effect-sizes/reference.md)
