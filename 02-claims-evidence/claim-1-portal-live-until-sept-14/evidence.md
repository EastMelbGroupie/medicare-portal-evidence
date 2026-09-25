# Claim 1 — The hacked Medicare statistics portal was still online on 14 September 2026 (UTC) / 15 September (Canberra time)

**Times:** archive timestamps are UTC. Canberra time in September 2026 is AEST, UTC+10.

**The claim:** OpenAI disclosed the breach by email on 10 September 2026. The portal's web
server and SAS login service were still live and answering at 00:03-01:31 on Tuesday
15 September, Canberra time (14:03-15:31 on 14 September UTC) — five days after the email,
and on the day Services Australia notified the Australian Signals Directorate. By 13:10 on
Thursday 17 September, Canberra time, the site was serving a branded "Service Unavailable"
(HTTP 503) page. The take-down therefore happened between 01:31 on 15 September and 13:10
on 17 September. The minister was told on 17 September; whether the take-down came before
or after that is not known.

**Why it matters:** the government's disclosure-timeline defence ("we acted as soon as we
were told") is not in question, but the window during which the portal remained exposed
is now known to be nearly 13 weeks after the 18 June breach — not the eight weeks
previously established — and included several days after the vendor's own disclosure.

## Evidence in this folder

Raw Common Crawl index dumps for the domain `medicarestatistics.humanservices.gov.au`,
one file per crawl, April-September 2026, plus Wayback Machine CDX (capture index)
listings. Common Crawl is an independent, non-profit web archive; anyone can re-run these
queries. The Common Crawl dumps were re-fetched on 25 September 2026 and match the live
index exactly.

**The key file: `CC-MAIN-2026-39-medicarestatistics.jsonl`** (crawled 14 September UTC):

- `20260914153142 200 https://medicarestatistics.humanservices.gov.au/` — homepage
  (a redirect to the report page)
- `20260914142334 200 https://medicarestatistics.humanservices.gov.au/statistics/mbs_item.jsp`
  — a redirect stub pointing to the report form page `mbs_item.html`
- `SASStoredProcess/guest?_PROGRAM=SBIP://METASERVER/...` → 302 to
  `SASLogon/login?...&direct_authentication_ticket=ST-...` (a fresh, server-issued ticket)
  → 302 to `SASLogon/login?service=...` → 200: the SAS Logon Manager page, freshly
  generated. The SAS application tier was running.

**`cc-2026-09-14-responses/`** — the actual response bodies of those three HTTP 200
captures, pulled from Common Crawl's public WARC files (see the README there). They show
the server and SAS application live and generating fresh login tickets. They do **not**
include the report form page itself (`mbs_item.html`), which was not in this crawl; its
last archived capture is 11 August 2026 (Wayback Machine, below).

**`wayback-cdx-mbs_item-2026.txt`** — Wayback Machine capture index for the report pages
in 2026 (dates UTC): `mbs_item.html` (the form page itself) captured with HTTP 200 on
18 April, 18 May, 26 June and 11 August 2026; the `mbs_item.jsp` redirect stub with HTTP 200 on
16 June (07:04 on 17 June, Canberra time — the day before the breach) and through August;
HTTP 503 from 18 September (the whole-domain index below has an earlier 503, on
17 September).

**`wayback-cdx-domain-sep-2026.txt`** — Wayback capture index for the whole domain in
September 2026. The first entry, `20260917031054`, is a 503 (the branded "Service
Unavailable" page, "The Medicare Australia Statistical Reporting suite, also known as
WebStats, is temporarily unavailable") at 13:10 on 17 September, Canberra time.

**`CC-MAIN-2026-25-FAILED-QUERY-504.html`** — the query for the June 2026 crawl
(CC-MAIN-2026-25) returned a "504 Gateway Time-out" from the Common Crawl index server on
24 and 25 September. No June Common Crawl data is included.

Combined timeline (Canberra time; UTC in brackets):

| Source | Canberra time (UTC) | What was captured |
|---|---|---|
| Common Crawl CC-MAIN-2026-17 | 15 Apr 2026 (14 Apr UTC) | 200 — site answering; PBS item form page served |
| Wayback Machine | 19 Apr 2026 (18 Apr UTC) | 200 — report form page `mbs_item.html` |
| Wayback Machine | 19 May 2026 (18 May UTC) | 200 — report form page |
| Common Crawl CC-MAIN-2026-21 | 22 May 2026 (21 May UTC) | 200 — site answering |
| Wayback Machine | 17 Jun 2026, 07:04 (16 Jun UTC) | **200 — site answering the day before the 18 June breach** (redirect stub) |
| Wayback Machine | 26 Jun 2026 | 200 — report form page, 8 days after breach |
| Common Crawl CC-MAIN-2026-30 | 19 Jul 2026 | 200 — site answering; SAS login service live |
| Wayback Machine | 11 Aug 2026 | 200 — report form page, ~8 weeks after breach (the day OpenAI became aware) |
| Common Crawl CC-MAIN-2026-34 | 18 Aug 2026 (17 Aug UTC) | 200 — site answering; SAS login service live |
| Common Crawl CC-MAIN-2026-39 | **15 Sep 2026, 00:03-01:31 (14 Sep UTC)** | **200 — site answering; SAS login service live, ~12.7 weeks after breach, 5 days after disclosure** |
| Wayback Machine | **17 Sep 2026, 13:10 (03:10 UTC)** | **503 — "Service Unavailable"** |

## How to verify (2 minutes, no special access)

Open in a browser:

```
https://index.commoncrawl.org/CC-MAIN-2026-39-index?url=medicarestatistics.humanservices.gov.au&matchType=domain&output=json
```

Look at the `timestamp` field (20260914..., UTC) and `status` field (200) in each row.
The Common Crawl index server is intermittently overloaded (502/504); retry if needed.
To see a response body, use the row's `filename`, `offset` and `length`:
`curl -r <offset>-<offset+length-1> https://data.commoncrawl.org/<filename> | gunzip`.

For the take-down:

```
https://web.archive.org/cdx/search/cdx?url=medicarestatistics.humanservices.gov.au&matchType=domain&from=20260901
```

The first row, `20260917031054`, has status 503.

For the disclosure dates (10 September email; 15 September ASD notification; 17 September
minister told), see the ABC live blog saved in
`../claim-2-two-separate-incidents/abc-live-blog-2026-09-24.html` ("Timeline of breach" entry).

For the site being offline now: `https://medicarestatistics.humanservices.gov.au/`
returns HTTP 503 on every path, including robots.txt (verified 24 and 25 September 2026).
