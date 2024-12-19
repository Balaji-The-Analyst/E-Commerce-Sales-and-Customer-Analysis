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
# Example: Review Score Distribution
import matplotlib.pyplot as plt
import seaborn as sns

sns.countplot(x='review_score', data=reviews_data, palette='viridis')
plt.title('Distribution of Review Scores')
plt.xlabel('Review Score')
plt.ylabel('Count')
plt.show()
```

![Review Score Distribution](https://github.com/user-attachments/assets/5f49f04a-ef79-42d6-a5e5-31aea131fb70)


- **Bivariate Analysis:**
  - Investigated the relationship between **review scores and prices**, finding that higher-priced products tend to receive better ratings, possibly reflecting higher quality or expectations.
  - Explored the effect of **order status on delivery times**, showing that "canceled" orders often have longer processing times before cancellation, while "delivered" orders average **7 days** for completion.

```python
# Example: Relationship between review scores and price
sns.boxplot(x='review_score', y='price', data=final_data)
plt.title('Review Score vs Product Price')
plt.xlabel('Review Score')
plt.ylabel('Price')
plt.show()
```

![Relationship between review scores and price](https://github.com/user-attachments/assets/a8fa65b6-4bce-4ecd-b410-85826222e7e6)


### 3. **Customer Lifetime Value (CLV)**
- Calculated total revenue, order frequency, and average order value for each customer.
- Visualized top customers using bar charts and treemaps.

```python
# Example: CLV Treemap.
import squarify
sizes = top_customers['total_revenue']
labels = top_customers['customer_id']

plt.figure(figsize=(12, 6))
squarify.plot(sizes=sizes, label=labels, alpha=0.8, color=sns.color_palette("viridis", len(sizes)))
plt.title('Customer Revenue Distribution (Top 10)', fontsize=14)
plt.axis('off')
plt.show()
```

![Customer Revenue Treemap](https://github.com/user-attachments/assets/1b37e96d-a1b8-43cc-8035-2b1dba0ec1d8)

- Segmented customers into tiers: High, Medium, and Low CLV.

### 4. **Sales Trends Analysis**
- Analyzed monthly revenue trends, revealing a **30% revenue increase during November and December**, indicating peak holiday season demand.
- Identified top-performing product categories (e.g., electronics and furniture) contributing **60% of total revenue**, while categories like toys and apparel underperformed.

```python
# Example: Monthly Revenue Trends
monthly_revenue = orders_data.groupby(orders_data['order_purchase_timestamp'].dt.to_period('M'))['revenue'].sum()
plt.figure(figsize=(12, 6))
monthly_revenue.plot(kind='line', marker='o', color='blue')
plt.title('Monthly Revenue Trends')
plt.xlabel('Month')
plt.ylabel('Revenue')
plt.grid()
plt.show()
```

![Monthly Revenue Trends](https://github.com/user-attachments/assets/54bc0c8c-759e-4d64-8e82-9d91a4339fcd)

```python
# Example: Identifying Underperforming Categories
plt.figure(figsize=(12, 6))
sns.scatterplot(data=category_performance, x='total_quantity', y='total_revenue', color='blue', alpha=0.6)
sns.scatterplot(data=low_performance, x='total_quantity', y='total_revenue', color='red', label='Underperforming')
plt.title('Revenue vs Quantity Sold (Product Categories)', fontsize=14)
plt.xlabel('Total Quantity Sold', fontsize=12)
plt.ylabel('Total Revenue', fontsize=12)
plt.legend()
plt.grid(axis='both', linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()
```

![Scatter plot of Underperforming Categories](https://github.com/user-attachments/assets/a907824c-dde3-42f5-b63d-87b52e43cd66)

### 5. **Delivery Performance Analysis**
- Measured actual delivery times and delays.
- Assessed on-time delivery rates by product categories, finding that **bulky items like furniture had a 25% delay rate**, while smaller products performed better.
- Visualized delivery performance trends using box plots and bar charts.
  
```python
# Example: Distribution of Actual Delivery Times
plt.figure(figsize=(8, 6))
sns.boxplot(data=final_data, x='actual_delivery_time', color='skyblue')
plt.title('Distribution of Actual Delivery Times', fontsize=14)
plt.xlabel('Delivery Time (Days)', fontsize=12)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.show()
```

![Distribution of Actual Delivery Times](https://github.com/user-attachments/assets/f0eca072-4fbc-4b67-a634-793f58c181e1)
  

### 6. **Customer Segmentation**
- Conducted RFM (Recency, Frequency, Monetary) analysis.
- Segmented customers into groups such as:
  - Champions
  - Loyal Customers
  - Big Spenders
- Visualized segment distributions and revenue contributions.

```python
# Example: Distribution of customer segments
plt.figure(figsize=(10, 6))
rfm['Customer_Segment'].value_counts().plot(kind='bar', color='skyblue')
plt.title('Customer Segmentation', fontsize=14)
plt.xlabel('Segment', fontsize=12)
plt.ylabel('Number of Customers', fontsize=12)
plt.xticks(rotation=45)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()

```

![Customer Segments](https://github.com/user-attachments/assets/ac56ed04-9e39-4961-9660-ee148d3b756d)

### 7. **Profitability and Cost Analysis**
- Calculated profit margins for products and categories, identifying **high-margin products contributing 40% of profits**.
- Summarized overall revenue, costs, and profit.
- Visualized top-performing products and categories.

```python
# Example: category profitability
plt.figure(figsize=(12, 6))
sns.barplot(x='total_profit', y='product_category_name_english', data=category_profitability, palette='coolwarm')
plt.title('Profitability by Product Category', fontsize=14)
plt.xlabel('Total Profit', fontsize=12)
plt.ylabel('Product Category', fontsize=12)
plt.grid(axis='x', linestyle='--', alpha=0.7)
plt.show()
```

![Product category profitability](https://github.com/user-attachments/assets/3a00b004-8881-477f-a5d5-7ff41f49f528)

---

## Insights and Recommendations
1. **Customer Insights:**
   - High CLV customers (top 10% by revenue) contribute approximately **45% of total revenue** and should be prioritized for loyalty programs.
   - Recent customers makeup **20% of total customers**, suggesting an opportunity for re-engagement campaigns to convert them into repeat buyers.

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
   git clone <[repository-url](https://github.com/Balaji-The-Analyst/E-Commerce-Sales-and-Customer-Analysis/blob/main/README.md)>
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
- **Dataset:** [Olist E-commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

For more detailed information about this project, please refer to the Python file or the PDF version available in the repository.
