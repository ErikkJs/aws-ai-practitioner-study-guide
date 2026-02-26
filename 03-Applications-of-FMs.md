# Domain 3: Applications of Foundation Models (28%)

[← Back to README](00-README.md) | [← Previous: Domain 2](02-Fundamentals-of-GenAI.md)

> **This is the highest-weighted domain on the exam. Study it thoroughly!**

---

## AWS AI/ML Services (Managed/Hosted)

### Language and NLP Services

| Service | Purpose | Remember As |
|---|---|---|
| **Amazon Comprehend** | NLP — sentiment analysis, key phrases, entity extraction, language detection | "Understand text" |
| **Amazon Transcribe** | **Speech-to-text** conversion | "Audio → Text" |
| **Amazon Polly** | **Text-to-speech** synthesis | "Text → Audio" |
| **Amazon Translate** | Neural machine translation between languages | "Language translation" |
| **Amazon Lex** | Build conversational **chatbots** (voice and text); powers Alexa | "Chatbot builder" |

#### Code: Amazon Comprehend — Sentiment Analysis

```python
# Detect sentiment of customer reviews using Amazon Comprehend
import boto3

comprehend = boto3.client("comprehend", region_name="us-east-1")

text = "I absolutely love this product! Best purchase I've ever made."

response = comprehend.detect_sentiment(
    Text=text,
    LanguageCode="en"
)

print(f"Text: {text}")
print(f"Sentiment: {response['Sentiment']}")
print(f"Scores: {response['SentimentScore']}")
# Output:
# Sentiment: POSITIVE
# Scores: {'Positive': 0.99, 'Negative': 0.00, 'Neutral': 0.01, 'Mixed': 0.00}
```

#### Code: Amazon Transcribe — Speech to Text

```python
# Transcribe an audio file stored in S3
import boto3

transcribe = boto3.client("transcribe", region_name="us-east-1")

# Start transcription job
transcribe.start_transcription_job(
    TranscriptionJobName="my-first-transcription",
    Media={"MediaFileUri": "s3://my-bucket/audio/meeting.mp3"},
    MediaFormat="mp3",
    LanguageCode="en-US",
    OutputBucketName="my-bucket"   # Where to save the transcript
)

print("Transcription job started!")
# The transcript will appear in your S3 bucket when complete
# It converts spoken audio → written text
```

#### Code: Amazon Rekognition — Image Analysis

```python
# Detect objects and labels in an image
import boto3

rekognition = boto3.client("rekognition", region_name="us-east-1")

response = rekognition.detect_labels(
    Image={
        "S3Object": {
            "Bucket": "my-bucket",
            "Name": "photos/beach.jpg"
        }
    },
    MaxLabels=5,
    MinConfidence=90  # Only return labels with >90% confidence
)

print("Detected objects:")
for label in response["Labels"]:
    print(f"  {label['Name']}: {label['Confidence']:.1f}% confidence")
# Output:
# Detected objects:
#   Beach: 99.2% confidence
#   Ocean: 98.7% confidence
#   Sand: 97.1% confidence
#   Sky: 96.3% confidence
#   Person: 94.5% confidence
```

#### Code: Amazon Textract — Extract Text from Documents

```python
# Extract text from a document (invoice, form, receipt)
import boto3

textract = boto3.client("textract", region_name="us-east-1")

response = textract.detect_document_text(
    Document={
        "S3Object": {
            "Bucket": "my-bucket",
            "Name": "documents/invoice.png"
        }
    }
)

# Print all detected text lines
print("Extracted text:")
for block in response["Blocks"]:
    if block["BlockType"] == "LINE":
        print(f"  {block['Text']}")
# Output:
# Extracted text:
#   Invoice #12345
#   Date: 2024-01-15
#   Total: $499.99
```

#### Code: Amazon Translate — Language Translation

```python
# Translate text between languages
import boto3

translate = boto3.client("translate", region_name="us-east-1")

response = translate.translate_text(
    Text="Hello, how are you today?",
    SourceLanguageCode="en",
    TargetLanguageCode="es"  # Spanish
)

print(f"Original:   {response['TranslatedText']}")
# Output: "Hola, ¿cómo estás hoy?"
```

#### Code: Amazon Polly — Text to Speech

```python
# Convert text to spoken audio
import boto3

polly = boto3.client("polly", region_name="us-east-1")

response = polly.synthesize_speech(
    Text="Welcome to Amazon Web Services!",
    OutputFormat="mp3",
    VoiceId="Joanna"  # US English female voice
)

# Save the audio to a file
with open("welcome.mp3", "wb") as f:
    f.write(response["AudioStream"].read())

print("Audio file saved as welcome.mp3")
```

