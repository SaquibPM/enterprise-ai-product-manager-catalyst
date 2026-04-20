---
name: /gtm-plan
description: Generate go-to-market plan with parallel workstreams (sales enablement, customer migration, release notes, comms)
category: pm-launch
tags: [gtm, launch, sales-enablement, customer-comms]
---

## Overview
Creates comprehensive GTM plan for feature launch with parallel workstreams: sales enablement, customer migration, release notes, and launch communications. Runs workstreams in parallel using subagents (one per workstream).

## Usage
```
/gtm-plan \
  --feature "AI contract risk scoring" \
  --launch-date "2026-08-31" \
  --target-segments finance,healthcare,government \
  --include-workstreams sales,migration,comms,content \
  --parallel-execution true
```

## Skill Chain (Parallel Workstreams)

### 1. GTM-Planning (Sequential Kickoff)
- **Contributes**: GTM strategy, messaging, timeline
- **Input**: Feature spec, customer research, competitive positioning
- **Output**: GTM framework and subagent assignments

### 2. Sales-Enablement (Parallel Subagent 1)
- **Contributes**: Sales collateral, competitive positioning docs, customer case studies
- **Creates**: Pitch deck, objection handling guide, ROI calculator

### 3. Customer-Migration (Parallel Subagent 2)
- **Contributes**: Migration guide, customer comms timeline, support plan
- **Creates**: Migration guide, upgrade path documentation

### 4. Release-Notes (Parallel Subagent 3)
- **Contributes**: Technical documentation, user guides, release notes
- **Creates**: Release notes, feature documentation

### 5. Stakeholder-Comms (Parallel Subagent 4)
- **Contributes**: Internal comms, launch timeline, announcement schedule
- **Creates**: CEO announcement, customer webinar invites

## Data Handoff

```
[Parallel Subagents - All Start Simultaneously]
Sales-Enablement ┐
Customer-Migration ├→ Final GTM Plan
Release-Notes ├→ (integrated)
Stakeholder-Comms ┘

Each subagent operates independently, then outputs merged into unified GTM plan
```

## Execution Model
**Parallel** — All 4 workstreams run simultaneously after kickoff. Each subagent owns one workstream. Merge results at end.

**User interaction points**:
1. Approve GTM strategy and target segments
2. Review and approve sales messaging (from Sales-Enablement)
3. Review and approve customer migration plan (from Customer-Migration)
4. Review and approve release notes and documentation (from Release-Notes)
5. Review and approve launch communications (from Stakeholder-Comms)
6. Final GTM plan assembly

## Output Format

```markdown
# Go-to-Market Plan: [Feature Name]

## GTM Strategy
- Launch date, target segments, messaging

## Workstream 1: Sales Enablement
- Sales deck
- Positioning vs competitors
- ROI calculator

## Workstream 2: Customer Migration
- Migration guide
- Customer comms timeline

## Workstream 3: Release Notes & Documentation
- Release notes
- User guides

## Workstream 4: Stakeholder Communications
- Launch announcement schedule
- Customer webinar
- Internal comms
```

## Estimated Timeline
- GTM Planning (kickoff): 10-15 minutes
- Parallel workstreams: 30-40 minutes each (run simultaneously = 30-40 min total)
- **Total**: 40-55 minutes (vs. 100+ if sequential)

## Quality Checklist
- [ ] Sales collateral emphasizes unique differentiation vs Coupa/Jaggr
- [ ] Customer migration guide is clear and actionable
- [ ] Release notes include technical details and use cases
- [ ] Launch communications are coordinated across all channels
- [ ] Target segment messaging tailored (not one-size-fits-all)
- [ ] Timeline is realistic and achievable
