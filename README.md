# Sales Dashboard 2026 | Power BI

This project is an end-to-end business intelligence (BI) solution built in Power BI Desktop. It converts two raw CSV files into a cleaned, modeled dataset and an interactive sales dashboard, and it demonstrates data cleaning, data modeling, DAX, and report design.

<img width="903" height="501" alt="Sales Dashboard 2026 showing KPI cards, sales by product, sales by category, sales over time, and filter slicers" src="https://github.com/user-attachments/assets/27bcb75d-f838-4ed5-b7c7-fc2204ab8d07" />

*Figure 1. Completed Sales Dashboard 2026 report.*

---

## Table of Contents

- [Project Overview](#project-overview)
- [Business Questions](#business-questions)
- [Tools and Techniques](#tools-and-techniques)
- [Dataset](#dataset)
- [Project Workflow](#project-workflow)
- [Data Model](#data-model)
- [Data Cleaning and Transformation](#data-cleaning-and-transformation)
- [Dashboard Components](#dashboard-components)
- [Key Findings](#key-findings)
- [Limitations](#limitations)
- [How to Run This Project](#how-to-run-this-project)
- [Repository Structure](#repository-structure)
- [Future Improvements](#future-improvements)
- [Acknowledgments](#acknowledgments)
- [Contact](#contact)

---

## Project Overview

The goal of this project is to give decision-makers a clear, interactive view of sales performance. The work covers four stages of a standard BI workflow:

1. **Connect** to the source data (two CSV files).
2. **Clean and transform** the data using Power Query and DAX.
3. **Model** the data by defining a relationship between tables.
4. **Visualize** the results in an interactive report.

## Business Questions

The dashboard is designed to answer the following questions:

- What is the total revenue?
- How many orders were placed, and by how many unique customers?
- Which products and product categories generate the most sales?
- How do sales change over time, by year and quarter?
- How do sales and customer activity differ by country, category, and date range?

## Tools and Techniques

| Area | Details |
|---|---|
| **Power BI Desktop** | Data connection, modeling, and report building |
| **Power Query (M)** | Data cleaning and transformation, with steps recorded under Applied Steps |
| **DAX** | Calculated columns, including `COALESCE` for handling null values |
| **Data modeling** | One-to-many relationship between tables |
| **Data visualization** | Bar chart, donut chart, line chart, KPI cards, and slicers |

## Dataset

The project uses two CSV files, stored in the `datasets/` folder.

**`customers.csv`**

| Column | Description |
|---|---|
| Customer ID | Unique identifier for each customer |
| First Name, Last Name | Customer name |
| Country, State, City | Customer location |
| Score | Customer score (contains missing values) |

**`orders.csv`**

| Column | Description |
|---|---|
| Order ID | Unique identifier for each order |
| Order Date | Date the order was placed |
| Product Name | Name of the product sold |
| Category | Product category |
| Sales | Sales amount for the order |
| Customer ID | Identifier that links each order to a customer |

## Project Workflow

```
CSV files → Inspection (Table view) → Cleaning (DAX, Power Query) → Data model → Report
```

1. **Connect:** Load `customers.csv` and `orders.csv` into Power BI Desktop and confirm the detected data types in the preview window.
2. **Inspect:** Use Table view to check data types, formats, and missing values.
3. **Clean and transform:** Resolve the issues listed in [Data Cleaning and Transformation](#data-cleaning-and-transformation).
4. **Model:** Create a relationship between the two tables on `Customer ID`.
5. **Visualize:** Build the report in Report view.

## Data Model

The model contains two tables joined by a **one-to-many relationship** on `Customer ID`:

```
customers (1) ───────< (*) orders
 Customer ID                Customer ID
```

Each customer appears once in `customers` and can appear in many rows of `orders`, because a customer can place multiple orders.

<img width="711" height="423" alt="Model view showing the customers and orders tables connected by a one-to-many relationship on Customer ID" src="https://github.com/user-attachments/assets/60a5c46f-05f3-43e3-b1d7-c399f3f867f4" />

*Figure 2. Data model in Power BI Model view.*

## Data Cleaning and Transformation

| Issue | Location | Solution |
|---|---|---|
| Order Date format was too long (full day and month names) | Table view | Applied a shorter date format |
| Large sales values were hard to read | Table view | Enabled the thousands separator |
| `Score` column contained null values | DAX | Created a `Clean Score` calculated column that replaces nulls with 0 |
| First and last names were stored in separate columns | Power Query | Merged them into a single `Customer Name` column, separated by a space |

The following DAX expression creates the `Clean Score` column:

```dax
Clean Score = COALESCE(customers[Score], 0)
```

`COALESCE` returns the first non-null value in its arguments. Here, it returns the original score when one exists and 0 when the score is null, so aggregations do not skip or misrepresent missing values.

Power Query records every transformation under **Applied Steps**, so the cleaning process is transparent and repeatable.

## Dashboard Components

| Component | Visual type | Purpose |
|---|---|---|
| Total Sales | Card | Sum of the `Sales` column |
| Total Number of Orders | Card | Count of `Order ID` |
| Number of Customers | Card | Distinct count of `Customer ID` |
| Total Sales by Product | Bar chart | Compare sales across products, with data labels |
| Sales by Category | Donut chart | Show each category's percentage share of sales |
| Sales Over Time | Line chart | Show sales trends, drilled down to quarter level |
| Country, Category, Order Date | Slicers | Filter all visuals; the date slicer uses the *Between* option to select a range |

Fields were renamed for readability (for example, `Count of Order ID` became `Total Number of Orders`), and the report includes a title text box and consistent color formatting.

<img width="891" height="508" alt="Report view showing the arrangement of cards, charts, and slicers on the dashboard canvas" src="https://github.com/user-attachments/assets/7a41145b-b599-4168-a380-2f25666ebfda" />

*Figure 3. Dashboard layout in Power BI Report view.*

## Key Findings

- The dataset contains **100 orders** placed by **12 unique customers**, with total sales of approximately **1 million**.
- The **Laptop** category accounts for the largest share of sales.
- Customer activity differs by country. Germany has 1 customer with 5 orders, while the United States has 8 customers.

## Limitations

- The dataset is a small, static snapshot of two CSV files. Findings are illustrative and should not be generalized.
- Data does not update automatically. After the source files change, the report must be refreshed manually in Power BI Desktop.
- Power BI Desktop runs on Windows only.

## How to Run This Project

### Prerequisites

- A Windows computer
- [Power BI Desktop](https://www.microsoft.com/store/apps/9NTXR16HNW1T) (free from the Microsoft Store)

### Procedure

1. Clone or download this repository:
   ```bash
   git clone https://github.com/Maxmilla-code/BI_sales_dashboard.git
   ```
2. Open `sales_dashboard.pbix` in Power BI Desktop.
3. If Power BI cannot locate the source files, update the file paths: select **Home → Transform data → Data source settings**, and point each source to its CSV file in the `datasets/` folder.
4. Select **Home → Refresh** to reload the data.

**Expected result:** The report loads with all visuals populated, and the slicers filter the data.

## Repository Structure

```
BI_sales_dashboard/
├── datasets/
│   ├── customers.csv
│   └── orders.csv
├── sales_dashboard.pbix             # Main Power BI report
├── Customers_orders_dashboard.pbix  # Additional version of the report
├── LICENSE
└── README.md
```

## Future Improvements

- Add DAX measures for KPIs such as average order value and year-over-year growth.
- Create a date table to support time-intelligence calculations.
- Add report pages for customer analysis and product performance.
- Replace the flat-file sources with a database connection, such as SQL Server.

## Acknowledgments

This project was built while following a Power BI crash course, which supplied the dataset and the project outline.

## Contact

**Maxmilla**

- LinkedIn: [your-link-here](https://www.linkedin.com/in/maxmuthoni/)
- GitHub: [Maxmilla-code](https://github.com/Maxmilla-code)
- Email: maxmillamuthoni1@gmail.com
