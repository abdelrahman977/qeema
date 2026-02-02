# Milestones KPIs Functions

This document provides detailed descriptions for SQL functions under the `dt_milestone` schema.
Each function can be executed using a `SELECT` query.

⚙️ **Parameter Logic:**
If any parameter is `NULL`, the corresponding filter will be **ignored** — the query behaves as if that condition wasn’t applied.

---

## 1. `f_dt_milestones_get_kpis_list`

### **Description:**

Retrieves a list of KPIs for a given unit and dimension.

### **Parameters:**

| Parameter         | Type    | Nullable | Description       |
| ----------------- | ------- | -------- | ----------------- |
| `input_unit_id`   | INT     | ✅        | Unit filter.      |
| `input_dimention` | VARCHAR | ✅        | Dimension filter. |

### **Execution Example:**

```sql
SELECT * FROM dt_milestone.f_dt_milestones_get_kpis_list(
    2,
    'Capability Utilization'
);
```

### **Return Data Types:**

| Column    | Type    | Description            |
| --------- | ------- | ---------------------- |
| kpi_id    | BIGINT  | Unique KPI identifier. |
| name      | VARCHAR | KPI name.              |
| dimension | VARCHAR | KPI dimension name.    |

---

## 2. `f_dt_milestones_update_kpi_value`

### **Description:**

Updates an existing KPI’s value for a given year and month in the `kpi_progress` table.

### **Parameters:**

| Parameter      | Type    | Nullable | Description            |
| -------------- | ------- | -------- | ---------------------- |
| `input_kpi_id` | BIGINT  | ❌        | KPI ID to update.      |
| `input_year`   | INT     | ❌        | Year of the record.    |
| `input_month`  | INT     | ❌        | Month of the record.   |
| `input_value`  | NUMERIC | ❌        | Updated numeric value. |

### **Execution Example:**

```sql
SELECT * FROM dt_milestone.f_dt_milestones_update_kpi_value(
    361,
    2025,
    1,
    10
);
```

### **Return Data Types:**

| Column   | Type    | Description                                             |
| -------- | ------- | ------------------------------------------------------- |
| (result) | BIGINT | Returns the ID of the affected row in 'kpi_progress' table, raises error otherwise. |

---

## 3. `f_dt_milestones_get_kpi_values`

### **Description:**

Retrieves historical KPI progress values, either monthly or quarterly.
When `quarterly`, the function automatically returns the **latest record within each quarter**.

### **Parameters:**

| Parameter         | Type    | Nullable | Description                   |
| ----------------- | ------- | -------- | ----------------------------- |
| `input_unit_id`   | BIGINT  | ✅        | Unit filter.                  |
| `input_kpi_id`    | BIGINT  | ✅        | KPI filter.                   |
| `input_dimention` | VARCHAR | ✅        | Dimension filter.             |
| `input_grouping`  | VARCHAR | ✅        | `'monthly'` or `'quarterly'`. |

### **Execution Example:**

```sql
SELECT * FROM dt_milestone.f_dt_milestones_get_kpi_values(
    NULL,
    NULL,
    'Digital Experience & Impact',
    'quarterly'
);
```

### **Return Data Types:**

| Column        | Type    | Description                    |
| ------------- | ------- | ------------------------------ |
| progress_id   | BIGINT  | Record ID from `kpi_progress`. |
| unit_id       | BIGINT  | Unit identifier.               |
| kpi_id        | BIGINT  | KPI identifier.                |
| dimension     | VARCHAR | KPI dimension.                 |
| year          | INT     | Year.                          |
| quarter       | TEXT    | Quarter label (e.g., `Q1`).    |
| month         | INT     | Month number.                  |
| progress_date | DATE    | Record date.                   |
| value         | NUMERIC | KPI value.                     |

---

## 4. `f_dt_milestones_get_kpi_attributes`

### **Description:**

Fetches KPI attributes such as weight, baseline, ambition, and the most recent value.

### **Parameters:**

| Parameter         | Type    | Nullable | Description       |
| ----------------- | ------- | -------- | ----------------- |
| `input_unit_id`   | BIGINT  | ✅        | Unit filter.      |
| `input_kpi_id`    | BIGINT  | ✅        | KPI filter.       |
| `input_dimention` | VARCHAR | ✅        | Dimension filter. |

### **Execution Example:**

```sql
SELECT * FROM dt_milestone.f_dt_milestones_get_kpi_attributes(
    NULL,
    355,
    'Digital Experience & Impact'
);
```

### **Return Data Types:**

| Column        | Type    | Description                 |
| ------------- | ------- | --------------------------- |
| unit_id       | BIGINT  | Unit ID.                    |
| kpi_id        | BIGINT  | KPI ID.                     |
| dimension     | VARCHAR | KPI dimension.              |
| current_value | NUMERIC | Latest KPI progress value.  |
| weight        | NUMERIC | Weight of KPI in dimension. |
| baseline      | NUMERIC | Baseline value.             |
| target        | NUMERIC | Target value.               |
| ambition      | NUMERIC | Ambition value.             |
| direction      | SMALLINT | Direction value.             |

