---
name: /stakeholder-update
description: Generate audience-specific stakeholder updates (executive, engineering, customer) with metrics and context
category: pm-launch
tags: [stakeholder-comms, updates, reporting, transparency]
---

## Overview
Creates three versions of a monthly product update, each tailored to a specific audience (executives, engineering, customers). Each version emphasizes different metrics and narratives while covering the same product updates.

## Usage
```
/stakeholder-update \
  --month april-2026 \
  --audiences exec,engineering,customer \
  --include-metrics true \
  --include-roadmap-changes true \
  --include-health-check true
```

## Skill Chain (Sequential)

### 1. Stakeholder-Comms (from pm-operations)
- **Contributes**: Core narrative, product updates, performance metrics
- **Input**: Product updates, metrics, changes
- **Process**:
  - Gather product launch news
  - Collect performance metrics
  - Note roadmap changes
  - Identify blockers/risks
- **Output**: Unified update document

### 2. Release-Notes (Sequential)
- **Contributes**: Technical details, feature documentation
- **Input**: Core update + product changes
- **Process**:
  - Add technical context for eng audience
  - Add customer-facing benefits for customer audience
  - Add business impact for exec audience
- **Output**: Audience-tailored versions

### 3. Metrics-Analysis (Sequential, from pm-growth)
- **Contributes**: Performance context, trend analysis
- **Input**: Monthly metrics
- **Process**:
  - Add revenue/growth trends
  - Add adoption metrics
  - Add health indicators
- **Output**: Final 3-version update

## Output Format

Each audience gets a customized version:
- **Executive**: Focus on revenue, competitive positioning, risk
- **Engineering**: Focus on technical achievements, tech debt, infrastructure
- **Customer**: Focus on value delivered, timeline, roadmap

All versions cover same updates but with different emphasis and context.

## Estimated Timeline
- Stakeholder-Comms: 20-25 minutes
- Release-Notes: 15-20 minutes (customize for audiences)
- Metrics-Analysis: 15-20 minutes
- **Total**: 50-65 minutes

## Quality Checklist
- [ ] Each version emphasizes relevant metrics for that audience
- [ ] Product updates consistent across all 3 versions
- [ ] Executive version includes business impact
- [ ] Engineering version includes technical details
- [ ] Customer version includes timeline and roadmap
- [ ] All versions are 2-3 pages (digestible length)
