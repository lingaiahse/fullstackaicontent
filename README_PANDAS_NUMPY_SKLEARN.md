# Pandas, NumPy, and scikit-learn

This README covers the three most popular Python libraries for data science and machine learning:

- `NumPy` for numerical computation and array operations
- `Pandas` for data manipulation and analysis
- `scikit-learn` for machine learning models, preprocessing, and evaluation

---

## 1. NumPy

### What is NumPy?

NumPy is the foundational numerical computing library in Python. It provides:

- multi-dimensional arrays (`ndarray`)
- vectorized arithmetic operations
- linear algebra, random number generation, and FFT
- tools for efficient numerical computation

### Core Concepts

- `np.array`: create homogeneous arrays
- array shape and indexing
- broadcasting
- `np.zeros`, `np.ones`, `np.arange`, `np.linspace`

### Example: Basic NumPy operations

```python
import numpy as np

# Create arrays
arr1 = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6])

# Arithmetic operations are element-wise
sum_arr = arr1 + arr2          # [5 7 9]
prod_arr = arr1 * arr2         # [4 10 18]

# Statistics
mean_value = np.mean(arr1)     # 2.0
std_value = np.std(arr2)       # 0.816496580927726

# Reshaping and broadcasting
matrix = np.arange(1, 7).reshape(2, 3)
row = np.array([10, 20, 30])
added = matrix + row           # broadcast row across each matrix row
```

### Real-time use case: Sensor data normalization

Imagine you receive time-series data from IoT sensors and need to normalize each sensor reading before analysis.

```python
raw_readings = np.array([[10.0, 20.0, 15.0],
                         [12.0, 18.0, 14.0],
                         [11.0, 21.0, 16.0]])

# Normalize each column to 0-1 range
min_vals = raw_readings.min(axis=0)
max_vals = raw_readings.max(axis=0)
norm_readings = (raw_readings - min_vals) / (max_vals - min_vals)
```

This makes the data easier to compare across sensors and feed into machine learning models.

---

## 2. Pandas

### What is Pandas?

Pandas is a data analysis library built on top of NumPy. It enables working with labeled data through:

- `Series` for one-dimensional labeled data
- `DataFrame` for two-dimensional tabular data
- powerful data cleaning, filtering, grouping, and aggregation

### Core Concepts

- `pd.read_csv`, `pd.read_excel`, `pd.read_sql`
- selecting rows and columns with `.loc[]` and `.iloc[]`
- missing value handling with `.dropna()` and `.fillna()`
- grouping with `.groupby()`
- joins with `merge()` and `concat()`

### Example: Working with a DataFrame

```python
import pandas as pd

# Create example DataFrame
data = {
    'date': ['2025-01-01', '2025-01-02', '2025-01-03', '2025-01-04'],
    'sales': [120, 150, 100, 170],
    'region': ['north', 'south', 'north', 'south']
}
df = pd.DataFrame(data)

# Convert date column to datetime type
df['date'] = pd.to_datetime(df['date'])

# Filter rows and create a derived column
north_sales = df[df['region'] == 'north']
df['sales_usd'] = df['sales'] * 1.0

# Group by region and compute summary statistics
summary = df.groupby('region')['sales'].agg(['sum', 'mean', 'max'])
```

### Real-time use case: Sales dashboard data pipeline

Pandas is ideal for cleaning data before reporting or feeding a model. Example workflow:

1. Load sales transaction CSVs
2. Convert timestamps
3. Fill or drop missing values
4. Aggregate revenue by date and region

```python
sales_df = pd.read_csv('sales_transactions.csv')

# Clean and enrich
sales_df['date'] = pd.to_datetime(sales_df['timestamp'])
sales_df = sales_df.dropna(subset=['amount', 'product_id'])
sales_df['revenue'] = sales_df['amount'] * sales_df['price']

# Aggregate weekly revenue by product
weekly = (
    sales_df
    .set_index('date')
    .resample('W')
    .agg({'revenue': 'sum', 'product_id': 'nunique'})
)
```

This generates clean, aggregated data for dashboards or model inputs.

---

## 3. scikit-learn

### What is scikit-learn?

scikit-learn (`sklearn`) is a machine learning library with tools for:

- supervised and unsupervised learning algorithms
- data preprocessing and feature extraction
- model evaluation and selection
- pipelines for repeatable workflows

### Core Concepts

- `Estimator` APIs: `.fit()`, `.predict()`, `.transform()`
- model selection: `train_test_split`, `GridSearchCV`
- preprocessing: `StandardScaler`, `OneHotEncoder`
- evaluation: `accuracy_score`, `mean_squared_error`

### Example: Train a classification model

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report

