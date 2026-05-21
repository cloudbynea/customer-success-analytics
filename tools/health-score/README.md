# 🏥 Customer Health Score Tool

**Multi-factor churn prediction engine** | Predict at-risk customers before they leave

---

## Overview

The Health Score is a predictive model that combines multiple factors to assess customer health and churn risk on a 0-100 scale. Early identification of at-risk customers enables proactive retention efforts.

### Key Benefits
- ✅ **Predict churn 60-90 days early**
- ✅ **Prioritize CS efforts** on highest-risk accounts
- ✅ **Reduce unplanned churn** by 15-25%
- ✅ **Identify expansion opportunities** in healthy accounts

---

## The Health Score Formula

```
Health Score = (Feature_Adoption × 0.30) + 
               (Support_Sentiment × 0.20) + 
               (Payment_Health × 0.25) +
               (Engagement_Velocity × 0.25)
```

### Components Explained

| Component | Weight | Range | What It Measures |
|-----------|--------|-------|------------------|
| **Feature Adoption** | 30% | 0-100 | % of features used by customer |
| **Support Sentiment** | 20% | 0-100 | Satisfaction from support tickets |
| **Payment Health** | 25% | 0-100 | Payment history + billing disputes |
| **Engagement Velocity** | 25% | 0-100 | Growth in usage month-over-month |

---

## Risk Levels

| Score | Status | Risk | Recommended Action |
|-------|--------|------|-------------------|
| 80-100 | ✅ Healthy | Low | Nurture for expansion |
| 60-79 | ⚠️ At-Risk | Medium | Schedule QBR, check for unmet needs |
| 40-59 | 🔴 Critical | High | Executive outreach, custom demo |
| <40 | 💀 At-Extreme-Risk | Very High | Intervention plan, CEO call |

---

## Implementation Guide

### Step 1: Prepare Your Data

Required columns in your CSV:
```
customer_id,account_name,features_adopted,features_total,
support_tickets_last_30d,support_satisfaction_avg,
billing_status,last_payment_date,months_overdue,
login_count_current_month,login_count_previous_month
```

### Step 2: Install Dependencies

```bash
pip install pandas numpy scikit-learn
```

### Step 3: Calculate Health Scores

```python
import pandas as pd
from health_score_calculator import HealthScoreCalculator

# Load your data
df = pd.read_csv('customer_data.csv')

# Initialize calculator
calculator = HealthScoreCalculator()

# Calculate scores
df['health_score'] = df.apply(calculator.calculate_score, axis=1)
df['risk_level'] = df['health_score'].apply(calculator.get_risk_level)

# Export results
df.to_csv('customers_with_health_scores.csv', index=False)

# Print summary
print(calculator.generate_report(df))
```

---

## Calculation Details

### Feature Adoption (30%)
```python
adoption_score = (features_adopted / features_total) * 100
```
- 100% adoption = 100 points
- 50% adoption = 50 points
- 0% adoption = 0 points

### Support Sentiment (20%)
```python
# Based on CSAT scores from tickets
sentiment_score = average_csat_score
if complaints_ratio > 0.3:
    sentiment_score *= 0.7  # Penalty for high complaints
```
- Average CSAT = 4.5/5 = 90 points
- Any billing complaints = -20 points
- Multiple unresolved tickets = -10 points each

### Payment Health (25%)
```python
payment_score = 100
if months_overdue > 0:
    payment_score -= 50  # Major red flag
if payment_disputes > 0:
    payment_score -= 20 * payment_disputes
if last_payment_date is None:
    payment_score = 0  # Inactive
```

### Engagement Velocity (25%)
```python
# Month-over-month login growth
growth_rate = (current_logins - previous_logins) / previous_logins
velocity_score = min(100, max(0, 50 + (growth_rate * 50)))
```
- +30% growth = 90 points
- Flat usage = 50 points
- -20% decline = 20 points
- No usage = 0 points

---

## Advanced Features

### Churn Prediction Model

```python
from churn_predictor import ChurnPredictor

predictor = ChurnPredictor()
predictor.train(training_data)

# Predict churn probability for each customer
df['churn_probability'] = predictor.predict_probability(df)

# Identify customers likely to churn in next 90 days
at_risk = df[df['churn_probability'] > 0.6]
```

### Expected Lifetime Value (ELV)

```python
def calculate_elv(customer):
    arpu = customer['mrr']
    retention_rate = customer['health_score'] / 100
    months_remaining = 12 / (1 - retention_rate + 0.001)
    gross_margin = customer['gross_margin']
    
    return arpu * months_remaining * gross_margin
```

---

## Example Output

