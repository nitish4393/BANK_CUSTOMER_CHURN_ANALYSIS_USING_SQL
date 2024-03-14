# BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL

# What is Bank Churn?

Bank customer churn, also known as customer attrition, refers to the phenomenon where customers stop doing business with
a bank or switch to another bank. Churn is a critical metric for banks as it directly impacts their customer base and revenue.
Customer churn can occur due to various reasons, such as dissatisfaction with services, better offers from competitors, 
changes in financial needs, or poor customer experiences.

Understanding and predicting bank customer churn is crucial for banks to proactively manage customer relationships, improve customer
satisfaction, and reduce revenue loss. By identifying customers who are at a higher risk of churning, banks can implement targeted
retention strategies, personalized marketing campaigns, and tailored customer service to mitigate churn and enhance customer loyalty.

![bank_project](https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/4d437e77-4236-4ebd-abcc-51e3ff35acb1)


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
```sql
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
```

# A] Overview of the dataset:

```sql
select count(*) from bank_customer_churn;
```

<img width="108" alt="Screenshot 2024-03-13 233842" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/2f37a04c-9f11-48fd-8c6e-ef9ee979fd44">

### Q]Are there any missing values in the dataset?
```sql
 select count(distinct customer_id) AS TOTAL_UNIQUE_RECORDS
 from bank_customer_churn;
```
  
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

  -- Modify the data type of the age_category column in the bank_customer_churn table to varchar(20). This allows for
     storing more descriptive category labels.
  -- Update the age_category values by categorizing customers based on their age ranges using a CASE statement. This enhances
     data organization and facilitates further analysis.
  
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
# B] Exploratory Data Analysis (EDA):


## Q] What is the distribution of the target variable (churn)?
```SQL
select churn,count(churn) as tot ,
concat(round(COUNT(churn) * 100.0 / SUM(COUNT(churn)) OVER (),2),'%') AS churn_percentage
 FROM bank_customer_churn 
group by churn;
```
<img width="213" alt="Screenshot 2024-03-14 003713" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/9579372f-2502-4f15-98d1-68f0807585ff">



##  Q] How do the input variables (credit score, age, balance, etc.) vary with respect to churn?

```SQL
select credit_score_category,count(case when churn=1 then 1 end ) as tot_churn_customer
from bank_customer_churn
group by credit_score_category
order by tot_churn_customer desc;
```
<img width="185" alt="Screenshot 2024-03-14 003919" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/5634ff7c-d1e7-4367-a1db-91ad65573ce9">

```sql
select age_category,count(case when churn=1 then 1 end ) as tot_customer_churn
from bank_customer_churn
group by age_category
order by tot_customer_churn desc;
```
<img width="166" alt="Screenshot 2024-03-14 004329" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/633185f0-721d-4273-a7af-ad3a075bdaa2">

```sql
select gender,count(case when churn=1 then 1 end ) as tot_churn_customer
from bank_customer_churn
group by gender;
```
<img width="128" alt="Screenshot 2024-03-14 004449" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/65a8fd69-23cf-40fd-a3b2-7f15fc28a147">

```sql
select country,count(case when churn=1 then 1 end ) as tot_churn_customer
from bank_customer_churn
group by country
order by tot_churn_customer desc;
```
<img width="142" alt="Screenshot 2024-03-14 004659" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/f327b611-c354-4c9c-92f2-ed44a4f6ba97">

 ###  Q] Are there any correlations between the input variables and churn?
 
   - Approximately 81% of churn is contributed by customers categorized under the credit score categories of 'Poor', 'Fair', and 'Good'.
   - Among the total of 2037 churned customers, Middle-aged Adults accounted for 1279 (approximately 62.82%), while Mature Adults
     contributed 591 (approximately 29.01%) to the churn rate.
   - Out of the 2037 churned customers, approximately 55.93% were Female, and approximately 44.07% were Male.
   - Out of the 2037 churned customers, approximately 39.98% were from Germany, 39.75% were from France, and 20.27% were from Spain.

# c] Customer demographics analysis:

 ### Q] What is the distribution of customers by  country?
   ```SQL
   select country,count(*) as TOT_CUSTOMER
   from bank_customer_churn
   group by country
   ORDER BY TOT_CUSTOMER DESC;
   ```
<img width="124" alt="Screenshot 2024-03-14 085016" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/e0aec43c-451a-4858-b6be-aae832b33329">

 ### Q] How does churn vary across different demographic groups?

```sql
select country,count(case when churn=1 then 1 end ) as tot_churn_customer
from bank_customer_churn
group by country
order by tot_churn_customer desc;
```
<img width="133" alt="Screenshot 2024-03-14 085659" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/fe17feff-2b5a-45f3-af25-29561b6246bd">

  - INSIGHT
    - France has the highest number of total customers.
    - Germany has the highest number of churned customers, followed closely by France and then Spain.
    - Despite having a lower total number of customers compared to France, Germany has a higher number of churned customers,
      indicating a potentially higher churn rate.

