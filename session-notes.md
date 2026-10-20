# Exploratory Testing Session Notes

## Session metadata
- Charter: charter.md
- Planned duration: 60 minutes uninterrupted
- Actual start time: 2026-10-20 23:03:39 +08
- Actual end time: 2026-10-21 00:05:21 +08

## Pre-session observation (not counted as session time)
- Time observed: 2026-10-20, approximately 23:01 Mongolia time
- GET /api/books returned 3 books.
- GET /api/stats returned {"total":4,"newest":2008}.
- Repeated both requests; mismatch persisted.
- Evidence: evidence/initial-api-count-mismatch.txt
- Status: confirmed discrepancy before formal session; recheck during the official session.

## 2026-10-20 23:23:32 +08 — Couch Potato: empty fields
- Input: title, author, and year were all left blank.
- Actual result: the app displayed 'Ном нэмэгдлээ'.
- A record with an empty title, empty author, and year 0 appeared in the list.
- Displayed total increased from 5 to 6; newest year remained 2024.
- Potential bug: the form accepts completely empty book data.
- Screenshot: not captured for this test.

## 2026-10-20 23:29:03 +08 — Search: restore all
- Action: clicked 'Бүгд' after searching for not-a-real-book-xyz.
- Actual result: all 5 visible records returned, including the empty record.
- Displayed total remained 6; newest year remained 2024.

## 2026-10-20 23:30:48 +08 — Delete: Test Book Alpha
- Action: clicked the delete control for Test Book Alpha.
- Actual result: Test Book Alpha disappeared from the list.
- Displayed total changed from 6 to 5; newest year changed from 2024 to 2008.
- Status message still displayed 'Ном нэмэгдлээ' after deletion.
- Potential bug: deletion displays a misleading success message.

## 2026-10-20 23:31:06 +08 — API books and stats after deletion
- GET /api/books returned 4 records: the original 3 books and the empty record.
- GET /api/stats returned {"total":5,"newest":2008}.
- The reported total was one higher than the number of returned book records.
- Evidence: evidence/api-count-after-delete.txt

## 2026-10-20 23:33:31 +08 — Check state after malformed JSON
- GET /api/books still returned 4 records.
- GET /api/stats returned {"total":5,"newest":2008}.
- No visible data change was observed after the malformed JSON request.
- Evidence: evidence/api-state-after-malformed-json.txt

## 2026-10-20 23:34:55 +08 — Verify empty JSON object side effect
- GET /api/books returned 5 records after POST {}.
- The new record had id 6, empty title, and empty author; year was absent.
- GET /api/stats returned {"total":6,"newest":2008}.
- This confirms that POST {} created a record without required book fields.
- Evidence: evidence/api-state-after-empty-object.txt

## 2026-10-20 23:37:50 +08 — Retest: API accepts empty object
- Precondition: restarted the app; GET /api/books initially returned the 3 seed books.
- Request: POST /api/books with JSON body {}.
- Actual response: HTTP 201 Created, {"result":"OK","id":4}.
- GET /api/books then returned 4 records; the new record had an empty title and author.
- GET /api/stats returned {"total":5,"newest":2008}.
- Confirmed bug: API accepts a book without required fields and creates an invalid record.
- Evidence: evidence/retest-api-empty-object.txt

## 2026-10-20 23:47:47 +08 — Retest: API statistics mismatch
- GET /api/books returned 5 records: 3 seed books and 2 empty records.
- GET /api/stats returned {"total":6,"newest":2008}.
- The reported total was one greater than the actual number of records returned.
- The browser displayed the same two empty records and a total of 6.
- Confirmed bug: statistics total does not match the number of books returned by the API.
- Evidence: evidence/retest-api-stats-mismatch.txt

## 2026-10-20 23:50:12 +08 — Retest: delete status message
- Precondition: restarted the app; GET /api/books returned the 3 seed books.
- Action: deleted Clean Code in the browser.
- Actual result: Clean Code disappeared; 2 books remained visible.
- The status message still displayed 'Ном нэмэгдлээ' after deletion.
- Confirmed UI bug: deletion leaves a misleading add-success message.
- The displayed total was 3 while 2 books remained, consistent with the statistics mismatch.

## 2026-10-20 23:57:46 +08 — Retest: UI accepts empty fields
- Precondition: restarted the app; the API initially returned the 3 seed books.
- Action: submitted the UI form with all fields empty.
- Actual result: an empty book was added and the UI displayed 'Ном нэмэгдлээ'.
- GET /api/books returned 4 records, including the empty record.
- GET /api/stats returned {"total":5,"newest":2008}.
- Confirmed UI validation bug and observed the statistics count mismatch again.
- Evidence: evidence/retest-ui-empty-fields.txt
