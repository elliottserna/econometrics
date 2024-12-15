# Experiments

## Overview

Experiments are a cornerstone of causal inference, providing a framework for directly testing hypotheses about the effects of interventions. By randomly assigning treatments, experiments eliminate selection bias and allow for clear identification of causal relationships. In this chapter, we explore the definitions, designs, and techniques essential for implementing and interpreting experiments in econometric research.

## Definitions

- **Identification Strategy:** A set of assumptions under which variation in treatment assignment $D$ is exogenous.
- **Estimation Strategy:** The econometric technique used to estimate the average causal effect of $D$ on $Y$, given the assumptions given in the identification strategy.
- **Experiment**: A study where the researcher randomly assigns treatment to subjects to establish causal relationships.
- **Randomization**: The process of assigning treatments to subjects using a random mechanism, ensuring that treatment and control groups are statistically equivalent on average.
- **Treatment Group**: The subset of subjects that receives the intervention or treatment being studied.
- **Control Group**: The subset of subjects that does not receive the intervention, serving as a baseline for comparison.
- **Quasi-Experiment**: A study design that approximates random assignment but lacks full control over the treatment assignment process.
- **Balance**: The equivalence of covariates across treatment and control groups, which is essential for valid comparisons.
- **Baseline Controls**: Pre-treatment variables included in the analysis to improve the precision of causal effect estimates.

## Research Designs

Causal inference research designs aim to isolate causal effects by ensuring that differences in outcomes between treatment and control groups are attributable solely to the intervention. They are designed to recover an unbiased estimate $\hat{\delta}$ of the true average treatment effect ($\delta_{ATE}$) of a treatment $D$ on an outcome $Y$.

> This note on recovering an *unbiased* estimate of the treatment effect is the crux of causal inference. More on the assumptions for unbiasedness to come.

Causal inference research designs help overcome the main source of bias: selection bias 

> Recall from the chapter on the [Potential Outcomes Framework](/content/causal/Potential-Outcomes.md), selection bias is the difference in average potential outcomes without treatment, $Y(0)$, between treated and untreated groups stemming often from selection of units into treatment.

To do this, research designs incorporate two components:

1. **Identification Strategy**
2. **Estimation Strategy**

### Identification Strategy:

A research design's identification strategy posits the set of necessary assumptions under which variation in treatment assignment $D$ is exogenous (i.e., uncorrelated with both observed covariates and unobserved confounders in the causal system). It specifies the mechanism by which treatment is (as good as) random and the conditions required to avoid bias.

There are two foundational classes of identification strategies:

**Experiments**

Experiments rely on randomization to create comparable treatment and control groups. This guarantees that any observed differences in outcomes are due to the treatment and not confounding factors. It also ensures independence of treatment, a condition for treatment to be independent and identically-distributed (iid).

**Quasi-Experiments**

Quasi-experiments exploit natural or policy-induced variations that, while not truly random, mimic random assignment. Under quasi-experimental designs, treatment is said to be 'as good as random' under certain identifying assumptions given the particular design (i.e., parallel trends under difference-in-differences). 

Examples include instrumental variable (IV) designs, difference-in-differences (DiD) approaches, and regression discontinuity (RD) designs. While not as robust as true experiments, these methods can provide valuable insights when randomization is infeasible.

### Estimation Strategy:

An estimation strategy refers to the econometric techniques used to estimate the causal effect of treatment $D$ on the outcome $Y$ based on the assumptions outlined in the identification strategy. These methods aim to measure the relationship between treatment and outcome while accounting for potential confounders and the underlying causal structure.

The choice of estimation strategy depends on the identification strategy and the specific assumptions it relies on. Common estimation strategies include:

- **Ordinary Least Squares (OLS):** Often used in randomized experiments or when treatment assignment is exogenous. It estimates the average treatment effect by regressing the outcome on the treatment variable, controlling for covariates.
  
- **Instrumental Variables (IV):** Used when treatment is potentially endogenous, i.e., correlated with unobserved confounders. IVs help to isolate the exogenous variation in treatment that is uncorrelated with the confounders, providing a consistent estimate of the causal effect.

- **Difference-in-Differences (DiD):** Commonly used in quasi-experimental settings where randomization is not possible. It compares the pre-treatment and post-treatment differences in outcomes between a treatment group and a control group, under the assumption of parallel trends.

- **Regression Discontinuity (RD):** A method for estimating causal effects when treatment assignment is determined by a cutoff point. It compares outcomes just above and just below the threshold, assuming that units near the cutoff are comparable except for the treatment.

## Experimental Designs

Let's motivate our exploration of experimental designs with an example:

A CEO is looking to measure the effect of a workplace wellness program ($D$) on the number of sick days workers take ($Y_i$). She hires you to estimate the following population model:

$$Y_i = \alpha + \delta_i D_i + \varepsilon_i$$

where $\delta_i$ is the treatment effect of the wellness program on worker $i$'s number of sick days taken. 

At her firm, the CEO runs an experiment in which:
- Workers are randomly assigned to participate in the wellness program.
- There are no SUTVA violations.

From the firm, you collect the data $\{Y_i, D_i\}_{i=1}^{n}$, with which you estimate:

$$Y_i = \hat{\alpha} + \hat{\delta} D_i + \hat{\varepsilon}_i$$

With this, you ask: Is $\hat{\delta}$ an unbiased estimate of $\delta_{ATE}$? Or, formally, $\mathbb{E}[\hat{\delta}] = \delta_{ATE}$? To answer this, recall the identifying assumptions for $\hat{\delta}$ to be unbiased---specifically, the strict exogeneity assumption.

> ### Identifying Assumptions
>
> The identifying assumptions of OLS estimation are:
>
> 1. **Linearity:** 
> 
> $$y_i = \sum^k_{j=1} \beta_j x_{ij}^{p} + \varepsilon_i$$
> 
> 2. **No Perfect Colinearity:** 
> 
> $$\text{Rank}(\mathbf{X}) = k$$
> 
> 3. **Spherical Error Variance:** 
> 
> $$\text{Var}[\boldsymbol{\varepsilon}] = \sigma^2 \mathbf{I}_n$$
> 
> 4. **Strict Exogeneity:** 
> 
> $$\mathbb{E}[\varepsilon_i \ | \ \mathbf{V}_i] = 0$$
> 
> in addition to the implied assumption of random sampling, no spillover effects, and perfect compliance.

