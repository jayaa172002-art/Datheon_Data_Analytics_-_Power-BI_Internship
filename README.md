# Datheon Data Analytics & Power BI Internship
**Tools:** Python, Pandas, Matplotlib, Seaborn, Jupyter Notebook, Power BI Desktop, DAX, Power Query, Excel
**Datasets:** Superstore Sales Dataset, IBM Telco Churn Dataset

## Task 1 - EDA on Superstore Retail Sales
**Notebook:** `Task-1-Superstore-EDA/superstore_eda.ipynb`
**Dataset:** `superstore_sales (1).csv`

### What I Did For Cleaning
- Original shape was (10800, 21)
- After cleaning it became (9994, 24)
- Removed 806 empty rows
- Removed 504 duplicate rows
- Checked missing values with `df.info()` and `df.isnull().sum()`
- Postal Code had 817 null values
- Converted Order Date and Ship Date with `pd.to_datetime`
- Created Year, Month and Profit Margin columns
- Profit Margin = Profit / Sales

### Overall Numbers
- Total Sales: $2,297,200
- Total Profit: $286,397
- Total Orders: 10K
- Overall Profit Margin: 12.47%

My first thought was 2.30M sales looks great. Then I saw profit is only 286K. That is just 12.47% margin. So this is a high volume, low margin business.

### My 6 Graphs - What I Found

#### 1. Category: Sales vs Profit
- Technology: Sales $836,154 | Profit $145,454 | Margin 17.40%
- Furniture: Sales $741,999 | Profit $18,451 | Margin 2.49%
- Office Supplies: Sales $719,047 | Profit $122,492 | Margin 17.03%

What surprised me: I expected Furniture to be most profitable because it sells so much. It was the opposite. Furniture is 2nd highest in sales but lowest in profit. Its margin is 7x lower than Technology. That was the biggest surprise in my whole analysis.

#### 2. Yearly Trend
- 2015: $484,000
- 2016: $478,000
- 2017: $622,000
- 2018: $742,000

Sales are growing every year, highest in 2018. 2016 was almost flat, then it jumped from 2017 onwards.

#### 3. Monthly Seasonality
- Lowest: Feb ~$65,000
- Highest: Nov ~$390,000
- Dec: ~$365,000
- Sep: ~$335,000

I checked this twice because I thought I made a mistake in Month grouping. Nov was really ~$390k and Feb was only ~$65k. Nov is 6x Feb. Q4 clearly drives the year. Feb is the weakest month.

#### 4. Region: Profit by Region
- West: $108,420 Profit, 37.88%
- East: $91,520 Profit, 31.96%
- South: $46,750 Profit, 16.32%
- Central: $39,710 Profit, 13.86%

West + East together are 69.84% of total profit. Central is weakest. West makes 2.7x Central in profit, even though sales are not that different.

#### 5. Segment: Sales by Customer Segment
- Consumer: $1,170,000 Sales
- Corporate: $720,000 Sales
- Home Office: $470,000 Sales

Consumer is the core engine. Consumer alone is bigger than I expected.

#### 6. Discount vs Profit Scatter
- At 0% discount: Profit up to $8,000
- At 20-30% discount: Profit near zero
- At 70-80% discount: Profit down to -$6,000
- Example: 45% discount on Table = -$383 loss

This one I saw clearly in the scatter plot. At 0% discount, some orders made $8,000 profit. At 70-80% discount, same type of orders went to -$6,000. Discount above 30% basically destroys profit, especially in Furniture.

### My Recommendations - Task 1
1. Fix Furniture first. Raise margin from 2.49% to 10%+. Check Tables and Bookcases pricing and shipping cost. Stop deep discount on Furniture.
2. Cap discount at 15-20%. Ban 40-80% discounts. If discount is above 25%, manager approval should be needed.
3. Plan for seasonality. Stock up and run campaigns in Sep-Dec, especially Nov. Cut inventory in Feb.
4. Push Technology. Highest sales and highest margin at 17.40%. Put it in top promotion.
5. Fix Central region. Only $40k profit vs West $108k. Need to check delivery cost and product mix in Central.
6. Grow Consumer segment. $1.17M sales. Loyalty offers and small repeat discounts will work better here than heavy discount.
7. Set a margin alert. If margin drops below 10% in any Category or Region, flag it. Right now overall is 12.47%, which is close to danger.

