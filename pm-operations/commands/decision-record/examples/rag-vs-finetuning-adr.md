# Architecture Decision Record: RAG vs Fine-tuning for Contract Analysis

**ADR Number**: ADR-2026-003
**Status**: ACCEPTED
**Decision Date**: April 15, 2026
**Authors**: Head of ML, VP Engineering, VP Product

---

## Title
Use Retrieval-Augmented Generation (RAG) over fine-tuning for AI-powered contract risk analysis.

---

## Context

**Problem Statement**:
Build AI-powered contract risk analysis for procurement platform. System must:
- Analyze enterprise contracts (1,000s of pages, varied formats)
- Flag compliance violations (HIPAA, FAR/DFARS, SOX, GDPR)
- Explain why a clause is flagged (interpretability required)
- Adapt to new compliance rules without full model retraining

**Decision Required**:
Should we use **RAG (Retrieval-Augmented Generation)** or **fine-tuning** as primary ML approach?

---

## Alternatives Considered

### Alternative 1: Fine-Tuning (Not Chosen)

**Approach**: Fine-tune large language model on 500 labeled contracts.

**Advantages**:
- Theoretically higher accuracy
- Single inference call (faster)

**Disadvantages**:
- Retraining required for new rules (2+ weeks per cycle)
- Expensive to train (GPU resources)
- Data labeling bottleneck (need 500+ per rule type)
- Interpretability weak
- Regulatory risk (not auditable)
- Effort: 12 person-weeks to productionize

---

### Alternative 2: RAG (Retrieval-Augmented Generation) — CHOSEN

**Approach**: Maintain library of compliance rules, use LLM + retrieval to apply rules.

**Advantages**:
- Easy to update rules (minutes vs. weeks)
- Interpretable (explains which rule was violated)
- Auditable (rule-based decisions)
- Lower data requirements (no labeled training data needed)
- Regulatory-friendly (compliance teams understand rules)
- Faster to market (6 person-weeks)
- Easy to add new compliance domains

**Disadvantages**:
- Rule quality dependency
- Computational cost (multiple LLM API calls)
- Latency: 1-2 seconds per analysis
- Less data-driven

---

## Decision

**Use RAG for contract compliance analysis.**

**Rationale**:
1. **Timeline**: 6 weeks vs. 12 weeks (critical for August 1 deadline)
2. **Rule Changeability**: Quarterly compliance changes require flexibility
3. **Explainability**: HIPAA/SOX require audit trail
4. **Cost**: Similar annual cost, less upfront engineering
5. **Data Efficiency**: Works with 500 contracts (fine-tuning needs 500+ per rule)

---

## Consequences

### Short-term (Q3 2026)
- Faster launch (on schedule)
- Lower engineering effort
- Rules understandable by legal team
- Easy to add new rules

### Medium-term (Q4 2026 - Q1 2027)
- Quick rule iteration
- Customer-specific rules possible
- Audit trail enables enterprise sales

### Long-term (2027+)
- If accuracy limited, revisit fine-tuning (will have 5k+ labeled examples)

---

## Trade-Offs Accepted

| Trade-Off | Mitigation |
|-----------|-----------|
| 2s latency vs 0.5s | Async processing, caching |
| LLM API cost scaling | Monitor pricing, evaluate alternatives |
| Manual rule library | Compliance team ownership, testing framework |
| LLM vendor dependency | Multi-vendor support |

---

## Review Criteria

**Success (6 months)**:
- Model accuracy: 90%+
- Customer NPS: 4.5+/5.0
- Time to add rules: < 1 week
- Uptime: 99.9%+
- Adoption: 80%+ of target customers

**Reconsider if**:
- Accuracy < 85%
- API costs > $10k/month
- Customer demands local model

---

**Approved by**: VP Engineering, VP Product, Head of ML
**Review Date**: January 2027