The **strict exogeneity** (zero conditional mean) assumption requires that the error term $\varepsilon_i$ is conditionally mean-independent of all specified covariates associated with $Y_i$. In our context, this means that the independent variable $D_i$ must be uncorrelated with the error term $\varepsilon_i$, **conditional on pre-treatment characteristics** $\mathbf{X}_i$ that are correlated with $Y_i$:  

$$
\mathbb{E}[\varepsilon_i \mid D_i, \mathbf{X}_i] = 0
$$

For $D_i$ to be exogenous, then, it must be unrelated to any unobserved characteristics affecting $Y_i$ that are not included in the regression model (and are thus captured by $\varepsilon_i$).

> **Note:** The implication here is that any variable that *does* affect $Y_i$ and is correlated with included regressors must be specified in the regression as an observed variable $V \in \mathbf{V}$ to avoid omitted variable bias (OVB). More on this later.
> 
> In a general context, though, $\mathbf{V}$ includes both the treatment indicator $D_i$ as well as all observable pre-treatment characteristics $\mathbf{X}_i$ that affect the outcome $Y_i$. These are *pre*-treatment characteristics because these are independent variables that determine outcomes irrespective of treatment status (as evidenced by the fact untreated units still observe an outcome $Y_i(0)$). 
> 
> In any case, where $D_i, \mathbf{X}_i \in \mathbf{V}_i$, we can generalize that, for a regressor to be exogenous, it must be true that $\mathbb{E}[\varepsilon_i \ | \ V_i] = 0$.

This is a strong assumption to satisfy, but, in our case, it's met because treatment ($D_i$) is randomly assigned. The randomness of assignment by definition implies no systematic association between receiving treatment and any other characteristic, observable or unobservable. Furthermore, this implies receiving treatment---a random process---itself has no predictive power over variation in outcomes. Random chance itself doesn't deterministically explain the number of sick days a worker takes because that choice itself is not random; only the observable pre-treatment characteristics $\mathbf{X}_i$, then, affect outcomes.

> Randomness is the key of a randomized experimental design. The randomness of treatment assignment *guarantees* the exogeneity of treatment from all variables by breaking any systematic relationship between the treatment and both the error term and the pre-treatment variables.:
>
> $$D_i \perp\!\!\!\perp Y_i(D), \mathbf{X}_i, \varepsilon_i $$

This subtle mechanism by which the randomness of treatment assignment is not only exogenous but independent of all other characteristics $\mathbf{V}$ points to the key identifying assumption of randomized experiments: that treatment is randomly assigned and thus independent of potential outcomes:

$$D_i \perp\!\!\!\perp Y_i(0), Y_i(1)$$

This identifying assumption is also known as unconditional independence.

> ### Randomization & Independence: Conditional vs. Unconditional
>
>One of the bedrock assumptions in causal inference is **conditional independence** of treatment assignment:
>
>$$D_i \perp\!\!\!\perp Y_i(0), Y_i(1) \ | \ \mathbf{X}_i$$
>
>This assumption holds that, after accounting for the full set of pre-treatment characteristics that determine outcomes ($\mathbf{X}_i$), treatment is independent of potential outcomes. In observational studies where treatment is not randomly assigned, conditional independence allows the econometrician to recover an unbiased estimate of the treatment effect, provided that strict exogeneity holds (i.e., all relevant predictors are included in the regression).
>
>In the context of experiments, randomization strengthens the independence assumption. Recall from the chapter on [Potential Outcomes Framework](/content/causal/Potential-Outcomes.md) that random assignment guarantees **unconditional independence** of treatment from potential outcomes:
>
>$$D_i \perp\!\!\!\perp Y_i(0), Y_i(1)$$
>
>This follows from the fact that randomization ensures treatment is independent of all variables, observed or unobserved:
>
>$$D_i \perp\!\!\!\perp Y_i(D), \mathbf{X}_i, \varepsilon_i$$
>
>While $D_i \perp\!\!\!\perp Y_i(0), Y_i(1) \ | \ \mathbf{X}_i$ applies generally, including in observational studies, randomization ensures that $D_i \perp\!\!\!\perp \mathbf{X}_i$, eliminating any systematic relationship between treatment and pre-treatment characteristics. Thus, in randomized experiments, unconditional independence subsumes conditional independence. However, this stronger assumption of **unconditional independence only holds if randomization is properly conducted**.
>
>Finally, note that even under randomization, conditioning on $X_j \in \mathbf{X}_i$ remains valid. Since $D_i \perp\!\!\!\perp \mathbf{X}_i$, conditioning on $\mathbf{X}_i$ introduces no additional bias, as there is no systematic correlation between treatment and pre-treatment characteristics to account for.
>
>  See Appendix A for a rigorous exploration of the strict exogeneity and independence assumptions as they relate to experimental designs.

## Testing the Key Identifying Assumption:

Continuing with our example, remember that you did not personally carry out the randomization of treatment---the CEO did. How, then, can you be certain the CEO correctly randomized treatment? Is there a way to empirically test whether $D_i$ is, in fact, randomly assigned and, therefore, the unconditional independence assumption is met? 

Let's take a closer look at the mechanics of unconditional independence to understand how it may be tested ex-post:

In experiments, unconditional independence is predicated on the perfect independence of treatment from both outcomes and all other variables arising from randomization. This is in contrast to conditional independence where treatment is independent of outcomes only after conditioning on the full set of confounding variables $\mathbf{X}_i$ that are jointly correlated with the outcome. This latter case allows for causal inference even in contexts where the assignment of treatment is not perfectly random.

> It's worth clarifying explicitly that the following discussion is in the context of unconditional independence arising from *randomization*. Random assignment provides many unique mechanisms by which the econometrician can isolate causal processes that may not be generalizable to non-random contexts.

Perfect independence of treatment assignment under randomized experiments leads to an important---yet nuanced---property regarding regression specification. In cases where treatment $D_i$ is the **only source of variation** in outcomes between treated and untreated groups, pre-treatment characteristics $\mathbf{X}_i$ need not be explicitly included as covariates in the regression model to achieve unbiased estimation.

This is because the purpose of including covariates in the deterministic component of the regression is typically twofold: 

1. To explain variation in the outcome that is directly attributable to the treatment effect; and 
   
2. To control for confounders that may influence both the treatment assignment $D_i$ and the outcome $Y_i$. 

