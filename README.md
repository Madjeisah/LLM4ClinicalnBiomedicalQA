# 🧬 Biomedical Language Model Inventory

[![Paper](https://img.shields.io/badge/Paper-Under_Review-blue)](https://example.com)
[![Models](https://img.shields.io/badge/Models-40+-green)](https://github.com)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

This repository provides a **systematic inventory and analysis of pre-trained language models** for biomedical and clinical applications. It includes detailed comparisons of encoder-only models, generative architectures, general-purpose LLMs, and dedicated medical models, along with a prioritized research gap matrix to guide future work.

---

## 📊 Key Features

- **Comprehensive Model Inventory**: 40+ models from BioBERT (2020) to Baichuan-M1-14B (2025)
- **Standardized Benchmarks**: BLURB (encoder-only), MedQA (4-option USMLE), MedMCQA, PubMedQA
- **Open-Source Tracking**: Clear indicators for ✓ publicly released, ~ partial/institutional, ✗ proprietary
- **Research Gap Matrix**: 10 prioritized gaps with novelty, evidence, feasibility, and impact ratings
- **Heterogeneity Notes**: Explicit documentation of evaluation protocol differences (MedQA variants, split versions, few-shot formats)

---

## 🗂️ Model Categories

| Category | Description | Example Models |
|----------|-------------|----------------|
| **Encoder-Only** | BERT-style models for classification, NER, and relation extraction | BioBERT, PubMedBERT, GatorTron (8.9B), BioLinkBERT |
| **Generative / Encoder-Decoder** | Sequence-to-sequence models for text generation and QA | BioGPT, BioMedLM, SciFive, BiomedGPT (multimodal) |
| **General-Purpose LLMs** | Web-scale models evaluated zero/few-shot on medical benchmarks | GPT-4, LLaMA-2-70B, Gemini 1.0 Ultra |
| **Dedicated Medical LLMs** | Domain-specific continued pre-training or fine-tuning | Med-PaLM 2, Meditron-70B, Med-Gemini-L (91.1% ★ SOTA) |

---

## 📈 Top Performance Snapshot

| Model | MedQA (4-option) | OS | Notes |
|-------|------------------|-----|-------|
| **Med-Gemini-L** ★ | 91.1% | ✗ | Google internal + uncertainty-guided web search |
| **Med-PaLM 2** | 86.5% | ✗ | PaLM 2 + ensemble refinement + chain-of-retrieval |
| **GPT-4** | 86.1% | ✗ | 5-shot prompting (Liévin et al. 2024) |
| **Meditron-70B** | 70.2% | ✓ | LLaMA-2-70B + CPT on PubMed + guidelines |
| **Med-PaLM** | 67.6% | ✗ | Flan-PaLM-540B + IFT on MultiMedQA |
| **GPT-3.5** | 60.2% | ✗ | 5-shot, 175B parameters |
| **Clinical Camel-70B** | 60.7% | ✓ | QLoRA SFT on medical dialogues |

> **★** = Current SOTA on MedQA (4-option USMLE)

---

## 🔬 Encoder-Only Models (Selected)

| Model | Params | Pre-training Corpus | BLURB | OS |
|-------|--------|---------------------|-------|-----|
| **BioLinkBERT** | 340M | PubMed (citation-linked pairs) | **86.4** | ✓ |
| **BioMegatron** | 345M | PubMed abstracts | 85.2 | ✓ |
| **PubMedBERT** | 110M | PubMed abstracts only | 84.9 | ✓ |
| **BioBERT** | 110M | PubMed + PMC full-text | 84.3 | ✓ |
| **UmlsBERT** | 110M | PubMed + PMC + UMLS CUIs | 84.5 | ✓ |
| **GatorTron** | 8.9B | UF Health clinical notes + PubMed (90B tokens) | 85.0‡ | ~ |

*BLURB scores from [microsoft.github.io/BLURB](https://microsoft.github.io/BLURB/leaderboard.html)*

---

## 🏥 Dedicated Medical LLMs

| Model | Base | Training Strategy | MedQA | OS |
|-------|------|-------------------|-------|-----|
| **Meditron-70B** | LLaMA-2-70B | CPT (45B tokens) + SFT | 70.2 | ✓ |
| **Clinical Camel-70B** | LLaMA-2-70B | QLoRA SFT | 60.7 | ✓ |
| **BioMistral-7B** | Mistral-7B-v0.1 | CPT on PMC Open Access | 44.4 | ✓ |
| **MedAlpaca-13B** | LLaMA-13B | SFT (160K GPT-4 generated QA) | 35.4 | ✓ |
| **PMC-LLaMA-13B** | LLaMA-13B | CPT (4.8M papers) + IFT | 27.6 | ✓ |

*Chinese-ecosystem models (HuatuoGPT, Qilin-Med, Zhongjing) evaluated on CMExam/CMedQA — see original papers.*

---

## 🔴 Research Gap Prioritization Matrix

| # | Gap | Novelty | Evidence | Feasibility | Impact |
|---|-----|---------|----------|-------------|--------|
| 1 | Calibration & abstention | ★★★ | Strong | High | **Critical** |
| 2 | Temporal drift & continual learning | ★★★ | Growing | Medium | **Critical** |
| 3 | Multilingual low-resource | ★★★ | Strong | Medium | High |
| 4 | Flexible clinical reasoning | ★★★ | Strong | High | **Critical** |
| 5 | Open-ended long-form QA | ★★ | Moderate | High | High |
| 6 | Rare disease QA | ★★★ | Confirmed | High | High |
| 7 | Health equity & bias | ★★ | Medium | Strong | **Critical** |
| 8 | RAG audit & provenance | ★★ | Moderate | High | High |
| 9 | 3-D multimodal QA | ★★★ | Confirmed | Medium | High |
| 10 | Regulatory evaluation | ★★ | Growing | Low | **Critical** |

---

## ⚠️ Evaluation Protocol Notes

**Scores across papers are not directly comparable** due to three primary sources of heterogeneity:

1. **MedQA variant**: 3-option vs. 4-option formats — the 4-option variant (standard from 2022) is harder.
2. **Train/test splits**: Differences between Jin et al. (2021) official release and community-reproduced splits.
3. **Few-shot prompt format**: Number of examples (1–8), chain-of-thought structure, and system prompt content vary.

> The **BLURB leaderboard** provides standardized encoder-only comparisons under a fixed evaluation protocol.

---

## 📚 Citation

If you use this inventory in your research, please cite:

```bibtex
@misc{biomedical-lm-inventory,
  title = {Biomedical Language Model Inventory: A Systematic Analysis of Pre-trained Models for Clinical NLP},
  author = {Your Name et al.},
  year = {2025},
  howpublished = {\url{https://github.com/yourusername/biomedical-lm-inventory}}
}
