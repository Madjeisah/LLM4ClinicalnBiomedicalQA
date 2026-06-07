<div align="center">

# 🧬 From Examination to Deployment: Large Language Models for Clinical and Biomedical Question Answering
### A Systematic Survey — 2019–2025

[![arXiv](https://img.shields.io/badge/arXiv-preprint-b31b1b?style=flat-square&logo=arxiv)](https://arxiv.org/)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Papers](https://img.shields.io/badge/papers_reviewed-120-blue?style=flat-square)](docs/paper_list.md)
[![Datasets](https://img.shields.io/badge/datasets_catalogued-22-orange?style=flat-square)](docs/datasets.md)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](CONTRIBUTING.md)

</div>

---

> **This repository accompanies the survey paper:** *"Large Language Models for Healthcare and Biomedical Question Answering: Methods, Benchmarks, and Open Problems"*
> covering 120 papers from 2019–2025, with a unified taxonomy of methods, a structured dataset comparison, a quantitative safety evaluation, and ten original research gaps.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Model Inventory](#model-inventory)
  - [A.1 Encoder-Only Models](#a1-encoder-only-models)
  - [A.2 Generative and Encoder-Decoder Models](#a2-generative-and-encoder-decoder-models)
  - [A.3 General-Purpose LLMs](#a3-general-purpose-llms)
  - [A.4 Dedicated Medical LLMs](#a4-dedicated-medical-llms)
- [Benchmark Datasets](#benchmark-datasets)
- [Key Findings](#key-findings)
- [Research Gaps](#research-gaps)
- [Citation](#citation)
- [Contributing](#contributing)

---

## Overview

This survey provides a comprehensive review of LLM-based biomedical and clinical question answering (QA), organized around four contributions:

| Contribution | Description |
|---|---|
| 🗂️ **Unified taxonomy** | 120 papers organised into 7 methodological families |
| 📊 **Dataset comparison** | 22 benchmarks compared across type, language, modality, and limitations |
| 🔒 **Safety evaluation** | Quantitative evidence on hallucination, calibration, temporal drift, and demographic bias |
| 🔭 **Research gaps** | 10 original gaps, each specified as a concrete missing benchmark or capability |

The central finding: benchmark scores correlate with real-world clinical performance at only ρ = 0.59. The competencies that explain the residual gap — flexible reasoning, calibrated uncertainty, and equitable performance across demographics — are structurally absent from current MCQ evaluation paradigms.

---

## Repository Structure

```
├── figures/
│   ├── figure2_evolution_timeline.pdf   # Architectural evolution timeline (§3)
│   ├── figure2_evolution_timeline.py    # Matplotlib source script
│   ├── figure3_medqa_progression.pdf    # MedQA performance progression (§3)
│   └── figure3_medqa_progression.py     # Matplotlib source script
├── tables/
│   ├── table1_models.tex                # 32-model comparison matrix
│   ├── table2_datasets.tex              # 22-dataset landscape table
│   ├── table3_methods.tex               # Methods summary
│   ├── table4_challenges.tex            # Challenge scorecard
│   └── table5_gaps.tex                  # Gap prioritisation matrix
├── sections/
│   ├── section1_introduction.tex
│   ├── section2_methodology.tex
│   ├── section3_foundations.tex
│   ├── section4_datasets.tex
│   ├── section5_methods_deepdive.tex
│   ├── section6_challenges.tex
│   ├── section7_gaps.tex
│   └── section8_conclusion.tex
├── appendix_a.tex                       # Full model inventory (this document)
├── acronyms_longtable.tex               # All abbreviations
├── references.bib                       # 140+ BibTeX entries
├── timeline_with_appendix.html          # Interactive figure + appendix (browser)
└── survey_template.tex                  # Master LaTeX file
```

---

## Model Inventory

> **Notation:** † few-shot prompting · ‡ fine-tuned · ★ current SOTA on MedQA (4-option USMLE) · n/a = not reported

### A.1 Encoder-Only Models

Evaluated primarily on **BLURB** (Biomedical Language Understanding & Reasoning Benchmark).
[BLURB Leaderboard →](https://microsoft.github.io/BLURB/leaderboard.html)

| Model | Reference | Params | Pre-training Corpus | Strategy | BLURB | OS | Weights / Code |
|---|---|---|---|---|---|---|---|
| **BioBERT** | Lee et al., ACL 2020 | 110M | PubMed abstracts + PMC full-text (4.5B tokens) | CPT from BERT-Base | 84.3 | ✅ | [github.com/dmis-lab/biobert](https://github.com/dmis-lab/biobert) |
| **SciBERT** | Beltagy et al., EMNLP 2019 | 110M | PubMed abstracts + CS papers (3.2B tokens) | Pre-train from scratch | 83.0 | ✅ | [github.com/allenai/scibert](https://github.com/allenai/scibert) |
| **Bio_ClinicalBERT** | Alsentzer et al., BioNLP 2019 | 110M | MIMIC-III discharge summaries (880M tokens) | CPT from BioBERT | 82.1 | ✅ | [HuggingFace: Bio_ClinicalBERT](https://huggingface.co/emilyalsentzer/Bio_ClinicalBERT) |
| **BioMegatron** | Shin et al., EMNLP 2020 | 345M | PubMed abstracts (4.5B tokens) | Pre-train from scratch (Megatron-LM) | 85.2 | ✅ | [NVIDIA NGC](https://catalog.ngc.nvidia.com/orgs/nvidia/models/biomegatron345muncased) |
| **PubMedBERT** | Gu et al., JAMIA 2022 | 110M | PubMed abstracts only (3.1B tokens) | Pre-train from scratch; domain vocab | 84.9 | ✅ | [HuggingFace: PubMedBERT](https://huggingface.co/microsoft/BiomedNLP-PubMedBERT-base-uncased-abstract) |
| **UmlsBERT** | Michalopoulos et al., NAACL 2021 | 110M | PubMed + PMC + UMLS CUIs | CPT with UMLS-augmented MLM | 84.5 | ✅ | [github.com/gmichalo/UmlsBERT](https://github.com/gmichalo/UmlsBERT) |
| **BioLinkBERT** | Yasunaga et al., ACL 2022 | 340M | PubMed (citation-linked document pairs) | Pre-train from scratch; linked docs | 86.4 | ✅ | [HuggingFace: BioLinkBERT-large](https://huggingface.co/michiyasunaga/BioLinkBERT-large) |
| **GatorTron** | Yang et al., npj Digital Medicine 2022 | 8.9B | UF Health clinical notes + PubMed (90B tokens) | Pre-train from scratch (Megatron-LM) | 85.0 ‡ | ⚠️ | [NVIDIA NGC](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/clara/models/gatortron_og) *(institutional reg.)* |
| **Clinical ModernBERT** | Gao et al., arXiv 2025 | 149M | Medical literature + de-identified clinical notes | CPT (SwiGLU, RoPE, ModernBERT arch) | 84.1 | ✅ | [HuggingFace: clinical-modernbert](https://huggingface.co/clinicalml/clinical-modernbert) |

> **Key insight:** In-domain pre-training from scratch with a domain-specific vocabulary (PubMedBERT) consistently outperforms continued pre-training from general checkpoints, even at smaller corpus scale. Vocabulary design is the single most impactful architectural decision for encoder-only biomedical models.

---

### A.2 Generative and Encoder-Decoder Models

Omitted from the main comparison table for space. Primary benchmarks: **PubMedQA** (reading comprehension), **ROUGE / BERTScore** (NLG tasks).

| Model | Reference | Arch | Params | Pre-training Corpus | Strategy | Best Result | OS | Weights / Code |
|---|---|---|---|---|---|---|---|---|
| **SciFive** | Phan et al., arXiv 2021 | Enc-Dec | 770M | PubMed abstracts + PMC full-text | Pre-train from scratch (T5) | NER / RE SOTA (2021) | ✅ | [github.com/justinphan3110/SciFive](https://github.com/justinphan3110/SciFive) |
| **BioGPT** | Luo et al., Briefings Bioinformatics 2022 | Dec | 1.5B | PubMed abstracts (15M; 342B tokens) | Pre-train from scratch (GPT-2 arch) | PubMedQA **81.0%** | ✅ | [github.com/microsoft/BioGPT](https://github.com/microsoft/BioGPT) |
| **BioMedLM** | Venigalla et al., MosaicML 2022 | Dec | 2.7B | PubMed abstracts + PMC full-text | Pre-train from scratch (GPT-NeoX arch) | MedMCQA 57.3% † | ✅ | [HuggingFace: BioMedLM](https://huggingface.co/stanford-crfm/BioMedLM) |
| **BioBART** | Yuan et al., BioNLP@ACL 2022 | Enc-Dec | 400M | PubMed abstracts (15M) | Pre-train from scratch (BART arch) | Dialogue ROUGE-L 27.8 | ✅ | [HuggingFace: biobart-v2-large](https://huggingface.co/GanjinZero/biobart-v2-large) |
| **BiomedGPT** | Zhang et al., arXiv 2023 | Multimodal | 182M | Radiology, pathology, protein, text | Pre-train from scratch (unified enc-dec) | 16 SOTA across 26 datasets | ✅ | [github.com/taokz/BiomedGPT](https://github.com/taokz/BiomedGPT) |

> **Key insight:** Pre-training objectives designed for general English (e.g., sentence permutation in BART) can harm biomedical downstream performance. BioBART ablations show sentence permutation hurts NLG tasks on structured biomedical prose.

---

### A.3 General-Purpose LLMs

All results zero-shot or few-shot unless noted. Parameter counts for closed models undisclosed (n/a).

| Model | Reference | Params | Pre-training Corpus | MedQA | MedMCQA | PubMedQA | OS | Access |
|---|---|---|---|---|---|---|---|---|
| **GPT-3.5** | Brown et al., NeurIPS 2020 | 175B | Web-scale (CommonCrawl, Books, Wikipedia) | 60.2 † | 62.7 † | 78.2 † | ❌ | OpenAI API `gpt-3.5-turbo` |
| **LLaMA-2-70B** | Touvron et al., arXiv 2023 | 70B | Web-scale (2T tokens, 2022 cutoff) | 62.5 † | n/a | n/a | ✅ | [HuggingFace: Llama-2-70b-hf](https://huggingface.co/meta-llama/Llama-2-70b-hf) |
| **GPT-4** | OpenAI, Technical report 2023 | n/a | Web-scale + RLHF | 86.1 † | n/a | n/a | ❌ | OpenAI API `gpt-4` |
| **Gemini 1.0 Ultra** | Google, arXiv 2023 | n/a | Web-scale + multimodal | 67.0 | 62.2 | n/a | ❌ | Google AI API |
| **GPT-4o** | OpenAI, Technical report 2024 | n/a | Web-scale + multimodal | n/a | n/a | n/a | ❌ | OpenAI API `gpt-4o` |

> **Key insight:** General-purpose LLMs at scale (≥ 70B parameters) subsume much of the benefit of domain-specific pre-training on standard MCQ benchmarks. However, benchmark performance and real-world clinical capability show only moderate correlation (ρ = 0.59), and calibration, open-ended reasoning, and patient communication remain persistent weaknesses regardless of scale.

---

### A.4 Dedicated Medical LLMs

Ordered by publication year, then MedQA score. Chinese-ecosystem models (HuatuoGPT, Qilin-Med, Zhongjing) are primarily evaluated on CMExam and CMedQA.

| Model | Reference | Params | Base Model | Training Strategy | MedQA | MedMCQA | OS | Weights / Code |
|---|---|---|---|---|---|---|---|---|
| **Med-PaLM** | Singhal et al., *Nature* 2023 | 540B | Flan-PaLM-540B | IFT on MultiMedQA; instruction prompt tuning | 67.6 † | 57.6 † | ❌ | Not released (Google internal) |
| **Meditron-70B** | Chen et al., arXiv 2023 | 70B | LLaMA-2-70B | CPT on PubMed + guidelines (45B tokens) then SFT | 70.2 | 64.4 | ✅ | [HuggingFace: meditron-70b](https://huggingface.co/epfl-llm/meditron-70b) |
| **Clinical Camel-70B** | Toma et al., arXiv 2023 | 70B | LLaMA-2-70B | QLoRA SFT on medical dialogues + MedQA train split | 60.7 ‡ | 54.2 ‡ | ✅ | [HuggingFace: ClinicalCamel-70B](https://huggingface.co/wanglab/ClinicalCamel-70B) |
| **MedAlpaca-13B** | Han et al., arXiv 2023 | 13B | LLaMA-13B | SFT on 160K medical QA pairs (GPT-4 generated) | 35.4 ‡ | n/a | ✅ | [github.com/kbressem/medAlpaca](https://github.com/kbressem/medAlpaca) |
| **PMC-LLaMA-13B** | Wu et al., arXiv 2023 | 13B | LLaMA-13B | CPT on 4.8M PMC full-text papers then IFT | 27.6 ‡ | n/a | ✅ | [github.com/chaoyi-wu/PMC-LLaMA](https://github.com/chaoyi-wu/PMC-LLaMA) |
| **HuatuoGPT** | Zhang et al., EMNLP Findings 2023 | 7B | Baichuan-7B | SFT on hybrid physician QA + GPT-4 simulation | n/a | n/a | ✅ | [github.com/FreedomIntelligence/HuatuoGPT](https://github.com/FreedomIntelligence/HuatuoGPT) |
| **Med-PaLM 2** | Singhal et al., *Nature Medicine* 2025 | n/a | PaLM 2 | IFT + ensemble refinement + chain-of-retrieval | 86.5 † | n/a | ❌ | Not released (Google internal) |
| **Qilin-Med** | Ye et al., arXiv 2024 | 7B | Baichuan-7B | CPT on ChiMed then SFT then DPO | n/a | n/a | ✅ | [github.com/CMKRG/QilinMed](https://github.com/CMKRG/QilinMed) |
| **Zhongjing** | Yang et al., arXiv 2023 | 13B | LLaMA-13B | SFT on real multi-turn physician dialogues | n/a | n/a | ✅ | [github.com/SupritYoung/Zhongjing](https://github.com/SupritYoung/Zhongjing) |
| **BioMistral-7B** | Labrak et al., arXiv 2024 | 7B | Mistral-7B-v0.1 | CPT on PMC Open Access (3B tokens) | 44.4 ‡ | n/a | ✅ | [HuggingFace: BioMistral-7B](https://huggingface.co/BioMistral/BioMistral-7B) |
| **Med-Gemini-L ★** | Saab et al., arXiv 2024 | n/a | Gemini 1.0 Ultra | IFT + uncertainty-guided web search at inference | **91.1 †★** | n/a | ❌ | Not released (Google internal) |
| **Polaris** | Mukherjee et al., arXiv 2024 | n/a | Proprietary | Safety-first IFT + DPO on physician preference pairs | n/a | n/a | ❌ | Not released (Accenture internal) |
| **Baichuan-M1-14B** | Xu et al., arXiv 2025 | 14B | From scratch | Full-stack: domain vocab + CPT + SFT + RLHF | n/a | n/a | ✅ | [HuggingFace: Baichuan-M1-14B](https://huggingface.co/baichuan-inc/Baichuan-M1-14B) |

> **Key insight — the fine-tuning paradox:** Instruction fine-tuning consistently improves safety, clinical communication alignment, and parameter efficiency, but rarely improves raw MCQ accuracy over a well-prompted general LLM at the same scale. MedAlpaca (35.4%) and PMC-LLaMA (27.6%) both fine-tune LLaMA-13B on medical data yet score far below GPT-3.5 (60.2%) with no medical training. Fine-tuning reshapes *how* knowledge is expressed, not *how much* knowledge is stored.

---

## Benchmark Datasets

22 datasets compared across four categories. Full details in [`tables/table2_datasets.tex`](tables/table2_datasets.tex).

| Category | Datasets | Key limitation |
|---|---|---|
| **MCQ examination** | MedQA, MedMCQA, MMLU-Med, MedExQA, HEAD-QA, CMExam | Contamination risk; MCQ ≠ clinical reasoning |
| **Literature / retrieval QA** | PubMedQA, BioASQ, MIRAGE, emrQA | Abstract-level only; retrieval infrastructure required |
| **Consumer / open-domain** | HealthSearchQA, MedicationQA, LiveQA-Med, MedDialog, MMedBench, AfriMed-QA | Proprietary or no gold answers; safety-critical |
| **Multimodal VQA** | VQA-RAD, PathVQA, SLAKE, OmniMedVQA, GMAI-MMBench, EHRXQA | 2-D images only; no 3-D volumetric benchmark exists |

---

## Key Findings

```
Benchmark → Clinical correlation:  ρ = 0.59  (Kim & Yoon, 2025)
MedQA SOTA (Med-Gemini-L):        91.1%  (Saab et al., 2024)
Demographic bias prevalence:       91.7% of 24 systematic review studies
Best uncertainty proxy (AUC):      0.68–0.79  (sample consistency, Savage et al., 2025)
GPT-4 mortality prediction acc.:   31.5%  vs GPT-3.5's 59.3% — optimism bias from RLHF
Hallucination rate (clinical):     Non-trivial; ~90% fact-conflicting vs input-conflicting
Temporal drift:                    Asymmetric — high guideline endorsement, low rejection of outdated advice
```

---

## Research Gaps

Ten original gaps identified, each specified as a **concrete missing benchmark or technical capability**:

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

## Evaluation Protocol Note

Results across all tables are drawn from original papers and **may not be directly comparable** due to:

1. **MedQA variant** — 3-option vs. 4-option formats; the 4-option variant is harder and is the standard from 2022 onwards.
2. **Train/test splits** — differ between the official Jin et al. (2021) release and community-reproduced splits.
3. **Few-shot prompt format** — varies in number of examples (1–8), chain-of-thought structure, and system prompt content.

Scores should be interpreted as **approximate rather than absolute rankings**.

---

## Citation

If this survey informs your research, please cite:

```bibtex
@article{hosen2026biomedllmsurvey,
  title   = {Large Language Models for Healthcare and Biomedical Question
             Answering: Methods, Benchmarks, and Open Problems},
  author  = {Hosen, Md Biplob and Hussein, Md Alomgeer and Masud, Md Akmol
             and Faruque, Omar and Reynolds, Tera L and Chen, Lujie Karen},
  journal = {arXiv preprint},
  year    = {2026}
}
```

---

## Understanding Answer-Level vs Claim-Level Hallucination

Consider the following example.

### Source Context

> *"Aspirin reduces cardiovascular risk but increases bleeding risk."*

### Model-Generated Answer

> *"Aspirin reduces cardiovascular risk and decreases bleeding risk."*

The generated answer contains two factual claims:

| Claim                               | Supported by Source? |
| ----------------------------------- | -------------------- |
| Aspirin reduces cardiovascular risk | ✅ Yes                |
| Aspirin decreases bleeding risk     | ❌ No                 |

The second claim is unsupported (and contradicts the source evidence), and therefore constitutes a **hallucination**.

---

### Answer-Level Hallucination

An **answer-level hallucination metric** asks:

> Does the answer contain at least one hallucinated claim?

Formally,

$$
\mathbf{1}_H(y_i)=
\begin{cases}
1, & \text{if answer } y_i \text{ contains at least one hallucinated claim},\
0, & \text{otherwise}.
\end{cases}
$$

The answer-level hallucination rate is

\frac{1}{N}
\sum_{i=1}^{N}
\mathbf{1}_H(y_i),
$$

where:

* (N) = total number of generated answers
* (y_i) = the (i)-th generated answer

Since the aspirin example contains the unsupported claim *"Aspirin decreases bleeding risk"*, the entire response is marked as hallucinated:

$$
\mathbf{1}_H(y_i)=1.
$$

### Interpretation

Answer-level hallucination measures:

> **How often does a generated response contain at least one unsupported claim?**

It does **not** distinguish between responses containing one hallucinated claim and responses containing many hallucinated claims.

---

### Claim-Level Hallucination

A **claim-level hallucination metric** evaluates each factual statement individually.

For the aspirin example:

[
N_{\mathrm{claims}} = 2
]

and

[
N_H = 1,
]

where:

* (N_H) = number of hallucinated (unsupported) claims
* (N_{\mathrm{claims}}) = total number of generated claims

The claim-level hallucination rate is

[
\mathrm{Hall}_{\mathrm{claim}}
==============================

\frac{N_H}
{N_{\mathrm{claims}}}
=====================

# \frac{1}{2}

50%.
]

### Interpretation

Claim-level hallucination measures:

> **What proportion of generated factual statements are unsupported?**

Unlike answer-level metrics, it provides a finer-grained estimate of factual reliability.

---

### Key Difference

Using the aspirin example:

| Metric                     | Result                    |
| -------------------------- | ------------------------- |
| Answer-level hallucination | (1) (hallucinated answer) |
| Claim-level hallucination  | (50%)                     |

Thus:

* **Answer-level hallucination** measures the frequency of problematic responses.
* **Claim-level hallucination** measures the proportion of unsupported factual statements within those responses.

Claim-level metrics are therefore particularly useful for evaluating **long-form biomedical question answering**, where a single response may contain many factual claims and only a subset may be hallucinated.


## Contributing

Spotted a missing model, an updated benchmark result, or a broken link?

1. **Open an issue** with the tag `model-update`, `dataset-update`, or `broken-link`
2. **Submit a pull request** — please include the primary citation for any added model or result
3. For substantial additions (a new model family, a new dataset category), open an issue first to discuss scope

All URLs were verified in **August 2025**; availability is subject to repository maintainer policies.

---

<div align="center">
<sub>Built with 🔬 for the biomedical NLP community</sub>
</div>