By doing so, the deterministic component isolates the causal effect of treatment, capturing the difference $\mathbb{E}[Y_i(1) - Y_i(0)]$ while accounting for potential confounding variables $\mathbf{X}_i$ that may jointly determine treatment and the outcome.

However, when $D_i$ is perfectly randomized, it is independent of all other variables, both observed and unobserved. $\mathbf{X}_i$ is thus no longer a set of confounding variables but rather a set of pre-treatment characteristics independent of treatment. Independence through randomization, then, ensures balance between treatment and control groups on all pre-treatment characteristics $\mathbf{X}_i$.

> ### Proof that Randomization Yields Balance of Pre-Treatment Characteristics
>
>To rigorously demonstrate that, under random assignment, the distribution of pre-treatment characteristics $\mathbf{X}_i$ is identical across treatment and control groups. let:
>
>- $D_i \in \{0, 1\}$ denote treatment status: $D_i = 1$ if unit $i$ is treated and $D_i = 0$ if untreated.
>- $\mathbf{X}_i$ denote the vector of pre-treatment characteristics for unit $i$.
>- The sample consist of $n$ units.
>
>**Randomization Assumption:** Letting treatment be assigned randomly, $D_i$ is independent of any pre-treatment characteristics $\mathbf{X}_i$. Formally:
>$$D_i \perp\!\!\!\perp \mathbf{X}_i$$
>
>This implies that the probability of receiving treatment does not depend on any element of $\mathbf{X}_i$:
>
>$$\text{Pr}(D_i \ | \ \mathbf{X}_i) = \text{Pr}(D_i)$$
>
>#### 1. Distribution of Pre-Treatment Characteristics
>By randomization, the treatment assignment $D_i$ is independent of $\mathbf{X}_i$. This independence implies:
>
>$$f(\mathbf{X}_i \mid D_i = 1) = f(\mathbf{X}_i \mid D_i = 0) = f(\mathbf{X}_i)$$
>
>Thus, randomization ensures that the treatment and control groups have the same distribution of pre-treatment characteristics.
>
>#### 2. Expected Value of Pre-Treatment Characteristics
>For any pre-treatment characteristic $X_j \in \mathbf{X}_i$, the expected value in each group is:
>
>$$\mathbb{E}[X_j \mid D_i = 1] = \mathbb{E}[X_j \mid D_i = 0] = \mathbb{E}[X_j]$$
>
>This follows directly from the equality of the conditional distributions $f_{D_i = 1}$ and $f_{D_i = 0}$.
>
>#### 3. Sample Balance
>In practice, econometricians work with finite samples. In a finite context, randomization ensures that the assignment of treatment is unrelated to the characteristics of any unit $i$. Hence, the treated and control groups are random subsamples of the full sample. 
>
>By the Law of Large Numbers (LLN), as the sample size $n \to \infty$, the sample means of $\mathbf{X}_i$ in the treatment and control groups converge to the population mean:
>
>$$\frac{1}{n_T} \sum_{i: D_i = 1} X_{ij} \xrightarrow{p} \mathbb{E}[X_j]$$
>
>$$\frac{1}{n_C} \sum_{i: D_i = 0} X_{ij} \xrightarrow{p} \mathbb{E}[X_j]$$
>where $n_T$ and $n_C$ are the sizes of the treatment and control groups, respectively.
>
> Thus, in large samples, the treated and control groups are balanced on all pre-treatment characteristics $\mathbf{X}_i$. While finite samples may exhibit some imbalance due to random variation, these differences vanish asymptotically. 
> 
> This illuminates why randomization is central to causal inference: it eliminates systematic confounding by design.


Given the balance of pre-treatment characteristics between treatment groups from randomization, the specification

$$Y_i = \alpha + \delta_i D_i + \varepsilon_i$$

provides an unbiased estimate of $\delta_i$ without the need to include any covariates $\mathbf{X}_i$.

> **Note:** While covariates are unnecessary for unbiasedness under these conditions, their inclusion can still reduce residual variance and improve the precision of $\hat{\delta}$, particularly in finite samples. More on this in the section, *Baseline Controls*.

From this, two conditions for unconditional independence under perfect randomization emerge:

1. $D_i$ is the *only* treatment being applied.
2. Pre-treatment characteristics are, on average, *balanced* across the treated and untreated groups.

Condition one can be validated by examining the treatment assignment process to ensure that no other treatments or factors are influencing treatment allocation. This is more of a design specification than a condition to be validated post-experiment, as the randomization process should explicitly prevent the introduction of other treatments.

Condition two, however, can be validated through **balance tests** that compare the means and distributions of observable pre-treatment characteristics $\mathbf{X}_i$ between the treated and control groups. If the treatment and control groups are balanced, we would expect no significant differences in the means or distributions of these characteristics across groups. Such balance would provide evidence in support of the randomization strategy’s validity.

> **Note:** The independence assumption is fundamentally untestable due to the unobservability of certain characteristics, as well as the unobservability of individuals' counterfactual potential outcomes. Balance tests provide evidence for or against independence holding given observable characteristics, but, by the nature of unobservables, they cannot conclusively validate independence in absolute terms. 

## Types of Balance Tests

Balance tests assess whether randomization successfully created comparable groups. They are useful for validating the independence assumptions underlying experimental designs.

> Though, to reiterate, balance tests cannot *prove* (or even *disprove*) that the independence assumption holds given its fundamental untestability. Balance tests merely provide some evidence for or against it holding.

On a basic level, balance tests evaluate whether pre-treatment characteristics $\mathbf{X}_i$ have the same value, on average, across treated and untreated groups:

$$\mathbb{E}[\mathbf{X}_i \ | \ D_i = 1] = \mathbb{E}[\mathbf{X}_i \ | \ D_i = 1]$$

Two common balance tests include:

- **Pairwise Balance Tests**
- **Joint Balance Tests**

### Pairwise Balance Tests
Pairwise Balance Tests tests compare individual covariates between the treatment and control groups. To do this, we run the following regression:

$$ X_{ij} = \theta_0 + \theta_1 D_i + u_i $$

We then perform a **$\mathbf{t}$-test** on the coefficient $\theta_1$ to check if there's a difference in the mean value of a covariate $X_j \in \mathbf{X}_i$ between the treatment and control groups. Under the null hypothesis $H_0: \theta_1 = 0$, balance would hold if $\hat{\theta}_1 = 0$.

