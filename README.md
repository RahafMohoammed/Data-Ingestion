📊 Data Engineering Project – Read File


📁 Project Description

This project is a typical Analytical Data Engineering pipeline. It involves:

Data ingestion from multiple sources.

Loading that data into a Snowflake data warehouse.

Performing data transformation within Snowflake.

Preparing the data for Business Intelligence (BI) analysis.

The BI tool Metabase connects to Snowflake to generate dashboards and reports for business users.

🗃️ Dataset Overview

The dataset comes from TPCDS, a well-known database benchmark focused on retail sales. It includes:

Sales data from websites and catalogs.

Inventory information for each item and warehouse.

15 dimension tables with customer, warehouse, item, and other details.

🎯 Business Requirements

The project aims to deliver dashboards/reports that:

Identify the top and bottom-performing items each week (based on sales and quantity).

Highlight items with low inventory weekly.

Detect items with low stock levels, showing the week and warehouse.

☁️ Project Infrastructure

All components are hosted in the cloud (AWS):

EC2 Servers:

t2.small → Metabase

t2.large → Airbyte

Tools:

Airbyte – data ingestion

Metabase – dashboard generation

Data Warehouse: Snowflake

AWS Lambda: Loads inventory data from S3 into Snowflake.

🛠️ Project Steps
1. Data Ingestion

Connect to:

Postgres (AWS RDS): Source of transactional data.

AWS S3: Source of inventory data (inventory.csv).

Tools:

Airbyte: Transfers data from Postgres to Snowflake.

Lambda: Loads inventory data from S3 to Snowflake.

Result: All tables loaded into Snowflake RAW schema.

2. Data Modeling
2.1 Data Background

Data split between RDS and S3.

RDS tables refresh daily.

Inventory data in S3 updates weekly, but ingestion runs daily.

2.2 Key Calculations

sum_qty_wk: Total weekly sales quantity.

sum_amt_wk: Total weekly sales amount.

sum_profit_wk: Total weekly net profit.

avg_qty_dy: Daily average sales quantity (sum_qty_wk / 7).

inv_on_hand_qty_wk: End-of-week inventory.

wks_sply: Weeks of supply (inv_on_hand_qty_wk / sum_qty_wk).

low_stock_flg_wk: Boolean flag if low stock detected.

2.3 Dimension Integration

Merge customer-related tables into a single Customer Dimension (Type 2 SCD).

Include Date_dim, Warehouse, and Item tables.

2.4 ETL and Data Loading

ETL scripts:

Merge new customer data into dimension tables.

Merge daily sales into fact tables.

Join sales and inventory to create weekly fact tables.

2.5 Scheduling

Create Snowflake tasks and stored procedures to:

Update daily sales fact tables.

Update weekly inventory fact tables.

3. Data Visualization
3.1 Metabase Setup

Validate EC2 instance and open TCP port 3000.

Install Metabase and connect it to the ANALYTICS schema in Snowflake.

3.2 Reports and Dashboards

Metabase dashboards should include:

Weekly top and bottom-performing items.

Weekly low-supply items.

Items with low stock, including week and warehouse info.

✅ Result: A fully automated data pipeline from ingestion to visualization — supporting business decision-making with accurate, timely dashboards.**
