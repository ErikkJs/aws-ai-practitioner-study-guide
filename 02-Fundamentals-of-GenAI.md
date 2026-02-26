# Domain 2: Fundamentals of Generative AI (24%)

[← Back to README](00-README.md) | [← Previous: Domain 1](01-Fundamentals-of-AI-and-ML.md)

---

## What is Generative AI?
- Creates new content (text, images, music) by **learning patterns from existing data**
- Uses algorithms to generate content that **mimics learned patterns**
- NOT random generation, NOT predefined rules, NOT manually coded
- Outputs are **non-deterministic** — same input can produce different outputs

---

## Key GenAI Terminology

### Tokens and Tokenization
- **Tokens** — the smallest processing units in LLMs (words, subwords, punctuation marks, or characters)
- **Tokenization** — the process of converting text into tokens for model processing
- Pricing for services like Amazon Bedrock is based on **token usage** (input + output tokens)

> **ELI5: What Are Tokens?**
>
> Tokens are like LEGO bricks for language. Instead of reading whole sentences,
> an AI breaks text into small pieces (tokens) and works with those.
>
> ```
> Sentence: "The cat sat on the mat"
> Tokens:   ["The", " cat", " sat", " on", " the", " mat"]
>            (6 tokens)
>
> Sentence: "unbelievable"
> Tokens:   ["un", "believ", "able"]
>            (3 tokens — long words get split into pieces!)
> ```
>
> Why does this matter? Because **you pay per token** on services like Bedrock.
> More tokens = more money. A typical word ≈ 1-2 tokens.

#### Code: Simple Tokenization Example

```python
# Showing how tokenization works conceptually
# (Real LLMs use more sophisticated tokenizers like BPE)

sentence = "Amazon Bedrock makes AI accessible"

# Simple word-level tokenization
tokens = sentence.split()
print(f"Text:   '{sentence}'")
print(f"Tokens: {tokens}")
print(f"Count:  {len(tokens)} tokens")
# Output:
# Text:   'Amazon Bedrock makes AI accessible'
# Tokens: ['Amazon', 'Bedrock', 'makes', 'AI', 'accessible']
# Count:  5 tokens

# Real tokenizers also handle subwords:
# "unbelievable" → ["un", "believ", "able"]
# "running" → ["run", "ning"]
```

### Context Window
- The **maximum number of tokens** an LLM can process at once (input + output combined)
- Larger context window = more information, more coherent answers
- Trade-off: larger windows require more memory, processing power, and cost

> **ELI5: What Is a Context Window?**
>
> Think of a context window as the AI's "working memory."
>
> If the context window is 4,000 tokens, the AI can "remember" about 3,000 words
> at a time (your question + its answer combined). If you paste a 100-page document,
> it won't fit — like trying to read a novel but you can only see 3 pages at a time.
>
> Bigger context window = more expensive but can handle longer documents.

### Embeddings
- **Numerical vector representations** of tokens that capture semantic meaning
- Each token is assigned a vector (list of numbers) representing its relationships with other tokens
- Stored in **vector databases** for semantic search and retrieval
- Similar meanings = vectors close together in high-dimensional space

#### Code: Creating Embeddings with Amazon Bedrock

```python
# Generate text embeddings using Amazon Bedrock
import boto3
import json

bedrock = boto3.client("bedrock-runtime", region_name="us-east-1")

# Create an embedding for a piece of text
response = bedrock.invoke_model(
    modelId="amazon.titan-embed-text-v2:0",
    body=json.dumps({
        "inputText": "What is machine learning?"
    })
)

result = json.loads(response["body"].read())
embedding = result["embedding"]

print(f"Embedding dimensions: {len(embedding)}")  # e.g., 1024
print(f"First 5 values: {embedding[:5]}")
# Output: [0.123, -0.456, 0.789, -0.012, 0.345]
# This list of numbers IS the meaning of the sentence in "math language"
```

```
  HOW EMBEDDINGS WORK (Simplified)

  Words plotted in "meaning space":

        happy ●               ● joyful
                  ● glad
                                    (similar meanings = close together)

        sad ●
              ● unhappy

        car ●    ● vehicle
                             (car/vehicle are close to each other,
                              but far from happy/sad)

  Each word becomes a list of numbers (a vector).
  "happy" → [0.9, 0.1, 0.0, ...]
  "joyful" → [0.85, 0.12, 0.01, ...]  ← similar numbers!
  "car" → [0.0, 0.0, 0.8, ...]        ← very different numbers
```

### Latent Space
- The **encoded knowledge and patterns** captured by a model
- Represents the model's internal understanding of data
- Used by VAEs to learn compact data representations

