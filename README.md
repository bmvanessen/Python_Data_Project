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


## 3. How well do jobs and skills pay for Data Analysts?

### Salary Analysis for Data roles

#### Visualize Data

```python
sns.boxplot(data=df_NL_top6, x="salary_year_avg", y="job_title_short", order=job_order)

plt.title("Salary Distribution in the Netherlands")
plt.xlabel("Yearly Salary ($USD)")
plt.ylabel(" ")
ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}K"))
plt.xlim(0, 600000)
plt.show()
```

#### Results
![Salary Distribution in the Netherlands](3_Project\images\salary_distribution_skills.png)
*Boxplot vizualizing the salary distribution for the top 6 most popular data roles in the Netherlands.*

#### Insights

- **Machine Learning Engineers earn the most, and have the widest range** — salaries span roughly $80K–$220K, with a median around $150K, reflecting high variance based on seniority and specialization.
- **Data Scientists follow closely**, with a tighter, higher-median range (~$110K–$170K), suggesting more standardized compensation across companies.
- **Senior Data Engineer** shows a surprisingly narrow interquartile range clustered near $140K, with two outliers (~$80K and ~$170K) — likely a small sample size skewing the distribution.
- **Data Engineers** have the tightest box overall (~$90K–$110K), but several outliers above $150K hint at senior or specialized roles pulling salaries higher.
- **Data Analysts and Business Analysts** sit at the lower end of the spectrum (~$70K–$120K), consistent with these being more entry-to-mid-level roles.
- **Outliers appear in nearly every category except Business Analyst and Machine Learning Engineer**, suggesting niche high-paying positions (e.g., leadership or specialized technical roles) exist even within "standard" job titles.

### Highest Paid & Most Demanded Skills for Data Analysts

#### Visualize Data

```python
fig, ax = plt.subplots(2, 1)

sns.set_theme(style="ticks")

#df_DA_top_pay.plot(kind="barh", y="median", ax=ax[0], legend=False)
sns.barplot(data=df_DA_top_pay, x="median", y=df_DA_top_pay.index, ax=ax[0], hue="median", palette="dark:b_r")
#ax[0].invert_yaxis()
ax[0].legend().remove()
ax[0].set_title("Top 10 Highest Paid Skills for Data Analysts")
ax[0].set_ylabel(" ")
ax[0].set_xlabel(" ")
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}K"))

#df_DA_skills.plot(kind="barh", y="median", ax=ax[1], legend=False)
sns.barplot(data=df_DA_skills, x="median", y=df_DA_skills.index, ax=ax[1], hue="median", palette="light:b")
#ax[1].invert_yaxis()
ax[1].legend().remove()
ax[1].set_title("Top 10 Most In-Demand Skills for Data Analysts")
ax[1].set_ylabel(" ")
ax[1].set_xlabel("Median Salary (USD)")
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f"${int(x/1000)}K"))

plt.tight_layout()
plt.show()
```

#### Results
![The Highest Paid & Most Demanded Skills for Data Analysts in the Netherlands](3_Project\images\Highest_paid_most_demand_skills.png)
*Barplot vizualizing the highest paid and most demanded skills for data analysts in the Netherlands.*

#### Insights

**Cloud and database skills command the highest premiums** — AWS, Azure, and NoSQL top the pay chart, all around $175K, notably higher than the rest of the field (highlighted in black to signal their outlier status).
- **Hadoop and Spark follow as the next tier**, at roughly $120K–$145K, reflecting continued (if declining) value for big-data infrastructure skills.
- **Most "highest paid" skills cluster tightly around $110K** — Airflow, VBA, PySpark, Looker, and Go all sit in a narrow band, suggesting limited salary differentiation once you're past the top outliers.
- **There's a clear disconnect between what pays well and what's in demand.** PySpark, Go, and Looker appear on *both* lists — a strong signal these are high-value, low-competition skills worth prioritizing.
- **The most in-demand skills are NOT the highest paid.** Excel, SQL, Python, and Power BI dominate job postings but sit at the bottom of the demand chart's salary scale (~$90K), reflecting their status as baseline/expected skills rather than differentiators.
- **R sits in an interesting middle ground** — moderately in-demand (~$100K median) but absent from the top-paid list, suggesting steady but not premium demand.

## 4. What is the most optimal skill to learn for Data Analysts?

### Visualize Data

```python
from adjustText import adjust_text


#df_DA_NL_skills_high_demand.plot(kind="scatter", x="skill_percent", y="median_salary")
sns.scatterplot(
    data=df_plot,
    x="skill_percent",
    y="median_salary",
    hue="technology",
)

sns.despine()
sns.set_theme(style="ticks")
texts = []

for i, txt in enumerate(df_DA_NL_skills_high_demand.index):
    texts.append(plt.text(df_DA_NL_skills_high_demand["skill_percent"].iloc[i], df_DA_NL_skills_high_demand["median_salary"].iloc[i], txt))

adjust_text(texts, arrowprops=dict(arrowstyle="->", color="gray", lw=1))

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.xlabel("Percent of Data Analyst Jobs")
plt.ylabel("Median Yearly Salary")
plt.title("Most Optimal Skills for Data Analysts in the Netherlands")
plt.tight_layout()
plt.show()
```

### Results
![Most Optimal Skills for Data Analysts in the Netherlands](3_Project\images\scatterplot_optimal_skills.png)

#### Insights

- **SQL is the clear "must-have" skill** — appearing in ~58% of job postings (by far the highest demand) while still commanding a strong ~$91K median salary, making it the single most valuable skill to prioritize.
- **Python offers the best pay-to-demand ratio among programming skills** — the highest salary on the chart (~$98K) combined with solid demand (~33%), positioning it as a premium, high-leverage skill.
- **Tableau is the standout analyst tool** — strong demand (~31%) paired with a competitive salary (~$93K), outperforming other tools in its category.
- **Oracle and SQL Server pay well despite low demand** (~7% each), suggesting database skills are niche but lucrative — likely tied to specialized or legacy-system roles.
- **Excel and Word/PowerPoint sit in the "low value" quadrant** — moderate-to-high demand (especially Excel at ~42%) but the lowest salaries on the chart (~$81K–$85K), reinforcing that these are baseline expectations rather than differentiators.
- **Go and R are outliers worth noting** — low demand (~5–20%) but respectable pay (~$90K–$93K), possibly reflecting specialized technical roles within data analytics.
- **Clustering by category**: programming languages (blue) generally command higher salaries than analyst tools (orange), while cloud/database skills (red/green) are split between high-pay/low-demand niches.