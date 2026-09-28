# Bayan — Applied Bilingual NLP System

**Bayan (بيان)** is an applied Natural Language Processing project designed to build an inspectable bilingual workflow for analyzing Arabic and English service feedback.

The system is developed progressively across text preparation, classification, named entity recognition (NER), extractive question answering, semantic retrieval, reranking, evaluation, optimization, and API serving.

> **Project Status:** In development — implementation details and measured results are documented only after they are executed and validated.
> ---

## Project Objective

Bayan is designed to process Arabic and English service feedback and transform unstructured text into structured, inspectable outputs.

The project combines multiple NLP tasks within one pipeline while emphasizing reproducibility, privacy, evaluation, and evidence-based engineering decisions.

The intended workflow is:

**Input → Privacy & Preprocessing → NLP Tasks → Semantic Retrieval → Unified Response → API**
---

## Core NLP Components

Bayan is organized around a set of complementary NLP components:

- **Topic & Sentiment Classification** — identify the main feedback category and its sentiment.
- **Named Entity Recognition (NER)** — extract supported entities from Arabic and English text.
- **Extractive Question Answering (QA)** — answer questions only when the supplied context supports the answer.
- **Semantic Retrieval** — retrieve similar historical cases using sentence embeddings and vector search.
- **Cross-Encoder Reranking** — refine the ordering of retrieved candidates.
- **Evaluation & Error Analysis** — measure performance and inspect failure cases before making engineering decisions.
- **API Serving** — expose the validated pipeline through a testable service interface.
- ---

## System Architecture

Bayan follows a modular pipeline that separates safe text handling, task-specific NLP models, retrieval, and serving.

```text
Arabic / English Input
        │
        ▼
Privacy & Safe Display Copy
        │
        ▼
Versioned Model Text
        │
        ├──────────────┬──────────────┐
        ▼              ▼              ▼
 Classification      NER       Extractive QA
        │              │              │
        └──────────────┴──────────────┘
                       │
                       ▼
              Semantic Retrieval
                       │
                       ▼
             Cross-Encoder Reranking
                       │
                       ▼
                Unified Response
                       │
                       ▼
                 Tested API
```
## Engineering Principles

Bayan is developed with the following engineering principles:

- **Privacy first:** sensitive information is masked before logging or downstream processing.
- **Reproducible preprocessing:** normalization and text preparation are treated as versioned data contracts.
- **Evidence before claims:** model quality and system performance are reported only after measurement.
- **Bilingual inspection:** Arabic and English behavior are evaluated rather than assumed to be equivalent.
- **Modular design:** classification, NER, QA, retrieval, reranking, and serving remain independently testable.
- **Train–serve consistency:** preprocessing decisions used during development must remain consistent during inference.
## Repository Structure

The repository is organized to keep experimentation, implementation, evaluation, and documentation clearly separated.

```text
bayan-applied-nlp-khalid-alajmi/
├── notebooks/        # Executable Colab notebooks and experiments
├── src/bayan/        # Reusable NLP pipeline components
├── tests/            # Automated validation and API tests
├── reports/          # Evaluation and benchmark artifacts
├── sample_outputs/   # Validated example outputs
├── runtime_report.json
└── README.md
```

## Development Roadmap

The project is implemented incrementally, with each stage producing verifiable evidence before the next stage is introduced.

1. **Runtime Readiness** — verify the execution environment, dependencies, connectivity, and UTF-8 support.
2. **Text Preparation** — establish privacy-safe display text, versioned normalization, sentence segmentation, and tokenizer analysis.
3. **Task Modeling** — implement and evaluate classification, NER, and extractive QA.
4. **Arabic NLP & Retrieval** — inspect Arabic-specific behavior and build semantic retrieval with vector search.
5. **Reranking & Evaluation** — rerank retrieved candidates and perform quantitative evaluation and error analysis.
6. **Optimization & Serving** — benchmark the validated pipeline and expose it through a tested API.
7. **Release & Reproducibility** — verify artifacts, tests, documentation, and freeze the assessed submission snapshot.

## Current Progress

