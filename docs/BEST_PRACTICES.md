# 📚 Best Practices Guide

**How to use these analytics tools effectively in your CS organization**

---

## Universal Best Practices

### 1. Data Quality First

```
❌ MISTAKE: Use incomplete data
✅ BEST PRACTICE: 
   □ Validate data before calculating
   □ Remove duplicates
   □ Fill missing critical fields
   □ Standardize date formats
   □ Spot-check accuracy
   
IMPACT: Bad data = bad decisions
        Clean data = actionable insights
```

### 2. Start Simple, Expand Over Time

```
MONTH 1: Master ONE tool
├─ Choose: Health Score OR NRR
├─ Get comfortable with it
├─ Validate against reality
└─ Build team understanding

MONTH 2: Add second tool
├─ Health Score + NRR together
├─ See connections between them
└─ Improve decision-making

MONTH 3+: Add remaining tools
├─ Segmentation
├─ Churn analysis
├─ Escalation tracking
└─ Fully integrated system

IMPACT: Gradual adoption = sustainability
```

### 3. Validate Early, Validate Often

```
FIRST RUN: Reality check
□ Do health scores match your intuition?
□ Is NRR close to what finance calculates?
□ Do at-risk customers feel right?
□ Do segments make sense?

If results are way off:
1. Check your data
2. Review your formulas
3. Adjust weights/thresholds
4. Calculate again
5. Repeat until realistic

GOLDEN RULE: Trust but verify.
             Numbers should match your reality.
```

### 4. Review Weekly, Report Monthly

```
WEEKLY TEAM SYNC (30 min):
├─ Review top 5 at-risk customers
├─ Discuss interventions
├─ Update on actions taken
├─ Identify blockers
└─ Celebrate wins

MONTHLY LEADERSHIP REVIEW (1 hour):
├─ NRR performance
├─ Churn trends
├─ Health score distribution
├─ Segment performance
├─ Board-ready metrics

QUARTERLY BOARD PRESENTATION (15 min):
├─ NRR vs target
├─ YoY comparisons
├─ Churn improvement
├─ Business impact ($)
└─ Strategic outlook
```

### 5. Take Action, Don't Just Report

```
❌ ANTI-PATTERN:
   Calculate metrics → Write report → File away

✅ BEST PRACTICE:
   Calculate metrics → Identify action items → Assign owners → Track outcomes → Measure impact

EXAMPLE:
   Health Score: 35 (Critical)
   ↓
   Action: Schedule executive call
   Owner: VP of CS
   Deadline: 48 hours
   Goal: Understand needs, offer solution
   Success: Positive call, customer stays
```

---

## Tool-Specific Best Practices

### Health Score (TOOL_1)

#### 1. Use Realistic Thresholds

```
Default weights (30-20-25-25) work for most companies.

BUT - Adjust based on YOUR business:

High-Touch SaaS (e.g., enterprise):
├─ Support Sentiment: 40% (very important)
├─ Feature Adoption: 25%
├─ Payment Health: 20%
└─ Velocity: 15%

Self-Service SaaS (e.g., SMB):
├─ Feature Adoption: 50% (most important)
├─ Velocity: 25%
├─ Support Sentiment: 15%
└─ Payment Health: 10%

IMPACT: Calibrated weights = predictive accuracy
```

#### 2. Predictive Power Matters

```
✅ BEST PRACTICE: 
   Health Score should predict churn 60-90 days early

VALIDATION:
1. Calculate health scores TODAY
2. Wait 60 days
3. Compare to actual churn
4. If match: weights are good
5. If don't match: adjust weights

EXAMPLE:
   If low health scores DON'T predict churn
   → Maybe support satisfaction isn't weighted enough
   → Increase weight and recalculate
```

#### 3. Investigate Low Scores

```
❌ MISTAKE: See low score → Add to churn list → Move on

✅ BEST PRACTICE:
   See low score → Understand WHY → Tailor intervention

Example: Score = 45 (Critical)

Breakdown:
├─ Adoption: 20 (very low)
├─ Sentiment: 80 (good)
├─ Payment: 100 (perfect)
└─ Velocity: 40 (declining)

Diagnosis:
   NOT using product (adoption)
   BUT happy with support
   AND pays on time
   
Intervention:
   NOT: "You're at risk, we need to talk"
   BUT: "Let's discuss adoption blockers"
   
IMPACT: Correct diagnosis = better outcomes
```

