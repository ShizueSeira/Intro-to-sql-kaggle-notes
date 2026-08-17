# 📊 Kaggle Intro to SQL Notes & Video Walkthroughs

Welcome! This repository tracks my journey, code solutions, and vlog-style walkthroughs as I complete the [Kaggle Intro to SQL Course](https://www.kaggle.com/learn/intro-to-sql). 

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
  #### A. `SELECT...FROM`
  - **`SELECT`**: Specifies the column(s) you want to retrieve.
  - **`FROM`**: Specifies the table source using the full path: `` `project_name.dataset_name.table_name` ``.
    - **Crucial Rule:** The path in the `FROM` clause **must** be enclosed in backticks (`` ` ``). Single, double, or triple quotes will fail.
    ```python
    query = """
            SELECT score, title
            FROM `bigquery-public-data.hacker_news.full`
            """
    ```
    - *Explanation:* Selects the `score` and `title` columns from the `full` table within the `hacker_news` dataset inside the `bigquery-public-data` project.

  - **Advanced Syntax & Practices:**
    - **Aggregates:** Functions like `COUNT()`, `SUM()`, `AVG()` can be used inside `SELECT`.
    - **Dot Notation (`table_name.column_name`):** Explicitly specifies which table a column belongs to, which is vital when performing table joins.
      - *Sneak Peek / Example (Part 6 Preview):*
        ```sql
        SELECT L.license, COUNT(1) AS number_of_files
        FROM `bigquery-public-data.github_repos.sample_files` AS sf
        INNER JOIN `bigquery-public-data.github_repos.licenses` AS L 
            ON sf.repo_name = L.repo_name
        GROUP BY L.license
        ORDER BY number_of_files DESC
        ```
      - *Breakdown of Dot Notation & Aliasing:*
        - `sample_files` is aliased as `sf` in the `FROM` clause.
        - `licenses` is aliased as `L` in the `INNER JOIN` clause.
        - Instead of writing full table names, dot notation uses aliases (e.g., `L.license` refers to the `license` column inside the `licenses` table).
    - **Aliasing (`AS`):** Assigns custom names to columns or tables to improve readability.
      ```sql
      SELECT COUNT(1) AS Count_of_Deleted_Comments
      FROM `bigquery-public-data.hacker_news.full`
      WHERE deleted = TRUE
      ```
      - *Aggregate Focus:* `COUNT(1)` counts the number of records meeting the `WHERE` condition.
      - *Aliasing Focus:* Replaces default output headers (like `f0_`) with a clean, descriptive title (`Count_of_Deleted_Comments`).

  #### B. `WHERE` Clause
  - Used to filter records by specifying conditions that rows must meet.
  - **Comparison Operators:** `<`, `<=`, `>`, `>=`, `=`, `<>` or `!=`.
    ```sql
    SELECT DISTINCT(country)
    FROM `bigquery-public-data.openaq.global_air_quality`
    WHERE unit = 'ppm'
    ```
    - *Explanation:* Selects countries from the `global_air_quality` table where `unit` is `'ppm'`. `DISTINCT()` eliminates duplicate country names from the final result.

  - **Logical Operators (`AND`, `OR`, `NOT`):** Used to combine multiple conditions in a `WHERE` clause.
    - **`AND` Operator:** Requires **both** statements to be true for a record to be selected.
    - **`OR` Operator:** Requires **at least one** statement to be true.
    - **`NOT` Operator:** Inverts the truth value of a condition.

  - **Range Filtering (`BETWEEN ... AND ...`):** Captures values within an inclusive boundary.
    ```sql
    SELECT country_name, AVG(value) AS avg_ed_spending_pct
    FROM `bigquery-public-data.world_bank_intl_education.international_education`
    WHERE indicator_code = 'SE.XPD.TOTL.GD.ZS' 
      AND year BETWEEN 2010 AND 2017
    GROUP BY country_name
    ORDER BY avg_ed_spending_pct DESC
    ```
    - *Logical Operator Focus (`AND`):* Combines two distinct conditions (`indicator_code = ...` **AND** `year BETWEEN ...`). Both statements must evaluate to `TRUE` for a row to be included.
    - *Range Filtering Focus (`BETWEEN`):* Restricts records to years 2010 through 2017 inclusive (both 2010 and 2017 are valid).

  - **List Filtering:** Use `IN (...)` to filter against a list of specific values.
  - **Pattern Matching (`LIKE`):**
    - `%` Wildcard: Matches 0 or more arbitrary characters.
      ```sql
      WHERE tags LIKE '%bigquery%'
      ```
      *(Matches any tag containing "bigquery" at the beginning, middle, or end).*
    - `_` Wildcard: Matches exactly one character.

  #### C. Query Formatting & Best Practices
  - Store SQL queries inside **triple quotation marks (`"""`)** in Python. This treats the query as a multi-line string literal, preserving line breaks for better structure and visual readability.
  - **Case Sensitivity:** SQL keywords are case-insensitive (`select` vs `SELECT`), but capitalizing SQL commands is standard industry best practice.
  
  ```python
  query = """
          SELECT city
          FROM `bigquery-public-data.openaq.global_air_quality`
          WHERE country = 'US'
          """
---

### Part 3: Group By, Having & Count (Unlisted, 36 mins.)
[![Part 3 Video Thumbnail](https://img.youtube.com/vi/Wh7QfRGJe30/maxresdefault.jpg)](https://youtu.be/Wh7QfRGJe30)

- **Notes:**
  #### A. `COUNT()`
  - Returns the number of non-null records/rows for a specified column.
  - **Syntax / Usage:**
    - `COUNT(column_name)` – Counts non-null entries in that specific column.
    - `COUNT(1)` or `COUNT(*)` – Counts all records/rows regardless of whether individual columns contain `NULL` values. Best used when you are unsure which column to pass to `COUNT()`.
  
  #### B. `GROUP BY`
  - Groups rows that share the same values in specified columns into summary rows.
  - **Aggregations:** When `COUNT()` (or other aggregate functions like `SUM()`, `AVG()`) is combined with a `GROUP BY` clause, it calculates metrics independently for each distinct group.
  - **Golden Rule / Best Practice:** Every column selected in your `SELECT` statement **must** either:
    1. Be explicitly listed in the `GROUP BY` clause, **or**
    2. Be wrapped inside an aggregate function (e.g., `COUNT()`, `SUM()`, `AVG()`).
    - *Why?* Including an un-aggregated column that isn't grouped creates ambiguity (the engine doesn't know which row value to return for that group), resulting in an error.

  #### C. `HAVING`
  - Used to filter groups **after** they have been aggregated by `GROUP BY`.
  - **Difference between `WHERE` and `HAVING`:**
    - `WHERE` filters individual rows **before** any grouping or aggregation takes place.
    - `HAVING` filters aggregated group records **after** `GROUP BY` is applied.
  - **Example Query & Visual Walkthrough:**
    ```sql
    SELECT author, COUNT(1) AS total_posts
    FROM `bigquery-public-data.hacker_news.comments`
    GROUP BY author
    HAVING COUNT(1) > 100
    ```
    - **How it works step-by-step:**
      1. `GROUP BY author` aggregates all records matching the same author name together.
      2. `COUNT(1)` calculates the total number of posts associated with that author after grouping is complete.
         - *Example:* If `'john'` appears in row 1 and row 14, `GROUP BY` collapses these into a single row for `'john'` with `total_posts = 2`.
      3. `HAVING COUNT(1) > 100` filters out any grouped records that do not meet the condition.
         - Since `'john'` only has 2 posts, his record is filtered out and will not appear in the final output.
         - An author with 101 posts satisfies the condition (`101 > 100`) and will be displayed in the results.

  #### D. Aliasing (`AS`)
  - Assigns a temporary name to a column or table to make query results cleaner and easier to read (e.g., `COUNT(1) AS total_posts`).
  - *(Note: Though detailed in Part 3 of the course, aliasing concepts are introduced in Part 2 under `SELECT` practices).*

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
