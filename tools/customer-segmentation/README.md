# 👥 Customer Segmentation Tool

**RFM Analysis + Profit-based segmentation** | Tailor your strategy to each customer

---

## Overview

Customer segmentation allows you to group similar customers and treat each segment differently. Two methods:

1. **RFM Segmentation**: Based on purchase behavior (Recency, Frequency, Monetary)
2. **Profit Segmentation**: Based on customer profitability and growth potential

---

## Part 1: RFM Segmentation

### What is RFM?

RFM stands for:
- **R (Recency)**: How recently did they transact?
- **F (Frequency)**: How often do they transact?
- **M (Monetary)**: How much do they spend?

Each customer gets a score (1-5) on each dimension, resulting in 9 possible segments.

### RFM Scoring Explained

#### Recency (How recent is their last purchase?)

```
Score 5: ≤30 days ago (Very recent)
Score 4: 31-60 days ago (Recent)
Score 3: 61-120 days ago (Moderate)
Score 2: 121-180 days ago (Old)
Score 1: >180 days ago (Very old)

Example:
- Last purchase: 15 days ago → Score 5 (RFM: 5__)
- Last purchase: 90 days ago → Score 3 (RFM: 3__)
```

#### Frequency (How often do they purchase?)

```
Score 5: 10+ purchases (Very frequent)
Score 4: 6-9 purchases (Frequent)
Score 3: 4-5 purchases (Moderate)
Score 2: 2-3 purchases (Rare)
Score 1: 1 purchase (First-time)

Example:
- 12 purchases → Score 5 (RFM: _5_)
- 4 purchases → Score 3 (RFM: _3_)
```

#### Monetary (How much do they spend?)

```
Score 5: Top 20% spenders
Score 4: 21-40% spenders
Score 3: 41-60% spenders
Score 2: 61-80% spenders
Score 1: Bottom 20% spenders

Example:
- Customer: $50,000 lifetime value → Score 5 (RFM: __5)
- Customer: $5,000 lifetime value → Score 2 (RFM: __2)
```

### RFM Segments (9 Total)

```
RFM Score | Segment Name      | Size | Action
-----------|------------------|------|---------------------------
5-5-5      | Champions         | 5%   | VIP treatment, loyalty rewards
4-4-5      | Loyal Customers   | 10%  | Upsell, cross-sell, retention
4-3-4      | Potential         | 15%  | Engagement campaigns
3-3-3      | Promising         | 20%  | Move towards Champions
3-2-3      | At-Risk           | 10%  | Win-back campaigns
2-2-2      | Hibernating       | 15%  | Reactivation offers
2-2-1      | Slipping Away     | 10%  | Last-chance campaigns
1-1-1      | Lost              | 10%  | Don't bother
1-1-2      | Can't Lose Them   | 5%   | High-value rescue mission
```

### Calculate RFM in Python

