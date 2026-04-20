---
name: /decision-record
description: Create Architecture Decision Record (ADR) for technical or product decisions with context, alternatives, and rationale
category: pm-operations
tags: [adr, decision-record, architecture, trade-offs]
---

## Overview
Generates an Architecture Decision Record (ADR) that documents significant technical or product decisions, including context, alternatives considered, trade-offs, and rationale. ADRs create institutional memory and reduce re-litigation of decisions.

## Usage
```
/decision-record \
  --decision "Use RAG over fine-tuning for contract analysis" \
  --category technical \
  --impact high \
  --stakeholders engineering,product,legal \
  --include-alternatives true \
  --include-trade-offs true
```

## Format

ADR follows standard format:
1. **Title**: Clear, decision-focused
2. **Context**: Why was this decision needed? What problem are we solving?
3. **Alternatives Considered**: What other options did we evaluate?
4. **Decision**: What did we decide? Why?
5. **Consequences**: What are the short/long-term impacts?
6. **Status**: Accepted, Proposed, Superseded, etc.

## Estimated Timeline
- Research + Writing: 30-40 minutes
- Review + Feedback: 15-20 minutes
- **Total**: 45-60 minutes

## Quality Checklist
- [ ] Context clearly explains why decision was needed
- [ ] 3+ alternatives considered
- [ ] Trade-offs explicitly stated (no alternative is perfect)
- [ ] Rationale is data-backed (not just opinion)
- [ ] Consequences include short-term, medium-term, long-term impacts
- [ ] Decision is clearly stated (not ambiguous)
- [ ] Status and review date included
