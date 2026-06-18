# Playwright is mandatory for liveness verification

To confirm a Posting is still active, the system navigates the URL with Playwright and
reads a real DOM snapshot — never WebSearch/WebFetch. A page showing only footer/navbar
is closed; title + description + Apply button is active. Expired signals win over generic
"Apply" text (`liveness-core.mjs`).

We made browser verification mandatory because search/fetch results are stale and
cached: they routinely report dead Postings as live, which corrupts the Tracker and
wastes the user's applications. A rendered snapshot is ground truth. The trade-off is
cost and environment: Playwright is slow and needs a browser, so it's unavailable in
headless batch workers — those fall back to WebFetch and **must** stamp the Report
`Verification: unconfirmed (batch mode)` so the user can re-verify. This deliberately
rejects the faster, cheaper search path in exchange for correctness.
