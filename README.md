# Overview

Welcome to my analysis of the job data market, focusing on data analysists roles. This project was created out of desire to navigate and to undersatnd the job market more effectively. It delves into the top-paying and in-demand skills to help find the optimal job opportunities for data analysts.

# The Questions
 Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in demand skills trending over the year for Data Analsyts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn?(High Demand and Hight Paying)

# Tools I Used

For my deepdive into the data analyst job market, I used the several key tools:

 Python: The backbone of the analysis, allowing me to analyze and find critical insights. I used the following Python Libraries:

* **Pandas Library**: This was used to analyze the data.
* **Matplotlib Library**: Used to visualize the data.
* **Seaborn Library**: Helped in creating more advanced visuals.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data





# The Analysis


## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles, I filtered out all the roles by the top 3 most popular roles, and got the top 5 skills for this top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targetting.

View my notebook with detailed steps here:
[2_Skills_Demand.ipynb](Python_Project\2_Skills_Demand.ipynb)

### Visualize Data

```python

fig,ax =plt.subplots(len(job_titles),1)

for i , job_title in enumerate(job_titles):
     df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
     
     sns.barplot(data= df_plot, x = 'skill_perc',
                 y = 'job_skills', ax=ax[i],hue = 'skill_perc', palette = 'dark:b_r')
     sns.set_theme(style = 'ticks')
    
     ax[i].set_title(job_title)
     ax[i].set_ylabel('')
     ax[i].set_xlabel('')
     ax[i].get_legend().remove()
     ax[i].set_xlim(0,78)
  
     for n, v in enumerate (df_plot['skill_perc']):
        ax[i].text(v + 1, n, f'{ v:.0f}%', va = 'center')

     if i!=len(job_titles) - 1:  
          ax[i].set_xticks([])
     
     fig.tight_layout(h_pad=0.5)
     fig.suptitle('Likelihood of skills Requested in US Job Postings',fontsize = 15)

     plt.show()
```




### Results

![Visualization of the Top 3 Data Roles and their corresponding Top 5 skills](Python_Project\images\skill_demand_top_3_data_roles.png)

### Insights

- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers(65%). 
- Sql is the most requested skill for Data Analysts and Data Scientist with it in over half the job postings for both roles. For Data Engineers, Python is the most sought - after skill,appearing in 65% of the job postings
- Data Engineers require more specialized technical skills(AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Sql,Tableau)
## How are in-demand skills trending for Data Analysts?

### Visualize Data

```python
df_plot = df_DA_USA_percent.iloc[:, :5]

sns.lineplot(data = df_plot, dashes = False, palette ='tab10')
sns.set_theme(style = 'ticks')
sns.despine()
plt.title('Trending Top Skills for Data Analysts in the US')
plt.xlabel('2023')
plt.ylim(0,75)
plt.ylabel('Likelihood in Job Posting')
plt.legend()
plt.tight_layout()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals = 0))

plt.show()

```
### Results




![Visualization of the skils trends over the year](Python_Project\images\top_5_trending_skills.png)


### Insights

- Sql remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand
- Excel experienced a significant increase in demand starting around Novemeber, surpassing both python and tableau by the end of the year
- Python experienced a signifacant increase around Septemeber,surpassing tableau but still remians low compared to tableau
- SAS is consistent throughout the year although there are some small increase in April and decrease around May 

## How well do jobs and skills pay Data Analysts?

### Salary Analysis for Data Roles

### Visualize Data
 ``` python
sns.boxplot(data = df_USA_top6 , x = 'salary_year_avg', y = 'job_title_short' , order = job_order)

plt.title('Salary Distribution ')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000)
ticks_x = plt.FuncFormatter( lambda x,_: f'${int (x/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

### Results

![Salary Analysis for data roles](Python_Project\images\salary_distribution_box_plot.png)

### Insights

- There is a significant Variation in salary changes across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with upto 600k, indicating high value placed on advanced data skills and experience in the industry.

- Senior Data Engineer and Senior Data scientist roles show a considerable amount of outliers on the higher end of the salary spectrum, suggesting exceptional skillsor circumstances can lead to high pay in these roles, In contrast Data analyst roles demonstarte more consistency in salary with fewer outliers.

- The median salary increase with seniority and specialization of the roles, (Senior Data Scientist, Senior Data Engineer)not only have higher median salaries but also larger differences in typical salaries, reflectin greater variance in compensation as responsibilities increase.

### Highest Paid and Most in Demand Skills for Data Analysts

Visualize Data

```python
fig , ax = plt.subplots(2,1)

