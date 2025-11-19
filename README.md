📌 1. Project Overview

This project implements a full end-to-end data analytics pipeline, analyzing over 100,000+ records from the Brazilian E-commerce Public Dataset (Olist).

It includes:
```
Python ETL → cleaning, transforming, normalizing, and slicing large CSV datasets

MySQL Data Warehouse → relational modeling, optimized joins, aggregated metrics

Tableau BI Dashboard → business insights, sales trends, user distribution & category analysis
```
最终成果是一个可直接用于 业务运营决策 / 面试展示 / 企业 BI Demo 的完整可视化仪表盘。

```🏗 2. Architecture Diagram
        Raw Olist CSV (100k+ rows)
                   │
                   ▼
            Python ETL (pandas)
     Cleaning · Type Casting · Slicing
                   │
                   ▼
          MySQL Data Warehouse
  Fact Tables · Dimension Tables · SQL Aggregation
                   │
                   ▼
                Tableau BI
    Trends · Sales · Categories · Geography · RFM
```
🛠 3. Tech Stack
Data Engineering

Python（pandas, numpy, sqlalchemy）

MySQL（InnoDB, indexes, joins）

CSV → SQL 自动化批量导入

Business Intelligence

Tableau Desktop

Trends / TopN / Geo Spatial

Choropleth Maps（州级）、City Scatter Map（城市级）

ETL Features

Custom date parsing

Null / inconsistent record removal

Price + freight value merging

Large CSV slicing for performance optimization

🐍 4. Python ETL Pipeline

```import pandas as pd
from sqlalchemy import create_engine
orders = pd.read_csv("orders.csv")
customers = pd.read_csv("customers.csv")
# Convert timestamp → datetime
orders["order_purchase_timestamp"] = pd.to_datetime(
    orders["order_purchase_timestamp"], errors='coerce'
)
# Remove invalid records
orders = orders.dropna(subset=["order_id", "customer_id"])
# Load into MySQL
engine = create_engine("mysql+pymysql://root:password@localhost/olist")
orders.to_sql("orders", engine, index=False, if_exists="replace")
customers.to_sql("customers", engine, index=False, if_exists="replace")
```

✔ Python 完成的工作：
| Task                | Description                |
| ------------------- | -------------------------- |
| 🧹 Cleaning         | 删除脏数据、空城市、错误时间戳            |
| 🔎 Standardization  | 统一字段格式（日期、amount、category） |
| 🧩 Join preparation | 预处理维度表、保持主键一致性             |
| ⚡ Performance       | 切割大 CSV 提升 Tableau 加载速度    |
| 📤 Loading          | 批量写入 MySQL，形成数据仓库          |




🐬 5. MySQL Data Warehouse
🔧 Schema Overview
```
customers
orders
order_items
order_payments
products
product_category_name_translation
```


✔ Example: 建立订单 × 用户宽表（Join）
```
SELECT 
    o.order_id,
    o.order_purchase_timestamp,
    c.customer_unique_id,
    c.customer_city,
    c.customer_state
FROM orders o
JOIN customers c
ON o.customer_id = c.customer_id;
```
✔ Category Sales Aggregation（销售额 = price + freight）
```
SELECT
    p.product_category_name,
    SUM(oi.price + oi.freight_value) AS total_sales
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
GROUP BY p.product_category_name
ORDER BY total_sales DESC;
```
✔ Repurchase Analysis（复购次数统计）
```
SELECT
    customer_unique_id,
    COUNT(order_id) AS order_times
FROM orders
GROUP BY customer_unique_id;
```

📊 6. Tableau Dashboard内容（最终可视化）

包含 6 大核心视图：

📦 Monthly Order Trend（月度订单）

💰 Monthly Revenue Trend（销售额）

🛍 Category Sales Top10（类目销售）

🏙 User Top10 Cities（城市用户 Top10）

📍 City-Level User Distribution（散点图地理分布）

🗺 State-Level Choropleth（州级分布地图）






🔍 7. Key Insights & Findings
📌 1. 订单量逐年稳步上升

2017–2018 增长显著，表明平台规模持续扩大。

📌 2. 销售额呈现明显峰值周期

旺季集中在每年 Q3–Q4。

📌 3. 类目销售高度集中

auto、computer_accessories 等头部类目贡献大部分营收。

📌 4. 用户地理分布明显倾向发达城市

São Paulo、Rio de Janeiro 位居前两名，远超其他城市。

📌 5. 首购用户占比高（~75%） → 复购空间巨大

适合进一步做留存策略、积分体系。


🧩 8. Skills Demonstrated
| Category                   | Skills                                                        |
| -------------------------- | ------------------------------------------------------------- |
| **Data Engineering**       | ETL Pipeline, Data Cleaning, Type Casting                     |
| **Database**               | Data Modeling, SQL Joins, Aggregation                         |
| **Analytics**              | Trend Analysis, Category Segmentation, Repurchase Calculation |
| **BI Visualization**       | Dashboard Design, Geo Maps, Color System                      |
| **Business Understanding** | 用户画像、类目结构、营收趋势                                                |

📁 9. Project Structure
```
├── data/                         # API or original CSV
├── python_etl/                   # All ETL scripts
├── sql/                          # MySQL schemas & queries
├── tableau/                      # Tableau workbook (.twbx)
├── dashboard/                    # Exported dashboards & cover
└── README.md                     # Project documentation
```

📨 10. Contact

Author: 王智帏杨（WZWY）
GitHub: https://github.com/Wzwy-ikun





























































