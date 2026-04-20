---
name: /synthesize-research
description: Transform raw research data (interviews, NPS, CAB sessions) into unified insights for product decisions
category: pm-discovery
tags: [research, synthesis, insights, customer-voice]
---

## Overview
Synthesizes qualitative and quantitative customer research into a structured insight document that prioritizes findings by business impact and customer frequency.

## Usage
```
/synthesize-research \
  --research-sources interviews,nps-survey,cab-sessions \
  --interviews 8 \
  --nps-responses 150 \
  --cab-sessions 2 \
  --topic "enterprise procurement analytics" \
  --focus-areas pricing,integration,compliance \
  --output-format insight-document
```

## Skill Chain (Sequential)

### 1. Customer-Interviews (Input)
- **Contributes**: Qualitative patterns, use cases, pain points
- **Data format**: Interview transcripts or summaries (~30min each × 8 interviews)
- **Key outputs**: Raw themes, verbatims, customer profiles

### 2. Research-Synthesis (Processing)
- **Contributes**: Cross-research pattern detection, insight prioritization, impact scoring
- **Input from previous**: Interview themes + NPS comments + CAB session notes
- **Process**:
  - Extract themes across all sources
  - Score themes by: frequency, impact on buying decision, revenue influence
  - Identify contradictions and segmentation patterns
  - Map to product strategy areas
- **Output**: Unified insight framework with 5-7 key findings
- **Interaction point**: PM reviews draft findings and selects top 3 for deeper analysis

### 3. CAB-Management (Validation)
- **Contributes**: Executive validation, priority confirmation
- **Input**: Synthesized insights from previous step
- **Process**: Present findings to CAB members, gather validation votes
- **Output**: Prioritized insight list with stakeholder sign-off

## Data Handoff

```
Customer Interviews
    ↓ (extract themes, quotes, pain points)
Research Synthesis
    ↓ (combine with NPS + CAB data)
    → Raw theme list (50-100 data points)
    → Frequency scoring (how many customers mentioned)
    → Impact scoring (revenue/strategic weight)
    → Segmentation analysis (which customer types care most)
CAB Management
    ↓ (validate top 3)
Final Insight Document
    → Prioritized findings
    → Supporting evidence & quotes
    → Recommended actions
```

## Execution Model
**Sequential** — Each step requires output from previous step. CAB validation happens after synthesis is complete (not parallel) because CAB members need to see synthesized insights, not raw data.

**User interaction points**:
1. Select which research sources to include
2. Review draft themes (approve/reject/edit)
3. Choose top 3 findings for CAB validation
4. Approve final priorities

## Output Format

```markdown
# Research Synthesis Report
## Executive Summary
- 3-5 sentence summary of top findings
- Business impact assessment

## Methodology
- Sources: 8 interviews, 150 NPS responses, 2 CAB sessions
- Sample: [customer segments included]
- Date range: [research period]

## Key Findings (Priority Order)
### Finding 1: [Title]
- **Frequency**: 7/8 customers mentioned
- **Impact**: $XXM revenue opportunity
- **Quotes**: [2-3 verbatims]
- **Recommendation**: [Action item]

[4 more findings in same format]

## Segmentation Insights
- Enterprise (>$100M ARR) priorities: [list]
- Mid-market priorities: [list]

## Competitive Context
- What competitors are doing
- Where we have differentiation
```

## Estimated Timeline
- Customer-Interviews: Input only (0 min)
- Research-Synthesis: 15-20 minutes
- CAB-Management: Async CAB review (1-3 days) or live review (30 min)
- **Total**: 30-60 minutes active time + async approval window

## Quality Checklist
- [ ] All interview quotes attributed to customer segment
- [ ] Frequency counts verified
- [ ] Impact scores justified by revenue or strategic weight
- [ ] Contradictions flagged and explained
- [ ] Recommendations are actionable
- [ ] CAB members have signed off on priorities
