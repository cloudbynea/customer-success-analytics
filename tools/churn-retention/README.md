# 📉 Churn & Retention Analysis Tool

**Cohort analysis + CLV + Payback period** | Understand customer economics

---

## Overview

This tool analyzes retention patterns through cohorts, calculates Customer Lifetime Value, and determines payback period—the three pillars of sustainable SaaS business economics.

### Why These Three Metrics Matter

| Metric | What It Shows | Business Impact |
|--------|---|---|
| **Cohort Retention** | How well you keep customers over time | Plan expansion budget |
| **CLV** | Total profit per customer | Justify CAC spending |
| **Payback Period** | Months to recover acquisition cost | Cash flow planning |

---

## Part 1: Cohort Retention Analysis

### What is a Cohort?

A cohort is a group of customers acquired in the same time period (month, quarter, year). Analyzing by cohort removes the noise of new customer acquisition and shows true retention.

### Cohort Table Example

```
                Jan 2024    Feb 2024    Mar 2024    Apr 2024
Jan Cohort      100%        92%         85%         78%
Feb Cohort              100%        91%         83%
Mar Cohort                      100%        90%
Apr Cohort                              100%

Reading: Jan cohort retained 78% of customers 3 months later
```

### How to Build a Cohort Table

```python
import pandas as pd
import numpy as np

def create_cohort_table(data, cohort_key, period_key):
    """
    Create a cohort retention table
    
    Args:
        data: DataFrame with customer data
        cohort_key: column name for cohort month
        period_key: column name for observation month
    """
    # Add cohort index
    cohort_data = data.groupby([cohort_key, period_key]).size().reset_index(name='count')
    cohort_pivot = cohort_data.pivot_table(
        index=cohort_key,
        columns=period_key,
        values='count'
    )
    
    # Calculate retention (divide by first month)
    cohort_size = cohort_pivot.iloc[:, 0]
    cohort_retention = cohort_pivot.divide(cohort_size, axis=0) * 100
    
    return cohort_retention

# Usage
customers = pd.read_csv('customers.csv')
cohort_table = create_cohort_table(
    customers,
    cohort_key='signup_month',
    period_key='observation_month'
)

print(cohort_table)
# Output:
#              Month 1  Month 2  Month 3  Month 4  Month 5
# Jan 2024     100.0    92.0     85.0     78.0     72.0
# Feb 2024     100.0    91.0     83.0     76.0     NaN
# Mar 2024     100.0    90.0     82.0     NaN      NaN
```

### Reading the Cohort Table

```
What to look for:
✅ First month = 100% (baseline)
✅ Declining column = steeper churn = retention problem
⚠️ Flat rows = one cohort performs differently
🔴 Columns declining faster = product decline
✅ Columns declining slower = improving product
```

### Cohort Retention Curves

```python
import matplotlib.pyplot as plt

def plot_cohort_curves(cohort_table):
    """Plot retention curves for each cohort"""
    plt.figure(figsize=(12, 6))
    
    for cohort in cohort_table.index:
        plt.plot(cohort_table.columns, cohort_table.loc[cohort], 
                marker='o', label=str(cohort))
    
    plt.xlabel('Months Since Signup')
    plt.ylabel('Retention %')
    plt.title('Cohort Retention Curves')
    plt.legend()
    plt.grid(True, alpha=0.3)
    plt.ylim([0, 105])
    plt.show()

# Result: Visual comparison of how each cohort performs
```

### Interpreting Retention Patterns

```
Pattern 1: HEALTHY RETENTION
Month 1: 100% → Month 3: 90% → Month 6: 85% → Month 12: 80%
↳ Gradual decay is normal
↳ Long tail retention (80% at 12mo) is excellent

Pattern 2: CLIFF CHURN
Month 1: 100% → Month 2: 70% → Month 3: 68% → Month 6: 65%
↳ Big drop early = onboarding problem
↳ Stabilizes = product is good once they understand it

Pattern 3: DECLINING PRODUCT
Jan Cohort: 100% → 80% → 60% → 40%
Feb Cohort: 100% → 75% → 50% → 25%
Mar Cohort: 100% → 70% → 40% → 15%
↳ Each cohort worse than previous
↳ Product quality or market issue

Pattern 4: IMPROVING PRODUCT
Jan Cohort: 100% → 85% → 65%
Feb Cohort: 100% → 88% → 70%
Mar Cohort: 100% → 91% → 75%
↳ Each cohort better = improvements working
↳ Keep doing what you're doing
```

---

## Part 2: Customer Lifetime Value (CLV)

### What is CLV?

CLV is the total profit you'll make from a customer over their entire relationship.

### Simple CLV Formula

```
CLV = (ARPU × Customer Lifespan in Months × Gross Margin) - CAC
```

