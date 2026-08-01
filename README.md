# 🚗 Car-ing is Sharing - Multi-Task LLM Chatbot Prototype

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-red.svg)
![Transformers](https://img.shields.io/badge/🤗%20Transformers-4.30+-orange.svg)
![Evaluate](https://img.shields.io/badge/HuggingFace-Evaluate-yellow.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

**Multi-task Large Language Model (LLM) prototype for sentiment analysis, translation, question answering, summarization, and bias evaluation of automotive reviews.**

</div>

---

# 📖 Overview

**Car-ing is Sharing** is a fictional vehicle sales and rental company expanding its services to the Spanish-speaking market.

This project demonstrates how multiple pretrained **Hugging Face Transformers** models can be orchestrated into a single NLP pipeline capable of automatically processing customer reviews.

The prototype performs:

- 🚗 Sentiment Analysis
- 🌎 English → Spanish Translation
- ❓ Question Answering
- 📝 Automatic Summarization
- 🔍 Bias Evaluation (Toxicity & Regard)

The project was developed entirely using pretrained models without additional fine-tuning.

---

# ✨ Features

- Binary sentiment classification
- Neural machine translation
- Extractive Question Answering
- Abstractive text summarization
- Toxicity detection
- Regard analysis
- Automatic evaluation metrics

---

# 🏗️ Architecture

## Models

| Task | Model |
|------|------|
| Sentiment Analysis | `distilbert-base-uncased-finetuned-sst-2-english` |
| Translation | `Helsinki-NLP/opus-mt-en-es` |
| Question Answering | `deepset/minilm-uncased-squad2` |
| Summarization | `facebook/bart-large-cnn` |
| Toxicity | Hugging Face Evaluate |
| Regard | Hugging Face Evaluate |

---

## Processing Pipeline

```text
                     Car Reviews Dataset
                             │
                             ▼
                 ┌─────────────────────────┐
                 │ Customer Review Dataset │
                 └─────────────┬───────────┘
                               │
        ┌──────────────┬────────┼─────────┬──────────────┐
        ▼              ▼        ▼         ▼
 Sentiment        Translation   QA    Summarization
 Classification    EN → ES            (BART)
        │              │        │         │
        ▼              ▼        ▼         ▼
 Accuracy         BLEU Score  Answer   Summary
 F1 Score
                       │
                       ▼
             Bias Evaluation
      (Toxicity + Regard Metrics)
```

---

# 📊 Results

The following results were obtained using the provided dataset.

---

## Sentiment Analysis

### Predictions

```text
POSITIVE
POSITIVE
POSITIVE
NEGATIVE
POSITIVE
```

### Binary Predictions

```text
[1, 1, 1, 0, 1]
```

### Performance

| Metric | Value |
|---------|------:|
| Accuracy | **0.800** |
| F1 Score | **0.8571** |

---

## Machine Translation

### Input

```text
I am very satisfied with my 2014 Nissan NV SL.
I use this van for business deliveries and personal use.
```

### Output

```text
Estoy muy satisfecho con mi Nissan NV SL 2014.
Uso esta camioneta para mis entregas de negocios y uso personal.
```

### BLEU Evaluation

| Metric | Value |
|---------|------:|
| BLEU Score | **0.7794** |

Detailed BLEU dictionary:

```python
{
    'bleu': 0.7794483794144497,
    'precisions': [
        0.9090909090909091,
        0.8571428571428571,
        0.75,
        0.631578947368421
    ],
    'brevity_penalty': 1.0,
    'length_ratio': 1.0476190476190477,
    'translation_length': 22,
    'reference_length': 21
}
```

---

## Question Answering

### Question

```text
What did he like about the brand?
```

### Answer

```text
ride quality, reliability
```

---

## Summarization

Generated summary:

```text
The Nissan Rogue provides me with the desired SUV experience without burdening me with an exorbitant payment. Handling and styling are great; I have hauled 12 bags of mulch in the back with the seats down and could have held more. The engine delivers strong performance.
```

Generation settings:

| Parameter | Value |
|-----------|------|
| min_new_tokens | 50 |
| max_new_tokens | 55 |
| num_beams | 4 |
| do_sample | False |
| length_penalty | 2.0 |

---

## Bias Evaluation

| Metric | Value |
|---------|------:|
| Max Toxicity | **0.000138** |
| Max Regard | **0.755964** |

Interpretation:

- Extremely low toxicity
- Positive and respectful language

---

# ⚙️ Installation

## Requirements

- Python 3.10+
- CUDA (optional)

---

## Clone Repository

```bash
git clone https://github.com/yourusername/caring-is-sharing-llm-prototype.git

cd caring-is-sharing-llm-prototype
```

---

## Create Virtual Environment

Linux/macOS

```bash
python -m venv venv

source venv/bin/activate
```

Windows

```powershell
python -m venv venv

venv\Scripts\activate
```

---

## Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Run

```bash
python main.py
```

The pretrained models are automatically downloaded during the first execution.

---

# 📦 Dependencies

```text
torch>=2.0.0
transformers>=4.30.0
evaluate>=0.4.0
pandas>=2.0.0
scikit-learn>=1.3.0
nltk>=3.8.0
```

---

# 💻 Example Output

```text
============================================================

Predicted labels:
[{'label': 'POSITIVE', 'score': 0.929397702217102},
 {'label': 'POSITIVE', 'score': 0.8654274940490723},
 {'label': 'POSITIVE', 'score': 0.9994640946388245},
 {'label': 'NEGATIVE', 'score': 0.9935314059257507},
 {'label': 'POSITIVE', 'score': 0.9986565113067627}]

Binary predictions:
[1, 1, 1, 0, 1]

Accuracy:
0.8

F1 Score:
0.8571428571428571

Translated review:
Estoy muy satisfecho con mi Nissan NV SL 2014.
Uso esta camioneta para mis entregas de negocios y uso personal.

BLEU Score:
0.7794483794144497

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

# 📂 Repository Structure

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

# 🔬 Implementation Details

## Sentiment Analysis

- DistilBERT SST-2
- Batch inference
- `torch.inference_mode()`
- Accuracy and F1 evaluation

---

## Translation

- MarianMT EN → ES
- BLEU evaluation using Hugging Face Evaluate
- Reference translations loaded from file

---

## Question Answering

- MiniLM fine-tuned on SQuAD2
- Extractive QA pipeline

---

## Summarization

- BART-Large-CNN
- Beam Search
- Deterministic decoding
- Controlled output length

---

## Bias Analysis

Performed on the generated summary.

Metrics:

- Toxicity
- Regard

---

# 🧪 Validation

The project validates each NLP task using standard evaluation metrics.

| Task | Metric |
|------|---------|
| Sentiment | Accuracy |
| Sentiment | F1 Score |
| Translation | BLEU |
| QA | Extractive Answer |
| Summarization | Token Constraints |
| Bias | Toxicity |
| Bias | Regard |

---

# 🚀 Possible Improvements

- Fine-tuning on automotive reviews
- Multi-language support
- FastAPI REST service
- Docker deployment
- Kubernetes deployment
- Web interface
- Batch inference
- GPU optimization
- MLOps integration

---

# 🤝 Contributing

Contributions are welcome.

```bash
git checkout -b feature/new-feature

git commit -m "Add new feature"

git push origin feature/new-feature
```

Open a Pull Request.

---

# 📄 License

This project is distributed under the MIT License.

---

# 🙏 Acknowledgements

- Hugging Face Transformers
- Hugging Face Evaluate
- PyTorch
- Scikit-learn
- NLTK
- Facebook AI Research (BART)
- Helsinki-NLP
- Deepset