# Example dataset
data = {
    'feature_a': [1.2, 2.4, 3.1, 4.0, 5.5, 6.1],
    'feature_b': [0.1, 0.3, 0.4, 0.6, 0.8, 1.0],
    'label': [0, 0, 1, 1, 1, 0]
}
df = pd.DataFrame(data)

X = df[['feature_a', 'feature_b']]
y = df['label']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

model = RandomForestClassifier(n_estimators=50, random_state=42)
model.fit(X_train_scaled, y_train)

predictions = model.predict(X_test_scaled)
print(classification_report(y_test, predictions))
```

### Real-time use case: Customer churn prediction

A common real-life task is predicting whether a customer will churn. The pipeline includes:

- loading customer activity data
- selecting features
- scaling numeric columns
- training and evaluating a classifier

```python
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.metrics import roc_auc_score

# Example feature columns
numeric_features = ['monthly_spend', 'avg_session_minutes']
category_features = ['contract_type', 'region']

preprocessor = ColumnTransformer(
    transformers=[
        ('num', StandardScaler(), numeric_features),
        ('cat', OneHotEncoder(handle_unknown='ignore'), category_features)
    ]
)

pipeline = Pipeline([
    ('preprocess', preprocessor),
    ('classifier', GradientBoostingClassifier(random_state=42))
])

