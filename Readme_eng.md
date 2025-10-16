<img width="367" height="288" alt="image" src="https://github.com/user-attachments/assets/871c602e-d0fe-4573-a87b-fddab66e65e4" />

# **FRAUD DETECTION** 

**1\. Introduction**

Fraud detection in financial transactions is one of the main challenges in payment systems. The occurrence of suspicious operations directly impacts the security, credibility, and sustainability of the business. Anticipating and identifying fraudulent transactions in an automated way is essential to reduce financial losses, protect customers, and optimize monitoring processes. This project applies data science and machine learning techniques to predict the probability of fraud based on historical information from customers, terminals, and transactions.

**2\. Problem Solved**

The main problem addressed in this project is the automatic identification of fraudulent transactions within a large volume of operations. Since the fraud rate is low compared to the total number of transactions, this is a case of imbalanced binary classification.  The objective is to improve the model’s ability to identify frauds (recall) without excessively compromising precision, while maintaining robust discrimination metrics such as AUC, Gini, and KS.

**3\. Problem-Solving Approach**

The project was developed in three main stages:

**3.1 Data Understanding**

In this phase, the training, testing, customer, and terminal datasets were read and explored. The tables were integrated into a single ABT (Analytical Base Table), enriching the transactions with registration and geographical information. Descriptive and sanity analyses were conducted to verify fraud rates by period, terminals with higher occurrence, and customer profiles involved. This stage provided insights into data distribution, seasonality, and the concentration of suspicious events.

**3.2 Data Preparation**

The next stage focused on feature engineering, creating variables that increase the predictive power of the model.  Temporal features were generated (hour, day of the week, weekend, time of day), distance calculations between customers and terminals using the Haversine formula, and aggregated variables in time windows (sums, averages, medians, minimums, maximums, and number of transactions within 1h, 24h, 7 days, etc.).  Risk flags were also included, such as nighttime transactions and consecutive transactions at the same terminal. This process resulted in an enriched and structured dataset designed to better capture fraud patterns.

**3.3 Predictive Modeling**

With the data prepared, preprocessing pipelines were built including missing value imputation, categorical variable encoding using Target Encoding, and standardization of numerical variables with StandardScaler. Different algorithms were tested, including Decision Tree, Logistic Regression, Random Forest, Gradient Boosting, XGBoost, and LightGBM. Model evaluation considered metrics such as accuracy, precision, recall, F1-score, AUC, Gini, and KS.

The XGBoost model, after hyperparameter tuning with Optuna, presented the best balance between performance and discriminative power. Dimensionality reduction tests were also performed (keeping the top 50 and then 25 most important variables) to compare latency and performance. The version with 50 variables was chosen, as it preserved the best classification results while maintaining suitable prediction time for production use.

**4\. Project Structure**

├── 00-Dados/                          \# Folder containing raw and derived datasets

 │   ├── customer.csv                   \# Customer information

 │   ├── terminal.csv                   \# Transaction terminal data

 │   ├── train.csv                      \# Training dataset for modeling

 │   ├── test.csv                       \# Test dataset for model validation

 │   └── abt\_vigente\_score.csv          \# Processed dataset ready for scoring or prediction

 ├── 01-Entendimento e Preparação dos Dados/   \# Notebooks for data exploration and cleaning

 ├── 02-Modelagem/                      \# Machine learning modeling and evaluation phase

 ├── Readme                             \# Project explanation

 

**5\. Technologies Used**

**Language**: Python

**Libraries**:

  *  pandas, numpy – data manipulation and analysis

  *  matplotlib, seaborn – data visualization

  *  scikit-learn – preprocessing, modeling, and metrics

  *  xgboost, lightgbm, catboost – boosting algorithms

  *  optuna – hyperparameter tuning

**6\. Results**

The initial exploratory analysis showed that the dataset contained 291,231 transactions, involving 998 customers and 1,994 different terminals, ensuring representativeness and robustness for testing. From the data preparation phase, dozens of derived variables were created to capture suspicious behaviors. In modeling, the tuned XGBoost model stood out, achieving an AUC-ROC of 0.7743, Gini of 0.45, and KS of 0.36 — values indicating good capability to distinguish legitimate from fraudulent transactions.

It was also possible to simplify the solution to only 50 variables without significant performance loss, reducing latency and making the model more suitable for production environments.

The direct impact of this solution includes faster fraud detection, enabling near real-time responses, reduction of financial losses by automatically identifying more suspicious cases, and strategic support for risk teams through a prioritization tool based on fraud scores. That away, the project delivered a robust and scalable pipeline capable of strengthening operational security and increasing business efficiency.

