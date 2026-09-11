# Lab Experiment 5: Time Series Analysis for Sales Data using Power BI

## Aim

To perform time series analysis on sales data using Power BI by analyzing monthly sales trends, month-to-month changes, sales growth, moving averages, and year-over-year performance.

## Procedure

### 1. Import and Prepare the Data

Import the following datasets into Power BI:

- Customers
- Orders
- Order Items
- Products

Open:

**Home → Transform Data**

Using Power Query:

- Remove unnecessary columns.
- Rename relevant columns.
- Change appropriate data types.
- Filter data where required.
- Replace values where required.
- Ensure `Order Date` is set to **Date**.

Click:

**Close & Apply**

### 2. Create Relationships

Go to **Model View** and create the required relationships using:

- Customer ID
- Order ID
- Product ID

Ensure that `Order Date` from the Orders table is available for time-based analysis.

### 3. Create a Calendar Table

Go to:

**Modeling → New Table**

```DAX
Calendar =
CALENDAR(
    MIN(Orders[order_date]),
    MAX(Orders[order_date])
)
```

Create the required columns:

```DAX
Year = YEAR(Calendar[Date])
```

```DAX
Month = FORMAT(Calendar[Date], "MMM")
```

```DAX
Month Number = MONTH(Calendar[Date])
```

Sort `Month` by `Month Number`.

Create the relationship:

```text
Calendar[Date]  1 ───── *  Orders[order_date]
```

### 4. Create the Required DAX Measures

**Total Sales**

```DAX
Total Sales =
SUMX(
    'Order Items',
    'Order Items'[quantity] *
    RELATED(Products[unit_price]) *
    (1 - 'Order Items'[discount_rate])
)
```

**Previous Month Sales**

```DAX
Previous Month Sales =
CALCULATE(
    [Total Sales],
    DATEADD(Calendar[Date], -1, MONTH)
)
```

**Sales Change**

```DAX
Sales Change =
[Total Sales] - [Previous Month Sales]
```

**Sales Growth %**

```DAX
Sales Growth % =
DIVIDE(
    [Sales Change],
    [Previous Month Sales]
)
```

Format `Sales Growth %` as Percentage.

**3 Month Moving Average**

```DAX
3 Month Moving Average =
AVERAGEX(
    DATESINPERIOD(
        Calendar[Date],
        MAX(Calendar[Date]),
        -3,
        MONTH
    ),
    [Total Sales]
)
```

**Total Orders**

```DAX
Total Orders =
DISTINCTCOUNT(Orders[order_id])
```

**Total Quantity**

```DAX
Total Quantity =
SUM('Order Items'[quantity])
```

**Average Order Value**

```DAX
Average Order Value =
DIVIDE(
    [Total Sales],
    [Total Orders]
)
```

### 5. Monthly Sales Trend

Create a **Line Chart**.

- X-axis → `Calendar[Month]`
- Y-axis → `Total Sales`

Title:

**Monthly Sales Trend**

Identify:

- Highest sales month
- Lowest sales month
- Increasing periods
- Decreasing periods
- Overall trend

### 6. Month-to-Month Sales Analysis

Create a visual using:

- X-axis → `Calendar[Month]`
- Y-axis → `Sales Change`

Title:

**Month-to-Month Sales Change**

Create another visual using:

- X-axis → `Calendar[Month]`
- Y-axis → `Sales Growth %`

Title:

**Monthly Sales Growth %**

Identify periods of sales increase and decrease.

### 7. Moving Average Analysis

Create a **Line Chart** containing:

- X-axis → `Calendar[Month]`
- Y-axis → `Total Sales`
- Y-axis → `3 Month Moving Average`

Title:

**Actual Sales vs 3-Month Moving Average**

Interpret whether the overall sales trend is increasing, decreasing, or fluctuating.

### 8. Monthly Orders and Quantity Analysis

Create a **Line Chart** for Monthly Orders:

- X-axis → `Calendar[Month]`
- Y-axis → `Total Orders`

Title:

**Monthly Orders Analysis**

Create another **Line Chart** for Monthly Quantity:

- X-axis → `Calendar[Month]`
- Y-axis → `Total Quantity`

Title:

**Monthly Quantity Analysis**

Identify:

- Highest order period
- Lowest order period
- Highest quantity period
- Lowest quantity period

### 9. Year-over-Year Analysis

If the dataset contains multiple years, create:

```DAX
Previous Year Sales =
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR(Calendar[Date])
)
```

```DAX
YoY Sales Growth % =
DIVIDE(
    [Total Sales] - [Previous Year Sales],
    [Previous Year Sales]
)
```

Use a suitable visual to compare sales across years.

In this dataset, only one year of data is available, so Year-over-Year analysis is omitted.

### 10. Date Slicer

Add a **Slicer** using:

```text
Calendar[Date]
```

Set the slicer type to **Between**.

The report should update according to the selected date range.

### 11. Interactive Report

Create a single report page containing:

- Total Sales Card
- Total Orders Card
- Total Quantity Card
- Average Order Value Card
- Monthly Sales Trend
- Month-to-Month Sales Change
- Monthly Sales Growth %
- Actual Sales vs 3-Month Moving Average
- Monthly Orders Analysis
- Monthly Quantity Analysis
- Date Slicer
- Sales Channel Slicer
- Product Category Slicer

### 12. Interpretation and Insights

Identify at least three meaningful business insights from the report.

Examples:

- Which month had the highest sales?
- Which month had the lowest sales?
- When did sales increase or decrease?
- What is the overall sales trend?
- Which period had the highest order volume?
- Which period had the highest quantity sold?
- What does the moving average indicate?

## Output

<img width="980" height="532" alt="Screenshot 2026-09-09 090144" src="https://github.com/user-attachments/assets/aa2c8ad1-0a5c-4267-be83-95e03465f86b" />


## Result

Thus, the sales data was successfully analyzed using time-series techniques in Power BI. Monthly trends, sales changes, growth rates, moving averages, order volume, and quantity sold were calculated and visualized to create an interactive Time Series Sales Analysis Report.