# Fit and evaluate
pipeline.fit(X_train, y_train)
probabilities = pipeline.predict_proba(X_test)[:, 1]
print('ROC AUC:', roc_auc_score(y_test, probabilities))
```

This pattern is widely used for production-ready model training and scoring.

---

## 4. scikit-learn model list

### Supervised learning

- Linear models
  - `LinearRegression`
    - Definition: Predicts a continuous numeric target using a linear combination of features.
    - Why used: Simple, interpretable, and fast for regression tasks.
    - Real use: Forecasting monthly sales from advertising spend.
  - `LogisticRegression`
    - Definition: Predicts binary or multiclass outcomes using a logistic function.
    - Why used: Standard baseline for classification problems with probability output.
    - Real use: Predicting whether a customer will churn (yes/no).
  - `Ridge`, `Lasso`, `ElasticNet`
    - Definition: Regularized linear regression models that penalize large coefficients.
    - Why used: Reduce overfitting and handle correlated features.
    - Real use: Predicting house prices with many input features.

- Support Vector Machines
  - `SVC`, `LinearSVC`
    - Definition: Classification models that separate classes by the widest possible margin.
    - Why used: Effective on medium-sized datasets and in high-dimensional space.
    - Real use: Spam detection or image classification when classes are separable.
  - `SVR`, `LinearSVR`
    - Definition: Regression models that fit a function within a margin of tolerance.
    - Why used: Robust to outliers and useful for regression with a clear margin.
    - Real use: Estimating energy consumption from temperature and usage patterns.

- Tree-based models
  - `DecisionTreeClassifier`, `DecisionTreeRegressor`
    - Definition: Models that make decisions by splitting features into a tree of rules.
    - Why used: Interpretable and able to capture nonlinear relationships.
    - Real use: Classifying loan applications as approved or rejected.
  - `RandomForestClassifier`, `RandomForestRegressor`
    - Definition: Ensembles of decision trees trained on random subsets of data.
    - Why used: Reduce overfitting and improve accuracy on tabular data.
    - Real use: Predicting credit risk or product demand.
  - `GradientBoostingClassifier`, `GradientBoostingRegressor`
    - Definition: Sequentially builds trees where each tree corrects previous errors.
    - Why used: High accuracy for structured data and competitive in applied ML.
    - Real use: Forecasting customer lifetime value or churn probability.
  - `HistGradientBoostingClassifier`, `HistGradientBoostingRegressor`
    - Definition: Faster gradient boosting with histogram-based binning.
    - Why used: Handles large datasets efficiently and scales better than classic GBDT.
    - Real use: Large-scale pricing or recommendation systems.

- Ensemble methods
  - `AdaBoostClassifier`, `AdaBoostRegressor`
    - Definition: Boosting method that combines weak learners into a stronger model.
    - Why used: Improves performance on hard-to-classify examples.
    - Real use: Fraud detection where rare cases need extra weight.
  - `BaggingClassifier`, `BaggingRegressor`
    - Definition: Trains multiple models on bootstrap samples and averages their outputs.
    - Why used: Reduces variance and improves stability.
    - Real use: Reducing overfitting in noisy financial prediction data.

- Neighbors
  - `KNeighborsClassifier`, `KNeighborsRegressor`
    - Definition: Instance-based models that predict based on the closest training examples.
    - Why used: Simple, non-parametric, and good for small datasets with local patterns.
    - Real use: Recommending similar products based on past user behavior.

- Naive Bayes
  - `GaussianNB`, `MultinomialNB`, `BernoulliNB`
    - Definition: Probabilistic classifiers assuming feature independence.
    - Why used: Fast, easy to train, and effective for text or categorical data.
    - Real use: Email spam filtering or sentiment analysis on reviews.

- Neural network models
  - `MLPClassifier`, `MLPRegressor`
    - Definition: Feedforward neural networks with one or more hidden layers.
    - Why used: Model complex nonlinear relationships when simple models underperform.
    - Real use: Predicting insurance risk from mixed numeric and categorical inputs.

### Unsupervised learning

- Clustering
  - `KMeans`, `MiniBatchKMeans`
    - Definition: Partition data into `k` clusters based on distance to cluster centers.
    - Why used: Group similar records without labels.
    - Real use: Customer segmentation for targeted marketing.
  - `DBSCAN`
    - Definition: Density-based clustering that finds core samples and noise.
    - Why used: Detect clusters of varying shape and identify outliers.
    - Real use: Finding abnormal network traffic patterns.
  - `AgglomerativeClustering`
    - Definition: Hierarchical clustering that merges closest clusters iteratively.
    - Why used: Reveals nested cluster relationships and tree structure.
    - Real use: Organizing products into hierarchical categories.

- Dimensionality reduction
  - `PCA`
    - Definition: Projects data onto orthogonal components that explain variance.
    - Why used: Reduce features, speed up training, and visualize high-dimensional data.
    - Real use: Compressing sensor data for anomaly detection.
  - `TruncatedSVD`
    - Definition: Similar to PCA but optimized for sparse matrices.
    - Why used: Reduce dimensions in text or recommendation data.
    - Real use: Topic modeling with TF-IDF text features.
  - `KernelPCA`
    - Definition: Nonlinear dimensionality reduction using kernel functions.
    - Why used: Capture nonlinear structure before clustering or classification.
    - Real use: Embedding complex image features for downstream models.

- Decomposition
  - `FactorAnalysis`
    - Definition: Explains observed variables through fewer latent factors.
    - Why used: Discover hidden structure in noisy data.
    - Real use: Analyzing survey responses or psychological test scores.
  - `NMF`
    - Definition: Factorizes data into non-negative matrices.
    - Why used: Produces interpretable, additive components.
    - Real use: Topic extraction from document-term matrices.

- Anomaly detection
  - `IsolationForest`
    - Definition: Detects outliers by isolating anomalies in random trees.
    - Why used: Works well for high-dimensional data and unusual samples.
    - Real use: Detecting fraudulent transactions or sensor failures.
  - `LocalOutlierFactor`
    - Definition: Uses local density to identify points that differ from neighbors.
    - Why used: Good for finding anomalies in clustered data.
    - Real use: Identifying defective products in manufacturing sensor streams.

### Model selection and preprocessing

- `train_test_split`
  - Definition: Splits data into training and testing subsets.
  - Why used: Evaluate model generalization on unseen data.
  - Real use: Reserving 20-30% of customer data for final validation.
- `GridSearchCV`, `RandomizedSearchCV`
  - Definition: Hyperparameter search using cross-validation.
  - Why used: Find the best model settings without manual trial and error.
  - Real use: Tuning tree depth and learning rate for gradient boosting.
- `cross_val_score`, `cross_validate`
  - Definition: Evaluates model performance across multiple folds.
  - Why used: Obtain robust estimates and avoid lucky splits.
  - Real use: Comparing classifiers for credit scoring.
- `Pipeline`, `ColumnTransformer`
  - Definition: Combine preprocessing and modeling into a single workflow.
  - Why used: Prevent data leakage and keep transformations consistent.
  - Real use: Encoding categorical features and scaling numeric features before training.
- `StandardScaler`, `MinMaxScaler`, `OneHotEncoder`
  - Definition: Transformers that scale numeric data or encode categorical variables.
  - Why used: Normalize features and convert categories into model-ready numeric arrays.
  - Real use: Scaling transaction amounts and encoding product categories for a churn model.

---

## 5. How they work together

NumPy arrays are the data engine under the hood. Pandas uses NumPy data structures for DataFrames and Series. scikit-learn accepts NumPy arrays or Pandas DataFrames for model training and prediction.

Typical workflow:

1. Load raw data with Pandas
2. Clean and explore with Pandas
3. Convert to NumPy arrays if needed
4. Preprocess and train models with scikit-learn
5. Evaluate and export predictions

---

## 5. Practical tips

- Use `pd.read_csv()` for fast CSV loading.
- Keep data in a DataFrame while cleaning, then switch to arrays for model input.
- Use `StandardScaler` for algorithms sensitive to scale, like SVM or logistic regression.
- Use `Pipeline` to avoid leakage and keep preprocessing consistent.
- Always validate model performance with a held-out test set or cross-validation.

---

## 6. Interview questions

### Basics

1. What is the difference between `NumPy` and `Pandas`?
   - Answer: NumPy provides fast numerical arrays and math operations. Pandas builds on NumPy and adds labeled data structures (`Series`, `DataFrame`) and data manipulation tools.

2. What is a `DataFrame`?
   - Answer: A DataFrame is a two-dimensional labeled data structure in Pandas, similar to a table with rows and columns.

3. How do you handle missing values in Pandas?
   - Answer: Use `.dropna()` to remove missing rows or `.fillna()` to replace missing values with a default value or aggregator.

4. What does `StandardScaler` do in scikit-learn?
   - Answer: `StandardScaler` standardizes features by removing the mean and scaling to unit variance, which helps many models converge faster and perform better.

5. How do you convert a Pandas DataFrame to a NumPy array?
   - Answer: Use `df.values` or `df.to_numpy()`.

### Medium

1. How do you merge two DataFrames in Pandas?
   - Answer: Use `pd.merge(df1, df2, on='key')` or `df1.merge(df2, on='key')` with join types like `inner`, `left`, `right`, or `outer`.

2. What is broadcasting in NumPy?
   - Answer: Broadcasting allows NumPy to perform arithmetic operations on arrays with different shapes by expanding the smaller array along dimensions where needed.

3. What is the purpose of `train_test_split`?
   - Answer: It splits data into separate training and testing sets to evaluate model performance on unseen data and avoid overfitting.

4. Explain the `Pipeline` class in scikit-learn.
   - Answer: `Pipeline` chains preprocessing steps and an estimator into a single object to ensure consistent application of transformations during training and prediction.

5. How can you compute summary statistics grouped by a column in Pandas?
   - Answer: Use `df.groupby('column').agg({'metric': ['sum', 'mean', 'max']})` or apply functions after grouping.

### Hard

1. What are the differences between `fit`, `transform`, and `fit_transform`?
   - Answer: `fit` learns parameters from training data, `transform` applies those learned parameters to data, and `fit_transform` does both in one step.

2. How do you handle categorical variables before using them in a scikit-learn model?
   - Answer: Convert them to numeric form using encoders like `OneHotEncoder`, `OrdinalEncoder`, or use `pd.get_dummies()` before modeling.

3. How do you prevent data leakage in a machine learning pipeline?
   - Answer: Keep preprocessing steps within a pipeline so that transformations are fit only on training data, not the test set, and avoid using target-derived features.

4. How would you optimize hyperparameters in scikit-learn?
   - Answer: Use `GridSearchCV` or `RandomizedSearchCV` to search parameter combinations with cross-validation and select the best performing model.

5. How can you handle time-series data with Pandas?
   - Answer: Convert date columns to datetime, set them as index, use `.resample()` for frequency-based aggregation, and create lag or rolling features.

### Scenario-based questions

1. Scenario: You have sales data with missing values in the `price` and `quantity` columns. What would you do before modeling?
   - Answer: Inspect missing patterns, decide whether to drop or impute missing values based on business context, fill numeric fields with median or mean if appropriate, and then engineer features like `revenue = price * quantity`.

2. Scenario: A model is performing well on the training set but poorly on the test set. What could be wrong?
   - Answer: The model is likely overfitting. Review model complexity, reduce features, add regularization, use cross-validation, and ensure preprocessing is correctly separated from the test set.

3. Scenario: Data contains both numeric and categorical features for a customer churn model. How do you build a preprocessing pipeline?
   - Answer: Use `ColumnTransformer` with `StandardScaler` for numeric features and `OneHotEncoder` for categorical features, then build a `Pipeline` that includes preprocessing and a classifier.

4. Scenario: You need to report weekly revenue from daily transaction data. What Pandas steps do you take?
   - Answer: Convert timestamp to datetime, set it as the DataFrame index, resample by week with `.resample('W')`, and aggregate revenue with `.sum()`.

5. Scenario: Your dataset is large and training is slow. What can you do with NumPy and Pandas to improve performance?
   - Answer: Use vectorized NumPy and Pandas operations instead of loops, reduce memory usage by using appropriate dtypes, and process only required columns or sample data when prototyping.

---

## 7. Further reading

- NumPy: https://numpy.org/
- Pandas: https://pandas.pydata.org/
- scikit-learn: https://scikit-learn.org/
