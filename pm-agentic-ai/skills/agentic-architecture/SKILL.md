---
name: agentic-architecture
description: >
  Design and evaluate agentic AI systems for enterprise scale. Learn how to architect multi-agent systems with proper governance, cost models, and guardrails. This skill covers the complete topology decision framework, memory architectures, and tool orchestration patterns that differentiate successful enterprise deployments from "agentic theater." Includes a detailed worked example of automating invoice processing in a Source-to-Pay platform with real cost and compliance considerations. You'll learn when autonomous execution is appropriate versus augmented intelligence, how to structure agent topology for scalability, and the specific anti-patterns that destroy enterprise trust. For product leaders shipping agentic systems at scale.
---

## Purpose

This skill teaches you how to architect agentic AI systems that work reliably in enterprise production. Not "agentic" in marketing speak—actual systems where AI agents make decisions, take actions, and face consequences. You'll learn to evaluate when your product needs agency, design topology for scale and cost, implement proper memory, select tools strategically, and place guardrails where failures cost money or compliance violations.

Use this skill when you're designing a new product feature or entire product that involves AI making autonomous or semi-autonomous decisions. Use it before you bring in engineering. Use it to pressure-test product designs that sound ambitious but might be technically reckless.

## Key Concepts

### The Agency Spectrum

Not all automation is agency. The level of autonomy you grant to an AI agent directly impacts your risk surface, operational cost, and compliance burden. This is a PM decision before it becomes an engineering problem.

**Level 1: Rule-Based Automation**
- The system applies deterministic logic to data. If X, then Y.
- No learning. No reasoning. Just configuration.
- Example: Automatically route invoices to AP based on vendor ID matching a lookup table.
- Cost: Low. Risk: Low. Compliance friction: Minimal.
- User experience: Predictable, boring, reliable.

**Level 2: Workflow Automation**
- The system chains tasks based on pre-defined workflows. It follows a script but adapts within that script.
- Examples: Invoice arrives → OCR extraction → parse line items → route to manager if exceeds threshold → send approval email.
- No real decision-making. Just orchestration of predetermined paths.
- Cost: Medium. Risk: Medium. Compliance friction: Moderate (need audit trails of state transitions).
- User experience: Transparent ("system is doing X right now") but still requires human intervention at gates.

**Level 3: Augmented Intelligence**
- AI augments human judgment. The agent analyzes data, proposes options, highlights risks, but humans decide.
- Examples: AI flags invoice discrepancies (PO quantity mismatch, price variance >10%, duplicate vendor name spellings) and surfaces them to AP clerk. Clerk reviews and decides.
- Cost: Medium. Risk: Medium (humans still decide). Compliance friction: Moderate (need to audit what AI flagged vs. what human chose).
- User experience: Faster, fewer errors, but humans remain in critical loops. Feels intelligent without being scary.

**Level 4: Autonomous Agency**
- The agent makes decisions and takes action without human review in-the-loop.
- Examples: Agent automatically matches invoice line items to PO lines within tolerance bands, updates inventory, routes to payment only if all validation gates pass, records audit trail, notifies stakeholders.
- Cost: Potentially high (due to agent complexity and tool integration). Risk: High (bad decisions cascade). Compliance friction: HIGH (audit trails, explainability, exception handling all mandatory).
- User experience: Transactions happen invisible to users unless something goes wrong (then it must be transparent, auditable, reversible).

**Decision Matrix for Your Feature**

| Question | Implies Level | Notes |
|----------|--------------|-------|
| Can a human easily verify the output? | 1-2 | If "yes", lower autonomy OK |
| Is the decision reversible? | 1-3 | Reversible decisions can be autonomous; irreversible need human gates |
| What's the cost of a wrong decision? | 1-4 | $10 = autonomous OK. $100K+ = must have human review. |
| Is this regulated? | 1-3 | Compliance often mandates human accountability |
| How confident can the agent be? | 1-4 | <90% confidence = augmented only. >98% = maybe autonomous. |
| How fast must it be? | 1-4 | Real-time needs automation. Daily batch can use augmented. |
| What happens if the agent hallucinates? | 1-4 | Hallucinations in payment = autonomous is insane. |

---

### Agent Topology Patterns

How you arrange agents—whether you use one or many, how they coordinate—determines scalability, cost, and maintainability. This is your architecture diagram for agentic systems.

**Pattern 1: Single Agent (Augmented LLM)**

One agent with access to multiple tools. The agent reasons about which tool to call, in what order, and how to synthesize results.