> **Note:** Recall that $\alpha$ determines the probability of a Type I error (falsely rejecting the null hypothesis). For example, if $\alpha = 0.05$, we expect to reject $H_0$ despite it is being true 5% of the time. Rejecting the null on a balance test, then, doesn't *necessarily* mean randomization failed; randomization could've, by pure chance, led to an unbalanced distribution between groups.
> 
> However, if we're uncertain of how $D_i$ was assigned, rejecting the null could be a nudge for further scrutiny on the validity of the randomization.

### Joint Balance Tests

Pairwise Balance Tests tests compare multiple covariates $\mathbf{X} = \{X_1, \dots, X_k\}$ between the treatment and control groups. To do this, we run the following regression:

$$ D_i = \gamma_0 + \gamma_1 X_{1i} + \cdots + \gamma_k X_{ki} + u_i $$

In this case, we then perform an ***F*-test** to evaluate whether $\{\gamma_1, \dots, \gamma_k\}$ are *jointly* different from zero. The null hypothesis, then, is written as:

$$H_0 = \gamma_1 = \cdots = \gamma_k = 0$$

This joint balance test follows a similar intuition to that of a single variable balance test in that, if the null holds, balance is assumed to hold. The only difference is that instead of comparing $T \sim t(d)$ we compare $F \sim \mathcal{F(d_1, d_2)}$.

> Despite testing from a different distribution, the earlier note on balance tests failing still holds. Rejecting the null on a balance test doesn't *necessarily* mean randomization failed given Type I error.

## Baseline Controls

The nuance of Type I error in balance tests raises an interesting question: if randomization is done correctly but balance tests fail, does it matter?

On balance tests, Altman (1985) contends: 

> *[When] treatment allocation was properly randomized, a difference of any sort between the two groups...will necessarily be due to chance...performing a significance test to compare baseline variables is to assess the probability of something having occurred by chance when we know that it did occur by chance. Such a procedure is clearly absurd.*

While compelling, it still holds that, even if it's known than an imbalance pre-treatment characteristics is due to chance, it could still bias $\hat{\delta}$. Suppose the workers assigned to the wellness program were already, on average, healthier than those in the control group. This looks like selection bias (even though it was just due to chance), and thus functions as selection bias.

> ### Mechanics of an Balance and Bias
>
>Recall the definition for the OLS estimator $\hat{\delta}$:
>
>$$\hat{\delta} = \frac{\text{Cov}(Y, D)}{\text{Var}(D)}$$
>
>Given that the outcome $Y_i$ is defined generally as:
>
>$$Y_i = Y_i(0) + \delta D_i$$
>
>where: 
>
>- $Y_i = Y_i(0)$ when $D_i = 0$ 
>- $Y_i = Y_i(0) + \delta D_i$ when $D_i = 1$.
>
>we can rewrite: 
>
>$$\text{Cov}(Y, D) = \text{Cov}(Y_i(0) + \delta D_i, D)$$
>
>By linearity of covariance, we can separate and simplify:
>
>$$\begin{align*}
   \text{Cov}(Y_i(0) + \delta D_i, D) &= \text{Cov}(Y_i(0), D_i) + \text{Cov}(\delta D_i, D) \\ 
   &= \text{Cov}(Y_i(0), D_i) + \delta \cdot \text{Var}(D)
\end{align*}$$
>
>Replacing, then, $Y_i$ with this product in the estimator:
>
>$$\begin{align*}
   \hat{\delta} = \frac{\text{Cov}(Y, D)}{\text{Var}(D)} &= \frac{\text{Cov}(Y_i(0), D_i)}{\text{Var}(D)} + \frac{\delta \cdot \text{Var}(D)}{\text{Var}(D)} \\
   &= \delta + \frac{\text{Cov}(Y_i(0), D_i)}{\text{Var}(D)}
\end{align*}$$
>
>where $\frac{\text{Cov}(Y_i(0), D_i)}{\text{Var}(D)}$ is a bias term.
>
>If $\text{Cov}(Y_i(0), D_i) = 0$ (i.e., $D_i \perp\!\!\!\perp Y_i$), the bias term is zero and we recover an unbiased estimate $\hat{\delta}$. If, however, $\text{Cov}(Y_i(0), D_i) \neq 0$, $\hat{\delta}$ is biased. 
>
>Let's understand, then, how imbalance could lead independence of treatment and outcomes to fail:
>
>Where the no-treatment outcome $Y_i(0)$ is a function of both $\mathbf{X}^{\prime}_i \boldsymbol{\gamma}$ and $\varepsilon_i$, we can rewrite:
>
>$$\begin{align*}
   \text{Cov}(Y_i(0), D_i) &= \text{Cov}(\mathbf{X}^{\prime}_i \boldsymbol{\gamma} + \varepsilon_i, D_i) \\
   &= \text{Cov}(\mathbf{X}^{\prime}_i \boldsymbol{\gamma}, D_i) + \text{Cov}(\varepsilon_i, D_i)
