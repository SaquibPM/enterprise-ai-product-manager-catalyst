# Enterprise AI Product Manager Catalyst

> **Install one plugin. Get a senior PM's frameworks, templates, and judgment — embedded directly in your AI coding tool.**

You run `/write-spec`. Two minutes later you have a 15-page enterprise PRD with compliance sections, multi-tenant data isolation requirements, and buyer vs. user persona analysis — not a generic template, but a document your VP of Engineering can review today.

You run `/design-ai-agent`. Fifteen minutes later you have a 30-page agent design document covering architecture, guardrails, human oversight, and UX — the kind of deliverable that normally takes a senior PM 2-3 days.

That's what this marketplace does. **37 skills, 13 commands, 6 workflows across 8 plugins** — covering the full product lifecycle from discovery through growth, plus a dedicated agentic AI plugin that has no equivalent anywhere.

```bash
# Clone and use in Claude Code — start using immediately
git clone https://github.com/SaquibPM/enterprise-ai-product-manager-catalyst.git
cd enterprise-ai-product-manager-catalyst
claude --plugin-dir .
```

---

## What This Actually Is

This is **not** a collection of notes, templates, or study materials. It's a working product — a modular skills plugin you install into Claude Code, Claude Cowork, or any AI tool that supports [SKILL.md format](https://agentskills.io/specification).

Once installed, the skills activate automatically. Ask Claude to "write a PRD for my invoice categorization feature" and it pulls in the `prd-development` skill — complete with RICE scoring formulas, compliance section templates, multi-tenant architecture considerations, and a worked example with real numbers. The output is a production-ready document, not a starting point you need to rewrite.

**What makes it different from every other PM skills collection:**

Every skill embeds the actual frameworks — not just framework names. When the prioritization skill uses RICE scoring, the formula is there, the worked example scores 5 real features, and the enterprise-specific weighting adjustments are documented. When the guardrails skill designs a safety layer, it includes pseudocode for each of the 5 guardrail layers, a compliance mapping table for SOC 2/GDPR/HIPAA, and cost control thresholds.

28,000+ lines of embedded methodology. Zero "go read the book" references. Zero placeholder sections.

---

## See It In Action

### Example 1: `/write-spec` — Enterprise PRD in 2 Minutes

**Input:** "Write a PRD for AI-assisted invoice categorization in our S2P platform"

**Output (436 lines):**
- Executive summary with business case and ROI projection
- User personas: Accounts Payable Clerk (daily user) vs. VP Finance (buyer/sponsor)
- Functional requirements with acceptance criteria
- Multi-tenant data isolation architecture
- Compliance section: SOC 2 audit logging, GDPR data residency, SOX financial controls
- API contract with versioning strategy
- Phased rollout plan by customer tier
- Success metrics: categorization accuracy, time-to-close, AP team throughput

### Example 2: `/design-ai-agent` — Agent Design Document in 15 Minutes

**Input:** "Design an autonomous contract analysis agent for our CLM platform"

**Output (887 lines):**
- Evaluator-Optimizer topology with ASCII system diagram
- 4-layer memory architecture (session, conversational, long-term, context window)
- Tool inventory scored across 4 dimensions (capability, reliability, cost, security)
- 5-layer guardrails stack with implementation pseudocode
- SOC 2 + GDPR compliance mapping table
- Human-in-the-loop decision matrix with confidence thresholds
- Cost model: $27.04/contract with 947% ROI analysis
- 4-phase rollout plan: pilot (5 customers) → limited → general → full autonomy

### Example 3: `/competitive-brief` — Competitive Analysis With Parallel Research

**Input:** "Compare our S2P platform against Coupa, SAP Ariba, and Jaggaer"

**What happens:** The command spawns one subagent per competitor. Each independently researches positioning, pricing, features, and recent launches. Results merge into a unified brief with side-by-side comparison matrix, vulnerability/opportunity analysis per competitor, and recommended positioning adjustments.

---

## Quick Start

### Claude Code (CLI)
```bash
git clone https://github.com/SaquibPM/enterprise-ai-product-manager-catalyst.git
cd enterprise-ai-product-manager-catalyst
claude --plugin-dir .
```

Once Claude Code loads, type `/prd-development` or `/agentic-architecture` to invoke any of the 37 skills directly. Or just describe what you need — Claude will automatically activate the right skill.

### Claude Cowork (Desktop)
Select the cloned repo folder as your workspace. Skills activate automatically when you describe a PM task.

