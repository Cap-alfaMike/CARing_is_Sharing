# 🚗 Car-ing is Sharing - Multi-Task LLM Chatbot Prototype

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![Transformers](https://img.shields.io/badge/Transformers-4.30+-orange.svg)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-red.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

**Multi-task chatbot prototype powered by Large Language Models (LLMs) for sentiment analysis, machine translation, question answering, summarization, and bias evaluation of automotive reviews.**

[Overview](#-overview) •
[Installation](#-installation) •
[Usage](#-usage) •
[Results](#-results) •
[Architecture](#-architecture)

</div>

---

# 📖 Overview

**Car-ing is Sharing** is a vehicle sales and rental company expanding its operations to the Spanish market.

This project implements a **multi-task chatbot prototype** using pretrained Hugging Face Transformer models to automate customer review processing, assisting both customers and human support agents.

## Features

- ✅ Sentiment Analysis
- 🌍 English → Spanish Translation
- ❓ Question Answering
- 📝 Automatic Summarization
- 🔍 Bias & Toxicity Analysis

---

# 🏗️ Architecture

## Models

| Task | Model | Framework | Evaluation |
|------|-------|-----------|------------|
| Sentiment Analysis | `distilbert-base-uncased-finetuned-sst-2-english` | Hugging Face Transformers | Accuracy, F1 |
| Translation | `Helsinki-NLP/opus-mt-en-es` | Hugging Face Transformers | BLEU |
| Question Answering | `deepset/minilm-uncased-squad2` | Hugging Face Transformers | Extractive QA |
| Summarization | `facebook/bart-large-cnn` | Hugging Face Transformers | Token Length |
| Toxicity | `evaluate/toxicity` | Hugging Face Evaluate | Max Toxicity |
| Regard | `evaluate/regard` | Hugging Face Evaluate | Max Regard |

---

## Processing Pipeline

```text
                     Car Reviews Dataset
                             │
                             ▼
                 ┌────────────────────────┐
                 │   Input Car Reviews    │
                 └────────────┬───────────┘
                              │
      ┌─────────────┬──────────┼────────────┬──────────────┐
      ▼             ▼          ▼            ▼
 Sentiment      Translation    QA      Summarization
 Classification   EN → ES    Pipeline      Pipeline
      │             │          │             │
      ▼             ▼          ▼             ▼
 Accuracy      BLEU Score   Answer       Summary
 F1 Score                    Extraction
      └─────────────┬──────────┴─────────────┘
                    ▼
             Bias Evaluation
        (Toxicity + Regard Score)
```

---

# 📊 Results

## 1. Sentiment Analysis

| Metric | Value |
|---------|------:|
| Accuracy | **0.800** |
| F1-Score | **0.857** |

### Prediction Distribution

- Positive Reviews: **4**
- Negative Reviews: **1**

---

## 2. Machine Translation (EN → ES)

| Metric | Value |
|---------|------:|
| BLEU Score | **0.753** |

### Example

**Original**

> I am very satisfied with my 2014 Nissan NV SL. I use this van for business deliveries and personal use.

**Translation**

> Estoy muy satisfecho con mi Nissan NV SL 2014. Uso esta camioneta para mis entregas de negocios y uso personal.

---

## 3. Question Answering

**Question**

> What did he like about the brand?

**Answer**

> ride quality, reliability

---

## 4. Summarization

- Summary Length: **55 tokens**
- Output satisfies the required **50–55 token** constraint.
- Grammatically coherent and complete.

---

## 5. Bias Analysis

| Metric | Value |
|---------|------:|
| Max Toxicity | **0.0001** |
| Max Regard | **0.756** |

Interpretation:

- Nearly zero toxicity.
- Positive and respectful language.

---

# 🚀 Installation

## Requirements

- Python 3.10+
- CUDA (optional)

## Clone Repository

```bash
git clone https://github.com/yourusername/caring-is-sharing-llm-prototype.git

cd caring-is-sharing-llm-prototype
```

## Create Virtual Environment

Linux / macOS

```bash
python -m venv venv

source venv/bin/activate
```

Windows

```powershell
python -m venv venv

venv\Scripts\activate
```

## Install Dependencies

```bash
pip install -r requirements.txt
```

## Run

```bash
python main.py
```

The required Hugging Face models will be downloaded automatically on first execution.

---

# 📦 Dependencies

```text
torch>=2.0.0
transformers>=4.30.0
pandas>=2.0.0
scikit-learn>=1.3.0
nltk>=3.8.0
evaluate>=0.4.0
```

---

# 💻 Usage

Run the entire pipeline:

```bash
python main.py
```

---

## Example Output

```text
============================================================

Predicted labels:
[{'label': 'POSITIVE', 'score': 0.929},
 {'label': 'POSITIVE', 'score': 0.865},
 ...]

Binary predictions:
[1, 1, 1, 0, 1]

Accuracy: 0.8
F1 Score: 0.8571428571428571

Translated review:

Estoy muy satisfecho con mi Nissan NV SL 2014.
Uso esta camioneta para mis entregas de negocios y uso personal.

BLEU:
0.7532821983227991

Question:
What did he like about the brand?

Answer:
ride quality, reliability

Summary:

The Nissan Rogue provides me with the desired SUV experience without burdening me with an exorbitant payment. Handling and styling are great; I have hauled 12 bags of mulch in the back with the seats down and could have held more. The engine delivers strong performance.

Max Toxicity:
0.00013837779988534749

Max Regard:
0.755964457988739
```

---

# 📁 Project Structure

```text
caring-is-sharing-llm-prototype/
│
├── data/
│   ├── car_reviews.csv
│   └── reference_translations.txt
│
├── main.py
├── requirements.txt
├── README.md
└── LICENSE
```

---

# 🔧 Implementation Details

## Performance Optimizations

- `torch.inference_mode()` for memory-efficient inference.
- Automatic GPU/CPU selection.
- Batch processing for sentiment analysis.
- Controlled generation using `min_new_tokens` and `max_new_tokens`.

---

## Data Processing

- Automatic removal of invalid labels.
- Robust indexing with `.iloc`.
- Sentence extraction using regular expressions.

---

# 🧪 Validation

## Metrics

- ✅ Accuracy (`sklearn.metrics`)
- ✅ F1 Score (`sklearn.metrics`)
- ✅ BLEU (`nltk`)
- ✅ Toxicity (`evaluate`)
- ✅ Regard (`evaluate`)

---

## Edge Cases Covered

- Missing labels.
- Invalid labels.
- Multi-sentence reviews.
- Truncated summaries (fixed using `early_stopping=False`).

---

# 📈 Roadmap

- Support additional languages (Portuguese, French, German).
- Fine-tune models with proprietary customer data.
- Develop a FastAPI REST API.
- Kubernetes deployment.
- Slack / Discord integration.
- Monitoring dashboard.

---

# 🤝 Contributing

Contributions are welcome.

```bash
# Fork repository

# Create feature branch
git checkout -b feature/AmazingFeature

# Commit changes
git commit -m "Add AmazingFeature"

# Push branch
git push origin feature/AmazingFeature

# Open Pull Request
```

---

# 📄 License

Distributed under the MIT License.

See the `LICENSE` file for details.

---

# 📧 Contact

**Developer:** Adalberto Correia

**LinkedIn:** https://www.linkedin.com/in/adalberto-correia-6597b134/

---

# 🙏 Acknowledgements

- Hugging Face Transformers
- PyTorch
- Hugging Face Evaluate
- Scikit-learn
- NLTK
- NVIDIA CUDA
