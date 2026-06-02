# Fleet Operations Reporting App

This repository is a clean SQL-first rebuild for local fleet operations reporting. It is not a continuation of prior table names, database names, refresh macros, imported extracts, or cache patterns.

The application is designed around direct user entry into a local SQL Server database and report generation from saved SQL data.

```text
User enters / maintains data in Python
        ↓
Python validates and writes to SQL Server
        ↓
The app reads from SQL Server views/tables
        ↓
Reports are generated from saved SQL data
        ↓
Report history and report row snapshots are written back to SQL
```

## Default local SQL target

```text
Database: FleetOpsReporting
Schema:   ops
Server:   PC2075\SQLEXPRESS
Auth:     Trusted Windows connection
```

Default connection string:

```text
Driver={ODBC Driver 17 for SQL Server};Server=PC2075\SQLEXPRESS;Database=FleetOpsReporting;Trusted_Connection=yes;
```

The bootstrap routine connects to `master` first to create `FleetOpsReporting` when needed, then creates the `ops` schema, tables, indexes, and views.

## What is included in v0.100

- Database setup script: `sql/create_fleet_ops_reporting.sql`
- Python schema bootstrap routine: `src/fleet_ops_app/fleet_schema.py`
- Desktop app shell with tabs:
  - Dashboard
  - Master Data
  - Work Orders
  - Issues
  - Reports
- Normalized SQL model:
  - `ops.Customer`
  - `ops.Fmc`
  - `ops.Installer`
  - `ops.Vehicle`
  - `ops.WorkOrder`
  - `ops.Issue`
  - `ops.IssueItem`
  - `ops.Note`
  - `ops.ReportRun`
  - `ops.ReportRunItem`
  - `ops.AppSetting`
- Initial PDF report builders:
  - Customer Supplied Items Request
  - Vehicles Without Confirmed Purchase Orders
- Validation tests for business identifier cleanup and issue type validation

## Setup

Install Python dependencies:

```powershell
python -m pip install -r requirements.txt
```

Create/update SQL objects manually:

```powershell
sqlcmd -S PC2075\SQLEXPRESS -E -i sql\create_fleet_ops_reporting.sql
```

Or let the app attempt schema bootstrap at startup:

```powershell
python run_app.py
```

## Optional environment override

```powershell
$env:FLEETOPS_SQL_SERVER="PC2075\SQLEXPRESS"
$env:FLEETOPS_SQL_DATABASE="FleetOpsReporting"
$env:FLEETOPS_SQL_SCHEMA="ops"
$env:FLEETOPS_SQL_DRIVER="ODBC Driver 17 for SQL Server"
```

A full connection string override is also supported:

```powershell
$env:FLEETOPS_SQL_CONNECTION_STRING="Driver={ODBC Driver 17 for SQL Server};Server=PC2075\SQLEXPRESS;Database=FleetOpsReporting;Trusted_Connection=yes;"
```

## Design rules

- SQL is the system of record.
- UI grids are temporary display state only.
- Reports query SQL views/tables at generation time.
- Report runs and row snapshots are saved back to SQL.
- SO and FO are business identifiers only, not primary keys.
- Internal identity keys drive relationships.
- No external refresh, cache, semantic model, or imported extract is required in v0.100.

## Validation

```powershell
python -m compileall src run_app.py tests
python -m unittest discover -s tests
```

## Next implementation targets

- Add full edit dialogs for all existing records.
- Add note history viewers.
- Add report preview screens before PDF creation.
- Add role/user tracking.
- Add controlled import/export utilities only after core data-entry workflow is stable.