### Vision and Document Services

| Service | Purpose | Remember As |
|---|---|---|
| **Amazon Rekognition** | Image/video analysis — faces, objects, text, content moderation | "See images/video" |
| **Amazon Textract** | **OCR** — extract text, tables, forms from documents | "Read documents" |

### Prediction and Recommendation Services

| Service | Purpose | Remember As |
|---|---|---|
| **Amazon Forecast** | **Time-series forecasting** (e.g., demand, sales) | "Predict the future" |
| **Amazon Personalize** | Real-time **recommendation engine** | "Recommend products" |
| **Amazon Fraud Detector** | Identify potentially fraudulent activity | "Catch fraud" |

### Search and Enterprise Services

| Service | Purpose | Remember As |
|---|---|---|
| **Amazon Kendra** | Intelligent **enterprise search** using NLP | "Smart search" |
| **Amazon QuickSight** | **Business intelligence** — interactive dashboards and visualizations | "BI dashboards" |

**Important distinction:** QuickSight is for **business data visualization** (sales, KPIs). CloudWatch Dashboard is for **AWS infrastructure monitoring** (server metrics, logs).

---

## Amazon Bedrock

### Overview
- Fully managed service providing access to **foundation models** from leading AI companies via **unified API**
- **Data security**: your data is NOT used to improve base FMs and is NOT shared with model providers
- Data always encrypted in transit and at rest (using AWS KMS)
- Supports AWS PrivateLink for private connectivity

### Foundation Model Providers on Bedrock

| Provider | Model(s) |
|---|---|
| **Anthropic** | Claude |
| **Amazon** | Titan (text, image, embeddings) |
| **Meta** | Llama |
| **Mistral AI** | Mistral |
| **Cohere** | Command, Embed |
| **AI21 Labs** | Jurassic |
| **Stability AI** | Stable Diffusion |

### Bedrock Pricing/Inference Options

| Option | Description |
|---|---|
| **On-Demand** | Pay-per-use based on input/output tokens; flexible |
| **Batch Inference** | Process multiple requests asynchronously; **50% cheaper** than on-demand |
| **Provisioned Throughput** | Dedicated capacity for predictable latency; **required for custom fine-tuned models** |

### Bedrock Key Features

| Feature | Description |
|---|---|
| **Knowledge Bases** | Managed RAG — integrates private data from S3 with vector databases for retrieval |
| **Agents** | Multi-step task automation — decompose user intent, invoke Lambda functions, use Kendra for search |
| **Guardrails** | Content filtering, PII redaction, topic blocking |
| **Model Evaluation** | Automatic and human-based evaluation of model outputs |
| **Fine-tuning** | Customize models with labeled data |
| **Continued Pre-training** | Expand model knowledge with unlabeled domain data |
| **Streaming** | Return results incrementally for real-time display |

#### Code: Bedrock invoke_model — Full Example

```python
# Invoke a foundation model on Amazon Bedrock
import boto3
import json

bedrock = boto3.client("bedrock-runtime", region_name="us-east-1")

# Using Claude on Bedrock
response = bedrock.invoke_model(
    modelId="anthropic.claude-3-haiku-20240307",
    body=json.dumps({
        "anthropic_version": "bedrock-2023-05-31",
        "max_tokens": 300,
        "temperature": 0.5,
        "messages": [
            {
                "role": "user",
                "content": "What are the 3 types of machine learning? Keep it brief."
            }
        ]
    })
)

result = json.loads(response["body"].read())
print(result["content"][0]["text"])
# Output: The three types of machine learning are:
# 1. Supervised Learning - uses labeled data...
# 2. Unsupervised Learning - finds patterns in unlabeled data...
# 3. Reinforcement Learning - learns through trial and error...
```

#### AWS CLI: Invoke a Bedrock Model

```bash
# Invoke a model from the command line
aws bedrock-runtime invoke-model \
    --model-id "anthropic.claude-3-haiku-20240307" \
    --body '{
        "anthropic_version": "bedrock-2023-05-31",
        "max_tokens": 200,
        "messages": [{"role": "user", "content": "What is AWS?"}]
    }' \
    --cli-binary-format raw-in-base64-out \
    output.json

# View the response
cat output.json | python3 -m json.tool
```

#### AWS CLI: List Available Bedrock Models

