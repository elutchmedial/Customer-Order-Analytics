
# Customer & Order Analytics

The Customer & Order Analytics project provides SQL queries for analyzing sales data in the BIT_DB database. The queries can be used to answer various business questions, such as how many orders were placed in January, which product was the cheapest one sold in January, and how many customers ordered more than 2 products at a time in February. The project includes a set of SQL queries that are designed to provide insights into sales data, and can be easily adapted for other data sets or databases.

## Queries

Here are the SQL queries included in the project:

### How many orders were placed in January?

```sql
SELECT COUNT(orderid)
FROM BIT_DB.JanSales
WHERE length(orderid) = 6 
AND orderid <> 'Order ID'
```

### How many of those orders were for an iPhone?

```sql
SELECT COUNT(orderid)
FROM BIT_DB.JanSales
WHERE Product='iPhone'
AND length(orderid) = 6 
AND orderid <> 'Order ID'
```

### Select the customer account numbers for all the orders that were placed in February.

```sql
SELECT distinct acctnum
FROM BIT_DB.customers cust
INNER JOIN BIT_DB.FebSales Feb
ON cust.order_id=FEB.orderid
WHERE length(orderid) = 6 
AND orderid <> 'Order ID'
```

### Which product was the cheapest one sold in January, and what was the price?

```sql
SELECT distinct product, price 
FROM BIT_DB.JanSales 
ORDER BY price ASC LIMIT 1
```

### What is the total revenue for each product sold in January? 

```sql
SELECT SUM(quantity)*price as Revenue, product
FROM BIT_DB.JanSales
GROUP BY product
```

### Which locations in New York received at least 3 orders in January, and how many orders did they each receive? (Hint: use HAVING).

```sql
SELECT distinct location, COUNT(ORDERid)
FROM BIT_DB.JanSales
WHERE location LIKE '%NY%' AND length(orderid) = 6
AND orderid <> 'Order ID'
GROUP BY location
HAVING count(orderid)>2
```

### How many of each type of headphone were sold in February?

```sql
SELECT sum(Quantity) as quantity, Product
FROM BIT_DB.FebSales 
WHERE Product like '%Headphones%'
GROUP BY Product
```

### What was the average amount spent per account in February?

```sql
SELECT avg(quantity*price)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers cust
ON feb.orderid = cust.oderid
WHERE length(orderid) = 6
AND orderid <> 'Order ID'
```

### What was the average quantity of products purchased per account in February?

```sql
SELECT sum(quantity)/count(cust.acctnum)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers cust
ON feb.orderid  = cust.orderid
WHERE length(orderid) = 6
AND orderid <> 'Order ID'
```

### Which product brought in the most revenue in January and how much revenue did it bring in total?

```sql
SELECT product, sum(quantity*price)
FROM BIT_DB.JanSales
GROUP BY product
ORDER BY sum(quantity*price) desc
LIMIT 1
```

### Which products were sold in February at 548 Lincoln St, Seattle, WA 98101, how many of each were sold, and what was the total revenue?

```sql
SELECT product, (Quantity)
SUM(quantity)* price as Revenue
FROM BIT_DB.FebSales 
WHERE location = '548 Lincoln St, Seattle, WA 98101'
GROUP BY
