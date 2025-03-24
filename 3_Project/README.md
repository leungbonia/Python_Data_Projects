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

## 2. How are in-demand skills trending for Dat Anlaysts?

### Visualise Data

```python

from matplotlib.ticker import PercentFormatter
df_plot = df_DA_US_percent.iloc[:, :5]

#plot
sns.lineplot(data = df_plot, dashes = False, palette='tab10')
sns.set_theme(style = 'ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analyst in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))
 #get current axis

for i in range(5):
    if i == 3 :
        plt.annotate(df_plot.columns[i], (11.2, df_plot.iloc[-1, i]-3))
    else:
        plt.annotate(df_plot.columns[i], (11.2, df_plot.iloc[-1, i]))

plt.tight_layout()
plt.show()
```
### Results

![Trending Top Skills for Data Analysts in the US in 2023](3_Project\images\trending_skills.png)
*Bar graph visualising the trending top skills for data analysts int he UK in 2023.*

### Insights

- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand. 
- Excel experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of the year. 
- Both Python and Tableau show realatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. Power BI while less demanded compared to the others, shows a slight upward trend towards the year's end. 

## 3. How well do jobs and skills pay for Data Analyst?

### Salary Analysis for Data roles

#### Visualise data
```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order = job_order)
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
#### Results
![Salary Distributions of Data Jobs in the US](3_Project\images\salary_analysis.png)
*Box plot showing the salary distribution for the top 6 data job titles.*

#### Insights
- There's a significant variation in salary range across different job titles. 

### Hightest Paid and Most Demanded Skills for Data Analysts

```python
fig, ax = plt.subplts(2, 1)

#Top 10 Highest paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

#Top 10 Most In-Demand Skills for Data Analysts
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```

#### Results
![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](3_Project\images\highest_paid_most_demand_skills.png)
*The highest paid skills and most in-demand skills for data analysts in the US*

#### Insights
- Highest paying skills may not be in demand. 
