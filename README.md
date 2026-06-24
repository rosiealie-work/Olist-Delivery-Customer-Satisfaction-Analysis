# Project Background

Olist is a Brazilian e-commerce marketplace founded in 2015 that helps small and medium-sized sellers reach customers through major online marketplaces, while supporting order management, marketplace operations, and delivery coordination.

In marketplace operations, delivery performance is a critical driver of customer satisfaction. Late deliveries, long carrier transit times, and missed delivery promises can negatively affect review scores and weaken the overall customer experience.

For this reason, this project analyzes delivery timelines, promised delivery gaps, customer–seller geography, order complexity, product characteristics, and review outcomes to understand how delivery performance affects customer satisfaction and to identify where logistics improvements should be prioritized.

Insights and recommendations are provided on the following key areas:

- **Customer Satisfaction Impact:** Evaluation of how delivery delays, lead time, and missed delivery promises affect customer review scores.

- **Delivery Stage & Carrier Transit Bottleneck Analysis:** Analysis of the order delivery journey across approval time, seller handling time, and carrier transit time to identify which stage is most strongly associated with late deliveries.
  
- **Regional Customer Pain Analysis:** Evaluation of late-delivery pain by customer state, focusing on late order volume, late rate, model-adjusted carrier delay residual, and delay burden.

The Python code for data cleaning, validation, feature engineering, and EDA can be found **[here](01_data_processing_eda.ipynb).**

The Python code for linear regression benchmarking model can be found **[here](02_linear_regression_benchmark.ipynb).**

The entire Power BI dashboard file can be downloaded and explored **[here](03_olist_powerbi_dashboard.pbit).**

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

Across **94.4K** delivered orders with valid timestamps and review scores, Olist maintains a healthy average review score of **4.15**. However, **customer satisfaction drops sharply when orders miss the promised delivery date**. Late orders receive an average review score of **2.56**, compared with **4.29** for not-late orders, creating a **1.73-point** CSAT penalty. The main delivery-stage signal behind late orders is **carrier transit**, where the slowest orders show a much larger late-rate lift than seller handling or order approval. After adjusting carrier transit time for distance, package characteristics, timing, and seller state, the highest-priority customer pain is concentrated in specific states such as **Alagoas, Bahia, and Rio de Janeiro**.

Below is the overview page from Power BI. The next two dashboard pages further break down the main delivery-performance factors behind late orders and identify which states experience the most severe delivery-related customer pain.

<img width="1415" height="773" alt="Ảnh chụp màn hình 2026-06-06 163748" src="https://github.com/user-attachments/assets/bae2a945-3782-4be6-b422-b9805a61616a" />




# Insights Deep Dive
## Transit Driver Insights

* **Among the three delivery stages, carrier transit is the strongest unadjusted delivery-stage signal associated with late orders.** Orders in the slowest carrier-transit quartile have a **24.0 percentage-point** higher late rate than those in the fastest quartile, far above seller handling (**9.4 pp**) and order approval (**2.7 pp**). This suggests that reducing long carrier transit times should be the first operational priority for lowering late deliveries, but not necessarily treated as the sole root cause here. It will be further validated through model-adjusted analysis to separate carrier underperformance from distance, region, order complexity, and upstream seller delays.
  
* **Late orders show a much larger median delay during carrier transit than in earlier delivery stages.** The median carrier-transit gap between late and not-late orders is **16.94 days**, compared with only **1.24 days** for seller handling and a minimal gap for order approval. This suggests that the strongest observable delay pattern occurs after seller handoff, making carrier transit the key operational stage to investigate further.
  
* **Late-delivery risk also escalates sharply once carrier transit becomes prolonged.** Across shorter transit buckets, late rate remains relatively low and stable, but it rises steeply for orders taking more than 19 days in transit. This indicates that long in-transit orders should be flagged earlier as high-risk cases, especially before they breach the promised delivery date and negatively affect customer satisfaction.
  
<img width="1438" height="786" alt="Ảnh chụp màn hình 2026-06-08 214549" src="https://github.com/user-attachments/assets/11e8bdd3-3987-4312-885c-d499356eb495" />



## Customer Pain Insights

**How to read this dashboard:** A linear regression benchmark is used to estimate the expected carrier transit time for each order. The **model-adjusted residual** is calculated as **Actual carrier transit days − Expected carrier transit days**, where a positive median residual means deliveries in that customer state take longer than expected after adjustment. To prioritize action, **Delay Burden = Late Orders × Positive Median Residual**, and the Priority Type is assigned using Power BI rule-based segmentation that combines order volume, late-order volume, residual severity, and delay burden.

* **The highest number of late orders does not automatically mean the worst carrier issue.** **São Paulo** has the largest number of late orders (**2,251**), but its median residual is negative (**-0.7 days**). This means that although São Paulo contributes the most late orders by volume, its carrier transit time is not worse than expected after model adjustment. For this reason, São Paulo is classified as a **High-Volume Watch** market rather than a carrier delay hotspot. The issue here is mainly scale-driven, so the business action should focus on capacity monitoring and SLA control, not necessarily carrier underperformance investigation.

