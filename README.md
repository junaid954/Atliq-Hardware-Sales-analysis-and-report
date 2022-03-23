# Atliq-Hardware-Sales-analysis-and-report

* Unlock Sales Insights that are not visible to the sales team. 
* Automated dashboard providing quick and latest sales Insights in order to support data driven decision making. 
* Sales team should be abel to take better decisions and save 10% cost on total spends. 
* Sales analyst should should stop gathering data manually in order to save 20% of their business time and reinvest it into value added activities.


1. SQL database dump is in db_dump.sql file above. Download `db_dump.sql` file to your local computer and import it as per instructions given in the tutorial video

### Data Analysis Using SQL

1. Show all customer records

    `SELECT * FROM customers;`

1. Show total number of customers

    `SELECT count(*) FROM customers;`

1. Show transactions for Delhi NCR market (market code for Delhi NCR is Mark004 

    `SELECT * FROM transactions where market_code='Mark004';`

1. Another way to display records of Delhi NCR join by table

    `SELECT sales.transactions.*, sales.markets.* 
    FROM sales.transactions 
    INNER JOIN sales.markets 
    ON sales.transactions.market_code = sales.markets.markets_code;`

1. Show distrinct product codes that were sold in Delhi NCR

    `SELECT distinct product_code FROM transactions where market_code='Mark004';`

1. Show transactions where currency is US dollars

    `SELECT * from transactions where currency="USD"`

1. Alternate way to display transactions in USD currency along with the market  name 

    ` SELECT sales.transactions.*, sales.markets.* 
    FROM sales.transactions 
    INNER JOIN sales.markets 
    ON sales.transactions.market_code = sales.markets.markets_code
    WHERE currency = 'USD';`

1. Sum of sales in the year 2018

    `SELECT SUM(sales.transactions.sales_amount)
    FROM sales.transactions 
    INNER JOIN
    sales.date
    ON sales.transactions.order_date = sales.date.date
    WHERE year = '2018';`

1. Sales transactions in January 2020

    `SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions 
    INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date
    where sales.date.year=2020 and sales.date.month_name="January";`

1. Count of currency 

    `SELECT count(*) from sales.transactions WHERE currency = 'USD';`
    
     `SELECT count(*) from sales.transactions WHERE currency = 'USD\r';`
     
     `SELECT count(*) from sales.transactions WHERE currency = 'INR';`
     
     `SELECT count(*) from sales.transactions WHERE currency = 'INR\r';`



    
# Data Analysis Using Power BI


1.  Remove records where zone was missing [New York, Paris]

    `= Table.SelectRows(sales_markets, each ([zone] <> ""))`


1. Remove sales amount with value less than or equal to 0

    `= Table.SelectRows(sales_transactions, each ([sales_amount] <> -1 and [sales_amount] <> 0))`


1. Formula to create norm_amount column

    `= Table.AddColumn(#"Filtered Rows", "norm_amount", each if [currency] = "USD" or [currency] ="USD#(cr)" then [sales_amount]*75 else [sales_amount], type any)`


1. Formula to convert USD currency into INR

    `= Table.AddColumn(#"Filtered Rows", "norm_amount", each if [currency] = "USD" or [currency] = "USD#(cr)" then [sales_amount]*75 else [sales_amount])`


1. Filter other currency values

    `= Table.SelectRows(#"Added Conditional Column", each ([currency] = "INR#(cr)" or [currency] = "USD#(cr)"))`


1. Revenue column inside new table  Base_measure 

    `Revenue = SUM('sales transactions'[sales_amount])`

1. Sales Quantity column inside new table  Base_measure

    `Sales_Quantity = SUM('sales transactions'[sales_qty])`
    
    ![image](https://user-images.githubusercontent.com/61817305/159731423-7aa884df-a816-4399-a8ac-fa88443aa9a7.png)

