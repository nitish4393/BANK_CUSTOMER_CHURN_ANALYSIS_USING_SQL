# BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL

# What is Bank Churn?

Bank customer churn, also known as customer attrition, refers to the phenomenon where customers stop doing business with a bank or switch to another bank. Churn is a critical metric for banks as it directly impacts their customer base and revenue. Customer churn can occur due to various reasons, such as dissatisfaction with services, better offers from competitors, changes in financial needs, or poor customer experiences.

Understanding and predicting bank customer churn is crucial for banks to proactively manage customer relationships, improve customer satisfaction, and reduce revenue loss. By identifying customers who are at a higher risk of churning, banks can implement targeted retention strategies, personalized marketing campaigns, and tailored customer service to mitigate churn and enhance customer loyalty.

# ABOUT DATA :-

## IN THIS  DATASET WE HAVE 12 COLUMN WITH 10000 ROWS

This dataset is for ABC Multistate bank with following columns:

customer_id,
credit_score,
country,
gender,
age,
tenure,
balance,
products_number,
credit_card,
active_member,
estimated_salary,
churn

# AIM :-

The aim of the data will be predicting the Customer Churn.

# ANALYSIS :-

create database portfoli;
use portfoli;

create table bank_customer_churn 
(customer_id int,
credit_score int,
country varchar(15),
gender varchar(8),
age int,
tenure int,
balance float,
products_number int,
credit_card int,
active_member int,
estimated_salary float,
churn int)

# A] Overview of the dataset:
This is an inline SQL code snippet:
`select count(*) from bank_customer_churn;`

<img width="108" alt="Screenshot 2024-03-13 233842" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/2f37a04c-9f11-48fd-8c6e-ef9ee979fd44">

### Q]Are there any missing values in the dataset?

 `select count(distinct customer_id) AS TOTAL_UNIQUE_RECORDS
 from bank_customer_churn;`
  
   - NO THERE ARE NO MISSING ITEMS
<img width="135" alt="Screenshot 2024-03-13 234320" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/92e871f8-2ab4-42c9-a0bf-243befbada1b">

  - In this case, you're aiming to enrich the bank_customer_churn table by incorporating a new column named credit_score_category.
  - This code snippet enhances the data by adding a new dimension (credit score category). This WILL be beneficial for further analysis and data visualization tasks.
```SQL
alter table bank_customer_churn
add column credit_score_category varchar(10);

update bank_customer_churn
set credit_score_category = CASE
    WHEN credit_score >= 800 THEN 'Excellent'
    WHEN credit_score >= 740 THEN 'Very Good'
    WHEN credit_score >= 670 THEN 'Good'
    WHEN credit_score >= 580 THEN 'Fair'
    ELSE 'Poor'
  END ;

  - Modify the data type of the age_category column in the bank_customer_churn table to varchar(20). This allows for storing more descriptive category labels.
  - Update the age_category values by categorizing customers based on their age ranges using a CASE statement. This enhances data organization and facilitates further analysis.
  
alter table bank_customer_churn
modify age_category varchar(20);


update bank_customer_churn
set age_category=CASE
    WHEN age < 18 THEN 'Minor'
    WHEN age >= 18 AND age < 30 THEN 'Young Adult'
    WHEN age >= 30 AND age < 50 THEN 'Middle-aged Adult'
    WHEN age >= 50 AND age < 65 THEN 'Mature Adult'
    ELSE 'Senior'
  END ;
  ```







