# Bayan — Engineering Decisions

This document records engineering decisions that have been implemented and validated in the Bayan project. Decisions are updated as the pipeline develops and new evidence becomes available.

---

## Day 1 — Text Processing & Transformers

### Decision 1 — Conservative Arabic Preprocessing

**Decision:**  
Use conservative normalization as the default preprocessing profile.

**Implemented profile:**
- Unicode NFC normalization
- Whitespace normalization
- Tatweel removal
- PII masking before downstream processing

**Rationale:**  
Aggressive Arabic normalization may remove linguistic information that could be useful to downstream NLP tasks. The project therefore preserves linguistic information unless a transformation is supported by measured evidence.

**Privacy contract:**  
The pipeline maintains separate safe display and model-processing representations. Synthetic email addresses and phone numbers are masked before downstream processing or publication.

---

### Decision 2 — Tokenizer Selection Requires Measurement

**Decision:**  
Do not select a multilingual tokenizer based only on its model family or reputation.

**Evidence inspected:**  
The executed Day 1 pipeline measured token fertility, truncation, and observed tokenization time on synthetic Arabic and English examples using a local WordPiece demonstration, mBERT, and XLM-R.

**Measured observations:**
- Local mean token fertility: `1.36`
- Local truncation rate @10: `0%`
- mBERT mean token fertility: `1.77`
- mBERT truncation rate @10: `80%`
- XLM-R mean token fertility: `1.50`
- XLM-R truncation rate @10: `80%`

**Interpretation:**  
These measurements are diagnostic evidence from a small teaching sample. They are not treated as proof that one tokenizer is universally superior. Final model and tokenizer choices must remain paired and should be evaluated on task-relevant bilingual data.

---

### Decision 3 — Attention Masking Contract

**Decision:**  
Padding positions must be blocked before softmax so that they cannot influence valid encoder representations.

**Validated behavior:**
- Valid attention rows sum to one.
- Masked positions receive zero attention weight.
- The Day 1 implementation uses `True` to represent positions allowed to participate and `False` for blocked positions within its explicit attention implementation.

**API caution:**  
Mask conventions can differ between libraries, so the expected semantics must be checked when integrating another API.

---

### Decision 4 — Attention Is Diagnostic Evidence

**Decision:**  
Use attention weights to inspect implementation behavior, not as proof that a token caused a model prediction.

**Rationale:**  
Attention visualization can help detect implementation problems such as incorrect masking, but attention weights alone do not establish causal explanations for model decisions.

---

### Day 1 Known Limitations

- The rule-based sentence segmentation baseline requires stronger abbreviation handling before production use.
- The current PII masking mechanism is educational and is not a production-grade personal-data detection system.
- Tokenizer measurements were produced from a small synthetic bilingual sample and should not be generalized to production data.
- Production model selection requires representative task-specific evaluation.

---

### Day 1 Verification

The executed Day 1 pipeline produced:

`DAY1_NOTEBOOK1_CORE=PASS`  
`DAY1_NOTEBOOK2_CORE=PASS`  
`EXPLORE_TOKENIZER_COMPARISON=PASS`  
`DISTINCTION_TOKENIZER_COMPARISON=PASS`  
`TRANSFORMER_ENCODER_FORWARD=PASS`  
`ATTENTION_PARITY=PASS`  
`ACTUAL_TRANSFORMER_FORWARD=PASS`  
`DAY1_GATE_A_TECHNICAL_EVIDENCE=PASS`  
`DAY1_PIPELINE_EXECUTION=COMPLETE`
