---
name: AI Feature Lifecycle
description: Full-cycle workflow for designing, evaluating, specifying, and launching AI-native features
workflow_type: sequential_parallel_gates
estimated_duration: "1 week of PM work orchestrated across multiple sessions"
plugin_chain: ["ai-design", "ml-ops", "specification", "compliance", "launch"]
---

## Overview

The AI Feature Lifecycle workflow is purpose-built for AI/ML features where traditional product development practices must be adapted. Unlike standard features, AI features require evaluation frameworks before specifications can be finalized, guardrails review throughout the development cycle, and mandatory compliance review specific to AI systems (bias, transparency, hallucination risk).

This workflow chains architecture design → evaluation framework → spec development → compliance review → launch planning into a 6-step sequence where each step gates the next. AI-specific considerations are embedded at every stage.

## Key Differences from Standard Features

| Standard Feature | AI Feature |
|-----------------|-----------|
| Spec → Implementation | Design → Evaluation → Spec → Implementation |
| Binary success/failure | Probabilistic outcomes, success metrics must be defined early |
| Once shipped, feature is done | Requires ongoing monitoring and retraining |
| Design assumptions are validated in design review | Design assumptions about AI behavior must be validated in eval framework |
| Compliance review is about data/privacy | Compliance review includes bias, transparency, hallucination risk, model governance |
| User testing is optional | User testing is mandatory (adversarial testing for guardrails) |
| Launch checklist is standard | Launch checklist includes model performance monitoring, drift detection, human-in-loop safety |

## Skills Involved

| Skill | Plugin | Purpose |
|-------|--------|---------|
| **agentic-architecture** | ai-design | Design AI agent architecture, define inputs/outputs, decision trees, action spaces |
| **ai-evaluation** | ml-ops | Build evaluation framework to assess AI quality before spec finalization |
| **guardrails-design** | ml-ops | Design safety mechanisms and human-in-loop workflows to prevent AI failures |
| **human-in-loop** | ml-ops | Plan ongoing user feedback loop to improve AI model performance |
| **ai-native-ux** | design | Design user experience for AI systems (output confidence, explain reasoning, correction workflows) |
| **prd-development** | specification | Create PRD for AI feature with performance requirements and evaluation criteria |
| **compliance-review** | compliance | Review AI-specific compliance (bias, transparency, model governance) |
| **gtm-planning** | launch | Develop launch strategy specific to AI features (phased rollout, monitoring) |

## Step-by-Step Sequence

### Step 1: AI Architecture & Design (Session 1 - Day 1-2)
**Owner:** PM + AI Architect + Design Lead  
**Input:** Feature vision (what problem should AI solve?), user context, constraints  
**Output:** AI architecture document with design decisions, input/output specs, decision flows

Run the **agentic-architecture** skill to:
- Define what the AI should accomplish (inference goal)
- Specify inputs: What data is the AI working with? What signals inform decisions?
- Specify outputs: What should the AI decide, recommend, or do?
- Document decision logic: Is this a classification, ranking, generation, or multi-step agent?
- Define action space: What can the AI actually do? Any constraints?
- Create decision tree/flowchart: How does the AI move from input to output?
- Identify decision points where humans must intervene
- Document failure modes: What can go wrong? (hallucinations, biased decisions, out-of-scope requests)
- Assess feasibility: Is this a solved problem or research-stage?

**Architecture Example (e.g., "AI Shift Scheduling Assistant"):**
```
Feature Vision: Help ops managers schedule shifts faster using AI recommendations

INPUTS:
- Current schedule (structured data)
- Available employees with skills/preferences (database)
- Shift requirements (coverage needed per role/time)
- Historical substitution patterns (what shifts are easy/hard to fill)
- Customer constraints (don't schedule Bob before 10am, etc.)

OUTPUTS:
- Recommended schedule (specific shifts assigned to employees)
- Confidence score (how confident is AI in this recommendation?)
- Explanation (why did AI suggest this assignment?)
- Alternative options (3 other valid schedules ranked by quality)

DECISION LOGIC:
  Input: Current shift requirements
  → Step 1: Identify available employees for each shift (filter by skills)
  → Step 2: Rank candidates by preference + historical data
  → Step 3: Optimize for coverage + fairness + employee satisfaction
  → Step 4: Return top recommendation + 3 alternatives
  → Output: Recommended schedule with confidence score

FAILURE MODES & MITIGATIONS:
1. AI violates labor laws (e.g., no breaks) → Validate against rules before returning
2. AI hallucinates employee (suggests someone not on roster) → Validate all names in database
3. AI suggests unfair schedule (discriminates against protected group) → Check fairness metrics
4. AI doesn't understand special requests → Human must review and override
5. Edge case: Employee on leave but AI suggests them → Must check leave calendar

ACTION SPACE:
- AI can recommend, but NOT auto-apply to production
- Human must review and approve before schedule takes effect
- AI can apply to draft/simulation, human must click "Confirm"

FEASIBILITY ASSESSMENT:
- Input processing: Solved (standard ETL)
- Ranking algorithm: Solved (combinatorial optimization exists)
- Constraints engine: Research-stage (need custom rules for complex edge cases)
- Fairness checking: Partially solved (need custom metrics for scheduling domain)
→ Verdict: Medium feasibility, 60-70% confidence this will work well out of the box
```

