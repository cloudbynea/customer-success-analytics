# 🏗️ System Architecture

**How the 5 tools work together as one integrated system**

---

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│           CUSTOMER DATA SOURCES                          │
├─────────────────────────────────────────────────────────┤
│ • Salesforce (CRM)                                       │
│ • HubSpot (CRM)                                          │
│ • Stripe/Zuora (Billing)                                 │
│ • Mixpanel/Amplitude (Product Usage)                     │
│ • Support Ticketing (Zendesk, Intercom, etc.)            │
│ • CSV/Excel (Manual data)                                │
└─────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────┐
│         DATA INGESTION & PREPARATION                     │
├─────────────────────────────────────────────────────────┤
│ • Data extraction from sources                           │
│ • Data cleaning and validation                           │
│ • Field mapping to standard schema                       │
│ • Duplicate detection and removal                        │
└─────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────┐
│      ANALYTICS ENGINE (5 INTEGRATED TOOLS)               │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌──────────────────────────────────────────────────┐   │
│  │ 1. HEALTH SCORE ENGINE                           │   │
│  ├──────────────────────────────────────────────────┤   │
│  │ Inputs:                                          │   │
│  │ • Feature adoption %                             │   │
│  │ • Support satisfaction (CSAT)                    │   │
│  │ • Payment health (on-time %)                     │   │
│  │ • Engagement velocity (login growth %)           │   │
│  │                                                  │   │
│  │ Output: Health Score (0-100)                     │   │
│  │ Feeds into: Segmentation, Escalation, NRR       │   │
│  └──────────────────────────────────────────────────┘   │
│                            ↓                             │
│  ┌──────────────────────────────────────────────────┐   │
│  │ 2. NRR CALCULATOR (PRIMARY METRIC)               │   │
│  ├──────────────────────────────────────────────────┤   │
│  │ Inputs:                                          │   │
│  │ • Beginning MRR (month start)                    │   │
│  │ • Expansion MRR (upsells, cross-sells)           │   │
│  │ • Churn MRR (lost customers, downgrades)         │   │
│  │                                                  │   │
│  │ Output: NRR % (e.g., 115%)                       │   │
│  │ Feeds into: Business strategy, Board reporting   │   │
│  └──────────────────────────────────────────────────┘   │
│                            ↓                             │
│  ┌──────────────────────────────────────────────────┐   │
│  │ 3. CHURN & RETENTION ANALYSIS                    │   │
│  ├──────────────────────────────────────────────────┤   │
│  │ Inputs:                                          │   │
│  │ • Customer signup date (cohort)                  │   │
│  │ • Historical MRR by month                        │   │
│  │ • Churn date (if churned)                        │   │
│  │                                                  │   │
│  │ Outputs:                                         │   │
│  │ • Cohort retention curves                        │   │
│  │ • CLV (Customer Lifetime Value)                  │   │
│  │ • Payback period (months)                        │   │
│  │ • LTV:CAC ratio                                  │   │
│  │                                                  │   │
│  │ Feeds into: Pricing, acquisition budget          │   │
│  └──────────────────────────────────────────────────┘   │
│                            ↓                             │
│  ┌──────────────────────────────────────────────────┐   │
│  │ 4. CUSTOMER SEGMENTATION                         │   │
│  ├──────────────────────────────────────────────────┤   │
│  │ Methods:                                         │   │
│  │ A) RFM Analysis                                  │   │
│  │    • Recency (last purchase)                     │   │
│  │    • Frequency (purchase count)                  │   │
│  │    • Monetary (total spent)                      │   │
│  │    → 9 segments (Champions to Lost)              │   │
│  │                                                  │   │
│  │ B) Profit-Based Quadrants                        │   │
│  │    • X-axis: Annual value                        │   │
│  │    • Y-axis: Growth potential                    │   │
│  │    → 4 quadrants (High-Value to Problem)         │   │
│  │                                                  │   │
│  │ Feeds into: CS resource allocation, playbooks    │   │
│  └──────────────────────────────────────────────────┘   │
│                            ↓                             │
│  ┌──────────────────────────────────────────────────┐   │
│  │ 5. ESCALATION TRACKER                            │   │
│  ├──────────────────────────────────────────────────┤   │
│  │ Inputs:                                          │   │
│  │ • Support tickets                                │   │
│  │ • Priority level (P1-P4)                         │   │
│  │ • First response time                            │   │
│  │ • Resolution time                                │   │
│  │                                                  │   │
│  │ Outputs:                                         │   │
│  │ • SLA compliance %                               │   │
│  │ • Response time metrics                          │   │
│  │ • Escalation patterns                            │   │
│  │ • At-risk tickets (near SLA deadline)            │   │
│  │                                                  │   │
│  │ Feeds into: Support team management, quality     │   │
│  └──────────────────────────────────────────────────┘   │
│                                                          │
└─────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────┐
│           OUTPUT & INTEGRATION LAYER                     │
├─────────────────────────────────────────────────────────┤
│ • Dashboards (HTML/Tableau/Looker)                      │
│ • Reports (PDF/Excel)                                   │
│ • API outputs (JSON)                                    │
│ • Alerts (Email/Slack)                                  │
│ • Salesforce updates (custom fields)                    │
│ • HubSpot sync (companies/contacts)                     │
└─────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────┐
│      DOWNSTREAM SYSTEMS & DECISIONS                      │
├─────────────────────────────────────────────────────────┤
│ • CS Team (interventions, resource allocation)          │
│ • Sales Team (expansion opportunities)                  │
│ • Product Team (feature adoption insights)              │
│ • Finance Team (revenue forecasting, unit economics)    │
│ • Executive Team (strategic decisions, board reports)   │
└─────────────────────────────────────────────────────────┘
```

---

## Data Flow Diagram

```
                    MONTHLY DATA CYCLE
                    
