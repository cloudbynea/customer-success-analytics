# 📖 Data Dictionary

**Complete reference of all metrics, fields, and calculations**

---

## Core Customer Fields

### customer_id
**Type**: String (unique identifier)
**Source**: Salesforce / HubSpot / Internal system
**Required**: Yes
**Example**: `CUST-12345` or `salesforce-001D000000IRFmaIAH`
**Description**: Unique identifier for each customer account
**Usage**: Join key across all calculations

### account_name
**Type**: String
**Source**: Salesforce Account Name / HubSpot Company
**Required**: Yes
**Example**: `Acme Corporation`, `TechFlow Inc`
**Description**: Company name for display purposes
**Usage**: Reports, dashboards, human-readable output

### mrr (Monthly Recurring Revenue)
**Type**: Numeric (decimal)
**Source**: Billing system (Stripe, Zuora, Salesforce)
**Required**: Yes
**Example**: `5000.00`, `250.50`, `15000`
**Units**: USD (or your currency)
**Description**: Current monthly recurring revenue for this customer
**Formula**: Sum of all active subscriptions for this customer
**Usage**: NRR, CLV, segmentation, expansion tracking
**Notes**: 
- Should be current month
- Exclude one-time fees
- Exclude discounts that don't affect ARR

### signup_date
**Type**: Date (YYYY-MM-DD)
**Source**: Salesforce / HubSpot / Product analytics
**Required**: Yes
**Example**: `2023-01-15`, `2024-02-20`
**Description**: Date customer first became active
**Usage**: Cohort analysis, payback period, lifespan calculation
**Notes**:
- Should be when contract started
- Not when account created in system
- Used for cohort grouping (usually monthly)

### industry
**Type**: String (categorical)
**Source**: Salesforce / HubSpot / Manual
**Required**: No (but helpful)
**Example**: `Technology`, `Healthcare`, `Finance`, `SaaS`
**Description**: Industry vertical for customer
**Usage**: Benchmarking, segmentation, trend analysis
**Notes**: Standardize values (use industry taxonomy)

### company_size
**Type**: String (categorical)
**Source**: Salesforce / HubSpot / Manual
**Required**: No (but helpful)
**Options**: 
- `1-10 (Micro)`
- `11-50 (Small)`
- `51-200 (Mid-Market)`
- `201-1000 (Enterprise)`
- `1000+ (Large Enterprise)`
**Description**: Estimated company size
**Usage**: Segmentation, SLA matching, playbook selection

### account_tier
**Type**: String (categorical)
**Source**: Your CS classification
**Required**: No (but recommended)
**Options**: 
- `SMB` (Small Business)
- `Mid-Market` 
- `Enterprise`
- `Strategic`
**Description**: How you've classified this customer
**Usage**: Resource allocation, SLA assignment, playbook selection
**Notes**: Define criteria (based on MRR or ARR)

### customer_status
**Type**: String (categorical)
**Source**: Billing system / Salesforce
**Required**: Yes
**Options**:
- `active` - Currently paying
- `churned` - Contract ended / No longer paying
- `dormant` - Inactive but not officially churned
- `trial` - In trial period
**Description**: Current lifecycle status
**Usage**: Filtering for calculations, churn analysis
**Notes**: 
- Don't include churned customers in health scores
- Include in cohort retention analysis

---

## Health Score Fields

### feature_adoption_percent
**Type**: Numeric (0-100)
**Source**: Product analytics (Mixpanel, Amplitude, custom)
**Required**: For health score calculation
**Example**: `65`, `15`, `100`
**Description**: Percentage of available features being used by customer
**Calculation**: (Features used / Total features) × 100
**Usage**: Health score component (30% weight)
**Thresholds**:
- 80-100%: Excellent (5/5)
- 60-79%: Good (4/5)
- 40-59%: Moderate (3/5)
- 20-39%: Low (2/5)
- 0-19%: Very Low (1/5)

### features_adopted
**Type**: Integer
**Source**: Product analytics
**Required**: No (alternative to percent)
**Example**: `15`, `5`, `25`
**Description**: Count of features actively used
**Usage**: Calculate adoption_percent if not available

