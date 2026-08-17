# 📊 Kaggle Intro to SQL Notes & Video Walkthroughs

Welcome! The repository tracks my journey, code solutions, and vlog-style walkthroughs as I complete the [Kaggle Intro to SQL Course](https://www.kaggle.com/learn/intro-to-sql). 

In these videos, I break down what I learned, walk through BigQuery SQL concepts, and yap about dataset exploration! 🎙️✨

---

## 🚀 Course Progress & Exercises

| Part | Topic | Notebook Link of Exercises | Video Walkthrough |
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
  - Store SQL queries inside **triple quotation marks (`"""`)** in Python. SQL treats the query as a multi-line string literal, preserving line breaks for better structure and visual readability.
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

  - **Valid Example Query (`query_good`):**
    ```sql
    SELECT parent, COUNT(id)
    FROM `bigquery-public-data.hacker_news.full`
    GROUP BY parent
    ```
    - *Applying Rule 1:* `parent` is explicitly listed in the `GROUP BY` clause because we want to aggregate rows with matching `parent` values.
    - *Applying Rule 2:* `COUNT(id)` wraps the `id` column inside an aggregate function, displaying the total count of records corresponding to each `parent` value.

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
    - **How the query works step-by-step:**
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
  #### A. `ORDER BY`
  - Sorts query results based on one or more specified columns.
  - **Sorting Order:**
    - **Ascending (`ASC` - Default):** Sorts from lowest to highest (1 to 9) or alphabetically (A to Z).
    - **Descending (`DESC`):** Reverses the order from highest to lowest or reverse alphabetically (Z to A).
  - **Visual Example:**
    ![ORDER BY Visual Example](https://storage.googleapis.com/kaggle-media/learn/images/IElLJrR.png)
    - *Explanation:* In the image above, sorting the animal column in descending order (`ORDER BY animal DESC`) places **Rabbit** first (Z to A), followed by **Dog**, and then **Cat**. Without `DESC`, the default ascending order places **Cat** first, followed by **Dog**, then **Rabbit**.

  #### B. Date & Datetime Data Types
  - BigQuery primarily uses two formats to handle temporal data:
    - **`DATE` Format:** `YYYY-[M]M-[D]D`
      - Features a 4-digit year, 1-to-2 digit month, and 1-to-2 digit day.
      - *Example:* `2024-06-17` represents **June 17, 2024**.
    - **`DATETIME` Format:** `YYYY-[M]M-[D]D hh:mm:ss`
      - Extends the date format by adding time details in 2-digit hours, minutes, and seconds.
      - *Example:* `2024-06-17 14:30:00` represents **June 17, 2024 at 2:30:00 PM**.

  #### C. `EXTRACT()` Function
  - Extracts specific date components (e.g., `YEAR`, `MONTH`, `DAY`, `WEEK`, `DAYOFWEEK`) from a `DATE` or `DATETIME` field.
  - **Visual Example:**
    ![EXTRACT Function Visual Example](https://storage.googleapis.com/kaggle-media/learn/images/vhvHIh0.png)
    - **Key Examples from Diagram:**
      1. `EXTRACT(DAY FROM Date) AS Day` on *Dr. Harris Bonkers* (`2019-01-08`) extracts **`8`** (from the `DD` component).
      2. `EXTRACT(WEEK FROM Date) AS Week` on *Tom* (`2019-05-16`) extracts **`19`**, as May 16th falls in the 19th week of 2019.

  > **Resource:** Check out the official [BigQuery Date and Time Functions Documentation](https://docs.cloud.google.com/bigquery/docs/reference/legacy-sql#datetimefunctions) for additional date extraction parameters and functions!
---

### Part 5: As & With (Unlisted, 66 mins.)
[![Part 5 Video Thumbnail](https://img.youtube.com/vi/toSMZayMrYE/maxresdefault.jpg)](https://youtu.be/toSMZayMrYE)

- **Notes:**
  #### A. Common Table Expressions (CTEs) & `WITH...AS`
  - Combining `WITH` and `AS` allows you to create a **Common Table Expression (CTE)**.
  - **What is a CTE?** A temporary named result set created to simplify and organize complex queries by breaking them down into smaller, modular steps.

  #### B. CTE Process & Visual Demonstration
  ![CTE Visual Example](https://storage.googleapis.com/kaggle-media/learn/images/3xQZM4p.png)

  - **How a CTE Works Step-by-Step:**
    1. **Define the CTE (`WITH...AS (...)`):** Write the CTE definition using `WITH cte_name AS (...)` with the inner query enclosed in parentheses `()`. This constructs your desired temporary table first.
    2. **Query the CTE:** Write your main query directly below, referencing the temporary table name specified in Step 1.

  - **Example Query & Walkthrough:**
    ```sql
    WITH Seniors AS (
        SELECT ID, Name
        FROM `pet_data.pets`
        WHERE Years_old > 5
    )
    SELECT ID
    FROM Seniors
    ```
    - **Step 1 Execution (`Seniors`):** Filters the `pets` table for records where `Years_old > 5` and temporarily stores `ID` and `Name` into the `Seniors` CTE.
    - **Step 2 Execution:** Selects `ID` directly from the newly created `Seniors` CTE table, yielding the final result (IDs `2` and `4`).

  #### C. Key Rules & Scope
  - A CTE exists **only** during the execution of the query it is attached to.
  - CTE cannot be referenced or reused in subsequent, separate queries.

  #### D. Practical CTE Example (Chicago Taxi Trips)
  ```python
  speeds_query = """
                 WITH RelevantRides AS
                 (
                     SELECT EXTRACT(HOUR FROM trip_start_timestamp) AS hour_of_day,
                            trip_miles,
                            trip_seconds
                     FROM `bigquery-public-data.chicago_taxi_trips.taxi_trips`
                     WHERE trip_start_timestamp > '2016-01-01' 
                         AND trip_start_timestamp < '2016-04-01' 
                         AND trip_seconds > 0 
                         AND trip_miles > 0
                 )
                 SELECT hour_of_day,
                        COUNT(1) AS num_trips,
                        3600 * SUM(trip_miles) / SUM(trip_seconds) AS avg_mph
                 FROM RelevantRides
                 GROUP BY hour_of_day
                 ORDER BY hour_of_day
                 """
  ```
  - **Breakdown of Query Logic:**
    - **Filtering inside the CTE:** Inside the constructed `RelevantRides` CTE, the `WHERE` clause defines the date range for `trip_start_timestamp` while ensuring both `trip_seconds` and `trip_miles` are strictly greater than zero.
    - **Simplified Main Query:** Referencing the CTE table makes the subsequent execution statement much simpler, focusing on summarizing average miles per hour (`avg_mph`) and the total number of trips (`num_trips`) depending on the hours of the day.
    - **Efficiency & Readability:** An efficient way to construct code that remains clean, organized, and convenient for readers.
      
---

### Part 6: Joining Data (Unlisted, 75 mins.)
[![Part 6 Video Thumbnail](https://img.youtube.com/vi/AMtgBqoLA6g/maxresdefault.jpg)](https://youtu.be/AMtgBqoLA6g)

- **Notes:**
  #### A. Core Concepts: `JOIN`, `ON`, and `INNER JOIN`
  - **`JOIN`:** Crucial for relational databases with multiple related tables. One table originates in the `FROM` clause (the primary/parent table), while the second table is specified in the `JOIN` clause. 
    > *Note:* While other types exist (e.g., `LEFT JOIN`, `RIGHT JOIN`), the course focuses specifically on `INNER JOIN`.
  - **`ON`:** Specifies the matching criteria between tables, linking the identifier column in the first table to the corresponding column in the second table.
  - **`INNER JOIN`:** Returns only the records that have matching identifiers present in **both** tables. If a record in Table A lacks a matching ID in Table B (or vice versa), the record will be excluded from the final output.

  #### B. Practical Example (Stack Overflow BigQuery Experts)
  ```python
  bigquery_experts_query = """
      SELECT p_a.owner_user_id AS user_id, COUNT(1) AS number_of_answers
      FROM `bigquery-public-data.stackoverflow.posts_questions` AS p_q
      INNER JOIN `bigquery-public-data.stackoverflow.posts_answers` AS p_a
          ON p_q.id = p_a.parent_id
      WHERE p_q.tags LIKE '%bigquery%'
      GROUP BY user_id
  """
  ```

  - **Breakdown of Query Logic:**
    - **Linking Tables:** The query joins `posts_questions` (aliased as `p_q`) and `posts_answers` (aliased as `p_a`) where `p_q.id` matches `p_a.parent_id`.
    - **Dot Notation & Aliases:** Table aliases (`p_q` and `p_a`) are used alongside dot notation to cleanly reference columns from specific tables.
    - **Filtering:** The `WHERE` clause ensures only questions tagged with `'bigquery'` are considered.
    - **Aggregation:** Groups results by `user_id` (`owner_user_id`) to count how many answers each user provided (`number_of_answers`).
---

> Click any thumbnail above or check out the links in the table to watch!

---

## 💡 References :
- **Jupyter Notebooks:** Complete Kaggle exercises using Google BigQuery and Python (`google.cloud.bigquery`).
- **Vlog Walkthroughs:** Casual, raw discussions on SQL logic and dataset exploration.
- **Reference Docs:** [BigQuery Date and Time Functions Documentation](https://docs.cloud.google.com/bigquery/docs/reference/legacy-sql#dayofweek)

---
