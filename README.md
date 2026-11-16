# retail-sales-analysis
An end-to-end retail analytics portfolio project using Python,
PostgreSQL and exploratory data analysis (EDA) to understand order
patterns, product performance, discounts, customer segments, regions and
shipping preferences.
Project status: Dataset preparation and broad EDA are completed.
Pricing-field validation, selected data-quality checks, business
observations, KPI calculations and structured SQL analysis remain in
progress. This README describes work completed so far and does not
present the project as finished.

1. Project Overview
The purpose of this project is to explore historical retail orders,
identify useful business patterns and prepare the data for further sales
and profitability analysis.
Business objectives
1. Analyse overall sales and business performance.
2. Evaluate product performance and profitability.
3. Evaluate discount patterns and their relationship with
   profitability.
4. Analyse customer segment performance.
5. Analyse regional sales and profitability.
6. Evaluate shipping mode usage and order performance.
2. Dataset
- Source: Kaggle -- Retail Orders
- File: orders.csv
- Records: 9,994
- Columns: 16
- Period: 1 January 2022 -- 31 December 2023
- Country: United States
- Product categories: 3
- Product subcategories: 17
- Customer segments: 3
The dataset includes order dates, shipping modes, customer segments,
location details, product categories and subcategories, prices,
quantities and discount percentages.
3. Tools and Technologies
- Python: Pandas, NumPy, Matplotlib, Seaborn
- Database: PostgreSQL 18
- Database interface: pgAdmin 4
- Python--PostgreSQL connection: Psycopg 3
- Notebook: Jupyter Notebook
- Repository: GitHub
4. Project Workflow and Work Completed
Step 1 -- Dataset understanding and business requirements
- Selected the Retail Orders dataset from Kaggle.
- Reviewed the dataset structure, columns and date range.
- Established six business objectives.
- Reviewed initial missing values, data types and numerical ranges.
- Identified limitations in the available data.
Step 2 -- Python data inspection and cleaning
Used Pandas to load and inspect the CSV file.
- Reviewed sample records, descriptive statistics, data types and
  missing values.
- Identified one missing Ship Mode value.
- Investigated non-standard shipping-mode entries: Not Available,
  unknown and N/A.
- Replaced affected entries with Standard Class after comparing
  similar records. These are imputed values; the original values
  are unknown.
- Converted Order Date from text to datetime in Python.
- Checked missing values, duplicate rows, Order Id uniqueness,
  categorical values, numerical ranges and date coverage.
- Retained zero values in cost price and List Price; they were not
  automatically deleted.
- No records were deleted during this cleaning work.
Step 3 -- PostgreSQL setup and data preparation
- Installed PostgreSQL 18 and pgAdmin 4.
- Created the retail_analytics database and retail schema.
- Imported the original CSV into retail.raw_orders.
- Created retail.cleaned_orders for prepared data.
- Preserved the raw imported table and applied cleaning changes to the
  cleaned table.
- Confirmed that both tables contain 9,994 records.
Final shipping-mode counts in retail.cleaned_orders:

  Ship Mode            Records
  Standard Class         5,968
Second Class           1,945
First Class            1,538
Same Day                 543
Total          9,994
Step 4 -- PostgreSQL data validation
- Both raw and cleaned tables contain 9,994 records.
- All 16 columns in the cleaned table have 9,994 non-null values.
- No complete duplicate rows were found.
- All 9,994 Order Id values are unique.
- Date coverage is 1 January 2022 to 31 December 2023.
- Order Date is stored as PostgreSQL DATE.
- Categorical whitespace checks found no leading or trailing spaces in
  the checked fields.
- Numerical ranges:
  - cost price: 0--18,110
  - List Price: 0--22,640
  - Quantity: 1--14
  - Discount Percent: 2--5
- Column data types were reviewed. Postal Code remains an integer and
  is treated as a location identifier, not a measure.
Step 5 -- Python connection and exploratory data analysis
Created the Jupyter Notebook Retail_Analytics_EDA.ipynb.
- Connected Python to PostgreSQL using Psycopg 3.
- Retrieved all records from retail.cleaned_orders into a Pandas
  DataFrame named retail.
- Confirmed the DataFrame contains 9,994 rows and 16 columns.
- Converted Order Date to datetime in the DataFrame for time-based
  analysis. This did not change the PostgreSQL table.
- Explored order patterns across products, locations, customer
  segments, shipping modes and time periods.
