# Week 10 AWS Practice: Amazon Quick, Redshift, and Athena

**Course:** IDS 706 — Data Engineering Systems, Duke University  
**Workshop:** Better Together: Amazon Quick, Redshift and Athena — Duke University, October 2026  
**AWS Region:** `us-east-1` (N. Virginia)  
**Submission:** Lab documentation and screenshots

## Overview

This workshop explored an end-to-end analytics stack combining Amazon Redshift for data warehousing, Amazon Athena for SQL queries over Amazon S3, and Amazon Quick (QuickSight) for interactive business intelligence and conversational analytics.

The activities covered Redshift data loading and materialized views, dashboard development, AWS Model Context Protocol (MCP) integration, natural-language data exploration, Athena-based lakehouse analytics, hospital operations reporting, and performance optimization using Apache Iceberg and SPICE.

The following sections document the five workshop labs, with screenshots illustrating configurations, queries, dashboards, and analytical results.

## 1. Modernize BI Workloads (20 points)

### Redshift Data Load and Revenue View

I used Amazon Redshift Query Editor v2 to load and analyze TPC-H customer, orders, and line-item data. The load-history query confirmed successful loads of **1,500,000 customer rows**, **15,000,000 orders rows**, and **59,986,052 line-item rows**.

I also queried the `mv_customer_revenue` materialized view, which provides customer attributes alongside revenue and order-count metrics for repeatable business analysis.

![Redshift table load verification](screenshots/Redshift_Data_Load.png)

![Customer revenue materialized view](screenshots/Redshift_Revenue_View.png)

### Amazon Quick Connection and Dashboard

I configured an Amazon Quick connection to Redshift and used the workshop data to create a market-segment analysis.

The resulting dashboard compared order counts across five market segments: **BUILDING, MACHINERY, FURNITURE, AUTOMOBILE, and HOUSEHOLD**.

Each segment contained approximately three million orders, indicating a relatively balanced distribution of orders across customer categories.

![Quick Redshift data source](screenshots/Quick_Redshift_Data_Source.png)

![Published market-segment dashboard](screenshots/Market_Segment_Dashboard.png)

### Quick Spaces and Conversational Analytics

I explored Quick Spaces and conversational analytics to ask natural-language questions about the available business data.

The conversational agent generated responses using the analytics content, demonstrating how business users can access information without writing SQL queries directly.

![Quick Space](screenshots/Quick_Space.png)

![Quick agent response](screenshots/Quick_Agent_Response.png)

**Key takeaway:** Redshift provides a structured foundation for analytical workloads, while Amazon Quick transforms warehouse data into interactive dashboards and natural-language analytics experiences.

## 2. Explore Quick Desktop and AWS MCP Integration (20 points)

### Quick Desktop and MCP Setup

I explored the Amazon Quick Desktop environment and tested its integration with AWS through the Model Context Protocol (MCP).

The MCP connection enabled interactions with AWS analytics resources through natural-language requests. I verified the integration using a successful MCP tool test.

![Quick Desktop setup](screenshots/Quick_Desktop_Setup.png)

![AWS MCP integration](screenshots/AWS_MCP_Integration_Success.png)

![MCP tool test](screenshots/AWS_MCP_Tool_Test_Success.png)

### Natural-Language Redshift Exploration

Using MCP-assisted exploration, I inspected Redshift cluster information, database tables, and SQL query history.

I then explored business questions involving quarterly revenue comparisons, order trends, and revenue analysis for Canadian customers.

The resulting outputs included tabular summaries and visualizations, illustrating how natural-language interactions can support SQL-backed business analysis.

![Redshift cluster exploration](screenshots/AWS_MCP_Redshift_Cluster.png)

![Redshift table exploration](screenshots/AWS_MCP_Redshift_Tables.png)

![Natural-language revenue comparison](screenshots/AWS_MCP_Quarterly_Revenue_Comparison.png)

![Order trends chart](screenshots/AWS_MCP_Order_Trends_Chart.png)

![Canada revenue analysis](screenshots/AWS_MCP_Canada_Revenue_Analysis.png)

**Key takeaway:** MCP-assisted analytics can simplify data exploration by connecting natural-language requests with database resources, SQL queries, and visual outputs. Accurate interpretation still depends on understanding metric definitions, filters, and the underlying data.

## 3. Agentic AI Analytics with Athena and Amazon Quick (20 points)

### Athena and Glue Data Catalog

I used Amazon Athena and the AWS Glue Data Catalog to explore TPC-H datasets stored in Amazon S3.

The activities included creating and inspecting the Glue database and verifying the customer, orders, and line-item tables through Athena queries.

![Athena Glue database](screenshots/Athena_Glue_Database_Created.png)

![Athena customer table](screenshots/Athena_Customer_CSV_Verified.png)

![Athena orders table](screenshots/Athena_Orders_CSV_Verified.png)

![Athena line-item table](screenshots/Athena_Lineitem_CSV_Verified.png)

### Unified Quick Dataset and SPICE Ingestion

