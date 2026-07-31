# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I am targetting.

View my notebook with detailed steps here:
[2_Skill_Count.ipynb](3_Project\2_Skills_Count.ipynb)

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style="ticks")
for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc["job_title_short"] == job_title].head(5)
    sns.barplot(data=df_plot, x="skill_percent", y="job_skills", ax=ax[i], hue="skill_count", palette="dark:b_r")
    ax[i].set_title(job_title)
    ax[i].set_xlabel(" ")
    ax[i].set_ylabel(" ")
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 70)

    for n, v in enumerate(df_plot["skill_percent"]):
        ax[i].text(v + 1, n, f"{v:.0f}%", va="center")

    if i != len(job_titles) - 1:
        ax[i].set_xticks([])
fig.suptitle("Likelihood of Skills Requested in Dutch (NL) Job Postings", fontsize=15)
fig.tight_layout(h_pad=0.5)
plt.show()
```
### Results

![Visualization of Top Skills for Data Roles](3_Project\images\skill_demand_all_data_roles.png)

### Insights

- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (64%)
- SQL is the most requested skill for Data Analysts (50%)

## 2. How are in-demand skills trending for Data Analysts?

### Visualize Data
```python
sns.lineplot(data=df_plot, dashes=False, palette="tab10")
sns.set_theme(style="ticks")
sns.despine()
plt.title("Trending Top Skills for Data Analysts in the Netherlands")
plt.ylabel("Likelihood in Job Posting")
plt.xlabel("2023")
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

from adjustText import adjust_text
texts = []
for i in range(5):
    texts.append(
        plt.text(
            11.2,                       
            df_plot.iloc[-1, i],        
            df_plot.columns[i]
        )
    )

adjust_text(
    texts,
    arrowprops=dict(
        arrowstyle="-",
        color="gray",
        lw=0.8
    ),
    expand_points=(2, 2),
    expand_text=(2, 2),
    force_text=0.8,
    force_points=0.8
)

plt.show()
```

### Results

![Trending Top Skills for Data Analysts in the Netherlands](3_Project\images\trending_skills.png)
*Line graph vizualizing the trending top skills for data analysts in 2023.*

### Insights

The analysis of Data Analyst job postings in the Netherlands reveals several key trends in the most requested skills throughout 2023:

- **SQL remains the most in-demand skill**: SQL consistently appears as the leading requirement, with around 50–60% of job postings mentioning this skill. This highlights the importance of database querying and data extraction capabilities for Data Analyst roles.

- **Python shows strong and stable demand**: Python is the second most requested skill, appearing in approximately 35–45% of job postings. Its popularity reflects the growing need for data automation, analysis, and advanced analytics.

- **Business Intelligence tools are increasingly relevant**: Power BI shows a steady presence in job postings, indicating the importance of data visualization and dashboarding skills for translating data into business insights.

- **Excel remains an essential skill**: Despite the growth of programming and BI tools, Excel continues to be frequently requested, demonstrating its ongoing relevance in reporting, analysis, and business operations.

- **The demand for technical skills fluctuates throughout the year**: While SQL and Python maintain a consistently high level of demand, other skills show more variation across months. This suggests that companies have changing priorities depending on hiring periods and business needs.

Overall, the findings indicate that a strong Data Analyst profile in the Netherlands combines **SQL, Python, BI visualization tools (such as Power BI), and Excel skills**. Developing a balanced skill set across these areas can improve competitiveness in the Dutch data analytics job market.