# Potential Outcomes Framework

## Overview

The Potential Outcomes Framework is one of the most widely used frameworks for understanding causal inference. It conceptualizes the causal effect as the difference between the outcomes we would observe if a unit received the treatment and the outcome we would observe if the unit did not receive the treatment. This framework highlights fundamental challenges and techniques in causal inference, such as defining causal effects, dealing with selection bias, and accounting for endogeneity.

## Definitions

- **Potential Outcomes**: The outcomes that could be observed for each unit (e.g., individual, group) under each treatment condition. Denoted as $Y(1)$ for the treated condition and $Y(0)$ for the untreated condition.

- **Counterfactual**: The unrealized outcome that would have occurred for an individual or unit if they had received a different treatment or intervention than the one they actually received. 

- **Causal Effect**: The difference in potential outcomes for an individual or unit due to treatment. 

- **Average Treatment Effect (ATE)**: The average difference in potential outcomes between the treated and untreated groups across the population.

- **Average Treatment Effect on the Treated (ATT)**: The average causal effect of treatment for those who actually received the treatment.

- **Average Treatment Effect on the Untreated (ATU)**: The average causal effect of treatment for those who did not receive the treatment.

- **Heterogeneous Treatment Effect Bias**: The bias that arises when the treatment effect varies across different subgroups in the population, making it difficult to estimate a single average treatment effect accurately.

- **Endogeneity**: A situation in which something inside the system being studied interferes with the ability to isolate the true effect of one variable on another

- **Selection Bias**: The bias introduced when the selection of individuals into treatment and control groups is not random, leading to systematic differences in observed outcomes that are not due to the treatment itself.

- **Independence Assumption**: The assumption that the treatment assignment is independent of potential outcomes. 

- **Stable Unit Treatment Value Assumption (SUTVA)**: The assumption that the potential outcomes for any unit do not depend on the treatment assignment of other units. 

## Potential Outcomes

The **potential outcomes** framework is rooted in the idea that for every individual $i$, there are two potential states of the world:

- $Y_i(1)$: The outcome if the individual receives the treatment ($D_i = 1$).
- $Y_i(0)$: The outcome if the individual does not receive the treatment ($D_i = 0$).

We refer to these as "potential" outcomes because, in any experiment or observational study, we can only observe one of these outcomes for each individual—either the outcome when treated or when untreated, but not both. This leads to the **fundamental problem of causal inference**, which limits our ability to directly measure individual-level causal effects.

## Fundamental Problem of Causal Inference

The **fundamental problem of causal inference** arises because we cannot observe both $Y_i(1)$ and $Y_i(0)$ for the same individual. We only observe:

- $Y_i(1)$ if the individual is treated ($D_i = 1$),
- $Y_i(0)$ if the individual is untreated ($D_i = 0$).

Whichever potential outcome is unrealized is the **counterfactual**, or the unrealized alternate state (outcome). 

## Causal Effects

The **causal effect** for an individual $i$ is defined as the difference between their potential outcomes:

$$
\delta_i = Y_i(1) - Y_i(0)
$$

This difference represents the effect of receiving the treatment ($D = 1$) compared to not receiving the treatment ($D = 0$) for that individual.

- **If $\delta_i > 0$**: The treatment had a **positive effect** for individual $i$. This means that the outcome for the treated scenario ($Y_i(1)$) is higher than the outcome for the untreated scenario ($Y_i(0)$).

- **If $\delta_i = 0$**: The treatment had **no effect** for individual $i$. In this case, the outcome for the treated scenario is the same as the outcome for the untreated scenario ($Y_i(1) = Y_i(0)$). This implies that the treatment did not change individual $i$'s outcome in any way.

- **If $\delta_i < 0$**: The treatment had a **negative effect** for individual $i$. Here, the outcome for the treated scenario ($Y_i(1)$) is lower than the outcome for the untreated scenario ($Y_i(0)$).

