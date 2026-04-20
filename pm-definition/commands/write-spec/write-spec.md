---
name: /write-spec
description: Generate enterprise PRD with compliance sections, acceptance criteria, and cross-functional requirements
category: pm-definition
tags: [prd, specification, requirements, compliance]
---

## Overview
Writes a comprehensive enterprise PRD (Product Requirements Document) including compliance sections, technical acceptance criteria, and security requirements. Chains PRD development with compliance review and acceptance criteria definition.

## Usage
```
/write-spec \
  --feature "AI-powered contract risk scoring" \
  --audience engineering,legal,compliance,security \
  --include-compliance true \
  --include-acceptance-criteria true \
  --compliance-frameworks sox,hipaa,gdpr \
  --output-format prd-detailed
```

## Skill Chain (Sequential)

### 1. PRD-Development (Processing)
- **Contributes**: Core feature definition, user stories, technical requirements
- **Input**: Feature brief, customer research, competitive context
- **Process**:
  - Define user personas and use cases
  - Write user stories with acceptance criteria
  - Detail technical requirements
  - Define success metrics
- **Output**: Draft PRD (all sections except compliance)

### 2. Compliance-Review (Validation)
- **Contributes**: Regulatory requirements, compliance checkpoints
- **Input**: Draft PRD
- **Process**:
  - Identify applicable regulations (SOX, HIPAA, GDPR, etc.)
  - Map features to compliance requirements
  - Add security checkpoints
  - Flag compliance risks
- **Output**: Compliance requirements section
- **Interaction point**: PM reviews compliance gaps and decides on scope

### 3. Acceptance-Criteria (Sequential, from pm-delivery)
- **Contributes**: QA-ready acceptance criteria, test scenarios
- **Input**: Completed PRD + compliance requirements
- **Process**:
  - Translate stories to testable criteria
  - Define edge cases and error scenarios
  - Create test data requirements
- **Output**: Final PRD with testable acceptance criteria

## Data Handoff

```
Feature Brief
    ↓
PRD Development
    ↓ (draft PRD)
Compliance Review
    ↓ (compliance requirements)
Acceptance Criteria
    ↓ (testable criteria)
Final PRD
    → User stories with acceptance criteria
    → Compliance sections
    → Security & privacy requirements
    → Success metrics
```

## Execution Model
**Sequential** — Each step requires output from previous. Compliance review must see draft PRD. Acceptance criteria must see compliance requirements to ensure tests cover compliance.

**User interaction points**:
1. Review draft PRD structure
2. Approve/modify compliance scope
3. Review acceptance criteria for clarity
4. Final PRD sign-off

## Output Format

```markdown
# Product Requirements Document
## Feature: [Title]

## Executive Summary
- 2-3 sentence overview
- Business impact
- Target launch date

## User Stories
### Story 1: [User role] wants to [goal] so that [benefit]
- **Acceptance Criteria**: 
  - [ ] Criteria 1
  - [ ] Criteria 2
- **Compliance Notes**: [if applicable]

## Technical Requirements
- [list]

## Compliance & Security
- Applicable frameworks: [list]
- Data classification: [level]
- Encryption requirements: [details]
- Audit trail: [requirements]

## Success Metrics
- [KPIs]

## Out of Scope
- [list]
```

## Estimated Timeline
- PRD-Development: 30-40 minutes
- Compliance-Review: 15-20 minutes
- Acceptance-Criteria: 15-20 minutes
- **Total**: 60-80 minutes

## Quality Checklist
- [ ] 5+ detailed user stories with acceptance criteria
- [ ] All acceptance criteria are testable (no vague language)
- [ ] Compliance section addresses all relevant frameworks
- [ ] Security requirements are specific (not generic)
- [ ] Success metrics are measurable
- [ ] Out of scope is clearly defined
- [ ] Engineering and compliance teams reviewed
