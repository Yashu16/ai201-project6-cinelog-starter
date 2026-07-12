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
**My position:** Keeping `public=True` as the default.
**Reasoning:** The whole purpose of CineLog app is to increase engagement in the film community. And that can only happen when users have their watchlist public, where other users can see the list and talk with them if they have similar interests. A watchlist is part of that shared, social surface, similar to how collections are inherently visible through features like browsing. Defaulting to public means the feature is useful out of the box: friends can see what you're planning to watch without every user having to discover and flip a visibility setting first. Optimizing for a private-by-default watchlist would mean most users' lists stay invisible, undermining the discovery/social behavior the app is designed around.
**Tradeoff acknowledged:** The cost is a privacy surprise: a user might add a film expecting it to be a private "to-do" list and not realize it's visible to others until told otherwise. This is mitigated by the fact that public is a per-entry field the user can control, but it does mean the safe default (private) is not the one we ship — we're prioritizing feature usefulness/social engagement over privacy-by-default here, which is worth flagging explicitly rather than leaving as an implicit inherited default.

## Comment 5 — Sort order
**My position:** Sort by `date_added` descending (newest first), matching your preference.
**Reasoning:** I agree with your reasoning — a watchlist is inherently a "what do I want to watch next" list, and recency is the signal users care about most: what did I just add, what am I excited about right now. Alphabetical sort is more useful for browsing a large, static catalog (like the films list), not for a personal, growing list a user is actively curating. This also makes the watchlist consistent with get_collection, which already sorts by date_added descending — so the two features behave the same way from a user's perspective.
**Engagement with reviewer's point:** The one case where alphabetical could matter is if a user's watchlist grows very large and they want to find one specific film quickly — but that's better solved with search/filtering than with the default sort order, and it doesn't outweigh the recency-first behavior most users expect when they open their watchlist. Going with `date_added desc` as the default, no changes needed to your reasoning.

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->