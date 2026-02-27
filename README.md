Marketing Analytics Cloud Project

Overview
This project demonstrates an end-to-end serverless marketing analytics pipeline built on AWS.

The goal was to analyze funnel performance, evaluate channel efficiency (ROAS), and measure campaign lift using a controlled before-and-after experiment.

Architecture

Microsoft Excel – Data cleaning
Amazon S3 – Data lake storage
Amazon Athena – SQL modeling & analysis
Amazon QuickSight – Executive dashboards

Key Analyses

1. Funnel Performance

Email achieved highest conversion rate (~4.9%). 
Organic drove highest traffic but lowest conversion. 
Paid Social underperformed in funnel efficiency. 

2. ROAS Analysis

Referral channel generated highest ROAS (0.58).
Paid Search and Paid Social showed inefficient spend allocation.
Significant budget optimization opportunity identified.

3. Campaign Lift Analysis (CMP008)

30-day controlled experiment.
Paid Social revenue increased from $946 → $1,680.
77.4% revenue lift post-launch.

Business Recommendations

1. Reallocate budget toward high-ROAS referral campaigns
2. Optimize paid social targeting for improved efficiency
3. Scale email marketing due to high conversion performance
4. ontinue structured A/B testing for campaign measurement

Skills Demonstrated

SQL (Athena)
Data Modeling
Cloud Data Engineering (S3 + Athena)
BI Dashboarding (QuickSight)
Marketing KPI Analysis
Controlled Experiment Analysis
Revenue Lift Measurement
ROAS Optimization

## 📌 Technical Summary

**Serverless AWS Stack:**
- Amazon S3 → Data Lake (raw/clean/optimized)
- Amazon Athena → SQL modeling + semantic views
- Parquet CTAS tables → SPICE optimization
- Amazon QuickSight → Executive dashboards

**Key analytics:**
- Funnel conversion by channel
- Channel ROAS with KPI ranking
- Controlled campaign lift analysis (77.4% lift)
- High-value customer segmentation

