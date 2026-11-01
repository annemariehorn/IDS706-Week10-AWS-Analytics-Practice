# Week 10 AWS Practice: Amazon Quick, Redshift, and Athena

**Course:** IDS 706 — Data Engineering Systems, Duke University  
**Workshop:** Better Together: Amazon Quick, Redshift and Athena — Duke University, October 2026  
**AWS Region:** `us-east-1` (N. Virginia)  
**Submission:** Lab documentation and screenshots

## Overview

This workshop explored an end-to-end analytics stack combining Amazon Redshift for data warehousing, Amazon Athena for SQL queries over Amazon S3, and Amazon Quick (QuickSight) for interactive business intelligence and conversational analytics.

The activities covered Redshift data loading and materialized views, dashboard development, AWS Model Context Protocol (MCP) integration, natural-language data exploration, Athena-based lakehouse analytics, hospital operations reporting, and performance optimization using Apache Iceberg and SPICE.

The following sections document the five workshop labs, with screenshots of configurations, queries, dashboards, and analytical results.

## 1. Modernize BI Workloads (20 points)

### Redshift Data Load and Revenue View

I used Amazon Redshift Query Editor v2 to load and verify TPC-H customer, orders, and line-item data. The `SYS_LOAD_HISTORY` query confirmed completed loads of **1,500,000 customer rows**, **15,000,000 orders rows**, and **59,986,052 line-item rows**.

I then queried the `mv_customer_revenue` materialized view, which combines customer attributes (nation, name, and market segment) with total revenue and order count per customer for repeatable business analysis.

![Redshift table load verification](screenshots/Redshift_Data_Load.png)

![Customer revenue materialized view](screenshots/Redshift_Revenue_View.png)

### Amazon Quick Data Source, Dataset, and Dashboard

I created and validated an Amazon Redshift data source in Amazon Quick (`customer-rev-datasource`, database `dev`, SSL enabled) and built a dataset from the workshop data.

Using that dataset, I published the **Market Segment Dashboard**, which compares total order counts across five market segments:

| Market Segment | Order Count | Share of Total |
|---|---:|---:|
| BUILDING | 3,004,382 | 20.03% |
| MACHINERY | 3,002,982 | 20.02% |
| FURNITURE | 3,001,268 | 20.01% |
| AUTOMOBILE | 3,000,540 | 20.00% |
| HOUSEHOLD | 2,990,828 | 19.94% |

The 15,000,000 orders are distributed almost evenly across segments, with each accounting for approximately 20% of the total. This suggests that differences in segment revenue may be driven more by order values than by order counts.

![Quick Redshift data source](screenshots/Quick_Redshift_Data_Source.png)

![Published market-segment dashboard](screenshots/Market_Segment_Dashboard.png)

### Quick Space and Grounded Chat Agent

I created the **Customer Revenue & Segmentation** Space, containing the `mv_customer_revenue` dataset, the Market Segment Dashboard, and `Nation.csv` reference data used to map nation keys to country names.

I then created the **Revenue Analytics Assistant** chat agent using this Space and asked: *"Which market segment generated the most revenue in Canada?"*

The agent identified **MACHINERY** as the highest-revenue segment in Canada, with approximately **$17.51 billion** across **120,510 orders**. It also returned the full segment ranking and cited `mv_customer_revenue` as its source, demonstrating how conversational analytics can generate data-grounded business insights.

![Quick Space](screenshots/Quick_Space.png)

![Quick agent response](screenshots/Quick_Agent_Response.png)

**Key takeaway:** Redshift provides a structured foundation for analytical workloads, while Amazon Quick turns warehouse data into dashboards and grounded natural-language analytics.

## 2. Explore Quick Desktop and AWS MCP Integration (20 points)

### Quick Desktop and MCP Setup

I signed in to Amazon Quick Desktop and connected it to AWS through the Model Context Protocol (MCP).

I verified the connection by asking Quick to list the available AWS MCP tools. The server exposed **8 tools**, including `search_documentation`, `list_regions`, `run_script`, and `get_presigned_url`.

I also tested the `list_regions` tool, which returned **37 AWS regions**.

![Quick Desktop setup](screenshots/Quick_Desktop_Setup.png)

![AWS MCP integration](screenshots/AWS_MCP_Integration_Success.png)

![MCP tool test](screenshots/AWS_MCP_Tool_Test_Success.png)

### Natural-Language Redshift Exploration

