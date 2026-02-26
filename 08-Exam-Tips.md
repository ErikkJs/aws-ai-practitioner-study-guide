# Exam Tips and Strategies

[← Back to README](00-README.md) | [← Previous: Key Comparisons](07-Key-Comparisons.md)

---

## General Test-Taking

1. **Read carefully**: Many wrong answers simply swap definitions between two concepts
2. **"Always" and "never" statements** are usually wrong
3. You have ~83 seconds per question (90 min / 65 questions)
4. 15 questions are unscored — don't waste time guessing which ones
5. Flag uncertain questions and return to them

---

## Key Facts to Memorize

1. **Complexity order**: Prompt Engineering < RAG < Fine-tuning
2. **Bedrock data privacy**: Your data is NEVER used to improve base models, NEVER shared with providers
3. **Shared Responsibility**: AWS = infrastructure security; You = data + access controls
4. **Regional vs. Global**: IAM, CloudFront, Route 53, WAF are GLOBAL; most others are REGIONAL
5. **Bias-Variance**: Underfit = high bias; Overfit = high variance; both can be high simultaneously
6. **Human evaluation** is best for subjective quality assessment of text outputs
7. **Benchmark datasets** are the lowest-effort way to evaluate for bias
8. **SageMaker Canvas** = no-code ML; **SageMaker Clarify** = bias detection (NOT for building models)
9. **Batch inference on Bedrock** = 50% cheaper than on-demand
10. **Provisioned Throughput** is required for fine-tuned/custom models on Bedrock
11. **Catastrophic forgetting** happens when fine-tuning on a single task
12. **AWS Config** = resource configurations; **CloudTrail** = API activity; **Artifact** = compliance reports
13. **ISO 42001** = AI management system standard (AWS was first major cloud provider certified)
14. **QuickSight** = business BI dashboards; **CloudWatch** = AWS infrastructure monitoring
15. **Top K** = number of candidates; **Top P** = percentage of candidates

---

## Service Selection Decision Flowchart

```
  START: What do you need to do?
  │
  ├── Work with AI/ML models?
  │   ├── Use pre-built AI models (no training)?
  │   │   ├── Text analysis (sentiment, entities) → Amazon Comprehend
  │   │   ├── Image/video analysis → Amazon Rekognition
  │   │   ├── Speech → text → Amazon Transcribe
  │   │   ├── Text → speech → Amazon Polly
  │   │   ├── Translation → Amazon Translate
  │   │   ├── Document OCR → Amazon Textract
  │   │   ├── Chatbot → Amazon Lex
  │   │   ├── Recommendations → Amazon Personalize
  │   │   ├── Forecasting → Amazon Forecast
  │   │   └── Fraud detection → Amazon Fraud Detector
  │   │
  │   ├── Use foundation models (GenAI)?
  │   │   ├── Via API (managed) → Amazon Bedrock
  │   │   ├── Free playground → PartyRock
  │   │   └── Enterprise Q&A assistant → Amazon Q Business
  │   │
  │   └── Build/train your own models?
  │       ├── No code needed → SageMaker Canvas
  │       ├── AutoML → SageMaker Autopilot
  │       ├── Full control (IDE) → SageMaker Studio
  │       └── Start with pre-built → SageMaker JumpStart
  │
  ├── Manage data?
  │   ├── Clean/transform data → SageMaker Data Wrangler or Glue DataBrew
  │   ├── ETL pipelines → AWS Glue
  │   ├── Label training data → SageMaker Ground Truth
  │   ├── Find PII in S3 → AWS Macie
  │   ├── Third-party data → AWS Data Exchange
  │   └── Catalog data → AWS Glue Data Catalog
  │
  ├── Monitor & evaluate?
  │   ├── Detect bias → SageMaker Clarify
  │   ├── Monitor deployed models → SageMaker Model Monitor
  │   ├── Document models → SageMaker Model Cards
  │   ├── Human review → Amazon A2I
  │   └── Business dashboards → Amazon QuickSight
  │
  └── Security & compliance?
      ├── Access control → AWS IAM
      ├── Encryption → AWS KMS
      ├── Threat detection → Amazon GuardDuty
      ├── Compliance reports → AWS Artifact
      ├── Audit evidence → AWS Audit Manager
      ├── API logs → AWS CloudTrail
      ├── Resource config → AWS Config
      └── Private connectivity → AWS PrivateLink
```

---

## Service Selection Strategy (Quick Reference)

- **Need to build ML models without code?** → SageMaker Canvas
- **Need to detect bias?** → SageMaker Clarify
- **Need to label data?** → SageMaker Ground Truth
- **Need to prep/clean data?** → SageMaker Data Wrangler
- **Need foundation model access?** → Amazon Bedrock
- **Need pre-built algorithms?** → SageMaker JumpStart
- **Need business dashboards?** → Amazon QuickSight
- **Need enterprise search?** → Amazon Kendra
- **Need chatbot?** → Amazon Lex
- **Need recommendations?** → Amazon Personalize
- **Need forecasting?** → Amazon Forecast
- **Need document OCR?** → Amazon Textract
- **Need speech-to-text?** → Amazon Transcribe
- **Need text-to-speech?** → Amazon Polly
- **Need text analysis/NLP?** → Amazon Comprehend
- **Need image analysis?** → Amazon Rekognition
- **Need AI coding assistant?** → Amazon Q Developer
- **Need enterprise AI assistant?** → Amazon Q Business
- **Need compliance reports?** → AWS Artifact
- **Need resource config monitoring?** → AWS Config
- **Need PII detection?** → AWS Macie
- **Need threat detection?** → Amazon GuardDuty