I prepared a unified TPC-H dataset in Amazon Quick by combining customer, order, and line-item information for downstream analysis.

The `tpch_unified_sql` dataset contained **17 columns**, and the Quick dataset summary confirmed a successful SPICE ingestion of **15,708,187 rows**, with a **100% success rate**.

This demonstrated how Athena-backed data can be prepared for interactive business intelligence workloads.

![Joined dataset preview](screenshots/TPC_H_Unified_Joined_Preview.png)

![Successful SPICE ingestion](screenshots/TPC_H_Unified_SPICE_Success.png)

### Natural-Language Topics and Dashboard Analysis

I configured the **TPC-H Analytics** Topic to support natural-language questions about the dataset.

Example questions explored:

- Average discounts for high-priority orders
- Average order size across market segments
- Return-flag analysis and interpretation

The verified average discount for high-priority orders was approximately **0.05**, or **5%**.

The return-flag analysis also highlighted the importance of distinguishing between counts and rates when interpreting business metrics.

![Verified high-priority discount answer](screenshots/Quick_Topic_Average_Discount_Verified.png)

![Average order size question](screenshots/Quick_Topic_Average_Order_Size.png)

![Return-flag question and interpretation](screenshots/Quick_Topic_Return_Flag_Test.png)

I also explored the **TPC-H Lakehouse Analytics** dashboard.

Its Executive Overview displayed approximately **$570.53 billion in revenue**, **3.93 million distinct orders**, and an average line-item discount of **0.05** within the dashboard's data context.

![TPC-H lakehouse analytics dashboard](screenshots/TPC-H_Lakehouse_Analytics_Dashboard.png)

### Grounded Analytics Agent

I configured and tested the **TPC-H Analytics Agent** to answer analytical questions using available data and knowledge resources.

The agent was designed to distinguish between numerical questions, which require analytics data, and explanatory questions, which benefit from field definitions and supporting knowledge.

Example interactions included ranking market segments by order volume, explaining the `l_shipmode` field, defining market segments, and generating revenue analysis with a visualization.

![Agent launched](screenshots/TPC-H_Agent_Launched.png)

![Agent data question](screenshots/TPC-H_Agent_Data_Test.png)

![Agent knowledge-grounded response](screenshots/TPC-H_Agent_Knowledge_Test.png)

![Agent explanation of market segments](screenshots/TPC-H_Agent_Market_Segment_Definition.png)

![Agent revenue analysis](screenshots/TPC-H_Agent_Revenue_Analysis.png)

**Key takeaway:** Athena, Glue, Quick datasets, Topics, dashboards, and analytics agents form complementary layers for data access, semantic modeling, visualization, and grounded conversational analytics.

## 4. Hospital Operations BI with Amazon Quick (20 points)

### Hospital Data Warehouse and Data Sources

I explored a hospital operations analytics scenario using Amazon Redshift and Amazon Quick.

The activities included working with hospital operational data, verifying materialized views, and validating the Quick connection to the Redshift warehouse.

SQL-based analysis covered patient flow, discharge barriers, transfer-center activity, and weekend boarding.

![Hospital Redshift load](screenshots/Hospital_Redshift_Data_Load.png)

![Hospital materialized views](screenshots/Hospital_Materialized_Views_Verification.png)

![Validated hospital Redshift connection](screenshots/HospitalOps_Redshift_Connection_Validated.png)

![Patient flow verification](screenshots/HospitalOps_Patient_Flow_Verification.png)

![Discharge barriers verification](screenshots/HospitalOps_Discharge_Barriers_Verification.png)

![Transfer center verification](screenshots/HospitalOps_Transfer_Center_Verification.png)

### Hospital Operations Dashboard

I explored the **Hospital Operations Dashboard**, including patient-flow and discharge-barrier analytics.

These visualizations illustrated how hospital operators can use operational metrics to identify congestion, monitor discharge obstacles, and support capacity planning.

The weekend-boarding analysis provided an additional perspective on patient-flow challenges.

![Hospital operations dashboard](screenshots/Hospital_Operations_Dashboard.png)

![Weekend boarding analysis](screenshots/HospitalOps_Weekend_Boarding_Analysis.png)

### Operational Reporting Flows

I explored operational reporting workflows designed to support recurring hospital management activities.

The workshop included three reporting scenarios:

- **Hospital Capacity Flow:** Monitoring patient capacity and operational conditions.
- **Discharge Barrier Escalation Report:** Identifying and communicating obstacles affecting patient discharge.
- **Transfer Center Weekly Scorecard:** Summarizing transfer-center activity and performance.

These workflows illustrated how analytics can support recurring operational reporting and decision-making.

![Hospital capacity flow](screenshots/Hospital_Capacity_Flow.png)

![Discharge barrier escalation flow](screenshots/Hospital_Barrier_Escalation_Flow.png)

![Transfer center weekly scorecard](screenshots/Hospital_Transfer_Center_Weekly_Scorecard.png)

