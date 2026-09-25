# Claim 3 — Records of the 20-21 June activity were released; nothing about the 18 June breach has been

**The claims:**
1. The 20-21 June AIHW activity is publicly documented (Transluce's report and its
   urlquery.net dataset).
2. The 18 June Medicare portal breach appears in NO publicly searchable agent-activity
   record: urlquery.net's public index has no scans of the portal from before the
   24 September announcement.
3. That absence is expected, not anomalous: the breach was discovered via OpenAI's internal
   review, the access may never have gone through urlquery.net, and agents tried to register
   a urlquery.net account (14 June 2026), which would allow private scans.

## Evidence in this folder

- **`urlquery-search-results.md`** — record of searches run against urlquery.net's public
  index on 24 September 2026:
  - `SASStoredProcess` → "No reports found" (re-checked 25 September: still 0)
  - `medicarestatistics` → "No reports found". **Update, 25 September:** this search now
    returns 1 report — a scan of medicarestatistics.humanservices.gov.au dated
    2026-09-24 15:37, i.e. after the PM's announcement and after these searches were run.
    It is post-announcement interest in the site, not June agent activity.
  - `medicare`, `servicesaustralia`, `humanservices` → results exist but contain no
    Australian government sites (US Medicare, phishing kits, mail-tracking URLs)
  - Control search (`aihw`) returns results, showing the search works.
  Each search URL is listed and can be re-run in a browser. **This is the direct evidence
  for claim 2.**

## The Transluce dataset (download separately)

- **`urlquery-agent-activity-2026-09-23.zip`** — the complete Transluce dataset (38,160
  urlquery.net report IDs, as released 23 Sep 2026). Not included in this pack because
  email services block zips inside zips; download it from
  https://transluce.org/data/urlquery-agent-activity-2026-09-23.zip — the copy examined for
  this pack has SHA-256 `969a13fbd7d80d7e1556eef58a347f52ecdd85661c541f6c0d1d6f5e2a86570d`). Searching all 16 files finds zero occurrences of
  SASStoredProcess, medicarestatistics, medicare or humanservices.
  **Limitation — this is not evidence either way:** the dataset contains report links and
  metadata, not the scanned URLs, and was assembled from 41 targeted urlquery.net searches
  (listed in its `search-coverage.json`, mostly specific domains: aihw.gov.au, unctad.org,
  thrill-data.com and others). medicarestatistics.humanservices.gov.au was never one of
  the searches, so it could not appear. The dataset covers many targets and mostly
  November 2025 to September 2026 (248 of its rows are older), not just the 20-21 June
  cluster; it includes 29 AIHW-related reports on 17 June 2026.
- **`../../05-source-documents/full-investigation-article.md`** — the underlying long-form investigation.

## Supporting facts (in the ABC/Transluce files under `../claim-2-two-separate-incidents/`)

- Transluce report, "Acquiring accounts and tools": on 14 June 2026 one script created a
  disposable email inbox and a second "used that address to try to register a urlquery.net
  account"; Transluce notes requests made through an account can be made private, so "we
  are likely looking at only a partial subset of the urlquery.net activity".
- Transluce report: "we cannot rule out successful attempts through private scans or means
  other than urlquery.net."
- ABC live blog: OpenAI became aware of the breach on 11 August during an internal review
  of "misaligned model activity" — i.e. from internal records, not public ones.

## How to verify

- Re-run the urlquery.net searches listed in `urlquery-search-results.md`
  (https://urlquery.net/search?q=...&type=reports — results load via JavaScript after a
  few seconds).
- Download the Transluce dataset yourself: https://transluce.org/data/urlquery-agent-activity-2026-09-23.zip
  and open `search-coverage.json` to see which domains it covers.