### features_total
**Type**: Integer
**Source**: Product database
**Required**: For adoption calculation
**Example**: `25`, `30`, `50`
**Description**: Total available features in product
**Usage**: Calculate adoption_percent

### support_satisfaction (CSAT)
**Type**: Numeric (1-5 scale)
**Source**: Support ticketing system (Zendesk, Intercom)
**Required**: For health score calculation
**Example**: `4.2`, `3.8`, `5.0`
**Description**: Average customer satisfaction score from support tickets
**Calculation**: Average of all CSAT responses last 30 days
**Usage**: Health score component (20% weight)
**Thresholds**:
- 4.5-5.0: Excellent (5/5)
- 4.0-4.4: Good (4/5)
- 3.0-3.9: Moderate (3/5)
- 2.0-2.9: Low (2/5)
- <2.0: Very Low (1/5)

### support_tickets_last_30d
**Type**: Integer
**Source**: Support ticketing system
**Required**: No (informational)
**Example**: `5`, `15`, `0`
**Description**: Number of support tickets in last 30 days
**Usage**: Engagement indicator, trend analysis

### payment_health_percent
**Type**: Numeric (0-100)
**Source**: Billing system
**Required**: For health score calculation
**Example**: `100`, `80`, `50`
**Description**: Percentage of payments on-time
**Calculation**: (On-time payments / Total payments) × 100
**Usage**: Health score component (25% weight)
**Thresholds**:
- 95-100%: Excellent (5/5)
- 90-94%: Good (4/5)
- 80-89%: Moderate (3/5)
- 70-79%: Low (2/5)
- <70%: Very Low (1/5)

### months_overdue
**Type**: Integer
**Source**: Billing system
**Required**: No (flag for payment issues)
**Example**: `0`, `1`, `3`
**Description**: Months invoice is overdue
**Usage**: Payment health calculation, risk flag
**Notes**: Any value > 0 = red flag

### last_payment_date
**Type**: Date (YYYY-MM-DD)
**Source**: Billing system
**Required**: For engagement metrics
**Example**: `2024-03-10`, `2024-01-15`
**Description**: Date of most recent payment
**Usage**: Calculate days_since_payment, engagement indicator
**Notes**: Null = never paid or churned

### engagement_velocity_percent
**Type**: Numeric (-100 to +100)
**Source**: Product analytics
**Required**: For health score calculation
**Example**: `+30`, `-15`, `0`
**Description**: Month-over-month change in usage
**Calculation**: ((Current logins - Previous logins) / Previous logins) × 100
**Usage**: Health score component (25% weight)
**Thresholds**:
- >20%: Excellent (5/5)
- 5-20%: Good (4/5)
- -5 to +5%: Moderate (3/5)
- -20 to -5%: Low (2/5)
- <-20%: Very Low (1/5)

### login_count_current_month
**Type**: Integer
**Source**: Product analytics
**Required**: For velocity calculation
**Example**: `45`, `5`, `100`
**Description**: Number of logins in current month
**Usage**: Calculate engagement velocity

### login_count_previous_month
**Type**: Integer
**Source**: Product analytics
**Required**: For velocity calculation
**Example**: `35`, `8`, `95`
**Description**: Number of logins in previous month
**Usage**: Calculate engagement velocity

### health_score
**Type**: Numeric (0-100)
**Source**: Calculated from above inputs
**Output**: Yes
**Example**: `75`, `35`, `95`
**Description**: Overall customer health score
**Formula**: (Adoption×0.30) + (Sentiment×0.20) + (Payment×0.25) + (Velocity×0.25)
**Usage**: Risk identification, prioritization
**Risk Levels**:
- 80-100: ✅ Healthy
- 60-79: ⚠️ At-Risk
- 40-59: 🔴 Critical
- 0-39: 💀 At-Extreme-Risk

---

## NRR Fields

