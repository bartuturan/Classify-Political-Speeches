# Classify Political Speeches

This repository contains notebooks and documentation for classifying EU political speech sentences, with a current focus on climate mitigation content.

The workflow combines:
- dictionary-based prefiltering,
- LLM labeling,
- human annotation,
- supervised transformer training,
- and large-scale inference on Parliament, Commission, and Council corpora.

## Project Scope

The documentation file in this repo is:
- `Documentation_ Classifying Political Speeches on Climate Mitigation.docx`

It defines climate mitigation as speech about actions that reduce greenhouse gas emissions or increase carbon sinks, in line with EU policy context (e.g., Green Deal, ETS, Fit for 55, energy transition).

## Repository Structure

- `data/`: institution-level folders (`Commission/`, `Council/`, `EP/`) for datasets/outputs.
- `notebooks/`: end-to-end pipeline notebooks.
- `requirements.txt`: basic Python dependencies.

## Notebook Guide

### 1) `Climate Mitigation-Prefiltering with dictionary-share.ipynb`
Purpose:
- Lemmatizes text with spaCy.
- Uses a climate mitigation dictionary to count matches per sentence.
- Produces dictionary-filtered datasets used for balanced sampling and analysis.

Typical output:
- Feather/CSV files with `how_many_words_in_climate_mitigation_dictionary`.

### 2) `Classify sentences with LLM-share.ipynb`
Purpose:
- Runs LLM-based sentence classification (Groq / Llama family) on sampled or full data.
- Supports checkpointing/retry logic for API rate-limit failures.
- Writes classification outputs with label and explanation fields.

Typical output columns:
- sentence text,
- `is_climate_LLM`,
- `llm_explanation`.

### 3) `train classifier with annotation data-share.ipynb`
Purpose:
- Merges institution-specific human-annotated files into a gold dataset.
- Trains and evaluates a RoBERTa classifier on gold labels.

Typical output:
- `gold_all.csv`.
- Fine-tuned model/checkpoints.

### 4) `pretrain on silver-share.ipynb`
Purpose:
- Merges LLM outputs into a silver dataset.
- Pretrains on silver labels and fine-tunes on gold labels.
- Compares silver+gold versus gold-only performance.

Typical output:
- `silver_all.csv`.
- Final model directory (for downstream institution-level inference).

### 5) `Label Speeches-ep-climate.ipynb`
Purpose:
- Applies the final trained classifier to the European Parliament corpus.
- Includes chunked processing for large feather files.

### 6) `Label Speeches-com-climate.ipynb`
Purpose:
- Applies the final trained classifier to the European Commission corpus.
- Uses chunked/batched inference.

### 7) `Label_Speeches_council-climate.ipynb`
Purpose:
- Applies the final trained classifier to the European Council corpus.
- Uses chunked/batched inference.

## Recommended Execution Order

Run notebooks in this order for a full pipeline:

1. `Climate Mitigation-Prefiltering with dictionary-share.ipynb`
2. `Classify sentences with LLM-share.ipynb`
3. Human annotation step (outside notebook, on sampled data)
4. `train classifier with annotation data-share.ipynb`
5. `pretrain on silver-share.ipynb`
6. `Label Speeches-ep-climate.ipynb`
7. `Label Speeches-com-climate.ipynb`
8. `Label_Speeches_council-climate.ipynb`

Notes:
- Steps 4 and 5 can be compared experimentally (gold-only vs silver+gold).
- Institution labeling notebooks (steps 6-8) should use the same selected final model directory.

## Methodology Summary (From Documentation)

- Collect speech data from EP, Commission, and Council.
- Split speeches into sentences.
- Apply dictionary prefiltering with lemmatized climate terms.
- Build a stratified sample (dictionary-hit and no-hit groups).
- Label sample with LLMs, then human-annotate a subset.
- Evaluate model quality with precision/recall/F1 and accuracy.
- Train RoBERTa on gold labels, with optional silver pretraining.

## Climate Dictionary (Core Terms)

The documentation uses a dictionary including terms such as:
- carbon neutrality,
- climate neutrality,
- net zero,
- greenhouse gas emission,
- EU ETS,
- carbon border adjustment / CBAM,
- climate law,
- Fit for 55,
- Green Deal,
- renewable energy deployment,
- energy efficiency directive,
- emission reduction target.

For the full list, see:
- `notebooks/Climate Mitigation-Prefiltering with dictionary-share.ipynb`
- `Documentation_ Classifying Political Speeches on Climate Mitigation.docx`

## Prompting Logic (LLM Labeling)

Binary decision:
- `1` if sentence is about climate mitigation in EU policy context.
- `0` otherwise.

The prompt criteria in the documentation emphasize:
- emission reduction,
- low-carbon technologies,
- renewable energy,
- carbon sinks,
- and EU mitigation policy instruments.

## Environment Notes

Many notebooks were originally developed in Google Colab with Google Drive paths. To run locally, update:
- input/output file paths,
- model directory paths,
- and API key loading logic.

Install dependencies:

```bash
pip install -r requirements.txt
```

Some notebooks also install extra packages inline (e.g., `transformers`, `datasets`, `evaluate`, `groq`, `pyarrow`, `spacy`).

## Documentation and Limitations

The documentation reports that dictionary filtering can outperform zero-shot LLM labeling in this task setup, depending on institution and sample.

Known limitations:
- ambiguous language where mitigation is implied indirectly,
- possible dataset/source bias,
- sensitivity to prompt wording and thresholding.

Potential next improvements:
- confidence/uncertainty scoring,
- expanded labeled set,
- broader topic transfer to additional policy domains.

