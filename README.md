# Enterprise AI Product Manager Catalyst

**37 skills · 13 commands · 6 workflows · 8 plugins**

A modular skills marketplace for enterprise B2B product managers who build at scale — from discovery through growth, with a dedicated plugin for designing, evaluating, and governing agentic AI products.

Built for Claude Code, Claude Cowork, and compatible AI coding tools.

---

## Why This Exists

Most PM skills collections treat product management as a generic discipline. They work fine if you're building a consumer app with a single buyer-user persona and a self-serve pricing page.

Enterprise B2B product management is a different job. You navigate multi-stakeholder procurement cycles that last months. You write PRDs with compliance sections because your customers' legal teams will read them. You design APIs that third parties integrate against and can't break. You balance the needs of the buyer who signs the contract against the user who lives in the product daily. You operate in regulated industries where "move fast and break things" gets you a lawsuit.

This marketplace encodes that context into every skill, every command, and every workflow.

It also includes something no other PM skills collection offers: a complete plugin for **agentic AI product management** — the emerging discipline of designing, evaluating, and governing AI systems that reason, decide, and act autonomously.

---

## Architecture

The marketplace uses a three-layer architecture: **Skills → Commands → Workflows**.

```
┌─────────────────────────────────────────────────────────────┐
│                        WORKFLOWS                            │
│   Cross-plugin chains that orchestrate end-to-end processes │
│   feature-kickoff · quarterly-planning · launch-sequence    │
│   competitive-response · ai-feature-lifecycle · customer-   │
│   discovery                                                 │
├─────────────────────────────────────────────────────────────┤
│                        COMMANDS                             │
│   User-invoked actions that chain 2-4 skills together       │
│   /write-spec · /competitive-brief · /design-ai-agent       │
│   /gtm-plan · /metrics-review · /eval-framework ...         │
├─────────────────────────────────────────────────────────────┤
│                         SKILLS                              │
│   Atomic building blocks encoding frameworks + enterprise   │
│   context. Each skill is self-contained with embedded       │
│   methodologies, worked examples, and common pitfalls.      │
└─────────────────────────────────────────────────────────────┘
```

**Skills** are the atomic units. Each encodes a specific PM framework with embedded methodology — not just a framework name, but the actual scoring formula, the worked example, the enterprise-specific considerations. A skill never defers its core content to an external file that isn't shipped.

**Commands** chain 2-4 skills together into a user-invoked workflow. When you run `/competitive-brief`, it chains competitive-analysis → market-sizing → vendor-evaluation and produces a structured output. Commands that benefit from parallelism (like `/competitive-brief` with one subagent per competitor) document their parallelization pattern.

**Workflows** orchestrate commands and skills across plugin boundaries for end-to-end processes like launching a feature or running quarterly planning.

---

## Plugins

Each plugin covers one product lifecycle phase and is independently installable.

| Plugin | Focus | Skills | Commands | Key Frameworks |
|--------|-------|:------:|:--------:|----------------|
| **pm-discovery** | Research & validation | 5 | 2 | Teresa Torres CDH, JTBD, TAM/SAM/SOM |
| **pm-definition** | Specs & planning | 5 | 3 | RICE/ICE/MoSCoW, Now-Next-Later, OpenAPI |
| **pm-delivery** | Build & ship | 4 | 1 | Given/When/Then, capacity planning, debt registries |
| **pm-launch** | GTM & rollout | 5 | 2 | Launch tiers, phased rollout, RFP response |
| **pm-growth** | Metrics & expansion | 4 | 1 | Metric trees, NRR, willingness-to-pay |
| **pm-operations** | Cross-functional ops | 5 | 1 | ADRs, RACI, vendor scorecards |
| **pm-leadership** | Vision & team | 4 | 1 | Pyramid principle, OKR cascading, PM ladders |
| **pm-agentic-ai** | AI product design | 5 | 2 | Agent topology, eval frameworks, HITL patterns |

---

## The Differentiator: pm-agentic-ai

This plugin has no equivalent in any competing PM skills marketplace. It addresses the emerging discipline of product-managing AI systems that reason, decide, and act — not chatbots with retrieval, but genuine agents.

| Skill | What It Covers |
|-------|---------------|
| **agentic-architecture** | Agent topology (single vs. multi-agent), tool/MCP selection, memory architecture, orchestration patterns, and the critical distinction between automation and agency |
| **ai-evaluation** | Eval dimensions, eval datasets, LLM-as-judge patterns, human evaluation workflows, regression testing across model upgrades |
| **guardrails-design** | Input validation, output filtering, scope limiting, cost controls, compliance guardrails, audit trails, data residency |
| **human-in-loop** | Approval workflows, confidence thresholds, exception handling, graceful degradation for regulated contexts |
| **ai-native-ux** | Progressive disclosure of reasoning, confidence indicators, explainability, user corrections that improve the system, trust calibration |

