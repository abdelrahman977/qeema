## Dynamic Process Report

This document provides details on SQL functions used for retrieving report statistics. Each function is executed using a `SELECT` query, with parameters allowing flexible filtering.

---

### 1. `f_get_reports_count_by_status`

#### **Description:**

Returns the count of reports grouped by status.

#### **Parameters:**

- `chosen_date_from` (DATE) – Start date (nullable, format: 'YYYY-MM-DD')
- `chosen_date_to` (DATE) – End date (nullable, format: 'YYYY-MM-DD')

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_reports_count_by_status(
    '2024-01-01', -- DATE FROM 'YYYY-MM-DD'
    '2024-12-31'  -- DATE TO 'YYYY-MM-DD'
);
```

#### **Return Data Types:**

| **Column** | **Data Type** |
| ---------- | ------------- |
| status     | TEXT          |
| count      | BIGINT        |

---

### 2. f\_get\_reports\_count\_by\_month

#### **Description:**

Returns the count of reports grouped by month.

#### **Parameters:**

- `chosen_category` (VARCHAR) – Category filter (nullable, supports comma-separated values, e.g., `'category1,category2'`)
- `chosen_status` (VARCHAR) – Status filter (nullable, supports comma-separated values)
- `chosen_date_from` (DATE) – Start date (nullable, format: 'YYYY-MM-DD')
- `chosen_date_to` (DATE) – End date (nullable, format: 'YYYY-MM-DD')

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_reports_count_by_month(
    'category1,category2', -- category filter
    'status1,status2', -- status filter
    '2024-01-01', -- DATE FROM 'YYYY-MM-DD'
    '2024-12-31'  -- DATE TO 'YYYY-MM-DD'
);
```

#### **Return Data Types:**

| Column | Data Type |
| ------ | --------- |
| year   | INT       |
| month  | INT       |
| count  | BIGINT    |

---

### 3. `f_get_reports_count_by_category`

#### **Description:**

Returns the count of reports grouped by category.

#### **Parameters:**

- `chosen_category` (VARCHAR) – Category filter (nullable, supports comma-separated values, e.g., `'category1,category2'`)
- `chosen_date_from` (DATE) – Start date (nullable, format: 'YYYY-MM-DD')
- `chosen_date_to` (DATE) – End date (nullable, format: 'YYYY-MM-DD')

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_reports_count_by_category(
    'category1,category2', -- category filter
    '2024-01-01', -- DATE FROM 'YYYY-MM-DD'
    '2024-12-31'  -- DATE TO 'YYYY-MM-DD'
);
```

#### **Return Data Types:**

| Column   | Data Type |
| -------- | --------- |
| category | TEXT      |
| count    | BIGINT    |

---

### 4. `f_get_reports_avg_response_time`

#### **Description:**

Returns the average response time for reports, grouped by year and month.

#### **Parameters:**

- `chosen_category` (VARCHAR) – Category filter (nullable, supports comma-separated values, e.g., `'category1,category2'`)
- `chosen_date_from` (DATE) – Start date (nullable, format: 'YYYY-MM-DD')
- `chosen_date_to` (DATE) – End date (nullable, format: 'YYYY-MM-DD')

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_reports_avg_response_time(
    'category1,category2', -- category filter
    '2024-01-01', -- DATE FROM 'YYYY-MM-DD'
    '2024-12-31'  -- DATE TO 'YYYY-MM-DD'
);
```

#### **Return Data Types:**

| Column              | Data Type        |
| ------------------- | ---------------- |
| year                | INT              |
| month               | INT              |
| avg\_response\_time | DOUBLE PRECISION |

---

### 5. `f_get_reports_percentage_sla`

#### **Description:**

Returns the percentage of reports classified as 'Within SLA' or 'Breached SLA'.

#### **Parameters:**

- `chosen_category` (VARCHAR) – Category filter (nullable, supports comma-separated values, e.g., `'category1,category2'`)
- `chosen_date_from` (DATE) – Start date (nullable, format: 'YYYY-MM-DD')
- `chosen_date_to` (DATE) – End date (nullable, format: 'YYYY-MM-DD')

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_reports_percentage_sla(
    'category1,category2', -- category filter
    '2024-01-01', -- DATE FROM 'YYYY-MM-DD'
    '2024-12-31'  -- DATE TO 'YYYY-MM-DD'
);
```

#### **Return Data Types:**

| Column     | Data Type        |
| ---------- | ---------------- |
| sla        | TEXT             |
| percentage | DOUBLE PRECISION |

