# Atliq-Hardware-Sales-analysis-and-report

### Task
* Unlock Sales Insights that are not visible to the sales team. 
* Create automated dashboard providing which will provide quick and latest sales Insights for better data driven decision making. 
* Sales team should be able to take better decisions and save 10% cost on total spends. 
* Sales analyst should should stop gathering data manually in order to save 20% of their business time and reinvest it into value added activities.



### Data Analysis Using SQL

1. Show all customer records

    `SELECT * FROM sales.customers;`

![image](https://user-images.githubusercontent.com/61817305/159735342-0d5f5cf3-f1c8-42d3-a19e-21540953e7a9.png)

1. Show total number of customers

    `SELECT count(*) FROM sales.customers;`
    
    ![image](https://user-images.githubusercontent.com/61817305/159735528-2596d3b2-35cb-4a99-9feb-db9df067afff.png)


1. Show transactions for Delhi NCR market (market code for Delhi NCR is Mark004 

    `SELECT * FROM transactions where market_code='Mark004';`
    
    ![image](https://user-images.githubusercontent.com/61817305/159735765-e21e9158-b8c9-466a-b55e-60f6f7e36bc7.png)


1. Another way to display records of Delhi NCR join by table

    `SELECT sales.transactions.*, sales.markets.* 
    FROM sales.transactions 
    INNER JOIN sales.markets 
    ON sales.transactions.market_code = sales.markets.markets_code;`
    
![Screenshot_5](https://user-images.githubusercontent.com/61817305/159733807-2647e485-8e00-48b2-9089-8893893fddbe.png)

1. Show distrinct product codes that were sold in Delhi NCR

    `SELECT distinct product_code FROM sales.transactions where market_code='Mark004';`
    
    ![image](https://user-images.githubusercontent.com/61817305/159734328-b6630e6f-f33a-4eb4-adf5-1ba76424efec.png)


1. Show transactions where currency is US dollars

    `SELECT * from sales.transactions where currency="USD"`
![image](https://user-images.githubusercontent.com/61817305/159734640-be7914e3-31b4-41ed-acbf-5312e37677ba.png)

1. Alternate way to display transactions in USD currency along with the market  name 

    ` SELECT sales.transactions.*, sales.markets.* 
    FROM sales.transactions 
    INNER JOIN sales.markets 
    ON sales.transactions.market_code = sales.markets.markets_code
    WHERE currency = 'USD';`

![image](https://user-images.githubusercontent.com/61817305/159734805-48ea1f01-babb-4738-8a8d-6e82cb905d21.png)

1. Sum of sales in the year 2018

    `SELECT SUM(sales.transactions.sales_amount)
    FROM sales.transactions 
    INNER JOIN
    sales.date
    ON sales.transactions.order_date = sales.date.date
    WHERE year = '2018';`

![image](https://user-images.githubusercontent.com/61817305/159734938-67bfaaa8-39a0-4a6f-9c11-ef4ddb150084.png)

1. Sales transactions in January 2020

    `SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions 
    INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date
    where sales.date.year=2020 and sales.date.month_name="January";`

![image](https://user-images.githubusercontent.com/61817305/159735081-6fbc64f0-91b4-49df-891a-93c0efafca1e.png)

1. Count of currency 

    `SELECT count(*) from sales.transactions WHERE currency = 'USD';`
    
     `SELECT count(*) from sales.transactions WHERE currency = 'USD\r';`
     
     `SELECT count(*) from sales.transactions WHERE currency = 'INR';`
     
     `SELECT count(*) from sales.transactions WHERE currency = 'INR\r';`



    
# Data Analysis Using Power BI

Model Tab shows us the relation b/w different tables
![image](https://user-images.githubusercontent.com/61817305/159736285-9f5b389c-a9d3-44a2-9c21-8e451a4f9c9c.png)



1.  Remove records where zone was missing [New York, Paris]

    `= Table.SelectRows(sales_markets, each ([zone] <> ""))`
    
    ![image](https://user-images.githubusercontent.com/61817305/159736421-6c8c39e0-1924-4102-a0da-d4652e518f30.png)



1. Remove sales amount with value less than or equal to 0

    `= Table.SelectRows(sales_transactions, each ([sales_amount] <> -1 and [sales_amount] <> 0))`
    
    ![image](https://user-images.githubusercontent.com/61817305/159736519-04c1a2d6-f4e0-4949-a228-936bae75343e.png)
![image](https://user-images.githubusercontent.com/61817305/159736532-e9f7f5d4-5d60-4992-9311-7e9bc62a03a5.png)
![image](https://user-images.githubusercontent.com/61817305/159736560-c3015248-20ac-4bb7-bb4c-7710d1c33df4.png)



1. Formula to create norm_amount column

    `= Table.AddColumn(#"Filtered Rows", "norm_amount", each if [currency] = "USD" or [currency] ="USD#(cr)" then [sales_amount]*75 else [sales_amount], type any)`
![image](https://user-images.githubusercontent.com/61817305/159736624-a30d482d-8016-4168-85b9-6b42c6c455ee.png)
![image](https://user-images.githubusercontent.com/61817305/159736834-6e737435-bffe-41ae-8146-153d3c304af8.png)


1. Formula to convert USD currency into INR

    `= Table.AddColumn(#"Filtered Rows", "norm_amount", each if [currency] = "USD" or [currency] = "USD#(cr)" then [sales_amount]*75 else [sales_amount])`
![image](https://user-images.githubusercontent.com/61817305/159736962-5119fe49-e1e0-48a3-8bbd-f4056ff5f518.png)


1. Filter other currency values

    `= Table.SelectRows(#"Added Conditional Column", each ([currency] = "INR#(cr)" or [currency] = "USD#(cr)"))`


1. Revenue column inside new table  Base_measure 

    `Revenue = SUM('sales transactions'[sales_amount])`

1. Sales Quantity column inside new table  Base_measure

    `Sales_Quantity = SUM('sales transactions'[sales_qty])`
    
    
 ## Final Dashboard
 

 ![image](https://user-images.githubusercontent.com/61817305/159731423-7aa884df-a816-4399-a8ac-fa88443aa9a7.png)