```bash
# See which foundation models are available in your region
aws bedrock list-foundation-models --query "modelSummaries[].{ID:modelId,Name:modelName,Provider:providerName}" --output table
```

### Model Selection Criteria

| Criteria | Consider |
|---|---|
| **Cost** | Subscription, compute, token pricing |
| **Modality** | Text, image, audio, multi-modal |
| **Latency** | Response speed requirements |
| **Model Size** | Complexity vs. performance trade-off |
| **Token Limits** | Input/output token constraints |

---

## Vector Databases on AWS

Used to store **embeddings** for similarity search in RAG workflows.

| Service | Key Feature |
|---|---|
| **Amazon OpenSearch** (incl. Serverless) | k-NN search for log analytics and vector similarity |
| **Aurora PostgreSQL** | pgvector extension for vector storage |
| **RDS PostgreSQL** | pgvector extension |
| **Amazon Neptune ML** | Graph Neural Networks for knowledge graphs |
| **Amazon MemoryDB** | High-speed vector storage with millisecond response |
| **Amazon DocumentDB** | Vector search with MongoDB compatibility |

---

## RAG Architecture on AWS

```
  RAG PIPELINE ON AWS

  ┌─────────────────────────────────────────────────────────────────┐
  │                    DATA INGESTION (ONE-TIME SETUP)               │
  │                                                                  │
  │  ┌────────┐    ┌──────────┐    ┌────────────┐    ┌───────────┐  │
  │  │ S3     │───▶│ Bedrock  │───▶│ Embedding  │───▶│ OpenSearch│  │
  │  │ Bucket │    │Knowledge │    │ Model      │    │ (Vector   │  │
  │  │(docs)  │    │ Base     │    │ (Titan)    │    │  DB)      │  │
  │  └────────┘    └──────────┘    └────────────┘    └───────────┘  │
  │   PDFs, docs    Orchestrates    Converts text      Stores       │
  │   CSVs, etc.    the pipeline    to vectors         embeddings   │
  └─────────────────────────────────────────────────────────────────┘

  ┌─────────────────────────────────────────────────────────────────┐
  │                    QUERY TIME (EVERY USER QUESTION)              │
  │                                                                  │
  │  ┌────────┐    ┌──────────┐    ┌────────────┐    ┌───────────┐  │
  │  │ User   │───▶│ Embed    │───▶│ Search     │───▶│ Combine   │  │
  │  │ Query  │    │ the      │    │ Vector DB  │    │ docs +    │  │
  │  │        │    │ query    │    │ for similar│    │ query     │  │
  │  └────────┘    └──────────┘    │ docs       │    └─────┬─────┘  │
  │                                └────────────┘          │        │
  │                                                        ▼        │
  │  ┌────────┐                                    ┌───────────┐    │
  │  │ Answer │◀───────────────────────────────────│ Bedrock   │    │
  │  │ + refs │                                    │ FM (Claude│    │
  │  └────────┘                                    │  etc.)    │    │
  │                                                └───────────┘    │
  └─────────────────────────────────────────────────────────────────┘
```

---

## Amazon SageMaker Services

### Data and Preparation

| Service | Purpose | Remember As |
|---|---|---|
| **SageMaker Data Wrangler** | Data preparation with **300+ built-in transformations** | "Clean and prep data" |
| **SageMaker Ground Truth** | **Human-in-the-loop** data labeling and annotation | "Label data" |
| **SageMaker Feature Store** | Centralized repository to store, share, and manage features | "Feature catalog" |
| **SageMaker Canvas** | **No-code** ML model building with visual interface | "No-code ML" |

### Model Development

| Service | Purpose | Remember As |
|---|---|---|
| **SageMaker Studio** | Web-based **IDE** for ML development | "ML development environment" |
| **SageMaker JumpStart** | ML hub with foundation models, built-in algorithms, prebuilt solutions | "Pre-built models hub" |
| **SageMaker Autopilot** | **AutoML** — automatically builds, trains, and tunes models; requires tabular data (CSV/Parquet) | "Automatic ML" |
| **SageMaker Clarify** | Detect and measure **bias** in datasets/models; model **explainability** | "Bias + explainability" |

### Training and Optimization

| Service | Purpose | Remember As |
|---|---|---|
| **SageMaker Training Jobs** | Managed infrastructure for model training | "Train models" |
| **SageMaker Debugger** | Real-time metrics and issue detection during training | "Debug training" |
| **SageMaker Experiments** | Track model versions, runs, and metrics | "Track experiments" |
| **Automatic Model Tuning (AMT)** | Hyperparameter optimization | "Auto-tune" |