### beginning_mrr
**Type**: Numeric (decimal)
**Source**: Billing system
**Required**: Yes (for NRR calculation)
**Example**: `100000.00`, `50000`, `5000`
**Description**: Total MRR at start of period
**Formula**: Sum of all active customer MRR on first day of month
**Usage**: NRR denominator

### expansion_mrr
**Type**: Numeric (decimal)
**Source**: Billing system
**Required**: Yes (for NRR calculation)
**Example**: `15000`, `5000`, `500`
**Description**: New MRR from upsells and cross-sells
**Formula**: Sum of all MRR increases from existing customers
**Usage**: NRR expansion numerator

### churn_mrr
**Type**: Numeric (decimal)
**Source**: Billing system
**Required**: Yes (for NRR calculation)
**Example**: `8000`, `3000`, `200`
**Description**: MRR lost from churn and downgrades
**Formula**: Sum of MRR decreases (churn + downgrades)
**Usage**: NRR churn numerator

### ending_mrr
**Type**: Numeric (decimal)
**Source**: Calculated
**Output**: Yes
**Formula**: Beginning MRR + Expansion MRR - Churn MRR
**Example**: `107000`, `52000`, `5300`
**Description**: Total MRR at end of period
**Usage**: NRR calculation, trend tracking

### nrr_percent
**Type**: Numeric (decimal)
**Source**: Calculated
**Output**: Yes
**Example**: `107`, `104`, `106`
**Description**: Net Revenue Retention percentage
**Formula**: (Ending MRR / Beginning MRR) × 100
**Range**: 0-200% (typically 80-130%)
**Benchmarks**:
- <100%: Declining revenue ❌
- 100-110%: Breaking even ✅
- 110-120%: Healthy ✅✅
- >120%: Exceptional 🚀

### churn_rate_percent
**Type**: Numeric (0-100)
**Source**: Calculated
**Example**: `8`, `3`, `5`
**Description**: Monthly churn rate (% of revenue)
**Formula**: (Churn MRR / Beginning MRR) × 100
**Interpretation**: Percentage of revenue lost to churn each month
**Targets**:
- <3%: Excellent
- 3-5%: Healthy
- 5-7%: Needs work
- >7%: At risk

### expansion_rate_percent
**Type**: Numeric (0-100)
**Source**: Calculated
**Example**: `15`, `10`, `3`
**Description**: Growth from existing customers (% of revenue)
**Formula**: (Expansion MRR / Beginning MRR) × 100
**Interpretation**: Percentage of revenue added from existing customers

---

## Cohort & Retention Fields

### cohort_month
**Type**: Date (YYYY-MM, year-month)
**Source**: signup_date grouped by month
**Required**: For cohort analysis
**Example**: `2023-01`, `2024-03`
**Description**: Month customer was acquired
**Usage**: Group customers for cohort analysis

### cohort_age_months
**Type**: Integer
**Source**: Calculated
**Example**: `1`, `12`, `24`
**Description**: How many months since customer signup
**Formula**: Current month - Signup month
**Usage**: Cohort retention curves

### retention_percent
**Type**: Numeric (0-100)
**Source**: Calculated
**Example**: `85`, `72`, `60`
**Description**: % of original cohort still active
**Formula**: (Current active / Cohort size at month 1) × 100
**Usage**: Retention curves, health monitoring
**Interpretation**:
- 90%+ at 12mo: Excellent
- 80-90%: Good
- 70-80%: Acceptable
- <70%: Needs improvement

---

## CLV & Payback Fields

### arpu (Average Revenue Per User)
**Type**: Numeric (decimal)
**Source**: MRR / active_users or segment average
**Required**: For CLV calculation
**Example**: `100`, `500`, `5000`
**Description**: Average monthly revenue per user/seat
**Usage**: CLV calculation, expansion potential
**Notes**: Use segment-level if customer-level unavailable

### gross_margin_percent
**Type**: Numeric (0-100)
**Source**: Finance team / Standard for SaaS
**Required**: For CLV calculation
**Example**: `70`, `75`, `80`
**Description**: Percentage of revenue left after COGS
**Typical ranges**:
- SaaS: 70-85% (high margin)
- Services: 40-60% (lower margin)
**Usage**: CLV profitability calculation

