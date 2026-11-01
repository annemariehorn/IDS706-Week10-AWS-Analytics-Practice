# Week 10 AWS Practice: Amazon Quick, Redshift, and Athena

**Course:** IDS 706 — Data Engineering Systems, Duke University  
**Workshop:** Better Together: Amazon Quick, Redshift and Athena — Duke University, October 2026  
**AWS Region:** `us-east-1` (N. Virginia)  
**Submission:** Lab documentation and screenshots

## Overview

This workshop explored an end-to-end analytics stack combining Amazon Redshift for warehousing, Amazon Athena for SQL over S3, and Amazon Quick (QuickSight) for interactive business intelligence and conversational analytics. The work covered Redshift data loading and materialized views, dashboard authoring, AWS MCP-assisted natural-language exploration, an Athena-backed lakehouse, an operational hospital BI scenario, and a comparison of Athena direct queries, Iceberg, and SPICE.

The sections below follow the five 20-point lab tracks in the assignment rubric. Screenshots are linked directly under the corresponding activities. All screenshots are from the workshop environment; values reflect the particular datasets and filters used in those sessions.

## 1. Modernize BI Workloads (20 points)

### Redshift data load and revenue view

I used Redshift Query Editor v2 to work with TPC-H customer, orders, and line-item data. The load-history query confirmed successful loads of **1,500,000 customer rows**, **15,000,000 orders rows**, and **59,986,052 line-item rows**. I also queried the `mv_customer_revenue` materialized view, which exposes customer attributes alongside revenue and order-count metrics for repeatable analysis.

![Redshift table load verification](screenshots/Redshift_Data_Load.png)

![Customer revenue materialized view](screenshots/Redshift_Revenue_View.png)

### Amazon Quick connection and dashboard

I configured a Quick connection to Redshift and used the workshop data to create a market-segment analysis. The dashboard compared order counts across **BUILDING, MACHINERY, FURNITURE, AUTOMOBILE, and HOUSEHOLD** segments. Counts were approximately three million orders per segment, showing a broadly balanced distribution.

![Quick Redshift data source](screenshots/Quick_Redshift_Data_Source.png)

![Published market-segment dashboard](screenshots/Market_Segment_Dashboard.png)

### Space and conversational agent

I worked with Quick Spaces and a conversational agent to ask questions against the analytics content. The agent returned a data-grounded response in the workshop interface, illustrating how curated BI assets can be made accessible through natural-language questions.

![Quick Space](screenshots/Quick_Space.png)

![Quick agent response](screenshots/Quick_Agent_Response.png)

**Key takeaway:** Redshift provides a structured analytics layer; Quick turns that data into dashboards and question-answering experiences for non-SQL users.

## 2. Explore Quick Desktop and AWS MCP Integration (20 points)

### Desktop and MCP setup

I explored the Quick Desktop setup and tested the AWS Model Context Protocol (MCP) integration. The captured session includes a successful MCP tool test and interactions with AWS analytics resources.

![Quick Desktop setup](screenshots/Quick_Desktop_Setup.png)

![AWS MCP integration](screenshots/AWS_MCP_Integration_Success.png)

![MCP tool test](screenshots/AWS_MCP_Tool_Test_Success.png)

### Explore Redshift with natural language

Using MCP-assisted exploration, I inspected Redshift cluster and table information and reviewed SQL query history. I then asked business questions and received tabular and chart-based responses, including quarterly revenue comparisons, order trends, and Canadian revenue analysis.

![Redshift cluster exploration](screenshots/AWS_MCP_Redshift_Cluster.png)

![Redshift table exploration](screenshots/AWS_MCP_Redshift_Tables.png)

![Natural-language revenue comparison](screenshots/AWS_MCP_Quarterly_Revenue_Comparison.png)

![Order trends chart](screenshots/AWS_MCP_Order_Trends_Chart.png)

![Canada revenue analysis](screenshots/AWS_MCP_Canada_Revenue_Analysis.png)

**Interpretation:** Natural-language requests can drive SQL-backed exploration and generate useful comparisons without requiring the analyst to manually author every query or chart. Results still need to be checked for metric definitions, filters, and date scope.

## 3. Agentic AI Analytics with Athena and Amazon Quick (20 points)

### Create and query lakehouse tables

I worked with Athena and the Glue Data Catalog to register and inspect TPC-H tables sourced from S3. The evidence includes the Glue database and queries against customer, orders, and line-item tables.

![Athena Glue database](screenshots/Athena_Glue_Database_Created.png)

![Athena customer table](screenshots/Athena_Customer_CSV_Verified.png)

![Athena orders table](screenshots/Athena_Orders_CSV_Verified.png)