**Key takeaway:** Hospital operations analytics can combine warehouse data, dashboards, and reporting workflows to support capacity management, discharge planning, and transfer coordination. Consistent metric definitions are essential for meaningful operational reporting.

## 5. Athena Direct Query to SPICE with Amazon Quick (20 points)

This module explored strategies for improving the efficiency of analytical workloads using Amazon Athena, Apache Iceberg, and Amazon Quick SPICE.

The central objective was to understand how storage formats, query design, pre-aggregation, and in-memory caching influence analytical performance and cost.

### Stage 1: Query Raw S3 Data

Amazon Athena supports SQL queries directly against data stored in Amazon S3 without requiring a provisioned database cluster.

The workflow involved an Athena query-results location, a Glue database, and external tables over TPC-H customer, orders, and line-item data.

A representative analytical workload calculated monthly net revenue and distinct order counts by market segment for **June–December 1998**.

Athena's **Data scanned** statistic provides a useful baseline for evaluating the efficiency and cost of subsequent query optimizations.

I explored the raw-S3 query workflow and reviewed the associated query results and statistics.

![Raw S3 Athena query baseline](screenshots/Athena_Raw_S3_Query_Baseline.png)

### Stage 2: Optimize with Apache Iceberg and Pre-Aggregation

Apache Iceberg tables stored in Parquet can improve query efficiency through columnar storage and partition-aware data access.

Columnar formats allow queries to read relevant columns, while effective partitioning can reduce unnecessary file scanning.

The workshop also introduced a `revenue_by_segment_monthly` data product, designed to store pre-aggregated revenue and order-count metrics by month and market segment.

This approach reduces the need to repeatedly perform large joins for frequently requested business metrics.

I verified the `orders_iceberg` table and explored its integration with an Amazon Quick dataset, illustrating how optimized lakehouse tables can support downstream analytics.

![Iceberg table verification](screenshots/Athena_Orders_Iceberg_Verified.png)

![Iceberg dataset in Quick](screenshots/Athena_Orders_Iceberg_SPICE_Dataset.png)

### Stage 3: Selective SPICE Acceleration

SPICE is Amazon Quick's in-memory analytics engine, designed to support responsive dashboard interactions and reduce repeated queries against underlying data sources.

The workshop demonstrated the design principle of importing only the columns and time periods required for frequently accessed dashboards.

For the revenue-analysis scenario, a selective dataset could contain:

- `order_month`
- `c_mktsegment`
- `net_revenue`
- `order_count`

A focused time window, such as **November–December 1998**, illustrates how selective ingestion can limit the volume of data stored in SPICE while preserving the information needed for dashboard analysis.

I explored SPICE datasets in Amazon Quick, including an Iceberg-backed dataset and a customer dataset, demonstrating how in-memory datasets can support interactive analytics.

![Customer SPICE dataset](screenshots/Athena_Customer_SPICE_Dataset.png)

### Optional S3 Tables Exploration

I also explored Amazon S3 Tables and their integration with Quick datasets.

S3 Tables provide managed Apache Iceberg storage with automated maintenance capabilities, helping simplify the management of analytical data stored in S3.

![S3 Table verification](screenshots/Athena_S3_Table_Verification.png)

![S3 Tables dataset in Quick](screenshots/Athena_Lineitem_S3_Tables_SPICE.png)

### Choosing an Analytics Architecture

The workshop illustrated how different analytical workloads benefit from different approaches to data access and optimization.

| Workload | Recommended approach | Reason |
|---|---|---|
| Initial exploration of raw S3 data | Athena Direct Query | Query source data without provisioning a warehouse |
| Repeated queries over growing datasets | Athena with Iceberg/Parquet | Improve scan efficiency through columnar storage and partition pruning |
| Frequently repeated business metrics | Pre-aggregated Iceberg data product | Reduce repeated joins and calculations |
| High-traffic, latency-sensitive dashboards | Selective SPICE import | Improve dashboard responsiveness and reduce repeated live queries |

**Key takeaway:** An efficient analytics architecture balances flexibility, query performance, storage design, and cost. Athena provides flexible access to S3 data, Iceberg supports optimized lakehouse storage, and SPICE accelerates frequently accessed analytical workloads.

## Overall Lessons Learned

1. **Data warehouses and lakehouses are complementary.** Amazon Redshift supports structured and repeatable warehouse analytics, while Amazon Athena enables SQL analysis of data stored directly in S3.

2. **Semantic modeling improves accessibility.** Curated Quick datasets and Topics make business metrics easier to explore through dashboards and natural-language questions.

3. **Grounded analytics agents improve interpretability.** Combining structured analytics data with supporting knowledge resources allows agents to answer numerical questions while explaining underlying business definitions.

4. **Performance optimization should match the workload.** Apache Iceberg, pre-aggregation, and SPICE address different performance needs and can be combined to create more efficient analytical systems.

5. **Analytics becomes valuable when it supports decisions.** Market-segment dashboards and hospital operations reporting demonstrate how cloud data infrastructure can translate technical datasets into actionable business information.
