# Data Analyst Job Market — SQL Project

**One-line:** SQL analysis to identify top-paying data analyst roles, in-demand skills, and which skills correlate with higher salaries.

**Goal:** Show my SQL work and results so employers can quickly see what I did and hire me.

---

## What I did
- Queried a job-postings dataset to answer five practical questions about the data analyst job market.
- Kept analysis reproducible: all SQL scripts are in `project_sql/`.
- Focused on remote-friendly roles and salary-based comparisons.

---

## Quick summary of the analyses (what to look for)
1. **Top-paying Data Analyst Jobs** — list top remote-paying roles.
2. **Skills for Top-Paying Jobs** — which skills appear in the highest-paid roles.
3. **Most In-Demand Skills** — which skills appear most often in job listings.
4. **Skills Based on Salary** — average salary per skill.
5. **Optimal Skills to Learn** — skills that combine high demand and high salary.

---

## Key SQL queries (same as used in the project)

### 1) Top Paying Data Analyst Jobs
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
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```
![alt text](1_top_paying_roles.png)
*ChatGPT generated this graph from my SQL query results*
### 2) Skills for Top Paying Jobs
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
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```
![alt text](2_top_paying_roles_skills.png)
*ChatGPT generated this graph from my SQL query results*
### 3) In-Demand Skills for Data Analysts
```sql
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_work_from_home = True
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```
| Skills   | Demand Count |
|----------|--------------|
| SQL      | 7291         |
| Excel    | 4611         |
| Python   | 4330         |
| Tableau  | 3745         |
| Power BI | 2609         |

*Table of the demand for the top 5 skills in data analyst job postings*
### 4) Skills Based on Salary
```sql
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```
| Skills        | Average Salary ($) |
|---------------|-------------------:|
| pyspark       |            208,172 |
| bitbucket     |            189,155 |
| couchbase     |            160,515 |
| watson        |            160,515 |
| datarobot     |            155,486 |
| gitlab        |            154,500 |
| swift         |            153,750 |
| jupyter       |            152,777 |
| pandas        |            151,821 |
| elasticsearch |            145,000 |

*Table of the average salary for the top 10 paying skills for data analysts*
### 5) Most Optimal Skills to Learn
```sql
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```
| Skill ID | Skills     | Demand Count | Average Salary ($) |
|----------|------------|--------------|-------------------:|
| 8        | go         | 27           |            115,320 |
| 234      | confluence | 11           |            114,210 |
| 97       | hadoop     | 22           |            113,193 |
| 80       | snowflake  | 37           |            112,948 |
| 74       | azure      | 34           |            111,225 |
| 77       | bigquery   | 13           |            109,654 |
| 76       | aws        | 32           |            108,317 |
| 4        | java       | 17           |            106,906 |
| 194      | ssis       | 12           |            106,683 |
| 233      | jira       | 20           |            104,918 |

*Table of the most optimal skills for data analyst sorted by salary*
---

## How to run
1. Open this repo in VS Code or any editor that works with SQL.
2. Load the dataset into PostgreSQL (if not already loaded).
3. Run the SQL files from `project_sql/` using `psql`, a GUI client, or VS Code SQL runner.
4. Export results as CSV or visualize in your preferred tool.

---

## Tools & Skills demonstrated
- PostgreSQL / SQL (advanced queries, joins, CTEs, aggregation)
- Data analysis mindset: turning business questions into repeatable SQL queries
- Reproducible project structure suitable for GitHub

---

## Results & contact
- Results (SQL command files) are available in the [Open project_sql folder](./project_sql/) folder or can be reproduced with the SQL scripts.
- **Want to hire me?** `Nicolas Zanni` — `zannimoralesnicolas@gmail.com` — `https://github.com/zannicolas` — `https://www.linkedin.com/in/nicolas-zanni`.


