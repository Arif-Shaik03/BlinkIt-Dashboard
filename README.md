# BlinkIt-Dashboard

🛒 BlinkIt Sales Analytics Dashboard
"India's Last Minute App" - Data-Driven Insights using Excel | MySQL | Python | Power BI
________________________________________
📘 Project Overview
This project visualizes and analyzes sales data from BlinkIt (formerly Grofers) to help identify patterns in total sales, average sales, item types, outlet ratings, and more. It offers business insights based on outlet type, location, size, and product category using a complete ETL and BI stack.
________________________________________
🧰 Tech Stack
Tool	Purpose
Excel	Data cleaning, merging, and exploratory data analysis
MySQL	Relational storage, structured queries, and joining large datasets
Python	Data preprocessing, transformation scripts, automation (ETL pipeline)
Power BI	Interactive dashboard, visualization, drill-down analysis
________________________________________
📊 Key Dashboard Metrics
•	Total Sales: $1.20M
•	Average Sales: $141
•	Number of Items: 8,523
•	Average Rating: 3.9
•	Sales Breakdown:
o	By Item Type (e.g., Dairy, Fruits, Household)
o	By Fat Content (Low Fat, Regular)
o	By Outlet Tier (Tier 1/2/3)
o	By Outlet Type (Supermarket Type1/2/3, Grocery Store)
•	Outlet Establishment Trend (2010–2022)
•	Item Visibility and Sales Correlation
________________________________________
🗃️ Data Pipeline Overview
mermaid
CopyEdit
graph TD
    A[Excel Files (Raw CSV)] --> B[Python Scripts (Cleaning & Normalizing)]
    B --> C[MySQL Database]
    C --> D[Power BI Dashboard]
________________________________________
🧩 Entity Relationship Design (MySQL)
sql
CopyEdit
-- Table: outlets
CREATE TABLE outlets (
    outlet_id INT PRIMARY KEY,
    outlet_type VARCHAR(50),
    outlet_size VARCHAR(20),
    outlet_location_type VARCHAR(20),
    establishment_year INT
);

-- Table: items
CREATE TABLE items (
    item_id INT PRIMARY KEY,
    item_type VARCHAR(50),
    fat_content VARCHAR(20),
    item_visibility FLOAT
);

-- Table: sales
CREATE TABLE sales (
    sale_id INT PRIMARY KEY,
    item_id INT,
    outlet_id INT,
    item_mrp FLOAT,
    item_sales FLOAT,
    item_rating FLOAT,
    FOREIGN KEY (item_id) REFERENCES items(item_id),
    FOREIGN KEY (outlet_id) REFERENCES outlets(outlet_id)
);
________________________________________
🐍 Python Script Example (clean_data.py)
python
CopyEdit
import pandas as pd
import mysql.connector

# Load raw data
sales = pd.read_csv('SalesData.csv')
items = pd.read_csv('ItemData.csv')
outlets = pd.read_csv('OutletData.csv')

# Clean fat content
items['fat_content'] = items['fat_content'].str.lower().replace({
    'low fat': 'Low Fat', 'reg': 'Regular', 'LF': 'Low Fat'
})

# Merge datasets
merged = sales.merge(items, on='item_id').merge(outlets, on='outlet_id')

# Export to MySQL
conn = mysql.connector.connect(host='localhost', user='root', password='yourpass', database='blinkit')
merged.to_sql(name='sales_full', con=conn, if_exists='replace', index=False)
________________________________________
📊 Power BI Dashboard Highlights
•	Filter Panel for dynamic analysis:
o	Outlet Location Type
o	Outlet Size
o	Item Type
•	Visuals Used:
o	KPI Cards (Total Sales, Avg Sales, Avg Rating, No. of Items)
o	Line Chart (Outlet Establishment Trend)
o	Bar & Donut Charts (Fat Content, Sales by Item Type)
o	Matrix Table (Outlet Type Breakdown)
o	Drill-down by Tier and Item Category
________________________________________
📂 Excel Contributions
•	Handled raw data transformation (null removal, renaming columns, category standardization)
•	Created pivot tables to identify outliers before ingesting into Python/MySQL
•	Data validation and conditional formatting for early data profiling
________________________________________
💡 Insights Derived
•	Tier 3 Outlets contribute highest to total sales (~$472K)
•	Supermarket Type1 has the highest number of items (5,577) but not the highest average sales
•	Low Fat products dominate sales in all tiers
•	Outlet establishment peaked in 2018, declining slightly post-2020
•	Sales not always proportional to item visibility
________________________________________
🛠️ Setup Instructions
1.	Clone the repository and extract the dataset into /data
2.	Run clean_data.py to normalize and load data into MySQL
3.	Open the Power BI .pbix file and update MySQL connection string
4.	Publish dashboard or share Power BI report with stakeholders
________________________________________
📈 Sample MySQL Query
sql
CopyEdit
SELECT 
    ot.outlet_type, 
    COUNT(*) AS num_items,
    AVG(s.item_sales) AS avg_sales, 
    AVG(s.item_rating) AS avg_rating
FROM sales s
JOIN outlets ot ON s.outlet_id = ot.outlet_id
GROUP BY ot.outlet_type
ORDER BY avg_sales DESC;
________________________________________
🚀 Future Enhancements
•	Add forecasting for future sales using Prophet or ARIMA
•	Create mobile-responsive dashboard using Power BI Service
•	Integrate live API data for real-time updates
•	Use streamlit or flask for web dashboard frontend (Python)
________________________________________
📄 License
This project is licensed under the MIT License.
________________________________________
🙌 Acknowledgements
•	BlinkIt for concept inspiration
•	Power BI for dashboarding
•	MySQL for relational database support
•	Pandas for data preprocessing