---

## Task 2 - Superstore Sales Dashboard
### How I Built It
Made a Star Schema for this one:
- Fact Table: `fact_orders`
- Dimensions: `dim_customer`, `dim_product`, `dim_date`

At first I connected everything directly and slicers were not working. I rebuilt it as Star Schema and then it worked.

### DAX Measures I Used
- Total Sales = SUM(Orders[Sales])
- Total Profit = SUM(Orders[Profit])
- Total Orders = COUNTROWS(Orders)
- Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0) = 12.47%
- YoY Sales Growth % = Year over Year growth

### Page 1 - Overview

**KPI Cards I got:**
- Total Sales: 2.30M
- Total Orders: 10K
- Total Profit: 286.40K

**Visuals on Page 1:**
1. Total Profit by Category: Technology 145K > Office Supplies 122K > Furniture 18K
2. Total Sales by Order Date: Daily sales from 2015 to 2018, peaks higher in 2017-2018
3. Total Profit by Region Donut: West 108.42K (37.88%) > East 91.52K (31.96%) > South 46.75K (16.32%) > Central 39.71K (13.86%)

**Slicers:** Year (2015, 2016, 2017, 2018), Region (Central, East, South, West)

### Page 2 - Product & Region Details

**1. Treemap - Profit by Category & Sub-category:**
- Technology: Copiers 55.62K, Phones 44.52K, Accessories 41.94K
- Office Supplies: Paper 34.05K, Binders 30.22K, Storage 21.28K, Appliances 18.14K, Art 6.53K, Labels 5.55K
- Furniture: Chairs 26.59K, Furnishings 13.0K, almost invisible in treemap

Again Furniture looks tiny here. Same problem as Task 1, just in Power BI now.

**2. Matrix - Total Sales by Category & Region:**
- Technology: Central 1,70,416.31, East 2,64,973.98, South 1,48,771.91, West 2,51,991.83, Total 8,36,154.03
- Furniture: Central 1,63,797.16, East 2,08,291.20, South 1,17,298.68, West 2,52,612.74, Total 7,41,999.80
- Office Supplies: Central 1,67,026.42, East 2,05,516.06, South 1,25,651.31, West 2,20,853.25, Total 7,19,047.03
- Grand Total: Central 5,01,239.89, East 6,78,781.24, South 3,91,721.91, West 7,25,457.82, Total 22,97,200.86

**3. Region Buttons:** Central, East, South, West - Click to filter whole page

### What I Learned From Task 2
1. Profit is concentrated. West + East = 69.84%. Central is only 13.86%.
2. South is weakest in every category in the Matrix. South total is only 3,91,721 vs West 7,25,457. It is not one product problem, it is a region problem.
3. East is Technology capital. Highest Technology Sales at 2,64,973 even though West leads overall.
4. Growth is visible in the line chart. 2018 spikes are clearly higher than 2015-2016.
5. High volume, low margin again. 10K orders = 2.30M sales but only 12.47% margin.

### My Recommendations - Task 2
1. Use Page 2 Matrix in monthly review. Filter Region = South to find why South is low in all 3 categories.
2. Stop promoting Furniture in campaigns. Push Technology (145K profit) instead.
3. Push Technology in East. East already buys most Technology, double campaigns there.
4. Fix South + Central. West is 1.85x South in sales. Check delivery cost, pricing and stock there.
5. Set margin alert. Flag if margin goes below 10% in any Region or Year slicer.
6. Use slicers in meetings. Filter Year = 2018 + Region = Central to show why Central lags.

---

## Task 3 - Customer Cohort & Churn Analytics
**Dataset:** IBM Telco Churn Dataset
**Dashboard:** Telco Customer Churn 

