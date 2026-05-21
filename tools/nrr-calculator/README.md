# 📈 NRR Calculator - THE PRIMARY METRIC

**Net Revenue Retention** | The ultimate health indicator for SaaS businesses

---

## What is NRR and Why It Matters

Net Revenue Retention (NRR) is the single most important metric for SaaS companies because it measures:
- ✅ Customer satisfaction and happiness
- ✅ Product-market fit strength
- ✅ Predictability of future revenue
- ✅ True business health beyond new sales

> **"NRR is the metric that venture capitalists look at first"** - Successful SaaS investors

---

## The NRR Formula

```
NRR = (Beginning MRR + Expansion - Churn) / Beginning MRR × 100
```

### Components

| Component | Definition | Example |
|-----------|-----------|---------|
| **Beginning MRR** | MRR at start of period | $100,000 |
| **Expansion** | Upsells + cross-sells | +$15,000 |
| **Churn** | Customers lost + downgrade | -$10,000 |
| **Ending MRR** | Result | $105,000 |
| **NRR %** | (105,000 / 100,000) × 100 | **105%** |

---

## NRR Benchmarks & What They Mean

| NRR % | Interpretation | Business Status | Action |
|-------|---|---|---|
| **<100%** | 🔴 Declining revenue | Losing more than you're growing | Urgent: Fix product issues, improve retention |
| **100-110%** | ⚠️ Breaking even | Can grow with sales alone | Good foundation, focus on expansion |
| **110-120%** | ✅ Healthy growth | Strong product-market fit | Scaling phase, invest in sales |
| **>120%** | 🚀 Exceptional | Rare and valuable | Category leader, aggressive expansion |

### Historical Context
- **Salesforce (early days)**: 130%+ (became dominant)
- **HubSpot (IPO)**: 115%+ (strong trajectory)
- **Typical SaaS average**: 95-105% (competitive)
- **Industry median**: 105% (for growth-stage)

---

## Step-by-Step Calculation

### Example: Acme Software (Monthly Calculation)

```
MONTH 1 BASELINE (January)
├── Beginning MRR: $100,000
├── Active Customers: 50
└── Avg Contract Value: $2,000

MONTH 2 TRANSACTIONS (February)
├── New Customers: 5 × $2,000 = +$10,000
├── Upsells (existing customers): +$8,000
├── Cross-sells: +$3,000
├── Downgrades: -$2,000
├── Churned Customers: 3 × $2,000 = -$6,000
└── Ending MRR: $113,000

CALCULATION
├── Beginning MRR: $100,000
├── Total Growth (Exp + Churn): $13,000 - $8,000 = +$5,000
├── Revenue Retained: $100,000
├── NRR = ($100,000 + $5,000) / $100,000 × 100 = 105%
└── Interpretation: For every $1 in MRR, we're keeping $1.05
```

---

## Cohort-Based NRR (Most Accurate)

Cohorts allow you to track how different customer groups perform over time:

```
Cohort Analysis - January 2024 Cohort

Month  | Starting | New    | Upsell | Churn | Ending  | Cohort NRR
-------|----------|--------|--------|-------|---------|----------
Jan    | $50,000  | -      | -      | -     | $50,000 | 100%
Feb    | $50,000  | -      | $5,000 | -2K   | $53,000 | 106%
Mar    | $53,000  | -      | $4,000 | -3K   | $54,000 | 108%
Apr    | $54,000  | -      | $3,000 | -3K   | $54,000 | 108%
May    | $54,000  | -      | $2,000 | -4K   | $52,000 | 104%

12-Month Cohort NRR: 104% (from $50K to $52K)
```

### Why Cohort NRR is Better
- Removes noise from new customer acquisition
- Shows true retention and expansion
- Allows comparison across quarters
- Identifies trends in customer longevity

---

## Drivers of NRR

### 1. Churn Rate (Negative Impact)

```
Churn Impact = (Churned MRR / Beginning MRR) × 100

Example:
- Lost 3 customers at $2,000/mo each = $6,000 churn
- Impact on NRR: -6% down from 100%
```

**Industry Benchmarks**
- Elite SaaS: <1% monthly churn
- High-growth: 3-5% monthly churn
- At-risk: >5% monthly churn
- In decline: >7% monthly churn

### 2. Expansion Revenue (Positive Impact)

```
Expansion Rate = (Upsell + Cross-sell MRR) / Beginning MRR × 100

Example:
- 20 customers upsell average $500 = $10,000
- Expansion rate: 10%
```

**Ways to Drive Expansion**
- ✅ Feature adoption training
- ✅ Identified customer needs
- ✅ Success metrics dashboards
- ✅ Regular business reviews
- ✅ Tiered pricing strategy

### 3. Downgrades (Mixed Impact)

```
Downgrade Rate = (Downgrades MRR / Beginning MRR) × 100

Example:
- 5 customers downgrade average $300 = $1,500
- Downgrade rate: -1.5%
```

---

## Calculation in Python

