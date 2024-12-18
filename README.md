# E-Commerce Sales & Customer Analysis

## Overview
This project involves the analysis of the Olist e-commerce dataset, which contains detailed transactional, customer, product, and review data. The goal is to perform an end-to-end analysis, simulating a real-world data analytics project, and provide actionable insights for improving business performance.

## Dataset
The dataset includes the following:
1. **Orders:** Information on purchase timestamps, delivery dates, and statuses (99,441 rows, 8 columns).
2. **Customers:** Demographics and geographic data of customers (99,441 rows, 5 columns).
3. **Products:** Product details including categories, dimensions, and weight (32,329 rows, 9 columns).
4. **Sellers:** Seller locations and IDs (3,095 rows, 4 columns).
5. **Payments:** Payment types and amounts for orders (103,886 rows, 5 columns).
6. **Reviews:** Customer reviews, ratings, and comments (99,441 rows, 7 columns).
7. **Geolocation:** Latitude and longitude of delivery locations (100,016 rows, 5 columns).
8. **Order Items:** Details of each item in an order (112,650 rows, 7 columns).

---

## Objective
To derive insights and recommendations for improving customer satisfaction, operational efficiency, and overall profitability. The analysis focuses on:
- Customer Lifetime Value (CLV)
- Sales trends
- Product performance
- Delivery performance
- Customer segmentation

---

## Project Steps

### 1. **Data Cleaning and Preprocessing**
- **Handled Missing Values:**
  - For delivery-related columns, missing values were filled based on order status. For instance, missing `order_delivered_customer_date` values for `delivered` orders were estimated using the `order_estimated_delivery_date`.
  - Reviews with missing comments were imputed as "No Comment" to retain valuable review scores.
- **Outlier Treatment:**
  - Identified and capped extreme outliers in freight value using the 95th percentile to avoid skewing delivery cost analysis.
- **Transformed Data:**
  - Converted timestamp columns like `order_purchase_timestamp` to datetime format for accurate time-based calculations.
  - Created derived metrics such as delivery time (in days) and delay duration to evaluate logistics performance.

### 2. **Exploratory Data Analysis (EDA)**
- **Univariate Analysis:**
  - Analyzed the distribution of review scores, showing that **57% of reviews are 5 stars**, while **11% are 1 star**, indicating a polarization in customer satisfaction.
  - Examined sales distribution, revealing that the majority of products are priced between $20 and $50, with some outliers in the high-end range.

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Example: Review Score Distribution
sns.countplot(x='review_score', data=reviews_data, palette='viridis')
plt.title('Distribution of Review Scores')
plt.xlabel('Review Score')
plt.ylabel('Count')
plt.show()
```

![Review Score Distribution](./visuals/review_score_distribution.png)

- **Bivariate Analysis:**
  - Investigated the relationship between **review scores and prices**, finding that higher-priced products tend to receive better ratings, possibly reflecting higher quality or expectations.
  - Explored the effect of **order status on delivery times**, showing that "canceled" orders often have longer processing times before cancellation, while "delivered" orders average **7 days** for completion.

```python
# Example: Delivery Time by Order Status
sns.boxplot(x='order_status', y='delivery_time', data=orders_data, palette='coolwarm')
plt.title('Delivery Time by Order Status')
plt.xlabel('Order Status')
plt.ylabel('Delivery Time (Days)')
plt.show()
```

![Delivery Time by Order Status](./visuals/delivery_time_by_status.png)

### 3. **Customer Lifetime Value (CLV)**
- Calculated total revenue, order frequency, and average order value for each customer.
- Visualized top customers using bar charts and treemaps.

```python
# Example: CLV Treemap
import squarify

# Mock Data Example for Treemap
clv_data = {'Customer': ['A', 'B', 'C'], 'Revenue': [3000, 2000, 1500]}
fig, ax = plt.subplots(figsize=(12, 8))
squarify.plot(sizes=clv_data['Revenue'], label=clv_data['Customer'], alpha=0.8, color=sns.color_palette('viridis'))
plt.title('Customer Revenue Contribution')
plt.axis('off')
plt.show()
```

![Customer Revenue Treemap](./visuals/customer_revenue_treemap.png)

- Segmented customers into tiers: High, Medium, and Low CLV.

### 4. **Sales Trends Analysis**
- Analyzed monthly revenue trends.
- Identified top-performing product categories by sales and quantity sold.
- Highlighted underperforming categories for improvement.

### 5. **Delivery Performance Analysis**
- Measured actual delivery times and delays.
- Assessed on-time delivery rates by product categories.
- Visualized delivery performance trends using box plots and bar charts.

### 6. **Customer Segmentation**
- Conducted RFM (Recency, Frequency, Monetary) analysis.
- Segmented customers into groups such as:
  - Champions
  - Loyal Customers
  - Big Spenders
- Visualized segment distributions and revenue contributions.

### 7. **Profitability and Cost Analysis**
- Calculated profit margins for products and categories.
- Summarized overall revenue, costs, and profit.
- Visualized top-performing products and categories.

---

## Insights and Recommendations
1. **Customer Insights:**
   - High CLV customers (top 10% by revenue) contribute approximately **45% of total revenue** and should be prioritized for loyalty programs.
   - Recent customers make up **20% of total customers**, suggesting an opportunity for re-engagement campaigns to convert them into repeat buyers.

2. **Sales Trends:**
   - Revenue peaks during the holiday season (November to December), accounting for **30% of annual revenue**, indicating the need for inventory readiness and promotional campaigns during this period.
   - Top product categories contribute **60% of total revenue**, while underperforming categories generate less than **5%** each, highlighting potential areas for improvement or removal.

3. **Delivery Performance:**
   - On-time delivery rates average **85%**, but certain product categories (e.g., bulky items) have delays in **25% of orders**, requiring better logistics coordination.
   - Average delivery time is **7 days**, with delayed orders taking an additional **3 days on average**, impacting customer satisfaction negatively.

4. **Profitability:**
   - High-margin products contribute **40% of total profits**, while low-margin products generate minimal profits, suggesting a focus on scaling profitable products and re-evaluating less profitable ones.

---

## Tools and Libraries
- **Python:** Pandas (v1.3.3), NumPy (v1.21.2), Matplotlib (v3.4.3), Seaborn (v0.11.2), Squarify (v0.4.3)
- **Jupyter Notebook** for development and visualization.

---

## How to Run
1. Clone the repository:
   ```bash
   git clone <repository-url>
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Run the Jupyter Notebook to reproduce the analysis.

---

## Results
The final analysis provides detailed visuals, insights, and recommendations that can be directly used to improve business performance in an e-commerce setting.

---

## Project Structure
```
.
├── data/                     # Raw and processed datasets
├── notebooks/                # Jupyter notebooks for analysis
├── visuals/                  # Generated plots and visuals
├── README.md                 # Project documentation
└── requirements.txt          # Python dependencies
```

---

## Acknowledgments
- **Dataset:** [Olist E-commerce Dataset](https://www.kaggle.com/datasets/olistbr)

Feel free to explore the repository and provide feedback!