---

## Practice Question Format Examples

### Format 1: Multiple Choice (Single Answer)

> **Question:** A company wants to analyze customer support emails to understand
> sentiment trends. Which AWS service should they use?
>
> A. Amazon Rekognition
> B. Amazon Comprehend
> C. Amazon Textract
> D. Amazon Translate
>
> **Answer:** B — Amazon Comprehend performs NLP including sentiment analysis.
> Rekognition is for images. Textract is for document OCR. Translate is for language translation.

### Format 2: Multiple Response (Select 2-3)

> **Question:** A team wants to improve a foundation model's accuracy for their
> industry domain. Which TWO approaches could they use? (Select TWO)
>
> A. Increase the model's temperature parameter
> B. Fine-tune the model with industry-specific labeled data
> C. Use RAG with industry documents
> D. Switch from On-Demand to Provisioned Throughput
>
> **Answer:** B and C — Fine-tuning adjusts model weights with labeled data.
> RAG augments prompts with external documents. Temperature affects randomness,
> not accuracy. Provisioned Throughput affects performance, not model quality.

### Format 3: Ordering

> **Question:** Arrange these model improvement techniques from LEAST to MOST complex:
>
> 1. Fine-tuning
> 2. Prompt Engineering
> 3. RAG
>
> **Answer:** 2 → 3 → 1 (Prompt Engineering → RAG → Fine-tuning)

### Format 4: Matching

> **Question:** Match each metric to its use case:
>
> | Metric | Use Case |
> |---|---|
> | ROUGE | ? |
> | BLEU | ? |
> | RMSE | ? |
> | F1-Score | ? |
>
> **Answer:**
> - ROUGE → Text summarization quality
> - BLEU → Translation quality
> - RMSE → Regression accuracy
> - F1-Score → Classification balance of precision/recall

### Format 5: Case Study

> **Question:** A hospital wants to extract patient information from handwritten
> intake forms, check the data for PII, store it securely, and use an AI model
> to suggest preliminary diagnoses.
>
> Which combination of services addresses ALL requirements?
>
> A. Textract → Macie → S3 (KMS encrypted) → Bedrock
> B. Comprehend → Macie → S3 → SageMaker
> C. Rekognition → Config → S3 → Bedrock
> D. Textract → GuardDuty → S3 → Comprehend
>
> **Answer:** A — Textract extracts text from documents (OCR). Macie detects PII.
> S3 with KMS provides secure storage. Bedrock provides AI model access for diagnosis.

---

## Day-Before-Exam Checklist

- [ ] Review [Key Comparisons](07-Key-Comparisons.md) — especially traps
- [ ] Skim [Services Cheat Sheet](06-AWS-Services-Cheat-Sheet.md)
- [ ] Re-read the 15 Key Facts to Memorize (above)
- [ ] Review the Service Selection Flowchart
- [ ] Get a good night's sleep
- [ ] Remember: 65 questions, 90 minutes, 700/1000 to pass

---

## Official Practice Resources

| Resource | Description |
|---|---|
| [AWS Exam Guide (AIF-C01)](https://docs.aws.amazon.com/aws-certification/latest/examguides/ai-practitioner-01.html) | Official exam blueprint and domain breakdown |
| [AWS Skill Builder](https://explore.skillbuilder.aws/learn) | Free and paid exam prep courses |
| [Official Practice Exam](https://explore.skillbuilder.aws/learn/courses/external/view/elearning/18744/aws-certified-ai-practitioner-official-practice-question-set) | Free 20-question practice set |
| [AWS Ramp-Up Guide: AI/ML](https://d1.awsstatic.com/training-and-certification/ramp-up_guides/Ramp-Up_Guide_Machine_Learning.pdf) | Curated learning path for AI/ML |
| [Amazon Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/) | Official Bedrock documentation |
| [PartyRock](https://partyrock.aws/) | Free hands-on experimentation with GenAI |

---

*Sources: [AWS Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/examguides/ai-practitioner-01.html) | [GitHub Study Notes](https://github.com/vicsz/aif-c01-study-notes) | [DEV.to Guide](https://dev.to/franciscojeg78/aws-certified-ai-practitioner-aif-c01-study-guide-1cd6) | [Tutorials Dojo](https://tutorialsdojo.com/aws-certified-ai-practitioner-aif-c01-exam-guide/) | [The Line Tech Cheat Sheet](https://thelinetech.uk/aws-certified-ai-practitioner-cheat-sheet/)*