**User Interaction Point:** AI architect and design lead confirm architecture is sound. PM validates that this approach actually solves the customer problem. Any "research-stage" components get flagged for deeper exploration in Step 2 (Evaluation). 1 hour sync.

**Data Handoff:** Architecture document becomes the spec for Step 2 (Evaluation Framework) and Step 5 (User Experience Design).

---

### Step 2: Evaluation Framework (Session 1-2, Day 2-4)
**Owner:** PM + Data Science + ML Engineer  
**Input:** Architecture from Step 1  
**Output:** Evaluation framework with metrics, test cases, success criteria

Run the **ai-evaluation** skill to:
- Define success metrics: How do we measure AI quality? (accuracy, F1, ranking NDCG, user satisfaction, etc.)
- Create test datasets: What edge cases must we evaluate? (boundary conditions, fairness tests, adversarial examples)
- Build evaluation rubric: How do humans grade AI output quality? (scale: 1-5, specific criteria)
- Establish baselines: What's the human performance on this task? What's the current non-AI baseline?
- Plan iterative testing: What's the minimum viable quality threshold to ship?
- Document failure thresholds: At what point do we NOT ship? (e.g., if fairness metric below X, stop)
- Create monitoring plan: How will we track AI quality in production?

**Evaluation Framework Example:**
```
AI FEATURE: Shift Scheduling Assistant

SUCCESS METRICS:

1. Accuracy Metrics
   - Coverage success rate: % of schedules that meet all shift coverage requirements
   - Goal: 95%+ (human baseline: 98% with 2hrs manual work per week)
   - Acceptable for launch: 90%+ with human review

2. Fairness Metrics
   - Shift distribution fairness: Do some employees get all desirable shifts?
   - Metric: Gini coefficient of shift hours across team (0=fair, 1=unfair)
   - Goal: <0.25 (human baseline: ~0.30 due to preferences)
   - Red line: >0.40 = STOP LAUNCH (likely discriminatory)

3. User Satisfaction
   - "How satisfied are you with AI-suggested schedule?" (1-5 scale)
   - Goal: 4.0+ average rating
   - Acceptable: 3.5+ for MVP launch

4. Latency
   - Time to generate schedule recommendation
   - Goal: <5 seconds for typical team size (50 people, 100 shifts)
   - MVP acceptable: <30 seconds

TEST CASES:

Category: Happy Path
  TC1: Standard team, no constraints → AI generates valid schedule
  TC2: Team with evening preferences → AI respects preferences
  TC3: Sick leave + last-minute changes → AI handles disruptions

Category: Fairness Testing
  TC4: New employee (no history) → Treated fairly vs. veterans
  TC5: Gender/race protected attributes → No discrimination detected
  TC6: Employee with disability → Accommodations respected

Category: Edge Cases & Failure Modes
  TC7: Impossible scenario (more shifts than employees) → Graceful degradation
  TC8: All employees unavailable (all on leave) → Clear error message
  TC9: Data corruption (missing employee record) → Don't suggest that employee
  TC10: Adversarial input (request violates labor law) → Catch and explain violation

HUMAN EVALUATION RUBRIC:

Each AI suggestion graded by scheduler on:
1. Coverage: Does it meet all shift requirements? (Y/N)
2. Fairness: Fair workload distribution? (1-5 scale)
3. Feasibility: Can we actually execute this? (1-5 scale)
4. Completeness: Are there gaps? (1-5 scale)
5. Explanation: Is AI reasoning clear? (1-5 scale)

EVALUATION PROCESS:

Phase 1: Dev Evaluation (internal)
- Run 100 test cases (various team sizes, constraint types)
- Human reviewer grades all outputs
- Success criteria: 95%+ coverage + 4.0+ average quality rating
- Improvement: Iterate on algorithm until criteria met

Phase 2: Beta User Evaluation (real customers, controlled)
- 10 beta customers use AI for 2 weeks
- They rate schedules before/after using AI
- Success: 3.5+ average satisfaction, <2 error reports per customer
- Feedback: Gather specific suggestions for improvement

Phase 3: Pre-Launch Validation (production readiness)
- Final fairness audit on real production data
- Latency testing under production load
- Rollback plan testing (can we quickly revert?)
- Success: All metrics pass, monitoring in place

FAILURE THRESHOLDS (STOP SHIP):

- Fairness metric (Gini) > 0.40 → Likely discriminatory, audit required
- Coverage success rate < 85% → Too unreliable for production
- New user satisfaction < 3.0 → Feature not providing value
- Any data privacy violation detected → Security review required
- Ability to explain AI decision is <60% of cases → Transparency failure
```

