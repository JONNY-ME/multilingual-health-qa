# Multilingual Health QA Notebooks

Colab experiment tracker for multilingual health question answering in low-resource African languages.

## Goal

The task is to generate accurate, fluent, and contextually appropriate health answers in the same language as the input question. The challenge focuses on low-resource African languages including Akan/Twi, Luganda, Kiswahili, and Amharic, alongside English subsets from several countries.

## Evaluation

Use a weighted multi-metric score:

| Metric | Weight | Purpose |
| --- | ---: | --- |
| ROUGE-1 F1 | 0.37 | Keyword and unigram overlap |
| ROUGE-L F1 | 0.37 | Sentence-level sequence similarity |
| LLM-as-a-Judge | 0.26 | Factuality, completeness, and language appropriateness |

Prediction files contain these columns for evaluation:

```text
ID, TargetRLF1, TargetR1F1, TargetLLM
```

## Current Baseline

Initial zero-shot run with `google/gemma-4-E4B-it`:

| Run | Model | Setup | ROUGE-L F1 | ROUGE-1 F1 | LLM Judge | Weighted PLB Score |
| --- | --- | --- | ---: | ---: | ---: | ---: |
| `gemma4-e4b-zero-shot-v1` | `google/gemma-4-E4B-it` | Zero-shot, Colab GPU | 0.3663 | 0.6071 | 0.6133 | 0.5196 |

Runtime note: the run took about 3 hours and used roughly 15 GB RAM in the Colab environment.

## Experiment Roadmap

1. Establish zero-shot Gemma 4 E4B-it baseline.
2. Try larger Gemma 4 variants, starting with `google/gemma-4-26B-A4B-it`.
3. Compare prompt variants on stratified validation samples before full test generation.
4. Add retrieval-based baselines to improve ROUGE-style lexical overlap.
5. Move to QLoRA fine-tuning only after baseline and retrieval runs are well understood.

## Main Notebook

This repository is intended to upload notebooks only, with notebooks placed at the repository root. The current baseline notebook is:

```text
01_gemma4_e4b_zero_shot_baseline.ipynb
```

It supports:

- Colab GPU setup with `uv`
- challenge data download from Google Drive
- Gemma 4 model loading
- zero-shot prompt generation
- checkpointed inference
- ROUGE-1 and ROUGE-L validation
- per-subset error analysis
- strict prediction CSV generation

## Colab Run Checklist

1. Select a GPU runtime in Colab Pro+.
2. Run the setup and data download cells.
3. Confirm that `Train.csv`, `Val.csv`, `Test.csv`, and `SampleSubmission.csv` load correctly.
4. Run smoke validation before full validation or test inference.
5. Save validation predictions and public leaderboard scores for each run.
6. Keep each output file name tied to the model and prompt version.

## Run Log Template


| Run | Model | Prompt | Decode Settings | Validation Scope | ROUGE-L F1 | ROUGE-1 F1 | LLM Judge | PLB Score | Notes |
| --- | --- | --- | --- | --- | ---: | ---: | ---: | ---: | --- |
| `run-id` | `model-id` | `prompt-vN` | `temperature=0.0` | full/test | 0.0000 | 0.0000 | 0.0000 | 0.0000 | short note |

## Reproducibility Notes

- Use open-source models and packages only.
- Set seeds for deterministic local validation.
- Do not use AutoML tools.
- Keep model name, prompt version, decoding settings, runtime type, and output filename in each run note.
- Do not manually edit generated prediction files except to fix formatting issues identified by validation scripts.

