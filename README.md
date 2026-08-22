## Company Background

This data warehouse is being built for a real, small leather goods manufacturer based in Tuscany, Italy. The company:

- Works with luxury brands such as **Chanel**, **Gucci**, and **Louis Vuitton**.
- Produces high-end leather accessories and packaging, including:
  - Watch boxes
  - Bags
  - Pen boxes
  - Other luxury leather goods and custom cases

### Current Process (Excel-Based)

At present, the company:

- Records all production and order data in **Excel files** (e.g., number of orders, product types, materials, deadlines).
- Tracks material purchases and production costs in separate spreadsheets.
- Calculates **costs and profits manually in Excel**, using formulas and pivot tables.
- Relies on ad-hoc reports and manual updates, which are:
  - Time-consuming to maintain.
  - Hard to standardize across multiple files and users.
  - Difficult to scale as the business grows.

### Project Goal: Automate and Extend Capabilities

This project aims to:

- **Automate** the current Excel-based workflow by:
  - Ingesting daily Excel files into a PostgreSQL data warehouse.
  - Cleaning and validating data with Python (pandas).
  - Transforming raw data into structured dimension and fact tables using SQL.
- **Extend** the company’s analytical capabilities by:
  - Providing a single, consistent source of truth for orders, materials, and costs.
  - Enabling repeatable, scalable analysis of profitability by product, customer, and material.
  - Laying the foundation for dashboards and more advanced analytics in the future.

In short, this warehouse will preserve the company’s existing Excel-based data collection but move the heavy lifting of calculation, aggregation, and reporting into a robust, automated data platform.