**User Interaction Point:** PM reviews eval framework and confirms success metrics are achievable and meaningful. Data science lead confirms test cases cover critical failure modes. Leadership reviews failure thresholds and confirms they're willing to delay launch if metrics aren't met. 1.5 hour working session.

**Data Handoff:** Evaluation framework becomes the spec for Step 3 (PRD Development). Success metrics defined here become launch criteria.

**CRITICAL GATE:** If eval framework reveals fundamental problems with architecture (e.g., fairness metrics impossible to meet), STOP and return to Step 1. Don't proceed to spec if architecture is fundamentally flawed.

---

### Step 3: Guardrails & Human-in-Loop Design (Session 2, Day 3-4)
**Owner:** PM + Design + AI Safety Lead  
**Input:** Architecture (Step 1), Evaluation framework (Step 2)  
**Output:** Guardrails design with safety mechanisms and human-in-loop workflows

Run the **guardrails-design** skill to:
- Identify where AI could cause harm (wrong decision, unfair outcome, hallucination)
- Design safety guardrails: Rules/constraints that prevent AI failures
- Plan human-in-loop workflows: Where must humans review/override AI?
- Define confidence thresholds: When does AI request human help?
- Design explainability: How do we help users understand AI reasoning?
- Create feedback mechanism: How do users teach AI to improve?

**Guardrails Design Example:**
```
GUARDRAILS FOR AI SHIFT SCHEDULER:

Guardrail 1: Rule-Based Validation
  - Check: All employees in schedule exist in database
  - Check: No employee scheduled for >10 hours/day (labor law)
  - Check: No employee scheduled <12 hours between shifts
  - Check: All required shifts are covered
  - If any fails: Don't show AI recommendation, show error with reason

Guardrail 2: Fairness Constraints
  - Check: No employee gets >20% more shifts than team average
  - Check: Shift distribution matches historical pattern (within 10%)
  - If violated: Flag to human with "This violates fairness. Adjust?"
  - Design: Separate "I confirm this is intentional" button

Guardrail 3: Explainability Display
  - For each shift assignment, show: "Why did AI suggest Bob for this shift?"
  - Answer: "Bob has matching skills, prefers evening shifts, and has open capacity"
  - Design: Expandable explanation for each assignment

Guardrail 4: Confidence-Based Workflow
  - High confidence (>90%): Show recommendation, ask "Approve?"
  - Medium confidence (70-90%): Show recommendation + alternatives, ask "Which do you prefer?"
  - Low confidence (<70%): Ask human to manually assign, AI provides "helpful suggestions"
  - Edge case: If AI confidence <50%, don't show recommendation at all

Guardrail 5: Human Override & Feedback
  - User can click any AI suggestion and change it
  - System asks: "Why did you change this?" (helps AI improve)
  - Feedback options: "Better schedule", "Violates constraint I didn't tell AI about", "Just prefer this"
  - Feedback is logged and fed back to model for retraining

Guardrail 6: Monitoring & Escalation
  - Track: % of AI suggestions that humans override (target: <20%)
  - Track: Patterns in overrides (specific employee? specific shift type?)
  - Alert: If override rate > 30% = notify AI team, may need retraining
  - Escalation: If fairness metric drifts in production = pause AI, manual mode

Guardrail 7: Adversarial Testing
  - User tries to game system: "Can you assign Bob all weekend shifts?"
  - AI should: Suggest this violates fairness, offer alternatives
  - Design: Show impact of request ("This gives Bob 4x more hours than others")
```

