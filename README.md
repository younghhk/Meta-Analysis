# 🧠 Meta-Analysis for Clinical Research

## 📘 Introduction

Meta-analysis is a **statistical method** for combining results from multiple independent studies to obtain a **pooled estimate** of an effect size. It increases **statistical power**, resolves uncertainty across studies, and provides a quantitative summary of evidence — commonly used in **clinical research** to synthesize outcomes such as treatment effects or risk estimates.

This repository demonstrates how to conduct a **meta-analysis** in R, including both **fixed-effect** and **random-effects** models, and supports estimates such as:

- Regression coefficients (**β**)  
- **Odds Ratios (OR)**  
- **Hazard Ratios (HR)**  
- **Risk Ratios (RR)**  
- **Mean Differences (MD)** and **Standardized Mean Differences (SMD)**  


##  Input Data Format

Each study should include the following columns:

| Study | Estimate | SE | Lower_CI | Upper_CI | Weight |
|--------|-----------|----|-----------|-----------|---------|
| Chanock 2020 | 0.45 | 0.12 | 0.21 | 0.69 | 0.10 |
| Albert 2021   | 0.38 | 0.09 | 0.20 | 0.56 | 0.12 |
| Hong 2022   | 0.60 | 0.15 | 0.31 | 0.89 | 0.08 |



Note: The **Weight** column represents how much influence each study has in the overall pooled estimate.  
It is determined by the **precision** of each study’s effect size — studies with smaller standard errors (SE) or larger sample sizes receive higher weight.

Formally:

$$
w_i = \frac{1}{\mathrm{Var}(\hat{\theta}_i)} = \frac{1}{SE_i^2}
$$

For **fixed-effect models**, all studies are assumed to estimate the same true effect, and weights depend only on sampling variance.  
For **random-effects models**, the weight is adjusted to account for between-study heterogeneity:

$$w_i^* = \frac{1}{SE_i^2 + \tau^2}$$

where \( \tau^2 \) is the between-study variance.

##  Model Framework

###  Fixed-Effect Model

Assumes all studies estimate the same true effect (no between-study heterogeneity).

$$
\hat{\theta}_{FE} = \frac{\sum_i w_i \theta_i}{\sum_i w_i}
$$

where  
$$w_i = \frac{1}{\mathrm{Var}(\theta_i)}$$

In a fixed-effect model, each study is weighted by the **inverse of its variance**, meaning studies with smaller variance (i.e., more precise estimates) have greater influence on the pooled effect.


###  Random-Effects Model

Allows for **between-study variability**, assuming each study estimates a different true effect.

$$
\hat{\theta}_{RE} = \frac{\sum_i w_i^* \theta_i}{\sum_i w_i^*}
$$

with  
$$w_i^* = \frac{1}{\mathrm{Var}(\theta_i) + \tau^2}$$

where $\tau^2$ represents the **between-study variance** (heterogeneity).  


## 🌳 Forest and Funnel Plots

### 🌲 Forest Plot

A **forest plot** visually summarizes the results of individual studies and the pooled estimate from a meta-analysis.

Each line represents:
- A **square** (the study’s effect estimate, size proportional to weight)
- A **horizontal line** (the study’s 95% confidence interval)
- A **diamond** (the pooled effect estimate and its confidence interval)

This plot helps you quickly see:
- The consistency (or variation) across studies  
- Whether most effects favor the same direction (e.g., treatment > control)  
- The overall combined effect and its uncertainty  




### 🪶 Funnel Plot

A **funnel plot** is a scatter plot used to check for **publication bias** or **small-study effects**.

- The **x-axis** shows the effect estimate (e.g., log OR, HR)  
- The **y-axis** shows study precision (often the inverse of SE or sample size)  

In the absence of bias:
- Points should form a symmetrical inverted funnel shape centered around the pooled estimate.  
- Asymmetry may indicate potential bias (e.g., smaller studies showing larger effects).




##  R Packages

Install and load the core packages:

```{r}
install.packages(c("meta", "metafor"))
# optional but recommended for diagnostics
install.packages("dmetar")
```