**Commands:**

`/design-ai-agent` chains agentic-architecture → guardrails-design → human-in-loop → ai-native-ux to produce a complete agent design document.

`/eval-framework` chains ai-evaluation → guardrails-design to produce an evaluation framework with dimensions, test cases, and quality gates.

---

## Workflows

Cross-plugin workflows chain skills and commands for end-to-end processes.

| Workflow | Chain | When to Use |
|----------|-------|-------------|
| **feature-kickoff** | Discovery → Spec → Design → Sprint | Starting a new feature from scratch |
| **quarterly-planning** | OKR → Prioritization → Roadmap → Comms | Beginning-of-quarter planning cycle |
| **launch-sequence** | GTM → Enablement → Rollout → Metrics | Coordinating a product launch |
| **competitive-response** | Competitive Brief → Roadmap → Stakeholder Update | Reacting to competitor moves |
| **ai-feature-lifecycle** | Design Agent → Eval → Spec → Launch | Shipping an AI-powered feature |
| **customer-discovery** | Interviews → Synthesis → JTBD → Spec | Running a discovery cycle |

---

## Commands Quick Reference

| Command | Plugin | What It Produces |
|---------|--------|-----------------|
| `/synthesize-research` | pm-discovery | Unified insight document from multiple research inputs |
| `/competitive-brief` | pm-discovery | Structured competitive comparison with parallel analysis |
| `/write-spec` | pm-definition | Enterprise PRD with compliance and multi-tenant sections |
| `/write-api-spec` | pm-definition | API specification with versioning and auth patterns |
| `/roadmap-update` | pm-definition | Updated roadmap with audience-specific views |
| `/plan-sprint` | pm-delivery | Sprint plan with goal alignment and capacity checks |
| `/gtm-plan` | pm-launch | Go-to-market plan with parallel workstreams |
| `/stakeholder-update` | pm-launch | Audience-specific updates (exec, eng, customer) |
| `/metrics-review` | pm-growth | Structured metrics review with diagnosis |
| `/decision-record` | pm-operations | Architecture Decision Record with context |
| `/strategy-brief` | pm-leadership | Strategic brief for executive audiences |
| `/design-ai-agent` | pm-agentic-ai | Agent design document (architecture, safety, UX, eval) |
| `/eval-framework` | pm-agentic-ai | AI evaluation framework with test cases and gates |

---

## Subagent Orchestration

Commands that benefit from parallelism document their subagent patterns. When running in Claude Code or Cowork with subagent support, these commands can execute significantly faster.

| Command | Pattern | What Each Subagent Does |
|---------|---------|------------------------|
| `/competitive-brief` | 1 per competitor | Research positioning, pricing, features, launches |
| `/stakeholder-update` | 1 per audience | Generate exec, engineering, and customer versions |
| `/synthesize-research` | 1 per data source | Analyze interviews, surveys, tickets independently |
| `/gtm-plan` | 1 per workstream | Sales enablement, customer comms, internal readiness |

Commands marked as sequential-only (`/write-spec`, `/write-api-spec`, `/roadmap-update`, `/decision-record`) require iterative user dialogue and should not be parallelized.

---

## MCP Integration

Skills detect and adapt to available MCP servers. Nothing fails if an MCP is missing — skills fall back gracefully and inform you once about what additional context they could use.

| MCP Server | What It Enables |
|------------|----------------|
| **Jira** | Pull epics/stories into `/write-spec`, backlog into `/roadmap-update`, velocity into `/metrics-review` |
| **Confluence** | Search prior specs and research, publish ADRs and updates |
| **Notion** | Read/write OKR databases, research notes, knowledge bases |
| **Gmail** | Send stakeholder updates and GTM materials |
| **Figma** | Pull design context into specs and PRDs |
| **Google Calendar** | Sprint ceremony scheduling, CAB meeting coordination |

---

## Enterprise B2B Context

Every skill in this marketplace is written with enterprise context baked in, not bolted on. Here is what that means in practice:

**In PRDs:** Compliance sections (SOC 2, GDPR, HIPAA implications), multi-tenant data isolation requirements, buyer vs. user persona analysis, procurement enablement notes.

**In Roadmaps:** Customer commitment tracking, contract renewal alignment, partner/ISV dependency mapping, regulatory deadline awareness.

**In Launch Plans:** Phased rollout by customer tier, migration runbooks, partner channel activation timing, procurement cycle awareness for pipeline impact.

**In Metrics:** Net revenue retention alongside product engagement, time-to-value by customer segment, adoption curves segmented by buyer vs. user persona.