**User Interaction Point:** Safety lead reviews guardrails and confirms they're comprehensive. Design confirms user experience for explainability is clear. PM validates that human-in-loop workflows don't create bottlenecks. 1 hour sync.

**Data Handoff:** Guardrails design flows into Step 4 (UX Design) and Step 6 (Compliance Review).

---

### Step 4: AI-Native UX Design (Session 2, Day 4)
**Owner:** Design Lead + PM  
**Input:** Architecture (Step 1), Guardrails (Step 3)  
**Output:** UX designs, prototype, user testing results

Run the **ai-native-ux** skill to:
- Design AI output display: How do users see and understand AI recommendations?
- Design confidence indicators: How do we show uncertainty visually?
- Design explanation UX: How do users understand AI reasoning?
- Design correction workflows: How do users correct AI and provide feedback?
- Create prototype and test with users
- Assess learnability: Will new users understand how to use AI feature?
- Test edge cases: How does UX handle low-confidence or error states?

**UX Design Example:**
```
SHIFT SCHEDULER UI - AI RECOMMENDATIONS VIEW:

[AI RECOMMENDED SCHEDULE - Week of April 15]

┌─ Coverage Status: 98% ✓ All shifts assigned
│
├─ [Shift] Monday, 9am-5pm   →  Alice (High confidence 94%)
│  📊 Why: Alice has "Customer Success" skills, prefers mornings, available
│  🔄 Override: Choose different person
│  👤 Alice's load: 35hrs this week (team avg: 33hrs)
│
├─ [Shift] Monday, 5pm-9pm   →  Carlos (Medium confidence 72%)
│  📊 Why: Carlos has "Support" skills, available. [2 other options]
│  🔄 Options: See alternatives | Override | Suggest someone
│  ⚠️  Carlos's preference: prefers not evening shifts
│
├─ [Shift] Monday, 11pm-6am  →  [UNASSIGNED - needs human]
│  ⚠️  Why: No employee available with "Security" skills
│  🔄 Action: Manually assign or add constraint override
│
Confidence Score: 94% (this schedule meets all constraints and preferences)
Time to create: 8 seconds (vs 120 minutes manual scheduling)

[Approve Schedule] [Review Details] [Save as Draft] [Manual Mode]

CONFIDENCE SCALE EXPLANATION:
- 95%+: High confidence - AI believes this will work well
- 70-95%: Medium confidence - AI found a solution but with trade-offs
- <70%: Low confidence - AI struggled, human input recommended
- <50%: AI requesting human help directly

ERROR STATES:

Empty state (no feasible schedule):
"I can't find a schedule that meets all constraints.
Options: Remove fairness constraint | Reduce coverage to 90% | Manual mode"

Edge case (conflicting constraints):
"These constraints conflict: Bob is on leave, but needed for coverage.
Options: Unscheduled Bob's leave | Accept 80% coverage | Assign someone else"

Fairness alert:
"This schedule gives Bob 50% more hours than others. Likely to cause fairness issues.
Approve anyway? [Yes] [Let me adjust]"
```

**User Interaction Point:** Design leads user testing with 3-5 schedulers (target users). Test prototype with edge cases (low confidence, errors, fairness alerts). Confirm UX is usable and builds trust in AI. 2 hour testing session.

**Data Handoff:** UX designs flow to Step 5 (PRD Development) and engineering for implementation.

---

### Step 5: PRD Development with AI-Specific Requirements (Session 3, Day 5)
**Owner:** PM  
**Input:** Architecture (Step 1), Eval framework (Step 2), Guardrails (Step 3), UX designs (Step 4)  
**Output:** Complete PRD with performance requirements, eval criteria, monitoring plan

Run the **prd-development** skill to:
- Document full feature specification with AI-specific sections
- Include: Success metrics, evaluation criteria, guardrails, human-in-loop workflows
- Define technical requirements: Data freshness, latency, infrastructure
- Document monitoring requirements: What metrics are tracked post-launch?
- Create data governance section: What data trains the model? Who has access?
- Define retraining triggers: When does the model need retraining?
- Include security considerations: Input validation, prompt injection protection