\end{align*}$$
>
>Given randomization, conditional on all confounding characteristics being specified in the deterministic component of the regression, $\varepsilon_i \perp\!\!\!\perp D_i$, meaning $\text{Cov}(\varepsilon_i, D_i) = 0$. Therefore:
>
>$$\text{Cov}(Y_i(0), D_i) = \text{Cov}(\mathbf{X}^{\prime}_i \boldsymbol{\gamma}, D_i)$$
>
>We are fundamentally interested, then, in answering whether treatment and the set of pre-treatment characteristics are uncorrelated: 
>
>$$\text{Cov}(\mathbf{X}_i, D_i) = 0$$
>
>Recall that, by randomization, the *assignment* of treatment $D_i$ is independent of $\mathbf{X}_i$, which in expectation yields the same distribution of pre-treatment characteristics across treatment groups:
>
>$$f(\mathbf{X}_i \mid D_i = 1) = f(\mathbf{X}_i \mid D_i = 0) = f(\mathbf{X}_i)$$
>
>Given this balanced distribution of $\mathbf{X}_i$, it follows that the probability of receiving treatment does not depend on any element of $\mathbf{X}_i$:
>
>$$\text{Pr}(D_i \ | \ \mathbf{X}_i) = \text{Pr}(D_i)$$
>
>Or more explicitly:
>
>$$\text{Pr}(D_i = 1 \ | \ \mathbf{X}_i) = \text{Pr}(D_i = 0 \ | \ \mathbf{X}_i) = \text{Pr}(D_i)$$
>
>In this case, $D_i \perp\!\!\!\perp \mathbf{X}_i$ and $\text{Cov}(D_i, \mathbf{X}_i) = 0$
>
>When, by random chance, however, the distribution of $\mathbf{X}_i$ is not equal across groups, the probability of receiving treatment *is* related to the set of pre-treatment characteristics $\mathbf{X}_i$. Given random assignment, these characteristics don't *cause* $D_i$ per se, but they are systematically related by virtue of a given characteristic (or set of characteristics) being disproportionately represented in one group over another. Formally, this means:
>
>$$\text{Pr}(D_i \ | \ \mathbf{X}_i) \neq \text{Pr}(D_i)$$
>
>$\mathbf{X}_i$, then, explains systematic differences across the values of $D_i$ (treatment group), giving:
>
>$$\text{Cov}(D_i, \mathbf{X}_i) \neq 0$$
>
>Given this, imbalance of pre-treatment characteristics across treatment groups yields a biased estimate $\hat{\delta}$:
>
>$$\begin{align*}
   \hat{\delta} &= \delta + \frac{\text{Cov}(Y_i(0), D_i)}{\text{Var}(D)} \\
   &= \delta + \frac{\text{Cov}(\mathbf{X}_i, D_i)}{\text{Var}(D)}
\end{align*}$$
>
> where:
>
> $$\text{Bias}(\hat{\delta}) = \frac{\text{Cov}(\mathbf{X}_i, D_i)}{\text{Var}(D)}$$

Furthermore, imbalance can lead to bias even if balance tests don’t reject.

Altman asserts:

> *[S]tatistical significance is immaterial when considering whether any imbalance between the groups may have affected the results.*

Let's imagine, though, an experiment with a very small sample size (e.g., $N = 10$). In such a case, even a large imbalance might be hard to reject, but still lead to bias.


### Example
The sample covariance between $D_i$ (treatment assignment) and $\mathbf{X}_i$ (a covariate, such as age) is given by:

$$\text{Cov}_\text{sample}(D_i, \mathbf{X}_i) = \frac{1}{n} \sum_{i=1}^n (D_i - \bar{D})(\mathbf{X}_i - \bar{\mathbf{X}})$$

where:
- $\bar{D} = \frac{1}{n} \sum_{i=1}^n D_i$: Sample mean of $D_i$,
- $\bar{\mathbf{X}} = \frac{1}{n} \sum_{i=1}^n \mathbf{X}_i$: Sample mean of $\mathbf{X}_i$.

If $D_i$ is assigned randomly, then:
1. **In expectation**: $\text{Cov}(D_i, \mathbf{X}_i) = 0$ in the population.
2. **In a finite sample**: The realized $D_i$ and $\mathbf{X}_i$ values can differ, causing $\text{Cov}_\text{sample}(D_i, \mathbf{X}_i) \neq 0$.

### Calculation
Suppose we have $n = 10$ individuals, with $D_i$ and $\mathbf{X}_i$ (age) as follows:

| $i$ | $D_i$ | $\mathbf{X}_i$ (Age) |
|--------|----------|------------------------|
| 1      | 1        | 40                     |
| 2      | 1        | 35                     |
| 3      | 1        | 30                     |
| 4      | 1        | 40                     |
| 5      | 1        | 35                     |
| 6      | 0        | 20                     |
| 7      | 0        | 25                     |
| 8      | 0        | 30                     |
| 9      | 0        | 25                     |
| 10     | 0        | 20                     |

#### Step 1: Compute Sample Means
- $\bar{D} = \frac{1}{10} \sum_{i=1}^{10} D_i = \frac{5}{10} = 0.5$,
- $\bar{\mathbf{X}} = \frac{1}{10} \sum_{i=1}^{10} \mathbf{X}_i = \frac{40 + 35 + 30 + 40 + 35 + 20 + 25 + 30 + 25 + 20}{10} = 30.0$.

#### Step 2: Compute Covariance
Using the formula:

$$\text{Cov}_\text{sample}(D_i, \mathbf{X}_i) = \frac{1}{10} \sum_{i=1}^{10} (D_i - \bar{D})(\mathbf{X}_i - \bar{\mathbf{X}})$$

we calculate the terms:

| $i$ | $D_i - \bar{D}$ | $\mathbf{X}_i - \bar{\mathbf{X}}$ | $(D_i - \bar{D})(\mathbf{X}_i - \bar{\mathbf{X}})$ |
|--------|----------------------|-------------------------------------|-----------------------------------------------------|
| 1      | $1 - 0.5 = 0.5$  | $40 - 30 = 10$                 | $0.5 \times 10 = 5$                            |
| 2      | $1 - 0.5 = 0.5$  | $35 - 30 = 5$                  | $0.5 \times 5 = 2.5$                           |
| 3      | $1 - 0.5 = 0.5$  | $30 - 30 = 0$                  | $0.5 \times 0 = 0$                             |
| 4      | $1 - 0.5 = 0.5$  | $40 - 30 = 10$                 | $0.5 \times 10 = 5$                            |
| 5      | $1 - 0.5 = 0.5$  | $35 - 30 = 5$                  | $0.5 \times 5 = 2.5$                           |
| 6      | $0 - 0.5 = -0.5$ | $20 - 30 = -10$                | $-0.5 \times -10 = 5$                          |
| 7      | $0 - 0.5 = -0.5$ | $25 - 30 = -5$                 | $-0.5 \times -5 = 2.5$                         |
| 8      | $0 - 0.5 = -0.5$ | $30 - 30 = 0$                  | $-0.5 \times 0 = 0$                            |
| 9      | $0 - 0.5 = -0.5$ | $25 - 30 = -5$                 | $-0.5 \times -5 = 2.5$                         |
| 10     | $0 - 0.5 = -0.5$ | $20 - 30 = -10$                | $-0.5 \times -10 = 5$                          |

Sum of $(D_i - \bar{D})(\mathbf{X}_i - \bar{\mathbf{X}})$:
$$5 + 2.5 + 0 + 5 + 2.5 + 5 + 2.5 + 0 + 2.5 + 5 = 30$$