```python
import pandas as pd
from datetime import datetime, timedelta
import numpy as np

class RFMSegmentation:
    def __init__(self, customer_data):
        """
        Args:
            customer_data: DataFrame with columns
            - customer_id
            - last_purchase_date
            - purchase_frequency (count)
            - total_spent (monetary value)
        """
        self.df = customer_data.copy()
        self.reference_date = datetime.now()
    
    def calculate_recency(self):
        """Calculate recency score (1-5)"""
        self.df['last_purchase_date'] = pd.to_datetime(
            self.df['last_purchase_date']
        )
        self.df['days_since_purchase'] = (
            self.reference_date - self.df['last_purchase_date']
        ).dt.days
        
        # Create score (higher is better for recency)
        self.df['r_score'] = pd.cut(
            self.df['days_since_purchase'],
            bins=[0, 30, 60, 120, 180, float('inf')],
            labels=[5, 4, 3, 2, 1],
            include_lowest=True
        ).astype(int)
        
        return self
    
    def calculate_frequency(self):
        """Calculate frequency score (1-5)"""
        self.df['f_score'] = pd.qcut(
            self.df['purchase_frequency'],
            q=5,
            labels=[1, 2, 3, 4, 5],
            duplicates='drop'
        ).astype(int)
        
        return self
    
    def calculate_monetary(self):
        """Calculate monetary score (1-5)"""
        self.df['m_score'] = pd.qcut(
            self.df['total_spent'],
            q=5,
            labels=[1, 2, 3, 4, 5],
            duplicates='drop'
        ).astype(int)
        
        return self
    
    def segment(self):
        """Assign RFM segment based on scores"""
        self.df['rfm_score'] = (
            self.df['r_score'].astype(str) +
            self.df['f_score'].astype(str) +
            self.df['m_score'].astype(str)
        )
        
        # Assign segment names
        segment_map = {
            '555': 'Champions',
            '554': 'Champions',
            '545': 'Champions',
            '544': 'Loyal Customers',
            '455': 'Loyal Customers',
            '454': 'Loyal Customers',
            '445': 'Loyal Customers',
            '444': 'Loyal Customers',
            '434': 'Potential',
            '443': 'Potential',
            '333': 'Promising',
            '323': 'At-Risk',
            '222': 'Hibernating',
            '221': 'Slipping Away',
            '111': 'Lost',
            '112': "Can't Lose Them",
            '121': "Can't Lose Them",
        }
        
        self.df['segment'] = self.df['rfm_score'].map(segment_map)
        # Fill any unmapped segments with closest match
        self.df['segment'].fillna('Promising', inplace=True)
        
        return self
    
    def get_results(self):
        """Return segmented data"""
        return self.df[['customer_id', 'r_score', 'f_score', 
                        'm_score', 'rfm_score', 'segment']]
    
    def get_summary(self):
        """Summary by segment"""
        return self.df.groupby('segment').agg({
            'customer_id': 'count',
            'total_spent': ['sum', 'mean'],
            'days_since_purchase': 'mean'
        }).round(2)

# Usage Example
customers = pd.read_csv('customers.csv')

rfm = RFMSegmentation(customers)
rfm.calculate_recency().calculate_frequency().calculate_monetary().segment()

print(rfm.get_summary())
# Output:
#                count    total_spent  days_since
# Champions        25      $500,000      15 days
# Loyal            50      $300,000      45 days
# Potential        75      $150,000      90 days
# ...
```

### RFM Segment Strategies

#### 1. Champions (5-5-5)
**Goal**: Maximize lifetime value and loyalty

```
Actions:
✅ VIP account management
✅ Early access to new features
✅ Loyalty rewards program
✅ Invite to advisory board
✅ Personalized service

Marketing:
✅ Personalized email campaigns
✅ Exclusive events/webinars
✅ Premium support
✅ Custom solutions
```

#### 2. Loyal Customers (4-4-5)
**Goal**: Upsell and cross-sell

```
Actions:
✅ QBR (Quarterly Business Reviews)
✅ Product training
✅ Case study opportunity
✅ Referral program

Marketing:
✅ Upgrade campaigns
✅ Feature education
✅ Cross-sell bundles
✅ "Expand with us" messaging
```

#### 3. At-Risk (2-3-2)
**Goal**: Save the customer

```
Actions:
✅ Immediate outreach
✅ Executive check-in
✅ Product demo of new features
✅ Special discount offer

Marketing:
✅ Win-back campaigns
✅ "We miss you" messaging
✅ Limited-time offers
✅ Customer success stories
```

#### 4. Lost (1-1-1)
**Goal**: Accept loss, maybe reactivate later

```
Actions:
✅ One final "comeback" offer (optional)
✅ Remove from regular campaigns
✅ Add to "re-engage later" list
✅ Exit survey (if possible)

Marketing:
✅ Minimal email
✅ Only special offers
✅ Annual "we're better now" campaign
```

---

## Part 2: Profit-Based Segmentation

### Segmentation by Profitability + Growth

While RFM is behavioral, profit-based segmentation focuses on economics:

