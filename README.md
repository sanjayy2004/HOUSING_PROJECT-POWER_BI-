# Housing Analysis (Denmark Real Estate) Dashboard

### Dashboard Link : [https://app.powerbi.com/links/A41TLTv_lH?ctid=56c1d497-700b-49cf-8f8d-3dd6b20d522f&pbi_source=linkShare&bookmarkGuid=73f6fe96-ee34-4878-933d-fe71de64756e]

## Problem Statement

Buying or selling a home is one of the biggest financial decisions people make, and in a market like Denmark's — where prices, interest rates, and inflation are constantly shifting — buyers, sellers, real estate agents, and investors often struggle to answer basic questions: Are prices rising or falling this year? Which regions and house types are performing best? Is the market favoring buyers or sellers when it comes to offer vs. purchase price? How do macro factors like interest rate, inflation, and mortgage bond yields affect what people are actually paying?

This dashboard was built to answer exactly these questions. It consolidates transaction-level housing data from across Denmark — covering house type, sales type, region, area, price, size, and macroeconomic indicators — into a single interactive report so stakeholders can track sales performance, compare regions and house types, and understand how pricing behaves over time, without digging through raw data.

Since sales performance, YOY growth, and regional/house-type comparisons all depend on constantly changing figures, this dashboard gives decision-makers (agents, analysts, investors) a live, filterable view instead of a static snapshot — helping them spot which regions/house types are underperforming and where pricing or offer trends need attention.

## Steps followed

- **Step 1 :** Created a free Google Cloud account to host the dataset.
- **Step 2 :** Loaded the housing dataset into Google BigQuery and connected BigQuery to Power BI as the data source.
- **Step 3 :** Used SQL in BigQuery for initial data understanding and transformations before pulling the data into Power BI.
- **Step 4 :** Understood and cleaned the data further using the Power Query Editor.
- **Step 5 :** Built the **YOY Sales Growth** measure to track year-over-year sales change:

  ```
  YOY_Sales_Growth =
  VAR CurrYearSales =
      CALCULATE(SUM(Housing[purchase_price]), YEAR(Housing[date]) = YEAR(MAX(Housing[date])))
  VAR PrevYearSales =
      CALCULATE(SUM(Housing[purchase_price]), YEAR(Housing[date]) = YEAR(MAX(Housing[date])) - 1)
  RETURN
      IF(PrevYearSales <> BLANK(), DIVIDE(CurrYearSales - PrevYearSales, PrevYearSales))
  ```

- **Step 6 :** Added the **Offer Price** column and built a scatter plot comparing Offer Price vs. Purchase Price.
- **Step 7 :** Used the **MEDIANX** DAX function to build a Median Sales Price Change by Region chart:

  ```
  Median Sales Price Change =
  VAR CurrMedianPrice = MEDIANX(FILTER(Housing, YEAR(Housing[date]) = YEAR(MAX(Housing[date]))), Housing[purchase_price])
  VAR PrevMedianPrice = MEDIANX(FILTER(Housing, YEAR(Housing[date]) = YEAR(MAX(Housing[date])) - 1), Housing[purchase_price])
  RETURN ...
  ```

- **Step 8 :** Added a **Units Sold (latest year & quarter)** measure using CALCULATE, DISTINCTCOUNT, YEAR, QUARTER and MAX:

  ```
  Units sold in latest Year & Quater =
  CALCULATE(
      DISTINCTCOUNT(Housing[house_id]),
      YEAR(Housing[date]) = YEAR(MAX(Housing[date])) && QUARTER(Housing[date]) = QUARTER(MAX(Housing[date]))
  )
  ```

- **Step 9 :** Built a **Last 12 Month Sales** measure using CALCULATE, DATESINPERIOD and SUM:

  ```
  Last 12 Months Sales =
  CALCULATE(SUM(Housing[purchase_price]), DATESINPERIOD(Housing[date], MAX(Housing[date]), -12, MONTH))
  ```