#### 4. Monthly Recalculation Required

```
❌ STATIC: Calculate once, use forever

✅ BEST PRACTICE: Recalculate monthly with fresh data

Timeline:
├─ 1st of month: Extract previous month data
├─ 2nd: Validate data quality
├─ 3rd: Recalculate all scores
├─ 4th: Review results, prioritize actions
├─ 5th-30th: Execute interventions

IMPACT: Catching problems early means saving customers
```

---

### NRR Calculator (TOOL_2) - PRIMARY METRIC

#### 1. Understand Your NRR First

```
Before taking action, know YOUR number:

Calculate for last 12 months:
├─ January NRR
├─ February NRR
├─ March NRR
├─ ... etc
└─ 12-month average

This is your BASELINE.

Then set targets:
├─ If NRR < 100%: Get to 100% first (emergency)
├─ If NRR 100-110%: Target 115% (growth mode)
├─ If NRR > 115%: Maintain, focus on scaling

IMPACT: Can't improve what you don't understand
```

#### 2. Track Drivers Separately

```
NRR = (Expansion - Churn) / Beginning

Track these SEPARATELY:

├─ CHURN REDUCTION
│  Goal: Lower monthly churn rate
│  Tactic: Improve health scores, interventions
│  Quick win: Save 3-5 at-risk customers
│
└─ EXPANSION GROWTH
   Goal: Increase upsell/cross-sell
   Tactic: Identify high-value customers, educate on features
   Quick win: 3 expansion conversations this month

Most companies can improve BOTH:
├─ Reduce churn by 2-3% → +3-4pp NRR
└─ Grow expansion by 2-3% → +3-4pp NRR
   = 6-8pp NRR improvement possible

IMPACT: Focused efforts move the needle
```

#### 3. Cohort Analysis for Deep Insights

```
Don't just look at overall NRR.

Break it down by COHORT:

By Signup Month:
├─ Jan 2023 cohort: 95% (leaky)
├─ Feb 2023 cohort: 105% (good)
├─ Mar 2023 cohort: 120% (excellent)
└─ Pattern: Improving product = better retention

By Tier:
├─ SMB: 90% (problem)
├─ Mid-Market: 110% (healthy)
├─ Enterprise: 125% (excellent)
└─ Action: Fix SMB retention

By Industry:
├─ Tech: 115% (good)
├─ Finance: 95% (needs work)
└─ Action: Review finance customer needs

IMPACT: Cohort analysis reveals specific issues
```

#### 4. Board Reporting

```
WHAT TO SHOW BOARD:

Month:    Jan   Feb   Mar   Apr   May   Jun
NRR:      105%  107%  109%  108%  110%  112%
Trend:    ↗     ↗     ↗     ↓     ↗     ↗

Key insight: "NRR improving trend, Feb dip investigated and addressed"

What they care about:
✓ Direction (improving? declining?)
✓ Benchmark (vs industry)
✓ Stability (volatile? consistent?)
✓ Business impact ($)

What to avoid:
✗ "We calculated NRR"
✗ Complex formulas
✗ Too many caveats
✗ Unclear conclusions

IMPACT: Clear communication builds confidence
```

---

### Churn & Retention (TOOL_3)

#### 1. Cohort Tables Tell Stories

```
Look at cohort SHAPE to understand what changed:

HEALTHY SHAPE:
Month 1: 100%
Month 2: 92%
Month 3: 85%
Month 4: 80%
→ Smooth decline = normal, expected churn

CLIFF CHURN:
Month 1: 100%
Month 2: 50%
Month 3: 48%
Month 4: 46%
→ Huge drop at month 2 = onboarding problem
→ Action: Improve onboarding

DECLINING PRODUCT:
Jan cohort: 100% → 95% → 85% → 70%
Feb cohort: 100% → 93% → 80% → 60%
Mar cohort: 100% → 90% → 75% → 50%
→ Each cohort worse = product getting worse
→ Action: Product quality emergency

IMPROVING PRODUCT:
Jan cohort: 100% → 85% → 65%
Feb cohort: 100% → 88% → 70%
Mar cohort: 100% → 91% → 75%
→ Each cohort better = improvements working
→ Action: Keep doing what you're doing

IMPACT: Shape = diagnosis + direction
```

#### 2. CLV Determines Budget

