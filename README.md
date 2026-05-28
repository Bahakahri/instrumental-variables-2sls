# Instrumental Variables: Causal Returns to Education

## Using College Proximity as an Exogenous Instrument to Correct Ability Bias

---

## Business Question

Does an additional year of schooling **causally** increase earnings? Or does ability 
bias inflate OLS estimates, naturally talented people get more education AND 
earn more regardless of the causal effect?

## Why This Is Interesting

**Endogeneity:** Education is not randomly assigned. OLS regression confounds the 
causal effect with ability bias. We cannot run an RCT on human education. We need 
exogenous variation to identify the true causal effect.

## Methodology

**Two-Stage Least Squares (2SLS) with Instrumental Variables (IV)**

1. **First Stage:** Regress education on college proximity (instrument) + controls
2. **Verify Relevance:** F-statistic > 10 confirms instrument strength
3. **Second Stage:** Regress wages on predicted education (from first stage)
4. **Compare:** IV estimate vs OLS to quantify endogeneity bias

## Stack

Python • pandas • numpy • statsmodels • matplotlib

---

## Key Results

| Estimator | Return to Education | 95% CI | N |
|-----------|-------------------|--------|---|
| **IV (2SLS)** | **14.50%** | [4.16%, 24.85%] | 3,010 |
| OLS | 7.48% | [6.80%, 8.17%] | 3,010 |

**First Stage Strength:** F-statistic = 195.2 (>> 10)  
**Endogeneity Test:** Hausman p = 0.184  
**Instrument:** College proximity (nearc4)

---

## Assumption Validation

| Assumption | Test | Result | Status |
|-----------|------|--------|--------|
| **Relevance** | First-stage F-stat > 10 | 195.2 |  Strong |
| **Exogeneity** | Hausman endogeneity test | p = 0.184 |  Addressed |
| **Exclusion Restriction** | Written argument | Defensible |  Addressed below |

### Exclusion Restriction: Why nearc4 Works

College proximity (nearc4) affects wages **only through education**, not directly.

The logic: Growing up near a college reduces the cost of attending college. This 
shifts educational attainment upward, especially for kids from low education families. 
More education => higher wages. That's the channel.

Why it doesn't directly affect wages: Nearc4 is just whether you grew up in a county 
with a college it's exogenous to you. It doesn't give you better jobs, networks, or 
anything else besides easier access to education. We control for region and urban 
status to account for local labor market differences.

Potential issue: If nearc4 is correlated with unobserved family background (parental 
ambition, wealth) that independently affects wages, the exclusion restriction breaks. 
But the large first-stage effect (F = 195.2) and big IV-OLS gap (14.5% vs 7.5%) 
suggest this isn't a major problem ability bias is real and the instrument is 
isolating something meaningful.

---

## Core Finding

> **One additional year of schooling increases wages by 14.5%** when using college 
> proximity as an instrument. This is nearly double the OLS estimate (7.5%), revealing 
> substantial upward **ability bias** in naive regression. Talented individuals both 
> pursue more education AND earn higher wages regardless of the causal effect OLS 
> conflates these. IV isolates the true causal return.

---

## Data

**Source:** Card (1995) National Longitudinal Survey (NLS) Young Men Cohort  
**Sample:** 3,010 men, born 1940s-1950s  
**Variables:**
- `educ`: Years of schooling
- `lwage`: Log hourly wage
- `nearc4`: Grew up in county with 4-year college (1/0)
- Controls: experience, race, region, urban status

---

## Limitations

1. **Single instrument:** nearc4 alone is the exclusion restriction. If it's correlated 
   with unobserved ability, the estimate is biased.
2. **Local average treatment effect (LATE):** IV estimates the effect for "compliers" 
   (those affected by college proximity), not the general population.
3. **Hausman test inconclusive:** p = 0.184 means we can't reject OLS at 5% level, 
   though IV and OLS differ substantially. This reflects IV's larger standard error.
4. **Historical data:** 1976 earnings, 1980s education levels. Returns may differ in 
   contemporary labor markets.

---

## Related Projects

- [Causal Uplift with Double Machine Learning](https://github.com/Bahakahri/causal-uplift-dmliv)
- [Synthetic Control Methods](https://github.com/Bahakahri/Synthetic-Control)
- [Staggered Difference-in-Differences](https://github.com/Bahakahri/staggered-difference-in-differences)
