## Encoding
### 1. **One-Hot Encoding**

**Description:**

- Converts each category into a binary column (0s and 1s).
- Suitable for nominal (unordered) categories.

**Example:**  
For a feature "Color" with categories: Red, Blue, Green:

|Color|Red|Blue|Green|
|---|---|---|---|
|Red|1|0|0|
|Blue|0|1|0|
|Green|0|0|1|

---

### 2. **Label Encoding**

**Description:**

- Assigns a unique integer to each category.
- Suitable for ordinal (ordered) categories.

**Example:**  
For "Size" (Small < Medium < Large):

|Size|Encoded|
|---|---|
|Small|0|
|Medium|1|
|Large|2|

---

### 3. **Ordinal Encoding**

**Description:**

- Similar to label encoding but specifically for ordinal data where the order matters.
- The numeric values represent the ranking or level.

**Example:**  
For "Education Level" (High School < Bachelor < Master < PhD):

|Education Level|Encoded|
|---|---|
|High School|1|
|Bachelor|2|
|Master|3|
|PhD|4|

---

### 4. **Helmert Encoding**

**Description:**

- Compares each level to the mean of subsequent levels.
- Commonly used in statistical modeling.

**Example:**  
For "Season" with categories [Winter, Spring, Summer]:

- Spring compares to Summer.
- Winter compares to the mean of Spring and Summer.

_Actual numeric values depend on statistical computations._

---

### 5. **Binary Encoding**

**Description:**

- Converts categories to binary numbers and then splits them into separate columns.
- Reduces the dimensionality compared to one-hot encoding.

**Example:**  
For "City" with categories: New York, Paris, Tokyo:

|City|Binary|Col1|Col2|
|---|---|---|---|
|New York|00|0|0|
|Paris|01|0|1|
|Tokyo|10|1|0|

---

### 6. **Frequency Encoding**

**Description:**

- Replaces each category with its frequency count.
- Useful for high cardinality categorical features.

**Example:**  
For "Country" with occurrences: USA(100), UK(50), India(150):

|Country|Frequency|
|---|---|
|USA|100|
|UK|50|
|India|150|

---

### 7. **Mean Encoding (Target Encoding)**

**Description:**

- Replaces each category with the mean of the target variable.
- Useful for predictive models but can cause overfitting.

**Example:**  
For "City" and average house prices:

|City|Average Price|
|---|---|
|New York|500,000|
|Paris|450,000|
|Tokyo|550,000|

---

### 8. **Weight of Evidence (WOE) Encoding**

**Description:**

- Calculates WOE for each category based on the ratio of good to bad outcomes.
- Commonly used in credit scoring.

**Formula:**  
WOE=ln⁡(P(Good)P(Bad))\text{WOE} = \ln\left(\frac{P(\text{Good})}{P(\text{Bad})}\right)

**Example:**  
For a "Job Type" and loan defaults:

|Job Type|Good Loans|Bad Loans|WOE|
|---|---|---|---|
|Manager|80|20|1.386|
|Clerk|50|50|0.000|

---

### 9. **Probability Ratio Encoding**

**Description:**

- Similar to WOE, but uses the probability ratio directly without taking the log.

**Formula:**  
PR=P(Good)P(Bad)\text{PR} = \frac{P(\text{Good})}{P(\text{Bad})}

**Example:**  
For a "Payment Method":

|Method|Good (%)|Bad (%)|Probability Ratio|
|---|---|---|---|
|Credit|80%|20%|4.0|
|Cash|60%|40%|1.5|

---

### 10. **Hashing Encoding**

**Description:**

- Applies a hash function to categories and maps them into a fixed number of columns.
- Useful for high-cardinality categorical data.

**Example:**  
For "Fruit" with hashing to 2 columns:

|Fruit|Hash Col1|Hash Col2|
|---|---|---|
|Apple|0.12|0.87|
|Banana|0.45|0.23|

---

### 11. **Backward Difference Encoding**

**Description:**

- Compares each level with the previous level.
- Common in statistical analysis for ordered categories.