This is a sharp causal effect, but it is fundamentally unobservable because we can never see both $Y_i(1)$ and $Y_i(0)$ for the same individual at the same time. The challenge in causal inference is to estimate average causal effects using observed data.

### Average Causal Effects

Given the inability to observe individual causal effects, we turn to **average causal effects**, which summarize the causal effect over a population:

#### Average Treatment Effect (ATE): 

The average causal effect of the treatment on the entire population is given by:

$$
\begin{align*}
    \delta_{ATE} &= E[Y_i(1) - Y_i(0)] \\
               &= E[\delta_i]
\end{align*}
$$

This represents the expected difference between outcomes under treatment and control, averaged across all individuals in the population.

We can also represent the average treatment effect as the sum of the **average treatment effect on the treated** scaled by the share of units treated and the **average treatment effect on the untreated** scaled by the share of units untreated:

$$\delta_{ATE} = \pi \delta_{ATT} + (1 - \pi) \delta_{ATU}$$

Where: 

- $\pi = P(D_i = 1)$ is the proportion of treated units.
- $\delta_{ATt}$ is the average treatment effect on the treated.
- $\delta_{ATU}$ is the average treatment effect on the untreated.

#### Average Treatment Effect on the Treated (ATT): 

The average effect of the treatment among those who received it:

$$
\begin{align*}
    \delta_{ATT} &= E[Y_i(1) - Y_i(0) \mid D_i = 1] \\
               &= E[\delta_i \mid D_i = 1]
\end{align*}
$$

This focuses specifically on the group that received the treatment, which is often the group of primary interest in policy evaluation or impact assessments.

#### Average Treatment Effect on the Untreated (ATU): 

The average effect of the treatment on those who did not receive it:

$$
\begin{align*}
    \delta_{ATU} &= E[Y_i(1) - Y_i(0) \mid D_i = 0] \\
           &= E[\delta_i \mid D_i = 0]
\end{align*}
$$

> **Preview:** The difference between $ATE$, $ATT$, and $ATU$ is important because treatment effects can be heterogeneous across populations (we will discuss this later).

### Simple Difference in Outcomes

In actuality, we never observe the full set of $(D_i, Y_i(1), Y_i(0), \delta_i )$ for every individual. Instead, we only observe the subset $(D_i, Y_i)$ where $Y_i$ corresponds to their treatment group (their respective $D_i$).

For this reason, the closest estimation of causal effects ($Y_i(1) - Y_i(0)$) we have is a **simple difference in outcomes (SDO)**:

$$
\begin{align*}
    \delta_{SDO} &= E[Y_i(1) \mid D_i = 1] - E[Y_i(0) \mid D_i = 0] \\
                 &= E[Y_i \mid D_i = 1] - E[Y_i \mid D_i = 0]
\end{align*}
$$

**However**, this SDO is a *biased* estimator of causal effects because it captures both **selection bias** and **heterogenous treatment effect bias**.

## Selection Bias

**Selection bias** occurs when the treated and control groups differ systematically in ways that affect the outcome, other than the treatment itself. Selection bias arises when variation in treatment ($D_i$) is **endogenous**, or not assigned by chance. Instead, individuals select themselves into treatment by non-random forces (e.g., to maximize their own utility).

Selection is an issue because individuals who choose to receive a treatment may have characteristics that also make them more likely to have certain outcomes, regardless of the treatment. 

Selection bias is defined as

$$
\text{Selection Bias} = E[Y_i(0) \mid D_i = 1] - E[Y_i(0) \mid D_i = 0]
$$

because the untreated potential outcome ($Y_i(0)$) for treated individuals ($D_i = 1$) may differ from the untreated potential outcome for untreated individuals ($D_i = 0$), even if the treatment had no effect.

