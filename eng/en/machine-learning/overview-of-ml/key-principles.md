# Key Terms and Principles

### Key Principles of Machine Learning

<div align="left"><figure><img src="../../../.gitbook/assets/ml-key-principles-min.png" alt="" width="375"><figcaption><p>Key Terms and Principles</p></figcaption></figure></div>

#### **Data-Driven Learning**

Machine learning relies on data. The more data we have, the better the model can learn. Data helps the model understand patterns and make predictions.

#### **Model**

A model is the core of machine learning. It’s a mathematical representation that the system builds by learning from data. Once trained, the model uses this knowledge to predict outcomes or classify data.

#### **Training**

During the training process, the model is exposed to a dataset (called the training set) where the input and the expected output are known. The model uses this data to adjust itself and learn how to make accurate predictions. The goal of training is to minimize errors when the model predicts the output.

#### **Testing**

After training, the model is tested with a different set of data (called the testing set) to see how well it performs on data it hasn’t seen before. This helps measure how accurate and reliable the model is.

### Features and Labels

Features are the input data used to make predictions. For example, if we are predicting house prices, the size of the house, number of rooms, and location would be considered features.

Labels are the output data we are trying to predict. In the house price example, the label would be the actual price of the house.

### How Machine Learning Works

1. **Collect Data**: You gather a set of data related to the problem you want to solve.
2. **Train the Model**: The machine learning algorithm uses this data to learn patterns. During training, the model adjusts its internal parameters to improve its accuracy.
3. **Test the Model**: After training, you test the model on new data to see if it can make good predictions.
4. **Make Predictions**: Once trained and tested, the model can predict outcomes on new, unseen data.

### **Common Terms in Machine Learning**

Understanding key terms in machine learning is essential for grasping how models are built, trained, and evaluated. Below are some important concepts explained in simple terms.

#### **Data Used Directly and Indirectly**

Machine learning models use data in two ways: directly and indirectly. Direct data is the main dataset fed into the model for training, while indirect data may include additional information like metadata, preprocessing steps, or external insights that influence the model’s performance.

#### **Primary and Accurate Data**

Primary data refers to raw, original data collected from a source, such as user inputs or sensor readings. Accurate data is clean, error-free, and well-processed to ensure the reliability of the machine learning model.

#### **Training and Reserved Datasets**

A dataset is usually split into different parts:

* **Training set** – used to teach the model patterns and relationships.
* **Validation set** – used to tune model parameters and avoid overfitting.
* **Test set** – used to evaluate the model's performance on unseen data.\
  Reserved datasets ensure that models generalize well and do not simply memorize patterns.

#### **Benchmark**

A benchmark is a standard or reference point used to compare machine learning models. It can be a dataset, an accuracy metric, or an existing model's performance. Benchmarks help researchers and developers measure improvements and select the best approaches.

#### **Machine Learning Pipeline**

A pipeline is a sequence of steps in a machine learning workflow. It typically includes data preprocessing, feature extraction, model training, evaluation, and deployment. Pipelines help automate and streamline the process of building AI systems.

#### **Parameters and Hyperparameters**

* **Parameters** are learned by the model during training (e.g., weights in a neural network).
* **Hyperparameters** are set before training and affect the learning process (e.g., learning rate, batch size). Finding the right hyperparameters is crucial for model performance.

#### **Model-Based and Instance-Based Learning**

* **Model-based learning** creates a general representation of data (e.g., decision trees, neural networks).
* **Instance-based learning** memorizes examples and makes predictions based on similarities (e.g., k-nearest neighbors).

#### **Shallow and Deep Learning**

* **Shallow learning** involves simple models with limited layers (e.g., linear regression, decision trees).
* **Deep learning** uses multi-layered neural networks to learn complex patterns from large datasets (e.g., convolutional neural networks for image recognition).

#### **Training and Evaluation**

Training involves feeding data into a machine learning model to adjust its parameters. Evaluation tests the model’s performance using unseen data to ensure it generalizes well. A good evaluation process helps detect overfitting and improve model accuracy.

#### **Overfitting**

Overfitting happens when a model learns the training data too well, including noise or random fluctuations. As a result, it may perform well on the training data but poorly on new, unseen data. Overfitting reduces a model’s ability to generalize.

#### **Underfitting**

Underfitting occurs when a model is too simple and cannot capture the patterns in the data properly. This usually happens when a model lacks enough complexity to learn from the dataset, leading to poor performance on both training and test data.

#### **Accuracy**

Accuracy is a common metric used to measure how often a machine learning model makes correct predictions. It is calculated as the ratio of correct predictions to the total number of predictions. However, accuracy alone may not be a reliable measure when dealing with imbalanced datasets.

Understanding these terms provides a strong foundation for anyone exploring machine learning, whether as a beginner or an advanced practitioner.