### Deployment and Monitoring

| Service | Purpose | Remember As |
|---|---|---|
| **SageMaker Model Monitor** | Detect **data drift, bias, and quality issues** in production | "Monitor deployed models" |
| **SageMaker Model Dashboard** | Centralized portal to view/search all models | "Model catalog viewer" |
| **SageMaker Model Cards** | Standardized **model documentation** (intended use, metrics, ethics) | "Model documentation" |
| **SageMaker Model Registry** | **Version control** and approval workflow for models | "Model versioning" |
| **SageMaker Pipelines** | ML workflow automation and **CI/CD** for ML | "ML automation" |

#### Code: Deploy a SageMaker Model Endpoint

```python
# Deploy a pre-trained model from SageMaker JumpStart
import sagemaker
from sagemaker.jumpstart.model import JumpStartModel

# Initialize session
session = sagemaker.Session()
role = "arn:aws:iam::123456789012:role/SageMakerRole"

# Pick a pre-built model from JumpStart
model = JumpStartModel(
    model_id="huggingface-text2text-flan-t5-base",
    role=role
)

# Deploy to a real-time endpoint
predictor = model.deploy(
    initial_instance_count=1,
    instance_type="ml.m5.large"
)

# Make a prediction
response = predictor.predict("Translate to French: Hello, how are you?")
print(response)
# Output: "Bonjour, comment allez-vous?"

# IMPORTANT: Delete endpoint when done to stop charges!
predictor.delete_endpoint()
```

### SageMaker Inference Types

| Type | Use Case | Key Detail |
|---|---|---|
| **Real-time** | Persistent endpoints, low-latency | One prediction at a time |
| **Batch Transform** | Predictions for an entire dataset | Large-scale processing |
| **Asynchronous** | Large payloads (up to 1GB), long processing (up to 1 hour) | Queued processing |
| **Serverless** | Intermittent traffic, can tolerate cold starts | Auto-scales to zero |

```
  SAGEMAKER TRAINING PIPELINE

  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
  │ S3       │───▶│ Training │───▶│ Model    │───▶│ Model    │───▶│ Endpoint │
  │ (data)   │    │ Job      │    │ Artifact │    │ Registry │    │ (deploy) │
  └──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
  Training data   Managed compute  Saved to S3     Version control  Serves
  (CSV, images)   (pick instance   (.tar.gz)       & approval       predictions
                   type: ml.p4d,                    workflow
                   ml.trn1, etc.)

  Optional: SageMaker Pipelines automates this entire flow (CI/CD for ML)
```

### CloudFormation: Deploy a SageMaker Endpoint

```yaml
# CloudFormation template to deploy a SageMaker real-time endpoint
# Save as sagemaker-endpoint.yaml

AWSTemplateFormatVersion: "2010-09-09"
Description: Deploy a SageMaker inference endpoint

Resources:
  # IAM Role for SageMaker
  SageMakerRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: Allow
            Principal:
              Service: sagemaker.amazonaws.com
            Action: sts:AssumeRole
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/AmazonSageMakerFullAccess

  # Model definition
  MyModel:
    Type: AWS::SageMaker::Model
    Properties:
      ModelName: my-ml-model
      ExecutionRoleArn: !GetAtt SageMakerRole.Arn
      PrimaryContainer:
        Image: 763104351884.dkr.ecr.us-east-1.amazonaws.com/pytorch-inference:2.0-cpu-py310
        ModelDataUrl: s3://my-bucket/models/model.tar.gz

  # Endpoint configuration (instance type and count)
  EndpointConfig:
    Type: AWS::SageMaker::EndpointConfig
    Properties:
      ProductionVariants:
        - ModelName: !GetAtt MyModel.ModelName
          VariantName: primary
          InstanceType: ml.m5.large
          InitialInstanceCount: 1

  # The actual endpoint
  Endpoint:
    Type: AWS::SageMaker::Endpoint
    Properties:
      EndpointName: my-ml-endpoint
      EndpointConfigName: !GetAtt EndpointConfig.EndpointConfigName
```

```bash
# Deploy with AWS CLI
aws cloudformation deploy \
    --template-file sagemaker-endpoint.yaml \
    --stack-name my-sagemaker-stack \
    --capabilities CAPABILITY_IAM
```

---

## AWS Compute Instance Types for ML