```
           High Growth Potential
           ↓
High Value Accounts | Mid-Value Growth Accounts
Strategic Focus     | Expand carefully
           ↓
Low Value Accounts | Problem Accounts
Optimize Margins    | Manage Risk
           ↓
           Low Growth Potential
```

### Four Quadrants

#### Quadrant 1: HIGH-VALUE ACCOUNTS ⭐
**Definition**: High MRR + High expansion potential + Low churn

```
Profile:
- Annual contract value: $50,000+
- NRR: >110%
- Feature adoption: >70%
- Support ticket satisfaction: >4.5/5

Strategy:
✅ Executive account manager
✅ Strategic partnership approach
✅ Custom solutions
✅ Advisory board involvement
✅ Premium pricing

Expected Outcome:
- Max CLV
- Increased market influence
- Case study/reference
```

#### Quadrant 2: GROWTH ACCOUNTS 📈
**Definition**: Medium MRR + High expansion potential + Low churn

```
Profile:
- Annual contract value: $10,000-$50,000
- NRR: 105-110%
- Feature adoption: 60-70%
- Support satisfaction: 4/5

Strategy:
✅ Proactive CS with clear milestones
✅ Expansion playbooks
✅ Quarterly business reviews
✅ Training and enablement
✅ Clear upgrade path

Expected Outcome:
- Grow into high-value tier
- Predictable expansion revenue
```

#### Quadrant 3: MAINTENANCE ACCOUNTS 🔧
**Definition**: Low MRR + Low expansion potential + Stable

```
Profile:
- Annual contract value: <$10,000
- NRR: 95-105%
- Feature adoption: 40-60%
- Self-service or minimal support

Strategy:
✅ Efficient operations
✅ Self-serve resources
✅ Group webinars
✅ Community support
✅ Lower CAC focus

Expected Outcome:
- Profitable at scale
- Low churn
- Minimal support cost
```

#### Quadrant 4: PROBLEM ACCOUNTS ⚠️
**Definition**: Any combination of churn risk + negative unit economics

```
Profile:
- High support requests
- Frequent billing issues
- Low feature adoption
- Frequent escalations

Strategy:
✅ Assess viability first
✅ Either fix fast or exit gracefully
✅ Don't throw resources at losing prospects
✅ Reduce support burden

Expected Outcome:
- Either win them back or exit
- Avoid loss expansion
```

### Profit-Based Segmentation Code

```python
class ProfitSegmentation:
    def __init__(self, customer_data):
        self.df = customer_data.copy()
    
    def segment(self):
        """Create profit-based segments"""
        
        # Calculate profit metrics
        self.df['annual_value'] = self.df['monthly_mrr'] * 12
        self.df['gross_profit'] = (
            self.df['annual_value'] * self.df['gross_margin'] -
            self.df['support_cost'] - self.df['cogs']
        )
        
        # Growth potential
        self.df['growth_score'] = (
            self.df['feature_adoption'] * 0.3 +
            self.df['nrr'] * 0.4 +
            self.df['expansion_potential'] * 0.3
        )
        
        # Churn risk
        self.df['churn_risk'] = (
            self.df['support_satisfaction'] * -0.3 +
            self.df['feature_adoption'] * -0.2 +
            self.df['payment_health'] * -0.5
        )
        
        # Assign quadrants
        value_median = self.df['annual_value'].median()
        growth_median = self.df['growth_score'].median()
        
        def assign_segment(row):
            if row['annual_value'] > value_median:
                if row['growth_score'] > growth_median:
                    return 'High-Value Accounts'
                else:
                    return 'Maintenance Accounts'
            else:
                if row['growth_score'] > growth_median:
                    return 'Growth Accounts'
                else:
                    return 'Problem Accounts'
        
        self.df['segment'] = self.df.apply(assign_segment, axis=1)
        
        return self
    
    def get_segment_summary(self):
        """Summary statistics by segment"""
        return self.df.groupby('segment').agg({
            'customer_id': 'count',
            'annual_value': ['sum', 'mean'],
            'gross_profit': ['sum', 'mean'],
            'churn_risk': 'mean',
            'growth_score': 'mean'
        }).round(2)

# Usage
customers = pd.read_csv('customers_with_metrics.csv')
seg = ProfitSegmentation(customers)
seg.segment()

print(seg.get_segment_summary())
```

