# Machine Learning Model Examples

## Who this guide is for
This guide is for beginners who want to see simple machine learning examples for finance, healthcare, images, and video.
It explains what each model does, why it is used, and how the code works.

## What you'll learn
- Basic ML problem types and model choices
- How to build and evaluate tabular, image, and video models
- How to think about model training and prediction


# 1. Banking: Credit Risk Classification

## Use Case
Predict whether a loan applicant is low or high risk based on financial features.

## Model Type
Supervised classification using Gradient Boosting or Logistic Regression.

## Python Example (scikit-learn)
```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.metrics import classification_report, roc_auc_score

# Load data
# Example columns: ['age', 'income', 'debt_ratio', 'credit_score', 'loan_amount', 'default']
data = pd.read_csv('banking_credit_data.csv')

# Features and target
y = data['default']
X = data[['age', 'income', 'debt_ratio', 'credit_score', 'loan_amount']]

# Train/test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Model training
model = GradientBoostingClassifier(n_estimators=100, learning_rate=0.1, max_depth=3, random_state=42)
model.fit(X_train, y_train)

# Predictions
preds = model.predict(X_test)
probs = model.predict_proba(X_test)[:, 1]

print(classification_report(y_test, preds))
print('ROC AUC:', roc_auc_score(y_test, probs))
```

## How It Works
- The model learns from labeled examples of past loan applications.
- Features like income, debt ratio, and credit score help predict default risk.
- Gradient Boosting builds many weak decision trees sequentially, correcting errors from prior trees.
- `predict_proba` returns a risk score, and a threshold can turn it into low/high risk.

---

# 2. Healthcare: Disease Prediction from Patient Data

## Use Case
Predict disease presence using clinical and demographic data.

## Model Type
Supervised classification using Random Forest or XGBoost.

## Python Example (scikit-learn)
```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import make_pipeline
from sklearn.metrics import accuracy_score, confusion_matrix

# Load patient data
# Example columns: ['age', 'bmi', 'blood_pressure', 'cholesterol', 'smoking_status', 'disease']
data = pd.read_csv('healthcare_patient_data.csv')

X = data[['age', 'bmi', 'blood_pressure', 'cholesterol', 'smoking_status']]
y = data['disease']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state=42)

pipeline = make_pipeline(
    StandardScaler(),
    RandomForestClassifier(n_estimators=200, max_depth=8, random_state=42)
)

pipeline.fit(X_train, y_train)
preds = pipeline.predict(X_test)

print('Accuracy:', accuracy_score(y_test, preds))
print('Confusion Matrix:\n', confusion_matrix(y_test, preds))
```

## How It Works
- Raw patient metrics are standardized before training.
- A Random Forest trains many decision trees on random subsets of data and features.
- Each tree votes, and the forest aggregates those votes for robust predictions.
- The confusion matrix helps measure true positives and false negatives, which are critical in healthcare.

---

# 3. Image: Object Classification

## Use Case
Classify images into categories such as dogs, cats, or medical scans.

## Model Type
Deep learning with Convolutional Neural Networks (CNNs).

## Python Example (TensorFlow/Keras)
```python
import tensorflow as tf
from tensorflow.keras import layers, models

# Example dataset: CIFAR-10 or custom image folder
(train_images, train_labels), (test_images, test_labels) = tf.keras.datasets.cifar10.load_data()
train_images, test_images = train_images / 255.0, test_images / 255.0

model = models.Sequential([
    layers.Conv2D(32, (3, 3), activation='relu', input_shape=(32, 32, 3)),
    layers.MaxPooling2D((2, 2)),
    layers.Conv2D(64, (3, 3), activation='relu'),
    layers.MaxPooling2D((2, 2)),
    layers.Conv2D(64, (3, 3), activation='relu'),
    layers.Flatten(),
    layers.Dense(64, activation='relu'),
    layers.Dense(10, activation='softmax')
])

model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

model.fit(train_images, train_labels, epochs=10, batch_size=64, validation_split=0.1)

loss, accuracy = model.evaluate(test_images, test_labels)
print('Test accuracy:', accuracy)
```

## How It Works
- Convolution layers scan image patches to learn spatial patterns like edges and textures.
- Max pooling reduces dimensionality and keeps important features.
- The model flattens feature maps into a vector, then dense layers learn higher-level category boundaries.
- Softmax outputs probability scores for each class.

---

# 4. Video: Action Recognition

## Use Case
Analyze video clips to identify activities such as walking, running, or gestures.

## Model Type
Temporal deep learning using 3D CNNs or sequence models.

## Python Example (PyTorch, simplified)
```python
import torch
import torch.nn as nn

class SimpleVideoModel(nn.Module):
    def __init__(self, num_classes=5):
        super(SimpleVideoModel, self).__init__()
        self.conv3d = nn.Conv3d(in_channels=3, out_channels=16, kernel_size=(3, 3, 3), padding=1)
        self.pool = nn.MaxPool3d((1, 2, 2))
        self.fc = nn.Linear(16 * 8 * 16 * 16, num_classes)

    def forward(self, x):
        x = self.conv3d(x)
        x = torch.relu(x)
        x = self.pool(x)
        x = x.view(x.size(0), -1)
        x = self.fc(x)
        return x

# Example input shape: (batch, channels, frames, height, width)
video_batch = torch.randn(4, 3, 8, 64, 64)
model = SimpleVideoModel(num_classes=5)
outputs = model(video_batch)
print(outputs.shape)  # -> torch.Size([4, 5])
```

## How It Works
- 3D convolutions process both spatial and temporal dimensions simultaneously.
- The model learns motion patterns across multiple frames instead of a single image.
- Output logits map to action categories, and a softmax can convert them into probabilities.
- This type of model is useful for surveillance, sports analytics, and medical video review.

---

# 5. Model Development Workflow

1. Define the problem and collect labeled data.
2. Clean and preprocess features or images/videos.
3. Choose a model architecture: classical ML for tabular data, CNN for images, 3D or sequence models for video.
4. Train on a training set and validate on held-out data.
5. Evaluate metrics: accuracy, ROC AUC, confusion matrix, precision/recall.
6. Deploy the model with a REST API, batch pipeline, or edge service.

# 6. Notes and Best Practices

- Banking and healthcare models require strong data governance and fairness checks.
- Always monitor for bias, especially in finance and medical domains.
- Use explainability tools like SHAP or LIME for tabular risk and disease models.
- For images and video, augment training data and use transfer learning when data is limited.
- Keep sensitive data secure and follow compliance rules such as GDPR and HIPAA.
