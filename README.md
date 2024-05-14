# Introduction
📊 Dive into the data job market! Focusing on Data Analyst roles, this project explores: ₹ top_paying jobs 🛠️ in-demand skills, and 📈 where high demand meets high salary in Data Analytics.

Check out the SQL queries here: [project_sql folder](/Project_sql/)
# Background
Inspired by a mission to analyze job market trends, salary data, and skill requirements across different sectors. By identifying the skills that command the highest salaries and are in high demand by employers, the project aims to provide valuable guidance to job seekers seeking to enhance their employability and maximize their earning potential.

Data hails from this amazing [SQL Course](https://www.lukebarousse.com/sql) by Luke Barousse. It's packed with insights on job titles, salaries,locations and essential skills.

### The Questions I wanted to answer through my SQL queries were:
1.	What are the top-paying data analyst jobs?
2.	What skills are required for these top-paying jobs?
3.	What skills are most in demand for data analysts?
4.	Which skills are associated with higher salaries?
5.	What are the most optimal skills to learn?

# Tools I Used
These are the tools that i used for my Analysis:
- **SQL:** The backbone of my analysis, allowing me to query the database and unearth critical insights.
- **Visual Studio Code:** My go-to for database management and executing SQL queries.
- **PostgreSQL:** The database management tool for handling data.
- **Git & GitHub:** Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.

# The Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market. Here’s how I approached each question:
### 1. Top Paying Data Analyst Roles
To identify the top 10 highest paying roles. I filtered Data Analyst Positions using their average yearly salary and location focusing on remote jobs. This query Highlights the high paying opportunities in the field.

```sql
SELECT
   job_id,
   job_title,
   job_location,
   job_schedule_type,
   salary_year_avg,
   job_posted_date,
   name AS company_name
FROM
   job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
   job_title_short = 'Data Analyst' AND
   job_location = 'Anywhere' AND
   salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10;
```
Here's the breakdown of the top data analyst jobs in 2023:

- **Wide Salary Range:** Top 10 paying data analyst roles span from $184,000 to $650,000, indicating significant salary potential in the field.
- **Diverse Employers:** Companies like SmartAsset, Meta, and AT&T are among those offering high salaries, showing a broad interest across different industries.
- **Job Title Variety:** There's a high diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specializations within data analytics.

![top_paying_jobs](https://github.com/akhilaa94/SQL_Project_Data_Job_Analysis/assets/169245384/ecfaaced-8429-4d98-b9de-0069dbb90560)

*This image was created using Microsoft Excel after exporting the results of the above query*

### 2. Skills for Top Paying Jobs
To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.

```sql
WITH top_paying_jobs AS (

    SELECT
    job_id,
    job_title,
    salary_year_avg,
    name AS company_name
    FROM
    job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
    job_title_short = 'Data Analyst' AND
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
    ORDER BY salary_year_avg DESC
    LIMIT 10
)

SELECT
top_paying_jobs.*,
skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC
```
Here's the breakdown of the most demanded skills for data analysts in India in 2023, based on job postings:

- **SQL** is the most demanded skills.
-	**Python** follows closely behind among the top-paying roles.
- **Tableau** is also highly sought after. Other skills like **R**, **Snowflake** **Pandas** and **Excel** show varying degrees of demand.

![top_paying_skills](https://github.com/akhilaa94/SQL_Project_Data_Job_Analysis/assets/169245384/9002f1e0-1073-4ebc-a433-088b02b80a0d)
*This image was created using Microsoft Excel after exporting the results of the above query*

### 3. In-Demand Skills for Data Analysts
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.

```sql
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5
```
Here's the breakdown of the most demanded skills for data analysts in 2023

**SQL** and **Excel** remain fundamental, emphasizing the need for strong foundational skills in data processing and spreadsheet manipulation.
Programming and Visualization Tools like **Python**, **Tableau**, and **Power BI** are essential, pointing towards the increasing importance of technical skills in data storytelling and decision support.


| **Skill**       | **Demand Count** |
|-----------------|------------------|
| SQL             | 92,628           |
| Excel           | 67,031           |
| Python          | 57,326           |
| Tableau         | 46,554           |
| Power BI        | 39,468           |

*Table of the demand for the top 5 skills in data analyst job postings*

### 4.  Skills With High Salaries
Exploring the average salaries associated with different skills revealed which skills are the highest paying Data Analyst jobs.
```sql
SELECT skills,
           Round(AVG(salary_year_avg),0) AS average_salary
    FROM   job_postings_fact
    INNER JOIN  skills_job_dim ON job_postings_fact.job_id=skills_job_dim.job_id
    INNER JOIN skills_dim ON  skills_job_dim.skill_id=skills_dim.skill_id
    WHERE  job_title_short ='Data Analyst'
           And salary_year_avg IS NOT NULL
    AND job_work_from_home= TRUE
    GROUP BY Skills
    ORDER BY average_salary DESC
    LIMIT 10
```
Here's a breakdown of the results for top paying skills for Data Analysts:

**High Demand for Big Data & ML Skills:** Top salaries are commanded by analysts skilled in big data technologies (PySpark, Couchbase), machine learning tools (DataRobot, Jupyter), and Python libraries (Pandas, NumPy), reflecting the industry's high valuation of data processing and predictive modeling capabilities.

**Software Development & Deployment Proficiency:** Knowledge in development and deployment tools (GitLab, Kubernetes, Airflow) indicates a lucrative crossover between data analysis and engineering, with a premium on skills that facilitate automation and efficient data pipeline management.

**Cloud Computing Expertise:** Familiarity with cloud and data engineering tools (Elasticsearch, Databricks, GCP) underscores the growing importance of cloud-based analytics environments, suggesting that cloud proficiency significantly boosts earning potential in data analytics.

| **Skill**         | **Average Salary ($)** |
|-------------------|---------------------------|
| PySpark           | $208,172                  |
| Bitbucket         | $189,155                  |
| Watson            | $160,515                  |
| Couchbase         | $160,515                  |
| DataRobot         | $155,486                  |
| GitLab            | $154,500                  |
| Swift             | $153,750                  |
| Jupyter           | $152,777                  |
| Pandas            | $151,821                  |
| Elasticsearch    | $145,000                  |
*Average Salary for the top 10 paying skills for Data Analysts*

###   5. Most Optimal Skills to Learn (High demand and High paying)
Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.
```sql
WITH skills_demand AS(
    SELECT
        skills_dim.skill_id,
        skills_dim.skills,
        COUNT(skills_job_dim.job_id) AS demand_count
    FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst'
        AND salary_year_avg IS NOT NULL
        AND job_work_from_home = True
    GROUP BY
        skills_dim.skill_id
), average_salary AS(
    SELECT
        skills_job_dim.skill_id,
        ROUND(AVG(salary_year_avg),2) AS avg_salary
    FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst'
        AND salary_year_avg IS NOT NULL
        AND job_work_from_home = True
    GROUP BY
        skills_job_dim.skill_id
)

SELECT
    skills_demand.skill_id,
    skills_demand.skills,
    demand_count,
    avg_salary
FROM
    skills_demand
INNER JOIN average_salary ON skills_demand.skill_id = average_salary.skill_id
WHERE
    demand_count>10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25
```
| **Skill**         | **Demand Count** | **Average Salary (USD)** |
|-------------------|------------------|---------------------------|
| Go                | 27               | $115,319.89               |
| Confluence        | 11               | $114,209.91               |
| Hadoop            | 22               | $113,192.57               |
| Snowflake         | 37               | $112,947.97               |
| Azure             | 34               | $111,225.10               |
| BigQuery          | 13               | $109,653.85               |
| AWS               | 32               | $108,317.30               |
| Java              | 17               | $106,906.44               |
| SSIS              | 12               | $106,683.33               |
| Jira              | 20               | $104,917.90               |
| Oracle            | 37               | $104,533.70               |
| Looker            | 49               | $103,795.30               |
| NoSQL             | 13               | $101,413.73               |
| Python            | 236              | $101,397.22               |
| R                 | 148              | $100,498.77               |
| Redshift          | 16               | $99,936.44                |
| Qlik              | 13               | $99,630.81                |
| Tableau           | 230              | $99,287.65                |
| SSRS              | 14               | $99,171.43                |
| Spark             | 13               | $99,076.92                |
| C++               | 11               | $98,958.23                |
| SAS               | 63               | $98,902.37                |
| SQL Server        | 35               | $97,785.73                |
| JavaScript        | 20               | $97,587.00                |

*Table of the most optimal skills for data anlayst sorted by salary*

Here's a breakdown of the most optimal skills for Data Analysts in 2023:

**•	High-Demand Programming Languages:** Python and R stand out for their high demand, with demand counts of 236 and 148 respectively. Despite their high demand, their average salaries are around $101,397 for Python and $100,499 for R, indicating that proficiency in these languages is highly valued but also widely available.

**•	Cloud Tools and Technologies:** Skills in specialized technologies such as Snowflake, Azure, AWS, and BigQuery show significant demand with relatively high average salaries, pointing towards the growing importance of cloud platforms and big data technologies in data analysis.

**•	Business Intelligence and Visualization Tools:** Tableau and Looker, with demand counts of 230 and 49 respectively, and average salaries around $99,288 and $103,795, highlight the critical role of data visualization and business intelligence in deriving actionable insights from data.

**•	Database Technologies:** The demand for skills in traditional and NoSQL databases (Oracle, SQL Server, NoSQL) with average salaries ranging from $97,786 to $104,534, reflects the enduring need for data storage, retrieval, and management expertise.



#  Conclusions

## Insights:
**1.	Top-Paying Data Analyst Jobs:** The top paying remote Data Analyst job offers the highest average yearly salary of $650,000 !

**2.	Skill For Top Paying Jobs:** Top paying Data Analyst jobs requires high proficiency in SQL. It is the most critical skill for earning a high salary.

**3.	Most Demanded Skill:** SQL is also the most demanded skill for data analyst jobs, making it an essential skill.

**4.	Skill with High Salaries:** Skills like PySpark, bitbucket, Watson are the specialized skills associated with highest average salary

**5.	Optimal Skills:** SQL is the most sought after skill in the whole market so it becomes the optimal skill for data analysts to maximise their worth when applying for jobs.


## Closing thoughts:
This project enhanced my SQL skills and provided valuable insights into the data analyst job market. The findings from the analysis serve as a guide to prioritizing skill development and job search efforts. Aspiring data analysts can better position themselves in a competitive job market by focusing on high-demand, high-salary skills. This exploration highlights the importance of continuous learning and adaptation to emerging trends in the field of data analytics.