> ### Decomposing SDO: Selection Bias
>
> We can extract selection bias from the simple difference in outcomes by first adding and subtracting the counterfactual for the treated ($E[Y_i(0) \mid D_i = 1]$) then consolidating terms:
>
> $$
\begin{align*}
    \delta_{SDO} &= E[Y_i(1) \mid D_i = 1] - E[Y_i(0) \mid D_i = 0] \\
                 &= E[Y_i(1) \mid D_i = 1] \ \underbrace{- E[Y_i(0) \mid D_i = 1] + E[Y_i(1) \mid D_i = 1]}_{\text{Add/Subtract Counterfactual for Treated}} - E[Y_i(0) \mid D_i = 0] \\
                 &= \underbrace{E[Y_i(1) \mid D_i = 1] - E[Y_i(0) \mid D_i = 1]}_{\text{ATT}} + E[Y_i(1) \mid D_i = 1] - E[Y_i(0) \mid D_i = 0] \\
                 &= \delta_{ATT} + \underbrace{E[Y_i(0) \mid D_i = 1] - E[Y_i(0) \mid D_i = 0]}_{\text{Selection Bias}}
\end{align*}$$

### Positive (Beneficial) Selection

**Positive selection** occurs when individuals who are more likely to have a favorable outcome are more likely to be included in the treatment group. In other words, the treated group tends to have higher baseline levels of the outcome variable compared to the control group.

$$
\delta_{SDO} > \delta_{ATT} \mid \delta_{SDO} = \delta_{ATT} + \text{Selection Bias}
$$

For example, if only high-performing employees are chosen to receive a particular training, they may show better productivity increases simply due to their initial high performance, not necessarily because of the training itself. This creates a bias toward ***overestimating*** the effectiveness of the training program.

### Negative (Adverse) Selection

**Negative selection** occurs when individuals who are less likely to have a favorable outcome are more likely to be included in the treatment group. This means the treated group tends to have lower baseline levels of the outcome variable compared to the control group.

$$
\delta_{SDO} < \delta_{ATT} \mid \delta_{SDO} = \delta_{ATT} + \text{Selection Bias}
$$

For example, if individuals with high initial weight are more likely to receive a medical intervention, the observed effect may be underestimated because these individuals might have less potential for weight loss due to their starting condition. This creates a bias toward underestimating the effectiveness of the intervention.

> **Note:** In the cases above, the formulas are assuming that larger values of $\delta$ are the favorable scenario (i.e., if $\delta_1 > \delta_0$, then $\delta_1 \succ \delta_0$). 
> 
> If instead smaller values of $\delta$ are favorable ($\delta_1 \prec \delta_0$), we flip the signs for positive and negative selection. Positive selection, in that case, would be when $\delta_{SDO} < \delta_{ATT}$ and negative selection would be when $\delta_{SDO} > \delta_{ATT}$.

## Heterogeneous Treatment Effects and Treatment Effect Bias

In many cases, the treatment effect is not homogeneous across individuals. **Heterogeneous treatment effects** mean that different individuals or subgroups of individuals may experience different causal effects ($\delta_i$) from the same treatment. 

> **Note:** If $\delta_i = \delta \ \forall i$ (i.e., **homogenous treatment effects**), then $\delta_{ATE} = \delta_{ATT} = \delta_{ATU}$.

Ignoring this heterogeneity can lead to **treatment effect bias**:

$$
\text{HTE Bias} = (1-\pi)(\delta_{ATT} - \delta_{ATU})
$$

> ### Decomposing SDO: Heterogenous Treatment Effect Bias
>
> We can extract heterogenous treatment effect (HTE) bias *in addition to* selection bias from the simple difference in outcomes. 
>
> We've already decomposed SDO to be:
>
> $$
\delta_{SDO} = \delta_{ATT} + \text{Selection Bias}$$
>
> Now recall the formulas for $\delta_{ATE}$:
> 
> $$
\begin{align*}
    \delta_{ATE} &= E[Y_i(1) - Y_i(0)] \\
                 &= \pi \delta_{ATT} + (1 - \pi) \delta_{ATU}
