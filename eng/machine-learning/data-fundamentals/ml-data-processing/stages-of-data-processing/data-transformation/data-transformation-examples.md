# Data Transformation Examples

In machine learning, data transformation is essential to ensure data is ready for analysis, training, and modeling. This chapter provides examples of common data transformation techniques, demonstrating how each process optimizes data for machine learning applications. These examples include encoding categorical data, normalizing numerical features, handling missing values, and engineering new features. Through these transformations, data is refined into a structured format, allowing algorithms to learn more effectively.

### 1. **Encoding Categorical Data**

Many machine learning algorithms cannot directly process categorical data, so it must be transformed into numerical form. Two primary methods for encoding categorical data are one-hot encoding and label encoding.

* **One-Hot Encoding**: This technique creates binary columns for each category in a variable. For example, a column “Color” with categories "Red," "Blue," and "Green" would be converted into three new columns—one for each color—with values of 1 or 0 indicating presence or absence.
* **Label Encoding**: Here, each category is assigned a unique integer. For example, "Red," "Blue," and "Green" in the “Color” column could be converted to 0, 1, and 2, respectively. However, this technique is typically only suitable when there is an ordinal relationship among categories.

### 2. **Normalizing and Scaling Numerical Data**

Normalizing and scaling data ensures that numerical features have consistent ranges, which is critical for algorithms sensitive to magnitude, such as k-nearest neighbors and neural networks.

* **Normalization**: This technique scales all data points to a specific range, usually 0 to 1. For instance, ages ranging from 0 to 100 would be converted so that all values fall between 0 and 1, ensuring consistency.
* **Standardization**: Here, data is adjusted to have a mean of zero and a standard deviation of one. For example, if house prices range widely, standardizing them would allow the model to treat them as a feature with equal weight, regardless of magnitude.

### 3. **Handling Missing Values**

Missing data is a common issue, and imputation techniques are used to fill in these gaps, preventing data loss and ensuring smooth model operation.

* **Mean/Median Imputation**: For numerical features, the missing values are often filled in with the mean or median of the available data. For instance, if income data has missing entries, these might be replaced with the median income value.
* **Advanced Imputation**: Techniques like k-nearest neighbors imputation predict missing values by examining similar data points. This is more complex but often provides more accurate replacements.

### 4. **Feature Engineering**

Feature engineering involves creating new variables or modifying existing ones to make the data more informative for the model. This technique is especially powerful for revealing hidden relationships within the data.

* **Date and Time Engineering**: For example, a timestamp can be broken down into features such as “hour,” “day of the week,” and “month,” allowing models to detect patterns over time.
* **Textual Data Transformation**: In NLP applications, transforming text data into word embeddings (such as Word2Vec or GloVe) enables the model to understand the contextual meaning of words, which is crucial for tasks like sentiment analysis or classification.

### 5. **Reshaping Data for Sequence and Image Models**

For certain types of data, restructuring is necessary to align with the format expected by specific models, such as those for time-series or image data.

* **Time-Series Reshaping**: In forecasting models, data is often restructured into rolling windows of fixed size. For example, daily sales figures might be grouped into weekly sequences, enabling the model to capture trends over time.
* **Image Reshaping**: Image data typically requires a 3D matrix (height, width, color channels). An image dataset with flat arrays must be reshaped before being fed into a Convolutional Neural Network (CNN).

### Conclusion

These data transformation examples demonstrate how raw data can be adjusted, structured, and refined to enhance the effectiveness of machine learning models. Each technique — encoding, normalizing, handling missing values, feature engineering, and reshaping—serves a specific purpose in making data compatible and meaningful for algorithms. By applying these transformations, data scientists ensure that their datasets are prepared to support reliable, high-performing models.
