# Data Analyst Job Market — SQL Project

**One-line:** SQL analysis to identify top-paying data analyst roles, in-demand skills, and which skills correlate with higher salaries.

**Goal:** Show my SQL work and results so employers can quickly see what I did and hire me.

---

## What I did
- Queried a job-postings dataset to answer five practical questions about the data analyst job market.
- Kept analysis reproducible: all SQL scripts are in `project_sql/`.
- Focused on remote-friendly roles and salary-based comparisons.

---

## Folder structure

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
- Results (tables/charts) are available in the `results/` folder or can be reproduced with the SQL scripts.
- **Want to hire me?** Add your preferred contact information below (GitHub / LinkedIn / email), or replace the placeholders.

**Contact:** `Your Name` — `your.email@example.com` — `https://github.com/yourusername` — `https://www.linkedin.com/in/yourprofile`

---

*This README is a condensed, hire-ready version of the original project README.*

