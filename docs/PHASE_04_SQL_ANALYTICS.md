# Phase 4 – SQL Analytics Layer

## Overview
In this phase, analytical views were created in Amazon Athena to transform raw marketing data into business-ready metrics.

---

## Views Created

### 1. v_channel_funnel
Calculates:
- Views
- Add-to-cart
- Purchases
- Conversion rate per channel

Purpose:
Measure funnel efficiency and drop-off points.

---

### 2. v_channel_roas
Calculates:
- Revenue per channel
- Total ad spend per channel
- Return on Ad Spend (ROAS)

Purpose:
Evaluate marketing cost efficiency.

---

### 3. v_high_value_customers
Identifies:
- Top 20% customers by lifetime revenue
- Revenue distribution concentration

Purpose:
Support customer segmentation and retention strategy.

---

## Campaign Lift Analysis
Performed 30-day before/after comparison around campaign launch (CMP008).

Result:
~77% increase in paid social revenue post-launch.

---

## Key Technical Learnings
- Resolved join inflation from mismatched aggregation levels
- Fixed CSV encoding issues (UTF-8 requirement)
- Handled invalid numeric parsing in Athena
- Implemented date parsing using date_parse() and regexp_extract()
- Created reusable SQL views for downstream BI tools