Finally:
$$\text{Cov}_\text{sample}(D_i, \mathbf{X}_i) = \frac{1}{10} \times 30 = 3.0$$

Despite random assignment, then the realized **sample covariance** is nonzero ($3.0$), reflecting **imbalance in finite samples**. This creates correlation between $D_i$ (treatment) and $\mathbf{X}_i$ (age), introducing bias if $\mathbf{X}_i$ is omitted in the regression.

Given the withstanding complications of unbalanced distributions of pre-treatment characteristics across treatment groups, econometricians studying experiments commonly estimate equations like:

$$Y_i = \hat{\alpha} + \hat{\delta} D_i + \mathbf{X}^{\prime}_i \hat{\beta} + \hat{\varepsilon}_i$$

where the pre-treatment characteristics $\mathbf{X}_i$ are defined as **baseline controls**.

### Reasons to Include Baseline Controls

Including baseline controls in the regression include:

1. Increased precision of $\hat{\delta}$.
2. Reduce bias in $\hat{\delta}$ from imbalance in $\mathbf{X}_i$.

#### Increase Precision

Suppose $D_i \perp\!\!\!\perp \mathbf{X}_i$ (as is true under randomization) and the distribution of $\mathbf{X}_i$ is balanced across treatment and control groups. Recall that, when including controls:

$$\hat{\delta} = \frac{\text{Cov}(\tilde{D}, Y)}{\text{Var}(\tilde{D})}$$

where $\tilde{D}_i$ is residual variation after controlling for $\mathbf{X}_i$:

$$D_i = \hat{\gamma_0} + \mathbf{X}_i \hat{\boldsymbol{\gamma}} + \tilde{D}_i$$

Because $D_i \perp\!\!\!\perp \mathbf{X}_i$, variation in $\tilde{D}_i = D_i$ since $\mathbf{X}_i$ explains no variation in $D_i$. Therefore:

$$\hat{\delta} = \frac{\text{Cov}(\tilde{D}, Y)}{\text{Var}(\tilde{D})} \approx \frac{\text{Cov}(D, Y)}{\text{Var}(D)} \rightarrow \mathbb{E}[\hat{\delta}] = \delta$$

meaning the average treatment effect is same with or without $\mathbf{X}_i$. If treatment is independent, including baseline controls, then, does not introduce new bias in calculating the treatment effect.

However, including $\mathbf{X}_i$ *can* help reduce $\text{SE}(\hat{\delta})$.

Where $\text{SE}(\hat{\delta}) = \text{Var}[\hat{\delta} \mid \mathbf{X}_i]$, and $\hat{\delta}$ is a function of $Y$, if the covariates $\mathbf{X}_i$ help explain some of the variation in $Y$, then the residual variation (captured by the error term $\varepsilon$) will be reduced. As a result:

$$\text{Var}[\hat{\delta} \mid \mathbf{X}_i] < \text{Var}[\hat{\delta}]$$

This implies that controlling for the variation in the outcome explained by the baseline covariates $\mathbf{X}_i$ reduces the "noisiness" of the estimated treatment effect without introducing additional bias. However, this improvement is contingent upon the assumption that the covariates $\mathbf{X}_i$ actually explain variation in the outcome. If they do not, then $\text{Var}[\hat{\delta} \mid \mathbf{X}_i] = \text{Var}[\hat{\delta}]$.

#### Decrease Bias

Now suppose $D$ is randomly assigned, but imbalance exists in the distribution of $\mathbf{X}_i$ across treatment groups. If we don't control for $\mathbf{X}_i$ by specifying it in the regression, then, our estimated treatment effect will be biased because $\mathbf{X}_i$ explains systematic differences across the values of $D_i$ (i.e., $\text{Cov}(D_i, \mathbf{X}_i) \neq 0$). Formally, this means estimating:

$$\hat{\delta} = \frac{\text{Cov}(Y, D)}{\text{Var}(D)} = \delta + \frac{\text{Cov}(\mathbf{X}_i, D_i)}{\text{Var}(D)}$$

where

$$\text{Bias}(\hat{\delta}) = \frac{\text{Cov}(\mathbf{X}, D)}{\text{Var}(D)}$$

If instead we include $\mathbf{X}_i$ in the specification 

$$Y_i = \alpha + \delta D_i + \mathbf{X}^{\prime}_i \beta + \varepsilon_i$$

we estimate:

$$\hat{\delta} = \frac{\text{Cov}(\tilde{D}, Y)}{\text{Var}(\tilde{D})} \rightarrow \mathbb{E}[\hat{\delta}] = \delta$$


In this case, we interpret $\hat{\delta}$ to be the average change in $Y$ from a one-unit change in $D$ **holding $\mathbf{X}$ fixed**. Or, put another way: *Among units with same value of $\mathbf{X}_i$, what is the difference in $Y_i$ for those with $D_i = 0$ vs. $D_i = 1$?*

Holding $\mathbf{X}_i$ fixed, then, corrects for imbalance, reducing any bias associated with it.

> Note: In any case, better than controlling for $\mathbf{X}_i$ *ex post*, is to randomize treatment in a way that avoids imbalance in $\mathbf{X}_i$ *ex ante*.

### Bad Controls

So far, an underlying assumption driving our discussion of baseline controls is that the controls included are *pre-treatment* characteristics; that is, these are traits observable *before* treatment and, furthermore, before randomization. But why must these controls be pre-treatment? Why can't we include characteristics measures after baseline?

For the sake of example, let $X_{ij}$ be worker $i$’s cholesterol level *after* they were assigned treatment $D_i$. Our goal is still to know how $D_i$ affects $Y_i$, and $X_{ij}$ may help increase the precision of our estimated treatment effect $\hat{\delta}$ by helping explain variation in sick days.

At first glance, $X_{ij}$ may seem a good control, but the fact we are measuring cholesterol levels *after* they were assigned treatment is problematic since cholesterol levels may themselves be affected by a workplace wellness program ($D_i$). In such a case, $X_{ij}$ should be on the left-hand side as an outcome. Including it in the specification as a regressor would mean $D_i$ is no longer independent of $X_{ij}$ since the effect of $D_i$ on the $X_{ij}$ implies $\text{Cov}(D_i, X_{ij}) \neq 0$. This leads to a biased estimate:

$$\hat{\delta} = \delta + \frac{\text{Cov}(D_i, X_{ij})}{\text{Var}(D)}$$

