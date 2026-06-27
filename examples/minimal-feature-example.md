# Minimal Feature Example: CSV Export

## Status

- Owner: Product engineering
- Date: 2026-06-27
- Status: Example

## Goal

Allow an admin user to export the currently filtered customer table as a CSV file.

## Users and Workflow

An admin filters the customer table, clicks "Export CSV", and receives a CSV containing the same rows and columns visible in the table.

## Required Behavior

- The export uses the current table filters.
- The export includes only visible columns.
- The CSV includes a header row.
- The filename includes the date: `customers-YYYY-MM-DD.csv`.

## Edge Cases

- If no rows match the filter, the CSV contains only the header row.
- If a field contains a comma, quote, or newline, it is escaped according to CSV rules.
- If the user lacks admin permissions, the export action is unavailable.

## Non-Goals

- Scheduled exports.
- Exporting hidden columns.
- Exporting more than the current filtered customer set.

## Verification

- Unit test CSV escaping.
- Integration test filtered export.
- Permission test for non-admin users.
- Manual check that the downloaded filename contains the current date.
