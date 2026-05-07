# RAG-test

Implementation and documentation workspace for:
**"Implementasi Deteksi Prompt Injection pada Chatbot Berbasis Retrieval-Augmented Generation (RAG)"**

## Project Scope

This repository aligns proposal objectives with implementation architecture for a secure Indonesian customer-service RAG chatbot.

### Core Goals

1. Maintain answer quality through retrieval-grounded generation.
2. Detect and mitigate prompt injection at input, retrieval-context, and output stages.
3. Preserve usability by controlling false positives.
4. Provide reproducible prototype artifacts for academic evaluation.

## Proposal-to-Implementation Traceability

| Proposal Focus | Repository Realization |
|---|---|
| Background: RAG benefits + injection risk | `docs/literature_review.md` |
| Problem statement: informative and secure chatbot | `docs/system_architecture.md` |
| Methodology refinement | `docs/methodology_enhanced.md` |
| Scope limitations (text-only, prototype) | reflected across all docs and scenario design |

## Documentation Index

- **Literature Review**: [`docs/literature_review.md`](docs/literature_review.md)
  - Mapping of 6 related works
  - Positioning matrix
  - Key research contributions
- **System Architecture**: [`docs/system_architecture.md`](docs/system_architecture.md)
  - Component breakdown
  - Text-based data flow diagrams
  - Detection–RAG integration points
  - Security layer visualization
- **Enhanced Methodology**: [`docs/methodology_enhanced.md`](docs/methodology_enhanced.md)
  - Detailed dataset composition (Section 8.2)
  - Hyperparameter configurations (Section 8.4)
  - Four testing scenarios (Section 8.5)
  - Reproducibility and artifact management (Section 8.6)

## Current Project Status

- [x] Proposal synchronization documents established
- [x] Research positioning and architecture traceability documented
- [x] Methodology sections expanded with reproducibility guidance
- [ ] Full implementation artifacts (code, models, experiments) to be finalized in iterative phases

## Research Positioning Summary

This project differentiates itself by combining:
- Indonesian-language prompt-injection detection (IndoBERT-oriented)
- Hybrid layered defense (rule-based + semantic detector + context/output checks)
- Customer-service RAG specialization with explicit scenario benchmarking

