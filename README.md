# Documentation: Classifying Political Speeches on Health Policy

## Project Overview

This repository contains code and documentation for classifying sentences from EU political speeches to determine whether they address health policy. The project combines dictionary-based filtering, large language model (LLM) classification, and human annotation to create and evaluate a labeled dataset.

## Contents

- **Introduction:** Project scope and motivation
- **Definition of Health Policy:** EU context and policy areas
- **Methodology:** Data sources, sampling, classification steps
- **Dictionary for Prefiltering:** Terms used to identify health-related sentences
- **Prompt for LLM Classification:** Instructions for automated classification

---

## 1. Introduction

This document outlines the process used to classify political speeches with the aim of identifying whether they address health policy. The classification task is designed to assist in understanding the coverage of health topics in EU political discourse.

## 2. Definition of Health Policy

> “EU countries hold primary responsibility for organising and delivering health services and medical care.  
> EU health policy therefore serves to complement national policies, to ensure health protection in all EU policies and to work towards a stronger Health Union.  
> EU policies and actions in public health aim to  
> - protect and improve the health of EU citizens  
> - support the modernisation and digitalisation of health systems and infrastructure  
> - improve the resilience of Europe's health systems  
> - equip EU countries to better prevent and address future pandemics”  
> &mdash; Overview, European Commission

## 3. Methodology

- A dataset of political speeches was collected from the EU Council, EU Commission, and the European Parliament.
- Sentences were sampled from each speech.
- Sentences were classified using lemmatized dictionary filtering.
- A stratified sample of 10,000 sentences was created: 5,000 with dictionary words, and 5,000 without.
- These sentences were then classified by the `llama3-8b` model using Groq.
- 1,000 sentences were randomly chosen for human annotation.
- Accuracy metrics were calculated to measure the performance of the language models.
- 'roberta-base' is pretrained on LLM annotated data and fine tuned in human annotated data.

## 4. Dictionary Used for Prefiltering

The following dictionary was used (after lemmatization) to prefilter sentences and create a balanced sample.

<details>
<summary>Show dictionary</summary>

```python
DICTIONARY_RAW = [
    # Core Health Terms
    "public health", "health policy", "healthcare system", "national health service", "health insurance",
    "universal healthcare", "access to healthcare", "health coverage", "primary care", "secondary care",
    "medical services", "hospital services", "outpatient care", "in-patient care", "health infrastructure",
    "telemedicine", "ehealth", "digital health",

    # Institutions & Policy Terms (EU Context)
    "european medicines agency", "eu4health", "european centre for disease prevention and control",
    "health security committee", "european health union", "cross-border healthcare",
    "health emergency preparedness and response authority",

    # Specific Health Issues & Areas
    "mental health", "chronic disease", "cancer care", "diabetes care", "cardiovascular disease",
    "pandemic response", "vaccination program", "immunisation strategy", "infectious diseases",
    "antimicrobial resistance", "rare diseases", "pharmaceutical policy", "medicine shortage",
    "drug access", "reproductive health", "maternal care", "child health",

    # Health-Related Professions & Workforce
    "doctors", "nurses", "general practitioners", "healthcare workers", "health professionals",
    "medical staff", "clinical workforce", "frontline workers",

    # Health-Related Social Policy
    "healthy ageing", "health inequalities", "health equity", "health promotion", "disease prevention",
    "healthy lifestyles"
]
```
</details>

## 5. Prompt Used for Classification

**Prompt:**

> Determine whether the following sentence is about **health** in the context of European Union (EU) policy.
>
> A sentence is about *health* if it refers to **healthcare systems**, **public health**, **medical services**, **diseases**, **health policy**, **pharmaceuticals**, **health crises** (like COVID-19), or related EU efforts.
>
> It also includes topics related to EU efforts to:
> - Support the modernization and digitalization of health systems and infrastructure
> - Improve the resilience of Europe’s health systems
> - Protect and improve the health of EU citizens
> - Prevent and address future pandemics
> - Complement national healthcare policies and ensure health protection across EU policies
>
> Now, analyze the sentence below and respond with either **"1"** if it is about health in the EU policy context, or **"0"** if it is not. Provide a brief explanation for your answer.
>
> `Sentence: "{sentence}"`
>
> `Response:`

---

