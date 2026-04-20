---
name: /competitive-brief
description: Profile key competitors with market context and positioning gaps for strategic planning
category: pm-discovery
tags: [competitive-analysis, market, positioning, strategy]
---

## Overview
Creates a structured competitive brief that profiles 3-5 key competitors with market context, feature parity matrices, and positioning differentiation. Runs competitive analysis in parallel across multiple competitors using subagents.

## Usage
```
/competitive-brief \
  --competitors coupa,jaggr,ariba \
  --focus-areas pricing,compliance,integrations,forecasting \
  --market s2p-solutions \
  --include-market-sizing true \
  --depth detailed
```

## Skill Chain (Parallel for Competitors + Sequential Market Context)

### 1. Competitive-Analysis (Parallel Subagents - 1 per competitor)
- **Contributes**: Feature matrix, pricing model, customer base, GTM strategy per competitor
- **Process per competitor**: 
  - Website analysis
  - Product walkthrough notes
  - Analyst reports (Gartner, IDC)
  - Customer reviews (G2, Capterra)
  - LinkedIn talent patterns
  - Financial/acquisition data
- **Output per competitor**: Standardized competitor profile
- **Parallelization**: Run for all 3 competitors simultaneously (3x faster)

### 2. Market-Sizing (Sequential, uses Competitive output)
- **Contributes**: TAM/SAM, growth rates, customer distribution
- **Input**: Aggregated competitive data (customer counts, pricing)
- **Process**:
  - Derive addressable market from competitor TAM claims
  - Analyze customer distribution across segments
  - Estimate market growth rates
- **Output**: Market sizing framework

### 3. Vendor-Evaluation (Sequential, uses Market output)
- **Contributes**: Competitive positioning matrix, differentiation gaps
- **Input**: Market context + all competitor profiles
- **Process**:
  - Create feature parity matrix
  - Score each competitor on key dimensions
  - Identify white space opportunities
- **Output**: Positioning brief with gaps
- **Interaction point**: PM reviews gaps and selects top 3 for go-to-market focus

## Data Handoff

```
[Parallel Subagents]
Competitive-Analysis (Competitor 1) ┐
Competitive-Analysis (Competitor 2) ├→ Aggregated profiles
Competitive-Analysis (Competitor 3) ┘
    ↓ (feeds)
Market-Sizing
    ↓ (TAM, customer distribution)
Vendor-Evaluation
    ↓ (features parity + positioning gaps)
Final Competitive Brief
    → Feature matrix
    → Positioning map
    → Market context
    → Recommended GTM focus
```

## Execution Model
**Parallel then Sequential**:
- Competitors analyzed in parallel (3 subagents, 1 per competitor) — saves 10-15 minutes
- Market sizing must run after competitive data is aggregated
- Vendor evaluation runs last to synthesize all data

**User interaction points**:
1. Select which competitors to analyze
2. Define evaluation dimensions (pricing, features, market position, etc.)
3. Review positioning map and approve top 3 gaps to target
4. Confirm GTM focus areas based on gaps

## Output Format

```markdown
# Competitive Brief: S2P Solutions Market

## Market Overview
- TAM: $X.XB (derived from competitor data)
- Growth: 15-18% CAGR
- Top 3 players: Coupa (45% share), Jaggr (20%), Ariba (18%)

## Competitive Profiles
### Competitor 1: Coupa
- **Founded**: 20XX
- **Customers**: ~1,200 (enterprise-focused)
- **Price point**: $X-YXk+ per month
- **Key features**: [list]
- **Strengths**: [list]
- **Weaknesses**: [list]
- **GTM**: [strategy]

[Similar for 2 more competitors]

## Feature Parity Matrix
[Table showing: Feature | Us | Coupa | Jaggr | Ariba]

## Positioning Map
[2D matrix: Ease of Use vs. Compliance Depth, etc.]

## White Space / Differentiation Gaps
- Gap 1: [gap description]
- Gap 2: [gap description]

## Recommended GTM Focus (Next 12 months)
- Primary target: [competitor positioning we beat]
- Secondary target: [segment we can win]
```

## Estimated Timeline
- Competitive-Analysis (parallel): 12-15 minutes per competitor, 15 min total (parallel)
- Market-Sizing: 8-10 minutes
- Vendor-Evaluation: 10-12 minutes
- **Total**: 35-40 minutes (vs. 60+ if sequential)

## Quality Checklist
- [ ] All 3+ competitors profiled with current product state
- [ ] Pricing data verified (use latest pricing page, not outdated sources)
- [ ] Feature matrix includes 15+ evaluation dimensions
- [ ] Positioning map shows clear white space
- [ ] Market sizing cross-checks with analyst reports
- [ ] Recommended gaps are actionable (6-12 month build)