# d] Product and service analysis:

 ### What is the distribution of customers by the number of products they have?
```sql
select products_number,count(*) as tot_customer
from bank_customer_churn
group by products_number
order by tot_customer desc;
```
<img width="140" alt="Screenshot 2024-03-14 091952" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/7bf29864-aa65-40b3-976b-201904a55ea1">


### How does churn vary based on the number of products a customer has?
```sql
select products_number,tot_churn_customer,tot_customer,
concat(round((tot_churn_customer / tot_customer) * 100,1),' %') as churn_rate from
(select products_number,count(case when churn=1 then 1 end ) as tot_churn_customer,count(*) as tot_customer
from bank_customer_churn
group by products_number
order by tot_churn_customer desc) as x;
```
<img width="274" alt="Screenshot 2024-03-14 093134" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/a32b796f-16c0-4407-99c6-1d7a8f527e60">


  - INSIGHT
    - high churn rate observed in product categories 3 and 4. This could involve factors like product pricing,
      competition, customer service quality, or lack of features.

# E] Customer behavior analysis:

### Q] How does tenure (length of time as a customer) vary with churn?

```SQL
SELECT tenure,count(case when churn=1 then 1 end ) as tot_churn_customer
from bank_customer_churn
group by tenure
order by tot_churn_customer desc;
```

###  Q] Do active members have lower churn rates compared to inactive members?

```SQL
select active_member,CONCAT(round(tot_churn_customer/tot,2) * 100,'%') as churn_rates from(
SELECT active_member,count(case when churn=1  then 1 end ) as tot_churn_customer,
count(*) tot
from bank_customer_churn
group by active_member
order by active_member desc) x;
```
<img width="135" alt="Screenshot 2024-03-14 100642" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/94d06904-4ebb-4bd7-a6f6-5f78991f20aa">

- INSIGHTS
   - Customers who are not active members (active_member = 0) have a higher churn rate at approximately 27.00%.
     Customers who are active members (active_member = 1) have a lower churn rate at approximately 14.00%.
   - Being an active member appears to be associated with a lower churn rate compared to inactive membership.
     This suggests that active engagement with the bank's services may contribute to higher customer retention rates.

### Are there any specific customer segments that are more prone to churn, and how can the bank address their needs?

```SQL
     WITH churn_data AS (
  SELECT credit_score_category,
         COUNT(CASE WHEN churn = 0 and active_member=0 THEN 1 END) AS tot
  FROM bank_customer_churn
  GROUP BY credit_score_category
)
SELECT
  credit_score_category,tot,
  round(tot * 100.0 / SUM(tot) OVER (),2) AS per
FROM churn_data
ORDER BY tot DESC;
```
<img width="218" alt="Screenshot 2024-03-14 102142" src="https://github.com/nitish4393/BANK_CUSTOMER_CHURN_ANALYSIS_USING_SQL/assets/120879393/0ce8718f-2386-44c2-9c86-b45c929f3085">

  - INSIGHT
     - The distribution of churned customers by credit score category indicates that a significant proportion of churned customers
        fall into the 'Fair', 'Poor', and 'Good' credit score categories.
     - Customers with lower credit scores ('Fair' and 'Poor') contribute to a larger portion of churn compared to those with higher
        credit scores ('Very Good' and 'Excellent').
     
 # RECOMMENDATIONS
   - Focus on Customer Engagement: Encourage inactive members to become more engaged with the bank's services by offering personalized incentives
     , targeted promotions, and improved customer support. Actively communicate with customers to understand their needs and preferences better.

   - Improve Product Offerings: Investigate the reasons behind the high churn rates observed in product categories 3 and 4. Conduct market research
     to understand customer expectations, competitors' offerings, and areas for improvement. Consider adjusting product pricing, enhancing customer
     service quality, or introducing new features to increase customer satisfaction and retention.

   - Targeted Marketing and Retention Strategies: Develop targeted marketing campaigns and retention strategies tailored to different customer
     segments based on factors such as credit score categories, age groups, and gender. Offer customized products, services, and incentives to
     address the specific needs and preferences of each segment.

   - Enhance Customer Service Quality: Invest in training and development programs to improve the quality of customer service provided by bank staff.
     Implement systems for collecting and analyzing customer feedback to identify areas for improvement and ensure timely resolution of customer issues.

   - Monitor and Address Churn Contributors: Continuously monitor key factors contributing to churn, such as credit score categories, age groups, and gender.
     Implement proactive measures to address these contributors, such as targeted interventions, loyalty programs, and product enhancements.Conduct
      focus groups or surveys with customers from 'Fair', 'Poor', and 'Good' credit score segments to understand their pain points and unmet needs.

   - Expand Market Reach: While France currently has the highest number of total customers, consider strategies to expand market reach in other regions,
      such as Germany and Spain. Develop localized marketing campaigns and product offerings to attract and retain customers in these regions.


      