```python
import pandas as pd
from datetime import datetime, timedelta

class NRRCalculator:
    """Calculate Net Revenue Retention for SaaS companies"""
    
    def __init__(self):
        self.data = None
    
    def load_data(self, filepath):
        """Load customer MRR history"""
        self.data = pd.read_csv(filepath)
        return self
    
    def calculate_nrr(self, period_start, period_end):
        """
        Calculate NRR for a specific period
        
        Args:
            period_start: datetime of month start
            period_end: datetime of month end
            
        Returns:
            dict with NRR and components
        """
        # Beginning MRR (start of period)
        month_start = self.data[self.data['date'] <= period_start]
        beginning_mrr = month_start['mrr'].sum()
        
        # Get all transactions during period
        period_data = self.data[
            (self.data['date'] > period_start) & 
            (self.data['date'] <= period_end)
        ]
        
        # Calculate movements
        expansion = period_data[period_data['event'] == 'upsell']['amount'].sum()
        cross_sell = period_data[period_data['event'] == 'cross_sell']['amount'].sum()
        downgrade = period_data[period_data['event'] == 'downgrade']['amount'].sum()
        churn = period_data[period_data['event'] == 'churn']['amount'].sum()
        
        # Calculate ending MRR
        total_expansion = expansion + cross_sell
        total_negative = downgrade + churn
        ending_mrr = beginning_mrr + total_expansion + total_negative
        
        # Calculate NRR
        if beginning_mrr == 0:
            nrr = 0
        else:
            nrr = (ending_mrr / beginning_mrr) * 100
        
        return {
            'period_start': period_start,
            'period_end': period_end,
            'beginning_mrr': beginning_mrr,
            'expansion': total_expansion,
            'churn': total_negative,
            'ending_mrr': ending_mrr,
            'nrr_percent': round(nrr, 2),
            'status': self._get_status(nrr)
        }
    
    def calculate_cohort_nrr(self, cohort_date, months=12):
        """
        Calculate NRR for a customer cohort over time
        """
        cohort_customers = self.data[self.data['cohort'] == cohort_date]
        
        results = []
        for month in range(months):
            end_date = cohort_date + timedelta(days=30*month)
            nrr = self.calculate_nrr(cohort_date, end_date)
            results.append(nrr)
        
        return results
    
    def get_drivers(self, nrr_data):
        """Break down what's driving NRR"""
        beginning = nrr_data['beginning_mrr']
        
        churn_impact = (nrr_data['churn'] / beginning) * 100
        expansion_impact = (nrr_data['expansion'] / beginning) * 100
        
        return {
            'expansion_contribution': expansion_impact,
            'churn_impact': churn_impact,
            'net_effect': expansion_impact + churn_impact
        }
    
    def _get_status(self, nrr):
        if nrr >= 120:
            return "Exceptional"
        elif nrr >= 110:
            return "Healthy"
        elif nrr >= 100:
            return "Breaking Even"
        else:
            return "Declining"

# Usage Example
calculator = NRRCalculator()
calculator.load_data('customer_mrr_history.csv')

nrr_result = calculator.calculate_nrr(
    datetime(2024, 2, 1),
    datetime(2024, 2, 29)
)

print(f"NRR: {nrr_result['nrr_percent']}%")
print(f"Status: {nrr_result['status']}")

drivers = calculator.get_drivers(nrr_result)
print(f"Expansion Impact: +{drivers['expansion_contribution']}%")
print(f"Churn Impact: {drivers['churn_impact']}%")
```

---

## NRR vs. Other Metrics

| Metric | What It Shows | When to Use |
|--------|---|---|
| **NRR** ⭐ | Overall business health | Strategic planning, VC meetings |
| **MRR** | Revenue this month | Cash flow tracking |
| **ARR** | Annual revenue projection | Reporting & valuation |
| **Churn Rate** | Customer loss % | Operational focus area |
| **CAC** | Cost to acquire | Sales efficiency |
| **LTV** | Customer lifetime value | Long-term planning |
| **Expansion Rate** | Revenue growth from existing | Growth potential |

---

## Dashboard Template

