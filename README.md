# The Analysis

## 1. Getting the most demanded skills for the top 3 popular data roles

To find the most demanded skills for the top 3 most popular data roles from the dataset. i filtered out those positions by which were most popular , and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills i should pay attention to to depending on the targeted role.

view my notebook with detailed steps here:
[test.ipynb](test.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0,78)
    ax[i].legend().set_visible(False)

    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n , f'{v:.0f}%', va='center')


fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5)
plt.show()
```

### Results

![Visualization of Top Skills for Data Roles in the United States](images\chart.png)

### Insights

- Python is a versatile skill,
  highly demanded across all three roles, but most prominently for Data Scientists (72%)
  and Data Engineers (65%).
- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over
  half the job postsings for both roles. For Data Engineers, Python is the most sought-after skill,
  appearing in 68% of job postings.
- Data Engineers require more specialized technical skills(AWS, Azure, Spark) compared
  to Data Analysts and Data Scientists who are expected to b proficient in more general data management
  and analysis tools(Excel, Tableau).

## How are in-demand skills trending for Data Analysts?

### Visualize Data

```python
df_plot = df_DA_US_percent.iloc[:, :5]

sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.despine()
plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Postings')
plt.xlabel('2023')
plt.legend().remove()


from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(11.3, df_plot.iloc[-1, i], df_plot.columns[i])



plt.tight_layout()
```

![Trending Skills for Data Analysts](images\top_trends_skills.png)
_Line graph visualizing the top trending skills for data analysts in the US in 2023_

### Insights:

- SQL remains the most consistently demanded skill all through the year, although its shows a gradual decrease in demand.
- Excel experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of year.
- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. SAS while less demanded compared to the others, shows a slight upward trend towards the year's end.

## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis

#### Visualize Data

```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

plt.title('Salary Distributions in the United States')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000)
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

#### Results

![Salary Distribution of Data Jobs in the US](images\salary_distributions.png)
_Box Plot Visualizing the salary distributions for the top 6 data job titles._

## 3. How well do jobs and skills pay for Data

#### Highest Paid & Most Demanded Skills for Data Analysts

```python
#Top 10 Highest Paid Skills for Data Analysts

sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, ax=ax[0], hue='median', palette='dark:b_r', legend=False)
ax[0].set_title('Top 10 Highest Paid Skills For Data Analyst')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}k'))

#Top 10 Most In-Demand Skills for Data Analysts

sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, ax=ax[1], hue='median', palette='light:b')
ax[1].set_title('Top 10 Most In-Demand Skills For Data Analyst')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}k'))

ax[1].set_xlim(ax[0].get_xlim())

```

![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](images\salary_analysis.png)
_Two seperate bar graphs visualiizing the highest paid skills and most in-demand skills for data analysts in the US._

#### Insights:

- The top graph shows specialized technical skills `dplyr`, `Bitbucket`, and `Gitlab` are associated with higher salaries, some reaching up to $200k, suggesting that advanced technical proficiency can increase earning potential.

- The bottom graph highlights that programming skills like `SQL`, `R` and `Python` are the most in-demand skills as they even offer the highest salaries.

- There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data Analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded programming skills.

## 4. What is the most optimal skill to learn for Data Analysts?

#### Visualize Data

```python
from adjustText import adjust_text
from matplotlib.ticker import PercentFormatter

df_DA_skills_high_demand.plot(kind='scatter', x='skills_percent', y='median_salary')

#Prepare texts for adjustText

texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skills_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], txt))

#Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

#set axis labels, title, and legend
plt.xlabel('Percent of Data Analysts Jobs')
plt.ylabel('Median Yearly Salary')
plt.title('Most Optimal Skill for Data Analysts in the US')
ax=plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

#adjust layout and display plot
plt.tight_layout()
plt.show()
```

![Most OptimalSkills for Data Analysts in the US](images\optimal_skills.png)
_A scatter plot visualizing the most optimal skills(high paying & high demand) for data analysts in the US._
