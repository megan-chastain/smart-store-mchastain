# smart-store-mchastain: BI Python Project

This document outlines the setup and development process for the Smart Sales project, a Business Intelligence (BI) Python project.

## Smart Sales Project Setup

These starter files provide the foundation for your Smart Sales project. To get started, follow these steps on a Windows machine, executing all commands from a VS Code terminal within the main project directory.

**Project Initialization:**

1.  **Create a GitHub Repository:**
    * Create a new repository on GitHub to manage your project's version control.

2.  **Clone the Repository:**
    * Clone the repository to a folder on your C-drive using the following command (replace `<your_github_repo_url>` with your actual repository URL):

        ```bash
        git clone <your_github_repo_url>
        ```

3.  **Create a Virtual Environment:**
    * To isolate your project's dependencies, create a virtual environment named `.venv` by running:

        ```bash
        py -m venv .venv
        ```

4.  **Activate the Virtual Environment:**
    * Activate the virtual environment to use its isolated packages:

        ```bash
        .venv\Scripts\activate
        ```

5.  **Install Required Packages:**
    * Install all necessary libraries listed in the `requirements.txt` file:

        ```bash
        py -m pip install --upgrade -r requirements.txt
        ```

6.  **Optional: Verify Environment Setup:**
    * You can verify that your virtual environment is set up correctly by running:

        ```bash
        py -m datafun_venv_checker.venv_checker
        ```

7.  **Run the Data Preparation Script:**
    * Execute the initial data preparation script to prepare your data:

        ```bash
        py scripts/data_prep.py
        ```

## Initial Software Libraries

The project relies on the following Python packages:

* `pip`: The package installer for Python.
* `loguru`: For enhanced logging.
* `ipykernel`: Enables Python kernels for Jupyter.
* `jupyterlab`: The JupyterLab interactive development environment.
* `numpy`: For numerical computing.
* `pandas`: For data manipulation and analysis.
* `matplotlib`: For basic plotting.
* `seaborn`: For statistical data visualization.
* `plotly`: For interactive visualizations.
* `pyspark==4.0.0.dev1`: Apache Spark for large-scale data processing (development version).
* `pyspark[sql]`: Spark SQL module.

## Folder Structure Setup

1.  **Add Folders:**
    * Add the following folders to the root project folder:
        * `Data`
        * `Scripts`
        * `utils`

2.  **Data Folder Setup:**
    * Inside the `Data` folder, create a folder named `Raw`.
    * Inside the `Raw` folder, add the following files:
        * `sales_data`
        * `customer_data`
        * `product_data`
    * Add the appropriate raw data to each file.

3.  **Utils Folder Setup:**
    * In the `utils` folder, add a file named `logger.py`.

4.  **Scripts Folder Setup:**
    * In the `Scripts` folder, add a file named `data_prep.py`.

## Version Control

* Continuously use `git commit` to save and log all changes and events. This ensures that your project's history is well-documented and that you can easily revert to previous versions if needed.

## Data Cleaning Process

This section details the data cleaning process using Python's `pandas` library.

### Data Cleaning Scripts

Run scripts to clean your data and perform the following operations:

* **Remove Duplicates:** Eliminate any duplicate entries within the dataset.
* **Handle Missing Values:** Address missing data points through imputation or removal.
* **Remove Outliers:** Identify and adjust outlier data points.
* **Ensure Consistent Formatting:** Standardize data formats for uniformity.

To execute a Python script (e.g., `file.py`) within the `scripts` folder, use:

```bash
py scripts\file.py

# Creating a Data Warehouse (SQLite)

This section describes how to create a SQLite database to serve as a simple data warehouse using a Python script.

## Steps

1.  **Create `create_dw.py`:**
    * Create a file named `create_dw.py` inside the `scripts` folder of your project.

2.  **Add Python Code:**
    * Add the following Python code to `create_dw.py`. This script uses the `sqlite3` library to create tables within an SQLite database.

    ```python
    import sqlite3

    def create_tables(db_path):
        """Creates tables in the SQLite database."""

        try:
            conn = sqlite3.connect(db_path)
            cursor = conn.cursor()

            # Example: Create a 'sales' table
            cursor.execute('''
                CREATE TABLE IF NOT EXISTS sales (
                    sale_id INTEGER PRIMARY KEY,
                    product_id INTEGER,
                    customer_id INTEGER,
                    sale_date TEXT,
                    quantity INTEGER,
                    price REAL
                )
            ''')

            # Example: Create a 'products' table
            cursor.execute('''
                CREATE TABLE IF NOT EXISTS products (
                    product_id INTEGER PRIMARY KEY,
                    product_name TEXT,
                    category TEXT
                )
            ''')

            #Example: Create a customers table
            cursor.execute('''
                CREATE TABLE IF NOT EXISTS customers (
                    customer_id INTEGER PRIMARY KEY,
                    customer_name TEXT,
                    city TEXT
                )
            ''')

            conn.commit()
            print(f"Tables created successfully in {db_path}")

        except sqlite3.Error as e:
            print(f"An error occurred: {e}")

        finally:
            if conn:
                conn.close()

    if __name__ == "__main__":
        db_path = "smart_store_dw.db"  # Database file name
        create_tables(db_path)

    ```

3.  **Run the Script:**
    * Open your terminal or command prompt.
    * Navigate to your project's root directory.
    * Execute the script using the following command:

        ```bash
        py scripts/create_dw.py
        ```

    * This will create a file named `smart_store_dw.db
    * db script must be in perfect alignment with your data or you will get errors.

    # Populating Star Schema Tables with SQL