| Stage | Status | Evidence |
|---|---|---|
| Runtime readiness | Completed | `runtime_report.json` and executed Runtime Doctor notebook |
| Text preparation | Planned | Pending implementation and validation |
| Classification | Planned | Pending implementation and evaluation |
| Named Entity Recognition | Planned | Pending implementation and evaluation |
| Extractive QA | Planned | Pending implementation and evaluation |
| Semantic retrieval | Planned | Pending implementation and evaluation |
| Reranking | Planned | Pending implementation and evaluation |
| API serving | Planned | Pending implementation and testing |

> Results, metrics, and performance claims will be added only after reproducible execution and validation.

 ## Evidence and Reproducibility

All project results are based on executed notebooks, saved artifacts, and reproducible evaluation.

Current verified evidence:

- Runtime environment validated successfully.
- Runtime readiness report saved as `runtime_report.json`.
- Runtime Doctor notebook stored under `notebooks/`.
- Future metrics and benchmark results will be reported only after execution and validation.

No model-performance claims are made before measurement.

## Technology Stack

The project will use the following tools and technologies as they are introduced and validated during implementation:

- Python
- Google Colab
- GitHub
- Hugging Face Transformers
- PyTorch
- Sentence Transformers
- FAISS
- Cross-Encoder models
- FastAPI
- Pytest / TestClient

Specific models, configurations, and versions will be documented after they are executed and validated.

## Privacy and Data Handling

Bayan is developed as a teaching prototype using synthetic bilingual examples.

The project follows a privacy-first text handling approach:

- Raw input is never logged before protection.
- Sensitive information is masked before logging or publication.
- `display_text` preserves a safe, human-readable version of the input.
- `model_text` contains the declared normalization required for model processing.
- Preprocessing decisions are versioned and applied consistently.
- Real personal data is not required for project development or demonstration.

The masking implemented in this project is educational and should not be treated as a production-grade PII detection system.

## Attribution

This repository contains my implementation, experiments, engineering decisions, evaluation results, and documentation developed as part of the Bayan Applied NLP learning project.

Course materials, instructional notebooks, and the original Bayan learning framework were prepared and delivered by **Meaad Al-Marri** as part of **SDAIA Academy**.

Any course-provided material retained in this repository is attributed to its original source. My project-specific results and performance claims are reported only when produced through my own executed and validated workflow.
---

## Day 1 — Text Processing & Tokenisation

Day 1 establishes the bilingual text-preparation pipeline used by Bayan. The implementation was executed end-to-end in Colab using synthetic Arabic and English examples.

### Implemented and Validated

- Unicode inspection and NFC normalization.
- Two-copy preprocessing contract preserving the raw input and producing protected model-ready text.
- PII masking for synthetic email and phone examples before model processing.
- Conservative Arabic normalization using NFC, whitespace normalization, and tatweel removal.
- Sentence segmentation using a spaCy sentencizer, including an explicit abbreviation limitation probe.
- Local WordPiece tokenization with token IDs and unknown-token inspection.
- Token fertility and truncation measurement.
- Padding and attention-mask construction.
- Token ID to embedding conversion with validated tensor shapes.
- Exploratory comparison with multilingual BERT (mBERT).
- Distinction comparison between mBERT and XLM-R.

### Measured Evidence

| Evidence | Result |
|---|---:|
| Local mean token fertility | 1.44 |
| Local truncation rate @10 | 20% |
| Input IDs shape | (2, 12) |
| Attention mask shape | (2, 12) |
| Embeddings shape | (2, 12, 8) |
| mBERT mean token fertility | 1.72 |
| mBERT truncation rate @10 | 80% |
| XLM-R mean token fertility | 1.43 |
| XLM-R truncation rate @10 | 80% |

Tokenization timing was also measured during the executed comparison; because the evaluation set is intentionally small, timing is treated as observational evidence rather than a general performance claim.

### Engineering Decision

The project uses conservative normalization by default to preserve Arabic linguistic information while applying only necessary preprocessing and privacy protection. Aggressive normalization is not used by default because it may remove useful linguistic distinctions.

Tokenizer metrics are treated as diagnostic evidence rather than proof that one multilingual tokenizer is universally superior to another.

### Verification

The executed Day 1 notebook completed all core, Explore, and Distinction checks successfully, including:

`DAY1_NOTEBOOK1_CORE=PASS`  
`EXPLORE_TOKENIZER_COMPARISON=PASS`  
`DISTINCTION_TOKENIZER_COMPARISON=PASS`

Notebook: `notebooks/01_text_processing_tokenization.ipynb`
