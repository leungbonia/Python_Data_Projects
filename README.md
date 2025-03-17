# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query hightlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm tartgeting. 

View my notebook with detailed stpes here:
[2_Skills_count.ipynb](3_Project\2_Skills_Count.ipynb)

### Visualise Data
```python

fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_percent[df_skills_percent['job_title_short'] == job_title].head(5)[::1]
    sns.barplot(data = df_plot, x = 'skill_percent', y='job_skills', ax = ax[i], hue= 'skill_count', palette='dark:b_r')

plt.show()
```

### Results
![Visualisation of Top Skills for Data roles](3_Project/images/skill_demand_roles.png)

### Insights
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineering (65%).
- SQL is the most requested skill for Data Analyst and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of job postings. 
- Data Engineers require more specialised technila skills (AWS, Azure, Spark) compared to Data Analyst and Data Scientists who are expected to be proficient in more generatl data management and analysis tools (Excel, Tableau). 

