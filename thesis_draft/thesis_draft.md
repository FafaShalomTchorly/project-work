# Thesis Draft

## Problem Statement

Multilevel (hierarchical) linear mixed models are widely used for data in which observations are nested within groups, which are in turn nested within larger groups. Opioid distribution data are an example: monthly records are nested within ZIP codes, and ZIP codes within counties. Deciding which levels of the hierarchy to include, and which covariates to keep at each level, is a central modeling choice. In practice it is usually made with the Bayesian Information Criterion (BIC).

The standard BIC, $-2\ell(\hat\theta) + k\log n$, comes from a Laplace approximation to the marginal likelihood. It assumes that the information about every parameter grows at the same rate $n$. In multilevel models this assumption fails. The information about a parameter grows with the number of units at its own level:

- month-level effects are informed by all $N$ observations;
- ZIP-level effects by the number of ZIP codes;
- county-level effects only by the number of counties.

Because there is no single "sample size," statistical software and textbooks disagree on what $n$ should be. Some use the total number of observations and some the number of top-level clusters. These choices can lead to different selected models, particularly when there are few top-level units.

For two-level nested models, Cho, Wu and Naveiras (2024) derived a level-specific penalty from the information matrix, and they explicitly leave designs with more levels for future work. Lorah and Womack (2019) suggested by simulation that the penalty should differ by level, but gave no derivation. As a result, no theoretically justified BIC is available for selecting levels and level-specific effects in three-level nested models, and it is unknown how existing BIC variants perform in that setting.

## Research Aim

The aim of this thesis is to derive, under clearly stated asymptotic assumptions, the BIC penalty for the balanced three-level nested linear mixed model. The thesis will then evaluate by simulation whether the resulting criterion selects the correct hierarchical structure more reliably than existing criteria, and apply it to ARCOS opioid distribution data.

The conjectured form to be derived, corrected or confirmed is

$$BIC_3 = -2\ell(\hat\theta) + (p_1+1)\log N + (p_2+1)\log(CZ) + (p_3+1)\log C,$$

where $C$ is the number of counties, $Z$ the number of ZIP codes per county, $T$ the number of months and $N = CZT$. The counts $p_1$, $p_2$ and $p_3$ are the numbers of month-, ZIP- and county-level fixed effects, with the intercept counted at the county level, and each $+1$ is that level's variance component.

## Research Questions

1. **Derivation.** For the balanced three-level random-intercept model, what penalty follows from the asymptotic behavior of the Fisher information matrix, and does it agree with the conjectured $BIC_3$?
2. **Assumptions.** Under what assumptions does each penalty term arise? These include the rates at which $C$, $Z$ and $T$ grow, positivity of the variance components, and the level at which each covariate varies. Where do those assumptions break down?
3. **Structural implications.** How do the penalties on the remaining parameters change when a level is removed from the model, and is the criterion consistent for selecting the correct set of levels?
4. **Finite-sample performance.** In simulation, how often does $BIC_3$ select the correct hierarchical structure and the correct level-specific fixed effects? How does it compare with BIC using the total sample size, BIC using the number of clusters, Jones's effective sample size, marginal and conditional AIC, and likelihood-ratio tests, especially when the number of counties is small?
5. **Application.** In the ARCOS opioid data, which of the ZIP and county levels does $BIC_3$ support, how is variation shared across levels, and do conclusions about income and rurality depend on the levels retained?

## Outline of the Method

**Step 1. Model setup.** Define the balanced three-level random-intercept model and write the county covariance matrix through its spectral decomposition. It has three distinct eigenvalues with known multiplicities.

**Step 2. Theoretical derivation.**

- (a) Use the Laplace approximation to express the log marginal likelihood in terms of the log-determinant of the Fisher information.
- (b) Use the block-diagonal structure of the information matrix to separate the fixed-effect block from the variance-component block.
- (c) Derive the asymptotic order of each block from the eigenvalue structure. The aim is to show which parameters have effective sample size $N$, $CZ$ or $C$.
- (d) State the assumptions under which each term arises, and identify where they fail.
- (e) Derive how the penalties change when a level is removed, and examine the consistency of the criterion.

**Step 3. Numerical verification.** Compute the log-determinant of the information matrix exactly over a grid of $C$, $Z$ and $T$, and check that its growth matches the derived coefficients.

**Step 4. Simulation study.**

- **Data generation.** Generate data from the three-level model, setting variance components to zero to create true reduced models.
- **Design factors.** Vary the number of counties, ZIP codes per county, months, and the intraclass correlations at each level.
- **Comparison.** Compare $BIC_3$ with the existing criteria listed in research question 4.
- **Outcomes.** Record the proportion of correctly selected structures, false-drop and false-keep rates per level, and coverage of fixed-effect intervals after selection.

**Step 5. Application to ARCOS data.**

- **Data preparation.** Prepare the response variable, including checking for zero MME values before any log transformation. Address ZIP codes that cross county boundaries.
- **Selection and interpretation.** Apply $BIC_3$ and the competing criteria, then interpret the selected structure and the income and rurality effects.

**Extensions (only after the three-level theory is complete).** Unbalanced designs, AR(1) errors, a fourth (region) level, and non-nested geographic structures.