meaning $\mathbb{E}[\hat{\delta}] \neq \delta_{ATE}$. 

Because the inclusion of $X_{ij}$ leads to bias, we refer to it as a **bad control**. Baseline controls, then, must capture *pre*-treatment characteristics so as to avoid bias.

Furthermore, to this idea of data collection timing, suppose your data $\{Y_i, D_i\}^n_{i=1}$ is obtained from the firm one year *after* the program is implemented; i.e., your data is from all workers employed at the firm at end of the year *after* randomization and assignment of treatment. 

Assuming perfect randomization, the earlier point stands to reason that, with a lag of one year, a lack pre-treatment baseline data could make validating independence challenging. But to a subtler point, collecting data so far removed from treatment could lead to attrition bias---especially given the health-related experimental context. The data only includes workers remaining with the firm at the end of the year. This means that workers who left the company during the year, be it voluntarily or involuntarily, are excluded from the analysis. The issue is that workers who left before the end of the year might systematically differ in important ways from those who stay. If workers who didn't participate in the wellness program had worse health outcomes and left the firm as a result, the remaining units in the comparison group would likely show better outcomes than what actually occurred. This leads to a distorted estimate of the program's effect, as the sample is no longer representative of the original population.

This phenomenon is what is referred to as **attrition bias**: a type of selection bias where participants drop out or are lost from a study over time, leading to a sample that is no longer representative of the original population. This can result in biased estimates of the treatment effect.

The key extract from this, then, is that measurement plays an instrumental role in treatment effect estimation, and timing of data collection raises important considerations for the recovery of unbiased treatment effect estimates.x

## Appendix

### A. Strict Exogeneity & Independence

Let us illustrate the mechanics of exogeneity and independence first through the lens of observational studies to understand the logic underpinning randomized experiments.

Where we are estimating the causal effects of treatment through regression, recall the population regression model:

$$Y_i(D) = \alpha + \delta D_i + \mathbf{X}^{\prime}_i \beta + \varepsilon_i$$

where $D \in \{0, 1\}$ is a treatment indicator, $\mathbf{X}$ is a set of observable pre-treatment characteristics, and the $\varepsilon$ is an error term capturing unobservable characteristics affecting the outcome $Y_i(D)$. 

Under strict exogeneity (unconfoundedness), $\mathbb{E}[\varepsilon_i \ | \ \mathbf{X}] = 0$. This means the error term, uncorrelated with all observed covariates $X$, captures only unobservable characteristics, which ensures no omitted variable bias (OVB) as no $X_i^ {omitted} \in \varepsilon_i$. This also means $\mathbf{X}_i$ and $\varepsilon_i$ capture the full sets of observable and unobservable characteristics affecting potential outcomes respectively.

>### Proof that $\mathbb{E}[\varepsilon_i \ | \ \mathbf{X}] = 0 \leftrightarrow \text{Cov}(\varepsilon_i, X_j) \ \forall i, j$
>
>#### Definitions
>1. **Covariance**:
>
>$$\begin{align*}
   \text{Cov}(\varepsilon_i, X_j) &= \mathbb{E}[\varepsilon_i] - \mathbb{E}[\varepsilon_i X_j] - \mathbb{E}[X_j] \\
   &= \mathbb{E}[\varepsilon_i X_j] - \mathbb{E}[\varepsilon_i] \ \mathbb{E}[X_j]
\end{align*}$$
>
> 2. **Strict Exogeneity**:
>
>$$\mathbb{E}[\varepsilon_i \mid X] = 0$$
>
>This means $\varepsilon_i$ is mean-independent of $X$, so $\mathbb{E}[\varepsilon_i]$ is constant and $\mathbb{E}[\varepsilon_i \mid X]$ depends on no elements of $X$.
>
>#### Step 1: Expand $\mathbb{E}[\varepsilon_i X_j]$
>
>By the **law of total expectation**:
>
>$$\mathbb{E}[\varepsilon_i X_j] = \mathbb{E}[\mathbb{E}[\varepsilon_i X_j \mid X]]$$
>
>Using the **properties of conditional expectation**, $X_j$ is treated as known when conditioned on $X$:
>
>$$\mathbb{E}[\varepsilon_i X_j \mid X] = X_j \mathbb{E}[\varepsilon_i \mid X]$$
>
>Substituting $\mathbb{E}[\varepsilon_i \mid X] = 0$:
>
>$$\mathbb{E}[\varepsilon_i X_j \mid X] = X_j \cdot 0 = 0$$
>
>Thus:
>
>$$\mathbb{E}[\varepsilon_i X_j] = \mathbb{E}[0] = 0.$$
>
>#### Step 2: Expand $\mathbb{E}[\varepsilon_i] \cdot \mathbb{E}[X_j]$
>
>The marginal expectation $\mathbb{E}[\varepsilon_i]$ can be written as:
>
>$$\mathbb{E}[\varepsilon_i] = \mathbb{E}[\mathbb{E}[\varepsilon_i \mid X]]$$
>
>Since $\mathbb{E}[\varepsilon_i \mid X] = 0$:
>
>$$\mathbb{E}[\varepsilon_i] = \mathbb{E}[0] = 0$$
>
>Thus:
>
>$$\mathbb{E}[\varepsilon_i] \cdot \mathbb{E}[X_j] = 0 \cdot \mathbb{E}[X_j] = 0$$
>
>#### Step 3: Combine Results
>
>Using the results from Steps 1 and 2:
>
>$$\text{Cov}(\varepsilon_i, X_j) = \mathbb{E}[\varepsilon_i X_j] - \mathbb{E}[\varepsilon_i]\mathbb{E}[X_j]$$
>
>Substituting:
>
>$$\text{Cov}(\varepsilon_i, X_j) = 0 - 0 = 0$$

Expressing the population regression model, then, as a conditional expectation, we have:

$$\mathbb{E}[Y_i(D) \ | \ \mathbf{X}_i] = \alpha + \delta D_i + \mathbf{X}^{\prime}_i \beta$$

where the conditional mean error is zero and outcomes are conditioned on observable pre-treatment outcomes. Furthermore, $\delta = \mathbb{E}[Y_i(1) - Y_i(0)]$, meaning that, in an experimental context, the treatment effect is a simple difference of average outcomes between the treated and untreated. 

Let's take a moment, though, to expand on *why* outcomes are conditioned solely on observed pre-treatment outcomes. 