Using the AWS MCP tools from Quick Desktop, I inspected the Redshift environment through natural-language requests.

The exploration identified:

- **Cluster:** A provisioned Redshift cluster in `us-east-1`, with status *available* and database `dev`.
- **Database objects:** Three tables (`customer`, `orders`, `lineitem`) and one materialized view (`mv_customer_revenue`).

I then explored business questions using natural language. Quick generated SQL-backed results and visualizations for:

1. Revenue by market segment in Q2 1998.
2. Monthly order volume and order value from 1992 through 1998.
3. Revenue by market segment for Canadian customers.

![Redshift cluster exploration](screenshots/AWS_MCP_Redshift_Cluster.png)

![Redshift table exploration](screenshots/AWS_MCP_Redshift_Tables.png)

![Natural-language revenue comparison](screenshots/AWS_MCP_Quarterly_Revenue_Comparison.png)

![Order trends chart](screenshots/AWS_MCP_Order_Trends_Chart.png)

![Canada revenue analysis](screenshots/AWS_MCP_Canada_Revenue_Analysis.png)

### Interpreting the Results

**Quarterly revenue comparison:** For Q2 1998, FURNITURE had the highest revenue at approximately **$17.17 billion**, while MACHINERY had the lowest at approximately **$17.06 billion**. The difference was relatively small, at less than 1% of segment revenue. Because the chart's axis began near $16.9 billion rather than zero, the visual differences appeared larger than their actual magnitude.

**Monthly order trends:** Monthly orders remained relatively steady at approximately **190,000 orders per month** through much of the observed period. The decline at the end of the chart reflects an incomplete final reporting month rather than necessarily indicating a sustained decline in demand.

**Canadian revenue:** MACHINERY ranked first at approximately **$17.51 billion**, followed by BUILDING, HOUSEHOLD, AUTOMOBILE, and FURNITURE. The difference between the highest- and lowest-revenue segments was under 2%, consistent with the relatively balanced market-segment distribution.

**Key takeaway:** MCP-assisted analytics connects natural-language requests with database resources, SQL queries, and visual outputs. Accurate interpretation still depends on understanding metric definitions, time filters, partial reporting periods, and chart scaling.

## 3. Agentic AI Analytics with Athena and Amazon Quick (20 points)

### Athena and Glue Data Catalog

I used Amazon Athena and the AWS Glue Data Catalog to work with TPC-H data stored in Amazon S3.

I created the Glue database `workshop_qs_athena_tpc_h_db_sql` and queried the customer, orders, and line-item tables in Athena.

The recorded query statistics included:

| Table | Run Time | Data Scanned |
|---|---:|---:|
| customer | 2.083 s | 3.55 MB |
| orders | 11.567 s | 10.95 MB |
| lineitem | 1 min 9.77 s | 7.19 MB |

These results demonstrated how Athena can query S3-resident data and report execution statistics useful for evaluating analytical workloads.

![Athena Glue database](screenshots/Athena_Glue_Database_Created.png)

![Athena customer table](screenshots/Athena_Customer_CSV_Verified.png)

![Athena orders table](screenshots/Athena_Orders_CSV_Verified.png)

![Athena line-item table](screenshots/Athena_Lineitem_CSV_Verified.png)

### Joined Quick Dataset and SPICE Ingestion

I prepared a joined TPC-H dataset in Amazon Quick (`tpch_unified_sql`) using the Athena data source `tpch-lakehouse-athena`.

The dataset combined customer, order, and line-item fields into a unified structure for business analysis.

It contained **17 columns**, and the Quick dataset summary confirmed a completed SPICE import of **15,708,187 rows (100% success)**.

![Joined dataset preview](screenshots/TPC_H_Unified_Joined_Preview.png)

![Successful SPICE ingestion](screenshots/TPC_H_Unified_SPICE_Success.png)

### Topic and Dashboard

I configured the **TPC-H Analytics** Topic to support natural-language questions about the unified dataset.

The Topic was tested with several analytical questions:

- **Average discount for high-priority orders:** Returned approximately **0.05 (5%)**, consistent across the five market segments. I marked this answer as verified.
- **Average order size by market segment:** Returned an overall average sales amount of approximately **38,231.86**, with HOUSEHOLD slightly highest at **38,250.96**.
- **Return-flag analysis:** Demonstrated the importance of distinguishing between counts and rates. The Topic interpreted a question about return rate as a count of return-flag values. A true return rate would require dividing the number of returned line items by the total number of line items within each segment.

