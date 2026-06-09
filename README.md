---
language:
  - eng
  - tig
tags:
  - tokenizer
  - machine-translation
  - low-resource
  - geez-script
  - marianmt
  - sentencepiece
license: mit
datasets:
  - nllb
  - opus
metrics:
  - bleu
---

# English–Tigrinya Machine Translation Model

[![Paper](https://img.shields.io/badge/Paper-FLLM2025-blue)](https://doi.org/10.1109/FLLM67465.2025.11390974)
[![Model](https://img.shields.io/badge/HuggingFace-Hailay%2FMachineT__TigEng-yellow)](https://huggingface.co/Hailay/MachineT_TigEng)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Languages](https://img.shields.io/badge/Languages-English%20%E2%86%94%20Tigrinya-orange)]()

> **Low-Resource English–Tigrinya MT: Leveraging Multilingual Models, Custom Tokenizers, and Clean Evaluation Benchmarks**  
> Hailay Kidu Teklehaymanot, G. Gebremariam Gidey, Wolfgang Nejdl  
> *3rd International Conference on Foundation and Large Language Models (FLLM 2025)*, pp. 121–128  
> 📍 25–28 November 2025 | Vienna, Austria | [DOI: 10.1109/FLLM67465.2025.11390974](https://doi.org/10.1109/FLLM67465.2025.11390974)

---

## Overview

This repository provides a **custom SentencePiece tokenizer** and a **fine-tuned MarianMT model** for bidirectional **English ↔ Tigrinya machine translation**. Tigrinya is a low-resource Ge'ez-script language spoken primarily in Eritrea and the Tigray region of Ethiopia, and is significantly underrepresented in standard multilingual NLP models.

The model is trained on the NLLB parallel corpus and evaluated against OPUS parallel data using BLEU, addressing the lack of clean, reliable translation benchmarks for this language pair.

---

## Model Details

| Property             | Value                                           |
|----------------------|-------------------------------------------------|
| **Task**             | Bidirectional Machine Translation (EN ↔ TIG)   |
| **Base Model**       | MarianMT (multilingual transformer)             |
| **Tokenizer**        | SentencePiece, customized for Ge'ez script      |
| **Training Data**    | NLLB Parallel Corpus (English–Tigrinya)         |
| **Evaluation Data**  | OPUS Parallel Corpus (English–Tigrinya)         |
| **Evaluation Metric**| BLEU                                            |
| **Frameworks**       | Hugging Face Transformers, PyTorch              |
| **License**          | MIT                                             |

---

## Training Details

| Parameter              | Value                        |
|------------------------|------------------------------|
| Epochs                 | 3                            |
| Batch size             | 8                            |
| Max sequence length    | 128 tokens                   |
| Learning rate          | `1.44e-07` with decay        |
| Training time          | ~12 hours (43,376.7s)        |
| Training speed         | 96.7 samples/sec             |
| Steps per second       | 12.08                        |

**Training Loss per Epoch**

| Epoch | Loss   | Gradient Norm |
|-------|--------|---------------|
| 1     | 0.4430 | 1.14          |
| 2     | 0.4077 | 1.11          |
| 3     | 0.4379 | 1.06          |
| Final | 0.4756 | —             |

---

## Usage

The model supports translation in **both directions**. The direction is controlled by a language prefix token passed to the tokenizer.

### English → Tigrinya

```python
from transformers import MarianMTModel, MarianTokenizer

model_name = "Hailay/MachineT_TigEng"
model = MarianMTModel.from_pretrained(model_name)
tokenizer = MarianTokenizer.from_pretrained(model_name)

english_text = "We must obey the Lord and leave them alone"
inputs = tokenizer(english_text, return_tensors="pt", padding=True, truncation=True)
translated = model.generate(**inputs)
print(tokenizer.decode(translated[0], skip_special_tokens=True))
```

### Tigrinya → English

Prepend `>>eng<<` to tell the model to produce English output:

```python
from transformers import MarianMTModel, MarianTokenizer

model_name = "Hailay/MachineT_TigEng"
model = MarianMTModel.from_pretrained(model_name)
tokenizer = MarianTokenizer.from_pretrained(model_name)

tigrinya_text = ">>eng<< ንሕና ንእግዚኣብሔር ክንእዘዝ ኣሎና"
inputs = tokenizer(tigrinya_text, return_tensors="pt", padding=True, truncation=True)
translated = model.generate(**inputs)
print(tokenizer.decode(translated[0], skip_special_tokens=True))
```

### Batch Translation

```python
sentences = [
    "We must obey the Lord and leave them alone",
    "The children are learning at school today",
    "Peace is important for all nations",
]

inputs = tokenizer(sentences, return_tensors="pt", padding=True, truncation=True)
translated = model.generate(**inputs)
for t in translated:
    print(tokenizer.decode(t, skip_special_tokens=True))
```

---

## Model Card

This model is designed for general-domain English ↔ Tigrinya translation. It performs well on a broad range of everyday text but may underperform on highly domain-specific or technical content without further fine-tuning. It is intended as a research baseline and a practical resource for the low-resource NLP community.

**Limitations:**
- Trained on 3 epochs; further training may improve BLEU scores
- Performance on highly formal or domain-specific text (legal, medical) is not evaluated
- Tigrinya dialectal variation (Eritrean vs. Ethiopian) may affect output quality

---

## Citation

If you use this model, tokenizer, or evaluation benchmark in your work, please cite:

```bibtex
@inproceedings{teklehaymanot2025lowresource,
  title     = {Low-Resource {E}nglish--{T}igrinya {MT}: Leveraging Multilingual Models,
               Custom Tokenizers, and Clean Evaluation Benchmarks},
  author    = {Teklehaymanot, Hailay Kidu and Gebremariam Gidey, G. and Nejdl, Wolfgang},
  booktitle = {2025 3rd International Conference on Foundation and Large
               Language Models (FLLM)},
  year      = {2025},
  address   = {Vienna, Austria},
  month     = {November},
  pages     = {121--128},
  doi       = {10.1109/FLLM67465.2025.11390974},
  publisher = {IEEE}
}
```

---

## Acknowledgements

- Training corpus: [NLLB](https://huggingface.co/datasets/allenai/nllb) (No Language Left Behind, Meta AI)
- Evaluation corpus: [OPUS](https://opus.nlpl.eu/) parallel data
- Base model: [MarianMT](https://huggingface.co/docs/transformers/model_doc/marian) via Hugging Face Transformers
- This work was carried out at the [L3S Research Center](https://www.l3s.de), Leibniz Universität Hannover.
