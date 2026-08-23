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

**NOTE** - This project will **not** use real data sources for privacy reasons. Therefore, the excel sheets will be made up artificially. 

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

### Business costs and pricing explained

The total cost of a unit of product is calculated as:
**direct costs + indirect cost**

Where:
- **direct costs = leather + carpenter + labor**
- **indirect costs = 35% of direct costs**

-------
**Leather** quantity for each order is calculated using `ordered_units` from the orders sheet and `leather_meters_per_unit` from the products sheet:

```
leather_meters_required = ordered_units * leather_meters_per_unit
```

The order-level leather cost is then calculated as:

```
estimated_leather_cost_eur =
    leather_meters_required * leather_price_per_meter_eur
```

For customer-supplied leather, `leather_price_per_meter_eur` is zero, so `estimated_leather_cost_eur` is zero. The quantity requirement is still calculated because it remains useful for production planning and operational analysis.

-------
**Labor** is calculated from the product-specific production time and the ordered quantity. The orders sheet contains `total_labor_time_minutes`, which is calculated as:

```text
total_labor_time_minutes =
    ordered_units * labor_time_per_unit_minutes
```

The company’s labor rate is represented by `labor_rate_per_minute_eur`, currently set to €0.47 per minute. The estimated labor cost is calculated as:

```text
estimated_labor_cost_eur =
    total_labor_time_minutes * labor_rate_per_minute_eur
```

-------
The column `estimated_carpentry_cost_eur` represents the **carpentry cost** associated with the order, based on the carpenter’s charge for the sample or related production work. In this simplified project, the explicitly calculated direct costs are leather, carpentry, and labor. They are combined in `estimated_direct_cost_eur`:

```text
estimated_direct_cost_eur =
    estimated_leather_cost_eur
    + estimated_carpentry_cost_eur
    + estimated_labor_cost_eur
```

-------
Other materials and shared business expenses are not calculated individually for each order in this first version. Items such as zips, buttons, linings, hardware, electricity, machinery, rent, administration, and other shared operating costs are considered **indirect costs** and represented by the `indirect_cost_rate`. This rate is currently **35%**, stored in the orders sheets as `indirect_cost_rate`.

**The indirect cost estimate is calculated as 35% of direct costs**:

```text
estimated_indirect_cost_eur =
    estimated_direct_cost_eur * indirect_cost_rate
```

The total estimated cost is therefore:

```text
estimated_total_cost_eur =
    estimated_direct_cost_eur
    + estimated_indirect_cost_eur
```

**In this project, the 35% factor already includes the company’s profit.** 
Consequently, `estimated_total_cost_eur` is not only a cost estimate: it is the final amount used to derive the customer quotation. The final quoted price per unit is calculated as:

```text
final_quoted_price_per_unit_eur =
    estimated_total_cost_eur / ordered_units
```

### Rationale behind decision to compare with monthly costs

The `monthly_costs_2024` and `monthly_costs_2025` sheets provide a separate view of the company’s costs by month. The rationale to keep track of monthly costs is because it's impossible to calculate indirect costs per each order, like electricity, rent, ammortization of machines. Therefore, it's easier to compare monthly revenue with monthly costs in order to check for the company's profitability and check, for example, if the 35% markup is enough to cover indirect expenses and the owner's profits.