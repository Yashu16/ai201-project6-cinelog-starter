# PR Response Doc — CineLog Watchlist Feature

## AI Usage


## Comment 1 — Rename
**What I did:** I changed save_to_watchlist to add_to_watchlist
**How I verified:** I have done gloabal search for save_to_watchlist and renamed them. And verified it by doing another global search to identify there's no save_to_watchlist. 

## Comment 2 — Deduplication
**What I did:** I have added code similar to that of collection_service in watchlist_service that gives an error if a user adds a film that was already added. 
**How I verified:** Manually verified with a script exercising `add_to_watchlist` twice with the same `user_id/film_id`: the first call created the entry successfully, the second call raised `AlreadyInWatchlistError` as expected, confirming no duplicate WatchlistEntry rows are created.

## Comment 3 — Missing test
**What I did:** Created `tests/test_watchlist.py`, following the fixture and assertion structure from `tests/test_collection.py`. Added `test_add_to_watchlist_nonexistent_film_raises`, the equivalent of `test_add_to_collection_nonexistent_film_raises`, which asserts that calling `add_to_watchlist` with a film_id that doesn't exist raises `FilmNotFoundError`.
**How I verified:** Ran `pytest tests/test_watchlist.py -v` and confirmed the test passes.

## Comment 4 — Default visibility
**My position:**
**Reasoning:**
**Tradeoff acknowledged:**

## Comment 5 — Sort order
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->