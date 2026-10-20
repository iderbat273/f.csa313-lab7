# BUG-01: UI accepts an empty book form

- **Severity:** Major
- **Type:** UI / Input validation
- **Status:** Reproduced
- **Evidence:** `screenshots/add-empty-fields.png`, `evidence/retest-ui-empty-fields.txt`

## Preconditions
Restart the app and verify that GET /api/books returns the 3 seed books.

## Steps to reproduce
1. Open the reading list in the browser.
2. Leave title, author, and year empty.
3. Click the add button.

## Actual result
The app displays "Ном нэмэгдлээ" and adds a record with empty title and author and an empty/zero year representation.

## Expected result
The form should reject missing required fields and explain what needs to be entered. No record should be created.