\end{align*}$$
>
> To extract the HTE bias, let's take the previously decomposed SDO and add and subtract the $\delta_{ATE}$, rearrange terms, substitute in the formulas for ATE, then simplify:
>
> $$
\begin{align*}
    \delta_{SDO} &= \delta_{ATT} \ \underbrace{+ \delta_{ATE} - \delta_{ATE}}_{\text{Add/Subtract ATE}} + \text{Selection Bias} \\
                 &= \delta_{ATE} + [-\delta_{ATE} + \delta_{ATT}] + \cdots\\
                 &= \delta_{ATE} + [-(\pi\delta_{ATT} + (1-\pi)\delta_{ATU}) + \delta_{ATT}] + \cdots\\
                 &= \delta_{ATE} + [-\pi\delta_{ATT} - (1-\pi)\delta_{ATU} + \delta_{ATT}] + \cdots\\ 
                 &= \delta_{ATE} + [(\delta_{ATT} - \delta_{ATU})-(\pi\delta_{ATT} - \pi\delta_{ATU})] + \cdots\\ 
                 &= \delta_{ATE} + \underbrace{(1-\pi)(\delta_{ATT} - \delta_{ATU})}_{\text{HTE Bias}} + \text{Selection Bias}
\end{align*}$$ 
>
> Meaning:
>
> $$
\begin{align*}
    \delta_{SDO} &= \delta_{ATE} + \text{Selection Bias} + \text{HTE Bias} \\
                 &= \delta_{ATE} + \underbrace{E[Y_i(0) \mid D_i = 1] - E[Y_i(0) \mid D_i = 0]}_{\text{Selection Bias}} \\
                 & \qquad \quad \  + \underbrace{(1-\pi)(\delta_{ATT} - \delta_{ATU})}_{\text{HTE Bias}}
\end{align*}$$

When treatment effect heterogeneity is unaccounted for, the estimated average treatment effect may not reflect the true effect for any particular subgroup. For example, the effect of a job training program might be larger for younger participants compared to older participants, or for individuals with lower initial skill levels.

> **Interaction terms** or **subgroup analysis** can help address heterogeneity by identifying groups for which the treatment effect may differ.

## Independence Assumption

The **independence assumption** is critical for identifying causal effects in observational studies. It states that the potential outcomes are independent of treatment assignment:

$$
(Y_i(0), Y_i(1)) \perp D_i
$$

This doesn't mean that treatment ($D_i$) doesn’t affect outcomes ($Y_i$). It just means that how treatment is assigned is unrelated to what the outcomes would be with or without that assignment mechanism.

**If treatment is independent of potential outcomes**, there is no selection bias, meaning:

$$
E[Y_i(1) \mid D_i = 1] = E[Y_i(1) \mid D_i = 0] = E[Y_i(1)] \\
E[Y_i(0) \mid D_i = 1] = E[Y_i(0) \mid D_i = 0] = E[Y_i(0)]
$$

Thus, 

$$\delta_{SDO} = \delta_{ATE} = \delta_{ATT} = \delta_{ATU}$$

> **Note:** Randomness ***guarantees*** independence. Treatment, then, is independence of potential outcomes if it is assigned randomly.

## Stable Unit Treatment Value Assumption (SUTVA)

The **Stable Unit Treatment Value Assumption (SUTVA)** is a key assumption in the Potential Outcomes Framework. SUTVA has two main components:

1. **No hidden variations of treatment**: The treatment is assumed to be applied uniformly to all treated units, meaning there are no unobserved differences in how the treatment is administered across units. In other words, treatment is homogenous and $D_i$ means the same thing for everyone.

2. **No interference between units**: The treatment assigned to one unit does not affect the potential outcomes of another unit. In essence, this means **no externalities or spillover effects**.

Violations of SUTVA can occur in situations where individuals' outcomes are affected by the treatment status of others (e.g., network effects) or where the treatment is applied inconsistently.