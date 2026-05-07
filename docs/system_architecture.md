# System Architecture (Proposal-Synchronized)

This architecture document operationalizes the proposal objective: a **customer-service RAG chatbot** that remains useful while resisting prompt injection.

## 1. Component Breakdown

| Layer | Component | Responsibility | Related Proposal Focus |
|---|---|---|---|
| Interface | User Query Handler | Accept text input, normalize request metadata | Text-only scope, customer-service context |
| Security Layer 1 | Rule-Based Guardrail | Fast pattern checks (keywords, regex, policy phrases) | Early prompt-injection detection |
| Security Layer 2 | IndoBERT Detector | Contextual classification (normal vs injection) in Indonesian | Language-aware classification |
| RAG Retrieval | Query Embedder + Retriever | Retrieve relevant KB chunks | RAG factual grounding |
| Security Layer 3 | Retrieval Context Sanitizer | Detect/strip embedded malicious instructions in retrieved chunks | Indirect injection mitigation |
| Generation | Prompt Composer + LLM | Build safe prompt from user query + sanitized context | Response generation with controlled context |
| Security Layer 4 | Output Validator | Detect policy violation/leakage before returning answer | Prompt leakage and unsafe output prevention |
| Governance | Logging & Evaluation | Persist decisions, scores, scenario metrics | Prototype evaluation and reproducibility |

## 2. End-to-End Data Flow (Text Diagram)

```text
[User Input]
    |
    v
(1) Input Normalization
    |
    v
(2) Security Gate A: Rule-Based Guardrail
    |---- blocked --> [Security Response: reject/clarify]
    |
    v
(3) Security Gate B: IndoBERT Injection Classifier
    |---- blocked --> [Security Response: reject/clarify]
    |
    v
(4) Retriever: fetch top-k knowledge chunks
    |
    v
(5) Security Gate C: Retrieval Context Sanitizer
    |---- suspicious context removed/quarantined
    |
    v
(6) Prompt Composer (safe template + approved context)
    |
    v
(7) LLM Generation
    |
    v
(8) Security Gate D: Output Validator
    |---- blocked/redacted --> [Safe fallback answer]
    |
    v
[Final Answer to User]
```

## 3. Detection–RAG Integration Points

1. **Pre-retrieval integration**: Gate A/B run before retrieval to reduce expensive processing of malicious requests.
2. **Post-retrieval integration**: Gate C inspects retrieved chunks to prevent indirect prompt injection from KB content.
3. **Pre-delivery integration**: Gate D validates generated responses for policy leakage or instruction-following drift.
4. **Feedback integration**: Logs from all gates support threshold tuning and false-positive reduction.

## 4. Security Layers Visualization

```text
+---------------------------------------------------------------+
|                    Secure Customer-Service RAG                |
+---------------------------------------------------------------+
| Layer 4: Output Validation (policy + leakage + safety checks) |
+---------------------------------------------------------------+
| Layer 3: Retrieval Context Sanitization                        |
+---------------------------------------------------------------+
| Layer 2: IndoBERT Classifier (semantic attack detection)       |
+---------------------------------------------------------------+
| Layer 1: Rule-Based Guardrails (keyword/regex heuristics)      |
+---------------------------------------------------------------+
| Core RAG: Retriever + Prompt Composer + Generator              |
+---------------------------------------------------------------+
```

## 5. Proposal-to-Architecture Traceability Matrix

| Proposal Requirement | Architecture Realization |
|---|---|
| Secure while informative chatbot | Multi-gate architecture surrounding RAG core |
| Focus on prompt injection at application/content level | Gate A/B/C/D on input, context, and output |
| Indonesian text handling | IndoBERT semantic detector |
| Text-only prototype | Single text pipeline without multimodal branches |
| Academic-stage evaluation | Logging + scenario-based metrics collection |