* **Carrier Delay Hotspots are states with both excess transit time and meaningful delay burden.** The dashboard identifies **2 Carrier Delay Hotspots**: **Alagoas** and **Bahia**. These states combine positive model-adjusted residuals with enough late-order burden to require deeper operational review. Alagoas has a high median residual of **+5.7 days** and a late rate of **23.4%**, while Bahia has a lower residual (**+1.8 days**) but much larger order volume and delay burden. This suggests that hotspot detection should consider both severity and scale, not residual alone.
  
* **Rio de Janeiro has the largest delay burden, making it a major operational priority.** Rio de Janeiro has the highest delay burden (**1,558**) with **1,558 late orders** and a positive median residual of **+1.0 day**. Although its residual is not high enough to be classified as a Carrier Delay Hotspot, the combination of high late volume and positive excess transit time makes it a **Volume-Driven Delay Risk**. This indicates that Rio de Janeiro should be prioritized for operational improvement because even a moderate excess transit delay can create a large customer impact when applied to many late orders.

* **High residual alone is not enough when sample size and late-order volume are low.** Amazonas has the highest carrier issue residual (**+7.6 days**), but it has only **138 total orders** and **5 late orders**, resulting in a much smaller delay burden. Because the issue is severe but limited in scale, it is classified as **Moderate Priority** rather than a top hotspot. This prevents the business from overreacting to small-sample states and keeps the prioritization focused on regions where excess transit time creates meaningful customer pain.

<img width="1438" height="785" alt="Ảnh chụp màn hình 2026-06-08 214314" src="https://github.com/user-attachments/assets/48b11677-08e0-4c75-b44b-043214bd7ffa" />





# Recommendations:

Based on the insights and findings above, we would recommend the Logistics, Operations, and Customer Experience teams to consider the following: 

* **Build an early-warning workflow for orders at risk of missing the promised delivery date.** Since late delivery is strongly associated with lower customer satisfaction, Olist should flag high-risk orders before they breach the estimated delivery date. Priority should be given to orders with unusually long carrier transit time, orders approaching the promised date, and orders shipped through historically delayed state routes. Once flagged, these orders can be escalated for carrier follow-up, proactive customer communication, or service recovery actions.
  
* **Create a carrier transit performance scorecard for ongoing operational monitoring.** Carrier transit is the strongest delivery-stage signal in this analysis, so it should be monitored separately from order approval and seller handling. The scorecard should track late rate, median carrier transit days, p75/p90 transit time, model-adjusted residual, and delay burden by customer state, seller state, and delivery route. This would help the operations team identify whether delays are broad-based, route-specific, or concentrated in a few high-risk regions.
  
* **Prioritize root-cause investigation in Carrier Delay Hotspot states.** Alagoas and Bahia should be reviewed first because they show both excess carrier transit time and meaningful delay burden after model adjustment. The investigation should focus on route coverage, carrier SLA compliance, handoff quality after seller dispatch, regional infrastructure constraints, and whether alternative carrier allocation or different delivery promises are needed for these states.
  
* **Apply a volume-driven improvement playbook to states with high delay burden.** States such as Rio de Janeiro should receive strong operational attention because even moderate excess carrier delay can create large customer impact when applied to many late orders. The priority should be to reduce delay at scale through carrier capacity planning, SLA enforcement, route-level monitoring, fulfillment cut-off review, and targeted escalation for orders approaching the promised delivery date.
  
* **Manage High-Volume Watch states through monitoring rather than carrier underperformance investigation.** States such as São Paulo or Minas Gerais generate a large number of late orders mainly because of order volume, but their model-adjusted residual does not indicate worse-than-expected carrier transit performance. These markets should be managed through peak-period monitoring, promise-date control, capacity buffers, and SLA governance, rather than immediate deep-dive investigations into carrier failure.


# Assumptions and Caveats:

Throughout the analysis, several assumptions were made to keep the dataset methodologically consistent and the business interpretation reliable. These assumptions and caveats are noted below:

  
* **Assumption 1: The final analytical sample includes only delivered orders with valid timestamps and available review scores.**

  This project focuses on completed delivery outcomes, so only delivered orders are included in the final analysis. Orders with missing review scores are excluded because customer satisfaction is measured using the final order-level review score. Orders with invalid timestamp logic are also removed because negative or impossible delivery-stage durations would make lead time, seller handling time, carrier transit time, and promise gap unreliable. As a result, the dashboard should be interpreted as an analysis of completed, reviewable, timestamp-valid delivered orders, not all orders in the raw Olist dataset.

* **Assumption 2: The order is the main unit of analysis.**

  Delivery timestamps in the Olist dataset describe the overall delivery journey of an order, not separate item-level shipments. Therefore, item, product, seller, price, freight, weight, and volume information is aggregated to one row per order. These features describe the overall size, value, and operational complexity of the order, but they should not be interpreted as proof that a specific product, seller, or item caused a delay.

