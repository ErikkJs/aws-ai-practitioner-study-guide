# Domain 1: Fundamentals of AI and ML (20%)

[← Back to README](00-README.md)

---

## The AI Hierarchy

```
Artificial Intelligence (AI)
  └── Machine Learning (ML)
        └── Deep Learning (DL)
              └── Generative AI (GenAI)
                    └── Large Language Models (LLMs)
```

- **AI** — field of computer science focused on creating systems that mimic human intelligence
- **ML** — subset of AI; enables systems to learn from data without explicit programming
- **Deep Learning** — subset of ML; uses multi-layer neural networks for complex pattern recognition
- **Generative AI** — subset of DL; focused on creating novel content (text, images, audio)
- **LLMs** — type of GenAI; trained on vast text data to understand and generate human-like text

> **ELI5: The AI Family Tree**
>
> Think of it like Russian nesting dolls:
> - **AI** is the biggest doll — any computer that acts "smart"
> - **ML** is inside AI — the computer learns from examples instead of being told exact rules
> - **Deep Learning** is inside ML — the computer uses "brain-like" layers to learn really complex things
> - **GenAI** is inside DL — the computer can *create* new things (write stories, draw pictures)
> - **LLMs** are inside GenAI — GenAI that specifically works with text (like ChatGPT or Claude)

---

## When to Use / Avoid AI/ML

**Use AI/ML when:**
- Assisting decision-making at scale
- Automating repetitive tasks
- Pattern recognition in large datasets
- Predictions based on historical data

**Avoid AI/ML when:**
- Cost-benefit is unfavorable
- Deterministic solutions are needed (simple rules suffice)
- Highly regulated areas requiring full explainability
- Insufficient data available

---

## Three Main Types of Machine Learning

1. **Supervised Learning** — uses **labeled data** to make predictions or classify data
2. **Unsupervised Learning** — identifies patterns and relationships in **unlabeled data**
3. **Deep Learning** — uses **neural networks with many layers** to learn from large amounts of data

> **ELI5: Supervised vs. Unsupervised Learning**
>
> **Supervised** = Learning with a teacher. You show the computer pictures of cats and dogs,
> and you *tell* it which is which. After enough examples, it can identify new ones on its own.
>
> **Unsupervised** = Learning without a teacher. You dump a pile of animal photos on the table
> and say "sort these into groups." The computer figures out on its own that some look alike.
> It doesn't know the word "cat" — it just knows "these 50 photos look similar."

### Supervised vs. Unsupervised Learning

| Supervised Learning | Unsupervised Learning |
|---|---|
| Uses **labeled** data | Uses **unlabeled** data |
| Makes predictions/classifications | Finds hidden patterns/relationships |
| Algorithms: Linear Regression, Logistic Regression, Decision Trees, Random Forest, SVM, Naive Bayes, Neural Networks | Algorithms: K-Means, Hierarchical Clustering, PCA, t-SNE |

### Supervised Learning Algorithms

| Algorithm | Type | Use Case |
|---|---|---|
| **Linear Regression** | Regression | Predict numerical values (e.g., house prices) |
| **Logistic Regression** | Classification | Binary classification (e.g., spam detection) |
| **Decision Trees** | Both | Rule-based splitting; interpretable |
| **Random Forest** | Both | Ensemble of decision trees; more accurate |
| **SVM (Support Vector Machine)** | Classification | Find optimal class boundaries |
| **Naive Bayes** | Classification | Text classification, spam filtering |
| **Neural Networks** | Both | Complex pattern recognition |

#### Code: Simple Linear Regression

```python
# Linear Regression: Predict house prices from square footage
# This is SUPERVISED learning — we have labeled data (we know the prices)

from sklearn.linear_model import LinearRegression
import numpy as np

# Training data: [square feet] → price
square_feet = np.array([[600], [800], [1000], [1200], [1500]])
prices      = np.array([150000, 200000, 250000, 300000, 375000])

# Train the model
model = LinearRegression()
model.fit(square_feet, prices)  # "Learn" the relationship

# Predict: What would a 1100 sq ft house cost?
prediction = model.predict([[1100]])
print(f"Predicted price: ${prediction[0]:,.0f}")
# Output: Predicted price: $275,000
```

### Unsupervised Learning Methods

| Method | Purpose | Example |
|---|---|---|
| **K-Means Clustering** | Groups data into K clusters | Customer segmentation |
| **Hierarchical Clustering** | Nested grouping of data | Taxonomy creation |
| **PCA (Principal Component Analysis)** | Dimensionality reduction | Feature reduction, preprocessing |
| **t-SNE** | Dimensionality reduction | Data visualization |