**In AI Features:** Data residency requirements, audit trail design, compliance guardrails for regulated industries, enterprise-grade cost controls.

---

## Installation

### Claude Code (CLI)

```bash
# Install all plugins
claude plugins install saquibjawed/enterprise-ai-product-manager-catalyst

# Install a single plugin
claude plugins install saquibjawed/enterprise-ai-product-manager-catalyst/pm-agentic-ai
```

### Claude Cowork (Desktop)

Install from the plugin marketplace: search for `enterprise-ai-product-manager-catalyst`.

### Manual Installation

```bash
git clone https://github.com/saquibjawed/enterprise-ai-product-manager-catalyst.git
# Copy desired plugin folders to your Claude skills directory
```

### Other AI Tools

Skills follow the [Agent Skills Specification](https://agentskills.io/specification) and are compatible with tools that support SKILL.md format, including Cursor, GitHub Copilot, and Windsurf.

---

## Repository Structure

```
enterprise-ai-product-manager-catalyst/
├── README.md
├── LICENSE                              # Apache 2.0
├── CONTRIBUTING.md
├── CHANGELOG.md
├── .claude-plugin/
│   └── plugin.json                      # Plugin manifest
│
├── pm-discovery/                        # 5 skills, 2 commands
│   ├── skills/
│   │   ├── customer-interviews/SKILL.md
│   │   ├── competitive-analysis/SKILL.md
│   │   ├── research-synthesis/SKILL.md
│   │   ├── market-sizing/SKILL.md
│   │   └── cab-management/SKILL.md
│   └── commands/
│       ├── synthesize-research/
│       └── competitive-brief/
│
├── pm-definition/                       # 5 skills, 3 commands
│   ├── skills/
│   │   ├── prd-development/SKILL.md
│   │   ├── api-spec-design/SKILL.md
│   │   ├── prioritization-frameworks/SKILL.md
│   │   ├── roadmap-planning/SKILL.md
│   │   └── compliance-review/SKILL.md
│   └── commands/
│       ├── write-spec/
│       ├── write-api-spec/
│       └── roadmap-update/
│
├── pm-delivery/                         # 4 skills, 1 command
│   ├── skills/
│   │   ├── sprint-planning/SKILL.md
│   │   ├── acceptance-criteria/SKILL.md
│   │   ├── analytics-instrumentation/SKILL.md
│   │   └── tech-debt-negotiation/SKILL.md
│   └── commands/
│       └── plan-sprint/
│
├── pm-launch/                           # 5 skills, 2 commands
│   ├── skills/
│   │   ├── gtm-planning/SKILL.md
│   │   ├── sales-enablement/SKILL.md
│   │   ├── release-notes/SKILL.md
│   │   ├── rfp-response/SKILL.md
│   │   └── customer-migration/SKILL.md
│   └── commands/
│       ├── gtm-plan/
│       └── stakeholder-update/
│
├── pm-growth/                           # 4 skills, 1 command
│   ├── skills/
│   │   ├── metrics-analysis/SKILL.md
│   │   ├── experimentation/SKILL.md
│   │   ├── pricing-packaging/SKILL.md
│   │   └── expansion-strategy/SKILL.md
│   └── commands/
│       └── metrics-review/
│
├── pm-operations/                       # 5 skills, 1 command
│   ├── skills/
│   │   ├── stakeholder-comms/SKILL.md
│   │   ├── decision-records/SKILL.md
│   │   ├── vendor-evaluation/SKILL.md
│   │   ├── platform-strategy/SKILL.md
│   │   └── incident-response/SKILL.md
│   └── commands/
│       └── decision-record/
│
├── pm-leadership/                       # 4 skills, 1 command
│   ├── skills/
│   │   ├── vision-strategy/SKILL.md
│   │   ├── okr-management/SKILL.md
│   │   ├── executive-presentations/SKILL.md
│   │   └── team-coaching/SKILL.md
│   └── commands/
│       └── strategy-brief/
│
├── pm-agentic-ai/                       # 5 skills, 2 commands
│   ├── skills/
│   │   ├── agentic-architecture/SKILL.md
│   │   ├── ai-evaluation/SKILL.md
│   │   ├── guardrails-design/SKILL.md
│   │   ├── human-in-loop/SKILL.md
│   │   └── ai-native-ux/SKILL.md
│   └── commands/
│       ├── design-ai-agent/
│       └── eval-framework/
│
└── workflows/                           # 6 cross-plugin workflows
    ├── feature-kickoff.md
    ├── quarterly-planning.md
    ├── launch-sequence.md
    ├── competitive-response.md
    ├── ai-feature-lifecycle.md
    └── customer-discovery.md
```

---

## Activity Coverage

This marketplace maps to 41 enterprise PM activities across 8 lifecycle phases. Every skill traces back to at least one activity.

| Phase | Activities | Plugin |
|-------|-----------|--------|
| Discovery | Customer interviews, competitive analysis, feedback synthesis, JTBD mapping, market sizing, design thinking, CAB management | pm-discovery |
| Definition | PRD/spec writing, prioritization, roadmap planning, technical feasibility, UX collaboration, API design, compliance review | pm-definition |
| Delivery | Sprint planning, standups/unblocking, UAT/QA, analytics instrumentation, tech debt negotiation | pm-delivery |
| Launch | GTM planning, sales enablement, release notes, documentation, customer migration, RFP response | pm-launch |
| Growth | Metrics review, experimentation, customer success, pricing/packaging, expansion strategy | pm-growth |
| Operations | Stakeholder comms, cross-functional alignment, vendor management, incident response, budget planning, platform strategy | pm-operations |
| Leadership | Vision/strategy, OKR management, team coaching, executive presentations | pm-leadership |
| Agentic AI | Agent architecture, AI evaluation, guardrails/governance, human-in-loop, AI-native UX | pm-agentic-ai |

---

## Quality Standards

Every skill in this marketplace meets these standards:

- **Embedded frameworks** — scoring formulas, decision trees, and templates are included in the skill, not just named
- **Worked examples** — at least one complete example showing input → output (e.g., a full RICE scoring for 5 features)
- **Enterprise context** — compliance, multi-tenant, procurement, and buyer/user dynamics are visible, not generic
- **Common pitfalls** — specific anti-patterns with explanations, not generic warnings
- **Eval test cases** — every skill ships with test cases in `evals/evals.json`
- **Sample command outputs** — every command ships with at least one example output

---

## Design Principles

**Enterprise-native, not enterprise-adapted.** Every skill is written for the enterprise context from the ground up. Compliance sections aren't an afterthought bolted onto a consumer PM template.

**Frameworks embedded, not referenced.** When a skill uses RICE scoring, the formula is there. When it uses Teresa Torres' Continuous Discovery Habits, the interview question frameworks are there. No skill tells you to "go read the book."

**MCP-adaptive, not MCP-dependent.** Skills detect available integrations and use them when present. Nothing breaks when Jira isn't connected — the skill adapts and tells you what you're missing.

**Parallelism-aware.** Commands document whether they can run subagents in parallel and what each subagent does. Sequential-only commands are marked as such.

**VP-scannable.** All outputs are written for an audience that scans before they read. Executive summaries first, details on demand.

---

## Competitive Landscape

| Collection | Skills | Differentiator | Enterprise Depth | Agentic AI |
|-----------|:------:|---------------|:----------------:|:----------:|
| **This marketplace** | 37 | Enterprise B2B + Agentic AI PM | Deep | Complete plugin |
| phuryn/pm-skills | 65 | Breadth + classic frameworks | Light | None |
| product-on-purpose/pm-skills | 31 | Triple Diamond framework | Light | None |
| deanpeters/Product-Manager-Skills | 47 | Pedagogic rigor + multi-platform | Light | None |
| alirezarezvani/claude-skills | 235+ | Scale across all domains | Medium | Partial |

---

## What Ships in v1.0

All 8 plugins are complete with production-grade content:

- [x] **pm-agentic-ai** — 5 skills, 2 commands, 2 sample outputs, 15 eval test cases
- [x] **pm-definition** — 5 skills, 3 commands, 3 sample outputs, 15 eval test cases
- [x] **pm-discovery** — 5 skills, 2 commands, 2 sample outputs, 15 eval test cases
- [x] **pm-launch** — 5 skills, 2 commands, 2 sample outputs, 15 eval test cases
- [x] **pm-delivery** — 4 skills, 1 command, 1 sample output, 12 eval test cases
- [x] **pm-growth** — 4 skills, 1 command, 1 sample output, 12 eval test cases
- [x] **pm-operations** — 5 skills, 1 command, 1 sample output, 15 eval test cases
- [x] **pm-leadership** — 4 skills, 1 command, 1 sample output, 12 eval test cases
- [x] 6 cross-plugin workflows with full content
- [x] 111 eval test cases across all 37 skills
- [x] 13 sample command outputs

**By the numbers:** 37 skills containing 28,000+ lines of embedded frameworks, worked examples, and enterprise-specific guidance. Every skill includes at least one complete worked example with real numbers — something no competing PM skills marketplace offers.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding skills, commands, and workflows.

---

## License

[Apache 2.0](LICENSE) — use it, extend it, build on it.

---

*Built by [Saquib Jawed](https://github.com/saquibjawed), Director of Product Management, for enterprise PMs who build at scale.*
