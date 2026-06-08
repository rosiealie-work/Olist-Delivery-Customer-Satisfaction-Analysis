# Project Background

Olist is a Brazilian e-commerce marketplace founded in 2015 that helps small and medium-sized sellers reach customers through major online marketplaces, while supporting order management, marketplace operations, and delivery coordination.

As Olist continues to handle a large volume of customer orders, delivery performance becomes a critical driver of customer satisfaction. Late deliveries, long carrier transit times, and missed delivery promises can negatively affect review scores and weaken the overall customer experience.

For this reason, this project analyzes delivery timelines, promised delivery gaps, customer–seller geography, order complexity, product characteristics, and review outcomes to understand how delivery performance affects customer satisfaction and to identify where logistics improvements should be prioritized.

Insights and recommendations are provided on the following key areas:

- **Customer Satisfaction Impact:** Evaluation of how delivery delays, lead time, and missed delivery promises affect customer review scores.

- **Delivery Bottleneck Analysis:** Analysis of the order delivery journey across approval time, seller handling time, and carrier transit time to identify the main source of delivery delays.

- **Order-Level Performance Analysis:** Assessment of how freight, weight, volume, product count, and seller count relate to delivery performance.

- **Regional Customer Pain Analysis:** Evaluation of late-delivery pain by customer state, focusing on late order volume, late rate, and model-adjusted carrier delay.

The Python code for data cleaning, validation, feature engineering, and EDA can be found [here](01_data_processing_eda.ipynb).

The Python code for linear regression benchmarking model can be found [here](02_linear_regression_benchmark.ipynb).

The entire Power BI dashboard file can be downloaded and explored [here](03_olist_powerbi_dashboard.pbit).

## Power BI Dashboard Walkthrough

A short demo of the interactive Power BI dashboard, highlighting the CSAT Impact, Transit Driver, and Customer Pain analysis pages.

https://github.com/user-attachments/assets/d52ec6c1-b8ed-4328-8a60-20529d253e73










# Data Structure & Initial Checks

The companies main database structure as seen below consists of four tables: table1, table2, table3, table4, with a total row count of X records. A description of each table is as follows:
- **Table 2:**
- **Table 3:**
- **Table 4:**
- **Table 5:**

[Entity Relationship Diagram here]



# Executive Summary

### Overview of Findings

Late delivery materially damages customer satisfaction, making delivery performance the key operational driver behind negative customer experience in this project. Specifically, CSAT remains healthy overall across 94.4K delivered orders, with an average review score of 4.15 and a late rate of 8.08%, but once an order misses the promised delivery date, the average review score drops from 4.29 for not-late orders to 2.56 for late orders, creating a 1.73-point CSAT penalty. The time trend reinforces this relationship, as 2018 Q1–Q2 show the weakest CSAT period alongside the highest late-rate levels.

Below is the overview page from Power BI, and the next two dashboard pages further break down the main delivery-performance factors behind late orders and identify which states experience the most severe delivery-related customer pain.

<img width="1415" height="773" alt="Ảnh chụp màn hình 2026-06-06 163748" src="https://github.com/user-attachments/assets/bae2a945-3782-4be6-b422-b9805a61616a" />




# Insights Deep Dive
## Transit Driver Insights

* **Main insight 1.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 2.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 3.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 4.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
* 
<img width="1128" height="620" alt="Ảnh chụp màn hình 2026-06-08 101113" src="https://github.com/user-attachments/assets/d651f116-ddf7-45fd-993a-b267d2c70d5c" />


### Category 2:

* **Main insight 1.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 2.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 3.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 4.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.

[Visualization specific to category 2]


### Category 3:

* **Main insight 1.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 2.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 3.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 4.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.

[Visualization specific to category 3]


### Category 4:

* **Main insight 1.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 2.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 3.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.
  
* **Main insight 4.** More detail about the supporting analysis about this insight, including time frames, quantitative values, and observations about trends.

[Visualization specific to category 4]



# Recommendations:

Based on the insights and findings above, we would recommend the [stakeholder team] to consider the following: 

* Specific observation that is related to a recommended action. **Recommendation or general guidance based on this observation.**
  
* Specific observation that is related to a recommended action. **Recommendation or general guidance based on this observation.**
  
* Specific observation that is related to a recommended action. **Recommendation or general guidance based on this observation.**
  
* Specific observation that is related to a recommended action. **Recommendation or general guidance based on this observation.**
  
* Specific observation that is related to a recommended action. **Recommendation or general guidance based on this observation.**
  


# Assumptions and Caveats:

Throughout the analysis, multiple assumptions were made to manage challenges with the data. These assumptions and caveats are noted below:

* Assumption 1 (ex: missing country records were for customers based in the US, and were re-coded to be US citizens)
  
* Assumption 1 (ex: data for December 2021 was missing - this was imputed using a combination of historical trends and December 2020 data)
  
* Assumption 1 (ex: because 3% of the refund date column contained non-sensical dates, these were excluded from the analysis)