- Created charts to make comparisons easier to interpret.
EDA findings recorded so far
Geographical patterns - California has the highest order-record
count, representing 20.02% of all records. - Wyoming has the lowest
order-record count, representing 0.01%. - Order-record counts by region:
West 3,203; East 2,848; Central 2,323; South 1,620. - New York City has
the highest city-level order count (915); Aberdeen has the lowest (1).

Product and category patterns - Office Supplies has the highest
total quantity sold: 22,906 units. - Highest-quantity subcategories
within each category: Furnishings (Furniture), Binders (Office Supplies)
and Phones (Technology). - Lowest-quantity subcategories within each
category: Bookcases (Furniture), Supplies (Office Supplies) and Copiers
(Technology). - Binders has the highest subcategory order-record count
(1,523); Copiers has the lowest (68). - Technology has the highest
average discount by category; Office Supplies has the lowest. - Office
Supplies has an average discount of 3.48%.

Customer segment and shipping patterns - Order-record counts:
Consumer 5,191; Corporate 3,020; Home Office 1,783. - Standard Class is
the most frequently used shipping mode in each segment. - Same Day is
the least frequently used shipping mode in each segment.

Time-based patterns - Yearly order-record counts: 2022 -- 5,037;
2023 -- 4,957. - Quarterly order-record counts were compared between
2022 and 2023. - A chronological monthly revenue trend chart was created
for January 2022 to December 2023 using the current provisional revenue
calculation.
EDA visualisations created
- Category-wise quantity sold.
- Subcategory-wise quantity sold: 2022 vs 2023.
- Order distribution by shipping mode.
- Segment-wise quantity sold.
- Top 10 cities by number of orders.
- Category-wise quantity sold across regions.
- Category distribution across regions, measured by record count.
- Quarterly order comparison: 2022 vs 2023.
- Category-wise quantity contribution percentage.
- Subcategory-wise quantity contribution percentage.
- State-wise order distribution.
- Subcategory-wise orders by customer segment.
- Subcategory-wise orders by shipping mode.
- Average discount by category, region and year.
- Average cost price vs List Price by subcategory.
- Average cost price vs List Price by category.
- Monthly revenue trend from January 2022 to December 2023.
5. Pricing Assumptions -- To Be Validated
The dataset does not provide explicit definitions for the pricing
fields. Current working interpretations:
- cost price: assumed cost to acquire one unit.
- List Price: assumed unit selling/list price before discount.
- Discount Percent: assumed percentage discount applied to List
  Price.
The notebook currently uses this provisional calculation:
Revenue = List Price * Quantity * (1 - Discount Percent / 100)
Current calculated totals using this formula:

Year     Provisional calculated revenue
2022                       5,475,541.30
2023                       5,603,786.90

Important: These are provisional figures, not validated financial
results. The exact meaning of the three pricing fields and the correct
business calculation must be confirmed before reporting final revenue or
profit. The sum of List Price values alone is not revenue.

The analysis also identified 264 product/price rows in the selected
output where both cost price and List Price are zero. Their impact
needs investigation; they have not been removed.
6. Data Limitations
The dataset does not include:
- Customer ID, so individual customer retention and RFM analysis are
  not possible.
- Ship Date, so delivery duration and late-delivery analysis are not
  possible.
- Product returns, inventory, marketing, payment or supplier
  information.
- Transactions from countries other than the United States.
- More than two years of historical data.
Customer information is limited to the three customer segments.
Shipping-mode replacements are assumptions based on comparison groups,
not confirmed original values.
7. Current Status and Next Steps
Completed
- Dataset understanding and business objectives.
- Python inspection and cleaning.
- PostgreSQL database and table setup.
- Import of raw and cleaned data.
- Data-quality and validation checks in PostgreSQL.
- Python connection to PostgreSQL.
- Broad exploratory analysis and visualisations.
- Chronological monthly revenue trend chart.
Remaining
1. Convert cost price, List Price and Discount Percent to
   suitable numeric types in Python after checking their current
   representations.
2. Confirm or clearly document the definitions of the pricing fields.
3. Investigate zero-price records and measure their impact.
4. Write concise business observations for important EDA findings.
5. Complete KPI calculations and structured SQL analysis in PostgreSQL.
6. Continue the planned product, discount, segment, regional and
   shipping performance analysis.
7. Build final dashboards, recommendations and project presentation
   after calculations have been validated.
8. Project Approach
This project is being developed step by step. Observed patterns are
distinguished from assumptions, and calculated financial measures will
remain provisional until the source-field definitions are confirmed. The
goal is to produce findings that are understandable, traceable and
useful for business decisions.
