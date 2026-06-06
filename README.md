# 🌍 Multilingual Health Question Answering in Low-Resource African Languages

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JokMaker/-multilingual-health-qa/blob/main/notebooks/01_baseline.ipynb)
![Python](https://img.shields.io/badge/python-3.10-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Competition](https://img.shields.io/badge/Zindi-MLT1%20Final%20Project-orange)

> Fine-tuning multilingual transformer models to answer health questions in low-resource African languages (Luganda, Kiswahili, Akan, Amharic) — submitted as part of the ALU Machine Learning Techniques I Final Project.

---

## 📌 Project Overview

Access to reliable health information is a critical challenge across sub-Saharan Africa. This project builds a multilingual model capable of understanding health-related questions in low-resource African languages and generating fluent, accurate responses in the same language.

**Competition:** [Multilingual Health QA — Zindi × ITU](https://zindi.africa)  
**Evaluation Metrics:** ROUGE-1 F1 (37%) · ROUGE-L F1 (37%) · LLM-as-a-Judge (26%)  
**Languages:** Luganda · Kiswahili · Akan · Amharic

---

## 🗂️ Repository Structure

```
multilingual-health-qa/
│
├── notebooks/
│   ├── 01_eda.ipynb                  # Exploratory Data Analysis
│   ├── 02_baseline.ipynb             # Zero-shot baseline (mT5-small)
│   ├── 03_finetune_mt5_small.ipynb   # Fine-tuned mT5-small
│   ├── 04_finetune_mt5_base.ipynb    # Fine-tuned mT5-base
│   ├── 05_lora_peft.ipynb            # LoRA/PEFT experiments
│   └── ...                           # Additional experiments
│
├── src/
│   ├── preprocess.py                 # Data preprocessing pipeline
│   ├── train.py                      # Training script
│   ├── evaluate.py                   # Evaluation (ROUGE + LLM-as-Judge)
│   └── inference.py                  # Inference & submission generation
│
├── data/
│   ├── raw/                          # Original competition data
│   └── processed/                    # Cleaned & tokenized data
│
├── submissions/                      # Zindi submission CSV files
├── results/                          # Experiment results & plots
├── requirements.txt
└── README.md
```

---

## ⚙️ Setup Instructions

### 1. Clone the repository
```bash
git clone https://github.com/JokMaker/-multilingual-health-qa.git
cd multilingual-health-qa
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run on Google Colab
Click the **Open in Colab** badge above or navigate to the `notebooks/` folder and open any notebook directly in Colab. Each notebook includes its own setup cell.

---

## 🧪 Experiment Tracker

| # | Experiment | Model | Config | ROUGE-1 | ROUGE-L | LLM-Judge | Notes |
|---|-----------|-------|--------|---------|---------|-----------|-------|
| 1 | Zero-shot baseline | mT5-small | No fine-tuning | - | - | - | Starting point |
| 2 | Fine-tune mT5-small | mT5-small | 3 epochs, lr=1e-4 | - | - | - | |
| 3 | Fine-tune mT5-base | mT5-base | 3 epochs, lr=1e-4 | - | - | - | |
| 4 | Prompt formatting v1 | mT5-small | Template A | - | - | - | |
| 5 | Prompt formatting v2 | mT5-small | Template B | - | - | - | |
| 6 | LoRA PEFT | mT5-base | r=16, alpha=32 | - | - | - | |
| 7 | LR sweep (low) | mT5-small | lr=3e-5 | - | - | - | |
| 8 | LR sweep (high) | mT5-small | lr=5e-4 | - | - | - | |
| 9 | Max token 128 | mT5-small | max_len=128 | - | - | - | |
| 10 | Max token 256 | mT5-small | max_len=256 | - | - | - | |
| 11 | Beam search | mT5-base+LoRA | num_beams=4 | - | - | - | |
| 12 | Ensemble | mT5-small + base | avg predictions | - | - | - | |

> Table updated after each Zindi submission.

---

## 📊 Leaderboard Progression

| Submission | Date | Public Score | Key Change |
|-----------|------|-------------|------------|
| #1 | - | - | Zero-shot baseline |
| #2 | - | - | First fine-tuned model |

---

## 🔑 Key Design Decisions

- **Model choice:** mT5 (multilingual T5) selected for native support of African languages
- **PEFT/LoRA:** Used to enable fine-tuning on free-tier GPUs without OOM errors
- **Evaluation:** ROUGE scores computed locally before each submission to guide experiments

---

## ⚠️ Ethical Considerations

This project involves health information in underrepresented languages. Key considerations:
- Misinformation risk in maternal and reproductive health contexts
- Language bias — model may perform unevenly across the 4 languages
- Responsible deployment requires human expert review before clinical use

---

## 📚 References

- Raffel et al. (2020). Exploring the Limits of Transfer Learning with T5. *JMLR*.
- Adelani et al. (2021). MasakhaNER: Named Entity Recognition for African Languages. *EMNLP*.
- Lin (2004). ROUGE: A Package for Automatic Evaluation of Summaries. *ACL Workshop*.

---

## 👤 Author

**Jok** — BSE Final Year, African Leadership University  
MLT1 Final Project · June 2026
