---
name: eval-framework
description: >
  Generate a comprehensive AI evaluation framework with dimensions, test cases, quality gates, and regression testing strategy. Use when launching new AI features, upgrading models, or establishing quality baselines for AI-powered products. Triggers on: eval framework, AI evaluation, model evaluation, AI quality, test AI, benchmark AI.
---

# eval-framework

## Usage

```
/eval-framework
```

Invoke this command when you need to establish a rigorous evaluation strategy for an AI-powered feature. The command synthesizes evaluation best practices with safety and compliance requirements.

## What This Command Does

The eval-framework command generates a complete evaluation strategy document that:

- **Defines evaluation dimensions** — correctness, latency, cost, safety, compliance
- **Designs test datasets** — representative cases, edge cases, adversarial examples
- **Establishes evaluation methods** — deterministic metrics, LLM-as-judge, human review cadence
- **Creates regression testing strategy** — model upgrade and rollback procedures
- **Integrates guardrails** — PII detection, isolation, cost controls, format compliance
- **Sets quality gates** — production readiness criteria before deployment
- **Designs monitoring** — metrics dashboards, alert thresholds, success metrics

Output is a complete, production-ready evaluation framework document (~200-250 lines).

## Skill Chain

This command chains two skills sequentially:

### Step 1: ai-evaluation

**Input**: Feature description, success metrics, user feedback patterns

**Processing**:
- Define primary evaluation dimensions (correctness, latency, cost, faithfulness)
- Design evaluation dataset structure (size, coverage, edge cases)
- Specify evaluation methods (deterministic checks, LLM-as-judge rubrics, human review schedules)
- Develop regression testing strategy for model upgrades
- Establish baseline metrics and thresholds

**Output**: Base evaluation framework with methods, metrics, and testing strategy

### Step 2: guardrails-design

**Input**: Base evaluation framework from Step 1

**Processing**:
- Identify safety evaluation dimensions (PII detection, data isolation, cost ceilings)
- Define compliance testing requirements (regulatory, data residency, audit trails)
- Create guardrail test cases and validation procedures
- Integrate safety metrics into quality gates
- Design monitoring for guardrail violations

**Output**: Complete evaluation framework with safety and compliance integrated

## Data Handoff

After Step 1 completes, the system presents the evaluation framework for user review. Key handoff data includes:

- Evaluation dimension priorities and weightings
- Dataset composition and sampling strategy
- Baseline metrics and target thresholds
- Human review cadence and annotation rubrics

User confirmation at this checkpoint ensures:
- Evaluation dimensions align with product priorities
- Metrics capture meaningful quality signals
- Test dataset represents production traffic patterns

Confirmed framework then flows to Step 2 for safety and compliance overlays.

## Parallelization

**Sequential execution** (not parallel).

Step 2 (guardrails-design) depends on Step 1's (ai-evaluation) output. Guardrails evaluation must integrate with the base evaluation framework to ensure safety metrics are tracked alongside performance metrics.

## User Interaction Points

**After Step 1 (ai-evaluation completes)**:

```
Review the proposed evaluation dimensions and metrics:

Primary Dimensions:
- [Correctness metric and threshold]
- [Latency metric and threshold]
- [Cost metric and threshold]

Secondary Dimensions:
- [Faithfulness metric]
- [Safety signal]

Do these dimensions and thresholds match your product requirements?
[CONFIRM] [ADJUST DIMENSIONS] [ADJUST THRESHOLDS]
```

User can confirm, adjust dimension definitions, or refine thresholds before proceeding to Step 2.

## Output Format

The evaluation framework document follows this structure:

```
# Evaluation Framework: [Feature Name]

## Executive Summary
- Quick description and business impact

## Feature Description
- Input types and expected outputs
- Use cases and user flows
- Success criteria and business metrics

## Evaluation Dimensions
- Primary (correctness, latency, cost)
- Secondary (faithfulness, safety)
- Tertiary (coverage, rarity handling)
- Specific metrics, measurement methods, and thresholds

## Evaluation Dataset Design
- Dataset composition (size, sources, document types)
- Coverage strategy (common cases, edge cases, adversarial cases)
- Annotation methodology and quality assurance
- Update cadence and refresh strategy

## Evaluation Methods
- Deterministic metrics (exact match, field-level accuracy, schema validation)
- LLM-as-Judge (rubrics, scoring, prompt templates)
- Human review (cadence, sample size, annotation process)
- Tool-based validation (regex, format checkers, rule engines)

## Regression Testing Strategy
- Model upgrade playbook (testing before rollout)
- Rollback criteria and procedures
- A/B test design for model changes
- Production monitoring and incident response

## Guardrail Evaluation
- PII detection and handling tests
- Cross-tenant isolation verification
- Cost ceiling and quota enforcement
- Output format and schema compliance

## Quality Gates
- Pre-production testing requirements
- Minimum metrics for deployment
- Exceptions and approval workflows
- Rollback triggers and procedures

## Dashboard & Monitoring
- Weekly metrics and trends
- Alert thresholds and escalation paths
- Incident response procedures
- User feedback integration

## Success Criteria (6-Month Targets)
- Correctness, latency, cost targets
- User satisfaction and adoption metrics
- Safety and compliance targets
```

## Estimated Time

**10-15 minutes** total execution time:
- Step 1 (ai-evaluation): 5-8 minutes
- User review and confirmation: 2-3 minutes
- Step 2 (guardrails-design): 3-5 minutes

## When to Use This Command

- **Launching new AI features** — establish baseline evaluation before release
- **Major model upgrades** — create new regression test suite
- **Quality concerns** — diagnose and fix evaluation blindspots
- **Compliance requirements** — integrate regulatory evaluation requirements
- **Cross-team alignment** — shared evaluation framework document for product, eng, safety

## Integration Points

The generated evaluation framework integrates with:

- **model-serving** command — for A/B testing and canary deployment
- **prompt-engineering** command — for prompt evaluation within broader feature eval
- **observability-setup** command — for metrics collection and alerting
- **incident-response** command — for guardrail violation handling