![Athena line-item table](screenshots/Athena_Lineitem_CSV_Verified.png)

### Prepare the unified Quick dataset

I prepared a joined TPC-H dataset in Quick, combining customer, order, and line-item fields for business analysis. The `tpch_unified_sql` dataset contained **17 columns**, and the Quick dataset summary showed a successful SPICE import of **15,708,187 rows (100% success)**.

![Joined dataset preview](screenshots/TPC_H_Unified_Joined_Preview.png)

![Successful SPICE ingestion](screenshots/TPC_H_Unified_SPICE_Success.png)

### Topic and dashboard

I configured the **TPC-H Analytics** Topic to support natural-language questions. Example queries covered average discount for high-priority orders, average order size by market segment, and return-flag analysis. The verified discount result was approximately **0.05** for high-priority orders. The return-flag screenshot illustrates an important semantic distinction: a *count* of return-flag values is not automatically a *return rate*.

![Verified high-priority discount answer](screenshots/Quick_Topic_Average_Discount_Verified.png)

![Average order size question](screenshots/Quick_Topic_Average_Order_Size.png)

![Return-flag question and interpretation](screenshots/Quick_Topic_Return_Flag_Test.png)

I also worked with the **TPC-H Lakehouse Analytics** dashboard. Its Executive Overview showed approximately **$570.53B in revenue**, **3.93M distinct orders**, and an average line-item discount of **0.05** for the dashboard's data context.

![TPC-H lakehouse analytics dashboard](screenshots/TPC-H_Lakehouse_Analytics_Dashboard.png)

### Grounded analytics agent

I configured and tested the **TPC-H Analytics Agent** with instructions to use analytics content for numeric answers and knowledge resources for field definitions. Tests included ranking market segments by order volume, explaining the `l_shipmode` field, defining market segments, and returning revenue analysis with a visual.

![Agent launched](screenshots/TPC-H_Agent_Launched.png)

![Agent data question](screenshots/TPC-H_Agent_Data_Test.png)

![Agent knowledge-grounded response](screenshots/TPC-H_Agent_Knowledge_Test.png)

![Agent explanation of market segments](screenshots/TPC-H_Agent_Market_Segment_Definition.png)

![Agent revenue analysis](screenshots/TPC-H_Agent_Revenue_Analysis.png)

**Key takeaway:** Athena, Glue, Quick datasets, Topics, and agents provide complementary layers: data access, semantic modeling, visualization, and grounded conversational analytics.

## 4. Hospital Operations BI with Amazon Quick (20 points)

### Warehouse and data sources

For the hospital scenario, I worked with Redshift data for operational reporting and verified materialized views. I also validated the Quick connection to the Redshift warehouse. The SQL evidence covers patient-flow, discharge-barrier, transfer-center, and weekend-boarding analyses.

![Hospital Redshift load](screenshots/Hospital_Redshift_Data_Load.png)

![Hospital materialized views](screenshots/Hospital_Materialized_Views_Verification.png)

![Validated hospital Redshift connection](screenshots/HospitalOps_Redshift_Connection_Validated.png)

![Patient flow verification](screenshots/HospitalOps_Patient_Flow_Verification.png)

![Discharge barriers verification](screenshots/HospitalOps_Discharge_Barriers_Verification.png)

![Transfer center verification](screenshots/HospitalOps_Transfer_Center_Verification.png)

### Operations dashboard and analytics

I built or accessed the **Hospital Operations Dashboard**, including a patient-flow and discharge-barriers view. This type of dashboard helps hospital operators identify congestion, monitor discharge obstacles, and prioritize follow-up. The weekend-boarding analysis added another operational perspective.

![Hospital operations dashboard](screenshots/Hospital_Operations_Dashboard.png)

![Weekend boarding analysis](screenshots/HospitalOps_Weekend_Boarding_Analysis.png)

### Operational reporting Flows

The workshop's reporting workflows addressed capacity, discharge-barrier escalation, and transfer-center activity. Screenshots show the **Hospital Capacity Flow**, **Discharge Barrier Escalation Report**, and **Transfer Center Weekly Scorecard** interfaces and outputs.

![Hospital capacity flow](screenshots/Hospital_Capacity_Flow.png)

![Discharge barrier escalation flow](screenshots/Hospital_Barrier_Escalation_Flow.png)

![Transfer center weekly scorecard](screenshots/Hospital_Transfer_Center_Weekly_Scorecard.png)

**Key takeaway:** Hospital operations benefit from joining warehouse metrics with dashboards and repeatable reporting workflows. Operational questions require consistent definitions of occupancy, barriers, transfers, and reporting periods.