![Verified high-priority discount answer](screenshots/Quick_Topic_Average_Discount_Verified.png)

![Average order size question](screenshots/Quick_Topic_Average_Order_Size.png)

![Return-flag question and interpretation](screenshots/Quick_Topic_Return_Flag_Test.png)

I also explored the **TPC-H Lakehouse Analytics** dashboard, which included Executive Overview, Segment & Customer Analysis, and Shipping Performance sheets.

The Executive Overview displayed:

- **Total discounted revenue:** Approximately $570.53 billion.
- **Distinct orders:** Approximately 3.93 million.
- **Average line-item discount:** Approximately 0.05 (5%).

These results illustrate the distinction between line-item records and distinct orders, as well as between gross sales and revenue after discounts.

The approximately **$600.6 billion** sales figure returned by the agent and the **$570.53 billion** discounted revenue figure are broadly consistent with the reported average discount of 5%. The dataset's 15.7 million rows represent line items, while the dashboard reports approximately 3.93 million distinct orders. A precise reconciliation would require confirming that both revenue measures use the same filters and aggregation definitions.

![TPC-H lakehouse analytics dashboard](screenshots/TPC-H_Lakehouse_Analytics_Dashboard.png)

### Knowledge Base, Space, and Grounded Analytics Agent

I configured the **TPC-H Analytics Agent** using the TPC-H Lakehouse Analytics Space, the TPC-H Analytics Topic, and supporting schema documentation.

The agent was instructed to answer numerical questions using analytics data and explanatory questions using knowledge resources, while citing its sources.

Example interactions included:

- **Market-segment ranking:** Retrieved order-volume information using the analytics Topic.
- **Shipping method definition:** Explained the `l_shipmode` field and its role in the TPC-H schema.
- **Market-segment definition:** Explained the `C_MKTSEGMENT` field and its five customer categories.
- **Revenue analysis:** Generated a table and visualization of total sales by market segment, with approximately $600.6 billion in overall sales.

The agent's responses included source citations, illustrating how structured data and supporting documentation can work together in a grounded conversational analytics system.

![Agent launched](screenshots/TPC-H_Agent_Launched.png)

![Agent data question](screenshots/TPC-H_Agent_Data_Test.png)

![Agent knowledge-grounded response](screenshots/TPC-H_Agent_Knowledge_Test.png)

![Agent explanation of market segments](screenshots/TPC-H_Agent_Market_Segment_Definition.png)

![Agent revenue analysis](screenshots/TPC-H_Agent_Revenue_Analysis.png)

**Key takeaway:** Athena, Glue, Quick datasets, Topics, dashboards, knowledge bases, and agents provide complementary layers for data access, semantic modeling, visualization, and grounded conversational analytics.

## 4. Hospital Operations BI with Amazon Quick (20 points)

### Hospital Data Warehouse and Data Sources

I worked with the hospital operations data warehouse in Amazon Redshift Serverless using the `hospital_ops` schema and connected the warehouse to Amazon Quick.

The base tables contained:

| Table | Records |
|---|---:|
| patient_flow | 1,000 |
| discharge_barriers | 800 |
| transfer_center | 600 |

I also verified three materialized views:

| Materialized View | Records |
|---|---:|
| mv_unit_capacity | 434 |
| mv_discharge_throughput | 798 |
| mv_transfer_performance | 558 |

I validated the **HospitalOpsRedshift** data source in Quick using the `HospitalOps-VPC-Connection`, connecting to the Redshift Serverless workgroup through database `dev`, port 5439, with SSL enabled.

I also queried the hospital tables through Athena using the `hospital_ops_db` database, confirming the corresponding record counts.

![Hospital Redshift load](screenshots/Hospital_Redshift_Data_Load.png)

![Hospital materialized views](screenshots/Hospital_Materialized_Views_Verification.png)

![Validated hospital Redshift connection](screenshots/HospitalOps_Redshift_Connection_Validated.png)

![Patient flow verification](screenshots/HospitalOps_Patient_Flow_Verification.png)

![Discharge barriers verification](screenshots/HospitalOps_Discharge_Barriers_Verification.png)

![Transfer center verification](screenshots/HospitalOps_Transfer_Center_Verification.png)

### Hospital Operations Dashboard

I explored the **Hospital Operations Command Center** dashboard, including its Patient Flow & Discharge Barriers analysis.

The dashboard combined occupancy, emergency department boarding, and discharge-barrier metrics with filters for facility, unit, patient acuity, staffing status, day of week, and transfer status.