---

## 5. `f_dt_milestones_get_unit_progress`

### **Description:**

Calculates the overall **Digital Index (DI)** progress for a unit and year by aggregating weighted KPI results.

### **Parameters:**

| Parameter       | Type   | Nullable | Description  |
| --------------- | ------ | -------- | ------------ |
| `input_unit_id` | BIGINT | ✅        | Unit filter. |
| `input_year`    | INT    | ✅        | Year filter. |

### **Execution Example:**

```sql
SELECT * FROM dt_milestone.f_dt_milestones_get_unit_progress(
    NULL,
    2025
);
```

### **Return Data Types:**

| Column             | Type    | Description                         |
| ------------------ | ------- | ----------------------------------- |
| unit_id            | BIGINT  | Unit ID.                            |
| year               | INT     | Year.                               |
| unit_baseline      | NUMERIC | Baseline of unit.                   |
| unit_target        | NUMERIC | Target of unit.                     |
| di_actual_progress | NUMERIC | Weighted DI progress for that year. |

---

## 6. `f_dt_milestones_get_unit_progress_series`

### **Description:**

Generates a **time series** of DI progress by month or quarter.
Automatically zeros out periods before the first KPI update in each dimension.
Adds an “Overall” record (aggregating all dimensions) when `input_dimension` is `NULL`.

### **Parameters:**

| Parameter          | Type    | Nullable | Description                   |
| ------------------ | ------- | -------- | ----------------------------- |
| `input_unit_id`    | BIGINT  | ✅        | Unit filter.                  |
| `input_year`       | INT     | ✅        | Year filter.                  |
| `input_dimension`  | VARCHAR | ✅        | Dimension filter.             |
| `input_grouped_by` | VARCHAR | ✅        | `'monthly'` or `'quarterly'`. |

### **Execution Example:**

```sql
SELECT * FROM dt_milestone.f_dt_milestones_get_unit_progress_series(
    1,
    2025,
    'Digital Experience & Impact',
    'monthly'
);
```

### **Return Data Types:**

| Column             | Type    | Description                    |
| ------------------ | ------- | ------------------------------ |
| unit_id            | BIGINT  | Unit ID.                       |
| year               | INT     | Year.                          |
| period             | INT     | Month (1–12) or quarter (1–4). |
| dimension          | VARCHAR | KPI dimension or “Overall”.    |
| unit_baseline      | NUMERIC | Unit baseline value.           |
| unit_target        | NUMERIC | Unit target value.             |
| di_actual_progress | NUMERIC | DI progress per month/quarter. |

---

## 7. `f_dt_milestones_get_avg_units_progress`

### **Description:**

Computes the **average DI progress** across all units for a given year.

### **Parameters:**

| Parameter    | Type | Nullable | Description  |
| ------------ | ---- | -------- | ------------ |
| `input_year` | INT  | ✅        | Year filter. |

### **Execution Example:**

```sql
SELECT dt_milestone.f_dt_milestones_get_avg_units_progress(2025);
```

### **Return Data Types:**

| Column   | Type    | Description                           |
| -------- | ------- | ------------------------------------- |
| (result) | NUMERIC | Average DI progress across all units. |

---

## 8. `f_kpi_overview`

### **Description:**

Returns a unified KPI overview matrix with:

* All KPI attributes (excluding the `id` column)
* `unit_name` instead of `unit_id`
* `year` from `kpi_progress`
* Monthly values `jan`…`dec` dynamically pivoted

Filters (`p_year`, `p_unit_id`, `p_dimension`) are optional — if `NULL`, they are ignored.

### **Parameters:**

| Parameter     | Type    | Nullable | Description                        |
| ------------- | ------- | -------- | ---------------------------------- |
| `p_year`      | INT     | ✅        | Year filter |
| `p_unit_id`   | BIGINT  | ✅        | Unit filter         |
| `p_dimension` | VARCHAR | ✅        | Dimension filter |
### **Execution Examples:**

```sql
SELECT * FROM dt_milestone.f_kpi_overview(2025, 1, 'Digital Experience & Impact');
```

### **Return Data Types:**

| Column      | Type      | Description                  |
| ----------- | --------- | ---------------------------- |
| name        | VARCHAR   | KPI name.                    |
| dimension   | VARCHAR   | KPI dimension.               |
| weight      | NUMERIC   | Weight value.                |
| baseline    | NUMERIC   | Baseline value.              |
| target      | NUMERIC   | Target value.                |
| ambition    | NUMERIC   | Ambition value.              |
| formula     | VARCHAR   | KPI formula.                 |
| created_by  | VARCHAR   | Created by.                  |
| create_date | TIMESTAMP | Creation date.               |
| updated_by  | VARCHAR   | Last updated by.             |
| update_date | TIMESTAMP | Last update timestamp.       |
| unit_name   | VARCHAR   | Unit name from `unit` table. |
| direction   | SMALLINT  | Direction of improvement.    |
| year        | INT       | Year of the progress.        |
| jan         | NUMERIC   | January value.               |
| feb         | NUMERIC   | February value.              |
| mar         | NUMERIC   | March value.                 |
| apr         | NUMERIC   | April value.                 |
| may         | NUMERIC   | May value.                   |
| jun         | NUMERIC   | June value.                  |
| jul         | NUMERIC   | July value.                  |
| aug         | NUMERIC   | August value.                |
| sep         | NUMERIC   | September value.             |
| oct         | NUMERIC   | October value.               |
| nov         | NUMERIC   | November value.              |
| dec         | NUMERIC   | December value.              |