## 5. Athena Direct Query to SPICE with Amazon Quick (20 points)

This module focused on the trade-offs among querying raw S3 data, optimizing data layout with Apache Iceberg, and selectively caching analytical data in SPICE.

### Stage 1: Query raw S3 data

The workflow begins by configuring an Athena query-results bucket and creating a Glue database. External tables over raw TPC-H customer, orders, and line-item files allow SQL queries without copying source data. A representative analysis joins the three tables to calculate monthly net revenue and distinct order counts by market segment for **June–December 1998**. Athena's **Data scanned** statistic serves as the baseline for cost comparison.

The saved evidence shows a raw-S3 Athena query and its query-results/statistics screen.

![Raw S3 Athena query baseline](screenshots/Athena_Raw_S3_Query_Baseline.png)

### Stage 2: Optimize with Iceberg and pre-aggregation

Apache Iceberg tables stored in Parquet improve scan efficiency because columnar storage can read only required columns, and partitioning can reduce the files touched by time-filtered queries. A focused `revenue_by_segment_monthly` data product further reduces repeated joins by storing one pre-aggregated result per month and segment.

The captured workshop evidence verifies an `orders_iceberg` table and its use in a Quick dataset. These screenshots establish the Iceberg-related setup, but do **not** independently establish a measured percentage reduction in bytes scanned; no numerical improvement is claimed here.

![Iceberg table verification](screenshots/Athena_Orders_Iceberg_Verified.png)

![Iceberg dataset in Quick](screenshots/Athena_Orders_Iceberg_SPICE_Dataset.png)

### Stage 3: Selective SPICE acceleration

SPICE is Quick's in-memory analytics engine. The workshop's recommended design is to import only the columns and recent periods needed for high-traffic dashboards, rather than ingesting all history. For the revenue example, the selective query retains `order_month`, `c_mktsegment`, `net_revenue`, and `order_count` for the most recent two months of the sample period (November and December 1998).

The screenshot evidence includes Quick SPICE datasets, including the Iceberg-backed dataset and a customer dataset. The provided screenshots do not establish that the specific two-month `revenue_by_segment_monthly` slice was loaded, so this selective design is described as the workshop's recommended approach rather than as a measured run.

![Customer SPICE dataset](screenshots/Athena_Customer_SPICE_Dataset.png)

### Optional S3 Tables exploration

I also inspected S3 Tables and a related Quick dataset. S3 Tables offer managed Apache Iceberg storage and automated table maintenance, reducing operational work relative to maintaining Iceberg files manually.

![S3 Table verification](screenshots/Athena_S3_Table_Verification.png)

![S3 Tables dataset in Quick](screenshots/Athena_Lineitem_S3_Tables_SPICE.png)

### Decision guide

| Workload | Recommended approach | Reason |
|---|---|---|
| First exploration of raw S3 files | Athena Direct Query | No preprocessing or warehouse provisioning |
| Repeated queries over growing datasets | Athena + partitioned Iceberg/Parquet | Less scanning through columnar reads and partition pruning |
| Fixed, frequently repeated metrics | Pre-aggregated Iceberg data product | Avoid recomputing large joins for every visual |
| High-traffic, latency-sensitive dashboard | Selective SPICE import | In-memory performance and fewer live Athena queries |

**Key takeaway:** Begin with the simplest query path; add storage optimization when scan cost grows; reserve SPICE for the specific data and dashboards that need consistently fast interactions.

## Overall Lessons Learned

1. **Warehouse and lakehouse are complementary.** Redshift supports structured, repeatable warehouse analytics, while Athena makes S3-resident data accessible without provisioning a query cluster.
2. **Semantic layers matter.** Quick Topics and curated datasets make natural-language questions more reliable, but metric names and aggregations still need verification.
3. **Agents should be grounded.** A useful analytics agent distinguishes between numeric results from data and definitions from documentation or knowledge resources.
4. **Performance optimization should be selective.** Iceberg and pre-aggregation reduce repeated work; SPICE is most useful when chosen for a specific performance need.
5. **Analytics should lead to decisions.** Market-segment dashboards and hospital operational reports demonstrate how technical pipelines can produce accessible business insights.

## Evidence Notes

- Screenshots are organized by lab and linked using relative paths under `screenshots/`.
- Workshop screenshots reflect the state captured during the temporary AWS lab session, not a permanently hosted production deployment.
- Where a screenshot verifies configuration or an output but not a benchmark, this README does not infer unrecorded performance results.
- The final Athena-to-SPICE lab's instructions were reviewed to document its architecture and decision rationale. Its direct-query dashboard and raw-vs-Iceberg cost comparison are not separately evidenced in the supplied screenshots.