**PRD Section: AI-Specific Requirements**
```
## AI Performance Requirements

### Success Criteria (from Evaluation Framework)
- Coverage success rate: ≥95% (all required shifts assigned)
- Fairness metric (Gini): <0.25 (schedule is equitable)
- User satisfaction: ≥4.0/5 (users trust recommendations)
- Explanation quality: >80% of suggestions have clear explanation
- Latency: <5 seconds for typical team (50 employees, 100 shifts)

### Failure Thresholds (Do NOT Ship If)
- Fairness metric >0.40 (likely discriminatory)
- Coverage success <85% (unreliable)
- Hallucination rate >5% (AI suggests non-existent employees)
- User satisfaction <3.0/5 (not providing value)

### Guardrails (MANDATORY - NO EXCEPTIONS)
1. Input validation: All employee names checked against database
2. Rule enforcement: No shift violates labor law constraints
3. Fairness checks: Flag assignments that violate fairness thresholds
4. Confidence thresholds: Low-confidence suggestions require human help
5. Override tracking: All human overrides logged for feedback loop

### Data Requirements
- Input data: Current roster, shift requirements, preferences, historical data
- Data freshness: Roster data must be <1 day old (avoid stale data)
- Training data: 2 years of historical schedules from 50+ teams
- Data access: Only authorized users can see schedule data (compliance)
- Data retention: Delete raw schedules after 12 months (privacy)

### Monitoring & Retraining
- Daily metrics: Coverage %, fairness score, override rate, user satisfaction
- Weekly review: Any metrics trending downward?
- Trigger for retraining: Override rate >30% OR fairness metric increases >0.05
- Retraining process: Re-run eval framework with new data, compare to baseline
- Rollback plan: If production metrics diverge, revert to previous model version

### Model Governance
- Model ownership: [AI Team Lead]
- Update frequency: Quarterly retraining with new customer data
- Version control: Every model update is tagged and tested
- Audit trail: All changes logged with justification
- Human review: Material model updates require PM + legal sign-off

### Security Considerations
- Prompt injection: AI system must validate all user inputs
- Data privacy: No training data from competitors or sensitive customer data
- Bias: Regular fairness audits, diverse training data
- Hallucination: Confidence scores must be calibrated, low-conf. suggestions reviewed
```

**User Interaction Point:** Engineering lead confirms PRD is technically feasible and includes all required monitoring. Compliance reviews data governance and security sections. PM and Data Science team confirm success metrics are measureable and realistic. 1.5 hour working session.

**Data Handoff:** PRD is locked and becomes source of truth for engineering implementation and compliance review (Step 6).

---

### Step 6: AI Compliance & Risk Review (Session 3, Day 5)
**Owner:** Compliance + Legal + Data Privacy + AI Safety Lead  
**Input:** PRD (Step 5), Architecture (Step 1), Guardrails (Step 3)  
**Output:** Compliance sign-off with risk mitigation plan, legal approval for launch

Run the **compliance-review** skill (AI-specific version) to:
- Assess bias risk: Does AI perpetuate or amplify existing biases?
- Review transparency: Can users understand how AI makes decisions?
- Evaluate hallucination risk: How likely is AI to give false information?
- Check data governance: Is training data appropriate and non-discriminatory?
- Assess human override: Is there adequate human control?
- Review explainability: Can the company explain AI decisions to users/regulators?
- Evaluate legal exposure: Does feature violate any regulations (GDPR, employment law)?
- Create monitoring plan: How will we detect problems post-launch?

