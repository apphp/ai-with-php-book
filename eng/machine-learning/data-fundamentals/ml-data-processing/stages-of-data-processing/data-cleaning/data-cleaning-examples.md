# Data Cleaning Examples

Here are some real-world examples illustrating each of the core data-cleaning techniques—handling missing values, normalization, and standardization.

### Example 1: Handling Missing Values

**Scenario**: A hospital is analyzing patient data to predict the likelihood of readmission. The dataset includes various attributes such as age, weight, blood pressure, and cholesterol levels. However, some patients are missing certain measurements.

**Solution:**

* **Removing Missing Values**: If a small percentage (e.g., 1-2%) of the data points for cholesterol levels are missing and appear randomly, the rows with missing cholesterol values could be removed, maintaining data integrity with minimal data loss.
* **Imputation**:
  * **Mean Imputation**: Suppose 10% of patients are missing blood pressure readings. In this case, the hospital could impute these missing values by filling in the average blood pressure of other patients. This approach assumes the missing data points aren’t significantly different from observed values.
  * **KNN Imputation**: For more sophisticated handling, the hospital could use K-Nearest Neighbors imputation, where missing values are filled based on similar patients with complete records. This method is particularly helpful if patients’ health metrics are known to correlate, as KNN imputation leverages patterns among similar data points.

### Example 2: Normalization

**Scenario**: An e-commerce platform is analyzing user behavior to recommend products. The dataset includes features like the number of page views, time spent on the site, and total money spent. These variables vary widely in scale—for example, page views may range up to thousands, while time on site is measured in minutes.

**Solution:**

* **Min-Max Scaling**: To ensure the model weighs each feature equally, the e-commerce team applies Min-Max Scaling to bring each variable into the range \[0,1]. For instance:
  * If a user’s session time ranges from 2 to 120 minutes, and one user’s time is 60 minutes, the scaled value would be: $$X_{\text{scaled}} = \frac{120 - 2}{60 - 2} = 0.49$$
  * This helps algorithms like KNN and neural networks to perform better, as they won’t be influenced by the differing scales of time versus page views or spending.

### Example 3: Standardization

**Scenario**: A credit bureau is developing a model to predict loan default risk based on income, age, credit score, and loan amount. These features have different units and ranges, with credit score typically ranging from 300 to 850, income from thousands to hundreds of thousands, and loan amounts varying widely.

**Solution:**

* **Z-score Scaling (Standardization)**: By applying standardization, each feature is transformed to have a mean of 0 and a standard deviation of 1. For instance, if the average income in the dataset is $50,000 with a standard deviation of $15,000, and one applicant has an income of $65,000, their standardized income would be: $$Z = \frac{65,000 - 50,000}{15,000} = 1.0$$
  * This allows algorithms such as Support Vector Machines and PCA to interpret all features as contributing equally, enhancing model stability.

### Summary

In these examples:

* **Missing value handling** (removal, mean imputation, KNN) ensures data completeness.
* **Normalization** aligns feature ranges for ML models that weigh distance between data points, such as KNN.
* **Standardization** equalizes feature contribution for models sensitive to variance, like SVMs and PCA.

These steps enable accurate, fair predictions by reducing biases caused by missing values and differing scales.