START OF MONTH
       ↓
1. EXTRACT DATA
   ├─ Pull MRR from billing system
   ├─ Pull feature usage from product
   ├─ Pull support tickets
   └─ Pull payment records
       ↓
2. CLEAN & VALIDATE
   ├─ Remove duplicates
   ├─ Fill missing values
   ├─ Validate data quality
   └─ Flag errors for review
       ↓
3. RUN CALCULATIONS (5 Tools)
   ├─ Calculate Health Scores → Identify at-risk
   ├─ Calculate NRR → Track business health
   ├─ Analyze Cohorts → Understand retention
   ├─ Segment Customers → Allocate resources
   └─ Track SLAs → Ensure operations excellence
       ↓
4. GENERATE OUTPUTS
   ├─ Update dashboards
   ├─ Generate reports
   ├─ Send alerts (at-risk customers)
   ├─ Sync to Salesforce/HubSpot
   └─ Create action items
       ↓
5. TAKE ACTION
   ├─ CS calls at-risk customers
   ├─ Sales pursues expansion opportunities
   ├─ Product analyzes adoption gaps
   ├─ Ops improves SLA compliance
   └─ Finance updates forecasts
       ↓
END OF MONTH (Repeat)
```

---

## Integration Points

### 1. Data Input Sources

#### CRM Systems
```
Salesforce:
├─ Customer list
├─ Account hierarchy
├─ Custom fields (health score, segment)
├─ Opportunity pipeline
└─ Activity history

HubSpot:
├─ Company database
├─ Contact records
├─ Deal pipeline
└─ Contact history
```

#### Billing Systems
```
Stripe/Zuora:
├─ MRR per customer
├─ Subscription status
├─ Payment history
├─ Billing cycles
└─ Churn events
```

#### Product Usage
```
Mixpanel/Amplitude:
├─ Feature adoption
├─ Login frequency
├─ Feature usage depth
├─ User engagement
└─ Session data
```

#### Support
```
Zendesk/Intercom:
├─ Ticket volume
├─ Support satisfaction (CSAT)
├─ Resolution time
├─ Sentiment analysis
└─ Support agent assignments
```

### 2. Output Destinations

#### Dashboards
```
Internal Team:
├─ CS leaders
├─ Sales leaders
├─ Finance team
├─ Product team
└─ Executive team

Tools:
├─ Tableau
├─ Looker
├─ Google Data Studio
├─ Salesforce dashboards
└─ Custom HTML dashboards
```

#### Salesforce Integration
```
Updates to Salesforce:
├─ Account fields:
│  ├─ Health Score (custom field)
│  ├─ Segment (RFM)
│  ├─ NRR % (for account cohort)
│  ├─ Risk Level
│  └─ Last CS health check
├─ Automated workflows:
│  ├─ Alert on health score drop
│  ├─ Create task for at-risk follow-up
│  ├─ Route to expansion playbook
│  └─ Trigger escalation
└─ Opportunity pipeline:
   ├─ Tag expansion opportunities
   ├─ Add expansion amount
   └─ Create renewal tasks
