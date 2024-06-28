![image](https://github.com/NguyenMav/DataCamp_SQL_Advance_SQL_Associate_Certificate/assets/149219810/0b1b2ce7-6983-4db9-ae7a-ea310c05eef5)


# Exam 1 (Multiple Choice - 35 questions)

## Explanations of exam

You will be given 100-140 seconds per multiple choice question depending on the complexity of the question. The exam should take about exactly 1 hour if no breaks are taken, but if you require a break in between, you have 2 hours to finish everything.

There will be an implicit three sections in this exam: Database management with PostgreSQL, database querying, and database theory.

1. Database management with PostgreSQL will focus on really understanding the keywords specific to PostgreSQL like pg-datatype and also basic DML.

2. Database querying is pretty simple for this certificate, as long as you can do intermediate-level SQL, like joins and unions along with basic SQl you should be fine.

3. Database concepts require a good understanding of entity-relationship diagram along with the crow's foot notations, normalisations and the different database schemas like star/snowflake/physical etc. along with an understanding of unstructured/semi-structured/structured data and what schemas work best.

# Exam 2 (Practical - 4 tasks): Hotel Operations

I know it might be tempting to just copy and paste answers. But trying to do the exams on your own will be beneficial for your future. Certificates have very little value in the data field, it's more to validate the knowledge you have, so you shouldn't try to cheat yourself. The practical exam practice solutions in another repo I provided are more than enough to give you an idea of what to expect in the exam. Regardless of your intent, here's my solution for Exam 2.

## Explanations of the exam

Please be able to complete the practice exam for the practical portion of the SQL Associate certificate, it will be very useful in completing this.

Anyhow, the most important task that you have to get right is Task 1, which makes up about half the tests that need to pass to actually finish the practical exam. 

Task 1, it's about making sure the data is cleaned, meaning you have to answer the business requirements exactly as they say. Double-checked/triple-checked each field and their values. There may be nulls or even '-' or abbreviation of a value that's not supposed to be there.

Make sure your table needs to answer every field requirement according to the business requirement in Task 1 explanations.

The other 3 Tasks are fairly simple database querying, as long as you have intermediate-level SQL querying skills, it shouldn't be an issue.

## Business Scenario

LuxurStay Hotels is a major, international chain of hotels. They offer hotels for both business and leisure travellers in major cities across the world. The chain prides themselves on the level of customer service that they offer. 

However, the management has been receiving complaints about slow room service in some hotel branches. As these complaints are impacting the customer satisfaction rates, it has become a serious issue. Recent data shows that customer satisfaction has dropped from the 4.5 rating that they expect. 

You are working with the Head of Operations to identify possible causes and hotel branches with the worst problems. 

## Data

The following schema diagram shows the tables available. You have only been provided with data where customers provided a feedback rating.

# Task 1

Before you can start any analysis, you need to confirm that the data is accurate and reflects what you expect to see. 

It is known that there are some issues with the `branch` table, and the data team have provided the following data description. 

Write a query to return data matching this description. You must match all column names and description criteria.

| Column Name | Criteria                                                |
|-------------|---------------------------------------------------------|
|id | Nominal. The unique identifier of the hotel. </br>Missing values are not possible due to the database structure.|
| location | Nominal. The location of the particular hotel. One of four possible values, 'EMEA', 'NA', 'LATAM' and 'APAC'. </br>Missing values should be replaced with “Unknown”. |
| total_rooms | Discrete. The total number of rooms in the hotel. Must be a positive integer between 1 and 400. </br>Missing values should be replaced with the default number of rooms, 100. |
| staff_count | Discrete. The number of staff employeed in the hotel service department. </br>Missing values should be replaced with the total_rooms multiplied by 1.5. |
| opening_date | Discrete. The year in which the hotel opened. This can be any value between 2000 and 2023. </br>Missing values should be replaced with 2023. |
| target_guests | Nominal. The primary type of guest that is expected to use the hotel. Can be one of 'Leisure' or 'Business'. </br>Missing values should be replaced with 'Leisure'. |

```sql
-- Write your query for task 1 in this cell
-- Analyse each requirements for each field and then apply the necessary change
-- total_rooms has nulls, need to replace with 100 DONE
-- opening_date has '-', need to replace with 2023 DONE
-- target_guests has 'B.', need to replace with 'Business' as it's not a missing value DONE
SELECT
	id,
	location,
	COALESCE(total_rooms, '100') AS total_rooms,
	staff_count,
	CASE 
		WHEN opening_date = '-' THEN '2023'
		ELSE opening_date END AS opening_date,
	REPLACE(target_guests, 'B.', 'Business') AS target_guests
FROM branch;
```

| id | location | total_rooms | staff_count | opening_date | target_guests |
|----|----------|-------------|-------------|--------------|---------------|
| 1  | LATAM    | 168         | 178         | 2017         | Business      |
| 2  | APAC     | 154         | 82          | 2010         | Leisure       |
| 3  | APAC     | 212         | 467         | 2003         | Leisure       |
| 4  | APAC     | 230         | 387         | 2023         | Business      |
| 5  | APAC     | 292         | 293         | 2002         | Business      |
| 6  | NA       | 260         | 590         | 2022         | Leisure       |
| 7  | EMEA     | 259         | 442         | 2018         | Business      |
| 8  | NA       | 259         | 285         | 2023         | Business      |
| 9  | NA       | 157         | 274         | 2001         | Business      |
| 10 | EMEA     | 205         | 138         | 2013         | Leisure       |

100 rows

```sql
-- Original branch table that hasn't had data cleaning
SELECT *
FROM branch;
```
| id | location | total_rooms | staff_count | opening_date | target_guests |
|----|----------|-------------|-------------|--------------|---------------|
| 1  | LATAM    | 168         | 178         | 2017         | Business      |
| 2  | APAC     | 154         | 82          | 2010         | Leisure       |
| 3  | APAC     | 212         | 467         | 2003         | Leisure       |
| 4  | APAC     | 230         | 387         | -            | Business      |
| 5  | APAC     | 292         | 293         | 2002         | Business      |
| 6  | NA       | 260         | 590         | 2022         | Leisure       |
| 7  | EMEA     | 259         | 442         | 2018         | B.            |
| 8  | NA       | 259         | 285         | -            | Business      |
| 9  | NA       | 157         | 274         | 2001         | Business      |
| 10 | EMEA     | 205         | 138         | 2013         | Leisure       |

100 rows

# Task 2

The Head of Operations wants to know whether there is a difference in time taken to respond to a customer request in each hotel. They already know that different services take different lengths of time. 

Calculate the average and maximum duration for each branch and service. Your output should include the columns `service_id`, `branch_id`, `avg_time_taken` and `max_time_taken`. Values should be rounded to two decimal places where appropriate. 

```sql
-- Write your query for task 2 in this cell
-- Check to include all necessary fields DONE
-- Round aggregate functions like AVG and MAX to 2 decimal places where appropriate DONE
SELECT
	service_id,
	branch_id,
	ROUND(AVG(time_taken), 2) AS avg_time_taken,
	MAX(time_taken) AS max_time_taken
FROM request
GROUP BY service_id, branch_id;
```

| service_id | branch_id | avg_time_taken | max_time_taken |
|------------|-----------|----------------|----------------|
| 2          | 46        | 13.09          | 16             |
| 4          | 99        | 9.13           | 13             |
| 1          | 8         | 2.56           | 10             |
| 2          | 13        | 13.53          | 17             |
| 1          | 46        | 2.08           | 4              |
| 3          | 15        | 6.73           | 7              |
| 2          | 35        | 13.17          | 16             |
| 1          | 1         | 2.44           | 12             |
| 3          | 13        | 6.8            | 8              |

385 rows

# Task 3

The management team want to target improvements in `Meal` and `Laundry` service in Europe (`EMEA`) and Latin America (`LATAM`). 

Write a query to return the `description` of the service, the `id` and `location` of the branch, the id of the request as `request_id` and the `rating` for the services and locations of interest to the management team. 

Use the original branch table, not the output of task 1. 

```sql
-- Write your query for task 3 in this cell
-- Include all fields of interest DONE
-- Now narrow down to meal, laundry in EMA and LATAM DONE
-- Requires joins between all three tables DONE
SELECT
	service.description,
	branch.id,
	branch.location,
	request.id AS request_id,
	request.rating
FROM public.service
INNER JOIN public.request
	ON service.id = request.service_id
INNER JOIN public.branch
	ON branch.id = request.branch_id
WHERE (branch.location = 'EMEA' 
	OR branch.location = 'LATAM')
	AND 
	(service.description = 'Meal'
	OR service.description = 'Laundry');
```

# Service Requests

| description | id | location | request_id | rating |
|-------------|----|----------|------------|--------|
| Laundry     | 63 | EMEA     | 3          | 4      |
| Laundry     | 69 | LATAM    | 6          | 5      |
| Meal        | 44 | EMEA     | 18         | 4      |
| Laundry     | 57 | LATAM    | 19         | 3      |
| Meal        | 1  | LATAM    | 21         | 4      |
| Meal        | 26 | LATAM    | 26         | 5      |
| Laundry     | 34 | EMEA     | 27         | 4      |
| Laundry     | 60 | LATAM    | 35         | 4      |
| Meal        | 21 | EMEA     | 37         | 4      |

5047 rows

# Task 4

So that you can take a more detailed look at the lowest performing hotels, you want to get service and branch information where the average rating for the branch and service combination is lower than 4.5 - the target set by management.  

Your query should return the `service_id` and `branch_id`, and the average rating (`avg_rating`), rounded to 2 decimal places.

```sql
-- Write your query for task 4 in this cell
-- Include all fields required DONE
-- Perform AVG on rating with AS to avg_rating and rounded to 2 decimal places DONE
-- avg_rating lower than 4.5 DONE (performed with CTE not HAVING)
WITH A AS (SELECT 
	service_id,
	branch_id,
	ROUND(AVG(rating), 2) AS avg_rating
FROM request
GROUP BY service_id, branch_id)

SELECT 
	service_id,
	branch_id,
	avg_rating
FROM A
WHERE avg_rating < 4.50;
```

# Service Ratings

| service_id | branch_id | avg_rating |
|------------|-----------|------------|
| 2          | 46        | 3.78       |
| 4          | 99        | 3.83       |
| 1          | 8         | 3.64       |
| 1          | 46        | 3.81       |
| 3          | 15        | 4.00       |
| 2          | 35        | 3.76       |
| 1          | 1         | 3.66       |
| 1          | 57        | 3.64       |
| 1          | 41        | 3.77       |
| 3          | 57        | 3.53       |

215 rows
