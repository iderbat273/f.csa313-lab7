# BUG-03: API creates a book from an empty JSON object

- **Severity:** Major
- **Type:** API / Data validation
- **Status:** Reproduced
- **Evidence:** `evidence/retest-api-empty-object.txt`

## Preconditions
Restart the app and verify that GET /api/books returns the 3 seed books.

## Steps to reproduce
1. Send a POST request to `/api/books`.
2. Set `Content-Type: application/json`.
3. Send the JSON body `{}`.

## Actual result
The API returns HTTP 201 Created with `{"result":"OK","id":4}`. A new record with an empty title and author appears in GET /api/books.

## Expected result
The API should reject a book without required fields and return an appropriate client-error status. No record should be created.