This section outlines the steps to create and run an SQL script that populates tables in a star schema within your SQLite data warehouse. We will assume you have a `smart_store_dw.db` database created (as per previous instructions) and that you have "prepared" data available (though the exact format and location are placeholders here - you'll need to adjust based on your actual prepared data).

**1. Create the SQL Script (`populate_dw.sql` in `scripts` folder):**

Create a new file named `populate_dw.sql` inside your `scripts` folder. This script will contain the `INSERT INTO` statements to populate your fact and dimension tables.

```sql
-- scripts/populate_dw.sql

-- Populate the DimCustomers table
INSERT INTO DimCustomers (CustomerID, CustomerName, City)
SELECT DISTINCT
    customer_id,
    customer_name,
FROM prepared_customers_data; -- Replace with your actual prepared data source/table

-- Populate the DimProducts table
INSERT INTO DimProducts (ProductID, ProductName, Category)
SELECT DISTINCT
    product_id,
    product_name,
    category
FROM prepared_products_data; -- Replace with your actual prepared data source/table

FROM prepared_sales_data; -- Replace with your actual prepared data source/table

-- Populate the FactSales table
INSERT INTO FactSales (Sale, Product, Customer)
    s.sale_id AS Sale,
    p.product_id AS Product,
    c.customer_id 
FROM prepared_sales_data s -- Replace with your actual prepared sales data source/table
JOIN prepared_products_data p ON s.product_id = p.product_id -- Assuming a join is needed
JOIN prepared_customers_data c ON s.customer_id = c.customer_id; -- Assuming a join is needed

-- You might need to adjust the joins and column names based on your prepared data structure.
-- Ensure the column names in these SELECT statements EXACTLY match the column names
-- in your prepared data sources.
-- Also, ensure the target table column names in the INSERT INTO statements
-- EXACTLY match the column names defined in your data warehouse schema
-- (e.g., in your create_dw.py script).
Initial set up of PowerBI and SQLite
    
1.  Open **ODBC Data Sources (64-bit)** from the Start Menu.
2.  Click the **System DSN** tab.
3.  Click **Add...** → choose **SQLite3 ODBC Driver** (or whatever data store you are using) → click **Finish**.
4.  Name it **SmartSalesDSN** (or any name you prefer).
5.  Click **Browse** and select your database file: e.g., `smart_sales.db` (or whatever you named it).
6.  Click **OK** to save.

To link datawarehouse to PowerBI:

1.  Open **Power BI Desktop**.
2.  Click **Get Data** (top left) → Select **ODBC** from the list.
3.  Choose the DSN you created in Task 1 (e.g., **SmartSalesDSN**).
4.  Click **OK**. Wait a moment. Power BI will show a list of available tables.
5.  Select the tables you want to analyze - for most of us:
    * **Customer table**
    * **Product table**
    * **Sales table**
6.  Click **Load** to bring the tables into Power BI.
7.  Switch to **Model view** (left panel) to see how the tables are connected.

To perform a query:

1.  Open **Power BI Desktop**.
2.  In the **Home** tab, click **Transform Data** to open **Power Query Editor**.
3.  In Power Query, click **Advanced Editor** (top menu).
4.  **Option 1 (Using SQL - Requires Data Source Connection):**
    Delete any code in the editor and replace it with your SQL query (example below). You **must** have already established a connection to your data source (e.g., via ODBC) for this to work. You will need to use your actual table names and column names for the SQL to function correctly with your data source.

    ```sql
    SELECT c.name, SUM(s.amount) AS total_spent
    FROM sale s
    JOIN customer c ON s.customer_id = c.customer_id
    GROUP BY c.name
    ORDER BY total_spent DESC;
    ```

    **Option 2 (Using M-code - More Power BI Native):**
    Delete any code in the editor and replace it with the following M-code. **Note:** This code assumes you have an ODBC data source named "smart_sales_DSN" with tables named "customer" and "sale" containing the specified columns. Adjust the data source name and table/column names to match your setup.

    ```m
    let
        Source = Odbc.DataSource("dsn=smart_sales_DSN", [HierarchicalNavigation=true]),
        customer_Table = Source{[Name="customer",Kind="Table"]}[Data],
        JoinedSalesAndCustomers = Table.Join(
            customer_Table,
            {"customer_id"},
            Source{[Name="sale",Kind="Table"]}[Data],
            {"customer_id"},
            JoinKind.Inner
        ),
        GroupedCustomers = Table.Group(
            JoinedSalesAndCustomers,
            {"name"},
            {{"Total Spent", each List.Sum([sale_amount_usd]), type number}}
        ),
        SortedCustomers = Table.Sort(GroupedCustomers, {{"Total Spent", Order.Descending}})
    in
        SortedCustomers
    ```

5.  Click **Done**.
6.  Rename the new query (on the left) to something like **Top Customers** or whatever you are focusing on.
7.  Click **Close & Apply** (upper left) to return to the report view.
8.  You can now use this table in visuals (e.g., bar chart).

To slice, dice, and drilldown:


**Windows (Power BI) - Slice, Dice, and Drilldown**

**Slicing:** Add a date range slicer
**Dicing:** Create a matrix visual for sales by product & region
**Drilldown:** Enable drill-through to explore sales by year → quarter → month

Slicing

Since SQLite doesn’t have real "Date" fields, use Power BI's Transform Data to extract parts of the date for slicing, dicing, and drilldown.

1.  Click **Transform Data** to open Power Query.
2.  Select the **sales** table.
3.  Select the **order_date** column (or any "date" field).
4.  On the top menu, click **Add Column** → **Date** → **Year**.
5.  Then click **Add Column** → **Date** → **Quarter**.
6.  Then click **Add Column** → **Date** → **Month** → **Name of Month**.
7.  Click **Close & Apply** to save changes and return to the report view.
8.  In Power BI, go to the **Report view** (center icon on the left).
9.  From the **Visualizations** pane, click on the **Slicer** icon.
10. Drag a **date field** into the slicer.
11. If it doesn’t show a range, click the dropdown (upper-right corner of slicer) and select **Between** to enable a date range slider.

Dicing 

To explore sales by product attributes (e.g. category and region or other characteristics), we’ll create a Matrix visual in Power BI.

1.  Go to the **Report view**.
2.  From the **Visualizations** pane, click the **Matrix** visual.
3.  Drag your first product feature (e.g., **category**) to the **Rows** field well.
4.  Drag your second product feature (e.g., **region**) to the **Columns** field well.
5.  Drag a **numeric field** to the **Values** field well.
6.  *Optional:* Format the numeric values by clicking the column dropdown in the **Values** area.

This matrix will help us “dice” the data and break it down by two categorical dimensions: product and region.

Drilldown

To explore sales over time, we’ll use a column or line chart and enable drilldown so we can click into sales by year, quarter, and month.

1.  Go to the **Report view**.
2.  From the **Visualizations** pane, click on either the **Clustered Column Chart** or **Line Chart**.
3.  Drag your **Year**, **Quarter**, and **Month** fields (created earlier from **order_date**) into the **X-Axis** or **Axis** field in that order:
    * First: **order\_year**
    * Then: **order\_quarter**
    * Then: **order\_month**
4.  Drag your **numeric value** (e.g., **total amount**) into the **Values** area.
5.  At the top left of the chart, click the **drilldown arrow icon** (a split-down arrow).
6.  Click on a bar or line point in the chart to drill from **Year** → **Quarter** → **Month**.
7.  To move back up, click the **up arrow** near the same spot.
8.  If nothing happens when clicking, make sure the chart supports hierarchy and the drilldown mode is active (look for the split arrow).