Where:
- **ARPU** = Average Revenue Per User (monthly)
- **Lifespan** = Expected months as customer
- **Gross Margin** = % of revenue left after COGS
- **CAC** = Customer Acquisition Cost

### Example Calculation

```
Input:
- Monthly ARPU: $100
- Expected customer lifetime: 24 months
- Gross margin: 70%
- Customer acquisition cost: $1,200

Calculation:
CLV = ($100 × 24 × 0.70) - $1,200
CLV = $1,680 - $1,200
CLV = $480 per customer

Interpretation:
Each customer should generate $480 profit
If you're spending more to acquire them, fix it
```

### Advanced CLV (With Churn)

```python
def calculate_clv_advanced(
    monthly_arpu,
    monthly_churn_rate,
    gross_margin,
    cac,
    discount_rate=0.1
):
    """
    Calculate CLV accounting for churn
    
    Uses survival probability to determine expected lifespan
    """
    # Calculate average customer lifetime
    lifetime_months = 1 / (monthly_churn_rate / 100)
    
    # Calculate total revenue (accounting for gradual customer loss)
    total_revenue = 0
    for month in range(int(lifetime_months * 2)):  # Calculate up to 2x lifetime
        survival_prob = (1 - monthly_churn_rate/100) ** month
        discount_factor = 1 / ((1 + discount_rate/100) ** month)
        monthly_revenue = monthly_arpu * survival_prob * discount_factor
        total_revenue += monthly_revenue
    
    gross_profit = total_revenue * (gross_margin / 100)
    clv = gross_profit - cac
    
    return {
        'clv': round(clv, 2),
        'lifetime_months': round(lifetime_months, 1),
        'total_revenue': round(total_revenue, 2),
        'gross_profit': round(gross_profit, 2)
    }

# Example
result = calculate_clv_advanced(
    monthly_arpu=100,
    monthly_churn_rate=3,  # 3% monthly churn
    gross_margin=70,
    cac=1200,
    discount_rate=10  # 10% discount rate
)

print(f"CLV: ${result['clv']}")
print(f"Lifetime: {result['lifetime_months']} months")
print(f"Total Revenue: ${result['total_revenue']}")
```

### LTV:CAC Ratio (Most Important)

```
LTV:CAC = CLV / CAC

Interpretation:
- <1:1 = Bad (losing money on each customer)
- 1:1 = Break even
- 2:1 = Acceptable
- 3:1 = Healthy/Sustainable
- 5:1+ = Excellent (can invest more in sales)

Example:
CLV = $5,000
CAC = $1,500
Ratio = 3.3:1 ✅ Sustainable

Rule: Never spend $1 in CAC if CLV < $3
```

---

## Part 3: Payback Period

### What is Payback Period?

Payback period is how many months it takes to recover the cost of acquiring a customer from the profit they generate.

### Formula

```
Payback Period = CAC / Monthly Profit per Customer

Where:
Monthly Profit = ARPU × Gross Margin

Example:
CAC = $1,200
ARPU = $100
Gross Margin = 70%
Monthly Profit = $100 × 0.70 = $70

Payback = $1,200 / $70 = 17.1 months

Interpretation: Takes 17 months to break even on customer acquisition
```

### Payback Period Benchmarks

| Payback | Status | Action |
|---------|--------|--------|
| <6 months | 🚀 Excellent | Can invest more in sales |
| 6-12 months | ✅ Good | Sustainable, room to grow |
| 12-18 months | ⚠️ Long | Acceptable but limits growth |
| 18-24 months | 🔴 Very Long | Bottleneck to growth |
| >24 months | ❌ Unsustainable | Fix pricing or margins |

### Calculating Your Payback Period

```python
def calculate_payback_period(cac, monthly_arpu, gross_margin):
    """
    Calculate payback period in months
    """
    monthly_profit = monthly_arpu * (gross_margin / 100)
    
    if monthly_profit <= 0:
        return float('inf')
    
    payback_months = cac / monthly_profit
    return payback_months

# Example for different scenarios
scenarios = [
    {'name': 'SMB Plan', 'cac': 500, 'arpu': 50, 'margin': 70},
    {'name': 'Mid-Market', 'cac': 2500, 'arpu': 200, 'margin': 75},
    {'name': 'Enterprise', 'cac': 5000, 'arpu': 1000, 'margin': 80},
]

for scenario in scenarios:
    payback = calculate_payback_period(
        scenario['cac'],
        scenario['arpu'],
        scenario['margin']
    )
    print(f"{scenario['name']}: {payback:.1f} months")

# Output:
# SMB Plan: 14.3 months
# Mid-Market: 16.7 months
# Enterprise: 6.3 months
```

### Improving Payback Period