```

#### HubSpot Integration
```
Updates to HubSpot:
├─ Company fields:
│  ├─ Health score
│  ├─ Customer segment
│  ├─ Churn risk
│  └─ NRR %
├─ Contact fields:
│  ├─ Primary contact health
│  └─ Engagement level
├─ Custom workflows:
│  ├─ At-risk alert sequence
│  ├─ Upsell workflow
│  └─ Win-back campaign
└─ Reporting:
   ├─ Health score trends
   ├─ Segment distribution
   └─ NRR by segment
```

#### Slack Alerts
```
Daily/Weekly Messages:
├─ New at-risk customers (top 5)
├─ SLA breaches this week
├─ NRR update (weekly)
├─ Escalation summary
└─ Action items for team
```

---

## Data Model

### Core Tables

#### customers
```
customer_id (PK)
account_name
mrr (Monthly Recurring Revenue)
signup_date
industry
company_size
account_tier (SMB, Mid-Market, Enterprise)
customer_status (active, churned, dormant)
```

#### monthly_metrics
```
month_id (PK)
customer_id (FK)
beginning_mrr
expansion_mrr
churn_mrr
feature_adoption_percent
support_satisfaction
payment_on_time_percent
login_count
```

#### health_scores
```
score_date
customer_id (FK)
health_score (0-100)
risk_level (Healthy, At-Risk, Critical)
adoption_score
sentiment_score
payment_score
velocity_score
trend (improving, stable, declining)
```

#### segments
```
segment_date
customer_id (FK)
rfm_score (e.g., "554")
segment_name (Champions, Loyal, etc.)
annual_value
growth_score
churn_risk_score
```

#### support_tickets
```
ticket_id (PK)
customer_id (FK)
created_date
first_response_date
resolved_date
priority (P1, P2, P3, P4)
escalated (true/false)
sla_met (true/false)
satisfaction_score
```

---

## Calculation Order (Dependencies)

```
Step 1: DATA IMPORT
   ↓
Step 2: CLEAN DATA
   ↓
Step 3: CALCULATE HEALTH SCORES
   (Requires: adoption, sentiment, payment, velocity)
   ↓
Step 4: CALCULATE NRR
   (Requires: MRR data by month)
   ↓
Step 5: ANALYZE CHURN & RETENTION
   (Requires: customer signup dates + MRR history)
   ↓
Step 6: SEGMENT CUSTOMERS
   (Requires: RFM data + health score + NRR)
   ↓
Step 7: TRACK SLA COMPLIANCE
   (Requires: support ticket data)
   ↓
Step 8: GENERATE OUTPUTS
   (All 5 tools complete)
```

**Each step depends on previous steps. Run in order.**

---

## Technology Stack

### Data Storage
```
Option 1: Spreadsheet (Excel/Sheets)
├─ Best for: <1000 customers, getting started
├─ Cost: Free (Google Sheets) or $
├─ Scalability: Limited to ~5,000 rows

Option 2: Database (Postgres/MySQL)
├─ Best for: 1,000-50,000 customers
├─ Cost: Managed (AWS/GCP)
├─ Scalability: Excellent

Option 3: Data Warehouse (Snowflake/BigQuery)
├─ Best for: 10,000+ customers, complex analysis
├─ Cost: Pay per usage
├─ Scalability: Unlimited
```

### Calculation Engine
```
Option 1: Excel Formulas
├─ Best for: Simple metrics, learning
├─ Languages: Excel formula syntax
├─ Maintenance: Manual

Option 2: Python/Pandas
├─ Best for: Automation, reproducibility
├─ Languages: Python
├─ Maintenance: Version control (GitHub)

Option 3: SQL
├─ Best for: Database-native, scalable
├─ Languages: SQL
├─ Maintenance: Query versioning
```

### Visualization
```
Option 1: Excel Charts
├─ Cost: Free
├─ Complexity: Basic
└─ Interactivity: Limited

Option 2: Google Data Studio
├─ Cost: Free
├─ Complexity: Medium
└─ Interactivity: Good

Option 3: Tableau/Looker
├─ Cost: $$/month
├─ Complexity: High
└─ Interactivity: Excellent
```

---

## Security Architecture

```
DATA FLOW SECURITY:
                    