**For questions, suggestions, or contributions, feel free to open an issue or pull request!**# Documentation: Classifying Political Speeches on Health Policy

## Project Overview

This repository contains code and documentation for classifying sentences from EU political speeches to determine whether they address health policy. The project combines dictionary-based filtering, large language model (LLM) classification, and human annotation to create and evaluate a labeled dataset.

## Contents

- **Introduction:** Project scope and motivation
- **Definition of Health Policy:** EU context and policy areas
- **Methodology:** Data sources, sampling, classification steps
- **Dictionary for Prefiltering:** Terms used to identify health-related sentences
- **Prompt for LLM Classification:** Instructions for automated classification

---

## 1. Introduction

This document outlines the process used to classify political speeches with the aim of identifying whether they address health policy. The classification task is designed to assist in understanding the coverage of health topics in EU political discourse.

## 2. Definition of Health Policy

> “EU countries hold primary responsibility for organising and delivering health services and medical care.  
> EU health policy therefore serves to complement national policies, to ensure health protection in all EU policies and to work towards a stronger Health Union.  
> EU policies and actions in public health aim to  
> - protect and improve the health of EU citizens  
> - support the modernisation and digitalisation of health systems and infrastructure  
> - improve the resilience of Europe's health systems  
> - equip EU countries to better prevent and address future pandemics”  
> &mdash; Overview, European Commission

## 3. Methodology

- A dataset of political speeches was collected from the EU Council, EU Commission, and the European Parliament.
- Sentences were sampled from each speech.
- Sentences were classified using lemmatized dictionary filtering.
- A stratified sample of 10,000 sentences was created: 5,000 with dictionary words, and 5,000 without.
- These sentences were then classified by the `llama3-8b` model using Groq.
- 1,000 sentences were randomly chosen for human annotation.
- Accuracy metrics were calculated to measure the performance of the language models.
- 'roberta-base' is pretrained on LLM annotated data and fine tuned in human annotated data.

## 4. Dictionary Used for Prefiltering

The following dictionary was used (after lemmatization) to prefilter sentences and create a balanced sample.

<details>
<summary>Show dictionary</summary>

```python
DICTIONARY_RAW = [
    # Core Health Terms
    "public health", "health policy", "healthcare system", "national health service", "health insurance",
    "universal healthcare", "access to healthcare", "health coverage", "primary care", "secondary care",
    "medical services", "hospital services", "outpatient care", "in-patient care", "health infrastructure",
    "telemedicine", "ehealth", "digital health",

    # Institutions & Policy Terms (EU Context)
    "european medicines agency", "eu4health", "european centre for disease prevention and control",
    "health security committee", "european health union", "cross-border healthcare",
    "health emergency preparedness and response authority",

    # Specific Health Issues & Areas
    "mental health", "chronic disease", "cancer care", "diabetes care", "cardiovascular disease",
    "pandemic response", "vaccination program", "immunisation strategy", "infectious diseases",
    "antimicrobial resistance", "rare diseases", "pharmaceutical policy", "medicine shortage",
    "drug access", "reproductive health", "maternal care", "child health",

    # Health-Related Professions & Workforce
    "doctors", "nurses", "general practitioners", "healthcare workers", "health professionals",
    "medical staff", "clinical workforce", "frontline workers",

    # Health-Related Social Policy
    "healthy ageing", "health inequalities", "health equity", "health promotion", "disease prevention",
    "healthy lifestyles"
]
```
</details>

## 5. Prompt Used for Classification

**Prompt:**

> Determine whether the following sentence is about **health** in the context of European Union (EU) policy.
>
> A sentence is about *health* if it refers to **healthcare systems**, **public health**, **medical services**, **diseases**, **health policy**, **pharmaceuticals**, **health crises** (like COVID-19), or related EU efforts.
>
> It also includes topics related to EU efforts to:
> - Support the modernization and digitalization of health systems and infrastructure
> - Improve the resilience of Europe’s health systems
> - Protect and improve the health of EU citizens
> - Prevent and address future pandemics
> - Complement national healthcare policies and ensure health protection across EU policies
>
> Now, analyze the sentence below and respond with either **"1"** if it is about health in the EU policy context, or **"0"** if it is not. Provide a brief explanation for your answer.
>
> `Sentence: "{sentence}"`
>
> `Response:`

---

**For questions, suggestions, or contributions, feel free to open an issue or pull request!**