#### Code: K-Means Clustering

```python
# K-Means Clustering: Group customers by spending behavior
# This is UNSUPERVISED learning — no labels, just finding patterns

from sklearn.cluster import KMeans
import numpy as np

# Customer data: [annual income ($K), spending score (1-100)]
customers = np.array([
    [15, 39], [16, 81], [17, 6],      # Low income
    [75, 72], [78, 73], [80, 75],      # High income, high spending
    [40, 20], [42, 18], [38, 22],      # Medium income, low spending
])

# Group into 3 clusters
kmeans = KMeans(n_clusters=3, random_state=42)
kmeans.fit(customers)

print("Cluster assignments:", kmeans.labels_)
# Output: Cluster assignments: [0 0 0 2 2 2 1 1 1]
# Translation: Customers are grouped into 3 segments automatically
```

### Semi-supervised Learning
- Combines supervised and unsupervised techniques
- Uses a small amount of labeled data + large amount of unlabeled data
- Example: **Sentiment analysis**

### Reinforcement Learning
- Trains software to make decisions that **maximize rewards** through trial and error
- Agent learns via environment interaction — receives rewards or penalties
- Use cases: **robotics, game playing, industrial automation**
- NOT for: clustering, regression, or predictions based on historical trends

> **ELI5: Reinforcement Learning**
>
> Think of training a dog. You don't show it 1000 examples of "sit."
> Instead, the dog tries things, and when it sits, you give it a treat (reward).
> When it does something wrong, no treat (penalty). Over time, it learns what earns treats.
> That's reinforcement learning — learning by trial and error with rewards.

---

## ML Algorithms

### K-Means vs. K-Nearest Neighbors (KNN)

| K-Means | KNN |
|---|---|
| **Unsupervised** learning | **Supervised** learning |
| Used for **clustering** | Used for **classification** |
| Groups data into clusters | Classifies based on proximity to labeled examples |
| Does NOT require labeled data | REQUIRES labeled data |

### ML Algorithm vs. ML Model
- **Algorithm** = set of mathematical instructions for solving a specific type of problem
- **Model** = the output of the algorithm **after being trained on data**

> **ELI5: Algorithm vs. Model**
>
> **Algorithm** = A recipe (instructions for baking a cake)
> **Model** = The actual cake you baked using that recipe and your specific ingredients (data)
>
> Same recipe + different ingredients = different cake. Same algorithm + different data = different model.

---

## Neural Networks

```
  NEURAL NETWORK ARCHITECTURE

  Input Layer      Hidden Layers       Output Layer
  (raw data)       (processing)        (predictions)

    ○─────────┐
              ├───○─────┐
    ○────┐    │         ├───○─────┐
         ├───○┤         │        ├───○  → Prediction
    ○────┘    │   ○─────┤        │
              ├───┘     ├───○────┘
    ○─────────┘         │
                  ○─────┘

  Data flows left → right
  Each connection has a "weight" that the network learns
  More layers = "deeper" network = "deep learning"
```

### Convolutional Neural Networks (CNNs)
- Designed for **image data** and **single image analysis**
- Uses convolutional layers to learn spatial hierarchies of features
- Best for: image recognition, image classification, object detection

```
  CNN ARCHITECTURE (Simplified)

  Input Image    Convolutional    Pooling      Fully        Output
  (pixels)       Layers           Layers       Connected

  ┌─────────┐   ┌─────────┐    ┌───────┐    ┌──────┐    ┌─────┐
  │ 🖼️      │   │ Feature │    │Smaller│    │Dense │    │ Cat │
  │ 224x224 │──▶│  Maps   │──▶│Feature│──▶│Layers│──▶│ Dog │
  │ pixels  │   │(edges,  │    │ Maps  │    │      │    │Bird │
  │         │   │shapes)  │    │       │    │      │    │     │
  └─────────┘   └─────────┘    └───────┘    └──────┘    └─────┘

  Step 1: Look      Step 2: Find    Step 3:     Step 4:      Step 5:
  at pixels         edges, shapes   Simplify    Combine      Classify
```

### Recurrent Neural Networks (RNNs)
- Designed for **sequential/temporal data**
- Best for: **video analysis**, time series, NLP tasks
- Maintains memory of previous inputs in the sequence

