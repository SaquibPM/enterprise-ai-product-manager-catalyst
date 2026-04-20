---
name: /plan-sprint
description: Create sprint plan with goal alignment, capacity allocation, and tech debt negotiation
category: pm-delivery
tags: [sprint-planning, agile, capacity, goals]
---

## Overview
Generates a detailed sprint plan including sprint goal, capacity allocation across features/tech debt, acceptance criteria clarity, and risk/blocking item tracking. Chains sprint planning with capacity validation and tech debt negotiation.

## Usage
```
/plan-sprint \
  --sprint 14 \
  --sprint-cycle 2-weeks \
  --team-capacity 40 \
  --include-tech-debt true \
  --tech-debt-budget 20% \
  --include-risks true
```

## Skill Chain (Sequential)

### 1. Sprint-Planning (Processing)
- **Contributes**: Feature list, story breakdown, capacity allocation
- **Input**: Product backlog, prioritization, team velocity
- **Process**:
  - Select top-priority features for sprint
  - Break features into stories
  - Assign story points
  - Calculate capacity fit
- **Output**: Draft sprint backlog

### 2. Acceptance-Criteria (Validation)
- **Contributes**: Acceptance criteria clarity, definition of done
- **Input**: Sprint backlog stories
- **Process**:
  - Ensure all stories have clear acceptance criteria
  - Define edge cases and test scenarios
  - Flag stories with ambiguous criteria
- **Output**: Sprint backlog with validated criteria

### 3. Tech-Debt-Negotiation (Sequential, from pm-delivery)
- **Contributes**: Tech debt allocation, justification for tech work
- **Input**: Sprint backlog + tech debt backlog
- **Process**:
  - Allocate 15-25% capacity for tech debt
  - Prioritize tech debt by impact/risk
  - Get engineering buy-in on trade-offs
- **Output**: Final sprint plan with tech debt included

## Data Handoff

```
Product Backlog
    ↓
Sprint Planning
    ↓ (selected stories, points)
Acceptance Criteria
    ↓ (validated criteria)
Tech Debt Negotiation
    ↓ (tech debt allocation)
Final Sprint Plan
    → Sprint goal
    → Feature stories (with points & criteria)
    → Tech debt items
    → Capacity summary
    → Risk register
```

## Execution Model
**Sequential** — Each step builds on previous. Acceptance criteria must be validated before committing. Tech debt negotiation happens last to ensure realistic commitment.

**User interaction points**:
1. Review feature selection and capacity fit
2. Approve/modify acceptance criteria
3. Negotiate tech debt allocation with engineering
4. Sprint kickoff with team

## Output Format

```markdown
# Sprint 14 Plan (May 19-30, 2026)

## Sprint Goal
"Enable legal teams to review contracts 4x faster with AI-powered risk scoring"

## Capacity Summary
- Total capacity: 40 person-days
- Features: 32 person-days (80%)
- Tech debt: 8 person-days (20%)
- Slack: 0% (high confidence in estimates)

## Stories & Acceptance Criteria
### Story 1: [User story]
- Story points: 8
- Acceptance criteria: [list]
- Acceptance criteria: [list]

[More stories...]

## Tech Debt Items
- [Item 1]: [justification]
- [Item 2]: [justification]

## Risks & Blockers
- Risk 1: [description]
- Blocker 1: [description]
```

## Estimated Timeline
- Sprint-Planning: 20-25 minutes
- Acceptance-Criteria: 15-20 minutes
- Tech-Debt-Negotiation: 10-15 minutes
- **Total**: 45-60 minutes

## Quality Checklist
- [ ] Sprint goal is clear and measurable
- [ ] All stories fit capacity (velocity × sprint length)
- [ ] Acceptance criteria are testable
- [ ] Tech debt allocation 15-25% of capacity
- [ ] Engineering team agrees with tech debt trade-offs
- [ ] Risks identified with mitigation plans
- [ ] Team has capacity for unforeseen blockers
