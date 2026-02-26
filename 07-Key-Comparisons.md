# Key Comparisons to Memorize

[← Back to README](00-README.md)

> The exam **loves** testing whether you know the difference between similar concepts.
> Study each comparison below until you can recall it instantly.

---

## 1. Prompt Engineering vs. RAG vs. Fine-tuning

```
Complexity:    Low ---------> Medium ---------> High
               Prompt Eng.   RAG               Fine-tuning
Coding:        None          Required          Required (data science)
Data:          None          External docs     Labeled training data
Changes Model: No            No                Yes (weights change)
```

> **Memory Trick:** "**P**rompt engineering is **P**lain (no code, no data).
> **R**AG **R**etrieves documents. **F**ine-tuning **F**undamentally changes the model."

> **Trap:** RAG does NOT change the model. It only adds context to the prompt.
> If the exam says "without modifying model weights," both Prompt Engineering
> and RAG are valid — but RAG uses external data while Prompt Engineering doesn't.

---

## 2. CNNs vs. RNNs

```
CNNs = Images (single image analysis)
RNNs = Sequences (video analysis, time series, NLP)
```

> **Memory Trick:** **C**NNs see **C**lear pictures. **R**NNs **R**emember sequences.

> **Trap:** Video analysis = **RNN** (not CNN), because video is a *sequence* of frames.
> Single image analysis = CNN. The exam uses "video" as a trick to see if you pick CNN.

---

## 3. K-Means vs. KNN

```
K-Means = Unsupervised, Clustering, No labels needed
KNN     = Supervised, Classification, Labels required
```

> **Memory Trick:** K-**Means** **M**ixes unlabeled data into groups.
> K-**N**earest **N**eighbors **N**eeds labeled neighbors to classify.

> **Trap:** They sound almost identical! The exam counts on you mixing them up.
> Key difference: K-Means = unsupervised (no labels); KNN = supervised (needs labels).

---

## 4. Overfitting vs. Underfitting

```
Overfitting  = High variance, low bias, too complex
Underfitting = High bias, low variance, too simple
Both high    = Possible, leads to poor performance
```