---

## 9. `f_dt_milestones_get_kpi_dimensions`

### **Description**

Returns KPI dimensions from the `kpi` table joined with `kpi_progress`.
Both filters (`p_year`, `p_unit_id`) are optional — if a parameter is `NULL`, that filter is ignored.

This function provides a flexible way to fetch KPI dimensions for any year, any unit, or all units/years.

---

### **Parameters**

| Parameter   | Type | Nullable | Description                       |
| ----------- | ---- | -------- | --------------------------------- |
| `p_year`    | INT  | ✅        | Year filter (from `kpi_progress`) |
| `p_unit_id` | INT  | ✅        | Unit filter (from `kpi.unit_id`)  |

---

### **Execution Examples**

```sql
-- Apply both filters
SELECT * FROM dt_milestone.f_get_kpi_dimensions(2025, 2);

-- Only filter by unit
SELECT * FROM dt_milestone.f_get_kpi_dimensions(NULL, 2);

-- Only filter by year
SELECT * FROM dt_milestone.f_get_kpi_dimensions(2025, NULL);

-- No filters (return all dimensions)
SELECT * FROM dt_milestone.f_get_kpi_dimensions(NULL, NULL);
```

---

### **Return Data Types**

| Column    | Type | Description                   |
| --------- | ---- | ----------------------------- |
| dimension | VARCHAR | The KPI dimension from `kpi`. |

---

## 10. `f_dt_milestones_get_kpi_milestones`

### **Description:**

Retrieves **milestones of specific kpi** , optionally filtered by year.


⚙️ **Null-Safe Filtering**
If any parameter is `NULL`, its filter is ignored.

---

### **Parameters:**

| Parameter    | Type   | Nullable | Description                    |
| ------------ | ------ | -------- | ------------------------------ |
| `kpi_id_arg` | BIGINT | ✅        | KPI identifier filter.         |
| `year_arg`   | INT    | ✅        | Filter by milestone start year |

---

### **Execution Example:**

```sql
SELECT *
FROM dt_milestone.f_dt_milestones_get_kpi_milestones(
    1,
    NULL
);
```

---

### **Return Data Types:**

| Column         | Type    | Description                           |
| -------------- | ------- | ------------------------------------- |
| start_dt       | DATE    | Milestone start date.                 |
| end_dt         | DATE    | Milestone end date.                   |
| year_num       | INTEGER | Calendar year (from lookup table).    |
| quarter_name   | TEXT    | Quarter name (e.g., `Q1`).            |
| month_num      | INTEGER | Calendar month number (1–12).         |
| work_stream    | TEXT    | Work stream name.                     |
| activity_name  | TEXT    | Activity name.                        |
| milestone_name | TEXT    | Milestone name.                       |
| team_name      | TEXT    | Team responsible for milestone.       |
| status         | TEXT    | Milestone status (e.g., `Completed`). |

---

## 11. `f_dt_milestones_kpi_completion`

### **Description:**

Calculates **completion statistics** for milestones associated with a KPI.

The function returns:

* Total milestone count
* Completed milestone count
* Completion ratio (`completed / total`)

⚙️ **Null-Safe Filtering**
If a parameter is `NULL`, its filter is ignored.

---

### **Parameters:**

| Parameter    | Type   | Nullable | Description                    |
| ------------ | ------ | -------- | ------------------------------ |
| `kpi_id_arg` | BIGINT | ✅        | KPI identifier filter.         |
| `year_arg`   | INT    | ✅        | Filter by milestone start year |

---

### **Execution Example:**

```sql
SELECT *
FROM dt_milestone.f_dt_milestones_kpi_completion(
    1,
    NULL
);
```

---

### **Return Data Types:**

| Column           | Type    | Description                                          |
| ---------------- | ------- | ---------------------------------------------------- |
| completed_count  | BIGINT  | Number of milestones with status = `Completed`.      |
| total_count      | BIGINT  | Total number of milestones.                          |
| completion_ratio | NUMERIC | Completion ratio (0–1), rounded to 4 decimal places. |

---

## Developer Notes (Extended)

* All functions are under schema: `dt_milestone`
* Completion logic is based on:

  ```sql
  status = 'Completed'
  ```
* Completion ratio safely handles zero milestones (returns `0`)
* Year filtering is based on:

  ```sql
  EXTRACT(YEAR FROM milestone.start_date)
  ```


## Developer Notes

* All functions are under schema: `dt_milestone`.
* For null-safe behavior:

  * Pass `NULL` to ignore a filter.