**When to use:**
- Simple intent-to-action mapping (customer asks question, agent researches and answers).
- Low task complexity (<3 distinct sub-problems).
- Tight latency requirements (every hop adds milliseconds).
- Low volume (doesn't need to scale to thousands per second).

**When NOT to use:**
- Multi-domain problems (invoice processing requires invoice expertise AND procurement expertise AND finance rules expertise).
- High error sensitivity (one tool failing cascades through the whole system).
- Significant tool contention (if tool A is slow, the agent waits, blocking all progress).

**Enterprise considerations:**
- Single point of failure for your entire feature. If the agent is degraded, everything is degraded.
- Context window cost explodes if you add too many tools. Each tool definition and example is tokens.
- Difficult to audit *why* the agent chose this action over that one (black box reasoning).
- Hard to version and A/B test (you're versioning the entire system, not specific reasoning modules).

**Cost profile:**
- Input tokens: Moderate (one system prompt, one tool set, conversational context).
- Output tokens: Moderate (agent generates reasoning then tool calls).
- Tool calls: All against one agent's decision-making, so throughput bottleneck if agent is slow.
- Per-transaction cost: ~$0.01-0.05 depending on context length.

---

**Pattern 2: Router Pattern**

A lightweight classifier looks at user input and routes to a specialist agent. The router asks: "Is this a question about pricing, returns, or shipping?" Then calls the appropriate specialist.

**When to use:**
- Multiple distinct domains that use different knowledge/tools.
- High volume with heterogeneous requests.
- You want to audit which specialist handled which class of request.
- Domain specialists have conflicting tools or conflicting reasoning styles.

**When NOT to use:**
- Requests frequently span multiple domains (router can't cleanly classify).
- Classification itself is ambiguous (unclear if user is asking about returns or refunds—related but different contexts).
- Latency must be <500ms (router adds a hop).

**Enterprise considerations:**
- Requires explicit routing logic. Document your router's classification scheme or you'll get ambiguous edge cases.
- Each specialist can be optimized for its domain, making auditing and compliance easier.
- Specialist models can differ (you might use Claude Sonnet for complex legal Q&A but Claude Haiku for FAQ routing).
- Router must handle misclassification gracefully (what if user question is ambiguous?).

**Cost profile:**
- Router call: Cheap (brief classification prompt).
- Specialist call: Varies (some specialists heavier than others).
- Total cost: Similar to single agent if specialist calls are comparable, but more observable (you can see which specialist cost how much).
- Per-transaction: ~$0.02-0.08.

---

**Pattern 3: Orchestrator-Worker Pattern**

A central orchestrator coordinates multiple worker agents. The orchestrator plans the steps, parcels out work, collects results, and synthesizes a final output.

**When to use:**
- Complex multi-step workflows that can be decomposed.
- You want parallel execution of independent tasks (faster than sequential).
- Different workers have different tool sets or reasoning styles.
- You need explicit control over task dependencies (task C depends on results of A and B).

**When NOT to use:**
- Simple linear workflows (use single agent instead, less latency).
- Tasks are highly interdependent (constant back-and-forth between orchestrator and workers).
- You don't understand the task decomposition yet (first understand the problem, *then* decompose it).

**Enterprise considerations:**
- Orchestrator becomes a critical dependency. If it fails, all workers are orphaned.
- Requires explicit state management (which tasks are done, which are in progress, how to handle timeouts or failures).
- Makes error handling more complex (if worker 2 fails, does worker 3 proceed? Does orchestrator retry?).
- Audit trail complexity (need to track not just final output but intermediate steps and decisions).
- Easier to cost-analyze (you see orchestrator cost + each worker cost separately).

**Cost profile:**
- Orchestrator calls: Multiple (plan, collect results, synthesize).
- Worker calls: Parallel, so time-efficient but potentially token-wasteful if workers duplicate context.
- Context duplication risk: Each worker might need the full problem context, multiplying input tokens.
- Per-transaction: ~$0.05-0.20 depending on worker count and parallelization.

Example: Invoice processing orchestrator plans to: (1) extract invoice data, (2) match to PO, (3) validate against procurement rules, (4) check compliance rules, (5) route for approval. Workers handle each step in parallel. Orchestrator collects results and decides next action.

---

**Pattern 4: Parallel Fan-Out (Scatter-Gather)**

Send the same task to multiple agents in parallel. Agents work independently, then you gather and merge results. Useful for consensus or diversity.

**When to use:**
- You want multiple perspectives on the same problem (multiple extractors to reduce hallucination risk).
- You're optimizing for accuracy over latency (willing to pay 2x the cost for higher confidence).
- You need consensus-based decisions (override if 2 of 3 agents agree).
- You have budget for parallelization (3 agents working in parallel cost 3x).

**When NOT to use:**
- Latency is critical (3x parallel = 3x wait time per agent).
- Budget is tight (expensive pattern).
- Task is deterministic (no benefit to multiple perspectives).

**Enterprise considerations:**
- Need a merge strategy (how do you decide which agent is right if they disagree?).
- Audit trail gets complex (three different paths to the same answer, need to explain which one was chosen and why).
- Useful for high-stakes decisions (invoice approval, compliance exceptions) where you need confidence >95%.

**Cost profile:**
- Input tokens: Multiplied by agent count (3 agents = 3x input cost).
- Output tokens: Separate per agent (3 streams of output).
- Parallelization: Doesn't reduce wall-clock time if you wait for all agents to finish.
- Per-transaction: ~$0.10-0.50 depending on agent count and complexity.

---

**Pattern 5: Evaluator-Optimizer (Generate-Evaluate-Refine)**

Agent generates an output (e.g., proposed invoice matching). A separate evaluator checks it (is this matching actually valid?). If it fails evaluation, the generator refines and tries again. Loops until passing evaluation or max attempts.

**When to use:**
- You have clear success criteria (you can write rules to evaluate outputs).
- Iterative refinement is possible (agent can learn from feedback and improve).
- You want to maximize quality for a finite time budget (loop 2-3 times then give up).
- You're optimizing for accuracy, not latency.

**When NOT to use:**
- You can't define what "good" looks like (no evaluator rules).
- Latency must be <1 second (loops kill latency).
- Agent is already high quality (most agents >90% first-attempt success don't benefit from looping).

**Enterprise considerations:**
- Need explicit evaluation rules that are auditable (why did evaluation fail?).
- Must set max iterations (prevent infinite loops).
- Need timeout per iteration (prevent agent from hanging).
- Useful for complex tasks (invoice matching with 50+ line items) where first-attempt accuracy is ~80% but loops push it to 95%+.

**Cost profile:**
- First attempt: Same as single agent.
- Per loop: Additional generate (output tokens) + evaluate cost.
- If average 1.5 loops: Cost = ~1.5x single agent cost.
- Per-transaction: ~$0.02-0.10 depending on task and loop count.

---

### Memory Architecture for Product Managers

How the agent remembers context shapes the user experience and cost. This isn't about vector databases or RAG—it's about what the agent knows when it acts.

**Working Memory (Context Window)**

The immediate, active context. For an invoice agent: the current invoice, the PO it's matching against, the compliance rules in scope, the user who requested this.

- Size limit: Claude 3.5 Sonnet = 200K tokens. That's ~150 pages of text. Sounds big, but depletes fast.
- Cost impact: Every token in working memory costs money. A 50K-token context = expensive.
- UX impact: If you include too much context, agent gets confused (more hallucinations). If too little, agent makes mistakes (missing information).

**Key PM questions:**
- How much context does the agent *actually need* to make this decision? (Not "all available context"—what's actually relevant?)
- Are you repeating context across multiple agent calls? (If you call the agent 3 times in a workflow, you're paying for the full context 3 times.)
- What happens when context window fills up? (Does the agent fail? Does it forget early context? Does the user experience degrade?)

**Episodic Memory (Interaction History)**

The agent remembers what happened in this session. "User asked to approve invoices from Vendor X. I flagged 3 discrepancies. User approved 2 and rejected 1."

- Cost impact: If you include all prior turns in context, history grows linearly. 10-turn conversation = 10x context.
- UX impact: Agent can reason across turns ("last time you rejected this because of the date, is this one OK?"). Without it, agent can't learn from your feedback.
- Retention: In a session (minutes to hours). Not persistent across sessions.

**Key PM questions:**
- Do users interact with the agent once per day, or in long multi-turn sessions?
- Does the agent need to remember your feedback to improve within this session?
- How do you prevent history from bloating context? (Compress? Summarize? Drop old turns?)

**Semantic Memory (Knowledge/Data)**

The agent has access to structured knowledge: procurement policies, vendor data, PO history, compliance rules, organizational structure. Usually stored outside the agent (vector DB, structured queries to APIs) and pulled in as needed.

- Cost impact: Retrieval calls have cost, but usually <$0.001 per call. Storage is separate from agent cost.
- UX impact: Agent can reference "company policy says..." without hallucinating. More authoritative.
- Latency impact: Each retrieval adds 100-500ms. Complex workflows with many retrievals slow down.

**Key PM questions:**
- What domain knowledge does the agent *need* to act correctly? (Policies? Historical data? Reference information?)
- How fresh does this knowledge need to be? (Yesterday's vendor list OK, or must be real-time?)
- How do you keep semantic memory up-to-date? (Auto-synced from source systems, manual updates, versioned and rolled out?)

**Procedural Memory (Learned Workflows)**

The agent learns how to do things by doing them. After handling 100 invoices, it's better at matching than on invoice 1. This is implicit in the model weights, hard to inspect, and usually not persistent across sessions.

- Cost impact: None (it's baked into the model).
- UX impact: Agent improves during a session, degraded at session restart.
- Explainability impact: You can't inspect *why* the agent does something (it just learned from examples).

**Key PM questions:**
- Do you want the agent to improve during a session? (Useful for augmented workflows, risky for autonomous.)
- Are you fine with the agent "forgetting" between sessions? (Or should learning persist?)
- How do you prevent the agent from learning bad habits? (If it makes a mistake and isn't corrected, does it replicate the mistake?)

---

### Tool and MCP Selection Framework

Tools and MCPs (Model Context Protocols) are how agents act on the world. Selecting the right tools shapes the agent's capability, cost, and risk profile. This is PM decision territory.

**Scoring Framework**

For each potential tool/MCP, score it on four dimensions. High score = include it. Low score = question whether it's necessary.

| Dimension | Scoring | Example |
|-----------|---------|---------|
| **Frequency of Use** | Daily = 10pts. Weekly = 5pts. Monthly = 2pts. Never = 0pts. | Invoice matching uses "fetch PO" daily. "Fetch vendor tax ID" maybe monthly. |
| **Data Sensitivity** | Public = 1pt. Internal = 3pts. Restricted (finance data) = 7pts. Regulated (compliance, audit) = 10pts. | Vendor IDs = restricted (3pts). PO amounts = regulated (10pts). |
| **Failure Impact** | Informational (tool returns wrong info) = 2pts. Operational (workflow delayed) = 4pts. Financial (wrong payment) = 8pts. Compliance (audit failure) = 10pts. | Wrong vendor name = operational (4pts). Wrong payment amount = financial (8pts). |
| **Latency Tolerance** | Real-time (<100ms) = 10pts. Near-real-time (1s) = 5pts. Batch (5min+) = 1pt. | Matching PO must be fast (10pts). Sending approval email can be delayed (1pt). |

**Decision Rule:** Sum scores. >20 = must-have tool. 10-20 = consider it. <10 = question it. If >30, you have tool sprawl; consolidate.

**Example: Invoice Processing Agent**

| Tool | Frequency | Sensitivity | Failure Impact | Latency | Total | Include? |
|------|-----------|-------------|-----------------|---------|-------|----------|
| Fetch PO by PO number | 10 (daily) | 7 (restricted) | 8 (financial) | 10 (real-time) | 35 | YES |
| Match invoice line to PO line | 10 | 7 | 8 | 10 | 35 | YES |
| Check vendor master data | 10 | 7 | 4 (operational) | 5 (1s OK) | 26 | YES |
| Validate invoice amount vs 3-way match | 10 | 10 (regulated) | 10 (compliance) | 10 | 40 | YES |
| Fetch historical invoice variance data | 5 (weekly) | 7 | 4 | 1 (batch) | 17 | QUESTION |
| Send approval routing email | 10 | 1 (approval routing is internal) | 2 (informational) | 1 (batch) | 14 | QUESTION |
| Audit log creation | 10 | 10 | 10 | 5 | 35 | YES |
| Look up cost center from GL account | 2 (rare) | 7 | 2 | 1 | 12 | QUESTION |

Result: Must-have tools = PO fetch, line matching, 3-way validation, audit logging. Nice-to-have = historical variance, approval emails. Consider whether cost-center lookup is worth the complexity.

---

## Application

Here's the step-by-step process for designing an agentic system. Each step has specific deliverables. Answer the questions at each step honestly.

**Step 1: Define the Problem and User Context**

Who is the user? What are they trying to do today? What's hard about it?

*Deliverables:*
- 1-page user narrative (not fiction—ground it in actual user research or operational data).
- Current state: How do users do this today? How long does it take? What errors happen?
- Desired state: What would be 10x better? (Not 10% better. 10x.)
- Success metrics: How do we know the agent is better? (Time saved? Errors reduced? Cost reduced?)

*Questions to answer:*
- Are users asking for "AI automation" or are they asking for "less manual work"? (If the latter, automation is the solution, not necessarily agentic AI.)
- What do users currently do wrong that an agent could avoid?
- What do users know that an agent wouldn't? (This identifies where you need augmentation, not autonomy.)

---

**Step 2: Establish Autonomy Level**

Based on the problem, what agency level is appropriate?

*Deliverables:*
- Clear decision: Rule-Based? Workflow? Augmented? Autonomous?
- Justification tied to: reversibility, cost of failure, regulatory constraints, user trust, latency requirements.
- Stakeholder alignment (especially legal/compliance if regulated).

*Questions to answer:*
- If the agent makes a mistake, can a human undo it in 5 minutes? If yes, autonomous is safer. If no, must be augmented.
- What's the worst-case scenario if the agent fails? ($100 loss? $1M loss? Compliance violation? Reputational damage?)
- Do users explicitly want autonomy, or are they just asking for help?

---

**Step 3: Select Agent Topology**

Based on the problem complexity and your autonomy level, which topology makes sense?

*Deliverables:*
- Topology diagram showing agent structure.
- Rationale for why you rejected other topologies.
- Identification of critical dependencies (what can't fail?).

*Questions to answer:*
- Can this be solved with one agent thinking hard, or does it require multiple specialists?
- What are the independent vs. dependent sub-tasks? (Dependency chain = sequential = potential bottleneck.)
- If something goes wrong (tool timeout, hallucination, network blip), what's your fallback? (Every topology needs a fallback strategy.)

---

**Step 4: Design Memory Architecture**

What does the agent need to remember?

*Deliverables:*
- Memory layer diagram: What goes in context window vs. external knowledge vs. retrieved on-demand?
- Context budget: How many tokens can you afford per transaction? (At 10K requests/day, $0.10/1M input tokens = $0.01/100K tokens = $0.10/day budget = max 1K tokens per transaction if free tier.)
- Knowledge source identification: APIs? Vector DB? Structured queries? Manual updates or auto-sync?

*Questions to answer:*
- What information is essential (agent fails without it)? (Goes in context.)
- What information is "nice to have" (agent works better with it)? (Retrieved on-demand.)
- What information is obsolete in <1 hour? (Don't cache; query fresh.)
- What information is obsolete in >1 day? (Cache it; update daily.)

---

**Step 5: Define Tool and MCP Set**

Which tools does the agent need to act?

*Deliverables:*
- Tool/MCP inventory with scoring (using framework above).
- For each tool: failure modes (what if it times out? returns wrong data? is unavailable?).
- Fallback strategy for each tool (if tool X fails, agent does Y).
- Cost estimate per tool per transaction.

*Questions to answer:*
- Which tools are non-negotiable (agent is useless without them)?
- Which tools are "nice to optimize" (agent works without them but slower)?
- What's your tool latency budget? (If agent waits >2 seconds for a tool, users notice.)
- How do you prevent tool sprawl? (Set a max tool count, e.g., 8 tools per agent.)

---

**Step 6: Design Guardrails and Governance**

How do you prevent the agent from doing bad things?

*Deliverables:*
- Explicit rules the agent must follow (cannot do X, must do Y before Z).
- Guardrail implementation: Soft (instructions in prompt) vs. hard (tool-level access control).
- Escalation rules: What triggers human intervention? (Amount threshold? Compliance risk? Low confidence?)
- Audit trail schema (what events must be logged for compliance?).

*Questions to answer:*
- What decisions must a human review before execution? (Compliance, >$X, new vendor?)
- What decisions can be autonomous but must be auditable? (Invoice match, approval routing?)
- If the agent is uncertain, does it ask, or guess? (Augmented = ask. Autonomous = must be high confidence.)
- How do you handle edge cases? (Request out-of-spec, data missing, conflicting rules?)

---

**Step 7: Build the Reasoning Loop**

How does the agent actually solve the problem?

*Deliverables:*
- Detailed prompt(s) that specify the agent's role, task, constraints, and expected behavior.
- Examples of inputs and expected outputs (few-shot prompts).
- Error recovery instructions (if the agent recognizes it made a mistake, what does it do?).
- Stop conditions (when does the agent stop reasoning and take action?).

*Questions to answer:*
- Can you write down, in plain English, how to solve this problem? (If you can't explain it to a human, you can't prompt an agent to do it.)
- What happens if the agent gets stuck (task appears unsolvable)? (Timeout? Escalate? Retry?)
- How do you test the agent's reasoning? (Do you validate the intermediate steps, or just the final output?)

---

**Step 8: Design the Cost Model**

What's the economics of this agent?

*Deliverables:*
- Per-transaction cost breakdown (input tokens + output tokens + tool calls).
- Break-even analysis: Agent cost vs. human labor cost. At what transaction volume is the agent cheaper?
- Scaling model: How does cost grow with transaction volume? (Linear? Sub-linear due to batching?)
- Budget allocation: If you have $10K/month for agent costs, what transaction volume can you support?

*Questions to answer:*
- What's the cost of human doing this today? (Salary cost per transaction.)
- What's the cost of agent doing this? (API cost per transaction.)
- What's your ROI threshold? (Must agent pay for itself in <6 months? <1 year?)
- What happens if token prices increase 50%? (Does the model still work?)

---

**Step 9: Plan Rollout and Monitoring**

How do you release this safely?

*Deliverables:*
- Rollout plan: Shadow mode? Opt-in cohort? Gradual volume ramp?
- Monitoring dashboard: Key metrics (success rate, cost per transaction, latency, error types).
- Feedback loop: How do users report problems? How do those reports get to the product team?
- Evaluation criteria: When is the agent ready for full rollout? (>95% success rate? <$0.05 per transaction? <2s latency?)

*Questions to answer:*
- What's your confidence in this design? (If <80%, run a pilot with 100 transactions first.)
- What can you learn from a pilot that will change the design? (Tool latency? Hallucination patterns? Cost overruns?)
- How do you handle agent failures in production? (Automated retry? Escalation to human? Notification to support?)

---

**Step 10: Define Success Criteria and Iteration Plan**

How do you know this was worth building?

*Deliverables:*
- Success metrics tied to user outcomes (time saved, error reduction, cost, user satisfaction).
- Cadence for reviewing metrics (weekly? monthly?).
- Iteration roadmap: If agent is performing below target, what will you change? (Different prompt? Additional tools? Different model?)

*Questions to answer:*
- What does success look like 30 days after launch? 90 days? 1 year?
- Which metrics matter most? (For enterprise, usually cost + compliance + user satisfaction, in that order.)
- If the agent is performing at 90% success, is that good enough, or must you iterate to 95%+?

---

## Worked Example: Enterprise Invoice Processing Agent

This is the complete, realistic design of an agentic system for a real enterprise problem. This is what separates actual product thinking from theoretical frameworks.

**Context: Zycus Source-to-Pay Platform**

Zycus serves large enterprises (1000s of employees, multiple business units) that spend $100M+ annually on procurement. Invoices flow in daily: 5,000-50,000 per day depending on client size. Current process:

1. Invoice arrives via email or EDI.
2. OCR extracts invoice data (vendor, amount, line items, date).
3. AP clerk manually matches to PO (does invoice amount match PO? Are line items and quantities correct?).
4. Clerk flags discrepancies (3-way match failures).
5. Discrepancies routed to procure or finance for resolution (manual back-and-forth).
6. Approved invoices go to payment, rejected ones sent back to vendor.

**Current state metrics:**
- Processing time: 2-5 days (slow due to manual matching and exception routing).
- Match success (first-time): 70% (30% require manual intervention due to discrepancies).
- Cost per invoice: ~$3-5 in labor (AP clerk salary-loaded at $40/hour, ~6-10 minutes per invoice).
- Error rate: 2-3% (wrong matches, missed discrepancies, duplicate payments).
- Compliance: Audit-heavy (every match decision must be traceable and reversible).

**Desired state (10x better):**
- Processing time: <2 hours (invoices matched and routed same day).
- Match success: >95% autonomous, <5% escalated for human review.
- Cost per invoice: <$0.10 in API costs (30x cheaper than labor, plus freeing AP staff for higher-value work).
- Error rate: <0.1% (AI more consistent than humans for routine matching).
- Compliance: Full audit trail, all decisions explainable, all decisions reversible (can undo within 24 hours).

---

### Step 1: Problem Definition

**User narrative:**
Sarah is an Accounts Payable Manager at a Fortune 500 company. Her team of 8 processes 15,000 invoices per month. They're drowning. Each invoice takes 6-10 minutes to match to a PO: find the PO number, verify quantities match, check prices, flag discrepancies, route to the right person for resolution. For 15,000 invoices, that's 1,500-2,500 hours of labor per month. Half the invoices need escalation due to discrepancies (damaged goods, partial shipments, price changes, duplicate invoices, typos). Sarah's team is burned out. The company is losing supply chain visibility because invoices sit in queues for days. Vendors complain about slow payment. Finance can't close the books on time due to invoice backlogs.

**Current state:**
- Process: Email/EDI → OCR → spreadsheet or legacy system matching → manual exception handling → payment.
- Time: 2-5 days per invoice (queue time dominates).
- Errors: ~300/month (2-3%) due to fatigue, typos, missed edge cases.
- Cost: 8 FTEs × $50K salary × 40% time on invoice matching = $160K/year. Plus cost of payment delays (supply chain risk, vendor friction).

**Desired state:**
- Invoices matched automatically within minutes of arrival.
- Discrepancies detected and routed immediately to responsible person (not generic queue).
- Sarah's team moves from "matching invoices" to "managing exceptions" (higher-value work).

**Metrics:**
- Processing time: <2 hours.
- Match success rate: >95% autonomous.
- Labor cost: <$0.10 per invoice.
- Error rate: <0.1%.
- User satisfaction: Sarah's team reports it as "makes their job easier, not scarier."

---

### Step 2: Autonomy Level Decision

**Decision: Hybrid Autonomous + Augmented**

Here's why:

- **Reversibility:** Invoice matching *is* reversible. If an agent matches invoice #12345 to PO #98765 and they're wrong, the payment can be held and corrected within 24 hours. No permanent damage.
  
- **Cost of failure:** Wrong match → payment delays or overpayment. Impact: $1K-10K per error (depending on amount), but rare if confidence is high.

- **Latency:** Must be fast (payment cannot wait 3 days for human review). Autonomy enables speed.

- **Regulatory:** Source-to-Pay is regulated (SOX, procurement compliance, audit). Every decision must be auditable and explainable.

- **Confidence requirement:** Agent can be confident (>98%) on ~70% of invoices (straightforward matches). On remaining ~30% (edge cases, discrepancies), agent flags for human review.

**Implementation:**
- **Low-risk invoices:** Autonomous matching. Agent matches, updates PO status, prepares payment, logs audit trail. No human review unless flag triggered.
- **High-risk invoices:** Augmented. Agent flags discrepancies, proposes resolution options, routes to appropriate human (procure for quantity mismatches, finance for price variance, compliance for vendor issues). Human decides.
- **Critical-risk invoices:** Manual only. Agent detects potential compliance issue (new vendor, unusual amount, duplicate), escalates to compliance team.

Cost: Low-risk autonomous = $0.02 per transaction. High-risk augmented = $0.05 per transaction (higher cost due to reasoning about options). Blended = $0.03/transaction, still 50-100x cheaper than human labor.

---

### Step 3: Agent Topology

**Decision: Orchestrator-Worker Pattern**

Why not single agent? Invoice matching requires at least 4 distinct skillsets:
1. **Extraction specialist:** Parse invoice data (vendor, amount, line items, dates). Handle poor OCR quality, variant formats.
2. **Matching specialist:** Find the right PO, match line items, detect discrepancies (qty mismatch, price variance, damaged goods, etc.).
3. **Compliance specialist:** Check vendor against blocked-party lists, validate compliance rules, flag unusual patterns.
4. **Routing specialist:** Decide where to escalate exceptions (procurement? Finance? Compliance? CFO?).

Single agent trying to do all four = confusion and hallucinations.

**Topology:**

```
USER SUBMISSION (Invoice)
    |
    v
ORCHESTRATOR (Invoice Router Agent)
    - Validates invoice format
    - Routes to workers in parallel
    - Collects results
    - Makes routing decision
    |
    +---> EXTRACTION WORKER --> Raw invoice data
    |
    +---> MATCHING WORKER --> PO match, discrepancies
    |
    +---> COMPLIANCE WORKER --> Risk assessment
    |
    v
DECISION LOGIC (Orchestrator)
    - All workers successful? --> AUTONOMOUS PROCESSING
    - Some issues detected? --> ROUTE TO HUMAN (with context)
    - High risk? --> ESCALATE TO COMPLIANCE
    |
    v
ACTION (Orchestrator)
    - Update PO status
    - Create approval routing task
    - Log audit trail
    - Notify stakeholders
```

**Worker specialization:**

| Worker | Tools | Confidence Target | Autonomous? |
|--------|-------|-------------------|------------|
| Extraction | OCR enhancement, vendor lookup, line-item parser | >95% | Yes (async, non-blocking) |
| Matching | PO lookup, line-item matcher, 3-way validation, variance calculator | >98% | Yes if low-variance |
| Compliance | Vendor risk lookup, blocked-party API, policy checker, audit rules | >99% | Yes if no risk |
| Routing | Escalation rule engine, email dispatcher, exception queue | >90% | Yes |

**Critical design detail:** Workers run in parallel. Orchestrator doesn't wait for extraction to complete before querying matching. This reduces latency from ~5 seconds (sequential) to ~2 seconds (parallel).

---

### Step 4: Memory Architecture

**Working Memory (Context for each agent call):**
- Orchestrator: Full invoice (OCR'd text, meta), user context, configuration. ~2K tokens.
- Extraction: Invoice data only, no PO context. ~3K tokens.
- Matching: Invoice data + fetched PO details + historical variance data. ~5K tokens.
- Compliance: Invoice meta + vendor data + policy list. ~3K tokens.

Total context: ~13K tokens per transaction. At 10,000 invoices/day, that's 130M tokens/day. At $2/1M input tokens (Claude Opus batch), that's $0.26/day or ~$80/year for a single customer. Scales to $800-8000/year for large customer (10-100K invoices/day). Acceptable.

**Episodic Memory:**
- Not used in this design. Each invoice is independent; no need to remember previous invoices. (If an invoice depended on prior matching decisions, episodic memory would help, but S2P doesn't work that way.)

**Semantic Memory:**
- Vendor master (ID, name, address, compliance status): Query API, cache for 24 hours.
- PO data: Query ERP (real-time). Cannot be stale; PO status changes frequently.
- Compliance policies (blocked-party lists, approval rules): Query API, cache for 1 hour (compliance is sensitive).
- Variance thresholds (10% price variance acceptable? 5%?): Baked into matching rules, updated quarterly.

**Procedural Memory:**
- Not explicitly used. Agent doesn't learn during session. (Workaround: Monthly retraining of matching patterns based on resolved exceptions.)

---

### Step 5: Tool and MCP Set

**Scoring each tool:**

| Tool | Frequency | Sensitivity | Failure Impact | Latency | Score | Include? |
|------|-----------|-------------|-----------------|---------|-------|----------|
| Extract invoice via OCR | 10 (every invoice) | 3 (internal) | 4 (operational) | 10 | 27 | YES |
| Lookup PO by PO# | 10 | 7 (restricted) | 8 (financial) | 10 | 35 | YES |
| Match invoice line to PO line | 10 | 7 | 8 | 10 | 35 | YES |
| Calculate price variance | 10 | 7 | 8 | 10 | 35 | YES |
| Validate 3-way match | 10 | 10 (regulated) | 10 (compliance) | 10 | 40 | YES |
| Check vendor blocked-party | 10 | 10 | 10 | 5 (can be slightly async) | 35 | YES |
| Fetch variance tolerance rules | 10 | 7 | 4 | 1 (mostly static) | 22 | YES |
| Create approval routing task | 10 | 7 | 2 (operational) | 5 | 24 | YES |
| Log audit trail | 10 | 10 | 10 (compliance) | 1 (async OK) | 31 | YES |
| Notify stakeholders (email) | 9 (for escalations) | 1 | 2 | 1 | 13 | QUESTION |
| Historical variance lookup | 5 (weekly) | 3 | 2 | 1 | 11 | OPTIONAL |
| Duplicate detection | 5 | 3 | 8 (financial) | 5 | 21 | CONSIDER |

**Final tool set:**
- **Must-have (score >30):** Extract, lookup PO, line matching, price variance, 3-way validation, blocked-party, audit logging. (7 tools)
- **Important (score 20-30):** Variance rules, approval routing, duplicate detection. (3 tools)
- **Optional (score <20):** Stakeholder notifications. (Can batch these async.)

Total: 10 tools. This is in the "robust but not sprawling" range. Reduces to 7 if you cut duplicates and email.

**Failure modes and fallbacks:**

| Tool | Failure Mode | Impact | Fallback |
|------|--------------|--------|----------|
| Extract OCR | Poor quality (garbled text) | Agent misses data | Flag invoice for manual OCR |
| Lookup PO | PO not found (typo?) | Cannot match | Escalate to procurement (user had to type PO#) |
| Line matcher | Timeout (ERP slow) | Delay | Retry up to 3x with backoff, then escalate |
| Blocked-party | API down | Compliance risk | Assume risk, escalate to compliance, log as override |
| Audit log | DB write fails | Compliance violation | Retry sync, then queue async write, alert infra |

Every tool has a fallback. If a tool is critical and has no fallback, you don't include it. (Example: If audit logging had no fallback, you can't use autonomous agents—it's non-negotiable for compliance.)

---

### Step 6: Guardrails and Governance

**Explicit rules (baked into agent instructions and tool access):**

1. **Never match if confidence <90%.** If agent is uncertain about the match, it must flag and escalate.

2. **Never approve if variance >15%.** Price variance of 15%+ is suspicious (could indicate fraud, damage, quantity issue). Must escalate to finance.

3. **Never process invoices with blocked vendors.** Blocked-party check is non-negotiable. If vendor is on any blocklist, escalate to compliance immediately, do not process further.

4. **Always create audit trail before taking action.** Before updating PO status or creating payment, log: invoice ID, PO ID, match confidence, discrepancies detected, decision made, timestamp, user (if human involved).

5. **Route high-value invoices (>$100K) to human review.** Even if match is perfect, large amounts get human approval. Risk tolerance varies by org.

6. **Flag duplicate invoices.** If agent detects potential duplicate (same vendor, same amount, same PO within 30 days), escalate. Do not process duplicate payments.

7. **Escalation must be to specific person, not generic queue.** Quantity mismatch → to receiving manager. Price variance → to finance manager. Vendor issue → to compliance. Don't create generic "exception queue."

**Soft guardrails (in prompt):**

```
You are an invoice processing agent. Your role is to match invoices to purchase orders 
accurately and flag discrepancies for human review.

CRITICAL RULES:
- You must not make autonomous decisions if you have ANY doubt about the match.
- If the match confidence is below 90%, or if any data is missing, escalate to a human.
- If the vendor is flagged as high-risk or blocked, escalate to compliance IMMEDIATELY.
- Price variances over 15% are suspicious and require human approval.
- Large invoices (>$100K) require explicit human approval before payment.
- Every decision must be logged with full reasoning for audit purposes.

ESCALATION PATHS:
- Quantity mismatch: Route to Receiving Manager
- Price variance: Route to Finance Manager
- Vendor issue: Route to Compliance Officer
- New vendor: Route to Procurement Manager
- Amount >$100K: Route to CFO approval queue

When escalating, provide:
1. The specific issue detected
2. Your confidence level
3. Recommended resolution (if you have one)
4. Full invoice and PO data so the human doesn't have to re-research
```

**Hard guardrails (tool-level access control):**

- Agent cannot directly approve payments. (Must go through human-approved routing.)
- Agent cannot modify vendor master data. (Only read access.)
- Agent cannot delete audit logs. (Append-only.)
- Agent cannot send emails directly to external parties (vendors, customers). (Can trigger internal notifications only.)

**Escalation rules (decision tree):**

```
INVOICE RECEIVED
|
├─ EXTRACTION FAILED (poor OCR)
|  └─ FLAG: "OCR quality too low, manual review needed"
|
├─ NO PO FOUND (vendor/PO# mismatch)
|  └─ ESCALATE TO: Procurement Manager (might be new PO, might be data entry error)
|
├─ 3-WAY MATCH FAILS
|  ├─ Quantity mismatch? → Escalate to Receiving Manager
|  ├─ Price variance >15%? → Escalate to Finance Manager
|  ├─ Vendor mismatch? → Escalate to Compliance
|  └─ Other discrepancy? → Escalate to original PO requester
|
├─ VENDOR FLAGS
|  ├─ Blocked-party hit? → ESCALATE TO: Compliance (do not process further)
|  ├─ High-risk vendor? → ESCALATE TO: Procurement (review before payment)
|  └─ New vendor? → ESCALATE TO: Procurement (validation required)
|
├─ AMOUNT CHECK
|  ├─ Invoice >$100K? → ESCALATE TO: CFO approval (regardless of match quality)
|  └─ Invoice <$100K + 3-way match OK + vendor OK + confidence >98%? → AUTONOMOUS PROCESSING
|
└─ AUTONOMOUS PROCESSING (all checks passed)
   ├─ Update PO status to "Matched"
   ├─ Create payment routing task
   ├─ Log audit trail
   └─ Notify AP manager (FYI, invoice matched and queued for payment)
```

---

### Step 7: Reasoning Loop

**Extraction agent prompt:**

```
You are an invoice data extraction specialist. Your task is to extract structured data 
from OCR'd invoice text.

INPUT: Raw OCR text from invoice image

EXTRACT:
1. Vendor name (and alternative names if visible)
2. Invoice number
3. Invoice date
4. PO number (if visible)
5. Line items (description, quantity, unit price, extended price)
6. Total amount
7. Due date
8. Any special notes (discounts, taxes, damage noted)

OUTPUT: JSON with the above fields. Be conservative: if you're not sure about a value, 
mark it as "UNCERTAIN" rather than guess.

EXAMPLES:
Input: "ACME CORP\nInvoice #INV-2024-001\nDate: March 15, 2024\nPO: PO-98765\n
        Item | Qty | Unit Price | Total\n
        Widgets | 100 | $10.00 | $1000.00\n
        TOTAL: $1000.00"

Output: {
  "vendor_name": "ACME CORP",
  "invoice_number": "INV-2024-001",
  "invoice_date": "2024-03-15",
  "po_number": "PO-98765",
  "line_items": [{"description": "Widgets", "quantity": 100, "unit_price": 10.00, "extended_price": 1000.00}],
  "total_amount": 1000.00,
  "due_date": "UNCERTAIN",
  "notes": []
}
```

**Matching agent prompt:**

```
You are an invoice-to-PO matching specialist. Your task is to validate that an invoice 
matches its corresponding purchase order.

INPUT:
- Extracted invoice data (JSON)
- Fetched PO details (JSON)

VALIDATE:
1. Does invoice total match PO total? (Within 5% is OK due to freight/taxes)
2. Do line items match? (Quantity, description, unit price)
3. Are there any quantity discrepancies? (Partial shipment, over-delivery, shortage?)
4. Are there any price discrepancies? (What's the percentage variance?)
5. Are dates reasonable? (Invoice date after PO date? Before delivery?)
6. Any duplicate invoices detected? (Same invoice number, same vendor, recent history?)

OUTPUT: {
  "match_status": "PASS" or "FAIL",
  "confidence": 0.0-1.0 (probability this is the correct match),
  "discrepancies": [
    {"type": "quantity_mismatch", "detail": "Invoice claims 100 units, PO is 95 units", "severity": "high"},
    ...
  ],
  "variance_percentage": 3.2,
  "recommendation": "AUTONOMOUS_PROCESS" or "ESCALATE_TO_<role>"
}

If confidence <90%, recommend escalation regardless of technical match.
If variance >15%, recommend escalation to Finance.
If you find any high-severity discrepancies, recommend escalation.
```

**Compliance agent prompt:**

```
You are a compliance and risk screening specialist. Your task is to assess invoice 
for compliance and risk issues.

INPUT:
- Invoice data
- Vendor data (from master)
- Compliance policies and rules

CHECK:
1. Is vendor on blocked-party list? (SDN, trade sanctions, etc.)
2. Is vendor high-risk? (New vendor, unusual country, compliance flags?)
3. Does invoice comply with company policies? (Approval limits, spending rules?)
4. Any unusual patterns? (Amount very large/small, unusual dates, weird charges?)

OUTPUT: {
  "risk_level": "LOW", "MEDIUM", or "HIGH",
  "flags": [
    {"issue": "Vendor on SDN list", "severity": "CRITICAL", "action": "ESCALATE_TO_COMPLIANCE"},
    ...
  ],
  "recommendation": "PROCEED" or "ESCALATE_TO_COMPLIANCE"
}

If risk level is HIGH or any CRITICAL flag, recommend escalation.
```

---

### Step 8: Cost Model

**Per-transaction costs:**

| Component | Calls | Input Tokens | Output Tokens | Cost |
|-----------|-------|--------------|---------------|------|
| Orchestrator (plan + synthesize) | 2 | 4K | 2K | $0.015 |
| Extraction worker | 1 | 3K | 1K | $0.008 |
| Matching worker | 1 | 5K | 2K | $0.013 |
| Compliance worker | 1 | 3K | 1.5K | $0.009 |
| Tool calls (PO lookup, vendor API, blocking check) | ~5 | N/A | N/A | $0.002 |
| **Total per invoice** | | | | **$0.047** |

(Pricing: Claude 3.5 Sonnet, April 2025: input $3/1M, output $15/1M)

**Scenario: 15,000 invoices/month (customer like Sarah)**

- Autonomous (70% of invoices): 10,500 × $0.047 = $494/month
- Augmented/escalated (30%): 4,500 × $0.070 (higher reasoning cost) = $315/month
- **Total: $809/month in API costs**

**Labor cost comparison:**
- Current state (human matching): 8 FTE × $50K salary × 40% = $16K/month
- New state (agent matching + 1 FTE for exceptions): 1 FTE × $50K × 50% = $2K/month + $0.8K API = $2.8K/month
- **Savings: $13.2K/month, 82% reduction in invoice processing labor**

**ROI:**
- Development cost: ~$100K (3 months, 2-3 engineers)
- Break-even: 100K / 13.2K = 7.6 months
- **Payback period: 8 months**
- **Year 2 savings: $158K** (minus maintenance)

**Scaling:**
- 50,000 invoices/month (larger customer): $2,700 API cost, $156K labor + $2.7K API vs. $27K + $2.7K = $150K saved, 15-month payoff
- 500,000 invoices/month (enterprise): $27K API cost, $1.56M labor savings, 4-month payoff

Cost model is favorable at any scale >1000 invoices/month.

**Risk: Token price increases**
- If token prices increase 50% (input $4.50/M, output $22.50/M): Cost per invoice = $0.07. Still saves $12.9K/month on 15K invoices. Model remains viable.

---

### Step 9: Rollout and Monitoring

**Phase 1: Shadow mode (2 weeks)**
- Agent runs in parallel with human matching.
- Agent makes recommendations, humans execute.
- Goal: Validate agent accuracy without risk.
- Success criteria: Agent agreement with human on >90% of invoices.

**Phase 2: Opt-in pilot (4 weeks)**
- 5 select customers (self-selected from user base) willing to go autonomous.
- Agent processes 100% of non-high-risk invoices.
- Humans review only escalations.
- Goal: Real-world performance, user feedback.
- Success criteria: >95% autonomous success rate, <0.5% error rate, user satisfaction >8/10.

**Phase 3: General availability (gradual ramp)**
- Week 1-2: Announce feature, available by request.
- Week 3-4: Enable for 25% of customer invoices.
- Week 5-6: Enable for 50% of customer invoices.
- Week 7-8: Enable for 100%.
- Pause and investigate if any customer reports >5% error rate.

**Monitoring dashboard (real-time):**

```
┌─────────────────────────────────────────────────────────┐
│ INVOICE PROCESSING AGENT - REAL-TIME DASHBOARD          │
├─────────────────────────────────────────────────────────┤
│                                                          │
│ Invoices Processed Today: 12,847                        │
│ Autonomous Success: 11,045 (85.9%)                      │
│ Escalated: 1,802 (14.1%)                                │
│   - Quantity mismatch: 542                              │
│   - Price variance: 389                                 │
│   - Vendor issue: 287                                   │
│   - Manual review: 584                                  │
│                                                          │
│ Error Rate: 0.08% (10 invoices, under 0.5% SLA)         │
│ Average Processing Time: 1.2 minutes (target: <2min)    │
│ API Cost Today: $602 (target: <$750)                    │
│                                                          │
│ Top Escalation Reasons (Last 7 days):                   │
│   1. New vendors: 342 escalations                       │
│   2. Price variance >15%: 178 escalations               │
│   3. Missing PO: 126 escalations                        │
│                                                          │
│ Recent Errors (Last 7 days):                            │
│   1. INV-2024-034: Duplicate match (vendor duplicate)   │
│   2. INV-2024-089: PO lookup timeout (vendor system)    │
│   3. INV-2024-102: OCR misread quantity (50 vs 5)       │
│                                                          │
│ Action Items:                                           │
│   [ ] Investigate new vendor escalation spike           │
│   [ ] Optimize PO lookup latency                        │
│   [ ] Improve OCR for small numerals                    │
└─────────────────────────────────────────────────────────┘
```

**Feedback loop:**
- Weekly report to product team: Success rate, error types, escalation patterns, cost.
- Monthly customer review: Savings realized, issues encountered, feature requests.
- Quarterly deep-dive: Root-cause analysis of errors, re-training data, model updates.

---

### Step 10: Success Criteria and Iteration

**30-day target (post-launch):**
- Autonomous success rate: >92%
- Error rate: <0.5%
- Processing time: <3 minutes per invoice
- User satisfaction: >7.5/10
- Cost: <$0.06/invoice

**90-day target:**
- Autonomous success rate: >95%
- Error rate: <0.2%
- Processing time: <2 minutes
- User satisfaction: >8.5/10
- Cost: <$0.05/invoice
- Labor savings: Customers report 60-70% reduction in AP labor

**Year 1 target:**
- Autonomous success rate: >98%
- Error rate: <0.1%
- Processing time: <90 seconds
- User satisfaction: >9/10
- Cost: <$0.04/invoice (due to scale + model improvements)
- Labor savings: Customers report 70-80% reduction in AP labor + ability to redeploy to strategic work

**Iteration triggers:**
- If autonomous success rate drops below 90%, pause rollout and investigate.
- If error rate exceeds 0.5%, trigger manual review of all high-value invoices.
- If user satisfaction drops below 7/10, conduct user interviews to understand why.
- If cost exceeds $0.08/invoice, revisit prompt engineering to reduce reasoning steps.

**Roadmap:**
- **Month 1-2:** Stabilize autonomous matching for standard invoices.
- **Month 3:** Add support for multi-line invoices and partial shipments.
- **Month 4:** Integrate with payment system for one-click approval workflows.
- **Month 5:** Add support for multiple languages (invoices often in foreign languages).
- **Month 6:** Extend to approval workflows (route exceptions to right approver based on org hierarchy).

---

## Common Pitfalls

**Pitfall 1: "Agentic Theater"**

What it is: Calling your feature "agentic" or "AI-powered" because marketing demands it, even though it's just workflow automation or rule-based logic.

What happens: Users and stakeholders expect autonomous decision-making. You deliver UI that makes API calls and calls it "AI." User opens the feature expecting intelligence, finds it's just a rule engine. Disappointment. Feature abandoned. Reputation damage.

How to avoid: Be honest about your agency level. "Workflow automation" is not exciting, but it's honest. If you're not at Autonomous level, call it "Augmented Intelligence" (AI assists humans) or "Intelligent Automation" (smart workflow orchestration). Users will respect competence over marketing.

---

**Pitfall 2: Over-Autonomy in Regulated Contexts**

What it is: Granting autonomous decision-making authority to an agent in a compliance-heavy domain (finance, healthcare, procurement).

What happens: Agent makes a mistake (hallucination, edge case not covered in training). The mistake is in production affecting real transactions. Compliance team discovers it. Audit becomes a nightmare. "Who approved this? Where's the approval record? Why didn't a human review this?" Your agent can't answer. You've violated audit trail requirements. Financial restatement. Regulatory fine. Your feature is shut down immediately. Customers lose trust.

How to avoid: In regulated domains, default to Augmented Intelligence (AI assists human, human decides). Human is always in the approval chain. AI surfaces options and risks. Human picks one. Audit trail is clear (AI suggested X, human chose Y, at timestamp Z). Compliant. Also: before building anything in regulated domains, get legal and compliance approval. Not after.

---

**Pitfall 3: Context Window Cost Explosion**

What it is: Including massive context in every agent call (all historical conversations, all reference data, all rules) to make the agent "smarter."

What happens: First 100 transactions cost $0.01 each. Scale to 10,000 transactions/day. Cost per transaction balloons to $0.10-0.50 due to context overhead. Suddenly your model doesn't work economically. You're burning money on tokens that could have been cut. Agent is slower too because it's processing huge context windows.

How to avoid: Be ruthless about context. What does the agent *need* to decide? Not "nice to have." *Need*. Fetch that from the database on-demand, don't include it in the context window. Invoke the "working memory" principle: context window = what's actively in focus. Everything else = retrieved when needed. This halves your token cost and speeds up the agent.

---

**Pitfall 4: Tool Sprawl Without Governance**

What it is: Gradually adding more and more tools to the agent without tracking which tools are actually used, or without removing tools that are no longer needed.

What happens: Agent's tool set grows to 20, 30, 50+ tools. Each tool adds to the context window (tool definitions are tokens). Agent gets confused about which tool to use. Agent starts making mistakes because it's trying to choose between too many options. Debugging becomes a nightmare (which tool failed? Why did the agent choose tool X over Y?). System becomes unmaintainable.

How to avoid: Set a hard max tool count per agent (e.g., 10 tools). Before adding a tool, score it using the framework above. Before keeping a tool, measure its usage monthly. If <5% of transactions use it, remove it (or move it to a specialized agent). Document which tools are critical vs. optional. Treat tool governance as a product responsibility, not engineering. If an engineer wants to add a tool, they need your approval.

---

**Pitfall 5: Ignoring the Cold-Start Problem**

What it is: Deploying an agent without any evaluation or tuning, expecting it to work well from day one.

What happens: Agent launches, success rate is 60%. Too many false positives (agent escalates everything) or false negatives (agent lets bad matches through). Users are frustrated. You scramble to tune the agent. Weeks of iteration. You should have caught this in a pilot.

How to avoid: Always run shadow mode first (agent runs parallel to human, no user-facing impact, let you measure accuracy). Shadow mode for at least 500-1000 transactions to establish baseline. Only then move to pilot. Pilot with 10-20% of users, not all. Learn before scaling.

---

**Pitfall 6: Building Multi-Agent When Single-Agent Suffices**

What it is: Designing an orchestrator-worker topology for a problem that could be solved by a single agent with better prompting.

What happens: You've now created 5 agents instead of 1. Latency increases (multiple calls to LLM). Cost increases (5x the API calls). Complexity increases (need to manage state, orchestration, failure modes across agents). Debugging becomes harder (is the bug in the orchestrator or worker 3?). You get the same result as a well-prompted single agent, but with 3-5x the latency and cost.

How to avoid: Start simple. Single agent. Good prompt. Measure performance. If the agent is struggling because of task complexity, *then* decompose into multi-agent. Not before. Most problems don't actually need multi-agent; they need better prompting or better tools.

---

## References

**Related skills in this marketplace:**
- `prompt-engineering-for-pms` — How to write effective prompts for agentic systems.
- `agent-guardrails-governance` — Deep dive into safety, compliance, and risk mitigation.
- `cost-modeling-agentic-ai` — Financial modeling for multi-agent systems at scale.
- `evaluating-agentic-outputs` — How to measure agent quality, build evaluation frameworks.
- `building-agent-tools-apis` — Designing tool interfaces that agents can rely on.

**External references:**
- **Anthropic's Building Effective Agents** (https://anthropic.com/research/building-effective-agents) — Foundational framework on agent patterns and when to use them.
- **OpenAI's Agent Patterns** (https://platform.openai.com/docs/guides/agents) — Complementary perspective on topology patterns.
- **NIST AI Risk Management Framework** (https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf) — Regulatory context for agentic AI in enterprise.
- **Lilian Weng's Agent Survey** (https://lilianweng.github.io/posts/2023-06-23-agent/) — Comprehensive taxonomy of agent architectures.
- **Andrew Ng on Agentic AI** (https://www.deeplearning.ai/the-batch/issue-242/) — Why agentic patterns matter for product.

---

**Last updated:** April 2025  
**Author context:** Saquib Jawed, Director of AI Product Management at Zycus. This skill reflects 5+ years shipping agentic systems in enterprise Source-to-Pay platforms. Every pitfall, every pattern, every cost number reflects real production decisions.
