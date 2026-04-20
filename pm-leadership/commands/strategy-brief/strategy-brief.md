---
name: /strategy-brief
description: Generate strategic brief for executive audiences covering vision, market opportunity, competitive positioning, and 12-month plan
category: pm-leadership
tags: [strategy, vision, executive-comms, planning]
---

## Overview
Creates comprehensive strategic brief for executive audiences (Board, C-suite). Covers product vision, market opportunity, competitive landscape, strategic positioning, and 12-month execution plan with milestones and financial projections.

## Usage
```
/strategy-brief \
  --vision "Autonomous Enterprise Procurement" \
  --timeframe 12-months \
  --audiences board,cxo,strategic-partners \
  --include-financials true \
  --include-competitive-moat true
```

## Skill Chain (Sequential)

### 1. Vision-Strategy (Processing)
- **Contributes**: Product vision, market opportunity, strategic direction
- **Input**: Product roadmap, market research, competitive analysis
- **Process**:
  - Define product vision (3-5 year horizon)
  - Articulate market opportunity (TAM, growth rate)
  - Position within competitive landscape
- **Output**: Strategic narrative

### 2. OKR-Management (Sequential)
- **Contributes**: 12-month OKRs, key milestones, success metrics
- **Input**: Strategic narrative
- **Process**:
  - Define quarterly OKRs (Objectives + Key Results)
  - Map to product roadmap
  - Identify go/no-go gates
- **Output**: 12-month milestone plan

### 3. Executive-Presentations (Sequential)
- **Contributes**: Financial projections, investor narrative, risk mitigation
- **Input**: OKRs + strategic narrative
- **Process**:
  - Project revenue impact of strategy
  - Identify risks and mitigations
  - Create compelling narrative for stakeholders
- **Output**: Final strategic brief

## Output Format

```markdown
# Strategic Brief: [Vision Name]

## Vision & Opportunity
- Product vision statement
- Market opportunity (TAM, TAM expansion)
- Competitive positioning

## 12-Month Strategy
- Quarterly OKRs
- Key milestones
- Success metrics

## Financial Projections
- Revenue impact
- Investment required
- Expected ROI

## Competitive Moat
- Differentiation points
- Barriers to entry
- Defensibility
```

## Estimated Timeline
- Vision-Strategy: 25-30 minutes
- OKR-Management: 20-25 minutes
- Executive-Presentations: 20-25 minutes
- **Total**: 65-80 minutes

## Quality Checklist
- [ ] Vision is aspirational but achievable
- [ ] Market opportunity quantified (TAM, growth rate)
- [ ] Competitive positioning is clear and defensible
- [ ] OKRs are measurable and stretch goals
- [ ] Financial projections are conservative and justified
- [ ] Risks identified with mitigation plans
- [ ] 12-month plan is realistic and sequenced
- [ ] Brief is inspiring to executive audience