### Generative Adversarial Networks (GANs)
- Two neural networks trained in a **competitive manner** (generator vs. discriminator)
- Used for **generating new data** that resembles training data (e.g., realistic images)
- NOT designed for classification

### Deep Learning vs. Traditional ML

| Deep Learning | Traditional ML |
|---|---|
| Uses neural networks with many layers | Uses algorithms like decision trees, SVM |
| Automatically derives features from raw data | Requires **manual feature extraction** |
| Works well with large amounts of data | Can work with smaller datasets |
| Can be computationally intensive | Generally faster to train |
| Both can be used for supervised, unsupervised, and reinforcement learning |

---

## Data Types and Concepts

### Data Classifications

| Type | Description | Examples |
|---|---|---|
| **Structured** | Organized in rows and columns | Databases, spreadsheets, CSV |
| **Semi-structured** | Has some organizational properties | JSON, XML, HTML |
| **Unstructured** | No predefined format | Text, images, videos, audio |
| **Labeled** | Data with known output/target values | Used in supervised learning |
| **Unlabeled** | Data without known output values | Used in unsupervised learning |

### Data Splitting: Training, Validation, and Test Sets

| Set | Purpose | Typical Split |
|---|---|---|
| **Training** | Train the model — learns patterns | ~80% |
| **Validation** | Tune hyperparameters and model selection (optional) | ~10% |
| **Test** | Evaluate final model performance on unseen data | ~10% |

#### Code: Train/Test Split

```python
# Splitting data into training and testing sets
from sklearn.model_selection import train_test_split
import numpy as np

# Example: 100 data points with features and labels
X = np.random.rand(100, 3)  # 100 samples, 3 features each
y = np.random.randint(0, 2, 100)  # Binary labels (0 or 1)

# Split: 80% training, 20% testing
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

print(f"Training samples: {len(X_train)}")  # 80
print(f"Testing samples:  {len(X_test)}")   # 20
# The model trains on X_train/y_train
# Then we check how well it does on X_test/y_test (data it's never seen)
```

> **ELI5: Train/Test Split**
>
> Imagine studying for a test. Your textbook = training data. The actual exam = test data.
> If you only measure yourself on textbook problems you've already seen, you don't know
> if you *really* understand. The test set is like an exam with *new* questions to see if
> you actually learned the concepts — not just memorized the answers.

---

## Feature Engineering

### Feature Extraction vs. Feature Selection

| Feature Extraction | Feature Selection |
|---|---|
| **Transforms** data into a new feature space | **Selects** most relevant features from existing ones |
| Reduces features by creating new representations | Reduces features by choosing a subset |
| Example: PCA (Principal Component Analysis) | Example: Forward selection, backward elimination |
| Both reduce dimensionality; both work in supervised and unsupervised learning |

---

## ML Development Lifecycle (Full Pipeline)

```
  ML DEVELOPMENT LIFECYCLE

  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
  │ 1. Business  │───▶│ 2. Frame ML  │───▶│ 3. Collect   │
  │    Goal      │    │    Problem   │    │    Data      │
  └──────────────┘    └──────────────┘    └──────┬───────┘
                                                  │
                                                  ▼
  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
  │ 6. Feature   │◀───│ 5. Data      │◀───│ 4. EDA       │
  │  Engineering │    │ Preprocessing│    │ (Explore)    │
  └──────┬───────┘    └──────────────┘    └──────────────┘
         │
         ▼
  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
  │ 7. Train     │───▶│ 8. Tune      │───▶│ 9. Evaluate  │
  │    Model     │    │ Hyperparams  │    │    Model     │
  └──────────────┘    └──────────────┘    └──────┬───────┘
                                                  │
                                                  ▼
                      ┌──────────────┐    ┌──────────────┐
                      │ 11. Monitor  │◀───│ 10. Deploy   │
                      │  & Retrain   │    │    Model     │
                      └──────────────┘    └──────────────┘
```

1. **Identify Business Goal** — establish success metrics, align stakeholders
2. **Frame ML Problem** — define inputs, outputs, performance metrics
3. **Data Collection** — gather data from various sources
4. **Exploratory Data Analysis (EDA)** — understand data distributions and patterns
5. **Data Preprocessing** — clean, normalize, handle missing values, remove duplicates, anonymize PII
6. **Feature Engineering** — select, create, and transform relevant features
7. **Model Training** — train algorithm on preprocessed data
8. **Hyperparameter Tuning** — optimize learning rate, epochs, batch size, etc.
9. **Model Evaluation** — assess performance on test set
10. **Deployment** — deploy model for inference (real-time, batch, serverless)
11. **Monitoring** — track performance, detect data drift, set alerts

