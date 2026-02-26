# AWS Services Cheat Sheet

[← Back to README](00-README.md)

---

## AI/ML Services Quick Reference

| Service | One-Line Description | When Would I Use This? |
|---|---|---|
| **Amazon Bedrock** | Managed foundation model access via unified API | I want to use AI models (Claude, Titan, Llama) without managing infrastructure |
| **Amazon SageMaker** | Full ML platform (build, train, deploy, monitor) | I want to build, train, and deploy my own ML models |
| **Amazon QuickSight** | Business intelligence dashboards and visualizations | I want to create charts and dashboards from business data |
| **Amazon Kendra** | Intelligent enterprise search using NLP | I want employees to search company documents using natural language |
| **Amazon Rekognition** | Image and video analysis (faces, objects, text, moderation) | I want to analyze photos or videos automatically |
| **Amazon Comprehend** | NLP — sentiment, entities, key phrases, language detection | I want to understand and analyze text data |
| **Amazon Transcribe** | Speech-to-text conversion | I want to convert audio recordings to written text |
| **Amazon Polly** | Text-to-speech synthesis | I want to convert written text to spoken audio |
| **Amazon Translate** | Neural machine translation | I want to translate text between languages |
| **Amazon Lex** | Conversational chatbots (voice + text); powers Alexa | I want to build a chatbot |
| **Amazon Textract** | OCR — extract text, tables, forms from documents | I want to extract data from scanned documents, PDFs, images |
| **Amazon Forecast** | Time-series forecasting | I want to predict future values (sales, demand, inventory) |
| **Amazon Personalize** | Real-time recommendation engine | I want to recommend products, content, or actions to users |
| **Amazon Fraud Detector** | Identify fraudulent activity | I want to detect fake accounts or fraudulent transactions |
| **Amazon Q Developer** | AI coding assistant for IDEs | I want AI help writing, debugging, and upgrading code |
| **Amazon Q Business** | Enterprise AI assistant (powered by Bedrock) | I want employees to ask questions about company data |
| **Amazon A2I** | Human review of ML predictions | I want humans to verify ML predictions before acting |
| **Amazon Connect** | Contact center service | I want to run a cloud-based call center |
| **PartyRock** | Free Bedrock playground for experimentation | I want to try GenAI for free without an AWS account |

---

## SageMaker Sub-Services Quick Reference

| Service | Remember As | When Would I Use This? |
|---|---|---|
| **Canvas** | "No-code ML" | I want to build ML models without writing code |
| **Data Wrangler** | "300+ data transformations" | I want to clean and prepare data for ML |
| **Ground Truth** | "Human-in-the-loop labeling" | I need humans to label training data |
| **Clarify** | "Bias detection + explainability" | I want to check if my model or data is biased |
| **Feature Store** | "Feature repository" | I want to store and reuse features across ML projects |
| **JumpStart** | "Pre-built models hub" | I want to start with a pre-built model instead of building from scratch |
| **Autopilot** | "AutoML — automatic model building" | I want AWS to automatically build the best model from my data |
| **Studio** | "ML IDE" | I want a development environment for ML in the browser |
| **Model Monitor** | "Detect drift + bias in production" | I want to track if my deployed model is degrading over time |
| **Model Cards** | "Model documentation" | I want to document my model's purpose, performance, and limitations |
| **Model Registry** | "Model version control" | I want to track different versions of my model |
| **Model Dashboard** | "Model catalog viewer" | I want a single place to view all my models |
| **Pipelines** | "ML CI/CD automation" | I want to automate my ML workflow end-to-end |
| **Debugger** | "Training issue detection" | I want to find problems during model training |
| **Experiments** | "Track runs and metrics" | I want to compare different training runs |

---

## Security and Compliance Services

| Service | Remember As | When Would I Use This? |
|---|---|---|
| **AWS IAM** | "Who can do what" | I want to control who has access to what resources |
| **AWS KMS** | "Encryption keys" | I want to encrypt my data at rest |
| **AWS Macie** | "Find PII in S3" | I want to find sensitive data (SSNs, emails) in my S3 buckets |
| **AWS PrivateLink** | "Private connections" | I want to connect to AWS services without using the public internet |
| **Amazon GuardDuty** | "Threat detection" | I want to detect suspicious activity in my AWS account |
| **Amazon Inspector** | "Vulnerability scanning" | I want to find security vulnerabilities in my resources |
| **AWS Config** | "Resource configuration monitoring" | I want to check if my resources are configured correctly |
| **AWS Artifact** | "Compliance reports" | I need official AWS compliance reports (SOC, ISO, etc.) |
| **AWS Audit Manager** | "Audit evidence" | I want to automatically collect evidence for audits |
| **AWS CloudTrail** | "API activity logs" | I want to know who did what and when in my account |
| **AWS Trusted Advisor** | "Best practice recommendations" | I want AWS to check if I'm following best practices |

---

## Data Services

| Service | Remember As | When Would I Use This? |
|---|---|---|
| **AWS Glue** | "ETL processing" | I want to extract, transform, and load data between systems |
| **AWS Glue DataBrew** | "Visual data prep" | I want to clean data using a visual, no-code interface |
| **AWS Glue Data Catalog** | "Metadata catalog" | I want to catalog and organize my data assets |
| **AWS Data Exchange** | "Third-party datasets" | I want to buy or subscribe to third-party data |
| **Amazon S3** | "Data storage" | I want to store any type of file in the cloud |
| **Amazon OpenSearch** | "Vector search + analytics" | I want full-text search, log analytics, or vector similarity search |

---

## Problem-to-Service Mapping

> Use this table when the exam gives you a scenario and asks "which service?"

| Problem | AWS Service |
|---|---|
| Build ML models without code | **SageMaker Canvas** |
| Detect bias in data/models | **SageMaker Clarify** |
| Label training data | **SageMaker Ground Truth** |
| Prep/clean data for ML | **SageMaker Data Wrangler** |
| Access foundation models (Claude, Titan, etc.) | **Amazon Bedrock** |
| Pre-built ML algorithms/models | **SageMaker JumpStart** |
| Business dashboards (sales, KPIs) | **Amazon QuickSight** |
| AWS infrastructure monitoring | **Amazon CloudWatch** |
| Enterprise search over company docs | **Amazon Kendra** |
| Build a chatbot | **Amazon Lex** |
| Product recommendations | **Amazon Personalize** |
| Time-series forecasting | **Amazon Forecast** |
| Extract text from documents/images | **Amazon Textract** |
| Speech → text | **Amazon Transcribe** |
| Text → speech | **Amazon Polly** |
| Analyze text (sentiment, entities) | **Amazon Comprehend** |
| Analyze images/video | **Amazon Rekognition** |
| Translate languages | **Amazon Translate** |
| AI coding assistant | **Amazon Q Developer** |
| Enterprise AI assistant | **Amazon Q Business** |
| Compliance reports | **AWS Artifact** |
| Resource configuration checks | **AWS Config** |
| PII detection in S3 | **AWS Macie** |
| Threat detection | **Amazon GuardDuty** |
| Fraud detection | **Amazon Fraud Detector** |
| Human review of ML outputs | **Amazon A2I** |
| API activity auditing | **AWS CloudTrail** |
| Audit evidence collection | **AWS Audit Manager** |

---

[Next: Key Comparisons →](07-Key-Comparisons.md)
