# Data Cleaning Types

### Chapter: Data Cleaning Step in ML Data Processing

Data cleaning is the crucial first step in preparing data for machine learning (ML) models. Quality data is essential for effective analysis, model accuracy, and interpretability. In this chapter, we’ll explore three core aspects of data cleaning: **handling missing values**, **normalization**, and **standardization**.

### 1. Handling Missing Values

Missing data is common in raw datasets and can impact model performance if not managed effectively. Here are the main techniques for handling missing values:

* **Removal of Missing Values**: In some cases, rows (or columns) with missing values can be safely removed. This is often suitable when only a small portion of the dataset is missing or when missing values are randomly scattered across many variables. However, if a significant portion of the data has missing values, this approach may reduce the dataset size, potentially biasing the results.
* **Imputation**: For datasets where data loss is undesirable, imputing missing values can be a better approach. Imputation techniques include:
  * **Mean/Median Imputation**: For numerical data, replacing missing values with the mean or median of the column is common. This is effective when data is approximately normally distributed.
  * **Mode Imputation**: For categorical data, the mode (most frequent value) is often used to fill missing values.
  * **Advanced Imputation**: Techniques like **K-Nearest Neighbors (KNN) imputation** and **Multivariate Imputation by Chained Equations (MICE)** consider relationships across multiple features for more sophisticated estimates.

Choosing the right method depends on data characteristics and the importance of preserving specific statistical properties.

### 2. Normalization

#### 2.1 Description

Normalization adjusts the scale of data, bringing all values into a consistent range (typically 0 to 1 or -1 to 1). It is especially useful for algorithms sensitive to data magnitude, such as **K-Nearest Neighbors (KNN)** and **Neural Networks**.

Common normalization techniques include:

* **Min-Max Scaling**: This method scales the data by mapping it to a \[0, 1] range, making all features directly comparable. The formula is: $$X_{\text{scaled}} = \frac{X - X_{\text{min}}}{X_{\text{max}} - X_{\text{min}}}$$
* **Max Abs Scaling**: This scales values by dividing by the maximum absolute value of each feature, maintaining the range but preserving the sign.

Normalization is ideal for data that doesn’t follow a Gaussian distribution or where you need to constrain features within a specific range.

### 3. Standardization

Standardization transforms features to have a mean of zero and a standard deviation of one. This step is beneficial for algorithms like **Support Vector Machines (SVM)** and **Principal Component Analysis (PCA)**, where feature variance significantly impacts the outcome.

Standardization typically uses **Z-score scaling**: $$Z = \frac{X - \mu}{\sigma}$$

where $$\mu$$ is the mean and $$\sigma$$ is the standard deviation of the feature. This technique aligns data distribution closer to a standard normal distribution, ensuring that each feature contributes equally in ML models sensitive to feature scaling.

### Conclusion

Data cleaning, which includes handling missing values, normalization, and standardization, sets a strong foundation for data analysis and machine learning. By carefully choosing and applying these techniques, we ensure that the dataset is ready for further preprocessing steps and modeling, improving model reliability and interpretability.