### monthly_churn_rate_percent
**Type**: Numeric (0-100)
**Source**: Historical analysis
**Required**: For advanced CLV calculation
**Example**: `3`, `5`, `2`
**Description**: Expected monthly churn rate for customer
**Usage**: Lifetime calculation with decay

### customer_acquisition_cost
**Type**: Numeric (decimal)
**Source**: Sales & marketing expense / customers acquired
**Required**: For payback calculation
**Example**: `1500`, `500`, `5000`
**Description**: Average cost to acquire this customer
**Calculation**: Total S&M spend / New customers acquired
**Usage**: Payback period, LTV:CAC ratio

### clv (Customer Lifetime Value)
**Type**: Numeric (decimal)
**Source**: Calculated
**Output**: Yes
**Example**: `5000`, `10000`, `25000`
**Formula**: (ARPU × Months until churn × Gross Margin) - CAC
**Usage**: Valuation, CAC justification, pricing decisions

### payback_period_months
**Type**: Numeric (decimal)
**Source**: Calculated
**Output**: Yes
**Example**: `12`, `6`, `18`
**Formula**: CAC / (Monthly ARPU × Gross Margin)
**Range**: Typically 6-24 months
**Targets**:
- <6mo: Excellent (can spend aggressively)
- 6-12mo: Healthy (sustainable)
- 12-18mo: Long (acceptable)
- >18mo: Unsustainable (fix model)

### ltv_cac_ratio
**Type**: Numeric (decimal)
**Source**: Calculated
**Output**: Yes
**Example**: `3.3`, `5.0`, `2.1`
**Formula**: CLV / CAC
**Targets**:
- <1:1: Losing money
- 1:1: Break even
- 3:1: Healthy (sustainable)
- 5:1: Excellent (invest more)

---

## Segmentation Fields

### rfm_score
**Type**: String (3 digits)
**Source**: Calculated
**Example**: `554`, `232`, `111`
**Description**: Recency-Frequency-Monetary score
**Components**:
- First digit (1-5): Recency (days since last purchase)
- Second digit (1-5): Frequency (purchase count)
- Third digit (1-5): Monetary (total spent)

### recency_score (1-5)
**Type**: Integer
**Source**: Calculated from last_purchase_date
**Example**: `5`, `2`, `1`
**Thresholds**:
- 5: ≤30 days ago (very recent)
- 4: 31-60 days ago
- 3: 61-120 days ago
- 2: 121-180 days ago
- 1: >180 days ago (very old)

### frequency_score (1-5)
**Type**: Integer
**Source**: Calculated from transaction count
**Example**: `5`, `3`, `1`
**Thresholds**:
- 5: 10+ purchases (very frequent)
- 4: 6-9 purchases
- 3: 4-5 purchases
- 2: 2-3 purchases
- 1: 1 purchase (first-time)

### monetary_score (1-5)
**Type**: Integer
**Source**: Calculated from total_spent
**Example**: `5`, `3`, `1`
**Thresholds**:
- 5: Top 20% spenders
- 4: 21-40% spenders
- 3: 41-60% spenders
- 2: 61-80% spenders
- 1: Bottom 20% spenders

### segment_rfm
**Type**: String (categorical)
**Source**: Calculated from RFM score
**Example**: `Champions`, `Loyal Customers`, `Lost`
**Options**:
- `Champions` (5-5-5 to 4-4-5)
- `Loyal Customers` (4-4-5 to 4-3-4)
- `Potential` (4-3-4 to 3-3-3)
- `Promising` (3-3-3)
- `At-Risk` (3-2-3 to 2-2-2)
- `Hibernating` (2-2-2)
- `Slipping Away` (2-2-1)
- `Lost` (1-1-1)
- `Can't Lose Them` (1-1-2 to 1-2-1)

