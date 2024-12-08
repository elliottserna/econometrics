# Regression: Overview of Linear Regression

## Definitions:

**Linear Regression:** A statistical method used to analyze the relationship between variables in data.

**Dependent Variable (Response Variable):** The variable we aim to predict or explain, typically denoted as $Y$.

**Independent Variables (Predictors):** One or more variables that are used to predict the value of the dependent variable, typically denoted as $X_1, X_2, \ldots, X_n$.

## Linear Regression

Linear regression serves as a tool for understanding how changes in independent variables affect the dependent variable. It provides insights into the nature of relationships within data.

Both regression and correlation assess the relationship between variables, telling us how they move together, but they differ fundamentally:

- **Correlation** quantifies the strength and direction of a linear relationship between two variables, but it does not account for the causal direction or the influence of other variables.
- **Regression** not only quantifies the relationship but also allows for predictions of the dependent variable based on the independent variables.

> **Note:** While regression can indicate relationships, it does not inherently establish causality. To understand why variables are related, additional assumptions and context are needed. Regression analysis should be complemented by theoretical understanding and appropriate study design to make causal inferences.

## Linearity

The keyword in "linear regression" is *linear*. If you've taken linear algebra, you know just how powerful linear forms can be in applications ranging from finding relationships between sets of variables to extrapolating causal inference. And even if you haven't, you have more than likely encountered the power of lines when you search google and get ordered search results, or when you open the weather app and see the weekly weather forecast.

Much of what we will explore in this section on linear regression is predicated on **linearity**, the specific type of relationship between variables in math where a function of the variables follows a straight line when graphed on a coordinate plane.

> **Mathematical Definition of Linearity:**
>
>In mathematics, a function $f: \mathbb{R}^n \to \mathbb{R}$ is said to be **linear** if it satisfies the following two properties for all vectors $\mathbf{x}, \mathbf{y} \in \mathbb{R}^n$ and all scalars $c \in \mathbb{R}$:
>
>1. **Additivity**: 
>   $$
   f(\mathbf{x} + \mathbf{y}) = f(\mathbf{x}) + f(\mathbf{y})$$
>
> 2. **Homogeneity (or Scalar Multiplication)**: 
>   $$
   f(c \cdot \mathbf{x}) = c \cdot f(\mathbf{x})$$

In the context of linear regression, linearity means that we are trying to express the relationship between a dependent variable (what we want to predict) and one or more independent variables (the predictors) in a way that fits a straight line.

### Linear Forms

If you recall middle school algebra, you recall the equation of a line:

$$
y=mx+b
$$

Where:

- $y$ is the dependent variable.
- $x$ is the independent variable.
- $m$ is the slope of the line.
- $b$ is the $y$-intercept constant.

Riveting stuff, really. 

But, in the context of regressions, that equation is a linear regression in its most basic expression. Compare the equation of a line to the basic form of a bivariate (two variable) linear regression:

$$
Y_i = \alpha + \beta X_{i} + \varepsilon_i
$$

Where:

- $Y_i$ is the dependent variable for the $i^{th}$ observation (e.g., person).
- $X_i$ is the independent variable for the $i^{th}$ observation.
- $\beta$ is the slope coefficient on the independent variable.
- $\alpha$ is the baseline constant.
- $\varepsilon_i$ is the error term for the $i^{th}$ observation.

More on this will explored in subsequent sections, but even with just this it's easy to see that a linear regression is just $y = mx + b$ with fancy notation.

So when you see a regression specification



$$
\begin{align*}
Y_{ijt} = & \alpha + \beta_1 T_{1it} + \beta_2 T_{2it} + \mathbf{X}^\prime \delta \\
          & + \sum_d (\gamma_d \cdot N_{dit}^T) + \sum_d (\phi_d \cdot N_{dit}) + u_i + \varepsilon_{ijt}
\end{align*}
$$

Just remember that, at its core, this is just a fleshed out version of $y = mx + b$.