```
CLV tells you HOW MUCH to spend on acquisition.

Your payback period dictates your budget:

Payback 6 months:
├─ Can spend aggressively on sales
├─ Should grow sales 30-50%/year
└─ Profitable growth possible

Payback 12 months:
├─ Can grow sales moderately
├─ Need 18-24 month horizon
└─ Sustainable but limited

Payback 24+ months:
├─ Can't spend much on acquisition
├─ Need to fix retention first
├─ Unsustainable current model
└─ Change pricing or reduce CAC

ACTION:
1. Calculate CLV and payback
2. Compare to CAC spending
3. Adjust budget accordingly
4. Track improvement

IMPACT: Data-driven spending decisions
```

#### 3. LTV:CAC Ratio Rule of Thumb

```
LTV:CAC = 3:1 is industry standard (healthy)
LTV:CAC = 5:1 is excellent (grow aggressively)
LTV:CAC = 1:1 is bad (losing money)

How to improve:

Option A: Increase LTV
├─ Improve retention (reduce churn)
├─ Increase expansion revenue
├─ Extend customer lifespan
└─ Time: 6 months to see impact

Option B: Decrease CAC
├─ Improve sales efficiency
├─ Use referral program
├─ Focus on high-fit customers
└─ Time: 1-3 months to see impact

BEST STRATEGY: Do both simultaneously
├─ Month 1-2: Reduce CAC
├─ Month 3-6: Improve retention
└─ 6-month result: 50% improvement possible

IMPACT: Focused unit economics improvement
```

---

### Customer Segmentation (TOOL_4)

#### 1. Segment-Specific Playbooks Are Essential

```
DON'T use same playbook for all customers.

DO create segment-specific playbooks:

CHAMPIONS (5% customers, 30% revenue):
├─ Cadence: Monthly
├─ Format: Executive business review
├─ Agenda: Strategic partnership discussion
├─ Ask: Advisory board, case study, referral
├─ Success: Loyalty, expansion, advocacy

LOYAL CUSTOMERS (10% customers, 40% revenue):
├─ Cadence: Quarterly
├─ Format: Business review + feature training
├─ Agenda: Achievement recognition + growth plan
├─ Ask: Expansion opportunity identification
├─ Success: Expansion revenue, retention

AT-RISK (15% customers, 20% revenue):
├─ Cadence: Monthly+ (intensive)
├─ Format: Health check calls
├─ Agenda: Problem diagnosis + solution offering
├─ Ask: What would make it better?
├─ Success: Stabilization, retention

LOST (10% customers, 10% revenue):
├─ Cadence: None (except special circumstances)
├─ Accept: These don't get attention
└─ Action: Accept loss, focus on others

IMPACT: Right playbook = right retention
```

#### 2. Resource Allocation by Segment

```
CS TEAM OF 10 PEOPLE ALLOCATION:

HIGH-VALUE accounts (2 FTE):
├─ 1:5 account manager ratio
├─ Deep relationship building
├─ Executive alignment
└─ Max ROI focus

GROWTH accounts (3.5 FTE):
├─ 1:15 manager ratio
├─ Playbook-based engagement
├─ Expansion focus
└─ Good balance of service + efficiency

MAINTENANCE accounts (3 FTE):
├─ 1:50+ manager ratio
├─ Self-service + group training
├─ Community support
└─ Cost-efficient model

PROBLEM accounts (1.5 FTE):
├─ Triage only
├─ Managed service
├─ Minimize resource burn
└─ Exit when appropriate

IMPACT: 30% of team gets 70% of revenue
        Don't waste resources on bottom 10%
```

#### 3. Segment Dynamics (They Change)

```
CUSTOMERS MOVE BETWEEN SEGMENTS:

Champions → Loyal:
├─ Expansion declined
├─ Engagement down
├─ Adjust playbook, add focus

At-Risk → Lost:
├─ Didn't respond to outreach
├─ Moving to competitor
├─ Accept and move on

Growth → Champions:
├─ Expansion successful
├─ Become advocates
├─ Promote to VIP playbook

MONTHLY SEGMENT REVIEW:
□ Who moved up? Promote playbook
□ Who moved down? Increase focus
□ Who stayed? Monitor and maintain

IMPACT: Dynamic management = better outcomes
```

---

### Escalation Tracker (TOOL_5)

#### 1. SLA Compliance is Team Reputation

