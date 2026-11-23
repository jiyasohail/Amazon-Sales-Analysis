# Amazon India Sales Analysis | SQL Server + Power BI

End-to-end sales analytics project: a raw Amazon India sales file is cleaned and normalised in **SQL Server**, turned into reporting views, and visualised in a **Power BI** dashboard covering revenue, cancellations, categories and geography.

## Business questions

- How much revenue is shipped, and how does it trend month by month?
- What share of orders is cancelled, and in which categories and states?
- Which product categories and states drive revenue?
- How do Amazon-fulfilled and merchant-fulfilled orders compare?

## Key findings

From the reporting views (Mar-Jun 2022, about 129k order lines):

- Shipped revenue totals about 71.1M INR across about 109.7k shipped order lines.
- About 14.2% of all order lines are cancelled.
- Two categories, Set (49.9%) and Kurta (27.1%), generate roughly three quarters of shipped revenue; Western Dress adds another 14.2%.
- Monthly shipped revenue peaks in April 2022 (26.2M), then declines through May (23.9M) and June (20.8M). March 2022 holds only a handful of orders, so it is a partial month.

## Dashboard

`powerbi/AmazonSales_Dashboard.pbix` connects to the five SQL views and contains:

- KPI cards: shipped revenue, total orders, cancellation rate, shipped orders
- Shipped revenue by month (line)
- Orders by status (bar)
- Shipped revenue by category (bar)
- Revenue by state (treemap)

## Database design (3NF)

| Table | Contents |
| --- | --- |
| `Orders` | order identity: index (PK), order_id, fulfilment, style, category, city, ship_state, B2B |
| `OrderDetails` | transactions: detail_id (PK), index (FK), order_date, status, courier_status, amount, qty |

## SQL layer

`sql/Amazon_Sales_Project.sql`: exploration and null audit, schema design, load with `TRY_CONVERT`, post-load validation, business analysis (revenue, cancellations, categories, time series, geography, fulfilment, styles), and five reporting views: `vw_MonthlyRevenue`, `vw_CategoryPerformance`, `vw_StateRevenue`, `vw_FulfilmentSummary`, `vw_OrderStatus`.

Data quality handled in the views: `vw_StateRevenue` standardises inconsistent state names (for example RJ, Rajshthan and Rajsthan all map to Rajasthan, and New Delhi maps to Delhi) and reports shipped-only revenue so it matches the other views.

## How to run

1. Create the database: `CREATE DATABASE AmazonSales_Project;`
2. Download the Amazon India sales report from Kaggle and import it as table `Amazon_Raw_Data` (SSMS > Tasks > Import Flat File).
3. Run `sql/Amazon_Sales_Project.sql`.
4. Open `powerbi/AmazonSales_Dashboard.pbix`. If your server is not `localhost`, change it in Transform data > Data source settings, then Refresh. The connection queries are also in `powerbi/sql_views_queries.pq`.

## Repository structure

```
Amazon-Sales-Analysis/
|-- sql/
|   `-- Amazon_Sales_Project.sql
|-- powerbi/
|   |-- AmazonSales_Dashboard.pbix
|   `-- sql_views_queries.pq
`-- README.md
```

## Tech stack

SQL Server, SSMS, T-SQL (CTEs, window functions, views), Power BI Desktop, DAX.

## Author

**Javariya Sohail** - Associate Data Analyst
[LinkedIn](https://www.linkedin.com/in/javariya-sohail-0bb895253) | [GitHub](https://github.com/jiyasohail)
