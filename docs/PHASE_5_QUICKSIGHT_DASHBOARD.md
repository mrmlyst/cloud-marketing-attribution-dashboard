Objective

Transform modeled Athena datasets into executive-ready dashboards using Amazon QuickSight while ensuring:

Metric consistency with SQL layer

Controlled experimental filtering

Performance optimization via SPICE

Clean visualization hierarchy

Business-driven storytelling

This phase represents the final analytical delivery layer of the cloud pipeline.

 Dataset Strategy
Data Source Used

Amazon Athena → project1_marketing_db

Rather than performing complex joins inside QuickSight, modeling was centralized in Athena. QuickSight consumed pre-modeled views and optimized Parquet tables.

This ensured:

Single source of truth

Reusable SQL logic

Reduced BI layer complexity

Easier debugging

SPICE Import Configuration

Datasets were imported into SPICE (Super-fast, Parallel, In-memory Calculation Engine) for:

Faster rendering

Reduced repeated Athena scans

Improved dashboard performance

Lower cost

SPICE was selected instead of direct query mode to simulate production BI environments.


Insert dataset import screen:

/docs/screenshots/quicksight_spice_import.png
 Campaign Lift Modeling Inside QuickSight

Although SQL modeled the base data, additional business logic was implemented in QuickSight.

Period Classification

A calculated field was created:

ifelse(
  {session_date} < parseDate("2025-12-01","yyyy-MM-dd"),
  "Before",
  "After"
)

This dynamically classified sessions relative to campaign launch date.

Revenue Lift Formula

Calculated field:

(
  sum(ifelse({period} = "After", {order_total}, 0))
  -
  sum(ifelse({period} = "Before", {order_total}, 0))
)
/
sum(ifelse({period} = "Before", {order_total}, 0))

This ensured lift was computed at aggregation level rather than row level.

Result:

77.42% Revenue Increase

Controlled Window Filtering

To eliminate seasonal bias:

Filters applied:

Channel = paid_social

session_date between 2025-11-01 and 2025-12-31

This simulated a structured experimental design.

 Funnel Performance Dashboard

Dataset: v_channel_funnel

Metrics included:

Views

Add-to-Cart

Purchases

Conversion Rate

Conversion rate calculation (SQL-layer):

purchases / views

Insights:

Email highest efficiency

Organic highest traffic, lowest conversion

Paid Social underperforming in funnel stage

Visual Types:

Vertical bar chart (conversion)

Stacked bar (funnel volume)

 ROAS Performance Dashboard

Dataset: v_roas_performance

ROAS Definition:

revenue / total_spend

Visualizations:

ROAS by Channel (sorted descending)

Revenue vs Ad Spend comparison

KPI showing Top Channel by ROAS

Top Performer:

Referral → ROAS 0.58

 Executive Dashboard Design

A consolidated executive dashboard was built to integrate:

Top Channel ROAS KPI

Overall Conversion Rate KPI

Total Revenue KPI

Campaign Lift KPI

ROAS by Channel

Revenue vs Spend

Conversion Rate by Channel

Paid Social Campaign Lift Chart

This layout follows a hierarchy:

Top Row → Executive KPIs
Middle Row → Channel Efficiency
Bottom Row → Behavioral + Experimental Analysis

Dashboard Interaction Layer

A date filter control was added to enable dynamic analysis.

This provides:

Scenario testing

Temporal comparison

Stakeholder flexibility

 BI Debugging Challenges Resolved

During implementation, the following were addressed:

Issue: SPICE Import SQL Errors

Solution:

Created CTAS Parquet tables in Athena

Simplified QuickSight dataset ingestion

Issue: Revenue mismatch (1608 vs 1680)

Root Cause:

End date not included in QuickSight filter

Solution:

Corrected inclusive date boundary

Issue: Join Inflation

Solution:

Centralized join logic in Athena

Avoided BI-layer duplication

 Performance & Cost Considerations

Optimization strategies implemented:

Parquet format via CTAS

SPICE for in-memory BI performance

Avoided repeated complex parsing in BI

Manual table creation for schema control

This mirrors real-world production BI optimization.

 Final Business Insights

Referral channel most cost-efficient.

Paid Social inefficient but responsive to structured campaign lift.

Email channel demonstrates strongest conversion efficiency.

Budget reallocation opportunities identified.

Controlled experimentation confirms campaign effectiveness.

 Skills Demonstrated

Cloud-native BI architecture

SPICE optimization

Controlled experiment design

Revenue lift computation

Marketing KPI modeling

Dataset debugging

Athena + QuickSight integration

Filter scoping & aggregation logic

 Outcome

The project culminated in a production-style executive marketing dashboard capable of:

Identifying channel inefficiencies

Quantifying campaign ROI

Enabling strategic budget decisions

Delivering interactive analytics in a serverless cloud environment

Architecture Layer Position

QuickSight represents the final analytical delivery layer in the cloud pipeline:

Excel → S3 → Athena → QuickSight