>### For the Curious:
>
> The above specification is from Miguel and Kremer's 2004 paper, "Worms: Identifying impacts on education and health in the presence of treatment externalities" on the effects deworming treatments have on education outcomes in Kenya. 
>
> In the study, schools were randomly assigned to different waves of treatment, allowing the researchers to compare schools that received treatment in earlier waves to schools that had not yet received treatment in that wave as a randomized control trial (RCT). 
>
> Breaking down the regression, we have:
>
> $$
Y_{ijt} = \alpha + 
\underbrace{\beta_1 T_{1it} + \beta_2 T_{2it}}_{\text{Treatment Effects}} + 
\underbrace{\mathbf{X}^\prime \delta}_{\text{Covariates}} +$$
>
> $$
\underbrace{\sum_d (\gamma_d \cdot N_{dit}^T)}_{\text{Dummies/Interactions (Type 1)}} + 
\underbrace{\sum_d (\phi_d \cdot N_{dit})}_{\text{Dummies/Interactions (Type 2)}} + $$
>
> $$
\underbrace{u_i}_{\text{Individual-Specific Effects}} + 
\underbrace{\varepsilon_{ijt}}_{\text{Idiosyncratic Error Term}}$$
>
> Where:
>
> **Overall Model**: 
> - $Y_{ijt}$: The dependent variable representing the individual health or education outcome for student $j$ in school $i$ during year $t$.
>
> **Treatment Effects**: 
> - $T_{1it}$: An indicator variable that equals 1 if school $i$ is assigned to the first year of deworming treatment in year $t$ and 0 otherwise. This variable captures the effect of the first treatment year.
>  
> - $T_{2it}$: An indicator variable that equals 1 if school $i$ is assigned to the second year of deworming treatment in year $t$ and 0 otherwise. This variable captures the effect of the second treatment year.
>
> **Covariates**: 
> - $\mathbf{X}^\prime$: A vector of additional school and pupil characteristics that may influence the health or education outcomes. These could include factors such as socioeconomic status, prior health metrics, and school resources.
>
> - $\delta$: Coefficients associated with the covariates $\mathbf{X}^\prime$, indicating the extent to which changes in these characteristics affect the outcome $Y_{ijt}$.
>
> **Dummies/Interactions (Type 1)**: 
> - $N_{dit}^T$: The total number of pupils in primary schools within distance $d$ from school $i$ in year $t$, where schools are randomly assigned to deworming treatment. This variable accounts for the local population density of treated schools that might affect treatment outcomes.
>
> - $\gamma_d$: Coefficients for the $N_{dit}^T$ terms, measuring the impact of the number of treated pupils at distance $d$ on the health or education outcome.
>
> **Dummies/Interactions (Type 2)**: 
> - $N_{dit}$: The total number of pupils in primary schools at distance $d$ from school $i$ in year $t$, regardless of treatment assignment. This variable reflects the overall local population density.
>
> - $\phi_d$: Coefficients for the $N_{dit}$ terms, capturing the effect of local school density on the health or education outcomes.
>
> **Individual-Specific Effects**: 
> - $u_i$: Unobserved individual-specific effects that account for unmeasured factors influencing $Y_{ijt}$ for each student $j$ in school $i$. This term captures the fixed characteristics of schools that might affect outcomes, which are not directly included in the model.
>
> **Idiosyncratic Error Term**: 
> - $\varepsilon_{ijt}$: The error term specific to individual $j$ in school $i$ during year $t$, capturing random variations in the outcome that are not explained by the model. This includes measurement error and other unobserved influences on $Y_{ijt}$.


## Relevant Concepts from Linear Algebra

Though by no means is linear algebra a prerequisite for understanding linear regression, some linear algebra concepts are helpful in understanding concepts in linear regression and econometrics.

Below are some selected concepts from linear algebra that may aid our presentation of linear regression:

### Scalars

A **scalar** is any constant. Scalars can either be standalone constants or used as a coefficients that *scale* another term (hence the name scalar). 

### Vectors

A **vector** is an ordered collection of numbers, which can be represented as a point in space. Vectors can be classified as either **column vectors** or **row vectors**:

- **Column Vector**: A vector with multiple rows and a single column. It is denoted as:

$$ \mathbf{v} = \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix} $$

- **Row Vector**: A vector with a single row and multiple columns. It is denoted as:
  
$$ \mathbf{v}^\top = [v_1, v_2, \ldots, v_n] $$

Generally, a vector $\mathbf{v}$ will be column vector. We can reorder the column vector into a row vector by transposing the vector, or flipping the vector across its diagonal. The superscripted $\top$ on $\mathbf{v}^\top$ denotes the transpose.

#### Operations on Vectors

- **Addition**: Two vectors of the same dimension can be added together:

$$ \mathbf{u} + \mathbf{v} = \begin{bmatrix}
u_1 + v_1 \\
u_2 + v_2 \\
\vdots \\
u_n + v_n
\end{bmatrix} $$

- **Scalar Multiplication**: A vector can be multiplied by a scalar (a single number):

$$ c \cdot \mathbf{v} = \begin{bmatrix}
c \cdot v_1 \\
c \cdot v_2 \\
\vdots \\
c \cdot v_n
\end{bmatrix} $$


- **Dot Product**: The dot product of two vectors results in a scalar:

$$ \mathbf{u}^\top \cdot \mathbf{v} = u_1 v_1 + u_2 v_2 + \ldots + u_n v_n $$

### Matrices

A **matrix** is a rectangular array of numbers arranged in rows and columns. It can be denoted as:

$$ \mathbf{A} = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{bmatrix} $$

where $m$ is the number of rows and $n$ is the number of columns.

> **Note:** Vectors are technically $m \times 1$ matrices.

When we refer to the *elements* within a matrix, however, we denote every element by its row ($i$) and its column ($j$) instead of using the $m \times n$ notation for describing the dimensions of the matrix. 

For example, we'll index an element $a_{ij}$ to be the element $a$ in the $i$-th row and the $j$-th column.


> ### Note on Notation
> In linear algebra, the notation for vectors and matrices follows a few conventions to indicate different levels of elements:
> 
> - **Boldface uppercase letters** like $\mathbf{A}$ represent **entire matrices**.
> - **Plain uppercase letters with subscript indexes** like $A_{ij}$ represent **collections of scalar elements** within a matrix (i.e., a vector within a matrix).
> - **Boldface lowercase letters** like $\mathbf{v}$ represent **vectors**.
> - **Plain lowercase letters with subscript indexes** like $a_{ij}$ represent **individual scalar elements** within a matrix.
> 
> For example, in the case of a matrix $\mathbf{A}$:
> 
> $$ \mathbf{A} = \begin{bmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{bmatrix} $$
>
> - $\mathbf{A}$ is the entire matrix.
> - $A_{ij}$ (uppercase) refers to the collection of elements at the  $i$-th and $j$-th positions of the matrix $\mathbf{A}$ (depending on context) with rows up to the $i$-th element, usually treated as a vector.
> - $a_{ij}$ (lowercase) refers to the element at the $i$-th row and $j$-th column.
>
> Let's say, instead, we define $\mathbf{A}$ as:
> 
> $$ \mathbf{A} = \begin{bmatrix} x_{1} & y_{1} & z_{1} \\ x_{2} & y_{2} & z_{2} \\ x_{3} & y_{3} & z_{3} \end{bmatrix} $$
>
> - $\mathbf{A}$ is the entire matrix.
> - $X_{i}$ (uppercase) is the collection of all $x$ elements (i.e., the **$\mathbf{x}$ vector**) in the matrix $\mathbf{A}$ with rows up to the $i$-th element.
> - $x_{i}$ (lowercase) refers to the element at the $i$-th row in the collection (vector) of $X_i$.
>
> Where the same logic applies across the $\mathbf{x}$, $\mathbf{y}$, and $\mathbf{z}$ vectors.
>
> ### Notation in Linear Regression
>
> In the earlier example from the Miguel & Kremer paper, we have:
>
> $$
Y_{ijt} = \alpha + \beta_1 T_{1it} + \beta_2 T_{2it} + \mathbf{X}^\prime \delta + \cdots$$
>
> Here, $T_{1it}$ and $T_{2it}$ are collections of elements of $T_1$ and $T_2$ in the dataset with resepctive $\beta$ scalar coefficients. 
> 
> $\mathbf{X}^\prime$ is defined as a "vector of additional school and pupil characteristics." But, where each characteristic contains entry for every individual $i$, $\mathbf{X}$ is really a *matrix* containing vectors for every characteristic (variable). $\mathbf{X}$, then, more closely follows the second specification of $\mathbf{A}$ from above.
>
> Lastly, we notice the prime on $\mathbf{X}^\prime$. Whereas transposes are usually denoted using a $\top$ superscript, they can also be denoted with a prime. $\mathbf{X}^\prime$ is then multiplied by $\delta$. It is convention that the regression coefficient on a matrix of characteristic vectors is placed after the matrix as opposed to before. 
> 
> We can write $\mathbf{X}^\prime \delta$ as:
>
> $$ \begin{bmatrix} x_{1} & x_{2} & x_{3} \\ y_{1} & y_{2} & y_{3} \\ z_{1} & z_{2} & z_{3} \end{bmatrix} \delta = \begin{bmatrix} \delta x_1 + \delta x_2 + \delta x_3 \\ \delta y_1 + \delta y_2 + \delta y_3 \\ \delta z_1 + \delta z_2 + \delta z_3 \end{bmatrix}$$

#### Operations on Matrices

- **Addition**: Two matrices of the same dimensions can be added element-wise:

  $$ \mathbf{C} = \mathbf{A} + \mathbf{B} \quad \text{where } C_{ij} = A_{ij} + B_{ij} $$

- **Scalar Multiplication**: A matrix can be multiplied by a scalar:

  $$ c \cdot \mathbf{A} = \begin{bmatrix}
  c \cdot a_{11} & c \cdot a_{12} & \cdots \\
  c \cdot a_{21} & c \cdot a_{22} & \cdots \\
  \vdots & \vdots & \ddots
  \end{bmatrix} $$

- **Matrix Multiplication**: The product of two matrices is obtained by taking the dot product of the rows of the first matrix with the columns of the second matrix. For matrices $\mathbf{A}$ (size $m \times n$) and $\mathbf{B}$ (size $n \times p$), the product $\mathbf{C} = \mathbf{A} \cdot \mathbf{B}$ results in a matrix $\mathbf{C}$ of size $m \times p$:

  $$ C_{ij} = \sum_{k=1}^{n} A_{ik} \cdot B_{kj} $$


> **Connections to Linear Regression:**
>
> Recall the basic form for linear regression:
> $$
Y_i = \alpha + \beta X_{i} + \varepsilon_i$$
>
> The subscript $i$ on the $X$ and $Y$ variables indicate individual $x$ and $y$ values for every individual (element) $i$ in the dataset. $X$ and $Y$, then, can be conceptualized as vectors (columns) in a matrix (the dataset with data on all individuals) in addition to $\varepsilon_i$.
>
> In this context, $\alpha$ and $\beta$ are scalars, with $\beta$ in particular representing the scalar coefficient on the vector $X$.
>
> But, to the earlier note of vectors being themselves $m \times 1$ matrices, we notate variables like $X$ and $Y$ as vectors with rows up to the $i$-th element. 

### Linear Combinations

A **linear combination** of a set of vectors $\{\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_n\}$ is any expression of the form:

$$
\mathbf{y} = c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_n\mathbf{v}_n
$$

where $\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_n$ are vectors and $c_1, c_2, \cdots, c_n$ are scalars (numbers). The resulting vector $\mathbf{y}$ is a linear combination of the vectors $\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_n$ with coefficients $c_1, c_2, \cdots, c_n$.


For example, the vector

$$ 
\mathbf{w} = \begin{bmatrix} 9 \\ 5 \\ 12 \end{bmatrix} 
$$

is a linear combination of the vectors

$$ 
\mathbf{u} = \begin{bmatrix} 3 \\ 1 \\ 3 \end{bmatrix}, \quad 
\mathbf{v} = \begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix}
$$