---

## Search Methods

| Keyword Search | Semantic Search |
|---|---|
| Matches **exact terms** | Understands **meaning** using embeddings |
| Simple string matching | Uses vector similarity |
| Fast but limited | More accurate for natural language |

---

## Large Language Models (LLMs)
- Very large deep learning models pre-trained on vast amounts of text data
- Used for: **generating human-like text, translating languages, summarizing text, answering questions, code generation**
- Examples: GPT-4, Claude, BERT, T5, Llama
- NOT designed for: speech synthesis, 3D model generation, video creation from text (those need specialized models)

## Multi-modal Models
- AI systems that work across **multiple data types** (text, images, audio, video)
- Can process and generate content in shared representation spaces
- Enable richer, more comprehensive outputs

---

## Generative AI Model Architectures

| Model Type | How It Works |
|---|---|
| **Transformer-based** | Uses **self-attention mechanism** to weigh importance of different parts of input; foundation of most modern LLMs |
| **Diffusion Models** | Create data by iteratively adding noise then learning to denoise; used for image generation |
| **GANs** | Two networks (generator + discriminator) trained competitively; generate realistic images |
| **VAEs (Variational Autoencoders)** | Learn a compact representation called **latent space**; use encoder + decoder networks |

```
  TRANSFORMER ARCHITECTURE (Simplified)

  Input: "The cat sat on the"
            │
            ▼
  ┌─────────────────────┐
  │    TOKENIZATION      │   Break text into tokens
  └──────────┬──────────┘
             ▼
  ┌─────────────────────┐
  │  EMBEDDING LAYER     │   Convert tokens to numbers (vectors)
  └──────────┬──────────┘
             ▼
  ┌─────────────────────┐
  │  SELF-ATTENTION      │   "Which words are related to which?"
  │  (The KEY innovation)│   The model learns that "cat" relates to
  │                      │   "sat" more than "the"
  └──────────┬──────────┘
             ▼
  ┌─────────────────────┐
  │  FEED-FORWARD        │   Process the relationships
  │  LAYERS              │
  └──────────┬──────────┘
             ▼
  ┌─────────────────────┐
  │  OUTPUT LAYER        │   Predict next token probabilities
  └──────────┬──────────┘
             ▼
  Output: "mat" (most likely next word)

  Self-attention = the model looks at ALL words in the input
  simultaneously, not one at a time like RNNs
```

---

## Foundation Models (FMs)
- AI model with a **large number of parameters** trained on **massive diverse data**
- Can generate a variety of responses for a wide range of use cases
- Can generate text, images, and embeddings
- Examples: Claude, Titan, Llama, Mistral

---

## GenAI Challenges

| Challenge | Description |
|---|---|
| **Hallucination** | Model produces plausible but **factually incorrect** information |
| **Toxicity** | Model generates potentially **offensive or harmful** outputs |
| **Non-determinism** | Same input can produce **different outputs** each time |
| **Interpretability** | "Black box" nature — hard to understand why model made a decision |
| **Bias** | Model reflects biases present in training data |

> **ELI5: What Is Hallucination?**
>
> When an AI "hallucinates," it confidently makes stuff up. It's like asking a friend
> who *always* gives an answer, even when they don't know. "Who wrote Hamlet?"
> → "Shakespeare" (correct!). "Who wrote the Garblex Protocol?" → "Dr. James Smith
> in 1987" (sounds real, but it's completely made up).
>
> **Why does this happen?** The model predicts the *most likely* next words. If it hasn't
> seen the right answer in training data, it generates something that *sounds* right.
>
> **How to reduce hallucination:** Use **RAG** (give the model real documents to reference)
> or lower the **temperature** setting to make outputs more conservative.

---

## Model Customization
- Process of using training data to **adjust model parameter values** to create a custom model
- Two methods in Amazon Bedrock:
  1. **Fine-tuning** — uses **labeled data** (inputs + corresponding outputs); adjusts model weights
  2. **Continued Pre-training** — uses **unlabeled data** (inputs only) to improve domain knowledge

### Fine-tuning Approaches

| Approach | Description |
|---|---|
| **Instruction-based fine-tuning** | Provide labeled task examples (instruction-response pairs) |
| **Domain-specific adaptation** | Tailor model to a specific field with domain data |
| **RLHF** | Use human feedback as reward signal to align model behavior |

### Catastrophic Forgetting
- Risk when fine-tuning on a **single task only**
- Model may **lose general-purpose capabilities** it learned during pre-training
- Mitigation: use diverse training data, multi-task fine-tuning