```
SLA = Service Level Agreement
Your promise: "We will respond by this time"

Industry benchmarks:
├─ 95%+ SLA compliance = excellent
├─ 90-95% = acceptable
├─ 80-90% = needs improvement
├─ <80% = serious problem

Your customers notice:
✓ Fast response = trust = loyalty
✗ Slow response = frustration = churn

ACTION:
1. Set realistic SLAs for your team
2. Track daily
3. Review weekly
4. If trending < 90%, hire immediately
5. Maintain 95%+ always

IMPACT: SLA compliance = customer satisfaction
```

#### 2. Escalation Patterns Reveal Problems

```
Track WHERE escalations happen:

By Category:
├─ Billing: 40% → Fix: Billing system improvements
├─ Integration: 30% → Fix: Documentation + training
├─ Performance: 20% → Fix: Product optimization
└─ Feature requests: 10% → Fix: Roadmap communication

By Customer:
├─ 20% from 5 customers → Fix: Dedicated success plans
├─ 30% from new customers → Fix: Better onboarding
└─ 50% from churned customers → Fix: Earlier intervention

ACTION:
1. Categorize each escalation
2. Look for patterns (80/20 rule)
3. Address top 3 issues
4. Measure improvement next month

IMPACT: Fix root causes, not symptoms
```

#### 3. At-Risk Tickets Need Proactive Management

```
PROACTIVE APPROACH:

Every day, identify tickets approaching SLA:
├─ < 1 hour to SLA deadline
├─ Review priority + complexity
├─ Consider escalation BEFORE miss
├─ Communicate with customer
└─ Set new expectation if needed

EXAMPLE:
P1 ticket created 3h 45min ago (15 min to SLA)
├─ Status: In progress, but complex issue
├─ Escalate to engineering NOW (before miss)
├─ Notify customer: "This is complex, escalating to our expert"
├─ Extended SLA: "We'll have update in 1 hour"
└─ Result: SLA met, customer happy, issue solved

REACTIVE APPROACH (Bad):
SLA missed → Customer angry → Scramble → Damage control

PROACTIVE APPROACH (Good):
Approaching SLA → Escalate early → Set expectations → Problem solved

IMPACT: Proactive = fewer angry customers
```

#### 4. SLA Threshold by Customer Tier

```
Not all customers need same SLA:

ENTERPRISE SLA:
├─ P1: 30 min response, 2 hr resolution
├─ P2: 2 hr response, 8 hr resolution
├─ P3: 4 hr response, 24 hr resolution
└─ P4: 8 hr response, 5-day resolution

MID-MARKET SLA:
├─ P1: 1 hr response, 4 hr resolution
├─ P2: 4 hr response, 24 hr resolution
├─ P3: 8 hr response, 72 hr resolution
└─ P4: 24 hr response, 30-day resolution

SMB SLA:
├─ P1: 4 hr response, 24 hr resolution
├─ P2: 8 hr response, 48 hr resolution
├─ P3: 24 hr response, 5-day resolution
└─ P4: 48 hr response, best effort

RATIONALE:
Enterprise = higher CAC = more resources
SMB = lower CAC = self-service model

IMPACT: Sustainable SLAs for your team
```

---

## Cross-Tool Integration

### How the Tools Work Together

```
EXAMPLE: A customer is churning

STEP 1: Health Score flags it
   "Acme Corp - Score 35 (Critical)"
   ├─ Adoption dropped to 20%
   ├─ No support tickets in 60 days
   └─ 40% engagement decline

STEP 2: Segmentation tells you how to handle it
   "Acme Corp - Growth Segment"
   ├─ Revenue: $3,000/mo
   ├─ Growth potential: High
   └─ Playbook: Proactive engagement + feature training

STEP 3: NRR context shows business impact
   "If Acme churns this month, NRR drops from 110% to 107%"
   └─ Priority: HIGH (impacts team metric)

STEP 4: Churn analysis shows pattern
   "Cohort Dec 2023 showing 15% churn this month (vs 8% expected)"
   ├─ These customers all have low adoption
   └─ Fix: Improve onboarding for Dec cohort

STEP 5: Escalation status checks support
   "No support tickets = disengagement signal"
   ├─ They're not asking for help
   ├─ They might be investigating alternatives
   └─ Need proactive outreach (not reactive)

ACTION TAKEN:
1. VP CS schedules call with Acme executive
2. Assess their needs and challenges
3. Offer training, optimization, or custom solution
4. Document in Salesforce with health score + segment
5. Escalate if needed
6. Check back in 30 days

RESULT (60 days later):
✓ Acme renewed
✓ Expanded by $1,000
✓ Health score improved to 75 (healthy)
✓ NRR impact: Avoided $3K churn + $1K expansion = +$4K
```

