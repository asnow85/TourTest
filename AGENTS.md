# Repository Instructions for Codex

## Project objective

Build and improve a SQL-first Python desktop app for direct fleet operations data entry, issue tracking, and report generation.

This is a clean rebuild. Use the local app-owned database `FleetOpsReporting` and schema `ops` as the default target.

## Non-negotiable architecture rules

- SQL Server is the system of record.
- The default database is `FleetOpsReporting`.
- The default schema is `ops`.
- Do not introduce external refresh, cache, semantic model, or imported-extract dependencies.
- Do not make UI grid state the source of truth.
- Reports must query saved SQL data at generation time.
- Report history must be saved to `ops.ReportRun` and `ops.ReportRunItem`.
- SO and FO are business identifiers only; never use them as primary keys.
- Use internal identity keys such as `work_order_id`, `vehicle_id`, and `issue_id` for relationships.

## Default SQL connection

```text
Driver={ODBC Driver 17 for SQL Server};Server=PC2075\SQLEXPRESS;Database=FleetOpsReporting;Trusted_Connection=yes;
```

The app may connect to `master` only for database bootstrap, then reconnect to `FleetOpsReporting`.

## Python file header requirement

Every changed `.py` file must keep a title/version/change summary header at the top using this style:

```python
# Fleet Operations Reporting App
# Version: 0.100
# Change Summary:
#   - Initial optimized SQL-first rebuild
# Planned Updates:
#   - Expand editable dialogs
```

Update the change summary whenever functionality changes.

## Validation commands

Run before and after edits when practical:

```powershell
python -m compileall src run_app.py tests
python -m unittest discover -s tests
```

## Preferred implementation style

- Keep dependencies minimal.
- Use `tkinter` for v0.100 UI unless explicitly asked otherwise.
- Use `pyodbc` for SQL Server.
- Use `reportlab` for PDFs.
- Favor small, explicit SQL queries.
- Keep SQL object names centralized in config or helper methods where practical.
- Fail gracefully when SQL is unavailable: show a readable setup/status message instead of crashing.
