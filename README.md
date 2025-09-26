# Elevate Labs Task 4

# SQL for Data Analysis

# Ecommerce SQL Analysis — Customers table

This small project demonstrates core SQL skills by creating an ecommerce database and a customers table, inserting sample rows, and using queries to explore and analyze the data. The README below describes what I implemented, the queries I ran, how to reproduce the work locally, and next steps.

# What I built

I created a minimal e-commerce schema focused on customer data and performed basic to intermediate SQL analysis on it. Highlights:

Created database ecommerce and table customers with fields for id, unique id, zip prefix, city, and state.

Inserted 10 representative customer rows.

Wrote queries demonstrating selection, filtering, sorting, grouping, subqueries, views, and basic optimization with indexes.

Files included in this repository

ecommerce_customers.sql — full SQL script: database + table creation, INSERTs, queries, view creation, and index statements.

screenshots/ — results grid screenshots for each major query (use these for submission).

README.md — this file describing the project and how to run it.

SQL implemented (summary)

The SQL script contains the following main parts.

Schema and data (run first):

        CREATE DATABASE ecommerce;
        USE ecommerce;
        
        CREATE TABLE customers (
          customer_id VARCHAR(50) PRIMARY KEY,
          customer_unique_id VARCHAR(50),
          customer_zip_code_prefix INT,
          customer_city VARCHAR(50),
          customer_state VARCHAR(2)
        );
        
        INSERT INTO customers VALUES
        ('06b8...bc7','861e...ebb0',14409,'franca','SP'),
        ('1895...ac77','290c...33dc3',9790,'sao bernardo do campo','SP'),
        -- (8 more rows)
        ('4b71...fa75','9afe...71f22',30575,'belo horizonte','MG');


Key analysis queries and their purpose:

        SELECT / WHERE / ORDER BY
        
        List customers in a state and sort by city to inspect local distribution.
        
        SELECT customer_id, customer_city
        FROM customers
        WHERE customer_state = 'SP'
        ORDER BY customer_city;
        
        GROUP BY and aggregates
        
        Count customers per state to find geographic concentration.
        
        SELECT customer_state, COUNT(*) AS num_customers
        FROM customers
        GROUP BY customer_state;


Subquery

Find cities that belong to the state with the highest customer count.

        SELECT customer_city
        FROM customers
        WHERE customer_state = (
          SELECT customer_state
          FROM customers
          GROUP BY customer_state
          ORDER BY COUNT(*) DESC
          LIMIT 1
        );
        
        
        View for repeated analysis
        
        Create a reusable summary of customers by state.
        
        CREATE VIEW customers_by_state AS
        SELECT customer_state, COUNT(*) AS total_customers
        FROM customers
        GROUP BY customer_state;


Indexes for optimization

Add an index on city to speed up city-based lookups.

        CREATE INDEX idx_city ON customers(customer_city);

MySQL Workbench

Open MySQL Workbench and connect to your local MySQL server.

Open the file ecommerce_customers.sql in the SQL editor.

Execute the script (use the lightning bolt icon). This creates the database, table, inserts data, creates view and indexes.

Run the individual SELECT queries found in the script; capture the results grid as screenshots and save them into the screenshots/ folder.

Saved the SQL file as your deliverable and include the screenshots with your report.
Saw how a small dataset can be summarized quickly and how views help reuse logic.

Next steps: add orders and order_items tables to practice JOINs and aggregations like SUM and AVG, increase sample size to test index effectiveness, and create a few performance-oriented EXPLAIN plans.