The **Average Occupancy by Facility and Unit** heatmap compared 15 care units across five facilities, making it easier to identify potential capacity-pressure areas.

I also analyzed weekend boarding patterns. Average emergency department boarding time was approximately **6.7 hours on weekends**, compared with **4.39 hours on weekdays**, representing a difference of approximately **53%**.

This suggests that weekend patient-flow processes warrant further investigation, including staffing, discharge timing, and operational coordination.

![Hospital operations dashboard](screenshots/Hospital_Operations_Dashboard.png)

![Weekend boarding analysis](screenshots/HospitalOps_Weekend_Boarding_Analysis.png)

### Topics, Space, and Operational Analytics Agent

The workshop explored the use of Amazon Quick Topics, Spaces, and agents to support hospital operations analysis.

The **Hospital Operations Analyst** scenario combined operational metrics with supporting organizational procedures.

The analytical framework included thresholds for identifying potential operational risks:

- Unit occupancy above 95%.
- Emergency department boarding above 4 hours.
- Discharge barriers unresolved beyond 12 hours.

A cross-domain analytical question examined which units required attention based on both occupancy pressure and unresolved discharge barriers.

This illustrated how multiple operational datasets can be considered together to support prioritization and escalation decisions.

![Hospital capacity flow](screenshots/Hospital_Capacity_Flow.png)

### Operational Reporting Flows

I explored three Amazon Quick reporting workflows designed to support recurring hospital management activities:

- **Hospital Capacity Flow:** Monitoring patient capacity and operational conditions.
- **Discharge Barrier Escalation Report:** Identifying barriers requiring escalation and generating structured reports.
- **Transfer Center Weekly Scorecard:** Summarizing transfer-center activity and performance.

The **Discharge Barrier Escalation Report** applied a threshold of average resolution time exceeding **8 hours** and summarized results by barrier owner.

The recorded analysis identified **457 qualifying records across 7 barrier owners**. Pharmacy, Payer, and Family were among the categories with the highest maximum average resolution times.

The report also illustrated how operational data can be combined with recommended escalation actions and reporting templates.

The **Transfer Center Weekly Scorecard** provided a structured view of transfer-center performance, including threshold-based status categories and facility-level activity.

![Discharge barrier escalation flow](screenshots/Hospital_Barrier_Escalation_Flow.png)

![Transfer center weekly scorecard](screenshots/Hospital_Transfer_Center_Weekly_Scorecard.png)

**Key takeaway:** Hospital operations analytics combines warehouse data, dashboards, agents, and reporting workflows to support capacity management, discharge planning, and transfer coordination. Consistent metric definitions and explicit operational thresholds are essential for meaningful reporting.

## 5. Athena Direct Query to SPICE with Amazon Quick (20 points)

This module explored approaches for improving the efficiency of analytical workloads using Amazon Athena, Apache Iceberg, and Amazon Quick SPICE.

The central focus was understanding how storage formats, query design, pre-aggregation, and in-memory caching affect analytical performance and cost.

### Stage 1: Query Raw S3 Data

Amazon Athena runs SQL directly against data stored in Amazon S3 without requiring a provisioned database cluster.

The workflow used an Athena query-results location, a Glue database, and external tables over TPC-H customer, orders, and line-item files.

I explored a raw-S3 aggregation query that calculated customer count and average account balance by market segment.

The recorded query statistics were:

- **Execution time:** 1.633 seconds.
- **Data scanned:** 2.28 GB.

The result showed approximately three million customers per segment, with average account balances ranging from roughly $4,495 to $4,502.

Athena's **Data scanned** metric is particularly important because query costs depend on the amount of data scanned. This query provides a recorded raw-data example, while the orders queries in Stage 2 illustrate a separate performance comparison.

![Raw S3 Athena query baseline](screenshots/Athena_Raw_S3_Query_Baseline.png)

### Stage 2: Optimize with Apache Iceberg and Pre-Aggregation

Apache Iceberg tables stored in Parquet can improve query efficiency through columnar storage and partition-aware data access.

Columnar formats allow analytical queries to read relevant columns, while effective partitioning can reduce unnecessary file scanning.

I explored the `orders_iceberg` table and its integration with an Amazon Quick SPICE dataset.

The workshop also introduced the `revenue_by_segment_monthly` data product, designed to store pre-aggregated monthly net revenue and distinct order counts by market segment.

