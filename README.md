# Probing Multilingual Sentiment Models on Turkish-English Code-Switching: Confidence, Attribution, and Embedding Geometry

A computational linguistics probing study examining how pre-trained language models process Turkish-English code-switching (CS) text compared to monolingual Turkish counterparts.

---

## Overview

Code-switching — the interleaving of two or more languages within a single utterance — is ubiquitous in Turkish social media and professional discourse. This project investigates whether transformer-based models respond to CS text through genuine linguistic sensitivity or through distributional pattern matching.

We probe two models (BERTurk and XLM-T) on a controlled ablation dataset derived from a real Turkish-English CS corpus, comparing prediction confidence, token-level SHAP attributions, and sentence embedding geometry across CS and Turkish-only conditions.

**Key finding:** CS sensitivity in XLM-T operates at the sentence distribution level, not the token level — a form of pattern matching rather than pragmatic understanding.

---

## Dataset

The CS corpus used in this study is the **Turkish-English Code-Switching Corpus** introduced by:

> Yirmibeşoğlu, Z., & Eryiğit, G. (2018). Detecting Code-Switching between Turkish-English Language Pair. In *Proceedings of the 2018 EMNLP Workshop W-NUT: The 4th Workshop on Noisy User-generated Text* (pp. 110–115). Association for Computational Linguistics. https://aclanthology.org/W18-6115

The corpus contains 391 social media utterances with token-level language tags (T = Turkish, E = English). We use 372 code-switched sentences and construct matched Turkish-only ablations by removing English-tagged tokens from each sentence.

**Please cite the original corpus paper if you use this data.**

The raw corpus files (`original.txt`, `with_tags.txt`) are included in `data/` with permission for academic use. For any other use, please refer to the original authors.

---

## Models

| Model | HuggingFace ID | Reference |
|-------|---------------|-----------|
| BERTurk Sentiment | `savasy/bert-base-turkish-sentiment-cased` | Yildirim (2024) |
| XLM-T | `cardiffnlp/twitter-xlm-roberta-base-sentiment` | Barbieri et al. (2022) |
| Sentence Embeddings | `paraphrase-multilingual-mpnet-base-v2` | — |

---

## Experiments

| # | Experiment | Method | Key Result |
|---|-----------|--------|------------|
| 1 | Confidence: CS vs TR-only | Two-sample t-test | XLM-T: CS > TR (p=0.002); BERTurk: n.s. |
| 2 | EN token ratio vs confidence | Pearson correlation | No significant correlation (both models) |
| 3 | Token-level attribution | SHAP (partition explainer) | EN vs TR token impact not significantly different (p=0.196) |
| 4 | Embedding geometry | PCA (multilingual mpnet) | CS sentences more dispersed; centroid distance = 0.383 |

---

## Results Summary

CS sensitivity in XLM-T is real (Experiment 1) but cannot be explained by the amount of English switching (Experiment 2) or by elevated individual English token influence (Experiment 3). Embedding geometry (Experiment 4) shows CS sentences occupy a broader, more dispersed region in multilingual space. Together, these findings suggest CS effects emerge at the sentence distribution level — consistent with statistical pattern matching rather than compositional pragmatic understanding.

---

## Repository Structure

```
turkish-english-code-switching/
├── README.md
├── data/
│   ├── original.txt               # Raw CS utterances (Yirmibeşoğlu & Eryiğit, 2018)
│   └── with_tags.txt              # Token-level language tags (T/E per line)
├── notebooks/
│   └── cs_analysis_pipeline.ipynb # Full pipeline: parse → baseline → inference → SHAP → PCA
├── paper/
│   ├── cs_paper.tex               # ACL-format paper (LaTeX)
│   └── custom.bib                 # Bibliography
└── outputs/
    ├── cs_full_analysis.csv        # Model predictions + confidence scores
    ├── embedding_pca.png           # PCA: CS vs TR-only embedding space
    ├── en_ratio_vs_confidence.png  # EN token ratio vs model confidence
    └── shap_en_vs_tr.png           # SHAP: EN vs TR token attribution
```

---


## Citation

If you use this code or findings, please cite the corpus and models:

```bibtex
@inproceedings{yirmibesoglu-eryigit-2018-detecting,
  title     = {Detecting Code-Switching between {T}urkish-{E}nglish Language Pair},
  author    = {Yirmibe{\c{s}}o{\u{g}}lu, Zeynep and Eryi{\u{g}}it, G{\"u}l{\c{s}}en},
  booktitle = {Proceedings of the 2018 {EMNLP} Workshop W-{NUT}},
  year      = {2018},
  pages     = {110--115},
  url       = {https://aclanthology.org/W18-6115}
}

@inproceedings{barbieri-etal-2022-xlm,
  title     = {{XLM}-{T}: Multilingual Language Models in {T}witter for Sentiment Analysis and Beyond},
  author    = {Barbieri, Francesco and Espinosa Anke, Luis and Camacho-Collados, Jose},
  booktitle = {Proceedings of the Thirteenth Language Resources and Evaluation Conference},
  year      = {2022},
  pages     = {258--266}
}

@misc{yildirim2024finetuning,
  title         = {Fine-tuning Transformer-based Encoder for Turkish Language Understanding Tasks},
  author        = {Savas Yildirim},
  year          = {2024},
  eprint        = {2401.17396},
  archivePrefix = {arXiv}
}
```

---