```html
<!DOCTYPE html>
<html>
<head>
    <title>NRR Dashboard</title>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
    <style>
        body { font-family: 'Segoe UI', sans-serif; }
        .dashboard { max-width: 1200px; margin: auto; padding: 20px; }
        .metric-box {
            display: inline-block;
            width: 22%;
            margin: 1%;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        }
        .metric-value { font-size: 36px; font-weight: bold; }
        .metric-label { font-size: 12px; color: #666; }
        .exceptional { background: #d4edda; border-left: 4px solid #28a745; }
        .healthy { background: #d1ecf1; border-left: 4px solid #17a2b8; }
        .warning { background: #fff3cd; border-left: 4px solid #ffc107; }
        .declining { background: #f8d7da; border-left: 4px solid #dc3545; }
    </style>
</head>
<body>
    <div class="dashboard">
        <h1>📊 Net Revenue Retention (NRR) Dashboard</h1>
        
        <div id="current-nrr" class="metric-box exceptional">
            <div class="metric-label">Current NRR</div>
            <div class="metric-value">115%</div>
        </div>
        
        <div id="beginning-mrr" class="metric-box">
            <div class="metric-label">Beginning MRR</div>
            <div class="metric-value">$500K</div>
        </div>
        
        <div id="ending-mrr" class="metric-box">
            <div class="metric-label">Ending MRR</div>
            <div class="metric-value">$575K</div>
        </div>
        
        <div id="nrr-trend" class="metric-box">
            <div class="metric-label">6-Month Trend</div>
            <div class="metric-value">↗ +5%</div>
        </div>
        
        <div id="chart"></div>
        <div id="cohort-chart"></div>
    </div>
    
    <script>
        // NRR Trend Chart
        const trace1 = {
            x: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
            y: [108, 110, 112, 114, 115, 115],
            type: 'scatter',
            name: 'NRR %'
        };
        
        Plotly.newPlot('chart', [trace1], {
            title: 'NRR Trend',
            xaxis: { title: 'Month' },
            yaxis: { title: 'NRR %' }
        });
    </script>
</body>
</html>
```

---

## Action Plan by NRR Level

### If NRR < 100% (Revenue Declining)
**Priority: CRITICAL**
```
Week 1:
  [ ] Analyze churn reasons (customer interviews)
  [ ] Review recent product changes
  [ ] Identify price-sensitive customers
  
Week 2:
  [ ] Launch win-back campaign for recent churners
  [ ] Create retention task force
  [ ] Implement churn alerts
  
Week 3:
  [ ] Implement customer health scores
  [ ] Increase CS resources
  [ ] Daily churn tracking
  
Success metric: Stabilize NRR at 100% within 60 days
```

### If NRR 100-110% (Breaking Even)
**Priority: HIGH**
```
Month 1:
  [ ] Audit customer success processes
  [ ] Identify expansion opportunities
  [ ] Launch training programs
  
Month 2:
  [ ] Implement health scoring
  [ ] Increase touchpoints
  [ ] Create expansion playbooks
  
Month 3:
  [ ] A/B test upsell messaging
  [ ] Analyze top expansion customers
  [ ] Build expansion hiring plan
  
Success metric: Increase NRR to 110%+ within 90 days
```

### If NRR 110-120% (Healthy)
**Priority: MEDIUM**
```
Ongoing:
  [ ] Maintain CS excellence
  [ ] Continuous product innovation
  [ ] Customer advisory board
  [ ] Industry benchmarking
  
Optimization:
  [ ] Refine expansion motions
  [ ] Optimize pricing
  [ ] Explore market expansion
  
Success metric: Maintain or increase NRR while growing customer base
```

### If NRR > 120% (Exceptional)
**Priority: SCALE**
```
Strategic:
  [ ] Scale sales and marketing
  [ ] Explore adjacent markets
  [ ] Consider IPO preparation
  [ ] Build brand leadership
  
Operations:
  [ ] Document winning playbooks
  [ ] Build competitive moat
  [ ] Invest in product innovation
  
Success metric: Scale with unit economics intact
```

---

## Common Mistakes to Avoid

❌ **Ignoring churn because you're hiring new customers**
- New CAC only offsets losses, doesn't grow company

❌ **Measuring NRR annually instead of monthly**
- Too slow to react to trends
- Monthly cohorts reveal true dynamics

❌ **Mixing new and existing customer revenue**
- Use only existing customer movements for NRR

❌ **Not tracking expansion separately**
- Hidden opportunity if you don't see upsell potential

❌ **Using gross instead of net retention**
- NRR is the ultimate metric; use it

---

## Integration Examples

### Salesforce
```python
# Push NRR to Account object
sf.Account.update('001D000000IRFmaIAH', {
    'NRR_Percent__c': 115.0,
    'NRR_Status__c': 'Healthy'
})
```

### Data Warehouse (Snowflake)
```sql
-- NRR calculation from data warehouse
WITH mrr_history AS (
    SELECT
        date,
        customer_id,
        mrr,
        LAG(mrr) OVER (PARTITION BY customer_id ORDER BY date) as prev_mrr
    FROM customer_mrr
)
SELECT
    DATE_TRUNC('MONTH', date) as period,
    SUM(CASE WHEN MONTH(date) = MONTH(prev_date) THEN mrr ELSE 0 END) / 
    SUM(prev_mrr) * 100 as nrr
FROM mrr_history
GROUP BY period;
```

---

## Next Steps

1. **Calculate your current NRR** using provided calculator
2. **Benchmark against industry** (see section above)
3. **Identify churn and expansion drivers**
4. **Create action plan** for improvement
5. **Track monthly** as primary metric
6. **Review in executive meetings** every month

---

## Resources

- **Excel Template**: Download NRR calculator
- **Data Source**: Automated feed from billing system
- **Reporting**: Monthly board deck inclusion
- **Ownership**: VP of Customer Success + CFO

See main README for implementation guides.
