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