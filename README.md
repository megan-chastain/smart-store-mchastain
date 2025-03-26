# smart-store-mchastain
BI Python project
# Smart Sales Project Setup

These starter files provide the foundation for your Smart Sales project. To get started, follow these steps on a Windows machine, executing all commands from a VSC terminal within the main project directory.

**Follow these steps in order:**

1.  **Create a GitHub Repository:**
    * Create a new repository on GitHub.

2.  **Clone the Repository:**
    * Clone the repository to a folder on your C-drive using the following command (replace with your repository URL):
       
        git clone <your_github_repo_url>
      

3.  **Create a Virtual Environment:**
    * To isolate your project's dependencies, create a virtual environment named `.venv` by running:
      
        py -m venv .venv
    

4.  **Activate the Virtual Environment:**
    * Activate the virtual environment to use its isolated packages:
     
        .venv\Scripts\activate
     

5.  **Install Required Packages:**
    * Install all necessary libraries listed in the `requirements.txt` file:
        
        py -m pip install --upgrade -r requirements.txt
       

6.  **Optional: Verify Environment Setup:**
    * You can verify that your virtual environment is set up correctly by running:
       
        py -m datafun_venv_checker.venv_checker
       

7.  **Run the Data Preparation Script:**
    * Execute the initial data preparation script to prepare your data:
        
        py scripts/data_prep.py
        

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
* ## Data Cleaning Scripts

## Data Cleaning Scripts

Run scripts to clean your data and perform the following operations:

* **Remove Duplicates:** Eliminate any duplicate entries within the dataset to ensure data integrity.
* **Handle Missing Values:** Address missing data points through imputation, removal, or other appropriate methods.
* **Remove Outliers:** Identify and remove or adjust outlier data points that may skew analysis.
* **Ensure Consistent Formatting:** Standardize data formats across all columns for uniformity and ease of analysis.

To execute the Python script located at `py scripts\file.py`, (enter your own file names) use the following command:

```bash
py scripts\file.py

To temporarily add local imports to your Python script, you can use the following path manipulation:

```python
import sys
import pathlib
from pathlib import Path

PROJECT_ROOT = str(pathlib.Path(__file__).resolve().parent.parent.parent)
sys.path.append(PROJECT_ROOT)

# Now you can import modules from your project's root directory
# Example: from your_module import your_function
To ensure that the project's root directory is included in the Python module search path, use the following code snippet:

```python
import sys
import pathlib

PROJECT_ROOT = pathlib.Path(__file__).resolve().parent.parent.parent

if str(PROJECT_ROOT) not in sys.path:
    sys.path.append(str(PROJECT_ROOT))
    To import the `logger` from the `utils.logger` module, use the following import statement:

```python
from utils.logger import logger  # noqa: E402