by the expression:

$$ 
2\begin{bmatrix} 3 \\ 1 \\ 3 \end{bmatrix} + 3\begin{bmatrix} 1 \\ 1 \\ 2 \end{bmatrix} = \begin{bmatrix} 9 \\ 5 \\ 12 \end{bmatrix} 
$$

where $c_1 = 2$ and $c_2 = 3$. 

This expression can be abstracted as:

$$
c_1\mathbf{u} + c_2\mathbf{v} = \mathbf{w}
$$

> **Connections to Linear Regression:**
>
> To expand on the earlier note on variables in a linear regression being vectors, the entire specification of a linear regression is itself a linear combination. 
>
> For example, if you have the regression specification:
>
> $$
Y_i = \alpha + \beta X_{i} + \varepsilon_i$$
>
> Where:
> - $Y_i$ is every individual's weekly earnings.
> - $X_i$ is every individual's years of college education.
> - $\alpha$ is the amount in weekly earnings given zero years of college education.
> - $\beta$ is the marginal effect of added year of college education on weekly earnings.
> - $\varepsilon_i$ is an idiosyncratic error term capturing exogenous variation in weekly earnings not explained by years of college education.
>
> We can write this as a linear combination, representing $Y$ and $X$ as vectors with values for every individual in the dataset:
>
> $$ 
\begin{bmatrix} 740 \\ 710 \\ 810 \\ 942 \\ 908 \\ 790 \end{bmatrix} = 700 + 30\begin{bmatrix} 2 \\ 0 \\ 4 \\ 6 \\ 7 \\ 3 \end{bmatrix} + \begin{bmatrix} -20 \\ 10 \\ -10 \\ +62 \\ -2 \\ 0 \end{bmatrix}$$
>
> Which allows us to estimate every individual $y_i$ from every $x_i$ and $\varepsilon_i$.

### Span

The **span** of a set of vectors $\{\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_n\}$ is the set of all possible **linear combinations** of these vectors. In other words, the span represents all vectors that can be formed by multiplying each vector $\mathbf{v}_i$ by a scalar $c_i$, and then summing the results. Mathematically, the span is written as:

$$
\text{Span}(\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_n) = \left\{ c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_n\mathbf{v}_n \mid c_1, c_2, \cdots, c_n \in \mathbb{R} \right\}
$$

This means the span of $\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_n$ is the set of all vectors that can be written as a linear combination of $\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_n$, with the scalars $c_1, c_2, \cdots, c_n$ coming from the set of real numbers $\mathbb{R}$. 

Span allows us to describe space. All linearly independent (more on this in the next section) vectors included in the span contribute new information to the span, providing a new unique direction (dimension) we can explore.

#### Example:
Consider the vectors:

$$
\mathbf{u} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}, \quad 
\mathbf{v} = \begin{bmatrix} 0 \\ 1 \end{bmatrix}
$$

The span of $\{\mathbf{u}, \mathbf{v}\}$ is the set of all vectors in $\mathbb{R}^2$ (2-D space) because there are two vectors (two dimensions) in the span. Any vector $\mathbf{w}$ in $\mathbb{R}^2$ can be written as a linear combination of $\mathbf{u}$ and $\mathbf{v}$:

$$
\mathbf{w} = c_1\mathbf{u} + c_2\mathbf{v} = c_1\begin{bmatrix} 1 \\ 0 \end{bmatrix} + c_2\begin{bmatrix} 0 \\ 1 \end{bmatrix} = \begin{bmatrix} c_1 \\ c_2 \end{bmatrix}
$$

Thus, the span of $\{\mathbf{u}, \mathbf{v}\}$ is the entire plane $\mathbb{R}^2$.

In contrast, if we have only a single vector, such as:

$$
\mathbf{a} = \begin{bmatrix} 2 \\ 1 \end{bmatrix}
$$

then the span of $\mathbf{a}$ (written as $\text{Span}(\mathbf{a})$) is the set of all vectors that lie on the line through the origin in the direction of $\mathbf{a}$. Any vector $\mathbf{w}$ in this span can be written as:

$$
\mathbf{w} = c_1 \mathbf{a} = c_1 \begin{bmatrix} 2 \\ 1 \end{bmatrix} = \begin{bmatrix} 2c_1 \\ c_1 \end{bmatrix}
$$

In this case, the span of $\mathbf{a}$ forms a line, not the entire plane. 

Therefore, the span describes the **space** that the vectors $\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_n$ cover, whether it be a line, a plane, or a higher-dimensional space.


### Linear Dependence & Independence

#### Linear Dependence

A set of vectors $\{\mathbf{v}_1, \mathbf{v}_2, \cdots, \mathbf{v}_k\}$ is said to be **linearly dependent** if there exist coefficients $c_1, c_2, \cdots, c_k$ that are *not all zero* such that:

$$
c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_k\mathbf{v}_k = \mathbf{0}
$$

This means that at least one vector in the set can be written as a linear combination of the others.

#### Linear Independence

On the other hand, a set of vectors is **linearly independent** if the only solution to the equation above is $c_1 = c_2 = \cdots = c_k = 0$. In this case, no vector in the set can be written as a combination of any other vector.

> **Note:** For our intents and purposes, when we're trying to represent a space with a span, all vectors included must be linearly independent to produce a minimal description of the space. 
> 
> The inclusion of linearly dependent vectors don't 'break' anything (at least, not glaringly), but they don't contribute any new information. To avoid redundancy, we can drop linearly dependent vectors until all included vectors are linearly independent.

#### Example:

Consider the vectors:

$$
\begin{equation*}
    \mathbf{v}_1 = \begin{bmatrix}
        1 \\ 0
    \end{bmatrix}, \quad
    \mathbf{v}_2 = \begin{bmatrix}
        0 \\ 1
    \end{bmatrix}, \quad
    \mathbf{v}_3 = \begin{bmatrix}
        1 \\ 1
    \end{bmatrix}
\end{equation*}
$$

The set $\{\mathbf{v}_1, \mathbf{v}_2, \mathbf{v}_3\}$ is linearly dependent because $\mathbf{v}_1 + \mathbf{v}_2 - \mathbf{v}_3 = \mathbf{0}$. 

However, the set $\{\mathbf{v}_1, \mathbf{v}_2\}$ is linearly *independent* because the only solution to 

$$
c_1\mathbf{v}_1 + c_2\mathbf{v}_2 = \begin{bmatrix} c_1 \\ c_2 \end{bmatrix} = \mathbf{0}
$$ 

is $c_1 = c_2 = 0$.

> **Connections to Linear Regression:**
>
> Later, we will learn about collinearity and multicollinearity, which are when independent variables in a regression are linearly dependent. Understanding linear independence allows us to understand why, for example, we can drop a collinear regressor in OLS.