Pre-aggregation reduces the need to repeatedly execute large customer–orders–line-item joins for commonly requested metrics.

The recorded Athena query examples provided the following performance statistics:

| Query Example | Table Format | Run Time | Data Scanned |
|---|---|---:|---:|
| Raw orders query | CSV | 11.567 s | 10.95 MB |
| Iceberg orders query | Iceberg / Parquet | 0.812 s | 4.35 KB |

The recorded results show an approximately **14× difference in execution time** and a **2,500× difference in data scanned** between the two query examples.

These measurements illustrate the potential benefits of optimized storage formats. However, a controlled comparison using equivalent queries would be necessary to determine how much of the difference is attributable specifically to Iceberg, Parquet, or partition pruning.

![Iceberg table verification](screenshots/Athena_Orders_Iceberg_Verified.png)

![Iceberg dataset in Quick](screenshots/Athena_Orders_Iceberg_SPICE_Dataset.png)

### Stage 3: Selective SPICE Acceleration

SPICE is Amazon Quick's in-memory analytics engine. It supports responsive dashboard interactions by serving imported data without requiring a live query to Athena for every interaction.

The workshop emphasized importing only the columns and time periods needed for frequently accessed dashboards.

For the monthly revenue-analysis scenario, a selective dataset can contain:

- `order_month`
- `c_mktsegment`
- `net_revenue`
- `order_count`

A focused reporting window, such as **November–December 1998**, illustrates how selective ingestion can reduce SPICE storage requirements while retaining the data needed for dashboard analysis.

I explored SPICE datasets in Amazon Quick, including the Iceberg-backed orders dataset and a customer dataset.

![Customer SPICE dataset](screenshots/Athena_Customer_SPICE_Dataset.png)

### Optional S3 Tables Exploration

I also explored Amazon S3 Tables and their integration with Amazon Quick datasets.

S3 Tables provide managed Apache Iceberg storage with automated maintenance capabilities, such as compaction, helping simplify the management of analytical data stored in S3.

The workshop included an S3 Tables configuration and a corresponding line-item dataset in Quick.

![S3 Table verification](screenshots/Athena_S3_Table_Verification.png)

![S3 Tables dataset in Quick](screenshots/Athena_Lineitem_S3_Tables_SPICE.png)

### Choosing the Right Analytics Approach

| Workload | Recommended Approach | Reason |
|---|---|---|
| Initial exploration of raw S3 data | Athena Direct Query | Query source data without provisioning a warehouse |
| Repeated queries over growing datasets | Athena with Iceberg/Parquet | Improve scan efficiency through columnar storage and partition pruning |
| Frequently repeated business metrics | Pre-aggregated Iceberg data product | Avoid repeating large joins and calculations |
| High-traffic, latency-sensitive dashboards | Selective SPICE import | Improve responsiveness and reduce repeated live queries |

**Athena Direct Query** is useful when data freshness is important, analytical requirements are changing, or dashboards are accessed infrequently.

**SPICE** is useful when many users repeatedly access the same dashboard and scheduled refreshes provide sufficient data freshness.

**Key takeaway:** An efficient analytics architecture balances flexibility, performance, storage design, and cost. Athena provides flexible access to S3 data, Iceberg and pre-aggregation can reduce repeated scanning and computation, and SPICE accelerates frequently accessed dashboards.

## Overall Lessons Learned

1. **Data warehouses and lakehouses are complementary.** Amazon Redshift supports structured, repeatable warehouse analytics, while Amazon Athena enables SQL analysis of data stored directly in S3.

2. **Semantic modeling improves accessibility.** Curated Quick datasets and Topics make business metrics easier to explore through dashboards and natural-language questions. However, interpretations such as "rate" versus "count" should be verified before results are trusted.

3. **Grounded analytics agents improve interpretability.** Combining structured analytics data with supporting knowledge resources enables agents to answer numerical questions, explain business definitions, and cite their sources.

4. **Performance optimization should match the workload.** Apache Iceberg, columnar storage, pre-aggregation, and SPICE address different performance needs and can be combined to create more efficient analytical systems.

5. **Analytics becomes valuable when it supports decisions.** Market-segment dashboards and hospital operations reporting demonstrate how cloud data infrastructure can transform technical datasets into actionable business insights.

## Workshop Access Note

The AWS workshop and lab environment became inaccessible before the **November 1, 12:25 PM** deactivation time stated in the Canvas assignment instructions. The screenshots and documentation reflect the activities and results captured before access ended.