**Option 1: Reduce CAC** (Fastest results)
```
Current: CAC $1,200, Payback 17 months
Reduce CAC by 30% → CAC $840, Payback 12 months ✅
Actions: Optimize sales process, referral program, ABM
```

**Option 2: Increase ARPU**
```
Current: ARPU $100, Payback 17 months
Increase to $150 → Payback 11 months ✅
Actions: Upsell, higher pricing tier, bundle products
```

**Option 3: Improve Margins**
```
Current: Margin 70%, Payback 17 months
Increase to 80% → Payback 15 months ✅
Actions: Automation, efficiency, reduce support costs
```

---

## Complete Analysis Dashboard

```python
class RetentionAnalyzer:
    """Complete cohort, CLV, and payback analysis"""
    
    def __init__(self, customer_data):
        self.df = customer_data
    
    def generate_report(self):
        """Generate comprehensive retention report"""
        
        # Cohort analysis
        cohort = self.create_cohort_table()
        
        # CLV by segment
        clv_analysis = self._calculate_clv_by_segment()
        
        # Payback metrics
        payback = self._calculate_payback()
        
        return {
            'cohort_table': cohort,
            'clv_analysis': clv_analysis,
            'payback_period': payback,
            'recommendation': self._get_recommendation(cohort, clv_analysis)
        }
    
    def _calculate_clv_by_segment(self):
        """CLV for each customer segment"""
        return self.df.groupby('segment').apply(
            lambda g: calculate_clv_advanced(
                g['monthly_arpu'].mean(),
                g['monthly_churn'].mean(),
                75,  # gross margin
                g['cac'].mean()
            )
        )
    
    def _calculate_payback(self):
        """Payback period analysis"""
        return {
            'overall': calculate_payback_period(
                self.df['cac'].mean(),
                self.df['monthly_arpu'].mean(),
                75
            ),
            'by_segment': self.df.groupby('segment').apply(
                lambda g: calculate_payback_period(
                    g['cac'].mean(),
                    g['monthly_arpu'].mean(),
                    75
                )
            )
        }
    
    def _get_recommendation(self, cohort, clv):
        """Actionable recommendations"""
        recommendations = []
        
        # Check cohort health
        latest_retention = cohort.iloc[:, -1].mean()
        if latest_retention < 70:
            recommendations.append("⚠️ Retention below 70% - implement health scoring")
        
        # Check CLV
        if clv['clv'].mean() < 0:
            recommendations.append("🔴 Negative CLV - raise prices or reduce CAC")
        
        return recommendations

# Usage
analyzer = RetentionAnalyzer(customer_data)
report = analyzer.generate_report()
```

---

## Best Practices

### 1. Cohort Analysis Dos ✅
- [ ] Track cohorts monthly
- [ ] Compare 3+ months of data
- [ ] Look for seasonal patterns
- [ ] Segment by acquisition channel
- [ ] Compare product versions

### 2. CLV Dos ✅
- [ ] Update CLV quarterly
- [ ] Calculate by segment
- [ ] Compare to CAC
- [ ] Use for pricing decisions
- [ ] Track trends over time

### 3. Payback Period Dos ✅
- [ ] Target <12 months
- [ ] Calculate by channel
- [ ] Monitor weekly
- [ ] Set payback goals
- [ ] Use to cap CAC spending

---

## Troubleshooting

**Q: Cohort retention looks bad at Month 2**
A: Normal. Most churn happens in first 90 days. Focus on onboarding.

**Q: CLV is negative despite paying customers**
A: CAC too high. Either raise prices, reduce sales spend, or improve margins.

**Q: Payback period is 24+ months**
A: Unsustainable. You can't fund this growth model. Fix pricing or CAC.

---

## Template: Monthly Review Checklist

```
MONTHLY RETENTION REVIEW CHECKLIST
Month: _________

Cohort Health
☐ Reviewed newest cohort retention (Month 1)
☐ Compared to previous month's cohort
☐ Checked for cliff churn (Month 1→2)
☐ Identified which cohorts underperforming
☐ Root-caused top 3 issues

CLV Analysis
☐ Calculated CLV for each segment
☐ Updated CLV vs CAC ratio
☐ Reviewed high-value customer patterns
☐ Identified expansion opportunities
☐ Benchmarked against industry

Payback Period
☐ Calculated current payback
☐ Compared to target (<12 months)
☐ Analyzed by acquisition channel
☐ Reviewed CAC spend levels
☐ Recommended budget adjustments

Action Items
☐ _______________________
☐ _______________________
☐ _______________________

Next Month Goals
☐ Target retention: _____
☐ Target CLV: _____
☐ Target payback: _____ months
```

---

## Next Steps

1. **Gather 6+ months of data**
2. **Build cohort table**
3. **Calculate CLV for top segments**
4. **Analyze payback period**
5. **Create action plan**
6. **Review monthly**

See main README for integration and deployment.