### Transfer Learning
- Taking a **pre-trained model** and fine-tuning it for a new, related task
- Leverages existing knowledge to improve performance on new problems
- Reduces need for large amounts of task-specific training data

---

## Improving FM Performance (Increasing Complexity)

```
Prompt Engineering → RAG → Fine-tuning
(Least complex)                (Most complex)
```

| Method | Complexity | Coding? | Changes Model? | Data Needed |
|---|---|---|---|---|
| **Prompt Engineering** | Low | None | No | None |
| **RAG** | Medium | Required | No | External documents |
| **Fine-tuning** | High | Data science expertise | Yes (weights change) | Labeled training data |

---

## Prompt Engineering Techniques

| Technique | Description |
|---|---|
| **Zero-shot** | No examples provided; relies on model pretraining |
| **One-shot** | Single example before the task |
| **Few-shot** | Multiple examples (2-5) to guide behavior |
| **Chain-of-thought** | Encourage step-by-step reasoning |
| **Prompt tuning** | Adjusting prompts iteratively for specific tasks |

### Side-by-Side: Zero-Shot vs. Few-Shot vs. Chain-of-Thought

```
┌─────────────────────────────────────────────────────────────────────────┐
│ ZERO-SHOT PROMPT                                                        │
│                                                                         │
│ Prompt:  "Classify this review as positive or negative:                 │
│           'The battery life is terrible and it keeps crashing.'"        │
│                                                                         │
│ Output:  "Negative"                                                     │
│                                                                         │
│ → No examples given. Model uses its training knowledge.                 │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│ FEW-SHOT PROMPT                                                         │
│                                                                         │
│ Prompt:  "Classify these reviews:                                       │
│           'Great product, love it!' → Positive                          │
│           'Broke after one day' → Negative                              │
│           'Works perfectly, fast shipping' → Positive                   │
│                                                                         │
│           Now classify:                                                  │
│           'The battery life is terrible and it keeps crashing.'"        │
│                                                                         │
│ Output:  "Negative"                                                     │
│                                                                         │
│ → 3 examples guide the model on format and task. More reliable.         │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│ CHAIN-OF-THOUGHT PROMPT                                                 │
│                                                                         │
│ Prompt:  "Classify this review as positive or negative.                 │
│           Let's think step by step.                                      │
│           Review: 'The battery life is terrible and it keeps crashing.'"│
│                                                                         │
│ Output:  "Let me analyze this step by step:                             │
│           1. 'battery life is terrible' — negative sentiment            │
│           2. 'keeps crashing' — negative sentiment                      │
│           3. No positive words found                                    │
│           4. Both statements express dissatisfaction                    │
│           Classification: Negative"                                     │
│                                                                         │
│ → Model shows its reasoning. Better for complex questions.              │
└─────────────────────────────────────────────────────────────────────────┘
```

### In-context Learning
- Enhancing model performance by **adding examples and data directly to prompts**
- The model adapts behavior based on context provided without retraining
- Few-shot prompting is a form of in-context learning

---

## Amazon Bedrock Inference Parameters

| Parameter | What It Does |
|---|---|
| **Temperature** | Controls creativity/randomness (0-1). Higher = more creative, lower = more deterministic |
| **Top K** | **Number** of most-likely candidates for next token. Higher = more diverse |
| **Top P** | **Percentage** of most-likely candidates for next token. Higher = less random |
| **Response Length (maxTokens)** | Min or max number of tokens in generated response |
| **Stop Sequences** | Character sequences that stop generation |
| **Penalties** | Discourage repeated sequences in output |

### Code: Calling Amazon Bedrock — Text Generation

```python
# Basic text generation with Amazon Bedrock using boto3
import boto3
import json

# Create the Bedrock Runtime client
bedrock = boto3.client("bedrock-runtime", region_name="us-east-1")

# Define the prompt and parameters
prompt = "Explain what machine learning is in 2 sentences."

request_body = {
    "anthropic_version": "bedrock-2023-05-31",
    "max_tokens": 200,
    "messages": [
        {
            "role": "user",
            "content": prompt
        }
    ],
    "temperature": 0.3,    # Low temperature = more focused/deterministic
    "top_p": 0.9           # Consider top 90% probable tokens
}

# Invoke the model
response = bedrock.invoke_model(
    modelId="anthropic.claude-3-haiku-20240307",
    body=json.dumps(request_body)
)

# Parse and print the response
result = json.loads(response["body"].read())
print(result["content"][0]["text"])
```

### JSON: Bedrock API Request/Response

