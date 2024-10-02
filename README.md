
# E-Commerce PostgreSQL Project

## Project Overview

This project involves building a relational database for an e-commerce company using PostgreSQL. The database stores essential information about customers, products, orders, employees, and the items in each order. The primary goal is to enable comprehensive analysis of customer behavior, product performance, sales trends, and employee performance using SQL queries.

### Key Features:
- **Structured Data Storage**: The project implements five core tables—Customers, Products, Orders, Order_Items, and Employees—to model real-world business data.
- **Data Analytics**: SQL queries are used to extract valuable insights, including customer purchasing habits, product sales data, order summaries, and employee salary analysis.

## Database Schema

The database consists of the following tables:

1. **Customers**:
    - Stores customer information, including personal details, contact information, and the date they joined.
    - Primary Key: `customer_id`

2. **Products**:
    - Stores product details, including product name, category, price, and stock quantity.
    - Primary Key: `product_id`

3. **Orders**:
    - Records all orders made by customers, including order date and total amount.
    - Primary Key: `order_id`
    - Foreign Key: `customer_id` references `Customers(customer_id)`

4. **Order_Items**:
    - Tracks the individual items in each order, including the product, quantity, and item price.
    - Primary Key: `order_item_id`
    - Foreign Key: `order_id` references `Orders(order_id)`
    - Foreign Key: `product_id` references `Products(product_id)`

5. **Employees**:
    - Contains employee details, including personal information, department, and salary.
    - Primary Key: `employee_id`

### Entity-Relationship Diagram (ERD)

The following relationships exist between the tables:
- **Customers** and **Orders**: One-to-Many relationship (one customer can have many orders).
- **Orders** and **Order_Items**: One-to-Many relationship (one order can contain multiple items).
- **Products** and **Order_Items**: One-to-Many relationship (a product can appear in many order items).
- **Employees**: Stand-alone entity containing details about company employees.

## Table Structures

### Customers Table:
```sql
CREATE TABLE Customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    phone VARCHAR(15),
    address VARCHAR(255),
    city VARCHAR(50),
    state VARCHAR(50),
    zip_code VARCHAR(10),
    join_date DATE
);
```

### Products Table:
```sql
CREATE TABLE Products (
    product_id SERIAL PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2),
    stock_quantity INT
);
```

### Orders Table:
```sql
CREATE TABLE Orders (
    order_id SERIAL PRIMARY KEY,
    order_date DATE,
    customer_id INT REFERENCES Customers(customer_id),
    total_amount DECIMAL(10, 2)
);
```

### Order_Items Table:
```sql
CREATE TABLE Order_Items (
    order_item_id SERIAL PRIMARY KEY,
    order_id INT REFERENCES Orders(order_id),
    product_id INT REFERENCES Products(product_id),
    quantity INT,
    item_price DECIMAL(10, 2)
);
```

### Employees Table:
```sql
CREATE TABLE Employees (
    employee_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    phone VARCHAR(15),
    hire_date DATE,
    department VARCHAR(50),
    salary DECIMAL(10, 2)
);
```

## Data Insertion

Sample data has been provided to populate the tables with realistic entries for customers, products, orders, and employees.

Example data insertion:
```sql
-- Insert sample customers
INSERT INTO Customers (first_name, last_name, email, phone, address, city, state, zip_code, join_date)
VALUES
('John', 'Doe', 'john.doe@xyz.com', '555-1234', '123 Main St', 'Springfield', 'IL', '62701', '2023-01-10'),
('Jane', 'Smith', 'jane.smith@xyz.com', '555-5678', '456 Elm St', 'Springfield', 'IL', '62701', '2023-02-15');

-- Insert sample products
INSERT INTO Products (product_name, category, price, stock_quantity)
VALUES
('Laptop', 'Electronics', 799.99, 50),
('Smartphone', 'Electronics', 599.99, 100);

-- Insert sample orders
INSERT INTO Orders (order_date, customer_id, total_amount)
VALUES
('2023-03-01', 1, 1399.98);

-- Insert sample order items
INSERT INTO Order_Items (order_id, product_id, quantity, item_price)
VALUES
(1, 1, 1, 799.99), (1, 2, 1, 599.99);

-- Insert sample employees
INSERT INTO Employees (first_name, last_name, email, phone, hire_date, department, salary)
VALUES
('Alice', 'Johnson', 'alice.johnson@xyz.com', '555-2345', '2022-05-15', 'Sales', 55000);
```

## Key SQL Queries

### 1. **Retrieve Customers Joined in 2023**
This query fetches all customers who joined in the year 2023.
```sql
SELECT * FROM Customers WHERE EXTRACT(YEAR FROM join_date) = 2023;
```

### 2. **List Electronics Products**
This query retrieves all products under the "Electronics" category.
```sql
SELECT * FROM Products WHERE category = 'Electronics';
```

### 3. **Total Number of Orders**
This query returns the total count of orders placed in the system.
```sql
SELECT COUNT(order_id) FROM Orders;
```

### 4. **Highest-Priced Product**
This query finds the most expensive product in the product catalog.
```sql
SELECT * FROM Products WHERE price = (SELECT MAX(price) FROM Products);
```

### 5. **Total Revenue Calculation**
This query calculates the total revenue generated from all orders.
```sql
SELECT SUM(item_price * quantity) AS total_revenue FROM Order_Items;
```

### 6. **Top 3 Products by Revenue**
This query lists the top 3 products that generated the highest revenue.
```sql
SELECT p.product_name, SUM(oi.item_price * oi.quantity) AS revenue
FROM Products p
JOIN Order_Items oi ON p.product_id = oi.product_id
GROUP BY p.product_name
ORDER BY revenue DESC
LIMIT 3;
```

### 7. **Customers Who Have Never Placed an Order**
This query lists customers who haven't placed any orders.
```sql
SELECT * FROM Customers WHERE customer_id NOT IN (SELECT customer_id FROM Orders);
```

### 8. **Employee Salary Analysis**
This query retrieves the average salary of employees in each department.
```sql
SELECT department, AVG(salary) AS avg_salary FROM Employees GROUP BY department;
```

## Analysis Use Cases

1. **Customer Insights**: Track new customer acquisitions and identify inactive customers who have never placed an order.
2. **Product Performance**: Analyze the best-performing products based on sales, track stock levels, and monitor high-revenue products.
3. **Order and Revenue Tracking**: Summarize total orders, revenue, and monitor purchasing trends.
4. **Employee Salary Comparison**: Compare employee salaries across departments, and identify departments with higher average pay.

## Setup Instructions

### Prerequisites:
- PostgreSQL installed on your machine.
- A SQL client or terminal access to execute SQL queries (e.g., pgAdmin, DBeaver).

### Database Setup:
1. Create a new PostgreSQL database:
    ```bash
    createdb e_commerce
    ```

2. Load the SQL script into the database:
    ```bash
    psql -d e_commerce -f e-commerce.sql
    ```

3. Run SQL queries using your preferred SQL client or via the command line.

## Conclusion

This PostgreSQL project provides a comprehensive foundation for managing and analyzing e-commerce data. It offers real-world business scenarios such as product sales tracking, customer analysis, and employee management. By using the queries included in this project, users can derive valuable insights to inform business decision-making.