### Baseline Numbers
- Total Customers: 7032
- Churned Customers: 1869
- Churn Rate %: 26.58%
- Formula: 1869 / 7032 = 26.58%

1 in 4 customers churn. That felt high to me when I first saw it.

### DAX Measures I Used
- Total Customers = COUNTROWS(Telco)
- Churned Customers = CALCULATE(COUNTROWS(Telco), Telco[Churn] = "Yes")
- Churn Rate % = DIVIDE([Churned Customers], [Total Customers], 0)
- Churn Rate by Contract, Tenure Cohort, Payment Method, Internet Service

### Dashboard - Page 1 - Telco Customer Churn Dashboard

**KPI Cards:**
- Total Customers: 7032
- Churned Customers: 1869
- Churn Rate %: 26.58%

**Slicer:** InternetService (DSL, Fiber optic, No)

**Visuals:**
1. Churn Rate % by Contract: Month-to-month 43% > One year 11% > Two year 3%
2. Churn Rate % by Tenure Cohort: 0-12 months 48% > 13-24 months 29% > 25-48 months 20% > 49-72 months 10%
3. Churn Rate % by Payment Method: Electronic check 46% > Mailed check 19% > Bank transfer 15% = Credit card 15%
4. Churn Rate % by Internet Service: Fiber optic ~42% > DSL ~25% > No Internet ~8%
5. Cohort Churn Matrix - Tenure vs Contract

### Cohort Matrix - Tenure vs Contract

| Tenure Cohort | Month-to-month | One year | Two year | Total |
| --- | --- | --- | --- | --- |
| 0-12 | 51% | 11% | - | 48% |
| 13-24 | 38% | 8% | - | 29% |
| 25-48 | 33% | 11% | 2% | 20% |
| 49-72 | 26% | 13% | 3% | 10% |
| Total | 43% | 11% | 3% | 27% |

Worst cell in my whole dashboard: 0-12 months + Month-to-month = 51% churn. More than half leave in first year if they are on monthly contract.

### What I Learned From Task 3
1. I thought Internet Service would be the main reason for churn. It was not. Contract type was. Month-to-month 43% vs Two-year 3% = 14x difference.
2. First year is critical. 0-12 months is 48% churn. If a customer survives first year, churn drops to 29%, then 20%, then 10%.
3. Fiber Optic is high risk. Fiber ~42% churn vs DSL ~25%. This matches what Datheon mentioned about Fiber users without tech support.
4. Payment method matters more than I expected. Electronic check 46% vs Automatic Bank / Credit 15%. Manual payment is a clear churn signal.
5. Loyal segment is 49-72 months + Two-year. Only 3% churn. Already loyal, low retention cost.

### My Recommendations - Task 3
1. Convert Month-to-Month to 1-Year. Offer 10-15% discount + free upgrade to move 0-12 Month-to-month users to 1-Year. That would cut churn from 51% to 11% in that cohort.
2. Focus on first 90 days onboarding. 48% churn in 0-12 months. Add welcome call, setup help, and 30-60-90 day check-ins for new customers.
3. Fix Fiber Optic experience. Audit Fiber support tickets. Bundle free Tech Support for Fiber users. Fiber + No Tech Support is highest risk combo.
4. Move Electronic Check to Auto-Pay. Offer $2-3 per month discount to switch Electronic check (46% churn) to Bank transfer or Credit card auto-pay (15% churn).
5. Make an early warning list. Flag customer if Tenure is less than 12 + Month-to-month + Electronic check + Fiber optic. This is highest churn probability profile. Do a proactive retention call before they churn.
6. Spend retention budget wisely. Spend on 0-12 + Month-to-month, not on 49-72 + Two-year (only 3% churn, already loyal).


## My Key Takeaways From All 3 Tasks
1. Technology and long-term contracts drive profit and retention.
2. Furniture and Month-to-month contracts destroy profit and retention.
3. First year and first months decide if customer stays or leaves.
4. Discount and manual payment are early warning signals for loss.
5. West region and Fiber optic need opposite actions: West to grow, Fiber to fix.

Thanks for reading. Feedback welcome.