```
CUSTOMER HEALTH SCORE REPORT
Generated: 2024-03-15

Total Customers: 250
Healthy (80+): 150 (60%)
At-Risk (60-79): 75 (30%)
Critical (<60): 25 (10%)

CRITICAL ALERT - TOP 5 AT-RISK CUSTOMERS:
1. Acme Corp (Health: 35, MRR: $5,000/mo)
   - Issue: Low feature adoption (10%)
   - Recommended: Onboarding refresh

2. TechFlow Inc (Health: 42, MRR: $3,000/mo)
   - Issue: Payment overdue 45 days
   - Recommended: AR collection + discount negotiation

3. Global Solutions (Health: 38, MRR: $8,000/mo)
   - Issue: 40% usage decline this month
   - Recommended: Executive QBR

4. DataCorp Ltd (Health: 45, MRR: $2,500/mo)
   - Issue: High support complaint ratio
   - Recommended: Product training session

5. StartupXYZ (Health: 50, MRR: $1,200/mo)
   - Issue: No logins in 30 days
   - Recommended: Win-back campaign

EXPANSION OPPORTUNITIES - HIGH VALUE HEALTHY CUSTOMERS:
1. Enterprise Plus (Health: 95, MRR: $10,000/mo)
   - Recommendation: Upsell premium features
   - Potential uplift: +$2,000/mo
```

---

## Dashboard Integration

### HTML Dashboard Template

```html
<!DOCTYPE html>
<html>
<head>
    <title>Customer Health Score Dashboard</title>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .metric { display: inline-block; padding: 20px; margin: 10px; border-radius: 8px; }
        .healthy { background: #d4edda; }
        .at-risk { background: #fff3cd; }
        .critical { background: #f8d7da; }
    </style>
</head>
<body>
    <h1>📊 Customer Health Score Dashboard</h1>
    
    <div id="summary">
        <div class="metric healthy">
            <h3>Healthy</h3>
            <p id="healthy-count">0</p>
        </div>
        <div class="metric at-risk">
            <h3>At-Risk</h3>
            <p id="atrisk-count">0</p>
        </div>
        <div class="metric critical">
            <h3>Critical</h3>
            <p id="critical-count">0</p>
        </div>
    </div>
    
    <div id="chart"></div>
    
    <script>
        // Load and visualize health scores
        fetch('health_scores.json')
            .then(r => r.json())
            .then(data => {
                // Distribution chart
                const scores = data.map(d => d.health_score);
                const trace = {
                    x: scores,
                    type: 'histogram',
                    nbinsx: 20
                };
                Plotly.newPlot('chart', [trace]);
                
                // Update counts
                document.getElementById('healthy-count').textContent = 
                    data.filter(d => d.health_score >= 80).length;
                document.getElementById('atrisk-count').textContent = 
                    data.filter(d => 60 <= d.health_score < 80).length;
                document.getElementById('critical-count').textContent = 
                    data.filter(d => d.health_score < 60).length;
            });
    </script>
</body>
</html>
```

---

## Best Practices

### 1. Update Frequency
- **Daily**: Feature usage, logins
- **Weekly**: Support tickets, payments
- **Monthly**: Overall score recalculation

### 2. Threshold Tuning
Adjust weights based on your business:
- **High-touch SaaS**: Increase support sentiment weight
- **Self-service SaaS**: Increase feature adoption weight
- **Enterprise**: Increase payment health weight

### 3. Actions by Risk Level

**Healthy (80+)**
- [ ] Add to expansion/upsell list
- [ ] Feature adoption training
- [ ] Invite to advisory board

**At-Risk (60-79)**
- [ ] Schedule health check call
- [ ] Identify unmet needs
- [ ] Offer discount for annual commitment

**Critical (<60)**
- [ ] Executive outreach within 48 hours
- [ ] Create intervention plan
- [ ] Assign dedicated CS resource

### 4. Common Pitfalls
- ❌ Using only one factor (e.g., just churn rate)
- ❌ Not updating data regularly
- ❌ Ignoring positive signals in low-score accounts
- ✅ Validate predictions with actual churn data
- ✅ Adjust weights based on results
- ✅ Use as input, not final decision

---

## API Integration

### Salesforce

```python
from simple_salesforce import Salesforce

sf = Salesforce(
    username='your_email',
    password='your_password',
    security_token='your_token'
)

for customer in customers:
    sf.Account.update(
        customer['sf_id'],
        {'Health_Score__c': customer['health_score']}
    )
```

### HubSpot

```python
import requests

API_KEY = 'your_hubspot_api_key'

for customer in customers:
    requests.patch(
        f"https://api.hubapi.com/crm/v3/objects/companies/{customer['hubspot_id']}",
        headers={'Authorization': f'Bearer {API_KEY}'},
        json={'properties': [
            {'name': 'health_score', 'value': str(customer['health_score'])}
        ]}
    )
```

---

## Troubleshooting

**Q: All scores are too low**
A: Check if all customers have complete data. Missing values default to 0.

**Q: Scores don't match my intuition**
A: Adjust component weights in `calculator.weights` dict

**Q: How do I handle new customers?**
A: Set trial period to 90 days before including in model

---

## Next Steps

1. Gather historical data (3-6 months)
2. Calculate baseline scores
3. Compare to actual churn
4. Calibrate weights
5. Deploy to production
6. Review weekly

See main README for integration instructions.
