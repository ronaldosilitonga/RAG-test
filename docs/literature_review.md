# Literature Review & Research Positioning

This document synchronizes the proposal narrative with implementation direction for **"Implementasi Deteksi Prompt Injection pada Chatbot Berbasis Retrieval-Augmented Generation (RAG)"**.

## 1. Mapping of Related Works

| No | Work | Focus | Main Method | Key Finding | Limitation for This Research |
|---|---|---|---|---|---|
| 1 | **Su et al.** | Prompt-injection robustness for LLM applications | Injection pattern analysis and defense heuristics | Injection can bypass naive instruction handling | Not specialized for Indonesian customer-service RAG |
| 2 | **Abdelnabi et al.** | Security risks in LLM-integrated systems | Threat taxonomy + empirical attacks | Real-world integrated apps are exploitable | Limited implementation guidance for domain-specific chatbot pipelines |
| 3 | **Liu et al.** | Prompt attack behavior and transferability | Adversarial prompt construction/testing | "Normal-looking" prompts can still manipulate models | Mostly model-centric, less pipeline-level defense layering |
| 4 | **Lan et al.** | Defensive prompting/guardrails | Rule/constraint-based prompt controls | Guardrails can reduce successful attacks | Rule-only methods tend to brittle generalization |
| 5 | **Jacob et al.** | RAG system security hardening | RAG-specific validation architecture | Security must cover retrieval and generation stages | Often evaluated in non-Indonesian and non-customer-service settings |
| 6 | **Shih & Kang** | Multi-layer LLM safety for practical deployment | Input filtering + output validation + policy checks | Layered defense is stronger than single detector | Practical tuning needed to keep false positives low |

## 2. Positioning Matrix

### 2.1 Capability Comparison

| Capability | Su et al. | Abdelnabi et al. | Liu et al. | Lan et al. | Jacob et al. | Shih & Kang | **This Research** |
|---|---:|---:|---:|---:|---:|---:|---:|
| Prompt injection threat modeling | ✓ | ✓ | ✓ | ~ | ✓ | ✓ | ✓ |
| Input-level attack detection | ~ | ~ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Retrieval-stage safeguards | ~ | ~ | ~ | ~ | ✓ | ✓ | ✓ |
| Output policy validation | ~ | ~ | ~ | ✓ | ✓ | ✓ | ✓ |
| Indonesian language focus | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ | **✓** |
| Customer-service RAG domain focus | ~ | ~ | ~ | ~ | ~ | ~ | **✓** |
| Hybrid rule + classifier architecture | ~ | ~ | ~ | ✓ | ✓ | ✓ | **✓ (Rule-based + IndoBERT)** |
| Low-FPR usability orientation | ~ | ~ | ~ | ~ | ~ | ✓ | **✓** |

Legend: ✓ supported, ~ partially discussed, ✗ not covered.

### 2.2 Improvement Positioning

| Prior Work Gap | Proposed Improvement in This Project |
|---|---|
| Defenses are often generic, not tailored for Indonesian text | Add **IndoBERT-based classification** and Indonesian lexical guardrails |
| Pipeline security usually assessed partially | Add explicit defense gates at **input, retrieval context, and output** |
| Trade-off between attack blocking and usability not operationalized | Include measurable focus on **false positive control** for customer-service UX |
| Limited implementation traceability from proposal to modules | Provide direct mapping from proposal sections to architecture components and test scenarios |

## 3. Key Contributions of This Research

1. **Domain-specific secure RAG design** for Indonesian customer-service chatbot workflows.
2. **Hybrid detection layer** combining rule-based guardrails and IndoBERT classifier.
3. **Pipeline-level security integration** that treats detection as part of input–retrieval–generation lifecycle.
4. **Evaluation framework with four scenarios** (Baseline 0/1/2 + Proposed) to quantify security and utility trade-offs.
5. **Traceability-first documentation** linking proposal requirements (Problem, Scope, Methodology) to implementation modules.

## 4. Proposal Traceability

| Proposal Area | Documentation Link |
|---|---|
| Background & risk motivation | Section 1 and 2 in this file |
| Problem statement (secure + informative chatbot) | Positioning matrix and contribution mapping |
| Scope limitation (text-only, Indonesian, prototype) | Contribution #1, #2 and methodology/testing references |
| Methodology expansion request | See `docs/methodology_enhanced.md` |