sns.set_theme(style = 'ticks')

# top 10 highest paid skills for data analysist

sns.barplot( data = df_DA_top_pay, x = 'median', y = df_DA_top_pay.index, ax =ax[0], hue = 'median', palette = 'dark:b_r')
ax[0].legend().remove()
# df_DA_top_pay.plot(kind = 'barh', y = 'median', ax = ax[0], legend = False)

ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].set_title('Top 10 Highest paid skills for Data Analysts') 
ax[0].xaxis.set_major_formatter(plt.FuncFormatter( lambda x,_: f'${int (x/1000)}K'))

sns.barplot( data = df_DA_top_skills, x = 'median', y = df_DA_top_skills.index, ax =ax[1], hue = 'median',palette = 'light:b' )
ax[1].legend().remove()

# df_DA_top_skills.plot(kind = 'barh', y = 'median', ax = ax[1],legend = False)

ax[1].set_xlim(ax[0].get_xlim())
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary')
ax[1].set_title('Top 10 Most in demand skills by salary for Data Analysts') 
ax[1].xaxis.set_major_formatter(plt.FuncFormatter( lambda x,_: f'${int (x/1000)}K'))

fig.tight_layout()
```
### Results

![Top paying and most in demand skills for data analysts](Python_Project\images\most_in_demand_skills_and_highpayingskills.png)

### Insights:

- The top graph shows specialized technical skills like `dplyr`, `Bitbucket`, and `Gitlab` are associated with higher salaries, some reaching upto $200k, suggesting that advanced technical proficiency can increase earning potential.

- The bottom graph highlights that foundational skills like `Excel`, `Powerpoint` and `Sql` are most in demand, even though they may not offer the highest salaries. This demonstrates the importance of core skills for employability in data analysis roles.

- There's a clear distinction betweenthe skills that are the highest paid and those are most in demand. Data Analysts aiming to maximize their career potential should consider developing a diverse skill that includes both high paying specialized skills and widely demannded foundational skills.

## What is the most Optimal Skill to learn for Data Analysts?

### Visualize Data
```python
sns.scatterplot(data= df_plot, x='skill_percent', y= 'median_salary', hue='technology')
sns.despine()
sns.set_theme(style = 'ticks')

plt.show()

```
### Results

![Visualization of the Top 3 Data Roles and their corresponding Top 5 skills](Python_Project\images\optimal_skills_scatter_plot.png)


### Insights: 
- The scatter plotshows that most of the cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefitswithin data analytics field.

- Analyst tools (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysissoftware are crucialfor currentdat roles. This category not only has good salareis but is also versatile across different types of data tasks.

- The database skills(color orange), such as Oracle and Sql Server, are associated with some of the highest salaries among data analyst tools.
This indicates a significant demand and valuation for data managemnet and manipulation expertise in the industry.

# What I Learned

Throughout the project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

* Advanced Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
* Data Cleaning Importance: I learned that thorough data cleaning nad preapration are crucial before any analysis can be done, ensuring the accuarcy of insights derived from the data.
* Stategic Skill Analysis: The project emphasized the importance of aligning one's skills with the market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights

This Project provided several general insights into data job market for analysts:
* **Skill Demand and Salary Correlation**: Theres a clear correlation between the demand of specific skills and the salaries of these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
* **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of data job market. Keeping up with these trends is essential for career growth in data analytics.
* **Economic Value of Skills**: Understanding which skills are both in demand and well compensated can guide data analysts in prioritizing learning to maximize their economic returns.

# Challenges I Faced

This project was not without its challanges, but it provide good learning opportunities

* Data Inconsistencies: Handling missing or inconsistent requires careful cnsideration and thorough data preparation and cleaning techniques to ensure the integrity of the analysis.
* Complex Data Visualization: Designing effective visual representations of complex datasets was challenging   conveying insights clearly and compelingly
* Balancing Breadth and Depth: Deciding how deeply to dive into each analysis while mantaining a broad       landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion
This exploration into the data analyst job market has been informative, highlighting the critical skills and trends that shape this evolving field. The insights I got  provide actionable guidance for anyone looking to advance their career in data analytics. As the market continue to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future exploration and underscores the importance of continous learning and adaptation in the data field.