---

## Measurement & Continuous Improvement

### Monthly Review Agenda (1 hour)

```
Time: 0-15 min | HEALTH SCORE REVIEW
├─ How many customers are at-risk? (target: <15%)
├─ Did last month's interventions work?
├─ Which 5 customers need priority this month?
└─ Action items: 3-5 specific customer calls

Time: 15-30 min | NRR ANALYSIS
├─ What was our NRR this month?
├─ How does it compare to target?
├─ What drove expansion? (top 5 upsells)
├─ What drove churn? (top 5 churn reasons)
└─ Action items: Address churn reasons

Time: 30-45 min | SEGMENTATION & RETENTION
├─ How did each segment perform?
├─ Any segment showing unexpected churn?
├─ Cohort retention - any new issues?
├─ Are playbooks working?
└─ Action items: Adjust playbooks if needed

Time: 45-60 min | OPERATIONS & PLANNING
├─ SLA compliance: On track?
├─ Escalation patterns: Any surprises?
├─ Team capacity: Sustainable?
├─ Next month focus: What to tackle?
└─ Action items: Resource planning, prioritization
```

### Success Metrics (Track These)

```
LAGGING INDICATORS (Results):
Monthly:
├─ NRR % (target: >110%)
├─ Customer churn rate (target: <5%)
├─ Expansion revenue (target: +10% MoM)
├─ Health score distribution (target: 60% healthy)
└─ SLA compliance (target: >95%)

Quarterly:
├─ YoY NRR improvement (target: +10pp)
├─ Cohort retention at 12mo (target: 85%+)
├─ CAC payback period (target: <12 months)
└─ CLV improvement (target: +15%)

LEADING INDICATORS (Activity):
Weekly:
├─ At-risk customers contacted (target: 100% of criticals)
├─ Expansion conversations (target: 5+ per week)
├─ SLA escalations (target: <10% of tickets)
├─ Health score data freshness (target: current)

Monthly:
├─ Customer success reviews held (target: 1 per champion)
├─ Expansion closed (target: +$X)
├─ Intervention success rate (target: 80%+)
└─ Team sentiment score (target: 8/10+)
```

---

## Common Pitfalls to Avoid

```
PITFALL 1: Analyzing without acting
├─ Problem: Calculate metrics but don't use them
├─ Impact: No business improvement
└─ Solution: For every metric, have 3 action items

PITFALL 2: Setting unrealistic thresholds
├─ Problem: Health score weights don't predict churn
├─ Impact: Team ignores tool (loses trust)
└─ Solution: Validate with historical data first

PITFALL 3: One-size-fits-all approach
├─ Problem: Same playbook for all customers
├─ Impact: Resource waste, poor outcomes
└─ Solution: Create segment-specific playbooks

PITFALL 4: Not communicating results
├─ Problem: Team doesn't know why metrics changed
├─ Impact: Confusion, misalignment
└─ Solution: Explain drivers, not just numbers

PITFALL 5: Static metrics
├─ Problem: Calculate once, never update
├─ Impact: Data becomes stale, decisions are wrong
└─ Solution: Monthly recalculation, weekly review

PITFALL 6: Ignoring data quality
├─ Problem: Garbage in = garbage out
├─ Impact: Wrong conclusions, wrong actions
└─ Solution: Validate data BEFORE calculating
```

---

## Final Wisdom

```
These tools are INPUTS to decision-making, not the decision itself.

Use analytics to:
✓ Inform decisions
✓ Prioritize efforts
✓ Measure impact
✓ Improve continuously

But remember:
✓ Customer relationships matter most
✓ Judgment + data = best outcomes
✓ Numbers tell what happened, not what to do
✓ Your team's expertise combined with data = magic

FORMULA FOR SUCCESS:
   Data (metrics) + Judgment (intuition) + Action (execution)
   = Results (growth + churn reduction)
```

---

*For tool-specific best practices, see each TOOL_X.md file.*

*For integration examples, see ARCHITECTURE.md.*
