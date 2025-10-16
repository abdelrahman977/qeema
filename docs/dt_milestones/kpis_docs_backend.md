# Milestones Reporting Functions

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
| (result) | BOOLEAN | Returns `TRUE` if updated, `FALSE` if no rows affected. |

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
    2024,
    'Digital Experience & Impact',
    'quarterly'
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

## Developer Notes

* All functions are under schema: `dt_milestone`.
* Use in backend API via:

  ```sql
  SELECT * FROM dt_milestone.<function_name>(...);
  ```
* For null-safe behavior:

  * Pass `NULL` to ignore a filter.
  * All functions return `0` when data doesn’t exist for the selected filters.
* Ensure indexes exist on:

  * `kpi_progress (kpi_id, year, month)`
  * `kpi (unit_id)`
  * for optimal API latency.