### annual_value
**Type**: Numeric (decimal)
**Source**: MRR × 12
**Example**: `60000`, `36000`, `30000`
**Description**: Annualized revenue from customer
**Usage**: Profit segmentation x-axis

### growth_score
**Type**: Numeric (0-100)
**Source**: Calculated
**Example**: `85`, `45`, `20`
**Formula**: (Adoption×0.3) + (NRR×0.4) + (Expansion potential×0.3)
**Description**: Customer's growth potential
**Usage**: Profit segmentation y-axis

### churn_risk_score
**Type**: Numeric (0-100)
**Source**: Inverse of health score
**Example**: `25`, `65`, `80`
**Formula**: 100 - health_score
**Description**: Risk of customer churning
**Usage**: Prioritization, intervention urgency

### segment_profit_based
**Type**: String (categorical)
**Source**: annual_value + growth_score
**Example**: `High-Value Accounts`, `Problem Accounts`
**Options**:
- `High-Value Accounts`: High revenue + high growth
- `Growth Accounts`: Medium revenue + high growth
- `Maintenance Accounts`: Low revenue + stable
- `Problem Accounts`: Any value + churn risk or low engagement

---

## Escalation & SLA Fields

### ticket_id
**Type**: String (unique)
**Source**: Support system (Zendesk, Intercom)
**Required**: Yes
**Example**: `ZD-12345`, `support-98765`
**Description**: Unique ticket identifier
**Usage**: Track individual tickets

### priority_level
**Type**: String (categorical)
**Source**: Support team assignment
**Required**: Yes
**Options**:
- `P1` (Critical)
- `P2` (High)
- `P3` (Medium)
- `P4` (Low)
**Description**: Ticket urgency level
**Usage**: SLA assignment, routing

### created_date
**Type**: DateTime
**Source**: Support system
**Required**: Yes
**Example**: `2024-03-15 09:30:00`
**Description**: When ticket was created
**Usage**: SLA deadline calculation

### first_response_date
**Type**: DateTime
**Source**: Support system
**Required**: For response SLA
**Example**: `2024-03-15 10:15:00`
**Description**: When first response was sent
**Usage**: Calculate response time

### resolved_date
**Type**: DateTime
**Source**: Support system
**Required**: For resolution SLA
**Example**: `2024-03-15 13:45:00`
**Description**: When ticket was fully resolved
**Usage**: Calculate resolution time

### sla_response_target_hours
**Type**: Numeric
**Source**: Your SLA definition
**Example**: `1`, `4`, `8`, `24`
**Description**: Target response time in hours by priority
**Usage**: Calculate response SLA compliance

### sla_resolution_target_hours
**Type**: Numeric
**Source**: Your SLA definition
**Example**: `4`, `24`, `72`, `720`
**Description**: Target resolution time in hours by priority
**Usage**: Calculate resolution SLA compliance

### response_time_hours
**Type**: Numeric (decimal)
**Source**: Calculated
**Example**: `1.5`, `3.25`, `12.75`
**Formula**: (first_response_date - created_date) / 3600
**Description**: Actual response time in hours
**Usage**: SLA compliance check

### resolution_time_hours
**Type**: Numeric (decimal)
**Source**: Calculated
**Example**: `4.5`, `24.0`, `48.5`
**Formula**: (resolved_date - created_date) / 3600
**Description**: Actual resolution time in hours
**Usage**: SLA compliance check

### sla_response_met
**Type**: Boolean
**Source**: Calculated
**Example**: `true`, `false`
**Formula**: response_time_hours <= sla_response_target_hours
**Description**: Whether response SLA was met
**Usage**: Compliance tracking

### sla_resolution_met
**Type**: Boolean
**Source**: Calculated
**Example**: `true`, `false`
**Formula**: resolution_time_hours <= sla_resolution_target_hours
**Description**: Whether resolution SLA was met
**Usage**: Compliance tracking

### escalated
**Type**: Boolean
**Source**: Support system
**Example**: `true`, `false`
**Description**: Whether ticket was escalated
**Usage**: Escalation pattern analysis

