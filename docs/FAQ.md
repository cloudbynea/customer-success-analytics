# ❓ FAQ - Frequently Asked Questions

**Common questions and clear answers**

---

## General Questions

### Q: What do I need to get started?
**A:** Minimum requirements:
- Customer list (names, IDs)
- Monthly revenue per customer (MRR)
- Customer signup dates
- Excel or Google Sheets (or Python)

Optional but recommended:
- Feature usage data
- Support ticket data
- Payment history
- Customer industry/size

No advanced technical skills needed.

---

### Q: How long does setup take?
**A:** 
- **Quick start**: 2-4 weeks part-time
- **Full implementation**: 4-8 weeks
- **Ongoing maintenance**: 4-6 hours/month

Most teams are operational and seeing results within 30 days.

---

### Q: What's the cost?
**A:** 
- **Portfolio**: $0 (open-source, MIT license)
- **Tools needed**: Depends on your choice
  - Spreadsheet: Free (Excel/Google Sheets)
  - Database: $50-500/month (optional)
  - Dashboards: Free-$1000/month (optional)
  - Support system: Likely already have

No licensing fees for this portfolio.

---

### Q: Can I customize these tools?
**A:** Absolutely! 

You can customize:
- ✅ Health score weights (adjust 30-20-25-25)
- ✅ SLA thresholds (P1-P4 timelines)
- ✅ Segment definitions (RFM or profit-based)
- ✅ Risk thresholds (what's "critical" vs "at-risk")
- ✅ Industry/company-specific fields

All formulas are documented and changeable.

---

### Q: Does this work for our company size?
**A:**
| Size | Tool | Effort | Scaling |
|------|------|--------|---------|
| <50 customers | Excel | 2-4 hrs/mo | Start small |
| 50-500 | Excel or SQL | 4-6 hrs/mo | Good |
| 500-5,000 | SQL + Python | 6-8 hrs/mo | Very good |
| 5,000-50,000 | Warehouse | 8-10 hrs/mo | Excellent |
| 50,000+ | ML + Real-time | 1-2 FTE | Enterprise |

We've designed this to work at any scale.

---

### Q: How often should I recalculate metrics?
**A:**
- **Health Scores**: Monthly (minimum), weekly (ideal)
- **NRR**: Monthly (required)
- **Churn/Retention**: Monthly
- **Segments**: Monthly (RFM) or quarterly (profit-based)
- **SLA Tracking**: Weekly (real-time ideal)

Set monthly calendar reminders for core metrics.

---

### Q: Will this replace our CRM?
**A:** No. This portfolio **complements** your CRM.

- **CRM** (Salesforce/HubSpot): Operations, contacts, deals
- **This portfolio**: Analytics, metrics, strategy

Integration: Push health scores and segments back to CRM.

---

## Health Score Questions

### Q: What does a health score of 65 mean?
**A:** Score of 65 = "At-Risk" ⚠️

It means:
- Not healthy enough to ignore
- But not critical yet
- Needs attention within 1-2 weeks
- Schedule health check call

The score is on a 0-100 scale, with 80+ being healthy.

---

### Q: Why is my health score so low when the customer is paying?
**A:** 
Health score considers MORE than just payment:
- ❌ They might have low feature adoption
- ❌ No support tickets = disengagement
- ❌ Usage declining = churn signal
- ❌ Could indicate they're exploring alternatives

**Action**: Schedule call to understand real needs.

---

### Q: Can I use health scores for upsell?
**A:** 

High health scores (80+) are actually BEST for upsell:
- ✅ Engaged with product
- ✅ Happy with support
- ✅ Paying on time
- ✅ Using features actively

**Strategy**: 
- At-risk (60-79) → Fix problems first
- Healthy (80+) → Upsell opportunity

---

### Q: How do I improve a low health score?
**A:** Depends on what's low:

| Low Component | Action |
|---|---|
| **Adoption** | Feature training, use cases, success stories |
| **Sentiment** | Support quality review, product issues fix |
| **Payment** | Billing discussion, payment plan options |
| **Velocity** | Check for roadblocks, engagement campaign |

Diagnose first, then treat.

---

### Q: The scores don't match my intuition. What's wrong?
**A:** Three possibilities:

1. **Your intuition could be wrong**
   - Review the scoring, you might see patterns you missed

2. **Data quality issue**
   - Check adoption/support/payment data for accuracy
   - Validate with customer list

3. **Thresholds need adjustment**
   - Adjust weights if adoption is more important for your business
   - Test weights against historical churn

Start by validating data quality first.

---

## NRR Questions

### Q: What NRR should we target?
**A:**
- **Minimum**: 100% (no decline)
- **Healthy**: 110%
- **Growth**: 115%+
- **Exceptional**: 120%+

Set realistic targets:
- Improving from 95% → 105% = major win
- From 105% → 115% = significant effort
- From 115% → 125% = elite company

---

### Q: How do we improve NRR?
**A:** Two levers:

**Lever 1: Reduce Churn** (quick wins)
- Health scores + interventions
- Target: 3-5% monthly reduction
- Timeline: 3-6 months

**Lever 2: Increase Expansion** (ongoing)
- Identify high-value customers
- Create expansion playbooks
- Target: +2-3% monthly growth
- Timeline: Ongoing

**Best approach**: Do both simultaneously.

---

### Q: How does our NRR compare to industry?
**A:**
| Industry | Typical NRR |
|----------|---|
| Enterprise SaaS | 115-130% |
| Mid-market SaaS | 105-115% |
| SMB SaaS | 95-110% |
| Horizontal SaaS | 100-110% |
| Vertical SaaS | 110-125% |

Find your industry benchmark, set target 10pp above.

---

### Q: Our NRR dropped. What do we do?
**A:** 
**Immediate (today)**:
1. Identify top 5 churned customers
2. Understand why they left
3. Check if it's pattern or one-off

**Week 1**:
1. Analyze churn drivers
2. Review health scores (were they flagged?)
3. Check expansion pipeline

**Week 2+**:
1. Implement churn prevention (health scores)
2. Launch win-back campaign for recent churners
3. Accelerate expansion efforts

**Goal**: Stabilize, then improve.

---

## Churn & Retention Questions

### Q: What CLV should we target?
**A:** At minimum, CLV should be 3x CAC.

| Metric | What It Means |
|--------|---|
| CLV = 2x CAC | Tight, limited investment room |
| CLV = 3x CAC | Healthy, sustainable growth |
| CLV = 5x CAC | Excellent, can invest aggressively |

If CLV < 2x CAC: Fix pricing or reduce CAC immediately.

---

### Q: How do we improve payback period?
**A:** 
**Fastest wins (1-3 months)**:
- Reduce CAC (optimize sales process)
- Tighten sales cycle
- Focus on self-serve customers

**Medium-term (3-6 months)**:
- Increase ARPU (higher pricing, bundling)
- Improve margins (automation)
- Reduce support costs

**Long-term (6-12 months)**:
- Improve retention (churn reduction)
- Increase expansion revenue
- Customer lifespan extension

Target: Get to <12 month payback.

---

### Q: Our CLV is negative. What's wrong?
**A:** 
Something is seriously broken. Check:

1. **Pricing too low**: Are you making any margin?
2. **CAC too high**: Sales spend unsustainable?
3. **Churn too high**: Customers leaving in 3-6 months?
4. **Support too expensive**: Cost per customer too high?

**Action**: Emergency cost review + pricing review.

This is unsustainable and needs immediate fix.

---

### Q: What does payback period mean practically?
**A:**
**12-month payback example**:
- You spend $1,200 to acquire customer
- Customer generates $100/month profit
- After 12 months, you've recovered $1,200
- Month 13+ = pure profit

**Implication**:
- Payback = how long before profitable
- Shorter payback = faster path to profitability
- Need 2-3 year retention minimum

---

## Segmentation Questions

### Q: What's the difference between RFM and Profit-based segments?
**A:**
| Aspect | RFM | Profit-Based |
|--------|-----|---|
| **What it measures** | Customer behavior | Business value |
| **Recency** | When did they last buy? | N/A |
| **Frequency** | How often do they buy? | N/A |
| **Monetary** | How much do they spend? | Yes (annual value) |
| **Growth** | Not included | Growth potential |
| **Use case** | Quick segmentation | Strategic allocation |
| **Segments** | 9 (Champions to Lost) | 4 (High-value to Problem) |

**Recommendation**: Use BOTH for complete picture.

---

### Q: Which segment should we focus on first?
**A:**
**For revenue protection**: At-Risk + At-Extreme-Risk
- These are churn risks
- Can save $X with intervention
- Quick wins in 30-60 days

**For revenue growth**: Champions + Loyal
- Already engaged
- Ready for expansion
- Highest conversion rates

**For efficiency**: Growth segment
- Medium value
- High growth potential
- Best ROI on CS investment

**Recommended sequence**:
1. Month 1: Save at-risk customers
2. Month 2: Expand in champions/growth
3. Month 3: Scale to all segments

---

### Q: How do segments change over time?
**A:** 
Customers move between segments monthly:

**Promotion** (good):
- At-Risk → At-Risk: Stabilized
- At-Risk → Promising: Recovering
- Loyal → Champion: Expanding

**Demotion** (bad):
- Champion → Loyal: Engagement down
- Loyal → At-Risk: Issue emerged
- At-Risk → Lost: Churned

**Monitor this monthly**:
```
Champions: 25 → 22 (3 demoted) ⚠️
Loyal: 50 → 52 (2 promoted from At-Risk) ✅
At-Risk: 75 → 78 (3 new) ⚠️
```

Investigate big changes.

---

### Q: Do we need different SLAs by segment?
**A:** 
**Practical approach**:

| Segment | SLA | Reason |
|---------|-----|--------|
| **Champion** | P1: 30min, P2: 2hr | Highest value, executives involved |
| **Loyal** | P1: 1hr, P2: 4hr | Tier 1 support, important |
| **Growth** | P1: 2hr, P2: 8hr | Standard service level |
| **At-Risk** | P1: 4hr, P2: 24hr | Don't over-invest |
| **Lost** | P1: 24hr | Minimal support |

**Reality**: Most teams use uniform SLAs.
**Best practice**: Adjust by customer tier if possible.

---

## SLA & Escalation Questions

### Q: What SLAs should we set?
**A:**

**Based on Priority**:
```
P1 (Critical):
├─ Response: 1-4 hours (your choice)
├─ Resolution: 4-24 hours
└─ Usually requires engineering
  
P2 (High):
├─ Response: 4-8 hours
├─ Resolution: 24-48 hours
└─ CS team can handle

P3 (Medium):
├─ Response: 8-24 hours
├─ Resolution: 48-72 hours
└─ Can wait, but not long

P4 (Low):
├─ Response: 24-48 hours
├─ Resolution: Best effort (30+ days)
└─ Feature requests, nice-to-haves
```

**Set realistic SLAs your team can hit >95%**.

---

### Q: We can't hit 95% SLA. What do we do?
**A:** 

**Option 1: Relax SLAs** (compromise)
- Make targets realistic
- P1: 4hr instead of 1hr
- P2: 24hr instead of 4hr
- Then hit 95% new targets

**Option 2: Hire more** (investment)
- 95% compliance requires capacity
- Generally need +25-30% headcount
- More sustainable long-term

**Option 3: Hybrid** (smart)
- Relax lower-priority SLAs
- Tighten P1 SLAs
- Automate common issues
- Result: Sustainable + good service

**Recommended**: Option 3 for most teams.

---

### Q: How do we reduce escalations?
**A:** 
Track escalation patterns first:

```
By Category:
├─ Billing: 40% → Fix: Improve docs, self-serve
├─ Integration: 30% → Fix: API docs, sandbox
├─ Feature: 20% → Fix: In-app help, training
└─ Bug: 10% → Fix: Product quality

By Customer:
├─ 5 customers = 30% of escalations → VIP program
├─ New customers = 25% → Better onboarding
```

**Top strategies**:
1. **Self-serve**: Knowledge base, FAQ, docs
2. **Training**: Feature training, best practices
3. **Process**: Improve triage, faster responses
4. **Product**: Fix bugs, improve usability

---

### Q: How do we know when to escalate?
**A:** 
Escalate when:

1. **Complexity exceeds support skill level**
   - Example: Database corruption → Engineering

2. **Priority level requires it**
   - Example: P1 ticket created → Auto escalate

3. **Customer threatening to churn**
   - Example: "We're canceling if not fixed today" → Manager

4. **SLA deadline approaching**
   - Example: 30 min to P1 SLA → Escalate to expedite

5. **Multiple failed attempts**
   - Example: 3 support attempts haven't worked → Expert review

Document escalations to see patterns.

---

## Integration Questions

### Q: Can we integrate with Salesforce?
**A:** Yes! See ARCHITECTURE.md for details.

**What to push to Salesforce**:
- ✅ Health Score (custom Account field)
- ✅ Segment (RFM, profit-based)
- ✅ Risk Level (Healthy/At-Risk/Critical)
- ✅ Last Health Check date

**Workflows to set up**:
- Alert on health score drop
- Create task for at-risk follow-up
- Route to expansion playbook
- Trigger escalation

**Implementation**: 2-4 hours with admin.

---

### Q: Can we integrate with HubSpot?
**A:** Yes! Similar to Salesforce.

**Company properties**:
- Health score
- Segment
- Churn risk
- NRR %

**Workflows**:
- At-risk escalation sequence
- Upsell workflow trigger
- Win-back campaign

**Implementation**: 2-3 hours with admin.

---

### Q: Can this live in our data warehouse?
**A:** Absolutely. 

**Recommended architecture**:
1. Extract data from source systems (daily)
2. Load into warehouse (Snowflake, BigQuery)
3. Transform with SQL (monthly)
4. Visualize in BI tool (Tableau, Looker)
5. Export metrics to CRM (daily API)

**Effort**: 2-4 weeks for engineering to build.

See ARCHITECTURE.md for SQL examples.

---

## Troubleshooting

### Q: Why do health scores look wrong?
**A:** Check in this order:

1. **Data quality**
   - Is adoption_percent between 0-100? ✓
   - Is CSAT between 1-5? ✓
   - Are dates valid? ✓
   - Any nulls? ✓

2. **Formula check**
   - Weights: 30-20-25-25? ✓
   - All components calculated? ✓
   - Correct threshold conversion? ✓

3. **Reality check**
   - Pick 5 customers you know
   - Do scores match your intuition?
   - If not, weights need adjustment

4. **Validation**
   - Compare low scores to actual churn
   - Do low scores predict churn?
   - If not, weights are wrong

---

### Q: NRR calculation doesn't match finance
**A:** Find the discrepancy:

1. **Timing difference**
   - Are you using same month as finance?
   - Same start/end date?

2. **Definition difference**
   - What counts as "expansion"?
   - What counts as "churn"?
   - Are you including all products?

3. **Data source difference**
   - Are you pulling from same billing system?
   - Same currency/units?
   - Timing of transaction recording?

**Resolution**: Reconcile definition with finance, align calculations.

---

### Q: Segments don't look right
**A:** 
1. Check RFM thresholds
   - Are quartiles calculated correctly?
   - Any ties affecting segmentation?

2. Check profit thresholds
   - Is median annual_value calculated right?
   - Growth score formula correct?

3. Validate manually
   - Pick 10 customers
   - Calculate their segment by hand
   - See if it matches

4. Adjust thresholds if needed
   - Segments should feel realistic
   - Should align with your strategy

---

### Q: SLA compliance trending down
**A:**

1. **Check ticket volume**
   - Did tickets increase? (less capacity)
   - New product causing more issues?

2. **Check staffing**
   - Any recent resignations?
   - Onboarding new people?

3. **Check ticket complexity**
   - Are tickets taking longer?
   - More escalations needed?

4. **Check process**
   - Are there bottlenecks?
   - Is triage working?

**Action**: Hire, process improvement, or relax SLAs.

---

## Advanced Questions

### Q: Can we use this for AI-powered recommendations?
**A:** 
Yes! Future version (planned for 2024) will include:
- AI-powered churn intervention suggestions
- Automated expansion opportunities
- Smart escalation routing
- Predictive analytics

For now, recommendations are manual based on metrics.

---

### Q: How do we handle multi-product customers?
**A:** 
Track separately or combined:

**Option 1: Separate by product**
- Customer X - Product A: $5K
- Customer X - Product B: $3K
- Calculate metrics per product

**Option 2: Combine**
- Customer X - Combined: $8K
- Single health score for customer
- More practical for most

**Recommendation**: Option 2 for simplicity.

---

### Q: How do we handle multi-currency customers?
**A:** 
Convert to base currency:

1. Define base currency (e.g., USD)
2. Use monthly exchange rates
3. Convert all revenues to USD
4. Calculate metrics in USD
5. Report back in local currency if needed

**Setup**: 1-2 hours for conversion rules.

---

### Q: How long does customer data history need to go back?
**A:** 

**Minimum**: 6 months
- Calculate health scores
- See trends
- Validate thresholds

**Recommended**: 12 months
- 12-month cohort retention
- Yearly comparisons
- Better pattern detection

**Ideal**: 24+ months
- Long-term trends
- Product changes impact
- Seasonal patterns

Start with what you have; collect more over time.

---

## Getting Help

### Q: Where do I go if I have more questions?
**A:** 

**In this portfolio**:
- See README.md for overview
- See specific TOOL_X.md for tool questions
- See ARCHITECTURE.md for technical details
- See BEST_PRACTICES.md for strategy

**Community**:
- GitHub Issues: Bug reports, features
- GitHub Discussions: Q&A
- Check CONTRIBUTING.md for how to help

**Your team**:
- Don't be afraid to ask colleagues
- Create internal wiki for your customizations
- Document your process

---

### Q: Can we contribute improvements?
**A:** 
Yes! We welcome contributions:

1. **Report bugs**: GitHub Issues
2. **Suggest features**: GitHub Issues  
3. **Share improvements**: Pull Requests
4. **Help others**: GitHub Discussions

See CONTRIBUTING.md for detailed guidelines.

---

## Summary

**Most important questions answered**:
- ✅ Setup time: 2-4 weeks
- ✅ Cost: Free (open-source)
- ✅ Customizable: Yes, all of it
- ✅ Scalable: Works at any size
- ✅ Integration: Salesforce, HubSpot, warehouse
- ✅ Expected ROI: +$1-5M/year
- ✅ Support: Community-driven

**Still have questions?**
Check the specific TOOL files and ARCHITECTURE.md first—most answers are there!

---

*Last updated: 2024-03-15*
*FAQ version: 1.0*
