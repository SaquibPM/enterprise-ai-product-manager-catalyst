---
name: human-in-loop
description: >
  Master human-in-the-loop (HITL) AI system design for enterprise. Learn to architect approval workflows, confidence thresholds, and escalation chains that keep humans in control while capturing AI efficiency. Design graceful degradation for edge cases. Implement SLA-driven review queues that actually work. Build frameworks for regulated industries—healthcare, finance, procurement. Prevent automation theater. Calibrate thresholds using real accuracy data. Route approvals based on expertise and risk. Avoid the pitfalls that have sunk 40% of enterprise AI deployments.
---

## Purpose

Human-in-the-loop (HITL) systems are the backbone of enterprise AI. They're where you capture AI's efficiency while maintaining human oversight, compliance, and judgment. But most PMs ship HITL systems that fail catastrophically: review queues back up, approvers ignore alerts, AI learns nothing from human corrections, edge cases cause production incidents. This skill teaches you to design HITL systems that actually work—with confidence thresholds calibrated to your business context, approval workflows matched to risk levels and regulatory constraints, escalation chains with real SLAs, and graceful degradation strategies for when things break. You'll learn from failures that cost enterprises millions: thresholds set too low (errors slip through compliance reviews), too high (human bottleneck kills the project), no feedback loop from corrections (model never improves), no SLAs on review queues (approvals sit for days). This skill is built on shipped systems in healthcare, financial services, and procurement—contexts where getting HITL wrong creates audit violations and operational chaos.

---

## Key Concepts

### The HITL Spectrum

Not all AI needs humans in the same way. There are four modes, each with different PM implications, costs, and risk profiles.

**1. Human-in-the-loop (HITL)** — AI generates output, human approves before execution
- AI is a draft tool; human owns the decision
- Every action waits for human review
- Slowest, safest, most certain of human oversight
- Cost: high latency, requires review team, doesn't scale with volume
- Use when: legal liability is high, compliance is strict, errors are irreversible, dollar amounts justify review cost

**2. Human-on-the-loop (HOTL)** — AI acts autonomously, human monitors and can intervene
- AI executes immediately; human watches for anomalies
- Humans are reactive, not proactive
- Faster than HITL, but requires active monitoring
- Cost: requires dashboards, alerting, incident response infrastructure
- Use when: most actions are safe, errors are reversible, volume is high, false positives would be expensive

**3. Human-over-the-loop (HOUL)** — human sets policies and constraints, AI operates within them
- AI is bounded by predefined rules
- Humans design the boundaries, not every transaction
- Fastest, scales well, but requires upfront policy design
- Cost: policy maintenance, edge case handling, threshold tuning
- Use when: actions are routine, patterns are clear, you can articulate decision rules, regulatory framework is stable

**4. Human-out-of-the-loop (HOOTL)** — fully autonomous
- No human involvement except in training
- Fastest, cheapest, highest risk
- Use when: errors are truly inconsequential (e.g., internal analytics), you have extensive historical validation, no regulation forbids it

### Mode Selection Matrix

| Risk Level | Low Volume (<100/day) | Medium Volume (100-1K/day) | High Volume (>1K/day) | Regulated Context |
|---|---|---|---|---|
| **Low Risk** (reversible, <$1K) | HOTL | HOUL | HOUL | HOUL with audit trail |
| **Medium Risk** (semi-reversible, $1K-$100K) | HITL | HOTL | HOUL | HOTL with policy guardrails |
| **High Risk** (irreversible, >$100K, PHI, spend commitment) | HITL | HITL | HITL + escalation | HITL mandatory |
| **Compliance-sensitive** (SOX, HIPAA, audit-critical) | HITL | HITL + expedited | HITL + intelligent routing | HITL + audit log |

---

### Confidence Threshold Design

This is where most HITL systems fail. Confidence scores from ML models are useless to PMs. You need to translate them into business decisions.

**Redefine Confidence in Business Terms**

