# A public R scraper for the portal's guest endpoint, published 27 March 2025

**Repo:** https://github.com/bfiripis/Web-Scrape-Medicare-Item-Statistics (GitHub user `bfiripis`)
**Brought to attention by:** @foilmanhacks, 24 Sep 2026, 16:04 UTC:
https://x.com/foilmanhacks/status/2103153675183284413 (`foilmanhacks-tweet.json` + the first
tweet image)
**Checked:** 25 September 2026

## What it is

It is an R script (httr/rvest) that downloads Medicare (MBS) and PBS item statistics,
broken down by month and state, from medicarestatistics.humanservices.gov.au. It calls
the portal's **post-migration execution tier** directly with plain HTTP GET requests:

```r
response <- GET(
  "https://medicarestatistics.humanservices.gov.au/SASStoredProcess/guest",
  query = list(
    "_PROGRAM" = "SBIP://METASERVER/Shared Data/sasdata/prod/VEA0032/SAS.StoredProcess/statistics/mbs_item_standard_report",
    "DRILL" = "ag", "group" = paste(item_group, collapse = ","),
    "VAR" = "services", "STAT" = "count", "RPT_FMT" = "by time period and state",
    "PTYPE" = "month", "START_DT" = "199307", "END_DT" = "202502"))
```

It sends no credentials, has no login step and uses no POST. The script asks for 30 items
per request, reads the first HTML table in the response, and writes a CSV.

## Dates (server-side records)

| Record | Time (UTC) | Canberra (AEDT, UTC+11) |
|---|---|---|
| GitHub repo created (`github-repo-api.json`, `created_at`) | 27 Mar 2025 07:25:32 | 18:25 |
| First script commit `8eddb6c` — already calls `/SASStoredProcess/guest` | 27 Mar 2025 07:38 | 18:38 |
| LinkedIn post by the author (post ID withheld, see below) | 27 Mar 2025 07:42:58 | 18:42 |
| Batch re-download helper added (`658f078`) | 2 Apr 2025 06:29 | 17:29 |
| Last push (`pushed_at`) | 10 Apr 2025 23:47:56 | 11 Apr 09:47 (AEST) |

On 27 March 2025 the Wayback Machine also first archived the new platform's
`SetupEnvironment.js`. This was the week of the migration, so the script was written
against the new platform within days of it going live. The LinkedIn time is decoded from
the post's ID (LinkedIn IDs encode their creation time). The post itself cannot be viewed
without logging in, and the Wayback Machine has no capture of it.

## What it adds to claim 4

- **It is working-client evidence for the step no archive shows.** The pack's weakest
  point is that no archive captures a report returned through `/SASStoredProcess/guest`
  after March 2025. This code was written to do exactly that and then parse the result.
  - It expects an 11-column table (`Item, Month, NSW … NT, Total`).
  - Its notes record operating experience: "slow response time from SAS backend", "the
    server doesn't respond in time and misses one or two batches", and "30 is max allowed
    by the server".
  - On 2 April the author added a helper to re-download missed batches.

  All of this indicates the author ran the script against the live endpoint and got
  report tables back.
- **It resolves the cookie question.** httr follows redirects and keeps a cookie jar per
  host by default. So an ordinary GET client completes the server-issued
  `direct_authentication_ticket` redirect chain that loops in Wayback replay and stops at
  the login page for Common Crawl's crawler.
- **The internal path was public outside the site.** The internal path
  `SBIP://METASERVER/Shared Data/sasdata/prod/VEA0032/…` was published on GitHub (and
  apparently LinkedIn) from 27 March 2025. That is independent of the site's own
  JavaScript.
- **The parameters match the portal's own public form.** The Wayback Machine holds
  guest-endpoint captures with the same parameter set (`DRILL=ag`,
  `RPT_FMT=by time period and state`, `END_DT=202502`), e.g. `20251119125917`.

## Limits

- **No output is committed.** The repo holds the scripts and their input lists (public
  MBS/PBS schedule files), but no downloaded results. That reports came back is a strong
  inference from the code and notes, not a captured response.
- **Public data only.** The script retrieves the same public item statistics the
  website's form displays. It shows nothing about non-public reports, internal file names
  or file writes, which are the parts of the 18 June incident the government objects to.
  @foilmanhacks's "It fits the bill exactly" overstates it: it fits the access route, not
  the non-public access.
- **The LinkedIn post is unverified.** @foilmanhacks says it "has been subsequently taken
  down". This could not be checked without a LinkedIn login.
- **The script has since changed.** The repo was last changed on 11 April 2025, when the
  original `script` file was deleted and replaced by `Scrape_MBS_Data` / `Scrape_PBS_Data`
  (10 April). The 27 March version is preserved here as
  `first-script-commit-8eddb6c-2025-03-27.R.txt`.

## A note on the author

The repo is an ordinary, openly published research tool. It uses the portal's own public
form parameters, limits itself to 30 items per request as the server requires, and pauses
between batches "to be nice to the server". There is no suggestion the author did
anything improper. It is included to show how the public interface worked, not to
implicate anyone.

For the author's privacy, this pack withholds their personal name. It omits the second
tweet image (a LinkedIn search result showing the name) and its links in
`foilmanhacks-tweet.json`, the LinkedIn post ID, and the display name on three commits in
`git-log.txt`. The commit hashes, dates and messages are unchanged, and the GitHub
evidence stands on its own.

## Files in this folder

| File | What it is |
|---|---|
| `first-script-commit-8eddb6c-2025-03-27.R.txt` | The original script as committed at 07:38 UTC, 27 Mar 2025 |
| `Scrape_MBS_Data.R.txt`, `Scrape_PBS_Data.R.txt` | The current scripts (10 Apr 2025) |
| `Redownload_single_batch_MBS.R.txt` | Helper for re-fetching batches the server dropped (adds a browser User-Agent) |
| `repo-README.md` | The repo's README, including the operational notes quoted above |
| `git-log.txt` | Full commit history (hash, date, author, message; one personal name withheld) |
| `github-repo-api.json` | GitHub API record for the repo (server-side `created_at`, `pushed_at`) |
| `foilmanhacks-tweet.json` | The tweet that surfaced the repo (text, time, media URLs; image 2 links removed) |
| `foilmanhacks-tweet-img1-github-screenshot.jpg` | Tweet image 1: GitHub screenshot of `Scrape_MBS_Data` |

## How to verify

- Open the repo: https://github.com/bfiripis/Web-Scrape-Medicare-Item-Statistics
- Creation time, from GitHub's server:
  `https://api.github.com/repos/bfiripis/Web-Scrape-Medicare-Item-Statistics` → `created_at`
- The first script:
  https://github.com/bfiripis/Web-Scrape-Medicare-Item-Statistics/blob/8eddb6c4a9bf479ad9ef1e43f954da05fcd9899c/script
- Full history: `git clone` the repo and run `git log --reverse`. The hashes should match
  `git-log.txt`.