Under the potential outcomes framework, an individual $i$'s potential outcomes $Y_i(0), Y_i(1)$ are established endogenously *before* any treatment event. Treatment, then, can't cause potential outcomes because potential outcomes are established prior to treatment. Before the intervention of a workplace wellness program, the number of sick days a worker takes can only be determined by pre-treatment characteristics because treatment hasn't been applied. Treatment doesn't create a new potential outcome; instead, it realizes one potential outcome over the other depending on if the individual is treated or not. 

To specify why we only condition on $\mathbf{X}_i$, recall that, under exogeneity, unobserved characteristics $\varepsilon_i$ have no predictive power in determining $Y_i(D)$ (that's why it drops out of the conditional expectation regression model). If exogeneity holds, all variables that yield an association between the potential outcomes and treatment are included in $\mathbf{X}_i$, meaning we have observed all $X$ needed to eliminate confounding and $\varepsilon_i$ yields no further predictive power for determining $Y_i(D)$. Potential outcomes, then, can only be determined by observed pre-treatment covariates $\mathbf{X}_i$ so long as the error term is uncorrelated with any observed covariate ($D_i$ and $\mathbf{X}_i$). 

Together, the potential outcomes framework and exogeneity conditions illuminate the underlying logic by which experimental treatments reveal causal effects. The last element to consider, however, is the treatment assignment mechanism. For an assignment mechanism to be exogenous (unconfounded), it must be independent of potential outcomes:

$$f_{D|Y(D),X}(d|y_0, y_1, x, \varepsilon) = f_{D|X}(d|x, \varepsilon), \quad \forall y_0, y_1, x, \varepsilon, \text{ and } d \in \{0,1\}$$

Or otherwise put:

$$D_i \perp\!\!\!\perp Y_i(D)$$

In a potential outcomes framework, because treatment itself doesn't *cause* potential outcomes (i.e., it has no predictive power), we can assert $D_i \perp\!\!\!\perp Y_i(D)$ if and only if the assignment of treatment is not itself informed by endogenous characteristics (i.e., if $D_i$ is uncorrelated with observed and unobserved pre-treatment characteristics $\mathbf{X}_i$ and $\varepsilon_i$). Under random assignment, however, the exogeneity of $D$ derives from the very nature of randomness. Random assignment, then, ensures: 

$$D_i \perp\!\!\!\perp \mathbf{X}_i, \varepsilon_i$$ 

If we hold the strict exogeneity assumption (unconfoundedness) assumption to be true, this also gives:

$$\varepsilon_i \perp\!\!\!\perp D_i, \mathbf{X}_i$$

given $\varepsilon_i \perp\!\!\!\perp D_i$ under randomness and $\varepsilon_i \perp\!\!\!\perp \mathbf{X}_i$ under the exogeneity assumption.

> In a context without randomization, if the same covariates $\mathbf{X}$ that cause potential outcomes cause assignment of treatment (selection), there is an association---a dependence---between treatment assignment and the potential outcomes. This is what leads to selection bias.

So, given randomness, we have an exogenous $D_i$ uncorrelated with both the vector of covariates $\mathbf{X}_i$ and the error $\varepsilon_i$. The assignment of treatment ($D$), then, having no predictive power of its own, only has an effect on $Y_i(D)$ through characteristics $\mathbf{X}$. Or, put another way, the fact a treated individual was randomly assigned treatment didn't change the 'value' of their outcome $Y_i$, it merely put them into a scenario where their observable characteristics produced their observed outcome $Y_i(D)$. "Conditioning"---or accounting for---the effect of $\mathbf{X}$ on $Y_i({D})$---then, means the assignment of treatment is conditionally independent of potential outcomes, given by:

$$D_i \perp\!\!\!\perp Y_i(0), Y_i(1) \ | \ \mathbf{X}_i$$

following the Dawid (1979) conditional independence notation. This is what is known as the conditional independence assumption.

With this established, let's return to our initial population model:

$$Y_i(D) = \alpha + \delta D_i + \mathbf{X}^{\prime}_i \beta + \varepsilon_i$$

For causal inference, we can express this model for both potential outcomes:

$$Y_i(0) = \alpha + \mathbf{X}_i \beta + \varepsilon_i \quad \text{and} \quad Y_i(1) = Y_i(0) + \delta$$

To evaluate the exogeneity of treatment, we want to prove:

$$\text{Pr}(D_i = d \ | \ Y_i(0), Y_i(1), \mathbf{X}_i) = \text{Pr}(D_i \ | \ \mathbf{X}_i)$$

I.e., if we prove that treatment is conditionally independent from all potential outcomes, $D_i \perp\!\!\!\perp Y_i(0), Y_i(1) \ | \ \mathbf{X}_i$, the exogeneity assumption for treatment holds.

Because $\varepsilon_i$ is a function of $Y_i(0)$ and $Y_i(1)$ is itself a function of $Y_i(0)$, we can assert:

$$\text{Pr}(D_i = d \ | \ Y_i(0), Y_i(1), \mathbf{X}_i) = \text{Pr}(D_i \ | \ \varepsilon_i, \mathbf{X}_i)$$

This is akin to saying that the probability of being assigned to treatment (or not) is contingent solely on the observed and unobserved characteristics of an inividual $i$. This doesn't prove exogeneity yet given $\varepsilon_i = f(Y_i(D))$; this just rearranges the expression into a form where we're explicitly conditioning on the stochastic and deterministic components that determine $Y_i(D)$. For exogeneity to hold, it must be true that $D_i \perp\!\!\!\perp \varepsilon_i, \mathbf{X}_i$.

But as we established earlier, valid random assignment of treatment guarantees $D_i \perp\!\!\!\perp \varepsilon_i, \mathbf{X}_i$ and gives $D_i \perp\!\!\!\perp Y_i(0), Y_i(1) \ | \ \mathbf{X}_i$. With $D_i$ being an exogenous regressor, it follows:

$$\text{Pr}(D_i \ | \ \varepsilon_i, \mathbf{X}_i) = \text{Pr}(D_i \ | \ \mathbf{X}_i)$$

Because random assignment ensures treatment is independent of outcomes conditioning on $\mathbf{X}_i$, $D_i$ is exogenous and $\hat{\delta}$ is an unbiased estimate of the treatment effect under a randomized experiment.