# 🌍 Multilingual Health Question Answering in Low-Resource African Languages

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/JokMaker/-multilingual-health-qa/blob/main/notebooks/03c_afrimt5_final.ipynb)
![Python](https://img.shields.io/badge/python-3.10-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Competition](https://img.shields.io/badge/Zindi-MLT1%20Final%20Project-orange)

> Fine-tuning multilingual transformer models to answer health questions in low-resource African languages (Luganda, Kiswahili, Akan, Amharic) - submitted as part of the ALU Machine Learning Techniques I Final Project.

---

## 📌 Project Overview

Access to reliable health information is a critical challenge across sub-Saharan Africa. This project builds a multilingual model capable of understanding health-related questions in low-resource African languages and generating fluent, accurate responses in the same language.

**Competition:** [Multilingual Health QA - Zindi × ITU/HASH](https://zindi.africa/competitions/multilingual-health-question-answering-in-low-resource-african-languages-challenge)
**Evaluation Metrics:** ROUGE-1 F1 (37%) · ROUGE-L F1 (37%) · LLM-as-a-Judge (26%)
**Languages:** Luganda · Kiswahili · Akan · Amharic · English (Uganda, Ghana, Kenya, Ethiopia)
**Best Public Score: 0.305464** (452x improvement over zero-shot baseline)

---

## 🗂️ Repository Structure

```
multilingual-health-qa/
│
├── notebooks/
│   ├── 01_eda.ipynb                        # Exploratory Data Analysis
│   ├── 02-baseline.ipynb                   # Experiment 1: Zero-shot baseline
│   ├── 03-finetuning-experiments.ipynb     # Experiments 2–10 (mT5-small/base + LoRA)
│   ├── 03b-afrimt5-experiments.ipynb       # AfriMT5 with official Val.csv (Kaggle)
│   └── 03c_afrimt5_final.ipynb             # Final clean training run (Colab, best model)
│
├── submissions/                            # Zindi submission CSV files
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
pip install transformers peft datasets rouge-score torch pandas numpy
```

### 3. Run on Google Colab
Click the **Open in Colab** badge above or navigate to the `notebooks/` folder and open any notebook directly in Colab. Each notebook includes its own setup cell.

---

## 🧪 Experiment Tracker

| # | Experiment | Model | Key Config | ROUGE-1 | ROUGE-L | Zindi Score |
|---|-----------|-------|------------|---------|---------|-------------|
| 1 | Zero-shot baseline | mT5-small | No fine-tuning | 0.0025 | 0.0021 | 0.000676 |
| 2 | Fine-tune mT5-small + LoRA | mT5-small | r=16, 2 epochs, lr=1e-3 | 0.0922 | 0.0833 | — |
| 3 | Improved decoding | mT5-small | repetition_penalty=1.3 | 0.1520 | 0.1228 | 0.145043 |
| 4 | Oversample Amharic | mT5-small | Amharic 3x oversample | 0.1548 | 0.1286 | — |
| 5 | Tokenisation analysis | - | Tokens/word per language | — | — | — |
| 6 | Extended training | mT5-small | 5 epochs | 0.1444 | 0.1193 | — |
| 7 | mT5-base + LoRA r=32 | mT5-base | r=32, Adafactor, 1 epoch | 0.1928 | 0.1565 | 0.211963 |
| 8 | Greedy vs beam search | mT5-base | num_beams=1 vs 4 | 0.1998 | 0.1493 | — |
| 9 | Per-language max length | mT5-base | Akan max_target=384 | 0.1928 | 0.1565 | — |
| 10 | Sampling vs deterministic | mT5-base | T=0.7, top_p=0.9 | 0.2110 | 0.1475 | — |

---

## 📊 Leaderboard Progression

| Submission | Model | Score |
|-----------|-------|-------|
| #1 | Zero-shot mT5-small | 0.000676 |
| #2 | mT5-small + LoRA + repetition penalty | 0.145043 |
| #3 | mT5-base + LoRA r=32 | 0.211963 |
| #4 | AfriMT5-base fp32 | 0.160617 |
| #5 | NLLB-200-distilled-600M + LoRA | 0.143190 |
| #6 | AfriMT5 + language prompts | 0.152884 |
| #7 | AfriMT5 3 epochs, batched inference | 0.268595 |
| #8 | AfriMT5 3 epochs, beam search | 0.277825 |
| #9 | AfriMT5 5 epochs, beam search | 0.294970 |
| **#10** | **AfriMT5 8 epochs, beam search** | **0.305464** |

---

## 🔑 Key Design Decisions

- **Model choice:** AfriMT5-base (masakhane/afri-mt5-base) - mT5 continued-pretrained on 17 African languages, providing stronger representations for Akan, Luganda, Kiswahili, and Amharic
- **PEFT/LoRA:** r=32, alpha=64, target_modules=["q","v"] - enables fine-tuning on T4/A100 without OOM errors
- **Learning rate:** 1e-3 for LoRA (not 5e-5 which is for full fine-tuning - a critical distinction)
- **Language-aware prompts:** "Answer this health question in [Language]: [Question]"
- **Label masking:** Padding tokens replaced with -100 so loss ignores them
- **Official Val.csv:** Used as validation set (6,686 examples) with full Train.csv for training

---

## 📈 Key Findings

1. **Model capacity** is the single most impactful factor - AfriMT5-base outperforms mT5-small regardless of training duration
2. **Tokenisation fragmentation** explains Amharic's persistent low performance - 3.52 tokens/word vs 1.67 for English, a structural limitation of general-purpose multilingual tokenisers
3. **Decoding configuration** improved ROUGE-1 by 65% at zero training cost (Experiment 3)
4. **More epochs consistently improve performance** — val loss decreased from 2.274 (1 epoch) to 1.796 (8 epochs) with no overfitting

---

## ⚠️ Ethical Considerations

This project involves health information in underrepresented languages. Key considerations:
- Misinformation risk in maternal and reproductive health contexts
- Performance disparity - Akan ROUGE-1: 0.308 vs Amharic ROUGE-1: 0.067 (4.6x gap)
- Responsible deployment requires human expert review before clinical use
- AI tools (Claude, GitHub Copilot) were used for coding assistance and report writing

---

## 📚 References

- Raffel et al. (2020). Exploring the Limits of Transfer Learning with T5. *JMLR*.
- Hu et al. (2022). LoRA: Low-Rank Adaptation of Large Language Models. *ICLR*.
- Lin (2004). ROUGE: A Package for Automatic Evaluation of Summaries. *ACL Workshop*.
- Masakhane Community (2023). AfriMT5: Multilingual T5 for African Languages.
- NLLB Team (2022). No Language Left Behind. *Meta AI*.

---

## 👤 Author

**Jok John Maker Kur**
BSE Year 3 · African Leadership University, Kigali, Rwanda
MLT1 Final Project · June 2026

---

*For questions or collaboration, open an issue or connect via [GitHub](https://github.com/JokMaker).*