### Other AI Tools (Cursor, GitHub Copilot, Windsurf)
```bash
git clone https://github.com/SaquibPM/enterprise-ai-product-manager-catalyst.git
# Copy the skills/ directory to your AI tool's skills folder
```

Compatible with any tool supporting [SKILL.md format](https://agentskills.io/specification).

---

## The 8 Plugins

Each plugin covers one product lifecycle phase and is independently installable.

| Plugin | Focus | Skills | Commands | What You Get |
|--------|-------|:------:|:--------:|-------------|
| **pm-discovery** | Research & validation | 5 | 2 | Teresa Torres CDH interview guides, JTBD mapping, TAM/SAM/SOM sizing with worked examples |
| **pm-definition** | Specs & planning | 5 | 3 | Enterprise PRDs with compliance sections, RICE/ICE/MoSCoW with scoring formulas, roadmap planning with contract-renewal alignment |
| **pm-delivery** | Build & ship | 4 | 1 | Given/When/Then acceptance criteria, sprint capacity planning, tech debt registries with negotiation frameworks |
| **pm-launch** | GTM & rollout | 5 | 2 | Phased rollout by customer tier, migration runbooks, RFP response frameworks, channel activation timing |
| **pm-growth** | Metrics & expansion | 4 | 1 | Metric trees, NRR decomposition, A/B test design with sample size calculators, willingness-to-pay research |
| **pm-operations** | Cross-functional ops | 5 | 1 | Architecture Decision Records, RACI matrices, vendor scorecards with weighted evaluation |
| **pm-leadership** | Vision & team | 4 | 1 | Pyramid principle for exec comms, OKR cascading, PM career ladders and coaching frameworks |
| **pm-agentic-ai** | AI product design | 5 | 2 | Agent topology patterns, eval frameworks, guardrails stacks, HITL design, AI-native UX patterns |

---

## The Agentic AI Plugin — What No One Else Has

The `pm-agentic-ai` plugin addresses an emerging discipline that no other PM skills marketplace covers: product-managing AI systems that reason, decide, and act autonomously.

This isn't about chatbots with retrieval. It's about designing systems where an AI agent processes a contract, decides which clauses are non-standard, escalates high-risk terms to legal, and auto-approves routine renewals — with the right guardrails, oversight, and UX to make it safe and trustworthy.

| Skill | What It Gives You |
|-------|------------------|
| **agentic-architecture** | 4-level Agency Spectrum (when you need agents vs. better automation), 5 agent topology patterns, tool/MCP selection scoring, memory architecture design, orchestration patterns |
| **ai-evaluation** | Eval dimension taxonomy, LLM-as-judge patterns, human evaluation workflows, regression testing playbooks, quality gate design |
| **guardrails-design** | 5-layer guardrails stack (input validation → prompt injection defense → output filtering → cost controls → compliance enforcement), regulatory mapping templates, audit trail design |
| **human-in-loop** | 4-mode HITL spectrum, confidence threshold calibration, approval routing logic, escalation path design, SLA frameworks |
| **ai-native-ux** | Progressive disclosure of AI reasoning, confidence indicators, explainability patterns, user correction loops, trust calibration models |

**Commands:**

`/design-ai-agent` chains all 4 core skills into a sequential workflow that produces a complete, VP-ready agent design document in ~15 minutes. Each step builds on the previous — architecture decisions inform guardrails, guardrails inform oversight thresholds, oversight informs UX patterns.

`/eval-framework` produces an evaluation framework with dimensions, test cases, quality gates, and regression testing plans for any AI-powered feature.

---

## All 13 Commands

| Command | Plugin | What It Produces | Time |
|---------|--------|-----------------|:----:|
| `/write-spec` | pm-definition | Enterprise PRD with compliance, multi-tenant, and procurement sections | ~3 min |
| `/write-api-spec` | pm-definition | API specification with versioning, auth patterns, and rate limiting | ~3 min |
| `/roadmap-update` | pm-definition | Roadmap with audience-specific views (exec, eng, customer) | ~2 min |
| `/synthesize-research` | pm-discovery | Unified insight document from interviews, surveys, and tickets | ~3 min |
| `/competitive-brief` | pm-discovery | Multi-competitor analysis with parallel subagent research | ~5 min |
| `/plan-sprint` | pm-delivery | Sprint plan with goal alignment and capacity checks | ~2 min |
| `/gtm-plan` | pm-launch | Go-to-market plan with parallel workstreams | ~4 min |
| `/stakeholder-update` | pm-launch | Audience-tailored updates (exec, eng, customer versions) | ~3 min |
| `/metrics-review` | pm-growth | Structured metrics review with root cause diagnosis | ~3 min |
| `/decision-record` | pm-operations | Architecture Decision Record with context and consequences | ~2 min |
| `/strategy-brief` | pm-leadership | Strategic brief for executive audiences | ~2 min |
| `/design-ai-agent` | pm-agentic-ai | Complete agent design document (architecture → guardrails → HITL → UX) | ~15 min |
| `/eval-framework` | pm-agentic-ai | AI evaluation framework with test cases and quality gates | ~5 min |

Commands that support parallel execution spawn subagents automatically when your AI tool supports it:

| Command | Pattern | What Each Subagent Does |
|---------|---------|------------------------|
| `/competitive-brief` | 1 per competitor | Research positioning, pricing, features, recent launches |
| `/stakeholder-update` | 1 per audience | Generate exec, engineering, and customer versions |
| `/synthesize-research` | 1 per data source | Analyze interviews, surveys, tickets independently |
| `/gtm-plan` | 1 per workstream | Sales enablement, customer comms, internal readiness |

---

## 6 Cross-Plugin Workflows

Workflows chain commands and skills across plugins for end-to-end PM processes.

| Workflow | Chain | When to Use |
|----------|-------|-------------|
| **feature-kickoff** | Discovery → Spec → Design → Sprint | Starting a new feature from scratch |
| **quarterly-planning** | OKR → Prioritization → Roadmap → Comms | Beginning-of-quarter planning cycle |
| **launch-sequence** | GTM → Enablement → Rollout → Metrics | Coordinating a product launch |
| **competitive-response** | Competitive Brief → Roadmap → Stakeholder Update | Reacting to competitor moves |
| **ai-feature-lifecycle** | Design Agent → Eval → Spec → Launch | Shipping an AI-powered feature end-to-end |
| **customer-discovery** | Interviews → Synthesis → JTBD → Spec | Running a discovery cycle from research to requirements |

---

## Why Enterprise B2B — Not Generic PM

Most PM skills collections are written for consumer products with a single buyer-user persona and a self-serve pricing page. Enterprise B2B product management is a fundamentally different job:

- **Procurement cycles** that last 6-18 months, not a credit card swipe
- **Buyer ≠ User** — the VP who signs the contract has different needs than the analyst who uses the product daily
- **Compliance is structural** — your customers' legal teams review your PRDs, your SOC 2 auditor reads your architecture docs
- **Multi-tenant by default** — every feature needs data isolation, tenant-specific configuration, and governance controls
- **APIs are products** — third parties build against your platform, so breaking changes have contractual consequences

This marketplace encodes that context into every skill. Compliance sections aren't an afterthought bolted onto a consumer template. Multi-tenant considerations aren't a checkbox — they're woven into architecture decisions, rollout plans, and acceptance criteria.

---

## MCP Integration

Skills detect and adapt to available MCP servers. Nothing fails if an MCP is missing — skills fall back gracefully and tell you what additional context they could use.

| MCP Server | What It Enables |
|------------|----------------|
| **Jira** | Pull epics/stories into `/write-spec`, backlog into `/roadmap-update`, velocity into `/metrics-review` |
| **Confluence** | Search prior specs and research, publish ADRs and updates |
| **Notion** | Read/write OKR databases, research notes, knowledge bases |
| **Gmail** | Send stakeholder updates and GTM materials |
| **Figma** | Pull design context into specs and PRDs |
| **Google Calendar** | Sprint ceremony scheduling, CAB meeting coordination |

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
│   Atomic building blocks: embedded frameworks, worked       │
│   examples, enterprise context, common pitfalls, evals      │
│   28,000+ lines across 37 skills — zero stubs               │
└─────────────────────────────────────────────────────────────┘
```

**Skills** are the atomic units. Each is a self-contained SKILL.md (360-1,200+ lines) with the full framework embedded — scoring formulas, decision trees, worked examples with real numbers, enterprise-specific pitfalls, and eval test cases.

**Commands** chain 2-4 skills into a user-invoked action that produces a complete deliverable. Commands document their parallelization patterns for AI tools that support subagent orchestration.

**Workflows** orchestrate commands across plugin boundaries for end-to-end processes like launching a feature or running quarterly planning.

---

## Quality Standards

Every skill in this marketplace ships with:

- **Embedded frameworks** — the actual scoring formulas, decision trees, and templates, not just framework names
- **Worked examples** — at least one complete example with real numbers (e.g., a full RICE scoring for 5 features, a complete A/B test design with sample size calculations)
- **Enterprise context** — compliance, multi-tenant, procurement, and buyer/user dynamics are structural, not bolted on
- **Common pitfalls** — specific anti-patterns with explanations, drawn from real enterprise PM experience
- **Eval test cases** — every skill ships with 3 structured test cases in `evals/evals.json` (111 total across the marketplace)
- **Sample command outputs** — every command includes at least one complete example output showing what the deliverable looks like

---

## Repository Structure

```
enterprise-ai-product-manager-catalyst/
├── README.md
├── CLAUDE.md                               # AI tool skill discovery
├── LICENSE                                  # Apache 2.0
├── CONTRIBUTING.md
├── CHANGELOG.md
├── .claude-plugin/
│   └── plugin.json                          # Plugin manifest
├── skills/                                  # Flat skill directory for Claude Code
│   ├── prd-development/SKILL.md             #   (mirrors from plugin subdirectories)
│   ├── agentic-architecture/SKILL.md
│   ├── ... (37 skills total)
│
├── pm-discovery/                            # 5 skills, 2 commands
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
├── pm-definition/                           # 5 skills, 3 commands
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
├── pm-delivery/                             # 4 skills, 1 command
│   ├── skills/
│   │   ├── sprint-planning/SKILL.md
│   │   ├── acceptance-criteria/SKILL.md
│   │   ├── analytics-instrumentation/SKILL.md
│   │   └── tech-debt-negotiation/SKILL.md
│   └── commands/
│       └── plan-sprint/
│
├── pm-launch/                               # 5 skills, 2 commands
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
├── pm-growth/                               # 4 skills, 1 command
│   ├── skills/
│   │   ├── metrics-analysis/SKILL.md
│   │   ├── experimentation/SKILL.md
│   │   ├── pricing-packaging/SKILL.md
│   │   └── expansion-strategy/SKILL.md
│   └── commands/
│       └── metrics-review/
│
├── pm-operations/                           # 5 skills, 1 command
│   ├── skills/
│   │   ├── stakeholder-comms/SKILL.md
│   │   ├── decision-records/SKILL.md
│   │   ├── vendor-evaluation/SKILL.md
│   │   ├── platform-strategy/SKILL.md
│   │   └── incident-response/SKILL.md
│   └── commands/
│       └── decision-record/
│
├── pm-leadership/                           # 4 skills, 1 command
│   ├── skills/
│   │   ├── vision-strategy/SKILL.md
│   │   ├── okr-management/SKILL.md
│   │   ├── executive-presentations/SKILL.md
│   │   └── team-coaching/SKILL.md
│   └── commands/
│       └── strategy-brief/
│
├── pm-agentic-ai/                           # 5 skills, 2 commands
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
└── workflows/                               # 6 cross-plugin workflows
    ├── feature-kickoff.md
    ├── quarterly-planning.md
    ├── launch-sequence.md
    ├── competitive-response.md
    ├── ai-feature-lifecycle.md
    └── customer-discovery.md
```

---

## Competitive Landscape

| Collection | Skills | Focus | Enterprise Depth | Agentic AI |
|-----------|:------:|-------|:----------------:|:----------:|
| **This marketplace** | **37** | **Enterprise B2B + Agentic AI** | **Deep — structural, not bolted on** | **Complete plugin (5 skills, 2 commands)** |
| phuryn/pm-skills | 65 | Breadth + classic frameworks | Light | None |
| product-on-purpose/pm-skills | 38 | Triple Diamond framework | Light | None |
| deanpeters/Product-Manager-Skills | 47 | Pedagogic rigor + multi-platform | Light | None |
| alirezarezvani/claude-skills | 235+ | Scale across all domains | Medium | Partial |

The difference isn't the count — it's the depth. Each of our 37 skills averages 750+ lines of embedded methodology. Most competing skills are 50-150 lines of framework references.

---

## What Ships in v1.0

- **37 skills** — 28,000+ lines of embedded frameworks, worked examples, and enterprise guidance
- **13 commands** — each with a complete sample output showing the actual deliverable
- **6 workflows** — cross-plugin orchestration for end-to-end PM processes
- **111 eval test cases** — structured tests across all 37 skills (3 per skill)
- **8 independently installable plugins** — use the full lifecycle or just the phases you need

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding skills, commands, and workflows.

## License

[Apache 2.0](LICENSE) — use it, extend it, build on it.

---

*Built by [Saquib Jawed](https://github.com/SaquibPM) — Director of Product Management, building tools for enterprise PMs who ship at scale.*
