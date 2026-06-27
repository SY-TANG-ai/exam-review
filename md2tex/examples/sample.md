## Hypothesis Testing

### Key Concepts

- **Null hypothesis** ($H_0$): status quo, assumed true until evidence suggests otherwise
- **Alternative hypothesis** ($H_1$ or $H_a$): contradicts $H_0$, what we seek evidence for
- **Test statistic**: computed from sample data, used to decide between $H_0$ and $H_1$
- **p-value**: probability of observing data as extreme as the sample, assuming $H_0$ is true
- **Significance level** $\alpha$: threshold for rejecting $H_0$ (commonly 0.05, 0.01)

### Common Tests

| Test | Purpose | Test Statistic | Assumptions |
|------|---------|---------------|-------------|
| z-test | Compare sample mean vs population mean | $z = \frac{\bar{x} - \mu}{\sigma / \sqrt{n}}$ | Known $\sigma$, normal population or large $n$ |
| t-test | Compare means, unknown $\sigma$ | $t = \frac{\bar{x} - \mu}{s / \sqrt{n}}$ | Normal population, unknown $\sigma$ |
| Paired t-test | Compare paired measurements | $t = \frac{\bar{d}}{s_d / \sqrt{n}}$ | Differences are normal |
| Two-sample t-test | Compare two independent means | $t = \frac{\bar{x}_1 - \bar{x}_2}{s_p \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}}$ | Equal variances, normal populations |
| Chi-square test | Test association / goodness-of-fit | $\chi^2 = \sum \frac{(O_i - E_i)^2}{E_i}$ | Expected counts $\ge 5$ |
| F-test | Compare two variances | $F = \frac{s_1^2}{s_2^2}$ | Normal populations |

### Decision Rule

Two approaches:

1. **p-value approach**: reject $H_0$ if $\text{p-value} < \alpha$
2. **Critical value approach**: reject $H_0$ if $|\text{test statistic}| > \text{critical value}$

### Type I and Type II Errors

|  | $H_0$ true | $H_0$ false |
|---|-----------|------------|
| **Reject $H_0$** | Type I error ($\alpha$) | Correct |
| **Fail to reject $H_0$** | Correct | Type II error ($\beta$) |

- **Power**: $1 - \beta$, probability of correctly rejecting a false $H_0$

### Confidence Interval vs Hypothesis Test

Confidence interval contains **all plausible values** of the parameter.

```
If 95% CI contains the null value   → do not reject H₀ at α = 0.05
If 95% CI excludes the null value   → reject H₀ at α = 0.05
```

---

## ANOVA (Analysis of Variance)

### One-Way ANOVA

Compare means of $k \ge 3$ groups.

$$
F = \frac{\text{between-group variability}}{\text{within-group variability}} = \frac{SSB/(k-1)}{SSW/(n-k)}
$$

**Assumptions**: independence, normality, equal variance across groups.

### Two-Way ANOVA

Factors: A (rows), B (columns), interaction A×B.

| Source | df | SS | MS | F |
|--------|----|----|----|----|
| Factor A | $a-1$ | SSA | MSA = SSA/(a-1) | MSA/MSE |
| Factor B | $b-1$ | SSB | MSB = SSB/(b-1) | MSB/MSE |
| Interaction | $(a-1)(b-1)$ | SSAB | MSAB = SSAB/(df) | MSAB/MSE |
| Error | $n-ab$ | SSE | MSE = SSE/(n-ab) | |
| Total | $n-1$ | SST | | |

### Post-hoc Tests

- **Tukey's HSD**: compare all pairwise means, controls family-wise error rate
- **Bonferroni correction**: adjust $\alpha$ by dividing by number of comparisons

---

## Regression

### Simple Linear Regression

$$
y_i = \beta_0 + \beta_1 x_i + \varepsilon_i, \quad \varepsilon_i \sim N(0, \sigma^2)
$$

**Estimates**:

$$
\hat{\beta}_1 = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sum (x_i - \bar{x})^2}, \quad
\hat{\beta}_0 = \bar{y} - \hat{\beta}_1 \bar{x}
$$

### R-squared (Coefficient of Determination)

$$
R^2 = 1 - \frac{SSE}{SST} = \frac{SSR}{SST}
$$

Interpreted as "proportion of variance in $y$ explained by $x$."

### ANOVA Table for Regression

| Source | df | SS | MS | F |
|--------|----|----|----|----|
| Regression | 1 | SSR | MSR = SSR/1 | MSR/MSE |
| Residual | $n-2$ | SSE | MSE = SSE/(n-2) | |
| Total | $n-1$ | SST | | |

### Model Diagnostics

- **Residuals vs. fitted**: check constant variance
- **Q-Q plot**: check normality
- **Cook's distance**: identify influential points

```text
Rule of thumb: D_i > 4/n is influential
```

---

## Distribution Relationships

```
Normal(μ, σ²) ────→ t(df)          [when σ unknown, estimated by s]
Normal(0, 1)   ────→ χ²(k)         [sum of k squared z-scores]
t(k)           ────→ F(1, k)       [square of t-statistic]
χ²(k₁)/k₁
──────────    ═════   F(k₁, k₂)   [ratio of two chi-squares]
χ²(k₂)/k₂
```
