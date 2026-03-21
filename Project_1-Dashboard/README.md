# Salary Dashboard

![Salary Dashboard](/0_Resources/Images/1_Salary_Dashboard_Final_Dashboard.gif)

## Overview

An interactive Excel dashboard for exploring compensation across data-related roles. Built on a real-world dataset of 2023 job listings, it lets users filter by job title, country, and schedule type to surface median salaries.

**Dashboard file:** [1_Salary_Dashboard.xlsx](1_Salary_Dashboard.xlsx)  
**Dataset:** [Resources Folder](/0_Resources) - job titles, salaries, locations, and skills from 2023 data science listings.

---

## Build

### Charts

#### Bar Chart - Data Science Job Salaries

![Bar Chart](/0_Resources/Images/1_Salary_Dashboard_Chart1.png)

A horizontal bar chart of median salaries by job title, sorted descending. The tier structure is stark: Senior Data Scientist, Machine Learning Engineer, and Senior Data Engineer all sit at $150K–$155K, forming a clear top bracket. Data Scientist, Data Engineer, and Software Engineer cluster in the $125K–$130K range. Then there's a visible drop - Cloud Engineer at $115K, Senior Data Analyst at $105K - before Analyst roles bottom out around $90K. The gap between the top bracket and the Analyst floor is $65K. Role level, not just seniority, is the primary driver.

#### Map Chart - Country Median Salaries

![Country Map](/0_Resources/Images/1_Salary_Dashboard_Country_Map.gif)

A color-coded global map plotting median salary by country. Large portions of Africa, Central Asia, and parts of Southeast Asia show no data - gray, not low-paying, just absent from the dataset. The coverage itself is a finding: the 2023 data science job market, as captured here, is concentrated in North America, Europe, Australia, and pockets of South America and Asia. 

---

### Formulas and Functions

#### Median Salary by Job Title
```excel
=MEDIAN(
IF(
    (jobs[job_title_short]=A2)*
    (jobs[job_country]=country)*
    (ISNUMBER(SEARCH(type,jobs[job_schedule_type])))*
    (jobs[salary_year_avg]<>0),
    jobs[salary_year_avg]
)
)
```

An array formula combining MEDIAN() and IF() to filter across four criteria simultaneously - job title, country, schedule type, and non-blank salaries. Powers the background table that feeds the dashboard's dynamic salary display.

![Background Table](/0_Resources/Images/1_Salary_Dashboard_Screenshot1.png)

![Dashboard Implementation](/0_Resources/Images/1_Salary_Dashboard_Job_Title.png)

#### Job Schedule Type - Filtered List
```excel
=FILTER(J2#,(NOT(ISNUMBER(SEARCH("and",J2#))+ISNUMBER(SEARCH(",",J2#))))*(J2#<>0))
```

Generates a clean list of unique schedule types by stripping out combined entries (those containing "and" or commas) and zero values. Feeds the schedule type dropdown in the dashboard.

![Background Table](/0_Resources/Images/1_Salary_Dashboard_Screenshot2.png)

![Dashboard Implementation](/0_Resources/Images/1_Salary_Dashboard_Type.png)

---

### Data Validation

The filtered schedule type list is applied as a data validation rule across the Job Title, Country, and Type dropdowns. This matters here specifically because the median salary formula depends on exact matches - any inconsistent or free-text entry would silently break the output rather than throw an error.

![Final Dashboard](/0_Resources/Images/1_Salary_Dashboard_Final_Dashboard.gif)

---

## Conclusion

The dashboard makes one thing immediately clear: compensation in data roles is tiered, not graduated. The $65K gap between a Senior Data Scientist at $155K and a Data Analyst at $90K isn't a smooth climb - it's two distinct markets sitting on the same chart. Location and schedule type refine the picture, but the bar chart settles the primary question before you touch a dropdown. For a deeper look at which skills drive those salary differences and how demand varies across the market, see [Project 2](/Project_2-Analysis).