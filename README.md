# 📊 Data Modeling — Salary Prediction of Data Professions

This project demonstrates a complete **data modeling pipeline** using Python and PostgreSQL. It takes a raw HR dataset of data professionals, cleans and restructures it into a normalized relational schema, and loads it into a PostgreSQL database.

---

## 📁 Project Structure

```
Data-Modeling-main/
│
├── Data Modeling.ipynb                        # Main Jupyter Notebook
├── Salary Prediction of Data Professions.csv  # Raw dataset (2,631 records)
├── Book1.xls                                  # Exported: Information table
├── Book2.xls                                  # Exported: Time Period table
├── Book3.xls                                  # Exported: Experience table
├── Book4.xls                                  # Exported: Job Info table
└── Screenshot 2024-08-08 153924.png           # Project screenshot
```

---

## 📌 Dataset

The raw CSV dataset contains **2,631 records** of employees working in data professions, with the following columns:

| Column | Description |
|---|---|
| FIRST NAME / LAST NAME | Employee name |
| SEX | Gender (M/F) |
| DOJ | Date of Joining |
| DESIGNATION | Job title (e.g., Analyst, Senior Analyst) |
| AGE | Employee age |
| SALARY | Annual salary |
| UNIT | Department (Finance, IT, Marketing, etc.) |
| LEAVES USED / REMAINING | Leave management data |
| RATINGS | Performance rating |
| PAST EXP | Years of prior experience |

---

## 🔄 Workflow

### 1. Data Loading & Cleaning
- Loaded the CSV using `pandas`
- Dropped the redundant `CURRENT DATE` column
- Converted `DOJ` to `datetime` format
- Cast string columns (`FIRST NAME`, `LAST NAME`, `SEX`, `DESIGNATION`, `UNIT`) to proper `string` dtype
- Verified zero null values across all columns

### 2. Data Normalization
The cleaned DataFrame was split into **4 sub-tables** to follow a normalized relational model:

| Table | Columns |
|---|---|
| **Information** | FIRST NAME, LAST NAME, AGE, SALARY |
| **Time Period** | LEAVES USED, LEAVES REMAINING, DOJ, SALARY |
| **Experience** | PAST EXP, RATINGS, SALARY |
| **Job** | DESIGNATION, UNIT, SALARY |

> `SALARY` is used as the primary key in the `Information` table and as a foreign key in the other three tables.

### 3. Export to CSV
Each sub-table was saved as a separate CSV file (`Book1.csv` through `Book4.csv`) for further use or inspection.

### 4. PostgreSQL Integration
- Connected to a local PostgreSQL instance using `psycopg2`
- Created a `Jobs` database
- Defined and created the 4 relational tables with appropriate constraints (PRIMARY KEY, FOREIGN KEY, NOT NULL)
- Loaded data from the CSV files into the database tables

---

## 🗄️ Database Schema

```sql
CREATE TABLE Information (
    First_Name  VARCHAR(200) NOT NULL,
    Last_Name   VARCHAR(200) NOT NULL,
    Age         VARCHAR(200) NOT NULL,
    Salary      INT PRIMARY KEY
);

CREATE TABLE timeperiod (
    leaves_used      VARCHAR(50) NOT NULL,
    leaves_remaining VARCHAR(50) NOT NULL,
    DOJ              VARCHAR(50) NOT NULL,
    Salary           INT REFERENCES Information(Salary)
);

CREATE TABLE experience (
    Past_exp  VARCHAR(50),
    Ratings   VARCHAR(50),
    Salary    INT REFERENCES Information(Salary)
);

CREATE TABLE Job (
    Designation VARCHAR(50) NOT NULL,
    Unit        VARCHAR(50) NOT NULL,
    Salary      INT REFERENCES Information(Salary)
);
```

---

## 🛠️ Technologies Used

- **Python 3.11**
- **Jupyter Notebook**
- **pandas** — data loading, cleaning, and transformation
- **psycopg2** — PostgreSQL connection and query execution
- **PostgreSQL** — relational database

---

## ⚙️ How to Run

### Prerequisites
Make sure you have the following installed:
- Python 3.8+
- Jupyter Notebook
- PostgreSQL (running locally)

Install required Python packages:
```bash
pip install pandas psycopg2-binary
```

### Steps
1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/Data-Modeling.git
   cd Data-Modeling
   ```

2. Make sure PostgreSQL is running and update the connection string in the notebook if needed:
   ```python
   conn = psycopg2.connect("host=127.0.0.1 dbname=postgres user=postgres password=your_password")
   ```

3. Open and run the notebook:
   ```bash
   jupyter notebook "Data Modeling.ipynb"
   ```

4. Run all cells in order to clean the data, create the database, and load the tables.

---

## 📝 Notes

- `SALARY` is used as the primary key, which assumes salaries are unique per employee. In a production scenario, a dedicated employee ID would be a better primary key choice.
- The notebook includes error handling for database operations using `try/except` blocks.
