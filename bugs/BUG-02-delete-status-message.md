# BUG-02: Delete action leaves an add-success message

- **Severity:** Minor
- **Type:** UI / Status message
- **Status:** Reproduced
- **Evidence:** `screenshots/delete-message.png`

## Preconditions
Restart the app and verify that GET /api/books returns the 3 seed books.

## Steps to reproduce
1. Open the reading list in the browser.
2. Delete the book "Clean Code".
3. Observe the status message.

## Actual result
The book disappears, but the page still displays "Ном нэмэгдлээ".

## Expected result
The page should display a deletion confirmation or clear the previous success message.
