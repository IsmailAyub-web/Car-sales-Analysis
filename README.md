# Car Sales Data Analysis

This project involves managing and visualizing car sales data using a structured approach that combines MySQL for data storage and transformation, and Power BI for data visualization.

## Overview
The aim of the project is to:
1. Normalize and clean raw car sales data.
2. Create an ETL (Extract, Transform, Load) workflow to populate normalized tables.
3. Use Power BI to create insightful visualizations based on the data.

## Steps Involved

### 1. Database Creation
Created a MySQL database named `car_sales` and defined the following schema:

#### `sales` Table
This table stores the raw car sales data loaded from a CSV file.

```sql
CREATE TABLE sales (
    Date DATE,
    customer_name VARCHAR(20),
    dealer_name VARCHAR(20),
    company VARCHAR(20),
    model VARCHAR(20),
    Year YEAR,
    body_style VARCHAR(10),
    engine VARCHAR(100),
    transmission VARCHAR(20),
    color VARCHAR(10),
    price_in_thousands DOUBLE,
    dealer_address VARCHAR(20),
    customer_address VARCHAR(20),
    council_area VARCHAR(20),
    phone VARCHAR(10),
    gender VARCHAR(10),
    annual_income DOUBLE,
    dealer_no VARCHAR(20),
    dealer_region VARCHAR(20)
);
```

#### `customer` Table
This table contains customer-specific details:

```sql
CREATE TABLE customer (
    customer_name VARCHAR(20),
    customer_address VARCHAR(20),
    council_area VARCHAR(20),
    phone VARCHAR(10),
    gender VARCHAR(10),
    annual_income DOUBLE
);
```

#### `dealer` Table
This table contains dealer-specific details:

```sql
CREATE TABLE dealer (
    dealer_name VARCHAR(20),
    dealer_address VARCHAR(20),
    council_area VARCHAR(20),
    dealer_no VARCHAR(20),
    dealer_region VARCHAR(20)
);
```

#### `car` Table
This table contains car-specific details:

```sql
CREATE TABLE car (
    company VARCHAR(20),
    model VARCHAR(20),
    Year YEAR,
    body_style VARCHAR(10),
    engine VARCHAR(100),
    transmission VARCHAR(20),
    color VARCHAR(10),
    price_in_thousands DOUBLE
);
```

### 2. Data Loading
The raw data was loaded into the `sales` table from a CSV file using the following command:

```sql
LOAD DATA LOCAL INFILE 'C:/Users/DELL/Desktop/Analysis/car sales/Car_Sales.csv'
INTO TABLE sales
FIELDS TERMINATED BY ','
ENCLOSED BY '\n'
IGNORE 1 ROWS;
```

### 3. ETL Procedures
To populate the normalized tables, the following stored procedures were created:

#### `etl_customer` Procedure
Extracts customer-related data from `sales` and inserts it into `customer`:

```sql
CREATE PROCEDURE etl_customer()
BEGIN
    INSERT INTO customer (customer_name, customer_address, council_area, phone, gender, annual_income)
    SELECT customer_name, customer_address, council_area, phone, gender, annual_income FROM sales;
END;
```

#### `etl_dealer` Procedure
Extracts dealer-related data from `sales` and inserts it into `dealer`:

```sql
CREATE PROCEDURE etl_dealer()
BEGIN
    INSERT INTO dealer (dealer_name, dealer_address, council_area, dealer_no, dealer_region)
    SELECT dealer_name, dealer_address, council_area, dealer_no, dealer_region FROM sales;
END;
```

#### `etl_car` Procedure
Extracts car-related data from `sales` and inserts it into `car`:

```sql
CREATE PROCEDURE etl_car()
BEGIN
    INSERT INTO car (company, model, Year, body_style, engine, transmission, color, price_in_thousands)
    SELECT company, model, Year, body_style, engine, transmission, color, price_in_thousands FROM sales;
END;
```

### 4. Power BI Integration
Once the data was normalized, it was imported into Power BI to create visualizations such as:
- **Annual Income Distribution**: Histogram grouped by gender.
- **Regional Dealer Sales**: Bar chart showing sales by region.
- **Car Pricing Trends**: Line chart showing price trends over the years.
- **Popular Models and Companies**: Pie charts and rankings.

## How to Use
1. Clone this repository and set up the `car_sales` database using the provided SQL schema.
2. Load your own CSV file into the `sales` table.
3. Execute the ETL procedures to populate the normalized tables:
   ```sql
   CALL etl_customer();
   CALL etl_dealer();
   CALL etl_car();
   ```
4. Connect Power BI to the MySQL database and create custom visualizations.

## Dependencies
- **MySQL** for database management.
- **Power BI** for visualization.
- CSV file containing raw car sales data.

## Conclusion
This project demonstrates how to effectively manage and visualize raw sales data using a structured workflow. By normalizing the data and leveraging ETL processes, meaningful insights can be derived using visualization tools like Power BI.

---
Feel free to contribute to this repository by submitting issues or pull requests.