**Example:**  
For "Quality" (Low, Medium, High):

|Quality|BDE1|BDE2|
|---|---|---|
|Low|0|0|
|Medium|1|0|
|High|1|1|

---

### 12. **Leave-One-Out Encoding**

**Description:**

- Similar to mean encoding, but excludes the current row’s target value when calculating the mean.
- Reduces overfitting compared to mean encoding.

**Example:**  
For "Region" with target:

|Region|Target|LOO Encoding|
|---|---|---|
|North|100|225|
|East|200|150|
|South|300|150|

---

### 13. **James-Stein Encoding**

**Description:**

- A Bayesian technique that shrinks category means toward the overall mean based on the sample size.
- Reduces overfitting, especially with small datasets.

**Formula:**  
$μ^c=w⋅μc+(1−w)⋅μglobal\hat{\mu}_c = w \cdot \mu_c + (1 - w) \cdot \mu_{\text{global}}$

---

### 14. **M-Estimator Encoding**

**Description:**

- A variation of target encoding with smoothing.
- Balances category mean with overall mean using a parameter mm.

**Formula:**  
Encoded Value=Sum(targets) + m * Mean(global)Count + m\text{Encoded Value} = \frac{\text{Sum(targets) + m * Mean(global)}}{\text{Count + m}}

**Example:**  
For "State" and sales with m=5m=5:

|State|Sales Mean|Encoded Value|
|---|---|---|
|Texas|200|202.5|
|Nevada|150|180.5|

---
![[Pasted image 20250215231536.png]]![[Pasted image 20250215231542.png]]
![[Pasted image 20250216000658.png]]

### 1. **Missing Value Ratio**

- **Idea:** Features with a high percentage of missing values provide less information and may harm model performance.
- **Technique:** Set a threshold (e.g., 50%), and drop features exceeding that threshold.
- **Example:** If a column has 80% missing values, it’s usually removed.

---

### 2. **Low Variance Filter**

- **Idea:** Features with very low variance (almost constant) do not contribute to the model’s predictive power.
- **Technique:** Remove features with variance below a defined threshold.
- **Example:** If a column has values `[1, 1, 1, 1, 1]`, it carries no useful information.

---

### 3. **High Correlation Filter**

- **Idea:** Highly correlated features provide redundant information.
- **Technique:** Compute the correlation matrix and drop one of the features from highly correlated pairs.
- **Example:** If `height` and `arm span` have a correlation of 0.98, remove one.

---

### 4. **Random Forest (Feature Importance)**

- **Idea:** Random forests can rank features by their importance based on how often they reduce impurity (e.g., Gini or entropy).
- **Technique:** Train a random forest and select features with the highest importance scores.
- **Example:** Using `model.feature_importances_` from `RandomForestClassifier` to drop low-scoring features.

---

### 5. **Backward Feature Extraction (Backward Elimination)**

- **Idea:** Start with all features and remove them one by one based on their impact on model performance.
- **Technique:** Train the model, remove the least significant feature (based on p-values or coefficients), and repeat until performance drops.
- **Example:** In regression, remove features with the highest p-value iteratively.

---

### 6. **Forward Feature Selection**

- **Idea:** Start with no features and add them one by one, selecting those that improve model performance the most.
- **Technique:** Iteratively add the best-performing feature until no further improvement is observed.
- **Example:** Begin with one feature, test model accuracy, and add the next best feature step-by-step.

---

**Dimensionality reduction** techniques like PCA (Principal Component Analysis) aim to reduce **collinearity** by transforming correlated features into a smaller set of uncorrelated features (principal components). This helps to mitigate issues like multicollinearity in models.

Converting a feature into multiple binary features → Binning

Using an arbitrary value or a value from a variable distribution to replace the maximum and minimum values → Capping

### **Feature Crossing**

**Feature crossing** is a technique in machine learning where two or more features are combined (typically by multiplication or concatenation) to create new, more informative features. This helps models, especially **linear models**, capture **non-linear relationships** in the data.