**Compliance Review Template:**
```
AI FEATURE COMPLIANCE REVIEW
Feature: Shift Scheduling Assistant

BIAS & FAIRNESS RISK:

Risk: AI could discriminate based on protected attributes (gender, race, etc.)
Mitigation 1: Fairness metric (Gini coefficient) tracked daily
Mitigation 2: Training data includes diverse teams (gender, race, age)
Mitigation 3: Guardrail: Flag if fairness metric exceeds threshold
Mitigation 4: Quarterly audit by external fairness expert
Risk Rating: MEDIUM → Controls in place

Risk: AI could disadvantage workers with disabilities
Mitigation: Explicitly include disability accommodations in training data
Mitigation: Guardrail: Never override explicit accessibility requirements
Risk Rating: LOW → Strong controls

TRANSPARENCY & EXPLAINABILITY RISK:

Risk: Users don't understand why AI made decisions
Mitigation: Every recommendation includes explanation ("Why this assignment?")
Mitigation: Confidence score shown (users know if AI is uncertain)
Mitigation: Override mechanism with feedback loop (users can correct AI)
Risk Rating: LOW → Explainability built-in

Risk: Company can't explain AI decisions to regulators/users
Mitigation: Model architecture documented and interpretable (not black-box)
Mitigation: Decision trees and rules available for audit
Mitigation: Audit trail of all model decisions (with explanations)
Risk Rating: LOW → Auditable system

HALLUCINATION & ACCURACY RISK:

Risk: AI suggests employees who don't exist (hallucination)
Mitigation: Guardrail 1 - Input validation against employee database
Mitigation: Confidence score only high if all references valid
Risk Rating: MEDIUM → Mitigated by technical guardrail

HUMAN CONTROL & OVERRIDE RISK:

Risk: AI makes decisions without human review
Mitigation: All AI recommendations require human approval before applying
Mitigation: Low-confidence suggestions always flagged for human help
Mitigation: Easy override mechanism (one-click change)
Risk Rating: LOW → Human-in-the-loop enforced

DATA GOVERNANCE & PRIVACY RISK:

Risk: Training data contains sensitive information
Mitigation: Training data is anonymized (no names, no personal details)
Mitigation: Only historical schedules used (no email, phone, addresses)
Mitigation: Data deleted after 12 months (GDPR compliance)
Risk Rating: LOW → Privacy controls in place

EMPLOYMENT LAW RISK:

Risk: AI could violate labor laws (hours, breaks, discrimination)
Mitigation: Guardrail 2 - Rules enforcement against labor law constraints
Mitigation: No AI decision can override labor law requirements
Risk Rating: LOW → Hardcoded compliance

LEGAL SIGN-OFF:

☑ Feature can be legally launched
☐ Conditional approval: [List conditions]
☐ Cannot launch: [List blockers]

Conditions: None
Risks identified and mitigated: Yes
Monitoring plan in place: Yes
Ready for launch: YES, approved [Date]

Signed: [Compliance Officer], [Legal], [Data Privacy Officer]
```

**User Interaction Point:** Compliance lead confirms risk assessment is comprehensive. Legal reviews any employment law implications. Data Privacy officer signs off on data governance. AI Safety lead confirms guardrails are adequate. If any major risks are identified, they're added to mitigation plan. 1 hour working session.

**Data Handoff:** Compliance sign-off is required before launch (Step 7). Any mitigation items become requirements in PRD.

