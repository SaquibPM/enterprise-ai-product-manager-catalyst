# CLAUDE.md

## Project Overview

This is an enterprise B2B product management skills marketplace with 37 skills, 13 commands, and 6 workflows across 8 plugins. Each skill provides frameworks, templates, and worked examples for PM activities in complex enterprise environments. Use this file to discover and apply the right skill or workflow for your task.

## How to Use Skills

Each skill is a SKILL.md file located in `{plugin}/skills/{skill-name}/`. When you need help with a PM activity:

1. Find the matching skill in the lookup table below
2. Navigate to the skill's SKILL.md file
3. Follow the embedded frameworks, templates, and worked examples
4. Apply enterprise context (compliance, multi-tenant, procurement cycles, buyer vs. user personas)

## Skill Lookup Table

| PM Request | Skill Location |
|---|---|
| Write a PRD | pm-definition/skills/prd-development |
| Prioritize features | pm-definition/skills/prioritization-frameworks |
| Design an AI agent | pm-agentic-ai/skills/agentic-architecture |
| Evaluate AI system | pm-agentic-ai/skills/ai-evaluation |
| Design guardrails for AI | pm-agentic-ai/skills/guardrails-design |
| Plan a sprint | pm-delivery/skills/sprint-planning |
| Create competitive analysis | pm-discovery/skills/competitive-analysis |
| Build launch plan | pm-launch/skills/gtm-planning |
| Plan customer migration | pm-launch/skills/customer-migration |
| Create release notes | pm-launch/skills/release-notes |
| Review metrics | pm-growth/skills/metrics-analysis |
| Run pricing analysis | pm-growth/skills/pricing-packaging |
| Write decision record | pm-operations/skills/decision-records |
| Set OKRs | pm-leadership/skills/okr-management |
| Coach PM team | pm-leadership/skills/team-coaching |

## How to Use Commands

Commands chain multiple skills to solve larger PM problems. Located in `{plugin}/commands/{command-name}/`:

1. Each command has a markdown file documenting the skill chain and workflow
2. Review the `examples/` folder for sample outputs with real data
3. Follow the command step-by-step, substituting your own inputs
4. Commands typically combine 2-4 skills from the same plugin

## How to Use Workflows

Cross-plugin workflows handle end-to-end PM processes. Located in `workflows/`:

- Feature kickoff: End-to-end feature definition, prioritization, and launch sequencing
- Quarterly planning: OKR setting, roadmap alignment, and execution planning
- Launch sequence: Positioning, go-to-market, customer migration, and metrics setup
- AI feature launch: Design, evaluation, guardrails, and launch for AI-powered features
- Competitive response: Discovery, positioning, and rapid feature planning
- Growth campaign: Metrics, pricing, and launch coordination for growth initiatives

## Enterprise Context

All skills are written for enterprise B2B environments. When generating outputs:

- Ensure compliance with regulated industries (healthcare, finance, government)
- Design for multi-tenant architecture and governance
- Account for procurement cycles and buyer vs. end-user personas
- Include considerations for implementation, migration, and support
- Factor in audit, security, and data residency requirements

## Quality Standards

When using these skills to generate outputs, always include:

- Embedded frameworks with actual formulas, decision trees, or rubrics
- Worked examples with real numbers and enterprise scenarios
- Enterprise-specific considerations and risk factors
- Common pitfalls and how to avoid them
- Templates and checklists ready to customize for your product
