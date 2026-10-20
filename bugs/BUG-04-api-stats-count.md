# BUG-04: Statistics total does not match the returned book count

- **Severity:** Major
- **Type:** API / Data consistency
- **Status:** Observed; verify from a clean state
- **Evidence:** `evidence/initial-api-count-mismatch.txt`, `evidence/retest-api-stats-mismatch.txt`

## Steps to reproduce
1. Request `GET /api/books`.
2. Count the records in the JSON response.
3. Request `GET /api/stats`.
4. Compare the returned `total` with the number of records.

## Actual result
The saved observations show cases where `total` is one greater than the number of records returned by `/api/books`.

## Expected result
The `total` value should equal the number of books returned by `/api/books` for the same application state.

## Note
The later retest included empty records created during exploratory testing. Confirm the mismatch again immediately after restarting the app before treating the clean-state behavior as fully verified.
