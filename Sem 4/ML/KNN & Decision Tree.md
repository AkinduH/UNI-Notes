
**KNN is a non-parametric** algorithm, meaning it does **not assume** any particular distribution of data. This makes it highly flexible for different types of datasets.
Model parameters grow with the training data by considering each training case as a parameter of the model.

**KNN is computationally expensive** because it **stores all training data** and computes distances for every new query point. This makes it slow for large datasets.

KNN is a **lazy** learner because it follows **instance-based learning** and also uses the training data when it actually needs to make a prediction on the unseen datasets.

Feature scaling improves the performance of the algorithm

Feature scaling has a significant contribution when it comes to calculating distance measurements

In KNN, the **training time** refers to the time it takes to store the data. KNN doesn't require any actual training phase (no model building), so the time complexity is **O(n)**, where **n** is the number of data points. The features **f** do not affect the training time complexity significantly because it simply stores the data.

KNN can be used for imputing missing value of both categorical and continuous variables.

- **Hamming Distance** is used to measure the difference between two strings or vectors of categorical variables. It counts the number of positions at which the corresponding elements are different.
    
- For example, if you are comparing two categorical vectors like **[Red, Blue, Green]** and **[Red, Green, Green]**, the Hamming distance would be **1** (because "Blue" and "Green" differ).
    
- **Euclidean** and **Manhattan distances** are used for continuous (numerical) data, not categorical variables.

KNN is a supervised learning algorithm because it requires labeled data to train the model. The algorithm makes predictions based on the labels of the data points in the training set.