User → GitHub Repo → Salesforce/HubSpot → Dashboards
       ↓                 ↓                  ↓
    (HTTP)         (OAuth 2.0)        (Encrypted)
    (Public or      (Secure)           (TLS/SSL)
    Private)
     
NO DATA:
✗ Stored externally
✗ Sent to 3rd party APIs
✗ Exposed in logs
✗ Transmitted unencrypted

YES SECURITY:
✓ Local calculation
✓ Encrypted connections
✓ Access control
✓ Audit logs
✓ Data retention policies
```

---

## Scalability Considerations

### By Customer Count
```
< 500 customers:
├─ Spreadsheet (Excel)
├─ Manual calculations
├─ Quarterly reviews
└─ Monthly 2-3 hour effort

500-5,000 customers:
├─ Database + Python
├─ Automated calculations
├─ Weekly reviews
└─ Monthly 4-6 hour effort

5,000-50,000 customers:
├─ Data warehouse (Snowflake)
├─ Fully automated
├─ Daily updates
├─ Real-time dashboards
└─ 1 FTE to maintain

50,000+ customers:
├─ Enterprise data platform
├─ ML-based predictions
├─ Real-time operations
├─ Predictive analytics
└─ 2-3 FTE dedicated team
```

### Optimization Tips
```
If calculations are slow:
1. Archive old data (>2 years)
2. Use indexes on key fields
3. Run batch jobs at night
4. Use aggregated tables
5. Cache results

If dashboards are laggy:
1. Reduce refresh frequency
2. Use pre-aggregated data
3. Limit historical data shown
4. Use pagination
5. Distribute load
```

---

## Monitoring & Health Checks

```
Daily Checks:
□ Data freshness (updated in last 24h)
□ No data validation errors
□ Health scores calculated
□ NRR tracking updated
□ SLA compliance tracked

Weekly Checks:
□ Compare to previous week
□ Spot check accuracy
□ Review at-risk customers
□ Check escalations
□ Team training reinforcement

Monthly Checks:
□ Full validation run
□ Reconcile with source systems
□ Review metric trends
□ Adjust thresholds if needed
□ Board reporting
```

---

## Disaster Recovery

```
If you lose your data:

1. Source systems (Salesforce, Stripe) still have original data
2. GitHub has all calculation logic
3. Historical snapshots in previous commits
4. Can recalculate from source within 2-3 hours

Backup Strategy:
├─ GitHub (version control)
├─ Salesforce/HubSpot (source of truth)
├─ Monthly CSV exports
└─ Quarterly snapshots
```

---

## Future Enhancements

```
Phase 2 (Q2 2024):
├─ Real-time data connectors (no CSV uploads)
├─ ML-based churn prediction
├─ Automated escalations

Phase 3 (Q3 2024):
├─ AI-powered recommendations
├─ Advanced cohort analysis
├─ Custom metric builder

Phase 4 (Q4 2024):
├─ Enterprise SLA system
├─ Multi-product tracking
├─ Competitive benchmarking
```

---

## Architecture Decisions

### Why 5 Tools?
```
✅ Each addresses different CS pain point
✅ Can be used independently
✅ But more powerful together
✅ Covers all critical metrics
✅ Proven by SaaS industry
```

### Why Not More?
```
✗ Complexity without value
✗ Difficult to maintain
✗ Overwhelming for teams
✗ Diminishing returns
```

### Why These Integration Points?
```
Salesforce & HubSpot:
✓ Where CS team works daily
✓ Easy to action on data
✓ Workflow automation

Slack:
✓ Real-time awareness
✓ Immediate action
✓ Team alignment

Dashboards:
✓ Executive visibility
✓ Decision-making
✓ Board reporting
```

---

## Troubleshooting Architecture Issues

### Data doesn't match Salesforce
```
1. Check extraction date
2. Verify filters (active only?)
3. Confirm MRR definition
4. Check for timing differences
5. Validate data types
```

### Health scores seem wrong
```
1. Review weighting (30-20-25-25)
2. Check input data
3. Verify thresholds
4. Compare to manual assessment
5. Adjust weights
```

### SLA calculations off
```
1. Confirm time zones
2. Check business hours setting
3. Verify priority definitions
4. Review escalation logic
5. Test with sample data
```

---

**For specific tool architecture, see each TOOL_X.md file.**

*For questions about integration, see FAQ.md*
