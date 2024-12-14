
#### Discrete Random Variables


countable number of distinct values
**probability mass function** P(X)
∑​P(X)=1

Cumulative Distribution Function
∑i=1->k P(xi)

Mean
E(X)   =   ∑i=1->n xiP(xi)

Varience
V(X)  =  E{(X-u)^2}    or    V(X)  =  E(X^2) - {E(X)}^2



#### Continuos Random Variables


infinite number of possible values within a given range
probability of a specific outcome is not defined
**probability density function**, f(x)
∫−∞->∞​ f(x)dx=1


Cumulative Distribution Function
∫−∞->x  ​f(x)dx


Mean
E(X)   =  ∫−∞->∞ xf(x) dx

Varience
V(X)  =  E{(X-u)^2}    or    V(X)  =  E(X^2) - {E(X)}^2



![[Pasted image 20240329114620.png]]

![[Pasted image 20240329114629.png]]


![[Pasted image 20240330001219.png]]


Covariance

$$
Cov(X, Y) = E{(X - E[X]) . (Y - E[Y])}
$$

- If (Cov(X, Y) > 0), then (X) and (Y) tend to increase and decrease together (positive relationship).
- If (Cov(X, Y) < 0), then as (X) increases, (Y) tends to decrease, and vice versa (negative relationship).
- If (Cov(X, Y) = 0), then (X) and (Y) are uncorrelated; i.e., changes in (X) do not correspond with changes in (Y).


Correlation

$$
Corr(X, Y) = Cov(X, Y) / sqrt[V(X) . V(Y)]
$$

- A correlation of +1 indicates a perfect positive linear relationship between variables.
- A correlation of -1 indicates a perfect negative linear relationship.
- A correlation of 0 suggests no linear relationship between the variables.



Normal distribution(Notation is below)
T~N(Mean,Varience)

The sum of the probabilities of mutually exclusive and exhaustive events must equal 1.
### Mutually Exclusive Events

Two events are **mutually exclusive** if they cannot occur at the same time. For example, if you roll a die, the events "rolling a 3" and "rolling a 5" are mutually exclusive because you can't roll both a 3 and a 5 in a single roll.

### Exhaustive Events

A set of events is **exhaustive** if they cover all possible outcomes of a given experiment. In other words, one of the events must occur. For instance, if you roll a die, the events "rolling a 1", "rolling a 2", "rolling a 3", "rolling a 4", "rolling a 5", and "rolling a 6" are exhaustive because one of these outcomes must happen.



A cohort study in the context of probability refers to a type of longitudinal study that follows a specific group or cohort of individuals over an extended period of time.

An experimental study is a type of research design in which the researcher actively introduces an intervention or treatment and evaluates its effects on one or more outcome variables.

A retrospective study design investigates exposures and outcomes that have already occurred by looking backward in time.


### 1. Stratified Sampling

- **Definition**: Divides the population into distinct sub-groups (strata) based on specific characteristics.
- **Method**: Randomly samples from each stratum.
- **Purpose**: Ensures each subgroup is adequately represented in the sample.

### 2. Systematic Sampling

- **Definition**: Selects every kkk-th item from a list or sequence.
- **Method**: Determines a fixed interval kkk (e.g., every 200th item).
- **Purpose**: Provides a simple and quick method for sampling when items are ordered.

### 3. Stratified sampling with equal allocation

- **Definition**: Similar to stratified random sampling but focuses on dividing the population into strata and ensuring proportional representation.
- **Method**: Samples are taken from each stratum to represent the whole population.
- **Purpose**: Achieves more accurate and representative samples by accounting for different sub-groups within the population.



a. **Population of Interest**

- **Definition**: The entire group about which you want to draw conclusions.
- **For this scenario**: All voters who will participate in the upcoming Mayoral election.

b. **Variable of Interest**

- **Definition**: The characteristic or attribute that is being measured or observed.
- **For this scenario**: The political leaning of voters (Republican, Democratic, Independent, or other/undecided).

c. **Sample**

- **Definition**: A subset of the population that is actually observed or surveyed.
- **For this scenario**: The 814 voters who were randomly selected and asked about their political leaning.

d. **Statistic**

- **Definition**: A numerical summary or measure computed from the sample data.
- **For this scenario**: The proportion of the sample who said they were “…leaning Democratic…” (38.2%).


### 1. **Nominal Data**

- **Definition**: Categorical data where the categories have no intrinsic order or ranking.
- **Characteristics**: Used for labeling or identifying categories.
- **Examples**: Gender, Employee Number, Colors, Types of Animals.

### 2. **Ordinal Data**

- **Definition**: Categorical data with a meaningful order or ranking, but the differences between categories are not uniform or measurable.
- **Characteristics**: Indicates the relative position or order but not the exact difference between them.
- **Examples**: Employee Rank, Education Level (e.g., High School, Bachelor’s, Master’s, Ph.D.), Customer Satisfaction Ratings (e.g., Poor, Fair, Good, Excellent).

### 3. **Interval Data**

- **Definition**: Numeric data where the intervals between values are meaningful, but there is no true zero point.
- **Characteristics**: Allows for the measurement of the difference between values but not the ratio between them.
- **Examples**: Temperature in Celsius or Fahrenheit, IQ Scores.

### 4. **Ratio Data**

- **Definition**: Numeric data with a meaningful zero point, where both differences and ratios between values are meaningful.
- **Characteristics**: Allows for the measurement of absolute differences and the calculation of ratios.
- **Examples**: Height, Weight, Years of Experience, Yearly Salary.

A **sample space** is a set of elements that represents all possible outcomes of a statistical experiment and a subset of **sample space** is known as the **event**. A single element of **sample space** is known as a **sample point**.