---

## Hyperparameters

| Hyperparameter | Description |
|---|---|
| **Epoch** | One complete pass through the entire training dataset. More epochs = better learning but risk of overfitting |
| **Batch Size** | Number of samples processed before updating model. Smaller = more frequent updates but noisier; Larger = stable but computationally expensive |
| **Learning Rate** | How much to adjust model weights per update. Higher = faster training but may skip optimal solutions; Lower = precise but slower |

---

## Bias and Variance

| Concept | Definition |
|---|---|
| **Bias** | Error from approximating a complex problem with a simpler model. High bias = **underfitting** |
| **Variance** | Error from model's sensitivity to fluctuations in training data. High variance = **overfitting** |

> **ELI5: Bias vs. Variance — The Dartboard Analogy**
>
> Imagine throwing darts at a dartboard:
>
> ```
>   High Bias,          Low Bias,           Low Bias,          High Bias,
>   Low Variance        High Variance       Low Variance       High Variance
>   (Underfitting)      (Overfitting)       (Just Right!)      (Worst Case)
>
>   ┌─────────┐        ┌─────────┐        ┌─────────┐        ┌─────────┐
>   │         │        │  x      │        │         │        │  x      │
>   │   xxx   │        │      x  │        │   x     │        │         │
>   │   xx●   │        │ x  ●   │        │    ●x   │        │      ●  │
>   │   x     │        │     x  │        │    x    │        │ x       │
>   │         │        │   x    │        │         │        │     x   │
>   └─────────┘        └─────────┘        └─────────┘        └─────────┘
>
>   Darts cluster       Darts scattered     Darts cluster      Darts scattered
>   but miss center     around center       right at center    AND miss center
> ```
>
> - **High Bias** = You consistently miss in the same direction (your aim is off)
> - **High Variance** = Your darts are scattered all over (inconsistent)
> - **Goal** = Low bias + low variance (accurate AND consistent)

**Key facts:**
- **Underfitting** = high bias, low variance — model too simple, misses patterns
- **Overfitting** = low bias, high variance — model too complex, memorizes noise
- It IS possible to have **both high bias AND high variance** (poorly designed model)
- More training reduces bias but can increase variance
- Goal: find the **sweet spot** between underfitting and overfitting
- Solutions for overfitting: simplify model, regularization, more data, cross-validation, dropout
- Solutions for underfitting: more complex model, more features, more training data

> **ELI5: Overfitting vs. Underfitting**
>
> **Overfitting** = A student who memorizes the textbook word-for-word but can't answer
> questions worded differently. They learned the *specific examples* instead of the *concepts*.
>
> **Underfitting** = A student who skimmed the textbook for 5 minutes. They don't know enough
> to answer any questions well.
>
> **Just right** = A student who understood the concepts and can apply them to new situations.

### Data Drift and Concept Drift
- **Data Drift** — the statistical properties of input data change over time (e.g., customer demographics shift)
- **Concept Drift** — the relationship between input and output changes over time (e.g., what defines "spam" evolves)
- Both require **continuous monitoring** and model retraining

### Cross-Validation
- Technique for evaluating model generalization by splitting data into multiple folds
- Each fold serves as the test set once while remaining folds serve as training data
- Helps detect overfitting and provides more reliable performance estimates

---

## Inference Types

### Real-time vs. Batch Inference

| Real-time Inference | Batch Inference |
|---|---|
| Immediate predictions, low latency | Processes large volumes at once, higher latency |
| **Synchronous** execution | **Asynchronous** execution |
| API-based invocation | Schedule-based/batch frameworks |
| Use cases: chatbots, fraud detection, recommendations | Use cases: ETL, periodic reports, data warehousing |

---

## Performance Metrics

### Confusion Matrix

|  | Predicted Positive | Predicted Negative |
|---|---|---|
| **Actual Positive** | True Positive (TP) | False Negative (FN) |
| **Actual Negative** | False Positive (FP) | True Negative (TN) |

```
  CONFUSION MATRIX — Visual

                    What the MODEL said
                    Positive    Negative
                  ┌───────────┬───────────┐
  What's    Pos   │    TP     │    FN     │
  ACTUALLY        │  "Got it  │ "Missed   │
  true            │   right"  │  one"     │
                  ├───────────┼───────────┤
            Neg   │    FP     │    TN     │
                  │  "False   │  "Got it  │
                  │   alarm"  │   right"  │
                  └───────────┴───────────┘

  Tip: The second word tells you what the MODEL predicted.
       TP = model said Positive, and it was True
       FP = model said Positive, but it was False (wrong!)
```