> **Memory Trick:** **O**verfitting = **O**ver-studied (memorized answers, can't generalize).
> **U**nderfitting = **U**nder-studied (didn't learn enough).

> **Trap:** The exam may state "both high bias and high variance" and ask if it's possible.
> Answer: **YES**, it is possible (and it's the worst case).

---

## 5. Classification vs. Regression Metrics

```
Classification: Precision, Recall, F1, Accuracy, AUC/ROC
Regression:     MAE, RMSE, MSE, R-squared
GenAI:          ROUGE (summarization), BLEU (translation), BERTScore (similarity), Perplexity
```

> **Memory Trick:**
> - **C**lassification metrics have **C**onfusion matrix relatives (Precision, Recall)
> - **R**egression metrics have **R**eal number errors (MAE, RMSE, MSE)
> - **ROUGE** = su**R**marization (**R**!)
> - **BLEU** = trans**L**ation (B**L**EU sounds French = translation)

> **Trap:** If the exam asks about measuring *summarization* quality, the answer is
> ROUGE (automated) or Human Evaluation (qualitative). Never RMSE or MSE — those
> are for regression (predicting numbers), not text quality.

---

## 6. Bedrock Inference Parameters

```
Temperature    = Creativity (0-1); higher = more creative
Top K          = NUMBER of candidates
Top P          = PERCENTAGE of candidates
Response Length = Token count (min/max)
Stop Sequences = Characters that halt generation
Penalties      = Discourage repetition
```

> **Memory Trick:** **K** = **K**ount (number). **P** = **P**ercentage.
> Temperature is like a creativity dial — turn it up for creative writing, down for factual answers.

> **Trap:** The exam will try to swap Top K and Top P definitions. Remember:
> K = how many candidates (a count), P = what percentage of probability mass.

---

## 7. Real-time vs. Batch Inference

```
Real-time = Synchronous, low latency, immediate
Batch     = Asynchronous, high throughput, delayed, cheaper (50% off on Bedrock)
```

> **Memory Trick:** **R**eal-time = **R**ight now. **B**atch = **B**ig pile, later.

> **Trap:** Bedrock batch inference is **50% cheaper** than on-demand. The exam
> loves this specific number.

---

## 8. Human vs. Automatic Model Evaluation (Bedrock)

```
Human:     Qualitative, custom datasets ONLY
Automatic: Quantitative, built-in OR custom datasets, BERT/F1/ROUGE/BLEU scores
```

> **Memory Trick:** **H**uman evaluation needs **H**andmade (custom) datasets.
> Automatic evaluation can use **A**ny dataset (built-in or custom).

> **Trap:** Human evaluation CANNOT use built-in datasets. Only custom datasets.
> The exam tests this specific limitation.

---

## 9. AWS Config vs. Artifact vs. Audit Manager vs. CloudTrail

```
Config:        Monitors resource CONFIGURATIONS
Artifact:      Access compliance REPORTS/AGREEMENTS (+ ISV reports)
Audit Manager: Automates AUDIT evidence collection
CloudTrail:    Logs API ACTIVITY (who did what)
```

> **Memory Trick:**
> - **Config** = **Config**urations (is encryption turned on?)
> - **Artifact** = **Art**ifacts/Reports (show me the SOC 2 report)
> - **Audit** Manager = **Audit** evidence (collect proof for auditors)
> - Cloud**Trail** = **Trail** of activity (who walked through here?)

> **Trap:** "Compliance reports" = Artifact, NOT Config. Config checks if
> resources are configured correctly, but doesn't produce compliance reports.
> Also: Trusted Advisor gives best practice recommendations, NOT compliance reports.

---

## 10. Security Attacks

```
Hijacking:        AI manipulated to misbehave (good answer then bad)
Jailbreaking:     Bypassing safety restrictions
Prompt Injection:  Embedding malicious instructions in prompts
Exposure:          Leaking sensitive/confidential data
Data Poisoning:    Corrupting training data
```

> **Memory Trick:**
> - **H**ijacking = **H**ostage-taking (AI forced to serve bad actor)
> - **J**ailbreaking = **J**ump over the rules
> - **P**rompt **I**njection = **P**oison **I**n the prompt
> - **E**xposure = **E**xposing secrets
> - **D**ata **P**oisoning = **D**irty **P**ractice on training data

> **Trap:** Prompt Injection vs. Jailbreaking — they're similar but different.
> Jailbreaking = making the model ignore its own rules.
> Prompt Injection = putting hidden instructions in the input to manipulate output.

---

## 11. Bedrock Pricing

```
On-Demand:            Pay per token, flexible, variable workloads
Batch:                50% cheaper, async processing, large datasets
Provisioned:          Dedicated capacity, predictable latency, required for fine-tuned models
```

> **Memory Trick:** **O**n-demand = **O**ccasional use. **B**atch = **B**udget option (50% off).
> **P**rovisioned = **P**redictable performance (and required for **P**ersonalized/fine-tuned models).

> **Trap:** Fine-tuned custom models REQUIRE Provisioned Throughput. They cannot
> use On-Demand. The exam tests this.

---

## 12. SageMaker Inference Types

```
Real-time:    Persistent endpoint, low latency, one prediction at a time
Batch:        Entire dataset, large-scale
Async:        Large payloads (1GB), long processing (1 hour)
Serverless:   Auto-scale to zero, intermittent traffic, tolerates cold starts
```

> **Memory Trick:** Think of a restaurant:
> - **Real-time** = Ordering at the counter (immediate, one at a time)
> - **Batch** = Catering order (cook everything at once for an event)
> - **Async** = Take-out order (large order, pick up later)
> - **Serverless** = Food truck (only shows up when there are customers)

---

## 13. AWS AI Services — Speech & Text

```
Transcribe = Speech → Text
Polly      = Text → Speech
Comprehend = Understand Text (NLP)
Translate  = Language → Language
Lex        = Chatbot (powers Alexa)
Textract   = Document → Structured Data (OCR)
```

> **Memory Trick:**
> - **Trans**cribe = **Trans**late audio → text
> - **Polly** = **Polly** wants a cracker (the parrot *speaks*)
> - **Comprehend** = **Comprehend**ing text (understanding it)
> - **Lex** = A**lex**a (chatbot)
> - **Textract** = e**xtract** from text/documents

> **Trap:** Transcribe = audio → text. Polly = text → audio. They are the reverse of
> each other. The exam WILL try to swap them.

---

## 14. Bias Types

```
Human Bias:       Personal beliefs in feature selection
Algorithmic Bias: Historical biases in training data
Data Bias:        Non-representative training data
Sampling Bias:    Over/under-representation of groups
Prejudice Bias:   Cultural prejudices in data
```

> **Memory Trick:** The bias types flow from source:
> 1. A **human** picks bad features → Human Bias
> 2. The **data collected** is skewed → Sampling/Data Bias
> 3. The **data content** has prejudices → Prejudice Bias
> 4. The **algorithm** learns the biases → Algorithmic Bias

---

## 15. Data Concepts

```
Data Residency:    WHERE data is stored (geography)
Data Logging:      WHO accessed data and WHEN
Data Lineage:      WHERE data came from and HOW it changed
Data Cataloging:   WHAT data exists (metadata)
```

> **Memory Trick:**
> - **R**esidency = **R**egion (physical location)
> - **L**ogging = **L**og book (access diary)
> - **L**ineage = **L**ine of descent (family tree of data)
> - **C**ataloging = **C**atalog (library index)

> **Trap:** Data Lineage vs. Data Logging. Lineage tracks where data *came from*
> and how it was transformed. Logging tracks who *accessed* it and when.

---

## 16. QuickSight vs. CloudWatch

```
QuickSight  = Business BI dashboards (sales, revenue, KPIs)
CloudWatch  = AWS infrastructure monitoring (CPU, memory, errors)
```

> **Trap:** When the exam says "dashboard," check the context carefully.
> Business metrics = QuickSight. Server/infrastructure metrics = CloudWatch.

---

## 17. SageMaker Canvas vs. Clarify

```
Canvas  = Build ML models (no-code, visual interface)
Clarify = Detect bias + explain model decisions (NOT for building models)
```

> **Trap:** Clarify does NOT build models. It analyzes them for bias and explainability.
> Canvas builds models. Don't mix them up.

---

## 18. A2I vs. RLHF

```
A2I  = AWS SERVICE for human review of predictions (post-deployment)
RLHF = TRAINING TECHNIQUE using human feedback (during training)
```

> **Trap:** Both involve humans, but at different stages.
> A2I = reviewing outputs. RLHF = improving training.

---

[Next: Exam Tips →](08-Exam-Tips.md)