### Allocating Resources by Segment

```
Recommended CS Allocation:

HIGH-VALUE (30% of revenue):
├── Dedicated account manager (1:5 ratio)
├── Executive reviews monthly
├── Custom integrations
└── Budget: 30% of CS resources

GROWTH (40% of revenue):
├── CS manager (1:15 ratio)
├── Quarterly business reviews
├── Playbook-based engagement
└── Budget: 35% of CS resources

MAINTENANCE (25% of revenue):
├── Group CS manager (1:50+ ratio)
├── Self-service focus
├── Webinar-based training
└── Budget: 25% of CS resources

PROBLEM (5% of revenue):
├── Triage only
├── Rescue mission if viable
├── Otherwise, exit plan
└── Budget: 10% of CS resources
```

---

## Combined Segmentation: RFM + Profit

### Example: Champions (RFM) in Growth Accounts (Profit)

```
Characteristics:
- Recent + Frequent + High spending (RFM)
- Medium value + high growth potential (Profit)

Action:
✅ They're expanding fast
✅ Capitalize on momentum
✅ Clear upsell path
✅ Prepare for high-value transition

Expected: 6-12 months to high-value tier
```

---

## Dashboard Template

```html
<!DOCTYPE html>
<html>
<head>
    <title>Customer Segmentation Dashboard</title>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>
<body>
    <h1>👥 Customer Segmentation</h1>
    
    <h2>RFM Segments</h2>
    <div id="rfm-chart"></div>
    
    <h2>Profit Segments</h2>
    <div id="profit-chart"></div>
    
    <h2>Segment Actions</h2>
    <div id="actions"></div>
    
    <script>
        // RFM distribution pie chart
        const rfm_data = [{
            labels: ['Champions', 'Loyal', 'Potential', 'At-Risk', 'Lost'],
            values: [25, 75, 150, 50, 50],
            type: 'pie'
        }];
        
        Plotly.newPlot('rfm-chart', rfm_data);
        
        // Profit segments scatter
        const profit_data = [{
            x: [15, 25, 35, 45],
            y: [50, 40, 25, 10],
            mode: 'markers',
            type: 'scatter',
            marker: {size: 12}
        }];
        
        Plotly.newPlot('profit-chart', profit_data, {
            xaxis: {title: 'Annual Value'},
            yaxis: {title: 'Growth Score'}
        });
    </script>
</body>
</html>
```

---

## Best Practices

### 1. Frequency of Segmentation
- [ ] Calculate RFM monthly
- [ ] Recalculate profit segments quarterly
- [ ] Review strategy alignment quarterly
- [ ] Adjust thresholds annually

### 2. Segment Alignment
- [ ] Same customer shouldn't be in conflicting segments
- [ ] Use dominant metric if conflict (usually value)
- [ ] Document segment definitions
- [ ] Communicate changes to team

### 3. Action Items
- [ ] Create playbook for each segment
- [ ] Define success metrics
- [ ] Assign ownership
- [ ] Track results

---

## Troubleshooting

**Q: All customers fall into one segment**
A: Your thresholds might be too generous. Adjust bins.

**Q: Segments don't match my intuition**
A: RFM is mathematical. Review with sales/CS team.

**Q: How often should I re-segment?**
A: RFM monthly, Profit quarterly. More frequent = more churn.

---

## Next Steps

1. **Choose segmentation method** (RFM, Profit, or both)
2. **Calculate segments** for your customer base
3. **Create playbooks** for each segment
4. **Assign ownership** to CS team
5. **Track results** monthly
6. **Adjust strategy** based on results

See main README for deployment.
