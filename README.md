🌟 Customer Behavior Analysis – End-to-End Data Pipeline

An end-to-end retail analytics project to understand customer behavior, shopping patterns, and purchase drivers using:

Python (Pandas)

PostgreSQL (ETL + SQL Analytics)

Power BI (Dashboard)

Mermaid (Pipeline Flowchart)

📌 Dataset

Source: Kaggle – Consumer Behavior and Shopping Habits

Rows: 3,900

Columns: 19

Format: CSV

Description: Contains customer demographics, purchases, ratings, discounts, shipping method, and shopping habits.

🚀 Project Workflow
1. Data Cleaning (Python Pandas)

The dataset was cleaned and transformed using Pandas:

✔ Steps Performed

Imported dataset into Jupyter Notebook

Checked missing values

Cleaned review_rating

Filled missing values using category-wise median

Snake Casing

Converted all column names → lowercase

Replaced spaces with _

Age Segmentation
Categorized age into:

Young

Adult

Middle aged

Senior

Converted frequency_of_purchases to numerical groups

Removed promo_code

Because promo_code_used and discount_applied were identical

Exported cleaned dataset → final_customer_behavior.csv

🗄️ 2. PostgreSQL ETL Pipeline

We used a two-table ETL approach to ensure clean and validated data:

✔ Staging Table: customer_stage (all TEXT)

Purpose: Safe CSV import without errors.

CREATE TABLE public.customer_stage (
    customer_id TEXT,
    age TEXT,
    gender TEXT,
    item_purchased TEXT,
    category TEXT,
    purchase_amount TEXT,
    location TEXT,
    size TEXT,
    color TEXT,
    season TEXT,
    review_rating TEXT,
    subscription_status TEXT,
    shipping_type TEXT,
    discount_applied TEXT,
    previous_purchases TEXT,
    payment_method TEXT,
    frequency_of_purchases TEXT,
    age_group TEXT,
    purchase_frequency_days TEXT
);

✔ Final Table: customer (typed + constraints)

Clean, validated dataset used for analysis.

INSERT INTO public.customer (
    customer_id, age, gender, item_purchased, category,
    purchase_amount, location, size, color, season,
    review_rating, subscription_status, shipping_type,
    discount_applied, previous_purchases, payment_method,
    frequency_of_purchases, age_group, purchase_frequency_days
)
SELECT
    customer_id::BIGINT,
    age::INT,
    gender,
    item_purchased,
    category,
    purchase_amount::BIGINT,
    location,
    size,
    color,
    season,
    review_rating::NUMERIC(3,1),
    subscription_status,
    shipping_type,
    CASE
        WHEN discount_applied ILIKE 'yes' THEN TRUE
        WHEN discount_applied ILIKE 'no'  THEN FALSE
        ELSE NULL
    END,
    previous_purchases::INT,
    payment_method,
    frequency_of_purchases,
    age_group,
    NULLIF(purchase_frequency_days, '')::NUMERIC(10,1)::INT
FROM public.customer_stage;

🧠 3. SQL Analytics (9 Insights)
1️⃣ Total revenue by gender
2️⃣ High-spending discount users
3️⃣ Top 5 products by rating
4️⃣ Avg spend by shipping type
5️⃣ Subscriber vs Non-subscriber spend
6️⃣ Highest discount-used products
7️⃣ Customer segmentation (New/Returning/Loyal)
8️⃣ Subscription likelihood among repeat buyers
9️⃣ Revenue by age group

(Add SQL queries in a folder like /sql/queries.sql for GitHub)

📊 4. Power BI Dashboard

Created measures:

Customer Count = COUNT(customer[customer_id])
Avg Rating = AVERAGE(customer[review_rating])
Avg Purchase = AVERAGE(customer[purchase_amount])


Dashboard Visualizations:

Revenue by Age Group

Revenue by Gender

Discount Usage Analysis

Shipping Type Insights

Top Products by Rating

Customer Segmentation Distribution

Purchase Frequency Analysis

🏗️ Mermaid Flowchart (End-to-End Pipeline)
flowchart TD

A[Kaggle Dataset (CSV)] --> B[Python Pandas Cleaning]
B --> C[Export Cleaned CSV]

C --> D[PostgreSQL - Staging Table (customer_stage)]
D --> E[Data Type Conversion + CAST]
E --> F[Final Table: customer]

F --> G[SQL Analytics (9 Insights)]
G --> H[Power BI Dashboard]

style A fill:#f3f3f3,stroke:#333,stroke-width:1px
style B fill:#ffe8cc,stroke:#d18800
style C fill:#e7f5ff,stroke:#1c7ed6
style D fill:#f8d7da,stroke:#b71c1c
style E fill:#d1e7dd,stroke:#0f5132
style F fill:#dbe4ff,stroke:#364fc7
style G fill:#faf0e6,stroke:#8b4513
style H fill:#e2e3e5,stroke:#333

📁 Project Structure
customer-behavior-analysis/
│
├── data/
│   ├── raw_dataset.csv
│   ├── final_customer_behavior.csv
│
├── sql/
│   ├── staging_table.sql
│   ├── final_table_insert.sql
│   ├── analysis_queries.sql
│
├── notebooks/
│   ├── cleaning.ipynb
│
├── dashboard/
│   ├── powerbi_dashboard.pbix
│
└── README.md

⚙️ How to Run This Project
1. Clone Repository
git clone https://github.com/your-username/customer-behavior-analysis.git
cd customer-behavior-analysis

2. Install Python Dependencies
pip install pandas numpy

3. Run Notebook
jupyter notebook notebooks/cleaning.ipynb

4. Import into PostgreSQL

Use pgAdmin → Import → staging table → run insertion query.

5. Open Power BI

Connect to PostgreSQL

Load customer table

Add measures

Build visuals

🖼️ Screenshots (Add Images Here for GitHub)
✔ Data Cleaning (Jupyter)
✔ PostgreSQL Tables
✔ SQL Query Outputs
✔ Power BI Dashboard
⭐ Conclusion

This project demonstrates a complete data pipeline:

Python-based cleaning

DB-level ETL architecture

Analytical SQL queries

Business insights

Power BI dashboard

A fully functional end-to-end analytics project suitable for portfolio and resume.