- **Step 10 :** Created the **Sales Performance** report page.
- **Step 11 :** Added a **Sales by Region** measure using CALCULATE, SUM and ALLEXCEPT:

  ```
  Sales by Region = CALCULATE(SUM(Housing[purchase_price]), ALLEXCEPT(Housing, Housing[region]))
  ```

- **Step 12 :** Downloaded and installed Microsoft SQL Server for further data handling.
- **Step 13 :** Used the **TOTALYTD** DAX function along with a table visual to track year-to-date sales:

  ```
  TotalYTD Sales = TOTALYTD(SUM(Housing[purchase_price]), Housing[date].[Date])
  ```

- **Step 14 :** Added a donut chart to the Sales Performance page.
- **Step 15 :** Added an Age column and a **Key Influencers** visual to surface what drives price/sales behavior.
- **Step 16 :** Built an **Offer to SQM Price** ratio measure:

  ```
  Offer to SQM Ratio = DIVIDE(SUM(Housing[Offer Price]), SUM(Housing[sqm]))
  ```


- **Step 17 :** Added a clustered bar chart comparing Average Offer Price vs. Purchase Price by house type.
- **Step 28 :** Added further visuals — average inflation/interest rate/yield by house type, average SQM & SQM price by house type — on the **House Type Analysis** page, and published the report.

## Report Pages

1. **House Market Overview** — YOY Sales Growth by Sales Type (line chart), Offer Price vs. Purchase Price (scatter chart), Units Sold in latest year & quarter (card), Last 12 Months Sales (card).
2. **Sales Performance** — Sales by Region (bar chart), YTD sales table, sales split (donut chart), Key Influencers visual, Offer to SQM ratio by sales type (bar chart).
3. **House Type Analysis** — Average Offer/Purchase price by house type, average inflation/interest/yield by house type, average SQM & SQM price by house type, with slicers for area, city, sales type, and region.

## Insights

*(Fill in with your actual figures from the published dashboard)*

### Sales Performance

- Year-over-year sales growth: **[ADD %]**
- Median sales price change (latest year vs. previous year): **[ADD VALUE]**
- Units sold in the latest year & quarter: **[ADD VALUE]**
- Total sales in the last 12 months: **[ADD VALUE]**
- Highest-performing region by sales: **[ADD REGION]**

### Offer vs. Purchase Price

- Overall relationship between offer price and purchase price (from the scatter chart): **[ADD INSIGHT]**
- Offer-to-SQM price ratio trend by sales type: **[ADD INSIGHT]**
- Average offer vs. purchase price gap by house type: **[ADD INSIGHT]**

### House Type & Region

- Best-performing house type (by average price / sales): **[ADD VALUE]**
- House type most sensitive to interest rate / inflation / mortgage bond yield: **[ADD VALUE]**
- Average SQM and SQM price by house type: **[ADD VALUE]**

### Key Drivers

- Top factors influencing purchase price (from Key Influencers visual): **[ADD FACTORS]**

## Tech Stack

- **Google BigQuery** — data warehousing, SQL-based data understanding & transformation
- **Google Cloud** — hosting the dataset
- **Power Query Editor** — data cleaning
- **Power BI Desktop / Service** — data modeling, DAX measures, report building & publishing
- **Microsoft SQL Server** — additional data handling

## Report Snapshot (Power BI Desktop)

![HOUSE MARKET OVERVIEW](<img width="1306" height="730" alt="Image" src="https://github.com/user-attachments/assets/8f4d13ee-2f20-46b4-b1cd-c4520cdea2f6" />)

![Snap_SalesPerformance](ADD_IMAGE_LINK_HERE)

![Snap_HouseTypeAnalysis](ADD_IMAGE_LINK_HERE)

## Snapshot of Dashboard (Power BI Service)

![Publish_Message](ADD_IMAGE_LINK_HERE)
