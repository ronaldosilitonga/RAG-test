# Enhanced Methodology (Refinement-Integrated)

This document extends methodology details and aligns implementation planning with proposal constraints.

## 8.1 Overview (Context)

The study follows a prototype-oriented secure-RAG methodology for Indonesian customer-service chatbot tasks, integrating detection and response quality evaluation.

## 8.2 Dataset Composition (Detailed)

### 8.2.1 Total Dataset

- **Total samples**: **275** text instances.
- **Class labels**:
  - `normal`: 125 samples (45.45%)
  - `injection`: 150 samples (54.55%)

### 8.2.2 Injection Breakdown

- **Direct injection**: 87 samples (31.64% of total; 58.00% of injection class)
- **Indirect injection**: 63 samples (22.91% of total; 42.00% of injection class)

### 8.2.3 Data Split (Stratified)

- **Train**: 220 samples (80%)
- **Validation**: 27 samples (10% rounded)
- **Test**: 28 samples (10% rounded)

All splits use deterministic seed for reproducibility.

## 8.3 Detection & RAG Pipeline Summary

1. Input text preprocessing and normalization.
2. Rule-based guardrail screening.
3. IndoBERT classifier scoring.
4. RAG retrieval (top-k chunk fetch).
5. Retrieval context sanitization.
6. LLM generation with safe prompt template.
7. Output validation and safe response fallback.

## 8.4 Hyperparameter Configurations

### 8.4.1 Rule-Based Detector

- Keyword dictionary: Indonesian + mixed-language injection phrases
- Regex patterns: instruction override, role-switch, prompt leakage intents
- Decision threshold: binary (rule hit => suspicious)

### 8.4.2 TF-IDF + Logistic Regression (Baseline 2)

- TF-IDF `ngram_range`: (1, 2)
- TF-IDF `max_features`: 10,000
- Logistic Regression `C`: 1.0
- `class_weight`: balanced
- `max_iter`: 1,000
- Random seed: 42

### 8.4.3 IndoBERT Classifier (Proposed Core Detector)

- Base model: IndoBERT family (Indonesian pre-trained transformer)
- Max sequence length: 256
- Batch size: 16
- Learning rate: 2e-5
- Epochs: 3
- Warmup ratio: 0.1
- Weight decay: 0.01
- Decision threshold: tuned on validation set (optimize F1 with low FPR constraint)
- Random seed: 42

### 8.4.4 Retrieval & Generation Defaults

- Chunk size: 256 tokens
- Chunk overlap: 32 tokens
- Retrieval top-k: 5
- Embedding/vector index: prototype-friendly vector store configuration

## 8.5 Testing Scenarios (4 Scenarios)

| Scenario | Name | Configuration | Expected Role |
|---|---|---|---|
| S0 | **Baseline 0 (No Defense)** | Pure RAG, no detector gates | Reference vulnerability level |
| S1 | **Baseline 1 (Rule-Based Only)** | Rule guardrails only before RAG | Fast heuristic defense benchmark |
| S2 | **Baseline 2 (TF-IDF + LR)** | ML classifier gate before RAG | Traditional ML benchmark |
| S3 | **Proposed (Hybrid Secure RAG)** | Rule-based + IndoBERT + context/output checks | Final method under evaluation |

### Evaluation Metrics

- Detection: Accuracy, Precision, Recall, F1, AUROC, False Positive Rate
- Utility: Answer relevance/consistency under benign prompts
- Security stress: Attack success rate across direct and indirect injections

## 8.6 Reproducibility & Artifact Management

1. **Versioned configuration**: all experiment settings tracked in repository config files.
2. **Deterministic runs**: fixed seeds for splitting, model initialization, and training pipelines.
3. **Dataset artifacts**:
   - Raw synthetic dataset snapshot
   - Train/val/test split manifests
4. **Model artifacts**:
   - Baseline model checkpoints/serialized weights
   - Proposed detector checkpoint + tokenizer metadata
5. **Evaluation artifacts**:
   - Scenario-level metric tables
   - Confusion matrices and error-analysis logs
6. **Traceability index**:
   - Link each experiment run to commit hash, config hash, and timestamp.
7. **Re-run protocol**:
   - Documented setup steps, dependency versions, and command sequence for re-execution.

## 9. Cross-Reference to Proposal Requirements

| Proposal Item | Methodology Realization |
|---|---|
| Focus on secure-yet-useful chatbot | Joint detection and utility metrics |
| Indonesian context | IndoBERT-centric detector settings |
| Scope limits to text/app-layer threats | Scenarios and gates restricted to text pipeline |
| Prototype academic evaluation | Scenario-driven benchmark design and artifact tracking |