```json
// REQUEST — What you send to Bedrock
{
    "anthropic_version": "bedrock-2023-05-31",
    "max_tokens": 200,
    "messages": [
        {
            "role": "user",
            "content": "What is cloud computing?"
        }
    ],
    "temperature": 0.7,
    "top_k": 50,
    "top_p": 0.9,
    "stop_sequences": ["\n\nHuman:"]
}

// RESPONSE — What Bedrock returns
{
    "id": "msg_01XYZ...",
    "type": "message",
    "role": "assistant",
    "content": [
        {
            "type": "text",
            "text": "Cloud computing is the delivery of computing services..."
        }
    ],
    "model": "anthropic.claude-3-haiku-20240307",
    "stop_reason": "end_turn",
    "usage": {
        "input_tokens": 14,
        "output_tokens": 87
    }
}
```

> **Tip:** Notice `usage.input_tokens` and `usage.output_tokens` in the response.
> This is how you get billed — you pay for BOTH the tokens you send AND the tokens you receive.

---

## RAG (Retrieval Augmented Generation)

- Fetches data from external sources to **enrich prompts** with relevant context
- Better results than prompt engineering, significantly **less hallucination**
- Best for: up-to-date information, proprietary data, frequently changing data
- Does NOT retrain the model — just augments the prompt

```
  RAG WORKFLOW

  ┌──────────┐     ┌──────────────────┐     ┌──────────────────┐
  │  User    │     │  1. RETRIEVE     │     │  2. AUGMENT      │
  │ Question │────▶│  Search your     │────▶│  Add retrieved   │
  │          │     │  knowledge base  │     │  docs to prompt  │
  └──────────┘     │  (vector DB)     │     │                  │
                   └──────────────────┘     └────────┬─────────┘
                                                      │
                                                      ▼
                   ┌──────────────────┐     ┌──────────────────┐
                   │  4. RETURN       │     │  3. GENERATE     │
                   │  Answer with     │◀────│  FM generates    │
                   │  source refs     │     │  answer using    │
                   │                  │     │  retrieved context│
                   └──────────────────┘     └──────────────────┘

  WHY RAG?
  - The FM doesn't "know" your company's internal docs
  - RAG lets you give it relevant docs at query time
  - No retraining needed — just plug in your data
  - Reduces hallucination because the model has real sources
```

### RAG vs. Agents (Amazon Bedrock)

| RAG | Agent |
|---|---|
| Queries and retrieves information from data sources to augment responses | Application that carries out orchestrations cyclically |
| Enriches prompts with external data | Interprets inputs and produces outputs using an FM |
| Best for: up-to-date information, reducing hallucination | Best for: carrying out customer requests, multi-step tasks |

---

## Model Evaluation

### Evaluation Methods

| Method | Best For | Key Facts |
|---|---|---|
| **Automatic Evaluation** | Quantitative assessment | Uses BERT Score, F1, ROUGE, BLEU; can use built-in OR custom datasets |
| **Human Evaluation** | Qualitative assessment | Assesses coherence, relevance, accuracy; must use **custom datasets only** |
| **Benchmark Datasets** | Standardized testing | Pre-compiled datasets (GLUE, SuperGLUE); least administrative effort |

### Specific Evaluation Metrics for GenAI

| Metric | Use Case |
|---|---|
| **ROUGE** | Text **summarization** quality |
| **BLEU** | **Translation** quality |
| **BERTScore** | **Semantic similarity** between generated and reference text |
| **Perplexity** | How well model predicts token sequences (lower = better) |

> **Tip: Memorize the metrics**
> - **ROUGE** = summarization (think: "ROUGE summarizes your makeup look")
> - **BLEU** = translation (think: "BLEU is French for blue — French = translation")
> - **BERTScore** = semantic similarity
> - **Perplexity** = lower is better (a confused model is "perplexed")

---

## Best Practices for Generative AI Adoption
- Implement **guardrails** and enhance **transparency**
- Clearly communicate that users are interacting with AI
- Continuously monitor and update models after deployment
- Consider ethical implications and potential biases

## Guardrails for Amazon Bedrock
- Filter undesirable and harmful content
- Redact personally identifiable information (PII)
- Implement safeguards based on responsible AI policies
- Controls interaction between users and FMs

## Watermark Detection for Amazon Bedrock
- Identifies images generated by Amazon Titan Image Generator
- Increases transparency around AI-generated content

## PartyRock
- **Amazon Bedrock playground** — a free tool to experiment with generative AI
- Build apps using foundation models without coding or an AWS account
- Good for learning and prototyping

---

[Next: Domain 3 — Applications of FMs →](03-Applications-of-FMs.md)