Don't use model logits or softmax probabilities. Instead:
- **Confidence = P(action is correct | data observed + model's historical accuracy on similar cases)**
- Measure against ground truth: did the human approve it? Did it work without issue?
- Calibrate on your actual data, not benchmark datasets

Example: Your PO routing agent says "route to IT department" with 94% confidence. What does that mean?
- It means: on similar POs from your training data, when the model said "IT," 94% of the time approvers actually agreed it was an IT purchase
- On 6% of cases, it was wrong (sent to wrong department, took 2 days to reroute)
- This is what confidence *should* mean

**Three-Tier Routing Logic**

```pseudocode
if confidence >= high_threshold (e.g., 95%):
    AUTO_APPROVE()  // skip review queue, execute immediately
    log_decision("auto", confidence)
    
elif confidence >= medium_threshold (e.g., 80%):
    EXPEDITED_REVIEW()  // route to review queue, SLA 4 hours
    assign_priority("normal")
    
else:  // confidence < 80%
    FULL_REVIEW()  // route to escalation, SLA 24 hours, might need specialist
    assign_priority("high")
    flag_for_manual_classification()
```

**Threshold Calibration Methodology**

Start conservative. Move slowly. Measure constantly.

1. **Week 1-2: All to FULL_REVIEW**
   - Collect ground truth on 100% of decisions
   - What did humans actually approve?
   - Build your calibration dataset

2. **Week 3-4: Set initial thresholds from data**
   - Use your calibration set to define thresholds
   - Aim for high_threshold such that accuracy >= 99% (false positive rate ≤ 1%)
   - Aim for medium_threshold such that accuracy >= 95% (you can tolerate 5% error for expedited path)
   - This means ~70% of items auto-approve, ~20% expedited, ~10% full review

3. **Week 5-8: Monitor in production**
   - Track actual approval rates at each tier
   - Measure: when auto-approved, did anything go wrong? (audit, rework, complaint)
   - Measure: when expedited, did approver always agree with AI? (false positive rate)
   - If false positive rate > 2% on auto tier → lower threshold
   - If false positive rate < 0.5% and review queue is deep → raise threshold

4. **Quarterly: Retune**
   - New data patterns emerge
   - Seasonal variation (budget cycles, hiring freezes)
   - Policy changes (new compliance rules, vendor restrictions)
   - Retrain model, recalibrate thresholds

**The Cost Tradeoff Matrix**

| Threshold Action | False Positive Cost | False Negative Cost | Right Call When |
|---|---|---|---|
| Set threshold too low (let errors through) | Compliance violations, audit issues, rework | Bottleneck avoided, fast | Never. Prioritize false positives in regulated contexts |
| Set threshold too high (unnecessary reviews) | Reviewer time wasted, queue backs up, SLA breach | Safety gained | Early stage, low confidence in model, high liability context |
| Set threshold in middle (balanced) | Occasional errors in expedited path | Some queue depth, slower auto rate | Mature system with calibrated model |

**Context-Specific Thresholds**

Your single confidence score won't apply universally. Adjust by:

```pseudocode
base_confidence = model_output_score

// Adjust by dollar amount
if amount > $500K:
    confidence = confidence * 0.85  // stricter bar for high-value decisions
elif amount < $10K:
    confidence = confidence * 1.10  // relax for low-value

// Adjust by customer tier
if customer == "tier_1_enterprise":
    confidence = confidence * 0.90  // more conservative for important customers
elif customer == "small_business":
    confidence = confidence * 1.05  // slightly relax for lower-risk customers

// Adjust by regulatory context
if contains_pii or in_regulated_industry:
    confidence = confidence * 0.80  // much stricter for sensitive data
    
// Adjust by time of day (reviewer availability)
if queue_depth > 50:  // queue backlog
    confidence = confidence * 0.95  // raise bar to reduce incoming volume
```

---

### Approval Workflow Patterns

Once you've routed to review, how does the review actually happen?

**Pattern 1: Single-Approver Queue**

One person (or team working in serial) reviews all flagged items.

```
New item → Queue → [Single Reviewer] → Approved/Rejected → Action
```

- **When to use:** Low volume (<20/day), very specialized expertise, high-risk decisions
- **SLA design:** 4 hours for expedited, 24 hours for full review (measured from queue entry to decision)
- **Queue management:** Depth monitor. If queue > 10 items, escalate to manager
- **Failure mode:** Approver becomes bottleneck; single point of failure

**Pattern 2: Round-Robin Distribution**

Distribute work evenly across a team of reviewers.

```
New item → Queue → [Reviewer A, B, C, D assigned round-robin] → Approved/Rejected → Action
```

- **When to use:** Medium volume (50-200/day), decision requires generic judgment (not specialty expertise)
- **SLA design:** 2 hours for expedited (any reviewer can handle), 6 hours for full review
- **Queue management:** Auto-assign to least-loaded reviewer. If all busy, trigger escalation
- **Failure mode:** Inconsistent decisions (Reviewer A approves what B rejects); no expertise-to-item matching

**Pattern 3: Expertise-Based Routing**

Route to the person with the right subject-matter expertise.

```
Item → [Classifier: "Is this IT? Finance? HR?"] → Route to IT Budget Owner / CFO / HR Manager → Approved/Rejected → Action
```

- **When to use:** Items require domain expertise; specialists exist on team
- **SLA design:** 4 hours for expedited (specialists check queue regularly), 24 hours for full review
- **Queue management:** Each specialist has own queue. Set per-specialist SLA targets; if one queue backs up, redistribute items to second-choice reviewer
- **Failure mode:** One specialist becomes bottleneck; items stuck if specialist is unavailable

**Pattern 4: Escalation Chain**

Route to level 1, if not reviewed in X hours, escalate to level 2, then level 3.

```
Item → [Manager Review, 4-hour SLA]
  → If not approved in 4 hours: escalate to [Director Review]
  → If not approved in 2 more hours: escalate to [VP, immediate]
  → If still not approved: auto-reject with notification
```

- **When to use:** High-risk items (>$100K), regulatory deadlines, compliance-critical decisions
- **SLA design:** Each level gets progressively tighter SLAs and higher authority
- **Queue management:** Must have automated escalation logic with override capability
- **Failure mode:** Too aggressive escalation (bother senior staff too often); too lenient (items stall at level 1)

---

### Graceful Degradation Strategies

Production breaks. Models fail. Queues back up. You need fallbacks.

**Scenario 1: AI Confidence Too Low to Act**

Your classifier is uncertain. Confidence is 62%, below even the full-review threshold of 80%.

**Fallback:** 
```
if confidence < 80%:
    ROUTE_TO_SPECIALIST()  // someone trained on edge cases
    flag_with_evidence()   // surface what confused the model
    escalate_if_queue_deep() // if specialist queue > 10 items, escalate to supervisor
    timeout_24_hours()     // human decides or it times out
```

**Why this works:** You've explicitly delegated to someone who handles ambiguity. You haven't frozen the system.

**Scenario 2: AI Model is Down or Slow**

The model serving endpoint is returning errors. Latency exceeds SLA.

**Fallback:**
```
if model_latency > 5_seconds or model_error:
    BYPASS_AI_CLASSIFICATION()
    ROUTE_TO_FULL_REVIEW()  // all items go to human queue
    SEND_ALERT("AI pipeline degraded")
    PERSIST_ITEMS_IN_DB()   // don't lose anything
    RETRY_WITH_EXPONENTIAL_BACKOFF()  // retry model every 30 seconds
```

**Why this works:** You've chosen safety (full review) over speed. You're not queueing items indefinitely; you're flushing them to human review immediately.

**Scenario 3: Review Queue is Overloaded**

Queue depth is 500 items; normal SLA is 4 hours. You'll miss SLAs.

**Fallback:**
```
if queue_depth > 200:
    RAISE_CONFIDENCE_THRESHOLD()  // move from 80% to 85% to reduce inflow
    SEND_ALERT_TO_REVIEWERS()     // "queue backlog; prioritize urgent items"
    AUTO_APPROVE_LOW_RISK()       // even low-confidence items <$1K auto-approve
    CALL_IN_COVERAGE()            // notify on-call reviewers to get online
    DELAY_ROUTINE_ITEMS()         // bump non-urgent items to next day
```

**Why this works:** You're fighting the queue with multiple levers. You're not freezing new items; you're reshaping what gets reviewed and when.

**Scenario 4: Edge Cases That Don't Fit Any Pattern**

Item is 50% IT purchase, 50% marketing. Doesn't fit your categories cleanly.

**Fallback:**
```
if model_uncertain_across_categories:
    ROUTE_TO_SENIOR_REVIEWER()  // someone who understands cross-functional decisions
    SURFACE_AMBIGUITY()          // highlight the conflicting signals
    COLLECT_FEEDBACK()           // human decides; we learn from this for next time
    CACHE_DECISION()             // future similar items → use this decision as reference
```

**Why this works:** You've elevated ambiguity explicitly. You've made it a learning opportunity, not a hidden failure.

---

### Exception Handling in Regulated Contexts

Regulations don't change your HITL design—they eliminate choices.

**Healthcare Context (HIPAA)**

When AI touches PHI (Protected Health Information):

```
if contains_PHI:
    HUMAN_REVIEW_MANDATORY = true
    AUDIT_LOG_REQUIRED = true
    ENCRYPTION_AT_REST = true
    ENCRYPTION_IN_TRANSIT = true
    DATA_RETENTION_POLICY = "30 days"
    
RULE:
    // AI can pre-screen for compliance, not override HIPAA
    if AI_flags_privacy_violation:
        ALERT_PRIVACY_OFFICER()
        ROUTE_TO_HIPAA_REVIEW()
        ESCALATE_IF_UNRESOLVED_4_HOURS()
        
    // Human always approves clinical decisions
    if clinical_decision:
        HUMAN_APPROVAL_REQUIRED = true
        AUDIT_LOG_EVERY_TOUCH()
```

**Financial Services Context (SOX Compliance)**

When AI makes spend recommendations or financial decisions:

```
if spend_commit > $0:
    SPEND_AUTHORIZATION_REQUIRED = true
    AUDIT_TRAIL_REQUIRED = true
    APPROVER_MUST_BE_AUTHORIZED()
    
if spend_commit > $50K:
    DUAL_APPROVAL_REQUIRED = true  // two independent humans
    ESCALATION_TIMER = 4_hours
    
RULE:
    // No AI can approve spend; AI can only route
    AI_DECISION = "recommend_approver_and_path"
    HUMAN_DECISION = "approve_or_reject_spend"
    AUDIT_BOTH()
```

**Procurement Context (SOX Audit Trail)**

When AI routes purchase orders:

```
RULE:
    // Every PO routing decision must be auditable
    LOG("timestamp", "po_id", "ai_confidence", "ai_recommendation", "human_decision", "human_reasoning", "approver_id")
    
    // AI can't override policy
    if policy_violation_detected:
        BLOCK_AUTO_APPROVAL()
        ROUTE_TO_COMPLIANCE_REVIEW()
        ESCALATE_TO_PROCUREMENT_MANAGER()
    
    // High-value POs need human eyes
    if amount > 100K:
        REQUIRE_HUMAN_REVIEW = true
        ESCALATION_TIMER = 1_hour
        BUDGET_OWNER_APPROVAL = true
```

---

## Application: 8-Step Process for Designing HITL

Follow this process every time you're architecting HITL for a new AI feature.

**Step 1: Define the Business Action and Failure Modes**

Question: What decision is the AI making? What happens if it's wrong?

Deliverable: One-paragraph description of the decision and 3-5 failure scenarios with business impact.

Example for PO routing agent:
- Decision: "Route this purchase order to the correct approver based on category, amount, and policy"
- Failure mode 1: Route to wrong approver (marketing PO to IT budget owner) → 2-day rework, SLA miss
- Failure mode 2: Route to approver who's on vacation → PO stalls for week, vendor gets angry
- Failure mode 3: Route to someone without authority for amount → compliance violation, audit finding
- Failure mode 4: Miss a compliance violation (restricted vendor, policy violation) → audit failure

**Step 2: Assess Regulatory and Compliance Constraints**

Question: Are there laws, regulations, or audit requirements that mandate human review?

Deliverable: List of constraints; any that say "human approval required" move you automatically to HITL mode.

Example:
- SOX Compliance: "All spend commitments must be approved by authorized approver with audit trail"
- Company Policy: "POs over $100K require dual approval"
- Vendor Agreement: "Strategic vendors require VP-level approval"

**Step 3: Estimate Volume and Latency Requirements**

Question: How many decisions per day? How fast must decisions be made?

Deliverable: Volume projection (daily, weekly, peak); required latency (seconds? minutes? hours?).

Example:
- Volume: 500 POs per day on average, 1,200 on Mondays, 50 on Fridays
- Latency: Standard POs need approval within 4 hours (vendor SLA), Urgent POs within 2 hours, Crisis POs within 30 minutes

**Step 4: Choose HITL Mode**

Question: Based on risk, compliance, and volume, what mode fits?

Deliverable: HITL mode selected with justification.

Example decision: "HUMAN-ON-THE-LOOP for routine POs (<$50K, standard categories). HUMAN-IN-THE-LOOP for high-value POs (>$50K) and non-standard categories. This balances SOX compliance requirements with volume."

**Step 5: Define Confidence Threshold**

Question: At what model confidence does this action skip human review? How will you calibrate?

Deliverable: Threshold values (auto-approve, expedited, full review); calibration plan.

Example:
```
HIGH_THRESHOLD = 95% confidence
MEDIUM_THRESHOLD = 85% confidence

Calibration:
- Week 1-2: Route all POs to human review; collect ground truth (did approver agree with AI recommendation?)
- Week 3: Build calibration dataset; set thresholds such that:
  - 95% threshold = 99% accuracy (1 error per 100 auto-approved POs)
  - 85% threshold = 96% accuracy (4 errors per 100 expedited POs)
- Week 4+: Monitor false positive rate; adjust quarterly
```

**Step 6: Design Approval Workflow**

Question: Who approves? In what order? How do they decide?

Deliverable: Workflow diagram showing routing logic, approver roles, SLAs, escalation.

Example:
```
PO routed by category (IT, Finance, Marketing, HR, Operations)
  → IT POs go to IT Budget Owner (SLA 4 hours)
  → Finance POs go to CFO (SLA 4 hours)
  → Marketing POs go to CMO (SLA 6 hours)
  
If amount > $100K:
  → Also route to CEO for visibility
  
If unresolved after SLA:
  → Escalate to next level (Budget Owner → SVP → CEO)
```

**Step 7: Plan Graceful Degradation**

Question: What breaks? How do you handle it without losing data or SLA?

Deliverable: Fallback decision tree (if X happens, do Y).

Example:
```
If AI model offline:
  → Route all POs to full review queue (safe default)
  
If review queue > 300 items:
  → Raise confidence threshold to auto-approve more items
  → Alert managers to call in coverage
  
If approver unavailable (on vacation):
  → Route to secondary approver in same function
  → Escalate if secondary also unavailable
```

**Step 8: Define Success Metrics and Monitoring**

Question: How do you know if HITL is working? What breaks it?

Deliverable: 5-7 metrics to track with target ranges and alerting thresholds.

Example metrics:
- **Auto-approval rate:** Target 70% (±5%), alert if < 65% (threshold too high) or > 75% (too low, missing errors)
- **Expedited review SLA met:** Target 95%, alert if < 90%
- **Full review SLA met:** Target 90%, alert if < 85%
- **False positive rate (approver disagreed with AI):** Target 2%, alert if > 3%
- **Review queue depth:** Target < 50 items, alert if > 100
- **End-to-end PO approval time:** Target median 3 hours, alert if > 5 hours
- **Rework rate (PO routed incorrectly, had to be reassigned):** Target 0.5%, alert if > 1%

---

## Worked Example: Purchase Order Approval Routing Agent

### The Problem

You're building an AI agent for an enterprise procurement platform. Every day, 500+ purchase orders arrive. Each needs routing to the correct approver based on:
- **PO amount** (does it exceed approval limits?)
- **PO category** (IT, Finance, Marketing, HR, Operations?)
- **Vendor** (is this a restricted vendor, strategic partner, new vendor?)
- **Policy compliance** (does this violate spending policies?)
- **Requester level** (does a junior employee's $50K purchase need extra scrutiny?)

Manually routing takes 2-3 hours per day. The procurement team is drowning.

### Why Full Autonomy Isn't Appropriate

You might think: "Just let the AI route all POs autonomously. It's deterministic."

But here's why that fails in enterprise:

1. **SOX Compliance mandates human approval** — spend commitments must be approved by authorized personnel with audit trail. You can't automate that away.
2. **Edge cases are common** — a $45K purchase might be IT hardware (standard, fast-track approver) or a strategic software license (needs CFO + legal review). The AI will be wrong 5-10% of the time.
3. **Vendor management is political** — the CEO just negotiated a deal with Vendor X. Approvers need context. AI doesn't have it.
4. **Policy violations are hidden** — "no Slack purchases in HR budget" is a policy that requires human judgment to apply consistently.

So you need humans in the loop. The question is: how much?

### HITL Mode Selection

**Decision:**
- POs under $10K, standard categories (IT Hardware, Office Supplies), approved vendors → **HUMAN-ON-THE-LOOP** (auto-route, monitor for errors)
- POs $10K–$100K, standard categories, approved vendors → **HUMAN-IN-THE-LOOP expedited** (AI recommends, human approves in 4 hours)
- POs over $100K, non-standard categories, new vendors → **HUMAN-IN-THE-LOOP full** (AI routes to expert, human decision required, 24-hour SLA)
- Any policy violation detected → **HUMAN-IN-THE-LOOP escalation** (route to compliance, 2-hour SLA, escalate to CFO if unresolved)

**Rationale:**
- Low-value routine POs are safe to auto-route; errors are reversible and inexpensive
- Medium-value standard POs need human review but can be expedited (approver sees AI recommendation; it's usually right)
- High-value and unusual POs need expert judgment
- Policy violations need oversight immediately

### Confidence Threshold Design with Real Numbers

#### Week 1-2: Baseline

You're routing 100% of POs to full human review. Procurement team reviews and approves or rejects AI recommendation.

```
Data collected:
- AI recommends: Route to IT Budget Owner
- Human decision: Yes, IT Budget Owner was correct approver
- Confidence: 92% (model was 92% confident)

- AI recommends: Route to Marketing Manager
- Human decision: No, should have gone to CFO (this was a software license, not a marketing campaign)
- Confidence: 78% (model was 78% confident)
```

You build a dataset of 500 POs with ground truth (human decision) and model confidence.

#### Week 3: Calibrate Thresholds

```
Analysis:
- At 95% confidence threshold: 87 POs auto-approve, 0 are wrong = 100% accuracy
- At 90% confidence threshold: 150 POs auto-approve, 3 are wrong = 98% accuracy
- At 85% confidence threshold: 210 POs auto-approve, 8 are wrong = 96% accuracy
- At 80% confidence threshold: 300 POs auto-approve, 20 are wrong = 93% accuracy
- At 75% confidence threshold: 380 POs auto-approve, 40 are wrong = 89% accuracy

Decision:
- HIGH_THRESHOLD = 95% confidence (auto-approve; target 99%+ accuracy because errors are expensive to fix)
- MEDIUM_THRESHOLD = 85% confidence (expedited review; 96% accuracy acceptable because human reviews within 4 hours)
- Below 85% = FULL_REVIEW (human decides; any confidence is acceptable)
```

#### Weeks 4+: Monitor and Adjust

```
Week 4 results:
- Auto-approved rate: 70% (expected 70%, good)
- Auto-approval error rate: 0.8% (target 1%, even better)
- Expedited review SLA met: 98% (target 95%, good)
- Expedited review false positive rate: 3.2% (target < 2%, slightly high)
  → Approvers disagree with expedited recommendations more than expected
  → Action: Lower MEDIUM_THRESHOLD from 85% to 82% to catch more edge cases

Week 8 results:
- Auto-approval error rate: 0.5% (comfortable; can consider raising threshold)
- But queue depth is 150 items (target < 50)
  → This means too many going to full review
  → Action: Raise HIGH_THRESHOLD from 95% to 96% to auto-approve more low-risk items
```

### Context-Specific Threshold Adjustments

```pseudocode
base_confidence = model_output  // e.g., 91%

// Adjust by dollar amount
if amount > $500K:
    confidence -= 5  // stricter bar for mega-deals (91% → 86%)
elif amount < $5K:
    confidence += 3  // relax for trivial amounts (91% → 94%)

// Adjust by vendor type
if vendor == "new" or not in approved_vendor_list:
    confidence -= 8  // new vendors need scrutiny (91% → 83%)
elif vendor == "strategic_partner":
    confidence -= 3  // slightly more scrutiny (91% → 88%)

// Adjust by category clarity
if model_is_uncertain_across_categories:
    confidence -= 10  // edge case; needs human eyes (91% → 81%)

// Adjust by requester level
if requester == "new_employee":
    confidence -= 2  // slight extra scrutiny (91% → 89%)

// Adjust by queue depth (system load)
if queue_depth > 150:
    confidence += 2  // raise bar to reduce incoming volume (91% → 93%)

final_confidence = base_confidence
```

Example flow for a specific PO:
```
PO arrives: $45K software license, category = IT Software, requester = Senior Engineer, vendor = New Vendor XYZ

Model scores: 88% confidence → IT category

Adjustments:
- Amount $45K: -2 confidence (medium-high value) → 86%
- New vendor: -8 confidence → 78%
- Category is clear: 0 adjustment → 78%
- Requester is senior: 0 adjustment → 78%

Final confidence: 78%

Decision: 78% < 85% (MEDIUM_THRESHOLD)
→ Route to EXPEDITED_REVIEW (4-hour SLA)
→ AI flags: "High-value, new vendor, confirm with IT Budget Owner"
```

### Approval Workflow: Expertise-Based Routing

POs are routed based on category and dollar amount:

```
PO Analysis:
  ├─ Category classification (AI)
  └─ Dollar amount bucket

Routing logic:
  
IT category:
  ├─ < $10K → Auto-route to IT Coordinator (SLA: 2 hours, can reject/approve)
  ├─ $10K–$50K → Route to IT Budget Owner (SLA: 4 hours)
  └─ > $50K → Route to IT Director (SLA: 6 hours) + notify CFO

Finance category (accounting software, banking services):
  ├─ All amounts → Route to CFO (SLA: 4 hours)
  └─ > $100K → Dual approval: CFO + CEO (SLA: 2 hours)

Marketing category:
  ├─ < $20K → Route to Marketing Manager (SLA: 6 hours)
  └─ > $20K → Route to CMO (SLA: 4 hours)

HR category:
  ├─ < $50K → Route to HR Manager (SLA: 6 hours)
  └─ > $50K → Route to CHRO (SLA: 4 hours)

Operations (general, facilities, services):
  ├─ < $30K → Route to Operations Manager (SLA: 4 hours)
  └─ > $30K → Route to COO (SLA: 6 hours)

Policy violations (non-approved vendor, restricted category):
  └─ ALL → Route to Procurement Manager + CFO (SLA: 2 hours, escalate to CEO if unresolved in 2 hours)
```

### Escalation Chain with Time-Based SLAs

The review queue can back up. When it does, escalate automatically:

```
Level 1 (Assignee):
  - Time limit: 4 hours (standard) or 2 hours (urgent) or 1 hour (critical)
  - If not approved by SLA: auto-escalate to Level 2
  - Approver gets email: "PO-12345 waiting 4+ hours, needs decision"

Level 2 (Manager/Functional Lead):
  - Time limit: 2 hours (standard) or 1 hour (urgent) or 30 minutes (critical)
  - If not approved by SLA: auto-escalate to Level 3
  - Manager gets email: "PO-12345 escalated from [Assignee], needs your approval"

Level 3 (Executive):
  - Time limit: 1 hour (standard) or 30 minutes (urgent) or immediate (critical)
  - If not approved by SLA: auto-escalate to Level 4
  - VP gets email: "PO-12345 needs immediate approval, escalated 2x already"

Level 4 (CEO/CFO):
  - Time limit: 30 minutes
  - If not approved: 
    - Send alert to both CEO + CFO
    - Mark PO as "stalled"
    - Notify requester: "PO delayed; procurement team will reach out"
    - Queue for manual management

Escalation rules:
- If approver is out-of-office: skip level, escalate to next available
- If escalation threshold exceeded: send SMS + email (not just email)
- If PO value > $500K: escalate directly to CEO level (skip intermediate)
```

### Graceful Degradation: Model Failure Scenarios

**Scenario 1: AI model is offline**
```
Alert: Model serving endpoint unreachable

Fallback:
  - Route ALL incoming POs to FULL_REVIEW
  - Bypass AI completely
  - Notify procurement team: "AI router is down; all POs in manual queue"
  - Retry model connection every 30 seconds
  - Show approvers: "PO routed manually due to system outage"
  - Once model is back online, restart processing
```

**Scenario 2: AI can't classify the category**
```
Alert: Model confidence < 60% on category classification

Fallback:
  - Route to Procurement Manager (expert on unusual cases)
  - Flag as: "Ambiguous PO; AI uncertain between [Option A] and [Option B]"
  - Provide approver with evidence: "AI was 58% confident in IT, 42% in Operations"
  - If Procurement Manager decides: feed back to model as training data
  - Once resolved, apply same decision to future similar POs
```

**Scenario 3: Review queue backs up**
```
Alert: Queue depth > 200 items (should be ~50)

Triggers:
  - Multiple approvers are out (conference, vacation)
  - System latency slowed down processing
  - Volume spike (end-of-quarter spending)

Fallback:
  - Raise confidence thresholds (auto-approve more items to reduce queue inflow)
  - Send alert to managers: "Procurement queue backing up; can you cover?"
  - Auto-approve low-risk items even if confidence is lower
  - Delay non-urgent POs to next day (mark as "deferred"; requester notified)
  - Notify executives: "Approval SLAs may be missed"
```

**Scenario 4: Approver is on vacation**
```
Alert: Assigned approver marked out-of-office in Outlook

Fallback:
  - Detect out-of-office status (check calendar)
  - Immediately escalate to backup approver (defined in Workday)
  - If backup also out: escalate to manager
  - Notify requester: "Your approval routed to [Backup Approver], SLA still 4 hours"
  - Don't let PO sit waiting for someone who isn't there
```

### Success Metrics and Monitoring

Track these metrics daily:

| Metric | Target | Yellow Alert | Red Alert |
|--------|--------|--------------|-----------|
| **Auto-approval rate** | 70% | <65% or >75% | <60% or >80% |
| **Expedited review rate** | 20% | <15% or >25% | <10% or >30% |
| **Full review rate** | 10% | <8% or >12% | <5% or >15% |
| **Auto-approval error rate** | <1% | 1-2% | >2% |
| **Expedited false positive** | <2% | 2-3% | >3% |
| **Review queue depth** | <50 items | 50-100 | >100 |
| **Standard SLA met (4 hrs)** | 95% | 90-95% | <90% |
| **Escalated SLA met (2 hrs)** | 93% | 88-93% | <88% |
| **End-to-end cycle time** | 3 hrs median | 4-5 hrs | >5 hrs |
| **Rework rate** | <0.5% | 0.5-1% | >1% |

Dashboard setup:
```
Real-time monitoring:
- Queue depth (how many waiting right now?)
- SLA breach countdown (which items are about to miss SLA?)
- Approver utilization (who's overloaded?)
- False positive trend (is accuracy degrading?)

Daily rollup:
- % of POs auto-approved
- % of POs requiring escalation
- Biggest slowdowns (what department is the bottleneck?)
- Biggest risks (what categories have highest error rate?)

Weekly review:
- Model accuracy by category (IT vs. Finance vs. Marketing)
- Approver consistency (do different approvers make different decisions?)
- New edge cases (what patterns did we see that AI wasn't trained on?)
- Threshold calibration (should we adjust thresholds?)
```

### Numbers in Production (6-Month Track Record)

Month 1 (rollout):
- Auto-approval rate: 65% (conservative thresholds)
- Expedited: 25%
- Full review: 10%
- Queue depth: 40 items (well-managed)
- SLA met: 98% (smooth rollout)
- False positive rate: 0.6% (better than expected)

Month 2-3 (optimization):
- Auto-approval rate: 72% (raised thresholds as accuracy validated)
- Expedited: 18%
- Full review: 10%
- Queue depth: 35 items (improved)
- SLA met: 96% (slight dip due to volume increase)
- Rework rate: 0.4% (learning what works)

Month 4-6 (steady state):
- Auto-approval rate: 71% (stable)
- Expedited: 19%
- Full review: 10%
- Queue depth: 42 items (consistent)
- SLA met: 97% (reliable)
- Approver satisfaction: "Much faster than manual routing"
- Business impact: Saved procurement team 15 hours/week; POs approved 2 days faster on average

---

## Common Pitfalls (And How to Avoid Them)

**Pitfall 1: Automation Theater**
- **What it looks like:** "Our AI routes all POs!" But then the human approver literally clicks the AI recommendation 99% of the time. The human hasn't thought in weeks.
- **What happens:** One of two things. (1) Someone sleepy approves a policy violation and you get an audit finding. (2) Approver quits; you realize they were doing invisible work you didn't measure. Confidence drops. The whole thing collapses.
- **How to avoid it:** Measure what approvers actually do. Track: "What % of AI recommendations does this approver change?" If > 10%, something's wrong (AI is unreliable or human is unnecessary). If < 1%, the human is rubber-stamping; they'll miss real errors eventually. Target 3-7% disagreement rate. This signals humans are engaged but mostly trusting AI.

**Pitfall 2: Threshold Set Too Low (Errors Slip Through)**
- **What it looks like:** You want to maximize auto-approvals, so you set the threshold at 75% confidence. 85% of POs auto-approve. Looks great in demos.
- **What happens:** Errors creep in. After 3 months, audit finds a vendor that should have been flagged but wasn't. Then another. Compliance team gets involved. Thresholds get slammed back down to 99%. Now almost nothing auto-approves. The system is "safe" but useless.
- **How to avoid it:** Start conservative (95% threshold = almost everything needs review). Measure actual accuracy in production (not in tests). Only relax thresholds when you have 4+ weeks of clean data showing <1% error rate. Retune quarterly; never relax more than once per quarter.

**Pitfall 3: Threshold Set Too High (Queue Bottleneck)**
- **What it looks like:** "We want to be safe," so threshold is 98%. Almost everything goes to expedited or full review. Queue builds. Approvers are drowning.
- **What happens:** Review SLAs start missing. POs sit for 3+ days. Business complains. You realize the AI can't help if approvers are underwater. You're overconfident in human availability.
- **How to avoid it:** Balance false positives (unnecessary human review) against false negatives (missed errors). Measure queue depth; if > 75 items, you're asking humans to do too much. Either raise threshold (let AI handle more), hire more approvers, or reduce volume. Don't pretend humans can review everything.

**Pitfall 4: No SLA on Review Queue**
- **What it looks like:** "Items stay in the queue until someone reviews them." No time target. No escalation.
- **What happens:** Items sit for 5+ days. Requester follows up repeatedly. Procurement team is blamed. Vendor relationship is damaged. Quietly, the system becomes unusable.
- **How to avoid it:** Define SLA per tier. Example: 4 hours for expedited, 24 hours for full review. Measure actual time in queue. Alert when approaching SLA. Auto-escalate when SLA breached. Without SLAs, you have no way to manage queue; it'll grow unbounded.

**Pitfall 5: Ignoring Reviewer Fatigue and Alert Blindness**
- **What it looks like:** You send 500 alerts per day to approvers. "PO-1234 needs decision!" "PO-5678 needs decision!" Most are low-importance.
- **What happens:** Approvers tune out. They scan alerts without reading. One day, a critical PO arrives with the same alert format as the 500 routine ones. It's missed. The human's brain has habituated to noise.
- **How to avoid it:** Prioritize alerts ruthlessly. Send urgent only (escalations, policy violations, high-value). For routine expedited approvals, use passive queue (approver checks when they have time) not alerts. If you're sending > 100 alerts per day, you're doing it wrong. Test alert fatigue by sampling: "Do approvers notice when we change alert format?" If they don't, you've lost attention.

**Pitfall 6: No Feedback Loop from Human Corrections Back to the Model**
- **What it looks like:** AI recommends "Route to IT." Human changes it to "Route to Finance." Decision is logged in a database. But nobody feeds it back to the model.
- **What happens:** The model never learns from its mistakes. Six months later, it's still making the same error 5% of the time. You keep raising thresholds to avoid it. The system becomes less helpful, not more. You blame the model when really you failed to close the loop.
- **How to avoid it:** Every time a human overrides an AI decision, capture it. Feed it back quarterly: "Here are 200 cases where AI was wrong. Retrain model on this data." Include in model training: domain knowledge from approvers (e.g., "this is a strategic vendor, treat differently"). After each retraining cycle, remeasure accuracy. You should see 10-20% improvement in error rate per cycle, initially.

---

## References

- Amershi, S., et al. (2019). "Guidelines for Human-AI Interaction." ACM CHI Conference. [Framework for HITL design principles]
- Bansal, G., et al. (2021). "Does the Whole Exceed its Parts? The Effect of AI Explanations on Complementary Team Performance." CHI Conference. [Evidence on human-AI teams; how explanations affect performance]
- Jacovi, A., & Goldberg, Y. (2020). "Towards Faithfully Interpretable NLP Systems: How should we define and evaluate faithfulness?" ACL. [On validity of confidence scores]
- Amershi, S., et al. (2020). "Software Engineering for Machine Learning: A Case Study." ICSE 2019. [Real-world HITL patterns; lessons from Microsoft]
- Madaio, M. A., et al. (2022). "Envisioning Sociotechnical Intelligent Assistant Systems." arXiv. [HITL in enterprise context]
- Enterprise AI Council (2023). "AI Governance in Regulated Industries: Best Practices." [SOX, HIPAA, compliance patterns]

---

## Final Notes for the Director

This skill is deliberately enterprise-focused. The frameworks here are built from deployed systems that cost millions when they failed. The threshold calibration methodology exists because a $500M fintech firm learned it the hard way. The SLA escalation patterns exist because human approvers will miss deadlines if you let them. The audit trail requirements exist because regulators don't care that your AI was "95% confident."

Your role as PM is not to build perfect AI. It's to build AI that humans will trust enough to adopt, with enough safeguards that you won't wake up at 2 AM to an audit violation. That's what HITL is. That's what this skill teaches.

Build it well. The enterprise is counting on you.
