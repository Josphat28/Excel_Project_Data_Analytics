# Salary Analysis

## Overview

A data-driven investigation into what the 2023 data job market actually rewards. Using Excel's Power tools for a full ETL-to-insight workflow, this project examines four questions about compensation, skill demand, and market patterns.

**Dataset:** [Resources Folder](/0_Resources) - real-world data science job listings from 2023, covering job titles, salaries, locations, and skills.

---

## Questions

1. Do more skills get you better pay?
2. What is the salary for data jobs across different regions?
3. What are the top skills of data professionals?
4. What is the pay for the top 10 skills?

---

## Q1. Do more skills get you better pay?

### Tool: Power Query (ETL)

The raw data came in two separate tables - one with job information, one with skills per job ID. Power Query was used to extract, clean, and load both into the workbook before analysis.

**Transformations applied:**
- Column types corrected
- Unnecessary columns removed
- Text cleaned and whitespace trimmed

`data_jobs_all` after transform:

![data_jobs_all](/0_Resources/Images/2_Project_Analysis_Screenshot1.png)

`data_jobs_skills` after transform:

![data_jobs_skills](/0_Resources/Images/2_Project_Analysis_Screenshot2.png)

Both tables loaded into the workbook:

![Loaded - data_jobs_all](/0_Resources/Images/2_Project_Analysis_Screenshot3.png)

![Loaded - data_jobs_skills](/0_Resources/Images/2_Project_Analysis_Screenshot4.png)

### Analysis

![Skills vs Salary Chart](/0_Resources/Images/2_Project_Analysis_Chart1.png)

There is a positive correlation between skills required and median salary, but it's not uniform. Engineering roles - Data Engineer and Senior Data Engineer - sit at both extremes: highest skill demand (~8 skills per posting) and highest pay ($125K–$150K). Data Scientist and Senior Data Scientist pay comparably well ($130K–$155K) while requiring roughly half the skills (~5), suggesting that depth in a specific domain can substitute for breadth. Analyst roles cluster at the bottom on both axes. The pattern isn't simply "more skills, more pay" - it's that the roles commanding top salaries are either engineering-heavy or scientifically specialized. The middle of the skill range is where the correlation is weakest.

---

## Q2. What is the salary for data jobs across different regions?

### Tool: PivotTables and DAX

A PivotTable was built on the Power Pivot data model, with `job_title_short` in rows and `salary_year_avg` in values. Two DAX measures were created - one for overall median salary, one filtered to United States jobs only - to enable a direct US vs. non-US comparison.

**Overall median salary:**
```
Median Salary := MEDIAN(data_jobs_all[salary_year_avg])
```

**US median salary:**
```
=CALCULATE(
    MEDIAN(data_jobs_all[salary_year_avg]),
    data_jobs_all[job_country] = "United States")
```

### Analysis

![Regional Salary Chart](/0_Resources/Images/2_Project_Analysis_Chart2.png)

The US premium exists but it's role-specific, not uniform across seniority. Machine Learning Engineers see the sharpest gap - $150K in the US against $101K non-US, a $49K difference. Software Engineers follow at $125K vs $89K. But Data Engineers and Senior Data Engineers show almost no premium - $125K vs $123.5K and $150K vs $147.5K respectively. Data Analysts show zero gap: $90K in both markets. The US market doesn't uniformly reward seniority more - it rewards specific roles more. Engineering and infrastructure roles appear to have converged globally; the US advantage concentrates in ML and generalist software positions.

---

## Q3. What are the top skills of data professionals?

### Tool: Power Pivot

A data model was built by linking `data_jobs_all` and `data_jobs_skills` on `job_id` — a relationship Power Pivot established automatically since the data had already been cleaned in Power Query.

![Data Model](/0_Resources/Images/2_Project_Analysis_Screenshot5.png)

![Power Pivot Menu](/0_Resources/Images/2_Project_Analysis_Screenshot6.png)

With the data model established and both tables linked, the analysis could now surface skill frequency across all job postings - not just within a single table.

### Analysis

![Top Skills Chart](/0_Resources/Images/2_Project_Analysis_Chart3.png)

SQL and Python are in a different category from everything else - appearing in roughly 70% and 65% of postings respectively. The next closest is AWS at ~43%, and from there the list drops steadily through a cluster of infrastructure tools (Spark, Azure, Snowflake, Java, Hadoop, Kafka, NoSQL) all sitting below 32%. The gap between Python and AWS is larger than the gap between AWS and the bottom of the list. SQL and Python aren't just common - they're structural requirements. Everything else is role-specific.

---

## Q4. What is the pay for the top 10 skills?

### Tool: Pivot Chart (Combo)

A combo PivotChart was built to plot two metrics simultaneously:
- **Primary axis (Clustered Column):** Median salary
- **Secondary axis (Line with Markers):** Skill likelihood (%)

This allows both dimensions - how much a skill pays and how commonly it appears - to be read in a single view.

### Analysis

![Skill Pay Chart](/0_Resources/Images/2_Project_Analysis_Chart4.png)

The salary range across all 10 skills is surprisingly compressed - from $82K (Word) to $98K (Python), a $16K spread. The more telling signal is the relationship between likelihood and pay. Python leads on salary and still appears in 30% of postings - high reward, high demand. Oracle pays nearly as well ($95K) but appears in only 7% of postings - a specialization premium for a tool the market needs but few people have. SQL is the inverse: highest likelihood (52%) but mid-table on salary ($92K). Ubiquity has priced it in. Excel appears in 40% of postings but pays $85K - widely required, poorly compensated. The market is telling you something: being common isn't the same as being valuable.

---

## Conclusion

Four questions, one consistent pattern underneath all of them: the data job market prices specificity, not volume.

Skills matter, but not linearly - Data Scientists reach $130K–$155K with ~5 skills while Data Engineers need ~8 to hit the same ceiling. The roles at the top aren't simply doing more; they're doing something specific. The US salary premium follows the same logic - Machine Learning Engineers see a $49K gap between US and non-US pay while Data Analysts see none. Geography amplifies specialization; it doesn't create it.

The skill demand picture reinforces this. SQL appears in 70% of postings and pays $92K. Oracle appears in 7% and pays $95K. The most common skill in the market earns less than one of the rarest. Ubiquity without scarcity is not leverage.

The market is legible if you read it correctly: depth in the right place compounds. Breadth alone doesn't.