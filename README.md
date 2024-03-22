# Power-Outage-Random-Forest-Regressor

by Krish Prasad (krprasad@ucsd.edu)

---

## Introduction


The dataset at hand is a comprehensive collection of data relating to major power outages across various U.S. states from the year 2000 to 2016. It spans 1525 entries, each documenting a unique power outage event with detailed attributes such as the year, month, state affected, NERC (North American Electric Reliability Corporation) region, climate information, the cause of the outage, its duration, impact on customers, and various economic factors.
Major outages, as defined by the Department of Energy, are those impacting at least 50,000 customers or causing an unplanned firm load loss of at least 300 MW. This rich dataset is composed of 55 variables, encapsulating not only the outage events themselves but also an array of associated regional characteristics, such as climatic conditions, land use, electricity consumption patterns, and economic factors of the states affected by these outages.

Compiled using publicly available datasets from reputable sources like the DOE's Office of Electricity Delivery and Energy Reliability, the U.S. Energy Information Administration (EIA), the National Oceanic and Atmospheric Administration (NOAA), the U.S. Census Bureau, and the U.S. Department of Labor's Bureau of Labor Statistics, the dataset is a comprehensive resource for understanding the multifaceted nature of power outages.

Our focused research question is: "How have characteristics of major power outages changed over time? Is there a clear trend?" To address this, we will analyze historical trends and patterns, assess risk predictors, and understand the vulnerabilities and resilience of power systems.

Significant columns that pertain to our central research question "How have characteristics of major power outages changed over time? Is there a clear trend?" include:

**YEAR and MONTH**: Essential for identifying time-based trends in outage occurrences.

**U.S._STATE** and POSTAL.CODE: Geographic identifiers to examine state-wise impacts and patterns.

**NERC.REGION and CLIMATE.REGION**: Regional classifications for analyzing outages in the context of regulatory entities and climatic zones.

**OUTAGE.START.DATE and OUTAGE.RESTORATION.DATE**: Markers for the duration and recovery from outages.

**CAUSE.CATEGORY and CAUSE.CATEGORY.DETAIL:**
Categorizations that could reveal the most frequent and impactful causes of outages.

**OUTAGE.DURATION, DEMAND.LOSS.MW, and CUSTOMERS.AFFECTED:** Quantitative measures of each outage's severity and reach.

**CLIMATE.CATEGORY and ANOMALY.LEVEL**: Indicators of climatic conditions during the outage, which might correlate with severe weather events.

**RES.PRICE, COM.PRICE, IND.PRICE, and TOTAL.PRICE**: Data points to correlate economic factors with outages.

**RES.SALES, COM.SALES, IND.SALES, TOTAL.SALES:**
Consumption patterns that might influence or be influenced by outages.

**POPULATION and related demographic variables:** Insight into the population dynamics at the time of the outages.


These columns, among others, will be leveraged to answer our central question. Understanding how the characteristics of power outages have changed over time is critical for multiple stakeholders. Policymakers and power companies can better prepare for future events by identifying trends and patterns in the causes and effects of outages. Researchers and the general public also stand to gain valuable insights into the resilience of the power grid and how climate, technological advancements, and urban development may be influencing outage characteristics.

Our analysis aims to not only chart the historical trends but also to forecast future vulnerabilities, guiding investments in infrastructure resilience and disaster response planning. This dataset has direct implications for the reliability of their electricity supply and the robustness of the national power grid against an array of threats and natural challenges. ​​

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>
---

## Data Cleaning and Exploratory Data Analysis


**Data Clearning**

An examination of missingness across the dataset reveals significant variability. A notable observation is that the HURRICANE.NAMES column has a very high proportion of missing values at 95.31%, suggesting hurricanes are not a common cause of power outages in the dataset. The DEMAND.LOSS.MW and CUSTOMERS.AFFECTED columns have moderate missingness, 45.96% and 28.88% respectively, indicating potential gaps in crucial impact data.

For columns with low to moderate missingness, strategies such as mean or median imputation for numerical columns, and mode imputation or predictive modeling for categorical columns are considered. High missingness columns like HURRICANE.NAMES may be dropped if deemed non-essential, or filled with domain-specific knowledge if crucial. For key columns like DEMAND.LOSS.MW and CUSTOMERS.AFFECTED, more complex imputation methods such as regression or machine learning could be employed, given their importance in assessing the outage impact.

Temporal columns such as OUTAGE.START and OUTAGE.RESTORATION demand specific strategies like interpolation, reflecting the necessity for precise time-related data in outage analysis. Complete case analysis might be an option for certain studies where the integrity of each data point is critical, and the missingness is not systematic.

In the cleanup process, the dataset undergoes preprocessing where redundant or extraneous rows and columns are removed, such as the HURRICANE.NAMES column due to its high missingness. The original datetime columns are consolidated into OUTAGE.START and OUTAGE.RESTORATION using appropriate datetime formats for analysis. Subsequently, the cleaned dataset is saved as 'outage.csv' for further use.

Below is head to the cleaned dataset with the most important columns shown

<iframe src="assets/cleaned_dataset_head.png" width=800 height=600 frameBorder=0></iframe>


---

**Univariate Analysis**

To understand the data's distribution, univariate plots are created for OUTAGE.DURATION using histograms, and for CUSTOMERS.AFFECTED using box plots. These visualizations reveal the spread and central tendencies of these key variables. Annotations such as mean and median are added to the box plot for CUSTOMERS.AFFECTED to provide quick statistical insights.

**Bivariate Analysis**

Bivariate analyses involve creating scatter plots to explore relationships between variables like OUTAGE.DURATION and CUSTOMERS.AFFECTED, as well as between OUTAGE.DURATION and DEMAND.LOSS.MW. These plots are instrumental in identifying trends and correlations between the severity of an outage and its impact on customers and demand loss.

To examine the effect of climate on power outages, the average OUTAGE.DURATION is plotted against CLIMATE.CATEGORY, offering insights into how different climate conditions may influence outage lengths. Additionally, scatter plots with trendlines are employed to dissect the interaction between outage duration, customers affected, and climate categories.

**Interesting Aggregates**

Finally, group and pivot table analyses are conducted to summarize average OUTAGE.DURATION by CLIMATE.CATEGORY, total CUSTOMERS.AFFECTED by NERC.REGION, and to create a comprehensive pivot table that cross-references the CLIMATE.CATEGORY and U.S._STATE with average OUTAGE.DURATION and total CUSTOMERS.AFFECTED. This multidimensional view is essential to contextualize the outages within various regional and climatic parameters.

The missingness analysis component of the code is pivotal for understanding data completeness and integrity, directing subsequent data cleaning, imputation, and analysis efforts, and ensuring the reliability of conclusions drawn from this significant dataset on power outages.







---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing


---