| Instance | Chip/GPU | Best For |
|---|---|---|
| **Inf1 / Inf2** | AWS Inferentia | Cost-effective **inference** |
| **P4** | NVIDIA A100 | High-performance, low-latency inference and training |
| **G5** | NVIDIA GPU | GPU-based workloads |
| **Trn1** | AWS Trainium | Cost-effective **training** |
| **Graviton (C6g, M6g)** | ARM-based | Energy-efficient, general workloads |

> **Tip:** Remember **Inf** = **Inf**erence, **Trn** = **Tr**ai**n**ing

---

## Data Services

| Service | Purpose |
|---|---|
| **AWS Glue** | **ETL** (Extract, Transform, Load) processes |
| **AWS Glue DataBrew** | Visual, no-code data preparation with recipes |
| **AWS Glue Data Catalog** | Metadata catalog for data assets |
| **AWS Data Exchange** | Access **third-party datasets** |
| **AWS Macie** | Discover and protect **PII/sensitive data** in S3 |

---

## Amazon Q Family

| Service | Purpose |
|---|---|
| **Amazon Q Developer** | AI assistant for **coding, testing, upgrading applications** in IDEs |
| **Amazon Q Business** | Generative AI assistant for enterprise data (IT, HR, help desks); **powered by Amazon Bedrock** |
| **Amazon Q in QuickSight** | Generative BI assistant for dashboards with natural language |
| **Amazon Q in Connect** | Helps customer service agents in Amazon Connect |
| **Amazon Q Apps** | Build generative AI apps within Q Business using natural language |

### Amazon Q Business Controls
- Chat responses can use: **model knowledge + enterprise data**, OR **enterprise data only**
- Supports **topic-specific controls** for blocked topics
- Allows control over end-user file uploads
- Global controls for blocked phrases
- Encrypts data with **AWS KMS**

```
  AMAZON Q BUSINESS SETUP

  ┌───────────────────────────────────────────────────────┐
  │                  Amazon Q Business                     │
  │                                                        │
  │  ┌──────────────┐   ┌──────────────┐                  │
  │  │ Data Sources  │   │  Guardrails  │                  │
  │  │ ┌──────────┐ │   │ • Blocked    │                  │
  │  │ │ S3       │ │   │   topics     │                  │
  │  │ │ SharePnt │ │   │ • Blocked    │                  │
  │  │ │ Salesfrce│ │   │   phrases    │                  │
  │  │ │ Confluence│ │   │ • File upload│                  │
  │  │ └──────────┘ │   │   controls   │                  │
  │  └──────────────┘   └──────────────┘                  │
  │           │                                            │
  │           ▼                                            │
  │  ┌──────────────────────────────┐                     │
  │  │      Amazon Bedrock FM       │                     │
  │  │   (powers the AI assistant)  │                     │
  │  └──────────────────────────────┘                     │
  │           │                                            │
  │           ▼                                            │
  │  ┌──────────────────────────────┐                     │
  │  │    Employee Web Interface    │                     │
  │  │  "What's our PTO policy?"    │                     │
  │  │  "How do I file an expense?" │                     │
  │  └──────────────────────────────┘                     │
  └───────────────────────────────────────────────────────┘
```

---

## Console Walkthrough Descriptions

### What Each Service Looks Like in the AWS Console

| Service | Console Experience |
|---|---|
| **Bedrock** | Model playground for testing prompts; Knowledge Base wizard to connect S3 data; Guardrails configuration page with sliders for content filtering |
| **SageMaker Studio** | Jupyter-like IDE in the browser; left sidebar with experiments, models, endpoints; built-in terminal |
| **SageMaker Canvas** | Drag-and-drop interface; upload CSV, select target column, click "Build"; no code needed |
| **Comprehend** | Paste text in a box, click "Analyze," see sentiment/entities/key phrases highlighted |
| **Rekognition** | Upload an image, see bounding boxes around detected objects with confidence percentages |
| **Textract** | Upload a document, see extracted text with overlaid bounding boxes on the original |
| **Transcribe** | Upload audio or enter S3 path, get transcription job status, download text output |

---

## RLHF (Reinforcement Learning from Human Feedback)
- ML technique using human feedback to optimize models
- Incorporates human feedback in the rewards function
- Used throughout generative AI, including LLMs

## Amazon Augmented AI (A2I)
- Provides **human review of ML predictions**
- Guarantees precision through human input
- Different from RLHF: A2I is an **AWS service for review**; RLHF is a **training technique**

---

[Next: Domain 4 — Responsible AI →](04-Responsible-AI.md)
