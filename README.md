# Amazon Fresh Analytics - PostgreSQL Project

## Project Overview
This project focuses on data modeling and querying for an **Amazon Fresh Analytics** database using PostgreSQL. It includes designing an **ER diagram**, defining relationships, writing **DDL and DML statements**, applying constraints, and performing **complex queries** for data analysis.

## Database Design & Data Modeling
### **Task 1: ER Diagram Creation**
- Designed an **Entity-Relationship (ER) Diagram** to visualize table relationships, including **Customers, Products, Orders, Suppliers, and Reviews.**

### **Task 2: Identifying Primary and Foreign Keys**
- Defined **Primary Keys (PK)** and **Foreign Keys (FK)** for each table to maintain data integrity.
- Established table relationships, e.g., **Customers → Orders → Order Details → Products**.

### **Table Schema & Relationships**
| Table Name | Primary Key | Foreign Keys | Relationships |
|------------|-------------|----------------|----------------|
| customers | customer_id | None | Referenced in orders, reviews |
| orders | order_id | customer_id → customers(customer_id) | Referenced in order_details |
| order_details | (order_id, product_id) | order_id → orders, product_id → products | - |
| products | product_id | sub_category → subcategories, supplier_id → suppliers | Referenced in order_details, reviews |
| suppliers | supplier_id | None | Referenced in products |
| reviews | review_id | customer_id → customers, product_id → products | - |
| categories | category_id | None | Referenced in subcategories |
| subcategories | sub_category_id | category_id → categories | Referenced in products |

---
## SQL Queries
### **Task 3: Basic Queries**
- Retrieve customers from a specific city:
  ```sql
  SELECT * FROM customers WHERE city = 'Allenbury';
  ```
- Fetch all products under the **Fruits** category:
  ```sql
  SELECT * FROM Products WHERE category = 'Fruits';
  ```

---
## Data Definition Language (DDL)
### **Task 4: Creating and Altering Tables**
- **Adding Constraints:**
  ```sql
  ALTER TABLE Customers ADD CONSTRAINT customers_pk PRIMARY KEY (Customer_ID);
  ALTER TABLE Customers ADD CONSTRAINT Age_Check CHECK (Age >= 18);
  ALTER TABLE Customers ADD CONSTRAINT Name UNIQUE (Name);
  ```

---
## Data Manipulation Language (DML)
### **Task 5: Inserting Data**
```sql
INSERT INTO PRODUCTS (Product_ID, Product_Name, Category, Sub_Category, Price_Per_Unit, Stock_Quantity, Supplier_ID)
VALUES ('2aa28375-c563-41b5-aa33', 'However Fruit', 'Fruits', 'Sub-Fruits-1', 207, 290, '0658c953-98c4-4d00-bf29-4fbfe4aca4cd');
```

### **Task 6: Updating Stock Quantity**
```sql
UPDATE PRODUCTS SET Stock_Quantity = 400 WHERE Product_ID = '2aa28375-c563-41b5-aa33';
```

### **Task 7: Deleting a Supplier**
```sql
DELETE FROM suppliers WHERE city = 'West Linda';
```

---
## SQL Constraints & Aggregations
### **Task 8: Applying Constraints**
- **Ensuring ratings are between 1 and 5:**
  ```sql
  ALTER TABLE REVIEWS ADD CONSTRAINT Check_Rating_Range CHECK (Rating >= 1 AND Rating <= 5);
  ```
- **Setting a default value for Prime Member column:**
  ```sql
  ALTER TABLE CUSTOMERS ALTER COLUMN PRIME_MEMBER SET DEFAULT 'No';
  ```

### **Task 9: Aggregation Queries**
- **Finding orders placed after January 1, 2024:**
  ```sql
  SELECT * FROM Orders WHERE Order_Date > '2024-01-01';
  ```
- **Listing products with average ratings > 4:**
  ```sql
  SELECT Product_ID, AVG(Rating) AS AVERAGE_RATING FROM REVIEWS GROUP BY PRODUCT_ID HAVING AVG(RATING) > 4;
  ```

---
## ACID Transactions & TCL
### **Task 10: Handling Transactions**
- Deduct stock and insert order details in a single transaction.
```sql
BEGIN;
UPDATE Products SET Stock_Quantity = Stock_Quantity - 5 WHERE Product_ID = '2aa28375-c563-41b5-aa33';
INSERT INTO Order_Details (Order_ID, Product_ID, Quantity) VALUES ('order123', '2aa28375-c563-41b5-aa33', 5);
COMMIT;
```

---
## Real-World Analysis
### **Task 11: Identifying High-Value Customers**
- **Find top customers based on spending:**
```sql
SELECT c.Customer_ID, c.Name, SUM(o.Order_Amount) AS Total_Spending, RANK() OVER (ORDER BY SUM(o.Order_Amount) DESC) AS Rank
FROM Customers c JOIN Orders o ON c.Customer_ID = o.Customer_ID
GROUP BY c.Customer_ID, c.Name HAVING SUM(o.Order_Amount) > 5000;
```

---
## Joins & Complex Aggregations
### **Task 12: Revenue Per Order**
```sql
SELECT o.Order_ID, o.Customer_ID, SUM(od.Quantity * od.Unit_Price) AS Total_Revenue
FROM Orders o JOIN Order_Details od ON o.Order_ID = od.Order_ID
GROUP BY o.Order_ID, o.Customer_ID;
```

### **Task 13: Top 3 Selling Products**
```sql
SELECT product_id, total_revenue FROM (
  SELECT product_id, SUM(quantity * unit_price) AS total_revenue,
         RANK() OVER (ORDER BY SUM(quantity * unit_price) DESC) AS revenue_rank
  FROM order_details GROUP BY product_id) ranked_products
WHERE revenue_rank <= 3;
```

---
## Conclusion
This project demonstrates the design, implementation, and querying of a PostgreSQL database for **Amazon Fresh Analytics**. It includes:
- **ER Diagram & Data Modeling**
- **DDL & DML Statements**
- **Constraints & Aggregations**
- **ACID Transactions & Real-World Analysis**

These tasks provide a strong foundation for database management and SQL proficiency.