**CRITICAL GATE:** If compliance identifies unmitigatable risks (e.g., fairness metric can't be fixed), feature does NOT proceed to launch. Return to Step 1 or defer.

---

### Step 7: Launch & Ongoing Monitoring (Session 4+, Week 2+)
**Owner:** PM + Engineering + Data Science  
**Input:** PRD (Step 5), Compliance approval (Step 6)  
**Output:** Feature shipped with monitoring active, baseline metrics established

Execute **launch** (adapted for AI):
- **Pre-launch:** Verify all monitoring is in place, guardrails are active, rollback procedure tested
- **Beta phase (Week 1-2):** Deploy to 10% of customers with heavy monitoring
- **Monitor metrics:** Daily review of coverage, fairness, confidence, override rate, user satisfaction
- **Feedback loop:** Collect user corrections and feed to AI model
- **Iterative improvement:** Identify patterns in user overrides and retrain model
- **GA phase (Week 3+):** Roll out to 100% with ongoing monitoring
- **Quarterly retraining:** Review performance quarterly, retrain if needed

**AI Feature Launch Checklist:**
```
PRE-LAUNCH (Day -1):
☑ All guardrails tested and active
☑ Monitoring dashboards live (coverage, fairness, confidence, overrides)
☑ Rollback procedure tested (can we quickly disable AI?)
☑ Compliance sign-off obtained
☑ User documentation complete
☑ Support team trained on common issues

BETA PHASE (Weeks 1-2, 10% of customers):
☑ Daily monitoring review (any issues?)
☑ Collect user feedback via survey
☑ Track: Coverage %, fairness metric, override rate, satisfaction
☑ Go/no-go decision: Hit targets? Continue to GA? Pause for improvement?

TARGETS FOR GA LAUNCH:
☑ Coverage ≥95% (all required shifts assigned)
☑ Fairness metric <0.25 (equitable distribution)
☑ Override rate <20% (users trust AI)
☑ User satisfaction ≥4.0/5
☑ No fairness complaints or bias reports

GA PHASE (Weeks 3+, 100% of customers):
☑ Continue daily monitoring
☑ Weekly metrics review
☑ Monthly user feedback survey
☑ Quarterly retraining decision
☑ Escalation: If any metric drifts, investigate and retrain

ONGOING MONITORING METRICS:

Daily:
- Coverage success rate
- Fairness metric (Gini coefficient)
- User satisfaction (moving average)
- Override rate
- Model confidence (average & distribution)
- Critical errors (hallucinations, crashes)

Weekly:
- Segment analysis: Which customer types see best/worst performance?
- Feedback patterns: What are users overriding most often?
- User satisfaction trend
- Support tickets related to AI

Monthly:
- Cohort analysis: New customers vs. experienced?
- A/B test results: AI vs. manual scheduling (if available)
- Bias audit: Are specific customer segments seeing unfair schedules?
- Model performance degradation (drift detection)

Quarterly:
- Full retraining decision: Should we retrain on new data?
- Feature expansion: What improvements did users request?
- Competitive analysis: Are we ahead or behind on AI features?
- Roadmap planning: What's next for AI roadmap?
```

**User Interaction Point:** PM reviews beta metrics daily for first week, then weekly through GA. If any critical issues arise (fairness metric >0.40, coverage <85%, hallucination detected), PM convenes emergency team meeting to decide: Fix and re-launch, or rollback?

**Data Handoff:** Ongoing monitoring data informs product roadmap and quarterly planning.

---

## Data Handoff

| From Step | To Step | Data | Format | Reference |
|-----------|---------|------|--------|-----------|
| 1 | 2 + 4 + 5 | Architecture with decision logic | Architecture doc | All downstream steps reference design |
| 2 | 3 + 5 + 6 | Evaluation framework with metrics | Evaluation spec | Guardrails and PRD reference success metrics |
| 3 | 4 + 5 | Guardrails design with safety mechanisms | Guardrails doc | UX and PRD incorporate guardrails |
| 4 | 5 | UX designs with prototype | Design mockups | PRD includes UX specs |
| 5 | 6 + 7 | Complete PRD with AI-specific requirements | Markdown PRD | Compliance reviews and engineering both use PRD |
| 6 | 7 | Compliance sign-off with risk mitigation | Sign-off doc | Required before launch |
| 7 | Ongoing | Launch metrics and monitoring data | Dashboard + reports | Feeds into product roadmap |

**Single Source of Truth:** Create "AI Feature Tracker" with links to all docs: Architecture → Evaluation → Guardrails → UX → PRD → Compliance → Launch. Updates to any doc are tracked.

---

## Parallelization Opportunities

### Sequential Critical Path:
```
Step 1 (Architecture) ──must complete──> Step 2 (Evaluation)
                                             ↓
                                      (gates Step 5 PRD)
                                             
Parallel:  Step 3 (Guardrails) ──────────┐
           Step 4 (UX Design) ───────────┤──> Step 5 (PRD)
           [both use Step 1 outputs] ────┘         ↓
                                             Step 6 (Compliance)
                                                    ↓
                                             Step 7 (Launch)
```

### Key Gates:
1. **Must complete Step 2** before finalizing Step 5 (PRD success metrics come from eval)
2. **Must complete Step 6** before launching Step 7 (compliance approval required)
3. **Steps 3-4 can run in parallel** with Step 2 (both use Step 1 architecture)

### Example Timeline:
```
Day 1:    [Step 1: Architecture Design] ──────────────────
Day 2:    [Step 2: Evaluation Framework] + [Step 3: Guardrails] (parallel)
Day 3:    [Step 2 continued] + [Step 4: UX Design] (parallel)
Day 4:    [Step 5: PRD Development] + [Step 3-4 refinements]
Day 5:    [Step 6: Compliance Review]
Week 2:   [Step 7: Launch & Monitoring]
```

---

## User Interaction Points

1. **Architecture Review (Day 1-2):** PM + AI Architect confirm architecture is sound. ~1 hour sync.

2. **Evaluation Framework Review (Day 2-3):** PM + Data Science define success metrics and test cases. ~1.5 hours.

3. **Guardrails & UX Sync (Day 3-4):** Design + Safety confirm guardrails are comprehensive. ~1 hour.

4. **PRD Finalization (Day 4-5):** Engineering + PM + Data Science confirm technical feasibility. ~1.5 hours.

5. **Compliance Sign-Off (Day 5):** Legal + Compliance + AI Safety review and approve. ~1 hour.

6. **Launch Readiness (Week 2, Day 1):** Final pre-launch checklist. All systems go? ~30 min.

7. **Beta Monitoring (Week 2-3):** Daily sync if issues, weekly otherwise. ~30 min daily, ~1 hour weekly.

8. **GA Decision (Week 3):** PM reviews beta metrics and approves GA launch. ~1 hour.

9. **Quarterly Review (Month 1, 3, 6):** Full team reviews performance and retraining decision. ~2 hours quarterly.

**Total PM Time Investment:** 8-10 hours of synchronous time across 4-5 sessions + 4-6 hours asynchronous work (PRD writing, review prep). Ongoing monitoring is ~1-2 hours per week initially, then ~2-4 hours monthly.

---

## Estimated Timeline

| Phase | Duration | Critical Notes |
|-------|----------|-----------------|
| Step 1: Architecture | 1-2 days | Must complete before Step 2 |
| Step 2: Evaluation | 2-3 days | Gates PRD finalization |
| Steps 3-4: Parallel | 2-3 days | Run alongside Step 2 |
| Step 5: PRD | 1-2 days | Waits for Steps 2, 3, 4 |
| Step 6: Compliance | 1 day | Serial, after PRD |
| Step 7: Launch & Beta | 2-3 weeks | Beta phase, then GA decision |
| **Total Elapsed Time** | **1 week + launch** | Architecture through PRD ready in 1 week; launch 2+ weeks later |

---

## Final Output Summary

By the end of this workflow, you have:

1. **AI Architecture Document**
   - Clear design of what AI will do, inputs/outputs, decision logic
   - Identified failure modes and feasibility assessment
   - Design decisions documented for future reference

2. **Evaluation Framework**
   - Success metrics (coverage, fairness, user satisfaction, etc.)
   - Test cases covering happy paths, edge cases, and adversarial scenarios
   - Baseline measurements and human performance comparisons
   - Failure thresholds (do NOT ship if metrics not met)

3. **Guardrails & Safety Design**
   - Rule-based validation to prevent AI failures
   - Confidence thresholds and human-in-loop workflows
   - Explainability design showing why AI made decisions
   - Override and feedback mechanisms for continuous improvement

4. **AI-Native UX Design**
   - Prototype showing how users see AI recommendations
   - Confidence indicators and explanation displays
   - Error handling for edge cases and low-confidence states
   - User testing results confirming usability

5. **Complete AI-Specific PRD**
   - Feature specification with all technical requirements
   - Performance requirements (latency, accuracy, fairness)
   - Monitoring and retraining triggers
   - Data governance and security considerations
   - Guardrails as mandatory requirements

6. **Compliance & Legal Sign-Off**
   - Bias and fairness risk assessment with mitigations
   - Transparency and explainability review
   - Employment law and regulatory compliance check
   - Ongoing monitoring plan for post-launch risk management
   - Legal approval to launch

7. **Successful Launch**
   - Feature deployed to beta users with heavy monitoring
   - Baseline metrics established (coverage, fairness, satisfaction)
   - Guardrails active and preventing failures
   - User feedback loop collecting corrections for model improvement
   - Rollback procedure tested and ready
   - Go/no-go decision made based on beta performance

8. **Ongoing AI Governance**
   - Daily/weekly/monthly monitoring dashboards active
   - Retraining triggers defined (override rate, fairness drift)
   - Quarterly review cycle for model updates
   - Continuous improvement based on user feedback and performance data

**Handoff Status:** AI feature is live, safe (guardrails in place), compliant (reviewed), and monitored (metrics tracked). Product team transitions to ongoing optimization and quarterly retraining. Feature enters steady-state operations phase.

---

## When to Use This Workflow

✓ **AI/ML features** that make decisions affecting users  
✓ **Features with fairness/bias concerns** (hiring, scheduling, lending, etc.)  
✓ **When model governance is required** (regulated industries, high stakes)  
✓ **When you need explainability** (users must understand AI reasoning)  
✓ **Features requiring ongoing monitoring** (drift detection, retraining)  
✓ **When compliance review is mandatory** (AI-specific regulations)  

---

## When NOT to Use This Workflow

✗ **Simple ML features** (basic scoring/ranking without user-facing impact) → Use lighter approach, skip guardrails/compliance if low-risk  
✗ **Third-party AI** (using OpenAI API directly, no custom model) → Simplified workflow; still need UX design and monitoring  
✗ **Fully research-stage** projects → Do research first, then come back to this workflow  
✗ **When you lack ML/AI expertise** → Partner with AI team or defer until you have capacity  

**Alternative:** For lighter AI features, use "Express AI" workflow (skip eval framework deep-dive, use simplified guardrails, assume third-party vendor handles compliance).
