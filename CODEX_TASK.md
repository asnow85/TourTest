# Codex Task: Continue Fleet Operations Reporting App v0.100

You are working in a clean SQL-first Python desktop app rebuild.

## Goal

Improve the starter app without reintroducing any external refresh, cache, semantic model, or imported-extract dependency. The app must remain a direct-entry operational reporting system backed by `FleetOpsReporting.ops` SQL objects.

## Immediate implementation target

Complete the v0.100 workflow foundation:

1. Verify the schema bootstrap routine can create:
   - database `FleetOpsReporting`
   - schema `ops`
   - all core tables, indexes, and views
2. Improve the tkinter UI while keeping it simple:
   - Dashboard counts from SQL views/tables
   - Master data add/edit/deactivate records
   - Work order add/edit/search
   - Issue add/edit/close with notes and supplied item rows
   - Reports generated from SQL only
3. Make report generation persist:
   - one row in `ops.ReportRun`
   - one row per included work order/issue/item in `ops.ReportRunItem`
   - JSON snapshot of each report row
4. Add useful tests around validation and snapshot serialization.
5. Update README with any new setup or usage steps.

## Hard constraints

- Default database must remain `FleetOpsReporting`.
- Default schema must remain `ops`.
- Do not use previous project table names, database names, or prefixes.
- Do not use `dbo` as the default schema for app-owned objects.
- Do not use SO or FO as a primary key.
- Do not assume SO is unique.
- Do not add spreadsheet macro refreshes.
- Do not add a local cache database.
- Do not add external semantic model dependencies.
- Reports must read from saved SQL data, not unsaved grid state.

## Required validation

Run:

```powershell
python -m compileall src run_app.py tests
python -m unittest discover -s tests
```

## Deliverable summary

When finished, summarize:

- Files changed
- Schema changes
- UI changes
- Report behavior changes
- Validation commands and results
- Any remaining assumptions