### escalation_reason
**Type**: String (categorical)
**Source**: Support team
**Example**: `complexity`, `customer_dissatisfaction`, `priority`, `expertise_needed`
**Description**: Why ticket was escalated
**Usage**: Root cause analysis

### satisfaction_score
**Type**: Numeric (1-5)
**Source**: Post-resolution survey
**Example**: `4`, `3`, `5`
**Description**: Customer satisfaction with resolution
**Usage**: Quality tracking, trend analysis

---

## Derived / Output Fields

### risk_level
**Type**: String (categorical)
**Source**: Calculated from health_score
**Options**:
- `Healthy` (80-100)
- `At-Risk` (60-79)
- `Critical` (40-59)
- `At-Extreme-Risk` (0-39)
**Description**: Health score category
**Usage**: Quick status indicator

### days_since_last_activity
**Type**: Integer
**Source**: Calculated from last_activity_date
**Example**: `5`, `30`, `90`
**Description**: Days since any customer activity
**Usage**: Engagement indicator, dormant flag

### expansion_opportunity_flag
**Type**: Boolean
**Example**: `true`, `false`
**Description**: Customer eligible for upsell/cross-sell
**Logic**: Healthy AND high adoption AND annual_value > median
**Usage**: Expansion targeting

### at_risk_flag
**Type**: Boolean
**Example**: `true`, `false`
**Description**: Needs immediate CS intervention
**Logic**: health_score < 60 OR churn_risk > 50
**Usage**: Prioritization, alerts

### nrr_impact
**Type**: Numeric (decimal)
**Source**: Calculated
**Example**: `1500`, `-3000`, `500`
**Description**: This customer's contribution to NRR
**Formula**: (Expansion - Churn MRR)
**Usage**: Identify NRR drivers/detractors

---

## Summary Table: All Key Metrics

| Category | Metric | Type | Range | Unit | Purpose |
|----------|--------|------|-------|------|---------|
| **Health** | health_score | 0-100 | Numeric | Points | Churn risk |
| **Health** | adoption_percent | 0-100 | Numeric | % | Feature usage |
| **Health** | csat | 1-5 | Numeric | Score | Support satisfaction |
| **Revenue** | mrr | Any | Numeric | $ | Monthly revenue |
| **Revenue** | arr | Any | Numeric | $ | Annual revenue |
| **Revenue** | nrr_percent | 80-130 | Numeric | % | Growth health |
| **Revenue** | expansion_rate | 0-100 | Numeric | % | Upsell growth |
| **Revenue** | churn_rate | 0-100 | Numeric | % | Loss rate |
| **Economics** | clv | Any | Numeric | $ | Customer value |
| **Economics** | cac | Any | Numeric | $ | Acquisition cost |
| **Economics** | payback_months | 1-60 | Numeric | Months | Break-even time |
| **Economics** | ltv_cac_ratio | 0-10 | Numeric | Ratio | Unit economics |
| **Segmentation** | rfm_score | 111-555 | String | Code | Behavior score |
| **Segmentation** | segment | Many | String | Category | Segment name |
| **Operations** | sla_compliance | 0-100 | Numeric | % | Support quality |
| **Operations** | response_time | 0-∞ | Numeric | Hours | Response speed |
| **Operations** | resolution_time | 0-∞ | Numeric | Hours | Resolution speed |

---

## Notes on Data Quality

### Missing Values
- **Health Score**: Use median of populated values
- **CSAT**: Use customer tier average if missing
- **MRR**: Flag for review (shouldn't happen)
- **Dates**: Use system default if available

### Data Validation Rules
```
✓ MRR >= 0
✓ Dates are valid and in chronological order
✓ customer_id is unique
✓ Percentages are 0-100
✓ Scores are in valid range
✓ No future dates in historical data
```

### Refresh Frequency
- **Daily**: Logins, tickets, payments
- **Weekly**: Health scores, SLA metrics
- **Monthly**: NRR, cohort analysis, CLV
- **Quarterly**: Trend analysis, benchmarking

---

*For more information on calculations, see each TOOL_X.md file.*
