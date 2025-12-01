# QBR Dashboard Functions Documentation

This document provides detailed descriptions for SQL functions under the `qbr_dashboard` schema. Each function can be executed using a `SELECT` query.

⚙️ **Parameter Logic:** If any parameter is `NULL`, the corresponding filter will be **ignored** — the query behaves as if that condition wasn’t applied.

---

# 1. `executive_summary_agile_maturity_index`

## **Description**

Retrieves Agile Maturity Index records, including 5-dimension radar values, highlights, and image path. Filters are optional and nullable.

## **Parameters**

| Parameter   | Type | Nullable | Description        |
| ----------- | ---- | -------- | ------------------ |
| `p_year`    | INT  | ✅        | Filter by year.    |
| `p_quarter` | TEXT | ✅        | Filter by quarter. |
| `p_lob`     | TEXT | ✅        | Filter by LOB.     |

## **Execution Example**

```sql
SELECT * FROM qbr_dashboard.executive_summary_get_agile_maturity_index(
    2024,
    'Q1',
    'EBU'
);
```

## **Return Data Types**

| Column         | Type      | Description            |
| -------------- | --------- | ---------------------- |
| id             | BIGINT    | Unique record ID.      |
| lob            | TEXT      | Line of Business.      |
| year           | INT       | Year of record.        |
| quarter        | TEXT      | Quarter (`Q1`–`Q4`).   |
| strategy       | NUMERIC   | Radar score.           |
| structure      | NUMERIC   | Radar score.           |
| processes      | NUMERIC   | Radar score.           |
| people         | NUMERIC   | Radar score.           |
| technology     | NUMERIC   | Radar score.           |
| key_highlights | TEXT      | Long descriptive text. |
| img_path       | TEXT      | Image or icon path.    |
| created_at     | TIMESTAMP | Record creation time.  |
| updated_at     | TIMESTAMP | Last update time.      |

---

# 2. `executive_summary_overall_maturity_index`

## **Description**

Returns the quarterly overall maturity index with calculated `progress_to_target = overall_maturity_score / maturity_target`.

## **Parameters**

| Parameter   | Type | Nullable | Description        |
| ----------- | ---- | -------- | ------------------ |
| `p_year`    | INT  | ✅        | Filter by year.    |
| `p_quarter` | TEXT | ✅        | Filter by quarter. |

## **Execution Example**

```sql
SELECT * FROM qbr_dashboard.executive_summary_get_overall_maturity_index(
    2025,
    'Q1'
);
```

## **Return Data Types**

| Column                 | Type      | Description                            |
| ---------------------- | --------- | -------------------------------------- |
| id                     | BIGINT    | Unique record ID.                      |
| year                   | INT       | Year of record.                        |
| quarter                | TEXT      | Quarter label.                         |
| overall_maturity_score | NUMERIC   | Actual maturity score.                 |
| maturity_target        | NUMERIC   | Target maturity score (default `4.0`). |
| strategy_score         | NUMERIC   | Breakdown score.                       |
| process_score          | NUMERIC   | Breakdown score.                       |
| technology_score       | NUMERIC   | Breakdown score.                       |
| people_score           | NUMERIC   | Breakdown score.                       |
| structure_score        | NUMERIC   | Breakdown score.                       |
| progress_to_target     | NUMERIC   | Computed ratio.                        |
| created_at             | TIMESTAMP | Record created timestamp.              |
| updated_at             | TIMESTAMP | Last update timestamp.                 |

---

# 3. `executive_summary_get_performance_heatmap`

## **Description**

Retrieves heatmap **metadata only** (LOB, tribe, year, quarter, total_tribe).
All dimension values now live inside the Squad table and are fetched by the squads function.

## **Parameters**

| Parameter   | Type | Nullable | Description                 |
| ----------- | ---- | -------- | --------------------------- |
| `p_year`    | INT  | ✅        | Filter by year.             |
| `p_quarter` | TEXT | ✅        | Filter by quarter.          |
| `p_lob`     | TEXT | ✅        | Filter by line of business. |
| `p_tribe`   | TEXT | ✅        | Filter by tribe name.       |

## **Execution Example**

```sql
SELECT * FROM qbr_dashboard.executive_summary_get_performance_heatmap(
    2024,
    'Q1',
    'EBU',
    'Digital Tribe'
);
```

## **Return Data Types**

| Column      | Type      | Description             |
| ----------- | --------- | ----------------------- |
| id          | BIGINT    | Heatmap record ID.      |
| lob         | TEXT      | Line of Business.       |
| tribe_name  | TEXT      | Tribe name.             |
| year        | INT       | Record year.            |
| quarter     | TEXT      | Record quarter.         |
| total_tribe | NUMERIC   | Business-provided total |
| created_at  | TIMESTAMP | Creation timestamp.     |
| updated_at  | TIMESTAMP | Last update timestamp.  |

---

# 4. `executive_summary_get_performance_heatmap_squads` `executive_summary_get_performance_heatmap_squads`

## **Description**

Returns individual squad maturity scores linked to heatmap records.

## **Parameters**

| Parameter      | Type   | Nullable | Description                                                  |
| -------------- | ------ | -------- | ------------------------------------------------------------ |
| `p_heatmap_id` | BIGINT | ✅        | Retur# 4. `executive_summary_get_performance_heatmap_squads` |

## **Description**

Returns all squad-level maturity scores belonging to a specific heatmap.
Each squad contains the full 5-dimension maturity breakdown + total average.

## **Parameters**

| Parameter      | Type   | Nullable | Description                            |
| -------------- | ------ | -------- | -------------------------------------- |
| `p_heatmap_id` | BIGINT | ✅        | Returns squads for a specific heatmap. |

## **Execution Example**

```sql
SELECT * FROM qbr_dashboard.executive_summary_get_performance_heatmap_squads(5);
```

## **Return Data Types**

| Column        | Type    | Description                     |
| ------------- | ------- | ------------------------------- |
| id            | BIGINT  | Squad record ID.                |
| heatmap_id    | BIGINT  | FK to heatmap table.            |
| squad_name    | TEXT    | Name of squad.                  |
| strategy      | NUMERIC | Strategy score.                 |
| structure     | NUMERIC | Structure score.                |
| processes     | NUMERIC | Processes score.                |
| people        | NUMERIC | People score.                   |
| technology    | NUMERIC | Technology score.               |
| total_average | NUMERIC | Average across the 5 dimensions |

---

## Developer Notes


