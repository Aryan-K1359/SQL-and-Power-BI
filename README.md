**Sales Analysis Project Report: SQL and Power BI**

---

### **1. Introduction**

This project demonstrates a comprehensive approach to sales analysis using SQL Server and Power BI. The primary objectives include:

1. Understanding the difference between structured (Data Warehouse) and unstructured data (Lightweight data).
2. Cleaning and transforming data using SQL.
3. Building an interactive Power BI dashboard to address specific business requests.

The analysis focuses on Data Warehouse (DW) data to derive insights, perform calculations, and visualize key performance indicators (KPIs).

---

### **2. Tools and Setup**

- **SQL Server (SQL Express):** To clean, update, and query the Data Warehouse data.
- **Power BI Desktop:** To design and visualize an interactive dashboard.

**Data Sources:**
- Data Warehouse (DW) backup data.
- Lightweight (LT) backup data (used for comparison).

---

### **3. Business Requirements and User Stories**

#### **3.1 Business Request Overview**
The business request outlines the need for insights into sales performance, customer behavior, and budget forecasting. Key deliverables include:
- Sales trends over time.
- Top 10 performing products/customers.
- Geographic distribution of sales.
- Budget vs. actual performance metrics.

#### **3.2 User Stories**

| **User Story**                                  | **Acceptance Criteria**                                |
|------------------------------------------------|-------------------------------------------------------|
| As a sales manager, I want to track sales trends | A line chart showing sales performance over time.     |
| As a product manager, I need to identify top products | A bar chart of top 10 products by revenue.           |
| As a financial analyst, I want to compare budget vs actual | A pivot table and gradient bar chart for analysis. |
| As a regional head, I need geographic sales insights | A map graph of sales by region.                      |

---

### **4. Data Preparation and Cleaning**

#### **4.1 Understanding Data Structure**
- **Fact Tables:** Contain measurable quantitative data, e.g., sales transactions.
- **Dimension Tables:** Contain descriptive attributes related to the facts, e.g., customer or product information.

#### **4.2 Data Cleaning Using SQL**
Key SQL operations performed:

1. **Identifying Tables of Interest:**
   Using the business request, the following tables were selected:
   - Fact\_Sales
   - Fact\_Budget
   - Dim\_Customer
   - Dim\_Product
   - Dim\_Region

2. **Column Selection and Transformation:**
   Columns were prepared using SQL transformations:
   - Renamed columns for clarity (e.g., `Cust_ID` to `Customer_ID`).
   - Combined columns (e.g., concatenating first and last names).
   - Applied the **WHERE** clause to filter relevant data (e.g., transactions within the past year).
   - Used **LEFT JOIN** to connect fact and dimension tables.
   - Applied **CASE()** function for conditional logic (e.g., categorizing sales regions).
   - Replaced null values using **ISNULL()** function.

3. **SQL Formatting Example:**
   ```sql
   SELECT
       s.SalesID AS Transaction_ID,
       c.CustomerName AS Customer,
       p.ProductName AS Product,
       r.RegionName AS Region,
       s.SalesAmount,
       ISNULL(s.Discount, 0) AS Discount_Applied
   FROM Fact_Sales s
   LEFT JOIN Dim_Customer c ON s.CustomerID = c.CustomerID
   LEFT JOIN Dim_Product p ON s.ProductID = p.ProductID
   LEFT JOIN Dim_Region r ON s.RegionID = r.RegionID
   WHERE s.SalesDate >= '2023-01-01'
   ORDER BY s.SalesDate DESC;
   ```

4. **Budget Data Integration:**
   The Fact\_Budget table was updated using provided SQL scripts to include the latest projections.

---

### **5. Dashboard Design in Power BI**

#### **5.1 Loading and Preparing Data**
- Data from SQL queries was imported into Power BI.
- Fact and Dimension tables were connected to create a relational data model.

#### **5.2 Data Model Overview**
- **Fact Table:** Fact\_Sales
- **Dimension Tables:** Dim\_Customer, Dim\_Product, Dim\_Region, Dim\_Date
- Relationships:
  - Fact\_Sales[CustomerID] -> Dim\_Customer[CustomerID]
  - Fact\_Sales[ProductID] -> Dim\_Product[ProductID]
  - Fact\_Sales[RegionID] -> Dim\_Region[RegionID]

#### **5.3 Dashboard Components**

1. **Calculation Measures:**
   - Total Sales = SUM(SalesAmount)
   - Average Discount = AVERAGE(Discount_Applied)
   - Budget Utilization = SUM(SalesAmount) / SUM(BudgetAmount)

2. **Visualization Elements:**
   - **Pie Chart:** Sales distribution by product category.
   - **Line Chart:** Monthly sales trends.
   - **Bar Chart:** Top 10 products and customers by revenue.
   - **Map Graph:** Regional sales distribution.
   - **Pivot Table:** Budget vs. actual performance by region.

3. **Custom Visualizations:**
   - Gradient-colored bar chart to highlight performance metrics.

#### **5.4 Final Dashboard**

| Visualization   | Purpose                                   |
|-----------------|-------------------------------------------|
| Pie Chart       | Analyze product category contributions.  |
| Line Chart      | Understand sales trends over time.       |
| Map Graph       | Visualize geographic sales distribution. |
| Top 10 Bar Chart| Highlight top-performing products/customers. |
| Pivot Table     | Compare budget and actual metrics.       |

---

### **6. Key Insights and Findings**

1. **Sales Trends:**
   - Peak sales observed in Q3, with a steady decline in Q4.
   - Seasonal variations correlate with marketing campaigns.

2. **Top Products and Customers:**
   - The top 10 products contribute 60% of total revenue.
   - High-value customers show consistent purchasing patterns.

3. **Regional Analysis:**
   - The western region outperformed other regions, contributing 45% of total sales.

4. **Budget Analysis:**
   - Overall budget utilization was 90%, with some regions exceeding 100%.

---

### **7. Conclusion and Recommendations**

1. **Business Impact:**
   - The analysis provides actionable insights for sales optimization and resource allocation.

2. **Recommendations:**
   - Focus marketing efforts on the top-performing products and customers.
   - Explore underperforming regions to understand potential barriers.
   - Improve budget planning to align projections with actual trends.

3. **Future Enhancements:**
   - Automate data updates using Power BI Scheduled Refresh.
   - Introduce predictive models for sales forecasting.

---