* **Assumption 3: The latest review is treated as the final order-level customer satisfaction outcome.**

  Some orders have multiple review records, but the review dataset does not reliably map each review to a specific item-level delivery event. Since delivery performance is measured at the order level, this project keeps only the most recent review for each order_id based on review_answer_timestamp. The latest review_score is used as the final customer satisfaction measure for the complete order. This keeps the review data aligned with the order-level delivery grain, but it means earlier customer feedback for the same order is not separately analyzed.

* **Assumption 4: Late delivery is defined by missed delivery promise, not simply long delivery duration.**

  The late_flag is defined as promise_gap_day > 0, where promise_gap_day = actual delivered customer date - estimated delivery date. This means an order is considered late only when it arrives after the promised delivery date. Long lead time alone is not classified as late if the order still arrives within the estimated delivery promise. This definition is appropriate for customer satisfaction analysis because customers are more likely to react to missed promises than to delivery duration in isolation. 

* **Assumption 5: Geographic distance is an approximate proxy for delivery complexity.**

  The regression model uses estimated seller-to-customer distance based on available geolocation data and the Haversine formula. This distance is useful for adjusting expected carrier transit time, but it is not the same as actual road distance, delivery route distance, number of logistics hubs, or real carrier network complexity. Therefore, distance-adjusted residuals should be interpreted as an improved benchmark, not a perfect representation of the true logistics path.

* **Assumption 6: The linear regression model is used for benchmarking, not causal inference or production-grade prediction.**

  The model estimates expected carrier transit days based on distance, package characteristics, order timing, and seller state. The purpose is to create a fair benchmark for identifying states where carrier transit is slower than expected. The residuals should not be interpreted as causal proof that the carrier alone caused the delay. The model helps compare performance after adjustment, but unobserved factors such as carrier identity, route capacity, weather, holidays, warehouse constraints, or local infrastructure may still affect delivery time.

* **Assumption 7: The model-adjusted Customer Pain analysis is based on homogeneous orders.**

  The regression benchmark focuses on homogeneous orders, which contain one seller and one product type. This reduces aggregation bias because the delivery outcome is directly linked to one seller-product-carrier flow. Complex orders are excluded from the modeling layer because the dataset does not reveal which seller, product, or shipment component caused the delay. Therefore, the Customer Pain residual analysis is best interpreted as a cleaner benchmark for simpler order structures, not as a complete explanation of every complex order.
  
* **Assumption 8: Outliers are retained because extreme delivery cases may represent real operational pain.**

  Extreme carrier transit times, long distances, heavy packages, and large-volume orders are not automatically removed or capped. They may represent valid operational cases that the business should understand. However, these extreme cases can create a long-tailed residual distribution and make the model less precise for very difficult or unusually delayed orders. For this reason, the dashboard emphasizes median residuals and delay burden rather than relying only on mean-based metrics.

* **Assumption 9: The selected OLS log-target model is optimized for residual benchmarking, not for minimizing every prediction error metric.**

  The final model uses an OLS regression fitted on log1p(carrier_transit_time_day) and converts predictions back to day scale. No smearing correction is applied because the dashboard focuses on median residual benchmarking rather than mean-level prediction. This choice is appropriate for identifying states where a typical order is slower than expected, but it may understate or overstate expected transit time for some individual orders.

* **Assumption 10: Late-rate lift is a diagnostic signal, not a causal claim.**

  The lift chart compares the late-rate gap between the slowest 25% and fastest 25% of orders within each delivery stage. Carrier Transit shows the largest unadjusted lift, making it the strongest stage-level warning signal for late-delivery risk. However, this does not prove that carrier transit is the sole root cause. Long transit time may also be driven by route distance, regional complexity, order characteristics, promise-date design, or upstream seller delays. For this reason, the lift chart is used to prioritize deeper investigation, while the Customer Pain analysis uses model-adjusted residuals to separate expected long transit from abnormal carrier delay.

* **Assumption 11: Priority Type is a rule-based segmentation framework.**

  The Customer Pain priority labels are created using business rules that combine order volume, late-order volume, late rate, model-adjusted residual severity, and delay burden. These rules are designed to prevent overreacting to small-sample states or ranking states only by raw late-order volume. However, the thresholds should be treated as analytical decision rules, not universal business constants. In a real business setting, they should be reviewed with operations stakeholders and refreshed as order volume, SLA targets, and carrier performance change.

* **Assumption 12: State-level results may hide more granular route, city, carrier, or seller patterns.**

  The Customer Pain dashboard aggregates results by customer state to support strategic prioritization. However, delivery problems may be concentrated in specific cities, seller-state to customer-state lanes, carriers, product categories, or time periods. A state classified as high priority should therefore be treated as a starting point for deeper operational analysis, not the final root-cause diagnosis.

* **Assumption 13: The analysis is based on historical Olist data and should not be interpreted as current Olist performance.**

  The project uses a historical public dataset for portfolio and analytical demonstration purposes. The findings are valid for the available dataset and the assumptions used in this analysis, but they should not be interpreted as a statement about Olist's current logistics operations, current carrier network, or present-day customer experience.
