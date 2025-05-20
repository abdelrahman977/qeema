## Dynamic Process Report

This document provides details on SQL functions used for retrieving report statistics. Each function is executed using a `SELECT` query, with parameters allowing flexible filtering. **If any filter parameter is NULL, it will be completely ignored, as if it was not included in the query.**

---

### 1. `f_get_requests_count_by_status`

#### **Description:**

Returns the count of requests grouped by status.

#### **Parameters:**

* `chosen_date_from` (DATE) – Start date (nullable, format: 'YYYY-MM-DD')
* `chosen_date_to` (DATE) – End date (nullable, format: 'YYYY-MM-DD')

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_requests_count_by_status(
    '2024-01-01',
    '2024-12-31'
);
```

#### **Return Data Types:**

| Column | Data Type |
| ------ | --------- |
| status | TEXT      |
| count  | BIGINT    |

---

### 2. `f_get_requests_count_by_month`

#### **Description:**

Returns the count of requests grouped by month.

#### **Parameters:**

* `chosen_category` (VARCHAR) – Category filter (nullable, supports comma-separated values)
* `chosen_status` (VARCHAR) – Status filter (nullable, supports comma-separated values)
* `chosen_date_from` (DATE) – Start date (nullable)
* `chosen_date_to` (DATE) – End date (nullable)

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_requests_count_by_month(
    'category1,category2',
    'status1,status2',
    '2024-01-01',
    '2024-12-31'
);
```

#### **Return Data Types:**

| Column | Data Type |
| ------ | --------- |
| year   | INT       |
| month  | INT       |
| count  | BIGINT    |

---

### 3. `f_get_requests_count_by_category`

#### **Description:**

Returns the count of requests grouped by category.

#### **Parameters:**

* `chosen_category` (VARCHAR) – Category filter (nullable, supports comma-separated values)
* `chosen_date_from` (DATE) – Start date (nullable)
* `chosen_date_to` (DATE) – End date (nullable)

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_requests_count_by_category(
    'category1,category2',
    '2024-01-01',
    '2024-12-31'
);
```

#### **Return Data Types:**

| Column   | Data Type |
| -------- | --------- |
| category | TEXT      |
| count    | BIGINT    |

---

### 4. `f_get_requests_avg_response_time`

#### **Description:**

Returns the average response time for requests, grouped by year and month.

#### **Parameters:**

* `chosen_category` (VARCHAR) – Category filter (nullable, supports comma-separated values)
* `chosen_date_from` (DATE) – Start date (nullable)
* `chosen_date_to` (DATE) – End date (nullable)

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_requests_avg_response_time(
    'category1,category2',
    '2024-01-01',
    '2024-12-31'
);
```

#### **Return Data Types:**

| Column              | Data Type        |
| ------------------- | ---------------- |
| year                | INT              |
| month               | INT              |
| avg\_response\_time | DOUBLE PRECISION |

---

### 5. `f_get_requests_percentage_sla`

#### **Description:**

Returns the percentage of requests classified as 'Within SLA' or 'Breached SLA'.

#### **Parameters:**

* `chosen_category` (VARCHAR) – Category filter (nullable, supports comma-separated values)
* `chosen_date_from` (DATE) – Start date (nullable)
* `chosen_date_to` (DATE) – End date (nullable)

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_requests_percentage_sla(
    'category1,category2',
    '2024-01-01',
    '2024-12-31'
);
```

#### **Return Data Types:**

| Column     | Data Type        |
| ---------- | ---------------- |
| sla        | TEXT             |
| percentage | DOUBLE PRECISION |

---

### 6. `f_get_requests_user_avg_response_time`

#### **Description:**

Returns the average number of workday hours per request approval for a given user, grouped by year and month.

#### **Parameters:**

* `chosen_user` (VARCHAR) – User filter (nullable, supports comma-separated values, e.g., `'user1,user2'`)
* `chosen_date_from` (DATE) – Start date (nullable)
* `chosen_date_to` (DATE) – End date (nullable)

#### **Execution Example:**

```sql
SELECT * FROM report_flow.f_get_requests_user_avg_response_time(
    'ashraf00@qeema.net',
    NULL,
    NULL
);
```

#### **Return Data Types:**

| Column                 | Data Type        |
| ---------------------- | ---------------- |
| year                   | INT              |
| month                  | INT              |
| avg\_hours\_per\_month | DOUBLE PRECISION |
