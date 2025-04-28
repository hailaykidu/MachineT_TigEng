<<<<<<< HEAD
# Machine Translation Model: English ↔ Tigrinya

This model is a fine-tuned machine translation model trained to translate between English and Tigrinya. It was trained on the parallel corpus of English and Tigrinya sentences.

## Model Overview

- **Model Type**: MarianMT (Multilingual Transformer Model)
- **Languages**: English ↔ Tigrinya
- **Model Architecture**: MarianMT, fine-tuned for English ↔ Tigrinya translation
- **Training Framework**: Hugging Face Transformers, PyTorch

## Training Details

- **Training Dataset**: NLLB Parallel Corpus (English ↔ Tigrinya)
- **Training Epochs**: 3
- **Batch Size**: 8
- **Max Length**: 128 tokens
- **Learning Rate**: Starts from `1.44e-07` and decays during training
- **Training Loss**: 
    - Final training loss: 0.4756
    - Per-epoch loss progress:
      - Epoch 1: 0.443
      - Epoch 2: 0.4077
      - Epoch 3: 0.4379

- **Gradient Norms**: 
    - Epoch 1: 1.14
    - Epoch 2: 1.11
    - Epoch 3: 1.06

- **Training Time**: 43376.7 seconds (~12 hours)
- **Training Speed**:
    - Training samples per second: 96.7
    - Training steps per second: 12.08

## Model Usage

This model can be used for translating English sentences to Tigrinya and vice versa.

### Example Usage (Python)

```python
from transformers import MarianMTModel, MarianTokenizer

# Load the model and tokenizer
model_name = "Hailay/MachineT_TigEng"  
model = MarianMTModel.from_pretrained(model_name)
tokenizer = MarianTokenizer.from_pretrained(model_name)

# Translate an English sentence to Tigrinya
english_text = "We must obey the Lord and leave them alone"
encoded_input = tokenizer(english_text, return_tensors="pt", padding=True, truncation=True)
translated = model.generate(**encoded_input)
translated_text = tokenizer.decode(translated[0], skip_special_tokens=True)

print(f"Translated text: {translated_text}")

##  Model Card

This model is trained to handle general English to Tigrinya translation tasks. It is suitable for a wide range of text, but might not perform well on domain-specific language or specialized terminology unless fine-tuned further.

##Model Architecture
The model is based on the MarianMT architecture, a transformer model designed for multilingual machine translation. It has been fine-tuned on English ↔ Tigrinya data.
##Acknowledgements
Corpus Name: NLLB

Package: NLLB.am-en in Moses format

Website: NLLB Corpus

If you use this model or the NLLB corpus in your work, please cite it as follows:
=======
# MachineT_TigEng
>>>>>>> ef11cd9c1ffe8cccf046d327d76af646e6a4c444
