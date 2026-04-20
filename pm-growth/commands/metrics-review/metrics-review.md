---
name: /metrics-review
description: Structured quarterly metrics review with metric tree, trend analysis, diagnosis, and recommendations
category: pm-growth
tags: [metrics, analytics, performance, diagnostics]
---

## Overview
Generates comprehensive quarterly metrics review with metric tree (hierarchy of metrics), trend analysis, root cause diagnosis for concerning trends, and data-backed recommendations for improvement.

## Usage
```
/metrics-review \
  --quarter q2-2026 \
  --include-metric-tree true \
  --include-trend-analysis true \
  --include-diagnosis true \
  --focus-areas adoption,revenue,retention,efficiency
```

## Skill Chain (Sequential)

### 1. Metrics-Analysis (Processing)
- **Contributes**: Metric collection, trend calculation, baseline comparison
- **Input**: Quarterly data (revenue, adoption, churn, efficiency metrics)
- **Process**:
  - Collect metrics from all systems (Stripe, analytics, support)
  - Calculate YoY and QoQ trends
  - Compare to targets
  - Flag concerning trends (> 10% miss vs. target)
- **Output**: Baseline metrics + trend analysis

### 2. Experimentation (Sequential)
- **Contributes**: Root cause analysis, hypothesis testing
- **Input**: Concerning metrics
- **Process**:
  - Diagnose why metrics missed (qualitative + data analysis)
  - Develop hypotheses for improvement
  - Recommend experiments/actions
- **Output**: Recommendations with expected impact

## Data Handoff

```
Quarterly Metrics
    ↓ (collection & trends)
Metrics Analysis
    ↓ (metric tree + trends)
Experimentation
    ↓ (diagnosis & recommendations)
Final Review
    → Metric tree (hierarchy)
    → Trend analysis & variance
    → Root cause analysis
    → Recommended actions
    → Expected impact
```

## Estimated Timeline
- Metrics-Analysis: 25-30 minutes
- Experimentation: 20-25 minutes
- **Total**: 45-55 minutes

## Quality Checklist
- [ ] Metric tree clearly shows hierarchy (leading → lagging indicators)
- [ ] All concerning metrics (>10% miss) have root cause analysis
- [ ] Recommendations are data-backed (include supporting data)
- [ ] Expected impact quantified (not vague)
- [ ] Metrics are compared to targets AND historical trends
- [ ] Metric review connects to business strategy
