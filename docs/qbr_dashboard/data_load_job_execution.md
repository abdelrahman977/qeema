# QBR Dashboards – Data Load

This document describes how to execute **QBR Dashboard upload jobs** into the database.

## Jobs Execution Commands

---

### Qeema SIT – Full Command Example

```bash
/home/dev/stc-dashboards/sit/data/QBR_Dashboards/talend_jobs/QBR_Dashboards_Automation/QBR_Dashboards_Automation_Job/QBR_Dashboards_Automation_Job_run.sh \
--context_param executed_by=<EXECUTOR_NAME> \
--context_param original_file_name=<CSV_ORIGINAL_FILE_NAME> \
--context_param file_name=<CSV_FILE_NAME> \
--context_param dashboard_name=<DASHBOARD_NAME> \
--context_param heatmap_id=<HEATMAP_ID>

```

---

### Production – Full Command Example

```bash
/data/cem_eye/ceo_dashboard/QBR_Dashboards/talend_jobs/QBR_Dashboards_Automation/QBR_Dashboards_Automation_Job/QBR_Dashboards_Automation_Job_run.sh \
--context_param executed_by=<EXECUTOR_NAME> \
--context_param original_file_name=<CSV_ORIGINAL_FILE_NAME> \
--context_param file_name=<CSV_FILE_NAME> \
--context_param dashboard_name=<DASHBOARD_NAME> \
--context_param heatmap_id=<HEATMAP_ID>
```

---


### Context Parameters Reference

This section explains **all supported context parameters**, their purpose, and whether they are **mandatory**.

| Context Parameter    | Required       | Description                                                                   |
| -------------------- | -------------- | ----------------------------------------------------------------------------- |
| `executed_by`        | ✅ Yes          | Name of the user or system executing the job (used for audit/logging). |
| `original_file_name` | ✅ Yes          | Original uploaded file name (kept for traceability and audit purposes).       |
| `file_name`          | ✅ Yes          | Actual CSV file name to be processed by the job.                       |
| `dashboard_name`     | ✅ Yes          | Identifies which dashboard/table the data will be loaded into. Values are `overall_maturity_index`, `executive_summary_performance_heatmap` and `executive_summary_performance_heatmap_squads`            |
| `heatmap_id`         | ⚠️ Conditional | Required **only** when passed `dashboard_name` is `executive_summary_performance_heatmap_squads`  |

---

#### Important Notes

`dashboard_name` controls which table is updated:

- `overall_maturity_index` → `qbr_dashboard.executive_summary_overall_maturity_index`
- `executive_summary_performance_heatmap` → `qbr_dashboard.executive_summary_performance_heatmap`
- `executive_summary_performance_heatmap_squads` → `qbr_dashboard.executive_summary_performance_heatmap_squads`

> **Note:** `heatmap_id` **must reference an existing ID** in `qbr_dashboard.executive_summary_performance_heatmap`.
