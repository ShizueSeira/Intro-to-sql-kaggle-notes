# 📊 Kaggle Intro to SQL Notes & Video Walkthroughs

Welcome, the repository tracks my journey, code solutions, and vlog-style walkthroughs as I complete the [Kaggle Intro to SQL Course](https://www.kaggle.com/learn/intro-to-sql). 

In these videos, I break down what I learned, walk through BigQuery SQL concepts, and yap about dataset exploration! 🎙️✨

---

## 🚀 Course Progress & Exercises

| Part | Topic | Notebook Link | Video Walkthrough |
| :---: | :--- | :--- | :--- |
| **01** | **Getting Started With SQL & BigQuery** | [📓 View Notebook P1](https://github.com/ShizueSeira/Intro-to-sql-kaggle-notes/blob/main/exercise-getting-started-with-sql-and-bigquery.ipynb) | [📺 Watch Video P1](https://youtu.be/RgFuIKwS2W0) |
| **02** | **Select, From & Where** | [📓 View Notebook P2](https://github.com/ShizueSeira/Intro-to-sql-kaggle-notes/blob/main/exercise-select-from-where.ipynb) | [📺 Watch Video P2](https://youtu.be/sPqRmo2mhpc) |
| **03** | **Group By, Having & Count** | [📓 View Notebook P3](https://github.com/ShizueSeira/Intro-to-sql-kaggle-notes/blob/main/exercise-group-by-having-count.ipynb) | [📺 Watch Video P3](https://youtu.be/Wh7QfRGJe30) |
| **04** | **Order By** | [📓 View Notebook P4](https://github.com/ShizueSeira/Intro-to-sql-kaggle-notes/blob/main/exercise-order-by.ipynb) | [📺 Watch Video P4](https://youtu.be/kf01xj073bg) |
| **05** | **As & With** | [📓 View Notebook P5](https://github.com/ShizueSeira/Intro-to-sql-kaggle-notes/blob/main/exercise-as-with.ipynb) | [📺 Watch Video P5](https://youtu.be/toSMZayMrYE) |
| **06** | **Joining Data** | [📓 View Notebook P6](https://github.com/ShizueSeira/Intro-to-sql-kaggle-notes/blob/main/exercise-joining-data.ipynb) | [📺 Watch Video P6](https://youtu.be/AMtgBqoLA6g) |

---

## 🎥 Featured Video Walkthroughs

### Part 1: Getting Started With SQL and BigQuery (Unlisted, 27 mins.)
[![Part 1 Video Thumbnail](https://img.youtube.com/vi/RgFuIKwS2W0/maxresdefault.jpg)](https://youtu.be/RgFuIKwS2W0)

- **Notes:**
  #### A. What is BigQuery & Core Python Commands
  - **Google BigQuery:** A managed cloud web service and enterprise data warehouse that lets you run standard SQL queries on massive datasets quickly.
  - **Import BigQuery in Python:**
    ```python
    from google.cloud import bigquery
    ```
  - **Creating client object:** Used for retrieving BigQuery datasets.
    ```python
    client = bigquery.Client()
    ```
  - **Construct dataset reference:** Uses the `dataset()` method to specify the project and dataset to retrieve.
    ```python
    dataset_ref = client.dataset("dataset_name", project="project_name")
    ```
  - **Fetch dataset:** Performs an API request using `get_dataset()` to retrieve a collection of tables.
    ```python
    dataset = client.get_dataset(dataset_ref)
    ```
  - **Retrieve list of tables:** Uses `list_tables()` to fetch tables from the specified dataset.
    ```python
    tables = list(client.list_tables(dataset))
    ```
  - **Print table list:** Loop through and output each table ID.
    ```python
    for table in tables:
        print(table.table_id)
    ```
  - **Construct table reference:** Selects a specific table using `.table()`.
    ```python
    table_ref = dataset_ref.table("dataset_name")
    ```
  - **Fetch table:** Fetches the table details via API request.
    ```python
    table = client.get_table(table_ref)
    ```

  #### B. Data Hierarchy
  1. **Client** – Holds projects and connects to the BigQuery service.
  2. **Project** – A collection of datasets.
  3. **Dataset** – A collection of tables.
  4. **Tables** – Composed of rows and columns of related data.

  ![BigQuery Data Hierarchy](https://storage.googleapis.com/kaggle-media/learn/images/biYqbUB.png)

  > *As shown above, requests must follow the hierarchy: start with a Client object, request the Project and Dataset, and finally request the Table.*

  #### C. Table Schema
  - Defines the structure of the table using `table.schema`.
  - Output example:
    ```python
    [SchemaField('title', 'STRING', 'NULLABLE', 'Story title', (), None)]
    ```
  - **Field Breakdown:**
    - `'title'` – Column name.
    - `'STRING'` – Data type.
    - `'NULLABLE'` – Indicates if the column can be left empty (`NULLABLE`) or required.
    - `'Story title'` – Column description.
    - `()` *(5th element)* – Child or nested schema fields for `RECORD` or `STRUCT` data types. An empty tuple `()` means there are no child/tuple fields.
    - `None` *(6th element)* – Policy tags for column-level security. `None` indicates no policy tags are assigned.

  #### D. Previewing Data (`list_rows`)
  - **Preview top rows:**
    ```python
    client.list_rows(table, max_results=5).to_dataframe()
    ```
    - `table`: The target BigQuery table object.
    - `max_results=5`: Limits the number of records returned to 5.
    - `.to_dataframe()`: Converts the BigQuery `RowIterator` object into a pandas `DataFrame`.
  - **Preview specific columns:**
    ```python
    client.list_rows(table, selected_fields=table.schema[:1], max_results=5).to_dataframe()
    ```
    - `selected_fields=table.schema[:1]`: Selects only specific fields (in current case, slicing the first column from the schema).

---

### Part 2: Select, From & Where (Unlisted, 25 mins.)
[![Part 2 Video Thumbnail](https://img.youtube.com/vi/sPqRmo2mhpc/maxresdefault.jpg)](https://youtu.be/sPqRmo2mhpc)

- **Notes:**
  - `SELECT`
  - `FROM`
  - `WHERE`
  - Formatting Notes
  - Limits on Kaggle's regards to working on big datasets
  - *Add notes here...*

---

### Part 3: Group By, Having & Count (Unlisted, 36 mins.)
[![Part 3 Video Thumbnail](https://img.youtube.com/vi/Wh7QfRGJe30/maxresdefault.jpg)](https://youtu.be/Wh7QfRGJe30)

- **Notes:**
  - `GROUP BY`
  - `HAVING`
  - `COUNT()`
  - *Add notes here...*

---

### Part 4: Order By (Unlisted, 44 mins.)
[![Part 4 Video Thumbnail](https://img.youtube.com/vi/kf01xj073bg/maxresdefault.jpg)](https://youtu.be/kf01xj073bg)

- **Notes:**
  - `ORDER BY`
  - Dates
  - `EXTRACT`
  - *Add notes here...*

---

### Part 5: As & With (Unlisted, 66 mins.)
[![Part 5 Video Thumbnail](https://img.youtube.com/vi/toSMZayMrYE/maxresdefault.jpg)](https://youtu.be/toSMZayMrYE)

- **Notes:**
  - `WITH`
  - `AS`
  - Common Table Expressions (CTEs)
  - *Add notes here...*

---

### Part 6: Joining Data (Unlisted, 75 mins.)
[![Part 6 Video Thumbnail](https://img.youtube.com/vi/AMtgBqoLA6g/maxresdefault.jpg)](https://youtu.be/AMtgBqoLA6g)

- **Notes:**
  - `JOIN`
  - `ON`
  - `INNER JOIN`
  - *Add notes here...*

---

> Click any thumbnail above or check out the links in the table to watch!

---

## 💡 What's Inside & Resources
- **Jupyter Notebooks:** Complete Kaggle exercises using Google BigQuery and Python (`google.cloud.bigquery`).
- **Vlog Walkthroughs:** Casual, raw discussions on SQL logic, dataset exploration, and common pitfalls for beginners.
- **Reference Docs:** [BigQuery Date and Time Functions Documentation](https://docs.cloud.google.com/bigquery/docs/reference/legacy-sql#dayofweek)

---

⭐ *Feel free to check out the notebooks or leave a star if you're following along on your SQL journey!*
