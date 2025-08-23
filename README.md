Documentation: Classifying Political Speeches on Health Policy

1 Introduction

This document outlines the process used to classify political speeches with the aim of identifying whether they address Health policy. The classification task was designed to assist in understanding political discourse on Health issues across different contexts and time periods.

2 Definition of Health Policy

“EU countries hold primary responsibility for organising and delivering health services and medical care. 
EU health policy therefore serves to complement national policies, to ensure health protection in all EU policies and to work towards a stronger Health Union. 
EU policies and actions in public health aim to
protect and improve the health of EU citizens
support the modernisation and digitalisation of health systems and infrastructure
improve the resilience of Europe's health systems
equip EU countries to better prevent and address future pandemics”
-Overview - European Commission

3 Methodology

A dataset of political speeches was collected from EU Council, EU Commission and the European Parliment
Sentences are sampled from each speech.
Sentences are classified with a lemmatized dictionary filtering.
A stratified sample of 10,000 sentences was created, consisting of 5,000 sentences with dictionary words and 5,000 sentences without.
These sentences are then classified by 'llama3-8b' using Groq.
1000 sentences were randomly chosen for human annotation.
Various accuracy metrics are calculated, measuring the performance of the language models.

4 Dictionary Used for Prefiltering

The following dictionary was used, after lemmatization, to prefilter sentences to create a more balanced sample.
text

Dictionary:
DICTIONARY_RAW =
[
    # Core Health Terms
    "public health",
    "health policy",
    "healthcare system",
    "national health service",
    "health insurance",
    "universal healthcare",
    "access to healthcare",
    "health coverage",
    "primary care",
    "secondary care",
    "medical services",
    "hospital services",
    "outpatient care",
    "in-patient care",
    "health infrastructure",
    "telemedicine",
    "ehealth",
    "digital health",


    # Institutions & Policy Terms (EU Context)
    "european medicines agency",
    "eu4health",
    "european centre for disease prevention and control",
    "health security committee",
    "european health union",
    "cross-border healthcare",
    "health emergency preparedness and response authority",


    # Specific Health Issues & Areas
    "mental health",
    "chronic disease",
    "cancer care",
    "diabetes care",
    "cardiovascular disease",
    "pandemic response",
    "vaccination program",
    "immunisation strategy",
    "infectious diseases",
    "antimicrobial resistance",
    "rare diseases",
    "pharmaceutical policy",
    "medicine shortage",
    "drug access",
    "reproductive health",
    "maternal care",
    "child health",


    # Health-Related Professions & Workforce
    "doctors",
    "nurses",
    "general practitioners",
    "healthcare workers",
    "health professionals",
    "medical staff",
    "clinical workforce",
    "frontline workers",


    # Health-Related Social Policy
    "healthy ageing",
    "health inequalities",
    "health equity",
    "health promotion",
    "disease prevention",
    "healthy lifestyles"
]


5 Prompt Used for Classification
Prompt:
Determine whether the following sentence is about **health** in the context of European Union (EU) policy.


A sentence is about *health* if it refers to **healthcare systems**, **public health**, **medical services**, **diseases**, **health policy**, **pharmaceuticals**, **health crises** (like COVID-19), **mental health**, **medical research**, or anything directly affecting human health.


It also includes topics related to EU efforts to:
* Support the modernization and digitalization of health systems and infrastructure
* Improve the resilience of Europe’s health systems
* Protect and improve the health of EU citizens
* Prevent and address future pandemics
* Complement national healthcare policies and ensure health protection across EU policies


Now, analyze the sentence below and respond with either **"1"** if it is about health in the EU policy context, or **"0"** if it is not. Provide a brief explanation for your answer.


Sentence: "{sentence}"


Response:"""