### Classification Metrics

| Metric | Formula | Description |
|---|---|---|
| **Accuracy** | (TP + TN) / Total | Overall correctness of predictions |
| **Precision** | TP / (TP + FP) | Accuracy of positive predictions |
| **Recall (Sensitivity/TPR)** | TP / (TP + FN) | Ability to find all positive instances |
| **F1-Score** | 2 x (Precision x Recall) / (Precision + Recall) | Harmonic mean of Precision and Recall |
| **Specificity (TNR)** | TN / (TN + FP) | Ability to correctly identify negatives |
| **False Positive Rate (FPR)** | FP / (FP + TN) | Rate of false alarms |
| **ROC Curve** | TPR vs FPR at various thresholds | Higher AUC = better model performance |
| **AUC** | Area Under ROC Curve | 1.0 = perfect; 0.5 = random guessing |

> **ELI5: Precision vs. Recall**
>
> Imagine you're searching for all the red M&Ms in a bag:
>
> **Precision** = Of the ones you *picked out*, how many are actually red?
> (Did you pick correctly? High precision = few mistakes in your picks)
>
> **Recall** = Of ALL the red M&Ms in the bag, how many did you *find*?
> (Did you find them all? High recall = you didn't miss many)
>
> You can have high precision but low recall (you only picked 3, but all 3 were red)
> Or high recall but low precision (you grabbed 50 M&Ms to make sure you got all 10 red ones)

#### Code: Calculate Precision, Recall, F1 from a Confusion Matrix

```python
# Given a confusion matrix, calculate metrics
# Scenario: Spam detection model tested on 100 emails

TP = 40  # Correctly identified spam
FP = 5   # Non-spam flagged as spam (false alarm)
FN = 10  # Spam that slipped through (missed)
TN = 45  # Correctly identified non-spam

# Calculate metrics
accuracy  = (TP + TN) / (TP + TN + FP + FN)
precision = TP / (TP + FP)
recall    = TP / (TP + FN)
f1_score  = 2 * (precision * recall) / (precision + recall)

print(f"Accuracy:  {accuracy:.2%}")   # 85.00%
print(f"Precision: {precision:.2%}")  # 88.89% - When we say "spam", we're right 89% of the time
print(f"Recall:    {recall:.2%}")     # 80.00% - We catch 80% of actual spam
print(f"F1-Score:  {f1_score:.2%}")   # 84.21% - Balance of precision and recall
```

### Regression Metrics (NOT for classification)

| Metric | Description |
|---|---|
| **MSE (Mean Squared Error)** | Average squared difference between predictions and actuals (lower = better) |
| **RMSE (Root Mean Squared Error)** | Square root of MSE; same-unit interpretation |
| **MAE (Mean Absolute Error)** | Average absolute difference |
| **R-squared** | Proportion of variance explained by the model |

### Other Metrics

| Metric | Description |
|---|---|
| **Perplexity** | How well a model predicts token sequences (lower = better) |
| **Average Response Time** | Runtime efficiency — how long model takes to generate prediction |

---

## NLP vs. Computer Vision

| NLP | Computer Vision |
|---|---|
| Analyzes and generates human language | Interprets visual information |
| Tasks: text analysis, translation, sentiment analysis, summarization | Tasks: image recognition, object detection, video analysis |
| Models: Transformers, BERT, RNNs | Models: CNNs, Object Detection networks |

---

## Decision Tree Diagram

```
  DECISION TREE — "Should I bring an umbrella?"

                    ┌──────────────────┐
                    │ Is it cloudy?    │
                    └────────┬─────────┘
                        ╱         ╲
                     Yes            No
                      ╱               ╲
            ┌────────────────┐   ┌──────────────┐
            │ Humidity > 70%?│   │ NO UMBRELLA  │
            └───────┬────────┘   └──────────────┘
                 ╱        ╲
              Yes           No
               ╱              ╲
     ┌──────────────┐   ┌──────────────┐
     │ BRING        │   │ NO UMBRELLA  │
     │ UMBRELLA     │   │              │
     └──────────────┘   └──────────────┘

  Each node = a question about a feature
  Each branch = a decision based on the answer
  Each leaf = a final prediction
  This is what makes decision trees "interpretable"
```

---

[Next: Domain 2 — Fundamentals of GenAI →](02-Fundamentals-of-GenAI.md)
